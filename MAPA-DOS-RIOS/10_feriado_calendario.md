# MAPA DOS RIOS — FERIADO E CALENDARIO

## 1. RESUMO

1. `Feriado` tem **6 campos cadastrais**, **3 abrangencias** (`nacional`, `estadual`, `municipal`) e **nenhum campo de status**. (`app/core/models.py:5-28`)
2. Foram identificados **4 eventos de transicao**: nascimento, edicao, carimbo historico de fonte e exclusao; **5 sitios de persistencia**. (`app/core/views_config.py:70`, `:83`, `:144`, `:201`; `app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:35`)
3. Nascimento tem **2 portas**: cadastro manual e importacao; existem **4 rotas mutantes** e **1 entrada historica por RunPython**. (`app/core/urls.py:42-45`; `app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:58`)
4. Importacao admite **9 combinacoes mes/dia**; unicidade cadastral usa **4 campos**: data, abrangencia, UF e municipio. (`app/core/views_config.py:113-123`; `app/core/models.py:28`)
5. Calendario acrescenta **0 transicoes persistidas proprias**: oferta e competencia sao resultados em memoria. (`app/colaboradores/services/calendario.py:11`, `:769`, `:794-810`)
6. Oferta tem **3 pernas** e aceita **4 status**; permissao de quem observa fica separada. (`app/colaboradores/services/calendario.py:94-104`; `app/templates/colaboradores/partials/_calendario_grade.html:131`)
7. Existem **4 chamadas diretas** ao produtor do calendario; a porta plural tem **1 consumidor direto**. (`app/colaboradores/views.py:1949`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:160`; `app/colaboradores/services/calendario.py:132`; `app/chamados/services/vigia_fio.py:271`)
8. CRUD solicita trilha; importacao e backfill nao a solicitam nesses corpos; falhas de auditoria sao capturadas. (`app/core/views_config.py:75`, `:87`, `:127-152`, `:194`; `app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:20-35`; `app/core/auditoria.py:17-29`)

## 2. TABELA DE TRANSICOES

“Cadastrado”, “alterado” e “removido” abaixo descrevem existencia e valores do registro; nao sao enums adicionados ao modelo. As abrangencias usam as mesmas operacoes, sem portas especificas nacional→estadual→municipal. (`app/core/models.py:5-31`; `app/core/views_config.py:55-70`)

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") |
|---|---|---|---|---|---|---|---|
| T1 | Registro inexistente → `Feriado` cadastrado. (`app/core/views_config.py:83`, `:144`) | Manual: `_requer_admin`, POST e `fonte` preenchida. (`app/core/views_config.py:12-13`, `:42-50`) Importacao: permissao/POST, data parseavel, allowlist e deduplicacao. (`app/core/views_config.py:136-147`, `:158`) Unicidade no modelo. (`app/core/models.py:28`) | **Nascente manual:** rota novo → `form_feriado`. (`app/core/urls.py:42`) **Nascente importacao:** rota importar → `importar_feriados_nacionais` → `_importar_do_payload`. (`app/core/urls.py:45`; `app/core/views_config.py:156`, `:167`) | Manual: `objects.create`. (`app/core/views_config.py:83-86`) Importado: `objects.get_or_create`. (`app/core/views_config.py:144-147`) | Manual grava os 6 campos e solicita trilha `criar`, com valores posteriores. (`app/core/views_config.py:83-94`) Importacao fixa nacional/UF vazia/municipio vazio e usa nome/fonte da allowlist como defaults; existente e preservado, sem nova criacao. Nao chama auditoria nesse corpo. (`app/core/views_config.py:127-152`) | Lista cadastral. (`app/core/views_config.py:24`) Alcance territorial. (`app/core/feriados.py:57-83`) Calendario. (`app/colaboradores/services/calendario.py:317-318`) | `app/core/tests/test_smoke_clique_feriado_form.py::test_o_clique_no_Salvar_do_novo_grava_o_feriado_digitado` — `app/core/tests/test_smoke_clique_feriado_form.py:119-127`. Importacao: `app/core/tests/test_contract_feriado_fonte.py::test_MORDE_payload_com_carnaval_cria_zero_carnaval` — `app/core/tests/test_contract_feriado_fonte.py:55-74`. |
| T2 | Mesmo registro, valores anteriores → valores submetidos dos 6 campos. (`app/core/views_config.py:61-70`) | `_requer_admin`, existencia por `fid`, POST e fonte preenchida. (`app/core/views_config.py:42-50`) | **Nascente:** rota editar → `form_feriado`. (`app/core/urls.py:43`; `app/core/views_config.py:41`) | Atribuicoes e `feriado.save()`. (`app/core/views_config.py:64-70`) | Atualiza nome/data/abrangencia/UF/municipio/fonte; solicita trilha `editar` com antes/depois. (`app/core/views_config.py:61-80`) | Lista e alcance. (`app/core/views_config.py:24`; `app/core/feriados.py:57-83`) Signal captura a data antiga e entrega datas antiga/nova ao servico de futuras. (`app/core/signals.py:14-36`) | `app/core/tests/test_smoke_clique_feriado_form.py::test_o_clique_no_Salvar_do_editar_muda_o_feriado_e_nao_cria_outro` — `app/core/tests/test_smoke_clique_feriado_form.py:135-143`. |
| T3 | Registro legado com `fonte=''` → fonte carimbada pela migracao. (`app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:29-35`) | Pula fonte preenchida; distingue nacional das demais abrangencias; nacional consulta mapa de datas ou recebe texto de ausencia de base. (`app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:29-34`) | **Nascente historica:** `Migration.operations` → `RunPython(carimbar_fonte, desfazer)`. (`app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:58`) | Modelo historico via `apps.get_model`; atribuicao e `save(update_fields=['fonte'])`. (`app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:27-35`) | Somente `fonte`: referencia cadastrada ou declaracao textual de pendencia. Sem chamada de auditoria nesse corpo. Reverso `desfazer` e `pass`. (`app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:17`, `:20-39`) | Lista exibe fonte. (`app/templates/core/config/feriados.html:48`) O predicado de alcance nao consulta fonte. (`app/core/feriados.py:57-67`) | nenhum |
| T4 | `Feriado` cadastrado → registro removido fisicamente. (`app/core/views_config.py:188-201`) | `_requer_admin`, POST e existencia do registro. (`app/core/views_config.py:186-188`) | **Nascente:** rota excluir → `excluir_feriado`. (`app/core/urls.py:44`; `app/core/views_config.py:185`) | `feriado.delete()`. (`app/core/views_config.py:201`) | Solicita trilha `excluir` antes do delete; motivo opcional; retrato anterior inclui nome/data/abrangencia/UF/municipio, sem fonte. (`app/core/views_config.py:193-200`) | `post_delete` entrega a data ao servico de futuras. (`app/core/signals.py:39-43`) Consultas seguintes leem o cadastro restante. (`app/core/feriados.py:27-29`) | `app/core/tests/test_smoke_clique_excluir_feriado.py::test_o_clique_em_Excluir_confirmado_na_caixa_apaga_o_feriado_com_trilha` — `app/core/tests/test_smoke_clique_excluir_feriado.py:110-126`. |

A trilha solicitada pelo CRUD passa por `registrar_log` e chega a `LogAuditoria`; o adaptador captura excecoes, portanto a chamada nao garante que a trilha persistiu. (`app/core/auditoria.py:8-29`; `app/core/observ.py:133-139`)

### Percurso do calendario: leitura, oferta, acao e competencia

- **Feriado nacional/local/praca:** `alcanca` decide por abrangencia, UF e municipio normalizado; sem municipio, somente nacional alcanca. `feriados_do_colab` compoe esse alcance com o municipio do posto do vinculo que responde pela data. Isso seleciona dados para leitura, sem alterar `Feriado`. (`app/core/feriados.py:32-67`, `:101-105`, `:124-166`)
- **Oferta de “Resolver dia”:** exige ultimo recurso, data passada e status em `falta|irregular|aberto|sem_certidao`. Ultimo recurso depende do inicio da competencia atual e da ausencia de pergunta viva respondida. O resultado e atribuido ao dicionario da tela. (`app/colaboradores/services/calendario.py:94-104`, `:463-477`, `:738-739`, `:769`)
- **Oferta versus permissao:** o template exige `pode_veredito` e `dia.resolver_dia_oferecido`; o predicado da oferta declara que permissao pertence ao observador. (`app/templates/colaboradores/partials/_calendario_grade.html:131-132`; `app/colaboradores/services/calendario.py:100-104`)
- **Resolver efetivamente:** o POST entra em `veredito_celula_view`, verifica metodo/permissao, datas e veredito, e despacha ao servico; esse corpo nao consulta novamente `resolver_dia_oferecido`. O cruzamento com a entidade que recebe a decisao fica no sitio `app/ponto/services/veredito_celula.py:211`. (`app/ponto/views.py:3352-3381`)
- **Competencia do calendario:** `janela_fechamento` usa corte da empresa, fallback 21 e limite 2–28. O calendario consulta a janela uma vez, usa-a em `modo=competencia` e entrega o rotulo pronto; as setas preservam esse modo. Nao grava competencia. (`app/ponto/janelas.py:10-21`, `:36-57`; `app/colaboradores/services/calendario.py:175-180`, `:794-810`; `app/templates/colaboradores/calendario_partial.html:37-41`)

**Nascentes de leitura localizadas:** perfil, drawer, relatorio e porta plural chamam `contexto_calendario`; o vigia consome a plural pelos seus dois censos. (`app/colaboradores/views.py:1949`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:160`; `app/colaboradores/services/calendario.py:132`; `app/chamados/services/vigia_fio.py:271`, `:429`, `:641`)

