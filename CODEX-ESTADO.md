# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 17:23** · publicado em **06/11/2026 17:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 17:23 · sessao 89a375b18f4a · O10-C · censo · origem=Code

O O10-C atual **não deposita SOLO numa lista persistida do admin**: classifica e imprime a linha; a lista `!` também é saída de terminal. A aplicação fica restrita a `trocaria`, obtida com par declarado. (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:90`, `:233`, `:237`, `:247`, `:254`)

**1) As listas e superfícies administrativas encontradas são estas:**

| Lista / entidade | Quem escreve a linha | Quem lê | Link ou ação existente |
|---|---|---|---|
| **Ausências — `Ausencia`**, com colaborador e data/período. (`app/ponto/models.py:743`, `:750`) | `ponto.services.ausencia.criar_ausencia`; o lançamento administrativo chama essa função. (`app/ponto/services/ausencia.py:130`, `:385`; `app/ponto/views.py:2415`) | `lista_ausencias` → `ponto/ausencias.html`, com filtros por colaborador, tipo e status. (`app/ponto/views.py:2155`, `:2183`, `:2202`, `:2244`) | Linha abre painel; há documento, aprovação/cancelamento e upload individual. Esses controles não recebem colega nem duas cartas. (`app/templates/ponto/ausencias.html:113`, `:142`, `:167`; `app/templates/ponto/partials/painel_ausencia.html:90`, `:134`) |
| **Central de Justificativas — `Justificativa`**, por colaborador/data do turno. (`app/ponto/models.py:136`, `:144`) | Há vários escritores: `_abrir_justificativa_fluxo`, API, `regularizar_turno_orfao` e `_abrir_justificativa_automatica`. (`app/ponto/views.py:1621`, `:1635`; `app/api/views.py:1997`, `:2007`; `app/ponto/services/regularizacao.py:91`; `app/ponto/management/commands/processar_alertas_turno.py:389`) | `fila_justificativas` → `ponto/fila_justificativas.html`, por padrão pendentes. (`app/ponto/views.py:1695`, `:1704`, `:1709`, `:1749`) | Abre painel da justificativa; este oferece POSTs de aprovação e rejeição. (`app/templates/ponto/fila_justificativas.html:148`; `app/templates/ponto/partials/painel_justificativa.html:51`, `:58`) |
| **Central de Chamados — `ChamadoColaborador`**, com colaborador e FK opcional de célula. (`app/chamados/models.py:32`, `:44`) | Portas de nascimento `ChamadoColaborador.abrir` — cria ou pega — e `criar`, sem deduplicação. (`app/chamados/models.py:796`, `:867`, `:1015`) | `painel_gestao` → `montar_contexto_painel` → listas resumida/detalhada. (`app/chamados/views.py:721`, `:726`, `:740`, `:767`) | Linha abre o fio; existem ações conforme o verbo, como validar e cobrar. (`app/templates/chamados/partials/_lista_chamados.html:42`, `:82`) |
| **Pendência administrativa da pergunta — `PerguntaDisputa.pendencia_admin`**, junto de `recusa_motivo`; não é uma tabela genérica de casos. (`app/chamados/models.py:2317`, `:2321`) | `_materializar_resposta_hora` grava os dois campos juntos. (`app/chamados/services/materializacao.py:759`, `:821`, `:825`) | `modal_fio_colaborador` → `chamados/partials/modal_fio.html`. (`app/chamados/views.py:869`, `:927`) | O bloco mostra aviso e motivo; a orientação de correção é textual, sem link próprio nesse bloco. (`app/templates/chamados/partials/modal_fio.html:217`; `app/chamados/services/explicador.py:85`) |
| **Pautas — `Pauta`**, destinadas a departamento/usuário; aceitam âncora de dia `<colab>:<AAAA-MM-DD>`. (`app/pautas/models.py:74`, `:83`) | Escritor canônico `pautas.services.escrever`, que cria pauta, trilha e contador. (`app/pautas/services.py:153`, `:180`, `:186`) | `pautas.views.aba` → painel `_copiloto.html`. (`app/pautas/views.py:29`, `:33`, `:61`; `app/templates/core/_copiloto.html:404`) | Há responder/feita/cancelar. A navegação por âncora atende `colab`, `chamado`, `posto`; **`dia` e `competencia` caem no aviso “ainda não tem tela própria”**. (`app/templates/core/_copiloto.html:432`, `:443`, `:452`, `:564`, `:570`) |
| **Fila humana do flip**, por batida/colaborador/dia. (`app/ponto/services/flip_auto.py:248`) | Não tem depósito próprio nesses caminhos: `avaliar_fila` deriva os itens e deduplica por ID da batida. (`app/ponto/services/flip_auto.py:266`, `:268`) | `flip_fila_view` seleciona `veredito='humano'` → `ponto/flip_fila.html`. (`app/ponto/views.py:3299`, `:3305`) | Link para espelho e POST `flip_batida`. (`app/templates/ponto/flip_fila.html:11`, `:16`) |
| **Pendências de cadastro / Furos de espelho** — agregadas por colaborador/período. (`app/relatorios/views_pend.py:21`, `:24`; `app/templates/relatorios/pendencias_espelho.html:42`) | São derivadas de `ranking_colabs` + `universo_empresa`; essas funções de montagem não depositam um caso próprio. (`app/relatorios/views_pend.py:23`; `app/relatorios/pendencias.py:120`, `:135`) | `pendencias_cadastro_view` / `pendencias_espelho_view` → templates homônimos. (`app/relatorios/views_pend.py:74`, `:84`, `:87`, `:95`) | Linhas exibem nome, contato, natureza, descrição, recomendação e dias; não têm ação/link na linha. (`app/templates/relatorios/pendencias_cadastro.html:42`; `app/templates/relatorios/pendencias_espelho.html:42`) |
| **Gestão de HE**, com pendências por colaborador/dia. (`app/ponto/views.py:884`) | `he_pendente_lavrado.lavrar` grava o retrato em `MetricaSnapshot`; decisões vigentes são lidas de `DecisaoHE`. (`app/ponto/services/he_pendente_lavrado.py:177`, `:182`; `app/ponto/services/gestao_he.py:81`) | `gestao_he` lê o retrato e renderiza `ponto/gestao_he.html`. (`app/ponto/views.py:931`, `:942`, `:951`) | A célula do dia posta para `decidir_he`; a view chama a porta de HE. (`app/templates/ponto/gestao_he.html:256`; `app/ponto/views.py:517`, `:529`) |

Sobre **“PortaFila”**: o correspondente encontrado é o valor `'porta_fila'` retornado por `porta_alcancavel_do_dia`, a partir dos chamados da célula na fila; essa função lê alcance, não cria casos. O próprio contrato exclui “Resolver dia” desse conceito específico de porta. (`app/chamados/juizes.py:1341`, `:1367`, `:1375`, `:1386`)

**2) Não localizei chamador de UI de `trocar_plantao` ou `editar_dia`.** Os chamadores produtivos encontrados são O10-C → `trocar_plantao` e, dentro desta, → `editar_dia`; o cabeçalho do comando declara ser o primeiro chamador de produção da troca. (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:4`, `:160`; `app/ponto/portas/celula.py:675`)

A tela mais próxima é **perfil do colaborador → Calendário de Ponto → dia**. O perfil carrega o calendário; a grade inclui “Resolver dia” já com colaborador e data. O drawer de diagnóstico também inclui essa grade. (`app/templates/colaboradores/detalhe.html:215`; `app/colaboradores/views.py:1935`; `app/templates/colaboradores/partials/_calendario_grade.html:125`; `app/templates/colaboradores/partials/_drawer_diagnostico.html:33`)

A cadeia existente é:

`_veredito_dia.html` → URL `celula/veredito/` → `veredito_celula_view` → serviço `veredito_celula`, com `trabalhou`, `faltou`, `justificado`. “Justificado” chama `criar_ausencia`. (`app/templates/ponto/partials/_veredito_dia.html:17`; `app/ponto/urls.py:58`; `app/ponto/views.py:3340`; `app/ponto/services/veredito_celula.py:18`, `:310`)

O que falta **na cadeia atual** para alcançar a troca dali: o formulário não coleta substituto, duas Ausências/cartas ou motivo; o handler não despacha para `trocar_plantao`. Para `editar_dia`, também não há despacho com `trabalha` e motivo. Esses argumentos constam das respectivas portas. (`app/templates/ponto/partials/_veredito_dia.html:19`; `app/ponto/views.py:3318`, `:3340`; `app/ponto/portas/celula.py:534`, `:561`, `:632`, `:646`)

Além disso, o botão não aparece automaticamente para todo SOLO: exige permissão, último recurso, dia passado e status `falta`, `irregular`, `aberto` ou `sem_certidao`. O botão “DP”, que abre pauta com colaborador/data, está sob a mesma condição da grade. (`app/colaboradores/services/calendario.py:94`, `:103`; `app/templates/colaboradores/partials/_calendario_grade.html:125`, `:126`)

**3) Os “dois anexos obrigatórios” são duas cartas no mesmo campo de modelo:**

- Carta do cedente: `ausencia_cedente.documento`.
- Carta do substituto: `ausencia_substituto.documento`.

A assinatura recebe duas `Ausencia`; o campo é `FileField`, com `upload_to="ausencias/documentos/"`, permitindo vazio no modelo. A obrigatoriedade dupla é imposta pela porta. (`app/ponto/portas/celula.py:632`, `:635`, `:664`; `app/ponto/models.py:777`)

A porta recarrega e trava os registros, recusa lado ausente/inexistente ou mesmo colaborador e testa literalmente `if not a.documento` em cada lado. Essa validação não abre o arquivo nem segue `documento_referencia`; a trilha guarda `documento.name`. (`app/ponto/portas/celula.py:653`, `:656`, `:660`, `:662`, `:664`, `:682`)

**4) Sim, existem precedentes de lote que deposita caso para o admin, sem resolver o fato por ele:**

| Precedente | Depósito e escritor | Idempotência / chave |
|---|---|---|
| **`processar_alertas_turno`** | `_abrir_justificativa_automatica` cria `Justificativa(status='pendente', tipo='esqueci_saida')`; a Central de Justificativas lê essas linhas. (`app/ponto/management/commands/processar_alertas_turno.py:389`, `:398`; `app/ponto/views.py:1704`) | `get_or_create(colaborador, data_turno)`. O banco impõe uma pendente por `(colaborador,data_turno)`, além da unicidade geral com `batida_entrada`. (`app/ponto/management/commands/processar_alertas_turno.py:391`; `app/ponto/models.py:187`, `:189`) |
| **`pautas_discordantes`** | Comando chama `sincronizar`; com aplicação, escreve pautas ao DP por `pautas.services.escrever` e fecha as que saíram do alvo. (`app/ponto/management/commands/pautas_discordantes.py:113`, `:118`; `app/ponto/services/pautas_dos_discordantes.py:173`) | Cabeça `[DISCORDANTE] <DONO> col<ID> <AAAA-MM>`, entre pautas vivas do DP, âncora competência. É agrupado por dono/colaborador/competência, contendo os dias. (`app/ponto/services/pautas_dos_discordantes.py:84`, `:143`, `:165`) |
| **`pauta_dia_em_aberto`** | Escreve pauta ao DP e filhas com detalhamento, pela porta `escrever`. (`app/pautas/management/commands/pauta_dia_em_aberto.py:139`) | Cabeça viva por âncora `empresa:AAAA-MM` e prefixo `[DIA EM ABERTO]`; completar filhas também tem guarda própria. (`app/pautas/management/commands/pauta_dia_em_aberto.py:104`, `:107`, `:117`) |

O escritor canônico de **Pauta** é `escrever`, mas ele faz `create`: a chave de idempotência é conferida por cada emissor, não deduplicada genericamente pela porta. (`app/pautas/services.py:153`, `:180`; `app/ponto/services/pautas_dos_discordantes.py:165`)

Há também precedente **por dia**: `pauta_dp_do_limbo_exportado` escreve ao DP com âncora `<colab>:<data>`. Deduplica pelo histórico inteiro — inclusive feita/cancelada — considerando âncora, destinatário DP, cabeça sem pai, prefixo e tag `[motivo:X]`. (`app/chamados/services/fabrica_da_celula.py:208`, `:219`, `:225`, `:236`)

E existe um serviço diretamente relacionado ao nome ausente: `declarar_cobertura_passada`, sem substituto, abre pauta à supervisão perguntando quem cobriu. A chave considera pauta viva, dia, destinatário supervisão e prefixo `[COBERTURA-A-CONFIRMAR]`; é serviço, não comando de lote. (`app/ponto/services/cobertura.py:238`, `:246`, `:329`, `:333`)

**5) O contraexemplo depende de “SOLO” significar ausência de `--par` ou ausência de colega no cadastro legado:**

- **Sem `--par`: não há caminho de aplicação da troca.** O caso retorna `so_um_lado` — ou outra recusa anterior, como pedido/rejeitada/sem carta — e apenas `trocaria` chama `aplicar_par`. Não passa pela escrita de células/trilhas da porta. (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:77`, `:90`, `:237`)
- **Mesmo sem par, `--aplicar` escreve arquivo:** cria diretório e abre um snapshot exclusivo antes do laço. Sem aplicação de par, esse arquivo não recebe as fotos de `aplicar_par`. (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:209`, `:216`, `:157`, `:164`)
- **Se SOLO significa o lançamento legado sem colega no cadastro, existe caminho de escrita por CLI:** `--par C:S --aplicar --por USER` fornece a segunda Ausência ao classificador e pode chegar à troca. A porta então escreve `CelulaDia` e trilhas `editar_dia`/`trocar_plantao`; o teste existente monta um lançamento legado SOLO, fornece a segunda carta por `--par` e espera duas células editadas e trilha. (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`, `:233`, `:238`, `:160`; `app/ponto/portas/celula.py:600`, `:609`, `:611`, `:686`; `app/ponto/tests/test_o10c_lote_trocas.py:54`, `:140`)
- **O comando não cria o lado do substituto:** consulta uma `Ausencia` já existente pelo ID declarado. Também não deposita o SOLO em Pauta, Justificativa ou chamado; a saída do laço é `stdout`, contadores e lista `!`. (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:188`, `:191`, `:233`, `:247`, `:254`)

Execução dos testes e conteúdo atual do banco: **[nao verificado]**. [nao verificado]

## 06/11 17:22 · sessao 24261d69a599 · - · - · origem=Ronald (tmux)

- **Pautas** aceitam âncora de colaborador, chamado, posto, **dia** (`<colab_id>:<AAAA-MM-DD>`) e competência; o destinatário pode ser departamento ou usuário. Escritor canônico: `pautas.services.escrever`, que cria a linha, registra trilha e lavra contador. (`app/pautas/models.py:30`, `app/pautas/models.py:74`, `app/pautas/models.py:83`, `app/pautas/services.py:153`, `app/pautas/services.py:180`).
- Leitor: `pautas.views.aba` chama `listar_por_aba` e devolve as linhas ao painel do copiloto. As ações existentes são marcar feita, responder e cancelar. (`app/pautas/views.py:29`, `app/pautas/views.py:33`, `app/pautas/views.py:61`, `app/templates/core/_copiloto.html:432`, `app/templates/core/_copiloto.html:443`, `app/templates/core/_copiloto.html:452`).
- **A pauta de dia não tem navegação acionável própria no copiloto**: seu texto pode incorporar URL do calendário, mas o painel o renderiza com `textContent`; as rotas clicáveis cobrem somente `colab`, `chamado` e `posto`, e `dia` cai no aviso “ainda não tem tela própria”. (`app/pautas/services.py:428`, `app/pautas/services.py:429`, `app/templates/core/_copiloto.html:404`, `app/templates/core/_copiloto.html:564`, `app/templates/core/_copiloto.html:570`).
- Existe bloco de pautas na ficha, mas ele mostra somente remetente, destinatário, estado e texto truncado em 180 caracteres. O serviço `pautas_do_colab` consulta âncoras de colaborador e seus dias; não encontrei chamador Python desse serviço além da própria definição — alimentação desse contexto **[não verificado]**. (`app/templates/colaboradores/detalhe.html:108`, `app/templates/colaboradores/detalhe.html:115`, `app/templates/colaboradores/detalhe.html:119`, `app/pautas/services.py:433`).

- **Central de chamados**: a linha persistida é `ChamadoColaborador`, com colaborador e FK opcional de `CelulaDia`; não há garantia de uma linha por dia no modelo. O painel usa `montar_contexto_painel`; a linha abre o fio e oferece ações de validar, cobrar e desarquivar conforme o verbo. (`app/chamados/models.py:32`, `app/chamados/models.py:44`, `app/chamados/models.py:101`, `app/chamados/views.py:721`, `app/chamados/views.py:727`, `app/templates/chamados/partials/_lista_chamados.html:42`, `app/templates/chamados/partials/_lista_chamados.html:82`).
- Escritores canônicos de nascimento: `ChamadoColaborador.abrir` faz cria-ou-pega com deduplicação por `colaborador + **lookup`, dentro de transação e trava na linha do colaborador; `criar` cria sem deduplicação. (`app/chamados/models.py:796`, `app/chamados/models.py:863`, `app/chamados/models.py:867`, `app/chamados/models.py:932`, `app/chamados/models.py:1015`, `app/chamados/models.py:1056`).
- **`PortaFila` não é entidade separada nas fontes encontradas**: `porta_fila` é valor do dicionário devolvido por `porta_alcancavel_do_dia`, quando há chamado da célula em `qs_na_fila`; essa função lê, não deposita casos. O conceito exclui explicitamente o botão Resolver dia do calendário. (`app/chamados/juizes.py:1341`, `app/chamados/juizes.py:1352`, `app/chamados/juizes.py:1367`, `app/chamados/juizes.py:1375`, `app/chamados/juizes.py:1386`).
- `qs_na_fila` seleciona chamados em estados de cobrança, excluindo os silenciados no futuro. (`app/chamados/services/painel.py:158`, `app/chamados/services/painel.py:186`).

- **Pendência do admin na pergunta**: é o campo `PerguntaDisputa.pendencia_admin`, acompanhado de `recusa_motivo`, e não uma tabela genérica de pendências. O escritor explícito é `_materializar_resposta_hora`, que grava ambos juntos. (`app/chamados/models.py:2312`, `app/chamados/models.py:2317`, `app/chamados/models.py:2321`, `app/chamados/services/materializacao.py:759`, `app/chamados/services/materializacao.py:821`, `app/chamados/services/materializacao.py:825`).
- O fio do admin carrega perguntas via `montar_chamados_fio`; `modal_fio_colaborador` renderiza o template que exibe o aviso e o motivo dessa pendência. Nesse bloco não há link de correção: o explicador fornece instrução textual para conferir horário e lançar correção com trilha. (`app/chamados/views.py:869`, `app/chamados/views.py:889`, `app/chamados/views.py:927`, `app/templates/chamados/partials/modal_fio.html:217`, `app/chamados/services/explicador.py:85`).

- **Precedente claro de lote que envia à lista do admin**: `pautas_discordantes` apura casos e chama `sincronizar`; com `--apply`, o serviço escreve pautas ao DP e marca feitas as que saíram do alvo. A chave é a cabeça textual `[DISCORDANTE] <DONO> col<ID> <AAAA-MM>`, agrupada por dono, colaborador e competência; a consulta de existentes considera pauta viva, âncora competência, destinatário DP e prefixo do emissor. (`app/ponto/management/commands/pautas_discordantes.py:113`, `app/ponto/management/commands/pautas_discordantes.py:118`, `app/ponto/services/pautas_dos_discordantes.py:84`, `app/ponto/services/pautas_dos_discordantes.py:93`, `app/ponto/services/pautas_dos_discordantes.py:165`, `app/ponto/services/pautas_dos_discordantes.py:173`).
- Outro comando existente, `pauta_dia_em_aberto`, escreve pautas ao DP por competência em vez de decidir os dias; deduplica uma cabeça viva por âncora `empresa:AAAA-MM` e prefixo `[DIA EM ABERTO]`. (`app/pautas/management/commands/pauta_dia_em_aberto.py:104`, `app/pautas/management/commands/pauta_dia_em_aberto.py:107`, `app/pautas/management/commands/pauta_dia_em_aberto.py:139`).
- Existe também emissão **por dia**: `pauta_dp_do_limbo_exportado` usa `escrever`, destinatário DP e âncora `<colab>:<data>`. Sua idempotência considera todo o histórico, inclusive pautas feitas ou canceladas, pela âncora de dia, destinatário, cabeça sem pai, prefixo `LIMBO EM COMPETENCIA EXPORTADA` e tag `[motivo:X]`. (`app/chamados/services/fabrica_da_celula.py:208`, `app/chamados/services/fabrica_da_celula.py:214`, `app/chamados/services/fabrica_da_celula.py:219`, `app/chamados/services/fabrica_da_celula.py:225`, `app/chamados/services/fabrica_da_celula.py:236`).
- Precedente diretamente relacionado ao nome de quem cobriu: `declarar_cobertura_passada` sem substituto abre pauta à supervisão pedindo o nome. Deduplica por pauta viva, âncora do dia, destinatário supervisão e prefixo `[COBERTURA-A-CONFIRMAR]`; trata-se de serviço, não de comando de lote. (`app/ponto/services/cobertura.py:238`, `app/ponto/services/cobertura.py:241`, `app/ponto/services/cobertura.py:246`, `app/ponto/services/cobertura.py:329`, `app/ponto/services/cobertura.py:333`).

## 06/11 17:21 · sessao 95781dacd4db · - · - · origem=Ronald (tmux)

Complemento para perguntas 1 e 4, com fontes:

