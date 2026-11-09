# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 11:12** · publicado em **09/11/2026 11:12** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 11:12 · sessao be52b0625b15 · - · - · origem=Ronald (tmux)

Censo dos campos e leis (somente leitura).

A escrita da troca é indireta: `trocar_plantao` chama `editar_dia` uma vez por lado; o cedente recebe `trabalha=False`, o substituto `True` (`app/ponto/portas/celula.py:673-675`). A porta é o escritor declarado de `CelulaDia` (`app/core/portas.py:107-113`).

| Campo | Quem escreve | Quem lê encontrado |
|---|---|---|
| `CelulaDia.trabalha` | Na troca, `editar_dia`, create/save (`app/ponto/portas/celula.py:600-609`). Outros atos da mesma porta: regenerar (`:243-245`, `:259-276`), reverter (`:788-795`), gerar (`:849-871`). | Autoridade `EC.eh_dia_trabalho` (`app/escala/models.py:1087-1096`); leitor de grade (`app/escala/services/leitor_celula.py:381-411`); cartório (`app/ponto/services/cartorio.py:1408-1414`); detalhe do dia (`app/ponto/services/dia_do_colab.py:91`); trilha/idempotência (`app/ponto/portas/celula.py:528`, `:594`). |
| `CelulaDia.dna` | `editar_dia` constrói com `montar_dna(ec,dia,trabalha)` e grava (`app/ponto/portas/celula.py:591`, `:600-609`). Outros atos: regenerar (`:244-245`, `:256-276`), reverter (`:786-795`), gerar (`:849-871`). | `EC.marcos_do_dia` (`app/escala/models.py:1124-1133`), pausas (`:1189-1200`), extra declarada (`:1235-1238`); mapas de DNA (`app/escala/utils.py:46-53`, `:97`, `:130`); cartório/impressão/ata (`app/ponto/services/cartorio.py:1348`, `:1386`, `:1800`, `:1821`); juiz de batida (`app/ponto/juiz_batida.py:158`); detalhe do dia (`app/ponto/services/dia_do_colab.py:46`); trilha/idempotência (`app/ponto/portas/celula.py:528`, `:594`). |
| `CelulaDia.escala_geradora` | `editar_dia` grava o vínculo devolvido por `vinculo_do_dia_carregado` (`app/ponto/portas/celula.py:584`, `:600-609`). Regenerar (`:259-276`), reverter (`:789-795`), gerar (`:849-871`). | Autoridade do vínculo do dia: geradora tem precedência e deve cobrir a vigência (`app/escala/alimentacao.py:269-286`); trilha/idempotência (`app/ponto/portas/celula.py:529`, `:595`). |
| `CelulaDia.origem` | Troca grava `'editada'` em `editar_dia` (`app/ponto/portas/celula.py:600-609`); criação automática grava `'gerada'` (`:259-260`, `:849-852`). | Regeneração só reescreve gerada (`app/ponto/portas/celula.py:238`, `:263`), reversão só gerada (`:777`), geração pula editada (`:854-855`); tripwire só gerada (`app/ponto/management/commands/tripwire_celulas.py:21-22`); comparação com template só gerada (`app/escala/management/commands/celulas_contra_o_template.py:29`); detalhe do dia (`app/ponto/services/dia_do_colab.py:91`); trilha/idempotência (`app/ponto/portas/celula.py:528`, `:594`). |
| `CelulaDia.editada_em` | `editar_dia` (`app/ponto/portas/celula.py:598-609`). | Carimbos da reescrita/cobrança órfã (`app/escala/services/regeneracao.py:72-105`); detalhe do dia (`app/ponto/services/dia_do_colab.py:94`); fato novo do cartório (`app/ponto/services/cartorio.py:365`); invalidação de veredito de perguntas (`app/chamados/veredito_lavrado.py:176-180`); invalidação do censo de fase (`app/escala/services/fase_12x36.py:188-193`); trilha (`app/ponto/portas/celula.py:530`). |
| `CelulaDia.editada_por` | `editar_dia` (`app/ponto/portas/celula.py:600-609`). | Leitor explícito encontrado: `_estado_da_edicao`, para trilha (`app/ponto/portas/celula.py:531`). Outro leitor de produção da identidade desse campo: [nao verificado]. |
| `CelulaDia.colaborador/data` quando não existe célula | `editar_dia` cria a linha com a chave (`app/ponto/portas/celula.py:600`). | Seleção da própria edição (`:593`); autoridades consultam pela chave (`app/escala/models.py:1092-1093`, `:1122-1123`). |
| `CelulaDia.gerada_em` em criação | Automático do modelo (`app/escala/models.py:1702`), decorrente do create de `editar_dia` (`app/ponto/portas/celula.py:600`). | Invalidação das perguntas (`app/chamados/veredito_lavrado.py:178-179`) e fase (`app/escala/services/fase_12x36.py:190`). |

A edição não grava `regenerada_em/regeneracoes/dna_anterior`; isso é declarado e a lista de `update_fields` confirma (`app/ponto/portas/celula.py:538-539`, `:609`). O DNA do substituto usa o template do próprio vínculo, não o horário copiado do cedente (`app/ponto/portas/celula.py:510-511`, `:584-591`, `:635-637`).

Outros estados/trilha:

- Cada edição produz `LogAuditoria`: usuário, ação `editar_dia`, modelo `CelulaDia`, objeto, descrição, antes/depois dos seis campos, motivo e `apesar_da_lavra` (`app/ponto/portas/celula.py:611-618`). O par produz ação `trocar_plantao`, modelo `Ausencia`, objeto cedente, lados com colaborador/data/ausencia/documento/trabalha/resultado e motivo/override (`:682-691`). O escritor físico é `core.observ.evento`, via `evento_kw` (`app/core/observ.py:94-148`).
- Leitor genérico da trilha: `core.views.log_auditoria`, permissão `ver_saude_sistema`, filtro e leitura dos logs (`app/core/views.py:686-738`). A tela mostra usuário/ação/modelo/objeto/descrição/IP, sem renderizar o JSON antes/depois (`app/templates/core/log_auditoria.html:55-74`). Consumidor específico das ações `editar_dia/trocar_plantao`, fora a própria emissão, não encontrado na busca: [nao verificado].
- Cada lado marca cobranças órfãs (`app/ponto/portas/celula.py:625-626`): escreve `contexto_json.orfa_regeneracao={em,marcos_na_emissao}` pela porta `renovar_contexto` (`app/ponto/management/commands/gerar_celulas.py:241-255`). O julgamento dessa marca/reescrita é `cobranca_orfa`, alimentado por ambos os carimbos (`app/escala/services/regeneracao.py:87-105`, `:141-155`).
- O rabo da edição pode chamar julgamento do dia e recálculo financeiro (`app/ponto/portas/celula.py:627-628`, `:424-458`), além de rejunção e reavaliação de falta após commit (`:490-502`). Não confundir os seis campos explícitos da edição com os campos dos modelos alcançados por esses serviços (`:600-609`, `:424-502`).

Juiz:

- `JUIZES.md` é lookup e diz que registro vivo é `core/juizes.py` (`app/docs/JUIZES.md:8-14`). Lá estão dia de trabalho, minutos, precedência, ausência, status e folga (`app/core/juizes.py:170-193`), e geometria/marcos do turno (`:195-237`). Não encontrei juiz registrado para “este par pode trocar?”; esse predicado/registro é [nao verificado]. A função `trocar_plantao` é operação da porta, não aparece como juiz no registro (`app/ponto/portas/celula.py:81-82`; `app/core/juizes.py:170`).
- A tabela aponta permissão de tela para `tem_acao` e contextual para `pode_agir_chamado` (`app/docs/JUIZES.md:48-49`); a troca só exige `autor.pk` e motivo, sem chamar essas autoridades (`app/ponto/portas/celula.py:646-649`, `:652-691`).

Leis já escritas (não transformar em perguntas novas):

- Par, folga sem desconto, assumido normal com AN/intra sem HE100, duas cartas, confirmação pelo admin, pedido por qualquer lado: `app/docs/CORTES.md:187`.
- Escritor humano na mesma porta, origem editada, trilha, duas chamadas no mesmo atomic e exportada barrada: aval posterior O10-B literal em `app/docs/PROMPTS.md:1901`, também `app/docs/RELATO.md:480`; casos registram o literal em `app/ponto/tests/test_o10b_editar_dia_troca.py:4-9`.
- L-113 genérica revogou guarda exportada como trava (`app/docs/LEIS.md:151`), mas o aval O10-B de 06/11 explicitamente manda exportada barrada (`app/docs/PROMPTS.md:1901`). Convém citar ambos sem inventar nova decisão.
- Quem fornece lado substituto no SOLO: admin, nunca inferido; SOLO aplica zero pares; competência 09 só com `!` após DRY (`app/docs/PROMPTS.md:1907`; `app/docs/RELATO.md:431`).
- Fonte do dia/marcos é célula; template é gerador e leitores usam EC (`CLAUDE.md:145-154`).
- Autor/motivo/trilha em ação crítica e escritor único já estão escritos (`CLAUDE.md:32-34`).

Lacuna de lei expressamente registrada: “troca que cai em folga trabalhada pede decisão do admin?” (`app/docs/RELATO.md:449`; `app/docs/BACKLOG.md:1876`). Não reduzir o número final só às lacunas novas sugeridas: essa pergunta já consta como aberta.

Para agrupamento das demais perguntas do usuário, as decisões específicas de recusa de par por interjornada/sobreposição, empresas/vínculos diferentes, datas iguais/diferentes e tipo/status/período das cartas não aparecem na porta (`app/ponto/portas/celula.py:632-700`). Os 11h são mencionados como “NÃO valida” em outra função de cobertura (`app/ponto/utils.py:152-155`), o que comprova referência existente, não uma lei de elegibilidade da permuta. “Qual regra deve valer?” nesses pontos é [nao verificado].

## 09/11 11:12 · sessao e6a1ea29d182 · - · - · origem=Ronald (tmux)

Item 3 — censo dos leitores:

| Leitor | Fonte comprovada |
|---|---|
| Espelho admin | A view chama `espelho_do_colab` (`app/ponto/views.py:157-158`); o previsto na competência vem de `grade_da_celula` (`app/ponto/services/espelho.py:506-520`). |
| Espelho app | Chama a mesma autoridade `espelho_do_colab` (`app/api/views.py:1379-1385`), mas os dias sem card buscam os horários diretamente no template (`app/api/views.py:1611-1614`). |
| PDF espelho | Busca os dias em `espelho_do_colab` (`app/relatorios/pdf_espelho.py:596-598`), porém a coluna **Previsto** usa `tipo_escala.hora_inicio/hora_fim`, imprimindo-os em cada linha com turnos (`app/relatorios/pdf_espelho.py:903-909`, `:978`). |
| Calendário admin | Consulta `grade_da_celula` (`app/colaboradores/services/calendario.py:332-334`), lê status por `status_dos_dias_lavrados` (`:602-607`) e dinheiro por `DiaPago` (`:290`, `:655-656`). A view consumidora está em `app/colaboradores/views.py:1945-1946`. |
| Calendário/escala API app | `/api/escala/v2/` registrado em `app/api/urls.py:72`; trabalho/folga vem de `escala.eh_dia_trabalho(data)`, porém horários vêm de `te.marcos_do_dia(data)` (`app/api/views.py:934-955`), publicados em `:968-971`. |
| Escala web/PWA | Usa ambos `ec.eh_dia_trabalho` e `ec.marcos_do_dia` (`app/escala/views.py:38-44`); a agenda semanal administrativa também usa ambos (`app/colaboradores/views.py:2223`, `:2232-2234`). |
| Folha/motor | O fechamento consulta `esc.eh_dia_trabalho` (`app/ponto/services/fechamento.py:729-732`), alimenta o motor com células (`:750`) e chama `apurar_por_fatia` (`:754-755`); esta passa `escala_colaborador=esc` ao motor (`:278-290`). O motor prefere `EC.marcos_do_dia` (`app/ponto/motor_calculo_v2.py:2290-2291`) e pergunta `EC.eh_dia_trabalho` (`:2447-2448`). |
| TXT | As verbas são lidas de `FechamentoMensal`, sem novo motor (`app/folha/export.py:703-724`); retenção consulta `CelulaDia` (`:1206`, `:1233-1256`); `grade_do_fechamento` delega a `grade_da_celula` (`:329-338`). Datas de falta vêm de ausências cujo efeito vigente desconta, não de template (`:169-185`). |

As autoridades realmente passam pela célula: `EC.eh_dia_trabalho` retorna `cel.trabalha` (`app/escala/models.py:1087-1096`); `EC.marcos_do_dia` lê `cel.dna.marcos` antes do fallback de template (`:1117-1133`). `grade_da_celula` carrega `CelulaDia` e lê sua ata/trabalha (`app/escala/services/leitor_celula.py:345-381`).

**Leitores diretos da base encontrados nos caminhos pedidos:**

- Calendário API app: horários dos dias via template (`app/api/views.py:955`); o trabalho/folga continua passando pela célula (`:950`).
- Espelho app, dias sem card: horários via template (`app/api/views.py:1613`, `:1639-1642`); trabalho/folga passa pela célula (`:1611`).
- Bloco escala de `/api/me/`, nas duas cascas: horários via template (`app/api/views.py:259-274`; `app/api/views_core.py:248-263`).
- Coluna Previsto do PDF: campos crus do template, não DNA (`app/relatorios/pdf_espelho.py:903-909`, `:978`).
- Cabeçalhos da escala: resumo da tela usa horas cruas do template (`app/ponto/services/espelho.py:201-212`, `app/templates/ponto/espelho.html:142`); PDF idem (`app/relatorios/pdf_espelho.py:730-740`); “minha escala” apresenta o padrão no cabeçalho (`app/templates/escala/minha_escala.html:33-34`), mas os horários das linhas vêm do EC (`app/escala/views.py:43-44`, `app/templates/escala/minha_escala.html:78-81`).
- Na degradação antiga, `montar_grade_prevista` consulta trabalha pelo EC (`app/escala/utils.py:760-762`), mas pode consultar horas no template sem mapa de DNA (`:767-768`); há chamador sem mapa em `app/escala/utils.py:1016`. O caminho normal `grade_espelho_janela` usa o builder de período (`:2337`), que consulta trabalha e mapa DNA antes do fallback (`:1605-1607`).

**Mostrariam errado?** Não há prova de divergência concreta do par real: **[nao verificado]**. Os caminhos diretos podem publicar horários diferentes da célula quando o template/escala ativa difere do DNA/geradora do dia; os sítios acima demonstram essa separação. Não é correto afirmar que ignoram toda a permuta: os caminhos do app consultam `trabalha` pela célula (`app/api/views.py:950`, `:1611`, `:260`).

Também não se pode assumir que o substituto receberia as horas do cedente: a porta atual faz o substituto trabalhar com o **próprio template** (`app/ponto/portas/celula.py:635-637`), chama `montar_dna` (`:591`), que congela os marcos de `ec.tipo_escala` naquela data (`app/ponto/management/commands/gerar_celulas.py:73-84`). Portanto, logo após a edição e sem diferença entre template atual e geradora, usar template pode coincidir com o novo DNA; o dano efetivo depende de dados não fornecidos **[nao verificado]**.

Outros leitores diretos encontrados, fora dos cinco consumidores principais: correção E/S retroativa nas duas APIs (`app/api/views.py:619-625`, `app/api/views_core.py:612-617`); `dia_posicional_completo` (`app/ponto/propositor.py:334-343`); defesa de pergunta `_marco_esperado_da_grade` (`app/chamados/juizes.py:1546-1563`); horário da cobrança “abriu e não bateu” (`app/ponto/services/abriu_nao_bateu.py:193-203`). Não confundir com fallbacks que primeiro consultam a célula: `veredito_celula._marcos_do_dia` (`app/ponto/services/veredito_celula.py:29-43`) e `marcos_turno` quando recebe `ec` (`app/escala/servico_jornada.py:68-71`).

## 09/11 11:11 · sessao d51be0d9df52 · - · - · origem=Ronald (tmux)

Item 1 — há canais genéricos para o colaborador pedir; não encontrei modelo/endpoint próprio que registre **o par de permuta** estruturado com dois colaboradores e duas datas. A conclusão negativa deve ficar como “[nao verificado] modelo próprio de pedido de permuta”; o que está comprovado é o modelo genérico `Ausencia`, sem campos de par, substituto ou dia assumido (`app/ponto/models.py:743-814`), e a solicitação textual em `ChamadoColaborador` (`app/chamados/models.py:38-52`).

**1) Ausência enviada pelo app/PWA**

