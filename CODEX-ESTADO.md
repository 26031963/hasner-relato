# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 14:55** · publicado em **12/11/2026 14:54** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 14:55 · sessao 617654d42634 · - · - · origem=Ronald (tmux)

Consolidado enviado ao pai, com referências físicas dos anexos.

- Os 14 faltantes v5 entram na ordem; as seis pousadas antigas saem; X1 passa a distinguir o restante. (`semente_item3_v6.py:12`, `:16–17`, `:61–63`; `contrato_item3_v6.txt:20–21`, `:36`, `:42`)
- ESMERIL-SEM-INSTANTE continua aberta no BACKLOG e entra sem trava, apesar do pouso documentado: viola concretamente o universo “não pousada”. (`app/docs/BACKLOG.md:1830`; `app/docs/RELATO.md:1005`; `semente_item3_v6.py:62`; `contrato_item3_v6.txt:2`, `:21`)
- SAIDA e AUD6 continuam sem identidade de tabela nesta cópia; o script exato, executado apenas em memória com sufixos simulados, retorna rc2. (`semente_item3_v6.py:55–57`, `:64–66`, `:82`; identidade presente: [nao verificado])
- APP continua atrás de O168, contrariando o aval literal citado na própria linha; a exceção não tem aval adicional demonstrado. (`app/docs/BACKLOG.md:1916`; `contrato_item3_v6.txt:26`, `:30`; aval adicional: [nao verificado])
- LASTRO-FOLGA permanece sem reconciliação; os planos ORDEM-ATE-O-ZERO e RAIA-2 entram mecanicamente, enquanto META é excluído explicitamente. (`app/docs/BACKLOG.md:1812`, `:1883`, `:1906`, `:2406`; `app/docs/RELATO.md:175`; `contrato_item3_v6.txt:20`, `:22`)
- Não bloqueia: a fórmula esquece descontar três IDs fora do bloco, mas o script já desconta; sufixos geram NAO SEI determinístico no matcher que o contrato manda preservar. (`contrato_item3_v6.txt:4`, `:10`, `:22–23`, `:44`, `:46`; `semente_item3_v6.py:72`; `bin/disjuntor.py:1688–1695`)

## 12/11 14:54 · sessao 4b2f9e7f47af · - · - · origem=Ronald (tmux)

Eu separaria assim:

1. **Novo bloqueante normativo:** exclusão geral por `_item_fechado` antes da reconciliação por ID. A v6:17,21 autoriza descartar fechadas pela coluna sem inventariá-las como pousadas; a semente:40–41 executa isso. O predicado atual também aceita tachado da descrição; `bin/hook_stop_fila1.py:168`. Isso não demonstra que alguma das 208 fechadas esteja indevidamente excluída — pouso delas: **[nao verificado]** —, mas o contrato permite exclusão sem a prova por ID exigida pelo aval. A v6:12 declara a mudança do censo e não resolve essa definição da semente. Corrigir exige reconciliar os IDs ou obter autorização para o critério por estado.

2. **ESMERIL-SEM-INSTANTE:** achado herdado **ABERTO**, com pouso documental já apontado pela leitura v5; `item3_contrato_v5.r`, seção 4; `app/docs/RELATO.md:1005`. Se permanece em ORDEM, contraria “não pousada”, mas a correção por ID é inequívoca; não é novo dilema contratual.

3. **Identidades ausentes:** **ABERTO operacional**, sem contar como novo bloqueante contratual pelo limiar solicitado. O contrato exige identidade única; v6:3. A semente recusa a inexistência; `semente_item3_v6.py:56–58`, `:85`. O contador fechar não substitui essa guarda. Não há licença contratual para omitir esses IDs silenciosamente.

4. **APP:** **ABERTO herdado**, agora com tentativa explícita de reinterpretação. A v6:30 reduz a LASTRO a precedência “antes de O168/LASTRO” registrada em `app/docs/BACKLOG.md:1916`; autorização nova para isso: **[nao verificado]**. Apresentaria no item 1 e não como defeito novo no item 2. Se o total final incluir qualquer aberto que requer aval, soma mais um.

5. **V5 integral ausente:** não contar como bloqueante; registrar detalhes herdados não demonstrados como **[nao verificado]**.

Portanto: **1 novo bloqueante** pelo critério estrito do item 2; **APP permanece aberto** e pode elevar para **2 impedimentos de fechamento** se o veredito contabilizar também os herdados que dependem de aval.

## 12/11 14:52 · sessao 211c513607a0 · - · - · origem=Ronald (tmux)

A trilha fornecida não identifica o escritor de `respondida_em` de nenhuma das 143 perguntas: **143 em SEM TRILHA, 17 colabs**; isso é conclusão sobre a suficiência do dado fornecido, não atribuição histórica. [não verificado]

- `pergunta_emitida` é gravado no nascimento, em `app/chamados/models.py:3026`, após inserir a pergunta em `app/chamados/models.py:3015`; esse log não descreve escrita posterior de resposta.
- `carimbo_desfeito_por_lastro` identifica a remoção do carimbo de materialização, em `app/chamados/services/relavra_via.py:244`, `app/chamados/services/relavra_via.py:246` e `app/chamados/services/relavra_via.py:248`. O antes/depois contém `materializada_em` e `via_resolucao`, não o ato que gravou `respondida_em`.
- A remoção chama `PerguntaDisputa.descarimbar`, que salva apenas `via_resolucao`, `materializada_em` e `materializada_por`, em `app/chamados/models.py:3209` e `app/chamados/models.py:3213`. Logo, o removedor não é o escritor da resposta.
- `chamado_encerrado` não identifica escritor único: é usado por `app/chamados/services/disputa_emissao.py:2058` e `app/chamados/services/materializacao.py:1269`.
- `sem_turno_aberto` tem lavrador atual em `app/chamados/reconciliador.py:341` e `app/chamados/reconciliador.py:406`, mas esse caminho não pede escrita de resposta: chama `carimbar` sem `resposta`. O comentário do bloco removido não prova autoria dos cinco PKs em produção, em `app/chamados/reconciliador.py:402`. Atribuir esses cinco ao escritor de `respondida_em` seria [não verificado].
- Igualdade entre `materializada_em` e `respondida_em`, bem como contagens de logs por disputa, não fornece identidade do ato escritor por PK. [não verificado]

Inventário das portas API/web e suas assinaturas:

| Porta | Escritor efetivo | Trilha e campos |
|---|---|---|
| Web `app/chamados/views.py:1265`, `app/chamados/views.py:1283` | `app/chamados/services/disputa_emissao.py:1467` | Core recusa vazio em `:1432–1434`; grava texto+timestamp em `:1467–1469`/`:1508`; decisão `resposta` em `:1511`; log `resp_pergunta_gravada`/`PerguntaDisputa` em `:1529`. |
| API `app/api/views.py:2614`, `app/api/views.py:2620` | Mesmo core | API recusa vazio em `app/api/views.py:2615`; logs `resp_pergunta_recusada`/`resp_pergunta_ok` via `app/api/views.py:2594`, `app/api/views.py:2624`; modelo `PerguntaDisputa`. |
| Correção admin `app/chamados/services/acoes_disputa.py:371` | Mesmo core | Texto HH:MM obrigatório em `:336`; log adicional `corrigir_resposta_admin` em `:375`; depois valida em `:379`. |
| Declaração de horário `app/chamados/services/declaracao_texto.py:419`, `app/chamados/services/declaracao_texto.py:693` | Mesmo core | Resposta passa pelo mesmo escritor e sua decisão/log; notas em `app/chamados/services/declaracao_texto.py:423`. |
| Upload foto `app/api/views.py:856`, `app/api/views_core.py:840` → `app/api/foto_recebida.py:48` | `app/chamados/services/disputa_emissao.py:1652` | Texto constante não vazio em `app/api/foto_recebida.py:25`; grava par em `:1652–1655`; decisão `resposta` com motivo automático em `:1656`; sem log próprio nesse helper. |
| Cura limbo `app/chamados/management/commands/curar_perguntas_em_limbo.py:67` | `app/chamados/services/disputa_emissao.py:1674` | Exige texto existente e timestamp nulo em `:1670`; salva só timestamp em `:1676`; decisão `resposta` em `:1677`; log `carimbar_resposta_pela_trilha`/`PerguntaDisputa` em `:1679`. |

Detalhes relevantes ao inventário do agente principal:

- `responder_automaticamente` não valida se seu argumento `texto` é vazio; o único chamador executável encontrado passa a constante não vazia da foto. `app/chamados/services/disputa_emissao.py:1647`, `app/chamados/services/disputa_emissao.py:1652`, `app/api/foto_recebida.py:25`, `app/api/foto_recebida.py:48`.
- O core limpa validação velha junto à resposta (`validada_em`, `validada_por`) em `app/chamados/services/disputa_emissao.py:1471`, `app/chamados/services/disputa_emissao.py:1480`; limpa carimbo sem lastro através de `reabrir_por_resposta` em `app/chamados/services/disputa_emissao.py:1485`; grava log adicional `validacao_desfeita_por_resposta` quando aplicável em `app/chamados/services/disputa_emissao.py:1520`.
- `PerguntaDisputa.carimbar` só atribui `respondida_em` se `resposta` foi pedida e o texto existente não é vazio, em `app/chamados/models.py:3131`. Não copia texto. Atribui timestamp em `:3133`; se `resposta='validar'`, atribui `validada_em`/`validada_por` em `:3135`; sempre escreve materialização/via em `:3130`; pode lavrar `veredito_lavrado`, `julgado_em`, `janela_expira_em` em `:3150`; salva em `:3153`. Não grava decisão nem LogAuditoria próprios nesse método.
- Chamadores que pedem resposta ao carimbo: absorção de ausência em `app/chamados/services/materializacao.py:567` (`validar`, decisão `absorcao` em `:569`); materialização em `app/chamados/services/materializacao.py:1283` (`validar`, lavra); resolução por chamado em `app/chamados/services/disputa_emissao.py:1986` (`validar`, lavra); fecho sem credor em `app/chamados/services/disputa_emissao.py:2060` (`responder`, log `fecho_sem_credor` em `:2063`).
- Nascimento genérico aceita `**campos`, insere e registra as assinaturas recebidas, em `app/chamados/models.py:3015`, `app/chamados/models.py:3018`, `app/chamados/models.py:3020`. É um caminho potencial de entrada de `respondida_em`; chamadas concretas com esse campo precisam ser verificadas pelo inventário principal.
- O método geral `PerguntaDisputa.save` apenas deriva identidade na inserção e chama `super().save` dentro de transação; não fabrica resposta/timestamp, em `app/chamados/models.py:3414`, `app/chamados/models.py:3421`, `app/chamados/models.py:3424`.
- O próprio código prevê projeção antiga sem linha de decisão: `MOTIVO_PROJECAO_SEM_LINHA`, em `app/chamados/models.py:3912`; descrição em `app/chamados/models.py:3918`; recuperação de resposta em `app/chamados/models.py:3932`. Portanto, ausência de decisões não exclui escritores históricos por si só.
- Comentário que reconhece carimbo antigo sem texto em `app/chamados/services/disputa_emissao.py:2053` é evidência do histórico descrito no código; não comprova que esse escritor produziu qualquer PK da sonda. [não verificado]

## 12/11 14:52 · sessao 939b40b206eb · - · - · origem=Ronald (tmux)

Leitores que decidem usando `respondida_em`/`resposta_colab`:

| Leitor | Decisão |
|---|---|
| `app/chamados/juizes.py:1510` e `:1515` | `texto_vazio` usa `(t or '').strip()`; `respondida_sem_texto` exige carimbo não nulo e texto vazio. |
| `app/chamados/juizes.py:1375` e `:1407` | Projeção temporal corta resposta futura; vitalidade dá precedência à resposta somente com carimbo **e texto**. |
| `app/chamados/models.py:3349` | Espelho SQL `q_vivas` considera resposta com carimbo e texto diferente de `''`/NULL; a decisão final continua no juiz. |
| `app/chamados/models.py:3042` | `estado`: validada/materializada prevalecem; nos demais ramos o carimbo sozinho distingue PENDENTE/RESPONDIDA (`:3046`, `:3049`). |
| `app/chamados/models.py:3497` | Propriedade `respondida` é somente `respondida_em is not None`. |
| `app/chamados/models.py:1843` | `respostas_sem_veredito` inclui sem validação/materialização quando existe carimbo **ou** texto, respeitando tranca. |
| `app/chamados/models.py:1854` | `pode_fechar` exige ausência de respostas sem veredito e de fio vivo. |
| `app/chamados/services/pergunta_no_app.py:69` | **App:** viva com carimbo entra em `historico`; apenas carimbo nulo + disputa aberta entra em `pendentes` (`:71-75`). |
| `app/chamados/services/pergunta_no_app.py:90` | Badge/contador usam tamanho de `pendentes` (`:91`, `:98-103`). |
| `app/api/views.py:2530` | API lê essa consulta; `aguarda_admin` depende de estado RESPONDIDA (`:2543-2546`); `respondidas` contém histórico com carimbo (`:2570`). |
| `app/api/views.py:1172` | Oferece solicitar correção quando todas perguntas possuem carimbo e reaberturas <2. |
| `app/chamados/services/fio.py:340` | Todas respondidas/possibilidade de correção usam carimbo sozinho. |
| `app/chamados/services/fio.py:660` e `app/chamados/services/acoes_disputa.py:130` | Fechamento manual barra se alguma viva tem carimbo nulo. |
| `app/chamados/services/validacao.py:393` | Validação exige carimbo; sem ele retorna “Aguardando resposta do colab”. |
| `app/chamados/services/validacao.py:576` | Recusa sem carimbo retorna “já pendente”. |
| `app/chamados/services/materializacao.py:1375` | Seleção para materializar: respondida, validada e não materializada. |
| `app/chamados/services/materializacao.py:835` | Plantio recusa texto vazio como “sem resposta do colaborador”, chamando porta de recusa (`:842`). |
| `app/chamados/juizes.py:1663` | Perguntas corrigíveis começam pelas carimbadas, depois juiz de lastro/vitalidade. |
| `app/chamados/juizes.py:1670` | Fila global de validação exige carimbo e exclui texto `''`, depois verifica viva. |
| `app/chamados/juizes.py:1699` | `declarou_hora` exige texto e exclui escapes maiúsculos/`[descartado`. |
| `app/chamados/juizes.py:1846`, `:1950`, `:1972` | Texto determina delta, classe e plausibilidade da resposta. |
| `app/chamados/juizes.py:2020` | Irmãs respondidas não consumidas exigem carimbo, ausência de validação/materialização e texto (`:2024`). |
| `app/chamados/juizes.py:1651` | Porta alcançável recusa `respondida_sem_texto`. |
| `app/chamados/services/vigia_fio.py:673` | Viva respondida sem texto recebe causa `b`; `:675` classifica carimbada sob âncora terminal como causa `c`. |
| `app/chamados/services/vigia_fio.py:783` | Contador seleciona pelo instante do carimbo e conta textos vazios (`:786`). |
| `app/chamados/views_cobrar.py:169` | Preview de validação seleciona vivas carimbadas; sem elas busca perguntas com carimbo nulo (`:175`). |
| `app/chamados/views.py:1416` | Reabrir/recobrar conta sem carimbo e vivas; se não há corrigíveis, recobra (`:1424-1425`). |
| `app/chamados/services/acoes_disputa.py:95` | Mensagem “questionário já pendente” conta carimbo nulo. |
| `app/chamados/services/cobranca.py:229` | Silenciosos: vivas sem carimbo em disputa aberta. |
| `app/chamados/services/furo_sem_canal.py:80` | Pergunta aberta para diagnóstico significa carimbo nulo. |
| `app/chamados/services/becos.py:29` | Pergunta visível para classificar beco exige carimbo nulo e destino não resolvido. |
| `app/chamados/services/fio.py:1233`, `:1244` | Dedup prioriza texto; protege mérito do admin com texto. |
| `app/chamados/reconciliador.py:365`, `:1083` | Texto bloqueia cura automática sem turno e resolução automática do marco. |
| `app/chamados/services/disputa_emissao.py:1821` | Reabrir credor recusa pergunta carimbada; `:1839` reconhece limbo por texto sem carimbo. |
| `app/chamados/services/disputa_emissao.py:1979` | Protege resposta com texto/carimbo sem materialização de fato contra vassoura automática. |
| `app/ponto/supra_juiz.py:545`, `:548` | Executor de cobrança sem furo recusa retratação automática se há carimbo em qualquer pergunta ligada. |
| `app/chamados/services/verbos.py:335`, `:342` | Métrica de resposta pós-cobrança compara carimbo com instante da cobrança. |
| `app/colaboradores/services/ficha.py:169` | Ficha usa respostas sem veredito; classifica sem resposta quando não está entre essas e não há texto (`:171`). |
| `app/folha/previa.py:142` | Material disponível para o dia distingue “resposta do colaborador” por texto, depois pergunta viva. |

Templates também tomam decisões pelo carimbo: `app/templates/chamados/partials/_pergunta_card.html:24` mostra “aguardando validação”; `:29` mostra resposta e controles; `app/templates/chamados/partials/_card_atendimento.html:94` mostra “você respondeu”, `:155` escolhe reenviar/enviar; `app/templates/chamados/partials/modal_fio.html:217` exibe bloco de resposta.

Caminhos de entrada e apagamento:

- Core web/API recusa vazio após strip em `app/chamados/services/disputa_emissao.py:1432-1434`, copia texto e carimbo em `:1467-1469`. API ainda recusa vazio na casca em `app/api/views.py:2614-2617`; web chama core em `app/chamados/views.py:1283`.
- `responder_automaticamente` protege texto existente em `app/chamados/services/disputa_emissao.py:1647`, mas **não valida texto recebido como não vazio**: atribui ambos em `:1652-1653`, salva e registra decisão `resposta` em `:1655-1657`. Chamador histórico das 143 [não verificado].
- `carimbar_resposta_pela_trilha` só carimba quando já existe texto e não há carimbo: `app/chamados/services/disputa_emissao.py:1670`; registra decisão `resposta` e evento homônimo em `:1677-1681`.
- Apagador explícito encontrado: `reabrir_para_correcao` zera **texto e carimbo juntos**, além da validação, em `app/chamados/services/disputa_emissao.py:1593`; registra foto `reabertura`/`recusa` antes em `:1615-1628`, aplica update em `:1630-1632`. Portanto esse caminho atual não preserva carimbo deixando texto vazio.
- Recusa do admin chama essa porta com ato `recusa` em `app/chamados/services/validacao.py:593-594`, registra evento e `LogAuditoria` `recusar_pergunta/PerguntaDisputa` em `:602-610`.

Aplicação às 143: pelo código da consulta, perguntas vivas com carimbo não nulo seguem para histórico, mesmo com texto vazio (`app/chamados/services/pergunta_no_app.py:69-75`); a API as inclui em `respondidas` e `historico` (`app/api/views.py:2559-2571`). O anexo não contém a medição efetiva do app de cada colaborador, portanto resultado real por colaborador e garantia “app não muda” [não verificado].

## 12/11 14:52 · sessao dd082d0c62f2 · - · - · origem=Ronald (tmux)

Inventário dos escritores de `respondida_em` encontrados no código executável:

| Escritor | Persistência e campos juntos | Trilha |
|---|---|---|
| `app/chamados/services/disputa_emissao.py:1468`, `responder_pergunta_disputa_core` | `resposta_colab` + `respondida_em`; limpa validação anterior e eventual carimbo sem lastro; salva em `:1508`. Texto vazio é recusado em `:1432–1434`. | `DecisaoPergunta.RESPOSTA` com instante/texto/autor em `:1511`; `resp_pergunta_gravada / PerguntaDisputa` em `:1529`; eventual `validacao_desfeita_por_resposta` em `:1520`; followup em `:1555`. |
| `app/chamados/services/disputa_emissao.py:1653`, `responder_automaticamente` | `resposta_colab=texto` + `respondida_em=instante`, save em `:1655`. Não valida que o argumento `texto` seja não vazio; apenas preserva texto que já existia em `:1647`. | `DecisaoPergunta.RESPOSTA` em `:1656`, motivo “resposta automatica…”. |
| `app/chamados/services/disputa_emissao.py:1674`, `carimbar_resposta_pela_trilha` | Salva somente `respondida_em` em `:1676`; exige texto já presente em `:1670`. | `DecisaoPergunta.RESPOSTA` em `:1677`; `carimbar_resposta_pela_trilha / PerguntaDisputa` em `:1679`. |
| `app/chamados/models.py:3133`, `PerguntaDisputa.carimbar` | Só escreve se `resposta` foi solicitada, texto não é vazio e carimbo estava nulo (`:3131–3134`). Sempre salva materialização/autor/via (`:3129`); com `resposta='validar'`, acrescenta validação/autor quando nulos (`:3135–3138`); opcionalmente veredito/julgado/janela (`:3150–3153`). | A porta não registra decisão nem log de sucesso em seu corpo (`:3104–3158`); depende do chamador. |
| `app/chamados/models.py:1567`, `ressuscitar_perguntas` | Zera `respondida_em`, validação/autor e os três campos do carimbo por `descarimbar_em_lote`; update real em `:3265–3267`. Só seleciona resposta null/vazia em `:1487`. | Preserva decisões legadas em `:1564`; log `perguntas_ressuscitadas / ChamadoColaborador` em `:1590`, com pks em `:1593–1594`. |
| `app/chamados/services/disputa_emissao.py:1593`, `reabrir_para_correcao` | Zera conjuntamente `respondida_em`, validação/autor e texto; update em `:1632`, ou `descarimbar_em_lote` em `:1630` quando desfaz absorção. | Decisão com foto anterior em `:1615–1628`, ato padrão `reabertura`, podendo ser `recusa` pelo parâmetro (`:1562`, `:1597–1598`). |

Chamadores da porta `carimbar` que podem escrever `respondida_em`:

| Chamador | Assinatura |
|---|---|
| `app/chamados/services/materializacao.py:567`, `_materializar_ausencia_de_resposta` | `resposta='validar'`, via declaração ou ausência (`:555`); `DecisaoPergunta.ABSORCAO` com escape/ausência/texto em `:569–572`. |
| `app/chamados/services/materializacao.py:1283`, `_materializar_uma` | `resposta='validar'`, `lavrar_veredito=True`; via declaração ou via do fato (`:1267–1284`); não registra decisão/log de sucesso nesse trecho. |
| `app/chamados/services/disputa_emissao.py:1986`, resolução pelo encerramento | `resposta='validar'`, `lavrar_veredito=True`; via do fato. Não registra decisão/log por pergunta nesse ramo (`:1986–1990`). |
| `app/chamados/services/disputa_emissao.py:2060`, ramo sem credor | `resposta='responder'`; log `fecho_sem_credor / PerguntaDisputa` em `:2063`, antes/depois com `respondida_em`, materialização, via e presença de texto em `:2051–2052`, `:2068–2070`. |

Outras atribuições são somente em memória: cópia usada para julgar ressuscitação em `app/chamados/models.py:1509`; rollback da porta em `app/chamados/models.py:3164`.

API e command delegam: foto fornece texto fixo não vazio em `app/api/foto_recebida.py:25`, chama automático em `:48`; cura do limbo chama a porta pela trilha em `app/chamados/management/commands/curar_perguntas_em_limbo.py:67` e reabertura em `:69`. As migrations de dados consultadas escrevem identidade em `app/chamados/migrations/0051_decisao_pergunta.py:25–27` e verificam colisões em `0053_pergunta_unica_por_familia.py:36–46`; `respondida_em` aparece como criação de campo em `0014_perguntadisputa.py:25`.

Pontos decisivos para o casamento:

- `carimbo_desfeito_por_lastro` identifica o **desfazedor da materialização**, cujo log é emitido em `app/chamados/services/relavra_via.py:248`; a foto anterior só inclui via/materialização em `:244–245`, e a porta conserva resposta/validação conforme `:206–207`. Não identifica o escritor anterior de `respondida_em`.
- Via anterior `sem_turno_aberto` tampouco identifica escritor da resposta: `app/chamados/reconciliador.py:406` chama `carimbar` sem `resposta=...`; comentário documenta a retirada do antigo bloco em `:402–403`.
- Via `chamado_encerrado` não é assinatura exclusiva: pode sair de `_materializar_uma` em `app/chamados/services/materializacao.py:1274`, do ramo sem credor em `app/chamados/services/disputa_emissao.py:2059`, além da derivação por fato em `:1986`.
- Carimbos históricos sem decisões são previstos por `DecisaoPergunta.linhas_da_projecao_sem_linha`, que só projeta resposta atual/instante e motivo genérico em `app/chamados/models.py:3912–3935`. Essa projeção não recupera o escritor histórico.

