# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 12:24** · publicado em **09/11/2026 12:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 12:24 · sessao eab8c15e7201 · FIO-E0-V4-E3 · verificador · origem=Code

**Há achados PROCEDENTES de unidade residual, custo e leitura indireta na sombra; os quatro consumidores principais estão unificados.** (`app/chamados/services/aviso_pendencia.py:13`; `app/chamados/juizes.py:1149`; `app/core/utils.py:18`; `app/api/views.py:325,2562`; `app/api/views_core.py:314`; `app/core/views.py:253`)

1. **UNIDADE — NAO PROCEDE nos quatro consumidores; PROCEDENTE no restante do app.**

   Badge dos dois kernels, dashboard e `api_disputa_perguntas` leem `consulta_do_app`. Ela aplica C6 e separa pendentes por **sem resposta + disputa aberta**. Fechada viva vai ao `historico`; só entra também em `respondidas` quando possui `respondida_em`. (`app/chamados/services/pergunta_no_app.py:54,67`; `app/api/views.py:2588,2600`)

   Permanecem fontes paralelas:

   - **PROCEDENTE:** aviso após mensagem recompõe C6 + sem resposta, incluindo fechada viva, e informa “AGUARDANDO sua resposta”. A cobrança reutiliza esse leitor. **Cura:** ambos consumirem `consulta_do_app()['pendentes']`. (`app/chamados/services/aviso_pendencia.py:9,13,21`; `app/api/views.py:1013,2236`; `app/chamados/services/cobranca.py:47`)
   - **PROCEDENTE:** `api_chamados_v2` carrega todas as perguntas das disputas abertas, sem C6, e entrega outra lista em `perguntas_disputa`. Pode servir pergunta morta. **Cura:** consumir a seleção/classificação central. (`app/api/views.py:1044,1059,1174`)
   - **FILA — universos internos distintos:** placar/copiloto e `_no_app` continuam considerando todas as C6 vivas, inclusive fechadas agora disponíveis apenas no histórico. Definir se histórico satisfaz “pergunta no app”. (`app/chamados/juizes.py:1489`; `app/chamados/services/pergunta_no_app.py:160,183`; `bin/placar_code.sh:317`)
   - **FILA — dívida preexistente:** há contagens por carimbo cru na cobrança, ações da disputa, censo da folha e contador com cron. Não certificam pendência apresentada pelo app. (`app/chamados/services/cobranca.py:229`; `app/chamados/services/acoes_disputa.py:95`; `bin/censo_folha.py:28`; `app/chamados/services/furo_sem_canal.py:80`; `app/config/crons.py:531`)

   Fechada viva também pode aparecer no fio administrativo e seus templates: esse caminho aplica C6 às fechadas; o fio do colaborador usa outro recorte. (`app/chamados/services/fio.py:216,238,253`; `app/templates/chamados/partials/modal_fio.html:156`)

