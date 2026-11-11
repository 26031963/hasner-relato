# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 15:41** · publicado em **11/11/2026 15:40** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 15:41 · sessao 321d419c4e23 · - · - · origem=Ronald (tmux)

Não identifiquei leitor real atual omitido pelas novas exclusões de chaves de dict ou `DESTINO_KW`: as chaves encontradas são payload/trilha; o literal sob `fields=` pertence ao índice. (`app/api/views.py:1165`; `app/api/views_mensageria.py:568`; `app/chamados/canal.py:79`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:354`; `app/chamados/services/materializacao.py:578`; `app/chamados/services/validacao.py:671`; `app/chamados/services/fio.py:1047`; `app/chamados/services/fio.py:1048`; `app/chamados/models.py:3483`)

Há uma lacuna demonstrável na regra de destino: `M.objects.create(x=p['data_evento'])` retorna censo vazio em simulação em memória; o contexto `escrita=True` suprime a constante do subscrito lido. Ocorrência real dessa forma na árvore: **[nao verificado]**. (`app/chamados/tests/test_catraca_data_evento_cru.py:102`; `app/chamados/tests/test_catraca_data_evento_cru.py:122`; `app/chamados/tests/test_catraca_data_evento_cru.py:139`)

`fields=['data_evento']` é suprimido em qualquer chamada, inclusive eventual serialização; isso constitui limite potencial, sem leitor atual localizado nessa forma. (`app/chamados/tests/test_catraca_data_evento_cru.py:62`; `app/chamados/tests/test_catraca_data_evento_cru.py:137`; `app/chamados/tests/test_catraca_data_evento_cru.py:28`)

## 11/11 15:39 · sessao 97538cdfafe8 · - · - · origem=Ronald (tmux)

Correção à conclusão anterior: **não há garantia geral de C previamente adquirido pelo passivo**. `carimbar` salva Q diretamente em `app/chamados/models.py:3152`; seu `save` apenas abre atomic e executa `super().save` em `app/chamados/models.py:3422`/`:3423`. Os receivers encontrados agendam becos por `on_commit` (`app/chamados/signals.py:239`) e inserem eventos (`app/chamados/signals.py:258`; `app/chamados/models.py:3248`), sem adquirir C nesses trechos.

| Candidatura | Sequências lidas | Classificação |
|---|---|---|
| C ↔ Q | Transição: C `app/chamados/models.py:290` → S143 → Q `app/chamados/services/disputa_emissao.py:1984` → `app/chamados/models.py:3152`. Passivo: atomic `app/chamados/services/passivo_validado.py:371` → Q `app/chamados/services/materializacao.py:1277` → regeneração `app/chamados/services/passivo_validado.py:438` → órfãs `app/ponto/portas/celula.py:289` → C `app/chamados/services/contexto.py:103`; `app/chamados/models.py:486`. | Inversão estrutural; deadlock exige a **mesma Q** elegível nos dois atos. S143 pula pergunta viva ou resposta sem lastro (`app/chamados/services/disputa_emissao.py:1964`; `:1977`). Elegibilidade simultânea **[nao verificado]**. |
| DP ↔ FM | Passivo pode emitir DP no cartório da regeneração (`app/ponto/portas/celula.py:434`; `app/chamados/services/fabrica_da_celula.py:279`) antes do FM (`app/ponto/portas/regen_exportada.py:179`). Pago/devido: FM `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196` → DP `:207`. | C prévio não pode ser presumido globalmente. Entretanto a possibilidade de emitir DP antes de C, conservar DP e alcançar **o mesmo FM** sem bloqueio anterior depende dos dados/ramificações: **[nao verificado]**. |
| Celula ↔ C | Passivo retém outer `app/chamados/services/passivo_validado.py:371`; regeneração retém outer `app/ponto/portas/regen_exportada.py:169`; UPDATE Celula `app/ponto/portas/celula.py:276` → órfãs/C `:289`. Oposto: C `app/chamados/models.py:290` → cartório → Celula `app/ponto/portas/celula.py:905`. | Candidatura runtime adicional; não excluir por suposta pré-trava universal de C. Compatibilidade dos ramos para os mesmos recursos **[nao verificado]**. |
| DP ↔ Q via S2/passivo | DP durante save do Chamado → S2 posterior `app/chamados/models.py:305`/`:312`. Passivo pode Q antes de outro DP `app/chamados/services/materializacao.py:1277`; `app/chamados/services/passivo_validado.py:443`. | **Não certificar esse par:** S2 seleciona Q com `validada_em IS NULL` (`app/chamados/models.py:310`), enquanto o universo do passivo é validado (`app/chamados/services/passivo_validado.py:3`). Sobreposição da mesma Q por essas duas seleções **[nao verificado]**. |

No ramo `data_hora`, `ch=None` e clones vazios permitem que a função auxiliar não reconcilie chamado algum: guarda de destino em `app/chamados/services/materializacao.py:773`, lista de clones em `:775`/`:776`, chamada auxiliar em `:1059`. O signal de Batida chama o reconciliador (`app/ponto/signals.py:215`), porém os atos dependem de candidatos e premissas; por exemplo, o laço pode apenas continuar em `app/chamados/reconciliador.py:1459`. Isso impede afirmar C obrigatório para todo plantio.

## 11/11 15:38 · sessao 5e6e5744386e · TRIAGEM-OFFLINE-SEM-CAMERA · verificador · origem=Code

**APROVADO para o delta no caminho com `JulianiPWA`.** Os tetos de armazenamento/POST e a prova de conclusão da transação foram introduzidos; permanecem as limitações discriminadas abaixo. (`app/static/pwa.js:55`, `app/static/pwa.js:80`, `app/static/pwa.js:230`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:142`)

1. **(a), (b) e (d): curados nesse caminho.** Abertura tem teto de 2 s e rejeição por bloqueio; transação tem teto, `onabort` e resolução somente em `oncomplete`; o gap tem envelope externo de 4 s. POST e corpo de erro compartilham o orçamento de 30 s. As telas capturam a rejeição da gravação e mostram erro. (`app/static/pwa.js:60`, `app/static/pwa.js:74`, `app/static/pwa.js:84`, `app/static/pwa.js:91`, `app/static/pwa.js:213`, `app/static/pwa.js:232`; `app/templates/ponto/triagem/apto.html:308`; `app/templates/ponto/bater_ponto_v2.html:329`)

   Os estados residuais têm consequências diferentes:

   - **Sync/indicador pendurados:** continuam com pedidos/transações fora de `_transacao`; fetch e corpos do sync também continuam sem teto. Isso pode paralisar envio/indicador, mas não desfaz a fila nem segura o retorno do escritor, que dispara o indicador sem `await`. (`app/static/pwa.js:223`, `app/static/pwa.js:248`, `app/static/pwa.js:271`, `app/static/pwa.js:277`, `app/static/pwa.js:306`)
   - **Sync recebe HTTP 400 e conclui a exclusão:** esse caminho deixa a fila vazia com apenas `console.warn`, sem erro visível. É uma perda silenciosa ainda presente; já consta como pendência própria na cópia. (`app/static/pwa.js:284`; `app/docs/BACKLOG.md:1944`)
   - **`tx.abort()` lança durante commit:** a promessa rejeita mesmo assim, portanto há erro visível. Porém o código não confirma rollback: pode haver gravação tardia após o erro. A ocorrência real desse estado é [nao verificado]. (`app/static/pwa.js:84`; `app/templates/ponto/bater_ponto_v2.html:329`)
   - **Conexão chega tarde:** a abertura já rejeitou; o sucesso tardio fecha a conexão e não retoma aquele escritor. (`app/static/pwa.js:60`, `app/static/pwa.js:72`, `app/static/pwa.js:215`)
   - **Sem `postarBatidaNoTeto`:** permanece `fetch/json` cru; uma promessa pendente deixa botão desabilitado, sem fila e sem erro. O ensaio explicitamente trata ausência de PWA como pendência própria, exigindo apenas preservar o envio online. (`app/templates/ponto/bater_ponto_v2.html:294`, `app/templates/ponto/bater_ponto_v2.html:312`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:552`)

   **(d)** agora marca a fila na conclusão, desfaz linhas no aborto e exige promessa resolvida mais exatamente uma linha persistida. Commit em curso e abertura tardia não são modelados pelo falso IndexedDB. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:142`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:203`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:605`)

2. **(c): permanece sem identidade original, mas isso sozinho não sustenta bloqueante no reenvio de 30 s.** Um clique online pode produzir o POST inicial e uma linha de fila; essa linha pode gerar **N retransmissões**, sem limite declarado, enquanto respostas/exclusões falharem. Quantidade de requisições não equivale à quantidade de batidas gravadas. (`app/templates/ponto/bater_ponto_v2.html:307`, `app/templates/ponto/bater_ponto_v2.html:323`; `app/static/pwa.js:256`, `app/static/pwa.js:276`, `app/static/pwa.js:295`)

   A porta deduplica **mesmo colaborador/mesmo tipo, ±60 s inclusivos**, entre não retratadas. A API usa **mesmo colaborador/mesmo tipo, ±5 minutos inclusivos**, sobre `ts_efetivo`; não sobre a chegada quando existe hora válida do aparelho. (`app/ponto/registro_batida.py:106`; `app/api/views.py:519`, `app/api/views.py:539`)

   Portanto, reenvio **sequencial**, primeira gravação já visível, mesmo tipo e instantes separados por até 30 s é absorvido. Garantia universal permanece [nao verificado]: o teto limita espera da resposta, não o prazo da sincronização; além disso, a dedup precede a eventual correção de tipo. Assim, identidade original fica como pergunta de desenho, sem duplicação demonstrada pelos anexos. (`app/static/pwa.js:230`, `app/static/pwa.js:244`; `app/api/views.py:539`, `app/api/views.py:582`)

3. **Não encontrei nova barreira nem segundo juiz da selfie nesses caminhos.** A lei fixa escrita em `registro_batida.py`; as telas consultam o juiz central e seguem com foto nula. No servidor, `selfie_ausente` acrescenta motivo de revisão, sem recusar a batida. (`CLAUDE.md:143`; `app/static/pwa.js:170`; `app/templates/ponto/triagem/apto.html:248`, `app/templates/ponto/triagem/apto.html:275`; `app/templates/ponto/bater_ponto_v2.html:269`; `app/api/views.py:449`)

4. **O instante único é a entrada do escritor, não necessariamente o primeiro clique.** `Date.now()` alimenta os dois timestamps antes das esperas; na recuperação online, o escritor só é chamado depois do POST frustrado, potencialmente 30 s depois. (`app/static/pwa.js:211`; `app/templates/ponto/bater_ponto_v2.html:293`, `app/templates/ponto/bater_ponto_v2.html:323`)

   O sync envia o timestamp **cru do aparelho**, sem enviar o timestamp corrigido/gap. A API o entrega a `hora_do_aparelho`: atraso entre 0 e 48 h preserva o instante; acima de 3 minutos acrescenta trilha; futuro ou antigo demais usa `agora`. Logo, 30 s de atraso, com relógio compatível, é aceito. O teto de POST não garante sincronização dentro de 30 s. (`app/static/pwa.js:265`; `app/api/views.py:519`; `app/api/hora_do_aparelho.py:62`, `app/api/hora_do_aparelho.py:141`)

5. **Nenhuma das quatro ausências de mutante demonstra, isoladamente, bloqueante de produto.** Fechamento tardio protege conexões futuras; catch interno do gap está protegido, para a fila, pelo envelope externo; catch do indicador é posterior à gravação e sem `await`; instante único está implementado, mas sem prova de regressão temporal. Os mutantes existentes cobrem outras guardas, não essas quatro. (`app/static/pwa.js:72`, `app/static/pwa.js:134`, `app/static/pwa.js:166`, `app/static/pwa.js:212`, `app/static/pwa.js:223`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:810`)

6. **O template apagado não tem referência executável encontrada na cópia.** Restam comentário do guard, comentário histórico do teste e tabela documental. URLs apontam à view, que escolhe v2 ou triagem. O SW referencia `/ponto/bater/`, não o nome do template; seu ramo de navegação retorna antes do trecho que alimentaria esse cache. (`app/scripts/check_a_lei_batida.py:20`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:673`; `app/docs/TICKETS.md:693`; `app/ponto/urls.py:11`; `app/ponto/views.py:225`, `app/ponto/views.py:356`; `app/static/sw.js:45`, `app/static/sw.js:67`)

   Há lacunas nos instrumentos alterados: o scanner não inclui apto/v2; o guard não inclui v2 e pula caminhos inexistentes. Elas não demonstram quebra causada pela exclusão. (`app/tools/check_frontend_urls.py:25`; `app/scripts/check_a_lei_batida.py:14`, `app/scripts/check_a_lei_batida.py:21`, `app/scripts/check_a_lei_batida.py:31`)

Suíte verde e smoke nesta cópia: **[nao verificado]**. Não executei o ensaio Chromium porque ele cria arquivos/perfil e depois os apaga. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:344`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:360`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:372`)

## 11/11 15:38 · sessao 86de3be02df9 · LASTRO-FOLGA-FORA-DO-CATALOGO · verificador · origem=Code

**Veredito: BLOQUEANTES B1–B3.** A guarda consulta a autoridade existente, mas herda um cache sem isolamento por tenant, introduz uma exceção sem tradução em portas HTTP e permite reintroduzir legado numa corrida entre leitura e gravação (`app/ponto/catalogo/ausencias.py:257`, `app/ponto/models.py:883`, `app/ponto/services/ausencia.py:418`, `app/api/views.py:1825`).

**1. Quem escreve `Ausencia.tipo`**

A construção e persistência estão em `criar_ausencia`; a troca do campo está em `editar_ausencia_registro`. Ambas validam pelo catálogo antes de salvar (`app/ponto/services/ausencia.py:194`, `app/ponto/services/ausencia.py:342`, `app/ponto/services/ausencia.py:418`, `app/ponto/services/ausencia.py:474`, `app/ponto/services/ausencia.py:498`, `app/ponto/services/ausencia.py:509`).

| Chamador produtivo encontrado | Valor fornecido |
|---|---|
| Lançamento administrativo | Variável do formulário (`app/ponto/views.py:2415`) |
| Solicitação PWA | Variável da solicitação (`app/ponto/views.py:2536`) |
| API | Variável, após tradução dos aliases legados (`app/api/views.py:1765`, `app/api/views.py:1817`) |
| Copiloto | Variável encaminhada ao produtor (`app/ponto/services/checar_ausencia.py:46`) |
| Veredito da célula | Literal `falta` ou variável `tipo_ausencia` (`app/ponto/services/veredito_celula.py:332`) |
| Materialização de declaração | `falta` ou `atestado`, pelo mapa (`app/chamados/services/materializacao.py:490`, `app/chamados/models.py:3716`) |
| Materialização de resposta | `falta` ou `atestado`, pela tabela de escapes (`app/chamados/services/materializacao.py:526`, `app/chamados/services/materializacao.py:602`) |
| Espelho de férias | Literal `ferias`, pelo produtor (`app/ponto/services/ausencia.py:1202`) |
| Regularização de retorno | Literal `falta`, por `garantir_ausencia` (`app/ponto/services/regularizacao.py:118`, `app/ponto/services/ausencia.py:461`) |
| Comando de abandono provisório | Literal `falta`, por `garantir_ausencia` (`app/ponto/management/commands/marcar_abandono_provisorio.py:76`) |
| Edição pela UI | POST ou tipo atual, pela porta de edição (`app/ponto/views.py:2794`) |
| Comando de rederivação H06 | Tipo atual, pela porta de edição (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`) |