- App: `GET/POST /api/ausencias/` → `api_ausencias`; autenticado e vinculado ao colaborador do usuário (`app/api/urls.py:82`; `app/api/views.py:1700-1706`).
- PWA: `/ponto/ausencias/submeter/` → `submeter_ausencia`, formulário `ponto/submeter_ausencia.html` (`app/ponto/urls.py:64`; `app/ponto/views.py:2466-2468`; `app/ponto/views.py:2560-2564`).
- Entrada: `tipo`, `data_inicio`, `data_fim`, `descricao`, documento e minutos; chama `criar_ausencia` com `registrado_por=request.user` (`app/api/views.py:1736`; `app/api/views.py:1788-1791`; `app/api/views.py:1830-1842`; `app/ponto/views.py:2476-2480`; `app/ponto/views.py:2536-2542`).
- Tipo é validado no catálogo vivo/cadastro `TipoAusencia`; a API também oferece as choices vigentes. Portanto, a disponibilidade de `troca_de_plantao` no app depende do cadastro que esta cópia não demonstra — **[nao verificado] no dado fornecido** (`app/ponto/services/ausencia.py:193-195`; `app/api/views.py:1883`; `app/ponto/catalogo/ausencias.py:17-23`). A lista estática de `Ausencia.TIPO_CHOICES` não contém `troca_de_plantao` (`app/ponto/models.py:661-679`).
- Campos persistidos: colaborador, tipo, data/data_fim, minutos/dias corridos, horários de cobertura parcial, hora_extra_100, descrição, documento, prazo_documento, chamado_origem, documento_referencia, status, registrado_por, aprovado_por/aprovada_em e timestamps (`app/ponto/models.py:743-814`).
- Estados: `aguardando_documento`, `aguardando_decisao`, `aprovada`, `rejeitada` (`app/ponto/models.py:701-705`). Na criação, `criar_ausencia` chama `estado_inicial`: com documento ou tipo que não exige documento → aguarda decisão; tipo que exige sem documento → aguarda documento (`app/ponto/services/ausencia.py:331-345`; `app/ponto/services/ausencia.py:790-797`).
- Escrita posterior do status é centralizada em `mudar_estado` (`app/ponto/services/ausencia.py:808-835`). `anexar_documento` muda de aguardando documento para aguardando decisão (`app/ponto/services/ausencia.py:846-875`); `decidir` aceita aprovada/rejeitada, como ato DP (`app/ponto/services/ausencia.py:897-927`); escalonamento de documento vencido também passa a aguardando decisão, sem declarar falta (`app/ponto/services/ausencia.py:992-1006`).
- App e PWA abrem chamado genérico `ausencia_colab` após criar a ausência (`app/api/views.py:1871-1874`; `app/ponto/views.py:2546-2547`). Esse chamado é aviso persistente ao DP, não decide a ausência; guarda `ausencia_id`, data, tipo e presença de documento em contexto JSON (`app/ponto/services/ausencia.py:637-644`; `app/ponto/services/ausencia.py:659-678`).

**Leitores comprovados dessas Ausencias**

- App GET devolve últimas 30 do próprio colaborador, incluindo status/descrição/minutos (`app/api/views.py:1876-1895`).
- Admin `/ponto/ausencias/`: exige `ver_ausencias`, consulta vigentes ou registro específico e filtra tipo/status; botões/endpoints de aprovar/rejeitar/anexar/editar existem (`app/ponto/views.py:2155-2187`; `app/ponto/urls.py:53-76`).
- API mensageria/DP lê `ausencias_do_dp`, conta por tipo/status e lista aguardando decisão (`app/api/views_mensageria.py:736-762`).
- Comando `escalonar_documentos_ausencia` chama o escalonador (`app/ponto/management/commands/escalonar_documentos_ausencia.py:25-26`); o escalonador lê fila aguardando documento e pode escrever aguardando decisão (`app/ponto/services/ausencia.py:992-1006`). **Execução atual desse cron em produção: [nao verificado].**
- Fechamento do aviso: `chamados_da_ausencia_decidida` seleciona `ausencia_colab` vivos e consulta ausência associada; `fechar_chamado_da_ausencia` reconcilia quando ausência sai dos estados abertos (`app/ponto/services/ausencia.py:686-716`). O lookup de crons declara `fechar_chamados_ausencia` a cada 30 minutos, mas execução real não é comprovada pelo código (`app/docs/CRONS.md:25`).
- Signals de Ausencia existem como consumidores em save/delete (`app/ponto/signals.py:144`; `app/ponto/signals.py:151`; `app/ponto/signals.py:250`; `app/ponto/signals.py:276`; `app/ponto/signals.py:283`).

**2) Solicitação textual / chamado**

- Existe taxonomia `Escala / Turno` com subcategorias “Troca de turno”, “Folga”, “Plantao” e palavras substituto/substituir (`app/chamados/classificador.py:38-49`).
- App `POST /api/chamados/abrir/` → `api_abrir_chamado`: aceita assunto, descrição, urgência, área, categoria e subcategoria; grava `ChamadoColaborador` com `modulo_origem='app_colaborador'` e nota da abertura (`app/api/urls.py:87`; `app/api/views.py:2151-2177`; `app/api/views.py:2201-2214`).
- Web/PWA `/chamados/abrir/` → `abrir_chamado`, chama `abrir_chamado_local` e renderiza `chamados/form_abertura.html` (`app/chamados/urls.py:10`; `app/chamados/views.py:147`; `app/chamados/views.py:196-215`).
- Não é modelo novo chamado “Solicitacao”: rótulo “Solicitação” decorre de `modulo_origem in ('chamados','app_colaborador')` (`app/chamados/rotulos.py:4-5`; `app/chamados/rotulos.py:26-38`).
- Campos principais: colaborador, assunto/título/descrição, área/categoria/subcategoria, módulo, contexto JSON, célula opcional, status_local, urgência, respondente/resolução, atribuição e SLA (`app/chamados/models.py:38-85`).
- Estados genéricos: aberto/em_analise/resolvido/fechado/cancelado/registrado/superado (`app/chamados/models.py:28-35`). Nascimento passa por `ChamadoColaborador.criar` e motor; escritor central posterior é `_gravar_estado` (`app/chamados/models.py:1054-1067`; `app/chamados/models.py:1093-1099`; `app/chamados/models.py:201-228`). Gestor muda estado por `transicionar` no serviço de ações (`app/chamados/services/acoes_chamado.py:313-319`).
- Leitores: painel DP consulta `ChamadoColaborador` (`app/chamados/services/painel.py:96-115`); “Meu Atendimento” separa solicitações e pendências e passa ao template (`app/chamados/views.py:612-642`); API de chamados carrega chamados visíveis do colaborador (`app/api/views.py:1031-1039`). A criação envia push para supervisão ou DP conforme área, com link painel (`app/api/views.py:2216-2228`).
- A apresentação da nova solicitação no PWA é controlada por parâmetro empresarial `app_solicitacoes`, desligado por padrão; quem tem solicitação já existente ainda vê aba (`app/chamados/services/solicitacoes_no_app.py:1-20`; `app/chamados/views.py:615-642`). **Valor real desse parâmetro: [nao verificado].**
- Leitor automático genérico de chamados: comando `alertar_chamados_sla` lê fila e registra ocorrência/push de SLA (`app/chamados/management/commands/alertar_chamados_sla.py:20`; `app/chamados/management/commands/alertar_chamados_sla.py:44-55`); execução real **[nao verificado]**.

**3) Pergunta**

Existe resposta a `PerguntaDisputa` pelo app, mas ela depende de pergunta existente da disputa do próprio colaborador; não é endpoint para abrir pedido estruturado de permuta (`app/api/views.py:2616-2619`; `app/api/views.py:2643-2655`). GET de perguntas usa `perguntas_do_app` e estados da pergunta (`app/api/views.py:2565-2568`; `app/api/views.py:2583-2585`). **Uma resposta específica de permuta materializar o par: [nao verificado].**

Não importar as contagens antigas de produção presentes em comentários/RELATO como dado atual: o anexo fornecido não contém registros de pedidos, tipos cadastrados ou flags por empresa — **[nao verificado]**.

## 09/11 10:59 · sessao 6efcaa755ca3 · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · construtor · origem=Code

Implementei P1/P4/P5 e escrevi primeiro os REDs, incluindo P2/P3 reservados à sessão. **RED executado, GREEN, mortes dos mutantes e resultado em produção: [nao verificado].**

**Entrega por hunk**

| Arquivo:linha | Alteração |
|---|---|
| [materializacao.py:109](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/services/materializacao.py:109) | Espera exige a própria metade validada. |
| `chamados/services/materializacao.py:175` | Vizinhança usa `batidas_apuraveis`, autoridade que exclui retratadas (`ponto/turnos.py:2866`). |
| [acoes_chamado.py:348](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/services/acoes_chamado.py:348) | Detalhe inclui C6 viva **ou** metade em espera pelo mesmo predicado. |
| [test_bug_par_intervalo.py:164](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/tests/test_bug_par_intervalo.py:164) | P1: S retratada 01:30; valida S01/E02 e cobra alternância canônica, carimbos e fechamento. |
| `chamados/tests/test_bug_par_intervalo.py:457` | Foto de estado/trilha; P2 cobra zero saves, eventos e agendamentos em duas chamadas (`:478`); P3 chama `fechar` real e exige consulta C7 após trava (`:497`). |
| `chamados/tests/test_bug_par_intervalo.py:528` | P4: duas respondidas sem aceite; explicador, declaração do colaborador e aplicação pelo administrador (`:543`, `:556`). |
| `chamados/tests/test_bug_par_intervalo.py:623` | P5: GET real do detalhe e modal, sem contexto injetado; cobra include e aviso HTML (`:627`, `:631`). |
| `chamados/tests/test_bug_par_intervalo.py:702` | P6: carimbo inicialmente vazio; primeira falha cobra 4 saves, 4 agendamentos e 2 eventos; segunda cobra zero novas escritas e foto idêntica (`:726`, `:743`). |
| [mutantes_bug_par_intervalo.py:131](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/tests/mutantes_bug_par_intervalo.py:131) | M38 restaura a guarda exata da volta 2; acrescentados M39–M44. |
| `chamados/tests/mutantes_bug_par_intervalo.py:205` | Inclui `acoes_chamado.py` no `py_compile`; `--comandos-v4` imprime os comandos individuais com settings CI (`:226`). |

Os novos testes usam a classe `TestCase`, relógio congelado e fixture `col960` existentes (`chamados/tests/test_bug_par_intervalo.py:32`, `:45`).

**Quem escreve e quem lê**

| Campo/função | ESCRЕVE / dono | LÊ, inclusive fora do diff |
|---|---|---|
| Espera do par | Predicado sem escrita: `materializacao.py:101`. | C7 `chamados/models.py:1864`; explicador `explicador.py:74`; declaração `declaracao_texto.py:213`, `:382`, `:678`; validação `validacao.py:193`, `:483`; vigia `vigia_carimbo.py:109`; tag `chamados_tags.py:19`. |
| Vizinhança / universo | Consulta sem escrita: `materializacao.py:169`; autoridade `ponto/turnos.py:2846`. | Aplicação individual `materializacao.py:962`; julgamento posterior do par `:1364`; apresentação `:539`; reexport `chamados/models.py:4080`. |
| `perguntas_disputa` | Contexto do detalhe `acoes_chamado.py:349`; contexto independente do modal `fio.py:253`. | View `chamados/views.py:549`; detalhe `templates/chamados/detalhe_local.html:309`; modal `templates/chamados/partials/modal_fio.html:156`. |
| `validada_em/por` | Aceite `validacao.py:134`; devolução `:202`; nova resposta `disputa_emissao.py:1477`; complemento pelo carimbo `chamados/models.py:3138`. | Espera `materializacao.py:109`; C6 `chamados/juizes.py:1153`; C7 `chamados/models.py:1850`. |
| `materializada_em/por`, `via_resolucao` | `_escrever_carimbo`, `chamados/models.py:3100`; descarimbo `:3211`; lote `:3269`; relavra `relavra_via.py:125`. | Lastro `chamados/juizes.py:660`; C6 `:1164`; devolução `validacao.py:188`; lavra persistida `veredito_lavrado.py:187`. |
| `Batida.retratada_em` | Retração `ponto/registro_batida.py:276`, por `anotar` (`:255`). | Universo canônico `ponto/turnos.py:2866`; tolerância `ponto/registro_batida.py:99`; lastro `ponto/services/lastro.py:262`. |
| Fotos e definições dos testes | Fixtures/helpers `test_bug_par_intervalo.py:81`, `:88`, `:472`, `:767`. | Asserções `:495`, `:526`, `:745`; alvos do runner `mutantes_bug_par_intervalo.py:131`. | [nao verificado]
| `MUTANTES` / fontes | Definições `mutantes_bug_par_intervalo.py:131`. | Leitura/recompilação em memória `:166`, `:179`, `:249`; conferência estática `:217`. |

O save do descarimbo agenda beco e pode emitir evento pelos receivers (`chamados/signals.py:225`, `:247`; `chamados/models.py:3252`). Exaustividade transitiva deste mapa: **[nao verificado]**.

**Linhas exatas para a SESSÃO escrever em `chamados/models.py`**

P2: após o retorno de `fora_do_fato`, antes de `_escrever_carimbo` (`chamados/models.py:3209`, `:3211`):

```python
        if (self.materializada_em is None
                and self.materializada_por_id is None
                and self.via_resolucao == ''):
            return []
```

P3: antes do `if motivo is None` (`chamados/models.py:1908`):

```python
        pelo_juiz = motivo is None
```

Após a chamada completa de `trava_na_ordem` (`chamados/models.py:1923`):

```python
            if pelo_juiz and not self.pode_fechar():
                raise ValueError(
                    'Nao e possivel fechar a disputa #%s: C7 reteve o fechamento.' % self.pk)
```

É necessário conservar a modalidade original porque `motivo` recebe `FECHOU_VAZIA` ou `FECHOU_VEREDITO` antes da trava (`chamados/models.py:1914`).

**Mutantes e asserções discriminantes previstas**

| Mutante | Regressão | Asserção que deve matar |
|---|---|---|
| M38 | Guarda exata v2 | Ambas passam por `descarimbar` mesmo inicialmente vazias (`test_bug_par_intervalo.py:724`). |
| M39 | Universo cru inclui retratada | Segunda validação aceita o par (`:176`). |
| M40 | Espera sem validação própria | Predicado falso para metade apenas respondida (`:533`). |
| M41 | Detalhe volta a filtrar só C6 | Metade em espera presente no contexto GET (`:641`). |
| M42 | Aplicação retorna antes de validar | Própria metade recebe validação (`:566`). |
| M43 | Declaração prematuramente em espera | Retorna `ja_declarada`, aguardando supervisão (`:550`). |
| M44 | Tag omite espera no modal | Aviso HTML presente uma vez no GET real (`:642`). |

M38 cobra a passagem pela porta; após P2, comparar somente estado/trilha não distingue a guarda antiga que pula um no-op (`test_bug_par_intervalo.py:724`, `:745`). Mortes executadas: **[nao verificado]**.

Comandos individuais reservados ao Code; equivalentes aos impressos por `--comandos-v4` (`mutantes_bug_par_intervalo.py:226`):

```bash
export DJANGO_SETTINGS_MODULE=config.settings.ci
export PYTHONDONTWRITEBYTECODE=1

teste() { [nao verificado]
  python3 manage.py test "chamados.tests.test_bug_par_intervalo.BugParIntervaloCol960.$1" --noinput
}
mutante() {
  python3 -m chamados.tests.mutantes_bug_par_intervalo "$1"
}

teste test_V4_P1_par_aplica_com_S_retratada_entre_metades [nao verificado]
teste test_V4_P2_descarimbar_vazio_duas_vezes_nao_escreve_nem_agenda [nao verificado]
teste test_V4_P3_fechar_real_consulta_C7_sob_trava_e_retem_espera [nao verificado]
teste test_V4_P4_respondidas_sem_aceite_aguardam_validacao [nao verificado]
teste test_V4_P4_declaracao_colab_sem_aceite_aguarda_supervisao [nao verificado]
teste test_V4_P4_aplicar_declaracao_valida_a_propria_metade [nao verificado]
teste test_V4_P5_GET_detalhe_conserva_metade_em_espera [nao verificado]
teste test_V4_P5_GET_modal_conserva_metade_em_espera [nao verificado]
teste test_V4_P6_falha_com_carimbo_vazio_duas_vezes_mesmo_estado_e_trilha [nao verificado]

mutante M38
mutante M39
mutante M40
mutante M41
mutante M42
mutante M43
mutante M44
```