2. **JUIZ ÚNICO C17 — NAO PROCEDE a acusação de cópia da partição ou ligação antecipada.**

   A tarefa chama `alimentar_fila`, que chama as autoridades existentes `particionar_verbos(qs_vivos(), agora=...)` e `qs_na_fila(agora=...)`. A partição chama `creditos_perguntas` e `verbo_do_chamado`; `_portas_da_tarefa` compõe portas a partir das ações já julgadas pelo C19. (`app/chamados/services/tarefa_do_dia.py:409,425`; `app/chamados/juizes.py:1429,1432`; `app/chamados/services/verbos.py:34,47`; `app/core/juizes.py:291`)

   C17 **ainda não virou fachada delegante**: conserva seu julgamento. Os três chamadores permanecem folha, vigia e cadastro; seus corpos não constam como alterados no diff fornecido. Comparação histórica independente com a base: **[nao verificado]**. (`app/chamados/juizes.py:1545`; `app/folha/fora_da_folha.py:67`; `app/chamados/services/vigia_fio.py:635`; `app/escala/services/cadastro_realidade.py:488`)

   Os campos antigos da tarefa são preservados; entram quatro chaves aditivas. A igualdade do dicionário inteiro muda, mas não foi localizado consumidor anterior de produção sendo migrado; o novo consumidor é o DIFF. Chamadas dinâmicas não localizáveis: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:177,433`; `app/chamados/management/commands/diff_e3_sombra.py:118`)

3. **L-111 — NAO PROCEDE a permanência do método.**

   Método removido, lápide no sítio; busca AST/textual não encontrou chamada, alias importado, `getattr` literal ou template restante. Construção dinâmica do nome: **[nao verificado]**. (`app/chamados/models.py:1795`; `app/chamados/tests/test_badge_disputa_pendente.py:8`)

   Os testes migrados ainda exigem: zero sem filhos, duas perguntas → duas pendências, desaparecimento após absolvição e concordância entre endpoint, dashboard e dois kernels. (`app/chamados/tests/test_esmeril2_left_join.py:19,27`; `app/chamados/tests/test_fio_le_o_juiz.py:267,272`; `app/chamados/tests/test_e3_app_orm.py:28,39,47`)

4. **SOMBRA — PROCEDENTE a objeção à cadeia estritamente só de leitura.**

   O comando não contém escritor ORM direto e envolve todos os aliases na guarda SQL. Porém `dias_trancados` chega a `get_user_sistema`, que executa **`get_or_create`**. Se o usuário faltar, haverá tentativa de criação; a guarda impede que o SQL escritor alcance o cursor. **Cura:** resolver por leitura `.get`, falhar explicitamente na ausência e testar esse ramo; a fixture atual sempre cria `sistema`. (`app/chamados/management/commands/diff_e3_sombra.py:34,92,108`; `app/chamados/services/tarefa_do_dia.py:370`; `app/chamados/juizes.py:1033,1146`; `app/core/utils.py:18`; `app/chamados/tests/e3_fixtures.py:26`)

   **PROCEDENTE, no caminho de erro:** C6 pode registrar exceção/traceback; a configuração SaaS inclui arquivo de log. A sanitização final do comando não desfaz essa emissão. **Cura:** tratamento sanitizado durante o instrumento e teste das dependências. Configuração efetiva e ocorrência em produção: **[nao verificado]**. (`app/chamados/juizes.py:1366,1377`; `app/core/observ.py:79`; `app/config/settings/saas.py:138,183`; `app/chamados/management/commands/diff_e3_sombra.py:98`)

   **NAO PROCEDE — agendamento:** `FORA_DE_PIPELINE` apenas declara; geração do cron percorre `CRONS`, e inventário executável reúne `CRONS + ENCADEADOS`. O diagrama apenas baixa tela de 110 para 109 sítios; não liga fachada nem acrescenta execução. (`app/config/crons.py:769,1798,1865`; `app/docs/ARQUITETURA.mmd:356`; `app/docs/MAPA.md:209`)

5. **REGISTRO DE TELA — NAO PROCEDE novo juiz ou alteração adicional nesse arquivo.**

   No diff fornecido, `core/juizes.py` perde somente o pendente do dashboard; C17 e C19 continuam registrados. A leitura curada está no dashboard. (`app/core/juizes.py:289,291,756`; `app/core/views.py:253`)

   O selo de tela sozinho verifica impressões remanescentes, portanto não prova a cura funcional. O mérito vem da matriz ORM que exige duas perguntas na mesma disputa → duas pendências nos consumidores completos. (`app/core/tests/test_contract_juiz_tela.py:34`; `app/chamados/tests/test_e3_app_orm.py:28,39,47`)

6. **CLASSES DE REPROVAÇÃO.**

   - **PROCEDENTE — catraca incompleta:** resolve aliases de importação, mas não `p = porta_alcancavel_do_dia` nem `getattr(j, 'porta_alcancavel_do_dia')(...)`; também exclui `models.py` e `core/juizes.py`. Esses desvios escapam da contagem. **Cura:** ampliar resolução/escopo e acrescentar mutantes dessas formas. Isso não demonstra quarto chamador atual. (`app/chamados/tests/test_e3_pendentes_app.py:49,67,98,113`)
   - **NAO PROCEDE — quatro casos sem mordida:** há expectativas próprias para só fila, A/B, verbos do universo e isento, com mutantes correspondentes. O verbo inventado encontra ainda expectativa explícita de `validar`. Nem todo teste novo precisa falhar sem a cura: o contrato preservado de C17 é regressão. Causa individual das 30 mortes apenas por `rc=1`: **[nao verificado]**. (`app/chamados/tests/test_e3_portas_tarefa.py:32,40,49,57,75`; `app/chamados/tests/e3_mutantes.py:49,127`; `app/chamados/tests/test_e3_pendentes_app.py:126`)
   - **NAO PROCEDE — nova inversão de travas:** a extensão escreve mapas em memória, sem introduzir aquisição de trava; a tentativa escritora indireta está no item 4. (`app/chamados/juizes.py:1428`; `app/chamados/services/tarefa_do_dia.py:425`)
   - **FILA — relógio integral da comparação:** o comando recebe instante explícito e o propaga, mas C17 ainda chama C6, cujo fallback lê `date.today()`. Portanto `--agora` não controla toda a comparação histórica. **Cura:** explicitar esse limite ou alimentar o horizonte dessa dependência antes de alegar reprodução integral. (`app/chamados/management/commands/diff_e3_sombra.py:84,107`; `app/chamados/juizes.py:1472,1357,575`)
   - **NAO PROCEDE — ambiguidade nova demonstrada:** tarefa exclui conversa ao agrupar `data_evento`; conversa continua gravando dia civil, fato usa dia do turno, e `data_civil` da instância deriva do instante do marco. Silêncio usa `> agora`; igualdade retorna à fila/histórico. Decisões e efeitos usam `<= agora`; janela de dias inclui as duas pontas. (`app/chamados/models.py:2786,2798`; `app/chamados/services/tarefa_do_dia.py:102,325,339,358`; `app/chamados/services/painel.py:190`; `app/chamados/services/verbos.py:51`; `app/chamados/management/commands/diff_e3_sombra.py:114`)

   `rota='porta_fila'` pode coexistir com `fila=()` quando o verbo é histórico: rota usa a âncora exigível na célula; `fila` exclui verbos fora da lista padrão. O caso está explicitamente testado. (`app/chamados/services/tarefa_do_dia.py:428,429,433`; `app/chamados/tests/test_e3_portas_tarefa.py:109`)

7. **CUSTO — PROCEDENTE: não há número constante universal no badge.**

   A consulta inicial é uma, mas C6 consulta tranca por pergunta; competência é memoizada, toque negativo não. **Cura:** alimentar tranca/toque em lote por chamada, preservando frescor entre chamadas. (`app/chamados/services/pergunta_no_app.py:47,54`; `app/chamados/juizes.py:948,1020,1137,1151`)

   Contagem estática incremental de `consulta_do_app`, não de `api_me` inteiro:

   | Cenário | SELECTs |
   |---|---:|
   | Nenhuma pergunta candidata | **1** (`pergunta_no_app.py:47`) |
   | Sem resposta, célula acusando, competências abertas | **1 + E + K** (`juizes.py:1003,1023,1342`) |
   | N perguntas, uma competência trancada, nenhum toque humano, células/chamados existentes, `sistema` existente e memo frio | **3 + E + 5N** (`juizes.py:1146,1185,1200,1211,1224,1234`) |

   `E=1` se Empresa precisar ser carregada; `K` é o número de competências ainda não memoizadas. Empresa fria no último cenário: **4 + 5N**. Outros ramos de C6 variam; quantidade observada em produção: **[nao verificado]**. (`app/chamados/juizes.py:1003,1020,1361,1374`)

8. **QUEM ESCREVE / QUEM LÊ — rastro principal, inclusive fora do diff.**

   | Campo/função | Produtor/escritor | Leitores |
   |---|---|---|
   | `pendentes/historico` | `pergunta_no_app.py:65` — memória | `api/views.py:325,2562`; `api/views_core.py:314`; `core/views.py:253` |
   | `respondida_em/resposta_colab` | `disputa_emissao.py:1464,1650,1671`; reabertura `:1590,1629`; carimbo `models.py:3121` | Consulta `pergunta_no_app.py:67`; C6 `juizes.py:1309`; crédito `raiox.py:139` |
   | `fechada_em` | Fecha `models.py:1929`; reabre `:2146,2170` | Consulta `pergunta_no_app.py:67`; fio `fio.py:216`; lista paralela `api/views.py:1046` |
   | `validada_em/materializada_em/via_resolucao` | `validacao.py:134,199`; `models.py:3088,3179,3253` | C6 `juizes.py:1293,1304,1330`; crédito `raiox.py:135` |
   | Âncora/data da pergunta | Nascimento `models.py:2786,2872`; portas `fio.py:804,970` | `chamado_que_lista`, `fio.py:116`; tarefa `tarefa_do_dia.py:329,362`; C17 `juizes.py:1549` |
   | `veredito_lavrado/julgado_em/janela_expira_em` | `veredito_lavrado.py:250,270` | Crédito `raiox.py:135`; tarefa `tarefa_do_dia.py:350` |
   | Decisões, `ato_em`, efeitos | Sequência `models.py:3934`; resposta/validação `disputa_emissao.py:1653`, `validacao.py:139` | Tarefa `tarefa_do_dia.py:337,346,358` |
   | `verbo/fila_ids/fila_cel` | `juizes.py:1429` — memória, autoridades painel/partição | `juizes.py:1465`; `tarefa_do_dia.py:409,426` |
   | `rota/fila/verbos/fio` | Tarefa `tarefa_do_dia.py:433`; C17 separado `juizes.py:1556` | DIFF `diff_e3_sombra.py:43,50`; atuais C17: `fora_da_folha.py:78,225`, `vigia_fio.py:643`, `cadastro_realidade.py:488` |
   | `status_local/silenciado_ate` | `models.py:228,296,1095`; `services/arquivo.py:166,202` | `painel.py:155,189`; `verbos.py:45,51` |
   | `modulo_origem/urgencia/criado_em` | `models.py:1095,550,67` | `verbos.py:45,47,57,60` |
   | `dia_do_turno/agora/data_civil` | Entrada `tarefa_do_dia.py:442`; parsing `diff_e3_sombra.py:83`; saída `tarefa_do_dia.py:102,177` | Carga/horizontes `tarefa_do_dia.py:317,339,358,411`; partição `verbos.py:51` |

   Caminhos dessa tabela são relativos a `app/`. Censo recursivo exaustivo de **todos** os escritores/leitores dinâmicos de cada insumo transitivo: **[nao verificado]**. DIFF medido por chamador/caso em produção e liberação da fachada: **[nao verificado]**.

## 09/11 12:22 · sessao c3a90278ff0c · - · - · origem=Ronald (tmux)

- **NAO PROCEDE — segundo juiz de rota/fila/verbos.** A extensão compõe ações já decididas e lê a partição existente: `tarefa_do_dia.py:403`, `:409`, `:425-434` → `juizes.py:1429`, `:1432` → `verbos.py:34`, `:47` → `catalogo/natureza.py:72`. A autoridade permanece C19 (`core/juizes.py:291`).

- **NAO PROCEDE — migração prematura.** C17 continua julgando em `juizes.py:1545-1557`; seus três consumidores continuam em `folha/fora_da_folha.py:67`, `vigia_fio.py:635`, `cadastro_realidade.py:488`. O diff fornecido não altera esses corpos; comparação independente com a base, sem `.git`, **[nao verificado]**.

- **NAO PROCEDE — alteração de campos antigos da tarefa.** A projeção E3 escreve somente `rota/fila/verbos/fio` (`tarefa_do_dia.py:433-434`); a saída anterior nasce em `:177-178`. Igualdade do dicionário completo muda pela adição das chaves; nenhum consumidor de produção externo anterior de `tarefa_do_dia` foi localizado; o novo consumidor é `diff_e3_sombra.py:118`. Exaustividade contra chamadas dinâmicas **[nao verificado]**.

- **NAO PROCEDE — semântica temporal das comparações novas.** Silêncio vigente usa `silenciado_ate > agora` tanto na fila quanto no verbo; igualdade significa retorno à fila/histórico (`painel.py:190`, `verbos.py:51`). Teto usa `marco+teto <= agora` (`ponto/services/cartorio.py:858`); decisões, passivo e efeitos usam horizonte inclusivo (`tarefa_do_dia.py:339`, `:347`, `:358`). O intervalo de dias é inclusivo (`diff_e3_sombra.py:105-106`, `:114`).

- **FILA — horizonte completo do DIFF não é controlado por `--agora`.** O comando entrega o instante à alimentação (`diff_e3_sombra.py:107`), mas esta chama C6 (`juizes.py:1472`), cujo fallback pode ler `date.today()` (`juizes.py:1357`, `:575`). É dependência existente, sem leitura direta nova do relógio no comando; medir com instante presente ou tornar explícito o horizonte do C6 antes de alegar reprodução temporal integral.

Mapa dos campos e funções:

| Produção/escrita | Leitura |
|---|---|
| `alimentar_fila`: `juizes.py:1429-1438` | `alimentar_portas`, `juizes.py:1465`; tarefa, `tarefa_do_dia.py:409`, `:426-429` |
| `alimentar_portas`: `juizes.py:1470-1476` | `folha/fora_da_folha.py:62`; `vigia_fio.py:604`; `cadastro_realidade.py:484`; C17, `juizes.py:1545`; DIFF, `diff_e3_sombra.py:107` |
| Projeção tarefa: `tarefa_do_dia.py:433-434`; projeção C17: `juizes.py:1556-1557` | Folha lê fio/fila em `fora_da_folha.py:78`, `:225`; vigia lê verbos/rota em `vigia_fio.py:643-646`; cadastro lê existência em `cadastro_realidade.py:488`; DIFF lê em `diff_e3_sombra.py:43`, `:50-57`, `:133-134` |
| `agora`: entrada/parsing `diff_e3_sombra.py:83-88`, guarda `tarefa_do_dia.py:317` | `tarefa_do_dia.py:339`, `:347`, `:350`, `:358`, `:363`, `:411` |
| `dia_do_turno`: argumento `tarefa_do_dia.py:442`, saída `:177`; pergunta agrupada por `data_evento`, `:324-329` | Instâncias e seleção, `:393`, `:400-405` |
| `data_civil`: conversão do instante do marco, `tarefa_do_dia.py:102` | Saída por instância, `:186`, sem substituir a chave de agrupamento `:329` |
| `status_local`: nascimento `models.py:1095`; mudança `:228-229`, persistência `:296` | `painel.py:155`, `:189` |
| `silenciado_ate`: `arquivo.py:166-172`, `:202-210` | `painel.py:190`; `verbos.py:45`, `:48-51` |
| `modulo_origem`: nascimento por `**campos`, `models.py:1095`, campo `:46` | `verbos.py:45`, `:47` → `catalogo/natureza.py:44-54` |
| `urgencia`: nascimento `models.py:1095`; escala `:550-553`; rebaixa `:470-473`; restaura `:251-253` | `verbos.py:45`, `:60-61` |
| `criado_em`: `auto_now_add`, `models.py:67` | Atraso, `verbos.py:45`, `:57-58` |
| Dia da pergunta: `models.py:2786`, derivação `:2871-2875`; conversa civil `:2798` | Tarefa exclui conversa e agrupa por dia do fato, `tarefa_do_dia.py:325-329` |
| Âncora: `fio.py:116-117` | `tarefa_do_dia.py:125`, `:362`; C17 `juizes.py:1549`; DIFF `diff_e3_sombra.py:63` |

Outros escritores dinâmicos além dos produtores apontados não foram certificados exaustivamente **[nao verificado]**.

## 09/11 12:21 · sessao 56d01450e0e8 · - · - · origem=Ronald (tmux)

- **NAO PROCEDE — divergência nos quatro consumidores solicitados:** ambos badges usam `len(consulta_do_app(... )['pendentes'])`, dashboard idem e endpoint lê a mesma consulta: `app/api/views.py:325`, `app/api/views_core.py:314`, `app/core/views.py:253`, `app/api/views.py:2562`. Universo passa por C6 em `app/chamados/services/pergunta_no_app.py:54`; recorte pendente exige ausência de resposta e disputa aberta em `:67`. Fechada viva vai para histórico em `:70`, servido em `app/api/views.py:2588`; não necessariamente para `respondidas`, cujo filtro é `respondida_em` em `:2600`.

- **PROCEDENTE — fonte paralela de pendência ao próprio colaborador:** `app/chamados/services/aviso_pendencia.py:7` recompõe seleção por q_vivas+C6+sem resposta em `:9`–`:13`, sem excluir disputa fechada. `aviso_texto_livre` usa essa lista (`:17`) e afirma “AGUARDANDO sua resposta” (`:21`); é chamado após mensagem do app em `app/api/views.py:1012`. Assim uma pergunta fechada viva, removida das pendentes pelo recorte em `pergunta_no_app.py:67`, ainda dispara aviso de pendência. Cura: consumidor ler `consulta_do_app()['pendentes']`.

- **PROCEDENTE — outra lista de perguntas entregue pelo app:** `api_chamados` monta `disputas_abertas` diretamente em `app/api/views.py:1044`, carrega todas as respectivas perguntas sem C6 em `:1059`, agrupa em `:1065` e serializa em `:1174`–`:1193`. Pode entregar pergunta morta nessa lista; não é a mesma seleção da consulta. Cura sugerida: obter perguntas e classificação da consulta única, conforme contrato dessa outra tela.

- **FILA — leitores internos preexistentes com universos distintos:** `_no_app` continua usando todas as C6 vivas em `app/chamados/juizes.py:1489`; contador `medir` e copiloto `do_dia` usam esse universo em `app/chamados/services/pergunta_no_app.py:160` e `:183`. O placar lê o comando em `bin/placar_code.sh:317`, que chama `medir` em `app/chamados/management/commands/chamados_vivos_sem_pergunta_no_app.py:14`; portanto pode considerar “no app” uma fechada viva que agora está no histórico. Cura em fila: explicitar se disponibilidade no histórico satisfaz esse contador; autorização para mudar a semântica interna além do recorte dado [nao verificado].

- **FILA — demais contadores/listagens paralelos:** `app/chamados/services/cobranca.py:229` só aplica q_vivas+sem resposta+aberta, sem C6, e devolve totais em `:248`; `app/chamados/services/acoes_disputa.py:95` e `app/chamados/services/disputa_emissao.py:1709` contam carimbo cru para mensagens de pendência; `app/chamados/views_cobrar.py:175` lista sem resposta sem C6. `bin/censo_folha.py:28` conta carimbo cru e rotula `PERG_VIVAS` em `:37`. `app/chamados/services/furo_sem_canal.py:80` usa só ausência de resposta para “pergunta aberta”, devolvendo contagem em `:95`; há cron desse contador em `app/config/crons.py:531`. São preexistentes; cura: leitores precisarem de pendência apresentada devem usar a consulta, leitores de vitalidade devem usar C6.

- **NAO PROCEDE — L-111:** AST de todos os `.py` procurou definição, atributo e string literal do método; único nó encontrado foi a string do `hasattr` em `app/chamados/tests/test_badge_disputa_pendente.py:8`. Busca textual encontrou somente lápide/comentários/teste, nenhum chamador/template restante [nao verificado quanto a construção dinâmica de nome]. Método ausente no sítio, seguido de `Meta` em `app/chamados/models.py:1798`; lápide presente em `:1795`. Testes migrados mantêm asserções de ausência de fantasma e duas perguntas→2 em `test_esmeril2_left_join.py:19` e `:27`, morte pela absolvição em `test_fio_le_o_juiz.py:267` e `:272`. Matriz cruza endpoint, dashboard e dois kernels em `test_e3_app_orm.py:28`–`:45`, incluindo fechadas/respondidas em `:52`–`:60`. Execução própria [nao verificado].

- **PROCEDENTE — N+1 permanece no badge, sem número constante universal:** consulta faz uma query inicial (`pergunta_no_app.py:47`–`:54`), porém C6 chama tranca por pergunta (`juizes.py:1297`, `:948`). Empresa é carregada uma vez se fria (`:1003`), tranca uma vez por competência (`:1020`–`:1024`; SELECT em `app/ponto/services/fechamento.py:1470`). Toque positivo é memoizado, negativo não (`juizes.py:1137`, `:1151`). Para N perguntas em uma competência trancada, sem toque humano, todas com célula e chamado, usuário sistema existente e memo frio: **`Q=3+E+5N`**, E=1 se Empresa fria, senão 0; os cinco SELECTs ficam em `:1185`, `:1200`, `:1211`, `:1224`, `:1234`. Empresa fria resulta **`4+5N`**. Sem perguntas, **1**; perguntas sem resposta com célula acusando e competências abertas: **`1+E+K`**, K competências ainda não memoizadas (`:1342`, `:1020`). É custo incremental de `consulta_do_app`, não da view inteira; medida real em produção [nao verificado]. Cura: alimentar tranca/toque por lote de pares em cada chamada, preservando frescor do negativo.

- **PROCEDENTE — dependência potencialmente escritora na sombra:** tranca chama `_admin_tocou_os_dias_depois` (`juizes.py:1033`), que chama `get_user_sistema` (`:1145`); este faz `User.objects.get_or_create` em `app/core/utils.py:18`. Ausente o usuário, tentaria INSERT; a guarda do comando pode bloquear a tentativa, mas a cadeia não contém apenas leitores. Cura: resolver sistema por leitura `.get` no caminho da sombra, sem criação implícita.

Mapa resumido: `consulta_do_app` produz mapas/listas somente em memória (`pergunta_no_app.py:65`–`:71`), lidos nos quatro sítios acima; `respondida_em` é lido pelo recorte em `:67` e escrito, entre outros, pela porta `PerguntaDisputa.carimbar` (`models.py:3121`, persistência `:3141`), além de caminhos de resposta/reabertura não censados integralmente [nao verificado]. `fechada_em` é lido em `pergunta_no_app.py:67`, escrito por fechamento em `models.py:1929` e salvo em `:1933`; reaberturas o limpam em `:2146`/`:2158` e `:2170`/`:2174`.

## 09/11 12:21 · sessao 764ff117cfac · - · - · origem=Ronald (tmux)

- **PROCEDENTE — dependência tenta escrever:** o DIFF chama `carga_da_tarefa` (`app/chamados/management/commands/diff_e3_sombra.py:108`), que chama `dias_trancados` (`app/chamados/services/tarefa_do_dia.py:370`). Com tranca existente, este chega ao leitor de toque (`app/chamados/juizes.py:1025`, `:1033`), que chama `get_user_sistema` (`:1146`); esse helper executa `get_or_create` (`app/core/utils.py:18`). Se faltar o usuário, tenta criar; a guarda SQL impede a persistência (`app/chamados/management/commands/diff_e3_sombra.py:34`, `:92`). **Cura:** consultar o usuário com `.get`, tratando ausência sem criar, e testar esse ramo; a fixture atual sempre fornece o usuário (`app/chamados/tests/e3_fixtures.py:26`).

- **PROCEDENTE, condicionado ao caminho de erro — escrita indireta/raspagem:** C6 pode chamar `registrar_engolido` (`app/chamados/juizes.py:1366`, `:1377`), que envia exceção e traceback ao logger (`app/core/observ.py:79`). A configuração SaaS conecta a raiz a arquivo e console (`app/config/settings/saas.py:183`), com `RotatingFileHandler` (`:138`). Isso ocorre antes da sanitização final do comando (`app/chamados/management/commands/diff_e3_sombra.py:97`). **Cura:** instrumento com tratamento sanitizado desse erro e teste de logger; ocorrência/configuração efetiva em produção **[nao verificado]**.

- **NÃO PROCEDE — agendamento:** a entrada apenas pertence a `FORA_DE_PIPELINE` (`app/config/crons.py:769`). O render percorre `CRONS` (`:1865`); o inventário executável reúne `CRONS + ENCADEADOS` (`:1798`). O diagrama apresentado baixa tela para 109 sítios (`app/docs/ARQUITETURA.mmd:356`); o mapa registra 67 sem chamar, cinco que chamam e derivam, 37 desconhecidos (`app/docs/MAPA.md:91`).

- **NÃO PROCEDE — novo juiz no registro:** o hunk fornecido só remove o pendente do dashboard **[nao verificado quanto ao histórico Git indisponível]**. O sítio atual lê `consulta_do_app` (`app/core/views.py:252`). O selo de tela verifica desaparecimento de impressões, sem certificar sozinho a cura (`app/core/tests/test_contract_juiz_tela.py:34`). O mérito funcional vem da matriz que exige duas perguntas na mesma disputa → duas pendências e verifica dashboard e ambos kernels (`app/chamados/tests/test_e3_app_orm.py:28`, `:39`, `:47`).

- **NÃO PROCEDE — quatro casos sem mordida:** há expectativas distintas para só fila, A/B, verbo do universo e isento (`app/chamados/tests/test_e3_portas_tarefa.py:32`, `:40`, `:49`, `:57`), com mutações correspondentes (`app/chamados/tests/e3_mutantes.py:49`). O mutante que inventa `cobrar` também encontra expectativa explícita de `validar` (`app/chamados/tests/test_e3_portas_tarefa.py:75`). O RED adicional envenena C6, exige uma alimentação e nenhuma query na releitura (`:113`).

- **FILA — alcance da evidência dos testes:** alguns testes novos preservam comportamento já existente, portanto passariam sem a cura, como o contrato C17 (`app/chamados/tests/test_e3_pendentes_app.py:126`). Isso não invalida os casos acima. O runner transforma qualquer falha em código 1 (`app/chamados/tests/e3_mutantes.py:127`); a causa individual das 30 mortes, sem suas saídas, permanece **[nao verificado]**. A guarda de arquivos examina somente o corpo do comando (`app/chamados/tests/test_e3_diff_sombra.py:77`, `:94`), deixando fora as dependências concretas citadas.

## 09/11 12:21 · sessao d8ef53d0b9a7 · SONDA-SEM-GRANT · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 12:18 · sessao 1659486be013 · - · - · origem=Ronald (tmux)

Censo operacional do carimbo:

| Estado publicado | Escritor e efeito | [nao verificado]
|---|---|
| `REFAZENDO` | `invalidar_carimbo`, por `carimbar`; apaga DIA e INCOMPLETA. Chamado antes do drop e antes do incremental. (bin/sombra.sh:112,224,288) |
| `REFEITA` / `DIVERGE` | Final do refazer, depois de máscaras, provas e provisionamento do leitor. Publica DIA, BASE_DIA e INCOMPLETA. (bin/sombra.sh:300,306,309,326,332,340,341,344) |
| Status anterior, ou `REFEITA` se vazio | Final do incremental; insumos guardados antes da invalidação; publicação final tem rc conferido. (bin/sombra.sh:217,218,224,236,239,240) |
| `BLOCO_RODANDO` | Entrada do bloco antes da lavra; falha de publicação impede o bloco. (bin/sombra.sh:375,384) |
| `OK` / `FALHOU` | Final do bloco; depende de DIVERGE e erros dos comandos; publicação final tem rc conferido. (bin/sombra.sh:435,436,446,448,449,452) |

Escritor físico operacional localizado: somente `carimbar`, que copia o anterior e promove por `mv`; fixtures de selos não escrevem o `.sombra_stamp` operacional. (bin/sombra.sh:94,97,100,101,103; bin/tests/test_sonda_sem_grant.sh:439,481,528)

| Leitor | Autoridade efetivamente usada |
|---|---|
| `conferir` | Biblioteca para parser/veredito; exige também migrations positivas do banco. (bin/sombra.sh:459,461,463,464,475,477,480) |
| Placar | Biblioteca para incompletude; processo separado `--conferir` para verde. (bin/placar_code.sh:33,34,35,165) |
| Incremental/bloco | `carimbo_campo`; têm guardas próprias sobre DIA/BASE_DIA/TIPO/DIVERGE, relacionadas à execução. (bin/sombra.sh:212,213,217,218,369,446,448) |
| Cobertura | **Parser/predicado próprios**: última chave vence, normaliza espaços/aspas, aceita `OK` ou `REFEITA`; completa cobre tudo, incremental exige base/janela. (app/core/sombra_cobertura.py:15,18,21,34,35,37,39,42,45,47) |
| `sombra.sh --cobre` | Importa cobertura do caminho absoluto da árvore viva, sem passar `hoje`. (bin/sombra.sh:244,247,249,255) |
| Deploy | Delega ao rc de `--conferir`; existem exceções explícitas por motivo e modo agendado. (bin/deploy.sh:151,152,159,162) |
| Esteira de raia | `trava.sh` delega ao rc de `--conferir`; moldes chamam essa trava. (bin/trava.sh:40,43,51,52; bin/molde_fatia/cadeia.sh:11; bin/molde_autocura/cadeia.sh:10) |
| Esteira o135 | Delega ao rc. (fatias_agendadas/o135-teto20/esteira.sh:80,81) |
| **Esteira abono-no-ar** | **Predicado/relógio próprios**: captura saída, ignora rc, procura somente `dia=<data atual>` e prossegue. Pode aceitar hoje/FALHOU/BLOCO_RODANDO/órfão para sua guarda local; deploy subsequente ainda possui outra guarda. (fatias_agendadas/abono-no-ar/esteira.sh:30,31,36,37; bin/sombra.sh:464,477; bin/deploy.sh:162) |
| Simulação/diff | Delegam a `--conferir` e `--cobre-competencia`. (bin/simular_folha.sh:68,70; bin/diff_janela_he_total.sh:19,22) |
| Hooks | Hook Stop consulta **trava da sombra**, não o carimbo; instalador pre-push produz wrapper. Régua executa selos, inclusive fixtures do carimbo. (bin/hook_stop_fila1.py:941,946,947; bin/hooks.sh:51,56; bin/regua.sh:138; bin/tests/test_sonda_sem_grant.sh:404,439) |

Não há referência temporal compartilhada: sombra captura HOJE antes da espera pela trava; BASE_DIA e incremental releem relógio; placar captura antes e depois chama outro processo; cobertura faz `date.today()` no host. A biblioteca não lê relógio. (bin/sombra.sh:83,213,341,485,487,498; bin/sombra_trava.sh:18,27,28; bin/placar_code.sh:34,165; app/core/sombra_cobertura.py:34; bin/sombra_carimbo.sh:32,36,44)

O container da sombra recebe `TZ=America/Sao_Paulo`; Django configura o mesmo fuso, mas isso não fixa o fuso do Python/shell do host. Igualdade efetiva de fusos e ocorrência real da virada: **[nao verificado]**. (bin/sombra.sh:199; app/config/settings/base.py:93; bin/sombra.sh:244,255; app/core/sombra_cobertura.py:34)

Residuais que deixam verde antigo ou falso resultado: [nao verificado]

1. **PROCEDENTE — entrada direta do incremental contorna a invalidação.** O script recebe INI/FIM diretamente, apaga/repõe a janela e só depois prova; não exige trava herdada nem passa pela publicação/invalidação do wrapper. Falha na prova deixa o stamp antigo; máscaras pertencem ao wrapper. É escrita da sombra fora da porta de invalidação, não segundo escritor físico do stamp. (bin/sombra_incremental.sh:19,20,22,30,31,66,68,70,85; bin/sombra.sh:224,228,230)

2. **PROCEDENTE — `--com-a-sombra`, `--rodar`, `--lavra`.** Executam lavra/escrita sem invalidar stamp. Lavra faz duas gravações ORM independentes; falha na segunda pode deixar primeira persistida com verde anterior. Os comandos arbitrários posteriores também podem alterar a sombra. `--lavra` sequer toma a trava nesse ramo. (bin/sombra.sh:170,171,174,180,181,503,507,508,509,534,536,537,539)

3. **PROCEDENTE — `--rodar` perde rc final.** Pipeline de execução é seguida por `echo` sem checagem; saída final pode ser rc 0 após falha. (bin/sombra.sh:45,537,539,540)

4. **PROCEDENTE — falha por comando ao repor lavra não entra no resultado do bloco.** Falha só escreve `REPOR_LAVRA_FALHOU`; rc contabilizado vem do comando posterior. Se esses comandos saem 0 e DIVERGE=0, pode publicar OK embora a reposição necessária tenha falhado. (bin/sombra.sh:423,424,426,427,430,436,446,448,451)

5. **PROCEDENTE — simular_folha escreve sob verde anterior.** Guarda antecede `PRE` arbitrário e `--recalcular`, sem invalidação; erro retorna falha mas conserva stamp. (bin/simular_folha.sh:67,68,70,71,72,73,75)

**DESCARTE — vermelho enquanto bloco roda, por si só, seria defeito.** `BLOCO_RODANDO` impede aprovar um ensaio que está sendo alterado; conferir só aceita OK, cobertura só aceita OK/REFEITA. A repetição concluída publica OK/FALHOU novamente. Falha deixa vermelho deliberadamente. (bin/sombra.sh:375,449,451,452; bin/sombra_carimbo.sh:41; app/core/sombra_cobertura.py:37)

Ocorrência desses caminhos em produção, conteúdo dos scripts externos e danos efetivos: **[nao verificado]**.

## 09/11 12:17 · sessao 063d0d5805fa · - · - · origem=Ronald (tmux)

**P1, P2, P3 e P5 fechados no fluxo de código e com asserções pertinentes; P4 fecha parser/predicados dos dois leitores bash, mas não o relógio compartilhado nem o terceiro parser. Execução dos selos e mortes dos mutantes: [nao verificado].** (bin/sombra.sh:94,103,224,240,375,452; bin/sombra_carimbo.sh:24,36,44; bin/tests/test_sonda_sem_grant_pg.sh:46,52,60; bin/placar_code.sh:34,165)

| Item | Evidência e mordida |
|---|---|
| P1 | `carimbar` trabalha em temporário e só publica pelo `mv`; falha remove temporário e retorna 1. A fixture `mv` agora aponta `STAMP` para o próprio carimbo verde dentro do diretório sem escrita; prova a impossibilidade de escrita e compara esse mesmo `$st`. O mutante descrito “trunca o stamp ao falhar mv” atingiria a comparação de conteúdo correta. A comparação usa `$(cat)`, portanto não é integridade byte a byte quanto a novas linhas finais. (bin/sombra.sh:96,97,103; bin/tests/test_sonda_sem_grant.sh:442,443,446,449,461) |
| P2 | Publica `BLOCO_RODANDO` antes da lavra e exige sucesso da publicação final. 9d inicia hoje/OK, simula queda do container e falha final de gravação com comando verde ou erro; cobra rc 1 e ausência do OK antigo. Os controles cobram OK/FALHOU e conservação de DIA/INCOMPLETA. Retirar a invalidação atingiria a asserção de status; ignorar rc final atingiria a asserção de rc. (bin/sombra.sh:375,384,449,452; bin/tests/test_sonda_sem_grant.sh:481,490,496,498,500,505,506,507,508,509) |
| P3 | Guarda STATUS/INCOMPLETA antes de invalidar; invalida antes do script incremental e exige carimbo final. 9e começa completa/OK, simula prova pós-transação falha, máscara falha e gravação falha; cobra rc 1 e ausência de hoje com OK/REFEITA. Retirar invalidação atingiria a asserção de verde; ignorar rc final atingiria rc. (bin/sombra.sh:217,218,224,226,236,240; bin/tests/test_sonda_sem_grant.sh:528,537,539,549,550,551) |
| P4 | Placar e conferir chamam os predicados nomeados; o parser rejeita duplicatas. Selos cobram a ligação estrutural, sem parser/comparação própria, e comportamento dos predicados: DIVERGE tem medição mas não verde; OK sem número tem verde mas não medição. O selo só procura `date` na biblioteca, não prova um único instante temporal entre processos. (bin/placar_code.sh:33,34,35,165; bin/sombra.sh:480; bin/sombra_carimbo.sh:26,27,41,50; bin/tests/test_sonda_sem_grant.sh:567,572,598,605,625,643,645,650,651,652,653) |
| P5 | ID só vem de `run` bem-sucedido; sem ID não chama rm/inspect. Limpeza por ID distingue inexistência de falha de daemon, preservando rc anterior não zero. O selo cobra argv exato com ID, nenhum rm/inspect após run recusado, mensagem “não sei se” e oito casos. Substituir ID por nome atingiria argv/identidade; usar nome sem ID atingiria caso colisão; equiparar daemon a sumiço atingiria rc e mensagem. (bin/tests/test_sonda_sem_grant_pg.sh:46,48,52,53,54,60,61,62,112,116,119,121,123,126,128,130,132) |

**Porta e `.env_leitor`:** no destino solicitado, catálogo pronto e login TCP `leitor|on` precedem a criação e promoção do novo arquivo. Falha da conferência, role ou login conserva o arquivo anterior; os selos cobram conteúdo/sha e ausência de temporários. A senha é compartilhada entre clusters, mas só o destino escolhido é conferido: gerar senha nova e provisionar um só destino pode deixar o outro recusando login até seu provisionamento, comportamento explicitamente documentado. (bin/sonda_provisionar.sh:22,24,45,150,151,154,161,165,171,174; bin/tests/test_sonda_sem_grant.sh:285,291,295,299,338; bin/tests/test_sonda_sem_grant_pg.sh:263,265; bin/sonda_leitura.sh:194)

**Opus 2:** existem as revogações de PUBLIC nos schemas e no banco; impacto da primeira execução em produção é [nao verificado]. HOST antigo é realmente recusado e tem caso de selo. Isso não constitui segundo escritor nem derivação paralela: são operações da porta explícita e recusa da leitura. (bin/sonda_provisionar.sh:127,137; bin/sonda_leitura.sh:175; bin/tests/test_sonda_sem_grant.sh:147,149)

**Opus 3:** PROCEDENTE a lacuna de prova/garantia; sucesso concreto de `SET SESSION ... READ WRITE; SELECT lo_create(0)` é **[nao verificado]** no dado admissível. O Python arbitrário passa fora do classificador `_sql`; o catálogo conferido cobre ACL de relações/colunas/sequências/schema/banco/defaults, membership e donos, mas não EXECUTE de funções/large objects. O selo PG não executa a sonda: seu `docker run` é stub que imprime `SONDA_RODOU`, enquanto fixtures de escrita usam `saas_user`; logo não tenta escrita autenticado como leitor. Se `lo_create` pelo leitor for confirmado, seria escrita pelo envelope apresentado como incapaz de acidente de escrita, portanto violação do escopo literal, não simples fila. (bin/sonda_leitor_comum.sh:34,35,99,119,123,125; bin/sonda_leitura.sh:8,13,238,239; bin/tests/test_sonda_sem_grant_pg.sh:153,154,179,188,226)

**Opus 4:** recusar durante `BLOCO_RODANDO` é o comportamento protetor necessário para P2, não defeito por si: um segundo bloco concluído publica novo OK/FALHOU. Não prova segundo escritor. A compatibilidade específica com consumidores de cobertura depende do terceiro parser, atribuído ao outro agente. (bin/sombra.sh:375,449,451,452; bin/tests/test_sonda_sem_grant.sh:508,509)

**Opus 5:** stdout é acrescentado ao bruto durante execução e todo stderr é acrescentado depois, portanto perde-se a ordem relativa stdout/stderr. É defeito de fidelidade temporal do relato, sem evidência aqui de segunda autoridade/escritor ou carimbo verde. (bin/codex_pergunta.sh:319,326)

**Outro caminho de possível falso verde:** dentro do bloco, falha ao repor a lavra é reduzida a `echo REPOR_LAVRA_FALHOU`; o comando seguinte continua, e só seu rc vai ao resultado. Se todos os comandos seguintes retornarem 0 e DIVERGE for 0, pode publicar OK sem cobrar essa falha de reposição. O caminho está no código; ocorrência real é **[nao verificado]**. (bin/sombra.sh:423,424,426,427,430,436,447,448,451)

## 09/11 12:17 · sessao b88c0e3228b0 · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 12:06 · sessao fff2cee9247d · SELOS-CANON-I3 · verificador · origem=Code

**REPROVADO para pouso nesta volta.** Há caminhos comprovados de troca do denominador sem abertura efetiva, aprovação sem snapshot e falsa reprovação após a DIETA. (`bin/codex_pergunta.sh:234`, `bin/codex_pergunta.sh:240`, `bin/disjuntor.py:883`, `bin/disjuntor.py:917`, `bin/disjuntor.py:892`)

RED/GREEN, execução dos 20 mutantes, frequência em produção e inventário dos briefs vivos: **[nao verificado]**. O relato do construtor não substitui essas evidências.

1. **Aval I-3 e quatro ETAPAS: realização parcial.**

   | Etapa | Onde realiza | O que falta |
   |---|---|---|
   | **1 — Forma na abertura** | `etapas_do_brief` exige bloco, conteúdo, resultado e sequência 1..N; `_censo_nega` aplica a cada brief coletado. (`bin/disjuntor.py:1703`, `bin/disjuntor.py:1717`, `bin/disjuntor.py:1725`, `bin/disjuntor.py:1869`) | A abertura está implementada mecanicamente; resultado executado do selo **[nao verificado]**. As asserções de recusa existem. (`bin/tests/test_etapas_no_brief.sh:99`, `bin/tests/test_etapas_no_brief.sh:107`, `bin/tests/test_etapas_no_brief.sh:116`) |
   | **2 — Fotografia estável** | Grava obra, N máximo, via, caminho e hash abreviado; o B3 lê a fotografia, sem reler o brief. K fica implicitamente determinado pela sequência 1..N. (`bin/disjuntor.py:1727`, `bin/disjuntor.py:1878`, `bin/disjuntor.py:1882`, `bin/disjuntor.py:874`) | Fotografia pode ser substituída por tentativa que depois perde a vaga; falha de gravação também não impede aprovação. (`bin/codex_pergunta.sh:234`, `bin/codex_pergunta.sh:240`, `bin/disjuntor.py:210`, `bin/disjuntor.py:1886`) |
   | **3 — Conferência n/N** | Cruza fotografia, handback e RELATO; insuficiência entra na mesma fila de reprovações B3 e produz “pela metade, faltam”. (`bin/disjuntor.py:916`, `bin/disjuntor.py:930`, `bin/disjuntor.py:1089`, `bin/disjuntor.py:1115`) | SEM-SNAPSHOT passa; DIETA pode apagar a evidência do universo consultado; verificador deriva N do brief atual; merge/deploy Bash não espera essa conferência. (`bin/disjuntor.py:917`, `bin/disjuntor.py:892`, `.claude/agents/verificador.md:18`, `bin/disjuntor.py:1977`) |
   | **4 — ESTADO 24h** | Medição entra em `ler`, `linha` e `gerar_estado`. (`bin/disjuntor.py:2204`, `bin/disjuntor.py:2315`, `bin/disjuntor.py:2447`, `bin/gerar_estado.py:281`) | Conta finais APROVADOS conferidos, não pousos efetivamente realizados; exclui SEM-SNAPSHOT do denominador. O próprio rótulo declara essa substituição. (`bin/disjuntor.py:2333`, `bin/disjuntor.py:2336`, `bin/disjuntor.py:2353`) |

   A decisão temporal do brief exige que as linhas medidas estejam publicadas antes do final e sejam carregadas pelo commit do pouso; o código apenas lê o RELATO corrente, sem conferir sua inclusão nesse commit. (`bin/disjuntor.py:892`, `bin/disjuntor.py:922`)

2. **JUIZ ÚNICO: forma centralizada; n/N com derivação paralela.**

   A forma tem um juiz, `etapas_do_brief`, chamado pela porta existente. CENSO e ETAPAS compartilham `_bloco_do_titulo`; os validadores dos conteúdos permanecem distintos, como correspondem a perguntas diferentes. (`bin/disjuntor.py:1626`, `bin/disjuntor.py:1653`, `bin/disjuntor.py:1710`, `bin/disjuntor.py:1869`)

   **A pergunta n/N não está em um único sítio:** o verificador recebe instrução para contar N no brief e n nas citações do RELATO; o B3 obtém N da fotografia e n de um conjunto de K filtrados. Não há instrução para o verificador consumir o resultado dessa autoridade. (`.claude/agents/verificador.md:18`, `bin/disjuntor.py:874`, `bin/disjuntor.py:894`, `bin/disjuntor.py:928`)

   Consequência concreta: brief aberto com três etapas e depois editado para cinco pode levar o verificador a devolver 3/5; contra snapshot N=3, o B3 registra **faltam 3**, embora o RELATO contenha as três etapas originais. (`.claude/agents/verificador.md:18`, `bin/disjuntor.py:930`)

   Os demais sítios citados delegam: Stop chama B3; wrapper chama `--censo`; ESTADO chama `ler/linha`; handoff chama o reconhecedor de PAREI. Não apresentam nesses caminhos outro parser de ETAPAS. (`bin/hook_stop_fila1.py:1204`, `bin/codex_pergunta.sh:234`, `bin/gerar_estado.py:280`, `bin/handoff_sessao.sh:130`)

3. **FOTOGRAFIA: escritor, chave, leitor e divergência.**

   O destino é `logs/sessao/disjuntor.log`. `_censo_nega` escreve diretamente no CLI; na via Agent acumula eventos, publicados por `pretool` depois que `_pretool` passa. A chave é a **obra literal**. (`bin/disjuntor.py:131`, `bin/disjuntor.py:1878`, `bin/disjuntor.py:1882`, `bin/disjuntor.py:1885`, `bin/disjuntor.py:1967`)

   `_snapshot_etapas` escolhe a **última** fotografia cuja obra seja exatamente igual e retorna somente N. Não vincula a fotografia ao agente final, pid, hash da abertura ou instante do final; não valida o hash do brief. (`bin/disjuntor.py:869`, `bin/disjuntor.py:879`, `bin/disjuntor.py:882`)

   **A identidade pode divergir.** O wrapper passa `$OBRA`; o B3 extrai a primeira identidade reconhecida no primeiro prompt do verificador. A abertura aceita a forma simples do censo, enquanto B3 também reconhece caudas, como `O9 (C)`. Uma abertura `O9` e um final identificado como `O9 (C)` não encontram a mesma fotografia. (`bin/codex_pergunta.sh:188`, `bin/codex_pergunta.sh:234`, `bin/disjuntor.py:172`, `bin/disjuntor.py:622`, `bin/disjuntor.py:634`, `bin/disjuntor.py:666`)

   Há ainda troca de denominador sem abertura: segunda tentativa da mesma obra grava N novo no `--censo`, depois falha no `flock`; o final da primeira construção lê esse N novo. (`bin/codex_pergunta.sh:234`, `bin/codex_pergunta.sh:240`, `bin/disjuntor.py:883`)

   Frequência real de divergência e proporção de finais que escapam por isso: **[nao verificado]**. Divergência para identidade legível sem fotografia cai em SEM-SNAPSHOT; identidade ilegível entra antes no contador de final sem obra. (`bin/disjuntor.py:666`, `bin/disjuntor.py:859`, `bin/disjuntor.py:917`)

4. **B3 n/N: “MEDIDA” é reconhecimento textual.**

   O formato efetivo, após retirar espaços e caracteres iniciais de Markdown, é:

   ```text
   ^ETAPA (\S+) (\d+):(.*)->\s*(.*?)\s*$
   ```

   Exige obra exatamente igual, K único dentro de 1..N e resultado com algum caractere ASCII alfanumérico, sem começar por `RESULTADO OBSERVAVEL`. O texto anterior à seta não é validado nem comparado à etapa fotografada. (`bin/disjuntor.py:871`, `bin/disjuntor.py:896`, `bin/disjuntor.py:897`, `bin/disjuntor.py:900`, `bin/disjuntor.py:902`)

   Confirmei em memória que `pendente`, `nao medido` e `**RESULTADO OBSERVAVEL rc 0**` contam como três medidas. Portanto o predicado prova presença textual, não execução ou resultado observado. (`bin/disjuntor.py:900`, `bin/disjuntor.py:1700`)

   Escritor concreto dessas linhas em produção: **[nao verificado]**. No selo, quem escreve é `_relato`; a instrução do verificador apenas manda contá-las. (`bin/tests/test_etapas_no_brief.sh:42`, `bin/tests/test_etapas_no_brief.sh:162`, `.claude/agents/verificador.md:18`)

   **DIETA pode reprovar obra legítima:** a regra move conteúdo com mais de três dias para RELATO-ARQUIVO; o leitor novo abre exclusivamente RELATO.md. Se a movimentação ocorrer antes da primeira conferência do final, `n_rel=0`, `faltam=N`, entra METADE e dispara B3. Se o final já estiver no memo, não há releitura retroativa. (`CLAUDE.md:439`, `app/docs/LEIS.md:135`, `bin/disjuntor.py:892`, `bin/disjuntor.py:913`, `bin/disjuntor.py:930`, `bin/disjuntor.py:936`, `bin/disjuntor.py:1108`)

   A regra dos três dias está em **L-109**; L-116 estabelece o teto das seções. (`app/docs/LEIS.md:135`, `app/docs/LEIS.md:148`)

   Outro leitor canônico que já una RELATO e RELATO-ARQUIVO: **[nao verificado]**. As referências encontradas ao arquivo arquivado em `esteira_vigia` e `gerar_cortes` são prosa, não implementação dessa leitura conjunta. (`app/core/esteira_vigia.py:823`, `bin/gerar_cortes.py:131`)

5. **SEM-SNAPSHOT: escape comprovado, universo real desconhecido.**

   É fail-open: grava memo, emite aviso e continua sem conferir RELATO, sem reprovação e sem NAO SEI. O memo impede nova conferência desse mesmo final mesmo que uma fotografia apareça depois. O selo exige que esse caso passe. (`bin/disjuntor.py:913`, `bin/disjuntor.py:917`, `bin/disjuntor.py:921`, `bin/tests/test_etapas_no_brief.sh:211`)

   Para obra sujeita ao I-3, isso deixa sobreviver a falta da prova exigida e contraria a cura na origem e o escopo literal do aval. Para obra comprovadamente fora da fila 1, a própria abertura já retorna fora do julgamento. (`CLAUDE.md:17`, `CLAUDE.md:37`, `bin/disjuntor.py:1853`, `bin/disjuntor.py:917`)

   **Nuance do contrato:** o cabeçalho histórico declara “FALHA ABERTO”; portanto não cabe afirmar que todo disjuntor sempre exigiu fechar na falha. Já o caminho B3 atual diz explicitamente que NAO SEI nega. SEM-SNAPSHOT evita essa classificação. (`bin/disjuntor.py:34`, `bin/disjuntor.py:1937`, `bin/disjuntor.py:1941`, `bin/disjuntor.py:917`)

   Universo mecânico: transcripts de subagentes com meta `verificador`, prompt final, obra legível e veredito APROVADO, ainda não conferidos; qualquer um sem fotografia exata entra no escape. Isso abrange potencialmente obras externas à fila, anteriores ao instrumento, com identidade divergente ou fotografia perdida. (`bin/disjuntor.py:847`, `bin/disjuntor.py:854`, `bin/disjuntor.py:863`, `bin/disjuntor.py:913`, `bin/disjuntor.py:917`)

   Quais finais APROVADOS estão vivos hoje e quantos pertencem a cada classe: **[nao verificado]**.

   **O contador não declara esperado 0.** SEM-SNAPSHOT é contado separadamente e excluído de `finais`; assim o rótulo de esperado 100% cobre somente os conferidos. (`bin/disjuntor.py:2333`, `bin/disjuntor.py:2336`, `bin/disjuntor.py:2353`)

6. **ESMERIL-SEMANTICA: dois motivos sob “censo”.**

   Sim: falta de CENSO e falta de ETAPAS produzem a mesma cabeça literal “sem CENSO” e o mesmo `PAREI: censo-<obra> | espera Codex`. O detalhe da negativa distingue os defeitos; a chave e o PAREI não. (`bin/disjuntor.py:1612`, `bin/disjuntor.py:1616`, `bin/disjuntor.py:1887`)

   Stop reconhece essa linha como espera por wrapper vivo com papel censo/leitura da mesma obra; não decide se o defeito original era CENSO ou ETAPAS. Handoff e alarme reutilizam essa prova. (`bin/hook_stop_fila1.py:736`, `bin/hook_stop_fila1.py:763`, `bin/hook_stop_fila1.py:773`, `bin/handoff_sessao.sh:130`, `bin/alarme_sessao_ociosa.py:137`)

   **O placar de censo não lê a linha PAREI:** lê eventos NEGOU/CENSO da trilha. Mas contabiliza a recusa de ETAPAS como negativa de censo e publica “aberturas negadas por censo” e espera por censo. Portanto existe colapso semântico, embora não sejam dois leitores da mesma linha física. (`bin/disjuntor.py:2208`, `bin/disjuntor.py:2261`, `bin/disjuntor.py:2308`, `bin/disjuntor.py:2371`)

7. **Selos: há mordida concreta, mas não cobertura suficiente.**

   | Grupo | Asserções existentes |
   |---|---|
   | **4 mutantes de forma** | RC das formas inválidas, sequência, resultado vazio e última etapa malformada. (`bin/tests/test_etapas_no_brief.sh:100`, `bin/tests/test_etapas_no_brief.sh:101`, `bin/tests/test_etapas_no_brief.sh:104`, `bin/tests/test_etapas_no_brief.sh:116`, `bin/tests/test_etapas_no_brief.sh:312`) |
   | **4 de fotografia** | Presença/ordem nas duas vias; brief editado mantém N; reabertura troca N. (`bin/tests/test_etapas_no_brief.sh:93`, `bin/tests/test_etapas_no_brief.sh:139`, `bin/tests/test_etapas_no_brief.sh:232`, `bin/tests/test_etapas_no_brief.sh:237`, `bin/tests/test_etapas_no_brief.sh:317`) |
   | **8 de B3** | Negação efetiva, cruzamento das fontes, cópia rejeitada, memo, ilegibilidade, SEM-SNAPSHOT e mensagem do disparo. (`bin/tests/test_etapas_no_brief.sh:167`, `bin/tests/test_etapas_no_brief.sh:180`, `bin/tests/test_etapas_no_brief.sh:188`, `bin/tests/test_etapas_no_brief.sh:203`, `bin/tests/test_etapas_no_brief.sh:214`, `bin/tests/test_etapas_no_brief.sh:220`, `bin/tests/test_etapas_no_brief.sh:325`) |
   | **4 de medida** | Dict exato, janela, texto no ESTADO e ilegibilidade → None. (`bin/tests/test_etapas_no_brief.sh:257`, `bin/tests/test_etapas_no_brief.sh:260`, `bin/tests/test_etapas_no_brief.sh:265`, `bin/tests/test_etapas_no_brief.sh:272`, `bin/tests/test_etapas_no_brief.sh:339`) |

   O harness aceita **qualquer rc diferente de zero** como morte; não exige a falha específica pretendida. Há controle sem mutação, mas a mensagem FALHA não é condição para aceitar a morte. (`bin/tests/test_etapas_no_brief.sh:289`, `bin/tests/test_etapas_no_brief.sh:304`, `bin/tests/test_etapas_no_brief.sh:307`)

   Nos exemplos pedidos:

   - **Bloco vazio e etapa sem RESULTADO:** têm asserções de recusa. (`bin/tests/test_etapas_no_brief.sh:100`, `bin/tests/test_etapas_no_brief.sh:107`, `bin/tests/test_etapas_no_brief.sh:116`)
   - **Contagem por prefixo:** o caso `OBRA-E3X` deve matar esse mutante; hoje a comparação é exata. (`bin/tests/test_etapas_no_brief.sh:201`, `bin/tests/test_etapas_no_brief.sh:203`, `bin/disjuntor.py:897`)
   - **n>N:** já passa sem mutação. Confirmei em memória N=3, RELATO com três etapas, `ETAPAS: 4/3` → PROVADAS. Não há guarda n≤N. (`bin/disjuntor.py:929`, `bin/disjuntor.py:930`, `bin/disjuntor.py:939`)
   - **Mutante simples sobrevivente por inspeção:** trocar `_ALNUM_RE.search(med)` por `True`, preservando a rejeição do prefixo. Passa a aceitar resultado vazio; os casos candidatos do RELATO não exercitam esse vazio. Sobrevivência executada: **[nao verificado]**. (`bin/disjuntor.py:900`, `bin/tests/test_etapas_no_brief.sh:162`, `bin/tests/test_etapas_no_brief.sh:200`)
   - **Snapshot reescrito por tentativa recusada:** não está coberto; o teste chama diretamente `_censo`, sem disputar a vaga do wrapper. (`bin/tests/test_etapas_no_brief.sh:64`, `bin/tests/test_etapas_no_brief.sh:234`, `bin/codex_pergunta.sh:240`)

   “20/20 morreram” e RED na base: **[nao verificado]**.

8. **QUEM ESCREVE / QUEM LÊ.**

   | Campo, arquivo ou linha | Escritor | Leitor |
   |---|---|---|
   | Briefs `logs/<obra>/brief*.md` e citados | Autor real **[nao verificado]** | Coletor comum e dois juízes. (`bin/disjuntor.py:1759`, `bin/disjuntor.py:1866`, `bin/disjuntor.py:1869`) |
   | BACKLOG / pertencimento à fila 1 | Autor real **[nao verificado]** | `_fila1_viva` usa o leitor do hook; `_pertence_fila1` decide pertencimento. (`bin/disjuntor.py:1775`, `bin/disjuntor.py:1786`, `bin/disjuntor.py:1818`) |
   | `disjuntor.log`: carimbo, evento e detalhe | `trilha`, append, detalhe limitado a 300 caracteres. (`bin/disjuntor.py:203`, `bin/disjuntor.py:208`) | Snapshot e medidores. (`bin/disjuntor.py:879`, `bin/disjuntor.py:2249`, `bin/disjuntor.py:2322`) |
   | `ETAPAS SNAPSHOT`: obra, N, via, briefs e sha12 | `_censo_nega`; publicação Agent por `pretool`. (`bin/disjuntor.py:1878`, `bin/disjuntor.py:1882`, `bin/disjuntor.py:1967`) | `_snapshot_etapas` usa somente obra/N. (`bin/disjuntor.py:869`, `bin/disjuntor.py:882`) |
   | Transcript/meta e `SubagentHandback` | Harness indicado pelo código; implementação do escritor **[nao verificado]**. (`bin/disjuntor.py:443`) | `_meta`, `_veredito_e_obra`, `_reprovacoes_finais`. (`bin/disjuntor.py:461`, `bin/disjuntor.py:654`, `bin/disjuntor.py:847`) |
   | `ETAPAS: n/N` | Verificador instruído pela configuração; execução real **[nao verificado]**. (`.claude/agents/verificador.md:18`) | Regex e conferência B3. (`bin/disjuntor.py:870`, `bin/disjuntor.py:928`) |
   | `ETAPA <obra> K: ... -> ...` | Sessão real **[nao verificado]**; fixture tem escritor explícito. (`bin/tests/test_etapas_no_brief.sh:42`) | `_etapas_medidas`, somente RELATO vivo. (`bin/disjuntor.py:892`) |
   | `est['etapas'][aid]` | `_confere_etapas`, sob `_com_estado`. (`bin/disjuntor.py:918`, `bin/disjuntor.py:931`, `bin/disjuntor.py:1158`) | Memo em `_confere_etapas`. (`bin/disjuntor.py:913`) |
   | `b3[obra].reprovados` e `.metade[aid]` | Integração em `decide_b3`. (`bin/disjuntor.py:1090`, `bin/disjuntor.py:1097`) | Limiar/disparo e mensagem. (`bin/disjuntor.py:1106`, `bin/disjuntor.py:1113`) |
   | `ETAPAS PROVADAS/METADE/SEM-SNAPSHOT` e FALHA da conferência | Preparados pela conferência; publicados após salvar estado. (`bin/disjuntor.py:919`, `bin/disjuntor.py:925`, `bin/disjuntor.py:937`, `bin/disjuntor.py:939`, `bin/disjuntor.py:1147`) | `_etapas_24h` reconhece os três eventos ETAPAS. (`bin/disjuntor.py:2312`, `bin/disjuntor.py:2324`) |
   | `NEGOU`, `CENSO APROVADO`, `PAREI censo` | CLI/porta; PAREI passa por `no_topo`, com trava do escritor existente. (`bin/disjuntor.py:2489`, `bin/disjuntor.py:1883`, `bin/disjuntor.py:334`, `app/core/esteira_vigia.py:846`) | Medidor de censo; Stop/handoff/alarme. (`bin/disjuntor.py:2261`, `bin/hook_stop_fila1.py:773`, `bin/handoff_sessao.sh:130`, `bin/alarme_sessao_ociosa.py:137`) |
   | `ler()['etapas']`, texto 24h e ESTADO | Medidor → composição → `gerar_estado` grava destino. (`bin/disjuntor.py:2204`, `bin/disjuntor.py:2348`, `bin/disjuntor.py:2447`, `bin/gerar_estado.py:390`) | `gerar_estado` chama `ler/linha`; `relato.sh` gera e encaminha ESTADO à publicação. (`bin/gerar_estado.py:280`, `bin/relato.sh:38`, `bin/relato.sh:105`) |

   Memo, reprovações e anúncios reutilizam a trava e a gravação atômica existentes; o anúncio ocorre ainda sob essa trava. A fotografia da abertura não participa dessa transição. (`bin/disjuntor.py:361`, `bin/disjuntor.py:376`, `bin/disjuntor.py:382`, `bin/disjuntor.py:393`, `bin/disjuntor.py:1882`)

9. **Classes já reprovadoras presentes.**

   - **Derivação paralela de fonte única:** verificador calcula N do brief corrente, B3 lê fotografia. (`.claude/agents/verificador.md:18`, `bin/disjuntor.py:916`)
   - **Ordem de trava:** fotografia publicada antes de adquirir a vaga; tentativa recusada pode substituir denominador vigente. (`bin/codex_pergunta.sh:234`, `bin/codex_pergunta.sh:240`, `bin/disjuntor.py:883`)
   - **Pouso antes do pré-requisito:** Bash principal sem chamada Codex sai antes de consultar B3; conferência fica para Stop ou próxima chamada de verificador. É lacuna herdada, agora alcançando a cobrança de etapas. (`bin/disjuntor.py:1977`, `bin/disjuntor.py:1989`, `bin/disjuntor.py:2005`, `bin/hook_stop_fila1.py:1204`)
   - **Campo com dois significados:** recusa ETAPAS registrada/medida como CENSO. (`bin/disjuntor.py:1887`, `bin/disjuntor.py:2261`, `bin/disjuntor.py:2371`)
   - **Persistência que falha aberta:** falha ao gravar fotografia é engolida; abertura passa e final pode escapar por SEM-SNAPSHOT. (`bin/disjuntor.py:210`, `bin/disjuntor.py:1886`, `bin/disjuntor.py:917`)
   - **Segundo escritor de máquina introduzido fora da porta:** não demonstrado; negativas reutilizam `no_relato/no_topo`. (`bin/disjuntor.py:304`, `bin/disjuntor.py:334`, `app/core/esteira_vigia.py:847`)
   - **Segundo juiz de negócio registrado duplicado:** **[nao verificado]**. O segundo julgamento demonstrado é o de n/N no verificador. (`.claude/agents/verificador.md:18`)
   - **Relógio novo fora da borda:** `_etapas_24h` recebe `agora`; `trilha` continua lendo seu relógio interno preexistente. Não há nova leitura independente nessa função de medida. (`bin/disjuntor.py:2315`, `bin/disjuntor.py:2319`, `bin/disjuntor.py:208`)
   - **Selo integralmente sem mordida:** não; há asserções comportamentais. RED legítimo e morte executada de cada mutante continuam **[nao verificado]**. (`bin/tests/test_etapas_no_brief.sh:167`, `bin/tests/test_etapas_no_brief.sh:257`)

10. **Impacto na próxima abertura: lista alegada, não censo certificado.**

   A entrega aponta sete briefs sem ETAPAS:

   - `akmed/brief_v5` — **[nao verificado]**.
   - `codexneg/brief_v4` — **[nao verificado]**.
   - Diretório `esminst`, brief não individualizado — **[nao verificado]**.
   - Diretório `painel_um`, brief não individualizado — **[nao verificado]**.
   - Diretório `plantio_mais1`, brief não individualizado — **[nao verificado]**.
   - `BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS/brief.md` — **[nao verificado]**.
   - `FIO-E0-V4-E4-ESCRITORES/brief.md` — **[nao verificado]**.

   Também são alegações da entrega: 

[truncado em 20000 de 20633 caracteres]

## 09/11 12:02 · sessao 03267d961e14 · - · - · origem=Ronald (tmux)

3. **Fotografia e identidade.** `_censo_nega` monta `SNAPSHOT <obra> :: N=<máximo dos briefs> ... <sha12>:N=...`, grava pela porta `trilha` no CLI e acumula eventos no Agent; `pretool` grava o acumulado depois que `_pretool` passa (`bin/disjuntor.py:1878`, `bin/disjuntor.py:1882`, `bin/disjuntor.py:1885`, `bin/disjuntor.py:1967`). Caminho = `logs/sessao/disjuntor.log`; chave = obra literal; leitor `_snapshot_etapas` aceita a última linha dessa obra e retorna **somente N**, ignorando hashes/pid/briefs (`bin/disjuntor.py:131`, `bin/disjuntor.py:869`, `bin/disjuntor.py:874`, `bin/disjuntor.py:882`).

**Defeito de ordem comprovado:** o wrapper grava a fotografia no `--censo` **antes** de obter a vaga e validar o HEAD; nova tentativa com mesmo ID pode mudar a última fotografia e depois ser recusada pelo `flock`, sem nova abertura bem-sucedida (`bin/codex_pergunta.sh:234`, `bin/codex_pergunta.sh:239`, `bin/codex_pergunta.sh:240`, `bin/codex_pergunta.sh:241`; leitura última foto `bin/disjuntor.py:882`). O selo testa reabertura bem-sucedida; não cobre tentativa que morre depois do censo (`bin/tests/test_etapas_no_brief.sh:234`, `bin/tests/test_etapas_no_brief.sh:237`).

Na via wrapper, abertura recebe `$CODEX_OBRA`; no final, B3 extrai a primeira obra do primeiro prompt do transcript. Não há vínculo entre esses dois IDs (`bin/codex_pergunta.sh:81`, `bin/codex_pergunta.sh:234`, `bin/disjuntor.py:622`, `bin/disjuntor.py:666`). Formas também diferem: censo aceita `[A-Za-z0-9-]+`; B3 admite identidades com cauda, como `O9 (C)` e `O9 NOTURNO-X` (`bin/disjuntor.py:172`, `bin/disjuntor.py:174`, `bin/disjuntor.py:634`). IDs distintos produzem ausência de foto correspondente e `SEM-SNAPSHOT`; ID não reconhecido pelo B3 entra antes em final sem obra (`bin/disjuntor.py:666`, `bin/disjuntor.py:882`, `bin/disjuntor.py:917`). Frequência real desses casos = `[nao verificado]`.

4. **MEDIDA / DIETA.** Formato efetivo: linha após `strip().lstrip('*_`> -')`, regex `^ETAPA (\S+) (\d+):(.*)->\s*(.*?)\s*$`; obra por igualdade exata, K único em `1..N`, trecho medido com ASCII alfanumérico e que não comece, após `upper()`, por `RESULTADO OBSERVAVEL` (`bin/disjuntor.py:871`, `bin/disjuntor.py:896`, `bin/disjuntor.py:897`, `bin/disjuntor.py:900`, `bin/disjuntor.py:902`). Isso reconhece a presença textual; não valida execução, valor esperado ou correspondência do resultado à etapa (`bin/disjuntor.py:900`, `bin/disjuntor.py:902`).

DIETA manda mover conteúdo com mais de três dias para `RELATO-ARQUIVO.md`; `_etapas_medidas` abre exclusivamente `RELATO.md` (`CLAUDE.md:439`, `app/docs/LEIS.md:135`, `bin/disjuntor.py:135`, `bin/disjuntor.py:892`). Se o movimento ocorre **antes da primeira conferência daquele final**, as linhas desaparecem do universo lido, `n_rel=0`, `faltam=N`, e entra em METADE mesmo com handback N/N (`bin/disjuntor.py:902`, `bin/disjuntor.py:922`, `bin/disjuntor.py:930`, `bin/disjuntor.py:936`). Se o final já foi conferido, memo impede releitura; portanto DIETA não derruba retroativamente todo final (`bin/disjuntor.py:913`).

Outro leitor que já una RELATO e RELATO-ARQUIVO: `[nao verificado]`. Busca literal em `.py/.sh`, inclusive ocultos/ignorados, encontrou apenas referências em prosa e testes; prosa não prova autoridade leitora (`app/core/esteira_vigia.py:823`, `bin/gerar_cortes.py:131`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:193`).

