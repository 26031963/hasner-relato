# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 18:56** · publicado em **06/11/2026 18:56** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 18:56 · sessao 9d4d73a5e5aa · BO-[nome]-0611 · sonda · origem=Code

Saída fornecida, sem reexecução da sonda. [nao verificado]

- **(a)** `TOTAL_RECUSAS 0 COLABS 0`: zero recusas, zero colaboradores. O recorte desde 07/10 não aparece na saída. [nao verificado]
- **(b)** `RECUSAS_SEM_BATIDA_EM_30MIN 0 COLABS 0`: zero candidatas a batida perdida, zero colaboradores. [nao verificado]
- **(c)** Não há série diária apresentada; existência de degrau e sua data: [nao verificado].
- **(d)** Não aparece linha de `col155`. [nao verificado]
- **(e)** `ERROS {}`: nenhum erro listado. [nao verificado]

**O caminho que escreve o log não é o único bloqueio por falta de GPS.** Na tela V2, coordenadas ausentes fazem `garantirRequisitos()` retornar `false`; o chamador encerra antes do envio à API. [app/templates/ponto/bater_ponto_v2.html:297](app/templates/ponto/bater_ponto_v2.html#L297), [app/templates/ponto/bater_ponto_v2.html:325](app/templates/ponto/bater_ponto_v2.html#L325).

Nas duas views, `if not latitude or not longitude` chama `registrar_log` com `batida_recusada_sem_gps` e retorna HTTP 403, código `gps_obrigatorio`. [app/api/views_core.py:440](app/api/views_core.py#L440), [app/api/views.py:449](app/api/views.py#L449). Se a auditoria lançar exceção, `registrar_log` captura e registra a falha; a view segue para o 403. [app/core/auditoria.py:25](app/core/auditoria.py#L25), [app/api/views_core.py:454](app/api/views_core.py#L454), [app/api/views.py:462](app/api/views.py#L462).

**A fila offline mantém a batida após 403 e reenvia na próxima sincronização.** Remove o item somente em resposta de sucesso ou HTTP 400; o 403 não executa essas exclusões. A sincronização lê novamente todos os pendentes e envia cada um. [app/static/pwa.js:130](app/static/pwa.js#L130), [app/static/pwa.js:153](app/static/pwa.js#L153), [app/static/pwa.js:158](app/static/pwa.js#L158), [app/static/pwa.js:166](app/static/pwa.js#L166). Há chamadas de sincronização no evento `online` e na inicialização, quando conectado. [app/static/pwa.js:320](app/static/pwa.js#L320), [app/static/pwa.js:328](app/static/pwa.js#L328).

## 06/11 18:54 · sessao 6c1044e8ea77 · BO-[nome]-0611 · sonda · origem=Code

**BO-[nome]-0611 — triagem somente leitura.** Os fatos das FICHAS/anexo estão identificados com **[nao verificado]**, pois foram fornecidos no texto, sem arquivo:linha acessível. As referências abaixo sustentam o comportamento da cópia; a separação de donos segue `CLAUDE.md:176`.

**01+04 — col39, 15/10: producao batida, dono BATIDA.** É o candidato de maior atividade, 20 contra 11. O atestado **virou Ausência 4879 e está aprovado** desde 16/10 16:36:56; há três marcações em 15/10, sem saída final, e a retenção informada é **`espelho_fato_em_ausencia`**, não “atestado que não chegou”. (FICHA 01+04 [nao verificado].)

O código retém fato em dia coberto por ausência; portanto, o conflito informado explica a retenção. A porta do documento é **Ausências**; o calendário também oferece **Resolver dia**, mas não se deve presumir que “Trabalhou” aceitará um dia já coberto por ausência. (`app/escala/models.py:1753`; `app/folha/export.py:1233`; `app/ponto/services/ausencia.py:637`; `app/ponto/services/veredito_celula.py:221`.)

**02 — col993 comparado com col1023: falta informacao.** Mantive col993, atividade 102 contra 91 e 19: a FICHA mostra várias correções materializadas em 03/11, com origem `disputa_s84_retro`. O que aparece parado é a volta de intervalo de **24/10, pergunta 53441**, ainda sem resposta, além das perguntas de 03/11 e 05/11; a Justificativa 3465 também está pendente, sem saída informada. (FICHAS 02a/02b [nao verificado].)

Os caminhos são diferentes:

- **Justificativa:** aprovar só gera saída se houver `saida_informada` e `batida_entrada`; pode ficar aprovada sem gerar batida. (`app/ponto/services/justificativa.py:137`; `app/ponto/services/justificativa.py:190`.)
- **RegularizacaoExterna:** exige confirmação do colaborador e produz origem `reg_externa`; ambas as FICHAS têm `regularizacoes=[]`. (`app/chamados/services/regularizacao_ext.py:36`; `app/chamados/services/regularizacao_ext.py:76`; FICHAS [nao verificado].)
- **PerguntaDisputa:** exige resposta para validar e materializar. Em intervalo fixo, o plantio pode usar a hora do marco, não a hora livre digitada. (`app/chamados/services/validacao.py:363`; `app/chamados/services/materializacao.py:404`.)

Col1023 mostra abonos em 03–04/11, uma batida retratada e marcações do app em 05–06/11; não identifica a suposta correção de horário que “chegou”. Falta o dia/hora ou ID da solicitação comparada para concluir onde **a correção alegada** parou. (FICHA 02b [nao verificado].)

**03 — lote: nosso com obra, dono ESTRUTURA; cobertura específica: sem obra.** A view executa o lote **sincronamente**, antes de devolver o resultado; o serviço percorre e valida as referências individualmente. Isso mantém trabalho longo na requisição, contrariando a orientação de job da infraestrutura. Enquadramento de padrão: **Event-driven architecture**, aplicado à retirada desse processamento da pilha HTTP. (`app/chamados/views_cobrar.py:379`; `app/chamados/services/validacao.py:680`; `CLAUDE.md:89`; `app/docs/GLOSSARIO.md:16`.)

O **Caddyfile não configura timeout específico para essa rota**. Os 30 segundos do `gthread` não constituem limite por requisição: a própria documentação da O284 distingue requisição longa de worker travado. Não há fundamento para afirmar “passou de 30 s, o usuário recebe erro”. (`Caddyfile:8`; `Caddyfile:10`; `app/gunicorn.conf.py:19`; `app/gunicorn.conf.py:21`; `app/docs/TICKETS.md:132`.)

O formulário HTMX não declara timeout próprio; o resultado aparece quando a resposta chega. Em erro HTTP ou de transporte há mensagens diferentes, mas **não foi identificado um X segundos para desistência da tela**. (`app/templates/chamados/partials/_lote_preview.html:21`; `app/templates/chamados/partials/_lote_preview.html:5`; `app/templates/core/_dialogo.html:61`; `app/templates/core/_dialogo.html:75`.)

Os 35 retornos anexados são 200, entre 10,435 e 122,373 s; **200 não prova validação integral**, pois o resultado também comporta recusadas e validadas sem gravação. A causa da subida a partir de 03/11 permanece desconhecida: faltam tamanho/referências dos lotes e perfil da execução. (Anexo `ui_lote.log` [nao verificado]; `app/templates/chamados/partials/_lote_preview.html:7`; `app/templates/chamados/partials/_lote_preview.html:9`.)

Não localizei obra específica que cubra esse endpoint. **O284 cobre PDF do espelho**, não validar chamados; atribuir sua cobertura a esta rota seria indevido. (`app/docs/TICKETS.md:132`; cobertura específica [nao verificado].)

**05 — col155: nosso com obra, dono ESTRUTURA; sem obra específica localizada.** É o maior candidato, atividade 23 contra 15; tem dois devices ativos e as marcações apresentadas chegaram com geolocalização. O anexo não identifica uma tentativa recusada nem sua versão efetiva do aplicativo. (FICHA 05 [nao verificado].)

**Sim, há bloqueio:** o helper do PWA retorna `null` quando não consegue GPS; a tela V2 interrompe o envio sem coordenadas; a API efetiva retorna **403 `gps_obrigatorio` antes do registro**, inclusive para envio pela fila offline. Isso contraria a regra interna de batida nunca barrada. (`app/static/pwa.js:289`; `app/templates/ponto/bater_ponto_v2.html:297`; `app/templates/ponto/bater_ponto_v2.html:325`; `app/static/pwa.js:153`; `app/api/views_core.py:440`; `CLAUDE.md:140`.)

Padrão quebrado: **Duplicated business logic / logic drift** — a tela antiga permite prosseguir sem GPS, enquanto V2/API barram. O bloqueio está provado no código; **uma batida individual perdida de col155 não está demonstrada**. (`app/docs/GLOSSARIO.md:23`; `app/templates/ponto/bater_ponto.html:507`; `app/api/views_core.py:454`; perda individual [nao verificado].)

**06 — col950: producao cadastro, dono CADASTRO.** O backlog o identifica expressamente como conta de teste; TESTE06 mostra devices iOS, seis batidas, **em comuns=true e em operação=false**. (`app/docs/BACKLOG.md:1797`; TESTE06 [nao verificado].)

“Comuns” exige situação ativa, empresa ativa e username de 11 dígitos; “operação” acrescenta rollout e vínculo vigente. **`situacao='desligado'` o retiraria dos dois universos**; `isento_ponto` não participa desses filtros. Qual condição adicional o exclui hoje da operação não aparece no anexo. (`app/colaboradores/queries.py:25`; `app/colaboradores/queries.py:65`; motivo específico [nao verificado].)

**07 — col836, 14/10: ja OK hoje quanto à exclusão da lista inicial.** O chamado **31151 existe e está resolvido via sistema desde 14/10 08:15:08**; não tem `superado_por` nem silenciamento, possui célula e pertence à empresa 2. (FICHA 07 [nao verificado].)

A lista pertinente é **Painel de gestão, `/chamados/`**: por padrão mostra cobrança, aberto/em análise; **Histórico** inclui resolvido/fechado. Esse status basta para excluir o registro da lista inicial; filtros efetivamente escolhidos e acesso concreto do usuário não foram fornecidos. (`app/chamados/urls.py:9`; `app/chamados/views.py:721`; `app/chamados/services/painel.py:298`; `app/chamados/services/painel.py:314`; filtros/acesso [nao verificado].)

O queryset citado não exclui automaticamente por célula, `superado_por` ou “empresa visível”; empresa, praça e colaborador entram por filtros explícitos. A pendência de escala indicada na célula é separada da visibilidade desse chamado resolvido. (`app/chamados/services/painel.py:96`; FICHA 07 [nao verificado].)

**08 — col617, 05–06/11: ja OK hoje quanto à alegação de autoaceite.** A ausência 5380 está rejeitada; a **5399 está aprovada**, cobrindo 05–06/11. Ambas foram registradas pelo colaborador, mas `aprov_proprio=false`: o dado não sustenta aprovação pelo próprio colaborador; tampouco mostra a data correta do documento ou a identidade do decisor. (FICHA 08 [nao verificado].)

Atestado com documento nasce **aguardando decisão**. A aprovação automática da declaração **`FALTA_DIA` não se estende ao atestado**: `ATESTADO` usa o estado inicial do serviço. A auditoria da tela distingue “Lançado por” de “Decidido por”. (`app/ponto/services/ausencia.py:779`; `app/chamados/services/materializacao.py:579`; `app/templates/ponto/partials/painel_ausencia.html:257`.)

**09 — col1000: ja OK hoje para os dias sem batida apresentados.** Em **26/10** há abono aprovado; em **31/10** o dia é folga e há decisões `admin_decidiu` registradas em 06/11. Isso explica dias sem marcação que não permanecem como falta pendente; o anexo não identifica o usuário que decidiu. (FICHA 09 [nao verificado].)

“Resolvido” pode ser o **status do chamado**, não uma afirmação de jornada completa; chamado filho agrupado também pode estar resolvido enquanto outra pendência continua. O calendário separa o vínculo do chamado da marca de decisão humana. (`app/chamados/models.py:772`; `app/templates/colaboradores/partials/_calendario_grade.html:108`; `app/templates/colaboradores/partials/_calendario_grade.html:109`.)

A alegação não descreve integralmente o estado atual: **27/10 tem perguntas pendentes; 29 e 30/10 têm chamados próprios em análise**, criados hoje. A tela exata originalmente chamada de “resolvido” permanece sem identificação. (FICHA 09 [nao verificado].)

**10 — col399: falta informacao para a cronologia da alteração alegada.** As batidas de 31/10 foram criadas às **06:20:44 e 18:22:42**; a ausência de troca de 24/10 foi criada em **04/11 16:13:01**. Hoje a célula de 31/10 informa `folga_trabalhada`, sem retenção; a de 24/10 informa ausência. (FICHA 10 [nao verificado].)

Esses registros antecedem o deploy O10-B documentado em **06/11 13:20**. Porém a FICHA não contém `editada_em`, `regenerada_em`, antes/depois nem trilha de mudança de tipo: **não permite datar quando as células/batidas teriam mudado ou virado espúrias**. (`app/docs/RELATO.md:17`; cronologia da alteração [nao verificado].)

O `git log main` solicitado não pôde ser consultado porque a cópia não tem repositório Git disponível. A O10-B existente escreve o par da troca pela porta da célula, mas seu código não prova aplicação ao caso. (`app/ponto/portas/celula.py:632`; histórico Git/aplicação [nao verificado].)

**11 — col921: ja OK hoje; regra de competência.** `resolver_dia=true` em **14, 15, 16, 18, 19 e 20/10**; falso nas folgas **17 e 24/10**, e também nos dias **21–23 e 25–28/10**, ainda irregulares. (FICHA 11+12 [nao verificado].)

O botão exige passado, status que pede decisão e **último recurso em competência já encerrada**; dentro da corrente, o caminho é chamado/pergunta. A mudança em 21/10 é compatível com a competência 21–20 apresentada na FICHA, não demonstra defeito. (`app/colaboradores/services/calendario.py:97`; `app/colaboradores/services/calendario.py:455`; `app/colaboradores/services/calendario.py:738`; `app/ponto/janelas.py:10`.)

**12 — col921: ja OK hoje quanto à recusa de falta em folga.** O veto recusa falta quando a autoridade da escala diz que não existe jornada prevista. As folgas mostradas são 17 e 24/10; as seis faltas aprovadas apresentadas recaem em dias de trabalho. (`app/ponto/precedencia.py:536`; FICHA 11+12 [nao verificado].)

**Não há lançamento específico de “abandono de posto em folga” demonstrado neste caso.** O comando de abandono provisório usa o próprio tipo `falta`; o modelo de Advertência tem “Ausência/Outro”, mas isso não comprova uma porta de tela para o lançamento pretendido nem autoriza tratar folga como falta. (`app/ponto/management/commands/marcar_abandono_provisorio.py:76`; `app/ponto/models.py:1164`; porta pretendida [nao verificado].)

**13+14 — col407: nosso com obra, dono ESTRUTURA.** Aqui há evidência contrária ao maior candidato: col114 apresenta caso diurno e perguntas de intervalo; **col407 corresponde à assinatura noturna relatada**. A pergunta **47210**, vinculada a 22/10, respondeu 07:00 em 22/10 e foi validada hoje às 14:45:08; gerou a batida **147824, saída em 23/10 07:00, origem `disputa_s84_retro`**, enquanto 21/10 continua com furo. (FICHAS 13+14 [nao verificado].)

Isso confirma a assinatura do aval informado: data da pergunta tratada como base do turno, com saída deslocada mais um dia. O plantio combina `data_evento` com a hora e passa pelo ajuste noturno; o backlog reconhece expressamente as semânticas diferentes desse campo. (`app/chamados/services/materializacao.py:344`; `app/chamados/services/materializacao.py:414`; `app/chamados/juizes.py:310`; `app/docs/BACKLOG.md:990`.)

Padrão: **Single Writer Principle**. A obra existente que cobre é **DIA-DA-PERGUNTA-E-O-TURNO, complemento O261**; o identificador literal `BUG-PLANTIO-NOTURNO-MAIS-1` veio no pedido, sem correspondência localizada nos dois registros pesquisados. (`app/docs/GLOSSARIO.md:12`; `app/docs/BACKLOG.md:990`; correspondência literal [nao verificado].)

A batida anômala veio da resposta validada, **não do aparelho**. `timestamp_dispositivo` e `sincronizado_em` são campos anuláveis; seus nulos não provam perda, e o anexo não traz uma tentativa local ausente do servidor nem valores monetários para demonstrar dinheiro errado. (`app/ponto/models.py:75`; `app/ponto/models.py:77`; FICHA/perda/dinheiro [nao verificado].)

**ERROS da sonda:** veio `ERROS {}`; nenhum item ficou inconclusivo por erro declarado da sonda. As lacunas centrais de **02 e 10**, a causa da lentidão de 03 e as identidades não expostas permanecem sem conclusão por falta de evidência. (Anexo [nao verificado].)

**(1) Tabela consolidada.** Datas/colaboradores abaixo vêm do anexo [nao verificado]; os enquadramentos remetem às evidências acima. “Não provado” em SOBE significa ausência de prova de dinheiro errado ou batida perdida [nao verificado].

| item | colNNN + dia | classe | padrão quebrado | obra | SOBE? |
|---|---|---|---|---|---|
| 01 | col39 · 15/10 | producao batida | n/a — BATIDA | n/a | Não provado |
| 02 | col993 × col1023 · pedido sem dia identificado | falta informacao | não determinado | não determinada | Não provado |
| 03 | col não informado · 28/10–06/11 | nosso com obra | Event-driven architecture | **sem obra** específica localizada | Não provado |
| 04 | col39 · 15/10 | producao batida | n/a — BATIDA | n/a | Não provado |
| 05 | col155 · 28/10–06/11 | nosso com obra | Duplicated business logic / logic drift | **sem obra** específica localizada | Não provado |
| 06 | col950 · TESTE06 | producao cadastro | n/a — CADASTRO | n/a | Não provado |
| 07 | col836 · 14/10 | ja OK hoje | n/a | n/a | Não provado |
| 08 | col617 · 05–06/11 | ja OK hoje | n/a | n/a | Não provado |
| 09 | col1000 · 26 e 31/10 sem batida | ja OK hoje | n/a | n/a | Não provado |
| 10 | col399 · 24 e 31/10 | falta informacao | não determinado | O10-B relacionada; aplicação não provada | Não provado |
| 11 | col921 · 14–28/10 | ja OK hoje | n/a | n/a | Não provado |
| 12 | col921 · folgas 17 e 24/10 | ja OK hoje | n/a | n/a | Não provado |
| 13 | col407 · turno 21/10 / saída plantada 23/10 | nosso com obra | Single Writer Principle | DIA-DA-PERGUNTA-E-O-TURNO / O261 | Não provado |
| 14 | col407 · pergunta 47210 de 22/10 | nosso com obra | Single Writer Principle | DIA-DA-PERGUNTA-E-O-TURNO / O261 | Não provado |

**(2) Contagem:** ja OK hoje **5**; nosso com obra **4**; producao batida **2**; producao cadastro **1**; falta informacao **2**. **SOMA = 14**, contando 01/04 e 13/14 separadamente, conforme as linhas da tabela.

**(3) Textos para o DP**

**01.** Abra o atestado em **Ausências** e confira a data e o período junto das marcações de 15/10. O documento já consta aprovado, mas existem marcações na mesma data. (`app/templates/ponto/partials/painel_ausencia.html:130`; FICHA [nao verificado].)

**02.** Abra o chamado da correção e confira a data da pergunta e se ela já recebeu resposta. Há horários registrados e perguntas ainda sem resposta; a aprovação de justificativa e a confirmação de lote são etapas diferentes. (`app/chamados/services/validacao.py:363`; `app/chamados/services/regularizacao_ext.py:36`; FICHAS [nao verificado].)

**03.** Em **Validar em lote**, aguarde o quadro de resultado. Confira nele quantas perguntas foram validadas, quais ficaram de fora e quais foram validadas sem gravar o ponto. (`app/templates/chamados/partials/_lote_preview.html:5`.)

**04.** No calendário de 15/10, consulte **Resolver dia** e compare as marcações com o atestado em Ausências. A pendência indicada é a existência de marcações em um dia já coberto pela ausência. (`app/templates/colaboradores/partials/_calendario_grade.html:125`; FICHA [nao verificado].)

**05.** Confira se a tentativa terminou com confirmação de registro. Quando aparece a mensagem de GPS obrigatório, aquela tentativa não é registrada; as marcações apresentadas neste levantamento chegaram com localização. (`app/api/views_core.py:454`; FICHA [nao verificado].)

**06.** Consulte a situação cadastral da conta de teste. Ela ainda aparece como ativa e entra nos colaboradores comuns, embora já esteja fora do universo de operação. (`app/colaboradores/queries.py:25`; TESTE06 [nao verificado].)

**07.** No painel **Chamados**, selecione **Histórico** para localizar o registro de 14/10. A lista inicial mostra os chamados que aguardam ação, e esse registro consta como resolvido. (`app/chamados/services/painel.py:298`; FICHA [nao verificado].)

**08.** Abra o segundo atestado em **Ausências** e confira o período 05–06/11 e a guia **Auditoria**. O registro está aprovado, e essa guia distingue quem lançou de quem decidiu. (`app/templates/ponto/partials/painel_ausencia.html:257`; FICHA [nao verificado].)

**09.** Confira separadamente os dias no calendário e abra o chamado pelo número exibido. Em 26/10 há abono e em 31/10 há folga; 29 e 30/10 têm chamados próprios em análise. (`app/templates/colaboradores/partials/_calendario_grade.html:108`; FICHA [nao verificado].)

**10.** No calendário, compare 24/10 e 31/10 com o pedido de troca. O levantamento mostra ausência no primeiro dia e folga trabalhada no segundo, mas não informa quando essa classificação mudou. (`app/colaboradores/services/calendario.py:662`; FICHA [nao verificado].)

**11.** Para os dias depois de 20/10, abra o chamado e acompanhe as perguntas. **Resolver dia** fica disponível como último recurso para competência já encerrada. (`app/colaboradores/services/calendario.py:455`.)

**12.** Confira no calendário se o dia está previsto como trabalho ou folga. **Falta** só aceita dia com jornada prevista; o levantamento não identifica um lançamento específico de abandono para esses dias de folga. (`app/ponto/precedencia.py:536`; lançamento específico [nao verificado].)

**13.** Confira no espelho o turno iniciado em 21/10 e a saída da manhã seguinte. A marca das 07:00 exibida em 23/10 veio da resposta validada no chamado, não de uma nova marcação do aparelho. (`app/chamados/services/materializacao.py:414`; FICHA [nao verificado].)

**14.** Abra o chamado em análise e confira a data indicada junto da pergunta respondida. A resposta consta validada, enquanto o dia 21/10 continua irregular no levantamento. (`app/chamados/services/fio.py:40`; FICHA [nao verificado].)

## 06/11 18:51 · sessao 5be272a7c35a · - · - · origem=Ronald (tmux)

- **01+04 — col39, 15/10: produção batida; dono BATIDA; SOBE não provado.** A FICHA registra atestado4879 aprovado em16/10 16:36:56 e três marcações no15/10, sem saída final; retém por `espelho_fato_em_ausencia` [nao verificado: anexo sem arquivo:linha]. O código acusa fato em dia coberto por ausência aprovada (`app/ponto/supra_juiz.py:400`) e retém esse veredito (`app/escala/models.py:1753`; `app/folha/export.py:1233`). Porta do atestado: **Ausências**, onde o chamado é aviso e não decide (`app/ponto/services/ausencia.py:637`). Calendário oferece **Resolver dia** conforme a flag (`app/templates/colaboradores/partials/_calendario_grade.html:125`), POST em `/ponto/celula/veredito/` (`app/ponto/urls.py:58`); o **Trabalhou** pode recusar dia classificado como ausência (`app/ponto/services/veredito_celula.py:221`).

  Texto DP: “Abra o atestado em Ausências e confira a data e o período junto das marcações de15/10. No calendário, consulte Resolver dia: há marcações e uma ausência já aprovada para a mesma data.” (`app/templates/ponto/partials/painel_ausencia.html:130`; `app/templates/colaboradores/partials/_calendario_grade.html:125`; dados [nao verificado]).

- **07 — col836, 14/10: já OK hoje para a alegação estrita da lista; cadastro segue pendente.** Chamado31151 está resolvido via sistema desde14/10 08:15:08; `superado_por` e `silenciado_ate` nulos, célula presente e empresa2 [nao verificado: FICHA]. A lista pertinente é **Painel de gestão**, `/chamados/` (`app/chamados/urls.py:9`; `app/chamados/views.py:721`): padrão mostra somente aberto/em_análise, enquanto Histórico inclui resolvido/fechado (`app/chamados/services/painel.py:298`; `app/chamados/catalogo/motor.py:29`). Não há filtro automático por célula, `superado_por` ou empresas visíveis nesse queryset; empresa, praça e colaborador só filtram por parâmetros explícitos (`app/chamados/services/painel.py:96`). Valores de `agrupado_em`, pai atual e filtros usados pelo usuário não vêm na sonda [nao verificado].

  Texto DP: “No painel Chamados, selecione Histórico para localizar o registro de14/10. A lista inicial mostra os chamados que ainda aguardam ação; esse registro consta como resolvido.” (`app/chamados/services/painel.py:298`; estado [nao verificado]).

- **08 — col617, 05–06/11: já OK hoje; SOBE não provado.** Ausência5380 rejeitada;5399 aprovada cobrindo05–06/11; ambas registradas pelo próprio colaborador, mas **`aprov_proprio=false`**, portanto o dado não sustenta aprovação pelo próprio colaborador [nao verificado: FICHA]. Com anexo, atestado nasce `aguardando_decisao` (`app/ponto/services/ausencia.py:779`); API não força aprovação (`app/api/views.py:1820`). A exceção automática é **FALTA_DIA**, confissão contra interesse; ATESTADO usa estado inicial do juiz (`app/chamados/services/materializacao.py:579`). Identidade do decisor, data correta do documento e alegação “sem aceite” não são demonstradas [nao verificado]. O identificador **ESPELHO-DIZ-QUEM-DECIDIU** não foi encontrado na cópia [nao verificado]; não atribuir obra inexistente.

  Texto DP: “Abra o segundo atestado em Ausências e confira o período05–06/11 e a guia Auditoria. O registro está aprovado; essa guia separa quem lançou de quem decidiu.” (`app/templates/ponto/partials/painel_ausencia.html:49`; `app/templates/ponto/partials/painel_ausencia.html:257`; estado [nao verificado]).

- **09 — col1000: produção cadastro para a pendência de escala agrupada; limitar a conclusão.** Em26/10 não há batida, mas há abono5218 aprovado por usuário diferente do colaborador;31/10 é folga e tem decisões `admin_decidiu` em06/11 10:19:59 [nao verificado: FICHA]. Pai35214 segue aberto por folga não definida; filhos35215 e36084 constam resolvidos via sistema com chaves `agrupado_em`/`defeito_cadastro`, mas o valor do pai não foi fornecido [nao verificado]. O código permite filho agrupado resolvido enquanto o furo permanece visível no espelho (`app/chamados/models.py:772`). **Não generalizar cadastro para27/29/30:** esses dias têm furos;29/30 possuem chamados37325/37326 em análise hoje, e27 possui perguntas vivas [nao verificado]. A tela exata alegada está [nao verificado]: o calendário mostra status do chamado no vínculo (`app/templates/colaboradores/partials/_calendario_grade.html:108`), e decisão humana por glifo próprio (`:109`), cujo autor exige trilha ausente da FICHA (`app/ponto/services/veredito_celula.py:85`).

  Texto DP: “Confira o dia no calendário e abra o chamado pelo número exibido. Um chamado agrupado pode aparecer resolvido enquanto a escala continua pendente;29 e30/10 têm registros próprios em análise.” (`app/templates/colaboradores/partials/_calendario_grade.html:108`; `app/chamados/models.py:772`; estados [nao verificado]).

**Referências adicionais úteis:** Auditoria de Ausências mostra “Lançado por” e “Decidido por” (`app/templates/ponto/partials/painel_ausencia.html:257`); mudar estado de ausência passa pelo escritor canônico e registra antes/depois (`app/ponto/services/ausencia.py:808`). Nenhum desses itens demonstrou perda de batida ou valor monetário errado; marcação SOBE seria [nao verificado].

## 06/11 18:51 · sessao a5c5820a10a6 · - · - · origem=Ronald (tmux)

**03 — lote síncrono; falha e causa do aumento não provadas.** A rota chega à view indicada; ela executa `validar_lote_classe_a` antes de renderizar a resposta, e o serviço percorre as referências uma por uma, reclassifica e chama a validação individual. Cada validação recalcula por evento, materializa a pergunta e varre a disputa. (`app/chamados/urls.py:37`; `app/chamados/views_cobrar.py:379`; `app/chamados/services/validacao.py:680`; `app/chamados/services/validacao.py:699`; `app/chamados/services/validacao.py:395`; `app/chamados/services/validacao.py:430`.)

O Caddy encaminha a rota à UI sem configurar timeout específico; a UI é `gthread`, timeout 30. A documentação de O284 explica expressamente que esses 30 segundos controlam worker travado, não estabelecem prazo máximo por requisição longa. (`Caddyfile:8`; `Caddyfile:10`; `app/gunicorn.conf.py:19`; `app/gunicorn.conf.py:21`; `app/docs/TICKETS.md:132`.)

Não há X segundos identificado para a interface desistir: o formulário usa HTMX sem timeout próprio e só apresenta resultado após a resposta; a cópia vendorizada de HTMX 1.9.10 tem `timeout:0`, enquanto a tela carrega essa versão por CDN. Em erro HTTP aparece toast “Não deu…”; em falha de transporte, “Sem resposta do servidor…”. (`app/templates/chamados/partials/_lote_preview.html:21`; `app/templates/chamados/partials/_lote_preview.html:5`; `app/core/tests/vendor/htmx-1.9.10.min.js:1`; `app/templates/base.html:19`; `app/templates/core/_dialogo.html:61`; `app/templates/core/_dialogo.html:75`.)

O anexo mostra 35 respostas 200, entre 10,435 e 122,373 segundos; isso não prova que todas as perguntas foram validadas, pois o resultado 200 também pode conter recusadas e validadas sem gravar ponto. A causa da subida em 03/11 permanece **[nao verificado]**: o anexo não traz referências, tamanho dos lotes, payload, perfil de queries nem estado da infraestrutura. (`app/templates/chamados/partials/_lote_preview.html:6`; `app/templates/chamados/partials/_lote_preview.html:7`; `app/templates/chamados/partials/_lote_preview.html:9`; dado `ui_lote.log` fornecido, sem arquivo:linha **[nao verificado]**.)

A regra “trabalho longo vai pra job” existe; porém não encontrei obra específica de job para esse endpoint. O284 trata PDF do espelho e `FUROS-EMPRESA-TIMEOUT` trata `dataset_furos`, portanto não cobrem automaticamente este lote. (`CLAUDE.md:89`; `app/docs/BACKLOG.md:1526`; `app/docs/BACKLOG.md:1824`.) Padrão específico quebrado do glossário: **[nao verificado]**; não atribuir “Transactional Outbox” apenas porque é trabalho demorado. (`app/docs/GLOSSARIO.md:17`.)

Texto DP: “Em Validar em lote, aguarde o quadro com a quantidade validada e os casos que ficaram de fora. O resultado diferencia perguntas validadas, recusadas e validadas sem gravação do ponto.” (`app/templates/chamados/partials/_lote_preview.html:5`.)

**05 — bloqueio sem GPS está provado no código; tentativa perdida de col155 não.** O candidato principal é col155, maior atividade; tem dois devices ativos e 18 batidas listadas, todas com geo, predominantemente APK; o anexo não contém tentativa recusada nem estado do GPS naquele momento. (Dado FICHA05 fornecido, sem arquivo:linha **[nao verificado]**.)

A rota efetiva de `/api/ponto/bater/` usa `views_core`; sem latitude ou longitude registra auditoria `batida_recusada_sem_gps` e retorna **403 `gps_obrigatorio` antes de registrar a batida**. (`app/api/urls_core.py:5`; `app/api/urls_core.py:14`; `app/api/views_core.py:440`; `app/api/views_core.py:443`; `app/api/views_core.py:454`.) Isso contraria a regra explícita “batida de chão nunca é barrada em runtime”. (`CLAUDE.md:140`.)

No PWA V2 há também bloqueio no cliente: tenta GPS por 10 segundos e depois 12; continuando sem coordenadas, retorna `false`, e o clique aborta antes do fetch. (`app/templates/ponto/bater_ponto_v2.html:292`; `app/templates/ponto/bater_ponto_v2.html:297`; `app/templates/ponto/bater_ponto_v2.html:325`; `app/templates/ponto/bater_ponto_v2.html:335`.) O helper `static/pwa.js::capturarGPS` isoladamente só retorna `null`; a fila offline envia ao mesmo endpoint e elimina a pendência apenas com resposta OK. (`app/static/pwa.js:289`; `app/static/pwa.js:153`; `app/static/pwa.js:158`.)

Padrão defensável: **Duplicated business logic / logic drift**, pois a tela antiga explicitamente prossegue sem GPS enquanto V2/API barram; obra específica em BACKLOG/TICKETS não encontrada, **sem obra**. (`app/docs/GLOSSARIO.md:23`; `app/templates/ponto/bater_ponto.html:507`; `app/templates/ponto/bater_ponto_v2.html:297`; `app/api/views_core.py:440`.) SOBE como **batida individual perdida** não demonstrado pelo dado; bloqueio estrutural é demonstrado. (`app/api/views_core.py:454`; FICHA05 **[nao verificado]**.)

Texto DP: “As marcações listadas deste aparelho chegaram com localização. Quando o aplicativo apresenta a mensagem de GPS obrigatório, a tentativa não é registrada; confira se houve confirmação de registro.” (`app/api/views_core.py:454`; FICHA05 **[nao verificado]**.)

**06 — col950; cadastro explica o universo.** O próprio BACKLOG identifica col950 como conta de teste; TESTE06 é a única linha com devices iOS e seis batidas, está em comuns e fora de operação. (`app/docs/BACKLOG.md:1797`; TESTE06 fornecido, sem arquivo:linha **[nao verificado]**.)

`colaboradores_comuns` exige situação ativo, empresa ativa e username de 11 dígitos; operação acrescenta empresa em rollout e vínculo vigente. Não se pode distinguir pelo anexo qual dessas duas condições adicionais exclui col950 da operação. (`app/colaboradores/queries.py:25`; `app/colaboradores/queries.py:50`; `app/colaboradores/queries.py:64`; `app/colaboradores/queries.py:65`.)

O campo existente `situacao='desligado'` o retiraria de ambos; `isento_ponto` existe, mas não participa desses filtros e não o retiraria dos contadores de comuns. (`app/colaboradores/models.py:294`; `app/colaboradores/models.py:299`; `app/colaboradores/models.py:323`; `app/colaboradores/queries.py:25`; `app/colaboradores/queries.py:65`.) Classe sugerida: **producao cadastro**, dono CADASTRO. (`CLAUDE.md:176`; `app/colaboradores/queries.py:25`.)

Texto DP: “A conta de teste ainda está cadastrada como ativa e entra no universo dos colaboradores comuns. Ela já está fora do universo de operação; a situação cadastral controla sua participação nos contadores.” (`app/colaboradores/queries.py:25`; `app/colaboradores/queries.py:65`; TESTE06 **[nao verificado]**.)

**Histórico Git:** `git log` não está disponível nesta cópia, pois o comando retorna “not a git repository”; horário de commit/deploy por Git permanece **[nao verificado]**. O BACKLOG registra O10-B no ar às 13:20, mas isso é documento, não verificação pelo Git pedido. (`app/docs/BACKLOG.md:1792`.)

## 06/11 18:50 · sessao 3674a942c380 · - · - · origem=Ronald (tmux)

**02 — candidato col993, comparação inconclusiva.** Pelo anexo, é o maior candidato (atividade 102, contra 91 de col723 e 19 de col78); falta dia/hora/ID para identificar a correção alegada, então recomendo classe **“falta informacao”**, dono não demonstrado. O anexo mostra várias correções que chegaram por `PerguntaDisputa` em 03/11: batidas `disputa_s84_retro` de 24, 26, 28 e 30/10 e 01/11; em 24/10 a pergunta 53441 ainda não tem resposta, enquanto 03/11 e 05/11 também têm perguntas sem resposta. Esses fatos são da **FICHA fornecida, sem arquivo:linha [nao verificado na cópia]**.

Os três caminhos diferem:

- **Justificativa:** a aprovação só cria saída quando existem `saida_informada` e `batida_entrada`; o código pode aprovar sem criar batida quando esses campos faltam. (`app/ponto/services/justificativa.py:137`, `:172`, `:190`, `:202`). O POST de justificativa lê tipo/data/descrição, sem ler horário informado. (`app/api/views.py:1954`, `:1997`, `:2007`).
- **RegularizacaoExterna:** precisa de confirmação do colaborador; disputa aberta segura o lote; cria origem `reg_externa`. (`app/chamados/services/regularizacao_ext.py:36`, `:51`, `:58`, `:76`). As FICHAS 02a e 02b têm `regularizacoes=[]` **[nao verificado na cópia]**.
- **PerguntaDisputa:** validação exige `respondida_em`; materialização escolhe perguntas respondidas, validadas e ainda não materializadas. (`app/chamados/services/validacao.py:363`, `app/chamados/services/materializacao.py:1344`). Horário fixo de intervalo planta no marco do chamado, podendo diferir do horário livre digitado — isso explica a mecânica do 11:00→12:00 da pergunta 53440, mas os marcos do chamado não foram anexados. (`app/chamados/services/materializacao.py:404`).

A FICHA 02b/col1023 mostra abonos em 03 e 04/11, uma batida de app de 04/11 retratada e batidas normais em 05/06, enquanto as Justificativas continuam pendentes e sem saída informada; não mostra a correção de horário usada como comparação **[nao verificado na cópia]**. Não substituiria col993 por col723 sem evidência identificando o pedido: col723 tem respostas de 26/10 validadas, mas materializadas anteriormente por `celula_absolveu`; isso é outro caso presente na FICHA **[nao verificado na cópia]**.

Texto DP: “Confira no chamado a data e o horário da correção que deseja acompanhar. Há horários já registrados no espelho e perguntas ainda sem resposta; a confirmação de um lote e a aprovação de uma justificativa são etapas diferentes.” (`app/chamados/services/regularizacao_ext.py:36`, `app/chamados/services/validacao.py:363`; situação específica no anexo **[nao verificado na cópia]**).

**13+14 — col407 pela evidência contrária ao maior candidato; “nosso com obra”, ESTRUTURA.** O maior candidato col114 tem batidas diurnas, tipos repetidos e perguntas de retorno de intervalo sem resposta; não corresponde ao relato noturno. Col407 corresponde precisamente: pergunta 47210 refere-se a 22/10, respondeu 07:00 em 22/10 08:07:38, validou em 06/11 14:45:08, e gerou a batida 147824, **S em 23/10 07:00**, origem `disputa_s84_retro`, sem timestamp de dispositivo; 21/10 continua `furo`, enquanto 23/10 está `ok` **[FICHA fornecida; nao verificado na cópia]**.

A assinatura confirma o caso **data civil versus dia do turno / deslocamento +1**: o plantio parte de `p.data_evento`, combina a hora e chama o ajuste noturno (`app/chamados/services/materializacao.py:344`, `:414`, `:420`); o ajuste pode escolher a data base ou a seguinte conforme envelope do turno (`app/chamados/juizes.py:303`, `:310`). O `data_evento` usado como dia da batida também é comparado ao dia do chamado pela porta (`app/chamados/services/fio.py:40`, `:84`).

**Padrão:** `Single Writer Principle` (`app/docs/GLOSSARIO.md:12`); também há quebra da fonte única do DNA descrita no backlog. **Obra existente:** `DIA-DA-PERGUNTA-E-O-TURNO`, complemento da O261, cobre explicitamente as três semânticas de `data_evento` e o plantio pelo instante do DNA (`app/docs/BACKLOG.md:990`). O vizinho `PLANTIO-ENVELOPE-PELO-GERADOR` registra o leitor do gerador usado pelo ajuste (`app/docs/BACKLOG.md:989`). O ID literal `BUG-PLANTIO-NOTURNO-MAIS-1` não apareceu na busca nos documentos; o aval desse nome está apenas no pedido **[nao verificado na cópia]**.

**Batida perdida / SOBE:** não há prova no anexo de uma batida feita no aparelho que não chegou; a batida anômala é plantio de resposta, enquanto as batidas `app` mostradas estão no servidor. `timestamp_dispositivo` é horário bruto do aparelho e `sincronizado_em` é chegada ao servidor, ambos nullable (`app/ponto/models.py:75`, `:77`); nulo não demonstra perda. Também não há valores de folha para provar dinheiro errado; recomendo **SOBE = não, sem prova de dinheiro ou batida perdida**, mantendo o bug de data confirmado pelo anexo **[nao verificado na cópia]**.

Texto DP para 13: “Confira as marcações do turno que começou em 21/10, incluindo a saída da manhã seguinte. A marca das 07:00 exibida em 23/10 veio da resposta validada no chamado, e não de uma nova marcação do aparelho.” (`app/chamados/services/materializacao.py:414`, `:420`; origem/datas do caso no anexo **[nao verificado na cópia]**).

Texto DP para 14: “Abra o chamado que permanece em análise e confira a data do turno indicada junto à pergunta. A resposta validada e o dia ainda pendente precisam ser conferidos separadamente no espelho.” (`app/chamados/services/fio.py:45`, `:51`; chamado/data do caso no anexo **[nao verificado na cópia]**).

## 06/11 18:50 · sessao c9d86a730ef0 · BUG-PLANTIO-NOTURNO-MAIS-1 · censo · origem=Code

O código permite o mecanismo **D → D+1 → D+2**: um emissor resolve `data_evento` como dia esperado da saída; depois, o materializador usa essa data como âncora de outro turno overnight. Isso confirma a possibilidade de **dois avanços semânticos de dia**, condicionada aos marcos disponíveis; não são duas chamadas consecutivas ao ajuste. A ocorrência específica em produção, incluindo entrada às 19:26 e batida final em D+2, permanece **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1041`; `app/escala/servico_jornada.py:483`; `app/chamados/services/materializacao.py:414`; `app/chamados/juizes.py:310`)

Todas as referências abaixo são relativas ao repositório.

1. O censo identificou **dois sítios físicos de persistência**: o `create` em `PerguntaDisputa.abrir` e o `save` em `reapontar_resolvedora`. O segundo só preenche uma pergunta que ainda não tem data; não substitui uma data preenchida. (`app/chamados/models.py:2476`; `app/chamados/services/fio.py:832`; `app/chamados/services/fio.py:847`)

A atribuição a `_px.data_evento` pertence a um objeto `_Proxy`, usado para consultar o juiz; não persiste pergunta. A atribuição `p.data_evento = None` desfaz em memória o preenchimento recusado e retorna antes do `save`. (`app/chamados/models.py:2415`; `app/chamados/services/fio.py:841`)

Existem **seis chamadas de nascimento por `PerguntaDisputa.abrir`** e **quatro delegações de reapontamento com data explícita**, enumeradas na tabela final. (`app/chamados/services/disputa_emissao.py:389`; `app/chamados/services/disputa_emissao.py:770`; `app/chamados/services/disputa_emissao.py:1195`; `app/chamados/services/declaracao_texto.py:325`; `app/chamados/services/declaracao_texto.py:614`; `app/chamados/services/acoes_disputa.py:259`; `app/chamados/reconciliador.py:943`; `app/chamados/reconciliador.py:1626`; `app/chamados/reconciliador.py:1638`; `app/chamados/services/declaracao_texto.py:320`)

Não localizei escrita adicional desse campo por `get_or_create`, `update_or_create`, `defaults`, `bulk_create`, `bulk_update`, `setattr`, `.update()` ou SQL direto; a ausência no inventário estático não possui uma referência única de código. **[nao verificado]**

O gerador genérico atende aos seguintes motivos, aqui denominados **G**: `sem_posto`, `em_ferias`, `orfao_14h`, `saida_sem_entrada`, `posto_desativado`, `colisao_escala`, `ordem_substituicao_invalida`, `batida_fora_escala_grave`, `gps_ausente`, `foto_ausente`, `gps_mock`, `device_comprometido`, `liveness_indisponivel`, `ancora_ausente`, `intervalo_saida` e `intervalo_volta`; motivos sem pergunta no catálogo são ignorados. (`app/chamados/catalogo/perguntas.py:34`; `app/chamados/services/disputa_emissao.py:294`)

Os quatro motivos derivados de marcos, aqui denominados **M**, são `saida_sem_entrada` para início/E, `orfao_14h` para fim/S, `intervalo_saida` para ida/S e `intervalo_volta` para volta/E. (`app/chamados/services/declaracao_texto.py:42`)

As fontes que chegam ao nascimento precisam ser distinguidas:

| Caminho | Fonte efetiva de `data_evento` |
|---|---|
| Gerador genérico | Data obtida da grade quando falta data para o texto; depois contexto do motivo; depois data local de `ultima_batida.timestamp`. Para os motivos que precisam de destino, `_derivar_chamado_destino` também participa da escolha. (`app/chamados/services/disputa_emissao.py:319`; `app/chamados/services/disputa_emissao.py:343`; `app/chamados/services/disputa_emissao.py:356`; `app/chamados/services/disputa_emissao.py:381`) |
| Julgamento da célula | Injeta `cel.data` no contexto e chama o gerador: **dia da célula**, inclusive para cobrança da saída overnight. (`app/chamados/services/fabrica_da_celula.py:139`; `app/chamados/services/fabrica_da_celula.py:176`) |
| Lista de entradas pendentes | Usa `entradas_pendentes[*].data`, produzida a partir do timestamp local da **entrada**; classifica esse dia e o injeta no contexto. (`app/ponto/utils_turno_orfao.py:34`; `app/chamados/services/disputa_emissao.py:257`; `app/chamados/services/disputa_emissao.py:272`) |
| Detector de `orfao_14h` da API | Coloca no contexto a **data esperada da saída**, calculada a partir da entrada; portanto pode fornecer D+1. Não é simplesmente a data do relógio da cobrança. (`app/chamados/detector_anomalias.py:42`; `app/chamados/detector_anomalias.py:47`; `app/escala/utils.py:963`; `app/escala/utils.py:968`) |
| API, fallback `ultima_batida` | O timestamp inserido nesse contexto é `agora.isoformat()`; seu consumo como fallback fornece o dia do relógio da requisição. (`app/api/views.py:772`; `app/chamados/services/disputa_emissao.py:356`) |
| PWA, `saida_sem_entrada` | Injeta `agora_local.date()` e `hora_tentativa`; cobra **entrada**, não saída final. (`app/ponto/services/triagem_batida.py:649`) |
| Complemento de marcos do agrupador | Usa o `dia` devolvido pelos marcos faltantes, filtrado por igualdade ao dia solicitado. Os chamadores fornecem data do contexto/pergunta, dia da ata ou data escolhida pelo agrupamento. (`app/chamados/services/disputa_emissao.py:806`; `app/chamados/services/disputa_emissao.py:913`; `app/chamados/services/disputa_emissao.py:1013`; `app/chamados/services/disputa_emissao.py:1052`; `app/chamados/services/disputa_emissao.py:1083`) |
| Intervalo dedicado do agrupador | Usa `marcos_faltantes[*].data` do contexto; motivos `intervalo_saida`/`intervalo_volta`. (`app/chamados/services/disputa_emissao.py:982`) |
| Grupo por batida de origem | Classifica o dia da entrada, mas grava `resolver_data_evento(...) or data_entrada`. Para `orfao_14h`, o resolver devolve o **dia esperado da saída**, potencialmente D+1. (`app/chamados/services/disputa_emissao.py:1037`; `app/chamados/services/disputa_emissao.py:1041`; `app/escala/servico_jornada.py:483`) |
| Agrupador sem batida de origem | Usa `data_evento_do_contexto`: prioridade `data_turno`, `data_batida`, `entrada_timestamp`, `pendentes` em formato BR e `horario_previsto`. (`app/chamados/services/disputa_emissao.py:1065`; `app/chamados/data_evento.py:16`) |
| Declaração de hora e aplicação de texto | Usam o dia do marco faltante escolhido. `_marcos_faltantes` devolve `data_dia`, inclusive a célula da véspera quando ela fornece o marco final. (`app/chamados/services/declaracao_texto.py:119`; `app/chamados/services/declaracao_texto.py:299`; `app/chamados/services/declaracao_texto.py:579`) |
| Conversa sem data explícita | `abrir` usa `timezone.localdate()`: relógio do nascimento da pergunta. Inclui a confirmação de âncora e a reabertura administrativa. (`app/chamados/models.py:2424`; `app/chamados/services/disputa_emissao.py:1195`; `app/chamados/services/acoes_disputa.py:259`) |
| Comando de smoke, fora de testes/migrations | Injeta a data literal `2026-05-15` quando o template contém `{data}` e chama o gerador. (`app/chamados/management/commands/smoke_chamados.py:90`) |
| Lote de competência | Delega à fábrica com o `dia` explícito da competência. (`app/chamados/management/commands/disparar_perguntas_competencia.py:268`) |
| Retroativo de selfie | Fornece `batida_em`, mas não `data` nem `ultima_batida` nesse contexto; o gerador não usa `batida_em` entre suas fontes de data, podendo chegar ao relógio de nascimento. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:96`; `app/chamados/services/disputa_emissao.py:343`; `app/chamados/models.py:2424`) |

2. **`_T1` está declarado em `core/juizes.py`, e aponta para `turnos_do_colab`.** Seu valor exato é `'a que turno (data_turno) pertence a batida?'`. (`app/core/juizes.py:368`; `app/core/juizes.py:196`)

As assinaturas relevantes são:

```python
turnos_do_colab(colab, ini, fim, celulas=None)
dia_das_batidas(colab, ini, fim, turnos=None)
_dia_das_batidas_puro(turnos, batidas_da_janela)
```

A primeira é o juiz registrado; a segunda fornece o mapa para apresentação; a terceira contém a atribuição `out[b.pk] = t.data_turno`. Para batida que nenhum turno reclama, usa `data_local(b.timestamp)`. (`app/ponto/turnos.py:1848`; `app/ponto/turnos.py:2874`; `app/ponto/turnos.py:2908`; `app/ponto/turnos.py:2915`; `app/ponto/turnos.py:2917`)

Já chamam `dia_das_batidas`: o espelho da API, as justificativas da API pelo alias `_ddb_j` e o espelho da tela. O carregador em lote chama o mesmo corpo, e o painel consome seu mapa. (`app/api/views.py:1446`; `app/api/views.py:2038`; `app/ponto/services/espelho.py:451`; `app/ponto/turnos.py:3007`; `app/core/services/painel_op.py:330`)

`turnos_do_colab` já é chamado, entre outros consumidores do fluxo investigado, por `_turno_tem_saida`, `_turno_cruza_meia_noite`, classificação da jornada, detector de anomalias, materialização, núcleo, API, espelho, PDF e pelo próprio vigia overnight. (`app/chamados/juizes.py:371`; `app/chamados/juizes.py:1842`; `app/escala/servico_jornada.py:392`; `app/chamados/detector_anomalias.py:155`; `app/chamados/services/materializacao.py:123`; `app/chamados/services/materializacao.py:220`; `app/ponto/nucleo.py:47`; `app/api/views.py:1449`; `app/ponto/services/espelho.py:614`; `app/relatorios/pdf_espelho.py:451`; `app/ponto/management/commands/vigia_plantio_overnight.py:38`)

3. O caminho da resposta de hora até a batida é o seguinte:

- A API recebe a resposta e chama `responder_pergunta_disputa_core`; o núcleo valida o formato HH:MM e grava `resposta_colab`/`respondida_em`. Esse passo não altera `data_evento`. (`app/api/views.py:2642`; `app/chamados/services/disputa_emissao.py:1263`; `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1301`)
- A ação de validação chama `validar_pergunta`, que carimba a validação e encaminha a pergunta de hora para `_materializar_uma`; depois processa o restante da disputa. (`app/chamados/services/acoes_disputa.py:19`; `app/chamados/services/validacao.py:396`; `app/chamados/services/validacao.py:422`; `app/chamados/services/validacao.py:430`)
- `_materializar_uma` verifica a divergência de datas e entra no ramo `_materializar_resposta_hora` dentro de transação. (`app/chamados/services/materializacao.py:1166`; `app/chamados/services/materializacao.py:1195`)
- `_materializar_resposta_hora` exige texto de resposta e chama `_ts_tipo_do_plantio`. (`app/chamados/services/materializacao.py:804`; `app/chamados/services/materializacao.py:821`)
- `_ts_tipo_do_plantio` escolhe `p.data_evento`, ou a data do contexto como fallback; resolve o tipo pelo marco quando possível; combina a hora com essa data e chama `_ajustar_saida_overnight`. Para intervalos fixos, pode usar a hora do marco. (`app/chamados/services/materializacao.py:344`; `app/chamados/services/materializacao.py:386`; `app/chamados/services/materializacao.py:407`; `app/chamados/services/materializacao.py:414`; `app/chamados/services/materializacao.py:420`)
- Após procura de batida existente e guardas de paridade, chama `registrar_batida(..., 'disputa_s84_retro', fato_de_chao=False, aprovada=True, ...)`; vincula `pergunta_origem` e consulta o juiz após o plantio, com rollback se ele reprovar. (`app/chamados/services/materializacao.py:834`; `app/chamados/services/materializacao.py:850`; `app/chamados/services/materializacao.py:902`; `app/chamados/services/materializacao.py:921`; `app/chamados/services/materializacao.py:1208`)
- `registrar_batida` grava o timestamp e tipo recebidos no `Batida.objects.create`. (`app/ponto/registro_batida.py:157`)

O chamador direto do ajuste é **`_ts_tipo_do_plantio`**. `_slot_esperado` e `estado_aplicacao_pergunta` também chegam ao ajuste por essa mesma derivação. (`app/chamados/services/materializacao.py:420`; `app/chamados/juizes.py:345`; `app/chamados/services/materializacao.py:443`)

Os três resultados de data efetivamente presentes no ajuste são:

| Caminho | Regra |
|---|---|
| Reancoragem pelos marcos | Carrega os marcos encadeados de **`data_evento`**, monta envelope de ±90 minutos e testa a hora local primeiro em `data_evento`, depois em `data_evento + 1`. Retorna o primeiro candidato dentro do envelope. (`app/chamados/juizes.py:303`; `app/chamados/juizes.py:307`; `app/chamados/juizes.py:310`) |
| Preservação | Com marcos, se nenhum candidato cabe, retorna `ts`; também preserva o timestamp nos demais ramos sem deslocamento. (`app/chamados/juizes.py:314`; `app/chamados/juizes.py:323`) |
| Recuo da entrada | **Sem marcos**, para tipo E com `hora_tentativa`, combina a hora da tentativa em `data_evento`; se a entrada calculada está depois dessa saída, retorna `ts - 1 dia`. (`app/chamados/juizes.py:315`) |

Esses são os comportamentos que preservam a hora local mencionados no comentário de `juizes.py:701`; não existem três incrementos independentes de data. (`app/chamados/juizes.py:701`; `app/chamados/juizes.py:309`; `app/chamados/juizes.py:320`)

**O ajuste interpreta `data_evento` como âncora inicial do turno previsto.** `marcos_turno` começa nessa data e avança quando o próximo marco não é posterior ao anterior. Assim, recebendo D+1 e marcos 19:00→07:00, o envelope vai de D+1 à noite até D+2 pela manhã; a resposta 07:00 é escolhida em D+2. (`app/chamados/juizes.py:305`; `app/escala/servico_jornada.py:74`; `app/escala/servico_jornada.py:81`; `app/chamados/juizes.py:310`)

O primeiro avanço pode ocorrer tanto no detector da API, que grava a saída esperada no contexto, quanto no agrupador por entrada, que chama `resolver_data_evento`. O segundo ocorre ao reancorar a cadeia em uma data que já era a da saída. **Mecanismo confirmado no código; atribuição desse caminho ao registro observado em produção: [nao verificado].** (`app/chamados/detector_anomalias.py:45`; `app/chamados/services/disputa_emissao.py:1041`; `app/escala/servico_jornada.py:491`; `app/chamados/juizes.py:310`)

Esse resultado depende de haver marcos compatíveis na data recebida e de as guardas permitirem a materialização; sem marcos, uma saída S não ganha automaticamente um dia. (`app/chamados/juizes.py:306`; `app/chamados/juizes.py:315`; `app/chamados/services/materializacao.py:1168`; `app/chamados/services/materializacao.py:850`)

4. **Os leitores divergem na premissa sobre a coluna.** Há leitores de dia da jornada/célula, leitores de dia civil da batida e leitores que aceitam deliberadamente múltiplas datas. (`app/chamados/juizes.py:371`; `app/chamados/services/fio.py:45`; `app/chamados/juizes.py:263`)

| Leitor | Premissa/comportamento |
|---|---|
| `_slot_esperado` | Para `hora`, herda a âncora de turno do plantio. Para `data_hora`, lê o instante completo da resposta e não aplica o ajuste. (`app/chamados/juizes.py:331`; `app/chamados/juizes.py:345`) |
| `estado_aplicacao_pergunta` | Antes do carimbo, usa o mesmo slot do plantio. Depois do carimbo pode retornar “aplicada” sem recalcular o slot. (`app/chamados/services/materializacao.py:433`; `app/chamados/services/materializacao.py:443`) |
| `_turno_tem_saida` | Trata como **dia do turno**: exige `t.data_turno == p.data_evento`. (`app/chamados/juizes.py:362`; `app/chamados/juizes.py:371`) |
| `via_pelo_fato` | Para adesão, consulta batidas no **dia civil**; para completude, procura `TurnoMaterializado.data_turno=data_evento`. Mistura os dois recortes. (`app/chamados/juizes.py:1447`; `app/chamados/juizes.py:1460`; `app/chamados/juizes.py:1479`) |
| `_pergunta_materializada_de_fato` / `_plantio_na_hora_do_slot` | Para batida vinculada de resposta `hora`, aceita a hora em `data_evento −1/0/+1`, com tolerância de 20 minutos. Para `data_hora`, exige o instante completo. (`app/chamados/juizes.py:259`; `app/chamados/juizes.py:263`; `app/chamados/juizes.py:718`) |
| `_pergunta_viva_calc`, `_celulas_da_pergunta`, régua de validação | Usam como **dia da célula/jornada** para classificar falta ou carregar marcos. A célula da FK só alimenta o julgamento quando sua data coincide. (`app/chamados/juizes.py:622`; `app/chamados/juizes.py:1197`; `app/chamados/juizes.py:1523`) |
| `data_do_fato_de`, filtros e modal | Devolvem a coluna diretamente; a apresentação afirma que é o dia onde a batida nasce. A comparação com o dia do chamado é por igualdade. (`app/chamados/services/fio.py:45`; `app/chamados/services/fio.py:87`; `app/chamados/templatetags/chamados_extras.py:30`; `app/templates/chamados/partials/modal_fio.html:161`) |
| Gate `DATA_EM_DISPUTA` e conferência | Bloqueia diferença com o dia do chamado até conferência. Conferir carimba a aceitação; não normaliza a coluna pelo turno. (`app/chamados/services/materializacao.py:1166`; `app/chamados/services/validacao.py:611`) |
| `resolvedora_valida` | Aceita igualdade ou diferença de um dia nos dois sentidos quando o turno do menor dia cruza a meia-noite; também permite quando esse cruzamento é indeterminado. (`app/chamados/juizes.py:1982`; `app/chamados/juizes.py:1991`) |
| Passivo validado: `_dia_do_plantio`, `_dias_do_par` | Declara explicitamente **dia da célula**: entrada 19:00 e saída 07:00 devem compartilhar `data_evento`, embora tenham timestamps em dias diferentes. (`app/chamados/services/passivo_validado.py:98`; `app/chamados/services/passivo_validado.py:463`) |
| Reconciliação: `lavrar_sem_turno_aberto`, `veredito_orfa` | Usa como **dia do turno** para consultar abertura e encerramento. (`app/chamados/reconciliador.py:369`; `app/chamados/reconciliador.py:1071`) |
| Acordar/semear células, fábrica e pergunta no app | Busca ou agrupa célula por igualdade com `data_evento`: premissa de **dia da célula**. (`app/chamados/models.py:2671`; `app/chamados/reconciliador.py:1291`; `app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/pergunta_no_app.py:109`) |
| Ausência e aceite de folga | Decide a jornada diretamente nesse dia: cria ausência/declara folga em `data_evento`. (`app/chamados/services/materializacao.py:472`; `app/chamados/services/materializacao.py:516`; `app/chamados/services/validacao.py:250`) |
| `classe_do_lote` | Combina diretamente `data_evento + HH:MM` para procurar conflito, sem ajuste overnight nesse trecho: premissa de **dia civil da batida**. (`app/chamados/services/validacao.py:824`) |
| `vigia_carimbo` | Consulta o juiz e o mesmo `_slot_esperado`; herda suas premissas e a aceitação da batida vinculada. (`app/chamados/services/vigia_carimbo.py:94`; `app/chamados/services/vigia_carimbo.py:105`) |
| `vigia_fio`, tranca, competência e expiração | Usa `data_evento` como dia financeiro da jornada; o censo principal do fio agrupa por `celula_id`. (`app/chamados/services/vigia_fio.py:400`; `app/chamados/services/vigia_fio.py:815`; `app/chamados/juizes.py:931`; `app/chamados/services/competencia_trancada.py:106`; `app/chamados/veredito_lavrado.py:95`) |
| Recalcular após validação | Passa `perg.data_evento` ao recalculo por evento, como dia do fato. (`app/chamados/services/validacao.py:404`) |
| Calendário, prévia, consulta do dia, API e atendimento | Indexam, filtram ou exibem a coluna diretamente, sem deslocamento overnight nesses pontos. (`app/colaboradores/services/calendario.py:472`; `app/folha/previa.py:64`; `app/ponto/services/dia_do_colab.py:64`; `app/api/views.py:1168`; `app/api/views_mensageria.py:568`; `app/templates/chamados/partials/_card_atendimento.html:87`) |
| Dia/competência do chamado e auditoria | Datas das perguntas alimentam o dia/competência do chamado; a auditoria procura chamado/célula nesse mesmo dia. (`app/chamados/juizes.py:70`; `app/chamados/juizes.py:203`; `app/chamados/juizes.py:2024`; `app/chamados/management/commands/auditar_invariantes_chamados.py:63`) |

5. O predicado **executado** pelo vigia não é o predicado de horário descrito no cabeçalho. O cabeçalho fala em batida anterior à entrada prevista por mais de seis horas; o corpo acusa um turno que contém plantio aprovado e cuja data é folga segundo o vínculo. (`app/ponto/management/commands/vigia_plantio_overnight.py:8`; `app/ponto/management/commands/vigia_plantio_overnight.py:19`)

A seleção efetiva é:

- Selecionar colaboradores com batida de origem `disputa_s84_retro`, aprovada, com timestamp a partir de `now() − dias`. (`app/ponto/management/commands/vigia_plantio_overnight.py:16`; `app/ponto/management/commands/vigia_plantio_overnight.

[truncado em 20000 de 27659 caracteres]

## 06/11 18:45 · sessao 96f30b8ed283 · - · - · origem=Ronald (tmux)

Censo persistente: **2 sítios físicos** — nascimento em `app/chamados/models.py:2476` e preenchimento de pergunta antiga sem data em `app/chamados/services/fio.py:832-848`. Há **6 chamadas a `PerguntaDisputa.abrir`** e **4 chamadas de reapontamento com data explícita**, detalhadas abaixo.

| Sítio | Motivos | Fonte da data | Cobra saída overnight? | Coerência com `_T1` |
|---|---|---|---|---|
| `models.py::abrir`, `app/chamados/models.py:2476` | Qualquer motivo recebido | Parâmetro; ausente vira `localdate()` em `:2424-2427` | Conforme chamador | Não normaliza pelo turno; depende do chamador (`:2414-2431`) |
| `fio.py::reapontar_resolvedora`, `app/chamados/services/fio.py:832-848` | Mantém motivo existente | Parâmetro, **somente se pergunta não tiver data** (`:832-835`) | Conforme pergunta | Consulta `resolvedora_valida` (`:838`), mas não deriva data pelo `_T1` |
| `gerar_perguntas_disputa`, `app/chamados/services/disputa_emissao.py:389-396` | Motivos do catálogo; inexistente é ignorado (`:294-296`; catálogo `app/chamados/catalogo/perguntas.py:34-139`) | Grade → contexto do motivo → data local de `ultima_batida`; chamado derivado quando aplicável (`:319-396`) | Sim para `orfao_14h` | **Mista**, conforme desdobramento abaixo |
| `_criar_pergunta_se_couber`, `app/chamados/services/disputa_emissao.py:770-777` | `orfao_14h`, `saida_sem_entrada`, `intervalo_saida`, `intervalo_volta`, conforme chamadores (`:809-813`, `:985-993`, `:1038-1048`, `:1071-1080`) | Parâmetro; quatro fontes desdobradas abaixo | Sim | **Mista** |
| `criar_disputa_ancora_de_chamado`, `app/chamados/services/disputa_emissao.py:1195-1202` | `ancora_ausente` | Sem parâmetro: relógio de `abrir` (`models.py:2424-2427`) | Não; pergunta pela primeira entrada (`disputa_emissao.py:1132-1134`) | Não aplicável: instante completo vem da resposta (`:1199`) |
| `declarar_hora_colab`, `app/chamados/services/declaracao_texto.py:325-326` | Quatro motivos do mapa `:42-46` | `dia` do marco faltante escolhido (`:288-300`) | Sim para marco `fim` | Coerente com a célula: `_marcos_faltantes` devolve `data_dia`, inclusive véspera (`:88-123`) |
| `aplicar_declaracao`, `app/chamados/services/declaracao_texto.py:614-615` | Quatro motivos do mapa `:42-46` | `p['dia']`, proposta casada com marco faltante (`:566-586`) | Sim para `fim` | Mesma semântica de célula da anterior |
| `acao_reabrir_disputa_admin`, `app/chamados/services/acoes_disputa.py:259-264` | Motivo escolhido entre `MOTIVOS_CHOICES` (`:245-249`) | Sem parâmetro: relógio de `abrir` (`models.py:2424-2427`) | Não nesta pergunta; `tipo_resposta='escolha'` (`acoes_disputa.py:262`) | Não aplicável |
| Reapontamento c2, `app/chamados/reconciliador.py:937-943` | Grupo dia: quatro motivos (`:786-800`; `models.py:3048-3049`) | `data_ctx`, derivada do argumento `dia` (`reconciliador.py:801`) | Sim | Não normaliza; preserva dia recebido |
| Reapontamento histórico, `app/chamados/reconciliador.py:1620-1626` | Motivo da pergunta (`:1541-1542`) | Data da pergunta; ausente, contexto do motivo (`:1552-1553`) | Conforme pergunta | Mista; depende da fonte anterior |
| Reapontamento derivado, `app/chamados/reconciliador.py:1638` | Motivo da pergunta (`:1541-1542`) | `_derivar_chamado_destino`: data da pergunta/contexto; depois dia do chamado (`disputa_emissao.py:174-179`, `:228-237`) | Conforme pergunta | Mista |
| Reapontamento de declaração, `app/chamados/services/declaracao_texto.py:320` | Quatro motivos do mapa `:42-46` | `dia` do marco escolhido (`:299-300`) | Conforme marco | Dia da célula; mesma fonte do nascimento `:325-326` |

**Desdobramento do gerador genérico (`disputa_emissao.py:389`):**

- **Célula:** `fabrica_da_celula.perguntar` injeta `dia` no contexto (`app/chamados/services/fabrica_da_celula.py:129-141`); no julgamento, recebe `cel.data` (`:174-176`). Os motivos saem dos marcos faltantes da ata (`:47-64`). Pode cobrar saída overnight e usa o dia da célula, coerente com a unidade do turno (`app/chamados/services/declaracao_texto.py:42-46`, `:88-123`).
- **Pendências:** `perguntar_entradas_pendentes` lê cada `entradas_pendentes[*].data`, classifica e injeta essa data (`app/chamados/services/disputa_emissao.py:257-274`). O produtor grava **dia local da entrada**, não dia do relógio (`app/ponto/utils_turno_orfao.py:31-40`).
- **Detector de órfão da API:** `detector_anomalias` grava em `contexto['orfao_14h']['data']` a **data da saída esperada** (`app/chamados/detector_anomalias.py:31-50`). Usa `escala.utils.data_saida_esperada`, que encadeia marcos e pode avançar o dia (`app/escala/utils.py:936-939`, `:950-969`). Portanto pode entregar D+1; diverge da semântica de dia da célula/entrada.
- **API sem data específica:** o contexto carrega `ultima_batida.timestamp=agora.isoformat()` (`app/api/views.py:772-779`; clone `app/api/views_core.py:772-779`), e o gerador usa a data local desse timestamp (`app/chamados/services/disputa_emissao.py:350-364`). É data do relógio da requisição; não há normalização pelo turno.
- **PWA, entrada faltante:** `saida_sem_entrada.data=agora_local.date()` (`app/ponto/services/triagem_batida.py:649-660`), consumida como contexto (`app/chamados/services/disputa_emissao.py:343-347`). Cobra entrada, não saída.
- **Conversa sem data:** termina no relógio `localdate()` do nascimento (`app/chamados/models.py:2424-2427`).
- **Smoke fora da pasta de testes:** injeta data literal `"2026-05-15"` para perguntas com `{data}` e chama o gerador (`app/chamados/management/commands/smoke_chamados.py:90-102`); sem placeholder, cai nas outras fontes/fallback do gerador (`app/chamados/services/disputa_emissao.py:343-396`).
- **Lote de competência:** delega a `fabrica_da_celula.perguntar(colab, dia, [motivo], ...)` (`app/chamados/management/commands/disparar_perguntas_competencia.py:268-280`), logo segue o ramo de dia explícito da célula (`app/chamados/services/fabrica_da_celula.py:129-141`).
- **Retro de selfie:** fornece `batida_em`, mas não `data` nem `ultima_batida` (`app/ponto/management/commands/marcar_foto_ausente_retro.py:96-104`); o gerador não lê `batida_em` para datar (`app/chamados/services/disputa_emissao.py:343-364`), portanto pode cair no relógio do nascimento (`app/chamados/models.py:2424-2427`).

**Desdobramento do helper de órfãos (`disputa_emissao.py:770`):**

| Fonte | Evidência | Motivos / semântica |
|---|---|---|
| Complemento de marcos | `app/chamados/services/disputa_emissao.py:806-813` | Quatro motivos via mapa; `dia` da célula. Chamado já perguntado usa `_ddc(contexto)` ou data da pergunta em `:913-920`; ramo por ata usa dia do chamado em `:1006-1014`. |
| Intervalo dedicado | `app/chamados/services/disputa_emissao.py:982-993` | `intervalo_saida`/`intervalo_volta`; data do próprio marco no contexto (`:985-987`). |
| Grupo por batida de origem | `app/chamados/services/disputa_emissao.py:1037-1048` | Classifica dia **da entrada**, mas grava `resolver_data_evento` ou dia da entrada. Para `orfao_14h`, resolver devolve **data da saída esperada** (`app/escala/servico_jornada.py:483-491`), calculada pelo fim do turno (`:200-209`) e encadeamento com +1 (`:61-85`, `:89-93`). Este é o ramo D→D+1 explícito. |
| Sem origem | `app/chamados/services/disputa_emissao.py:1065-1084` | Data de `_ddc(contexto_json)`; classifica esse dia, cria âncora e complementos. Hierarquia: `data_turno`, `data_batida`, `entrada_timestamp`, pendentes BR, `horario_previsto` (`app/chamados/data_evento.py:16-39`). |

O predicado de preenchimento em `reapontar_resolvedora` impede que seus quatro chamadores substituam uma `data_evento` já preenchida (`app/chamados/services/fio.py:832-835`). A atribuição `models.py:2418` pertence a `_Proxy`, sem persistência (`app/chamados/models.py:2414-2419`); `fio.py:843` apenas desfaz a atribuição em memória antes do retorno de recusa (`app/chamados/services/fio.py:841-844`).

Contagem consolidada: **2 escritores físicos persistentes; 6 sítios de nascimento por `abrir`; 4 delegações de reapontamento com data explícita** (`app/chamados/models.py:2476`; `app/chamados/services/fio.py:834-848`; seis e quatro sítios enumerados na tabela). A busca fora de testes/migrations não encontrou outro `create`, `get_or_create`, `defaults`, `bulk_create`, `bulk_update`, `setattr` ou `.update()` que grave `data_evento` [nao verificado por referência estática única]. O P13 lista apenas `models.py` e `services/fio.py`, mas seu detector cobre somente atribuição de atributo e `.update(...)`, portanto sozinho não prova o censo ampliado (`app/core/tests/test_contract_escritores_estado.py:60-62`, `:177-181`).

## 06/11 18:44 · sessao 38685ef3f498 · - · - · origem=Ronald (tmux)

_T1 é a constante `core/juizes.py:368`: `_T1 = 'a que turno (data_turno) pertence a batida?'`. O registro aponta para **`turnos_do_colab`**, em `app/core/juizes.py:195-196`, assinatura **`turnos_do_colab(colab, ini, fim, celulas=None)`**, em `app/ponto/turnos.py:1848`. Ela entrega turnos com `data_turno` na janela e passa ao corpo `_turnos_do_colab_puro`, em `app/ponto/turnos.py:1849-1857,1913-1916`; este passa ao pareador e filtra pelo `data_turno`, em `app/ponto/turnos.py:1990-1993`.

O mapa para apresentação é **`dia_das_batidas(colab, ini, fim, turnos=None)`**, em `app/ponto/turnos.py:2874`. Seu contrato explicitamente remete ao _T1: batida reclamada pertence a `t.data_turno`; não reclamada pertence a `data_local(timestamp)`, em `app/ponto/turnos.py:2877-2881`. O corpo é **`_dia_das_batidas_puro(turnos, batidas_da_janela)`**, em `app/ponto/turnos.py:2908`, com a atribuição exata `out[b.pk] = t.data_turno`, em `app/ponto/turnos.py:2915-2916`, e `setdefault(..., data_local(...))`, em `app/ponto/turnos.py:2917-2918`.

Chamadores diretos de `dia_das_batidas` fora de testes/migrations: espelho do app (`app/api/views.py:1446`), justificativas do app pelo alias `_ddb_j` (`app/api/views.py:2037-2038`) e espelho da tela (`app/ponto/services/espelho.py:450-451`). O lote oferece `CargaDaJanela.dia_das_batidas(self, colab)` e chama o mesmo corpo (`app/ponto/turnos.py:3007-3010`); o painel o lê em `app/core/services/painel_op.py:330`.

Chamadores diretos de **`turnos_do_colab`** encontrados, incluindo aliases, fora de testes/migrations: [nao verificado]

- No próprio módulo: `app/ponto/turnos.py:778,2096,2148,2179,2901,3033`.
- Ponto: `app/ponto/nucleo.py:47`; `app/ponto/turno_leitura.py:89,95`; `app/ponto/views.py:377,1585,3177`; `app/ponto/services/espelho.py:614`; `app/ponto/services/bordas_realizado.py:66`; `app/ponto/services/teto_na_lista.py:68`; `app/ponto/services/vigia_pareador_celula.py:97`; `app/ponto/services/esmeril_espelho.py:98`; `app/ponto/services/triagem_batida.py:552`; `app/ponto/services/regularizacao.py:61`; `app/ponto/services/passivo_s84.py:82`.
- Comandos: `app/ponto/management/commands/detectar_par_relampago.py:101`; `app/ponto/management/commands/dna_x_batida_real.py:69`; `app/ponto/management/commands/marcar_abandono_provisorio.py:46`; `app/ponto/management/commands/detectar_ausencias.py:336`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`; `app/ponto/management/commands/vigia_plantio_overnight.py:38`; `app/ponto/management/commands/verificar_nucleo.py:29`; `app/ponto/management/commands/e6_oraculo.py:151`; `app/ponto/management/commands/censo_fase_12x36.py:180`.
- Chamados: `app/chamados/models.py:1273`; `app/chamados/juizes.py:371,1842`; `app/chamados/detector_anomalias.py:155`; `app/chamados/services/materializacao.py:123,220`; `app/chamados/services/disputa_emissao.py:644`.
- Escala: `app/escala/utils.py:1602,1919`; `app/escala/regua_defesa.py:358`; `app/escala/servico_jornada.py:250,392`; `app/escala/services/escala_auto.py:87`; `app/escala/services/impacto_troca.py:18`.
- Outros: `app/api/views.py:1449`; `app/relatorios/pdf_espelho.py:213,451`; `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:124`; `app/colaboradores/services/drawer.py:45`.

**Vigia:** o corpo vigente difere do cabeçalho antigo que descreve comparação de hora com entrada prevista: `app/ponto/management/commands/vigia_plantio_overnight.py:8-10,19-23`. O predicado executado é:

1. Candidato: colaborador com batida `origem__in=('disputa_s84_retro',)`, `aprovada=True`, `timestamp >= now()-dias`, em `app/ponto/management/commands/vigia_plantio_overnight.py:16,30-32`.
2. Carrega `turnos_do_colab(colab, hoje-dias, hoje)`, seleciona no turno batidas de mesma origem aprovadas e exige `t.data_turno is not None`, em `app/ponto/management/commands/vigia_plantio_overnight.py:26-29,38-41`.
3. Exige vínculo e tipo; calcula `ec.eh_dia_trabalho(t.data_turno)`; **só emite quando `trab is False`**, em `app/ponto/management/commands/vigia_plantio_overnight.py:42-56`.
4. Não consulta `PerguntaDisputa`, `data_evento`, entrada que a saída deveria fechar, distância de 24h nem exige cruzamento de meia-noite: seu corpo completo vai de `app/ponto/management/commands/vigia_plantio_overnight.py:19` a `:57`.

Logo, ele **não detecta necessariamente** o deslocamento “um dia depois”: se a batida errada receber `data_turno=D+2` e esse dia for trabalho no 12x36, `trab is False` falha; o dia pretendido D não é comparado (`app/ponto/management/commands/vigia_plantio_overnight.py:38-56`). Há caminho concreto para `data_turno=D+2`: com mapa de DNA e sem marcos em D+1, `_vespera` retorna `None` (`app/ponto/turnos.py:1297-1299`); `_data_do_turno` sem véspera retorna o dia local da referência (`app/ponto/turnos.py:283-286`). O mapa só inclui DNA com `marcos` (`app/escala/utils.py:103-108`). Isso explica a cegueira estrutural; **o retorno efetivo do pareador no caso de produção continua [nao verificado]**. Não afirmar universalmente que toda S D+2 escapará: sem mapa/na configuração alternativa o pareador pode atribuí-la à véspera (`app/ponto/turnos.py:1297-1300,291-296`).

**Tipo E/S do app:** não há resposta binária “sempre aceita” ou “sempre deriva”. A API recebe e valida `'E'/'S'` (`app/api/views.py:432,438-439`; clone `app/api/views_core.py:419,425-426`).

- Ao vivo, `abs(agora-ts_efetivo) <= 300s`: chama `proximo_tipo_de(colab, ts_efetivo)` e substitui o tipo divergente, em `app/api/views.py:600-608`; clone `app/api/views_core.py:592-600`.
- Fora disso: busca vínculo do dia local, obtém marcos do template no dia, chama `tipo_por_marcos(...)` e pode substituir o cliente, em `app/api/views.py:609-631`; clone `app/api/views_core.py:601-623`.
- `tipo_por_marcos` devolve o único tipo casado em até 30 minutos; sem marcos, sem casamento ou ambiguidade conserva o cliente, em `app/ponto/turnos.py:2422-2445`.
- A view passa o tipo **já julgado** a `registrar_batida`, em `app/api/views.py:683-684`; clone `app/api/views_core.py:680-681`.
- `registrar_batida(colaborador, timestamp, tipo, origem, *, autoridade_humana=False, fato_de_chao=None, **campos)` não deriva E/S: dedup pode devolver existente (`app/ponto/registro_batida.py:94,113-122`), e os dois `create` gravam diretamente `tipo=tipo` (`app/ponto/registro_batida.py:152-158`).
- `proximo_tipo_de` dá E sem turno; S aberto fora de intervalo; em intervalo desempata pela proximidade aos marcos; intermitente tem ramo de alternância, em `app/ponto/turnos.py:2448-2493`.

**PERGUNTA:** “A batida materializada está no dia/instante do turno que a pergunta pretendia completar?” não é a pergunta executada pelo vigia; este pergunta se o **turno resolvido atual** está em dia que o vínculo diz folga (`app/ponto/management/commands/vigia_plantio_overnight.py:19-23,38-56`).

## 06/11 18:44 · sessao c3c6441f8b27 · - · - · origem=Ronald (tmux)

Leitores relevantes e premissas, somente leitura:

| Leitor | O que faz com `data_evento` / premissa |
|---|---|
| `_ts_tipo_do_plantio` | Usa como data-base do turno: busca régua e grade do dia, combina HH:MM nela e chama ajuste overnight; admite resultado no dia seguinte. `app/chamados/services/materializacao.py:279-297`, `:344-347`, `:370-372`, `:413-420`; ajuste em `app/chamados/juizes.py:303-314`. |
| `_slot_esperado` | Para resposta `hora`, delega ao plantio, herdando a premissa acima. Para `data_hora`, a data vem da resposta completa, sem ajuste. `app/chamados/juizes.py:331-347`. |
| `estado_aplicacao_pergunta` | Quando ainda não materializada, deriva exatamente pelo plantio e procura batida/vizinha naquele timestamp. Se já carimbada, pode retornar “aplicada” antes de derivar. `app/chamados/services/materializacao.py:431-452`. |
| `_turno_tem_saida` | Assume **dia do turno/entrada**: pede `turnos_do_colab(colab, dia, dia)` e exige `t.data_turno == p.data_evento`. `app/chamados/juizes.py:362-372`. |
| `via_pelo_fato` | Mistura dois recortes declarados: para adesão, qualquer batida apurável que toque o **dia civil**; para completude, `TurnoMaterializado.data_turno == data_evento`, isto é, **dia do turno**. `app/chamados/juizes.py:1447-1462`, `:1470-1485`. |
| `_pergunta_materializada_de_fato` / `_plantio_na_hora_do_slot` | Deriva slot; para plantadas vinculadas de resposta `hora`, aceita mesma hora em `data_evento -1/0/+1`, com ±20 minutos. Para `data_hora`, exige instante completo. Portanto esse termo deliberadamente não revalida uma data única. `app/chamados/juizes.py:242-268`, `:701-725`. |
| `_pergunta_viva_calc` | Na ausência de prova suficiente, chama `classificar_falta(colab, p.data_evento)`; usa como **dia da célula/jornada**. `app/chamados/juizes.py:1197-1203`; classificação por turno em `app/escala/servico_jornada.py:392-393`. |
| `_celulas_da_pergunta` | Só alimenta a classificação quando `p.celula.data == p.data_evento`, reforçando a identidade **dia da célula**; caso contrário não usa a FK como insumo. `app/chamados/juizes.py:612-629`. |
| `data_do_fato_de` | Devolve o próprio `p.data_evento` e compara por igualdade com dia do chamado. A prosa diz “é nela que a materialização planta a batida”: premissa de **dia do evento faltante**, embora o ajuste permita outro dia. `app/chamados/services/fio.py:43-52`, `:84-88`. |
| Tela do fio / filtros | `data_do_fato` delega à função acima; divergência mostra os dois dias. A tela explicita “Dia da pergunta … (é aqui que a batida nasce)”: premissa de **dia civil do plantio**. `app/chamados/templatetags/chamados_extras.py:15-31`, `:37-58`; `app/templates/chamados/partials/modal_fio.html:161-167`. |
| Gate `DATA_EM_DISPUTA` / conferência | Materialização bloqueia diferença entre `data_evento` e dia do chamado até conferência; a conferência mantém `data_evento`, apenas carimba que ele vale. `app/chamados/services/materializacao.py:1166-1173`; `app/chamados/services/validacao.py:611-627`. |
| `resolvedora_pertence_ao_turno` | Não exige uma identidade única: aceita igualdade; aceita diferença ±1 se o turno do menor dia cruza a meia-noite **ou se isso é indeterminado**. Reconhece explicitamente que perguntas/chamados podem usar entrada ou saída. `app/chamados/juizes.py:1982-1997`. |
| `_dia_do_plantio` / `_dias_do_par` do passivo validado | Premissa explícita **dia da célula/entrada**: saída overnight do dia 20 nasce no dia 21, mas competência é a do dia 20; entrada 19:00 e saída 07:00 têm o **mesmo** `data_evento`. `app/chamados/services/passivo_validado.py:98-113`, `:460-466`. |
| `lavrar_sem_turno_aberto` / `veredito_orfa` | Usam como **dia do turno** para verificar turno aberto e encerramento temporal. `app/chamados/reconciliador.py:369-372`, `:1071-1082`. |
| `acordar_celulas` / semeadura | Preferem FK da célula; sem ela buscam `(colaborador, data_evento)`, assumindo **dia da célula**. `app/chamados/models.py:2671-2684`; `app/chamados/reconciliador.py:1291-1299`. |
| Fábrica da célula / pergunta no app | Identificam perguntas do dia por igualdade `data_evento == cel.data` ou dia da ata; premissa **dia da célula**. `app/chamados/services/fabrica_da_celula.py:118-126`; `app/chamados/services/pergunta_no_app.py:92-110`. |
| Materialização de ausência / aceite de folga | Cria ausência ou declara folga diretamente em `p.data_evento`; trata como **dia da jornada a decidir**, não timestamp da saída. `app/chamados/services/materializacao.py:472-485`, `:516-532`; `app/chamados/services/validacao.py:250-271`. |
| Régua de validação | `_marco_esperado_da_grade` usa como dia dos marcos da escala; fallback de `motivo_implausivel_validacao`. `app/chamados/juizes.py:1517-1543`, `:1700-1709`. |
| `classe_do_lote` | Combina **diretamente** `data_evento + HH:MM` para procurar conflito ±`CONFLITO_LOTE_MIN`; não passa pelo ajuste overnight nesse trecho. Premissa **dia civil da batida**. `app/chamados/services/validacao.py:824-829`. |
| `vigia_carimbo` | Lê o juiz e `_slot_esperado`; herda o mesmo cálculo de data do plantio. `app/chamados/services/vigia_carimbo.py:94-116`. |
| `vigia_fio` | Censo principal agrupa por `p.celula_id`, não por data; medição de dono/exportação usa competência de `p.data_evento`, assumindo **dia financeiro da jornada**. `app/chamados/services/vigia_fio.py:400`, `:466`, `:616`, `:815-817`. |
| `vigia_plantio_overnight` | Não lê `PerguntaDisputa.data_evento` nem `pergunta_origem`: resolve o `data_turno` da própria batida e acusa somente `ec.eh_dia_trabalho(t.data_turno) is False`. `app/ponto/management/commands/vigia_plantio_overnight.py:30-49`. |
| Tranca / exportação / prazo | Usam diretamente como dia da competência: `dia_da_pergunta_trancado`, seleção de perguntas da competência e `expira_em`. Premissa **dia financeiro da jornada**; não recalculam a data pelo timestamp ajustado. `app/chamados/juizes.py:931-945`; `app/chamados/services/competencia_trancada.py:106-108`; `app/chamados/veredito_lavrado.py:95-116`. |
| Recalcular após validar | Passa `perg.data_evento` ao recalculo por evento, como dia do fato financeiro. `app/chamados/services/validacao.py:404-408`. |
| Calendário / prévia da folha | Indexam perguntas pelo dia `data_evento`; premissa **dia da célula apresentada/apurada**. `app/colaboradores/services/calendario.py:472-477`; `app/folha/previa.py:64-67`. |
| API / atendimento / consulta do dia | API serializa a data sem conversão; cards a exibem; `dia_do_colab` lista perguntas por igualdade à data pedida. Não aplicam ajuste overnight. `app/api/views.py:1168-1169`; `app/api/views_mensageria.py:568`; `app/templates/chamados/partials/_card_atendimento.html:87-90`; `app/ponto/services/dia_do_colab.py:64-71`. |
| Dia/competência do próprio chamado | `_dia_da_disputa`, versão em lote e `_dias_das_perguntas_vivas` usam datas das perguntas como fallback/insumo para dia ou competência do chamado. `app/chamados/juizes.py:70-77`, `:203-211`, `:2024-2040`. |
| Auditoria de órfãs | Procura chamado com `contexto_json.data_turno == p.data_evento` **ou** `celula.data == p.data_evento`, assumindo **dia do turno/célula**; cobertura de ausência usa o mesmo dia. `app/chamados/management/commands/auditar_invariantes_chamados.py:63-78`, `:156-163`. |

**Divergência provada:** a fonte `resolver_data_evento(orfao_14h)` entrega **data da saída esperada**, enquanto materializador, completude e passivo usam essa mesma coluna como **data-base da jornada/célula**; telas e conflito do lote a tratam como **dia civil da batida**. `app/escala/servico_jornada.py:483-491`; `app/chamados/services/materializacao.py:414-420`; `app/chamados/juizes.py:371`; `app/chamados/services/passivo_validado.py:463-465`; `app/templates/chamados/partials/modal_fio.html:166`; `app/chamados/services/validacao.py:824-829`.

**PERGUNTA:** o leitor de conflito do lote calcula `data_evento + hora` diretamente, enquanto o slot canônico passa pelo ajuste overnight; os dois leitores podem consultar dias distintos para a mesma resposta. `app/chamados/services/validacao.py:824-829`; `app/chamados/services/materializacao.py:413-420`.

**PERGUNTA:** a prova de materialização por batida vinculada aceita ±1 dia do `data_evento`, portanto não distingue por si só a saída correta D+1 da saída tardia D+2 quando a pergunta está em D+1 e ambas têm a hora declarada. `app/chamados/juizes.py:263-268`, `:718-725`.

## 06/11 18:42 · sessao b78facd1c787 · PAINEL-SITUACIONAL-UM-MONTADOR · censo · origem=Code

A cópia tem **dois montadores com contratos diferentes**: o lote produz 21 chaves; a célula viva produz 18, sem `estado_app`, `em_turno` e `justif_pend`. Só o lote considera o token ao resolver `plataforma`. (`app/colaboradores/services/situacional.py:180–210`; `app/colaboradores/painel_cell.py:41–70`)

Valores atuais, quantidade de colaboradores afetados, comportamento observado em produção e resultado de execução dos testes: **[nao verificado]**. [nao verificado]

1) **Quem monta a linha e os contextos**

Os produtores da linha são `montar_situacional(colabs_qs, corte, agora, f_plataforma='')` e `montar_linha_colab(c, maps=None)`. O primeiro devolve `(linhas, contadores, by_plat)`; o segundo devolve diretamente um dicionário. (`app/colaboradores/services/situacional.py:65,180,230–238`; `app/colaboradores/painel_cell.py:11,41`)

Comparação de **todas as chaves**, agrupando apenas campos com a mesma fonte:

| Campo | `montar_situacional` | `montar_linha_colab` |
|---|---|---|
| `colab` | O objeto `c`. (`app/colaboradores/services/situacional.py:181`) | O objeto `c`. (`app/colaboradores/painel_cell.py:42`) |
| `empresa` | Nome fantasia, senão razão social; `None` sem empresa. (`app/colaboradores/services/situacional.py:182`) | Mesma expressão. (`app/colaboradores/painel_cell.py:43`) |
| `last_login` | `c.usuario.last_login`, senão `None`. (`app/colaboradores/services/situacional.py:178,183`) | Mesma fonte. (`app/colaboradores/painel_cell.py:44`) |
| `l24` | Compara login com **`agora` recebido − 1 dia**. (`app/colaboradores/services/situacional.py:184`) | Compara com **`timezone.now()` interno − 1 dia**. (`app/colaboradores/painel_cell.py:45`) |
| `appver`, `ultimo_ip` | Do device ativo mais recente por `-ultimo_login`; `None` sem device. (`app/colaboradores/services/situacional.py:16–23,185,187`) | Mesma escolha no modo pontual; com `maps`, usa o device fornecido. (`app/colaboradores/painel_cell.py:20–21,28–29,46,48`) |
| `plataforma` | Chama `plataforma_exibicao(dev, principal, tem_batida, token_plat)`. Token: o mais recente por usuário com plataforma não vazia. (`app/colaboradores/services/situacional.py:90–93,186`) | Chama o helper com **três argumentos**, omitindo token. (`app/colaboradores/painel_cell.py:47`) |
| `tem_push` | Existência de assinatura ativa, carregada em conjunto. (`app/colaboradores/services/situacional.py:94,188`) | Existência pontual de assinatura ativa, ou pertencimento ao `push_set` fornecido. (`app/colaboradores/painel_cell.py:22,30,49`) |
| `tem_foto` | `bool(c.foto)`. (`app/colaboradores/services/situacional.py:189`) | Mesma expressão. (`app/colaboradores/painel_cell.py:50`) |
| `ultima_batida` | Última batida crua com `timestamp >= corte`; a consulta não impõe `timestamp <= agora` nem exclui retratadas. (`app/colaboradores/services/situacional.py:137–145,190`) | Última batida crua das **14 horas anteriores ao `now()` interno**, ou timestamp fornecido em `maps`. (`app/colaboradores/painel_cell.py:24,27,33–35,51`) |
| `estado_app` | Sem device → `sem device ativo`; sem adesão viva na vida → `instalou, nunca bateu`; sem adesão viva em `[corte, agora]` → `bateu e parou`; senão `ativo`. (`app/colaboradores/services/situacional.py:115,150,191–194`; `app/ponto/services/adesao.py:48–65`) | **Ausente.** O dicionário completo está em `app/colaboradores/painel_cell.py:41–70`. |
| `geo`, `geo_tipo` | Cor e tipo devolvidos por `_geo_do_pino`, usando coordenadas e accuracy da última batida do corte recebido. (`app/colaboradores/services/situacional.py:137–145,179,195–198`) | Mesmo helper, mas com a batida de 14 h ou coordenadas fornecidas em `maps`. (`app/colaboradores/painel_cell.py:25,27,33–40,52–53`) |
| `b_lat`, `b_lon` | Coordenadas dessa última batida convertidas em string; ausência/valor falso vira `''`. (`app/colaboradores/services/situacional.py:199–200`) | Mesma conversão, com a janela/insumo da célula. (`app/colaboradores/painel_cell.py:39,54–55`) |
| `p_lat`, `p_lon` | Coordenadas do posto em string, ou `''`. (`app/colaboradores/services/situacional.py:201–202`) | Mesmas expressões. (`app/colaboradores/painel_cell.py:56–57`) |
| `p_raio` | `_raio_do_posto(c.posto)`. (`app/colaboradores/services/situacional.py:203`) | Mesmo helper. (`app/colaboradores/painel_cell.py:58`) |
| `em_turno` | Booleano do resultado de `turnos_abertos_de(colabs, agora)`. (`app/colaboradores/services/situacional.py:169–170,208`) | **Ausente.** (`app/colaboradores/painel_cell.py:41–70`) |
| `justif_pend` | Quantidade de justificativas pendentes daquele colaborador, ou zero. (`app/colaboradores/services/situacional.py:124–128,209`) | **Ausente.** (`app/colaboradores/painel_cell.py:41–70`) |
| `led` | Disputa → vermelho; turno pelo carregador plural com `agora` recebido → amarelo; batida do corte → verde; senão cinza. (`app/colaboradores/services/situacional.py:56–62,169–170,210`) | Mesma precedência, mas consulta singular `turno_aberto_de(c, agora=timezone.now())`; verde/cinza depende da batida de 14 h ou de `maps`. (`app/colaboradores/painel_cell.py:68–70`) |

Não há chave exclusiva da célula. **`token_plat` não é chave de saída de nenhum dos dois**: é parâmetro do helper; no lote vem de `token_plat_map`. (`app/colaboradores/services/situacional.py:90–93,180–210`; `app/colaboradores/painel_cell.py:41–70`; `app/colaboradores/views.py:3034`)

O helper resolve plataforma nesta ordem: **token → device → plataforma principal → `desconhecido` se há batida → `None`**. Portanto a divergência não se limita à janela de batidas: a célula também omite a fonte de maior precedência. (`app/colaboradores/views.py:3039–3045`; `app/colaboradores/painel_cell.py:47`)

`maps` exige `devices_map`, `push_set`, `disputa_set`, `batida_recente` e `batida_coord`; não fornece estado do app, token ou instante de referência e não substitui a consulta singular de turno. (`app/colaboradores/painel_cell.py:20–25,45,47,68–70`)

Os demais produtores explícitos de contexto são:

| Função | Contexto produzido |
|---|---|
| `_cell_colab_status` | `{'l': montar_linha_colab(c)}` para `_ps_linha.html`. (`app/colaboradores/painel_cell.py:75–81`) |
| `painel_situacional` | `quadros_barra`, `linhas`, `placar`, `proximo_offset`, `qs_lotes`, `restantes`, `total_linhas`, `total_ativos`, `logaram_24h`, `fora_posto`, `com_push`, `dormindo`, `plat_app`, `plat_apk`, `plat_ios`, `plat_pwa_android`, `plat_pwa_ios`, `plat_nunca`, `op_em_turno`, `op_justif`, `empresas`, `f_periodo`, `f_plataforma`, `f_empresa`, `f_praca`, `empresa_fora_rollout`, `cortados_painel`, `pracas`, `f_q`, `gmaps_key`. (`app/colaboradores/views.py:2696–2727`) |
| `painel_situacional_linhas` | `linhas`, `proximo_offset`, `restantes`, `qs_lotes`, para `_ps_lote.html`. (`app/colaboradores/views.py:2809–2814`) |
| `quadros_barra_situacional` | Quadros com `titulo`/`itens`; itens com `rotulo`, `numero`, `cor`, `attrs` e opcionais `classe`, `negrito_escuro`, `title`; atalhos com `atalho`, `rotulo`, `icone`, e `botao`/`onclick` ou `href`. Alimenta `quadros_barra`. (`app/colaboradores/views.py:2490–2549,2693–2697`) |

O painel e o lote percorrem `linhas`, vinculando cada dicionário a `l` antes de incluir `_ps_linha.html`. `render_cell` apenas encaminha template e contexto para `render_to_string`; não monta outra linha. (`app/templates/colaboradores/partials/painel_situacional.html:300–306`; `app/templates/colaboradores/partials/_ps_lote.html:11`; `app/core/live_cell.py:27–28`)

2) **Chamadores, argumentos e imports**

Chamadas diretas de `montar_linha_colab`:

| Chamador | Argumentos e forma de acesso |
|---|---|
| `_cell_colab_status` | `montar_linha_colab(c)`, com objeto buscado por `pk=ident` e relações empresa/posto/usuário carregadas. (`app/colaboradores/painel_cell.py:75–81`) |
| O200, `_cores` | Import local `from colaboradores import painel_cell`; chama `painel_cell.montar_linha_colab(self.c)`, com `now` interceptado para o instante `agora`. (`app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:105–115`) |
| O200c, `_pino` | Mesmo import local; passa `Colaborador.objects.get(pk=self.c.pk)`, com relógio interceptado. (`app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:137–148`) |
| LED, `_led(agora)` | Mesmo import local; passa `self.c`, com `painel_cell.timezone.now` interceptado para o argumento `agora`. (`app/colaboradores/tests/test_led_pelo_juiz.py:56–67`) |

Essas quatro chamadas omitem `maps`; nenhuma passa corte, instante ou usuário ao montador. (`app/colaboradores/painel_cell.py:11,81`; `app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:115`; `app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:148`; `app/colaboradores/tests/test_led_pelo_juiz.py:67`)

Chamadas diretas de `montar_situacional` em código de produção:

| Chamador | Argumentos |
|---|---|
| `painel_situacional`, com placar | `list(colabs_qs[:30]), corte, agora, f_plataforma`. (`app/colaboradores/views.py:2659,2664–2665`) |
| `painel_situacional`, sem placar | `colabs_qs, corte, agora, f_plataforma`; depois seleciona as primeiras 30 linhas. (`app/colaboradores/views.py:2679–2680`) |
| `painel_situacional_linhas`, com plataforma | Lista dos objetos correspondentes aos IDs do recorte em `[offset:offset+100]`, seguida de `corte, agora, ''`. (`app/colaboradores/views.py:2758,2797–2802`) |
| `painel_situacional_linhas`, sem plataforma | `list(qs[offset:offset+100]), corte, agora, ''`. (`app/colaboradores/views.py:2804–2805`) |
| `lavrar_placar_situacional.Command.handle` | Lista de todo `colaboradores_em_operacao()` com `select_related`; `corte=agora−7 dias`; `agora=timezone.now()`; plataforma omitida. (`app/core/management/commands/lavrar_placar_situacional.py:31,48–56`) |
| Auditor de invariantes, bloco S7 | Lista de todo `colaboradores_em_operacao()` com `select_related`; `_corte7=_agora7−7 dias`; `_agora7=_tz_s7.now()`; plataforma omitida. Só entra na chamada quando há placar global com idade ≤300 s. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1202–1216`) |

Nesses chamadores os imports de `montar_situacional` são **locais e sem apelido**. `_tz_l` e `_tz_s7` são apelidos de `timezone`. (`app/colaboradores/views.py:2645,2749,2752`; `app/core/management/commands/lavrar_placar_situacional.py:44`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1200,1212`)