| Lista existente | Quem escreve / forma da linha | Quem lê | Link/ação existente |
|---|---|---|---|
| **Ausências** | Escritor da entidade: `ponto.services.ausencia.criar_ausencia`, que recebe colaborador/tipo/data/documento/status e constrói `Ausencia`; chamador administrativo `lancar_ausencia` chama essa função. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:385`, `app/ponto/views.py:2415`). Constraint: `(colaborador,tipo,data)` para linhas não rejeitadas. (`app/ponto/models.py:827`) | `lista_ausencias` consulta por vigência e permite filtros por colaborador, tipo e status; renderiza `ponto/ausencias.html`. (`app/ponto/views.py:2155`, `app/ponto/views.py:2178`, `app/ponto/views.py:2183`, `app/ponto/views.py:2202`, `app/ponto/views.py:2244`) | A linha mostra data/colaborador/tipo e abre painel pelo ID; há documento, aprovação/cancelamento em lote, anexar documento e editar registro. (`app/templates/ponto/ausencias.html:113`, `app/templates/ponto/ausencias.html:142`, `app/templates/ponto/ausencias.html:150`, `app/templates/ponto/ausencias.html:167`, `app/templates/ponto/partials/painel_ausencia.html:90`, `app/templates/ponto/partials/painel_ausencia.html:134`) Esses formulários não coletam colega/substituto nem dois documentos: upload único `documento`, edição de tipo/data/duração/descrição. (`app/templates/ponto/partials/painel_ausencia.html:92`, `app/templates/ponto/partials/painel_ausencia.html:134`) |
| **Central de Justificativas** | Escritores encontrados: `_abrir_justificativa_fluxo` busca pendente por `(colaborador,data_turno)` e atualiza ou cria; `api_justificativas` faz o mesmo; `regularizar_turno_orfao` usa `get_or_create(colaborador,batida_entrada,data_turno)`; comando `processar_alertas_turno._abrir_justificativa_automatica` usa `get_or_create(colaborador,data_turno)`. (`app/ponto/views.py:1568`, `app/ponto/views.py:1621`, `app/ponto/views.py:1635`, `app/api/views.py:1944`, `app/api/views.py:1997`, `app/api/views.py:2007`, `app/ponto/services/regularizacao.py:19`, `app/ponto/services/regularizacao.py:91`, `app/ponto/management/commands/processar_alertas_turno.py:389`) Portanto não há escritor único entre esses caminhos. (mesmas fontes) | `fila_justificativas` lê `Justificativa`, filtra status e renderiza a central. (`app/ponto/views.py:1695`, `app/ponto/views.py:1704`, `app/ponto/views.py:1749`) | Cada linha tem colaborador/data e abre `painel_justificativa`; painel contém POSTs `aprovar_justificativa` e `rejeitar_justificativa`, que chamam serviços próprios de decisão. (`app/templates/ponto/fila_justificativas.html:98`, `app/templates/ponto/fila_justificativas.html:109`, `app/templates/ponto/fila_justificativas.html:148`, `app/templates/ponto/partials/painel_justificativa.html:51`, `app/templates/ponto/partials/painel_justificativa.html:58`, `app/ponto/views.py:1915`, `app/ponto/views.py:1927`) |
| **Pendências de cadastro / Furos de espelho** | São listas derivadas em memória: `_base` consulta `ranking_colabs` + `universo_empresa`; `pendencias_cadastro` e `pendencias_espelho` copiam/filtram os dicionários recebidos. Não há depósito persistido nessas funções. (`app/relatorios/views_pend.py:14`, `app/relatorios/views_pend.py:23`, `app/relatorios/pendencias.py:120`, `app/relatorios/pendencias.py:135`) São por colaborador/período, com contadores/dias afetados, não um caso persistido por dia. (`app/templates/relatorios/pendencias_espelho.html:24`, `app/templates/relatorios/pendencias_espelho.html:42`) | `pendencias_cadastro_view` / `pendencias_espelho_view`, templates homônimos. (`app/relatorios/views_pend.py:74`, `app/relatorios/views_pend.py:84`, `app/relatorios/views_pend.py:87`, `app/relatorios/views_pend.py:95`) | Linhas apresentam nome, telefone, natureza, descrição, recomendação e dias; nenhum link/form/ação nas linhas atuais. (`app/templates/relatorios/pendencias_cadastro.html:42`, `app/templates/relatorios/pendencias_espelho.html:42`) |
| **Fila humana do flip E/S** | Derivada de `avaliar_fila`, que recebe `fila_flip(dias)` e deduplica por ID da batida; `fila_flip` une pares de mesmo tipo e recusas de paridade, sem depósito em modelo próprio nesses caminhos. (`app/ponto/services/flip_auto.py:144`, `app/ponto/services/flip_auto.py:248`, `app/ponto/services/flip_auto.py:268`) | `flip_fila_view` filtra itens com `veredito='humano'` e renderiza `ponto/flip_fila.html`. (`app/ponto/views.py:3299`, `app/ponto/views.py:3305`) | Linha por batida/colaborador/dia, link para espelho e POST `flip_batida`; view chama `flip_tipo`. (`app/templates/ponto/flip_fila.html:7`, `app/templates/ponto/flip_fila.html:11`, `app/templates/ponto/flip_fila.html:16`, `app/ponto/views.py:3260`, `app/ponto/views.py:3266`) |

**Precedente adicional de comando que deposita caso para decisão administrativa:** `processar_alertas_turno.Command._abrir_justificativa_automatica` cria `Justificativa` pendente, tipo `esqueci_saida`, descrição automática, opcionalmente vinculada ao chamado. O `get_or_create` usa **`(colaborador,data_turno)`**, com demais campos em `defaults`; a central lê essa mesma entidade. (`app/ponto/management/commands/processar_alertas_turno.py:389`, `app/ponto/management/commands/processar_alertas_turno.py:391`, `app/ponto/views.py:1704`)

A garantia de banco da fila de justificativas inclui unicidade de **pendente por `(colaborador,data_turno)`** e a unicidade geral `(colaborador,data_turno,batida_entrada)`. (`app/ponto/models.py:187`, `app/ponto/models.py:189`)

**PortaFila:** o nome encontrado no código é o valor de rota **`'porta_fila'`**, retornado pelo juiz de alcance do dia, não uma classe/modelo `PortaFila` nas fontes encontradas; o retorno carrega IDs da fila e verbos. (`app/chamados/juizes.py:1352`, `app/chamados/juizes.py:1386`; inexistência absoluta de modelo `[não verificado]`)

O calendário administrativo também oferece um botão **DP** que abre pauta já com colaborador e data, na mesma fileira do “Resolver dia”. (`app/templates/colaboradores/partials/_calendario_grade.html:125`, `app/templates/colaboradores/partials/_calendario_grade.html:126`)

## 06/11 17:21 · sessao fac77d592841 · - · - · origem=Ronald (tmux)

2. **Não há chamador UI identificado para `trocar_plantao` ou `editar_dia`.** A chamada de produção da primeira está no comando O10-C (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:160`), cujo cabeçalho declara ser seu primeiro chamador de produção (`:4`). A chamada de `editar_dia` está dentro da própria troca (`app/ponto/portas/celula.py:675`).

A tela mais próxima é **perfil do colaborador → Calendário de Ponto → dia**: o perfil carrega `calendario_ponto` (`app/templates/colaboradores/detalhe.html:215`); a rota é `app/colaboradores/urls.py:24`; a view monta o calendário e concede `pode_veredito` conforme `regularizar_batida` (`app/colaboradores/views.py:1935`, `:1945`, `:1948`, `:1957`); o template inclui a grade (`app/templates/colaboradores/calendario_partial.html:62`), que inclui **Resolver dia** com colaborador e data (`app/templates/colaboradores/partials/_calendario_grade.html:125-126`).

O drawer de diagnóstico também chega à mesma grade: view (`app/colaboradores/views.py:2938-2943`), contexto (`app/colaboradores/services/drawer.py:120-121`) e inclusão (`app/templates/colaboradores/partials/_drawer_diagnostico.html:33-36`).

**Resolver dia é outra cadeia:** form → `ponto:veredito_celula` (`app/templates/ponto/partials/_veredito_dia.html:17`) → rota (`app/ponto/urls.py:58`) → `veredito_celula_view` (`app/ponto/views.py:3312-3341`) → serviço `veredito_celula` (`app/ponto/services/veredito_celula.py:191`), com apenas `trabalhou`, `faltou` e `justificado` (`:18`). “Justificado” chama `criar_ausencia`, não a troca (`:310-317`).

A lacuna factual nessa tela é: o formulário só oferece `colab`, `data`, `volta`, veredito e tipo de ausência (`app/templates/ponto/partials/_veredito_dia.html:19-25`); não coleta substituto, duas Ausências/cartas ou motivo da troca exigidos pela assinatura da porta (`app/ponto/portas/celula.py:632`, `:646-667`), nem despacha para ela (`app/ponto/views.py:3340-3341`). Também não oferece o booleano `trabalha` e motivo exigidos por `editar_dia` (`app/ponto/portas/celula.py:534`, `:561-564`).

Além disso, Resolver dia não é garantido em qualquer dia: exige permissão e o predicado do dia (`app/templates/colaboradores/partials/_calendario_grade.html:125`); esse predicado exige último recurso, passado e status `falta`, `irregular`, `aberto` ou `sem_certidao` (`app/colaboradores/services/calendario.py:94`, `:97-104`).

3. **Os dois anexos são duas cartas, cada uma no campo `Ausencia.documento`: uma do cedente e outra do substituto.** O campo é `FileField`, `upload_to="ausencias/documentos/"`, permitindo vazio no modelo (`app/ponto/models.py:777-780`); a obrigatoriedade dupla nasce na porta (`app/ponto/portas/celula.py:635`, `:664-667`).

A porta recusa lado ausente (`app/ponto/portas/celula.py:653-655`), recarrega e trava as duas Ausências por PK (`:656-659`), recusa registro inexistente (`:660-661`), recusa o mesmo colaborador nos dois lados (`:662-663`) e verifica literalmente `if not a.documento` para cada lado (`:664-667`). A trilha guarda o nome de cada documento (`:682-691`).

Essa validação não consulta `documento_referencia` nem abre o arquivo; seu teste é o preenchimento de `a.documento` (`app/ponto/portas/celula.py:664-667`). O painel de Ausência permite anexar/substituir **um documento individual** (`app/templates/ponto/partials/painel_ausencia.html:90-93`), pela view `anexar_documento_ausencia`, que recebe `request.FILES['documento']` e chama o serviço de anexo (`app/ponto/views.py:2133-2151`).

## 06/11 17:11 · sessao b51996e540eb · R1-CAUSAD-V3 · leitura · origem=Code

**A cobertura contábil fecha: outubro tem `504 medidos + 120 excluídos = 624`; novembro, `467 + 111 = 578`. O impacto abaixo abrange os MEDIDOS; todas as partes declaram `COBERTURA_INTEGRAL=False`.** [nao verificado] Origem: P1–P6, `COBERTURA_COLAB`, `UNIVERSO` e `COBERTURA_INTEGRAL`.

P1–P6 identificam os seis blocos fornecidos. Como são transcrições no pedido, sem arquivo:linha verificável, cada resultado recebe `[nao verificado]` e sua chave de origem. Nas tabelas, as rubricas aparecem sem o prefixo `horas_`. [nao verificado]

Calculei `Δ = Decimal(PARCIAL.v3_h) − Decimal(PARCIAL.v2_h)`, somando sem arredondamento intermediário. `C/D` significa colaboradores/dias-colaborador; no impacto, corresponde a `colabs_div/dias_colab_div`. `fm_dias` não foi usado como quantidade de dias. [nao verificado] Origem: campos das linhas `PARCIAL`.

**1) Cobertura e exclusões**

Cada célula abaixo vem de `Pn:COBERTURA_COLAB|competência`; as seis parcelas formam o total. [nao verificado]

| Parte | 10/2026: medidos + excluídos = universo | 11/2026: medidos + excluídos = universo | Referência |
|---|---:|---:|---|
| P1 | 86 + 24 = 110 | 74 + 22 = 96 | [nao verificado] |
| P2 | 81 + 28 = 109 | 79 + 20 = 99 | [nao verificado] |
| P3 | 86 + 16 = 102 | 78 + 19 = 97 | [nao verificado] |
| P4 | 79 + 17 = 96 | 71 + 20 = 91 | [nao verificado] |
| P5 | 89 + 18 = 107 | 86 + 13 = 99 | [nao verificado] |
| P6 | 83 + 17 = 100 | 79 + 17 = 96 | [nao verificado] |
| **Soma** | **504 + 120 = 624** | **467 + 111 = 578** | [nao verificado] |

As contas por parte e somadas fecham; os totais coincidem com `UNIVERSO|2026-10|colabs_porta=624` e `UNIVERSO|2026-11|colabs_porta=578`. Isso é fechamento da cobertura contábil, não cobertura integral da medida. [nao verificado]

Motivos: origem `Pn:EXCLUIDO|competência|colNNN|PRODUTO:classe`, com contagens resumidas em `PRODUTO|competência|classe`. `—` significa ausência de linha dessa classe, contribuindo zero à soma do texto. [nao verificado]

| Competência / motivo | P1 | P2 | P3 | P4 | P5 | P6 | Soma | Referência |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| 10 — RECUSA_PORTA_SEM_DONO | 18 | 19 | 13 | 11 | 11 | 13 | **85** | [nao verificado] |
| 10 — SEM_RETORNO_FECHAMENTO | 6 | 6 | 1 | 5 | 7 | 3 | **28** | [nao verificado] |
| 10 — MOTOR_FECHAMENTO_horas_trabalhadas | — | 3 | 2 | 1 | — | 1 | **7** | [nao verificado] |
| **10 — total** | **24** | **28** | **16** | **17** | **18** | **17** | **120** | [nao verificado] |
| 11 — RECUSA_PORTA_SEM_DONO | 21 | 16 | 17 | 16 | 12 | 15 | **97** | [nao verificado] |
| 11 — SEM_RETORNO_FECHAMENTO | — | 1 | 1 | 3 | 1 | 1 | **7** | [nao verificado] |
| 11 — MOTOR_FECHAMENTO_horas_trabalhadas | 1 | 3 | 1 | 1 | — | 1 | **7** | [nao verificado] |
| **11 — total** | **22** | **20** | **19** | **20** | **13** | **17** | **111** | [nao verificado] |

Portanto, por classe: outubro `85 + 28 + 7 = 120`; novembro `97 + 7 + 7 = 111`. [nao verificado] Origem: parcelas da tabela anterior.

**2) Impacto por rubrica dos MEDIDOS**

Origem de cada célula Pn: `Pn:PARCIAL|competência|horas_<rubrica>`, campos `v2_h/v3_h`. A coluna soma é a adição das seis diferenças decimais exibidas. [nao verificado]

**10/2026 — Δ em horas**

| Rubrica | P1 | P2 | P3 | P4 | P5 | P6 | **Σ Δ** | Referência |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| trabalhadas | -2.542890555555 | -2.842305277778 | -3.035321666668 | 0 | -9.540302056388 | -4.528065277778 | **-22.488884834167** | [nao verificado] |
| intra_indenizada | -2 | 0 | -4.05666694444443 | -0.63527333333331 | -8.16666666666663 | -8 | **-22.85860694444437** | [nao verificado] |
| extras_50 | 0 | 0 | 0 | -0.502948333333335 | 1.4735158538888901 | 0.224154212500004 | **1.1947217330555591** | [nao verificado] |
| extras_100 | 0 | 10.985971388888899 | 12.149131944444441 | 0 | 0 | 0 | **23.135103333333340** | [nao verificado] |
| extras_100_feriado | 0 | 10.985971388888899 | 12.149131944444441 | 0 | 0 | 0 | **23.135103333333340** | [nao verificado] |
| extras_50_noturna | 0 | 0 | 0 | 0 | 0 | 0 | **0** | [nao verificado] |
| extras_100_noturna | 2.061344224166667 | 6.164677786111110 | 0.014159722222222 | 1.4042363383333330 | 0.9115786111111086 | 0.9999999999999999 | **11.5559966819444405** | [nao verificado] |
| folga_trabalhada | 0 | 0 | 0 | 0 | 0 | 0 | **0** | [nao verificado] |
| noturnas | 0 | -2.5623561507937 | 0 | 2.5716699836512 | 0.5618157142858 | 0 | **0.5711295471433** | [nao verificado] |
| atraso | 2.368802113333331 | 4.579233728611108 | 1.106468535277777 | 0.186039155000003 | 2.329948153055554 | -0.000374091944444 | **10.570117593333329** | [nao verificado] |
| saida_antecipada | 0.000684534722225 | 0.000212258611107 | -0.557056842500004 | -0.186263274722222 | 0.000085790277780 | 0.000029152222226 | **-0.742308381388888** | [nao verificado] |

**11/2026 — Δ em horas**

| Rubrica | P1 | P2 | P3 | P4 | P5 | P6 | **Σ Δ** | Referência |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| trabalhadas | 0 | -2.8425216666665 | -3.9930194444451 | 0 | -17.0800343119445 | 0 | **-23.9155754230561** | [nao verificado] |
| intra_indenizada | -1.16666666666666 | 0 | -1 | 0.40334305555555 | -9.62847277777777 | -6 | **-17.39179638888888** | [nao verificado] |
| extras_50 | 0 | 0 | 0 | 0 | -3.3361696744444433 | 0.380845000000001 | **-2.9553246744444423** | [nao verificado] |
| extras_100 | 1.5E-14 | -7.052801863055549 | 12.066969811944446 | 3.958275277777773 | 20.045843333333323 | 0 | **29.018286560000008** | [nao verificado] |
| extras_100_feriado | 1.5E-14 | -7.052801863055549 | 12.066969811944446 | 3.958275277777773 | 16.043546944444436 | 0 | **25.015990171111121** | [nao verificado] |
| extras_50_noturna | 0 | 0 | 0 | 0 | 0 | 0 | **0** | [nao verificado] |
| extras_100_noturna | 1.137680829722223 | 7.819367573333334 | 1 | 1.4999583002777781 | 0.002409166666664 | 1.8612788888888885 | **13.3206947588888876** | [nao verificado] |
| folga_trabalhada | 0 | 0 | 0 | 0 | 0 | 0 | **0** | [nao verificado] |
| noturnas | 0 | 0 | 0 | 0 | 0 | 0 | **0** | [nao verificado] |
| atraso | -0.000158523888889 | 0.622596846944446 | 0.000051118333336 | 1.128749770277779 | 0.076214444444446 | -0.000024863333337 | **1.827428792777781** | [nao verificado] |
| saida_antecipada | -0.000158888888891 | 0.000201111111110 | 0.000008888888885 | -0.755222120833338 | -0.076319288888886 | 4.331691891666661 | **3.500201593055541** | [nao verificado] |

Contagens afetadas publicadas: origem `Pn:PARCIAL|competência|horas_<rubrica>|…|colabs_div=C|dias_colab_div=D`. Cada sequência está na ordem **P1 + P2 + P3 + P4 + P5 + P6**. [nao verificado] [nao verificado]

| Rubrica | 10/2026 — parcelas C/D → soma | 11/2026 — parcelas C/D → soma | Referência |
|---|---|---|---|
| trabalhadas | 1/1 + 1/1 + 2/2 + 0/0 + 3/3 + 1/1 → **8/8** | 0/0 + 1/1 + 1/1 + 0/0 + 3/5 + 0/0 → **5/7** | [nao verificado] |
| intra_indenizada | 1/2 + 0/0 + 4/9 + 2/3 + 1/7 + 2/8 → **10/29** | 1/1 + 0/0 + 1/1 + 1/2 + 3/10 + 2/6 → **8/20** | [nao verificado] |
| extras_50 | 0/0 + 0/0 + 0/0 + 1/2 + 2/5 + 1/1 → **4/8** | 0/0 + 0/0 + 0/0 + 0/0 + 3/3 + 1/2 → **4/5** | [nao verificado] |
| extras_100 | 0/0 + 1/1 + 1/1 + 0/0 + 0/0 + 0/0 → **2/2** | 0/0 + 1/1 + 1/1 + 1/1 + 1/3 + 0/0 → **4/6** | [nao verificado] |
| extras_100_feriado | 0/0 + 1/1 + 1/1 + 0/0 + 0/0 + 0/0 → **2/2** | 0/0 + 1/1 + 1/1 + 1/1 + 1/1 + 0/0 → **4/4** | [nao verificado] |
| extras_50_noturna | seis 0/0 → **0/0** | seis 0/0 → **0/0** | [nao verificado] |
| extras_100_noturna | 3/3 + 2/2 + 0/0 + 2/2 + 1/1 + 1/1 → **9/9** | 2/2 + 3/3 + 1/1 + 2/2 + 0/0 + 1/1 → **9/9** | [nao verificado] |
| folga_trabalhada | seis 0/0 → **0/0** | seis 0/0 → **0/0** | [nao verificado] |
| noturnas | 0/0 + 2/9 + 0/0 + 1/3 + 1/4 + 0/0 → **4/16** | seis 0/0 → **0/0** | [nao verificado] |
| atraso | 1/1 + 3/4 + 1/1 + 1/1 + 1/1 + 0/0 → **7/8** | 0/0 + 1/1 + 0/0 + 2/4 + 1/1 + 0/0 → **4/6** | [nao verificado] |
| saida_antecipada | 0/0 + 0/0 + 1/1 + 1/1 + 0/0 + 0/0 → **2/2** | 0/0 + 0/0 + 0/0 + 1/3 + 1/1 + 1/1 → **3/5** | [nao verificado] |

Esses C/D são as contagens de divergência publicadas pelo instrumento: há deltas residuais com `colabs_div=0`, como `extras_100_noturna` de outubro/P3. Portanto, não representam a contagem de todo valor textual que mudou. [nao verificado] Origem: P3, `PARCIAL|2026-10|horas_extras_100_noturna` e `RESIDUO|…|ATE_LIMIAR`. [nao verificado]

Para distinguir as duas medidas, seguem também as somas decimais de `SOMA DIVERGENCIA|competência|rubrica|partes_h`. Elas excluem o resíduo da divergência atribuída e não substituem o delta completo de `PARCIAL`. [nao verificado]

| Rubrica | 10/2026 — Σ partes_h | 11/2026 — Σ partes_h | Referência |
|---|---:|---:|---|
| trabalhadas | -22.4888848341666705 | -23.9155754230555561 | [nao verificado] |
| intra_indenizada | -22.85860694444444556 | -17.39179638888888795 | [nao verificado] |
| extras_50 | 1.19472173305555515 | -2.95532467444444169 | [nao verificado] |
| extras_100 | 23.135103333333331 | 29.0182865599999961 | [nao verificado] |
| extras_100_feriado | 23.135103333333331 | 25.0159901711111061 | [nao verificado] |
| extras_50_noturna | 0 | 0 | [nao verificado] |
| extras_100_noturna | 11.54273918194444492 | 13.2956696391666664 | [nao verificado] |
| folga_trabalhada | 0 | 0 | [nao verificado] |
| noturnas | 0.57112954714284966 | 0 | [nao verificado] |
| atraso | 10.5703825000000005 | 1.827685833333333356 | [nao verificado] |
| saida_antecipada | -0.74333333333333329 | 3.500166944444443826 | [nao verificado] |