Caminho do vazio:

- O core rejeita envio vazio (`disputa_emissao.py:1433`); a porta de materialização exige texto (`models.py:3131`); a cura pela trilha exige texto (`disputa_emissao.py:1670`).
- O automático aceita argumento vazio em sua assinatura/corpo (`disputa_emissao.py:1638–1656`), mas seu chamador encontrado entrega a constante não vazia (`api/foto_recebida.py:25`, `:48`).
- O apagador encontrado limpa texto **e** `respondida_em` juntos (`disputa_emissao.py:1593`), portanto não explica sozinho o par vazio/carimbado.
- `data_conferida_em` tem escritor separado, `app/chamados/services/validacao.py:664`, com save apenas de conferência/autor em `:666`.
- Atribuição histórica de cada uma das 143 a um desses caminhos: [não verificado].

## 12/11 14:52 · sessao 4b2f9e7f47af · - · - · origem=Ronald (tmux)

Conferência não-semente:

- **FECHADOS contratualmente:** G1 antes do primeiro comentário e guarda sem unicidade global de `<!--`; v6:6–7. G2/lápide, G3/`GERADO:FIM`, manual em 2371, autoria por zonas e cópia byte a byte são mantidos pela incorporação do v5; v6:5. A cópia continua com leitura/recomposição textual, a substituir na construção; `bin/gerar_backlog.py:111`, `:127`, `:138–140`.
- **FECHADA a omissão semântica do censo:** v6:12 declara expressamente que a coluna e o tachado deixam de excluir IDs da fonte; corresponde ao comportamento atual em `bin/hook_stop_fila1.py:168`, `:180`, `:250–252` e `bin/disjuntor.py:1634`.
- **FECHADOS segundo classificador e TRAVA/censo:** `_pertence_fila1` permanece; v6:4,10. O matcher atual está em `bin/disjuntor.py:1688–1695`. A v6:14 distingue APROVADO de autorização de construção; o código aprova o brief em `bin/disjuntor.py:1721–1727`.
- **Referência incorreta não bloqueia:** v6:8 menciona OBRAS:FIM em 2051 no vivo; nesta cópia está em `app/docs/BACKLOG.md:2042`. A própria v6:8 exige âncora por conteúdo; não há escolha contratual.
- **Referência estreita de selo não bloqueia:** v6:15 substitui `test_hook_estado_e_a_coluna.sh:53–59`, mas os casos completos abrangem `:51–63`, e o contador continua em `:80–108`. A obrigação “a coluna não move o topo; só o .ids move” na v6:15 permite migrar os pontos coerentemente sem decisão nova.

**Possível bloqueante por violação de aval já documentado:** APP continua após C20-O168 na cabeça; v6:26,30. `app/docs/BACKLOG.md:1916` registra o aval “depois da TRIAGEM, antes de O168/LASTRO”. A v6:30 admite a discrepância e reduz a frase a LASTRO alegando abertura anterior de O168. O anexo não fornece autorização nova de Ronald para esse corte; autorização: **[nao verificado]**. Se “aval literal” inclui esse aval citado pelo próprio contrato, é bloqueante; se a leitura pedida restringe o aval literal exclusivamente ao aval de origem reproduzido no pedido, deixá-lo como achado v5 ainda aberto.

O contrato v5 completo não foi encontrado na árvore pesquisada; as obrigações incorporadas por “como no v5” só puderam ser cotejadas com a leitura v5 fornecida, não com o documento integral: **[nao verificado]**.

## 12/11 14:51 · sessao 498d1af4a5a1 · - · - · origem=Ronald (tmux)

Mapa dos escritores/leitores da cópia:

| Papel hoje | Evidência | Depois da v6 |
|---|---|---|
| Sessão escreve marcador manualmente; gerador conserva e reescreve fisicamente | `bin/gerar_backlog.py:113`, `:119`, `:127`, `:138–140`; marcador em `app/docs/BACKLOG.md:8` | Sessão escreve `ORDEM-VIVA.ids`; gerador publica projeção. (`contrato_item3_v6.txt`, cláusulas 0 e 2; numeração física [nao verificado]) |
| Hook lê marcador, confere identidade no bloco e veta pela coluna | `bin/hook_stop_fila1.py:209`, `:213–227` | Adapter deve consultar `ordem_viva.em_curso`; identidade pode continuar consultando BACKLOG inteiro. (`contrato_item3_v6.txt`, cláusulas 0–1; numeração física [nao verificado]) |
| Hook calcula andáveis pela tabela e estado | `bin/hook_stop_fila1.py:231–252`, `:267`, `:270–273` | Deve consultar fonte estruturada e TRAVA; não basta trocar só `_item_da_ordem_viva`. (`contrato_item3_v6.txt`, cláusulas 0 e 4; numeração física [nao verificado]) |
| Hook calcula parados/travas pela coluna | `bin/hook_stop_fila1.py:323–345`, `:349–382`; uso `_ids_que_nao_andam` em `:1298` | Também precisa migrar; `travas_da_fila` alimenta handoff. (`bin/handoff_sessao.sh:294`) |
| Disjuntor lê lista de andáveis do hook | `bin/disjuntor.py:1623–1634`; consumidores `:1707`, `:2306–2307` | Migração expressa para `ordem_viva.ids_ordem()`. (`contrato_item3_v6.txt`, cláusula 4; numeração física [nao verificado]) |
| Handoff lê topo e próximo pelo hook | `bin/handoff_sessao.sh:64`, `:71`, `:290`, `:310`; lista travada `:294` | Pode migrar mantendo adapters do hook; texto antigo “1º ABERTO do bloco OBRAS” precisa correção. (`bin/handoff_sessao.sh:314–316`; cláusulas 0–1 do contrato v6, numeração física [nao verificado]) |
| Interrompidas lê topo pelo hook | `bin/interrompidas_estado.py:54`, `:93`, `:121–124` | Migra por adapter; varredura de interrupções pode continuar lendo dados documentais. (`bin/interrompidas_estado.py:94–118`) |
| ESTADO publica topo via interrompidas | `bin/gerar_estado.py:236–243` | Migração transitiva pelo mesmo adapter; rótulo “sem marcador” precisa correção. (`bin/gerar_estado.py:243`) |
| `backlog_estado` escreve coluna e consulta próximo/parados via hook | `bin/backlog_estado.py:56–63` | Coluna deixa de alterar ordem; chamadas precisam usar adapters migrados; essa escrita não passa a ser escrita do `.ids`. (`contrato_item3_v6.txt`, cláusulas 0 e 4; numeração física [nao verificado]) |
| Fabricante lê IDs, ordem física e estado do BACKLOG | `bin/fabricante_alvo.py:238`, `:251–258`, `:273–283` | Deve excluir IDs da fila 1 por igualdade via fonte estruturada, conforme menção da cláusula 1; mecanismo detalhado herdado do v5 [nao verificado]. |
| `gerar_backlog_topo` produz índice O-número por ordem numérica | `bin/gerar_backlog_topo.py:19`, `:26`, `:33–40` | É índice de obras O-número abertas, não leitura da ORDEM VIVA; não há motivo demonstrado para obrigá-lo a virar a ordem viva. (`bin/gerar_backlog_topo.py:2`, `:33–38`) |
| `relato.sh` publica BACKLOG e índice | `bin/relato.sh:35`, `:116` | A projeção G2 publicada no BACKLOG permite alcançar humanos sem publicar o `.ids` separadamente. (`contrato_item3_v6.txt`, cláusula 2; numeração física [nao verificado]) |

Consumidor adicional: `bin/alarme_sessao_ociosa.py::_fila` calcula quantidade “fila 1” diretamente de `## OBRAS`, removendo linhas por `☑`, `FEITO` ou `FECHAD` em toda a linha; não usa hook nessa leitura. (`bin/alarme_sessao_ociosa.py:89–116`) O resultado desarma ou dispara o alarme conforme fila vazia/não vazia. (`bin/alarme_sessao_ociosa.py:40–45`) A v6 fornecida não o nomeia expressamente; ele não lê ordem/topo, portanto tratá-lo como bloqueante depende de ampliar “ordem única” a todo contador de fila, o que os anexos não demonstram. [nao verificado]

Nenhum outro leitor direto do marcador foi identificado entre scripts Python/shell além do hook; o gerador o captura para preservar, e consumidores de topo usam o hook. (`bin/hook_stop_fila1.py:209`; `bin/gerar_backlog.py:119`; `bin/handoff_sessao.sh:64`; `bin/interrompidas_estado.py:93`) A adaptação completa ainda não existe nesta cópia. (`bin/hook_stop_fila1.py:209`, `:250`; `bin/disjuntor.py:1634`)

## 12/11 14:51 · sessao 0aaaa0d2c6dd · - · - · origem=Ronald (tmux)

Censo dos campos e da semântica, somente leitura:

| Campo | Escritores | Leitores e significado |
|---|---|---|
| `PeriodoFechado.ativo` | Default `True`, `app/ponto/models.py:933`; criação `trancar_periodo`, `app/ponto/services/fechamento.py:1408-1410`; retranca `True`, `:1413`; reabertura `False`, `:1437`. | Não encontrei predicado produtivo que leia `ativo`; o contrato declara expressamente “`reaberto_em` é a verdade; `ativo` é só documento”, `app/ponto/services/fechamento.py:1466`. |
| `fechado_por` | Criação por `get_or_create(defaults=...)`, `app/ponto/services/fechamento.py:1410`; retranca `:1414`; FK definida em `app/ponto/models.py:922`. | Não encontrei consumidor produtivo do valor, além da persistência/FK; nenhum template lê esse campo. [não verificado quanto a consumidores externos à cópia] |
| `fechado_em` | `auto_now_add=True`, `app/ponto/models.py:926`; retranca explícita em `app/ponto/services/fechamento.py:1415`. | `chamados/juizes.py::dias_trancados`, `:1070`: marco temporal para procurar toque humano posterior; `chamados/services/competencia_trancada.py::medir`, `:105`: resgate posterior permanece fora do encerramento. |
| `reaberto_em` | Default `NULL`, `app/ponto/models.py:931`; `trancar_periodo` limpa em `app/ponto/services/fechamento.py:1417`; `reabrir_periodo` escreve agora em `:1439`. | `trancar_periodo`, `:1411-1412`, distingue ato novo/retranca; K7 `:1470` e K4 `:1480` definem viva por `NULL`; consultas diretas `chamados/juizes.py::dia_em_competencia_trancada`, `:2043`, e `chamados/services/competencia_trancada.py::medir_frota`, `:312`, usam a mesma definição. |
| `FechamentoMensal.status` | Default `aberto`, `app/ponto/models.py:410`; `recalcular_fechamento_mes` cria aberto em `app/ponto/services/fechamento.py:898-900`; `aprovar_lote` aprovado `:1329-1332`; `aprovar` aprovado `:1337`; `reabrir_lote` aberto `:1346`; `reabrir` aberto `:1351`; `reabrir_periodo` somente aprovados→aberto `:1451-1452`. `restaurar_fechamento` tem capacidade de escrever `status`: aceita todos os campos concretos e não o exclui, `:1374-1393`, apesar da docstring `:1370`; chamada real restaurando status [não verificado]. | As leituras detalhadas abaixo distinguem carimbo do colaborador, tranca da empresa e entrega do TXT. Não existe nesses escritores atribuição literal `status='exportado'`; a opção permanece no modelo `app/ponto/models.py:402`. |

`trancar_periodo` não escreve `FechamentoMensal.status`: só cria/retranca PF e dispara a fila, `app/ponto/services/fechamento.py:1400-1429`. O único caminho normal aqui associa aprovação e tranca por sequência na view, `app/ponto/views.py:1362-1367`; essa associação não é pré-condição de `trancar_periodo`.

Leitores produtivos de `FechamentoMensal.status` — predicado, apresentação ou serialização explícita:

| Sítio | O que lê/faz |
|---|---|
| `app/ponto/models.py::FechamentoMensal.save`, `:497-498` | Consulta status original e impede `save()` aprovado→aprovado; independe PF. |
| `app/ponto/models.py::FechamentoMensal.__str__`, `:506` | Imprime status. |
| `app/ponto/services/fechamento.py::recalcular_fechamento_mes`, `:890-895`, `:1137` | Modo leitura copia todos os campos concretos da linha, incluindo status; aberto salva pelo model, demais persistem números por `update()` preservando carimbo `:1146-1160`. |
| `app/ponto/services/fechamento.py::reabrir_periodo`, `:1451` | Seleciona apenas aprovados para retornar a aberto; PF viva com FM aberto produz zero nesse update. |
| `app/ponto/services/fechamento.py::marcos_da_competencia`, `:1533-1534` | `aprovada=True` se existir algum aprovado/exportado da empresa; separado de `trancada`, `:1538`. |
| `app/ponto/janelas.py::competencia_fechada`, `:86-88` | Ajuste por colaborador bloqueado somente se FM aprovado/exportado. FM aberto + PF viva não bloqueia aqui. |
| `app/ponto/views.py::fechamento_mensal`, `:1157`, `:1169` | Conta aprovados; seleciona abertos para decidir disponibilidade de aprovação pelas travas. |
| `app/ponto/views.py::aprovar_fechamento`, `:1270`, `:1362` | Universo/carimbo somente aberto. |
| `app/ponto/views.py::reavaliar_vazios`, `:1412` | Reabre apenas aprovados com trava conta vazia; tranca não veta, `:1400`. |
| `app/ponto/views.py::reabrir_colaborador`, `:1447` | Recusa se não aprovado; tranca não veta, `:1444-1446`. |
| `app/folha/views.py::gerar_dominio_txt`, `:148-163`, `:246-252` | Guarda de existência aberto/aprovado e carimbo dos abertos elegíveis; não consulta tranca. |
| `app/folha/export.py::montar_linhas_export`, `:717-721` | Filtra aberto/aprovado pela constante `STATUS_NO_TXT`, `:687`, tanto injetados quanto ORM. |
| `app/folha/services/ranking_he.py::ranking`, `:152`, `:157` | Retorna status e usa conjunto não vazio para “há número”; o carimbo lavrado depende de `competencia_entregue`, não status, `:77-91`. |
| `app/folha/services/ranking_he.py::apuracao_colab`, `:223`, `:229` | Mesmo uso de status, sobre leitura online. |
| `app/relatorios/views.py::horas_mes`, `:95` | Exporta status no CSV. |
| `app/relatorios/views.py::inconsistencias`, `:160` | Exporta status no CSV. |
| `app/ponto/management/commands/checklist_golive.py::Command.handle`, `:117-122` | Conta abertos da competência corrente e gera aviso. Não consulta PF. |
| `app/ponto/management/commands/projecoes_vencidas.py::Command.handle`, `:65-67` | Imprime status das projeções vencidas. |
| `app/templates/ponto/_fech_linha.html`, `:2`, `:4`, `:40` | `data-status`, checkbox se aberto, selo se aprovado. |
| `app/templates/ponto/partials/painel_fechamento.html`, `:10-11`, `:80`, `:107` | Selo aprovado; aprovação individual somente não aprovado e não trancado. |
| `app/templates/core/dashboard_supervisor.html:123` | Selo aprovado. |
| `app/templates/relatorios/horas_mes.html:75`; `app/templates/relatorios/inconsistencias.html:50` | Selo aprovado. |

Leitores por fotografia/introspecção que também leem `FM.status`:

- `app/ponto/portas/regen_exportada.py::hash_do_gravado`, `:71-73`, campos de `_campos`, `:56-59`, exclusões `:37-38`: inclui status.
- `app/ponto/management/commands/carimbo_gravado.py::foto`, `:53-55`, campos `:34-36`, exclusões `:29-30`: inclui status.
- `app/ponto/management/commands/lavrar_dias_pagos.py::Command._hash_dos_fechamentos`, `:68-77`: inclui status.
- `app/ponto/management/commands/aplicar_09_he_intra.py::Command.handle._foto`, `:125-129`, `CAMPOS` inclui status `:49`.
- `app/chamados/services/foto_e4b.py::foto_do_colaborador.guardar`, `:247-252`, chamado para FM em `:290`; online FM em `:328-332`; ambos incluem status.
- `bin/relavrar_o118_col221.py::_hash`, `:26`; fotografia de reversão no módulo `:30-37`.
- `bin/relavrar_o119_col221.py::_hash`, `:25`; fotografia no módulo `:29-36`.
- `bin/relavrar_o120_col221_09.py::_hash`, `:29`; fotografia no módulo `:62-66`.
- `bin/r5_idempotencia_frota.py`, fotografia FM `:72-77`, status explícito `:75`.

Não contar como leitores de status os snapshots/restores O263 e `snapshot/restore_relavratura_10_2026`: usam `CAMPOS` de `app/ponto/management/commands/aplicar_09_corte_b.py:67-74`, que não contém status. `bin/aplicar_o263_comp10.py:28-31`, `bin/diff_o119_frota.py:27-30` e `bin/diff_o124_frota.py:31-34` limitam-se a tipos numéricos, portanto também não leem status.

Semântica/decisões:

1. `app/ponto/services/fechamento.py:1456-1457` descreve sequência aprovada→exportada→trancada→paga; `marcos_da_competencia` calcula separadamente e só acusa relações ausentes, `:1521-1525`, `:1542`. É contador, não requisito de escrita. Com PF nova e nenhum aprovado/exportado, aprovada fica falsa; exportada/paga para os dez pares [não verificado].
2. A lei já responde recalcular/exportar com tranca: `app/core/juizes.py:525-541`, especialmente O141 em `:533-534`, diz que TXT e recálculo não consultam tranca. `CLAUDE.md:418-422` permite correção provada em qualquer competência, sem trava/aviso por exportada/trancada/paga.
3. A fronteira `F6`/`K4` é explicitamente separada: `app/core/juizes.py:76-86`. F6 usa aprovado/exportado por colaborador; regularização externa usa K4, e a fusão é decisão aberta `:83-86`. Não chamar isso de suposição acidental de equivalência.
4. A aprovação não é pré-requisito do TXT: `app/folha/export.py:851-855`; aberto é aceito por `:687`. Manual contradiz: `app/templates/core/manual.html:185` ainda exige Aprovado, divergência documental já respondida pela lei 31/07.
5. Há divergência concreta de casca com portas: `app/templates/ponto/fechamento.html:208` esconde “Fechar período” se PF viva; `app/templates/ponto/partials/painel_fechamento.html:80` esconde aprovar individual se FM aberto + PF viva. Portas `app/ponto/views.py:1270-1367` e `:1470-1515` não vetam tranca. Corrigir isso tem lastro na lei `CLAUDE.md:422`; quais colaboradores concretos precisam desses atos não consta do anexo [não verificado].
6. `app/templates/ponto/_fech_linha.html:42` esconde reabrir colaborador por tranca, apesar da porta permitir, `app/ponto/views.py:1444-1452`. Para cenário sem aprovado este ramo não é alcançado; continua divergência sob coexistência PF viva + aprovado.
7. Tranca não transforma a guarda `F6` em trava: `app/ponto/registro_batida.py:132-137` usa F6 para escrita institucional, e `app/ponto/services/flip_batida.py:21-24` também. Com FM aberto ela permanece liberada por esse critério, `app/ponto/janelas.py:87-89`; outros vetos continuam próprios dos caminhos.
8. `fechado_em` novo torna o corte de toque “depois da nova tranca”: a comparação está em `app/chamados/juizes.py:1070-1074` e `app/chamados/services/competencia_trancada.py:105`. Quais toques pré-existem nos dez pares [não verificado].

Nenhum arquivo foi escrito por minhas ferramentas; utilizei somente `rg`, `nl`, `sed` para leitura.

## 12/11 14:50 · sessao e6e03394338d · - · - · origem=Ronald (tmux)

Censo produtivo completo dos leitores diretos de K7/K4 (o caminho exato foi conferido por AST, incluindo aliases/imports locais; testes de chamadas diretas ficam ao fim): [nao verificado]

| Porta | Leitor, função e chamada | Mudança sob tranca viva | Dependência de FM |
|---|---|---|---|
| K7 | `app/chamados/juizes.py:1066`, `dias_trancados` (import local `:1029`) | Calcula a tranca por empresa/competência do dia; guarda `fechado_em` para descontar toque humano posterior. `:1062-1079`. | Nenhuma. |
| K7 | `app/chamados/services/competencia_trancada.py:83`, `medir` (import local `:76`) | Sem tranca retorna vazio; com tranca mede chamados/perguntas/disputas da competência, preservando cadastro/rescisão, resgatados posteriores e perguntas vivas fora. `:84-159`. Gancho de `trancar_periodo:1428` usa essa medida e executa encerramento no ato ou processo. `competencia_trancada.py:277-304`. | Nenhuma; não lê FM. |
| K7 | `app/ponto/views.py:1027`, `painel_fechamento`, alias de módulo `_fech_svc` (`:969`) | Alimenta `periodo_fechado`; o partial deixa de mostrar aprovação para FM não aprovado. `app/templates/ponto/partials/painel_fechamento.html:80`. | Divergência visível exige FM existente, leitura disponível e `status != aprovado`; a tranca independe dele. |
| K7 | `app/ponto/views.py:1126`, `fechamento_mensal`, alias local `_periodo_trancado` (`:1125`) | Oculta “Fechar período”; mostra “Período fechado” e Reabrir. `app/templates/ponto/fechamento.html:208`, `:221-232`. | Nenhuma para o selo/reabrir; botão já requer `pode_aprovar`. |
| K7 | `app/ponto/views.py:1540`, `reabrir_periodo`, alias de módulo `_fech` (`:1539`) | Viva permite reabertura, ausente gera 404. `:1541-1543`. Serviço devolve só FM aprovado para aberto. `app/ponto/services/fechamento.py:1449-1452`. | Acesso à reabertura não depende de aprovado; com FM não aprovado, nenhum desses carimbos muda. |
| K4 | `app/chamados/models.py:1045`, `ChamadoColaborador._dia_em_competencia_trancada` (import local `:1041`) | Responde trancada para competência do chamado, salvo módulos fora da tranca. `:1042-1045`. Consumidor `_renascer_por_premissa_viva` recusa renascimento automático em `:1010-1011`. | Nenhuma. |
| K4 | `app/escala/services/furos_diarios.py:77`, `apurar`, alias local `_trancada` (`:75`) | Deixa de incluir a janela **anterior** para regravar/apagar FuroDiario; a corrente permanece. `:68-78`, `:97-112`. | Nenhuma. Para os dez pares históricos e data atual 12/11/2026, eles não são a janela anterior; não há efeito desse cron hoje, pela geometria `app/ponto/janelas.py:24-33`, `:66-72`. |
| K4 | `app/ponto/services/fechamento.py:1538`, `marcos_da_competencia` | `trancada=True`, escolhe marco mais avançado (`:1543`), acusa violação de predecessor conforme flags (`:1542`). | Aprovação é flag separada lida de FM em `:1533-1534`; não é pré-requisito da tranca. |

ORM direto fora de K7/K4:

- `app/chamados/juizes.py:2043`, `dia_em_competencia_trancada`: `filter(empresa_id=..., reaberto_em__isnull=True)`, itera competências e testa janela do dia em `:2045-2047`; fail-open `:2039-2048`. Nenhuma dependência de FM.
- `app/chamados/services/competencia_trancada.py:312`, `medir_frota`: `filter(reaberto_em__isnull=True)`, passa cada par para `medir` em `:313`, agrega contagens em `:314-320`. Nenhuma dependência de FM.
- Leitura/escrita por `get_or_create`, fora do recorte estrito “filter/exists”: `app/ponto/services/fechamento.py:1408`, `trancar_periodo`; lê `reaberto_em` em `:1411-1412` para decidir se há ato e se precisa reativar. Não valida FM.

Leitores transitivos relevantes:

