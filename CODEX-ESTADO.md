# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 13:30** · publicado em **11/11/2026 13:30** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 13:30 · sessao 23e5b22c9217 · LASTRO-FOLGA · censo · origem=Code

**O fecho separa dois casos: fora de `TIPO_CHOICES` é permitido; fora do catálogo vivo é rejeitado pela criação e pela edição.** A persistência comum, `Ausencia.save()`, não valida nenhum desses conjuntos. O dado fornecido não demonstra qual caminho gravou `folga`, nem se esse código estava cadastrado e ativo naquele momento. (`app/ponto/services/ausencia.py:193-195,474-475`; `app/ponto/models.py:868-908`; origem e cadastro históricos: [nao verificado])

**1. DELTA a0269430 → e70a54f7e**

Comparação direta com a cópia `../a026943099b8d88468240fd4b18491141b6810af`; identificação dos hashes pelos nomes dos diretórios, sem metadados Git: [nao verificado].

Abaixo estão somente alterações pertinentes aos caminhos do censo e novos leitores/escritores:

| Caminho | Delta | Citação atual |
|---|---|---|
| `editar_ausencia_registro` | Guarda o intervalo anterior e invalida o fechamento abrangendo os intervalos antigo e novo. A validação de tipo permanece pelo catálogo vivo. | `app/ponto/services/ausencia.py:474-475,497-524` |
| Materialização de disputa | Retirou minutos artificiais de dia inteiro e `checar_parcial=False`; continua criando pela porta validada. | `app/chamados/services/materializacao.py:474,490-499,526-535` |
| **Novo command** `rederivar_minutos_h06_comp11` | Confere identidade incluindo tipo e chama edição com `tipo=a.tipo`; portanto, passa pelo validador existente. Também lê tipo para snapshot. | `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:120-123,142-146,169-172` |
| `cobertura_ausencia_periodo` | Extraiu seleção e expansão para os **novos** `_q_cobertura` e `_cobertura_pura`; conserva o tratamento do desconhecido. | `app/ponto/turnos.py:2752-2783` |
| **Novo acesso em lote** `CargaDaJanela.cobertura` | Reutiliza seleção e expansão da cobertura. | `app/ponto/turnos.py:3068-3082` |
| **Novo carregador** `carga_pergunta_viva` | Carrega ausências para cobrança usando o mesmo filtro de cobertura; oferece essas linhas ao classificador. | `app/chamados/services/carga_pergunta_viva.py:79-108,149-150` |
| `classificar_falta` | Aceita carga previamente lida; mantém o veto à cobrança quando encontra ausência não parcial. | `app/escala/servico_jornada.py:280-293,347-353` |
| Fechamento financeiro | Substituiu “tem minutos” por `cobre_parte_do_dia`; removeu acumulador morto. Desconhecido continua suprimindo previsto. | `app/ponto/services/fechamento.py:672-673,688-701` |
| Coletor de efeitos do espelho/PDF | Nos dois ramos de parcialidade, passou a consultar `cobre_parte_do_dia`. | `app/ponto/services/efeito_ausencia.py:33-55` |
| Crédito parcial | Exige parcialidade confirmada pelo juiz, além de tipo pago. | `app/ponto/services/credito_parcial.py:20-28` |
| `dia_decidido.rotulo_de` | Deixa de apresentar minutos como abono parcial quando a ausência explica o dia inteiro. | `app/ponto/services/dia_decidido.py:487-497` |
| Lista de ausências | Acrescentou leitura de parcialidade e de fim aberto pelo tipo para apresentação. | `app/ponto/views.py:2233-2245` |
| Cartório, ramo sem adesão | Passou a considerar o julgamento de dia sem trabalho no horizonte declarado; preserva a precedência da cobertura integral de ausência. | `app/ponto/services/cartorio.py:267-285,1230-1280` |

Nenhum escritor/leitor citado foi encontrado como removido na comparação. Essa é uma constatação negativa da inspeção, sem relatório persistido: [nao verificado].

**2. Escritores: quem aceita fora de qual catálogo**

| Entrada/escritor | Validação existente e limite |
|---|---|
| Admin da aplicação | Recebe tipo do POST e chama `criar_ausencia`, que rejeita código fora do vivo. Aceita fora dos choices quando cadastrado. `app/ponto/views.py:2378,2415-2422`; `app/ponto/services/ausencia.py:193-195`. |
| PWA | Verifica o vivo diretamente e chama o produtor validado. `app/ponto/views.py:2509-2511,2536-2542`. |
| API | Traduz aliases legados de atestado/luto e chama o produtor validado; não traduz `folga`. `app/api/views.py:1765-1768,1817-1824`. |
| Edição HTTP | Delega à edição, que rejeita fora do vivo. `app/ponto/views.py:2794-2802`; `app/ponto/services/ausencia.py:474-475`. |
| Formulário | `clean_tipo` valida pelo vivo. Seu campo é `CharField`, não uma escolha congelada. A definição existe; sua utilização efetiva pelos endpoints não ficou demonstrada. `app/ponto/forms.py:41-68`; utilização: [nao verificado]. |
| Django admin | `AusenciaAdmin` usa `SoLeitura`, negando inclusão, alteração e exclusão. `app/ponto/admin.py:37-47`; `app/core/admin.py:59-75`. |
| Copiloto | Prévia consulta o vivo; escrita chama o produtor. `app/ponto/services/checar_ausencia.py:23-24,43-48`. |
| Veredito humano | Envia `falta` ou o tipo recebido ao produtor validado. `app/ponto/services/veredito_celula.py:331-340`. |
| Materialização | Envia tipo do mapa/argumento ao mesmo produtor. `app/chamados/services/materializacao.py:474,490-499,526-535`. |
| Regularização e abandono provisório | Enviam `falta` por `garantir_ausencia`, que delega à criação validada. `app/ponto/services/regularizacao.py:118-122`; `app/ponto/management/commands/marcar_abandono_provisorio.py:76-84`; `app/ponto/services/ausencia.py:453-462`. |
| Espelhamento de férias | Envia literalmente `ferias` ao produtor validado. `app/ponto/services/ausencia.py:1202-1207`. |
| Command novo de minutos | Reenvia o tipo carregado à edição validada. `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169-172`. |
| Command de rederivar fim | Seleciona registros pelo intervalo/tipo e chama `rederivar_fim`; este faz `save()` completo sem validar tipo. Pode regravar código desconhecido já carregado. `app/ponto/management/commands/curar_ausencia_sem_fim.py:29-31,42-46`; `app/ponto/services/ausencia.py:1293-1298`. |
| Persistência `Ausencia.save()` | Valida coerência do intervalo e deriva outros campos, mas não consulta validade do tipo. É a brecha estrutural demonstrável para persistência direta. `app/ponto/models.py:868-908`. |

Não foi identificado importador próprio que introduza `Ausencia.tipo`; cobertura de execução externa, SQL manual ou importação não presente na cópia: [nao verificado]. O varredor do repositório contempla criação, alterações em lote e saves atribuíveis, mas declara limites de atribuição. (`app/core/censo_escritas.py:6-56,61-82,469-491`)

**Por que os choices não barraram:** a criação exclui deliberadamente `tipo` do `full_clean`, depois de validar pelo vivo. Há teste explícito exigindo que código cadastrado fora dos choices seja aceito. (`app/ponto/services/ausencia.py:361-387`; `app/ponto/tests/test_selo_b5f1_porta_ausencia.py:180-194`)

**Como `folga` poderia passar pelas portas validadas:** o cadastro permite criar esse código, sem reserva nominal; se ativo, ele entra no catálogo vivo. Isso seria aceitação deliberada pelo cadastro, não ausência de validação. Existência desse cadastro na gravação fornecida: [nao verificado]. (`app/ponto/views_tipos_ausencia.py:61-94`; `app/ponto/services/ausencia.py:1301-1310`; `app/ponto/catalogo/ausencias.py:249-252,284-285`)

**3. Autoridade e listas existentes**

| Lista/fonte | Papel |
|---|---|
| `TipoAusencia` ativo → `vigente()` → `codigos_validos()` | Autoridade operacional de validade. Há cache por processo de 60 segundos; consulta vazia ou falha recai na semente. `app/ponto/catalogo/ausencias.py:240-285`. |
| `catalogo.ausencias.CATALOGO` | Semente/fallback com 17 códigos; não contém `folga`. `app/ponto/catalogo/ausencias.py:87-105`. |
| `Ausencia.TIPO_CHOICES` | Lista congelada com 17 códigos, ligada ao campo; não contém `folga`. `app/ponto/models.py:661-679,749`. |
| `forms.CATALOGO` | Outra lista, de regras de duração; ausência nela recebe máximo padrão de 30 dias. A validade continua sendo consultada no vivo. `app/ponto/forms.py:13-38,60-68`. |
| `_FAIXA_DE_TIPO` | Mapa separado de precedência; não é catálogo de validade. `app/ponto/precedencia.py:52-79`. |
| `TIPOS_PAGOS`, bloqueantes e abertos do modelo | Conjuntos derivados da autoridade, não listas independentes de validade. `app/ponto/models.py:680-689`. |
| Migration de seed e choices históricos | Listas históricas de instalação/esquema. `app/ponto/migrations/0052_seed_tipoausencia.py:9-37`; `app/ponto/migrations/0070_alter_ausencia_tipo.py:19-22`. |

Portanto, existe **uma autoridade operacional pretendida**, acompanhada de listas congeladas e mapas com outros papéis. Não existe uma única enumeração física. (`app/ponto/catalogo/ausencias.py:17-27,240-285`; `app/ponto/models.py:661-689`; `app/ponto/forms.py:13-38`)

**4. Decisores: comportamento de código ausente de `vigente()`**

| Decisor | Resultado para desconhecido |
|---|---|
| Catálogo | Efeito `None`; rótulo é o código. `app/ponto/catalogo/ausencias.py:293-300`. |
| `precedencia.faixa_de` | Retorna `atestado`, inclusive para `folga`. Só `folga_compensatoria` e `feriado_folga` têm faixa folga no mapa. `app/ponto/precedencia.py:52-79`. |
| Fatos do período | Inclui ausência não rejeitada, preserva código e aplica a precedência; pode vencer feriado, folga e trabalho. `app/ponto/precedencia.py:273-279,336-339,379-408`. |
| Cobertura | Folha exige aprovada; cobrança aceita não rejeitada. Intervalo fechado pode cobrir vários dias; desconhecido sem fim cobre somente a data inicial. `app/ponto/turnos.py:2511-2520,2554-2559,2770-2783`. |
| Parcialidade/integral | Não é parcial; sendo diferente de `falta`, explica o dia inteiro. Porém não entra no mapa integral **abonado**, que exige efeito `ABONA`. `app/ponto/turnos.py:2597-2602,2670-2692`. |
| Cartório | Copia a precedência para a célula; ausência aprovada integral dispensa lâmpadas e grava o código literal em `dispensada`. `app/ponto/services/cartorio.py:790-800,1984-1993`. |
| Motor/calculador | O desconhecido fica fora do mapa de integrais abonados que zera atraso/saída; o motor não valida novamente o catálogo nesse ramo. `app/ponto/turnos.py:2686-2692`; `app/ponto/motor_calculo_v2.py:1085-1105`; `app/ponto/calculador/regras.py:117-120`. |
| Fechamento financeiro | Não abona e **suprime previsto**, pois `None != DESCONTA`. `app/ponto/services/fechamento.py:693-701,729-732`. |
| Coletor do espelho/PDF | Não justifica nem suprime pelo efeito: exige `ABONA` ou `SUPRIME` explícitos. Conserva a divergência com o fechamento. `app/ponto/services/efeito_ausencia.py:53-65`. |
| Crédito parcial | Não recebe crédito: precisa pertencer aos tipos pagos e ser parcial. `app/ponto/services/credito_parcial.py:26-28`. |
| Export de faltas/8792 | Fica fora do filtro de tipos que descontam. `app/folha/export.py:87-91,138-140,179-181`. |
| Export de abonos/8932 | Fica fora do conjunto vivo de abonados. `app/folha/export.py:193-205`. |
| Veredito visual | Retorna `ABONADO`, salvo rejeitada; lançamento aberto aparece pendente. `app/ponto/services/dia_decidido.py:447-450,460-481`. |
| Status do dia | Faixa atestado com efeito `None` resulta em `ausencia`. `app/ponto/status_dia.py:296-319`. |

Esses resultados descrevem condições do código. Ata, competência, export e efeito financeiro efetivamente associados ao registro fornecido: [nao verificado].

**5. Arquivos da cura na origem e testes vizinhos** [nao verificado]

Para o recorte **impedir introdução ou troca para tipo fora do vivo pela persistência comum**, o pacote operacional mínimo fechado é:

- `app/ponto/models.py`: guarda no escritor `Ausencia.save`, consultando a autoridade existente; o tratamento deve distinguir introdução de tipo de preservação de histórico, já prevista pelo cadastro inativo. (`app/ponto/models.py:607-608,868-908`; `app/ponto/catalogo/ausencias.py:284-285`)
- `app/ponto/tests/test_selo_b5f1_porta_ausencia.py`: acrescentar casos de persistência direta inválida e preservar o caso concreto válido fora dos choices. (`app/ponto/tests/test_selo_b5f1_porta_ausencia.py:180-194`)

Esse pacote fecha a ausência de guarda demonstrada; atribuí-lo como cura da causa histórica de `folga` permanece [nao verificado]. Impor `TIPO_CHOICES` como validade contrariaria o contrato existente. (`app/ponto/services/ausencia.py:361-370`; `app/ponto/tests/test_cadastro_tipo.py:43-50`)

Os testes vizinhos que enumeram catálogo, choices, conjuntos ou diferenças entre eles são: [nao verificado]

| Arquivo | Enumeração/contrato |
|---|---|
| `app/ponto/tests/test_catalogo_ausencias.py` | Choices × semente, efeitos, partição e dropdown. `:10-20,36-42,56-60` |
| `app/ponto/tests/test_cadastro_tipo.py` | Seed, vivo, novo, inativo e fallback. `:14-34,43-50` |
| `app/ponto/tests/test_selo_b5f1_porta_ausencia.py` | Código concreto vivo fora dos choices. `:180-194` |
| `app/ponto/tests/test_aus_f4_criar_pelo_juiz.py` | Vivo menos choices e inválido; o laço pode ficar vazio. `:43-54` |
| `app/api/tests/test_api_ausencias_tipos.py` | Confere anúncio GET contra choices congelados. `:46-51` |
| `app/ponto/tests/test_fim_separado.py` | Semente e conjuntos de duração. `:34-42,55-60` |
| `app/ponto/tests/test_contract_rotulo_x_efeito.py` | Semente por efeito e tipos nomeados. `:34-37,69-75` |
| `app/ponto/tests/test_rotulo_x_efeito.py` | Rótulos por catálogo. `:45-64` |
| `app/ponto/tests/test_rotulo_do_dia_decidido.py` | Tipos da semente. `:84-92` |
| `app/ponto/tests/test_contract_espelho_sem_emoji.py` | Ícones de todos os tipos da semente. `:271-282` |
| `app/ponto/tests/test_ui3_rotulo_do_espelho.py` | Tipos da semente e desconhecido. `:25-30,46-64` |
| `app/ponto/tests/test_catalogo_vivo.py` | Tipos novos/inativos nos conjuntos. `:30-50` |
| `app/ponto/tests/test_conjuntos_vivos.py` | Conjuntos de efeito e exclusões do export. `:14-45` |
| `app/ponto/tests/test_aviso_home_office.py` | Presença do tipo nos choices e conjuntos. `:11-17` |
| `app/ponto/tests/test_o124_medico_sai_do_cadastro.py` | Tipo médico fora da semente e alterações do conjunto vivo. `:87-93,109-140` |

A correção na origem e a autoridade do cadastro já têm regra; dado gravado e cadastro seguem o destino separado da L-099. O dado disponível não sustenta criar regra nominal especial para `folga`. (`CLAUDE.md:17-27,32-44`; `app/docs/LEIS.md:122`; `app/ponto/models.py:562-569`; situação histórica de `folga`: [nao verificado])

perguntas de lei: 0

## 11/11 13:30 · sessao c168102e1e42 · RAIZ-DA-DISPUTA · censo · origem=Code

**O fecho mantém D8 aberta. O DELTA Git e a lista futura de edições não podem ser certificados nesta cópia.** (`app/chamados/services/fio.py:1275`; limitações da comparação e do perímetro futuro: [nao verificado].)

1. **DELTA entre `9ca563bb3` e `e70a54f7e`**

**[nao verificado].** A tentativa de comparação falhou porque a cópia não contém repositório Git; não há diff fornecido entre os commits. A documentação confirma que esse ambiente recebe uma árvore por `git archive`. Portanto, deslocamentos de citações não demonstram quais linhas mudaram ou sumiram entre os commits. (`CLAUDE.md:344`; ausência de `.git` e saída do comando: [nao verificado].) [nao verificado]

A lista solicitada de **somente mudanças/supressões** permanece não verificada; isso não significa delta vazio. **[nao verificado]**

2. **O aval responde à pergunta BATIDA sem episódio × BATIDA com episódio?**

