# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 12:03** · publicado em **12/11/2026 12:04** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 12:03 · sessao d57e2b6a2b71 · - · - · origem=Ronald (tmux)

Contagem por expressão, sem testes/snapshots: **8 interpolações de data = 4 `.format` + 4 `.replace`; 7 locais lógicos**, porque `query.py:64,66` são os dois ramos da mesma função. Há ainda formatação administrativa sem data e cópia de catálogo para âncora (`app/chamados/services/acoes_disputa.py:265`; `app/chamados/services/disputa_emissao.py:1374`).

| Sítio arquivo:linha | O que faz | Observação / fonte da data |
|---|---|---|
| `app/chamados/catalogo/query.py:64` | `replace('{data}', ISO convertido para dd/mm/aaaa)` | Usa **contexto da disputa por motivo**, `contexto_disputa[motivo]['data']`, lido em `:58-59`; não consulta marco absoluto. Chamado no emissor em `disputa_emissao.py:345,354`. |
| `app/chamados/catalogo/query.py:66` | Ramo de exceção: `replace('{data}', data_str)` | Usa a mesma string crua do contexto; não valida turno/dia civil nesse ramo (`query.py:58-66`). |
| `app/chamados/services/disputa_emissao.py:879` | Formata texto de cada marco faltante | `dia` vem da tupla retornada por `_marcos_faltantes` (`:859-861,873`) ou `faltantes` recebido; exige `dia == data_evento` (`:874`). A tupla é ancorada na **data da célula**, inclusive a véspera, em `declaracao_texto.py:109,139`. |
| `app/chamados/services/disputa_emissao.py:1151` | Formata pergunta de intervalo a partir do chamado | `_de = date.fromisoformat(_m['data'])`, onde `_m` vem de `contexto_json.marcos_faltantes` (`:1143-1148`). Um produtor demonstrável põe ali o **dia civil do marco encadeado**: `app/ponto/management/commands/detectar_intervalo_ausente.py:77-84`. O emissor passa esse valor a `dia_do_turno_da_celula` e à criação (`disputa_emissao.py:1152-1155`); origem efetiva de cada registro de produção `[nao verificado]`. |
| `app/chamados/services/disputa_emissao.py:1208` | Formata pergunta do grupo com batida de origem | Usa `data_evento = resolver_data_evento(...batida_origem...)` (`:1205`). O resolvedor retorna **dia do turno da batida**, sem somar o dia civil da saída (`app/escala/servico_jornada.py:495-509`). |
| `app/chamados/services/disputa_emissao.py:1246` | Formata pergunta sem batida de origem | Usa `data_evento = _ddc(ch.contexto_json)` (`:1232`; import `:1042`). `_ddc` tenta `data_turno`, `data_batida`, `entrada_timestamp`, `pendentes`, `horario_previsto`: portanto aceita fontes com nomes e bases diferentes (`app/chamados/data_evento.py:16-39`). |
| `app/chamados/services/declaracao_texto.py:374` | Formata pergunta criada na declaração do colaborador | `dia` vem do marco faltante casado por `_casar_hora` (`:341-345`); `_marcos_faltantes` devolve data da célula em `:109,139`. Persiste pelo `abrir` em `:376-377`. |
| `app/chamados/services/declaracao_texto.py:664` | Formata pergunta criada ao aplicar declaração textual | Usa `p['dia']`, produzido por `propostas_do_chamado`: grade → `_marcos_faltantes` (`:615`) → `_casar_hora` (`:625-628`) → dict (`:635`); persiste em `:666-667`. |
| `app/chamados/services/acoes_disputa.py:265` | Formata confirmação de reabertura administrativa com `% label_motivo` | **Sem data no texto**; nasce como conversa (`:262-268`), cujo `data_evento` é `localdate()` (`app/chamados/models.py:2795-2803`). |
| `app/chamados/services/disputa_emissao.py:1374` | Copia pergunta de catálogo da âncora ou literal de fallback | **Sem data conhecida no texto**; cria conversa `ancora_ausente`, tipo `data_hora` (`:1369-1377`). |
| `app/chamados/services/disputa_emissao.py:440,447` | Entrega `texto` ao nascimento, por `abrir_conversa` ou `abrir` | Texto vem de `texto_pergunta_com_data` (`:345,354`); data passada ao nascimento pode ser derivada depois por outro caminho (`:371-424,451`). Logo texto e `data_evento` não são recalculados juntos nesse ponto. |
| `app/chamados/services/disputa_emissao.py:840` | Repasse de `pergunta` por `_criar_pergunta_se_couber` | Não reformata; encaminha texto e data recebidos ao `abrir` (`:823-845`). |
| `app/chamados/models.py:3015` e `:3018` | Instancia com `**campos` e salva inserção | Persistência comum dos textos recebidos; `abrir` e `abrir_conversa` chegam via `_sob_contrato/_nascer` (`:2790-2803,2820-2828`). O reuso retorna a existente sem reescrever texto (`:2998-3009`). |
| `app/chamados/catalogo/perguntas.py:49` e `app/chamados/models.py:3566` | Literais do template “Confirme o horário da saída no dia {data}” | São **fontes do template**, não formatação de `data_evento`. A referência raspada `models.py:3564-3565` não corresponde a um formatador nesta cópia (`models.py:3564-3566`). |

**Leitores diretos do campo persistido: 20 ocorrências = 14 Python + 6 templates.** Se excluir `__str__` e o comando smoke, restam 18; isso é um recorte possível, não a contagem de todos os leitores. Evidências nas vinte linhas abaixo.

| Sítio arquivo:linha | O que faz | Observação |
|---|---|---|
| `app/api/views.py:1155` | API de chamados fornece `p.pergunta` | Texto cru; data é fornecida separadamente em `:1153-1154`. |
| `app/api/views.py:2551` | API das perguntas do app, `_item` | Texto cru para pendentes/histórico (`:2519-2559`). |
| `app/api/views_mensageria.py:564` | API da fila de validação | Texto cru; `data_evento` separado em `:568`. |
| `app/ponto/services/dia_do_colab.py:69` | Consulta do dia entrega pergunta | Texto cru; seleciona pelo `data_evento=data` (`:64-71`). |
| `app/chamados/views_cobrar.py:291` | Amostra da validação em lote | Texto cru no dict de saída. |
| `app/chamados/canal.py:81` | Perguntas mudas com resposta no fio | Texto cru; correlaciona fio por colaborador e `data_evento` (`:71-84`). |
| `app/chamados/services/explicador.py:71` | Monta dict explicativo | Mantém texto cru; calcula estado/ação em outros campos (`:68-79`). |
| `app/chamados/services/disputa_emissao.py:1549` | Copia pergunta para nota de resposta | Envolve o texto cru com tag e resposta; salva ocorrência no fio (`:1555-1556`). |
| `app/chamados/services/validacao.py:599` | Copia pergunta para nota de recusa | Envolve texto cru com autor, resposta anterior e motivo (`:598-600`). |
| `app/chamados/templatetags/chamados_extras.py:11` | Detecta “pergunta cega” | Lê texto cru com regex de data; não o recompõe. |
| `app/chamados/management/commands/auditar_disputas.py:74` | Auditor procura data no texto | Regex no campo cru (`:69-83`). |
| `app/chamados/management/commands/curar_perguntas_em_limbo.py:33` | Procura nota correspondente | Usa texto cru como substring `Pergunta: ...`; não recompõe. |
| `app/chamados/management/commands/smoke_chamados.py:108` | Verifica placeholder residual | Lê pergunta persistida e acusa `{data}`; o `p_cat.pergunta` de `:97` é catálogo, não campo persistido (`:94-108`). |
| `app/chamados/models.py:3547` | `__str__` da pergunta | Prefixa motivo e corta texto cru em 60 caracteres. |
| `app/templates/chamados/partials/_card_atendimento.html:93` | Cartão de resposta do colaborador | Exibe `p.pergunta` cru; sem projeção de marco. |
| `app/templates/chamados/partials/_card_atendimento.html:175` | Cartão de pergunta no histórico | Exibe `p.pergunta` cru. |
| `app/templates/chamados/partials/_pergunta_card.html:28` | Cartão de supervisão/Central | Exibe `perg.pergunta` cru. |
| `app/templates/chamados/partials/modal_fio.html:160` | Pergunta no modal do fio | Exibe texto cru; data do fato é mostrada em bloco separado (`:167`). |
| `app/templates/chamados/partials/_lote_preview.html:24` | Prévia de lote | Campo cru com `truncatechars:60`; data em coluna separada. |
| `app/templates/colaboradores/partials/painel_colaborador.html:197` | Painel do colaborador, bloco “no app” | Exibe `p.pergunta` cru. |
| `app/templates/chamados/partials/modal_fio.html:439` | **Propagação indireta:** dict do explicador | Exibe texto copiado com truncamento, além do dia separado; origem direta em `explicador.py:71`. Não entra nas 20 leituras diretas. |
| `app/chamados/views_cobrar.py:186` | **Propagação indireta:** dict do canal | Repassa texto cru já copiado por `canal.py:81`; relação demonstrada em `views_cobrar.py:180-187`. |
| `mensageria/nucleo/ferramentas.py:119` | **Propagação indireta:** item recebido da API | Copia `i.get('pergunta')` para avaliação; não recompõe. |
| `app/relatorios/pdf_chamados.py:145` e `:147` | **PDF indireto:** imprime `fup.conteudo` | Pode carregar as cópias da pergunta feitas em `disputa_emissao.py:1549-1556` e `validacao.py:598-600`; corta conteúdo em 1.000 caracteres. Não lê `PerguntaDisputa.pergunta` diretamente. |
| `app/chamados/catalogo/query.py:45` | Interpolador existente de catálogo | Não é projeção universal dos leitores: recebe motivo/contexto e lê **catálogo** (`:52-66`); chamados encontrados no emissor `disputa_emissao.py:345,354` e auditor `auditar_disputas.py:191`. Os leitores diretos acima acessam o campo. |

Não apareceu escritor de `p.pergunta = ...` nem `update(pergunta=...)` fora dos testes na varredura da cópia; a prova positiva disponível é a cadeia de inserção por `**campos` em `app/chamados/models.py:3015-3018`; ausência absoluta por mecanismos dinâmicos `[nao verificado]`. PWA com renderizador próprio de `PerguntaDisputa.pergunta` não foi demonstrado; os pontos de app comprovados são APIs e templates citados, e a execução efetiva do cliente em produção é `[nao verificado]`.

## 12/11 12:03 · sessao 84c8983eaadc · - · - · origem=Ronald (tmux)

Censo somente leitura. Não encontrei `AGENTS.md` na árvore nem nos ancestrais pesquisados; regra adicional de arquivo: `[nao verificado]`.

**3. VIA_FATO_OCUPADO**

| sítio arquivo:linha | o que faz | observação |
|---|---|---|
| `app/chamados/models.py:2488` | Declara `VIA_FATO_OCUPADO='fato_ocupado'`. | Definição, não escrita de registro. |
| `app/chamados/services/materializacao.py:1095`, `:1097`, `:1099`, `:1105` | **Único chamador ativo encontrado que grava essa via**: relê carimbo/via/recusa, traduz texto, decide `_carimba = materializada_em is None and codigo == 'COLISAO'`, chama `carimbar_identidade`. | Não consulta motivo/marco/lâmpada da pergunta. |
| `app/chamados/catalogo/recusa.py:66`, `:77` | Tradutor classifica `COLISAO` quando o texto contém `colisao tipo-oposto`, sem distinguir maiúsculas. | `any(m in baixo...)` em `:81-83`; classificação textual. |
| `app/chamados/models.py:3179`, `:3186`, `:3187`, `:3189`, `:3191`; `:3098` | Porta valida a família, grava `materializada_em`, `materializada_por`, `via_resolucao`, e `recusa_motivo` quando fornecido. | Não pergunta ao C6 (`:3170-3177`). Não limpa resposta nem validação. |
| `app/ponto/registro_batida.py:138`, `:139`, `:145`, `:150` | Origem da mensagem: existe batida oposta não retratada na janela inclusiva `timestamp±g_op`, e a candidata **não** casa com marco previsto do próprio tipo; sem autoridade humana retorna recusa. | **Não é somente vizinhança**: há exceção por geometria prevista. Entretanto não consulta **o marco cobrado pela PerguntaDisputa**, nem a sua lâmpada. A assinatura recebe colaborador, timestamp, tipo, origem (`:94`). |
| `app/ponto/registro_batida.py:21` | `g_op=300s` quando há escala e marcos no dia civil local da timestamp; senão `60s`. | Código `:28-32`; o texto do catálogo fala genericamente “menos de 5 minutos” (`recusa.py:67`). |
| `app/ponto/registro_batida.py:35` | `_casa_marco_proprio`: verifica dia local e véspera; considera algum marco previsto do mesmo tipo a distância `<=5min`. | `:42-49`; usa `escala_vigente` e `marcos_turno`, sem PerguntaDisputa. |
| `app/chamados/models.py:2511` | Inclui a via em `VIAS_DE_IDENTIDADE`. | Leitores derivados na tabela 4. |
| `app/chamados/models.py:2535` | Choice/descrição da via. | Disponível a `get_via_resolucao_display`. |
| `app/chamados/models.py:3738` | Inclui em `VIAS_SEM_TESTEMUNHA`. | Família separada da identidade; retirar de uma não retira da outra. |
| `app/chamados/juizes.py:679`, `:689` | Juiz do lastro reconhece a família sem testemunha e absolve sempre as identidades carimbadas. | Se sair só da identidade, continua no ramo sem testemunha, mas passa por janela/classificação de falta (`:690-705`). |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:47`, `:48`, `:460`, `:746` | Família sem testemunha alimenta piso I6 e sua apresentação. | Fato ocupado permanece nesse piso se apenas a identidade mudar. |
| `app/templates/chamados/partials/_card_atendimento.html:186` | Caso literal `fato_ocupado` ou `paridade_recusada`: “ja havia batida registrada.” | Leitor de apresentação explícito. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40`, `:42` | Só sobrescreve vias `('', 'legado_sem_lastro', None)`. | Fato ocupado é excluído pelo filtro genérico, embora citado apenas no comentário `:36`. |

Migrações `app/chamados/migrations/0040_a27_recusa_motivo.py:21`, `0041_b2_via_legado_sem_lastro.py:16`, `0042_hx_via_ausencia_dia.py:16`, `0046_via_celula_absolveu.py:16`, `0048_via_competencia_trancada.py:15`, `0051_decisao_pergunta.py:111` preservam o valor em **choices**, não constituem novos escritores desse valor em registros.

**4. VIAS_DE_IDENTIDADE — todos os consumidores ativos diretos e pelas duas funções da fonte**

