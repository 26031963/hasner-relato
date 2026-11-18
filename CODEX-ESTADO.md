# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 10:16** · publicado em **18/11/2026 10:18** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 10:16 · sessao fec0b6249760 · RESPOSTAS-DE-LEI-1811-1010 · censo · origem=Code

**O censo ainda está ABERTO nos quatro itens.** A saída recebida permitiu confirmar escritores, leitores e caminhos que faltavam, mas não sustenta uma lista exaustiva nem a classificação individual de todos os acessos dinâmicos. **[nao verificado: completude do censo]**

Há duas correções relevantes para o construtor:

- A escrita de `validada_em` não se limita ao ato de validar: há limpeza pela resposta humana, devoluções e preenchimentos associados ao carimbo. A distinção é entre o dono do **ato de validar** e todos os escritores do campo. [app/chamados/services/validacao.py:1] [app/chamados/models.py:3249] [app/chamados/services/disputa_emissao.py:1494]
- A carga remove as decisões marcadas como ressuscitação antes de escolher o último ato, mas `respostas_anteriores` percorre atos com fotografia sem esse filtro. Um novo ato marcado precisa ter os dois leitores avaliados. [app/chamados/services/tarefa_do_dia.py:421] [app/chamados/models.py:3688]

As listas abaixo registram os sítios confirmados nesta volta; **não são a lista completa pedida**. **[nao verificado: sítios restantes]**

### 1. Escritores e leitores R/V/C e porta `registrar` — STATUS ABERTO

**Escrita da projeção:**

- Resposta humana e invalidação da validação anterior: [app/chamados/services/disputa_emissao.py:1481].
- Resposta automática e carimbo pela trilha: [app/chamados/services/disputa_emissao.py:1675] [app/chamados/services/disputa_emissao.py:1698].
- Validação e devoluções: [app/chamados/services/validacao.py:168] [app/chamados/services/validacao.py:273] [app/chamados/services/validacao.py:349].
- Carimbo com preenchimento condicional de resposta e validação: [app/chamados/models.py:3249].
- Escrita em lote dinâmica por `**outros`: [app/chamados/models.py:3371] [app/chamados/models.py:3404]. Os chamadores confirmados limpam R/V/C na ressuscitação, R/V/C na reabertura e o par de resposta na devolução L-130: [app/chamados/models.py:1637] [app/chamados/services/disputa_emissao.py:1610] [app/chamados/services/disputa_emissao.py:1649] [app/chamados/services/validacao.py:440].
- A entrada por foto delega à resposta automática e à validação; o cabeçalho antigo do módulo que a descreve como escrita local não corresponde ao corpo atual. [app/api/foto_recebida.py:2] [app/api/foto_recebida.py:39]

**Escrita da sequência pela porta `DecisaoPergunta.registrar`:**

- Modelo e portas de resposta/reabertura: [app/chamados/models.py:1619] [app/chamados/models.py:2013] [app/chamados/models.py:2145] [app/chamados/models.py:3137] [app/chamados/services/disputa_emissao.py:1525] [app/chamados/services/disputa_emissao.py:1632] [app/chamados/services/disputa_emissao.py:1679] [app/chamados/services/disputa_emissao.py:1701].
- Validação, devoluções e absorção: [app/chamados/services/validacao.py:173] [app/chamados/services/validacao.py:279] [app/chamados/services/validacao.py:354] [app/chamados/services/validacao.py:439] [app/chamados/services/materializacao.py:1048].
- Cartório e comandos de acervo: [app/ponto/services/veredito_celula.py:195] [app/chamados/management/commands/backfill_decisoes_pergunta.py:324] [app/chamados/management/commands/backfill_fio_morto_e4b.py:141].
- Falso positivo: `registrar` em [app/chamados/views.py:91] é da recusa crítica, não a porta de `DecisaoPergunta`. [app/chamados/views.py:90]

**Leituras confirmadas que o desenho precisa considerar:**

- Juízes, faces SQL e ordenação da fila: [app/chamados/juizes.py:352] [app/chamados/juizes.py:856] [app/chamados/juizes.py:1389] [app/chamados/juizes.py:1534] [app/chamados/juizes.py:1552] [app/chamados/juizes.py:1572] [app/chamados/juizes.py:1580] [app/chamados/juizes.py:1761].
- Estado e apresentação da pergunta; histórico de decisões: [app/chamados/models.py:3159] [app/chamados/models.py:3681] [app/chamados/models.py:3701].
- Fio e materialização: [app/chamados/services/fio.py:188] [app/chamados/services/fio.py:263] [app/chamados/services/materializacao.py:546] [app/chamados/services/materializacao.py:1153] [app/chamados/services/materializacao.py:2091].
- API e apresentação: [app/api/views.py:1163] [app/api/views.py:2561] [app/api/views.py:2639] [app/api/views_mensageria.py:567] [app/chamados/templatetags/chamados_tags.py:139] [app/chamados/templatetags/chamados_tags.py:166].
- Histórico cronológico lê dinamicamente os três instantes e o texto; a reversão do passivo também fotografa campos da pergunta: [app/chamados/services/historico.py:96] [app/chamados/services/passivo_validado.py:64] [app/chamados/services/passivo_validado.py:83].
- Auditoria, placares e procedência têm leituras próprias: [app/chamados/management/commands/auditar_invariantes_chamados.py:1077] [app/chamados/management/commands/auditar_invariantes_chamados.py:1107] [app/chamados/services/painel.py:261] [app/chamados/services/verbos.py:263] [app/ponto/services/procedencia_batida.py:71].

**Por que permanece aberto:** faltam a classificação de todos os leitores listados no inventário, o rastreamento completo dos chamadores e a resolução individual dos acessos genéricos que podem receber objetos ou nomes em execução. **[nao verificado]**

### 2. Importadores, selo e medida da sombra — STATUS ABERTO

Importadores confirmados: `relavrar_via_i6` importa `universo_i6`; `dedup_perguntas_fato` importa `aplicar` e `CONTADORES_DO_APPLY`; `worklist` importa `_parse_pendentes` e `MODULOS_ALVO`, e tem consumidores na view e no comando. [app/chamados/management/commands/relavrar_via_i6.py:46] [app/chamados/management/commands/dedup_perguntas_fato.py:105] [app/ponto/worklist.py:37] [app/colaboradores/views.py:3061] [app/ponto/management/commands/worklist_dp.py:12]

O selo global de vitalidade declarado examina `materializada_em` e `validada_em` e formas sintáticas específicas; ele não demonstra, sozinho, cobertura de todos os campos ou acessos dinâmicos. [app/chamados/tests/test_contract_leitores_vitalidade.py:14] [app/chamados/tests/test_contract_leitores_vitalidade.py:329] O selo específico lista os quatro escritores esperados de `resposta_mantem_validacao`. [app/chamados/tests/test_f2_resposta_mantem_validacao.py:19]

A medida raspada da **sombra** informa:

| Projeção atual nula com ato histórico correspondente | Último físico / efetivo | Total |
|---|---|---:|
| Resposta | reabertura / reabertura | 52 |
| Resposta | recusa / recusa | 7 |
| Validação | devolução / devolução | 60 |
| Validação | reabertura / reabertura | 9 |
| Validação | resposta / resposta | 3 |

Esses valores são os fornecidos no anexo da conversa; produção e interseção dos grupos são **[nao verificado]**. A função confirmada mede o sentido inverso, projeção sem linha correspondente por ato e instante; os números acima não demonstram erros nem uma correspondência geral ato → projeção. [app/chamados/models.py:4122] [app/chamados/services/disputa_emissao.py:1632]

A exclusividade dos importadores e a classificação completa dos selos permanecem **[nao verificado]**.

### 3. Campos novos e leituras de REABERTURA — STATUS ABERTO

- `da_ressuscitacao` e `resposta_mantem_validacao` têm defaults e restrições por tipo de ato. [app/chamados/models.py:4071] [app/chamados/models.py:4077] [app/chamados/models.py:4095] [app/chamados/models.py:4098]
- A marca de ressuscitação é lida pelo predicado em [app/chamados/models.py:4103] e pela carga em [app/chamados/services/tarefa_do_dia.py:421]. A porta de ressuscitação nesta cópia registra a projeção faltante e limpa os campos; não se pode presumir que já grave a nova marca. [app/chamados/models.py:1619] [app/chamados/models.py:1637]
- A reabertura fotografa resposta, instantes e autor antes de limpar a projeção; `respostas_anteriores` é leitor da sequência e a carga do fio busca também o autor original. [app/chamados/services/disputa_emissao.py:1632] [app/chamados/models.py:3688] [app/chamados/services/fio.py:263]
- Os valores declarados para `resposta_mantem_validacao` são `True` na fotografia da projeção, resposta automática e carimbo pela trilha; `False` na resposta humana. [app/chamados/models.py:4141] [app/chamados/services/disputa_emissao.py:1525] [app/chamados/services/disputa_emissao.py:1679] [app/chamados/services/disputa_emissao.py:1701]

**Contradição a resolver no desenho:** o comentário do modelo associa a regra atual ao `ato_motivo`, mas `_pela_decisao` aqui trata toda última `RESPOSTA` com texto como `aguarda_validacao`, sem consultar esse motivo ou `resposta_mantem_validacao`. [app/chamados/models.py:4072] [app/chamados/services/tarefa_do_dia.py:158] Não está demonstrado que trocar esse leitor pela nova marca preserve o comportamento atual. **[nao verificado]**

A lista completa de leitores indiretos e o efeito de todos os caminhos de `registrar` seguem **[nao verificado]**.

### 4. Cascas, templates e PDF — STATUS ABERTO

- `base_tpl` é escolhido pelo contexto entre `base_app.html` e `base.html`; `meu_atendimento.html` usa essa escolha e inclui o cartão nas duas abas. [app/core/context_processors.py:194] [app/core/context_processors.py:205] [app/templates/chamados/meu_atendimento.html:1] [app/templates/chamados/meu_atendimento.html:127] [app/templates/chamados/meu_atendimento.html:144]
- O cartão do colaborador lê resposta e instante. [app/templates/chamados/partials/_card_atendimento.html:106] [app/templates/chamados/partials/_card_atendimento.html:143] [app/templates/chamados/partials/_card_atendimento.html:183] [app/templates/chamados/partials/_card_atendimento.html:391]
- A casca administrativa `detalhe_local` estende `base.html` e inclui `_pergunta_card`, que também pode ser renderizado isoladamente por HTMX. [app/templates/chamados/detalhe_local.html:1] [app/templates/chamados/detalhe_local.html:310] [app/chamados/views.py:398]
- `modal_fio` lê histórico, resposta e validação e também é renderizado isoladamente; portanto o grafo que parte apenas das bases não o enumera como alcançado. [app/templates/chamados/partials/modal_fio.html:205] [app/templates/chamados/partials/modal_fio.html:219] [app/templates/chamados/partials/modal_fio.html:228] [app/chamados/views.py:927] [app/chamados/views.py:1399]
- A prévia de lote lê a resposta e é renderizada pela view de cobrança. [app/templates/chamados/partials/_lote_preview.html:24] [app/chamados/views_cobrar.py:360]
- No caminho de PDF inspecionado, a view alimenta o gerador de histórico e o bloco examinado lê followups. Relações indiretas adicionais entre PDF e R/V/C permanecem **[nao verificado]**. [app/relatorios/views.py:871] [app/relatorios/pdf_chamados.py:135]

### Classificação dos dinâmicos examinados

- **Escrita real de R/V/C:** `models.py:3404`, por `**outros`, com os três chamadores identificados no item 1. [app/chamados/models.py:3379] [app/chamados/models.py:3404]
- **Leitura real:** `historico.py:105`; `passivo_validado.py:83`; e `_PD` em `veredito_lavrado.py:202` e `:206` é alias de `PerguntaDisputa`, com filtros sobre materialização e resposta. [app/chamados/services/historico.py:96] [app/chamados/services/passivo_validado.py:64] [app/chamados/veredito_lavrado.py:163] [app/chamados/veredito_lavrado.py:202]
- **Não é acesso R/V/C nesses sítios:** os dinâmicos de `ChamadoColaborador` em `models.py:637`, `:953`, `:976`, `:1018` e `:1142`; os filtros de chave de família em `fio.py:2110`, `:2112`, `:2122`, `:2134`, `:2164`, `:2166` e `:2187`; e os filtros dinâmicos por identidade nas duas portas de validação. [app/chamados/models.py:637] [app/chamados/models.py:953] [app/chamados/models.py:976] [app/chamados/models.py:1018] [app/chamados/models.py:1142] [app/chamados/services/fio.py:1515] [app/chamados/services/fio.py:2110] [app/chamados/services/fio.py:2112] [app/chamados/services/fio.py:2122] [app/chamados/services/fio.py:2134] [app/chamados/services/fio.py:2164] [app/chamados/services/fio.py:2166] [app/chamados/services/fio.py:2187] [app/chamados/services/validacao.py:673] [app/chamados/services/validacao.py:890]
- **Ainda indeterminados:** os demais candidatos genéricos do inventário, inclusive o alcance completo dos wrappers de `carga_pergunta_viva` e dos resolvedores genéricos de `core`, não foram classificados um a um. [app/chamados/services/carga_pergunta_viva.py:30] [app/chamados/services/carga_pergunta_viva.py:59] [app/core/intake.py:487] [app/core/reexibir.py:49] **[nao verificado]**

Para terminar a classificação dos **selos** sem acesso a logs, seria necessária a parte da saída bruta filtrada referente a `tests/`; para o censo de produção ainda falta completar a leitura dos candidatos e caminhos indiretos acima. **[nao verificado]**

alcance nao verificado restante: 4

## 18/11 10:15 · sessao 6e3da0f0e6be · RESPOSTAS-DE-LEI-1811-1010 · censo · origem=Code

O censo **ainda não autoriza abrir o construtor**: os itens 1, 3 e 5 permanecem abertos. O inventário amplia os sítios conhecidos, mas não demonstra que todos os acessos dinâmicos, aliases indiretos, carregamentos de templates e leitores foram classificados. [não verificado]

Há uma contradição concreta com a formulação da volta 1 sobre Q4: o card de pergunta exibe “validada” diretamente quando `validada_em` existe, mas o “Validada” do modal está dentro do ramo `perg.respondida` e depois do ramo prioritário `pendencia_admin`. Assim, validação projetada sem linha de decisão, por si só, **não demonstra** que o modal mostrará “Validada”; isso também depende do par de resposta e da ausência de pendência prioritária. [app/templates/chamados/partials/_pergunta_card.html:23](app/templates/chamados/partials/_pergunta_card.html:23) [app/templates/chamados/partials/modal_fio.html:217](app/templates/chamados/partials/modal_fio.html:217) [app/templates/chamados/partials/modal_fio.html:222](app/templates/chamados/partials/modal_fio.html:222) [app/templates/chamados/partials/modal_fio.html:228](app/templates/chamados/partials/modal_fio.html:228)

### 1. Cascas do card — STATUS: ABERTO

Os caminhos de produção identificados para a F7 são:

- **Card do colaborador, leitura:** `meu_atendimento` inclui `_card_atendimento` nos dois loops; a casca pode ser `base.html` ou `base_app.html`, conforme `base_tpl`. O card lê o par de resposta e a fase. [app/templates/chamados/meu_atendimento.html:1](app/templates/chamados/meu_atendimento.html:1) [app/templates/chamados/meu_atendimento.html:127](app/templates/chamados/meu_atendimento.html:127) [app/templates/chamados/meu_atendimento.html:144](app/templates/chamados/meu_atendimento.html:144) [app/core/context_processors.py:210](app/core/context_processors.py:210) [app/templates/chamados/partials/_card_atendimento.html:106](app/templates/chamados/partials/_card_atendimento.html:106) [app/templates/chamados/partials/_card_atendimento.html:189](app/templates/chamados/partials/_card_atendimento.html:189)
- **Card da pergunta, leitura:** a página de detalhe usa `base.html` e inclui `_pergunta_card`; o rerender parcial também chega ao mesmo template por `_render_card`, alias de `render`. [app/templates/chamados/detalhe_local.html:1](app/templates/chamados/detalhe_local.html:1) [app/templates/chamados/detalhe_local.html:310](app/templates/chamados/detalhe_local.html:310) [app/chamados/views.py:397](app/chamados/views.py:397) [app/chamados/views.py:404](app/chamados/views.py:404)
- **Modal do fio, leitura:** é um fragmento renderizado diretamente, sem casca própria; uma hospedeira comprovada é o painel em `base.html`, que o busca pela rota. Há também chamada da fila de benefícios com `modo=beneficio`; a lista integral de hospedeiras e suas cascas permanece [não verificado]. [app/chamados/views.py:927](app/chamados/views.py:927) [app/chamados/views.py:1400](app/chamados/views.py:1400) [app/chamados/services/fio.py:543](app/chamados/services/fio.py:543) [app/templates/chamados/painel_gestao.html:1](app/templates/chamados/painel_gestao.html:1) [app/templates/chamados/painel_gestao.html:528](app/templates/chamados/painel_gestao.html:528) [app/templates/colaboradores/fila_solicitacoes_beneficio.html:119](app/templates/colaboradores/fila_solicitacoes_beneficio.html:119)
- **Prévia de lote, leitura:** é fragmento renderizado diretamente; o painel em `base.html` abre-o no drawer. Outras hospedeiras não foram excluídas. [app/templates/chamados/painel_gestao.html:14](app/templates/chamados/painel_gestao.html:14) [app/chamados/views_cobrar.py:360](app/chamados/views_cobrar.py:360) [app/chamados/views_cobrar.py:364](app/chamados/views_cobrar.py:364) [app/chamados/views_cobrar.py:380](app/chamados/views_cobrar.py:380) [app/chamados/views_cobrar.py:388](app/chamados/views_cobrar.py:388) [app/templates/chamados/partials/_lote_preview.html:24](app/templates/chamados/partials/_lote_preview.html:24)

