# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 23:08** · publicado em **29/10/2026 23:10** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 23:08 · sessao 30188e4fd539 · o261c2 · leitura · origem=Code

**Religar o pai não separa competências:** o método obtém um pai VIVO e altera somente `DisputaSupervisao.chamado`; todas as perguntas sem destino passam a ancorar nele pelo mesmo fallback. Não há seleção por `data_evento` ou exportação nessa operação. (`app/chamados/models.py:1741–1782`; `app/chamados/services/fio.py:116–117`)

**Exportada e trancada têm efeitos diferentes nesta árvore.** A tranca pode calar uma pergunta; competência somente exportada continua viva. A antiga `dia_em_competencia_exportada` foi removida. (`app/chamados/services/competencia_trancada.py:3–11`; `app/chamados/juizes.py:1504–1528`)

Para ler as tabelas:

- **C6** significa `pergunta_viva`: verifica a competência de **`p.data_evento`** por `janela_atual` e `periodo_trancado`; cala a pergunta se houver tranca, salvo toque humano posterior. **Não consulta exportação vigente.** (`app/chamados/juizes.py:887–907`; `app/chamados/juizes.py:1005–1016`)
- **`q_vivas()` não equivale ao C6:** é um pré-filtro deliberadamente mais amplo. Exclui a via antiga `competencia_trancada` carimbada, mas **não aplica a tranca dinâmica por leitura**. (`app/chamados/models.py:2325–2332`; `app/chamados/models.py:2334–2353`)

Na fila do admin, os caminhos são estes:

| Caminho | Como inclui/lista/conta | Filtro de competência |
|---|---|---|
| `qs_vivos` → `qs_na_fila` | VIVOS; depois somente `COBRANCA`, excluindo `silenciado_ate > now`. Portanto, VIVO sozinho não garante presença na fila. (`app/chamados/services/painel.py:153–155`; `app/chamados/services/painel.py:186–188`) | **Nenhum** por pergunta, exportação ou tranca nesses métodos. |
| Lista principal, resumo e totais | A lista padrão corta por `COBRANCA` e verbos arquivado/histórico; totais usam `qs_na_fila`. (`app/chamados/services/painel.py:210–223`; `app/chamados/services/painel.py:314–338`; `app/chamados/services/painel.py:462–475`) | **Nenhum adicional** por competência. |
| Crédito da pergunta ao pai; verbos cobrar/validar | `creditos_perguntas` busca perguntas por destino **ou pai vivo**; aplica `q_vivas()`, exclui materializada com `VL_SIM` e, sem destino, credita o pai. A partição por verbos consome esses créditos. (`app/chamados/services/raiox.py:54–72`; `app/chamados/services/raiox.py:133–149`; `app/chamados/services/verbos.py:31–59`) | **Não exportação; não tranca dinâmica pelo C6.** |
| Placar, navbar e filas vizinhas | Placar/navbar usam contexto e partição; fila copiloto, inteligência e fila por causa usam `qs_vivos`, `qs_na_fila` ou a mesma partição. (`app/chamados/views.py:840–861`; `app/core/management/commands/lavrar_badge_navbar.py:39–42`; `app/chamados/services/fila.py:53–55`; `app/chamados/services/fila.py:88–100`; `app/chamados/services/na_fila.py:15–24`; `app/chamados/services/fila_por_causa.py:158–161`) | **Nenhum corte adicional** por exportação; herdam as seleções acima. |
| Fio/modal admin | Carrega perguntas das disputas **abertas sem filtrar C6**; nas fechadas, exige C6. Agrupa por `chamado_que_lista`; o modal percorre as anexadas. (`app/chamados/services/fio.py:218–240`; `app/templates/chamados/partials/modal_fio.html:154–155`) | **Não exportação.** Tranca via C6 somente no filtro das disputas fechadas. |
| Destaque e ordenação do fio | `tem_pergunta_viva` exige `.viva` e ação possível; os cards correspondentes sobem. (`app/chamados/services/fio.py:279–281`; `app/chamados/services/fio.py:409–415`) | **Não exportação; tranca via C6.** |
| Detalhe local do pai | Busca disputa aberta do chamado e filtra perguntas pelo juiz exato. (`app/chamados/services/acoes_chamado.py:335–348`) | **Não exportação; tranca via C6.** |
| Contador do modal | Ativos por `qs_na_fila`; disputas por `fechada_em IS NULL`. (`app/chamados/views.py:900–912`) | **Não** pela competência das perguntas. |

Na cobrança e nas portas do colaborador:

| Caminho | Seleção e efeito | Filtro de competência |
|---|---|---|
| Lembrete diário | Seleciona **disputas abertas**, carimba o aviso diário e envia push ao colaborador e agregado à supervisão. (`app/ponto/management/commands/alertar_disputas_pendentes.py:33–35`; `app/ponto/management/commands/alertar_disputas_pendentes.py:48–108`) | **Não exportação nem tranca:** não seleciona pelas perguntas. |
| Push de abrir/agregar disputa | `_enviar_push_disputa_colab` envia webpush/FCM; sua guarda é o contexto de silêncio do push. (`app/chamados/services/disputa_emissao.py:1483–1507`) | **Não** por competência. |
| Prazo do pai religado | Os defaults do novo pai recebem `prazo_sla=agora+20min`. O próprio corpo do religamento salva o vínculo e registra evento, sem chamada de push. (`app/chamados/models.py:1749–1782`; `app/chamados/catalogo/motor.py:45–51`) | **Não** por competência. |
| Cobrança por pessoa/chat | Pendentes = `q_vivas()` + C6 + sem resposta. Conta perguntas/dias, grava prazo padrão de 48h nos pais e envia FCM/webpush. (`app/chamados/services/aviso_pendencia.py:7–13`; `app/chamados/services/cobranca.py:75–95`; `app/chamados/services/cobranca.py:163–187`) | **Não exportação; tranca via C6.** |
| Aviso após texto livre | Enumera essas pendentes e informa que o questionário continua aguardando resposta. (`app/chamados/services/aviso_pendencia.py:16–24`) | **Não exportação; tranca via C6.** |
| Recobrança do questionário | A view exige alguma pergunta sem resposta viva; o serviço conta **todas sem resposta**, renova prazo de 20min e envia push. (`app/chamados/views.py:1413–1425`; `app/chamados/services/disputa_emissao.py:1444–1464`) | **Não exportação.** C6 na decisão da view; o contador do serviço não separa competências nem vitalidade individual. |
| Botão Cobrar individual | Valida chamado/área, limita aviso diário por pessoa e envia FCM. (`app/chamados/views_cobrar.py:24–57`) | **Não:** não consulta a competência das perguntas. |
| Cobrar em massa e prévia | Aplica `muda_a_folha(ch)` antes de agrupar por pessoa. (`app/chamados/views_cobrar.py:68–114`) | **Sim, mas pelo dia do CHAMADO**, obtido do módulo/contexto; **não por `p.data_evento`**. Sem dia, exclui o chamado inteiro. (`app/chamados/services/cobranca_folha.py:29–35`; `app/chamados/services/cobranca_folha.py:54–71`) |
| Escalonamento supervisão | Seleciona pai `disputa_supervisao` em `COBRANCA`, não silenciado e com disputa aberta; cobra por prazo vencido e idade. (`app/ponto/management/commands/escalonar_chamados_supervisao.py:42–59`; `app/ponto/management/commands/escalonar_chamados_supervisao.py:93–198`) | **Não exportação nem tranca das perguntas.** |
| Alerta genérico de SLA | Percorre `qs_na_fila` e aplica regras de prazo/alerta. (`app/chamados/management/commands/alertar_chamados_sla.py:24–52`) | **Nenhum filtro próprio** por competência. |
| `perguntas_do_app` / API GET perguntas | Busca perguntas do colaborador por `q_vivas()` e confirma C6; a API usa essa lista e o pai como fallback da âncora. (`app/chamados/services/pergunta_no_app.py:39–44`; `app/api/views.py:2603–2604`; `app/api/views.py:2620–2646`) | **Não exportação; tranca via C6.** A seleção do helper não exige pai vivo nem disputa aberta. |
| PWA: formulário e POST | Monta fio com `so_janela=True`; formulário exige `p.viva`; POST exige dono e C6. (`app/chamados/views.py:566–569`; `app/templates/chamados/partials/_card_atendimento.html:73–78`; `app/chamados/views.py:1238–1246`) | **Não exportação; tranca via C6.** |
| API POST resposta | Busca por dono, exige C6 e delega ao core. (`app/api/views.py:2680–2698`) | **Não exportação; tranca via C6.** |
| API chamados v2 | Usa janela do colaborador, mas anexa e serializa **todas as perguntas das disputas abertas**, usando destino vivo ou pai. (`app/api/views.py:1015–1016`; `app/api/views.py:1028–1049`; `app/api/views.py:1158–1178`) | **Não exportação; não C6 na seleção dessas perguntas.** |
| Pauta de pessoas cobradas e caladas | `_calados` seleciona `q_vivas()`, sem resposta, disputa aberta, e verifica prazo dos pais; alimenta `linhas_para_a_pauta`. (`app/chamados/services/cobranca.py:228–257`) | **Não exportação; não confirma tranca dinâmica pelo C6.** |

A janela do colaborador **não é uma proteção por competência de `data_evento`**: usa `criado_em` do chamado, com exceção por perguntas `q_vivas()` com destino não nulo. Assim, um pai criado agora pode carregar perguntas antigas anexadas pelo fio. (`app/chamados/services/janela_colab.py:49–74`; `app/chamados/services/fio.py:218–239`)

Na cobrança em massa, o contexto novo do religamento contém apenas `origem`, `disputa` e `pai_anterior`; portanto esse pai pode ser recusado inteiro por `sem_data`. Isso não constitui exclusão seletiva das perguntas exportadas de uma disputa mista. (`app/chamados/models.py:1762`; `app/chamados/services/cobranca_folha.py:54–61`)

Nos caminhos que julgam ou contam:

| Caminho | Julgamento/contagem e filtro |
|---|---|
| Vitalidade, `.viva`, `perguntas_vivas_de`, `fio_vivo` | Usam C6: **tranca por `data_evento`, sem exportação**. (`app/chamados/models.py:2355–2366`; `app/chamados/juizes.py:1097–1111`) |
| `estado` | Chama C6, mas pode retornar `PENDENTE`/`RESPONDIDA` mesmo quando a pergunta está morta sem carimbo; não substitui `.viva`. Sem consulta de exportação. (`app/chamados/models.py:2201–2208`) |
| `respostas_sem_veredito` / `pode_fechar` | A lista exclui dias trancados, salvo `ignorar_tranca=True`; fechar exige nenhuma resposta sem veredito e nenhum fio vivo. **Sem exportação.** (`app/chamados/models.py:1425–1439`) |
| Fila global de validação; classificação/lote/API mensageria | Respondidas com texto, `q_vivas()` e confirmação C6; classificação e contagens consomem essa fila. **Sem filtro adicional de exportação; tranca via C6.** (`app/chamados/juizes.py:1121–1127`; `app/chamados/services/validacao.py:858–880`; `app/api/views_mensageria.py:539–550`) |
| Validação individual e materialização | **Não barram por competência exportada/trancada** nessas portas; a recusa por tranca foi removida. Materialização seleciona respondida + validada + não materializada. (`app/chamados/services/validacao.py:348–376`; `app/chamados/services/materializacao.py:1063–1073`; `app/chamados/services/materializacao.py:1120–1122`; `app/chamados/services/materializacao.py:1286–1296`) |
| Pendentes após materializar | Conta `perguntas_vivas_de` e consulta `pode_fechar`: **C6/tranca, sem exportação**. (`app/chamados/services/materializacao.py:1230–1233`) |
| Juiz do lastro / `veredito_lavrado` | Lavra `_pergunta_materializada_de_fato`; reconhece a **via carimbada** de tranca, mas não é o filtro dinâmico do C6. **Sem exportação.** (`app/chamados/veredito_lavrado.py:219–231`; `app/chamados/juizes.py:603–617`) |
| Cron de lavra stale | Seu universo exige **destino vivo**; a pergunta sem destino ancorada somente no pai não entra por esse filtro. **Sem exportação.** (`app/chamados/management/commands/lavrar_perguntas_stale.py:56–58`) |
| Relatórios de disputa | Classifica disputas abertas pelas perguntas C6: **tranca, sem exportação**. (`app/chamados/relatorios_disputa.py:10–22`; `app/chamados/relatorios_disputa.py:39–50`) |
| `fora_da_folha` / `o_que_segura` | Recortam a competência consultada e classificam fio por perguntas ligadas à célula, usando `.viva`. **Não excluem por exportação; herdam tranca.** (`app/folha/fora_da_folha.py:46–73`; `app/folha/fora_da_folha.py:119–173`; `app/folha/fora_da_folha.py:188–213`) |
| Prévia da folha | Carrega por `data_evento` na competência pedida; prioriza qualquer texto de resposta e, sem texto, usa `fio_vivo`. **Sem exportação; tranca via C6 somente nesse segundo ramo.** (`app/folha/previa.py:64–67`; `app/folha/previa.py:139–146`) |
| Vigias de fio | `viva_em_disputa_fechada` e `viva_sob_pai_terminal` usam `q_vivas()` + C6, **sem exportação**. Após religar, sai especificamente do segundo universo porque o pai está VIVO. (`app/chamados/services/vigia_fio.py:352–362`; `app/chamados/services/vigia_fio.py:401–413`) |

O recorte `_dentro_da_janela_viva`, usado em degraus de vitalidade/lastro, é temporal; não consulta exportação e não equivale a excluir competência exportada. (`app/chamados/juizes.py:517–544`; `app/chamados/juizes.py:616–617`; `app/chamados/juizes.py:1066–1077`)

Quanto ao mecanismo pedido: **não há, no religamento e nos leitores acima, uma máscara por exportação que mantenha fora da fila e da cobrança somente essas perguntas já vivas.** O wrapper exige existir **alguma** viva sem destino e religa a disputa inteira; não particiona as demais. (`app/chamados/models.py:1788–1795`; `app/chamados/services/fio.py:116–117`)

Os mecanismos próximos têm limites concretos:

- **Tranca:** atua individualmente pelo `data_evento`, mas exportada não significa trancada; além disso, os caminhos que usam apenas `q_vivas()` ou a disputa aberta não aplicam uniformemente essa leitura. (`app/ponto/services/fechamento.py:1432–1448`; `app/chamados/juizes.py:887–907`; `app/chamados/models.py:2325–2332`)
- **Encerramento individual:** existe `fechar_por_competencia_trancada(autor)`, que escreve via/carimbo na pergunta; seu corpo não verifica exportação. É alteração do estado da pergunta, não máscara de exportadas vivas; o serviço atual de tranca deixou de carimbá-las. (`app/chamados/models.py:2230–2242`; `app/chamados/services/competencia_trancada.py:9–11`)
- **Silêncio do chamado:** `silenciado_ate` retira o chamado inteiro de `qs_na_fila`; não distingue irmãs de competências diferentes. (`app/chamados/services/painel.py:186–188`)
- **Separação no passivo O261:** a sonda identifica exportadas por exportação vigente, deriva a competência de `data_evento` e mede disputas mistas, mas não instala exclusão em pergunta ou leitor. (`logs/o261/passivo_pai_terminal.py:35–44`; `logs/o261/passivo_pai_terminal.py:69–88`)
- **Lote de órfãs enterradas:** separa exportadas para pauta antes de reabrir, mas seu universo é de perguntas carimbadas `fantasma`; não protege irmãs já vivas ao religar uma disputa. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:67–69`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:92–102`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:140–156`)

## 29/10 23:08 · sessao e8ed830a5577 · o261c2 · verificador · origem=Code

**Encontrei falhas funcionais e lacunas de cobertura; o diff não garante o nascimento sob chamado vivo do dia.** [models.py:1749](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1749), [models.py:2176](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:2176)

A análise foi estática; testes, interleavings concorrentes e presença atual dos estados no banco **[não verificado]**. Nenhum arquivo foi alterado **[não verificado]**. [nao verificado]

1. **A porta procura um container da pessoa, sem perguntar pelo chamado vivo do dia.**  
   Entrada: pergunta sem destino de 15/10, disputa fechada, pai resolvido e cobrança `batida_ausente` já viva daquele colaborador/dia. Ao desfazer o carimbo sem lastro, a cadeia reabre a disputa e procura/cria outro `disputa_supervisao`, mantendo a pergunta sem destino. A consulta nova não recebe dia nem célula; ignora a cobrança existente. Resultado: outro chamado vivo, em vez de costura na cobrança do fato. [relavra_via.py:222](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/relavra_via.py:222), [models.py:1819](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1819), [models.py:1749](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1749)

   A autoridade existente escolhe a cobrança pelo dia forte e pelo desempate “mais nova”; a costura de pergunta existente tem porta própria. O pai criado pela cura recebe contexto sem `data_turno`, portanto tampouco é reconhecido pelo juiz forte como chamado daquele dia. **Classe: derivação paralela de fonte única; pedido (C) incompleto.** [juizes.py:1610](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/juizes.py:1610), [fio.py:740](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/fio.py:740), [models.py:1762](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1762), [juizes.py:118](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/juizes.py:118)

2. **O nascimento continua quando o religamento foi recusado.**  
   Entrada: disputa aberta sob pai terminal; existe chamado da mesma chave, terminal, agrupado sob outro pai vivo. `ChamadoColaborador.abrir` devolve esse filho pelo ramo AGRUPADO-EM. `religar_pai_vivo` recusa e retorna `False`; `PerguntaDisputa.abrir` ignora o resultado e cria a pergunta sem destino sob o pai terminal original. **Classe: pré-requisito não imposto pelo gate.** [models.py:689](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:689), [models.py:1766](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1766), [models.py:2176](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:2176)

3. **O dedup retorna antes do gate novo.**  
   Entrada: fato existente sem destino, disputa aberta e pai resolvido. A célula correta encontrada para o colaborador/dia acusa; o gate reaponta a célula, relavra e devolve a pergunta existente antes de religar o pai. Resultado: a pergunta pode voltar a viver sob pai terminal. Mesmo uma existente já viva continua sendo devolvida nessa condição. **Classe: cura incompleta na origem; caminho que escapa do gate.** [models.py:2147](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:2147), [models.py:2158](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:2158), [models.py:2171](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:2171)

4. **Reabrir a pergunta numa disputa já aberta não assegura pai vivo.**  
   Entrada: disputa aberta sob pai resolvido, pergunta sem destino absorvida por ausência; a ausência é rejeitada e nenhuma outra cobre o dia. O materializador devolve a pergunta a pendente e chama `reabrir_sistema`; essa porta retorna imediatamente porque a disputa já está aberta, sem alcançar o religamento. Resultado: pergunta viva, pai ainda terminal. **Classe: cobertura incompleta das reaberturas.** [materializacao.py:615](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/materializacao.py:615), [materializacao.py:628](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/materializacao.py:628), [models.py:1805](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1805)

5. **A trava do religamento termina antes do nascimento.**  
   Cenário concorrente: D está aberta, suas perguntas antigas estão validadas e o pai é terminal. T1 religa D ao novo pai N e sai do `atomic`; antes do INSERT da pergunta nova, T2 fecha N. A guarda S127 permite esse fechamento porque existem perguntas antigas, mas nenhuma está viva. T1 então insere a pergunta viva sob N já fechado. O emissor comum gera perguntas depois de retornar do `atomic` do núcleo da disputa. **Classe: ordem de trava/transação; pré-requisito não preservado até a escrita.** [models.py:1782](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1782), [models.py:2178](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:2178), [juizes.py:1817](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/juizes.py:1817), [disputa_emissao.py:618](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:618)

6. **Uma falha no religamento deixa reabertura parcial e impede o retry.**  
   Entrada concreta possível no legado: pai terminal com `urgencia_original='critica'`. A nova porta copia esse valor; `ChamadoColaborador.abrir` o rejeita antes do dedup. Entretanto, `reabrir_sistema` já salvou a disputa aberta. Uma segunda tentativa retorna antes de religar, porque ela agora está aberta. O estoque histórico desse valor é documentado; sua presença atual **[não verificado]**. **Classe: transação incompleta; retry não recupera a operação.** [models.py:1761](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1761), [models.py:659](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:659), [juizes.py:1850](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/juizes.py:1850), [models.py:1815](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1815), [models.py:1805](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1805)

7. **Há cópia da redução registrada como C15.**  
   Para o queryset de perguntas sem destino, a porta calcula `any(pergunta_viva(...))` localmente, repetindo exatamente `fio_vivo`, cuja documentação atribui o universo ao chamador. O booleano coincide hoje; a incompletude é estrutural: a mesma pergunta passa a ter dois sítios de decisão. **Classe: segundo juiz que duplica autoridade registrada; LEI-AKITA 2.** [models.py:1793](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1793), [juizes.py:1102](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/juizes.py:1102), [core/juizes.py:241](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/core/juizes.py:241)

8. **O teste do carimbo não morde contador desconectado.**   [nao verificado]
   A fixture desse teste cria disputas sem pai. Logo `viva_sob_pai_terminal` vale zero, e trocar o valor escrito pelo comando por uma constante `0` preservaria o verde. O teste unitário do contador não cobre essa desconexão no comando. **Classe: selo sem mordida na integração.** [test_o261_vigia_fio.py:93](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_vigia_fio.py:93), [test_o261_vigia_fio.py:690](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_vigia_fio.py:690), [test_o261_vigia_fio.py:701](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_vigia_fio.py:701)

**Censo dos caminhos**

- **Nascimento de pergunta:** seis emissores chegam a `PerguntaDisputa.abrir`: [disputa_emissao.py:389](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:389), [disputa_emissao.py:770](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:770), [disputa_emissao.py:1160](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:1160), [acoes_disputa.py:265](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/acoes_disputa.py:265), [declaracao_texto.py:322](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/declaracao_texto.py:322), [declaracao_texto.py:611](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/declaracao_texto.py:611). O escape está dentro do gate, no retorno de dedup e na recusa ignorada descritos acima. [models.py:2171](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:2171)
- **Nascimento da disputa/vínculo:** núcleo comum e dois ancoradores manuais criam a disputa com chamado previamente obtido pela porta. [disputa_emissao.py:523](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:523), [disputa_emissao.py:927](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:927), [disputa_emissao.py:1147](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:1147). O novo escritor posterior ao nascimento é `religar_pai_vivo`. [models.py:1774](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1774)
- **Reabertura da disputa:** `reabrir_admin` e `reabrir_sistema`; a ação administrativa alcança o gate pelo nascimento da confirmação. [models.py:1678](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1678), [models.py:1797](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1797), [acoes_disputa.py:256](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/acoes_disputa.py:256).
- **Chamadores da reabertura sistêmica:** ressuscitação, desfazer carimbo, devolução de validação, rejeição de ausência e comando de órfãs. [models.py:1189](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1189), [relavra_via.py:222](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/relavra_via.py:222), [validacao.py:200](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/validacao.py:200), [materializacao.py:631](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/materializacao.py:631), [reabrir_orfas_enterradas.py:154](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/management/commands/reabrir_orfas_enterradas.py:154).
- **Retorno da pergunta à vida sem nascimento:** correção de questionário, fantasma, resposta nova e competência reaberta não passam pelo novo gate. Há caminhos que tentam reabrir o chamado separadamente, mas isso não cobre o caso da disputa já aberta do achado 4. [disputa_emissao.py:1327](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:1327), [models.py:2269](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:2269), [disputa_emissao.py:1265](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:1265), [competencia_trancada.py:247](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/competencia_trancada.py:247).

**RED dos testes — conclusão por inspeção; execução [não verificado]** [nao verificado]

| Teste | Sem a mudança correspondente | [nao verificado]
|---|---|
| Nascimento sem destino | Falha na âncora viva; não exige chamado do dia. [linha 74](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_pai_terminal.py:74) |
| Pai vivo permanece | Passa sem a cura; controle negativo. [linha 85](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_pai_terminal.py:85) |
| Destino vivo não religa | Passa sem a cura; controle negativo. [linha 95](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_pai_terminal.py:95) |
| Idempotência | Falha pela trilha ausente; cobre duas chamadas sequenciais de motivos diferentes. [linha 111](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_pai_terminal.py:111) |
| Reabertura sistêmica | Falha sem religamento; só cobre disputa inicialmente fechada. [linha 129](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_pai_terminal.py:129) |
| Contador do vigia | Morde contador sempre zero, com transição esperada `1 → 0`. [linha 142](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_pai_terminal.py:142) |
| Carimbo | Morde ausência da chave; não morde valor constante zero no comando. [linha 701](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_vigia_fio.py:701) |

Dois religamentos sobre a mesma disputa são serializados pela nova trava; essa proteção termina antes do nascimento, conforme o achado 5. [models.py:1743](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1743)

O vigia lê `pergunta_viva` e o conjunto oficial `VIVOS`, publica esperado zero e chega à comparação genérica do painel; não identifiquei defeito nessa cobrança do pedido (D). [vigia_fio.py:398](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/vigia_fio.py:398), [vigiar_fio_morto.py:203](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/management/commands/vigiar_fio_morto.py:203), [fio_estado.py:71](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/bin/fio_estado.py:71)

C7 permanece registrado em `DisputaSupervisao.pode_fechar`; a duplicação encontrada é C15. Não há registro novo em `core/juizes.py` neste diff que acione JUIZ-NOVO. [core/juizes.py:233](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/core/juizes.py:233), [core/juizes.py:241](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/core/juizes.py:241), [CLAUDE.md:86](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/CLAUDE.md:86). Violação funcional adicional por relógio, alteração de gravado exportado/L-092 ou pouso sem pré-requisito **[não verificado]**.

## 29/10 23:06 · sessao 057bb4011ce7 · - · - · origem=Ronald (tmux)

Julgamento e contagem — evidências para consolidação:

| Caminho | Exportação da competência | Tranca/fechamento |
|---|---|---|
| `pergunta_viva` / `_pergunta_viva_calc` | Não consulta exportação. O antigo `dia_em_competencia_exportada` foi removido, com justificativa expressa. `app/chamados/juizes.py:1005`, `app/chamados/juizes.py:1504` | Sim: `dia_da_pergunta_trancado` mata antes de resposta/carimbo; deriva competência de `data_evento`, lê `periodo_trancado` e preserva dias tocados por humano após a tranca. `app/chamados/juizes.py:1010`, `app/chamados/juizes.py:887`, `app/chamados/juizes.py:897`, `app/chamados/juizes.py:907`, `app/chamados/juizes.py:945` |
| `q_vivas` | Não consulta exportação. `app/chamados/models.py:2334` | Exclui via `competencia_trancada` materializada; **não implementa a tranca por leitura**: é superset deliberado do C6. `app/chamados/models.py:2325`, `app/chamados/models.py:2343`, `app/chamados/models.py:2350` |
| `PerguntaDisputa.viva` | Herda ausência de filtro de exportação do C6. `app/chamados/models.py:2366` | Delega ao C6, portanto herda tranca por leitura. `app/chamados/models.py:2366` |
| `PerguntaDisputa.estado` | Sem consulta de exportação. `app/chamados/models.py:2194` | Chama C6, mas pode devolver `PENDENTE`/`RESPONDIDA` mesmo quando C6 diz morta sem carimbo; usar `estado` sozinho não equivale a usar `.viva`. `app/chamados/models.py:2201`, `app/chamados/models.py:2208` |
| `perguntas_vivas_de` e `fio_vivo` | Sem filtro adicional de exportação. `app/chamados/juizes.py:1097`, `app/chamados/juizes.py:1102` | Reduzem `pergunta_viva`, herdando tranca. `app/chamados/juizes.py:1099`, `app/chamados/juizes.py:1111` |
| `respostas_sem_veredito` / `pode_fechar` / fechamento da disputa | Não consultam exportação. `app/chamados/models.py:1428`, `app/chamados/models.py:1439` | A lista exclui dias trancados, salvo `ignorar_tranca=True`; `pode_fechar` exige nenhuma resposta sem veredito e nenhum fio vivo. `app/chamados/models.py:1432`, `app/chamados/models.py:1439` |
| Fila global de validação `perguntas_respondidas_a_validar` | Não consulta exportação; seleciona respondidas com texto via `q_vivas` e confirma C6. `app/chamados/juizes.py:1121`, `app/chamados/juizes.py:1127` | Herda tranca pelo C6. `app/chamados/juizes.py:1127` |
| `fila_validar_classificada`, contagens por classe/chamado e API mensageria de validação | Não acrescentam filtro de exportação; usam fila global e classificam resposta/delta/conflito de batida. `app/chamados/services/validacao.py:791`, `app/chamados/services/validacao.py:817`, `app/chamados/services/validacao.py:867`, `app/chamados/services/validacao.py:875`, `app/api/views_mensageria.py:539` | Herdam tranca quando usam a fila global; `fila_validar_classificada(qs=...)` aceita universo fornecido pelo chamador. `app/chamados/services/validacao.py:867` |
| Validação individual/lote | Individual não barra por competência exportada/trancada; guarda de tranca foi expressamente removida. Lote reclassifica por resposta e chama a mesma ação individual. `app/chamados/services/validacao.py:348`, `app/chamados/services/validacao.py:362`, `app/chamados/services/validacao.py:681`, `app/chamados/services/validacao.py:692` | Ato humano não é barrado pela tranca nesse caminho. `app/chamados/services/validacao.py:362` |
| Materialização após validação | Seleção é respondida + validada + não materializada; a porta declara não ter portão de competência. `app/chamados/services/materializacao.py:1071`, `app/chamados/services/materializacao.py:1286` | Recusa por tranca foi removida. A cauda conta perguntas vivas por C6 e usa `pode_fechar`. `app/chamados/services/materializacao.py:1120`, `app/chamados/services/materializacao.py:1230` |
| Juiz do lastro `_pergunta_materializada_de_fato` / cache `veredito_lavrado` | Não consultam exportação; lavra chama o juiz do lastro. `app/chamados/juizes.py:588`, `app/chamados/veredito_lavrado.py:219` | Reconhece a **via carimbada** `VIA_COMPETENCIA_TRANCADA` como absolvição; não é o mesmo filtro de tranca por leitura do C6. `app/chamados/juizes.py:603`, `app/chamados/juizes.py:613` |
| `creditos_perguntas` do raio X/placar | Seleciona perguntas por destino **ou pai vivo**, aplica `q_vivas` e exclui materializadas com cache `VL_SIM`; não consulta exportação nem confirma C6. `app/chamados/services/raiox.py:54`, `app/chamados/services/raiox.py:72`, `app/chamados/services/raiox.py:133` | Não executa tranca por leitura; só exclusões presentes em `q_vivas` e cache. `app/chamados/services/raiox.py:72`, `app/chamados/services/raiox.py:133` |
| Cron `lavrar_perguntas_stale` | Sem filtro de exportação, mas universo exige **chamado_destino vivo**: pergunta sem destino ancorada no pai não entra por esse filtro. `app/chamados/management/commands/lavrar_perguntas_stale.py:56` | Mesmo universo; lavra juiz do lastro. `app/chamados/management/commands/lavrar_perguntas_stale.py:85` |
| `relatorios_disputa.classificar/coletar` | Sem filtro de exportação; disputas abertas são classificadas pelas perguntas vivas. `app/chamados/relatorios_disputa.py:17`, `app/chamados/relatorios_disputa.py:39` | Herda C6. `app/chamados/relatorios_disputa.py:17` |
| `folha/fora_da_folha` e `o_que_segura` | Recortam competência solicitada e dias retidos, mas não excluem por exportação vigente. Perguntas são carregadas por célula. `app/folha/fora_da_folha.py:51`, `app/folha/fora_da_folha.py:119`, `app/folha/fora_da_folha.py:188` | Fio usa `.viva` e `estado`, herdando tranca C6; somas por fio/grupo usam essa classificação. `app/folha/fora_da_folha.py:66`, `app/folha/fora_da_folha.py:154`, `app/folha/fora_da_folha.py:204` |
| `folha/previa` | Carrega perguntas cujo `data_evento` está na competência pedida, sem filtro de exportação. `app/folha/previa.py:64` | `_material` prioriza qualquer texto de resposta, inclusive de pergunta morta; só sem texto usa `fio_vivo` e herda tranca. `app/folha/previa.py:139` |
| Vigias `viva_em_disputa_fechada`, `viva_sob_pai_terminal` | Usam `q_vivas` + C6, sem exportação. `app/chamados/services/vigia_fio.py:352`, `app/chamados/services/vigia_fio.py:358`, `app/chamados/services/vigia_fio.py:401`, `app/chamados/services/vigia_fio.py:409` | Herdam tranca C6; após religar em pai vivo, a pergunta deixa especificamente o universo de `viva_sob_pai_terminal`. `app/chamados/services/vigia_fio.py:403` |
| Vigia `censo` de fio morto | Universo: chamados vivos com célula, células acusadoras e perguntas dessas células; sem filtro de exportação. `app/chamados/services/vigia_fio.py:186`, `app/chamados/services/vigia_fio.py:228`, `app/chamados/services/vigia_fio.py:244` | `fio_do_dia` delega `fio_vivo`; herda C6. `app/chamados/services/vigia_fio.py:164`, `app/chamados/services/vigia_fio.py:260` |

`_dentro_da_janela_viva` é um recorte **temporal**, formado pela janela anterior e atual de hoje; não consulta exportação ou tranca. Ele participa do juiz do lastro e de um degrau da vitalidade, sem equivaler a “competência exportada”. `app/chamados/juizes.py:517`, `app/chamados/juizes.py:538`, `app/chamados/juizes.py:544`, `app/chamados/juizes.py:616`, `app/chamados/juizes.py:1066`.

Sobre exclusão seletiva: há mecanismo real por **tranca**, que pode calar perguntas de certas competências da mesma disputa por leitura de `data_evento`; porém exportada e trancada não são equivalentes: o juiz de fechamento declara que aprovada/exportada/paga são marcos e “fechada = trancada”. `app/chamados/juizes.py:887`, `app/chamados/juizes.py:907`, `app/ponto/services/fechamento.py:1441`.

Existe seleção por exportação no lote específico `reabrir_orfas_enterradas`: exportadas vão à pauta em vez de serem reabertas. Isso não instala uma máscara de fila/cobrança sobre irmãs já vivas de uma disputa religada; o universo é restrito a perguntas carimbadas como `fantasma` e o APPLY reabre apenas as selecionadas. `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:92`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:98`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:140`.

A via `fechar_por_competencia_trancada` existe por pergunta, mas declara competência **trancada**, não exportada, e escreve via/carimbo; `pendencia_admin` é explicitamente pendência de admin e não encerramento. Nenhum desses é máscara seletiva por exportação. `app/chamados/models.py:2230`, `app/chamados/models.py:2238`, `app/chamados/models.py:2022`, `app/chamados/models.py:2034`.

## 29/10 23:06 · sessao a5260489adb0 · - · - · origem=Ronald (tmux)

Consolidado de cobrança (somente leitura):

| Caminho | Seleção/efeito | Competência exportada de `p.data_evento`? |
|---|---|---|
| Lembrete diário | Disputas com `fechada_em IS NULL`; carimbo diário e push colaborador; agregado à supervisão. `app/ponto/management/commands/alertar_disputas_pendentes.py:33-35`, `:48-70`, `:87-108`; cron `app/config/crons.py:719`. | **Não**. Não consulta perguntas, exportação ou tranca. |
| Cobrança por pessoa / chat | `_pendentes` delega a `perguntas_pendentes_do_colab`, que aplica `q_vivas`, `pergunta_viva` e sem resposta. `app/chamados/services/cobranca.py:45-48`; `app/chamados/services/aviso_pendencia.py:7-13`. Preview conta datas, perguntas e pais, sem filtro adicional: `cobranca.py:75-95`. Escreve prazo 48h nos pais e envia FCM/webpush: `:163-187`, `:98-119`. Chat registra serviço em `app/core/intake.py:65-68` e executa em `app/core/views_ia.py:828-846`. | **Não exportada**. **Sim tranca**, herdada de `pergunta_viva`; detalhes abaixo. |
| Aviso após texto livre | `aviso_texto_livre` enumera as perguntas pendentes e dias e informa que seguem aguardando resposta. `app/chamados/services/aviso_pendencia.py:16-24`. Usado no web em `app/chamados/views.py:1119` e API em `app/api/views.py:996`, `:2271`. | **Não exportada**; tranca via juiz. |
| Recobrança no fio | View distingue perguntas sem resposta vivas por `pergunta_viva`, depois chama serviço: `app/chamados/views.py:1413-1425`. Serviço conta **todas sem resposta**, renova prazo 20min no chamado recebido e envia push: `app/chamados/services/disputa_emissao.py:1444-1464`; regra 20min em `app/chamados/catalogo/motor.py:45-51`. | **Não exportada**. View exige ao menos uma pergunta viva; serviço não separa competências nem reavalia vitalidade de cada pergunta no contador. |
| Botão Cobrar individual | Busca chamado, valida área, limita um aviso/dia por pessoa e envia FCM. `app/chamados/views_cobrar.py:24-57`. | **Não**: sequer consulta perguntas/competência. |
| Cobrar em massa e prévia | Parte da fila/verbos, chamados VIVOS; aplica `muda_a_folha(ch)` antes de agrupar por pessoa: `app/chamados/views_cobrar.py:68-97`; prévia `:102-114`; envio `:116-133`. | **Filtra exportação do dia do CHAMADO**, não de cada pergunta: `app/chamados/services/cobranca_folha.py:29-35`, `:54-71`. Não separa perguntas de competências diferentes. Sem data, exclui chamado inteiro: `:54-61`. |
| Prazo ao religar | Defaults do novo pai recebem `prazo_sla=agora+20min`: `app/chamados/models.py:1749-1764`; regra `app/chamados/catalogo/motor.py:45-51`. | **Não**. Defaults do novo pai só têm contexto `origem/disputa/pai_anterior`, sem dia: `models.py:1762`; logo filtro em massa pode recusá-lo inteiro como `sem_data`, não separar perguntas. |
| Escalonamento do pai | Seleciona pai de módulo `disputa_supervisao`, em `COBRANCA`, não silenciado, com disputa aberta; prazo vencido gera push DP; níveis seguintes por idade. `app/ponto/management/commands/escalonar_chamados_supervisao.py:42-59`, `:93-104`, `:109-198`. | **Não**. Nenhuma seleção por pergunta/data/competência. |
| SLA genérico | Itera `qs_na_fila`, usa juiz de SLA, corte de passivo e teto de push DP. `app/chamados/management/commands/alertar_chamados_sla.py:24-52`. | Sem filtro próprio; herda universo da fila. |
| `perguntas_do_app` | Busca perguntas do colaborador com `q_vivas`, ordena e aplica `pergunta_viva`. `app/chamados/services/pergunta_no_app.py:39-44`. | **Não exportada**; tranca via juiz. |
| API GET perguntas | `api_disputa_perguntas` chama `perguntas_do_app`, separa respondidas e usa pai como fallback de ancoragem. `app/api/views.py:2603-2604`, `:2620-2646`. | **Não exportada**; tranca via juiz. |
| API POST resposta | Busca pergunta por dono e exige `pergunta_viva`; depois delega ao core. `app/api/views.py:2680-2698`. | **Não exportada**; tranca via juiz. |
| PWA formulário | `views.py:566-569` chama `montar_chamados_fio(so_janela=True)`; perguntas de disputas abertas são anexadas pela âncora em `app/chamados/services/fio.py:218-239`; formulário exige `p.viva` em `app/templates/chamados/partials/_card_atendimento.html:73-78`. | **Não exportada**; tranca via juiz. |
| PWA POST resposta | Mesma verificação por dono e `pergunta_viva`. `app/chamados/views.py:1238-1246`. | **Não exportada**; tranca via juiz. |
| API chamados v2 | Usa janela do colaborador; carrega **todas** as perguntas das disputas abertas, indexando destino VIVO ou pai; serializa sem filtro de vitalidade. `app/api/views.py:1015-1016`, `:1028-1049`, `:1158-1178`. | **Não**, nem C6 nessa seleção de perguntas. |

A distinção decisiva: `pergunta_viva` delega ao cálculo em `app/chamados/juizes.py:778-804`; este rejeita dia trancado e via de resolução por tranca em `:1005-1016`. A tranca é determinada pelo **`p.data_evento`**, janela da empresa e `periodo_trancado`, salvo toque humano posterior: `:875-907`. Esse caminho não consulta exportação vigente. A remoção de `dia_em_competencia_exportada` está expressamente documentada em `:1504-1528`.

A janela do colaborador não resolve a mistura de competências: seleciona `criado_em` na janela corrente e abre exceção por `q_vivas` **com destino não nulo**; não testa exportação nem `p.data_evento`. `app/chamados/services/janela_colab.py:49-74`. Pai recém-criado entra pela sua data atual, carregando perguntas antigas via `fio.py:218-239`.

Sobre push no próprio religamento: o método salva a troca de pai e registra evento, sem chamar notificações (`app/chamados/models.py:1774-1782`). O gate `abrir` cria, registra nascimento e eventualmente emite evento de célula (`:730-739`); `_trilha_do_nascimento` só grava auditoria (`:880-892`). Os receivers de chamado inspecionados tratam terminais, julgamento por dia forte e silêncio de isentos (`app/chamados/signals.py:53-82`, `:164-180`, `:227-251`), e os receivers de disputa relevantes condicionam o efeito ao fechamento (`:18-29`, `:97-107`, `:124-140`). Não há push automático nessas rotas inspecionadas.

Outros pushes de cobrança/reabertura são explícitos, sem filtro de exportação: abrir/agregar disputa usa `_enviar_push_disputa_colab`, com guarda global de ContextVar, em `app/chamados/services/disputa_emissao.py:1483-1507` (chamadores `:607`, `:623`, `:1089`, `:1170`); reabrir questionário notifica em `app/chamados/services/acoes_disputa.py:90-110`; reabrir administrativamente notifica em `:265-293`; recusar resposta pede resposta nova em `app/chamados/services/validacao.py:540-561`.

## 29/10 23:05 · sessao 9ff2e22e4a8d · - · - · origem=Ronald (tmux)

Achados comprováveis por leitura; testes não executados para preservar a árvore. [nao verificado]

1. **Recusa não impede nascimento — gate sem mordida.** Em `app/chamados/models.py:1766-1773`, `religar_pai_vivo()` recusa chamado terminal devolvido pelo ramo AGRUPADO-EM, mas `PerguntaDisputa.abrir()` ignora o resultado e cria a pergunta em `:2176-2179`. Cenário: disputa aberta sob pai terminal; existe chamado da mesma chave agrupado sob outro pai vivo. `ChamadoColaborador.abrir()` devolve esse filho terminal em `:689-696`; a disputa permanece no pai terminal e a pergunta sem destino nasce viva ali. O agrupamento aceita pai vivo do mesmo colaborador (`:575-596`). Classe: **pouso antes do pré-requisito / gate sem mordida**.

2. **Trava termina antes do nascimento — corrida com fechamento.** O atomic de religamento termina em `app/chamados/models.py:1782`; o INSERT ocorre depois, em `:2178`. Um caminho real gera perguntas após sair do atomic de `_abrir_ou_agregar_disputa` (`app/chamados/services/disputa_emissao.py:598-606,618`). Cenário: disputa aberta com perguntas antigas validadas e pai terminal; T1 religa a pai novo vivo e libera trava; T2 fecha esse pai antes do INSERT; T1 cria pergunta viva sob pai já fechado. T2 passa S127 porque perguntas antigas existem e nenhuma está viva (`app/chamados/juizes.py:1006-1007,1817-1823`; fechamento em `app/chamados/models.py:142-155,183,262-264`). O resolver não altera antigas já materializadas/validadas (`app/chamados/services/disputa_emissao.py:1634-1637`). Classe: **ordem de trava/transação / pré-requisito não preservado**. O teste de idempotência só faz duas chamadas sequenciais (`app/chamados/tests/test_o261_pai_terminal.py:116-120`).

3. **Reabertura parcialmente gravada e retry inútil.** `reabrir_sistema()` salva abertura e nota antes do atomic do religamento (`app/chamados/models.py:1811-1819`). Cenário: pai terminal legado com `urgencia_original='critica'`; a nova porta copia esse valor (`:1761`), e `ChamadoColaborador.abrir()` rejeita antes do dedup (`:659-661`; validação em `app/chamados/juizes.py:1880-1889`). A disputa fica aberta com pai terminal; retry retorna imediatamente porque já está aberta (`app/chamados/models.py:1805-1806`). O estoque histórico desses valores é documentado em `app/chamados/juizes.py:1850-1855`; sua presença atual no banco não foi verificada. Classe: **transação incompleta / idempotência de retry**.

Matriz dos testes, por inspeção: [nao verificado]

| Teste | Sem mudança correspondente | [nao verificado]
|---|---|
| Nascimento sem destino | Falha na âncora viva (`test_o261_pai_terminal.py:74-82`). |
| Pai vivo permanece | **Passa sem cura**; controle negativo (`:85-92`). |
| Destino vivo não religa | **Passa sem cura**; controle negativo (`:95-108`). |
| Idempotência | Falha pela trilha ausente; não testa concorrência (`:111-126`). |
| Reabrir sistema | Falha sem novo religamento (`:129-139`). |
| Vigia conta estado | Falha com função ausente/contador zero (`:142-153`). |
| Carimbo alterado | Falha sem chave nova, mas constante zero no comando passa: fixture não planta pai terminal (`app/chamados/tests/test_o261_vigia_fio.py:689-710`). |

A serialização de dois religamentos sobre a mesma disputa está presente: leitura sob trava da disputa e do colaborador, seguida do gate que trava colaborador (`app/chamados/models.py:1743-1749,666-670`). Isso não protege o intervalo até a criação da pergunta nem coordena o fechador (`:2177-2179,122-183`).

## 29/10 23:05 · sessao f7e20b14288d · - · - · origem=Ronald (tmux)

Fila admin — evidências consolidadas:

| Caminho | Seleção/efeito | Exportada? Trancada/fechada? |
|---|---|---|
| `qs_vivos` → `qs_na_fila` | Universo `status_local ∈ VIVOS`; fila `status_local ∈ COBRANCA` menos `silenciado_ate > now` (`app/chamados/services/painel.py:153-155`, `186-188`). | Nenhum filtro por `data_evento`, exportação ou tranca nesses métodos (`app/chamados/services/painel.py:153-155`, `186-188`). |
| Lista principal / resumo | View chama `montar_contexto_painel`; lista padrão corta por `COBRANCA`, depois exclui verbos arquivado/histórico; resumo agrupa essa mesma lista (`app/chamados/views.py:726-738`; `app/chamados/services/painel.py:314-329`, `337-338`, `462-475`). | Nenhum filtro de exportação/tranca nesses cortes (`app/chamados/services/painel.py:314-329`). |
| Totais `abertos`, `em_analise`, `ativos`, `emergencias` | Contam `qs_na_fila`; `ativos=abertos+em_analise` (`app/chamados/services/painel.py:210-223`). | Sem corte adicional por competência (`app/chamados/services/painel.py:210-223`). |
| Crédito da pergunta ao pai | Busca `Q(chamado_destino__in=vivo) | Q(disputa__chamado__in=vivo)`; sem destino, credita `disputa.chamado`; filtra `q_vivas()` e exclui materializada com `VL_SIM` (`app/chamados/services/raiox.py:54-55`, `72`, `133-149`). | Exportação: não. Tranca dinâmica: **não chama juiz exato**. `q_vivas` exclui legado carimbado com `VIA_COMPETENCIA_TRANCADA`, mas declara não expressar a tranca por leitura (`app/chamados/models.py:2325-2332`, `2342-2353`). |
| Pilhas / verbos / pilulas | `particionar_verbos` usa esses créditos; respondida → validar, não respondida → cobrar, salvo precedência auditoria/módulo; aplica arquivado/histórico conforme silêncio (`app/chamados/services/verbos.py:31-59`; `app/chamados/catalogo/natureza.py:41-63`, `72-75`). | Sem filtro adicional de exportação ou tranca; herda limitações de `creditos_perguntas` (`app/chamados/services/verbos.py:31-59`). |
| Placar JSON / navbar | JSON lê lavrado sem filtros ou recalcula contexto; cron lavra contexto; badge admin soma validar+decidir; fallback navbar recalcula mesma partição (`app/chamados/views.py:840-861`; `app/core/management/commands/lavrar_badge_navbar.py:39-42`, `68-83`; `app/core/context_processors.py:88-93`; `app/chamados/services/verbos.py:19-24`). | Sem corte adicional; herda métodos acima (`app/core/management/commands/lavrar_badge_navbar.py:39-42`, `72-83`). |
| Fila copiloto / idade / gestão | `por_verbo` usa `particionar_verbos(qs_vivos())`; estoque/gestão usam `qs_na_fila` (`app/chamados/services/fila.py:53-55`, `88-100`, `117-141`, `145-170`). | Sem corte adicional por exportação/tranca (`app/chamados/services/fila.py:53-55`, `88-100`, `145-170`). |
| Inteligência `na_fila` | Usa vivos e conta chamados cujo verbo pertence a `VERBOS` (`app/chamados/services/na_fila.py:10-26`). | Sem corte adicional; herda crédito/partição (`app/chamados/services/na_fila.py:15-24`). |
| Fila por causa / toques | Universo `qs_na_fila`; classifica cada chamado; resposta sem veredito ganha gaveta própria (`app/chamados/services/fila_por_causa.py:158-161`, `195-200`, `210-216`). | Não exclui competência exportada ou trancada no universo; `respostas_sem_veredito` é outra leitura cuja implementação cabe à investigação de julgamento (`app/chamados/services/fila_por_causa.py:158-161`, `210-216`). |
| Modal/fio admin | Admin carrega todos chamados; busca perguntas de disputas abertas e vivas de fechadas; agrupa por `chamado_que_lista` (`app/chamados/services/fio.py:173-184`, `218-240`). | Exportação: não. Em disputa **aberta**, anexa todas sem juiz; em disputa **fechada**, exige `pergunta_viva` (`app/chamados/services/fio.py:229-239`). Modal desenha cada pergunta anexada (`app/templates/chamados/partials/modal_fio.html:154-155`). |
| Destaque/ordenação do fio | `tem_pergunta_viva` usa property `.viva` + `pergunta_sem_acao_possivel`; cards com ela sobem (`app/chamados/services/fio.py:279-281`, `409-415`). | `.viva` delega ao juiz, logo tranca dinâmica conta; exportação não é condição desse trecho (`app/chamados/models.py:2355-2360`; `app/chamados/juizes.py:1005-1016`). |
| Detalhe local do pai | Busca disputa aberta por `chamado`, carrega perguntas dela e filtra `_viva(_p)` (`app/chamados/services/acoes_chamado.py:335-348`). | Exportação: não; tranca dinâmica: sim, pelo C6 (`app/chamados/juizes.py:1005-1016`). |
| Contador modal | Ativos via `qs_na_fila`, disputas via `fechada_em IS NULL` (`app/chamados/views.py:900-912`). | Não corta pela competência das perguntas (`app/chamados/views.py:900-912`). |

Distinção importante: **trancada não é equivalente a somente exportada na árvore atual**. A própria regra diz “NUNCA exportada: competência só exportada segue viva”; a antiga `dia_em_competencia_exportada` foi removida e há lápide explícita (`app/chamados/services/competencia_trancada.py:3-11`; `app/chamados/juizes.py:1504-1526`). O juiz exato mata pela tranca **do `data_evento`**, salvo toque humano posterior à tranca (`app/chamados/juizes.py:887-907`, `1005-1016`).

Mecanismos vizinhos que poderiam confundir a conclusão:

- `arquivar_competencia_encerrada` silencia **chamados**, usando dia do chamado/criação; pula chamado com disputa contendo qualquer pergunta viva. Não separa perguntas por exportação (`app/chamados/management/commands/arquivar_competencia_encerrada.py:100-128`, `148-162`).
- Serviço de tranca seleciona perguntas por `data_evento` na janela; hoje não as carimba, pois C6 lê a tranca. Preserva chamado com pergunta viva fora da competência e disputa cujo pai vivo fica fora (`app/chamados/services/competencia_trancada.py:106-149`, `171-198`). Isso atende **tranca**, não “apenas exportada” (`app/chamados/services/competencia_trancada.py:3-11`).
- `religar_pai_vivo` não lê nenhuma pergunta/competência/exportação; cria/obtém pai vivo com prazo e escreve apenas `d.chamado` (`app/chamados/models.py:1741-1782`). Wrapper exige existir **alguma** viva sem destino, sem particionar demais perguntas (`app/chamados/models.py:1788-1795`). Todas sem destino passam a usar pai novo pelo `or` da ancoragem (`app/chamados/services/fio.py:116-117`).

Não encontrei AGENTS na busca `rg --files -g AGENTS.md`; existência fora desse universo [nao verificado].

## 29/10 23:05 · sessao dfa06780a531 · - · - · origem=Ronald (tmux)

Achado principal: `religar_pai_vivo` substitui “chamado vivo do dia” por “container vivo da pessoa”, contrariando o pedido literal de `app/docs/CORTES.md:381`. A busca nova não recebe dia/célula nem consulta a cobrança existente; filtra apenas colaborador, categoria, módulo e `VIVOS`. `app/chamados/models.py:1749`

Cenário concreto: existe pergunta viva sem destino de 15/10, disputa fechada com pai resolvido e cobrança `batida_ausente` aberta daquele colaborador/dia. `desfazer_carimbo_sem_lastro` reabre essa disputa; a nova cadeia cria outro `disputa_supervisao` e mantém a pergunta sem destino, embora a cobrança correta já exista. Caminho: `app/chamados/services/relavra_via.py:195`, `app/chamados/services/relavra_via.py:222`, `app/chamados/models.py:1819`, `app/chamados/models.py:1795`, `app/chamados/models.py:1749`. Classe: derivação/chave paralela e cura fora da origem do vínculo.

A autoridade do dia seleciona cobrança por dia forte e desempate “mais nova”; a porta canônica de costura valida, aponta destino e lavra no mesmo ato. `app/chamados/juizes.py:1610`, `app/chamados/juizes.py:1621`, `app/chamados/services/fio.py:740`, `app/chamados/services/fio.py:792`

O pai novo também nasce sem célula e sem `data_turno`; portanto o juiz forte do dia não o identifica como cobrança de 15/10. `app/chamados/models.py:1762`, `app/chamados/models.py:905`, `app/chamados/juizes.py:118`

Há uma incompletude estrutural adicional: o novo `any(pergunta_viva(q) for q in qs)` copia literalmente a redução de `fio_vivo`, autoridade registrada C15. Hoje os booleanos coincidem; não há diferença comportamental demonstrada, mas resta segundo escritor da redução. `app/chamados/models.py:1793`, `app/chamados/juizes.py:1102`, `app/core/juizes.py:241`

Não identifiquei cópia nova de C7 nem registro novo que dispare JUIZ-NOVO; `pai_terminal` consulta o `VIVOS` oficial. `app/chamados/models.py:1434`, `app/chamados/models.py:1715`, `app/core/juizes.py:233`, `CLAUDE.md:86`

Não elevaria o `_tz.now()` do SLA a achado: encontrei a classe proibida citada, mas nenhuma regra concreta de borda aplicável que sustentasse a reprovação. `app/chamados/models.py:1763`, `CLAUDE.md:592`

## 29/10 23:04 · sessao 842bc2ead565 · - · - · origem=Ronald (tmux)

Censo de caminhos, por leitura:

- Nascimento: seis chamadas a `PerguntaDisputa.abrir`: `app/chamados/services/disputa_emissao.py:389`, `:770`, `:1160`; `app/chamados/services/acoes_disputa.py:265`; `app/chamados/services/declaracao_texto.py:322`, `:611`. A criação efetiva fica em `app/chamados/models.py:2178`.
- Nascimento da disputa: núcleo comum em `app/chamados/services/disputa_emissao.py:523`, agrupador manual em `:927` e âncora manual em `:1147`; usam `ChamadoColaborador.abrir` antes de criar, respectivamente em `:504`, `:908`, `:1128`.
- Reabertura da disputa: duas portas escrevem `fechada_em=None`, `reabrir_admin` em `app/chamados/models.py:1701` e `reabrir_sistema` em `:1811`. A ação administrativa cria pergunta depois, alcançando o gate novo (`app/chamados/services/acoes_disputa.py:256`, `:265`).
- Chamadores de `reabrir_sistema`: ressuscitação em `app/chamados/models.py:1189`; desfazer carimbo em `app/chamados/services/relavra_via.py:222`; devolver validação em `app/chamados/services/validacao.py:200`; rejeição de ausência em `app/chamados/services/materializacao.py:631`; comando de órfãs em `app/chamados/management/commands/reabrir_orfas_enterradas.py:154`.
- Reabertura de pergunta também ocorre em `app/chamados/models.py:2269`, `:2284`, `:2303`, e na porta `app/chamados/services/disputa_emissao.py:1327`; esta é chamada pelo colaborador em `app/chamados/models.py:1871`, pela recusa administrativa em `app/chamados/services/validacao.py:540`, por ausência rejeitada em `app/chamados/services/materializacao.py:628` e pelo comando de limbo em `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`.

Achados para consolidar:

1. **Gate novo não cobre a pergunta existente que ganha vida ao ser reapontada.** `PerguntaDisputa.abrir` troca `celula`, relavra, salva e retorna a existente antes da chamada nova a `religar_pai_vivo` (`app/chamados/models.py:2154`, `:2168`, `:2171`, `:2177`). Cenário: fato existente sem destino, disputa aberta e pai resolvido; sua célula histórica absolve, mas a célula correta encontrada para o colaborador/dia acusa. O reapontamento torna a pergunta viva e a devolve ainda sob pai terminal. Classe: **cura incompleta na origem / caminho que escapa do gate**. A vida é decidida com os insumos da pergunta, inclusive célula, conforme a documentação do prefiltro em `app/chamados/models.py:2317`.

2. **Reabrir pergunta em disputa já aberta continua sem garantir pai vivo.** A rejeição da ausência limpa os carimbos das perguntas e chama `reabrir_sistema` (`app/chamados/services/materializacao.py:628`, `:631`). Porém essa porta retorna imediatamente quando a disputa já está aberta, antes do religamento recém-adicionado (`app/chamados/models.py:1805`, `:1819`). Cenário: disputa aberta sob pai resolvido, pergunta sem destino absorvida por ausência; ausência rejeitada e nenhuma outra ausência cobrindo o dia → pergunta volta a pendente, pai permanece resolvido. Classe: **cura incompleta / pré-requisito da reabertura não assegurado**. A condição de rejeição e ausência sem cobertura está em `app/chamados/services/materializacao.py:608`, `:615`.

Observações de autoridade:

- O diff não acrescenta outra implementação de `pode_fechar`; a autoridade registrada continua `DisputaSupervisao.pode_fechar` em `app/core/juizes.py:233`.
- O novo `any(pergunta_viva(q)...)` em `app/chamados/models.py:1793` repete a redução já existente em `fio_vivo`, `app/chamados/juizes.py:1102`–`:1111`. Há duplicação estrutural da redução C15, mas divergência funcional atual entre as duas expressões **[não verificado]**.
- A nova escrita do vínculo fica em `app/chamados/models.py:1774`; os outros três locais identificados são os nascimentos citados acima. Segundo escritor posterior ao nascimento **[não verificado]**.

Não executei testes ou operações de escrita. **[não verificado]** [nao verificado]

## 29/10 23:03 · sessao 96578cc4f702 · o261c2 · sonda · origem=Code

- MISTAS 18 disputas | perguntas EXPORTADAS nelas 201 | ABERTAS nelas 212 | colabs 18. [nao verificado]
- EXPORTADAS nas mistas por competencia: `{'2:09/2026': 192, '3:09/2026': 9}`. [nao verificado]
- SO-EXPORTADA 22 disputas | perguntas 269. [nao verificado]
- SO-ABERTA 33 disputas | perguntas 125. [nao verificado]
- universo repartido 807 (tem de ser == vigia 807) | disputas 73 | colabs 66. Universo == VIGIA: 807 == 807. [nao verificado]
- ERROS=0. [nao verificado]

As duas somas fecham: ABERTA + EXPORTADA + sem data = 337 + 470 + 0 = 807; mistas + so-exportada + so-aberta = 18 + 22 + 33 = 73. [nao verificado]

## 29/10 22:09 · sessao 61d5e68cd5c8 · - · - · origem=Ronald (tmux)

Achado confirmado: novo `any(pergunta_viva(q) for q in qs)` duplica C15, registrado como `fio_vivo` em `app/core/juizes.py:123` e `:241`; autoridade em `app/chamados/juizes.py:1102`. O contrato existente detecta exatamente essa forma: `app/chamados/tests/test_o261_fio_tem_uma_porta.py:99`, `:127`, `:142`. Executei o detector AST real contra o trecho novo em memória: acusou `religar_se_fio_sob_pai_morto`.

Outros pontos encaminhados:

- Admin conserva `ch` antigo: nota e push usam esse objeto mesmo se `PerguntaDisputa.abrir` mudar o pai; base `app/chamados/services/acoes_disputa.py:270`, `:275`.
- Novo pai manual perde `contexto_json.chamados_orfaos_ids`, necessário para dissolução imediata por entrada sincronizada: nascimento em `app/chamados/services/disputa_emissao.py:920`; leitor em `app/ponto/services/entrada_sincronizada.py:47`.
- Reparentar muda a âncora de todas as perguntas sem destino, inclusive históricas, enquanto followups ficam no pai anterior: `app/chamados/services/fio.py:117`; novas respostas escrevem no pai atual em `app/chamados/services/disputa_emissao.py:1309`.
- Não encontrei segundo escritor de `DisputaSupervisao.chamado` pós-nascimento na base; escritores de nascimento em `app/chamados/services/disputa_emissao.py:527`, `:931`, `:1151`.
- Não classificaria a suíte inteira como sem mordida: os testes positivos de pai terminal verificam mudança ausente na base; os controles de pai vivo/destino vivo passam também sem cura. O teste de idempotência usa motivos diferentes e não demonstra duas entradas iguais, conforme anexo `app/chamados/tests/test_o261_pai_terminal.py:111`.

Censo ampliado de consumidores foi enviado nas mensagens anteriores, com linhas.

## 29/10 22:09 · sessao 6d3f808735cb · - · - · origem=Ronald (tmux)

Achados de escopo e selo:

- **P2 — Escopo de testes diverge do corte.** O corte permite “testes de tela”; o wrapper exige testes **novos**, por nome/caminho. Assim, alteração legítima de teste existente é contada fora, enquanto teste novo sem relação com tela passa. O selo exercita somente teste novo. Referências: `app/docs/CORTES.md:394`; `bin/codex_pergunta.sh:116`; `bin/codex_pergunta.sh:117`; `bin/tests/test_codex_pergunta.sh:100`; `bin/tests/test_codex_pergunta.sh:164`.

- **P2 — Selo não garante o destino efetivo da chamada.** `_estrutura` aceita presença de `-C "$WS"` e proíbe somente algumas grafias de destino vivo. Mantendo esse argumento e acrescentando `-C "$VIVO/app"`, sua verificação continua sem diagnósticos; o teste comportamental também verifica somente presença de argumentos isolados. Confirmei a lacuna estrutural executando exclusivamente a função extraída em memória com essa mutação, sem arquivos. Referências: `bin/tests/test_codex_pergunta.sh:35`; `bin/tests/test_codex_pergunta.sh:39`; `bin/tests/test_codex_pergunta.sh:40`; `bin/tests/test_codex_pergunta.sh:133`; `bin/codex_pergunta.sh:251`. Execução comportamental do mutante: **[nao verificado]**.

- **P2 — Ressalva da 7b ausente.** As proibições gerais de escrita continuam sem exceção para o piloto, embora o wrapper já habilite `workspace-write`; falta a ressalva expressamente exigida pelo contrato v2(a) fornecido. Referências: `CLAUDE.md:578`; `CLAUDE.md:588`; `CLAUDE.md:599`; `bin/codex_pergunta.sh:246`.

- **P2 — Procedimento publicado ainda contradiz v2.** O RELATO continua mandando remover o papel “no mesmo marco”, enquanto v2(j) exige pouso próprio posterior; o descarte publicado usa `checkout .`, sem limpar novos arquivos, divergindo do reset à base + `clean -fdx` do v2(h). Referências: `app/docs/RELATO.md:10`; `app/docs/RELATO.md:12`; `bin/codex_pergunta.sh:39`.

Não trataria `rc=0` com `fora_do_escopo>0` como falha isolada do wrapper: descarte e selos semânticos são responsabilidade expressa do Code. Referências: `app/docs/RELATO.md:10`; `app/docs/RELATO.md:11`; `app/docs/CORTES.md:394`.

## 29/10 22:08 · sessao 06d376475023 · - · - · origem=Ronald (tmux)

Achados e lacunas dos consumidores/selos:

- **Alta — orientação de LIMITE incorreta para construtor.** `bin/codex_pergunta.sh:263` manda seguir por sonnet sem esperar, inclusive no novo papel. `bin/codex_limite_estado.py:157` repete essa instrução global. Contraria a espera do piloto e a proibição de reconstrução pelo Code, também registradas em `app/docs/RELATO.md:10`. O selo só simula LIMITE para leitura (`bin/tests/test_codex_limite.sh:24`); o falso construtor só tem modos ok/escreve/dorme (`bin/tests/test_codex_pergunta.sh:96`).

- **Média — classificação de construtor sem mordida no selo de estado.** As fixtures só incluem papéis antigos e inventado (`bin/tests/test_codex_papel_estado.sh:14`); a asserção aceita o prefixo terminando em sonda (`bin/tests/test_codex_papel_estado.sh:31`). Remover construtor de `PAPEIS` faria o papel cair em outro (`bin/codex_limite_estado.py:78`) sem contrariar essas asserções.

- **Média — ESPERANDO não tem integração testada.** O selo aceita a linha em qualquer posição e qualquer PID numérico (`bin/tests/test_codex_pergunta.sh:160`), sem exigir primeira linha útil, PID do wrapper ou aceitação pelo hook. O consumidor real exige identidade do processo/script (`bin/hook_stop_fila1.py:474`, `bin/hook_stop_fila1.py:521`). Não identifiquei incompatibilidade nominal entre a emissão atual (`bin/codex_pergunta.sh:90`) e esse consumidor; execução integrada [nao verificado].

- **Média — cobertura parcial da preservação e do timeout.** O teste comportamental antigo exercita leitura (`bin/tests/test_codex_pergunta.sh:129`), sem censo/verificador. O teste sonda continua separado (`bin/tests/test_codex_sonda.sh:58`). Timeout cobre gate e exec nos cenários (`bin/tests/test_codex_pergunta.sh:179`, `bin/tests/test_codex_pergunta.sh:187`), sem cenário de todos os slots ocupados ou filho resistente a TERM.

- **Média — ressalva de 7b ainda pendente para o pouso.** Permanecem proibições gerais de escrita e sandbox diferente de read-only em `CLAUDE.md:574`, `CLAUDE.md:578`, `CLAUDE.md:588` e `CLAUDE.md:599`. O contrato v2(a) anexado exige ressalva no pouso; ocorrência desse pouso [nao verificado]. [nao verificado]

Compatibilidade confirmada por leitura: os 12 campos não quebram o parser, que aceita pelo menos nove e conserva os índices antigos (`bin/codex_limite_estado.py:66`). Construtor está no vocabulário (`bin/codex_limite_estado.py:23`); o gerador mantém o leitor único e propaga papel/origem sem enumeração paralela (`bin/gerar_codex_estado.py:41`, `bin/gerar_codex_estado.py:155`, `bin/gerar_codex_estado.py:176`).

Nenhum teste executado, pois criam arquivos temporários (`bin/tests/test_codex_pergunta.sh:68`, `bin/tests/test_codex_papel_estado.sh:7`); resultados de execução [nao verificado].

## 29/10 22:08 · sessao 61d74141d87b · - · - · origem=Ronald (tmux)

Achados sustentados pela base e pelo diff:

1. **Chamado extra na fila / pouso antes do pré-requisito.** O novo trecho de `PerguntaDisputa.abrir` religa qualquer nascimento sem destino antes de saber se a pergunta vive. Cenário: célula com veredito não acusatório, sem marcos pendentes; disputa aberta sob pai resolvido; nenhuma pergunta existente. O helper cria pai aberto, mas C6 declara a pergunta morta. A guarda do chamado só examina módulos de turno, e a fila inclui o novo pai aberto. Base: `app/chamados/juizes.py:1053`, `app/chamados/models.py:848`, `app/chamados/services/painel.py:186`; trecho novo imediatamente antes do `create` hoje em `app/chamados/models.py:2085`.

2. **Segundo juiz registrado.** `religar_se_fio_sob_pai_morto` introduz `any(pergunta_viva(q) for q in qs)`, duplicando a redução C15, cuja autoridade é `fio_vivo`. Registro: `app/core/juizes.py:123`; autoridade: `app/chamados/juizes.py:1102`; consumidor já convertido: `app/chamados/services/disputa_emissao.py:1188`. Ler `VIVOS` em `pai_terminal`, isoladamente, respeita C1: `app/core/juizes.py:100`.

3. **Derivação paralela da chave do pai; pode haver dois pais vivos.** Nascimento canônico fixa módulo `disputa_supervisao`, enquanto os manuais usam `disputa_supervisao_manual` e incluem subcategoria no lookup. O novo helper herda módulo do pai velho e tira subcategoria do lookup. Cenário: disputa aberta com pai manual terminal e outro pai canônico vivo do mesmo colaborador; o helper não encontra o canônico e cria outro manual vivo. Também pode reutilizar pai manual de outro subtipo. Fontes: `app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:908`, `app/chamados/services/disputa_emissao.py:1128`; ausência de unicidade: `app/chamados/models.py:639`.

4. **Ordem de trava: caminho inverso alcança o helper.** `ChamadoColaborador.abrir` segura Colaborador, renasce chamado existente, ressuscita perguntas e chama `reabrir_sistema`, que passa a travar Disputa. Caminho: `app/chamados/models.py:669` → `:685` → `:770` → `:1098` → `:1188`. O concorrente novo segura Disputa e solicita Colaborador no mesmo gate. Cenário possível com disputa fechada contendo pergunta com destino ressuscitável e outra viva sem destino; um emissor conserva instância aberta obtida antes do fechamento e entra no novo nascimento. Além disso, `select_for_update().select_related('colaborador')` trava ambas as tabelas sem `of=('self',)`, portanto a consulta não expressa a sequência individual prometida pela docstring. Lei: `app/chamados/models.py:646`.

5. **Consumidor do ponteiro não atualizado no admin.** Se `ch.reabrir()` recusa, o gate pode mover `d_fec.chamado` para novo pai. O diff refresca `d_fec`, mas mantém `ch` antigo: justificativa administrativa continua anexada ao pai terminal e push anuncia seu número. Recusas: `app/chamados/models.py:1051`; nota: `app/chamados/services/acoes_disputa.py:269`; push: `:275`. A pergunta administrativa é conversa e nasce sempre: `app/chamados/models.py:2033`.

6. **Idempotência da recusa.** Duas chamadas diretas iguais ao novo `religar_pai_vivo`, com gate devolvendo filho terminal AGRUPADO-EM, produzem duas trilhas `disputa_religar_recusado`; o estado não é curado e o nascimento continua. O comportamento do gate que devolve esse terminal está em `app/chamados/models.py:689`. A duplicação das trilhas decorre do trecho novo do diff; frequência em produção: [nao verificado].

O teste anexado usa sempre célula acusatória e não cobre o achado 1, nem módulos manuais, concorrência ou recusa AGRUPADO. A efetiva execução dos testes/RED: [nao verificado]. [nao verificado]

## 29/10 22:08 · sessao 12b050fdf0bb · - · - · origem=Ronald (tmux)

Achados por inspeção estática; execução real destes cenários [nao verificado].

- **Alta — bypass da exclusão, timeout e protocolo `.rc`.** `CODEX__INTERNO=1` só exige que alguém tenha o lock; não comprova que o processo recebeu o lock do externo. Durante uma chamada ativa, uma segunda chamada interna passa em `flock -n 6`, pula validação de volta, timeout e escrita de `.rc`, e alcança o `codex exec`. Referências: `bin/codex_pergunta.sh:84`, `:85`, `:91`, `:94`, `:98`, `:100`, `:246`. O selo só testa chamada interna com lock livre: `bin/tests/test_codex_pergunta.sh:147`.

- **Alta — sinal prematuro de conclusão por ordem errada de trava.** Volta inválida ou destino inválido gravam `<obra>.rc` antes de tentar o lock. Uma segunda chamada errada com a mesma obra pode produzir `rc=2` enquanto a primeira ainda escreve. Referências: `bin/codex_pergunta.sh:75`, `:85`, `:86`, `:88`. O selo preserva `.rc` apenas no caso lock ocupado com argumentos válidos: `bin/tests/test_codex_pergunta.sh:170`, `:174`, `:176`.

- **Alta — confinamento não valida o diretório efetivamente passado ao Codex.** `_destino_ok` resolve o caminho físico da worktree, mas aceita `app/` por simples `-d`. Uma worktree literal com `app` sendo symlink para outro lugar passa na validação; a chamada usa esse `app` em `-C`. Referências: `bin/codex_pergunta.sh:78`, `:79`, `:81`, `:247`. O selo verifica symlink da raiz, não de `app/`: `bin/tests/test_codex_pergunta.sh:144`. Existência disso no piloto [nao verificado].

- **Alta — lock não é único por worktree.** A identidade da trava depende de `CODEX_LOGD` ou do diretório da cópia do wrapper, enquanto o destino é sempre a mesma worktree literal. Duas chamadas com `LOGD` distintos usam travas e slots distintos sobre o mesmo destino. Referências: `bin/codex_pergunta.sh:42`, `:44`, `:67`, `:74`, `:87`, `:224`, `:230`. Uso real de diretórios distintos [nao verificado].

- **Média — `.rc` não cobre erros de instrumento anteriores ao ramo construtor.** Pergunta ilegível, hash inválido e anexos/sonda ilegíveis terminam antes da inicialização/publicação do `.rc`, podendo deixar arquivo antigo no lugar. Referências: `bin/codex_pergunta.sh:57`, `:58`, `:65`, `:66`, `:74`, `:89`. Também não há tratamento de sinal no externo para garantir publicação em interrupção: `bin/codex_pergunta.sh:84`, `:91`, `:94`; comportamento de interrupção em produção [nao verificado].

- **Média — instrução de LIMITE contradiz o piloto.** O caminho comum de rc75 manda seguir por sonnet mesmo para construtor, embora o contrato fornecido determine esperar e não reconstruir. Referência: `bin/codex_pergunta.sh:263`; contrato anexo, item (g) [nao verificado como arquivo local].

- **Média — base declarada não é vinculada à worktree.** O hash é resolvido em `VIVO`, mas a validação da worktree não compara seu HEAD nem repositório de origem com esse hash. `BASE: $HASH` pode descrever outra árvore; `wt_head` só é registrado no fim. Referências: `bin/codex_pergunta.sh:58`, `:81`, `:134`, `:203`. Divergência real [nao verificado].

- **Média — falta a ressalva da regra 7b exigida pelo contrato v2(a).** A cópia mantém proibições categóricas de escrita pelo Codex. Referências: `CLAUDE.md:565`, `:574`, `:578`, `:588`, `:599`. O corte autoriza exceção circunscrita: `app/docs/CORTES.md:394`.

## 29/10 22:07 · sessao 3140036661a5 · - · - · origem=Ronald (tmux)

Censo dos chamadores de produção, pela cópia base:

| Porta | Chamadores |
|---|---|
| `PerguntaDisputa.abrir` | `app/chamados/services/disputa_emissao.py:389` (`gerar_perguntas_disputa`), `:770` (`_criar_pergunta_se_couber`), `:1160` (`criar_disputa_ancora_de_chamado`); `app/chamados/services/declaracao_texto.py:322` (`declarar_hora_colab`), `:611` (`aplicar_declaracao`); `app/chamados/services/acoes_disputa.py:259` (`acao_reabrir_disputa_admin`). |
| `DisputaSupervisao.reabrir_sistema` | `app/chamados/models.py:1189` (`ressuscitar_perguntas`); `app/chamados/services/relavra_via.py:222`; `app/chamados/services/validacao.py:200`; `app/chamados/services/materializacao.py:631`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:154`. |