| sítio arquivo:linha | o que faz | observação / efeito se fato ocupado sair apenas da tupla |
|---|---|---|
| `app/chamados/models.py:2511` | Fonte: admin, duplicata, fato ocupado, paridade recusada, motivo retratado. | Passa de cinco para quatro valores. |
| `app/chamados/models.py:3090` | `vias_de_identidade()` devolve a própria tupla. | Deixa de devolver fato ocupado. |
| `app/chamados/models.py:3096` | `vias_que_encerram_por_identidade()` acrescenta competência trancada. | Deixa de devolver fato ocupado; conserva tranca. |
| `app/chamados/models.py:3119` | `carimbar` recusa famílias que pertencem à outra porta. | Fato ocupado passa a poder entrar nesta porta e será submetido ao C6 (`:3140`). |
| `app/chamados/models.py:3179` | `carimbar_identidade` exige pertencimento. | **Chamador atual de `materializacao.py:1105` passa a levantar `ValueError`** (`:3180`) antes de gravar. |
| `app/chamados/models.py:3348`, `:3351`, `:3362` | `q_vivas` faz pré-filtro SQL: identidade materializada exclui perguntas sem resposta viva. | Fato ocupado deixa de excluir por identidade; validação, resposta e veredito da célula ainda condicionam o resultado (`:3349-3363`). |
| `app/chamados/juizes.py:689`, `:691` | `_pergunta_materializada_de_fato`: identidades carimbadas absolvem sem reclassificar o dia. | Fato ocupado deixa de absolver sempre. Continua em `VIAS_SEM_TESTEMUNHA` (`models.py:3738`): pode absolver por ausência de dia, fora da janela, falta não acusada ou exceção (`juizes.py:690-705`); não passa automaticamente a exigir a lâmpada específica. |
| `app/chamados/juizes.py:1427`, `:1428` | C6 mata identidade materializada no degrau posterior à resposta. | Fato ocupado segue para lâmpada (`:1434-1436`), veredito da célula (`:1438-1453`), lei do dia (`:1454-1462`) e lastro (`:1472-1479`). **Validada ainda morre antes de tudo** (`:1391-1392`); resposta não vazia também pode retornar antes (`:1407-1419`). |
| `app/chamados/services/relavra_via.py:181` | `q_carimbo_desfazivel` exclui identidades materializadas. | Fato ocupado torna-se elegível; só cai se a linha relida passar no filtro e o C6 a considerar viva (`:236-246`). Validada permanece morta no C6. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:195`, `:206` | `_contraprova_f1t` rotula o degrau pelo qual morreu. | Não classifica mais como “via de identidade”; se ainda morta e não validada, rotula “carimbo/celula”. Se viva, vira alarme (`:202-210`). |

São **quatro acessos diretos à tupla** (`models.py:3090`, `:3096`; `relavra_via.py:181`; auditor `:195`) e **cinco consumidores operacionais das funções de acesso** (`models.py:3119`, `:3179`, `:3348`; `juizes.py:689`, `:1427`). A própria definição está em `models.py:2511`.

**5. fato_do_marco_no_espelho**

| sítio arquivo:linha | o que faz | observação |
|---|---|---|
| `app/chamados/juizes.py:922` | Universo de motivos: `saida_sem_entrada→(E,hi)`, `orfao_14h→(S,hf)`, `intervalo_saida→(S,hii)`, `intervalo_volta→(E,hfi)`. | Outro motivo resulta `None` (`:941-943`). |
| `app/chamados/juizes.py:944`, `:948` | Usa exclusivamente `p.celula`, `cel.dna.marcos` e `cel.ata.lampadas`. | Não consulta banco diretamente, resposta, `data_evento`, timestamp, `veredito`, `_abs_marcos` ou batidas. FK pode ser carregada pelo ORM ao acessar `p.celula`. |
| `app/chamados/juizes.py:956`, `:960` | Para intervalo, seleciona todas as chaves existentes da mesma posição dos pares de pausa. | Pergunta de intervalo cobra AND das pausas, sem identificar uma única instância de pausa. |
| `app/chamados/juizes.py:962`, `:965`, `:968`, `:969`, `:971` | Faz lookup das lâmpadas por `(hora,tipo)`; `None` sem horas ou se faltar alguma lâmpada; retorna `all(bool(acesa))`. | `acesa=None` numa lâmpada presente vira **False**, porque usa `bool`, não `None`. Não valida identidade/data da célula contra a pergunta. |
| `app/chamados/models.py:1504` | `_universo_da_ressuscitacao` não ressuscita marco já aceso. | Usa cópia com a célula do julgamento em curso (`:1502`). |
| `app/chamados/services/materializacao.py:906`, `:907` | Guarda de **paridade** só carimba a recusa quando o marco está aceso. | `is not True` vai à recusa pendente; essa guarda não cobre COLISAO (`:919-930`, `:1099-1106`). |
| `app/chamados/juizes.py:1334` | `_resposta_concorda_com_a_luz` exige marco aceso antes de comparar resposta/luz e coerência do dia. | C6 o chama em `:1419`. |
| `app/chamados/juizes.py:1435` | C6: com materialização e marco aceso, mata antes do veredito do dia. | Chamado só depois dos degraus anteriores. |
| `app/chamados/juizes.py:1452` | C6: célula que não acusa com marcos pendentes ainda pode absolver se o marco da pergunta acendeu. | Segundo sítio de chamada dentro de `_pergunta_viva_calc`. |
| `app/chamados/reconciliador.py:1085` | `veredito_orfa` exige marco aceso; não trata resposta com texto (`:1083`) e depois exige C6 vivo e teto encerrado (`:1087-1097`). | Consumidor de órfãs. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:78` | Mede True/False/None das fantasmas e só considera luz False com C6 vivo (`:83`). | Recorte específico `VIA_FANTASMA`, materializada, não validada (`:67-68`). |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:43` | Seleciona perguntas materializadas não validadas e sem história para recarimbar marco aceso. | Via permitida em `:40-42`. |
| `app/ponto/services/veredito_celula.py:47`, `:56`, `:66`, `:67` | `_abs_marcos` encadeia entrada, pausas e saída, acrescentando dias enquanto o instante não for posterior ao anterior. | Entrada 19h/saída 7h produz S em D+1 pela regra `:67-68`. |
| `app/ponto/services/veredito_celula.py:251`, `:252`, `:275`, `:280` | Veredito usa cadeia absoluta, indexa por `(tipo,HH:MM)` e cria candidatos no instante absoluto faltante. | Instante D+1 preservado no candidato (`:282`, `:291`). |
| `app/ponto/services/cartorio.py:620`, `:648`, `:656` | Ata representa lâmpadas com chaves do DNA e casamento por tipo/hora. | Isso permite ao juiz acima ler `hf='07:00'` na célula do turno D, mesmo sendo saída D+1. |

Contagem das chamadas ativas do juiz: **8 sítios em 7 funções, 6 arquivos**: `models.py:1504`; `materializacao.py:907`; `juizes.py:1334`, `:1435`, `:1452`; `reconciliador.py:1085`; comandos `reabrir_orfas_enterradas.py:78`, `encerrar_perguntas_com_fato.py:43`.

Conclusão demonstrável para noturna: o juiz consegue responder pela **lâmpada de saída `hf` da célula do turno**, inclusive quando a cadeia absoluta a coloca em D+1; ele não recalcula nem confere o dia civil do marco (`juizes.py:941-971`; `veredito_celula.py:66-71`). A lâmpada e a célula corretas nos casos de produção 51467/56641: `[nao verificado]`.

## 12/11 12:01 · sessao 9a4a3a589e82 · MAPA-DO-FIO-MORTO · censo · origem=Code

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **d — SEM_ACAO** | 241 (`recorte.out:50`) | **Juiz:** `censo_alcance`: sem vivas ligadas à célula, colaborador na CxR e dia fora dela; rota `sem_porta`, papel `nenhuma porta` (`app/chamados/services/vigia_fio.py:610`, `app/chamados/services/vigia_fio.py:677`, `app/chamados/services/vigia_fio.py:699`). **Escritor:** `d` não é campo persistido; a retenção lê veredito/protestos, escritos por `lavrar_veredito` (`app/folha/export.py:1241`, `app/ponto/portas/celula.py:974`). Ausência de viva não identifica nascimento, morte ou escritor histórico [nao verificado]. | **E3:** “vigia_fio leem SO tarefa_do_dia”; **E4B:** “backfill do fio morto (matriz_e4 C3, classes a-e) pela porta canonica” (`app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1894`). Cobrem leitura/backfill; `tarefa_nada` não produz ato reparador no comando (`app/chamados/management/commands/backfill_fio_morto_e4b.py:117`). | **Decisão específica para retenção com tarefa vazia: não consta dos dois anexos da pergunta 2** (`avais_e3_leitores.txt:5`, `avais_e3_leitores.txt:18`; `censo.r`, seção 3 [nao verificado quanto à numeração física]). Que os **241** tenham tarefa vazia também não foi medido: `SEM_ACAO` é classificação da sonda, sem consulta a C19 (`sonda_mapa_fio.py:39`). |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **porta_fila — decidir — SEM_ACAO** | 15 (`recorte.out:36`) | **Rota:** C17 aceita chamado da célula na fila antes de exigir pergunta; censo copia rota e verbos (`app/chamados/juizes.py:1647`, `app/chamados/juizes.py:1657`, `app/chamados/services/vigia_fio.py:642`). **Papel:** `particionar_verbos → creditos_perguntas → verbo_do_chamado`; ausência de crédito pode resultar em `decidir` (`app/chamados/services/verbos.py:34`, `app/chamados/services/raiox.py:135`, `app/chamados/catalogo/natureza.py:61`). **Escritores:** status por `_gravar_estado`; mortes/carimbos pelas portas discriminadas na resposta 4 (`app/chamados/models.py:201`, `app/chamados/models.py:3153`). Autoria histórica individual [nao verificado]. | **E3:** “5 respondedores morrem com certidao” e leitores passam a C19 (`app/docs/BACKLOG.md:1873`). C19 já publica somente portas de ações exigíveis; fila residual não cria tarefa (`app/chamados/services/tarefa_do_dia.py:538`). **E4C/E4D:** “LEITORES DIVERGE + selo 2” / “parar projecao” (`app/docs/BACKLOG.md:1885`). | Os **15 ficam fora do comando E4B**, cujo recorte exige `sem_porta` (`app/chamados/management/commands/backfill_fio_morto_e4b.py:79`). A migração E3 não demonstra a ação C19 nem a cura histórica de cada dia; isso não aparece no recorte [nao verificado]. Cobertura adicional específica por outro PRONTO [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **porta_fila — cobrar — SEM_ACAO** | 6 (`recorte.out:30`) | **Rota:** chamado da célula em `qs_na_fila`; **papel:** crédito de pergunta sem resposta resulta em `cobrar` (`app/chamados/juizes.py:1647`, `app/chamados/services/raiox.py:140`, `app/chamados/catalogo/natureza.py:50`, `app/chamados/catalogo/natureza.py:60`). Crédito usa `q_vivas`/lavra; app acrescenta C6 e disputa aberta: são predicados diferentes (`app/chamados/services/raiox.py:74`, `app/chamados/services/raiox.py:135`, `app/chamados/services/pergunta_no_app.py:58`, `app/chamados/services/pergunta_no_app.py:71`). **Escritores:** nascimento `_nascer`, resposta/reabertura e estado do chamado (`app/chamados/models.py:3015`, `app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/models.py:228`). | **E3:** “fio, tela admin, API/app, badge/PWA […] leem SO tarefa_do_dia” (`app/docs/BACKLOG.md:1873`). **RAIZ:** “elo raiz<-filhos […] DisputaSupervisao nao reage” cobre a relação estrutural, sem identificar estes seis (`app/docs/BACKLOG.md:1904`). | **E4B exclui estes seis `porta_fila`** (`app/chamados/management/commands/backfill_fio_morto_e4b.py:79`). `app=0` não revela qual guarda excluiu cada pergunta; atribuir os seis a disputa fechada, identidade ou outro motivo é [nao verificado]. PRONTO específico que decida cada um desses estados [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **a — registrado — SEM_ACAO** | 2 (`recorte.out:47`) | **Juiz:** `censo_alcance` encontra chamado vivo fora da fila e publica o status como detalhe (`app/chamados/services/vigia_fio.py:664`, `app/chamados/services/vigia_fio.py:669`). `registrado` pertence a VIVOS, mas não a COBRANCA (`app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`). **Escritores:** nascimento `abrir/criar`, decidido pelo motor; mudanças por `_gravar_estado` (`app/chamados/models.py:934`, `app/chamados/models.py:972`, `app/chamados/models.py:1061`, `app/chamados/models.py:1096`, `app/chamados/models.py:228`). | **E4B:** “classes a-e”; **E3:** “vigia_fio leem SO tarefa_do_dia” (`app/docs/BACKLOG.md:1894`, `app/docs/BACKLOG.md:1873`). **E1:** “censo de toda porta do fato” cobre a estrutura das entradas, sem provar defeito nestes registros (`app/docs/BACKLOG.md:1859`). | O PRONTO não transforma `registrado` em ação exigível; o catálogo declara registro sem cobrança (`app/chamados/catalogo/motor.py:25`). A ação C19 e o ato E4B aplicável aos dois não foram fornecidos [nao verificado]. Eventual lacuna além desse escopo [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **e — pergunta_viva_sob_chamado_terminal — SEM_ACAO** | 2 (`recorte.out:52`) | **Juiz:** após as causas anteriores, viva com âncora terminal recebe esse detalhe (`app/chamados/services/vigia_fio.py:675`, `app/chamados/services/vigia_fio.py:687`). Âncora é destino, senão chamado da disputa (`app/chamados/services/fio.py:97`, `app/chamados/services/fio.py:117`). **Escritores:** estado terminal por `_gravar_estado`; vínculo por `_nascer/reapontar_resolvedora` (`app/chamados/models.py:228`, `app/chamados/models.py:3015`, `app/chamados/services/fio.py:880`). Sequência histórica concreta [nao verificado]. | **E4B:** “classes a-e”; cura de credor sem resposta existe em `reabrir_credor_de_pergunta_viva` (`app/docs/BACKLOG.md:1894`, `app/chamados/services/disputa_emissao.py:1805`). **E3:** leitura única; **RAIZ:** “elo raiz<-filhos” (`app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1904`). | A cura do credor possui vetos de cascata, exportação, limbo e reabertura; não promete tornar a pergunta pendente no app (`app/chamados/services/disputa_emissao.py:1835`, `app/chamados/services/disputa_emissao.py:1839`, `app/chamados/services/disputa_emissao.py:1848`). Motivo de `app=0` e cobertura individual desses vetos [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **e — sem_pergunta_viva — SEM_ACAO** | 1 (`recorte.out:53`) | **Juiz:** sem vivas e sem enquadramento nas causas anteriores recebe esse detalhe (`app/chamados/services/vigia_fio.py:677`, `app/chamados/services/vigia_fio.py:685`). **Escritor compatível com a via raspada `fato_ocupado`:** recusa COLISAO do materializador → `carimbar_identidade` (`app/chamados/services/materializacao.py:1097`, `app/chamados/services/materializacao.py:1105`, `app/chamados/models.py:3191`). A via não prova autoria histórica [nao verificado]. | **E4B:** “classes a-e”; **E3:** leitura única; **E4C/E4D:** “LEITORES DIVERGE + selo 2” / “parar projecao” (`app/docs/BACKLOG.md:1894`, `app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1885`). | A via de identidade pode encerrar a pergunta sem resolver a exigibilidade do dia; C6 e C19 fazem leituras distintas (`app/chamados/juizes.py:1428`, `app/chamados/services/tarefa_do_dia.py:230`). Ação necessária, C19 concreto e cura desse dia não constam da medida [nao verificado]. Cobertura contratual adicional específica [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **e — COLAB_RESPONDE** | 48 (`recorte.out:51`) | **Juiz da causa:** viva sob âncora terminal (`app/chamados/services/vigia_fio.py:687`). **Juiz da ação da sonda:** existe pendência do app no mesmo `data_evento`; não significa reparação completa do dia (`sonda_mapa_fio.py:27`, `sonda_mapa_fio.py:41`). **Escritores:** estado terminal `_gravar_estado`; nascimento/costura da pergunta `_nascer/reapontar_resolvedora` (`app/chamados/models.py:228`, `app/chamados/models.py:3015`, `app/chamados/services/fio.py:880`). | **E4B:** “backfill do fio morto […] classes a-e”; reabre credor de pergunta viva sem resposta sob âncora fora de VIVOS (`app/docs/BACKLOG.md:1894`, `app/chamados/services/disputa_emissao.py:1821`, `app/chamados/services/disputa_emissao.py:1827`). **E3:** leitores únicos; **RAIZ:** “elo raiz<-filhos” (`app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1904`). | **Cobertura parcial:** a cascata pode impedir a reabertura; o dado não contém os insumos dos vetos (`app/chamados/services/disputa_emissao.py:1835`, `app/chamados/services/disputa_emissao.py:1839`, `app/chamados/services/disputa_emissao.py:1848`). Não há prova de 48 curas nem de que a pergunta contada em `app` seja a mesma que determinou a causa [nao verificado]. Lacuna contratual adicional [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **c — ADMIN_VALIDA** | 15 (`recorte.out:48`) | **Juiz:** viva respondida sob âncora terminal; validação ausente decorre de C6 (`app/chamados/services/vigia_fio.py:671`, `app/chamados/services/vigia_fio.py:675`, `app/chamados/juizes.py:1391`). **Escritores:** resposta por `responder_pergunta_disputa_core`, automática, trilha ou complemento de `carimbar`; status por `_gravar_estado` (`app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1653`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/models.py:3133`, `app/chamados/models.py:228`). | **E4B:** “classes a-e”; **E3:** “tela admin […] leem SO tarefa_do_dia”; **RAIZ:** reação raiz/filhos (`app/docs/BACKLOG.md:1894`, `app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1904`). **E4C/E4D:** leitores/escritores da projeção (`app/docs/BACKLOG.md:1885`). | **A cura de reabrir credor exclui respondidas**, portanto não alcança diretamente a pergunta que caracteriza `c` (`app/chamados/services/disputa_emissao.py:1821`). Outros atos E4B dependem da resposta C19, ausente na medida (`app/chamados/management/commands/backfill_fio_morto_e4b.py:87`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:98`). PRONTO específico garantindo a passagem desses 15 a uma lista de validação [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **d — COLAB_RESPONDE** | 2 (`recorte.out:49`) | **Causa:** ausência de vivas por **FK célula** e dia fora da CxR (`app/chamados/services/vigia_fio.py:610`, `app/chamados/services/vigia_fio.py:677`). **Ação:** pendência por **data_evento**, em outro universo; app não exclui conversas (`sonda_mapa_fio.py:27`, `sonda_mapa_fio.py:30`, `app/chamados/services/pergunta_no_app.py:49`). **Escritores dos vínculos:** `_nascer` associa célula do fato; `rechavear_data_evento` muda dia/célula (`app/chamados/models.py:2897`, `app/chamados/models.py:3015`, `app/chamados/services/fio.py:1038`). | **E3:** API/app e vigia leem C19; **E1:** “censo de toda porta do fato”; **E4B:** “classes a-e” (`app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1859`, `app/docs/BACKLOG.md:1894`). | A diferença de universos não prova erro de identidade, célula ou C6. Qual pergunta gera `app=1`, sua natureza e seu vínculo com a célula são [nao verificado]. Cobertura individual ou lacuna adicional não podem ser atribuídas pelos agregados [nao verificado]. |

**2. NAO.** O brief E3 fornece o procedimento V/T/X e exige decisões no censo; não escreve o que C19 deve mandar — nem que deva mandar nada — para o estado específico “folha retém e tarefa não manda nada”. O `censo.r` fornecido distingue nada/desconhecido/sem porta e conserva o universo, sem decidir esse estado (`avais_e3_leitores.txt:5`, `avais_e3_leitores.txt:18`; `censo.r`, seção 3, itens 4–5 [nao verificado quanto à numeração física]).

Os **241 são `SEM_ACAO` da sonda**, não uma medição de `tarefa_do_dia == nada`; essa classificação consulta validação, app, Resolver dia e rota (`recorte.out:50`, `sonda_mapa_fio.py:39`).

| 3. Tela candidata | View/template e função REAL que monta a lista | c — 15 | e — 48 |
|---|---|---|---|
| **Drawer administrativo — Mensagens** | `painel_colaborador` chama `projecao_do_app`; lista real: `perguntas_do_app → consulta_do_app → projecao_do_app`; template percorre pendências (`app/colaboradores/views.py:940`, `app/chamados/services/pergunta_no_app.py:45`, `app/chamados/services/pergunta_no_app.py:61`, `app/chamados/services/pergunta_no_app.py:78`, `app/templates/colaboradores/partials/painel_colaborador.html:193`). | **Exclui a pergunta respondida** de `pendentes`; ela vai ao histórico. Outra pendência do mesmo dia pode aparecer (`app/chamados/services/pergunta_no_app.py:71`, `app/chamados/services/pergunta_no_app.py:74`; ocorrência individual [nao verificado]). | **Inclui as pendências que produziram `app>0`**, sem filtro de status da âncora/silêncio (`sonda_mapa_fio.py:27`, `app/chamados/services/pergunta_no_app.py:71`). Mostra dia do turno para fatos; conversa recebe dia `None`. Identidade entre pendência do app e pergunta sob terminal [nao verificado] (`app/chamados/services/pergunta_no_app.py:106`). |
| **Modal administrativo do fio** | `modal_fio_colaborador → montar_chamados_fio(so_janela=False)`; template percorre cartões e perguntas (`app/chamados/views.py:889`, `app/chamados/services/fio.py:163`, `app/templates/chamados/partials/modal_fio.html:68`, `app/templates/chamados/partials/modal_fio.html:157`). | **O predicado admite:** todos os chamados do colaborador; perguntas de disputa aberta e vivas de disputa fechada. Terminal/silêncio não excluem (`app/chamados/services/fio.py:177`, `app/chamados/services/fio.py:221`, `app/chamados/services/fio.py:235`). Porém a enumeração exige disputa com chamado; esse FK não consta do recorte: inclusão individual [nao verificado] (`app/chamados/services/fio.py:184`). | Mesmo predicado: **admite pergunta sob terminal**, com a mesma ressalva do FK da disputa (`app/chamados/services/fio.py:184`, `app/chamados/services/fio.py:240`). Inclusão das 48 perguntas concretas [nao verificado]. |
| **Página administrativa Fio** | `fio_colaborador → montar_chamados_fio`; template renderiza cartões/followups (`app/chamados/views.py:677`, `app/chamados/services/fio.py:163`, `app/templates/chamados/fio_colaborador.html:153`, `app/templates/chamados/fio_colaborador.html:237`). | Inclui o **chamado terminal**, pois o carregamento filtra somente colaborador; esse template não possui a lista de perguntas do modal. Ver chamado não comprova ver a pergunta (`app/chamados/services/fio.py:177`, `app/templates/chamados/fio_colaborador.html:161`). | Mesmo comportamento (`app/chamados/services/fio.py:177`, `app/templates/chamados/fio_colaborador.html:161`). |
| **Central/painel de gestão** | View `painel_gestao → montar_contexto_painel`; templates `painel_gestao.html` e `_lista_chamados.html` (`app/chamados/views.py:727`, `app/chamados/views.py:740`, `app/chamados/views.py:767`, `app/chamados/services/painel.py:198`). | **Exclui a âncora terminal do padrão:** exige COBRANCA. Filtro explícito pelo status admite o chamado; “histórico” cobre resolvido/fechado, sem provar qual status ocorreu aqui (`app/chamados/services/painel.py:302`, `app/chamados/services/painel.py:306`, `app/chamados/services/painel.py:316`; status individual [nao verificado]). | Mesmo predicado: **fora da lista padrão**, acessível pelo filtro explícito correspondente (`app/chamados/services/painel.py:306`, `app/chamados/services/painel.py:316`). |
| **Pendências de disputa** | `pendencias_disputa → relatorios_disputa.coletar`; template de pendências (`app/colaboradores/views.py:2998`, `app/colaboradores/views.py:3001`, `app/chamados/relatorios_disputa.py:26`, `app/templates/colaboradores/partials/pendencias_disputa.html:6`). | **Condicional:** `coletar` exige disputa aberta; o recorte não fornece seu fechamento. Categoria depende de todas as vivas da disputa (`app/chamados/relatorios_disputa.py:18`, `app/chamados/relatorios_disputa.py:40`). Inclusão individual [nao verificado]. | A disputa da **pendência do app** entra, pois o app exige disputa aberta. A tela mostra pessoa/disputa, não cada dia/pergunta (`app/chamados/services/pergunta_no_app.py:71`, `app/chamados/relatorios_disputa.py:40`, `app/templates/colaboradores/partials/pendencias_disputa.html:6`). Correspondência com a pergunta terminal [nao verificado]. |
| **Calendário do perfil** | `calendario_ponto → contexto_calendario`; grade `_calendario_grade.html` (`app/colaboradores/views.py:1949`, `app/colaboradores/services/calendario.py:159`, `app/templates/colaboradores/partials/_calendario_grade.html:67`). | **Inclui o dia na janela selecionada**, enumerada inclusive; não lista a pergunta. Ticket da âncora terminal fica fora da carga VIVOS; eventual pai vivo é outro vínculo (`app/colaboradores/services/calendario.py:437`, `app/colaboradores/services/calendario.py:447`, `app/colaboradores/services/calendario.py:603`, `app/colaboradores/services/calendario.py:721`). | Mesmo predicado: **dia visível na janela**, sem prova de exposição da pergunta terminal (`app/colaboradores/services/calendario.py:603`, `app/colaboradores/services/calendario.py:721`). |
| **Cadastro × Realidade** | View `cadastro_realidade → cadastro_realidade.lista`; ramo candidato C11 é `beco_em_lote` (`app/escala/views.py:476`, `app/escala/views.py:487`, `app/escala/services/cadastro_realidade.py:787`, `app/escala/services/cadastro_realidade.py:442`). | **Inclusão [nao verificado]:** C11 exige ativo, veredito `indefinida`, protesto específico, sem porta, viva sob terminal e folga não definida; o recorte não contém todos esses insumos (`app/escala/services/cadastro_realidade.py:477`, `app/escala/services/cadastro_realidade.py:488`, `app/escala/services/cadastro_realidade.py:515`, `app/escala/services/cadastro_realidade.py:518`). | Mesma insuficiência: `sem_porta` com terminal **não basta** para inclusão (`app/escala/services/cadastro_realidade.py:477`, `app/escala/services/cadastro_realidade.py:518`). |

| 4. Campo/estado | Quem ESCREVE | Quem LÊ | Mesmo significado e bordas |
|---|---|---|---|
| **`rota`** | Saída transitória de C17; censo copia ou atribui balde/`sem_porta`. C19 também produz sua própria rota, a partir das ações (`app/chamados/juizes.py:1657`, `app/chamados/services/vigia_fio.py:646`, `app/chamados/services/vigia_fio.py:699`, `app/chamados/services/tarefa_do_dia.py:553`). | Sonda; backfill seleciona `sem_porta`; folha lê C17 (`sonda_mapa_fio.py:32`, `sonda_mapa_fio.py:47`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:79`, `app/folha/fora_da_folha.py:67`). | **Não equivalentes:** C17 admite presença na fila; C19 exige porta de ação julgada. Não são campos persistidos da pergunta (`app/chamados/juizes.py:1647`, `app/chamados/services/tarefa_do_dia.py:541`, `app/chamados/services/tarefa_do_dia.py:546`). |
| **`causa`/`detalhe`** | `censo_alcance`, por precedência a→b→c→d→e; grava apenas a estrutura de retorno (`app/chamados/services/vigia_fio.py:667`, `app/chamados/services/vigia_fio.py:699`). | Sonda agrega somente `sem

[truncado em 20000 de 31536 caracteres]

## 12/11 12:00 · sessao 25cc736dbb89 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**Veredito geral: bloqueia declarar a c16 integralmente fechada.** A guarda do resumo foi corrigida e existe agora teste integrado com o recalculador real; porém a garantia literal “nunca turno aberto” sem célula continua sem preservação, e outro leitor da marca ainda elimina a data ao filtrar anomalias. (`app/relatorios/pdf_espelho.py:477`; `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:83`; `app/ponto/tests/test_lastro_c11_sem_banco.py:418`; `app/relatorios/pdf_espelho.py:675`)

**1. Bloqueios da preverif c14**

| Bloqueio | Veredito | O contraexemplo ainda reproduz? |
|---|---|---|
| Órfã 10/11 07:02 suprime entrada legítima 11/11 07:02 | **Fecha nesse ponto.** | **Não** no resumo nem na marca. A seleção restringe o HH:MM à linha recebida e guarda o timestamp completo; guarda e marca consultam esse mesmo conjunto. O teste exige uma abertura em 11/11, preservando a órfã de 10/11. (`app/relatorios/pdf_espelho.py:475`; `:477`; `:178`; `:516`; `:568`; `app/ponto/tests/test_lastro_c14_sem_banco.py:118`) |
| Prova integrada de (d), com recalculador substituído | **Fecha por inspeção; execução ORM [nao verificado].** | O mutante `return {}` já não sobreviveria: `_lido()` chama o recalculador real e exige o colaborador no retorno. O simulador também é chamado realmente. A prova antiga continua parcial, agora declaradamente. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:83`; `:85`; `:89`; `app/ponto/tests/test_lastro_c14_sem_banco.py:393`) |
| Selo sem célula enfraquece “nunca turno aberto” | **Fecha a comparação com a tela; bloqueia afirmar preservação da garantia literal.** | O comportamento sem ata permanece: marca ausente e um turno aberto no harness. O novo selo compara contadores, mas deixou de exigir zero; a documentação restringiu “nunca” ao caso **com ata**. O resultado da fixture ORM de 16:19 permanece **[nao verificado]**. (`app/ponto/tests/test_lastro_c11_sem_banco.py:416`; `:418`; `app/relatorios/tests/test_cartao_marca_espuria.py:8`; `:135`; `:155`) |

**Há uma sobrevivência da classe “perder a data”, fora da guarda corrigida:** o cabeçalho reúne HH:MM de todas as órfãs e remove qualquer anomalia contendo esses horários. Com órfã em **10/11 07:02**, também elimina **“Entrada sem saída em 11/11 07:02.”** O formato dessa mensagem vem do adaptador produtivo. Isso bloqueia considerar a classe completamente fechada no escopo solicitado; ocorrência efetiva em produção: **[nao verificado]**. (`app/relatorios/pdf_espelho.py:675`; `:678`; `app/ponto/adaptador_motor.py:40`)

**2. Teste c16 — fecha a costura; tem limites explícitos** [nao verificado]

As três funções são importadas diretamente e chamadas sem substituição por `Mock`: `julgar_celula`, `recalcular_fechamento_mes` e `instantaneo`. O congelamento substitui o relógio, não essas funções. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:31`; `:34`; `:35`; `:46`; `:79`; `:83`; `:89`)

O oráculo é **misto, com âncora independente suficiente para esse caso**:

- **Pela regra:** cadastro 08–17 com intervalo 12–13, quatro batidas na segunda e ausência aprovada na terça; esperados literais de **960 previstos, dois dias, 480 abonados e 480 realizados**. Não são capturados da saída para formar o esperado. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:56`; `:66`; `:73`; `:107`)
- **Por invariância:** depois da relavra, compara seis campos e o retrato do retido com o estado anterior. A contraprova retira os metadados e exige perda de 480 no previsto e abono zero. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:116`; `:123`; `:130`; `:139`)
- **Limite:** a ata antiga continua sintetizada da nova; isso não executa o cartório histórico. `ausencia_label` é cobrado por presença, sem assert de conteúdo. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:95`; `:101`)

**Mutantes produtivos que fariam falhar, por inspeção:**

| Mutante | Mordida |
|---|---|
| Recalculador retorna `{}` | Falha no `assertIn`. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:85`) |
| Cartório perde os metadados da ausência | Falha na presença/tipo/id da ata nova. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:95`; `:97`; `:98`) |
| Cálculo ignora `ausencia_tipo` | Falha na contraprova que exige diferença de 480. A regra produtiva consulta esse campo. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:139`; `app/ponto/services/dia_pago.py:392`) |
| Simulador consulta fechamento gravado para os retidos | Nesta fixture, sem fechamento gravado, desaparece a prontidão exigida. (`app/folha/management/commands/simular_folha.py:76`; `:85`; `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:125`; `:128`) |

O comentário “MESMO objeto” excede o demonstrado pelo teste: `_lido()` e `_foto()` fazem chamadas independentes. A implementação do simulador transporta os objetos da sua própria chamada ao recalculador. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:83`; `:89`; `:121`; `app/folha/management/commands/simular_folha.py:130`; `:152`)

**3. Selo cartão — fecha a comparação sem célula**

Compara **a mesma autoridade funcional**, consultada duas vezes: chama `espelho_do_colab` diretamente e chama o coletor PDF, que também consulta esse serviço, com o mesmo período e instante nesse cenário. Não compara a mesma instância de retorno. (`app/relatorios/tests/test_cartao_marca_espuria.py:75`; `:129`; `:132`; `app/relatorios/pdf_espelho.py:474`)

O contador esperado é lido diretamente do resumo da tela. Para a marca, o teste **deriva um booleano** por pertinência do HH:MM em `orfas_grade` daquele dia; portanto não lê um `batida_tela.orfa` pronto, mas também não reconstrói grade ou pareamento. (`app/relatorios/tests/test_cartao_marca_espuria.py:133`; `:140`; `:142`; `:144`)

**Fila:** isso prova a comparação de contador e marca sem célula, não a disposição visual completa. O positivo com ata verifica horário presente e ausência de “turno aberto”, mas não exige o asterisco nem a separação das marcações comuns. (`app/relatorios/tests/test_cartao_marca_espuria.py:109`; `:110`; `:112`; `:155`)

**4. Checklist das classes**

| Classe | Veredito |
|---|---|
| Segundo juiz duplicando autoridade | **Fecha quanto ao delta de classificação:** não reaparece grade própria no PDF; ele adapta `orfas_grade` recebido da tela. **Fila herdada:** PDF e tela continuam contando abertos com guardas diferentes. (`app/relatorios/pdf_espelho.py:474`; `:479`; `:516`; `app/ponto/services/espelho.py:436`) |
| Segundo escritor fora da porta | **Fecha no delta produtivo:** escreve somente atributo de apresentação `batida.orfa` e estruturas do retorno. A ata continua passando pela porta; os `update(ata=...)` novos são fixtures do teste. (`app/relatorios/pdf_espelho.py:568`; `app/ponto/portas/celula.py:1033`; `:1040`; `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:103`; `:137`) |
| Relógio fora da borda | **Fila herdada:** `agora` chega à tela, mas não a `folha_manda`, que consulta turno vivo e teto sem recebê-lo; ambos podem ler o relógio corrente. (`app/relatorios/pdf_espelho.py:474`; `:587`; `app/relatorios/cartao_pela_celula.py:398`; `:412`; `app/ponto/turnos.py:2290`; `app/relatorios/cartao_pela_celula.py:40`) |
| Derivação paralela de fonte única | **Bloqueia fechar a classe:** o filtro global de anomalias continua decidindo por coincidência textual de HH:MM. A contagem própria de abertos também permanece. (`app/relatorios/pdf_espelho.py:675`; `:679`; `:516`) |
| Selo/RED sem mordida | **Fecha para as operações novas examinadas:** há casos adversários de data, contador e marca, além da contraprova c16. Execução ORM e RED histórico: **[nao verificado]**. (`app/ponto/tests/test_lastro_c14_sem_banco.py:118`; `:129`; `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:130`) |
| Campo com dois significados | **Fecha a perda de data na guarda; fila nos leitores restantes:** o desenho ainda reduz marcas a HH:MM para suprimir alertas da linha. `orfa=False` continua podendo significar falta de informação disponível, pois ata antiga admite `?` e ausência de célula devolve vazio. (`app/relatorios/pdf_espelho.py:178`; `:919`; `:923`; `app/escala/services/leitor_celula.py:190`; `:390`; `:394`) |

**5. Escritores e leitores dos campos do delta e de suas fontes**