O grafo fornecido identifica includes compartilhados pelas duas bases, mas não prova, sozinho, todas as hospedeiras dos fragmentos nem renderização e cliques em execução. Essa revisão transitiva continua aberta. [app/templates/base.html:93](app/templates/base.html:93) [app/templates/base_app.html:85](app/templates/base_app.html:85) [app/templates/base.html:399](app/templates/base.html:399) [app/templates/base_app.html:154](app/templates/base_app.html:154) [não verificado]

### 2. Escritor de reabertura — STATUS: FECHADO para a dúvida específica

`reabrir_para_correcao` prepara a limpeza do par e da validação, registra uma decisão com a fotografia anterior e chama `descarimbar_em_lote`; o corpo dessa função não chama `linhas_da_projecao_sem_linha`. Isso fecha a dúvida sobre **essa chamada**, sem fechar o censo de escritores. [app/chamados/services/disputa_emissao.py:1577](app/chamados/services/disputa_emissao.py:1577) [app/chamados/services/disputa_emissao.py:1610](app/chamados/services/disputa_emissao.py:1610) [app/chamados/services/disputa_emissao.py:1632](app/chamados/services/disputa_emissao.py:1632) [app/chamados/services/disputa_emissao.py:1649](app/chamados/services/disputa_emissao.py:1649)

### 3. Leitores cruzados e escritores — STATUS: ABERTO

A lista seguinte é de **sítios comprovados a considerar**, não uma lista integral nem uma determinação de que todos devam ser alterados pela F7. O brief completo e o efeito exato pretendido em cada consumidor não estão demonstrados no material fornecido. [não verificado]

- **Escrita persistida:** resposta e validação em `disputa_emissao`; validações em `validacao`; carimbo com possível preenchimento de resposta e validação em `models`; e limpeza em lote, inclusive campos passados por `**outros`. O último ponto é relevante: a porta verifica expressamente se `outros` contém campos do par e os passa ao `update`. [app/chamados/services/disputa_emissao.py:1481](app/chamados/services/disputa_emissao.py:1481) [app/chamados/services/disputa_emissao.py:1494](app/chamados/services/disputa_emissao.py:1494) [app/chamados/services/disputa_emissao.py:1675](app/chamados/services/disputa_emissao.py:1675) [app/chamados/services/disputa_emissao.py:1698](app/chamados/services/disputa_emissao.py:1698) [app/chamados/services/validacao.py:168](app/chamados/services/validacao.py:168) [app/chamados/services/validacao.py:273](app/chamados/services/validacao.py:273) [app/chamados/services/validacao.py:349](app/chamados/services/validacao.py:349) [app/chamados/models.py:3247](app/chamados/models.py:3247) [app/chamados/models.py:3251](app/chamados/models.py:3251) [app/chamados/models.py:3254](app/chamados/models.py:3254) [app/chamados/models.py:3371](app/chamados/models.py:3371) [app/chamados/models.py:3404](app/chamados/models.py:3404)
- **Atribuições que não são, isoladamente, escrita persistida:** as de `models.py:1561–1563` alteram uma cópia usada no julgamento; as de `models.py:3282–3284` restauram a instância em memória após recusa. Classificá-las simplesmente como escritoras de banco seria incorreto. [app/chamados/models.py:1548](app/chamados/models.py:1548) [app/chamados/models.py:1561](app/chamados/models.py:1561) [app/chamados/models.py:3278](app/chamados/models.py:3278)
- **Juízes e fase, leitura:** projeção temporal, juiz do par, propriedade `respondida` e `estado` são pontos centrais; `estado` dá precedência à validação projetada. [app/chamados/juizes.py:1382](app/chamados/juizes.py:1382) [app/chamados/juizes.py:1544](app/chamados/juizes.py:1544) [app/chamados/models.py:3152](app/chamados/models.py:3152) [app/chamados/models.py:3674](app/chamados/models.py:3674)
- **Interfaces e API, leitura:** além dos quatro templates do item 1, há apresentação de resposta por filtros e saídas da API e mensageria. [app/chamados/templatetags/chamados_tags.py:139](app/chamados/templatetags/chamados_tags.py:139) [app/chamados/templatetags/chamados_tags.py:166](app/chamados/templatetags/chamados_tags.py:166) [app/api/views.py:1163](app/api/views.py:1163) [app/api/views.py:2561](app/api/views.py:2561) [app/api/views_mensageria.py:567](app/api/views_mensageria.py:567)
- **Outros consumidores de produção, leitura:** materialização, reconciliação, histórico, painel, folha e cartório têm leituras diretas ou via juiz que precisam ser avaliadas conforme a mudança final. [app/chamados/services/materializacao.py:191](app/chamados/services/materializacao.py:191) [app/chamados/services/materializacao.py:2091](app/chamados/services/materializacao.py:2091) [app/chamados/reconciliador.py:365](app/chamados/reconciliador.py:365) [app/chamados/services/historico.py:98](app/chamados/services/historico.py:98) [app/chamados/services/painel.py:270](app/chamados/services/painel.py:270) [app/folha/previa.py:141](app/folha/previa.py:141) [app/ponto/services/cartorio.py:1641](app/ponto/services/cartorio.py:1641)
- **Auditoria e reconstrução, leitura e eventual escrita de decisões:** os comandos consultam as projeções, e o backfill chama a construção de linhas ausentes. Seu comportamento completo e a necessidade de mudança pela F7 permanecem [não verificado]. [app/chamados/management/commands/auditar_invariantes_chamados.py:1077](app/chamados/management/commands/auditar_invariantes_chamados.py:1077) [app/chamados/management/commands/backfill_decisoes_pergunta.py:56](app/chamados/management/commands/backfill_decisoes_pergunta.py:56) [app/chamados/management/commands/backfill_decisoes_pergunta.py:67](app/chamados/management/commands/backfill_decisoes_pergunta.py:67)

**Indeterminados examinados:**

- `models.py:637–638`: acesso dinâmico aos campos de urgência de `ChamadoColaborador`, não uma escrita da projeção de `PerguntaDisputa`. [app/chamados/models.py:620](app/chamados/models.py:620) [app/chamados/models.py:637](app/chamados/models.py:637)
- `models.py:953`, `:976`, `:1018`, `:1142`: filtros e criação da porta de `ChamadoColaborador`; não são, nesses sítios, escritas de `PerguntaDisputa`. [app/chamados/models.py:953](app/chamados/models.py:953) [app/chamados/models.py:976](app/chamados/models.py:976) [app/chamados/models.py:1018](app/chamados/models.py:1018) [app/chamados/models.py:1142](app/chamados/models.py:1142)
- `models.py:3404`: **escrita relevante**, inclusive dos campos dinâmicos `outros`, além dos carimbos explícitos. [app/chamados/models.py:3371](app/chamados/models.py:3371) [app/chamados/models.py:3404](app/chamados/models.py:3404)
- `historico.py:105`: **leitura relevante**; o laço percorre `respondida_em`, `validada_em` e `materializada_em`, e também lê o texto. [app/chamados/services/historico.py:98](app/chamados/services/historico.py:98) [app/chamados/services/historico.py:105](app/chamados/services/historico.py:105) [app/chamados/services/historico.py:109](app/chamados/services/historico.py:109)
- `passivo_validado.py:83`: **leitura relevante** para fotografia de reversão; a lista inclui materialização, resposta, validação e autor da validação. [app/chamados/services/passivo_validado.py:64](app/chamados/services/passivo_validado.py:64) [app/chamados/services/passivo_validado.py:83](app/chamados/services/passivo_validado.py:83)
- `validacao.py:677` e `:894`: filtros dinâmicos por `pk` e, opcionalmente, disputa; selecionam a pergunta, mas essas chaves não são campos da projeção. Os corpos seguintes ainda são consumidores relevantes. [app/chamados/services/validacao.py:673](app/chamados/services/validacao.py:673) [app/chamados/services/validacao.py:677](app/chamados/services/validacao.py:677) [app/chamados/services/validacao.py:890](app/chamados/services/validacao.py:890) [app/chamados/services/validacao.py:894](app/chamados/services/validacao.py:894)
- `veredito_lavrado.py:202` e `:206`: **leituras relevantes** dos instantes de materialização e resposta, com limite temporal dinâmico; `:177` e `:180` consultam `Batida`. [app/chamados/veredito_lavrado.py:163](app/chamados/veredito_lavrado.py:163) [app/chamados/veredito_lavrado.py:177](app/chamados/veredito_lavrado.py:177) [app/chamados/veredito_lavrado.py:180](app/chamados/veredito_lavrado.py:180) [app/chamados/veredito_lavrado.py:202](app/chamados/veredito_lavrado.py:202) [app/chamados/veredito_lavrado.py:206](app/chamados/veredito_lavrado.py:206)
- `fio.py:2112`, `:2122`, `:2134`, `:2164`, `:2166`, `:2187`: consultas dinâmicas às chaves de família e aos campos de exclusão do índice; os nomes são definidos nas tabelas do próprio arquivo. A classificação de seu impacto indireto completo continua [não verificado]. [app/chamados/services/fio.py:1515](app/chamados/services/fio.py:1515) [app/chamados/services/fio.py:1521](app/chamados/services/fio.py:1521) [app/chamados/services/fio.py:2109](app/chamados/services/fio.py:2109) [app/chamados/services/fio.py:2164](app/chamados/services/fio.py:2164)
- `explicador.py:43`: `dict.update` em resultado de apresentação, não `QuerySet.update` nem escrita de modelo. [app/chamados/services/explicador.py:41](app/chamados/services/explicador.py:41)
- `pdf_espelho.py:856`: o `getattr` apontado lê campos de rubricas dos turnos; não é, nesse ponto, leitura direta da projeção da pergunta. Outros caminhos indiretos até PDF permanecem [não verificado]. [app/relatorios/pdf_espelho.py:844](app/relatorios/pdf_espelho.py:844) [app/relatorios/pdf_espelho.py:855](app/relatorios/pdf_espelho.py:855)
- `core/reexibir.py:45` e `:49`: atribuições genéricas em memória; os chamadores encontrados são em `colaboradores/views.py`. Não demonstram, por si, escrita persistida da pergunta. [app/core/reexibir.py:21](app/core/reexibir.py:21) [app/core/reexibir.py:45](app/core/reexibir.py:45) [app/core/reexibir.py:49](app/core/reexibir.py:49) [app/colaboradores/views.py:1116](app/colaboradores/views.py:1116) [app/colaboradores/views.py:1281](app/colaboradores/views.py:1281)

**Não concluído:** não classifiquei individualmente todos os demais indeterminados e aliases da saída, nem demonstrei a lista integral de consumidores indiretos, SQL bruto ou caminhos de PDF. O item 3, portanto, não está fechado. [não verificado]

### 4. Contrato F1 antigo — STATUS: FECHADO quanto à implicação para a F7

A F7 deve censar os leitores efetivos desta cópia; não há demonstração de equivalência geral entre eles e o fold histórico descrito no anexo. Aqui, `respondida` delega ao juiz do par projetado, enquanto a projeção temporal e `estado` consideram outros campos. A ancestralidade e os bytes do objeto histórico informados no anexo permanecem [não verificado]. [app/chamados/models.py:3674](app/chamados/models.py:3674) [app/chamados/juizes.py:1544](app/chamados/juizes.py:1544) [app/chamados/juizes.py:1382](app/chamados/juizes.py:1382) [app/chamados/models.py:3152](app/chamados/models.py:3152)

### 5. Frota U1/U2 — STATUS: ABERTO

O anexo declara resultado de sombra com **U1 global = 29.931** e **15.118 validações projetadas sem linha**, com `rc_sombra=0`; não há acesso independente ao banco ou log nesta sessão. [não verificado]

O segundo contador usa correspondência de projeção por pergunta, ato e instante; ele não mede o último ato exigido para U2. Portanto, a sonda relatada fecha o resultado declarado para U1 e para o contador auxiliar, mas **U2 continua não medida**. [app/chamados/models.py:4122](app/chamados/models.py:4122) [app/chamados/models.py:4135](app/chamados/models.py:4135) [não verificado]

alcance nao verificado restante: 3

## 18/11 10:15 · sessao a2b0196b566b · - · - · origem=Ronald (tmux)

Evidências e limitações enviadas ao agente principal, incluindo os escritores de decisão, os leitores de REABERTURA, os importadores, os limites dos selos e a divergência entre o comentário sobre `ato_motivo` e o comportamento de `_pela_decisao` ([models.py:4075](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4075), [tarefa_do_dia.py:158](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:158)).

## 18/11 10:15 · sessao 7bb0d685aa36 · - · - · origem=Ronald (tmux)

Classificação dos candidatos dinâmicos relevantes enviada ao agente principal, com os sítios de leitura e escrita de R/V/C, os falsos positivos identificados e as lacunas que permanecem [nao verificado].

## 18/11 10:14 · sessao e888750e899c · - · - · origem=Ronald (tmux)

Rastreamento de templates, cascas dinâmicas e PDF enviado ao agente principal, com os respectivos arquivos, linhas e limites de verificação.

## 18/11 10:13 · sessao b997298c28f7 · RESPOSTAS-DE-LEI-1811-1010 · sonda · origem=Code

```text
=== SOMBRA (sombra_db, leitor) 18/11 10:12:49 ===
sonda_leitura: cen_F9_s1.py [3b68d969da5dc8d8] no cpuset 4-7, lendo sombra como leitor (LEITURA)

R_projecao_nula_com_ato_resposta	ultimo_fisico=reabertura	ultimo_efetivo=reabertura	total=52
R_projecao_nula_com_ato_resposta	ultimo_fisico=recusa	ultimo_efetivo=recusa	total=7
V_projecao_nula_com_ato_validacao	ultimo_fisico=devolucao	ultimo_efetivo=devolucao	total=60
V_projecao_nula_com_ato_validacao	ultimo_fisico=reabertura	ultimo_efetivo=reabertura	total=9
V_projecao_nula_com_ato_validacao	ultimo_fisico=resposta	ultimo_efetivo=resposta	total=3
rc_sombra=0
```
linhas copiadas: 9

## 18/11 10:12 · sessao a775ebdff421 · RESPOSTAS-DE-LEI-1811-1010 · sonda · origem=Code

```text
=== SOMBRA (sombra_db, leitor) 18/11 10:12:49 ===
sonda_leitura: cen_F7_s1.py [c5b054230a562d09] no cpuset 4-7, lendo sombra como leitor (LEITURA)

U1_carimbo_fora_de_identidade_global: 29931
validada_projetada_sem_linha: 15118
U2: NAO MEDIDA — porta real F1 indisponivel nesta copia
rc_sombra=0
```
linhas copiadas: 7

## 18/11 10:11 · sessao 514b43461e1f · RESPOSTAS-DE-LEI-1811-1010 · leitura · origem=Code

**Hoje, a partição já lê o juiz da L-130:** entre as perguntas vivas, fica pendente aquela cuja disputa está aberta e que não tem **instante e texto não vazio**; as demais ficam no histórico. Portanto, instante sem texto já é pendente hoje, desde que a disputa esteja aberta. [pergunta_no_app.py:76](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/pergunta_no_app.py:76), [juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544)

Depois da F5, o brief propõe ler a sequência de atos, conservando a projeção quando não houver decisão; o aval também determina que a ressuscitada, com o par zerado, apareça pendente. A definição completa para silêncio e absorção ainda é pergunta de lei do item 7; o número total que passará do histórico para pendente, portanto, está **[não verificado]**. [não verificado: anexos `brief.md` §3–4 e `respostas_de_lei_1811_1010.txt` itens 5 e 7]

A ressuscitação atual zera instante e texto sem gravar ato de devolução. A marca `da_ressuscitacao` existe, mas o código a descreve como inerte até o P2; assim, a sonda não consegue identificar todas as ressuscitadas pela sequência de atos. Uma ressuscitada cujo par já está zerado e cuja disputa está aberta **já é pendente hoje**, não uma passagem do histórico no pouso. [models.py:1567](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1567), [models.py:1619](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1619), [models.py:4065](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4065), [pergunta_no_app.py:81](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/pergunta_no_app.py:81)

A sonda abaixo chama as funções reais pedidas. Para a previsão, replica a aproximação do anexo: último ato por `ato_em, pk`, com RESPOSTA de texto vazio, RECUSA e REABERTURA como pendentes; mostra absorção rejeitada separadamente, pois sua aplicação à partição está por decidir. Ela não identifica o estoque inteiro de ressuscitadas nem substitui a medição pela função real da F5. [models.py:4081](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4081), [tarefa_do_dia.py:158](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:158), [não verificado: anexo `brief.md` §3 e §6]

