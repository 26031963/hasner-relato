# CODEX-ESTADO

> Ultima resposta processada: **07/11/2026 20:48** · publicado em **07/11/2026 20:52** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 07/11 20:48 · sessao b2a3c085a19a · FIO-E0-V4-E1 · verificador · origem=Code

**O item (e) continua PROCEDENTE:** existe caminho agendado que grava o Chamado e depois pede Colaborador na mesma transação, independentemente do S143. A guarda corrige os saves de campo testados, mas não fecha a classe de inversão. (`app/chamados/services/limbo_folgas.py:269`, `app/chamados/services/contexto.py:102`, `app/chamados/services/fio.py:855`, `app/chamados/models.py:290`)

**1. PROCEDENTE — inversão residual e censo dos escritores.**

O contraexemplo é `apurar_furos_diarios → agrupar_furos_sob_limbo → agrupar_furo`. O comando está encadeado; a seleção dos furos não adquire previamente as travas do colaborador. (`app/config/crons.py:1504`, `app/escala/management/commands/apurar_furos_diarios.py:19`, `app/chamados/services/limbo_folgas.py:308`, `app/chamados/services/limbo_folgas.py:326`)

Dentro de `agrupar_furo`, a sequência permitida é:

| Passo | Evidência |
|---|---|
| Abre `atomic` | `app/chamados/services/limbo_folgas.py:269` |
| Grava contexto do Chamado | `app/chamados/services/limbo_folgas.py:270` → `app/chamados/services/contexto.py:102` |
| Havendo pergunta viva, pede Colaborador ao reapontar | `app/chamados/services/limbo_folgas.py:277` → `app/chamados/services/fio.py:855` |
| Ao reconciliar, também pode pedir Colaborador → Disputa → Chamado | `app/chamados/services/limbo_folgas.py:278` → `app/chamados/models.py:637` → `app/chamados/models.py:290` |

Logo, T1 pode segurar o Chamado escrito e esperar Colaborador; T2, em `_gravar_estado`, pode segurar Colaborador e esperar o mesmo Chamado. Isso demonstra o caminho de inversão pelo código; candidatos existentes e deadlock ocorrido na frota são **[nao verificado]**. (`app/chamados/models.py:2525`, `app/chamados/models.py:2545`, `app/chamados/services/limbo_folgas.py:269`)

Censo dos sítios de escrita da aplicação fora de `_gravar_estado`:

| Escritor | Operação/local |
|---|---|
| `rebaixar` | `save`, `app/chamados/models.py:484` |
| `escalar_urgencia` | `save`, `app/chamados/models.py:538` |
| `definir_prazo_sla` | `save`, `app/chamados/models.py:575` |
| `marcar_escalonado_dp` | `save`, `app/chamados/models.py:593` |
| `_rebaixar_sem_post_save` | `QuerySet.update`, `app/chamados/models.py:619` |
| `abrir` | `get_or_create`, `app/chamados/models.py:954` |
| `criar` | `create`, `app/chamados/models.py:1078` |
| `reescrever_texto` | `save`, `app/chamados/services/texto.py:35` |
| `anotar_contexto` | `save`, `app/chamados/services/contexto.py:77` |
| `renovar_contexto` | `save`, `app/chamados/services/contexto.py:102` |
| `retirar_contexto` | `save`, `app/chamados/services/contexto.py:124` |
| `fundir_contexto` | `save`, `app/chamados/services/contexto.py:159` |
| `apontar_justificativa` | `save`, `app/chamados/services/vinculos.py:88` |
| `apontar_celula` | `save`, `app/chamados/services/vinculos.py:99` |
| `apontar_ticket_glpi` | `save`, `app/chamados/services/vinculos.py:114` |
| `assumir_responsavel` | `save`, `app/chamados/services/vinculos.py:131` |
| `liberar_responsavel` | `save`, `app/chamados/services/vinculos.py:142` |
| `silenciar` | `save`, `app/chamados/services/arquivo.py:170` |
| `levantar` | `save`, `app/chamados/services/arquivo.py:208` |

Para **cada save/inserção dessa tabela**, o conjunto registrado é o mesmo: um `pre_save` e cinco `post_save`; as condições abaixo determinam quais corpos prosseguem. (`app/chamados/apps.py:8`, `app/chamados/signals.py:53`, `app/chamados/signals.py:86`, `app/chamados/signals.py:174`, `app/chamados/signals.py:183`, `app/chamados/signals.py:265`, `app/chamados/signals.py:332`)

| Receiver | Comportamento e travas |
|---|---|
| `_cartorio_pre_save_chamado` | SELECT comum de status; escreve `_b5_status_antes`; não pede trava explícita. `app/chamados/signals.py:175`, `app/chamados/signals.py:180` |
| `rebaixa_urgencia_ao_resolver` | Em encerrado, fora de raw, faz UPDATE de urgência no próprio Chamado; nenhum `select_for_update`. `app/chamados/signals.py:68`, `app/chamados/signals.py:72`, `app/chamados/models.py:619` |
| S143 | Se passar pelas guardas, pede Colaborador → Disputas → Chamados; o corpo pode carimbar Perguntas. `app/chamados/signals.py:100`, `app/chamados/signals.py:111`, `app/chamados/services/disputa_emissao.py:1683`, `app/chamados/services/disputa_emissao.py:1749` |
| `_cartorio_post_save_chamado` | Usa nascimento/comparação de status, sem consultar `update_fields`; chama cartório sincronamente. A fábrica pode chegar a `trava_na_ordem` do universo do colaborador. `app/chamados/signals.py:188`, `app/chamados/signals.py:199`, `app/ponto/services/cartorio.py:997`, `app/chamados/services/fabrica_da_celula.py:149`, `app/chamados/services/disputa_emissao.py:530` |
| `_isentos_post_save_chamado` | Trabalho registrado por `on_commit`. `app/chamados/signals.py:276`, `app/chamados/signals.py:289` |
| `_filho_morto_acorda_o_pai` | Trabalho registrado por `on_commit`. `app/chamados/signals.py:337`, `app/chamados/signals.py:352` |

O UPDATE direto de `_rebaixar_sem_post_save` não emite esses receivers. `get_or_create`, quando encontra a linha, também não representa nova escrita. (`app/chamados/models.py:602`, `app/chamados/models.py:619`, `app/chamados/models.py:954`)

Há outro residual **condicional**: uma porta de campo recebe instância com status desatualizado; o pre_save lê o status atual do banco, mas o post_save do cartório compara essa leitura ao status antigo da instância. Pode então rejulgar e pedir travas depois do UPDATE, embora `update_fields` não contenha status. A guarda nova protege somente S143. (`app/chamados/services/texto.py:35`, `app/chamados/signals.py:100`, `app/chamados/signals.py:180`, `app/chamados/signals.py:188`, `app/chamados/signals.py:199`)

Também existem escritas fora da rotina da aplicação:

- Migrações: saves de urgência em `app/chamados/migrations/0023_rebaixar_urgencia_resolvidos.py:10`, `app/chamados/migrations/0023_rebaixar_urgencia_resolvidos.py:17`, `app/chamados/migrations/0024_rebaixar_urgencia_resolvidos_regressao.py:11`, `app/chamados/migrations/0024_rebaixar_urgencia_resolvidos_regressao.py:19`; updates em `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `app/chamados/migrations/0038_a3_registrado_backfill.py:15`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/migrations/0039_a3_via_backfill.py:25`, `app/chamados/migrations/0039_a3_via_backfill.py:27`, `app/chamados/migrations/0039_a3_via_backfill.py:28`, `app/chamados/migrations/0039_a3_via_backfill.py:33`, `app/chamados/migrations/0039_a3_via_backfill.py:34`.
- Máscara da sombra: `bulk_update` de texto/contexto do Chamado pelo gerente genérico; sem receivers. O script exige banco chamado `sombra`. (`bin/sombra_regras_pessoais.py:82`, `bin/sombra_regras_pessoais.py:142`, `bin/sombra_mascara_pessoal.py:10`, `bin/sombra_mascara_pessoal.py:62`, `bin/sombra_mascara_pessoal.py:64`)

A solicitação de trava depois do save também permanece no S143 acionado por `_gravar_estado`, mas ali a ordem já foi adquirida antes: é solicitação reentrante, distinta da aquisição tardia demonstrada no LIMBO. (`app/chamados/models.py:290`, `app/chamados/models.py:296`, `app/chamados/services/disputa_emissao.py:1683`)

**2. NAO PROCEDE perda demonstrada no fechamento canônico; PROCEDENTE a diferença semântica e a janela concorrente do save inteiro.**

“Roda” na tabela significa **chega à chamada do resolver**; o receiver pode ser invocado e retornar antes. Em todos os casos, `raw=True` ou status da instância fora de `ENCERRADOS` retorna imediatamente. (`app/chamados/signals.py:87`, `app/chamados/signals.py:92`, `app/chamados/signals.py:111`)

| Forma | S143 chega ao resolver? |
|---|---|
| `created=True`, status encerrado | Sim. `app/chamados/signals.py:79` |
| `update_fields` contém `status_local`, valor mudou | Sim. `app/chamados/signals.py:81` |
| `update_fields` contém `status_local`, valor igual | Sim. `app/chamados/signals.py:82` |
| `update_fields` não contém status | Não, mesmo se o status em memória divergir do banco. `app/chamados/signals.py:82` |
| Save inteiro; status diferente da leitura do pre_save | Sim. `app/chamados/signals.py:83` |
| Save inteiro; status igual à leitura do pre_save | Não. `app/chamados/signals.py:83` |
| Instância carregada antes de outro processo mudar o status | No save inteiro, decide pela leitura nova do pre_save, não pela fotografia do carregamento. `app/chamados/signals.py:180` |
| Raw/loaddata | Não; essa exclusão antecede a guarda nova. `app/chamados/signals.py:92` |

O escritor único de `_b5_status_antes` é `_cartorio_pre_save_chamado`: atribui `None` para raw/instância sem PK; nos demais casos sobrescreve com SELECT do banco. Seus leitores são a guarda nova, o post_save do cartório, o receiver dos isentos e o do filho morto. (`app/chamados/signals.py:177`, `app/chamados/signals.py:180`, `app/chamados/signals.py:83`, `app/chamados/signals.py:187`, `app/chamados/signals.py:276`, `app/chamados/signals.py:337`)

O pre_save é registrado no mesmo módulo carregado pelo `ready`; sua definição aparecer depois do S143 no arquivo não transforma o pre_save em post_save. Não há outro escritor desse atributo no código encontrado. (`app/chamados/apps.py:8`, `app/chamados/signals.py:174`, `app/chamados/signals.py:177`, `app/chamados/signals.py:180`)

Ausência do atributo faz a guarda comparar `None` ao status e liberar o resolver; não provoca supressão. Já a leitura pode envelhecer entre o SELECT e o UPDATE, pois não adquire trava. (`app/chamados/signals.py:83`, `app/chamados/signals.py:180`)

**Contraexemplo concorrente do save inteiro, por leitura estática:** A lê `resolvido` no pre_save; B reabre e confirma; A grava seu save inteiro com `resolvido`. A efetivamente encerra novamente, mas compara `resolvido == resolvido` e pula o resolver. A guarda anterior resolveria. Execução desse entrelaçamento e chamador produtivo usando esse save inteiro: **[nao verificado]**. (`app/chamados/signals.py:180`, `app/chamados/signals.py:83`, `app/chamados/signals.py:100`)

Esse contraexemplo não suprime o fechamento por `_gravar_estado`: a porta trava antes e sempre inclui `status_local` em `update_fields`. (`app/chamados/models.py:229`, `app/chamados/models.py:290`, `app/chamados/models.py:296`)

Também mudou o comportamento de recuperação: saves inteiros sem mudança e saves de campo em encerrados deixaram de tentar novamente um resolver anteriormente falho. A falha pode ser engolida pelo próprio S143; ocorrência concreta desse passivo é **[nao verificado]**. (`app/chamados/signals.py:100`, `app/chamados/signals.py:112`)

**3. PROCEDENTE — a catraca morde exemplos reais, mas não garante o enunciado amplo.**

A catraca detecta os exemplos “sem guarda” e “guarda depois da trava”. Entretanto, lê somente `chamados/signals.py`, reconhece três nomes de chamada e aceita qualquer chamada à guarda em linha anterior. (`app/chamados/tests/test_e1_v12.py:97`, `app/chamados/tests/test_e1_v12.py:124`, `app/chamados/tests/test_e1_v12.py:131`, `app/chamados/tests/test_e1_v12.py:135`, `app/chamados/tests/test_e1_v12.py:144`)

Além da cadeia indireta declarada no LIMITE, ela não vê:

| Forma | Motivo |
|---|---|
| Receiver em outro módulo, `pre_save`, `.connect()`, receiver assíncrono | Arquivo único, filtro por `FunctionDef` e decorator específico. `app/chamados/tests/test_e1_v12.py:101`, `app/chamados/tests/test_e1_v12.py:118`, `app/chamados/tests/test_e1_v12.py:131` |
| `sender=ChamadoColaborador` como classe; decorator/sinal qualificado ou com alias | Exige os identificadores literais e sender constante textual. `app/chamados/tests/test_e1_v12.py:103` |
| `select_for_update` direto, SQL cru, outros nomes ou aliases das travas | `_TRAVAM` contém apenas três nomes. `app/chamados/tests/test_e1_v12.py:97`, `app/chamados/tests/test_e1_v12.py:121` |
| Guarda invertida, resultado descartado, ramo não executado, guarda dentro de função interna | Não examina fluxo, polaridade nem se a guarda controla a trava; compara números de linha. `app/chamados/tests/test_e1_v12.py:120` |

A extração AST executada somente em memória confirmou: remoção da guarda é denunciada; inversão e chamada descartada passam. O resultado decorre do critério implementado em `app/chamados/tests/test_e1_v12.py:122`.

**Os testes `S143NaoInverteAOrdemTest` dariam vermelho por análise estática se a guarda fosse removida ou invertida:** o teste exige zero chamadas ao resolver na porta de texto e uma no save de status; os testes de ordem também observam o UPDATE antes da solicitação de Colaborador. Execução com banco nesta revisão: **[nao verificado]**. (`app/chamados/tests/test_e1_v12.py:53`, `app/chamados/tests/test_e1_v12.py:85`, `app/chamados/tests/test_e1_v12.py:89`, `app/chamados/services/disputa_emissao.py:1683`)

Há três ressalvas adicionais:

- A mensagem “com o mesmo status” não corresponde ao caso exercitado: antes salva `fechado`, depois chama `_gravar_estado('resolvido')`. (`app/chamados/tests/test_e1_v12.py:87`, `app/chamados/tests/test_e1_v12.py:91`, `app/chamados/tests/test_e1_v12.py:92`)
- O rastreador vê UPDATE e FOR UPDATE; não vê INSERT nem `executemany`, além dos limites declarados de FK e ordem intratabela. (`app/chamados/tests/test_e1_v9.py:47`, `app/chamados/tests/test_e1_v9.py:67`)
- `_inversoes` cobra apenas queda de rank; não exige Colaborador anterior nem rejeita PK desconhecida como `violacoes_linhas` faz. (`app/chamados/tests/test_e1_v12.py:30`, `app/chamados/tests/test_e1_v9.py:94`)

**4. NAO PROCEDE o rótulo falso anterior; o novo resumo corresponde ao command, com limites de precisão.**

Sem flag, mede e retorna sem aplicar; `--aplicar` retrata duplicatas e registra SILENCIO faltante de `admin_decidiu`. As projeções históricas de resposta/validação são medidas, mas não são gravadas por esse apply. O índice citado nasce na 0053. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:3`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:276`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:284`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:310`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:423`, `app/chamados/migrations/0053_pergunta_unica_por_familia.py:62`)

As recusas incluem horizonte divergente, mérito perdedor, candidatas escritas exportadas/sem dia e `sem_ato` fora do horizonte; qualquer recusa desfaz o apply inteiro. O rótulo não enumera todas elas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:193`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:219`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:260`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:273`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:297`, `app/config/crons.py:776`)

“Recusa competência exportada” significa recusar **escrever candidatas exportadas**. Um PAREI referente apenas às projeções medidas não impede por si só a chamada ao apply. “Com aval” é conduta, sem validação de aval pelo parser. “Ato de uma vez” e “antes do índice” descrevem finalidade; não há bloqueio automático de segunda execução ou execução pós-0053. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:310`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:350`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:429`)

Censo dos leitores do rótulo:

| Leitor | Efeito da alteração |
|---|---|
| `RotuloDoBackfillTest` | Lê esta string; confere migração e tokens, sem verificar toda a semântica operacional. `app/chamados/tests/test_e1_v12.py:174` |
| B6 dos crons | Usa as chaves do dicionário, sem consumir esta descrição. `app/chamados/tests/test_contract_crons.py:220` |
| Testes de outras chaves | Não consomem o valor alterado. `app/ponto/tests/test_aus_sem_fim.py:123`, `app/ponto/tests/test_o320_pauta_dos_discordantes.py:266` |
| Render do crontab | Percorre `CRONS`; não usa esta string. `app/config/crons.py:1846`, `app/config/crons.py:1926` |
| ARQUITETURA/MAPA | Usam entradas, estágios, papéis e contadores; não esse rótulo. `app/core/management/commands/gerar_diagrama.py:161`, `app/core/management/commands/gerar_diagrama.py:233`, `app/core/management/commands/gerar_diagrama.py:393`, `app/core/management/commands/gerar_diagrama.py:861` |
| Selos gerais de cron/tabuleiro | Cobram cadastro/papel; esta troca de descrição não altera esses insumos. `app/chamados/tests/test_contract_crons.py:221`, `app/core/tests/test_contract_tabuleiro.py:15` |

**5. PROCEDENTE a divergência de universos; NAO PROCEDE usar a sonda como prova de reprodução nesta cópia ou na frota.**

O dado anexado mostra, naquela execução, L0 de **10/09** recebendo a célula de **18/10** e o teste falhando. A correspondência daquela execução ao código exato deste commit é **[nao verificado]**: nesta cópia, `_nascer` ainda escolhe pelo `do_fato` imediato, e não pelo fecho. (`app/chamados/models.py:2703`)

A divergência estática permanece: `_nascer` consulta com episódio; `_candidata_reusavel` consulta sem episódio; os planos consultam o fecho. Assim, no universo sintético L/A/B do anexo, a abertura alfa pode ver L/A, enquanto o fecho encontra B. (`app/chamados/models.py:2577`, `app/chamados/models.py:2579`, `app/chamados/models.py:2703`, `app/chamados/services/declaracao_texto.py:242`, `app/chamados/services/fio.py:1051`, `app/chamados/services/fio.py:1204`)

**O índice permite B0 abaixo do horizonte e B1 acima**, pois sua condição só alcança datas desde 21/09/2026. Isso não demonstra formação desse par pela frota atual: a 0052 deixa o acervo sem origem; a porta atual trava o colaborador e reutiliza uma linha do mesmo episódio em qualquer dia. Existência real de B0 antigo com origem preenchida: **[nao verificado]**. (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:28`, `app/chamados/migrations/0053_pergunta_unica_por_familia.py:32`, `app/chamados/migrations/0053_pergunta_unica_por_familia.py:65`, `app/chamados/migrations/0052_pergunta_batida_origem.py:17`, `app/chamados/models.py:2669`, `app/chamados/models.py:2579`, `app/chamados/models.py:2748`)

Há uma fragilidade condicional mais simples no código atual: pedir o mesmo episódio com outro dia pode encontrar uma pergunta anterior, buscar a célula pelo dia pedido e salvar essa célula sem alterar o `data_evento` da pergunta encontrada. Chamador produtivo efetivamente fornecendo esses dias diferentes: **[nao verificado]**. (`app/chamados/models.py:2579`, `app/chamados/models.py:2693`, `app/chamados/models.py:2708`, `app/chamados/models.py:2747`)

Censo dos acessos à célula da Pergunta:

| Papel | Locais |
|---|---|
| Escrita persistente no nascimento/reuso | `app/chamados/models.py:2708`, `app/chamados/models.py:2747`, `app/chamados/models.py:2751` |
| Escrita persistente no rechaveamento, junto com o dia | `app/chamados/services/fio.py:960`, `app/chamados/services/fio.py:965` |
| Alteração somente em cópia na memória | `app/chamados/models.py:1467`, `app/chamados/models.py:1474` |
| SET_NULL por exclusão da célula | `app/chamados/models.py:2283`; exclusão produtiva efetiva **[nao verificado]** |
| Dia da disputa | `app/chamados/juizes.py:82`, `app/chamados/juizes.py:215` |
| Células da pergunta, com conferência de dia/colaborador | `app/chamados/juizes.py:634`, `app/chamados/juizes.py:638` |
| Memo de vitalidade | `app/chamados/juizes.py:816`, `app/chamados/juizes.py:830` |
| Marco no espelho, concordância da resposta e decisão administrativa | `app/chamados/juizes.py:904`, `app/chamados/juizes.py:1121`, `app/chamados/juizes.py:1166` |
| Portas alcançadas pela célula | `app/chamados/juizes.py:1320` |
| Outbox/célula a acordar | `app/chamados/models.py:2961`, `app/chamados/models.py:2974`, `app/chamados/signals.py:240` |
| Reconciliação, trilha e semeadura | `app/chamados/reconciliador.py:621`, `app/chamados/reconciliador.py:1145`, `app/chamados/reconciliador.py:1296` |
| A

[truncado em 20000 de 26013 caracteres]

## 07/11 20:44 · sessao 9e2390694b62 · - · - · origem=Ronald (tmux)

**Item 1: PROCEDENTE, residual demonstrável pelo código.**

O caminho agendado `apurar_furos_diarios` chama `agrupar_furos_sob_limbo`, que chama `agrupar_furo` sem adquirir previamente as travas do colaborador: `app/config/crons.py:1504`, `app/escala/management/commands/apurar_furos_diarios.py:18`, `app/chamados/services/limbo_folgas.py:308`, `app/chamados/services/limbo_folgas.py:326`.

Dentro de `agrupar_furo`, o `atomic` começa na linha 269; a chamada `renovar_contexto` na linha 270 escreve o Chamado por `save(update_fields=['contexto_json','atualizado_em'])`. Depois, a mesma transação chama `reapontar_resolvedora`, quando há perguntas vivas, ou `reconciliar`, quando o fechamento é permitido: `app/chamados/services/limbo_folgas.py:269`, `app/chamados/services/limbo_folgas.py:270`, `app/chamados/services/contexto.py:100`, `app/chamados/services/contexto.py:102`, `app/chamados/services/limbo_folgas.py:277`, `app/chamados/services/limbo_folgas.py:278`.

O primeiro caminho solicita Colaborador por `trava_do_colaborador`; o segundo chega a `_gravar_estado`, que solicita Colaborador → Disputa → Chamado. Portanto o Chamado já escrito precede a solicitação das travas anteriores na ordem global: `app/chamados/services/fio.py:855`, `app/chamados/models.py:633`, `app/chamados/models.py:637`, `app/chamados/models.py:290`, `app/chamados/models.py:2525`, `app/chamados/models.py:2532`, `app/chamados/models.py:2545`.