5. **SEM-SNAPSHOT.** É fail-open demonstrável: ausência de foto grava memo e aviso, depois `continue`, sem conferir sequer o RELATO e sem adicionar reprovado; o final nunca é reavaliado se a fotografia surgir depois (`bin/disjuntor.py:917`, `bin/disjuntor.py:918`, `bin/disjuntor.py:921`, `bin/disjuntor.py:913`). O selo exige que esse caso passe (`bin/tests/test_etapas_no_brief.sh:207`, `bin/tests/test_etapas_no_brief.sh:210`).

O percentual exclui SEM-SNAPSHOT do denominador `finais`, enquanto seu contador separado não declara esperado 0 (`bin/disjuntor.py:2333`, `bin/disjuntor.py:2335`, `bin/disjuntor.py:2336`, `bin/disjuntor.py:2353`, `bin/disjuntor.py:2355`). Logo pode publicar 100% entre conferidos com finais escapando da conferência (`bin/disjuntor.py:2333`, `bin/disjuntor.py:2353`).

Conflita com a exigência literal I-3 fornecida de reprovar pouso sem cada etapa medida `[nao verificado: anexo não existe na cópia]` e com origem/escopo literal de `CLAUDE.md:17`, `CLAUDE.md:37`. **Nuance:** o cabeçalho histórico do próprio disjuntor ainda diz “FALHA ABERTO”; não afirmar que todo contrato dele exige fail-closed (`bin/disjuntor.py:34`). O B3 atual exige negar quando não sabe se a obra está parada (`bin/disjuntor.py:1937`, `bin/disjuntor.py:1941`, `bin/disjuntor.py:1245`).