**Commands e crons que chegam a essa leitura:** `vigiar_fio_morto`, `backfill_fio_morto_e4b` e `materializar_passivo_validado` chamam os censos; o primeiro participa de encadeamento de cron. Esses sitios nao acrescentam escritor de Feriado ou Calendario. (`app/chamados/management/commands/vigiar_fio_morto.py:180-181`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:78`; `app/chamados/management/commands/materializar_passivo_validado.py:206`; `app/config/crons.py:1736-1738`)

**Demais commands com feriado no nome, discriminados por destino:** `reavaliar_ausencias_feriado` e vigia sem escrita, agendado as 07:02 com `--alarme`; `marcar_feriado_do_posto` escreve cadastro de Posto; `sincronizar_feriado_admin` chega ao escritor do vinculo. Os dois ultimos constam como atos humanos/sob ordem. Sao cruzamentos, sem mapa dessas outras entidades aqui. (`app/ponto/management/commands/reavaliar_ausencias_feriado.py:35-46`; `app/config/crons.py:608`, `:1346-1351`, `:1421`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:131-137`; `app/colaboradores/management/commands/sincronizar_feriado_admin.py:53-56`)

**Efeitos dos eventos de Feriado sobre outras entidades:** os sitios de saida sao `app/ponto/services/cadastro_futuro.py:49-79` e `app/chamados/signals.py:326-337`. O primeiro recebe datas antiga/nova no save e a data removida no delete; o segundo recebe apenas a data atual no save e declara nao tratar delete. (`app/core/signals.py:35-43`; `app/chamados/signals.py:322-337`)

