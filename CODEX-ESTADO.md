# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 08:27** · publicado em **30/10/2026 08:30** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 08:27 · sessao d5dea9825fc6 · O261-VETO-NA-PORTA · leitura · origem=Code

A cópia **ainda não reúne nascimento e reabertura numa única função**: `abrir` insere por `get_or_create`, `criar` por `create`, e as transições gravam por `_gravar_estado`. O veto compartilhado dos nascimentos é celular, limitado a `MODULOS_DE_TURNO`; não consulta o veto de régua pedido. (`app/chamados/models.py:730`, `app/chamados/models.py:834`, `app/chamados/models.py:231`, `app/chamados/models.py:839`, `app/chamados/models.py:849`)

Abaixo, “desenho” identifica proposta, sem implementação; comportamento futuro permanece **[nao verificado]**.

**1. Contrato de entrada do juiz único**

**Fonte.** As funções existentes precisam representar uma decisão só, distinguindo consulta e execução de consequências:

| Função existente | Responsabilidade atual |
|---|---|
| `motivo_do_veto(colab, ec, data)` | Leitura: degeneração primeiro, suspeita depois; retorna `VETO_DEGENERADA`, `VETO_REGUA` ou `None`. (`app/ponto/services/furos_vetados.py:19`, `app/ponto/services/furos_vetados.py:25`) |
| `regua_degenerada(tipo_escala, data)` | Predicado estrutural: template ausente, exceção ao obter marcos, início/fim ausentes ou iguais. Pergunta ao template, não aos marcos congelados da célula. (`app/escala/regua_defesa.py:16`, `app/escala/regua_defesa.py:23`, `app/escala/regua_defesa.py:26`, `app/escala/regua_defesa.py:31`) |
| `vetar_cobranca_se_degenerada(colab, ec, data)` | Usa aquele predicado, mas também abre/deduplica chamado de cadastro `regua_degenerada`. (`app/escala/regua_defesa.py:45`, `app/escala/regua_defesa.py:51`, `app/escala/regua_defesa.py:80`) |
| `vetar_cobranca_se_regua_suspeita(colab, ec)` | Sem vínculo ou avaliação, não veta; caso contrário delega a `deve_vetar_regua`. (`app/escala/regua_defesa.py:554`, `app/escala/regua_defesa.py:560`) |
| `deve_vetar_regua(ec)` | Exige faixa suspeita e pelo menos duas avaliações baixas. Corroboração administrativa alimenta esse mesmo predicado com confiança zero e dois dias baixos. (`app/escala/regua_defesa.py:641`, `app/escala/regua_defesa.py:644`, `app/escala/regua_defesa.py:662`, `app/escala/regua_defesa.py:666`) |

**Unidade:** colaborador + dia do fato + vínculo responsável por aquele dia. O resolvedor existente é `vinculo_do_emissor → escala_vigente`; a célula/geradora do dia precede a linha do tempo. Isso difere da chave de dedup dos dois emissores, que é colaborador + módulo + `data_turno`, sem vínculo. (`app/ponto/services/furos_vetados.py:10`, `app/ponto/services/furos_vetados.py:15`, `app/escala/servico_jornada.py:29`, `app/escala/servico_jornada.py:55`, `app/ponto/management/commands/detectar_ausencias.py:688`, `app/ponto/management/commands/reconciliar_grade.py:499`)

Para a porta, o dia já tem autoridade: `dia_do_fato(..., fortes=True)` lê célula, chave do catálogo e `data_turno`; sem esses elementos, retorna `None`. Não autoriza substituir dia desconhecido por hoje ou por criação do chamado. (`app/chamados/juizes.py:78`, `app/chamados/juizes.py:89`, `app/chamados/juizes.py:104`, `app/chamados/juizes.py:118`)

**Universo: há três conjuntos diferentes.**

| Constante | Módulos |
|---|---|
| `FURO_MODULES` | `batida_ausente`, `turno_aberto`, `intervalo_pendente`, `entrada_bloqueada_turno_aberto`. (`app/chamados/catalogo/furo_modulos.py:15`) |
| `MODULOS_DE_TURNO` | `turno_aberto`, `turno_aberto_24h`, `entrada_bloqueada_turno_aberto`. (`app/chamados/reconciliador.py:336`) |
| `CALADOS_POR_ISENCAO` | Inclui `sem_batidas_mes`; não inclui `entrada_bloqueada_turno_aberto`. Não é sinônimo dos anteriores. (`app/chamados/catalogo/modulos.py:235`) |

**Desenho:** para alcançar os emissores de furo/turno enumerados no censo, declararia o universo como **união de `FURO_MODULES` e `MODULOS_DE_TURNO`**, cinco módulos. Usar apenas `MODULOS_DE_TURNO` deixa detector/grade de `batida_ausente` descobertos; usar apenas `FURO_MODULES` deixa `turno_aberto_24h` descoberto. Essa união é proposta, não constante canônica já existente. (`app/chamados/catalogo/furo_modulos.py:15`, `app/chamados/reconciliador.py:336`, `app/ponto/management/commands/reconciliar_grade.py:500`, `app/ponto/management/commands/processar_alertas_avancados.py:121`) **[nao verificado — desenho]**

**Exclusões do contrato proposto:** containers `disputa_supervisao*`, avisos de cadastro — inclusive o próprio `regua_degenerada` — e demais módulos fora desse universo. O cadastro precisa atravessar a porta sem sofrer o veto cuja consequência ele registra; o container não deve herdar o veto de um dos dias das perguntas. (`app/chamados/catalogo/modulos.py:133`, `app/chamados/catalogo/modulos.py:144`, `app/chamados/catalogo/modulos.py:146`, `app/escala/regua_defesa.py:53`) **[nao verificado — desenho]**

O predicado de régua não contém filtros de módulo, isenção, situação, afastamento, feriado, posto ou célula. A exclusão de desligado está no leitor `furos_vetados_por_regua`, que mantém afastados deliberadamente; não pertence a `motivo_do_veto`. (`app/ponto/services/furos_vetados.py:19`, `app/ponto/services/furos_vetados.py:25`, `app/ponto/services/furos_vetados.py:39`, `app/ponto/services/furos_vetados.py:49`)

**Diferenças atuais, sem eleger um caminho como correto:**

| Diferença | `motivo_do_veto` | Detector `_abrir_chamado` | Grade `_emitir_furo` |
|---|---|---|---|
| **`ec=None`** | Retorna `None` imediatamente. (`app/ponto/services/furos_vetados.py:26`) | Trata como template ausente: abre cadastro e retorna `VETO_DEGENERADA`. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/escala/regua_defesa.py:23`, `app/escala/regua_defesa.py:45`, `app/escala/regua_defesa.py:51`) | Igual ao detector; a assinatura inclusive aceita `ec=None`. (`app/ponto/management/commands/reconciliar_grade.py:480`, `app/ponto/management/commands/reconciliar_grade.py:487`) |
| **Degeneração com vínculo** | Consulta `regua_degenerada`, sem escrita. (`app/ponto/services/furos_vetados.py:28`) | Consulta a versão com efeito de cadastro; mesmo predicado estrutural. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/escala/regua_defesa.py:46`) | Igual ao detector. (`app/ponto/management/commands/reconciliar_grade.py:487`) |
| **Cadastro decorrente** | Não abre. (`app/ponto/services/furos_vetados.py:23`) | Abre/deduplica por colaborador + template, não por dia; guarda `data_exemplo`. (`app/escala/regua_defesa.py:51`, `app/escala/regua_defesa.py:54`, `app/escala/regua_defesa.py:76`) | Mesmo efeito. (`app/ponto/management/commands/reconciliar_grade.py:487`) |
| **Suspeita e precedência** | Depois da degeneração, consulta `vetar_cobranca_se_regua_suspeita`. (`app/ponto/services/furos_vetados.py:28`, `app/ponto/services/furos_vetados.py:30`) | Mesma função e precedência. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/detectar_ausencias.py:604`) | Mesma função e precedência. (`app/ponto/management/commands/reconciliar_grade.py:487`, `app/ponto/management/commands/reconciliar_grade.py:491`) |
| **Feriado do dia** | Não consulta. (`app/ponto/services/furos_vetados.py:25`) | Faixa `feriado` de `fatos_do_dia` retorna `VETO_FERIADO`. (`app/ponto/management/commands/detectar_ausencias.py:615`) | Não há essa consulta dentro de `_emitir_furo`. (`app/ponto/management/commands/reconciliar_grade.py:480`) |
| **Posto fechado no feriado** | Não consulta. (`app/ponto/services/furos_vetados.py:25`) | `not cobra_no_dia` retorna `VETO_FERIADO_DO_POSTO`. (`app/ponto/management/commands/detectar_ausencias.py:622`) | Não há essa consulta dentro de `_emitir_furo`. (`app/ponto/management/commands/reconciliar_grade.py:480`) |
| **Veto celular** | Não consulta. (`app/ponto/services/furos_vetados.py:25`) | Consulta `celula_veta_cobranca`; retorna `VETO_CELULA_<motivo>`. (`app/ponto/management/commands/detectar_ausencias.py:625`) | Mesma consulta e retorno. (`app/ponto/management/commands/reconciliar_grade.py:494`) |
| **Célula ausente** | Não consulta existência. (`app/ponto/services/furos_vetados.py:25`) | Fora de limbo, abre aviso de dia sem grade e retorna `VETO_SEM_CELULA`; limbo atravessa essa guarda. (`app/ponto/management/commands/detectar_ausencias.py:651`, `app/ponto/management/commands/detectar_ausencias.py:654`, `app/ponto/management/commands/detectar_ausencias.py:662`) | `celula_veta_cobranca` aceita célula ausente; não há exigência adicional no método. (`app/ponto/marcos.py:156`, `app/ponto/management/commands/reconciliar_grade.py:494`, `app/ponto/management/commands/reconciliar_grade.py:499`) |
| **Encerrado que não renasceu** | Não trata estado de chamado. (`app/ponto/services/furos_vetados.py:19`) | Retorna `VETO_ENCERRADO_NAO_RENASCEU`, antes de push/fio. (`app/ponto/management/commands/detectar_ausencias.py:729`) | Tenta `reabrir()` novamente, ignora seu resultado e retorna `None`. (`app/ponto/management/commands/reconciliar_grade.py:519`, `app/ponto/management/commands/reconciliar_grade.py:521`) |
| **Limbo após nascimento** | Não trata. (`app/ponto/services/furos_vetados.py:19`) | Pode agrupar sob pai limbo ou registrar ressalva, conforme `pode_calar`. (`app/ponto/management/commands/detectar_ausencias.py:737`, `app/ponto/management/commands/detectar_ausencias.py:748`, `app/ponto/management/commands/detectar_ausencias.py:751`, `app/ponto/management/commands/detectar_ausencias.py:755`) | Não tem esse tratamento no método. (`app/ponto/management/commands/reconciliar_grade.py:499`, `app/ponto/management/commands/reconciliar_grade.py:521`) |

O veto celular também não é sinônimo da guarda compartilhada dos nascimentos: `celula_veta_cobranca` testa `trabalha is False` ou cinco vereditos explícitos; `_celula_que_absolve` alcança somente três módulos com célula vinculada e delega a veredito não vazio fora de `VEREDITOS_ACUSAM`, sem marcos pendentes. (`app/ponto/marcos.py:119`, `app/ponto/marcos.py:159`, `app/ponto/marcos.py:161`, `app/chamados/models.py:849`, `app/chamados/reconciliador.py:975`, `app/chamados/reconciliador.py:977`)

As guardas anteriores do `handle` da grade continuam relevantes: cobertura, isenção e `classificar_falta(...)[1] is None` podem impedir chegar a `_emitir_furo`. Portanto, ausência de teste de feriado dentro do método não prova emissão pelo comando completo. (`app/ponto/management/commands/reconciliar_grade.py:216`, `app/ponto/management/commands/reconciliar_grade.py:230`, `app/ponto/management/commands/reconciliar_grade.py:243`, `app/ponto/management/commands/reconciliar_grade.py:254`)

**Pendência de contrato:** o resultado único para vínculo ausente não está resolvido pelos comportamentos existentes. Substituir os dois emissores por `motivo_do_veto` escolheria silenciosamente “não veta” e retiraria a consequência de cadastro. A Lei fornecida resolve automático versus humano, mas não arbitra essa divergência. (`app/ponto/services/furos_vetados.py:26`, `app/escala/regua_defesa.py:45`, `app/escala/regua_defesa.py:51`) **[nao verificado — decisão para `ec=None`]**

**2. Porta única na aggregate root**

**Não existe função atual pronta para esse papel.** O núcleo que eu promoveria a escritor único de nascimento **e** mudança de vitalidade é `ChamadoColaborador._gravar_estado`: hoje ele já concentra a atribuição, a trilha de estado e o `save`, mas não recebe os nascimentos. Apenas acrescentar veto nele deixaria `abrir/criar` descobertos. (`app/chamados/models.py:185`, `app/chamados/models.py:192`, `app/chamados/models.py:227`, `app/chamados/models.py:231`, `app/chamados/models.py:730`, `app/chamados/models.py:834`) **[nao verificado — desenho]**

Nesse desenho:

- `abrir` conserva dedup/trava; `criar` conserva sua semântica sem dedup; ambos entregam o nascimento ao núcleo antes de inserir. A trava por colaborador já existente em `abrir` deve continuar envolvendo decisão e escrita. (`app/chamados/models.py:599`, `app/chamados/models.py:666`, `app/chamados/models.py:669`, `app/chamados/models.py:805`) **[nao verificado — desenho]**
- `reabrir`, `_renascer_por_premissa_viva` e `transicionar` convergem ao mesmo núcleo; o veto atua em nascimento ou passagem de não vivo para `VIVOS`, antes da mutação. Dedup de vivo e costura em vivo não são nova admissão. (`app/chamados/models.py:183`, `app/chamados/models.py:260`, `app/chamados/models.py:685`, `app/chamados/models.py:770`, `app/chamados/models.py:1089`, `app/chamados/reconciliador.py:805`, `app/chamados/catalogo/motor.py:24`) **[nao verificado — desenho]**
- O núcleo orquestra as funções existentes do veto; não ganha um terceiro predicado. A leitura pura serve aos leitores; a consequência de cadastro fica na execução da decisão, sem transformar consulta informativa em emissão. (`app/ponto/services/furos_vetados.py:23`, `app/ponto/services/furos_vetados.py:25`, `app/escala/regua_defesa.py:46`, `app/escala/regua_defesa.py:51`) **[nao verificado — desenho]**

**Como distingue automático de humano hoje:**

| Porta | Informação disponível hoje |
|---|---|
| `abrir` | Não tem parâmetro próprio de ato, autor ou via. `**lookup` é filtro ORM. (`app/chamados/models.py:599`, `app/chamados/models.py:670`) |
| `criar` | Não tem discriminador próprio; usa `**campos`. O comentário “user-initiated” não garante isso: alerta automático também chama `criar`. (`app/chamados/models.py:805`, `app/ponto/management/commands/processar_alertas_avancados.py:121`) |
| `_renascer_por_premissa_viva` | Recebe somente marcos; delega a `reabrir`. (`app/chamados/models.py:742`, `app/chamados/models.py:770`) |
| `reabrir` | Recebe somente `marcos`, embora documente admin/sistema. (`app/chamados/models.py:1023`) |
| `transicionar` | Recebe `user`, calcula perfil e usa evento `TRANSICAO_HUMANA`. (`app/chamados/models.py:122`, `app/chamados/models.py:125`, `app/chamados/models.py:260`) |
| `_gravar_estado` | Recebe `via`, `user`, `set_user`; a reabertura atual chama sem esses dados. Ao voltar a vivo, limpa `via_encerramento`. (`app/chamados/models.py:185`, `app/chamados/models.py:212`, `app/chamados/models.py:1089`) |

**Desenho:** ato administrativo explícito, autor e motivo devem atravessar toda a cadeia. Usuário presente, HTTP, nome `criar`, execução de command ou `forcar=True` não bastam para identificar override humano. O cartório já distingue `toque` de `forcar`, e há usuários de sistema nos serviços. (`app/ponto/services/cartorio.py:1312`, `app/ponto/services/cartorio.py:1316`, `app/chamados/services/competencia_trancada.py:239`) **[nao verificado — desenho]**

A trilha pode recuperar usuário da request implicitamente, mas isso é atribuição de auditoria, não classificação do ato: a batida do colaborador também ocorre em request. (`app/core/observ.py:100`, `app/core/observ.py:110`, `app/ponto/views.py:275`)

**Trilha existente reutilizável:** `_trilha_da_recusa` grava `nascimento_recusado` sobre `CelulaDia`, deduplicado por célula + módulo + veredito. Hoje exige `(dia, célula)` e não aceita diretamente motivo de régua, vínculo ausente ou operação de reabertura. (`app/chamados/models.py:862`, `app/chamados/models.py:866`, `app/chamados/models.py:870`, `app/chamados/models.py:873`)

O desenho precisa generalizar esse suporte: recusa automática registra operação/unidade/motivo; ato humano admitido registra autor, motivo do ato e veto apresentado, sem descrevê-lo como nascimento recusado. A resposta da porta precisa carregar o veto até a tela. A infraestrutura atual de `evento` é best-effort; garantia obrigatória de persistência da trilha ainda não existe ali. (`app/chamados/models.py:862`, `app/core/observ.py:94`, `app/core/observ.py:98`) **[nao verificado — desenho]**

As recusas já existentes de reabertura — fechamento administrativo sem fato novo, premissa regularizada, agrupamento, irmão vivo e motor — são outras guardas. Não devem ser confundidas nem removidas silenciosamente pela centralização do veto de régua. (`app/chamados/models.py:1003`, `app/chamados/models.py:1009`, `app/chamados/models.py:1012`, `app/chamados/models.py:1015`, `app/chamados/models.py:1019`)

**3. Destino de cada caminho do censo**

Nesta classificação, **A** = automático: nascimento/reabertura de módulo do universo sofre veto na porta; **H** = ato administrativo explícito: atravessa esse veto, apresenta motivo e registra trilha; **F** = fora do universo. São destinos propostos, **[nao verificado — desenho]**.

| Caminho | Mudança proposta e evidência atual |
|---|---|
| `abrir` — nascimento | **A/H conforme ato declarado**; decisão antes do INSERT. (`app/chamados/models.py:726`, `app/chamados/models.py:730`) |
| `criar` — nascimento | **A/H conforme ato declarado**; decisão antes do INSERT, sem presumir manual pelo nome. (`app/chamados/models.py:805`, `app/chamados/models.py:834`) |
| `abrir` — dedup encerrado | **A/H**; precisa atingir a decisão comum. Hoje retorna desse ramo antes do veto celular de nascimento. (`app/chamados/models.py:685`, `app/chamados/models.py:688`, `app/chamados/models.py:726`) |
| `_renascer_por_premissa_viva` | **A** nos chamadores de renascimento do censo; veto antes de reabrir. (`app/chamados/models.py:742`, `app/chamados/models.py:770`) |
| `reabrir` | **A/H**; acrescentar propagação explícita do ato hoje ausente. (`app/chamados/models.py:1023`) |
| `transicionar` para vivo | **H** nos formulários de gestor/DP do censo; mesma decisão, com aviso e trilha. Hoje passa por `_aplicar`, sem passar por `reabrir`. (`app/chamados/models.py:183`, `app/chamados/models.py:260`) |
| Detector `_abrir_chamado` | **A**; deixa de decidir régua por fora — L-111. Demais guardas permanecem identificadas como outras perguntas. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/detectar_ausencias.py:604`, `app/ponto/management/commands/detectar_ausencias.py:688`) |
| `emitir_furo_retroativo` | **A**; herda a centralização do detector. (`app/ponto/management/commands/emitir_furo_retroativo.py:38`, `app/ponto/management/commands/emitir_furo_retroativo.py:142`) |
| Grade `_emitir_furo` | **A**; retira decisões externas de régua; a tentativa adicional de `reabrir` também fica coberta. (`app/ponto/management/commands/reconciliar_grade.py:487`, `app/ponto/management/commands/reconciliar_grade.py:491`, `app/ponto/management/commands/reconciliar_grade.py:499`, `app/ponto/management/commands/reconciliar_grade.py:520`) |
| Cartório — integral, cobrança morta e limbo de hoje | **A**, salvo ato admin explicitamente propagado; emissão delega ao detector. A leitura de veto para impressão continua sendo leitura. (`app/ponto/services/cartorio.py:1083`, `app/ponto/services/cartorio.py:1358`) |
| Cartório — furo parcial | Mesmo destino; delega ao detector. (`app/ponto/services/cartorio.py:1394`) |
| C2 `reemitir_chamado_do_dia` | **A**; elimina o retorno antecipado por régua — L-111. Costura em chamado já vivo continua fora do evento de nascimento/reabertura. (`app/chamados/reconciliador.py:805`, `app/chamados/reconciliador.py:852`, `app/chamados/reconciliador.py:855`, `app/chamados/reconciliador.py:862`) |
| Alerta crítico de turno | **A**; passa a encontrar o veto na porta. (`app/ponto/management/commands/processar_alertas_turno.py:316`, `app/ponto/management/commands/processar_alertas_turno.py:335`) |
| Alerta avançado `turno_aberto_24h` | **A**, embora use `criar`; coberto pela união de módulos proposta. (`app/ponto/management/commands/processar_alertas_avancados.py:121`) |
| Retroativo `turno_aberto_24h` | **A**; dry pode consultar informativamente, escrita decide na porta. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:134`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:146`) |
| `bater_ponto` → entrada bloqueada | **A**: reação automática à batida do colaborador, não override admin. Falta resolver a unidade: contexto contém resumo de vários pendentes e total, sem dia estruturado. (`app/ponto/views.py:275`, `app/ponto/views.py:280`, `app/ponto/views.py:291`, `app/ponto/views.py:301`) |
| View de abertura → `abrir_chamado_local` | Abertura humana existe, mas override **admin** não está demonstrado: serviço não 

