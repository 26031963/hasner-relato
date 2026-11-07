# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 21:26** · publicado em **06/11/2026 21:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 21:26 · sessao d5f797084ef3 · - · - · origem=Ronald (tmux)

Ampliação fechada: grupo **trocar vínculo de escala** passa a ter 3 telas e 3 pares: manual–lista, manual–modal e lista–modal; todas convergem em `julgar_vinculo` / `executar_vinculo` (`app/colaboradores/views.py:411`, `app/colaboradores/views.py:423`, `app/escala/regua_defesa.py:316`, `app/escala/regua_defesa.py:321`).

Tela manual: GET `/colaboradores/vincular/<cid>/painel/` → `painel_vinculo`, POST `/colaboradores/<cid>/vincular/escala/` → `vincular_escala`; formulário `colaboradores/partials/painel_vinculo.html` (`app/colaboradores/urls.py:12`, `app/colaboradores/urls.py:13`, `app/colaboradores/views.py:262`, `app/colaboradores/views.py:373`, `app/templates/colaboradores/partials/painel_vinculo.html:136`).

Os dois novos pares diferem porque manual recebe template, início e âncora humanos; as duas telas sugeridas fixam hoje e recusam ciclo antes de chamar o juiz. Manual permite retroagir até a janela anterior e criar ciclo com âncora declarada (`app/templates/colaboradores/partials/painel_vinculo.html:155`, `app/templates/colaboradores/partials/painel_vinculo.html:159`, `app/templates/colaboradores/partials/painel_vinculo.html:167`, `app/escala/regua_defesa.py:310`, `app/escala/regua_defesa.py:313`, `app/colaboradores/services/vinculo.py:69`, `app/colaboradores/services/vinculo.py:126`).

**Muda dia/dinheiro? Sim, potencial:** o início escolhido determina o trecho regenerado até hoje; regeneração padrão recalcula por evento as competências tocadas. Ocorrência efetiva em produção [não verificado] (`app/colaboradores/services/vinculo.py:320`, `app/ponto/portas/celula.py:110`, `app/ponto/portas/celula.py:354`, `app/ponto/portas/celula.py:445`, `app/ponto/portas/celula.py:458`). Leitor do dia: `eh_dia_trabalho` retorna `CelulaDia.trabalha` (`app/escala/models.py:1094`).

Copiar tipo GET versus criar pelo wizard **não recomendo contar como par**: os comandos de tela são explicitamente “Copiar esta escala” e “Nova escala”, e não há segunda tela comprovada para o mesmo comando copiar (`app/templates/escala/tipos_lista.html:90`, `app/escala/views.py:108`). A cópia parcial existe: `copiar_tipo` cria com subconjunto explícito e não inclui calendário padrão, override por dia, HE fixa e duração do intervalo; isso é diferença entre comandos distintos (`app/escala/services/cadastro_tipo.py:367`, `app/escala/models.py:89`, `app/escala/models.py:203`, `app/escala/models.py:78`, `app/escala/models.py:110`).

## 06/11 21:26 · sessao 2f360630892b · - · - · origem=Ronald (tmux)

Confirmado: **não contar “recalcular” × “gerar TXT” como par da mesma ação**. Recalcular persiste o fechamento (`app/ponto/views.py:1252`); gerar TXT solicita `somente_leitura=True` (`app/folha/views.py:208`), cujo ramo entrega objetos sem salvar (`app/ponto/services/fechamento.py:1121`), depois aprova e registra exportação (`app/folha/views.py:252`, `:297`).

Correção ao inventário de telas: aprovação **selecionada** está acessível pelo JavaScript em `app/templates/ponto/fechamento.html:143`. Aprovação **cega**, recalcular antigo e reabrir competência estão dentro do bloco `display:none` aberto em `:152`, com formulários em `:209`, `:189`, `:223`, fechado em `:234`. O recalcular visível está em `:87`. Portanto, apresentar o cego como **endpoint existente com formulário oculto**, sem presumir botão acessível.

Reabrir individual e reavaliar vazios provêm da **mesma tela de fechamento**: `app/templates/ponto/_fech_linha.html:42` e `app/templates/ponto/fechamento.html:76`. A linha é incluída tanto na página (`:262`) quanto no fragmento de paginação (`app/templates/ponto/_fech_lote.html:8`), sem segunda experiência de tela independente comprovada.

Leitor relevante da diferença de carimbo: `competencia_fechada` consulta fechamento da pessoa e recusa `aprovado/exportado` (`app/ponto/janelas.py:86`, `:87`). O registro institucional de batida recusa antes de criar (`app/ponto/registro_batida.py:132`, `:137`); flip recusa antes de alterar (`app/ponto/services/flip_batida.py:21`, `:24`). Isso altera a possibilidade de regularizar posteriormente; mudança efetiva de dinheiro/dia por tentativa posterior é **[não verificado]**. O ato de aprovação isolado só grava carimbo/observação (`app/ponto/services/fechamento.py:1322`, `:1330`).

## 06/11 21:25 · sessao 4ab21ee7a095 · BUG-PLANTIO-NOTURNO-MAIS-1 · censo · origem=Code

**Não atribuo descarte à triagem sem evidência da decisão.** A regra exige registrar o motivo do descarte; “foi visto” e “foi descartado” precisam ser demonstrados separadamente. (`CLAUDE.md:368`, `CLAUDE.md:377`)

Os trechos abaixo são transcrições dos anexos fornecidos; sua numeração física em disco é [nao verificado].

1. **(a) — falha do Codex no núcleo “troca de significado sem backfill”, com percepção parcial do passivo.**

   **Pergunta exata — v1.q, item 6:**
   > pouso antes do pre-requisito; campo com dois significados. [nao verificado]

   **Pergunta exata — f1.q:**
   > quantas perguntas VIVAS (aberta/respondida) com D_e_dia_da_saida -- essas sao as que mudam de leitura quando o leitor migrar

   **Resposta exata — v1.r, item 6:**
   > Suite, pouso e saneamento do passivo: **[nao verificado]**. A costura não converte datas já preenchidas, pois só data perguntas sem dia. [nao verificado]

   **Resposta exata — f1.r:**
   > **(d) VIVAS em `D_e_dia_da_saida`: 81**

   O tema estava perguntado, mas a resposta não denunciou a incompatibilidade entre migrar os leitores e conservar as datas antigas. A costura realmente só preenche perguntas sem data. (`app/chamados/services/fio.py:832`)

   **Triagem:** existe a decisão “passivo so depois da cura no ar”, mas ela não prova descarte de um alerta explícito do Codex sobre essa incompatibilidade. (`app/docs/RELATO.md:41`) Os recortes “sem marcos” e “plantio em D−1” não foram medidos pela saída fornecida. [nao verificado]

2. **(a) — falha do Codex: reconheceu a retirada do recuo, mas não a regressão sobre perguntas antigas.**

   **Pergunta exata — v1.q, item 1:**
   > Ha algum par escritor/leitor em que os significados divergem (ESMERIL-SEMANTICA: unidade, fuso, dia do turno x dia civil x instante)?

   **Resposta exata — v1.r, item 6:**
   > Há asserts que atingem o defeito principal: escritor retorna D, saída planta em D+1, detector grava D e sem marcos não recua.

   A resposta tratou “sem marcos não recua” como comportamento correto, sem separar perguntas novas das antigas, cujo D ainda significava dia da saída. Na cópia, o ramo antigo recua a entrada quando ela ultrapassa a hora da saída. (`app/chamados/juizes.py:315`, `app/chamados/juizes.py:320`)

   **Triagem específica desse risco:** [nao verificado]. As 21 vivas são um teto do recorte SSE fornecido; quantas efetivamente estão sem marcos permanece [nao verificado].

3. **(a) — falha do Codex: inventariou os leitores, mas perdeu o efeito da retirada de D−1 sobre plantios existentes.**

   **Pergunta exata — v1.q, item 1:**
   > Para cada leitor: com que significado ele le (dia do turno, dia civil da batida, dia da saida, dia em que a pergunta foi feita).

   **Resposta exata — v1.r, inventário:**
   > `_plantio_na_hora_do_slot` usa D/D+1

   > `_pergunta_materializada_de_fato`, `_pergunta_viva_calc`

   A resposta mencionou os componentes, mas não apontou a cadeia de perda de reconhecimento → pergunta viva → descarimbo. A cópia mostra a busca antiga incluindo D−1, seu uso como prova de materialização e o descarimbo condicionado a `pergunta_viva`. (`app/chamados/juizes.py:265`, `app/chamados/juizes.py:724`, `app/chamados/services/relavra_via.py:227`, `app/chamados/services/relavra_via.py:231`)

   A fábrica chama esse descarimbo durante a emissão pelo dia. (`app/chamados/services/fabrica_da_celula.py:136`) **Triagem específica:** [nao verificado].

4. **(c) — RED histórico fora do alcance demonstrado; não há certificação do defeito no main.**

   **Pergunta exata — v3.q, item 4:**
   > O RED test_MORDE_saida_sem_dia_do_turno_nao_vira_500 morde o pai (39324b76)?

   **Resposta exata — v3.r, item 4:**
   > **Há mordida dedutiva contra o pai descrito no anexo; execução contra `39324b76`: [nao verificado].**

   **Resposta exata — v4.r, item 3:**
   > objeto e execução contra `39324b76`: **[nao verificado]**.

   O alvo explícito dessas perguntas era o pai intermediário. A resposta não afirmou execução nem prova contra o main original. O relato registra a correção dos “500 nos gemeos” como procedente, mas não demonstra sua existência no main. (`app/docs/RELATO.md:11`) A atribuição histórica aos commits indicados pelo opus permanece [nao verificado].

5. **VISTO; (b) comprovado apenas parcialmente, sem fechar a imputação do núcleo inteiro.**

   **Pergunta exata — v2.q, item 1:**
   > agora deixa de nascer pergunta e some sem rastro alem do log?

   **Pergunta exata — v2.q, item 2:**
   > Ha outro emissor (varredura, cron) que a pega depois, ou ela fica invisivel?

   **Resposta exata — v2.r, item 1:**
   > A recusa específica, entretanto, fica apenas no `logger.info`, sem pergunta ou pendência própria.

   **Resposta exata — v2.r, item 2:**
   > A guarda então pula a disputa e a triagem pode chegar à indicação de sucesso, sem registrar pendência própria nesse ramo.

   Portanto, o silêncio foi explicitamente visto. [nao verificado quanto às linhas físicas dos anexos]

   **Triagem localizada:** o efeito relacionado “push sem pergunta” foi colocado em fila. (`app/docs/BACKLOG.md:1798`) A linha que descarta especificamente a ausência de pendência, prazo, tripwire ou contador é [nao verificado]. Não há base para chamar esse núcleo de falha do Codex nem para fechar integralmente **(b)**.

6. **(a) parcial — o lote `dias_do_fato` não foi apontado; a derivação civil singular foi parcialmente vista.**

   **Pergunta exata — v1.q, item 7:**
   > Ha fallback disfarcado ("se nao achar, usa o dia do timestamp", `or data_entrada`, default de dia) restante em algum escritor tocado ou vizinho?

   **Resposta exata — v1.r, item 7:**
   > O derivador aceita o dia do chamado; esse juiz tem degraus de dia civil da batida e timestamp cru do contexto.

   A resposta reconheceu a família “dia civil da batida”, mas não identificou a duplicação no lote. A cópia confirma ambas as derivações por `localdate(timestamp)`. (`app/chamados/juizes.py:50`, `app/chamados/juizes.py:193`)

   **Triagem exata — v2.q:**
   > FILA: _dia_do_contexto_fraco (juizes.py)

   Essa linha trata do contexto fraco; não demonstra descarte específico de `_dia_da_batida` e `dias_do_fato`. Esse descarte é [nao verificado].

7. **Núcleo VISTO; (b) não comprovado. (a) parcial quanto ao comentário antigo.**

   **Pergunta exata — v1.q, item 6:**
   > derivacao paralela de fonte unica (ex.: dia do turno derivado sem _T1); selo/RED sem mordida

   **Resposta exata — v1.r, item 6:**
   > o construtor de célula apenas devolve a data recebida.

   > `NewType` não estabelece uma validação semântica nessa porta.

   > Os testes novos verificam ausência/`None`, não proveniência do dia. [nao verificado]

   O núcleo do achado — cast sem validação de significado e selo insuficiente para provar proveniência — já estava explicitamente na resposta. [nao verificado quanto às linhas físicas dos anexos]

   **Triagem exata — v2.q:**
   > mypy so checa chamador anotado, selo nao segue **dict/setattr por variavel/SQL cru

   Essa fila cobre limitações relacionadas, mas não comprova descarte do alerta semântico específico. **(b) integral:** [nao verificado]. O comentário que ainda promete “data correta da SAIDA” permanece na cópia e não foi denunciado especificamente nas respostas. (`app/escala/utils.py:913`)

Regras preventivas propostas para as classes identificadas:

- **(a):** Toda troca semântica deve percorrer uma pergunta antiga até seus leitores e efeitos de escrita, incluindo reconhecimento e descarimbo. [nao verificado]
- **(b), parcial:** Cada alerta deve receber triagem própria com decisão, motivo e cura ou adiamento identificável; uma fila vizinha não substitui essa linha. (`CLAUDE.md:368`)
- **(c):** Toda alegação de RED histórico deve identificar o hash original e evidenciar falha pelo motivo correto nesse hash. [nao verificado]

**Não atribuo (d):** a regra local reserva MODO-COMUM ao que Codex **e** opus deixaram passar; os sete achados foram apresentados pelo opus. (`CLAUDE.md:385`; anexo `opus_final.txt` [nao verificado quanto às linhas físicas])

## 06/11 21:24 · sessao 53ecda1d422f · - · - · origem=Ronald (tmux)

Complemento folga manual, com correção do disparador:

- **Plano → aplicar lote:** `/escala/plano-folgas/` → `/escala/plano-folgas/aplicar/`, `app/escala/urls.py:8`, `:10`; view `app/escala/views_plano_folgas.py:144` chama `aplicar_folgas_em_lote` em `:185`; template dispara POST em `app/templates/escala/plano_folgas.html:171`; escritor `app/escala/folgas.py:362` → `_foto_do_dia` cria em `:337`, altera em `:342` ou apaga em `:333`.
- **Detalhe → aceitar `FOLGA_CONTESTA`:** `/chamados/local/<id>/`, `app/chamados/urls.py:14`; view `app/chamados/views.py:359`, ramo `:483`, chamada `:501`; template `app/templates/chamados/detalhe_local.html:310` inclui `app/templates/chamados/partials/_pergunta_card.html:92`, POST com ação em `:95`.
- **Modal → aceitar:** `/chamados/modal/<colaborador>/`, `app/chamados/urls.py:39`; view `app/chamados/views.py:869` delega em `:954`; template `app/templates/chamados/partials/modal_fio.html:274`, ação `:276`.
- **Inline → aceitar:** `/chamados/validar-inline/<cid>/`, `app/chamados/urls.py:33`; view `app/chamados/views_cobrar.py:145` chama em `:208`; botão `app/templates/chamados/partials/_lista_chamados.html:82`.

Os três aceites convergem em `app/chamados/services/acoes_disputa.py:19` → `validar_pergunta` reconhece folga em `app/chamados/services/validacao.py:382` → `_aceitar_a_folga` chama `declarar_dia` em `:291` → `app/ponto/services/declarar_dia.py:189` → escritor `lancar_folga`, `app/escala/folgas.py:280`, `:312`.

**Um grupo Plano × aceites, três pares atômicos divergentes:**

1. Plano recusa qualquer dia anterior ao início da competência atual: `app/escala/folgas.py:379`, `:385`. Aceite permite dia com até 120 dias de antiguidade: constante `app/ponto/services/declarar_dia.py:53`, verificação `:101`.
2. Plano aceita tipo escolhido `F1…F9/FD`, `app/escala/views_plano_folgas.py:173`; aceite escreve `FD`, `app/ponto/services/declarar_dia.py:190`.
3. Plano só pede relavra em `app/escala/folgas.py:395`; aceite também fecha perguntas vivas e tenta reconciliar chamados: `app/ponto/services/declarar_dia.py:204` → `app/ponto/services/veredito_celula.py:174`, `:177`, `:182`, `:183`.
4. Aceite carimba pergunta, fecha sua via administrativa e registra `val_perg_folga`: `app/chamados/services/validacao.py:290`, `:299`, `:300`; Plano não passa por esse escritor, conforme cadeia `app/escala/views_plano_folgas.py:185` → `app/escala/folgas.py:362`.

**Leitor e gravidade:** `app/ponto/precedencia.py:356` lê a folga e `:384` a transforma em fato `folga`. **Muda dia: sim, potencialmente**, sobretudo para dia anterior à competência atual que o Plano recusa e o aceite pode gravar; ocorrência e dinheiro efetivo em produção [não verificado]. Ambas as rotas relavram por `app/escala/folgas.py:272` e preservam a guarda de competência entregue em `app/ponto/portas/celula.py:214`.

**Convergência dos aceites com validação divergente:** detalhe só admite disputa aberta (`app/chamados/views.py:495`); inline também procura aberta (`app/chamados/views_cobrar.py:160`). Modal admite pergunta viva em disputa fechada (`app/chamados/services/fio.py:596`, `:601`, `:605`) e acrescenta trilha em `:611`. Portanto detalhe × modal e inline × modal são **mais dois pares divergentes**, embora compartilhem escritor; detalhe × inline converge no mesmo escritor, sem divergência adicional comprovada para folga.

**Fronteira varrida:** quatro views aptas ao aceite — detalhe, fio, modal e inline, nas referências acima e `app/chamados/views.py:659`, `:674`. O template do fio completo só tem formulário de mensagem/status (`app/templates/chamados/fio_colaborador.html:257`); disparador visual `validar_pergunta` nessa tela [não verificado], então não o contei nos três pares Plano × telas.

`validar_lote` e `validar_lote_tela` **não escrevem folga**: `FOLGA_CONTESTA` vira classe C por substring `folga`, `app/chamados/services/validacao.py:755`, `:806`, `:808`; lote recusa qualquer classe diferente de A em `:688`. Toggle tem view escritora em `app/escala/views_plano_folgas.py:98`, mas disparador em template [não verificado].

## 06/11 21:24 · sessao f30767be6376 · - · - · origem=Ronald (tmux)

Fechamento cruzado: **há duas representações de férias que as telas podem alterar separadamente**; não encontrei sincronização inversa `Ausencia → AgendamentoFerias` nos escritores/modelo/signals examinados. Evidências: `app/ponto/models.py:868` até o `super().save()` em :908; receivers de Ausencia em `app/ponto/signals.py:143`, :150, :275 e :282; consumidor limita-se a perguntas/chamados em signals:250. Sincronização existente tem sentido `AgendamentoFerias → Ausencia`, `app/ferias/models.py:434` → `_sync_ausencia`, models:379 → `app/ponto/services/ausencia.py:1160`.