Executei somente a conferência estática abaixo: **rc 0**, `py_compile` dos quatro Python tocados sem persistência e sintaxe/âncoras M1–M44 conferidas (`mutantes_bug_par_intervalo.py:195`, `:211`, `:217`). [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo --sintaxe
```

**DECISAO-ABERTA:** P2/P3 aguardam escrita da sessão; P6 depende de P2. A inserção proposta consulta C7 sob trava, mas as leituras anteriores de respostas/perguntas permanecem antes dela (`chamados/models.py:1897`, `:1906`, `:1921`). Prova concorrente, GREEN e certificação pelo Code/opus: **[nao verificado]**.

## 09/11 10:57 · sessao 476fb91835ff · - · - · origem=Ronald (tmux)

Considerando os novos comandos com `config.settings.ci`, não encontrei bloqueador concreto dos GETs por inspeção: `ci.py:5` seleciona `config.urls_ci`, `config/urls_ci.py:9` inclui as rotas de chamados e os middlewares de sessão/autenticação/mensagens vêm de `config/settings/base.py:47`, `:50`, `:55`. O administrador da fixture tem `is_staff=True` e `is_superuser=True` (`chamados/tests/test_bug_par_intervalo.py:41`), aceitos pelo gestor e por RBAC (`chamados/views.py:35`; `colaboradores/mixins.py:328`). Execução permanece [nao verificado].

O modal recebe perguntas de disputas abertas sem filtro C6 (`chamados/services/fio.py:221`), associa a disputa em `:258` e o template entra pelo mapa real de permissão (`templates/chamados/partials/modal_fio.html:122`). O detalhe passa pelo contexto real da view (`chamados/views.py:549`) e pelo include em `templates/chamados/detalhe_local.html:310`. Os novos testes não injetam contexto (`chamados/tests/test_bug_par_intervalo.py:633`).

As contagens do P6 correspondem ao caminho inspecionado **depois da cura P2**:

- Duas recusas salvam apenas `recusa_motivo` (`chamados/services/materializacao.py:1195`), cada uma agenda um beco (`chamados/signals.py:225`) e não emite outbox porque esse campo não pertence a `CAMPOS_QUE_ACORDAM` (`chamados/models.py:3224`; `chamados/signals.py:245`).
- Duas devoluções salvam validação (`chamados/services/validacao.py:205`), agendam outros dois becos e emitem os dois eventos (`chamados/signals.py:225`, `:247`; `chamados/models.py:3252`).
- A segunda recusa idêntica não salva (`chamados/services/materializacao.py:1183`, `:1192`); validação já vazia não salva (`chamados/services/validacao.py:198`) e foto idêntica não audita (`:222`). As notas também dependem da mudança do motivo (`chamados/services/materializacao.py:1195`).

O registro de trava em `on_commit` não deve inflar a primeira contagem: `_travar_ato_materializacao` está antes da captura (`chamados/tests/test_bug_par_intervalo.py:714`), e `_registrar_travados` não adiciona novamente colaborador já registrado (`chamados/services/trava_do_colaborador.py:42`). Resultado executado: [nao verificado].

Única lacuna adicional do P6: o teste configura a falha fixa, mas não cobra que `_materializar_resposta_hora` foi realmente alcançada; uma recusa anterior estável poderia satisfazer parte das mesmas asserções (`chamados/tests/test_bug_par_intervalo.py:719`). Capturar esse mock e cobrar uma chamada por tentativa reforça a origem pretendida da falha. [nao verificado]

## 09/11 10:53 · sessao 476fb91835ff · - · - · origem=Ronald (tmux)

Mapa por leitura estática:

| Função/campo | Escritor/dono | Leitores fora do diff |
|---|---|---|
| `aguarda_outra_metade_intervalo` | Predicado sem escrita: `chamados/services/materializacao.py:101`. | C7 `chamados/models.py:1864`; explicador `chamados/services/explicador.py:74`; declaração `chamados/services/declaracao_texto.py:213`, consumida em `:382`, `:485`, `:678`; validação/devolução `chamados/services/validacao.py:193`, `:483`; vigia `chamados/services/vigia_carimbo.py:109`; tag `chamados/templatetags/chamados_tags.py:19`; estado de aplicação `chamados/services/materializacao.py:527`. |
| `_vizinha_mesmo_tipo` | Consulta sem escrita: `chamados/services/materializacao.py:169`. | Estado da aplicação `chamados/services/materializacao.py:542`; aplicação individual `:965`; par posterior `:1367`; reexport em `chamados/models.py:4080`. |
| `batidas_apuraveis` | Autoridade sem escrita, filtro de retratação em `ponto/turnos.py:2866`. | Espelho `ponto/services/espelho.py:1022`; turno órfão `ponto/utils_turno_orfao.py:28`; diagnóstico `ponto/services/diagnostico_escala.py:142`; fio `chamados/services/fio.py:1521`, `:1881`; validação `chamados/services/validacao.py:896`; consultas em lote `ponto/turnos.py:2989`, `:3009`. |
| Contexto `perguntas_disputa` do detalhe | `chamados/services/acoes_chamado.py:348`, publicado em `:409`. | View GET `chamados/views.py:549`; loop `templates/chamados/detalhe_local.html:309`; include `:310`. Modal possui escritor próprio em `chamados/services/fio.py:253`, consumido por `templates/chamados/partials/modal_fio.html:156`. |
| `validada_em/por` | Aceite `chamados/services/validacao.py:134`; devolução `:202`; nova resposta `chamados/services/disputa_emissao.py:1477`; reabertura em lote `:1590`; carimbo completando validação `chamados/models.py:3138`; reabertura da disputa `:1565`. | C6 `chamados/juizes.py:1153`; foto da validação `chamados/services/validacao.py:147`; crédito por autor `chamados/services/verbos.py:240`; C7 `chamados/models.py:1850`. |
| `materializada_em/por/via_resolucao` | Porta comum `_escrever_carimbo`, `chamados/models.py:3100`–`:3104`; descarimbo `:3211`; lote `:3269`; via por fato `:3278`; relavra `chamados/services/relavra_via.py:125`. | Lastro `chamados/juizes.py:660`, `:672`, `:687`; C6 `:1164`, `:1190`; devolução `chamados/services/validacao.py:188`; lavra persistida `chamados/veredito_lavrado.py:187`. |
| `Batida.retratada_em` | Retração pela porta `ponto/registro_batida.py:276`, escrita efetiva por `anotar` em `:255`; desfazer humano por `anotar` em `ponto/services/retratar_humano.py:207`. | Universo canônico `ponto/turnos.py:2866`; tolerância `ponto/registro_batida.py:99`; lastro `ponto/services/lastro.py:262`; vigia de carimbo `chamados/services/vigia_carimbo.py:119`. |

Efeitos do `save` de descarimbo: agendamento pelo receiver `chamados/signals.py:215`–`:225`; emissão por `chamados/signals.py:242`; resolução da célula e chamada da porta em `chamados/models.py:3235`–`:3252`. Ausência de deduplicação nessa emissão consta da porta `ponto/portas/celula.py:891`.

Texto mínimo para a SESSÃO, sem edição realizada:

P2: inserir entre o retorno de `fora_do_fato` e `_escrever_carimbo`, em `chamados/models.py:3211`:

```python
        if (self.materializada_em is None
                and self.materializada_por_id is None
                and self.via_resolucao == ''):
            return []
```

P3: guardar a modalidade original antes do `if motivo is None` de `chamados/models.py:1909`:

```python
        pelo_juiz = motivo is None
```

Após a chamada completa de `trava_na_ordem`, em `chamados/models.py:1923`, inserir:

```python
            if pelo_juiz and not self.pode_fechar():
                raise ValueError(
                    'Nao e possivel fechar a disputa #%s: C7 reteve o fechamento.' % self.pk)
```

A flag precisa conservar a entrada original: antes da trava, o código já converte `motivo=None` em `FECHOU_VAZIA` ou `FECHOU_VEREDITO`; verificar `motivo is None` somente depois disso tornaria a guarda inoperante. (`chamados/models.py:1916`)

Essa inserção consulta C7 sob trava, mas as leituras anteriores de `_pend`, `perguntas` e `vivas` permanecem anteriores à trava; proteção integral dessas leituras exigiria movê-las para dentro do bloco após a trava. (`chamados/models.py:1897`, `:1907`, `:1921`)

Exaustividade transitiva do mapa: [nao verificado]. Nenhum teste, sintaxe, banco, rede ou escrita foi executado por este explorer. [nao verificado] [nao verificado]

## 09/11 10:52 · sessao f14c3a354d98 · SONDA-SEM-GRANT · verificador · origem=Code

**Não fecharia a volta 4:** permanecem caminhos que conservam carimbo verde após alteração da sombra e uma limpeza que pode remover container alheio por colisão de nome. (bin/sombra.sh:214,225,228,430,433; bin/tests/test_sonda_sem_grant_pg.sh:41,50,93,95)

1. **Itens PROCEDENTES da triagem v3**

| Item | Fechamento e mordida |
|---|---|
| **2a — fechado; NÃO PROCEDE reabrir a lacuna original.** | SUPERUSER é aplicado; o selo exige `super>0`, rc 5, recusa nomeada e sonda não executada. O log registra precisamente essas falhas para `super=0`. (bin/tests/test_sonda_sem_grant_pg.sh:183,185,186,187,207; anexo sonda_v4_mut.log:11) |
| **2b — fechado para `$PGU → leitor`; NÃO PROCEDE reabrir esse caso.** | O estrago de schema grantable passa pelas mesmas cobranças; a porta agora revoga esse default. O mutante da medição morre na detecção; retirar o REVOKE morre na recuperação pela porta. (bin/tests/test_sonda_sem_grant_pg.sh:183,189,208; bin/sonda_provisionar.sh:132; anexo sonda_v4_mut.log:16,31) |
| **4 — guarda original fechada; residuais abaixo são PROCEDENTES.** | `carimbar` verifica as operações e `refazer` exige invalidação antes do drop. O log mata ausência da guarda e retorno do `mv` ignorado pela asserção “chamou dropdb”. (bin/sombra.sh:94,95,98,99,101,275,276; bin/tests/test_sonda_sem_grant.sh:408; anexo sonda_v4_mut.log:1,26) |
| **6 — fechado no pedido literal; NÃO PROCEDE reabrir invalidado/ontem.** | Placar retorna `?` nesses casos; invalidação também apaga INCOMPLETA. O log registra a comparação esperada matando a leitura antiga. (bin/placar_code.sh:37; bin/sombra.sh:110; bin/tests/test_sonda_sem_grant.sh:434,436,439; anexo sonda_v4_mut.log:6) |
| **10 — silêncio original corrigido; identidade ainda PROCEDENTE.** | O selo cobra rc preservado, falha nomeada e `rm -f` exato; o log mata a implementação antiga nessas cobranças. Isso não demonstra autoria do container removido. (bin/tests/test_sonda_sem_grant_pg.sh:40,44,48,76,80,82; anexo sonda_v4_mut.log:21) |

O log fornecido registra sete mortes compatíveis com as asserções indicadas; patches exatos dos mutantes, comandos completos, baseline e reexecução independente são **[nao verificado]**. (anexo sonda_v4_mut.log:1,6,11,16,21,26,31)

**PROCEDENTE — ressalva no selo 9c:** no caso `mv`, o STAMP aponta para `sem_dir/stamp`, mas a asserção de integridade compara outro arquivo, `RF/stamp`. Ela não prova preservação do carimbo existente após falha do `mv` final. Cura sugerida: provocar a falha final mantendo STAMP apontado para o carimbo verde efetivamente examinado. A mordida contra alcançar `dropdb` continua válida. (bin/tests/test_sonda_sem_grant.sh:394,396,399,408,411)

2. **T4: outros caminhos**

- **PROCEDENTE — bloco:** começando com hoje/OK, nova execução pode calcular FALHOU, falhar em `carimbar` e terminar com o `echo` bem-sucedido. O OK anterior permanece; `conferir` pode aprová-lo enquanto houver migrations. Cura sugerida: invalidar sob a trava antes de executar o bloco, exigir publicação final e testar erro do bloco combinado com falha de gravação. (bin/sombra.sh:356,407,417,429,430,433,455,459)
- **PROCEDENTE — incremental:** a transação altera a janela antes da prova e das máscaras, sem invalidar o carimbo. Falha posterior conserva o verde anterior; falha em `carimbar` também é engolida pelo `echo`. Se o carimbo anterior era completa/OK, a cobertura ainda aceita “completa”. Cura sugerida: invalidar antes da alteração, guardar explicitamente os insumos necessários e exigir publicação final. (bin/sombra_incremental.sh:61,66,68,81,85; bin/sombra.sh:214,216,218,225,228; app/core/sombra_cobertura.py:39,40,41)
- **NÃO PROCEDE — fim do refazer:** `carimbar` é o último comando da função, portanto seu rc se propaga; se falhar, a invalidação anterior permanece. `--dump-agora` converge para a mesma guarda antes do drop; `conferir` não escreve o carimbo. (bin/sombra.sh:110,259,275,276,328,331,332,436,459)

3. **Predicados: NÃO são iguais**

| Caso | Placar | `conferir`, supondo migrations positivas |
|---|---|---|
| Dia vazio ou ontem | `?` | Recusa |
| Hoje, status vazio, INCOMPLETA=0 | `0` | Recusa |
| Hoje, OK, INCOMPLETA ausente | `?` | Aceita |
| Hoje, REFEITA/DIVERGE/FALHOU, número presente | Mostra número | Recusa |
| Hoje, REFAZENDO | `?` | Recusa |

Essas diferenças decorrem de `STATUS != REFAZENDO` mais número obrigatório no placar, contra `STATUS=OK` mais existência mínima do banco em `conferir`. (bin/placar_code.sh:34,35,36,37; bin/sombra.sh:439,455,457,459)

**PROCEDENTE — derivação própria no novo leitor.** Completude medida e ensaio verde são perguntas distintas; não devem ser anunciadas como predicados iguais. Cura sugerida: uma autoridade com parser comum e predicados explicitamente distintos. A cobertura já possui terceiro julgamento: hoje, OK ou REFEITA, depois tipo/base/janela. Até o parsing diverge: última ocorrência no placar, concatenação em `conferir`, última ocorrência com normalização na cobertura. (CLAUDE.md:20,21,22; bin/placar_code.sh:34,37; bin/sombra.sh:439; app/core/sombra_cobertura.py:18,21,35,37,42,45)

Leitores operacionais encontrados:

- **Diretos:** incremental, bloco e `conferir`; placar; `core/sombra_cobertura.py`. (bin/sombra.sh:210,211,224,356,427,439,443,444; bin/placar_code.sh:34,35,36; app/core/sombra_cobertura.py:35,37,39,42,45)
- **Delegados, sem predicado próprio do stamp:** deploy, indicador verde do placar, esteira em `trava.sh`, simulação de folha e diff de janela; os dois últimos também exigem cobertura. (bin/deploy.sh:162; bin/placar_code.sh:169; bin/trava.sh:52; bin/simular_folha.sh:68,70; bin/diff_janela_he_total.sh:19,22)

4. **Porta/defaults — PROCEDENTE a cobertura desigual**

`REVOKE ... ON SCHEMAS` cobre defaults de `$PGU` para `leitor`. A conferência conta defaults de **qualquer concedente**, destinados ao leitor **ou PUBLIC**, também para tabelas e sequências. A porta não revoga todo esse universo. (bin/sonda_provisionar.sh:125,126,128,129,132; bin/sonda_leitor_comum.sh:119,120,121,122)

**A porta deixa esses privilégios, mas não deixa passar como pronto:** a leitura recusa rc 5; a porta recusa rc 1 antes de promover `.env_leitor`. Há bloqueio sem autocura, sem falso pronto nesse caminho. Quem cura é o operador autorizado, revogando o default no concedente/destinatário/escopo de origem; o próprio diagnóstico orienta REVOKE manual. Cura efetiva em produção: **[nao verificado]**. (bin/sonda_leitor_comum.sh:132,145; bin/sonda_leitura.sh:187,135; bin/sonda_provisionar.sh:148,150,151,171)

5. **`_limpa` — PROCEDENTE colisão; NÃO PROCEDE nome vazio no fluxo atual**

O trap precede a criação. Se `run --name "$PGT"` falhar por colisão, o EXIT executa `rm -f "$PGT"` sobre o container que já ocupava o nome. A guarda só exclui três nomes conhecidos; não prova autoria. Cura sugerida: remover exclusivamente o ID obtido da criação bem-sucedida, sem remover por nome após criação recusada; acrescentar caso de colisão ao selo. (bin/tests/test_sonda_sem_grant_pg.sh:35,36,41,50,93,95)

PGT não fica vazio nessa atribuição: contém prefixo literal e PID; a substituição usada pela fixture ocorre em subshell. (bin/tests/test_sonda_sem_grant_pg.sh:35,72,73,74)

**PROCEDENTE — ausência não comprovada:** o rc de `rm` é ignorado e qualquer falha de `inspect` equivale a “sumiu”. Cura sugerida: distinguir inexistência de falha da consulta e testar esta última. Falha real do Docker: **[nao verificado]**. (bin/tests/test_sonda_sem_grant_pg.sh:41,42,85,86,87,88)

6. **Classes adicionais**

- **Segundo escritor operacional do stamp — NÃO PROCEDE no código localizado:** publicação e invalidação passam por `carimbar`; os escritores adicionais dos selos usam fixtures temporárias. Escritor externo/manual: **[nao verificado]**. (bin/sombra.sh:101,110,225,328,430; bin/tests/test_sonda_sem_grant.sh:363,394,432,447)
- **Relógio — FILA para o preexistente; PROCEDENTE no novo leitor independente:** HOJE nasce antes da trava, a base relê a data e a cobertura usa `date.today()`. O placar acrescenta outra captura antes de chamar posteriormente `conferir`. Cura sugerida: referência temporal obtida na borda e passada aos julgamentos. Travessia efetiva da meia-noite: **[nao verificado]**. (anexo triagem_preverif_v3.txt:10; bin/sombra.sh:83,328,464; app/core/sombra_cobertura.py:34; bin/placar_code.sh:39,169)
- **Selo sem mordida — NÃO PROCEDE como acusação geral:** há mortes pertinentes registradas; preservação no caso `mv`, identidade do container e os caminhos incremental/bloco continuam fora dessa prova. (anexo sonda_v4_mut.log:1,6,11,16,21,26,31; bin/tests/test_sonda_sem_grant.sh:396,411; bin/tests/test_sonda_sem_grant_pg.sh:80,93)
- **Campos com dois sentidos — PROCEDENTE na interpretação de prontidão:** STATUS distingue resultado do refazer e resultado do bloco; incremental preserva o status anterior mesmo declarando bloco fora da janela. DIA sozinho não significa pronto, e INCOMPLETA é a medição do refazer. Cura sugerida: preservar essas distinções na autoridade comum, sem equiparar “medição válida” a “ensaio verde”. (bin/sombra.sh:227,319,329,331,432; bin/placar_code.sh:37; app/core/sombra_cobertura.py:37)

7. **Quem escreve / quem lê**

Todos os campos do stamp abaixo são materializados por `carimbar`; ele lê o anterior, produz temporários e promove o resultado. (bin/sombra.sh:94,95,98,99,101)

| Campo de `.sombra_stamp` | Quem escreve | Quem lê |
|---|---|---|
| DIA | Invalidação/completo: sombra.sh:110,328 | sombra.sh:210,356,439; placar_code.sh:34; sombra_cobertura.py:35 |
| STATUS | Invalidação/incremental/completo/bloco: sombra.sh:110,227,331,432 | sombra.sh:224,439; placar_code.sh:35; sombra_cobertura.py:37 |
| INCOMPLETA | Invalidação/completo: sombra.sh:110,329 | placar_code.sh:36 |
| TIPO / BASE_DIA | Incremental/completo: sombra.sh:225,328 | sombra.sh:211,441,443; sombra_cobertura.py:39,45 |
| JANELA_INI/FIM | Incremental/completo: sombra.sh:225,329 | sombra.sh:443; sombra_cobertura.py:42 |
| DIVERGE | Completo: sombra.sh:329 | sombra.sh:427,444 |
| INCREMENTAL_EM / ERROS | Incremental/bloco: sombra.sh:226,431 | sombra.sh:443,444 |
| DUMP, REFEITA, BLOCO, INCREMENTAL_S, BLOCO_NA_JANELA, COMANDOS, ALARMES, FORA_DO_ENSAIO | sombra.sh:226,227,328,330,430,431 | Leitor operacional desses campos: **[nao verificado]** |

| Outros estados/arquivos alcançados pelo delta | Quem escreve / quem lê |
|---|---|
| Defaults de schema; `super` | Porta revoga default e escreve NOSUPERUSER; consulta produz `default_escrita`/`super`; parser, porta e leitura consomem. Fixtures aplicam os estragos. (bin/sonda_provisionar.sh:105,132,148; bin/sonda_leitor_comum.sh:68,119,132,145; bin/sonda_leitura.sh:187; bin/tests/test_sonda_sem_grant_pg.sh:207,208) |
| `.env_leitor`, senha, temporário | Porta lê senha e publica; leitura valida/lê e entrega ao container. (bin/sonda_provisionar.sh:77,168,169,171; bin/sonda_leitura.sh:173,174,178,234) |
| Fontes recortadas pelos selos | Selos leem `sombra.sh` e `placar_code.sh`; escritor dessas fontes não é identificado pelo dado. (bin/tests/test_sonda_sem_grant.sh:358,359,380,422) **[nao verificado]** |
| Fixtures 9/9c/10: stamps, dumps vazios, docker falso, argv, saída e temporários | Selo monta; docker falso registra argv; asserções leem. (bin/tests/test_sonda_sem_grant.sh:363,383,384,385,387,394,403,408,411,413,432,447,450) |
| Fixtures `_limpa`: `lf/docker`, `lf/argv`, `lf/out`, `lf/t`; container PGT | Selo cria entradas; falso grava argv; asserções leem; `_limpa` remove pasta/container e consulta existência. (bin/tests/test_sonda_sem_grant_pg.sh:41,42,46,61,63,71,74,76,80,93) |
| Imagem/recursos do selo PG | Selo lê `recursos.sh` e `db_teste.sh`; autoria desses valores: **[nao verificado]**. (bin/tests/test_sonda_sem_grant_pg.sh:29,31) |
| Trilhas de provisionamento/leitura e saída da porta na sombra | Traps escrevem as trilhas; sombra redireciona e lê a falha da porta. Outros leitores operacionais: **[nao verificado]**. (bin/sonda_provisionar.sh:60,61; bin/sonda_leitura.sh:121,122; bin/sombra.sh:252,253) |

## 09/11 10:51 · sessao f5bd0a6923b7 · - · - · origem=Ronald (tmux)

- **PROCEDENTE — T4 residual em bloco:** partindo de carimbo hoje/OK, uma nova execução pode calcular `FALHOU`, falhar ao gravar e terminar com o `echo` bem-sucedido, preservando o OK anterior. `conferir` aceita esse OK se ainda houver migrations. Cura: invalidar antes da execução e exigir sucesso da publicação; testar carimbo inicialmente OK, erro no bloco e falha de gravação. (`bin/sombra.sh:356,407,417,428,429,430,433,455,459`)

- **PROCEDENTE — incremental conserva cobertura mentirosa:** altera a janela e confirma a transação antes de atualizar o carimbo. Falha da prova ou máscara deixa o carimbo antigo; falha de `carimbar` também é ignorada pelo `echo` seguinte. Um carimbo anteriormente `completa/OK` continua descrevendo cobertura completa. Cura: invalidar antes da alteração, preservar explicitamente os insumos necessários e exigir a publicação final; selo cobrando invalidação e recusa após cada falha. (`bin/sombra.sh:210,214,216,218,224,225,228`; `bin/sombra_incremental.sh:61,66,68,77,81,85`; `app/core/sombra_cobertura.py:39,40,41`)

- **NÃO PROCEDE — fim do refazer como preservação de verde antigo:** invalidação é exigida antes do drop; a publicação final é o último comando da função, portanto seu rc se propaga. Falha mantém dia vazio/REFAZENDO. `--dump-agora` converge para a mesma invalidação antes do drop; `conferir` não grava o carimbo. (`bin/sombra.sh:110,259,264,265,275,276,328,331,332,436,459`)

- **NÃO PROCEDE — predicados iguais:** o placar exige dia hoje, status diferente de REFAZENDO e número presente; conferir exige dia hoje, status exatamente OK e migrations positivas, ignorando o número. Status vazio com número: placar mostra número, conferir recusa. Hoje/OK sem INCOMPLETA: placar mostra `?`, conferir pode aprovar. Dia vazio ou ontem são rejeitados pelos dois; REFEITA/DIVERGE/FALHOU são admitidos como medida pelo placar, mas não como ensaio verde por conferir. São perguntas diferentes; centralizar parser e definições explícitas de validade/prontidão evita apresentar duas regras como idênticas. (`bin/placar_code.sh:34,35,36,37`; `bin/sombra.sh:439,455,456,459`)

- **PROCEDENTE — terceiro predicado e parsing próprio:** cobertura aceita hoje com OK **ou REFEITA**, depois aplica tipo/base/janela, sem consultar existência do banco. Seu parser usa última ocorrência e remove espaços/aspas; placar usa última ocorrência literal, enquanto conferir concatena todas as ocorrências. Cura: parser comum e predicados nomeados para perguntas distintas. (`app/core/sombra_cobertura.py:18,19,20,21,34,35,37,39,42,45,47`; `bin/placar_code.sh:34,35,36`; `bin/sombra.sh:439`)

- **Leitores diretos operacionais:** incremental lê DIA/BASE/TIPO/STATUS; bloco lê DIA/DIVERGE; conferir lê DIA/STATUS/TIPO/JANELAS/BASE/INCREMENTAL_EM/DIVERGE/ERROS; placar lê DIA/STATUS/INCOMPLETA; cobertura lê DIA/STATUS/TIPO/JANELAS/BASE. (`bin/sombra.sh:210,211,224,356,427,439,441,443,444`; `bin/placar_code.sh:34,35,36`; `app/core/sombra_cobertura.py:35,37,39,42,45`)

- **Leitores indiretos, sem regra própria do carimbo:** deploy; verde do placar; portão da esteira em `trava.sh`; simulação de folha e diff de janela. Estes dois últimos também exigem cobertura. (`bin/deploy.sh:162`; `bin/placar_code.sh:169`; `bin/trava.sh:40,43,52`; `bin/simular_folha.sh:68,70`; `bin/diff_janela_he_total.sh:19,22`)

- **FILA — relógio:** HOJE é capturado antes da trava; incremental e publicação completa consultam novamente a data; cobertura calcula hoje por conta própria. O placar adiciona captura anterior à chamada posterior de conferir. Cura: data de referência única na borda adequada, passada aos leitores; travessia real da meia-noite **[nao verificado]**. (`bin/sombra.sh:83,211,328,464,466,468,470,477`; `app/core/sombra_cobertura.py:34`; `bin/placar_code.sh:39,169`)

- **NÃO PROCEDE — segundo escritor operacional localizado:** os quatro caminhos escrevem por `carimbar`; invalidação escreve DIA/STATUS/INCOMPLETA, completo escreve os três, incremental/bloco escrevem STATUS. Escritor externo/manual **[nao verificado]**. (`bin/sombra.sh:92,101,110,225,227,328,329,331,430,432`)

## 09/11 10:51 · sessao 86ce7ad8eff9 · - · - · origem=Ronald (tmux)

- **2a/2b: fechados no código e selos mordem a causa indicada.** Estragos `SUPERUSER` e default de schema grantable entram no mesmo ciclo: chave positiva, rc 5, recusa nomeada, ausência de execução e cura pela porta. O log fornecido registra falhas correspondentes; alterações exatas dos mutantes e execução independente **[nao verificado]**. (`bin/tests/test_sonda_sem_grant_pg.sh:181`, `:183`, `:185`, `:186`, `:187`, `:189`, `:207`, `:208`)

- **4: guarda fechada; mordida pertinente.** `carimbar` propaga falhas e `refazer` aborta antes do drop; 9c cobra ausência de `dropdb`, rc, mensagem e temporários. **Ressalva do selo:** caso `mv` aponta `STAMP` para caminho ausente, mas compara o outro arquivo `RF/stamp`; essa asserção não prova preservação de um carimbo existente após falha final de `mv`. (`bin/sombra.sh:94`, `:98`, `:99`, `:101`, `:275`; `bin/tests/test_sonda_sem_grant.sh:396`, `:399`, `:408`, `:409`, `:410`, `:411`, `:413`)

- **6: fechado no pedido literal; mordida pertinente.** Os casos dia vazio, `REFAZENDO`, ontem e número vazio cobram `?`; o mutante registrado cai nessa comparação. Status vazio não foi incluído no selo. (`bin/placar_code.sh:37`; `bin/tests/test_sonda_sem_grant.sh:434`, `:436`, `:437`, `:438`, `:439`, `:440`, `:441`, `:442`)

- **10: falha silenciosa original fechada, identidade ainda PROCEDENTE.** Cleanup preserva rc e acusa container ainda encontrado; o selo mata o comportamento antigo por rc e argv. Porém o trap antecede a criação: se `run --name "$PGT"` falhar por colisão, `EXIT` chama `rm -f "$PGT"` sobre o container alheio. Cura: registrar o ID da criação bem-sucedida, remover somente esse ID e não remover após criação recusada. O selo não cobre colisão. (`bin/tests/test_sonda_sem_grant_pg.sh:39`, `:41`, `:42`, `:44`, `:48`, `:50`, `:76`, `:80`, `:82`, `:93`, `:95`)

- **Nome vazio: NÃO PROCEDE no fluxo atual.** A atribuição sempre contém prefixo literal e PID; a alteração de `PGT` no teste fica no subshell. **“Nome exato criado pelo selo”: não provado**, pois exatidão do nome não prova autoria. (`bin/tests/test_sonda_sem_grant_pg.sh:35`, `:72`, `:73`, `:74`)

- **Limite da verificação de limpeza:** `rm` falha sem exame direto e qualquer erro de `inspect` equivale a “ausente”; o fake não distingue ausência de falha da consulta. Falha efetiva do Docker **[nao verificado]**. (`bin/tests/test_sonda_sem_grant_pg.sh:41`, `:42`, `:85`, `:86`, `:87`, `:88`)

## 09/11 10:50 · sessao 72a0c7209703 · - · - · origem=Ronald (tmux)

- **PROCEDENTE quanto à diferença de cobertura; cura automática ampliada pode ir à FILA.** `REVOKE ... ON SCHEMAS` cura o caso `$PGU → leitor`, mas a conferência conta defaults de **qualquer** concedente e também de `PUBLIC`; a porta só revoga defaults `FOR ROLE $PGU ... FROM $ROLE`. Isso vale para schemas, tabelas e sequências. (bin/sonda_provisionar.sh:125-132; bin/sonda_leitor_comum.sh:119-122)
- **Não há falso pronto nesse caso:** `default_escrita != 0` reprova o parser; leitura sai rc 5 e porta sai rc 1 antes de promover `.env_leitor`. Há bloqueio persistente sem autocura pela porta. (bin/sonda_leitor_comum.sh:132,145; bin/sonda_leitura.sh:187,129-135; bin/sonda_provisionar.sh:147-151,171)
- **Quem cura:** o código manda `REASSIGN/REVOKE a mao`; para defaults, a correção sugerida é um operador autorizado revogar no concedente/objeto/escopo original, para `leitor` ou `PUBLIC`. A realização disso em produção é **[nao verificado]**. (bin/sonda_provisionar.sh:150; bin/sonda_leitor_comum.sh:119-122)
- **Item 2b fechado no domínio testado:** novo estrago é `saas_user → leitor`, `USAGE ON SCHEMAS WITH GRANT OPTION`; a estrutura exige chave positiva, rc 5, chave na recusa, sonda não executada e recuperação pela porta. Outros concedentes e defaults para `PUBLIC` não têm casos nesse conjunto. (bin/tests/test_sonda_sem_grant_pg.sh:179-191,192-209)
- **Item 2a fechado estruturalmente:** estrago SUPERUSER chega à consulta real e às mesmas quatro cobranças; a porta escreve NOSUPERUSER. (bin/tests/test_sonda_sem_grant_pg.sh:156-159,182-189,207; bin/sonda_leitor_comum.sh:68; bin/sonda_provisionar.sh:105)

Mapa dos insumos SQL alterados:

| Estado/campo | Quem escreve | Quem lê |
|---|---|---|
| Default schema de `$PGU` para `leitor` | Porta: REVOKE. (bin/sonda_provisionar.sh:132,144) | Conferência `defaclobjtype='n'`, ACL/grantee/privilégio/grantable. (bin/sonda_leitor_comum.sh:119-122) |
| `rolsuper` | Porta NOSUPERUSER; fixture SUPERUSER. (bin/sonda_provisionar.sh:105; bin/tests/test_sonda_sem_grant_pg.sh:207) | Consulta `super`; parser e consumidores. (bin/sonda_leitor_comum.sh:68,132,145; bin/sonda_leitura.sh:187; bin/sonda_provisionar.sh:148) |
| `default_escrita` | Derivado da consulta, sem escritor materializado de catálogo com esse nome. (bin/sonda_leitor_comum.sh:119-122) | Parser; porta antes/depois; leitura; selo PG. (bin/sonda_leitor_comum.sh:132,145; bin/sonda_provisionar.sh:72,147-148; bin/sonda_leitura.sh:184-187; bin/tests/test_sonda_sem_grant_pg.sh:182-186) |
| `PGU/ROLE/SCHEMAS/PG/BANCO` | Constantes e `_destino`. (bin/sonda_leitor_comum.sh:8-15,22-25) | Geração SQL e execução psql da porta/conferência. (bin/sonda_provisionar.sh:51,125-132; bin/sonda_leitor_comum.sh:64,96,120) |
| `.env_leitor` / senha | Porta, temporário e promoção. (bin/sonda_provisionar.sh:168-171) | Porta lê senha; leitura valida/lê e passa ao container. (bin/sonda_provisionar.sh:77; bin/sonda_leitura.sh:173-178,234) |

## 09/11 10:39 · sessao c1c80cb6a02e · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · verificador · origem=Code

**Há achados PROCEDENTES; GREEN, mortes M1–M38 e resultado em produção permanecem [nao verificado].**

1. **ATO ÚNICO — NAO PROCEDE para par identificado; garantia delimitada.** Validação, varredura e O330 passam pelo envelope comum. As duas aplicações, julgamento posterior, carimbos e reconciliação ficam no mesmo savepoint. Sem par identificado, continua possível aplicar uma pergunta individualmente, inclusive com irmã já aplicada. `app/chamados/services/validacao.py:474`; `app/chamados/services/materializacao.py:1596`; `app/chamados/services/passivo_validado.py:532`; `app/chamados/services/materializacao.py:1344`; `app/chamados/services/materializacao.py:1361`; `app/chamados/services/materializacao.py:1431`.

   **PROCEDENTE — paridade sobre universo errado.** O julgamento posterior usa vizinhas sem excluir batidas retratadas. Uma S retratada às 01:30 pode reprovar S01/E02, embora o conjunto vivo alterne. **Cura:** usar o universo canônico de apuração e testar esse contracaso. `app/chamados/services/materializacao.py:178`; `app/chamados/services/materializacao.py:184`; `app/chamados/services/materializacao.py:1367`; `app/ponto/turnos.py:2866`.

   O censo AST com apelidos encontrou os plantadores de Pergunta nos ramos hora/data-hora. Regularização externa e veredito do dia têm unidades próprias e permitem resultados parciais: **FILA**, auditoria separada. `app/chamados/services/materializacao.py:1017`; `app/chamados/services/materializacao.py:1098`; `app/chamados/services/regularizacao_ext.py:166`; `app/ponto/services/veredito_celula.py:320`.

2. **ROLLBACK — limpeza funciona, mas descarimbo vazio tem efeitos.** Após restaurar o banco, o código decide pelo lastro, descarimba quando reprovado, grava recusa nas duas e chama a devolução. `app/chamados/services/materializacao.py:1386`.

   | Campo | Resultado após o tratamento da falha |
   |---|---|
   | `materializada_em/por`, `via_resolucao` | Lastro reprovado: `None/None/''`; lastro aprovado: conserva valores restaurados. `app/chamados/services/materializacao.py:1391`; `app/chamados/models.py:3211`. |
   | `validada_em/por` | Limpos quando é hora declarada, sem materialização, com recusa; não há limpeza incondicional para qualquer tipo. `app/chamados/services/validacao.py:190`; `app/chamados/services/validacao.py:198`. |
   | `recusa_motivo` | Recebe a recusa conjunta, truncada em 240 caracteres. `app/chamados/services/materializacao.py:1393`; `app/chamados/services/materializacao.py:1181`. |

   **PROCEDENTE — `save` sem mudança não é inócuo.** Mesmo com os três campos vazios, `descarimbar` salva com `update_fields=['via_resolucao','materializada_em','materializada_por']`; isso agenda o beco e insere EventoCelula quando há célula, sem deduplicação nessa emissão. **Cura:** tornar o descarimbo vazio um no-op na porta dona, comparando os três campos. `app/chamados/models.py:3211`; `app/chamados/signals.py:225`; `app/chamados/signals.py:242`; `app/chamados/models.py:3248`; `app/ponto/portas/celula.py:891`.

   Esse `save` não chama diretamente LogAuditoria nem re-lavra o veredito da pergunta. A devolução acrescenta E1 quando remove validação e audita quando sua foto muda; a nota de recusa depende de mudança do motivo. O censo estático conta sítios, não quantas vezes esse `save` executa. `app/chamados/models.py:3206`; `app/chamados/services/validacao.py:208`; `app/chamados/services/validacao.py:224`; `app/chamados/services/materializacao.py:1195`; `app/core/censo_escritas.py:451`.

   **“2x = mesmo estado e mesma trilha” não está demonstrado.** Repetir diretamente o descarimbo pode acrescentar eventos; repetir o envelope depois da devolução encontra ambas sem validação e não reaplica o par. `app/chamados/models.py:3215`; `app/ponto/portas/celula.py:891`; `app/chamados/services/materializacao.py:1415`; `app/chamados/services/materializacao.py:1433`.

   **NAO PROCEDE quanto a perda comprovada de carimbo legítimo fora do fato:** essas linhas são excluídas da identificação e a própria porta recusa descarimbá-las; vias de identidade carimbadas são absolvidas pelo lastro. Outro caso legítimo perdido nesta população: **[nao verificado]**. `app/chamados/services/materializacao.py:72`; `app/chamados/models.py:3209`; `app/chamados/juizes.py:670`.

3. **TRAVAS — NAO PROCEDE a inversão entre os dois validadores.** Ambos começam pelo mesmo Colaborador, depois Disputas, Chamados e Perguntas, ordenados por PK; a pergunta é relida após travar. O330 antecipa o universo da competência. Prova concorrente com duas conexões: **FILA**, não executada. `app/chamados/services/materializacao.py:125`; `app/chamados/models.py:2677`; `app/chamados/models.py:2686`; `app/chamados/models.py:2699`; `app/chamados/models.py:2703`; `app/chamados/services/validacao.py:435`; `app/chamados/services/passivo_validado.py:435`.

4. **ESTADO/FECHAMENTO — dois achados PROCEDENTES.**

   - **C7 tem um segundo julgamento dentro de `fechar`.** `pode_fechar` retém espera, mas `fechar` verifica respostas/C6 sem consultá-lo. Com metade aceita e irmã retratada, a porta pode fechar apesar de C7 negar. **Cura:** consultar C7 sob trava; testar a chamada real a `fechar`. P2 só testa `pode_fechar`. `app/chamados/models.py:1863`; `app/chamados/models.py:1897`; `app/chamados/models.py:1907`; `app/chamados/models.py:1943`; `app/chamados/tests/test_bug_par_intervalo.py:427`.
   - **Espera confunde resposta com aceite.** Com ambas respondidas e nenhuma validada, o predicado já retorna espera. O explicador pula “aguarda validação”; “Aplicar declaração” retorna antes de validar a própria pergunta. **Cura:** exigir validação da própria metade para esse estado. `app/chamados/services/materializacao.py:109`; `app/chamados/services/materializacao.py:117`; `app/chamados/services/explicador.py:74`; `app/chamados/services/declaracao_texto.py:678`; `app/chamados/services/declaracao_texto.py:709`.

   | Leitor | O que efetivamente lê/diz |
   |---|---|
   | C6, painel e badge | Validação mata a pergunta; o crédito continua por perguntas vivas/verbos. `app/chamados/juizes.py:1153`; `app/chamados/services/raiox.py:72`; `app/chamados/services/verbos.py:34`. |
   | Fio | Aberta carrega perguntas; fechada só admite C6 vivas, ocultando a espera se o fechamento escapar. `app/chamados/services/fio.py:221`; `app/chamados/services/fio.py:238`. |
   | Explicador, declaração, API | Leem/publicam o predicado de espera. `app/chamados/services/explicador.py:74`; `app/chamados/services/declaracao_texto.py:484`; `app/api/views.py:1126`. |
   | Vigia de carimbo/O330 | Separam espera da dívida e do alvo reparável. `app/chamados/services/vigia_carimbo.py:109`; `app/chamados/services/passivo_validado.py:324`. |
   | `vigia_fio`, `o_que_segura` | Continuam por C6 e autoridades de alcance/dono; não tratam espera como fio vivo. `app/chamados/services/vigia_fio.py:164`; `app/folha/fora_da_folha.py:221`; `app/chamados/juizes.py:1322`. |
   | Crons | Beco consulta C7; alerta diário continua avisando disputas abertas. Não há reabertura automática por espera nesses caminhos. `app/chamados/services/becos.py:33`; `app/ponto/management/commands/alertar_disputas_pendentes.py:33`; `app/ponto/management/commands/alertar_disputas_pendentes.py:59`. |

5. **IDENTIDADE — NAO PROCEDE a derivação paralela na tag.** O identificador consulta pausas efetivas/DNA e se abstém por multiplicidade; a tag chama o mesmo predicado. `par_ambiguo` preserva o despacho avulso, não constitui prova de equivalência integral de estado/trilha com HEAD. `app/chamados/services/materializacao.py:77`; `app/chamados/services/materializacao.py:88`; `app/escala/models.py:1189`; `app/chamados/templatetags/chamados_tags.py:18`; `app/chamados/services/materializacao.py:1431`.

6. **TEXTO/TELA — catálogo e proximidade corrigidos; percurso incompleto.** “Colada” exige testemunha viva nos ±60s inclusivos compartilhados com a porta; espera tem entrada própria. **NAO PROCEDE** a duplicação desses predicados. `app/chamados/services/materializacao.py:1357`; `app/ponto/registro_batida.py:94`; `app/ponto/registro_batida.py:115`; `app/chamados/catalogo/recusa.py:25`.

   **PROCEDENTE — aviso some do detalhe após recarga.** Os dois includes existem, mas o contexto do detalhe exclui a metade validada por C6. P5 injeta a pergunta diretamente. **Cura:** incluir espera no filtro de apresentação e testar GET real. `app/templates/chamados/partials/_pergunta_card.html:29`; `app/templates/chamados/partials/modal_fio.html:160`; `app/chamados/services/acoes_chamado.py:348`; `app/chamados/tests/test_bug_par_intervalo.py:486`.

7. **SELOS/SEMÂNTICA.** M7 e M35 agora têm asserções discriminantes estáticas; mortes executadas continuam **[nao verificado]**. **PROCEDENTE:** P6/M38 não provam a alteração específica v3: começam com carimbo preenchido, portanto passariam com a guarda anterior que só descarimbava nesse caso. **Cura:** mutante restaurando exatamente aquela guarda, cenário já vazio e contagem de saves/eventos/trilha em duas chamadas. `app/chamados/tests/test_bug_par_intervalo.py:280`; `app/chamados/tests/test_bug_par_intervalo.py:498`; `app/chamados/tests/test_bug_par_intervalo.py:508`; `app/chamados/tests/mutantes_bug_par_intervalo.py:131`.

   **FILA — relógio e dia civil/turno.** Há `now()` por metade no carimbo/aprovação; passar um instante do ato eliminaria essa dispersão. As derivações continuam pelos helpers existentes; contraprova de fuso ou competência neste dado: **[nao verificado]**. `app/chamados/services/materializacao.py:1253`; `app/chamados/services/materializacao.py:1373`; `app/chamados/services/materializacao.py:1023`; `app/chamados/services/materializacao.py:1351`.

8. **ESCRITORES/LEITORES adicionais dos campos centrais**, além dos consumidores acima:

   | Campo | Escritores / leitores fora do diff |
   |---|---|
   | Resposta e `respondida_em` | Resposta humana/automática/trilha; carimbo preenche ausente. Leitura pela derivação do slot. `app/chamados/services/disputa_emissao.py:1464`; `app/chamados/services/disputa_emissao.py:1649`; `app/chamados/services/disputa_emissao.py:1671`; `app/chamados/models.py:3134`; `app/chamados/juizes.py:339`. |
   | `validada_em/por` | Validação, devolução, nova resposta, preenchimento pelo carimbo e reabertura. `app/chamados/services/validacao.py:134`; `app/chamados/services/validacao.py:202`; `app/chamados/services/disputa_emissao.py:1477`; `app/chamados/models.py:3137`; `app/chamados/models.py:1565`. |
   | Materialização/via | Portas de carimbo/descarimbo, lote, encerramento por fato e relavra; leitores C6/lastro/lavra persistida. `app/chamados/models.py:3100`; `app/chamados/models.py:3267`; `app/chamados/models.py:3278`; `app/chamados/services/relavra_via.py:125`; `app/chamados/juizes.py:645`; `app/chamados/veredito_lavrado.py:187`. |
   | Recusa | Identidade, pendência, recusa canônica/humana e escape; leitores catálogo e devolução. `app/chamados/models.py:3191`; `app/chamados/services/materializacao.py:940`; `app/chamados/services/validacao.py:605`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/catalogo/recusa.py:92`; `app/chamados/services/validacao.py:195`. |
   | `contexto_json` | Portas anotar/renovar/retirar/fundir; marco faltante lê seu conteúdo. `app/chamados/services/contexto.py:76`; `app/chamados/services/contexto.py:102`; `app/chamados/services/contexto.py:125`; `app/chamados/services/contexto.py:161`; `app/chamados/juizes.py:508`. |
   | Batida: instante/tipo/origem/vínculo | `registrar_batida` e `anotar`; leitores tolerância, vizinhança e lastro. `app/ponto/registro_batida.py:104`; `app/chamados/services/materializacao.py:1038`; `app/ponto/registro_batida.py:94`; `app/chamados/services/materializacao.py:169`; `app/chamados/juizes.py:732`. |

   Exaustividade de **todos** os leitores transitivos de cada campo/função na árvore: **[nao verificado]**.

## 09/11 10:37 · sessao 54312d58dc6d · - · - · origem=Ronald (tmux)

- **PROCEDENTE — teste da alteração específica v3 insuficiente.** P6 começa com `materializada_em` preenchido; restaurar a guarda anterior `materializada_em is not None and not lastro` continuaria satisfazendo esse teste. M38 inverte o lastro, sem testar o descarimbo já vazio. Acrescentar cenário vazio, captura dos efeitos de `save` e repetição 2x com contagem de trilha. `app/chamados/tests/test_bug_par_intervalo.py:508`; `app/chamados/tests/test_bug_par_intervalo.py:669`; `app/chamados/tests/test_bug_par_intervalo.py:529`; `app/chamados/tests/mutantes_bug_par_intervalo.py:131`.

- **PROCEDENTE — universo da paridade inclui retratadas.** O novo ato consulta `_vizinha_mesmo_tipo`, que busca `Batida` sem excluir retratadas, e recusa incondicionalmente a testemunha encontrada. Contraexemplo estático: sequência viva E18:32/S01:00/E02:00/S06:30 mais S retratada01:30 faz a saída encontrar S01:30 e recusar o conjunto. Cura no helper, usando `batidas_apuraveis`, com RED desse cenário. Ocorrência em produção [nao verificado]. `app/chamados/services/materializacao.py:178`; `app/chamados/services/materializacao.py:184`; `app/chamados/services/materializacao.py:1367`; `app/ponto/turnos.py:2866`.

- **NAO PROCEDE — M7/M35 continuam sem discriminação estática.** M7 agora encontra DNA com uma pausa e quatro perguntas, exigindo `par_ambiguo`; M35 precisa produzir o aviso HTML exato uma vez no modal, e o runner troca também o filtro registrado. Mortes efetivamente executadas [nao verificado]. `app/chamados/tests/test_bug_par_intervalo.py:280`; `app/chamados/tests/test_bug_par_intervalo.py:288`; `app/chamados/tests/test_bug_par_intervalo.py:493`; `app/chamados/tests/test_bug_par_intervalo.py:498`; `app/chamados/tests/mutantes_bug_par_intervalo.py:230`.

- **NAO PROCEDE — nova cópia literal da proximidade/identidade.** Porta e ato leem `batida_mesmo_tipo_na_tolerancia`; identidade consulta `escala_vigente → vinculo_do_dia`, `pausas_do_dia` e mapa canônico existente. A guarda pós-par reutiliza o helper antigo; isso não elimina o defeito de universo acima. `app/ponto/registro_batida.py:115`; `app/chamados/services/materializacao.py:1357`; `app/chamados/services/materializacao.py:87`; `app/escala/servico_jornada.py:57`; `app/escala/models.py:1197`; `app/chamados/juizes.py:882`.

- **FILA — relógio por metade.** `_carimbar_aplicacao` lê `now()` internamente e é chamado para cada metade; aprovação também lê por escrita. A expressão foi extraída do escritor anterior segundo o diff fornecido; contraprova temporal neste dado [nao verificado]. Passar um instante do ato seria a cura estrutural. `app/chamados/services/materializacao.py:1253`; `app/chamados/services/materializacao.py:1373`; `app/chamados/services/materializacao.py:1023`.

Inventário de escritores fora e dentro do diff:

| Campo | Escritores |
|---|---|
| `materializada_em/por` | `_escrever_carimbo`, chamado pelas portas; descarimbo em lote. `app/chamados/models.py:3100`; `app/chamados/models.py:3267` |
| `via_resolucao` | Mesmas portas, mais encerramento por fato e relavra. `app/chamados/models.py:3278`; `app/chamados/services/relavra_via.py:125` |
| `validada_em/por` | Validar, devolver, responder novamente, preencher ausente no carimbo. `app/chamados/services/validacao.py:134`; `app/chamados/services/validacao.py:202`; `app/chamados/services/disputa_emissao.py:1477`; `app/chamados/models.py:3137` |
| `resposta_colab/respondida_em` | Resposta humana, automática, recuperação pela trilha; carimbo preenche instante ausente. `app/chamados/services/disputa_emissao.py:1464`; `app/chamados/services/disputa_emissao.py:1649`; `app/chamados/services/disputa_emissao.py:1671`; `app/chamados/models.py:3134` |
| `recusa_motivo` | Identidade, pendência do plantio, recusa canônica, limpeza do par, recusa humana, escape da resposta. `app/chamados/models.py:3191`; `app/chamados/services/materializacao.py:940`; `app/chamados/services/materializacao.py:1193`; `app/chamados/services/materializacao.py:1378`; `app/chamados/services/validacao.py:605`; `app/chamados/services/disputa_emissao.py:1494` |
| `contexto_json` | Portas anotar/renovar/retirar/fundir. `app/chamados/services/contexto.py:76`; `app/chamados/services/contexto.py:102`; `app/chamados/services/contexto.py:125`; `app/chamados/services/contexto.py:161` |

A existência das asserções não certifica GREEN, RED anterior ou morte dos mutantes: [nao verificado].

## 09/11 10:37 · sessao 30e4a759213b · - · - · origem=Ronald (tmux)

**PROCEDENTE — C7 duplicado na porta de fechamento.** `pode_fechar()` agora retém espera, mas `fechar()` continua julgando apenas respostas sem veredito e perguntas vivas; não consulta C7. Com primeira metade validada e irmã retratada, C7 recusa e `fechar(user)` pode gravar fechamento. Cura: porta consultar C7 sob trava; RED chamar `fechar`, além de `pode_fechar`. `app/chamados/models.py:1863`, `app/chamados/models.py:1897`, `app/chamados/models.py:1907`, `app/chamados/models.py:1943`. O novo P2 só verifica `pode_fechar`. `app/chamados/tests/test_bug_par_intervalo.py:427`.

**PROCEDENTE — espera confunde resposta com aceite.** O predicado não exige validação da própria pergunta quando identifica par. Ambas respondidas, nenhuma validada: ambas viram `aguardando_par`, e o explicador deixa de oferecer validar a própria resposta. Cura: exigir `p.validada_em` para espera de metade aceita. `app/chamados/services/materializacao.py:109`, `app/chamados/services/materializacao.py:117`, `app/chamados/services/explicador.py:74`, `app/chamados/services/explicador.py:85`.

Isso também bloqueia portas: declaração do colaborador retorna antes de permitir alterar resposta; “Aplicar declaração” retorna antes de validar a própria pergunta já respondida. `app/chamados/services/declaracao_texto.py:382`, `app/chamados/services/declaracao_texto.py:424`, `app/chamados/services/declaracao_texto.py:678`, `app/chamados/services/declaracao_texto.py:709`.

**PROCEDENTE — aviso desaparece do detalhe após recarga.** O include existe, mas o contexto do detalhe seleciona apenas C6 vivas; a metade aceita é C6 morta. O retorno HTMX mostra aviso porque renderiza a pergunta diretamente, enquanto recarregar exclui aquela pergunta. Cura: filtro de apresentação incluir o predicado de espera. `app/chamados/services/acoes_chamado.py:348`, `app/chamados/juizes.py:1153`, `app/templates/chamados/detalhe_local.html:309`, `app/chamados/views.py:399`, `app/chamados/views.py:404`. P5 injeta pergunta diretamente e não testa essa montagem. `app/chamados/tests/test_bug_par_intervalo.py:487`.

**NÃO PROCEDE — templatetag derivando identidade paralela.** A tag chama o mesmo predicado; cartão e modal incluem o mesmo fragmento. `app/chamados/templatetags/chamados_tags.py:18`, `app/templates/chamados/partials/_pergunta_card.html:29`, `app/templates/chamados/partials/modal_fio.html:160`. A identidade lê pausas efetivas da escala; essa autoridade lê DNA da célula antes do template. `app/chamados/services/materializacao.py:86`, `app/escala/models.py:1189`, `app/escala/models.py:1197`, `app/escala/models.py:1207`.

Inventário compacto dos demais leitores:

| Leitor | Efeito |
|---|---|
| C6 / consulta viva | Aceite mata pergunta; SQL também a exclui. `app/chamados/juizes.py:1153`, `app/chamados/models.py:3362`. |
| Painel/badge | Crédito usa consulta viva; irmã não validada determina Cobrar/Validar. `app/chamados/services/raiox.py:72`, `app/chamados/services/raiox.py:133`, `app/chamados/services/verbos.py:34`, `app/chamados/services/verbos.py:47`. |
| Fio | Disputa aberta carrega perguntas; fechada só admite C6 vivas, ocultando aceite pendente se fechamento escapar. `app/chamados/services/fio.py:221`, `app/chamados/services/fio.py:238`. |
| Explicador | Espera antes de estado; fechamento por C7. `app/chamados/services/explicador.py:74`, `app/chamados/services/explicador.py:144`. |
| Projeção/API | Publicam espera e retiram aceitação. `app/chamados/services/declaracao_texto.py:484`, `app/api/views.py:1126`. |
| Vigia de carimbo/O330 | Espera sai da dívida e do alvo reparável. `app/chamados/services/vigia_carimbo.py:109`, `app/chamados/services/vigia_carimbo.py:133`, `app/chamados/services/passivo_validado.py:324`. |
| Vigia do fio | Continua julgando fio por C6; só importa espera na abertura de classes da porta. `app/chamados/services/vigia_fio.py:164`, `app/chamados/services/vigia_fio.py:370`, `app/chamados/services/vigia_fio.py:462`. |
| `o_que_segura` | Lê `porta_alcancavel_do_dia`, alimentada por C6 e verbos do painel. `app/folha/fora_da_folha.py:217`, `app/folha/fora_da_folha.py:221`, `app/chamados/juizes.py:1322`, `app/chamados/juizes.py:1396`. |
| Alerta diário | Toda disputa aberta segue recebendo aviso; não distingue espera. `app/ponto/management/commands/alertar_disputas_pendentes.py:33`, `app/ponto/management/commands/alertar_disputas_pendentes.py:59`. |
| Auditor I18 | Só acusa recusa vazia; espera normal possui código e não entra. `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`, `app/chamados/services/materializacao.py:1436`. |

Se irmã sair do fato, o vigia poderá classificar o dia como sem fio, passando pelos outros donos; alarme efetivo depende da célula e desses donos **[nao verificado]**. `app/chamados/services/vigia_fio.py:416`, `app/chamados/services/vigia_fio.py:430`.

## 09/11 10:36 · sessao 463d4de49e19 · - · - · origem=Ronald (tmux)

**PROCEDENTE — paridade inclui batidas retratadas.** O pós-par consulta `_vizinha_mesmo_tipo` após ambas as escritas, mas esse helper lê `Batida.objects` sem excluir `retratada_em`; uma S retratada às 01:30 reprova S01/E02 mesmo quando o conjunto vivo alterna. Cura: consumir `batidas_apuraveis`, acrescentando RED com retratada entre metades. `app/chamados/services/materializacao.py:1366`; `app/chamados/services/materializacao.py:178`; `app/chamados/services/materializacao.py:184`; autoridade efetiva: `app/ponto/turnos.py:2866`. Ocorrência em produção: [nao verificado].

**NAO PROCEDE — divisão transacional para par identificado.** Validação direta e varredura chegam a `_materializar_uma`; O330 também, com inclusão explícita dos carimbos sem lastro. O envelope identifica o par, espera ambas respondidas/validadas e chama `_materializar_par`; as duas escritas, julgamento posterior, carimbos e reconciliação ficam no mesmo savepoint. `app/chamados/services/validacao.py:469`; `app/chamados/services/validacao.py:474`; `app/chamados/services/materializacao.py:1596`; `app/chamados/services/passivo_validado.py:532`; `app/chamados/services/materializacao.py:1414`; `app/chamados/services/materializacao.py:1433`; `app/chamados/services/materializacao.py:1344`; `app/chamados/services/materializacao.py:1361`; `app/chamados/services/materializacao.py:1373`; `app/chamados/services/materializacao.py:1380`.

A garantia tem universo delimitado: quando identidade retorna `None`, aplica-se uma pergunta avulsa. Portanto, o código admite plantio individual para intervalo sem par elegível; isso inclui irmã já materializada e casos ambíguos, preservando o caminho avulso. `app/chamados/services/materializacao.py:73`; `app/chamados/services/materializacao.py:77`; `app/chamados/services/materializacao.py:1431`; teste explícito da irmã aplicada: `app/chamados/tests/test_bug_par_intervalo.py:725`.

**NAO PROCEDE — inversão entre dois validadores dessas metades.** Ambos adquirem primeiro a mesma linha do Colaborador, depois disputas, chamados e perguntas; os grupos são ordenados por PK. A validação relê a pergunta após travar. O330 trava o conjunto da competência antes dos dias. `app/chamados/services/materializacao.py:125`; `app/chamados/services/materializacao.py:134`; `app/chamados/models.py:2677`; `app/chamados/models.py:2686`; `app/chamados/models.py:2699`; `app/chamados/models.py:2703`; `app/chamados/services/validacao.py:434`; `app/chamados/services/validacao.py:435`; `app/chamados/services/passivo_validado.py:435`.

**FILA — prova concorrente.** O teste confere argumentos das travas, sem duas conexões concorrentes; ausência operacional de deadlock é [nao verificado]. `app/chamados/tests/test_bug_par_intervalo.py:605`.

**Censo de plantadores:** os únicos consumidores de `registrar_batida` cujo plantio recebe `PerguntaDisputa` são os ramos hora e data/hora; ambos chegam pelo envelope comum. `app/chamados/services/materializacao.py:1017`; `app/chamados/services/materializacao.py:1098`; `app/chamados/services/materializacao.py:1477`.

Demais consumidores encontrados por AST, considerando nomes importados/apelidos e chamadas por atributo: APIs de chão (`app/api/views.py:684`, `app/api/views_core.py:681`), exceção (`app/chamados/services/acoes_chamado.py:44`), regularização externa (`app/chamados/services/regularizacao_ext.py:77`), justificativas (`app/ponto/services/justificativa.py:188`, `app/ponto/services/justificativa.py:361`, `app/ponto/views.py:1815`), triagem (`app/ponto/services/triagem_batida.py:701`) e veredito do dia (`app/ponto/services/veredito_celula.py:322`). Instrumento PWA: `app/ponto/management/commands/testar_pwa.py:184`, `:198`, `:219`, `:228`, `:229`.

**FILA — unidades externas parcialmente aplicáveis.** Regularização externa continua após falha de metade e pode marcar lote materializado; veredito do dia acumula erro por marco e só reverte se nenhuma ação ocorreu. Não são o aceite das duas PerguntaDisputa. `app/chamados/services/regularizacao_ext.py:166`; `app/chamados/services/regularizacao_ext.py:191`; `app/chamados/services/regularizacao_ext.py:197`; `app/ponto/services/veredito_celula.py:320`; `app/ponto/services/veredito_celula.py:342`.

## 09/11 10:29 · sessao cbbbf78661b6 · SONDA-SEM-GRANT · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 10:26 · sessao 415d564f876f · - · - · origem=Ronald (tmux)

T2 está implementado: o selo executa a conferência real, exige a chave certa positiva e recusa rc 5; o primeiro estrago é INSERT, suficiente para detectar `AND false` nas subconsultas de `esc_tab` (`bin/tests/test_sonda_sem_grant_pg.sh:108`, `:133`, `:136`, `:137`, `:144`). A execução verde e os “16/16 mortos” permanecem **[nao verificado]**: o usuário não forneceu `sonda_v3_mut.log`, RED ou GREEN, e os próprios selos só apontam para esses arquivos (`bin/tests/test_sonda_sem_grant.sh:32`; `bin/tests/test_sonda_sem_grant_pg.sh:21`).

**PROCEDENTE — cobertura semântica ainda incompleta.** Há contraprovas estáticas adicionais aos mutantes alegados:

- Trocar a medida de `super` por constante zero (`bin/sonda_leitor_comum.sh:68`) não encontra caso de leitor superuser: o falso devolve `super=0`, e os estragos PG não incluem esse atributo (`bin/tests/test_sonda_sem_grant.sh:62`, `:129`; `bin/tests/test_sonda_sem_grant_pg.sh:144`). Sobrevivência executada **[nao verificado]**. Conserto mínimo: conceder SUPERUSER na fixture e exigir `super>0`, rc 5 e ausência de execução da sonda.
- Remover `is_grantable` apenas do ramo de defaults de schema (`bin/sonda_leitor_comum.sh:122`) escapa da cobrança textual, que verifica somente o ramo SELECT, e dos estragos PG, que não incluem defaults de schema (`bin/tests/test_sonda_sem_grant.sh:214`; `bin/tests/test_sonda_sem_grant_pg.sh:144`). Sobrevivência executada **[nao verificado]**. Conserto mínimo: caso `ALTER DEFAULT PRIVILEGES ... GRANT USAGE ON SCHEMAS ... WITH GRANT OPTION`, exigindo `default_escrita>0`.

Sete estragos PG já exercitam comportamento previsto na volta 2: INSERT, UPDATE de coluna, USAGE de sequência, CREATE de schema, TEMP de PUBLIC, membership e default INSERT (`bin/tests/test_sonda_sem_grant_pg.sh:144`, `:145`, `:149`, `:151`, `:153`, `:154`, `:155`; predicados atuais anteriores ao acréscimo de `is_grantable`: `bin/sonda_leitor_comum.sh:103`, `:108`, `:112`, `:115`, `:118`, `:121`, `:123`). Resultado executado com código v2 **[nao verificado]**. Os seis GRANT OPTION e o override por banco são os casos dirigidos às novidades T1/T3 (`bin/tests/test_sonda_sem_grant_pg.sh:146`, `:147`, `:148`, `:150`, `:152`, `:156`, `:157`).

**NÃO — trap ausente no erro normal.** O trap é instalado antes do nascimento do container e cobre as saídas explícitas por falha de criação, readiness, pg_hba e âncora (`bin/tests/test_sonda_sem_grant_pg.sh:37`, `:38`, `:44`, `:46`, `:53`, `:57`, `:82`). **PROCEDENTE, restrito:** a limpeza ignora falha de `docker stop` e não verifica remoção; vazamento nessa condição **[nao verificado]** (`bin/tests/test_sonda_sem_grant_pg.sh:37`). Conserto mínimo: fallback por nome exato com `docker rm -f`, conservando o rc original e tornando falha da limpeza visível.

Não identifiquei SQL inequivocamente inválida somente pela leitura; execução em PG17 **[nao verificado]**. O alvo declarado sai de `IMAGEM=postgres:17-alpine` (`bin/db_teste.sh:23`; `bin/tests/test_sonda_sem_grant_pg.sh:31`). Bash `-n` passou; execução dos testes não realizada **[nao verificado]**.

Os selos não estão órfãos: a régua descobre `tests/test_*.sh` e bloqueia na falha (`bin/regua.sh:138`, `:140`, `:143`). A conferência é produzida pelo comum e consumida pelos dois caminhos e pelo selo PG (`bin/sonda_leitor_comum.sh:63`; `bin/sonda_leitura.sh:184`, `:187`; `bin/sonda_provisionar.sh:72`, `:144`, `:145`; `bin/tests/test_sonda_sem_grant_pg.sh:108`, `:110`).

## 09/11 10:26 · sessao eb32a108d276 · - · - · origem=Ronald (tmux)

T1 — **NAO permanece como falha de medição**: `is_grantable` entra nas duas parcelas de `esc_tab` e em `esc_seq`, `esc_schema`, `esc_banco`, `default_escrita`; todas passam pelo mesmo parser. Referências: `bin/sonda_leitor_comum.sh:103`, `:108`, `:112`, `:115`, `:118`, `:121`, `:122`, `:131-149`. Resultado efetivo em PG17: [nao verificado].

T3 — **NAO permanece estaticamente**: `ro_banco` conta configuração do leitor no banco destino diferente de `on`; a porta remove esse ajuste e exige o mesmo `leitor|on` usado pela leitura antes de publicar a credencial. Referências: `bin/sonda_leitor_comum.sh:74-78`, `:132`, `:167-171`; `bin/sonda_provisionar.sh:108`, `:154-168`; `bin/sonda_leitura.sh:193-196`. Resultado efetivo: [nao verificado].

**ESMERIL da conferência — sem divergência encontrada entre os consumidores operacionais.** Todas as chaves são produzidas por `_q_conferencia`; leitura e porta usam `_conferencia_pronta`, que exige quatro presenças iguais a `1`, demais valores iguais a `0`, recusando chave ausente. Referências: `bin/sonda_leitor_comum.sh:66-149`; `bin/sonda_leitura.sh:184-187`; `bin/sonda_provisionar.sh:72`, `:144-145`.

| Campos | Produtor da medida |
|---|---|
| `role`, `super`, `poderes`, `login` | `bin/sonda_leitor_comum.sh:67-70` |
| `ro`, `ro_banco` | `bin/sonda_leitor_comum.sh:71-78` |
| `connect`, `usage_falta` | `bin/sonda_leitor_comum.sh:79-83` |
| `tab_falta`, `seq_falta`, `default_falta` | `bin/sonda_leitor_comum.sh:84-98` |
| `esc_tab`, `esc_seq`, `esc_schema`, `esc_banco` | `bin/sonda_leitor_comum.sh:99-118` |
| `default_escrita`, `membro`, `dono` | `bin/sonda_leitor_comum.sh:119-125` |

O selo falso replica os mesmos valores esperados e verifica trechos por chave; o selo PG usa a consulta e o parser comuns, e exige chave positiva nos estragos. Referências: `bin/tests/test_sonda_sem_grant.sh:62`, `:129`, `:184-219`; `bin/tests/test_sonda_sem_grant_pg.sh:107-111`, `:130-157`.

**Escritores/leitores:**

- Role, senha e ajustes de sessão: porta escreve em `bin/sonda_provisionar.sh:104-109`; conferência lê em `bin/sonda_leitor_comum.sh:67-78`, `:123-125`; login verifica em `:167-171`.
- ACL/defaults: porta escreve em `bin/sonda_provisionar.sh:119-140`; conferência lê em `bin/sonda_leitor_comum.sh:79-125`.
- `.env_leitor`: porta lê senha em `bin/sonda_provisionar.sh:75-78`, grava temporário/promove em `:165-168`; leitura lê/verifica em `bin/sonda_leitura.sh:173-179`, Docker recebe em `:234`.
- `POSTGRES_DB/USER/PASSWORD/HOST/PORT`: Docker recebe em `bin/sonda_leitura.sh:234-236`; Django lê em `app/config/settings/saas.py:86-90`; sombra sobrescreve banco/host/porta em `app/config/settings/sombra.py:49-51`.
- Segundo escritor operacional desses estados fora da porta: não encontrado na pesquisa; inexistência absoluta [nao verificado]. Escritores dos selos usam raiz temporária: `bin/tests/test_sonda_sem_grant.sh:86-94`; `bin/tests/test_sonda_sem_grant_pg.sh:77-83`, `:175-179`.

**Limitação verificável do selo PG:** `docker run` da sonda vira no-op; assim ele não exercita conexão Django nem origem IP do container da sonda. Referência: `bin/tests/test_sonda_sem_grant_pg.sh:95`. Impacto em produção: [nao verificado].

**Não sustentar achado pela ausência de REVOKE explícito de coluna:** a semântica real de propagação de `REVOKE ALL ON TABLES` não foi executada nesta auditoria; portanto alegar que os casos de coluna necessariamente ficam vermelhos seria [nao verificado]. A porta usa esse comando em `bin/sonda_provisionar.sh:121`; o selo exige cura em `bin/tests/test_sonda_sem_grant_pg.sh:139-147`.

## 09/11 10:25 · sessao 305d1a7821ea · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · construtor · origem=Code

1. **Cura na origem:** [materializacao.py:1388](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-075842/app/chamados/services/materializacao.py:1388). A antiga guarda lia o carimbo como **fato de escrita a desfazer**, não como vitalidade. Agora o juiz do lastro decide a limpeza, executada por `descarimbar`. O contrato continua estrito: detecta comparações cruas com `None`; a comparação saiu do chamador, sem nova allowlist (`chamados/tests/test_contract_leitores_vitalidade.py:315`).

   **ESCREVE:** `PerguntaDisputa.descarimbar`, pelo dono `_escrever_carimbo`, limpa `materializada_em`, `materializada_por` e `via_resolucao` juntos (`chamados/models.py:3206`, `:3100`). **LÊ:** juiz do lastro (`chamados/juizes.py:645`); devolução da validação (`chamados/services/validacao.py:163`). `_materializar_uma` chama o ato alterado (`chamados/services/materializacao.py:1439`).

2. **M7:** novo caso em [test_bug_par_intervalo.py:273](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-075842/app/chamados/tests/test_bug_par_intervalo.py:273). Quatro perguntas representam duas pausas declaradas; o DNA tem uma pausa para impedir que outra guarda esconda M7. Cobra exatamente `(None, 'par_ambiguo')` e o efeito avulso do HEAD: recusa por paridade, validação devolvida, nenhuma batida nova, irmãs e chão preservados (`chamados/tests/test_bug_par_intervalo.py:288`, `:294`).

   **ESCREVE a fixture:** `pergunta` e `responder` (`chamados/tests/test_bug_par_intervalo.py:81`, `:88`). **LÊ a identidade:** `identificar_par_intervalo`, consumido pela validação e pelo materializador (`chamados/services/materializacao.py:55`; `chamados/services/validacao.py:467`).

3. **M35:** o helper agora fornece `disputa_obj` e `pode_agir_map`, abre o ramo real do modal e cobra o `<div>` completo do aviso, sem projeção injetada (`chamados/tests/test_bug_par_intervalo.py:481`, `:498`).

   **ESCREVE o contexto:** helper do teste (`chamados/tests/test_bug_par_intervalo.py:487`); em produção, `fio.py:253`, `:258`, `:467`. **LÊ:** template real (`templates/chamados/partials/modal_fio.html:122`, `:160`), fragmento compartilhado (`templates/chamados/partials/_espera_par_intervalo.html:2`) e filtro (`chamados/templatetags/chamados_tags.py:16`).

4. **Mutantes:** ajuste da âncora M6 e inclusão de M38 em [mutantes_bug_par_intervalo.py:34](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-075842/app/chamados/tests/mutantes_bug_par_intervalo.py:34), `:131`. O runner lê essas definições e recompila em memória (`chamados/tests/mutantes_bug_par_intervalo.py:212`).

| Mutante | Teste discriminante | Asserção esperada | [nao verificado]
|---|---|---|
| M7 | `test_R7_multiplas_perguntas_nao_escolhem_primeiro_par` | Igualdade com `(None, 'par_ambiguo')`, linha 288. |
| M25 / M35 | respectivos testes P5 de cartão/modal | Aviso HTML presente exatamente uma vez, linha 498. | [nao verificado]
| M26 / M38 novo | `test_P6_O330_falha_com_carimbo_previo_descarimba_antes_de_devolver` | Ambas descarimbadas, linha 529; autor e via limpos, linhas 532–533. |
| M6 | `test_R4_lastro_da_segunda_metade_reprova_as_duas` | Retorno recusado, linha 211. |

Essas mortes por asserção nesta volta são **[nao verificado]**; não executei testes ou mutantes. [nao verificado]

**DECISAO-ABERTA:** C7 já está ligado ao predicado nesta cópia (`chamados/models.py:1861`). Nenhuma alteração de model foi necessária nesta volta. GREEN e certificação pelo Code/opus permanecem **[nao verificado]**.

Comandos individuais reservados ao Code:

```bash
DJANGO_SETTINGS_MODULE=config.settings.saas PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_contract_leitores_vitalidade.LeitoresVitalidadeContract.test_ninguem_julga_vitalidade_por_carimbo_cru --noinput
DJANGO_SETTINGS_MODULE=config.settings.saas PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_bug_par_intervalo.BugParIntervaloCol960.test_R7_multiplas_perguntas_nao_escolhem_primeiro_par --noinput
DJANGO_SETTINGS_MODULE=config.settings.saas PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_bug_par_intervalo.BugParIntervaloCol960.test_P5_modal_le_espera_sem_projecao_injetada --noinput
DJANGO_SETTINGS_MODULE=config.settings.saas PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_bug_par_intervalo.BugParIntervaloCol960.test_P6_O330_falha_com_carimbo_previo_descarimba_antes_de_devolver --noinput

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo M6
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo M7
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo M25
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo M26
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo M35
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo M38
```

Executei somente a verificação estática abaixo: **rc 0**, `py_compile` dos três Python tocados sem persistir bytecode e conferência de sintaxe/âncoras M1–M38 (`chamados/tests/mutantes_bug_par_intervalo.py:173`, `:188`).

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo --sintaxe
```

## 09/11 10:25 · sessao 936ff5f03d79 · - · - · origem=Ronald (tmux)

T4 — **PROCEDENTE residual; fecha apenas o caminho sem erro de arquivo.** A invalidação grava `SOMBRA_DIA=` e `SOMBRA_STATUS=REFAZENDO` antes do `dropdb`, sob a trava (`bin/sombra.sh:104`, `bin/sombra.sh:269`, `bin/sombra.sh:270`, `bin/sombra.sh:458`). Porém o script usa `set -uo pipefail`, sem `-e`, e não testa o retorno da invalidação; falha no `mv` do carimbo permite continuar para o `dropdb` com carimbo antigo verde (`bin/sombra.sh:45`, `bin/sombra.sh:97`, `bin/sombra.sh:269`). Conserto mínimo: `invalidar_carimbo || falhou ...`; fazer `carimbar` propagar falhas intermediárias e criar temporário na pasta do carimbo (`bin/sombra.sh:89`).

A invalidação normal é recusada por incremental, bloco, conferir e cobertura; placar e deploy delegam sua decisão a conferir (`bin/sombra.sh:204`, `bin/sombra.sh:350`, `bin/sombra.sh:453`, `app/core/sombra_cobertura.py:35`, `app/core/sombra_cobertura.py:37`, `bin/placar_code.sh:155`, `bin/deploy.sh:162`). Entretanto **PROCEDENTE semântica residual no placar**: ele continua apresentando o antigo `SOMBRA_INCOMPLETA=0` como medida do “carimbo de hoje”, mesmo após dia vazio/REFAZENDO (`bin/placar_code.sh:160`, `bin/placar_code.sh:161`). Conserto mínimo: publicar `?` quando o carimbo não for válido para essa medida (`bin/placar_code.sh:160`).

T5 — **NÃO, no caminho da cópia a corrida relatada está fechada.** O `flock` vem antes da leitura/geração da senha e permanece aberto até o `mv` (`bin/sonda_provisionar.sh:68`, `bin/sonda_provisionar.sh:69`, `bin/sonda_provisionar.sh:75`, `bin/sonda_provisionar.sh:79`, `bin/sonda_provisionar.sh:168`). A chamada do refazer segue sombra → porta (`bin/sombra.sh:458`, `bin/sombra.sh:321`, `bin/sombra.sh:246`). Nenhuma aquisição da trava da sombra consta no provisionador; inversão/autoe­spera nesse caminho: **NÃO** (`bin/sonda_provisionar.sh:41`, `bin/sonda_provisionar.sh:68`, `bin/sonda_provisionar.sh:153`). A trava da sombra evita reacquisição nos filhos pela marca herdada (`bin/sombra_trava.sh:22`, `bin/sombra_trava.sh:38`).

Relógio — **PROCEDENTE preexistente D3.** `HOJE` é capturado antes de aguardar a trava; a data da base é consultada novamente ao publicar, permitindo datas discordantes se a espera/execução cruzar meia-noite (`bin/sombra.sh:83`, `bin/sombra.sh:458`, `bin/sombra.sh:322`). A cobertura também consulta relógio quando o chamador omite `hoje` (`app/core/sombra_cobertura.py:34`, `bin/sombra.sh:236`). Conserto mínimo: capturar o dia depois da trava e passar a data explicitamente; cruzamento real em produção [nao verificado].

Inventário do carimbo:

| Campos | Quem escreve | Quem lê |
|---|---|---|
| `SOMBRA_DIA` | invalidação/completo (`bin/sombra.sh:104`, `bin/sombra.sh:322`) | incremental/bloco/conferir/cobertura (`bin/sombra.sh:204`, `bin/sombra.sh:350`, `bin/sombra.sh:433`, `app/core/sombra_cobertura.py:35`) |
| `SOMBRA_STATUS` | invalidação/incremental/completo/bloco (`bin/sombra.sh:104`, `bin/sombra.sh:221`, `bin/sombra.sh:325`, `bin/sombra.sh:426`) | incremental/conferir/cobertura (`bin/sombra.sh:218`, `bin/sombra.sh:433`, `app/core/sombra_cobertura.py:37`) |
| `SOMBRA_TIPO` | incremental/completo (`bin/sombra.sh:219`, `bin/sombra.sh:322`) | incremental/conferir/cobertura (`bin/sombra.sh:205`, `bin/sombra.sh:435`, `app/core/sombra_cobertura.py:39`) |
| `SOMBRA_BASE_DIA` | completo (`bin/sombra.sh:322`) | incremental/conferir/cobertura (`bin/sombra.sh:205`, `bin/sombra.sh:437`, `app/core/sombra_cobertura.py:45`) |
| `SOMBRA_JANELA_INI/FIM` | incremental/completo (`bin/sombra.sh:219`, `bin/sombra.sh:323`) | conferir/cobertura (`bin/sombra.sh:437`, `app/core/sombra_cobertura.py:42`) |
| `SOMBRA_DIVERGE` | completo (`bin/sombra.sh:323`) | bloco/conferir (`bin/sombra.sh:421`, `bin/sombra.sh:438`) |
| `SOMBRA_INCOMPLETA` | completo (`bin/sombra.sh:323`) | placar (`bin/placar_code.sh:160`) |
| `SOMBRA_INCREMENTAL_EM` | incremental (`bin/sombra.sh:220`) | conferir (`bin/sombra.sh:437`) |
| `SOMBRA_ERROS` | bloco (`bin/sombra.sh:425`) | conferir (`bin/sombra.sh:438`) |
| `DUMP`, `REFEITA`, `BLOCO` | completo/bloco (`bin/sombra.sh:322`, `bin/sombra.sh:324`, `bin/sombra.sh:424`) | leitor operacional [nao verificado] |
| `INCREMENTAL_S`, `BLOCO_NA_JANELA` | incremental (`bin/sombra.sh:220`, `bin/sombra.sh:221`) | leitor operacional [nao verificado] |
| `COMANDOS`, `ALARMES`, `FORA_DO_ENSAIO` | bloco (`bin/sombra.sh:424`, `bin/sombra.sh:425`) | leitor operacional [nao verificado] |

A gravação material dos campos passa por `carimbar`, que preserva os campos não substituídos (`bin/sombra.sh:89`, `bin/sombra.sh:91`, `bin/sombra.sh:94`, `bin/sombra.sh:97`). O selo de host testa a invalidação bem-sucedida contra carimbo verde, mas não cobre falha da escrita do carimbo (`bin/tests/test_sonda_sem_grant.sh:361`, `bin/tests/test_sonda_sem_grant.sh:362`, `bin/tests/test_sonda_sem_grant.sh:364`).

## 09/11 10:10 · sessao 86b79a2759f4 · FIO-E0-V4-E4-ESCRITORES · censo · origem=Code

**O número 16 confere para as portas de alteração do estado. Incluindo os nascimentos por `create`/`get_or_create`/inserção, como o pedido exige, são 22 portas produtivas: 16 + 2 criadores de Chamado + 3 de Disputa + 1 de Pergunta.** Há ainda o executor genérico `PerguntaDisputa.save` e uma exclusão restrita a ensaio, discriminados abaixo. A diferença não é uma porta nova da E4: é a inclusão dos nascimentos que o recorte histórico separava como identidade. (`app/docs/RELATO.md:242`; `app/chamados/models.py:833`; `app/chamados/models.py:1054`; `app/chamados/models.py:2822`; `app/chamados/services/disputa_emissao.py:474`; `app/chamados/services/disputa_emissao.py:1024`; `app/chamados/services/disputa_emissao.py:1299`; `app/chamados/models.py:3402`; `app/chamados/models.py:563`)

A classificação usa a E1: **decisão** é o ato preservado na sequência append-only; **efeito** é sua consequência, conferida contra o fato/lavra; **fica** é identidade, data, cache ou metadado do continente. Uma porta pode ter ramos diferentes, explicitados sem contá-la duas vezes. A E1 declara que os campos atuais são projeção até a E4; a tarefa exige conferir decisões e efeitos, sem escritor próprio. (`app/chamados/models.py:3828`; `app/chamados/models.py:3833`; `app/chamados/models.py:3840`; `app/chamados/services/tarefa_do_dia.py:118`; `app/chamados/services/tarefa_do_dia.py:140`; `app/docs/CORTES.md:426`)

A análise foi estática por AST, incluindo imports com apelido, imports relativos, módulos `__init__` e imports locais. Não houve importação da aplicação, consulta a banco/log nem escrita de arquivo. Completude perante despacho inteiramente dinâmico, SQL construído dinamicamente, templates e execução real: **[nao verificado]**. Nos chamadores abaixo, **P** significa código produtivo/operacional; **T** identifica testemunhas de teste, sem afirmar execução delas nem esgotar todas as fixtures de nascimento. **[nao verificado]** [nao verificado]

Para compactar os campos repetidos:

- **C** = `materializada_em`, `materializada_por`, `via_resolucao`. (`app/chamados/models.py:3117`)
- **R** = `resposta_colab`, `respondida_em`; **V** = `validada_em`, `validada_por`. (`app/chamados/models.py:2444`; `app/chamados/models.py:3366`)
- **L** = `veredito_lavrado`, `julgado_em`, `janela_expira_em`. (`app/chamados/veredito_lavrado.py:250`)
- **F** = `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`. (`app/chamados/models.py:1933`)
- **DP** = `DecisaoPergunta`; “sem DP próprio” não significa ausência de trilha nem ausência de decisão registrada pelo chamador. (`app/chamados/models.py:3836`; `app/chamados/services/disputa_emissao.py:1612`)

Leis/cortes conferidos para as decisões abaixo: L-002, L-007, L-051, L-061, L-073, L-080, L-085, L-088, L-110, L-111, L-113, L-117 e L-124; FIO-E0-V4 e LEI-TRANCA-E2-JUIZ. (`app/docs/LEIS.md:60`; `app/docs/LEIS.md:65`; `app/docs/LEIS.md:92`; `app/docs/LEIS.md:95`; `app/docs/LEIS.md:100`; `app/docs/LEIS.md:107`; `app/docs/LEIS.md:117`; `app/docs/LEIS.md:120`; `app/docs/LEIS.md:136`; `app/docs/LEIS.md:137`; `app/docs/LEIS.md:151`; `app/docs/LEIS.md:138`; `app/docs/LEIS.md:145`; `app/docs/CORTES.md:426`; `app/docs/CORTES.md:427`)

## CENSO

escritores:

- `app/chamados/models.py:201` `ChamadoColaborador._gravar_estado` -- `status_local=novo`; ao terminar, `resolvido_em` recebe instante, `via_encerramento=via`, urgência é rebaixada; ao reviver, limpa resolução/via e restaura urgência; pode gravar `respondido_por`; grava `atualizado_em` -- **efeito** nas transições derivadas; **decisao** no ramo de transição humana. Persiste em `app/chamados/models.py:296`; sem DP próprio, com trilha `estado_chamado` e relavra L das perguntas atingidas. P: `_aplicar` (`app/chamados/models.py:450`), `analisar` (`app/chamados/models.py:806`), `reconciliar` (`app/chamados/models.py:654`), `cancelar` (`app/chamados/models.py:788`), `reabrir` (`app/chamados/models.py:1442`), `encerrar` (`app/chamados/models.py:1616`), `encerrar_por_competencia_trancada` (`app/chamados/models.py:1636`). T: `app/chamados/tests/test_hx_b7_trilha.py:32`; `app/chamados/tests/test_o261e_f1_porta_encerramento.py:206`; `app/chamados/tests/test_o261e_f1_codex_v5.py:89`.

- `app/chamados/models.py:1852` `DisputaSupervisao.fechar` -- F recebe instante, autor, texto e motivo; excepcionalmente carimba perguntas vivas -- **efeito** no fechamento normal, após nenhuma viva/resposta sem veredito; **decisao** nas exceções `admin_encerra`/`desligamento`, que já registram DP `SILENCIO` no mesmo atomic (`app/chamados/models.py:1915`; `app/chamados/models.py:1921`; `app/chamados/models.py:1933`). P: `admin_encerra` e `remover_motivo` (`app/chamados/models.py:1947`; `app/chamados/models.py:2069`); reconciliador (`app/chamados/reconciliador.py:412`; `app/chamados/reconciliador.py:612`; `app/chamados/reconciliador.py:1171`); foto (`app/api/foto_recebida.py:80`); desligamento (`app/colaboradores/services/desligamento.py:142`); entrada sincronizada (`app/ponto/services/entrada_sincronizada.py:59`); materialização (`app/chamados/services/materializacao.py:586`; `app/chamados/services/materializacao.py:1333`); emissão (`app/chamados/services/disputa_emissao.py:1281`; `app/chamados/services/disputa_emissao.py:1961`); tranca (`app/chamados/services/competencia_trancada.py:204`); beco (`app/chamados/services/becos.py:57`); comandos (`app/ponto/management/commands/fechar_chamados_meio_periodo.py:109`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`). T: `app/chamados/tests/test_porta_disputa_ciclo.py:285`; o contrato e as exceções estão na própria porta (`app/chamados/models.py:1855`).