| Campo/valor | Quem escreve ou fornece | Quem lê |
|---|---|---|
| `orfas` da grade | Pareamento com marcos; publicação por dia e conservação das não consumidas. (`app/escala/utils.py:1725`; `:1782`; `:1835`; `:1858`) | Cartório e adaptador de apresentação. (`app/ponto/services/cartorio.py:736`; `app/escala/utils.py:1113`) |
| `ata.orfas` | Cartório monta; porta atribui e persiste. (`app/ponto/services/cartorio.py:736`; `app/ponto/portas/celula.py:1033`; `:1040`) | Leitor conserva horas ou devolve `?`, publicando `orfas`. (`app/escala/services/leitor_celula.py:187`; `:190`; `:420`) |
| `dias[].orfas_grade` | Adaptador → espelho; sem grade/marcos pode devolver vazio. (`app/escala/utils.py:1087`; `:1113`; `app/ponto/services/espelho.py:699`; `:854`) | HTML, mapa PDF e esperado do selo. (`app/templates/ponto/espelho.html:240`; `app/relatorios/pdf_espelho.py:475`; `app/relatorios/tests/test_cartao_marca_espuria.py:143`) |
| `dias`, `data`, `batidas` | Espelho publica linhas e batidas agrupadas pelo juiz. Reclamadas recebem dia do turno; restantes recebem dia civil. (`app/ponto/services/espelho.py:579`; `:663`; `:775`; `:776`; `app/ponto/turnos.py:2953`; `:2955`) | PDF indexa classificação, seleciona timestamps e recorta linhas. (`app/relatorios/pdf_espelho.py:475`; `:478`; `:562`) |
| `batida.timestamp` | Registro da batida grava o instante recebido. (`app/ponto/registro_batida.py:157`) | Adaptador de períodos, ponte de dia e PDF. (`app/ponto/adaptador_motor.py:37`; `app/ponto/services/espelho.py:458`; `app/relatorios/pdf_espelho.py:478`; `:568`) |
| `resultado.periodos`, `periodos_ft`; `periodo.entrada`, `turno_aberto` | Motor acrescenta períodos e marca aberto quando falta saída; entrada vem do timestamp transportado pelo adaptador. (`app/ponto/motor_calculo_v2.py:2492`; `:2494`; `:2751`; `:2753`; `app/ponto/adaptador_motor.py:37`) | Guarda PDF e conta da tela. (`app/relatorios/pdf_espelho.py:178`; `:515`; `:516`; `app/ponto/services/espelho.py:424`; `:436`) |
| `_orfas_por_data`, `_orfas_instantes` | PDF transforma o contrato recebido em mapa por linha e conjunto de instantes. (`app/relatorios/pdf_espelho.py:475`; `:477`) | Construção do conjunto, guarda e marca. (`app/relatorios/pdf_espelho.py:479`; `:178`; `:516`; `:568`) |
| `resumo.turnos_abertos` | PDF inicia e incrementa; tela conta separadamente. Na competência, folha pode sobrescrever com o fechamento. (`app/relatorios/pdf_espelho.py:253`; `:517`; `app/ponto/services/espelho.py:437`; `app/relatorios/cartao_pela_celula.py:112`; `:346`) | Retorno PDF, HTML e selo comparativo. (`app/relatorios/pdf_espelho.py:603`; `app/templates/ponto/espelho.html:90`; `app/relatorios/tests/test_cartao_marca_espuria.py:133`) |
| `_abertos_ts`, `dias[].turnos`, `datas_turno_aberto` | PDF guarda entradas contadas; cards vêm da ponte instante→pk→dia da tela; PDF publica datas das linhas correspondentes. (`app/relatorios/pdf_espelho.py:514`; `:518`; `:576`; `:578`; `app/ponto/services/espelho.py:461`; `:777`) | `folha_manda` e desenho dos alertas. (`app/relatorios/cartao_pela_celula.py:396`; `:418`; `app/relatorios/pdf_espelho.py:921`) |
| `batida.orfa` | PDF atribui em memória; calendário tem outro escritor transitório. (`app/relatorios/pdf_espelho.py:568`; `app/colaboradores/services/calendario.py:627`) | Filtro global de anomalias, separação, asterisco, alertas e legenda. (`app/relatorios/pdf_espelho.py:675`; `:835`; `:844`; `:919`; `:983`) |

**Ressalva de origem:** `orfas_grade` nem sempre vem da ata: antes de `apur_ini` e no tratamento de `AtaAgregadaSemMarcos`, a tela usa o builder. O PDF recebe esses caminhos pelo mesmo contrato da tela. (`app/ponto/services/espelho.py:493`; `:523`; `:536`; `:542`; `:565`)

## 12/11 11:57 · sessao b126c59da370 · - · - · origem=Ronald (tmux)

Censo do delta e dos produtores/leitores:

| Campo | Escritor/origem | Leitores |
|---|---|---|
| `ata.orfas` | `ata_do_dia` copia `dia.orfas` como strings; porta atribui ata e persiste com evento. (`app/ponto/services/cartorio.py:727`, `:736`; `app/ponto/portas/celula.py:1006`, `:1033`, `:1040`, `:899`) | `_orfas_da_ata` conserva horas; ata antiga vira `?`; `grade_da_celula` publica. (`app/escala/services/leitor_celula.py:180`, `:187`, `:190`, `:420`) |
| `orfas_grade` | Adaptador devolve `gd.orfas`, espelho publica; sem grade/marcos devolve vazio. (`app/escala/utils.py:1087`, `:1113`; `app/ponto/services/espelho.py:699`, `:702`, `:854`) | HTML e PDF. (`app/templates/ponto/espelho.html:151`, `:240`; `app/relatorios/pdf_espelho.py:475`) |
| `dias[].data` | Espelho percorre datas da janela e publica a linha. (`app/ponto/services/espelho.py:663`, `:775`) | Indexação de órfãs e recorte PDF; datas dos turnos abertos. (`app/relatorios/pdf_espelho.py:475`, `:479`, `:562`, `:577`) |
| `dias[].batidas` | Juiz atribui data por `pk`; espelho agrupa e publica. (`app/ponto/turnos.py:2945`, `:2953`, `:2955`; `app/ponto/services/espelho.py:579`, `:665`, `:776`) | PDF recupera timestamps classificados e atribui apresentação. (`app/relatorios/pdf_espelho.py:478`, `:567`) |
| `batida.timestamp` | Campo persistido da batida; adaptador transporta como entrada de período. (`app/ponto/models.py:23`; `app/ponto/adaptador_motor.py:37`, `:43`) | Ponte de dia, seleção de órfãs, marca e desenho. (`app/ponto/services/espelho.py:458`; `app/relatorios/pdf_espelho.py:478`, `:568`, `:840`) |
| `periodo.entrada`, `periodo.turno_aberto` | Motor cria entrada localizada e marca aberto quando saída é `None`; pareamento vem da autoridade. (`app/ponto/motor_calculo_v2.py:509`, `:530`, `:2751`, `:2753`, `:2878`, `:2880`, `:3216`, `:3218`) | Guarda do resumo PDF e conta da tela. (`app/relatorios/pdf_espelho.py:178`, `:516`; `app/ponto/services/espelho.py:426`, `:436`) |
| `_orfas_por_data`, `_orfas_instantes` | PDF adapta a classificação lida da tela, restringindo HH:MM à própria linha e armazenando timestamp completo. (`app/relatorios/pdf_espelho.py:475`, `:477`) | Mesma coleção alimenta resumo e `batida.orfa`. (`app/relatorios/pdf_espelho.py:178`, `:516`, `:568`) |
| `batida.orfa` | PDF atribui atributo transitório, sem `save`. Outro escritor transitório existe no calendário; não encontrei chamada desse serviço pelo PDF. (`app/relatorios/pdf_espelho.py:568`; `app/colaboradores/services/calendario.py:622`, `:627`) | Filtro de anomalias, separação/asterisco, alertas da linha e legenda. (`app/relatorios/pdf_espelho.py:675`, `:679`, `:835`, `:844`, `:919`, `:923`, `:983`) |
| `resumo.turnos_abertos` | PDF inicia em zero e conta com guarda; tela conta sem guarda; na competência a autoridade folha pode sobrescrever pelo campo do fechamento. (`app/relatorios/pdf_espelho.py:253`, `:516`, `:517`, `:587`; `app/ponto/services/espelho.py:436`, `:437`; `app/relatorios/cartao_pela_celula.py:112`, `:346`) | Resumo HTML e retorno PDF; datas abertas alimentam `folha_manda`. (`app/templates/ponto/espelho.html:90`, `:92`; `app/relatorios/pdf_espelho.py:603`; `app/relatorios/cartao_pela_celula.py:396`, `:418`) |
| `_abertos_ts`, `datas_turno_aberto`, `dias[].turnos` | PDF guarda entradas contadas; data vem dos cards publicados pela tela, que usa ponte instante→pk→juiz. (`app/relatorios/pdf_espelho.py:518`, `:576`, `:578`; `app/ponto/services/espelho.py:453`, `:461`, `:463`, `:666`, `:777`) | Compositor de abertos e alertas do desenho. (`app/relatorios/pdf_espelho.py:587`, `:597`, `:921`; `app/relatorios/cartao_pela_celula.py:418`) |

**Atenção ao alcance da origem:** `orfas_grade` nem sempre vem de ata: o trecho anterior a `apur_ini` usa builder, assim como o ramo `AtaAgregadaSemMarcos`. O PDF recebe ambos pelo contrato da tela. (`app/ponto/services/espelho.py:493`, `:523`, `:533`, `:536`, `:542`)

**Fecho específico da data:** órfã 10/11 07:02 já não contamina entrada 11/11 07:02 no resumo nem na marca, porque ambos leem timestamps completos recuperados dentro da linha correta. (`app/relatorios/pdf_espelho.py:475`, `:479`, `:178`, `:516`, `:568`)

**Fila herdada comprovável por leitura:** o desenho ainda apaga a data de marcas para filtrar anomalias globalmente. Uma órfã 10/11 07:02 elimina do cabeçalho qualquer anomalia de outro dia contendo “07:02”; ocorrência concreta de produção [nao verificado]. (`app/relatorios/pdf_espelho.py:675`, `:679`; formato das anomalias em `app/ponto/adaptador_motor.py:34`, `:41`)

**Fila semântica:** a classificação original só distingue strings HH:MM; atas antigas podem conter `?`, sem célula retorna vazio, e o adaptador sem marcos também descarta órfãs. Portanto `batida.orfa=False` continua significando ausência de marca disponível, sem provar casamento legítimo. (`app/escala/services/leitor_celula.py:190`, `:390`, `:394`; `app/escala/utils.py:1087`; `app/relatorios/pdf_espelho.py:568`)

**Fila herdada de relógio:** `agora` chega à tela, mas não a `folha_manda`; esta chama turno vivo e teto sem repassar o instante. (`app/relatorios/pdf_espelho.py:474`, `:587`; `app/relatorios/cartao_pela_celula.py:398`, `:412`; `app/ponto/turnos.py:2290`, `:2291`; `app/relatorios/cartao_pela_celula.py:40`)

## 12/11 11:56 · sessao d257aac0c7f0 · - · - · origem=Ronald (tmux)

| Campo | Quem escreve | Quem lê / significado |
|---|---|---|
| `respondida_em` | `responder_pergunta_disputa_core`: instante `now`, texto junto, salva e registra ato (`app/chamados/services/disputa_emissao.py:1462`, `:1467`, `:1468`, `:1508`, `:1511`). `responder_automaticamente`: instante recebido (`:1652`, `:1653`, `:1655`). `carimbar_resposta_pela_trilha`: instante da nota recebido (`:1661`, `:1665`, `:1674`, `:1676`). `carimbar`: completa nulo, apenas com texto, pelo instante do carimbo (`app/chamados/models.py:3125`, `:3131`, `:3133`). `_nascer` conserva `**campos` recebidos (`:3015`, `:3018`, `:3020`). Limpeza: `reabrir_para_correcao` (`app/chamados/services/disputa_emissao.py:1593`, `:1630`, `:1632`), `ressuscitar_perguntas` (`app/chamados/models.py:1567`). | DateTimeField: instante, não dia (`app/chamados/models.py:2449`). C6 lê projeção temporal, ignora `> agora`, inclui igualdade (`app/chamados/juizes.py:1375`, `:1380`, `:1389`, `:1407`). App usa cru `None` + disputa aberta (`app/chamados/services/pergunta_no_app.py:70`, `:71`). Crédito separa cru nulo/não nulo (`app/chamados/services/raiox.py:135`, `:139`, `:140`). Fio usa `all(p.respondida_em…)` (`app/chamados/services/fio.py:340`). Portanto instante tem tipo comum, mas presença de carimbo recebe significados diferentes de pendência nesses leitores. |
| `validada_em` | `_carimbar_validacao`: `timezone.now`, salva e registra ato (`app/chamados/services/validacao.py:127`, `:134`, `:137`, `:139`). `carimbar(...resposta='validar')`: completa nulo com instante comum se texto (`app/chamados/models.py:3131`, `:3135`, `:3136`, `:3153`). `_nascer` conserva recebido (`:3015`, `:3020`). Limpam: nova resposta (`app/chamados/services/disputa_emissao.py:1471`, `:1480`, `:1508`), devolução (`app/chamados/services/validacao.py:199`, `:202`), reabertura e ressuscitação acima. | DateTimeField (`app/chamados/models.py:2450`). C6 ignora validação posterior a `agora`; presente no instante mata a pergunta antes do marco (`app/chamados/juizes.py:1378`, `:1391`). C19 usa decisão `VALIDACAO` e lâmpada: validada sem efeito pede materializar (`app/chamados/services/tarefa_do_dia.py:141`, `:145`, `:146`); não equivale a morte/encerramento. C19 inclui atos `ato_em <= agora` (`:389`, `:391`) e passivo `<= agora` (`:398`, `:399`). |
| `silenciado_ate`, autor, motivo | Escritores físicos `arquivo.silenciar` (`app/chamados/services/arquivo.py:141`, `:166`, `:171`) e `arquivo.levantar` (`:177`, `:202`, `:210`). Chamadores: arquivar/desarquivar/lote (`app/chamados/views_arquivar.py:37`, `:77`, `:140`); ausência (`app/chamados/services/silencio_ausencia.py:73`, `:87`); isentos (`app/chamados/services/silencio_isentos.py:43`); comandos competência/prescritos/sem dia (`app/chamados/management/commands/arquivar_competencia_encerrada.py:288`; `arquivar_prescritos.py:94`; `arquivar_sem_dia_irrecuperavel.py:94`). | Instante de vencimento, ativo exclusivamente `> agora`; igualdade já não silencia (`app/chamados/models.py:508`, `:512`; `app/chamados/services/painel.py:188`, `:190`). Partição usa o mesmo `>` para ARQUIVADO, mas campo preenchido e vencido vira HISTORICO (`app/chamados/services/verbos.py:48`, `:51`), enquanto `qs_na_fila` volta a incluí-lo: classificações diferentes após vencimento. Silêncio não altera `status_local` (`app/chamados/services/arquivo.py:154`). |
| ato `SILENCIO` | `DisputaSupervisao.fechar` nas portas admin/desligamento chama `fechar_por_admin` e registra ato com instante do carimbo (`app/chamados/models.py:1919`, `:1922`, `:1925`, `:1926`). | C19 interpreta como decisão que encerra ações (`app/chamados/services/tarefa_do_dia.py:150`, `:151`). É ato de pergunta, distinto do véu temporal do chamado, que não muda estado (`app/chamados/services/arquivo.py:154`; `app/chamados/models.py:3842`, `:3844`). |
| `status_local` do chamado | Juiz motor, transição humana (`app/chamados/models.py:123`, `:124`); escrita persistente `_gravar_estado` (`:201`, `:228`, `:296`). Nascimento `abrir`: motor decide e `get_or_create` (`:934`, `:936`, `:939`, `:972`); `criar`: motor decide e INSERT (`:1061`, `:1065`, `:1096`). | Censo lê status das âncoras (`app/chamados/services/vigia_fio.py:618`) e usa TERMINAIS (`:675`, `:687`). Fila exige COBRANCA (`app/chamados/services/painel.py:189`). Catálogo distingue VIVOS, COBRANCA, ENCERRADOS e TERMINAIS (`app/chamados/catalogo/motor.py:24`, `:29`, `:30`, `:31`). C6/lastro pode ler ENCERRADOS, não todo TERMINAIS (`app/chamados/models.py:330`, `:332`). Status terminal não prova pergunta morta; app não filtra status do chamado (`app/chamados/services/pergunta_no_app.py:70`, `:71`). Status concreto dentro de TERMINAIS dos 48: [nao verificado]. |
| `via_resolucao` | `_escrever_carimbo` atribui em memória (`app/chamados/models.py:3098`, `:3102`); persistem `carimbar` (`:3153`), identidade (`:3191`), descarimbo (`:3213`), lote (`:3266`, `:3267`). Exceções físicas: `encerrar_por_fato_no_espelho` (`:3276`, `:3277`), `relavra_via.relavrar` (`app/chamados/services/relavra_via.py:125`, `:127`). | C6 lê via associada à materialização; carimbo posterior ao horizonte é retirado, identidade fora do fato permanece (`app/chamados/juizes.py:1376`, `:1382`, `:1384`); vias que vencem resposta (`:1402`) e demais identidades (`:1428`) têm degraus próprios. C19 lê decisões/efeitos/lavra, não resume tudo à via (`app/chamados/services/tarefa_do_dia.py:116`, `:133`, `:141`, `:150`). Via é vocabulário de encerramento (`app/chamados/models.py:2546`); a via isolada não prova qual escritor histórico agiu [nao verificado]. |
| Dia ao qual R/V/via se referem | Instantes acima registram o ato; o dia é outro campo (`app/chamados/models.py:2449`, `:2450`; `app/chamados/juizes.py:334`). | `_dia_de_turno_da_pergunta` usa `data_evento` como dia de turno para fato; conversa grava dia civil e devolve `None` ao juiz de turno (`app/chamados/juizes.py:334`, `:335`, `:338`). C6 janela inclui as duas pontas (`:618`). C19 carrega fatos `conversa=False`, intervalo de dias inclusivo `gte/lte` (`app/chamados/services/tarefa_do_dia.py:373`, `:375`, `:376`). |

| Via raspada | Juiz/escritor atual comprovado |
|---|---|
| `celula_absolveu` | `perguntas_absolvidas_pela_celula` → `retratar_perguntas_absolvidas_por_celula` → `carimbar` (`app/chamados/reconciliador.py:585`, `:596`; `app/chamados/models.py:3140`, `:3153`). |
| `fantasma` | `veredito_orfa` exige luz `is True`, viva e dia encerrado (`app/chamados/reconciliador.py:1083`, `:1085`, `:1087`, `:1089`, `:1091`); consumidor chama `encerrar_fantasma` (`:1148`) → `carimbar` (`app/chamados/models.py:3280`, `:3288`). |
| `fato_ocupado` | Recusa COLISAO, ainda não materializada → `carimbar_identidade` (`app/chamados/services/materializacao.py:1097`, `:1099`, `:1105`). |
| `declaracao` / `ausencia_dia` | Absorção escolhe por `declarou_hora` e carimba (`app/chamados/services/materializacao.py:548`, `:555`, `:567`); materialização principal escolhe via e carimba com validação/lavra (`:1283`, `:1284`). |
| `chamado_encerrado` | `resolver_perguntas_por_chamado_resolvido`: exige C6 morta, escolhe `via_pelo_fato` ou fallback, chama carimbar (`app/chamados/services/disputa_emissao.py:2048`, `:2058`, `:2059`, `:2060`); seleção final da via (`app/chamados/juizes.py:1757`). |
| Autoria histórica individual dessas linhas | As portas atuais acima não provam autor, instante nem caminho que produziu cada registro raspado [nao verificado]. |

## 12/11 11:56 · sessao 6014d0662f17 · - · - · origem=Ronald (tmux)

**Q2 — fecha por inspeção; execução ORM [nao verificado].** O módulo importa diretamente `instantaneo`, `julgar_celula` e `recalcular_fechamento_mes`, chama os três e não instala duplos: `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:31`, `:34`, `:35`, `:79`, `:83`, `:89`.

- **Regra independente:** cadastro 08–17, intervalo 12–13 e jornada 480 (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:56`); quatro batidas reais na segunda (`:66`); ausência folga aprovada na terça (`:73`). Os esperados literais são 960 previstos, 2 dias, 480 abonados, 480 realizados (`:107`). A regra produtiva preserva ausência como jornada, inclui o previsto e abona a parte não realizada (`app/ponto/services/dia_pago.py:392`, `:398`, `:400`, `:402`).

- **Integração real:** o recalculador lê `grade_do_fechamento`, chama `por_dia_da_grade` e preenche previstos/abono/realizado (`app/ponto/services/fechamento.py:1014`, `:1047`, `:1048`, `:1062`, `:1064`); o leitor transporta os três metadados da ata (`app/escala/services/leitor_celula.py:436`). O simulador chama novamente o recalculador real e entrega seus objetos aos três consumidores (`app/folha/management/commands/simular_folha.py:129`, `:133`, `:134`, `:152`).

- **Morde, por raciocínio do código:** `return {}` antecipado reprova `assertIn` (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:85`); perda do transporte na ata reprova presença/tipo/id (`:95`, `:97`, `:98`); recalculador cego a `ausencia_tipo` reprova contraprova que exige 480→0 de abono e diferença 480 no previsto (`:130`, `:139`). Trocar a autoridade de `_retidos` pelo gravado, vazio nesta fixture, deixa `apto_folha=None` e reprova o acesso/asserção de percentagem (`app/folha/management/commands/simular_folha.py:76`, `:85`; `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:125`, `:128`). Execução desses mutantes [nao verificado].

- **Limites:** a ata antiga continua sintetizada da nova, removendo metadados e mudando `tipo_dia`; não é reprodução executada do cartório histórico (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:101`). Os asserts posteriores também comparam saídas antes/depois; não são todos valores independentes (`:116`, `:119`, `:123`). `ausencia_label` é cobrado por presença, sem conferir seu conteúdo (`:95`, `:98`).

- **Comentário impreciso:** “MESMO objeto” não é identidade demonstrada: `_lido()` e `_foto()` fazem chamadas independentes, embora a implementação atual do simulador transporte seus próprios objetos do recalculador (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:83`, `:89`, `:121`; `app/folha/management/commands/simular_folha.py:130`). A execução do ensaio com banco e o seu resultado permanecem [nao verificado].

## 12/11 11:56 · sessao a845178fa0c6 · - · - · origem=Ronald (tmux)

| Tela candidata | View/template e função real da lista | Classe c — 15 | Classe e — 48 |
|---|---|---|---|
| Drawer lateral, aba Mensagens | `app/colaboradores/views.py:915`, chamada `:940`, render `:990`; template `app/templates/colaboradores/partials/painel_colaborador.html:193`; lista real: `perguntas_do_app` → `consulta_do_app` → `projecao_do_app`, `app/chamados/services/pergunta_no_app.py:45`, `:61`, `:78`. | A pergunta respondida fica fora de `pendentes`: exige `respondida_em is None`; vai a `historico`, que esse bloco não renderiza. Outras perguntas do dia podem entrar; col516 tem `app=1` no dado. (`app/chamados/services/pergunta_no_app.py:71`, `:74`; `app/templates/colaboradores/partials/painel_colaborador.html:193`) | **Inclui as perguntas pendentes medidas por `app>0`**: usa a mesma consulta da sonda; não filtra status da âncora ou silêncio. Mostra pergunta, dia e marco. (`app/chamados/services/pergunta_no_app.py:49`, `:58`, `:71`, `:106`; `app/templates/colaboradores/partials/painel_colaborador.html:195`) |
| Modal do fio | `app/chamados/views.py:869`, montagem `:889`, render `:927`; template `app/templates/chamados/partials/modal_fio.html:68`, perguntas `:157`; lista real `montar_chamados_fio`, `app/chamados/services/fio.py:163`. | Chamados entram sem filtro de status/silêncio; pergunta viva entra tanto em disputa aberta quanto fechada. **Ressalva:** enumeração de disputas exige `chamado__isnull=False`, ausente no dado; inclusão individual permanece [nao verificado]. (`app/chamados/services/fio.py:177`, `:184`, `:221`, `:235`) | Mesmo predicado e mesma ressalva de FK. Terminal não exclui cartão/pergunta; não equivale a aparecer na fila principal. (`app/chamados/services/fio.py:177`, `:186`, `:225`, `:240`) |
| Página Fio administrativo | `app/chamados/views.py:659`, montagem `:677`, render `:697`; `app/templates/chamados/fio_colaborador.html:153`; lista real `montar_chamados_fio`, `app/chamados/services/fio.py:163`. | Inclui o chamado terminal pelo filtro apenas de colaborador; template renderiza cartões e followups, sem loop de `perguntas_disputa`. Ver o chamado não comprova ver a pergunta. (`app/chamados/services/fio.py:177`; `app/templates/chamados/fio_colaborador.html:161`, `:237`) | Mesmo comportamento. (`app/chamados/services/fio.py:177`; `app/templates/chamados/fio_colaborador.html:161`, `:237`) |
| Central/painel de gestão | `app/chamados/views.py:721`, chamada `:727`, templates `:740`, `:767`; lista real `montar_contexto_painel`, `app/chamados/services/painel.py:198`. | Âncora terminal **excluída do padrão** por `status_local__in=COBRANCA`. Drill explícito pelo status a inclui; `historico` inclui apenas resolvido/fechado, não cancelado/superado. Status concreto de cada âncora: [nao verificado]. (`app/chamados/services/painel.py:302`, `:306`, `:316`; `app/chamados/catalogo/motor.py:29`) | Mesmo predicado. (`app/chamados/services/painel.py:302`, `:306`, `:316`) |
| Pendências de disputa | `app/colaboradores/views.py:2998`, chamada `:3001`, template `:3011`; lista real `coletar`, `app/chamados/relatorios_disputa.py:26`. | **Condicional:** só disputa aberta entra. Dado não fornece `fechada_em`; inclusão dos 15 [nao verificado]. Se aberta, a categoria depende de TODAS as vivas da disputa: qualquer sem resposta → `AGUARDA_COLAB`; caso contrário → `AGUARDA_SUPERVISAO`. (`app/chamados/relatorios_disputa.py:18`, `:21`, `:40`) | **Disputa incluída**, pois pendência do app exige disputa aberta; classificação `AGUARDA_COLAB` pela pergunta sem resposta. A tela mostra pessoa e idade da disputa, não dias/perguntas individuais. (`app/chamados/services/pergunta_no_app.py:71`; `app/chamados/relatorios_disputa.py:21`, `:41`; `app/templates/colaboradores/partials/pendencias_disputa.html:6`) |
| Calendário do perfil | `app/colaboradores/views.py:1938`, chamada `:1949`, template `:1965`; lista real `contexto_calendario`, `app/colaboradores/services/calendario.py:159`; grade `app/templates/colaboradores/partials/_calendario_grade.html:67`. | **Dia incluído na janela escolhida**, sem filtro por classe/status de chamado: enumera datas inclusive. Âncora terminal não vira badge de ticket: tickets vêm de `VIVOS`; exceção agrupada exige pai vivo. Resolver dia ausente conforme dado e gate do template. (`app/colaboradores/services/calendario.py:437`, `:447`, `:603`, `:609`, `:721`; `app/templates/colaboradores/partials/_calendario_grade.html:125`) | Mesmo predicado: dia visível, pergunta não listada por essa grade. (`app/colaboradores/services/calendario.py:603`, `:721`; `app/templates/colaboradores/partials/_calendario_grade.html:67`) |
| Cadastro × Realidade | `app/escala/views.py:476`, chamada `:487`, template `:492`; lista real `lista`, `app/escala/services/cadastro_realidade.py:787`; ramo C11 `beco_em_lote`, `:442`. | Inclusão **[nao verificado]**: C11 exige ativo, `veredito='indefinida'`, protesto específico, sem porta, viva sob terminal e `folga_nao_definida`; o recorte não informa todos esses campos. (`app/escala/services/cadastro_realidade.py:477`, `:488`, `:515`, `:518`) | Mesma insuficiência. Sem porta + terminal não bastam para provar presença na lista. (`app/escala/services/cadastro_realidade.py:477`, `:518`) |