[truncado em 20000 de 38206 caracteres]

## 30/10 08:24 · sessao d13ad87f9021 · - · - · origem=Ronald (tmux)

Censo confirmado na cópia; classificação abaixo é **desenho proposto**, baseada no chamador e na Lei fornecida, não comportamento já implementado. A distinção de canal ainda não chega a `abrir`, `criar`, `reabrir` ou `_renascer_por_premissa_viva`: assinaturas não recebem ato/autor; `transicionar` recebe `user`. (`app/chamados/models.py:599`, `app/chamados/models.py:805`, `app/chamados/models.py:1023`, `app/chamados/models.py:742`, `app/chamados/models.py:122`)

| Caminho do censo | Efeito pelo desenho |
|---|---|
| `abrir` nascimento | Porta deve barrar emissão automática vetada antes de `get_or_create`; hoje há escrita em `models.py:730`. (`app/chamados/models.py:726`, `app/chamados/models.py:730`) |
| `criar` nascimento | Mesma política; nome `criar` não prova ato humano, pois alerta avançado também chama essa função. (`app/chamados/models.py:805`, `app/chamados/models.py:834`, `app/ponto/management/commands/processar_alertas_avancados.py:121`) |
| `abrir` dedup encerrado | Deve passar pela mesma decisão de renascimento automático; ramo retorna antes do veto celular atual. (`app/chamados/models.py:685`, `app/chamados/models.py:686`, `app/chamados/models.py:688`, `app/chamados/models.py:726`) |
| `_renascer_por_premissa_viva` | Automático; aplicar veto na porta compartilhada antes da reabertura. (`app/chamados/models.py:742`, `app/chamados/models.py:770`) |
| `reabrir` | Mistura admin/sistema hoje; precisa canal explícito, pois a assinatura só tem `marcos`. (`app/chamados/models.py:1023`, `app/chamados/models.py:1024`) |
| `transicionar` para vivo | Precisa mesma política com canal humano comprovado; hoje segue direto `_aplicar → _gravar_estado`, sem guardas de `reabrir`. (`app/chamados/models.py:122`, `app/chamados/models.py:183`, `app/chamados/models.py:260`) |
| Detector `_abrir_chamado` | Automático: passa a barrar na porta, deixa de executar veto de régua por fora; preservar demais filtros de emissão. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/detectar_ausencias.py:604`, `app/ponto/management/commands/detectar_ausencias.py:688`) |
| `emitir_furo_retroativo` | Herda mudança do detector; executar command não caracteriza override administrativo por chamado. (`app/ponto/management/commands/emitir_furo_retroativo.py:38`, `app/ponto/management/commands/emitir_furo_retroativo.py:142`) |
| Grade `_emitir_furo` | Automático: retirar perguntas de régua externas e cobrir tanto `abrir` quanto `ch.reabrir` posterior. (`app/ponto/management/commands/reconciliar_grade.py:487`, `app/ponto/management/commands/reconciliar_grade.py:491`, `app/ponto/management/commands/reconciliar_grade.py:499`, `app/ponto/management/commands/reconciliar_grade.py:520`) |
| Cartório integral/cobrança morta/limbo | Emissão delegada automática passa à política da porta; consulta para impressão pode continuar como leitura, sem decidir recusa por fora. (`app/ponto/services/cartorio.py:1083`, `app/ponto/services/cartorio.py:1358`) |
| Cartório parcial | Idem; mesma chamada do detector. (`app/ponto/services/cartorio.py:1394`) |
| C2 `reemitir_chamado_do_dia` | Retirar retorno antecipado pelo veto; nascimento/renascimento automático fica na porta. Costurar num vivo não é nascimento. (`app/chamados/reconciliador.py:850`, `app/chamados/reconciliador.py:852`, `app/chamados/reconciliador.py:854`, `app/chamados/reconciliador.py:855`, `app/chamados/reconciliador.py:862`) |
| Alerta crítico de turno | Automático, passa a ser barrado na porta. (`app/ponto/management/commands/processar_alertas_turno.py:316`, `app/ponto/management/commands/processar_alertas_turno.py:335`) |
| Alerta avançado 24h | Automático apesar de usar `criar`; só coberto se 24h integrar universo decidido. (`app/ponto/management/commands/processar_alertas_avancados.py:121`, `app/chamados/catalogo/furo_modulos.py:15`, `app/chamados/catalogo/modulos.py:109`) |
| Retroativo turno 24h | Mesma ressalva de universo; consulta dry pode continuar informativa, decisão real na porta. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:134`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:146`) |
| `bater_ponto` entrada bloqueada | **Emissão automática em resposta à batida do colaborador**, não ato admin: barrar na porta se vetado. Contexto não identifica dia, apenas `pendentes`/`total`; contrato deve resolver isso. (`app/ponto/views.py:275`, `app/ponto/views.py:280`, `app/ponto/views.py:291`, `app/ponto/views.py:301`) |
| Abertura local genérica | **Não presumir admin**: serviço não recebe autor e view usa colaborador vinculado; aceita qualquer módulo em `MOTIVOS`, inclusive furo. Override admin aqui é [nao verificado]. (`app/chamados/services/abertura.py:19`, `app/chamados/views.py:170`, `app/chamados/views.py:174`, `app/chamados/views.py:192`, `app/chamados/views.py:197`) |
| Resposta tardia | Resposta de **colaborador**; renascimento é reação automática condicionada à célula, portanto não override admin. (`app/chamados/views.py:1041`, `app/chamados/services/resposta_tardia.py:34`, `app/chamados/services/resposta_tardia.py:45`) |
| Resposta livre, novo status | Ato gestor explícito: veto informa+trilha, passa; já leva `request.user` ao modelo. (`app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/acoes_chamado.py:315`) |
| Fio, novo status | Ato DP com autorização por chamado: informar+trilhar, passar; já leva `request.user`. (`app/chamados/services/fio.py:702`, `app/chamados/services/fio.py:708`, `app/chamados/services/fio.py:715`) |
| Desarquivar | Ato humano autorizado: informar+trilhar, passar. Hoje perde usuário em `ch.reabrir()`, embora usuário exista na trilha de arquivo/nota. (`app/chamados/views_arquivar.py:54`, `app/chamados/views_arquivar.py:60`, `app/chamados/views_arquivar.py:68`, `app/chamados/views_arquivar.py:75`, `app/chamados/views_arquivar.py:79`) |
| Reabrir competência | Wrapper recebe `por`+`motivo`; serviço recebe `autor`, mas perde-o no `ch.reabrir()`. Definir explicitamente se efeito em lote é ato humano delegado ou emissão automática; não inferir apenas por `autor` opcional, que cai em usuário sistema. (`app/ponto/services/fechamento.py:1400`, `app/ponto/services/fechamento.py:1416`, `app/chamados/services/competencia_trancada.py:228`, `app/chamados/services/competencia_trancada.py:239`, `app/chamados/services/competencia_trancada.py:258`) |
| Reabrir disputa admin | É ato admin explícito, exige motivo/justificativa; pai é usualmente container, fora universo; se chamado selecionado for turno, informar+trilhar e passar pelo canal admin. (`app/chamados/services/acoes_disputa.py:242`, `app/chamados/services/acoes_disputa.py:245`, `app/chamados/services/acoes_disputa.py:250`, `app/chamados/services/acoes_disputa.py:266`, `app/chamados/services/acoes_disputa.py:270`) |
| Resolver perguntas → pai | Reação automática; pais `disputa_supervisao*` ficam fora do veto de turno, não bloquear container por veto de um dia. (`app/chamados/services/disputa_emissao.py:1599`, `app/chamados/services/disputa_emissao.py:1606`, `app/chamados/catalogo/modulos.py:144`, `app/chamados/catalogo/modulos.py:146`) |
| Command órfãs enterradas | Reabertura sistema do pai; recusa antecipada é `recusa_da_reabertura`, não pergunta externa ao juiz da régua. Pai fora universo usual; se turno, canal automático. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:105`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:154`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`) |
| Migration 0038 reverso | Bypass histórico de escritor; é vivo→vivo (`registrado→aberto`), não nascimento/reabertura encerrado→vivo. Não mudar migration pelo desenho. (`app/chamados/migrations/0038_a3_registrado_backfill.py:15`, `app/chamados/catalogo/motor.py:24`) |
| Núcleo disputa / agrupador órfãos / âncora | Pais fora universo do veto; não transferir veto de furo ao pai. Se gerar c2 depois, somente nascimento/renascimento do filho é barrado. (`app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:507`, `app/chamados/services/disputa_emissao.py:908`, `app/chamados/services/disputa_emissao.py:911`, `app/chamados/services/disputa_emissao.py:1128`, `app/chamados/services/disputa_emissao.py:1131`, `app/chamados/services/disputa_emissao.py:409`) |
| Abertura DP para colaborador | Humano explícito com autor; catálogo atual não declara módulos estritos como abríveis por esse formulário, mas permite container manual. Portanto não inventar que já há abertura admin de turno por essa porta. (`app/chamados/services/abertura.py:122`, `app/chamados/services/abertura.py:149`, `app/chamados/services/abertura.py:172`, `app/chamados/services/abertura.py:188`, `app/chamados/catalogo/modulos.py:105`, `app/chamados/catalogo/modulos.py:146`) |

**Wrappers do censo:** herdam o caminho final; presença de HTTP, signal ou command não muda por si só canal para admin. Exceção comprovada é botão `Cobrar este dia`: serviço se define manual, recebe autor/request, chama cartório `toque=True`, grava trilha manual e hoje retorna recusa quando `out.vetados`. Pela Lei nova, precisa propagar canal admin até a porta; apenas mover veto para o modelo mantendo esse wrapper sem canal ainda barraria o clique humano. (`app/chamados/services/cobrar_dia.py:1`, `app/chamados/services/cobrar_dia.py:16`, `app/chamados/services/cobrar_dia.py:29`, `app/chamados/services/cobrar_dia.py:37`, `app/chamados/services/cobrar_dia.py:54`, `app/chamados/views_cobrar.py:325`)

- Cartório command, célula, vigia de hora, passivo, signals insumos/chamado, regeneração de vínculo, pós-vínculo e régua corrigida: mesma porta automática por detector; signal do chamado acontece após escrita. (`app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:1560`, `app/ponto/services/vigia_de_hora.py:130`, `app/ponto/services/furos_sem_cobranca.py:128`, `app/ponto/signals.py:93`, `app/chamados/signals.py:180`, `app/ponto/portas/celula.py:372`, `app/colaboradores/services/pos_vinculo.py:72`, `app/escala/regua_defesa.py:692`)
- Commands de rejulgamento e scripts: mesma decisão automática do cartório, sem override admin explícito por chamado. (`app/ponto/management/commands/celula_veredito_velho.py:144`, `app/ponto/management/commands/curar_residuo_bug115.py:143`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:71`, `bin/rejulgar_o118_col221.py:24`, `bin/r5_idempotencia_frota.py:109`)
- Contexto c2, reconciliação command/signal, fábrica no julgamento e disparo competência: nascimentos do filho por c2 seguem porta automática; pai continua fora universo. (`app/chamados/reconciliador.py:671`, `app/chamados/reconciliador.py:1518`, `app/chamados/services/disputa_emissao.py:409`, `app/ponto/services/cartorio.py:770`, `app/chamados/services/fabrica_da_celula.py:122`, `app/chamados/management/commands/disparar_perguntas_competencia.py:288`)
- API ambos kernels, triagem saída sem entrada e foto retroativa: primeiro container fora universo; emissão c2 posterior segue política automática. (`app/api/views.py:778`, `app/api/views_core.py:778`, `app/ponto/services/triagem_batida.py:651`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`, `app/chamados/services/disputa_emissao.py:598`, `app/chamados/services/disputa_emissao.py:606`)
- Declaração de horário: chamada do núcleo gera pai fora universo; não presumir override admin para todo efeito derivado da declaração. (`app/chamados/services/declaracao_texto.py:307`, `app/chamados/services/declaracao_texto.py:605`, `app/chamados/services/disputa_emissao.py:563`, `app/chamados/services/disputa_emissao.py:504`)
- Outros chamadores agrupador/núcleo: mesmo pai fora universo; nenhum motivo para repetir veto por dia no container. (`app/chamados/services/acoes_disputa.py:221`, `app/chamados/services/pergunta_no_app.py:159`, `app/chamados/services/cobrar_dia.py:44`, `app/ponto/management/commands/processar_alertas_turno.py:384`, `app/ponto/management/commands/processar_alertas_avancados.py:158`, `app/ponto/management/commands/detectar_ausencias.py:791`, `app/ponto/management/commands/reconciliar_grade.py:416`, `app/chamados/management/commands/smoke_chamados.py:99`)

Alerta de desenho: não usar `criar` como sinônimo de manual, `abrir` como sinônimo de automático ou usuário presente como sinônimo de override; os exemplos acima contradizem essas três heurísticas. (`app/ponto/management/commands/processar_alertas_avancados.py:121`, `app/chamados/services/abertura.py:47`, `app/chamados/services/competencia_trancada.py:239`)

## 30/10 08:23 · sessao cfff94ef1c2e · - · - · origem=Ronald (tmux)

Referências abaixo relativas à raiz da cópia; nenhuma consulta a banco/log foi feita e nenhum teste foi executado. [nao verificado] [nao verificado]

**Contrato e unidade**

- A leitura existente é `motivo_do_veto(colab, ec, data)`: retorna `VETO_DEGENERADA`, `VETO_REGUA` ou `None`; usa `regua_degenerada(ec.tipo_escala, data)` primeiro e `vetar_cobranca_se_regua_suspeita(colab, ec)` depois. (`app/ponto/services/furos_vetados.py:19`, `app/ponto/services/furos_vetados.py:25`, `app/ponto/services/furos_vetados.py:28`, `app/ponto/services/furos_vetados.py:30`)
- A versão emissora para degeneração é `vetar_cobranca_se_degenerada(colab, ec, data)`: usa o mesmo `regua_degenerada`, mas transforma vínculo ausente em template ausente e abre um chamado de cadastro `regua_degenerada`. (`app/escala/regua_defesa.py:36`, `app/escala/regua_defesa.py:45`, `app/escala/regua_defesa.py:51`, `app/escala/regua_defesa.py:53`)
- A unidade do juízo é **colaborador + dia + vínculo que responde por esse dia**; o módulo não entra nesses predicados. `vinculo_do_emissor` delega a `escala_vigente`, que carrega e pergunta ao `escala.alimentacao.vinculo_do_dia`; célula/geradora do dia tem precedência sobre a linha do tempo. (`app/ponto/services/furos_vetados.py:10`, `app/ponto/services/furos_vetados.py:15`, `app/escala/servico_jornada.py:15`, `app/escala/servico_jornada.py:29`, `app/escala/servico_jornada.py:38`, `app/escala/servico_jornada.py:55`)
- Não confundir unidade do juízo com dedup: detector e grade deduplicam **colaborador + módulo + data_turno**, sem vínculo na chave. (`app/ponto/management/commands/detectar_ausencias.py:688`, `app/ponto/management/commands/detectar_ausencias.py:691`, `app/ponto/management/commands/reconciliar_grade.py:499`, `app/ponto/management/commands/reconciliar_grade.py:501`)
- A constante estrita `FURO_MODULES` contém `batida_ausente`, `turno_aberto`, `intervalo_pendente`, `entrada_bloqueada_turno_aberto`; `MODULOS_DE_TURNO` contém `turno_aberto`, **`turno_aberto_24h`**, `entrada_bloqueada_turno_aberto`. São universos diferentes; o contrato da porta precisa explicitar se usa a união, evitando perder `24h` ou perder `batida_ausente`/`intervalo_pendente`. (`app/chamados/catalogo/furo_modulos.py:15`, `app/chamados/reconciliador.py:336`)
- `motivo_do_veto` não filtra módulo, situação, isenção, afastamento, célula, feriado nem posto: sua função contém somente as duas provas de régua. Esses filtros não podem ser atribuídos ao juiz de régua por inferência. (`app/ponto/services/furos_vetados.py:19`, `app/ponto/services/furos_vetados.py:25`)
- A exclusão de desligado existente em `furos_vetados_por_regua` é do leitor, não do predicado; o leitor mantém afastados deliberadamente. (`app/ponto/services/furos_vetados.py:39`, `app/ponto/services/furos_vetados.py:49`)