Chamadas diretas nos testes: [nao verificado]

| Chamador | Argumentos / import |
|---|---|
| `_monta(n)` | `_qs(n)`, `corte=agora−7 dias`, `agora=timezone.now()`; queryset da fixture com `select_related`, ordenado por nome/pk. Import no topo, sem apelido. (`app/colaboradores/tests/test_situacional_pergunta_em_lote.py:29,83–92`) |
| `_situ` | `Colaborador.objects.filter(pk=self.c.pk)`, `agora−7 dias`, `agora`; import local, sem apelido. (`app/colaboradores/tests/test_situacional_pelo_juiz.py:19–23`) |
| O200, `_cores` | Queryset de `self.c.pk`, com `select_related`; `agora−14h`, `agora`; import local. (`app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:112–120`) |
| O200c, `_pino` | Queryset de `self.c.pk`, com `select_related`; `agora−14h`, `agora`; import local. (`app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:145–152`) |
| Adesão, `_estados` | Queryset de `retratado` e `vivo`; `corte=agora−14h`, `agora`; import no topo. (`app/colaboradores/tests/test_adesao_pelo_juiz.py:18,48–53`) |
| Estado do app, `_estados` | Queryset de `retratado`, `vivo`, `misto`; `corte=_dias(30)`, `agora=timezone.now()`; import no topo. (`app/colaboradores/tests/test_estado_app_pelo_juiz.py:23,65–69`) |
| `test_placar_global_bate_com_a_soma_das_linhas` | Lista do universo em operação com `select_related`; `corte=agora−PERIODO_PADRAO_DIAS`, `agora`; import local. (`app/core/tests/test_selo_placar_situacional.py:80–91`) |