## 12/11 11:55 · sessao d4ac8dbd5e32 · - · - · origem=Ronald (tmux)

| Obra | PRONTO literal curto na cópia | Cobertura comprovável e limite |
|---|---|---|
| E3-LEITORES | “fio, tela admin, API/app, badge/PWA, o_que_segura, vigia_fio leem SO tarefa_do_dia; 5 respondedores morrem com certidao” (`app/docs/BACKLOG.md:1873`) | Cobre migração dos leitores de todas as classes; não decide por si a ação do estado d-241. O PRONTO do anexo exige “V curada (ou 0)”, mas o dado não traz classificação V/T/X desses 241. (`avais_e3_leitores.txt:5`, `avais_e3_leitores.txt:9`; linhas contadas no anexo colado, arquivo físico [nao verificado]) |
| E4B | “backfill do fio morto (matriz_e4 C3, classes a-e) pela porta canonica” (`app/docs/BACKLOG.md:1894`) | O universo executável é só `sem_porta`; os 21 `porta_fila` ficam fora desse comando. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:76`) |
| E4B, cura e | PRONTO acima. (`app/docs/BACKLOG.md:1894`) | `reabrir_credor_de_pergunta_viva` cobre pergunta sem resposta, viva, com âncora fora de VIVOS; veta limbo/exportadas/recusa da reabertura. Portanto cobre a classe e-48 em escopo, sem prova de cura dos 48; c-15 respondida é excluída dessa cura. (`app/chamados/services/disputa_emissao.py:1805`, `:1821`, `:1827`, `:1835`, `:1839`, `:1848`, `:1851`) |
| E4B, demais atos | PRONTO acima. (`app/docs/BACKLOG.md:1894`) | Consulta C19, registra passivo sem linha, pergunta e materializa conforme ação; `tarefa_nada` produz rótulo, sem ato; `rejulgar` só retorna aviso. Não prova cura de d-241, a-2 ou e-sem-viva-1 pelos agregados. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:87`, `:98`, `:101`, `:117`, `:138`, `:142`, `:144`, `:148`) |
| E4-ESCRITORES/E4C/E4D | “16 portas de estado da pergunta encolhem para efeitos + decisoes de E1”; “E4c LEITORES DIVERGE + selo 2 -> E4d parar projecao” (`app/docs/BACKLOG.md:1885`) | Cobre troca de representação/leitores/escritores; o trecho não promete ação humana específica nem transição individual dessas nove classes. (`app/docs/BACKLOG.md:1885`) |
| E1-ENTRADAS | “ampliada ao censo de toda porta do fato com as 3 curas de classe no mesmo commit” (`app/docs/BACKLOG.md:1859`) | Cobre portas do fato em âmbito estrutural; atribuir qualquer desses estados concretos a uma das três curas, sem dados adicionais, permanece [nao verificado]. |
| RAIZ | “elo raiz<-filhos: PerguntaDisputa com escritores fora do agregado, DisputaSupervisao nao reage”; sequência F0–F7. (`app/docs/BACKLOG.md:1904`) | Cobre a reação do agregado e o problema da dona fechada em âmbito estrutural; o PRONTO fornecido não identifica qual cura individual alcança c-15/e-48 ou os seis `cobrar` sem app. (`app/docs/BACKLOG.md:1904`) |
| PORTA-QUE-NAO-AGE | Substituída por MAPA-DO-FIO-MORTO no anexo. (`anexo_backlog.txt:11`, arquivo físico [nao verificado]) | Não usar a antiga promessa de 311→326 como cobertura ativa; o aval MAPA é medida, nenhum código. (`aval_mapa_do_fio_morto_1211.txt:2`, `:4`, arquivo físico [nao verificado]) |

**Pergunta 2: NÃO.** Nos anexos `avais_e3_leitores.txt` e `censo.r` não há decisão específica sobre o que C19 deve mandar — ou que deva mandar nada — quando o dia retém a folha e C19 não envia ação. A fase V/T/X é procedimento para decidir; “conservar o universo” e distinguir nada/desconhecido/sem porta não escolhem a ação desse estado. (`avais_e3_leitores.txt:4`, `:5`, `:10`, `:18`; `censo.r`, seção 3, decisões 4–5; arquivos físicos [nao verificado])

Atenção: a cópia possui relato histórico que discutiu esse desacordo e reclassificou universos anteriores; não equivale a decisão escrita no brief/censo anexados nem classifica os atuais 241. (`app/docs/RELATO.md:363`, `:365`, `:370`)

Diferença essencial para d-2: `censo_alcance` lê vivas por **FK célula**; a sonda lê fatos por **data_evento** e o app possui outro universo. Portanto `d + COLAB_RESPONDE` não autoriza concluir incoerência de C6 nem identificar o escritor concreto. (`app/chamados/services/vigia_fio.py:610`, `:614`, `:640`, `:677`; `sonda_mapa_fio.py`, laço de `ps_dia`/`pend_app`, arquivo físico [nao verificado])

## 12/11 11:49 · sessao 6d7e592f4fec · AUDITORIA-DE-TRAVAS-CORTE · verificador · origem=Code

REPROVADO

As curas de medição e precedência estão presentes e os seis mutantes declarados caem no log fornecido. Restam divergências com o aval literal, um aviso truncado para nomes maiores e a possibilidade de gastar novamente a cota herdada em `auto`. Ocorrência desses caminhos em produção: `[nao verificado]`. (`bin/disjuntor.py:1024`, `bin/disjuntor.py:1043`, `bin/disjuntor.py:1177`, `bin/disjuntor.py:210`; `item2v2_mutantes.log:21`, `item2v2_mutantes.log:76`)