**Não.** A frase “O par e LIDO dos marcos da celula do dia (saida e volta do mesmo intervalo)” decide a identidade da **pausa**, sem estabelecer equivalência entre BATIDAS com e sem episódio. **[nao verificado — frase e alcance do anexo recebido, sem arquivo local numerado.]**

O código continua declarando a questão aberta: a BATIDA do acervo “nao retrata linha de episodio nem e retratada por ela”; o ramo executável mantém essas linhas em `sem_episodio_intocada`. (`app/chamados/services/fio.py:1274`, `app/chamados/services/fio.py:1275`, `app/chamados/services/fio.py:1294`.)

**Falta precisamente o critério que determine quando essas duas linhas representam o mesmo fato e permita ou vede a retratação entre elas.** O aval fornecido não contém esse critério. (`app/chamados/services/fio.py:1275`; conteúdo do aval: [nao verificado].)

3. **Já existe por AST a função “estas duas perguntas são o par da mesma pausa”?**

**Nenhuma encontrada no AST inspecionado.** A varredura foi executada em memória sobre `app` e `bin`, excluindo testes, migrations e diretórios gerados; execução sem artefato local numerado: **[nao verificado]**. [nao verificado]

Os sítios pertinentes existentes têm estes contratos:

| Sítio | O que efetivamente responde |
|---|---|
| `CelulaDia.PARES_PAUSA_DNA` | Declara `hii/hfi` e `hii2/hfi2`; não compara perguntas. (`app/escala/models.py:1652`.) |
| `EscalaColaborador.pausas_do_dia` | Enumera pausas completas do DNA; célula existente sem marcos retorna vazio. (`app/escala/models.py:1197`, `app/escala/models.py:1207`.) |
| `EscalaColaborador.intervalo_do_dia` | Retorna somente a primeira pausa. (`app/escala/models.py:1163`.) |
| `tarefa_do_dia._posicoes` / `instancias_do_turno` | Enumeram instâncias, inclusive `int_ida:2`, e expõem posição, hora, lâmpada e teto. (`app/chamados/services/tarefa_do_dia.py:58`, `app/chamados/services/tarefa_do_dia.py:92`.) |
| `fato_do_marco_no_espelho` | Para intervalo, exige acesas as lâmpadas da posição em **todas** as pausas cadastradas. (`app/chamados/juizes.py:957`, `app/chamados/juizes.py:971`.) |
| `_resposta_concorda_com_a_luz` | Retorna `None` quando há mais de uma pausa naquela posição, pois não identifica qual delas a pergunta cobra. (`app/chamados/juizes.py:1334`, `app/chamados/juizes.py:1336`.) |

A limitação está explícita: nenhum emissor carrega o índice da pausa, e o nascimento rejeita `instancia_marco` diferente da posição simples derivada do motivo. (`app/chamados/services/declaracao_texto.py:49`, `app/chamados/models.py:3441`.)

4. **Portas que fecham automaticamente hoje: autor e motivo**

Nos fechos normais abaixo, o chamador omite `motivo`; a porta grava **`veredito` quando existem perguntas ou `vazia` quando não existem**. Grava o autor recebido em `fechada_por`, a resolução e o evento `disputa_fechada`; não consulta o autor do último carimbo. (`app/chamados/models.py:1903`, `app/chamados/models.py:1933`, `app/chamados/models.py:1937`.)