Achados:

1. **O nascimento continua após religação recusada.** O gate de chamado pode devolver filho terminal agrupado sob pai vivo, mesmo com `status_local__in=VIVOS` (`app/chamados/models.py:689`, `:695`). O diff detecta isso em `religar_pai_vivo` e retorna `False`, mas o novo bloco de `PerguntaDisputa.abrir` ignora esse retorno e segue ao `create` (inserção antes da linha base `app/chamados/models.py:2085`). Cenário: disputa aberta sob pai terminal agrupado, emissão de novo fato vivo sem destino por `gerar_perguntas_disputa` (`app/chamados/services/disputa_emissao.py:389`). A pergunta nasce viva sob o mesmo terminal; o evento de recusa apenas registra. Duas tentativas idênticas de religação recusada também geram dois `disputa_religar_recusado`, contrariando “1 trilha” (bloco novo no diff).

2. **Chamado a mais antes de determinar o destino final.** `gerar_perguntas_disputa` abre a pergunta (`app/chamados/services/disputa_emissao.py:389`) e somente depois tenta `reatar_pergunta_orfa` (`:404`, `:409`). O reatamento pode criar/reusar chamado do dia (`app/chamados/reconciliador.py:779`) e costurar a pergunta nele (`:819`), ou reconhecer histórico e resolver (`:1433`, `:1439`). O novo gate terá criado pai vivo antes disso; depois da costura, ele sobra vivo sem aquela pergunta ancorada. A possibilidade é demonstrável pelo fluxo; ocorrência concreta em produção: [nao verificado].