Há assim uma forma de ciclo: T1 nesse agrupamento segura Chamado e pede Colaborador; T2 em `_gravar_estado` segura Colaborador e pede o mesmo Chamado. A existência de candidatos e a ocorrência efetiva em produção são **[nao verificado]**; a sequência permitida está em `app/chamados/services/limbo_folgas.py:269`, `app/chamados/models.py:290`, `app/chamados/models.py:2525`, `app/chamados/models.py:2545`.

**Censo de escritores produtivos fora de `_gravar_estado`:**

| Escritor | Escrita |
|---|---|
| `rebaixar` | `app/chamados/models.py:484` |
| `escalar_urgencia` | `app/chamados/models.py:538` |
| `definir_prazo_sla` | `app/chamados/models.py:575` |
| `marcar_escalonado_dp` | `app/chamados/models.py:593` |
| `_rebaixar_sem_post_save` | `QuerySet.update`, `app/chamados/models.py:619` |
| `abrir` | `get_or_create`, `app/chamados/models.py:954`; trava prévia em `app/chamados/models.py:888` |
| `criar` | `create`, `app/chamados/models.py:1078`; atomic próprio em `app/chamados/models.py:1077` |
| `reescrever_texto` | `app/chamados/services/texto.py:35` |
| `anotar_contexto` | `app/chamados/services/contexto.py:77` |
| `renovar_contexto` | `app/chamados/services/contexto.py:102` |
| `retirar_contexto` | `app/chamados/services/contexto.py:124` |
| `fundir_contexto` | `app/chamados/services/contexto.py:159` |
| `apontar_justificativa` | `app/chamados/services/vinculos.py:88` |
| `apontar_celula` | `app/chamados/services/vinculos.py:99` |
| `apontar_ticket_glpi` | `app/chamados/services/vinculos.py:114` |
| `assumir_responsavel` | `app/chamados/services/vinculos.py:131` |
| `liberar_responsavel` | `app/chamados/services/vinculos.py:142` |
| `silenciar` | `app/chamados/services/arquivo.py:170` |
| `levantar` | `app/chamados/services/arquivo.py:208` |

Para **cada `save`/inserção** dessa tabela, o conjunto registrado é o mesmo: um `pre_save` e cinco `post_save`, registrados pela importação do app: `app/chamados/apps.py:8`, `app/chamados/apps.py:10`, `app/chamados/signals.py:53`, `app/chamados/signals.py:86`, `app/chamados/signals.py:174`, `app/chamados/signals.py:183`, `app/chamados/signals.py:265`, `app/chamados/signals.py:332`.

- `_cartorio_pre_save_chamado`: lê `status_local` por SELECT comum, sem trava explícita; escreve `_b5_status_antes`: `app/chamados/signals.py:175`, `app/chamados/signals.py:180`.
- `rebaixa_urgencia_ao_resolver`: fora de `raw`, para instância encerrada chama o UPDATE de urgência; não adquire `select_for_update`: `app/chamados/signals.py:68`, `app/chamados/signals.py:72`, `app/chamados/models.py:619`.
- S143: depois das duas guardas chama o resolver, que adquire Colaborador → Disputas → Chamados: `app/chamados/signals.py:92`, `app/chamados/signals.py:100`, `app/chamados/signals.py:111`, `app/chamados/services/disputa_emissao.py:1683`.
- `_cartorio_post_save_chamado`: usa `created` e `_b5_status_antes`, **não `update_fields`**; pode chamar `julgar_celula` sincronicamente. Esta pode fabricar perguntas e chegar à trava do universo do colaborador: `app/chamados/signals.py:184`, `app/chamados/signals.py:188`, `app/chamados/signals.py:199`, `app/ponto/services/cartorio.py:997`, `app/chamados/services/fabrica_da_celula.py:149`, `app/chamados/services/disputa_emissao.py:530`.
- `_isentos_post_save_chamado`: agenda o trabalho por `on_commit`, logo não o executa dentro da transação original: `app/chamados/signals.py:276`, `app/chamados/signals.py:289`.
- `_filho_morto_acorda_o_pai`: também agenda por `on_commit`: `app/chamados/signals.py:337`, `app/chamados/signals.py:352`.

O UPDATE direto de `_rebaixar_sem_post_save` não emite esses sinais; a própria implementação registra essa escolha: `app/chamados/models.py:601`, `app/chamados/models.py:619`. Não encontrei outro `bulk_update`/`bulk_create` produtivo de Chamado nesse censo; confirmação dinâmica é **[nao verificado]**.

Também há escritores históricos, por modelos de migração: saves de urgência em `app/chamados/migrations/0023_rebaixar_urgencia_resolvidos.py:10`, `app/chamados/migrations/0023_rebaixar_urgencia_resolvidos.py:17`, `app/chamados/migrations/0024_rebaixar_urgencia_resolvidos_regressao.py:11`, `app/chamados/migrations/0024_rebaixar_urgencia_resolvidos_regressao.py:19`; updates em `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `app/chamados/migrations/0038_a3_registrado_backfill.py:15`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/migrations/0039_a3_via_backfill.py:25`, `app/chamados/migrations/0039_a3_via_backfill.py:27`, `app/chamados/migrations/0039_a3_via_backfill.py:28`, `app/chamados/migrations/0039_a3_via_backfill.py:33`, `app/chamados/migrations/0039_a3_via_backfill.py:34`.

Outro residual condicional: uma porta de campo com instância cujo `status_local` difere do banco faz `_cartorio_pre_save_chamado` registrar o status atual do banco; mesmo sem escrever status, o `post_save` do cartório compara esse valor ao status antigo da instância e pode rejulgar após o UPDATE do Chamado. A nova guarda só protege S143: `app/chamados/signals.py:100`, `app/chamados/signals.py:180`, `app/chamados/signals.py:188`, `app/chamados/signals.py:199`.

## 07/11 20:43 · sessao 093afe6629cd · - · - · origem=Ronald (tmux)

3. **PROCEDENTE, catraca de alcance restrito; NAO PROCEDE que não morda nada.** `CatracaS143Test` lê exclusivamente `app/chamados/signals.py`; exige somente a presença textual da definição S143 e o resultado AST vazio (`app/chamados/tests/test_e1_v12.py:129-133`). Os dois exemplos negativos de receiver sem guarda e guarda depois da trava realmente são denunciados pelo algoritmo (`app/chamados/tests/test_e1_v12.py:135-150`).

Além da cadeia indireta declarada como LIMITE, ficam invisíveis:

- Receiver em outro arquivo; receiver `pre_save`; registro por `.connect()`; `@receiver` ou `post_save` qualificados/aliased; `sender=ChamadoColaborador` como classe; `AsyncFunctionDef`: o filtro exige `FunctionDef`, decorator Call cujo `func.id == receiver`, primeiro argumento `id == post_save`, sender Constant terminando em `ChamadoColaborador` (`app/chamados/tests/test_e1_v12.py:101-119`, `:131`).
- `select_for_update()` direto, SQL cru e qualquer outro nome de trava; alias dos três nomes reconhecidos: `_TRAVAM` contém só `resolver_perguntas_por_chamado_resolvido`, `trava_na_ordem`, `trava_do_colaborador`; o algoritmo só compara o nome final da chamada (`app/chamados/tests/test_e1_v12.py:97`, `:120-125`).
- Guarda invertida, resultado da guarda descartado, guarda sob ramo não executado ou dentro de função interna: basta existir uma chamada com o nome esperado em linha anterior; não analisa controle de fluxo, argumento, retorno, dominância nem polaridade (`app/chamados/tests/test_e1_v12.py:120-125`).
- Pode também dar falso positivo por funções internas nunca chamadas: `ast.walk(fn)` percorre seus corpos (`app/chamados/tests/test_e1_v12.py:120`).

Executado **somente em memória**, via `python3 -B`, extraindo AST das funções do teste: atual `[]`; guarda removida denunciou S143; guarda invertida `[]`; guarda descartada `[]`; `select_for_update` direto `[]`; sender classe `[]`. A reprodução deriva exatamente dos filtros acima; sem arquivo/log gerado (`app/chamados/tests/test_e1_v12.py:97-125`).

**S143NaoInverteAOrdemTest daria vermelho se removesse ou invertesse a guarda**, por análise do código: portas de campo em encerrado passariam pelo resolver e pediriam Colaborador depois do UPDATE do Chamado (`app/chamados/tests/test_e1_v12.py:53-75`; `app/chamados/services/disputa_emissao.py:1682-1684`). Há também verificação explícita `call_count == 0` na porta de texto e `==1` no save de status; inversão viola ambos (`app/chamados/tests/test_e1_v12.py:80-93`). Execução desses testes com banco: **[nao verificado]**.

Ressalvas dos testes: [nao verificado]

- A mensagem “_gravar_estado com o mesmo status” é falsa para o caso exercitado: ele salva `fechado` antes, depois chama `_gravar_estado('resolvido')`; não prova repetição do mesmo valor (`app/chamados/tests/test_e1_v12.py:87-92`).
- Rastreador só vê UPDATE e FOR UPDATE, ignora INSERT e `executemany`; os limites adicionais declarados incluem FK FOR KEY SHARE e ordem de pk intratabela (`app/chamados/tests/test_e1_v9.py:47-52`, `:67-89`).
- `_inversoes` cobra apenas queda de rank; não cobra pk desconhecida nem Colaborador anterior, ao contrário de `violacoes_linhas` (`app/chamados/tests/test_e1_v12.py:30-40`; `app/chamados/tests/test_e1_v9.py:94-109`). Não examina locks adquiridos antes de entrar no wrapper (`app/chamados/tests/test_e1_v9.py:90-91`).

4. **NAO PROCEDE o rótulo falso anterior; a nova descrição corresponde ao resumo operacional, com limites de precisão.** Novo texto está em `app/config/crons.py:775-779`; parser tem somente `--aplicar`, sem ele imprime “nada escrito” e retorna (`app/chamados/management/commands/backfill_decisoes_pergunta.py:310-312`, `:423-425`).

O que efetivamente faz:

- Mede possíveis linhas de resposta/validação, usando `DecisaoPergunta.linhas_da_projecao_sem_linha`; não grava esse backfill de projeções (`app/chamados/management/commands/backfill_decisoes_pergunta.py:3-7`, `:42-89`).
- Apply grava `SILENCIO` para `admin_decidiu` sem ato e retrata duplicatas via `retratar_para_canonica → retratar_duplicata` (`app/chamados/management/commands/backfill_decisoes_pergunta.py:276-289`; `app/chamados/services/fio.py:1070-1076`).
- Recusa horizonte divergente, mérito perdido, candidatas escritas de competência exportada ou sem dia, e `sem_ato` fora do horizonte; qualquer recusa desfaz o atomic inteiro (`app/chamados/management/commands/backfill_decisoes_pergunta.py:193-221`, `:251-275`, `:297-301`). CLI traduz essas recusas em rc 5/3/4/6 (`:430-446`).
- Acervo abaixo do horizonte não é escrito; perdedoras são contadas (`app/chamados/management/commands/backfill_decisoes_pergunta.py:238-245`).
- Índice de episódio nasce de fato em 0053 (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:62-66`).

“Recusa competência exportada” deve ser lido como **recusa escrever candidatas exportadas**, não como “qualquer PAREI exibido bloqueia o command”: projeções apenas medidas podem imprimir PAREI em `:350-357`, mas `paradas` não impede a chamada `aplicar(grupos, sem_ato)` em `:429` (`app/chamados/management/commands/backfill_decisoes_pergunta.py:350-357`, `:429`). “Com aval” é requisito de conduta; o parser não possui flag/validação de aval (`:310-312`). As demais recusas não foram enumeradas no rótulo; ele não declara exaustividade (`app/config/crons.py:776-779`).

“Antes do índice” descreve a finalidade histórica, não um bloqueio de execução pós-0053: command não consulta presença do índice antes de medir/aplicar (`app/chamados/management/commands/backfill_decisoes_pergunta.py:327-429`). Não há exclusão automática desse ato após primeiro uso; idempotência é prevista (`:173-174`, `:267-269`).

**Censo dos leitores de `FORA_DE_PIPELINE`:**

- Único leitor do **valor desta chave** é o teste novo `RotuloDoBackfillTest`: lê `backfill_decisoes_pergunta`, confere migração citada, ausência de “so dry”/“nao escreve” e presença de `--aplicar`; não verifica completude das recusas, destino das escritas ou autorização (`app/chamados/tests/test_e1_v12.py:174-193`).
- B6 usa só chaves do dict para casa de command e entradas podres; string nova não altera resultado (`app/chamados/tests/test_contract_crons.py:220-237`).
- Outros leitores executáveis do dict são testes de **outras chaves**: membership `ausencia_bloqueante_sem_fim` (`app/ponto/tests/test_aus_sem_fim.py:122-125`) e membership/tamanho do motivo de `pautas_discordantes` (`app/ponto/tests/test_o320_pauta_dos_discordantes.py:265-271`).
- Demais ocorrências de `FORA_DE_PIPELINE` no código são comentários/docstrings, sem consumo do valor desta chave (`app/config/crons.py:493`, `:1389`, `:1533`, `:1545`, `:1591`; `app/ponto/management/commands/pautas_discordantes.py:34`; `app/chamados/management/commands/validar_classe_a.py:57`; `app/folha/management/commands/lavrar_retrato_folha.py:20`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:13`; `app/relatorios/management/commands/diff_extrato_parcial.py:14`; `app/ponto/services/ausencia.py:230`).

**Render, desenhos e selos gerais não mudam por esta string:**

- Render só percorre `CRONS` e concatena `linhas()` (`app/config/crons.py:1846-1876`, `:1926-1927`). CLI delega a esse render (`app/core/management/commands/crons.py:23`, `:49-50`); selo host compara render ao crontab (`bin/crons.sh:24-33`).
- Fonte de entradas agendadas continua `list(CRONS)+list(ENCADEADOS)` (`app/config/crons.py:1771-1782`).
- ARQUITETURA pipeline lê CRONS/ENCADEADOS/ESTAGIOS, tabuleiro lê papeis e `todas_as_entradas`, vigias lê CRONS/papeis/agendamentos/contadores; nenhum desses lê rótulo FORA (`app/core/management/commands/gerar_diagrama.py:161-205`, `:229-243`, `:393-397`).
- Há leitura textual do arquivo de crons por contadores, mas só regex de comentários `# contador=...` e entradas C/RAW/constantes; string nova não casa esses padrões (`app/core/management/commands/gerar_diagrama.py:372-389`).
- MAPA lê PAPEIS e gera ponteiros estruturais; rótulo não entra (`app/core/management/commands/gerar_diagrama.py:861-908`). Selo de desenho gera ambos (`:911-920`).
- Selos de tabuleiro cobram papel/juízes declarados, não essa string (`app/core/tests/test_contract_tabuleiro.py:15-28`).
- Leis L-060/061/062/101 dizem papéis, consumidor da lâmpada, condições de silêncio e prazo; o diff altera uma descrição de ato fora da rotina, sem alterar cadastro de papel, agenda ou emissor (`app/docs/LEIS.md:94-96`, `:127`; `app/config/crons.py:775-779`).

## 07/11 20:43 · sessao 4586ceea7d83 · - · - · origem=Ronald (tmux)

Item 5: **PROCEDENTE** a divergência de universos permanece; **NAO PROCEDE** atribuir ao código atual a eleição L0 específica da sonda, sem evidência do patch que aquela execução usou. `_nascer` escolhe em `do_fato(..., batida_origem_id=_bo)` (`app/chamados/models.py:2703`); o plano expande pelo fecho (`app/chamados/services/fio.py:1051`). No cenário L1/A1/B1 do anexo, a consulta alfa vê L1/A1; só a expansão posterior encontra B1 e atravessa beta (`app/chamados/models.py:2577`, `app/chamados/models.py:2579`, `app/chamados/services/fio.py:1204`). A sonda fornecida relata L0 recebendo celula de d1, mas ligação dessa execução ao conteúdo exato deste commit é **[nao verificado]**.

A dupla B0 abaixo do horizonte/B1 acima é **permitida pelo índice**, porque a condição contém `data_evento >= 2026-09-21` (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:28`, `app/chamados/migrations/0053_pergunta_unica_por_familia.py:31`, `app/chamados/migrations/0053_pergunta_unica_por_familia.py:65`). Porém, isso **não demonstra sua formação na frota atual**: 0052 só acrescenta FK nullable, sem preencher o acervo (`app/chamados/migrations/0052_pergunta_batida_origem.py:17`); e a porta atual serializa colaborador e reutiliza a primeira linha do episódio, independentemente do dia, em vez de criar a segunda (`app/chamados/models.py:2667`, `app/chamados/models.py:2579`, `app/chamados/models.py:2748`). A existência real de B0 com origem no acervo é **[nao verificado]**.

Há uma fragilidade própria do código atual, sem precisar do fecho: se `abrir` receber o mesmo episódio com outro dia, encontra a linha anterior por episódio sem filtro de dia, busca a célula pelo **dia pedido** e grava essa célula na linha encontrada, preservando `data_evento` da linha (`app/chamados/models.py:2579`, `app/chamados/models.py:2693`, `app/chamados/models.py:2708`, `app/chamados/models.py:2747`). Um chamador produtivo efetivamente fornecendo dias diferentes para essa mesma batida é **[nao verificado]**. O emissor prioriza `contexto[motivo]['data']` e só consulta o turno da última batida se aquele dia estiver ausente (`app/chamados/services/disputa_emissao.py:371`, `app/chamados/services/disputa_emissao.py:378`).

`batida_origem` só é derivada no nascimento; não encontrei escritor posterior nem `_fixar_episodio_da_canonica` nesta cópia (`app/chamados/models.py:2696`, `app/chamados/models.py:3149`, `app/chamados/models.py:3177`). Rechaveamento altera dia e célula juntos, verifica colisão e não cria outra pergunta (`app/chamados/services/fio.py:947`, `app/chamados/services/fio.py:954`, `app/chamados/services/fio.py:959`, `app/chamados/services/fio.py:965`). Reapontamento só data linha anteriormente sem dia (`app/chamados/services/fio.py:838`).

Censo de `PerguntaDisputa.celula`:

| Papel | Locais |
|---|---|
| Escritor persistente: nascimento/reuso | `app/chamados/models.py:2708`, `app/chamados/models.py:2747`, `app/chamados/models.py:2751` |
| Escritor persistente: rechaveamento | `app/chamados/services/fio.py:960`, `app/chamados/services/fio.py:965` |
| Escrita somente em cópia em memória | `app/chamados/models.py:1467`, `app/chamados/models.py:1474` |
| SET_NULL caso a célula seja excluída | `app/chamados/models.py:2283`; exclusão produtiva efetiva **[nao verificado]** |
| Dia da disputa | `app/chamados/juizes.py:82`, `app/chamados/juizes.py:215` |
| Alimentação da lei; confere identidade dia/colaborador | `app/chamados/juizes.py:634`, `app/chamados/juizes.py:638` |
| Memo de vitalidade | `app/chamados/juizes.py:816`, `app/chamados/juizes.py:830` |
| Lâmpada/marco e concordância da resposta | `app/chamados/juizes.py:904`, `app/chamados/juizes.py:1121` |
| Decisão administrativa na vitalidade | `app/chamados/juizes.py:1166` |
| Portas alcançáveis por célula | `app/chamados/juizes.py:1320`, `app/chamados/juizes.py:1323` |
| Outbox e célula a acordar | `app/chamados/models.py:2961`, `app/chamados/models.py:2974`, `app/chamados/signals.py:240` |
| Absolvição, trilha e semeadura | `app/chamados/reconciliador.py:621`, `app/chamados/reconciliador.py:627`, `app/chamados/reconciliador.py:1145`, `app/chamados/reconciliador.py:1296` |
| Agrupamentos do vigia de fio | `app/chamados/services/vigia_fio.py:400`, `app/chamados/services/vigia_fio.py:466`, `app/chamados/services/vigia_fio.py:486`, `app/chamados/services/vigia_fio.py:614` |
| Filtro e relatório do vigia de carimbo | `app/chamados/services/vigia_carimbo.py:100`, `app/chamados/services/vigia_carimbo.py:131` |
| Snapshot/verificação da migração de dia | `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:269`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:341` |
| Auditoria de invariantes | `app/chamados/management/commands/auditar_invariantes_chamados.py:1783` |

Reincidências pertinentes: o juiz da canônica continua único, mas seus consumidores entregam universos diferentes — **PROCEDENTE derivação paralela do universo**, não segundo algoritmo de eleição (`app/chamados/services/fio.py:989`, `app/chamados/models.py:2703`, `app/chamados/services/declaracao_texto.py:242`, `app/chamados/services/fio.py:1219`). A leitura do relógio dentro da porta permanece **PROCEDENTE**, preexistente ao diff (`app/chamados/services/fio.py:1087`). A descrição `celula=null → sem data` não é universal: o nascimento pode ter data e não encontrar célula — **PROCEDENTE ambiguidade documental**, além da possibilidade condicional de célula/dia divergirem descrita acima (`app/chamados/models.py:2282`, `app/chamados/models.py:2693`, `app/chamados/models.py:2751`).

## 07/11 19:53 · sessao 8788c795d4ac · FIO-E0-V4-E1 · verificador · origem=Code

**Veredito: cura parcial.** O veto passou a consultar o fecho; permanecem ressalvas na propriedade declarada, nos resumos, no alcance das catracas e no censo de leitores. (`app/chamados/services/fio.py:1051`; `app/chamados/services/fio.py:1191`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:414`; `app/chamados/tests/test_e1_v7_catracas.py:154`; `app/chamados/services/fio.py:1144`)

Vínculo independente da cópia com commit/pai, reexecução da SUITE, histórico RED→GREEN/mutante e ocorrência produtiva dos contraexemplos: **[nao verificado]**. [nao verificado]

1. **NAO PROCEDE a antiga dependência do recorte para `episodios_unidos`, sob estado estável e linhas elegíveis do mesmo fato.** O plano calcula os episódios sobre o fecho; continua classificando somente as perdedoras recebidas, com precedência mérito → episódios unidos → sem episódio → retrata. A mudança pode impedir retratações antes permitidas; não acrescenta retratações ao plano anterior. (`app/chamados/services/fio.py:1043`; `app/chamados/services/fio.py:1051`; `app/chamados/services/fio.py:1054`)

| Chamador | Resultado da conferência |
|---|---|
| `retratar_para_canonica` | Recalcula o plano antes de escrever; o veto alcança o fecho mesmo recebendo poucas perdedoras. (`app/chamados/services/fio.py:1075`) |
| `_colisao_ao_datar` | Entrega o universo prospectivo, preservando `p` com a data nova em memória; qualquer categoria impeditiva recusa a datação. (`app/chamados/services/fio.py:890`; `app/chamados/services/fio.py:904`) |
| `_migrar_sob_trava` | Continua entregando conflito do destino + `p`, mas o plano expande esse recorte; se outra viva preservada impedir a migração da canônica, `p` permanece no velho. (`app/chamados/services/fio.py:1395`; `app/chamados/services/fio.py:1400`; `app/chamados/services/fio.py:1407`) |
| Backfill apply | Entrega perdedoras do horizonte; o fecho recupera episódios ligados por linhas abaixo dele. A porta replaneja antes da escrita. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:241`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:284`) |
| Backfill DRY | Usa o mesmo plano sobre as linhas do horizonte. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:385`) |
| Dedup DRY / apply | DRY usa o plano; apply delega ao backfill. (`app/chamados/management/commands/dedup_perguntas_fato.py:81`; `app/chamados/management/commands/dedup_perguntas_fato.py:105`) |

**PROCEDENTE a ressalva de custo e alcance das travas.** Cada PK alcançada é expandida uma vez por execução do fecho; num estado finito e estável, termina. Há consulta de vizinhos por expansão, sem limite explícito de tamanho; o plano acrescenta esse trabalho inclusive durante transações. Latência efetiva: **[nao verificado]**. (`app/chamados/services/fio.py:1181`; `app/chamados/services/fio.py:1204`; `app/chamados/services/fio.py:1206`)

O fecho faz leitura comum, não trava suas linhas. Apply toma Colaborador, calcula o universo, trava Disputas/Perguntas e depois relê; migração e colisão dependem da trava do Colaborador e do protocolo dos escritores. A porta direta não adquire trava por si. **NAO PROCEDE corrida ou inversão demonstrada somente pela nova consulta; não cabe afirmar que toda linha lida está bloqueada.** (`app/chamados/management/commands/backfill_decisoes_pergunta.py:210`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:213`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/services/fio.py:1357`; `app/chamados/services/fio.py:888`; `app/chamados/services/fio.py:1075`)

2. **NAO PROCEDE quebra localizada de chamador produtivo por instância não salva.** Os caminhos encontrados trabalham com perguntas recuperadas do banco; `universo_do_grupo` também materializa suas sementes por consulta. O `assert` rejeita explicitamente `pk=None`; o teste cobre essa rejeição. (`app/chamados/services/fio.py:1380`; `app/chamados/reconciliador.py:1628`; `app/chamados/services/declaracao_texto.py:333`; `app/chamados/services/fio.py:1217`; `app/chamados/services/fio.py:1200`; `app/chamados/tests/test_e1_v11.py:130`)

**PROCEDENTE ressalva na propriedade do docstring:** persistência e `conversa=False` não bastam. Para uma pergunta persistida com `via_resolucao='duplicata'`, `universo_da_linha(p)` preserva a semente; `universo_do_grupo([p.pk])` a exclui por `VIAS_FORA_DO_FATO`. A igualdade exige também elegibilidade da via. A exceção da conversa e a datação somente em memória agora estão declaradas. (`app/chamados/services/fio.py:1191`; `app/chamados/services/fio.py:1202`; `app/chamados/services/fio.py:1217`; `app/chamados/services/fio.py:1227`; `app/chamados/models.py:2262`)

3. **NAO PROCEDE dupla classificação das perdedoras num universo estável; PROCEDENTE apresentação ainda incompleta.** Apply separa perdedoras sem dia, datadas abaixo do horizonte e categorias do plano; dedup reproduz essa separação. A canônica é excluída das perdedoras, e `canonica_sem_episodio` é marcador separado. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:240`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:243`; `app/chamados/management/commands/dedup_perguntas_fato.py:84`; `app/chamados/services/fio.py:1041`)