- `app/ponto/services/furos_sem_cobranca.py:85`, `medir`, chama `dia_em_competencia_trancada` e exclui o dia do contador em `:86`; seu universo é só janela corrente (`:60-62`), logo os dez pares históricos não entram hoje.
- `app/ponto/services/cluster_espurio.py:254`, `tratar_legado_encerrado_sem_lastro`, chama `_dia_em_competencia_trancada`; trancada troca ação para relavrar via tranca e marca `PAROU_SEM_PORTA`, sem escrever (`:237-238`, `:258-261`, `:272-273`). Nenhuma dependência FM.
- `app/folha/management/commands/pre_fechamento.py:74`, `Command._marcos_da_anterior`, apenas imprime marco/violações da anterior (`:75-77`).
- `app/ponto/services/fechamento.py:1515`, `empresas_exportadas_no_escopo`, consulta `marcos_da_competencia` mas usa somente `['exportada']`; tranca não altera esse resultado.
- `app/ponto/services/fechamento.py:1555`, `marco_da_competencia`, retorna o marco agregado. Não achei chamador produtivo.
- Não há leitores diretos produtivos de K7/K4/ORM PF em geração/edição de CelulaDia, regeneração ou registro/regularização de batida. A ausência tem proteção explícita para TXT/recalculo: `app/ponto/tests/test_contract_juiz_fechamento.py:117-131`; lei literal registrada em `app/core/juizes.py:532-540`.

Campos de PF, para consolidação com o outro agente:

- `ativo`: default/modelo `app/ponto/models.py:933`; escreve criação `fechamento.py:1410`, retranca `:1413`, reabre `:1437`. Nenhum leitor produtivo de valor encontrado; K7 declara “só documento” em `:1466`.
- `fechado_em`: `auto_now_add` `app/ponto/models.py:926`; retranca escreve `fechamento.py:1415`; leitores `chamados/juizes.py:1070` e `chamados/services/competencia_trancada.py:105` comparam toque/resgate posterior.
- `fechado_por`: campo `app/ponto/models.py:922`; escreve criação `fechamento.py:1410`, retranca `:1414`; nenhum leitor produtivo de valor encontrado.
- `reaberto_em`: campo `app/ponto/models.py:931`; retranca limpa `fechamento.py:1417`; reabre escreve `:1439`. Leitores de valor `:1411-1412`; predicados `:1470`, `:1480`, `chamados/juizes.py:2043`, `competencia_trancada.py:312`.
- A única verdade executável de “viva” nesses leitores é `reaberto_em IS NULL`; nenhum exige `ativo=True` ou FM aprovado. `fechamento.py:1466-1480`.

Decisões candidatas:

1. `DECISAO: app/templates/ponto/fechamento.html:208 | tranca sem aprovação oculta Aprovar/Fechar período embora FM aberto continue carimbável pela porta | lei existente: nenhuma trava/aviso por trancada, CLAUDE.md:418-422; app/core/juizes.py:567-573 separa tranca do ato`.
2. `DECISAO: app/templates/ponto/partials/painel_fechamento.html:80 | tranca viva com FM != aprovado oculta aprovação individual | mesma lei existente, CLAUDE.md:418-422; app/core/juizes.py:567-573`.
3. `DECISAO: app/ponto/services/fechamento.py:1542 | contador pressupõe ordem aprovada→exportada→trancada→paga; tranca de acervo pode ser relatada “trancada sem exportada” | pergunta de lei`. **Não afirme que ocorre nos dez:** a existência de ExportacaoDominio não foi dada; ausência de FM aprovado não basta para “trancada sem exportada”. A função testa predecessor imediato, não trancada→aprovada. `:1533-1542`.
4. O comentário de modelo “batidas read-only” (`app/ponto/models.py:913`) e a redação antiga “competência imutável” (`competencia_trancada.py:4-6`, `fechamento.py:1457`) divergem da lei vigente `CLAUDE.md:418-422`; não são leitores executáveis nem prova de bloqueio.
5. A tranca arquiva cobranças/perguntas sem pressupor aprovação, por regra explícita `competencia_trancada.py:9-16` e C20 `app/docs/JUIZES.md:86-90`; nenhum pedido de nova lei para esse efeito demonstrado.
6. `furos_diarios.apurar:77` ainda cala re-apuração anterior por tranca; a lei ampla “nenhuma trava por trancada” pode já responder, mas **não é bloqueio novo hoje nos dez pares**. Não inventar impacto histórico baseado em `hoje` hipotético.

Chamadas diretas de teste, se interpretar TODO incluindo testes (todas em `app/ponto/tests/test_marco_da_competencia.py`, sem efeito de produção):

- `MarcoDaCompetenciaTest.test_MORDE_fechada_e_trancada_e_nao_aprovada`: K4 `:42`, `:45`, `:48`; K7 `:46`, `:49`.
- `test_MORDE_exportada_sem_aprovada_e_contador_nao_fecha`: K4 `:67`.
- `test_MORDE_paga_e_holerite_de_lote_publicado_da_empresa`: K4 `:82`.
- `test_tranca_de_outra_empresa_nao_conta_e_sem_empresa_e_aberta`: K4 `:88`, `:89`; K7 `:90`.

Não enumerei ORM PF em fixtures/tests/migrations como leitores produtivos; há muitas criações e atualizações artificiais. Se a resposta usar “TODO” sem qualificar produção, precisa esclarecer essa separação.

## 12/11 14:49 · sessao c9775433a019 · - · - · origem=Ronald (tmux)

Censo produtivo da família chamados, somente leitura. Todos os caminhos abaixo usam a tranca viva independentemente de existir `FechamentoMensal` ou de seu status; os corpos listados não consultam FM. (`app/chamados/juizes.py:1028`, `app/chamados/services/competencia_trancada.py:76`, `app/chamados/models.py:1041`.)

| Porta | Leitor, função e chamada | Efeito ao trancar |
|---|---|---|
| K7 | `app/chamados/juizes.py:1066`, `dias_trancados` | Calcula C20 por empresa/competência da data do turno; usa `pf.fechado_em` como início do intervalo para procurar toque humano. (`app/chamados/juizes.py:1062`, `app/chamados/juizes.py:1070`.) |
| K7 | `app/chamados/services/competencia_trancada.py:83`, `medir` | Antes retorna universo vazio; trancada passa a selecionar chamados/disputas elegíveis para encerramento e respostas para Pauta DP. Não carimba perguntas. (`app/chamados/services/competencia_trancada.py:84`, `app/chamados/services/competencia_trancada.py:126`, `app/chamados/services/competencia_trancada.py:147`, `app/chamados/services/competencia_trancada.py:159`.) |
| K4 | `app/chamados/models.py:1045`, `ChamadoColaborador._dia_em_competencia_trancada` | `renascer` recusa reabertura automática. Cadastro e atos DP nomeados ficam fora. (`app/chamados/models.py:1010`, `app/chamados/models.py:1042`, `app/chamados/catalogo/modulos.py:268`.) |
| PF direto | `app/chamados/juizes.py:2043`, `dia_em_competencia_trancada` | Itera competências com `reaberto_em IS NULL`; retorna verdadeiro quando a data pertence à janela de folha. Fail-open em exceção. (`app/chamados/juizes.py:2040`, `app/chamados/juizes.py:2045`.) |
| PF direto | `app/chamados/services/competencia_trancada.py:312`, `medir_frota` | Inclui a competência no contador e chama `medir`; não escreve. (`app/chamados/services/competencia_trancada.py:313`.) |
| C20 | `app/chamados/juizes.py:1395`, `_pergunta_viva_calc` | Pergunta passa a morta por leitura, antes de considerar a resposta; não planta batida, não absolve nem carimba. (`app/chamados/juizes.py:1393`, `app/chamados/juizes.py:1397`.) |
| C20 | `app/chamados/models.py:1847`, `DisputaSupervisao.respostas_sem_veredito` | Exclui respostas trancadas da lista que impede fechar a disputa; `ignorar_tranca=True` conserva-as para a Pauta DP. (`app/chamados/models.py:1841`, `app/chamados/models.py:1854`, `app/chamados/services/competencia_trancada.py:114`.) |
| C20 | `app/chamados/reconciliador.py:668`, `perguntas_absolvidas_pela_celula` | Pula a pergunta, impedindo que a vassoura a carimbe como absolvida permanentemente. (`app/chamados/reconciliador.py:664`.) |
| C20 | `app/chamados/reconciliador.py:748`, `_dia_fora_do_alcance_c` | Retira a pergunta do alcance novo da cura O261(C): a reemissão nova não age sobre ela. A consulta de entregue acontece antes. (`app/chamados/reconciliador.py:720`, `app/chamados/reconciliador.py:743`, `app/chamados/reconciliador.py:756`.) |
| C20 | `app/chamados/services/disputa_emissao.py:1970`, `_resolver_perguntas_por_chamado_resolvido` | Pula carimbo definitivo na pergunta morta pela tranca, permitindo que destrancar a devolva. (`app/chamados/services/disputa_emissao.py:1968`.) |
| C20 lote | `app/chamados/juizes.py:991`, `dia_da_pergunta_trancado` | Avulso delega ao lote de um; conversa não tem dia de turno/competência de fato. (`app/chamados/juizes.py:983`, `app/chamados/juizes.py:989`.) |
| C20 lote | `app/chamados/juizes.py:1145`, `tranca_da_consulta` | Pré-carrega os resultados por identidade da pergunta; o avulso os lê em `:987`. É alimentação da leitura, sem efeito persistente próprio. (`app/chamados/juizes.py:1125`, `app/chamados/juizes.py:987`.) |
| C20 lote | `app/chamados/services/tarefa_do_dia.py:422`, `CargaDaTarefa.__init__` | Alimenta conjunto de dias trancados; `_tarefa_pura` devolve razão `competencia_trancada` e nenhuma ação exigível por instância. (`app/chamados/services/tarefa_do_dia.py:195`, `app/chamados/services/tarefa_do_dia.py:198`, `app/chamados/services/tarefa_do_dia.py:202`.) |
| C20 resultado carregado | `app/chamados/services/carga_pergunta_viva.py:91`, `CargaDaVida.__init__`; `:147`, `CargaDaVida.trancada` | Reutiliza conjunto da tarefa, consultado por C6 na alternativa `carga.trancada(p)`. (`app/chamados/juizes.py:1395`.) |

Leitores do alimentador `tranca_da_consulta`, relevantes para aliases: `pergunta_no_app.perguntas_do_app` chama em `app/chamados/services/pergunta_no_app.py:57`; `_resolver_perguntas_por_chamado_resolvido` pode transformá-lo em `partial` com memo inicial em `app/chamados/services/disputa_emissao.py:1922`, usa-o em `:1942` e abre consulta fresca para fechamento em `:2081`. O resultado filtrado do app é `[p ... if pergunta_viva(p)]`. (`app/chamados/services/pergunta_no_app.py:58`.)

Exceção C20: a tranca só cala se não houver toque humano posterior em `(colab, dia)`. Os três atos são cobrar dia, resolver dia e desarquivar; humano significa usuário presente diferente do sistema; comparação estrita `criado_em > fechado_em`. (`app/chamados/juizes.py:1071`, `app/chamados/juizes.py:1083`, `app/chamados/juizes.py:1237`, `app/chamados/juizes.py:1253`.) Sem data, colaborador ou empresa, retorna falso; conversa não é fato trancável. (`app/chamados/juizes.py:1024`, `app/chamados/juizes.py:989`.) Quantos casos reais têm toque: `[não verificado]`.

O leitor indireto atual de `dia_em_competencia_trancada` é `app/ponto/services/furos_sem_cobranca.py:85`, função `medir`: remove dias trancados da contagem; a própria seleção está limitada à competência atual calculada com `hoje`. (`app/ponto/services/furos_sem_cobranca.py:60`, `app/ponto/services/furos_sem_cobranca.py:61`.)

Campos PF nesta família: `fechado_em` é lido em `app/chamados/juizes.py:1070` (referência do toque) e `app/chamados/services/competencia_trancada.py:105` (chamado resgatado depois da tranca fica com admin). `reaberto_em` é filtro de tranca viva nos dois PF diretos, `app/chamados/juizes.py:2043` e `app/chamados/services/competencia_trancada.py:312`. Escritas desses campos nesta família: nenhuma nos corpos citados; `ativo`/`fechado_por` tampouco são consultados nesses leitores. (`app/chamados/juizes.py:1004`, `app/chamados/services/competencia_trancada.py:66`.)

Efeito persistente do gancho: `disparar_no_ato` mede e, se houver ações, chama `encerrar` até o limite ou inicia comando em processo próprio. (`app/chamados/services/competencia_trancada.py:279`, `app/chamados/services/competencia_trancada.py:282`, `app/chamados/services/competencia_trancada.py:298`.) `encerrar` fecha chamados pela via da tranca, fecha disputas somente se `pode_fechar` permite e cria Pauta DP para respostas sem veredito. (`app/chamados/services/competencia_trancada.py:185`, `app/chamados/services/competencia_trancada.py:203`, `app/chamados/services/competencia_trancada.py:208`.) Quantidade de ações nas dez competências: `[não verificado]`.

DECISAO: `app/chamados/juizes.py:1395` / `app/chamados/services/tarefa_do_dia.py:195` | perguntas e tarefas passam a silêncio automático, reversível por toque humano | respondida por L-051 + corte LEI-TRANCA-E2-JUIZ, `app/chamados/juizes.py:1071`, `app/docs/CORTES.md:427`.

DECISAO: `app/chamados/services/competencia_trancada.py:83` | gancho pode fechar chamados/disputas e encaminhar respostas ao DP sem FM aprovado | regra explícita de tranca, `app/chamados/services/competencia_trancada.py:3`, `app/chamados/services/competencia_trancada.py:179`; exceções em `app/chamados/catalogo/modulos.py:268`.

DECISAO: `app/chamados/services/competencia_trancada.py:4` | texto diz “imutável” e sequência “aprovada → exportada → trancada”, mas o corpo não exige aprovação/exportação | O44 permite toque e validação, `app/chamados/services/cobrar_dia.py:8`, `app/chamados/services/validacao.py:399`; pressuposição executável “trancada implica aprovada” nesta família não demonstrada.

DECISAO: `app/chamados/services/cobrar_dia.py:39` / `app/chamados/services/validacao.py:399` | escrita manual continua autorizada; não há veto novo da tranca nessas portas | O44 “a tranca não barra o toque”, `app/chamados/services/cobrar_dia.py:8`, `app/chamados/services/validacao.py:399`.

Perguntas novas de lei identificadas nesta família: zero; leis existentes já respondem aos efeitos acima. (`app/docs/CORTES.md:427`, `app/chamados/juizes.py:1071`.)

## 12/11 14:31 · sessao 0b5862773d72 · AUDITORIA-DE-TRAVAS · leitura · origem=Code

**O v5 fecha os limites da publicação e o segundo classificador, mas ainda não fecha a semente nem declara toda a mudança de semântica do censo.** (`contrato_item3_v5.txt:12–26`; `contrato_item3_v5_semente.txt:4`, `:13`, `:18`, `:23`; `BACKLOG.md:1916`; `RELATO.md:266`, `:616`)

Uso `BACKLOG.md` e `RELATO.md` para os arquivos de `app/docs/`. Nos anexos, conto linhas físicas a partir de 1. Pousos e estados abaixo são declarações documentais; estado atual de produção: **[nao verificado]**.

**1. Furos do v4**

| Furo | Veredito sobre o v5 |
|---|---|
| G1 inclusivo | **Fecha:** termina antes do primeiro comentário; a antiga linha 7 pertence a P1. (`contrato_item3_v5.txt:12`, `:14`; `BACKLOG.md:7`) |
| G2 sem inserção/lápide | **Fecha:** (a) insere G2 e (b) substitui expressamente o marcador antigo pela lápide. (`contrato_item3_v5.txt:18–19`) |
| G3 sem `GERADO:FIM` | **Fecha:** (c) insere o delimitador entre BATI-02 e a primeira linha manual. (`contrato_item3_v5.txt:20`; `BACKLOG.md:2370–2371`) |
| Manual começando em 2371 | **Fecha:** preserva 2371 até EOF, incluindo as três linhas anteriormente mal delimitadas. (`contrato_item3_v5.txt:20`; `BACKLOG.md:2371–2373`) |
| “Render não produz” | **Fecha:** autoria passa a ser delimitada por zonas; BATIDA obsoleta permanece em G3 e pode desaparecer. (`contrato_item3_v5.txt:11`, `:15`, `:33`; `BACKLOG.md:2365–2370`; `app/core/juizes.py:1266`) |
| Byte a byte × mecanismo | **Fecha contratualmente:** exige substituir leitura/recomposição por cópia das zonas preservadas e verificar com `cmp`. O código desta cópia ainda usa texto, `rstrip`, `split` e `join`. (`contrato_item3_v5.txt:22–24`; `bin/gerar_backlog.py:111`, `:127`, `:139`) |
| Quatro propriedades do censo | **Resolve três e declara a alteração da quarta:** qualificada conforme, exato acompanhado de variante e `ValueError` direto conservam o matcher; ausência de OBRAS é substituída por falha do `.ids`. **Ainda omite a perda da exclusão por estado**, detalhada no item 3. (`contrato_item3_v5.txt:26–29`; `bin/tests/test_censo_antes_da_vaga.sh:542`, `:578`, `:603–608`; `bin/hook_stop_fila1.py:250–252`) |
| Segundo classificador | **Fecha:** permanece `_pertence_fila1`; a igualdade do fabricante compara a própria célula identificadora e não classifica pedidos livres. (`contrato_item3_v5.txt:7`, `:9`, `:26`; `bin/disjuntor.py:1672–1695`) |
| Semente: pousados | **Parcial:** retira O169-FECHADA, BO-0611-05 e TRAVA-SELO de `[ORDEM]`; mantém X1, cuja fase tem pouso documental, com incompletude “6 de 7”. (`contrato_item3_v5_semente.txt:23`, `:34–36`; `RELATO.md:265–266`, `:8059`, `:312`) | [nao verificado]
| Semente: faltantes | **Parcial:** inclui os três apontados no v4, mas ainda deixa outras linhas explicitamente fila 1 abertas sem representação. (`contrato_item3_v5_semente.txt:12`, `:15–16`; `BACKLOG.md:1831–1832`, `:1865–1866`, `:1986–1987`) |
| Travas LASTRO/F0F1 | **Fecha as atribuições erradas:** LASTRO recebe liberação condicionada; ESTACIONADA pertence à BO-DISPUTA. (`contrato_item3_v5_semente.txt:5`, `:9`, `:22`; `aval_respostas_1211_1230.txt:6`; `BACKLOG.md:1882`) |
| Linha BUG-CADASTRO | **Fecha a seção, mas não a referência:** está corretamente em POS-20; a nota continua apontando 2417, que é RAIZ-F0. BUG-CADASTRO está em 2408. (`contrato_item3_v5_semente.txt:25`, `:40`; `BACKLOG.md:2408`, `:2417`) |

Resta uma ambiguidade operacional na seção 2: **“marcador repetido (`<!--` de G1)” não pode significar unicidade global de `<!--`**. Esse token aparece em vários comentários preservados e no próprio G2 novo; essa interpretação recusaria um arquivo válido. A fronteira pela **primeira ocorrência** está definida; a regra de repetição do token genérico precisa ser corrigida. (`contrato_item3_v5.txt:12–13`, `:23`; `BACKLOG.md:7–9`, `:2042`)

**2. Partição depois de (a), (b), (c)**

**Sim: os limites descritos distribuem todas as linhas ancestrais uma vez cada**, com a substituição autorizada da linha 8; G2 e os novos delimitadores/cabeçalhos são inserções. (`contrato_item3_v5.txt:12–21`; `BACKLOG.md:1–7`, `:2042–2044`, `:2370–2371`, `:2425`)

| Zona | Linhas do HEAD que recebe |
|---|---|
| G1 | **1–6**. (`BACKLOG.md:1–7`; `bin/gerar_backlog.py:65–73`) |
| G2 | **Nenhuma ancestral:** bloco novo antes da antiga linha 7. (`contrato_item3_v5.txt:13`, `:18`; `BACKLOG.md:7`) |
| P1 | **7–2042**, com linha **8** substituída pela lápide. (`contrato_item3_v5.txt:14`, `:19`; `BACKLOG.md:7–8`, `:2042`) |
| G3 | **2043–2370**, seguido do novo `GERADO:FIM`. (`contrato_item3_v5.txt:15`, `:20`; `BACKLOG.md:2043–2044`, `:2370`) |
| CAUDA | **2371–2425**, precedidas do cabeçalho novo. (`contrato_item3_v5.txt:16`, `:20`; `BACKLOG.md:2371`, `:2425`) |

**Linhas de conteúdo produzido pelo render promovidas a P1/CAUDA: nenhuma identificada. Linhas manuais comidas por G1/G3: nenhuma identificada.** O gerador produz cabeçalho, contagem e famílias; a janela intermediária é conteúdo recuperado do arquivo anterior. (`bin/gerar_backlog.py:65–100`, `:119–127`)

Se “ESCREVE” significar a chamada física de escrita, o gerador atual também reescreve as linhas preservadas **8–2042**. Isso é cópia de conteúdo manual já preservado, não promoção de conteúdo gerado a manual. (`BACKLOG.md:8`, `:2042`; `bin/gerar_backlog.py:119–127`, `:138–140`)

Os casos antes problemáticos agora ficam assim:

| Linhas | Destino |
|---|---|
| 7, comentário ESTRUTURAL | P1. (`BACKLOG.md:7`; `contrato_item3_v5.txt:14`) |
| 2365–2370, BATIDA obsoleta | G3; desaparece se não renderizada. (`BACKLOG.md:2365–2370`; `app/core/juizes.py:1266`; `contrato_item3_v5.txt:15`) |
| 2371, SELO-MORDE | CAUDA. (`BACKLOG.md:2371`; `contrato_item3_v5.txt:20`) |
| 2372, MARCO-1490 | CAUDA. (`BACKLOG.md:2372`; `contrato_item3_v5.txt:20`) |
| 2373, ORFAOS-L-111 | CAUDA. (`BACKLOG.md:2373`; `contrato_item3_v5.txt:20`) |

**3. O censo muda de universo, mesmo com o matcher intacto**

Hoje `_fila1_viva` devolve `_itens_andaveis`, que exclui `_item_parado`: lê fechamento e palavras de parada na célula `[3]`, além de tachado na descrição `[2]`. O v5 entrega toda `[ORDEM]`, incluindo travados, sem reproduzir esse filtro. Portanto deixa de valer **“estado parado/fechado exclui a obra da fonte do censo”**. (`bin/disjuntor.py:1623–1634`; `bin/hook_stop_fila1.py:95–97`, `:168`, `:180`, `:244–252`; `contrato_item3_v5.txt:5`, `:26`)

| Linhas de `bin/tests/test_censo_antes_da_vaga.sh` | Efeito |
|---|---|
| **112–115** | FECHADA hoje dá rc0/FORA pela coluna. Depois conserva essa saída somente se ficar fora de `[ORDEM]`; se entrar, sem brief, exige censo e nega. A propriedade de exclusão por estado desaparece. (`:43`, `:112–115`; `bin/disjuntor.py:1707`, `:1715–1716`) |
| **250–251** | Mesmo deslocamento no Agent para FECHADA: dispensa passa a decorrer da ausência no `.ids`. (`:250–251`; `bin/disjuntor.py:1707`, `:1728`) |
| **252–253** | FILA2/CONGELADA também deixa de ser exclusão calculada pelo estado; depende de não constar em `[ORDEM]`. (`:44`, `:252–253`; `bin/hook_stop_fila1.py:95–97`) |
| **116–124** | IDs sem relação e validação da entrada continuam preserváveis; não dependem do filtro de estado. (`:116–124`; `bin/disjuntor.py:1690–1691`, `:2288`) |
| **125–130; 281–288** | Falha do BACKLOG continua obrigatoriamente NAO SEI pela validação de fonte do v5. (`:125–130`, `:281–288`; `contrato_item3_v5.txt:25–26`) |
| **131–135; 289–293** | Alteração já declarada: ausência de OBRAS deixa de ser falha; ausência da fonte estruturada passa a ser falha. (`:131–135`, `:289–293`; `contrato_item3_v5.txt:29`) |
| **301–305** | Forma inadequada de membro continua recusada depois do pertencimento. (`:301–305`; `bin/disjuntor.py:1707–1712`) |
| **534–543** | Qualificada sem brief nega; com brief conforme aprova. Preservado com a mesma célula na fonte. (`:534–543`; `bin/disjuntor.py:1688–1694`, `:1715–1727`) |
| **544–558; 559–574** | Ambiguidade, caixa, fronteira, cauda e dois candidatos conservam o matcher. (`:544–574`; `bin/disjuntor.py:1688–1695`) |
| **575–582** | Exato mais variante continua NAO SEI; exato sozinho conforme continua aprovado. (`:575–582`; `bin/disjuntor.py:1688–1694`) |
| **583–619** | Espelhos e `ValueError` direto permanecem; controles O1/O142 permanecem False. (`:583–619`; `bin/disjuntor.py:1689–1695`) |
| **621–645; 750–763** | Formato da negativa e mutantes do matcher continuam obrigações independentes; a troca de fonte não exige alterá-los. (`:621–645`, `:750–763`; `contrato_item3_v5.txt:26`, `:28`) |