**Diferenças de predicado/comportamento nas funções pedidas**

| Diferença | `motivo_do_veto` hoje | Detector `_abrir_chamado` hoje | Grade `_emitir_furo` hoje |
|---|---|---|---|
| `ec=None` | Retorna `None` imediatamente. (`app/ponto/services/furos_vetados.py:26`) | Chama degenerada; `te=None` é degenerada, abre cadastro e retorna `VETO_DEGENERADA`. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/escala/regua_defesa.py:23`, `app/escala/regua_defesa.py:45`, `app/escala/regua_defesa.py:51`) | Mesmo comportamento do detector; `ec` inclusive tem default `None`. (`app/ponto/management/commands/reconciliar_grade.py:480`, `app/ponto/management/commands/reconciliar_grade.py:487`) |
| Degeneração com `ec` presente | Consulta pura `regua_degenerada`. (`app/ponto/services/furos_vetados.py:28`) | Consulta versão com efeito de cadastro; mesmo predicado estrutural. (`app/ponto/management/commands/detectar_ausencias.py:599`, `app/escala/regua_defesa.py:46`) | Igual detector. (`app/ponto/management/commands/reconciliar_grade.py:486`) |
| Efeito lateral | Não abre cadastro. (`app/ponto/services/furos_vetados.py:23`) | Degenerada abre/deduplica cadastro por colaborador + template, independente do dia; contexto guarda apenas `data_exemplo`. (`app/escala/regua_defesa.py:41`, `app/escala/regua_defesa.py:51`, `app/escala/regua_defesa.py:54`, `app/escala/regua_defesa.py:76`) | Igual detector. (`app/ponto/management/commands/reconciliar_grade.py:487`) |
| Régua suspeita | Mesmo `vetar_cobranca_se_regua_suspeita`, após degeneração. (`app/ponto/services/furos_vetados.py:30`) | Mesmo predicado e precedência. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/detectar_ausencias.py:604`) | Mesmo predicado e precedência. (`app/ponto/management/commands/reconciliar_grade.py:487`, `app/ponto/management/commands/reconciliar_grade.py:491`) |
| Feriado do dia | Não consulta. (`app/ponto/services/furos_vetados.py:25`) | `fatos_do_dia(...).faixa == 'feriado'` retorna `VETO_FERIADO`. (`app/ponto/management/commands/detectar_ausencias.py:615`) | Não consulta dentro de `_emitir_furo`. (`app/ponto/management/commands/reconciliar_grade.py:480`) |
| Fechamento do posto em feriado | Não consulta. (`app/ponto/services/furos_vetados.py:25`) | `not cobra_no_dia(...)` retorna `VETO_FERIADO_DO_POSTO`. (`app/ponto/management/commands/detectar_ausencias.py:622`) | Não consulta dentro de `_emitir_furo`. (`app/ponto/management/commands/reconciliar_grade.py:480`) |
| Veto da célula | Não consulta. (`app/ponto/services/furos_vetados.py:25`) | Consulta `celula_veta_cobranca`, retorna `VETO_CELULA_<motivo>`. (`app/ponto/management/commands/detectar_ausencias.py:625`) | Igual detector. (`app/ponto/management/commands/reconciliar_grade.py:494`) |
| Ausência de célula | Não consulta. (`app/ponto/services/furos_vetados.py:25`) | Sem célula e fora de limbo: abre aviso interno `_chamado_dia_sem_grade` e retorna `VETO_SEM_CELULA`. Limbo não cai nessa guarda. (`app/ponto/management/commands/detectar_ausencias.py:651`, `app/ponto/management/commands/detectar_ausencias.py:654`, `app/ponto/management/commands/detectar_ausencias.py:662`) | `celula_veta_cobranca` não veta célula ausente; não há guarda adicional. (`app/ponto/marcos.py:156`, `app/ponto/management/commands/reconciliar_grade.py:494`, `app/ponto/management/commands/reconciliar_grade.py:499`) |
| Recusa de renascimento | Não trata estado de chamado. (`app/ponto/services/furos_vetados.py:19`) | Após `abrir`, existente que continua fora de `VIVOS` retorna `VETO_ENCERRADO_NAO_RENASCEU`; não segue para push/fio. (`app/ponto/management/commands/detectar_ausencias.py:729`) | Após `abrir`, se `ENCERRADOS`, chama `reabrir()` outra vez, ignora resultado e retorna `None`. (`app/ponto/management/commands/reconciliar_grade.py:519`) |
| Limbo depois de nascer | Não trata. (`app/ponto/services/furos_vetados.py:19`) | Pode agrupar furo sob pai limbo e retornar `AGRUPADO_LIMBO_FOLGAS`; ou gravar ressalva conforme `pode_calar`. (`app/ponto/management/commands/detectar_ausencias.py:737`, `app/ponto/management/commands/detectar_ausencias.py:748`, `app/ponto/management/commands/detectar_ausencias.py:751`, `app/ponto/management/commands/detectar_ausencias.py:755`) | Não tem esse tratamento em `_emitir_furo`. (`app/ponto/management/commands/reconciliar_grade.py:499`, `app/ponto/management/commands/reconciliar_grade.py:521`) |

Detalhes dos predicados já existentes:

- `regua_degenerada` considera template ausente, exceção ao obter marcos, início/fim ausentes ou início igual ao fim. Não usa os marcos congelados da célula: pergunta diretamente `tipo_escala.marcos_do_dia(data)`. (`app/escala/regua_defesa.py:23`, `app/escala/regua_defesa.py:26`, `app/escala/regua_defesa.py:27`, `app/escala/regua_defesa.py:29`, `app/escala/regua_defesa.py:31`)
- Suspeita não equivale simplesmente a classe suspeita nem a cadastro vivo: vínculo ausente/nunca avaliado não veta; precisa faixa `<40` e `regua_dias_baixo >=2`. A função recebe `colab`, mas seu corpo decide só pelo vínculo. (`app/escala/regua_defesa.py:554`, `app/escala/regua_defesa.py:560`, `app/escala/regua_defesa.py:599`, `app/escala/regua_defesa.py:644`)
- Corroboração humana existente grava escore zero, dois dias baixos e avaliação corrente, ligando aquele mesmo predicado sem juiz adicional. (`app/escala/regua_defesa.py:662`, `app/escala/regua_defesa.py:666`)
- `celula_veta_cobranca` veta `trabalha is False` ou veredito em `nunca_bateu`, `trabalhou`, `justificado`, `coberta`, `cobranca_indevida`; sem célula não veta e `trabalha=None` não é folga. (`app/ponto/marcos.py:119`, `app/ponto/marcos.py:151`, `app/ponto/marcos.py:156`, `app/ponto/marcos.py:159`, `app/ponto/marcos.py:161`)

**Limites importantes para desenho L-111**

- Retirar consultas antecipadas de **régua** do detector/grade exige preservar o efeito de cadastro dentro da execução automática do juiz composto, ou declarar explicitamente sua supressão; trocar tudo simplesmente por `motivo_do_veto` altera `ec=None` e elimina o efeito de cadastro. (`app/ponto/services/furos_vetados.py:26`, `app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/reconciliar_grade.py:487`, `app/escala/regua_defesa.py:51`)
- Não há base no código para transportar automaticamente feriado, posto, célula ausente e limbo para o juiz de régua: são guardas diferentes, em funções/fontes diferentes. (`app/ponto/management/commands/detectar_ausencias.py:615`, `app/ponto/management/commands/detectar_ausencias.py:622`, `app/ponto/management/commands/detectar_ausencias.py:651`)
- A grade tem ainda guardas anteriores, no `handle`, de cobertura, isenção e `classificar_falta(...)[1] is None`; não pertencem ao corpo de `_emitir_furo`. A seleção do vínculo do dia também já acontece no chamador via `vinculo_do_dia`. (`app/ponto/management/commands/reconciliar_grade.py:196`, `app/ponto/management/commands/reconciliar_grade.py:216`, `app/ponto/management/commands/reconciliar_grade.py:230`, `app/ponto/management/commands/reconciliar_grade.py:243`, `app/ponto/management/commands/reconciliar_grade.py:254`)
- Os dois emissores são de `batida_ausente`; não recebem o veto compartilhado `_celula_que_absolve`, cujo filtro é só `MODULOS_DE_TURNO`. (`app/ponto/management/commands/reconciliar_grade.py:54`, `app/ponto/management/commands/detectar_ausencias.py:690`, `app/chamados/models.py:849`, `app/chamados/reconciliador.py:336`)
- A régua degenerada abre módulo de **cadastro**, fora tanto de `FURO_MODULES` quanto de `MODULOS_DE_TURNO`; o filtro de universo precisa excluí-lo para o veto não barrar sua própria consequência/reentrar. (`app/escala/regua_defesa.py:53`, `app/chamados/catalogo/furo_modulos.py:15`, `app/chamados/reconciliador.py:336`)

## 30/10 08:23 · sessao 5dacd1f8e7da · - · - · origem=Ronald (tmux)

Selos atuais:

- `test_contract_emissao_gate.py` usa AST, mas detecta apenas `*.objects.create(..., status_local=...)`; isenta todo `chamados/models.py` e pula migrations. Não cobre criação com estado padrão, `get_or_create`, lotes nem nascimento por instância+`save`. (`app/chamados/tests/test_contract_emissao_gate.py:14`, `:21`, `:40`, `:44`)
- O selo mais forte é `test_chokepoint_chamado_gate`: usa o censo de escrita e exige **zero fora das portas**, teto vazio e allowlist vazia. Porém sua unidade de autorização é o **arquivo**, não a função: qualquer escrita em módulo listado como porta é aceita. (`app/chamados/tests/test_chokepoint_chamado_gate.py:323`, `:350`, `:371`, `:376`)
- `ChamadoColaborador` autoriza vários módulos, incluindo todo `chamados/models.py`; portanto o selo atual não prova que nascimento/reabertura passou pela mesma guarda. (`app/core/portas.py:195`, `:216`, `:233`)
- O censo reconhece construção, `create/get_or_create/update_or_create/bulk_create`, `update/bulk_update`, `save` e gerentes relacionados; resolve imports, mas pula migrations. (`app/core/censo_escritas.py:8`, `:15`, `:20`, `:61`, `:66`, `:458`)
- `test_contract_status_nucleo` é regex, não AST: permite a linha `self.status_local = novo`, detecta atribuição e `update(status_local=...)`, pula migrations. (`app/chamados/tests/test_contract_status_nucleo.py:7`, `:9`, `:16`, `:23`)
- O contrato de dedup exige textualmente que `abrir` distinga `ENCERRADOS` e chame `_renascer_por_premissa_viva`; exige também que renascimento delegue a `reabrir`. Não verifica o veto de régua. (`app/chamados/tests/test_contract_dedup_status.py:28`, `:37`, `:41`, `:54`)

Para morder emissão por fora, o desenho precisa restringir **funções/atos dentro dos módulos autorizados**, além do selo atual de arquivo; provar que todo nascimento e `ENCERRADOS → VIVOS` passa pela guarda antes da escrita; e conter casos que violem deliberadamente por `create` sem `status_local`, `get_or_create`, instância+`save`, `update/bulk_update` e escrita dentro de `models.py` fora da função autorizada. Isso é requisito proposto, ainda inexistente. [nao verificado]

Contadores:

| Contador | O que responde hoje |
|---|---|
| `furos_sem_cobranca.medir` | **Não responde vivo em dia vetado**: monta conjunto de vivos de qualquer módulo e exclui o dia antes de consultar o veto; `veto` fica fora do total. (`app/ponto/services/furos_sem_cobranca.py:68`, `:74`, `:83`, `:96`, `:109`) |
| `furos_vetados_por_regua` | Lista células `veredito='furo'`, até hoje, exceto desligados, cujo juiz veta; não cruza com chamado vivo. (`app/ponto/services/furos_vetados.py:49`, `:52`, `:56`) |
| `vigia_fio.censo` | Parte de chamados vivos **com célula**, recorta `FURO_MODULES`, lê células acusadoras e mede fio/pergunta/decisão pendente; não consulta veto de régua. (`app/chamados/services/vigia_fio.py:186`, `:201`, `:228`, `:260`, `:268`, `:288`) |
| `vigia_carimbo` | Perguntas de hora já validadas sem materialização de fato; não conta chamado vivo em dia vetado. (`app/chamados/services/vigia_carimbo.py:97`, `:105`, `:119`) |
| `vigia_loop` | Conta pares fecha/reabre sem fato novo, escritas por cron e divergência da última trilha contra banco; não cruza com veto. (`app/chamados/services/vigia_loop.py:28`, `:45`, `:50`, `:86`, `:114`) |

Nenhum dos contadores examinados responde exatamente “chamado de turno vivo em dia vetado”. As exclusões e predicados acima demonstram os limites desses candidatos; ausência de qualquer outro contador na árvore inteira permanece [nao verificado].

## 30/10 08:18 · sessao 9bf5c72ae18e · O261-V · sonda · origem=Code

Pela sonda fornecida, sem verificação independente:

- **451 aparece em `fora_de_fase`**: template `179`; âncora `2026-10-29`, origem `declarada`; `trabalho_sem_batida=6`, `folga_com_turno=6`. **Sem âncora sugerida**: `{'ancora': None, 'motivo': 'ganho de 0 pp e ruido (minimo 40)', 'batidas': 27, 'casa_hoje': 52}`. [nao verificado]
- **Contador da frota**: `{'fonte': 'fase_12x36', 'n': 11, 'janela_dias': 14, 'piso': 3, 'universo': 341}`, com `n_vinculos=11`. [nao verificado]
- **451 consta em Cadastro x Realidade**: `CXR_COL451_paths=['/postos[116][1][1]/colab_id']`. A assinatura e o motivo dessa inclusão não aparecem no dado fornecido. [nao verificado]

Chamadores diretos de `fora_de_fase` no código de produção da cópia:

| Chamador | Local da chamada |
|---|---|
| `api_mensageria_fase_12x36` | `app/api/views_mensageria.py:1670` |
| `particionar` | `app/chamados/services/fila_por_causa.py:173` |
| `propostas` | `app/escala/services/propositor.py:456` |
| `linhas_para_a_pauta` | `app/escala/services/fase_12x36.py:215` |
| `Command.handle` de `vigiar_familia_escala` | `app/ponto/management/commands/vigiar_familia_escala.py:23` |

**Nenhum desses fornece a saída de `fora_de_fase` como entrada de Cadastro x Realidade.** `lista` reúne as entradas de cadastro em `cad`, lê a lavra do esmeril e monta os IDs pela união de `por_colab` com `cad` (`app/escala/services/cadastro_realidade.py:513`, `:518`, `:527`, `:535`, `:542`, `:549`, `:556`, `:563`, `:570`, `:575`, `:579`).

O destino sugerido dessa lista vem de `d['destino']` da lavra (`app/escala/services/cadastro_realidade.py:581`, `:590`), produzido por **`escala.detector_proposta.propor_escala`** (`app/ponto/services/esmeril_espelho.py:213`, `:223`); o chamador de `fora_de_fase` é **`escala.services.propositor.propostas`** (`app/escala/services/propositor.py:456`).

## 30/10 08:16 · sessao 9dc80579edf4 · O261-NOTURNO · sonda · origem=Code

Transcrição de `NOT|`, por motivo; “—” indica linha ausente no dado, sem assumir zero. [nao verificado]

| Motivo | instante_outro_dia | mesmo_dia | sem_instante | Soma das parcelas exibidas | Fonte |
|---|---:|---:|---:|---|---|
| intervalo_saida | 776 | 3443 | 3514 | 776 + 3443 + 3514 = **7733** | [nao verificado] |
| intervalo_volta | 1460 | 3261 | 3623 | 1460 + 3261 + 3623 = **8344** | [nao verificado] |
| orfao_14h | 2516 | 4653 | 6668 | 2516 + 4653 + 6668 = **13837** | [nao verificado] |
| saida_sem_entrada | — | 4983 | 4785 | 4983 + 4785 = **9768** | [nao verificado] |

Demais linhas `NOT|`, todas subordinadas a `instante_outro_dia`: [nao verificado]

| Campo | intervalo_saida | intervalo_volta | orfao_14h | Fonte |
|---|---:|---:|---:|---|
| data_evento==celula=True | 776 | 1460 | 2516 | [nao verificado] |
| plantio_no_dia_celula | 9 | 4 | 13 | [nao verificado] |
| plantio_no_dia_instante | 40 | 78 | 153 | [nao verificado] |
| texto_mostra_dia_da_celula=True | 776 | 1460 | 2516 | [nao verificado] |
| viva | 343 | 413 | 472 | [nao verificado] |

| Linha | Valores | Fonte |
|---|---|---|
| NOT\|plantios_amostrados | 271; com_batida_real_na_hora_no_dia_da_celula=39; colabs=7 | [nao verificado] |
| ERROS | 0 | [nao verificado] |

As somas calculadas estão acima. O dado não apresenta um total independente de cada motivo para confirmar se as parcelas fecham esse total. [nao verificado]

## 30/10 08:13 · sessao 7bacb9cf86c7 · O261-NOTURNO · sonda · origem=Code

**1. Nas quatro ATAs anexadas, E 19:00 está apagada.** Não há batida associada a esse marco: `luz=None`, `tipo_real=None`. O anexo não fornece instante de uma batida acendedora de E 19:00. [nao verificado]

| Célula | Dia | E 19:00 | Batida associada na ATA |
|---|---|---|---|
| 129745 | 21/10/2026 | Apagada | Nenhuma (`luz=None`) [nao verificado] |
| 129747 | 23/10/2026 | Apagada | Nenhuma (`luz=None`) [nao verificado] |
| 129749 | 25/10/2026 | Apagada | Nenhuma (`luz=None`) [nao verificado] |
| 129751 | 27/10/2026 | Apagada | Nenhuma (`luz=None`) [nao verificado] |

Isso concorda com a cobrança descrita de **“E 19:00 faltante”** e discorda de considerar esse mesmo marco cumprido para encerrar `saida_sem_entrada` como `fantasma`. Os registros desses chamados e perguntas, seus vínculos e horários de encerramento não estão na sonda; essa associação histórica individual permanece [nao verificado].

Na cópia, as leituras são estas:

- **Cobrança do furo:** `marcos_apagados` lê as lâmpadas apagadas da ATA; o cartório usa esse resultado para formar os marcos faltantes. (`app/ponto/services/lastro.py:118`; `app/ponto/services/cartorio.py:1429`)
- **Juiz atual da pergunta:** `saida_sem_entrada` cobra `('E', 'hi')`; procura a hora no DNA e lê `acesa` na ATA por `(hora, tipo)`. (`app/chamados/juizes.py:823`; `app/chamados/juizes.py:848`; `app/chamados/juizes.py:866`)
- **Chamador atual do encerramento:** exige que esse juiz devolva exatamente `True`; depois, só chama `encerrar_fantasma` quando o veredito é `ENCERRA`. Assim, uma lâmpada E19 apagada não passa por essa condição. (`app/chamados/reconciliador.py:1032`; `app/chamados/reconciliador.py:1079`; `app/chamados/reconciliador.py:1092`)
- **Escritor:** `encerrar_fantasma` grava via, instante do carimbo e autor. Seu corpo não consulta ATA nem batidas; `materializada_em=now()` é o instante do encerramento, não uma batida que acendeu o marco. (`app/chamados/models.py:2167`)

Portanto, **não é demonstrável que o chamador atual tenha produzido os encerramentos históricos mencionados**: faltam os respectivos registros e a ATA existente no instante de cada encerramento. [nao verificado]

**2. O predicado antigo preservado na lápide era morfológico:** turno recomputado completo **e** última saída a até 180 minutos do fim previsto, com `TOL_MORFOLOGIA_MIN=180`; aparecem os nomes removidos `_eh_fantasma`, `_bem_formado` e `_turnos_na_janela`. (`app/ponto/management/commands/reconciliar_perguntas_orfas.py:10`)

O comentário do reconciliador distingue expressamente os dois critérios: a regra posterior encerra pela lâmpada do próprio marco; a antiga julgava pela morfologia do turno. (`app/chamados/reconciliador.py:982`)

O universo identificado como “mesmo SQL do cron que carimbava” contém perguntas sem materialização, sem validação, em disputa aberta, dos motivos `orfao_14h`/`saida_sem_entrada`, com `data_evento` preenchida. (`app/chamados/reconciliador.py:992`)

**A lápide não preserva como o algoritmo antigo escolhia o dia da batida.** Não permite afirmar D, D+1 ou outro recorte. Tampouco a sonda contém as execuções de aproximadamente 06:35 em 22–28/10; atribuir-lhes o algoritmo completo ou uma batida específica seria [nao verificado].

**3. P26: o ramo efetivamente executado é [nao verificado] para cada uma das 26 linhas.** O carregamento de `_marcos` depende do vínculo e do tipo encontrados naquele dia; esses resultados não constam no anexo. (`app/chamados/juizes.py:298`)

Além disso, existem **dois retornos distintos que podem conservar `ts`**: `_marcos` preenchido, mas hora fora do envelope (`return ts`, linha 312), e caminho sem marcos, cujo retorno final está na linha 321. Logo, plantio em D não prova `_marcos` vazio. (`app/chamados/juizes.py:304`; `app/chamados/juizes.py:312`; `app/chamados/juizes.py:313`; `app/chamados/juizes.py:321`)

A aplicação matemática às **horas respondidas e marcos informados** está abaixo. O código encadeia os marcos atravessando a meia-noite, amplia o primeiro/último em 90 minutos e testa D antes de D+1. (`app/escala/servico_jornada.py:74`; `app/chamados/juizes.py:305`)

Os valores de produção transcritos e o ramo histórico de cada linha permanecem [nao verificado].

| P# | Resposta | Envelope calculado pelos marcos informados | Resultado pela hora respondida |
|---|---|---|---|
| 22818 | 07:00 | D 17:30 → D+1 08:30 | Envelope: D+1 |
| 22970 | 23:45 | D 17:30 → D+1 08:30 | Envelope: D |
| 22993 | 03:41 | D 22:00 → D+1 09:00 | Envelope: D+1 |
| 25281 | 03:01 | D 17:30 → D+1 08:30 | Envelope: D+1 |
| 25282 | 07:01 | D 17:30 → D+1 08:30 | Envelope: D+1 |
| 26108 | 23:30 | D 22:00 → D+1 09:00 | Envelope: D |
| 27227 | 18:55 | D 17:30 → D+1 08:30 | Envelope: D |
| 27290 | 12:00 | D 16:30 → D+1 07:30 | Fora: `return ts` |
| 27670 | 23:16 | D 22:00 → D+1 09:00 | Envelope: D |
| 27739 | 03:00 | D 17:30 → D+1 08:30 | Envelope: D+1 |
| 32357 | 01:00 | D 16:30 → D+1 07:30 | Envelope: D+1 |
| 33283 | 06:00 | D 20:30 → D+1 07:30 | Envelope: D+1 |
| 33629 | 23:14 | D 22:00 → D+1 09:00 | Envelope: D |
| 34397 | 09:15 | D 22:00 → D+1 09:00 | Fora: `return ts` |
| 35366 | 23:11 | D 22:00 → D+1 09:00 | Envelope: D |
| 37847 | 18:57 | D 17:30 → D+1 08:30 | Envelope: D |
| 38265 | 04:00 | D 17:30 → D+1 08:30 | Envelope: D+1 |
| 39186 | 23:57 | D 17:30 → D+1 08:30 | Envelope: D |
| 39761 | 23:59 | D 14:30 → D+1 01:30 | Envelope: D |
| 40675 | 23:56 | D 17:30 → D+1 08:30 | Envelope: D |
| 41549 | 23:50 | D 17:30 → D+1 08:30 | Envelope: D |
| 41707 | 19:00 | D 17:30 → D+1 08:30 | Envelope: D |
| 42368 | 22:41 | D 17:30 → D+1 08:30 | Envelope: D |
| 43845 | 11:59 | D 17:30 → D+1 08:30 | Fora: `return ts` |
| 46364 | 11:00 | D 17:30 → D+1 08:30 | Fora: `return ts` |
| 49873 | 10:11 | D 17:30 → D+1 08:30 | Fora: `return ts` |

Essa tabela não identifica o ramo histórico: nas perguntas de intervalo, o materializador pode substituir a resposta pela hora do marco faltante **antes** de montar `ts` e chamar o ajuste. O anexo não informa essa `_hora_fonte` efetiva. (`app/chamados/services/materializacao.py:388`; `app/chamados/services/materializacao.py:396`; `app/chamados/services/materializacao.py:404`)

Também não cabe deduzir ausência de marcos pela ausência de intervalo: havendo `hi` e `hf`, `marcos_turno` produz ao menos E inicial e S final. (`app/escala/servico_jornada.py:72`)

**4. O texto `pergunta` é formatado na emissão e gravado na criação.** O formatador substitui `{data}` por `contexto_disputa[motivo]['data']`, com `%d/%m/%Y`; a emissão passa `pergunta=texto` para `abrir`. (`app/chamados/catalogo/query.py:58`; `app/chamados/catalogo/query.py:64`; `app/chamados/services/disputa_emissao.py:316`; `app/chamados/services/disputa_emissao.py:389`)

`pergunta` é um `TextField`; `abrir` persiste os campos recebidos em `objects.create(..., **campos)`. Quando encontra pergunta existente, reaponta destino/célula e retorna sem reformatar seu texto. (`app/chamados/models.py:1855`; `app/chamados/models.py:2061`; `app/chamados/models.py:2085`)

As exibições citadas leem `p.pergunta`/`perg.pergunta`. Portanto, a data escrita na pergunta não comprova o dia da batida plantada. (`app/templates/chamados/partials/_card_atendimento.html:93`; `app/templates/chamados/partials/modal_fio.html:157`; `app/chamados/catalogo/query.py:64`)

## 30/10 08:12 · sessao d27e633afa7f · - · - · origem=Ronald (tmux)

Q3: **o ramo efetivamente executado é [não verificado] em todas as 26 linhas**. A sonda fornece DNA, resposta e plantio; não fornece `_ec`, `tipo_escala`, `_marcos`, `tipo`, `hora_tentativa` nem `_hora_fonte` efetivos. O carregamento de `_marcos` depende desses primeiros campos (`app/chamados/juizes.py:298-303`). A geradora pode ser rejeitada por vigência, devolvendo `None`, apesar de existir célula com DNA (`app/escala/alimentacao.py:237-251`).

Além disso, a dicotomia proposta é incompleta: existe `_marcos` **presente**, mas nenhuma data candidata dentro do envelope, seguido de `return ts` (`app/chamados/juizes.py:304-312`). Portanto “plantou em D” não demonstra `_marcos` vazio (`app/chamados/juizes.py:312,321`).

Os marcos são encadeados E-hi / S-hii / E-hfi / S-hf, avançando o dia quando o instante seguinte não é posterior ao anterior (`app/escala/servico_jornada.py:74-85`). O envelope vai do primeiro marco menos 90 min ao último mais 90 min; testa D antes de D+1 (`app/chamados/juizes.py:305-311`).

Cálculo **condicional**, usando os marcos DNA e a **hora da resposta** como entrada do ajuste; isto não identifica o ramo histórico real (`app/chamados/juizes.py:304-312`):

| P# | Resposta | Envelope pelo DNA | Resultado do cálculo |
|---|---|---|---|
|22818|07:00|D 17:30–D+1 08:30|D+1|
|22970|23:45|D 17:30–D+1 08:30|D|
|22993|03:41|D 22:00–D+1 09:00|D+1|
|25281|03:01|D 17:30–D+1 08:30|D+1|
|25282|07:01|D 17:30–D+1 08:30|D+1|
|26108|23:30|D 22:00–D+1 09:00|D|
|27227|18:55|D 17:30–D+1 08:30|D|
|27290|12:00|D 16:30–D+1 07:30|Fora; `return ts`|
|27670|23:16|D 22:00–D+1 09:00|D|
|27739|03:00|D 17:30–D+1 08:30|D+1|
|32357|01:00|D 16:30–D+1 07:30|D+1|
|33283|06:00|D 20:30–D+1 07:30|D+1|
|33629|23:14|D 22:00–D+1 09:00|D|
|34397|09:15|D 22:00–D+1 09:00|Fora; `return ts`|
|35366|23:11|D 22:00–D+1 09:00|D|
|37847|18:57|D 17:30–D+1 08:30|D|
|38265|04:00|D 17:30–D+1 08:30|D+1|
|39186|23:57|D 17:30–D+1 08:30|D|
|39761|23:59|D 14:30–D+1 01:30|D|
|40675|23:56|D 17:30–D+1 08:30|D|
|41549|23:50|D 17:30–D+1 08:30|D|
|41707|19:00|D 17:30–D+1 08:30|D|
|42368|22:41|D 17:30–D+1 08:30|D|
|43845|11:59|D 17:30–D+1 08:30|Fora; `return ts`|
|46364|11:00|D 17:30–D+1 08:30|Fora; `return ts`|
|49873|10:11|D 17:30–D+1 08:30|Fora; `return ts`|

**Atenção à hora de entrada:** `_ts_tipo_do_plantio` começa com a resposta, mas para `intervalo_saida`/`intervalo_volta` substitui-a pelo marco faltante, quando encontrado (`app/chamados/services/materializacao.py:388-395`); monta `ts` em `data_evento` e só depois chama o ajuste (`app/chamados/services/materializacao.py:396-404`). Logo não se pode transformar esta tabela condicional em atribuição histórica de ramo.

Usando **a hora do plantio** no mesmo cálculo, mudariam: P25281 19:00→D; P27739 19:00→D; P32357 19:00→D; P38265 19:00→D; P27670 03:00→D+1. O ajuste preserva a hora e decide apenas data (`app/chamados/juizes.py:307-311,318`).

A fonte do vínculo lê célula e geradora (`app/escala/servico_jornada.py:46-58`). Com vínculo e tipo existentes, `marcos_turno` usa `ec.marcos_do_dia` (`app/escala/servico_jornada.py:68-73`), que prefere DNA preenchido (`app/escala/models.py:1094-1108`). A ausência de `hii/hfi` não esvazia `_marcos` quando `hi/hf` existem; ainda há E-hi e S-hf (`app/escala/servico_jornada.py:72-77`).

## 30/10 08:11 · sessao 496a8624e08a · - · - · origem=Ronald (tmux)

O texto `pergunta` é formatado na emissão e persistido na criação:

- `app/chamados/catalogo/query.py:45-66`: busca o template e substitui `{data}` pelo valor de `contexto_disputa[motivo]['data']`, formatado em `%d/%m/%Y` na linha 64.
- `app/chamados/services/disputa_emissao.py:316-326`: chama esse formatador durante a geração.
- `app/chamados/services/disputa_emissao.py:389-397`: entrega `pergunta=texto` a `PerguntaDisputa.abrir`.
- `app/chamados/models.py:1855`: `pergunta` é `TextField`; `:2085-2086` persiste `**campos` por `objects.create`.
- Se a pergunta já existe, `abrir` reaponta destino/célula e retorna, sem reformatação do texto (`app/chamados/models.py:2061-2084`).
- A exibição citada lê o texto guardado: `{{ p.pergunta }}` em `app/templates/chamados/partials/_card_atendimento.html:93` e `{{ perg.pergunta }}` em `app/templates/chamados/partials/modal_fio.html:157`.

Assim, o texto datado não demonstra o instante nem o dia da batida plantada: sua data foi interpolada do contexto na emissão (`app/chamados/catalogo/query.py:58-64`; `app/chamados/services/disputa_emissao.py:316-326`).

Sobre o chamado vivo, a cópia atual extrai marcos apagados de `cel.ata['lampadas']` (`app/ponto/services/lastro.py:118-135`), nomeia-os em `_marcos_do_furo_parcial` (`app/ponto/services/cartorio.py:1429-1474`) e abre/atualiza o chamado usando esses marcos (`app/ponto/services/cartorio.py:1386-1405`). O espelhamento atualiza `contexto_json.marcos_faltantes` com `_marco_por='lampada_da_celula'` (`app/ponto/management/commands/detectar_ausencias.py:543-559`); a descrição criada diz “marco HH:MM (…) sem batida” (`:670-679`) e guarda a lista de marcos no contexto (`:700-706`).

O dado anexado não contém registros dos chamados vivos, das perguntas `saida_sem_entrada`, seus carimbos ou instantes de fechamento; associar uma execução histórica específica a essas linhas permanece [não verificado].

## 30/10 08:11 · sessao c1ac187e5ae1 · - · - · origem=Ronald (tmux)

- Nas quatro ATAs fornecidas, `E 19:00` está apagada (`acesa=False`, `luz=None`, `tipo_real=None`); nenhuma batida acendedora desse marco consta nelas. Como o dado está apenas no prompt, sem arquivo:linha: **[não verificado]**.
- O dado fornecido não traz registros dos chamados vivos, perguntas `saida_sem_entrada`, carimbos, datas de encerramento ou batidas brutas. A atribuição de cada encerramento histórico a um chamador e o instante que ele considerou ficam **[não verificado]**.
- `saida_sem_entrada` corresponde a `('E', 'hi')`; o juiz localiza a hora no DNA e lê `acesa` na ATA pela chave `(hora,tipo)`: `app/chamados/juizes.py:823-827`, `:848-872`. Sob essas ATAs e `hi=19:00`, a resposta desse juiz é `False`; o predicado atual interrompe imediatamente, sem produzir `fantasma`: `app/chamados/reconciliador.py:1032-1033`.
- O chamador atual consulta `veredito_orfa`, só escreve quando recebe `ENCERRA`, e chama a porta `encerrar_fantasma`: `app/chamados/reconciliador.py:1079-1093`. A porta apenas grava `via_resolucao`, `materializada_em` e autor; não lê ATA ou batidas: `app/chamados/models.py:2167-2174`.
- Portanto, a cobrança alegada de “E 19:00 faltante” é compatível com as ATAs; o alegado encerramento desse mesmo marco como `fantasma` diverge do estado da ATA. A existência e cronologia concretas desses registros não estão demonstradas pela sonda: **[não verificado]**.
- A lápide histórica documenta `_eh_fantasma`/`_bem_formado`/`_turnos_na_janela`, turno recomputado completo e última saída a até 180 minutos do fim previsto (`TOL_MORFOLOGIA_MIN=180`): `app/ponto/management/commands/reconciliar_perguntas_orfas.py:10-15`. O comentário atual distingue explicitamente essa morfologia da leitura da lâmpada: `app/chamados/reconciliador.py:982-987`.
- O universo preservado pelo comentário “mesmo SQL do cron que carimbava” é: pergunta sem materialização, sem validação, disputa aberta, motivo `orfao_14h`/`saida_sem_entrada` e `data_evento` presente: `app/chamados/reconciliador.py:992-999`. A guarda de resposta é descrita como herdada do cron em `app/ponto/tests/test_vassoura_orfas_resposta.py:8-10`.
- O algoritmo histórico de escolha do **dia** da batida não foi preservado nessa lápide/comentário. Ela não demonstra se a satisfação usava D, D+1 ou outro dia; atribuir um desses mecanismos ao cron de 22–28/10 fica **[não verificado]**. O horário de ~06:35 de cada execução também não consta na sonda: **[não verificado]**.
- Informação documental adicional: o registro de 27/10 15h diz que a proposta de tolerância pelas duas pontas foi retirada, permanecendo somente a saída: `app/docs/RELATO.md:518`. Não usei isso como prova de execução de produção.

## 30/10 08:06 · sessao 329ed3bd6ff8 · O261-NOTURNO · sonda · origem=Code

1. A sonda é **aritmeticamente compatível nos números apresentados**, mas não fornece um total independente por motivo para conferir a cobertura. As somas abaixo são derivadas das categorias informadas. `[nao verificado]`

| Motivo | Instante em outro dia | Mesmo dia | Sem instante | Soma informada | Evidência |
|---|---:|---:|---:|---:|---|
| `intervalo_saida` | 903 | 3316 | 3514 | **7733** | [nao verificado] |
| `intervalo_volta` | 1378 | 3343 | 3623 | **8344** | [nao verificado] |
| `orfao_14h` | 2516 | 4653 | 6668 | **13837** | [nao verificado] |
| `saida_sem_entrada` | — | 4983 | 4785 | **9768** | [nao verificado] |
| Soma das categorias informadas | **4797** | **16295** | **18590** | **39682** | [nao verificado] |

“—” significa que a linha não foi fornecida; zero explícito para `saida_sem_entrada|instante_outro_dia` é `[nao verificado]`.

| Motivo, somente com instante em outro dia | Perguntas | `data_evento==célula=True` | Texto mostra dia da célula=True | Plantio no dia da célula | Plantio no dia do instante | Evidência |
|---|---:|---:|---:|---:|---:|---|
| `intervalo_saida` | 903 | 903 | 903 | 9 | 40 | [nao verificado] |
| `intervalo_volta` | 1378 | 1378 | 1378 | 4 | 77 | [nao verificado] |
| `orfao_14h` | 2516 | 2516 | 2516 | 13 | 153 | [nao verificado] |
| Soma | **4797** | **4797** | **4797** | **26** | **270** | [nao verificado] |