- `app/chamados/models.py:2123` `DisputaSupervisao.reabrir_admin` -- limpa F; incrementa `reaberturas_admin`; pode agregar `motivos` -- **decisao** no ato administrativo; **fica** o contador/metadado. Persiste em `app/chamados/models.py:2158`; sem DP próprio. P: `app/chamados/services/acoes_disputa.py:259`. T: `app/chamados/tests/test_disputa_transicoes.py:134`; `app/chamados/tests/test_disputa_transicoes.py:148`; `app/chamados/tests/test_disputa_transicoes.py:156`; `app/chamados/tests/test_disputa_transicoes.py:163`; `app/chamados/tests/test_disputa_transicoes.py:172`; `app/chamados/tests/test_disputa_transicoes.py:181`.

- `app/chamados/models.py:2160` `DisputaSupervisao.reabrir_sistema` -- limpa F -- **efeito**, religando o continente após a pergunta voltar a exigir ação. Persiste em `app/chamados/models.py:2174`; sem DP próprio. P: `app/chamados/models.py:1572`; `app/chamados/services/materializacao.py:686`; `app/chamados/services/validacao.py:218`; `app/chamados/services/relavra_via.py:261`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:165`. Chamador direto de teste não localizado: **[nao verificado]**.

- `app/chamados/models.py:3092` `PerguntaDisputa.carimbar` -- C recebe instante/autor/via; opcionalmente completa `respondida_em`, V e L -- **efeito** para o carimbo; **decisao** quando completa resposta/validação. Persiste em `app/chamados/models.py:3141`; sem DP próprio, inclusive nos ramos que completam assinaturas (`app/chamados/models.py:3119`). P: `app/chamados/reconciliador.py:405`; `app/chamados/reconciliador.py:595`; `app/chamados/models.py:3276`; `app/chamados/services/materializacao.py:561`; `app/chamados/services/materializacao.py:1277`; `app/chamados/services/materializacao.py:1431`; `app/chamados/services/disputa_emissao.py:1860`; `app/chamados/services/disputa_emissao.py:1934`. T: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:725`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:738`; `app/chamados/tests/test_o261e_f2_fila.py:194`; `app/chamados/tests/test_o261e_f2_fila.py:205`; `app/chamados/tests/test_o261e_f2_fila.py:221`; `app/chamados/tests/test_o261e_f2_fila.py:228`.

- `app/chamados/models.py:3157` `PerguntaDisputa.carimbar_identidade` -- C recebe via terminal/autor/instante, ou preserva autor/instante anteriores; pode gravar `recusa_motivo` -- **efeito** da identidade/decisão aplicável; o ato humano que o determina é **decisao**. Persiste em `app/chamados/models.py:3179`; sem DP próprio. P: `fechar_por_admin` (`app/chamados/models.py:3053`), `remover_motivo` (`app/chamados/models.py:2058`), recusa/paridade (`app/chamados/services/materializacao.py:924`; `app/chamados/services/materializacao.py:1099`), duplicata (`app/chamados/services/fio.py:1350`). O chamador `remover_motivo` já registra DP `RETRATACAO` (`app/chamados/models.py:2053`); `Disputa.fechar` registra `SILENCIO` (`app/chamados/models.py:1921`). T: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:744`; `app/chamados/tests/test_e1_v5_portas_backfill.py:166`; `app/chamados/tests/test_e1_volta2.py:131`.