Universo real de finais APROVADO vivos e sua contagem: `[nao verificado]`. Em código, universo = arquivos `agent-*.jsonl` no diretório de subagentes cujo meta seja verificador, primeiro prompt seja final, obra legível e último veredito APROVADO; cai no escape quando falta fotografia exata (`bin/disjuntor.py:847`, `bin/disjuntor.py:852`, `bin/disjuntor.py:855`, `bin/disjuntor.py:863`, `bin/disjuntor.py:917`).

10. **Briefs vivos.** O diretório `logs` não está disponível nesta cópia `[nao verificado]`; não se pode certificar lista/estado de fila. Lista da entrega é afirmação do construtor, não censo observado: `akmed/brief_v5`, `codexneg/brief_v4`, `esminst`, `painel_um`, `plantio_mais1`, `BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS/brief.md`, `FIO-E0-V4-E4-ESCRITORES/brief.md` `[nao verificado]`. Condição mecânica comprovada: se forem da fila 1 e sem ETAPAS, próxima abertura recusa; fora da fila 1 retorna antes do juiz (`bin/disjuntor.py:1853`, `bin/disjuntor.py:1869`, `bin/disjuntor.py:1871`, `bin/disjuntor.py:1887`).

## 09/11 12:02 · sessao 882c97d3b33a · - · - · origem=Ronald (tmux)