As referências aos anexos contam linhas desde o primeiro conteúdo de cada anexo. Comparação pelo histórico Git do intervalo informado e execução integral local do selo: `[nao verificado]`. O script cria, altera e apaga arquivos; minha avaliação da execução usa o log anexado. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:51`, `bin/tests/test_disjuntor.sh:1676`)

**Delta dos achados da volta 1:**

| Achado | A volta 2 cura? | O selo morde? |
|---|---|---|
| Aviso da segunda AUTO | **Parcialmente.** `auto_esgotada` governa tanto a mensagem quanto o aviso do disparo. Porém `trilha()` corta o detalhe em 300 caracteres: para `AUDITORIA-DE-TRAVAS-CORTE`, com uma reprovação final, o aviso tem 317 e termina em `so linha`, perdendo `/rearma do Ronald`. (`bin/disjuntor.py:1031`, `bin/disjuntor.py:1177`, `bin/disjuntor.py:1183`, `bin/disjuntor.py:1194`, `bin/disjuntor.py:210`) | **Sim, para a retirada do aviso**, com `OBRA-A`; não cobre o truncamento acima. O mutante falha no caso 6, rc=6. (`bin/tests/test_disjuntor.sh:1233`, `bin/tests/test_disjuntor.sh:1239`, `bin/tests/test_disjuntor.sh:1988`; `item2v2_mutantes.log:51`, `item2v2_mutantes.log:55`) |
| Testemunha positiva de `lib['auto']` | **Sim.** O caso 7 exige `N >= 1`, igualdade entre trilha e medidor e presença de `auto=N` na apresentação. (`bin/tests/test_disjuntor.sh:1241`, `bin/tests/test_disjuntor.sh:1249`) | **Sim.** Retirar `auto` do medidor produz trilha=3, medidor=0 e rc=7. (`bin/tests/test_disjuntor.sh:1991`; `item2v2_mutantes.log:63`, `item2v2_mutantes.log:66`) |
| Contrato FALHA × SOBE | **O contrato fornecido está corrigido:** agora pede SOBE, como o código. Não é uma troca de evento no código, que já usava SOBE na volta 1. (`contrato_item2.txt:4`; `bin/disjuntor.py:1053`; `preverif_item2.r`, resposta 1 — localização exata `[nao verificado]`) | O caso 2 exige exatamente um SOBE; trocar o evento por FALHA violaria esse assert. Não há mutante específico dessa troca entre os seis declarados. (`bin/tests/test_disjuntor.sh:1181`, `bin/tests/test_disjuntor.sh:1190`, `bin/tests/test_disjuntor.sh:1986`) |
| Nomes da cota e da medida | **Sim, na representação atual:** `auto_usada` é a lista cumulativa por obra; `lib['auto']` conta eventos na janela do painel. **Há ressalva de compatibilidade:** a guarda ignora um antigo `r['auto']`. (`bin/disjuntor.py:150`, `bin/disjuntor.py:1057`, `bin/disjuntor.py:1177`, `bin/disjuntor.py:2025`) | Os casos 1/2 verificam `auto_usada` e o 7 verifica a medida positiva. Não há caso nem mutante declarado para preservar cota anteriormente gravada em `auto`. (`bin/tests/test_disjuntor.sh:1174`, `bin/tests/test_disjuntor.sh:1184`, `bin/tests/test_disjuntor.sh:1249`, `bin/tests/test_disjuntor.sh:1986`) |
| Precedência DESENHO/ERRO-BOBO × AUTO | **Sim.** A ordenação estável põe ambas as portas antigas antes de AUTO, preservando sua ordem relativa; a cota só é gasta se nenhuma candidata antiga elegível liberar. (`bin/disjuntor.py:1043`, `bin/disjuntor.py:1046`, `bin/disjuntor.py:1057`, `bin/disjuntor.py:1065`) | **Sim para a composição com ERRO-BOBO:** sem ordenação, gasta AUTO e o caso 8 cai, rc=8. DESENHO participa da mesma regra, mas não tem composição própria no caso 8. (`bin/tests/test_disjuntor.sh:1254`, `bin/tests/test_disjuntor.sh:1261`, `bin/tests/test_disjuntor.sh:1992`; `item2v2_mutantes.log:75`, `item2v2_mutantes.log:77`) |

**1. O diff realiza literalmente o item 2?**

**Não integralmente.** O disparo continua sendo por quantidade de reprovações finais ou voltas Codex procedentes, sem consultar FORMA/ERRO-BOBO/DESENHO; a chamada que dispara retorna PARADA antes de examinar liberações. A classe é examinada na linha posterior `AUTOLIBERA`. Isso implementa o mecanismo de porta separada do contrato, mas não um disparo condicionado literalmente a DESENHO. (`aval_auditoria_de_travas_corte_1211.txt:4`; `contrato_item2.txt:2`; `bin/disjuntor.py:656`, `bin/disjuntor.py:1024`, `bin/disjuntor.py:1033`, `bin/disjuntor.py:767`)

A segunda ocorrência também não fica exclusivamente dependente do Ronald: a cota restringe apenas `tipo == 'auto'`; uma nova linha antiga `ERRO-BOBO` libera sem cota nem comprovação de autoria. O próprio guardião descreve essa porta como disponível ao Code. Isso preserva o contrato, mas diverge da leitura literal “a 2a da mesma obra sobe a mim”. (`aval_auditoria_de_travas_corte_1211.txt:4`; `contrato_item2.txt:2`; `bin/disjuntor.py:239`, `bin/disjuntor.py:761`, `bin/disjuntor.py:1050`)

A porta nova exige classe aceita e motivo com letra ou dígito; registra a liberação com tipo `auto` e a linha original, incluindo a classe. Esse trecho atende ao rastro exigido. (`bin/disjuntor.py:148`, `bin/disjuntor.py:767`, `bin/disjuntor.py:1062`)

**2. Quem escreve e quem lê?**

| Campo/valor | Escritores | Leitores |
|---|---|---|
| Antigo `est['b3'][obra]['auto']` | Nenhum escritor específico identificado na implementação atual; o JSON genérico preserva chaves existentes. (`bin/disjuntor.py:378`, `bin/disjuntor.py:383`, `bin/disjuntor.py:1057`) | Nenhum leitor semântico atual da cota: a guarda lê exclusivamente `auto_usada`. Os carregadores genéricos leem o objeto inteiro. (`bin/disjuntor.py:371`, `bin/disjuntor.py:405`, `bin/disjuntor.py:1177`) |
| `auto_usada` | `decide_b3` acrescenta o SHA quando AUTO libera; `_com_estado` persiste. (`bin/disjuntor.py:1057`, `bin/disjuntor.py:383`) | `auto_esgotada`, chamado pela recusa, pelo aviso do disparo e por `mensagem_b3`; testes 1, 2 e 8. (`bin/disjuntor.py:1050`, `bin/disjuntor.py:1031`, `bin/disjuntor.py:1194`; `bin/tests/test_disjuntor.sh:1174`, `bin/tests/test_disjuntor.sh:1184`, `bin/tests/test_disjuntor.sh:1256`) |
| `auto_sobe` | `decide_b3` acrescenta SHA recusado, persistido pela mesma porta. (`bin/disjuntor.py:1052`, `bin/disjuntor.py:1081`) | A própria deduplicação de SOBE. (`bin/disjuntor.py:1051`) |
| `lib['auto']` → `b3_libera['auto']` | `ler()` inicializa e incrementa a partir de eventos LIBERA reconhecidos após o corte temporal. (`bin/disjuntor.py:2021`, `bin/disjuntor.py:2025`, `bin/disjuntor.py:2048`, `bin/disjuntor.py:2093`) | `linha()`; `gerar_estado.py` via `ler/linha`; CLI `--linha`; testes antigo e positivo novo. (`bin/disjuntor.py:2310`, `bin/disjuntor.py:2313`, `bin/disjuntor.py:2395`; `bin/gerar_estado.py:280`; `bin/tests/test_disjuntor.sh:430`, `bin/tests/test_disjuntor.sh:1246`) |
| Evento `SOBE` | `decide_b3` prepara; `_anuncia` chama `trilha()` somente após persistência bem-sucedida. (`bin/disjuntor.py:1053`, `bin/disjuntor.py:1070`, `bin/disjuntor.py:1073`) | Leitor específico identificado: `conta()` no selo. O medidor do ESTADO não enumera SOBE; o leitor de censo o descarta pelo reconhecedor. (`bin/tests/test_disjuntor.sh:1160`, `bin/tests/test_disjuntor.sh:1181`; `bin/disjuntor.py:2027`, `bin/disjuntor.py:2097`, `bin/disjuntor.py:2140`) |
| SHA consumido | `_sha_b3` calcula; `decide_b3` acrescenta a `consumidas`. `_fotografa` grava os SHA anteriores em `topo_no_disparo`. (`bin/disjuntor.py:681`, `bin/disjuntor.py:1061`, `bin/disjuntor.py:836`, `bin/disjuntor.py:845`) | Guarda contra reutilização/anterioridade; AUTO aceita também o armazena em `auto_usada`, e AUTO recusada em `auto_sobe`. (`bin/disjuntor.py:1046`, `bin/disjuntor.py:1052`, `bin/disjuntor.py:1057`) |
| `B3_AUTO_COTA` | Constante definida como 1. (`bin/disjuntor.py:149`) | Predicado da cota, detalhe de SOBE e instrução da mensagem. (`bin/disjuntor.py:1177`, `bin/disjuntor.py:1054`, `bin/disjuntor.py:1187`) |
| Classe/tipo `auto` | Code escreve a linha conforme contrato; o reconhecedor produz a candidata `('auto', linha)`. A fixture escreve pelo auxiliar `rel()`. (`contrato_item2.txt:2`; `bin/disjuntor.py:768`; `bin/tests/test_disjuntor.sh:1151`) | Reconhecedor valida classe/motivo; decisão aplica precedência/cota; LIBERA leva o tipo ao medidor. (`bin/disjuntor.py:767`, `bin/disjuntor.py:1043`, `bin/disjuntor.py:1050`, `bin/disjuntor.py:1062`, `bin/disjuntor.py:2048`) |

Os leitores decisórios fora do trecho alterado são:

- **Stop:** `hook_stop_fila1 → julga_b3 → decide_b3`; para cada parada, chama `mensagem_b3`, que agora relê a cota persistida. (`bin/hook_stop_fila1.py:1204`, `bin/hook_stop_fila1.py:1206`; `bin/disjuntor.py:1102`, `bin/disjuntor.py:1194`)
- **PreToolUse:** `_nega_b3 → estado_b3 → julga_b3 → decide_b3`; PARADA recebe `mensagem_b3`. (`bin/disjuntor.py:1826`, `bin/disjuntor.py:1828`, `bin/disjuntor.py:1111`, `bin/disjuntor.py:1102`)
- **Wrapper:** `codex_pergunta.sh → --b3-parada → b3_parada → estado_b3`; retorno não zero recusa a volta. (`bin/codex_pergunta.sh:378`; `bin/disjuntor.py:2407`, `bin/disjuntor.py:1164`)
- **ESTADO:** `gerar_estado.py` recebe a medida por `ler/linha`, incorpora `dj_txt` e grava a página. Não usa a cota como contador do painel. (`bin/gerar_estado.py:281`, `bin/gerar_estado.py:378`, `bin/gerar_estado.py:390`; `bin/disjuntor.py:2025`)
- **Carregadores genéricos:** os demais consumidores de `ler_estado` selecionam B1, B4 ou listas históricas; a referência do hook aos arquivos do disjuntor serve para rejeitá-los como prova de espera. (`bin/disjuntor.py:600`, `bin/disjuntor.py:1907`, `bin/disjuntor.py:2238`; `bin/hook_stop_fila1.py:664`)

**3. Segunda AUTO, DESENHO e preservação da cota.**

**Estado atual, segunda AUTO sozinha:** não libera; acrescenta o SHA recusado a `auto_sobe`, anuncia uma vez e não o acrescenta a `consumidas`. Duas chamadas e a preservação após rearme são exercitadas pelo caso 2. (`bin/disjuntor.py:1050`, `bin/disjuntor.py:1055`, `bin/disjuntor.py:1061`; `bin/tests/test_disjuntor.sh:1176`, `bin/tests/test_disjuntor.sh:1190`; `item2v2_mutantes.log:3`)

**Segunda ocorrência pela porta antiga:** com cota gasta, obra disparada e linhas novas posteriores à fotografia:

```text
AUTOLIBERA OBRA-A: classe=FORMA segundo erro
ERRO-BOBO OBRA-A: segundo erro escrito pelo Code
```

A linha antiga libera primeiro. Na volta 2, diferentemente do contraexemplo anterior, essa composição nem chega a produzir SOBE para a AUTO, pois encerra o loop na antiga. Não há autenticação do autor no reconhecedor. (`bin/disjuntor.py:761`, `bin/disjuntor.py:1043`, `bin/disjuntor.py:1065`; `bin/tests/test_disjuntor.sh:1218`)

**DESENHO pela porta nova:** `classe=DESENHO` é rejeitada. A validação é do rótulo, portanto `classe=FORMA erro de desenho` pode ser aceita; a natureza real do defeito não é julgada pelo reconhecedor. Classificação correta em produção: `[nao verificado]`. (`bin/disjuntor.py:148`, `bin/disjuntor.py:767`; `bin/tests/test_disjuntor.sh:1195`)

**Rearme/reinício com estado atual:** rearme não apaga `auto_usada`; a próxima decisão relê o JSON. Se o arquivo desaparecer, a inicialização parte de `{}`; isso é perda de estado, não um efeito necessário do reinício. (`bin/disjuntor.py:1140`, `bin/disjuntor.py:1142`, `bin/disjuntor.py:370`, `bin/disjuntor.py:372`)

**Contraexemplo de compatibilidade:** obra disparada, demais condições de liberação satisfeitas, estado contendo `auto=['sha-da-primeira']` e sem `auto_usada`; uma AUTO nova libera e passa a criar `auto_usada=[sha-da-segunda]`. Não há conversão da chave antiga na migração existente. O contrato novo explicitamente define ausência de `auto_usada` como zero, portanto esse comportamento obedece ao contrato, mas não preserva “1x por obra” entre versões. Existência desse estado em produção: `[nao verificado]`. (`bin/disjuntor.py:808`, `bin/disjuntor.py:1050`, `bin/disjuntor.py:1057`, `bin/disjuntor.py:1177`; `contrato_item2.txt:4`)

**4. As linhas DESENHO/ERRO-BOBO antigas mudaram?**

Seus validadores continuam com as mesmas exigências mostradas no diff, e a ordenação nova preserva sua ordem relativa. A cota AUTO não impede essas linhas de liberar. (`bin/disjuntor.py:750`, `bin/disjuntor.py:761`, `bin/disjuntor.py:1043`, `bin/disjuntor.py:1050`)

Na composição introduzida na volta 1, há mudança corretiva: AUTO antes de ERRO-BOBO deixa de gastar cota e consumir o SHA da AUTO; a antiga vence e recebe o evento LIBERA. Isso é demonstrado pelo caso 8. O mesmo critério de ordenação inclui DESENHO. (`bin/disjuntor.py:1043`, `bin/disjuntor.py:1061`, `bin/disjuntor.py:1062`; `bin/tests/test_disjuntor.sh:1254`, `bin/tests/test_disjuntor.sh:1261`)

**5. O selo morde cada mutante? Há assert vazio?**

**Sim, os seis declarados mordem no log fornecido; o controle sem mutação passa.** (`item2v2_mutantes.log:10`, `item2v2_mutantes.log:11`)

| Mutante | Mordida identificada |
|---|---|
| `auto-libera-a-2a` | Caso 2: segunda libera e cota chega a 2; falhas `[2]`, rc=2. (`bin/tests/test_disjuntor.sh:1190`; `item2v2_mutantes.log:14`, `item2v2_mutantes.log:21`) |
| `auto-libera-desenho` | Caso 3: DESENHO reconhecida e liberada; falhas `[3]`, rc=3. (`bin/tests/test_disjuntor.sh:1199`; `item2v2_mutantes.log:26`, `item2v2_mutantes.log:32`) |
| `sem-classe-libera` | Casos 3/4: reconhece DESENHO e libera os quatro inválidos; falhas `[3, 4]`, rc=3. (`bin/tests/test_disjuntor.sh:1199`, `bin/tests/test_disjuntor.sh:1210`; `item2v2_mutantes.log:37`, `item2v2_mutantes.log:43`) |
| `sem-aviso-da-2a` | Caso 6: mensagem e aviso perdem a marca; falhas `[6]`, rc=6. (`bin/tests/test_disjuntor.sh:1239`; `item2v2_mutantes.log:51`, `item2v2_mutantes.log:54`) |
| `medidor-sem-auto` | Caso 7: trilha=3, medidor=0; falhas `[7]`, rc=7. (`bin/tests/test_disjuntor.sh:1249`; `item2v2_mutantes.log:63`, `item2v2_mutantes.log:65`) |
| `auto-sem-precedencia` | Caso 8: ERRO-BOBO=0, AUTO=1, cota gasta e AUTO posterior recusada; falhas `[8]`, rc=8. (`bin/tests/test_disjuntor.sh:1261`; `item2v2_mutantes.log:75`, `item2v2_mutantes.log:76`) |

Não há `all([])` nos controles atuais: `rs` recebe quatro resultados e `ctrl`, três. O positivo do medidor também exige pelo menos um evento. (`bin/tests/test_disjuntor.sh:1205`, `bin/tests/test_disjuntor.sh:1208`, `bin/tests/test_disjuntor.sh:1214`, `bin/tests/test_disjuntor.sh:1219`, `bin/tests/test_disjuntor.sh:1249`)

Persistem fragilidades: o caso 5 ignora o retorno de `dispara()` e só exige ausência no mapa de paradas; o caso 8 também ignora o segundo disparo e não exige LIBERA(auto)/cota após a tentativa final. São verificações que podem passar sem provar aquela transição se a preparação falhar; o log fornecido não demonstra esse esvaziamento. (`bin/tests/test_disjuntor.sh:1217`, `bin/tests/test_disjuntor.sh:1219`, `bin/tests/test_disjuntor.sh:1257`, `bin/tests/test_disjuntor.sh:1261`)

O harness exige âncora única e inclui controle da cópia sem mutação. Isso sustenta os mutantes declarados, sem cobrir automaticamente truncamento, migração da chave antiga ou composição AUTO+DESENHO. (`bin/tests/test_disjuntor.sh:1670`, `bin/tests/test_disjuntor.sh:1674`, `bin/tests/test_disjuntor.sh:1696`, `bin/tests/test_disjuntor.sh:1254`, `bin/tests/test_disjuntor.sh:1986`)

**6. Classes que já reprovaram obra.**

| Classe | Resultado |
|---|---|
| Segundo escritor fora da porta | **Não identificado para a cota atual.** A alteração específica de `auto_usada` está em `decide_b3`, persistida por `_com_estado`. (`bin/disjuntor.py:1057`, `bin/disjuntor.py:1081`) |
| Relógio fora da borda | **Presente, herdado:** `decide_b3` chama `time.time()` quando `agora` falta; `b3_parada` não fornece esse instante. `trilha()` também lê `datetime.now()`. A nova cota é independente de relógio. (`bin/disjuntor.py:992`, `bin/disjuntor.py:1164`, `bin/disjuntor.py:210`, `bin/disjuntor.py:1177`) |
| Selo sem mordida | **Não para os seis declarados.** Há lacunas concretas de cobertura, descritas acima, e o nome curto do caso 6 deixa passar o truncamento. (`item2v2_mutantes.log:21`, `item2v2_mutantes.log:32`, `item2v2_mutantes.log:43`, `item2v2_mutantes.log:54`, `item2v2_mutantes.log:65`, `item2v2_mutantes.log:76`; `bin/tests/test_disjuntor.sh:1236`; `bin/disjuntor.py:210`) |
| Campo com dois significados | **Curado nos nomes atuais:** `auto_usada` e `lib['auto']` são células distintas para cota e medida. A compatibilidade com o antigo `auto` permanece descoberta. (`bin/disjuntor.py:150`, `bin/disjuntor.py:1177`, `bin/disjuntor.py:2025`) |
| Ordem de trava | **Não identifiquei inversão acrescentada.** Decisão, persistência e anúncio permanecem sob a trava do estado; wrapper e triagem adquirem resposta antes de estado. A nova leitura da mensagem não adquire outra trava. (`bin/disjuntor.py:363`, `bin/disjuntor.py:395`, `bin/disjuntor.py:1081`, `bin/disjuntor.py:901`, `bin/disjuntor.py:933`, `bin/disjuntor.py:404`; `bin/codex_pergunta.sh:376`, `bin/codex_pergunta.sh:378`) |

“Sobe ao Ronald” tem evidência de evento SOBE e de mensagens locais; entrega efetiva ao Ronald permanece `[nao verificado]`. O ESTADO não apresenta SOBE como evento próprio. (`bin/disjuntor.py:1053`, `bin/disjuntor.py:1183`, `bin/disjuntor.py:2027`)

## 12/11 11:48 · sessao 3d6205c84e19 · PORTA-QUE-NAO-AGE · sonda · origem=Code

Os valores abaixo vêm exclusivamente do JSON fornecido. Nas tabelas, `[nao verificado]` marca a ausência de comprovação desses valores por arquivo:linha da cópia; as referências ao código explicam os campos.

1. `medicao_completa = true`; `dias_erro = 0`; `dias_medidos = 15`, de `dias_fixos = 15`. Não há dia com erro, etapa/função de falha ou classe de erro reportados. [nao verificado]

2. Tabela por dia.

`F(ids)` representa o objeto completo: `rota = porta_fila`, `fio = chamado`, `fila = [ids]` e `verbos = {cada id: decidir}`. Esses valores são os reportados no JSON. [nao verificado] O formato da porta está em `app/chamados/juizes.py:1657`; os significados de fila, verbos e fio, em `app/chamados/juizes.py:1621`.

A coluna resolver transcreve `resolver_dia_oferecido_nas_datas`; dono transcreve `dono_fora_do_fio_chamado`. O mapeamento exato desse último campo raspado à função do código é [nao verificado]. `motivos_a_perguntar` devolve os motivos que ainda pedem pergunta (`app/chamados/services/fabrica_da_celula.py:92`).

| col | data | porta_alcancavel_do_dia | resolver | n perguntas | n vivas | motivos_a_perguntar | dono | Evidência |
|---|---|---|---|---:|---:|---|---|---|
| col129 | 2026-10-25 | F(35101) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col129 | 2026-11-01 | F(36592, 37904) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col129 | 2026-11-02 | F(37905) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col129 | 2026-11-08 | F(37925) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col258 | 2026-10-30 | F(35974) | False | 1 | 0 | vazio `[]` | False | [nao verificado] |
| col258 | 2026-11-06 | F(37406) | False | 1 | 0 | vazio `[]` | False | [nao verificado] |
| col258 | 2026-11-09 | F(38110) | False | 1 | 0 | vazio `[]` | False | [nao verificado] |
| col427 | 2026-09-21 | F(28969) | False | 0 | 0 | vazio `[]` | False | [nao verificado] |
| col451 | 2026-10-21 | F(33286) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col451 | 2026-10-23 | F(34254) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col451 | 2026-10-25 | F(34850) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col451 | 2026-10-27 | F(35331) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col502 | 2026-10-28 | F(35432) | False | 4 | 0 | vazio `[]` | False | [nao verificado] |
| col899 | 2026-10-24 | F(34431) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col999 | 2026-10-28 | F(35602, 35697) | False | 1 | 0 | vazio `[]` | False | [nao verificado] |

3. Tabela por pergunta.

Respondida/validada indica exclusivamente o preenchimento do respectivo timestamp, campos definidos em `app/chamados/models.py:2449` e `app/chamados/models.py:2450`. `validada_por` está em `app/chamados/models.py:3378`; `via_resolucao`, em `app/chamados/models.py:2562`. Vitalidade é calculada em `app/chamados/juizes.py:1389`; o marco admite `True`, `False` e `None` (`app/chamados/juizes.py:930`).

Datas e horas abaixo são locais, todas de 2026, com offset `-03:00`; os microssegundos foram preservados. [nao verificado] “Quem carimbou” usa somente `auditoria_com_payload_da_via_atual`, conforme solicitado; autoria por outros campos ou decisões permanece [nao verificado]. Os campos de usuário e criação da auditoria estão em `app/core/models.py:295` e `app/core/models.py:307`.

| pk | col / dia | motivo | via_resolucao | Respondida? / local | Validada? / local | validada_por_id | pergunta_viva | fato_do_marco_no_espelho | Quem carimbou: usuario_id / criado_em_local / auditoria pk | Evidência |
|---:|---|---|---|---|---|---:|---|---|---|---|
| 50169 | col129 / 10-25 | saida_sem_entrada | fantasma | sim / 10-27 07:53:49.378656 | sim / 10-28 09:45:05.311132 | 657 | False | False | [nao verificado] | [nao verificado] |
| 50170 | col129 / 10-25 | orfao_14h | fantasma | não / null | não / null | null | False | True | [nao verificado] | [nao verificado] |
| 52730 | col129 / 11-01 | saida_sem_entrada | declaracao | sim / 11-02 18:07:34.143947 | sim / 11-09 07:44:34.128888 | 657 | False | False | [nao verificado] | [nao verificado] |
| 53028 | col129 / 11-01 | orfao_14h | fantasma | não / null | não / null | null | False | True | 666 / 11-02 09:42:03.090529 / 845648 | [nao verificado] |
| 53288 | col129 / 11-02 | saida_sem_entrada | declaracao | sim / 11-03 06:31:00.239353 | sim / 11-09 07:44:49.944781 | 657 | False | False | [nao verificado] | [nao verificado] |
| 53333 | col129 / 11-02 | orfao_14h | declaracao | sim / 11-09 06:41:16.655654 | sim / 11-09 07:44:39.581541 | 657 | False | True | [nao verificado] | [nao verificado] |
| 55524 | col129 / 11-08 | saida_sem_entrada | declaracao | sim / 11-09 06:31:03.566947 | sim / 11-09 07:46:49.602920 | 657 | False | False | [nao verificado] | [nao verificado] |
| 55594 | col129 / 11-08 | orfao_14h | declaracao | sim / 11-09 06:31:38.641578 | sim / 11-09 07:51:24.055621 | 657 | False | True | [nao verificado] | [nao verificado] |
| 52131 | col258 / 10-30 | orfao_14h | fantasma | não / null | não / null | null | False | True | 666 / 11-10 12:21:02.666408 / 927797 | [nao verificado] |
| 54827 | col258 / 11-06 | orfao_14h | fantasma | não / null | não / null | null | False | True | 666 / 11-10 12:16:02.672755 / 927673 | [nao verificado] |
| 56040 | col258 / 11-09 | orfao_14h | ausencia_dia | não / null | não / null | null | False | True | [nao verificado] | [nao verificado] |
| 46703 | col451 / 10-21 | saida_sem_entrada | fantasma | sim / 10-24 19:12:21.246122 | sim / 10-26 14:54:46.529352 | 657 | False | False | [nao verificado] | [nao verificado] |
| 46704 | col451 / 10-21 | orfao_14h | declaracao | sim / 10-24 19:12:31.807198 | sim / 10-29 11:33:16.500910 | 657 | False | True | [nao verificado] | [nao verificado] |
| 49422 | col451 / 10-23 | saida_sem_entrada | fantasma | sim / 10-24 19:12:52.450098 | sim / 10-26 14:54:46.995846 | 657 | False | False | [nao verificado] | [nao verificado] |
| 49423 | col451 / 10-23 | orfao_14h | declaracao | sim / 10-24 19:13:02.454158 | sim / 10-29 11:33:43.367442 | 657 | False | True | [nao verificado] | [nao verificado] |
| 50179 | col451 / 10-25 | saida_sem_entrada | fantasma | sim / 10-28 18:56:55.079529 | sim / 10-29 11:33:57.614654 | 657 | False | False | [nao verificado] | [nao verificado] |
| 50180 | col451 / 10-25 | orfao_14h | declaracao | sim / 10-28 18:57:04.067558 | sim / 10-29 11:34:01.448378 | 657 | False | True | [nao verificado] | [nao verificado] |
| 50883 | col451 / 10-27 | saida_sem_entrada | fantasma | sim / 10-28 18:57:23.247879 | sim / 10-29 11:34:13.719129 | 657 | False | False | [nao verificado] | [nao verificado] |
| 51025 | col451 / 10-27 | orfao_14h | declaracao | sim / 10-28 18:57:32.171962 | sim / 10-29 11:34:17.021036 | 657 | False | True | [nao verificado] | [nao verificado] |
| 51042 | col502 / 10-28 | saida_sem_entrada | declaracao | sim / 10-30 14:02:10.543140 | sim / 10-30 15:04:02.578131 | 657 | False | True | [nao verificado] | [nao verificado] |
| 51108 | col502 / 10-28 | intervalo_saida | declaracao | sim / 10-30 13:56:58.855588 | sim / 10-30 15:04:09.520077 | 657 | False | True | [nao verificado] | [nao verificado] |
| 51132 | col502 / 10-28 | intervalo_volta | declaracao | sim / 10-30 13:57:12.532781 | sim / 10-30 15:04:15.720394 | 657 | False | True | [nao verificado] | [nao verificado] |
| 51185 | col502 / 10-28 | orfao_14h | declaracao | sim / 10-30 07:16:04.323504 | sim / 10-30 12:19:16.432883 | 657 | False | False | [nao verificado] | [nao verificado] |
| 49654 | col899 / 10-24 | orfao_14h | fato_ocupado | sim / 11-11 07:40:55.133099 | sim / 11-11 07:41:09.186539 | 657 | False | False | [nao verificado] | [nao verificado] |
| 49655 | col899 / 10-24 | saida_sem_entrada | declaracao | sim / 11-01 12:19:46.972682 | sim / 11-11 07:40:58.843348 | 657 | False | True | [nao verificado] | [nao verificado] |
| 51467 | col999 / 10-28 | orfao_14h | fato_ocupado | sim / 11-12 07:08:34.900551 | sim / 11-12 07:08:40.334664 | 657 | False | False | [nao verificado] | [nao verificado] |

4. Classes com o filtro solicitado: `pergunta_viva = False` e `fato_do_marco_no_espelho ∈ {False, None}`. Nesse recorte, os marcos são todos `False`; não há `None`. [nao verificado]

| Classe / via_resolucao | n perguntas | n dias-colab | Dias | pks | Evidência |
|---|---:|---:|---|---|---|
| fantasma | 5 | 5 | col129: 2026-10-25; col451: 2026-10-21, 2026-10-23, 2026-10-25, 2026-10-27 | 50169, 46703, 49422, 50179, 50883 | [nao verificado] |
| declaracao | 4 | 4 | col129: 2026-11-01, 2026-11-02, 2026-11-08; col502: 2026-10-28 | 52730, 53288, 55524, 51185 | [nao verificado] |
| fato_ocupado | 2 | 2 | col899: 2026-10-24; col999: 2026-10-28 | 49654, 51467 | [nao verificado] |
| Sem pergunta, com motivos_a_perguntar não vazio | 0 | 0 | nenhum | nenhum | [nao verificado] |

Esses agrupamentos registram a via atual e o estado observado. A causa C6 por degrau permanece `[nao verificado]`, conforme o próprio JSON.

5. Para col427 em 2026-09-21, o D4 reporta `status = folga`, `fonte = lavrado`; a janela atual informada é 2026-10-21 a 2026-11-20. [nao verificado]

| Operando da regra anexada | Valor / resultado | Evidência |
|---|---|---|
| `dia.get('pode_ultimo_recurso')` | True | [nao verificado] |
| `dia['data'] < hoje` | `2026-09-21 < 2026-11-12` → True | [nao verificado] |
| `dia.get('status') in STATUS_QUE_PEDEM_DECISAO` | `folga` no conjunto informado → False | [nao verificado] |
| Conjunção / retorno real | `True and True and False` → False | [nao verificado] |

**O operando que torna o resultado falso é a pertinência do status:** `folga` não pertence a `('falta', 'irregular', 'aberto', 'sem_certidao')`. O conjunto está em `app/colaboradores/services/calendario.py:94`; a conjunção, em `app/colaboradores/services/calendario.py:103`.

6. **As classes filtradas do item 4 cobrem 11 dos 15 dias**, com soma `5 + 4 + 2 = 11`, sem sobreposição entre esses grupos. Os quatro dias restantes são: [nao verificado]

| Dia-colab fora das classes filtradas | Por quê | Evidência |
|---|---|---|
| col258 / 2026-10-30 | Única pergunta, pk 52131, tem marco True | [nao verificado] |
| col258 / 2026-11-06 | Única pergunta, pk 54827, tem marco True | [nao verificado] |
| col258 / 2026-11-09 | Única pergunta, pk 56040, tem marco True | [nao verificado] |
| col427 / 2026-09-21 | Não tem pergunta; motivos_a_perguntar também está vazio, portanto não entra na classe própria solicitada | [nao verificado] |

Considerando **todas as seis classes originais do JSON**, inclusive as de marco aceso, a soma é `5 + 4 + 4 + 8 + 1 + 2 = 24` participações, cobrindo **14 dias-colab distintos** por haver sobreposições. Nesse conjunto original, o único dia fora de qualquer classe é **col427 / 2026-09-21**, sem perguntas. [nao verificado]

## 12/11 11:46 · sessao ca639e2b122d · - · - · origem=Ronald (tmux)

Âncoras no código, somente leitura:

| Campo/regra | Arquivo:linha |
|---|---|
| `pergunta_viva` | `app/chamados/juizes.py:862`; cálculo delegado em `app/chamados/juizes.py:1389` |
| `validada_em` preenchida encerra a pergunta nesse cálculo | `app/chamados/juizes.py:1391` |
| `fato_do_marco_no_espelho`: `True`, `False` ou `None`; `None` significa impossibilidade de afirmar | `app/chamados/juizes.py:930` |
| Mapa motivo → marco | `app/chamados/juizes.py:922`: `saida_sem_entrada`→`hi`; `orfao_14h`→`hf`; `intervalo_saida`→`hii`; `intervalo_volta`→`hfi` |
| Leitura das lâmpadas e resultado do marco | `app/chamados/juizes.py:965` |
| `motivos_a_perguntar` | `app/chamados/services/fabrica_da_celula.py:92`; retorno exclui os motivos já representados no domínio do fato em `app/chamados/services/fabrica_da_celula.py:154` |
| `porta_alcancavel_do_dia` | `app/chamados/juizes.py:1612`; retorno `rota`, `fila`, `verbos`, `fio` em `app/chamados/juizes.py:1657` |
| Significado de `fila`, `verbos` e `fio` | `app/chamados/juizes.py:1621` |
| `respondida_em` / `validada_em` | `app/chamados/models.py:2449` / `app/chamados/models.py:2450` |
| `validada_por` | `app/chamados/models.py:3378` |
| `via_resolucao` | `app/chamados/models.py:2562` |
| Auditoria: `usuario`, payload antes/depois, `criado_em` | `app/core/models.py:295`, `app/core/models.py:300`, `app/core/models.py:301`, `app/core/models.py:307` |
| `dono_fora_do_fio` | `app/chamados/services/vigia_fio.py:274`; ramos em `app/chamados/services/vigia_fio.py:306`; retorno sem dono em `app/chamados/services/vigia_fio.py:315` |

O mapeamento exato do campo raspado `dono_fora_do_fio_chamado` à função encontrada está [nao verificado].

## 12/11 11:46 · sessao e902d776f71d · FIO-E0-V4-E3-LEITORES · verificador · origem=Code

BLOQUEIA — a migração está parcial: permanecem respondedores próprios, há divergência entre sombra e vigia, e silêncio vencido continua com dois significados. (`app/chamados/services/raiox.py:139`, `app/chamados/management/commands/diff_e3_sombra.py:54`, `app/chamados/services/vigia_fio.py:650`, `app/chamados/services/verbos.py:51`)

FILA — identidade: HEAD `513ff80771b84636b5b485452a74058b2a507ebb` e comparação Git com `087fe758a`: [nao verificado]; a cópia disponibilizada não contém `.git`. Execução ORM, suite e mortes dos mutantes: [nao verificado]. As reproduções abaixo usaram somente funções extraídas por AST, em memória, com entradas sintéticas. [nao verificado]

- FILA — **item 1, sonda/soma:** existe registro histórico de **232**, reconciliado como 118+114; não é resultado certificado desta volta. Sonda atual e inclusão dos mesmos 232 sítios/dias: [nao verificado]. RED de cura não se aplica à medição. (`app/docs/RELATO.md:249`)

- FILA — **item 2, V/T/X:** há classificações históricas diferentes: V=3/T=228/X=1 e, posteriormente, V=15/T=0/X=217 distribuído em subclasses. Classificação válida para este HEAD: [nao verificado]; não escolher uma delas como resultado atual. Sem RED de cura aplicável. (`app/docs/RELATO.md:235`, `app/docs/RELATO.md:237`, `app/docs/RELATO.md:238`, `app/docs/RELATO.md:239`, `app/docs/RELATO.md:240`, `app/docs/RELATO.md:241`)

- FILA — **item 3, V curada ou zero:** a árvore contém exposição administrativa pela vida C6 e pela decisão E1; esses corpos não são alterados pelo diff fornecido. Cura dos V reais, quantidade residual e RED correspondente nesta volta: [nao verificado]. (`app/chamados/services/tarefa_do_dia.py:243`, `app/chamados/services/tarefa_do_dia.py:314`)

- FILA — **item 4, chamadores C17:** **nada de defeito local demonstrado na substituição**: folha, vigia, beco e pauta consultam a porta da tarefa. Há casos que separam C17 de C19; retornar ao C17 contraria as expectativas desses testes. Mutantes executados por leitor: [nao verificado]. (`app/folha/fora_da_folha.py:72`, `app/chamados/services/vigia_fio.py:638`, `app/escala/services/cadastro_realidade.py:497`, `app/ponto/services/pautas_dos_discordantes.py:81`, `app/chamados/tests/test_e3_leitores_c17_chamadores.py:124`, `app/chamados/tests/test_e3_leitores_c17_chamadores.py:154`, `app/chamados/tests/test_e3_leitores_c17_chamadores.py:170`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:166`)

- BLOQUEIA — **item 5, cinco respondedores:** não morreram: crédito, relatório, cobrança e template continuam respondendo por critérios próprios; o fio ainda consome crédito/desenlace. Contraexemplo: timestamp preenchido com resposta vazia produz “aguarda supervisão”, enquanto C19 exige RESPONDER do colaborador. Certidão e RED/mutante por cada sítio restante: [nao verificado]. (`app/chamados/services/raiox.py:139`, `app/chamados/relatorios_disputa.py:21`, `app/chamados/services/cobranca.py:229`, `app/templates/chamados/partials/_card_atendimento.html:94`, `app/chamados/services/fio.py:405`, `app/chamados/services/tarefa_do_dia.py:137`)

- BLOQUEIA — **item 6, catraca/paridade:** a catraca verifica somente chamadas C17; não certifica zero respondedores nem zero derivações próprias. Contraexemplo: `creditos_perguntas` permanece decidindo por timestamp sem infringir essa contagem. A injeção sintética testa retorno ao C17, não esses respondedores. Paridade executada: [nao verificado]. (`app/chamados/tests/test_e3_pendentes_app.py:33`, `app/chamados/tests/test_e3_pendentes_app.py:145`, `app/chamados/tests/test_e3_pendentes_app.py:150`, `app/chamados/services/raiox.py:139`)

- BLOQUEIA — **item 7, DIFF da sombra:** a projeção do instrumento ficou diferente da produção. Para `verbos={âncora: 'validar'}`, `fio='colab'`, a sombra informa `exige_admin=True`; o vigia informa False. É exatamente a combinação coberta pelo caso de resposta vazia; o teste da sombra conserva o cálculo anterior. Queda real por classe: [nao verificado]. (`app/chamados/management/commands/diff_e3_sombra.py:54`, `app/chamados/services/vigia_fio.py:650`, `app/chamados/tests/test_o261e_f5_alcance.py:177`, `app/chamados/tests/test_e3_diff_sombra.py:143`)

- FILA — **item 8, suite inteira:** resultado desta árvore [nao verificado]; não há certificação de execução no material fornecido. Sem sítio de cura/RED próprio: é etapa de execução. [nao verificado] [nao verificado]

- FILA — **item 9, opus final:** aprovação desta árvore [nao verificado]. Sem sítio de cura/RED próprio. [nao verificado]

- FILA — **item 10, pouso:** pouso deste HEAD [nao verificado]. Sem sítio de cura/RED próprio. [nao verificado] [nao verificado]

- FILA — **item 11, cache por disputa:** **nada no caso aberto multicartão demonstrado pelo RED**: o mapa agora é por disputa e alimenta o cache correspondente. Reverter para perguntas do cartão contraria os asserts de resumo e universo; morte executada [nao verificado]. Limite restante: disputa **fechada** com uma viva e uma validada entrega somente a viva ao explicador, porque as mortas são excluídas antes do mapa; portanto “disputa inteira” não vale literalmente nesse ramo. (`app/chamados/services/fio.py:257`, `app/chamados/services/fio.py:330`, `app/chamados/tests/test_e3_leitores_fio_disputa.py:95`, `app/chamados/tests/test_e3_leitores_fio_disputa.py:105`, `app/chamados/services/fio.py:241`, `app/chamados/services/fio.py:255`)

- FILA — **item 12, aviso sem anuência:** **nada de defeito local demonstrado na cura**: web e API usam `aguarda_o_colaborador`, recortando perguntas sem resposta/texto e consultando C15/C6. Reverter ao timestamp cru contraria o caso morta por célula e o caso viva sem texto. Execução/mutante morto [nao verificado]. (`app/chamados/services/fio.py:163`, `app/chamados/services/fio.py:358`, `app/api/views.py:1175`, `app/chamados/tests/test_e3_leitores_fio_disputa.py:125`, `app/chamados/tests/test_e3_leitores_fio_disputa.py:143`, `app/chamados/tests/test_e3_leitores_fio_disputa.py:156`)

- FILA — **bloqueante #4, relógio:** **nada na cura direta de `alimentar_portas`**: C6 agora recebe `agora`; C19 já o passa em sua carga. Retirar o argumento contraria o RED e a verificação de repasse. `fio_vivo` continua sem parâmetro temporal, mas não é chamado diretamente pelo corpo C19 conferido. Execução/mutante morto [nao verificado]. (`app/chamados/juizes.py:1575`, `app/chamados/services/tarefa_do_dia.py:493`, `app/chamados/services/tarefa_do_dia.py:533`, `app/chamados/tests/test_e3_bloqueantes.py:110`, `app/chamados/tests/test_e3_bloqueantes.py:135`, `app/chamados/juizes.py:1494`)

- FILA — **bloqueante #8, furo sem canal:** **nada de derivação crua restante no trecho curado**: mede a porta publicada pela tarefa. Repor `VIVOS`/timestamp nulo contraria os casos chamado sem ação, âncora silenciada e registrada; existe controle positivo com porta. Execução/mutante morto [nao verificado]. (`app/chamados/services/furo_sem_canal.py:90`, `app/chamados/services/furo_sem_canal.py:100`, `app/chamados/tests/test_e3_bloqueantes.py:148`, `app/chamados/tests/test_e3_bloqueantes.py:155`, `app/chamados/tests/test_e3_bloqueantes.py:164`, `app/chamados/tests/test_e3_bloqueantes.py:171`)

- BLOQUEIA — **bloqueante #9, silêncio:** o filtro comum foi colocado no modelo, não em C19, e a lista real do admin continua divergindo. Exemplo: chamado aberto com `silenciado_ate=agora−1min` volta ao queryset e à lista do colaborador, mas vira `HISTORICO` e desaparece do painel padrão. O RED morde a reversão para `IS NULL`, porém só verifica os dois querysets; não morde essa divergência de apresentação. (`app/chamados/models.py:524`, `app/chamados/services/painel.py:190`, `app/chamados/services/janela_colab.py:78`, `app/chamados/services/verbos.py:51`, `app/chamados/services/painel.py:324`, `app/chamados/tests/test_e3_b4_silencio_vencido.py:35`)