```python
# Executar no shell Django da copia, com acesso de leitura ao banco.
# Nao grava nem registra atos.
from collections import Counter

from django.db.models import BooleanField, Case, OuterRef, Q, Subquery, Value, When
from django.utils import timezone

from chamados.juizes import pergunta_viva, q_nao_respondida, texto_vazio
from chamados.models import DecisaoPergunta as D, PerguntaDisputa as P

agora = timezone.now()
ultimo = D.objects.filter(pergunta_id=OuterRef("pk")).order_by("-ato_em", "-pk")
pendente_hoje = Q(disputa__fechada_em__isnull=True) & q_nao_respondida()

base = (
    P.objects.filter(P.q_vivas())
    .annotate(
        hoje_pendente=Case(
            When(pendente_hoje, then=Value(True)),
            default=Value(False),
            output_field=BooleanField(),
        ),
        ultimo_ato=Subquery(ultimo.values("ato")[:1]),
        ultimo_texto=Subquery(ultimo.values("ato_texto")[:1]),
        ultima_ausencia_status=Subquery(
            ultimo.values("absorcao_ausencia__status")[:1]
        ),
        ultima_marca_ressuscitacao=Subquery(
            ultimo.values("da_ressuscitacao")[:1]
        ),
    )
    .select_related("disputa", "celula")
    .order_by("pk")
)

cont = Counter()
prev = Counter()
indefinidos = Counter()

for p in base.iterator():
    if not pergunta_viva(p, agora=agora):
        continue

    cont["vivas"] += 1
    if p.hoje_pendente:
        cont["pendentes_hoje"] += 1
        continue

    cont["historico_hoje"] += 1

    # A disputa fechada permanece fora dos pendentes pela regra do app.
    if p.disputa.fechada_em is not None:
        cont["historico_disputa_fechada"] += 1
        continue

    cont["historico_disputa_aberta"] += 1
    ato = p.ultimo_ato

    # Categorias mutuamente exclusivas, apenas entre vivas que estao
    # hoje no historico e cuja disputa esta aberta.
    if ato == D.RESPOSTA and texto_vazio(p.ultimo_texto):
        prev["ultimo_ato_resposta_texto_vazio"] += 1
    elif ato == D.RECUSA:
        prev["ultimo_ato_recusa"] += 1
    elif ato == D.REABERTURA:
        if p.ultima_marca_ressuscitacao:
            prev["reabertura_marcada_ressuscitacao"] += 1
        else:
            prev["reabertura_sem_marca"] += 1
    elif ato == D.ABSORCAO and p.ultima_ausencia_status == "rejeitada":
        prev["absorcao_rejeitada_a_decidir"] += 1
    elif ato is None:
        # O brief conserva a projecao quando nao ha ato.
        cont["historico_sem_ato_conservado"] += 1
    elif ato == D.RESPOSTA:
        cont["historico_ultimo_ato_resposta_com_texto"] += 1
    else:
        # Nao atribuir uma regra de particao aos demais atos.
        chave = str(ato)
        if ato == D.ABSORCAO:
            chave += ":" + str(p.ultima_ausencia_status)
        indefinidos[chave] += 1

print("agora", timezone.localtime(agora).isoformat())
print("vivas", cont["vivas"])
print("pendentes hoje", cont["pendentes_hoje"])
print("historico hoje", cont["historico_hoje"])
print("historico hoje com disputa aberta", cont["historico_disputa_aberta"])

print("PREVISAO pela regra do brief, nao funcao do sistema:")
for motivo in (
    "ultimo_ato_resposta_texto_vazio",
    "ultimo_ato_recusa",
    "reabertura_marcada_ressuscitacao",
    "reabertura_sem_marca",
):
    print(" ", motivo, prev[motivo])

print(
    " subtotal dos motivos acima, sujeito a identificacao incompleta "
    "da ressuscitacao:",
    sum(prev[k] for k in (
        "ultimo_ato_resposta_texto_vazio",
        "ultimo_ato_recusa",
        "reabertura_marcada_ressuscitacao",
        "reabertura_sem_marca",
    )),
)
print(
    " absorcao rejeitada: PREVISAO condicional, particao a decidir:",
    prev["absorcao_rejeitada_a_decidir"],
)
print(" demais atos no historico aberto, sem previsao:", dict(indefinidos))
print(
    " ressuscitadas totais: NAO IDENTIFICAVEIS pela marca atual; "
    "nao somar uma categoria estimada ao subtotal"
)
```

`P.q_vivas()` é um pré-filtro; o script passa cada candidata pelo juiz real `pergunta_viva`. A face SQL `q_nao_respondida()` corresponde à negação do juiz usado pela partição atual. [models.py:3458](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3458), [juizes.py:1583](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1583)

Os números de produção e a execução da sonda estão **[não verificados]**: os dados fornecidos não incluem o resultado desta medição, e não há acesso ao banco. O subtotal impresso é uma aproximação sujeita à identificação incompleta da ressuscitação; não deve ser apresentado como o número definitivo de passagem no pouso. [não verificado: anexo `brief.md` §6] [nao verificado]

## 18/11 10:11 · sessao a70628e212cf · RESPOSTAS-DE-LEI-1811-1010 · censo · origem=Code

**Item 1 — ABERTO.** O inventário de escritores e leitores R/V/C e de `registrar` depende da execução e classificação da saída abaixo. A porta `registrar` aparece, por exemplo, em [models.py:1619](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1619). [não verificado: completude e classificação do inventário]

**Item 2 — ABERTO; medida registrada para E4d.** A sonda identifica perguntas cuja projeção atual de resposta ou validação está nula e que possuem, respectivamente, **algum ato histórico** de resposta ou validação. Esse é o rótulo da medida registrada para o universo solicitado “ato sem projeção”: **projeção atual nula com existência de ato histórico correspondente; não há função real verificada nesta cópia que estabeleça a correspondência ato → projeção**. A função identificada opera no sentido projeção → sequência: [models.py:4122](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4122).

| Medida da sombra | Último físico | Último efetivo | Total |
|---|---|---|---:|
| Projeção de resposta nula com ato histórico de resposta | reabertura | reabertura | 52 |
| Projeção de resposta nula com ato histórico de resposta | recusa | recusa | 7 |
| Projeção de validação nula com ato histórico de validação | devolução | devolução | 60 |
| Projeção de validação nula com ato histórico de validação | reabertura | reabertura | 9 |
| Projeção de validação nula com ato histórico de validação | resposta | resposta | 3 |

Os números são a saída raspada informada para `sombra_db`, com `rc_sombra=0`; não são uma contagem de produção verificada aqui. [não verificado: produção e eventual interseção entre os dois grupos] A reabertura pode limpar a projeção após registrar a fotografia, portanto a medida não classifica essas ocorrências como erros: [disputa_emissao.py:1577](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1577). Os imports e o selo global permanecem abertos até a análise do inventário; há exemplos em [relavrar_via_i6.py:47](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/relavrar_via_i6.py:47) e [test_contract_leitores_vitalidade.py:14](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_contract_leitores_vitalidade.py:14). [não verificado: exclusividade e classificação completa]

**Item 3 — ABERTO.** Os campos novos e a interpretação de REABERTURA têm pontos conhecidos em [models.py:4071](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4071) e [tarefa_do_dia.py:163](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:163); os demais caminhos dependem da saída e de sua análise. [não verificado: completude]

**Item 4 — ABERTO.** O grafo das duas cascas depende da saída abaixo; existem includes nas duas bases em [base.html:393](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/base.html:393) e [base_app.html:145](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/base_app.html:145). [não verificado: caminhos transitivos e consumidores completos]

Execute o script abaixo **de dentro de `app/`**. Ele imprime ocorrências estáticas e candidatos com arquivo, linha e papel; também sinaliza limites que exigem inspeção. Os papéis são classificações sintáticas, e a saída precisa ser examinada na volta 3: nomes montados em tempo de execução, atribuição de tipos e includes dinâmicos não são resolvidos integralmente por AST. [não verificado: saída e identidade do HEAD]

```python
import ast
import os
import re

CAMPOS = {
    "respondida_em", "resposta_colab", "validada_em", "validada_por",
    "materializada_em", "resposta_mantem_validacao", "REABERTURA",
}
SIMBOLOS = CAMPOS | {"registrar", "DecisaoPergunta"}
ALVOS_IMPORT = {
    "relavrar_via_i6", "dedup_perguntas_fato", "worklist",
    "curar_perguntas_em_limbo",
    "auditar_invariantes_chamados", "backfill_decisoes_pergunta",
    "reconciliar_fantasmas",
    "universo_i6", "aplicar", "CONTADORES_DO_APPLY",
    "_parse_pendentes", "MODULOS_ALVO",
}
ARQUIVOS_IMPORT = {
    "relavrar_via_i6.py", "dedup_perguntas_fato.py",
    "worklist.py", "curar_perguntas_em_limbo.py",
}
BASES = ("base.html", "base_app.html")
SKIP = {".git", "__pycache__", ".venv", "venv", "node_modules"}
encontrados = []


def emitir(path, linha, papel, detalhe):
    detalhe = re.sub(r"\s+", " ", str(detalhe)).strip()
    encontrados.append((path.replace(os.sep, "/"), linha, papel, detalhe))


def arquivos():
    for raiz, dirs, nomes in os.walk("."):
        dirs[:] = sorted(d for d in dirs if d not in SKIP)
        for nome in sorted(nomes):
            if nome.endswith((".py", ".html")):
                yield os.path.normpath(os.path.join(raiz, nome))


def fonte_no(no):
    try:
        return ast.unparse(no)
    except Exception:
        return type(no).__name__


def nomes_campo(texto):
    # Aceita também chaves ORM como validada_em__isnull.
    return sorted(
        c for c in SIMBOLOS
        if re.search(r"(?<![A-Za-z0-9_])" + re.escape(c)
                     + r"(?=$|[^A-Za-z0-9_]|__)", texto)
    )


def modulo_atual(path):
    partes = path[:-3].split(os.sep)
    if partes[-1] == "__init__":
        partes.pop()
    return ".".join(partes)


def resolver_import(path, no):
    modulo = no.module or ""
    if not no.level:
        return modulo
    partes = modulo_atual(path).split(".")
    if not path.endswith(os.sep + "__init__.py"):
        partes = partes[:-1]
    sobe = no.level - 1
    if sobe:
        partes = partes[:-sobe] if sobe <= len(partes) else []
    return ".".join(partes + ([modulo] if modulo else []))


class Inventario(ast.NodeVisitor):
    def __init__(self, path):
        self.path = path
        self.escopo = []
        self.pais = []

    def contexto(self):
        return ".".join(self.escopo) or "<modulo>"

    def visit(self, no):
        self.pais.append(no)
        try:
            return super().visit(no)
        finally:
            self.pais.pop()

    def em_escopo(self, no):
        self.escopo.append(no.name)
        self.generic_visit(no)
        self.escopo.pop()

    visit_FunctionDef = em_escopo
    visit_AsyncFunctionDef = em_escopo
    visit_ClassDef = em_escopo

    def visit_Import(self, no):
        for alias in no.names:
            relevante = (
                os.path.basename(self.path) in ARQUIVOS_IMPORT
                or any(x in alias.name.split(".") for x in ALVOS_IMPORT)
            )
            if relevante:
                emitir(self.path, no.lineno, "IMPORT",
                       "%s; import %s; alias=%s" %
                       (self.contexto(), alias.name, alias.asname or "-"))
        self.generic_visit(no)

    def visit_ImportFrom(self, no):
        resolvido = resolver_import(self.path, no)
        for alias in no.names:
            relevante = (
                os.path.basename(self.path) in ARQUIVOS_IMPORT
                or any(x in resolvido.split(".") for x in ALVOS_IMPORT)
                or alias.name in ALVOS_IMPORT
                or alias.name == "*"
            )
            if relevante:
                emitir(self.path, no.lineno, "IMPORT_FROM",
                       "%s; escrito=%s%s; resolvido=%s; nome=%s; alias=%s" %
                       (self.contexto(), "." * no.level, no.module or "",
                        resolvido, alias.name, alias.asname or "-"))
        self.generic_visit(no)

    def visit_Attribute(self, no):
        if no.attr in SIMBOLOS:
            papel = ("ESCRITA_ATTRIBUTE" if isinstance(no.ctx, ast.Store)
                     else "EXCLUSAO_ATTRIBUTE" if isinstance(no.ctx, ast.Del)
                     else "LEITURA_ATTRIBUTE")
            if no.attr == "registrar":
                papel = "REGISTRAR_CANDIDATO"
            emitir(self.path, no.lineno, papel,
                   "%s; %s" % (self.contexto(), fonte_no(no)))
        self.generic_visit(no)

    def visit_Name(self, no):
        if no.id in SIMBOLOS:
            emitir(self.path, no.lineno, "NOME_CANDIDATO",
                   "%s; %s; contexto=%s" %
                   (self.contexto(), no.id, type(no.ctx).__name__))
        self.generic_visit(no)

    def visit_Constant(self, no):
        if isinstance(no.value, str):
            campos = nomes_campo(no.value)
            if campos:
                emitir(self.path, no.lineno, "STRING_CANDIDATA",
                       "%s; campos=%s; valor=%r" %
                       (self.contexto(), ",".join(campos), no.value))
        self.generic_visit(no)

    def visit_Call(self, no):
        func = no.func
        nome = func.attr if isinstance(func, ast.Attribute) else (
            func.id if isinstance(func, ast.Name) else "")
        if nome in {"getattr", "setattr", "delattr", "hasattr"}:
            if len(no.args) >= 2:
                chave = no.args[1]
                if isinstance(chave, ast.Constant) and isinstance(chave.value, str):
                    if nomes_campo(chave.value):
                        emitir(self.path, no.lineno, nome.upper(),
                               "%s; %s" % (self.contexto(), fonte_no(no)))
                else:
                    emitir(self.path, no.lineno, "ACESSO_DINAMICO_NAO_RESOLVIDO",
                           "%s; %s" % (self.contexto(), fonte_no(no)))
        if nome in {"update", "filter", "exclude", "get", "values",
                    "values_list", "create", "get_or_create",
                    "update_or_create", "order_by", "only", "defer"}:
            for kw in no.keywords:
                if kw.arg is None:
                    emitir(self.path, no.lineno, "KWARGS_DINAMICOS_NAO_RESOLVIDOS",
                           "%s; %s" % (self.contexto(), fonte_no(no)))
                elif nomes_campo(kw.arg):
                    papel = ("ORM_ESCRITA_CANDIDATA" if nome in
                             {"update", "create", "get_or_create",
                              "update_or_create"} else "ORM_LEITURA_CANDIDATA")
                    emitir(self.path, no.lineno, papel,
                           "%s; chamada=%s; chave=%s" %
                           (self.contexto(), nome, kw.arg))
            for arg in no.args:
                if isinstance(arg, ast.Starred):
                    emitir(self.path, no.lineno, "ARGS_DINAMICOS_NAO_RESOLVIDOS",
                           "%s; %s" % (self.contexto(), fonte_no(no)))
        if nome == "registrar":
            emitir(self.path, no.lineno, "CHAMADA_REGISTRAR_CANDIDATA",
                   "%s; %s" % (self.contexto(), fonte_no(no)))
        self.generic_visit(no)


templates = {}
arestas = []
exibem = set()
tag_re = re.compile(r"({{.*?}}|{%.*?%})", re.S)
diretiva_re = re.compile(r"^{%\s*(include|extends)\s+(.+?)\s*%}$", re.S)
literal_re = re.compile(r"""^\s*(['"])(.*?)\1(?:\s|$)""", re.S)

for path in arquivos():
    path = path.removeprefix("./")
    try:
        with open(path, encoding="utf-8") as f:
            conteudo = f.read()
    except (OSError, UnicodeError) as exc:
        emitir(path, 1, "ERRO_LEITURA", exc)
        continue

    if path.endswith(".py"):
        try:
            arvore = ast.parse(conteudo, filename=path)
        except (SyntaxError, ValueError) as exc:
            emitir(path, getattr(exc, "lineno", 1) or 1, "ERRO_AST", exc)
            continue
        Inventario(path).visit(arvore)
        continue

    # Indexa todos os diretórios chamados templates, inclusive os de apps.
    partes = path.split(os.sep)
    if "templates" in partes:
        i = len(partes) - 1 - partes[::-1].index("templates")
        nome_template = "/".join(partes[i + 1:])
        templates.setdefault(nome_template, []).append(path)
    else:
        nome_template = path.replace(os.sep, "/")

    for m in tag_re.finditer(conteudo):
        tag = m.group(0)
        linha = conteudo.count("\n", 0, m.start()) + 1
        campos = nomes_campo(tag)
        if campos:
            emitir(path, linha, "TEMPLATE_LEITURA_CANDIDATA",
                   "campos=%s; %s" % (",".join(campos), tag))
        if re.search(r"(?i)pergunta", tag):
            exibem.add(nome_template)
            emitir(path, linha, "TEMPLATE_PERGUNTA_CANDIDATO", tag)
        d = diretiva_re.match(tag)
        if d:
            tipo, expressao = d.groups()
            literal = literal_re.match(expressao)
            if literal:
                destino = literal.group(2)
                arestas.append((nome_template, destino, path, linha, tipo))
            else:
                emitir(path, linha, "TEMPLATE_DESTINO_DINAMICO_NAO_RESOLVIDO",
                       "%s; %s" % (tipo, expressao))

print("=== OCORRENCIAS PYTHON, IMPORTS E TEMPLATES ===")
for path, linha, papel, detalhe in sorted(encontrados):
    print("%s:%s | %s | %s" % (path, linha, papel, detalhe))

print("=== GRAFO DE INCLUDES E EXTENDS A PARTIR DAS BASES ===")
# Inclui tanto includes saindo da base quanto filhos que herdam dela.
alcancados = set(BASES)
mudou = True
while mudou:
    mudou = False
    for origem, destino, path, linha, tipo in arestas:
        novo = None
        if tipo == "include" and origem in alcancados:
            novo = destino
        elif tipo == "extends" and destino in alcancados:
            novo = origem
        if novo is not None and novo not in alcancados:
            alcancados.add(novo)
            mudou = True

for origem, destino, path, linha, tipo in sorted(arestas):
    if (tipo == "include" and origem in alcancados) or (
        tipo == "extends" and destino in alcancados
    ):
        status = "resolvido" if destino in templates else "DESTINO_NAO_ENCONTRADO"
        if len(templates.get(destino, [])) > 1:
            status = "DESTINO_AMBIGUO"
        print("%s:%s | %s | %s -> %s | %s | arquivos=%s" %
              (path, linha, tipo, origem, destino, status,
               ",".join(templates.get(destino, [])) or "-"))

print("=== TEMPLATES ALCANCADOS COM PERGUNTA CANDIDATA ===")
for nome in sorted(alcancados & exibem):
    print("%s | arquivos=%s" %
          (nome, ",".join(templates.get(nome, [])) or "NAO_ENCONTRADO"))

print("=== LIMITES ===")
print("Classificacoes sao sintaticas e incluem candidatos e falsos positivos.")
print("Inspecionar erros, aliases, chamadas indiretas e acessos dinamicos.")
print("Destinos dinamicos, templates gerados e exibicao sem nome 'pergunta' "
      "exigem analise adicional.")
```