Os literais efetivamente produzidos pelos caminhos fixos encontrados estão semeados:

| Literal | Semente |
|---|---|
| `falta` | Sim (`app/ponto/migrations/0052_seed_tipoausencia.py:10`) |
| `atestado` | Sim (`app/ponto/migrations/0052_seed_tipoausencia.py:17`) |
| `ferias` | Sim (`app/ponto/migrations/0052_seed_tipoausencia.py:23`) |
| `declaracao`, destino do alias `atestado_acompanhante` | Sim (`app/api/views.py:1766`, `app/ponto/migrations/0052_seed_tipoausencia.py:18`) |
| `licenca_luto`, destino do alias `obito` | Sim (`app/api/views.py:1767`, `app/ponto/migrations/0052_seed_tipoausencia.py:20`) |

`atestado_medico` é traduzido para `atestado`; os aliases não são escritos literalmente pela API (`app/api/views.py:1765`). Não há incompatibilidade demonstrada entre esses literais e a semente; estarem ativos hoje em produção é **[não verificado]**.

O admin de Ausência bloqueia adicionar, alterar e apagar; os receivers de Ausência consomem o fato, sem atribuir `tipo` (`app/ponto/admin.py:38`, `app/core/admin.py:59`, `app/core/admin.py:62`, `app/core/admin.py:65`, `app/ponto/signals.py:143`, `app/ponto/signals.py:275`). O `update` produtivo de cancelamento de férias altera somente status e carimbo (`app/ponto/services/ausencia.py:1281`). A cópia CSV encontrada contorna o modelo, mas pertence ao pipeline de ensaio (`bin/sombra_incremental.sh:58`, `bin/sombra_incremental.sh:62`).

**O escritor histórico de #5356 permanece [não verificado].** Os testes demonstram que `QuerySet.update(tipo=...)` consegue fabricar esse legado, mas isso não identifica a origem produtiva (`app/ponto/tests/test_lastro_folga_save_guarda.py:67`).

**2. Catálogo, cache e cadastro pela UI — B1**

Os leitores diretos de `codigos_validos()` são formulário, prévia do copiloto, criação, edição, composição dos tipos que exigem documento, vigia, solicitação PWA e agora a guarda do modelo (`app/ponto/forms.py:63`, `app/ponto/services/checar_ausencia.py:23`, `app/ponto/services/ausencia.py:194`, `app/ponto/services/ausencia.py:474`, `app/ponto/services/ausencia.py:804`, `app/ponto/services/vigia_ausencia.py:103`, `app/ponto/views.py:2509`, `app/ponto/models.py:879`).

O cache é um único dicionário de módulo, com TTL de **60 segundos medidos por `monotonic()`**. A criação/edição pela UI salva `TipoAusencia`, cujo signal invalida esse dicionário no processo local (`app/ponto/catalogo/ausencias.py:257`, `app/ponto/catalogo/ausencias.py:258`, `app/ponto/catalogo/ausencias.py:275`, `app/ponto/views_tipos_ausencia.py:94`, `app/ponto/services/ausencia.py:1310`, `app/ponto/models.py:1260`).

Consequências:

- No processo invalidado, a próxima leitura consulta novamente o banco; outro processo com cache quente pode recusar o tipo recém-criado até expirar o TTL. Também pode continuar aceitando um tipo desativado nesse intervalo (`app/ponto/catalogo/ausencias.py:263`, `app/ponto/catalogo/ausencias.py:276`, `app/ponto/models.py:879`).
- **B1:** a chave não inclui tenant/schema. Com `ponto` instalado por tenant, uma leitura no tenant B pode reutilizar o catálogo anteriormente carregado no tenant A, admitindo código alheio ou recusando código próprio (`app/config/settings/saas.py:34`, `app/config/settings/saas.py:40`, `app/config/settings/saas.py:68`, `app/ponto/catalogo/ausencias.py:257`, `app/ponto/catalogo/ausencias.py:276`).
- Tabela vazia, todos os tipos inativos **ou qualquer exceção na consulta** fazem retornar a semente congelada. Portanto, vazio não significa “nenhum código permitido”; inclusive desativar todos reabilita os códigos da semente na autoridade (`app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:252`, `app/ponto/catalogo/ausencias.py:253`, `app/ponto/catalogo/ausencias.py:278`).

A guarda herda esses comportamentos de uma autoridade preexistente; ocorrência concreta entre workers/tenants de produção é **[não verificado]** (`app/ponto/models.py:878`).

**3. Consulta histórica, corrida e contornos — B3**

Com cache quente, código válido não exige a consulta histórica. Código fora do catálogo em instância persistida acrescenta **um SELECT por chamada de save**; cache frio acrescenta também a consulta do catálogo. Uma decisão pode chamar save duas vezes, pagando duas consultas históricas para legado (`app/ponto/models.py:879`, `app/ponto/models.py:882`, `app/ponto/catalogo/ausencias.py:249`, `app/ponto/services/ausencia.py:911`, `app/ponto/services/ausencia.py:927`). Latência e SQL efetivamente emitido são **[não verificado]**.

**B3 — comparação e gravação não são atômicas.** Interleaving permitido pelo código: A consulta e encontra `folga`; B corrige para `abono`; A termina seu save completo e recoloca `folga`. A guarda não usa lock nem UPDATE condicionado ao tipo observado (`app/ponto/models.py:883`, `app/ponto/models.py:885`, `app/ponto/models.py:933`). Ocorrência produtiva é **[não verificado]**.

Quanto aos contornos:

- `update_fields` **não desliga** a guarda. Mesmo salvando só documento/status, ela compara o tipo em memória com o banco; pode recusar uma operação que nem escreveria `tipo`, caso outra operação já tenha corrigido o legado (`app/ponto/models.py:891`, `app/ponto/models.py:893`, `app/ponto/services/ausencia.py:835`, `app/ponto/services/ausencia.py:859`).
- `QuerySet.update(tipo=...)` contorna a guarda, demonstrado expressamente pelas fixtures (`app/ponto/tests/test_lastro_folga_save_guarda.py:67`).
- O override não contém proteção em `save_base`, bulk ou SQL direto. A implementação dessas APIs Django não está demonstrada pelos arquivos examinados; confirmação de `save_base(raw=True)`/`loaddata`, `bulk_create` e `bulk_update` nesta cópia: **[não verificado]**. Não há exceção explícita para `raw` dentro da guarda (`app/ponto/models.py:868`, `app/ponto/models.py:891`).

**4. Exceção HTTP e batida — B2**

**B2 — a nova exceção não é traduzida consistentemente.** Na criação, somente o `ValidationError` do `full_clean` vira `AusenciaInvalida`; o save posterior limpa eventual anexo e relança a exceção original. A API captura apenas `AusenciaInvalida` (`app/ponto/services/ausencia.py:387`, `app/ponto/services/ausencia.py:418`, `app/ponto/services/ausencia.py:419`, `app/ponto/services/ausencia.py:429`, `app/api/views.py:1825`).

Cenário concreto permitido: o código passa na validação inicial; outra thread desativa o tipo e invalida o cache; o save consulta o catálogo atualizado e lança o novo `ValidationError`, que escapa da API. O caminho de edição tem a mesma janela e sua view também não captura essa exceção (`app/ponto/services/ausencia.py:194`, `app/ponto/models.py:1263`, `app/ponto/models.py:887`, `app/ponto/services/ausencia.py:474`, `app/ponto/services/ausencia.py:509`, `app/ponto/views.py:2818`). Resposta 500 observada em produção: **[não verificado]**.

Outros propagadores são veredito da célula, espelho de férias, mudanças de estado, decisão e anexação; nenhum deles traduz especificamente o novo erro do catálogo (`app/ponto/services/veredito_celula.py:340`, `app/ponto/services/ausencia.py:1209`, `app/ponto/services/ausencia.py:835`, `app/ponto/services/ausencia.py:927`, `app/ponto/services/ausencia.py:865`). Legado inalterado passa normalmente; a falha exige mudança concorrente ou outro desacordo entre instância e persistência (`app/ponto/models.py:885`).

Lançamento e submissão web têm captura externa de `Exception`, portanto não cabe afirmar 500 indistintamente nessas telas (`app/ponto/views.py:2458`, `app/ponto/views.py:2558`).

**Não foi demonstrado caminho da batida até `Ausencia.save`.** O chokepoint cria `Batida`; recompute, julgamento da célula, reconciliação e fechamento por evento têm contenção de exceções nos caminhos examinados (`app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:175`, `app/ponto/signals.py:19`, `app/ponto/signals.py:85`, `app/ponto/signals.py:131`, `app/ponto/signals.py:206`). Violação nova da lei “batida nunca barrada” por esta guarda: **[não verificado]**.

**5. Segundo juiz e dois significados**

A guarda é **leitora da mesma autoridade de validade**, pois chama `codigos_validos`, exatamente como criar e editar. `faixa_de` responde outra pergunta, a precedência; seu default continua sendo `atestado` para código desconhecido (`app/ponto/models.py:878`, `app/ponto/services/ausencia.py:194`, `app/ponto/services/ausencia.py:474`, `app/ponto/precedencia.py:76`).

A divergência estrutural com `TIPO_CHOICES` permanece: o campo declara choices congeladas, enquanto o serviço exclui `tipo` do `full_clean` para aceitar códigos vivos adicionais. A guarda não cria essa segunda lista; o teste novo preserva explicitamente código vivo fora das choices (`app/ponto/models.py:661`, `app/ponto/models.py:749`, `app/ponto/services/ausencia.py:387`, `app/ponto/tests/test_lastro_folga_save_guarda.py:134`).

**6. Fixtures e mordida**

As alterações preservam as asserções originais relevantes:

- Carimbo: mantém `obito` legado e exige `aprovada_em` na aprovação/rejeição (`app/ponto/tests/test_decisao_carimba.py:22`, `app/ponto/tests/test_decisao_carimba.py:32`, `app/ponto/tests/test_decisao_carimba.py:39`).
- Espelho: mantém `obito` e compara a apresentação com o juiz do status (`app/ponto/tests/test_caracterizacao_espelho.py:53`, `app/ponto/tests/test_caracterizacao_espelho.py:89`).
- Fechamento: mantém `atestado_medico` desconhecido e exige DSR perdido igual a zero (`app/ponto/tests/test_fechamento_ausencia_pela_autoridade.py:83`, `app/ponto/tests/test_fechamento_ausencia_pela_autoridade.py:86`).
- Cadastro/dropdown: acrescentam cleanup de cache e mantêm admissão de novo código, retirada do inativo e fallback vazio (`app/ponto/tests/test_cadastro_tipo.py:15`, `app/ponto/tests/test_cadastro_tipo.py:23`, `app/ponto/tests/test_cadastro_tipo.py:35`, `app/ponto/tests/test_dropdown_vivo.py:21`, `app/ponto/tests/test_dropdown_vivo.py:29`, `app/ponto/tests/test_dropdown_vivo.py:34`).

Há uma diferença: as fixtures agora disparam primeiro os efeitos de `abono` e depois os do legado. O segundo save executa os receivers, mas equivalência completa dos efeitos intermediários é **[não verificado]** (`app/ponto/tests/test_caracterizacao_espelho.py:50`, `app/ponto/tests/test_caracterizacao_espelho.py:55`, `app/ponto/signals.py:143`, `app/ponto/signals.py:275`).

O selo novo tem asserções que detectariam remoção da guarda e quebra da preservação histórica; não cobre os cenários B1–B3. Execução verde, RED anterior e mutantes executados são **[não verificado]** (`app/ponto/tests/test_lastro_folga_save_guarda.py:73`, `app/ponto/tests/test_lastro_folga_save_guarda.py:94`, `app/ponto/tests/test_lastro_folga_save_guarda.py:102`, `app/ponto/tests/test_lastro_folga_save_guarda.py:134`).

**7. Classes de reprovação**

Os bloqueantes são: **B1**, cache usado pela barreira sem isolamento por tenant e sem invalidação entre processos; **B2**, exceção nova sem tradução nas portas; **B3**, leitura histórica separada da gravação, permitindo reintrodução concorrente do código fora do catálogo (`app/ponto/catalogo/ausencias.py:257`, `app/ponto/models.py:1263`, `app/api/views.py:1825`, `app/ponto/models.py:883`, `app/ponto/models.py:933`).

Não foi demonstrado segundo produtor produtivo de `tipo`; há contorno por `update` nas fixtures e duplicidade preexistente entre choices e catálogo (`app/ponto/tests/test_lastro_folga_save_guarda.py:67`, `app/ponto/models.py:749`). O diff não acrescenta relógio, mas passa a depender do TTL preexistente na decisão de persistência (`app/ponto/models.py:878`, `app/ponto/catalogo/ausencias.py:275`). Origem de #5356, censo real da frota, suite completa e pré-requisitos de pouso permanecem **[não verificado]**.

## 11/11 15:37 · sessao d270727d20b6 · - · - · origem=Ronald (tmux)

Meu julgamento é **APROVADO no escopo da cura com `JulianiPWA`**, sem afirmar que todos os caminhos de rede/armazenamento ficaram limitados. A abertura rejeita no teto/bloqueio; a transação rejeita no teto/aborto; o POST e seu corpo de erro têm teto. (`app/static/pwa.js:60-74`; `app/static/pwa.js:83-94`; `app/static/pwa.js:230-241`)