**Backfill DRY ainda omite os três informativos** `abaixo_do_horizonte`, `perdedora_sem_dia` e `canonica_sem_episodio`. Dedup os imprime em DRY/apply; backfill os apresenta no APLICADO. Portanto, a afirmação de resumos equivalentes não vale para todos os modos. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:377`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:414`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:447`; `app/chamados/management/commands/dedup_perguntas_fato.py:113`; `app/chamados/management/commands/dedup_perguntas_fato.py:118`)

Os universos globais também diferem: dedup aplica `--colab`, `--ate` e `--limite`; backfill busca grupos sem esses recortes. Comparação numérica exige igualar os grupos. (`app/chamados/management/commands/dedup_perguntas_fato.py:58`; `app/chamados/management/commands/dedup_perguntas_fato.py:64`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:360`)

Os totais não constituem uma partição global de perguntas: uma BATIDA sem origem pode aparecer em `fora_do_indice` e numa colisão; uma perdedora `admin_decidiu` sem resposta/ato pode receber silêncio e depois retratação, contando dois atos da mesma PK. Isso não é dupla classificação de perdedora. Na recusa, rollback zera escrita e informativos e conserva as causas. (`app/chamados/services/fio.py:1261`; `app/chamados/services/fio.py:1292`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:266`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:276`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:284`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:300`)

4. **PROCEDENTE alcance residual das catracas fora do LIMITE declarado.** As formas corrigidas possuem asserções específicas, mas restam estes contraexemplos estáticos. (`app/chamados/tests/test_e1_v11.py:182`; `app/chamados/tests/test_e1_v11.py:197`; `app/chamados/tests/test_e1_v11.py:202`; `app/chamados/tests/test_e1_v7_catracas.py:36`)

| Catraca | Contraexemplo residual |
|---|---|
| A | `SELECT '--' FROM chamados_perguntadisputa FOR UPDATE` escapa: a regex trata o conteúdo da string SQL como comentário e remove a trava. Não é SQL dinâmico. (`app/chamados/tests/test_e1_v7_catracas.py:147`; `app/chamados/tests/test_e1_v7_catracas.py:154`) |
| A | `SELECT 'FOR UPDATE' FROM chamados_perguntadisputa` produz falso positivo sintético: o detector reconhece texto dentro da string SQL como trava. Ocorrência desse falso positivo no repo: **[nao verificado]**. (`app/chamados/tests/test_e1_v7_catracas.py:155`) |
| B | `x=PerguntaDisputa.do_fato(...); y=x; y.first()` escapa: a origem `do_fato` não é propagada transitivamente entre nomes. (`app/chamados/tests/test_e1_v7_catracas.py:221`; `app/chamados/tests/test_e1_v7_catracas.py:247`) |
| B | `j,z=canonica_do_grupo,0; j([p for p in ps if p.ok])` escapa: a propagação do juiz não trata desempacotamento. (`app/chamados/tests/test_e1_v7_catracas.py:227`) |

**NAO PROCEDE ausência geral de mordida.** V11 verifica estado persistido no apply e na migração, e exige retratação no controle sem ponte. `UmPlanoPorUniversoTest` exige duas chamadas totais — apply + porta — e uma retratação; não prova trava de linhas. (`app/chamados/tests/test_e1_v11.py:89`; `app/chamados/tests/test_e1_v11.py:103`; `app/chamados/tests/test_e1_v11.py:117`; `app/chamados/tests/test_e1_v11.py:156`)

`PlanoParticionaTest` prova cobertura/disjunção com fecho identidade, como declara; a integração com o banco fica nos testes V11. O limbo ganhou controle factual positivo. O teste de contadores prova os três nomes, mas só exige valor positivo para `abaixo_do_horizonte`; não cobre apply nem valores positivos dos outros dois. (`app/chamados/tests/test_e1_v10.py:123`; `app/chamados/tests/test_e1_v10.py:135`; `app/chamados/tests/test_e1_v10.py:265`; `app/chamados/tests/test_e1_v11.py:173`; `app/chamados/tests/test_e1_v11.py:175`)

**NAO PROCEDE relógio solto novo localizado nesses testes:** datas literais e classes concretas de banco congeladas. (`app/chamados/tests/test_e1_v11.py:26`; `app/chamados/tests/test_e1_v11.py:74`; `app/chamados/tests/test_e1_v11.py:135`; `app/chamados/tests/test_e1_v11.py:162`)

5. **PROCEDENTE: o censo continua incompleto.** Além dos leitores agora enumerados, faltam ao menos os seguintes leitores que decidem ou contam pelo campo cru. (`app/chamados/services/fio.py:1126`; `app/chamados/services/fio.py:1144`)

| Leitor omitido | Decisão/contagem |
|---|---|
| Calendário | Indexa perguntas respondidas vivas por `data_evento` e usa o mapa para permitir último recurso. A restrição à competência anterior limita quando uma conversa interfere; ocorrência produtiva: **[nao verificado]**. (`app/colaboradores/services/calendario.py:473`; `app/colaboradores/services/calendario.py:477`; `app/colaboradores/services/calendario.py:738`) |
| `mapa_divergencia` | Conta dias de Pergunta como cobertura de furo, sem excluir conversa. (`app/ponto/management/commands/mapa_divergencia.py:270`; `app/ponto/management/commands/mapa_divergencia.py:275`) |
| Vigia de carimbos | Separa aberta/exportada pela competência de `p.data_evento`; o filtro não exclui conversa. (`app/chamados/services/vigia_fio.py:806`; `app/chamados/services/vigia_fio.py:816`; `app/chamados/services/relavra_via.py:181`) |
| Aceite de folga | Lê o dia cru e declara folga; a validação desvia diretamente para essa porta. (`app/chamados/services/validacao.py:269`; `app/chamados/services/validacao.py:321`; `app/chamados/services/validacao.py:412`) |
| Ausência pela resposta | Cria Ausência e seleciona irmãs pelo dia cru; a resposta chama diretamente esse caminho. (`app/chamados/services/materializacao.py:517`; `app/chamados/services/materializacao.py:521`; `app/chamados/services/materializacao.py:532`; `app/chamados/services/disputa_emissao.py:1422`) |
| Outbox da Pergunta | Transporta a data sem discriminador e pode escolher célula por colaborador/dia para emitir evento. (`app/chamados/models.py:2961`; `app/chamados/models.py:2977`; `app/chamados/models.py:2981`; `app/chamados/signals.py:221`) |

6. **Reincidências:**

| Classe | Veredito |
|---|---|
| Segundo juiz registrado da canônica | **NAO PROCEDE localizado:** a eleição permanece em `canonica_do_grupo`; o registro aponta as autoridades de Chamado. (`app/chamados/services/fio.py:989`; `app/core/juizes.py:260`) |
| Segundo escritor de duplicata | **NAO PROCEDE localizado:** porta → retratador → carimbo de identidade. (`app/chamados/services/fio.py:1075`; `app/chamados/services/fio.py:1076`; `app/chamados/services/fio.py:1094`) |
| Relógio fora da borda | **PROCEDENTE residual:** retratador, remoção de motivo e lavra ainda consultam relógio internamente. (`app/chamados/services/fio.py:1088`; `app/chamados/models.py:1994`; `app/chamados/veredito_lavrado.py:112`; `app/chamados/veredito_lavrado.py:228`) |
| Derivação paralela | **PROCEDENTE divergência de universo:** abertura/declaração elegem sobre um passo de `do_fato`; reconciliação usa fecho. Uma ponte legada pode mudar a canônica; isso não demonstra retratação indevida, pois o plano preserva o legado. (`app/chamados/models.py:2703`; `app/chamados/services/declaracao_texto.py:242`; `app/chamados/services/fio.py:1219`; `app/chamados/services/fio.py:1058`) |
| Selo/RED sem mordida | **NAO PROCEDE genericamente; PROCEDENTE cobertura incompleta**, conforme os contraexemplos e limites do item 4. (`app/chamados/tests/test_e1_v11.py:117`; `app/chamados/tests/test_e1_v11.py:175`; `app/chamados/tests/test_e1_v7_catracas.py:154`) |
| Ordem de trava | **NAO PROCEDE em `remover_motivo`; PROCEDENTE residual em texto/S143:** UPDATE de Chamado pode anteceder a aquisição de Colaborador pela resolução disparada no sinal. (`app/chamados/models.py:1996`; `app/chamados/models.py:2031`; `app/chamados/services/texto.py:35`; `app/chamados/signals.py:92`; `app/chamados/services/disputa_emissao.py:1683`) |
| Pouso antes do pré-requisito | O requisito existe no backlog; cumprimento e cronologia efetiva: **[nao verificado]**. (`app/docs/BACKLOG.md:1827`) |
| Campo com dois significados | **PROCEDENTE residual:** dia factual/civil e texto de recusa/referência à canônica ainda exigem discriminação pelos leitores. (`app/chamados/models.py:2643`; `app/chamados/models.py:2654`; `app/chamados/services/fio.py:1095`; `app/chamados/services/validacao.py:584`) |

7. **PROCEDENTE incompatibilidade residual de significado; mapa de escrita/leitura:**

| Campo/valor | QUEM ESCREVE | QUEM LÊ / significado |
|---|---|---|
| `via_resolucao` | Escritor do carimbo, portas de carimbo/descarimbo, lote, encerramento por fato e relavra. (`app/chamados/models.py:2830`; `app/chamados/models.py:2915`; `app/chamados/models.py:2941`; `app/chamados/models.py:2993`; `app/chamados/models.py:3004`; `app/chamados/services/relavra_via.py:125`) | Mérito, idempotência, identidade, C6, índices, cartório e apresentação. `admin_decidiu` com resposta é mérito protegido; `duplicata` sai do fato. (`app/chamados/services/fio.py:1007`; `app/chamados/services/fio.py:1085`; `app/chamados/models.py:2934`; `app/chamados/juizes.py:1164`; `app/chamados/models.py:3213`; `app/ponto/services/cartorio.py:1599`) |
| `recusa_motivo` | Retratação para a canônica, recusa humana, escape exigindo admin, pendência e materializador. (`app/chamados/services/fio.py:1095`; `app/chamados/services/validacao.py:584`; `app/chamados/services/disputa_emissao.py:1432`; `app/chamados/services/materializacao.py:847`; `app/chamados/services/materializacao.py:1099`) | Validação, explicador, auditoria, regex do flip e telas. Não significa exclusivamente recusa de mérito. (`app/chamados/services/validacao.py:192`; `app/chamados/services/explicador.py:90`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`; `app/ponto/services/flip_auto.py:132`; `app/templates/chamados/partials/modal_fio.html:225`) |
| `data_evento` | Nascimento factual recebe dia do turno; conversa recebe dia civil; reapontamento preenche NULL; rechaveamento altera dia. (`app/chamados/models.py:2643`; `app/chamados/models.py:2654`; `app/chamados/models.py:2751`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:959`) | Identidade, horizonte, fecho, competência e leitores do item 5. O significado depende de `conversa`; os leitores crus não preservam essa distinção. (`app/chamados/models.py:2610`; `app/chamados/services/fio.py:1147`; `app/chamados/services/fio.py:1181`; `app/chamados/veredito_lavrado.py:229`; `app/colaboradores/services/calendario.py:477`) |
| `conversa` | Portas factual/conversa e nascimento; migração histórica adiciona default `False`. (`app/chamados/models.py:2643`; `app/chamados/models.py:2654`; `app/chamados/models.py:2752`; `app/chamados/migrations/0051_decisao_pergunta.py:99`) | Identidade, índices, horizonte, fecho e C6 usam o discriminador. A migração declara conversas antigas indistinguíveis; classificação real do acervo fornecido: **[nao verificado]**. (`app/chamados/models.py:2611`; `app/chamados/models.py:3213`; `app/chamados/services/fio.py:1147`; `app/chamados/services/fio.py:1178`; `app/chamados/juizes.py:336`; `app/chamados/migrations/0051_decisao_pergunta.py:5`) |
| `batida_origem` | Explícita/derivada no nascimento; derivação na inserção confere família e colaborador. (`app/chamados/models.py:2696`; `app/chamados/models.py:3177`; `app/chamados/models.py:3190`; `app/chamados/models.py:3196`) | Identidade do episódio, índice, veto, vizinhos, agrupamento e contadores fora do índice. O episódio pode atravessar dias; contexto posterior da Disputa não substitui essa origem histórica. (`app/chamados/models.py:2579`; `app/chamados/models.py:3218`; `app/chamados/services/fio.py:1020`; `app/chamados/services/fio.py:1109`; `app/chamados/services/fio.py:1292`; `app/chamados/models.py:3185`) |
| Retorno de `aplicar` | Três escalares de atos, listas de PKs informativas/categorias e listas de causas; rollback zera os primeiros. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:281`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:291`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:300`) | Os dois comandos apresentam o retorno; dedup separa causas por `CONTADORES_DO_APPLY`. `canonica_sem_episodio` é marcador, não outra perdedora. (`app/chamados/management/commands/dedup_perguntas_fato.py:107`; `app/chamados/management/commands/dedup_perguntas_fato.py:111`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:430`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:447`; `app/chamados/services/fio.py:1041`) |

Há ainda contradição concreta na apresentação: a tela traduz `duplicata` como **“pergunta repetida do mesmo dia”**, embora BATIDA seja identificada por episódio+motivo e possa atravessar dias. O texto do retratador também imprime a data da perdedora, sem demonstrar igualdade com a data da canônica. (`app/templates/chamados/partials/_card_atendimento.html:186`; `app/chamados/services/fio.py:1109`; `app/chamados/models.py:2579`; `app/chamados/services/fio.py:1095`)

## 07/11 19:51 · sessao 19491c2ba8cb · - · - · origem=Ronald (tmux)

**Item 3 — NAO PROCEDE dupla classificação das perdedoras; PROCEDENTE ressalva de apresentação/universo.**

- Em um grupo estável, perdedoras repartem-se entre NULL, abaixo do horizonte e uma categoria do plano; a canônica fica fora. Listas do retorno são deduplicadas por PK. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:240`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:243`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:303`; `app/chamados/services/fio.py:1043`)
- Dedup imprime os três novos informativos em DRY e apply. Backfill **DRY ainda não imprime** `abaixo_do_horizonte`, `perdedora_sem_dia`, `canonica_sem_episodio`; backfill APLICADO imprime. A frase “os dois resumos nomeiam os mesmos” vale somente na comparação com o APLICADO. (`app/chamados/management/commands/dedup_perguntas_fato.py:72`; `app/chamados/management/commands/dedup_perguntas_fato.py:85`; `app/chamados/management/commands/dedup_perguntas_fato.py:111`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:375`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:414`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:447`)
- Universos globais dos comandos diferem deliberadamente: dedup aplica `--colab`, `--ate`, `--limite`; backfill chama agrupamento sem esses recortes. Comparação numérica exige os mesmos grupos. (`app/chamados/management/commands/dedup_perguntas_fato.py:45`; `app/chamados/management/commands/dedup_perguntas_fato.py:58`; `app/chamados/management/commands/dedup_perguntas_fato.py:64`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:360`)
- Totais do backfill **não formam uma partição global de perguntas**: BATIDA sem origem pode constar em `fora_do_indice` e também numa colisão/classificação do plano. A própria busca de colisões acrescenta a ponte legada. Não é dupla classificação de perdedora, são medidas diferentes. (`app/chamados/services/fio.py:1261`; `app/chamados/services/fio.py:1292`; `app/chamados/services/fio.py:1056`; `app/chamados/services/fio.py:1058`)
- Uma perdedora `admin_decidiu`, sem resposta e sem ato pode receber silêncio e depois retratação: `linhas_silencio` e `retratadas` contam dois atos da mesma PK, corretamente. Mérito exige resposta; silêncio é registrado antes da retratação. (`app/chamados/services/fio.py:1007`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:266`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:276`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:284`)
- Recusa faz rollback e zera contadores de escrita/informativos; permanece somente a causa. Não mistura escrita persistida e recusa no retorno normal. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:297`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:300`)

**Item 7 — mapa condensado.**

| Campo | Escritores | Leitores / significado |
|---|---|---|
| `via_resolucao` | `_escrever_carimbo`, portas de carimbo/descarimbo, lote, encerramento por fato e relavra. (`app/chamados/models.py:2830`; `app/chamados/models.py:2915`; `app/chamados/models.py:2941`; `app/chamados/models.py:2993`; `app/chamados/models.py:3004`; `app/chamados/services/relavra_via.py:125`) | Mérito, idempotência, saída da identidade, C6, índices, cartório, apresentação. `admin_decidiu` e `duplicata` conservam significados distintos. (`app/chamados/services/fio.py:1007`; `app/chamados/services/fio.py:1085`; `app/chamados/models.py:2934`; `app/chamados/juizes.py:1164`; `app/chamados/juizes.py:1190`; `app/chamados/models.py:3213`; `app/ponto/services/cartorio.py:1599`; `app/chamados/services/explicador.py:95`) |
| `recusa_motivo` | Duplicata/referência à canônica, recusa humana, escape exigindo admin, pendência e recusa do materializador. (`app/chamados/services/fio.py:1094`; `app/chamados/models.py:2920`; `app/chamados/services/validacao.py:584`; `app/chamados/services/disputa_emissao.py:1432`; `app/chamados/services/materializacao.py:847`; `app/chamados/services/materializacao.py:1096`; `app/chamados/services/materializacao.py:1099`) | Validação, explicador, auditores, regex do flip e telas. Campo polivalente, não indicação exclusiva de recusa de mérito. (`app/chamados/services/validacao.py:192`; `app/chamados/services/validacao.py:208`; `app/chamados/services/explicador.py:90`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1112`; `app/ponto/services/flip_auto.py:132`; `app/templates/chamados/partials/modal_fio.html:225`) |
| `data_evento` | Factual recebe dia do turno; conversa recebe dia civil. Nascimento persiste; reapontamento preenche NULL e rechaveamento altera dia. (`app/chamados/models.py:2643`; `app/chamados/models.py:2654`; `app/chamados/models.py:2751`; `app/chamados/services/fio.py:838`; `app/chamados/services/fio.py:959`) | Identidade, horizonte, fecho, agrupamento, competência e C6. Leitura correta exige discriminador da conversa; os leitores residuais ficam com agente do censo. (`app/chamados/models.py:2577`; `app/chamados/models.py:2610`; `app/chamados/services/fio.py:1147`; `app/chamados/services/fio.py:1181`; `app/chamados/services/fio.py:1275`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:113`; `app/chamados/juizes.py:336`) |
| `conversa` | Portas factual/conversa e nascimento; migração adiciona default histórico `False`. (`app/chamados/models.py:2643`; `app/chamados/models.py:2654`; `app/chamados/models.py:2752`; `app/chamados/migrations/0051_decisao_pergunta.py:99`) | Identidade/índices, horizonte, fecho, balde contábil e C6. A migração declara que conversa antiga não é distinguível e nasce `False`; ocorrência real no acervo fornecido: [nao verificado]. (`app/chamados/models.py:2611`; `app/chamados/models.py:3213`; `app/chamados/services/fio.py:1147`; `app/chamados/services/fio.py:1178`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:71`; `app/chamados/juizes.py:336`; `app/chamados/migrations/0051_decisao_pergunta.py:5`) |
| `batida_origem` | Explícita/derivada no nascimento e derivação na inserção; derivador confere família e colaborador. Acervo permanece NULL por desenho. (`app/chamados/models.py:2696`; `app/chamados/models.py:2752`; `app/chamados/models.py:3177`; `app/chamados/models.py:3190`; `app/chamados/models.py:3196`; `app/chamados/migrations/0052_pergunta_batida_origem.py:3`) | Identidade do episódio, índice, veto, vizinhos, chave de agrupamento e censo fora do índice. Mesmo episódio pode atravessar dias. (`app/chamados/models.py:2579`; `app/chamados/models.py:3218`; `app/chamados/services/fio.py:1014`; `app/chamados/services/fio.py:1020`; `app/chamados/services/fio.py:1181`; `app/chamados/services/fio.py:1109`; `app/chamados/services/fio.py:1292`) |
| Retorno `aplicar` | Categorias/informativos por PK, três escalares de atos e causas de recusa. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:240`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:281`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:291`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:300`) | Dois comandos traduzem retorno; dedup usa `CONTADORES_DO_APPLY` para separar recusa. Canônica sem episódio é marcador, não perdedora. (`app/chamados/management/commands/dedup_perguntas_fato.py:107`; `app/chamados/management/commands/dedup_perguntas_fato.py:111`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:430`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:447`; `app/chamados/services/fio.py:1041`) |