**Não há fundamento para dizer que os resultados de 112–115/250–253 necessariamente ficam vermelhos:** a fixture pode excluir FECHADA/FILA2 de `[ORDEM]`. Mas então esses testes passam a provar exclusão pela lista, e deixam de provar exclusão pela coluna. Essa mudança também precisa ser declarada. (`contrato_item3_v5.txt:28`; `bin/tests/test_censo_antes_da_vaga.sh:43–44`, `:112–115`, `:250–253`)

**TRAVA não é veto do censo neste desenho:** o predicado recebe somente IDs, e um brief conforme pode obter APROVADO para um ID travado. Isso não equivale a autorizar sua construção nem prova que a trava foi satisfeita. (`contrato_item3_v5.txt:5`, `:26`; `bin/disjuntor.py:1703–1707`, `:1721–1727`)

**4. Semente: primeira célula exata e estado da linha**

| Seção | ID | Linha exata e declaração do BACKLOG |
|---|---|---|
| ORDEM | `SAIDA-NOTURNA-NO-DIA-CERTO` | Linha exata: **[nao verificado]**. Há despacho no RELATO, que não substitui a identidade exigida. (`RELATO.md:29`; `contrato_item3_v5.txt:4`) |
| ORDEM | `QUARENTENA-DO-COLAB` | REPROVADA; espera a vez. (`BACKLOG.md:765`) |
| ORDEM | `FIO-E0-V4-E3-LEITORES` | Fila 1, ABERTA, EM OBRA; entrada após E4B. (`BACKLOG.md:1873`) |
| ORDEM | `LASTRO-FAIXA-SEM-DEFAULT` | Fila 1, ABERTA, c3. (`BACKLOG.md:1813`) |
| ORDEM | `FIO-E0-V4-E4-ESCRITORES` | Fila 1; E4a–E4d; E4a-1 NO AR, sem fechamento da obra inteira. (`BACKLOG.md:1885`) |
| ORDEM | `C20-O168` | Fila 1, ABERTA, espera vaga. (`BACKLOG.md:2008`) |
| ORDEM | `RAIZ-DA-DISPUTA-F0-CASO-7279` | Fila 1, F0/sonda, frente da F1/HJ-067/HJ-073. (`BACKLOG.md:2417`) |
| ORDEM | `RAIZ-DA-DISPUTA-F0F1` | F0/F1 prontos; F1 respondida; c22 após E3/E4B. (`BACKLOG.md:1882`) |
| ORDEM | `AUDITORIA-DE-TRAVAS-CORTE` | Instrumento EM CURSO. (`BACKLOG.md:2404`) |
| ORDEM | `BUG-ORFAO-COM-PAR` | ESTRUTURA fila 1; sonda antes de classe/cura. (`BACKLOG.md:2418`) |
| ORDEM | `SONDA-MATRIZ-DE-TRANSICOES` | Fila 1 instrumento ABERTA; matriz rc0; volta 8 após CODEX-2-RAIAS. (`BACKLOG.md:1980`) |
| ORDEM | `SONDA-SEM-GRANT` | Fila 1 instrumento ABERTA; sondas congeladas até pouso. (`BACKLOG.md:1979`) | [nao verificado]
| ORDEM | `AUDITORIA-0911` | Fila 1 medida ABERTA; sondas após SONDA-SEM-GRANT. (`BACKLOG.md:1981`) |
| ORDEM | `SEGREDO-ARGV-RESIDUO` | Fila 1 instrumento ABERTA; depois do pouso SONDA. (`BACKLOG.md:1982`) | [nao verificado]
| ORDEM | `SOMBRA-CARIMBO-MENTE` | Fila 1 instrumento ABERTA; depois do pouso SONDA. (`BACKLOG.md:1983`) | [nao verificado]
| ORDEM | `TRIAGEM-OFFLINE-SEM-CAMERA` | REVERTIDA; retorno condicionado à classe e RED. (`BACKLOG.md:1915`) |
| ORDEM | `APP-MENSAGENS-MSG-0` | Fila 1/raia 2 ABERTA, nada construído; após TRIAGEM, **antes de O168/LASTRO**. (`BACKLOG.md:1916`) |
| ORDEM | `MEDE-CLEAR-X-COMPACT` | Fila 1 medida ABERTA, em curso. (`BACKLOG.md:1796`) |
| ORDEM | `CODEX-QUEIMA-B` | Fila 1 medida ABERTA, sondas rodando. (`BACKLOG.md:1797`) |
| ORDEM | `C20-FIO-MUDO` | Fila 1 ABERTA, construtor aberto. (`BACKLOG.md:1817`) |
| ORDEM | `BO-DISPUTA-SEM-PERGUNTA` | Linha própria registra construtor lançado. (`BACKLOG.md:1898`) |
| ORDEM | `E3-X1-FOLGA-NAO-E-SEM-COBERTURA` | Linha própria ABERTA, construtor após c8b. (`BACKLOG.md:1876`) |
| POS-20 | `BUG-CADASTRO-NAO-ALCANCA-O-FUTURO` | POS-20, frente da fila 2. (`BACKLOG.md:2408`) |
| POS-20 | `UI-PAINEL-FILA-OPUS-V5` | POS-20, fila registrada; pouso citado pertence à UI original. (`BACKLOG.md:2407`) | [nao verificado]
| POS-20 | `AUDITORIA-ITEM6-CONTADOR-124` | Linha exata: **[nao verificado]**. RELATO registra “não construído → fila”, sem classificá-lo como POS-20. (`RELATO.md:9`) |
| POS-20 | `TRIAGEM-FILA-OPUS-V6` | POS-20, ABERTA. (`BACKLOG.md:1815`) |
| POS-20 | `E4B-FILA-OPUS-C14` | POS-20, fila do opus. (`BACKLOG.md:1896`) |
| POUSADAS | `FIO-E0-V4-E4B` | POUSOU, commit 087fe758a. (`BACKLOG.md:1894`) |
| POUSADAS | `FIO-E0-V4-E4A-2` | NO AR, merge 613a51498. (`BACKLOG.md:1893`) |
| POUSADAS | `C20-O169` | POUSADA/FECHADA. (`BACKLOG.md:1798`) |
| POUSADAS | `O169-FECHADA` | Linha própria B3/volta 5; pouso posterior do delta documentado. (`BACKLOG.md:1969`; `RELATO.md:265`) | [nao verificado]
| POUSADAS | `BO-0611-05` | Linha própria B3/PARADA; merge/deploy posterior documentado. (`BACKLOG.md:1897`; `RELATO.md:8059`) | [nao verificado]
| POUSADAS | `TRAVA-MENOS-FILA-COM-SELO` | Linha própria ABERTA; ligação a TRAVA-SELO e pouso documentados. (`BACKLOG.md:2396`; `RELATO.md:8107`, `:312`) | [nao verificado]

A identidade de **SAIDA-NOTURNA** e **AUDITORIA-ITEM6-CONTADOR-124** continua sem linha exata demonstrada; a semente não satisfaz a exigência de exatamente uma linha para cada ID apenas citando seus despachos. (`contrato_item3_v5_semente.txt:2`, `:27`; `contrato_item3_v5.txt:4`; linhas exatas: **[nao verificado]**)

**Pouso em `[ORDEM]`: X1 permanece sem reconciliação.** O RELATO declara pouso da fase X1, “6 de 7”, e apply pós-pouso. Isso prova pouso parcial e incompletude; não prova fechamento integral, mas também não sustenta tratar seu estado como desconhecido. O restante precisa ser distinguido do pouso já registrado. (`contrato_item3_v5_semente.txt:23`, `:45`; `RELATO.md:248`, `:266`, `:185`) [nao verificado]

**Faltantes documentais:** além dos três corrigidos, estas primeiras células continuam fora da semente embora suas linhas declarem fila 1 aberta ou trabalho restante:

| ID faltante | Declaração |
|---|---|
| `COBPARC-ATA-DEFASADA-09` | Fila 1, aberta. (`BACKLOG.md:772`) |
| `COBPARC-AUSENCIA-CALA-DOIS-JUIZES` | Fila 1, aberta, depois da COBPARC. (`BACKLOG.md:773`) |
| `COBPARC-FUROS-LEITOR-PARCIAL` | Fila 1, aberta, depois da COBPARC. (`BACKLOG.md:774`) |
| `FASE-CONFLITANTE-VINCULO-DO-DIA` | Fila 1, aberta. (`BACKLOG.md:827`) |
| `LENVIVOS-A6-ALARME` | Fila 1, aberta. (`BACKLOG.md:887`) |
| `O125` | Fila 1, aberta. (`BACKLOG.md:1213`) |
| `ESMERIL-SEM-DIA-DO-TURNO` | Fila 1, aberta. (`BACKLOG.md:1831`) |
| `ESMERIL-SEM-BORDA` | Fila 1, aberta. (`BACKLOG.md:1832`) |
| `V2-BUGS-DO-MOTOR` | Fila 1; itens 1/3/4/5 abertos. (`BACKLOG.md:1835`) |
| `MOTOR-LE-O-TURNO` | Fila 1; espera E2/E3. (`BACKLOG.md:1850`; `RELATO.md:890`) |
| `E1-FILA-V2-3-A-6` | Fila 1, aberto. (`BACKLOG.md:1865`) |
| `E1-FILA-OPUS6-FILA` | Fila 1, aberto. (`BACKLOG.md:1866`) |
| `CODEX-2-RAIAS-PORTAO-CPU` | Fila 1 instrumento ABERTA; ainda falta pouso. (`BACKLOG.md:1986`; `RELATO.md:589`) | [nao verificado]
| `CODEX-ALIVIA-1-WRAPPER-BIN` | Fila 1 instrumento ABERTA, complemento da CODEX-2-RAIAS. (`BACKLOG.md:1987`) |

Essas são omissões de **linhas documentadas**, sem afirmar ausência de pouso em produção. Há ainda reconciliações necessárias: LASTRO-FOLGA consta ABERTA, mas seu desenho foi respondido por LASTRO-FAIXA; ESMERIL-SEM-INSTANTE consta aberta, mas tem pouso posterior; linhas de plano como ORDEM-ATE-O-ZERO, RAIA-2-BO-VISIVEL e META-18 declaram fila 1 ABERTA, sem exclusão específica desse tipo no contrato. (`BACKLOG.md:1812`, `:1830`, `:1883`, `:1906`, `:2406`; `RELATO.md:175`, `:1005`; `contrato_item3_v5.txt:2–6`) [nao verificado]

As travas e dependências restantes são:

- **E3 → pouso QUARENTENA:** há bloqueio de desenho real, mas não decisão impondo esse evento específico. O RELATO oferece absorver SVV, reverter a lista ou declarar limite, além de outros dois desenhos pendentes. A trava escolhe uma condição ainda não determinada pelo dado. (`contrato_item3_v5_semente.txt:4`; `RELATO.md:19`) [nao verificado]
- **SONDA → MATRIZ:** a matriz já está rc0; a condição restante escrita é **pouso CODEX-2-RAIAS + rebase**. A trava não representa essa condição, e CODEX-2-RAIAS falta na semente. (`contrato_item3_v5_semente.txt:13`, `:44`; `BACKLOG.md:1980`, `:1986`; `RELATO.md:616`) [nao verificado]
- **AUDITORIA → SONDA:** tem lastro explícito. (`contrato_item3_v5_semente.txt:14`; `BACKLOG.md:1981`; `RELATO.md:698`)
- **BO-DISPUTA ESTACIONADA:** tem lastro explícito. (`contrato_item3_v5_semente.txt:22`; `BACKLOG.md:1882`; `RELATO.md:373`)
- **APP:** estar após TRIAGEM respeita a linha; estar após O168/LASTRO a contraria. (`contrato_item3_v5_semente.txt:5`, `:7`, `:17–18`; `BACKLOG.md:1916`)
- **SAIDA:** o censo pode ocorrer antes, mas seu construtor espera pouso E3. Colocá-la no topo sem trava nem distinção de fase não representa essa dependência de construção. (`contrato_item3_v5_semente.txt:2`, `:4`; `RELATO.md:29`; `contrato_item3_v5.txt:5`) [nao verificado]
- **Endereços das notas:** BUG-CADASTRO é **2408**, não 2417; a regra da MATRIZ é **1980**, não 1985, que contém PLACAR-RETIDO-DO-LASTRO. (`contrato_item3_v5_semente.txt:40`, `:44`; `BACKLOG.md:2408`, `:2417`, `:1980`, `:1985`)

## 12/11 14:26 · sessao 95dd89c9b57b · - · - · origem=Ronald (tmux)

Resultado da semente, somente leitura. Todas as referências abaixo são documentais; produção atual [nao verificado].

| Seção | ID exato | Linha e estado declarado |
|---|---|---|
| ORDEM | SAIDA-NOTURNA-NO-DIA-CERTO | Primeira célula exata ausente [nao verificado]; despacho/construtor após pouso E3 em `app/docs/RELATO.md:29`. |
| ORDEM | QUARENTENA-DO-COLAB | REPROVADA; espera vez. `app/docs/BACKLOG.md:765` |
| ORDEM | FIO-E0-V4-E3-LEITORES | Fila 1, ABERTA, EM OBRA; entrada após E4B. `app/docs/BACKLOG.md:1873` |
| ORDEM | LASTRO-FAIXA-SEM-DEFAULT | Fila 1, ABERTA, c3. `app/docs/BACKLOG.md:1813` |
| ORDEM | FIO-E0-V4-E4-ESCRITORES | Fila 1, E4a–E4d; E4a-1 NO AR, sem fechamento inteiro. `app/docs/BACKLOG.md:1885` |
| ORDEM | C20-O168 | Fila 1, ABERTA, espera vaga. `app/docs/BACKLOG.md:2008` |
| ORDEM | RAIZ-DA-DISPUTA-F0-CASO-7279 | Fila 1, F0/sonda, frente F1/HJ-067/HJ-073. `app/docs/BACKLOG.md:2417` |
| ORDEM | RAIZ-DA-DISPUTA-F0F1 | Fila 1, F0/F1 PRONTOS, F1 RESPONDIDA, c22 após E3/E4B c4. `app/docs/BACKLOG.md:1882` |
| ORDEM | AUDITORIA-DE-TRAVAS-CORTE | Instrumento EM CURSO. `app/docs/BACKLOG.md:2404` |
| ORDEM | BUG-ORFAO-COM-PAR | ESTRUTURA fila 1, sonda antes classe/cura. `app/docs/BACKLOG.md:2418` |
| ORDEM | SONDA-MATRIZ-DE-TRANSICOES | Fila 1 instrumento ABERTA; matriz rc0 já feita, volta8 após CODEX-2-RAIAS. `app/docs/BACKLOG.md:1980` |
| ORDEM | SONDA-SEM-GRANT | Fila 1 instrumento ABERTA, sondas congeladas até pouso. `app/docs/BACKLOG.md:1979` |
| ORDEM | AUDITORIA-0911 | Fila 1 medida ABERTA, após SONDA. `app/docs/BACKLOG.md:1981` |
| ORDEM | SEGREDO-ARGV-RESIDUO | Fila 1 instrumento ABERTA, depois pouso SONDA. `app/docs/BACKLOG.md:1982` |
| ORDEM | SOMBRA-CARIMBO-MENTE | Fila 1 instrumento ABERTA, depois pouso SONDA. `app/docs/BACKLOG.md:1983` |
| ORDEM | TRIAGEM-OFFLINE-SEM-CAMERA | REVERTIDA; retorna após classe/RED. `app/docs/BACKLOG.md:1915` |
| ORDEM | APP-MENSAGENS-MSG-0 | Fila 1 raia2 ABERTA, nada construído; após TRIAGEM, **antes O168/LASTRO**. `app/docs/BACKLOG.md:1916` |
| ORDEM | MEDE-CLEAR-X-COMPACT | Fila 1 medida ABERTA, em curso. `app/docs/BACKLOG.md:1796` |
| ORDEM | CODEX-QUEIMA-B | Fila 1 medida ABERTA, sondas rodando. `app/docs/BACKLOG.md:1797` |
| ORDEM | C20-FIO-MUDO | Fila 1 ABERTA, construtor aberto. `app/docs/BACKLOG.md:1817` |
| ORDEM | BO-DISPUTA-SEM-PERGUNTA | Linha própria diz construtor lançado; estacionamento posterior documentado. `app/docs/BACKLOG.md:1898`; `app/docs/RELATO.md:373` |
| ORDEM | E3-X1-FOLGA-NAO-E-SEM-COBERTURA | Linha própria ABERTA; RELATO declara fase X1 POUSADA, “X1 6 de 7”, com apply pós-pouso. `app/docs/BACKLOG.md:1876`; `app/docs/RELATO.md:266`, `:185` |
| POS-20 | BUG-CADASTRO-NAO-ALCANCA-O-FUTURO | POS-20, frente fila2. `app/docs/BACKLOG.md:2408` |
| POS-20 | UI-PAINEL-FILA-OPUS-V5 | POS-20, fila registrada; pousada qualifica UI original. `app/docs/BACKLOG.md:2407` |
| POS-20 | AUDITORIA-ITEM6-CONTADOR-124 | Primeira célula exata ausente [nao verificado]; RELATO diz não construído → fila, sem POS-20. `app/docs/RELATO.md:9` |
| POS-20 | TRIAGEM-FILA-OPUS-V6 | POS-20 ABERTA. `app/docs/BACKLOG.md:1815` |
| POS-20 | E4B-FILA-OPUS-C14 | POS-20, fila opus. `app/docs/BACKLOG.md:1896` |
| POUSADAS | FIO-E0-V4-E4B | POUSOU 087fe758a. `app/docs/BACKLOG.md:1894` |
| POUSADAS | FIO-E0-V4-E4A-2 | NO AR 613a51498. `app/docs/BACKLOG.md:1893` |
| POUSADAS | C20-O169 | POUSADA/FECHADA. `app/docs/BACKLOG.md:1798` |
| POUSADAS | O169-FECHADA | Linha própria B3/volta5; delta v5 pousou depois. `app/docs/BACKLOG.md:1969`; `app/docs/RELATO.md:265` |
| POUSADAS | BO-0611-05 | Linha própria B3/PARADA; merge/deploy depois. `app/docs/BACKLOG.md:1897`; `app/docs/RELATO.md:8059` |
| POUSADAS | TRAVA-MENOS-FILA-COM-SELO | Linha própria ABERTA; ligação TRAVA-SELO/construção e pouso documentados. `app/docs/BACKLOG.md:2396`; `app/docs/RELATO.md:8107`, `:312` |

Achados para consolidar:

- **Identidade ainda quebra:** os dois IDs sem primeira célula exata não satisfazem seção0; referências ao RELATO não substituem linha do BACKLOG. A ausência foi varrida em todas as 2425 linhas [nao verificado como referência positiva de linha].
- **Pouso em ORDEM permanece:** X1 tem pouso documental; “6 de 7” demonstra incompletude medida, não anula o pouso registrado nem demonstra outro ID independente para o restante. `app/docs/RELATO.md:248`, `:266`, `:185`.
- **Trava E3 específica sem lastro:** E3 tem bloqueio real, mas a condição “esperar pouso QUARENTENA” não é determinada. O desenho oferece absorver SVV, reverter lista b4 ou declarar limite; há ainda outros dois desenhos. `app/docs/RELATO.md:19`.
- **Trava SONDA/MATRIZ desatualizada:** MATRIZ já rc0; condição declarada restante é pouso CODEX-2-RAIAS + rebase, cujo ID falta. `app/docs/BACKLOG.md:1980`, `:1986`; `app/docs/RELATO.md:616`, `:589`.
- **Trava BO ESTACIONADA tem lastro.** `app/docs/BACKLOG.md:1882`; `app/docs/RELATO.md:373`.
- **LASTRO/F0F1:** remover perguntas/travas antigas é sustentado; LASTRO foi rearmado condicionado ao censo/selo; ESTACIONADA era BO, não RAIZ. `app/docs/RELATO.md:20`; `app/docs/BACKLOG.md:1882`.
- **Dependência contrariada:** APP após TRIAGEM está certo, mas está depois de LASTRO/O168, contrariando a linha exata. `app/docs/BACKLOG.md:1916`. SAIDA no topo também contraria despacho de construir após pouso E3. `app/docs/RELATO.md:29`, `:23`.
- **Notas erradas:** BUG-CADASTRO é BACKLOG:2408, enquanto :2417 é RAIZ-F0. Regra MATRIZ é :1980; :1985 é PLACAR-RETIDO-DO-LASTRO. `app/docs/BACKLOG.md:2408`, `:2417`, `:1980`, `:1985`.
- **Faltantes inequívocos adicionais:** CODEX-2-RAIAS-PORTAO-CPU ABERTA e CODEX-ALIVIA-1-WRAPPER-BIN ABERTA/COMPLEMENTO, MOTOR-LE-O-TURNO fila1 esperando E2/E3, E1-FILA-V2-3-A-6 e E1-FILA-OPUS6-FILA fila1 abertos; ESMERIL-SEM-INSTANTE, ESMERIL-SEM-DIA-DO-TURNO e ESMERIL-SEM-BORDA fila1 abertas; V2-BUGS-DO-MOTOR com itens1/3/4/5 abertos. `app/docs/BACKLOG.md:1986`, `:1987`, `:1850`, `:1865`, `:1866`, `:1830`, `:1831`, `:1832`, `:1835`.
- **Linhas de planos também ficam sem reconciliação:** ORDEM-ATE-O-ZERO, RAIA-2-BO-VISIVEL e META-18 declaram fila1 ABERTA, mas são ordem/plano; contrato não define a exclusão desse tipo de ID. Não trataria automaticamente como novas obras. `app/docs/BACKLOG.md:1883`, `:1906`, `:2406`.
- Não declarar H12B/PAUTA08 faltantes não pousadas só pelo retrato antigo ABERTA: há pousos posteriores documentados. `app/docs/BACKLOG.md:1805`, `:1808`; `app/docs/RELATO.md:1144`, `:1044`.

## 12/11 14:25 · sessao f36a941955ec · - · - · origem=Ronald (tmux)

A partição proposta cobre as **2425 linhas do HEAD**, uma vez cada, após (a), (b), (c); G2 e os delimitadores/cabeçalhos novos são inserções. A linha 8 fica em P1 com a substituição expressamente autorizada pela lápide. (`app/docs/BACKLOG.md:1`, `:7`, `:8`, `:2042`, `:2043`, `:2370`, `:2371`, `:2425`; `contrato_item3_v5.txt:12–23`, numeração física do anexo)

| Zona após migração | Linhas ancestrais do HEAD |
|---|---|
| G1 | 1–6. (`app/docs/BACKLOG.md:1`, `:6`, `:7`; `bin/gerar_backlog.py:65–73`) |
| G2 | Novo bloco inserido antes da antiga linha 7. (`contrato_item3_v5.txt:13`, `:19`; `app/docs/BACKLOG.md:7`) |
| P1 | 7–2042, inclusive o marcador final; linha 8 substituída pela lápide. (`app/docs/BACKLOG.md:7`, `:8`, `:2042`; `contrato_item3_v5.txt:14`, `:20`) |
| G3 | 2043–2370 e o novo GERADO:FIM. (`app/docs/BACKLOG.md:2043`, `:2044`, `:2370`; `contrato_item3_v5.txt:15`, `:21`) |
| CAUDA | 2371–2425, precedidas pelo novo cabeçalho. (`app/docs/BACKLOG.md:2371`, `:2425`; `contrato_item3_v5.txt:16`, `:21`) |

**Não identifiquei linha gerada promovida a manual nem linha manual comida pelos limites propostos.** O cabeçalho produzido corresponde a G1; contagem e tabelas por família ficam em G3; o gerador atual recupera a janela manual de ORDEM-VIVA-TOPO até OBRAS:FIM, mas não gera seu conteúdo. A primeira linha manual hoje perdida pelo gerador, 7, passa a P1; a cauda começa corretamente em 2371. (`bin/gerar_backlog.py:65–100`, `:119–127`; `app/docs/BACKLOG.md:7`, `:2042`, `:2370–2374`)