Há também execução indireta pelo comando de lavra nos testes do placar e do Haiku; ela chega ao mesmo `handle`, que prepara o universo e os tempos. (`app/core/tests/test_selo_placar_situacional.py:81,103,111,121,124,132,187,199,203,223,237,250`; `app/core/tests/test_placar_situacional_um_so_numero.py:91,126`; `app/core/tests/test_haiku_em_turno_agora.py:65,76`; `app/core/management/commands/lavrar_placar_situacional.py:48–56`)

Nenhuma assinatura recebe usuário: o escopo chega pelo queryset/lista preparado pelo chamador. O universo `colaboradores_em_operacao()` exige colaborador ativo, empresa ativa, usuário com nome de 11 dígitos, empresa em rollout e vínculo que cubra hoje. (`app/colaboradores/services/situacional.py:65`; `app/colaboradores/painel_cell.py:11`; `app/colaboradores/queries.py:25–29,50–65`)

3) **Carga, lote, tempos e célula viva**

Na carga, `agora=timezone.now()`; `periodo='hoje'` significa **últimas 24 h**, `'30d'` significa 30 dias e qualquer outro valor resulta em 7 dias. O padrão do GET é `'7d'`. (`app/colaboradores/views.py:2566,2584–2590`)

O lote calcula **outro `agora` a cada requisição**, por `_tz_l.now()`, e reaplica a mesma regra de corte. Ele não recebe o instante da carga inicial. (`app/colaboradores/views.py:2764,2771–2777,2801–2805`)