**2 — Juiz único.** A forma tem um juiz, `etapas_do_brief`, chamado para cada brief pela porta comum `_censo_nega` (`bin/disjuntor.py:1703`, `bin/disjuntor.py:1869`). CENSO e ETAPAS compartilham o recorte `_bloco_do_titulo`; seus validadores de conteúdo são distintos (`bin/disjuntor.py:1626`, `bin/disjuntor.py:1653`, `bin/disjuntor.py:1710`).

Há **derivação paralela no verificador**: sua instrução manda obter N do brief e n das citações no RELATO; B3 obtém N da última fotografia e calcula n por conjunto de K válidos, cruzando os dois valores (`.claude/agents/verificador.md:18`, `bin/disjuntor.py:874`, `bin/disjuntor.py:894`, `bin/disjuntor.py:928`). Um brief alterado pode produzir denominador diferente e reprovação integral (`bin/disjuntor.py:930`).

Stop, wrapper, ESTADO e handoff não implementam parser próprio de ETAPAS: delegam respectivamente a B3, `--censo`, `ler/linha` e ao reconhecedor de PAREI (`bin/hook_stop_fila1.py:1204`, `bin/codex_pergunta.sh:234`, `bin/gerar_estado.py:280`, `bin/handoff_sessao.sh:130`). Juiz registrado de negócio duplicado por esta mudança: `[nao verificado]`.

**6 — ESMERIL-SEMANTICA.** Há dois motivos sob a mesma chave: `NEG_CENSO` afirma literalmente “sem CENSO”, inclusive quando só ETAPAS falha; ambos escrevem `PAREI: censo-<obra> | espera Codex` (`bin/disjuntor.py:1612`, `bin/disjuntor.py:1616`, `bin/disjuntor.py:1887`). O medidor lê essa negativa como censo e publica “aberturas negadas por censo”/horas esperando censo (`bin/disjuntor.py:2261`, `bin/disjuntor.py:2308`, `bin/disjuntor.py:2371`). Stop/handoff reconhecem apenas o mesmo literal e a existência de wrapper vivo com papel censo/leitura; não distinguem o defeito original (`bin/hook_stop_fila1.py:730`, `bin/hook_stop_fila1.py:763`, `bin/hook_stop_fila1.py:773`, `bin/handoff_sessao.sh:130`).

**8 — Escritores e leitores.**

| Estado/linha | Escritor | Leitor |
|---|---|---|
| `logs/sessao/disjuntor.log` | `trilha`, append, carimbo interno e detalhe limitado a 300 caracteres: `bin/disjuntor.py:203` | Snapshot: `bin/disjuntor.py:879`; medidas censo/ETAPAS: `bin/disjuntor.py:2249`, `bin/disjuntor.py:2322` |
| `ETAPAS SNAPSHOT` | Wrapper: `bin/disjuntor.py:1882`; Agent, depois de passar demais verificações: `bin/disjuntor.py:1967` | Última linha com obra exatamente igual: `bin/disjuntor.py:881` |
| `ETAPAS PROVADAS/METADE/SEM-SNAPSHOT` | Preparadas em `bin/disjuntor.py:919`, `bin/disjuntor.py:937`, `bin/disjuntor.py:939`; anunciadas após salvar estado: `bin/disjuntor.py:1147` | `_etapas_24h`: `bin/disjuntor.py:2324` |
| `est['etapas'][aid]` | `_confere_etapas`: `bin/disjuntor.py:910`, `bin/disjuntor.py:918`, `bin/disjuntor.py:931` | Memo impede nova conferência: `bin/disjuntor.py:913` |
| `b3[obra]['metade'][aid]` | Integração na fila já existente: `bin/disjuntor.py:1096` | Mensagem do disparo: `bin/disjuntor.py:1113` |
| `ETAPAS: n/N` no handback | Agente instruído por `.claude/agents/verificador.md:18`; execução real `[nao verificado]` | Extração de handback: `bin/disjuntor.py:670`; parser/cruzamento: `bin/disjuntor.py:928` |
| `ETAPA <obra> K: ... -> ...` no RELATO | Escritor concreto da sessão/linhas reais `[nao verificado]` | Leitura só de RELATO vivo: `bin/disjuntor.py:892` |
| `PAREI censo` | `_parei_censo_no_relato` chama porta canônica `no_topo`: `bin/disjuntor.py:334`; a porta usa trava do diretório: `app/core/esteira_vigia.py:846`, `app/core/esteira_vigia.py:892` | Stop: `bin/hook_stop_fila1.py:773`; handoff: `bin/handoff_sessao.sh:130`; alarme: `bin/alarme_sessao_ociosa.py:137` |
| `ler()['etapas']` / texto ESTADO | Medidor e composição: `bin/disjuntor.py:2204`, `bin/disjuntor.py:2348`, `bin/disjuntor.py:2447` | `gerar_estado` chama `ler/linha`, inclui `dj_txt`, grava destino: `bin/gerar_estado.py:280`, `bin/gerar_estado.py:378`, `bin/gerar_estado.py:390` |

O memo/metade passa pela trava única do estado, gravação atômica e anúncio ainda sob trava (`bin/disjuntor.py:357`, `bin/disjuntor.py:376`, `bin/disjuntor.py:382`, `bin/disjuntor.py:392`). Não encontrei segundo escritor de máquina introduzido para RELATO; as negativas reutilizam `no_relato`/`no_topo` (`bin/disjuntor.py:304`, `bin/disjuntor.py:334`).

**9 — Classes presentes.**

- **Derivação paralela:** verificador calcula N do brief atual enquanto B3 lê fotografia (`.claude/agents/verificador.md:18`, `bin/disjuntor.py:916`).
- **Campo com dois significados:** negativa de ETAPAS contabilizada/rotulada como CENSO (`bin/disjuntor.py:1887`, `bin/disjuntor.py:2261`, `bin/disjuntor.py:2371`).
- **Ordem de trava/snapshot antes da abertura efetiva:** wrapper grava snapshot dentro de `--censo` antes de tentar prender a vaga; vaga ocupada ou HEAD inválido recusam depois. Assim uma tentativa sem abertura pode substituir o denominador vigente (`bin/disjuntor.py:1882`, `bin/codex_pergunta.sh:234`, `bin/codex_pergunta.sh:239`, `bin/codex_pergunta.sh:241`, `bin/disjuntor.py:883`).
- **Fail-open na persistência:** `trilha` engole falha e não informa sucesso; a porta retorna aprovação mesmo que não tenha gravado snapshot; ausência depois passa como SEM-SNAPSHOT (`bin/disjuntor.py:210`, `bin/disjuntor.py:1882`, `bin/disjuntor.py:1886`, `bin/disjuntor.py:917`).
- **Pouso antes da conferência:** Bash principal sem `codex_pergunta` sai antes de B3; B3 é consultado em próximas chamadas de verificador ou Stop. Um merge/deploy Bash após o handback pode ocorrer antes dessa consulta (`bin/disjuntor.py:1977`, `bin/disjuntor.py:1989`, `bin/disjuntor.py:2005`, `bin/hook_stop_fila1.py:1204`). O wrapper chama B3 sem transcript, portanto não descobre sozinho um novo handback opus (`bin/disjuntor.py:1238`, `bin/disjuntor.py:1241`, `bin/codex_pergunta.sh:378`).
- **Relógio fora da borda:** `_etapas_24h` nova recebe `agora` e deriva a janela sem ler relógio (`bin/disjuntor.py:2315`, `bin/disjuntor.py:2319`). `trilha` lê `datetime.now` internamente e `decide_b3` tem fallback `time.time`, ambos preexistentes pelo diff fornecido (`bin/disjuntor.py:208`, `bin/disjuntor.py:1067`). Violação nova de relógio independente: `[nao verificado]`.

## 09/11 12:02 · sessao 57a45d0fa874 · - · - · origem=Ronald (tmux)

Leitura estática confirma asserções concretas para os 20 mutantes; execução/20 mortos permanece `[nao verificado]`, pois o selo cria, altera e apaga arquivos (`bin/tests/test_etapas_no_brief.sh:23`, `:294`, `:301`).

| Mutantes | Asserção que deve matar |
|---|---|
| e1-sem-chamada; e1-sem-sequencia; e1-ruim-some; e1-resultado-vazio | RC das formas inválidas, sequência, última etapa inválida e resultado vazio (`bin/tests/test_etapas_no_brief.sh:99`, `:104`, `:116`, `:101`). |
| e2-sem-foto-wrapper; e2-sem-foto-agent | Snapshot presente e anterior ao APROVADO (`bin/tests/test_etapas_no_brief.sh:93`, `:139`). |
| e2-b3-le-o-brief; e2-primeira-foto | Brief editado mantém N=3; reabertura muda para N=5 (`bin/tests/test_etapas_no_brief.sh:232`, `:237`). |
| e3-metade-passa; e3-so-o-verificador; e3-so-o-relato | Negação efetiva no próximo verificador e disparo esperado; RELATO inferior e ausência da linha do verificador (`bin/tests/test_etapas_no_brief.sh:167`, `:180`, `:192`, `:196`). |
| e3-copiada-conta | Expectativa copiada não completa etapa (`bin/tests/test_etapas_no_brief.sh:200`, `:203`). |
| e3-sem-memo; e3-sem-snapshot-sem-memo | Exatamente um evento em chamadas repetidas (`bin/tests/test_etapas_no_brief.sh:188`, `:214`). |
| e3-ilegivel-livre | RELATO ilegível precisa produzir deny e depois permitir nova conferência (`bin/tests/test_etapas_no_brief.sh:220`, `:222`). |
| e3-sem-aviso-de-metade | Texto literal do disparo (`bin/tests/test_etapas_no_brief.sh:180`). |
| e4-finais-nao-contam; e4-sem-janela | Dict exato, incluindo exclusão de evento velho/futuro (`bin/tests/test_etapas_no_brief.sh:252`, `:253`, `:257`). |
| e4-fora-da-linha; e4-ilegivel-zero | Texto em `--linha`/ESTADO e resultado None (`bin/tests/test_etapas_no_brief.sh:260`, `:265`, `:272`). |

A mordida tem limite: o harness aceita **qualquer RC diferente de zero**, sem conferir se caiu pela asserção pretendida; a mensagem FALHA apenas é impressa (`bin/tests/test_etapas_no_brief.sh:304`, `:307`). Existe controle sem mutação (`bin/tests/test_etapas_no_brief.sh:289`).

Casos pedidos:

- Aceitar bloco vazio ou etapa sem RESULTADO é detectado pelas asserções existentes (`bin/tests/test_etapas_no_brief.sh:107`, `:100`, `:116`).
- Contar obra por prefixo deve ser detectado: o caso inclui `OBRA-E3X` como única outra testemunha de K=3; a comparação atual é igualdade (`bin/tests/test_etapas_no_brief.sh:201`, `:203`; `bin/disjuntor.py:897`).
- **n>N já passa no código atual**: provei em memória N=3, RELATO 3 etapas e `ETAPAS: 4/3` → PROVADAS; não há guarda n≤N (`bin/disjuntor.py:929`, `:930`, `:939`). O selo não apresenta caso n>N (`bin/tests/test_etapas_no_brief.sh:178`, `:207`).
- Mutante simples sobrevivente por inspeção: substituir `_ALNUM_RE.search(med)` por `True`, preservando o restante da condição. Aceita resultado vazio; todas as linhas candidatas usadas pelo teste têm resultado não vazio, sem caso dedicado ao vazio no RELATO (`bin/disjuntor.py:900`; `bin/tests/test_etapas_no_brief.sh:162`, `:200`, `:222`). Execução desse mutante: `[nao verificado]`.
- “Medida” é filtro lexical: `pendente`, `nao medido` e `**RESULTADO OBSERVAVEL rc 0**` contam; confirmei em memória resultado 3. Só se exige ASCII alfanumérico e ausência do prefixo literal sem Markdown (`bin/disjuntor.py:896`, `:900`, `:1700`).
- Snapshot posterior substitui o anterior: leitor toma último N da obra, sem vincular abertura/aid ou verificar hash (`bin/disjuntor.py:869`, `:879`, `:883`). O teste cobre reabertura sequencial, não final antigo após nova abertura concorrente (`bin/tests/test_etapas_no_brief.sh:229`, `:237`).
- Hash registrado não fixa semanticamente as etapas: leitor extrai somente N; alteração com o mesmo N não é comparada ao snapshot (`bin/disjuntor.py:869`, `:883`, `:916`).

## 09/11 11:55 · sessao ccfc3330625e · O10-D-PERMUTA-RECIPROCA · censo · origem=Code

**O ponto novo decisivo: duas chamadas cabem no mesmo `atomic`, mas o rollback integral não vem automaticamente.** A porta captura sua recusa e retorna `'recusada'`; se o compositor deixar o `atomic` externo terminar normalmente, a primeira troca pode ser confirmada (`app/ponto/portas/celula.py:652`; `app/ponto/portas/celula.py:694`).

1. **Duas chamadas no mesmo `atomic`**

   **(a) Travas e espera.** Cada chamada trava suas duas cartas por `pk`, exclusivamente nas linhas de `Ausencia`; depois percorre suas células por `(colaborador_id, data)`, adquirindo a trava dentro de `editar_dia` (`app/ponto/portas/celula.py:657`; `app/ponto/portas/celula.py:673`; `app/ponto/portas/celula.py:593`).

   Essa ordem é **por chamada, não pelas quatro células**. Para A<B, a sequência composta pode ser `A/d1 → B/d1 → A/d2 → B/d2`. Duas composições concorrentes começando pelos pares opostos podem adquirir conjuntos distintos e depois esperar circularmente pelas cartas/células restantes — dedução estática dessas aquisições; deadlock efetivamente ocorrido: **[nao verificado]** (`app/ponto/portas/celula.py:657`; `app/ponto/portas/celula.py:673`).

   Dentro da mesma transação, a segunda chamada não precisa aguardar a própria transação liberar suas travas; espera causada por outra transação continua possível. Ensaio concorrente dessa composição: **[nao verificado]**; o teste existente inspeciona AST, sem executar essa concorrência (`app/ponto/tests/test_o10b_editar_dia_troca.py:314`).

   **(b) Lavra.** Cada edição alcançada consulta novamente a empresa atual e as competências lavradas, **antes inclusive de decidir `'igual'`**. Portanto, são até quatro avaliações, interrompidas na primeira recusa; `apesar_da_lavra` pula a guarda (`app/ponto/portas/celula.py:513`; `app/ponto/portas/celula.py:568`; `app/ponto/portas/celula.py:594`; `app/ponto/portas/celula.py:676`).

   Isso não constitui uma pré-validação das quatro: a consulta do lado seguinte ocorre depois da escrita do anterior. Tampouco há trava dos registros de exportação nessa consulta; o TOCTOU já aparece na fila documental (`app/ponto/portas/celula.py:675`; `app/ponto/management/commands/gerar_celulas.py:161`; `app/docs/BACKLOG.md:1876`).

   **(c) Carimbos e efeitos.** Cada célula alterada recebe seu próprio `timezone.now()`, autor e trilha; não nasce um carimbo comum de permuta. Com quatro alterações, o caminho grava quatro trilhas `editar_dia` e duas `trocar_plantao`, sem uma identificação comum (`app/ponto/portas/celula.py:598`; `app/ponto/portas/celula.py:611`; `app/ponto/portas/celula.py:686`).

   | Efeito | Resultado da composição |
   |---|---|
   | Recálculo | A deduplicação por competência é local a cada execução do efeito posterior. Com quatro alterações e sem falhas nos trechos protegidos por `try/except`, são quatro agendamentos diretos; se d1/d2 pertencem à mesma competência, cada colaborador/competência pode ser recalculado duas vezes. Não há chave de cancelamento ou consolidação entre chamadas (`app/ponto/portas/celula.py:450`; `app/ponto/portas/celula.py:458`; `app/ponto/portas/celula.py:627`; `app/ponto/services/fechamento.py:1824`). |
   | Rejunção e reavaliação | Cada alteração agenda um callback contendo ambas. A rejunção repete a busca por colaborador, mas só alcança FK nula; a reavaliação recorta os dias e chamados vivos. Repetição de execução não significa necessariamente repetição de escrita (`app/ponto/portas/celula.py:490`; `app/ponto/portas/celula.py:502`; `app/chamados/rejuncao.py:60`; `app/chamados/services/feriado_reavaliar.py:17`; `app/chamados/services/feriado_reavaliar.py:76`). |
   | Rejulgamento | É síncrono, após cada edição; pode observar a composição ainda incompleta. Pula futuro/vereditos humanos e captura falhas. Não há um rejulgamento final agregado das quatro células nesse caminho (`app/ponto/portas/celula.py:427`; `app/ponto/portas/celula.py:434`; `app/ponto/portas/celula.py:437`; `app/ponto/portas/celula.py:627`). |
   | Cobrança órfã | É marcada dentro da transação da edição, por pessoa/dia, consultando os carimbos recém-escritos. O marcador atualiza contexto, não cancela os outros efeitos da composição (`app/ponto/portas/celula.py:625`; `app/ponto/management/commands/gerar_celulas.py:241`; `app/ponto/management/commands/gerar_celulas.py:255`). |

   Colisões concretas entre os efeitos transitivos das quatro células: **[nao verificado]**.

   **(d) Visibilidade e cache.** A segunda chamada usa os argumentos apenas para obter os `pk` e relê cartas e colaboradores; cada edição relê sua célula com trava e carrega novamente o vínculo. Pelo caminho de leitura, encontra as escritas anteriores da mesma transação, sem reutilizar as instâncias da primeira chamada (`app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:657`; `app/ponto/portas/celula.py:593`; `app/escala/alimentacao.py:355`).

   O cartório também relê a célula. O identity map encontrado pertence às instâncias explicitamente incluídas em `celulas_do_ato` e é restaurado ao sair; não é um cache global das duas trocas. Objetos ou mapas que um futuro compositor mantenha por conta própria: **[nao verificado]** (`app/ponto/services/cartorio.py:1976`; `app/escala/alimentacao.py:43`; `app/escala/alimentacao.py:47`).

   **(e) `'igual'`.** Um lado igual não impede a edição do outro: a porta visita ambos e só retorna `'igual'` quando ambos o são. Um lado igual preserva carimbo e não produz sua trilha individual nem seus efeitos; se o outro mudar, a trilha do par registra os dois resultados (`app/ponto/portas/celula.py:594`; `app/ponto/portas/celula.py:596`; `app/ponto/portas/celula.py:673`; `app/ponto/portas/celula.py:679`; `app/ponto/portas/celula.py:690`).

   Essa igualdade compara estado da célula, não pedido/cartas/motivo/autor novos. Quatro células distintas não compartilham essa chave; reutilizar as mesmas duas células em sentido inverso pode reescrevê-las novamente (`app/ponto/portas/celula.py:594`; `app/ponto/portas/celula.py:675`).

   **(f) Rollback.** Há dois resultados diferentes:

   - **Segunda retorna `'recusada'`, externo termina normalmente:** o savepoint da segunda chamada é desfeito; a primeira permanece elegível ao commit, inclusive suas trilhas e callbacks (`app/ponto/portas/celula.py:652`; `app/ponto/portas/celula.py:694`).
   - **Compositor provoca rollback externo:** desfaz as escritas transacionais de ambas, incluindo auditoria, marca órfã e gravações síncronas do rejulgamento. O savepoint de auditoria não é commit independente; os callbacks registrados por `on_commit` dependem do commit externo (`app/core/observ.py:133`; `app/ponto/portas/celula.py:625`; `app/ponto/portas/celula.py:434`; `app/ponto/portas/celula.py:502`; `app/ponto/services/fechamento.py:1802`).

   **Há um limite adicional ao rollback:** o rejulgamento pode alcançar abertura de cobrança e envio imediato de push de supervisão, antes do commit. Rollback SQL não recolhe uma notificação já enviada; envio concreto nesse cenário: **[nao verificado]** (`app/ponto/services/cartorio.py:1740`; `app/ponto/management/commands/detectar_ausencias.py:778`; `app/chamados/push_utils.py:40`).