Não usei `INSTRUMENTO_HISTORICO` como esse DIFF: por exemplo, `folga_trabalhada` tem `PARCIAL.v2_h=v3_h=0` em todas as partes de outubro, enquanto o histórico mostra outra comparação. Pequenas diferenças frente a `RUBRICA.delta` também foram preservadas: os cálculos acima subtraem as strings decimais de `PARCIAL`, sem reutilizar a subtração binária já impressa. [nao verificado] Origem: linhas `PARCIAL`, `RUBRICA` e `INSTRUMENTO_HISTORICO` dos anexos.

**3) Dono por rubrica e contagens**

Origem: `Pn:DONO|competência|horas_<rubrica>|classe|…|C|D`. Abaixo, `—` significa nenhuma linha `DONO` publicada para aquela classe/rubrica. [nao verificado] [nao verificado]

**10/2026**

| Rubrica | ESTRUTURA: parcelas → C/D | CADASTRO: parcelas → C/D | BATIDA: parcelas → C/D | Referência |
|---|---|---|---|---|
| trabalhadas | P3 1/1 → **1/1** | P1 1/1 + P2 1/1 + P5 3/3 + P6 1/1 → **6/6** | P3 1/1 → **1/1** | [nao verificado] |
| intra_indenizada | P3 1/1 → **1/1** | P3 3/6 + P4 2/3 + P5 1/7 + P6 2/8 → **8/24** | P1 1/2 + P3 2/2 → **3/4** | [nao verificado] |
| extras_50 | — | P4 1/2 + P5 2/5 + P6 1/1 → **4/8** | — | [nao verificado] |
| extras_100 | P2 1/1 + P3 1/1 → **2/2** | — | — | [nao verificado] |
| extras_100_feriado | P2 1/1 + P3 1/1 → **2/2** | — | — | [nao verificado] |
| extras_50_noturna | — | — | — | [nao verificado] |
| extras_100_noturna | P1 3/3 + P4 2/2 + P5 1/1 → **6/6** | P2 2/2 + P6 1/1 → **3/3** | — | [nao verificado] |
| folga_trabalhada | — | — | — | [nao verificado] |
| noturnas | P2 2/9 + P4 1/3 + P5 1/2 → **4/14** | P5 1/1 → **1/1** | P5 1/1 → **1/1** | [nao verificado] |
| atraso | P1 1/1 + P2 2/3 + P3 1/1 + P5 1/1 → **5/6** | P2 1/1 + P4 1/1 → **2/2** | — | [nao verificado] |
| saida_antecipada | P3 1/1 → **1/1** | P4 1/1 → **1/1** | — | [nao verificado] |

**11/2026**

| Rubrica | ESTRUTURA: parcelas → C/D | CADASTRO: parcelas → C/D | BATIDA: parcelas → C/D | Referência |
|---|---|---|---|---|
| trabalhadas | — | P2 1/1 + P5 3/5 → **4/6** | P3 1/1 → **1/1** | [nao verificado] |
| intra_indenizada | — | P1 1/1 + P3 1/1 + P4 1/2 + P5 3/8 + P6 2/6 → **8/18** | P5 1/2 → **1/2** | [nao verificado] |
| extras_50 | — | P5 3/3 + P6 1/2 → **4/5** | — | [nao verificado] |
| extras_100 | P2 1/1 + P3 1/1 + P4 1/1 → **3/3** | P5 1/1 → **1/1** | P5 1/2 → **1/2** | [nao verificado] |
| extras_100_feriado | P2 1/1 + P3 1/1 + P4 1/1 → **3/3** | P5 1/1 → **1/1** | — | [nao verificado] |
| extras_50_noturna | — | — | — | [nao verificado] |
| extras_100_noturna | P1 2/2 + P2 2/2 + P4 2/2 → **6/6** | P2 1/1 + P3 1/1 + P6 1/1 → **3/3** | — | [nao verificado] |
| folga_trabalhada | — | — | — | [nao verificado] |
| noturnas | — | — | — | [nao verificado] |
| atraso | P2 1/1 + P4 1/1 → **2/2** | P4 1/3 + P5 1/1 → **2/4** | — | [nao verificado] |
| saida_antecipada | P6 1/1 → **1/1** | P4 1/3 + P5 1/1 → **2/4** | — | [nao verificado] |

Os colaboradores não devem ser somados entre donos como união: outubro/intra soma `12` presenças nas classes, mas `PARCIAL` informa `10` colaboradores; outubro/noturnas dá `6` contra `4`; novembro/intra, `9` contra `8`; novembro/extras_100, `5` contra `4`. [nao verificado] Origem: `DONO` e `PARCIAL` dessas rubricas, P1–P6.

`RESIDUO` está separado porque o juiz de dono retorna `CADASTRO`, `BATIDA` ou `ESTRUTURA`. [e6_oraculo.py:98](/home/ronald/codex_ws/320ef1c738b115e3f7e0004e4c51a48a6eb2d953/app/ponto/management/commands/e6_oraculo.py:98)

Contagens publicadas de **RESIDUO/ATE_LIMIAR**, na ordem P1 + P2 + P3 + P4 + P5 + P6; são participação nessa classe, não a união de colaboradores afetados de `PARCIAL`. Origem: `Pn:RESIDUO|competência|horas_<rubrica>|ATE_LIMIAR|…|C|D`. [nao verificado] [nao verificado]

| Rubrica | 10/2026 — parcelas → C/D | 11/2026 — parcelas → C/D | Referência |
|---|---|---|---|
| trabalhadas | 86/1222 + 81/1254 + 86/1222 + 79/1236 + 89/1333 + 83/1271 → **504/7538** | 74/588 + 79/650 + 78/629 + 71/591 + 86/726 + 79/663 → **467/3847** | [nao verificado] |
| intra_indenizada | 86/1221 + 81/1255 + 86/1215 + 79/1233 + 89/1329 + 83/1264 → **504/7517** | 74/587 + 79/651 + 78/629 + 71/589 + 86/721 + 79/657 → **467/3834** | [nao verificado] |
| extras_50 | 84/1198 + 81/1242 + 86/1196 + 79/1215 + 89/1312 + 83/1251 → **502/7414** | 70/561 + 76/627 + 77/603 + 70/582 + 84/705 + 79/646 → **456/3724** | [nao verificado] |
| extras_100 | 84/1198 + 81/1242 + 86/1195 + 79/1217 + 89/1317 + 83/1252 → **502/7421** | 70/561 + 76/626 + 77/602 + 70/581 + 84/705 + 79/648 → **456/3723** | [nao verificado] |
| extras_100_feriado | 84/1198 + 81/1242 + 86/1195 + 79/1217 + 89/1317 + 83/1252 → **502/7421** | 70/561 + 76/626 + 77/602 + 70/581 + 84/707 + 79/648 → **456/3725** | [nao verificado] |
| extras_50_noturna | 84/1198 + 81/1242 + 86/1196 + 79/1217 + 89/1317 + 83/1252 → **502/7422** | 70/561 + 76/627 + 77/603 + 70/582 + 84/708 + 79/648 → **456/3729** | [nao verificado] |
| extras_100_noturna | 84/1195 + 81/1240 + 86/1196 + 79/1215 + 89/1316 + 83/1251 → **502/7413** | 70/559 + 76/624 + 77/602 + 70/580 + 84/708 + 79/647 → **456/3720** | [nao verificado] |
| folga_trabalhada | 86/1223 + 81/1255 + 86/1224 + 79/1236 + 89/1336 + 83/1272 → **504/7546** | 74/588 + 79/651 + 78/630 + 71/591 + 86/731 + 79/663 → **467/3854** | [nao verificado] |
| noturnas | 85/1207 + 81/1239 + 86/1213 + 79/1222 + 89/1322 + 83/1259 → **503/7462** | 73/575 + 78/642 + 78/621 + 70/582 + 86/728 + 79/655 → **464/3803** | [nao verificado] |
| atraso | 83/1192 + 79/1227 + 84/1177 + 79/1216 + 88/1312 + 82/1243 → **495/7367** | 69/559 + 75/622 + 75/596 + 69/574 + 84/707 + 78/645 → **450/3703** | [nao verificado] |
| saida_antecipada | 83/1193 + 79/1231 + 84/1177 + 79/1216 + 88/1313 + 82/1243 → **495/7373** | 69/559 + 75/623 + 75/596 + 69/575 + 84/707 + 77/644 → **449/3704** | [nao verificado] |

As contagens de **RESIDUO/SEM_DECISAO** coincidem, rubrica a rubrica e parte a parte, com `PRODUTO:SEM_DECISAO_horas_<rubrica>` nas tabelas seguintes. Os deltas v3−v2 publicados nessas linhas de resíduo são zero. [nao verificado] Origem: P1–P6, `RESIDUO|competência|rubrica|SEM_DECISAO` e respectivos `PRODUTO`. [nao verificado]

**4) PRODUTO por classe**

Células em **C/D**, origem `Pn:PRODUTO|competência|classe|C|D`. Classes agrupadas na mesma linha têm individualmente os mesmos valores; o total não multiplica pelo número de classes. [nao verificado]

**10/2026**

| Classe(s) | P1 | P2 | P3 | P4 | P5 | P6 | **Σ C/D** | Referência |
|---|---|---|---|---|---|---|---|---|
| DIA_FORA_UNIVERSO_PORTA; SEM_DECISAO_horas_trabalhadas; SEM_DECISAO_horas_intra_indenizada; SEM_DECISAO_horas_folga_trabalhada | 56/296 | 51/247 | 58/324 | 48/321 | 59/346 | 52/229 | **324/1763** | [nao verificado] |
| MOTOR_FECHAMENTO_horas_trabalhadas | — | 3/54 | 2/42 | 1/19 | — | 1/24 | **7/139** | [nao verificado] |
| RECUSA_PORTA_SEM_DONO | 18/237 | 19/295 | 13/137 | 11/191 | 11/155 | 13/181 | **85/1196** | [nao verificado] |
| SEM_DECISAO_horas_atraso; SEM_DECISAO_horas_saida_antecipada | 59/326 | 55/271 | 61/370 | 49/340 | 61/369 | 55/258 | **340/1934** | [nao verificado] |
| SEM_DECISAO_horas_extras_100; SEM_DECISAO_horas_extras_100_feriado | 59/321 | 53/259 | 59/352 | 49/340 | 60/365 | 54/249 | **334/1886** | [nao verificado] |
| SEM_DECISAO_horas_extras_100_noturna; SEM_DECISAO_horas_extras_50; SEM_DECISAO_horas_extras_50_noturna | 59/321 | 53/260 | 59/352 | 49/340 | 60/365 | 54/249 | **334/1887** | [nao verificado] |
| SEM_DECISAO_horas_noturnas | 59/312 | 52/254 | 59/335 | 49/332 | 61/356 | 55/242 | **335/1831** | [nao verificado] |
| SEM_RETORNO_FECHAMENTO | 6/0 | 6/0 | 1/0 | 5/0 | 7/0 | 3/0 | **28/0** | [nao verificado] |

**11/2026**

| Classe(s) | P1 | P2 | P3 | P4 | P5 | P6 | **Σ C/D** | Referência |
|---|---|---|---|---|---|---|---|---|
| DIA_FORA_UNIVERSO_PORTA; SEM_DECISAO_horas_trabalhadas; SEM_DECISAO_horas_intra_indenizada; SEM_DECISAO_horas_folga_trabalhada | 74/817 | 77/857 | 76/805 | 70/791 | 86/967 | 78/876 | **461/5113** | [nao verificado] |
| MOTOR_FECHAMENTO_horas_trabalhadas | 1/14 | 3/39 | 1/14 | 1/11 | — | 1/12 | **7/90** | [nao verificado] |
| RECUSA_PORTA_SEM_DONO | 21/329 | 16/269 | 17/272 | 16/310 | 12/239 | 15/223 | **97/1642** | [nao verificado] |
| SEM_DECISAO_horas_atraso; SEM_DECISAO_horas_saida_antecipada | 74/846 | 78/885 | 77/839 | 71/804 | 86/990 | 79/894 | **465/5258** | [nao verificado] |
| SEM_DECISAO_horas_extras_100; SEM_DECISAO_horas_extras_100_feriado; SEM_DECISAO_horas_extras_100_noturna; SEM_DECISAO_horas_extras_50; SEM_DECISAO_horas_extras_50_noturna | 74/844 | 77/881 | 76/832 | 70/800 | 86/990 | 78/891 | **461/5238** | [nao verificado] |
| SEM_DECISAO_horas_noturnas | 74/830 | 78/866 | 76/814 | 70/800 | 86/970 | 79/884 | **463/5164** | [nao verificado] |
| SEM_RETORNO_FECHAMENTO | — | 1/0 | 1/0 | 3/0 | 1/0 | 1/0 | **7/0** | [nao verificado] |

Não há união global de pessoas entre classes nessas saídas; somar classes produziria presenças repetidas. `SEM_RETORNO_FECHAMENTO` informa dias `0`, mas também publica `DIAS_NAO_LOCALIZADOS`: esse zero não comprova ausência de dias afetados. [nao verificado] Origem: linhas `PRODUTO` e `DIAS_NAO_LOCALIZADOS` das respectivas partes.

**5) Execução por parte**

Origem: rodapés `rc_sombra`, `COLABS_COM_FALHA_INSTRUMENTO`, `EXCLUIDOS_TOTAL`; para os demais campos, presença ou ausência de ocorrência no texto fornecido. [nao verificado]

| Parte | rc_sombra | INCOMPLETO | Colabs com falha do instrumento | Linhas de erro | TRATADA_LOCAL | Excluídos nas duas competências | Referência |
|---|---:|---|---:|---|---|---:|---|
| P1 | 0 | não aparece | 0 | não aparecem | não aparece | 46 | [nao verificado] |
| P2 | 0 | não aparece | 0 | não aparecem | não aparece | 48 | [nao verificado] |
| P3 | 0 | não aparece | 0 | não aparecem | não aparece | 35 | [nao verificado] |
| P4 | 0 | não aparece | 0 | não aparecem | não aparece | 37 | [nao verificado] |
| P5 | 0 | não aparece | 0 | não aparecem | não aparece | 31 | [nao verificado] |
| P6 | 0 | não aparece | 0 | não aparecem | não aparece | 34 | [nao verificado] |
| **Soma** | — | — | **0** | — | — | **231** | [nao verificado] |

Os `231 = 120 + 111` são ocorrências de exclusão por competência, sem deduplicação entre meses. Ausência de linha `TRATADA_LOCAL` não fornece uma contagem comprovada de exceções tratadas localmente. [nao verificado]

Todas as linhas `SOMA` publicadas trazem indicadores `True`. Isso registra o que os anexos declaram; não certifica a execução nem valida o motor v3. [nao verificado] Origem: linhas `SOMA` de P1–P6. O p

[truncado em 20000 de 22933 caracteres]

## 06/11 17:07 · sessao 7fc157e55244 · E-SEM-VIVA-B · censo · origem=Code

**Classificação: os cinco achados ficam em (b), com a ressalva de precisão do item 2 abaixo.** Os alertas sobre classificação incompleta, testemunha e perda da C5 aparecem nas respostas e foram restringidos ou descartados pela triagem. (`v2.r:1,20–24`; `v3_triagem:3–4`; `v3b.r:32–38`.)

Nas referências aos **anexos fornecidos**, conto linhas desde a primeira após `== anexo: ... ==`, incluindo linhas vazias. Essas referências identificam o texto recebido; as referências `app/...` identificam o código conferido.

Primeiro, quem perguntou e quem triou: a pergunta original exigia comparação com **`classificar_export`**, incluindo demissão nula, e cobrava derivação paralela e força dos testes. Portanto, esses temas foram perguntados. A restrição a “rescisão + retentor” aparece expressamente na triagem e depois volta como premissa da pergunta 3b. (`v2.q:7,10`; `v3_triagem:3`; `v3b.q:3`.) Essa restrição também recebeu concordância posterior do Codex: “**O descarte de escopo procede**”. Isso é participação no rebaixamento, embora o problema já tivesse sido apontado. (`v3b.r:38`.) [nao verificado]

1. **Derivação paralela do juiz registrado — (b), falha da triagem.**

   **Pergunta literal:** “2o juiz duplicando juiz registrado” e “derivacao paralela de fonte unica”. (`v2.q:10`.)

   **Resposta literal:** “A C5 consulta a autoridade da rescisão; continua sem consultar os demais portões de `classificar_export` e os filtros do retentor.” (`v2.r:1`.)

   **Triagem e motivo literal:** “Equivalencia com classificar_export inteiro [...] NAO provada: fora do recorte (aval "comp que ele segura" = rescisao+retentor pela triagem v2)”. (`v3_triagem:3`.)

   O alerta sobre a composição incompleta foi visto. A triagem redefiniu a comparação e posteriormente declarou “derivacao paralela ausente”. (`v2.r:1`; `v3_triagem:3,8`.) Na cópia, a C5 compõe diretamente as duas funções; o registro aponta `classificar_export` como juiz, e existe leitor que primeiro consulta esse classificador. (`app/escala/services/cadastro_realidade.py:158,169–172`; `app/core/juizes.py:298–299`; `app/folha/fora_da_folha.py:115–137`.)

2. **Desligado sem demissão/sem fechamento incluído na C5 — (b), com identificação anterior parcial do encadeamento.**

   **Pergunta literal:** “O conjunto de dias que colabs_nunca_bateu devolve para um desligado coincide com o conjunto que folha/export.py::classificar_export seguraria [...]? Construa contra-exemplos: [...] demissao nula”. (`v2.q:7`.)

   **Resposta literal:** “Sem fechamento, não existe item na classificação padrão; acrescentando um fechamento, ele sairia por `sem_codigo_dominio`, antes de `espelho_nunca_bateu`. Portanto a igualdade testada não prova a igualdade perguntada.” (`v2.r:24`.)

   **Triagem e motivo literal:** “Equivalencia com classificar_export inteiro (sem fechamento, sem_codigo_dominio, ferias_com_batida, cadastro_zero) NAO provada: fora do recorte”. (`v3_triagem:3`.)

   **Ressalva:** essa resposta não explicita a ligação específica entre demissão nula e exclusão do universo do fechamento. Ela identifica, porém, a ausência de fechamento nas próprias fixtures e o consequente desencontro com a classificação — inclusive o “inverso” mencionado pelo opus. Por isso classifico o achado numerado em (b), sem atribuir ao Codex uma descrição completa que ele não escreveu. (`v2.r:24`; `opus_final_97411e5c.md:3`.)

   O encadeamento específico é verificável no código: desligado com demissão nula fica fora do universo; a classificação padrão percorre fechamentos; o teste exige que seus dias entrem na C5. (`app/ponto/services/fechamento.py:402,423–432`; `app/folha/export.py:775–780`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:86–90`.)

3. **Testemunha repete a composição e fixture não tem fechamento — (b), falha da triagem.**

   **Pergunta literal:** “testemunha = exportacao da folha” e “selo/RED sem mordida (os testes mordem os mutantes relevantes? qual mutante sobreviveria?)”. (`v2.q:3,10`.) [nao verificado]

   **Resposta literal:** “O helper chama somente `sai_pela_rescisao` e `motivos_retencao_celula`; o classificador real exige fechamento”. Também: “Portanto a igualdade testada não prova a igualdade perguntada.” (`v2.r:22,24`.)

   **Triagem e motivo literal:** “Testemunha do teste = a mesma funcao, contraexemplos do Codex [...] viram casos.” Depois, a classificação inteira foi considerada “fora do recorte”. (`v2.triagem:2`; `v3_triagem:3`.) [nao verificado]

   A cura adotada incorporou filtros do retentor, mas manteve a testemunha composta pelas mesmas duas autoridades da implementação. A fixture cria colaborador e células; o helper recompõe rescisão + retenção. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:31–35,49–60`; `app/escala/services/cadastro_realidade.py:169–172`.)

4. **Equivalência inteira não provada deixada como informativa — (b), falha da triagem.**

   **Pergunta literal:** “Os descartes [...] item 2: limite ate ontem e classificar_export inteiro fora do recorte [...] procedem?” (`v3b.q:3`.)

   **Resposta literal:** “O descarte de escopo procede; igualdade com todo `classificar_export` continua sem demonstração.” (`v3b.r:38`.)

   **Triagem e motivo literal:** “2 PROCEDENTE informativo” e “fora do recorte (aval "comp que ele segura" = rescisao+retentor pela triagem v2)”. (`v3_triagem:3`.)

   Aqui o rebaixamento está documentado diretamente: o limite foi reconhecido e aceito. A pergunta 3b voltou a submetê-lo à revisão, e o Codex concordou com o descarte. (`v3_triagem:3`; `v3b.q:3`; `v3b.r:38`.)

5. **Exceção elimina toda a C5, mas a lista continua — (b), falha da triagem.**

   **Pergunta literal:** “Os descartes (item 3: excecao no retentor derruba a C5 [...] ) procedem?” (`v3b.q:3`.)

   **Resposta literal:** “`lista()` captura antes de incorporar qualquer C5, inclusive dos ativos. A lista continua após registrar a exceção.” (`v3b.r:32`.)

   **Triagem e motivo literal:** “DESCARTADO (duvida operacional)” porque “falha ALTA e a lei da casa” e “engolir por colab seria band-aid”. (`v3_triagem:4`.)

   O Codex também contestou essa justificativa: o comentário citado tratava de campos nulos do fechamento, e “descartar a ressalva operacional por essa justificativa não procede”. (`v3b.r:34`.) O código confirma a perda da contribuição C5 com registro da exceção e continuação da lista. A expressão da triagem posterior “derruba a lista() inteira” é imprecisa. (`app/escala/services/cadastro_realidade.py:813–822`; `v3b.triagem:15–17`.)