**Provas do calendario, sem converter projecoes em transicoes:**

- Oferta: `app/chamados/tests/test_o261_vigia_fio.py::test_MORDE_cada_perna_do_predicado`, em `app/chamados/tests/test_o261_vigia_fio.py:930-934`; leitura pelo template: `app/chamados/tests/test_o261_vigia_fio.py::test_o_template_LE_o_juiz_e_nao_tem_segunda_copia`, em `app/chamados/tests/test_o261_vigia_fio.py:943-952`.
- Paridade da porta plural: `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py::test_RED_porta_do_vigia_le_o_mesmo`, em `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:79-87`.
- Competencia: `app/colaboradores/tests/test_ui_cal_competencia.py::test_RED_as_setas_nao_perdem_o_modo`, em `app/colaboradores/tests/test_ui_cal_competencia.py:135-144`; `app/colaboradores/tests/test_ui_cal_competencia.py::test_MORDE_o_template_NAO_deriva_a_janela`, em `app/colaboradores/tests/test_ui_cal_competencia.py:147-167`.

## 3. TRES LISTAS

### L1 TRANSICAO COM MAIS DE UM JUIZ — 4

Contagem literal de transicoes condicionadas por mais de uma funcao/predicado, incluindo permissao, metodo e existencia; essa contagem nao significa quatro conflitos entre regras concorrentes. Os gates correspondentes estao nos sitios abaixo. (`app/core/views_config.py:42-50`, `:136-147`, `:158`, `:186-188`; `app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:29-34`)