3. **Inversão de ordem de trava.** `ChamadoColaborador.abrir` trava Colaborador (`app/chamados/models.py:669`), renasce chamado encerrado (`:685`, `:686`), chama `reabrir` (`:770`), que ressuscita perguntas (`:1098`), que reabre disputa (`:1189`). O novo `reabrir_sistema` pode então chamar a religação, que trava Disputa. Na direção contrária, a emissão trava Disputa primeiro (`app/chamados/services/disputa_emissao.py:467`) e entra no gate que trava Colaborador (`app/chamados/models.py:669`). Cenário: chamado do dia encerrado renasce e ressuscita pergunta destinada nele; sua disputa fechada também tem outra pergunta viva sem destino sob pai terminal. Concorrência com emissão nessa disputa cria C→D versus D→C. Deadlock executado: [nao verificado].

4. **Retorno antecipado não cobre perguntas existentes.** O dedup de identidade devolve `ex` antes do novo guard (`app/chamados/models.py:2061`, `:2084`), podendo inclusive trocar célula e relavrar (`:2071`, `:2081`). Logo uma entrada que reaproveita pergunta viva sem destino sob pai terminal mantém o defeito. Isso é reaproveitamento, não nascimento novo; não deve ser anunciado como falha no caminho de `create`.

## 29/10 22:07 · sessao 3c6958a11cf4 · o261c2 · sonda · origem=Code