| Indicador final | Valor | Evidência |
|---|---:|---|
| Plantios amostrados | 270 | [nao verificado] |
| Com batida real, origem app, a ≤15 min da mesma hora no dia da célula | 39 | [nao verificado] |
| Colaboradores | 7 | [nao verificado] |
| `ERROS` | **0** | [nao verificado] |

Os plantios no dia do instante somam `40+77+153=270`, numericamente igual à amostra final. As duas categorias de plantio somam 296; o destino das demais perguntas com instante em outro dia é `[nao verificado]`. A execução e a correção interna da sonda também são `[nao verificado]`.

2. Nos **4797 casos informados**, o texto gravado nomeia **o dia da célula**, e `data_evento` também é esse dia. `[nao verificado]`

| Motivo | Texto do catálogo |
|---|---|
| `orfao_14h` | “Confirme o horário da saída no dia {data}” — [perguntas.py:49](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/catalogo/perguntas.py:49) |
| `intervalo_saida` | “Confirme o horário da saída do intervalo no dia {data}” — [perguntas.py:125](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/catalogo/perguntas.py:125) |
| `intervalo_volta` | “Confirme o horário da volta do intervalo no dia {data}” — [perguntas.py:133](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/catalogo/perguntas.py:133) |

O código sustenta a diferença entre essas datas pelos seguintes caminhos:

- **DNA:** `montar_lampadas()` grava `instante` em `data+1` quando o turno cruza meia-noite e a hora do marco é menor que `hi`; nos demais casos usa `data`. [gerar_celulas.py:67](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/gerar_celulas.py:67)
- **Texto pela grade:** o leitor entrega marcos contendo somente `tipo` e `hora`; `_marcos_faltantes()` devolve o dia da grade em `(data_dia, pos, tipo, hora)`. A emissão formata esse `dia` no texto e o passa como `data_evento`. [leitor_celula.py:120](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/escala/services/leitor_celula.py:120), [declaracao_texto.py:120](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/declaracao_texto.py:120), [disputa_emissao.py:812](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/disputa_emissao.py:812), [disputa_emissao.py:777](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/disputa_emissao.py:777)
- **Declaração direta:** usa o mesmo `dia` no texto e em `data_evento`. [declaracao_texto.py:321](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/declaracao_texto.py:321)
- **Outros caminhos de emissão:** o genérico substitui `{data}` por `contexto[motivo]['data']`; o dedicado de intervalo formata a data de `marcos_faltantes`. [disputa_emissao.py:317](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/disputa_emissao.py:317), [query.py:58](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/catalogo/query.py:58), [disputa_emissao.py:952](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/disputa_emissao.py:952)

**O plantio parte de `data_evento` e deriva o dia pelo ajuste overnight.** `_ts_tipo_do_plantio()` escolhe `p.data_evento`, com fallback para a data do contexto; combina essa data com a hora respondida e chama `_ajustar_saida_overnight()`. Nos intervalos, a hora pode ser substituída pela hora do marco faltante. [materializacao.py:328](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/materializacao.py:328), [materializacao.py:391](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/materializacao.py:391), [materializacao.py:398](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/materializacao.py:398), [materializacao.py:404](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/materializacao.py:404)

O ajuste consulta os marcos do turno e testa, nessa ordem, **D e D+1**, retornando o primeiro candidato dentro do envelope entre primeiro marco−90 minutos e último marco+90 minutos. Fora desse envelope conserva o instante recebido. Sem marcos, para entrada com `hora_tentativa`, pode recuar para **D−1**. Portanto, não há regra “todo instante em outro dia planta em D+1”. [juizes.py:301](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:301), [juizes.py:305](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:305), [juizes.py:313](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:313)

O registro recebe esse `ts`; o vínculo `pergunta_origem` é anotado tanto para batida nova quanto reutilizada. [materializacao.py:872](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/materializacao.py:872), [materializacao.py:891](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/materializacao.py:891)

3. O consumidor que **recompõe diretamente `data_evento + hora respondida`, sem ajuste overnight**, é `classe_do_lote()`: combina `perg.data_evento` com `d['resp']`, procura batidas apuráveis em **±60 minutos** e retorna B com conflito ou A. [validacao.py:747](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/validacao.py:747), [validacao.py:812](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/validacao.py:812), [validacao.py:817](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/validacao.py:817)

Essa classificação alimenta a seleção de validação em lote, a fila classificada e a classe devolvida pela API. [validacao.py:681](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/validacao.py:681), [validacao.py:868](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/validacao.py:868), [views_mensageria.py:545](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/api/views_mensageria.py:545)

Também há leitores que usam `data_evento` para escolher o marco e comparam **somente HH:MM**, sem construir o instante ajustado:

- `delta_da_resposta()`: usa o marco do chamado ou o fallback da grade, que consulta `marcos_do_dia(data_evento)`, e calcula a distância entre horários. [juizes.py:1247](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:1247), [juizes.py:1256](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:1256), [juizes.py:1299](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:1299), [juizes.py:1316](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:1316)
- `motivo_implausivel_validacao()`: faz essa seleção de marco e compara a hora; é chamado antes da validação. [juizes.py:1425](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:1425), [validacao.py:367](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/validacao.py:367)
- `classificar_resposta_validacao()` consome `delta_da_resposta()`; o comando `validar_classe_a` usa essa classificação. [juizes.py:1409](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:1409), [validar_classe_a.py:63](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/management/commands/validar_classe_a.py:63)

Para respostas `hora`, `_slot_esperado()` já delega ao derivador do plantio e recebe o ajuste. [juizes.py:343](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:343)

## 30/10 08:04 · sessao fea8f0a62a04 · - · - · origem=Ronald (tmux)

Leitor que efetivamente recompõe **data_evento + hora respondida sem ajuste**:

- `classe_do_lote()` lê `delta_da_resposta()` em `app/chamados/services/validacao.py:812`; combina `perg.data_evento` com `d['resp']` em `:817-819`; procura batidas apuráveis no intervalo de ±60 minutos (`:747`, `:820-821`); retorna lote B com conflito (`:822`) ou lote A (`:823`). Esse trecho não chama `_ajustar_saida_overnight`.
- Esse resultado alimenta a fila classificada em `app/chamados/services/validacao.py:868`, a seleção de validação em lote em `:681` e a classe devolvida pela API em `app/api/views_mensageria.py:542-546`.

Leitores de **data_evento para escolher o marco e hora respondida para comparar**, sem construir instante ajustado:

- `_marco_esperado_da_grade()` lê `perg.data_evento` e consulta `marcos_do_dia(d)` (`app/chamados/juizes.py:1247`, `:1256`), selecionando `hii`/`hfi` para intervalo (`:1261-1264`).
- `delta_da_resposta()` lê a resposta e reduz `data_hora` a HH:MM (`app/chamados/juizes.py:1299-1302`), usa o marco do chamado ou aquele fallback (`:1303-1305`) e compara minutos pela distância circular (`:1316-1337`).
- `motivo_implausivel_validacao()` segue a mesma seleção de marco e comparação de HH:MM (`app/chamados/juizes.py:1425-1436`); é chamado antes da validação em `app/chamados/services/validacao.py:367-370`.
- `classificar_resposta_validacao()` usa `delta_da_resposta()` (`app/chamados/juizes.py:1409-1413`); o comando `validar_classe_a` usa essa classificação (`app/chamados/management/commands/validar_classe_a.py:63-67`).

**Não incluir como leitor sem ajuste:** `_slot_esperado()` para resposta do tipo `hora` delega ao próprio `_ts_tipo_do_plantio()` (`app/chamados/juizes.py:343-345`). Para `data_hora`, ele usa a data explicitamente respondida (`:331-338`), não `data_evento`.

## 30/10 08:04 · sessao be7d2a5d50f0 · - · - · origem=Ronald (tmux)

O texto usa o **dia da célula/data_evento**, e o DNA pode registrar o marco em **D+1**: `montar_lampadas` aplica `data + 1 dia` quando o turno cruza meia-noite e a hora do marco é menor que `hi`, gravando esse dia em `instante` (`app/ponto/management/commands/gerar_celulas.py:55`, `:67-69`). `montar_dna` incorpora essas lâmpadas (`app/ponto/management/commands/gerar_celulas.py:94`, `:103`).

| Motivo | Texto canônico |
|---|---|
| `orfao_14h` | “Confirme o horário da saída no dia {data}” — `app/chamados/catalogo/perguntas.py:48-50` |
| `intervalo_saida` | “Confirme o horário da saída do intervalo no dia {data}” — `app/chamados/catalogo/perguntas.py:124-126` |
| `intervalo_volta` | “Confirme o horário da volta do intervalo no dia {data}” — `app/chamados/catalogo/perguntas.py:132-134` |
| `saida_sem_entrada` | “Confirme o horário da entrada no dia {data}” — `app/chamados/catalogo/perguntas.py:56-58` |

Cadeia que comprova o dia da célula na formatação:

- O leitor indexa `CelulaDia` por `c.data`, põe esse dia no resultado `data` e entrega os marcos da ata (`app/escala/services/leitor_celula.py:325`, `:337`, `:354-359`). Os marcos carregam apenas `tipo` e `hora`, sem `instante` (`app/escala/services/leitor_celula.py:120`).
- `_marcos_faltantes` lê esses dias da grade, extrai `tipo/hora` e devolve `(data_dia, pos, tipo, hora)` (`app/chamados/services/declaracao_texto.py:82-89`, `:109`, `:120`).
- Na complementação da emissão, cada tupla fornece `dia`; esse `dia` é formatado como `%d/%m/%Y` e passado à abertura (`app/chamados/services/disputa_emissao.py:806-813`). A abertura grava esse argumento em `data_evento` (`app/chamados/services/disputa_emissao.py:770-777`).
- Na declaração direta, o marco casado fornece `dia`; a criação usa esse mesmo dia no texto e em `data_evento` (`app/chamados/services/declaracao_texto.py:296-297`, `:321-323`).
- Na declaração por proposta, `dia` vem do marco casado e entra na proposta; a criação repete esse dia no texto e em `data_evento` (`app/chamados/services/declaracao_texto.py:576-583`, `:610-612`).

Outros caminhos de formatação na emissão:

- Caminho genérico usa `texto_pergunta_com_data(motivo, disputa.contexto)` (`app/chamados/services/disputa_emissao.py:316-317`); o helper substitui `{data}` por `contexto[motivo]['data']` (`app/chamados/catalogo/query.py:58-64`).
- Caminho dedicado de intervalo usa `_m['data']`, tanto no texto como no argumento `data_evento` (`app/chamados/services/disputa_emissao.py:950-957`).
- Agrupamento por episódio formata com o `data_evento` resolvido e passa esse mesmo valor à abertura (`app/chamados/services/disputa_emissao.py:1006-1013`).

A afirmação sobre **quais registros de produção exibiram o dia da célula** depende exclusivamente da sonda fornecida, portanto precisa receber `[nao verificado]`; o código comprova os caminhos de formação acima, sem comprovar a execução histórica de cada registro.

## 30/10 07:53 · sessao e13c20d27801 · O261-V · sonda · origem=Code

**1. Batida → pergunta de origem → texto gravado.**

As linhas abaixo transcrevem a sonda fornecida, sem arquivo:linha disponível nem consulta independente a produção: **[não verificado]**. O código grava o vínculo em `pergunta_origem` em [materializacao.py:891](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/materializacao.py:891).

| Batida | Timestamp mostrado | `pergunta_origem` | Campo `pergunta` e dia mostrado |
|---|---|---|---|
| B140648 | 20/10 07:00 | P46023 | Texto e dia da pergunta ausentes da sonda. **[não verificado]** |
| B140649 | 22/10 07:00 | P46704 | “Confirme o horário da saída no dia **21/10/2026**”. **[não verificado]** |
| B140650 | 29/10 11:33:16 | `None` | Origem `app`; não é `disputa_s84_retro`. **[não verificado]** |
| B140651 | 20/09 19:00 | P34113 | Texto e dia da pergunta ausentes da sonda. **[não verificado]** |
| B140652 | 24/10 07:00 | P49423 | “Confirme o horário da saída no dia **23/10/2026**”. **[não verificado]** |
| B140653 | 26/10 07:00 | P50180 | “Confirme o horário da saída no dia **25/10/2026**”. **[não verificado]** |
| B140654 | 28/10 07:00 | P51025 | “Confirme o horário da saída no dia **27/10/2026**”. **[não verificado]** |

Não é possível preencher os textos de P46023/P34113 pelo timestamp da batida ou pelo template: o valor efetivamente gravado nessas perguntas não foi fornecido. **[não verificado]**

**2. Escritor de `fantasma`, predicado e batidas que teriam casado.**

O escritor da cópia é `PerguntaDisputa.encerrar_fantasma(autor)`: atribui `via_resolucao=VIA_FANTASMA`, `materializada_em=now()` e `materializada_por=autor`; salva somente esses três campos, sem exigir nem escrever resposta. [models.py:2162](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/models.py:2162), [models.py:2170](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/models.py:2170).

O chamador atual é `encerrar_orfas_pela_lampada`, que obtém o usuário de sistema e chama essa porta. O helper busca `username='sistema'`; sua correspondência com o ID 666 é **[não verificado]**. [reconciliador.py:1086](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/reconciliador.py:1086), [core/utils.py:18](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/core/utils.py:18).

O predicado atual funciona assim:

- `saida_sem_entrada` cobra `('E', 'hi')`; `orfao_14h` cobra `('S', 'hf')`. [juizes.py:823](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:823).
- Lê a hora em `p.celula.dna['marcos']` e procura a lâmpada da ATA pela chave **`(hora, tipo)`**, verificando `acesa`. Esse trecho não consulta uma `Batida`, não retorna seu ID e não lê `dna['lampadas'][].instante`. [juizes.py:845](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:845), [juizes.py:863](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:863).
- Exige resposta vazia, marco comprovadamente aceso e pergunta viva. Depois exige dia anterior a hoje e encerramento desse dia. **A linha que escolhe o dia para consultar o encerramento é `dia_encerrado_motivo(..., p.data_evento, ...)`.** [reconciliador.py:1030](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/reconciliador.py:1030), [reconciliador.py:1038](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/reconciliador.py:1038).

Portanto, **a associação local usa hora+tipo dentro da célula vinculada**; o dia participa pela escolha dessa célula e pelo encerramento em `p.data_evento`. Não existe nesse predicado uma comparação direta de timestamp de batida por “dia+hora” ou “só hora”. A porta de abertura escolhe a célula por `data=data_evento`. [models.py:2060](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/models.py:2060), [juizes.py:866](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:866).

Há uma limitação temporal: o comando atual `reconciliar_perguntas_orfas` é vigia. Seu comentário registra a remoção da regra antiga `_eh_fantasma`/`_bem_formado`/`_turnos_na_janela`, que julgava turno completo e última saída até 180 minutos do fim previsto. Esse comentário não fornece o corpo antigo nem prova a execução que produziu cada carimbo de 22–28/10. **Predicado histórico exato desses carimbos: [não verificado].** [reconciliar_perguntas_orfas.py:1](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/reconciliar_perguntas_orfas.py:1), [reconciliar_perguntas_orfas.py:10](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/reconciliar_perguntas_orfas.py:10).

Aplicação ao material fornecido:

| Pergunta | Marco indicado no pedido | Batida que casou |
|---|---|---|
| P46703 | E 21/10 19:00 | ID não demonstrado. **[não verificado]** |
| P49422 | E 23/10 19:00 | ID não demonstrado. **[não verificado]** |
| P50179 | E 25/10 19:00 | ID não demonstrado. **[não verificado]** |
| P50883 | E 27/10 19:00 | ID não demonstrado. **[não verificado]** |
| P50169 | E 25/10 21:30 | ID não demonstrado. **[não verificado]** |

A sonda não fornece DNA/ATA dessas células nem as batidas que acenderam suas lâmpadas. Entre B140648–140654, a única entrada mostrada é B140651, de **20/09 19:00**, criada em **29/10**; as demais são saídas. Nenhuma identifica a entrada responsável pelos carimbos apresentados. **[não verificado]**

O trecho também não exibe `respondida_em`; a desigualdade `materializada_em < respondida_em` declarada no pedido não pode ser conferida nele. **[não verificado]**

**3. Leitores/escritores que usam o dia da célula e tratamento do instante noturno.**

O produtor do DNA grava o instante próprio do marco: escolhe **D+1 quando o turno cruza a meia-noite e `hora < hi`**, e escreve esse dia em `instante`. [gerar_celulas.py:67](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/gerar_celulas.py:67).

| Caminho | Uso do dia / instante |
|---|---|
| **Grade consumida pelas perguntas** | Indexa por `c.data`, devolve esse dia e projeta os marcos somente como `tipo/hora`, sem `instante`. [leitor_celula.py:325](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/escala/services/leitor_celula.py:325), [linha 337](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/escala/services/leitor_celula.py:337), [linha 120](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/escala/services/leitor_celula.py:120). |
| **Marcos faltantes** | Retorna `(data_dia, posição, tipo, hora)` usando o dia da grade, inclusive para a saída da véspera; não transporta o instante próprio. [declaracao_texto.py:88](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/declaracao_texto.py:88), [linha 116](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/declaracao_texto.py:116). |
| **Texto da pergunta complementar** | Formata `{data}` com o `dia` retornado acima e usa esse mesmo dia na criação da pergunta. [disputa_emissao.py:806](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/disputa_emissao.py:806). |
| **Texto nas portas de declaração** | Colaborador e aplicação pelo admin usam o dia do marco selecionado tanto no texto quanto em `data_evento`. [declaracao_texto.py:321](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/declaracao_texto.py:321), [linha 610](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/declaracao_texto.py:610). |
| **Carimbo fantasma atual** | Consulta a célula vinculada e sua lâmpada por `hora/tipo`; usa `data_evento` para o encerramento do dia. Não consulta o instante do DNA. [juizes.py:845](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:845), [linha 866](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:866), [reconciliador.py:1038](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/reconciliador.py:1038). |
| **Plantio de resposta `hora`** | Começa com `data_evento + hora`, mas chama o ajuste overnight antes de devolver o timestamp. O ajuste procura D ou D+1 dentro do envelope da cadeia de marcos, com margem de 90 minutos. **Não permanece obrigatoriamente no dia da célula.** [materializacao.py:398](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/materializacao.py:398), [linha 404](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/materializacao.py:404), [juizes.py:301](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:301). |
| **Classificação da validação em lote** | Na busca de conflito, combina diretamente `perg.data_evento + hora respondida`; esse trecho não passa pelo ajuste overnight. [validacao.py:817](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/validacao.py:817). |
| **Âncora de cobrança de furo parcial** | Projeta marcos em `tipo/hora/motivo`, ordena por hora e combina a primeira com `data`; essa âncora é enviada à abertura do chamado. [cartorio.py:1467](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/cartorio.py:1467), [linha 1470](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/cartorio.py:1470), [linha 1395](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/cartorio.py:1395). |
| **Reemissão C2** | Busca a célula pelo dia e grava seus marcos apagados no contexto. A projeção contém somente `hora/tipo/motivo`, sem instante. [reconciliador.py:661](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/reconciliador.py:661), [linha 674](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/reconciliador.py:674), [lastro.py:128](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/lastro.py:128). |