**PROCEDENTE incompatibilidade residual concreta de apresentação:** tela traduz `duplicata` como “pergunta repetida do mesmo dia”, embora identidade BATIDA seja episódio+motivo, sem dia na chave. Texto do escritor também imprime a data da perdedora sem garantir que seja a data da canônica. (`app/templates/chamados/partials/_card_atendimento.html:186`; `app/chamados/services/fio.py:1109`; `app/chamados/models.py:2579`; `app/chamados/services/fio.py:1095`)

**Possível leitor ausente do censo encaminhado ao outro agente:** `linhas_do_evento` transporta dia cru; `acordar_celulas` escolhe célula por colaborador+dia e emite evento sem conhecer `conversa`; sinal produtivo usa esse caminho. (`app/chamados/models.py:2961`; `app/chamados/models.py:2976`; `app/chamados/models.py:2981`; `app/chamados/signals.py:221`)

## 07/11 19:51 · sessao 0be3bf1f7461 · - · - · origem=Ronald (tmux)

Complemento de travas:

- **NAO PROCEDE corrida demonstrada pelo novo fecho.** Apply toma Colaborador, calcula U, bloqueia Disputas/Perguntas de U e relê o universo antes do plano. Sem mudança concorrente de conectividade, o fecho interno permanece em U. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:210`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:213`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:226`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`)
- **PROCEDENTE limite: ler o fecho não bloqueia suas linhas.** O plano usa consultas normais; `select_related` não equivale a `select_for_update`. A porta direta também não toma trava por si. (`app/chamados/services/fio.py:1051`; `app/chamados/services/fio.py:1181`; `app/chamados/services/fio.py:1075`; `app/chamados/services/fio.py:1076`)
- Migração segura Colaborador durante o ato; colisão chega com Colaborador adquirido por `fato_livre_para`, e declara expressamente consultas sem FOR UPDATE. A proteção dessas leituras depende do protocolo compartilhado pelos escritores, não de cada Pergunta lida estar bloqueada. (`app/chamados/services/fio.py:1357`; `app/chamados/services/fio.py:1358`; `app/chamados/models.py:2623`; `app/chamados/models.py:2624`; `app/chamados/services/fio.py:888`)
- Na busca de escritores, nascimento factual e rechaveamento tomam Colaborador; as portas impedem devolver ao fato linhas em `VIAS_FORA_DO_FATO`. Não localizei expansão produtiva da conectividade que dispense Colaborador; ocorrência concorrente concreta permanece **[nao verificado]**. (`app/chamados/models.py:2667`; `app/chamados/models.py:2669`; `app/chamados/services/fio.py:947`; `app/chamados/models.py:2913`; `app/chamados/models.py:2939`; `app/chamados/models.py:2993`)

Pouso: existe requisito local de E1 pousar depois de FERIAS/V2, enquanto o backlog ainda registra a obra aberta; isso não prova a cronologia efetiva do pouso. (`app/docs/RELATO.md:20`; `app/docs/BACKLOG.md:1827`)

## 07/11 19:51 · sessao 45318f034c07 · - · - · origem=Ronald (tmux)

Item 4: **PROCEDENTE lacuna residual de alcance; NAO PROCEDE falso positivo encontrado na árvore atual.**

- `_sql_trava` remove comentários por regex sem reconhecer strings SQL. Assim, `SELECT '--' FROM chamados_perguntadisputa FOR UPDATE` escapa; `SELECT 'FOR UPDATE' FROM chamados_perguntadisputa` é acusado apesar de não travar. Ambos são SQL estático, fora do limite declarado de SQL dinâmico. (`app/chamados/tests/test_e1_v7_catracas.py:36`; `app/chamados/tests/test_e1_v7_catracas.py:147`; `app/chamados/tests/test_e1_v7_catracas.py:154`)
- B perde propagação de fonte: `x=PerguntaDisputa.do_fato(...); y=x; y.first()` devolve zero, porque `do_fato_em` reconhece somente atribuições cujo valor contém a chamada original. Também perde `j,z=canonica_do_grupo,0; j([p for p in ps if p.ok])`, pois propagação do juiz exige valor `Name/Attribute` e alvo `Name`. Esses limites não estão declarados. (`app/chamados/tests/test_e1_v7_catracas.py:36`; `app/chamados/tests/test_e1_v7_catracas.py:221`; `app/chamados/tests/test_e1_v7_catracas.py:227`)
- Executei os detectores puros, extraídos via AST em memória, sobre 749 fontes; A/B/C/D/A2 retornaram zero. Resultado da execução sem arquivo de log: **[nao verificado]**. A varredura e suas exclusões estão em `app/chamados/tests/test_e1_v7_catracas.py:54`, `:57`; exigência de zero em `:390`.

**NAO PROCEDE ausência geral de mordida nos testes citados.** [nao verificado]

- V11 apply e migração afirmam persistência, recusa e contadores; controle sem ponte exige retratação real. (`app/chamados/tests/test_e1_v11.py:86`; `:89`; `:103`; `:104`; `:105`; `:117`; `:118`)
- Semente exige `AssertionError`; universo único observa duas chamadas totais — apply + porta — e retratação persistida única. Não prova travas de linhas, nem declara fazê-lo. (`app/chamados/tests/test_e1_v11.py:130`; `:153`; `:156`; `:157`; `:158`)
- Contadores do dedup têm positivo apenas para abaixo do horizonte; sem-dia e canônica sem episódio são exigidos com zero. Portanto testa apresentação dos três nomes, não a aritmética positiva dos outros dois. (`app/chamados/tests/test_e1_v11.py:170`; `:175`)
- `PlanoParticionaTest` declara o fecho mockado como identidade e afirma cobertura/disjunção das categorias; integração real está nos testes V11. (`app/chamados/tests/test_e1_v10.py:123`; `:127`; `:135`; `:136`; `app/chamados/tests/test_e1_v11.py:86`)
- Limbo ganhou controle factual positivo; retirar toda consulta deixa de passar. (`app/chamados/tests/test_e1_v10.py:249`; `:264`; `:265`)
- Relógio solto novo nos testes indicados: **NAO PROCEDE localizado**; datas são literais, classes concretas de banco V11 estão congeladas. (`app/chamados/tests/test_e1_v11.py:26`; `:28`; `:74`; `:135`; `:162`)

Item 5: **PROCEDENTE censo ainda incompleto.** O censo enumera os leitores em `app/chamados/services/fio.py:1126`–`:1144`, mas omite, ao menos:

| Leitor omitido | Decisão/contagem |
|---|---|
| Calendário | Indexa perguntas respondidas vivas pelo `data_evento` cru e usa isso para permitir último recurso. Conversa pode afetar o dia civil quando este já pertence à competência encerrada; ocorrência produtiva **[nao verificado]**. (`app/colaboradores/services/calendario.py:473`; `:477`; `:738`; `:739`) |
| Mapa de divergência | Inclui qualquer Pergunta na janela e conta seu dia como cobertura de furo restante, sem excluir conversa. (`app/ponto/management/commands/mapa_divergencia.py:270`; `:273`; `:275`; `:279`) |
| Vigia de carimbos | Separa carimbos vivos em aberta/exportada pela competência de `p.data_evento`; filtro de entrada não exclui conversa. (`app/chamados/services/vigia_fio.py:806`; `:816`; `:818`; `app/chamados/services/relavra_via.py:181`) |
| Aceite de folga | Lê dia cru e declara folga; validação desvia para essa porta antes do gate de data do materializador. (`app/chamados/services/validacao.py:269`; `:305`; `:321`; `:412`) |
| Ausência pela resposta | Lê dia cru, cria Ausência e seleciona irmãs por esse dia; a resposta chama diretamente essa porta. (`app/chamados/services/materializacao.py:517`; `:521`; `:532`; `app/chamados/services/disputa_emissao.py:1422`) |
| Outbox da Pergunta | Captura `data_evento` sem discriminador e escolhe célula pelo par colaborador/dia para emitir evento; signal alcança Pergunta atualizada. (`app/chamados/models.py:2961`; `:2972`; `:2977`; `:2981`; `app/chamados/signals.py:221`) |

Não foram executados testes Django nem acessados banco/logs; execução produtiva dos contraexemplos: **[nao verificado]**. [nao verificado]

## 07/11 19:50 · sessao 0be3bf1f7461 · - · - · origem=Ronald (tmux)

Item 6 — conclusões estáticas:

| Reincidência | Veredito e prova |
|---|---|
| Segundo juiz da canônica | **NAO PROCEDE localizado.** A eleição permanece em `canonica_do_grupo`, resposta primeiro e menor PK no empate; os chamadores consultados delegam a ela. O registro de autoridades de Chamado permanece em `app/core/juizes.py:260–278`. (`app/chamados/services/fio.py:989`; `app/chamados/services/fio.py:995`; `app/chamados/models.py:2702`; `app/chamados/services/declaracao_texto.py:245`) |
| Segundo escritor de duplicata fora da porta | **NAO PROCEDE localizado.** A busca produtiva por `retratar_duplicata`, `VIA_DUPLICATA`, atribuições de `via_resolucao` e carimbo literal encontrou a sequência porta → retratador → carimbo. A relavra só aceita vias derivadas, não duplicata. (`app/chamados/services/fio.py:1075`; `app/chamados/services/fio.py:1076`; `app/chamados/services/fio.py:1094`; `app/chamados/services/relavra_via.py:77`; `app/chamados/services/relavra_via.py:118`) |
| Relógio fora da borda | **PROCEDENTE residual.** Retratador usa `timezone.now()` quando omitido; remoção de motivo usa `_tz.now()` internamente; lavra lê `date.today()` e `timezone.now()`. Esses trechos não pertencem ao diff fornecido. (`app/chamados/services/fio.py:1087`; `app/chamados/services/fio.py:1088`; `app/chamados/models.py:1994`; `app/chamados/veredito_lavrado.py:112`; `app/chamados/veredito_lavrado.py:228`) |
| Derivação paralela do universo | **PROCEDENTE divergência residual**, embora a triagem a descarte por desenho. Abertura e declaração usam `do_fato` de um passo; reconciliação usa o fecho. Para A(E,d1), A2(E,d2), L2(d2,respondida), abertura E/d1 recebe A/A2, enquanto o fecho alcança L2 por A2 e a elege. Isso demonstra universos/canônicas diferentes; não demonstra retratação indevida porque o plano preserva o legado. (`app/chamados/models.py:2577`; `app/chamados/models.py:2579`; `app/chamados/models.py:2703`; `app/chamados/models.py:2704`; `app/chamados/services/declaracao_texto.py:242`; `app/chamados/services/declaracao_texto.py:245`; `app/chamados/services/fio.py:1205`; `app/chamados/services/fio.py:1219`; `app/chamados/services/fio.py:1058`) |
| Ordem de trava em remover_motivo | **NAO PROCEDE inversão no corpo atual.** Colaborador é adquirido antes de Disputa/Chamado/Pergunta e antes do carimbo/save. O fechamento subsequente fica fora desse atomic quando não há transação externa. (`app/chamados/models.py:1995`; `app/chamados/models.py:1996`; `app/chamados/models.py:1998`; `app/chamados/models.py:2031`; `app/chamados/models.py:2035`; `app/chamados/models.py:2042`) |
| Ordem de trava residual por S143 | **PROCEDENTE caminho residual.** `reescrever_texto` salva Chamado sob atomic; o post_save para Chamado já encerrado chama resolução sem exigir mudança de status; só então a porta pede Colaborador/Disputa/Chamado/Pergunta. Portanto UPDATE Chamado antecede a ordem global. Esses trechos não pertencem ao diff fornecido. (`app/chamados/services/texto.py:31`; `app/chamados/services/texto.py:35`; `app/chamados/signals.py:81`; `app/chamados/signals.py:91`; `app/chamados/signals.py:92`; `app/chamados/services/disputa_emissao.py:1683`) |
| Pouso antes do pré-requisito | Pouso efetivo, cronologia e cumprimento histórico: **[nao verificado]**. | [nao verificado]
| Campo com dois significados | **PROCEDENTE residual.** A conversa guarda dia civil, o fato guarda dia factual; lavra ainda calcula expiração pelo campo cru e sua doc associa conversa a NULL. `recusa_motivo` guarda referência de duplicata, recusa humana e recusa de materialização. (`app/chamados/models.py:2683`; `app/chamados/models.py:2689`; `app/chamados/veredito_lavrado.py:107`; `app/chamados/veredito_lavrado.py:229`; `app/chamados/services/fio.py:1095`; `app/chamados/services/validacao.py:584`; `app/chamados/services/materializacao.py:1099`) |

Ressalva adicional ao item 2: a propriedade do docstring continua ampla demais para linha persistida `conversa=False` **fora do fato**. `universo_da_linha(p)` inclui a própria semente, mas `universo_do_grupo([p.pk])` elimina `VIAS_FORA_DO_FATO` antes do fecho; para duplicata isolada, o primeiro contém p e o segundo devolve vazio. A doc restringiu por conversa/persistência, mas não pela elegibilidade da via. (`app/chamados/services/fio.py:1191`; `app/chamados/services/fio.py:1192`; `app/chamados/services/fio.py:1202`; `app/chamados/services/fio.py:1217`; `app/chamados/services/fio.py:1218`; `app/chamados/services/fio.py:1227`)

## 07/11 19:15 · sessao 88edcf0169ab · FIO-E0-V4-E1 · verificador · origem=Code

**Veredito: cura parcial.** Permanecem universos diferentes na abertura e na reconciliação, veto de episódios dependente do recorte, leitores que tratam conversa como turno e lacunas nos selos. (`app/chamados/models.py:2703`; `app/chamados/services/fio.py:1048`; `app/chamados/models.py:364`; `app/chamados/tests/test_e1_v7_catracas.py:168`)

Identidade independente do commit/pai, execução da suíte informada, RED→GREEN histórico e ocorrência produtiva dos contraexemplos: **[nao verificado]**. [nao verificado]

**1. PROCEDENTE ressalva de universo; NAO PROCEDE a antiga assimetria entre linhas factuais persistidas e elegíveis, sob leitura estável.**

| Entrada | Retorno |
|---|---|
| BATIDA sem episódio | Expande pelo dia, alcançando também episódios distintos daquele dia e suas demais ligações. (`app/chamados/models.py:2583`; `app/chamados/models.py:2610`; `app/chamados/services/fio.py:1188`) |
| BATIDA com episódio em vários dias | Alcança todas as linhas do episódio; cada uma expande o legado de seu dia, continuando até esgotar as PKs alcançáveis. (`app/chamados/models.py:2577`; `app/chamados/models.py:2579`; `app/chamados/services/fio.py:1187`) |
| MARCO | Motivo, colaborador, dia e instância derivada; inclui a própria semente. (`app/chamados/models.py:2610`; `app/chamados/services/fio.py:1185`) |
| Conversa | `universo_da_linha(p)=[p]`, mas `universo_do_grupo([p.pk])=[]`. A igualdade anunciada tem essa exceção explícita. (`app/chamados/services/fio.py:1200`; `app/chamados/services/fio.py:1208`) |
| Semente não salva | Duas sementes com `pk=None` colidem no dicionário; uma semente `None` com vizinho persistido provoca `TypeError` em `sorted(None, inteiro)`. (`app/chamados/services/fio.py:1184`; `app/chamados/services/fio.py:1192`) |
| Semente datada somente em memória | Preserva a instância recebida, mas consultas iniciadas pelos outros membros veem o banco. Para MARCO, p ainda NULL no banco, p=D em memória e q persistida em D: `fecho(p)=[p,q]`, `fecho(q)=[q]`. (`app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:1185`; `app/chamados/models.py:2610`) |

Num grafo finito e estável, termina: cada PK entra uma única vez na fila. Isso não elimina a exceção de ordenação da semente não salva. (`app/chamados/services/fio.py:1184`; `app/chamados/services/fio.py:1189`; `app/chamados/services/fio.py:1192`)

**Permanece divergência produtiva entre chamadores:** `_nascer` e a declaração continuam elegendo sobre `do_fato` de um passo. Com A(E,d1), A2(E,d2), L2(d2,respondida), abrir E/d1 pode escolher A; o backfill pelo fecho escolhe L2. O juiz é o mesmo, mas recebe universos diferentes. (`app/chamados/models.py:2703`; `app/chamados/models.py:2704`; `app/chamados/services/declaracao_texto.py:242`; `app/chamados/services/declaracao_texto.py:245`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:237`)

A antiga sobrescrita pela primeira PK foi removida; `grupos_de_colisao` e apply deduplicam universos por conjunto de PKs. A migração ainda planeja por pergunta e pode reexaminar o mesmo universo quando recusas o deixam inalterado; escrita duplicada decorrente disso: **[nao verificado]**. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:213`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:231`; `app/chamados/services/fio.py:1255`; `app/chamados/services/fio.py:1363`; `app/chamados/services/fio.py:1383`)

**2. NAO PROCEDE a antiga omissão de mérito; PROCEDENTE dependência do recorte.**

Com PKs distintas, cada perdedora recebe exatamente uma categoria, nesta precedência: mérito → episódios unidos → sem episódio → retrata. `canonica_sem_episodio` é marcador da canônica, não categoria de perdedora; `NAO_RETRATA` contém somente as três categorias que preservam perdedoras. (`app/chamados/services/fio.py:1038`; `app/chamados/services/fio.py:1040`; `app/chamados/services/fio.py:1049`; `app/chamados/services/fio.py:1051`; `app/chamados/services/fio.py:1062`)

**Contraexemplo residual:** d0 abaixo do horizonte contém A0(α), L0(sem episódio), B0(β); d1 no horizonte contém A1(α,respondida,canônica) e A2(α). O fecho reúne todos pela ponte A0→L0→B0. O plano completo declara `episodios_unidos`; apply e DRYs entregam somente A2 como perdedora ao plano, que não vê β e retrata A2. A precedência não mudou; mudou a informação usada para decidir `unidos`. (`app/chamados/models.py:2577`; `app/chamados/models.py:2583`; `app/chamados/services/fio.py:1048`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:240`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:385`; `app/chamados/management/commands/dedup_perguntas_fato.py:77`)

Na migração, A1 no velho, A2 no novo e os demais fora desses chamados também escondem β do plano: a canônica é escolhida pelo universo inteiro, mas o veto recebe somente conflito+[p]. Isso permite migrar A1 e retratar A2, contrariando a declaração “universo com episódios distintos: nada se retrata”. Não demonstra retratação de α para β. (`app/chamados/services/fio.py:1033`; `app/chamados/services/fio.py:1378`; `app/chamados/services/fio.py:1380`; `app/chamados/services/fio.py:1383`; `app/chamados/services/fio.py:1399`; `app/chamados/services/fio.py:1405`)

No VETO-UNICO, com mesmo episódio e p1 canônica:

| Caminho | p1 / p2 / p3 |
|---|---|
| Porta direta | p1 permanece; p2 preservada e declarada em mérito; p3 retratada. (`app/chamados/services/fio.py:1007`; `app/chamados/services/fio.py:1051`; `app/chamados/services/fio.py:1073`) |
| Migração | Preserva p1; declara recusa de p2; retrata p3. O teste fixa esses valores. (`app/chamados/services/fio.py:1389`; `app/chamados/services/fio.py:1404`; `app/chamados/tests/test_e1_v9.py:168`) |
| Backfill/dedup apply | Mérito recusa o ato inteiro; ninguém muda; contadores de escrita zerados. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:252`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:274`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:297`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:300`) |
| Colisão ao datar p1 | Recusa sem retratar; desfaz a data recém-atribuída. Pode recusar antes pelo teste de encerradas. (`app/chamados/services/fio.py:897`; `app/chamados/services/fio.py:905`; `app/chamados/services/fio.py:852`) |

**NAO PROCEDE mistura de escrita persistida e recusa no retorno normal de `aplicar`.** Porta direta e migração podem retratar p3 e declarar p2 recusada, como exige o aval; apply faz rollback integral. (`app/chamados/services/fio.py:1074`; `app/chamados/services/fio.py:1404`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:297`)

Canônica externa não migrada para o novo é o comportamento previsto no contraexemplo, não prova de fato sem resposta. Ausência produtiva de resolvedora causada nesta volta: **[nao verificado]**. (`app/chamados/tests/test_e1_v9.py:153`; `app/chamados/tests/test_e1_v9.py:159`; `app/chamados/tests/test_e1_v9.py:168`)

**3. NAO PROCEDE segundo escritor produtivo de duplicata localizado.**

Os caminhos encontrados convergem: reapontamento/migração/backfill → `retratar_para_canonica` → `retratar_duplicata` → `carimbar_identidade(VIA_DUPLICATA)`. Todos passam pelo plano, com a ressalva dos recortes do item 2. (`app/chamados/services/fio.py:867`; `app/chamados/services/fio.py:1405`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:284`; `app/chamados/services/fio.py:1073`; `app/chamados/services/fio.py:1091`)

`retratada_em` pertence a **Batida**. A remoção de motivo carimba Pergunta com `motivo_retratado`, outro ato, sem eleição de canônica. Não é um segundo escritor do ato “duplicata para a canônica”. (`app/ponto/models.py:67`; `app/chamados/models.py:2031`)

**4. NAO PROCEDE permanência dos defeitos nomeados; PROCEDENTE censo incompleto.**

Factual nasce com dia do turno e `conversa=False`; conversa nasce com `localdate()` e `True`; `_nascer` persiste ambos. Reapontamento preenche NULL; rechaveamento altera dia existente. (`app/chamados/models.py:2643`; `app/chamados/models.py:2654`; `app/chamados/models.py:2751`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:959`)

O helper agora distingue conversa na testemunha de saída, nos pontos corrigidos do C6 e na tranca. Veredito e limbo excluem conversa dos querysets por dia. **Essas curas procedem.** (`app/chamados/juizes.py:332`; `app/chamados/juizes.py:375`; `app/chamados/juizes.py:671`; `app/chamados/juizes.py:763`; `app/chamados/juizes.py:948`; `app/chamados/juizes.py:1216`; `app/ponto/services/veredito_celula.py:180`; `app/chamados/services/limbo_folgas.py:203`)

Permanecem leitores que decidem ou contam pelo dia civil:

| Leitor | Consequência |
|---|---|
| Gate do Chamado | Inclui conversa entre ancoradas, verifica competência pelo campo cru e procura cobrança sobrevivente pelo mesmo dia civil; pode ignorar a conversa ou reapontá-la. `resolvedora_valida` aceita conversa antes de conferir dia. (`app/chamados/models.py:344`; `app/chamados/models.py:364`; `app/chamados/models.py:390`; `app/chamados/models.py:437`; `app/chamados/juizes.py:1986`) |
| Serviço de competência trancada | Seleciona conversa pela competência civil e pode incluí-la na pauta do DP e nos contadores. A leitura C6 da tranca está corrigida; essa seleção não. (`app/chamados/services/competencia_trancada.py:107`; `app/chamados/services/competencia_trancada.py:129`; `app/chamados/services/competencia_trancada.py:133`) |
| I21/I21a, F1 e F3 | Consultam ausência/falta usando datas sem excluir conversa; F1 pode escolher a data de uma conversa como data examinada. (`app/chamados/management/commands/auditar_invariantes_chamados.py:162`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1571`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1664`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1682`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1803`) |
| Lavra | Calcula expiração por `p.data_evento` cru; a documentação ainda associa conversa a data NULL. (`app/chamados/veredito_lavrado.py:107`; `app/chamados/veredito_lavrado.py:229`) |