```text
VIGIA viva_sob_pai_terminal (a funcao do pouso): {'perguntas': 807, 'disputas': 73} [nao verificado]
universo repartido 807 (tem de ser == vigia 807) | disputas 73 | colabs 66
status do pai: {'resolvido': 752, 'fechado': 55} | todos terminais: True
ancora (chamado_que_lista) == pai em todas: True
competencia ABERTA 337 | EXPORTADA 470 em 2 empresa x competencia | sem data_evento 0
ABERTA por competencia: {'2:10/2026': 205, '3:10/2026': 10, '4:04/2026': 2, '4:05/2026': 52, '2:11/2026': 66, '3:11/2026': 2}
EXPORTADA por competencia: {'2:09/2026': 446, '3:09/2026': 24}
DISPUTAS: so aberta 33 | so exportada 22 | mista 18 | com sem-data 0
por motivo: {'saida_sem_entrada': 236, 'intervalo_saida': 226, 'intervalo_volta': 211, 'orfao_14h': 109, 'batida_fora_escala_grave': 20, 'sem_posto': 3, 'foto_ausente': 2}
por via_resolucao: {'-': 191, 'completude_turno': 4, 'chamado_encerrado': 316, 'sem_turno_aberto': 287, 'admin_decidiu': 2, 'ausencia_dia': 6, 'celula_absolveu': 1}
por mes do dia: {'2026-04': 20, '2026-05': 34, '2026-08': 150, '2026-09': 382, '2026-10': 221}
p#51322: no universo=True viva=True destino=None disputa=4376 aberta=True pai=19611 status_pai=resolvido
p#34412: no universo=True viva=True destino=None disputa=4376 aberta=True pai=19611 status_pai=resolvido
FORA (nao e desta cura): vivas com DESTINO terminal 2155
FORA (de viva_em_disputa_fechada, dono admin): vivas sem destino em disputa FECHADA sob pai terminal 31
ERROS=0
FIM-DA-SONDA
```