As execuções de M6, RED e suíte, ocorrências em produção e eventuais omissões de opus anteriores permanecem **[não verificado]**. A análise estática dos testes e dos caminhos acima sustenta as classificações sem depender dessas execuções. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:31–90`; `app/escala/services/cadastro_realidade.py:169–177,813–819`.)

**Contagem por achado numerado, incluindo as sobreposições entre eles:** (`opus_final_97411e5c.md:2–6`; evidências discriminadas acima.)

| Classe | Quantidade |
|---|---:|
| (a) Codex não viu, tema perguntado | 0 |
| (b) Codex viu; triagem descartou/rebaixou | **5** |
| (c) Fora do alcance | 0 |
| (e) Não perguntado | 0 |
| (d) Modo-comum demonstrado | 0 |
| **Total** | **5** |

## 06/11 17:07 · sessao 2c03cec8cc75 · - · - · origem=Ronald (tmux)

Contagens abaixo em **colabs/dias-colab**, apenas das linhas `DONO` emitidas; `—` significa ausência de linha daquela classe no texto. Cada célula referencia a chave `DONO|competência|rubrica|classe` das partes indicadas e permanece `[nao verificado]` por ser transcrição sem arquivo:linha.

| 10/2026 — rubrica | ESTRUTURA: parcelas → soma | CADASTRO: parcelas → soma | BATIDA: parcelas → soma |
|---|---|---|---|
| horas_trabalhadas | P3 1/1 → **1/1** | P1 1/1 + P2 1/1 + P5 3/3 + P6 1/1 → **6/6** | P3 1/1 → **1/1** |
| horas_intra_indenizada | P3 1/1 → **1/1** | P3 3/6 + P4 2/3 + P5 1/7 + P6 2/8 → **8/24** | P1 1/2 + P3 2/2 → **3/4** |
| horas_extras_50 | — | P4 1/2 + P5 2/5 + P6 1/1 → **4/8** | — |
| horas_extras_100 | P2 1/1 + P3 1/1 → **2/2** | — | — |
| horas_extras_100_feriado | P2 1/1 + P3 1/1 → **2/2** | — | — |
| horas_extras_50_noturna | — | — | — |
| horas_extras_100_noturna | P1 3/3 + P4 2/2 + P5 1/1 → **6/6** | P2 2/2 + P6 1/1 → **3/3** | — |
| horas_folga_trabalhada | — | — | — |
| horas_noturnas | P2 2/9 + P4 1/3 + P5 1/2 → **4/14** | P5 1/1 → **1/1** | P5 1/1 → **1/1** |
| horas_atraso | P1 1/1 + P2 2/3 + P3 1/1 + P5 1/1 → **5/6** | P2 1/1 + P4 1/1 → **2/2** | — |
| horas_saida_antecipada | P3 1/1 → **1/1** | P4 1/1 → **1/1** | — |

Todas as células da tabela: `[nao verificado — DONO|2026-10|rubrica|classe, P1..P6]`.

| 11/2026 — rubrica | ESTRUTURA: parcelas → soma | CADASTRO: parcelas → soma | BATIDA: parcelas → soma |
|---|---|---|---|
| horas_trabalhadas | — | P2 1/1 + P5 3/5 → **4/6** | P3 1/1 → **1/1** |
| horas_intra_indenizada | — | P1 1/1 + P3 1/1 + P4 1/2 + P5 3/8 + P6 2/6 → **8/18** | P5 1/2 → **1/2** |
| horas_extras_50 | — | P5 3/3 + P6 1/2 → **4/5** | — |
| horas_extras_100 | P2 1/1 + P3 1/1 + P4 1/1 → **3/3** | P5 1/1 → **1/1** | P5 1/2 → **1/2** |
| horas_extras_100_feriado | P2 1/1 + P3 1/1 + P4 1/1 → **3/3** | P5 1/1 → **1/1** | — |
| horas_extras_50_noturna | — | — | — |
| horas_extras_100_noturna | P1 2/2 + P2 2/2 + P4 2/2 → **6/6** | P2 1/1 + P3 1/1 + P6 1/1 → **3/3** | — |
| horas_folga_trabalhada | — | — | — |
| horas_noturnas | — | — | — |
| horas_atraso | P2 1/1 + P4 1/1 → **2/2** | P4 1/3 + P5 1/1 → **2/4** | — |
| horas_saida_antecipada | P6 1/1 → **1/1** | P4 1/3 + P5 1/1 → **2/4** | — |

Todas as células da tabela: `[nao verificado — DONO|2026-11|rubrica|classe, P1..P6]`.

**Não somar colabs entre donos como união:** um colaborador pode aparecer em classes diferentes em dias distintos. Exemplos concretos: em 10/2026, intra soma `12` presenças nos donos contra `10` colabs na união de `PARCIAL`; noturnas soma `6` contra `4`. Em 11/2026, intra soma `9` contra `8`; extras_100 soma `5` contra `4`. `[nao verificado — DONO e PARCIAL das respectivas rubricas, P1..P6]`

Somas de `partes_h` das linhas **`SOMA DIVERGENCIA`**, calculadas com `Decimal`, sem arredondamento intermediário. Estes valores representam a divergência atribuída e **não substituem** `Σ(Decimal(PARCIAL.v3_h) − Decimal(PARCIAL.v2_h))`, que inclui resíduos. `[nao verificado — SOMA DIVERGENCIA|competência|rubrica|partes_h, P1..P6]`

| Rubrica | 10/2026: Σ partes_h | 11/2026: Σ partes_h |
|---|---:|---:|
| horas_trabalhadas | -22.4888848341666705 | -23.9155754230555561 |
| horas_intra_indenizada | -22.85860694444444556 | -17.39179638888888795 |
| horas_extras_50 | 1.19472173305555515 | -2.95532467444444169 |
| horas_extras_100 | 23.135103333333331 | 29.0182865599999961 |
| horas_extras_100_feriado | 23.135103333333331 | 25.0159901711111061 |
| horas_extras_50_noturna | 0 | 0 |
| horas_extras_100_noturna | 11.54273918194444492 | 13.2956696391666664 |
| horas_folga_trabalhada | 0 | 0 |
| horas_noturnas | 0.57112954714284966 | 0 |
| horas_atraso | 10.5703825000000005 | 1.827685833333333356 |
| horas_saida_antecipada | -0.74333333333333329 | 3.500166944444443826 |

Todas as células da tabela: `[nao verificado — SOMA DIVERGENCIA|competência|rubrica|partes_h, P1..P6]`.

Parcelas de `partes_h`, na ordem P1, P2, P3, P4, P5, P6, para auditoria da soma:

```text
10/2026
horas_trabalhadas:
-2.5428905555555561; -2.8423052777777782; -3.0353216666666674; 0; -9.5403020563888905; -4.5280652777777783
horas_intra_indenizada:
-2; 0; -4.0566669444444443; -0.63527333333333336; -8.1666666666666679; -8
horas_extras_50:
0; 0; 0; -0.50294833333333311; 1.4735158538888897; 0.22415421249999856
horas_extras_100 e horas_extras_100_feriado, cada:
0; 10.985971388888888; 12.149131944444443; 0; 0; 0
horas_extras_100_noturna:
2.0639545019444445; 6.1646777861111115; 0; 1.4042363383333334; 0.90987055555555552; 1
horas_noturnas:
0; -2.5623561507936552; 0; 2.571669983650791; 0.56181571428571386; 0
horas_atraso:
2.3685; 4.5787158333333338; 1.1068333333333333; 0.186; 2.3303333333333334; 0
horas_saida_antecipada:
0; 0; -0.5571666666666667; -0.18616666666666659; 0; 0