O censo nomeia telas e `data_do_fato_de`, inclusive sua escrita de recusa, mas não nomeia os leitores residuais acima. A triagem anexada não substitui esse censo no código. (`app/chamados/services/fio.py:1123`; `app/chamados/services/fio.py:1129`; `app/chamados/services/fio.py:1133`)

**5. NAO PROCEDE a antiga ausência de partição; PROCEDENTE omissão residual na apresentação.**

| Contador | Quem produz / quem consome |
|---|---|
| `retratadas` | Apply soma retratações efetivas; ambos os comandos apresentam. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:291`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:448`; `app/chamados/management/commands/dedup_perguntas_fato.py:103`) |
| `abaixo_do_horizonte` | Apply separa perdedoras datadas fora do horizonte; backfill imprime; dedup reconhece como informativo, mas omite da linha final. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:244`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:447`; `app/chamados/management/commands/dedup_perguntas_fato.py:99`; `app/chamados/management/commands/dedup_perguntas_fato.py:106`) |
| `perdedora_sem_dia` | Apply separa NULL; backfill imprime pela tupla de contadores; dedup omite. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:243`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:449`; `app/chamados/management/commands/dedup_perguntas_fato.py:104`) |
| `canonica_sem_episodio` | Plano produz marcador da canônica; apply acumula; backfill imprime; dedup omite. Não integra `NAO_RETRATA`. (`app/chamados/services/fio.py:1047`; `app/chamados/services/fio.py:1062`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:253`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:449`) |
| `sem_episodio_intocada`, `episodios_unidos` | Plano classifica; apply acumula; ambos os comandos imprimem. O veto continua sujeito ao recorte do item 2. (`app/chamados/services/fio.py:1054`; `app/chamados/services/fio.py:1056`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:254`; `app/chamados/management/commands/dedup_perguntas_fato.py:104`) |
| `merito_recusado` | Plano produz; apply recusa; backfill DRY apresenta como `merito_em_colisao`; dedup DRY inclui em `nao_retrata`. (`app/chamados/services/fio.py:1052`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:252`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:391`; `app/chamados/management/commands/dedup_perguntas_fato.py:78`) |
| `a_retratar`, `nao_retrata` | Backfill DRY soma `retrata` e apresenta separadamente mérito/demais categorias; dedup usa `retrata` como excedentes e soma as três categorias em `nao_retrata`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:377`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:387`; `app/chamados/management/commands/dedup_perguntas_fato.py:68`; `app/chamados/management/commands/dedup_perguntas_fato.py:80`) |

Para um universo único com PKs distintas, as perdedoras ficam em abaixo/sem-dia/uma categoria do plano/retrata; a canônica fica separada. O marcador da canônica não deve ser somado como outra perdedora. Na recusa, o retorno apaga contadores informativos e conserva as causas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:240`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:243`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:300`)

DRY e apply agora usam o plano sobre o mesmo recorte de escrita, sem concorrência; isso não significa números finais iguais quando há veto: DRY conta candidatos e apply recusado escreve zero. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:385`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:300`; `app/chamados/management/commands/dedup_perguntas_fato.py:77`)

Parser produtivo da linha final do dedup não localizado na busca; inexistência externa: **[nao verificado]**. Os testes encontrados leem substrings; o `split` próximo lê descrição de log. (`app/chamados/tests/test_e1_v5_chave_familia.py:129`; `app/chamados/tests/test_e1_v5_chave_familia.py:131`; `app/chamados/tests/test_e1_v10.py:209`)

**6. NAO PROCEDE inversão no corpo de `remover_motivo` ou caminho próprio nos sinais encontrados.**

A porta adquire Colaborador, depois Disputas/Chamados/Perguntas, antes de carimbar e salvar. A implementação efetiva segue Colaborador → Disputa → Chamado → Pergunta. Posto e âncora chamam a mesma porta; chamada direta entra nesse mesmo corpo. (`app/chamados/models.py:1996`; `app/chamados/models.py:1998`; `app/chamados/models.py:2031`; `app/chamados/models.py:2035`; `app/chamados/models.py:2525`; `app/chamados/models.py:2532`; `app/chamados/models.py:2545`; `app/chamados/models.py:2549`; `app/colaboradores/signals.py:77`; `app/escala/signals.py:161`)

Sem atomic externo, remoção e fechamento são duas transações; `fechar` antecipa novamente a ordem. Isso limita a indivisibilidade, mas não demonstra inversão. (`app/chamados/models.py:2035`; `app/chamados/models.py:2042`; `app/chamados/models.py:1875`; `app/chamados/models.py:1880`)

**7. PROCEDENTE alcance insuficiente; NAO PROCEDE ausência geral de mordida.**

As formas corrigidas têm asserções concretas: espaço duplo, `AnnAssign`, apelido simples local e literal `duplicata`. (`app/chamados/tests/test_e1_v10.py:285`; `app/chamados/tests/test_e1_v10.py:290`; `app/chamados/tests/test_e1_v10.py:296`; `app/chamados/tests/test_e1_v10.py:302`)

Ainda escapam formas fora dos limites declarados:

| Catraca | Burla |
|---|---|
| A | SQL estático com `CHAMADOS_PERGUNTADISPUTA`: comparação da tabela distingue caixa. Também `FOR/**/UPDATE`: regex exige espaço entre tokens. (`app/chamados/tests/test_e1_v7_catracas.py:137`; `app/chamados/tests/test_e1_v7_catracas.py:145`) |
| B | `canonica_do_grupo({p.pk:p for p in ps if p.ok}.values())`: falta `DictComp`. Também `j=fio.canonica_do_grupo; j(recorte)`: propagação exige `ast.Name`. (`app/chamados/tests/test_e1_v7_catracas.py:168`; `app/chamados/tests/test_e1_v7_catracas.py:216`) |
| D | `getattr(p,"carimbar_identidade")("duplicata",u)`: o detector exige chamada cujo nome seja `carimbar_identidade`. (`app/chamados/tests/test_e1_v7_catracas.py:312`) |

Esses casos não são SQL dinâmico, `min/sorted`, filtro de QuerySet, método ligado a apelido ou valor vindo de variável — os limites atualmente declarados. (`app/chamados/tests/test_e1_v7_catracas.py:35`; `app/chamados/tests/test_e1_v7_catracas.py:42`)

**PROCEDENTE ressalva nos testes novos:** [nao verificado]

- T observa `kw['perguntas']`, não linhas bloqueadas. Uma porta que ignorasse esse argumento manteria a asserção verde. Com o fecho novo, os dois grupos do teste também têm o mesmo universo; restaurar isoladamente o dicionário antigo não reproduz a perda anterior. (`app/chamados/tests/test_e1_v10.py:175`; `app/chamados/tests/test_e1_v10.py:185`; `app/chamados/tests/test_e1_v10.py:190`; `app/chamados/services/fio.py:1188`)
- O teste de limbo exige apenas ausência da conversa em `visto`: retirar inteiramente a consulta a `fio_vivo` também passa, pois não há controle factual positivo nem exigência de chamada. (`app/chamados/tests/test_e1_v10.py:268`; `app/chamados/tests/test_e1_v10.py:278`)

**NAO PROCEDE relógio solto novo localizado em v10:** datas literais e classes concretas de banco congeladas. (`app/chamados/tests/test_e1_v10.py:30`; `app/chamados/tests/test_e1_v10.py:72`; `app/chamados/tests/test_e1_v10.py:166`; `app/chamados/tests/test_e1_v10.py:195`; `app/chamados/tests/test_e1_v10.py:222`)

**8. Reincidências.**

| Classe | Veredito |
|---|---|
| Segundo juiz registrado duplicado | **NAO PROCEDE demonstrado:** eleição continua em `canonica_do_grupo`; **PROCEDENTE universos diferentes fornecidos à autoridade**, conforme item 1. (`app/chamados/services/fio.py:989`; `app/chamados/models.py:2703`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:237`) |
| Segundo escritor de duplicata | **NAO PROCEDE localizado**, conforme item 3. (`app/chamados/services/fio.py:1073`; `app/chamados/services/fio.py:1091`) |
| Relógio fora da borda | **PROCEDENTE preexistente:** retratador, remoção e lavra leem relógio internamente. (`app/chamados/services/fio.py:1084`; `app/chamados/models.py:1994`; `app/chamados/veredito_lavrado.py:112`; `app/chamados/veredito_lavrado.py:228`) |
| Derivação paralela | **PROCEDENTE:** um passo na abertura versus fecho na reconciliação; veto recebe recortes diferentes. (`app/chamados/models.py:2703`; `app/chamados/services/fio.py:1202`; `app/chamados/services/fio.py:1383`) |
| Selo/RED sem mordida | **NAO PROCEDE genericamente; PROCEDENTE insuficiência específica**, conforme item 7. (`app/chamados/tests/test_e1_v10.py:185`; `app/chamados/tests/test_e1_v10.py:278`) |
| Ordem de trava | **PROCEDENTE residual preexistente:** texto de Chamado encerrado faz UPDATE Chamado → S143 → Colaborador/Disputas; receiver não exige mudança de status. (`app/chamados/services/texto.py:31`; `app/chamados/services/texto.py:35`; `app/chamados/signals.py:81`; `app/chamados/signals.py:92`; `app/chamados/services/disputa_emissao.py:1683`) |
| Pouso antes do pré-requisito | Pouso efetivo e atendimento histórico dos pré-requisitos: **[nao verificado]**. | [nao verificado]
| Campo com dois significados | **PROCEDENTE:** dia factual/civil ainda confundido por leitores; `recusa_motivo` permanece polivalente. (`app/chamados/models.py:2654`; `app/chamados/models.py:364`; `app/chamados/services/validacao.py:584`; `app/chamados/services/fio.py:1092`) |

**9. PROCEDENTE incompatibilidade residual; map

[truncado em 20000 de 23819 caracteres]

## 07/11 19:12 · sessao d496e10b7b23 · - · - · origem=Ronald (tmux)

**1. PROCEDENTE ressalvas de universo; NAO PROCEDE a assimetria antiga para linhas factuais persistidas e elegíveis, com leitura estável.** O fecho enfileira cada PK uma vez e termina num grafo finito; BATIDA sem episódio expande pelo dia e alcança episódios daquele dia; episódio alcança todas suas linhas e, por elas, os legados dos diferentes dias; MARCO segue motivo/dia/instância derivada. A ponte legada pode unir episódios distintos. (`app/chamados/services/fio.py:1163`; `app/chamados/services/fio.py:1182`; `app/chamados/models.py:2577`; `app/chamados/models.py:2583`; `app/chamados/models.py:2610`)

- Conversa: `universo_da_linha(p)=[p]`, enquanto `universo_do_grupo([p.pk])=[]`; este último exclui conversas e vias fora do fato. A igualdade declarada não vale indiscriminadamente. (`app/chamados/services/fio.py:1200`; `app/chamados/services/fio.py:1208`)
- Semente não salva: PK `None` mais vizinho persistido provoca `TypeError` ao ordenar `None` e inteiro; duas sementes não salvas também colidem na chave `None`. Reproduzido com extração AST somente em memória. (`app/chamados/services/fio.py:1184`; `app/chamados/services/fio.py:1192`)
- Semente datada apenas em memória: preserva-se sua instância; outro membro consulta o banco e pode não reencontrá-la. Exemplo MARCO: p estava NULL no banco, recebe D em memória; q está persistida em D. `fecho(p)=[p,q]`, `fecho(q)=[q]`. Reproduzido com extração AST e vizinhos simulados; ocorrência produtiva **[nao verificado]**. (`app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:1184`; `app/chamados/services/fio.py:1189`; `app/chamados/models.py:2610`)
- **Permanece derivação paralela:** `_nascer` e leitores da declaração ainda elegem pelo `do_fato` de um passo. Com A(E,d1), A2(E,d2), L2(d2,respondida), abrir E/d1 pode escolher A; o fecho do backfill escolhe L2. (`app/chamados/models.py:2703`; `app/chamados/models.py:2704`; `app/chamados/services/declaracao_texto.py:242`; `app/chamados/services/declaracao_texto.py:245`; `app/chamados/services/declaracao_texto.py:267`; `app/chamados/services/fio.py:237`)
  
A última referência acima deve ser `app/chamados/management/commands/backfill_decisoes_pergunta.py:237`.

`grupos_de_colisao` e apply deduplicam universos por `frozenset`; a antiga sobrescrita pela primeira PK foi removida. Migração continua planejando por pergunta e pode examinar novamente um universo inalterado quando há recusas; duplicação de escrita decorrente disso **[nao verificado]**. (`app/chamados/services/fio.py:1255`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:213`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:231`; `app/chamados/services/fio.py:1363`; `app/chamados/services/fio.py:1383`)

**2. NAO PROCEDE a antiga omissão de mérito; PROCEDENTE plano dependente do recorte.** Para entradas com PKs distintas, perdedoras particionam-se por `mérito → episódios unidos → sem episódio → retrata`. `canonica_sem_episodio` é marcador da canônica, não categoria de perdedora. (`app/chamados/services/fio.py:1038`; `app/chamados/services/fio.py:1040`; `app/chamados/services/fio.py:1049`; `app/chamados/services/fio.py:1051`; `app/chamados/services/fio.py:1062`)

**Contraexemplo residual:** d0 abaixo do horizonte contém A0(α), L0(sem episódio), B0(β); d1 no horizonte contém A1(α,respondida,canônica) e A2(α). O fecho contém todos, pela ponte A0→L0→B0. Plano completo declara `episodios_unidos`; apply e DRYs passam somente A2 ao plano, que retrata A2. Migração com A1 no velho/A2 no novo e os demais fora desses chamados também omite β do plano e retrata A2. A ordem das categorias permanece; o universo que determina `unidos` muda. Extração AST confirmou os resultados dos planos, sem banco. (`app/chamados/models.py:2577`; `app/chamados/models.py:2583`; `app/chamados/services/fio.py:1048`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:240`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:385`; `app/chamados/management/commands/dedup_perguntas_fato.py:77`; `app/chamados/services/fio.py:1378`; `app/chamados/services/fio.py:1383`; `app/chamados/services/fio.py:1399`; `app/chamados/services/fio.py:1405`)

No exemplo VETO-UNICO, assumindo mesmo episódio e p1 canônica:

| Caminho | Resultado |
|---|---|
| Porta direta | p1 fica; p2 preservada e declarada em mérito; p3 retratada. (`app/chamados/services/fio.py:1007`; `app/chamados/services/fio.py:1051`; `app/chamados/services/fio.py:1072`) |
| Migração | Mesmo veto de p2; p3 retrata quando integra as linhas desse ato. (`app/chamados/services/fio.py:1379`; `app/chamados/services/fio.py:1389`; `app/chamados/services/fio.py:1404`) |
| Backfill/dedup apply | Mérito recusa o ato inteiro; p1/p2/p3 ficam como estavam, contadores de escrita zerados. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:252`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:274`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:297`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:300`) |
| Colisão ao datar p1 | Recusa sem retratar; desfaz a data recém-atribuída. Pode recusar antes pelo teste de encerradas. (`app/chamados/services/fio.py:897`; `app/chamados/services/fio.py:905`; `app/chamados/services/fio.py:852`) |

**NAO PROCEDE mistura de escrita persistida e recusa no retorno normal de `aplicar`.** A migração e a porta direta podem devolver retratações e recusas, conforme o desenho específico desses atos. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:297`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:300`; `app/chamados/services/fio.py:1074`; `app/chamados/services/fio.py:1404`)

**3. NAO PROCEDE segundo escritor produtivo de duplicata localizado.** Caminhos encontrados: reapontamento/migração/backfill → `retratar_para_canonica` → `retratar_duplicata` → `carimbar_identidade(VIA_DUPLICATA)`. Todos passam pelo plano, embora os recortes do item 2 alterem sua informação. (`app/chamados/services/fio.py:867`; `app/chamados/services/fio.py:1405`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:284`; `app/chamados/services/fio.py:1073`; `app/chamados/services/fio.py:1091`)

`retratada_em` pertence a Batida; não é campo de PerguntaDisputa. Remover motivo grava `motivo_retratado`, outro ato sem eleição de canônica. (`app/ponto/models.py:67`; `app/chamados/models.py:2031`)

**5. PROCEDENTE omissão residual na apresentação; NAO PROCEDE a antiga ausência geral de partição.**

| Contador | Escrita / leitura |
|---|---|
| `retratadas` | Soma efetivas; ambos os comandos leem. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:291`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:448`; `app/chamados/management/commands/dedup_perguntas_fato.py:103`) |
| `abaixo_do_horizonte` | Perdedoras datadas fora do horizonte; backfill imprime, dedup descarta da classificação de recusa sem imprimir. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:244`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:447`; `app/chamados/management/commands/dedup_perguntas_fato.py:99`; `app/chamados/management/commands/dedup_perguntas_fato.py:106`) |
| `perdedora_sem_dia` | Perdedoras NULL; backfill imprime pela tupla; dedup omite. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:243`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:449`; `app/chamados/management/commands/dedup_perguntas_fato.py:104`) |
| `canonica_sem_episodio` | Marcador da canônica; apply acumula e backfill imprime; dedup omite. Não compõe `NAO_RETRATA`. (`app/chamados/services/fio.py:1047`; `app/chamados/services/fio.py:1062`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:253`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:449`) |
| `sem_episodio_intocada`, `episodios_unidos` | Plano escreve; apply acumula; ambos resumos imprimem, sujeitos ao recorte do item 2. (`app/chamados/services/fio.py:1054`; `app/chamados/services/fio.py:1056`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:254`; `app/chamados/management/commands/dedup_perguntas_fato.py:104`) |
| `merito_recusado` | Plano escreve; apply recusa/zera; backfill DRY imprime como `merito_em_colisao`; dedup soma em `nao_retrata` no DRY. (`app/chamados/services/fio.py:1052`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:252`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:391`; `app/chamados/management/commands/dedup_perguntas_fato.py:78`) |
| `a_retratar`, `nao_retrata` | DRY backfill soma `retrata` e categorias sem mérito; dedup soma três categorias e usa `retrata` como excedentes. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:377`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:387`; `app/chamados/management/commands/dedup_perguntas_fato.py:68`; `app/chamados/management/commands/dedup_perguntas_fato.py:80`) |

Para um universo único com PKs distintas, as perdedoras se distribuem entre abaixo/sem-dia/categorias/retrata; a canônica é separada. Em recusa, contadores informativos são apagados no retorno e sobra a causa da recusa. DRY conta candidatos; apply recusado escreve zero. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:240`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:243`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:300`)

Parser produtivo da linha final do dedup não foi localizado na busca do repositório; inexistência externa **[nao verificado]**. Testes leem substrings dessa linha; o `split` encontrado lê a descrição do log. (`app/chamados/tests/test_e1_v5_chave_familia.py:129`; `app/chamados/tests/test_e1_v5_chave_familia.py:131`; `app/chamados/tests/test_e1_v10.py:209`; `app/chamados/tests/test_e1_v10.py:218`)

## 07/11 19:12 · sessao 24b6af79db30 · - · - · origem=Ronald (tmux)

6. **NAO PROCEDE inversão no corpo de `remover_motivo` nem portas alternativas nos receivers encontrados.** O método toma Colaborador em `models.py:1996`, depois Disputas/Chamados/Perguntas em `models.py:1998`, antes do carimbo em `models.py:2031` e do save em `models.py:2035`. A implementação efetiva trava na ordem em `models.py:2525`, `models.py:2532`, `models.py:2545`, `models.py:2549`. Posto e âncora chamam essa mesma porta em `app/colaboradores/signals.py:77` e `app/escala/signals.py:161`.

Sem atomic externo, remover e fechar são duas transações: o primeiro atomic termina em `app/chamados/models.py:2035`, `fechar` é chamado em `app/chamados/models.py:2042`, e abre outro atomic com antecipação da ordem em `app/chamados/models.py:1875` e `app/chamados/models.py:1880`. Não é demonstração de inversão.

7. **PROCEDENTE: há burlas adicionais fora do LIMITE declarado.** Executei os detectores extraídos por AST, sem Django/banco/escrita; os exemplos abaixo devolveram `[]`: [nao verificado]

| Catraca | Exemplo que escapa | Causa |
|---|---|---|
| A | SQL literal `SELECT 1 FROM CHAMADOS_PERGUNTADISPUTA FOR UPDATE` | Nome da tabela é comparado com distinção de caixa. `app/chamados/tests/test_e1_v7_catracas.py:137` |
| A | SQL literal `SELECT 1 FROM chamados_perguntadisputa FOR/**/UPDATE` | Regex exige espaços, não comentários entre tokens. `app/chamados/tests/test_e1_v7_catracas.py:145` |
| B | `canonica_do_grupo({p.pk:p for p in ps if p.ok}.values())` | `_recorta` contempla ListComp/GeneratorExp/SetComp, não DictComp. `app/chamados/tests/test_e1_v7_catracas.py:168` |
| B | `j = fio.canonica_do_grupo; return j([p for p in ps if p.ok])` | Propagação do apelido aceita somente valor `ast.Name`. `app/chamados/tests/test_e1_v7_catracas.py:216` |
| D | `getattr(p,"carimbar_identidade")("duplicata",u)` | Exige função chamada com nome `carimbar_identidade`; o argumento literal sozinho não basta. `app/chamados/tests/test_e1_v7_catracas.py:312` |

Os limites declarados abrangem SQL dinâmico sem tabela, min/sorted/QuerySet, método ligado a apelido e valor vindo de variável; não abrangem esses exemplos estáticos. `app/chamados/tests/test_e1_v7_catracas.py:35`.

**NAO PROCEDE falta geral de mordida.** Os exemplos curados — espaço duplo, AnnAssign/apelido simples e literal duplicata — são detectados; seus testes exigem achados concretos em `app/chamados/tests/test_e1_v10.py:285`, `app/chamados/tests/test_e1_v10.py:290`, `app/chamados/tests/test_e1_v10.py:296`, `app/chamados/tests/test_e1_v10.py:302`. Executei em memória os 22 testes de `test_e1_v7_catracas.py`, substituindo somente `SimpleTestCase` por `unittest.TestCase`: 22 OK. A árvore também devolveu zero nas varreduras A/B/D. Essa execução não demonstra RED→GREEN histórico nem execução da suíte Django. [nao verificado]

**PROCEDENTE insuficiência do teste T novo:** observa argumentos destinados à porta, não linhas efetivamente travadas: `_espia` acumula `kw['perguntas']`, e a asserção lê essa lista. Uma implementação que ignorasse `perguntas` conservaria a asserção verde. `app/chamados/tests/test_e1_v10.py:185`, `app/chamados/tests/test_e1_v10.py:190`. Com o fecho novo, alfa e beta do teste possuem o mesmo universo; voltar isoladamente ao dicionário pela primeira PK não perde linhas desse cenário. `app/chamados/tests/test_e1_v10.py:175`, `app/chamados/services/fio.py:1174`.