Os escopos das duas views têm diferenças concretas:

| Aspecto | Carga | Lote |
|---|---|---|
| Permissão | `login_required` e `tem_acao(user, 'ver_painel_situacional')`. (`app/colaboradores/views.py:2552,2562–2564`) | `login_required` e `is_superuser or is_staff`. (`app/colaboradores/views.py:2739,2754–2756`) |
| Praça | GET, persistindo preferência; sem GET, preferência salva. (`app/colaboradores/views.py:2569–2573`) | GET ou preferência salva. (`app/colaboradores/views.py:2766–2767`) |
| Busca | Nome, CPF **ou matrícula**. (`app/colaboradores/views.py:2620–2624`) | Nome ou CPF. (`app/colaboradores/views.py:2784–2786`) |
| Ordem | Nome, pk. (`app/colaboradores/views.py:2658`) | Nome, pk. (`app/colaboradores/views.py:2787`) |

As URLs da carga e do lote são `/colaboradores/painel-situacional/` e `/colaboradores/painel-situacional/linhas/`. (`app/config/urls.py:26`; `app/colaboradores/urls.py:81–83`)

A cadeia da célula viva é:

- O `<tr>` declara `data-cell="colab-status:<pk>"` e `data-cell-every="45"`. (`app/templates/colaboradores/partials/_ps_linha.html:1`)
- O JS chama **`/live-cell/colab-status/<pk>/`**. A rota importa `live_cell` com o apelido `live_cell_view`; o resolvedor procura o tipo registrado e chama `fn(request, ident)`. (`app/static/js/live-cell.js:10,35–38`; `app/config/urls.py:39`; `app/core/urls.py:4,16`; `app/core/live_cell.py:18–22`)
- `_cell_colab_status(request, ident)` é o renderizador registrado para `colab-status`; busca somente por PK, carrega empresa/posto/usuário e chama `montar_linha_colab(c)`. Não encaminha período, filtros, corte, agora ou usuário ao montador. (`app/colaboradores/painel_cell.py:74–81`)
- A resposta contém um `<tr>` completo. O JS cria um `tbody` temporário e faz `replaceChild(novo, el)`: **substitui a linha inteira, incluindo atributos e todas as células**. Se o colaborador não existe, `None` vira HTTP 204 e o JS remove a linha. (`app/templates/colaboradores/partials/_ps_linha.html:1–21`; `app/static/js/live-cell.js:40–51`; `app/colaboradores/painel_cell.py:79–80`; `app/core/live_cell.py:23–25`)