## 30/10 07:52 · sessao 03ab7c3f332f · - · - · origem=Ronald (tmux)

Na cópia, os caminhos relevantes são:

| Caminho | Dia utilizado / tratamento |
|---|---|
| DNA | `montar_lampadas` escolhe D+1 quando o turno cruza e a hora é anterior a `hi`; grava esse dia em `instante`. `app/ponto/management/commands/gerar_celulas.py:67-69`. |
| Grade consumida pelas perguntas | `grade_da_celula` indexa células por `c.data` e devolve esse dia em `base['data']`; `_celulas_da_ata` projeta marcos contendo somente `tipo` e `hora`, sem `instante`. `app/escala/services/leitor_celula.py:120`, `:325`, `:337`, `:354-365`. |
| Marcos faltantes | `_marcos_faltantes` percorre o dia da grade (`data_dia`), extrai somente `tipo/hora` e retorna `(data_dia, pos, tipo, hora)`. `app/chamados/services/declaracao_texto.py:88`, `:109`, `:120`. |
| Texto da pergunta complementar | A fábrica usa o `dia` retornado acima no texto e em `data_evento`; não lê `lampadas[].instante`. `app/chamados/services/disputa_emissao.py:806-813`. |
| Texto nas portas de declaração | A declaração do colaborador usa o `dia` casado ao marco para texto e `data_evento`; a aplicação pelo admin faz o mesmo com `p['dia']`. `app/chamados/services/declaracao_texto.py:296`, `:321-323`, `:576`, `:610-612`. |
| Vínculo pergunta → célula | `PerguntaDisputa.abrir` busca a célula em `data=data_evento`; reaponta a existente ou grava a nova pergunta com essa célula. `app/chamados/models.py:2058-2060`, `:2071-2073`, `:2085-2086`. |
| Carimbo fantasma atual | `fato_do_marco_no_espelho` lê `p.celula`, escolhe a hora em `dna.marcos` e consulta `ata.lampadas` por `(hora,tipo)`, retornando `acesa`. Não casa uma `Batida` e não lê `lampadas[].instante`. `app/chamados/juizes.py:845-849`, `:863-872`. O escritor exige resposta vazia, consulta esse fato e usa `p.data_evento` no teto/encerramento antes de chamar `encerrar_fantasma`. `app/chamados/reconciliador.py:1030-1038`, `:1092`. |
| Plantio de resposta `hora` | Começa com `data_evento + hora`, porém chama ajuste overnight antes de devolver o timestamp. O ajuste reconstrói os marcos, procura D ou D+1 dentro do envelope de 90 minutos e devolve o candidato. Portanto não cabe descrevê-lo como “sempre planta no dia da célula”. `app/chamados/services/materializacao.py:328-329`, `:398`, `:404`; `app/chamados/juizes.py:301-312`. A cadeia reconstrói datas crescentes em `app/escala/servico_jornada.py:81-84`. |
| Classificação de validação em lote | Para procurar conflito, combina diretamente `perg.data_evento + hora respondida` e consulta ±60 minutos, sem passar pelo ajuste overnight nesse trecho. `app/chamados/services/validacao.py:747`, `:817-822`. |
| Âncora de cobrança de furo parcial | Projeta os apagados somente em `tipo/hora/motivo`, ordena por hora, escolhe a primeira e combina essa hora com `data`. Essa âncora é usada na abertura do chamado. `app/ponto/services/cartorio.py:1463-1474`, `:1394-1396`. |
| Reemissão C2 / nomeação de marcos | C2 busca a célula pelo dia e grava `marcos_apagados(cel)` no contexto. Esse leitor projeta somente `hora/tipo/motivo`. `app/chamados/reconciliador.py:661`, `:674-677`; `app/ponto/services/lastro.py:128-135`. O comando `nomear_marco_por_lampada` também usa essa projeção e a grava no contexto. `app/chamados/management/commands/nomear_marco_por_lampada.py:59-61`, `:109`. |

Distinções adicionais para evitar generalização:

- O detector dedicado de intervalo recompõe a cadeia desde `cel.data`, mas grava em cada marco a **data do datetime encadeado**, permitindo D+1. `app/ponto/management/commands/detectar_intervalo_ausente.py:73-80`.
- O veredito humano de dia também recompõe a cadeia e avança o dia quando o marco seguinte não é posterior ao anterior. `app/ponto/services/veredito_celula.py:63-70`, `:229`.
- `juiz_batida` chama o produtor `montar_lampadas(celula.data, ...)` e depois lê o `instante` reconstruído; não mantém todos os marcos em D. `app/ponto/juiz_batida.py:174-176`, `:236`.

## 30/10 07:51 · sessao 2084a93f251f · - · - · origem=Ronald (tmux)

O único escritor ativo encontrado de `via_resolucao='fantasma'` é `PerguntaDisputa.encerrar_fantasma(autor)`: grava via, `timezone.now()` e autor, salvando somente esses três campos; não exige nem grava resposta. [app/chamados/models.py:2162–2174]

O chamador atual é `encerrar_orfas_pela_lampada`, que obtém `get_user_sistema()` e chama a porta em `:1092`. O ID desse usuário não é fixado no código: o helper procura/cria `username='sistema'`; que corresponda ao usuário 666 é **[não verificado]**. [app/chamados/reconciliador.py:1086–1092; app/core/utils.py:6–18]

O predicado atual não busca uma batida por timestamp nem devolve seu ID. Para `saida_sem_entrada`, escolhe `('E','hi')`; para `orfao_14h`, `('S','hf')`. Lê `cel.dna['marcos']`, cruza `cel.ata['lampadas']` por `(hora,tipo)` e retorna se todas as lâmpadas correspondentes estão acesas. Não lê `dna['lampadas'][].instante`. [app/chamados/juizes.py:823–828,842–872]

Antes de carimbar, exige resposta vazia, data presente, fato do marco `True`, pergunta viva, `data_evento < hoje` e encerramento do dia do colaborador. A linha que escolhe o dia para esse encerramento é `dia_encerrado_motivo(p.disputa.colaborador, p.data_evento, agora=agora)`. [app/chamados/reconciliador.py:1030–1040]

O comando `reconciliar_perguntas_orfas` da cópia atual é somente vigia. Seu comentário registra que a regra antiga `_eh_fantasma` / `_bem_formado` / `_turnos_na_janela` foi removida em 29/10; diz que julgava completude do turno e última saída até 180 minutos do fim previsto. O corpo antigo e a execução que produziu carimbos de 22–28/10 não estão demonstrados pelo código atual. [app/ponto/management/commands/reconciliar_perguntas_orfas.py:1–15] **Escritor/predicado histórico exato: [não verificado].**

A sonda colada não contém `respondida_em`, DNA, ATA ou batidas E nos dias 21/23/25/27 às 19:00, nem E em 25/10 às 21:30. Portanto não permite identificar qual batida satisfez P46703/P49422/P50179/P50883/P50169; o predicado atual tampouco identifica a batida. **Casamentos e antecedência `materializada_em < respondida_em`: [não verificado]**.

Citações devem ser expandidas para links absolutos pelo consolidante, e as referências em intervalo podem ser convertidas em linhas individuais.

## 30/10 07:48 · sessao 0b5243c88ab6 · O261-VETO-NA-PORTA · censo · origem=Code

Não existe uma porta única de escrita para nascimento e reabertura: há `get_or_create` em `abrir`, `create` em `criar`, `save` em `_gravar_estado` e um `update` direto no reverso da migration 0038. (`app/chamados/models.py:730`, `app/chamados/models.py:834`, `app/chamados/models.py:231`, `app/chamados/migrations/0038_a3_registrado_backfill.py:15`)

O universo estrito é `batida_ausente`, `turno_aberto`, `intervalo_pendente` e `entrada_bloqueada_turno_aberto`; incluí também `turno_aberto_24h`, os emissores de chamados-pais de disputa e as reaberturas genéricas sem filtro que exclua furo. (`app/chamados/catalogo/furo_modulos.py:15`, `app/chamados/catalogo/modulos.py:109`, `app/chamados/services/disputa_emissao.py:507`, `app/chamados/services/acoes_chamado.py:315`)

Para evitar confundir vetos diferentes, as tabelas usam estas referências:

| Referência | Veto efetivamente consultado |
|---|---|
| **Régua** | `motivo_do_veto` consulta `regua_degenerada` e `vetar_cobranca_se_regua_suspeita`. Com vínculo ausente, retorna `None`. (`app/ponto/services/furos_vetados.py:26`, `app/ponto/services/furos_vetados.py:28`, `app/ponto/services/furos_vetados.py:30`) |
| **Célula** | `celula_veta_cobranca`: veta `trabalha=False` e os vereditos `nunca_bateu`, `trabalhou`, `justificado`, `coberta`, `cobranca_indevida`. Sem célula, não veta. (`app/ponto/marcos.py:119`, `app/ponto/marcos.py:156`, `app/ponto/marcos.py:159`, `app/ponto/marcos.py:161`) |
| **Nascimento** | `abrir` e `criar` consultam `_celula_que_absolve`; somente alcança `MODULOS_DE_TURNO` com célula vinculada: `turno_aberto`, `turno_aberto_24h`, `entrada_bloqueada_turno_aberto`. Portanto, esse veto compartilhado não alcança `batida_ausente` nem `intervalo_pendente`. (`app/chamados/models.py:726`, `app/chamados/models.py:822`, `app/chamados/models.py:849`, `app/chamados/reconciliador.py:336`) |
| **Nascimento — predicado** | `_celula_que_absolve → _absolvido_pela_celula`: recusa quando há veredito não vazio fora de `VEREDITOS_ACUSAM` e não há marcos pendentes. É veto pela célula, não consulta de régua. (`app/chamados/models.py:851`, `app/chamados/reconciliador.py:974`, `app/chamados/reconciliador.py:975`, `app/chamados/reconciliador.py:977`) |
| **Reabertura** | `reabrir → recusa_da_reabertura`: fechamento administrativo sem fato novo; premissa regularizada de furo; agrupamento; irmão vivo; motor. (`app/chamados/models.py:1050`, `app/chamados/models.py:1003`, `app/chamados/models.py:1009`, `app/chamados/models.py:1012`, `app/chamados/models.py:1015`, `app/chamados/models.py:1019`) |

A coluna “antes” considera a escrita indicada. A chamada ao cartório pelo `post_save` do chamado acontece depois dessa escrita e não constitui veto anterior a ela. (`app/chamados/signals.py:164`, `app/chamados/signals.py:180`)

| Caminho — arquivo::função | Onde nasce/reabre | Chama `motivo_do_veto` **antes**? | Outro veto anterior |
|---|---|---|---|
| `app/chamados/models.py::ChamadoColaborador.abrir` — nascimento | `get_or_create` em `app/chamados/models.py:730` | **Não**, no corpo da porta. (`app/chamados/models.py:666`) | **Nascimento**, chamado em `app/chamados/models.py:726`. |
| `app/chamados/models.py::ChamadoColaborador.criar` | `objects.create` em `app/chamados/models.py:834` | **Não**, no corpo da porta. (`app/chamados/models.py:805`) | **Nascimento**, chamado em `app/chamados/models.py:822`. |
| `app/chamados/models.py::ChamadoColaborador.abrir` — dedup encerrado | `_renascer_por_premissa_viva` em `app/chamados/models.py:686` → `reabrir` em `app/chamados/models.py:770` → escrita em `app/chamados/models.py:1089` / `app/chamados/models.py:231` | **Não**, nessa cadeia. (`app/chamados/models.py:742`, `app/chamados/models.py:1023`) | Ativo, não cascata, competência não trancada; depois **Reabertura**. O ramo retorna antes do veto **Nascimento**. (`app/chamados/models.py:764`, `app/chamados/models.py:766`, `app/chamados/models.py:768`, `app/chamados/models.py:688`) |
| `app/chamados/models.py::ChamadoColaborador.reabrir` | `_gravar_estado` em `app/chamados/models.py:1089` → `save` em `app/chamados/models.py:231` | **Não**. (`app/chamados/models.py:1023`) | **Reabertura**, em `app/chamados/models.py:1050`. |
| `app/chamados/models.py::ChamadoColaborador.transicionar` — destino vivo | `_aplicar` em `app/chamados/models.py:183` → `_gravar_estado` em `app/chamados/models.py:260` → `save` em `app/chamados/models.py:231` | **Não**. (`app/chamados/models.py:122`) | Matriz/perfil do motor. As guardas de justificativa, disputa e premissa são para fechamento/resolução; essa direção não passa por **Reabertura**. (`app/chamados/models.py:126`, `app/chamados/models.py:129`, `app/chamados/models.py:142`, `app/chamados/models.py:165`) |
| `app/ponto/management/commands/detectar_ausencias.py::Command.handle → _abrir_chamado` | `handle` chama em `app/ponto/management/commands/detectar_ausencias.py:439`; `_abrir_chamado` chama `abrir` em `app/ponto/management/commands/detectar_ausencias.py:688` | **Não** na cadeia própria do detector. (`app/ponto/management/commands/detectar_ausencias.py:570`) | Régua degenerada e suspeita diretamente; feriado; posto fechado; **Célula**; existência de célula, com exceção de limbo. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/detectar_ausencias.py:604`, `app/ponto/management/commands/detectar_ausencias.py:616`, `app/ponto/management/commands/detectar_ausencias.py:623`, `app/ponto/management/commands/detectar_ausencias.py:626`, `app/ponto/management/commands/detectar_ausencias.py:654`) |
| `app/ponto/management/commands/emitir_furo_retroativo.py::Command.handle` | Alias `_Emissor` importado em `app/ponto/management/commands/emitir_furo_retroativo.py:38`; chama `_abrir_chamado` em `app/ponto/management/commands/emitir_furo_retroativo.py:142` → detector `:688` acima | **Não** nessa cadeia. (`app/ponto/management/commands/emitir_furo_retroativo.py:142`) | Herda os vetos do detector; antes exige classificação cobravel e dia de trabalho. (`app/ponto/management/commands/emitir_furo_retroativo.py:109`, `app/ponto/management/commands/emitir_furo_retroativo.py:127`) |
| `app/ponto/management/commands/reconciliar_grade.py::Command.handle → _emitir_furo` | Chamada em `app/ponto/management/commands/reconciliar_grade.py:254`; `abrir` em `app/ponto/management/commands/reconciliar_grade.py:499`; reabertura explícita adicional em `app/ponto/management/commands/reconciliar_grade.py:520` | **Não**. (`app/ponto/management/commands/reconciliar_grade.py:480`) | Régua degenerada e suspeita diretamente; **Célula**; eventual reabertura herda **Reabertura**. (`app/ponto/management/commands/reconciliar_grade.py:487`, `app/ponto/management/commands/reconciliar_grade.py:491`, `app/ponto/management/commands/reconciliar_grade.py:494`, `app/ponto/management/commands/reconciliar_grade.py:520`) |
| `app/ponto/services/cartorio.py::_julgar_colab_corpo` — integral, cobrança morta e limbo de hoje | Delega ao detector em `app/ponto/services/cartorio.py:1358` → `abrir` em `app/ponto/management/commands/detectar_ausencias.py:688` | **Sim:** impressão em `app/ponto/services/cartorio.py:1083` → `_veto_do_dia` → alias `_motivo_veto` em `app/ponto/services/cartorio.py:1013`, importado em `app/ponto/services/cartorio.py:1007`. | O resultado entra na impressão; o bloqueio de régua da emissão delegada ocorre no detector. Há também piso, vivo existente, desligamento e afastamento. (`app/ponto/services/cartorio.py:1319`, `app/ponto/services/cartorio.py:1327`, `app/ponto/services/cartorio.py:1339`, `app/ponto/services/cartorio.py:1344`, `app/ponto/management/commands/detectar_ausencias.py:600`) |
| `app/ponto/services/cartorio.py::_julgar_colab_corpo` — furo parcial | Delega ao detector em `app/ponto/services/cartorio.py:1394` → `abrir` em `app/ponto/management/commands/detectar_ausencias.py:688` | **Sim**, pela mesma cadeia `app/ponto/services/cartorio.py:1083` → `app/ponto/services/cartorio.py:1013`. | Piso, desligamento, afastamento, ausência de chamado vivo; depois vetos do detector. (`app/ponto/services/cartorio.py:1381`, `app/ponto/services/cartorio.py:1386`, `app/ponto/services/cartorio.py:1393`) |
| `app/chamados/reconciliador.py::reemitir_chamado_do_dia` — c2 | `abrir` em `app/chamados/reconciliador.py:862` | **Sim, condicionado a `not _costura_no_vivo`:** alias `_mv_c2` importado em `app/chamados/reconciliador.py:853`, chamado em `app/chamados/reconciliador.py:854`; veto retorna antes de abrir em `app/chamados/reconciliador.py:855`. | Ativo, horizonte, isenção, ausência, protesto cobravel, **Célula** e motor. (`app/chamados/reconciliador.py:789`, `app/chamados/reconciliador.py:809`, `app/chamados/reconciliador.py:819`, `app/chamados/reconciliador.py:821`, `app/chamados/reconciliador.py:836`, `app/chamados/reconciliador.py:838`, `app/chamados/reconciliador.py:858`) |
| `app/ponto/management/commands/processar_alertas_turno.py::Command.handle → _marco_critico → _abrir_chamado_critico` | `app/ponto/management/commands/processar_alertas_turno.py:141` → `app/ponto/management/commands/processar_alertas_turno.py:316` → `abrir` em `app/ponto/management/commands/processar_alertas_turno.py:335` | **Não**. (`app/ponto/management/commands/processar_alertas_turno.py:324`) | Isenção, ausência, hora extra permitida; depois **Nascimento** ou **Reabertura**, conforme o ramo de `abrir`. (`app/ponto/management/commands/processar_alertas_turno.py:85`, `app/ponto/management/commands/processar_alertas_turno.py:97`, `app/ponto/management/commands/processar_alertas_turno.py:135`) |
| `app/ponto/management/commands/processar_alertas_avancados.py::Command.handle` | `criar`, módulo `turno_aberto_24h`, em `app/ponto/management/commands/processar_alertas_avancados.py:121` | **Não**. (`app/ponto/management/commands/processar_alertas_avancados.py:121`) | Isenção, dia de trabalho, turno aberto, ausência e **Nascimento**. (`app/ponto/management/commands/processar_alertas_avancados.py:32`, `app/ponto/management/commands/processar_alertas_avancados.py:49`, `app/ponto/management/commands/processar_alertas_avancados.py:63`, `app/ponto/management/commands/processar_alertas_avancados.py:116`) |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py::Command.handle` | `abrir`, módulo `turno_aberto_24h`, em `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:146` | **Não**. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:146`) | Turno aberto, tempo mínimo, vínculo, ausência e dedup; depois portas do modelo. O dry consulta `celula_absolveria`. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:92`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:101`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:105`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:108`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:134`) |
| `app/ponto/views.py::bater_ponto` | Alias `_Chamado.criar` em `app/ponto/views.py:291`, módulo `entrada_bloqueada_turno_aberto` em `app/ponto/views.py:299` | **Não** na cadeia específica desse nascimento. (`app/ponto/views.py:275`) | Condição de bloqueio/regularização e dedup de vivos. O contexto passado contém apenas `pendentes` e `total`, sem dia/célula; **Nascimento** depende de célula vinculada. (`app/ponto/views.py:275`, `app/ponto/views.py:280`, `app/ponto/views.py:281`, `app/chamados/models.py:849`) |
| `app/chamados/views.py::abrir_chamado → app/chamados/services/abertura.py::abrir_chamado_local` | View chama em `app/chamados/views.py:197`; serviço chama `criar` em `app/chamados/services/abertura.py:47` | **Não**. (`app/chamados/services/abertura.py:19`) | A view aceita módulo presente em `MOTIVOS`, incluindo os de furo. Herda **Nascimento**, mas o contexto desse serviço não fornece `data_turno`. (`app/chamados/views.py:170`, `app/chamados/views.py:174`, `app/chamados/catalogo/modulos.py:105`, `app/chamados/services/abertura.py:58`) |