| Porta | Autor passado hoje | Resolução gravada |
|---|---|---|
| Ausência declarada | `user` recebido. | `Ausencia declarada pelo colaborador; perguntas do dia resolvidas.` (`app/chamados/services/materializacao.py:591`.) |
| Cauda da materialização | `request_user`. | Primeira não vazia entre `resolucao_pedida`, `disputa.resolucao` e `Regularizacao granular: {n_mat} pergunta(s) materializada(s).` (`app/chamados/services/materializacao.py:1333`, `app/chamados/services/materializacao.py:1338`.) |
| Reconciliação sem turno aberto | Sistema, também autor do carimbo. | `Reconciliador: sem turno aberto no dia; perguntas resolvidas.` (`app/chamados/reconciliador.py:400`, `app/chamados/reconciliador.py:405`, `app/chamados/reconciliador.py:412`.) |
| Absolvição pela célula | Sistema, também autor dos carimbos. | `Celula soberana julgada absolveu o(s) dia(s); perguntas resolvidas pelo reconciliador.` (`app/chamados/reconciliador.py:587`, `app/chamados/reconciliador.py:595`, `app/chamados/reconciliador.py:612`.) |
| Órfãs pela lâmpada | Sistema, também autor de `encerrar_fantasma`. | `Perguntas orfas encerradas no evento da celula: a lampada do proprio marco acendeu (ORFAS-ENTERRA-FURO-REAL (b)).` (`app/chamados/reconciliador.py:1142`, `app/chamados/reconciliador.py:1147`, `app/chamados/reconciliador.py:1171`.) |
| Beco | Parâmetro `sistema`; a entrada do evento fornece sistema. | `Nenhuma pergunta viva; chamado-pai resolvido. Beco fechado pelo varredor (HX-BECO).` (`app/chamados/services/becos.py:18`, `app/chamados/services/becos.py:57`, `app/chamados/services/becos.py:78`.) |
| Emissão sem pergunta exigível | Sistema. | `Fechada automaticamente: nada a perguntar ao colaborador neste dia (o juiz da jornada nao aponta marco faltante). Se o fato voltar a acusar, o cartorio reabre pelo caminho normal.` (`app/chamados/services/disputa_emissao.py:1283`, `app/chamados/services/disputa_emissao.py:1284`.) |
| Chamado resolvido / S128 | `user` recebido; o signal fornece sistema. | `Saida registrada; turno encerrado - resolvido automaticamente pelo sistema (S128).` (`app/chamados/services/disputa_emissao.py:1964`, `app/chamados/signals.py:115`.) |
| Remoção do último motivo | `ator` recebido. | `resolucao` recebida; os eventos de posto/âncora usam sistema e as resoluções `auto_revalidacao_posto_reativado` / `auto_revalidacao_ancora_calibrada`. (`app/chamados/models.py:2072`, `app/colaboradores/signals.py:77`, `app/escala/signals.py:161`.) |
| Entrada sincronizada | Sistema, embora a função receba `autor_user`. | `Entrada registrada em {hora} (sync); orfao reconciliado, disputa dissolvida automaticamente.` (`app/ponto/services/entrada_sincronizada.py:19`, `app/ponto/services/entrada_sincronizada.py:59`.) |
| Foto recebida | `user`, também passado à validação. | `Auto-fechada: foto recebida no upload separado do APK.` (`app/api/foto_recebida.py:79`, `app/api/foto_recebida.py:80`.) |
| Comando `encerrar_perguntas_com_fato`, com `--apply` | Usuário indicado por `--por`. | `BUG 117: todas as perguntas tinham o fato no espelho. Encerrada pela porta, com trilha.` (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:53`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:63`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`.) |

A cauda de materialização também atende à validação e ao reparador do passivo; o comando de becos usa a mesma porta de beco. (`app/chamados/services/validacao.py:469`, `app/chamados/services/passivo_validado.py:405`, `app/chamados/management/commands/fechar_becos_disputa.py:57`.)

As exceções automáticas nomeadas são:

| Porta | Autor / motivo / resolução |
|---|---|
| Competência trancada | `autor` recebido ou sistema; motivo `competencia_trancada`; resolução identifica competência e empresa. Esse caminho não carimba perguntas. (`app/chamados/services/competencia_trancada.py:175`, `app/chamados/services/competencia_trancada.py:178`, `app/chamados/services/competencia_trancada.py:204`.) |
| Desligamento | `_ator` recebido ou sistema; motivo `desligamento`; resolução `Colab desligado em {data_demissao} - fechamento automatico.` (`app/colaboradores/services/desligamento.py:131`, `app/colaboradores/services/desligamento.py:142`.) |

`admin_encerra` é a porta administrativa explícita, com motivo textual obrigatório, não uma cauda automática. (`app/chamados/models.py:1943`, `app/chamados/models.py:1950`.)

**Restrição concreta para F3:** `materializada_por` não substitui necessariamente o autor do ato atual. A retratação pode preservar o carimbo antigo com `manter_carimbo=True`; o beco, por sua vez, agenda somente `disputa_id` e escolhe sistema depois. (`app/chamados/models.py:2061`, `app/chamados/models.py:3182`, `app/chamados/signals.py:231`, `app/chamados/services/becos.py:78`.)

5. **Arquivos separados para conferência de sobreposição**

**Lista futura FECHADA: [nao verificado].** O código permite identificar os sítios envolvidos, mas não comprova quais serão editados: isso depende de onde forem implementados o transporte do autor, a reação da raiz e a identidade da instância. A relação abaixo é um **perímetro conservador proposto para reserva**, não um diff certificado. **[nao verificado quanto às edições futuras.]**

**F3 — produção:**

- `app/chamados/models.py:1855` — fechamento, trilha e portas de carimbo (`:3103`, `:3168`).
- `app/chamados/signals.py:228` — reação da pergunta sem transporte de autor.
- `app/chamados/services/becos.py:57` — cauda do beco.
- `app/chamados/services/materializacao.py:591` — fecho por ausência e cauda geral (`:1338`).
- `app/chamados/reconciliador.py:412` — três caudas (`:612`, `:1171`).
- `app/chamados/services/disputa_emissao.py:1284` — emissão muda e S128 (`:1964`).
- `app/api/foto_recebida.py:80` — fecho posterior à validação.
- `app/ponto/services/entrada_sincronizada.py:59` — fecho com sistema.
- `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` — cauda do comando.
- `app/chamados/services/explicador.py:137` — resposta parcial concorrente com C7.
- `app/chamados/relatorios_disputa.py:20` — `PRONTA_FECHAR` derivado apenas de ausência de vivas.

**F3 — testes existentes propostos:** `app/chamados/tests/test_c7_red_minimos.py:147`; `app/chamados/tests/test_varredura_becos_evento_pergunta.py:47`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:43`. Edição futura: **[nao verificado]**.

**F4 — produção:**

- `app/chamados/models.py:3441` — identidade persistida e consultas por instância (`:2753`).
- `app/chamados/services/declaracao_texto.py:49` — perda do índice da pausa.
- `app/chamados/services/disputa_emissao.py:823` — emissão das perguntas dos marcos.
- `app/chamados/services/fabrica_da_celula.py:145` — deduplicação e emissão por motivo/dia.
- `app/chamados/services/fio.py:1460` — identidade MARCO inclui instância.
- `app/chamados/services/materializacao.py:1163` — unidade individual e lote (`:1381`).
- `app/chamados/services/validacao.py:461` — aplicação individual anterior ao lote.
- `app/chamados/services/passivo_validado.py:516` — outro consumidor da unidade individual.
- `app/chamados/juizes.py:957` — leitores existentes de lâmpada/concordância da pausa (`:1334`).

**F4 — testes existentes propostos:** `app/chamados/tests/test_e1_v5_indice.py:121`; `app/chamados/tests/test_o261_paridade_respeita_a_lampada.py:174`; `app/chamados/tests/test_validar_materializa_wiring.py:51`. Edição futura: **[nao verificado]**.

A fonte dos pares já existe em `app/escala/models.py:1652`, e o enumerador de instâncias já existe em `app/chamados/services/tarefa_do_dia.py:58`; serem dependências não demonstra necessidade de edição. Documentação, migrations e novos arquivos do futuro diff: **[nao verificado]**.

A interseção dos perímetros propostos é **`app/chamados/models.py`, `app/chamados/services/materializacao.py` e `app/chamados/services/disputa_emissao.py`**; portanto, essa reserva não permite tratar F3/F4 como obras disjuntas. (`app/chamados/models.py:3103`, `app/chamados/services/materializacao.py:1163`, `app/chamados/services/disputa_emissao.py:823`; previsão de edição: [nao verificado].)

6. **Fecho de lei:** permanece somente D8, explicitamente aberta no plano de retratação; a identidade da pausa foi decidida pelo aval recebido. (`app/chamados/services/fio.py:1275`; aval sem arquivo local numerado: [nao verificado].)

perguntas de lei: 1

## 11/11 13:30 · sessao d0eecec3bfe2 · - · - · origem=Ronald (tmux)

Escapes deduzidos, sem executar:

1. **Continuação que transforma referência em chamada: rc 0.** O `Attribute` continua na linha antiga; o `Call` que cruza a nova linha não é candidato. (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:66`, `bin/selo_proporcao.py:106`, `bin/selo_proporcao.py:119`)

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,2 +1,3 @@
 (obj.save
+()
 )
```

2. **`exec` e `eval`: rc 0.** A string completa não é exatamente `"save"` nem SQL; nomes `exec`/`eval` não pertencem a `SIGNAL`. (`bin/selo_proporcao.py:57`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:119`)

```diff
+exec("obj.save()")
```

```diff
+eval("obj.save()")
```

3. **F-string com nome calculado: rc 0.** Os fragmentos constantes são `"sa"` e `"ve"`, nenhum deles reconhecido. (`bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:119`)

```diff
+getattr(obj, f"{'sa'}ve")()
```

4. **`setattr` com alias novo: rc 0.** Nem `"acao"` nem os fragmentos `"sa"`/`"ve"` são reconhecidos; a chamada final usa atributo `acao`. (`bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:119`)

```diff
+setattr(obj, "acao", getattr(obj, "sa" + "ve"))
+obj.acao()
```

5. **Cursor por alias com SQL fragmentado: rc 0.** O detector não acompanha o alias nem monta `BinOp`; nenhum fragmento isolado casa com SQL. Alias sozinho com `"DELETE FROM x"` daria rc 1. (`bin/selo_proporcao.py:31`, `bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:119`)

```diff
+executar = cursor.execute
+executar("DELETE " + "FROM x")
```

6. **Exclusão de testes continua aberta: rc 0.** Se produção já importa `gravar` de `.test_writer`, basta mudar somente o helper excluído: o import antigo não será examinado. Isso introduz escrita nova; não é helper já escritor. (`bin/selo_proporcao.py:18`, `bin/selo_proporcao.py:42`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:119`)

```diff
--- a/app/x/test_writer.py
+++ b/app/x/test_writer.py
@@ -1,2 +1,2 @@
 def gravar(obj):
-    pass
+    obj.save()
```

**Import dinâmico novo também não fecha essa exclusão:** o detector só reconhece `ast.Import`/`ast.ImportFrom`; a string do módulo não é nome de escrita. (`bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:61`, `bin/selo_proporcao.py:98`)

```diff
+__import__("app.x.test_writer", fromlist=["gravar"]).gravar(obj)
```

7. **`import *` por reexportação neutra: rc 0**, sob preparação explícita: `app/x/__init__.py` já contém `from .tests.fixt import gravar`; `fixt.py` antes tinha `pass`. A alteração do teste é excluída e `from app.x import *` não contém módulo de teste; `gravar` não é `SIGNAL`. (`bin/selo_proporcao.py:57`, `bin/selo_proporcao.py:63`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:119`)

```diff
--- a/app/x/tests/fixt.py
+++ b/app/x/tests/fixt.py
@@ -1 +1 @@
-def gravar(obj): pass
+def gravar(obj): obj.save()
--- /dev/null
+++ b/app/x/use.py
@@ -0,0 +1,2 @@
+from app.x import *
+gravar(obj)
```

8. **Decorador ativa escrita antiga: rc 0.** A adição cria somente `Lambda`/`Call`/nomes não reconhecidos; o `save` do corpo permanece em linha antiga. Pode ser enquadrado no limite de helper preexistente, embora a ativação no carregamento seja nova. (`bin/selo_proporcao.py:19`, `bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:106`, `bin/selo_proporcao.py:119`)

```diff
+@(lambda f: f())
 def gravar():
     obj.save()
```

Para decorador sem helper escritor preexistente, mas compartilhando o mecanismo de nome calculado: (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:119`)

```diff
+@(lambda f: getattr(obj, "sa" + "ve")())
+def f(): pass
```

9. **Arquivo `.pyw` ou `.sql` carregado por novo `.py`: rc 0.** As extensões são excluídas; `exec`, `open`, `read` e `execute` não são nomes reconhecidos. (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/run.py
@@ -0,0 +1 @@
+exec(open(__file__ + "w").read())
--- /dev/null
+++ b/app/x/run.pyw
@@ -0,0 +1 @@
+obj.save()
```

```diff
--- /dev/null
+++ b/app/x/run.py
@@ -0,0 +1 @@
+cursor.execute(open(__file__ + ".sql").read())
--- /dev/null
+++ b/app/x/run.py.sql
@@ -0,0 +1 @@
+DELETE FROM x;
```

10. **Template:** `.html` é ignorado e o backend configurado é Django. `{{ obj.gravar }}` pode chamar método customizado existente que escreve; contexto, existência e execução reais são **[nao verificado]** e esse caso pode cair no limite de helper aceito. Não usar `{{ obj.save }}`/`{{ obj.delete }}` como prova automática: `alters_data` pode impedir a execução. (`bin/selo_proporcao.py:19`, `bin/selo_proporcao.py:98`, `app/config/settings/base.py:72`; comportamento concreto de `alters_data`: [nao verificado])

Precisão terminológica: na continuação há nós AST (`Call`/`Expr`) cruzando a linha adicionada; o escape é que **nenhum nó reconhecido por `nome_de_escrita`** cruza essa linha. (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:104`, `bin/selo_proporcao.py:106`)

## 11/11 13:30 · sessao 37302d31d8ea · - · - · origem=Ronald (tmux)

Censo de chamadores por AST dos `.py` de `app/`, excluídos testes e migrations; as referências abaixo são os sítios reais de chamada. [nao verificado]

**Portas e alcance**

- O cartório chama diretamente `ChamadoColaborador.reconciliar()` em seis sítios: adiados depois da lavra/E2 (`app/ponto/services/cartorio.py:998`); marcos acesos ou dia sem trabalho (`app/ponto/services/cartorio.py:1525`); marcos inexistentes (`app/ponto/services/cartorio.py:1538`); protesto CSF ou cobrança em dia sem trabalho (`app/ponto/services/cartorio.py:1559`); FIO-DUP (`app/ponto/services/cartorio.py:1637`); FIO-DUP-C8 (`app/ponto/services/cartorio.py:1652`).
- Os quatro primeiros não consultam o predicado de conversa do supra; os dois últimos usam somente a guarda local de texto `any((q.resposta_colab or '').strip()...)`, universo `perguntas_que_resolvem`, definida em `app/ponto/services/cartorio.py:1623`, aplicada em `app/ponto/services/cartorio.py:1634` e `app/ponto/services/cartorio.py:1651`.
- A porta comum entre evento e supra é `reconciliar`, não `retratar`: o cartório chama a primeira diretamente; o supra chama `retratar`, que delega a `_retratar`, que chama `reconciliar`. (`app/ponto/services/cartorio.py:1559`; `app/ponto/management/commands/supra_juiz.py:286`; `app/chamados/reconciliador.py:544`; `app/chamados/reconciliador.py:523`.)
- `reconciliar` consulta `_decisao_do_fechamento` e escreve via `_gravar_estado`; as guardas atuais são estado permitido, premissa regularizada/dedup e pergunta viva, não o predicado histórico de conversa do supra. (`app/chamados/models.py:641`; `app/chamados/models.py:663`; `app/chamados/models.py:682`; `app/chamados/models.py:685`; `app/chamados/models.py:654`.)
- `_decisao_do_fechamento` tem exatamente dois chamadores internos: `reconciliar(registrar=True)` e `reconciliaria(registrar=False)`. (`app/chamados/models.py:651`; `app/chamados/models.py:661`.)
- Os leitores externos de `reconciliaria` são o balde do lastro, o vigia `reconciliar_chamados` e o dry/alarme de `detectar_intervalo_ausente`. Uma guarda dentro de `_decisao_do_fechamento` muda também essas leituras. (`app/ponto/services/lastro.py:318`; `app/chamados/management/commands/reconciliar_chamados.py:79`; `app/ponto/management/commands/detectar_intervalo_ausente.py:138`.)

**Todos os chamadores diretos adicionais de `reconciliar`**

| Arquivo | Função | Linha(s) |
|---|---|---|
| `app/chamados/signals.py` | `disputa_fechada_fecha_chamado_pai` | 42 |
| `app/chamados/reconciliador.py` | `_retratar` | 523 |
| `app/chamados/services/adesao.py` | `sincronizar_adesao` | 115, 150 |
| `app/chamados/services/feriado_reavaliar.py` | `encerrar` | 47 |
| `app/chamados/services/limbo_folgas.py` | `aceitar_sugestao` | 101 |
| `app/chamados/services/limbo_folgas.py` | `agrupar_furo` | 283, 286 |
| `app/chamados/services/silencio_ausencia.py` | `reconciliar_por_ausencia` | 101 |
| `app/chamados/services/materializacao.py` | `_materializar_ausencia_de_resposta` | 587 |
| `app/chamados/services/materializacao.py` | `_reconciliar_destino_e_clones` / `_fecha` | 776 |
| `app/chamados/management/commands/auditar_chamados_orfaos.py` | `handle` | 67 |
| `app/colaboradores/services/pos_vinculo.py` | `fechar_adesao_com_batida` | 42 |
| `app/colaboradores/management/commands/marcar_feriado_do_posto.py` | `_marcar` | 140 |
| `app/ponto/services/triagem_batida.py` | `_autoresolve_f2_f4` | 597 |
| `app/ponto/services/veredito_celula.py` | `fechar_o_fio_do_dia` | 202 |
| `app/ponto/services/ausencia.py` | `fechar_chamado_da_ausencia` | 694 |
| `app/ponto/services/cluster_espurio.py` | `reconciliar_colab` | 189 |
| `app/ponto/services/entrada_sincronizada.py` | `resolver_cobranca_da_entrada` | 31 |
| `app/ponto/services/cobertura.py` | `designar_cobertura` | 122 |
| `app/ponto/services/cobertura.py` | `declarar_cobertura_passada` | 317 |
| `app/ponto/services/lastro.py` | `fechar_pelo_marco_colab` | 400 |
| `app/ponto/management/commands/detectar_intervalo_ausente.py` | `handle` | 138 |
| `app/ponto/management/commands/curar_residuo_bug115.py` | `_ata_sem_furo` | 151 |
| `app/ponto/management/commands/detectar_ausencias.py` | `handle` | 158 |
| `app/ponto/management/commands/detectar_ausencias.py` | `_fechar_por_afastamento` | 824 |
| `app/ponto/management/commands/detectar_ausencias.py` | `_fechar_por_cobertura` | 834 |
| `app/ponto/management/commands/detectar_ausencias.py` | `_fechar_chamado` | 845 |

**Se a guarda entrar em `retratar` ou `_retratar`**

`retratar` tem estes oito chamadores externos; entrar somente nesse wrapper público não alcança o cartório nem os dois chamadores internos diretos de `_retratar`. (`app/chamados/reconciliador.py:544`; `app/ponto/services/cartorio.py:1559`; `app/chamados/reconciliador.py:1004`; `app/chamados/reconciliador.py:1464`.)

| Arquivo | Função | Linha |
|---|---|---|
| `app/ponto/services/justificativa.py` | `encerrar_chamado_de_origem` | 46 |
| `app/ponto/services/geofence.py` | `reavaliar_chamados_geofence` | 441 |
| `app/ponto/management/commands/supra_juiz.py` | `handle` | 286 |
| `app/ponto/management/commands/reconciliar_grade.py` | `handle` | 309 |
| `app/ponto/management/commands/reconciliar_fantasmas.py` | `handle` | 280 |
| `app/ponto/management/commands/reconciliar_vinculo.py` | `handle` | 68 |
| `app/ponto/management/commands/processar_alertas_turno.py` | `_resolver_chamado_do_turno` | 216 |
| `app/chamados/management/commands/retratar_chamados_lei_cala.py` | `handle` | 59 |

Dentro de `_retratar`, alcançaria também `retratar_chamados_absolvidos_por_celula` e `retratar_premissa_morta_colab`, além desses oito caminhos. (`app/chamados/reconciliador.py:1004`; `app/chamados/reconciliador.py:1464`.)

**Se entrar no escritor mais baixo**

`_gravar_estado` é o escritor de `status_local`, `resolvido_em`, `via_encerramento` e urgência; o evento de auditoria conserva status anterior, status novo e via, sem indicar “cartório/CSF”. (`app/chamados/models.py:201`; `app/chamados/models.py:228`; `app/chamados/models.py:242`; `app/chamados/models.py:292`.)

Se guardado sem recorte, alcança sete entradas distintas, incluindo transições que nem são encerramento:

| Chamador interno | Referência |
|---|---|
| `_aplicar` — transição humana | `app/chamados/models.py:450` |
| `reconciliar` | `app/chamados/models.py:654` |
| `cancelar` | `app/chamados/models.py:788` |
| `analisar` | `app/chamados/models.py:806` |
| `reabrir` | `app/chamados/models.py:1442` |
| `encerrar` | `app/chamados/models.py:1619` |
| `encerrar_por_competencia_trancada` | `app/chamados/models.py:1639` |

`encerrar` é outra porta: usa evento `ENCERRAR`, vai a `fechado`, consulta pergunta viva e não usa `_decisao_do_fechamento`; o CSF do cartório e do supra não a chama diretamente. (`app/chamados/models.py:1605`; `app/chamados/models.py:1614`; `app/chamados/models.py:1617`; `app/ponto/services/cartorio.py:1559`; `app/chamados/reconciliador.py:523`.)

**Caminhos indiretos do cartório**

- E2 no cartório carimba perguntas; depois fecha disputas; o signal da disputa fecha o chamado-pai por `reconciliar`. Nenhum desses pontos consulta `pode_executar_csf`. (`app/ponto/services/cartorio.py:993`; `app/chamados/reconciliador.py:595`; `app/chamados/reconciliador.py:611`; `app/chamados/reconciliador.py:612`; `app/chamados/signals.py:42`.)
- O cartório chama também o reconciliador de cluster, que fecha chamados por outro motivo usando a mesma porta. (`app/ponto/services/cartorio.py:1028`; `app/ponto/services/cartorio.py:1029`; `app/ponto/services/cluster_espurio.py:189`.)
- Quando um chamado deixa de ser vivo, o signal `_filho_morto_acorda_o_pai` pode registrar `on_commit` e chamar `reconciliar_chamados_colab`; este executa E2, órfãs e premissa morta. Não existe chamada explícita final dessa função dentro de `julgar_colab`. (`app/chamados/signals.py:350`; `app/chamados/signals.py:368`; `app/chamados/signals.py:369`; `app/chamados/reconciliador.py:1432`; `app/chamados/reconciliador.py:1433`; `app/chamados/reconciliador.py:1441`; `app/ponto/services/cartorio.py:1085`.)
- A lavra gera `EventoCelula`; o consumidor pode fechar por absolvição da célula, premissa morta e lastro. Esse caminho também passa pela porta geral sem a guarda de conversa do supra. (`app/ponto/portas/celula.py:796`; `app/ponto/portas/celula.py:896`; `app/chamados/reconciliador.py:1368`; `app/chamados/reconciliador.py:1372`; `app/chamados/reconciliador.py:1383`; `app/ponto/services/lastro.py:400`.)

Assim, uma guarda global na porta geral muda os chamadores da tabela; uma guarda nos chamadores cartório/supra deixa esses outros usos com o comportamento atual, mas precisa cobrir os vários sítios do cartório e explicitar se o consumidor pós-lavra integra o alcance pretendido. (`app/chamados/models.py:641`; `app/ponto/services/cartorio.py:998`; `app/ponto/services/cartorio.py:1525`; `app/ponto/services/cartorio.py:1559`; `app/chamados/reconciliador.py:1368`.)

**Testes e selos relevantes** [nao verificado]

| Cobertura | Referência |
|---|---|
| Cartório: CSF aparece na sombra, chamado fecha na aplicação, fio não ressuscita | `app/ponto/tests/test_o261_cobrado_le_o_fio.py:393`; asserts em `:412`, `:416`, `:420` |
| Evento real `registrar_batida` → cartório → E2 → chamado encerrado, sem estado terminal com pergunta viva | `app/chamados/tests/test_o261e_f1_porta_encerramento.py:358`; asserts em `:390`, `:392`, `:395` |
| Guarda geral recusa pergunta viva por destino/container | `app/chamados/tests/test_o261e_f1_porta_encerramento.py:82`; `:94` |
| Guarda geral permite reata ao sobrevivente e mantém dry sem escrita | `app/chamados/tests/test_o261e_f1_porta_encerramento.py:135`; `:143`; `:152` |
| Cobrança de marco satisfeito fecha mesmo com outro furo no dia, atualmente via consumidor | `app/ponto/tests/test_bug115_lampada_do_marco.py:185`; chamada consumidor `:192`; assert `:205` |
| Marco ainda apagado mantém cobrança viva | `app/ponto/tests/test_bug115_lampada_do_marco.py:207`; assert `:221` |
| Universo do cartório: módulo de furo, vocabulário do supra, data declarada | `app/ponto/tests/test_cartorio_so_chamado_de_batida.py:53`; `:63`; `:74` |
| Resposta escrita não ressuscita fio — não testa preservação CSF | `app/ponto/tests/test_o261_cobrado_le_o_fio.py:341` |
| Selo custo real da batida com um/três chamados encerrando | `app/ponto/tests/test_selo_chokepoint_escrita.py:387`; `:392` |
| Selo custo real da batida com um/três chamados permanecendo | `app/ponto/tests/test_selo_chokepoint_escrita.py:422`; `:427` |
| Selo custo constante da guarda no caminho real | `app/ponto/tests/test_selo_chokepoint_escrita.py:432` |
| Selo teto de `on_commit` da batida | `app/ponto/tests/test_selo_chokepoint_escrita.py:201`; cobrança `:242` |

Nenhum resultado de execução desses testes foi obtido neste censo; apenas o conteúdo foi lido. [nao verificado] [nao verificado]

## 11/11 13:29 · sessao c4906477109b · - · - · origem=Ronald (tmux)

Resultados confirmados por execução em memória com `python3 -B`, sem escrever arquivos; o rc referido abaixo é o equivalente ao retorno de `problemas()` (`bin/mapa20_valida.py:221`, `bin/mapa20_valida.py:225`).

| Entrada | Juiz e operação | Resultado | Prova existente |
|---|---|---|---|
| Tabela válida | Forma/cabeçalho/universo, células e X/N (`bin/mapa20_valida.py:135`, `bin/mapa20_valida.py:154`, `bin/mapa20_valida.py:166`, `bin/mapa20_valida.py:202`) | Fixture verde: rc 0 | `bin/tests/test_mapa20_valida.sh:42`; completude/vazias: `bin/tests/test_mapa20_valida.sh:45` |
| `encerrada` com X<N em célula de obra | `_afirma_encerrada()` e guarda de completude (`bin/mapa20_valida.py:68`, `bin/mapa20_valida.py:200`) | rc 1 | `bin/tests/test_mapa20_valida.sh:55`; AUSENTE encerrada: `bin/tests/test_mapa20_valida.sh:56` |
| `encerrada` fora da tabela: negrito, ATX, setext, lista | SEM juiz de estado para a prosa: iteração restringe-se ao bloco da tabela (`bin/mapa20_valida.py:147`, `bin/mapa20_valida.py:166`, `bin/mapa20_valida.py:200`) | Quatro contracasos em memória: rc 0 | SEM PROVA de regressão na cópia; os casos de segunda tabela tratam pipes, não prosa (`bin/tests/test_mapa20_valida.sh:62`, `bin/tests/test_mapa20_valida.sh:63`) |
| Invisível Cf | Recusa categoria Cf no texto inteiro (`bin/mapa20_valida.py:132`) | U+200B: rc 1 | `bin/tests/test_mapa20_valida.sh:71` |
| Combinantes Mn/homóglifo | SEM juiz dessas classes: só Cf é recusado; estado usa regex literal (`bin/mapa20_valida.py:58`, `bin/mapa20_valida.py:132`) | `enc`+U+034F+`errada`, U+FE00 equivalente, `еncerrada` cirílico: rc 0 com 1 de 2 | SEM PROVA de regressão na cópia (`bin/tests/test_mapa20_valida.sh:67`, `bin/tests/test_mapa20_valida.sh:71`) |
| FF, FS, U+2028 unindo E3/E4B | `splitlines()` fabrica linhas lógicas; SEM juiz da divergência com linha física (`bin/mapa20_valida.py:125`) | Três contracasos em memória: rc 0 | SEM PROVA de regressão na cópia; o teste de foto verifica mesma variável, não separadores (`bin/tests/test_mapa20_valida.sh:206`) |
| N com fonte fora de aval/TICKETS/BACKLOG | `_fonte_ruim()` só exige citação existente, contida na raiz e linha não vazia (`bin/mapa20_valida.py:73`, `bin/mapa20_valida.py:78`, `bin/mapa20_valida.py:88`) | `bin/relato.sh:2`: rc 0 | SEM PROVA de elegibilidade; teste verde legitima a fonte errada em `bin/tests/test_mapa20_valida.sh:27`, `bin/tests/test_mapa20_valida.sh:42`; testes negativos só existência/raiz em `bin/tests/test_mapa20_valida.sh:107` |
| SEM DONO | Exige `n/a (SEM DONO)` e sai antes da guarda de encerrada (`bin/mapa20_valida.py:182`, `bin/mapa20_valida.py:185`, `bin/mapa20_valida.py:200`) | SEM DONO com `encerrada`: rc 0 | Ausência/vazia: `bin/tests/test_mapa20_valida.sh:45`; X/N indevido: `bin/tests/test_mapa20_valida.sh:72`; estado encerrada: SEM PROVA |
| RASCUNHO | SEM juiz específico; prosa sem pipes pode ficar fora da tabela (`bin/mapa20_valida.py:137`, `bin/mapa20_valida.py:144`, `bin/mapa20_valida.py:166`) | Prefixar `RASCUNHO` a mapa válido: rc 0 | SEM PROVA; mapa vivo RASCUNHO do anexo: [nao verificado] na cópia |
| Legenda literal `montar_mapa.py:103` | Arquivo ausente na cópia; literal não fornecido integralmente | [nao verificado] | SEM PROVA; não inventar a legenda a partir do comentário do juiz (`bin/mapa20_valida.py:128`) |

Detalhes adicionais relevantes:

- Não há conferência semântica de X contra prova no RELATO: a implementação calcula completude comparando os dois inteiros e confere apenas a fonte de N (`bin/mapa20_valida.py:190`, `bin/mapa20_valida.py:192`).
- A fixture pública usa `a.md:1` com conteúdo literal `fonte`; portanto ela prova o circuito de publicação, não a lei de origem/contagem de N (`bin/tests/test_mapa20_valida.sh:133`, `bin/tests/test_mapa20_valida.sh:148`).
- `status/encerrada.sim` é removido como caminho e passa com X<N (`bin/mapa20_valida.py:52`, `bin/mapa20_valida.py:65`). O caso local existente é `logs/encerrada.md:10`, em `bin/tests/test_mapa20_valida.sh:102`; o `test:414` citado no opus não corresponde à numeração desta cópia [nao verificado].
- Numeração local a usar: `splitlines()` em `bin/mapa20_valida.py:125`; Cf em `bin/mapa20_valida.py:132`; fonte em `bin/mapa20_valida.py:73`; SEM DONO `continue` em `bin/mapa20_valida.py:185`; guarda encerrada em `bin/mapa20_valida.py:200`.

## 11/11 13:29 · sessao 3e14c3367581 · - · - · origem=Ronald (tmux)

Escritores comprováveis:

| Origem | Evidência |
|---|---|
| `montar_mapa.py` | Atribuído como escritor pelo próprio validador; implementação e linha de gravação ausentes da cópia: [nao verificado]. (`bin/mapa20_valida.py:10`, `bin/mapa20_valida.py:128`) |
| Edição pela sessão `PRONTO-AUSENTE-RASCUNHO` | Informada no item 5 do anexo `opus_final_ffa_achados.txt`; comando, autoria individual e linha da gravação: [nao verificado]. |
| Outro escritor de produção | [nao verificado]. |
| Fixture de regravação | Grava `REGRAVADO` no mapa do repositório temporário de teste. (`bin/tests/test_mapa20_valida.sh:146`, `bin/tests/test_mapa20_valida.sh:154`) |
| Fixtures verde/vermelha | Copiam ou escrevem mapa no repositório temporário de teste. (`bin/tests/test_mapa20_valida.sh:177`, `bin/tests/test_mapa20_valida.sh:178`) |

O publicador lê o mapa vivo e faz uma foto; essa operação não escreve no mapa vivo. (`bin/relato.sh:97`)

A premissa de escritor único é contradita pelo dado fornecido, que relata montador e edição externa; confirmação independente na cópia: [nao verificado]. O comentário que fundamenta a forma rígida continua afirmando exclusividade do montador. (`bin/mapa20_valida.py:10`)

Nos dois avais fornecidos, há obrigação de regravar por evento e de julgar na publicação, mas nenhuma escolha literal de escritor exclusivo; concluir que a lei escolheu o montador seria [nao verificado]. A pergunta de lei é: **quem fica como único escritor autorizado do mapa?** A escolha não deve ser feita pelo censo. (`bin/relato.sh:90`, `bin/mapa20_valida.py:10`; avais fornecidos, item 3 do primeiro e item 4 do complemento.)

A legenda de `montar_mapa.py:103` é mencionada no anexo, mas seu literal não foi fornecido e o arquivo está ausente; conteúdo, aceitação e teste específico: [nao verificado]. [nao verificado]

Ordem de dependência proposta, sem implementar:

1. Escolher o escritor autorizado e sua forma de saída; o juiz atual fundamenta a forma nessa escolha. (`bin/mapa20_valida.py:10`)
2. Ancorar o juiz no literal dos avais, cobrindo conteúdo visível fora da tabela, caracteres, separadores, fonte de N e estados especiais; hoje o conteúdo julgado restringe-se ao bloco da tabela e `SEM DONO` sai antes da checagem de encerramento. (`bin/mapa20_valida.py:125`, `bin/mapa20_valida.py:132`, `bin/mapa20_valida.py:166`, `bin/mapa20_valida.py:182`, `bin/mapa20_valida.py:200`)
3. Ancorar a publicação no artefato final julgado; hoje a raspagem ocorre depois da validação. (`bin/relato.sh:97`, `bin/relato.sh:127`)
4. Ancorar o chamador num resultado distinguível; o serviço apenas declara `ExecStart` e o publicador usa rc 1 tanto para retenção quanto para falhas. (`bin/hasner-relato.service:15`, `bin/relato.sh:100`, `bin/relato.sh:110`)
5. Fazer os testes morderem essas transições; os atuais testes de publicação usam raspador substituto. (`bin/tests/test_mapa20_valida.sh:158`, `bin/tests/test_mapa20_valida.sh:162`)

Lista candidata de arquivos da cura, sujeita à escolha do escritor: `logs/MAPA-20/montar_mapa.py`, `logs/MAPA-20/mapa.md`, `bin/mapa20_valida.py`, `bin/relato.sh`, `bin/tests/test_mapa20_valida.sh`, `bin/hasner-relato.service`. Os dois primeiros não estão disponíveis: [nao verificado]. Uma lista fechada incondicional antes de decidir o escritor seria [nao verificado].

## 11/11 13:29 · sessao a8335eaf93cf · - · - · origem=Ronald (tmux)

Transições verificadas na cópia:

| Juiz/operação | Resultado público previsto pelo código | rc | Prova existente |
|---|---|---|---|
| Foto e validador: `bin/relato.sh:97` | Aceito: foto entra no par de raspagem, depois no `git add`, commit e push (`bin/relato.sh:98`, `bin/relato.sh:143`, `bin/relato.sh:153`, `bin/relato.sh:163`, `bin/relato.sh:164`). | 0 se resto concluir; falha posterior também retorna 1 (`bin/relato.sh:164`, `bin/relato.sh:165`). | `verde_novo`, `verde_so_mapa`: `bin/tests/test_mapa20_valida.sh:196`, `bin/tests/test_mapa20_valida.sh:197`. |
| Mapa recusado/ausente: `bin/relato.sh:100` | Novo mapa excluído; antigo, se existir, é raspado e renomeado; resto segue (`bin/relato.sh:102`, `bin/relato.sh:105`, `bin/relato.sh:106`). | 1 mesmo com publicação do restante ou nenhuma mudança (`bin/relato.sh:158`, `bin/relato.sh:165`). | `vermelho_resto`, `vermelho_nada`, `ausente`: `bin/tests/test_mapa20_valida.sh:198`, `bin/tests/test_mapa20_valida.sh:199`, `bin/tests/test_mapa20_valida.sh:200`. |
| Raspagem/rename do retido falha: `bin/relato.sh:105` | Apaga cópia local antiga e adiciona remoção ao commit, caso consiga removê-la (`bin/relato.sh:109`, `bin/relato.sh:152`, `bin/relato.sh:153`). | 1; resto segue se remoção concluir (`bin/relato.sh:110`, `bin/relato.sh:165`). | `raspa_falha`: `bin/tests/test_mapa20_valida.sh:201`. |
| Remoção também falha: `bin/relato.sh:110` | Interrompe antes do commit/push; remoto conserva estado anterior (`bin/relato.sh:110`, `bin/relato.sh:163`, `bin/relato.sh:164`). | 1 (`bin/relato.sh:110`). | `rm_falha`: `bin/tests/test_mapa20_valida.sh:204`. |
| Escritor altera vivo depois do selo: `bin/tests/test_mapa20_valida.sh:154` | Publicação usa foto já capturada (`bin/relato.sh:97`, `bin/relato.sh:98`). | 0 no caso sintético (`bin/tests/test_mapa20_valida.sh:203`). | `regravado`: `bin/tests/test_mapa20_valida.sh:149`, `bin/tests/test_mapa20_valida.sh:203`. |
| Commit falha: `bin/relato.sh:163` | Não alcança push (`bin/relato.sh:163`, `bin/relato.sh:164`). | 1 (`bin/relato.sh:163`). | `commit_falha`: `bin/tests/test_mapa20_valida.sh:202`. |
| Push falha: `bin/relato.sh:164` | Atualização no remoto não demonstrada [nao verificado]. | 1 (`bin/relato.sh:164`). | **SEM PROVA específica**: lista dos nove casos em `bin/tests/test_mapa20_valida.sh:196` até `bin/tests/test_mapa20_valida.sh:204`. |

**Buraco entre julgamento e publicação:** o raspador transforma texto em `bin/raspar_publico.py:31`, confere somente nomes/CPF em `bin/raspar_publico.py:36`, grava em `bin/raspar_publico.py:40`; a chamada acontece depois do julgamento (`bin/relato.sh:97`, `bin/relato.sh:129`). Não há julgamento de conformidade MAPA-20 desses bytes depois da transformação antes do commit (`bin/relato.sh:143`, `bin/relato.sh:163`). O teste usa raspador falso que prefixa `RASPADO:` e compara esse texto, sem validar novamente o resultado (`bin/tests/test_mapa20_valida.sh:158`, `bin/tests/test_mapa20_valida.sh:166`, `bin/tests/test_mapa20_valida.sh:189`). A consequência concreta da raspagem real no mapa vivo depende dos nomes e do mapa, ausentes [nao verificado].

**Leitores do rc:**

- `hasner-relato.service` entrega a execução ao systemd por `ExecStart`, com `Type=oneshot` (`bin/hasner-relato.service:13`, `bin/hasner-relato.service:15`). O arquivo não configura ação específica para MAPA retido, `OnFailure`, `Restart` ou discriminação de rc; termina em comentários depois da afinidade (`bin/hasner-relato.service:19`, `bin/hasner-relato.service:21`). Estado efetivo da unit instalada/monitoramento externo [nao verificado].
- `hasner-relato.timer` fornece agendamento periódico; não lê nem discrimina o rc (`bin/hasner-relato.timer:5`, `bin/hasner-relato.timer:6`, `bin/hasner-relato.timer:8`).
- O teste executa **recortes**, captura `$?`, lê e compara o resultado (`bin/tests/test_mapa20_valida.sh:138`, `bin/tests/test_mapa20_valida.sh:182`, `bin/tests/test_mapa20_valida.sh:183`, `bin/tests/test_mapa20_valida.sh:187`, `bin/tests/test_mapa20_valida.sh:191`).
- Outros chamadores produtivos, aliases, units instaladas ou pessoas lendo o rc [nao verificado]. Os demais arquivos encontrados mencionam o publicador ou verificam sua costura; exemplo: teste apenas procura a chamada ao checador em `bin/tests/test_relato_afirma_com_prova.sh:44`; ele não lê o rc do publicador.

**Ambiguidade:** rc 1 representa tanto publicação parcial bem-sucedida com MAPA retido (`bin/relato.sh:100`, `bin/relato.sh:165`) quanto interrupções antes de publicar (`bin/relato.sh:28`, `bin/relato.sh:32`, `bin/relato.sh:110`, `bin/relato.sh:163`) ou falha de push (`bin/relato.sh:164`). O rc sozinho não distingue esses resultados (`bin/relato.sh:158`, `bin/relato.sh:165`).

## 11/11 13:29 · sessao d5f84499206c · - · - · origem=Ronald (tmux)

Não é possível certificar uma lista futura **FECHADA** apenas pelo código atual: escolher onde pôr a reação da raiz, como transportar o autor e como preservar a instância da pausa são decisões de implementação ainda não materializadas em diff. [nao verificado]

Para um brief com perímetro conservador **proposto**, os arquivos concretos são:

| F3 — produção | Razão verificável |
|---|---|
| `app/chamados/models.py` | Porta de fecho e trilha; carimbo normal e identidade. `:1855`, `:1932`, `:3103`, `:3168`. |
| `app/chamados/signals.py` | Beco agenda fecho sem transportar autor; S143 resolve com usuário sistema. `:228`, `:237`, `:115`. |
| `app/chamados/services/becos.py` | Fecha com parâmetro `sistema`; entrada de evento fornece usuário sistema. `:57`, `:78`. |
| `app/chamados/services/materializacao.py` | Caudas atuais fecham diretamente; necessidade de compatibilidade/idempotência com reação nova da raiz. `:591`, `:1307`, `:1338`. |
| `app/chamados/reconciliador.py` | Três caudas diretas de fecho. `:412`, `:612`, `:1171`. |
| `app/chamados/services/disputa_emissao.py` | Fechos diretos de disputa muda e S128. `:1284`, `:1964`. |
| `app/api/foto_recebida.py` | Valida perguntas antes de chamar `d.fechar`, que pode encontrar disputa já fechada pela reação nova. `:79`, `:80`. |
| `app/ponto/services/entrada_sincronizada.py` | Fecho direto com sistema. `:59`. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py` | Cauda direta depois de carimbar perguntas. `:88`. |
| `app/chamados/services/explicador.py` | Ainda responde fechamento por `not fio_vivo`, sem a outra cláusula C7. `:137`. |
| `app/chamados/relatorios_disputa.py` | Ainda emite `PRONTA_FECHAR` apenas por ausência de vivas. `:18`, `:20`. |

F3 — testes existentes adequados para concentrar mudanças: `app/chamados/tests/test_c7_red_minimos.py:147` (`:428` idempotência, `:574` escritor único), `app/chamados/tests/test_varredura_becos_evento_pergunta.py:47` (`:70` hoje exige pai resolvido), `app/chamados/tests/test_o261e_f2_porta_carimbo.py:611`. A alteração futura desses testes é proposta, não fato executado. [nao verificado]

| F4 — produção | Razão verificável |
|---|---|
| `app/chamados/models.py` | Identidade rejeita `instancia_marco` diferente da posição simples derivada do motivo; consultas também derivam posição só pelo motivo. `:3441`, `:3446`, `:2753`. |
| `app/chamados/services/declaracao_texto.py` | Declara perda do índice das pausas; faltantes retornam posição simples e lote agrupa por motivo/dia. `:49`, `:139`, `:582`, `:590`. |
| `app/chamados/services/disputa_emissao.py` | Emissores materializam perguntas dos marcos faltantes. `:823`, `:837`, `:850`, `:875`. |
| `app/chamados/services/fabrica_da_celula.py` | Dedup e emissão atuais operam por motivo/dia. `:145`, `:154`, `:170`, `:172`, `:174`. |
| `app/chamados/services/materializacao.py` | Unidade e lote são individuais; ponto de composição do ato conjunto. `:1163`, `:1225`, `:1381`. |
| `app/chamados/services/validacao.py` | Valida uma pergunta e chama unidade individual antes do lote. `:435`, `:461`, `:463`, `:469`. |
| `app/chamados/services/passivo_validado.py` | Importa e chama unidade individual; não pode deixar caminho alternativo furar unidade do par. `:39`, `:514`, `:516`. |
| `app/chamados/juizes.py` | Leitor de fato do marco ainda verifica todas as pausas para motivos de intervalo; precisa conferir instância efetivamente cobrada. `:930`, `:949`, `:965`. |
| `app/chamados/services/fio.py` | Identidade da família MARCO inclui instância; leitura e migração devem continuar coerentes com seu significado ampliado. `:1373`, `:1460`, `:1495`. |

F4 — testes existentes onde cabe concentrar a mudança: `app/chamados/tests/test_emissao_costura_intervalo.py:50` (`:65` dedup), `app/chamados/tests/test_guarda_paridade_materializador.py:59`, `app/chamados/tests/test_plantio_intervalo_overnight.py:27`, `app/chamados/tests/test_porta_validacao_pergunta.py:224`. Alterações futuras são propostas. [nao verificado]

Dependências de leitura que **não precisam ser declaradas como edição só por serem consumidas**: `app/escala/models.py:1652` fornece pares DNA; `app/escala/services/leitor_celula.py:114` fornece grade; `app/chamados/services/tarefa_do_dia.py:58` e `:92` já enumeram instâncias, e `:109` já aceita instância persistida. Necessidade futura de editá-las: [nao verificado].

Interseção do perímetro conservador proposto F3/F4: `app/chamados/models.py`, `app/chamados/services/materializacao.py`, `app/chamados/services/disputa_emissao.py`. Portanto, nesse perímetro, F3 e F4 não são disjuntas. A propriedade de serem futuras edições continua [nao verificado].

## 11/11 13:29 · sessao 19e32ec7963c · - · - · origem=Ronald (tmux)

Base disponível em `../a026943099b8d88468240fd4b18491141b6810af`; comparação por AST realizada sem importar a aplicação nem executar testes. [nao verificado: identificação do hash sem metadados Git]

Delta pertinente confirmado:

- `editar_ausencia_registro` passou a invalidar o fechamento entre os limites antigo e novo depois do `save`; validação de tipo continua igual. `app/ponto/services/ausencia.py:473-475,496-524`.
- Materialização de ausência retirou minutos fixos e `checar_parcial=False`; as duas chamadas continuam pela criação validada. `app/chamados/services/materializacao.py:474-498,526-534`.
- `cobertura_ausencia_periodo` delega aos novos `_q_cobertura` e `_cobertura_pura`; desconhecido sem fim continua restrito à data inicial. `app/ponto/turnos.py:2752-2783`.
- `lista_ausencias` ganhou decisões pelo catálogo/parcialidade para apresentação. `app/ponto/views.py:2234-2245`.
- Nenhum escritor ou leitor citado desapareceu; exclusões encontradas (`data_saida_esperada`, `_fim_turno_encadeado`) não eram decisões citadas por `Ausencia.tipo`. [nao verificado: constatação negativa da comparação]

Testes que enumeram tipos/catálogos: [nao verificado]

| Arquivo | Evidência |
|---|---|
| `app/ponto/tests/test_catalogo_ausencias.py` | `:10-20,36-42,56-60` |
| `app/ponto/tests/test_cadastro_tipo.py` | `:14-34,40-50` |
| `app/ponto/tests/test_selo_b5f1_porta_ausencia.py` | `:180-194`: exige aceitar tipo vivo fora dos choices |
| `app/api/tests/test_api_ausencias_tipos.py` | `:46-51`: ainda confere GET contra choices |
| `app/ponto/tests/test_aus_f4_criar_pelo_juiz.py` | `:43-54`: enumera válidos fora dos choices |
| `app/ponto/tests/test_fim_separado.py` | `:34-42,55-60` |
| `app/ponto/tests/test_contract_rotulo_x_efeito.py` | `:34-37,69-75` |
| `app/ponto/tests/test_rotulo_x_efeito.py` | `:47-64` |
| `app/ponto/tests/test_rotulo_do_dia_decidido.py` | `:84-92` |
| `app/ponto/tests/test_contract_espelho_sem_emoji.py` | `:271-282` |
| `app/ponto/tests/test_ui3_rotulo_do_espelho.py` | `:25-30,46-64` |
| `app/ponto/tests/test_conjuntos_vivos.py` | `:14-45`: efeitos e exclusões da rubrica |
| `app/ponto/tests/test_catalogo_vivo.py` | `:30-50`: cadastro novo/inativo nos conjuntos |

Esses enumeradores já existiam na base; não foram encontrados novos enumeradores de catálogo entre as adições. [nao verificado: constatação negativa da comparação]

A cura não deve converter a validação para `TIPO_CHOICES`: isso quebraria contrato explícito de aceitação do cadastro vivo. `app/ponto/tests/test_selo_b5f1_porta_ausencia.py:180-194`; `app/ponto/tests/test_cadastro_tipo.py:43-50`.

A L-099 já separa estrutura de cadastro/batida e determina destino de admin para os últimos; não exige uma nova pergunta de lei para essa separação. `app/docs/LEIS.md:122`. A regra de origem e de cadastro já existe em `CLAUDE.md:17-27,32-44`.

## 11/11 13:29 · sessao 6eda4fef642f · - · - · origem=Ronald (tmux)

Auditoria estática; não executei selo nem suíte. [nao verificado] [nao verificado]

| Achado v1 | Cura v2 e caso |
|---|---|
| Oito métodos ORM | `Attribute.attr` independe de espaçamento; os oito nomes estão no conjunto. `bin/selo_proporcao.py:28`, `bin/selo_proporcao.py:55`. Casos independentes no laço: `bin/tests/test_selo_proporcao.sh:28`. |
| Espaço depois do ponto | Curado pelo AST; caso `espaco_no_ponto`. `bin/selo_proporcao.py:55`, `bin/tests/test_selo_proporcao.sh:31`. |
| Quebra antes de `()` | O exemplo v1 é curado; caso `quebra_de_linha` cria ambas as linhas, portanto não prova continuação isolada adicionada. `bin/selo_proporcao.py:106`, `bin/tests/test_selo_proporcao.sh:32`. |
| Alias | Referência ao atributo agora basta; caso `alias`, sem invocação. `bin/selo_proporcao.py:55`, `bin/tests/test_selo_proporcao.sh:33`. |
| `getattr(obj,"save")()` | String exata reconhecida; caso `getattr_string`. `bin/selo_proporcao.py:59`, `bin/tests/test_selo_proporcao.sh:34`. |
| `(obj.save)()` | Reconhecido pelo mesmo `Attribute`; não há caso específico. `bin/selo_proporcao.py:55`, `bin/tests/test_selo_proporcao.sh:31`. |
| `#` em string | AST evita truncamento; `cerquilha_string`. `bin/selo_proporcao.py:101`, `bin/tests/test_selo_proporcao.sh:35`. |
| Signal por alias/getattr | O `.connect` do exemplo v1 é reconhecido; caso isolado `signal_connect`, sem reproduzir exatamente o exemplo. `bin/selo_proporcao.py:30`, `bin/selo_proporcao.py:55`, `bin/tests/test_selo_proporcao.sh:39`. |
| Command por alias | A referência `M.objects.create` é reconhecida; `command_alias` contém apenas a referência, sem chamada. Não existe regra específica de command. `bin/selo_proporcao.py:55`, `bin/tests/test_selo_proporcao.sh:43`. |
| Migration por rename | Lista `--name-status -z -M` lê destino e regra independe de linhas adicionadas; `migration_rename`. `bin/selo_proporcao.py:82`, `bin/selo_proporcao.py:86`, `bin/selo_proporcao.py:94`, `bin/tests/test_selo_proporcao.sh:42`. |
| Binário/`-diff` | Fonte vem de `git show`; linhas usam `--text`; `binario_attr`. `bin/selo_proporcao.py:46`, `bin/selo_proporcao.py:100`, `bin/tests/test_selo_proporcao.sh:44`. |
| Caminho com espaço/Unicode | NUL e `surrogateescape` removem dependência dos cabeçalhos; respectivos casos. Não há caso específico com tabulação. `bin/selo_proporcao.py:82`, `bin/selo_proporcao.py:89`, `bin/tests/test_selo_proporcao.sh:45`, `bin/tests/test_selo_proporcao.sh:46`. |
| Falso cabeçalho `+++` no conteúdo | Parser não lê cabeçalhos de caminho; cura no código, sem caso correspondente. `bin/selo_proporcao.py:82`, `bin/selo_proporcao.py:100`, `bin/tests/test_selo_proporcao.sh:27`. |
| Revisão interpretada como opção | Rejeita prefixo `-` e resolve commits antes do diff; `opcao_como_revisao`. `bin/selo_proporcao.py:78`, `bin/selo_proporcao.py:80`, `bin/tests/test_selo_proporcao.sh:58`. |
| Importação de módulo de teste em produção | Import estático reconhecido; `prod_importa_teste` cobre `test_writer`, sem caso independente para importação de `tests.writer`. Cura limitada a `Import`/`ImportFrom`; carregamento dinâmico fica fora. `bin/selo_proporcao.py:61`, `bin/selo_proporcao.py:63`, `bin/tests/test_selo_proporcao.sh:47`. |

Persistem problemas nos testes: [nao verificado]

- `signal_nome` possui dois gatilhos independentes no mesmo `ImportFrom`: módulo terminando em `signals` e alias `post_save`; remover qualquer um mantém rc 1. `bin/tests/test_selo_proporcao.sh:38`, `bin/selo_proporcao.py:64`.
- `apagou_migration` não apaga migration: cria `app/x/velho2.py`; não testa o ramo `st == 'D'`. `bin/tests/test_selo_proporcao.sh:54`, `bin/selo_proporcao.py:92`.
- O bloco de `bash -e` não verifica `$?`; em seguida, o único rc comparado vem de `bash -c` sem `-e`. Regressão no cenário anunciado pode passar. `bin/tests/test_selo_proporcao.sh:62`, `bin/tests/test_selo_proporcao.sh:63`.
- A função verifica checkout, resultado global do preparo, commit sem `--allow-empty` e HEAD diferente; melhorou a preparação v1. Porém `bash -c "$4"` executa scripts internos sem `-e`, e comandos separados por `;` podem esconder falha anterior. O preparo especial de `bash -e` também ignora falhas de checkout/add/commit. `bin/tests/test_selo_proporcao.sh:20`, `bin/tests/test_selo_proporcao.sh:21`, `bin/tests/test_selo_proporcao.sh:22`, `bin/tests/test_selo_proporcao.sh:44`, `bin/tests/test_selo_proporcao.sh:61`.
- `so_remocao` substitui uma chamada por `pass`; cobre remoção da escrita com adição inócua, não patch exclusivamente de remoção. `bin/tests/test_selo_proporcao.sh:49`.
- As exclusões de fixture agora estão isoladas corretamente; também foram acrescentados `bulk_update` e `get_or_create`. `bin/tests/test_selo_proporcao.sh:28`, `bin/tests/test_selo_proporcao.sh:51`, `bin/tests/test_selo_proporcao.sh:52`.

Sobre rc:

- Erros ordinários dentro de `main`, incluindo git, revisão e parse, viram 2; a fronteira adicional captura outras `Exception`. `bin/selo_proporcao.py:34`, `bin/selo_proporcao.py:70`, `bin/selo_proporcao.py:108`, `bin/selo_proporcao.py:124`.
- Não é universal: o wrapper apenas executa Python, sem normalizar falha de inicialização/execução; Python ausente pode produzir 127, não executável 126, e sinais também escapam. Ocorrência real desses ambientes: [nao verificado]. `bin/selo_proporcao.sh:4`.
- Argumentos excedentes são aceitos e ignorados: só existe limite mínimo, e somente quatro posições são consumidas. Uma chamada `base head B repo lixo` pode produzir 0 para diff inócuo. `bin/selo_proporcao.py:70`, `bin/selo_proporcao.py:73`, `bin/selo_proporcao.py:74`, `bin/selo_proporcao.py:119`.
- Rc 1 significa **achado sintático com classe B**, não escrita real demonstrada; referências e strings também geram achados. `bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:115`.
- O problema v1 de rc interno 3 desaparece: `exec` preserva diretamente o rc Python. Um chamador `bash -e` que encerre nessa chamada recebe 1; a suíte atual não verifica esse caminho. `bin/selo_proporcao.sh:4`, `bin/selo_proporcao.py:117`, `bin/tests/test_selo_proporcao.sh:62`.

## 11/11 13:29 · sessao c92a7eaf4320 · - · - · origem=Ronald (tmux)

Censo dos itens 1 e 5, somente leitura.

- `pode_executar_csf(ch)` retorna `False` se alguma pergunta de **qualquer** disputa em `ch.disputa_origem.all()` ou de `ch.perguntas_que_resolvem.all()` possui `respondida_em is not None`. Retorna `True` nos demais casos, inclusive relações vazias. Não filtra pergunta viva, disputa aberta, texto, validação, materialização, data nem o campo `conversa`. (`app/ponto/supra_juiz.py:539`)
- Único chamador de produção encontrado: comando supra, importação em `app/ponto/management/commands/supra_juiz.py:257` e chamada em `:261`; barrado incrementa `CSF_COM_CONVERSA_PRESERVADO` e recebe `continue`. (`app/ponto/management/commands/supra_juiz.py:261`)
- **Outra função diretamente relacionada à mesma proteção por conversa:** `_tem_resposta_do_colab`, local ao julgamento do cartório, retorna `any((q.resposta_colab or '').strip() for q in perguntas)`. Nos dois leitores, recebe somente `perguntas_que_resolvem.all()`; não examina `disputa_origem` nem exige timestamp. Protege ressuscitação/FIO-DUP depois do bloco CSF. (`app/ponto/services/cartorio.py:1623`, `:1631`, `:1634`, `:1651`)
- Não encontrei terceiro predicado geral de conversa de chamado, nem função `tem_interacao`, após buscas por conversa, diálogo, interação, resposta, Followup e ambas as relações. Ausência em todo o universo: **[não verificado]**; evidência positiva do registro atual são as perguntas declaradas em `app/core/juizes.py:107` e seguintes, que distinguem pergunta viva/fio/disputa, sem declarar a guarda CSF. Mensagem livre de `FollowupChamado` não entra no predicado CSF pelo corpo integral da função. (`app/ponto/supra_juiz.py:543`)

Predicados relacionados, **não equivalentes**:

| Função | Predicado e universo |
|---|---|
| `PerguntaDisputa.respondida` | `respondida_em is not None`; uma pergunta. (`app/chamados/models.py:3496`) |
| `fio_vivo(perguntas)` | `any(pergunta_viva(p))`; coleção inteiramente definida pelo chamador, não exige resposta. (`app/chamados/juizes.py:1488`) |
| `DisputaSupervisao.respostas_sem_veredito` | Perguntas da disputa sem validação/materialização, com timestamp **ou** texto não branco, fora de tranca salvo `ignorar_tranca`; devolve lista. (`app/chamados/models.py:1826`) |
| `DisputaSupervisao.pode_fechar` | Ausência de respostas sem veredito e de fio vivo; disputa. (`app/chamados/models.py:1848`) |
| `irmas_respondidas_sem_consumo(ch)` | Sem materialização/validação, timestamp presente e texto não branco. Universo de crédito: destino é `ch`, ou pai da disputa é `ch` e destino não está vivo. (`app/chamados/juizes.py:1980`, `:2012`) |
| `retencao_da_retratacao(ch)` | Retém pelas irmãs anteriores ou justificativa alheia pendente durante outra aprovação. Não protege toda conversa histórica. (`app/chamados/reconciliador.py:437`) |
| `disputa_viva_do_chamado(ch)` | Primeira disputa aberta diretamente ligada; retém se não tiver perguntas ou tiver fio vivo. (`app/chamados/juizes.py:2371`) |
| `Command._disputa_viva(ch)` do arquivador | Resolve uma disputa aberta por `_disputa_do_chamado`; devolve-a se alguma pergunta for viva; erro produz sentinela conservadora. (`app/chamados/management/commands/arquivar_competencia_encerrada.py:148`) |
| `_disputa_do_chamado(ch)` | Resolve uma disputa aberta, primeiro ligação direta, depois pergunta de destino; não mede conversa. (`app/chamados/services/fio.py:120`) |
| `canonica_do_grupo(perguntas)` | Prioriza pergunta com texto não branco, depois menor PK; universo de perguntas do mesmo fato, não chamado. (`app/chamados/services/fio.py:1225`) |

Testes e selos confirmados: [nao verificado]

| Arquivo | Cobertura |
|---|---|
| `app/ponto/tests/test_supra_juiz_executor.py:38` | Quatro unitários: vazio; pergunta sem timestamp permite; timestamp em disputa barra; timestamp direto barra. |
| `app/ponto/tests/test_supra_juiz_cobranca.py:19` | CSF puro: todas casadas acusa; missing/None/sem chamado vivo/real abaixo do previsto não acusam. |
| `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:89` | Chama `--executar` real. Casos: lavra concorde preserva (`:106`); indevida retrata e conta (`:116`); lavra manda mesmo puro calado (`:124`); sem lavra conta (`:134`); sem código preserva e conta (`:142`); retorno `False` do retratador não vira `CSF_RETRATADO` (`:155`). |
| `app/ponto/tests/test_t61_retratacao_em_seco.py:29` | Constante única, estado vigente `False` (`:40`), chamada de escrita textualmente depois da flag/trava (`:47`), placar travado visível (`:57`), texto do juiz (`:64`), grade da célula (`:76`). |
| `app/ponto/tests/test_cartorio_so_chamado_de_batida.py:53` | Universo do cartório: módulos de batida, paridade com supra (`:63`), chamado sem dia excluído (`:74`). Não testa guarda de conversa no CSF. |
| `app/ponto/tests/test_bug115_lampada_do_marco.py:185` | Fecho parcial pelo consumidor de evento/lastro; satisfeita fecha apesar de outro furo, marco apagado preserva (`:207`). |
| `app/ponto/tests/test_hx_cartorio_ressuscita.py:68` | Resposta textual protege contra ressuscitação, não contra o CSF anterior. |

Os testes acima não demonstram guarda CSF aplicada no cartório; cobertura específica nova será necessária **[não verificado quanto a execução futura]**. Nenhum teste foi executado nesta exploração. [nao verificado]

## 11/11 13:28 · sessao ad57caedd1f2 · - · - · origem=Ronald (tmux)

Não encontrei função que compare duas perguntas e responda se cobram os dois marcos da mesma pausa. O catálogo atual perde o índice da pausa, e o nascimento da pergunta rejeita instância diferente da derivada apenas do motivo. (`app/chamados/services/declaracao_texto.py:49`; `app/chamados/models.py:3438`; `app/chamados/models.py:3446`.)

Varredura AST em memória: `app` e `bin`, excluídos testes, migrations e diretórios gerados; 836 arquivos, 5.369 funções, nenhum erro de parse, 25 candidatos revistos. Evidência de execução sem artefato numerado: [nao verificado]. [nao verificado]

Os candidatos relevantes respondem perguntas diferentes:

| Função/dado | Predicado efetivo |
|---|---|
| `CelulaDia.PARES_PAUSA_DNA` | Declara os pares de chaves `hii/hfi` e `hii2/hfi2`; não compara perguntas. (`app/escala/models.py:1652`.) |
| `EscalaColaborador.pausas_do_dia` | Enumera pares completos do DNA da célula; célula sem marcos retorna vazio. (`app/escala/models.py:1197`; `app/escala/models.py:1207`.) |
| `EscalaColaborador.intervalo_do_dia` | Retorna somente a primeira pausa encontrada pelo método anterior. (`app/escala/models.py:1163`.) |
| `CelulaDia.chaves_do_turno` | Enumera os marcos na ordem canônica do turno. (`app/escala/models.py:1659`.) |
| `tarefa_do_dia._posicoes` | Numera repetições da posição, inclusive `int_ida:2`. (`app/chamados/services/tarefa_do_dia.py:58`; `app/chamados/services/tarefa_do_dia.py:68`.) |
| `tarefa_do_dia.instancias_do_turno` | Expõe instância, posição, tipo, hora, lâmpada e teto; não compara perguntas. (`app/chamados/services/tarefa_do_dia.py:92`; `app/chamados/services/tarefa_do_dia.py:102`.) |
| `fato_do_marco_no_espelho` | Para intervalo, exige acesas todas as lâmpadas daquela posição em todas as pausas cadastradas. (`app/chamados/juizes.py:957`; `app/chamados/juizes.py:971`.) |
| `_resposta_concorda_com_a_luz` | Com mais de uma pausa na posição, retorna `None`: não identifica qual delas a resposta cobra. (`app/chamados/juizes.py:1334`; `app/chamados/juizes.py:1336`.) |

D8 permanece aberta: o comentário declara a lei aberta, e o ramo executável conserva `sem_episodio_intocada`. (`app/chamados/services/fio.py:1274`; `app/chamados/services/fio.py:1275`; `app/chamados/services/fio.py:1294`; `app/chamados/services/fio.py:1295`.)

O trecho do aval “O par é LIDO dos marcos da célula do dia (saída e volta do mesmo intervalo)” responde à identidade da pausa; não fornece o critério de equivalência entre BATIDA sem episódio e BATIDA com episódio. Falta precisamente dizer quando essas linhas são o mesmo fato e se uma pode retratar a outra. Frase e alcance do anexo recebido: [nao verificado].

Perímetro F4 proposto para o brief — proposta técnica, garantia de diff futuro [nao verificado]:

- `app/chamados/models.py`: agrupamento DS e identidade persistida. (`app/chamados/models.py:3425`.)
- `app/chamados/services/declaracao_texto.py`: posição e índice hoje colapsados. (`app/chamados/services/declaracao_texto.py:49`.)
- `app/chamados/services/disputa_emissao.py`: emissão/complemento e seleção por motivo. (`app/chamados/services/disputa_emissao.py:226`; `app/chamados/services/disputa_emissao.py:1081`.)
- `app/chamados/services/fio.py`: consulta dos vizinhos não transmite a instância, embora a chave a contenha. (`app/chamados/services/fio.py:1447`; `app/chamados/services/fio.py:1460`.)
- `app/chamados/services/materializacao.py`: unidade individual, fonte de hora e lote. (`app/chamados/services/materializacao.py:412`; `app/chamados/services/materializacao.py:1163`; `app/chamados/services/materializacao.py:1380`.)
- `app/chamados/services/validacao.py`: materializa uma pergunta antes do lote. (`app/chamados/services/validacao.py:461`; `app/chamados/services/validacao.py:469`.)
- `app/chamados/juizes.py`: leitores existentes de lâmpada e concordância precisam preservar o intervalo cobrado. (`app/chamados/juizes.py:957`; `app/chamados/juizes.py:1334`.)
- Testes existentes apropriados: `app/chamados/tests/test_e1_v5_indice.py:121`, `app/chamados/tests/test_o261_paridade_respeita_a_lampada.py:174`, `app/chamados/tests/test_validar_materializa_wiring.py:51`.

A fonte da célula já contém os pares; a necessidade de alterá-la não apareceu nesta leitura. (`app/escala/models.py:1652`.)

## 11/11 13:28 · sessao 67154ccb23ef · - · - · origem=Ronald (tmux)

Evidências confirmadas por comparação real com a cópia `a026943099b8d88468240fd4b18491141b6810af`; nenhum arquivo foi alterado.

**Delta de escritores**

- `editar_ausencia_registro`: acrescentou captura do intervalo antigo e invalidação do fechamento dos intervalos antigo/novo; validação de tipo continua igual, pelo catálogo vivo. `app/ponto/services/ausencia.py:474-475,497,517-524`.
- Materialização de disputa: retirou os minutos fixos de dia inteiro e `checar_parcial=False`; mantém a criação pela mesma porta. `app/chamados/services/materializacao.py:474,490-499,526-535`.
- Novo command `rederivar_minutos_h06_comp11`: confere identidade incluindo `a.tipo` e chama edição com `tipo=a.tipo`, portanto passa pela validação existente. `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:142-146,169-172`.
- Novo leitor da lista de ausências: acrescenta parcialidade e fim aberto por tipo à apresentação. `app/ponto/views.py:2233-2243`.
- `criar_ausencia`, `Ausencia.save`, forms, catálogo, cadastro de tipos, admins, checar_ausencia, regularização e commands antigos não tiveram mudança funcional de validação de tipo; `criar` mudou apenas comentário sobre H06 e o modelo mudou help_text de minutos. Referências atuais: `app/ponto/services/ausencia.py:193-195,200-205`; `app/ponto/models.py:757-759,868-908`.

**Escritores e validação**

| Porta | Validação já existente / limite |
|---|---|
| Admin da aplicação | POST aceita o argumento cru, mas chama `criar_ausencia`; este rejeita fora do catálogo vivo. `app/ponto/views.py:2378,2415-2422`; `app/ponto/services/ausencia.py:193-195`. |
| PWA | Validação própria pelo vivo e chamada ao produtor validado. `app/ponto/views.py:2509-2511,2536-2542`. |
| API | Traduz somente aliases legados de atestado/luto e chama produtor validado. `app/api/views.py:1765-1768,1817-1824`. |
| Edição HTTP | Delega a edição validada pelo vivo. `app/ponto/views.py:2794-2802`; `app/ponto/services/ausencia.py:474-475`. |
| Form | `clean_tipo` consulta vivo; porém **nenhum código de produção instancia `AusenciaForm`**: a ocorrência de produção é sua definição. Não atribuir a ele a validação HTTP efetiva. `app/ponto/forms.py:41,60-68`. |
| Django admin | `AusenciaAdmin` usa `SoLeitura`; add/change/delete são negados. `app/ponto/admin.py:37-47`; `app/core/admin.py:59-66`. |
| Copiloto | Prévia valida vivo e escrita chama produtor. `app/ponto/services/checar_ausencia.py:23-24,43-48`. |
| Veredito humano | Passa `falta` ou tipo recebido ao produtor validado. `app/ponto/services/veredito_celula.py:331-340`. |
| Materialização | Mesmo produtor; tipo vem do mapa/argumento. `app/chamados/services/materializacao.py:474,490-499,526-535`. |
| Férias | Literal `ferias`, pelo produtor validado. `app/ponto/services/ausencia.py:1202-1207`. |
| Command novo | Edição validada, conservando o código carregado. `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169-172`. |
| Command de rederivar fim | Regrava o valor carregado sem validar tipo. `app/ponto/management/commands/curar_ausencia_sem_fim.py:29-31,42-46`; `app/ponto/services/ausencia.py:1293-1297`. |
| ORM/modelo | `save` deriva carimbo, minutos e intervalo e chama `super`; não valida catálogo nem choices. `app/ponto/models.py:868-908`. |

As portas validadas aceitam **fora dos choices**, desde que no vivo; isso é deliberado e protegido por teste. `app/ponto/services/ausencia.py:361-370,387`; `app/ponto/tests/test_selo_b5f1_porta_ausencia.py:180-194`.

O cadastro pode criar código `folga`: valida nome, efeito, código vazio e duplicidade, sem reserva desse código; o serviço não chama `full_clean`. Se ativo, entrará no vivo. `app/ponto/views_tipos_ausencia.py:61-66,80,89-94`; `app/ponto/services/ausencia.py:1301-1310`; `app/ponto/catalogo/ausencias.py:249-252,284-285`. Existência histórica desse cadastro e caminho efetivo da gravação fornecida: **[nao verificado]**.

**Listas e autoridade**

- Autoridade de validade: `TipoAusencia` ativo → `_do_banco` → `vigente` → `codigos_validos`; cache de 60 segundos; tabela vazia/erro recai na semente. `app/ponto/catalogo/ausencias.py:240-285`.
- Choices congelados, 17 entradas: `app/ponto/models.py:661-679,749`.
- Semente runtime, 17 entradas: `app/ponto/catalogo/ausencias.py:87-105`.
- Outra lista `CATALOGO` no form, com regras de duração; desconhecido dessa lista recebe máximo de 30 dias, mas validade é conferida no catálogo vivo. `app/ponto/forms.py:13-29,37-38,60-68`.
- Semente histórica da migration: `app/ponto/migrations/0052_seed_tipoausencia.py:9-37`; choices históricos mais recentes: `app/ponto/migrations/0070_alter_ausencia_tipo.py:19-22`.
- `TIPOS_PAGOS`, bloqueantes e abertos do modelo são aliases derivados, não novas listas de validade. `app/ponto/models.py:680-689`.
- Cadastro declara explicitamente que o admin cria novos tipos e consumidor pergunta flags, sem conhecer nome. `app/ponto/models.py:562-569`.

**Pacote para brief da cura na origem**

A brecha estrutural demonstrável é `Ausencia.save` sem validação; a origem efetiva do registro de produção continua **[nao verificado]**. Para uma cura estritamente de impedir **novo tipo fora do vivo** pelo caminho ORM comum, o arquivo operacional mínimo é `app/ponto/models.py:868-908`, preservando a autoridade `app/ponto/catalogo/ausencias.py:284-285`; impor choices quebraria contrato explícito. `app/ponto/tests/test_selo_b5f1_porta_ausencia.py:180-194`.

Se houver decisão de centralizar o validador nomeado e reutilizá-lo, os locais de validação existentes são exclusivamente `app/ponto/catalogo/ausencias.py:284-285`, `app/ponto/services/ausencia.py:193-195,474-475`, `app/ponto/forms.py:60-68` e o novo guard do modelo em `app/ponto/models.py:868`. Não há necessidade demonstrada de editar cascatas de endpoints ou leitores para a cura de validade na origem. A aplicação da guarda a saves de registros legados exige preservar o recorte separado de cura de dado; comportamento pretendido desses saves: **[nao verificado]**.

**Testes vizinhos que efetivamente enumeram catálogo/choices** [nao verificado]

- `app/ponto/tests/test_catalogo_ausencias.py:10-20,39-42,56-60`: igualdade choices↔semente, efeito e partição.
- `app/api/tests/test_api_ausencias_tipos.py:46-51`: anúncios GET limitados aos choices; este teste não cobre cadastro adicional ativo.
- `app/ponto/tests/test_cadastro_tipo.py:14-34,43-50`: seed, vivo, inativo/fallback, proibição de validar pelos choices.
- `app/ponto/tests/test_aus_f4_criar_pelo_juiz.py:43-54`: enumera vivo menos choices e garante aceitação; o laço pode ficar vazio.
- `app/ponto/tests/test_selo_b5f1_porta_ausencia.py:180-194`: caso concreto fora dos choices e dentro do vivo.
- `app/ponto/tests/test_fim_separado.py:37,58-59`: enumera semente e conjuntos.
- `app/ponto/tests/test_rotulo_do_dia_decidido.py:86`: enumera semente para rótulo.
- `app/ponto/tests/test_ui3_rotulo_do_espelho.py:26,47,63`: enumera semente e desconhecido.
- `app/ponto/tests/test_rotulo_x_efeito.py:45`: fornece semente ao teste de efeitos.
- `app/ponto/tests/test_aviso_home_office.py:11`: exige tipo nos choices.
- Contratos adjacentes de arquitetura: `app/ponto/tests/test_contract_porta_ausencia.py:30-41,68-78`; `app/ponto/tests/test_contract_catalogo_congelado.py:42-60`; caso de cadastro adicional: `app/ponto/tests/test_o137_esmeril_ausencia_le_o_cadastro.py:82-91`.

## 11/11 13:28 · sessao cb68d13e7bcf · - · - · origem=Ronald (tmux)

Censo dos itens 3 e 7, somente leitura:

- `classificar_dia` recebe contagens e insumos; não recebe chamados nem consulta conversa. Retorna CSF quando `tipo_dia == 'trabalho'`, previsto verdadeiro, realizado ≥ previsto, `chamados_vivos` verdadeiro e todas as células casadas. Isento retorna antes. Referências: `app/ponto/supra_juiz.py:204`, `:220`, `:463`, `:467`.
- O comando percorre colaboradores ativos da empresa e pula quem não tem batidas na janela; carrega apenas chamados dos `FURO_MODULES`, associa pelo juiz `data_do_chamado`, separa `vivos` por `VIVOS` e conta o fio por `fio_vivo`. Referências: `app/ponto/management/commands/supra_juiz.py:110`, `:113`, `:117`, `:186`, `:190`, `:199`, `:212`, `:215`.
- Hoje a classificação alimenta placar/alarme; a escrita lê a lavra `cobranca_indevida` e seus protestos. Ausência de lavra conta `CSF_SEM_LAVRA`; falta de código conta `CSF_LAVRA_SEM_CODIGO`; nenhum dos dois casos retrata. Referências: `app/ponto/management/commands/supra_juiz.py:223`, `:235`, `:246`, `:248`, `:249`, `:250`, `:253`.
- A guarda roda **antes** da verificação de cobrança órfã e mesmo em dry-run. Se retorna falso, incrementa `CSF_COM_CONVERSA_PRESERVADO` e dá `continue`: não chama o retratador. Referências: `app/ponto/management/commands/supra_juiz.py:257`, `:260`, `:261`, `:262`, `:263`, `:275`.
- `--executar`, com `T6_1_RETRATACAO_EM_SECO=False`, chama o retratador; só o retorno verdadeiro incrementa `CSF_RETRATADO`; falso conta recusa. Sem flag conta `CSF_RETRATAVEL_DRY`. Referências: `app/ponto/management/commands/supra_juiz.py:45`, `:279`, `:286`, `:288`, `:290`, `:295`.
- `CSF_RETRATADO` é um `Counter` **local de cada execução**: criado em `:105`, escrito em `:288` e impresso por `most_common()` em `:299–300`. Ausência da linha não é emissão explícita de zero. Não há leitura operacional pelo alarme, que consulta somente `FURO_SEM_COBRANCA` e `COBRANCA_SEM_FURO`. Referências: `app/ponto/management/commands/supra_juiz.py:105`, `:288`, `:299`, `:305`, `:306`.
- Os leitores executáveis encontrados do nome literal são testes: exige presença após retratação em `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:122`; exige ausência quando a lavra não traz código em `:153`; exige ausência quando a porta recusa em `:167`. As alegações históricas nos documentos não são medição atual de produção [nao verificado].
- O cron vivo ainda declara `supra_juiz --executar`, por empresa e dependente de `processar_cartorio`, em `app/config/crons.py:599`; a classificação de papel distingue o supra sem `--executar` como vigia em `:2342`.
- O selo funcional do comando cobre lavra concorde sem retratação, lavra indevida com retratação, classificação local divergente sem alterar a decisão da lavra, ausência de lavra, ausência de código e recusa da porta. Referências: `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:106`, `:116`, `:124`, `:134`, `:142`, `:155`.

A medição histórica **exata** pedida no item 7 não é recuperável só pelo banco desta cópia: a trilha da transição grava somente status anterior e `{status_local, via}`, sem motivo CSF ou porta chamadora (`app/chamados/models.py:292–295`). As diversas reconciliações do cartório chamam a mesma porta (`app/ponto/services/cartorio.py:1525`, `:1538`, `:1559`), cuja via persistida é `sistema`, não `reconciliacao` (`app/chamados/catalogo/motor.py:88–96`; `app/chamados/models.py:654`). O ator identifica processo/request, não pilha (`app/core/observ.py:22–32`, `:94–138`).

Também não cabe reconstruir literalmente o predicado antigo usando só os vínculos e carimbos atuais: ressuscitar zera `respondida_em` (`app/chamados/models.py:1563–1566`); a relação de origem pode ficar nula (`:1750–1755`); há reapontamento de destino pela porta do fio (`app/chamados/services/fio.py:20`). A sequência de decisões preserva atos, mas o legado sem sequência tem tratamento próprio (`app/chamados/models.py:3840–3849`, `:3913–3938`).

Para a sonda final, recomendo manter **dois resultados exatos como `"nao medido"`**, com essas razões, e fornecer somente um **proxy expressamente identificado**, separado:

1. transições `estado_chamado` nos últimos 30 dias;
2. ator `sistema:processar_cartorio`;
3. origem em `COBRANCA`, destino em `TERMINAIS`, via `EVENTO_VIA['RECONCILIAR']`;
4. chamados atuais ligados a pergunta com `respondida_em <= instante da transição`;
5. agrupar por `empresa_id`, `modulo_origem` e, se desejado, `PerguntaDisputa.motivo`, contando IDs distintos/eventos;
6. deixar explícito que não prova CSF, não cobre cartório acionado sob outro processo e não reconstrói associações históricas.

`bin/sonda_leitura.sh` aceita um arquivo Python em `:56`, usa produção por padrão em `:93–97` e executa shell Django no schema `juliani` em `:235–240`. **Não executar o wrapper nesta sessão de nenhuma escrita:** ele cria/altera credencial, GRANTs, rede, temporários e trilha (`:126–143`, `:153–181`, `:185–193`, `:216–218`, `:248–250`). A sonda Python pode ser entregue como texto, sem criar arquivo.

## 11/11 13:28 · sessao c2e43acb8731 · - · - · origem=Ronald (tmux)

Comparação feita contra a cópia `../a026943099b8d88468240fd4b18491141b6810af`, por diff e AST sem docstrings.

Delta dos leitores citados:

| Leitor | Mudança desde a0269430 | Citação atual |
|---|---|---|
| `efeitos_da_ausencia` | Os dois ramos que consideravam qualquer `minutos` como parcial passaram a usar `cobre_parte_do_dia(a, parciais)`. | `app/ponto/services/efeito_ausencia.py:38,48,55` |
| `recalcular_fechamento_mes` | Mesma troca no ramo de cobertura; retirou acumulador `_min_parcial_pagos` sem leitor. O desconhecido continua suprimindo previsto (`None != DESCONTA`). | `app/ponto/services/fechamento.py:672-673,690-701,731` |
| `creditos_por_dia` | Só credita minutos quando o juiz confirma parcialidade, além da presença em `TIPOS_PAGOS`. | `app/ponto/services/credito_parcial.py:20-28` |
| `dia_decidido.rotulo_de` | Quando o veredito é `ABONADO`, zera minutos apresentados se a ausência não é parcial; inclusive desconhecido, porque seu veredito continua `ABONADO`. | `app/ponto/services/dia_decidido.py:468-473,490-497` |
| `cobertura_ausencia_periodo` | Extraiu filtro e expansão para dois novos leitores auxiliares, conservando critério; novo acesso em lote reutiliza os mesmos. | `app/ponto/turnos.py:2752-2783,3068-3081` |
| `classificar_falta` | Recebe agora `carga` e `agora`; ausência pré-carregada pode alimentar o mesmo guarda de não parcialidade. | `app/escala/servico_jornada.py:280-295,346-352` |
| Cartório, leitor indireto de cobertura | Ramo sem adesão passou a considerar dia sem trabalho no horizonte desde competência 2026-11; cobertura integral preserva o comportamento anterior. | `app/ponto/services/cartorio.py:267-285,1230-1245,1254-1277` |

Novos auxiliares explicitamente relevantes: `_q_cobertura`, `_cobertura_pura` e `CargaDaJanela.cobertura`; desconhecido sem `data_fim` continua expandido apenas no dia inicial. `app/ponto/turnos.py:2757-2783,3068-3081`.

Nenhum dos leitores de tipo citados desapareceu. Precedência, parcialidade/integral abonado, suprajuiz, grade, estado/documento, avisos, inteligência, vigias e filtros de ausência do export mantiveram a semântica do anexo; funções desses arquivos podem ter mudanças laterais, mas não no tratamento descrito de `Ausencia.tipo`. Para essa conclusão de comparação: evidência fornecida pelo diff entre as duas cópias, sem arquivo de relatório salvo [nao verificado].

Comportamento atual para **ausente de `vigente()`**, necessário ao fecho:

| Decisor | Resultado |
|---|---|
| Catálogo | `efeito_vigente` devolve `None`; código cru como rótulo. `app/ponto/catalogo/ausencias.py:293-300`. |
| `precedencia.faixa_de` | `atestado`, inclusive literal `folga`; mapa distingue somente `folga_compensatoria` e `feriado_folga` como faixa folga. `app/ponto/precedencia.py:52-79`. |
| `fatos_do_periodo` | Inclui não rejeitada; conserva código; ordem permite vencer feriado, folga e trabalho. `app/ponto/precedencia.py:273-279,336-339,379-408`. |
| Cobertura | Folha exige aprovada; cobrança aceita não rejeitada; cobre intervalo fechado, ou só início se fim ausente e fora de `TIPOS_EM_ABERTO`. `app/ponto/turnos.py:2511-2520,2554-2559,2770-2783`. |
| Parcialidade/integral | Não parcial; diferente de `falta`, explica dia inteiro; não entra no mapa integral abonado porque falta efeito `ABONA`. `app/ponto/turnos.py:2597-2602,2670-2692`. |
| Cartório | Aprovada integral dispensa lâmpadas e escreve código cru em `dispensada`; precedência é copiada para ata. `app/ponto/services/cartorio.py:790-800,1984-1993`. |
| Fechamento financeiro | Não abona; suprime previsto por `None != DESCONTA`. `app/ponto/services/fechamento.py:696-701,731`. |
| Espelho/PDF, coletor de efeitos | Não inclui em justificadas nem suprimidas; exige `ABONA` ou `SUPRIME` explícitos. `app/ponto/services/efeito_ausencia.py:57-65`. |
| Motor/calculador | Não recebe esse tipo no mapa que zera pontualidade; mapa exige `ABONA`. Código só aparece na mensagem do ramo quando a ausência chega nesse mapa. `app/ponto/turnos.py:2686-2692`; `app/ponto/motor_calculo_v2.py:1085-1105`; `app/ponto/calculador/regras.py:117-120`. |
| Export 8792 | Fora do filtro, que usa `descontam_vigentes()`. `app/folha/export.py:87-91,138-140,179-181`. |
| Export 8932 | Fora do conjunto vivo de abonados. `app/folha/export.py:193-205`. |
| Veredito visual | `ABONADO`, salvo rejeitada; lançamento aberto aparece pendente. `app/ponto/services/dia_decidido.py:447-450,460-481`. |
| Status do dia | Quando deriva natureza pela precedência, faixa atestado com efeito `None` produz `ausencia`. `app/ponto/status_dia.py:296-319`. |

Cuidado no delta de export: `sai_pela_rescisao`, `classificar_export` e `competencia_entregue` mudaram, porém não são mudanças do filtro/decisão por `Ausencia.tipo`; os filtros 8792/8932 permaneceram iguais. `app/folha/export.py:54-62,169-205,556-592,810-814,859-867`.

## 11/11 13:28 · sessao 13e2beea08ed · - · - · origem=Ronald (tmux)

- O chamador concreto é `bin/tests/test_selo_proporcao.sh`, que define `SELO` na linha 8, chama e compara rc nas linhas 23–24; também chama nas linhas 56–59, 62 e 63. A chamada com `bash -e` não tem seu rc conferido; a comparação da linha 63 pertence a outra chamada, sem `-e`. (`bin/tests/test_selo_proporcao.sh:8`, `bin/tests/test_selo_proporcao.sh:23`, `bin/tests/test_selo_proporcao.sh:24`, `bin/tests/test_selo_proporcao.sh:56`, `bin/tests/test_selo_proporcao.sh:62`, `bin/tests/test_selo_proporcao.sh:63`)

- A régua executa esse teste pelo glob e converte qualquer falha do teste em rc 1; o teste usa repositório sintético, portanto não classifica o diff real do pouso. (`bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`, `bin/tests/test_selo_proporcao.sh:10`, `bin/tests/test_selo_proporcao.sh:16`, `bin/tests/test_selo_proporcao.sh:23`)

- Não encontrei chamada do selo pelo pouso/pre-push, nem registro de `PROPORCAO-POR-RISCO` em `CORTES.md` ou `RELATO.md`; uso manual e implantação efetiva: [nao verificado]. [nao verificado]

- O achado do atalho da régua permanece: o fingerprint cobre somente `.py`/`.html` de `app/`, a decisão “JA VERDE” compara esse fingerprint e retorna antes dos testes de host; `ARVORE_FP` é calculado, mas não participa dessa condição. (`bin/regua.sh:25`, `bin/regua.sh:33`, `bin/regua.sh:45`, `bin/regua.sh:86`, `bin/regua.sh:93`, `bin/regua.sh:138`)

- O achado independente de worktree permanece: `test_hook_nao_e_copia.sh` procura `.git/hooks/pre-push` diretamente e falha quando ausente; outro teste já resolve `core.hooksPath`/`git-common-dir`. (`bin/tests/test_hook_nao_e_copia.sh:21`, `bin/tests/test_hook_nao_e_copia.sh:25`, `bin/tests/test_hook_nao_e_copia.sh:27`, `bin/tests/test_commit_so_o_declarado.sh:56`, `bin/tests/test_commit_so_o_declarado.sh:57`)

- A exclusão de testes ficou parcialmente fechada. Imports adicionados em produção são marcados; porém, se o import já existia na base e o diff muda somente `test_writer.py` para escrever, o arquivo continua excluído e resulta em rc 0. O caso `prod_importa_teste` adiciona somente o import, sem criar módulo nem escrita. (`bin/selo_proporcao.py:61`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:106`, `bin/selo_proporcao.py:115`, `bin/selo_proporcao.py:119`, `bin/tests/test_selo_proporcao.sh:47`)

- Falsos positivos deduzidos: `dict.update`, `set.update`, `[].delete()` e strings Python exatamente `"update"`/`"create"` produzem rc 1 com B, pois não há resolução de tipo ou contexto. Já palavra `update` em template `.html` é ignorada e produz rc 0. O código declara aceita a direção conservadora para A. (`bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:115`, `bin/selo_proporcao.py:119`, `bin/selo_proporcao.py:19`)

- Alteração apenas de comentário em migration ou `signals.py` também produz rc 1: as regras por caminho são incondicionais para arquivos não deletados, apesar de o cabeçalho dizer “NOVO ou RENOMEADO”. (`bin/selo_proporcao.py:10`, `bin/selo_proporcao.py:92`, `bin/selo_proporcao.py:94`, `bin/selo_proporcao.py:96`, `bin/selo_proporcao.py:115`)

## 11/11 13:28 · sessao b162c8de5d02 · - · - · origem=Ronald (tmux)

A cópia contém **12 sítios de fechamento automático normal**, mais **duas exceções automáticas nomeadas**; os 12 normais passam `motivo=None`, e `fechar` grava `vazia` sem perguntas ou `veredito` com perguntas, além do autor recebido, resolução e evento `disputa_fechada`. (`app/chamados/models.py:1903`, `app/chamados/models.py:1932`, `app/chamados/models.py:1937`.)

| Porta atual | Autor passado | Resolução gravada |
|---|---|---|
| Ausência declarada | `user` recebido e também usado para carimbar as perguntas absorvidas | `Ausencia declarada pelo colaborador; perguntas do dia resolvidas.` (`app/chamados/services/materializacao.py:566`, `app/chamados/services/materializacao.py:591`.) |
| Cauda de materialização | `request_user` recebido | Primeira não vazia entre `resolucao_pedida`, `disputa.resolucao`, `Regularizacao granular: {n_mat} pergunta(s) materializada(s).` (`app/chamados/services/materializacao.py:1333`, `app/chamados/services/materializacao.py:1338`.) |
| Reconciliação sem turno aberto | `get_user_sistema()`, também autor do carimbo | `Reconciliador: sem turno aberto no dia; perguntas resolvidas.` (`app/chamados/reconciliador.py:400`, `app/chamados/reconciliador.py:405`, `app/chamados/reconciliador.py:412`.) |
| Absolvição pela célula | `get_user_sistema()`, também autor dos carimbos | `Celula soberana julgada absolveu o(s) dia(s); perguntas resolvidas pelo reconciliador.` (`app/chamados/reconciliador.py:587`, `app/chamados/reconciliador.py:595`, `app/chamados/reconciliador.py:612`.) |
| Órfãs pela lâmpada | `get_user_sistema()`, também autor de `encerrar_fantasma` | `Perguntas orfas encerradas no evento da celula: a lampada do proprio marco acendeu (ORFAS-ENTERRA-FURO-REAL (b)).` (`app/chamados/reconciliador.py:1142`, `app/chamados/reconciliador.py:1147`, `app/chamados/reconciliador.py:1171`.) |
| Beco | Parâmetro `sistema`; entrada do evento fornece `get_user_sistema()` | `Nenhuma pergunta viva; chamado-pai resolvido. Beco fechado pelo varredor (HX-BECO).` (`app/chamados/services/becos.py:18`, `app/chamados/services/becos.py:57`, `app/chamados/services/becos.py:78`.) |
| Emissão sem pergunta exigível | `get_user_sistema()` | `Fechada automaticamente: nada a perguntar ao colaborador neste dia (o juiz da jornada nao aponta marco faltante). Se o fato voltar a acusar, o cartorio reabre pelo caminho normal.` (`app/chamados/services/disputa_emissao.py:1283`, `app/chamados/services/disputa_emissao.py:1284`.) |
| Chamado resolvido / S128 | `user` recebido; o signal de chamado fornece sistema | `Saida registrada; turno encerrado - resolvido automaticamente pelo sistema (S128).` (`app/chamados/services/disputa_emissao.py:1801`, `app/chamados/services/disputa_emissao.py:1964`, `app/chamados/signals.py:115`.) |
| Remoção do último motivo | `ator` recebido | `resolucao` recebida; eventos concretos usam sistema e `auto_revalidacao_posto_reativado` ou `auto_revalidacao_ancora_calibrada`. (`app/chamados/models.py:2066`, `app/chamados/models.py:2072`, `app/colaboradores/signals.py:77`, `app/escala/signals.py:161`.) |
| Entrada sincronizada | Sistema, embora a função receba `autor_user` | `Entrada registrada em {hora} (sync); orfao reconciliado, disputa dissolvida automaticamente.` (`app/ponto/services/entrada_sincronizada.py:19`, `app/ponto/services/entrada_sincronizada.py:59`.) |
| Foto recebida | `user` recebido, também usado na validação | `Auto-fechada: foto recebida no upload separado do APK.` (`app/api/foto_recebida.py:51`, `app/api/foto_recebida.py:79`, `app/api/foto_recebida.py:80`.) |
| Comando de acervo, somente com `--apply` | Usuário consultado pelo argumento `--por` | `BUG 117: todas as perguntas tinham o fato no espelho. Encerrada pela porta, com trilha.` (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:53`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:63`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`.) |

A cauda de materialização também atende à validação e ao reparador de passivo: o primeiro entrega `autor` ao lote; o segundo chama a mesma cauda com `user`. (`app/chamados/services/validacao.py:469`, `app/chamados/services/materializacao.py:1383`, `app/chamados/services/passivo_validado.py:405`.) O comando de becos também usa a mesma porta `fechar_beco`. (`app/chamados/management/commands/fechar_becos_disputa.py:57`.)

| Exceção automática | Autor / motivo |
|---|---|
| Competência trancada | `autor` recebido ou sistema; `motivo_fechamento='competencia_trancada'`; resolução inclui competência e empresa. Nenhuma pergunta é carimbada por esse caminho. (`app/chamados/services/competencia_trancada.py:175`, `app/chamados/services/competencia_trancada.py:178`, `app/chamados/services/competencia_trancada.py:204`.) |
| Desligamento | `_ator` recebido ou sistema; `motivo_fechamento='desligamento'`; resolução `Colab desligado em {data_demissao} - fechamento automatico.` (`app/colaboradores/services/desligamento.py:131`, `app/colaboradores/services/desligamento.py:142`.) |

`admin_encerra` é a porta explícita de encerramento administrativo com motivo obrigatório; passa autor recebido e `motivo_fechamento='admin_encerra'`. As exceções desligamento/admin também carimbam perguntas vivas e registram decisões SILÊNCIO com o autor do fechamento. (`app/chamados/models.py:1918`, `app/chamados/models.py:1924`, `app/chamados/models.py:1943`, `app/chamados/models.py:1950`.)

**Ponto crítico para a F3:** autor do ato não é sempre o `materializada_por` atualmente persistido. `remover_motivo` registra o ato com `ator`, mas chama `carimbar_identidade(...manter_carimbo=True)`, que pode preservar instante e autor antigos. (`app/chamados/models.py:2056`, `app/chamados/models.py:2061`, `app/chamados/models.py:3182`.) O evento de beco perde a autoria do ato: agenda só `disputa_id` e a porta escolhe sistema. (`app/chamados/signals.py:228`, `app/chamados/signals.py:238`, `app/chamados/services/becos.py:78`.)

**Perímetro concreto para conferência de sobreposição da F3:** os arquivos abaixo contêm as portas/reações atuais diretamente relevantes; que todos sejam necessariamente editados depende do desenho ainda não implementado **[nao verificado]**.

- `app/chamados/models.py` — carimbos e fecho central. (`app/chamados/models.py:1855`, `app/chamados/models.py:3103`, `app/chamados/models.py:3168`.)
- `app/chamados/signals.py` — reação sem transporte de autor e S143 com sistema. (`app/chamados/signals.py:115`, `app/chamados/signals.py:228`.)
- `app/chamados/services/becos.py`. (`app/chamados/services/becos.py:57`.)
- `app/chamados/services/materializacao.py`. (`app/chamados/services/materializacao.py:591`, `app/chamados/services/materializacao.py:1338`.)
- `app/chamados/reconciliador.py`. (`app/chamados/reconciliador.py:412`, `app/chamados/reconciliador.py:612`, `app/chamados/reconciliador.py:1171`.)
- `app/chamados/services/disputa_emissao.py`. (`app/chamados/services/disputa_emissao.py:1284`, `app/chamados/services/disputa_emissao.py:1964`.)
- `app/ponto/services/entrada_sincronizada.py`. (`app/ponto/services/entrada_sincronizada.py:59`.)
- `app/api/foto_recebida.py`. (`app/api/foto_recebida.py:80`.)
- `app/chamados/management/commands/encerrar_perguntas_com_fato.py`. (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`.)

Competência trancada e desligamento são fechos próprios sem exigência de um novo último carimbo no primeiro caso; incluí-los como edição obrigatória da regra F3 excederia o que o aval determina **[nao verificado]**. Seus sítios atuais estão identificados acima. (`app/chamados/services/competencia_trancada.py:178`, `app/colaboradores/services/desligamento.py:142`.)

## 11/11 13:25 · sessao c2682795b25e · FIO-E0-V4-E4B · construtor · origem=Code

Cura, testes, mutantes e análise dos chamadores entregues no [recibo c6](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c6.txt:6). [nao verificado]

Observado localmente: **104 testes em memória e 40 selos passaram**; RED da base e os dois mutantes falharam nas assertions esperadas ([verificações](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c6.txt:54)). [nao verificado]

ORM não executado: **[nao verificado]**. Os [comandos para a sessão](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c6.txt:88) estão registrados. Sem commit, respeitando a proibição explícita do pedido. [nao verificado]

## 11/11 13:21 · sessao da29efa3c9fc · PROPORCAO-POR-RISCO · verificador · origem=Code

**REPROVADO — há escapes de severidade ALTA que permitem declarar B com escrita direta no diff.** A detecção depende da grafia e do cabeçalho textual, e zero achados autoriza B. (`bin/selo_proporcao.sh:22`, `bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:48`)

Os resultados abaixo foram conferidos com diffs sintéticos fornecidos em memória ao script. Execução contra commits reais, banco, implantação e autenticidade do commit/base: **[nao verificado]**. A suíte de host completa cria e remove arquivos, portanto não foi executada nesta revisão somente leitura. (`bin/tests/test_selo_proporcao.sh:9`, `bin/tests/test_selo_proporcao.sh:18`)

1. **ALTA — escritas da lista passam com rc 0 por diferenças de grafia e de representação do diff.**

   Este diff reúne exemplos para **save, create, update, delete, bulk_create, bulk_update, get_or_create e update_or_create**. São expressões Python válidas; todas escapam porque a regex exige o nome imediatamente depois do ponto. O parser retornou **rc 0 para B**. (`bin/selo_proporcao.sh:22`, `bin/selo_proporcao.sh:44`, `bin/selo_proporcao.sh:48`)

   ```diff
   --- /dev/null
   +++ b/app/x/escritor.py
   @@ -0,0 +1,8 @@
   +obj. save()
   +M.objects. create(a=1)
   +M.objects. update(a=1)
   +obj. delete()
   +M.objects. bulk_create(xs)
   +M.objects. bulk_update(xs, ["a"])
   +M.objects. get_or_create(a=1)
   +M.objects. update_or_create(a=1)
   ```

   Outras formas concretas também retornaram **rc 0**, cada uma adicionada isoladamente a um `.py` comum: a análise é linha a linha, exige a chamada literal e corta a linha no primeiro `#`. (`bin/selo_proporcao.sh:22`, `bin/selo_proporcao.sh:27`, `bin/selo_proporcao.sh:36`)

   | Forma | Linhas adicionadas ao diff |
   |---|---|
   | Quebra antes do parêntese | `+(obj.save`<br>`+())` |
   | Alias | `+gravar = obj.save`<br>`+gravar()` |
   | `getattr` | `+getattr(obj, "save")()` |
   | Método entre parênteses | `+(obj.save)()` |
   | `#` dentro de string | `+tag = "#"; obj.save()` |

   **Signals:** este acréscimo em `app/x/eventos.py` retorna **rc 0**. A regex reconhece alguns nomes literais e `@receiver`; a regra por arquivo cobre somente `signals.py`. (`bin/selo_proporcao.sh:24`, `bin/selo_proporcao.sh:40`)

   ```diff
   --- a/app/x/eventos.py
   +++ b/app/x/eventos.py
   @@ -10,0 +11,2 @@
   +from django.db.models import signals as sig
   +getattr(sig, "post" + "_save").connect(gravar, sender=M)
   ```

   **Management command que escreve:** este acréscimo retorna **rc 0**. Não existe tratamento próprio para `management/commands/`; ele depende das mesmas regex de qualquer `.py`. (`bin/selo_proporcao.sh:22`, `bin/selo_proporcao.sh:35`, `bin/selo_proporcao.sh:37`)

   ```diff
   --- a/app/x/management/commands/gravar.py
   +++ b/app/x/management/commands/gravar.py
   @@ -10,0 +11,2 @@
   +        escrever = M.objects.create
   +        escrever(a=1)
   ```

   **Migration por rename:** este patch retorna **rc 0**, sem sequer listar o destino. O scanner só aprende o caminho em `+++` e só aplica a regra de migrations numa linha adicionada. Conteúdo da migration e efeito no banco deste exemplo: **[nao verificado]**. (`bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:32`, `bin/selo_proporcao.sh:34`)

   ```diff
   diff --git a/app/x/pendente.py b/app/x/migrations/0002_gravar.py
   similarity index 100%
   rename from app/x/pendente.py
   rename to app/x/migrations/0002_gravar.py
   ```

   **Binário:** esta representação também retorna **rc 0**, porque não tem `+++` nem linhas adicionadas. Um `.py` textual marcado `-diff` em `.gitattributes` é o cenário relevante; inserir NUL num `.py` não demonstra Python executável. Aplicação real desse atributo neste repositório: **[nao verificado]**. (`bin/selo_proporcao.sh:19`, `bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:32`)

   ```diff
   diff --git a/app/x/escritor.py b/app/x/escritor.py
   Binary files a/app/x/escritor.py and b/app/x/escritor.py differ
   ```

   **Caminhos:** os cabeçalhos abaixo, seguidos de `+obj.save()`, retornam **rc 0**. `<TAB>` representa tabulação real. O parser preserva tabulação ou aspas finais; consequentemente, o caminho não termina em `.py`. (`bin/selo_proporcao.sh:29`, `bin/selo_proporcao.sh:35`)

   ```text
   +++ b/app/x/escritor outro.py<TAB>
   +++ "b/app/x/caf\303\251.py"
   ```

   **`+++` dentro do conteúdo:** este diff cria Python sintaticamente válido e retorna **rc 0**. A segunda linha do arquivo é `++ /dev/null`, dentro da string; no patch, ela vira um falso cabeçalho que redefine `arq=None`. (`bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:29`, `bin/selo_proporcao.sh:32`)

   ```diff
   --- /dev/null
   +++ b/app/x/escritor.py
   @@ -0,0 +1,4 @@
   +DOC = """
   +++ /dev/null
   +"""
   +obj.save()
   ```

   **Modo do diff:** a chamada `bin/selo_proporcao.sh --name-only HEAD B repo` permite que o suposto argumento de revisão seja interpretado como opção do Git. Uma saída contendo somente `app/x/escritor.py` vira zero achados e **rc 0**; as revisões não são validadas separadamente. Execução completa desse cenário num repositório real: **[nao verificado]**. (`bin/selo_proporcao.sh:19`, `bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:48`)

2. **ALTA — a exclusão de testes permite esconder módulo de produção que grava.** [nao verificado]

   Sim. Este diff retorna **rc 0**: a escrita é descartada pelo nome `test_*.py`, enquanto o módulo de produção importa e chama a função. O scanner não verifica consumidores nem separa fixture de módulo usado pela aplicação. (`bin/selo_proporcao.sh:25`, `bin/selo_proporcao.sh:32`)

   ```diff
   --- /dev/null
   +++ b/app/x/test_writer.py
   @@ -0,0 +1,2 @@
   +def gravar(obj):
   +    obj.save()
   --- a/app/x/views.py
   +++ b/app/x/views.py
   @@ -10,0 +11,2 @@
   +from .test_writer import gravar
   +gravar(obj)
   ```

   A variante `app/x/tests/writer.py`, importada por `from .tests.writer import gravar`, também retorna **rc 0**. A exclusão por diretório é independente do nome do arquivo e ocorre inclusive antes da regra de migrations. Existência desse uso em produção na cópia: **[nao verificado]**. (`bin/selo_proporcao.sh:25`, `bin/selo_proporcao.sh:32`, `bin/selo_proporcao.sh:34`)

3. **MÉDIA — casos do host não isolam o mecanismo anunciado.**

   | Caso | Por que pode continuar passando com parte da guarda quebrada |
   |---|---|
   | `command_escreve` | `.delete()` basta em qualquer `.py`; não prova reconhecimento de commands. (`bin/tests/test_selo_proporcao.sh:33`; `bin/selo_proporcao.sh:22`) |
   | `signal_receiver` | Contém `@receiver` **e** `post_save`; qualquer um sustenta o resultado se o outro deixar de funcionar. (`bin/tests/test_selo_proporcao.sh:31`; `bin/selo_proporcao.sh:24`) |
   | `fixture_teste` | Usa `tests/` **e** `test_a.py`; remover uma das exclusões não faz o caso falhar. (`bin/tests/test_selo_proporcao.sh:37`; `bin/selo_proporcao.sh:25`) |
   | `orm_bulk` / `orm_goc` | Exercitam somente `bulk_create` e `update_or_create`; não há casos independentes para `bulk_update` e `get_or_create`. (`bin/tests/test_selo_proporcao.sh:27`, `bin/tests/test_selo_proporcao.sh:28`) |

   Há ainda uma fragilidade de preparação: checkout, alteração, add e commit não têm verificação individual; falta `errexit` e o commit permite árvore vazia. Uma preparação falha pode deixar o caso julgando outro diff. Ocorrência efetiva: **[nao verificado]**. (`bin/tests/test_selo_proporcao.sh:5`, `bin/tests/test_selo_proporcao.sh:17`, `bin/tests/test_selo_proporcao.sh:18`)

4. **ALTA para conteúdo omitido; o pipe interno trata corretamente as falhas perguntadas.**

   | Caminho | Resultado |
   |---|---|
   | Git retorna não zero | Selo retorna **2**. (`bin/selo_proporcao.sh:19`) |
   | Python retorna **1 ou 2** | Selo retorna **2**. (`bin/selo_proporcao.sh:46`, `bin/selo_proporcao.sh:51`) |
   | Produtor do pipe falha | `pipefail` preserva a falha; resultado fora de 0/3 vira **2**. (`bin/selo_proporcao.sh:15`, `bin/selo_proporcao.sh:20`, `bin/selo_proporcao.sh:51`) |
   | Git retorna 0 com saída vazia | Selo retorna **0**. Correto para diff realmente vazio; falso verde se conteúdo relevante foi omitido. (`bin/selo_proporcao.sh:26`, `bin/selo_proporcao.sh:44`, `bin/selo_proporcao.sh:48`) |

   Portanto, os falsos verdes demonstrados estão na interpretação/omissão do diff, não no mapeamento de Python rc 1/2. (`bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:35`, `bin/selo_proporcao.sh:51`)

   **MÉDIA, condicionada à invocação:** com `bash -e`, o pipeline que retorna 3 encerra o script antes de `r=$?`: escrita resulta em **rc 3**, tanto para A quanto B. Esse modo de chamada hoje: **[nao verificado]**. (`bin/selo_proporcao.sh:15`, `bin/selo_proporcao.sh:20`, `bin/selo_proporcao.sh:44`, `bin/selo_proporcao.sh:46`)

   Um chamador externo que use `selo ... | cat` sem `pipefail` pode observar o rc do `cat`. Uso desse encadeamento hoje: **[nao verificado]**. O teste novo captura diretamente o rc do selo. (`bin/tests/test_selo_proporcao.sh:19`)

5. **MÉDIA — alcance dos consumidores e falha preexistente em worktree.**

   - **Pre-push:** não executa o loop de `bin/tests`; chama selos específicos. Não há chamada ao selo de proporção nesse fluxo da cópia. Instalação efetiva do hook e chamada manual no pouso: **[nao verificado]**. (`bin/pre-push.sh:13`, `bin/pre-push.sh:16`, `bin/pre-push.sh:19`, `bin/pre-push.sh:38`)
   - **Régua:** o novo `test_selo_proporcao.sh` entra automaticamente pelo glob e pode barrar a suíte pelo rc. Ele julga um repositório sintético, não o diff real da obra. (`bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`; `bin/tests/test_selo_proporcao.sh:9`, `bin/tests/test_selo_proporcao.sh:19`)
   - **Atalho da régua:** uma mudança somente em `bin/` pode ser pulada pelo “JA VERDE”: a condição compara o fingerprint de `.py`/`.html` de `app/` e retorna antes dos testes de host, sem comparar `ARVORE_FP`. (`bin/regua.sh:25`, `bin/regua.sh:33`, `bin/regua.sh:45`, `bin/regua.sh:86`, `bin/regua.sh:93`)
   - **Teste novo isolado em worktree:** não pressupõe `.git` como diretório; resolve o selo pelo próprio caminho e fornece explicitamente o repositório sintético. Execução efetiva em worktree: **[nao verificado]**. (`bin/tests/test_selo_proporcao.sh:6`, `bin/tests/test_selo_proporcao.sh:19`)
   - **Pasta inteira em worktree:** contém uma quebra independente deste selo. `test_hook_nao_e_copia.sh` exige `$RAIZ/.git/hooks/pre-push` e retorna 1 quando ausente; a própria cópia documenta que `.git` de worktree é arquivo e mostra resolução pelo diretório comum em outro teste. (`bin/tests/test_hook_nao_e_copia.sh:21`, `bin/tests/test_hook_nao_e_copia.sh:25`, `bin/tests/test_hook_nao_e_copia.sh:27`; `bin/tests/test_commit_so_o_declarado.sh:52`, `bin/tests/test_commit_so_o_declarado.sh:57`)

## 11/11 13:19 · sessao 93579e5840e6 · - · - · origem=Ronald (tmux)

Achados para consolidar, todos derivados de `bin/selo_proporcao.sh` e simulados em memória com o script original e função `git` que fornece o diff, sem escrever arquivos:

- **ALTA — ORM direto escapa:** `+obj. save()`, `+M.objects. create(a=1)`, `+M.objects. update(a=1)`, `+obj. delete()`, `+M.objects. bulk_create([])` são Python válido (`ast.parse`) e deram rc 0. A regex exige ponto imediatamente seguido do nome. Aplica-se também a `bulk_update`, `get_or_create`, `update_or_create`. `bin/selo_proporcao.sh:22`.
- **ALTA — continuação/alias/getattr:** deram rc 0 os diffs `+obj.save\` seguido de `+()`, `+write = obj.save` seguido de `+write()`, `+getattr(obj, "save")()` e `+(obj.save)()`. `bin/selo_proporcao.sh:22`, `bin/selo_proporcao.sh:27`.
- **ALTA — `#` dentro de string oculta escrita na mesma linha:** `+label = "#"; obj.save()` deu rc 0 porque corta no primeiro `#`, sem análise léxica. `bin/selo_proporcao.sh:36`.
- **ALTA — cabeçalho forjado no conteúdo:** este diff cria Python válido e deu rc 0:

```diff
+++ b/app/writer.py
@@ -0,0 +1,4 @@
+DOC = """
+++ /dev/null
+"""
+obj.save()
```

O conteúdo real da segunda linha é `++ /dev/null` dentro de uma string. O parser considera a linha do patch um cabeçalho, põe `arq=None` e ignora a escrita. `bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:29`, `bin/selo_proporcao.sh:32`.

- **ALTA — caminhos:** `+++ b/app/writer other.py<TAB>` e `+++ "b/app/caf\303\251.py"` seguidos de `+obj.save()` deram rc 0: o caminho fica com TAB ou aspas finais e falha em `.endswith(".py")`. Git real `diff --no-index` sobre memfds, com prefixos contendo espaço/Unicode, confirmou esses formatos de cabeçalho; não houve arquivos criados. `bin/selo_proporcao.sh:29`, `bin/selo_proporcao.sh:35`.
- **ALTA — migration por rename:** patch com `similarity index 100%`, `rename from app/operation.py`, `rename to app/migrations/0002_operation.py` e sem hunks deu rc 0. A regra migration só roda após encontrar linha adicionada sob `+++`. `bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:32`, `bin/selo_proporcao.sh:34`. A geração real desse rename nesta cópia está [nao verificado], pois não há `.git`.
- **ALTA — binário:** patch `Binary files /dev/null and b/app/writer.py differ` deu rc 0 e lista zero arquivos. Para demonstrar escrita executável, usar arquivo Python textual marcado `-diff` por `.gitattributes`; NUL num `.py` não demonstra execução Python válida. Efeito real de `.gitattributes` no repo está [nao verificado]. `bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:32`.
- **ALTA — modo do diff por argumento:** `BASE=--name-only`, `HEAD_=HEAD` entra como opção, pois não há validação de revisão nem separador de opções. `--name-only` sobrepõe `-U0`, produz nomes sem patch; o parser aceita isso como zero escrita. Git real `--no-index -U0 --name-only` confirmou saída só de nomes; execução completa em repo está [nao verificado]. `bin/selo_proporcao.sh:19`, `bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:48`. **Não usar `--stat` como exemplo:** ele preserva o patch quando combinado com `-U0`. `diff.noprefix=true` sozinho também não basta para escapar.
- **ALTA — signal/command:** `+ps.connect(write_handler)` em `.py` fora de `signals.py`, com alias `ps` já existente, deu rc 0; `management/commands/w.py` com `+write = obj.save` / `+write()` também deu rc 0. Não há regra especial para management, apenas ORM/SQL/SIG; signal reconhece lista finita de nomes e `@receiver`. `bin/selo_proporcao.sh:24`, `bin/selo_proporcao.sh:37`, `bin/selo_proporcao.sh:40`.

Todos os rc 0 acima passam pela saída final `bin/selo_proporcao.sh:48`; os testes foram do parser e encadeamento originais, com diff sintético, não de commits existentes.

_3882 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