Há a linha `ERROS=0`, que informa zero erros. [nao verificado]  
Universo repartido igual ao vigia: 807 = 807. [nao verificado]  
A soma fecha no universo: 337 + 470 + 0 = 807. [nao verificado]

## 29/10 21:57 · sessao e114400beabb · o261c2 · leitura · origem=Code

**Sim: uma pergunta pode nascer ligada a chamado pai terminal.** O caso mais explícito é a reabertura administrativa: cria a pergunta e só depois tenta reabrir o chamado (`app/chamados/services/acoes_disputa.py:259`, `app/chamados/services/acoes_disputa.py:265`).

Antes do inventário, há dois vínculos diferentes: `PerguntaDisputa.disputa` → `DisputaSupervisao.chamado`, e `PerguntaDisputa.chamado_destino`. Não existe campo direto `PerguntaDisputa.chamado` nessas declarações (`app/chamados/models.py:1849`, `app/chamados/models.py:1324`, `app/chamados/models.py:2283`).

Os conjuntos oficiais são `VIVOS=('aberto','em_analise','registrado')`, `ENCERRADOS=('resolvido','fechado')` e `TERMINAIS=ENCERRADOS+('cancelado','superado')`. Portanto, excluir apenas `ENCERRADOS` deixa passar dois estados terminais (`app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:30`, `app/chamados/catalogo/motor.py:31`).

**1–2) Sítios de criação e possibilidade de pai terminal**

O inventário encontrado contém **seis chamadas de produção a `PerguntaDisputa.abrir`**, discriminadas abaixo, convergindo na inserção física `cls.objects.create(...)` (`app/chamados/models.py:2085`). A classe homônima conceitual `Pergunta`, do catálogo, é uma `dataclass`, não um modelo Django (`app/chamados/catalogo/perguntas.py:14`, `app/chamados/catalogo/perguntas.py:17`).

Três sítios compartilham a escolha de pai por `_abrir_ou_agregar_disputa`: geração comum e as duas declarações. A query é:

```python
DisputaSupervisao.objects.select_for_update().filter(
    colaborador=colaborador, fechada_em__isnull=True,
).first()
```

Ela filtra **disputa aberta**, sem filtrar status do chamado pai e sem recorte de data. O ramo existente devolve essa disputa preservando seu pai; a ordenação padrão da disputa é `-aberta_em` (`app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:502`, `app/chamados/models.py:1390`).

Sem disputa aberta, o mesmo núcleo obtém o pai por:

```python
ChamadoColaborador.abrir(
    colaborador=colaborador,
    categoria='disputa_supervisao',
    modulo_origem='disputa_supervisao',
    status_local__in=VIVOS,
    defaults=...,
)
```

Depois cria a disputa com `chamado=chamado`. `subcategoria` fica nos `defaults`, não participa desse lookup (`app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:514`, `app/chamados/services/disputa_emissao.py:523`).

| Sítio / função | Escolha dos vínculos e status terminal no nascimento |
|---|---|
| **`gerar_perguntas_disputa`**, chamada em `app/chamados/services/disputa_emissao.py:389`; definição em `:280` | Recebe a disputa. O fluxo comum vem de `criar_ou_atualizar_disputa`, usando o núcleo acima: **pai terminal é permitido no reuso de disputa aberta**, pois o status dele não é conferido (`app/chamados/services/disputa_emissao.py:598`, `:606`, `:618`, `:467`). O destino é derivado pela query `ChamadoColaborador.objects.filter(colaborador=disputa.colaborador, status_local__in=VIVOS).exclude(categoria='disputa_supervisao')`: **terminais são excluídos na seleção do destino** (`app/chamados/services/disputa_emissao.py:181`). Há filtros adicionais de motivo atendido, ausência de pergunta não materializada do mesmo motivo, dia compatível e candidato único após desempate por turno (`:194`, `:209`, `:214`, `:219`, `:221`). Pode nascer sem destino; o reatamento ocorre depois da criação (`:404`, `:409`). |
| **`_criar_pergunta_se_couber`**, chamada em `app/chamados/services/disputa_emissao.py:770`; definição em `:756` | Recebe disputa e destino. O agrupador tem **dois caminhos de seleção da disputa**. No repasse, busca `PerguntaDisputa.objects.filter(chamado_destino=ch, materializada_em__isnull=True).select_related('disputa').first()` e usa `_pv.disputa`, **sem exigir disputa aberta nem pai vivo**; chama a complementação, que chega a este sítio (`app/chamados/services/disputa_emissao.py:874`, `:877`, `:885`, `:813`). No caminho geral, usa `DisputaSupervisao.objects.filter(colaborador=colaborador, fechada_em__isnull=True).first()`, também sem status do pai (`:900`). Sem disputa, chama `ChamadoColaborador.abrir` com categoria `disputa_supervisao`, módulo `disputa_supervisao_manual`, subcategoria `orfao_14h`, `status_local__in=VIVOS`, e associa o retorno como pai (`:908`, `:931`). **Pai terminal: sim; disputa já fechada: também possível no repasse.** |
| **`criar_disputa_ancora_de_chamado`**, chamada em `app/chamados/services/disputa_emissao.py:1160`; definição em `:1093` | Escolhe `DisputaSupervisao.objects.select_for_update().filter(colaborador=colaborador, fechada_em__isnull=True).first()`, **sem status do pai** (`app/chamados/services/disputa_emissao.py:1121`). Sem disputa, obtém pai por `ChamadoColaborador.abrir` com categoria `disputa_supervisao`, módulo `disputa_supervisao_manual`, subcategoria `ancora_ausente`, `status_local__in=VIVOS`; associa em `chamado=chamado_pai` (`:1128`, `:1151`). Destino é o argumento `chamado_limbo`, exigindo categoria `ancora_limbo` e recusando apenas `ENCERRADOS`: **pai terminal permitido no reuso; destino cancelado/superado não é barrado pelo teste de status**, enquanto resolvido/fechado é barrado (`:1107`, `:1109`, `:1166`). |
| **`declarar_hora_colab`**, chamada em `app/chamados/services/declaracao_texto.py:322`; definição em `:257` | Obtém disputa por `disputa_para_declaracao`, que delega ao núcleo comum acima: **pai terminal permitido no reuso de disputa aberta** (`app/chamados/services/declaracao_texto.py:307`, `app/chamados/services/disputa_emissao.py:563`, `:467`). Destino é diretamente o argumento `ch`, sem query nem teste de status nesta função; exige módulo de furo e hora correspondente a marco faltante (`app/chamados/services/declaracao_texto.py:271`, `:281`, `:292`, `:323`). **Pelo contrato da função, destino terminal é permitido.** No chamador HTTP atual, porém, `resposta_tardia.receber` antecede a declaração: chamado não vivo precisa renascer ou a resposta retorna como tardia antes de chegar à criação (`app/chamados/views.py:1053`, `:1054`, `:1090`, `app/chamados/services/resposta_tardia.py:35`, `:45`, `:67`). |
| **`aplicar_declaracao`**, chamada em `app/chamados/services/declaracao_texto.py:611`; definição em `:589` | Também obtém disputa por `disputa_para_declaracao` e pelo núcleo comum: **pai terminal permitido no reuso** (`app/chamados/services/declaracao_texto.py:605`, `app/chamados/services/disputa_emissao.py:563`, `:467`). Destino é diretamente `ch`; as propostas verificam módulo, dia e marcos faltantes, **sem filtro de status** (`app/chamados/services/declaracao_texto.py:551`, `:554`, `:563`, `:612`). **Pai e destino podem ser terminais.** O detalhe administrativo busca o chamado por PK e `ticket_id_glpi__isnull=True`, sem status, e chama esta função (`app/chamados/views.py:363`, `:480`). |
| **`acao_reabrir_disputa_admin`**, chamada em `app/chamados/services/acoes_disputa.py:259`; definição em `:232` | Escolhe explicitamente a disputa fechada mais recentemente: `DisputaSupervisao.objects.filter(chamado=ch, fechada_em__isnull=False).order_by('-fechada_em').first()` (`app/chamados/services/acoes_disputa.py:252`). Reabre **a disputa** antes da pergunta; esse método limpa os campos de fechamento da disputa (`:256`, `app/chamados/models.py:1701`, `:1713`). Não passa destino, portanto ele é `None` (`app/chamados/services/acoes_disputa.py:259`, `app/chamados/models.py:2028`). **O chamado pai pode estar resolvido/fechado exatamente no nascimento:** a pergunta nasce em `:259`; `ch.reabrir()` só vem em `:265–266`. |