- **Aborto lançando durante commit:** a exceção é engolida, mas a promessa rejeita mesmo assim e a tela mostra erro. Pode restar gravação tardia, contrariando a garantia de “nenhum fantasma”; não produz clique pendente ou perda silenciosa. Ocorrência real desse estado: [nao verificado]. (`app/static/pwa.js:84-93`; `app/templates/ponto/bater_ponto_v2.html:329-331`)
- **Conexão tardia:** a abertura já rejeitou; `onsuccess` tardio fecha a conexão. Não volta a executar a gravação daquele escritor. (`app/static/pwa.js:60-72`; `app/static/pwa.js:215-216`)
- **Sync/indicador:** os pedidos e transações continuam fora de `_transacao`, assim como o fetch/corpos do sync. Pendência neles não desfaz a fila já gravada nem segura o retorno do escritor. Há perda silenciosa explícita se o sync recebe HTTP 400 e a exclusão completa: só há `console.warn`. (`app/static/pwa.js:223`; `app/static/pwa.js:248-253`; `app/static/pwa.js:271-310`)
- **Exceção importante de (b):** sem `postarBatidaNoTeto`, a v2 continua fazendo `fetch` e `json` sem teto; resposta pendente deixa botão desabilitado, sem fila e sem erro. É caminho residual demonstrável pelo código. O próprio ensaio declara PWA ausente como outra fila e só exige preservar o envio online; portanto não o apresentaria como regressão nova desta cura. (`app/templates/ponto/bater_ponto_v2.html:294-295`; `app/templates/ponto/bater_ponto_v2.html:312-314`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:552-557`)
- **(c):** um clique pode gerar POST inicial e uma linha reenviável repetidamente. Isso não equivale a múltiplas linhas no servidor: reenvio sequencial do mesmo tipo, com instantes separados por 30 s e primeira gravação visível, cabe em ±5 min da API e ±60 s da porta. Falta de identidade isolada não sustenta bloqueante nesse cenário; garantia para relógio incompatível, correção de tipo ou concorrência permanece [nao verificado]. (`app/templates/ponto/bater_ponto_v2.html:307-323`; `app/static/pwa.js:256-298`; `app/api/views.py:539-555`; `app/ponto/registro_batida.py:106-115`)
- **(d)/mordidas:** o ensaio agora marca fila na conclusão e exige promessa resolvida/persistência, mas seu falso IDB não modela commit em curso nem fechamento tardio. Essas limitações impedem certificação universal; sozinhas não demonstram novo bloqueante de produto. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:142-153`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:195-215`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:605-616`)

## 11/11 15:37 · sessao b199bd514621 · - · - · origem=Ronald (tmux)

| Recurso adicional | Ordem demonstrada no código | Classificação do par |
|---|---|---|
| TurnoMaterializado | Passivo: atomic do par → materializador → INSERT Batida → receiver do núcleo → DELETE/INSERT TurnoMaterializado → cartório/fábrica → eventual C. (`app/chamados/services/passivo_validado.py:371`; `:427`; `:511`; `:516`; `app/chamados/services/materializacao.py:1220`; `:1007`; `app/ponto/registro_batida.py:157`; `app/ponto/signals.py:36`; `app/ponto/nucleo.py:58`; `:59`; `:81`; `app/ponto/signals.py:135`; `app/chamados/services/fabrica_da_celula.py:365`) | **Candidato TM→C contra C→TM**: aprovação de justificativa toma C antes de registrar Batida e recomputar os mesmos turnos. Compatibilidade das travas implícitas de FK e execução concorrente **[nao verificado]**. (`app/ponto/services/justificativa.py:140`; `:141`; `:129`; `:187`; `app/ponto/signals.py:36`; `app/ponto/nucleo.py:81`) |
| Justificativa | C/D/Ch → Justificativa FOR UPDATE → eventual Batida/TM → cartório/Pauta DP → UPDATE Justificativa → UPDATE FM. (`app/ponto/services/justificativa.py:129`; `:140`; `:142`; `:187`; `:208`; `:215`; `app/ponto/services/fechamento.py:1656`) | **Inversão abstrata DP→FM contra FM→DP**, mas a aprovação e o pago/devido adquirem C antes; para mesma linha FM, o mesmo colaborador serializa os dois atos. (`app/ponto/services/justificativa.py:129`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`; `:196`; `:207`) |
| Justificativa na materialização retroativa | Atomic → C/D/Ch → Justificativa FOR UPDATE → registrar Batida/TM → UPDATE Justificativa/cartório → UPDATE FM. (`app/ponto/services/justificativa.py:342`; `:343`; `:344`; `:361`; `:386`; `:390`; `app/ponto/services/fechamento.py:1656`) | Mesma ressalva de serialização por C para o par DP/FM. (`app/ponto/services/justificativa.py:129`; `:343`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`) |
| Ausencia | Aprovação de intervalo: atomic → UPDATE Ausencia → cartório síncrono → possível Pauta DP → invalidar FM. A porta da ausência não adquire C antes do UPDATE. (`app/ponto/views.py:2641`; `:2643`; `app/ponto/services/ausencia.py:911`; `:835`; `app/ponto/signals.py:143`; `:147`; `app/ponto/services/ausencia.py:946`; `app/ponto/services/fechamento.py:1656`) | **Candidato DP→FM sem C**, quando a fábrica segue por limbo exportado, que escreve DP diretamente; satisfatibilidade conjunta dos guards com aprovação parcial **[nao verificado]**. (`app/chamados/services/fabrica_da_celula.py:214`; `:228`; `:231`; `:279`; `app/ponto/services/ausencia.py:924`) |
| Ausencia em rejeição/cancelamento | A porta também invalida FM após rejeitar, mas as views lidas não envolvem o ato inteiro em atomic. (`app/ponto/views.py:2700`; `:2701`; `:2734`; `:2351`; `app/ponto/services/ausencia.py:946`) | **Não comprova retenção de DP até FM** nessas entradas. `Ausencia.save` termina em `super().save`, sem atomic próprio envolvendo a decisão inteira. (`app/ponto/models.py:868`; `:908`; `app/ponto/services/ausencia.py:835`; `:946`) |
| Ausencia de férias | Cancelamento: Agendamento → PeriodoAquisitivo → UPDATE/DELETE Ausencia de tipo `ferias` → signals/cartório → eventual C/DP. (`app/ferias/services.py:364`; `:367`; `:402`; `:411`; `app/ponto/services/ausencia.py:1281`; `:1283`; `app/ferias/models.py:451`; `app/ponto/services/ausencia.py:1183`; `:1190`; `app/ponto/signals.py:151`) | **Não demonstrar C↔Ausencia usando escapes de resposta**: esses escapes escrevem apenas `falta`/`atestado`, não a mesma linha `ferias`. (`app/chamados/services/materializacao.py:597`; `:598`; `:599`; `app/ponto/services/ausencia.py:1183`) |

No ramo ESCAPE, a trava C do `responder_pergunta_disputa_core` está em um atomic que termina antes da chamada de materialização da ausência. O caminho web lido, portanto, não comprova C retido durante esse UPDATE de Ausencia. (`app/chamados/services/disputa_emissao.py:1503`; `:1511`; `:1541`; `app/chamados/views.py:1228`; `:1283`)

O recálculo FM por evento de Batida continua em `on_commit`; ele não demonstra FM síncrono depois de DP no atomic anterior. A invalidação FM das duas portas de justificativa acima é síncrona e constitui caminho distinto. (`app/ponto/registro_batida.py:175`; `app/ponto/services/fechamento.py:1824`; `app/ponto/services/justificativa.py:215`; `:390`; `app/ponto/services/fechamento.py:1656`)

## 11/11 15:36 · sessao e3b00d6965c5 · - · - · origem=Ronald (tmux)

Censo de escritores concluído, somente por leitura.

| Sítio | `tipo` que fornece | Seed |
|---|---|---|
| `app/ponto/services/ausencia.py:341`, `:385`, `:418` | Variável: constrói e salva a linha nova | Validada em `:193-195` |
| `app/ponto/services/ausencia.py:498`, `:509` | Variável: troca do tipo existente | Validada em `:473-475` |
| `app/ponto/views.py:2415` | Variável do lançamento administrativo | Passa pelo produtor |
| `app/ponto/views.py:2536` | Variável da solicitação PWA | Passa pelo produtor |
| `app/api/views.py:1817` | Variável da API | Passa pelo produtor |
| `app/ponto/services/checar_ausencia.py:46` | Variável do copiloto; chamada HTTP em `app/core/views_ia.py:985` | Passa pelo produtor |
| `app/ponto/services/veredito_celula.py:332`, `:336` | `'falta'` ou variável `tipo_ausencia` | `'falta'`: `app/ponto/migrations/0052_seed_tipoausencia.py:10` |
| `app/chamados/services/materializacao.py:490` | `'falta'`/`'atestado'`, mapa em `app/chamados/models.py:3716` | Seed `0052:10`, `:17` |
| `app/chamados/services/materializacao.py:526` | `'falta'`/`'atestado'`, mapa em `:602`; consumidor `app/chamados/services/disputa_emissao.py:1541` | Seed `0052:10`, `:17` |
| `app/ponto/services/ausencia.py:1202` | `'ferias'` | Seed `0052:23` |
| `app/ponto/services/regularizacao.py:118` | `'falta'`, por `garantir_ausencia` | Seed `0052:10` |
| `app/ponto/management/commands/marcar_abandono_provisorio.py:76` | `'falta'` | Seed `0052:10` |
| `app/ponto/views.py:2794` | Edição: POST ou tipo atual | Usa `editar_ausencia_registro` |
| `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169` | Edição: `a.tipo` atual | Usa `editar_ausencia_registro` |

Os únicos literais produtores encontrados foram `falta`, `atestado` e `ferias`, todos semeados; o catálogo da seed grava `ativo=True`. `app/ponto/migrations/0052_seed_tipoausencia.py:10`, `:17`, `:23`, `:36`.

A API recebe nomes legados, mas traduz antes de escrever: `atestado_medico → atestado`, `atestado_acompanhante → declaracao`, `obito → licenca_luto`; os destinos estão semeados. Isso não constitui escritor de literal desconhecido. `app/api/views.py:1765`; `app/ponto/migrations/0052_seed_tipoausencia.py:17`, `:18`, `:20`.

Férias materializam pela porta: `_sync_ausencia → espelhar_ferias → criar_ausencia(tipo='ferias')`. `app/ferias/models.py:386`, `:391`; `app/ponto/services/ausencia.py:1202`.

O admin de Ausencia é somente leitura, com bloqueio de adicionar/alterar/apagar; os signals de Ausencia encontrados consomem o fato. `app/ponto/admin.py:38`; `app/core/admin.py:59`, `:62`, `:65`; `app/ponto/signals.py:143`, `:275`.

Há `queryset.update` produtivo de Ausencia para rejeitar férias, mas altera apenas `status`/`atualizado_em`, sem escrever `tipo`. `app/ponto/services/ausencia.py:1281`.

Fora do app, `sombra_incremental.sh` copia `ponto_ausencia` incluindo `tipo`, por CSV, contornando o modelo; o destino declarado é o banco de ensaio. `bin/sombra_incremental.sh:14`, `:36`, `:57`, `:62`. A máscara genérica usa bulk, mas os campos de Ausencia declarados são `documento` e `descricao`. `bin/sombra_regras_pessoais.py:64`, `:109`.

Não encontrei outro escritor produtivo/importador de `tipo` na cópia; os contratos existentes fiscalizam construção direta e escritas fora da porta, mas não foram executados nesta auditoria. `app/ponto/tests/test_contract_um_produtor_de_ausencia.py:49`, `:74`, `:88`; `app/ponto/tests/test_chokepoint_ausencia_gate.py:53`.

A origem específica da escrita de #5356 permanece **[não verificado]**.

## 11/11 15:36 · sessao 62b4dee2cfd9 · - · - · origem=Ronald (tmux)

(4) **Há caminhos HTTP que não traduzem o novo `ValidationError`, mas o legado sem troca de tipo passa normalmente.** A guarda permite tipo existente idêntico (`app/ponto/models.py:882–889`); portanto, não é correto afirmar que decidir/cancelar qualquer ausência legada passará a dar 500.

- `criar_ausencia`: traduz apenas `ValidationError` de `full_clean` (`app/ponto/services/ausencia.py:385–391`). O `save()` está depois, em `:418`; o `except Exception` limpa anexo e relança exceções diferentes da constraint conhecida (`:419–429`). API captura apenas `AusenciaInvalida` (`app/api/views.py:1817–1844`). Se o catálogo mudar/expirar entre a validação inicial e o save, o novo `ValidationError` escapa dessa API; ocorrência real [não verificado].
- `editar_ausencia_registro`: valida vivo em `:474–475`, atribui tipo em `:498`, salva sem tradução em `:509`. A view captura somente `AusenciaInvalida`, `ValueError`, `TypeError` (`app/ponto/views.py:2818`). Mesma janela de corrida; ocorrência real [não verificado].
- `veredito_celula`: chama produtor e captura só `AusenciaInvalida` (`app/ponto/services/veredito_celula.py:331–341`); a view chama o serviço sem catch (`app/ponto/views.py:3339–3341`).
- Decisão/status: `mudar_estado` salva sem catch (`app/ponto/services/ausencia.py:835`), `decidir` pode salvar novamente (`:927`). Lotes aprovação/rejeição capturam apenas `DoesNotExist` (`app/ponto/views.py:2293`, `:2324`); lote cancelamento não envolve a chamada com catch (`:2351`). Reverter captura domínio, não Django VE (`:2588`); período captura domínio/ValueError/DoesNotExist (`:2674`); cancelar individual só DoesNotExist (`:2745`). **Sem troca concorrente de tipo/remoção da linha, a exceção histórica da guarda permite esses saves** (`app/ponto/models.py:882–889`).
- Anexo: save é protegido apenas para limpeza e re-raise (`app/ponto/services/ausencia.py:859–865`); chamada da UI sem catch (`app/ponto/views.py:2151`) e reenvio da API fora do try de criação (`app/api/views.py:1848–1850`). Mesmo limite de concorrência para linha legada.
- Lançar/submeter pela web capturam `Exception` (`app/ponto/views.py:2458`, `:2558`); não classificá-los genericamente como 500.
- Django admin não escreve Ausência: usa `SoLeitura` (`app/ponto/admin.py:37–47`), com add/change/delete negados (`app/core/admin.py:59–66`).

**Batida:** não encontrei caminho produtor de Ausência nos receivers examinados; o chokepoint cria `Batida`, não `Ausencia` (`app/ponto/registro_batida.py:152–160`). Pós-evento protegido por catch genérico (`:175–183`). Núcleo recompõe somente `TurnoMaterializado` (`app/ponto/nucleo.py:40–67`), protegido no signal (`app/ponto/signals.py:19–31`). Cartório da batida entra em `engolir` (`app/ponto/signals.py:85–93`, `:131–135`); reconciliador e adesão protegidos separadamente (`:206–237`). Realtime é protegido internamente (`app/ponto/realtime.py:22–46`). Onboarding grava `Colaborador` (`app/colaboradores/signals.py:86–102`); seu signal downstream trata silêncio de chamados, não ausência (`app/chamados/signals.py:268–279`). **Nenhuma barreira nova de Ausencia à batida demonstrada nesses caminhos.**

O mapeamento não prova ausência de qualquer outra falha antiga na batida: onboarding não tem catch próprio (`app/colaboradores/signals.py:101–102`); isso não é efeito da guarda nova.

Férias: espelho usa `criar_ausencia(tipo='ferias')`, captura só domínio e registra recusas (`app/ponto/services/ausencia.py:1201–1216`); `AgendamentoFerias.save` chama sync após salvar agendamento (`app/ferias/models.py:446–451`). A criação com `ferias` inativo já era recusada no precheck; só uma mudança entre precheck/save introduz o novo VE bruto. Remarcação da view captura apenas `AgendamentoNaoRemarcavel` (`app/ferias/views.py:686–690`).