11/2026
horas_trabalhadas:
0; -2.8425216666666664; -3.9930194444444447; 0; -17.080034311944445; 0
horas_intra_indenizada:
-1.1666666666666667; 0; -1; 0.40334305555555555; -9.6284727777777768; -6
horas_extras_50:
0; 0; 0; 0; -3.3361696744444429; 0.38084500000000121
horas_extras_100:
0; -7.0528018630555556; 12.066969811944444; 3.9582752777777777; 20.04584333333333; 0
horas_extras_100_feriado:
0; -7.0528018630555556; 12.066969811944444; 3.9582752777777777; 16.04354694444444; 0
horas_extras_100_noturna:
1.1387258297222222; 7.8098224536111109; 1; 1.4858424669444446; 0; 1.8612788888888887
horas_atraso:
0; 0.62282416666666662; 0; 1.1285283333333334; 0.076333333333333336; 0
horas_saida_antecipada:
0; 0; 0; -0.75533333333333341; -0.076333333333333364; 4.3318336111111106
```

Todas as parcelas: `[nao verificado — SOMA DIVERGENCIA|competência|rubrica|partes_h, P1..P6]`. As rubricas omitidas desse bloco têm seis `partes_h=0` emitidos: extras_50_noturna e folga_trabalhada nos dois meses, mais noturnas em novembro. `[nao verificado — mesmas chaves]`

## 06/11 17:05 · sessao 2588e3e439fa · - · - · origem=Ronald (tmux)

Consolidação apenas dos anexos textuais; todas as referências abaixo são `[nao verificado]`, com a chave da linha de origem.

Cobertura — origem em cada parte: `COBERTURA_COLAB|competencia`; fechamento também indicado por `SOMA COBERTURA|competencia|True`. `[nao verificado]`

| Parte | 10/2026: medidos + excluídos = universo | 11/2026: medidos + excluídos = universo |
|---|---:|---:|
| 1 | 86 + 24 = 110 | 74 + 22 = 96 |
| 2 | 81 + 28 = 109 | 79 + 20 = 99 |
| 3 | 86 + 16 = 102 | 78 + 19 = 97 |
| 4 | 79 + 17 = 96 | 71 + 20 = 91 |
| 5 | 89 + 18 = 107 | 86 + 13 = 99 |
| 6 | 83 + 17 = 100 | 79 + 17 = 96 |
| **Soma** | **504 + 120 = 624** | **467 + 111 = 578** |

Todas fecham; os universos somados coincidem com `UNIVERSO|2026-10|colabs_porta=624` e `UNIVERSO|2026-11|colabs_porta=578`. Todas as partes apresentam `COBERTURA_INTEGRAL|competencia|False` nos dois meses. `[nao verificado]`

Motivos de exclusão — origem em cada parte: `EXCLUIDO|competencia|colNNN|PRODUTO:classe`, resumida por `PRODUTO|competencia|classe`. Os traços abaixo indicam classe sem linha `PRODUTO` na parte, sem atribuir observação adicional. `[nao verificado]`

| Competência / classe | P1 | P2 | P3 | P4 | P5 | P6 | Soma |
|---|---:|---:|---:|---:|---:|---:|---:|
| 10 / RECUSA_PORTA_SEM_DONO | 18 | 19 | 13 | 11 | 11 | 13 | **85** |
| 10 / SEM_RETORNO_FECHAMENTO | 6 | 6 | 1 | 5 | 7 | 3 | **28** |
| 10 / MOTOR_FECHAMENTO_horas_trabalhadas | — | 3 | 2 | 1 | — | 1 | **7** |
| **10 / total** | **24** | **28** | **16** | **17** | **18** | **17** | **120** |
| 11 / RECUSA_PORTA_SEM_DONO | 21 | 16 | 17 | 16 | 12 | 15 | **97** |
| 11 / SEM_RETORNO_FECHAMENTO | — | 1 | 1 | 3 | 1 | 1 | **7** |
| 11 / MOTOR_FECHAMENTO_horas_trabalhadas | 1 | 3 | 1 | 1 | — | 1 | **7** |
| **11 / total** | **22** | **20** | **19** | **20** | **13** | **17** | **111** |

PRODUTO: células no formato **colabs/dias**, somadas por classe entre partes. Cada célula vem da linha `PRODUTO|competencia|classe|colabs|dias` da parte correspondente. Não somar classes como pessoas distintas, pois elas podem abranger os mesmos colaboradores. `[nao verificado]`

| 10/2026 — classe(s) com valores iguais | P1 | P2 | P3 | P4 | P5 | P6 | Soma |
|---|---|---|---|---|---|---|---|
| DIA_FORA_UNIVERSO_PORTA; SEM_DECISAO_horas_trabalhadas; SEM_DECISAO_horas_intra_indenizada; SEM_DECISAO_horas_folga_trabalhada | 56/296 | 51/247 | 58/324 | 48/321 | 59/346 | 52/229 | **324/1763** |
| MOTOR_FECHAMENTO_horas_trabalhadas | — | 3/54 | 2/42 | 1/19 | — | 1/24 | **7/139** |
| RECUSA_PORTA_SEM_DONO | 18/237 | 19/295 | 13/137 | 11/191 | 11/155 | 13/181 | **85/1196** |
| SEM_DECISAO_horas_atraso; SEM_DECISAO_horas_saida_antecipada | 59/326 | 55/271 | 61/370 | 49/340 | 61/369 | 55/258 | **340/1934** |
| SEM_DECISAO_horas_extras_100; SEM_DECISAO_horas_extras_100_feriado | 59/321 | 53/259 | 59/352 | 49/340 | 60/365 | 54/249 | **334/1886** |
| SEM_DECISAO_horas_extras_100_noturna; SEM_DECISAO_horas_extras_50; SEM_DECISAO_horas_extras_50_noturna | 59/321 | 53/260 | 59/352 | 49/340 | 60/365 | 54/249 | **334/1887** |
| SEM_DECISAO_horas_noturnas | 59/312 | 52/254 | 59/335 | 49/332 | 61/356 | 55/242 | **335/1831** |
| SEM_RETORNO_FECHAMENTO | 6/0 | 6/0 | 1/0 | 5/0 | 7/0 | 3/0 | **28/0** |

Todos os valores da tabela anterior: `[nao verificado]`, origem `PRODUTO|2026-10|classe` nas partes 1–6.

| 11/2026 — classe(s) com valores iguais | P1 | P2 | P3 | P4 | P5 | P6 | Soma |
|---|---|---|---|---|---|---|---|
| DIA_FORA_UNIVERSO_PORTA; SEM_DECISAO_horas_trabalhadas; SEM_DECISAO_horas_intra_indenizada; SEM_DECISAO_horas_folga_trabalhada | 74/817 | 77/857 | 76/805 | 70/791 | 86/967 | 78/876 | **461/5113** |
| MOTOR_FECHAMENTO_horas_trabalhadas | 1/14 | 3/39 | 1/14 | 1/11 | — | 1/12 | **7/90** |
| RECUSA_PORTA_SEM_DONO | 21/329 | 16/269 | 17/272 | 16/310 | 12/239 | 15/223 | **97/1642** |
| SEM_DECISAO_horas_atraso; SEM_DECISAO_horas_saida_antecipada | 74/846 | 78/885 | 77/839 | 71/804 | 86/990 | 79/894 | **465/5258** |
| SEM_DECISAO_horas_extras_100; SEM_DECISAO_horas_extras_100_feriado; SEM_DECISAO_horas_extras_100_noturna; SEM_DECISAO_horas_extras_50; SEM_DECISAO_horas_extras_50_noturna | 74/844 | 77/881 | 76/832 | 70/800 | 86/990 | 78/891 | **461/5238** |
| SEM_DECISAO_horas_noturnas | 74/830 | 78/866 | 76/814 | 70/800 | 86/970 | 79/884 | **463/5164** |
| SEM_RETORNO_FECHAMENTO | — | 1/0 | 1/0 | 3/0 | 1/0 | 1/0 | **7/0** |

Todos os valores da tabela anterior: `[nao verificado]`, origem `PRODUTO|2026-11|classe` nas partes 1–6.

Os zeros em dias de `SEM_RETORNO_FECHAMENTO` vêm acompanhados de `DIAS_NAO_LOCALIZADOS|competencia|PRODUTO|SEM_RETORNO_FECHAMENTO|colabs=...`; portanto, não representam comprovação de ausência de dias afetados. `[nao verificado]`

| Parte | rc | INCOMPLETO | COLABS_COM_FALHA_INSTRUMENTO | TRATADA_LOCAL | EXCLUIDOS_TOTAL |
|---|---:|---|---:|---|---:|
| 1 | 0 | sem ocorrência no texto | 0 | sem ocorrência no texto | 46 |
| 2 | 0 | sem ocorrência no texto | 0 | sem ocorrência no texto | 48 |
| 3 | 0 | sem ocorrência no texto | 0 | sem ocorrência no texto | 35 |
| 4 | 0 | sem ocorrência no texto | 0 | sem ocorrência no texto | 37 |
| 5 | 0 | sem ocorrência no texto | 0 | sem ocorrência no texto | 31 |
| 6 | 0 | sem ocorrência no texto | 0 | sem ocorrência no texto | 34 |
| **Soma** | — | — | **0** | — | **231** |

Origem: rodapé de cada parte, linhas `rc_sombra=0`, `COLABS_COM_FALHA_INSTRUMENTO|0`, `EXCLUIDOS_TOTAL|N`; busca textual das ocorrências restantes. Todas as células são `[nao verificado]`. Não há linha de erro de instrumento no texto fornecido; ausência efetiva de erros e contagem de exceções tratadas localmente permanecem `[nao verificado]`. Os 231 excluídos somam **ocorrências por competência**, sem deduplicação entre outubro e novembro. `[nao verificado]`

## 06/11 17:05 · sessao 4407b2add58b · - · - · origem=Ronald (tmux)

- O DIFF v2 × v3 é medida de impacto por rubrica, com dono; não valida o motor novo. [CLAUDE.md:45](/home/ronald/codex_ws/320ef1c738b115e3f7e0004e4c51a48a6eb2d953/CLAUDE.md:45), [CLAUDE.md:415](/home/ronald/codex_ws/320ef1c738b115e3f7e0004e4c51a48a6eb2d953/CLAUDE.md:415).
- Exigem `!`: tocar o gravado de competência exportada e alterar `motor_calculo_v2.py`; a documentação do calculador declara o repontamento da troca como passo dependente de `!`. [CLAUDE.md:262](/home/ronald/codex_ws/320ef1c738b115e3f7e0004e4c51a48a6eb2d953/CLAUDE.md:262), [CLAUDE.md:403](/home/ronald/codex_ws/320ef1c738b115e3f7e0004e4c51a48a6eb2d953/CLAUDE.md:403), [periodo.py:8](/home/ronald/codex_ws/320ef1c738b115e3f7e0004e4c51a48a6eb2d953/app/ponto/calculador/periodo.py:8).
- Há dependências concretas do v2: constantes de tolerância importadas em `periodo.py`; classes/função noturna e chamada `he_noturna` em `regras.py`. Isso identifica dependência de código, não uma lista medida de `!` por colaborador. [periodo.py:163](/home/ronald/codex_ws/320ef1c738b115e3f7e0004e4c51a48a6eb2d953/app/ponto/calculador/periodo.py:163), [regras.py:35](/home/ronald/codex_ws/320ef1c738b115e3f7e0004e4c51a48a6eb2d953/app/ponto/calculador/regras.py:35), [regras.py:825](/home/ronald/codex_ws/320ef1c738b115e3f7e0004e4c51a48a6eb2d953/app/ponto/calculador/regras.py:825).
- As três lacunas `FORA_DA_MEDIDA` dos anexos correspondem ao `NAO_DECIDE` do código: `horas_falta` vem da ausência aprovada; `horas_reflexo_dsr` e `saldo_banco_horas` são ajustes mensais. [regras.py:429](/home/ronald/codex_ws/320ef1c738b115e3f7e0004e4c51a48a6eb2d953/app/ponto/calculador/regras.py:429).
- O juiz do dono retorna `CADASTRO`, `BATIDA` ou `ESTRUTURA`. `RESIDUO` nos anexos deve ser apresentado separadamente, preservando `ATE_LIMIAR` e `SEM_DECISAO`, sem atribuí-lo como quarto retorno desse juiz. [e6_oraculo.py:98](/home/ronald/codex_ws/320ef1c738b115e3f7e0004e4c51a48a6eb2d953/app/ponto/management/commands/e6_oraculo.py:98).
- A releitura declara que o alerta textual permanece contando a versão v2 quando o número v3 muda; é limite do código, não lacuna adicional explicitamente medida pelos anexos. [periodo.py:573](/home/ronald/codex_ws/320ef1c738b115e3f7e0004e4c51a48a6eb2d953/app/ponto/calculador/periodo.py:573).
- Os anexos fornecidos não identificam exportação das competências nem trazem lista explícita de `!`; não há fundamento para marcar colaboradores ou rubricas como exportados somente pelo delta. [nao verificado]
- Não encontrei `AGENTS.md` aplicável nem o script da sonda na cópia; `logs/` está ausente. [nao verificado]

## 06/11 15:24 · sessao 460f61b14993 · O10-C · verificador · origem=Code

**Laudo: a cura fecha o contraexemplo serial da volta 2. Não encontrei outra entrada estável, aceita nas duas rodadas, que volte a escrever banco ou `LogAuditoria`. Não identifiquei bloqueio novo para entregar o comando com o apply condicionado ao aval informado.** C:198–206; P:594–596,679–680. Execução e ocorrência em produção: **[não verificado]**.

Referências nesta cópia:

- **C** = [comando O10-C](/home/ronald/codex_ws/40019f43b68afe3e4270634d6f251eb3af9440f0/app/ponto/management/commands/trocas_plantao_lote_o10c.py).
- **P** = [porta da célula](/home/ronald/codex_ws/40019f43b68afe3e4270634d6f251eb3af9440f0/app/ponto/portas/celula.py).
- **T** = [testes O10-C](/home/ronald/codex_ws/40019f43b68afe3e4270634d6f251eb3af9440f0/app/ponto/tests/test_o10c_lote_trocas.py).
- **A** = [serviço de Ausência](/home/ronald/codex_ws/40019f43b68afe3e4270634d6f251eb3af9440f0/app/ponto/services/ausencia.py).
- **G** = [gerar_celulas](/home/ronald/codex_ws/40019f43b68afe3e4270634d6f251eb3af9440f0/app/ponto/management/commands/gerar_celulas.py).

**1. Cura e segunda rodada**

A guarda agora verifica `(colaborador_id, data)` dos dois lados de todos os pares e rejeita colisões antes de abrir snapshot ou aplicar qualquer par. Essas são exatamente as chaves usadas pela porta para editar as células. C:198–216; P:673–675.

Com cartas e cadastro estáveis, cada cedente declarado aplica uma vez, pares diferentes não disputam células e a segunda rodada encontra `origem`, `trabalha`, DNA e vínculo iguais. A porta retorna antes da escrita, da auditoria e dos efeitos posteriores. C:188–192,232–238; P:591–628,679–680; G:73–123.

A segunda rodada **continua criando e escrevendo outro JSONL**, inclusive registrando `igual`; isso é artefato operacional, distinto de nova escrita de negócio ou auditoria. C:209–228,157–164.

A conclusão não cobre concorrência irrestrita: a guarda global lê sem trava, enquanto o apply relê cartas sob trava por par. `data` é editável, mas apontar essa mutabilidade não demonstra sozinho uma colisão realizável: o editor também veta sobreposição. Não encontrei aí outro contraexemplo serial aceito nas duas rodadas. C:188–206,145–150; A:487–509.

Os testes acrescentados cobram rejeição com zero chamadas da porta e, para substituto lavrado, coluna e `!`. Execução: **[não verificado]**. T:219–229,292–301. [nao verificado]

**2. Quem escreve e quem lê `Ausencia.colaborador_id/data`**

| Papel | Caminho verificado |
|---|---|
| Definição | FK `colaborador` e campo `data`. `app/ponto/models.py:743–750`. |
| Criação | `criar_ausencia` recebe ambos, monta a instância e salva. A:130–134,341–343,385,417–418. |
| Chamadores de criação | Lançamento administrativo e API fornecem colaborador/data à porta. `app/ponto/views.py:2415–2421`; `app/api/views.py:1820–1826`. |
| Edição posterior de `data` | Tela lê o POST e chama `editar_ausencia_registro`; serviço atribui `data` e salva. `app/ponto/views.py:2773,2794–2802`; A:499–509. |
| Edição posterior de `colaborador` | Não identifiquei escritor de produção; inexistência absoluta: **[não verificado]**. O editor examinado não recebe colaborador. A:465–467. |
| Django Admin | Somente leitura. `app/ponto/admin.py:38–43`. |
| Leitor novo | Guarda de disjunção entre pares. C:198–206. |
| Outros leitores do comando | Classificação, localização/trava/foto da célula e apresentação. C:85–109,135–137,155–158,240–245. |
| Porta | Confere pessoas distintas, ordena/localiza as células e registra os lados na auditoria. P:662–675,682–690. |
| Cobertura e reação à Ausência | Juiz de cobertura lê colaborador/data; sinais usam os campos para rejulgar dias após save/delete. `app/ponto/turnos.py:2753–2769`; `app/ponto/signals.py:123–128,143–152`. |

O produtor efetivo das cartas específicas de produção não aparece no material raspado: **[não verificado]**.

**3. Os descartes escondem bloqueio da entrega?**

Não identifiquei bloqueio novo comprovado, mas dois argumentos precisam de alcance preciso:

- **(a) Deadlock:** comando e porta adquirem as cartas por PK e as células por colaborador/data, dentro da transação externa. Isso sustenta o descarte como defeito adicional de ordenação deste comando; não prova ausência geral de deadlock com outros escritores. C:144–160; P:652–675. Reprodução com gerador: **[não verificado]**.
- **(b) Célula inexistente:** um `IntegrityError` escapa e desfaz o atomic do par. Entretanto, isso não cobre toda criação concorrente: se outra criação confirmar entre a foto vazia e a leitura da porta, a porta pode encontrar uma célula existente e prosseguir. Nesse caso, `antes=None` descreve a foto, mas não necessariamente o antes efetivo da porta. C:132,144–165; P:524–527,593–601,694–699. Reprodução: **[não verificado]**.
- **(c) Reversão:** permanece incompleta: o reversor existente rejeita `editada`. É limitação declarada para o aval do apply, sem justificar abrir outro escritor nesta entrega. C:44–45; P:776–778; `CLAUDE.md:398–402`.
- **(d) Resultado:** stdout posterior testemunha a saída normal do atomic do par; o JSONL isolado continua sem confirmação durável de commit. Não trataria sua linha `trocada` como prova suficiente de confirmação. C:144,164–165,225–228,238–247. [nao verificado]
- **(e) Mutantes:** a inspeção estática dos testes não demonstra aquisição de trava, durabilidade do `fsync` ou importação da autoridade quando uma cópia local tem valores iguais. Isso limita a prova dos testes; não estabelece defeito funcional. C:76–81,155–156,228; T:153–180,338–348. [nao verificado]

**4. Escrita fora da porta, juiz duplicado e nome/CPF**

**Banco e auditoria:** não identifiquei escrita direta pelo comando; o ato de negócio é `trocar_plantao`. Há escrita explícita de diretório, JSONL e stdout. C:160–164,210–228,247–255.

**Juízes:** não identifiquei duplicação de juiz registrado. O comando consulta `ABERTAS`, lavra e vínculo pelas autoridades existentes; a nova guarda verifica conflito entre pares declarados, sem recalcular cobertura da Ausência. C:65–109,198–206; `app/core/juizes.py:241,244`; G:151–179.

**Nome/CPF:** a saída normal usa IDs e presença do documento, sem imprimir nome ou CPF cadastral. C:240–246. A garantia universal de anonimização, porém, não está demonstrada: erros ecoam argumentos brutos; o snapshot copia DNA integral; e a auditoria da porta inclui `documento.name`. C:119,132–137,187; P:528–531,682–690. Presença efetiva de nome/CPF nesses conteúdos: **[não verificado]**.

Suíte executada, interleavings concorrentes e conferência individual dos 16 registros: **[não verificado]**. [nao verificado]

## 06/11 15:22 · sessao f99a6f92b10b · - · - · origem=Ronald (tmux)

Mapeamento verificado de `Ausencia.colaborador_id/data`:

| Papel | Evidência |
|---|---|
| Definição dos campos | `app/ponto/models.py:743-750`: FK `colaborador`, `DateField data`. Unicidade é por `(colaborador,tipo,data)` entre não rejeitadas, não por célula: `app/ponto/models.py:826-830`. |
| Escritor de criação de produção | `criar_ausencia` recebe `colaborador/data` em `app/ponto/services/ausencia.py:130-134`, monta kwargs em `:341-343`, instancia `Ausencia` em `:385`, persiste em `:417-418`; o `save` do modelo termina em `app/ponto/models.py:908`. |
| Chamadores HTTP que fornecem ambos | Lançamento administrativo: `app/ponto/views.py:2415-2421`; PWA: `app/ponto/views.py:2536` (chamada localizada; detalhes não incluídos nesta leitura); API: `app/api/views.py:1820-1826`. |
| Outros produtores pela mesma porta | Materialização: `app/chamados/services/materializacao.py:484-490,520-526`; regularização: `app/ponto/services/regularizacao.py:118-122`; abandono: `app/ponto/management/commands/marcar_abandono_provisorio.py:76-84`; veredito: `app/ponto/services/veredito_celula.py:315-317`; férias: `app/ponto/services/ausencia.py:1202-1207`; copiloto: `app/ponto/services/checar_ausencia.py:43-48`. Esses caminhos são produtores genéricos; criação efetiva das cartas específicas do dado: **[não verificado]**. |
| Escritor posterior de `data` | A tela lê `POST data` em `app/ponto/views.py:2773` e chama `editar_ausencia_registro` em `:2794-2802`; o serviço atribui `ausencia.data=data` em `app/ponto/services/ausencia.py:499` e salva em `:509`. A própria documentação explicita que a tela não abre transação em `:512-514`. |
| Reatribuição posterior de `colaborador` | Não identifiquei escritor de produção; ausência absoluta de outros escritores: **[não verificado]**. O editor recebe a ausência, tipo e data, sem argumento colaborador: `app/ponto/services/ausencia.py:465-467`. |
| Django Admin | É somente leitura, não escritor: `app/ponto/admin.py:38-43`. |
| Escritor de testes | Fixture `_lanc` usa `Ausencia.objects.create(colaborador=colab,data=data,...)`: `app/ponto/tests/test_o10c_lote_trocas.py:54-57`. |

Leitores diretamente relevantes:

- **Delta:** monta a chave `(a.colaborador_id,a.data)` e detecta colisão entre pares: `app/ponto/management/commands/trocas_plantao_lote_o10c.py:198-206`.
- **Classificação:** usa ambos para lavra e vínculo; verifica colaborador igual e duração: `app/ponto/management/commands/trocas_plantao_lote_o10c.py:85-109`.
- **Foto e travas:** `_lado` registra ambos e busca a célula: `app/ponto/management/commands/trocas_plantao_lote_o10c.py:135-137`; aplicação relê cartas travadas em `:145-147` e trava células por ambos em `:155-158`.
- **Porta:** relê cartas em `app/ponto/portas/celula.py:657-659`, recusa colaborador igual em `:662-663`, usa os dois campos para ordenar e chamar `editar_dia` em `:673-675`, registra ambos em trilha em `:682-690`.
- **Apresentação:** stdout e lavra consultam ambos em `app/ponto/management/commands/trocas_plantao_lote_o10c.py:240-245`.
- **Juiz registrado de cobertura:** registro em `app/core/juizes.py:241`; implementação filtra colaborador/data e expande período em `app/ponto/turnos.py:2753-2769`. Essa decisão é cobertura de ausência, distinta da disjunção entre pares declarados.
- **Reação a save/delete:** sinais usam colaborador e data para rejulgar dias: `app/ponto/signals.py:123-128,143-152`.

**Atenção ao contraexemplo concorrente:** `data` pode mudar entre preflight sem trava (`C:188-206`) e releitura por par (`C:145-147`), mas o editor primeiro consulta sobreposição e recusa outra ausência (`app/ponto/services/ausencia.py:487-494`). Portanto, só apontar mutabilidade não prova que se consiga mover uma carta diretamente para célula já ocupada por outra carta viva. Esse interleaving concreto precisa de demonstração adicional.

## 06/11 15:22 · sessao 9d68a1e6ccf6 · - · - · origem=Ronald (tmux)

A cura fecha o contraexemplo serial anterior: reserva `(colaborador_id, data)` de ambos os lados de todos os pares e rejeita colisão antes de abrir snapshot ou chamar porta. A chave coincide com as células que a porta efetivamente edita. `app/ponto/management/commands/trocas_plantao_lote_o10c.py:198-216`; `app/ponto/portas/celula.py:635-637,673-675`.

Não encontrei outra entrada **estável**, aceita nas duas rodadas, que reescreva banco/trilha. Cada cedente declarado aplica uma vez; pares distintos ficam com células disjuntas; na segunda aplicação a porta compara `origem`, `trabalha`, DNA e vínculo e retorna `igual` antes de escrita, trilha e rabo. O DNA é produzido do vínculo/template, sem depender dos carimbos escritos pela própria troca. `app/ponto/management/commands/trocas_plantao_lote_o10c.py:188-192,232-238`; `app/ponto/portas/celula.py:591-596,609-628,679-680`; `app/ponto/management/commands/gerar_celulas.py:73-123`.

A nova guarda não é protegida contra edição concorrente das datas: lê antes das travas; o apply recarrega as cartas por par e usa as datas atuais sem repetir a disjunção global. A data é editável pela porta de Ausência. Isso permite divergência entre preflight e aplicação; contudo, se a sobreposição persistir, a rodada seguinte aborta na guarda, portanto **não é contraexemplo serial de reescrita na segunda rodada**. `app/ponto/management/commands/trocas_plantao_lote_o10c.py:188-206,145-156`; `app/ponto/services/ausencia.py:465-509`.

O teste novo exige `CommandError` e zero chamadas da porta para o contraexemplo anterior; as novas asserções do substituto lavrado cobram coluna e `!`. Execução dos testes: **[não verificado]**. `app/ponto/tests/test_o10c_lote_trocas.py:219-229,292-301`.

## 06/11 15:22 · sessao 450936a8afa5 · - · - · origem=Ronald (tmux)

- **Escritor:** o comando chama apenas `trocar_plantao` para alterar negócio; não contém `save/create/update/delete` de modelo. Cria diretório/JSONL e escreve stdout como artefatos operacionais. `app/ponto/management/commands/trocas_plantao_lote_o10c.py:160–162,210–228,243–255`. O teste AST cobre chamadas proibidas, mas sua execução nesta auditoria é **[não verificado]**. `app/ponto/tests/test_o10c_lote_trocas.py:338–348`.

- **Juízes:** usa `ABERTAS`, registrada como autoridade; vínculo e lavra vêm das funções existentes. A enumeração da lavra pergunta ao juiz `competencia_fechada_para_celula`. Não identifiquei duplicação de juiz nesses caminhos. `app/ponto/management/commands/trocas_plantao_lote_o10c.py:65–81,103–109`; `app/core/juizes.py:244`; `app/ponto/management/commands/gerar_celulas.py:151–179`. Critérios adicionais de seleção do lote são declarados separadamente no próprio comando. `app/ponto/management/commands/trocas_plantao_lote_o10c.py:17–23,77–102`.

- **PII:** a linha normal usa IDs, datas, estado e presença do documento. As frases atuais da porta também usam IDs/fontes fixas. `app/ponto/management/commands/trocas_plantao_lote_o10c.py:240–246`; `app/ponto/portas/celula.py:580–589,653–667`; `app/ponto/management/commands/gerar_celulas.py:201–219`. Entretanto, erros ecoam argumentos brutos `--par`/`--por`; o JSON copia o DNA integral, sem filtro. Esses caminhos não permitem afirmar anonimização universal; presença efetiva de nome/CPF é **[não verificado]**. `app/ponto/management/commands/trocas_plantao_lote_o10c.py:119,132–137,157–158,187,225–228`. A trilha da **porta** inclui `documento.name`, também sem filtro; presença de PII nesse nome é **[não verificado]**. `app/ponto/portas/celula.py:682–691`.

- **(a) Deadlock:** comando e porta travam cartas por PK e células por colaborador/data, na mesma transação externa. A trava antecipada não introduz uma ordem diferente daquela já usada pela porta; ocorrência com gerador é **[não verificado]**. `app/ponto/management/commands/trocas_plantao_lote_o10c.py:144–160`; `app/ponto/portas/celula.py:652–675`.

- **(b) Célula ausente:** `antes=None` representa a consulta vazia; criação concorrente pode causar `IntegrityError`, que escapa e desfaz o atomic do par. A unicidade é por colaborador/data. `app/ponto/management/commands/trocas_plantao_lote_o10c.py:132,144–165`; `app/ponto/portas/celula.py:524–527,599–601,694–699`; `app/escala/models.py:1833`. Uma criação concorrente que confirme entre foto e porta também pode fazer a foto divergir do estado posteriormente observado pela porta; reprodução é **[não verificado]**. `app/ponto/management/commands/trocas_plantao_lote_o10c.py:155–160`; `app/ponto/portas/celula.py:593–597`.

- **(c) Reversão:** o reversor existente rejeita `editada`; o comando declara a limitação. Isso mantém incompleto o procedimento de reversão, mas não demonstra novo defeito que, sozinho, impeça entregar código cujo apply espera aval específico; suficiência desse aval em produção é **[não verificado]**. `app/ponto/portas/celula.py:776–778`; `app/ponto/management/commands/trocas_plantao_lote_o10c.py:44–45`; `CLAUDE.md:398–402`.

- **(d) JSON antes do commit:** o resultado continua sendo escrito dentro do atomic. A linha normal de stdout vem depois do retorno de `aplicar_par`, portanto depois da saída desse atomic. O stdout é testemunho posterior; o JSON isolado continua sem marcador durável de commit. Interrupção concreta nessa janela é **[não verificado]**. `app/ponto/management/commands/trocas_plantao_lote_o10c.py:144,164–165,225–228,238–247`.

## 06/11 15:06 · sessao efb8fe82e623 · O10-C · verificador · origem=Code

**Laudo: as curas são parciais. Permanece um bloqueio de idempotência: cartas distintas podem representar as mesmas células em pares opostos. A foto também continua vulnerável quando a célula inexiste.** `app/ponto/models.py:826–831`; `C:93–101,120–124,154–157`; `P:593–601,673–675`.

Referências abreviadas nesta cópia:

- **C** = [comando O10-C](/home/ronald/codex_ws/fe9e8c19e5360e9658bd076d943845666a742b2a/app/ponto/management/commands/trocas_plantao_lote_o10c.py).
- **P** = [porta da célula](/home/ronald/codex_ws/fe9e8c19e5360e9658bd076d943845666a742b2a/app/ponto/portas/celula.py).
- **T** = [testes O10-C](/home/ronald/codex_ws/fe9e8c19e5360e9658bd076d943845666a742b2a/app/ponto/tests/test_o10c_lote_trocas.py).
- **G** = [comando gerar_celulas](/home/ronald/codex_ws/fe9e8c19e5360e9658bd076d943845666a742b2a/app/ponto/management/commands/gerar_celulas.py).

Execução da suíte, execução de mutantes, ancestralidade Git e ocorrência dos problemas em produção: **[não verificado]**. O material fornecido não permite conferir os 16 registros individualmente: **[não verificado]**. [nao verificado]

**1. Cada cura está correta e completa?**

| Cura | Verificação |
|---|---|
| `ABERTOS` → autoridade `ABERTAS` | **Correta.** Importa e consulta a constante registrada como autoridade. `C:75–80`; `app/ponto/services/ausencia.py:765`; `app/core/juizes.py:244`. |
| SOLO/qualquer classe lavrada → `!` | **Correta na implementação.** A apresentação consulta os dois lados independentemente da classe e alimenta `bang` pela lavra. `C:230–243`. |
| `--par` bijetivo | **Correta por identidade da carta; incompleta para garantir idempotência das células.** O conjunto `vistos` guarda PKs de Ausência, enquanto a escrita é por colaborador/data. `C:120–124`; `P:673–675`. Contraexemplo abaixo. |
| Trava antes da foto | **Corrige a janela para linhas existentes**, adquirindo as duas travas antes de `_lado`. **Não reserva célula ausente.** `C:128–136,143–157`; `P:524–527`. |
| Resultado no JSONL | **Registra o retorno da porta**, mas ainda dentro do atomic externo. Não constitui sozinho confirmação de commit: uma interrupção entre o `fsync` e a conclusão da transação deixa resultado no arquivo sem prova de confirmação no banco. `C:143,159–164,213–216`. | [nao verificado]
| Casa em `FORA_DE_PIPELINE` | **Correta para o contrato de casa.** Entrada declarada e reconhecida pelo selo existente. `app/config/crons.py:1346–1348`; `app/chamados/tests/test_contract_crons.py:220–236`. |
| Docstring de equivalência | **Corrigida quanto à distinção lote/porta:** agora declara critérios adicionais e admite `igual`/`recusada`. Permanece a limitação operacional de reversão explicitada no próprio texto. `C:17–23,39–45`. |

O furo da bijeção admite este contraexemplo estático, no mesmo dia aberto, com documentos e vínculos válidos:

| Carta | Colaborador | Tipo | Papel |
|---|---|---|---|
| A | X | `troca_de_plantao` | Cedente do primeiro par |
| B | Y | Outro tipo | Substituto do primeiro par |
| C | Y | `troca_de_plantao` | Cedente do segundo par |
| D | X | Outro tipo | Substituto do segundo par |

Essa configuração respeita a unicidade existente, que inclui **tipo**; o comando exige o tipo legado somente do cedente. Os quatro PKs diferentes passam em `_pares`. `app/ponto/models.py:826–831`; `C:76–77,93–101,120–124`.

Com `A:B` e `C:D`, o primeiro par deixa X folgando/Y trabalhando; o segundo faz o contrário nas **mesmas células**. Na rodada seguinte, ambos voltam a alterar estado e produzir trilha. Portanto, a promessa de idempotência do lote continua falsa para entradas aceitas. `C:220–226`; `P:594–609,673–691`. Execução desse contraexemplo: **[não verificado]**.

**2. Quem trava `CelulaDia`, em qual ordem e com quais riscos?**

| Caminho | Aquisição e ordem |
|---|---|
| O10-C | Ausências por PK crescente; depois células por `(colaborador_id,data)` crescente, antes da foto. `C:143–157`. |
| `trocar_plantao` | Ausências por PK crescente; chama `editar_dia` na ordem crescente de `(colaborador_id,data)`. `P:652–675`. |
| `editar_dia` isolado | `select_for_update` de uma célula. `P:592–593`. |
| `gerar_celulas` | **Não usa leitura travada.** Mantém atomic para todo o lote, percorre vínculos segundo sua ordenação padrão e dias crescentes; `create`/`save` são os pontos de escrita. `G:309–311`; `P:824–871`. |
| `regenerar_celulas_vinculo` | **Não usa leitura travada.** Um colaborador por chamada, dias crescentes dentro do atomic; escreve nos ramos de criação/regeneração. `P:218–221,245,259–277,281`. |
| Reversão/lavra/carimbos | Também escrevem células: reversão por `save`, lavra/carimbos por `salvar_e_emitir`, insumos por `bulk_update` sem ordenação própria. `P:785–795,899–902,1003,1020,1035,1067,1082,1094–1103`. |

**Há risco estático de deadlock com o gerador.** A ordenação dos vínculos é `-data_inicio`, não colaborador/data. Havendo escrita nas duas células, ele pode conservar B e esperar A, enquanto O10-C conserva A e espera B. A ausência de `select_for_update` no gerador não elimina as travas adquiridas pelas escritas. `app/escala/models.py:1003`; `G:309–311`; `P:824–839,849–871`; `C:154–155`. Reprodução e ocorrência real: **[não verificado]**.

A regeneração isolada segue datas crescentes de um único colaborador, mas **a leitura anterior à escrita permanece vulnerável**: pode carregar `origem='gerada'`, aguardar a troca e depois sobrescrever trabalha/DNA usando essa decisão antiga, mesmo após a troca tornar a célula humana. A trava nova em C não força esse escritor a reler a origem. `P:219,263–277`; `P:603–609`. Reprodução: **[não verificado]**.

Não identifiquei chamador de tela para `editar_dia` na cópia; os chamadores encontrados são comando → troca → edição. O próprio comando se declara primeiro chamador de produção da troca. Caminho “editar_dia vindo da tela”: **[não verificado]**. `C:4,159`; `P:675`.

**Célula inexistente:** o SELECT retorna vazio, a foto pode guardar `antes=None`, e a porta posteriormente tenta criar. Outro escritor pode criar entre esses momentos, tornando a foto diferente do antes efetivamente encontrado pela porta; uma corrida de criação também pode terminar em `IntegrityError`, pois existe unicidade por colaborador/data e o handler da troca só captura `_TrocaRecusada`. `C:128–131,154–159`; `P:524–527,593–601,694–699`; `app/escala/models.py:1833`. Ocorrência concreta: **[não verificado]**.

**3. Quem escreve e quem lê os elementos novos consumidos pelo delta?**

`ABERTAS` e `FORA_DE_PIPELINE` são definições de código; o JSONL é arquivo de execução. Não são três campos novos de banco. `app/ponto/services/ausencia.py:765`; `app/config/crons.py:769,1346–1348`; `C:198–216`.

| Elemento | Quem escreve/define | Quem lê |
|---|---|---|
| `ABERTAS` | Definição em `app/ponto/services/ausencia.py:765`. | Novo leitor O10-C: `C:75–80`. Registro da autoridade: `app/core/juizes.py:244`. Demais leitores identificados abaixo. |
| `FORA_DE_PIPELINE` | Dicionário e entrada O10-C em `app/config/crons.py:769,1346–1348`. | Contrato de casa e de entradas válidas: `app/chamados/tests/test_contract_crons.py:220–236`. O10-C não consulta essa entrada para executar; seu fluxo está em `C:176–250`. |
| Snapshot JSONL | `handle` cria arquivo exclusivo; `diario` serializa, faz `flush` e `fsync`. `aplicar_par` fornece foto e resultado. `C:156–163,198–216`. | Leitor automatizado encontrado: teste abre antes da porta e depois da execução. `T:161–180`. Reversor de produção que consuma esse formato: **[não verificado]**; o reversor existente consulta a célula/DNA, não esse arquivo. `P:776–795`. | [nao verificado]

Leitores efetivos de `ABERTAS` identificados além do comando:

| Uso | Referência |
|---|---|
| Rejulgar tipo, decidir/tratar ausência | `app/ponto/services/ausencia.py:541,692,716` |
| Propriedade do modelo e decisão do dia | `app/ponto/models.py:719–720`; `app/ponto/services/dia_decidido.py:447–448` |
| Filtros, contadores e atos da tela de ausências | `app/ponto/views.py:2187,2195,2206,2282,2316,2606,2618,2687,2691` |
| Espelho | `app/ponto/services/espelho.py:878–880` |
| API: pendência e recebimento de documento | `app/api/views.py:1582–1586,3101–3103` |
| Materialização, silêncio e vigia | `app/chamados/services/materializacao.py:562–564`; `app/chamados/services/silencio_ausencia.py:57,77`; `app/chamados/services/vigia_fio.py:298,309` |
| Auditoria e reabertura | `app/chamados/management/commands/auditar_invariantes_chamados.py:634,643`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:27,36` |
| Teste de estados | `app/ponto/tests/test_a4_estados_com_porta.py:286–289` |