- BLOQUEIA — **#9, porta aparentemente alcançável:** com pergunta exigível ancorada no chamado vencido da célula, a tarefa pode publicar `rota='porta_fila'`, `fila=()`, `verbos={âncora: 'historico'}`. `portas` conserva o chamado; somente `fila` o exclui. O consumidor recebe uma porta cujo chamado foi ocultado na lista administrativa. (`app/chamados/juizes.py:1533`, `app/chamados/services/tarefa_do_dia.py:548`, `app/chamados/services/tarefa_do_dia.py:550`, `app/chamados/services/tarefa_do_dia.py:551`, `app/chamados/services/tarefa_do_dia.py:555`, `app/chamados/services/painel.py:331`)

- BLOQUEIA — **segundo juiz de fechamento, fora do diff e alcançado pelo item 11:** o explicador calcula `pode_fechar` somente como `not fio_vivo`, omitindo a guarda C7 de resposta sem veredito. Exemplo: resposta com texto, sem validação/materialização, célula `ok` decidida pelo admin, dia não trancado: C6 pode dar morta, explicador diz “Tudo resolvido”, mas C7 recusa fechar. (`app/chamados/services/explicador.py:137`, `app/chamados/services/explicador.py:140`, `app/chamados/juizes.py:1404`, `app/chamados/juizes.py:1452`, `app/chamados/models.py:1855`, `app/chamados/models.py:1866`, `app/chamados/models.py:1899`)

- BLOQUEIA — **mutante sem alvo:** `diff_fachada_vazia` procura o antigo corpo de `candidata`; agora existe um alias. O harness lança “Mutante sem alvo” antes de executar teste. Esse RC não comprova mordida do RED. (`app/chamados/tests/e3_mutantes.py:112`, `app/chamados/management/commands/diff_e3_sombra.py:43`, `app/chamados/tests/e3_mutantes.py:162`)

- BLOQUEIA — **mutante temporal equivalente:** `alimentacao_sem_agora` substitui somente a primeira ocorrência de `agora=self.agora)`: atinge C6 em `_ler_vida`, deixando `alimentar_fila` intacta. C6 recupera o mesmo instante de `carga.agora`; portanto o mutante não reverte a alimentação pretendida. (`app/chamados/tests/e3_mutantes.py:101`, `app/chamados/tests/e3_mutantes.py:164`, `app/chamados/services/tarefa_do_dia.py:493`, `app/chamados/services/tarefa_do_dia.py:533`, `app/chamados/juizes.py:883`)

- BLOQUEIA — **leitor esquecido: API/app/badges/PWA:** a consulta ainda manda qualquer pergunta com timestamp para histórico. Exemplo: resposta vazia com timestamp deixa de contar como pendência no app/dashboard, enquanto C19 exige RESPONDER. Os dois kernels consomem essa projeção antiga; a alteração da correção do cartão não migra esses consumidores. (`app/chamados/services/pergunta_no_app.py:71`, `app/chamados/services/pergunta_no_app.py:91`, `app/chamados/services/tarefa_do_dia.py:137`, `app/api/views.py:315`, `app/api/views_core.py:303`, `app/api/views.py:993`, `app/api/views.py:2533`, `app/core/views.py:252`)

- BLOQUEIA — **leitor esquecido: fio/admin/ficha:** crédito por timestamp alimenta verbo e desenlace; ficha traduz esse verbo em dono. No mesmo caso vazio, oferecem validação/admin enquanto a tarefa exige resposta do colaborador. (`app/chamados/services/raiox.py:139`, `app/chamados/services/verbos.py:34`, `app/chamados/catalogo/natureza.py:48`, `app/chamados/services/fio.py:421`, `app/chamados/services/desenlace.py:72`, `app/colaboradores/services/ficha.py:159`, `app/chamados/services/tarefa_do_dia.py:137`)

- BLOQUEIA — **leitor esquecido: cobrança:** `_calados` ainda usa pré-filtro `q_vivas`, timestamp nulo e disputa aberta. Pergunta com resposta vazia e timestamp deixa de ser cobrada por esse leitor, embora C19 ainda exija RESPONDER; pergunta morta por tranca também não é eliminada por uma consulta efetiva ao C6 nesse sítio. (`app/chamados/services/cobranca.py:229`, `app/chamados/services/tarefa_do_dia.py:137`, `app/chamados/juizes.py:1395`)

- BLOQUEIA — **leitor esquecido: calendário:** exclui resposta vazia antes de consultar C6 e deriva `pode_ultimo_recurso` separadamente. Exemplo: pergunta viva com resposta vazia em competência passada não bloqueia essa oferta, enquanto C19 ainda publica RESPONDER. (`app/colaboradores/services/calendario.py:474`, `app/colaboradores/services/calendario.py:476`, `app/colaboradores/services/calendario.py:738`, `app/chamados/services/tarefa_do_dia.py:137`)

- FILA — **leitor esquecido: copiloto/navbar:** copiloto continua expondo C6 e `pergunta_no_app.do_dia`, sem a tarefa; navbar continua contando chamados da fila. Exemplo: chamado na fila sem pergunta pode contar no navbar, enquanto C19 tem apenas ação do sistema e nenhuma porta humana. A navbar usa unidade diferente de perguntas; não equiparar numericamente os badges. (`app/ponto/services/dia_do_colab.py:71`, `app/ponto/services/dia_do_colab.py:108`, `app/core/context_processors.py:185`, `app/chamados/tests/test_e3_leitores_c17_chamadores.py:101`)

- BLOQUEIA — **campo `respondida_em`: significados incompatíveis entre leitores.** É instante de resposta humana, automática ou recuperada da trilha; também pode ser preenchido por `carimbar` e limpo na reabertura. C6 recorta pelo instante; C19 lê a decisão; crédito/app/relatório ainda interpretam presença como resposta suficiente. Contraexemplo comum: timestamp com texto vazio. (`app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1653`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/models.py:3145`, `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/juizes.py:1380`, `app/chamados/services/tarefa_do_dia.py:135`, `app/chamados/services/raiox.py:139`, `app/chamados/services/pergunta_no_app.py:71`, `app/chamados/relatorios_disputa.py:21`)

- BLOQUEIA — **campo `silenciado_ate`: escritores e fronteira estão identificados, leitores discordam.** `arquivo.silenciar` grava; `arquivo.levantar` limpa. O modelo define silêncio vigente por `ate > agora`: igualdade já venceu. Partição/painel conservam significado histórico permanente para prazo preenchido vencido, produzindo o contraexemplo #9. (`app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:202`, `app/chamados/models.py:512`, `app/chamados/models.py:524`, `app/chamados/services/verbos.py:51`, `app/chamados/services/painel.py:331`)

- FILA — **campo `data_evento`: dois significados discriminados.** Fato usa dia do turno; conversa usa dia civil. Nascimento, costura e rechaveamento passam pela atribuição em `gravar_dia_do_fato`; C19 exclui conversas e filtra datas inclusivamente. Mistura material adicional demonstrada: **nada**; legado vivo com data NULL [nao verificado], portanto a condição de triagem #2 continua sem medida. (`app/chamados/models.py:2796`, `app/chamados/models.py:2808`, `app/chamados/models.py:3029`, `app/chamados/services/fio.py:819`, `app/chamados/services/fio.py:898`, `app/chamados/services/fio.py:1056`, `app/chamados/services/tarefa_do_dia.py:375`)

- FILA — **campo `status_local`: nada de segundo escritor persistente demonstrado nos caminhos conferidos.** Gravação de transição em `_gravar_estado`; nascimento pela abertura. As outras atribuições indicadas simulam/restauram memória. Leitores usam universos distintos: VIVOS para crédito, COBRANCA para fila, fila para portas; chamado vivo não equivale a tarefa exigível. (`app/chamados/models.py:228`, `app/chamados/models.py:1108`, `app/chamados/models.py:356`, `app/chamados/models.py:371`, `app/chamados/services/raiox.py:139`, `app/chamados/services/painel.py:189`, `app/chamados/juizes.py:1533`)

- BLOQUEIA — **valores `pode_fechar` e `porta`:** não são campos persistentes nesses trechos. C7 calcula fechamento; o explicador escreve uma resposta paralela incompleta. C19 escreve rota/fila/verbos/fio em memória; `porta_da_tarefa` somente projeta, e folha/vigia/beco/pauta leem essa saída. Os contraexemplos são a guarda C7 omitida e a porta com fila vazia descritas acima. (`app/chamados/models.py:1866`, `app/chamados/services/explicador.py:163`, `app/chamados/services/tarefa_do_dia.py:555`, `app/chamados/services/tarefa_do_dia.py:565`, `app/folha/fora_da_folha.py:75`, `app/chamados/services/vigia_fio.py:638`, `app/escala/services/cadastro_realidade.py:497`, `app/ponto/services/pautas_dos_discordantes.py:81`)

- FILA — **relógios: não existe uniformidade completa.** Nas bordas, vigia e lavrador do retrato capturam um instante; C19/C6 usam dia civil local e incluem fatos com instante `<= agora`. Porém `censo_alcance(agora=..., hoje=None)` lê o dia da máquina; `chamados_visiveis_colab(agora=...)` também calcula sua janela sem derivá-la desse instante. Exemplo: consultar outro mês com apenas `agora` mistura janela da máquina com silêncio/tarefa do instante recebido. (`app/chamados/management/commands/vigiar_fio_morto.py:166`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/chamados/services/tarefa_do_dia.py:393`, `app/chamados/veredito_lavrado.py:104`, `app/chamados/services/vigia_fio.py:567`, `app/chamados/services/janela_colab.py:42`, `app/chamados/services/janela_colab.py:58`)

- FILA — **ordem de trava: nada novo demonstrado nos caminhos conferidos.** Silêncio chama a porta de ordem; rechaveamento trava antes da escrita; o núcleo implementa C→D→CH→P. Ausência global de inversão por execução/reflexão [nao verificado]. (`app/chamados/services/arquivo.py:170`, `app/chamados/services/arquivo.py:209`, `app/chamados/services/fio.py:1025`, `app/chamados/models.py:2679`, `app/chamados/models.py:2688`, `app/chamados/models.py:2701`, `app/chamados/models.py:2705`)

- FILA — **limite escritor/cascata:** permanece a condição de `post_save` que trata qualquer silêncio preenchido como motivo para não reprocessar isento, inclusive vencido. Exemplo: entrada em estado vivo com prazo vencido retorna antes do processamento. É lado escritor a registrar no censo, não prova de cura pelo novo filtro. (`app/chamados/signals.py:285`, `app/chamados/signals.py:299`)

- FILA — **classe A/B:** no diff fornecido, **classe B**: nenhum hunk em static/JS, service worker ou template base. Conferência independente pelo Git [nao verificado].

- FILA — **fora do escopo:** **nada demonstrado** como alteração de produto alheia aos avais; propagação de `agora` alcança os chamadores dos serviços migrados. Portão de arquivos contra o diff separado da E4B c10, vaga/teto e marcos desta volta no RELATO: [nao verificado]. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:78`, `app/folha/retrato_folha.py:1158`, `app/ponto/management/commands/pautas_discordantes.py:68`)

## 12/11 11:44 · sessao f335ea201ffa · PORTA-QUE-NAO-AGE · censo · origem=Code

perguntas de lei: **8 — D1–D8 abaixo, sem escolher entre as leituras existentes.**

O código permite que uma pergunta deixe de estar viva **antes da conferência do marco cobrado**: por validação, tranca, identidade ou absolvição do dia. A porta `carimbar` consulta o C6 depois de aplicar os carimbos em memória; com `resposta='validar'`, a própria validação recém-aplicada pode determinar a morte (`app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1395`, `app/chamados/juizes.py:1428`, `app/chamados/models.py:3131`, `app/chamados/models.py:3140`).

O dado raspado informa os 15 dias, mas não traz os campos das perguntas, autores, instantes, atas ou status D4. Portanto, **a classe de morte de cada um dos 15 e o veto concreto do col427 em 21/09 permanecem [nao verificado]**. Os testes citados abaixo foram lidos; execução e resultado atual: [nao verificado]. [nao verificado]

**Convenções da matriz**

| Sigla | Campos |
|---|---|
| C | `materializada_em`, `materializada_por`, `via_resolucao` (`app/chamados/models.py:3129`). |
| R | `resposta_colab`, `respondida_em` (`app/chamados/services/disputa_emissao.py:1467`). |
| V | `validada_em`, `validada_por` (`app/chamados/services/validacao.py:134`). |
| L | `veredito_lavrado`, `julgado_em`, `janela_expira_em` (`app/chamados/models.py:3150`). |

**1. Escritores encontrados por AST e grep**

A varredura usou em memória o detector AST existente, ampliando os campos pesquisados. O detector acompanha atribuições, `setattr`, construtores, dicionários propagados, `update` e `bulk_update`; declara limitações para `__dict__`, nomes vindos de outro módulo e efeitos de `SET_NULL` (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:99`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:111`). Universo informado pela execução: 767 arquivos Python de produção em `app`, sem testes/migrations/assets/logs, zero falhas de parse; artefato persistido dessa execução: [nao verificado].

| Escritor físico | Campos e persistência |
|---|---|
| `_escrever_carimbo` | Atribui C; quem persiste são `carimbar`, `carimbar_identidade` e `descarimbar`, por `save(update_fields=campos)` (`app/chamados/models.py:3098`, `app/chamados/models.py:3153`, `app/chamados/models.py:3191`, `app/chamados/models.py:3213`). |
| `carimbar` | Além de C, completa `respondida_em` e V nulos quando há texto e o parâmetro pede responder/validar; pode persistir L no mesmo `save` (`app/chamados/models.py:3131`, `app/chamados/models.py:3150`). |
| `descarimbar_em_lote` | `PerguntaDisputa.objects…exclude(…FORA_DO_FATO).update(...)`: limpa C e os campos recebidos em `**outros` (`app/chamados/models.py:3253`, `app/chamados/models.py:3265`). |
| `encerrar_por_fato_no_espelho` | Atribui e salva apenas `via_resolucao='fato_materializado'` (`app/chamados/models.py:3276`). |
| `relavra_via.relavrar` | Atribui a nova via, relavra e salva `via_resolucao` + L (`app/chamados/services/relavra_via.py:125`). |
| `_carimbar_validacao` | Atribui V, salva os dois campos e registra `DecisaoPergunta.VALIDACAO` (`app/chamados/services/validacao.py:134`, `app/chamados/services/validacao.py:139`). |
| `devolver_validacao_recusada` | Limpa V, salva e registra `DEVOLUCAO` (`app/chamados/services/validacao.py:199`, `app/chamados/services/validacao.py:205`). |
| `responder_pergunta_disputa_core` | Atribui R; limpa V anterior; incorpora eventual descarimbo por resposta; salva em transação (`app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/services/disputa_emissao.py:1485`, `app/chamados/services/disputa_emissao.py:1508`). |
| `responder_automaticamente` | Atribui/salva R e registra `RESPOSTA` (`app/chamados/services/disputa_emissao.py:1652`). |
| `carimbar_resposta_pela_trilha` | Atribui/salva `respondida_em` e registra `RESPOSTA` com o instante recebido (`app/chamados/services/disputa_emissao.py:1674`). |
| `reabrir_para_correcao` | Monta limpeza de R/V; usa `qs.update(**campos)` ou `descarimbar_em_lote`, conforme `desfazer_absorcao` (`app/chamados/services/disputa_emissao.py:1593`, `app/chamados/services/disputa_emissao.py:1629`). |
| `ressuscitar_perguntas` | Passa `respondida_em=None`, `validada_em=None`, `validada_por=None` para `descarimbar_em_lote` (`app/chamados/models.py:1567`). |
| Nascimento `_nascer` | Recebe `**campos`, constrói a pergunta e faz `save(force_insert=True)`; assinaturas R/V recebidas são conservadas e viram atos da sequência (`app/chamados/models.py:3015`, `app/chamados/models.py:3018`). |
| Efeito declarativo da FK | `validada_por` admite `SET_NULL` ao apagar o usuário; não é atribuição explícita de um serviço de pergunta (`app/chamados/models.py:3378`). |

As atribuições em `_universo_da_ressuscitacao` e `_desfazer_em_memoria` servem à simulação/restauração em memória, sem persistência própria (`app/chamados/models.py:1507`, `app/chamados/models.py:3160`).

Não apareceu outro `bulk_update` dos quatro campos no universo varrido; garantia fora dos limites declarados do detector: [nao verificado]. `flip_auto` é leitura por `filter(via_resolucao=...)`, não escritor (`app/ponto/services/flip_auto.py:128`).

Os apelidos/reexports relevantes não constituem escritores adicionais:

- `_reabrir`: `app/chamados/models.py:2220` e `app/chamados/services/acoes_disputa.py:102`; `_reabrir2`: `app/chamados/views.py:1435`.
- Reexports dos corpos de materialização e emissão por `chamados.models`: `app/chamados/models.py:4088`, `app/chamados/models.py:4093`.
- `PerguntaDisputa` como `_PD`, `_PD_ordem` e `_PD_inf`: por exemplo, `app/chamados/services/disputa_emissao.py:1501`, `app/chamados/services/validacao.py:428`, `app/chamados/services/painel.py:260`.
- Juízes importados com apelido no materializador: `declarou_hora as _declarou`, `via_pelo_fato as _via_fato`, `fato_do_marco_no_espelho as _lampada_do_marco` (`app/chamados/services/materializacao.py:1267`, `app/chamados/services/materializacao.py:1270`, `app/chamados/services/materializacao.py:906`).

**2. Matriz das operações que encerram ou carimbam encerramento**

`carimbar` não equivale a “exigiu marco aceso”: aplica C e, opcionalmente, resposta/validação; consulta C6; se viva, restaura e retorna `False`; se morta, salva e retorna `True` (`app/chamados/models.py:3130`, `app/chamados/models.py:3140`, `app/chamados/models.py:3153`). `carimbar_identidade` grava sem consultar C6; o julgamento fica no chamador (`app/chamados/models.py:3169`).

| Via / entrada | Juiz → operação | Persistido → retorno | Prova que exercita |
|---|---|---|---|
| **Validação**, resposta recebida, ainda não validada | Guardas de resposta/implausibilidade → `_carimbar_validacao`; depois materialização (`app/chamados/services/validacao.py:393`, `app/chamados/services/validacao.py:404`, `app/chamados/services/validacao.py:435`). | V + ato `VALIDACAO`; recusa de hora declarada pode desfazer V. Retorna `validada`, `ja_no_espelho` ou `nao_gravou` (`app/chamados/services/validacao.py:461`, `app/chamados/services/validacao.py:499`, `app/chamados/services/validacao.py:515`). | C6 mata validada sem exigir prova do marco: `app/chamados/tests/test_pergunta_viva.py:17`. Fecho sem batida no slot: `app/chamados/tests/test_fechamento_unificado.py:74`. |
| **`admin_decidiu`**, dia decidido | `q_vivas` do dia → `fechar_por_admin` → identidade (`app/ponto/services/veredito_celula.py:180`, `app/ponto/services/veredito_celula.py:190`). | C + ato `SILENCIO`; helper retorna booleano (`app/ponto/services/veredito_celula.py:195`, `app/chamados/models.py:3065`). | `app/chamados/tests/test_e1_v5_portas_backfill.py:83`. |
| **Silêncio / desligamento**, disputa com perguntas vivas | `respostas_sem_veredito` barra resposta sem julgamento; portas `admin_encerra/desligamento` percorrem vivas e chamam `fechar_por_admin` (`app/chamados/models.py:1887`, `app/chamados/models.py:1919`). | C nas perguntas; ato `SILENCIO`; fecho/autor/resolução/motivo na disputa. `True`; repetição idêntica, `False` (`app/chamados/models.py:1925`, `app/chamados/models.py:1933`, `app/chamados/models.py:1880`). | `app/chamados/tests/test_e4_expand_orm.py:63`. |
| **Folga aceita → `admin_decidiu`** | `o_dia_ja_e_folga` ou `checar_declaracao` → validação e decisão de folga (`app/chamados/services/validacao.py:280`, `app/chamados/services/validacao.py:305`). | V + C; retorna `folga_ja_declarada` ou `folga_declarada` (`app/chamados/services/validacao.py:288`, `app/chamados/services/validacao.py:302`, `app/chamados/services/validacao.py:320`, `app/chamados/services/validacao.py:367`). | Teste específico localizado, corpo não aberto: [nao verificado]. |
| **`motivo_retratado`**, motivo removido | Guarda de motivo/disputa → `remover_motivo` → identidade (`app/chamados/models.py:2034`, `app/chamados/models.py:2062`). | Ato `RETRATACAO` + C, preservando carimbo anterior quando existente; retorna `removido` (`app/chamados/models.py:2057`, `app/chamados/models.py:2068`). | Preserva resposta e tira de viva: `app/chamados/tests/test_e1_entradas.py:142`. |
| **`duplicata`**, linha perdedora do fato | `plano_de_retratacao` → `retratar_duplicata` → identidade (`app/chamados/services/fio.py:1341`, `app/chamados/services/fio.py:1346`). | C, conservando instante/autor anterior; `recusa_motivo` aponta canônica. `True`; já duplicata, `False` (`app/chamados/services/fio.py:1351`, `app/chamados/services/fio.py:1360`). | Apply/idempotência: `app/chamados/tests/test_e1_v5_portas_backfill.py:207`; veto de mérito: `app/chamados/tests/test_e1_v9.py:210`. |
| **`fato_ocupado`**, colisão de tipo oposto | `traduzir_recusa`; código `COLISAO` e sem materialização → identidade, sem teste local da lâmpada (`app/chamados/services/materializacao.py:1097`). | C + motivo, eventualmente nota no destino; retorno implícito `None` (`app/chamados/services/materializacao.py:1105`, `app/chamados/services/materializacao.py:1113`). | `app/chamados/tests/test_a27_recusa_com_dono.py:47`. |
| **`paridade_recusada`**, vizinha do mesmo tipo | **Exige lâmpada `is True`**; depois identidade (`app/chamados/services/materializacao.py:906`, `app/chamados/services/materializacao.py:930`). | C; retorna `None`. Apagada/indeterminada registra recusa, sem C (`app/chamados/services/materializacao.py:919`, `app/chamados/services/materializacao.py:931`). | Três valores da lâmpada: `app/chamados/tests/test_o261_paridade_respeita_a_lampada.py:174`, `:183`, `:192`. |
| **`celula_absolveu`**, pergunta absolvida pelo dia | `perguntas_absolvidas_pela_celula` → `carimbar`/C6 (`app/chamados/reconciliador.py:585`, `app/chamados/reconciliador.py:596`). | C; incrementa quantidade e pode fechar disputa pelo C7; retorna quantidade (`app/chamados/reconciliador.py:599`, `app/chamados/reconciliador.py:612`, `app/chamados/reconciliador.py:618`). | Consumidor: `app/chamados/tests/test_jv3_reconciliar_vira_vigia.py:147`; teto bloqueia carimbo: `app/chamados/tests/test_o261_e2_absolvicao_exige_fato_encerrado.py:62`. |
| **`sem_turno_aberto`**, motivo admitido, sem texto/materialização | Núcleo sem turno aberto; C6 antes e dentro de `carimbar` (`app/chamados/reconciliador.py:363`, `app/chamados/reconciliador.py:398`, `app/chamados/reconciliador.py:406`). | C; eventual fecho C7; retorna `True` quando lavrou (`app/chamados/reconciliador.py:412`, `app/chamados/reconciliador.py:415`). | **Pergunta previamente validada, marco apagado, via persistida e disputa fechada:** `app/chamados/tests/test_o261_lavra_le_a_lampada.py:148`. |
| **`fantasma`**, órfã atual | `veredito_orfa` exige marco aceso, C6 viva e teto encerrado → consumidor → `encerrar_fantasma`/`carimbar` (`app/chamados/reconciliador.py:1083`, `app/chamados/reconciliador.py:1108`, `app/chamados/models.py:3288`). | C; booleano do helper; consumidor devolve contagens (`app/chamados/models.py:3285`, `app/chamados/reconciliador.py:1113`). | Recusa viva respondida e aceita aprovada: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:665`, `:676`. |
| **`juiz_aprovou`**, pendente após fecho da disputa | Lastro aprova; C6 também precisa aceitar o carimbo (`app/chamados/services/materializacao.py:1433`). | C; devolve lista dos PKs carimbados (`app/chamados/services/materializacao.py:1437`). | Resposta por validar não sai: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:687`. |
| **`declaracao`**, aplicação da resposta | Materializa/reutiliza efeito; para hora/data_hora exige lastro; escolhe via por `_declarou`; consulta C6 com V aplicada (`app/chamados/services/materializacao.py:1238`, `app/chamados/services/materializacao.py:1267`, `app/chamados/services/materializacao.py:1283`). | C + complementos R/V + L; C6 recusando provoca rollback; unidade marca sucesso (`app/chamados/services/materializacao.py:1283`, `app/chamados/services/materializacao.py:1288`). | Plantio real: `app/chamados/tests/test_fechamento_unificado.py:90`. Absorção também pode dar essa via sem plantio próprio: `app/chamados/tests/test_hx_via_ausencia_dia.py:104`. |
| **`ausencia_dia`**, escape/irmãs absorvidas | `declarou_hora` distingue declaração de escape; `carimbar(...resposta='validar')` consulta C6 (`app/chamados/services/materializacao.py:548`, `app/chamados/services/materializacao.py:555`, `app/chamados/services/materializacao.py:567`). | C, complementos R/V conforme texto; ato `ABSORCAO` se gravou (`app/chamados/services/materializacao.py:569`). Retorno integral do serviço: [nao verificado]. | Irmã muda continua sem resposta e recebe a via: `app/chamados/tests/test_hx_via_ausencia_dia.py:78`; escape: `:90`. |
| **`completude_turno`** | `via_pelo_fato`: TM com saída e motivo fora de intervalo → S128 ou materializador (`app/chamados/juizes.py:1754`, `app/chamados/services/disputa_emissao.py:1986`, `app/chamados/services/materializacao.py:1270`). | Pelo S128: C, complementos R/V e L; retorna contagens de perguntas/disputas (`app/chamados/services/disputa_emissao.py:1986`, `app/chamados/services/disputa_emissao.py:2094`). | S128 com/sem batida no slot: `app/chamados/tests/test_fechamento_unificado.py:60`, `:74`. |
| **`adesao`** | Agrupado, sem batida apurável tocando o dia civil e pai não LIMBO-FOLGAS → `via_pelo_fato` → carimbo (`app/chamados/juizes.py:1713`, `app/chamados/juizes.py:1733`, `app/chamados/juizes.py:1740`). | Mesmos campos/retorno do S128 acima (`app/chamados/services/disputa_emissao.py:1986`, `app/chamados/services/disputa_emissao.py:2094`). | Teste exercitando especificamente essa seleção: [nao verificado]. |
| **`cadastro_folga`** | Mesmo ramo sem batida; pai `sem_previsao_escala/folga_nao_definida` → carimbo (`app/chamados/juizes.py:1738`). | Mesmos campos/retorno do S128 (`app/chamados/services/disputa_emissao.py:1986`, `app/chamados/services/disputa_emissao.py:2094`). | Teste exercitando especificamente essa seleção: [nao verificado]. |
| **`chamado_encerrado`** | Resto de `via_pelo_fato`; também ramo sem credor, após C6 já dar morta (`app/chamados/juizes.py:1757`, `app/chamados/services/disputa_emissao.py:2048`, `app/chamados/services/disputa_emissao.py:2058`). | C; no ramo sem credor completa apenas resposta, não valida; trilha `fecho_sem_credor`; retorno global em contagens (`app/chamados/services/disputa_emissao.py:2060`, `app/chamados/services/disputa_emissao.py:2063`, `app/chamados/services/disputa_emissao.py:2094`). | Sem texto fecha; com resposta viva recusa: `app/chamados/tests/test_o261_resposta_vazia.py:78`, `:103`. |
| **`fato_materializado`** | Comando exige lâmpada `is True` → `encerrar_por_fato_no_espelho` (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:41`). | Só via; `True`, ou `False` se já nessa via/fora do fato (`app/chamados/models.py:3274`). | Teste específico do comando: [nao verificado]. |
| **`competencia_trancada`** | Escritor antigo retirado; hoje C6 lê a tranca. A porta de identidade não aceita novo carimbo dessa via (`app/chamados/models.py:3067`, `app/chamados/models.py:3119`). | Nenhum campo novo; acervo antigo é descarimbado ao reabrir (`app/chamados/models.py:3318`). | Leitura sem escrita/destrancar: `app/chamados/tests/test_c7_red_minimos.py:361`, `:369`. |
| **`legado_sem_lastro` / via vazia** | Vocabulário/acervo lido; escritor atual atribuindo esses valores como encerramento não localizado: [nao verificado]. | `legado_sem_lastro` e `chamado_encerrado` pertencem a `VIAS_SEM_LASTRO`; resposta posterior pode descarimbar (`app/chamados/models.py:3766`, `app/chamados/models.py:3314`). | Prova específica de morte por essa via isolada: [nao verificado]. |
| **Re-lavra de via derivada** | `declarou_hora` ou `via_pelo_fato`; apenas vias anteriores `declaracao/completude_turno` (`app/chamados/services/relavra_via.py:77`, `app/chamados/services/relavra_via.py:95`, `app/chamados/services/relavra_via.py:118`). | Via + L; retorna `(n_via,n_ver)` (`app/chamados/services/relavra_via.py:125`, `app/chamados/services/relavra_via.py:139`). | Teste funcional específico: [nao verificado]. |