## 11/11 15:36 · sessao 61e4e870505c · - · - · origem=Ronald (tmux)

Fixtures preservam a mordida original na leitura estática:

- **Carimbo:** continua usando `obito` legado, depois executa `save()` de aprovação/rejeição e exige `aprovada_em`; o estado pendente continua exigindo ausência do carimbo. A mudança foi na montagem da linha, sem retirar asserções. `app/ponto/tests/test_decisao_carimba.py:16-43`.
- **Espelho:** mantém `obito` e o compara com o status lavrado pelo juiz; preserva verificações de turnos, batidas e resumo. `app/ponto/tests/test_caracterizacao_espelho.py:48-55`, `:89-101`.
- **Fechamento:** mantém o desconhecido `atestado_medico` e exige DSR perdido igual a zero; os testes de ordem entre falta e atestado continuam presentes. `app/ponto/tests/test_fechamento_ausencia_pela_autoridade.py:57-87`.
- **Cadastro/dropdown:** apenas acrescentam limpeza do cache ao fim; continuam exigindo admissão imediata de código novo, remoção do inativo e fallback quando a tabela está vazia. `app/ponto/tests/test_cadastro_tipo.py:11-37`; `app/ponto/tests/test_dropdown_vivo.py:16-49`.
- **Diferença de fixture:** agora há um primeiro julgamento com `abono`, seguido de julgamento com legado. Os receivers de Ausencia não distinguem `created`, portanto o segundo `save()` executa seus consumidores; equivalência completa de todos os efeitos intermediários [não verificado]. `app/ponto/tests/test_caracterizacao_espelho.py:50-55`; `app/ponto/signals.py:143-147`, `:275-279`.

O selo novo tem asserções que falhariam sem a guarda: rejeita criação direta, `objects.create`, troca de vivo para morto e troca entre mortos; também protege história, desativação posterior e código vivo fora de choices. `app/ponto/tests/test_lastro_folga_save_guarda.py:73-142`. Execução verde, RED histórico e mutantes efetivamente executados [não verificado].

Lacunas relevantes:

1. **Corrida:** leitura do tipo e persistência não estão protegidas pela própria guarda com lock/transação. Interleaving possível: A lê legado `folga`; B corrige para `abono`; A passa na comparação e salva novamente `folga`. Ocorrência em produção [não verificado]. `app/ponto/models.py:879-893`, `:933`.
2. **`update_fields` não contorna:** a guarda roda antes de analisar os argumentos do `save`; até atualização apenas de descrição valida o `self.tipo` em memória e consulta o legado. Pode recusar uma alteração em memória que nem seria persistida. `app/ponto/models.py:891-893`, `:933`.
3. **`QuerySet.update` contorna de fato:** a própria fixture demonstra a gravação do tipo morto, seguida de leitura confirmando o valor. `app/ponto/tests/test_lastro_folga_save_guarda.py:63-69`.
4. **`raw`/bulk:** semântica de `save_base(raw=True)` e de bulk não foi confirmada no código instalado, pois Django não está disponível no host; dependência declarada é Django 5.2.14. `app/requirements.txt:13`; comportamento executado [não verificado].
5. **Multi-db:** a guarda ignora `using` na consulta histórica. As configurações examinadas só declaram alias `default`, portanto impacto real multi-db [não verificado]. `app/ponto/models.py:883-884`; `app/config/settings/saas.py:83-93`; `app/config/settings/prod.py:9-18`.
6. **Custo:** fora do vivo e com instância persistida, há uma consulta extra ao tipo; com cache expirado também há leitura do catálogo. O `Meta.ordering` inclui `colaborador__nome`, e a consulta histórica não remove a ordenação; SQL efetivamente emitido [não verificado]. `app/ponto/models.py:819`, `:882-884`; `app/ponto/catalogo/ausencias.py:249-252`, `:275-280`.

Os novos testes não exercitam concorrência, `update_fields`, processos/tenants diferentes ou bulk/raw. `app/ponto/tests/test_lastro_folga_save_guarda.py:73-142`.

## 11/11 15:36 · sessao 05d6cdb86f93 · - · - · origem=Ronald (tmux)

- **(d) curado no desenho do ensaio:** a marca de fila ocorre em `_fimTx`, imediatamente antes de `oncomplete`; `add` apenas anota linha provisória; `abort` desfaz essa linha; o verificador exige promessa resolvida e exatamente uma linha persistida. `tx_lento` e Mi2 detectam gravação tardia quando o aborto é retirado. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:142`, `:152`, `:178`, `:203`, `:605`, `:613`, `:823`.) Execução verde nesta cópia: [nao verificado].

- **Limite da prova:** o falso IndexedDB abre em 1 ms e `close()` é vazio; `abort()` só lança quando `_feito` já está verdadeiro. Portanto não prova fechamento de conexão tardia nem a situação “commit iniciado, aborto lança, `oncomplete` ainda pendente”. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:195`, `:204`, `:210`, `:215`.)

- **As quatro guardas sem mutante:** os mutantes existentes cobrem abertura/teto/bloqueio, transação/teto/aborto, guarda externa do gap, POST e corpo de erro; não há mutante para as quatro guardas indicadas. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:810`, `:830`, `:842`.)  
  - `db.close()` tardio protege futuras aberturas/versões; a abertura atual já rejeitou. (`app/static/pwa.js:60`, `:72`.)  
  - Catch interno do gap não é indispensável à fila, pois esta envolve `obterGap()` em `_noTeto`, que absorve rejeição. (`app/static/pwa.js:134`, `:166`, `:213`.)  
  - `.catch` do indicador ocorre depois da transação de gravação e sem `await`; sua retirada gera rejeição acessória, não desfaz a fila. (`app/static/pwa.js:216`, `:223`.)  
  - Instante único está correto no código, alimentando ambos os timestamps, mas falta prova de regressão temporal. (`app/static/pwa.js:212`, `:214`, `:218`.)  
  Não demonstrei bloqueante de produto **só** pela ausência desses quatro mutantes; a prova para “aborto lança durante commit” permanece ausente. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:204`; `CLAUDE.md:375`.)

- **Referências ao apagado:** a busca literal encontra somente a lápide no guard, comentário histórico de teste e tabela de órfãos; nenhum desses executa render/include. (`app/scripts/check_a_lei_batida.py:20`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:673`; `app/docs/TICKETS.md:693`.) As URLs vivas continuam apontando à view; ela renderiza v2 ou um template da triagem. (`app/ponto/urls.py:11`; `app/ponto/urls.py:12`; `app/ponto/views.py:225`; `app/ponto/views.py:356`.)

- **SW:** guarda URLs, não nomes de templates; `/ponto/bater/` permanece na lista. Para navegação, porém, o ramo `navigate` retorna antes do bloco que alimenta esse cache; é comportamento presente na cópia, não referência quebrada causada pela exclusão. (`app/static/sw.js:45`, `:50`, `:67`, `:74`.)

- **Ferramentas alteradas:** `check_frontend_urls.py` lista somente pwa.js, sw.js e dashboard antigo; apto/v2 ficam fora dessa cobertura. O guard da Lei retirou o arquivo morto, mas não incluiu v2 e conserva caminhos `app/apps/ponto/views.py`, pulando arquivos inexistentes. São lacunas desses instrumentos; não provam quebra de rota nem novo bloqueio de selfie. (`app/tools/check_frontend_urls.py:25`; `app/scripts/check_a_lei_batida.py:14`, `:20`, `:21`, `:31`.)

Não executei o Chromium: o próprio ensaio cria arquivos/perfil e os apaga, contrariando a regra somente leitura. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:344`, `:351`, `:360`, `:363`, `:372`.)

## 11/11 15:36 · sessao 4ab2dacb6e2c · - · - · origem=Ronald (tmux)

(2) Há **até um POST inicial mais uma linha de fila por clique online**; a linha pode provocar novos POSTs sem limite declarado enquanto a resposta/remoção falhar. Isso não equivale ao mesmo número de linhas `Batida`: a sincronização mantém a linha em erro e só a remove após resposta 2xx e transação de exclusão completa (`app/templates/ponto/bater_ponto_v2.html:307-331`; `app/static/pwa.js:256-298`).

- O chokepoint deduplica **mesmo colaborador e mesmo tipo, ±60 s inclusivos**, entre batidas não retratadas, comparando o `timestamp` passado à porta (`app/ponto/registro_batida.py:106-115`).
- A API deduplica **mesmo colaborador e mesmo tipo, ±5 min inclusivos**, sobre `ts_efetivo`, e responde `ja_registrada`, sem criar outra linha. Não é janela sobre horário da chegada quando o aparelho forneceu hora válida (`app/api/views.py:518-555`).
- Portanto, **reenvio sequencial, mesmo tipo, diferença de até 30 s e primeira gravação já visível** cabe nas duas janelas. A falta de identidade original, isoladamente, não prova duplicação nesse cenário (`app/static/pwa.js:230-240`; `app/api/views.py:539-555`; `app/ponto/registro_batida.py:106-115`).
- A garantia universal fica **[nao verificado]**: a API deduplica antes de eventualmente corrigir o tipo; a consulta usa o tipo recebido, enquanto a primeira gravação pode ter usado outro (`app/api/views.py:539-558`; `app/api/views.py:578-613`). Há também vários gatilhos de sincronização e nenhuma trava na entrada da função mostrada (`app/static/pwa.js:16`; `app/static/pwa.js:244-253`; `app/static/pwa.js:441-449`).
- Com `tx.abort()` lançando no teto, o código rejeita mesmo assim; ele não consegue assegurar que aquela transação não concluirá posteriormente. Isso pode deixar uma linha que será reenviada apesar do erro mostrado, mas não demonstra duas linhas no servidor, devido às deduplicações acima (`app/static/pwa.js:84-93`; `app/static/pwa.js:216-223`; `app/templates/ponto/bater_ponto_v2.html:329-331`).

(3) A lei copiada declara batida de chão sem barreira e escrita por `ponto/registro_batida.py` (`CLAUDE.md:143-144`). Os caminhos examinados consultam o juiz central de câmera/captura e seguem com foto nula; não encontrei segundo juiz de admissibilidade da selfie nesses caminhos (`app/static/pwa.js:170-204`; `app/templates/ponto/triagem/apto.html:248-264`; `app/templates/ponto/triagem/apto.html:275-289`; `app/templates/ponto/bater_ponto_v2.html:269-284`). No servidor, ausência de foto consulta `selfie_ausente` e acrescenta motivo, sem retorno de recusa (`app/api/views.py:449-465`; `app/ponto/registro_batida.py:80-91`). Existem barreiras anteriores de situação e colisão oposta; não são introduzidas por esse delta (`app/api/views.py:415-423`; `app/ponto/registro_batida.py:145-151`).

(4) O escritor lê `Date.now()` uma vez **na entrada de `registrarBatidaOffline`**, antes do gap e do IndexedDB; grava esse instante cru em `timestamp_dispositivo` (`app/static/pwa.js:211-222`). Isso não é necessariamente o instante do primeiro clique: na v2 online, a chamada acontece depois do POST sem resposta, que pode consumir 30 s (`app/templates/ponto/bater_ponto_v2.html:293-323`; `app/static/pwa.js:230-237`).

A fila envia `timestamp_dispositivo`; não envia seu `timestamp` corrigido nem o gap (`app/static/pwa.js:261-270`). O leitor é `api_bater_ponto`, que passa esse campo a `hora_do_aparelho`; sem campo, vale `agora` (`app/api/views.py:509-521`; `app/api/hora_do_aparelho.py:135-139`). A função aceita a hora do aparelho com atraso de **0 a 48 h**, usando-a como hora efetiva; acima de 3 min acrescenta trilha de offline aceito. Hora futura ou antiga demais usa `agora`, preservando a hora lida no campo do aparelho (`app/api/hora_do_aparelho.py:62-67`; `app/api/hora_do_aparelho.py:141-153`). Assim, atraso de 30 s é aceito sem descarte quando o relógio está correto (`app/api/hora_do_aparelho.py:141-146`).

## 11/11 15:35 · sessao 36452142e014 · DATA-EVENTO-CRU · verificador · origem=Code

**Veredito: bloqueantes — há leitores reais fora do censo e ocorrências de escrita/schema registradas como leitores.** (`app/ponto/management/commands/vigia_plantio_overnight.py:75`; `app/chamados/models.py:2789`; `app/chamados/models.py:3486`)

1. **O selo MORDE nas formas reconhecidas, mantendo o JSON fixo.** Chave nova, aumento em escopo existente e redução sem baixar a allowlist ficam vermelhos; desaparecimento também exige baixa. Isso vale para Python e templates. Confirmei esses cinco cenários em memória usando a comparação existente. (`app/chamados/tests/test_catraca_data_evento_cru.py:133`)

   O teste denominado `test_MORDE_leitor_novo_no_censo` prova descoberta, mas não executa a comparação que reprova o leitor novo. Não executei esse caso, pois cria e apaga arquivos temporários. (`app/chamados/tests/test_catraca_data_evento_cru.py:178`)

   A catraca compara apenas com o JSON atual: aumentar código **e** allowlist juntos pode passar; não há comparação com o baseline anterior. Substituir uma leitura por outra no mesmo escopo também preserva a contagem, limite já declarado. (`app/chamados/tests/test_catraca_data_evento_cru.py:134`; `app/chamados/tests/test_catraca_data_evento_cru.py:26`)

2. **Falsos negativos.** O varredor reconhece `getattr` literal, `values/values_list/only('data_evento')`, `F/OuterRef('data_evento')`, `filter(data_evento__...)`, `annotate(d=F(...))`, subscrito literal e `.get('data_evento')`. Um `{% with d=p.data_evento %}` também entra. (`app/chamados/tests/test_catraca_data_evento_cru.py:82`; `app/chamados/tests/test_catraca_data_evento_cru.py:174`)

   Escapam nomes calculados, argumentos expandidos por `*`/`**`, campos declarados em listas/tuplas, `source='data_evento'`, serialização genérica e template `|attr`. A detecção depende de atributo, subscrito literal, argumento posicional literal ou keyword que comece pelo campo. (`app/chamados/tests/test_catraca_data_evento_cru.py:23`; `app/chamados/tests/test_catraca_data_evento_cru.py:82`)

   **Leituras verdadeiras omitidas hoje:**

   - `pergunta_origem__data_evento__isnull=False` lê o campo pela relação, mas não casa com a regex ancorada em `data_evento`. (`app/ponto/management/commands/vigia_plantio_overnight.py:75`; `app/chamados/tests/test_catraca_data_evento_cru.py:50`)
   - A chave da família marco contém `data_evento`, usado por `values(*chave)`, `order_by(*chave)`, `g[k]` e `filter(**dict(...))`; essas ocorrências escapam. (`app/chamados/services/fio.py:1373`; `app/chamados/services/fio.py:1948`; `app/chamados/services/fio.py:1949`; `app/chamados/services/fio.py:1950`; `app/chamados/services/fio.py:1970`)
   - O lookup dinâmico de nulidade também escapa. (`app/chamados/services/fio.py:1378`; `app/chamados/services/fio.py:1946`)
   - Fora de `app/`, mensageria lê o valor serializado com `i.get('data_evento')`; essa exclusão está declarada. (`mensageria/nucleo/ferramentas.py:127`; `app/chamados/tests/test_catraca_data_evento_cru.py:25`)

   Existência atual de serializer ou template `|attr` lendo esse campo: **[nao verificado]**.