**PROCEDENTE possibilidade de vacuidade no teste de limbo:** verifica somente que a conversa não apareceu em `visto`; não exige que `_fio` tenha sido chamado ou que pergunta factual positiva tenha entrado. Remover a chamada de `fio_vivo` inteira satisfaria esse teste. `app/chamados/tests/test_e1_v10.py:268`, `app/chamados/tests/test_e1_v10.py:278`.

**NAO PROCEDE relógio solto novo localizado em v10.** Datas/instante são literais, e todas as classes concretas de banco têm `freeze_time`. `app/chamados/tests/test_e1_v10.py:30`, `app/chamados/tests/test_e1_v10.py:72`, `app/chamados/tests/test_e1_v10.py:166`, `app/chamados/tests/test_e1_v10.py:195`, `app/chamados/tests/test_e1_v10.py:222`.

8. **PROCEDENTE reincidência de ordem fora do sítio curado:** texto de Chamado encerrado faz atomic → UPDATE Chamado → S143 → aquisição Colaborador/Disputas. O receiver testa status, sem exigir alteração de status nem filtrar `update_fields`. `app/chamados/services/texto.py:31`, `app/chamados/services/texto.py:35`, `app/chamados/signals.py:81`, `app/chamados/signals.py:91`, `app/chamados/services/disputa_emissao.py:1683`. Saves de contexto e vínculos têm a mesma possibilidade quando recebem Chamado encerrado. `app/chamados/services/contexto.py:75`, `app/chamados/services/vinculos.py:86`.

**PROCEDENTE relógio produtivo fora da borda, preexistente:** retratador usa fallback `timezone.now`; remoção lê `_tz.now`; lavra lê `date.today` e `timezone.now`. `app/chamados/services/fio.py:1084`, `app/chamados/models.py:1994`, `app/chamados/veredito_lavrado.py:112`, `app/chamados/veredito_lavrado.py:228`.

Leitor adicional avisado ao explorador de campos: lavra chama `expira_em(p.data_evento)` sem discriminador; a própria documentação supõe conversa com data NULL. `app/chamados/veredito_lavrado.py:107`, `app/chamados/veredito_lavrado.py:229`.

## 07/11 19:11 · sessao cfd05f1ee55f · - · - · origem=Ronald (tmux)

**Item 4 — PROCEDENTE residual; NAO PROCEDE permanência dos quatro defeitos nomeados no diff.**

O escritor factual é `abrir(...data_evento=dia_do_turno)` → `_nascer(conversa=False)`; conversa usa `abrir_conversa` → `localdate()` → `_nascer(conversa=True)`. O proxy e a linha recebem o discriminador; reapontamento preenche NULL e rechaveamento altera dia existente. (`app/chamados/models.py:2643`; `app/chamados/models.py:2654`; `app/chamados/models.py:2685`; `app/chamados/models.py:2751`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:959`)

`_dia_de_turno_da_pergunta` retorna None para conversa; é usado na tranca, na testemunha de saída e nos três pontos corrigidos do C6. A conversa não cai no fallback de contexto de `_turno_tem_saida`. Veredito e limbo agora excluem conversa dos querysets por dia. **NAO PROCEDE** a permanência desses defeitos específicos. (`app/chamados/juizes.py:332`; `app/chamados/juizes.py:375`; `app/chamados/juizes.py:376`; `app/chamados/juizes.py:671`; `app/chamados/juizes.py:763`; `app/chamados/juizes.py:948`; `app/chamados/juizes.py:1216`; `app/ponto/services/veredito_celula.py:180`; `app/chamados/services/limbo_folgas.py:203`)

**PROCEDENTE — o helper não cobre todos os decisores do repositório.**

- O gate de fechamento do Chamado busca todas as perguntas ancoradas, inclusive conversa; decide competência por `p.data_evento` cru, procura cobrança sobrevivente do mesmo colab/dia civil e pode reapontar a conversa para ela. `resolvedora_valida` aceita conversa antes de conferir dia. Assim uma conversa legítima pode ser reatada a uma cobrança escolhida pelo dia civil, ou ignorada pelo gate se a competência daquele dia foi entregue. (`app/chamados/models.py:344`; `app/chamados/models.py:364`; `app/chamados/models.py:390`; `app/chamados/models.py:392`; `app/chamados/models.py:419`; `app/chamados/models.py:437`; `app/chamados/juizes.py:1986`; `app/chamados/services/vigia_fio.py:834`; `app/chamados/services/vigia_fio.py:838`)
- A rotina de tranca ainda seleciona perguntas pela competência civil sem excluir conversas e pode incluí-las na pauta `decidir_pelo_dp` ou no contador `perguntas_lidas_trancadas`. Isso é diferente da leitura C6 da tranca, já corrigida. (`app/chamados/services/competencia_trancada.py:107`; `app/chamados/services/competencia_trancada.py:129`; `app/chamados/services/competencia_trancada.py:132`)
- I21/I21a inclui conversas e consulta ausência pelo campo cru; F1 escolhe a primeira data de qualquer pergunta e julga `classificar_falta`; F3 consulta a falta pelo mesmo campo sem filtro conversa. **PROCEDENTE** a contagem de invariantes pelo dia civil. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1571`; `app/chamados/management/commands/auditar_invariantes_chamados.py:162`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1651`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1664`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1682`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1777`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1803`)

O censo existente nomeia tela/recusa (`folha.previa`, `dia_do_colab`, `data_do_fato_de`), mas **não nomeia esses auditores, o gate Chamado nem a pauta da tranca**; portanto não basta alegar que todos estão no censo. (`app/chamados/services/fio.py:1123`; `app/chamados/services/fio.py:1129`; `app/chamados/services/fio.py:1133`)

Ocorrência produtiva concreta desses caminhos: **[nao verificado]**.

**Item 9 — mapa de escritores/leitores e significado.**

| Campo/valor | Escritores | Leitores e conclusão |
|---|---|---|
| `via_resolucao` | `_escrever_carimbo` escreve o trio; descarimbo em lote zera; `encerrar_por_fato_no_espelho` e relavra escrevem somente a via como atos declarados. (`app/chamados/models.py:2830`; `app/chamados/models.py:2995`; `app/chamados/models.py:3004`; `app/chamados/services/relavra_via.py:125`) | C6, testemunha, SQL `q_vivas`, identidade do fato/índice, mérito do plano, idempotência, auditoria e cartório. `admin_decidiu` não garante terminalidade antes da resposta; `duplicata`/`motivo_retratado` carimbadas vencem a resposta e saem da identidade. **NAO PROCEDE** confundir todas as vias como “morta incondicionalmente”. (`app/chamados/juizes.py:660`; `app/chamados/juizes.py:1164`; `app/chamados/juizes.py:1169`; `app/chamados/juizes.py:1190`; `app/chamados/models.py:2581`; `app/chamados/models.py:3079`; `app/chamados/services/fio.py:1007`; `app/chamados/services/fio.py:1082`; `app/ponto/services/cartorio.py:1599`) |
| `retratada_em` | É **Batida**, não Pergunta. `retratar_batida` escreve instante pela porta `anotar`; desfazer humano zera pela mesma porta. (`app/ponto/models.py:67`; `app/ponto/registro_batida.py:274`; `app/ponto/services/retratar_humano.py:207`) | Geometria/apuração excluem; C6 exige batidas vivas; lavra lê instante para frescor; desfazer lê prazo. A idempotência do plantio pode ler batida crua para evitar replantar anulada: essa diferença de universo é declarada e corresponde a outra pergunta. (`app/ponto/turnos.py:2866`; `app/chamados/juizes.py:734`; `app/chamados/juizes.py:790`; `app/chamados/veredito_lavrado.py:140`; `app/ponto/services/retratar_humano.py:53`; `app/chamados/juizes.py:783`) |
| `recusa_motivo` | Duplicata pelo carimbo de identidade; recusa humana; resposta exigindo admin; pendência; recusa do materializador. (`app/chamados/services/fio.py:1092`; `app/chamados/models.py:2921`; `app/chamados/services/validacao.py:584`; `app/chamados/services/disputa_emissao.py:1432`; `app/chamados/services/materializacao.py:847`; `app/chamados/services/materializacao.py:1099`) | Explicador, I18/I19, revisão da validação e regex de batida vizinha no flip. **PROCEDENTE polivalência preexistente**: texto de duplicata, recusa, pendência e resposta aguardando julgamento; não significa exclusivamente veto de mérito. (`app/chamados/services/explicador.py:90`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1112`; `app/chamados/services/validacao.py:192`; `app/ponto/services/flip_auto.py:132`) |
| `data_evento` / `conversa` | Portas factual/conversa, `_nascer`, reapontamento e rechaveamento; identidade atribui instância somente fora de conversa. (`app/chamados/models.py:2643`; `app/chamados/models.py:2654`; `app/chamados/models.py:2751`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:959`; `app/chamados/models.py:3165`) | Identidade/horizonte, índices, universos, resolvedora e curas C6 distinguem conversa; gate Chamado, pauta, auditores e telas citados acima ainda leem dia civil como se factual. **PROCEDENTE significado residual incompatível**. (`app/chamados/models.py:2611`; `app/chamados/services/fio.py:1136`; `app/chamados/services/fio.py:1167`; `app/chamados/juizes.py:1986`; `app/chamados/models.py:364`; `app/chamados/services/competencia_trancada.py:107`) |
| `batida_origem` | Aceita na criação; quando ausente, `_chave_de_identidade` deriva uma vez do contexto de nascimento, conferindo família e colaborador. (`app/chamados/models.py:2696`; `app/chamados/models.py:3177`; `app/chamados/models.py:3190`; `app/chamados/models.py:3196`) | `do_fato`, índice BATIDA, agrupamento e plano leem episódio estável. O contexto é mutável e não pode recompor a coluna histórica. **NAO PROCEDE** confundir episódio salvo com “última batida atual” da Disputa. (`app/chamados/models.py:2579`; `app/chamados/models.py:3218`; `app/chamados/services/fio.py:1106`; `app/chamados/services/fio.py:1020`; `app/chamados/models.py:3185`) |
| `Disputa.motivos/contexto` | Emissão cria/agrega/funde; emissão de entradas pendentes injeta contexto; ajuste de âncora complementa; remoção/aparo retira motivos; anotação/renovação atualizam contexto; reabertura acrescenta motivo. (`app/chamados/services/disputa_emissao.py:293`; `app/chamados/services/disputa_emissao.py:541`; `app/chamados/services/disputa_emissao.py:562`; `app/chamados/services/disputa_emissao.py:589`; `app/chamados/services/disputa_emissao.py:1267`; `app/chamados/models.py:1936`; `app/chamados/models.py:2033`; `app/chamados/models.py:2074`; `app/chamados/models.py:2092`; `app/chamados/models.py:2129`) | Emissão lê lista/infos para perguntar; juiz/materializador usam fallback de data; nascimento deriva origem; auditores/telas leem escopo agregado. A lista de motivos não é um inventário de perguntas vivas: remover motivo mantém perguntas históricas, carimbando-as. (`app/chamados/services/disputa_emissao.py:345`; `app/chamados/services/disputa_emissao.py:372`; `app/chamados/juizes.py:377`; `app/chamados/services/materializacao.py:347`; `app/chamados/models.py:3192`; `app/chamados/models.py:2024`; `app/chamados/models.py:2031`) |

**Item 8, parte desta exploração:** PROCEDENTE campo com dois significados e leitores fora do censo; relógio interno preexistente continua em `abrir_conversa`, remoção e lavra. Novas ocorrências produtivas ou implantação: **[nao verificado]**. (`app/chamados/models.py:2654`; `app/chamados/models.py:1994`; `app/chamados/veredito_lavrado.py:112`; `app/chamados/veredito_lavrado.py:229`)

## 07/11 18:23 · sessao 3a1ea14e2abb · FIO-E0-V4-E1 · verificador · origem=Code

**Veredito: cura parcial.** O veto de mérito foi centralizado, mas permanecem divergências de universo, caminhos com ordem de trava invertida, leitores que confundem conversa com turno e diagnósticos descartados pelos comandos. (`app/chamados/services/fio.py:1049`; `app/chamados/services/fio.py:1162`; `app/chamados/services/texto.py:35`; `app/ponto/services/veredito_celula.py:177`; `app/chamados/management/commands/dedup_perguntas_fato.py:96`)

Identidade independente do commit/pai, execução da suíte informada, RED→GREEN histórico e ocorrência produtiva dos contraexemplos: **[nao verificado]**. [nao verificado]

**1. NAO PROCEDE segundo escritor produtivo de duplicata localizado; PROCEDENTE ressalva na declaração do mérito.**

O caminho produtivo encontrado é único:

`reapontar_resolvedora` / `_migrar_sob_trava` / `backfill.aplicar` → `retratar_para_canonica` → `retratar_duplicata` → `carimbar_identidade(VIA_DUPLICATA)`. Todos consultam `plano_de_retratacao`, cujo veto é `merito_perdedor`; não localizei cópia produtiva desse veto nem UPDATE direto marcando Pergunta como duplicata. (`app/chamados/services/fio.py:867`; `app/chamados/services/fio.py:1361`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:274`; `app/chamados/services/fio.py:1049`; `app/chamados/services/fio.py:1050`; `app/chamados/services/fio.py:1068`; `app/chamados/services/fio.py:1005`)

`retratada_em` pertence a **Batida**. A Pergunta sai do fato pela via `duplicata` ou `motivo_retratado`; remover motivo é outro ato, sem eleição de canônica. (`app/ponto/models.py:67`; `app/chamados/models.py:2926`; `app/chamados/models.py:2031`)

No contraexemplo p1/p2/p3, com universo comum e p1 canônica:

| Escritor/caminho | Resultado |
|---|---|
| Migração | p1 permanece; p2 conserva `admin_decidiu` e resposta, aparece em `recusadas`; p3 vira duplicata. (`app/chamados/services/fio.py:1336`; `app/chamados/services/fio.py:1340`; `app/chamados/services/fio.py:1360`; `app/chamados/services/fio.py:1361`) |
| Porta `retratar_para_canonica([p2,p3],p1)` | Preserva p2, declara seu mérito e retrata p3. (`app/chamados/services/fio.py:1033`; `app/chamados/services/fio.py:1049`; `app/chamados/services/fio.py:1051`) |
| Backfill/dedup apply | O mérito de p2 recusa o **apply inteiro**: p1/p2/p3 ficam como estavam; retratadas=0. Compatível com CLASSE-NA-CATRACA (3). (`app/chamados/management/commands/backfill_decisoes_pergunta.py:243`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:287`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:290`) |
| Colisão ao datar p1 | Recusa sem retratar ninguém; se p1 ganhou a data nessa tentativa, volta a NULL. Uma outra linha encerrada também basta para recusar antes do exame de mérito. (`app/chamados/services/fio.py:849`; `app/chamados/services/fio.py:852`; `app/chamados/services/fio.py:895`; `app/chamados/services/fio.py:903`) |

**Ressalva:** se a canônica é BATIDA sem episódio, o plano retorna antes de examinar mérito. A perdedora de mérito permanece protegida, mas não aparece em `merito_recusado`: aparece somente a PK da canônica em `canonica_sem_episodio`. (`app/chamados/services/fio.py:1030`; `app/chamados/services/fio.py:1032`; `app/chamados/services/fio.py:1033`)

**2. PROCEDENTE: há linhas preservadas sem resolver a colisão e caminhos sem garantia de resolvedora no destino.**

| Chamador | Linha preservada e linha `p`; declaração |
|---|---|
| Migração, `p` canônica | Havendo outra intocável no destino, `p` permanece no velho; ninguém é retratado; a recusa de `p` é declarada. (`app/chamados/services/fio.py:1346`; `app/chamados/services/fio.py:1350`) |
| Migração, canônica externa | Pode retratar `p` sem mover a canônica para o novo. Declara duplicatas/recusas, mas não garante uma resolvedora ancorada no novo. (`app/chamados/services/fio.py:1328`; `app/chamados/services/fio.py:1336`; `app/chamados/services/fio.py:1361`) |
| Colisão ao datar | Mérito, ausência de episódio, outra canônica ou linha encerrada recusam; `p` volta sem dia e as outras ficam intocadas. Não nasce uma segunda identidade datada nessa tentativa. (`app/chamados/services/fio.py:852`; `app/chamados/services/fio.py:892`; `app/chamados/services/fio.py:895`; `app/chamados/services/fio.py:903`) |
| Reapontamento de `p` já datada | Não verifica colisão: trava o colaborador, valida o destino e salva. Pode conservar duas perguntas do fato no destino sem declarar colisão. (`app/chamados/services/fio.py:854`; `app/chamados/services/fio.py:856`; `app/chamados/services/fio.py:868`) |
| Backfill/dedup apply | Mérito provoca rollback integral. Ausência de episódio é tratada como contador informativo: as linhas podem continuar juntas, com retorno de sucesso. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:242`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:246`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:290`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:293`) |
| DRY | Não escreve. O dedup conta perdedoras do horizonte sem consultar o plano; o backfill lê do plano somente mérito. A apresentação não corresponde integralmente ao que será retratado. (`app/chamados/management/commands/dedup_perguntas_fato.py:72`; `app/chamados/management/commands/dedup_perguntas_fato.py:78`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:373`) |

**Contraexemplo residual concreto:** L sem episódio no velho; A do episódio E no novo; mesmo motivo/dia; ambas vivas. `do_fato(E)` inclui L, mas `universo_da_linha(L)` retorna somente L. A migração não vê A como conflito, migra L e declara sucesso, deixando L e A no novo. (`app/chamados/models.py:2577`; `app/chamados/models.py:2579`; `app/chamados/services/fio.py:1162`; `app/chamados/services/fio.py:1332`; `app/chamados/services/fio.py:1334`; `app/chamados/services/fio.py:1355`)

Quando a canônica está fora dos dois chamados, a migração pode deixar o novo sem pergunta viva daquele fato; o teste “sem mérito” exige justamente retratar p2 e p3, mantendo p1 validada fora deles. O chamador `reabrir` registra o resultado e mantém o velho fechado; não procura outra resolvedora para cada recusa. (`app/chamados/tests/test_e1_v9.py:175`; `app/chamados/tests/test_e1_v9.py:179`; `app/chamados/tests/test_e1_v9.py:183`; `app/chamados/models.py:1397`; `app/chamados/models.py:1408`; `app/chamados/models.py:1409`)

Resolvedora efetivamente costurada a **outro fato** por esses contraexemplos: **[nao verificado]**. A lacuna demonstrada é duplicidade ou ausência de ancoragem, conforme os ramos acima.

**3. NAO PROCEDE a antiga inversão no corpo de `remover_motivo` e nos três pontos de justificativa; PROCEDENTE permanência da classe.**

`remover_motivo` agora adquire Colaborador; depois Disputas explícita/abertas; Chamados pais/vivos/destinos; finalmente todas as Perguntas listadas. Relê as precondições antes dos carimbos e do UPDATE da Disputa. (`app/chamados/models.py:1996`; `app/chamados/models.py:1998`; `app/chamados/models.py:2001`; `app/chamados/models.py:2031`; `app/chamados/models.py:2035`)

A implementação de `trava_na_ordem` segue Colaborador → Disputa → Chamado → Pergunta. Os receivers de Posto e âncora delegam à mesma porta; não têm escritor alternativo da remoção. Chamadas diretas entram no mesmo método. (`app/chamados/models.py:2525`; `app/chamados/models.py:2532`; `app/chamados/models.py:2545`; `app/chamados/models.py:2549`; `app/colaboradores/signals.py:77`; `app/escala/signals.py:161`; `app/chamados/tests/test_porta_remocao_motivo.py:255`)

**Limite:** o atomic da remoção termina antes de `self.fechar`. Sem atomic externo são duas transações; o teste do sinal fornece atomic externo e não demonstra essa chamada avulsa como ato indivisível. (`app/chamados/models.py:2035`; `app/chamados/models.py:2042`; `app/chamados/tests/test_e1_v9.py:276`)

Justificativa passou a incluir as Disputas abertas nos três pontos; a aprovação antecipa essa aquisição à trava da própria Justificativa. (`app/ponto/services/justificativa.py:41`; `app/ponto/services/justificativa.py:80`; `app/ponto/services/justificativa.py:129`; `app/ponto/services/justificativa.py:140`)

**Outro caminho invertido:** alterar texto de Chamado encerrado, com via diferente de `competencia_trancada`, faz atomic → **UPDATE Chamado** → S143 → aquisição de **Colaborador/Disputas**. O receiver não exige mudança de status nem filtra `update_fields`. Portanto permanece Chamado → Colaborador/Disputa. Portas de contexto e vínculos também salvam Chamado sob atomic sem antecipação local. (`app/chamados/services/texto.py:31`; `app/chamados/services/texto.py:35`; `app/chamados/signals.py:81`; `app/chamados/signals.py:92`; `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/services/disputa_emissao.py:1683`; `app/chamados/services/contexto.py:75`; `app/chamados/services/vinculos.py:86`)

**Outra falha no apply:** `_univ` usa a primeira PK como chave. Grupos α=`[L,Aα,Aα2]` e β=`[L,Bβ]`, com L menor PK compartilhada, sobrescrevem um universo. As travas podem cobrir somente L/Bβ, mas o laço posterior ainda processa α e retrata Aα2 quando Aα respondida vence. Aα2 — e eventual Disputa exclusiva de α — ficam fora da antecipação prometida. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:211`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:212`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:214`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:222`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:228`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:274`)

Há ainda INSERT/UPDATE sob atomic sem trava local em `Chamado.criar`, `PerguntaDisputa.save` e `ressuscitar_perguntas`. Isso demonstra alcance insuficiente de A/A2; não demonstra, sozinho, que todos os respectivos chamadores chegam destravados. (`app/chamados/models.py:1077`; `app/chamados/models.py:3151`; `app/chamados/models.py:1530`)

**4. NAO PROCEDE a antiga união α/β pela expansão de L; PROCEDENTE divergência residual entre universos.**

| Entrada | Retorno |
|---|---|
| BATIDA sem episódio L | `universo_da_linha(L)=[L]`. (`app/chamados/services/fio.py:1162`) |
| BATIDA com episódio E | Linhas de E/motivo, independentemente do dia, mais legadas sem episódio do **dia da linha consultada**, acrescentando a própria linha. (`app/chamados/models.py:2577`; `app/chamados/models.py:2579`; `app/chamados/services/fio.py:1165`; `app/chamados/services/fio.py:1166`) |
| MARCO | Dia/motivo/instância derivada, mais a própria linha. Nascimento e leitura usam a mesma derivação de instância. (`app/chamados/models.py:2610`; `app/chamados/models.py:3168`; `app/chamados/models.py:3175`; `app/chamados/services/fio.py:1166`) |
| Conversa | O helper não tem ramo geral próprio: pode consultar linhas factuais e acrescentar a conversa; BATIDA sem origem retorna somente ela. Os consumidores atuais evitam essa mistura: grupo exclui conversa e migração usa explicitamente `[p]`. (`app/chamados/services/fio.py:1162`; `app/chamados/services/fio.py:1165`; `app/chamados/services/fio.py:1146`; `app/chamados/services/fio.py:1329`) |

`universo_do_grupo` expande cada linha inicial, une por PK e ordena. L não expande novamente pelo dia; desapareceu o contraexemplo específico L+A(α)+B(β). (`app/chamados/services/fio.py:1149`; `app/chamados/services/fio.py:1152`; `app/chamados/services/fio.py:1162`)

**Persistem duas assimetrias:**

- L isolada não vê E, enquanto E vê L: causa a duplicidade da migração descrita no item 2. (`app/chamados/services/fio.py:1162`; `app/chamados/models.py:2577`)
- Mesmo episódio E com A(d1), A2(d2) e L2(d2) respondida: `universo_da_linha(A)` encontra A/A2, mas não L2; `universo_do_grupo([A,A2])` expande A2 e encontra L2. Migração e backfill podem escolher canônicas diferentes para o mesmo episódio. (`app/chamados/models.py:2577`; `app/chamados/models.py:2579`; `app/chamados/services/fio.py:1149`; `app/chamados/services/fio.py:1332`; `app/chamados/services/fio.py:1336`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:228`)