**4. Algum descarte esconde bloqueio?**

- **(a) `on_commit` mockado:** não demonstra isoladamente defeito funcional do comando, mas continua sendo limitação da prova. O molde executa imediatamente callbacks que, no caminho real, são registrados para depois do commit. Herdar o molde não verifica adiamento ou descarte no rollback. `T:59–63`; `app/ponto/tests/test_o10b_editar_dia_troca.py:39–41`; `P:493–502`; `app/ponto/services/fechamento.py:1782–1808`.

- **(b) Reversão fiel:** **manteria como bloqueio operacional do apply, não como exigência de abrir escritor fora deste aval.** Documentar `INCOMPLETO` é correto, mas não demonstra reversibilidade: a troca produz `editada`, o reversor aceita apenas `gerada` com DNA anterior, e repetir `editar_dia` remonta DNA e renova carimbos. A regra exige arquivo de reversão anterior ao apply; a suficiência operacional deste arquivo continua sem demonstração. `C:43–44`; `P:591,598–609,776–778`; `CLAUDE.md:398–400`. Exceção autorizando aplicar nessa condição: **[não verificado]**.

- **(c) Novo snapshot na segunda rodada:** descarte válido quanto à distinção arquivo/estado de negócio. Contudo, **não salva a idempotência do lote**, pelo contraexemplo de células coincidentes acima. `C:198–216`; `P:594–596`; `C:120–124`; `app/ponto/models.py:826–831`.

- **(d) TOCTOU de lavra/vínculo:** reclassificar e reconferir reduz a janela, mas não a elimina; a porta ainda consulta guarda/vínculo/DNA antes de adquirir a trava. O backlog documenta expressamente a guarda anterior ao atomic. É risco herdado identificado, não cura completa. `C:149–155`; `P:567–593`; `app/docs/BACKLOG.md:1790`. Violação efetiva da exportada nesta execução: **[não verificado]**.

- **(e) Atomic por par:** descarte coerente com o desenho declarado; não encontrei promessa de atomicidade do lote inteiro. Cada chamada tem seu atomic, dentro do laço de aplicação. `C:39,143,220–226`.

**5. Os testes mordem os mutantes das curas?** [nao verificado]

A avaliação abaixo é estática; execução de mutação: **[não verificado]**.

| Mutante | Mordida identificada |
|---|---|
| Voltar a aceitar `A:B` + `B:A` ou `A:A` | **Sim:** exige erro e zero chamadas da porta. `T:278–287`. |
| Voltar a emitir `!` somente para classe `lavrada` | **Sim:** exige `!` para SOLO e pedido. `T:126–135`. |
| Omitir resultado do snapshot | **Sim, no sucesso:** exige `[None,'trocada']`. `T:176–180`. |
| Aceitar substituto rejeitado | **Sim:** exige classe específica e zero chamadas. `T:289–296`. |
| Aceitar três componentes no par | **Sim:** exige erro. `T:275–276`. |
| Acrescentar chamadas diretas `execute/raw/cursor` | **Sim, pela blacklist AST.** `T:329–334`. |
| **Retirar, atrasar ou inverter as travas novas** | **Não há mordida demonstrada:** foto e costura são seriais e não verificam aquisição/ordem das travas. `T:153–180,310–334`; `C:154–155`. |
| Voltar a copiar localmente `ABERTAS` | **Não há mordida demonstrada:** mesmos valores mantêm os resultados; também existe fallback para qualquer status diferente de aprovada. `C:80–83`; `app/ponto/services/ausencia.py:765`; `app/core/juizes.py:1406–1420`. |
| Tirar `fsync`, mantendo `flush` | **Não há mordida demonstrada:** ler o arquivo não testa durabilidade. `T:162–180`; `C:214–216`. |
| Ignorar lavra exclusivamente do substituto na coluna/`!` | **Não há mordida demonstrada:** o teste específico exige somente a classe. `T:219–226`; `C:230–237`. | [nao verificado]
| Aceitar cartas distintas que disputam as mesmas células | **Não há mordida demonstrada:** o teste novo cobre repetição de PKs; o de idempotência usa um único par. `T:228–241,278–287`. | [nao verificado]
| Remover a casa em `FORA_DE_PIPELINE` | **Sim pelo selo existente**, não pelos testes novos O10-C. `app/chamados/tests/test_contract_crons.py:214–228`. |
| Reintroduzir docstring equivocada | **Não há mordida demonstrada:** o teste compara dois casos de comportamento, sem validar a declaração geral. `T:336–349`. | [nao verificado]

## 06/11 15:05 · sessao 2ae8f65e87f3 · - · - · origem=Ronald (tmux)

- **Ordem O10-C/porta:** Ausências por PK crescente, depois células por `(colaborador_id, data)`. `app/ponto/management/commands/trocas_plantao_lote_o10c.py:143–155`; `app/ponto/portas/celula.py:652–675`.
- **Travas explícitas de CelulaDia encontradas:** comando em `trocas_plantao_lote_o10c.py:155` e `editar_dia` em `celula.py:593`. `trocar_plantao` chega à trava pelo `editar_dia`. `celula.py:673–675`.
- **Gerador pode inverter ordem entre colaboradores:** seu comando conserva um atomic para todo o lote; vínculos não recebem ordenação global por colaborador e seguem `EscalaColaborador.Meta.ordering = ["-data_inicio"]`; cada vínculo escreve seus dias em ordem crescente. Portanto, é possível o gerador conservar B e esperar A enquanto O10-C conserva A e espera B. É ciclo possível pela leitura do código; reprodução e ocorrência real **[não verificado]**. `app/ponto/management/commands/gerar_celulas.py:309–311`; `app/ponto/portas/celula.py:824–839,849–871`; `app/escala/models.py:1003`.
- **Regeneração isolada:** trabalha em um colaborador, datas crescentes; nessa operação não há inversão da ordem de células. Entretanto, não faz leitura travada: a origem é lida antes de quaisquer saves. Um escritor pode ler `gerada`, esperar O10-C e depois sobrescrever trabalha/DNA já tornados humanos. A trava adicionada em C não cura esse escritor concorrente. `app/ponto/portas/celula.py:218–221,245,257–277`. Reprodução **[não verificado]**.
- **Outros escritores adquirem travas implícitas:** reversão (`celula.py:785–795`); `salvar_e_emitir`, usado por lavra/carimbos (`celula.py:899–900,1003,1020,1035,1067,1082`); backfill de insumos com `bulk_update`, sem ordenação própria (`celula.py:1094–1103`).
- **Célula inexistente:** o SELECT de C não reserva sua ausência; snapshot pode registrar `None`, e a porta pode encontrar uma célula criada depois ou competir no `create`. Há restrição única por colaborador/data; `IntegrityError` não é convertido em `recusada` porque o handler cobre somente `_TrocaRecusada`. `trocas_plantao_lote_o10c.py:128–131,155–159`; `celula.py:524–526,593,599–601,694`; `app/escala/models.py:1833`. Concorrência real **[não verificado]**.
- **Tela:** não identifiquei chamador de produção em views/admin na cópia. Chamadas presentes: comando → troca → editar. A declaração do próprio comando é “1º chamador de produção”. Caminho “editar_dia vindo da tela”: **[não verificado]**. `trocas_plantao_lote_o10c.py:4,159`; `celula.py:675`.
- **Ressalva de escopo das travas:** os SELECTs de células não usam `of=('self',)` nem limpam o ordering, enquanto os modelos ordenam por colaborador/nome. Possível JOIN e trava adicional de Colaborador: SQL efetivo **[não verificado]**; evitar afirmar que travam exclusivamente CelulaDia. `trocas_plantao_lote_o10c.py:155`; `celula.py:593`; `app/escala/models.py:1834`; `app/colaboradores/models.py:403`.

## 06/11 15:04 · sessao b4d294fee06c · - · - · origem=Ronald (tmux)

Referências: **C** = `app/ponto/management/commands/trocas_plantao_lote_o10c.py`; **T** = `app/ponto/tests/test_o10c_lote_trocas.py`; **P** = `app/ponto/portas/celula.py`.

**Furo bloqueante na cura da bijeção:** ela elimina repetição de **Ausencia.pk**, mas não colisão de **célula**. A unicidade viva de Ausencia inclui `tipo`, permitindo duas cartas de tipos distintos para o mesmo colaborador/dia. O substituto não precisa ter `tipo='troca_de_plantao'`. Assim, quatro PKs distintos podem representar `A(X,troca):B(Y,outro_tipo)` e `C(Y,troca):D(X,outro_tipo)`, no mesmo dia. Os dois pares passam no parser e fazem as mesmas células alternarem, gerando novas escritas em toda rodada. Sustentação: `C:76`, `C:93-101`, `C:120-124`; `app/ponto/models.py:826-831`; `P:594-596`, `P:673-675`. O teste novo cobre IDs repetidos, invertidos e autopar; não cobre células coincidentes com cartas distintas. `T:278-287`. Contraexemplo executado: **[não verificado]**.

Cobertura por leitura estática; execução e mutação real: **[não verificado]**.

| Mutante | Mordida identificada |
|---|---|
| Voltar a aceitar `A:B` + `B:A` ou `A:A` | **Sim**: exige `CommandError` e zero chamadas da porta. `T:278-287`. |
| Voltar a enviar a `!` somente `classe='lavrada'` | **Sim**: SOLO e pedido continuam nessas classes e precisam aparecer em `!`. `T:126-135`. |
| Omitir linha de resultado do JSONL | **Sim** para sucesso: exige `[None,'trocada']`. `T:176-180`. |
| Aceitar substituto rejeitado | **Sim**: classe específica e zero chamadas. `T:289-296`. |
| Aceitar três componentes em `--par` | **Sim**: exige erro. `T:275-276`. |
| Acrescentar chamadas diretas `execute`, `raw` ou `cursor` | **Sim** na blacklist AST. `T:329-334`. |
| Tirar as travas novas, movê-las para depois da foto ou inverter sua ordem | **Não demonstrado**: teste da foto executa serialmente e só confere conteúdo; costura não inspeciona locks. `T:153-180`, `T:310-334`; trava que seria removida: `C:154-155`. | [nao verificado]
| Voltar a copiar localmente os valores de `ABERTAS` | **Não demonstrado**: valores continuariam iguais, há fallback para status diferente de aprovada, e selo de ausência não proíbe essa tupla. `C:80-83`; `app/ponto/services/ausencia.py:765`; `app/core/juizes.py:1406-1420`. |
| Tirar `fsync`, mantendo `flush` | **Não demonstrado**: teste apenas abre/lê arquivo, sem exigir durabilidade. `C:214-216`; `T:162-180`. | [nao verificado]
| Omitir DNA, autor, hora ou geradora da foto | **Não demonstrado**: asserções conferem par, `trabalha`, origem e resultado. `T:171-180`. |
| Ignorar lavra exclusivamente do substituto na coluna/`!` | **Não demonstrado**: teste específico exige somente classe, mantendo-se possível a classificação correta com apresentação errada. `T:219-226`; apresentação: `C:230-237`. | [nao verificado]
| Enviar a `!` somente classes `so_um_lado`, `pedido` e `lavrada` | **Não demonstrado**: fixtures lavradas não cobrem rejeitada, período, sem carta etc. `T:119-135`, `T:204-226`. |
| Remover entrada de `FORA_DE_PIPELINE` | **Sim pelo selo existente**, não pelos testes novos: varre comandos e exige casa. `app/chamados/tests/test_contract_crons.py:214-228`; entrada: `app/config/crons.py:1346-1348`. |
| Voltar à docstring equivocada | **Não demonstrado**: teste verifica resultados de dois casos, não texto ou equivalência geral. `T:336-349`. | [nao verificado]

**Resultado JSONL não prova commit:** ele é escrito com `fsync` dentro do atomic externo e antes da saída desse contexto. O teste chama isso de “confirmado”, mas só verifica o retorno da porta. `C:143`, `C:159-164`, `C:213-216`; `T:176-180`. Portanto a linha distingue resultado da porta; confirmação durável da transação pelo arquivo: **[não verificado]**. [nao verificado]

**Sobre os descartes:** herdar o molde não transforma o teste em prova de `on_commit` real: `_run` executa callbacks imediatamente, e o molde faz exatamente isso. `T:59-63`; `app/ponto/tests/test_o10b_editar_dia_troca.py:39-41`. Isso sustenta uma limitação da prova, sem demonstrar isoladamente defeito funcional do comando. `P:493-502`.

**Reversão permanece pendência material:** declarar ausência da porta não satisfaz por si só o requisito existente de arquivo de reversão anterior ao apply. A troca produz origem `editada`, enquanto o reversor disponível aceita somente `gerada` com DNA anterior. `CLAUDE.md:398-400`; `C:43-44`; `P:600-609`, `P:776-778`. Aceitação dessa incompletude como exceção operacional: **[não verificado]**.

## 06/11 14:58 · sessao 5caca5add164 · V3-IMPACTO-COM-DONO · sonda · origem=Code

No dado fornecido, todas as linhas `SOMA` apresentam `True`; não há marcador `INCOMPLETO`. [nao verificado]

Transcrição integral da saída fornecida, sem verificação em arquivo: [nao verificado]