3. **Falsos positivos.** Atribuição simples e `create(data_evento=...)` são excluídos; uma leitura no lado direito continua contando. (`app/chamados/tests/test_catraca_data_evento_cru.py:48`; `app/chamados/tests/test_catraca_data_evento_cru.py:82`)

   Contudo, qualquer outra chamada com keyword `data_evento=` conta. Assim, `abrir` e `abrir_conversa` entram apenas por encaminharem o valor ao escritor, sem ler campo de pergunta. (`app/chamados/tests/test_catraca_data_evento_cru.py:96`; `app/chamados/models.py:2789`; `app/chamados/models.py:2801`)

   Os dois `Q` internos às declarações de índices também contam, apesar da exclusão de `UniqueConstraint`: a recursão visita seus filhos. A allowlist registra essas duas ocorrências em `PerguntaDisputa.Meta`. (`app/chamados/tests/test_catraca_data_evento_cru.py:99`; `app/chamados/models.py:3486`; `app/chamados/models.py:3492`; `app/chamados/tests/catraca_data_evento_cru.json:39`)

   Não há resolução de modelo: um atributo homônimo de outro objeto seria contado igualmente. Outro modelo concreto afetado hoje: **[nao verificado]**. (`app/chamados/tests/test_catraca_data_evento_cru.py:82`)

4. **Juiz excluído pelo nome exato.** A exclusão é `out.pop(JUIZ, 0)`. Renomeá-lo faz surgir chave nova e deixa a contagem do juiz isento em zero, reprovando também a prova de existência; não passa vazio. (`app/chamados/tests/test_catraca_data_evento_cru.py:44`; `app/chamados/tests/test_catraca_data_evento_cru.py:127`; `app/chamados/tests/test_catraca_data_evento_cru.py:146`)

5. **JSON e contratos.** O leitor é `test_so_desce`; a mensagem exige baixa manual. Escritor automatizado/autoria histórica: **[nao verificado]**. (`app/chamados/tests/test_catraca_data_evento_cru.py:134`; `app/chamados/tests/test_catraca_data_evento_cru.py:143`)

   Não há colisão nos enumeradores examinados: O20 seleciona `test_contract_*.py`; P13 exclui `tests/` e exige `.py`; o selo de relógio seleciona `test*.py`, excluindo o JSON. Ausência universal de colisões na suite: **[nao verificado]**. (`app/core/tests/test_o20_contrato_julga_codigo.py:164`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:147`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:150`; `app/core/tests/test_selo_teste_sem_relogio.py:53`)

   O JSON soma **217 ocorrências em 120 chaves: 212 Python + 5 templates**, coincidindo com o censo implementado. Publicação desse total no BACKLOG/ESTADO: **[nao verificado]**. (`app/chamados/tests/catraca_data_evento_cru.json:2`; `app/chamados/tests/catraca_data_evento_cru.json:119`)

6. **Classes anteriores de reprovação.** Há mordida efetiva na comparação, porém cobertura incompleta nas formas acima. Os arquivos novos implementam contagem e comparação, sem introduzir juiz do dia, leitura de relógio ou derivação paralela do dia. Execução da suite Django/pre-push neste hash: **[nao verificado]**. (`app/chamados/tests/test_catraca_data_evento_cru.py:68`; `app/chamados/tests/test_catraca_data_evento_cru.py:112`; `app/chamados/tests/test_catraca_data_evento_cru.py:133`)

**Bloqueantes para aprovação:** incluir os leitores reais omitidos e retirar os repasses de escrita/schema do censo, recalibrando a allowlist. (`app/ponto/management/commands/vigia_plantio_overnight.py:75`; `app/chamados/services/fio.py:1948`; `app/chamados/models.py:2789`; `app/chamados/models.py:3486`)

## 11/11 15:34 · sessao 97538cdfafe8 · - · - · origem=Ronald (tmux)

Inventário estático de `select_for_update`: **23 sites fora de testes/migrations; 18 com alcance por código até Pauta DP**, dos quais **16 têm borda produtiva identificada e 2 pertencem às portas editar/trocar usadas só pelos testes encontrados**. Contagem por AST de `Call(Attribute(..., 'select_for_update'))`; inclui as linhas enumeradas abaixo. A execução do censo não deixou arquivo de saída: **[nao verificado]**. [nao verificado]

| Recurso / função | Sites de aquisição | Ordem e alcance até DP |
|---|---|---|
| Colaborador, Disputa, Chamado, Pergunta — `PerguntaDisputa.trava_na_ordem` | `app/chamados/models.py:2666`, `:2675`, `:2688`, `:2692` | **C → D → Ch → P**, cada conjunto por PK; `_gravar_estado` pede universo antes de salvar, e o save acorda cartório/fábrica. `app/chamados/models.py:290`; `app/chamados/models.py:296`; `app/chamados/signals.py:217`; `app/ponto/services/cartorio.py:1020`. |
| FechamentoMensal — command pago/devido | `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196` | Atomic `:189` → C `:193` → FM `:196` → Pauta cabeça `:207` → DP → filhas `:211` → DP. |
| Justificativa — aprovação/rejeição/materialização | `app/ponto/services/justificativa.py:142`, `:277`, `:344`; `app/ponto/views.py:1794` | **C/D/Ch antes da Justificativa**, pelos pedidos `:141`, `:276`, `:343` e view `:1793`; depois save de J → signal cartório → fábrica → DP. `app/ponto/services/justificativa.py:208`; `app/ponto/services/justificativa.py:287`; `app/ponto/signals.py:165`; `app/ponto/services/cartorio.py:1020`. |
| AgendamentoFerias e PeriodoAquisitivo — `_travar_agendamento` | `app/ferias/services.py:364`, `:367` | Ag → PA → Ausencia por UPDATE/DELETE/INSERT → cartório ou pauta de férias recusadas → DP. `_trocar_janela`: atomic `:433`, locks `:434`, exclusão `:445`, save `:450`; sync em `app/ferias/models.py:449` e `:451`; pauta direta em `app/ponto/services/ausencia.py:1216` e `:1245`. |
| AgendamentoFerias — alinhamento por data | `app/ferias/services.py:600` | Atomic `:599` → Ag → save de status `:604` → sync Ausencia `app/ferias/models.py:451` → cartório/DP, ou pauta de recusa `app/ponto/services/ausencia.py:1245`. |
| SolicitacaoFerias — view avaliar | `app/ferias/views.py:740` | Atomic `:739` → Solicitação → aprovação `:753` → criação/save Agendamento `app/ferias/services.py:503` → sync Ausencia `app/ferias/models.py:451` → DP; depois UPDATE PA `app/ferias/services.py:506`. |
| DecisaoHE — decisão/período | `app/ponto/portas/he.py:174`, `:555` | Prévia atomic `app/ponto/services/autorizacao_he_periodo.py:321` ou confirmação `:343` → `_ato` → `autorizar_em_lote` `:277` → HE `app/ponto/portas/he.py:555` → decisão HE `:562`/`:174` → pauta DP `app/ponto/services/autorizacao_he_periodo.py:300`/`:244`. |
| Ausencia — rederivação H06 | `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:138` (chamada continua em `:139`) | Atomic `:137` → Ausencia → editar `:169` → save `app/ponto/services/ausencia.py:509` → signal `app/ponto/signals.py:147` → cartório/fábrica/DP. |
| CelulaDia — editar dia | `app/ponto/portas/celula.py:593` | Atomic `:592` → Celula → órfãs `:626` → `app/ponto/management/commands/gerar_celulas.py:255` → contexto `app/chamados/services/contexto.py:103` → C/Ch `app/chamados/models.py:486`. Julgamento/DP vem após saída desse atomic, em `app/ponto/portas/celula.py:627` e `:434`; fica na mesma transação quando herda o atomic de trocar `:652`. Chamador encontrado: `app/ponto/portas/celula.py:675`; bordas externas encontradas só testes, por exemplo `app/ponto/tests/test_o10b_editar_dia_troca.py:116`. |
| Ausencia — trocar plantão | `app/ponto/portas/celula.py:657` | Atomic `:652` → Ausencias por PK `:657` → células por colaborador/data `:673` → editar `:675` → C/Ch por órfãs → julgamento → DP. Uso identificado só em teste, por exemplo `app/ponto/tests/test_o10b_editar_dia_troca.py:116`; chamador produtivo **[nao verificado]**. |

**Cinco sites excluídos do alcance DP no mesmo atomic lido:**

| Sites | Razão |
|---|---|
| `app/ponto/portas/he.py:318`, `:445` | Recusar ponta/revogar têm os commands próprios como chamadores encontrados; não alcançam a pauta do ato de período. Chamadas: `app/ponto/management/commands/recusar_ponta_pequena.py:170`; `app/ponto/management/commands/lavrar_he_pendente.py:59`. |
| `app/core/services/aplicacao_convencao.py:115` | O atomic cadastra/altera AplicacaoConvencao e grava trilha; retorna em `:124`/`:134`. Não foi encontrado receiver que leve esse model ao cartório; alcance indireto **[nao verificado]**. |
| `app/holerite/management/commands/processar_lote_holerite.py:54`, `:124` | Atomics cuidam de estado do lote; chamada ao motor fica fora deles, em `:89`. Alcance de DP dentro desses dois atomics **[nao verificado]**. |

| Advisory | Aquisição / duração |
|---|---|
| Âncora E4B | `app/chamados/services/fecho_runtime.py:89`: `pg_advisory_xact_lock`, dentro atomic `:119`; todas as âncoras ordenadas em `:120`, antes de escrever `:127`. |
| Vigia de hora | `app/ponto/services/vigia_de_hora.py:99`: `pg_try_advisory_lock`, sessão; falha retorna em `:100`/`:101`; julgamento em `:131`, unlock em `:136`. Não é aquisição bloqueante; não há atomic amplo em `rodar`. |
| Consumidor de eventos | `app/chamados/reconciliador.py:1235`: `pg_try_advisory_lock`, sessão; libera em `:1242`. O consumidor usa atos separados por chamado, sem atomic de lote, conforme implementação/chamadas `:1364`, `:1365`, `:1368`; delimitação descrita em `:1325`/`:1326`. |

**Escritas que também precisam entrar na sequência, embora não sejam `select_for_update`:**

- `Pauta INSERT → trilha → Contador.get_or_create → Contador UPDATE`: `app/pautas/services.py:180`, `:186`, `:188`, `:352`, `:354`. `lavrar` **não tem decorador atomic** em `:344`; chamado avulso em autocommit não conserva DP entre alvos; comportamento efetivo do banco **[nao verificado]**.
- Pauta existente UPDATE → trilha → DP: ler `app/pautas/services.py:215`/`:218`; fazer `:247`/`:251`; fazer pelo sistema `:272`/`:276`; cancelar `:294`/`:297`.
- Cartório escreve Celula antes de chamar a fábrica: `app/ponto/services/cartorio.py:1845` → `app/ponto/portas/celula.py:1072` → atomic/save `:904`/`:905`; depois fábrica `app/ponto/services/cartorio.py:1020`. Na lavra do cartório, o argumento é `save=False` em `app/ponto/services/cartorio.py:1825`, portanto citar somente `celula.py:1040` não prova o UPDATE desse caminho.
- Núcleo de Batida: signal `app/ponto/signals.py:36` → `app/ponto/nucleo.py:57` → DELETE de TurnoMaterializado `:58`/`:81` → bulk INSERT `:59`; o cartório posterior do mesmo save vem em `app/ponto/signals.py:135`.
- Invalidar FM é **síncrono**: `app/ponto/services/fechamento.py:1656`. Exemplo DP pode precedê-lo: aprovação `app/ponto/services/justificativa.py:208` → signal/cartório/fábrica → DP → `invalidar_fechamento` `:215`. O recálculo, separadamente, é `on_commit` em `app/ponto/services/fechamento.py:1824`.
- Passivo: atomic do par `app/chamados/services/passivo_validado.py:371` → plantio `:428` → regeneração exportada `:438` → outer da porta `app/ponto/portas/regen_exportada.py:169` → regeneração `:174` → FM `:179` → DiaPago `:188` → Pauta passivo `app/chamados/services/passivo_validado.py:443`. A regeneração pode acionar cartório/fábrica antes do FM por `app/ponto/portas/celula.py:354`/`:434`.
- Matrícula sem número pede C do maior matriculado em `app/colaboradores/models.py:436`; seu atomic `:429` termina antes do `super().save` `:439`, salvo atomic herdado. Alcance DP naquele atomic isolado **[nao verificado]**.

**Pares e limites:**