`respondida_em` sozinho **não é uma via de morte**: resposta com texto, sem validação, normalmente mantém a pergunta viva; as exceções anteriores na ordem do C6 continuam prevalecendo (`app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1402`, `app/chamados/juizes.py:1407`). As portas automática e de recuperação pela trilha gravam resposta; a validação posterior pertence a outro escritor (`app/chamados/services/disputa_emissao.py:1638`, `app/chamados/services/disputa_emissao.py:1661`).

**3. Saídas de viva que acontecem só por leitura**

Estas saídas não precisam de uma nova escrita nos quatro campos pesquisados (`app/chamados/juizes.py:1389`).

| Estado de entrada → juiz | Operação / persistência / retorno | Prova |
|---|---|---|
| V presente → primeiro degrau C6 | Não consulta marco; nenhuma escrita; `False` (`app/chamados/juizes.py:1391`). | `app/chamados/tests/test_pergunta_viva.py:17`. |
| Dia trancado, sem toque humano posterior → `dia_da_pergunta_trancado` | Nenhuma escrita; C6 `False` (`app/chamados/juizes.py:974`, `app/chamados/juizes.py:1395`). | `app/chamados/tests/test_c7_red_minimos.py:361`, `:391`. |
| `duplicata/motivo_retratado/tranca` com materialização | Vence até resposta existente; nenhuma escrita; `False` (`app/chamados/models.py:2525`, `app/chamados/juizes.py:1402`). | Duplicata respondida sai da fila: `app/chamados/tests/test_leitores_ouvem_a_lei.py:28`. |
| Demais identidades com materialização, alcançado o degrau | Nenhuma consulta necessária ao marco; `False` (`app/chamados/juizes.py:1427`). | `app/chamados/tests/test_pergunta_viva.py:31`. |
| Célula não acusadora, sem marcos pendentes | C6 absolve pelo dia; não exige positivamente a luz do marco nesse primeiro termo; `False` (`app/chamados/juizes.py:1438`, `app/chamados/juizes.py:1452`). | `app/chamados/tests/test_pergunta_viva.py:39`. |
| Materializada, via sem testemunha | Lastro aceita identidade, ausência de dia, fora da janela viva, ou ausência de falta; exceção da classificação também termina em `True`; C6 pode devolver `False` (`app/chamados/juizes.py:679`, `app/chamados/juizes.py:691`, `app/chamados/juizes.py:702`, `app/chamados/juizes.py:1473`). | Cobertura exata de todas essas condições: [nao verificado]. |
| `escolha` materializada | Lastro usa apenas `materializada_em`; não exige batida/lâmpada (`app/chamados/juizes.py:706`). | Teste específico desse ramo: [nao verificado]. |
| Hora/data_hora, slot não derivável | Lastro pode aceitar destino encerrado ou retratação com trilha; na janela viva tenta vetar pela falta (`app/chamados/juizes.py:765`, `app/chamados/juizes.py:786`, `app/chamados/juizes.py:797`). | Teste específico de cada fallback: [nao verificado]. |
| Hora/data_hora ou texto, últimos degraus | Lastro aceita batida viva no slot, retratação com trilha ou saída do turno; esses termos não exigem a ata do marco acesa (`app/chamados/juizes.py:752`, `app/chamados/juizes.py:812`, `app/chamados/juizes.py:817`). | Batida no slot: `app/chamados/tests/test_fechamento_unificado.py:48`; ocorrência com ata apagada: [nao verificado]. |
| Exceção ao co

[truncado em 20000 de 30259 caracteres]

## 12/11 11:44 · sessao 52942a8dad1b · PORTA-QUE-NAO-AGE · censo · origem=Code

```python
# Execucao e resultados em producao: [nao verificado].
# Molde: anexo sonda_medida3.py fornecido no pedido [nao verificado].
# Universo literal do pedido [nao verificado].
#
# Juizes chamados pelo censo:
# alimentar_portas: app/chamados/services/vigia_fio.py:604.
# porta_alcancavel_do_dia: app/chamados/services/vigia_fio.py:635.
# resolver_dia_oferecido_nas_datas, via _resolver_da_rodada:
# app/chamados/services/vigia_fio.py:265,636.
# dono_fora_do_fio, somente no ramo sem porta e passado:
# app/chamados/services/vigia_fio.py:642,648,654.
#
# Perguntas DO DIA: celula_id, sem filtrar por data_evento.
# app/chamados/services/vigia_fio.py:614.
# A fabrica usa outra chave: colaborador + data_evento + conversa=False.
# app/chamados/services/fabrica_da_celula.py:148.
#
# Classes abaixo agrupam observacoes, sem inventar causa/degrau do C6.
# O juiz devolve booleano: app/chamados/juizes.py:862,1389.

import datetime as dt
import inspect
import io
import json
import logging
import traceback
from collections import defaultdict
from contextlib import contextmanager, redirect_stdout, redirect_stderr

from django.db import connection, transaction


FIXO = {
    129: "2026-10-25 2026-11-01 2026-11-02 2026-11-08",
    258: "2026-10-30 2026-11-06 2026-11-09",
    427: "2026-09-21",
    451: "2026-10-21 2026-10-23 2026-10-25 2026-10-27",
    502: "2026-10-28",
    899: "2026-10-24",
    999: "2026-10-28",
}
DATAS = {
    cid: [dt.date.fromisoformat(s) for s in datas.split()]
    for cid, datas in FIXO.items()
}
PARES = [(cid, data) for cid, datas in DATAS.items() for data in datas]
chamada = {}


def endereco(arquivo, linha):
    caminho = (
        "app/" + arquivo.split("/app/", 1)[1]
        if "/app/" in arquivo
        else "<sonda-ou-biblioteca>"
    )
    return caminho + ":" + str(linha)


def tb_seguro(tb):
    # Nao inclui mensagens, codigo-fonte ou variaveis locais.
    return "\n".join(
        endereco(frame.filename, frame.lineno) + " " + frame.name
        for frame in traceback.extract_tb(tb)
    )


class FalhaInternaRegistrada(RuntimeError):
    pass


class Captura(logging.Handler):
    def __init__(self):
        super().__init__(logging.WARNING)
        self.registros = []

    def emit(self, record):
        self.registros.append(record)


@contextmanager
def somente_capturar_logs(captura):
    # Alteracoes apenas em memoria; mensagens livres nao vao aos handlers.
    loggers = [logging.getLogger()] + [
        obj
        for obj in list(logging.Logger.manager.loggerDict.values())
        if isinstance(obj, logging.Logger)
    ]
    anteriores = [
        (obj, obj.handlers[:], obj.propagate, obj.level, obj.disabled)
        for obj in loggers
    ]
    try:
        for obj in loggers:
            obj.handlers = [captura]
            obj.propagate = False
            obj.setLevel(logging.WARNING)
            obj.disabled = False
        yield
    finally:
        for obj, handlers, propaga, nivel, desativado in anteriores:
            obj.handlers = handlers
            obj.propagate = propaga
            obj.setLevel(nivel)
            obj.disabled = desativado


def chamar(funcao, *args, **kwargs):
    global chamada
    assinatura = inspect.signature(funcao)
    chamada = {
        "funcao": funcao.__module__ + "." + funcao.__name__,
        "assinatura": str(assinatura),
    }
    try:
        assinatura.bind(*args, **kwargs)
    except TypeError as exc:
        # bind informa parametros ausentes/incompativeis sem imprimir objetos.
        chamada["argumento_incompativel_ou_ausente"] = str(exc)
        raise

    antes = len(captura.registros)
    resultado = funcao(*args, **kwargs)
    novos = captura.registros[antes:]
    if novos:
        # Falhas internas registradas tambem invalidam a medicao:
        # app/chamados/juizes.py:1463,1476;
        # app/colaboradores/services/calendario.py:478.
        exc = FalhaInternaRegistrada("funcao registrou WARNING/ERROR")
        exc.registros = novos
        causa = next(
            (
                record.exc_info[1]
                for record in novos
                if record.exc_info and record.exc_info[1]
            ),
            None,
        )
        if causa is not None:
            raise exc from causa
        raise exc

    if connection.needs_rollback:
        raise FalhaInternaRegistrada("transacao marcada para rollback")
    return resultado


def erro_seguro(exc):
    causa = exc.__cause__
    return {
        "erro_classe": type(exc).__name__,
        "erro": (
            "falha de leitura; mensagem livre omitida para preservar a raspagem"
        ),
        "chamada": dict(chamada),
        "tb": tb_seguro(exc.__traceback__),
        "causa_classe": type(causa).__name__ if causa is not None else None,
        "causa_tb": (
            tb_seguro(causa.__traceback__) if causa is not None else None
        ),
        "registros_internos": [
            {
                "nivel": record.levelname,
                "fonte": endereco(record.pathname, record.lineno),
                "erro_classe": (
                    record.exc_info[0].__name__
                    if record.exc_info and record.exc_info[0]
                    else None
                ),
            }
            for record in getattr(exc, "registros", ())
        ],
    }


def local(valor):
    return timezone.localtime(valor).isoformat() if valor is not None else None


def evidencia_autoria(p):
    # Autor do carimbo persistido nao garante autor da via atual:
    # app/chamados/models.py:3183;
    # app/chamados/services/relavra_via.py:123.
    #
    # Projecoes sem descricao, payload, texto de resposta ou ator livre:
    # app/core/models.py:295;
    # app/chamados/models.py:3872.
    logs = LogAuditoria.objects.filter(
        modelo="PerguntaDisputa", objeto_id=p.pk
    )
    todos_pks = list(logs.order_by("pk").values_list("pk", flat=True))
    coincidentes = list(
        logs.filter(valor_depois__via_resolucao=p.via_resolucao)
        .order_by("pk")
        .values("pk", "usuario_id", "criado_em")
    )
    decisoes = list(
        DecisaoPergunta.objects.filter(pergunta_id=p.pk)
        .order_by("pk")
        .values("pk", "ato_por_id", "ato_em")
    )
    return {
        "materializada_por_id": p.materializada_por_id,
        "materializada_em_local": local(p.materializada_em),
        "autoria_da_via_atual_apenas_pelo_campo": "[nao verificado]",
        "auditoria_n": len(todos_pks),
        "auditoria_pks": todos_pks,
        "auditoria_com_payload_da_via_atual_n": len(coincidentes),
        "auditoria_com_payload_da_via_atual": [
            {
                "pk": item["pk"],
                "usuario_id": item["usuario_id"],
                "criado_em_local": local(item["criado_em"]),
            }
            for item in coincidentes
        ],
        "decisoes_n": len(decisoes),
        "decisoes": [
            {
                "pk": item["pk"],
                "ato_por_id": item["ato_por_id"],
                "ato_em_local": local(item["ato_em"]),
            }
            for item in decisoes
        ],
        # Auditoria e best-effort: app/core/observ.py:94,139.
        "completude_historica": "[nao verificado]",
        # DecisaoPergunta nao tem via_resolucao:
        # app/chamados/models.py:3872.
        "vinculo_das_decisoes_com_a_via": "[nao verificado]",
    }


captura = Captura()

# Impede saidas livres dos chamadores; somente o JSON final sera impresso.
with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
    try:
        with transaction.atomic():
            with connection.cursor() as cur:
                cur.execute(
                    "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                )
                cur.execute("SELECT current_user, current_schema()")
                if cur.fetchone() != ("leitor", "juliani"):
                    raise RuntimeError("contexto_incorreto")

            with somente_capturar_logs(captura):
                from django.db.models import Q
                from django.utils import timezone

                from chamados.juizes import (
                    alimentar_portas,
                    fato_do_marco_no_espelho,
                    pergunta_viva,
                    porta_alcancavel_do_dia,
                    um_colaborador_por_identidade,
                )
                from chamados.models import DecisaoPergunta, PerguntaDisputa
                from chamados.services.fabrica_da_celula import motivos_a_perguntar
                from chamados.services.vigia_fio import (
                    ListaDoAdmin,
                    dono_fora_do_fio,
                )
                from colaboradores.models import Colaborador
                from colaboradores.services.calendario import (
                    STATUS_QUE_PEDEM_DECISAO,
                    contexto_calendario,
                    resolver_dia_oferecido,
                    resolver_dia_oferecido_nas_datas,
                )
                from core.models import LogAuditoria
                from escala.models import CelulaDia
                from ponto.janelas import janela_atual
                from ponto.status_dia import status_do_dia_lavrado

                agora = timezone.now()
                hoje = timezone.localdate(agora)

                filtro = Q(pk__in=[])
                for cid, datas in DATAS.items():
                    filtro |= Q(colaborador_id=cid, data__in=datas)

                colabs = Colaborador.objects.select_related("empresa").in_bulk(
                    list(DATAS)
                )
                celulas = list(
                    CelulaDia.objects.filter(filtro).select_related("colaborador")
                )
                por_dia = {
                    (cel.colaborador_id, cel.data): cel for cel in celulas
                }

                # Mesma alimentacao compartilhada do censo:
                # app/chamados/services/vigia_fio.py:604.
                alim = chamar(
                    alimentar_portas,
                    [cel.pk for cel in celulas],
                    colabs.values(),
                )
                todas = chamar(
                    um_colaborador_por_identidade,
                    PerguntaDisputa.objects.filter(
                        celula_id__in=[cel.pk for cel in celulas]
                    )
                    .select_related("disputa", "celula")
                    .order_by("pk"),
                    alim["instancias"],
                )
                por_celula = defaultdict(list)
                for p in todas:
                    por_celula[p.celula_id].append(p)

                lista = ListaDoAdmin(hoje)
                resolver_cache = {}
                dias = []

                for cid, data in PARES:
                    chamada = {"etapa": "carregar_dia"}
                    try:
                        # Isola erro SQL do dia; erro nunca vira resultado zero.
                        with transaction.atomic():
                            colab = colabs[cid]
                            cel = por_dia[(cid, data)]
                            cel.colaborador = colab
                            perguntas = por_celula.get(cel.pk, [])

                            porta = chamar(
                                porta_alcancavel_do_dia,
                                cel.pk,
                                alimentacao=alim,
                            )

                            # Plural por colaborador, como a rodada do censo:
                            # app/chamados/services/vigia_fio.py:258.
                            if cid not in resolver_cache:
                                try:
                                    resolver_cache[cid] = chamar(
                                        resolver_dia_oferecido_nas_datas,
                                        colab,
                                        DATAS[cid],
                                    )
                                except Exception as exc:
                                    resolver_cache[cid] = (
                                        exc,
                                        dict(chamada),
                                    )
                                    raise

                            resolucao = resolver_cache[cid]
                            if isinstance(resolucao, tuple):
                                exc, origem = resolucao
                                chamada = dict(origem)
                                raise exc
                            oferecido = resolucao[data]

                            linha = {
                                "col": "col%d" % cid,
                                "data": data.isoformat(),
                                "celula_pk": cel.pk,
                                "chave_perguntas": "celula_id",
                                "porta_alcancavel_do_dia": porta,
                                "resolver_dia_oferecido_nas_datas": oferecido,
                                "vivas_lidas_na_alimentacao_pks": sorted(
                                    p.pk
                                    for p in alim["vivas"].get(cel.pk, ())
                                ),
                                "motivos_a_perguntar": chamar(
                                    motivos_a_perguntar,
                                    cel,
                                    hoje,
                                    agora=agora,
                                ),
                                "perguntas": [],
                            }

                            for p in perguntas:
                                viva = chamar(pergunta_viva, p)
                                marco = chamar(fato_do_marco_no_espelho, p)
                                linha["perguntas"].append({
                                    "pk": p.pk,
                                    "disputa_pk": p.disputa_id,
                                    "celula_pk": p.celula_id,
                                    "data_evento": p.data_evento,
                                    "motivo": p.motivo,
                                    "via_resolucao": p.via_resolucao,
                                    "respondida_em_local": local(p.respondida_em),
                                    "validada_em_local": local(p.validada_em),
                                    "validada_por_id": p.validada_por_id,
                                    "pergunta_viva": viva,
                                    # Preserva True/False/None:
                                    # app/chamados/juizes.py:930.
                                    "fato_do_marco_no_espelho": marco,
                                    "autoria": evidencia_autoria(p),
                                })

                            # Mesma condicao de entrada nesse ramo do censo:
                            # app/chamados/services/vigia_fio.py:642,648,654.
                            chamar_dono = porta is None and data < hoje
                            linha["dono_fora_do_fio_chamado"] = chamar_dono
                            if chamar_dono:
                                linha["dono_fora_do_fio"] = chamar(
                                    dono_fora_do_fio,
                                    colab,
                                    data,
                                    perguntas,
                                    lista,
                                    com_papel=True,
                                    oferecido=oferecido,
                                )

                            if cid == 427 and data == dt.date(2026, 9, 21):
                                d4 = chamar(
                                    status_do_dia_lavrado,
                                    colab,
                                    data,
                                    agora=agora,
                                )
                                ctx = chamar(
                                    contexto_calendario,
                                    colab,
                                    {
                                        "modo": "custom",
                                        "data_ini": data.isoformat(),
                                        "data_fim": data.isoformat(),
                                        "mes": str(data.month),
                                        "ano": str(data.year),
                                    },
                                )
                                dia_tela = next(
                                    item
                                    for item in ctx["dias"]
                                    if item["data"] == data
                                )
                                direto = chamar(
                                    resolver_dia_oferecido, dia_tela, hoje
                                )
                                if direto != oferecido:
                                    raise RuntimeError(
                                        "resolver_plural_diverge_da_tela"
                                    )

                                linha["D4"] = {
                                    "status": d4["status"],
                                    "fonte": d4["fonte"],
                                }
                                # O juiz nao retorna motivo separado. Publica
                                # operandos reais e sua expressao original,
                                # sem implementar outro predicado explicativo:
                                # app/colaboradores/services/calendario.py:97.
                                linha["motivo_do_falso_operandos_do_juiz"] = {
                                    "retorno_real": direto,
                                    "pode_ultimo_recurso": (
                                        dia_tela["pode_ultimo_recurso"]
                                    ),
                                    "data": dia_tela["data"],
                                    "hoje": hoje,
                                    "status_D4_na_tela": dia_tela["status"],
                                    "status_que_pedem_decisao": list(
                                        STATUS_QUE_PEDEM_DECISAO
                                    ),
                                    "janela_atual": chamar(
                                        janela_atual, hoje, colab.empresa
                                    ),
                                    "regra": inspect.getsource(
                                        resolver_dia_oferecido
                                    ),
                                    "fonte": (
                                        "app/colaboradores/services/"
                                        "calendario.py:97"
                                    ),
                                    "causa_exclusiva_sem_exame_dos_operandos": (
                                        "[nao verificado]"
                                    ),
                                }

                            if timezone.localdate() != hoje:
                                raise RuntimeError("relogio_cruzou_data_local")

                            linha["ok"] = True
                            transaction.set_rollback(True)

                        dias.append(linha)

                    except Exception as exc:
                        # Descarta a medicao parcial; publica erro do dia.
                        dias.append({
                            "col": "col%d" % cid,
                            "data": data.isoformat(),
                            "ok": False,
                            **erro_seguro(exc),
                        })

                # Classes observadas de perguntas MORTAS pelo juiz real.
                # Um dia pode ter mais de uma classe; nao somar os dias entre
                # classes como

[truncado em 20000 de 23818 caracteres]

## 12/11 11:41 · sessao e7dc53643837 · - · - · origem=Ronald (tmux)

Censo AST executado em memória: 767 arquivos Python de produção em `app`, excluindo testes, migrations, assets e logs; zero falhas de parse. Reutilizei por extração AST o detector de `app/chamados/tests/test_o261e_f2_porta_carimbo.py:99`, com campos ampliados; ele cobre atribuição, `setattr`, dicionários propagados, construtor, `update` e `bulk_update`, mas declara limites para nomes vindos de outro módulo e `__dict__` em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:111`. Execução de testes e confirmação no banco: [nao verificado].

**Escritores físicos encontrados**

| Campo | Atribuição e persistência |
|---|---|
| `via_resolucao` | `_escrever_carimbo`: `app/chamados/models.py:3102`; persistido por `carimbar` em `:3153`, `carimbar_identidade` em `:3191` e `descarimbar` em `:3213`. |
| `via_resolucao` em lote | `PerguntaDisputa.objects…update` em `app/chamados/models.py:3265`, com via vazia, materialização e autor nulos em `:3267`. |
| `via_resolucao` isolada | `encerrar_por_fato_no_espelho`, atribuição/save em `app/chamados/models.py:3276`; `relavrar`, atribuição/save em `app/chamados/services/relavra_via.py:125`. |
| `respondida_em` | `carimbar`, atribuição em `app/chamados/models.py:3133`, save em `:3153`; resposta humana em `app/chamados/services/disputa_emissao.py:1468`, save em `:1508`; automática em `:1653`, save em `:1655`; recuperação pela trilha em `:1674`, save em `:1676`. |
| `validada_em`/`validada_por` | Simetria de `carimbar` em `app/chamados/models.py:3136`, save em `:3153`; ato de validar em `app/chamados/services/validacao.py:134`, save em `:137`; devolução da validação em `:199`, save em `:202`; resposta nova limpa ambos em `app/chamados/services/disputa_emissao.py:1480`, save em `:1508`. |
| Limpeza dos três campos de resposta/validação | `ressuscitar_perguntas` passa campos explícitos a `descarimbar_em_lote` em `app/chamados/models.py:1567`; `reabrir_para_correcao` monta `campos` em `app/chamados/services/disputa_emissao.py:1593` e chama a mesma porta em `:1630`. |
| Alterações somente em memória | Cópia para consultar o C6 em `app/chamados/models.py:1507`; restauração da tentativa recusada em `:3163`. |

Não apareceu outro `bulk_update` nem outro `QuerySet.update` desses campos no universo acima [nao verificado como garantia além dos limites do detector]. `flip_auto.py:128` é **leitor** por `filter`, não escritor; `competencia_trancada.py:126` determina saída por leitura e `:255` apenas reabre o legado.

**Apelidos e reexports**

- Reabertura: `_reabrir` em `app/chamados/models.py:2220` e `app/chamados/services/acoes_disputa.py:102`; `_reabrir2` em `app/chamados/views.py:1435`.
- `PerguntaDisputa` como `_PD`: `app/colaboradores/models.py:432`, `app/chamados/veredito_lavrado.py:201`, `app/chamados/views.py:398`, `app/colaboradores/services/calendario.py:469`, `app/ponto/services/cartorio.py:1635`, `app/chamados/services/limbo_folgas.py:270`, `app/chamados/services/disputa_emissao.py:1501`, `app/chamados/services/raiox.py:177` e `app/chamados/services/fio.py:392`, `:768`, `:883`.
- `_PD_ordem`: `app/ponto/services/declarar_dia.py:189`, `app/ponto/services/veredito_celula.py:225`, `app/chamados/services/validacao.py:428`; `_PD_inf`: `app/chamados/services/painel.py:260`.
- Reexports explícitos dos escritores pelo modelo: `app/chamados/models.py:4088` e `:4093`. Importação `*` de `chamados` na produção varrida: não encontrada [nao verificado fora desse universo].