A tabela BATIDA obsoleta, linhas **2365–2370**, fica em G3 e pode desaparecer sem promoção para manual: `PENDENTES['batida']` está vazio; famílias vazias são omitidas pelo gerador. (`app/docs/BACKLOG.md:2365–2370`; `app/core/juizes.py:1266`; `bin/gerar_backlog.py:76–79`)

Ressalva textual: **“marcador repetido (`<!--` de G1)” precisa significar a fronteira da primeira ocorrência**, como a definição de G1, e não unicidade global do token genérico `<!--`. Há muitas linhas de comentário em P1, além do novo G2 começar com esse token; uma contagem global rejeitaria o próprio BACKLOG válido. (`contrato_item3_v5.txt:12`, `:13`, `:24`; exemplos `app/docs/BACKLOG.md:7`, `:8`, `:9`, `:712`, `:2042`)

O mecanismo novo e seus tripwires ainda são **contrato**, sem implementação demonstrada nesta cópia: o código atual continua leitura em texto, `rstrip`/`split`, `join` e reescrita. (`contrato_item3_v5.txt:23–25`; `bin/gerar_backlog.py:111`, `:127`, `:138–140`)

## 12/11 14:25 · sessao 22c96c6beaae · RESCALDO-DAS-RAIAS-1211 · censo · origem=Code

A cópia não contém `.git`; não pude abrir branches/refs nem conferir os blobs completos das raias. Os trechos posteriores aos cortes de 5.000 bytes, os diffs não apresentados das segundas raias e a identidade integral dos patches ficam **[nao verificado]**.

Nas PARADAS, **S1–S15** remetem aos arquivos do `--stat` transcritos abaixo; isso identifica conteúdo atribuído à raia, sem afirmar que todos esses caminhos estejam fisicamente ausentes do main. **[nao verificado]**

| OBRA | CLASSE | Prova no main | Regra da raia, situação e motivo |
|---|---|---|---|
| QUARENTENA-DO-COLAB | PARADA | nao achei | Isola cobrança por colaborador/período; o main ainda seleciona por cobrança e silêncio futuro (`app/chamados/services/painel.py:188`); fora: **S1**; motivo: cópia reprovada por fechamento da janela, vigia, segundo escritor e provas (`app/docs/RELATO.md:2493`). |
| SILENCIO-VENCIDO-VOLTA | PARADA | nao achei | Devolve o silêncio vencido à fila/pílula/app; a fila já o aceita, mas o verbo ainda vira HISTORICO (`app/chamados/services/painel.py:189`; `app/chamados/services/verbos.py:51`); fora: **S1**; motivo: pouso preso à QUARENTENA (`app/docs/RELATO.md:19`). |
| COBPARC-FILA-RELOGIO | PARADA | nao achei | Transmite um instante único da borda pelos laços; o main ainda lê relógio por chamada (`app/ponto/services/cartorio.py:373`); fora: **S2**; motivo: estacionada por aval, faltando mutantes/suite e resposta sobre o padrão (`app/docs/RELATO.md:1540`). |
| JUSTIFICATIVA-PAGA-O-PERIODO | PARADA | nao achei | Limita cobertura ao marco/período e leva pagamento parcial à porta; fora: **S3**; motivo: condições do DIFF falharam, motor/folha não consumiam o parcial e o aval posterior exigiu novo DIFF (`app/docs/RELATO.md:2553`; `app/docs/BACKLOG.md:1061`). |
| O261-PAI-TERMINAL | PARADA | nao achei | Religa disputa a pai vivo; o main ainda devolve disputa aberta sem conferir o pai e declara essa lacuna no nascimento (`app/chamados/services/disputa_emissao.py:566`; `app/chamados/services/fio.py:755`); fora: **S4**; motivo: religação mistura competências, inclusive exportadas (`app/docs/RELATO.md:2218`). |
| ATA-X-TURNO-PAUSA | PARADA | nao achei | Faz a ata reconhecer a pausa do juiz e acusa pausa deslocada no motor/CxR; fora: **S5**; motivo: retomada depende de 10/10 e dinheiro igual, e o registro mais recente a deixa no fim da fila Codex (`app/docs/RELATO.md:847`; `app/docs/RELATO.md:753`). |
| LASTRO-DEGRAU-2 | PARADA | nao achei | Substitui casamento próprio pela grade/ata composta; o main ainda compara tipo e distância linear (`app/ponto/services/lastro.py:307`); fora: **S6**; motivo: resultado dependia da janela, exigindo cura anterior no construtor (`app/docs/RELATO.md:2393`; `app/docs/RELATO.md:2379`). |
| JUIZ-DO-PAR-DUAS-ENTRADAS | ABSORVIDA-POR JUIZ-DO-PAR-CURA-NA-FONTE | `app/docs/RELATO.md:2486`; `app/docs/RELATO.md:2446`; `app/escala/utils.py:613` | A ata senta cada batida pelo papel do turno; o main realiza isso na fonte `_match_marcos`, compartilhada pelos leitores, com pouso documentado. |
| BUG-ENC-CELULAS-FORMA | PARADA | nao achei | Corrige a alimentação de `dias_encerrados`, mas o main ainda repassa `celulas` cru ao pareador (`app/ponto/turnos.py:2150`); fora: **S7**; motivo: faltavam cura da via singular, selos, suite e impacto, com vaga devolvida à F2 (`app/docs/RELATO.md:7541`). |
| GRADE-FOLGA-DO-VINCULO | ABSORVIDA-POR GRADE-FOLGA-JUIZ | `app/docs/RELATO.md:1600`; `app/docs/RELATO.md:7508`; `app/escala/utils.py:1149`; `app/ponto/precedencia.py:237` | O núcleo calendário-do-vínculo/manual-da-pessoa está no juiz comum; o trecho `eh_trabalho` do calendário foi retirado expressamente, portanto não afirmo absorção de todos os deltas anexados (`app/docs/RELATO.md:1590`; `app/docs/BACKLOG.md:826`). |
| O275 | PARADA | nao achei | Cria cadastro do tipo para tornar feriado previsto; fora: **S8**; motivo: pouso retido na escolha entre FORMA A e FORMA B, que produzem códigos diferentes (`app/docs/RELATO-ARQUIVO.md:753`; `app/docs/RELATO-ARQUIVO.md:755`). |
| O276 | NO-MAIN-COM-OUTRO-NOME | `app/ponto/turnos.py:1430`; `app/ponto/turnos.py:1447`; `app/docs/RELATO.md:3293` | O teto do dia chegou pelo R1-H-PORTAO-DE-26H: saída além dele deixa turno aberto e saída órfã; o main também aplica a exceção L-084 das duas pontas deslocadas (`app/ponto/turnos.py:1435`). |
| V3-RETOMA-FILA | PARADA | nao achei | Faz `diff_calculador` obter v2 da lavra, mas o main conserva laço próprio e CCT do template inicial (`app/ponto/management/commands/diff_calculador.py:245`; `app/ponto/calculador/alimentacao.py:389`); fora: **S9**; motivo: instrumento espera o pouso da V3 (`app/docs/RELATO.md:1707`). |
| V3-RETOMA-PERIODOS-DA-CELULA | PARADA | nao achei | Faz a v3 ler períodos/pausas lavrados e recusar período sem proveniência; o main ainda guarda segmentos e continua diante das lacunas (`app/ponto/calculador/alimentacao.py:158`; `app/ponto/calculador/alimentacao.py:79`); fora: **S10**; motivo: pista dependente, P4 aberto e base atrasada; só P5 isolada pousou (`app/docs/RELATO.md:7328`; `app/docs/RELATO.md:7408`). |
| V3-E1 | PARADA | nao achei | É bateria/arnês, sem implementação de produção no stat; fora: **S11**; motivo: nao sei; o último resultado encontrado registra validação de status vermelha, sem declarar destino posterior do arnês (`app/docs/RELATO.md:1805`; `app/docs/RELATO.md:1815`). |
| E3-ISENTO-INSUMOS | ABSORVIDA-POR ISENTO-H10 | `app/docs/BACKLOG.md:934`; `app/docs/BACKLOG.md:924`; `app/ponto/services/cartorio.py:548` | A isenção entrou na impressão dos insumos pela ISENTO-H10, com horizonte `regra >= 4`, substituindo o alcance atemporal da cópia. |
| CASA-ESCALA-E4 | NO-MAIN-COM-OUTRO-NOME | `app/colaboradores/queries.py:64`; `app/ponto/nucleo.py:15`; `app/folha/previa.py:81`; `app/docs/RELATO.md:3386` | Os leitores anexados já perguntam aos juízes `q_vinculo_cobre_a_data`, `vinculo_cobre_a_data` e `escalas_do_periodo`, inclusive no dinheiro. |
| QUATRO-PRAZOS | NO-MAIN-COM-OUTRO-NOME | `app/ponto/management/commands/processar_alertas_avancados.py:63`; `app/ponto/management/commands/processar_alertas_turno.py:118`; `app/ponto/management/commands/detectar_ausencias.py:471` | As partes anexadas estão nos juízes `turno_aberto_ha`, `lembrete_fim_turno`, `marcos_vencidos` e `dia_encerrado`, chamados pelo código de produção. |
| JUIZ-VARREDURA-1 | NO-MAIN-COM-OUTRO-NOME | `app/ponto/management/commands/reavaliar_ausencias_feriado.py:35`; `app/ponto/management/commands/reavaliar_ausencias_feriado.py:43`; `app/docs/RELATO.md:1297` | A conversão do cron em vigia está no main pela versão JUIZ-1 v5; ele conta e alarma, com pouso posterior ao da cópia anexada. |
| JUIZ-VARREDURA-3 | ABSORVIDA-POR C20-JUIZ-3 | `app/docs/RELATO.md:7859`; `app/docs/RELATO.md:7862`; `app/chamados/reconciliador.py:1373`; `app/chamados/management/commands/reconciliar_chamados.py:118` | O cron virou vigia pela C20-JUIZ-3, junto dos produtores/consumidores de eventos necessários à retratação no ato. |
| F1C4-3-RG-SEM-CELULA-CASO | NO-MAIN-COM-OUTRO-NOME | `app/ponto/management/commands/reconciliar_grade.py:281` | A raia acrescenta o teste; a regra já está na F1C4-3-NONE-COBRA: sem célula, o dia não entra nos mortos por indefinição. |
| O10-C | PARADA | nao achei | Leva trocas históricas ao par pela porta existente; fora: **S12**; motivo: SOLO exige lista do admin com porta de UI alcançável, retirando a cópia do comboio para uma volta 2 (`app/docs/RELATO.md:1088`; `app/docs/RELATO.md:1091`). |
| DEPLOY-[nome]-ELO | ABSORVIDA-POR DEPLOY-RELOAD-GRACIOSO | `app/docs/BACKLOG.md:930`; `bin/deploy.sh:341`; `bin/deploy.sh:359` | A proteção do elo foi resolvida por HUP gracioso e recusa de restart com cron vivo, substituindo o mecanismo de admissão anexado. |
| SUITE-POR-IMPACTO | PARADA | nao achei | Seleciona testes por cobertura e certifica inteira em cópia; fora: **S13**; motivo: mapa/replay do PRONTO 5 incompletos e rebase ainda em cópia (`app/docs/RELATO.md:1385`; `app/docs/RELATO.md:1387`; `app/docs/RELATO.md:1399`). |
| PADRAO-DE-CIMA | PARADA | nao achei | A raia pertinente altera disjuntor/harness; fora: **S14**; motivo: nove voltas e reprovação de desenho, com a décima sem verificador/opus; a primeira raia anexada é de outra obra (`app/docs/RELATO.md:1595`; `app/docs/BACKLOG.md:870`). |
| CODEX-CONSTRUTOR-PILOTO | ABSORVIDA-POR CODEX-CONSTRUTOR-NEGOCIO | `app/docs/RELATO.md:1127`; `app/docs/RELATO.md:873`; `bin/codex_pergunta.sh:217`; `bin/codex_pergunta.sh:415` | O instrumento anexado foi generalizado para construtor em worktree validada, com escrita limitada a `WT/app`; isso não comprova entrega da tela do piloto (`app/docs/RELATO.md:2239`). |
| CODEX-CONTEXTO-DA-CASA | PARADA | nao achei | Gera AGENTS e valida brief independente do verificador; fora: **S15**; motivo: pouso retido para aguardar briefs dos construtores em voo, com complementos ainda dependentes dessa obra (`app/docs/BACKLOG.md:1020`; `app/docs/RELATO.md:1743`). |
| OTIMIZA-1 | PARADA | nao achei | Publica inteiras por pouso no ESTADO; o gerador ainda passa da recusa diretamente ao Codex (`bin/gerar_estado.py:265`); fora: **S13**; motivo: faltavam suite, replay, verificador e pouso próprio (`app/docs/RELATO.md:1395`). |
| FIO-ESTADO-LE-O-ALCANCE | NO-MAIN-COM-OUTRO-NOME | `bin/fio_estado.py:137`; `bin/fio_estado.py:175`; `app/docs/BACKLOG.md:966` | A manchete foi reimplementada em `_ler_carimbo`/`linha` sob contrato v2, com pouso próprio registrado; a validação das somas de partições da raia antiga não integra essa versão (`app/docs/RELATO.md:1787`). |

Os inventários seguintes transcrevem os nomes exibidos nos stats anexados, agrupando prefixos e alternativas com `{…}`; a comparação integral desses arquivos com os blobs das raias é **[nao verificado]**. Em **S1**, o resumo anuncia 54 arquivos, mas nem todos os nomes foram exibidos no dado fornecido; os nomes omitidos são **[nao verificado]**.

- **S1 — QUARENTENA / SILENCIO:** `app/chamados/services/{pergunta_no_app.py,quarentena.py,silencio_ausencia.py,silencio_isentos.py,verbos.py,vigia_fio.py}`; `app/chamados/{signals.py,urls.py,views_arquivar.py}`; `app/chamados/tests/{test_f1_arquivar.py,test_f7_janela_arquivo.py,test_f8_lista_esconde.py,test_f9_contador_bate_com_a_lista.py,test_o261_vigia_fio.py,test_porta_arquivo_silencio.py,test_quarentena_do_colab.py,test_silenciar_isentos.py,test_silencio_vencido_2a_leitura.py,test_silencio_vencido_volta.py,test_varredura_isentos_evento.py}`; `app/colaboradores/services/{colaborador.py,ficha.py,isencao.py}`; `app/colaboradores/{views.py,tests/test_porta_colaborador.py}`; `app/core/{juizes.py,portas.py,management/commands/lavrar_badge_navbar.py,tests/test_selo_performance.py,tests/test_tripwire_contagem_de_pendencia.py}`; `app/docs/{ARQUITETURA.mmd,MAPA.md}`; `app/ponto/management/commands/{alertar_disputas_pendentes.py,escalonar_chamados_supervisao.py}`; `app/ponto/tests/test_selo_chokepoint_escrita.py`; `app/templates/chamados/partials/{_busca_arquivados.html,_lista_chamados.html,_meus_arquivados.html,_quarentenas.html}`. **[nao verificado]**
- **S2 — COBPARC-FILA-RELOGIO:** `app/chamados/management/commands/{disparar_perguntas_competencia.py,fechar_becos_disputa.py,reabrir_orfas_enterradas.py,reconciliar_chamados.py,retratar_chamados_lei_cala.py,smoke_chamados.py}`; `app/chamados/{models.py,reconciliador.py,tests/test_selo_universo_reconciliador.py}`; `app/chamados/services/{adesao.py,becos.py,competencia_trancada.py,disputa_emissao.py,fabrica_da_celula.py,pergunta_no_app.py}`; `app/escala/{management/commands/apurar_furos_diarios.py,regua_defesa.py,tests/test_cobertura_parcial_periodo.py}`; `app/ponto/management/commands/{detectar_ausencias.py,detectar_cluster_espurio.py,detectar_par_relampago.py,emitir_turno_aberto_retroativo.py,marcar_foto_ausente_retro.py,processar_alertas_avancados.py,processar_alertas_turno.py,reabrir_chamados_ausencia_aberta.py,reconciliar_fantasmas.py,reconciliar_grade.py,reconciliar_vinculo.py,supra_juiz.py}`; `app/ponto/services/{cartorio.py,cluster_espurio.py,geofence.py,justificativa.py}`; `app/ponto/{views.py,tests/test_cobparc_relogio_na_borda.py,tests/test_cobparc_relogio_um_instante_por_laco.py,tests/test_o_outbox_celula.py}`. **[nao verificado]**
- **S3 — JUSTIFICATIVA:** `app/chamados/tests/test_cobertura_parcial_nao_emite_retroativo.py`; `app/core/juizes.py`; `app/escala/{utils.py,tests/test_cobertura_justificativa_so_o_marco.py,tests/test_grade_segue_a_precedencia.py,tests/test_grade_visual_ferias.py,tests/test_justificativa_paga_o_periodo.py}`; `app/ponto/{turnos.py,services/cartorio.py,services/efeito_ausencia.py,services/fechamento.py}`. **[nao verificado]**
- **S4 — PAI-TERMINAL:** `app/chamados/{management/commands/vigiar_fio_morto.py,models.py,services/acoes_disputa.py,services/vigia_fio.py,tests/test_o261_pai_terminal.py,tests/test_o261_vigia_fio.py}`; `logs/o261/{codex_c2_censo_emissores.txt,codex_c2_sonda_passivo.txt,passivo_pai_terminal.py,recorte_c2_b21ee2c3.log,red_c2_e3f6d405.log}`. **[nao verificado]**
- **S5 — ATA-PAUSA, união das duas raias:** `app/core/tests/test_medida_turno_do_par.py`; `app/escala/{services/cadastro_realidade.py,tests/test_ata_x_turno_pausa.py,utils.py}`; `app/ponto/{management/commands/diff_janela_he.py,motor_calculo_v2.py,services/cxr_na_lista.py,tests/test_ata_pausa_l084_cxr.py,tests/test_juiz_do_par_uma_entrada.py,turnos.py}`. **[nao verificado]**
- **S6 — LASTRO:** `app/ponto/{management/commands/fechar_cobranca_com_lastro.py,services/cartorio.py,services/lastro.py,tests/test_bug114_fechador_de_dois_dias.py,tests/test_lastro_degrau2_juiz_do_par.py,tests/test_tripwire_b5j_universo_de_apuracao.py}`. **[nao verificado]**
- **S7 — BUG-ENC:** `app/ponto/{tests/test_bug_enc_celulas_forma.py,tests/test_e4_carga_paridade.py,turnos.py}`. **[nao verificado]**
- **S8 — O275:** `app/config/crons.py`; `app/escala/{management/commands/vinculos_tipo_feriado_contra_vinculo.py,migrations/0043_tipoescala_feriado_e_dia_trabalho.py,models.py,services/cadastro_tipo.py,tests/test_o275_sdf_feriado_e_dia.py,tests/test_tripwire_o275_leitor_unico_do_flag.py,views_wizard.py}`; `app/templates/escala/{_wizard_preview.html,tipos_lista.html,wizard_tipo_escala.html}`. **[nao verificado]**
- **S9 — V3-FILA:** `app/ponto/{calculador/alimentacao.py,management/commands/diff_calculador.py,tests/test_diff_calculador_lado_v2_e_a_lavra.py,tests/test_v3_dobra_do_ciclo_do_dia.py}`. **[nao verificado]**
- **S10 — V3-PERIODOS, união das duas raias:** `app/ponto/{calculador/alimentacao.py,calculador/periodo.py,calculador/regras.py,motor_calculo_v2.py}`; `app/ponto/tests/{test_s5b_split_noturno_tem_um_dono.py,test_v3_dobra_do_ciclo_do_dia.py,test_v3_dobra_noturna_e_cct_do_dia.py,test_v3_parei_p5_parte_menor_que_todo.py,test_v3_periodos_da_celula.py}`. **[nao verificado]**
- **S11 — V3-E1:** `app/docs/E1-PERGUNTA-AO-DP-CONTADOR.md`; `app/ponto/tests/{test_e1_lote1_valor_do_dia.py,test_e1_lote2_status_do_dia.py,test_e1_lote3.py,test_v3_e1_valor.py}`. **[nao verificado]**
- **S12 — O10-C:** `app/config/crons.py`; `app/ponto/{management/commands/trocas_plantao_lote_o10c.py,tests/test_o10c_lote_trocas.py}`. **[nao verificado]**
- **S13 — SUITE / OTIMIZA, união das raias:** `bin/{gerar_estado.py,pista_estado.py,pre-push.sh,recursos.sh,regua.sh,suite.sh,suite_impacto.py,suite_impacto.sh,trava_teste.sh}`; `bin/suite_impacto/{contexto_modulo.py,coveragerc,extrair_mapa.py,selos_so_na_principal.txt,sitecustomize.py}`; `bin/tests/{test_medida_suites_por_pouso.sh,test_porta_le_env_da_casa.sh,test_selo_espera_processo_morde.sh,test_suite_impacto_certificado.sh,test_suite_impacto_selecao.sh,test_suite_sh.sh,test_trava_teste.sh}`. **[nao verificado]** [nao verificado]
- **S14 — PADRAO, raia pertinente `wt-pdc-v3-170430`:** `bin/{codex_pergunta.sh,disjuntor.py,hook_stop_fila1.py,padrao_vocab.py}`; `bin/tests/{_mordida.sh,test_disjuntor.sh,test_hook_esperando_com_prova.sh,test_lei_akita.sh,test_padrao_de_cima.sh}`. **[nao verificado]**
- **S14, primeira raia anexada, de CONTENCAO:** `.claude/settings.json`; `CLAUDE.md`; `app/docs/{TICKETS.md,contratos/CONTENCAO-QUE-MORDE-V2.txt}`; `bin/{arvore_hash.sh,codex_pergunta.sh,contrato_do_push.py,disjuntor.py,hook_stop_fila1.py,marco_de_clear.py,pre-push.sh,suite.sh}`; `bin/tests/{test_contrato_do_push.sh,test_disjuntor.sh,test_handoff_sessao.sh,test_hora_da_secao_nao_e_futura.sh}`. **[nao verificado]**
- **S15 — CODEX-CONTEXTO:** `AGENTS.md`; `bin/{codex_brief_verificador.py,codex_limite_estado.py,codex_pergunta.sh,gerar_agents_md.py,relato.sh}`; `bin/tests/{test_agents_md_gerado.sh,test_codex_brief_verificador.sh}`. **[nao verificado]**

**Soma das classificações:** NO-MAIN-COM-OUTRO-NOME **6** + ABSORVIDA-POR **6** + MORTA-POR-CORTE **0** + PARADA **17** = **29**. **[nao verificado: contagem derivada desta resposta, sem arquivo próprio]**

**Obras com raia aparentemente alheia:**

- **PADRAO-DE-CIMA → `wt-contencao-v4-111700`:** o head `3a743cdd` é identificado como CONTENCAO-QUE-MORDE-V2; a raia própria do PDC é `wt-pdc-v3-170430`. (`app/docs/BACKLOG.md:870`; `app/docs/RELATO.md:1595`)
- **JUIZ-DO-PAR-DUAS-ENTRADAS → segunda raia, `wt-atapausa-092038`:** os commits anexados são de ATA-X-TURNO-PAUSA **[nao verificado]**; o main distingue a classe da pausa da divergência da ata e diz que aquela cópia antiga não foi portada. (`app/docs/RELATO.md:2530`)

## 12/11 14:25 · sessao ecf2340693ca · - · - · origem=Ronald (tmux)

A seção 4 conserva o classificador, mas **muda o domínio entregue a ele**: `_fila1_viva` hoje retorna apenas células julgadas andáveis; o v5 passa a entregar toda `[ORDEM]`, inclusive IDs com TRAVA. (`bin/disjuntor.py:1623`, `bin/disjuntor.py:1634`; `contrato_item3_v5.txt:26`)

O filtro atual efetivamente lê coluna/estado:

- `_itens_andaveis` descarta `_item_parado`. (`bin/hook_stop_fila1.py:250`)
- `_item_parado` combina fechamento e expressões de parada na célula `[3]`. (`bin/hook_stop_fila1.py:180`)
- Fechamento lê `[3]` e tachado `~~` na descrição `[2]`. (`bin/hook_stop_fila1.py:168`)
- Parada inclui `CONGELADA`, `FILA 2`, `aguardando`, `PAREI`, `NAO CONSTRUIR`, portão e outras formas. (`bin/hook_stop_fila1.py:95`, `bin/hook_stop_fila1.py:96`, `bin/hook_stop_fila1.py:97`)