Os dois emissores que consultam régua diretamente usam o mesmo predicado de degeneração e a mesma função de suspeita de `motivo_do_veto`. Há diferença com `ec=None`: `motivo_do_veto` não veta; `vetar_cobranca_se_degenerada` trata a escala ausente como degenerada. (`app/ponto/services/furos_vetados.py:26`, `app/ponto/services/furos_vetados.py:28`, `app/ponto/services/furos_vetados.py:30`, `app/escala/regua_defesa.py:23`, `app/escala/regua_defesa.py:45`, `app/escala/regua_defesa.py:46`)

As reaberturas adicionais são estas; as linhas genéricas abaixo operam sobre o chamado selecionado, sem restringir a operação aos módulos de disputa. (`app/chamados/services/acoes_chamado.py:315`, `app/chamados/services/fio.py:705`, `app/chamados/views_arquivar.py:58`, `app/chamados/services/competencia_trancada.py:252`)

| Caminho — arquivo::função | Onde reabre | `motivo_do_veto` antes? | Outro veto anterior |
|---|---|---|---|
| `app/chamados/views.py::responder_chamado_colaborador → app/chamados/services/resposta_tardia.py::receber` | Alias `_rt.receber` em `app/chamados/views.py:1053` → `_renascer_por_premissa_viva` em `app/chamados/services/resposta_tardia.py:45` | **Não**. (`app/chamados/services/resposta_tardia.py:31`) | Exige célula em `VEREDITOS_ACUSAM`; depois ativo/cascata/tranca e **Reabertura**. (`app/chamados/services/resposta_tardia.py:44`, `app/chamados/services/resposta_tardia.py:45`, `app/chamados/models.py:764`, `app/chamados/models.py:770`) |
| `app/chamados/views.py::detalhe_chamado_local → app/chamados/services/acoes_chamado.py::acao_resposta_livre` | Delegação em `app/chamados/views.py:547` → `transicionar` em `app/chamados/services/acoes_chamado.py:315` | **Não**. (`app/chamados/services/acoes_chamado.py:283`) | Gestor, status válido e motor; sem veto equivalente de premissa para destino vivo. (`app/chamados/services/acoes_chamado.py:313`, `app/chamados/models.py:126`, `app/chamados/models.py:165`) |
| `app/chamados/views.py::fio_colaborador/modal_fio_colaborador → app/chamados/services/fio.py::processar_post_fio` | Delegações em `app/chamados/views.py:674` e `app/chamados/views.py:954` → `transicionar` em `app/chamados/services/fio.py:715` | **Não**. (`app/chamados/services/fio.py:702`) | Autorização por área, status válido e motor; sem passar por **Reabertura**. (`app/chamados/services/fio.py:708`, `app/chamados/services/fio.py:713`, `app/chamados/models.py:183`) |
| `app/chamados/views_arquivar.py::desarquivar_chamado` | `reabrir` em `app/chamados/views_arquivar.py:68` | **Não**. (`app/chamados/views_arquivar.py:52`) | Permissão/área, via de competência trancada e **Reabertura**. (`app/chamados/views_arquivar.py:54`, `app/chamados/views_arquivar.py:60`, `app/chamados/views_arquivar.py:64`) |
| `app/ponto/services/fechamento.py::reabrir_periodo → app/chamados/services/competencia_trancada.py::reabrir` | Alias `_reabrir_fila` em `app/ponto/services/fechamento.py:1415`, chamado em `app/ponto/services/fechamento.py:1416` → `ch.reabrir` em `app/chamados/services/competencia_trancada.py:258` | **Não**. (`app/chamados/services/competencia_trancada.py:228`) | Via de encerramento, dia dentro da competência e **Reabertura**. (`app/chamados/services/competencia_trancada.py:252`, `app/chamados/services/competencia_trancada.py:255`) |
| `app/chamados/services/fio.py::processar_post_fio → app/chamados/services/acoes_disputa.py::acao_reabrir_disputa_admin` | Delegação em `app/chamados/services/fio.py:577` → `ch.reabrir` em `app/chamados/services/acoes_disputa.py:266` | **Não**. (`app/chamados/services/acoes_disputa.py:245`) | Motivo/justificativa, disputa fechada, chamado encerrado e **Reabertura**. (`app/chamados/services/acoes_disputa.py:248`, `app/chamados/services/acoes_disputa.py:250`, `app/chamados/services/acoes_disputa.py:252`, `app/chamados/services/acoes_disputa.py:265`) |
| `app/chamados/services/disputa_emissao.py::resolver_perguntas_por_chamado_resolvido` | `_pai.reabrir` em `app/chamados/services/disputa_emissao.py:1606` | **Não** nessa cadeia. (`app/chamados/services/disputa_emissao.py:1596`) | Disputa aberta, pai não vivo, irmãs respondidas sem consumo e **Reabertura**. (`app/chamados/services/disputa_emissao.py:1599`, `app/chamados/services/disputa_emissao.py:1602`, `app/chamados/services/disputa_emissao.py:1604`) |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py::Command.handle` | `pai.reabrir` em `app/chamados/management/commands/reabrir_orfas_enterradas.py:161` | **Não**. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:105`) | Consulta antecipada à própria recusa da reabertura e às outras perguntas que ressuscitariam; depois **Reabertura**. O alvo é o pai da disputa. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:96`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:105`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:106`) |
| `app/chamados/migrations/0038_a3_registrado_backfill.py::desfazer` | `update(status_local='aberto')` em `app/chamados/migrations/0038_a3_registrado_backfill.py:15` | **Não**. (`app/chamados/migrations/0038_a3_registrado_backfill.py:13`) | Nenhum veto de régua/célula/premissa. Seleciona todos os `registrado`, sem filtro de módulo. É vivo → vivo, pois `registrado` integra `VIVOS`. (`app/chamados/migrations/0038_a3_registrado_backfill.py:15`, `app/chamados/catalogo/motor.py:24`) |

A guarda **Reabertura** consulta premissa regularizada, mas não chama `motivo_do_veto`: lê justificativa aprovada, célula/veredito administrativo, marcos acesos ou inexistentes, `_celula_furada` e os fatos de fallback. (`app/chamados/models.py:937`, `app/chamados/models.py:951`, `app/chamados/models.py:959`, `app/chamados/models.py:968`, `app/chamados/models.py:970`, `app/chamados/models.py:975`, `app/chamados/models.py:985`)

Em `disputa_emissao`, há duas escritas distintas: o chamado-pai da disputa e, pelo c2, o chamado de furo do dia. O veto do segundo não antecede retroativamente a criação do primeiro. (`app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:606`, `app/chamados/services/disputa_emissao.py:409`, `app/chamados/reconciliador.py:862`)

| Caminho — arquivo::função | Nascimento | `motivo_do_veto` antes? | Outro veto anterior |
|---|---|---|---|
| `app/chamados/services/disputa_emissao.py::_abrir_ou_agregar_disputa` | `abrir` em `app/chamados/services/disputa_emissao.py:504`; módulo `disputa_supervisao` em `app/chamados/services/disputa_emissao.py:507` | **Não** no núcleo. (`app/chamados/services/disputa_emissao.py:419`) | Dedup/agregação; esse módulo não pertence a `MODULOS_DE_TURNO`, portanto **Nascimento** não o barra por célula. (`app/chamados/services/disputa_emissao.py:502`, `app/chamados/reconciliador.py:336`, `app/chamados/models.py:849`) |
| `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos` | `abrir` em `app/chamados/services/disputa_emissao.py:908`; módulo manual em `app/chamados/services/disputa_emissao.py:911` | **Não** no agrupador. (`app/chamados/services/disputa_emissao.py:847`) | Portas do modelo; módulo fora de `MODULOS_DE_TURNO`. (`app/chamados/services/disputa_emissao.py:911`, `app/chamados/models.py:849`) |
| `app/chamados/services/disputa_emissao.py::criar_disputa_ancora_de_chamado` | `abrir` em `app/chamados/services/disputa_emissao.py:1128`; módulo manual em `app/chamados/services/disputa_emissao.py:1131` | **Não**. (`app/chamados/services/disputa_emissao.py:1093`) | Recusa chamado-limbo encerrado; módulo fora do veto **Nascimento**. (`app/chamados/services/disputa_emissao.py:1109`, `app/chamados/models.py:849`) |
| `app/chamados/views.py::_abrir_para_colaborador → app/chamados/services/abertura.py::abrir_chamado_para_colaborador` | Serviço chamado em `app/chamados/views.py:136`; `criar` em `app/chamados/services/abertura.py:172` | **Não**. (`app/chamados/services/abertura.py:122`) | Tipos permitidos e exigência de dia. A porta permite `disput

[truncado em 20000 de 31032 caracteres]

## 30/10 07:45 · sessao 526619754147 · - · - · origem=Ronald (tmux)

Consolidado da varredura global, para unir às portas de `models.py` e aos emissores centrais.

`R` abaixo = `reabrir` → `recusa_da_reabertura` (`app/chamados/models.py:1050`) → vetos por fechamento humano sem fato novo (`:1003`), premissa regularizada de chamado de furo (`:1009`), agrupamento (`:1012`), irmão vivo (`:1015`) e transição do motor (`:1019`) → `_gravar_estado` (`:1089`) → `save` (`:231`). Essa cadeia não chama `motivo_do_veto`; a premissa consulta justificativa aprovada (`:937`), célula/veredito humano (`:951`), marcos acesos/inexistentes (`:959`, `:968`), `_celula_furada` (`:970`), feriado (`:976`) e premissa por fatos (`:985`). Não é o mesmo veto de régua de `furos_vetados`.

| Caminho | Nascimento/reabertura | `motivo_do_veto` antes? | Outros vetos comprovados |
|---|---|---|---|
| `app/ponto/views.py::bater_ponto` | `_Chamado.criar` em `:291`, módulo `entrada_bloqueada_turno_aberto` em `:299` | Não | Só condições de entrada, bloqueio por turno e ausência de regularização pendente (`:275`), dedup de vivos (`:281`). Contexto contém apenas `pendentes/total` (`:280`): não informa dia/célula à porta. |
| `app/ponto/management/commands/processar_alertas_turno.py::_abrir_chamado_critico` | `ChamadoColaborador.abrir` em `:335`, módulo `turno_aberto` (`:337`) | Não | No `handle`: isenção (`:85`), ausência (`:97`), vínculo quando necessário (`:106`), HE permitida (`:135`). Cadeia `handle :141` → `_marco_critico :316` → `_abrir_chamado_critico :335`; herda veto de célula do `abrir`. |
| `app/ponto/management/commands/processar_alertas_avancados.py::Command.handle` | `.criar` em `:121`, módulo `turno_aberto_24h` (`:123`) | Não | Isenção (`:32`), vínculo (`:37`), dia de trabalho (`:49`), turno efetivamente aberto (`:63`), ausência (`:116`); herda veto de célula do `criar`. |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py::Command.handle` | `.abrir` em `:146`, módulo `turno_aberto_24h` (`:147`) | Não | Turno aberto (`:92`), 24h (`:101`), vínculo (`:104`), ausência (`:108`), dedup (`:118`); herda veto de célula do `abrir`. Dry pergunta explicitamente `celula_absolveria` em `:134`. |
| `app/chamados/services/resposta_tardia.py::receber` | `_renascer_por_premissa_viva()` em `:45` → `models.py:770` → R | Não | Exige célula com `VEREDITOS_ACUSAM` (`:44`, `:45`); renascer exige ativo (`models.py:764`), não cascade (`:766`), competência não trancada (`:768`), depois R. |
| `app/chamados/services/acoes_chamado.py::acao_resposta_livre` | `.transicionar(novo_status)` em `:315` → `models.py:183` → `_aplicar :260` → `_gravar_estado :231` | Não | Para destino vivo: matriz e perfil do motor (`models.py:126`; `catalogo/motor.py:139`). Não passa por R nem sua premissa. |
| `app/chamados/services/fio.py::processar_post_fio` | `.transicionar` em `:715` → mesma cadeia humana acima | Não | Autorização por área em `:708`; matriz/perfil do motor. Não passa por R nem sua premissa. |
| `app/chamados/views_arquivar.py::desarquivar_chamado` | `.reabrir()` em `:68` → R | Não | Permissão (`:54`), área (`:60`), somente via de competência trancada entra nessa reabertura (`:64`), depois R. |
| `app/chamados/services/competencia_trancada.py::reabrir` | `.reabrir()` em `:258` → R | Não | Seleção por via de encerramento (`:252`) e dia da competência (`:254`); depois R. |
| `app/chamados/services/acoes_disputa.py::acao_reabrir_disputa_admin` | `.reabrir()` em `:266` → R | Não | Admin (`:242`), motivo e justificativa (`:248`, `:250`), disputa fechada (`:252`), `reabrir_admin` (`:256`), chamado encerrado (`:265`), depois R. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py::Command.handle` | `pai.reabrir()` em `:161` → R | Não | Lâmpada apagada e pergunta viva (`:82`), janela (`:87`), competência não exportada (`:98`), pré-consulta de R (`:105`) e ausência de irmãs ressuscitáveis (`:106`); depois R. |

Entradas das reaberturas genéricas:

- Resposta livre: `app/chamados/views.py::detalhe_chamado_local` (`:359`) → `acao_resposta_livre` (`:547`) → transição (`app/chamados/services/acoes_chamado.py:315`).
- Fio: `app/chamados/views.py::fio_colaborador` (`:659`) → `processar_post_fio` (`:674`); `modal_fio_colaborador` (`:869`) → mesma função (`:954`) → transição (`app/chamados/services/fio.py:715`).
- Reabertura administrativa: esses dois fios → `app/chamados/services/fio.py:577` → `acao_reabrir_disputa_admin` → chamado (`app/chamados/services/acoes_disputa.py:266`).
- Resposta tardia: `app/chamados/views.py::responder_chamado_colaborador` (`:1041`) → alias `_rt.receber` (`:1053`) → renascimento (`app/chamados/services/resposta_tardia.py:45`).
- Competência: `app/ponto/views.py::reabrir_periodo` (`:1521`) → alias `_fech.reabrir_periodo` (`:1544`) → `app/ponto/services/fechamento.py:1416`, alias `_reabrir_fila` importado em `:1415` → `app/chamados/services/competencia_trancada.py:258`.

Atenção à migration: `app/chamados/migrations/0038_a3_registrado_backfill.py::desfazer` escreve diretamente `registrado → aberto` em `:15`, **sem filtro de módulo**, sem `motivo_do_veto` e sem portas do modelo. O caminho de ida restringe `AUDITORIA` (`:3`, `:10`); o reverso não restringe (`:15`). Portanto não se pode excluir seu reverso apenas porque a ida é de auditoria.

Varredura AST, sem executar aplicação, de `app/`, `mensageria/`, `bin/`, excluindo `tests/` e arquivos `test*`: a única atribuição de atributo `status_local` encontrada foi `app/chamados/models.py:192`; chamadas ORM com esse keyword foram `models.py:834`, migration `0038:10/:15` e `0039:24/:33`. Não apareceu `bulk_update` com `status_local` nem `bulk_create` de ChamadoColaborador. A ausência global é resultado da varredura [não verificável por referência única de arquivo:linha].

Wrappers do cartório que podem chegar aos emissores, herdando suas guardas:

| Entrada | Chamada ao cartório |
|---|---|
| `app/ponto/management/commands/processar_cartorio.py::Command.handle` | `julgar_colab` em `:65` |
| `app/ponto/services/vigia_de_hora.py::rodar` | `julgar_celula` em `:130` |
| `app/chamados/services/cobrar_dia.py::cobrar_dia` | `julgar_celula(..., toque=True)` em `:29` |
| `app/ponto/services/furos_sem_cobranca.py::cobrar` | `julgar_celula` em `:128`; command `cobrar_furos_sem_cobranca.py:30` chama `cobrar` |
| `app/ponto/signals.py::_julgar_dias` | `cartorio.julgar_celula` em `:93`; recebe Batida save `:135`/delete `:140`, Ausencia save `:147`/delete `:152`, Justificativa save `:165` |
| `app/chamados/signals.py::_cartorio_post_save_chamado` | `cartorio.julgar_celula` em `:180` — **depois** do save de chamado (`:164`), portanto não conta como veto anterior desse nascimento/reabertura |
| `app/ponto/portas/celula.py::regenerar_celulas_vinculo` | `julgar_celula` em `:372` |
| `app/colaboradores/services/pos_vinculo.py::reconciliar_apos_vinculo` | `julgar_celula` em `:72` |
| `app/escala/regua_defesa.py::rejulgar_furos_do_vinculo` | `julgar_colab` em `:692` |
| `app/ponto/management/commands/celula_veredito_velho.py::Command.handle` | `julgar_colab` em `:144` |
| `app/ponto/management/commands/curar_residuo_bug115.py::Command._lavrar` | `julgar_celula` em `:143` |
| `app/ponto/management/commands/rejulgar_ausencia_em_folga.py::Command.handle` | `julgar_celula` em `:71` |
| `bin/rejulgar_o118_col221.py::<módulo>` | `julgar_colab` em `:24` |
| `bin/r5_idempotencia_frota.py::rodada` | `call_command('processar_cartorio', ..., apply=True)` em `:109` |

Fora do universo solicitado: `adesao.py` reabre apenas pai `sem_previsao_escala` (`app/chamados/services/adesao.py:75`, `:92`); `reabrir_chamados_ausencia_aberta` seleciona `ausencia_colab` (`app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:29`, `:49`). `detectar_intervalo_ausente` atualmente só reconcilia e encerra o laço antes de qualquer emissão (`app/ponto/management/commands/detectar_intervalo_ausente.py:134`, `:152`); não há nascimento ali.