alcance nao verificado restante: 4

## 18/11 10:11 · sessao d7a04e67cf1b · RESPOSTAS-DE-LEI-1811-1010 · censo · origem=Code

**4. Contrato F1 antigo — STATUS: FECHADO quanto à implicação para a F7.** O anexo apresenta uma leitura histórica que ordenava decisões por `(ato_em, pk)` e fazia o fold dos atos; o dado fornecido informa que `be2b8d733` não é ancestral do HEAD. A F7 deve, portanto, censar os leitores do HEAD, sem presumir que esse contrato histórico esteja presente ou seja executado hoje. [não verificado]

No HEAD, a propriedade `respondida` delega a `pergunta_respondida`, que lê o par projetado `respondida_em` e `resposta_colab`; a projeção temporal lê também validação e materialização, e `estado` dá precedência a `validada_em`. A leitura histórica do último ato, do motivo de recusa e dos demais resultados do fold não tem equivalência geral demonstrada por esses leitores: o inventário do item 3 continua necessário. [models.py:3674](app/chamados/models.py:3674) [juizes.py:1382](app/chamados/juizes.py:1382) [juizes.py:1544](app/chamados/juizes.py:1544) [models.py:3152](app/chamados/models.py:3152)

**5. Frota U1/U2 — STATUS: FECHADO para o resultado da sonda na sombra; U2 não medida.** A saída fornecida informa **U1 global = 29.931** e **validação projetada sem linha = 15.118**, com `rc_sombra=0`. São resultados declarados da sombra, não uma leitura independente nesta sessão. [não verificado]

O segundo número conta ausência de correspondência da validação projetada por pergunta, ato e instante; não determina qual é o último ato. **U2 permanece não medida**, pois a saída declara indisponível a porta real F1. [models.py:4122](app/chamados/models.py:4122) [não verificado]

**Itens 1 e 3 — STATUS: ABERTOS até a execução e revisão da saída.** O script abaixo inventaria as formas estáticas pedidas, incluindo testes e migrações, e imprime incertezas. Atribuição de casca, papel de escrita e resolução de nomes dinâmicos exigem revisão quando a sintaxe não os determina; a execução e a completude semântica permanecem **[não verificado]**. [nao verificado]

Execute a partir de `app/`, sem Django:

```python
import ast
import os
import re

CAMPOS = {
    "validada_em", "validada_por", "respondida_em",
    "resposta_colab", "materializada_em",
}
FUNCOES = {
    "pergunta_respondida", "respondida_sem_texto", "texto_sem_carimbo",
    "projecao_temporal_da_pergunta", "linhas_da_projecao_sem_linha",
    "pergunta_viva", "_pergunta_viva_calc",
}
METODOS = {"get", "values", "values_list", "filter"}
ESCRITORES = {"update", "create", "get_or_create", "update_or_create"}
BASES = {"base.html", "base_app.html"}
CARD = "chamados/partials/_pergunta_card.html"

def arquivos():
    for raiz, dirs, nomes in os.walk("."):
        dirs[:] = sorted(d for d in dirs if d not in {
            ".git", "__pycache__", ".venv", "venv", "node_modules",
        })
        for nome in sorted(nomes):
            yield os.path.join(raiz, nome)

def local(caminho, linha):
    return "app/%s:%d" % (caminho.removeprefix("./"), linha)

def campo_em_nome(nome):
    # Inclui relações e sufixos ORM: pergunta__validada_em__isnull.
    partes = nome.split("__")
    return sorted(
        c for c in CAMPOS
        if any(p == c or p == c + "_id" for p in partes)
    )

def nome_chamada(no):
    if isinstance(no, ast.Name):
        return no.id
    if isinstance(no, ast.Attribute):
        return no.attr
    return ""

def trecho(texto, no):
    s = ast.get_source_segment(texto, no) or "?"
    return " ".join(s.split())[:240]

todos = list(arquivos())
templates = {}
for caminho in todos:
    if not caminho.endswith((".html", ".htm", ".txt")):
        continue
    # Permite templates em app/templates e em diretórios templates de apps.
    normal = caminho.removeprefix("./").replace(os.sep, "/")
    marcador = "templates/"
    if marcador not in normal:
        continue
    nome = normal.split(marcador, 1)[1]
    try:
        with open(caminho, encoding="utf-8") as f:
            texto = f.read()
    except (OSError, UnicodeError) as exc:
        print("ERRO template", caminho, repr(exc))
        continue
    templates.setdefault(nome, []).append((caminho, texto))

# Não elimina arquivos com o mesmo nome de template: a precedência do loader
# é uma questão de configuração, e ambas as definições são inventariadas.
arestas = []
tags = re.compile(r"{%\s*(include|extends)\s+(.*?)%}", re.S)
literal = re.compile(r"""^\s*(['"])(.*?)\1""", re.S)

for nome, versoes in sorted(templates.items()):
    for caminho, texto in versoes:
        for m in tags.finditer(texto):
            linha = texto.count("\n", 0, m.start()) + 1
            tipo, argumento = m.group(1), m.group(2).strip()
            achou = literal.match(argumento)
            destino = achou.group(2) if achou else None
            arestas.append((nome, destino, tipo, caminho, linha, argumento))

# Propaga cascas por herança e por inclusão. A atribuição é conservadora:
# um fragmento reutilizado pode receber várias cascas.
cascas = {nome: set() for nome in templates}
for base in BASES:
    cascas.setdefault(base, set()).add(base)

for origem, destino, tipo, caminho, linha, argumento in arestas:
    if tipo == "extends" and destino is None and re.search(
        r"\bbase_tpl\b", argumento
    ):
        cascas.setdefault(origem, set()).update(BASES)

mudou = True
while mudou:
    mudou = False
    for origem, destino, tipo, caminho, linha, argumento in arestas:
        if destino is None:
            continue
        cascas.setdefault(origem, set())
        cascas.setdefault(destino, set())
        antes = len(cascas[origem] if tipo == "extends" else cascas[destino])
        if tipo == "extends":
            cascas[origem].update(cascas[destino])
            depois = len(cascas[origem])
        else:
            cascas[destino].update(cascas[origem])
            depois = len(cascas[destino])
        mudou |= antes != depois

# Inclui recursivamente todos os que levam ao card; depois segue todas as
# arestas de include/extends a partir deles e das duas bases.
sementes = set(BASES) | {CARD}
mudou = True
while mudou:
    mudou = False
    for origem, destino, tipo, caminho, linha, argumento in arestas:
        if tipo == "include" and destino in sementes and origem not in sementes:
            sementes.add(origem)
            mudou = True

alcance = set(sementes)
mudou = True
while mudou:
    mudou = False
    for origem, destino, tipo, caminho, linha, argumento in arestas:
        if origem in alcance and destino and destino not in alcance:
            alcance.add(destino)
            mudou = True

print("=== GRAFO TRANSITIVO: includes, extends e cascas possíveis ===")
for nome in sorted(alcance):
    print("TEMPLATE", nome, "casca=",
          ",".join(sorted(cascas.get(nome, set()))) or "indeterminada")
    if nome not in templates:
        print("  AUSENTE OU CARREGADOR EXTERNO [nao verificado]")
    elif len(templates[nome]) > 1:
        print("  MULTIPLAS DEFINICOES: precedencia [nao verificado]")
    for origem, destino, tipo, caminho, linha, argumento in arestas:
        if origem != nome:
            continue
        print(" ", local(caminho, linha), tipo,
              destino if destino else "DINAMICO: " + argumento,
              "" if destino else "[nao verificado]")

print("=== ACESSOS EM TEMPLATES: leitura sintatica ===")
expressao = re.compile(r"{{(.*?)}}|{%(.*?)%}", re.S)
acesso = re.compile(
    r"\b[A-Za-z_]\w*(?:\.[A-Za-z_]\w*)*"
    r"\.(validada_em|validada_por|respondida_em|resposta_colab|materializada_em)"
    r"(?:_id)?\b"
)
for nome, versoes in sorted(templates.items()):
    for caminho, texto in versoes:
        for m in expressao.finditer(texto):
            conteudo = m.group(1) if m.group(1) is not None else m.group(2)
            for achou in acesso.finditer(conteudo):
                pos = m.start() + 2 + achou.start()
                linha = texto.count("\n", 0, pos) + 1
                print(local(caminho, linha), "leitura", nome,
                      achou.group(0),
                      "casca=" + (",".join(sorted(cascas.get(nome, set())))
                                  or "indeterminada"))

print("=== PYTHON: acessos e referencias estaticas ===")
for caminho in sorted(p for p in todos if p.endswith(".py")):
    try:
        with open(caminho, encoding="utf-8") as f:
            texto = f.read()
        arvore = ast.parse(texto, filename=caminho)
    except (OSError, UnicodeError, SyntaxError) as exc:
        print("ERRO Python", caminho, repr(exc), "[nao verificado]")
        continue

    pais = {}
    for pai in ast.walk(arvore):
        for filho in ast.iter_child_nodes(pai):
            pais[filho] = pai

    # Registra aliases explícitos; referências a nomes coincidentes são
    # candidatas, pois AST isolada não resolve tipos nem importações indiretas.
    aliases = {nome: nome for nome in FUNCOES}
    for no in ast.walk(arvore):
        if isinstance(no, ast.ImportFrom):
            for item in no.names:
                if item.name in FUNCOES:
                    aliases[item.asname or item.name] = item.name

    saidas = set()

    def emitir(no, papel, categoria, detalhe):
        saidas.add((no.lineno, no.col_offset, papel, categoria, detalhe))

    for no in ast.walk(arvore):
        if isinstance(no, ast.Attribute):
            campos = campo_em_nome(no.attr)
            if campos:
                papel = ("escrita" if isinstance(no.ctx, (ast.Store, ast.Del))
                         else "leitura")
                emitir(no, papel, "atributo " + ",".join(campos),
                       trecho(texto, no))
            if no.attr in FUNCOES:
                emitir(no, "referencia", "funcao " + no.attr,
                       trecho(texto, no))

        if isinstance(no, ast.Name) and no.id in aliases:
            emitir(no, "referencia", "funcao " + aliases[no.id],
                   trecho(texto, no))

        if isinstance(no, (ast.FunctionDef, ast.AsyncFunctionDef)):
            if no.name in FUNCOES:
                emitir(no, "definicao", "funcao " + no.name, no.name)

        if isinstance(no, ast.ImportFrom):
            for item in no.names:
                if item.name in FUNCOES:
                    emitir(no, "importacao", "funcao " + item.name,
                           trecho(texto, no))

        if not isinstance(no, ast.Call):
            continue
        metodo = nome_chamada(no.func)

        if metodo in {"getattr", "setattr", "hasattr", "delattr"}:
            if len(no.args) >= 2:
                chave = no.args[1]
                if isinstance(chave, ast.Constant) and isinstance(chave.value, str):
                    campos = campo_em_nome(chave.value)
                    if campos:
                        papel = ("escrita" if metodo in {"setattr", "delattr"}
                                 else "leitura")
                        emitir(no, papel, metodo + " " + ",".join(campos),
                               trecho(texto, no))
                else:
                    emitir(no, "indeterminado", metodo + " nome dinamico",
                           trecho(texto, no))

        if metodo in METODOS | ESCRITORES | {"Q"}:
            papel = ("escrita possivel" if metodo in ESCRITORES
                     else "leitura ou consulta")
            for argumento in no.args:
                if isinstance(argumento, ast.Constant) and isinstance(argumento.value, str):
                    campos = campo_em_nome(argumento.value)
                    if campos:
                        emitir(no, papel, metodo + " " + ",".join(campos),
                               trecho(texto, no))
                elif isinstance(argumento, ast.Starred):
                    emitir(no, "indeterminado", metodo + " argumentos dinamicos",
                           trecho(texto, no))
            for kw in no.keywords:
                if kw.arg is None:
                    emitir(no, "indeterminado", metodo + " keywords dinamicos",
                           trecho(texto, no))
                else:
                    campos = campo_em_nome(kw.arg)
                    if campos:
                        emitir(no, papel, metodo + " " + ",".join(campos),
                               trecho(texto, no))

    for linha, coluna, papel, categoria, detalhe in sorted(saidas):
        print(local(caminho, linha), papel, categoria, "|", detalhe)

print("=== LIMITES ===")
print("Nomes dinamicos, aliases indiretos, SQL bruto, dicionarios,")
print("templates carregados fora da arvore e fluxo em tempo de execucao:")
print("[nao verificado]; referencias homonimas e cascas sao candidatas.")
```

alcance nao verificado restante: 2

## 18/11 10:08 · sessao fced45f6860a · RESPOSTAS-DE-LEI-1811-1010 · sonda · origem=Code

```text
=== SOMBRA (sombra_db, leitor) 18/11 10:06:59 ===
sonda_leitura: i7_sonda.py [a39a32993596c041] no cpuset 4-7, lendo sombra como leitor (LEITURA)

agora 2026-11-18T10:07:00.928492-03:00 prefiltro 14975 vivas 11381
vivas por ultimo_ato {None: 10626, 'resposta': 399, 'silencio': 15, 'reabertura': 275, 'recusa': 6, 'devolucao': 60}
vivas pendentes_hoje 10092 historico_hoje 1289
Q1 RESPOSTA texto_nao_vazio pendente_hoje->historico 0
Q2 RESPOSTA texto_vazio historico_hoje->pendente 0
Q3 RECUSA historico_hoje->pendente 0
Q4 REABERTURA historico_hoje->pendente 29
Q5 SILENCIO pendente_hoje->historico 11
Q6 ABSORCAO rejeitada historico_hoje->pendente 0
SEM_ATO vivas sem DP 10626
rc_sombra=0
```
linhas copiadas: 14

## 18/11 10:06 · sessao a72dc2fb3002 · [nome]-NAO-DESFAZ-O-PAPEL · verificador · origem=Code

1. **[ACHADO] A função genérica aceita uma grade cuja régua não está alinhada às células.** `contar_espurias` usa `grade['marcos']` por índice para interpretar `dia['celulas']` ([orfa_classe.py:165](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:165), [orfa_classe.py:194](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:194)). Já `grade_da_celula` devolve régua de união no topo e marcos próprios por dia; o remapeamento é uma operação separada ([leitor_celula.py:281](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/services/leitor_celula.py:281), [leitor_celula.py:413](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/services/leitor_celula.py:413), [leitor_celula.py:445](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/services/leitor_celula.py:445)). Nessa entrada, um `missing` pode ser associado à hora de outro marco e alterar o recorte. A entrada `contar_espurias_do_colab` usa o builder, não esse leitor ([orfa_classe.py:233](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:233)).

2. **[ACHADO] O campo `orfas` perde o produtor ao passar pela ata.** O builder emite `Orfa`, uma string com atributo `produtor` ([orfa_classe.py:125](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:125), [utils.py:631](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:631)); o cartório grava apenas `str(h)` e o leitor devolve strings ([cartorio.py:742](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/services/cartorio.py:742), [leitor_celula.py:180](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/services/leitor_celula.py:180)). Se essa grade for passada ao contador genérico, as órfãs com hora passam a `sem_produtor`, nunca P1, e não provocam pergunta ao dono ([orfa_classe.py:175](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:175), [orfa_classe.py:202](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:202)). O caminho da entrada por colaborador recompõe a grade e preserva o atributo em memória; uso do contador sobre ata em produção: **[não verificado]** ([orfa_classe.py:233](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:233)).

3. **Insumos do juiz: equivalentes sob os mesmos dados, com diferença de execução.** A entrada passa `ini`, `fim`, `hoje` e `agora` ao mesmo `InsumosDoDono`; o `handle` também passa um `espelho` já calculado ([orfa_classe.py:229](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:229), [e6_oraculo.py:296](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/management/commands/e6_oraculo.py:296), [e6_oraculo.py:328](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/management/commands/e6_oraculo.py:328)). Sem espelho, `dono` primeiro pergunta com CXR falso e só calcula o espelho se a resposta preliminar for ESTRUTURA; CADASTRO e BATIDA independem dessa lista pela ordem do juiz ([e6_oraculo.py:98](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/management/commands/e6_oraculo.py:98), [e6_oraculo.py:162](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/management/commands/e6_oraculo.py:162), [e6_oraculo.py:182](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/management/commands/e6_oraculo.py:182)). Portanto, com a mesma fotografia dos dados e espelho bem-sucedido, a omissão não muda a resposta; o `handle` pula o colaborador se o espelho falha, enquanto a entrada pode responder CADASTRO/BATIDA sem pedir o espelho e pode propagar falha quando o pede ([e6_oraculo.py:315](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/management/commands/e6_oraculo.py:315), [orfa_classe.py:235](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:235)). Igualdade das fotografias em produção: **[não verificado]**.