- `app/chamados/models.py:3192` `PerguntaDisputa.descarimbar` -- C passa a `None/None/''`; recusa linha fora do fato; `salvar=False` só prepara memória -- **efeito**. Persiste em `app/chamados/models.py:3201`; sem DP próprio. P: `app/chamados/models.py:3287`; `app/chamados/models.py:3304`; `app/chamados/models.py:3311`; simulação sem persistência em `app/chamados/models.py:1506`; `app/chamados/services/relavra_via.py:246`. T: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:758`; `app/chamados/tests/test_e1_v5_portas_backfill.py:168`; `app/chamados/tests/test_e1_v5_portas_backfill.py:176`.

- `app/chamados/models.py:3241` `PerguntaDisputa.descarimbar_em_lote` -- C passa a `None/None/''`, mais campos recebidos em `outros`; exclui vias fora do fato -- **efeito**. `QuerySet.update` em `app/chamados/models.py:3253`; sem DP próprio. P: `app/chamados/models.py:1565`; `app/chamados/services/disputa_emissao.py:1627`, cujo chamador registra DP antes do update (`app/chamados/services/disputa_emissao.py:1612`). T: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:752`; `app/chamados/tests/test_e1_v13_ordem.py:90`; `app/chamados/tests/test_e1_v13_ordem.py:101`; `app/chamados/tests/test_e1_v13_ordem.py:106`.