Portanto a propriedade **“parada/fechada na coluna → excluída da fonte do censo” deixa de valer**. Isso não requer alterar `_pertence_fila1`, pois a entrada já mudou antes de ele ser chamado. (`bin/disjuntor.py:1634`, `bin/disjuntor.py:1707`, `bin/disjuntor.py:2306`)

| Linhas do selo | Efeito exato |
|---|---|
| `112–115` | Hoje `OBRA-FECHADA` retorna rc0 e grava FORA porque a linha da fixture tem estado `**FECHADA**`. Depois só conserva essa saída se o ID ficar fora de `[ORDEM]`; estando dentro, sem brief, passa a recusa/NEGOU. (`bin/tests/test_censo_antes_da_vaga.sh:43`, `bin/tests/test_censo_antes_da_vaga.sh:112`, `bin/tests/test_censo_antes_da_vaga.sh:114`; `bin/disjuntor.py:1715`, `bin/disjuntor.py:2311`) |
| `250–251` | Mesmo deslocamento no Agent: a dispensa de `OBRA-FECHADA` passa a depender da exclusão no `.ids`. Se incluído, sem brief, o censo nega. (`bin/tests/test_censo_antes_da_vaga.sh:250`, `bin/tests/test_censo_antes_da_vaga.sh:251`; `bin/disjuntor.py:1707`, `bin/disjuntor.py:1728`) |
| `252–253` | `OBRA-FILA2` hoje passa por estar `CONGELADA, FILA 2`; depois passa por ausência em `[ORDEM]`. Inclusão passaria a exigir censo. (`bin/tests/test_censo_antes_da_vaga.sh:44`, `bin/tests/test_censo_antes_da_vaga.sh:252`, `bin/tests/test_censo_antes_da_vaga.sh:253`; `bin/hook_stop_fila1.py:95`) |
| `534–645` | O conjunto equivalente de células preserva os resultados do matcher, incluindo qualificada aprovada, exato com variante ambíguo e `ValueError` direto. A troca da fonte por si só não muda essas três propriedades. (`bin/disjuntor.py:1688`, `bin/disjuntor.py:1689`, `bin/disjuntor.py:1692`, `bin/disjuntor.py:1694`; `bin/tests/test_censo_antes_da_vaga.sh:542`, `bin/tests/test_censo_antes_da_vaga.sh:578`, `bin/tests/test_censo_antes_da_vaga.sh:603`, `bin/tests/test_censo_antes_da_vaga.sh:606`) |

**Não é correto afirmar que as saídas de `112–115` e `250–253` inevitavelmente mudam**, pois o v5 autoriza migrar a fixture e pode excluir esses dois IDs de `[ORDEM]`. O que inevitavelmente muda é a propriedade provada: sai a autoridade do estado, entra a presença no `.ids`. (`contrato_item3_v5.txt:28`; `bin/tests/test_censo_antes_da_vaga.sh:43`, `bin/tests/test_censo_antes_da_vaga.sh:44`)

Um ID com TRAVA em `[ORDEM]` pode receber `CENSO APROVADO` se o brief estiver conforme: `_pertence_fila1` só recebe strings de IDs, e a porta do censo não recebe nem consulta a TRAVA. Portanto **“TRAVA → censo nega” não é propriedade desse desenho**. (`contrato_item3_v5.txt:5`, `contrato_item3_v5.txt:26`; `bin/disjuntor.py:1707`, `bin/disjuntor.py:1721`, `bin/disjuntor.py:1722`)

## 12/11 14:22 · sessao 750fcce341e8 · - · - · origem=Ronald (tmux)

Resultado recomendado: **8 PARADA; 2 ABSORVIDA-POR**. As descrições das raias dependem dos anexos; sem refs locais, o conteúdo além dos 5.000 bytes é [nao verificado].

| OBRA | CLASSE | Provas conferidas e frase utilizável |
|---|---|---|
| QUARENTENA-DO-COLAB | PARADA | **nao achei**. A raia isola a cobrança por colaborador/período; o main continua selecionando a fila apenas por COBRANCA e silêncio futuro em `app/chamados/services/painel.py:188`; motivo: reprovação da 7e864870 por fechar/levantar janela, vigia, segundo escritor, migration e provas, em `app/docs/RELATO.md:2493`. |
| SILENCIO-VENCIDO-VOLTA | PARADA | **nao achei**. A raia devolve silêncio vencido à fila/pílula/app; só a metade da fila está presente (`app/chamados/services/painel.py:189`), enquanto `app/chamados/services/verbos.py:51` ainda transforma vencido em HISTORICO; motivo: presa à QUARENTENA reprovada, expressamente `app/docs/RELATO.md:19`. |
| COBPARC-FILA-RELOGIO | PARADA | **nao achei**. A raia lê um instante na borda e o transmite pelos laços; `app/ponto/services/cartorio.py:373` ainda usa `ate or now()` por chamada; motivo: estacionada por aval, faltando mutantes/suite e com pergunta do padrão aberta, `app/docs/RELATO.md:1540`. |
| JUSTIFICATIVA-PAGA-O-PERIODO | PARADA | **nao achei**. A raia limita a justificativa ao marco/período e leva pagamento parcial à porta; motivo registrado: a condição literal do DIFF falhou porque motor/folha não liam o parcial; o aval posterior concedeu `!` e exigiu novo DIFF, não prova pouso (`app/docs/RELATO.md:2553`, `app/docs/BACKLOG.md:1061`). |
| O261-PAI-TERMINAL | PARADA | **nao achei**. A raia religa disputa com pai terminal a pai vivo; o main ainda retorna a disputa aberta sem examinar o pai (`app/chamados/services/disputa_emissao.py:535`, retorno `:566`), e declara que nascimento com pai/destino terminal não passa pela proteção existente (`app/chamados/services/fio.py:755`); motivo: religar por disputa mistura competências/exportada, `app/docs/RELATO.md:2218`. |
| ATA-X-TURNO-PAUSA | PARADA | **nao achei**. A raia acende pares de pausa pelo juiz e acusa `l084_pausa` no motor/CxR; motivo mais recente encontrado: última da fila Codex e nada aberto (`app/docs/RELATO.md:753`), com retomada subordinada a 10/10 e dinheiro igual (`app/docs/RELATO.md:847`); prova histórica dos bloqueios `app/docs/RELATO.md:2975`. |
| LASTRO-DEGRAU-2 | PARADA | **nao achei**. A raia troca a tolerância própria pelo julgamento da grade/ata composta; o main ainda casa por tipo e distância linear em `app/ponto/services/lastro.py:307`; motivo: grade responde diferente conforme janela, cura serializada após grupo (1), `app/docs/RELATO.md:2393`, causas medidas `:2379`. |
| JUIZ-DO-PAR-DUAS-ENTRADAS | **ABSORVIDA-POR JUIZ-DO-PAR-CURA-NA-FONTE** | `app/docs/RELATO.md:2486` liga explicitamente a raia 34b0d5a5 ao redesenho na fonte; pouso declarado em `app/docs/RELATO.md:2446` e `:2449`; produção `app/escala/utils.py:596`, `:613` e `:619` deriva os papéis, alinha e produz células, com implementação em `:502`. A raia fazia a ata sentar batidas pelo papel; o main realiza isso no `_match_marcos`, compartilhado com outros leitores; resíduo 573 explicitamente separado em `app/docs/RELATO.md:2455`. |
| BUG-ENC-CELULAS-FORMA | PARADA | **nao achei**. A raia corrige a alimentação de `dias_encerrados` e deixa RED da via singular; o main ainda repassa `celulas` cru ao pareador em `app/ponto/turnos.py:2150`; motivo: estacionamento para liberar F2, com cura do vigia, bool/pad, suite e impacto incompletos em `app/docs/RELATO.md:7541`. |
| GRADE-FOLGA-DO-VINCULO | **ABSORVIDA-POR GRADE-FOLGA-JUIZ**, com recorte explícito | Núcleo: produção `app/escala/utils.py:1149` pergunta ao juiz; `app/ponto/precedencia.py:237` aceita folga manual e `:239` restringe calendário ao vínculo do dia. Associação registrada em `app/docs/RELATO.md:1600` (desenho GRADE-v5 respondido por GRADE-FOLGA-JUIZ), pouso `app/docs/RELATO.md:7508`, confirmado como main com juiz em `:7537`. **Delimitar**: a raia anexa inclui `eh_trabalho` de calendário e fase_conflitante; foram retirados expressamente: `app/docs/RELATO.md:1590`, `app/docs/BACKLOG.md:850`, `app/docs/BACKLOG.md:826`. Hoje o campo `eh_trabalho` morreu pela lavra em `app/colaboradores/services/calendario.py:89`; fase_conflitante ainda lê foto própria em `app/escala/services/fase_conflitante.py:74`. Portanto absorção do núcleo vigente; não afirmar que todos os deltas da raia pousaram. |

**O261 exige cuidado:** `app/docs/BACKLOG.md:874` anuncia absorção de FIO-MORTO-PELO-RETRATO (2)(3)(4) pela O261-E, mas F3 aparece parada na mesma linha; a proteção de encerramento em `app/chamados/models.py:374` não é regra de nascimento. A declaração `app/chamados/services/fio.py:755` impede usar esse plano como prova de obra realizada.

**Raias aparentemente mal atribuídas:** para JUIZ-DO-PAR-DUAS-ENTRADAS, a **segunda** anexada, `wt-atapausa-092038`, é da ATA-X-TURNO-PAUSA pelos commits fornecidos [nao verificado]; o main também distingue as duas classes em `app/docs/RELATO.md:2530` (“cópia antiga 383adb81 ... é a classe da pausa ... não portada”). A primeira, `wt-juizpar-2810-1850`, é da obra (`app/docs/RELATO.md:2520`). As demais 1–10 têm vínculo nominal/registro compatível nas provas acima; histórico completo das refs [nao verificado].

Arquivos **literalmente ausentes** conferidos (não confundir com todo delta do stat): quarentena.py; testes quarentena e ambos silêncio; `_quarentenas.html`; ambos testes COBPARC relógio; três testes novos justificativa; teste pai terminal; três testes ATA pausa/juiz uma entrada; teste LASTRO degrau2; testes BUG-ENC e E4-CARGA [nao verificado quanto a arquivo:linha de ausência]. Os arquivos de produção modificados do stat já existem, mas o delta não está provado como pousado. [nao verificado]

## 12/11 14:22 · sessao 7e5d0ea3b9d0 · - · - · origem=Ronald (tmux)

Conferência das 10 obras, somente leitura. A cópia não tem `.git`; branches, refs e identidade dos blobs das raias não puderam ser conferidos. [nao verificado]

| Obra | Classe sugerida | Prova no main e síntese |
|---|---|---|
| O275 | PARADA | **não achei** o campo `feriado_e_dia_trabalho`; o leitor atual começa em `app/escala/models.py:1252`. A raia cria cadastro do tipo para tornar feriado previsto; pouso retido por FORMA A × B, segundo `app/docs/RELATO-ARQUIVO.md:753` e `app/docs/RELATO-ARQUIVO.md:755`; trava dupla explicitada em `app/docs/RELATO-ARQUIVO.md:1486`. |
| O276 | NO-MAIN-COM-OUTRO-NOME | `app/ponto/turnos.py:1430` e `app/ponto/turnos.py:1447`: fecho em fluxo pergunta teto do dia; ultrapassado, cria turno aberto e saída órfã em `app/ponto/turnos.py:1453`. Pouso sob **R1-H-PORTAO-DE-26H** documentado em `app/docs/RELATO.md:3293`. **Ressalva:** main aplica exceção L-084, devolvendo teto de 26 h quando ambas as pontas estão deslocadas (`app/ponto/turnos.py:1435`); não é reprodução literal irrestrita da raia. |
| V3-RETOMA-FILA | PARADA | **não achei** o lado v2 obtido da lavra: main ainda monta `m_dia` por laço próprio em `app/ponto/management/commands/diff_calculador.py:245`; CCT continua calculada com `ini` e template do chamador em `app/ponto/calculador/alimentacao.py:389`. Motivo registrado: instrumento pousa depois da V3, `app/docs/RELATO.md:1707`. |
| V3-RETOMA-PERIODOS-DA-CELULA | PARADA | **não achei** a regra completa: main ainda preenche `pares_por_dia` com segmentos em `app/ponto/calculador/alimentacao.py:158`, e declara/continua quando período não tem dia ou carimbo em `app/ponto/calculador/alimentacao.py:79`, em vez da recusa da raia. Motivo: dependência da pista, P4 aberto e base atrasada, `app/docs/RELATO.md:7328`. **P5 isolada foi refeita e pousou** sob outra obra, `app/docs/RELATO.md:7408`, mas isso não absorve a obra inteira. |
| V3-E1 | PARADA | **não achei** os cinco arquivos do stat. É bateria/arnês, não implementação de produção; resultado registrado da validação tem 14 falhas de status em 11 casos, `app/docs/RELATO.md:1805`; registro anterior de não pousar junto à v2 e necessidade de E1 validado em `app/docs/RELATO.md:1815`. Motivo mais recente integral: **não sei**; a linha de validação não declara o destino posterior do arnês. |
| E3-ISENTO-INSUMOS | ABSORVIDA-POR ISENTO-H10 | `app/docs/BACKLOG.md:934` diz explicitamente SUPERADA por ISENTO-H10 e que `c9f16a60` vira sua base; pouso em `app/docs/BACKLOG.md:924`. Produção `app/ponto/services/cartorio.py:548` acrescenta `isento:1` com isenção e `regra >= 4`; o horizonte substitui a aplicação atemporal da raia. |
| CASA-ESCALA-E4 | NO-MAIN-COM-OUTRO-NOME | `app/colaboradores/queries.py:64` usa `q_vinculo_cobre_a_data`; dinheiro usa a juíza em `app/folha/previa.py:81`, `app/ponto/services/fechamento.py:229`; carregamento em `app/ponto/nucleo.py:15`. Parte 5 pousada e dez cópias migradas documentadas em `app/docs/RELATO.md:3384` e `app/docs/RELATO.md:3386`. Mesmo comportamento em main, com nomes dos juízes. |
| QUATRO-PRAZOS | NO-MAIN-COM-OUTRO-NOME | `app/ponto/turnos.py:3186`, `app/ponto/turnos.py:3199`, `app/ponto/turnos.py:3229`; chamadas reais `app/ponto/management/commands/processar_alertas_avancados.py:63` e `app/ponto/management/commands/processar_alertas_turno.py:118`. Gate do detectar pergunta autoridades em `app/ponto/management/commands/detectar_ausencias.py:471` e `app/ponto/management/commands/detectar_ausencias.py:474`. Pousos das partes anexadas: `app/docs/RELATO.md:3350` e `app/docs/RELATO.md:3406`. |
| JUIZ-VARREDURA-1 | NO-MAIN-COM-OUTRO-NOME | Cron atual apenas conta e alarma em `app/ponto/management/commands/reavaliar_ausencias_feriado.py:35` e `app/ponto/management/commands/reavaliar_ausencias_feriado.py:43`; pouso posterior como **JUIZ-1 v5** em `app/docs/RELATO.md:1297`, registro NO AR em `app/docs/RELATO.md:1294`. A raia anexada inclui também JV4A, mas contém a obra JV1; não é raia alheia. [nao verificado quanto aos blobs] |
| JUIZ-VARREDURA-3 | ABSORVIDA-POR C20-JUIZ-3 | `app/docs/RELATO.md:7859` a `app/docs/RELATO.md:7863` descrevem pouso C20-JUIZ-3 e nomeiam o cron vigia, outbox e receiver; `app/docs/BACKLOG.md:1816` registra NO AR. Produção: consumidor chama retratação da premissa em `app/chamados/reconciliador.py:1373`; cron só conta e alarma em `app/chamados/management/commands/reconciliar_chamados.py:118`. |

Arquivos do stat para as quatro PARADAS, distinguindo **ausentes fisicamente** de alterações de raia em arquivos já existentes:

- **O275:** ausentes `app/escala/management/commands/vinculos_tipo_feriado_contra_vinculo.py`, `app/escala/migrations/0043_tipoescala_feriado_e_dia_trabalho.py`, `app/escala/tests/test_o275_sdf_feriado_e_dia.py`, `app/escala/tests/test_tripwire_o275_leitor_unico_do_flag.py`. [nao verificado: ausência não tem linha] Outros arquivos do stat já existem: `app/config/crons.py`, `app/escala/{models.py,services/cadastro_tipo.py,views_wizard.py}`, `app/templates/escala/{_wizard_preview.html,tipos_lista.html,wizard_tipo_escala.html}`; identidade das alterações [nao verificado].
- **V3-RETOMA-FILA:** ausentes `app/ponto/tests/test_diff_calculador_lado_v2_e_a_lavra.py` e `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py`. [nao verificado: ausência não tem linha] Os dois arquivos de produção do stat existem, mas conservam regra anterior nas linhas citadas acima.
- **V3-RETOMA-PERIODOS-DA-CELULA:** ausentes `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py`, `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py`, `app/ponto/tests/test_v3_periodos_da_celula.py`. [nao verificado: ausência não tem linha] Existem os quatro arquivos de produção do stat e `test_s5b_split_noturno_tem_um_dono.py`, `test_v3_parei_p5_parte_menor_que_todo.py`; a existência do último decorre da P5 posterior, sem comprovar toda a regra (`app/docs/RELATO.md:7408`).
- **V3-E1:** ausentes todos os cinco: `app/docs/E1-PERGUNTA-AO-DP-CONTADOR.md`, `app/ponto/tests/{test_e1_lote1_valor_do_dia.py,test_e1_lote2_status_do_dia.py,test_e1_lote3.py,test_v3_e1_valor.py}`. [nao verificado: ausência não tem linha]

Todos os primeiros diffs deste grupo foram truncados a 5.000 bytes no dado fornecido; os corpos posteriores ao corte não são visíveis e a identidade completa das alterações não é verificável sem refs. [nao verificado] Não encontrei raia evidentemente alheia neste grupo; `wt-jv4a-033135` empilha JV4A sobre JV1, e `wt-v3p5v2-cura4-220249` empilha P5 sobre V3-PERIODOS, conforme os commits anexados. [nao verificado] [nao verificado]

Contagem deste grupo: NO-MAIN-COM-OUTRO-NOME **4**, ABSORVIDA-POR **2**, MORTA-POR-CORTE **0**, PARADA **4** = **10**. [nao verificado]

## 12/11 14:22 · sessao db63305967d7 · - · - · origem=Ronald (tmux)

Classificações recomendadas das 9:

| Obra | Classe | Prova e conclusão |
|---|---|---|
| F1C4-3-RG-SEM-CELULA-CASO | NO-MAIN-COM-OUTRO-NOME | `app/ponto/management/commands/reconciliar_grade.py:281-282`: sob F1C4-3-NONE-COBRA, só inclui dia indefinido nos mortos se existe célula e o juiz diz sem trabalho; sem célula não retrata. A raia acrescenta apenas o caso de teste. [nao verificado: conteúdo integral da raia] |
| O10-C | PARADA | Não achei o lote no main; motivo `app/docs/RELATO.md:1088-1094`: SOLO exige lista do admin e porta de UI alcançável, sai do comboio e precisa da volta 2. |
| DEPLOY-[nome]-ELO | ABSORVIDA-POR DEPLOY-RELOAD-GRACIOSO | `app/docs/BACKLOG.md:930`, `app/docs/RELATO.md:1513`; código `bin/deploy.sh:110-117`, `bin/deploy.sh:338-359`: preserva o elo usando HUP e recusa restart com cron vivo; substituiu o mecanismo antigo de admissão. |
| SUITE-POR-IMPACTO | PARADA | Não achei seleção por cobertura; motivo `app/docs/RELATO.md:1385-1387`: mapa falhou, replay/PRONTO 5 permanece fila; rebase ainda em cópia em `app/docs/RELATO.md:1399`. |
| PADRAO-DE-CIMA | PARADA | Não achei agrupamento de DESENHO por entidade; motivo `app/docs/RELATO.md:1595`, `app/docs/RELATO.md:7310`: 9 voltas, opus reprovou, volta 10 cancelada e obra estacionada. Não há prova de proibição definitiva. |
| CODEX-CONSTRUTOR-PILOTO | ABSORVIDA-POR CODEX-CONSTRUTOR-NEGOCIO | Para o **instrumento anexado**, `app/docs/RELATO.md:1127` declara generalização/rebase do papel antes não pousado, `app/docs/RELATO.md:873` e `app/docs/BACKLOG.md:1780` registram pouso v5; produção `bin/codex_pergunta.sh:197-204`, `bin/codex_pergunta.sh:217-238`, `bin/codex_pergunta.sh:253`, `bin/codex_pergunta.sh:415-419` valida cópia e restringe escrita a WT/app. A tela POSICAO-DO-DESLIGAMENTO não está provada por essa raia; `app/docs/RELATO.md:2239` diz que a WT do piloto ainda não existia. |
| CODEX-CONTEXTO-DA-CASA | PARADA | Não achei gerador de AGENTS/validador do brief; motivo `app/docs/BACKLOG.md:1020`: pouso retido para não recusar briefs de construtores em voo; complemento ainda posterior em `app/docs/RELATO.md:1743`. |
| OTIMIZA-1 | PARADA | Não achei medida inteiras/pouso: `bin/gerar_estado.py:265-270` passa da recusa diretamente ao Codex; `bin/trava_teste.sh:40-42` executa comando sem gravar suites.log. Motivo `app/docs/RELATO.md:1395`: faltam suite inteira, replay PRONTO 5, verificador e pouso próprio. |
| FIO-ESTADO-LE-O-ALCANCE | NO-MAIN-COM-OUTRO-NOME | Reimplementação v2 em `_ler_carimbo` e `linha`: `bin/fio_estado.py:137-156`, `bin/fio_estado.py:175-189`; valida alcance/cegueira/universo em `bin/fio_estado.py:74-95`; integrado em `bin/gerar_estado.py:260-261`. `app/docs/BACKLOG.md:966` registra pouso próprio 9453c4f6 sob o mesmo ID, portanto a premissa de ausência de pouso conflita com o main. A validação de soma das partições mostrada na raia antiga não existe nessa v2; contrato revisto em `app/docs/RELATO.md:1787`. |

Arquivos das PARADAS, conforme os `--stat` fornecidos, sem poder conferir os blobs das raias nesta cópia sem `.git`: **[nao verificado]**.

- **O10-C:** `app/config/crons.py`; `app/ponto/management/commands/trocas_plantao_lote_o10c.py`; `app/ponto/tests/test_o10c_lote_trocas.py`.
- **SUITE-POR-IMPACTO**, primeira raia: `bin/pre-push.sh`, `bin/recursos.sh`, `bin/regua.sh`, `bin/suite.sh`, `bin/suite_impacto.py`, `bin/suite_impacto.sh`, `bin/suite_impacto/{contexto_modulo.py,coveragerc,extrair_mapa.py,selos_so_na_principal.txt,sitecustomize.py}`, `bin/tests/{test_porta_le_env_da_casa.sh,test_suite_impacto_certificado.sh,test_suite_impacto_selecao.sh}`. Segunda raia também traz `bin/gerar_estado.py`, `bin/pista_estado.py`, `bin/trava_teste.sh`, `bin/tests/{test_medida_suites_por_pouso.sh,test_selo_espera_processo_morde.sh,test_suite_sh.sh,test_trava_teste.sh}`.
- **PADRAO-DE-CIMA**, raia pertinente `wt-pdc-v3-170430`: `bin/codex_pergunta.sh`, `bin/disjuntor.py`, `bin/hook_stop_fila1.py`, `bin/padrao_vocab.py`, `bin/tests/{_mordida.sh,test_disjuntor.sh,test_hook_esperando_com_prova.sh,test_lei_akita.sh,test_padrao_de_cima.sh}`. A primeira raia, aparentemente de outra obra, traz `.claude/settings.json`, `CLAUDE.md`, `app/docs/TICKETS.md`, `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt`, `bin/{arvore_hash.sh,codex_pergunta.sh,contrato_do_push.py,disjuntor.py,hook_stop_fila1.py,marco_de_clear.py,pre-push.sh,suite.sh}`, `bin/tests/{test_contrato_do_push.sh,test_disjuntor.sh,test_handoff_sessao.sh,test_hora_da_secao_nao_e_futura.sh}`.
- **CODEX-CONTEXTO-DA-CASA:** `AGENTS.md`, `bin/{codex_brief_verificador.py,codex_limite_estado.py,codex_pergunta.sh,gerar_agents_md.py,relato.sh}`, `bin/tests/{test_agents_md_gerado.sh,test_codex_brief_verificador.sh}`.
- **OTIMIZA-1:** os 21 arquivos da segunda raia de SUITE-POR-IMPACTO, enumerados acima. [nao verificado]