Módulo adjacente, caso o censo inclua também alarmes de batidas espúrias: `app/ponto/services/cluster_espurio.py::reconciliar_colab` chama `ChamadoColaborador.abrir` em `:84`, módulo `cluster_batida_espuria` (`:11`, `:86`), sem `motivo_do_veto`; exige ativo (`:73`), escala ativa (`:75`), célula trabalha e sem `sem_ancora` (`:24`), cluster e marco apagado (`:27`, `:30`). Esse módulo não é um dos quatro de `furo_modulos.py:15`, nem um `MODULOS_DE_TURNO`.

## 30/10 07:45 · sessao 0fb2da7ae025 · O261-V · leitura · origem=Code

**O código demonstra o escritor e a derivação do instante; o anexo não demonstra o vínculo criador de cada B# com uma P#.** Esse vínculo é gravado em `pergunta_origem`, campo ausente da sonda fornecida. ([materializacao.py:891](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/materializacao.py:891))

**1. Escritor e escolha do dia**

O escritor da resposta de hora é `_materializar_resposta_hora`: deriva o instante em `materializacao.py:786` e chama `registrar_batida` com `origem='disputa_s84_retro'` em `:872–879`. O `INSERT` ocorre em `registro_batida.py:157–159`. ([materializacao.py:786](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/materializacao.py:786), [materializacao.py:872](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/materializacao.py:872), [registro_batida.py:157](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/registro_batida.py:157))

A derivação segue esta sequência:

- Usa `p.data_evento`; na ausência, `disputa.contexto[p.motivo]['data']`. ([materializacao.py:328](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/materializacao.py:328))
- Extrai a hora de `resposta_colab`, combina inicialmente **data-base + 07:00** e chama `_ajustar_saida_overnight`. ([materializacao.py:391](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/materializacao.py:391), [materializacao.py:398](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/materializacao.py:398), [materializacao.py:404](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/materializacao.py:404))
- A cadeia `19:00 → 07:00` avança o segundo marco para D+1: **a virada está em `servico_jornada.py:82–83`**. ([servico_jornada.py:82](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/escala/servico_jornada.py:82))
- O ajuste monta o envelope **D 17:30 até D+1 08:30**. **A escolha final do dia está em `juizes.py:308–311`**: testa D e D+1, retornando o primeiro candidato dentro do envelope. D 07:00 fica fora; D+1 07:00 fica dentro. ([juizes.py:305](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/juizes.py:305), [juizes.py:308](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/juizes.py:308))

Os IDs e horários abaixo são os apresentados no anexo; a correspondência criadora permanece sem comprovação:

| Batida apresentada | Pergunta/resposta apresentada na célula | Vínculo criador |
|---|---|---|
| B#140648 — 20/10 07:00 | Pergunta correspondente não fornecida | [nao verificado] |
| B#140649 — 22/10 07:00 | P#46704 — `07:00`, célula 21/10 | [nao verificado] |
| B#140652 — 24/10 07:00 | P#49423 — `07:00`, célula 23/10 | [nao verificado] |
| B#140653 — 26/10 07:00 | P#50180 — `07:00`, célula 25/10 | [nao verificado] |
| B#140654 — 28/10 07:00 | P#51025 — `07:00`, célula 27/10 | [nao verificado] |

B#140650 e B#140651 não aparecem no dado fornecido. [nao verificado]

**2. Qual órfã a pergunta escolhe**

**Nesse caminho, a fábrica não escolhe uma batida órfã por B# ou instante.** Ela lê os marcos faltantes da ata e converte posição/tipo em motivo; `('fim', 'S')` vira `orfao_14h`. ([fabrica_da_celula.py:48](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:48), [fabrica_da_celula.py:59](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:59), [declaracao_texto.py:44](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/declaracao_texto.py:44))

A seleção percorre os slots da ata, exige `missing`, identifica o último como `fim` e devolve `(dia, posição, tipo, hora)`. Esse trecho não seleciona elementos de `orfas`. ([declaracao_texto.py:97](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/declaracao_texto.py:97), [declaracao_texto.py:106](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/declaracao_texto.py:106))

O texto canônico é **“Confirme o horário da saída no dia {data}”**. A fábrica fornece o dia da célula no contexto; o formatador interpola esse dia no texto. ([perguntas.py:49](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/catalogo/perguntas.py:49), [fabrica_da_celula.py:122](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:122), [query.py:58](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/catalogo/query.py:58))

Portanto, atribuir essas perguntas especificamente às batidas das **06:55:11, 06:52:47, 06:58:07 ou 06:46:01** não está demonstrado: faltam o texto gravado, `data_evento` e a ata das perguntas históricas. [nao verificado]

**3. O instante plantado corresponde ao descrito?**

**Há divergência entre o dia civil expresso pelo texto canônico e a interpretação noturna do plantio.** A fábrica escreve “saída no dia D”; a resposta `07:00`, interpretada nesse texto, descreve D 07:00. O ajuste planta D+1 07:00 para o turno D 19:00 → D+1 07:00. A mudança acontece em **`juizes.py:308–311`**; o dia D do texto vem de **`fabrica_da_celula.py:122`**. ([perguntas.py:49](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/catalogo/perguntas.py:49), [fabrica_da_celula.py:122](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:122), [juizes.py:308](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/juizes.py:308))

Que esse tenha sido o texto efetivamente mostrado nas quatro P# históricas: [nao verificado].

**4. Por que `21/10=[]` e os outros dias recebem uma hora**

`InsumosDoDono` lê a janela **[ini − 12h, fim + 1 dia + 14h]**, converte os timestamps para horário local e chama `minutos_do_oraculo(ts)` sem pares ou envelopes. `batidas_apuraveis` filtra por timestamp e retratação; não exclui `disputa_s84_retro` pela origem. ([e6_oraculo.py:124](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/management/commands/e6_oraculo.py:124), [turnos.py:2838](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/turnos.py:2838))

Nesse ramo, o núcleo corta quando há contagem par e gap ≥8h, gap >14h ou vão total >14h. **Todas as horas do grupo são atribuídas à data da primeira batida**, em `nucleo.py:237–238`. ([nucleo.py:215](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/calculador/nucleo.py:215), [nucleo.py:237](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/calculador/nucleo.py:237))

Executei essa função da cópia sobre os timestamps fornecidos, com recorte inferior em **20/10 12:00**. O resultado reproduz a assimetria: [nao verificado]

| Trecho alimentado | Resultado da regra |
|---|---|
| 20/10 18:53 → 21/10 06:55 | Duas batidas no grupo iniciado em **20/10**; `21/10=[]` |
| 22/10 07:00 plantada → 22/10 19:01 | Fecha contagem par; o gap seguinte ≥8h separa 23/10 06:52 |
| 24/10 07:00 plantada → 24/10 19:11 | Mesmo corte; 25/10 recebe somente `06:58` |
| 26/10 07:00 plantada → 26/10 19:03 | Mesmo corte; 27/10 recebe somente `06:46` |

Essa reprodução decorre das condições de corte e da atribuição à primeira data. ([nucleo.py:216](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/calculador/nucleo.py:216), [nucleo.py:237](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/calculador/nucleo.py:237))

Incluindo também **B#140648 de 20/10 07:00**, a mesma função devolve `21/10=['06:55']`: o grupo anterior já contém duas batidas antes da manhã de 21/10, e ocorre o corte. Logo, a reprodução distingue precisamente o efeito da borda sobre a primeira plantada. ([nucleo.py:216](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/calculador/nucleo.py:216))

**Os parâmetros `ini/fim` efetivamente usados pela sonda não foram apresentados; afirmar que ela usou esse recorte exato permanece [nao verificado].**

Para o dono, uma hora satisfaz a condição de contagem ímpar; zero horas não satisfaz. O código também consulta batida fora de turno antes da paridade e, depois, CxR. ([e6_oraculo.py:96](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/management/commands/e6_oraculo.py:96))

**5. Por que não aparecem em `dias_cadastro_x_realidade`**

A regra exige **duas distâncias existentes, ambas com valor absoluto >180 minutos**; o operador é `AND`. As distâncias comparam instantes completos, incluindo a data. A inclusão na lista acontece somente após esse predicado. ([motor_calculo_v2.py:1885](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/motor_calculo_v2.py:1885), [motor_calculo_v2.py:1911](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/motor_calculo_v2.py:1911), [motor_calculo_v2.py:1926](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/motor_calculo_v2.py:1926))

**Os grupos cronológicos do oráculo não são os pares do motor.** O Motor12x36 delega o pareamento à autoridade; uma saída órfã sem entrada é descartada dos períodos pelo adaptador. Um período aberto retorna antes do julgamento de pontualidade. ([motor_calculo_v2.py:2686](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/motor_calculo_v2.py:2686), [adaptador_motor.py:30](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/adaptador_motor.py:30), [motor_calculo_v2.py:2690](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/motor_calculo_v2.py:2690))

A sonda informa ausência desses dias na lista, mas não fornece os períodos efetivamente julgados, os papéis da ata nem os dois deltas. **Qual condição impediu a inclusão de cada dia, individualmente: [nao verificado].** A presença de furo ou órfã, isoladamente, não satisfaz o predicado da L-084. ([motor_calculo_v2.py:1885](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/motor_calculo_v2.py:1885))

## 30/10 07:44 · sessao 089b01bfeed8 · - · - · origem=Ronald (tmux)

Portas de escrita verificadas:

| Porta | Escrita efetiva | `motivo_do_veto` antes? | Outro veto |
|---|---|---|---|
| `app/chamados/models.py::abrir` | `get_or_create`, `app/chamados/models.py:730` | Não. | `_celula_que_absolve`, `app/chamados/models.py:726`, chama `app/chamados/reconciliador.py::_absolvido_pela_celula`, `app/chamados/models.py:848`. Aplica somente a `MODULOS_DE_TURNO` com `celula_id`, `app/chamados/models.py:849`; rejeita nascimento sob veredito não vazio, fora de `VEREDITOS_ACUSAM`, sem marcos pendentes, `app/chamados/reconciliador.py:974`. |
| `app/chamados/models.py::criar` | `objects.create`, `app/chamados/models.py:834` | Não. | Mesma consulta `_celula_que_absolve`, `app/chamados/models.py:822`. |
| `app/chamados/models.py::abrir`, dedup encerrado | Chama `_renascer_por_premissa_viva`, `app/chamados/models.py:686`; chega a `_gravar_estado`, `app/chamados/models.py:1089`, e `save`, `app/chamados/models.py:231` | Não. | Colaborador ativo, cascata e competência trancada, `app/chamados/models.py:764`; depois as guardas de `reabrir`. |
| `app/chamados/models.py::reabrir` | `_gravar_estado`, `app/chamados/models.py:1089`; `save`, `app/chamados/models.py:231` | Não. | `recusa_da_reabertura`, `app/chamados/models.py:1050`: fato novo após fechamento humano, `app/chamados/models.py:1003`; `premissa_regularizada` de furo, `app/chamados/models.py:1009`; agrupado, irmão vivo e motor, `app/chamados/models.py:1012`. |
| `app/chamados/models.py::transicionar` com destino vivo | `_aplicar`, `app/chamados/models.py:183`; `_gravar_estado`, `app/chamados/models.py:260`; `save`, `app/chamados/models.py:231` | Não. | Sem veto de premissa de furo na direção viva: as guardas específicas são para destino `fechado` ou `resolvido`, `app/chamados/models.py:129`, `app/chamados/models.py:142`, `app/chamados/models.py:165`. O motor permite `resolvido → em_analise`, `app/chamados/catalogo/motor.py:102`, e exceção administrativa de transição, `app/chamados/catalogo/motor.py:150`. |

`premissa_regularizada` consulta justificativa aprovada, `app/chamados/models.py:937`; veredito administrativo, dia sem trabalho e marcos acesos/inexistentes, `app/chamados/models.py:950`; `_celula_furada`, `app/chamados/models.py:970`; feriado/folga, `app/chamados/models.py:975`; fatos da premissa, `app/chamados/models.py:978`.

Chamadores relevantes:

| Caminho | Ponto de abertura/reabertura | Veto anterior |
|---|---|---|
| `app/chamados/views.py::abrir_chamado → app/chamados/services/abertura.py::abrir_chamado_local` | View chama serviço em `app/chamados/views.py:197`; serviço chama `criar` em `app/chamados/services/abertura.py:47`; criação física em `app/chamados/models.py:834`. | Não chama `motivo_do_veto`; herda veto de `criar`. A view aceita qualquer chave de `MOTIVOS` em `app/chamados/views.py:173`; módulo vem do POST em `app/chamados/views.py:170`. O contexto do serviço não fornece `data_turno`, `app/chamados/services/abertura.py:58`. |
| `app/chamados/views.py::_abrir_para_colaborador → app/chamados/services/abertura.py::abrir_chamado_para_colaborador` | Serviço chamado em `app/chamados/views.py:136`; `criar` em `app/chamados/services/abertura.py:172`. | Não chama `motivo_do_veto`; herda veto de `criar`. Tipos permitidos são validados em `app/chamados/services/abertura.py:148`. `disputa_supervisao_manual` é abrível nessa porta, `app/chamados/catalogo/modulos.py:146`; os módulos `batida_ausente`, `turno_aberto`, `turno_aberto_24h`, `intervalo_pendente` não têm declaração de abertura humana, `app/chamados/catalogo/modulos.py:105`. |
| `app/chamados/services/disputa_emissao.py::_abrir_ou_agregar_disputa` | `ChamadoColaborador.abrir`, `app/chamados/services/disputa_emissao.py:504`; módulo `disputa_supervisao`, `app/chamados/services/disputa_emissao.py:507`. | Não chama `motivo_do_veto`; herda `abrir`. |
| `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos` | `abrir`, `app/chamados/services/disputa_emissao.py:908`; módulo `disputa_supervisao_manual`, `app/chamados/services/disputa_emissao.py:911`. | Não chama `motivo_do_veto`; herda `abrir`. |
| `app/chamados/services/disputa_emissao.py::criar_disputa_ancora_de_chamado` | `abrir`, `app/chamados/services/disputa_emissao.py:1128`; módulo manual, `app/chamados/services/disputa_emissao.py:1131`. | Não chama `motivo_do_veto`; herda `abrir`; rejeita chamado-limbo encerrado em `app/chamados/services/disputa_emissao.py:1109`. |
| `app/chamados/services/disputa_emissao.py::gerar_perguntas_disputa` | Pergunta recém-criada sem destino chama `reatar_pergunta_orfa`, `app/chamados/services/disputa_emissao.py:404`, `app/chamados/services/disputa_emissao.py:409`; a cadeia segue ao reconciliador. | O veto do ramo de emissão de chamado do dia deve ser atribuído à cadeia do reconciliador; os chamados-pais acima são escritas anteriores e distintas, `app/chamados/services/disputa_emissao.py:504`. |
| `app/chamados/services/disputa_emissao.py::resolver_perguntas_por_chamado_resolvido` | `_pai.reabrir`, `app/chamados/services/disputa_emissao.py:1606`. | Não chama `motivo_do_veto`; exige disputa aberta e resposta sem consumo creditada ao pai, `app/chamados/services/disputa_emissao.py:1599`, `app/chamados/services/disputa_emissao.py:1604`, além das guardas de `reabrir`. |
| `app/chamados/views_arquivar.py::desarquivar_chamado` | `ch.reabrir`, `app/chamados/views_arquivar.py:68`, quando via é competência trancada, `app/chamados/views_arquivar.py:64`. | Não chama `motivo_do_veto`; herda guardas de `reabrir`. |
| `app/chamados/services/competencia_trancada.py::reabrir` | `ch.reabrir`, `app/chamados/services/competencia_trancada.py:258`; seleciona chamados encerrados pela tranca, `app/chamados/services/competencia_trancada.py:252`. | Não chama `motivo_do_veto`; herda guardas de `reabrir`. É importado por apelido `_reabrir_fila` em `app/ponto/services/fechamento.py:1415`. |
| `app/chamados/services/acoes_disputa.py::acao_reabrir_disputa_admin` | `ch.reabrir`, `app/chamados/services/acoes_disputa.py:266`. | Não chama `motivo_do_veto`; herda guardas de `reabrir`. O fio delega à função em `app/chamados/services/fio.py:577`. |
| `app/chamados/views.py::responder_chamado_colaborador → app/chamados/services/resposta_tardia.py::receber` | View chama `receber`, `app/chamados/views.py:1053`; serviço chama `_renascer_por_premissa_viva`, `app/chamados/services/resposta_tardia.py:45`. | Não chama `motivo_do_veto`; exige célula com `VEREDITOS_ACUSAM`, `app/chamados/services/resposta_tardia.py:44`, e herda renascimento/reabertura. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py::Command.handle` | `pai.reabrir`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`. | Não chama `motivo_do_veto`; consulta previamente a própria `recusa_da_reabertura` e recusa ressuscitação de outras perguntas, `app/chamados/management/commands/reabrir_orfas_enterradas.py:105`. |
| `app/chamados/views.py::detalhe_chamado_local → app/chamados/services/acoes_chamado.py::acao_resposta_livre` | Delegação, `app/chamados/views.py:547`; transição humana, `app/chamados/services/acoes_chamado.py:315`. | Não chama `motivo_do_veto`; sem veto equivalente de premissa para destino vivo, conforme `transicionar` acima. |
| `app/chamados/views.py::fio_colaborador/modal_fio_colaborador → app/chamados/services/fio.py::processar_post_fio` | Delegações em `app/chamados/views.py:674` e `app/chamados/views.py:954`; transição humana em `app/chamados/services/fio.py:715`. | Não chama `motivo_do_veto`; mesma porta humana acima. |

Para evitar inclusão indevida no universo estrito: `api_abrir_chamado` grava módulo fixo `app_colaborador`, `app/api/views.py:2237`, `app/api/views.py:2246`; `api_justificativas` abre wrapper `justificativa_colab`, `app/api/views.py:2033`, `app/api/views.py:2036`; os aliases `_Ch9.criar` dos dois kernels gravam `batida_fora_escala_leve`, `app/api/views.py:746`, `app/api/views.py:754`, `app/api/views_core.py:746`, `app/api/views_core.py:754`. `reabrir_chamados_ausencia_aberta` seleciona somente `ausencia_colab`, `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:29`.

Os três pontos físicos distintos são `get_or_create`, `app/chamados/models.py:730`; `objects.create`, `app/chamados/models.py:834`; e `save` de estado, `app/chamados/models.py:231`. As reaberturas pela porta `reabrir` compartilham `_gravar_estado`, `app/chamados/models.py:1089`; as humanas também chegam a `_gravar_estado`, por `app/chamados/models.py:260`, mas não atravessam `reabrir` nem suas guardas.

_387 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