| Ação cruzada | Escritores/efeitos diferentes | Leitor e gravidade |
|---|---|---|
| Conceder férias via `/ponto/ausencias/lancar/` × agendar férias | `lancar_ausencia`, `app/ponto/views.py:2368`, chama `criar_ausencia` com tipo recebido e status aprovada em :2415. Produz apenas Ausencia; montagem dos campos em `app/ponto/services/ausencia.py:341`. Já agendamento escreve Agendamento e `periodo.dias_concedidos`, `app/ferias/services.py:321`, e espelha Ausencia por model save, models:434. | Espelho vê cobertura aprovada, `app/ponto/services/espelho.py:588`; saldo depende de concedidos/vendidos, `app/ferias/models.py:216`; folha procura **Agendamento** em `app/folha/export.py:491`, retenção :868. **Sim, muda dia/saldo/retenção potencialmente**: conceder só Ausencia abona cobertura sem consumir saldo nem compor o mesmo universo de férias com batida. Ocorrência real **[não verificado]**. |
| Justificar dia com tipo férias × agendar férias | `/ponto/celula/veredito/`, `app/ponto/urls.py:58`, view `veredito_celula_view`, `app/ponto/views.py:3312`, passa tipo da tela em :3341. Escritor `veredito_celula`, `app/ponto/services/veredito_celula.py:191`, cria apenas Ausencia aprovada em :315 e ainda fecha fio em :325, lavra célula :330 e registra log :334. Não cria Agendamento/saldo; contraste com services férias:321. | Mesmos leitores/impacto acima; efeito adicional no fio/célula só nesse caminho, veredito_celula:324. |
| Editar Ausencia de férias × editar datas do Agendamento | `editar_ausencia`, chamada em `app/ponto/views.py:2794` → `editar_ausencia_registro`, `app/ponto/services/ausencia.py:465`, altera só tipo/range/minutos da Ausencia em :498, salva :509 e invalida fechamento :524. Editar Agendamento usa `_trocar_janela`, `app/ferias/services.py:366`, muda também Agendamento/concedidos e remove Ausencias antigas em :378. | **Sim, muda dia**: a cobertura pode sair da janela original sem mover a janela do Agendamento, preservando saldo e retenção calculada sobre o intervalo antigo; leitores espelho:588 e export:491. Existência desse estado em produção **[não verificado]**. |
| Cancelar Ausencia de férias × cancelar Agendamento | Cancelamento lote não exclui tipo férias: busca qualquer Ausencia não rejeitada, `app/ponto/views.py:2345`, decide rejeitada em :2351. `decidir`, `app/ponto/services/ausencia.py:897`, muda estado e salva campos da Ausencia, :911/:927. Cancelar Agendamento atualiza concedidos/status/obs, `app/ferias/services.py:352`, e save remove espelhos, `app/ponto/services/ausencia.py:1188`. | **Sim, muda dia**: cancelamento somente da Ausencia retira cobertura do espelho, mas Agendamento continua no universo `ferias_na_janela` enquanto não cancelado, `app/ferias/services.py:51`; folha ainda pode reter por férias com batida, export:868; saldo não recebe devolução. Ocorrência real **[não verificado]**. |

**Condição de disponibilidade do tipo:** catálogo congelado inclui `ferias`, `app/ponto/catalogo/ausencias.py:101`; `vigente()` usa banco ou catálogo congelado, :278; choices agrupadas incluem todos os códigos, :313; painel do colaborador exclui só `falta`, `app/colaboradores/views.py:1959`. Portanto os caminhos aceitam férias se o catálogo vigente a contiver; disponibilidade efetiva de férias no cadastro de produção **[não verificado]**. `Ausencia.exige_documento` declara exceção de férias no fluxo próprio, `app/ponto/models.py:727`, reforçada por `FORA_DO_A4 = ('ferias',)`, `app/ponto/services/ausencia.py:751`; não é veto de criação pelo admin.

**Venda histórica:** `criar_periodo_aquisitivo` lê `dias_vendidos`, `app/ferias/views.py:194`, e passa para `registrar_gozo_historico`, :210; escritor incrementa vendidos junto do gozo, `app/ferias/services.py:300`, enquanto venda separada usa `vender_dias_do_periodo`, services:507. Porém os templates examinados não oferecem campo de vendidos no formulário histórico: `app/templates/ferias/_secao_colab.html:6`–:33 e `criar_periodo.html:9`–:30. Assim o segundo **escritor** está comprovado, mas o segundo **disparo de tela de venda** continua **[não verificado]**; não contar como par de ação visual fechado sem achar formulário adicional.

## 06/11 21:23 · sessao d5f797084ef3 · - · - · origem=Ronald (tmux)

Escala fechada em leitura estática. Caminho base `/escala/`: `app/config/urls.py:27`.

**Universo:** 9 funções com escrita POST, em 11 rotas; 1 função POST adicional sem persistência (`wizard_preview`); 2 escritores disparados por GET (`copiar_tipo_escala`, `excluir_tipo_escala`). As duas rotas extras de POST são criar/editar calendário e criar/editar tipo; preview declara e implementa objeto em memória (`app/escala/urls.py:24`, `app/escala/urls.py:25`, `app/escala/urls.py:35`, `app/escala/urls.py:36`, `app/escala/views_wizard.py:226`, `app/templates/escala/tipos_lista.html:90`, `app/templates/escala/tipos_lista.html:92`).

Lista das 9 funções escritoras POST: `escala_remove_colab` (`app/escala/views.py:256`), `editar_folga_calendario` (`app/escala/views.py:316`), `aplicar_folga_calendario` (`app/escala/views.py:376`), `corroborar_regua_da_lista` (`app/escala/views.py:456`), `realocar_regua_da_lista` (`app/escala/views.py:463`), `wizard_salvar` (`app/escala/views_wizard.py:98`), `wizard_calendario_criar` (`app/escala/views_wizard.py:140`), `plano_folgas_toggle` (`app/escala/views_plano_folgas.py:98`) e `plano_folgas_aplicar` (`app/escala/views_plano_folgas.py:144`).

**Par divergente inteiramente dentro de escala:**

| Ação | Tela A → escritor | Tela B → escritor | Diferença / leitor / gravidade |
|---|---|---|---|
| Criar calendário de folgas | `/escala/folgas/criar/`, `editar_folga_calendario` (`app/escala/urls.py:35`, `app/escala/views.py:316`), formulário `escala/folga_calendario_editar.html` (`app/templates/escala/folga_calendario_editar.html:9`) → `salvar_calendario_de_folga` (`app/escala/views.py:356`, `app/escala/folgas.py:548`) | `/escala/tipos/wizard/calendario/criar/`, `wizard_calendario_criar` (`app/escala/urls.py:20`, `app/escala/views_wizard.py:140`), botão `escala/wizard_tipo_escala.html` (`app/templates/escala/wizard_tipo_escala.html:160`) → `criar_calendario_de_folga` (`app/escala/views_wizard.py:171`, `app/escala/views_wizard.py:192`, `app/escala/folgas.py:577`) | Escritores diferentes. Editor recebe dias independentes para 28/30/31; wizard replica uma lista aos três tamanhos com clamp (`app/escala/views.py:340`, `app/escala/folgas.py:570`, `app/escala/folgas.py:590`). Pares semana/dia malformados são descartados pelo editor, mas recusados com 400 pelo wizard (`app/escala/views.py:333`, `app/escala/views_wizard.py:163`). Ambos convergem apenas no veto de nome repetido (`app/escala/folgas.py:539`, `app/escala/folgas.py:554`, `app/escala/folgas.py:584`). Leitor: `datas_que_o_calendario_manda` → `gerar_foto_folgas` → `FolgaDia` e relavra (`app/escala/folgas.py:43`, `app/escala/folgas.py:150`, `app/escala/folgas.py:180`, `app/escala/folgas.py:203`). **Não muda dia/dinheiro imediatamente:** criar grava só calendário/tamanhos; há potencial de dias diferentes depois da materialização (`app/escala/folgas.py:548`, `app/escala/folgas.py:577`, `app/escala/folgas.py:150`). |

**Pares de fronteira comprovados; segunda tela em chamados:**

1. **Confirmar cadastro de escala errado.** Tela A `/escala/cadastro-x-realidade/` (`app/escala/views.py:476`, render `app/escala/views.py:492`), formulário em `app/templates/escala/cadastro_x_realidade.html:28`, POST `/escala/cadastro-x-realidade/regua/<ec_pk>/corroborar/` → `corroborar_regua_da_lista` → **converge em `corroborar_regua`** (`app/escala/urls.py:15`, `app/escala/views.py:456`, `app/escala/views.py:459`, `app/escala/regua_defesa.py:647`). Tela B modal `/chamados/modal/<colaborador_id>/` (`app/chamados/urls.py:39`, `app/chamados/views.py:869`, render `app/chamados/views.py:927`), template `chamados/partials/modal_fio.html`, formulário POST `/chamados/local/<chamado_id>/` com `acao=corroborar_regua` (`app/templates/chamados/partials/modal_fio.html:559`, `app/templates/chamados/partials/modal_fio.html:561`, `app/chamados/urls.py:14`, `app/chamados/views.py:359`, `app/chamados/views.py:438`). **Difere:** lista escolhe EC pela PK; modal escolhe vínculo ativo pelo colaborador e tipo do contexto; só modal passa `chamado` e anexa ocorrência (`app/escala/views.py:449`, `app/chamados/views.py:441`, `app/chamados/views.py:443`, `app/escala/regua_defesa.py:670`). **Não há diferença de dia/dinheiro comprovada para o mesmo EC:** campos são gravados na mesma chamada; diferença comprovada é ocorrência e seleção do alvo (`app/escala/regua_defesa.py:662`). Leitor de cobrança comum: `vetar_cobranca_se_regua_suspeita`, detector (`app/escala/regua_defesa.py:554`, `app/ponto/management/commands/detectar_ausencias.py:603`). Leitor da diferença: followups do chamado (`app/chamados/views.py:372`).

2. **Realocar escala para destino sugerido.** Mesmas telas A e B acima; A POST `/escala/cadastro-x-realidade/regua/<ec_pk>/realocar/` → `realocar_regua_da_lista` → `realocar_vinculo_para_destino` (`app/escala/urls.py:17`, `app/escala/views.py:463`, `app/escala/views.py:471`, formulário `app/templates/escala/cadastro_x_realidade.html:28`). B POST `/chamados/local/<chamado_id>/`, `acao=realocar_regua` → `realocar_para_destino` → **converge em `realocar_vinculo_para_destino`** → `julgar_vinculo` / `executar_vinculo` (`app/templates/chamados/partials/modal_fio.html:568`, `app/templates/chamados/partials/modal_fio.html:570`, `app/chamados/views.py:449`, `app/escala/regua_defesa.py:326`, `app/escala/regua_defesa.py:329`, `app/escala/regua_defesa.py:316`, `app/escala/regua_defesa.py:321`). **Difere:** lista usa destino da última lavra; modal usa destino persistido no contexto do chamado e só ele anexa ocorrência de realocação (`app/escala/views.py:469`, `app/escala/regua_defesa.py:328`, `app/escala/regua_defesa.py:333`). **Pode mudar dia/dinheiro se os destinos divergirem:** destinos efetivamente recebidos iguais em produção [não verificado]; o escritor cria vínculo desde hoje e regenera células (`app/escala/regua_defesa.py:313`, `app/colaboradores/services/vinculo.py:321`). Leitor de dia: `EscalaColaborador.eh_dia_trabalho` lê `CelulaDia.trabalha` (`app/escala/models.py:1069`, `app/escala/models.py:1094`).

3. **Gravar folga semanal no template.** A wizard editar tipo `/escala/tipos/<tid>/wizard/`, POST `/escala/tipos/<tid>/wizard/salvar/`, template `escala/wizard_tipo_escala.html` (`app/escala/urls.py:23`, `app/escala/urls.py:25`, `app/templates/escala/wizard_tipo_escala.html:15`, `app/templates/escala/wizard_tipo_escala.html:128`), view `wizard_salvar` → **converge em `salvar_tipo`** (`app/escala/views_wizard.py:98`, `app/escala/views_wizard.py:108`, `app/escala/services/cadastro_tipo.py:140`). B modal citado acima, `acao=aceitar_folga_limbo` → `detalhe_chamado_local` → `aceitar_sugestao` → mesmo `salvar_tipo` (`app/templates/chamados/partials/modal_fio.html:545`, `app/chamados/views.py:431`, `app/chamados/services/limbo_folgas.py:73`, `app/chamados/services/limbo_folgas.py:91`). **Difere:** B exige sugestão fixa do contexto, grava LogAuditoria e reconcilia/anexa nota nos chamados limbo do template; A recebe dias do formulário e não executa esse bloco (`app/chamados/services/limbo_folgas.py:81`, `app/chamados/services/limbo_folgas.py:94`, `app/chamados/services/limbo_folgas.py:98`, `app/escala/views_wizard.py:108`). **Para mesmos dias, diferença comprovada em dinheiro/dia: não; trilha e chamados: sim.** Escritor do campo comum `folga_dia_semana` e regeneração comuns (`app/escala/services/cadastro_tipo.py:259`, `app/escala/services/cadastro_tipo.py:320`). Eventual efeito financeiro da reconciliação extra [não verificado].

**Candidatos que não devem entrar na contagem de pares fechados:**

- Aplicar calendário coletivo vs individual: escritor coletivo e rota existem (`app/escala/urls.py:37`, `app/escala/views.py:376`), mas disparador em tela coletiva [não verificado]. Tela individual comprovada: POST `/colaboradores/<cid>/vincular/folga/`, view `vincular_folga`, template `colaboradores/partials/painel_vinculo.html` (`app/colaboradores/urls.py:17`, `app/colaboradores/views.py:212`, `app/templates/colaboradores/partials/painel_vinculo.html:191`). Ambos convergem em `trocar_calendario_de_folga` e `gerar_foto_folgas` (`app/escala/views.py:409`, `app/escala/views.py:411`, `app/colaboradores/views.py:239`, `app/colaboradores/views.py:245`). Só individual registra `HistoricoVinculo`; coletivo faz relavra externa extra com autor e retorna barrados (`app/colaboradores/views.py:241`, `app/escala/views.py:416`, `app/escala/views.py:424`). **Não afirmar que individual deixa célula velha:** gerar foto relavra internamente (`app/escala/folgas.py:203`). Leitor do histórico: `app/colaboradores/views.py:306`, `app/templates/colaboradores/partials/painel_vinculo.html:53`.
- Toggle vs lote: endpoints diferentes, mas só lote tem disparador encontrado na mesma tela `plano_folgas.html`; segunda tela do toggle [não verificado] (`app/escala/urls.py:9`, `app/escala/urls.py:10`, `app/templates/escala/plano_folgas.html:171`). Diferença real do código: lote recusa datas anteriores à competência corrente; caminhos individuais não têm essa guarda antes da foto (`app/escala/folgas.py:379`, `app/escala/folgas.py:385`, `app/escala/folgas.py:309`, `app/escala/folgas.py:354`).
- Django admin de TipoEscala: registrado como `admin.ModelAdmin` puro; EscalaColaborador está protegido por `SoLeitura` (`app/escala/admin.py:7`, `app/escala/admin.py:8`, `app/escala/admin.py:13`). Rota exata, template e implementação da view/escritor herdado não constam da cópia examinada [não verificado]. Não contar como par escritor fechado.

## 06/11 21:23 · sessao 1cdcaaaf806b · - · - · origem=Ronald (tmux)

Censo `chamados`: **22 views POST com ramo administrativo**, incluindo quatro de regularização externa; **27 verbos semânticos**, ou **25 famílias** juntando assumir/liberar e autorizar geofence com/sem advertência. Inventário abaixo; a contagem inclui escritores HTTP existentes mesmo quando o formulário atual não foi encontrado. (`app/chamados/urls.py:10`, `app/chamados/urls.py:25`, `app/chamados/urls.py:48`)

Views:

- `views.py`: `abrir_chamado:147`, `adicionar_followup:274`, `detalhe_chamado_local:359`, `fio_colaborador:659`, `modal_fio_colaborador:869`, `abrir_chamado_rapido:961`, `atribuir_chamado:1008`, `conferir_data_pergunta:1292`, `reabrir_disputa_supervisao:1372`.
- `views_cobrar.py`: `cobrar_chamado:24`, `cobrar_massa:61`, `cobrar_dia_view:306`, `validar_inline:145`, `validar_lote:232`, `validar_lote_tela:372`.
- `views_arquivar.py`: `arquivar_chamado:24`, `desarquivar_chamado:52`, `arquivar_lote:93`.
- `views_regularizacao.py`: `regularizacao_criar:42`, `regularizacao_add_item:74`, `regularizacao_del_item:137`, `regularizacao_enviar:154`.

**Pares divergentes confirmados**

1. **Validar pergunta: detalhe × modal.** Detalhe `/chamados/local/<id>/`, view `app/chamados/views.py:359`, formulário `app/templates/chamados/partials/_pergunta_card.html:92`, restringe a disputa aberta (`app/chamados/views.py:493`). Modal `/chamados/modal/<colab>/`, view `app/chamados/views.py:869`, formulário `app/templates/chamados/partials/modal_fio.html:274`, aceita pergunta viva em disputa fechada e passa `sem_reabrir=True` (`app/chamados/services/fio.py:591`, `app/chamados/services/fio.py:605`). **Convergem no escritor** `app/chamados/services/validacao.py:343`, mas o modal possui gate diferente e log adicional `validar_viva_em_disputa_fechada` (`app/chamados/services/fio.py:611`). **Pode mudar dia/dinheiro:** a validação materializa e recalcula o fechamento (`app/chamados/services/validacao.py:406`, `app/chamados/services/validacao.py:423`).

2. **Regularizar/fechar disputa: detalhe × modal.** Detalhe: formulário `app/templates/chamados/detalhe_local.html:315` → `app/chamados/views.py:470` → `app/chamados/services/acoes_disputa.py:118`. Modal: formulário `app/templates/chamados/partials/modal_fio.html:348` → `app/chamados/services/fio.py:642`. Detalhe procura somente disputa cujo **container é o chamado**; modal usa `_disputa_do_chamado`, que também encontra disputa pela pergunta cujo destino é o chamado (`app/chamados/services/fio.py:564`, `app/chamados/services/fio.py:134`). **Convergem no materializador**, chamado em `app/chamados/services/acoes_disputa.py:139` e `app/chamados/services/fio.py:666`; os blocos de orquestração permanecem duplicados. **Pode mudar dia/dinheiro**, pois esse escritor materializa respostas e fecha a disputa; a diferença de alcance pode impedir o detalhe de executar o que o modal executa (`app/chamados/services/acoes_disputa.py:139`).