| Par | Avaliação estática |
|---|---|
| DP → âncora / âncora → DP | Inversão demonstrada no runtime pelo ato externo: transição antes do agrupador `app/chamados/services/fecho_runtime.py:151`/`:154`; fábrica DP `app/chamados/services/fabrica_da_celula.py:369` ou `:279`; agrupador âncora `app/chamados/services/fecho_runtime.py:121` → DP `:127`/`app/pautas/services.py:354`. Recursos compartilháveis entre colaboradores diferentes pela âncora empresa/competência `app/chamados/services/fecho_runtime.py:116` e contador por departamento `app/pautas/services.py:352`. |
| C → Celula / Celula → C | Oposto efetivo de transição: C `app/chamados/models.py:2666` → cartório `app/chamados/signals.py:217` → Celula `app/ponto/portas/celula.py:905`. Inverso em trocar: outer `app/ponto/portas/celula.py:652` → Celula `:593` → órfãs `:626` → C `app/chamados/models.py:486`. Código permite o par para mesmos C/Celula; chamador de trocar encontrado só testes `app/ponto/tests/test_o10b_editar_dia_troca.py:116`. Runtime concorrente **[nao verificado]**. |
| DP → FM / FM → DP | Há as duas ordens **de classes**: aprovação J `app/ponto/services/justificativa.py:208`/`:215` e pago/devido `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`/`:207`. **Não certificar deadlock desse par:** aprovação já toma C em `app/ponto/services/justificativa.py:141`/`:129`, e pago toma o mesmo C antes do FM em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`. Para o mesmo FM, esse C serializa os dois caminhos; outro ato sem esse serializador **[nao verificado]**. |
| Regeneração Celula → C contra transição C → Celula | Regeneração tem UPDATE Celula `app/ponto/portas/celula.py:276` antes de órfãs/C `:289`; entretanto cartório vem depois do atomic local `:354`. A porta exportada conserva ambos no outer `app/ponto/portas/regen_exportada.py:169`/`:174`, mas seu único chamador produtivo encontrado é passivo `app/chamados/services/passivo_validado.py:438`, cujo plantio já carimba pela porta em `app/chamados/services/materializacao.py:1277` antes da regeneração. Não certificar aquisição nova de C nem ciclo runtime sem resolver essa pré-trava. |
| Justificativa → C / C → Justificativa | Não demonstrado no código lido: as quatro bordas pedem C/D/Ch antes de J (`app/ponto/services/justificativa.py:141`, `:276`, `:343`; `app/ponto/views.py:1793`). |

Correção ao meu informe intermediário: `rejeitar_ausencias_de_ferias` usa UPDATE sem signal (`app/ponto/services/ausencia.py:1281`), portanto o alcance cartório/DP do cancelamento-admin vem do save posterior do Agendamento (`app/ferias/services.py:417` → `app/ferias/models.py:451`), que remove Ausencias e dispara `post_delete` (`app/ponto/services/ausencia.py:1190`; `app/ponto/signals.py:152`).

## 11/11 15:34 · sessao b65421d650d3 · - · - · origem=Ronald (tmux)

Achados para consolidação:

- **Falso negativo atual, sem reflexão:** `Batida.objects.filter(pergunta_origem__data_evento__isnull=False)` consulta o campo de `PerguntaDisputa`, mas o prefixo relacional impede casar com `^data_evento...$`. O vínculo de `pergunta_origem` é `PerguntaDisputa`. — `app/ponto/management/commands/vigia_plantio_overnight.py:73-75`; `app/ponto/models.py:41`; `app/chamados/tests/test_catraca_data_evento_cru.py:50,97`.
- **Leituras dinâmicas atuais omitidas:** a chave da família MARCO inclui `'data_evento'`; `grupos_de_colisao` a usa em `values(*chave)`, `order_by(*chave)`, `g[k]` e `filter(**dict(zip(chave,ch)))`. Nenhuma dessas ocorrências casa com os predicados de constantes diretas do varredor. O queryset vem de `PerguntaDisputa`. — `app/chamados/services/fio.py:1373,1430-1433,1945,1948-1950,1970`; varredor `app/chamados/tests/test_catraca_data_evento_cru.py:64-65,84-98`.
- **Filtros dinâmicos atuais omitidos:** `_NULO_FORA_DO_INDICE['marco']` é `'data_evento__isnull'`; seus usos em `exclude(**{...})` e `filter(**{...})` ficam fora. — `app/chamados/services/fio.py:1378,1946,2000`; varredor `app/chamados/tests/test_catraca_data_evento_cru.py:96-98`.
- `getattr(p,'data_evento')`, `values/values_list/only('data_evento')`, `F/OuterRef('data_evento')`, `filter(data_evento__...)`, `annotate(d=F('data_evento'))`, `d['data_evento']` e até `d.get('data_evento')` **são vistos**: constantes posicionais de quaisquer chamadas não excluídas entram. — `app/chamados/tests/test_catraca_data_evento_cru.py:82-98`.
- `fields=['data_evento']`, `source='data_evento'`, constantes em tuplas/listas passadas por `*`, argumentos por `**`, `getattr(p,nome)` e `|attr` **não são vistos**; presença atual de serializers ou templates `|attr` de `PerguntaDisputa`: **[nao verificado]**. Os limites de reflexão/serialização/`|attr` já estão declarados. — `app/chamados/tests/test_catraca_data_evento_cru.py:23-25,64-65,89-98,105-109`.
- `{% with d=q.data_evento %}` **é visto**, inclusive no teste sintético. — `app/chamados/tests/test_catraca_data_evento_cru.py:174-176`.
- Fora de `app/`, existe consumo do valor serializado: `mensageria/nucleo/ferramentas.py:127` lê `i.get('data_evento')`; a API fornece o campo de `p` em `app/api/views_mensageria.py:568`. Esse serviço está explicitamente fora do censo. — `app/chamados/tests/test_catraca_data_evento_cru.py:25,41,112`; transporte `mensageria/nucleo/core_client.py:42-55`.
- A única declaração de campo modelo encontrada é `PerguntaDisputa.data_evento`; **não há evidência para afirmar segundo modelo existente**. O comentário diz “UM modelo”, sem dizer “outro”. — `app/chamados/models.py:2406,2423`; `app/chamados/tests/test_catraca_data_evento_cru.py:26`. Outro modelo hoje: **[nao verificado]**.
- Os `getattr` dinâmicos de histórico e reversão não demonstram leitores omitidos de `data_evento`: suas listas não incluem o campo. — `app/chamados/services/historico.py:98-105`; `app/chamados/services/passivo_validado.py:64-66,83`.

## 11/11 15:34 · sessao 5bcb028b7dc6 · - · - · origem=Ronald (tmux)

- O JSON é lido apenas por `test_so_desce`, via `open(..., encoding='utf-8')` e `json.load`; não há escritor automatizado demonstrado. A baixa depende da alteração manual exigida pela mensagem do teste. `app/chamados/tests/test_catraca_data_evento_cru.py:42`, `:134`, `:135`, `:143`. Autoria histórica do arquivo: [nao verificado].

- Soma conferida em memória: **217 ocorrências / 120 chaves**, sendo **212 Python + 5 templates**, diferente de “190+4” do contrato fornecido. Fonte: `app/chamados/tests/catraca_data_evento_cru.json:2` até `:121`; os três templates somam 1+3+1 em `:119`, `:120`, `:121`. Não encontrei publicação do contador em BACKLOG/ESTADO: [nao verificado].

- **Sem colisão demonstrada nos enumeradores examinados**: O20 considera somente `test_contract_*.py`, portanto nem o novo `test_catraca_...py` nem o JSON entram nesse censo (`app/core/tests/test_o20_contrato_julga_codigo.py:160`, `:164`); seu outro varredor exige `.py` (`:88`, `:92`). P13 exclui `tests/` e exige `.py` (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:146`, `:147`, `:150`). O selo de relógio considera somente arquivos `test*.py`, podendo examinar o novo teste, mas não o JSON (`app/core/tests/test_selo_teste_sem_relogio.py:50`, `:53`). O vocabulário inclui testes, mas suas extensões são `.py/.html/.js/.md`, excluindo JSON (`app/pautas/tests/test_contract_vocabulario.py:20`, `:29`, `:32`). Ausência universal de colisões na suite executada: [nao verificado].

- A integração declarada está no código: `chamados` integra `LABELS` (`bin/regua.sh:23`), executadas pela régua (`:165`, `:168`) e pelo arsenal do pre-push, que deriva a mesma lista (`bin/pre-push.sh:184`, `:186`, `:187`). Hook instalado e execução real nesta cópia: [nao verificado].

- **Excluir o teste não fica protegido por esses selos**: labels exige apenas que o app com *algum* teste esteja na lista (`bin/tests/test_labels_fonte_unica.sh:24`, `:30`, `:32`); testes fora do git compara testes presentes na árvore com os rastreados, sem exigir a existência desta catraca específica (`bin/testes_fora_do_git.sh:46`, `:48`, `:49`; `app/core/selos_no_git.py:26`). Excluir só o JSON, mantendo a catraca, causa falha de abertura (`app/chamados/tests/test_catraca_data_evento_cru.py:134`).

- O diff fornecido acrescenta instrumento e censo, sem juiz de domínio ou leitura de relógio; a catraca mede sintaxe (`app/chamados/tests/test_catraca_data_evento_cru.py:67`, `:112`, `:133`). O caso de mordida cria arquivos temporários (`:181`, `:184`, `:186`, `:188`); não o executei por respeito à regra de leitura. Regra paralela do dia introduzida por esses dois arquivos: não demonstrada (`:43`, `:127`, `:149`).

## 11/11 15:33 · sessao 363ba840fe9f · - · - · origem=Ronald (tmux)

- **Morde nas formas reconhecidas:** chaves novas, aumentos e quedas falham separadamente; queda ou desaparecimento exige ajustar JSON (`app/chamados/tests/test_catraca_data_evento_cru.py:137–144`). O censo atual coincide exatamente com JSON: 120 chaves, 217 ocorrências, sendo 212 Python + 5 templates; juiz excluído tem 1 ocorrência (`app/chamados/tests/test_catraca_data_evento_cru.py:112–128`; `app/chamados/tests/catraca_data_evento_cru.json:2–121`).
- **Juiz protegido por nome exato:** exclusão via `out.pop(JUIZ, 0)`; renomear gera leitor novo e faz a prova de existência falhar; não passa vazio (`app/chamados/tests/test_catraca_data_evento_cru.py:44`, `:127`, `:137`, `:146–149`).
- **FP concreto:** qualquer keyword `data_evento=` em chamada não listada como escrita conta, mesmo quando apenas entrega o dia a uma porta escritora (`app/chamados/tests/test_catraca_data_evento_cru.py:48–49`, `:92–98`). Casos atuais: `_sob_contrato` (`app/chamados/models.py:2789`, `:2801`); `reapontar_resolvedora` (`app/chamados/reconciliador.py:954`, `:1642`, `:1655`; `app/chamados/services/declaracao_texto.py:370`); `abrir` (`app/chamados/services/declaracao_texto.py:377`, `:667`; `app/chamados/services/disputa_emissao.py:451`, `:844`).
- **FP concreto em schema:** embora `UniqueConstraint` seja excluído, a recursão visita os `Q(data_evento__gte=...)` internos, registrando duas ocorrências como leitores na chave `PerguntaDisputa.Meta` (`app/chamados/models.py:3486`, `:3492`; `app/chamados/tests/catraca_data_evento_cru.json:39`; `app/chamados/tests/test_catraca_data_evento_cru.py:99`).
- **Escrita simples não conta:** exige contexto `Load`; `create(data_evento=...)` está na exclusão; leitura real no RHS continua contando (`app/chamados/tests/test_catraca_data_evento_cru.py:48`, `:82–99`). O escritor existente é excluído corretamente (`app/chamados/services/fio.py:800`).
- **FN adicional:** `p.data_evento += 1` também lê o valor anterior, mas AST usa contexto `Store`, e o varredor não o registra (`app/chamados/tests/test_catraca_data_evento_cru.py:82`).
- **Sem resolução de modelo:** atributo/keyword/subscrito são reconhecidos por grafia; campo homônimo de outro objeto contaria igualmente (`app/chamados/tests/test_catraca_data_evento_cru.py:82–98`).
- **Prova chamada MORDE é parcial:** teste temporário prova descoberta de uma chave Python e um template; não chama a comparação que deve reprovar, nem injeta aumento no mesmo escopo (`app/chamados/tests/test_catraca_data_evento_cru.py:178–191`). A comparação efetiva, entretanto, implementa os três vermelhos (`:137–144`).
- **Observação de integridade:** comparação só usa JSON presente; não compara ao JSON de commit anterior. Aumentar JSON junto com código pode passar (`app/chamados/tests/test_catraca_data_evento_cru.py:134–139`).

## 11/11 15:33 · sessao d323d1ac48ed · - · - · origem=Ronald (tmux)