4. **Data perguntada: nos caminhos do builder examinados, corresponde à linha.** `grade_espelho_janela` passa `ref=fim` e o clip esvazia linhas sem removê-las ([utils.py:2346](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:2346), [utils.py:968](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:968)). Tanto o builder por turno quanto o legacy montam cada linha com `ref - i`; o legacy remapeia os segmentos por data antes de emitir a lista ([utils.py:1784](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:1784), [utils.py:1027](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:1027), [utils.py:1040](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:1040)). Não encontrei nesses caminhos uma linha em que a data seja diferente de `fim - i`; o contador genérico, porém, confia no `ref_fim` fornecido e não valida a grade ([orfa_classe.py:179](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:179)).

5. **Significados e unidades exigem leitura cuidadosa.** P1 nasce no casador; P2 é sobra atribuída à data civil; no builder por turno, órfãs do casamento ficam na data de entrada do turno e sobras P3 são atribuídas à data civil ([utils.py:631](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:631), [utils.py:801](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:801), [utils.py:1445](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:1445), [utils.py:1824](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:1824)). O contador lê `missing` e a hora da coluna, lê a classe de `classificar_orfas` e o produtor do item; grava dono **por dia P1**, mas repete esse dono também nos itens P2/P3 do mesmo dia ([orfa_classe.py:165](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:165), [orfa_classe.py:200](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:200)). Assim, `por_dono` e `dias_p1` contam dias, enquanto `p1` e `por_produtor` contam ocorrências; o dono em `itens` é do **dia**, não uma decisão sobre cada órfã ([orfa_classe.py:203](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:203)). Leitor externo desses novos campos: **[não verificado]**.

6. **Raio: a constante deste módulo seleciona o universo e, por isso, afeta o alarma.** Ela é lida em `espurias_do_dia`; a seleção determina quais dias chegam ao juiz e, consequentemente, pode mudar `por_dono` e `alarma`, sem mudar a resposta do juiz para um mesmo dia nem o corpo de `classificar_orfas` ([orfa_classe.py:120](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:120), [orfa_classe.py:171](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:171), [orfa_classe.py:202](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:202)). Há outra constante **homônima**, independente, em `regua_defesa`, usada por decisões fora do contador; ela não é a constante de `orfa_classe` ([regua_defesa.py:392](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/regua_defesa.py:392), [motor_calculo_v2.py:2076](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/motor_calculo_v2.py:2076)).

7. **Testes: os quatro mutantes perguntados são cobertos por asserções, com limite de integração.** Os testes exigem CADASTRO sem alarma, ESTRUTURA com alarma e BATIDA contado sem alarma ([test_contador_espurias.py:133](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/tests/test_contador_espurias.py:133)). O teste da entrada substitui `InsumosDoDono` por mock e exige sua construção e chamada de `.dono(fim)`, portanto morde uma entrada que abandone essa chamada ([test_contador_espurias.py:275](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/tests/test_contador_espurias.py:275)). Ele não exercita o juiz real, a construção real da grade ou a igualdade entre espelhos; a data é testada numa grade fabricada ([test_contador_espurias.py:152](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/tests/test_contador_espurias.py:152), [test_contador_espurias.py:278](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/tests/test_contador_espurias.py:278)). Execução dos testes: **[não verificado]**.

8. **Checklist do delta fornecido:** a entrada delega ao juiz existente; não encontrei segundo juiz de dono no trecho examinado ([orfa_classe.py:227](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:227), [e6_oraculo.py:195](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/management/commands/e6_oraculo.py:195)). A entrada recebe `agora` e deriva `hoje` desse instante, sem ler outro relógio nela própria ([orfa_classe.py:218](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:218)). O retorno é montado em memória; segundo escritor persistente pelo trecho examinado: não identificado ([orfa_classe.py:208](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:208)). Permanecem os riscos de representação e perda de proveniência dos achados 1–2 e a mistura de dia do turno/data civil do achado 5. Comparação independente dos commits, integridade integral do delta e resultado em produção: **[não verificado]**.

## 18/11 10:06 · sessao af4df093d524 · RESPOSTAS-DE-LEI-1811-1010 · censo · origem=Code

A seção B contém **cinco itens numerados**, dos quais quatro estavam pendentes: 1, 3, 4 e 5. O item 2 já estava fechado no anexo e sua conclusão sobre a chamada específica foi confirmada no corpo de `reabrir_para_correcao`. [app/chamados/services/disputa_emissao.py:1577](app/chamados/services/disputa_emissao.py:1577)

Não consegui fechar os quatro pendentes com a evidência disponível. Em particular, a cópia não tem metadados Git acessíveis para conferir o HEAD ou ler `be2b8d733` **[não verificado]**, e a busca nesta árvore não encontrou a porta histórica `atos_pergunta.leitura` **[não verificado]**. Abaixo separo o que foi comprovado do que ainda falta.

**Método.** Usei busca literal com `rg` para campos, nomes de funções, templates e includes; examinei os corpos relevantes; e percorri com AST os arquivos Python de `app`, excluindo testes e migrações **[não verificado: classificação integral]**. A AST procurou atributos, imports de `atos_pergunta`, inclusive aliases, e nomes literais em `getattr`/`setattr`/`hasattr`; encontrou também **97 chamadas com nome dinâmico ainda não resolvido**, portanto não sustenta uma lista completa de leitores e escritores **[não verificado]**. O alias de renderização do fragmento foi identificado em [app/chamados/views.py:397](app/chamados/views.py:397). O inventário de 407 ocorrências literais Python inclui comentários e não equivale a 407 leitores **[não verificado]**.

### 1. Cascas do card — STATUS: ABERTO

O caminho estático confirmado é:

- `meu_atendimento` escolhe a casca por `base_tpl` e inclui o card nos dois loops: [meu_atendimento.html:1](app/templates/chamados/meu_atendimento.html:1), [meu_atendimento.html:127](app/templates/chamados/meu_atendimento.html:127), [meu_atendimento.html:144](app/templates/chamados/meu_atendimento.html:144); a escolha entre `base_app.html` e `base.html` está em [context_processors.py:210](app/core/context_processors.py:210).
- A página de detalhe usa `base.html` e inclui o card de pergunta em [detalhe_local.html:1](app/templates/chamados/detalhe_local.html:1) e [detalhe_local.html:310](app/templates/chamados/detalhe_local.html:310). O fragmento é renderizado pelo alias `_render_card` em [views.py:397](app/chamados/views.py:397) e [views.py:404](app/chamados/views.py:404).
- O modal é renderizado em [views.py:927](app/chamados/views.py:927), [views.py:1400](app/chamados/views.py:1400) e [fio.py:543](app/chamados/services/fio.py:543). O painel abre a prévia de lote pelo drawer em [painel_gestao.html:14](app/templates/chamados/painel_gestao.html:14); os renders da prévia estão em [views_cobrar.py:360](app/chamados/views_cobrar.py:360), [views_cobrar.py:364](app/chamados/views_cobrar.py:364), [views_cobrar.py:380](app/chamados/views_cobrar.py:380) e [views_cobrar.py:388](app/chamados/views_cobrar.py:388).
- Includes diretos identificados em `base.html`: [base.html:92](app/templates/base.html:92), [base.html:93](app/templates/base.html:93), [base.html:290](app/templates/base.html:290), [base.html:388](app/templates/base.html:388), [base.html:393](app/templates/base.html:393), [base.html:399](app/templates/base.html:399), [base.html:432](app/templates/base.html:432). Em `base_app.html`: [base_app.html:85](app/templates/base_app.html:85), [base_app.html:145](app/templates/base_app.html:145), [base_app.html:154](app/templates/base_app.html:154). O copiloto inclui o ícone em [core/_copiloto.html:34](app/templates/core/_copiloto.html:34).

**Falta:** completar a inspeção transitiva de includes e comprovar renderização e cliques nas duas cascas e nos fragmentos **[não verificado]**. A matriz antiga também não deve ser tratada como retrato literal deste HEAD: os gates de resposta do modal e do card de pergunta agora usam `respondida`, em [modal_fio.html:185](app/templates/chamados/partials/modal_fio.html:185), [modal_fio.html:217](app/templates/chamados/partials/modal_fio.html:217), [\_pergunta_card.html:24](app/templates/chamados/partials/_pergunta_card.html:24) e [\_pergunta_card.html:29](app/templates/chamados/partials/_pergunta_card.html:29).

### 2. Escritor de reabertura — STATUS: FECHADO para a chamada questionada

`reabrir_para_correcao` prepara a limpeza do par e da validação, registra diretamente a decisão com a fotografia anterior e chama `descarimbar_em_lote`; seu corpo não chama `linhas_da_projecao_sem_linha`. Isso fecha a dúvida específica do item, não o censo de todos os escritores. [disputa_emissao.py:1610](app/chamados/services/disputa_emissao.py:1610), [disputa_emissao.py:1632](app/chamados/services/disputa_emissao.py:1632), [disputa_emissao.py:1649](app/chamados/services/disputa_emissao.py:1649).

### 3. Leitores cruzados — STATUS: ABERTO

A lista abaixo é **parcial**, não o censo integral pedido:

- Juiz do par: [juizes.py:1544](app/chamados/juizes.py:1544); fase que prioriza `validada_em`: [models.py:3152](app/chamados/models.py:3152); flag do card: [fio.py:178](app/chamados/services/fio.py:178).
- Leitores diretos da validação na interface: [\_pergunta_card.html:20](app/templates/chamados/partials/_pergunta_card.html:20), [\_pergunta_card.html:23](app/templates/chamados/partials/_pergunta_card.html:23), [\_pergunta_card.html:42](app/templates/chamados/partials/_pergunta_card.html:42), [modal_fio.html:228](app/templates/chamados/partials/modal_fio.html:228). O card do colaborador consome a fase em [\_card_atendimento.html:189](app/templates/chamados/partials/_card_atendimento.html:189).
- Texto e instante na API: [api/views.py:1163](app/api/views.py:1163), [api/views.py:2561](app/api/views.py:2561); filtro de apresentação que lê texto, inclusive por `getattr`: [chamados_tags.py:139](app/chamados/templatetags/chamados_tags.py:139), [chamados_tags.py:166](app/chamados/templatetags/chamados_tags.py:166).
- Comandos que consultam ou classificam projeções incluem [backfill_decisoes_pergunta.py:56](app/chamados/management/commands/backfill_decisoes_pergunta.py:56), [auditar_invariantes_chamados.py:206](app/chamados/management/commands/auditar_invariantes_chamados.py:206) e [auditar_invariantes_chamados.py:1077](app/chamados/management/commands/auditar_invariantes_chamados.py:1077). A função que detecta projeção sem linha é usada por escritores e pelo backfill, por exemplo [validacao.py:167](app/chamados/services/validacao.py:167), [disputa_emissao.py:1480](app/chamados/services/disputa_emissao.py:1480), [models.py:1619](app/chamados/models.py:1619) e [backfill_fio_morto_e4b.py:141](app/chamados/management/commands/backfill_fio_morto_e4b.py:141).

**Q4 é CONSERVADA**, conforme decisão fornecida no pedido: validação projetada sem linha deve aparecer como “Validada” **[não verificado no código como decisão histórica]**. Os leitores que precisam preservar esse caso são, no mínimo, os que exibem ou decidem a fase/validação nos pontos citados de `pergunta_card`, `modal_fio`, `estado` e card do colaborador; a lista integral de leitores indiretos ainda depende do censo aberto acima. [models.py:3159](app/chamados/models.py:3159), [\_pergunta_card.html:23](app/templates/chamados/partials/_pergunta_card.html:23), [modal_fio.html:228](app/templates/chamados/partials/modal_fio.html:228), [\_card_atendimento.html:189](app/templates/chamados/partials/_card_atendimento.html:189).

**Falta:** classificar todas as ocorrências ORM, escritores, leitores indiretos, nomes dinâmicos e caminhos PDF; não tenho evidência para afirmar uma lista completa arquivo:linha **[não verificado]**.

### 4. Contrato F1 antigo — STATUS: ABERTO

A assinatura e o fold foram fornecidos no texto da matriz, mas não foi possível conferir o conteúdo histórico nem seus chamadores nesta cópia **[não verificado]**. A propriedade efetiva desta cópia delega ao juiz do par projetado: [models.py:3674](app/chamados/models.py:3674), [juizes.py:1544](app/chamados/juizes.py:1544).

**Falta:** acesso ao objeto Git histórico `be2b8d733`, ou ao conteúdo da implementação e seus chamadores **[não verificado]**. Não é possível afirmar que a descrição fornecida da F1 corresponde byte a byte à implementação histórica **[não verificado]**.

### 5. Frota U1/U2 — STATUS: ABERTO; fecha só por sonda

As contagens de produção não foram fornecidas **[não verificado]**. Para U1, existe uma porta real cujo predicado é carimbo presente e via fora de `VIAS_DE_IDENTIDADE`: [relavra_via.py:171](app/chamados/services/relavra_via.py:171). A sonda abaixo usa essa porta. Para U2, a matriz exige o último ato pela leitura F1, mas essa porta não foi localizada nesta cópia **[não verificado]**. Assim, não posso entregar honestamente uma sonda U2 pela função real sem reproduzir o fold; falta a implementação F1 e sua porta executável **[não verificado]**.

A mesma execução mede o contador pedido de **validação projetada sem linha**, chamando a função real: ela compara pergunta, ato e instante, e devolve a linha virtual de validação quando não há correspondência. [models.py:4122](app/chamados/models.py:4122).

Executar no ambiente Django de produção, a partir de `app`, com `python manage.py shell`, colando o bloco abaixo. Ele apenas consulta e imprime contagens; não chama `registrar` **[não verificado em produção]**.

```python
from chamados.models import DecisaoPergunta, PerguntaDisputa
from chamados.services.relavra_via import q_carimbo_desfazivel

# U1: universo global da porta; sem restringir a disputa aberta.
u1 = PerguntaDisputa.objects.filter(
    q_carimbo_desfazivel(so_disputa_aberta=False)
).count()

# Mesmo predicado e mesma função usados pelo sistema: correspondência
# por (pergunta_id, ato=VALIDACAO, ato_em=validada_em).
qs = (
    PerguntaDisputa.objects
    .filter(validada_em__isnull=False)
    .only(
        "pk", "respondida_em", "resposta_colab",
        "validada_em", "validada_por_id",
    )
    .order_by("pk")
)

sem_linha = 0
lote = []

def contar_lote(perguntas):
    return sum(
        1
        for linha in DecisaoPergunta.linhas_da_projecao_sem_linha(perguntas)
        if linha["ato"] == DecisaoPergunta.VALIDACAO
    )

for pergunta in qs.iterator(chunk_size=1000):
    lote.append(pergunta)
    if len(lote) == 1000:
        sem_linha += contar_lote(lote)
        lote = []

if lote:
    sem_linha += contar_lote(lote)

print("U1_carimbo_fora_de_identidade_global:", u1)
print("validada_projetada_sem_linha:", sem_linha)
print("U2: NAO MEDIDA — porta real F1 indisponivel nesta copia")
```

O contador de validação sem linha **não é U2**: ele mede a ausência de uma linha de validação no mesmo instante; U2 depende da sequência e do último ato **[não verificado para a implementação F1 histórica]**. [models.py:4135](app/chamados/models.py:4135)

alcance nao verificado restante: 4

## 18/11 10:06 · sessao bd5c2c14a952 · RESPOSTAS-DE-LEI-1811-1010 · censo · origem=Code

O censo **não está fechado**: consegui confirmar alguns pontos e preparar uma sonda para separar os casos por último ato, mas não completei a classificação de todos os resultados literais, indiretos e por AST. A cópia não tem metadados Git disponíveis; portanto, a identidade do HEAD também fica **[nao verificado]**.

### 1. Escritores e leitores R/V/C

**STATUS: ABERTO.** Falta a lista completa de ocorrências classificadas, inclusive resolução de apelidos, nomes montados dinamicamente, chamadores, PDF e templates incluídos. A busca literal por esses campos em Python e HTML retornou 2.709 linhas; esse número inclui testes, comentários e ocorrências que ainda precisariam ser classificadas. **[nao verificado: completude do censo]** [nao verificado]

Sítios confirmados, ainda **não exaustivos**:

- API: resposta e instante em [views.py:1163](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:1163), [views.py:2561](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2561) e [views_mensageria.py:567](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views_mensageria.py:567).
- Template do fio: resposta e instante em [modal_fio.html:219](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:219); validação em [modal_fio.html:228](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:228).
- Reabertura lê a fotografia R/V/C e chama a porta de atos em [disputa_emissao.py:1632](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1632).
- `respostas_anteriores` lê a sequência de decisões em [models.py:3681](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3681).

**Método executado:** busca literal com `rg` por nomes de campos e símbolos; inspeção pontual dos resultados. Não executei o walker AST solicitado nem resolvi todos os acessos indiretos. **[nao verificado: acessos por apelido e nome dinâmico]** [nao verificado]

### 2. Importadores e selo global

**STATUS: ABERTO.** Estão confirmadas as dependências abaixo, mas não a exclusividade de importadores nem a classificação completa dos selos. **[nao verificado: lista completa]**