Os 45 s são cadência **base**, com jitter de 0,75–1,25, condicionada à visibilidade e ao teto de seis requisições simultâneas. (`app/static/js/live-cell.js:11,17,58–76`)

O painel inicia `LiveCell.scan()`. Separadamente, a sentinela dos lotes usa HTMX com `outerHTML`, substituindo a própria sentinela pelas próximas linhas e eventual nova sentinela. (`app/templates/colaboradores/partials/painel_situacional.html:306,327`; `app/templates/colaboradores/partials/_ps_lote.html:11–12`)

4) **Usar `montar_situacional` para um colaborador**

**Sim, aceita queryset de um colaborador; não há dependência de agregados do lote para decidir sua linha.** A função materializa a entrada, restringe os mapas aos IDs/usuários recebidos e monta cada linha antes de calcular os contadores. (`app/colaboradores/services/situacional.py:82–94,115–150,175–238`)

Não há paginação, limite ou ordenação interna de colaboradores: a ordem é a da entrada; o único corte adicional de colaboradores é `f_plataforma`, quando preenchido. O carregador plural de turnos também separa insumos e julgamento por colaborador. (`app/colaboradores/services/situacional.py:82,172–176`; `app/ponto/turnos.py:2324–2362`)

O retorno para um colaborador continua sendo:

- Uma lista `linhas`, com a linha de 21 campos se ele estiver na entrada e passar pelo filtro opcional. (`app/colaboradores/services/situacional.py:172–211`)
- Contadores locais: `total_ativos`, `logaram_24h`, `fora_posto`, `com_push`, `dormindo`, `op_em_turno`, `op_justif`. (`app/colaboradores/services/situacional.py:213–237`)
- `by_plat`, inicializado pelo catálogo de plataformas mais `nunca`, contando a plataforma exibida das linhas retornadas. (`app/colaboradores/services/situacional.py:226–238`)

A extração pode seguir esta forma; com queryset unitário existente e sem filtro de plataforma, o teste O200 já usa `linhas[0]`. (`app/colaboradores/services/situacional.py:65,172–181,230`; `app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:116–124`)

```python
linhas, _, _ = montar_situacional(qs.filter(pk=ident), corte, agora)
linha = next((l for l in linhas if l['colab'].pk == ident), None)
```

Isso **não significa custo zero**: ainda carrega devices, tokens, push, adesão, disputas, justificativas, batidas e turnos, além de calcular os agregados. As relações empresa/posto/usuário são acessadas na montagem; os chamadores de tela usam `select_related` para elas. (`app/colaboradores/services/situacional.py:88–170,178–238`; `app/colaboradores/views.py:2593,2779`)

O teste de custo exige igualdade do número de queries entre **30 e 100** colaboradores; não estabelece custo absoluto nem mede o caso de um. Os testes seguintes verificam equivalência do conjunto de turnos e do campo `em_turno`. (`app/colaboradores/tests/test_situacional_pergunta_em_lote.py:94–130`)

5) **Quem lê os campos afetados**

| Campo / leitor | Uso |
|---|---|
| `estado_app` — partial | Seleciona três rótulos explicitamente; o ramo final escreve `sem device ativo`. Como a linha da célula não fornece a chave, esse é o ramo correspondente à ausência dela. (`app/templates/colaboradores/partials/_ps_linha.html:12`; `app/colaboradores/painel_cell.py:41–70`) |
| `estado_app` — testes | Os testes de adesão e estado do app extraem a chave das linhas do lote e distinguem batidas vivas/retratadas e uso dentro/fora da janela. (`app/colaboradores/tests/test_adesao_pelo_juiz.py:48–70`; `app/colaboradores/tests/test_estado_app_pelo_juiz.py:65–88`) |
| `plataforma` — partial e filtro JS | Alimenta `data-plat` e a pílula; o filtro do navegador lê `data-plat`. (`app/templates/colaboradores/partials/_ps_linha.html:1,11`; `app/templates/colaboradores/partials/painel_situacional.html:56`) |
| `plataforma` — agregação e barra | O serviço soma `by_plat` pela plataforma exibida; a view expõe os contadores e prepara a barra. (`app/colaboradores/services/situacional.py:226–229`; `app/colaboradores/views.py:2525–2532,2693–2695,2709–2714`) |
| `token_plat` — helper compartilhado | É lido por `plataforma_exibicao` e tem precedência sobre device/principal. Os testes próprios cobrem essa precedência e os fallbacks. (`app/colaboradores/views.py:3034–3045`; `app/colaboradores/tests/test_pilula_plataforma.py:9–23`; `app/colaboradores/tests/test_plataforma_pwa.py:45–59`) |
| Plataforma/token — ficha | A ficha busca seu próprio device/token e chama `plataforma_exibicao`; não consome o dicionário de nenhum dos dois montadores. (`app/colaboradores/services/ficha.py:140–151`) |

Há uma distinção adicional: **o filtro de plataforma do servidor usa o device ativo, enquanto a plataforma exibida pode vir do token**. Nesse filtro, `nunca` significa ausência de device ativo; ele não testa ausência de plataforma exibida. (`app/colaboradores/services/situacional.py:16–31,172–186`)

O **lavrador do placar não lê `estado_app`, `plataforma` ou `token_plat`**: chama o serviço 

[truncado em 20000 de 23910 caracteres]

## 06/11 18:39 · sessao 9ad1417ffcd9 · - · - · origem=Ronald (tmux)

Auditoria complementar dos itens 1 e 4:

- São **21 chaves no lote e 18 na célula**; exclusivas de `montar_situacional`: `estado_app`, `em_turno`, `justif_pend`; exclusivas de `montar_linha_colab`: nenhuma. `token_plat` **não é chave de saída** em nenhum montador: é argumento de `plataforma_exibicao`, alimentado no lote pelo mapa de tokens e omitido na célula. (`app/colaboradores/services/situacional.py:180–210`; `app/colaboradores/painel_cell.py:41–70`; `app/colaboradores/views.py:3034–3045`)

- As 18 comuns são: `colab`, `empresa`, `last_login`, `l24`, `appver`, `plataforma`, `ultimo_ip`, `tem_push`, `tem_foto`, `ultima_batida`, `geo`, `geo_tipo`, `b_lat`, `b_lon`, `p_lat`, `p_lon`, `p_raio`, `led`. (`app/colaboradores/painel_cell.py:41–70`; `app/colaboradores/services/situacional.py:180–210`)

- Diferenças de derivação entre chaves comuns: `l24` usa `agora` recebido no lote, `timezone.now()` interno na célula; `plataforma` usa token somente no lote; `ultima_batida`, `b_lat`, `b_lon`, `geo`, `geo_tipo` usam a janela definida pelo `corte` recebido no lote e janela interna de 14h na célula sem `maps`; `led` pergunta ao carregador plural com `agora` no lote e ao singular com `now()` na célula, com verde/cinza dependentes das diferentes janelas de última batida. (`app/colaboradores/services/situacional.py:137–150,169–210`; `app/colaboradores/painel_cell.py:20–70`)

- A célula aceita `maps`, mas exige as entradas `devices_map`, `push_set`, `disputa_set`, `batida_recente`, `batida_coord` por indexação direta; isso só substitui esses insumos, não fornece `estado_app` nem token e não substitui a consulta de turno/relógio do LED. (`app/colaboradores/painel_cell.py:20–25,41–70`)

- Outros campos comuns usam as mesmas expressões/fontes por pessoa: `colab`, `empresa`, `last_login`, `appver`, `ultimo_ip`, `tem_push`, `tem_foto`, `p_lat`, `p_lon`, `p_raio`. Device é escolhido por `ativo=True` e `-ultimo_login` em ambos; push por `ativa=True`. (`app/colaboradores/services/situacional.py:16–23,88–94,181–203`; `app/colaboradores/painel_cell.py:28–30,42–58`)

Produtores diretos de contexto:

- `painel_situacional`: `quadros_barra`, `linhas`, `placar`, `proximo_offset`, `qs_lotes`, `restantes`, `total_linhas`, `total_ativos`, `logaram_24h`, `fora_posto`, `com_push`, `dormindo`, `plat_app`, `plat_apk`, `plat_ios`, `plat_pwa_android`, `plat_pwa_ios`, `plat_nunca`, `op_em_turno`, `op_justif`, `empresas`, `f_periodo`, `f_plataforma`, `f_empresa`, `f_praca`, `empresa_fora_rollout`, `cortados_painel`, `pracas`, `f_q`, `gmaps_key`. (`app/colaboradores/views.py:2696–2727`)
- `painel_situacional_linhas`: `linhas`, `proximo_offset`, `restantes`, `qs_lotes`. (`app/colaboradores/views.py:2809–2814`)
- `_cell_colab_status`: `{'l': montar_linha_colab(c)}`. (`app/colaboradores/painel_cell.py:75–81`)
- `quadros_barra_situacional` produz os dicionários aninhados de `quadros_barra`: quadros com `titulo`/`itens`; itens de operação/adoção com `rotulo`, `numero`, `cor`, `attrs`, opcionalmente `classe`, `negrito_escuro`, `title`; atalhos com `atalho`, `rotulo`, `icone` e `botao`/`onclick` ou `href`. O painel os passa ao include da barra. (`app/colaboradores/views.py:2490–2549,2693–2697`; `app/templates/colaboradores/partials/painel_situacional.html:228`)
- Não confundir `render_cell` com um montador adicional: apenas passa o contexto recebido para `render_to_string`. (`app/core/live_cell.py:27–28`)

Item 4, conclusão e limites:

- Um queryset de um colaborador é suportado: a função converte a entrada em lista, recorta os mapas pelos IDs/usuários dessa lista, mantém a ordem de entrada e não impõe paginação ou limite interno; há somente o filtro opcional de plataforma antes da construção das linhas. (`app/colaboradores/services/situacional.py:82–94,115–150,172–176`)
- Os agregados são calculados **depois** das linhas, a partir delas, e não retroalimentam qualquer campo; portanto o total do universo recebido muda, mas não decide a linha individual. (`app/colaboradores/services/situacional.py:175–238`)
- O turno plural recebe somente os colaboradores fornecidos, carrega os insumos pelos IDs e julga cada colaborador separadamente, armazenando a resposta pelo seu ID. (`app/ponto/turnos.py:2324–2362`)
- Retorno: `(linhas, contadores, by_plat)`; `contadores` contém `total_ativos`, `logaram_24h`, `fora_posto`, `com_push`, `dormindo`, `op_em_turno`, `op_justif`; `by_plat` inicializa as plataformas de `DeviceColaborador.PLATAFORMA_CHOICES` e `nunca`, somando a plataforma exibida nas linhas. (`app/colaboradores/services/situacional.py:213–238`)
- Extração segura conceitual: `linhas, _, _ = montar_situacional(qs.filter(pk=pk), corte, agora)`; `linha = next((l for l in linhas if l['colab'].pk == pk), None)`. Sem filtro de plataforma e com colaborador existente na entrada, haverá a única linha; com filtro de plataforma ele pode ser excluído. (`app/colaboradores/services/situacional.py:65,82–85,172–181,230`)
- Não afirmar “custo zero”: mesmo para um colaborador são carregados mapas, adesão, justificativas, batidas e turno, e calculados contadores/histograma; as relações `empresa`, `posto`, `usuario` são acessadas durante a montagem. Os chamadores de produção já usam `select_related` dessas relações. (`app/colaboradores/services/situacional.py:88–170,178–238`; `app/colaboradores/views.py:2593,2779`; `app/colaboradores/painel_cell.py:77`)
- Teste de custo existente compara **30 e 100** e exige diferença zero no número de queries, sem estabelecer teto absoluto nem medição para um colaborador. Os testes seguintes verificam equivalência do conjunto de turno aberto nos dois sentidos e o veredito `em_turno` na linha, não igualdade de todas as chaves entre os dois montadores. (`app/colaboradores/tests/test_situacional_pergunta_em_lote.py:94–130`)
- Ressalva importante: o filtro `f_plataforma` usa **device ativo** (`nunca` = ausência de device ativo), enquanto a plataforma exibida pode vir do token; são cálculos distintos na mesma função. (`app/colaboradores/services/situacional.py:26–31,172–186`)