Censo por AST de todos os 3.323 `.py` sob `app/`, sem falha de parse, resolvendo `from pautas.services import escrever as ...`, `from pautas import services as _ps/sv`, import relativo da view e chamadas internas da porta: **33 callsites produtivos `escrever`**, **22 callsites em testes**, **6 callsites produtivos `lavrar`**, **1 em teste**. Resultado da execução em memória, sem arquivo de saída: **[nao verificado]**. A unidade é callsite AST, não função nem frequência runtime. O alias produtivo `_ps` está em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:157`; o relativo `sv`, em `app/pautas/views.py:12`.

Os 33 sites produtivos de `escrever` se dividem em **18 DP literais + 4 por constante DP + 3 destinatários dinâmicos que podem ser DP + 8 excluídos por destino diferente**. A contagem pode ser reconstruída pela tabela abaixo, onde há uma referência por callsite.

Legenda `P→D`: `INSERT Pauta` → trilha → `ContadorPauta.get_or_create`/`UPDATE`, dentro do atomic da porta (`app/pautas/services.py:152`, `app/pautas/services.py:180`, `app/pautas/services.py:186`, `app/pautas/services.py:188`, `app/pautas/services.py:352`, `app/pautas/services.py:354`). O `UPDATE` é a aquisição de linha do contador; o código não usa `select_for_update` nele (`app/pautas/services.py:354`).

| Emissor/site(s) DP | Chamador produtivo / ordem demonstrada |
|---|---|
| `fecho_runtime._escrever_pautas`, 1 literal (`app/chamados/services/fecho_runtime.py:127`) | Wrapper atomic: transição antes do agrupador (`app/chamados/services/fecho_runtime.py:147`, `app/chamados/services/fecho_runtime.py:151`, `app/chamados/services/fecho_runtime.py:154`); agrupador toma todas as advisories ordenadas, depois `P→D` (`app/chamados/services/fecho_runtime.py:119`, `app/chamados/services/fecho_runtime.py:120`, `app/chamados/services/fecho_runtime.py:127`). |
| `pauta_dp_do_limbo_exportado`, 1 literal (`app/chamados/services/fabrica_da_celula.py:279`) | Fábrica a chama no julgamento (`app/chamados/services/fabrica_da_celula.py:215`); própria função lê dedup e chama `P→D`, sem atomic externo próprio (`app/chamados/services/fabrica_da_celula.py:277`, `app/chamados/services/fabrica_da_celula.py:279`). Locks herdados do cartório/transição permanecem no atomic do chamador (`app/chamados/services/fecho_runtime.py:147`). |
| `pauta_cadastro_sem_turno`, 1 literal (`app/chamados/services/fabrica_da_celula.py:369`) | Fábrica chama em `:204`; atomic próprio `:364` → `PerguntaDisputa.trava_na_ordem(colab)` `:365` → dedup `:367` → `P→D` `:369` (`app/chamados/services/fabrica_da_celula.py:204`, `app/chamados/services/fabrica_da_celula.py:364`, `app/chamados/services/fabrica_da_celula.py:365`, `app/chamados/services/fabrica_da_celula.py:367`, `app/chamados/services/fabrica_da_celula.py:369`). |
| `pre_fechamento._pauta`, 1 literal (`app/folha/management/commands/pre_fechamento.py:110`) | Cron declarado (`app/config/crons.py:406`). Fecha Pautas antigas por portas individuais antes de escrever a nova; não há atomic envolvendo o conjunto nesse corpo (`app/folha/management/commands/pre_fechamento.py:85`, `app/folha/management/commands/pre_fechamento.py:96`, `app/folha/management/commands/pre_fechamento.py:101`, `app/folha/management/commands/pre_fechamento.py:110`). Cada fechamento `UPDATE Pauta→D`; nova `P→D` (`app/pautas/services.py:272`, `app/pautas/services.py:276`, `app/pautas/services.py:180`, `app/pautas/services.py:188`). |
| `pautas_de_ferias._pauta`, 1 literal (`app/ferias/management/commands/pautas_de_ferias.py:167`) | Cron declarado (`app/config/crons.py:711`); leitura dedup → `P→D`, sem outra trava própria no corpo (`app/ferias/management/commands/pautas_de_ferias.py:145`, `app/ferias/management/commands/pautas_de_ferias.py:154`, `app/ferias/management/commands/pautas_de_ferias.py:167`). |
| `_pauta_de_vencido`, 1 literal (`app/ponto/services/ausencia.py:1055`) | Command chama escalonador (`app/ponto/management/commands/escalonar_documentos_ausencia.py:25`, `app/ponto/management/commands/escalonar_documentos_ausencia.py:26`); ele muda Ausência e chama Pauta (`app/ponto/services/ausencia.py:1002`, `app/ponto/services/ausencia.py:1005`); emissor lê dedup e faz `P→D` (`app/ponto/services/ausencia.py:1039`, `app/ponto/services/ausencia.py:1055`). Atomic herdado de execução externa além dessas portas: [nao verificado]. |
| `_pauta_de_ferias_recusada`, 1 literal (`app/ponto/services/ausencia.py:1245`) | `AgendamentoFerias.save` salva agendamento, sincroniza espelho (`app/ferias/models.py:449`, `app/ferias/models.py:451`, `app/ferias/models.py:392`); espelho tenta criar Ausências, coleta recusas e escreve Pauta (`app/ponto/services/ausencia.py:1202`, `app/ponto/services/ausencia.py:1209`, `app/ponto/services/ausencia.py:1216`). Nas portas de alteração: `Agendamento FOR UPDATE→PA FOR UPDATE→Ausências→P→D` (`app/ferias/services.py:364`, `app/ferias/services.py:367`, `app/ferias/services.py:445`, `app/ferias/services.py:450`). No agendamento novo/histórico, `P→D→UPDATE PA` (`app/ferias/services.py:298`, `app/ferias/services.py:302`, `app/ferias/services.py:321`, `app/ferias/services.py:326`). |
| `pautas_dos_discordantes.sincronizar`, 1 constante DP (`app/ponto/services/pautas_dos_discordantes.py:38`, `app/ponto/services/pautas_dos_discordantes.py:177`) | Command importa/chama serviço (`app/ponto/management/commands/pautas_discordantes.py:114`). Abre primeiro, fecha depois, sem atomic envolvendo ambos no serviço (`app/ponto/services/pautas_dos_discordantes.py:173`, `app/ponto/services/pautas_dos_discordantes.py:175`, `app/ponto/services/pautas_dos_discordantes.py:182`). Assim, cada abertura é `P→D`; cada fechamento `UPDATE Pauta→D` (`app/pautas/services.py:180`, `app/pautas/services.py:188`, `app/pautas/services.py:272`, `app/pautas/services.py:276`). |
| HE `_linha_para_o_dp`, 1 literal (`app/ponto/services/autorizacao_he_periodo.py:244`) | Views importam serviço (`app/ponto/views.py:696`, `app/ponto/views.py:734`); prévia/confirmar envolvem atomic (`app/ponto/services/autorizacao_he_periodo.py:321`, `app/ponto/services/autorizacao_he_periodo.py:343`). `_ato` autoriza lote antes da Pauta (`app/ponto/services/autorizacao_he_periodo.py:277`, `app/ponto/services/autorizacao_he_periodo.py:299`): `DecisaoHE FOR UPDATE` em cada dia, ordem de entrada (`app/ponto/portas/he.py:546`, `app/ponto/portas/he.py:548`, `app/ponto/portas/he.py:555`), retrava em `decidir_he` (`app/ponto/portas/he.py:174`), depois `P→D` (`app/ponto/services/autorizacao_he_periodo.py:244`). Recálculo agendado em on_commit, não demonstra FM síncrono depois de DP (`app/ponto/portas/he.py:191`, `app/ponto/portas/he.py:202`). |
| Passivo `_pauta_da_competencia`, 2 constantes DP; `pauta_de_quem_saiu`, 1 constante DP (`app/chamados/services/passivo_validado.py:49`, `app/chamados/services/passivo_validado.py:229`, `app/chamados/services/passivo_validado.py:233`, `app/chamados/services/passivo_validado.py:266`) | Command chama reparar (`app/chamados/management/commands/materializar_passivo_validado.py:68`). Atomic por par `:371` → plantio `:427/:428` → regeneração exportada `:438` → Pauta cabeça/filhas ou sem movimento `:443/:450` (`app/chamados/services/passivo_validado.py:371`, `app/chamados/services/passivo_validado.py:427`, `app/chamados/services/passivo_validado.py:428`, `app/chamados/services/passivo_validado.py:438`, `app/chamados/services/passivo_validado.py:443`, `app/chamados/services/passivo_validado.py:450`). Regeneração chama recálculo FM e retrato DPago antes do retorno (`app/ponto/portas/regen_exportada.py:169`, `app/ponto/portas/regen_exportada.py:174`, `app/ponto/portas/regen_exportada.py:179`, `app/ponto/portas/regen_exportada.py:188`); sequência integral dos locks internos do plantio/regeneração: [nao verificado] nesta subpesquisa. |
| `resposta_tardia.receber`, 1 literal (`app/chamados/services/resposta_tardia.py:63`) | View importa serviço (`app/chamados/views.py:1050`). Ramo tardio anexa nota, registra log, então `P→D`; não declara atomic próprio (`app/chamados/services/resposta_tardia.py:31`, `app/chamados/services/resposta_tardia.py:53`, `app/chamados/services/resposta_tardia.py:55`, `app/chamados/services/resposta_tardia.py:63`). |
| `competencia_trancada._pauta_do_dp`, 1 literal (`app/chamados/services/competencia_trancada.py:231`) | Disparo síncrono chama `encerrar` (`app/chamados/services/competencia_trancada.py:281`, `app/chamados/services/competencia_trancada.py:282`); `encerrar` usa atomics separados por chamado e disputa (`app/chamados/services/competencia_trancada.py:182`, `app/chamados/services/competencia_trancada.py:197`), depois escreve Pauta fora desses blocos (`app/chamados/services/competencia_trancada.py:207`, `app/chamados/services/competencia_trancada.py:208`). Herança de atomic do disparador de tranca: [nao verificado]. |
| `reabrir_orfas_enterradas._pautas`, 1 literal (`app/chamados/management/commands/reabrir_orfas_enterradas.py:208`) | Command de runtime; atomics das reaberturas encerram em cada iteração (`app/chamados/management/commands/reabrir_orfas_enterradas.py:146`); chamada `_pautas` depois do laço (`app/chamados/management/commands/reabrir_orfas_enterradas.py:178`): dedup → `P→D` (`app/chamados/management/commands/reabrir_orfas_enterradas.py:201`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:208`). |
| `pauta_folga_nao_definida_exportada`, 2 literais (`app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:134`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:139`) | Command envolve cabeça/filhas em atomic (`app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:133`): `P cabeça→D→P filhas→D`, sem advisory E4B nesses sites (`app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:134`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:139`). |
| `pauta_dia_em_aberto`, 3 literais (`app/pautas/management/commands/pauta_dia_em_aberto.py:127`, `app/pautas/management/commands/pauta_dia_em_aberto.py:139`, `app/pautas/management/commands/pauta_dia_em_aberto.py:142`) | Command chama a porta por filha/cabeça, sem atomic de conjunto no corpo (`app/pautas/management/commands/pauta_dia_em_aberto.py:76`, `app/pautas/management/commands/pauta_dia_em_aberto.py:126`, `app/pautas/management/commands/pauta_dia_em_aberto.py:139`, `app/pautas/management/commands/pauta_dia_em_aberto.py:141`): cada chamada `P→D`. |
| `pauta_pago_x_devido_exportada`, 2 literais, alias `_ps` (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:157`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:211`) | Atomic (`:189`) → `PerguntaDisputa.trava_na_ordem(colab)` (`:193`) → FM `FOR UPDATE` (`:196`) → `P cabeça→D→P filhas→D` (`:207/:211`) (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:189`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:211`). |
| View genérica, 1 dinâmico (`app/pautas/views.py:76`, `app/pautas/views.py:78`) | Endpoint POST → `P→D` quando POST informa DP; não declara atomic próprio (`app/pautas/views.py:70`, `app/pautas/views.py:72`, `app/pautas/views.py:76`). |
| Responder, 1 dinâmico (`app/pautas/services.py:194`, `app/pautas/services.py:196`) | View chama `responder` (`app/pautas/views.py:112`); DP ocorre se remetente da Pauta original for DP e `autor_user_id` ausente (`app/pautas/services.py:196`, `app/pautas/services.py:197`); `P resposta→D`, referência ao pai (`app/pautas/services.py:200`, `app/pautas/services.py:180`, `app/pautas/services.py:188`). Estado concreto com autoria ausente: [nao verificado]. |
| Semanal, 1 dinâmico (`app/pautas/semanal.py:276`) | Chamador de ausência fornece DP (`app/ponto/services/vigia_ausencia.py:239`); command importa revisão (`app/ponto/management/commands/vigiar_familia_ausencia.py:23`). Sem atomic de conjunto (`app/pautas/semanal.py:202`); ramo zerado fecha Pautas por portas individuais (`app/pautas/semanal.py:226`, `app/pautas/semanal.py:229`), ramo aceso escreve `P→D` (`app/pautas/semanal.py:235`, `app/pautas/semanal.py:276`). |

Os **8 excluídos**:

| Callsite | Destino lido |
|---|---|
| `app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:107` | `supervisao`, constante `app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:24` |
| `app/ponto/services/pautas_dos_deslogados.py:77` | `supervisao`, constante `app/ponto/services/pautas_dos_deslogados.py:27` |
| `app/ponto/services/cobertura.py:333` | `supervisao`, literal `app/ponto/services/cobertura.py:333` |
| `app/ponto/services/afastado_avisa.py:74` | `supervisao`, literal `app/ponto/services/afastado_avisa.py:74` |
| `app/ponto/services/pautas_do_esmeril.py:76` | `supervisao`, literal `app/ponto/services/pautas_do_esmeril.py:76` |
| `app/core/services/crons_quebrados.py:71` | `ti`, literal `app/core/services/crons_quebrados.py:71` |
| `app/core/management/commands/alarme_esteira.py:54` | `ti`, literal `app/core/management/commands/alarme_esteira.py:54` |
| `app/core/management/commands/alarme_arvore.py:50` | `ti`, literal `app/core/management/commands/alarme_arvore.py:50` |

`lavrar` tem **5 sites internos**: após escrever (`app/pautas/services.py:188`), marcar lida (`app/pautas/services.py:218`), marcar feita (`app/pautas/services.py:251`), feita pelo sistema (`app/pautas/services.py:276`), cancelar (`app/pautas/services.py:297`). Nos quatro últimos, a ordem é `UPDATE Pauta→trilha→D` (`app/pautas/services.py:215`, `app/pautas/services.py:247`, `app/pautas/services.py:272`, `app/pautas/services.py:294`). O **6º é o command avulso**, que não envolve todas as lavras num atomic próprio: `app/pautas/management/commands/lavrar_pautas.py:34`, `app/pautas/management/commands/lavrar_pautas.py:36`, `app/pautas/management/commands/lavrar_pautas.py:51`; `lavrar` também não tem decorador atomic (`app/pautas/services.py:344`). Logo não carregar o contador DP de uma iteração até a seguinte sem demonstrar um atomic externo: [nao verificado].

Par adicional estrutural encontrado, além de DP↔âncora E4B:

| Caminho A | Caminho B | Alcance/limite |
|---|---|---|
| **DP→PeriodoAquisitivo**: `registrar_gozo_historico` atomic → `ag.save()` → espelho com recusa → Pauta DP → `alvo.save()` (`app/ferias/services.py:277`, `app/ferias/services.py:298`, `app/ferias/models.py:449`, `app/ferias/models.py:451`, `app/ponto/services/ausencia.py:1216`, `app/ponto/services/ausencia.py:1245`, `app/ferias/services.py:302`). `executar_agendamento` produz a mesma ordem (`app/ferias/services.py:315`, `app/ferias/services.py:321`, `app/ferias/services.py:326`). | **PeriodoAquisitivo→DP**: `_trocar_janela` atomic → trava AG → trava PA → save AG → espelho → Pauta DP (`app/ferias/services.py:433`, `app/ferias/services.py:364`, `app/ferias/services.py:367`, `app/ferias/services.py:450`, `app/ferias/models.py:451`, `app/ponto/services/ausencia.py:1245`). | Chamadores runtime: histórico `app/ferias/views.py:211`; agendar `app/ferias/views.py:299`, `app/ferias/views.py:636`; corrigir datas `app/ferias/views.py:503`. **Inversão no grafo condicional; ciclo concreto [nao verificado]**: o guard anterior do agendamento pode recusar (`app/ferias/models.py:435`), enquanto a Pauta exige recusa posterior de `criar_ausencia` (`app/ponto/services/ausencia.py:1202`, `app/ponto/services/ausencia.py:1209`). Não conferi constraints SQL/FKs efetivamente implantadas nem fixture concorrente que atravesse ambos os guards: [nao verificado]. |

Não transformar esse último candidato em “deadlock demonstrado”: só o ramo condicional e as duas ordens estão no código; satisfação conjunta e concorrência real ficam **[nao verificado]**.

## 11/11 15:32 · sessao ed15150ab293 · FABLE-GABARITO · verificador · origem=Code

1. **BLOQUEANTE.** Entrada: alterar `PRAZO` em `app/core/tests/marca_cron15.py:24`, declarando B. Esse valor participa da decisão do cron de produção (`app/core/management/commands/crons.py:92`, `:97`). O selo aceita o diretório `tests` indiscriminadamente (`bin/selo_proporcao.py:66`) e libera B (`bin/selo_proporcao.py:105`, `:111`). Motivo literal: **“arquivo de teste”**, **“qualquer outro .py no diff = classe A”** e **“A fuga entre modulos NAO entra no custo aceito”**; regras registradas em `app/docs/CORTES.md:444`. A fuga concreta alcança produção.

2. **BLOQUEANTE.** Entrada: acrescentar `## FEITO: exemplo` à allowlist `bin/tests/afirma_sem_prova_base.txt`, declarando B. O portão lê esse arquivo e desconsidera afirmações nele presentes (`bin/relato_afirma_com_prova.py:54`, `:77`, `:97`). O selo libera seu caminho pela mesma exceção indiscriminada (`bin/selo_proporcao.py:66`, `:111`). Motivo literal: **“CLASSE B so se TODO arquivo do diff”** pertencer às categorias autorizadas; a categoria é **“arquivo de teste”**, não qualquer conteúdo de `bin/tests/` (`app/docs/CORTES.md:444`). Aqui muda uma permissão efetivamente consumida pelo portão.