2. **Validação antes da primeira escrita**

   | Controle | Reutilização sem gravar |
   |---|---|
   | Interjornada de 11h | Validador pronto localizado: **[nao verificado]**. O helper de disponibilidade declara expressamente que não valida 11h (`app/ponto/utils.py:152`). |
   | Sobreposição de turnos | Juiz pronto da colisão geométrica da permuta: **[nao verificado]**. `marcos_turno` fornece datetimes crescentes, incluindo meia-noite, e `fim_turno` fornece o término; são insumos, não o veredito (`app/escala/servico_jornada.py:61`; `app/escala/servico_jornada.py:89`). |
   | Permissão | `tem_acao(user, *acoes)` responde por ação sem gravação; `pode_agir_chamado` responde pelo departamento dono do chamado. A ação específica e a autorização contextual das quatro células: **[nao verificado]** (`app/colaboradores/mixins.py:322`; `app/chamados/utils.py:68`). |
   | Competência exportada/lavrada | `competencia_fechada_para_celula` já é juiz separado, registrado como K9, retornando bool ou fontes. `_competencias_exportadas` enumera suas janelas e `autoridade_da_lavra` fornece a frase da recusa, sem gravar (`app/folha/export.py:589`; `app/core/juizes.py:322`; `app/ponto/management/commands/gerar_celulas.py:126`; `app/ponto/management/commands/gerar_celulas.py:208`). |

   Não há, entre esses quatro controles, um predicado existente exclusivamente preso à escrita que precise ser extraído: a lavra já está separada; permissão existe fora, mas não é chamada pelas portas; 11h e colisão não aparecem como guardas nelas (`app/ponto/portas/celula.py:559`; `app/ponto/portas/celula.py:568`; `app/ponto/portas/celula.py:646`; `app/ponto/portas/celula.py:675`).

   `ausencias_sobrepostas` responde sobre períodos de **Ausencia**, não sobre colisão horária dos plantões; usá-la como resposta à segunda checagem mudaria a pergunta (`app/ponto/turnos.py:2795`).

3. **ID próprio na trilha**

   `evento` não possui argumento `correlacao`, `lote` ou `grupo`; aceita `antes/depois`, persistidos nos JSONs da auditoria. `LogAuditoria` tem um `objeto_id` inteiro e nenhuma coluna específica de correlação. Portanto, há espaço técnico no JSON para o identificador, quatro células e pedido (`app/core/observ.py:94`; `app/core/observ.py:133`; `app/core/models.py:298`; `app/core/models.py:300`).

   Atenção ao adaptador: passar simplesmente `correlacao=...` a `evento_kw` não funciona, pois ele só encaminha os campos conhecidos (`app/core/observ.py:142`).

   Existe precedente de metadado `lote` dentro do JSON, mas seu valor é o rótulo fixo `'arquivar_lote'`, sem identificar exclusivamente cada execução. Padrão existente que já ligue quatro células e pedido de permuta: **[nao verificado]** (`app/chamados/views_arquivar.py:146`; `app/chamados/services/arquivo.py:133`).

   Hoje `trocar_plantao` não recebe esse ID: audita a carta cedente como objeto e dois lados no JSON. `Ausencia.chamado_origem` pode apontar para o pedido, mas não é copiado para essa trilha (`app/ponto/portas/celula.py:632`; `app/ponto/portas/celula.py:682`; `app/ponto/portas/celula.py:686`; `app/ponto/models.py:786`).

4. **Duas cartas por lado**

   Para representar literalmente quatro coordenadas distintas pela porta atual, a composição precisa destas quatro `Ausencia`; pessoa/data vêm exclusivamente das cartas relidas (`app/ponto/portas/celula.py:659`; `app/ponto/portas/celula.py:673`):

   | Chamada | Cedente | Substituto |
   |---|---|---|
   | Primeira | A/d1 | B/d1 |
   | Segunda | B/d2 | A/d2 |

   São quatro registros, mas o código não exige quatro arquivos físicos diferentes. Cada chamada exige argumentos presentes, registros encontrados, colaboradores distintos e `documento` preenchido; não compara nomes nem abre os arquivos (`app/ponto/portas/celula.py:653`; `app/ponto/portas/celula.py:660`; `app/ponto/portas/celula.py:662`; `app/ponto/portas/celula.py:664`; `app/ponto/portas/celula.py:684`).

   Não há consumo ou exclusividade global da carta entre chamadas. Reutilizar as mesmas duas invertidas volta às mesmas duas células, em vez de produzir as quatro; a porta também não exige igualdade de datas dentro de cada chamada (`app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:673`).

   Tipo, status, aprovação, período e vínculo com pedido não integram a elegibilidade documental verificada pela troca; lavra e vínculo do dia são avaliados separadamente por `editar_dia` (`app/ponto/portas/celula.py:664`; `app/ponto/portas/celula.py:675`; `app/ponto/portas/celula.py:568`; `app/ponto/portas/celula.py:584`).

A referência documental diverge: nesta cópia, `BACKLOG.md:1879` trata `BO-0511-G2-TRIAGEM`; a correção mencionada no aval não está nessa linha (`app/docs/BACKLOG.md:1879`).

Do censo v1, ficam marcadas como **respondidas pelo aval fornecido**: **v1.1**, quanto à exigência de validar 11h; **v1.2**, quanto à validação impeditiva de sobreposição; **v1.4**, quanto à unidade de quatro células — que exige duas datas distintas para A/B. Numeração física do aval na cópia: **[nao verificado]**.

perguntas de lei: 4

1. **v1.1 — residual:** quais turnos entram na checagem das 11h, inclusive no retroativo? Esse universo não foi explicitado no aval fornecido; juiz específico localizado: **[nao verificado]** (`app/ponto/utils.py:152`).
2. **v1.3 — não respondida:** há restrição entre empresas, vínculos ou tipos de escala diferentes? **[nao verificado]**; a porta consulta vínculo individual, sem comparar esses atributos (`app/ponto/portas/celula.py:584`; `app/ponto/portas/celula.py:673`).
3. **v1.5 — não respondida:** quais condições de tipo, status e período tornam as quatro cartas elegíveis? **[nao verificado]**; a exigência documental atual é apenas presença de `documento` (`app/ponto/portas/celula.py:664`).
4. **v1.6 — não respondida:** eventual resultado `folga_trabalhada` deve pedir decisão do admin? Continua registrada como pergunta aberta (`app/docs/RELATO.md:449`; `app/docs/BACKLOG.md:1876`).

## 09/11 11:53 · sessao 1d0e367e27e2 · - · - · origem=Ronald (tmux)

Complemento:

- **O cache encontrado não reaproveita automaticamente a fotografia da primeira troca.** `julgar_celula` relê a célula e cria contadores novos; seus caches de regra/veto/afastamento são locais, e o `finally` restaura ou limpa o estado da thread (`app/ponto/services/cartorio.py:1976`, `app/ponto/services/cartorio.py:1988`, `app/ponto/services/cartorio.py:1138`, `app/ponto/services/cartorio.py:1274`, `app/ponto/services/cartorio.py:1296`, `app/ponto/services/cartorio.py:1074`). O identity map de EC é atributo das instâncias explicitamente envolvidas em `celulas_do_ato`, instalado/restaurado pelo contexto; os carregadores usados pela edição e pelo cartório fazem queries novas (`app/escala/models.py:1063`, `app/escala/alimentacao.py:43`, `app/escala/alimentacao.py:47`, `app/escala/alimentacao.py:355`, `app/escala/alimentacao.py:148`, `app/escala/servico_jornada.py:46`). Cache de objetos mantidos por um futuro compositor: **[nao verificado]**.

- **Há efeito externo possível antes do commit: push de supervisão.** O rejulgamento síncrono pode alcançar `_abrir_chamado`, cujo caminho chama `push_supervisao` imediatamente; este chega a `webpush`, sem `on_commit` nesse encadeamento (`app/ponto/portas/celula.py:434`, `app/ponto/services/cartorio.py:1740`, `app/ponto/management/commands/detectar_ausencias.py:774`, `app/chamados/push_utils.py:89`, `app/chamados/push_utils.py:110`, `app/chamados/push_utils.py:40`). Portanto, rollback das escritas SQL não promete recolher uma notificação eventualmente enviada; ocorrência real, destinatários e subscriptions: **[nao verificado]**.

## 09/11 11:52 · sessao 1d0e367e27e2 · - · - · origem=Ronald (tmux)

Novidades para consolidação, por leitura estática:

- **Recusa retornada pela 2ª chamada não desfaz automaticamente a 1ª.** `trocar_plantao` abre seu `atomic`, lança `_TrocaRecusada` internamente, captura após sair dele e retorna `'recusada'`; portanto, um `atomic` externo que termine normalmente preserva a primeira troca. O compositor precisa tratar a recusa fazendo o rollback externo; exceções não capturadas, como falha obrigatória da trilha, já propagam (`app/ponto/portas/celula.py:652`, `app/ponto/portas/celula.py:676`, `app/ponto/portas/celula.py:692`, `app/ponto/portas/celula.py:694`). Ensaio específico com duas trocas externas: **[nao verificado]**.

- **A ordenação atual é por par, não pelas quatro células/cartas.** Cada chamada trava cartas por `pk`, depois edita células por `(colaborador_id, data)`; o lock da célula ocorre dentro de `editar_dia` (`app/ponto/portas/celula.py:656`, `app/ponto/portas/celula.py:657`, `app/ponto/portas/celula.py:673`, `app/ponto/portas/celula.py:593`). Com A<B, a composição faz A/d1,B/d1,A/d2,B/d2: A/d2 vem depois de B/d1, contrariando uma ordenação global das quatro chaves; duas composições concorrentes que iniciem pelos pares opostos podem esperar circularmente pelos lotes de cartas/células — dedução das duas aquisições locais (`app/ponto/portas/celula.py:657`, `app/ponto/portas/celula.py:673`). Deadlock efetivamente ocorrido em produção: **[nao verificado]**. O teste de concorrência declarado consulta AST, e o teste de ordem mede apenas uma troca; não demonstram concorrência real da composição de quatro células (`app/ponto/tests/test_o10b_editar_dia_troca.py:314`, `app/ponto/tests/test_o10b_editar_dia_troca.py:442`).

- **Lavra é reconsultada em cada `editar_dia`, inclusive antes de decidir `'igual'`.** A empresa vem de query própria; `_competencias_exportadas` consulta os candidatos e o juiz, sem cache compartilhado entre chamadas; `apesar_da_lavra` pula a guarda (`app/ponto/portas/celula.py:513`, `app/ponto/portas/celula.py:568`, `app/ponto/management/commands/gerar_celulas.py:154`, `app/ponto/management/commands/gerar_celulas.py:161`, `app/ponto/management/commands/gerar_celulas.py:173`, `app/ponto/portas/celula.py:594`). Não há lock de exportação nessa consulta, nem validação antecipada das quatro células no corpo atual (`app/ponto/management/commands/gerar_celulas.py:161`, `app/ponto/portas/celula.py:675`).

- **A 2ª chamada relê instâncias.** Os objetos `Ausencia` recebidos servem para extrair `pk`; as cartas e seus colaboradores são recarregados, e a célula é relida com trava em cada edição (`app/ponto/portas/celula.py:656`, `app/ponto/portas/celula.py:657`, `app/ponto/portas/celula.py:593`). O vínculo também é alimentado novamente por queries de células e escalas (`app/escala/alimentacao.py:316`, `app/escala/alimentacao.py:329`, `app/escala/alimentacao.py:355`, `app/escala/alimentacao.py:357`; query de células em `app/escala/alimentacao.py:73`). Assim, não há reaproveitamento das instâncias anteriores nesse caminho; visibilidade concreta em execução: **[nao verificado]**.

- **`'igual'` é por célula e não aborta o outro lado.** A igualdade exige origem editada, mesmo `trabalha`, DNA e geradora; ignora motivo/autor novos, preserva carimbo e retorna antes da trilha, marca órfã e efeitos posteriores (`app/ponto/portas/celula.py:594`, `app/ponto/portas/celula.py:596`, `app/ponto/portas/celula.py:598`, `app/ponto/portas/celula.py:611`, `app/ponto/portas/celula.py:625`, `app/ponto/portas/celula.py:627`). A troca só retorna `'igual'` após visitar ambos; com apenas um lado igual, grava a trilha do par incluindo o resultado de cada lado (`app/ponto/portas/celula.py:673`, `app/ponto/portas/celula.py:679`, `app/ponto/portas/celula.py:683`, `app/ponto/portas/celula.py:690`). As quatro células distintas não compartilham essa chave de igualdade; reaproveitar a mesma pessoa/data com sentido inverso altera novamente `trabalha`, não vira no-op (`app/ponto/portas/celula.py:594`, `app/ponto/portas/celula.py:675`).

- **Carimbos são independentes por edição.** Cada célula alterada recebe seu próprio `timezone.now`; não existe carimbo comum da permuta; `gerada_em` permanece o de criação, e a edição só salva os seis campos indicados (`app/ponto/portas/celula.py:598`, `app/ponto/portas/celula.py:609`, `app/escala/models.py:1702`). Coincidência temporal concreta: **[nao verificado]**.

- **Não há deduplicação entre chamadas para os efeitos.** A deduplicação financeira `_por_competencia` é uma variável local de `_o_chao_do_dia_mudou`; `editar_dia` chama esse rabo separadamente, com um dia (`app/ponto/portas/celula.py:450`, `app/ponto/portas/celula.py:456`, `app/ponto/portas/celula.py:627`). Se as quatro células mudarem, o caminho direto agenda quatro recálculos e quatro callbacks de rejunção+reavaliação; se d1/d2 caírem na mesma competência, o mesmo colaborador/competência é recalculado duas vezes. Não há cancelamento ou chave de coalescência nesse registro (`app/ponto/portas/celula.py:458`, `app/ponto/services/fechamento.py:1807`, `app/ponto/services/fechamento.py:1824`, `app/ponto/portas/celula.py:490`, `app/ponto/portas/celula.py:502`). Contagem total incluindo signals e efeitos transitivos: **[nao verificado]**.

- **Rejunção repetida é limitada por FK nula; reavaliação, por chamados vivos.** A rejunção procura `celula__isnull=True`; a reavaliação usa `universo` com `status_local__in=VIVOS`, recorta os dias e encerra somente os considerados espúrios pelo juiz (`app/chamados/rejuncao.py:60`, `app/chamados/rejuncao.py:98`, `app/chamados/services/feriado_reavaliar.py:17`, `app/chamados/services/feriado_reavaliar.py:33`, `app/chamados/services/feriado_reavaliar.py:76`, `app/chamados/services/feriado_reavaliar.py:82`). Portanto, são consultas repetidas, sem equivaler necessariamente a novas escritas; colisões concretas: **[nao verificado]**.

- **Rejulgamento acontece antes do commit externo e vê estados intermediários da composição.** Cada edição chama o juiz imediatamente após escrever sua célula; pula futuro e vereditos humanos, e engole falhas (`app/ponto/portas/celula.py:627`, `app/ponto/portas/celula.py:427`, `app/ponto/portas/celula.py:430`, `app/ponto/portas/celula.py:434`, `app/ponto/portas/celula.py:437`). O caminho direto não adia os quatro rejulgamentos para o estado final; eventual interferência entre seus resultados: **[nao verificado]**.

- **Cobrança órfã é marcada na transação, separadamente por célula/dia.** O marcador lê os carimbos recém-escritos, filtra cobranças vivas e grava `orfa_regeneracao` com instante próprio e marcos de emissão, usando a porta de contexto (`app/ponto/portas/celula.py:625`, `app/ponto/management/commands/gerar_celulas.py:241`, `app/ponto/management/commands/gerar_celulas.py:243`, `app/ponto/management/commands/gerar_celulas.py:255`, `app/chamados/services/contexto.py:101`). Quatro células distintas não sobrescrevem diretamente o mesmo carimbo de célula; alcance concreto dos chamados associados: **[nao verificado]**.

- **Rollback externo efetivo inclui trilhas e escritas dos efeitos imediatos, mas os savepoints não são commits independentes.** Auditoria usa outro `transaction.atomic` e cria o `LogAuditoria` dentro dele; falta de trilha causa `RuntimeError` na edição/troca (`app/core/observ.py:124`, `app/core/observ.py:133`, `app/ponto/portas/celula.py:619`, `app/ponto/portas/celula.py:692`). Os efeitos posteriores estão registrados em `on_commit`, não executados diretamente nessa parte do caminho (`app/ponto/services/fechamento.py:1824`, `app/ponto/portas/celula.py:502`). Se o compositor realmente desfizer o atomic externo, a intenção documentada é que callbacks só rodem com fato gravado (`app/ponto/services/fechamento.py:1802`); descarte efetivo da fila por Django em execução: **[nao verificado]**. Mensagens de log e contadores em memória não são gravações transacionais e podem permanecer após falha (`app/core/observ.py:73`, `app/ponto/portas/celula.py:439`).

## 09/11 11:52 · sessao d8d8f4da4ae5 · - · - · origem=Ronald (tmux)

3) **Correlação disponível por JSON; nenhuma API própria de correlação.**