**Matriz dos caminhos principais**

| Entrada → juiz → operação | Persistência → retorno → prova aberta |
|---|---|
| Pergunta sem validação → `carimbar` consulta C6 **depois de aplicar carimbos**, `app/chamados/models.py:3130` e `:3140` | Trio materialização/via; havendo texto, `resposta='validar'` preenche resposta e validação antes do C6, `:3131`; save `:3153`, `True` em `:3158`; viva desfaz e retorna `False`, `:3144`. Testes de recusa/restauração: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:665`, `:687`, `:713`. |
| Admin decide/silêncio → não consulta C6 na porta de identidade, `app/chamados/models.py:3169` | `fechar_por_admin` escolhe `admin_decidiu`, `:3065`; trio em `:3187`, save/`True` em `:3191`; idempotente `False` em `:3056`. Fecho explícito da disputa grava `DecisaoPergunta.SILENCIO`, `:1920`; prova `app/chamados/tests/test_e1_entradas.py:121`. |
| Dia decidido pelo admin → lista `q_vivas` daquele dia, `app/ponto/services/veredito_celula.py:180` | `fechar_por_admin` em `:190`; linha `SILENCIO` em `:195`. Prova real e idempotência: `app/chamados/tests/test_e1_v5_portas_backfill.py:83`. |
| Folga aceita → juiz `o_dia_ja_e_folga` ou `checar_declaracao`, `app/chamados/services/validacao.py:280`, `:305` | Validação + `admin_decidiu`, `:288`, `:320`, `:329`; retorna `folga_ja_declarada` em `:302` ou `folga_declarada` em `:367`; recusa posterior reverte transação em `:343`. Provas nomeadas em `app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:136` e `:197`; corpo não aberto [nao verificado]. |
| Motivo removido → pertence aos motivos atuais e disputa aberta, `app/chamados/models.py:2034` | Linha `RETRATACAO` em `:2057`, identidade `motivo_retratado` em `:2062`, conserva carimbo anterior; retorna `removido` em `:2068`. Prova aberta preserva resposta e constata morte: `app/chamados/tests/test_e1_entradas.py:142`. |
| Duplicata → `plano_de_retratacao`, `app/chamados/services/fio.py:1341` | `retratar_duplicata` grava `duplicata`, conserva carimbo, anota canônica em `:1360`; `True` em `:1364`, já duplicata `False` em `:1351`. Prova aberta apply/idempotência: `app/chamados/tests/test_e1_v5_portas_backfill.py:207`; veto de mérito: `app/chamados/tests/test_e1_v9.py:210`. |
| Paridade recusada → exige lâmpada `is True`, `app/chamados/services/materializacao.py:906` | Identidade `paridade_recusada` em `:930`, retorna `None` em `:931`; apagada apenas recusa e retorna `None`, `:919`. Provas abertas dos três valores: `app/chamados/tests/test_o261_paridade_respeita_a_lampada.py:175`, `:183`, `:192`. |
| Colisão tipo oposto → `traduzir_recusa`, `app/chamados/services/materializacao.py:1097` | Sem materialização e código `COLISAO`, identidade `fato_ocupado` + motivo em `:1105`; sem teste local da lâmpada. Retorno implícito `None`, função termina em `:1113`; prova específica [nao verificado]. |
| Ausência declarada → `declarou_hora as _declarou_hora`, `app/chamados/services/materializacao.py:548`; C6 dentro da porta | Irmãs não materializadas: `declaracao` ou `ausencia_dia`, `:555`; `carimbar(...resposta='validar')`, `:567`; se gravou, linha `ABSORCAO`, `:569`; se recusou, evento e lista de recusadas, `:573`. Prova de recusa aberta apenas por localização: `app/chamados/tests/test_o261e_f2_fila.py:238` [nao verificado corpo]. |
| Materializador → juiz de lastro após plantio, `app/chamados/services/materializacao.py:1240`; via por `_declarou`/`_via_fato`, `:1267` | `carimbar` com simetria da validação e lavra, `:1283`; C6 recusa → `_Reprovada`, rollback, `:1285`. Prova específica de rollback localizada em `app/chamados/tests/test_hx_materializa_depois_julga.py:30` [nao verificado corpo]. |
| Fecho de disputa, pendentes aprovadas → lastro em `app/chamados/services/materializacao.py:1433`, C6 na porta | `juiz_aprovou` em `:1437`, devolve lista em `:1439`; prova aberta barra resposta não validada: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:687`. |
| Sem turno aberto → motivo limitado, sem texto/materialização e núcleo sem turno, `app/chamados/reconciliador.py:363`; C6 antes em `:398`, novamente pela porta em `:406` | Trio com `sem_turno_aberto`; eventual fecho da disputa por C7 em `:412`; retorna `True`, `:415`. Prova aberta **com marco apagado e validação prévia**: `app/chamados/tests/test_o261_lavra_le_a_lampada.py:148`; construção da lâmpada apagada em `:115`. |
| Célula absolveu → `perguntas_absolvidas_pela_celula`, `app/chamados/reconciliador.py:585`; C6 na porta | `celula_absolveu`, `:596`; contabiliza e eventualmente fecha por C7, `:612`. Prova localizada em `app/chamados/tests/test_selo_celula_absolveu.py:66` [nao verificado corpo]. |
| Fantasma atual → `veredito_orfa == ENCERRA`, `app/chamados/reconciliador.py:1108` | Consumidor chama `encerrar_fantasma`; porta passa pelo C6, `app/chamados/models.py:3288`. Provas abertas: barra viva respondida em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:665`; aceita turno fechado em `:676`. |
| Fato materializado → comando exige lâmpada `is True`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:41` | Só troca via, `app/chamados/models.py:3276`; retorna `True`, `:3278`; não é via cega nesse chamador. Prova específica [nao verificado]. |
| Re-lavra de carimbo derivado → `declarou_hora`/`via_pelo_fato`, `app/chamados/services/relavra_via.py:95` | Altera via e lavra no mesmo save `:125`; retorna `(n_via,n_ver)`, `:139`; não exige lâmpada localmente. Prova específica [nao verificado]. |

**Divergência comprovada para decisão**

O `carimbar` exige “C6 deu morta”, mas o C6 pode responder sobre a **validação que o próprio carimbar acabou de preencher**, antes de conferir o efeito do marco: `app/chamados/models.py:3135` e `:3140`. A outra leitura é a lâmpada de `CLAUDE.md:169`. O teste `app/chamados/tests/test_o261_lavra_le_a_lampada.py:148` comprova a variante já validada: lâmpada de entrada apagada, via `sem_turno_aberto` persistida e disputa fechada. O teste de paridade explicita que validada sai do fio mesmo sem absolvição por fato, `app/chamados/tests/test_o261_paridade_respeita_a_lampada.py:210`. Cabe decisão numerada na consolidação, sem escolher entre essas leituras.

**Cobertura no backlog aberto**

- E4 cobre o conjunto de portas de estado, com E4b backfill, E4c leitores e E4d parada da projeção: `app/docs/BACKLOG.md:1885`.
- Identidade/duplicata tem fato próprio adiado para depois da E4d: `app/docs/BACKLOG.md:1889`; excedentes duplicados estão explicitamente em E1/E4: `:1912`.
- Reação da raiz aos carimbadores e fecho fica em RAIZ F3/F4; F5 se combina com E4d: `app/docs/BACKLOG.md:1904`; os sete carimbadores sem C7 estão nomeados em `:1911`.
- `celula_absolveu` pelo dia errado tem complemento específico **ABSOLVIDA-PELA-FOLGA**, E1-ENTRADAS, com pergunta #51407: `app/docs/BACKLOG.md:1935`. Isso não demonstra a classe dos outros 14 dias [nao verificado].

## 12/11 11:40 · sessao 92e371f1103f · - · - · origem=Ronald (tmux)

BLOQUEIA — `app/chamados/services/explicador.py:137` duplica C7 incompleto: define `pode_fechar = not fio_vivo(ps)`, mas C7 exige também ausência de respostas sem veredito (`app/chamados/models.py:1866`). Contraexemplo: pergunta com texto e `respondida_em`, sem validação/materialização, célula `ok` decidida pelo admin; C6 pode considerá-la morta (`app/chamados/juizes.py:1404`, `app/chamados/juizes.py:1452`), enquanto a guarda mantém a resposta pendente (`app/chamados/models.py:1855`). Explicador publica “Tudo resolvido” (`app/chamados/services/explicador.py:140`), modal oferece fechar/regularizar (`app/templates/chamados/partials/modal_fio.html:446`), e fechamento recusa (`app/chamados/models.py:1899`).

BLOQUEIA — #9 deixa silêncio vencido com significados diferentes: fila e lista do colaborador excluem somente futuro (`app/chamados/services/painel.py:190`, `app/chamados/services/janela_colab.py:78`), mas a partição classifica qualquer prazo vencido como `HISTORICO` (`app/chamados/services/verbos.py:48`), e a lista padrão administrativa o exclui (`app/chamados/services/painel.py:324`). Contraexemplo: chamado aberto com silêncio vencido aparece ao colaborador e entra na fila, porém desaparece da lista padrão do admin; a tarefa ainda remove esse chamado de sua `fila` publicada (`app/chamados/services/tarefa_do_dia.py:551`). Os testes novos verificam `qs_na_fila` e `chamados_visiveis_colab`, sem verificar a lista efetiva do painel (`app/chamados/tests/test_e3_b4_silencio_vencido.py:36`).

FILA — `respondida_em`: escritor de resposta humana em `app/chamados/services/disputa_emissao.py:1468`, com projeção e decisão no mesmo ato (`app/chamados/services/disputa_emissao.py:1503`); escritores adicionais de resposta automática e carimbo em `app/chamados/services/disputa_emissao.py:1653`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/models.py:3145`; leitores relevantes C6 (`app/chamados/juizes.py:1375`), aviso corrigido (`app/chamados/services/fio.py:174`) e crédito ainda por timestamp (`app/chamados/services/raiox.py:139`). Contraexemplo: resposta com timestamp e texto vazio vira crédito de respondida no painel, mas o aviso continua esperando o colaborador (`app/chamados/services/raiox.py:139`, `app/chamados/services/fio.py:174`).

FILA — `silenciado_ate`: escritores conferidos `arquivo.silenciar` e `arquivo.levantar` (`app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:202`); instante futuro estrito, igualdade significa vencido (`app/chamados/models.py:512`, `app/chamados/models.py:524`). Contraexemplo de leitor divergente: prazo vencido permanece histórico na partição (`app/chamados/services/verbos.py:51`).

FILA — `data_evento`: atribuição direta única encontrada em `app/chamados/services/fio.py:819`; nascimento usa essa porta (`app/chamados/models.py:3029`), costura também (`app/chamados/services/fio.py:898`) e rechaveamento também (`app/chamados/services/fio.py:1056`). Campo significa dia do turno para fato (`app/chamados/models.py:2796`) e dia civil para conversa (`app/chamados/models.py:2808`); C19 separa `conversa=False` (`app/chamados/services/tarefa_do_dia.py:375`). Ocorrência material adicional de mistura entre esses significados: nada demonstrado; incidência em produção [nao verificado].

FILA — `status_local`: núcleo persistente em `app/chamados/models.py:228`; nascimento em `app/chamados/models.py:1108`; outras atribuições encontradas simulam e restauram estado em memória (`app/chamados/models.py:356`, `app/chamados/models.py:371`). Segundo escritor direto fora dessas portas: nada encontrado; reflexão e kwargs dinâmicos [nao verificado].

FILA — porta: saída escrita em memória pelo próprio C19 (`app/chamados/services/tarefa_do_dia.py:555`), projetada por `porta_da_tarefa` (`app/chamados/services/tarefa_do_dia.py:565`); não há persistência nesse trecho (`app/chamados/services/tarefa_do_dia.py:540`). Contraexemplo material: silêncio vencido pode manter `rota=porta_fila` enquanto `fila=()` porque `da_celula` é calculado antes da exclusão de `HISTORICO` (`app/chamados/services/tarefa_do_dia.py:550`, `app/chamados/services/tarefa_do_dia.py:555`).

FILA — ordem de trava: nada novo material encontrado nos caminhos conferidos; núcleo de status trava universo antes de salvar (`app/chamados/models.py:280`), silêncio pede a porta de ordem (`app/chamados/services/arquivo.py:170`, `app/chamados/services/arquivo.py:209`), rechaveamento trava antes da releitura/escrita (`app/chamados/services/fio.py:1025`). Ordem implementada C→D→CH→P em `app/chamados/models.py:2679`, `app/chamados/models.py:2688`, `app/chamados/models.py:2701`, `app/chamados/models.py:2705`; cobertura global/transitiva [nao verificado].

## 12/11 11:39 · sessao bf78c34d5db7 · - · - · origem=Ronald (tmux)

BLOQUEIA — item 5, respondedores vivos: `raiox.creditos_perguntas` ainda separa respondidas por timestamp (`app/chamados/services/raiox.py:135`, `app/chamados/services/raiox.py:139`); `relatorios_disputa.classificar` decide dono por C6 + timestamp (`app/chamados/relatorios_disputa.py:18`, `app/chamados/relatorios_disputa.py:21`); `_calados` usa `q_vivas` + timestamp nulo (`app/chamados/services/cobranca.py:229`); cartão deriva documento/formulário/espera no template (`app/templates/chamados/partials/_card_atendimento.html:65`, `app/templates/chamados/partials/_card_atendimento.html:77`, `app/templates/chamados/partials/_card_atendimento.html:94`). Contraexemplo: resposta vazia com timestamp continua tarefa RESPONDER do colaborador (`app/chamados/services/tarefa_do_dia.py:135`), mas relatório diz AGUARDA_SUPERVISAO (`app/chamados/relatorios_disputa.py:23`) e cartão diz “Aguardando a supervisao conferir” (`app/templates/chamados/partials/_card_atendimento.html:101`).

BLOQUEIA — API/badge/PWA não migraram: `consulta_do_app` ainda exige timestamp nulo + disputa aberta para pendente (`app/chamados/services/pergunta_no_app.py:70`); `projecao_do_app` conta essa lista (`app/chamados/services/pergunta_no_app.py:90`). Consumidores: dois kernels (`app/api/views.py:315`, `app/api/views_core.py:303`), API chamados (`app/api/views.py:993`), API perguntas (`app/api/views.py:2533`) e dashboard (`app/core/views.py:252`). Contraexemplo: resposta vazia com timestamp cai no histórico, badge não conta RESPONDER, enquanto tarefa mantém RESPONDER (`app/chamados/services/tarefa_do_dia.py:137`); o próprio novo teste identifica essa resposta como ainda devida pelo colaborador (`app/chamados/tests/test_e3_leitores_fio_disputa.py:143`).

BLOQUEIA — tela admin/fio continuam derivando dono por fonte antiga: partição chama `creditos_perguntas` (`app/chamados/services/verbos.py:34`); natureza prioriza resposta creditada como `admin_validar` (`app/chamados/catalogo/natureza.py:48`); fio passa esses créditos ao desenlace (`app/chamados/services/fio.py:405`, `app/chamados/services/fio.py:421`); desenlace imprime “Sua vez: Validar” (`app/chamados/services/desenlace.py:72`). Contraexemplo: resposta vazia timestamp presente produz crédito respondido (`app/chamados/services/raiox.py:139`) e oferece validação onde tarefa exige RESPONDER (`app/chamados/services/tarefa_do_dia.py:137`).

BLOQUEIA — #9 não resolve o consumidor real da fila: silêncio vencido vira HISTORICO na partição (`app/chamados/services/verbos.py:48`), e painel padrão exclui esses ids (`app/chamados/services/painel.py:324`). Ao mesmo tempo, queryset da fila e lista do colaborador devolvem o chamado (`app/chamados/services/painel.py:189`, `app/chamados/services/janela_colab.py:77`). Contraexemplo: chamado aberto com `silenciado_ate=agora−1min` aparece para colaborador e no contador COBRANCA (`app/chamados/services/painel.py:212`), mas não na lista padrão administrativa (`app/chamados/services/painel.py:331`). RED #9 verifica somente os dois querysets (`app/chamados/tests/test_e3_b4_silencio_vencido.py:35`), portanto não morde essa divergência de apresentação.

BLOQUEIA — porta da tarefa pode publicar fila vazia sob silêncio vencido: `alimentar_fila` inclui chamado vencido em `fila_ids` (`app/chamados/juizes.py:1533`); `_portas_da_tarefa` mantém esse inteiro em `portas` (`app/chamados/services/tarefa_do_dia.py:548`), exclui HISTORICO apenas de `fila` (`app/chamados/services/tarefa_do_dia.py:551`) e publica rota porque `portas` permanece preenchido (`app/chamados/services/tarefa_do_dia.py:555`). Contraexemplo: pergunta exigível ancorada no chamado vencido da célula gera `porta_fila`, `fila=()`, enquanto lista admin esconde a âncora (`app/chamados/services/painel.py:331`).

BLOQUEIA — item 6, selo não cobre o universo prometido: catraca exige somente um chamador C17 (`app/chamados/tests/test_e3_pendentes_app.py:145`); seu mutante acrescenta chamada C17 (`app/chamados/tests/test_e3_pendentes_app.py:153`). Contraexemplo: todos os leitores próprios acima permanecem e a catraca não os busca, pois o detector só reconhece o alvo literal resolvido `chamados.juizes.porta_alcancavel_do_dia` (`app/chamados/tests/test_e3_pendentes_app.py:33`, `app/chamados/tests/test_e3_pendentes_app.py:89`).

BLOQUEIA — leitor administrativo adicional não migrou: calendário consulta C6 sem relógio recebido (`app/colaboradores/services/calendario.py:476`), exclui texto vazio antes do juiz (`app/colaboradores/services/calendario.py:474`) e calcula `pode_ultimo_recurso` separadamente (`app/colaboradores/services/calendario.py:738`), consumido por `resolver_dia_oferecido` (`app/colaboradores/services/calendario.py:103`). Contraexemplo: pergunta viva com resposta vazia em competência passada não bloqueia essa oferta, enquanto tarefa publica RESPONDER (`app/chamados/services/tarefa_do_dia.py:137`); incidência real [nao verificado].

BLOQUEIA — copiloto permanece com derivação e relógio próprios: `dia_do_colab` publica C6 sem `agora` (`app/ponto/services/dia_do_colab.py:71`) e delega retenção ao antigo `do_dia` (`app/ponto/services/dia_do_colab.py:108`), que lê `timezone.localdate`, VIVOS, universo do app e `retencao` (`app/chamados/services/pergunta_no_app.py:217`, `app/chamados/services/pergunta_no_app.py:223`). Contraexemplo: pergunta viva respondida com texto segue descrita genericamente como esperando “resposta ou decisao” (`app/ponto/services/dia_do_colab.py:148`), sem publicar tarefa/porta única (`app/ponto/services/dia_do_colab.py:73`); contador de leitores fora da tarefa e pergunta golden solicitados não foram encontrados por busca textual, ausência completa [nao verificado].

FILA — badge navbar mantém unidade chamado em fila (`app/core/context_processors.py:185`), diferente do badge de perguntas pendentes (`app/chamados/services/pergunta_no_app.py:91`); contraexemplo: chamado na fila sem pergunta produz navbar 1, enquanto tarefa pode ter apenas ação do sistema e nenhuma porta, caso já explicitado no novo teste (`app/chamados/tests/test_e3_leitores_c17_chamadores.py:101`). Migração desse sítio permanece sem RED específico identificado [nao verificado].

FILA — `_calados` ainda trata pré-filtro como vida exata (`app/chamados/services/cobranca.py:229`), embora o contrato de `q_vivas` declare seleção mais generosa que C6 (`app/chamados/models.py:3349`). Contraexemplo: pergunta sem resposta em dia trancado por leitura pode entrar em cobrança apesar de morta no C6; incidência e execução [nao verificado].

## 12/11 11:39 · sessao eef0afa7e826 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

- **B1 curado:** resumo e marca usam o mesmo conjunto de instantes. RED na c14 (`0 != 1`) e GREEN após a cura. [Evidência](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c15_execucoes.r:6).
- **B2:** selo compara PDF e tela, incluindo a marca de 10/06. A fixture original não executou por ausência de Django; seus números permanecem **[nao verificado]**. [Detalhes](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c15_execucoes.r:16).
- **B3:** mantido o duplo porque o recalculador exige leituras ORM. A prova parcial mata três mutantes; `recalculador_vazio` sobrevive, expondo a lacuna integrada. [Justificativa em três linhas](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c15_execucoes.r:24).

**37 testes sem banco e ruff passaram; oito mutantes mortos e um sobrevivente.** Comandos e saídas no [relatório](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c15_execucoes.r:34). [nao verificado]

Sem commit, respeitando a proibição inicial. [Registro](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c15_execucoes.r:36). [nao verificado]

## 12/11 11:39 · sessao be892ef56153 · - · - · origem=Ronald (tmux)

BLOQUEIA — item 7, instrumento diverge do vigia migrado: `diff_e3_sombra.py:54` calcula `exige_admin` pelos verbos; `vigia_fio.py:650` usa `porta['fio'] == 'admin'`. Contraexemplo: pergunta respondida sem texto, verbo `validar`, fio `colab`: sombra informa admin=True, vigia False; o próprio caso está em `test_o261e_f5_alcance.py:177`, com assert False em `:192`. O teste do instrumento conserva a resposta antiga em `test_e3_diff_sombra.py:143`. [nao verificado]

BLOQUEIA — selo/mutante sem alvo: `e3_mutantes.py:112` procura o antigo `return` da candidata, mas o código agora tem alias em `diff_e3_sombra.py:43`; o harness encerra por `AssertionError('Mutante sem alvo')` em `e3_mutantes.py:162`. Contagem literal por AST em memória confirmou 0 alvos; morte do mutante por teste: [nao verificado]. [nao verificado]

BLOQUEIA — mutante de relógio atinge sítio errado e é equivalente: `e3_mutantes.py:101` troca o primeiro `agora=self.agora)` por `agora=None)`; há dois alvos, e `replace(...,1)` em `:164` alcança C6 em `tarefa_do_dia.py:493`, deixando `alimentar_fila` em `:533` intacta. C6 recupera `carga.agora` em `juizes.py:883`; portanto não reverte a cura pretendida.

FILA — item 11 não cobre literalmente a disputa inteira quando fechada: `fio.py:241` pré-filtra suas perguntas com `q_vivas`, `:255` descarta mortas antes de preencher o novo mapa em `:257`. Contraexemplo: disputa fechada com uma pergunta viva e outra validada: explicação recebe só a viva; o RED novo monta disputa aberta em `test_e3_leitores_fio_disputa.py:84`. Incidência de produção: [nao verificado].

#4 — cura da alimentação presente em `juizes.py:1575`; C6 recebe o instante, deriva seu dia local em `:885`, e evita o fallback de `:592`. Tarefa já chama C6 com seu instante em `tarefa_do_dia.py:493` e alimenta fila em `:533`. RED e controle em `test_e3_bloqueantes.py:110`/`:129`; verificação de repasse em `:135`. Reversão de `:1575` deve contrariar assert de `:145` por leitura estática; execução e mutante morto: [nao verificado].

#8 — REDs distinguem chamado sem ação, âncora silenciada e registrada em `test_e3_bloqueantes.py:148`, `:155`, `:164`; controle com porta em `:171` e mock da autoridade em `:184`. Reversão ao predicado cru contraria esses asserts por leitura estática; execução/mutante morto: [nao verificado].

#9 — RED vencido, controles vigente/NULL, instante recebido e fronteira exatamente igual em `test_e3_b4_silencio_vencido.py:41`, `:46`, `:50`, `:54`, `:59`. Reversão a NULL contraria o RED de `:43`; teste não exercita `tarefa_do_dia`, apenas os dois querysets (`:35`). Execução/mutante morto: [nao verificado]. [nao verificado]

Item 4 — REDs de folha, pauta e vigia distinguem o C17 anterior em `test_e3_leitores_c17_chamadores.py:124`, `:135`, `:154`, `:170`; controle de não chamada ao C17 em `:183`. Beco possui caso divergente em `test_f3_beco1_folga_na_lista.py:166`. Execução e mutantes por leitor: [nao verificado].

Item 6 — catraca verifica exclusivamente chamadas a C17 em `test_e3_pendentes_app.py:33`/`:145`; injeção demonstra detectar retorno da folha ao C17 em `:150`. Não certifica 0 derivações próprias nem 0 respondedores vivos; sua pergunta concreta é a contagem C17 (`:147`).

Item 11 — caso multicartão e universo completo aberto em `test_e3_leitores_fio_disputa.py:95`; regressão à lista do primeiro cartão contradiz asserts em `:103`/`:105`. Controle todas mortas em `:108`. Execução/mutante morto: [nao verificado].

Item 12 — caso morta sem resposta em `test_e3_leitores_fio_disputa.py:125`; controles viva sem resposta/com texto/sem texto em `:134`, `:138`, `:143`; API em `:156`/`:164`. Reversão ao timestamp cru contraria `:131` e `:146`. Execução/mutante morto: [nao verificado].

PRONTO 1–3/7–10 — o código documental ainda registra E3-LEITORES “1 de 10” em `RELATO.md:77`/`:95`; não há resultado da obra atual nesses trechos. Sonda 232, V/T/X, cura V, queda por classe, suite final, opus e pouso deste HEAD: [nao verificado]. [nao verificado]

_4223 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