- `app/chamados/models.py:3257` `PerguntaDisputa.encerrar_por_fato_no_espelho` -- somente `via_resolucao=fato_materializado` -- **efeito**. Persiste em `app/chamados/models.py:3265`; sem DP próprio. P: `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`. T: `app/chamados/tests/test_porta_chamado_e3.py:72`; `app/chamados/tests/test_porta_chamado_e3.py:74`; `app/chamados/tests/test_e1_v5_portas_backfill.py:170`.

- `app/chamados/services/validacao.py:127` `_carimbar_validacao` -- V recebe instante/autor -- **decisao**. Save em `app/chamados/services/validacao.py:137`; DP `VALIDACAO`, mesmo instante/atomic, em `app/chamados/services/validacao.py:139`. P: `_aceitar_a_folga` (`app/chamados/services/validacao.py:288`; `app/chamados/services/validacao.py:320`) e `validar_pergunta` (`app/chamados/services/validacao.py:435`). T: `app/chamados/tests/test_e1_entradas.py:98`.

- `app/chamados/services/validacao.py:163` `devolver_validacao_recusada` -- V passa a nulo, preservando a resposta; pode reabrir o continente pela porta -- **decisao** `DEVOLUCAO`. Save em `app/chamados/services/validacao.py:202`; DP no mesmo atomic em `app/chamados/services/validacao.py:205`. P: `app/chamados/services/validacao.py:464`. T: `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:144`; `app/chamados/tests/test_o261_recusa_fala.py:178`; `app/chamados/tests/test_o261_recusa_fala.py:186`; `app/chamados/tests/test_o261_recusa_fala.py:199`; `app/chamados/tests/test_o261_recusa_fala.py:210`; `app/chamados/tests/test_e1_volta2.py:108`.