- `evento` aceita `acao/modelo/objeto_id`, descrição, `antes/depois`, ator e IP; não tem `correlacao/lote/grupo`. Os JSONs são gravados sem transformação em `valor_antes/valor_depois` (`app/core/observ.py:94`; `app/core/observ.py:133`). `evento_kw` encaminha somente os campos conhecidos: passar um kw adicional de correlação não o propaga (`app/core/observ.py:142`).
- `LogAuditoria` possui um único `objeto_id` inteiro e dois JSONs; não possui coluna própria para correlação (`app/core/models.py:295`; `app/core/models.py:298`; `app/core/models.py:300`; `app/core/models.py:307`). Assim, há lugar técnico para incluir um identificador e as quatro chaves de célula + pedido dentro de `depois`; contrato/padrão específico já existente para permuta: **[nao verificado]** (`app/core/observ.py:136`).
- Há precedente de **metadado livre de lote**: `extra={'lote': 'arquivar_lote', ...}` é acrescentado ao JSON de trilha junto a `chamado_pk/colaborador_id`; esse `lote` é rótulo fixo, não identificador exclusivo de cada execução (`app/chamados/views_arquivar.py:146`; `app/chamados/services/arquivo.py:130`; `app/chamados/services/arquivo.py:133`).
- Há também precedente de **objeto agrupador**: eventos de holerite usam `modelo='LoteHolerite'` e `objeto_id=lote_id` (`app/holerite/management/commands/processar_lote_holerite.py:148`).
- Hoje a troca não recebe nem propaga tal identificador: sua assinatura não o contém; registra `modelo='Ausencia'`, `objeto_id=a_c.pk` e JSON com dois lados, motivo e override, sem pedido/chamado/correlação (`app/ponto/portas/celula.py:632`; `app/ponto/portas/celula.py:682`; `app/ponto/portas/celula.py:686`). As trilhas individuais de edição também não recebem correlação (`app/ponto/portas/celula.py:611`).
- A `Ausencia.chamado_origem` já pode ligar cada carta a um chamado, mas `trocar_plantao` não lê nem copia esse campo para sua trilha (`app/ponto/models.py:786`; `app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:682`).

4) **Quatro células distintas exigem quatro registros de carta para a composição literal atual; não necessariamente quatro documentos físicos diferentes.**

| Chamada | Carta cedente | Carta substituto |
|---|---|---|
| Primeira | `Ausencia(A,d1)` | `Ausencia(B,d1)` |
| Segunda | `Ausencia(B,d2)` | `Ausencia(A,d2)` |

A correspondência decorre de a porta escolher pessoa/data exclusivamente de cada carta relida e editar essas duas coordenadas; não recebe pessoa/data alternativas (`app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:673`). Uma mesma `Ausencia` tem uma pessoa e uma `data`; reutilizá-la na segunda chamada não passa a representar outra data (`app/ponto/models.py:743`; `app/ponto/models.py:750`). Reusar as mesmas duas cartas invertidas é aceito pelo corpo, mas volta a editar as mesmas duas células, não constitui quatro coordenadas distintas (`app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:673`).

Para cada chamada, a porta exige:

- Argumentos não nulos, registros encontrados pelos `pk`, colaboradores distintos dentro do par e `documento` verdadeiro em ambos (`app/ponto/portas/celula.py:653`; `app/ponto/portas/celula.py:660`; `app/ponto/portas/celula.py:662`; `app/ponto/portas/celula.py:664`).
- Não compara nomes dos documentos nem abre os arquivos; o mesmo caminho de documento pode satisfazer mais de uma carta (`app/ponto/portas/celula.py:664`; `app/ponto/portas/celula.py:684`). O helper de teste inclusive permite repetir seu default `o10b/carta.pdf` (`app/ponto/tests/test_o10b_editar_dia_troca.py:106`).
- Não testa tipo, status, aprovação, `data_fim`, horários parciais, `documento_referencia` ou `chamado_origem`; não valida existência física ou conteúdo do anexo (`app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:664`; `app/ponto/portas/celula.py:673`). Os campos existem no modelo (`app/ponto/models.py:749`; `app/ponto/models.py:751`; `app/ponto/models.py:768`; `app/ponto/models.py:786`; `app/ponto/models.py:791`; `app/ponto/models.py:799`).
- Vínculo do dia e lavra são checados por `editar_dia` para cada coordenada, independentemente da carta (`app/ponto/portas/celula.py:568`; `app/ponto/portas/celula.py:584`; `app/ponto/portas/celula.py:675`).

**Perguntas antigas e aval:** o aval fornecido responde que insuficiência de 11h e sobreposição devem participar da validação das quatro células antes de escrever; não detalha universo de turnos, elegibilidade entre empresas/vínculos/escalas, tipo/status/período documental, nem a pergunta `folga_trabalhada` (**anexo `aval_reciproca.txt`, conteúdo fornecido no enunciado; arquivo físico não localizado [nao verificado]**). A pergunta de `folga_trabalhada` continua registrada literalmente como aberta na cópia (`app/docs/RELATO.md:449`; `app/docs/BACKLOG.md:1876`).

**Divergência documental relevante:** o aval fornecido diz “BACKLOG:1879 corrigido”, mas a linha 1879 desta cópia trata `BO-0511-G2-TRIAGEM`; não trata permuta (`app/docs/BACKLOG.md:1879`). A presença do aval novo em arquivo da cópia é **[nao verificado]**.

## 09/11 11:52 · sessao 299fbae42c6c · - · - · origem=Ronald (tmux)

Pré-checagens reutilizáveis encontradas:

| Checagem | Código sem escrita e limite |
|---|---|
| Competência exportada/lavrada | `competencia_fechada_para_celula(empresa_id, mes, ano, colaborador_id=None, fontes=False)` retorna bool ou fontes, combinando TXT vigente e holerite do próprio colaborador com lote publicado; executa consultas, sem gravação (`app/folha/export.py:589`; `app/folha/export.py:609`). Está registrado como K9 (`app/core/juizes.py:315`). |
| TXT entregue | `competencia_entregue` retorna bool ou registro vigente, excluindo o vigente invalidado; chama `anotar_vigencia`/`so_vigentes`, que consultam e anotam instâncias em memória, sem persistir (`app/folha/export.py:556`; `app/folha/export.py:580`; `app/folha/models.py:192`; `app/folha/models.py:203`). |
| Janela lavrada do dia | `_competencias_exportadas` enumera competências candidatas, pergunta K9 e converte em janelas; pode ser reutilizada antes das quatro escritas, como já faz `editar_dia` (`app/ponto/management/commands/gerar_celulas.py:126`; `app/ponto/management/commands/gerar_celulas.py:161`; `app/ponto/management/commands/gerar_celulas.py:172`; `app/ponto/portas/celula.py:568`). |
| Recusa falante da lavra | `autoridade_da_lavra` escolhe a competência intersectada e `fonte_da_lavra` traduz as fontes retornadas pelo mesmo K9; não gravam (`app/ponto/management/commands/gerar_celulas.py:183`; `app/ponto/management/commands/gerar_celulas.py:208`). |
| Permissão por ação | `tem_acao(user, *acoes)` retorna bool por autenticação, superuser e permissões `colaboradores.rbac_*`; não grava (`app/colaboradores/mixins.py:322`). `editar_escalas` já é ação usada nas views de escala; sua eleição específica para confirmar permuta: **[nao verificado]** (`app/escala/views.py:140`). |
| Permissão contextual de chamado | `pode_agir_chamado` retorna a permissão do departamento dono do chamado; é reutilizável para esse contexto, sem julgar os quatro dias nem o par (`app/chamados/utils.py:32`; `app/chamados/utils.py:52`; `app/chamados/utils.py:68`). |
| Interjornada 11h | Validador pronto localizado: **[nao verificado]**. O helper de disponibilidade declara expressamente que **não** valida descanso interjornada de 11h (`app/ponto/utils.py:141`; `app/ponto/utils.py:152`). |
| Sobreposição geométrica de turnos | Validador pronto do par localizado: **[nao verificado]**. Há componentes de geometria e leitura, abaixo, mas eles não retornam esse veredito (`app/escala/servico_jornada.py:61`; `app/core/juizes.py:195`). |

Componentes reaproveitáveis de geometria: `EC.eh_dia_trabalho` lê célula ou fallback do vínculo; `EC.marcos_do_dia` lê DNA ou template; `marcos_turno(..., ec=..., celulas=...)` encadeia marcos em datetimes crescentes, inclusive virada da meia-noite; `fim_turno` retorna o último marco. São insumos para calcular descanso e colisão, não um juiz que valide a permuta (`app/escala/models.py:1087`; `app/escala/models.py:1098`; `app/escala/models.py:1117`; `app/escala/servico_jornada.py:61`; `app/escala/servico_jornada.py:89`).

Cuidado com domínio: `ausencias_sobrepostas`, `veto_de_lancamento` e `checar_ausencia` são consultas/validações reutilizáveis de **Ausência/fato humano**, não sobreposição horária de plantões; `checar_ausencia` já é prévia explicitamente sem escrita (`app/ponto/turnos.py:2795`; `app/ponto/precedencia.py:430`; `app/ponto/services/checar_ausencia.py:15`).

No caminho de escrita, a lavra já é bloqueada por `editar_dia` antes do seu `atomic`; não existe necessidade de extrair o juiz da escrita, pois K9 e a enumeração de janelas já estão separados (`app/ponto/portas/celula.py:568`; `app/ponto/portas/celula.py:592`; `app/folha/export.py:589`). Identificação do autor e motivo são guardas locais, não autorização; `trocar_plantao` apenas exige `autor.pk`/motivo e chama `editar_dia`, sem `tem_acao`/`pode_agir_chamado` (`app/ponto/portas/celula.py:646`; `app/ponto/portas/celula.py:668`). Interjornada e colisão geométrica tampouco aparecem como guardas locais nessas portas; implementação pronta dessas duas verificações: **[nao verificado]** (`app/ponto/portas/celula.py:559`; `app/ponto/portas/celula.py:584`; `app/ponto/portas/celula.py:673`).

`core/permissoes` nesta cópia: **[nao verificado]**; os endereços concretos de permissão encontrados são os acima (`app/colaboradores/mixins.py:322`; `app/chamados/utils.py:68`).

## 09/11 11:50 · sessao d03007e09ff6 · RETRATO-POR-EVENTO · censo · origem=Code

O frescor atual pode deixar passar mudanças nas fontes do retrato: ele observa somente o carimbo do fechamento, enquanto o lavrador também lê cadastro, células, vínculos, férias, ausências, rubricas e o fio dos chamados. Porém, “escala e ausência não invalidam” não vale para todos os caminhos desta cópia: existem invalidações indiretas do fechamento. (`app/folha/retrato_folha.py:1298`; `app/escala/signals.py:124`; `app/ponto/services/ausencia.py:941`)

Execução atual dos crons, conteúdo atual da fila e custo atual de produção: **[nao verificado]**.

**1. Fontes do lavrador**

O caminho começa em `handle → montar_linhas → fora_da_folha → dias_que_seguram → classificar_export`; acrescenta `_causas_gravadas` e, para o dono do bloqueio, `alimentar_portas/porta_alcancavel_do_dia`. (`app/folha/management/commands/lavrar_retrato_folha.py:65`; `app/folha/retrato_folha.py:1157`; `app/folha/retrato_folha.py:1160`; `app/folha/fora_da_folha.py:119`; `app/folha/fora_da_folha.py:166`; `app/folha/fora_da_folha.py:178`)

O fecho inclui os seguintes caminhos transitivos:

- Retenção → célula → protestos → juiz dos períodos/pausas. (`app/folha/export.py:960`; `app/folha/export.py:1266`; `app/folha/export.py:1253`; `app/folha/export.py:1142`)
- Homologação → eventos do fechamento, ciclo do vínculo, régua CCT e datas de ausência. (`app/folha/export.py:994`; `app/folha/export.py:995`; `app/folha/export.py:996`; `app/folha/export.py:1005`)
- Cadastro zero → previsto → métodos de vínculo/template → folgas e feriados. (`app/folha/export.py:934`; `app/escala/utils.py:2266`; `app/escala/utils.py:2269`; `app/escala/models.py:1286`; `app/escala/models.py:1395`)
- Dono → fila/verbos/pergunta viva → tranca, trilha humana, materialização e batidas. (`app/chamados/juizes.py:1307`; `app/chamados/juizes.py:1310`; `app/chamados/juizes.py:1322`; `app/chamados/juizes.py:1157`; `app/chamados/juizes.py:1088`; `app/chamados/juizes.py:1234`)

A enumeração abaixo distingue **leitura ORM da linha inteira** de consumo individual em Python: consultas sem `only/defer/values` carregam os campos concretos declarados, inclusive campos que a decisão não usa. É por isso que `Colaborador.cpf` aparece no inventário, embora o classificador use nome/código e não consulte o atributo CPF. (`app/folha/export.py:777`; `app/folha/export.py:831`; `app/colaboradores/models.py:319`)

Nas relações, o nome do campo inclui a coluna correspondente `*_id`; `pk` designa a identidade usada nas consultas. As declarações desses campos estão nos respectivos modelos. (`app/colaboradores/models.py:321`; `app/escala/models.py:872`; `app/chamados/models.py:2424`)

Campos carregados por ORM, um por linha:

```text
Empresa.pk -> handle (app/folha/management/commands/lavrar_retrato_folha.py:51)
Empresa.razao_social -> empresas_visiveis (app/colaboradores/queries.py:87)
Empresa.nome_fantasia -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.cnpj -> empresas_visiveis (app/colaboradores/queries.py:86)
Empresa.ativa -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.em_rollout -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.dia_inicio_competencia -> corte_da_empresa (app/ponto/janelas.py:49)
Empresa.janela_he_ativa -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.janela_he_piso_min -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.limite_decisao_he_min -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.janela_he_teto_min -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.janela_he_saida_ativa -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.janela_he_saida_teto_min -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.he_pendente_trava_export -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.janela_he_desde -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.regime_trabalhista -> regua_para (app/core/regua_cct.py:353)

FechamentoMensal.pk -> classificar_export (app/folha/export.py:777)
FechamentoMensal.colaborador -> classificar_export (app/folha/export.py:779)
FechamentoMensal.mes -> classificar_export (app/folha/export.py:778)
FechamentoMensal.ano -> classificar_export (app/folha/export.py:778)
FechamentoMensal.status -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_trabalhadas -> classificar_export (app/folha/export.py:823)
FechamentoMensal.horas_noturnas -> eventos_do_fechamento (app/folha/export.py:447)
FechamentoMensal.horas_extras -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_extras_50 -> eventos_do_fechamento (app/folha/export.py:441)
FechamentoMensal.horas_extras_50_noturna -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_extras_100 -> eventos_do_fechamento (app/folha/export.py:421)
FechamentoMensal.horas_extras_100_feriado -> eventos_do_fechamento (app/folha/export.py:420)
FechamentoMensal.horas_extras_100_noturna -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_folga_trabalhada -> eventos_do_fechamento (app/folha/export.py:446)
FechamentoMensal.horas_atraso -> eventos_do_fechamento (app/folha/export.py:452)
FechamentoMensal.horas_saida_antecipada -> eventos_do_fechamento (app/folha/export.py:452)
FechamentoMensal.horas_falta -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_intra_indenizada -> eventos_do_fechamento (app/folha/export.py:448)
FechamentoMensal.turnos_abertos -> classificar_export (app/folha/export.py:825)
FechamentoMensal.minutos_previstos -> classificar_export (app/folha/export.py:777)
FechamentoMensal.dias_incertos -> classificar_export (app/folha/export.py:777)
FechamentoMensal.minutos_abonados -> classificar_export (app/folha/export.py:777)
FechamentoMensal.minutos_realizados -> classificar_export (app/folha/export.py:777)
FechamentoMensal.dias_previstos -> classificar_export (app/folha/export.py:777)
FechamentoMensal.motivos_espelho -> classificar_export (app/folha/export.py:777)
FechamentoMensal.causa_espelho -> _causas_gravadas (app/folha/retrato_folha.py:1115)
FechamentoMensal.previsto_em -> classificar_export (app/folha/export.py:777)
FechamentoMensal.derivado_em -> classificar_export (app/folha/export.py:777)
FechamentoMensal.semanas_dsr_ok -> classificar_export (app/folha/export.py:777)
FechamentoMensal.semanas_dsr_perdido -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_reflexo_dsr -> classificar_export (app/folha/export.py:777)
FechamentoMensal.saldo_banco_horas -> classificar_export (app/folha/export.py:777)
FechamentoMensal.inconsistencias -> classificar_export (app/folha/export.py:960)
FechamentoMensal.aprovado_por -> classificar_export (app/folha/export.py:777)
FechamentoMensal.aprovado_em -> classificar_export (app/folha/export.py:777)
FechamentoMensal.observacao -> classificar_export (app/folha/export.py:777)
FechamentoMensal.criado_em -> classificar_export (app/folha/export.py:777)
FechamentoMensal.atualizado_em -> classificar_export (app/folha/export.py:777)

Colaborador.pk -> dias_que_seguram (app/folha/fora_da_folha.py:131)
Colaborador.isento_ponto -> isento_ponto (app/core/constants.py:18)
Colaborador.matricula_hasner -> classificar_export (app/folha/export.py:779)
Colaborador.matricula_nexti -> classificar_export (app/folha/export.py:779)
Colaborador.codigo_dominio -> classificar_export (app/folha/export.py:832)
Colaborador.cod_esocial -> classificar_export (app/folha/export.py:779)
Colaborador.cpf -> classificar_export (app/folha/export.py:779)
Colaborador.nome -> classificar_export (app/folha/export.py:831)
Colaborador.empresa -> classificar_export (app/folha/export.py:778)
Colaborador.posto -> classificar_export (app/folha/export.py:779)
Colaborador.situacao -> classificar_export (app/folha/export.py:802)
Colaborador.data_admissao -> motivos_retencao_celula (app/folha/export.py:1198)
Colaborador.data_demissao -> motivos_retencao_celula (app/folha/export.py:1200)
Colaborador.cargo -> classificar_export (app/folha/export.py:779)
Colaborador.data_nascimento -> classificar_export (app/folha/export.py:779)
Colaborador.genero -> classificar_export (app/folha/export.py:779)
Colaborador.estado_civil -> classificar_export (app/folha/export.py:779)
Colaborador.pis -> classificar_export (app/folha/export.py:779)
Colaborador.rg -> classificar_export (app/folha/export.py:779)
Colaborador.rg_emissor -> classificar_export (app/folha/export.py:779)
Colaborador.telefone -> classificar_export (app/folha/export.py:779)
Colaborador.telefone2 -> classificar_export (app/folha/export.py:779)
Colaborador.email_pessoal -> classificar_export (app/folha/export.py:779)
Colaborador.end_rua -> classificar_export (app/folha/export.py:779)
Colaborador.end_numero -> classificar_export (app/folha/export.py:779)
Colaborador.end_complemento -> classificar_export (app/folha/export.py:779)
Colaborador.end_bairro -> classificar_export (app/folha/export.py:779)
Colaborador.end_cidade -> classificar_export (app/folha/export.py:779)
Colaborador.end_uf -> classificar_export (app/folha/export.py:779)
Colaborador.end_cep -> classificar_export (app/folha/export.py:779)
Colaborador.motivo_desligamento -> classificar_export (app/folha/export.py:779)
Colaborador.usuario -> classificar_export (app/folha/export.py:779)
Colaborador.perfil -> classificar_export (app/folha/export.py:779)
Colaborador.deve_trocar_senha -> classificar_export (app/folha/export.py:779)
Colaborador.fcm_token -> classificar_export (app/folha/export.py:779)
Colaborador.foto -> classificar_export (app/folha/export.py:779)
Colaborador.fcm_removido_em -> classificar_export (app/folha/export.py:779)
Colaborador.notificacao_negada_em -> classificar_export (app/folha/export.py:779)
Colaborador.fcm_removido_motivo -> classificar_export (app/folha/export.py:779)
Colaborador.onboarding_completo -> classificar_export (app/folha/export.py:779)
Colaborador.wk_em_operacao -> classificar_export (app/folha/export.py:779)
Colaborador.onboarding_pendente -> classificar_export (app/folha/export.py:779)
Colaborador.aceite_termos_em -> classificar_export (app/folha/export.py:779)
Colaborador.aceite_termos_versao -> classificar_export (app/folha/export.py:779)
Colaborador.gps_concedido_em -> classificar_export (app/folha/export.py:779)
Colaborador.camera_concedida_em -> classificar_export (app/folha/export.py:779)
Colaborador.notif_concedida_em -> classificar_export (app/folha/export.py:779)
Colaborador.telefone_confirmado_em -> classificar_export (app/folha/export.py:779)
Colaborador.email_confirmado_em -> classificar_export (app/folha/export.py:779)
Colaborador.criado_em -> classificar_export (app/folha/export.py:779)
Colaborador.atualizado_em -> classificar_export (app/folha/export.py:779)
Colaborador.plataforma_principal -> classificar_export (app/folha/export.py:779)

CelulaDia.pk -> motivos_retencao_celula (app/folha/export.py:1221)
CelulaDia.colaborador -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.data -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.trabalha -> motivos_retencao_celula (app/folha/export.py:1255)
CelulaDia.origem -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.escala_geradora -> vinculo_do_dia (app/escala/alimentacao.py:270)
CelulaDia.dna -> periodos_do_dia (app/ponto/juiz_batida.py:158)
CelulaDia.gerada_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.regenerada_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.regeneracoes -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.dna_anterior -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.editada_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.editada_por -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.julgada_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.impressao -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.veredito -> motivos_retencao_celula (app/folha/export.py:1233)
CelulaDia.veredito_via -> _pergunta_viva_calc (app/chamados/juizes.py:1168)
CelulaDia.veredito_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.protestos -> motivos_retencao_celula (app/folha/export.py:1246)
CelulaDia.ata -> periodos_do_dia (app/ponto/juiz_batida.py:157)
CelulaDia.ausencia -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.tem_entrada -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.tem_saida -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.insumos_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.status_dia -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.status_rotulo -> motivos_retencao_celula (app/folha/export.py:1206)

EscalaColaborador.pk -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:155)
EscalaColaborador.colaborador -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:149)
EscalaColaborador.posto -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
EscalaColaborador.tipo_escala -> classificar_export (app/folha/export.py:930)
EscalaColaborador.data_inicio -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:149)
EscalaColaborador.data_fim -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:151)
EscalaColaborador.ativa -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:150)
EscalaColaborador.regua_confianca -> classificar_export (app/folha/export.py:930)
EscalaColaborador.regua_classe -> classificar_export (app/folha/export.py:930)
EscalaColaborador.regua_avaliada_em -> classificar_export (app/folha/export.py:930)
EscalaColaborador.regua_dias_baixo -> classificar_export (app/folha/export.py:930)
EscalaColaborador.regua_corroborada_em -> classificar_export (app/folha/export.py:930)
EscalaColaborador.regua_corroborada_por -> classificar_export (app/folha/export.py:930)
EscalaColaborador.folga_calendario -> classificar_export (app/folha/export.py:930)
EscalaColaborador.trabalha_em_feriado -> eh_dia_trabalho_calculado (app/escala/models.py:1390)
EscalaColaborador.data_ancora_colaborador -> classificar_export (app/folha/export.py:801)
EscalaColaborador.origem_ancora -> classificar_export (app/folha/export.py:930)
EscalaColaborador.chamado_ancora -> classificar_export (app/folha/export.py:930)

TipoEscala.pk -> classificar_export (app/folha/export.py:930)
TipoEscala.codigo -> classificar_export (app/folha/export.py:930)
TipoEscala.descricao -> classificar_export (app/folha/export.py:930)
TipoEscala.minutos_jornada -> classificar_export (app/folha/export.py:930)
TipoEscala.hora_inicio -> marcos_do_dia (app/escala/models.py:343)
TipoEscala.hora_fim -> marcos_do_dia (app/escala/models.py:343)
TipoEscala.hora_inicio_intervalo -> marcos_do_dia (app/escala/models.py:344)
TipoEscala.hora_fim_intervalo -> marcos_do_dia (app/escala/models.py:345)
TipoEscala.hora_inicio_intervalo2 -> pausas_cadastradas (app/escala/models.py:276)
TipoEscala.hora_fim_intervalo2 -> pausas_cadastradas (app/escala/models.py:276)
TipoEscala.noturno -> classificar_export (app/folha/export.py:930)
TipoEscala.permite_hora_extra -> classificar_export (app/folha/export.py:930)
TipoEscala.he_extra_antes_min -> classificar_export (app/folha/export.py:930)
TipoEscala.he_extra_depois_min -> classificar_export (app/folha/export.py:930)
TipoEscala.folga_calendario_padrao -> classificar_export (app/folha/export.py:930)
TipoEscala.intervalo_modo -> _intervalo_livre (app/escala/models.py:250)
TipoEscala.intervalo_indenizavel -> periodos_do_dia (app/ponto/juiz_batida.py:310)
TipoEscala.intervalo_duracao_min -> intervalo_cadastrado (app/ponto/juiz_batida.py:143)
TipoEscala.apelido -> classificar_export (app/folha/export.py:930)
TipoEscala.tipo_ciclo -> classificar_export (app/folha/export.py:800)
TipoEscala.ciclo_dias -> classificar_export (app/folha/export.py:930)
TipoEscala.is_template -> classificar_export (app/folha/export.py:930)
TipoEscala.template_nome -> classificar_export (app/folha/export.py:930)
TipoEscala.data_ancora -> classificar_export (app/folha/export.py:930)
TipoEscala.dias_trabalho_semana -> eh_dia_trabalho (app/escala/models.py:831)
TipoEscala.template_pai -> classificar_export (app/folha/export.py:930)
TipoEscala.nivel -> classificar_export (app/folha/export.py:930)
TipoEscala.tipo_base -> classificar_export (app/folha/export.py:930)
TipoEscala.editavel -> classificar_export (app/folha/export.py:930)
TipoEscala.perfil_apuracao -> classificar_export (app/folha/export.py:930)
TipoEscala.folga_dia_semana -> eh_dia_trabalho (app/escala/models.py:838)
TipoEscala.horarios_por_dia -> _override_do_dia (app/escala/models.py:227)

Posto.pk -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.nome -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.empresa -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.tolerante_offline -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.praca -> regua_para (app/core/regua_cct.py:330)
Posto.municipio -> lugar_do_vinculo (app/core/feriados.py:122)
Posto.endereco -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.numero -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.complemento -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.bairro -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.cidade -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.cep -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.opera_em_feriado -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.lotacao_minima -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.lotacao_maxima -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.latitude -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.longitude -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.raio_metros -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.ativo -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.revisar -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)

Praca.pk -> regua_para (app/core/regua_cct.py:330)
Praca.nome -> regua_para (app/core/regua_cct.py:330)
Praca.cidade -> regua_para (app/core/regua_cct.py:330)
Praca.estado -> regua_para (app/core/regua_cct.py:330)
Praca.sindicato -> regua_para (app/core/regua_cct.py:330)
Praca.obs -> regua_para (app/core/regua_cct.py:330)
Praca.adicional_noturno_percentual -> regua_para (app/core/regua_cct.py:330)
Praca.tolerancia_minutos -> regua_para (app/core/regua_cct.py:330)
Praca.banco_horas_prazo_dias -> regua_para (app/core/regua_cct.py:330)
Praca.ativa -> regua_para (app/core/regua_cct.py:330)

Municipio.pk -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Municipio.codigo_ibge -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Municipio.nome -> _lugar (app/core/feriados.py:53)
Municipio.uf -> _lugar (app/core/feriados.py:52)

Feriado.pk -> feriados_de (app/core/feriados.py:83)
Feriado.nome -> feriados_de (app/core/feriados.py:83)
Feriado.data -> _qs (app/core/feriados.py:29)
Feriado.abrangencia -> alcanca (app/core/feriados.py:60)
Feriado.uf -> alcanca (app/core/feriados.py:63)
Feriado.municipio -> alcanca (app/core/feriados.py:66)
Feriado.fonte -> feriados_de (app/core/feriados.py:83)

AplicacaoConvencao.pk -> _aplicacao_vigente (app/core/regua_cct.py:259)
AplicacaoConvencao.sindicato -> _aplicacao_vigente (app/core/regua_cct.py:255)
AplicacaoConvencao.empresa -> _aplicacao_vigente (app/core/regua_cct.py:254)
AplicacaoConvencao.praca -> _aplicacao_vigente (app/core/regua_cct.py:256)
AplicacaoConvencao.ativo -> _aplicacao_vigente (app/core/regua_cct.py:254)
AplicacaoConvencao.criado_em -> _aplicacao_vigente (app/core/regua_cct.py:259)
AplicacaoConvencao.criado_por -> _aplicacao_vigente (app/core/regua_cct.py:259)
AplicacaoConvencao.motivo -> _aplicacao_vigente (app/co

[truncado em 20000 de 69455 caracteres]

## 09/11 11:41 · sessao 6d93743673c9 · - · - · origem=Ronald (tmux)

Fecho confirmado por AST em memória e inspeção de métodos:

- `classificar_export → motivos_retencao_espelho → motivos_retencao_celula → nao_retem_pela_pausa → periodos_do_dia`. (`app/folha/export.py:960`, `:1266`, `:1253`, `:1165`, `:1142`)
- `classificar_export → _codigos_nao_homologados → eventos_do_fechamento / _ciclo_do_colab / feriado_12x36_em_dobra / datas_falta → regua_para / alimentacao / TipoAusencia / Ausencia`. (`app/folha/export.py:966`, `:994`, `:995`, `:996`, `:1005`, `:380`, `:359`, `:180`)
- `classificar_export → minutos_previstos_do_dia → EscalaColaborador.eh_dia_trabalho / marcos_do_dia / pausas_do_dia → TipoEscala / FolgaDia / Feriado`. (`app/folha/export.py:934`; `app/escala/utils.py:2266`, `:2269`, `:2291`; `app/escala/models.py:1096`, `:1133`, `:1208`, `:1286`, `:1395`)
- `fora_da_folha → dias_que_seguram → classificar_export`; depois `alimentar_portas → particionar_verbos / qs_na_fila / pergunta_viva`; `porta_alcancavel_do_dia → chamado_que_lista / perguntas_do_app`. (`app/folha/fora_da_folha.py:161`, `:119`, `:166`; `app/chamados/juizes.py:1307`, `:1310`, `:1322`, `:1399`, `:1400`, `:1339`)
- `pergunta_viva → dia_da_pergunta_trancado → periodo_trancado / _toque_humano_no_dia → LogAuditoria / User`; outros ramos chegam a `classificar_falta`, materialização e `turnos_do_colab`, lendo batidas, células, vínculos e templates. (`app/chamados/juizes.py:1157`, `:964`, `:971`, `:1087`, `:1088`, `:1221`, `:1234`, `:384`)

A resolução sintática preliminar alcançou 178 funções; esse número exclui expansão posterior de métodos de instância, reexports e callbacks. Portanto, não equivale a prova de 178 funções no fecho completo. `[nao verificado]`

`prontidao`, `grade_do_fechamento`, `get_motor_cct`, `EmpresaIntegracao`, `ExportacaoDominio` e `Holerite` não pertencem aos caminhos inspecionados dessas raízes; `grade_da_celula` entra por `_tipo_pelo_marco`, separadamente. (`app/folha/export.py:757`, `:986`; `app/chamados/services/materializacao.py:282`)

Foram enviados ao principal os inventários detalhados de campos, o mapa de leituras de linhas completas e os relógios. Há uma escrita potencial dentro desse fecho: `get_user_sistema()` usa `User.objects.get_or_create(username='sistema')`; nenhum código da aplicação foi executado nesta inspeção. (`app/core/utils.py:18`; `app/chamados/juizes.py:1087`)

## 09/11 11:41 · sessao 8a265e5c5b41 · - · - · origem=Ronald (tmux)

O-OUTBOX verificado na cópia:

- Modelo `EventoCelula`: campos `celula_id`, `colaborador_id`, `data`, `criado_em`, `processado_em`, `tentativas`, `erro`; sem payload e sem índice único de pendente. Não contém empresa nem competência gravadas (`app/escala/models.py:1845-1871`).
- Escritor único: `emitir_evento_celula`; faz `EventoCelula.objects.create(celula_id=..., colaborador_id=..., data=...)` (`app/ponto/portas/celula.py:882-891`).
- `salvar_e_emitir` grava célula e evento no mesmo `atomic`; somente mudanças de `veredito` ou `ata` acordam o consumidor (`app/ponto/portas/celula.py:878-902`).
- Consumidor único de produção: comando `consumir_eventos_celula`, com advisory lock por schema; cron a cada minuto (`app/chamados/management/commands/consumir_eventos_celula.py:17-31`; `app/chamados/reconciliador.py:1215-1242`; `app/config/crons.py:393-394`).
- Agrupa eventos por colaborador dentro de cada lote de até 500; não é debounce por empresa×competência. Roda lotes até esvaziar ou ultrapassar 45 s (`app/chamados/reconciliador.py:1209-1211`; `app/chamados/reconciliador.py:1342-1352`; `app/chamados/reconciliador.py:1402-1404`).
- Consumidores funcionais: rejunção, retratação de perguntas absolvidas, retratação de chamados absolvidos, premissa morta, órfãs pela lâmpada e fechamento por lastro (`app/chamados/reconciliador.py:1363-1383`).
- Falha mantém pendente, incrementa tentativas e guarda erro; após cinco tentativas sai do universo vivo; processados são apagados após sete dias (`app/chamados/reconciliador.py:1209-1210`; `app/chamados/reconciliador.py:1246-1255`; `app/chamados/reconciliador.py:1388-1404`).
- Semeadura evita pendente vivo já existente por célula; a emissão ordinária não deduplica (`app/chamados/reconciliador.py:1278-1310`; `app/ponto/portas/celula.py:891`).

Emissores já existentes:

- `CelulaDia.lavrar_veredito`, `carimbar` e `carimbar_precedencia`: emitem condicionalmente pela interseção com veredito/ata; alteração apenas de status não emite (`app/ponto/portas/celula.py:1003`; `app/ponto/portas/celula.py:1013-1020`; `app/ponto/portas/celula.py:1035`; `app/ponto/portas/celula.py:1067`; `app/ponto/portas/celula.py:1082`).
- `ChamadoColaborador.abrir` e `criar`: emitem quando nasce chamado de módulo de turno com célula ligada (`app/chamados/models.py:971-980`; `app/chamados/models.py:1093-1099`).
- `recompute_turnos`: emite para turnos anteriormente abertos que fecharam, se existir célula para o dia (`app/ponto/nucleo.py:57-66`; `app/ponto/nucleo.py:86-93`).
- `PerguntaDisputa.save`: atualização existente emite via receiver quando toca `CAMPOS_QUE_ACORDAM`; nascimento não emite. Save e receiver estão no mesmo `atomic` (`app/chamados/models.py:3220-3222`; `app/chamados/signals.py:237-244`; `app/chamados/models.py:3413-3423`).
- `reabrir_para_correcao`: update em lote emite explicitamente depois da escrita; `descarimbar_em_lote`, isoladamente, não emite (`app/chamados/services/disputa_emissao.py:1602-1630`; `app/chamados/models.py:3252-3266`).
- `PerguntaDisputa.acordar_celulas` deduplica as células apenas dentro da chamada, pela PK; usa FK da pergunta ou busca pelo par colaborador/data (`app/chamados/models.py:3231-3248`).
- Batidas e ausências podem emitir indiretamente: seus saves/deletes chamam cartório, que lavra a célula; somente mudança de ata/veredito gera evento (`app/ponto/signals.py:85-93`; `app/ponto/signals.py:131-152`; `app/ponto/portas/celula.py:878-902`).
- Mudança do status do chamado pode emitir indiretamente pelo rejulgamento da célula; alterações apenas de contexto/texto/urgência não passam nessa guarda (`app/chamados/signals.py:187-203`).
- `DisputaSupervisao.fechar` pode emitir indiretamente ao fechar chamado-pai/materializar perguntas; não possui INSERT próprio no outbox (`app/chamados/signals.py:18-45`; `app/chamados/signals.py:147-163`).

Correções relevantes ao aval:

- Escala não pode ser classificada genericamente como “não invalida”: pós-save de `EscalaColaborador` chama `invalidar_previsto`, que atualiza `FechamentoMensal.atualizado_em` quando `previsto_em` não é nulo (`app/escala/signals.py:124-133`; `app/ponto/services/fechamento.py:1607-1613`).
- `salvar_tipo` também invalida fechamento após regeneração de frota (`app/escala/services/cadastro_tipo.py:351-359`).
- Ausência também não pode ser classificada genericamente como “não invalida”: edição e decisão chamam `invalidar_fechamento`; o update só alcança registros com `derivado_em` não nulo (`app/ponto/services/ausencia.py:518-524`; `app/ponto/services/ausencia.py:941-948`; `app/ponto/services/fechamento.py:1652-1656`).
- `TipoEscalaAdmin` é escritor fora da porta: usa `admin.ModelAdmin` editável; `EscalaColaboradorAdmin` usa `SoLeitura`. O admin não chama o serviço `salvar_tipo` que faz a propagação/invalidação (`app/escala/admin.py:7-13`; `app/escala/services/cadastro_tipo.py:306-359`).
- `CatalogoDominio`: escritor Python não encontrado; escritor de produção `[nao verificado]`. Modelo/leitor existem (`app/folha/models.py:248-258`; `app/folha/export.py:259-260`).

Inventário de escritores enviado nas mensagens anteriores: Empresa/Colaborador/EscalaColaborador; FolgaDia/calendários/agenda; Ausencia/TipoAusencia; AgendamentoFerias/PeriodoAquisitivo; ChamadoColaborador/PerguntaDisputa/DisputaSupervisao; FechamentoMensal/PeriodoFechado/RubricaDominio. O censo AST da cópia foi executado sem escrita; ele próprio declara que saves de parâmetros, forms e modelos dinâmicos podem permanecer não atribuíveis, portanto não prova completude sozinho (`app/core/censo_escritas.py:43-56`).

_3135 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