**5. PROCEDENTE: o censo de `data_evento` continua incompleto.**

Os escritores declaram dois significados: `abrir` recebe dia do turno e fixa `conversa=False`; `abrir_conversa` lê o dia civil e fixa `True`; `_nascer` persiste. Reapontamento preenche NULL; rechaveamento altera dia existente. (`app/chamados/models.py:2643`; `app/chamados/models.py:2654`; `app/chamados/models.py:2751`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:957`)

Os filtros novos corrigem os leitores nomeados no diff, inclusive o I22 composto: conversa deixou de satisfazer seu `any` factual. **NAO PROCEDE a permanência daquela cegueira específica.** (`app/chamados/juizes.py:77`; `app/chamados/juizes.py:941`; `app/chamados/services/fabrica_da_celula.py:127`; `app/chamados/services/relavra_via.py:154`; `app/chamados/services/pergunta_no_app.py:93`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1045`)

Entretanto, permanecem decisores fora do censo:

| Leitor | Consequência |
|---|---|
| `fechar_o_fio_do_dia` | Seleciona por data sem excluir conversa; fecha por admin e registra SILENCIO. Decidir turno D pode silenciar conversa emitida em D sobre outro assunto. (`app/ponto/services/veredito_celula.py:177`; `app/ponto/services/veredito_celula.py:187`; `app/ponto/services/veredito_celula.py:192`) |
| C6 `_pergunta_viva_calc` | Consulta janela e `classificar_falta` pelo dia civil; pode julgar a vitalidade da conversa pela falta de outro turno. (`app/chamados/juizes.py:1207`; `app/chamados/juizes.py:1211`) |
| `_pergunta_materializada_de_fato` | Consulta janela/falta pelo mesmo campo sem discriminador. (`app/chamados/juizes.py:664`; `app/chamados/juizes.py:671`) |
| Invariante F1 | Inclui conversas, escolhe a primeira data de pergunta e julga a falta desse dia; a conversa pode determinar a data examinada. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1651`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1664`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1682`) |
| I21/I21a e F3 | Usam cobertura de ausência/falta sobre a data sem excluir conversa, alterando a contagem dos invariantes. (`app/chamados/management/commands/auditar_invariantes_chamados.py:162`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1571`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1777`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1803`) |
| `limbo_folgas.filhos_no_defeito` | Uma conversa do dia civil pode segurar outro filho pelo `fio_vivo`. (`app/chamados/services/limbo_folgas.py:201`; `app/chamados/services/limbo_folgas.py:204`) |

Não exige flag corrompida: o emissor cria conversa legítima com o motivo original quando não encontra dia factual. (`app/chamados/services/disputa_emissao.py:431`; `app/chamados/services/disputa_emissao.py:439`)

O censo também reconhece que `data_do_fato_de` compara civil com turno e pode causar recusa de materialização com escrita de `recusa_motivo`. Portanto “nenhum planta, retrata nem silencia” não equivale a ausência de decisão/escrita. (`app/chamados/services/fio.py:1106`; `app/chamados/services/fio.py:1109`; `app/chamados/services/materializacao.py:1189`; `app/chamados/services/materializacao.py:1210`)

**6. NAO PROCEDE a antiga classificação de NULL como abaixo; PROCEDENTE divergência de contagem e apresentação.**

| Retorno | Quem escreve | Quem lê |
|---|---|---|
| `retratadas` | Soma retratações efetivas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:281`) | Ambos os comandos. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:430`; `app/chamados/management/commands/dedup_perguntas_fato.py:96`) |
| `abaixo_do_horizonte` | Perdedoras **datadas** fora do horizonte. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:235`) | Backfill imprime; dedup somente exclui das recusas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:430`; `app/chamados/management/commands/dedup_perguntas_fato.py:92`) |
| `perdedora_sem_dia` | Perdedoras NULL. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:234`) | Dedup somente classifica como não recusa; nenhum dos dois resumos apresenta a lista. (`app/chamados/management/commands/dedup_perguntas_fato.py:92`; `app/chamados/management/commands/dedup_perguntas_fato.py:97`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:429`) |
| `canonica_sem_episodio` | Plano devolve a **PK da canônica**, não as perdedoras bloqueadas. (`app/chamados/services/fio.py:1031`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:244`) | Mesma classificação de não recusa, sem apresentação nos resumos. (`app/chamados/management/commands/dedup_perguntas_fato.py:92`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:429`) |
| `sem_episodio_intocada` | Plano devolve perdedoras legadas. (`app/chamados/services/fio.py:1037`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:245`) | Mesma omissão de apresentação. (`app/chamados/management/commands/dedup_perguntas_fato.py:92`; `app/chamados/management/commands/dedup_perguntas_fato.py:97`) |
| `merito_recusado` | Plano; apply acumula. (`app/chamados/services/fio.py:1036`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:243`) | Migração, colisão, DRY backfill e ambos os applies. (`app/chamados/services/fio.py:1340`; `app/chamados/services/fio.py:903`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:373`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:421`; `app/chamados/management/commands/dedup_perguntas_fato.py:95`) |

**Não é uma partição das linhas:** L canônica sem episódio bloqueando A1/A2 produz somente `[L]`; A1/A2 ficam fora de todas essas listas de destino. L também pode aparecer nos dois contadores de episódio, sendo canônica de um grupo e perdedora de outro; a deduplicação é por lista, não entre categorias. (`app/chamados/services/fio.py:1030`; `app/chamados/services/fio.py:1032`; `app/chamados/services/fio.py:1037`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:293`)

**DRY ≠ apply:** L canônica legada + A(E) dá um excedente no DRY do dedup e zero retratada no apply; o motivo fica escondido na saída. O DRY backfill igualmente descarta do plano os resultados sem episódio. (`app/chamados/management/commands/dedup_perguntas_fato.py:73`; `app/chamados/management/commands/dedup_perguntas_fato.py:78`; `app/chamados/management/commands/dedup_perguntas_fato.py:96`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:373`)

**NAO PROCEDE mistura de escrita persistida e recusa no retorno normal do apply:** qualquer recusa acumulada causa rollback e zera os contadores. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:287`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:290`)

**7. PROCEDENTE insuficiência dos selos fora do LIMITE declarado; NAO PROCEDE ausência geral de mordida.**

| Catraca | Burla adicional |
|---|---|
| A | SQL literal com `FOR  UPDATE`, dois espaços, escapa da substring exata; não depende de concatenação ou tabela dinâmica. (`app/chamados/tests/test_e1_v7_catracas.py:129`) |
| A2 | Alias `gravar=self.save; gravar()` e decorador `@transaction.atomic` escapam: procura `ast.With` e nomes de chamadas. Além disso, para na primeira trava, mesmo que ela adquira só Colaborador. (`app/chamados/tests/test_e1_v7_catracas.py:319`; `app/chamados/tests/test_e1_v7_catracas.py:323`; `app/chamados/tests/test_e1_v7_catracas.py:327`) |
| B | `rec: list=[p for p in ps if p.ok]` seguido do juiz escapa: propagação considera `Assign`, não `AnnAssign`. Alias local do juiz também escapa. (`app/chamados/tests/test_e1_v7_catracas.py:181`; `app/chamados/tests/test_e1_v7_catracas.py:194`;

[truncado em 20000 de 29192 caracteres]

## 07/11 18:19 · sessao 1c46dadb09e8 · - · - · origem=Ronald (tmux)

**PROCEDENTE a perda de universo em `_univ`.** Os grupos α=`[L,Aα,Aα2]` e β=`[L,Bβ]`, ambos começando pela PK de L, colidem na chave do dicionário; β sobrescreve α. Assim, `pks_c` pode conter somente L/Bβ, deixando Aα, Aα2 e eventual Disputa exclusiva de α fora das travas explícitas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:211`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:212`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:214`)

O processamento posterior continua percorrendo **todos** os grupos. Com Aα respondida, ela vence; Aα2 é planejada para retratação, e L fica intocada por falta de episódio. A escrita alcança Aα2 sem antecipar sua trava pela porta: `aplicar → retratar_para_canonica → retratar_duplicata → carimbar_identidade → save`. Isso contradiz a afirmação de que o universo inteiro foi travado antes dos vetos. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:222`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:228`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:242`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:274`; `app/chamados/services/fio.py:1050`; `app/chamados/services/fio.py:1068`; `app/chamados/models.py:2923`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:207`)

Reprodução em banco: **[nao verificado]**.

## 07/11 18:19 · sessao 348184f1c6dc · - · - · origem=Ronald (tmux)

**3 — PROCEDENTE residual de ordem; NAO PROCEDE permanência da inversão no corpo de `remover_motivo`.** Ele trava Colaborador, depois Disputas explícita/abertas, Chamados pais/vivos/destinos e todas as Perguntas antes de qualquer carimbo; relê as precondições sob trava. (`app/chamados/models.py:1995`; `app/chamados/models.py:1998`; `app/chamados/models.py:2001`; `app/chamados/models.py:2031`). A implementação da porta adquire cada tabela nessa ordem. (`app/chamados/models.py:2525`; `app/chamados/models.py:2532`; `app/chamados/models.py:2545`; `app/chamados/models.py:2549`).

Posto e âncora somente consultam e delegam a essa porta; seus UPDATE anteriores são de Posto/Escala, fora das quatro tabelas. Produção tem esses dois chamadores; chamadas diretas estão nos testes. (`app/colaboradores/signals.py:71`; `app/colaboradores/signals.py:77`; `app/escala/signals.py:155`; `app/escala/signals.py:161`; `app/chamados/tests/test_porta_remocao_motivo.py:255`).

**Ressalva concreta:** o atomic de `remover_motivo` termina antes de `self.fechar`; sem atomic externo, remoção e fechamento ficam em transações diferentes. O teste do sinal adiciona atomic externo e não cobre essa separação. (`app/chamados/models.py:2035`; `app/chamados/models.py:2042`; `app/chamados/tests/test_e1_v9.py:276`).

**Justificativa — NAO PROCEDE a permanência da omissão apontada:** os três pontos incluem `abertas_do_colaborador=True`; aprovação chama a antecipação antes da trava da justificativa. (`app/ponto/services/justificativa.py:41`; `app/ponto/services/justificativa.py:80`; `app/ponto/services/justificativa.py:129`; `app/ponto/services/justificativa.py:140`).

**Outro caminho realmente invertido:** `reescrever_texto(ch)` em Chamado terminal, via diferente de `competencia_trancada`, texto alterado e sem trava externa: atomic → UPDATE Chamado → S143 → porta que adquire Colaborador/Disputas. O receiver não exige mudança de status nem examina `update_fields`. **Chamado → Colaborador/Disputa continua possível.** (`app/chamados/services/texto.py:31`; `app/chamados/services/texto.py:35`; `app/chamados/signals.py:81`; `app/chamados/signals.py:91`; `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/services/disputa_emissao.py:1683`). Contexto e vínculos também salvam Chamado sob atomic sem antecipação. (`app/chamados/services/contexto.py:75`; `app/chamados/services/contexto.py:100`; `app/chamados/services/vinculos.py:86`).

Outras escritas sob atomic sem trava local: INSERT de `Chamado.criar`, `PerguntaDisputa.save` e descarimbo em lote de `ressuscitar_perguntas`; este último depois pode reabrir Disputa. Isso demonstra escritores implícitos fora do alcance de A/A2, sem afirmar que todos os chamadores chegam destravados. (`app/chamados/models.py:1077`; `app/chamados/models.py:1530`; `app/chamados/models.py:1560`; `app/chamados/models.py:2147`; `app/chamados/models.py:3151`).

**7 — PROCEDENTE insuficiência dos selos fora dos limites declarados; NAO PROCEDE ausência geral de mordida.**

| Selo | Burla confirmada em execução pura do detector |
|---|---|
| A | SQL literal com tabela protegida e `FOR  UPDATE` — dois espaços — retorna `[]`: busca substring exata. (`app/chamados/tests/test_e1_v7_catracas.py:129`) |
| A2 | `gravar=self.save; gravar()` sob atomic retorna `[]`; só examina nomes de chamadas. Decorador `@transaction.atomic` também fica fora da procura por `ast.With`. (`app/chamados/tests/test_e1_v7_catracas.py:319`; `app/chamados/tests/test_e1_v7_catracas.py:323`) |
| B | `rec: list=[p for p in ps if p.ok]; canonica_do_grupo(rec)` retorna `[]`; só propaga `ast.Assign`. `juiz=canonica_do_grupo; juiz(recorte)` também escapa. (`app/chamados/tests/test_e1_v7_catracas.py:181`; `app/chamados/tests/test_e1_v7_catracas.py:194`; `app/chamados/tests/test_e1_v7_catracas.py:202`) |
| C | SQL UPDATE direto pelo cursor não pertence à lista de escritores; o único limite declarado de C é helper externo. (`app/chamados/tests/test_e1_v7_catracas.py:34`; `app/chamados/tests/test_e1_v7_catracas.py:214`; `app/chamados/tests/test_e1_v7_catracas.py:238`) |
| D | `p.carimbar_identidade("duplicata",u)` retorna `[]`; também `carimba=p.carimbar_identidade; carimba(P.VIA_DUPLICATA,u)`. A constante vale literalmente `"duplicata"`, mas o detector exige chamada com nome `carimbar_identidade` e argumento AST chamado `VIA_DUPLICATA`. (`app/chamados/models.py:2364`; `app/chamados/tests/test_e1_v7_catracas.py:280`) |

A2 aceita parar na primeira trava, mesmo que ela trave somente Colaborador e depois haja Pergunta → Disputa. O próprio mutante “curado” só insere `trava_na_ordem(self.colaborador_id)`, insuficiente para provar a ordem inteira. (`app/chamados/tests/test_e1_v7_catracas.py:327`; `app/chamados/tests/test_e1_v9.py:544`). Fluxo/helper estão declarados como limites, mas alias de método, decorador atomic e suficiência do conjunto travado não estão. (`app/chamados/tests/test_e1_v7_catracas.py:38`).

**Tracker — NAO PROCEDE que ainda registre só primeira tabela.** Agora deduplica por `(tabela,pk)`; para as consultas da porta, a primeira coluna é efetivamente PK. O UPDATE usual sonda o WHERE e remove os parâmetros do SET. (`app/chamados/tests/test_e1_v9.py:60`; `app/chamados/tests/test_e1_v9.py:80`; `app/chamados/tests/test_e1_v9.py:85`; `app/chamados/models.py:2533`). Os mutantes exigem detectar segunda Disputa e UPDATE sem Colaborador. (`app/chamados/tests/test_e1_v9.py:318`; `app/chamados/tests/test_e1_v9.py:328`).

**PROCEDENTE excesso de alcance se apresentado como todas as aquisições:** ele não intercepta INSERT, DELETE ou FOR NO KEY UPDATE; `vistas` não reseta quando uma transação/savepoint libera trava. Seu limite cita apenas executemany, FK do INSERT e ordem PK, e os testes usam atomic externo. (`app/chamados/tests/test_e1_v9.py:47`; `app/chamados/tests/test_e1_v9.py:51`; `app/chamados/tests/test_e1_v9.py:70`; `app/chamados/tests/test_e1_v9.py:85`; `app/chamados/tests/test_e1_v9.py:277`). Correspondência da sonda prévia com locks efetivamente obtidos depois de espera concorrente: **[nao verificado]**.

Executei os 30 testes puros dos detectores v7 + `CatracaAlcanceTest` v9 em memória, substituindo `SimpleTestCase` por `unittest.TestCase` e retirando somente imports entre testes: **30 OK**; as varreduras atuais A/B/C/D/A2 resultaram vazias. Resultado dessa execução: **[nao verificado por arquivo persistido]**. Ela verifica mordida sintética atual; RED→GREEN histórico e testes Django: **[nao verificado]**. [nao verificado]

**Relógio — NAO PROCEDE permanência do defeito novo localizado:** classes de banco v8/v9 estão congeladas; os testes puros usam datas literais e mocks. (`app/chamados/tests/test_e1_v8.py:46`; `app/chamados/tests/test_e1_v8.py:100`; `app/chamados/tests/test_e1_v8.py:130`; `app/chamados/tests/test_e1_v9.py:149`; `app/chamados/tests/test_e1_v9.py:221`; `app/chamados/tests/test_e1_v9.py:266`; `app/chamados/tests/test_e1_v9.py:337`; `app/chamados/tests/test_e1_v9.py:443`).

## 07/11 18:19 · sessao 1c46dadb09e8 · - · - · origem=Ronald (tmux)

**Itens 1/2/4/6 — consolidação só por código.**

1. **NAO PROCEDE segundo escritor produtivo de `VIA_DUPLICATA` localizado.** A escrita está em `fio.retratar_duplicata`, que chama `carimbar_identidade`; seu único chamador produtivo é `retratar_para_canonica`. Os consumidores são reapontamento, migração e apply do backfill. (`app/chamados/services/fio.py:1068`; `app/chamados/services/fio.py:1050`; `app/chamados/services/fio.py:867`; `app/chamados/services/fio.py:1361`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:274`)

   `retratada_em` pertence a **Batida**, não representa a retratação da Pergunta para uma canônica; a retratação da Batida passa por `retratar_batida`, e seu desfazimento por `anotar`. (`app/ponto/models.py:67`; `app/ponto/registro_batida.py:274`; `app/ponto/services/retratar_humano.py:207`)

   **Contraexemplo nominal do aval:** migração mantém p1 canônica, preserva p2 com mérito e declara p2 recusada, retrata p3. O apply identifica o mesmo mérito, mas desfaz o ato inteiro: p1/p2/p3 permanecem como estavam, contadores zerados. Essa diferença decorre do envelope de tudo ou nada, não de cópia do veto. (`app/chamados/services/fio.py:1336`; `app/chamados/services/fio.py:1340`; `app/chamados/services/fio.py:1360`; `app/chamados/services/fio.py:1361`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:243`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:287`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:290`)

2. **PROCEDENTE persistência de resultados sem eliminar as duas linhas, e declaração incompleta pelos comandos.**

   - Datar p com colisão e mérito/sem episódio recusa falando; restaura `p.data_evento=None` e não retrata as outras. Se p não é canônica ou alguma outra já está encerrada, também recusa antes de escrever. (`app/chamados/services/fio.py:849`; `app/chamados/services/fio.py:852`; `app/chamados/services/fio.py:892`; `app/chamados/services/fio.py:895`; `app/chamados/services/fio.py:903`)
   - Na migração, p canônica com outra linha intocável no destino permanece no velho e sai recusada. Se a canônica é externa, p pode virar duplicata sem mover a canônica para o novo; o retorno registra duplicata/recusa, mas não garante uma resolvedora ancorada no novo. (`app/chamados/services/fio.py:1328`; `app/chamados/services/fio.py:1346`; `app/chamados/services/fio.py:1350`; `app/chamados/services/fio.py:1361`)
   - **Contraexemplo residual:** L sem episódio no velho, A de episódio E no destino, mesmo motivo/dia. A identidade consultada por E inclui L, porém `universo_da_linha(L)` devolve somente L; a migração não vê A como conflito, migra L e retorna sucesso, deixando L e A no destino. O reapontamento de pergunta já datada também não examina colisões: apenas trava colaborador, valida o destino e salva. (`app/chamados/models.py:2577`; `app/chamados/models.py:2579`; `app/chamados/services/fio.py:1162`; `app/chamados/services/fio.py:1332`; `app/chamados/services/fio.py:1334`; `app/chamados/services/fio.py:1355`; `app/chamados/services/fio.py:854`; `app/chamados/services/fio.py:868`)
   - O chamador `reabrir` mantém o velho fechado e apenas anexa nota/evento com o resultado da migração; não trata individualmente recusas nem procura outra resolvedora. (`app/chamados/models.py:1397`; `app/chamados/models.py:1400`; `app/chamados/models.py:1408`; `app/chamados/models.py:1409`)
   - No apply, linha sem episódio pode impedir retratação sem constituir recusa; duas linhas permanecem e o retorno é de sucesso com contador informativo. Os comandos descartam esse diagnóstico ao apresentar o resultado. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:242`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:246`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:293`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:429`; `app/chamados/management/commands/dedup_perguntas_fato.py:92`; `app/chamados/management/commands/dedup_perguntas_fato.py:97`)