- `app/chamados/services/disputa_emissao.py:1422` `responder_pergunta_disputa_core` -- grava R; limpa V anterior; pode descarimbar e gravar recusa explicativa de escape -- **decisao** `RESPOSTA`; limpeza dos carimbos é consequência da resposta nova. Save em `app/chamados/services/disputa_emissao.py:1505`; DP em `app/chamados/services/disputa_emissao.py:1508`, preservando projeção legada sem linha. P: `app/api/views.py:2651`; `app/chamados/views.py:1283`; `app/chamados/services/declaracao_texto.py:419`; `app/chamados/services/declaracao_texto.py:693`; `app/chamados/services/acoes_disputa.py:371`. T: `app/chamados/tests/test_porta_resposta_pergunta.py:177`; `app/chamados/tests/test_porta_resposta_pergunta.py:209`; `app/chamados/tests/test_porta_resposta_pergunta.py:229`; `app/chamados/tests/test_e1_volta2.py:62`; `app/chamados/tests/test_e1_volta2.py:75`.

- `app/chamados/services/disputa_emissao.py:1559` `reabrir_para_correcao` -- limpa R e V; opcionalmente desfaz C da absorção -- **decisao** `REABERTURA` ou `RECUSA`. DP com foto anterior em `app/chamados/services/disputa_emissao.py:1612`; update em `app/chamados/services/disputa_emissao.py:1629`, ou porta de lote em `app/chamados/services/disputa_emissao.py:1627`; emite evento da célula no mesmo atomic. P, incluindo apelidos locais: `app/chamados/models.py:2234`; `app/chamados/views.py:1436`; `app/chamados/services/materializacao.py:682`; `app/chamados/services/validacao.py:593`; `app/chamados/services/acoes_disputa.py:103`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`. T: `app/chamados/tests/test_reabrir_nao_destroi.py:42`; `app/chamados/tests/test_reabrir_nao_destroi.py:102`; `app/chamados/tests/test_e1_v13_ordem.py:64`; `app/chamados/tests/test_e1_volta2.py:210`; `app/chamados/tests/test_e1_volta2.py:226`.

- `app/chamados/services/disputa_emissao.py:1635` `responder_automaticamente` -- R recebe texto/instante do fato entregue por outro caminho; não sobrescreve texto existente -- **decisao** de sistema `RESPOSTA`. Save em `app/chamados/services/disputa_emissao.py:1652`; DP em `app/chamados/services/disputa_emissao.py:1653`. P: `app/api/foto_recebida.py:48`. T: `app/chamados/tests/test_porta_chamado_e3.py:46`; `app/chamados/tests/test_porta_chamado_e3.py:48`; `app/chamados/tests/test_porta_chamado_e3.py:53`.

- `app/chamados/services/disputa_emissao.py:1658` `carimbar_resposta_pela_trilha` -- `respondida_em` recebe o instante original testemunhado, sem inventar instante da resposta -- **decisao** reconstruída `RESPOSTA`. Save em `app/chamados/services/disputa_emissao.py:1673`; DP em `app/chamados/services/disputa_emissao.py:1674`. P: `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`. T: `app/chamados/tests/test_curar_perguntas_em_limbo.py:37`; `app/chamados/tests/test_curar_perguntas_em_limbo.py:50`; `app/chamados/tests/test_curar_perguntas_em_limbo.py:51`; `app/chamados/tests/test_porta_resposta_pergunta.py:247`; `app/chamados/tests/test_porta_resposta_pergunta.py:251`; `app/chamados/tests/test_porta_resposta_pergunta.py:262`.

- `app/chamados/services/relavra_via.py:106` `relavrar` -- via derivada atual e L, no mesmo save -- **efeito** para a via; **fica** L como cache. Save em `app/chamados/services/relavra_via.py:127`; sem DP próprio; exclui vias que não pertencem a `VIAS_DERIVADAS` (`app/chamados/services/relavra_via.py:118`). P: `app/chamados/services/relavra_via.py:157`; `app/chamados/management/commands/relavrar_via_i6.py:102`. T: `app/chamados/tests/test_porta_veredito_pergunta.py:363`; `app/chamados/tests/test_porta_veredito_pergunta.py:383`; `app/chamados/tests/test_porta_veredito_pergunta.py:386`; `app/chamados/tests/test_porta_veredito_pergunta.py:404`.

- `app/chamados/models.py:833` `ChamadoColaborador.abrir` -- cria identidade/contexto e `status_local` inicial decidido pelo motor; `get_or_create` em `app/chamados/models.py:971`, defaults em `app/chamados/models.py:938` -- **fica** o nascimento/identidade; não é escritor da tarefa. Sem DP próprio. P: `app/ponto/views.py:3012`; `app/escala/regua_defesa.py:51`; `app/api/views.py:1997`; `app/chamados/reconciliador.py:910`; `app/chamados/views.py:973`; `app/ponto/services/cluster_espurio.py:130`; `app/ponto/services/ausencia.py:659`; `app/ponto/management/commands/detectar_par_relampago.py:368`; `app/ponto/management/commands/reconciliar_grade.py:507`; `app/ponto/management/commands/detectar_ausencias.py:190`; `app/ponto/management/commands/detectar_ausencias.py:271`; `app/ponto/management/commands/detectar_ausencias.py:510`; `app/ponto/management/commands/detectar_ausencias.py:693`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`; `app/ponto/management/commands/processar_alertas_turno.py:335`; `app/chamados/services/disputa_emissao.py:568`; `app/chamados/services/disputa_emissao.py:1101`; `app/chamados/services/disputa_emissao.py:1336`; `app/chamados/services/adesao.py:74`. T: `app/ponto/tests/test_o_outbox_celula.py:287`; `app/ponto/tests/test_o_outbox_celula.py:555`.