3. **Responder/mudar status/resolver chamado: detalhe/painel de turno × fio/modal.** Detalhe e painel de turno postam `/chamados/local/<id>/` (`app/templates/chamados/detalhe_local.html:385`, `app/templates/chamados/partials/painel_chamado_turno.html:68`) → `app/chamados/services/acoes_chamado.py:283`. Fio `/chamados/colaborador/<id>/` e modal usam `processar_post_fio` (`app/chamados/views.py:674`, `app/chamados/views.py:954`) → `app/chamados/services/fio.py:704`; formulários em `app/templates/chamados/fio_colaborador.html:257` e `app/templates/chamados/partials/modal_fio.html:660`. Detalhe anexa nota `resposta_livre`, logs adicionais `responder` e `status`; fio/modal anexam `mensagem_dp` e não acrescentam esses dois logs (`app/chamados/services/acoes_chamado.py:292`, `app/chamados/services/acoes_chamado.py:295`, `app/chamados/services/acoes_chamado.py:318`, `app/chamados/services/fio.py:714`). **Ambos têm auditoria canônica do estado**, via `transicionar` → `_gravar_estado` → `evento('estado_chamado')` (`app/chamados/models.py:122`, `app/chamados/models.py:274`). Gate HTTP: detalhe aceita mudar status sem conteúdo; fio exige conteúdo (`app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/fio.py:705`), embora todos os formulários tornem o texto obrigatório (`app/templates/chamados/detalhe_local.html:387`, `app/templates/chamados/fio_colaborador.html:261`). **Diferença visível adicional:** detalhe oferece reabrir como `aberto`; fio/modal não (`app/templates/chamados/detalhe_local.html:398`, `app/templates/chamados/fio_colaborador.html:267`, `app/templates/chamados/partials/modal_fio.html:662`). Leitores de estado/perguntas recebem o mesmo mecanismo comum: `app/chamados/signals.py:76`, `app/chamados/services/disputa_emissao.py:1555`. Impacto financeiro específico da diferença de logs: **não**; diferença de alcance HTTP/UI pode alterar o estado do chamado e suas perguntas (`app/chamados/models.py:250`).

4. **Abrir mensagem para colaborador: formulário completo × mensagem rápida.** `/chamados/abrir/`, view `app/chamados/views.py:147`, formulário `app/templates/chamados/form_abertura.html:127` → escritor `app/chamados/services/abertura.py:122`. `/chamados/colaborador/<id>/mensagem-rapida/`, view/escrita `app/chamados/views.py:961`, formulário `app/templates/colaboradores/partials/painel_colaborador.html:188` → `ChamadoColaborador.abrir` (`app/chamados/views.py:973`). Completo cria chamado com assunto, descrição, prazo, autor/contexto e log próprio; deduplica pela entrada completa (`app/chamados/services/abertura.py:165`, `app/chamados/services/abertura.py:172`, `app/chamados/services/abertura.py:194`). Rápido reaproveita mensagem DP viva e acrescenta ocorrência (`app/chamados/views.py:973`, `app/chamados/views.py:986`). **Não muda dia/dinheiro diretamente:** o rápido escreve `mensagem_dp`; o completo permite outros tipos, que devem ser separados ao comparar (`app/chamados/views.py:975`, `app/chamados/services/abertura.py:149`).

**Convergências limpas comprovadas**

- **Autorizar geofence com/sem advertência e recusar geofence:** detalhe, formulários `app/templates/chamados/detalhe_local.html:285`, e modal, formulários `app/templates/chamados/partials/modal_fio.html:498`, convergem em `app/chamados/services/acoes_chamado.py:165` e `app/chamados/services/acoes_chamado.py:243`; roteamento modal em `app/chamados/services/fio.py:529`. Recusar converge depois em `ponto.registro_batida.retratar_batida` (`app/chamados/services/acoes_chamado.py:269`).
- **Aprovar exceção de batida:** detalhe `app/templates/chamados/detalhe_local.html:254` e modal `app/templates/chamados/partials/modal_fio.html:591` postam a mesma view e convergem em `app/chamados/services/acoes_chamado.py:21` (`app/chamados/views.py:426`).
- **Reabrir questionário:** detalhe `app/templates/chamados/detalhe_local.html:9` e modal `app/templates/chamados/partials/modal_fio.html:363` convergem na mesma view `app/chamados/views.py:1372`, escritor de reabertura chamado em `app/chamados/views.py:1435`.
- **Recusar pergunta:** detalhe `app/templates/chamados/partials/_pergunta_card.html:109` e modal `app/templates/chamados/partials/modal_fio.html:285` convergem em `app/chamados/services/validacao.py:494`, via casca `app/chamados/services/acoes_disputa.py:65`.

**Cuidados para a consolidação**

- Full `fio_colaborador.html` tem formulário de mensagem/status; não encontrei formulários de ações de disputa nele. Para disputa, usar **modal × detalhe**, evitando atribuir tela disparadora ao fio completo. [não verificado para ocorrência fora do template inspecionado]
- `corrigir_resposta` tem ramo no detalhe (`app/chamados/views.py:523`), mas o único formulário encontrado está no modal (`app/templates/chamados/partials/modal_fio.html:296`); não fechar par visual.
- `aplicar_declaracao_texto` tem escritor no detalhe (`app/chamados/views.py:478`), porém seu formulário encontrado posta ao **modal** (`app/templates/chamados/partials/modal_fio.html:137`), cujo roteador não tem esse ramo e termina em “Nada a fazer” (`app/chamados/services/fio.py:738`). Não há segunda tela comprovada: registrar como lacuna separada.
- O modal marca notas lidas inclusive no GET (`app/chamados/views.py:877`); isso fica fora da contagem solicitada de views POST.
- Não usar comentários históricos de produção como dado atual: nenhuma ocorrência de produção foi verificada nesta investigação. [não verificado]

## 06/11 21:23 · sessao 2f360630892b · - · - · origem=Ronald (tmux)

Censo fechado deste lote: **8 views POST com escrita em folha/relatórios**, sendo **4 no recorte direto** (TXT + três PDFs); mais **6 views POST de fechamento em ponto** consultadas por serem do domínio pedido. As oito são folha `lista_rubricas`, `gerar_dominio_txt`, `nova_integracao`, `editar_integracao`, `excluir_integracao`; relatórios `anexar_espelho_chamado`, `espelho_pdf_avulso`, `espelho_lote`. Referências: `app/folha/views.py:27`, `:102`, `:466`, `:487`, `:501`; `app/relatorios/views.py:971`, `:1050`, `:1152`. As seis adicionais: `app/ponto/views.py:1226`, `:1257`, `:1383`, `:1434`, `:1461`, `:1520`.

**Dois grupos repetidos, seis pares divergentes**, mais **um par de telas convergente puro**. Cadastro de rubricas/integração é auxiliar, com quatro views POST sem outra tela equivalente encontrada nesta varredura (`app/folha/urls.py:8`, `:10`, `:11`, `:12`).

1. **Aprovar fechamento — três portas, três pares divergentes.**

   - **A, competência:** UI `/ponto/fechamento/`, view `fechamento_mensal`, template `ponto/fechamento.html`; formulário de todos em `app/templates/ponto/fechamento.html:209`, seleção em `:143`. POST `/ponto/fechamento/aprovar/` → `aprovar_fechamento`, `app/ponto/views.py:1257` → **`aprovar_lote`**, `app/ponto/services/fechamento.py:1316`; rotas `app/ponto/urls.py:28`, `:32`.
   - **B, painel individual:** `/ponto/fechamento/painel/` → `painel_fechamento`, `app/ponto/views.py:966`, render em `:1006`, template `ponto/partials/painel_fechamento.html`, formulário `app/templates/ponto/partials/painel_fechamento.html:86`. POST `/ponto/fechamento/aprovar-colaborador/` → `aprovar_colaborador`, `app/ponto/views.py:1461` → **`aprovar`**, `app/ponto/services/fechamento.py:1328`; rotas `app/ponto/urls.py:30`, `:33`.
   - **C, exportação:** UI `/folha/` → `index`, `app/folha/views.py:436`, template `folha/index.html`, formulário `app/templates/folha/index.html:63`; e `/folha/exportar/` → `tela_exportar_dominio`, `app/folha/views.py:83`, template `folha/exportar.html`, preview em `app/templates/folha/exportar.html:26`, formulário gerado `app/folha/views.py:422`. POST `/folha/exportar/gerar/` → `gerar_dominio_txt`, `app/folha/views.py:102` → **mesmo `aprovar_lote` de A**, chamado em `:252`; rotas `app/folha/urls.py:7`, `:13`, `:14`.

   **Diferenças:**

   | Par | Diferença comprovada |
   |---|---|
   | A × B | A decide pelas linhas gravadas (`app/ponto/views.py:1290`); B deriva `fechamento_lido` (`:1480`). A cego considera conta vazia; A selecionado/B dispensam esse veto (`:1304`, `:1509`). A usa update com observação (`:1362`; `app/ponto/services/fechamento.py:1323`); B faz save inteiro (`:1333`). A pode trancar (`app/ponto/views.py:1367`); B só aprova (`:1515`). |
   | A × C | **Convergem em `aprovar_lote`**, mas A lê gravado (`app/ponto/views.py:1290`), C calcula em leitura (`app/folha/views.py:207`), limita carimbo a quem entra no TXT e dispensa conta vazia (`:243`–`:248`; constante `app/folha/export.py:754`). C não passa observação (`app/folha/views.py:252`), escreve `LogAuditoria` `aprovacao_por_geracao` (`:254`) e registra TXT (`:297` → `app/folha/models.py:174`). A pode trancar (`app/ponto/views.py:1367`). |
   | B × C | Ambos consultam cálculo vivo, mas B usa escritor `aprovar`/save inteiro (`app/ponto/views.py:1480`, `:1515`; `app/ponto/services/fechamento.py:1333`); C usa `aprovar_lote`/update e só subconjunto elegível para exportar (`app/folha/views.py:246`, `:252`), além de auditoria e histórico TXT (`:254`, `:297`). |

   **Leitores:** lista lê fechamento gravado (`app/ponto/views.py:1135`); painel lê `fechamento_lido` (`:993`); ambos leem a tranca viva (`:1027`, `:1126`). Trancar também dispara encerramento da fila de chamados/perguntas (`app/ponto/services/fechamento.py:1413`, `:1421`).

   **Gravidade:** **não muda dinheiro/dia diretamente neste carimbo**, que grava status, aprovador, data e eventualmente observação (`app/ponto/services/fechamento.py:1322`, `:1330`). A tranca muda fila e controles; “TXT não tranca” está declarado **conforme/intencional** no registro vivo (`app/core/juizes.py:510`, `:518`). Não inferir bloqueio financeiro: aprovado não decide entrada no TXT (`app/folha/export.py:843`), e recalcular continua gravando os valores calculados de aprovado (`app/ponto/services/fechamento.py:1139`, `:1153`). Ocorrência financeira em produção: **[não verificado]**.

2. **Emitir espelho PDF — três telas, três pares divergentes.**

   - **A, anexar ao chamado:** UI `/chamados/local/<id>/`, rota `app/chamados/urls.py:14`, view `detalhe_chamado_local`, `app/chamados/views.py:359`, render `:550`, template `chamados/detalhe_local.html`, formulário `app/templates/chamados/detalhe_local.html:348`. POST `/relatorios/espelho/chamado/<id>/anexar/`, rota `app/relatorios/urls.py:24`, view `app/relatorios/views.py:971` → **`services.gerar_pdf_espelho`**, `app/relatorios/services.py:49`, chamada `app/relatorios/views.py:1003`.
   - **B, avulso:** `/relatorios/espelho/avulso/`, rota `app/relatorios/urls.py:28`, view `app/relatorios/views.py:1050`, template `relatorios/espelho_pdf_avulso.html`, formulário `app/templates/relatorios/espelho_pdf_avulso.html:18` → **mesmo service**, chamada `app/relatorios/views.py:1127`, com `chamado=None`.
   - **C, lote:** `/relatorios/espelho/lote/`, rota `app/relatorios/urls.py:29`, view `app/relatorios/views.py:1152`, template `relatorios/espelho_lote.html`, formulário `app/templates/relatorios/espelho_lote.html:17` → **`lote_espelho.pedir`**, `app/relatorios/lote_espelho.py:83`, cria registro em `:103`; execução `:187` → `services.gerar_pdf_espelho_lote_bytes`, `app/relatorios/services.py:165` → mesmo service individual em `:237`.

   **Escritor comum do registro auditável:** `app/relatorios/pdf_espelho.py:1074` → `app/relatorios/pdf_base.py:152`, que cria `RegistroPDFAuditavel`.

   | Par | Diferença comprovada |
   |---|---|
   | A × B | Convergem no registro PDF, mas **só A** cria nota e anexo no chamado (`app/relatorios/services.py:75`, `:88`, `:90`); B passa `chamado=None` (`app/relatorios/views.py:1129`). |
   | A × C | Só A anexa ao chamado (`app/relatorios/services.py:88`); C cria lote e trilha do pedido (`app/relatorios/views.py:1227`, `:1238`) e pode substituir PDF auditável recusado por informacional (`app/relatorios/services.py:244`, `:250`). |
   | B × C | B mensal recusa `ValueError` do auditável (`app/relatorios/views.py:1131`); C captura qualquer exceção e tenta informacional sem hash/registro CLT (`app/relatorios/services.py:244`, `:250`; `app/relatorios/pdf_espelho.py:1038`). B livre limita 180 dias (`app/relatorios/views.py:1085`); validação de C passa das datas à seleção sem teto equivalente (`:1176`–`:1202`). B livre omite `por` (`:1098`), C informa `por=user` (`app/relatorios/services.py:250`), portanto o rodapé do B livre fica sem autor (`app/relatorios/pdf_espelho.py:1067`). |

   C chama a geração com `request=None` (`app/relatorios/lote_espelho.py:205`), enquanto A/B passam request (`app/relatorios/views.py:1005`, `:1129`); o registro usa `get_ip_origem(request)` (`app/relatorios/pdf_base.py:163`). C também mantém eventos de resultado (`app/relatorios/lote_espelho.py:254`). A/B convergem no bloqueio por disputa (`app/relatorios/pdf_espelho.py:1084`), mas C oferece fallback (`app/relatorios/services.py:244`).

   **Leitores:** anexo em `baixar_espelho_followup`, `app/relatorios/views.py:1020`; painel do lote em `:1284`; arquivo pronto em `:1295`.

   **Gravidade:** **não muda dinheiro/dia**: os escritores aqui criam registro de PDF, nota/anexo e lote (`app/relatorios/pdf_base.py:152`; `app/relatorios/services.py:88`; `app/relatorios/lote_espelho.py:103`). Divergem documento, autoria, IP e trilha; efeito financeiro em produção **[não verificado]**.

**Convergência pura para o fechamento do censo:** gerar TXT por `/folha/` e `/folha/exportar/` converge no **mesmo endpoint e escritor**, `gerar_dominio_txt` (`app/templates/folha/index.html:63`; `app/folha/views.py:422`; `app/folha/urls.py:14`), com registro final único em `ExportacaoDominio.registrar` (`app/folha/views.py:297`; `app/folha/models.py:174`). A montagem usa `montar_linhas_export`, `app/folha/export.py:682`, chamada `app/folha/views.py:268`.

Não contei como “duas telas” reabrir individual/reavaliar vazios/reabrir competência: suas origens encontradas estão na mesma tela de fechamento (`app/templates/ponto/_fech_linha.html:42`; `app/templates/ponto/fechamento.html:76`, `:223`); endpoints e escritores foram varridos (`app/ponto/views.py:1383`, `:1434`, `:1520`).

## 06/11 21:23 · sessao 596a49a8fba7 · - · - · origem=Ronald (tmux)

Censo de `ponto`: **18 funções de view com escrita administrativa**, expostas em **19 rotas**; a diferença é `salvar_tipo_ausencia`, compartilhada entre criação e edição. A superfície herdada do Django Admin fica separada, porque sua implementação não está nesta cópia. Evidências: `app/ponto/urls.py:37`, `app/ponto/urls.py:54`, `app/ponto/urls.py:58`, `app/ponto/urls.py:65`, `app/ponto/urls.py:83`.

Inventário nominal:

| Views | Referência |
|---|---|
| Aprovar/rejeitar justificativa individual | `app/ponto/views.py:1911`, `app/ponto/views.py:1923` |
| Aprovar/rejeitar justificativa em lote | `app/ponto/views.py:1763`, `app/ponto/views.py:1859` |
| Materializar saída retroativa | `app/ponto/views.py:3106` |
| Anexar documento de ausência | `app/ponto/views.py:2133` |
| Aprovar/rejeitar/cancelar ausência em lote | `app/ponto/views.py:2270`, `app/ponto/views.py:2304`, `app/ponto/views.py:2333` |
| Lançar ausência | `app/ponto/views.py:2368` |
| Reverter rejeição | `app/ponto/views.py:2569` |
| Aprovar ausência / editar período / rejeitar / cancelar / editar registro | `app/ponto/views.py:2599`, `app/ponto/views.py:2662`, `app/ponto/views.py:2680`, `app/ponto/views.py:2720`, `app/ponto/views.py:2751` |
| Resolver dia | `app/ponto/views.py:3312` |
| Criar/editar tipo de ausência | `app/ponto/views_tipos_ausencia.py:57` |

São **14 ações**, caso se contem separadamente: oito verbos de ausência, três de justificativa, resolver dia e criar/editar tipo. Se resolver dia for normalizado como lançamento de ausência, são 13; se materializar saída for agrupado com aprovação de justificativa, são 12. A enumeração acima permite fixar a convenção sem confundir views, rotas e verbos. Evidências: referências da tabela.

**Grupo divergente: aprovar justificativa — três caminhos explícitos.**

- Individual: `/ponto/justificativa/<jid>/aprovar/`, view `app/ponto/views.py:1911`, template `app/templates/ponto/partials/painel_justificativa.html:51` → escritor `aprovar_justificativa_dp`, `app/ponto/services/justificativa.py:117`. Valida saída posterior à entrada (`:137`), só gera saída quando existem saída informada **e entrada** (`:137`), grava estado (`:190`), invalida fechamento (`:199`), fecha origem e bloqueio de entrada (`:202`), registra LogAuditoria e followup (`:214`).
- Lote: `/ponto/justificativa/aprovar-lote/`, view e escritor `app/ponto/views.py:1763`, template `app/templates/ponto/fila_justificativas.html:176`. Grava status diretamente (`views.py:1796`), gera saída com saída informada mesmo sem entrada (`:1808`), não executa a validação saída>entrada presente na porta individual; fecha apenas origem (`:1837`). Não chama o fechamento específico de bloqueio, LogAuditoria da aprovação ou followup que existem na porta individual (`services/justificativa.py:203`, `:215`, `:219`).
- Emergência: `/ponto/justificativa/<jid>/materializar-saida/`, view `app/ponto/views.py:3106`, template `app/templates/ponto/materializar_saida_retroativa.html:38` → `materializar_saida_dp`, `app/ponto/services/justificativa.py:316`. Recebe novo horário e exige motivo com dez caracteres (`views.py:3130`); carrega qualquer status (`services/justificativa.py:329`), grava `saida_informada` e status aprovado (`:367`), fecha cascata de chamados (`:378`) e registra outra ação de auditoria (`:398`). Valida saída>entrada quando há entrada (`:340`).