3. **Item 4 — cura do contraexemplo L+A(α)+B(β), mas PROCEDENTE divergência residual dos universos.**

   - L BATIDA sem episódio: `universo_da_linha` retorna `[L]`. Episódio E: retorna todas as linhas daquele episódio/motivo, independentemente do dia, mais legadas sem episódio do dia da linha consultada. MARCO: usa dia/motivo e a instância derivada do motivo, acrescentando a própria linha caso ausente. (`app/chamados/services/fio.py:1162`; `app/chamados/services/fio.py:1165`; `app/chamados/services/fio.py:1166`; `app/chamados/models.py:2577`; `app/chamados/models.py:2579`; `app/chamados/models.py:2610`)
   - `universo_do_grupo` exclui conversas e linhas fora do fato, expande cada linha inicial, une por PK e ordena. A legada encontrada durante a expansão não é expandida novamente; portanto o contraexemplo específico que unia α e β pela legada foi eliminado. (`app/chamados/services/fio.py:1146`; `app/chamados/services/fio.py:1149`; `app/chamados/services/fio.py:1152`; `app/chamados/services/fio.py:1162`)
   - **Divergência estática:** mesmo episódio E com A(d1), A2(d2), e legada L2(d2) respondida. `universo_da_linha(A)` encontra A/A2, mas não L2; `universo_do_grupo([A,A2])` expande também A2 e inclui L2. Migração iniciada por A pode escolher canônica diferente de backfill/DRY/abrir para d2. A chave BATIDA permite a diferença de dias; não é necessário inventar outro episódio. (`app/chamados/models.py:2577`; `app/chamados/models.py:2579`; `app/chamados/services/fio.py:1149`; `app/chamados/services/fio.py:1165`; `app/chamados/services/fio.py:1332`; `app/chamados/services/fio.py:1336`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:228`)
   - Conversa passada diretamente a `universo_da_linha` não recebe tratamento geral próprio: salvo BATIDA sem origem, busca linhas factuais por `do_fato` e depois acrescenta a conversa. `universo_do_grupo` a exclui; a migração evita esse caso por ramo explícito `[p]`. **Não localizado consumidor produtivo que entregue conversa ao helper nessa forma.** (`app/chamados/services/fio.py:1162`; `app/chamados/services/fio.py:1165`; `app/chamados/services/fio.py:1167`; `app/chamados/services/fio.py:1146`; `app/chamados/services/fio.py:1329`)
   - **NAO PROCEDE defeito MARCO multi-instância demonstrado:** o nascimento rejeita instância diferente daquela derivada do motivo; o leitor usa a mesma derivação. Duas pausas distintas continuam explicitamente reservadas à E2. (`app/chamados/models.py:3168`; `app/chamados/models.py:3175`; `app/chamados/models.py:2610`; `app/chamados/services/declaracao_texto.py:48`)

4. **Item 6 — PROCEDENTE diferença entre contadores, DRY e apresentação do apply.**

   | Valor | Escritor | Leitor produtivo |
   |---|---|---|
   | `retratadas` | Soma escritas efetivas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:281`) | Backfill e dedup. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:430`; `app/chamados/management/commands/dedup_perguntas_fato.py:96`) |
   | `abaixo_do_horizonte` | Perdedoras datadas fora do horizonte; saída deduplica PKs. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:235`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:293`) | Backfill imprime quantidade; dedup apenas exclui das recusas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:430`; `app/chamados/management/commands/dedup_perguntas_fato.py:92`) |
   | `perdedora_sem_dia` | Perdedoras NULL. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:234`) | Dedup apenas exclui das recusas; CLI backfill não imprime. (`app/chamados/management/commands/dedup_perguntas_fato.py:92`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:429`) |
   | `canonica_sem_episodio` | Plano devolve **PK da canônica**, não das perdedoras bloqueadas. (`app/chamados/services/fio.py:1030`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:244`) | Apenas classificação de não recusa no dedup. (`app/chamados/management/commands/dedup_perguntas_fato.py:92`) |
   | `sem_episodio_intocada` | PKs das perdedoras legadas. (`app/chamados/services/fio.py:1037`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:245`) | Apenas classificação de não recusa no dedup. (`app/chamados/management/commands/dedup_perguntas_fato.py:92`) |
   | `merito_recusado` | Plano/porta; apply acumula. (`app/chamados/services/fio.py:1033`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:243`) | Migração, colisão, DRY backfill e ambos CLIs apply. (`app/chamados/services/fio.py:1340`; `app/chamados/services/fio.py:903`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:373`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:421`; `app/chamados/management/commands/dedup_perguntas_fato.py:95`) |

   A perdedora NULL deixou de ser contada como abaixo: **NAO PROCEDE a permanência desse defeito específico**. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:234`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:235`)

   **Contador não particiona as linhas:** com L canônica sem episódio e A1/A2 perdedoras datadas no horizonte, o plano devolve somente `canonica_sem_episodio=[L]`; A1/A2 não aparecem como retratadas, sem dia, abaixo ou sem episódio intocadas. A PK L pode aparecer nos dois contadores sem episódio em grupos diferentes, como canônica de um e perdedora de outro. (`app/chamados/services/fio.py:1030`; `app/chamados/services/fio.py:1032`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:231`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:244`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:293`)

   O DRY do dedup conta toda perdedora no horizonte, sem consultar o plano: L canônica legada + A(E) produz DRY excedentes=1 e apply excedentes=0; o motivo desaparece da apresentação. O DRY backfill consulta o plano somente para `merito_recusado`, omitindo os resultados sem episódio. (`app/chamados/management/commands/dedup_perguntas_fato.py:72`; `app/chamados/management/commands/dedup_perguntas_fato.py:78`; `app/chamados/management/commands/dedup_perguntas_fato.py:96`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:373`)

   Se a canônica é legada, o plano retorna antes de examinar mérito: uma perdedora `admin_decidiu` com resposta fica protegida, porém o mérito não é declarado como tal. (`app/chamados/services/fio.py:1030`; `app/chamados/services/fio.py:1033`)

Execução em banco e ocorrência produtiva destes contraexemplos: **[nao verificado]**.

## 07/11 18:18 · sessao 770621486b10 · - · - · origem=Ronald (tmux)

**Itens 5 e 9: PROCEDENTE a permanência de leitores que confundem dia civil de conversa com dia de turno; o censo continua incompleto.** As migrações enumeradas no diff existem, mas não cobrem os leitores abaixo. (`app/chamados/services/fio.py:1100`; `app/chamados/services/fio.py:1106`)

Contraexemplos fortes:

- **Escrita que silencia conversa por coincidência de dia civil:** `fechar_o_fio_do_dia` seleciona perguntas por `data_evento=data`, usando `q_vivas`, sem `conversa=False`; chama `fechar_por_admin` e registra SILENCIO. `q_vivas` tampouco exclui conversa. Assim, decidir um turno em D alcança uma conversa emitida em D sobre outro fato, deixando `admin_decidiu` e trilha de decisão daquele dia. (`app/ponto/services/veredito_celula.py:177`; `app/ponto/services/veredito_celula.py:187`; `app/ponto/services/veredito_celula.py:192`; `app/chamados/models.py:3074`; `app/chamados/models.py:3088`; `app/chamados/models.py:2797`)
- **O próprio C6 ainda julga conversa pela falta de seu dia civil:** `_pergunta_viva_calc` chega a `_dentro_da_janela_viva(p.data_evento)` e `classificar_falta(colab,p.data_evento)` sem discriminar conversa. Com carimbo derivado e sem resposta, pode ressuscitar pela falta de um turno que não é o assunto da conversa. O juiz de materialização também lê esse dia como turno. (`app/chamados/juizes.py:1207`; `app/chamados/juizes.py:1211`; `app/chamados/juizes.py:664`; `app/chamados/juizes.py:671`)
- **Invariante F1:** lê todas as resolvedoras, escolhe a primeira `data_evento`, antes de consultar a célula do chamado, e decide a absolvição pela lei daquela data. Uma conversa morta e muda pode determinar o dia civil escolhido e ocultar o furo do chamado. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1651`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1664`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1665`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1682`)
- **I21/I21a:** a consulta inclui conversa; a cobertura de ausência usa seu `data_evento` civil. Uma ausência aprovada naquele dia pode mandar uma resposta invisível de conversa para o piso, sem resolver o assunto da conversa. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1571`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1578`; `app/chamados/management/commands/auditar_invariantes_chamados.py:160`; `app/chamados/management/commands/auditar_invariantes_chamados.py:162`)
- **F3:** universo por motivo e data, sem conversa, seguido de `classificar_falta` pelo dia civil. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1777`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1803`)
- **Recusa/retém outro fato:** `limbo_folgas.filhos_no_defeito` consulta todas as perguntas do dia e usa `fio_vivo` para segurar o filho; uma conversa daquele dia civil basta. O calendário também marca `_perg_viva[data_evento]`, influenciando a oferta de “Resolver dia”, sem excluir conversa. (`app/chamados/services/limbo_folgas.py:201`; `app/chamados/services/limbo_folgas.py:204`; `app/colaboradores/services/calendario.py:455`; `app/colaboradores/services/calendario.py:473`; `app/colaboradores/services/calendario.py:477`)

Não depende de corrupção da flag: existem conversas legítimas para motivos genéricos quando não há dia do fato; a porta é chamada com o motivo original. A âncora também nasce explicitamente como conversa. (`app/chamados/services/disputa_emissao.py:431`; `app/chamados/services/disputa_emissao.py:439`; `app/chamados/services/disputa_emissao.py:1272`)

Mapa de campos/valores:

| Campo | Escritores | Leitores e significado |
|---|---|---|
| `data_evento` | Fato: `abrir` exige dia e fixa `conversa=False`; conversa: `abrir_conversa` usa `localdate()` e fixa `True`; `_nascer` cria; reapontamento preenche NULL; rechaveamento altera dia existente. (`app/chamados/models.py:2641`; `app/chamados/models.py:2643`; `app/chamados/models.py:2654`; `app/chamados/models.py:2751`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:957`) | Identidade/horizonte distinguem conversa; resolvedora também. O diagnóstico `data_do_fato_de` compara os dois dias e materialização pode recusar e gravar DATA_EM_DISPUTA: esse leitor está declarado no censo. Os leitores acima não estão e ainda tratam civil como turno. (`app/chamados/models.py:2611`; `app/chamados/services/fio.py:1113`; `app/chamados/juizes.py:1976`; `app/chamados/services/fio.py:87`; `app/chamados/services/materializacao.py:1189`; `app/chamados/services/materializacao.py:1210`) |
| `conversa` | Duas portas fixam o valor; `_nascer` persiste e transmite ao proxy do juiz. (`app/chamados/models.py:2643`; `app/chamados/models.py:2654`; `app/chamados/models.py:2685`; `app/chamados/models.py:2752`) | Sem identidade de fato: filtros/índices e derivador de chave respeitam; resolvedora não compara seu civil ao turno. Não é respeitado pelos leitores acima. (`app/chamados/models.py:2611`; `app/chamados/models.py:3165`; `app/chamados/models.py:3213`; `app/chamados/juizes.py:1976`) |
| `via_resolucao` | `_escrever_carimbo`; descarimbo em lote; exceção `encerrar_por_fato_no_espelho`; relavra. (`app/chamados/models.py:2834`; `app/chamados/models.py:2995`; `app/chamados/models.py:3004`; `app/chamados/services/relavra_via.py:125`) | C6 lê identidade/terminalidade, modelo exclui vias fora do fato, mérito lê `admin_decidiu` mais resposta, retratador lê idempotência. `admin_decidiu` sozinho não elimina resposta pendente; duplicata carimbada elimina antes da resposta. (`app/chamados/juizes.py:1155`; `app/chamados/juizes.py:1160`; `app/chamados/juizes.py:1180`; `app/chamados/models.py:2611`; `app/chamados/services/fio.py:1005`; `app/chamados/services/fio.py:1059`) |
| `retratada_em` | **Campo de Batida**, escrito por `retratar_batida` via `anotar`, e limpo pelo desfazer humano também via `anotar`. Não é campo de Pergunta. (`app/ponto/models.py:67`; `app/ponto/registro_batida.py:274`; `app/ponto/services/retratar_humano.py:207`) | Batida fora da apuração; juiz exige testemunha viva; timestamp alimenta frescor da lavra. A retratação da Pergunta é sua via terminal, não esse timestamp. (`app/ponto/turnos.py:2849`; `app/chamados/juizes.py:726`; `app/chamados/juizes.py:781`; `app/chamados/veredito_lavrado.py:140`; `app/chamados/models.py:2926`) |
| `recusa_motivo` | Duplicata via `carimbar_identidade`; recusa humana; justificativa da resposta; pendência; recusa de materialização. (`app/chamados/services/fio.py:1069`; `app/chamados/models.py:2921`; `app/chamados/services/validacao.py:584`; `app/chamados/services/disputa_emissao.py:1432`; `app/chamados/services/materializacao.py:847`; `app/chamados/services/materializacao.py:1097`; `app/chamados/services/materializacao.py:1099`) | É texto de proveniências diferentes, dívida já declarada; explicador mostra; I18/I19 usam presença como dono; devolução de validação copia para ato; flip automático extrai batida vizinha por regex. Portanto não tem significado único de “recusa por mérito da retratação”. (`app/chamados/services/validacao.py:42`; `app/chamados/services/explicador.py:90`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1112`; `app/chamados/services/validacao.py:208`; `app/ponto/services/flip_auto.py:132`) |
| `batida_origem` | Derivada no nascimento a partir de `contexto.ultima_batida.id`, com família e colaborador conferidos; `_nascer` aceita/propaga a origem e inserção preenche quando ausente. (`app/chamados/models.py:2696`; `app/chamados/models.py:2698`; `app/chamados/models.py:3177`; `app/chamados/models.py:3190`; `app/chamados/models.py:3192`; `app/chamados/models.py:3196`) | Chave do episódio em `do_fato`, índice e agrupamento; `_sem_episodio` distingue acervo. Contexto é mutável e a coluna preserva o episódio de nascimento. (`app/chamados/models.py:2574`; `app/chamados/models.py:2579`; `app/chamados/models.py:3218`; `app/chamados/services/fio.py:1083`; `app/chamados/services/fio.py:1012`; `app/chamados/models.py:3185`) |
| `Disputa.motivos/contexto` | Kernel abre/agrega motivos e funde contexto; métodos removem/respondem motivos, anotam/renovam contexto, reabrem; emissores manuais complementam motivos. (`app/chamados/services/disputa_emissao.py:541`; `app/chamados/services/disputa_emissao.py:548`; `app/chamados/services/disputa_emissao.py:562`; `app/chamados/services/disputa_emissao.py:589`; `app/chamados/models.py:1936`; `app/chamados/models.py:2033`; `app/chamados/models.py:2074`; `app/chamados/models.py:2092`; `app/chamados/models.py:2129`; `app/chamados/services/disputa_emissao.py:1267`) | Emissão usa motivo/data/contexto e última batida; materialização e juiz usam contexto como fallback de data; derivador de episódio lê última batida só no nascimento; auditor verifica coerência e catálogo. `motivos` é escopo agregado, não lista de perguntas vivas. (`app/chamados/services/disputa_emissao.py:345`; `app/chamados/services/disputa_emissao.py:372`; `app/chamados/services/disputa_emissao.py:387`; `app/chamados/services/materializacao.py:343`; `app/chamados/services/materializacao.py:472`; `app/chamados/juizes.py:370`; `app/chamados/models.py:3185`; `app/chamados/management/commands/auditar_disputas.py:88`; `app/colaboradores/services/drawer.py:75`) |

**Relógio fora da borda ainda existe no código atingido:** embora testes novos congelem, portas ainda leem `now` internamente e `abrir_conversa` usa `localdate`; lavrar lê `timezone.now` e `expira_em` usa `date.today`. A afirmação de inexistência global dessa reincidência não se sustenta; se o recorte for somente fixtures novas, o agente de selos verifica os decoradores. (`app/chamados/models.py:2654`; `app/chamados/models.py:2797`; `app/chamados/veredito_lavrado.py:228`; `app/chamados/veredito_lavrado.py:112`)

Não demonstrei segundo juiz **registrado** duplicado por esta volta; afirmar inexistência semântica geral permanece **[nao verificado]**.

## 07/11 18:18 · sessao 4c6d1511766f · MOTOR-LE-O-TURNO-INSTRUMENTO · construtor · origem=Code

Cura aplicada: toda chamada estática aos dois pareadores, em qualquer função do motor/modelo/serviço DiaPago, passa a contar fora da composição do juiz. Cada chamada publica `arquivo::funcao`, sítio e linha; o gap permanece separado ([instrumento](/home/ronald/wt-codex-motor-le-o-turno-instrumento-181155/app/core/medida_turno_do_par.py:405)). [nao verificado]

Medição anterior à edição: exatamente estas duas portas:

| Arquivo:linha | Função | Destino |
|---|---|---|
| `ponto/motor_calculo_v2.py:503` | `turnos_via_autoridade` | `parear_turnos` |
| `ponto/motor_calculo_v2.py:882` | `MotorBase._turnos_do_juiz` | `turnos_de_batidas` |

DiaPago está definido em `ponto/models.py:281`. A lavratura importa o modelo em `ponto/services/dia_pago.py:163` e monta o dia por `_dia_de` em `:174`; a grade é importada em `:443` e usada em `:464`.

Resultados executados em memória:

- Original: **35 testes, rc=0**; contadores **9/85**, registros e linhas preservados (`core/tests/test_medida_turno_do_par.py:553`, `:569`).
- Antes da cura: M1/M6 produziram falhas de asserção. Após a cura, sobre fontes reais: **M1 e M6 → 10/86; M7 → 9/85** (`core/tests/test_medida_turno_do_par.py:739`, `:748`, `:760`).
- Os **14 mutantes saíram rc=1 por asserção**. Corpo do runner: `core/tests/test_medida_turno_do_par.py:19`; comando individual de cada execução:

| Mutante | rc | Comando em arquivo:linha |
|---|---:|---|
| composicao | 1 | `core/tests/test_medida_turno_do_par.py:73` |
| linhas | 1 | `core/tests/test_medida_turno_do_par.py:74` |
| gap | 1 | `core/tests/test_medida_turno_do_par.py:75` |
| classificar | 1 | `core/tests/test_medida_turno_do_par.py:76` |
| P1-linha-unica | 1 | `core/tests/test_medida_turno_do_par.py:77` |
| P2-len1 | 1 | `core/tests/test_medida_turno_do_par.py:78` |
| Q1-lines10 | 1 | `core/tests/test_medida_turno_do_par.py:79` |
| Q2-len2 | 1 | `core/tests/test_medida_turno_do_par.py:80` |
| Q3-tuple | 1 | `core/tests/test_medida_turno_do_par.py:81` |
| Q4-self | 1 | `core/tests/test_medida_turno_do_par.py:82` |
| R1-tres-funcoes | 1 | `core/tests/test_medida_turno_do_par.py:83` |
| R2-portas-fixas | 1 | `core/tests/test_medida_turno_do_par.py:84` |
| R3-so-motor | 1 | `core/tests/test_medida_turno_do_par.py:85` |
| R4-conta-composicao | 1 | `core/tests/test_medida_turno_do_par.py:86` |

**Campos novos: nenhum.** `censo_fontes` escreve `chave`, `criterio`, `sitio` e `linhas` em `core/medida_turno_do_par.py:412`. A catraca lê identidade/sítio/critério em `core/tests/test_medida_turno_do_par.py:563`, linhas em `:571`, e os testes MORDE conferem a publicação em `:781`.

## 07/11 18:13 · sessao 6364bb5ec91d · MOTOR-LE-O-TURNO-INSTRUMENTO · censo · origem=Code

As referências aos anexos contam linhas desde o início de cada bloco fornecido. A confirmação dos commits posteriores nesta cópia-base é [nao verificado]. [nao verificado]

1. **O1 — portas novas fora das funções fixas**

   **Pergunta:** “O instrumento cumpre o criterio LITERAL? Algum item conta algo fora de A1/B1/nomeados, ou deixa de contar?”; “Cite algum mutante do instrumento que passaria em todos os testes.” (`v1.q:7`, `v1.q:10`) [nao verificado]

   **Resposta:** diagnóstico das novas chamadas M1/M6: **ausente**. O trecho correlato trata outra forma de pareador novo: “Um `range(..., 2)` novo sobe contador e dispara RED.” (`v1.r:34`; `triagem_opus1.txt:2`)

   **Triagem:** descarte específico de M1/M6: **ausente**. A avaliação geral foi: “1 NAO PROCEDE no criterio (9/85, 9/89 reproduzidos); a garantia incompleta e os itens 2 e 4.” (`triagem_v1.txt:2`)

   **Classe (a):** omissão de contagem e mordida estavam perguntadas; o Codex examinou o incremento por `range` novo e deixou passar o incremento por chamada nova à autoridade. (`v1.q:7`, `v1.q:10`; `v1.r:34`; `triagem_opus1.txt:2`)

2. **O2 — leitor host e publicação no ESTADO ausentes**

   **Pergunta:** cobrança explícita dos itens contratuais “leitor `bin/*_estado.py` + linha no ESTADO”: **ausente**. Houve a pergunta correlata: “QUEM ESCREVE e QUEM LE cada valor que o diff grava ou le”. (`v1.q:15`; `triagem_opus1.txt:3`)

   **Resposta:** diagnóstico da entrega contratual faltante: **ausente**. O Codex registrou apenas: “Publicação efetiva desses valores em placar/HOST fora da suite: **[nao verificado]**.” (`v1.r:104`) [nao verificado]

   **Triagem:** descarte explícito da obrigação contratual: **ausente**. Sobre o inventário de leitores: “8 tabela recebida, sem achado.” (`triagem_v1.txt:9`)

   **Classe (d):** a obrigação específica de entregar a publicação host não foi perguntada nem diagnosticada; “publicação não verificada” não constitui esse achado. A restrição do construtor a `app/` não impedia a leitura pelo verificador. (`v1.q:15`; `v1.r:104`; `triagem_opus1.txt:3`; `CLAUDE.md:341`, `CLAUDE.md:351`)

3. **FILA — E2 troca quatro identidades B1 no rebase, conservando o total**

   **Pergunta:** sobre essa troca concreta: **ausente**. A pergunta semelhante era sobre outro registro: “COMPOSICAO_REGISTRADA congela a lista inteira? Uma troca de identidade com o mesmo tamanho e cobrada?” (`v2.q:6`)

   **Resposta:** identificação dos quatro B1 trocados: **ausente**. A proteção geral de identidade foi reconhecida: “A igualdade percorre todos os itens dos dois contadores: A1, B1 e os três nomeados, com identidade completa e listas inteiras.” (`v3.r:154`)

   **Triagem:** descarte anterior desse evento: **ausente**. O enquadramento final diz: “ao rebasear, 4 itens B1 trocam de identidade com total igual -- a E2 classifica, nao copia”. (`triagem_opus1.txt:4`)

   **Classe (c), quanto ao evento concreto:** faltam as quatro identidades e as árvores comparadas; a classe geral de troca de identidade já estava reconhecida, portanto não cabe afirmar modo-comum sobre ela. (`v3.r:154`; `triagem_opus1.txt:4`; identidades concretas: [nao verificado])

4. **FILA — LINHAS_REGISTRADAS absolutas**

   **Pergunta:** “Linha mudada por edicao NAO relacionada do motor reprova (custo esperado) e a mensagem diz o que fazer?” (`v3.q:7`)

   **Resposta:** “**PROCEDENTE o custo de deslocamento:** uma edição sem relação com o negócio reprova se deslocar ocorrências registradas.” (`v3.r:156`)

   **Triagem:** “custo de deslocamento e o esperado e a mensagem diz o que fazer.” O opus manteve: “LINHAS_REGISTRADAS absolutas (custo de vermelho falso, nunca verde falso)”. (`triagem_v3.txt:7`; `triagem_opus1.txt:4`) [nao verificado]

   **Classe (b):** o Codex viu o custo e a triagem o aceitou expressamente como esperado, sem tratá-lo como bloqueador. (`v3.r:156`; `triagem_v3.txt:7`)

5. **FILA — M2, M4 e M5 declarados no NAO_VE**

   **Pergunta:** referência individual a M2, M4 ou M5: **ausente**. O tema correlato estava presente: “Distinga o que o NAO_VE declara do que some calado.” (`v3.q:6`)

   **Resposta:** identificação individual desses mutantes: **ausente**. Sobre os limites examinados, o Codex escreveu: “**Não enumera especificamente `for-target`, `with-as` e chaves de `dict` como buracos do gap.**” (`v3.r:152`)

   **Triagem:** “Q5 PROCEDENTE (b)+(d) TEXTO”; a mesma linha registra que a perda da parcela continua cobrada pela catraca numérica e por `LINHAS_REGISTRADAS`. A vinculação disso aos nomes M2/M4/M5 não aparece; o opus fornece somente “M2/M4/M5 declarados no NAO_VE”. (`triagem_v3.txt:6`; `triagem_opus1.txt:4`)

   **Classes individuais — M2: [nao verificado]; M4: [nao verificado]; M5: [nao verificado].** Sem os enunciados dos três mutantes, não há base para escolher entre descarte de achado já visto, classe (b), e limitação fora do alcance, classe (c), nem para associá-los aos exemplos de v3(b). (`triagem_opus1.txt:4`; `v3.r:152`)

_2794 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