## 06/11 18:39 · sessao beb420b7acdb · - · - · origem=Ronald (tmux)

Itens 5–6, com referências sempre relativas à raiz:

- `estado_app` é lido pelo template da linha: três valores explícitos (`ativo`, `bateu e parou`, `instalou, nunca bateu`); qualquer outro caso segue para `sem device ativo`. Portanto a ausência dessa chave no montador da célula alcança o ramo final do template (`app/templates/colaboradores/partials/_ps_linha.html:12`; dicionário sem a chave em `app/colaboradores/painel_cell.py:41`).
- `plataforma` alimenta `data-plat` e a pílula da linha; o JS filtra por esse atributo (`app/templates/colaboradores/partials/_ps_linha.html:1`, `app/templates/colaboradores/partials/_ps_linha.html:11`, `app/templates/colaboradores/partials/painel_situacional.html:56`).
- O serviço conta `by_plat` usando a **plataforma exibida** de cada linha; a view transforma isso em contadores e quadros da barra (`app/colaboradores/services/situacional.py:226`, `app/colaboradores/services/situacional.py:229`, `app/colaboradores/views.py:2693`, `app/colaboradores/views.py:2709`, `app/colaboradores/views.py:2525`).
- O filtro servidor de plataforma usa o **device ativo mais recente**, sem consultar token; `nunca` significa ausência desse device. Esse filtro e a plataforma exibida não seguem a mesma derivação (`app/colaboradores/services/situacional.py:16`, `app/colaboradores/services/situacional.py:26`, `app/colaboradores/services/situacional.py:172`, `app/colaboradores/services/situacional.py:186`).
- `token_plat` não é chave do dicionário da linha: é o quarto argumento de `plataforma_exibicao`, preenchido a partir do token mais recente com plataforma não vazia. A precedência do helper é token → device → `plataforma_principal` → `desconhecido` se há batida, senão `None` (`app/colaboradores/services/situacional.py:90`, `app/colaboradores/services/situacional.py:186`, `app/colaboradores/views.py:3034`).
- `montar_linha_colab` chama esse helper com três argumentos, omitindo token; a ficha do colaborador também chama o helper, mas obtém seu próprio token mais recente, independentemente dos dois montadores (`app/colaboradores/painel_cell.py:47`, `app/colaboradores/services/ficha.py:140`, `app/colaboradores/services/ficha.py:146`). A ficha transforma sua plataforma em texto de cabeçalho (`app/colaboradores/services/ficha.py:214`).
- O lavrador chama `montar_situacional` e ignora `_by_plat`; não lê `estado_app`, `plataforma` ou `token_plat`. Para agrupar/contar lê `colab.empresa_id`, `last_login`, `geo_tipo`, `tem_push`, `em_turno`, `justif_pend` e o tamanho da lista; não depende do formato exclusivo de `montar_linha_colab` (`app/core/management/commands/lavrar_placar_situacional.py:56`, `app/core/management/commands/lavrar_placar_situacional.py:59`, `app/core/management/commands/lavrar_placar_situacional.py:63`).
- Os leitores diretos de `montar_linha_colab` dependem das chaves do seu retorno: render da célula com `l`, testes GEO com `geo`, GEO/raio com `geo_tipo` e `geo`, LED com `led` (`app/colaboradores/painel_cell.py:81`, `app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:115`, `app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:148`, `app/colaboradores/tests/test_led_pelo_juiz.py:67`).
- Os testes de estado do app leem `estado_app` do serviço de lote: adesão retratada versus viva; uso no período retratado versus vivo e última retratada precedida de viva (`app/colaboradores/tests/test_adesao_pelo_juiz.py:48`, `app/colaboradores/tests/test_adesao_pelo_juiz.py:55`, `app/colaboradores/tests/test_estado_app_pelo_juiz.py:65`, `app/colaboradores/tests/test_estado_app_pelo_juiz.py:71`).
- O helper de plataforma tem testes próprios de precedência token/device/fóssil/desconhecido/None e testes de resolução sem token (`app/colaboradores/tests/test_pilula_plataforma.py:9`, `app/colaboradores/tests/test_plataforma_pwa.py:45`).

As três linhas pedidas são chamadas dentro de helpers, não asserções isoladas:

| Referência | O que a chamada extrai e o que os testes afirmam | [nao verificado]
|---|---|
| `app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:115` | `_cores` congela o relógio da célula, extrai `geo`, chama lote para o mesmo colaborador com corte de 14 h, guarda o resumo e retorna as duas cores (`:105`, `:113`, `:119`, `:124`). Asserções: GPS incerto/sinal ruim/accuracy ausente não produzem vermelho; confiantemente fora continua vermelho e contador 1; dentro continua verde; sem coordenada é cinza (`:128`, `:149`, `:162`, `:181`, `:196`, `:215`). |
| `app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:148` | `_pino` congela relógio, busca colaborador atualizado, monta célula e lote com corte de 14 h e retorna pares `(geo_tipo, geo)` (`:137`, `:146`, `:151`, `:154`). Asserções relevantes: raio zero usa padrão, não acusa batida a 50 m/accuracy 10; contador `fora_posto == 0`; raio 100 declarado continua acusando 300 m/accuracy 10; pino e escritor concordam nos casos definidos (`:159`, `:175`, `:190`, `:199`, `:214`). |
| `app/colaboradores/tests/test_led_pelo_juiz.py:67` | `_led` congela `painel_cell.timezone.now` e extrai somente `led` (`:56`, `:66`). Afirma que última batida E com turno expirado não pinta amarelo; turno realmente aberto continua amarelo; ausência de batidas produz cinza (`:69`, `:96`, `:114`). Não chama o montador de lote (`:56`). |

Existe selo explícito das duas raízes **dos montadores GEO**: O200 compara célula/lote em cinco combinações `(metros, accuracy)`; O200c compara o par inteiro `(geo_tipo, geo)` em três raios × três combinações de batida, nove cenários. Isso não verifica paridade de todas as chaves nem de `estado_app`/`plataforma` (`app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:204`, `app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:235`).

Há também a regra e o selo genérico das duas **cascas HTML**, diferentes das duas funções: a regra aponta para o teste que verifica se `core/_dialogo.html` é alcançável por includes a partir de `base.html` e `base_app.html`; o teste seguinte conta exatamente um arquivo dono de `id="hx-confirma-fundo"` (`CLAUDE.md:200`, `app/core/tests/test_selo_modal_nao_nasce_vazio.py:98`, `app/core/tests/test_selo_modal_nao_nasce_vazio.py:115`, `app/core/tests/test_selo_modal_nao_nasce_vazio.py:121`).

## 06/11 18:38 · sessao 94c497bc14a3 · - · - · origem=Ronald (tmux)

**Chamadores diretos de `montar_linha_colab` (nenhum passa `maps`; assinatura `c, maps=None`):**

| Chamador | Argumentos / contexto | Referência |
|---|---|---|
| `_cell_colab_status(request, ident)` | `c`, obtido por `pk=ident`, com `select_related('empresa','posto','usuario')`; sem corte/agora/usuário explícitos | `app/colaboradores/painel_cell.py:75`, `:77`, `:81` |
| `O200...::_cores` | `self.c`; relógio congelado em `agora=timezone.now()`; lê `['geo']` | `app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:105`, `:111`, `:113`, `:115` |
| `O200c...::_pino` | `Colaborador.objects.get(pk=self.c.pk)`; relógio congelado em `agora`; lê `geo_tipo` e `geo` | `app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:137`, `:144`, `:146`, `:148`, `:154` |
| `LedPeloJuizTest::_led(agora)` | `self.c`; patch de `painel_cell.timezone.now` com o argumento `agora`; devolve `['led']` | `app/colaboradores/tests/test_led_pelo_juiz.py:56`, `:65`, `:66`, `:67` |

Os três testes importam localmente o módulo `painel_cell` e chamam o atributo, sem apelido; o registro em `ColaboradoresConfig.ready` apenas importa o módulo e registra o renderizador, sem chamar o montador. `app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:111`; `app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:144`; `app/colaboradores/tests/test_led_pelo_juiz.py:65`; `app/colaboradores/apps.py:9`; `app/colaboradores/painel_cell.py:74`.

**Chamadores diretos de `montar_situacional` — produção:**

| Chamador | Argumentos / universo | Referência |
|---|---|---|
| `painel_situacional`, ramo com placar | `list(colabs_qs[:30]), corte, agora, f_plataforma` | `app/colaboradores/views.py:2659`, `:2664` |
| `painel_situacional`, ramo sem placar | `colabs_qs, corte, agora, f_plataforma`; monta tudo, entrega primeiras 30 | `app/colaboradores/views.py:2679`, `:2680` |
| `painel_situacional_linhas`, com filtro plataforma | lista de objetos dos IDs retornados por `ids_do_recorte(qs,f_plataforma)`, cortados por `offset:offset+100`; `corte, agora, ''` | `app/colaboradores/views.py:2758`, `:2797`, `:2801` |
| `painel_situacional_linhas`, sem filtro plataforma | `list(qs[offset:offset+100]), corte, agora, ''` | `app/colaboradores/views.py:2804` |
| `lavrar_placar_situacional.Command.handle` | lista de todo `colaboradores_em_operacao().select_related('empresa','posto','usuario')`, `corte=agora−7 dias`, `agora=timezone.now()`; plataforma omitida | `app/core/management/commands/lavrar_placar_situacional.py:44`, `:48`, `:49`, `:54`, `:56` |
| `auditar_invariantes_chamados.Command.handle`, S7 | lista de todo `colaboradores_em_operacao().select_related(...)`, `_corte7=_agora7−7 dias`, `_agora7=_tz_s7.now()`; plataforma omitida; só quando existe placar global com idade ≤300s | `app/chamados/management/commands/auditar_invariantes_chamados.py:1202`, `:1204`, `:1210`, `:1212`, `:1213`, `:1214`, `:1216` |

Os imports do montador nas views, no lavrador e no auditor são locais e sem `as`; `_tz_l` e `_tz_s7` são apelidos de `timezone`, não do montador. `app/colaboradores/views.py:2645`, `:2749`, `:2752`; `app/core/management/commands/lavrar_placar_situacional.py:44`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1200`, `:1212`.

**Chamadores diretos — testes:** [nao verificado]

| Chamador | Argumentos | Referência |
|---|---|---|
| `SituacionalPerguntaEmLoteTest::_monta(n)` | `_qs(n)` (primeiros `n` colaboradores da fixture, `select_related`, ordenados nome/pk), `corte=agora−7 dias`, `agora=timezone.now()`; import no topo sem apelido | `app/colaboradores/tests/test_situacional_pergunta_em_lote.py:29`, `:83`, `:87`, `:90` |
| `SituacionalPeloJuizTest::_situ` | `Colaborador.objects.filter(pk=self.c.pk)`, `agora−7 dias`, `agora`; import local sem apelido | `app/colaboradores/tests/test_situacional_pelo_juiz.py:19`, `:20`, `:22` |
| O200 `_cores` | queryset de `self.c.pk` com `select_related`, `agora−14h`, `agora`; mesmo agora usado no patch da célula | `app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:112`, `:113`, `:116`, `:119` |
| O200c `_pino` | queryset de `self.c.pk` com `select_related`, `agora−14h`, `agora`; mesmo agora usado no patch da célula | `app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:145`, `:146`, `:149`, `:151` |
| Adesão `_estados` | queryset dos dois colaboradores `retratado/vivo`, `corte=agora−14h`, `agora`; import no topo sem apelido | `app/colaboradores/tests/test_adesao_pelo_juiz.py:18`, `:48`, `:50` |
| Estado app `_estados` | queryset dos três `retratado/vivo/misto`, `corte=_dias(30)`, `agora=timezone.now()`; import no topo sem apelido | `app/colaboradores/tests/test_estado_app_pelo_juiz.py:23`, `:65`, `:66` |
| `test_placar_global_bate_com_a_soma_das_linhas` | lista do universo em operação com `select_related`, `corte=agora−PERIODO_PADRAO_DIAS`, `agora`; import local sem apelido | `app/core/tests/test_selo_placar_situacional.py:80`, `:83`, `:87`, `:89`, `:91` |

Nenhuma dessas chamadas passa usuário ao montador: a assinatura recebe apenas `colabs_qs,corte,agora,f_plataforma=''`; o escopo vem do queryset/lista preparado pelo chamador. `app/colaboradores/services/situacional.py:65`.

**Origem dos tempos, escopo e rotas:**

- Carga: `agora=timezone.now()`; `periodo='hoje'` significa últimas 24h, `'30d'` significa 30 dias e os demais valores dão 7 dias. Acesso por `login_required` e `tem_acao(user,'ver_painel_situacional')`; filtros empresa, praça persistida em `PreferenciaUI`, busca por nome/CPF/matrícula; universo parte de `colaboradores_em_operacao()`, com ordem nome/pk. `app/colaboradores/views.py:2552`, `:2562`, `:2566`, `:2569`, `:2584`, `:2592`, `:2594`, `:2618`, `:2622`, `:2658`.
- Lote recalcula seu próprio `agora=_tz_l.now()` e corte com a mesma regra de período a cada requisição; usa `login_required` e gate `is_superuser or is_staff`; praça vem do GET ou preferência, empresa do GET, busca por nome/CPF — aqui não inclui matrícula. `app/colaboradores/views.py:2739`, `:2754`, `:2764`, `:2771`, `:2779`, `:2784`, `:2786`.
- Carga `/colaboradores/painel-situacional/` e lote `/colaboradores/painel-situacional/linhas/` são montados por prefixo e rotas. `app/config/urls.py:26`; `app/colaboradores/urls.py:81`, `:82`.
- `_cell_colab_status` é registrado em `'colab-status'`, recebe `(request, ident)`; rota universal `/live-cell/<tipo>/<ident>/` resolve o registro e chama `fn(request,ident)`. URL concreta: `/live-cell/colab-status/<pk>/`. O alias de import relevante aqui é `live_cell as live_cell_view`. `app/colaboradores/painel_cell.py:74`, `:75`; `app/core/live_cell.py:18`, `:19`, `:22`; `app/core/urls.py:4`, `:16`; `app/config/urls.py:39`.
- A célula busca o colaborador só pelo PK, com `select_related`; não passa filtros do painel, corte, agora ou usuário ao montador. Se não encontra, retorna `None`, convertido em HTTP 204. `app/colaboradores/painel_cell.py:77`, `:79`, `:81`; `app/core/live_cell.py:23`.
- Cada `<tr>` declara `data-cell="colab-status:<pk>" data-cell-every="45"`; o JS faz fetch nessa URL, cria um `tbody` temporário e substitui o elemento antigo inteiro por `replaceChild(novo,el)`. Portanto substitui a linha inteira, incluindo atributos e todas as células; HTTP 204 remove a linha. `app/templates/colaboradores/partials/_ps_linha.html:1`; `app/static/js/live-cell.js:10`, `:38`, `:40`, `:46`, `:50`.
- Os 45s são cadência base com jitter de 0,75–1,25 e atualização condicionada a visibilidade e teto de seis requisições simultâneas; não é garantia de atualização exata a cada 45s. `app/static/js/live-cell.js:11`, `:17`, `:58`, `:61`, `:69`, `:76`.
- O painel chama `LiveCell.scan()` ao carregar; nos lotes, a sentinela usa `hx-swap="outerHTML"` para substituir a própria sentinela pelo lote de `<tr>` e nova sentinela. `app/templates/colaboradores/partials/painel_situacional.html:306`, `:327`; `app/templates/colaboradores/partials/_ps_lote.html:11`, `:12`.

Não executei views, testes, comando de lavra ou acesso a banco/logs; resultado de execução em produção permanece [nao verificado]. [nao verificado]

## 06/11 18:38 · sessao 35fb1de5f07d · DIA-SEM-TRABALHO-NA-FRENTE · sonda · origem=Code

O corpo de `sonda_dst.py` não aparece no texto recebido nem foi localizado na cópia. Portanto, a conformidade das chamadas e a implementação exata de `por_que`/`pelo_colab` ficam **[não verificado]**.

**1) Literais**

Transcrição da saída fornecida, sem reexecução ou confirmação independente. O marcador **[não verificado]** aplica-se a todo o bloco. Grupos com menos de três exemplos foram preservados como recebidos. **[não verificado]**

```text
U_D_BASE sem_porta classe d n=217 omitidos=0
U_D n=173 (CAUSA-D 11:2x: 165+5=170) fora={'BATIDA': 3, 'CADASTRO': 14, 'ESTRUTURA_mas_dia_de_trabalho': 27}
U_A n=170 (chat 17:4x PROD: 175) | INTERSECAO mesmo colab+dia=0 | so_D=173 so_A=170 | UNIAO=343
LINHAS 343 == UNIAO 343 True
QUADRO (por_que, batidas, porta, status pede decisao?, segura folha?) -- soma=343
GRUPO|n=156|folga_do_calendario ; bat0 ; sem_porta ; nao_pede(folga) ; segura|dono={'ESTRUTURA': 156}|em={'D': 156}
   ex col144 2026-09-21 clausula=trabalha_False fer=False/True/False aus=[] n_bat=0 orfas=0 porta=None status=folga resolver=False comp=10/2026
   ex col144 2026-09-23 clausula=trabalha_False fer=False/True/False aus=[] n_bat=0 orfas=0 porta=None status=folga resolver=False comp=10/2026
   ex col144 2026-09-25 clausula=trabalha_False fer=False/True/False aus=[] n_bat=0 orfas=0 porta=None status=folga resolver=False comp=10/2026
GRUPO|n=113|folga_do_calendario ; bat2+ ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'BATIDA': 15, 'CADASTRO': 71, 'ESTRUTURA': 27}|em={'A': 113}
   ex col60 2026-11-03 clausula=trabalha_False fer=False/True/False aus=[] n_bat=3 orfas=3 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col60 2026-11-05 clausula=trabalha_False fer=False/True/False aus=[] n_bat=3 orfas=3 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col97 2026-11-02 clausula=trabalha_False fer=False/None/None aus=[] n_bat=2 orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=28|feriado_sem_trabalha_em_feriado ; bat2+ ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'CADASTRO': 21, 'ESTRUTURA': 7}|em={'A': 28}
   ex col43 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=2 orfas=2 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col203 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=4 orfas=4 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col207 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=2 orfas=2 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=13|folga_do_calendario ; bat1 ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'BATIDA': 12, 'ESTRUTURA': 1}|em={'A': 13}
   ex col49 2026-11-04 clausula=trabalha_False fer=False/True/False aus=[] n_bat=1 orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col69 2026-10-25 clausula=trabalha_False fer=False/True/False aus=[] n_bat=1 orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col300 2026-11-05 clausula=trabalha_False fer=False/True/False aus=[] n_bat=1 orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=8|feriado_trab_feriado_True_mas_folga ; bat0 ; sem_porta ; nao_pede(folga) ; segura|dono={'ESTRUTURA': 8}|em={'D': 8}
   ex col144 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=0 orfas=0 porta=None status=folga resolver=False comp=11/2026
   ex col209 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=0 orfas=0 porta=None status=folga resolver=False comp=11/2026
   ex col268 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=0 orfas=0 porta=None status=folga resolver=False comp=11/2026
GRUPO|n=5|ausencia_lancada ; bat2+ ; sem_porta ; pede ; segura|dono={'ESTRUTURA': 5}|em={'D': 5}
   ex col137 2026-11-03 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(4851, 'reciclagem', 'por_outro', 'sem_chamado')] n_bat=4 orfas=0 porta=None status=irregular resolver=False comp=11/2026
   ex col137 2026-11-05 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(4851, 'reciclagem', 'por_outro', 'sem_chamado')] n_bat=4 orfas=0 porta=None status=irregular resolver=False comp=11/2026
   ex col292 2026-10-22 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(5110, 'atestado', 'por_outro', 'sem_chamado')] n_bat=2 orfas=0 porta=None status=irregular resolver=False comp=11/2026
GRUPO|n=5|feriado_trab_feriado_True_mas_folga ; bat2+ ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'ESTRUTURA': 1, 'BATIDA': 1, 'CADASTRO': 3}|em={'A': 5}
   ex col366 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=4 orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col788 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=8 orfas=8 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col868 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=4 orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=2|ata_tipo_dia_folga_sem_ausencia_cobrindo ; bat0 ; sem_porta ; nao_pede(escala_suspeita) ; segura|dono={'CADASTRO': 2}|em={'A': 2}
   ex col97 2026-11-03 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=0 orfas=1 porta=None status=escala_suspeita resolver=False comp=11/2026
   ex col97 2026-11-05 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=0 orfas=1 porta=None status=escala_suspeita resolver=False comp=11/2026
GRUPO|n=2|folga_do_calendario ; bat2+ ; sem_porta ; pede ; segura|dono={'ESTRUTURA': 2}|em={'D': 2}
   ex col348 2026-10-24 clausula=trabalha_False fer=False/True/False aus=[] n_bat=2 orfas=0 porta=None status=irregular resolver=False comp=11/2026
   ex col348 2026-10-31 clausula=trabalha_False fer=False/True/False aus=[] n_bat=2 orfas=0 porta=None status=irregular resolver=False comp=11/2026
GRUPO|n=2|feriado_sem_trabalha_em_feriado ; bat1 ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'BATIDA': 1, 'CADASTRO': 1}|em={'A': 2}
   ex col820 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=1 orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col900 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=1 orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; bat1 ; sem_porta ; pede ; segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col741 2026-11-01 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=1 orfas=1 porta=None status=aberto resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; bat1 ; porta ; nao_pede(escala_suspeita) ; segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col741 2026-11-03 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=1 orfas=1 porta=porta_pergunta status=escala_suspeita resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; bat2+ ; sem_porta ; nao_pede(escala_suspeita) ; segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col741 2026-11-05 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=2 orfas=2 porta=None status=escala_suspeita resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; bat1 ; sem_porta ; pede ; nao_segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col866 2026-10-21 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=1 orfas=1 porta=None status=aberto resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; bat1 ; porta ; pede ; segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col866 2026-11-02 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=1 orfas=1 porta=porta_pergunta status=aberto resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; bat2+ ; porta ; nao_pede(escala_suspeita) ; segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col866 2026-11-04 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=3 orfas=2 porta=porta_pergunta status=escala_suspeita resolver=False comp=11/2026
GRUPO|n=1|ausencia_lancada_pelo_colab ; bat2+ ; sem_porta ; pede ; segura|dono={'ESTRUTURA': 1}|em={'D': 1}
   ex col889 2026-10-25 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(4670, 'atestado', 'pelo_colab', 'sem_chamado')] n_bat=4 orfas=0 porta=None status=irregular resolver=False comp=11/2026
GRUPO|n=1|feriado_sem_trabalha_em_feriado ; bat0 ; sem_porta ; nao_pede(folga) ; segura|dono={'ESTRUTURA': 1}|em={'D': 1}
   ex col969 2026-10-12 clausula=trabalha_False fer=True/False/True aus=[] n_bat=0 orfas=None porta=None status=folga resolver=False comp=10/2026
GRUPO|n=1|folga_do_calendario ; bat2+ ; sem_porta ; nao_pede(ok) ; nao_segura|dono={'BATIDA': 1}|em={'A': 1}
   ex col975 2026-11-01 clausula=trabalha_False fer=False/True/False aus=[] n_bat=4 orfas=2 porta=None status=ok resolver=False comp=11/2026