Esse grupo contém três pares combinatórios; os escritores de aprovação são distintos. A diferença pode alterar a saída registrada e o resultado do dia; leitor financeiro específico fica com a análise do pai, mas a própria porta identifica a aprovação como insumo do fechamento e invalida sua projeção. `app/ponto/services/justificativa.py:194`, `app/ponto/views.py:1804`.

**Par divergente: rejeitar justificativa individual × lote.**

Individual: `/ponto/justificativa/<jid>/rejeitar/`, `app/ponto/views.py:1923`, template `app/templates/ponto/partials/painel_justificativa.html:58` → `app/ponto/services/justificativa.py:247`: decisão em atomic com trava (`:258`), chamado de rejeição (`:275`) e followup (`:309`). Lote: `/ponto/justificativa/rejeitar-lote/`, view/escritor `app/ponto/views.py:1859`, template `app/templates/ponto/fila_justificativas.html:188`: get e save direto (`views.py:1874`), push (`:1882`), sem os chamados/followup da porta individual. O evento comum rejulga o dia nos dois saves: `app/ponto/signals.py:161`. Portanto, não atribuir ao lote ausência de rejulgamento.

**Par que converge no escritor, mas diverge no alcance/validação: aprovar ausência individual × lote.**

Individual: `/ponto/ausencias/<aid>/aprovar/`, `app/ponto/views.py:2599`, template `app/templates/ponto/partials/painel_ausencia.html:309`. Expande raiz e irmãos (`views.py:2615`), permite confirmação de desconto com auditoria extra (`:2612`), recebe período coberto (`:2629`) e envolve os irmãos num atomic (`:2641`); envia push (`:2646`).

Lote: `/ponto/ausencias/lote/aprovar/`, `app/ponto/views.py:2270`, template `app/templates/ponto/ausencias.html:308`. Só processa os IDs enviados (`views.py:2280`); gate com `confirmado=False` (`:2284`); não recebe período. Ambos convergem em `_a4_decidir`, `app/ponto/views.py:82` → `decidir`, `app/ponto/services/ausencia.py:897` → `mudar_estado`, `app/ponto/services/ausencia.py:808`.

**Par que converge no escritor, mas diverge no alcance/campo: rejeitar ausência individual × lote.**

Individual: `/ponto/ausencias/<aid>/rejeitar/`, `app/ponto/views.py:2680`, template `app/templates/ponto/partials/painel_ausencia.html:330`: expande raiz/irmãos (`views.py:2688`), não recebe motivo e envia push (`:2704`). Lote: `/ponto/ausencias/lote/rejeitar/`, `app/ponto/views.py:2304`: só IDs selecionados, recebe motivo e acrescenta em `descricao` (`:2311`). Ambos passam por `_a4_decidir` (`:2701`, `:2319`). Existe função JS emissora em `app/templates/ponto/ausencias.html:316`, mas botão que a invoque no template atual **[não verificado]**; a barra visível contém Aprovar e Cancelar (`:117`, `:120`).

**Convergências fechadas e limites.**

- Cancelar ausência individual e lote convergem no mesmo escritor e nos mesmos campos/auditoria: `app/ponto/views.py:2734`, `app/ponto/views.py:2351`. O botão atual do lote está em `app/templates/ponto/ausencias.html:120`; disparador de tela do cancelamento individual **[não verificado]**.
- Resolver falta/justificado no calendário e diagnóstico converge na mesma view `/ponto/celula/veredito/`, `app/ponto/views.py:3312` → `app/ponto/services/veredito_celula.py:191`. Calendário inclui formulário em lote (`app/templates/colaboradores/calendario_partial.html:63`); diagnóstico também (`app/templates/colaboradores/partials/_drawer_diagnostico.html:36`); ação unitária usa o mesmo endpoint (`app/templates/colaboradores/partials/_calendario_grade.html:126`, `app/templates/ponto/partials/_veredito_dia.html:17`). O ramo falta/justificado chama `criar_ausencia` (`services/veredito_celula.py:315`), fecha fio (`:325`) e lavra o dia (`:330`).
- **Não afirmar Resolver dia no espelho ou fechamento:** existe comentário na view do espelho (`app/ponto/views.py:163`), mas disparador nos templates atuais **[não verificado]**.
- Anexar documento tem view administrativa e chama a porta única: `app/ponto/views.py:2133` → `app/ponto/services/ausencia.py:846`; disparador no template atual **[não verificado]**.
- Django Admin: registro `JustificativaAdmin(admin.ModelAdmin)` aberto em `app/ponto/admin.py:18`; `Justificativa` não sobrescreve `save` no corpo da classe (`app/ponto/models.py:115` até `:197`) e declara status/decisor/data (`:159`). `AusenciaAdmin` usa SoLeitura (`app/ponto/admin.py:38`), cujo bloqueio está em `app/core/admin.py:59`. Django aparece como dependência em `app/requirements.txt:13`; URL detalhada, view, template e escritor herdados **[não verificado]**, pois o código do framework não está na cópia nem instalado no Python disponível.

## 06/11 21:23 · sessao f2a149a1cf55 · - · - · origem=Ronald (tmux)

Censo Q2 — somente leitura, sem contagens de produção.

**Distinção dos estados.** `PerguntaDisputa.estado` é propriedade derivada; combina `validada_em`, C6 e `materializada_em`, não há coluna `status` da pergunta (`app/chamados/models.py:2491-2506`). `DisputaSupervisao` abre/fecha por `fechada_em`; `.aberta` significa `fechada_em is None` (`app/chamados/models.py:1686-1687,1763-1765`); `motivo_fechamento` é coluna própria (`app/chamados/models.py:1708-1725`). `ChamadoColaborador.status_local` é outro estado; o selo distingue explicitamente os dois modelos/campos (`app/chamados/tests/test_selo_status_so_pelas_portas.py:28-53`).