- `relavrar_via_i6` importa e chama `universo_i6`: [relavrar_via_i6.py:47](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/relavrar_via_i6.py:47). A função também é chamada no próprio auditor em [auditar_invariantes_chamados.py:497](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:497) e [auditar_invariantes_chamados.py:739](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:739).
- `dedup_perguntas_fato` importa `aplicar` e `CONTADORES_DO_APPLY`: [dedup_perguntas_fato.py:105](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/dedup_perguntas_fato.py:105).
- `worklist` importa `_parse_pendentes` e `MODULOS_ALVO`: [worklist.py:37](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/worklist.py:37).
- O selo de vitalidade declara apenas `materializada_em` e `validada_em`: [test_contract_leitores_vitalidade.py:14](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_contract_leitores_vitalidade.py:14). O walker examina as formas especificadas em [test_contract_leitores_vitalidade.py:329](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_contract_leitores_vitalidade.py:329).
- Há também selo específico para declarações de `resposta_mantem_validacao`: [test_f2_resposta_mantem_validacao.py:19](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_f2_resposta_mantem_validacao.py:19).

**Método executado:** busca literal dos nomes dos módulos e funções e leitura pontual dos selos. Falta varredura AST abrangente de imports, aliases e nomes dinâmicos. **[nao verificado]**

### 3. Campos novos e leituras de REABERTURA

**STATUS: ABERTO.** Os pontos abaixo foram verificados, mas falta rastrear todos os caminhos indiretos de `registrar` e todos os leitores sem depender da busca literal. **[nao verificado: lista completa]**

- Campos e restrições: [models.py:4071](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4071), [models.py:4077](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4077), [models.py:4095](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4095); migrações [0054:17](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/migrations/0054_decisao_da_ressuscitacao.py:17) e [0055:18](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:18).
- Declarações de `resposta_mantem_validacao` localizadas: [models.py:4143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4143), [disputa_emissao.py:1527](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1527), [disputa_emissao.py:1681](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1681) e [disputa_emissao.py:1703](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1703).
- A carga chama o predicado real `es_da_ressuscitacao` antes de formar a sequência em [tarefa_do_dia.py:427](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:427); `_pela_decisao` interpreta uma REABERTURA remanescente em [tarefa_do_dia.py:163](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:163).
- A tela de respostas anteriores interpreta atos com fotografia em [models.py:3688](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3688).

### 4. Duas cascas e último ato dos casos raspados

**STATUS: ABERTO.** A separação da frota **fecha só por sonda**; o rastreamento completo dos consumidores das duas cascas ainda falta. A busca literal encontrou includes diferentes em [base.html:393](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/base.html:393), [base.html:432](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/base.html:432) e [base_app.html:145](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/base_app.html:145), mas isso não encerra seus caminhos indiretos. **[nao verificado: consumidores completos]**

A sonda abaixo conta os dois universos descritos no anexo e os separa tanto pelo último ato físico quanto pelo último após aplicar o predicado real que a carga usa. A ordem `(ato_em, pk)` corresponde à ordem declarada no modelo e usada pela carga: [models.py:4082](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4082), [tarefa_do_dia.py:421](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:421). **[nao verificado: resultados de produção]**

Execute no ambiente Django e no tenant correto, com acesso de leitura:

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py shell <<'PY'
from collections import Counter
from django.db.models import Exists, OuterRef, Prefetch
from chamados.models import DecisaoPergunta as D, PerguntaDisputa as P

atos = D.objects.filter(pergunta_id=OuterRef("pk"))
qs = (
    P.objects.annotate(
        tem_resposta=Exists(atos.filter(ato=D.RESPOSTA)),
        tem_validacao=Exists(atos.filter(ato=D.VALIDACAO)),
    )
    .filter(
        # União dos dois universos; cada pergunta pode entrar nos dois.
        __import__("django.db.models", fromlist=["Q"]).Q(
            respondida_em__isnull=True, tem_resposta=True
        )
        | __import__("django.db.models", fromlist=["Q"]).Q(
            validada_em__isnull=True, tem_validacao=True
        )
    )
    .prefetch_related(
        Prefetch("decisoes", queryset=D.objects.order_by("ato_em", "pk"))
    )
)

contagens = Counter()
for p in qs.iterator(chunk_size=500):
    sequencia = list(p.decisoes.all())
    efetivos = [d for d in sequencia if not D.es_da_ressuscitacao(d)]
    fisico = sequencia[-1].ato if sequencia else "SEM_ATO"
    efetivo = efetivos[-1].ato if efetivos else "SEM_ATO_EFETIVO"

    if p.respondida_em is None and p.tem_resposta:
        contagens[("R_projecao_nula_com_ato_resposta", fisico, efetivo)] += 1
    if p.validada_em is None and p.tem_validacao:
        contagens[("V_projecao_nula_com_ato_validacao", fisico, efetivo)] += 1

for (universo, ultimo_fisico, ultimo_efetivo), total in sorted(contagens.items()):
    print(
        f"{universo}\tultimo_fisico={ultimo_fisico}"
        f"\tultimo_efetivo={ultimo_efetivo}\ttotal={total}"
    )