No sítio `_criar_pergunta_se_couber`, o **destino** depende do ramo do agrupador: chamado de intervalo `_chi`; chamado `ch` no caminho por ata; primeiro chamado do grupo por batida, `chamados_grupo[0]`, na ordem herdada do chamador; ou `ch` no caminho sem batida de origem (`app/chamados/services/disputa_emissao.py:956`, `:978`, `:1000`, `:1012`, `:1043`). Esses chamados passam por `chamado_orfao_e_regularizavel`, cuja regra exclui apenas `ENCERRADOS`: **resolvido/fechado são recusados; cancelado/superado podem passar se satisfizerem as demais guardas** (`app/chamados/services/disputa_emissao.py:870`, `:892`, `:729`, `:691`, `:718`).

**O filtro `status_local__in=VIVOS` passado a `ChamadoColaborador.abrir` não garante retorno vivo em todos os ramos.** Após o lookup inicial, o gate remove os filtros `status_local`, procura chamados com `contexto_json.agrupado_em` e pode devolver o próprio filho terminal quando o outro chamado indicado em `agrupado_em` está vivo. O juiz confere o status desse outro pai, não o do filho devolvido (`app/chamados/models.py:670`, `:689`, `:691`, `:693`, `:695`, `:581`, `:588`, `:596`).

Na **inserção física**, `PerguntaDisputa.abrir` recebe ambos os vínculos e não exige disputa aberta nem chamado vivo. A validação de `chamado_destino` trata da compatibilidade de data/turno, sem conferir status (`app/chamados/models.py:2028`, `:2043`, `:2085`, `app/chamados/juizes.py:1702`). Assim, “excluído por `VIVOS`” acima descreve a seleção; não há revalidação de status no INSERT (`app/chamados/models.py:2085`).

Há ainda uma distinção entre **nascimento e reaproveitamento**: o dedup procura a última pergunta por colaborador, motivo e data, em qualquer disputa. Pode reapontar destino/célula, mas preserva a disputa original e retorna sem criar outra pergunta (`app/chamados/models.py:2022`, `:2061`, `:2064`, `:2071`, `:2084`).

**3) Funções existentes para “chamado vivo do dia” e terminalidade**

**Existe `cobranca_viva_do_dia(colaborador, modulo_origem, dia, excluir=None)`**. Além de colaborador/data, ela exige módulo ou conjunto de módulos (`app/chamados/juizes.py:1610`, `:1620`). Sua seleção é:

```python
ChamadoColaborador.objects.filter(
    colaborador=colaborador,
    modulo_origem__in=_mods,
    status_local__in=VIVOS,
).filter(
    Q(contexto_json__data_turno=dia.isoformat()) | Q(celula__data=dia)
).select_related('celula').order_by('-criado_em', '-pk')
```

Exclui opcionalmente o próprio chamado e retorna o primeiro cujo `dia_do_fato(c, fortes=True)` confirma o dia (`app/chamados/juizes.py:1621`, `:1624`, `:1626`).

Seus chamadores diretos de produção são:

- `_gate_premissa_do_fechamento` — `app/chamados/models.py:498`.
- `sobrevivente_por_ultimo` — `app/chamados/juizes.py:1644`.
- `_autoresolve_f2_f4` — `app/ponto/services/triagem_batida.py:589`.
- `resolver_cobranca_da_entrada` — `app/ponto/services/entrada_sincronizada.py:28`.

Há também seletores plurais: `chamados_vivos_do_dia`, consumido por `fechar_o_fio_do_dia`, e `_chamados_vivos_do_dia`, consumido pelo serviço de cobertura (`app/ponto/services/veredito_celula.py:136`, `:182`, `app/ponto/services/cobertura.py:218`, `:310`).

**Existe teste explícito de terminalidade:** `_resolv(ch)` verifica `status_local in RESOLVIDO`, sendo `RESOLVIDO = TERMINAIS`. É usado para o pai e os destinos por `classificar` e `fechar_beco` (`app/chamados/services/becos.py:13`, `:21`, `:30`, `:31`, `:43`, `:46`). Essas funções são chamadas pelo wrapper do evento e pelo comando de becos; o wrapper é acionado após commit pelo `post_save` da pergunta (`app/chamados/services/becos.py:69`, `:73`, `app/chamados/management/commands/fechar_becos_disputa.py:39`, `:57`, `app/chamados/signals.py:191`, `:200`, `:202`).

`core/juizes.py` registra C1, “o chamado está vivo?”, apontando a autoridade para `catalogo/motor.py::VIVOS`; a pergunta sobre fio vivo é registrada separadamente em C15 (`app/core/juizes.py:107`, `:227`, `:123`, `:241`).

**4) Leitores dependentes desses vínculos**

| Leitor | Regra efetiva |
|---|---|
| **Fila administrativa `qs_na_fila`** | Não busca perguntas: parte de chamados em `VIVOS`, restringe a `COBRANCA` e exclui os silenciados no futuro. **Uma pergunta viva não faz seu pai terminal entrar nessa fila** (`app/chamados/services/painel.py:155`, `:186`, `:187`). |
| **Crédito de perguntas / partição por verbos** | `particionar_verbos` chama `creditos_perguntas`. Este busca perguntas cujo destino **ou** pai está no universo de chamados vivos fornecido; credita ao destino se ele pertence a esse universo, senão ao pai. Se ambos são terminais, a pergunta fica fora dessa seleção (`app/chamados/services/verbos.py:31`, `app/chamados/services/raiox.py:54`, `:144`, `:146`). |
| **`fio.chamado_que_lista`** | Retorna `p.chamado_destino_id or d.chamado_id`, **sem consultar status** (`app/chamados/services/fio.py:116`, `:117`). |
| **Montagem do fio administrativo** | Carrega todos os chamados do colaborador, sem status; inclui perguntas de disputas abertas e perguntas vivas de disputas fechadas. Agrupa por `chamado_que_lista` e anexa ao card. Portanto, pai/destino terminal não é excluído apenas pelo status (`app/chamados/services/fio.py:174`, `:213`, `:218`, `:234`, `:236`, `:249`). |
| **PWA do colaborador** | Usa `montar_chamados_fio(..., so_janela=True)`: restringe chamados à janela e não inclui perguntas de disputas fechadas. Os formulários consultam `p.viva` (`app/chamados/views.py:569`, `app/chamados/services/fio.py:169`, `:213`, `app/templates/chamados/partials/_card_atendimento.html:77`). |
| **Janela do colaborador** | Amplia a janela com **destinos** de perguntas `q_vivas`, não com seus pais. Filtra colaborador, janela ou destino, `silenciado_ate IS NULL` e módulos internos; **não filtra status** (`app/chamados/services/janela_colab.py:61`, `:67`, `:70`). |
| **API nativa `api_chamados_v2`** | Usa essa janela e somente disputas abertas. O destino vence **apenas se estiver em `VIVOS`**; caso contrário, ancora no pai da disputa. Só serializa perguntas nos cards presentes; `pode_responder` é falso para `ENCERRADOS` (`app/api/views.py:1015`, `:1028`, `:1047`, `:1155`, `:1177`). |
| **Questionário separado `api_disputa_perguntas`** | Usa `perguntas_do_app`, selecionando por colaborador, `q_vivas` e `pergunta_viva`, **sem exigir disputa aberta ou pai vivo**. Cada item informa o destino; o envelope usa o primeiro destino pendente não nulo ou o pai da primeira disputa, sem teste de status nessa escolha (`app/api/views.py:2603`, `app/chamados/services/pergunta_no_app.py:42`, `app/api/views.py:2626`, `:2642`). |
| **Lote de validação / mensageria** | `chamado_da_pergunta` retorna destino ou pai, sem status. É consumido na montagem do lote e pela API de mensageria (`app/chamados/services/validacao.py:786`, `:788`, `:869`, `app/api/views_mensageria.py:542`, `:546`). |

Portanto, os leitores **não compartilham uma única regra de âncora**: fio/lote preferem qualquer destino não nulo; crédito administrativo prefere destino pertencente ao universo vivo; API nativa prefere destino cujo status está em `VIVOS` (`app/chamados/services/fio.py:117`, `app/chamados/services/validacao.py:788`, `app/chamados/services/raiox.py:144`, `app/api/views.py:1047`).

Ocorrências concretas no banco de produção e eventual criação por código externo ao recorte: **[nao verificado]**.

## 29/10 21:55 · sessao c6476a4dda70 · - · - · origem=Ronald (tmux)

**Censo: 6 chamadas produtivas a `PerguntaDisputa.abrir`, mais 1 inserção física no próprio gate.** A outra classe `Pergunta` encontrada é `dataclass` de catálogo, sem ORM (`app/chamados/catalogo/perguntas.py:14`, `:17`, `:18`); o modelo persistente é `PerguntaDisputa`, com `disputa` FK obrigatória (`app/chamados/models.py:1849`), e a disputa aponta para `chamado` opcional (`app/chamados/models.py:1324`).

| Sítio / função | Escolha da disputa/chamado pai | Destino escolhido | Terminal no nascimento |
|---|---|---|---|
| `app/chamados/models.py:2085` — `PerguntaDisputa.abrir`, assinatura `:2028` | Recebe `disputa` do caller; não escolhe nem testa `fechada_em`/`disputa.chamado.status_local` (`:2036`, `:2085`). Dedup de fato pode devolver pergunta de outra disputa, sem trocar `ex.disputa`: consulta `do_fato(...).last()` em `:2061`, retorna em `:2084`; identidade filtra apenas colaborador, motivo, data (`:2022`). | Recebe `chamado_destino`; `resolvedora_valida` é chamada em `:2043-2051` e verifica datas/turno, sem status (`app/chamados/juizes.py:1702-1738`). | **Sim**, ambos os vínculos podem apontar para pais terminais se fornecidos; gate não impõe vitalidade. |
| `app/chamados/services/disputa_emissao.py:389` — `gerar_perguntas_disputa`, definição `:280` | Recebe disputa; fluxo usual `criar_ou_atualizar_disputa` chama núcleo em `:598` e gera perguntas em `:606`, `:618`. Núcleo escolhe `DisputaSupervisao.objects.select_for_update().filter(colaborador=colaborador, fechada_em__isnull=True).first()` (`:467-469`), **sem filtrar status do chamado pai**, retorna existente em `:502`. Sem disputa: `ChamadoColaborador.abrir(colaborador=..., categoria='disputa_supervisao', modulo_origem='disputa_supervisao', status_local__in=VIVOS, defaults=...)` (`:504-519`), depois cria disputa com esse pai (`:523-528`). | `_derivar_chamado_destino` chamado em `:380`, `:382`; query exata: `ChamadoColaborador.objects.filter(colaborador=disputa.colaborador, status_local__in=VIVOS).exclude(categoria='disputa_supervisao')` (`:181-184`); filtros adicionais motivo atendido (`:194-196`), ausência de pergunta mesmo motivo não materializada (`:209-212`), dia (`:214-217`), desempate por turno (`:219-220`), exige candidato único (`:221-237`). Pode nascer sem destino e chamar reatamento depois (`:404-410`). | **Pai da disputa: sim**, ramo disputa existente só confere `fechada_em`; **destino derivado: terminais excluídos pela query VIVOS** (conclusão sobre elegibilidade, sem afirmar proteção contra corrida). |
| `app/chamados/services/disputa_emissao.py:770` — `_criar_pergunta_se_couber`, definição `:756` | Recebe disputa. No agrupador `criar_disputa_a_partir_de_chamados_orfaos`, há **dois ramos**: repasse escolhe `_pv = PerguntaDisputa.objects.filter(chamado_destino=ch, materializada_em__isnull=True).select_related('disputa').first()` (`:874`) e usa `_dv = _pv.disputa` (`:877`), **sem filtrar fechamento/status**, gerando irmãs em `:885` → `:813`; ramo geral usa `DisputaSupervisao.objects.filter(colaborador=colaborador, fechada_em__isnull=True).first()` (`:900-902`). Sem disputa, pai por `ChamadoColaborador.abrir(colaborador=..., categoria='disputa_supervisao', modulo_origem='disputa_supervisao_manual', subcategoria='orfao_14h', status_local__in=VIVOS, defaults=...)` (`:908-923`), FK em `:931`. | Recebe destinos da lista do caller: intervalo `_chi` (`:956-958`); por ata `ch` (`:978-979`); grupo por batida escolhe `canonico = chamados_grupo[0]`, primeiro da ordem herdada (`:998-1001`), passado em `:1012-1018`; sem origem passa `ch` (`:1043-1049`); repasse passa `ch` (`:885`). Listas submetidas a `chamado_orfao_e_regularizavel` (`:870`, `:892`), que chama `motivo_fora_da_fabrica` (`:729`). Esse juiz exclui apenas `ENCERRADOS` (`:691-692`), não todos TERMINAIS. | **Disputa: pode estar fechada no repasse** (`:874-885`). **Chamado pai: pode ser terminal**, nenhum teste no reuso. **Destino: resolvido/fechado excluídos; cancelado/superado não excluídos pelo teste de status**, podendo passar demais guardas (`:689-720`). |
| `app/chamados/services/disputa_emissao.py:1160` — `criar_disputa_ancora_de_chamado`, definição `:1093` | `DisputaSupervisao.objects.select_for_update().filter(colaborador=colaborador, fechada_em__isnull=True).first()` (`:1121-1123`), sem status do chamado pai. Sem disputa: `ChamadoColaborador.abrir(colaborador=..., categoria='disputa_supervisao', modulo_origem='disputa_supervisao_manual', subcategoria='ancora_ausente', status_local__in=VIVOS, defaults=...)` (`:1128-1143`), FK em `:1151`. | Argumento `chamado_limbo`, exigindo categoria `ancora_limbo` (`:1107-1108`) e recusando apenas `status_local in ENCERRADOS` (`:1109-1110`); passado como destino em `:1166`. | **Pai da disputa: sim** no reuso; **destino: cancelado/superado permitidos pelo teste de status**, resolvido/fechado recusados. |
| `app/chamados/services/declaracao_texto.py:322` — `declarar_hora_colab`, definição `:257` | `disputa_para_declaracao(...)` (`:307-309`) → núcleo (`app/chamados/services/disputa_emissao.py:563-566`), mesma query/novo pai da linha de `gerar_perguntas_disputa` acima (`:467-528`). | Argumento `ch`, passado em `:323`. Função não filtra status; exige módulo de furo (`:271-272`), data e hora casando marco faltante (`:281-295`). | **Contrato da função: permite destino terminal**, pois não testa status. **HTTP atual protege**: `app/chamados/views.py:1046` pega por `pk,colaborador`, sem status, mas chama `resposta_tardia.receber` em `:1053`; retorna antes da declaração quando tardia (`:1054-1055`). `receber` deixa vivos seguir (`app/chamados/services/resposta_tardia.py:35-36`), ou renasce e atualiza objeto (`:45-47`), ou devolve `tardia` (`:67`). **Chamado pai da disputa continua podendo ser terminal**, por núcleo reutilizar disputa aberta sem testar pai. |
| `app/chamados/services/declaracao_texto.py:611` — `aplicar_declaracao`, definição `:589` | `disputa_para_declaracao(...)` (`:605-607`), mesmo núcleo/queries (`app/chamados/services/disputa_emissao.py:467-528`, `:563-566`). | Argumento `ch`, passado em `:612`. Propostas exigem módulo/dia/marco faltante, **sem status** (`app/chamados/services/declaracao_texto.py:551-568`). | **Sim**, pai da disputa e destino podem ser terminais. HTTP busca `get_object_or_404(ChamadoColaborador, pk=chamado_id, ticket_id_glpi__isnull=True)` (`app/chamados/views.py:363`), sem status; gestor chama aplicar em `:478-480`, sem `resposta_tardia` nesse ramo. |
| `app/chamados/services/acoes_disputa.py:259` — `acao_reabrir_disputa_admin`, definição `:232` | Query **explicitamente fechada**: `DisputaSupervisao.objects.filter(chamado=ch, fechada_em__isnull=False).order_by('-fechada_em').first()` (`:252`); reabre disputa antes (`:256`), via `reabrir_admin` que zera fechamento (`app/chamados/models.py:1701-1713`). | Não passa destino (`app/chamados/services/acoes_disputa.py:259-264`), portanto `None` pelo default (`app/chamados/models.py:2028`). | **Chamado pai pode estar resolvido/fechado no instante exato da criação**: `.abrir` ocorre em `:259`, `ch.reabrir()` apenas **depois**, em `:265-266`. Disputa já está reaberta. |