| Fronteira | Escritores e leitores conferidos |
|---|---|
| Escrita — identidade/data/destino | `PerguntaDisputa.abrir` nasce/reutiliza/reaponta (`app/chamados/models.py:2398-2477`); `fio.reapontar_resolvedora` altera destino, só preenche `data_evento` quando antes vazio e relavra no mesmo save (`app/chamados/services/fio.py:829-848`). Dois módulos permitidos pelo contrato, não módulo único (`app/core/tests/test_contract_escritores_estado.py:48-62`). |
| Escrita — resposta/validação | Resposta humana: `responder_pergunta_disputa_core` escreve texto+instante, desfaz validação anterior e pode descarimbar resolução sem lastro (`app/chamados/services/disputa_emissao.py:1282-1301`). Resposta automática sem sobrescrever texto: `responder_automaticamente` (`app/chamados/services/disputa_emissao.py:1422-1435`). Reconstrução de instante com lastro da nota: `carimbar_resposta_pela_trilha` (`app/chamados/services/disputa_emissao.py:1439-1453`). Validação administrativa escreve em `validacao.py:131-133`; devolução desfaz em `validacao.py:187-190`. |
| Escrita — carimbo/resolução | `_escrever_carimbo` centraliza materializada_em/por/via (`app/chamados/models.py:2556-2560`); portas `carimbar`, `carimbar_identidade`, `descarimbar`, `descarimbar_em_lote` (`app/chamados/models.py:2562,2625,2646,2692`). Via isolada também possui portas próprias; selo diferencia isso (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:35-38`; `encerrar_por_fato_no_espelho`, `app/chamados/models.py:2702-2710`). |
| Escrita — disputa/chamado | Selo AST declara portas de Chamado `_gravar_estado`, `criar`, `abrir`, `_vivas_entre` somente em memória; Disputa `fechar`, `reabrir_admin`, `reabrir_sistema` (`app/chamados/tests/test_selo_status_so_pelas_portas.py:36-53`). Implementações: chamado `models.py:195,332,796,1015`; disputa `models.py:1796,2040,2077`. Fechamento disputa grava fechada_em/por/resolucao/motivo em atomic (`app/chamados/models.py:1845-1869`). |
| Escrita — célula | Portas `regenerar_celulas_vinculo`, `editar_dia`, `gerar_celulas_periodo`, `lavrar_veredito`, `carimbar`, `carimbar_precedencia` (`app/ponto/portas/celula.py:110,534,811,969,1047,1071`). `lavrar_veredito` escreve ata, veredito/via/em/protestos e status_dia/status_rotulo (`app/ponto/portas/celula.py:969-1040`). `gerar_celulas_periodo` cria/atualiza trabalha, geradora e DNA; preserva passado salvo enriquecimento e edição humana (`app/ponto/portas/celula.py:847-871`). |
| Materialização | Seleciona perguntas respondidas+validadas e ainda não materializadas (`app/chamados/services/materializacao.py:1344-1354`); hora gera batida a partir de data_evento+hora, data_hora ancora ciclo, escolha pode declarar ausência (`app/chamados/services/materializacao.py:1313-1328`). Data pergunta divergente do chamado bloqueia até conferência humana (`app/chamados/services/materializacao.py:1156-1173`). Lê `chamado_destino` para operação/fecho (`app/chamados/services/materializacao.py:339-345,1049-1052`). |
| Consulta — pergunta | Identidade de leitura `do_fato(colab,motivo,data_evento)` (`app/chamados/models.py:2381-2395`); C6 exato por `.viva` (`app/chamados/models.py:2798-2807`); `q_vivas` é **superset**, não equivalente SQL completo (`app/chamados/models.py:2770-2777`). `pode_fechar` lê C6 e respostas sem veredito (`app/chamados/models.py:1767-1794`). |
| Consulta — cobrança | `aviso_pendencia.perguntas_pendentes_do_colab` = q_vivas + C6 exato + respondida_em nulo (`app/chamados/services/aviso_pendencia.py:7-13`). `cobranca._pendentes` delega; `checar_cobranca` lê data_evento, disputa e SLA do chamado **pai** (`app/chamados/services/cobranca.py:45-67,75-94`). |
| Consulta — célula/fábrica | `motivos_a_perguntar` lê trabalha/ata por juiz, situação ativa, data, teto por instante, veredito e parcial_so_lista; também deduplica qualquer pergunta histórica do dia exceto via duplicata (`app/chamados/services/fabrica_da_celula.py:74-126`). `_marcos_faltantes` lê grade de d-1 e d, tipo_dia/cobertura parcial, marcos/células missing (`app/chamados/services/declaracao_texto.py:65-123`). |
| Tela admin | `fio.montar_chamados_fio` inclui perguntas C6 vivas de disputa fechada, sem reabrir, e ancora por destino (`app/chamados/services/fio.py:209-239`). Modal lê datas via filtros, mostra ambos os dias divergentes e conferência (`app/templates/chamados/partials/modal_fio.html:156-179`), resposta/carimbos/pendência/admin (`app/templates/chamados/partials/modal_fio.html:212-245`). Relatório lê somente disputas abertas e classifica perguntas vivas (`app/chamados/relatorios_disputa.py:11-23,40-47`). |
| Tela app/API | `perguntas_do_app` busca todas as perguntas do colab, q_vivas e C6 exato, sem filtro de disputa aberta (`app/chamados/services/pergunta_no_app.py:42-53`). API delega essa seleção, entrega estado explícito, separa respondidas/aguarda_admin e destino (`app/api/views.py:2551-2594`); POST aceita somente pergunta C6 viva e chama core (`app/api/views.py:2627-2643`). |
| Tela PWA colab | O fio com `so_janela=True` segue pelas disputas abertas, diferente do admin (`app/chamados/services/fio.py:209-221`); card condiciona formulário por `.viva`, apresenta data_evento, resposta e estado (`app/templates/chamados/partials/_card_atendimento.html:77-100,183`). |

**Universos e significados que divergem.**

- `data_evento` é DateField, descrito como snapshot do serviço de jornada (`app/chamados/models.py:2218-2220`), mas nasce de três espécies distintas: dia do contexto/grade (`app/chamados/services/disputa_emissao.py:319-347`), **data civil local da batida** para anomalia (`app/chamados/services/disputa_emissao.py:350-364`) e **localdate do nascimento** para conversa, que nasce sempre sem dedup (`app/chamados/models.py:2422-2427`). Não cabe assumir que toda data_evento já é turno canônico.
- A porta trata identidade pelo valor cru colab+motivo+data_evento e pega a última linha sem filtro de estado (`app/chamados/models.py:2392-2395,2430-2432`); o gate de compatibilidade com destino, porém, declara unidade **turno**, não igualdade aritmética de datas (`app/chamados/models.py:2408-2421`).
- `fio.data_do_fato_de` lê p.data_evento e compara com `dia_do_fato(ch, fortes=True)`; devolve os dois e divergência, não canonicaliza nem corrige data (`app/chamados/services/fio.py:40-60,77-88`). `reapontar_resolvedora` não re-chaveia data existente, apenas completa a ausente (`app/chamados/services/fio.py:831-835`).
- `estado` pode rotular PENDENTE/RESPONDIDA mesmo quando C6 julga morta sem carimbo; a própria propriedade `.viva` documenta essa diferença (`app/chamados/models.py:2502-2506,2802-2807`). Não usar `.estado` como sinônimo de exigibilidade.
- A API serve vivas em disputa fechada (`app/chamados/services/pergunta_no_app.py:46-53`), mas badge do colab exige disputa aberta (`app/chamados/models.py:1729-1740`) e PWA fio declara restrição pelas abertas (`app/chamados/services/fio.py:209-221`). São leitores com universos diferentes.
- SLA da cobrança fica no chamado **pai** (`app/chamados/services/cobranca.py:51-66`); apresentação/resolução usa chamado **destino**, com pai apenas como fallback (`app/chamados/services/fio.py:97-117`). Destino não significa a mesma coisa que responsável/SLA.
- Carimbos respondida_em/validada_em/materializada_em são instantes; `data_evento` é dia; vencimento de janela é dia (`app/chamados/models.py:2218-2229,2356-2361,2827-2829`). Não agrupar pelo mesmo relógio sem contrato explícito.
- `_coberto_por_ausencia` possui docstring antiga “aprovada não-falta”, mas implementação usa ausência vencedora para cobrança, explica dia inteiro **ou parcial sem período**, preserva resto de parcial com período (`app/chamados/services/materializacao.py:55-82`). A documentação não descreve integralmente o universo atual.

**Vigia fio — escritores/leitores.**

- `vigia_fio` é leitura e composição, não escreve estado de domínio (`app/chamados/services/vigia_fio.py:318-323`). `fio_do_dia` delega à redução `fio_vivo` (`app/chamados/services/vigia_fio.py:156-166`).
- `censo`: universo chamado vivo com célula, recorte empresa, módulos FURO_MODULES; depois células VEREDITOS_ACUSAM; perguntas agrupadas por célula; hoje/futuro fora do alarme (`app/chamados/services/vigia_fio.py:334-379,398-424`).
- `censo_alcance`: outro universo, empresas visíveis × competências a lavrar × dias_que_seguram folha; alimenta portas uma vez e lê alcance real (`app/chamados/services/vigia_fio.py:555-618`).
- `dono_fora_do_fio` lê Resolver dia oferecido, ausência aberta, aceite de folga e lista do admin, nesta ordem (`app/chamados/services/vigia_fio.py:274-315`); não exige exatamente um responsável: retorna o primeiro balde.
- `ListaDoAdmin` lê lista Cadastro x Realidade no universo empresas visíveis e dono BATIDA/CADASTRO; falha preserva o alarme (`app/chamados/services/vigia_fio.py:188-228`).
- Contadores adicionais: disputa vazia (`app/chamados/services/vigia_fio.py:727-734`), viva em fechada (`:737-759`), respondida sem texto depois de marco temporal (`:774-786`), carimbo sem lastro dividido por competência entregue (`:789-844`). São universos distintos.
- Command consumidor lê todos na mesma rodada/relógio (`app/chamados/management/commands/vigiar_fio_morto.py:159-176`) e **escreve arquivo de medição**, não domínio, `_carimbar` (`:115`) com censo+alcance+passivos (`:211-225`), somente frota, não recorte empresa (`:228-230`).
- Cron chama o command (`app/config/crons.py:1703`). Leitor host usa `app/logs/fio_morto.json`, valida alcance e recusa universo vazio como verde (`bin/fio_estado.py:61-88`); `bin/gerar_estado.py:255` referencia a mesma fonte.

**Já existe outbox parcial, relevante ao ADR.**

- Em célula, os campos que acordam são somente veredito e ata; save+evento no mesmo atomic (`app/ponto/portas/celula.py:878-902`).
- Pergunta possui conjunto próprio amplo de campos que acordam (`app/chamados/models.py:2655-2662`); `PerguntaDisputa.save` e signal outbox são atômicos (`app/chamados/models.py:2847-2855`, `app/chamados/signals.py:205-221`). Nascimento pergunta não emite por esse receiver (`app/chamados/signals.py:211-217`).
- Reabertura por update salva `RespostaAnterior` e emite célula explicitamente no mesmo atomic (`app/chamados/services/disputa_emissao.py:1393-1417`).
- Já há reaberturas reais: ressuscitar perguntas pelo C6 sobre cópia pós-estado (`app/chamados/models.py:1420-1458,1461`); sistema reabre disputa sem exigir user/teto (`app/chamados/models.py:2077-2095`); nova resposta derruba validação e carimbo sem lastro (`app/chamados/services/disputa_emissao.py:1282-1301`). Isso não equivale ao protocolo de episódio com extinção provada do ADR.

**Limite do censo:** conferi os pontos de produção e os contratos AST acima; não executei os selos nem consultei dados. “Não existem outros escritores fora das portas” é a intenção verificável do selo, mas passagem atual dos testes está [não verificado]. Comentários de testes antigos também envelhecem: `test_porta_disputa_ciclo.py:50-56` ainda descreve escrita em lote no desligamento, enquanto o selo mais específico só permite as portas (`test_selo_status_so_pelas_portas.py:36-53`) e o código de desligamento registra a retirada desse update (`app/colaboradores/services/desligamento.py:133`).

## 06/11 21:23 · sessao 8a3204923ac2 · - · - · origem=Ronald (tmux)

Censo de `ponto`: **10 views POST administrativas; 9 ações semânticas**, contando aprovação/rejeição de justificativa separadamente, materialização, flip, retratação, desfazer, resolver dia e aprovação/rejeição offline. As rotas estão em `app/ponto/urls.py:37`, `:40`, `:41`, `:42`, `:43`, `:44`, `:58`, `:59`, `:61`, `:62`, `:77`; views em `app/ponto/views.py:434`, `:1763`, `:1859`, `:1911`, `:1923`, `:3106`, `:3260`, `:3274`, `:3286`, `:3312`.

1. **Aprovar justificativa: unitário × lote — divergente, muda dia/dinheiro.**
   - Tela A: `/ponto/justificativa/<jid>/painel/`, view `app/ponto/views.py:1662`, template `app/templates/ponto/partials/painel_justificativa.html:51`, POST `/ponto/justificativa/<jid>/aprovar/`, view `app/ponto/views.py:1911` → escritor `app/ponto/services/justificativa.py:117`.
   - Tela B: `/ponto/justificativa/fila/`, view `app/ponto/views.py:1695`, template `app/templates/ponto/fila_justificativas.html:176`, POST `/ponto/justificativa/aprovar-lote/`, view/escritor `app/ponto/views.py:1763`, escrita direta `:1796`.
   - Unitário valida saída posterior à entrada (`app/ponto/services/justificativa.py:137`), exige entrada para criar S (`:137`), retorna sem aprovar quando já há `batida_gerada` (`:169`). Lote aprova antes (`app/ponto/views.py:1796`), cria S apenas exigindo saída informada e ausência de gerada (`:1808`), sem a validação saída>entrada.
   - Unitário fecha também bloqueio de entrada (`app/ponto/services/justificativa.py:203`) e registra LogAuditoria/followup (`:214`, `:218`). Lote chama encerramento da origem (`app/ponto/views.py:1837`), sem esses atos explícitos.
   - Leitor: `app/ponto/services/fechamento.py:660` inclui justificativas aprovadas em `datas_justificadas`; motor usa isso para faltas, abono e DSR em `app/ponto/motor_calculo_v2.py:2600`, `:2605`, `:2618`, `:2620`.

2. **Aprovar/materializar saída da justificativa: três caminhos — grupo divergente.**
   - A/B anteriores; C `/ponto/justificativa/<jid>/materializar-saida/`, view `app/ponto/views.py:3106`, template `app/templates/ponto/materializar_saida_retroativa.html:38` → escritor `app/ponto/services/justificativa.py:316`.
   - C exige horário e motivo ≥10 caracteres (`app/ponto/views.py:3133`, `:3135`), aceita justificativa por PK sem exigir status pendente (`app/ponto/services/justificativa.py:329`), grava saída informada, gerada e aprovada (`:365`), fecha chamados de turno por cascata (`:378`) e origem (`:393`), registra `mat_saida_retro` (`:398`). A/B exigem pendente (`app/ponto/services/justificativa.py:127`; `app/ponto/views.py:1794`).
   - C pode aprovar justificativa previamente rejeitada sem batida vinculada; A/B não selecionam esse estado. Impacto em dia/dinheiro pelo mesmo leitor de justificativas aprovadas, `app/ponto/services/fechamento.py:660`.

3. **Rejeitar justificativa: unitário × lote — divergente.**
   - Tela A painel, formulário `app/templates/ponto/partials/painel_justificativa.html:58`, view `app/ponto/views.py:1923` → `app/ponto/services/justificativa.py:247`.
   - Tela B fila, formulário `app/templates/ponto/fila_justificativas.html:188`, view/escritor `app/ponto/views.py:1859`.
   - Unitário usa atomic, trava origem e `select_for_update` (`app/ponto/services/justificativa.py:258`, `:261`, `:262`), abre chamado de rejeição (`:276`) e followup (`:309`). Lote lê e salva diretamente (`app/ponto/views.py:1874`), fazendo push (`:1883`), sem os mesmos atos.
   - Rejeitar o mesmo pendente mantém o mesmo status nominal, mas divergência de concorrência permite sobrescrever decisão intermediária no lote; ocorrência em produção [não verificado]. O chamado/fio diferem pelo escritor `app/ponto/services/justificativa.py:276`; todo save rejulga o dia em `app/ponto/signals.py:161`.

4. **Retratar marcação: calendário × drawer — divergente de formulário, mesmo escritor.**
   - Calendário: rota `app/colaboradores/urls.py:24`, view `app/colaboradores/views.py:1935`, template `app/templates/colaboradores/calendario_partial.html:62`.
   - Drawer: rota `app/colaboradores/urls.py:86`, view `app/colaboradores/views.py:2938`, template `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`.
   - Ambos incluem formulário de retratação `app/templates/colaboradores/partials/_calendario_grade.html:121` → view `app/ponto/views.py:3274` → **converge em** `app/ponto/services/retratar_humano.py:150`.
   - Calendário fornece `motivos_retratacao` (`app/colaboradores/views.py:1956`, `:1958`); contexto do drawer não fornece essa chave (`app/colaboradores/services/drawer.py:139`). O select obrigatório fica sem opções válidas no drawer; o escritor exige motivo cadastrado/ativo (`app/ponto/services/retratar_humano.py:166`). Resultado visual inferido do código; navegador/produção [não verificado].
   - Muda dia/dinheiro: retratação tira marcação do universo apurável (`app/ponto/turnos.py:2864`), lido pelo espelho (`app/ponto/services/espelho.py:1022`, `:1179`).

Convergências para o fim:

- **Flip**: calendário + drawer acima, formulário comum `app/templates/colaboradores/partials/_calendario_grade.html:121`; terceira tela `/ponto/flip-fila/`, view `app/ponto/views.py:3299`, template `app/templates/ponto/flip_fila.html:16`. Todos → view `app/ponto/views.py:3260` → **converge em** `app/ponto/services/flip_batida.py:10`. Fila fornece motivo; calendário/drawer omitem, aceito pelo argumento padrão do escritor (`:10`); trilha/recompute comuns (`:30`, `:39`).
- **Desfazer retratação**: calendário + drawer, formulário comum `app/templates/colaboradores/partials/_calendario_grade.html:121` → view `app/ponto/views.py:3286` → **converge em** `app/ponto/services/retratar_humano.py:190`; prazo e origem conferidos igualmente (`:199`, `:203`).
- **Resolver dia**: calendário + drawer e formulários individual/lote `app/templates/ponto/partials/_veredito_dia.html:17`, `app/templates/ponto/partials/_veredito_lote_barra.html:2` → mesma view `app/ponto/views.py:3312` → **converge em** `app/ponto/services/veredito_celula.py:191`. Completa batidas (`:301`), cria ausência (`:315`), fecha perguntas/chamados (`:325` → `:160`) e lavra célula/trilha (`:330`, `:334`).

Fora dos pares: aprovação/rejeição offline possuem uma tela local, `app/ponto/views.py:434`, template `app/templates/ponto/batidas_offline_pendentes.html:108`, `:114`; escrevem via `anotar`/`apagar` em `app/ponto/views.py:472`, `:479`. `abrir_justificativa` e `regularizar_retorno` são selfserve do usuário-colaborador (`app/ponto/views.py:1558`, `:3168`); painel de justificativa e espelho não possuem ramo de escrita (`app/ponto/views.py:1662`, `:94`).

## 06/11 21:22 · sessao 53ecda1d422f · - · - · origem=Ronald (tmux)

Censo `holerite`: **6 views POST administrativas escritoras**, distribuídas em publicar documento, enviar aviso e reenviar aviso; `upload_lote_v2` também cria a prévia/lote. Rotas: `app/holerite/urls.py:8`, `:9`, `:14`, `:16`, `:37`, `:39`. Escritas: `app/holerite/views.py:618`, `:745`, `:1193`, `:1255`, `:1259`, `:1758`, `:1773`, `:1823`, `:1828`.

**Publicar holerite: grupo com três pares divergentes.**

| Tela / rota | View → escritor | Template |
|---|---|---|
| `/holerite/upload/` | `app/holerite/views.py:1145` → delete dos antigos `:1191`, create `:1193` | `app/templates/holerite/upload.html:9`; outro disparador: `app/templates/holerite/_drawer_modo_avulso.html:11`, renderizado por `app/holerite/views.py:363` |
| `/holerite/upload-lote/` | `app/holerite/views.py:1217` → delete `:1255`, create `:1259` | `app/templates/holerite/upload_lote.html:12` |
| `/holerite/lote/` | `app/holerite/views.py:1721` → job `:1759` → `app/holerite/management/commands/processar_lote_holerite.py:89` → `app/holerite/motor.py:390`, grava documento `:480` | `app/templates/holerite/upload_lote_v2.html:23`; drawer `app/templates/holerite/_lote_drawer.html:17`; drawer empresa `app/templates/holerite/_drawer_modo_lote.html:13`; confirmar publicação `app/templates/holerite/_lote_progresso.html:119` |

1. **Avulso × ZIP:** avulso barra substituição se há ciência, registra trilha antes de apagar e preenche `empresa_folha_id`; ZIP apaga todos da pessoa/competência sem essas verificações nem preencher empresa. `app/holerite/views.py:1174`, `:1176`, `:1184`, `:1191`, `:1202`, `:1255`, `:1259`. Ciência é `CASCADE`, portanto o delete ZIP apaga a assinatura vinculada: `app/holerite/models.py:193`. Leitor: matriz separa documentos por `empresa_folha_id` em `app/holerite/views.py:295`; ciência determina estado “assinado” em `app/holerite/matriz.py:476`. **Dinheiro/dia: não diretamente; perde prova de ciência e altera identificação da folha na matriz** — campos gravados em `app/holerite/views.py:1193`, `:1259`; efeito financeiro real [não verificado].

2. **Avulso × lote PDF:** avulso substitui documento sem ciência e cria sem lote; PDF pula pessoa já publicada (`ja_publicado`), mantém assinatura/documento, grava empresa do CNPJ e vínculo ao lote. `app/holerite/views.py:1174`, `:1191`, `:1193`; `app/holerite/motor.py:323`, `:344`, `:479`, `:480`. PDF roda gravação em transação, `app/holerite/motor.py:385`; carimba publicação do lote em `app/holerite/management/commands/processar_lote_holerite.py:129`. **Dinheiro/dia: sim, potencialmente**, pois só documento com lote publicado fecha competência para célula: `app/folha/export.py:614`. Avulso cria sem esse lote em `app/holerite/views.py:1193`. Guarda de regeneração enumera apenas holerite com lote publicado em `app/ponto/management/commands/gerar_celulas.py:169` e pergunta ao juiz em `:173`; edição do dia também consulta essa guarda em `app/ponto/portas/celula.py:568`. Alteração concreta em produção [não verificado].

3. **ZIP × lote PDF:** além da mesma divergência de fechamento, ZIP deleta inclusive documento com ciência e recria sem empresa/lote; PDF faz publicação incremental, com empresa do documento/lote e preservação dos existentes. `app/holerite/views.py:1255`, `:1259`; `app/holerite/motor.py:323`, `:344`, `:415`, `:479`, `:480`; fechamento exige lote publicado em `app/folha/export.py:614`. **Dinheiro/dia: sim, potencialmente**, pela guarda diferente da célula; perda de ciência também comprovada pelo `CASCADE` de `app/holerite/models.py:193`. Ocorrência em produção [não verificado].

**Avisar/reenviar holerite: dois pares divergentes com o histórico.** Entrega pode ficar fora da interpretação estrita de C; os caminhos estão fechados abaixo para decisão do consolidado. Escopo desses caminhos: `app/holerite/views.py:605`, `:734`, `:1794`.

| Tela / rota | View → escritor | Template |
|---|---|---|
| Drawer da competência, `/holerite/celula/<empresa>/<ano>/<mes>/enviar/` | `app/holerite/views.py:605` → `app/holerite/servicos.py:145` → `:76` | `app/templates/holerite/_celula_lista.html:26` |
| Mesmo drawer, `/holerite/celula/<empresa>/<ano>/<mes>/reenviar/` | `app/holerite/views.py:734` → `app/holerite/servicos.py:76`, com não lidos e trava | `app/templates/holerite/_celula_lista.html:34` |
| Histórico, `/holerite/lote/<id>/avisar/` | `app/holerite/views.py:1794` → push direto `:1813`, update `:1821`, save lote `:1828` | `app/templates/holerite/historico_lotes.html:36`, `:38`, `:57` |

- **Enviar drawer × avisar histórico:** drawer seleciona pelo filtro e pessoa (`app/holerite/views.py:565`), usa FCM com fallback Web Push (`app/holerite/servicos.py:38`, `:42`), só novos (`:154`), limpa/grava falha (`:113`, `:129`) e audita envio (`:134`). Histórico seleciona itens `ok` do lote, usa só FCM e marca todos os documentos da pessoa/competência, sem atualizar falha; também carimba o lote (`app/holerite/views.py:1808`, `:1813`, `:1821`, `:1826`). Leitor: `app/holerite/matriz.py:480`, `:493` usa aviso/falha; histórico lê aviso do lote em `app/templates/holerite/historico_lotes.html:34`. **Dinheiro/dia: não**, escritas atingem aviso/falha/lote, conforme `app/holerite/servicos.py:113`, `:129` e `app/holerite/views.py:1821`.

- **Reenviar drawer × reenviar histórico:** drawer pula já lidos e respeita trava horária (`app/holerite/servicos.py:103`, `:106`; parâmetros em `app/holerite/views.py:745`); histórico manda `forcar=1` e repete para todos os itens `ok` sem essas guardas (`app/templates/holerite/historico_lotes.html:55`; `app/holerite/views.py:1805`, `:1808`, `:1813`). Mesmos leitores e gravidade do par anterior, pelas escritas de `app/holerite/views.py:1821` e estados de `app/holerite/matriz.py:476`.

**Convergências separadas.**

- Avulso em página inteira e drawer **converge em `upload_holerite`**, mesmo escritor: `app/templates/holerite/upload.html:9`, `app/templates/holerite/_drawer_modo_avulso.html:11`, `app/holerite/views.py:1193`.
- Lote PDF em página inteira, drawer genérico e drawer da empresa **converge em `upload_lote_v2` → job → `motor._gravar`**: templates em `app/templates/holerite/upload_lote_v2.html:23`, `app/templates/holerite/_lote_drawer.html:17`, `app/templates/holerite/_drawer_modo_lote.html:13`; cadeia em `app/holerite/views.py:1759`, `app/holerite/management/commands/processar_lote_holerite.py:89`, `app/holerite/motor.py:390`. Drawer empresa acrescenta `empresa` (`app/templates/holerite/_drawer_modo_lote.html:19`), que gera validação de CNPJ esperado na análise (`app/holerite/management/commands/processar_lote_holerite.py:107`), ausente quando não fornecido — portanto convergência com validação contextual diferente.
- Enviar/reenviar, execução curta e job **convergem em `servicos.enviar`**: `app/holerite/views.py:618`, `:745`; `app/holerite/servicos.py:155`; job em `app/holerite/management/commands/enviar_holerites_em_lote.py:66`. São ramificações de execução, sem segunda tela própria comprovada [não verificado].
- Lista de entrega e relatório PDF convergem em `_relatorio_da_selecao`, mas **sem escritor de estado**, excluídos do censo POST: `app/holerite/views.py:963`, `:971`, `:977`.

Exportação CSV é GET com escrita apenas de trilha (`app/holerite/views.py:1071`, `:1097`), assim como maço PDF (`:917`, `:943`); não achei outro caminho da mesma ação entre rotas/templates de holerite (`app/holerite/urls.py:18`, `:26`; segundo disparador [não verificado]).

## 06/11 21:22 · sessao f30767be6376 · - · - · origem=Ronald (tmux)

Censo `ferias`: **14 views POST administrativas**, excluindo resposta do colaborador (`app/ferias/views.py:868`). Prefixo `/ferias/` em `app/config/urls.py:32`; rotas abaixo em `app/ferias/urls.py:9`.

| View POST administrativa | Rota relativa `/ferias/` | Tela/template disparador → escritor |
|---|---|---|
| `criar_periodo_aquisitivo`, `app/ferias/views.py:123` | `<cid>/novo-periodo/`, urls:15 | `ferias/criar_periodo.html:9` e `ferias/_secao_colab.html:6` → `garantir_periodo_aquisitivo`, `app/ferias/services.py:254`; ramo histórico → `registrar_gozo_historico`, services:270, chamada views:210 |
| `agendar_ferias`, views:269 | `periodo/<periodo_id>/agendar/`, urls:16 | `ferias/agendar.html:13` e `_secao_colab.html:60` → `_executar_agendamento`, views:259 → `executar_agendamento`, services:306 |
| `agendar_lote`, views:569 | `agendar-lote/`, urls:10 | `ferias/agendar_lote.html:24` → `_executar_agendamento`, views:617 → services:306 |
| `cancelar_agendamento`, views:321 | `agendamento/<ag_id>/cancelar/`, urls:18 | `ferias/saldo.html:71` e `_secao_colab.html:160` → `cancelar_agendamento_dp`, services:330, chamada views:332 |
| `cancelar_agendamento_admin`, views:341 | `agendamento/<ag_id>/cancelar-admin/`, urls:19 | `_secao_colab.html:162` → wrapper views:512 → `executar_cancelamento_admin`, services:341 |
| `cancelar_admin_massa`, views:519 | `cancelar-admin-massa/`, urls:9 | `ferias/cancelar_admin_massa.html:27` → wrapper views:559 → services:341 |
| `corrigir_concluida_admin`, views:378 | `agendamento/<ag_id>/corrigir-concluida/`, urls:20 | `_secao_colab.html:166` → wrapper views:407 → services:341; **view distinta**, sem flag em cancelar-admin |
| `editar_agendamento`, views:633 | `agendamento/<ag_id>/editar/`, urls:22 | `_secao_colab.html:130` → `remarcar_agendamento`, services:403 → `_trocar_janela`, services:366 |
| `editar_datas_admin`, views:421 | `agendamento/<ag_id>/editar-datas-admin/`, urls:21 | `_secao_colab.html:146` → `corrigir_datas_agendamento`, services:398 → `_trocar_janela`, services:366 |
| `avaliar_solicitacao`, views:711 | `solicitacoes/<sid>/avaliar/`, urls:12 | `ferias/solicitacoes.html:26` → aprovar: services:410; rejeitar: services:453; chamadas views:731 e :738 |
| `editar_periodo`, views:777 | `periodo/<periodo_id>/editar/`, urls:23 | → `editar_periodo_aquisitivo`, services:464, chamada views:795; template disparador **[não verificado]** |
| `excluir_periodo`, views:808 | `periodo/<periodo_id>/excluir/`, urls:24 | `_secao_colab.html:101` → `excluir_periodo_aquisitivo`, services:476, chamada views:819 |
| `agendamento_enviar`, views:830 | `agendamento/<ag_id>/enviar/`, urls:25 | `_secao_colab.html:125` → `pedir_confirmacao`, services:484, chamada views:845 |
| `vender_dias`, views:906 | `periodo/<periodo_id>/vender/`, urls:17 | `_secao_colab.html:84` → `vender_dias_do_periodo`, services:507, chamada views:926 |

Os caminhos das linhas da tabela abreviadas como `views`, `services`, `urls` são respectivamente `app/ferias/views.py`, `app/ferias/services.py`, `app/ferias/urls.py`; templates são de `app/templates/`. O drawer é carregado por `/ferias/<cid>/secao/`, view `secao_ferias_colab`, `app/ferias/views.py:756`, que renderiza `_secao_colab.html` em :765; a lista o abre em `app/templates/ferias/lista.html:81`.

**Grupos com diferenças efetivas**

1. **Cancelar férias: normal × admin × lote × concluída.** Telas/views/escritores estão na tabela; não são escritores únicos: normal usa `cancelar_agendamento_dp`, services:330; demais usam `executar_cancelamento_admin`, services:341.

   - Normal bloqueia somente `concluido`, views:325; portanto aceita `cancelado` e decrementa novamente `dias_concedidos`, services:333. Admin exige `pode_cancelar_admin`, views:360, propriedade restrita a `agendado/em_gozo`, models:335; lote exclui cancelado/concluído, views:529; concluída exige exclusivamente `concluido`, views:396, e justificativa ≥5, views:399. **Impacto dia/saldo: sim**, repetição do POST normal pode devolver dias repetidamente; saldo é `direito − concedidos − vendidos`, `app/ferias/models.py:216`. Ocorrência real em produção **[não verificado]**.
   - Normal grava período e agendamento separadamente, services:332; admin envolve os mesmos efeitos em `atomic`, services:352, rejeita Ausencias antes da remoção, services:356, e acrescenta carimbo a `obs`, services:358.
   - **Ambos removem as Ausencias no resultado final**: `AgendamentoFerias.save`, `app/ferias/models.py:434` chama `_sync_ausencia`, models:379, que chama `espelhar_ferias`; cancelado apaga as linhas em `app/ponto/services/ausencia.py:1188`. Não há fundamento para dizer que cancelamento normal deixa ausência órfã.
   - Admin individual registra `LogAuditoria`, views:367; concluída também, views:409. Lote somente executa escritor e mensagem, views:551; normal somente executa escritor e resposta, views:331. **Diferença de trilha sem diferença necessária de dia no primeiro cancelamento**, sustentada pelos escritores acima.
   - **Leitores**: saldo na tela, `app/templates/ferias/saldo.html:41`; `_secao_colab.html:50`. Espelho lê cobertura de ausência, `app/ponto/services/espelho.py:582`, e cobertura aprovada, :588. Folha exclui cancelado de férias, `app/ferias/services.py:51`, e usa essas férias para retenção por férias com batida, `app/folha/export.py:491`; retenção é aplicada em export:868.

2. **Corrigir datas × editar agendamento.** **Converge em `_trocar_janela`, `app/ferias/services.py:366`**, pelas cascas services:398 e :403; deve constar no grupo divergente por validação/trilha.
   - Normal aceita só status `agendado`, views:649; admin aceita qualquer não cancelado, views:452, exige justificativa ≥5, views:454, e registra log, views:497. Normal chama escritor e responde, views:667, sem essa trilha.
   - Ambos validam saldo pelo mesmo juiz `saldo_para_remarcar`, views:465 e :661 → `app/ferias/models.py:219`.
   - Mesmo efeito final: altera datas/dias/aviso/status, limpa Ausencias da janela antiga e atualiza concedidos em `atomic`, services:377. **Muda dia: sim**, porque a porta admin permite mover férias já iniciadas/concluídas que a normal recusa; mesmos leitores de cobertura/folha descritos acima. Nenhuma divergência do escritor das datas: está unificado.

3. **Materializar férias: agendamento individual/lote × aprovação de solicitação × registro histórico.**
   - Individual/lote usam `executar_agendamento`, services:306. Aprovação usa escritor próprio `aprovar_solicitacao`, services:410, instanciando/salvando Agendamento diretamente, services:434; histórico usa `registrar_gozo_historico`, services:270, também salvando diretamente, services:298.
   - Individual/lote passam `_validar_agendamento`, views:226, incluindo saldo, fracionamento, duplicata e veto, views:232, :239, :241, :253. Aprovação seleciona apenas PA adquirido, services:424, testa saldo, :430, e faz `full_clean`, :443; primeiro marca solicitação aprovada, :416, e pode terminar **sem agendamento**, retornando aviso sem PA/saldo ou capturando exceção, :428 e :448.
   - Histórico valida coerência e teto de vendidos, services:289; não chama `clean` do agendamento e limita `dias_concedidos` por `min(direito, concedidos + dias)`, services:299. Individual/lote incrementam sem esse teto, services:324.
   - **Muda dia/saldo: sim**: o mesmo intervalo pode ser recusado no agendamento padrão, gravado como histórico, ou ter solicitação aprovada sem materialização; são inferências dos caminhos citados, ocorrência concreta **[não verificado]**.
   - Convergência inferior: todos salvam `AgendamentoFerias`, cujo save executa veto e espelho de Ausencia, `app/ferias/models.py:434`. Espelho materializa Ausencia aprovada por dia, `app/ponto/services/ausencia.py:1202`. Portanto não afirmar ausência de veto no histórico.
   - Leitores: cobertura do espelho, `app/ponto/services/espelho.py:588`; folha lê datas dos agendamentos, `app/folha/export.py:491`; ausência de agendamento muda retenção `ferias_com_batida`, export:868. Valores pagos em produção **[não verificado]**.

**Pares convergentes para o fim do censo**

- **Agendar individual × lote**: mesmo juiz `_validar_agendamento`, views:288/:605; mesmo escritor `executar_agendamento`, services:306, por views:298/:617. Lote revalida na execução, views:616, e grava observação `[agendado em lote ...]`, views:618; individual usa `obs` informado, views:286. Resultado de dias igual para os mesmos dados aceitos; diferença de seleção/carimbo, não escritor.
- **Cancelar admin individual × lote × concluída**: converge em `executar_cancelamento_admin`, services:341; mantenha referências também no grupo divergente pelas guardas e auditoria distintas, não conte duas vezes.
- **Criar/sincronizar PA na tela completa × drawer**: `ferias/criar_periodo.html:9` e `_secao_colab.html:6` → mesma view `criar_periodo_aquisitivo`, views:123 → `garantir_periodo_aquisitivo`, services:254. Drawer oferece gozo histórico, `_secao_colab.html:22`; template completo não tem esses campos, `criar_periodo.html:9` até :30. **Mesmo endpoint, capacidade de tela diferente**, sem escritor duplicado.
- **Agendar na tela completa × drawer**: `agendar.html:13` e `_secao_colab.html:60` → mesma view views:269 → services:306. Template completo permite `obs`, `agendar.html:27`; formulário drawer tem início/dias, `_secao_colab.html:60` até :78.
- **Cancelar normal em saldo × drawer**: `saldo.html:71` e `_secao_colab.html:160` → mesma view views:321 → services:330. Saldo mostra botão por status `agendado`, saldo:70; drawer exige `pode_cancelar`, `_secao_colab.html:159`, que também exige ainda não ter iniciado, models:330. Assim há diferença de disponibilidade da tela mesmo com endpoint comum.

Ações com só um fluxo de tela identificado neste app: rejeitar solicitação, excluir PA, enviar confirmação, vender dias; chamadas e templates constam na tabela. Editar PA tem view escritora comprovada, mas disparador de tela **[não verificado]**. O abono pecuniário vendido grava `dias_vendidos`, services:507; não confundir com abono de falta. Não incluir venda histórica como segunda ação de tela: a view aceita `dias_vendidos`, views:194, mas os formulários examinados não apresentam esse campo; disparo visual **[não verificado]**.

## 06/11 21:22 · sessao 8ec4d90b3e8e · - · - · origem=Ronald (tmux)

Varredura `api`/`api_core` e `colaboradores` fechada: **1 view POST admin no recorte**, `colab_regularizar`; as portas API operam sobre o próprio `request.user.colaborador`, não oferecem decisão administrativa sobre outra pessoa (`app/colaboradores/views.py:2948`, `app/colaboradores/views.py:2960`, `app/api/views.py:2608`, `app/api/views.py:2853`, `app/api/views.py:2998`, `app/api/views_core.py:1171`, `app/api/views_core.py:1316`).

**Achado divergente adicional: retratar batida pelo calendário versus drawer.**

- Calendário: rota `colaboradores/<cid>/calendario/` → `calendario_ponto` → `colaboradores/calendario_partial.html`; fornece `motivos_retratacao=list(_mot())` (`app/colaboradores/urls.py:24`, `app/colaboradores/views.py:1935`, `app/colaboradores/views.py:1956`, `app/colaboradores/views.py:1958`, `app/colaboradores/views.py:1962`).
- Drawer: rota `colaboradores/colab/<cid>/diagnostico-drawer/` → `colab_diagnostico_drawer` → `colaboradores/partials/_drawer_diagnostico.html`; contexto vem de `contexto_drawer`, cujo retorno não contém `motivos_retratacao` (`app/colaboradores/urls.py:86`, `app/colaboradores/views.py:2938`, `app/colaboradores/views.py:2942`, `app/colaboradores/services/drawer.py:139`).
- Ambos incluem `_calendario_grade.html`, que contém o mesmo formulário POST para `ponto:retratar_batida`, com `<select name="motivo_id" required>` preenchido por `motivos_retratacao` (`app/templates/colaboradores/calendario_partial.html:62`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`, `app/templates/colaboradores/partials/_calendario_grade.html:121`).
- Portanto a casca drawer não fornece opções do motivo obrigatório; bloqueio observado em navegador/produção **[não verificado]**. Convergem no endpoint `retratar_batida_view` e seu escritor `_retratar`, recebendo esse `motivo_id` (`app/ponto/views.py:3274`, `app/ponto/views.py:3280`).