1. **T1 — nascimento:** manual combina `_requer_admin`, POST e fonte; importacao combina permissao/POST, data parseavel, allowlist e `get_or_create`. Sao duas admissoes distintas para o mesmo nascimento cadastral. (`app/core/views_config.py:12-13`, `:42-50`, `:136-147`, `:158`)
2. **T2 — edicao:** autorizacao, registro identificado, POST e fonte preenchida. (`app/core/views_config.py:42-50`)
3. **T3 — carimbo historico:** fonte ainda vazia, abrangencia nacional ou local e consulta ao mapa de datas no ramo nacional. (`app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:29-34`)
4. **T4 — exclusao:** autorizacao, POST e registro existente. (`app/core/views_config.py:186-188`)

A composicao da oferta do calendario nao aumenta essa contagem: ela produz um booleano em memoria, sem transicao persistida propria. (`app/colaboradores/services/calendario.py:103-104`, `:769`)

### L2 ESCRITOR FORA DA PORTA — 0 desvios demonstrados

- As cinco escritas localizadas estao dentro das operacoes identificadas na matriz: `form_feriado`, `_importar_do_payload`, `carimbar_fonte` e `excluir_feriado`. O modelo nao declara um escritor unico de estado que essas operacoes estejam contornando. (`app/core/models.py:5-31`; `app/core/views_config.py:41-94`, `:127-152`, `:185-201`; `app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:20-35`)
- Existe **1 escritor historico fora do CRUD runtime**, o backfill de fonte; ele foi incluido como porta propria T3, sem transforma-lo em violacao de uma porta unica inexistente. (`app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:20-35`, `:58`)
- O selo de consulta ao modelo **nao prova escritor unico**: isenta todo `views_config.py` e exclui migrations da varredura. (`app/core/tests/test_contract_feriado.py:49-64`)

### L3 ESTADO ALCANCAVEL SEM SAIDA — 0 demonstrados

- Registro nascido pelo cadastro ou importacao pode ser editado ou excluido; fonte carimbada tambem pode ser substituida pela edicao. (`app/core/views_config.py:83-86`, `:144-147`, `:64-70`, `:201`)
- Exclusao elimina o registro; nao grava um estado terminal. O modelo nao declara terminal, e nascimento continua disponivel para a chave cadastral. (`app/core/models.py:5-31`; `app/core/views_config.py:83`, `:201`)
- O reverso vazio da migracao nao torna a fonte terminal: a porta de edicao continua escrevendo esse campo. (`app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:38-39`; `app/core/views_config.py:69-70`)
- Oferta e modo do calendario sao recalculados por leitura; nao constituem estados persistidos sem saida. (`app/colaboradores/services/calendario.py:168-212`, `:769`, `:804`)

## 4. NAO LIDO OU INCERTO

- Correspondencia independente desta copia com o hash informado: **[nao verificado]**; a arvore disponibilizada nao permitiu confirmar o commit pelo Git.
- Quantidades, ocorrencias, execucoes de cron e trilhas efetivamente existentes em producao para Feriado/Calendario: **[nao verificado]**. O dado raspado fornecido nao apresenta essa populacao.
- Testes citados foram lidos; resultado de execucao nesta analise: **[nao verificado]**. Nao foi localizada prova especifica do carimbo historico T3: **[nao verificado]**.
- Quando a consulta de janela/perguntas do ultimo recurso falha, o calendario captura a excecao depois de inicializar valores de fallback; frequencia e efeito ocorrido em producao: **[nao verificado]**. (`app/colaboradores/services/calendario.py:459-479`)
- A fonte e exigida na porta manual, mas o campo aceita vazio no modelo e o alcance territorial nao a consulta. Qualidade das fontes existentes e consequencias concretas em producao: **[nao verificado]**. (`app/core/views_config.py:50-54`; `app/core/models.py:19-22`; `app/core/feriados.py:57-67`)
- Escritas externas a esta copia, comandos ad hoc e SQL executado fora dos sitios lidos: **[nao verificado]**.