MARGINAL|POR_QUE|{'feriado_sem_trabalha_em_feriado': 31, 'folga_do_calendario': 285, 'ata_tipo_dia_folga_sem_ausencia_cobrindo': 8, 'ausencia_lancada': 5, 'feriado_trab_feriado_True_mas_folga': 13, 'ausencia_lancada_pelo_colab': 1}
MARGINAL|BAT|{'2+': 157, '1': 19, '0': 167}
MARGINAL|PORTA|{None: 340, 'porta_pergunta': 3}
MARGINAL|STATUS|{'folga_trabalhada': 161, 'escala_suspeita': 5, 'irregular': 8, 'folga': 165, 'aberto': 3, 'ok': 1}
MARGINAL|PEDE_DECISAO|{False: 332, True: 11}
MARGINAL|RESOLVER_DIA|{False: 343}
MARGINAL|SEGURA|{False: 163, True: 180}
MARGINAL|DONO|{'CADASTRO': 104, 'BATIDA': 30, 'ESTRUTURA': 209}
MARGINAL|EM|{'A': 170, 'D': 173}
MARGINAL|CLAUSULA|{'trabalha_False': 329, 'ata_tipo_dia=folga': 8, 'ata_tipo_dia=ausencia': 6}
D_FOLGA_TRABALHADA n=161 com_bat=161 sem_porta=161 resolver_dia_True=0 (STATUS_QUE_PEDEM_DECISAO inclui? False)
C444 {'col': 'col444', 'dia': '2026-10-25', 'emp': 2, 'em': 'A', 'clausula': 'trabalha_False', 'tipo_dia_ata': 'folga', 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'aus': [], 'por_que': 'folga_do_calendario', 'n_bat': 4, 'n_orfas': 4, 'porta': None, 'status': 'folga_trabalhada', 'status_pede_decisao': False, 'resolver_dia': False, 'segura': False, 'comp': '11/2026', 'dono': 'ESTRUTURA'}
ERROS {}
CONCLUSAO_LIBERADA
```

**2) Funções, contratos e decisões próprias da sonda**

| Função e localização | Argumentos e contrato do código | A sonda respeita? |
|---|---|---|
| `dia_sem_trabalho(cel)` — `app/chamados/juizes.py:413` | Instância da célula, com `trabalha` e `ata`; retorna booleano. Ausência parcial recebe tratamento específico em `app/chamados/juizes.py:426`. | **[não verificado]** |
| `censo_alcance(empresas=None, hoje=None, limite=50, lista=None)` — `app/chamados/services/vigia_fio.py:542` | Empresas visíveis por padrão; recorte por interseção. `limite` não corta os dias `sem_porta`; linhas têm célula, colaborador, data, competência, rota, causa, detalhe e papel. `app/chamados/services/vigia_fio.py:545`, `app/chamados/services/vigia_fio.py:548`, `app/chamados/services/vigia_fio.py:551`. | **[não verificado]** |
| `InsumosDoDono.dono(self, data)` — `app/ponto/management/commands/e6_oraculo.py:172` | Data do dia consultado, sobre instância carregada para colaborador/janela. O construtor exige `colaborador, ini, fim, hoje` e `agora` por palavra-chave; demais alimentações são opcionais. `app/ponto/management/commands/e6_oraculo.py:114`, `app/ponto/management/commands/e6_oraculo.py:128`. | **[não verificado]** |
| `dia_das_batidas(colab, ini, fim, turnos=None)` — `app/ponto/turnos.py:2874` | Colaborador e janela apresentada; `turnos` é alimentação opcional. Retorna `{batida.pk: date}`, respeitando o dia do turno e a data local das não reclamadas. `app/ponto/turnos.py:2875`, `app/ponto/turnos.py:2895`, `app/ponto/turnos.py:2911`. | **[não verificado]** |
| `dias_da_ausencia(tipo, data, data_fim, ini, fim)` — `app/ponto/turnos.py:2774` | Cinco argumentos separados; retorna interseção fechada de datas. Sem fim, estende até `fim` somente para `TIPOS_EM_ABERTO`; nos demais tipos, dia único. `app/ponto/turnos.py:2775`, `app/ponto/turnos.py:2779`. | **[não verificado]** |
| `porta_alcancavel_do_dia(celula_id, *, alimentacao=None)` — `app/chamados/juizes.py:1341` | Recebe **ID da célula**; alimentação opcional é a de `alimentar_portas(lote)`. Retorna `None` ou dados da porta. `app/chamados/juizes.py:1342`, `app/chamados/juizes.py:1370`. | **[não verificado]** |
| `status_dos_dias_lavrados(pares, *, agora=None, carga=None, fatos=None)` — `app/ponto/status_dia.py:221` | Pares `(colaborador, data)`; retorna mapa por `(colaborador_id, data)`. A carga deve conter datas e colaboradores; `fatos` exige `carga`. `app/ponto/status_dia.py:222`, `app/ponto/status_dia.py:238`, `app/ponto/status_dia.py:259`. | **[não verificado]** |
| `resolver_dia_oferecido_nas_datas(colaborador, datas)` — `app/colaboradores/services/calendario.py:117` | Várias datas do mesmo colaborador; lê o campo do calendário e retorna `{date: bool}`; data não devolvida responde `False`. `app/colaboradores/services/calendario.py:118`, `app/colaboradores/services/calendario.py:122`, `app/colaboradores/services/calendario.py:135`. | **[não verificado]** |
| `o_que_segura(colab, mes, ano)` — `app/folha/fora_da_folha.py:203` | Colaborador e competência; retorna dias retentores, veredito e orientação. As datas da saída são strings. `app/folha/fora_da_folha.py:204`, `app/folha/fora_da_folha.py:227`. | **[não verificado]** |
| `competencia_do_fato(colaborador, quando)` — `app/ponto/services/fechamento.py:1586` | Fato de um dia; retorna `(mes, ano)`. Aceita `date` ou `datetime`; datetime consciente é localizado antes de extrair a data. `app/ponto/services/fechamento.py:1587`, `app/ponto/services/fechamento.py:1542`, `app/ponto/services/fechamento.py:1578`. | **[não verificado]** |

Sobre `por_que`:

- A regra existente para **dia sem trabalho** usa `trabalha is False` ou `ata.tipo_dia` preenchido e diferente de `trabalho`. Na ausência parcial, somente `trabalha is False` mantém essa conclusão. Uma reprodução que omita essa exceção diverge da implementação. `app/chamados/juizes.py:422`, `app/chamados/juizes.py:426`, `app/chamados/juizes.py:428`.
- A causa **feriado que vira folga** já tem juiz: `feriado_vira_folga`. Exige flag de trabalho em feriado falsa, ciclo elegível e feriado no lugar do vínculo. Usar somente “é feriado + flag falsa” seria uma reprodução incompleta. `app/escala/models.py:1373`, `app/escala/models.py:1382`, `app/escala/models.py:1389`.
- `trabalha_em_feriado=True` coexistir com folga não contradiz a regra: a transformação por feriado somente altera `base is True`, preservando a folga que já existia. `app/escala/models.py:1367`.
- Os nomes apresentados podem servir como rótulos de agrupamento; determinar se o script apenas rotula, repete predicados ou altera a classificação exige seu corpo. **[não verificado]**

Sobre `pelo_colab`: já existe identificação de autoria pelo vínculo entre `ausencia.registrado_por.colaborador` e `ausencia.colaborador_id`. Ela integra a guarda de aprovação sem documento para tipos que exigem anexo; não certifica origem via disputa nem decisão administrativa. `app/ponto/services/ausencia.py:627`, `app/ponto/services/ausencia.py:628`, `app/ponto/services/ausencia.py:630`. Se o teste da sonda reproduz essa identificação, ou usa outro critério, fica **[não verificado]**.

**3) Guarda do emissor, outros escritores e consumidores do status**

Em `motivos_a_perguntar`, a ordem é:

1. `dia_sem_trabalho(cel)`; somente se falso, pelo curto-circuito do `or`, testa colaborador diferente de `ativo`. Qualquer resultado verdadeiro devolve `[]`. `app/chamados/services/fabrica_da_celula.py:91`.
2. Dia futuro: `cel.data > hoje`. `app/chamados/services/fabrica_da_celula.py:93`.
3. Define `agora`, se necessário, e calcula `vencidos_no_instante`, antes da próxima guarda. `app/chamados/services/fabrica_da_celula.py:95`, `app/chamados/services/fabrica_da_celula.py:98`.
4. Veredito preenchido fora de `CelulaDia.VEREDITOS_ACUSAM`. `app/chamados/services/fabrica_da_celula.py:99`.
5. `parcial_so_lista`, usando ata e julgamento anterior recebido ou da célula. `app/chamados/services/fabrica_da_celula.py:102`.
6. Carrega perguntas do mesmo colaborador/data, excluindo `VIA_DUPLICATA`, e elimina motivos já existentes da saída de `motivos_pela_ata`. `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:126`.

Dentro de `motivos_pela_ata`, vencidos vazios calam; `_ata_do_dia` que não permite também cala; sem ata e com conjunto de vencidos, cala; sem ata e `vencidos=None`, consulta `classificar_falta`; com ata, seleciona os marcos vencidos. `app/chamados/services/fabrica_da_celula.py:45`, `app/chamados/services/fabrica_da_celula.py:49`, `app/chamados/services/fabrica_da_celula.py:52`, `app/chamados/services/fabrica_da_celula.py:59`.

Assim, uma célula com `trabalha is False` já é recusada na **primeira guarda**, mesmo havendo batidas; esse predicado não consulta `status='folga_trabalhada'`. `app/chamados/juizes.py:428`, `app/chamados/services/fabrica_da_celula.py:91`. Que as células medidas tinham exatamente o estado declarado na saída permanece **[não verificado]**.

**Sim, existem outros caminhos de nascimento sem essa guarda global:**

- **Chamado da célula:** `detectar_par_relampago.Command.handle` pode chamar `ChamadoColaborador.abrir`, após suas próprias condições, com módulo `par_relampago` e `data_turno`. A porta resolve a célula antes da gravação. `app/ponto/management/commands/detectar_par_relampago.py:352`, `app/ponto/management/commands/detectar_par_relampago.py:368`, `app/ponto/management/commands/detectar_par_relampago.py:383`, `app/chamados/models.py:913`, `app/chamados/models.py:932`.
- As guardas centrais de absolvição e veto alcançam conjuntos específicos de módulos que não incluem `par_relampago`; portanto não constituem veto geral a nascimento em folga. `app/chamados/models.py:1073`, `app/chamados/models.py:1097`, `app/ponto/services/furos_vetados.py:74`, `app/chamados/catalogo/furo_modulos.py:15`, `app/chamados/reconciliador.py:336`.
- **Pergunta:** a API pode acrescentar `foto_ausente`, registrar a última batida e chamar `criar_ou_atualizar_disputa`; este chama `gerar_perguntas_disputa`, que pode derivar a data da última batida e chamar `PerguntaDisputa.abrir`. Esse gate resolve a célula e cria a pergunta sem consultar `dia_sem_trabalho`. `app/api/views_core.py:469`, `app/api/views_core.py:474`, `app/api/views_core.py:772`, `app/api/views_core.py:778`, `app/chamados/services/disputa_emissao.py:606`, `app/chamados/services/disputa_emissao.py:350`, `app/chamados/services/disputa_emissao.py:389`, `app/chamados/models.py:2430`, `app/chamados/models.py:2476`.

**Nascer não basta para ser porta alcançável:** chamado da célula precisa estar na fila; pergunta precisa estar viva, no app, sob âncora na fila e passar pelo teste de resposta não vazia. `app/chamados/juizes.py:1344`, `app/chamados/juizes.py:1375`, `app/chamados/juizes.py:1380`. A existência efetiva dessas portas nos 161 casos não é demonstrada; a saída fornecida declara todos sem porta. **[não verificado]**

Além de `resolver_dia_oferecido`, o consumidor direto encontrado de `STATUS_QUE_PEDEM_DECISAO` é o teste que importa, confere a tupla e itera seus valores. `app/chamados/tests/test_o261_vigia_fio.py:901`, `app/chamados/tests/test_o261_vigia_fio.py:902`, `app/chamados/tests/test_o261_vigia_fio.py:903`. O uso de produção está em `resolver_dia_oferecido`; a tupla não contém `folga_trabalhada`. `app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:104`.

**4) Ausência do próprio colaborador via disputa**

O leitor é **`decididos_do_periodo`**, que monta `{data: cartao_do_dia}`. Sua consulta não exige autor admin, aprovação ou chamado de origem; expande as datas do lançamento e deixa o mais recente sobrescrever. Portanto, a autoria pelo próprio colaborador não impede que o dia

[truncado em 20000 de 21951 caracteres]

## 06/11 18:35 · sessao c724227316a6 · - · - · origem=Ronald (tmux)

`motivos_a_perguntar` recusa nesta ordem:

1. `dia_sem_trabalho(cel)`; pelo `or` com curto circuito, só depois testa colaborador diferente de `ativo`. Qualquer um devolve `[]`. (`app/chamados/services/fabrica_da_celula.py:89-92`)
2. Dia futuro (`cel.data > hoje`), devolvendo `[]`. (`app/chamados/services/fabrica_da_celula.py:93-94`)
3. Define `agora` quando ausente e calcula `vencidos_no_instante`; esse cálculo antecede a próxima guarda. (`app/chamados/services/fabrica_da_celula.py:95-98`)
4. Veredito preenchido fora de `CelulaDia.VEREDITOS_ACUSAM`. (`app/chamados/services/fabrica_da_celula.py:99-100`)
5. `parcial_so_lista`, usando a ata e o julgamento anterior recebido ou da própria célula. (`app/chamados/services/fabrica_da_celula.py:101-104`)
6. Retira motivos já presentes em perguntas do mesmo colaborador/data, excluindo somente `VIA_DUPLICATA`; retorna motivos de `motivos_pela_ata` ainda ausentes. (`app/chamados/services/fabrica_da_celula.py:118-126`)

Dentro de `motivos_pela_ata`, conjunto vazio de vencidos cala; `_ata_do_dia` que não permite cala; sem ata e com conjunto de vencidos também cala; sem ata e `vencidos=None` consulta `classificar_falta`; com ata, só seleciona marcos vencidos. (`app/chamados/services/fabrica_da_celula.py:45-64`)

`dia_sem_trabalho` aceita ausência parcial como exceção: nela, só `cel.trabalha is False` cala; nos demais casos, cala por `trabalha is False` ou `ata.tipo_dia` preenchido e diferente de `trabalho`. (`app/chamados/juizes.py:422-428`)

Há outros escritores possíveis sem essa guarda global; portanto o silêncio desse emissor não demonstra impossibilidade de outra porta. Um exemplo concreto de chamado da célula é `detectar_par_relampago.Command.handle`: após suas guardas próprias (`par_completo`, dry-run/retratador), chama `ChamadoColaborador.abrir` com módulo `par_relampago` e `contexto_json.data_turno`; não consulta `dia_sem_trabalho` nesse caminho. (`app/ponto/management/commands/detectar_par_relampago.py:327`, `app/ponto/management/commands/detectar_par_relampago.py:352-385`)

O catálogo declara `par_relampago` como fato do dia, chave `data_turno`, dono supervisão; `abrir` resolve a célula correspondente antes de gravar. (`app/chamados/catalogo/modulos.py:113-114`; `app/chamados/models.py:913-932`) As guardas centrais não proíbem todo nascimento em folga: `_celula_que_absolve` só alcança `MODULOS_DE_TURNO`; `_veto_da_regua` só alcança a união `FURO_MODULES + MODULOS_DE_TURNO`, listas que não incluem `par_relampago`. (`app/chamados/models.py:1073-1076`; `app/chamados/models.py:1097-1099`; `app/ponto/services/furos_vetados.py:74-81`; `app/chamados/catalogo/furo_modulos.py:15-16`; `app/chamados/reconciliador.py:336-337`) Assim, há possibilidade estática de um chamado desse módulo nascer para uma folga trabalhada que satisfaça o detector; ocorrência nos 161 dias da amostra: **[não verificado]**.

Também há nascimento de pergunta por outro caminho: a API acrescenta `foto_ausente` conforme `selfie_ausente`, registra `ultima_batida` e chama `criar_ou_atualizar_disputa`; esse helper chama `gerar_perguntas_disputa`, que deriva a data local da última batida e chama `PerguntaDisputa.abrir`. (`app/api/views_core.py:459-475`; `app/api/views_core.py:769-780`; `app/chamados/services/disputa_emissao.py:605-618`; `app/chamados/services/disputa_emissao.py:350-364`; `app/chamados/services/disputa_emissao.py:389-396`) O gate da pergunta resolve a célula pelo dia e efetua o `create`, sem guarda `dia_sem_trabalho`. (`app/chamados/models.py:2398-2477`) `foto_ausente` tem pergunta no catálogo. (`app/chamados/catalogo/perguntas.py:94-97`)

Nascimento de pergunta não garante porta alcançável: ela ainda precisa estar viva pelo C6, no app, sob âncora na fila, sem resposta vazia quando respondida; chamado da célula precisa estar na fila. (`app/chamados/juizes.py:1341-1369`; `app/chamados/juizes.py:1375-1386`) O C6 pode encerrar pela célula, mas resposta textual ainda não validada tem precedência quando não houve decisão explícita do admin. (`app/chamados/juizes.py:1135-1162`; `app/chamados/juizes.py:1180-1196`) Logo, possibilidade de outro escritor: **sim, condicional**; porta efetivamente nascida para algum dos 161 casos: **[não verificado]**.

A busca global do identificador encontrou somente um leitor de produção de `STATUS_QUE_PEDEM_DECISAO`: `resolver_dia_oferecido`, em `app/colaboradores/services/calendario.py:104`; o outro consumidor é o teste, que importa, confere a tupla e itera seus status. (`app/chamados/tests/test_o261_vigia_fio.py:901-903`) A tupla contém `falta`, `irregular`, `aberto`, `sem_certidao`, sem `folga_trabalhada`. (`app/colaboradores/services/calendario.py:94`)

Nenhum `AGENTS.md` foi encontrado nos diretórios ancestrais consultados nem sob `app`; existência fora desses lugares: **[não verificado]**.

## 06/11 18:35 · sessao 03448364f99c · - · - · origem=Ronald (tmux)

Contratos encontrados:

| Função | Contrato documentado |
|---|---|
| `dia_das_batidas(colab, ini, fim, turnos=None)` | `app/ponto/turnos.py:2874`. Recebe colaborador e janela apresentada; `turnos` é alimentação opcional. Retorna `{batida.pk: date}`: batida reclamada pertence ao dia do turno; outra pertence à sua data local. `app/ponto/turnos.py:2875`, `app/ponto/turnos.py:2895`, `app/ponto/turnos.py:2911`. |
| `dias_da_ausencia(tipo, data, data_fim, ini, fim)` | `app/ponto/turnos.py:2774`. Retorna lista de datas na interseção com `[ini,fim]`; ausência sem fim estende até `fim` somente para `TIPOS_EM_ABERTO`, senão ocupa o dia único. `app/ponto/turnos.py:2775`, `app/ponto/turnos.py:2779`. |
| `status_dos_dias_lavrados(pares, *, agora=None, carga=None, fatos=None)` | `app/ponto/status_dia.py:221`. `pares` contém `(colaborador,data)`; retorna `{(colaborador_id,data):dict}`. `carga` deve conter datas e colaboradores; `fatos` só pode ser passado com `carga`. `app/ponto/status_dia.py:222`, `app/ponto/status_dia.py:238`, `app/ponto/status_dia.py:259`. |
| `o_que_segura(colab, mes, ano)` | `app/folha/fora_da_folha.py:203`. Retorna os dias que retêm o colaborador naquela competência, com veredito, motivo, fio, destrava e chamado. As datas retornadas são strings. `app/folha/fora_da_folha.py:204`, `app/folha/fora_da_folha.py:227`. |
| `competencia_do_fato(colaborador, quando)` | `app/ponto/services/fechamento.py:1586`. Retorna `(mes,ano)` do fato de um dia; aceita `date` ou `datetime`, normalizando datetime consciente para a data local. `app/ponto/services/fechamento.py:1587`, `app/ponto/services/fechamento.py:1542`, `app/ponto/services/fechamento.py:1578`. |

A conformidade das chamadas da sonda com esses contratos é **[não verificado]**: o texto fornecido contém a saída, mas não o corpo de `sonda_dst.py`; tampouco localizei esse arquivo na cópia **[não verificado]**.

Para avaliar `por_que`:

- `dia_sem_trabalho` já decide por duas cláusulas: `trabalha is False` ou `ata.tipo_dia` presente e diferente de `trabalho`; ausência parcial conserva cobrança, salvo `trabalha is False`. `app/chamados/juizes.py:413`, `app/chamados/juizes.py:426`.
- Identificar feriado apenas por `trabalha_em_feriado=False` repetiria incompletamente a regra existente: `feriado_vira_folga` exige também ciclo elegível e feriado no lugar do vínculo. `app/escala/models.py:1373`, `app/escala/models.py:1382`, `app/escala/models.py:1389`.
- `feriado_trab_feriado_True_mas_folga` não demonstra contradição: a transformação por feriado só altera `base is True`, preservando folga já existente; a precedência também admite feriado vencer folga. `app/escala/models.py:1367`, `app/ponto/precedencia.py:17`, `app/ponto/precedencia.py:376`.
- Ausência, feriado, folga e trabalho já têm classificação e precedência próprias na produção; o vencedor é obtido ordenando os fatos por `PRECEDENCIA`. `app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:379`, `app/ponto/precedencia.py:396`.
- Sem o script, dizer se `por_que` apenas agrupa ou reimplementa/contradiz essa regra permanece **[não verificado]**.

## 06/11 18:34 · sessao e15b5cdc899a · - · - · origem=Ronald (tmux)

O leitor central é **`decididos_do_periodo(colaborador, data_ini, data_fim, *, ausencias=None)`**, que declara responder “houve decisão humana neste dia?”, inclusive para rejeitadas; sua consulta filtra colaborador e interseção com a janela, **sem exigir autor admin, aprovação ou chamado de origem**. Portanto, uma ausência registrada pelo próprio colaborador alcança esse leitor se sua data/período intersectar a janela. Ele expande o intervalo e monta `cartao_do_dia`; o lançamento mais recente vence. (`app/ponto/services/dia_decidido.py:529`, `:532`, `:547`, `:550`, `:557`, `:561`.)

O cartão inclui `veredito`, `cor`, `rotulo`, minutos, tipo, status, ID, quem e quando; `quem_quando` prefere quem aprovou/recusou e usa quem registrou como alternativa. Logo, não exclui o próprio colaborador na autoria. (`app/ponto/services/dia_decidido.py:500`, `:506`, `:512`, `:516`.)

A ligação com a tela do admin existe:

- Calendário chama `decididos_do_periodo` e `cobertura_ausencia_periodo(..., para='folha')`; passa ambos para `do_dia` e publica `palavra_dia`, `cor_dia`, `veredito_dia`. O template mostra o rótulo no chip de ausência ou “O que foi decidido neste dia: …”. (`app/colaboradores/services/calendario.py:531`, `:533`, `:534`, `:715`, `:724`; `app/templates/colaboradores/partials/_calendario_grade.html:97`, `:98`.)
- Espelho chama o mesmo leitor para a janela, transporta `_decisao_do_dia` e entrega-o a `do_dia`. (`app/ponto/services/espelho.py:647`, `:649`, `:871`, `:328`, `:340`.)
- Histórico do drawer lê `linha_do_tempo`, que consulta todas as ausências do colaborador, exclui filhas de intervalo e distingue pendentes; o template imprime rótulo, quem e quando. (`app/ponto/views.py:2014`, `:2015`; `app/ponto/services/dia_decidido.py:566`, `:587`, `:593`, `:610`; `app/templates/ponto/partials/painel_ausencia.html:184`, `:188`.)

**Limite:** alcançar `decididos_do_periodo` não demonstra que houve aprovação do admin. Essa função não filtra status, e `cartao_do_dia` calcula o veredito pelo tipo, salvo rejeitada; `do_dia` trata pedido pendente separadamente quando recebe `pendente`. (`app/ponto/services/dia_decidido.py:550`, `:453`, `:461`, `:468`, `:420`.) Para aprovada, a cobertura de folha entra primeiro; pendência deve dizer “aguardando decisão”. (`app/ponto/turnos.py:2514`; `app/ponto/services/dia_decidido.py:363`, `:224`, `:420`.)

No dado fornecido, `col889/2026-10-25` tem ausência `4670`, tipo `atestado`, `pelo_colab`, `sem_chamado`; **status da ausência, datas inicial/final, origem efetiva via disputa, aprovação, rótulo renderizado e presença efetiva na tela não constam**. [não verificado] O código admite alcançar esse dia, porém a sonda não mediu esse leitor. [não verificado]

Sobre `pelo_colab`, já existe uma regra equivalente de identificação de autoria: `getattr(ausencia.registrado_por, 'colaborador', None)` e comparação de seu `pk` com `ausencia.colaborador_id`; ali ela serve especificamente à guarda de aprovação de tipo que exige documento sem anexo. Não prova origem via disputa nem decisão administrativa. (`app/ponto/services/ausencia.py:627`, `:628`, `:630`, `:632`.)

A materialização via disputa usa `registrado_por=user`, `chamado_origem=p.chamado_destino` e descrição da disputa; a documentação distingue falta aprovada e atestado cujo estado inicial é decidido pelo juiz. Assim, ausência nascida via disputa não implica decisão do admin, e um destino vazio pode produzir `chamado_origem=None`. (`app/chamados/services/materializacao.py:495`, `:498`, `:499`, `:520`, `:525`, `:526`, `:527`.)

## 06/11 18:26 · sessao eeb4262ba705 · DIA-SEM-TRABALHO-NA-FRENTE · sonda · origem=Code

**1) Transcrição literal**

Fonte deste bloco: o dado raspado da mensagem, sem arquivo numerado disponível para conferência [nao verificado]. Preservei os exemplos existentes; os grupos menores não trazem três exemplos. Não há linha `CONCLUSAO` no texto recebido: há `INCOMPLETO: 100 erros` [nao verificado].

```text
U_D_BASE sem_porta classe d n=217 omitidos=0
U_D n=173 (CAUSA-D 11:2x: 165+5=170) fora={'BATIDA': 3, 'CADASTRO': 14, 'ESTRUTURA_mas_dia_de_trabalho': 27}
U_A n=170 (chat 17:4x PROD: 175) | INTERSECAO mesmo colab+dia=0 | so_D=173 so_A=170 | UNIAO=343
LINHAS 343 == UNIAO 343 True
QUADRO (por_que, batidas, porta, status pede decisao?, segura folha?) -- soma=343
GRUPO|n=156|folga_do_calendario ; batERRO ; sem_porta ; nao_pede(folga) ; segura|dono={'ESTRUTURA': 156}|em={'D': 156}
   ex col144 2026-09-21 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=10/2026
   ex col144 2026-09-23 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=10/2026
   ex col144 2026-09-25 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=10/2026