```text
sonda_leitura: script.py [ba072165f8ac8a44] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE|6|6
FINALIDADE|IMPACTO_NUNCA_VALIDACAO
BASE_SOMAS|COLABORADORES_MEDIDOS
COBERTURA_SOMA|MEDIDOS_MAIS_EXCLUIDOS_IGUAL_UNIVERSO_PARTE
ATO|hoje=2026-11-06|agora=2026-11-06T14:49:54.974180-03:00
FORA_DA_MEDIDA|horas_falta
FORA_DA_MEDIDA|horas_reflexo_dsr
FORA_DA_MEDIDA|saldo_banco_horas
UNIVERSO|2026-10|colabs_porta=624
UNIVERSO_PARTE 2026-10 100
MEDIDO|2026-10|col29
RECUSA|2026-10|col41|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=25
EXCLUIDO|2026-10|col41|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col53|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col53|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col59
RECUSA|2026-10|col83|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col83|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col89
MEDIDO|2026-10|col95
MEDIDO|2026-10|col101
MEDIDO|2026-10|col107
MEDIDO|2026-10|col113
MEDIDO|2026-10|col119
MEDIDO|2026-10|col125
MEDIDO|2026-10|col131
MEDIDO|2026-10|col137
HISTORICO_T|2026-10|col143|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col143|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col149
MEDIDO|2026-10|col155
MEDIDO|2026-10|col161
MEDIDO|2026-10|col173
MEDIDO|2026-10|col191
DIA|2026-10|horas_extras_100_noturna|col191|2026-10-12|CADASTRO|0|1|1
MEDIDO|2026-10|col197
MEDIDO|2026-10|col203
RECUSA|2026-10|col209|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=15
EXCLUIDO|2026-10|col209|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col215
MEDIDO|2026-10|col221
MEDIDO|2026-10|col227
MEDIDO|2026-10|col239
EXCLUIDO|2026-10|col245|PRODUTO:MOTOR_FECHAMENTO_horas_trabalhadas
MEDIDO|2026-10|col251
RECUSA|2026-10|col257|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=21
EXCLUIDO|2026-10|col257|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col263
MEDIDO|2026-10|col269
MEDIDO|2026-10|col281
MEDIDO|2026-10|col287
RECUSA|2026-10|col317|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=25
EXCLUIDO|2026-10|col317|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col335
MEDIDO|2026-10|col341
MEDIDO|2026-10|col347
MEDIDO|2026-10|col365
MEDIDO|2026-10|col371
MEDIDO|2026-10|col389
MEDIDO|2026-10|col407
MEDIDO|2026-10|col425
HISTORICO_T|2026-10|col437|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col437|PRODUTO:SEM_RETORNO_FECHAMENTO
RECUSA|2026-10|col443|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=1
EXCLUIDO|2026-10|col443|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col455
MEDIDO|2026-10|col467
MEDIDO|2026-10|col473
MEDIDO|2026-10|col491
MEDIDO|2026-10|col503
MEDIDO|2026-10|col515
MEDIDO|2026-10|col521
MEDIDO|2026-10|col551
MEDIDO|2026-10|col557
MEDIDO|2026-10|col569
MEDIDO|2026-10|col575
MEDIDO|2026-10|col587
MEDIDO|2026-10|col599
DIA|2026-10|horas_trabalhadas|col599|2026-10-20|CADASTRO|10.100982500000001|5.5729172222222223|-4.5280652777777783
DIA|2026-10|horas_intra_indenizada|col599|2026-09-22|CADASTRO|1|0|-1
DIA|2026-10|horas_intra_indenizada|col599|2026-09-24|CADASTRO|1|0|-1
DIA|2026-10|horas_intra_indenizada|col599|2026-09-26|CADASTRO|1|0|-1
DIA|2026-10|horas_intra_indenizada|col599|2026-09-28|CADASTRO|1|0|-1
DIA|2026-10|horas_intra_indenizada|col599|2026-09-30|CADASTRO|1|0|-1
DIA|2026-10|horas_intra_indenizada|col599|2026-10-02|CADASTRO|1|0|-1
DIA|2026-10|horas_intra_indenizada|col599|2026-10-06|CADASTRO|1|0|-1
DIA|2026-10|horas_extras_50|col599|2026-10-16|CADASTRO|0|0.22415421249999856|0.22415421249999856
MEDIDO|2026-10|col617
RECUSA|2026-10|col641|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col641|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col647|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col647|PRODUTO:RECUSA_PORTA_SEM_DONO
HISTORICO_T|2026-10|col659|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col659|PRODUTO:SEM_RETORNO_FECHAMENTO
RECUSA|2026-10|col677|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col677|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col701
MEDIDO|2026-10|col707
MEDIDO|2026-10|col719
MEDIDO|2026-10|col737
MEDIDO|2026-10|col749
MEDIDO|2026-10|col755
MEDIDO|2026-10|col761
MEDIDO|2026-10|col821
MEDIDO|2026-10|col827
MEDIDO|2026-10|col833
MEDIDO|2026-10|col839
MEDIDO|2026-10|col845
MEDIDO|2026-10|col857
MEDIDO|2026-10|col863
MEDIDO|2026-10|col869
MEDIDO|2026-10|col881
MEDIDO|2026-10|col887
MEDIDO|2026-10|col893
MEDIDO|2026-10|col899
MEDIDO|2026-10|col905
MEDIDO|2026-10|col911
MEDIDO|2026-10|col917
MEDIDO|2026-10|col923
MEDIDO|2026-10|col929
MEDIDO|2026-10|col935
MEDIDO|2026-10|col941
DIA|2026-10|horas_intra_indenizada|col941|2026-10-01|CADASTRO|1|0|-1
MEDIDO|2026-10|col947
MEDIDO|2026-10|col953
MEDIDO|2026-10|col959
MEDIDO|2026-10|col965
MEDIDO|2026-10|col971
MEDIDO|2026-10|col977
MEDIDO|2026-10|col983
RECUSA|2026-10|col989|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=4
EXCLUIDO|2026-10|col989|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col995
RECUSA|2026-10|col1001|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col1001|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col1007|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col1007|PRODUTO:RECUSA_PORTA_SEM_DONO
COBERTURA_COLAB|2026-10|medidos=83|universo_parte=100|excluidos=17
COBERTURA_INTEGRAL|2026-10|False
PRODUTO|2026-10|DIA_FORA_UNIVERSO_PORTA|52|229
PRODUTO|2026-10|MOTOR_FECHAMENTO_horas_trabalhadas|1|24
PRODUTO|2026-10|RECUSA_PORTA_SEM_DONO|13|181
PRODUTO|2026-10|SEM_DECISAO_horas_atraso|55|258
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100|54|249
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100_feriado|54|249
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100_noturna|54|249
PRODUTO|2026-10|SEM_DECISAO_horas_extras_50|54|249
PRODUTO|2026-10|SEM_DECISAO_horas_extras_50_noturna|54|249
PRODUTO|2026-10|SEM_DECISAO_horas_folga_trabalhada|52|229
PRODUTO|2026-10|SEM_DECISAO_horas_intra_indenizada|52|229
PRODUTO|2026-10|SEM_DECISAO_horas_noturnas|55|242
PRODUTO|2026-10|SEM_DECISAO_horas_saida_antecipada|55|258
PRODUTO|2026-10|SEM_DECISAO_horas_trabalhadas|52|229
PRODUTO|2026-10|SEM_RETORNO_FECHAMENTO|3|0
DIAS_NAO_LOCALIZADOS|2026-10|PRODUTO|SEM_RETORNO_FECHAMENTO|colabs=3
DONO|2026-10|horas_trabalhadas|CADASTRO|10.100982500000001|5.5729172222222223|-4.5280652777777783|1|1
RESIDUO|2026-10|horas_trabalhadas|ATE_LIMIAR|11949.778875040278|11949.778875040278|2.4424906541753444e-14|83|1271
RESIDUO|2026-10|horas_trabalhadas|SEM_DECISAO|55.446929781111109|55.446929781111109|0|52|229
PARCIAL|2026-10|horas_trabalhadas|v2_h=11959.879857540278|v3_h=11955.3517922625|autoridade2_h=12015.326787321388|autoridade3_h=12010.798722043612|porta_h=11955.3517922625|emitido_decidido_h=11955.3517922625|fm_dias=12015.36|fm_retorno=12015.36|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_trabalhadas|11959.879857540278|11955.3517922625|-4.5280652777782961|1|1
INSTRUMENTO_HISTORICO|2026-10|horas_trabalhadas|v2_h=11959.351114151112|v3_h=11955.3517922625|delta_h=-3.9993218886120303|colabs=83
DONO|2026-10|horas_intra_indenizada|CADASTRO|8|0|-8|2|8
RESIDUO|2026-10|horas_intra_indenizada|ATE_LIMIAR|437.77514605361114|437.77514605361114|0|83|1264
RESIDUO|2026-10|horas_intra_indenizada|SEM_DECISAO|0|0|0|52|229
PARCIAL|2026-10|horas_intra_indenizada|v2_h=445.77514605361114|v3_h=437.77514605361114|autoridade2_h=445.77514605361114|autoridade3_h=437.77514605361114|porta_h=437.77514605361114|emitido_decidido_h=437.77514605361114|fm_dias=445.77|fm_retorno=445.77|colabs_div=2|dias_colab_div=8
RUBRICA|2026-10|horas_intra_indenizada|445.77514605361114|437.77514605361114|-8|2|8
INSTRUMENTO_HISTORICO|2026-10|horas_intra_indenizada|v2_h=445.77514605361114|v3_h=437.77514605361114|delta_h=-8|colabs=83
DONO|2026-10|horas_extras_50|CADASTRO|0|0.22415421249999856|0.22415421249999856|1|1
RESIDUO|2026-10|horas_extras_50|ATE_LIMIAR|50.156459266388886|50.156459266388886|2.886579864025407e-15|83|1251
RESIDUO|2026-10|horas_extras_50|SEM_DECISAO|0|0|0|54|249
PARCIAL|2026-10|horas_extras_50|v2_h=50.156459266388886|v3_h=50.38061347888889|autoridade2_h=50.156459266388886|autoridade3_h=50.38061347888889|porta_h=50.38061347888889|emitido_decidido_h=50.38061347888889|fm_dias=50.15|fm_retorno=50.15|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_extras_50|50.156459266388886|50.38061347888889|0.22415421250000378|1|1
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50|v2_h=50.156459266388886|v3_h=50.380613478888897|delta_h=0.22415421250001089|colabs=83
RESIDUO|2026-10|horas_extras_100|ATE_LIMIAR|78.35403251527778|78.35403251527778|5.6976992568458229e-15|83|1252
RESIDUO|2026-10|horas_extras_100|SEM_DECISAO|3.430878888888889|3.430878888888889|0|54|249
PARCIAL|2026-10|horas_extras_100|v2_h=78.35403251527778|v3_h=78.35403251527778|autoridade2_h=81.784911404166664|autoridade3_h=81.784911404166664|porta_h=78.35403251527778|emitido_decidido_h=78.35403251527778|fm_dias=81.80|fm_retorno=81.80|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_100|78.35403251527778|78.35403251527778|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100|v2_h=81.784911404166664|v3_h=88.786138348611118|delta_h=7.0012269444444541|colabs=83
RESIDUO|2026-10|horas_extras_100_feriado|ATE_LIMIAR|75.457405663888892|75.457405663888892|0|83|1252
RESIDUO|2026-10|horas_extras_100_feriado|SEM_DECISAO|3.430878888888889|3.430878888888889|0|54|249
PARCIAL|2026-10|horas_extras_100_feriado|v2_h=75.457405663888892|v3_h=75.457405663888892|autoridade2_h=78.888284552777776|autoridade3_h=78.888284552777776|porta_h=75.457405663888892|emitido_decidido_h=75.457405663888892|fm_dias=78.90|fm_retorno=78.90|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_100_feriado|75.457405663888892|75.457405663888892|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_feriado|v2_h=0|v3_h=85.88951149722223|delta_h=85.88951149722223|colabs=83
RESIDUO|2026-10|horas_extras_50_noturna|ATE_LIMIAR|0|0|0|83|1252
RESIDUO|2026-10|horas_extras_50_noturna|SEM_DECISAO|0|0|0|54|249
PARCIAL|2026-10|horas_extras_50_noturna|v2_h=0|v3_h=0|autoridade2_h=0|autoridade3_h=0|porta_h=0|emitido_decidido_h=0|fm_dias=0.0|fm_retorno=0.0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_50_noturna|0|0|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50_noturna|v2_h=0|v3_h=0|delta_h=0|colabs=83
DONO|2026-10|horas_extras_100_noturna|CADASTRO|0|1|1|1|1
RESIDUO|2026-10|horas_extras_100_noturna|ATE_LIMIAR|9.6132297222222221|9.6132297222222221|0|83|1251
RESIDUO|2026-10|horas_extras_100_noturna|SEM_DECISAO|0|0|0|54|249
PARCIAL|2026-10|horas_extras_100_noturna|v2_h=9.6132297222222221|v3_h=10.613229722222222|autoridade2_h=9.6132297222222221|autoridade3_h=10.613229722222222|porta_h=10.613229722222222|emitido_decidido_h=10.613229722222222|fm_dias=9.62|fm_retorno=9.62|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_extras_100_noturna|9.6132297222222221|10.613229722222222|1|1|1
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_noturna|v2_h=0|v3_h=10.613229722222222|delta_h=10.613229722222222|colabs=83
RESIDUO|2026-10|horas_folga_trabalhada|ATE_LIMIAR|0|0|0|83|1272
RESIDUO|2026-10|horas_folga_trabalhada|SEM_DECISAO|0|0|0|52|229
PARCIAL|2026-10|horas_folga_trabalhada|v2_h=0|v3_h=0|autoridade2_h=0|autoridade3_h=0|porta_h=0|emitido_decidido_h=0|fm_dias=0.0|fm_retorno=0.0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_folga_trabalhada|0|0|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_folga_trabalhada|v2_h=55.975673170277773|v3_h=0|delta_h=-55.975673170277773|colabs=83
RESIDUO|2026-10|horas_noturnas|ATE_LIMIAR|2888.7313102103176|2888.7313102103176|-5.3290705182007514e-15|83|1259
RESIDUO|2026-10|horas_noturnas|SEM_DECISAO|0|0|0|55|242
PARCIAL|2026-10|horas_noturnas|v2_h=2888.7313102103176|v3_h=2888.7313102103176|autoridade2_h=2888.7313102103176|autoridade3_h=2888.7313102103176|porta_h=2888.7313102103176|emitido_decidido_h=2888.7313102103176|fm_dias=2888.72|fm_retorno=2888.72|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_noturnas|2888.7313102103176|2888.7313102103176|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_noturnas|v2_h=2884.2627212785715|v3_h=2896.503724575397|delta_h=12.241003296825511|colabs=83
RESIDUO|2026-10|horas_atraso|ATE_LIMIAR|49.063499999999998|49.063125908055554|-0.00037409194444509852|82|1243
RESIDUO|2026-10|horas_atraso|SEM_DECISAO|0|0|0|55|258
PARCIAL|2026-10|horas_atraso|v2_h=49.063499999999998|v3_h=49.063125908055554|autoridade2_h=49.063499999999998|autoridade3_h=49.063125908055554|porta_h=49.063125908055554|emitido_decidido_h=49.063125908055554|fm_dias=49.06|fm_retorno=49.06|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_atraso|49.063499999999998|49.063125908055554|-0.00037409194444393279|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_atraso|v2_h=48.034666666666666|v3_h=48.034459241388888|delta_h=-0.00020742527777883879|colabs=83
RESIDUO|2026-10|horas_saida_antecipada|ATE_LIMIAR|56.371499999999997|56.371529152222223|2.9152222222106339e-05|82|1243
RESIDUO|2026-10|horas_saida_antecipada|SEM_DECISAO|0|0|0|55|258
PARCIAL|2026-10|horas_saida_antecipada|v2_h=56.371499999999997|v3_h=56.371529152222223|autoridade2_h=56.371499999999997|autoridade3_h=56.371529152222223|porta_h=56.371529152222223|emitido_decidido_h=56.371529152222223|fm_dias=56.37|fm_retorno=56.37|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_saida_antecipada|56.371499999999997|56.371529152222223|2.915222222554803e-05|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_saida_antecipada|v2_h=52.894000000000005|v3_h=52.89411109666667|delta_h=0.00011109666666480308|colabs=83
UNIVERSO|2026-11|colabs_porta=578
UNIVERSO_PARTE 2026-11 96
MEDIDO|2026-11|col29
RECUSA|2026-11|col41|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=25
EXCLUIDO|2026-11|col41|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-11|col53|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=23
EXCLUIDO|2026-11|col53|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col59
RECUSA|2026-11|col83|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=16
EXCLUIDO|2026-11|col83|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col89
MEDIDO|2026-11|col95
MEDIDO|2026-11|col101
MEDIDO|2026-11|col107
MEDIDO|2026-11|col113
RECUSA|2026-11|col119|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=22
EXCLUIDO|2026-11|col119|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col125
MEDIDO|2026-11|col131
MEDIDO|2026-11|col137
MEDIDO|2026-11|col149
MEDIDO|2026-11|col155
MEDIDO|2026-11|col161
MEDIDO|2026-11|col173
MEDIDO|2026-11|col191
MEDIDO|2026-11|col197
MEDIDO|2026-11|col203
RECUSA|2026-11|col209|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=15
EXCLUIDO|2026-11|col209|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col215
MEDIDO|2026-11|col221
MEDIDO|2026-11|col227
MEDIDO|2026-11|col239
MEDIDO|2026-11|col245
MEDIDO|2026-11|col251
RECUSA|2026-11|col257|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=21
EXCLUIDO|2026-11|col257|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col263
MEDIDO|2026-11|col269
MEDIDO|2026-11|col281
MEDIDO|2026-11|col287
RECUSA|2026-11|col317|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=23
EXCLUIDO|2026-11|col317|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col335
MEDIDO|2026-11|col341
RECUSA|2026-11|col347|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=23
EXCLUIDO|2026-11|col347|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col365
MEDIDO|2026-11|col371
MEDIDO|2026-11|col389
MEDIDO|2026-11|col407
MEDIDO|2026-11|col425
MEDIDO|2026-11|col455
MEDIDO|2026-11|col467
MEDIDO|2026-11|col473
MEDIDO|2026-11|col491
MEDIDO|2026-11|col503
MEDIDO|2026-11|col515
MEDIDO|2026-11|col521
MEDIDO|2026-11|col551
MEDIDO|2026-11|col557
MEDIDO|2026-11|col569
MEDIDO|2026-11|col575
MEDIDO|2026-11|col587
MEDIDO|2026-11|col599
DIA|2026-11|horas_intra_indenizada|col599|2026-10-22|CADASTR

[truncado em 20000 de 40749 caracteres]

## 06/11 14:57 · sessao c7446096827e · O10-C · verificador · origem=Code

**Laudo: há bloqueios em C.** O comando duplica uma autoridade registrada, deixa SOLO exportado fora da lista `!`, aceita lotes cuja repetição volta a escrever e não oferece reversão fiel das células editadas. Também viola o selo existente “todo command tem casa”. Referências detalhadas abaixo.

A comparação efetiva `320ef1c7..a7d1bfa2` ficou **[não verificado]**: a cópia não contém repositório Git. Execução da suíte, RED histórico, conteúdo dos 16 registros e efeitos em produção: **[não verificado]**.

Para encurtar as referências:

- **C** = [app/ponto/management/commands/trocas_plantao_lote_o10c.py](/home/ronald/codex_ws/a7d1bfa296b5a328978102842ba220e71b205514/app/ponto/management/commands/trocas_plantao_lote_o10c.py)
- **T** = [app/ponto/tests/test_o10c_lote_trocas.py](/home/ronald/codex_ws/a7d1bfa296b5a328978102842ba220e71b205514/app/ponto/tests/test_o10c_lote_trocas.py)
- **P** = [app/ponto/portas/celula.py](/home/ronald/codex_ws/a7d1bfa296b5a328978102842ba220e71b205514/app/ponto/portas/celula.py)
- **A** = `app/ponto/services/ausencia.py`
- **G** = `app/ponto/management/commands/gerar_celulas.py`

**1. Quem escreve e quem lê**

O quadro cobre os caminhos identificados relevantes à obra; exaustividade de todos os leitores e escritores históricos: **[não verificado]**.

| Campo/estado | Quem escreve | Quem lê |
|---|---|---|
| `Ausencia.status` | Criação recebe estado inicial; transição por `mudar_estado`, escritor declarado. `A:333–344,418,808–835`. | C classifica cedente e substituto e imprime estado. `C:66–71,83–84,210`. A porta não consulta status antes das edições. `P:653–675`. |
| `Ausencia.data` | Criação e `editar_ausencia_registro`. `A:342,418,499–509`. | Classificação, lavra, vínculo, snapshot e impressão; a porta escolhe o dia pela carta. `C:72–94,120,209`; `P:675`. |
| `Ausencia.data_fim` | Criação/edição; `Ausencia.save` também pode derivá-la de `dias_corridos`. `A:343,418,500–509`; `app/ponto/models.py:902–907`. | C barra fim diferente do início; cobertura lê o período. `C:72–74,85–87`; `app/ponto/turnos.py:2760–2766`. A troca usa somente `data`. `P:675`. |
| `Ausencia.documento` | Criação e `anexar_documento`. `A:350–351,418,846–859`. | C testa presença e imprime S/N; porta testa presença e inclui o caminho na trilha. `C:75,88,210`; `P:664–667,682–690`. |
| `Ausencia.tipo` | Criação e edição. `A:342,418,498–509`. | C seleciona universo e valida tipo do cedente; modelo consulta efeito no catálogo. `C:64–65,166`; `app/ponto/models.py:840`. |
| `Ausencia.colaborador_id` | Criação recebe `colaborador` e salva. `A:341–342,385,418`. | C compara pessoas, consulta autoridades, fotografa e imprime IDs; porta identifica e ordena os lados. `C:81,90–94,120,209`; `P:662,673–684`. |
| `CelulaDia.trabalha` | `editar_dia`; geração/regeneração no mesmo módulo da porta. `P:259–277,600–609`. | Snapshot/idempotência e juiz do previsto. `C:113–121`; `P:528,594`; `app/escala/models.py:1092–1096`. |
| `CelulaDia.origem` | Porta cria `gerada` ou grava `editada`. `P:259–260,600–609`. | Snapshot, idempotência, regeneração e reversão. `P:528,594,263,777`. |
| `CelulaDia.dna` | Produtor `montar_dna`; persistência pela porta. `G:73–103`; `P:591,600–609`. | Snapshot, idempotência e leitura dos marcos congelados. `P:528,594`; `app/escala/models.py:1122–1124`. |
| `CelulaDia.editada_em` | `editar_dia`. `P:598–609`. | Snapshot, carimbos de reescrita, frescor de veredito/fase. `P:530`; `app/escala/services/regeneracao.py:98–105`; `app/chamados/veredito_lavrado.py:151–154`; `app/escala/services/fase_12x36.py:188–192`. |
| `LogAuditoria`, ação `editar_dia` | Porta fornece antes/depois ao escritor `core.observ.evento`. `P:611–620`; `app/core/observ.py:134–148`. | Consulta genérica da auditoria; C não consulta a trilha. `app/core/views.py:705–710`; `C:124–143,155–228`. |
| `LogAuditoria`, ação `trocar_plantao` | Porta emite evento após as edições, com identificação das cartas. `P:682–693`; `app/core/observ.py:134–148`. | Consulta genérica da auditoria; C recebe apenas resultado/frases. `app/core/views.py:705–710`; `C:140–143`. |
| `ExportacaoDominio` | `registrar`, chamado pela exportação HTTP e pelo regerador. `app/folha/models.py:174–188`; `app/folha/views.py:297`; `app/folha/management/commands/regerar_txt_dominio.py:118`. | G enumera competências e pergunta ao juiz; C e P chamam G. `G:161–174`; `C:55–58`; `P:567–570`. |
| `Holerite` / publicação | Motor cria holerite; processador publica o lote. `app/holerite/motor.py:480–483`; `app/holerite/management/commands/processar_lote_holerite.py:123–146`. | G enumera holerites com lote publicado; juiz confirma pessoa/competência. `G:163–174`; `app/folha/export.py:612–620`. |

**Não identifiquei escrita desses campos pelo comando fora da porta.** C chama `trocar_plantao`; sua escrita própria é o arquivo JSONL. A porta tampouco altera as cartas nessa operação. `C:124–143,183,192–195`; `P:653–693`.

Isso atende à centralização da escrita, mas não resolve a duplicação de decisão descrita a seguir. O padrão declarado inclui escritor único e autoridade única por pergunta. `app/docs/GLOSSARIO.md:11–12,23`; `app/docs/PLANO-PADROES.md:46–53`.

**2. `classificar`: testemunha ou segundo juiz?**

**Há duplicação concreta:** `ABERTOS` copia literalmente `ABERTAS` e decide se a ausência está aberta. Essa pergunta já tem autoridade registrada. `C:50,68–69`; `A:765`; `app/core/juizes.py:244`.

O anexo confirma a autoridade e também estabelece que o registro vivo prevalece sobre o lookup. `app/docs/JUIZES.md:7–15,50–52`.

Para lavra e vínculo, C usa as mesmas funções que a porta:

- `_empresa_do_colab` + `_competencias_exportadas`; este último consulta `competencia_fechada_para_celula`, juiz registrado. `C:53–58`; `P:567–570`; `G:173`; `app/core/juizes.py:315–322`.
- `vinculo_do_dia_carregado`, carregador que encaminha ao juiz de vínculo. `C:63,93–96`; `P:556,584`; `app/escala/alimentacao.py:289–317`; `app/docs/JUIZES.md:101–105`.

**A declaração de equivalência com a porta é falsa para outros filtros.** Supondo cartas existentes, pessoas distintas, documentos, vínculo e competência aberta:

| Entrada | DRY | Porta |
|---|---|---|
| Cedente `aguardando_decisao` | `pedido`. `C:68–71`. | Aceita prosseguir: não verifica status. `P:653–675`. |
| Cedente rejeitado | `rejeitada`. `C:66–67`. | Também não verifica esse estado. `P:653–675`. |
| Cedente de outro tipo | `nao_e_troca`. `C:64–65`. | Não verifica tipo. `P:653–675`. |
| Qualquer carta com período de vários dias | `periodo`. `C:72–74,85–87`. | Edita somente `a.data`, sem verificar fim. `P:675`. |
| Substituto rejeitado | `carta_rejeitada`. `C:83–84`. | Não verifica status. `P:653–675`. |

Essas restrições podem representar critérios próprios do lote; apresentá-las como mera leitura do que a porta decidiria contraria a implementação e a regra “testemunha lê, não recalcula”. `C:17–24`; `CLAUDE.md:20–22`.

No sentido contrário, não identifiquei predicado estável omitido que, com autor válido e insumos inalterados, transforme `trocaria` em `recusada`: os filtros de documentos, pessoas, lavra e vínculo estão presentes. Contudo, vínculo/lavra podem mudar entre as consultas, produzindo recusa posterior. `C:75–96,134–142`; `P:664–677,568–590`. Além disso, `trocaria` pode resultar em `igual`, pois C não compara o estado final das células. `C:97`; `P:594–596,679–680`.

**Falha no aval “09 exportada = !”:** SOLO retorna antes da classificação de lavra; a coluna imprime `lavrada=S`, mas somente `classe == 'lavrada'` entra na lista `!` e no contador `bang`. Isso também afeta linhas rejeitadas, pedidos e outras classes anteriores à lavra. `C:66–92,209–225`. Quantos dos 16 registros cairiam nisso: **[não verificado]**.

**3. Atomicidade e concorrência de `aplicar_par`**

- **Ordem explícita correta entre os lados:** C trava as Ausências por PK; P repete essa ordem e edita os lados por `(colaborador_id, data)`. Não encontrei inversão explícita dessas ordens no caminho inspecionado. `C:128–130`; `P:657–658,670–675`.
- **A reclassificação protege as cartas:** ela ocorre depois das travas. Alteração anterior de estado/documento/período pode converter o par em `pulada`. `C:129–136`.
- **A foto não protege as células:** `_celula` usa `.first()` sem trava. Outro `editar_dia` pode modificar a célula entre a foto e a aquisição da trava pela porta; o JSONL então não será o “antes real” da edição. `C:113–138`; `P:592–597`.
- **Permanece TOCTOU de autoridades:** lavra, empresa, vínculo e DNA são consultados antes da trava da célula; não há trava correspondente sobre publicação/exportação ou cadastro nesse caminho. `C:90–96`; `P:567–593`. O risco da guarda anterior ao atomic já está registrado como pendência de B. `app/docs/BACKLOG.md:1790`.
- **Célula inexistente:** a consulta não reserva uma linha ausente; há criação posterior e restrição única por colaborador/data. Concorrência pode terminar em erro de integridade, em vez de serialização prévia. `P:593,599–601`; `app/escala/models.py:1833`. Ocorrência concreta: **[não verificado]**.
- **Rollback do par:** `_TrocaRecusada` desfaz o atomic interno da troca e retorna `recusada`; exceção propagada também atravessa o atomic externo de C. Falha da trilha crítica levanta exceção. O JSONL já escrito permanece. `P:619–620,652–699`; `C:128–143,193–195`.
- **Atomicidade é por par:** pares anteriores já podem ter sido confirmados quando um par posterior falha. `C:128,199–205`.

O rabo de `editar_dia` ocorre depois de seu atomic local, **ainda dentro dos atomics da troca e de C**. O julgamento inline participa dessa transação, embora suas falhas sejam capturadas. O dinheiro e a reavaliação são registrados em `on_commit`: aguardam o commit externo e seus callbacks são descartados se a transação correspondente voltar. `P:627–628,422–462,490–502`; `app/ponto/services/fechamento.py:1782–1808`.

Depois do commit, falha de recálculo/reavaliação não desfaz a troca já confirmada; os caminhos tratam falhas como best effort. Portanto “par confirmado” não prova “todas as projeções atualizadas com sucesso”. `app/ponto/services/fechamento.py:1791–1805`; `P:493–502`; `app/core/observ.py:85–91`.

**4. Idempotência e reversão**

**Um par isolado, sem mudanças externas:** a porta devolve `igual`, sem alterar células, trilha ou executar o rabo novamente. **O comando ainda cria outro snapshot e grava outra linha antes dessa comparação.** A promessa “segunda rodada não escreve nada” não vale para arquivo. `P:594–596,627–628,679–680`; `C:137–142,176–195`; `T:12,215–228`.

**O lote inteiro também não é necessariamente idempotente.** Contraexemplo estático:

1. A e B são cartas aprovadas, documentadas, de pessoas distintas, no mesmo dia aberto.
2. Declarar `--par A:B --par B:A` é aceito: o parser só proíbe repetir o mesmo cedente.
3. O primeiro par deixa A em folga/B trabalhando; o segundo deixa B em folga/A trabalhando.
4. A rodada seguinte repete as alterações e cria novas trilhas.

Não há detecção desse conflito entre pares. `C:100–110,199–205`; `P:673–691`. Execução desse contraexemplo contra banco: **[não verificado]**.

**Snapshot: pré-requisito temporal parcialmente atendido.** O arquivo nasce em `logs/o10c`, e cada foto recebe `flush/fsync` antes da chamada da porta. `C:137–142,176–195`; exigência em `CLAUDE.md:398–400`.

**Não considero demonstrada sua suficiência como reversão operacional**, porque:

- A foto pode estar obsoleta por concorrência. `C:113–138`; `P:593–597`.
- Guarda seis campos da célula, mas não os estados das projeções/chamados que o ato também pode modificar; tampouco registra resultado confirmado ou tenant no payload. `P:528–531,625–628,422–502`; `C:137–143`.
- A reversão existente exige `origem='gerada'`; a troca grava `editada`. `P:600–609,776–778`.
- Chamar `editar_dia` com o booleano anterior remonta DNA atual, força origem `editada` e renova autor/hora; não restaura fielmente a foto anterior. `P:591,598–609`.

Existe caminho para **editar novamente** uma célula editada, inclusive no sentido contrário. Não identifiquei caminho para **restaurar exatamente** seu estado anterior pelo snapshot. `P:594–609`; `C:149–153`.

Hash antes/depois da exportada, prova de frota e aplicação real dos demais requisitos de DINHEIRO: **[não verificado]**; são exigidos em `CLAUDE.md:398–400`.

**5. Os testes mordem?** [nao verificado]

Mordem partes relevantes: resultado real dos dois lados/trilha, chamada com cartas e autor e comportamento distinto para par aberto/exportado. Não são todos vazios. `T:132–143,275–287,301–314`.

A tabela abaixo indica mutantes que **o teste individual** não demonstraria detectar; sobrevivência por execução de mutação: **[não verificado]**. [nao verificado]

| Teste, identificado pela linha em T | Mutante plausível não coberto | [nao verificado]
|---|---|
| `dry_classifica…`, `T:88–107` | Criar arquivo também no DRY: não verifica filesystem. |
| `dry_com_par_completo…`, `T:109–117` | Remover guarda de lavra: fixture usa dia aberto. |
| `solo_em_competencia_lavrada…bang`, `T:119–127` | Remover emissão de `!`: só exige `so_um_lado` e `lavrada=S`. |
| `aprovado_com_par…`, `T:132–143` | Remover snapshot: não lê o arquivo. |
| `snapshot…antes_da_porta`, `T:145–167` | Remover `fsync`, ou omitir DNA/autor/hora/geradora da foto: asserções só cobrem par, trabalha e origem. |
| `rejeitado_e_aguardando…`, `T:169–177` | Aceitar substituto rejeitado: cedentes já saem antes dessa verificação. |
| `sem_a_carta…`, `T:179–189` | Remover reclassificação sob trava: ambos já são barrados na primeira classificação. |
| `dia_em_competencia_exportada…`, `T:191–204` | Consultar lavra só do cedente: ambos estão na mesma competência exportada. |
| `lado_do_substituto…bang`, `T:206–213` | Suprimir linha `!` para lavra apenas do substituto: só verifica classe. |
| `segunda_rodada…`, `T:215–228` | Aceitar pares invertidos conflitantes ou escrever novo snapshot: testa só um par e não filesystem. |
| `nenhum_colaborador…desconto`, `T:230–249` | Remover `fsync`: não inspeciona arquivo; tampouco cobre descontos fora dos três colaboradores/período fotografados. |
| `aplicar_exige_autor`, `T:251–253` | Aceitar username inexistente quando fornecido: só testa ausência de `--por`. |
| `par_repetido_ou_mal_escrito…`, `T:255–261` | Aceitar `1:2:3`: só cobre duplicata e `abc`. |
| `cedente_nao_e_troca…`, `T:263–270` | Remover tratamento de cedente inexistente: fixture contém registro existente. |
| `a_costura_MORDE`, `T:275–287` | Remover trava das Ausências ou atomic externo: porta está mockada, sem concorrência/rollback. |
| `comando_so_escreve_pela_porta`, `T:289–299` | Acrescentar SQL por `cursor.execute`: blacklist não inclui esse caminho. |
| `dry_responde_o_que_a_porta_faria`, `T:301–314` | Remover guarda de período: só compara par normal e exportado, sem período. |

**Há selo sem mordida no requisito específico SOLO/`!`:** o próprio teste espera uma classe que não alimenta `bang`. `T:126–127`; `C:214–225`. [nao verificado]

**A semântica real de `on_commit` não é testada:** `_run` substitui o callback por execução imediata, mascarando adiamento e descarte no rollback. `T:59–63`; `app/ponto/tests/test_o10b_editar_dia_troca.py:39–41`.

A regra dos casos está escrita no docstring; a anterioridade ao código e o RED evidenciado não podem ser confirmados nesta cópia. `T:5–14`; **[não verificado]** para histórico/execução. A exigência está em `CLAUDE.md:28–29,45–51`.

**6. Nome ou CPF**

Na saída normal não há leitura de `colaborador.nome` ou `cpf`: imprime IDs, datas, status e presença de carta. O snapshot usa IDs e o estado da célula; o produtor atual de DNA gera dados de escala/marcos. `C:119–121,137–138,207–212`; `P:528–531`; `G:73–103,119–122`.

**Isso não prova ausência absoluta de dado pessoal:** o DNA anterior é copiado integralmente, sem sanitização; erros reproduzem `--par` inválido e username fornecido. Conteúdo real desses valores: **[não verificado]**. `P:528`; `C:105,165`.

A trilha da porta também conserva `documento.name`; eventual nome/CPF embutido no caminho não é raspado. Isso é trilha, não o snapshot de C. `P:682–690`. Conteúdo dos caminhos reais: **[não verificado]**.

**7. Selos existentes**

| Selo | Resultado da leitura |
|---|---|
| **Todo command tem casa** | **Viola.** O varredor inclui o arquivo novo, mas ele não está nas listas aceitas. `app/chamados/tests/test_contract_crons.py:214–228`; `app/config/crons.py:281,769,1484,1755–1766`. Execução da suíte: **[não verificado]**. |
| **Chokepoint da célula** | Não identifiquei escrita externa no comando; o gate exige allowlist vazia. `C:124–143`; `app/ponto/tests/test_chokepoint_celula_gate.py:20–36,50–54`. |
| **Except-pass** | Não viola esse padrão: o handler usa `continue`, enquanto o selo acusa corpo `ast.Pass`. `C:180–188`; `app/ponto/tests/test_contract_no_except_pass.py:65–78`. |
| **Relógio/instante** | Há relógio em `aplicar_par` e `handle`, para carimbo/nome de arquivo. Não identifiquei violação do selo de grade: essas funções não chamam seus alvos. `C:137,179`; `app/escala/tests/test_selo_grade_pede_o_instante.py:24–38,93–119`. |
| **MAPA** | O comando não pertence ao universo obrigatório, derivado dos registros de juízes e portas. Comparação completa do diagrama regenerado: **[não verificado]**. `app/core/mapa/contrato.py:157–177`. |
| **Juiz de ausência** | Há cópia de autoridade, mas os padrões existentes não cobrem essa tupla de status; esse selo não basta para detectá-la. `C:50,68`; `app/core/juizes.py:244,1406–1420`. |

**8. Classes de reprovação presentes**

- **Segundo juiz / derivação paralela: sim**, pela cópia `ABERTOS` e pela classificação apresentada como equivalente à porta, embora tenha critérios diferentes. `C:17–24,50,64–89`; `app/core/juizes.py:244`; `CLAUDE.md:20–22`.
- **Segundo escritor fora da porta: não identificado** para os estados auditados. `C:124–143`; `P:600–618,685–693`.
- **Relógio fora da borda de negócio: não demonstrado.** As leituras novas servem ao arquivo e ficam no módulo de comando; não julgam dia ou dinheiro. `C:137,179`; escopo do corte em `app/docs/CORTES.md:370`.
- **Selo/RED sem mordida: sim, parcialmente**, no SOLO/`!`; também faltam provas da concorrência e do `on_commit` real. `T:119–127,61,275–314`.
- **Ordem de trava invertida: não identificada no par.** Permanecem foto sem trava e TOCTOU da lavra/vínculo/DNA. `C:113–138`; `P:567–597,657–675`.
- **Pouso antes do pré-requisito B: não demonstrado.** A cópia contém a porta, e o RELATO registra B no ar; o diff e a ancestralidade efetiva do commit são **[não verificado]**. `P:534,632`; `app/docs/RELATO.md:3,40–41`.
- **Pré-requisito de reversão antes do apply: insuficiente operacionalmente**, apesar do arquivo anterior à chamada: a foto pode ser obsoleta e o reversor existente recusa a origem produzida. `C:113–142,193–195`; `P:600–609,776–778`; `CLAUDE.md:398–400`.

## 06/11 14:53 · sessao 0cec89bc15c9 · - · - · origem=Ronald (tmux)

Mapa do caminho exercido pelo lote; inventário exaustivo de todos os leitores históricos do repositório: [não verificado].

| Estado/campo | Quem escreve no caminho de produção | Quem lê |
|---|---|---|
| `Ausencia.colaborador_id` | A criação recebe `colaborador`, monta `kwargs` e salva a instância pela porta `criar_ausencia` (`app/ponto/services/ausencia.py:130`, `:341`, `:385`, `:418`). | Lote compara os colaboradores, consulta lavra/vínculo, fotografa e imprime IDs (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:81`, `:90`, `:94`, `:120`, `:209`); porta usa os dois IDs (`app/ponto/portas/celula.py:662`, `:673`, `:683`). |
| `Ausencia.tipo` | Criação em `kwargs` e `save`; edição por `editar_ausencia_registro` (`app/ponto/services/ausencia.py:342`, `:418`, `:465`, `:498`, `:509`). | Seleção do universo e classificação do cedente (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:64`, `:166`); efeito por catálogo no modelo (`app/ponto/models.py:840`). A porta `trocar_plantao` não o consulta (`app/ponto/portas/celula.py:653-675`). |
| `Ausencia.data` | Criação e edição (`app/ponto/services/ausencia.py:342`, `:418`, `:499`, `:509`). | Classificação de período, lavra/vínculo, snapshot e impressão (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:72`, `:85`, `:90`, `:94`, `:120`, `:209`); porta escolhe o dia editado de cada carta (`app/ponto/portas/celula.py:675`). |
| `Ausencia.data_fim` | Criação e edição (`app/ponto/services/ausencia.py:343`, `:418`, `:500`, `:509`); `Ausencia.save` também deriva fim a partir de `dias_corridos` (`app/ponto/models.py:902-907`); `mudar_estado` inclui `data_fim` no `update_fields` para persistir derivações do modelo (`app/ponto/services/ausencia.py:834-835`). | Lote recusa `data_fim != data`, dos dois lados (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:72`, `:85`); cobertura usa o fim ou fallback da autoridade (`app/ponto/turnos.py:2760-2766`). A porta troca somente `a.data` (`app/ponto/portas/celula.py:675`). |
| `Ausencia.status` | Criação recebe estado ou `estado_inicial` (`app/ponto/services/ausencia.py:333`, `:344`, `:418`); transição pelo escritor declarado `mudar_estado` (`app/ponto/services/ausencia.py:808-835`). | Lote verifica cedente aprovada/rejeitada/aberta e substituto rejeitada, imprimindo status (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:66-71`, `:83`, `:210`). A porta não verifica status (`app/ponto/portas/celula.py:653-675`). |
| `Ausencia.documento` | Criação recebe anexo (`app/ponto/services/ausencia.py:350-351`, `:418`); atualização pela porta `anexar_documento` (`app/ponto/services/ausencia.py:846`, `:857-859`). | Lote testa presença de ambos, imprime apenas S/N (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:75`, `:88`, `:210`); porta exige presença e grava nome do arquivo na trilha do par (`app/ponto/portas/celula.py:664-667`, `:684`). |
| `CelulaDia.trabalha` | `editar_dia` cria ou atualiza (`app/ponto/portas/celula.py:600-609`); geração/regeneração pelo mesmo módulo (`app/ponto/portas/celula.py:259`, `:272-277`). | Lote fotografa pelo helper (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:113-121` → `app/ponto/portas/celula.py:528`); porta compara para idempotência (`app/ponto/portas/celula.py:594`); juiz previsto lê `cel.trabalha` (`app/escala/models.py:1069`, `:1092-1096`). |
| `CelulaDia.origem` | Porta cria como gerada ou editada, e edição força editada (`app/ponto/portas/celula.py:259-260`, `:600`, `:606`, `:609`). | Snapshot (`app/ponto/portas/celula.py:528`), idempotência (`:594`), regeneração limita a gerada (`:263`) e reversão de regeneração recusa editada (`:777`). |
| `CelulaDia.dna` | Produtor `montar_dna` é chamado pela edição; persistência fica na porta (`app/ponto/portas/celula.py:591`, `:600-609`); regeneração escreve pelo mesmo módulo (`:256`, `:272-277`). | Snapshot e comparação (`app/ponto/portas/celula.py:528`, `:594`); marcos do dia leem DNA congelado (`app/escala/models.py:1098`, `:1122-1124`). |
| `CelulaDia.editada_em` | `editar_dia` usa `timezone.now()` e persiste (`app/ponto/portas/celula.py:598-609`). | Snapshot (`app/ponto/portas/celula.py:530`); carimbos de reescrita (`app/escala/services/regeneracao.py:98-105`); fatos novos para veredito e fase (`app/chamados/veredito_lavrado.py:151-154`, `app/escala/services/fase_12x36.py:188-192`). |
| `LogAuditoria.acao='editar_dia'` | Porta emite `evento_kw` com antes/depois (`app/ponto/portas/celula.py:611-618`); adaptador vai ao escritor `evento`, que faz INSERT (`app/core/observ.py:134-148`). | Tela genérica consulta e permite filtrar ação (`app/core/views.py:705-710`); lote não consulta a trilha (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:124-143`, `:155-228`). |
| `LogAuditoria.acao='trocar_plantao'` | Porta emite após os lados; inclui IDs, datas, documento e resultado (`app/ponto/portas/celula.py:682-693`), pelo mesmo escritor (`app/core/observ.py:134-148`). | Mesma tela genérica (`app/core/views.py:705-710`); o lote somente recebe retorno/frases da porta (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:140-143`). |
| `ExportacaoDominio` | `registrar` cria registro (`app/folha/models.py:174-188`), chamado pela exportação HTTP (`app/folha/views.py:297`) e pelo regerador (`app/folha/management/commands/regerar_txt_dominio.py:118`). | `_competencias_exportadas` enumera mês/ano e pergunta ao juiz (`app/ponto/management/commands/gerar_celulas.py:161-174`); lote e porta chamam esse carregador (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:55-58`, `app/ponto/portas/celula.py:567-570`). |
| `Holerite` / publicação de lote | Motor cria documento associado ao lote (`app/holerite/motor.py:480-483`); job de publicação grava `LoteHolerite.publicado_em` (`app/holerite/management/commands/processar_lote_holerite.py:126-130`, `:146`); uploads também criam holerite (`app/holerite/views.py:1193-1203`, `:1259-1265`). | `_competencias_exportadas` enumera somente holerites com lote publicado (`app/ponto/management/commands/gerar_celulas.py:163-174`); o juiz faz o predicado final por pessoa e competência (`app/folha/export.py:612-620`). |

O lote não contém chamada direta de escrita ORM desses campos: o caminho de mutação é `celula.trocar_plantao(...)`; grava seu arquivo por `open/write/flush/fsync` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:140-143`, `:183`, `:192-195`). O selo novo verifica nomes de chamadas, não prova sozinho a ausência de qualquer escrita indireta (`app/ponto/tests/test_o10c_lote_trocas.py:289-299`).