**Raia aparentemente errada:** PADRAO-DE-CIMA → `wt-contencao-v4-111700`: `app/docs/BACKLOG.md:870` identifica precisamente seu head `3a743cdd` como **CONTENCAO-QUE-MORDE-V2**; a raia própria do PDC é identificada em `app/docs/RELATO.md:1595`.

**Soma deste recorte:** 2 NO-MAIN-COM-OUTRO-NOME + 2 ABSORVIDA-POR + 0 MORTA-POR-CORTE + 5 PARADA = **9**. [nao verificado: consolidação das outras 20 pelo agente principal]

## 12/11 14:17 · sessao de60d27cadd0 · QUARENTENA-REABRE · censo · origem=Code

O censo encontra **três divergências de sentido para DECISAO**. A divergência do silêncio vencido já está resolvida pelo corte existente: o código ainda precisa migrar. A fila devolve o vencido; verbos o transforma em `HISTORICO`; a lista do colaborador exige silêncio nulo. (`app/docs/CORTES.md:335`; `app/chamados/services/painel.py:190`; `app/chamados/services/verbos.py:51`; `app/chamados/services/janela_colab.py:73`)

A obra continua registrada como reprovada e aguardando reaplicação, com as curas posteriores nomeadas. (`app/docs/BACKLOG.md:765`)

Esta foi uma leitura estática, sem execução de suite, banco ou DIFF de frota. Resultados de execução desses verificadores: [nao verificado]. A identidade Git do arquivo recebido não pôde ser conferida por histórico Git nesta cópia: [nao verificado]. [nao verificado]

**1. Sítio a sítio do diff antigo**

Nesta tabela, os caminhos antigos são relativos a `app/` e vêm do anexo, sem confirmação histórica independente: [nao verificado]. As referências da terceira coluna são da cópia atual.

“Existe” significa que o sítio foi localizado; **não significa igualdade integral do corpo**. “Alterado” identifica evolução relevante. Para adições da especificação que não foram localizadas, registro “não localizado”: ausência atual não prova que o sítio nasceu e depois morreu. [nao verificado]

| Arquivo antigo::função/bloco — hunk antigo | Estado | Sítio no HEAD | O que o diff antigo fazia |
|---|---|---|---|
| `chamados/management/commands/arquivar_competencia_encerrada.py::Command.handle` — `-97` | Existe; guarda preservada | `app/chamados/management/commands/arquivar_competencia_encerrada.py:100` | Documentava `IS NULL` como “nunca teve véu”, evitando reaquivar vencido. |
| `chamados/management/commands/arquivar_prescritos.py::Command.handle` — `-50`, `-89` | Existem os dois blocos | `app/chamados/management/commands/arquivar_prescritos.py:53`; `app/chamados/management/commands/arquivar_prescritos.py:93` | Mesma explicação na seleção e na releitura antes da escrita. |
| `chamados/management/commands/arquivar_sem_dia_irrecuperavel.py::Command.handle` — `-55` | Existe | `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:58` | Preservava a guarda “nunca teve véu”. |
| `chamados/management/commands/auditar_invariantes_chamados.py::Command.handle/I5` — `-641` | Existe | `app/chamados/management/commands/auditar_invariantes_chamados.py:645` | Substituía comparação própria por `ch.silenciado_em(agora)`. |
| `chamados/management/commands/silenciar_chamados_isentos.py::Command` — arquivo | Existe; transformação ainda por fazer | `app/chamados/management/commands/silenciar_chamados_isentos.py:25`; `app/chamados/management/commands/silenciar_chamados_isentos.py:49` | Retirava `--apply` e escrita; passava a vigia da janela. |
| `chamados/models.py::ChamadoColaborador.silenciado_em` — `-303` | Existe, deslocado no arquivo | `app/chamados/models.py:508` | Acrescentava documentação do predicado por instância; a expressão continua `campo > agora`. |
| `chamados/models.py::QuarentenaColab/QuarentenaDiaDevolvido` — `-2667` | Adições não localizadas [nao verificado] | Registro atual da família: `app/core/portas.py:58`; obra pendente: `app/docs/BACKLOG.md:765` | Criava janela, causa, origem sem trilha, levantamento e exceção por dia devolvido. |
| `chamados/services/aviso_pendencia.py::perguntas_pendentes_do_colab` — `-10` | **Responsabilidade mudou de lugar**; wrapper permanece | `app/chamados/services/aviso_pendencia.py:7` → `app/chamados/services/pergunta_no_app.py:61` | Filtrava perguntas pela janela. Hoje a seleção pendente vem de `consulta_do_app`. |
| `chamados/services/cobrar_dia.py::cobrar_dia` — `-26` | Existe, **alterado** | `app/chamados/services/cobrar_dia.py:31`; `app/chamados/services/cobrar_dia.py:39` | Devolvia o dia e julgava em savepoint, desfazendo devolução/trilha sem escala. Hoje há toque registrado antes do cartório. |
| `chamados/services/disputa_emissao.py::gerar_perguntas_disputa` — `-289` | Existe, alterado | `app/chamados/services/disputa_emissao.py:300` | Carregava/cacheava janelas uma vez por disputa/rodada. |
| Mesmo sítio — `-386` | Existe, **fronteira alterada** | `app/chamados/services/disputa_emissao.py:421`; `app/chamados/services/disputa_emissao.py:439`; `app/chamados/services/disputa_emissao.py:444` | Barrava a pergunta isolada antes de abrir. Hoje há portas separadas de conversa e fato. |
| `chamados/services/disputa_emissao.py::criar_ou_atualizar_disputa` — `-594` | Existe | `app/chamados/services/disputa_emissao.py:634`; `app/chamados/services/disputa_emissao.py:662` | Acrescentava savepoint condicionado à janela e rollback da rodada inteiramente isolada. |
| Mesmo hunk::`_TudoIsolado` | Adição não localizada [nao verificado] | Fluxo atual: `app/chamados/services/disputa_emissao.py:669` | Sinalizava rollback sem disputa/agregação/push. |
| `chamados/services/disputa_emissao.py::_criar_pergunta_se_couber` — `-762` | Existe, deslocado | `app/chamados/services/disputa_emissao.py:823` | Perguntava ao juiz antes de abrir pergunta factual. |
| `chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos` — `-860` | Existe, **alterado** | `app/chamados/services/disputa_emissao.py:1027`; `app/chamados/services/disputa_emissao.py:1045`; `app/chamados/services/disputa_emissao.py:1059` | Filtrava isolados na entrada. Hoje já filtra lista do admin e trava o conjunto do repasse. |
| `chamados/services/fila.py::imports` — `-30` | Existe | `app/chamados/services/fila.py:33` | Importava formas positiva/negativa do silêncio. |
| `chamados/services/fila.py::ROTULO_EXTRA` — `-42` | Existe | `app/chamados/services/fila.py:42` | Trocava histórico de vencido por quarentena. |
| `chamados/services/fila.py::por_verbo` — `-50` | Existe | `app/chamados/services/fila.py:53` | Propagava `agora` à partição. |
| `chamados/services/fila.py::idade_e_vazao` — `-89` | Existe | `app/chamados/services/fila.py:91` | Passava o mesmo instante à fila. |
| Mesmo sítio — `-119` | Existe | `app/chamados/services/fila.py:122`; `app/chamados/services/fila.py:126` | Contava arquivados/registrados pelas metades do juiz do silêncio. |
| Mesmo sítio — `-138` | Existe | `app/chamados/services/fila.py:141` | Passava o instante também a `por_verbo`. |
| `chamados/services/janela_colab.py::chamados_visiveis_colab` — `-48` | Existe | `app/chamados/services/janela_colab.py:49` | Trocava `IS NULL` por silêncio vigente e excluía quarentena. |
| `chamados/services/materializacao.py::_ressuscitar_por_resposta` — `-653` | Existe, alterado ao redor | `app/chamados/services/materializacao.py:700`; `app/chamados/services/materializacao.py:718` | Resgatava somente silêncio vigente, evitando trilha falsa para vencido. |
| `chamados/services/painel.py::etag_do_placar` — `-82` | Existe | `app/chamados/services/painel.py:79` | Corrigia a documentação da expiração. |
| `chamados/services/painel.py::qs_na_fila` — `-155`, `-167` | Existe; `agora` **já incorporado pela E2** | `app/chamados/services/painel.py:158`; `app/chamados/services/painel.py:186` | Acrescentava relógio recebido e documentava retorno do vencido. |
| Mesmo sítio + helpers — `-184` | Fila existe; helpers novos não localizados [nao verificado] | `app/chamados/services/painel.py:188` | Acrescentava quarentena e extraía `_q_silenciado`, `fora_do_silencio`, `silenciado_agora`. |
| `chamados/services/painel.py::montar_contexto_painel` — `-198`, `-210` | Existem os blocos | `app/chamados/services/painel.py:203`; `app/chamados/services/painel.py:215` | Retirava conta própria de arquivados. |
| Mesmo sítio — `-224` | Existe | `app/chamados/services/painel.py:229` | Fazia arquivados vir da mesma partição da pílula. |
| Mesmo sítio — `-303` | Existe, **alterado** | `app/chamados/services/painel.py:300`; `app/chamados/services/painel.py:339` | Lista padrão passava à fila; drill Arquivados seguia IDs da partição. Hoje também há modo resumo. |
| Mesmo sítio — `-390` | Existe | `app/chamados/services/painel.py:385` | Entregava `silenciado_agora` ao template. |
| `chamados/services/pergunta_no_app.py::CAUSAS` — `-20` | Existe, expandido | `app/chamados/services/pergunta_no_app.py:23` | Acrescentava causa quarentena, dona admin. |
| `chamados/services/pergunta_no_app.py::perguntas_do_app` — `-39` | Existe, **alterado** | `app/chamados/services/pergunta_no_app.py:45` | Filtrava janela; hoje precisa preservar identity map e tranca em lote. |
| `chamados/services/pergunta_no_app.py::retencao` — `-53` | Existe, alterado | `app/chamados/services/pergunta_no_app.py:121` | Quarentena antes de sem-dia/hoje/no-app; exige reaplicação do RED dessa ordem. |
| `chamados/services/pergunta_no_app.py::medir` — `-110` | Existe, alterado | `app/chamados/services/pergunta_no_app.py:178` | Carregava janelas em lote e as passava à retenção. |
| `chamados/services/quarentena.py` — arquivo novo | Não localizado [nao verificado] | Contrato pendente: `app/docs/BACKLOG.md:765` | Criava predicado, lote, porta, trilha, isenção e lista de vigentes. Não há substituto atual comprovado. |
| `chamados/services/silencio_ausencia.py::reconciliar_por_ausencia` — `-77` | Existe | `app/chamados/services/silencio_ausencia.py:81` | Trocava comparação própria por `silenciado_em`. |
| `chamados/services/silencio_isentos.py` — arquivo | Existe; forma antiga | `app/chamados/services/silencio_isentos.py:20`; `app/chamados/services/silencio_isentos.py:34` | Retirava os escritores e introduzia vigia. |
| `chamados/services/verbos.py::constantes` — `-10` | Existe, alterado | `app/chamados/services/verbos.py:13`; `app/chamados/services/verbos.py:18` | Retirava `HISTORICO`, acrescentava `QUARENTENA`. Hoje também há `FORA_DA_LISTA_PADRAO`, lida pela série E. |
| `chamados/services/verbos.py::particionar_verbos` — `-24` | Existe; `agora` já recebido | `app/chamados/services/verbos.py:30` | Usava os conjuntos do juiz para silêncio/quarentena, inclusive vivo sem verbo. |
| `chamados/services/vigia_fio.py::documentação/soma` — `-103` | Existe, evoluído | `app/chamados/services/vigia_fio.py:356`; `app/chamados/services/vigia_fio.py:449` | Acrescentava quarentena à partição do universo. |
| Mesmo arquivo::`censo` — `-211`, `-243`, `-263` | Existe, **alterado substancialmente** | `app/chamados/services/vigia_fio.py:318`; `app/chamados/services/vigia_fio.py:395`; `app/chamados/services/vigia_fio.py:428` | Criava contador, carregava janelas e desviava o dia isolado do alarme. Hoje existe segunda passada do dono. |
| Mesmo arquivo::`linhas` — `-385` | Existe, deslocado | `app/chamados/services/vigia_fio.py:853` | Imprimia quarentena sem alarmar. |
| `chamados/signals.py::_isentos_*` — `-197` | **Continuam vivos** | `app/chamados/signals.py:271`; `app/chamados/signals.py:285` | O diff os removia, substituindo escrita no chamado pela janela da flag. |
| `chamados/urls.py::urlpatterns` — `-28` | Bloco existe; rotas novas não localizadas [nao verificado] | `app/chamados/urls.py:26` | Acrescentava lista, abrir, levantar e corrigir início. |
| `chamados/views_arquivar.py::arquivar_lote` — `-123` | Existe | `app/chamados/views_arquivar.py:135` | Documentava guarda “nunca teve véu”. |
| Mesmo arquivo::`busca_arquivados` — `-166` | Existe | `app/chamados/views_arquivar.py:153` | Anotava `arquivado_agora` por instância. |
| Mesmo arquivo::`meus_arquivados` — começo do hunk `-193` | Existe | `app/chamados/views_arquivar.py:185` | Mesma anotação para o colaborador. |
| Mesmo hunk::`_recusa_da_quarentena`, `_empresa_visivel`, `_data_post`, `quarentenas`, `quarentena_abrir/levantar/corrigir_inicio` | Adições não localizadas [nao verificado] | Superfície atual: `app/chamados/views_arquivar.py:24`; contrato: `app/docs/BACKLOG.md:765` | Criava permissão conjunta, escopo, leitura e comandos HTTP da janela. |
| `colaboradores/services/colaborador.py::OPERACOES` — `-31` | Existe | `app/colaboradores/services/colaborador.py:34` | Retirava flag de cadastro e criava operação específica de isenção. |
| `colaboradores/services/ficha.py::ficha_do_colab` — `-157` | Existe, alterado | `app/colaboradores/services/ficha.py:154`; `app/colaboradores/services/ficha.py:160` | Retirava histórico de vencido; hoje lê `FORA_DA_LISTA_PADRAO`. |
| `colaboradores/services/isencao.py` — arquivo novo | Não localizado [nao verificado] | Escritor atual: `app/colaboradores/views.py:176` | Unificava flag, trilha e operação da janela. |
| `colaboradores/views.py::imports` — `-10` | Existe | `app/colaboradores/views.py:183` | Retirava import de auditoria tornado desnecessário com a delegação. |
| Mesmo arquivo::`passe_livre` — `-171` | Existe | `app/colaboradores/views.py:173` | Delegava troca à porta de isenção. |
| `core/juizes.py::_C16` — `-123` | **Identificador ocupado por outra pergunta** | `app/core/juizes.py:131` | Inscrevia quarentena; hoje `_C16` é veto de nascimento/reabertura. |
| Mesmo arquivo::`JUIZES['chamado']` — `-239` | Registro existe, expandido | `app/core/juizes.py:272`; `app/core/juizes.py:288` | Registrava o juiz já cortado de quarentena. Não copiar a chave antiga. |
| Mesmo arquivo::`PENDENTES['tela']` — `-735` | Pendente continua presente | `app/core/juizes.py:792` | Retirava comparação de relógio do template depois da migração. |
| `core/management/commands/lavrar_badge_navbar.py::Command.handle` — `-34`, `-46` | Existem | `app/core/management/commands/lavrar_badge_navbar.py:33`; `app/core/management/commands/lavrar_badge_navbar.py:47` | Atualizava comentários sobre histórico; esses hunks não acrescentavam escrita. |
| `core/portas.py::FAMILIA_DE` — `-69` | Bloco existe; entidades novas não localizadas [nao verificado] | `app/core/portas.py:70` | Incluía as entidades da janela na família chamado. |
| Mesmo arquivo::`PORTAS` — `-238` | Bloco existe; inscrições novas não localizadas [nao verificado] | `app/core/portas.py:237` | Declarava um escritor para ambas as entidades. |
| `ponto/management/commands/alertar_disputas_pendentes.py::Command.handle` — `-39`, `-117` | Existem | `app/ponto/management/commands/alertar_disputas_pendentes.py:42`; `app/ponto/management/commands/alertar_disputas_pendentes.py:118` | Pulava disputa cujas perguntas estivessem todas isoladas e imprimia a contagem. |
| `ponto/management/commands/escalonar_chamados_supervisao.py::Command.handle` — `-37` | Existe | `app/ponto/management/commands/escalonar_chamados_supervisao.py:42` | Recortava candidatos pelas metades de silêncio e quarentena. |
| `templates/chamados/partials/_busca_arquivados.html` — `-20`, `-43` | Existem os dois blocos | `app/templates/chamados/partials/_busca_arquivados.html:20`; `app/templates/chamados/partials/_busca_arquivados.html:46` | Acrescentava acesso à quarentena e consumia `arquivado_agora`. |
| `templates/chamados/partials/_lista_chamados.html` — `-82` | Existe | `app/templates/chamados/partials/_lista_chamados.html:85` | Botão Resgatar passava a ler silêncio vigente. |
| `templates/chamados/partials/_meus_arquivados.html` — `-30` | Existe | `app/templates/chamados/partials/_meus_arquivados.html:33` | Rótulo Arquivado deixava de depender de campo apenas preenchido. |
| `templates/chamados/partials/_quarentenas.html` — arquivo novo | Não localizado [nao verificado] | Obrigação de tela: `app/docs/BACKLOG.md:765` | Formulário, lista, rótulo sem trilha, correção e levantamento. |

**2. Censo pela regra e por AST**

O censo separa cobrança, verbo, mensagem não lida, universo, medição e ciclo. Essa distinção já existe no contrato; não se deve transformar todo leitor de `VIVOS` em leitor da fila. (`app/core/tests/test_tripwire_contagem_de_pendencia.py:66`)

Foram usados dois universos:

- **Varredura ampla:** 811 arquivos Python de `app/`, excluindo componentes `tests`, `migrations`, `docs` e `__pycache__`; zero erro de parse. Resultado desta execução sem artefato persistido: [nao verificado].
- **Tripwire existente:** 759 arquivos; exclui também nomes começando `test` e diretórios de ativos/logs. Encontrou 58 terminais de contagem por status, dois conformes e 25 funções não conformes, todas declaradas. Resultado desta execução sem artefato persistido: [nao verificado]. Algoritmo e exclusões: `app/core/tests/test_tripwire_contagem_de_pendencia.py:289`; `app/core/tests/test_tripwire_contagem_de_pendencia.py:317`.

A diferença entre os universos não significa “52 testes”: o corte por nome também alcança arquivos como `testar_pwa.py` e configurações de teste. Classificação integral desses 52: [nao verificado]. [nao verificado]

Este foi o script da varredura de contagem/chamadas, executado em memória, sem importar Django nem executar classes de teste: [nao verificado]

```python
import ast, pathlib, collections

p = pathlib.Path('app/core/tests/test_tripwire_contagem_de_pendencia.py')
tree = ast.parse(p.read_text())
tree.body = [
    n for n in tree.body
    if not isinstance(n, ast.ClassDef)
    and not (
        isinstance(n, ast.ImportFrom)
        and n.module and n.module.startswith('django')
    )
]
env = {'__file__': str(p.resolve())}
exec(compile(tree, str(p), 'exec'), env)

pairs = list(env['arquivos_de_producao']())
hits = [
    (r, *h) for r, s in pairs
    for h in env['contagens_por_status'](s)
]
print('UNIVERSO', len(pairs), 'CONTAGENS', len(hits),
      'CONFORMES', sum(h[-1] for h in hits))

nc = env['sitios_nao_conformes'](pairs)
for k, v in nc.items():
    print(k, v, env['DECLARADOS'].get(k, ('NOVO', ''))[0])

watch = {
    'qs_na_fila', 'qs_vivos', 'particionar_verbos',
    'perguntas_do_app', 'perguntas_pendentes_do_colab',
    'pergunta_viva', 'fio_vivo', 'gerar_perguntas_disputa',
    'criar_ou_atualizar_disputa', 'perguntar_pela_celula',
    'tarefa_do_dia', 'porta_alcancavel_do_dia',
    '_portas_da_tarefa', 'alimentar_portas', 'alimentar_fila',
    'colab_em_quarentena', 'fora_do_silencio', 'silenciado_agora'
}
for r, s in pairs:
    t = ast.parse(s)
    aliases = {
        a.asname or a.name: a.name
        for n in ast.walk(t) if isinstance(n, ast.ImportFrom)
        for a in n.names
    }
    out = collections.defaultdict(list)

    class V(ast.NodeVisitor):
        def __init__(self):
            self.stack = []
        def visit_ClassDef(self, n):
            self.stack.append(n.name)
            self.generic_visit(n)
            self.stack.pop()
        def visit_FunctionDef(self, n):
            self.stack.append(n.name)
            self.generic_visit(n)
            self.stack.pop()
        visit_AsyncFunctionDef = visit_FunctionDef
        def visit_Call(self, n):
            f = n.func
            name = (
                aliases.get(f.id, f.id) if isinstance(f, ast.Name)
                else f.attr if isinstance(f, ast.Attribute) else ''
            )
            if name in watch:
                out['.'.join(self.stack) or '<modulo>'].append(
                    f'{name}@{n.lineno}')
            self.generic_visit(n)

    V().visit(t)
    for fn, calls in out.items():
        print(r + '::' + fn, ' '.join(calls))
```

O complemento de campos percorreu os 811 arquivos, visitando `Attribute` com contexto `Load/Store`, argumentos nomeados, literais de campos e definições. Encontrou as atribuições explícitas de silêncio na porta `arquivo`; a flag é passada por dicionário e escrita pelo `setattr` da porta genérica. (`app/chamados/services/arquivo.py:166`; `app/chamados/services/arquivo.py:202`; `app/colaboradores/views.py:176`; `app/colaboradores/services/colaborador.py:59`)

Limite: o tripwire não certifica universos recebidos por parâmetro, filtros montados dinamicamente, `getattr` ou listas acumuladas por `append`. O scanner de chamadas reconhece aliases de `ImportFrom` e nomes finais de atributos, sem análise completa de tipos. Completude dinâmica: [nao verificado]. Limites já declarados do tripwire: `app/core/tests/test_tripwire_contagem_de_pendencia.py:31`.

Para interpretar as tabelas seguintes, os estados e seus escritores são:

| Estado lido | Quem o escreve hoje |
|---|---|
| **C — chamado:

[truncado em 20000 de 59441 caracteres]

## 12/11 14:16 · sessao 6cef734b37fa · AUDITORIA-DE-TRAVAS · leitura · origem=Code

**O v4 ainda não fecha o contrato.** Fecha várias ambiguidades do v3, mas a divisão do publicador continua imprecisa, a regra do disjuntor altera propriedades dos selos e a semente contém identidades não demonstradas, pousos em `[ORDEM]` e faltantes declarados como fila 1 aberta. (`contrato_item3_v4.txt:7`, `:11`, `:13`; `contrato_item3_v4_semente.txt:2`, `:9`, `:27`; `app/docs/RELATO.md:265`; `app/docs/BACKLOG.md:1980`)

Conto as linhas físicas dos anexos fornecidos a partir de 1. Nas referências abreviadas, `BACKLOG.md` e `RELATO.md` são os arquivos de `app/docs/`; os scripts e selos mantêm seus nomes. Pousos abaixo são **declarações documentais**; confirmação em produção: **[nao verificado]**.

**1. Furos do v3: o que o v4 fecha**