GRUPO|n=126|folga_do_calendario ; batERRO ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'BATIDA': 27, 'CADASTRO': 71, 'ESTRUTURA': 28}|em={'A': 126}
   ex col49 2026-11-04 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col60 2026-11-03 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=3 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col60 2026-11-05 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=3 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=30|feriado_sem_trabalha_em_feriado ; batERRO ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'CADASTRO': 22, 'ESTRUTURA': 7, 'BATIDA': 1}|em={'A': 30}
   ex col43 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=ERRO orfas=2 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col203 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=ERRO orfas=4 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col207 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=ERRO orfas=2 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=8|feriado_trab_feriado_True_mas_folga ; batERRO ; sem_porta ; nao_pede(folga) ; segura|dono={'ESTRUTURA': 8}|em={'D': 8}
   ex col144 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=11/2026
   ex col209 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=11/2026
   ex col268 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=11/2026
GRUPO|n=5|ausencia_lancada ; batERRO ; sem_porta ; pede ; segura|dono={'ESTRUTURA': 5}|em={'D': 5}
   ex col137 2026-11-03 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(4851, 'reciclagem', 'por_outro', 'sem_chamado')] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
   ex col137 2026-11-05 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(4851, 'reciclagem', 'por_outro', 'sem_chamado')] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
   ex col292 2026-10-22 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(5110, 'atestado', 'por_outro', 'sem_chamado')] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
GRUPO|n=5|feriado_trab_feriado_True_mas_folga ; batERRO ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'ESTRUTURA': 1, 'BATIDA': 1, 'CADASTRO': 3}|em={'A': 5}
   ex col366 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col788 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=8 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col868 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=3|ata_tipo_dia_folga_sem_ausencia_cobrindo ; batERRO ; sem_porta ; nao_pede(escala_suspeita) ; segura|dono={'CADASTRO': 3}|em={'A': 3}
   ex col97 2026-11-03 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=None status=escala_suspeita resolver=False comp=11/2026
   ex col97 2026-11-05 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=None status=escala_suspeita resolver=False comp=11/2026
   ex col741 2026-11-05 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=2 porta=None status=escala_suspeita resolver=False comp=11/2026
GRUPO|n=2|folga_do_calendario ; batERRO ; sem_porta ; pede ; segura|dono={'ESTRUTURA': 2}|em={'D': 2}
   ex col348 2026-10-24 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
   ex col348 2026-10-31 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
GRUPO|n=2|ata_tipo_dia_folga_sem_ausencia_cobrindo ; batERRO ; porta ; nao_pede(escala_suspeita) ; segura|dono={'CADASTRO': 2}|em={'A': 2}
   ex col741 2026-11-03 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=porta_pergunta status=escala_suspeita resolver=False comp=11/2026
   ex col866 2026-11-04 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=2 porta=porta_pergunta status=escala_suspeita resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; batERRO ; sem_porta ; pede ; segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col741 2026-11-01 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=None status=aberto resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; batERRO ; sem_porta ; pede ; nao_segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col866 2026-10-21 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=None status=aberto resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; batERRO ; porta ; pede ; segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col866 2026-11-02 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=porta_pergunta status=aberto resolver=False comp=11/2026
GRUPO|n=1|ausencia_lancada_pelo_colab ; batERRO ; sem_porta ; pede ; segura|dono={'ESTRUTURA': 1}|em={'D': 1}
   ex col889 2026-10-25 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(4670, 'atestado', 'pelo_colab', 'sem_chamado')] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
GRUPO|n=1|feriado_sem_trabalha_em_feriado ; batERRO ; sem_porta ; nao_pede(folga) ; segura|dono={'ESTRUTURA': 1}|em={'D': 1}
   ex col969 2026-10-12 clausula=trabalha_False fer=True/False/True aus=[] n_bat=ERRO orfas=None porta=None status=folga resolver=False comp=10/2026
GRUPO|n=1|folga_do_calendario ; batERRO ; sem_porta ; nao_pede(ok) ; nao_segura|dono={'BATIDA': 1}|em={'A': 1}
   ex col975 2026-11-01 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=2 porta=None status=ok resolver=False comp=11/2026
MARGINAL|POR_QUE|{'feriado_sem_trabalha_em_feriado': 31, 'folga_do_calendario': 285, 'ata_tipo_dia_folga_sem_ausencia_cobrindo': 8, 'ausencia_lancada': 5, 'feriado_trab_feriado_True_mas_folga': 13, 'ausencia_lancada_pelo_colab': 1}
MARGINAL|BAT|{'ERRO': 343}
MARGINAL|PORTA|{None: 340, 'porta_pergunta': 3}
MARGINAL|STATUS|{'folga_trabalhada': 161, 'escala_suspeita': 5, 'irregular': 8, 'folga': 165, 'aberto': 3, 'ok': 1}
MARGINAL|PEDE_DECISAO|{False: 332, True: 11}
MARGINAL|RESOLVER_DIA|{False: 343}
MARGINAL|SEGURA|{False: 163, True: 180}
MARGINAL|DONO|{'CADASTRO': 104, 'BATIDA': 30, 'ESTRUTURA': 209}
MARGINAL|EM|{'A': 170, 'D': 173}
MARGINAL|CLAUSULA|{'trabalha_False': 329, 'ata_tipo_dia=folga': 8, 'ata_tipo_dia=ausencia': 6}
D_FOLGA_TRABALHADA n=161 com_bat=0 sem_porta=161 resolver_dia_True=0 (STATUS_QUE_PEDEM_DECISAO inclui? False)
C444 {'col': 'col444', 'dia': '2026-10-25', 'emp': 2, 'em': 'A', 'clausula': 'trabalha_False', 'tipo_dia_ata': 'folga', 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'aus': [], 'por_que': 'folga_do_calendario', 'n_bat': 'ERRO', 'n_orfas': 4, 'porta': None, 'status': 'folga_trabalhada', 'status_pede_decisao': False, 'resolver_dia': False, 'segura': False, 'comp': '11/2026', 'dono': 'ESTRUTURA'}
ERRO|dia_das_batidas|col43|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col49|2026-11-04|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col60|2026-11-03|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col69|2026-10-25|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col97|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col123|2026-11-05|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col130|2026-10-30|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col137|2026-11-03|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col144|2026-09-21|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col203|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col207|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col209|2026-09-21|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col211|2026-10-24|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col227|2026-11-03|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col242|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col268|2026-09-21|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col270|2026-10-22|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col278|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col282|2026-11-01|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col292|2026-10-22|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col300|2026-10-24|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col301|2026-10-22|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col312|2026-09-21|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col328|2026-10-22|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col348|2026-10-24|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col354|2026-10-30|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col366|2026-10-27|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col369|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col373|2026-10-21|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col384|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERROS {'dia_das_batidas:TypeError': 100}
INCOMPLETO: 100 erros
rc_sombra=0
```

**2) Funções, contratos e decisões da sonda**

O corpo de `sonda_dst.py` não está disponível no material consultável; sua identificação/hash na saída não permite conferir suas chamadas. Portanto, os argumentos efetivamente passados a **cada uma** das funções abaixo são [nao verificado].

| Função e localização | Contrato documentado/implementado | Sonda cumpre? |
|---|---|---|
| `dia_sem_trabalho(cel)` — `app/chamados/juizes.py:413-428` | Recebe célula; lê `trabalha` e `ata`. Ausência parcial tem tratamento próprio. | [nao verificado] |
| `censo_alcance(empresas=None, hoje=None, limite=50, lista=None)` — `app/chamados/services/vigia_fio.py:542-554` | Censo de dias retidos sem porta; `empresas=None` usa empresas visíveis. `limite` não corta os dias sem porta. | [nao verificado] |
| `InsumosDoDono.dono(self, data)` — `app/ponto/management/commands/e6_oraculo.py:172-183` | Recebe a data; utiliza os insumos da instância. O construtor recebe colaborador, `ini`, `fim`, `hoje` e exige `agora=` — `:113-145`. | [nao verificado] |
| `dia_das_batidas(colab, ini, fim, turnos=None)` — `app/ponto/turnos.py:2874-2897` | Janela apresentada; `turnos=` é alimentação. Retorna **`{batida.pk: date}`**, não contagem. | [nao verificado] |
| `dias_da_ausencia(tipo, data, data_fim, ini, fim)` — `app/ponto/turnos.py:2774-2790` | Retorna datas no intervalo fechado; ausência sem fim estende até `fim` somente para `TIPOS_EM_ABERTO`. | [nao verificado] |
| `porta_alcancavel_do_dia(celula_id, *, alimentacao=None)` — `app/chamados/juizes.py:1341-1370` | Recebe **ID da célula**; alimentação opcional vem de `alimentar_portas(lote)`. Retorna `None` ou dicionário de rota/fila/verbos/fio. | [nao verificado] |
| `status_dos_dias_lavrados(pares, *, agora=None, carga=None, fatos=None)` — `app/ponto/status_dia.py:221-247` | Pares `(colaborador, data)`; carga opcional deve conter pessoas e datas; `fatos` somente com carga. Validações em `:259-267`. | [nao verificado] |
| `resolver_dia_oferecido_nas_datas(colaborador, datas)` — `app/colaboradores/services/calendario.py:117-138` | Datas de um mesmo colaborador; retorna `{date: bool}` lendo o calendário. Dia ausente do calendário responde `False`. | [nao verificado] |
| `o_que_segura(colab, mes, ano)` — `app/folha/fora_da_folha.py:203-231` | Recebe colaborador e competência; retorna dicionário com os dias retidos e seus motivos/fios. | [nao verificado] |
| `competencia_do_fato(colaborador, quando)` — `app/ponto/services/fechamento.py:1586-1588` | Fato de um dia; retorna `(mes, ano)`. Normaliza `date`/`datetime` pelo caminho `:1542-1583`. | [nao verificado] |

O erro impresso comprova apenas a mensagem de comparação entre `date` e `datetime`; sem a chamada e o traceback, atribuí-lo definitivamente a argumentos errados da sonda seria [nao verificado]. O código chama o pareador quando `turnos` não foi alimentado e contém comparação de datas de segmento com `data_turno`. (`app/ponto/turnos.py:2900-2905,1990-1993`)

**`com_bat=0` não comprova “zero batidas”**: a própria saída registra `MARGINAL|BAT|{'ERRO': 343}`; a contagem correta das batidas permanece [nao verificado].

Quanto às decisões próprias:

- **`por_que`:** a implementação e sua ordem de precedência são [nao verificado]. O juiz existente não devolve esses motivos: devolve um booleano por `trabalha is False` ou tipo de ata preenchido diferente de `trabalho`, com exceção para cobertura parcial. Tratar os nomes impressos como causas oficiais extrapolaria esse contrato. (`app/chamados/juizes.py:413-428`)
- **`folga_do_calendario`:** `trabalha=False`, isoladamente, não documenta a causa da folga; o próprio código explica que a célula guarda esse booleano sem guardar o porquê. O rótulo pode servir ao agrupamento, mas sua derivação pela sonda é [nao verificado]. (`app/escala/models.py:1373-1384`)
- **Os dois rótulos de feriado:** `trabalha_em_feriado=True` pode coexistir com folga; essa flag impede a transformação por feriado, mas não transforma uma folga já existente em trabalho. Portanto, o nome `feriado_trab_feriado_True_mas_folga` não demonstra contradição. A regra completa também considera ciclo e lugar do vínculo; um teste apenas da flag seria insuficiente. (`app/escala/models.py:1367-1371,1382-1395`)
- **`ausencia_lancada` e `ata_tipo_dia_folga_sem_ausencia_cobrindo`:** a ata não laboral já participa do juiz; a lista de ausências não é requisito adicional desse predicado. Se a sonda usa essa lista somente para subdividir os resultados, é agrupamento; se redecide o booleano com ela, a equivalência é [nao verificado]. (`app/chamados/juizes.py:422-428`)
- **`pelo_colab`:** já existe teste de identidade entre o colaborador de `registrado_por` e o colaborador da ausência. Se a sonda compara `registrado_por_id` com `colab.usuario_id`, repete essa identidade; a comparação efetivamente escrita é [nao verificado]. Essa identidade não prova origem via disputa nem aprovação. (`app/ponto/services/ausencia.py:627-631`; `app/colaboradores/models.py:351`; `app/chamados/services/materializacao.py:495-528`)

**3) Guarda do emissor, outros escritores e leitores da constante**

`motivos_a_perguntar` executa nesta ordem:

1. Recusa por `dia_sem_trabalho(cel)`; somente se falso, pelo curto-circuito do `or`, verifica colaborador diferente de `ativo`. (`app/chamados/services/fabrica_da_celula.py:89-92`)
2. Recusa data futura. (`app/chamados/services/fabrica_da_celula.py:93-94`)
3. Obtém `agora`, calcula marcos vencidos e recusa veredito preenchido fora de `VEREDITOS_ACUSAM`. (`app/chamados/services/fabrica_da_celula.py:95-100`)
4. Recusa ausência parcial destinada somente à lista administrativa, por `parcial_so_lista`. (`app/chamados/services/fabrica_da_celula.py:101-104`)
5. Carrega perguntas existentes, exclui duplicatas e devolve somente motivos ainda não perguntados. O cálculo dos motivos aplica o conjunto de marcos vencidos e as guardas de ata. (`app/chamados/services/fabrica_da_celula.py:118-126,45-64`)

Para as linhas cuja cláusula impressa é `trabalha_False`, **a primeira guarda basta**, independentemente de o status ser `folga_trabalhada`. (`app/chamados/juizes.py:428`; `app/chamados/services/fabrica_da_celula.py:91-92`; correspondência efetiva das células com a impressão: [nao verificado])

**Sim, existem caminhos fora dessa fábrica que podem criar chamado da célula ou pergunta:**

- **Chamado da célula:** a abertura administrativa aceita tipo catalogado e data do fato; por exemplo, `ausencia_colab` tem escopo DIA e chave `data`. A criação resolve a célula por colaborador/data. As guardas dessa criação não constituem proibição universal de `folga_trabalhada`. (`app/chamados/services/abertura.py:147-153,172-192`; `app/chamados/catalogo/modulos.py:120-122`; `app/chamados/models.py:1026-1039,1073-1076,1097-1106,1193-1201`)
- **Pergunta por batida em folga:** o detector devolve anomalia grave quando a escala diz folga; a API encaminha o motivo à disputa; o helper gera perguntas. A data é extraída do contexto e `PerguntaDisputa.abrir` associa a célula daquele colaborador/data, sem consultar `dia_sem_trabalho` nesse nascimento. (`app/chamados/detector_anomalias.py:164-175`; `app/api/views_core.py:725-730,771-780`; `app/chamados/services/disputa_emissao.py:350-366,389-397,598-618`; `app/chamados/models.py:2428-2430,2476-2477`)

**Nascer não basta para constituir porta alcançável:** o chamado precisa estar na fila; pela pergunta, ela precisa estar viva, no app e sob âncora na fila. Uma pergunta

[truncado em 20000 de 22672 caracteres]

## 06/11 18:25 · sessao 2da3cfdb4a00 · PAUTA-08 · verificador · origem=Code

1. **Fecha o achado da volta 2 sobre congelar o gravado até a gravação da pauta, mas não estabelece essa garantia para toda escrita da reversão.** O comando agora executa o `select_for_update` de `FechamentoMensal`, confere o hash e escreve cabeça, filhas e atualização da reversão dentro do mesmo `atomic` por colaborador. Não identifiquei caminho que persista uma cabeça nova sem essa conferência no mesmo bloco. ([comando:190](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:190), [comando:196](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196), [comando:199](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:199), [comando:207](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207), [comando:217](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:217))

   **Existe, porém, reversão gravada sem conferência no mesmo `atomic`: o JSON inicial.** Ele recebe o plano e os hashes medidos anteriormente, com `pautas=[]`, e é escrito antes do laço e da transação. O próprio teste de hash divergente abre esse arquivo após a recusa. ([comando:120](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120), [comando:162](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:162), [comando:167](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:167), [teste:211](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:211))

   O arquivo também continua fora do rollback do banco: falha no commit posterior à sua atualização pode deixar PKs mortos nele; `_reverter` pula PK inexistente. A comparação final ocorre depois das transações por colaborador. ([comando:214](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:214), [comando:228](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:228), [comando:275](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:275))

2. **O teste novo morde presença e ordem do SQL, não toda a garantia anunciada.** Exige algum `FOR UPDATE` contendo `fechamentomensal` antes do primeiro `INSERT` contendo `pauta`. Não verifica o filtro, linhas bloqueadas, lock anterior ao hash, duração do bloqueio ou concorrência. ([teste:225](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:225), [teste:228](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:228), [teste:231](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:231))

   **Mutante que passaria por análise estática:** mover somente o lock da linha 196 para depois do `if/raise` de hash, antes de `_existente`. Continua havendo lock antes do `INSERT`; o teste de hash adulterado continua recusando antes de criar pautas ou acrescentar PKs ao JSON. As demais verificações seriais preservam seus resultados. Execução dos 26 testes com esse mutante: **[nao verificado]**. ([comando:196](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196), [comando:199](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:199), [comando:202](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:202), [teste:204](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:204), [teste:217](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:217))

   Esse mutante reabre uma janela entre conferência e lock: uma alteração concorrente pode entrar ali, seguida da escrita da cabeça sem reconferência; o hash final só reclamaria depois da saída do `atomic`. ([comando:199](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:199), [comando:207](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207), [comando:218](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:218), [comando:228](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:228))

3. **Sim: a frase nova sobre HE autorizada em exportada corresponde ao código, condicionada a haver autorizações efetivas.** `_ato` chama automaticamente `_linha_para_o_dp` quando `exportada` e `autorizados` são verdadeiros; o emissor chama `pautas.services.escrever`. A confirmação envolve `_ato` em transação e recusa divergência do DIFF mostrado; a prévia desfaz sua transação. ([LEIS.md:177](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/docs/LEIS.md:177), [autorizacao_he_periodo.py:299](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/autorizacao_he_periodo.py:299), **[emissor:244](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/autorizacao_he_periodo.py:244)**, [prévia:321](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/autorizacao_he_periodo.py:321), [confirmação:343](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/autorizacao_he_periodo.py:343))

4. **Escritores e leitores no caminho examinado:**

   | Objeto | Quem escreve/produz | Quem lê |
   |---|---|---|
   | Hash do gravado | `hash_do_gravado` calcula MD5 truncado sobre campos de `FechamentoMensal`; o comando guarda o resultado no plano e no JSON. ([regen_exportada.py:62](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:62), [comando:120](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120), [comando:165](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:165)) | O comando compara por colaborador e no fim. A porta REGEN calcula antes/depois, envia à auditoria e devolve `mudou`. ([comando:199](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:199), [comando:228](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:228), [regen_exportada.py:206](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:206), [regen_exportada.py:218](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:218)) |
   | `FechamentoMensal` | PAUTA-08 pede somente leitura. No ramo escritor relacionado, `recalcular_fechamento_mes` usa `get_or_create`, `save` ou `update`; REGEN chama esse ramo. ([comando:135](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135), [fechamento.py:889](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/fechamento.py:889), [fechamento.py:1128](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/fechamento.py:1128), [fechamento.py:1144](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/fechamento.py:1144), [regen_exportada.py:179](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:179)) | `hash_do_gravado`, `valores_do_gravado`, recálculo que semeia objeto destacado e novo SELECT de bloqueio. ([regen_exportada.py:71](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:71), [regen_exportada.py:81](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:81), [fechamento.py:881](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/fechamento.py:881), [comando:196](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196)) |
   | Arquivo de reversão | `_escrever` chama `_grava` inicialmente e após acrescentar PKs; `_grava` serializa em temporário e executa `os.replace`. ([comando:167](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:167), [comando:216](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:216), [comando:302](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:302)) | `_reverter` carrega JSON, valida marca/schema e percorre PKs para cancelar pautas; **não reconfere `hash_gravado` do plano**. ([comando:262](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:262), [comando:266](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:266), [comando:273](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:273), [comando:283](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:283)) |

   Ocorrência desses cenários em produção: **[nao verificado]**.

## 06/11 18:23 · sessao 88a1e5c18480 · - · - · origem=Ronald (tmux)

O teste novo **morde a presença e a ordem textual do SQL**, não a garantia inteira anunciada: procura qualquer `FOR UPDATE` em `fechamentomensal` antes do primeiro `INSERT` contendo `pauta`; não verifica se o lock antecede o hash, quais linhas foram selecionadas ou uma disputa concorrente. (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:225`, `:228`, `:229`, `:231`)

**Mutante preferível:** mover somente o lock de `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196` para depois da comparação/recusa de hash (`:199`–`:201`), antes de `_existente` (`:202`). O SQL continua antes do `INSERT` (`:207`), satisfazendo o novo teste; o teste do hash adulterado continua recusando antes das criações e preservando a lista vazia do JSON. (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:204`, `:213`, `:214`, `:217`, `:231`)

Esse mutante permite alteração concorrente **entre a conferência e a aquisição do lock**: a pauta pode ser criada sem nova conferência depois da alteração; a comparação final só ocorre após sair do `atomic` daquela pessoa. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:190`, `:199`, `:207`, `:218`, `:228`)

Outro mutante explícito: acrescentar `pk__isnull=True` ao filtro do lock, gerando `SELECT … FOR UPDATE` sem travar linha alguma; o teste não inspeciona o filtro nem o resultado. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:228`, `:231`)

A previsão de que esses mutantes passam pelos 26 testes decorre da inspeção estática das asserções; execução dos testes e dos mutantes: **[nao verificado]**. [nao verificado]

_2413 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