PY
```

Essa sonda **não mede o universo completo “ato sem projeção”**. Não localizei nesta cópia uma função real que faça a correspondência nesse sentido; `linhas_da_projecao_sem_linha` mede o sentido inverso, da projeção para a sequência, por ato e instante: [models.py:4122](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4122). Também não seria correto tratar toda divergência entre um ato histórico e a projeção atual como erro: a reabertura registra a fotografia e limpa a projeção em [disputa_emissao.py:1577](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1577). Portanto, o script solicitado para o universo completo **permanece aberto**: falta localizar e verificar a autoridade real aplicável ou definir a correspondência antes de apresentar uma contagem como sua. **[nao verificado]**

alcance nao verificado restante: 4

## 18/11 10:05 · sessao 9f6d94eb54e5 · RESPOSTAS-DE-LEI-1811-1010 · sonda · origem=Code

**1. Execução da sonda.** A saída fornecida não contém traceback, mostra as seis consultas e termina com `rc_sombra=0`: pelo relato, a execução terminou sem erro. A execução e o arquivo efetivamente rodado não puderam ser conferidos independentemente; a cópia não inclui `i7_sonda.py`. [não verificado]

**2. Tabela do item 7.** Na regra atual, entre as perguntas vivas, fica pendente aquela cuja disputa está aberta e cujo par projetado não satisfaz o juiz de resposta; as demais ficam no histórico. Fora do universo de vivas, a pergunta fica fora dessa partição. Isso vale para todas as linhas, inclusive SEM_ATO. [pergunta_no_app.py:45](app/chamados/services/pergunta_no_app.py:45), [pergunta_no_app.py:76](app/chamados/services/pergunta_no_app.py:76), [juizes.py:1544](app/chamados/juizes.py:1544)

Os números abaixo são **os informados na saída raspada**, para diferenças condicionais entre a partição atual e os destinos da proposta, nas perguntas vivas no instante da sonda. Não são contagens de todas as perguntas com cada ato. Como não há acesso à execução ou ao banco, sua confirmação independente é **[não verificado]**. A descrição da proposta vem do anexo fornecido; os arquivos do brief não estão disponíveis na cópia. **[não verificado]**

| Último ato | App hoje | Proposta do brief | LU-16 | Número que muda |
|---|---|---|---|---|
| RESPOSTA | Regra geral acima | Texto não vazio → histórico; vazio → pendente. [não verificado] | Não trata expressamente do ato. [CORTES.md:495](app/docs/CORTES.md:495) | **Q1: 0** pendente → histórico; **Q2: 0** histórico → pendente. [não verificado] |
| VALIDACAO | Regra geral | “Validada”; partição não fixada no anexo. [não verificado] | Não trata expressamente do ato. [CORTES.md:495](app/docs/CORTES.md:495) | [não verificado] |
| DEVOLUCAO | Regra geral | “Não validada”; partição não fixada no anexo. [não verificado] | Não trata expressamente do ato. [CORTES.md:495](app/docs/CORTES.md:495) | [não verificado] |
| RECUSA | Regra geral | Pendente. [não verificado] | Não trata expressamente do ato. [CORTES.md:495](app/docs/CORTES.md:495) | **Q3: 0** histórico → pendente. [não verificado] |
| REABERTURA | Regra geral | Pendente. [não verificado] | Não trata expressamente do ato. [CORTES.md:495](app/docs/CORTES.md:495) | **Q4: 29** histórico → pendente. [não verificado] |
| SILENCIO | Regra geral | Histórico, como proposta a decidir. [não verificado] | Para **chamado**, silêncio não fecha; só fato na célula. A frase não fixa expressamente a partição de toda pergunta. [CORTES.md:495](app/docs/CORTES.md:495) | **Q5: 11** pendente → histórico. [não verificado] |
| RETRATACAO | Regra geral | Destino não fixado no anexo. [não verificado] | Não trata expressamente do ato. [CORTES.md:495](app/docs/CORTES.md:495) | [não verificado] |
| ABSORCAO | Regra geral | Rejeitada → pendente, como proposta a decidir; outros status [não verificado]. | Não trata expressamente do ato. [CORTES.md:495](app/docs/CORTES.md:495) | **Q6: 0** rejeitadas, histórico → pendente. [não verificado] |
| SEM_ATO | Regra geral | Destino na partição [não verificado]. | Não trata expressamente do caso. [CORTES.md:495](app/docs/CORTES.md:495) | **10.626 vivas sem decisão**, segundo a saída; quantas mudariam [não verificado]. |

Para resposta vazia, há também corte posterior que registra “fora do app na F5”; sua relação com o destino pendente atribuído ao brief precisa ser decidida. [CORTES.md:509](app/docs/CORTES.md:509)

**3. Pergunta ao dono, em três linhas:**

Na partição da F5, SILENCIO deve ir ao histórico mesmo quando a partição atual o coloca em pendente? [CORTES.md:495](app/docs/CORTES.md:495), [pergunta_no_app.py:76](app/chamados/services/pergunta_no_app.py:76)  
ABSORCAO rejeitada deve ir a pendente mesmo quando a partição atual a coloca no histórico? [pergunta_no_app.py:76](app/chamados/services/pergunta_no_app.py:76), proposta do anexo [não verificado]  
Para RESPOSTA com texto vazio, prevalece fora do app, conforme o corte posterior, ou pendente, conforme a proposta do brief? [CORTES.md:509](app/docs/CORTES.md:509), proposta do anexo [não verificado]

## 18/11 10:02 · sessao 0b9072fa3ee4 · [nome]-NAO-DESFAZ-O-PAPEL · leitura · origem=Code

**A medição por órfã e o DIFF de frota respondem a perguntas diferentes:** `cobertura.md` registra mudanças de casamento em vários dias; `por_dia.md` registra zero diferenças nos campos medidos do espelho e do motor dentro da competência. O delta `c4b_delta.diff` acrescenta somente testes, sem alterar código de produção; portanto, não muda nenhuma linha do DIFF medido. (Fontes: `cobertura.md:Totais`; `por_dia.md:Camadas`; `c4b_delta.diff:diff --git`.) [nao verificado]

Nas referências aos anexos abaixo, a linha é identificada pelo par `colNNN/dia`, pois os anexos fornecidos não vieram numerados. No item (a), **a obra que cobre o caso está [não verificado]** quando a régua e as batidas não demonstram a condição exigida pela obra. Em particular, o veredito ESTRUTURA é residual e, por si só, não prova que a célula diga “sem trabalho” nem identifica a falha específica. ([e6_oraculo.py:98](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/e6_oraculo.py:98).)

### (a) Não intervalo + ESTRUTURA

| ID | Dia | Mecanismo observado; obra |
|---|---|---|
| col082 | 2026-10-30 | Duas órfãs perto de S14:40; a cura casa 15:20 e 13:51 sai de órfã, mas o marco muda para outro estado. Despejo no pareamento; **[nome]-NAO-DESFAZ-O-PAPEL**. (`autopsia_1711.out:col82 30/10`; `cobertura.md:82 30/10`) |
| col082 | 2026-10-24 | 15:24 órfã perto de S14:40; cura ocupa o marco. Despejo no pareamento; **[nome]-NAO-DESFAZ-O-PAPEL**. (`autopsia_1711.out:col82 24/10`; `cobertura.md:82 24/10`) |
| col082 | 2026-10-22 | 15:27 órfã perto de S14:40; cura ocupa o marco. Despejo no pareamento; **[nome]-NAO-DESFAZ-O-PAPEL**. (`autopsia_1711.out:col82 22/10`; `cobertura.md:82 22/10`) |
| col107 | 2026-10-25 | 15:38 órfã perto de S14:40; 15:52 já ocupa E15:50, a 14 min; persiste. Rajada recusada pela guarda; cobertura por outra obra **[não verificado]**. (`autopsia_1711.out:col107 25/10`; `cobertura.md:107 25/10`) |
| col198 | 2026-11-09 | Saída 06:58, após entrada na véspera; S07:00 vazio e sem ocupante. Associação ao dia/turno e obra **[não verificado]**. (`autopsia_1711.out:col198 09/11`; `cobertura.md:198 09/11`) |
| col366 | 2026-11-09 | Entrada na véspera e três batidas de madrugada exatamente nos marcos vazios; sem ocupante. Associação ao dia/turno e obra **[não verificado]**. (`autopsia_1711.out:col366 09/11`; `cobertura.md:366 09/11`) |
| col366 | 2026-11-05 | Mesmo padrão: três batidas de madrugada nos marcos vazios, sem ocupante. Associação e obra **[não verificado]**. (`autopsia_1711.out:col366 05/11`; `cobertura.md:366 05/11`) |
| col366 | 2026-11-03 | Mesmo padrão: três batidas de madrugada nos marcos vazios, sem ocupante. Associação e obra **[não verificado]**. (`autopsia_1711.out:col366 03/11`; `cobertura.md:366 03/11`) |
| col366 | 2026-10-30 | Mesmo padrão: três batidas de madrugada nos marcos vazios, sem ocupante. Associação e obra **[não verificado]**. (`autopsia_1711.out:col366 30/10`; `cobertura.md:366 30/10`) |
| col366 | 2026-10-28 | Mesmo padrão: três batidas de madrugada nos marcos vazios, sem ocupante. Associação e obra **[não verificado]**. (`autopsia_1711.out:col366 28/10`; `cobertura.md:366 28/10`) |
| col489 | 2026-11-10 | S00:00 órfã exatamente no marco vazio, após entrada na véspera; sem ocupante. Associação ao dia/turno e obra **[não verificado]**. (`autopsia_1711.out:col489 10/11`; `cobertura.md:489 10/11`) |
| col566 | 2026-11-17 | Saída 06:32 após entrada na véspera; S06:00 vazio, sem ocupante. Associação ao dia/turno e obra **[não verificado]**. (`autopsia_1711.out:col566 17/11`; `cobertura.md:566 17/11`) |
| col868 | 2026-11-17 | Saída 06:49 após entrada e intervalo noturnos; S07:00 vazio, sem ocupante. Associação ao dia/turno e obra **[não verificado]**. (`autopsia_1711.out:col868 17/11`; `cobertura.md:868 17/11`) |
| col947 | 2026-11-12 | Batida 12:29 registrada como S perto de E13:00 vazio; a cura a casa ao marco de entrada. Pareamento reclassifica o tipo no marco; **[nome]-NAO-DESFAZ-O-PAPEL**. (`autopsia_1711.out:col947 12/11`; `cobertura.md:947 12/11`; [utils.py:670](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:670)) |
| col947 | 2026-10-29 | Batida 12:28 registrada como S perto de E13:00 vazio; a cura a casa ao marco de entrada. Pareamento reclassifica o tipo no marco; **[nome]-NAO-DESFAZ-O-PAPEL**. (`autopsia_1711.out:col947 29/10`; `cobertura.md:947 29/10`; [utils.py:670](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:670)) |
| col1017 | 2026-11-17 | Saída 05:57 após entrada na véspera; S06:00 vazio, sem ocupante. Associação ao dia/turno e obra **[não verificado]**. (`autopsia_1711.out:col1017 17/11`; `cobertura.md:1017 17/11`) |
| col1031 | 2026-11-17 | Saída 06:55 após entrada na véspera; S07:00 vazio, sem ocupante. Associação ao dia/turno e obra **[não verificado]**. (`autopsia_1711.out:col1031 17/11`; `cobertura.md:1031 17/11`) |
| col1031 | 2026-11-15 | 00:01 e 06:55 órfãs perto de E00:00 e S07:00 vazios; sem ocupante. Associação ao dia/turno e obra **[não verificado]**. (`autopsia_1711.out:col1031 15/11`; `cobertura.md:1031 15/11`) |
| col1031 | 2026-11-11 | 00:07 e 06:52 órfãs perto de E00:00 e S07:00 vazios; sem ocupante. Associação ao dia/turno e obra **[não verificado]**. (`autopsia_1711.out:col1031 11/11`; `cobertura.md:1031 11/11`) |

**Total (a): 19 dias; 5 com efeito observado da [nome]; 14 com cobertura por obra [não verificado].** Para decidir DIA-SEM-TRABALHO L1/L2 nos casos pendentes faltam a célula do dia e sua situação de trabalho, vínculo/folga e DNA; para atribuir MOTOR-LE-O-TURNO ou outra causa faltam também o turno montado e o realizado do dia na medição de banco. A régua exibida é uma união de colunas e não prova, sozinha, o estado da célula ou a atribuição do turno. ([utils.py:1588](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1588); [utils.py:1605](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1605); [utils.py:1814](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1814).)

### (b) Intervalo + CADASTRO

A coluna abaixo relata o teste **por órfã**; em todos os oito dias, o DIFF de frota medido não mostrou mudança nos campos do espelho na janela. (`por_dia.md:Agregados na janela`.) [nao verificado]

| ID | Dia | Resultado por órfã |
|---|---|---|
| col107 | 2026-10-30 | 15:11 sai de órfã e ocupa E15:50. (`cobertura.md:107 30/10`) |
| col369 | 2026-11-16 | 13:58 sai de órfã e ocupa S15:00. (`cobertura.md:369 16/11`) |
| col877 | 2026-11-02 | 14:31 sai de órfã e ocupa S14:00. (`cobertura.md:877 02/11`) |
| col923 | 2026-11-09 | 04:36 sai de órfã e ocupa S04:00. (`cobertura.md:923 09/11`) |
| col923 | 2026-11-08 | 04:31 sai de órfã e ocupa S04:00. (`cobertura.md:923 08/11`) |
| col923 | 2026-10-31 | 04:31 sai de órfã e ocupa S04:00. (`cobertura.md:923 31/10`) |
| col964 | 2026-10-28 | 11:35 permanece órfã; E12:00 tem ocupante a 11 min. (`cobertura.md:964 28/10`) |
| col988 | 2026-11-01 | 12:25 sai de órfã e ocupa E13:00. (`cobertura.md:988 01/11`) |

**Total (b): 8 dias; 7 com órfã casada e 1 persistente na medição por órfã; 0 dias com diferença no DIFF de frota.** (`dias.tsv:107 30/10` a `dias.tsv:988 01/11`; `cobertura.md:107 30/10` a `cobertura.md:988 01/11`; `por_dia.md:Agregados na janela`.)

### (c) Intervalo + ESTRUTURA

| ID | Dia não zerado | “Por que não” na medição |
|---|---|---|
| col252 | 2026-11-17 | Próprio marco a 33 min; ocupante 12:50 em S12:38, a 15 min. (`cobertura.md:252 17/11`) |
| col252 | 2026-11-13 | Próprio marco a 49 min; ocupante 12:35 em S12:38, a 14 min. (`cobertura.md:252 13/11`) |
| col252 | 2026-11-11 | Próprio marco a 46 min; ocupante 12:39 em S12:38, a 13 min. (`cobertura.md:252 11/11`) |
| col252 | 2026-11-10 | Próprio marco a 46 min; ocupante 12:39 em S12:38, a 13 min. (`cobertura.md:252 10/11`) |
| col252 | 2026-11-09 | Próprio marco a 46 min; ocupante 12:38 em S12:38, a 14 min. (`cobertura.md:252 09/11`) |
| col444 | 2026-11-16 | Próprio marco a 55 min; ocupante 14:01 em E14:00, a 6 min. (`cobertura.md:444 16/11`) |

**Total (c): 29 dias; 23 zerados e 6 não zerados**, contando como zerado o dia em que todas as órfãs listadas ocupam seus marcos na medição por órfã. (`dias.tsv:148 04/11` a `dias.tsv:898 27/10`; `cobertura.md:Totais` e linhas dos respectivos dias.)

## 18/11 10:01 · sessao 86bd4abda65b · E4C-F4 · verificador · origem=Code

1. **O delta corrige o ambiente do teste c2 sem mascarar o mutante.** Os dois módulos sem banco passaram: **12 testes, zero falhas e zero erros**. O teste c2 agora fornece as duas constantes usadas pelo contador ([teste c2:54](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/tests/test_bug_par_c2_sem_banco.py:54), [contador:343](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/services/materializacao.py:343)).

   Executei também `bug_par_c2_mutantes --mutante c2_contador_alias`: o controle passou e o alvo falhou por **asserção**, com `congelados_no_recorte` igual a 0 em vez de 2; resultado do executor: `falhas=1 erros=0 morto=True` ([mutante:17](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/tests/bug_par_c2_mutantes.py:17), [asserção:66](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/tests/test_bug_par_c2_sem_banco.py:66)).

2. **A exclusão filtra somente as U163 carimbadas no segundo laço; não torna a pauta inteira exatamente igual ao conjunto L1.** O materializador chama `PerguntaDisputa.carimbar`; a atribuição de `materializada_em` ocorre em `_escrever_carimbo`, também usado pela porta de carimbo por identidade ([materialização:1981](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/services/materializacao.py:1981), [modelo:3216](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/models.py:3216), [modelo:3287](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/models.py:3287)). O próprio `Command.handle` lê o campo no filtro, acumula `pauta` e a imprime; em `--apply`, volta a imprimi-la após as portas ([comando:74](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/management/commands/bug_par_103.py:74), [comando:89](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/management/commands/bug_par_103.py:89), [comando:126](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/management/commands/bug_par_103.py:126)). Outros consumidores externos dessa saída: **[não verificado]**.

   No segundo laço, uma U163 não carimbada que satisfaça os filtros de resposta e intervalo entra se sua competência estiver entregue **ou se faltar o dia do fato**; competência aberta com dia do fato não entra. As fora163 continuam sujeitas aos mesmos filtros e à mesma condição, sem a nova exclusão por carimbo ([comando:74](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/management/commands/bug_par_103.py:74), [predicado de resposta:1575](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/juizes.py:1575)). A guarda chama esses dois casos, respectivamente, L1 e L5 ([materialização:811](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/services/materializacao.py:811)).

   A **pauta completa** tem também o primeiro laço, que lista U103 entregues ou sem dia independentemente do carimbo ([comando:53](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/management/commands/bug_par_103.py:53)). Já o contador só percorre U163: coloca não carimbadas L1 em `bug_par_103_pauta_dp`, L5 em `bug_par_103_sem_dia`, e trata carimbadas segundo a elegibilidade de paridade — algumas vão para a pauta, outras para `resolvidas` ([contador:323](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/services/materializacao.py:323)). Portanto, as saídas não são conjuntos idênticos. Quais linhas concretas de produção entram: **[não verificado]**.

3. **A mordida dos dois testes de integração não foi executada**: o executor classifica `u163_fora_da_pauta` como integração e exige `--django`; não há banco disponível nesta verificação ([executor:160](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/tests/bug_par_mutantes.py:160), [executor:199](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/tests/bug_par_mutantes.py:199)).

   Pela leitura, o mutante passa a excluir toda U163 no segundo laço, e o alvo exige que as duas U163 entregues apareçam na pauta; isso **indica** que deve mordê-lo, mas o resultado executado é **[não verificado]** ([mutante:116](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/tests/bug_par_mutantes.py:116), [teste alvo:594](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/tests/test_bug_par_volta.py:594)). Da mesma forma, o caso novo grava as duas metades, entrega a competência e exige que nenhuma apareça na pauta; isso **indica** que detectaria a remoção da exclusão, mas sua execução com essa remoção é **[não verificado]** ([teste novo:622](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/tests/test_bug_par_volta.py:622), [filtro:74](/home/ronald/codex_ws/35039881d358d5cd1caeb23d94dfac64ab53bdd0/app/chamados/management/commands/bug_par_103.py:74)).

## 18/11 09:58 · sessao ef1bb5a14e98 · RESPOSTAS-DE-LEI-1811-1010 · verificador · origem=Code

**O item 3 deve ser devolvido:** há um escritor retroativo além do backfill, e o selo pedido não consegue distinguir a origem de todas as linhas pelos campos atuais. O item 4 também deve ser devolvido para esclarecer a revogação parcial: o trecho anterior que permanece atribui o caso ao admin e proíbe dizer que o colaborador deve texto, enquanto o novo corte o coloca pendente com porta do colaborador. ([disputa_emissao.py:1685](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1685), [models.py:4119](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4119), [CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509); aval fornecido: [nao verificado: arquivo:linha do anexo])

**Critério da tabela:** PROCEDE significa que foi encontrada a falha perguntada; NAO PROCEDE significa que a evidência examinada sustenta a condição ou que não há o problema naquela pergunta. Um **[nao verificado]** delimita o que não pode ser concluído e não é tratado como prova de acerto.

| Item | 1. Condição inexequível no literal ou na ordem | 2. Condição sem senão | 3. Colisão com lei, aval ou obra | 4. Referências, unidade e universo |
|---|---|---|---|---|
| **1 — F7** | **NAO PROCEDE** para o predicado: é expressável por pergunta com `validada_em` preenchida e sem DP `VALIDACAO` do mesmo `pergunta_id` e `ato_em`; o gerador já usa essa correspondência. A execução da medida na sombra e seu resultado são **[nao verificado]**. ([models.py:4122](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4122), [models.py:4144](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4144)) | **NAO PROCEDE**: o contador é medida, sem meta zero declarada; para a abertura, os quatro levantamentos devem estar fechados. A abertura antes disso é vedada pela ordem anterior fornecida. **[nao verificado: arquivo:linha dos anexos]** | **NAO PROCEDE** para o corte de conservar: D05 conserva as fases públicas, e o código atual mostra `VALIDADA` quando há validação sem materialização. D05, sozinho, não escolhia expressamente o caso sem linha; a escolha específica vem do aval fornecido. ([BACKLOG.md:2137](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:2137), [models.py:3159](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3159); aval: **[nao verificado: arquivo:linha]**) | **NAO PROCEDE** quanto ao número citado: o F7.r fornecido declara **4**, contando quatro levantamentos abertos, não quatro linhas de código. Sua presença como arquivo físico e suas linhas são **[nao verificado]**. Unidade do predicado: pergunta. ([models.py:4135](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4135)) |
| **2 — F9** | **NAO PROCEDE** quanto à definição: é possível manter o reparo atual e definir uma medida separada; a obtenção do número na sombra é **[nao verificado]**. O comando seleciona texto com instante nulo e exige igualdade exata com `MEDIDAS`. ([curar_perguntas_em_limbo.py:21](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/curar_perguntas_em_limbo.py:21), [curar_perguntas_em_limbo.py:46](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/curar_perguntas_em_limbo.py:46)) | **NAO PROCEDE**: a medida não declara um resultado-alvo; os quatro levantamentos são condição de abertura, logo a falta de fechamento mantém o construtor fechado. **[nao verificado: arquivo:linha dos anexos]** | **NAO PROCEDE**: manter o reparo da projeção por allowlist até sua morte na E4d corresponde ao item 4 do corte anterior; o código também recusa se o conjunto divergir. ([CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509), [curar_perguntas_em_limbo.py:48](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/curar_perguntas_em_limbo.py:48)) | **NAO PROCEDE** quanto à finalidade e ao número: o comando repara o meio-par da própria projeção; o F9.r fornecido declara **4** levantamentos abertos. O universo “ato sem projeção” é diferente e não tem número no dado recebido **[nao verificado]**. ([curar_perguntas_em_limbo.py:5](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/curar_perguntas_em_limbo.py:5); arquivo:linha do F9.r **[nao verificado]**) |
| **3 — F3** | **PROCEDE**. O censo pedido em (i) encontra ao menos outro escritor retroativo: `carimbar_resposta_pela_trilha` grava `ato_em=instante` recebido da nota anterior; deve, portanto, cair no ramo **PAREI**, antes da abertura. Em (ii), o motivo da foto da projeção é compartilhado pelo backfill e por escritores ordinários: não identifica exclusivamente a origem da inserção. Além disso, o `marcos_do_lote` atual nem lê DP, de modo que comparar com e sem essas linhas nele hoje dá um selo incapaz de testar a futura leitura por DP. ([disputa_emissao.py:1685](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1685), [curar_perguntas_em_limbo.py:67](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/curar_perguntas_em_limbo.py:67), [models.py:4119](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4119), [backfill_decisoes_pergunta.py:67](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/backfill_decisoes_pergunta.py:67), [validacao.py:167](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/validacao.py:167), [veredito_lavrado.py:201](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:201)) | **NAO PROCEDE** para as duas condições expressas: outro escritor implica PAREI com referência e pergunta; diferença no selo implica PAREI com tabela e não pousa. **[nao verificado: arquivo:linha do aval fornecido]** | **NAO PROCEDE** como colisão demonstrada: o aval fornecido responde à questão histórica aberta no F3.r e exige fechar os três levantamentos antes do construtor, compatível com a proibição da ordem fornecida. Interseção efetiva com todas as obras em voo: **[nao verificado]**. ([raiox.py:38](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/raiox.py:38); anexos **[nao verificado: arquivo:linha]**) | **PROCEDE**: o aval afirma que `marcos_do_lote(ate=...)` lê pelo instante **do ato**, mas hoje ele filtra tempos de batida, célula e projeções `materializada_em`/`respondida_em`; não consulta `DecisaoPergunta`. O **3** restante confere com o F3.r fornecido, como três levantamentos abertos; arquivo:linha desse anexo **[nao verificado]**. ([veredito_lavrado.py:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:151), [veredito_lavrado.py:197](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:197)) |
| **4 — F5** | **NAO PROCEDE** quanto à execução do comportamento citado: o juiz considera o meio-par não respondido e a partição o coloca pendente quando a disputa está aberta; o teste citado verifica essa situação. A aplicação à frota inteira é **[nao verificado]**. ([juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544), [pergunta_no_app.py:76](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/pergunta_no_app.py:76), [test_o261e_f5_alcance.py:184](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_o261e_f5_alcance.py:184)) | **NAO PROCEDE**: o corte decide o comportamento; a restrição da competência entregue permanece condicionada ao `!` pela L-130. Não há teste condicional adicional sem ramo alternativo identificado. ([LEIS.md:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:151)) | **PROCEDE**: a revogação literal alcança “fora do app / dono admin”, mas preserva o restante do item 5 anterior, inclusive “a leitura do painel para esse caso diz admin, nunca ‘colab deve texto’”. Isso colide com a nova afirmação de pendência e porta do colaborador. É necessário esclarecer a extensão da revogação. ([CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509), [test_o261e_f5_alcance.py:192](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_o261e_f5_alcance.py:192); literal do novo aval **[nao verificado: arquivo:linha]**) | **NAO PROCEDE** quanto ao teste: suas asserções atuais conferem para a fixture indicada, embora a docstring anterior ainda descreva o resultado antigo. A L-130 também distingue o estoque de competência entregue, sujeito a `!`. ([test_o261e_f5_alcance.py:184](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_o261e_f5_alcance.py:184), [test_o261e_f5_alcance.py:198](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_o261e_f5_alcance.py:198), [LEIS.md:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:151)) |
| **5 — F5** | **NAO PROCEDE** para o fluxo descrito: a ressuscitação limpa instante e texto; a partição usa o juiz e considera pendente a pergunta não respondida em disputa aberta. A medida de quantas passarão a pendentes é **[nao verificado]**. ([models.py:1619](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1619), [models.py:1637](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1637), [pergunta_no_app.py:82](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/pergunta_no_app.py:82)) | **NAO PROCEDE**: o número deve ser medido antes do pouso; o item não estabelece limite numérico cujo resultado falso precise de tratamento. Sem a medida, a condição temporal não está cumprida **[nao verificado]**. | **NAO PROCEDE** quanto à decisão geral de devolução: o código registra que ela segue não decidida e evita usar a reabertura que produziria esse efeito na tarefa. A ressalva da colisão do item 4 sobre o painel permanece pertinente ao caso de resposta vazia. ([models.py:1630](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1630), [CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509)) | **NAO PROCEDE** para a unidade e a leitura descritas, com a ressalva de disputa aberta: o juiz opera sobre a pergunta e a partição consulta também `fechada_em`. O número que muda na sombra e eventuais efeitos transitivos fora dos sítios examinados são **[nao verificado]**. ([juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544), [pergunta_no_app.py:82](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/pergunta_no_app.py:82)) |

**Quem escreve e quem lê os campos dos itens 1 e 3**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `PerguntaDisputa.validada_em` | A validação atribui o instante e registra o ato; `carimbar(..., resposta='validar')` também pode atribuí-lo. Devolução, resposta e ressuscitação têm caminhos que o limpam. JUIZES distingue o escritor único **do ato de validar** dos demais escritores do campo. ([validacao.py:161](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/validacao.py:161), [models.py:3222](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3222), [validacao.py:269](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/validacao.py:269), [disputa_emissao.py:1494](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1494), [models.py:1637](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1637), [JUIZES.md:38](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/JUIZES.md:38)) | `estado` o traduz em fase; o gerador de linhas ausentes o compara à DP; o placar filtra e conta validações. Censo exaustivo de leitores e escritores: **[nao verificado]**. ([models.py:3152](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3152), [models.py:4144](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4144), [verbos.py:263](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/verbos.py:263)) |
| `DecisaoPergunta.ato_em` | A porta persistente é `registrar`, que recebe o instante dos chamadores. Exemplos: validação usa `validada_em`; foto da projeção usa o instante projetado; a trilha usa o instante histórico da nota. O backfill também usa o gerador da projeção. Censo integral, inclusive chamadas indiretas: **[nao verificado]**. ([models.py:4152](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4152), [validacao.py:173](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/validacao.py:173), [models.py:4141](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4141), [disputa_emissao.py:1701](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1701), [backfill_decisoes_pergunta.py:67](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/backfill_decisoes_pergunta.py:67)) | A ordenação padrão é `(ato_em, pk)`; a carga da tarefa ordena e aplica o horizonte pelo instante do ato. **Hoje `marcos_do_lote` não é leitor desse campo.** Censo integral de leitores: **[nao verificado]**. ([models.py:4081](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4081), [tarefa_do_dia.py:418](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:418), [veredito_lavrado.py:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:151)) |

**AVAL-DEVOLVIDO — por item**

- **1: não.** ([models.py:4144](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4144))
- **2: não.** ([curar_perguntas_em_limbo.py:46](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/curar_perguntas_em_limbo.py:46))
- **3: sim** — perguntas 1 e 4. ([disputa_emissao.py:1685](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1685), [veredito_lavrado.py:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:151))
- **4: sim** — pergunta 3. ([CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509))
- **5: não**, com a ressalva da colisão do item 4 quando aplicada ao mesmo caso de resposta vazia. ([models.py:1630](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1630), [CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509))

Nenhuma medida de produção ou execução de teste foi realizada; seus resultados são **[nao verificado]**. [nao verificado]

## 18/11 09:58 · sessao d7adafe51575 · E4C-F4 · verificador · origem=Code

**Parecer: a alteração central corresponde ao corte, mas a cópia não está pronta para ser considerada verificada.** Há um teste existente que falha sem mutação e invalida o controle de um mutante; há também uma segunda consulta K6 no novo ramo do contador, embora a decisão do balde venha da testemunha. A execução dos testes de integração e dos três mutantes novos ficou **[não verificado]**. ([materializacao.py](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:342), [test_bug_par_c2_sem_banco.py](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_c2_sem_banco.py:54), [bug_par_c2_mutantes.py](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/bug_par_c2_mutantes.py:17))

A cópia não tem `.git`; usei o diff fornecido na pergunta como comparação com o pai. Os textos dos avais foram fornecidos sem numeração de linhas; suas referências literais abaixo são **[não verificado: arquivo:linha do anexo]**. Não consultei banco ou logs, e executei somente testes sem banco, com escrita de bytecode desativada. O total e a lista da medição de produção exigida antes da construção são **[não verificado]**. [nao verificado]

### 1. Escopo do aval

- **Consulta K6 e docstrings:** retirar a exclusão U163 e atualizar as descrições corresponde ao corte. Os hunks de `_k6_que_barra`, `_competencia_que_barra` e `_materializar_uma` alteram descrição ou comentário; a composição F6 permanece no código mostrado. ([materializacao.py:804](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:804), [materializacao.py:831](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:831), [materializacao.py:1866](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:1866); aval **[não verificado: arquivo:linha]**).
- **Comando:** a troca de exclusão U163 por U103 inclui U60 e evita repetir U103 já tratada pelo primeiro laço, atendendo ao complemento. O filtro do segundo laço, porém, inclui também U60 respondidas já materializadas: ele não filtra `materializada_em` nem pergunta à testemunha se aquela linha é efetivamente barrada. Assim, a saída é mais ampla que “as 163 que a L1 barrar”, embora o laço anterior para fora163 já tivesse esse formato. Se essa ampliação foi autorizada literalmente, é uma questão de interpretação do aval **[não verificado: arquivo:linha do aval]**. ([bug_par_103.py:53](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/management/commands/bug_par_103.py:53), [bug_par_103.py:73](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/management/commands/bug_par_103.py:73)).
- **Contador:** o novo ramo envia L1/L5 não materializadas aos baldes existentes, retira-as de `irma` e, por consequência, de `universo` e `pendentes_aval`; isso corresponde ao item 2. A consulta adicional discutida abaixo é um detalhe de implementação que o texto literal não pede. ([materializacao.py:341](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:341); complemento **[não verificado: arquivo:linha]**).
- **Validação:** eliminar a busca automática da irmã é uma otimização adicional ao texto literal. Para a irmã encontrada pela porta, a justificativa está no filtro de mesma disputa e mesmo `data_evento`; a porta continua passando explicitamente a irmã que encontrou. ([validacao.py:209](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/validacao.py:209), [materializacao.py:139](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:139), [validacao.py:726](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/validacao.py:726)).
- **RED e mutantes:** há casos de par U163 entregue, sem irmã entregue, controles abertos e o controle anterior com irmã fora163. A existência dos casos atende ao item 3 na leitura do código; que tenham ficado vermelhos no pai e verdes na cópia é **[não verificado]**. ([test_bug_par_volta.py:146](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_volta.py:146), [test_bug_par_volta.py:516](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_volta.py:516), [test_bug_par_volta.py:546](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_volta.py:546)).

### 2. Escritores, leitores e significado

A cadeia direta localizada é esta:

| Valor ou função | Leitores localizados | Efeito do diff |
|---|---|---|
| `k6_consulta_a_pergunta` | `_k6_que_barra` em [materializacao.py:819](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:819); teste extraído em [test_bug_par_sem_banco.py:140](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_sem_banco.py:140) | O domínio passa de fora163 para toda metade de intervalo; o leitor de produção usa o novo domínio. |
| `_k6_que_barra` | `_competencia_que_barra` em [materializacao.py:838](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:838); `entrega_que_barra_a_validacao` em [validacao.py:225](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/validacao.py:225); teste em [test_bug_par_sem_banco.py:142](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_sem_banco.py:142) | L1/L5 conservam o significado dos estados e passam a alcançar U163. |
| `_competencia_que_barra` | Derivador em [materializacao.py:925](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:925) e [materializacao.py:949](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:949); escrita em [materializacao.py:1483](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:1483) e [materializacao.py:1867](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:1867); testes em [test_bug_par_sem_banco.py:142](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_sem_banco.py:142) | Onde recebe perguntas, K6 alcança U163; onde recebe só instantes, continua a consulta F6. |
| `entrega_que_barra_a_validacao` | Testemunha em [materializacao.py:902](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:902); porta antes e depois do ato em [validacao.py:740](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/validacao.py:740) e [validacao.py:777](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/validacao.py:777) | A testemunha consulta a própria metade; a porta também fornece a irmã. A equivalência depende da identidade de dia da irmã legítima. |
| Contador e chaves | Comando lê `pendentes_aval` em [bug_par_103.py:81](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/management/commands/bug_par_103.py:81); testes em [test_bug_par_103.py:85](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_103.py:85), [test_bug_par_projetado.py:499](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_projetado.py:499), [test_bug_par_projetado.py:1050](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_projetado.py:1050), [test_bug_par_c2_sem_banco.py:42](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_c2_sem_banco.py:42) e [test_bug_par_volta.py:594](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_volta.py:594) | Os nomes `bug_par_103_pauta_dp` e `bug_par_103_sem_dia` agora incluem não carimbadas U163; `universo`, `irma` e `por_irma` deixam de incluí-las. O comando lê apenas o total pendente, cujo significado foi atualizado. |

A lista acima cobre os pontos diretos identificados pela busca, mas **a exaustividade de todos os leitores de teste e indiretos solicitados permanece [não verificado]**. Em particular, outros usos do contador aparecem em [test_bug_par_projetado.py:202](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_projetado.py:202) e seguintes; não concluí a inspeção individual de cada asserção.

Nas telas, o card usa a tag que lê a testemunha e trata L1/L5 como bloqueios; o fio alimenta o mesmo estado para o modal, que também o trata e esconde a validação. Esses leitores aceitam o domínio ampliado dos estados. ([chamados_tags.py:131](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/templatetags/chamados_tags.py:131), [_pergunta_card.html:43](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/templates/chamados/partials/_pergunta_card.html:43), [fio.py:376](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/fio.py:376), [modal_fio.html:274](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/templates/chamados/partials/modal_fio.html:274)).

O explicador consulta a testemunha no ramo de recusa materializada e a espera de declarado no ramo seguinte; não verifiquei que sua explicação para toda U163 já validada e barrada por L1 seja adequada. O passivo chama a unidade de materialização, e seu comando chama o reparador. `relavra_via` lê a via e recalcula a via pelo fato nos trechos abertos; leitura indireta adicional e cron efetivo ficam **[não verificado]**. ([explicador.py:115](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/explicador.py:115), [explicador.py:131](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/explicador.py:131), [passivo_validado.py:537](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/passivo_validado.py:537), [materializar_passivo_validado.py:68](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/management/commands/materializar_passivo_validado.py:68), [relavra_via.py:95](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/relavra_via.py:95), [relavra_via.py:117](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/relavra_via.py:117)).

Não encontrei, nos leitores de produção acima, uma condição executável que continue pulando U163 no K6 ou limite a Pauta DP às U103 carimbadas; isso **não prova ausência em toda a árvore [não verificado]**. O comando seleciona todas as U103 no primeiro laço e outras metades respondidas no segundo, de modo que Pauta DP já não significa apenas carimbadas U103. ([bug_par_103.py:53](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/management/commands/bug_par_103.py:53), [bug_par_103.py:73](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/management/commands/bug_par_103.py:73)).

### 3. Classes de reprovação

**O contador faz uma segunda consulta ao juiz.** A decisão de encaminhar ao balde vem de `e['estado']`, mas `competencia_do_fato` é chamada novamente para o rótulo e também devolve o booleano K6. Portanto, é a **mesma composição canônica**, sem segundo predicado de entregue, mas não é simplesmente ler uma testemunha que já carregue mês e ano: há uma nova leitura K6. O ramo não usa o booleano dessa segunda leitura para escolher entregue; usa `None` como alternativa para sem-dia. Consistência entre as duas leituras sob mudança concorrente é **[não verificado]**. ([materializacao.py:342](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:342), [materializacao.py:788](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:788)).

A irmã legítima encontrada por `metade_irma_do_intervalo` tem a mesma disputa e o mesmo dia do fato; logo não pode ter competência diferente pela composição de dia e empresa usada aqui. A função pública aceita uma irmã passada explicitamente sem validar essa identidade, mas a porta mostrada obtém a irmã pela função que impõe o filtro. Outros chamadores com irmã arbitrária são **[não verificado]**. ([materializacao.py:139](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:139), [validacao.py:200](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/validacao.py:200), [validacao.py:726](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/validacao.py:726)).

K6 usa `data_evento`; F6 usa a data local do instante de plantio. A janela usa início no dia de corte e fim no dia anterior ao corte seguinte, inclusivos. São dois insumos diferentes explicitados no código; não identifiquei campo reutilizado com esses dois significados nos trechos abertos. ([materializacao.py:788](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:788), [janelas.py:15](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/ponto/janelas.py:15), [janelas.py:75](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/ponto/janelas.py:75)).

O diff não mostra novo escritor; a porta trava colaborador, disputa e chamados antes do carimbo e da consulta anterior à escrita. Não concluí auditoria global de juiz duplicado, relógio fora da borda, ordem de todas as travas ou selos: **[não verificado]**. ([validacao.py:724](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/validacao.py:724), [validacao.py:740](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/validacao.py:740)).

### 4. Mutantes

Por inspeção, os três novos têm alvos capazes de distinguir a mudança: o primeiro restaura o pulo das duas metades do RED; o segundo impede a classificação no balde; o terceiro volta a excluir U163 da lista. **Morte efetiva por asserção e controle verde: [não verificado]**, pois não executei os testes de integração. ([bug_par_mutantes.py:110](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/bug_par_mutantes.py:110), [test_bug_par_volta.py:516](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_volta.py:516), [test_bug_par_volta.py:594](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_volta.py:594)).

O `f1_testemunha_sem_irma` removido troca a omissão da irmã por `None`; na implementação atual ambos seguem o mesmo ramo e passam `None` à guarda. É equivalente para esse sítio e essa implementação. ([validacao.py:219](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/validacao.py:219); mutação anterior no diff fornecido **[não verificado: arquivo:linha do diff]**).

**Controle inválido confirmado:** `c2_contador_alias`, de catálogo vizinho, aponta para teste que já falha sem mutação: o ambiente extraído não fornece as duas constantes novas, e a primeira chamada ao contador lança `NameError` em `BLOQUEADA_COMPETENCIA_ENTREGUE`. Executei 12 testes sem banco; 11 passaram e esse teve erro. Outros controles ou equivalências do catálogo completo: **[não verificado]**. ([bug_par_c2_mutantes.py:17](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/bug_par_c2_mutantes.py:17), [test_bug_par_c2_sem_banco.py:54](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_c2_sem_banco.py:54), [materializacao.py:343](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:343)).

### 5. Testes vizinhos [nao verificado]

O teste antigo com irmã fora163 entregue mantém as asserções de L1, mas sua explicação final ainda diz que a testemunha sem irmã diverge da porta; depois da cura, a própria pergunta fornece L1. A asserção não deve quebrar por esse motivo. ([test_bug_par_volta.py:146](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_volta.py:146)).

No teste de precedência, a asserção de L1 após `self.k6.return_value = True` continua correta, mas agora é satisfeita pela própria U163; a asserção anterior de L2 usa K6 aberto por padrão. Não identifiquei ali uma asserção que necessariamente quebre. ([test_bug_par_volta.py:22](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_volta.py:22), [test_bug_par_volta.py:325](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_volta.py:325)).

A falha confirmada do teste sem banco ocorre **antes de qualquer asserção**, por ambiente incompleto, e não por expectativa antiga sobre o pulo. A lista completa de testes vizinhos com asserções que mudariam de resultado permanece **[não verificado]**. ([test_bug_par_c2_sem_banco.py:59](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/tests/test_bug_par_c2_sem_banco.py:59), [materializacao.py:343](/home/ronald/codex_ws/9abae82b0f2ed849a6873e8be92ef0b1aa96a659/app/chamados/services/materializacao.py:343)).

## 18/11 09:57 · sessao d46d2da37529 · DST-L1 · leitura · origem=Code

**Falta uma porta de decisão integral por pessoa × dia para DST-L1.** A retratação humana já grava que uma marcação não foi trabalho; os outros candidatos examinados gravam fatos de alcance diferente ou recusam o caso central. ([retratar_humano.py:150](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/retratar_humano.py:150), [veredito_celula.py:243](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:243), [he.py:159](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:159), [celula.py:638](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:638))

| Célula de r2 | Escritor, fato e leitor; fechamento |
|---|---|
| 1. Ainda sem decisão / “nenhuma” | **Sem escrita proposta para o default:** nenhuma leitura examinada distingue “ainda não decidido” de “decidido: nenhuma” **[não verificado: falta marca que diferencie os dois]**. `decisao_do_dia` retorna `{}` sem veredito humano de Resolver dia; isso não prova uma decisão DST. ([veredito_celula.py:85](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:85), [veredito_celula.py:110](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:110)) |
| 2. Trabalho reconhecido | **Escritor DST não demonstrado [não verificado].** `veredito_celula('trabalhou')` recusa grade não `trabalho`. `decidir_he` grava `DecisaoHE.estado`, minutos e autor para HE fora da janela; Gestão HE lê as vigentes, mas isso não registra a decisão integral sobre a batida. ([veredito_celula.py:243](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:243), [he.py:159](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:159), [he.py:65](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:65), [gestao_he.py:105](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/gestao_he.py:105)) |
| 3. Escala a corrigir / correção aplicada | **Sim para a correção efetiva; decisão pendente “a corrigir” não demonstrada [não verificado].** `editar_dia` grava `trabalha`, DNA, origem `editada`, autor e instante, com trilha; `dia_sem_trabalho` lê `trabalha`. A edição aciona tratamento da mudança; resultado específico DST **[não verificado: não há porta DST demonstrada]**. ([celula.py:638](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:638), [celula.py:649](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:649), [celula.py:665](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:665), [juizes.py:445](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:445)) |
| 4. Batida invalidada | **Sim, por marcação:** `retratar` registra o ato humano “não foi trabalho”; `retratar_batida` grava `aprovada=False` e `retratada_em`. `decisao_de` lê autoria e motivo da trilha; `batidas_apuraveis` exclui a retratada. Leitura de decisão integral **por dia DST** após a retirada da batida **[não verificado]**. ([retratar_humano.py:150](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/retratar_humano.py:150), [registro_batida.py:294](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/registro_batida.py:294), [retratar_humano.py:80](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/retratar_humano.py:80), [turnos.py:2868](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/turnos.py:2868)) |
| 5. Troca / revogação | Transição entre as quatro decisões DST **[não verificado: porta integral não demonstrada]**. Para fatos existentes, HE pode ser trocada e a retratação humana pode ser desfeita sob as condições da própria porta. ([he.py:70](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:70), [retratar_humano.py:190](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/retratar_humano.py:190)) |
| 6. Segunda execução | Idempotência da decisão DST integral **[não verificado: porta integral não demonstrada]**. HE igual vigente retorna `no_op`; edição igual retorna `igual`; retratação repetida retorna a batida. ([he.py:195](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:195), [celula.py:632](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:632), [registro_batida.py:291](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/registro_batida.py:291)) |
| 7. HE vigente + órfã | Gestão lê HE vigente; `_celula_furada`, em dia sem trabalho, consulta a véspera antes do ramo de órfãs; Resolver dia recusa `trabalhou` se a grade não for trabalho. Equivalência entre HE e decisão DST **[não verificado: são fatos de escopos distintos]**. ([gestao_he.py:105](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/gestao_he.py:105), [juizes.py:497](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:497), [veredito_celula.py:243](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:243)) |
| 8. Indicação visual DST | **[não verificado]** para a indicação de *decisão DST*: `veredito_do_dia` pode exibir folga, feriado ou `sem_turno_pareado` conforme seus insumos e precedência, sem distinguir as quatro decisões. ([dia_decidido.py:376](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_decidido.py:376), [dia_decidido.py:389](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_decidido.py:389)) |

A porta mais próxima para uma decisão humana **por dia** é `veredito_celula` / `decisao_do_dia`, mas seu vocabulário é `trabalhou`, `faltou`, `justificado`, e ela recusa `trabalhou` na grade sem trabalho. Falta a porta que grave e permita ler a decisão DST e suas transições; a identidade do HEAD informado permanece **[não verificado: a cópia não fornece metadados Git, conforme r2]**. ([veredito_celula.py:18](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:18), [veredito_celula.py:85](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:85), [veredito_celula.py:243](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:243))

_6886 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