**Regularização externa — convergência de escritor com divergência de entrada.**

- Drawer/form próprio: `colab_regularizar` exige categoria válida e pelo menos um item; não exige motivo textual; chama `criar_regularizacao_externa` (`app/colaboradores/views.py:2961`, `app/colaboradores/views.py:2965`, `app/colaboradores/views.py:2981`, `app/colaboradores/views.py:2983`). Template `colaboradores/partials/_form_regularizar.html` fornece categoria obrigatória e observação opcional (`app/templates/colaboradores/partials/_form_regularizar.html:2`, `app/templates/colaboradores/partials/_form_regularizar.html:7`, `app/templates/colaboradores/partials/_form_regularizar.html:12`).
- Painel chamados: `regularizacao_criar` exige motivo textual, abre lote vazio com categoria default vazia, por `abrir_lote`; este deduplica lote vazio e converge em `criar_regularizacao_externa` (`app/chamados/views_regularizacao.py:42`, `app/chamados/views_regularizacao.py:48`, `app/chamados/views_regularizacao.py:54`, `app/chamados/services/regularizacao_ext.py:309`, `app/chamados/services/regularizacao_ext.py:312`, `app/chamados/services/regularizacao_ext.py:318`).
- Escritor comum cria lote, itens e chamado no fio (`app/chamados/services/regularizacao_ext.py:230`, `app/chamados/services/regularizacao_ext.py:236`, `app/chamados/services/regularizacao_ext.py:246`). Categoria gravada alimenta `metricas_marreta`, inclusive balde `_nao_classificado`; criação via drawer não passa pela deduplicação de `abrir_lote` (`app/chamados/services/regularizacao_ext.py:238`, `app/chamados/services/regularizacao_ext.py:277`, `app/chamados/services/regularizacao_ext.py:285`, `app/chamados/services/regularizacao_ext.py:288`).

**Convergências das cascas colaboradores.**