3. **FILA.** Entrada: C0 → C1 altera `app/x/views.py`; C1 → C2 altera `app/templates/x/a.html`. Com base C1, o selo recebe somente o template; com C0, recebe também o `.py` e exige A (`bin/selo_proporcao.py:78`, `:89`, `:92`, `:108`). Isso demonstra possibilidade de enganar pelo recorte da entrada. Motivo literal do aval original: **“achado do Codex sobre a FORMA de testar (como o selo poderia ser enganado) vira linha de fila, nunca volta”**; regra registrada em `app/docs/CORTES.md:443`. Obrigação específica de usar `merge-base(main, HEAD)` nos avais: **[nao verificado]**.

4. **FILA, nos limites comprovados.** Entrada: BASE e HEAD com listas diferentes; o código lê a BASE (`bin/selo_proporcao.py:47`, `:91`), enquanto um comentário promete HEAD (`bin/tests/test_selo_proporcao.sh:91`). Os testes efetivos também usam BASE (`bin/tests/test_selo_proporcao.sh:44`). Aplica-se o literal **“achado […] sobre a FORMA de testar […] vira linha de fila”**, registrado em `app/docs/CORTES.md:443`. Contrato e promessa específica da MEDIDA na cópia: **[nao verificado]**. Chamador automático de pouso não localizado; execução manual no pouso: **[nao verificado]**. Portanto, não está demonstrado descumprimento do literal **“SELO no pouso”** (`app/docs/CORTES.md:443`).

Reprodução dos rc nesta sessão: **[nao verificado]**; os resultados descritos são compatíveis com os ramos citados do código.

## 11/11 15:32 · sessao b199bd514621 · - · - · origem=Ronald (tmux)

**Matriz humana: 20 arestas distintas permitidas**, sendo 10 pela tabela comum e outras 10 pelo privilégio `admin_ti`; `cancelado` e `superado` não podem ser origem nem destino de transição humana. (`app/chamados/catalogo/motor.py:99`; `app/chamados/catalogo/motor.py:142`; `app/chamados/catalogo/motor.py:145`; `app/chamados/catalogo/motor.py:150`; `app/chamados/catalogo/motor.py:153`)

Notação das sequências:

- **E**: `atomic E4B → C → D abertas → Ch vivos/pais/alvo → UPDATE Ch → receivers → relavra S2 → âncoras ordenadas → INSERT Pauta E4B → contador DP → fim atomic E4B`. Os receivers e a relavra podem acrescentar escritas opcionais descritas abaixo. (`app/chamados/services/fecho_runtime.py:147`; `app/chamados/models.py:290`; `app/chamados/models.py:296`; `app/chamados/models.py:307`; `app/chamados/services/fecho_runtime.py:120`; `app/chamados/services/fecho_runtime.py:127`)
- **N**: `atomic _gravar_estado → C → D abertas → Ch vivos/pais/alvo → UPDATE Ch → receivers → eventual relavra S2 → fim atomic _gravar_estado`; este ramo não chama o agrupador E4B. (`app/chamados/services/fecho_runtime.py:144`; `app/chamados/services/fecho_runtime.py:145`; `app/chamados/models.py:280`; `app/chamados/models.py:290`; `app/chamados/models.py:296`; `app/chamados/models.py:303`)
- **C/D/Ch/P** são as linhas de Colaborador, Disputa, Chamado e Pergunta; cada conjunto explícito é adquirido em PK crescente. (`app/chamados/models.py:2666`; `app/chamados/models.py:2675`; `app/chamados/models.py:2688`; `app/chamados/models.py:2692`)

| De → para | Perfil pela porta | Sequência | DP antes da âncora E4B |
|---|---|---|---|
| aberto → em_analise | Comum (`motor.py:100`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior nesse ramo (`fecho_runtime.py:145`) |
| aberto → resolvido | Comum (`motor.py:100`) | E (`fecho_runtime.py:147`) | Pode, pela fábrica (`fabrica_da_celula.py:204`; `:215`; `:369`) |
| aberto → fechado | Comum (`motor.py:100`) | E (`fecho_runtime.py:147`) | Pode (`fabrica_da_celula.py:204`; `:215`; `:369`) |
| aberto → registrado | admin_ti (`motor.py:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| em_analise → aberto | Comum (`motor.py:101`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| em_analise → resolvido | Comum (`motor.py:101`) | E (`fecho_runtime.py:147`) | Pode (`fabrica_da_celula.py:204`; `:215`; `:369`) |
| em_analise → fechado | Comum (`motor.py:101`) | E (`fecho_runtime.py:147`) | Pode (`fabrica_da_celula.py:204`; `:215`; `:369`) |
| em_analise → registrado | admin_ti (`motor.py:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| resolvido → aberto | admin_ti (`motor.py:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| resolvido → em_analise | Comum (`motor.py:102`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| resolvido → fechado | Comum (`motor.py:102`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| resolvido → registrado | admin_ti (`motor.py:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| fechado → aberto | admin_ti (`motor.py:150`; `:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| fechado → em_analise | admin_ti (`motor.py:150`; `:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| fechado → resolvido | admin_ti (`motor.py:150`; `:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| fechado → registrado | admin_ti (`motor.py:150`; `:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| registrado → aberto | admin_ti (`motor.py:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| registrado → em_analise | Comum (`motor.py:105`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| registrado → resolvido | admin_ti (`motor.py:153`) | E (`fecho_runtime.py:147`) | Pode (`fabrica_da_celula.py:204`; `:215`; `:369`) |
| registrado → fechado | Comum (`motor.py:105`) | E (`fecho_runtime.py:147`) | Pode (`fabrica_da_celula.py:204`; `:215`; `:369`) |

Na tabela, `motor.py` significa `app/chamados/catalogo/motor.py`, `models.py` significa `app/chamados/models.py` e os nomes dos serviços pertencem a `app/chamados/services/`. As arestas dependem dos guards da porta: decisão de perfil, justificativa pendente, disputa viva, pergunta viva em competência aberta e premissa regularizada para `resolvido`. (`app/chamados/models.py:132`; `app/chamados/models.py:136`; `app/chamados/models.py:148`; `app/chamados/models.py:165`; `app/chamados/models.py:181`)

As 20 arestas têm chamador runtime pelos dois serviços: detalhe local e resposta rápida no fio/modal; ocorrência concreta em produção **[nao verificado]**. (`app/chamados/services/acoes_chamado.py:313`; `app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:719`; `app/chamados/services/fio.py:722`; `app/chamados/views.py:547`; `app/chamados/views.py:674`; `app/chamados/views.py:954`; `app/chamados/urls.py:14`; `app/chamados/urls.py:21`; `app/chamados/urls.py:39`)

| Subcaminho síncrono dos receivers | Ordem e escritas |
|---|---|
| S143, apenas destino encerrado | Retoma C/D/Ch pela porta, tenta devolver alcance antes dos carimbos, grava perguntas mortas, pode reabrir pai e fechar disputas. (`app/chamados/signals.py:96`; `:115`; `app/chamados/services/disputa_emissao.py:1878`; `:1945`; `:1984`; `:2009`; `:2058`; `:2090`) |
| Carimbo de pergunta | `UPDATE Pergunta`, dentro de `PerguntaDisputa.save` e seu atomic; o receiver pode inserir EventoCelula. (`app/chamados/models.py:3152`; `:3422`; `:3423`; `app/chamados/signals.py:258`; `app/ponto/portas/celula.py:896`) |
| Fecho de disputa | Reaquisição C/D/Ch, `UPDATE Disputa`, S88 pode reconciliar o pai; S144 carimba pendentes já aprovadas pelo lastro. (`app/chamados/models.py:1910`; `:1936`; `app/chamados/signals.py:42`; `:164`; `app/chamados/services/materializacao.py:1418`; `:1427`) |
| Cartório do chamado | Roda se o status mudou e não há julgamento reentrante; deriva dia pelo juiz e chama `julgar_celula`. (`app/chamados/signals.py:193`; `:211`; `:215`; `:217`) |
| Lavra da célula | `UPDATE CelulaDia → INSERT EventoCelula`; a lavra do modelo delega à porta. (`app/escala/models.py:1826`; `app/ponto/portas/celula.py:904`; `:905`; `:907`; `:896`) |
| Fábrica após a lavra | Cartório chama fábrica das células julgadas; cadastro sem turno toma C e escreve DP; limbo exportado escreve DP diretamente, herdando as travas do ato. (`app/ponto/services/cartorio.py:1020`; `app/chamados/services/fabrica_da_celula.py:204`; `:215`; `:279`; `:364`; `:365`; `:369`) |
| Relavra S2 após o save | Pode `UPDATE Pergunta` depois do cartório e, portanto, depois de uma Pauta da fábrica. (`app/chamados/models.py:296`; `:307`; `:312`; `app/chamados/veredito_lavrado.py:275`) |
| Eventos pós-commit | Beco de disputa e silêncio de isento são callbacks `on_commit`; não integram a sequência anterior ao commit do atomic E4B. (`app/chamados/signals.py:239`; `:280`) |

As condições da emissão antecipada são explícitas: célula passada de colaborador ativo, dia não dispensado e teto sem turno para cadastro; para limbo, motivos existentes, `trabalha=None` e competência entregue. Os guards e a célula concreta que satisfazem cada combinação em produção são **[nao verificado]**. (`app/chamados/services/fabrica_da_celula.py:201`; `:203`; `:204`; `:228`; `:231`)

| Transição indireta | Chamador síncrono alcançável e ordem |
|---|---|
| aberto → resolvido; em_analise → resolvido; registrado → resolvido | `RECONCILIAR` pelo S88, cartório ou cluster; C/D/Ch antes do UPDATE do estado e seus receivers. (`app/chamados/catalogo/motor.py:111`; `app/chamados/signals.py:42`; `app/ponto/services/cartorio.py:986`; `:1511`; `:1524`; `:1545`; `:1623`; `:1638`; `app/ponto/services/cluster_espurio.py:124`; `app/chamados/models.py:650`; `:654`; `:711`) |
| resolvido → em_analise; fechado → em_analise | `REABRIR` no S143 ou no renascimento por emissão; `_gravar_estado` seguido de ressuscitação C/D/Ch/P, UPDATE perguntas e possível reabertura de disputa. (`app/chamados/catalogo/motor.py:113`; `app/chamados/services/disputa_emissao.py:1849`; `:2009`; `app/chamados/models.py:921`; `:1019`; `:1442`; `:1451`; `:1562`; `:1566`; `:1575`) |
| aberto → em_analise; registrado → em_analise | `ANALISAR` pelo repasse do emissor do cartório; C/D/Ch/P do conjunto no topo do repasse, depois `_gravar_estado`. (`app/chamados/catalogo/motor.py:112`; `app/ponto/services/cartorio.py:1763`; `:1801`; `app/ponto/management/commands/detectar_ausencias.py:802`; `app/chamados/services/disputa_emissao.py:1059`; `:1085`; `:1159`; `:1178`; `:1223`; `:1252`; `app/chamados/models.py:806`) |
| inexistente → aberto | Emissão de chamado pela fábrica, cluster ou emissor do cartório; C antes do dedup e INSERT. (`app/chamados/catalogo/motor.py:131`; `app/chamados/models.py:902`; `:905`; `:935`; `:971`; `app/chamados/services/disputa_emissao.py:568`; `app/ponto/services/cluster_espurio.py:130`; `app/ponto/management/commands/detectar_ausencias.py:693`) |
| inexistente → registrado | Existe no catálogo de nascimento; alcance concreto desse destino pelos emissores deste recorte **[nao verificado]**. (`app/chamados/catalogo/motor.py:133`; `app/chamados/models.py:935`) |
| `ENCERRAR` / `CANCELAR` | Existem no catálogo e no modelo; caminho síncrono saindo desses receivers até essas portas **[nao verificado]**. (`app/chamados/catalogo/motor.py:114`; `:116`; `app/chamados/models.py:773`; `:1612`) |

**Censo dos três nomes:** 17 chamadas AST, contando apelidos de `from ... import ...`, acessos qualificados e excluindo strings de mutantes; execução sem artefato persistido **[nao verificado]**. Os callsites lidos são:

| Função | Produção | Teste | [nao verificado]
|---|---|---|
| `transicionar_com_pauta` — 3 | 2: `app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:722` | 1: `app/chamados/tests/test_e4b_c6_memoria.py:75` |
| `_escrever_pautas` — 13 | 1: `app/chamados/services/fecho_runtime.py:154` | 12: memória `:128`, `:129`, `:163`, `:168`, `:189`, `:231`; ORM `:138`, `:182`, `:208`, `:209`, `:225`, `:266`, nos arquivos `app/chamados/tests/test_e4b_c6_memoria.py` e `app/chamados/tests/test_e4b_c6_orm.py` |
| `_travar_ancora` — 1 | 1: `app/chamados/services/fecho_runtime.py:121` | 0 encontrados **[nao verificado]** |

Os 12 testes diretos de `_escrever_pautas` não executam transição de Chamado nessa chamada; o teste de memória do adaptador executa a transição simulada para `fechado`. (`app/chamados/tests/test_e4b_c6_memoria.py:75`; `:128`; `:163`; `:231`; `app/chamados/tests/test_e4b_c6_orm.py:138`; `:182`; `:208`; `:225`; `:266`)

## 11/11 15:31 · sessao a139ee4f6b1c · - · - · origem=Ronald (tmux)

(1) **BLOQUEANTE.** A exceção aceita qualquer diretório `tests` ([bin/selo_proporcao.py:66](bin/selo_proporcao.py:66)), mas esse módulo participa do caminho de produção: import em [app/core/management/commands/crons.py:92](app/core/management/commands/crons.py:92), decisão pelo prazo em [app/core/management/commands/crons.py:97](app/core/management/commands/crons.py:97). Entrada concreta: alterar `PRAZO` ([app/core/tests/marca_cron15.py:24](app/core/tests/marca_cron15.py:24)). O caminho recebe B e retorna 0 ([bin/selo_proporcao.py:105](bin/selo_proporcao.py:105), [bin/selo_proporcao.py:111](bin/selo_proporcao.py:111)). Contraria “qualquer outro .py no diff = classe A” e “A fuga entre modulos NAO entra no custo aceito” [não verificado: transcrição fornecida].

(3) **FILA**, pelos elementos fornecidos. A base é argumento explícito ([bin/selo_proporcao.py:78](bin/selo_proporcao.py:78)); exige ancestralidade ([bin/selo_proporcao.py:89](bin/selo_proporcao.py:89)) e calcula exatamente esse intervalo ([bin/selo_proporcao.py:92](bin/selo_proporcao.py:92)). Entrada concreta: C0 → C1 altera `.py` fora da lista → C2 altera template; base=C1 inclui apenas template, classificado B ([bin/selo_proporcao.py:62](bin/selo_proporcao.py:62)). Os avais não prescrevem main/merge-base; enquadramento literal: “FORMA de testar (como o selo poderia ser enganado) vira linha de fila, nunca volta” [não verificado: transcrição fornecida]. Vincular a base ao diff completo da obra é pendência; impor especificamente merge-base seria requisito adicional [não verificado].

Os retornos foram inferidos do código; execução dessas entradas nesta sessão [não verificado].

_3960 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