| Furo | Veredito sobre o v4 |
|---|---|
| **RED × “sem ser editado”** | **Fecha a contradição contratual:** conserva casos/assertivas e autoriza migrar a fixture, inclusive dirigir o leitor ao `.ids` temporário. A passagem efetiva do RED permanece **[nao verificado]**. (`contrato_item3_v4.txt:10`) |
| **Topo com TRAVA** | **Fecha no desenho:** topo travado continua em curso; seu veto deve excluí-lo; aparecem caso e mutante específicos. (`contrato_item3_v4.txt:4`, `:6`, `:9`) |
| **Gramática de TRAVA** | **Fecha:** nome livre, não vazio, aparado e sem `\|`; a enumeração deixa expressamente de ser fechada. (`contrato_item3_v4.txt:2`) |
| **Cauda/zonas do publicador** | **Não fecha operacionalmente:** limites e primeira migração continuam ambíguos; o intervalo manual começa três linhas antes do declarado; ausência no render atual não identifica autoria manual. Detalho no item 2. (`contrato_item3_v4.txt:7`; `BACKLOG.md:2371`, `:2372`, `:2373`; `app/core/juizes.py:1266`) |
| **945/944** | **Fecha a unidade e corrige o número desta cópia.** Recontagem em memória pelas funções atuais: **945 totais, 944 além do topo**. O “depois” ainda depende de semente válida. (`contrato_item3_v4.txt:12`; `bin/hook_stop_fila1.py:231`, `:270`; `BACKLOG.md:712`, `:2042`) |
| **Identidade fora de OBRAS** | **Fecha por mudança explícita da identidade:** qualquer tabela do BACKLOG, nas três seções, com exatamente uma linha. Isso aceita as linhas externas apontadas no v3; não resolve ids sem linha demonstrada. (`contrato_item3_v4.txt:3`; `BACKLOG.md:2404`, `:2407`, `:2408`, `:2417`; identidades ausentes: **[nao verificado]**) |
| **ErroFonte de IO/BACKLOG** | **Fecha a obrigação contratual:** ausência/ilegibilidade das fontes e decode inválido do `.ids` devem virar `ErroFonte`, bloqueando; outras exceções conservam a falha aberta. Implementação dessa conversão: **[nao verificado]**. (`contrato_item3_v4.txt:8`; `bin/hook_stop_fila1.py:1460`) |
| **Leitores omitidos** | **Fecha a enumeração anteriormente apontada:** inclui interrompidas, primeiras leituras do handoff, wrappers, fabricante, publicação e emendas documentais; distingue as perguntas documentais que ficam. A migração efetiva: **[nao verificado]**. (`contrato_item3_v4.txt:6`; `bin/interrompidas_estado.py:93`, `:105`, `:125`; `bin/relato.sh:115`, `:116`) |
| **Selos em conflito** | **Parcial:** declara corretamente a substituição da antiga autoridade da coluna. A promessa sobre o censo é falsa: a regra nova não preserva todas as propriedades, inclusive algumas que ela cita expressamente. Detalho no item 3. (`contrato_item3_v4.txt:11`; `bin/tests/test_hook_estado_e_a_coluna.sh:53`, `:54`, `:55`; `bin/tests/test_censo_antes_da_vaga.sh:542`, `:578`, `:603`) |
| **Semente** | **Não fecha:** corrigiu APP depois de TRIAGEM e trocou a exclusão indevida do delta O169, mas agora mantém esse delta já pousado em `[ORDEM]`; também mantém estados prováveis como desconhecidos e omite obras abertas. (`contrato_item3_v4_semente.txt:9`, `:14`, `:15`, `:16`, `:18`, `:22`; `RELATO.md:265`, `:266`, `:312`, `:8059`; `BACKLOG.md:1980`) |

Há ainda duas travas da semente sem sustentação nas evidências citadas:

- `LASTRO-FAIXA-SEM-DEFAULT` continua com `PERGUNTA-DE-DESENHO-S1`, embora sua linha diga que o aval responde o desenho e o aval fornecido libere B3 condicionado ao censo antes da c17. Essa pergunta não sustenta a trava mantida. (`contrato_item3_v4_semente.txt:6`; `BACKLOG.md:1813`; `aval_respostas_1211_1230.txt:6`)
- `RAIZ-DA-DISPUTA-F0F1 | TRAVA: ESTACIONADA` atribui à RAIZ o estado que a linha atribui a **BO-DISPUTA-SEM-PERGUNTA**. A RAIZ registra F0/F1 prontos, F1 respondida e c22 esperando vaga. (`contrato_item3_v4_semente.txt:11`; `BACKLOG.md:1882`; `RELATO.md:373`)

**2. G1/G2/G3 e preservação**

**A cláusula “TODO o resto” cobre o complemento em abstrato; a seção não define uma partição operacional inequívoca para a primeira migração.** Portanto não cabe afirmar perda inevitável, nem certificar preservação sem perda/duplicação pelo desenho atual. (`contrato_item3_v4.txt:7`)

O gerador inteiro hoje monta cabeçalho, contagem e tabelas por família; depois recupera uma janela antiga, insere-a antes da contagem e reescreve o arquivo. Essa janela começa no marcador antigo, quando presente, e termina em `OBRAS:FIM`; não preserva a cauda manual atual. (`bin/gerar_backlog.py:65`, `:76`, `:93`, `:100`, `:119`, `:127`, `:138`, `:140`)

| Parte | Problema concreto |
|---|---|
| **G1** | A primeira linha `<!--` é a **linha 7**, manual. “Até a primeira linha” precisa significar **antes dela**; a leitura inclusiva contradiz a preservação expressa dessa mesma linha. (`contrato_item3_v4.txt:7`; `BACKLOG.md:7`) |
| **G2** | É um bloco novo; o gerador atual preserva `ORDEM-VIVA-TOPO`, em vez de renderizar o `.ids`. A migração precisa inserir G2 e realizar a exceção expressa da lápide do marcador antigo. (`contrato_item3_v4.txt:7`; `BACKLOG.md:8`; `bin/gerar_backlog.py:119`, `:131`) |
| **G3** | A contagem começa em **2044**; o corpo gerado antigo termina em **2370**. O novo `GERADO:FIM` ainda precisa ser colocado na primeira migração, separando corpo antigo e conteúdo manual. (`BACKLOG.md:2044`, `:2365`, `:2370`; `bin/gerar_backlog.py:93`, `:100`, `:139`; `contrato_item3_v4.txt:7`) |
| **Migração manual** | O intervalo anunciado começa em **2374**, mas o conteúdo manual já começa em **2371**. (`contrato_item3_v4.txt:7`; `BACKLOG.md:2371`, `:2372`, `:2373`) |
| **Critério “render não produz”** | Não distingue manual de gerado obsoleto. A tabela BATIDA continua no BACKLOG, mas `PENDENTES['batida']` hoje está vazio: o render atual legitimamente deixa de produzi-la. Preservar todo negativo desse diff promoveria conteúdo gerado antigo a conteúdo manual. (`BACKLOG.md:2365`, `:2369`, `:2370`; `app/core/juizes.py:1266`; `bin/gerar_backlog.py:76`, `:78`) |
| **Byte a byte** | O mecanismo atual lê texto, remove/recompõe finais de linha e remonta a janela. A promessa nova exige substituir esse mecanismo, além de ampliar a preservação. (`bin/gerar_backlog.py:111`, `:127`, `:139`; `contrato_item3_v4.txt:7`) |

As linhas pedidas que **não caem em G1/G2/G3, OBRAS, CAUDA já delimitada nem no intervalo de migração anunciado** são:

- `SELO-MORDE-DO-PASSIVO-AFIRMA-MAIS-DO-QUE-MEDE`. (`BACKLOG.md:2371`)
- `MARCO-DO-PAR-DE-1490-MIN-NAO-FOI-MEDIDO`. (`BACKLOG.md:2372`)
- `ORFAOS-L-111-NO-FECHAMENTO-E-NO-ESPELHO`. (`BACKLOG.md:2373`)

Elas ficam cobertas **somente pelo comando geral “TODO o resto”**. Logo o defeito é a delimitação/migração incompleta; dizer que estão fora de toda obrigação de preservação seria incorreto. (`contrato_item3_v4.txt:7`)

**3. Disjuntor: propriedades de pertencimento**

**Não preserva todas.** Há quatro mudanças demonstráveis: positivo da célula qualificada, exato acompanhado de variante, ausência do marcador OBRAS e retorno do predicado chamado diretamente. (`contrato_item3_v4.txt:3`, `:4`, `:6`, `:11`; `bin/tests/test_censo_antes_da_vaga.sh:135`, `:293`, `:542`, `:578`, `:603`)

Na tabela, todas as linhas de selo pertencem a `bin/tests/test_censo_antes_da_vaga.sh`.

| Linhas | Propriedade atual | Efeito do v4 |
|---|---|---|
| **112–115** | FECHADA: wrapper rc0 e trilha FORA. | Preservável excluindo o id de `[ORDEM]`; deixa de decorrer da coluna. (`contrato_item3_v4.txt:4`, `:5`) |
| **116–120** | IDs válidos sem relação: rc0/FORA. | Preservável pela ausência exata e ausência de relação. (`contrato_item3_v4.txt:11`) |
| **121–124** | IDs ilegíveis ou vazios são recusados. | Depende de conservar a validação de entrada; o guard não a substitui. (`bin/disjuntor.py:2288`) |
| **125–130; 281–288** | BACKLOG ausente/ilegível/diretório: NAO SEI, recusa nas duas vias. | Coberto pela falha de fonte. (`contrato_item3_v4.txt:8`) |
| **131–135; 289–293** | Ausência de `OBRAS:INICIO`: NAO SEI/recusa. | **Muda:** identidade em qualquer ponto e lista de falhas não exigem esse marcador. Com `.ids` e linhas intactos, retirar só o marcador deixa de quebrar a fonte definida. (`contrato_item3_v4.txt:3`, `:8`) |
| **250–253** | Agent de FECHADA/FILA2 passa sem censo. | Preservável pela exclusão no `.ids`, não pelas palavras da coluna. (`contrato_item3_v4.txt:4`, `:5`) |
| **301–305** | Membro fora da forma do censo, `O9 (C)`: recusa por forma. | Exige conservar a validação posterior ao pertencimento. (`bin/disjuntor.py:1707`, `:1709`) |
| **534–538** | `OBRA-Q` perante `OBRA-Q (SILENCIADO-ATE-X)`, sem brief: rc1/NEGOU/PAREI. | A recusa pode continuar, mas DENTRO passa a NAO SEI. (`contrato_item3_v4.txt:3`, `:4`, `:11`) |
| **539–543** | **Mesma célula qualificada, brief conforme: rc0/APROVADO.** | **Muda para rc1/NEGOU:** o `.ids` precisa conter a célula inteira; `OBRA-Q` não pertence exatamente e cai no guard mesmo com brief conforme. (`contrato_item3_v4.txt:3`, `:4`, `:11`) |
| **544–548** | `O9` perante `O9 (C)`: NAO SEI; nunca FORA. | Intenção expressamente coberta pelo guard. (`contrato_item3_v4.txt:11`) |
| **549–551** | Caixa trocada: recusa. | Intenção expressamente coberta. (`contrato_item3_v4.txt:11`) |
| **552–555** | `OBRA` não casa `OBRA-Q`/`OBRA-E3`: rc0/FORA. | Depende de conservar a fronteira inteira, incluindo hífen; “token/caixa” não define essa gramática. (`contrato_item3_v4.txt:11`; `bin/hook_stop_fila1.py:110`, `:117`) |
| **556–558** | ID encontrado apenas na cauda: NAO SEI. | Não garantido: o texto novo não define se token interno da cauda conta. Hoje a busca alcança o id inteiro dentro da célula. (`contrato_item3_v4.txt:11`; `bin/disjuntor.py:1689`) |
| **559–563** | Agent com pedido qualificado ou nu: recusa com PAREI da obra normalizada. | Recusa preservável; classificação DENTRO muda para NAO SEI e depende da normalização do pedido. (`contrato_item3_v4.txt:11`; `bin/disjuntor.py:1746`) |
| **564–566** | Agent `O9` contra `O9 (C)`: nega e escreve “nao sei”. | Intenção coberta. (`contrato_item3_v4.txt:11`) |
| **567–574** | Dois candidatos qualificados: rc1/NEGOU, nunca FORA. | Preservável quando algum candidato estiver em `[ORDEM]`; essa condição passa a integrar o guard. (`contrato_item3_v4.txt:11`) |
| **575–578** | **`O9` exato mais `O9 (C)`: NAO SEI mesmo com brief conforme.** | **Muda para rc0/APROVADO:** `pertence(O9)=True` impede executar o guard, que exige **não pertencer**. IDs distintos não violam unicidade exata. (`contrato_item3_v4.txt:3`, `:4`, `:11`) |
| **579–582** | `O9` sozinho, conforme: rc0. | Preservável. (`contrato_item3_v4.txt:4`) |
| **583–591** | Espelho no Agent: pedido qualificado contra célula nua gera NAO SEI/PAREI, nunca FORA. | Não garantido: exige comparação nos dois sentidos, explicitada hoje e ausente da fórmula “token/caixa”. (`contrato_item3_v4.txt:11`; `bin/disjuntor.py:1677`, `:1689`) |
| **592–593** | Espelhos no wrapper: rc1 por forma, antes do pertencimento. | Preservável conservando validação; esse controle não prova o guard. (`bin/disjuntor.py:2288`) |
| **596–608** | **Três espelhos chamados diretamente em `_pertence_fila1` precisam lançar `ValueError`; False também reprova.** | **Muda literalmente para False** se esse predicado virar pertencimento exato. Um guard externo não preserva essa propriedade da função. (`contrato_item3_v4.txt:4`, `:6`, `:11`) |
| **609–619** | `O1`/`O142`, nos dois sentidos: False. | Preservável por igualdade exata; guard também precisa respeitar fronteiras. (`contrato_item3_v4.txt:4`, `:11`; `bin/hook_stop_fila1.py:117`) |
| **621–645** | Negativa conserva ID inteiro; parser e literal do PAREI são únicos. | Continua obrigação independente; o guard não resolve essa propriedade. (`bin/disjuntor.py:1637`; `contrato_item3_v4.txt:11`) |

Também há a propriedade transversal de **duas vias pelo mesmo predicado, com NEGOU e nunca FORA na ambiguidade**. Hoje wrapper e `_censo_nega` usam `_pertence_fila1`; qualquer classificação nova precisa alcançar ambas as vias. (`bin/tests/test_censo_antes_da_vaga.sh:516`, `:519`, `:524`, `:527`; `bin/disjuntor.py:1707`, `:2306`, `:2315`)

Os mutantes de célula crua, exato calando outra, dois candidatos, wrapper próprio, token cru, substring e espelho confirmam que essas obrigações excedem uma troca de fixture. (`bin/tests/test_censo_antes_da_vaga.sh:750`, `:751`, `:752`, `:753`, `:754`, `:757`, `:763`)

**Sim: a regra introduz um segundo classificador de pertinência para decidir a dispensa do censo.** O booleano exato retorna False tanto para variante quanto para id sem relação; distinguir NAO SEI de FORA exige derivar outra relação, por token/caixa, sobre BACKLOG e `[ORDEM]`. O v4 desloca essa classificação para o disjuntor, sem defini-la completamente. (`contrato_item3_v4.txt:4`, `:6`, `:11`; `bin/disjuntor.py:1688`, `:1695`, `:1707`, `:2311`)

**4. Semente: identidade exata e estado documental**

Cada ID abaixo reproduz a primeira célula que deve ser procurada; a coluna seguinte registra o que a linha do BACKLOG declara. (`contrato_item3_v4.txt:3`; `contrato_item3_v4_semente.txt:1–33`)

| Seção | ID exato | Linha e estado declarado |
|---|---|---|
| ORDEM | `SAIDA-NOTURNA-NO-DIA-CERTO` | Linha exata: **[nao verificado]**. Há aval/despacho no RELATO. (`RELATO.md:29`) |
| ORDEM | `QUARENTENA-DO-COLAB` | REPROVADA; espera a vez. Pertencimento literal à fila 1 nessa linha: **[nao verificado]**. (`BACKLOG.md:765`) |
| ORDEM | `BUG-CADASTRO-NAO-ALCANCA-O-FUTURO` | **POS-20, frente da fila 2**; construção na próxima vaga. (`BACKLOG.md:2408`) |
| ORDEM | `FIO-E0-V4-E3-LEITORES` | Fila 1, ABERTA, EM OBRA. (`BACKLOG.md:1873`) |
| ORDEM | `LASTRO-FAIXA-SEM-DEFAULT` | Fila 1, ABERTA, c3; aval responde o desenho. (`BACKLOG.md:1813`) |
| ORDEM | `FIO-E0-V4-E4-ESCRITORES` | Fila 1; etapas E4a–E4d; E4a-1 NO AR, sem fechamento da obra inteira. (`BACKLOG.md:1885`) |
| ORDEM | `C20-O168` | Fila 1, espera vaga, ABERTA. (`BACKLOG.md:2008`) |
| ORDEM | `O169-FECHADA` | Fila 1 REGISTRAR; delta próprio em B3/volta 5; cita pouso antigo de C20-O169. **O próprio v5 também pousou no RELATO.** (`BACKLOG.md:1969`; `RELATO.md:265`) | [nao verificado]
| ORDEM | `RAIZ-DA-DISPUTA-F0-CASO-7279` | Fila 1, F0/sonda, frente da F1. (`BACKLOG.md:2417`) |
| ORDEM | `RAIZ-DA-DISPUTA-F0F1` | Fila 1; F0/F1 prontos, F1 respondida, c22 espera vaga. “ESTACIONADA” qualifica BO-DISPUTA. (`BACKLOG.md:1882`) |
| ORDEM | `AUDITORIA-DE-TRAVAS-CORTE` | Instrumento EM CURSO. (`BACKLOG.md:2404`) |
| ORDEM | `BUG-ORFAO-COM-PAR` | ESTRUTURA fila 1; sonda, classe e cura pendentes. (`BACKLOG.md:2418`) |
| ORDEM | `TRIAGEM-OFFLINE-SEM-CAMERA` | REVERTIDA; retorno condicionado à classe e RED. (`BACKLOG.md:1915`) |
| ORDEM | `APP-MENSAGENS-MSG-0` | Fila 1/raia 2, ABERTA, nada construído; depois da TRIAGEM. (`BACKLOG.md:1916`) |
| ORDEM | `BO-0611-05` | Linha própria ainda B3/PARADA. (`BACKLOG.md:1897`) |
| ORDEM | `BO-DISPUTA-SEM-PERGUNTA` | Linha própria registra construtor lançado. (`BACKLOG.md:1898`) |
| ORDEM | `TRAVA-MENOS-FILA-COM-SELO` | Fila 1 instrumento, ABERTA, preverif v1. (`BACKLOG.md:2396`) |
| ORDEM | `MEDE-CLEAR-X-COMPACT` | Fila 1 medida, em curso, ABERTA. (`BACKLOG.md:1796`) |
| ORDEM | `CODEX-QUEIMA-B` | Fila 1 medida, sondas rodando, ABERTA. (`BACKLOG.md:1797`) |
| ORDEM | `C20-FIO-MUDO` | Fila 1, construtor aberto, ABERTA. (`BACKLOG.md:1817`) |
| ORDEM | `E3-X1-FOLGA-NAO-E-SEM-COBERTURA` | Fila 1, ABERTA, construtor após c8b. (`BACKLOG.md:1876`) |
| ORDEM | `SONDA-SEM-GRANT` | Fila 1 instrumento, ABERTA; sondas congeladas até pouso. (`BACKLOG.md:1979`) | [nao verificado]
| ORDEM | `AUDITORIA-0911` | Fila 1 medida, ABERTA; sondas após SONDA-SEM-GRANT. (`BACKLOG.md:1981`) |
| POS-20 | `UI-PAINEL-FILA-OPUS-V5` | POS-20, fila/registrado; pouso citado pertence à tela original. (`BACKLOG.md:2407`) | [nao verificado]
| POS-20 | `AUDITORIA-ITEM6-CONTADOR-124` | Linha exata: **[nao verificado]**. RELATO diz “não construído → fila”, sem classificá-lo como POS-20. (`RELATO.md:9`) |
| POS-20 | `TRIAGEM-FILA-OPUS-V6` | POS-20, ABERTA. (`BACKLOG.md:1815`) |
| POS-20 | `E4B-FILA-OPUS-C14` | POS-20, fila do opus final. (`BACKLOG.md:1896`) |
| POUSADAS | `FIO-E0-V4-E4B` | POUSOU 12/11 08:46, commit 087fe758a. (`BACKLOG.md:1894`) |
| POUSADAS | `FIO-E0-V4-E4A-2` | NO AR, merge 613a51498. (`BACKLOG.md:1893`) |
| POUSADAS | `C20-O169` | POUSADA 05/11, FECHADA. (`BACKLOG.md:1798`) |

Para os **nove `TRAVA: ESTADO-NAO-PROVADO`**, a reconciliação documental é:

| ID | Estado sustentado por BACKLOG + RELATO |
|---|---|
| `BO-0611-05` | **POUSADA.** O RELATO registra merge fb9269ea9 e deploy rc0, superando o retrato B3 da linha própria. (`BACKLOG.md:1897`; `RELATO.md:451`, `:452`, `:8059`) |
| `BO-DISPUTA-SEM-PERGUNTA` | **ESTACIONADA.** c21 não pousa; raia preservada. (`BACKLOG.md:1882`; `RELATO.md:373`) |
| `TRAVA-MENOS-FILA-COM-SELO` | **POUSADA**, sob o nome curto TRAVA-SELO. A ligação entre os nomes aparece no despacho, cópia e artefatos; depois há pouso/deploy. (`BACKLOG.md:2396`; `RELATO.md:8100`, `:8107`, `:312`) | [nao verificado]
| `MEDE-CLEAR-X-COMPACT` | **ABERTA documentalmente.** Há PARTE 1 e resultados, sem fechamento demonstrado nessas evidências. (`BACKLOG.md:1796`; `RELATO.md:7696`, `:7703`) |
| `CODEX-QUEIMA-B` | **ABERTA documentalmente.** Há despacho/resultados intermediários, sem fechamento demonstrado. (`BACKLOG.md:1797`; `RELATO.md:7694`, `:1241`) |
| `C20-FIO-MUDO` | **ABERTA/incompleta.** O RELATO exige manter a linha aberta pelo escopo incompleto; isso não prova ausência de pouso parcial. (`BACKLOG.md:1817`; `RELATO.md:1218`, `:7995`, `:7999`) | [nao verificado]
| `E3-X1-FOLGA-NAO-E-SEM-COBERTURA` | **Fase X1 POUSADA**, com a ressalva literal “X1 6 de 7” e apply pós-pouso; não fecha toda E3-LEITORES. (`BACKLOG.md:1876`; `RELATO.md:266`, `:185`) | [nao verificado]
| `SONDA-SEM-GRANT` | **ABERTA/congelada.** Ainda faltam volta 8, verificações e pouso. (`BACKLOG.md:1979`; `RELATO.md:591`, `:616`) | [nao verificado]
| `AUDITORIA-0911` | **ABERTA.** Medidas A–E ainda faltantes/congeladas. (`BACKLOG.md:1981`; `RELATO.md:596`, `:698`) |

**ID inequívoco em `[ORDEM]` já pousado: `O169-FECHADA`.** É o pouso do próprio delta v5, com merge, deploy, apply de três casos e DRY seguinte vazio; não depende de confundi-lo com C20-O169. (`contrato_item3_v4_semente.txt:9`; `RELATO.md:265`) [nao verificado]

**Faltante explícito: `SONDA-MATRIZ-DE-TRANSICOES`.** O BACKLOG declara fila 1 instrumento **ABERTA**, e a semente não o contém. Também faltam `SEGREDO-ARGV-RESIDUO` e `SOMBRA-CARIMBO-MENTE`, ambos literalmente fila 1 instrumento ABERTA. (`BACKLOG.md:1980`, `:1982`, `:1983`; `contrato_item3_v4_semente.txt:1–34`)

Por fim, a nota da semente erra o endereço de BUG-CADASTRO: **2417 é RAIZ-F0-CASO-7279; BUG-CADASTRO está em 2408**. O aval fornecido escolhe a candidata 4(b) e exige publicar N antes de construir; não declara transferência de POS-20/fila 2 para fila 1. Essa transferência fica **[nao verificado]**. (`contrato_item3_v4_semente.txt:34`; `BACKLOG.md:2417`, `:2408`; `aval_respostas_1211_1230.txt:2`)

_4299 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