Calendário e drawer compartilham os formulários de resolver dia, flip e desfazer retratação; incluem a mesma grade e barra de lote (`app/templates/colaboradores/calendario_partial.html:62`, `app/templates/colaboradores/calendario_partial.html:63`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:36`, `app/templates/colaboradores/partials/_calendario_grade.html:121`, `app/templates/colaboradores/partials/_calendario_grade.html:126`). Forms resolver dia apontam ambos para `ponto:veredito_celula` (`app/templates/ponto/partials/_veredito_dia.html:17`, `app/templates/ponto/partials/_veredito_lote_barra.html:2`).

**API/core e exclusões.**

- Config normal inclui `api.urls`; config core inclui `api.urls_core` (`app/config/urls.py:37`, `app/config/urls_core.py:6`).
- Core apenas expõe responder regularização e regularizar turno neste recorte, para o próprio colaborador; convergem respectivamente em `confirmar_e_materializar`/`recusar_regularizacao_externa` e `regularizar_turno_orfao`, iguais à API normal (`app/api/urls_core.py:16`, `app/api/urls_core.py:19`, `app/api/views_core.py:1195`, `app/api/views_core.py:1325`, `app/api/views_core.py:1329`, `app/api/views.py:2877`, `app/api/views.py:3007`, `app/api/views.py:3011`).
- `mensageria/validar` é GET de leitura; `mensageria/decidir` retorna fila; `mensageria/resolver-dia-recusado` lê contador de recusas. Não contam como views de escrita administrativa (`app/api/views_mensageria.py:523`, `app/api/views_mensageria.py:539`, `app/api/views_mensageria.py:1134`, `app/api/views_mensageria.py:1148`, `app/api/views_mensageria.py:1166`, `app/api/views_mensageria.py:1847`, `app/api/views_mensageria.py:1863`).
- Único POST mensageria é push remoto; fora da ação administrativa por tela solicitada (`app/api/views_mensageria.py:477`, `app/api/views_mensageria.py:478`).
- `pendencias_disputa` apenas renderiza leitura; calendário e diagnóstico são cascas que encaminham os POSTs ao app ponto (`app/colaboradores/views.py:2994`, `app/colaboradores/views.py:2997`, `app/colaboradores/views.py:3007`, `app/colaboradores/views.py:1935`, `app/colaboradores/views.py:2938`).

## 06/11 21:22 · sessao 9e0e2735af5c · - · - · origem=Ronald (tmux)

(3) Contradições e limites comprovados:

| Ponto v3 | Código da cópia |
|---|---|
| Identidade com episódio | A1.2 usa `(colaborador, motivo, data_evento)`; consulta não filtra estado ou disputa e segura a última linha. Conversa sem data informada nasce sempre, gravando o dia civil atual. Não há dimensão episódio nessa identidade. `app/chamados/models.py:2380-2405`, `:2422-2432`, `:2475-2477`. |
| Reaproveitamento em qualquer estado | **Confirmado**: existente em qualquer disputa/estado é devolvida; pode trocar célula/destino e relavrar cache, sem limpar carimbos ou reabrir automaticamente dentro desse gate. `app/chamados/models.py:2398-2405`, `:2431-2475`. |
| “Episódio” | Já existe vocábulo com sentido diferente: agrupamento de chamados pela mesma batida origem; clones ficam em `chamados_relacionados_ids`. Não é geração pós-extinção. `app/chamados/services/disputa_emissao.py:1018-1041`; `app/chamados/models.py:2838-2844`. |
| Correção de dia = re-chavear ocorrência | A porta habitual `reapontar_resolvedora` só preenche `data_evento` quando antes vazio. Dia existente não é corrigido nesse ramo; novo nascimento consulta a identidade pelo novo dia. `app/chamados/services/fio.py:831-835`; `app/chamados/models.py:2431`. Uma porta de re-chaveamento com tratamento de colisão: **não achei** nesses dois sítios. |
| Extinta não ressuscita | Hoje há reabertura de chamado, descarimbo e volta de pergunta/disputa; a nomenclatura não equivale a extinção. `ChamadoColaborador.reabrir` verifica premissa e fato novo para fechamento admin, mas reutiliza a mesma linha e ressuscita perguntas. `app/chamados/models.py:1296-1320`, `:1390-1402`. |
| Quais ressuscitam | `ressuscitar_perguntas` filtra materializadas sem texto, testa marco e C6 sobre cópia descarimbada e limpa carimbos; também reabre disputa. `app/chamados/models.py:1434-1458`, `:1499-1507`, `:1532`. Outras inversas: fantasma, resposta nova sem lastro, competência reaberta. `app/chamados/models.py:2723-2757`. `desfazer_carimbo_sem_lastro` lê C6 sob trava e pode reabrir disputa fechada. `app/chamados/services/relavra_via.py:217-245`. |
| Cobrar suspensa | Não afirmar isso sobre fila admin: `qs_na_fila` exclui silêncio futuro. `app/chamados/services/painel.py:158-188`. Contudo, silêncio não é estado: é véu. `app/chamados/services/arquivo.py:10-14`, `:33-36`. App consulta `q_vivas` + C6 sem olhar silêncio, e C6 não consulta o véu nesse corpo. `app/chamados/services/pergunta_no_app.py:42-53`; `app/chamados/juizes.py:1135-1218`. `recobrar_questionario` conta perguntas cruas sem resposta, renova prazo e chama push sem guarda de silêncio. Caminho estático existe; ocorrência em produção **[não verificado]**. `app/chamados/services/disputa_emissao.py:1485-1505`. |
| Responsável inalcançável → admin/incident | Push web retorna `sem_usuario`, FCM retorna `sem_token`; emissor ignora retornos e só trata exceções. Redirecionamento admin/incident não achei nesses caminhos. `app/chamados/push_utils.py:114-118`; `app/chamados/fcm_utils.py:101-106`; `app/chamados/services/disputa_emissao.py:1540-1552`. |
| Exatamente um encaminhamento | Juiz existente admite **várias portas/verbos simultâneos**, consolidando um `fio` pela escada. Portanto distinguir unicidade de responsabilidade consolidada de unicidade de porta real. `app/chamados/juizes.py:1323-1338`, `:1350-1357`, `:1376-1387`. |
| Um módulo para todas transições | Hoje portas distintas: estado chamado no modelo, carimbo no modelo, resposta em `disputa_emissao`, validação em `validacao`, silêncio em `arquivo`, costura em `fio`. A lista de escritores confirma separações; não concluir “escrita solta” apenas por estarem em módulos distintos. `app/core/tests/test_contract_escritores_estado.py:35-62`, `:83-103`; `app/chamados/services/validacao.py:343`; `app/chamados/services/disputa_emissao.py:1362`. |

(5) Cortaria ou reformularia no ADR:

- **Cortar reinvenção da preservação da resposta:** já há `RespostaAnterior`, copiada antes de zerar resposta e carimbos, no mesmo `atomic`, com emissões para a célula. Manter como contrato de integração. `app/chamados/services/disputa_emissao.py:1393-1417`.
- **Cortar outbox de célula “do zero”:** porta já emite `EventoCelula` e consumidor já existe; extensão de contrato/versão/admin precisa ser declarada, sem apresentar mecanismo como inexistente. `app/ponto/portas/celula.py:882-902`; `app/chamados/reconciliador.py:1304`.
- **S4 sombra:** se significar validação por equivalência ao código antigo, contradiz L-110. Certificação deve vir de casos pela regra e propriedades; sombra/diff podem medir impacto. Se significar convergência/idempotência entre consumidores do modelo novo, isso é outro uso e precisa estar escrito. `CLAUDE.md:44-58`; `app/docs/LEIS.md:136`.
- **Episódio/extinção:** não são simples campos técnicos: mudam identidade, dedup e a lei atual de ressuscitação. Precisam de pergunta/universo explícitos e corte para autoridade nova; não cabem como “PM não julga” se PM decidir o que os juízes existentes não respondem. `app/chamados/models.py:2398-2405`; `CLAUDE.md:76-78`, `:171-179`; `app/docs/LEIS.md:138`.
- **Histórico:** manter histórico de transições, porém evitar transformar isso em event sourcing completo ou bitemporal amplo: o plano proíbe ambos fora do escopo declarado. `app/docs/PLANO-PADROES.md:3-5`.
- **Passivo em três rótulos:** não achei consumidores de `ORIGINAL | RECONSTRUIDO COM LASTRO | AMBIGUO` como classificação de obrigação em `app/chamados`, `app/escala/models.py`; o código tem “ambíguo” em instrumentos distintos, sem esse contrato **[não verificado]**. Não introduzir campos sem leitor definido; regra local exige nome e leitor, e remove autoridades sem chamador. `CLAUDE.md:42-43`, `:184-185`.
- **Fronteira completa:** coerente como censo, mas a ordem proposta não dispensa atualizar duas cascas e medir unidades/universos. Regra exige leitor sem derivação e censo fechado de escritores antes de filtro novo. `CLAUDE.md:20-22`, `:187-194`; `app/docs/LEIS.md:98`.
- **Falta no ADR horizonte da migração:** obra nova tem horizonte para frente; passado conserva foto até fato novo; backfill único final é obra própria. Migrar todo passivo silenciosamente contradiz esse corte. `app/docs/LEIS.md:145`; `app/docs/PLANO-PADROES.md:61-67`.
- **Falta distinguir suspensão de silêncio:** ausência aberta suspende por prazo e renova; prazo vencido pode devolver cobrança sem novo evento, enquanto ausência aprovada só encerra se premissa morrer e célula deixar de acusar. `app/chamados/services/silencio_ausencia.py:1-13`, `:60-99`.
- **Falta separar “porta existe” de usuário capaz de agir:** `porta_alcancavel_do_dia` verifica fila/app/âncora e exclui deliberadamente acesso manual e Resolver dia; não prova usuário humano habilitado ou canal operacional. `app/chamados/juizes.py:1341-1369`, `:1376-1387`.
- **Falta definir unknown:** C6 tem saídas conservadoras diferentes em degraus diferentes, e cache distingue indeterminado/nunca lavrado. “Sem prova” não pode virar uma classificação homogênea de extinção ou suspensão sem decisão nova. `app/chamados/juizes.py:1204-1218`; `app/chamados/models.py:2334-2339`.

## 06/11 21:22 · sessao f6ce0f964ded · - · - · origem=Ronald (tmux)

Colaboradores fechado: as duas superfícies do recorte convergem no mesmo endpoint POST; não encontrei escritor local de `Ausencia`, `Justificativa` ou férias nas views de colaboradores. Rotas varridas: `app/colaboradores/urls.py:7-89`; configuração: `app/config/urls.py:26`.

| Ação | Tela A | Tela B | Escritor/resultado |
|---|---|---|---|
| Decidir falta ou ausência justificada, individual/lote | `/colaboradores/<cid>/calendario/`; `calendario_ponto`, `app/colaboradores/views.py:1935`; `app/templates/colaboradores/calendario_partial.html:62-63` | `/colaboradores/colab/<cid>/diagnostico-drawer/`; `colab_diagnostico_drawer`, `app/colaboradores/views.py:2938`; `app/templates/colaboradores/partials/_drawer_diagnostico.html:35-36` | Ambas usam `_calendario_grade.html:126` → `ponto/partials/_veredito_dia.html:17` e `_veredito_lote_barra.html:2` → `/ponto/celula/veredito/`, `app/ponto/urls.py:58` → `veredito_celula_view`, `app/ponto/views.py:3312-3341` → `veredito_celula`, `app/ponto/services/veredito_celula.py:191`. |

Converge em `veredito_celula`: falta/justificado criam `Ausencia` por `criar_ausencia`, com um dia, status aprovada e autor/aprovador (`app/ponto/services/veredito_celula.py:310-317`); fecha o fio, lavra veredito humano na célula e registra auditoria (`app/ponto/services/veredito_celula.py:324-336`). Nenhuma divergência de escrita entre calendário e drawer demonstrada por esses caminhos. As duas superfícies não acrescentam views POST: consomem uma view POST de ponto (`app/ponto/views.py:3312-3341`).

Par adicional divergente para consolidar com ponto: **lançar ausência × Resolver dia**, embora ambos chamem `criar_ausencia`.

- Lançamento: `/ponto/ausencias/lancar/` [rota exata não conferida]; `app/ponto/views.py:2368`; template `ponto/lancar_ausencia.html`, renderizado em `app/ponto/views.py:2460`. Passa range, minutos, documento e HE100 (`app/ponto/views.py:2415-2422`); para INSS grava `Colaborador.situacao='afastado'` (`app/ponto/views.py:2433-2435`).
- Resolver dia: superfícies acima; passa um dia e não fornece minutos/documento/HE100 (`app/ponto/services/veredito_celula.py:315-317`); fecha perguntas/chamados, lavra célula e registra log de veredito (`app/ponto/services/veredito_celula.py:324-336`). Não chama a alteração de situação existente no lançamento (`app/ponto/services/veredito_celula.py:309-337`).
- Leitor do carimbo exclusivo: calendário lê `CelulaDia.veredito`, `veredito_via`, `veredito_em` e `decisao_do_dia` (`app/colaboradores/services/calendario.py:411-416`).
- Leitores da ausência: calendário usa cobertura para cobrança (`app/colaboradores/services/calendario.py:484-485`) e para folha (`app/colaboradores/services/calendario.py:532-534`); `DiaPago` extrai abono por dia da grade (`app/ponto/services/dia_pago.py:362-400`); export identifica dias que descontam pelo catálogo (`app/folha/export.py:169-181`).
- Gravidade: caminhos permitem alcance diário/minutos diferentes, capazes de alterar dias/abono, conforme os parâmetros anteriores e o cálculo de abono (`app/ponto/services/dia_pago.py:391-399`); diferença monetária para entrada equivalente ou ocorrência em produção: **[não verificado]**.

Fora do recorte estrito: `colab_regularizar` escreve regularização de batidas via `criar_regularizacao_externa`, sem escrever ausência nessa view (`app/colaboradores/views.py:2948-2990`); agenda e folgas tratam outros estados (`app/colaboradores/views.py:212-259`, `app/colaboradores/views.py:2261-2397`).

## 06/11 21:22 · sessao 781c44e2a3ea · - · - · origem=Ronald (tmux)

Q1 — os sete itens existem, mas não respondem uma pergunta homogênea de exigibilidade.

| Item | Assinatura/endereço | Pergunta/universo real; limite para o ADR |
|---|---|---|
| `VEREDITOS_ACUSAM` | Constante de classe, `app/escala/models.py:1767` | Pertencimento do **veredito do dia** a sete acusações. Não recebe motivo, marco, responsável, ausência nem episódio; o comentário define continuidade de pergunta/chamado, mas o juiz da pergunta pode encerrar um marco mesmo com outro marco do dia acusando (`app/escala/models.py:1760`; `app/chamados/juizes.py:1173`). |
| `premissa_morta` | `(modulo_origem, subcategoria=None, fatos: Optional[Mapping]=None) -> bool`, `app/chamados/catalogo/premissa.py:144` | Classe conhecida + fato sustentador explicitamente falso. Desconhecida ou fato não coletado permanece viva; não é “obrigação exigível” nem comprovação universal de extinção (`app/chamados/catalogo/premissa.py:43`; `app/chamados/catalogo/premissa.py:115`). A tabela cobre dez classes exatas/wildcards e escolhe um fato por classe (`app/chamados/catalogo/premissa.py:45`). |
| `ausencia_cobre` | `(colab, data_ref, *, para)`, `app/ponto/turnos.py:2536` | Retorna **uma Ausencia**, escolhida por maior data/PK, que toca a data por dia único, intervalo fechado ou aberto dos tipos permitidos (`app/ponto/turnos.py:2552`). `para='cobranca'` exclui rejeitadas; `folha` aceita só aprovadas (`app/ponto/turnos.py:2509`). Não responde sozinho integral/parcial nem aplicabilidade ao marco. |
| `dia_sem_trabalho` | `(cel) -> bool`, `app/chamados/juizes.py:413` | `trabalha is False` ou ata com tipo preenchido diferente de trabalho; ausência parcial permite resto, salvo `trabalha=False` (`app/chamados/juizes.py:422`). Não é previsto de escala nem qualquer obrigação de qualquer motivo. |
| `STATUS_QUE_PEDEM_DECISAO` | Tupla em `app/colaboradores/services/calendario.py:94` | Vocabulário do botão “Resolver dia”; o juiz é `resolver_dia_oferecido(dia, hoje)` e exige **último recurso + passado + status**, excluindo permissão do usuário (`app/colaboradores/services/calendario.py:97`). Usar só a tupla perde duas cláusulas. |
| “cobertura parcial” | Família, não uma função única: `cobre_parte_do_dia(a, parciais=None)` em `app/ponto/turnos.py:2583`; `ausencia_explica_o_dia_inteiro(a, parciais=None)` em `app/ponto/turnos.py:2668`; `periodo_da_parcial_no_dia(a, horas)` em `app/ponto/turnos.py:2614`; `ausencia_parcial_cobra_o_resto(registro)` em `app/chamados/juizes.py:400` | Quatro perguntas diferentes: registro parcial (<480 min, um dia, tipo aceita horas); explica todo dia (não falta nem parcial); relação das batidas com período (`sem_periodo/dentro/fora/nao_sei`); leitura do carimbo parcial na ata/grade. Não substituir uma pela outra (`app/ponto/turnos.py:2595`; `app/ponto/turnos.py:2675`; `app/ponto/turnos.py:2632`; `app/chamados/juizes.py:410`). |
| “juiz do turno” | Autoridade registrada `turnos_do_colab(colab, ini, fim, celulas=None)` em `app/core/juizes.py:196`; implementação `app/ponto/turnos.py:1848` | Retorna turnos cujo `data_turno` cai na janela; carrega batidas não retratadas com margem e marcos por vínculo/célula (`app/ponto/turnos.py:1860`; `app/ponto/turnos.py:1990`). Corpo do dia é `_data_do_turno(entrada, vespera, saida=None, cont_max_s=14*3600)`; também trata saída órfã pelo envelope da véspera (`app/ponto/turnos.py:246`; `app/ponto/turnos.py:291`). Não oferece dia canônico para fato sem batida/turno por si só. |

Comparação com anexo: a cópia contém a mesma descrição de cobertura e exceção parcial em `app/docs/JUIZES.md:50` e `app/docs/JUIZES.md:59`; também declara `_protesta` não migrado para a segunda cláusula (`app/docs/JUIZES.md:70`). Confirmação viva: `_protesta` usa somente `trabalha is not False` e marcos acesos (`app/ponto/services/cartorio.py:1407`). O lookup não lista `STATUS_QUE_PEDEM_DECISAO` nem o juiz `resolver_dia_oferecido` nas linhas examinadas (`app/docs/JUIZES.md:20` a `app/docs/JUIZES.md:106`).

Há diferença de **domínio de dia** não explicada pelo lookup: `data_do_chamado(modulo_origem, contexto_json)` apenas lê a chave declarada do catálogo (`app/chamados/catalogo/modulos.py:219`). `dia_do_fato(chamado, fortes=False)` aceita célula, catálogo, `data_turno` e, sem `fortes`, dia civil da batida, datas de disputa e carimbos (`app/chamados/juizes.py:80`; `app/chamados/juizes.py:47`; `app/chamados/juizes.py:122`). Nenhuma dessas duas funções reexecuta o pareador para canonicalizar o dia do chamado (`app/chamados/juizes.py:90`; `app/chamados/catalogo/modulos.py:222`).

Q4 — não achei a função literal “existe obrigação exigível para `(colab,motivo,dia_turno)`”; procurei `obrigacao`, `exigivel`, funções `*exig*`, cobrança/premissa nos seguintes arquivos: `app/chamados/juizes.py`, `app/chamados/models.py`, `app/chamados/services/vigia_fio.py`, `app/chamados/services/materializacao.py`, `app/ponto/services/cartorio.py`, `app/core/juizes.py` [não verificado para arquivos fora desse escopo].

As aproximações existentes não equivalem:

- `cobranca_viva_do_dia(colaborador, modulo_origem, dia, *, excluir=None)` procura o chamado mais novo de módulos dados em `VIVOS`, ligado à célula/data JSON e confirmado por `dia_do_fato(fortes=True)`; não filtra motivo da pergunta, suspensão ou alcance (`app/chamados/juizes.py:1886`).
- `pergunta_viva(p)` responde sobre uma **pergunta já existente**, considerando validação, tranca, resposta, vias de identidade, marco, veredito e testemunha materializada; não descobre obrigação sem pergunta (`app/chamados/juizes.py:822`; `app/chamados/juizes.py:1135`).
- `fio_vivo(perguntas)` reduz `any(pergunta_viva(...))` no universo recebido, expressamente definido pelo chamador (`app/chamados/juizes.py:1233`).
- `porta_alcancavel_do_dia(celula_id, *, alimentacao=None)` já responde **alcance** por chamado da célula na fila OU pergunta viva no app com âncora na fila. Retorna todos os verbos/portas; não impõe “exatamente um”. “Resolver dia” é explicitamente outro balde, fora dessa porta (`app/chamados/juizes.py:1341`; `app/chamados/juizes.py:1350`; `app/chamados/juizes.py:1367`).

Os universos diferem materialmente:

| Universo | Recorte atual |
|---|---|
| Premissa | Módulo/subcategoria e fato escolhido pelo coletor; fora do catálogo não extingue (`app/chamados/catalogo/premissa.py:124`). |
| Cobertura | Um colaborador/data; nenhuma exigência de célula, chamado, ativo ou competência (`app/ponto/turnos.py:2552`). |
| Turno | Batidas não retratadas + janela/marcos; pode haver mais de um turno no mesmo dia (`app/ponto/turnos.py:1861`; `app/chamados/juizes.py:1919`). |
| Pergunta viva | Pergunta existente; tranca antes da resposta; fallback de falta só na competência anterior + atual + véspera (`app/chamados/juizes.py:1138`; `app/chamados/juizes.py:1197`; `app/chamados/juizes.py:556`). |
| Vigia `censo` | Chamados `VIVOS` com célula, sem janela, depois veredito acusador; distingue `FURO_MODULES` de outros módulos (`app/chamados/services/vigia_fio.py:335`; `app/chamados/services/vigia_fio.py:350`; `app/chamados/services/vigia_fio.py:378`). |
| Vigia `censo_alcance` | Empresas visíveis e competências a lavrar; dias retidos fornecidos por `dias_que_seguram`, mesmo sem chamado vivo (`app/chamados/services/vigia_fio.py:566`; `app/chamados/services/vigia_fio.py:577`). |
| Suspensão por ausência | Apenas chamados vivos de `CALADOS_POR_ISENCAO ∪ {'justificativa'}`; ausência aberta suspende, aprovada encerra somente com premissa/lâmpada compatíveis (`app/chamados/services/silencio_ausencia.py:21`; `app/chamados/services/silencio_ausencia.py:60`; `app/chamados/services/silencio_ausencia.py:93`). |

A composição já existente `dono_fora_do_fio` lê, em ordem, Resolver dia, ausência aberta, folga validada e lista admin; recebe **todas as perguntas da célula**, sem motivo, e é compartilhada pelos dois censos (`app/chamados/services/vigia_fio.py:274`; `app/chamados/services/vigia_fio.py:306`). Transformar essa composição em sentença universal de obrigação por motivo/episódio adiciona quantificação, precedência e tratamento dos ausentes que essas APIs não definem [não verificado]; a própria redução por precedência de classe de dia é documentada como predicado novo no vigia (`app/chamados/services/vigia_fio.py:94`).

Falta crucial no ADR: o juiz **`motivo_do_veto(colab, ec, data)`**. Ausência de vínculo/template ou régua suspeita pode vetar cobrança automática mesmo com célula acusadora (`app/ponto/services/furos_vetados.py:27`). Seu universo declarado é `FURO_MODULES ∪ MODULOS_DE_TURNO` (`app/ponto/services/furos_vetados.py:74`). Logo, célula acusadora + premissa viva + sem ausência não bastam para dizer exigível (`app/ponto/services/furos_vetados.py:35`). O leitor de furos vetados exclui desligados, mantém afastados deliberadamente; cobertura e turno não têm esse mesmo filtro (`app/ponto/services/furos_vetados.py:47`; `app/ponto/turnos.py:2552`; `app/ponto/turnos.py:1861`).

Leis aplicáveis à conclusão: um juiz por pergunta, um escritor por entidade e idempotência são contratos já vigentes (`CLAUDE.md:175`); leitores não derivam regras (`CLAUDE.md:20`); autoridade “já existente” só é declaração técnica se **mesmo predicado e universo igual antes/depois** (`CLAUDE.md:275`); novo juiz exige corte explícito (`CLAUDE.md:74`).

## 06/11 21:13 · sessao b38bc38e9798 · V2-BUGS-I2 · sonda · origem=Code

```python
# Sonda não executada; resultados por colaborador: [nao verificado].
# Caminho real, sem remontar motores:
# app/ponto/services/fechamento.py:724,754
# Leitura sem lavratura:
# app/ponto/services/fechamento.py:539,876,1121,1184
# Proteção adicional: transação PostgreSQL READ ONLY, sempre revertida.
# Comportamento dessa proteção no envelope: [nao verificado].

import sys
sys.dont_write_bytecode = True

import ast
import inspect
import io
import json
import logging
import math
import os
import re
from contextlib import redirect_stdout, redirect_stderr
from datetime import date

from django.db import connection, transaction
from django.utils import timezone
from ponto.services import fechamento as F
from ponto import motor_calculo_v2 as V
from core import regua_cct as R


PKS = [191, 270, 292, 301, 390, 468, 469, 825, 849, 865]
DIA = date(2026, 10, 12)
JANELA = (date(2026, 9, 21), date(2026, 10, 20))

REF_F = "app/ponto/services/fechamento.py:754"
REF_HE = "app/ponto/motor_calculo_v2.py:1014"
REF_M = {
    "M1": "app/ponto/motor_calculo_v2.py:3093",
    "M2": "app/ponto/motor_calculo_v2.py:3339",
    "M3": (
        "app/ponto/services/fechamento.py:955; "
        "app/ponto/services/dia_pago.py:244"
    ),
    "OUTRO": REF_HE,
}

COD_AP = F.apurar_por_fatia.__code__
COD_REC = F.recalcular_fechamento_mes.__code__
[nome] = {
    V.MotorTurnoPartido.calcular_mes.__code__: (3093, "M1"),
    V.MotorComercial.calcular_mes.__code__: (3339, "M2"),
}

# Captura também fallbacks com except Exception nos módulos observados.
# Exemplos: fechamento.py:1074,1080,1296; regua_cct.py:447.
HANDLERS = {}
for modulo in (F, V, R):
    caminho = os.path.abspath(inspect.getsourcefile(modulo))
    arvore = ast.parse(inspect.getsource(modulo))
    linhas = set()
    for node in ast.walk(arvore):
        if isinstance(node, ast.ExceptHandler):
            amplo = (
                node.type is None
                or isinstance(node.type, ast.Name)
                and node.type.id in {"Exception", "BaseException"}
            )
            if amplo:
                linhas.add(node.body[0].lineno)
    HANDLERS[caminho] = linhas


def numero(valor):
    n = float(valor)
    if not math.isfinite(n):
        raise ValueError("valor numerico nao finito")
    return n


def local(dt):
    return None if dt is None else timezone.localtime(dt).strftime(
        "%Y-%m-%d %H:%M:%S"
    )


def registrar_erro(st, exc, fonte):
    if not any(exc is anterior for anterior, _ in st["erros"]):
        st["erros"].append((exc, fonte))


def ocultar(st, texto):
    for nome in sorted(st["nomes"], key=len, reverse=True):
        texto = re.sub(re.escape(nome), st["colab"], texto, flags=re.I)
    return re.sub(
        r"(?<!\d)\d{3}\.?\d{3}\.?\d{3}-?\d{2}(?!\d)",
        "[CPF_REMOVIDO]",
        texto,
    )


def construir_trace(st):
    def trace(frame, evento, arg):
        caminho = os.path.abspath(frame.f_code.co_filename)
        if caminho not in HANDLERS:
            return None

        try:
            codigo = frame.f_code
            L = frame.f_locals
            linha = frame.f_lineno

            if codigo is COD_REC:
                colab = L.get("colab")
                if colab is not None:
                    nome = colab.__dict__.get("nome")
                    if isinstance(nome, str) and nome:
                        st["nomes"].add(nome)

            if evento == "line":
                if linha in HANDLERS[caminho]:
                    exc = sys.exc_info()[1]
                    # Ausência declarada, não erro: fechamento.py:54,1082.
                    if exc is not None and not isinstance(exc, F._GradeSemCelula):
                        registrar_erro(
                            st, exc, "%s:%d" % (caminho, linha)
                        )

                # Depois do calcular_mes real, antes da composição.
                # app/ponto/services/fechamento.py:286,294
                if codigo is COD_AP and linha == 294:
                    r = L["r_fatia"]
                    if id(r) not in st["fatias_vistas"]:
                        st["fatias_vistas"].add(id(r))
                        st["fatias"].append((
                            L["fatia_ini"], L["fatia_fim"], L["motor"], r
                        ))

                # Dentro do ramo verdadeiro: não reavalia feriado/dobra.
                ramo = [nome].get(codigo)
                if ramo is not None and linha == ramo[0]:
                    ultimo = L["ultimo"]
                    for p in L["periodos_dia"]:
                        st["ramos"][id(p)] = (
                            ramo[1], id(p) == id(ultimo)
                        )

                # Partição efetiva do fechamento, inclusive quando vazia.
                # app/ponto/services/fechamento.py:945,958
                if codigo is COD_REC and linha == 958:
                    st["ft_feriado"] = {
                        id(p) for p in L["_ft_feriado"]
                    }
                    st["janela"] = (L["_data_ini"], L["_data_fim"])

            if evento == "return" and codigo is COD_AP and arg is not None:
                st["apuracao"] = arg

        except Exception as exc:
            # Uma falha na instrumentação também invalida o colaborador.
            registrar_erro(st, exc, "[nao verificado]:instrumentacao")

        return trace

    return trace


def analisar(st, leitura, pk):
    if pk not in leitura:
        raise RuntimeError("colaborador ausente do retorno do recalculo")
    if st["apuracao"] is None:
        raise RuntimeError("apuracao real nao capturada")
    if st["ft_feriado"] is None:
        raise RuntimeError("particao ft_feriado nao capturada")
    if st["janela"] != JANELA:
        raise RuntimeError("janela real divergente: %r" % (st["janela"],))

    donos = {}
    linhas = []
    for ini, fim, motor, resultado_fatia in st["fatias"]:
        linhas.append({
            "tipo": "motor",
            "colab": st["colab"],
            "fatia": [ini.isoformat(), fim.isoformat()],
            "classe": type(motor).__name__,
            "feriado_em_dobra": bool(motor.feriado_em_dobra),
            "fonte": "app/ponto/services/fechamento.py:279",
        })
        for lista in ("periodos", "periodos_ft"):
            for p in getattr(resultado_fatia, lista):
                if id(p) in donos and donos[id(p)] is not motor:
                    raise RuntimeError("periodo atribuido a motores distintos")
                donos[id(p)] = motor

    resultado = st["apuracao"].resultado
    selecionados = {}
    for lista in ("periodos", "periodos_ft"):
        for p in getattr(resultado, lista):
            if timezone.localtime(p.entrada).date() == DIA:
                item = selecionados.setdefault(id(p), [p, []])
                item[1].append("resultado." + lista)

    parcelas = []
    mecanismos = set()

    for p, listas in sorted(
        selecionados.values(), key=lambda item: item[0].entrada
    ):
        motor = donos[id(p)]
        promovido = id(p) in st["ft_feriado"]
        extra100 = numero(
            p.minutos_trabalhados if promovido else p.minutos_extra_100
        )

        # Mesmas pontas, extra50, intervalos e prorrogação da casca real.
        # app/ponto/motor_calculo_v2.py:1014
        n50, n100 = V.he_noturna(
            entrada=p.entrada,
            saida=p.saida,
            extra_50_min=p.minutos_extra_50,
            extra_100_min=extra100,
            prorrogacao=motor.prorrogacao_noturna_pos5h,
            intervalos=p.intervalos_do_motor,
        )
        n50, n100 = numero(n50), numero(n100)
        subset = numero(p.minutos_extra_100_noturna)

        if promovido:
            mecanismo = "M3"
            motivo = "promocao_ft_soma_HE100_sem_somar_subset_noturno"
        elif subset != 0:
            mecanismo = "OUTRO"
            motivo = "subset_do_periodo_ja_e_diferente_de_zero"
        elif id(p) in st["ramos"]:
            mecanismo, ultimo = st["ramos"][id(p)]
            motivo = (
                "dobra_no_ultimo_periodo_com_continue_sem_split"
                if ultimo else
                "HE100_da_jornada_concentrada_no_ultimo_periodo"
            )
        else:
            mecanismo = "OUTRO"
            if "resultado.periodos_ft" in listas:
                motivo = "FT_nao_promovido_a_ft_feriado"
            elif extra100 == 0:
                motivo = "base_HE100_zero_no_periodo"
            elif n100 == 0:
                motivo = "he_noturna_tambem_devolve_zero"
            else:
                motivo = "zero_sem_M1_M2_M3_observado_[nao verificado]"

        mecanismos.add(mecanismo)
        parcelas.append((mecanismo, n50, n100))
        linhas.append({
            "tipo": "periodo",
            "colab": st["colab"],
            "classe": type(motor).__name__,
            "feriado_em_dobra": bool(motor.feriado_em_dobra),
            "entrada_local": local(p.entrada),
            "saida_local": local(p.saida),
            "listas": listas,
            "ft_feriado": promovido,
            "dia_da_jornada": str(
                getattr(resultado, "dia_da_jornada", {}).get(
                    id(p), "[nao verificado]"
                )
            ),
            "minutos_trabalhados": numero(p.minutos_trabalhados),
            "minutos_noturnos_relogio": numero(p.minutos_noturnos_relogio),
            "minutos_noturnos_legais": numero(p.minutos_noturnos_legais),
            "minutos_extra_100": numero(p.minutos_extra_100),
            "minutos_extra_100_noturna": subset,
            "extra_100_usado_na_sonda": extra100,
            "extra_50_usado_na_sonda": numero(p.minutos_extra_50),
            "prorrogacao": bool(motor.prorrogacao_noturna_pos5h),
            "intervalos_locais": [
                [local(a), local(b)] for a, b in p.intervalos_do_motor
            ],
            "he_noturna_retorno_min_50_100": [n50, n100],
            "he100_noturna_h": n100 / 60,
            "mecanismo": mecanismo,
            "explicacao": motivo,
            "fonte": REF_M[mecanismo] + "; " + REF_HE,
        })

    if not selecionados:
        mecanismos.add("OUTRO")
        linhas.append({
            "tipo": "sem_periodo",
            "colab": st["colab"],
            "explicacao": "nenhuma_entrada_local_em_2026-10-12_nas_duas_listas",
            "fonte": "app/ponto/services/fechamento.py:294",
        })

    return linhas, parcelas, mecanismos


def emitir(item):
    print(json.dumps(item, ensure_ascii=True, sort_keys=True))


grupos = {
    m: {"colabs": set(), "n50": [], "n100": []}
    for m in ("M1", "M2", "M3", "OUTRO")
}
falhas = []
sucessos = 0
logging_anterior = logging.root.manager.disable

try:
    logging.disable(sys.maxsize)

    for pk in PKS:
        st = {
            "colab": "col%d" % pk,
            "nomes": set(),
            "erros": [],
            "fatias": [],
            "fatias_vistas": set(),
            "ramos": {},
            "apuracao": None,
            "ft_feriado": None,
            "janela": None,
        }
        linhas = parcelas = mecanismos = None
        trace_anterior = sys.gettrace()

        # Não encaminha logs/prints internos, inclusive o nome no handler.
        # app/ponto/services/fechamento.py:1299
        with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
            try:
                if connection.vendor != "postgresql":
                    raise RuntimeError("protecao READ ONLY exige PostgreSQL")
                if connection.in_atomic_block:
                    raise RuntimeError("sonda exige transacao propria")

                with transaction.atomic():
                    with connection.cursor() as cursor:
                        cursor.execute("SET TRANSACTION READ ONLY")
                    try:
                        sys.settrace(construir_trace(st))
                        leitura = F.recalcular_fechamento_mes(
                            10, 2026,
                            colaborador_ids=[pk],
                            somente_leitura=True,
                            lavrar_dias_pagos=False,
                            retornar_objeto=True,
                        )
                    finally:
                        sys.settrace(trace_anterior)
                        transaction.set_rollback(True)

                if not st["erros"]:
                    linhas, parcelas, mecanismos = analisar(st, leitura, pk)

            except Exception as exc:
                registrar_erro(st, exc, "[nao verificado]:sonda")
            finally:
                sys.settrace(trace_anterior)

        if st["erros"]:
            exc, fonte = st["erros"][0]
            falha = {
                "tipo": "erro",
                "colab": st["colab"],
                "n_excecoes_capturadas": len(st["erros"]),
                "primeiro_repr": ocultar(st, repr(exc)),
                "fonte": fonte,
            }
            falhas.append(falha)
            emitir(falha)
            continue

        sucessos += 1
        for linha in linhas:
            emitir(linha)
        for mecanismo in mecanismos:
            grupos[mecanismo]["colabs"].add(pk)
        for mecanismo, n50, n100 in parcelas:
            grupos[mecanismo]["n50"].append(n50)
            grupos[mecanismo]["n100"].append(n100)

finally:
    logging.disable(logging_anterior)

# Contagem distinta por mecanismo; um colaborador pode aparecer em mais de um.
# Totais incluem somente colaboradores sem erro; minutos / 60:
# app/ponto/motor_calculo_v2.py:409,451
for mecanismo, grupo in grupos.items():
    emitir({
        "tipo": "total_mecanismo",
        "mecanismo": mecanismo,
        "n_colabs": len(grupo["colabs"]),
        "he50_noturna_h": math.fsum(grupo["n50"]) / 60,
        "he100_noturna_h": math.fsum(grupo["n100"]) / 60,
        "fonte": REF_M[mecanismo] + "; " + REF_HE,
    })

emitir({
    "tipo": "total_geral",
    "colabs_solicitados": len(PKS),
    "colabs_validos": sucessos,
    "colabs_com_erro_excluidos_das_somas": len(falhas),
    "he50_noturna_h": math.fsum(
        n for g in grupos.values() for n in g["n50"]
    ) / 60,
    "he100_noturna_h": math.fsum(
        n for g in grupos.values() for n in g["n100"]
    ) / 60,
    "contagens_por_mecanismo_podem_se_sobrepor": True,
    "total_parcial": bool(falhas),
    "fonte": REF_HE,
})
print("ERROS %d | %s | %s" % (
    len(falhas),
    "INVALIDO" if falhas else "VALIDO",
    "app/ponto/services/fechamento.py:1296",
))
if falhas:
    print("PRIMEIRO_REPR %s | %s" % (
        falhas[0]["primeiro_repr"], falhas[0]["fonte"]
    ))
```

Funções reais da cópia chamadas, conforme o motor de cada fatia:

- `recalcular_fechamento_mes` — `app/ponto/services/fechamento.py:448`
- `apurar_por_fatia` — `app/ponto/services/fechamento.py:132`
- `_montar_motor` — `app/ponto/services/fechamento.py:724`
- `get_motor_cct` — `app/core/regua_cct.py:428`
- `get_motor` — `app/ponto/motor_calculo_v2.py:3602`
- `exige_memos` — `app/ponto/calculador/nucleo.py:474`
- `MotorBase.calcular_mes` — `app/ponto/motor_calculo_v2.py:2265`
- `MotorTurnoPartido.calcular_mes` — `app/ponto/motor_calculo_v2.py:2899`
- `MotorComercial.calcular_mes` — `app/ponto/motor_calculo_v2.py:3295`
- `he_noturna` — `app/ponto/motor_calculo_v2.py:409`
- Auxiliares de `he_noturna`: `_inicio_do_trecho_extra` — `app/ponto/motor_calculo_v2.py:378`; `minutos_noturnos` — `app/ponto/motor_calculo_v2.py:82`; `minutos_noturnos_dos_intervalos` — `app/ponto/motor_calculo_v2.py:121`.

## 06/11 21:10 · sessao a403f60ac888 · - · - · origem=Ronald (tmux)

A sonda pode chamar `recalcular_fechamento_mes(10, 2026, colaborador_ids=[pk], somente_leitura=True, lavrar_dias_pagos=False, retornar_objeto=True)` e observar as instâncias efetivamente usadas, preservando `_montar_motor` e `apurar_por_fatia` reais. (`app/ponto/services/fechamento.py:448`, `app/ponto/services/fechamento.py:724`, `app/ponto/services/fechamento.py:754`)

Pontos seguros para `sys.settrace`, filtrando os `__code__` das funções:

- No frame `apurar_por_fatia`, evento `line` na linha **294**: `motor`, `r_fatia`, `esc`, `fatia_ini` e `fatia_fim` estão disponíveis após o cálculo. Deduplicar por `id(r_fatia)`, pois essa linha é cabeça de laço. (`app/ponto/services/fechamento.py:286`, `app/ponto/services/fechamento.py:294`)
- No mesmo frame, evento `return`: `arg` é `_Apuracao`, contendo resultado composto. As listas compostas conservam os mesmos objetos dos resultados individuais. (`app/ponto/services/fechamento.py:295`, `app/ponto/services/fechamento.py:306`, `app/ponto/services/fechamento.py:318`)
- No frame `recalcular_fechamento_mes`, evento `line` na linha **958**: `_ft_feriado` já foi classificado e existe mesmo quando vazio. Capturar identidades dessa lista. (`app/ponto/services/fechamento.py:945`, `app/ponto/services/fechamento.py:949`, `app/ponto/services/fechamento.py:958`)
- Erro fatal por colaborador é engolido em `except`; na linha **1297**, ler `frame.f_locals["_e_colab"]`. O log normal desse ramo contém `colab.nome`; desabilitar logging durante a sonda evita vazamento. (`app/ponto/services/fechamento.py:1296`, `app/ponto/services/fechamento.py:1299`)
- Existem erros internos engolidos na grade nas linhas **1074** e **1080**; observar esses handlers se a regra `ERROS` deve abranger falhas além do erro final do colaborador. `_GradeSemCelula` é uma ausência declarada tratada no mesmo ramo. (`app/ponto/services/fechamento.py:54`, `app/ponto/services/fechamento.py:1074`, `app/ponto/services/fechamento.py:1080`)

Garantias do modo leitura:

- Não consulta/trilha exportada para escrita quando `somente_leitura=True` e `lavrar_dias_pagos=False`. (`app/ponto/services/fechamento.py:539`)
- Cria `FechamentoMensal` destacado em memória, sem `get_or_create`. (`app/ponto/services/fechamento.py:876`, `app/ponto/services/fechamento.py:891`)
- Devolve o objeto destacado no dicionário. (`app/ponto/services/fechamento.py:1121`, `app/ponto/services/fechamento.py:1128`)
- Não entra na lavratura `DiaPago`. (`app/ponto/services/fechamento.py:1184`)

Funções reais relevantes e localização:

- `recalcular_fechamento_mes`: `app/ponto/services/fechamento.py:448`
- `_montar_motor`, função local real: `app/ponto/services/fechamento.py:724`
- `apurar_por_fatia`: `app/ponto/services/fechamento.py:132`
- `get_motor_cct`: `app/core/regua_cct.py:428`
- `get_motor`: `app/ponto/motor_calculo_v2.py:3602`
- `exige_memos`: `app/ponto/calculador/nucleo.py:474`

A escolha por classe deve ser lida do objeto capturado: `get_motor` considera geometria além do rótulo do template e pode converter `turno_partido` em `MotorComercial` ou `Motor12x36ComEscala`. (`app/ponto/motor_calculo_v2.py:3635`, `app/ponto/motor_calculo_v2.py:3663`)

## 06/11 21:09 · sessao 42f90021c573 · - · - · origem=Ronald (tmux)

- `app/ponto/motor_calculo_v2.py:409`: `he_noturna(*, entrada, saida, extra_50_min, extra_100_min, prorrogacao=True, intervalos=())` retorna `(minutos_50_noturnos, minutos_100_noturnos)`; a distribuição proporcional fica em `:446-451`.
- `app/ponto/motor_calculo_v2.py:1007`: `_aplicar_he_noturna` passa **entrada e saída cruas**, `minutos_extra_50`, `minutos_extra_100`, `motor.prorrogacao_noturna_pos5h` e `periodo.intervalos_do_motor` (`:1014-1018`). Não usar `efetivo_entrada/efetivo_saida` na sonda.
- Campos reais: `minutos_trabalhados` (`:213`), `minutos_noturnos_relogio` (`:214`), `minutos_noturnos_legais` (`:215`), `minutos_extra_100` (`:218`), `minutos_extra_100_feriado` (`:219`), `minutos_extra_100_noturna` (`:225`), `intervalos_do_motor` (`:247`).
- M1: `MotorTurnoPartido.calcular_mes` (`:2899`) zera extras/subsets (`:2933-2938`), atribui dobra (`:3092-3096`) e sai com `continue` (`:3101`); a chamada `_aplicar_he_noturna` fica no caminho normal (`:3147`).
- M2: `MotorComercial.calcular_mes` (`:3295`) zera extras/subsets (`:3319-3324`), atribui dobra (`:3337-3342`) e sai com `continue` (`:3352`); a chamada `_aplicar_he_noturna` fica no caminho normal (`:3399`).
- `_feriado_dobra_no_dia` retorna `bool(self.feriado_em_dobra) and self.eh_feriado(dia)` (`app/ponto/motor_calculo_v2.py:934`).
- Para comprovar o ramo efetivamente tomado sem reavaliar regras, um `sys.settrace` restrito aos code objects dos dois overrides pode registrar `frame.f_locals["ultimo"]` em eventos `line` nas linhas `3093` e `3339`: ambas já estão dentro da condição verdadeira da dobra (`app/ponto/motor_calculo_v2.py:3092-3093`, `:3337-3339`).
- O mapa `resultado.dia_da_jornada` inclui os períodos normais e FT no TurnoPartido (`app/ponto/motor_calculo_v2.py:2940-2959`).

_2573 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