**Atenção ao significado de `ChamadoColaborador.abrir(... status_local__in=VIVOS)`: não garante retorno vivo em todos os ramos.** O gate primeiro faz `cls.objects.filter(colaborador=colaborador, **lookup).first()` (`app/chamados/models.py:670`), mas depois remove todos os filtros `status_local` (`:691`) e procura objetos com `contexto_json__has_key='agrupado_em'` (`:693-694`), retornando `_f` quando `agrupado_sob_pai_vivo()` (`:695-696`). Esse juiz verifica o status do **outro chamado referido em `agrupado_em`**, não do próprio `_f` (`:581`, `:588-596`). Assim, mesmo a obtenção de pai “novo” nos três emissores pode devolver um chamado terminal agrupado sob outro vivo. O comentário declara expressamente “devolve o filho” (`:689-690`), e a docstring identifica filho resolvido (`:575-580`).

Estados exatos: `VIVOS=('aberto','em_analise','registrado')`, `ENCERRADOS=('resolvido','fechado')`, `TERMINAIS=ENCERRADOS+('cancelado','superado')` (`app/chamados/catalogo/motor.py:24`, `:30-31`).

A enumeração negativa (“nenhum outro create/admin/SQL/serializer”) resulta de busca estática em todos os Python de produção e buscas por SQL/aliases/reverse manager; não há referência única de código capaz de provar ausência global **[não verificado]**. Os seis callers e a inserção física acima são os sítios concretos encontrados.

## 29/10 21:54 · sessao 77a2372554f7 · - · - · origem=Ronald (tmux)

**Mapa dos vínculos:** pergunta → `disputa` é FK obrigatória; o chamado pai é `pergunta.disputa.chamado`; existe também `pergunta.chamado_destino` opcional. Referências: `app/chamados/models.py:1849`, `app/chamados/models.py:1324`, `app/chamados/models.py:2283`.

| Leitor | Dependência e comportamento |
|---|---|
| Fila administrativa `qs_na_fila` | **Não lê perguntas diretamente**: toma `qs_vivos` (`status_local__in=VIVOS`) e restringe a `COBRANCA`, excluindo `silenciado_ate > now`. Portanto chamado terminal não entra nessa fila, mesmo tendo pergunta viva. `app/chamados/services/painel.py:153`, `app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:186`, `app/chamados/services/painel.py:187`. `VIVOS`, `COBRANCA`, `TERMINAIS`: `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`, `app/chamados/catalogo/motor.py:31`. |
| Partição administrativa por verbos | `particionar_verbos` chama `creditos_perguntas`: `app/chamados/services/verbos.py:31`. Este busca perguntas cujo destino **ou** pai pertence ao queryset de chamados vivos: `app/chamados/services/raiox.py:54`. Usa `q_vivas`, exclui materializada com veredito `VL_SIM`: `app/chamados/services/raiox.py:72`, `app/chamados/services/raiox.py:133`. Credita ao destino se ele pertence ao universo vivo fornecido; senão ao pai: `app/chamados/services/raiox.py:140`. Se ambos terminais, pergunta fica fora do universo dessa consulta: inferência direta de `app/chamados/services/raiox.py:54`. |
| `fio.chamado_que_lista` | Retorna **sempre** `p.chamado_destino_id or p.disputa.chamado_id`; não consulta status de nenhum deles: `app/chamados/services/fio.py:116`, `app/chamados/services/fio.py:117`. |
| Montagem do fio admin | Busca todos os chamados do colaborador, sem filtro de status: `app/chamados/services/fio.py:174`. Busca disputas com chamado não nulo: `app/chamados/services/fio.py:181`. Carrega perguntas das disputas abertas e, no admin, perguntas candidatas a vivas das disputas fechadas; confirma estas com `pergunta_viva`: `app/chamados/services/fio.py:213`, `app/chamados/services/fio.py:218`, `app/chamados/services/fio.py:231`. Agrupa pela âncora `chamado_que_lista` e anexa ao card correspondente: `app/chamados/services/fio.py:236`, `app/chamados/services/fio.py:249`. Assim, pai terminal não é excluído só pelo status no fio admin; destino existente ganha a âncora mesmo terminal: `app/chamados/services/fio.py:174`, `app/chamados/services/fio.py:117`. |
| Fio PWA do colaborador | `meu_atendimento` chama `montar_chamados_fio(..., so_janela=True)`: `app/chamados/views.py:566`, `app/chamados/views.py:569`. Nesse modo usa `chamados_visiveis_colab` e **não** inclui perguntas de disputas fechadas: `app/chamados/services/fio.py:169`, `app/chamados/services/fio.py:213`. Os formulários usam as perguntas anexadas ao card e `p.viva`: `app/templates/chamados/partials/_card_atendimento.html:73`, `app/templates/chamados/partials/_card_atendimento.html:77`. |
| Janela dos chamados do colaborador | Estende a janela com IDs de **destinos**, não pais, de perguntas `q_vivas`: `app/chamados/services/janela_colab.py:61`. Consulta chamados por janela OR esses destinos, colaborador, `silenciado_ate IS NULL`, excluindo módulos internos. **Não há filtro de status atual nessa função**: `app/chamados/services/janela_colab.py:67`, `app/chamados/services/janela_colab.py:70`. Portanto não reproduzir literalmente comentários antigos dizendo que todo terminal some da lista. |
| API nativa `/chamados` (`api_chamados_v2`) | Usa a mesma janela: `app/api/views.py:1015`. Considera apenas disputas abertas: `app/api/views.py:1028`. Sua âncora difere do fio: destino vence somente quando `status_local in VIVOS`; caso contrário usa `p.disputa.chamado_id`: `app/api/views.py:1043`, `app/api/views.py:1047`. Serializa perguntas por card presente na lista: `app/api/views.py:1122`, `app/api/views.py:1158`, `app/api/views.py:1177`. Campo `pode_responder` é falso para `ENCERRADOS`: `app/api/views.py:1155`. |
| Questionário separado do app (`api_disputa_perguntas`) | Usa `perguntas_do_app`: `app/api/views.py:2603`. A seleção filtra colaborador por disputa, `q_vivas` e `pergunta_viva`, **sem exigir disputa aberta nem pai vivo**: `app/chamados/services/pergunta_no_app.py:42`. Cada item informa `chamado_id=p.chamado_destino_id`: `app/api/views.py:2626`. O `chamado_id` do envelope é o primeiro destino não nulo de pergunta não RESPONDIDA, ou o pai da primeira disputa; **não valida status desse destino/pai**: `app/api/views.py:2615`, `app/api/views.py:2642`. |
| Detalhe local administrativo | Resolve apenas disputa aberta vinculada diretamente ao chamado exibido; depois filtra suas perguntas por `pergunta_viva`. Não usa âncora destino para selecionar a disputa no GET: `app/chamados/services/acoes_chamado.py:335`, `app/chamados/services/acoes_chamado.py:341`, `app/chamados/services/acoes_chamado.py:348`. |
| Portas administrativas que consomem a listagem | Validar/recusar/corrigir no detalhe verificam igualdade com `chamado_que_lista`, exigindo disputa aberta: `app/chamados/views.py:493`, `app/chamados/views.py:499`, `app/chamados/views.py:510`, `app/chamados/views.py:516`, `app/chamados/views.py:528`, `app/chamados/views.py:534`. No fio, validação permite disputa fechada se pergunta continua viva: `app/chamados/services/fio.py:589`, `app/chamados/services/fio.py:592`, `app/chamados/services/fio.py:594`, `app/chamados/services/fio.py:599`; recusar/corrigir ainda exigem disputa aberta: `app/chamados/services/fio.py:626`. |
| Fila de validação em lote / IA | `chamado_da_pergunta` também retorna destino ou pai, sem status: `app/chamados/services/validacao.py:786`. Consumidores: montagem do lote `app/chamados/services/validacao.py:869` e resposta da API de mensageria `app/api/views_mensageria.py:542`, `app/api/views_mensageria.py:546`. |
| Juiz de irmãs com resposta pendente | Seleciona pergunta por destino `=ch` OU pai `=ch` quando destino não está em `VIVOS`; depende expressamente da mesma regra de crédito: `app/chamados/juizes.py:1471`, `app/chamados/juizes.py:1474`. |

**Conclusão para o relatório:** há três regras de âncora diferentes: fio/lote usam destino não nulo ou pai; crédito administrativo usa destino pertencente ao universo vivo ou pai; API nativa usa destino de status VIVOS ou pai. Referências respectivas: `app/chamados/services/fio.py:117`, `app/chamados/services/validacao.py:788`, `app/chamados/services/raiox.py:144`, `app/api/views.py:1047`.

## 29/10 21:54 · sessao 293a5aca92dc · - · - · origem=Ronald (tmux)

Evidências para o item 3:

- **Há seletor explícito de chamado vivo do dia:** `cobranca_viva_do_dia(colaborador, modulo_origem, dia, excluir=None)`, em `app/chamados/juizes.py:1610`. Filtra `colaborador`, `modulo_origem__in` e `status_local__in=VIVOS`; restringe a `contexto_json__data_turno=dia.isoformat()` **ou** `celula__data=dia`; ordena por `-criado_em, -pk`; exclui opcionalmente o próprio chamado; retorna o primeiro para o qual `dia_do_fato(c, fortes=True) == dia` (`app/chamados/juizes.py:1620–1626`).
- Seus chamadores diretos de produção são `_gate_premissa_do_fechamento`, em `app/chamados/models.py:498`; `sobrevivente_por_ultimo`, em `app/chamados/juizes.py:1644`; `_autoresolve_f2_f4`, em `app/ponto/services/triagem_batida.py:589`; e `resolver_cobranca_da_entrada`, em `app/ponto/services/entrada_sincronizada.py:28`.
- `sobrevivente_por_ultimo` põe o chamado escolhido por aquele juiz por último no lote (`app/chamados/juizes.py:1641–1647`). É chamado por `fechar_o_fio_do_dia` (`app/ponto/services/veredito_celula.py:182`), pelo cartório (`app/ponto/services/cartorio.py:1196`) e pelo serviço de cobertura (`app/ponto/services/cobertura.py:310`).

**Status declarados:**

```python
VIVOS      = ('aberto', 'em_analise', 'registrado')
COBRANCA   = ('aberto', 'em_analise')
ENCERRADOS = ('resolvido', 'fechado')
TERMINAIS  = ENCERRADOS + ('cancelado', 'superado')
```

A declaração está em `app/chamados/catalogo/motor.py:24`, `:29`, `:30`, `:31`. O registro de juízes formula C1 como “o chamado está vivo?” (`app/core/juizes.py:107`) e aponta a autoridade para `chamados/catalogo/motor.py::VIVOS` (`app/core/juizes.py:227`); separa a pergunta sobre o fio em C15 (`app/core/juizes.py:123–125`, `:241`).

**Há função que testa terminalidade**, embora local ao serviço de becos: `_resolv(ch)` retorna `ch is not None and getattr(ch, 'status_local', None) in RESOLVIDO` (`app/chamados/services/becos.py:21–22`), sendo `RESOLVIDO = TERMINAIS` (`app/chamados/services/becos.py:11–13`). Portanto, não cabe afirmar ausência absoluta de função “pai terminal”.

- `classificar(d)` usa `_resolv` para destinos das perguntas (`app/chamados/services/becos.py:30`) e para `d.chamado`, o pai (`:31`).
- `fechar_beco(d, sistema)` usa para o pai (`app/chamados/services/becos.py:43`) e destinos (`:46`).
- `classificar` é chamado pelo wrapper `fechar_beco_da_disputa` (`app/chamados/services/becos.py:69`) e pelo comando `fechar_becos_disputa` (`app/chamados/management/commands/fechar_becos_disputa.py:39`).
- `fechar_beco` é chamado pelo wrapper (`app/chamados/services/becos.py:73`) e pelo comando (`app/chamados/management/commands/fechar_becos_disputa.py:57`).
- O wrapper é chamado após commit pelo signal `post_save` de `PerguntaDisputa` (`app/chamados/signals.py:191–202`).

Outros seletores do dia existentes:

- `chamados_vivos_do_dia(colaborador, data, cel=None)` reúne chamados com `status_local__in=VIVOS` e `contexto_json__data_turno=data.isoformat()`, somando os `cel.chamados` também vivos sem duplicar (`app/ponto/services/veredito_celula.py:136–157`). Chamador direto: `fechar_o_fio_do_dia`, `app/ponto/services/veredito_celula.py:182`.
- `_chamados_vivos_do_dia(titular, dia)` percorre chamados do titular em `VIVOS`, ordenados por `-criado_em,-pk`; aceita data da célula igual ao dia ou, sem data de célula, a data declarada pelo catálogo (`app/ponto/services/cobertura.py:218–231`). Chamador: `app/ponto/services/cobertura.py:310`.
- `agrupado_sob_pai_vivo` é específico do pai registrado em `contexto_json.agrupado_em`: busca por PK, verifica mesmo colaborador e retorna `pai.status_local in VIVOS` (`app/chamados/models.py:581–596`). Seus leitores de produção: fechamento (`app/chamados/models.py:495`), dedup de abertura (`:695`), impedimento de reabertura (`:1012`), badge do espelho (`app/escala/utils.py:1036`) e calendário do colaborador (`app/colaboradores/services/calendario.py:556`).

_272 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