O anexo é lookup; registro vivo manda (`app/docs/JUIZES.md:7-15`). Para “ausência aberta ou decidida”, ele aponta `services/ausencia.py::ABERTAS` (`app/docs/JUIZES.md:50-52`), igualmente registrado em `app/core/juizes.py:244`. O lote cria cópia literal `ABERTOS` e a consulta, portanto duplica essa autoridade mesmo com valores atualmente iguais (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:50`, `:68`; autoridade atual em `app/ponto/services/ausencia.py:765`).

O lote consulta corretamente a autoridade existente de lavra por meio de `_competencias_exportadas`, que chama `competencia_fechada_para_celula`; esse juiz está registrado como `_K9` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:55-58`, `app/ponto/management/commands/gerar_celulas.py:173`, `app/core/juizes.py:315-322`). Também usa o mesmo carregador de vínculo da porta (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:63`, `:93-96`; `app/ponto/portas/celula.py:556`, `:584`).

A declaração “pergunta a mesma função que a porta pergunta” não vale para todos os filtros: o lote verifica tipo, estado e período; a porta verifica existência, colaboradores diferentes, documentos, lavra e vínculo, sem ler tipo/status/fim (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:64-96`; `app/ponto/portas/celula.py:653-675`). Contraprovas estáticas, supondo demais condições válidas: cedente com carta e `aguardando_decisao` dá `pedido` no DRY, mas a porta aceita; cedente tipo diferente dá `nao_e_troca`, mas a porta aceita; qualquer carta com fim diferente da data dá `periodo`, mas a porta edita somente sua data; substituto rejeitado dá `carta_rejeitada`, mas a porta aceita (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:64-89`; `app/ponto/portas/celula.py:664-675`). Essas são divergências de contrato; sua ocorrência nos 16 registros reais é [não verificado].

Não há reversor fiel de edição identificado no caminho inspecionado: `reverter_regeneracao_dia` exige origem gerada (`app/ponto/portas/celula.py:776-778`); chamar `editar_dia` com booleano anterior remonta DNA atual e força origem editada, sem restaurar timestamp/origem/DNA antigos (`app/ponto/portas/celula.py:591`, `:598-609`).

## 06/11 14:53 · sessao 1597aea2e856 · - · - · origem=Ronald (tmux)

Achado concreto: o comando viola o selo **B6.1 — todo command tem casa**. O teste varre `*/management/commands/*.py` e exige presença em `todas_as_entradas()` ou `FORA_DE_PIPELINE`; o novo comando não aparece em nenhum dos dois, confirmado por consulta Python somente leitura aos dados de configuração. Portanto `test_todo_command_tem_casa` falharia (`app/chamados/tests/test_contract_crons.py:214–228`; fontes: `app/config/crons.py:281`, `:769`, `:1484`, `:1755–1766`).

Outras verificações:

- **Chokepoint:** o censo oficial aplicado somente ao novo fonte retornou `[]`; o comando só chama a porta em `:140`, sem verbo ORM de escrita. O gate da célula varre produção e tem allowlist vazia (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:128–143`; `app/ponto/tests/test_chokepoint_celula_gate.py:20–36`, `:50–54`; `app/core/censo_escritas.py:451–466`).
- **Except-pass:** nenhum caso no AST. O único handler trata colisão do nome do snapshot com `continue`, enquanto o selo acusa exclusivamente handler cujo corpo é um `ast.Pass` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:180–188`; `app/ponto/tests/test_contract_no_except_pass.py:65–78`).
- **Relógio:** há leitura em `aplicar_par`, fora de `handle`, e outra em `handle`; a primeira carimba o snapshot, sem escolher comportamento por data (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:137`, `:179`). O selo de instante examina somente funções que chamam seus `ALVOS`; `aplicar_par` não chama esses alvos (`app/escala/tests/test_selo_grade_pede_o_instante.py:25–38`, `:93–119`). Não constatei violação mecânica desse selo; conformidade com eventual lei mais ampla sobre borda do relógio: [não verificado].
- **Relógio dos testes:** o selo procura leituras diretas no próprio arquivo; o novo teste não as contém. A fixture importada tem `@freeze_time(HOJE)` (`app/core/tests/test_selo_teste_sem_relogio.py:32–45`; `app/ponto/tests/test_o10c_lote_trocas.py:33–34`; `app/ponto/tests/test_o10b_editar_dia_troca.py:68–69`).
- **MAPA:** o comando não pertence ao universo obrigatório de inscrição, que deriva exclusivamente de `core/juizes.py` e `core/portas.py`; consulta somente leitura confirmou isso (`app/core/mapa/contrato.py:157–184`; `app/core/tests/test_selo_mapa_contrato.py:41–49`). O MAPA commitado deliberadamente exclui contagens de leitores novos; elas vão à saída do gerador (`app/core/management/commands/gerar_diagrama.py:861–865`, `:936–939`; `app/core/tests/test_selo_diagrama_do_codigo.py:328–333`). Comparação completa do MAPA regenerado: [não verificado].
- **Cópia de juiz que passa pelos selos:** `ABERTOS` replica a pergunta registrada em `ABERTAS`; o comando declara a tupla em `:50` e a consulta em `:68`. A autoridade registrada é `ponto/services/ausencia.py::ABERTAS` (`app/core/juizes.py:244`). O selo não acusa essa cópia porque os padrões da família não incluem status aberto; análise de todos os padrões proibidos sobre o novo fonte retornou zero ocorrências (`app/core/juizes.py:1406–1420`; varredor `app/ponto/tests/test_contract_juiz_celula.py:48–60`).

_2350 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