- `app/chamados/models.py:1054` `ChamadoColaborador.criar` -- cria identidade/contexto/status inicial; auditoria aberta pode nascer `registrado` pela decisão do motor (`app/chamados/models.py:1064`); create em `app/chamados/models.py:1095` -- **fica**. Sem DP próprio. P, incluindo `_Chamado`, `_Ch9` e `_ChG`: `app/colaboradores/views.py:1978`; `app/colaboradores/views.py:2363`; `app/ponto/utils.py:40`; `app/ponto/utils.py:103`; `app/ponto/views.py:291`; `app/api/views_core.py:747`; `app/api/views.py:747`; `app/api/views.py:2201`; `app/colaboradores/services/vinculo.py:393`; `app/colaboradores/services/desligamento.py:163`; `app/ponto/services/justificativa.py:291`; `app/ponto/services/geofence.py:339`; `app/ponto/services/triagem_batida.py:107`; `app/ponto/services/triagem_batida.py:336`; `app/ponto/services/triagem_batida.py:459`; `app/ponto/management/commands/processar_alertas_avancados.py:121`; `app/chamados/services/regularizacao_ext.py:246`; `app/chamados/services/abertura.py:47`; `app/chamados/services/abertura.py:172`. Ensaio: `app/chamados/management/commands/smoke_chamados.py:123`. T: `app/colaboradores/tests/test_vincular_escala_selos.py:26`; `app/ponto/tests/test_cobertura_followup.py:45`.

- `app/chamados/services/disputa_emissao.py:474` `_abrir_ou_agregar_disputa` -- cria continente, motivos/contexto e estado inicial aberto, `fechada_em=NULL` pelo campo; create em `app/chamados/services/disputa_emissao.py:587` -- **fica**. Default em `app/chamados/models.py:1754`; sem DP próprio. P: `app/chamados/services/disputa_emissao.py:627`; `app/chamados/services/disputa_emissao.py:662`; `app/ponto/management/commands/reconciliar_grade.py:424`. Ensaio: `app/chamados/management/commands/smoke_chamados.py:99`. T: `app/chamados/tests/test_e1_fila_v2_2_integracao.py:132`.

- `app/chamados/services/disputa_emissao.py:1024` `criar_disputa_a_partir_de_chamados_orfaos` -- cria continente aberto, identidade, motivos/contexto; create em `app/chamados/services/disputa_emissao.py:1120` -- **fica**. `fechada_em=NULL` pelo default (`app/chamados/models.py:1754`); sem DP próprio. P: `app/ponto/management/commands/processar_alertas_avancados.py:159`; `app/ponto/management/commands/detectar_ausencias.py:802`; `app/ponto/management/commands/processar_alertas_turno.py:385`; `app/chamados/services/cobrar_dia.py:49`; `app/chamados/services/pergunta_no_app.py:208`; `app/chamados/services/acoes_disputa.py:222`. T: `app/chamados/tests/test_chamados_models.py:135`; `app/chamados/tests/test_chamados_models.py:155`; `app/chamados/tests/test_chamados_models.py:157`; `app/tenants/tests.py:74`; `app/tenants/tests.py:85`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:78`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:244`.

- `app/chamados/services/disputa_emissao.py:1299` `criar_disputa_ancora_de_chamado` -- cria continente aberto e vínculo com chamado, motivos/contexto; create em `app/chamados/services/disputa_emissao.py:1355` -- **fica**. `fechada_em=NULL` pelo default (`app/chamados/models.py:1754`); sem DP próprio. P: `app/chamados/services/acoes_disputa.py:189`. T: `app/chamados/tests/test_fio_ancora.py:74`; `app/chamados/tests/test_fio_ancora.py:92`; `app/chamados/tests/test_fio_ancora.py:93`; `app/chamados/tests/test_fio_ancora.py:104`; `app/chamados/tests/test_fio_ancora.py:151`; `app/chamados/tests/test_3b_reuso_1061.py:30`.

- `app/chamados/models.py:2822` `PerguntaDisputa._nascer` -- insere identidade, dia, instância, célula,

[truncado em 20000 de 75733 caracteres]

_3113 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
