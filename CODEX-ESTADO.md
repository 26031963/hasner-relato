# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 16:39** · publicado em **11/11/2026 16:39** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 16:39 · sessao 07260de7f099 · - · - · origem=Ronald (tmux)

A cobertura nova é útil, mas não demonstra que cada caso depende do braço novo:

- `doc_md` e `doc_md_cortes` deixam de passar se o matcher curinga for desligado; os negativos cobrem subpasta, extensão, caixa e pasta diferente. A fixture contém apenas `app/docs/*.md`, portanto não exercita padrões com prefixo ou vários `*` (`bin/tests/test_selo_proporcao.sh:27`, `:87–93`; implementação `bin/selo_proporcao.py:98–107`).
- `pwa_excecao_e_lista` prova EXCECAO antes da LISTA porque a fixture também lista o PWA; `sw_excecao`, mistura com B, apagado e rename exercitam exceção antes de static (`bin/tests/test_selo_proporcao.sh:27`, `:94–98`; `bin/selo_proporcao.py:113–124`).
- `pwa_outra_pasta` é controle nominal: passa mesmo removendo a exceção. `pwa_declarada_A` também passa sem exceção, pois só verifica rc 0 e uma declaração A recebe rc 0 tanto para classificação real A quanto B (`bin/tests/test_selo_proporcao.sh:49`, `:99–100`; `bin/selo_proporcao.py:168–174`).
- O caso chamado `pwa_renomeado` realmente renomeia `sw.js`, não `pwa.js` (`bin/tests/test_selo_proporcao.sh:98`).
- Os novos casos de bloco usam LISTA válida e verificam rc 2 **e motivo**, evitando que ausência de outro bloco esconda a guarda pretendida (`bin/tests/test_selo_proporcao.sh:51–59`, `:134–144`).
- Falta `?` na pasta: há `*` na pasta, `[` na pasta, e `?` no nome. Remover apenas a proibição de `?` na pasta não é coberto por esses casos (`bin/tests/test_selo_proporcao.sh:141–144`; guarda `bin/selo_proporcao.py:90`).
- Não há caso independente de EXCECAO alterada só no HEAD/disco: os testes correspondentes adulteram apenas LISTA; os testes de bloco alteram BASE e o HEAD herda esse conteúdo. Ler somente a EXCECAO do HEAD pode sobreviver por essa ausência — inferência estática (`bin/tests/test_selo_proporcao.sh:51–58`, `:109–110`, `:162–166`).
- A exceção da fixture só contém caminhos static, embora a função aceite uma exceção alternativa; não há exercício nominal sob templates/tests (`bin/tests/test_selo_proporcao.sh:18–28`; precedência implementada em `bin/selo_proporcao.py:113–125`).

Não executei o harness porque ele cria e remove arquivos (`bin/tests/test_selo_proporcao.sh:14–32`). A execução alegada de 53/53 mutantes e 32 RED permanece `[nao verificado]`; os arquivos de resultados não constam da cópia disponível `[nao verificado]`.

## 11/11 16:38 · sessao be19b9bab978 · - · - · origem=Ronald (tmux)

Na cópia, o SW tem fonte nominal correta na EXCECAO; o caminho efetivamente aberto depende do runtime. (`app/docs/CORTES.md:445`; `app/core/views.py:528-530`.)

- As duas cascas carregam `pwa.js`, que registra `/sw.js` com escopo `/`. (`app/templates/base.html:20`; `app/templates/base_app.html:20`; `app/static/pwa.js:14`.)
- A URL raiz inclui `core.urls`; `/sw.js` chama `views.service_worker`. (`app/config/urls.py:39`; `app/core/urls.py:24`.)
- A view não renderiza template: abre `sw.js`, substitui `__BUILD_ID__` e responde JavaScript. (`app/core/views.py:570-573`.)
- `_raiz_do_static()` prefere `/app/staticfiles/sw.js` quando existe; caso contrário usa `/app/static/sw.js`. A fonte versionada é `app/static/sw.js`, incluída nominalmente em `CORTES.md`. Conteúdo e ramo efetivamente servido em produção: `[nao verificado]`. (`app/core/views.py:528-530`; `app/docs/CORTES.md:445`.)
- A configuração registra `BASE_DIR/static` como fonte de static e `BASE_DIR/staticfiles` como destino coletado; `staticfiles/` é ignorado pelo Git. (`app/config/settings/base.py:98-99`; `.gitignore:13`.)
- Mesmo um caminho de diff literal `app/staticfiles/sw.js` seria A pelo restante das regras: não tem componente `static`, não está na LISTA e cai no retorno final. Portanto, a diferença entre caminho fonte e coletado não demonstra B indevido. (`bin/selo_proporcao.py:98-107`; `bin/selo_proporcao.py:110-127`.)
- O código confirma escrita offline no PWA: `registrarBatidaOffline` abre transação de escrita e adiciona batida; sincronização remove itens enviados. O SW delega sincronização à página por mensagem, ou pede ao usuário para abrir o app. (`app/static/pwa.js:107-113`; `app/static/pwa.js:161-172`; `app/static/sw.js:112-127`.)

Escritor estrutural de `CORTES.md`: `bin/gerar_cortes.py` lê `app/docs/CORTES.json`, transforma cada `frase` em linha Markdown e reescreve o destino inteiro; a execução normal chama esse escritor. (`bin/gerar_cortes.py:23-24`; `bin/gerar_cortes.py:31-32`; `bin/gerar_cortes.py:106-109`; `bin/gerar_cortes.py:114-116`; `bin/gerar_cortes.py:142-152`.)

Os blocos atuais estão em `CORTES.md:444-445`, mas não há correspondentes PROPORCAO/LISTA-FECHADA/EXCECAO em `CORTES.json` na cópia `[nao verificado: ausência sem linha positiva única]`; o gerador não preserva apêndices manuais, então regeneração apagaria esses blocos. Esse é o limite (c) já declarado no anexo, não um achado fora do contrato. (`bin/gerar_cortes.py:97-116`; `app/docs/CORTES.md:3`; anexo `v10_contrato.txt`, item 4(c), sem arquivo local: `[nao verificado]`.)

## 11/11 16:05 · sessao 91788de74858 · PROPORCAO-POR-RISCO · verificador · origem=Code

APROVADO

Bloqueadores: nenhum novo demonstrado na cópia. O bloqueante 1 da v8 está curado pela guarda em `bin/selo_proporcao.py:80-81`.

1. **Cura e precedência da LISTA**

   `templatetags` agora é procurado em **todos os componentes de diretório**, antes dos braços `templates`, `static` e `tests`. Portanto, os dois arquivos do contraexemplo da v8 recebem `None`, tornam a obra A e produzem rc 1 quando declarada B (`bin/selo_proporcao.py:74-87`, `bin/selo_proporcao.py:123-135`).

   **Sim, `app/chamados/templatetags/chamados_tags.py` continua B pela LISTA**: consta em `app/docs/CORTES.md:444`, e a consulta retorna `'lista'` antes da guarda nova (`bin/selo_proporcao.py:76-80`). A execução usa a lista da **BASE**; sua presença naquela revisão histórica é [nao verificado], pois a cópia disponibilizada não permite resolver as revisões Git.

2. **Outras descobertas por pasta**

   Não encontrei outro contraexemplo concreto de descoberta automática na cópia [nao verificado quanto à ausência global e à execução em produção].

   - **Admin/apps:** a configuração padrão aponta para `saas`, que declara aplicativos de topo; não declara aplicativos sob `templates/static/tests` (`app/config/wsgi.py:14`, `app/config/settings/saas.py:22-53`). Caminhos como `app/core/admin.py` e `app/core/apps.py` recebem A pelas regras finais do classificador (`bin/selo_proporcao.py:82-88`).
   - **Signals/inicialização:** os imports encontrados são explícitos, por exemplo `core.signals`, `core.cell` e `chamados.signals`; esses caminhos também recebem A (`app/core/apps.py:8-10`, `app/chamados/apps.py:8-10`, `bin/selo_proporcao.py:82-88`).
   - **Checks, locale e fixtures:** internacionalização está ligada (`app/config/settings/base.py:92-95`). Não encontrei configuração ou caminho concreto que demonstre outra entrada automática classificada B; ausência global e comportamento interno desses carregadores nesta instalação: [nao verificado].
   - **Templatetags em outro nível:** a guarda usa pertinência em `dirs`, sem restringir profundidade; qualquer componente `templatetags` é barrado, salvo LISTA (`bin/selo_proporcao.py:74-81`).
   - **Limite anterior permanece:** o comando importa `core.tests.test_contract_tblib_no_paralelo`, que continua B pelo braço de teste. É o limite já declarado, com import explícito, não uma nova descoberta por pasta (`app/chamados/management/commands/tblib_ausente_na_imagem.py:17-18`, `bin/selo_proporcao.py:29-30`, `bin/selo_proporcao.py:86-87`).

3. **Os casos novos mordem o braço**

   Sim. Nenhum está na lista sintética, fora de `app/`, ou sob `migrations/management`; ambos dependem exclusivamente da condição nova (`bin/tests/test_selo_proporcao.sh:24`, `bin/tests/test_selo_proporcao.sh:70-71`, `bin/selo_proporcao.py:76-85`).

   | Caso | v9 | Removendo somente o braço novo |
   |---|---|---|
   | `tags_static_py` | Arquivos A; rc 1 | Arquivos B por `static`; rc 0 |
   | `tags_templates_py` | Arquivo A; rc 1 | Arquivo B por `templates`; rc 0 |

   Esses resultados seguem de `bin/selo_proporcao.py:80-85` e `bin/selo_proporcao.py:127-135`. Logo, **os testes passam na v9 e falham no mutante**, porque esperam rc 1 (`bin/tests/test_selo_proporcao.sh:44-45`, `bin/tests/test_selo_proporcao.sh:70-71`).

   Ressalva: `tags_templates_py` não cria `templates/__init__.py`; comprova o predicado de caminho, mas sua importação recursiva efetiva pelo Django é [nao verificado] (`bin/tests/test_selo_proporcao.sh:71`).

4. **Quem escreve e quem lê**

   | Valor | Quem produz/escreve | Quem lê |
   |---|---|---|
   | Caminho `p` | `git diff`, decodificação e laço: `bin/selo_proporcao.py:112-125` | `motivo_b`: `bin/selo_proporcao.py:73-74` |
   | `partes`, `dirs`, `nome` | Divisão/fatiamento: `bin/selo_proporcao.py:74-75` | Guarda nova e braços seguintes: `bin/selo_proporcao.py:80-87` |
   | LISTA real | Conteúdo atual: `app/docs/CORTES.md:444`; autor da inserção [nao verificado] | `git show BASE`, parser e consulta: `bin/selo_proporcao.py:62-76`, `bin/selo_proporcao.py:111` |
   | Arquivo `CORTES.md` | Gerador lê JSON e sobrescreve Markdown: `bin/gerar_cortes.py:23-24`, `bin/gerar_cortes.py:32`, `bin/gerar_cortes.py:116` | Selo lê versão BASE: `bin/selo_proporcao.py:62` |
   | LISTA sintética | Função `lista`, argumento e commit: `bin/tests/test_selo_proporcao.sh:19-28` | Mesmo parser: `bin/selo_proporcao.py:62-70` |
   | Arquivos/conteúdos novos | Scripts dos casos, executados e commitados pelo preparo: `bin/tests/test_selo_proporcao.sh:36-38`, `bin/tests/test_selo_proporcao.sh:70-71` | Selo lê **nomes**; conteúdo não entra no predicado: `bin/selo_proporcao.py:112`, `bin/selo_proporcao.py:73-88` |
   | Motivo/`None`, `fora`, classe e rc | Classificador e agregação: `bin/selo_proporcao.py:76-88`, `bin/selo_proporcao.py:123-135` | Protocolo e wrapper: `bin/selo_proporcao.py:145`, `bin/selo_proporcao.sh:7-11` |
   | Esperado `B 1`, rc observado e falhas | Casos, captura e contador: `bin/tests/test_selo_proporcao.sh:33`, `bin/tests/test_selo_proporcao.sh:44`, `bin/tests/test_selo_proporcao.sh:70-71` | Comparação e resultado final: `bin/tests/test_selo_proporcao.sh:45`, `bin/tests/test_selo_proporcao.sh:203-204` |

   O harness completo não foi executado: cria arquivos e repositório temporário, contrariando a restrição de somente leitura (`bin/tests/test_selo_proporcao.sh:13-28`). Execução integral e identidade Git do HEAD informado: [nao verificado].

## 11/11 16:04 · sessao bb6c2e8c42d3 · - · - · origem=Ronald (tmux)

Os dois casos isolam o braço novo; nenhum recebe rc 1 por outra guarda: pertencem a `app/`, não contêm `migrations`/`management` e não estão na lista sintética (`bin/tests/test_selo_proporcao.sh:24`, `:70-71`; `bin/selo_proporcao.py:76-85`).

Verificação somente em memória da função `motivo_b`, removendo exclusivamente `or 'templatetags' in dirs`:

| Arquivo sintético | v9 | Sem braço |
|---|---|---|
| `app/x/templatetags/static/__init__.py` | `None` | `static` |
| `app/x/templatetags/static/carga.py` | `None` | `static` |
| `app/x/templatetags/templates/carga.py` | `None` | `templates` |

Os resultados decorrem de `bin/selo_proporcao.py:80-85`. Com v9, ambos os **testes** passam porque o selo retorna 1, esperado pelo harness; sem o braço, os **arquivos** passam como B/rc 0 e os testes falham (`bin/tests/test_selo_proporcao.sh:44-45`, `:70-71`; `bin/selo_proporcao.py:127-135`).

`tags_templates_py` não cria `templates/__init__.py`, ao contrário do caso static; portanto é caso do predicado de caminho, sem demonstração suficiente de importação recursiva efetiva pelo Django (`bin/tests/test_selo_proporcao.sh:70-71`). Importação efetiva desse exemplo: [nao verificado].

A lista mantém precedência: `app/chamados/templatetags/chamados_tags.py` aparece em `app/docs/CORTES.md:444`; `motivo_b` devolve `'lista'` antes da guarda de templatetags (`bin/selo_proporcao.py:76-80`). A lista usada numa execução real é a revisão BASE, não necessariamente o arquivo atual (`bin/selo_proporcao.py:62`, `:111`).

Cadeia de escritores e leitores:

| Valor | Quem escreve/produz | Quem lê |
|---|---|---|
| Caminhos alterados `p` | Git diff, `bin/selo_proporcao.py:112`; decodificação `:117` | Laço `:124-125`; `motivo_b` `:74` |
| `partes`, `dirs`, `nome` | Split/fatiamento, `bin/selo_proporcao.py:74-75` | Guarda nova `:80`; regras `:82-87` |
| Lista real | Conteúdo em `app/docs/CORTES.md:444`; autor da inserção [nao verificado] | `git show BASE`, `bin/selo_proporcao.py:62`; parser `:63-70`; consulta `:76` |
| Arquivo `CORTES.md` | Gerador sobrescreve em `bin/gerar_cortes.py:116`, fonte JSON `:23`, lida em `:32` | Selo lê versão BASE em `bin/selo_proporcao.py:62` |
| Lista sintética | `bin/tests/test_selo_proporcao.sh:19`, argumento `:24`, commit `:28` | Mesmo parser do selo, `bin/selo_proporcao.py:62-70` |
| Arquivos e bytes dos novos casos | Scripts `bin/tests/test_selo_proporcao.sh:70-71`, executados por `:36`, versionados `:37-38` | Selo lê somente nomes, `bin/selo_proporcao.py:112`; conteúdo não entra no predicado `:73-88` |
| `m`/`None` | `bin/selo_proporcao.py:77-88` | Impressão/acumulação `:125-128` |
| `fora`, `real`, rc | `bin/selo_proporcao.py:123-135` | Protocolo `:145`; wrapper `bin/selo_proporcao.sh:7-11`; harness `bin/tests/test_selo_proporcao.sh:44-45` |
| Esperado `B 1` e nomes dos casos | `bin/tests/test_selo_proporcao.sh:70-71` | Argumentos do `caso` `:41-45` |
| Contador de falhas | `bin/tests/test_selo_proporcao.sh:33` | Resultado final `:203-204` |

O harness completo não foi executado: cria diretório temporário, repositório e arquivos (`bin/tests/test_selo_proporcao.sh:13-28`), incompatíveis com a exigência de somente leitura.

## 11/11 16:04 · sessao bf5e6ffaae43 · - · - · origem=Ronald (tmux)

Não encontrei outro bloqueante concreto de descoberta automática no material examinado [nao verificado].

- A configuração padrão aponta para `config.settings.saas`; ela herda `base` e monta os aplicativos de nomes de topo, sem aplicativos declarados sob `templates/static/tests`: `app/config/wsgi.py:14`, `app/manage.py:9`, `app/config/settings/saas.py:10`, `app/config/settings/saas.py:22`, `app/config/settings/saas.py:34`, `app/config/settings/saas.py:53`.
- Os imports de inicialização encontrados são explícitos: `signals`, `cell`, `painel_cell`, `calculadores`; seus caminhos não recebem B pelas regras automáticas: `app/core/apps.py:8`, `app/chamados/apps.py:8`, `app/colaboradores/apps.py:7`, `app/ponto/apps.py:7`, `app/escala/apps.py:8`, `app/inteligencia/apps.py:7`, `bin/selo_proporcao.py:82`.
- Templates e estáticos têm descoberta configurada, já contemplada pela letra do contrato: `app/config/settings/base.py:70`, `app/config/settings/base.py:97`; internacionalização está ligada: `app/config/settings/base.py:92`.
- O import de código B de teste por comando permanece o limite declarado de tblib: escritor da chamada em `app/chamados/management/commands/tblib_ausente_na_imagem.py:17`, uso do valor em `app/chamados/management/commands/tblib_ausente_na_imagem.py:18`.
- `crons` importa `core.tests.marca_cron15`, cujo nome não satisfaz `test_*.py`: `app/core/management/commands/crons.py:92`, `bin/selo_proporcao.py:86`.
- Não apareceram `.py` sob `app/**/templates/` ou `app/**/static/`, nem caminhos `fixtures/` ou `locale/` no inventário pesquisado; ausência global e configuração efetivamente usada em produção: [nao verificado].
- Não há código vendorizado do Django para confirmar nesta cópia a semântica interna de autodiscover de admin, apps, checks, locale ou fixtures [nao verificado].

## 11/11 15:59 · sessao c5d9a24422ee · FIO-E0-V4-E4B · construtor · origem=Code

Implementada a ordem **DP → âncoras → exists → escrever**, preservando as advisories ([fecho_runtime.py:122](chamados/services/fecho_runtime.py:122)).

Resultados locais: RED na base c8 e mutantes morreram nas assertions previstas; 123 testes e Ruff verdes. Comandos, evidências e fila estão no [recibo c9](docs/FIO-E0-V4-E4B-c9.txt:33).

ORM e certificação Code/opus: [nao verificado].

## 11/11 15:56 · sessao 111529e65077 · UI-PAINEL-MOSTRA-O-APP · verificador · origem=Code

REPROVADO

1. **Bloqueante (1) curado no código; censo do diff completo [não verificado].** O template usa `pendentes_com_dia` e apresenta `dia`, com “—” para valor vazio; deixou de interpretar `p.data_evento` como dia do turno (`app/templates/colaboradores/partials/painel_colaborador.html:195–198`). O juiz retorna `None` para conversa (`app/chamados/juizes.py:334–338`).

   Há outros leitores na cópia:
   
   - API: serialização direta de `p.data_evento` em `app/api/views.py:1153–1154`; interpolação desse campo numa mensagem de diagnóstico em `app/api/views.py:2625`.
   - Serviço: comparação direta, excluindo conversa, em `app/chamados/services/pergunta_no_app.py:149`.
   - Teste alterado pela v5: consulta ao juiz em `app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:129–131`; leitura crua para conferir o fato e rejeitar a data civil da conversa em `app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:175–178`.

   Quais desses leitores foram introduzidos ou tocados pela obra inteira contra `e70a54f7e` está **[não verificado]**: o anexo contém somente o delta v5, e esta cópia não disponibiliza histórico Git.

2. **Sem novo bloqueio na montagem dos pares.** A compreensão percorre exatamente `consulta['pendentes']`, preservando objetos e ordem, sem filtro, ordenação ou regra própria de dia, e chama `_dia_de_turno_da_pergunta(p)` uma vez por elemento nessa montagem (`app/chamados/services/pergunta_no_app.py:106`). A lista de pares é nova; a lista original de pendentes continua sendo devolvida pela expansão de `consulta` (`app/chamados/services/pergunta_no_app.py:107–109`).

   O universo vem de C6, ordenado por ID, depois separado pelo estado de resposta e fechamento da disputa (`app/chamados/services/pergunta_no_app.py:49–58`, `:69–75`). A chamada única aqui não significa uma única chamada total ao juiz durante toda a consulta: C6 também o utiliza (`app/chamados/juizes.py:1448`).

   Precisão sobre “registrado”: o registro declara C6 como `chamados/juizes.py::pergunta_viva`; `_dia_de_turno_da_pergunta` é o auxiliar existente dessa autoridade (`app/core/juizes.py:278`; `app/chamados/juizes.py:334–338`, `:1448`).

3. **Escritores e leitores: mesma origem para rótulo e contagem.** A projeção é chamada pelos dois `/me/` e pela view do painel (`app/api/views.py:315–319`; `app/api/views_core.py:303–307`; `app/colaboradores/views.py:940–941`). A view passa o resultado como `no_app` ao template (`app/colaboradores/views.py:1006`).

   | Chave | Quem escreve | Quem lê em execução normal |
   |---|---|---|
   | `pendentes` | `consulta_do_app`, `app/chamados/services/pergunta_no_app.py:69–75` | Projeção para tamanho e pares, `:91`, `:106`; condição do template, `app/templates/colaboradores/partials/painel_colaborador.html:193` |
   | `historico` | `consulta_do_app`, `app/chamados/services/pergunta_no_app.py:69–75` | Preservado pela projeção, `:107`; sem consumo nos três leitores acima |
   | `pendentes_com_dia` | Projeção, `app/chamados/services/pergunta_no_app.py:106–109` | Template, `app/templates/colaboradores/partials/painel_colaborador.html:195–198` |
   | `questionarios_pendentes` | Projeção, `app/chamados/services/pergunta_no_app.py:91` | APIs, `app/api/views.py:318`, `app/api/views_core.py:306`; contador da aba, `app/templates/colaboradores/partials/painel_colaborador.html:39` |
   | `chamados_nao_lidos` | Projeção, `app/chamados/services/pergunta_no_app.py:92–97` | Regra do badge, `:100–101`; APIs, `app/api/views.py:317`, `app/api/views_core.py:305` |
   | `chamados_badge`, incluindo `rotulo`, `count`, `cor` | Projeção, `app/chamados/services/pergunta_no_app.py:98–103` | APIs, `app/api/views.py:319`, `app/api/views_core.py:307`; faixa, `app/templates/colaboradores/partials/painel_colaborador.html:190–191` |

   Os dois `/me/` publicam os mesmos campos recebidos da projeção; `pendentes_com_dia` não entra no payload (`app/api/views.py:358–360`; `app/api/views_core.py:346–348`). O contador do **botão** soma chamados e perguntas; a **faixa** usa `badge.count` (`app/templates/colaboradores/partials/painel_colaborador.html:39–40`, `:191`).

   Nos testes, existe ainda um leitor auxiliar para renderização (`app/colaboradores/tests/test_selo_situacao_e_lampada.py:118–122`) e uma sobrescrita deliberada do badge para provar propagação pelos três leitores (`app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:219–227`).

4. **BLOQUEANTE — o RED ORM pelo motivo certo continua sem comprovação disponível.** Pela leitura do teste, ambos os mutantes devem falhar: template usando `p.data_evento` ou projeção entregando essa data para conversa produzirão a data civil onde a asserção exige “Dia: —”; há também rejeição explícita da data civil (`app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:166–178`).

   A mordida **executada no ORM**, o verde atual e o RED sobre **as duas bases** estão **[não verificado]**. O registro existente descreve RED de memória na base `9ca563bb3` e ausência de execução ORM naquela ocasião (`app/colaboradores/tests/test_ui_painel_mostra_o_app.py:6–11`). Ele não comprova a alegada repetição v5.

   A verificação disponível aqui foi de memória: sete testes verdes, e variantes dos dois defeitos morreram por asserção, sem erro. Os contratos correspondentes estão em `app/colaboradores/tests/test_ui_painel_mostra_o_app.py:60–70`, `:74–110`; o próprio arquivo limita esse selo, sem certificação ORM (`:3–4`).

5. **FILA — fechamento de escopo e relato.** Há adaptações auxiliares visíveis: alvos dos mutantes E3 apontam para a projeção (`app/chamados/tests/e3_mutantes.py:23–26`, `:87–88`), o censo declara seu contador (`app/core/tests/test_tripwire_contagem_de_pendencia.py:82–85`) e outro selo fornece `no_app` ao template (`app/colaboradores/tests/test_selo_situacao_e_lampada.py:116–122`).

   A correspondência dessas alterações com o **diff completo** e com o **texto do commit**, assim como smoke do Ronald, encaixe na raia e entrega da linha HAIKU, permanece **[não verificado]**. Esses materiais não foram fornecidos; não há base para declarar que o commit os cumpriu ou omitiu. [nao verificado]

## 11/11 15:49 · sessao 09a259e1349b · DATA-EVENTO-CRU · verificador · origem=Code

**Veredito: BLOQUEANTE — a sobrecontagem pode encobrir um leitor novo no mesmo escopo.** Reproduzido em memória, preservando a escrita e o retorno da função. (`app/chamados/services/fio.py:800`; `app/chamados/services/fio.py:801`; `app/chamados/tests/test_catraca_data_evento_cru.py:189`)

1. **Bloqueantes 3 e 5 da v2 e censo.**

   **3:** os quatro repasses de escrita foram corrigidos pela inclusão de `reapontar_resolvedora` em `ESCRITA`. O quinto caso, `return 'data_evento'`, continua contado e foi declarado sobrecontagem admitida. (`app/chamados/tests/test_catraca_data_evento_cru.py:64`; `app/chamados/tests/test_catraca_data_evento_cru.py:24`; `app/chamados/services/fio.py:801`)

   **5:** corrigido. `M.objects.create(x=p['data_evento'])` agora conta **1**, pois o subscrito `Load` restaura a contagem da chave mesmo dentro da escrita. (`app/chamados/tests/test_catraca_data_evento_cru.py:119`; `app/chamados/tests/test_catraca_data_evento_cru.py:123`; `app/chamados/tests/test_catraca_data_evento_cru.py:235`)

   Recalculei **212 ocorrências em 118 escopos: 207 Python + 5 templates**, com igualdade integral ao JSON. As únicas alterações frente à v2 reconstituída pelo delta são estas. (`app/chamados/tests/test_catraca_data_evento_cru.py:164`; `app/chamados/tests/catraca_data_evento_cru.json:2`)

   | Chave | v2 → v3 | Referência |
   |---|---:|---|
   | `chamados/reconciliador.py::reatar_pergunta_orfa` | 5 → 3 | `app/chamados/tests/catraca_data_evento_cru.json:47`; `app/chamados/reconciliador.py:1642`; `app/chamados/reconciliador.py:1655` |
   | `chamados/reconciliador.py::reemitir_chamado_do_dia` | 1 → ausente | `app/chamados/reconciliador.py:954`; `app/chamados/tests/test_catraca_data_evento_cru.py:64` |
   | `chamados/services/declaracao_texto.py::declarar_hora_colab` | 1 → ausente | `app/chamados/services/declaracao_texto.py:370`; `app/chamados/tests/test_catraca_data_evento_cru.py:64` |

2. **As duas portas novas escondem leitura real atual? Não encontrei.** Os quatro `data_evento=` de `reapontar_resolvedora` são pedidos de datação; a porta grava pelo escritor e salva. `rechavear_data_evento` recebe `novo_dia`, grava e salva. As leituras diretas existentes nas implementações continuam contadas. (`app/chamados/services/fio.py:876`; `app/chamados/services/fio.py:879`; `app/chamados/services/fio.py:927`; `app/chamados/services/fio.py:1037`; `app/chamados/services/fio.py:1043`; `app/chamados/services/fio.py:867`; `app/chamados/services/fio.py:934`; `app/chamados/services/fio.py:1013`; `app/chamados/tests/test_catraca_data_evento_cru.py:104`)

3. **Bloqueante: a sobrecontagem não é estável contra compensação.** Em memória, substituí somente o retorno de `gravar_dia_do_fato` por:

   ```python
   return {'data_evento': linha.data_evento}.popitem()[0]
   ```

   O retorno permanece `'data_evento'`, a atribuição permanece efetiva e surge **uma leitura real direta**. Entretanto, o escopo fica **1 → 1**, o censo completo permanece **212/118**, igual ao JSON, e `test_so_desce` passa alimentado com esse censo mutante. O literal antes contado vira chave de dict excluída; o atributo novo ocupa sua contagem. (`app/chamados/services/fio.py:800`; `app/chamados/services/fio.py:801`; `app/chamados/tests/test_catraca_data_evento_cru.py:104`; `app/chamados/tests/test_catraca_data_evento_cru.py:108`; `app/chamados/tests/test_catraca_data_evento_cru.py:110`; `app/chamados/tests/test_catraca_data_evento_cru.py:189`)

   Isso demonstra um **primeiro leitor real financiado por rótulo sobrecontado**, sem reflexão, lambda, alias ou dois saltos. O limite documentado de trocar leitor por leitor não cobre essa distinção. (`app/chamados/services/fio.py:791`; `app/chamados/tests/test_catraca_data_evento_cru.py:24`; `app/chamados/tests/test_catraca_data_evento_cru.py:37`)

4. **As duas formas novas mordem seus mutantes.** Retirar `reapontar_resolvedora` de `ESCRITA` faz surgir `escreve: 1` indevido e falha o teste. Remover o tratamento de subscrito `Load` reduz `escreve_lendo` de **3 para 2** e também falha. (`app/chamados/tests/test_catraca_data_evento_cru.py:64`; `app/chamados/tests/test_catraca_data_evento_cru.py:119`; `app/chamados/tests/test_catraca_data_evento_cru.py:227`; `app/chamados/tests/test_catraca_data_evento_cru.py:235`; `app/chamados/tests/test_catraca_data_evento_cru.py:237`)

**FILA:** retirar apenas `rechavear_data_evento` de `ESCRITA` sobrevive a `test_MORDE_cada_forma`; falta uma forma correspondente na fixture. É lacuna de cobertura, sem leitor real omitido demonstrado. (`app/chamados/tests/test_catraca_data_evento_cru.py:64`; `app/chamados/tests/test_catraca_data_evento_cru.py:219`; `app/chamados/tests/test_catraca_data_evento_cru.py:235`)

Os quatro métodos sem escrita passaram em memória com base `unittest`; o método que cria temporários não foi executado. Suíte Django: **[nao verificado]**. (`app/chamados/tests/test_catraca_data_evento_cru.py:185`; `app/chamados/tests/test_catraca_data_evento_cru.py:198`; `app/chamados/tests/test_catraca_data_evento_cru.py:203`; `app/chamados/tests/test_catraca_data_evento_cru.py:240`; `app/chamados/tests/test_catraca_data_evento_cru.py:245`)

## 11/11 15:48 · sessao bc8105267297 · - · - · origem=Ronald (tmux)

**Item (3): não é estável contra leitor novo invisível.** Há reprodução em memória preservando o retorno de `gravar_dia_do_fato`: substituir `return 'data_evento'` por `return {'data_evento': linha.data_evento}.popitem()[0]`. O retorno continua `'data_evento'`, a leitura direta ocorre uma vez, mas o mapa completo permanece idêntico: escopo **1→1**. O literal original contado é substituído por chave de dict excluída e atributo lido contado. (`app/chamados/services/fio.py:800`; `app/chamados/services/fio.py:801`; `app/chamados/tests/test_catraca_data_evento_cru.py:104`; `app/chamados/tests/test_catraca_data_evento_cru.py:108`; `app/chamados/tests/test_catraca_data_evento_cru.py:110`)

Isso demonstra **leitor novo invisível**, sem reflexão, lambda, alias ou leitura indireta: antes havia somente o nome devolvido para escrita; depois há uma leitura real adicional. A comparação exclusivamente numérica aceita o resultado, pois nenhuma chave ou contagem muda. O limite declarado fala em trocar um leitor por outro; aqui um rótulo sobrecontado financia o primeiro leitor real desse escopo. (`app/chamados/services/fio.py:791`; `app/chamados/tests/test_catraca_data_evento_cru.py:24`; `app/chamados/tests/test_catraca_data_evento_cru.py:37`; `app/chamados/tests/test_catraca_data_evento_cru.py:189`)

As quatro sobrecontagens atuais são nomes de escrita/parâmetros, e inserir isoladamente uma leitura em cada escopo produz **1→2**, portanto morde. Isso não elimina a reprodução compensada acima. (`app/chamados/services/fio.py:801`; `app/chamados/models.py:3221`; `app/chamados/signals.py:255`; `app/escala/management/commands/hub_smoke.py:35`; `app/escala/management/commands/hub_smoke.py:40`; `app/escala/management/commands/hub_smoke.py:51`; `app/chamados/tests/test_catraca_data_evento_cru.py:190`)

Segunda reprodução independente: em `hub_smoke.handle`, montar `SIMPLES` como lista sem o literal, recolocá-lo por `SIMPLES.append('data_evento')`, e acrescentar `PerguntaDisputa.objects.filter(data_evento__isnull=True).exists()`. A pertinência dos parâmetros é preservada, surge filtro real e o mapa inteiro continua igual, **1→1**, porque `append` suprime o destino. (`app/escala/management/commands/hub_smoke.py:35`; `app/escala/management/commands/hub_smoke.py:41`; `app/chamados/tests/test_catraca_data_evento_cru.py:63`; `app/chamados/tests/test_catraca_data_evento_cru.py:134`; `app/chamados/tests/test_catraca_data_evento_cru.py:138`)

## 11/11 15:47 · sessao 3cddfded21d6 · LASTRO-FOLGA-FORA-DO-CATALOGO · verificador · origem=Code

**APROVADO nesta pré-verificação da c2.** B2 foi curado; B1 e B3 têm correções efetivas e resíduos preexistentes cuja triagem procede. Isso não certifica pouso: RED/GREEN executados, suite inteira desta árvore e opus final permanecem **[nao verificado]** (`app/ponto/catalogo/ausencias.py:284`, `app/ponto/models.py:881`, `app/ponto/services/ausencia.py:429`, `app/ponto/services/ausencia.py:523`, `CLAUDE.md:377`).

**(1) B1–B3**

| Bloqueante anterior | Situação | Fundamentação |
|---|---|---|
| **B1 — cache** | **Parcialmente curado; isolamento curado.** | Cada leitura obtém o schema atual e usa sua própria entrada. Propagação entre processos continua limitada ao TTL de 60 s e à invalidação local; essa parcela já pertencia à autoridade consultada pelas portas (`app/ponto/catalogo/ausencias.py:262`, `app/ponto/catalogo/ausencias.py:265`, `app/ponto/catalogo/ausencias.py:272`, `app/ponto/catalogo/ausencias.py:284`, `app/ponto/services/ausencia.py:202`, `app/ponto/services/ausencia.py:486`). |
| **B2 — exceção nas portas** | **Curado.** | Criação e edição traduzem especificamente `ValidationError.code == 'tipo_fora_do_catalogo'` para `AusenciaInvalida(codigo='tipo_invalido')`; a API captura essa classe e responde 400 (`app/ponto/services/ausencia.py:135`, `app/ponto/services/ausencia.py:429`, `app/ponto/services/ausencia.py:523`, `app/api/views.py:1825`, `app/api/views.py:1835`). |
| **B3 — escrita parcial/corrida** | **Parcialmente curado; veto indevido curado.** | `update_fields` sem `tipo` retorna antes de consultar o catálogo. O SELECT histórico continua separado do save completo, portanto a corrida permanece possível (`app/ponto/models.py:881`, `app/ponto/models.py:888`, `app/ponto/models.py:938`). |

**Os motivos de triagem procedem.** TTL, invalidação local e fallback por tabela vazia/exceção continuam na autoridade existente; as duas portas já a consultavam. O save completo também permanece: a guarda não criou o lost update genérico. Reclassifico esses resíduos da resposta anterior como limitações preexistentes, sem sustentarem bloqueante novo desta volta (`app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:252`, `app/ponto/catalogo/ausencias.py:253`, `app/ponto/catalogo/ausencias.py:288`, `app/ponto/services/ausencia.py:202`, `app/ponto/services/ausencia.py:486`, `app/ponto/models.py:938`).

**(2) Leitores, invalidação e troca de schema**

Os consumidores encontrados das respostas cacheadas são:

| Entrada | Quem lê |
|---|---|
| `vigente()` diretamente | Declaração de texto e atestados em lote (`app/chamados/services/declaracao_texto.py:179`, `app/ponto/services/atestados_lote.py:28`). |
| `codigos_validos()` | Formulário, guarda, prévia, criação, edição, exigência de documento, vigia e PWA (`app/ponto/forms.py:63`, `app/ponto/models.py:884`, `app/ponto/services/checar_ausencia.py:23`, `app/ponto/services/ausencia.py:202`, `app/ponto/services/ausencia.py:486`, `app/ponto/services/ausencia.py:822`, `app/ponto/services/vigia_ausencia.py:103`, `app/ponto/views.py:2509`). |
| `efeito_vigente()` | Modelo, dia decidido, status, serviço de ausência e diagnóstico (`app/ponto/models.py:842`, `app/ponto/services/dia_decidido.py:468`, `app/ponto/status_dia.py:311`, `app/ponto/services/ausencia.py:629`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:49`). |
| `rotulo_vigente()` / `rotulo_curto()` | Prévia, portas, PWA, dia decidido e calendário (`app/ponto/services/checar_ausencia.py:40`, `app/ponto/services/ausencia.py:445`, `app/ponto/services/ausencia.py:505`, `app/ponto/views.py:2550`, `app/ponto/services/dia_decidido.py:215`, `app/colaboradores/services/calendario.py:675`, `app/ponto/catalogo/ausencias.py:175`). |
| Choices vivas/agrupadas | API, intake, drawer, colaboradores, telas de ausência e relatórios (`app/api/views.py:1864`, `app/core/intake.py:369`, `app/colaboradores/services/drawer.py:138`, `app/colaboradores/views.py:1959`, `app/ponto/views.py:2085`, `app/ponto/views.py:2248`, `app/ponto/views.py:2462`, `app/ponto/views.py:2561`, `app/relatorios/views.py:242`). |
| `parciais_vigentes()` | Portas, telas, API, turnos, escala, materialização, fechamento, crédito, coletor de efeito e vigias (`app/ponto/services/ausencia.py:214`, `app/ponto/services/ausencia.py:492`, `app/ponto/views.py:2086`, `app/ponto/views.py:2237`, `app/api/views.py:1881`, `app/ponto/turnos.py:2599`, `app/ponto/turnos.py:2687`, `app/escala/utils.py:1187`, `app/chamados/services/materializacao.py:81`, `app/ponto/services/fechamento.py:692`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/services/efeito_ausencia.py:38`, `app/ponto/services/vigia_ausencia_veredito.py:49`). |
| Abonam/descontam e proxies `TIPOS_*` | Exportação, inteligência, precedência, documentos, alertas, aviso da batida, vigia e filtros de telas/relatórios (`app/folha/export.py:140`, `app/folha/export.py:181`, `app/folha/export.py:198`, `app/inteligencia/resumo.py:43`, `app/inteligencia/management/commands/calcular_scores.py:14`, `app/inteligencia/management/commands/gerar_alertas.py:58`, `app/ponto/precedencia.py:131`, `app/ponto/services/ausencia.py:646`, `app/ponto/services/alertas_ausencia.py:50`, `app/ponto/services/aviso_da_batida.py:79`, `app/ponto/services/vigia_ausencia.py:95`, `app/ponto/views.py:2198`, `app/ponto/views.py:2201`, `app/relatorios/views.py:282`). |

Não encontrei dependência externa do formato antigo `{'t','v'}`: definição, limpeza, consulta e gravação de `_CACHE` estão no próprio módulo. Os proxies chamam novamente suas funções a cada operação; os aliases do modelo preservam esses proxies (`app/ponto/catalogo/ausencias.py:261`, `app/ponto/catalogo/ausencias.py:272`, `app/ponto/catalogo/ausencias.py:285`, `app/ponto/catalogo/ausencias.py:289`, `app/ponto/catalogo/ausencias.py:412`, `app/ponto/catalogo/ausencias.py:415`, `app/ponto/catalogo/ausencias.py:464`, `app/ponto/models.py:683`, `app/ponto/models.py:689`).

O chamador produtivo encontrado de `invalidar_cache()` é o receiver de `post_save`/`post_delete` de `TipoAusencia`; a UI chega por `gravar_tipo_ausencia`. Os testes também invalidam explicitamente, sem acessar o formato interno (`app/ponto/models.py:1265`, `app/ponto/models.py:1268`, `app/ponto/views_tipos_ausencia.py:94`, `app/ponto/services/ausencia.py:1326`, `app/ponto/services/ausencia.py:1328`, `app/ponto/tests/test_cadastro_tipo.py:12`).

**Sim, há troca de schema dentro do mesmo comando/chamada:** o runner percorre tenants com `schema_context`, e o bootstrap entra em `tenant_context`. A chave é recalculada em cada `vigente()`, portanto A→B→A seleciona entradas distintas. `'public'` também é distinto de `None`; este significa ausência do atributo, tornando impreciso o comentário que aproxima control-plane de `None` (`app/tenants/cron.py:23`, `app/tenants/cron.py:25`, `app/tenants/bootstrap.py:19`, `app/ponto/catalogo/ausencias.py:260`, `app/ponto/catalogo/ausencias.py:267`, `app/ponto/catalogo/ausencias.py:284`).

O cron configura `tenant_command`; execução integrada real desse pacote e desses contextos nesta avaliação: **[nao verificado]** (`app/config/crons.py:21`).

**(3) Traduções, anexo, transação e outros saves**

Outra `ValidationError` do save **não é engolida**: o filtro exige classe e código exatos; criação relança os demais erros e edição usa `raise`. A validação de fim anterior ao início, por exemplo, continua subindo inteira (`app/ponto/services/ausencia.py:135`, `app/ponto/services/ausencia.py:433`, `app/ponto/services/ausencia.py:441`, `app/ponto/services/ausencia.py:523`, `app/ponto/models.py:926`).

Na criação, o save está dentro do `atomic` interno; limpeza e tradução acontecem depois de sair dele, com a limpeza primeiro. **A recusa real da guarda não chega a gravar upload**, pois ocorre antes de `super().save`; nome inalterado faz o cleanup retornar `False`. Logo, “anexo morre” não é consequência necessária dessa recusa. O helper tenta apagar arquivo com nome alterado e contém falha de exclusão (`app/ponto/services/ausencia.py:423`, `app/ponto/services/ausencia.py:425`, `app/ponto/services/ausencia.py:428`, `app/ponto/services/ausencia.py:432`, `app/ponto/models.py:898`, `app/ponto/models.py:938`, `app/ponto/services/ausencia.py:122`, `app/ponto/services/ausencia.py:125`, `app/ponto/services/ausencia.py:127`).

A edição não introduz transação própria; conserva a transação do chamador quando existente. A tradução envolve seu save, antes da invalidação do fechamento (`app/ponto/services/ausencia.py:521`, `app/ponto/services/ausencia.py:530`, `app/ponto/services/ausencia.py:542`).

Fora dessas duas portas, os saves encontrados são rejulgamento de prazo, estado, anexo, carimbos/período e reabertura. Todos usam `update_fields` sem `tipo`, evitando a nova recusa do catálogo nesses caminhos (`app/ponto/services/ausencia.py:570`, `app/ponto/services/ausencia.py:853`, `app/ponto/services/ausencia.py:877`, `app/ponto/services/ausencia.py:945`, `app/ponto/services/ausencia.py:1148`, `app/ponto/services/ausencia.py:1158`, `app/ponto/services/ausencia.py:1166`, `app/ponto/models.py:881`).

A exceção restante é `rederivar_fim`: faz save completo e pode devolver a exceção crua diante de desacordo com o gravado. Seu chamador encontrado é o comando `curar_ausencia_sem_fim`, sem captura; não foi identificado novo propagador HTTP por esse caminho (`app/ponto/services/ausencia.py:1314`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:46`). O admin bloqueia escrita, e os receivers de Ausência encaminham o fato sem atribuir `tipo` (`app/ponto/admin.py:38`, `app/core/admin.py:59`, `app/core/admin.py:62`, `app/core/admin.py:65`, `app/ponto/signals.py:143`, `app/ponto/signals.py:275`).

**(4) `update_fields`**

Não encontrei chamador produtivo de `Ausencia.save` passando `update_fields` posicionalmente. O override, contudo, só lê `kwargs`: essa forma permanece uma lacuna latente da assinatura, sem caminho produtivo demonstrado. `tipo` é `CharField`, sem o alias de FK `tipo_id` (`app/ponto/models.py:749`, `app/ponto/models.py:896`, `app/ponto/models.py:898`).

Não encontrei caminho produtivo que altere `tipo` esperando persistir por lista que o omite: a edição atribui o campo e faz save completo; os saves parciais encontrados escrevem outros campos (`app/ponto/services/ausencia.py:510`, `app/ponto/services/ausencia.py:522`, `app/ponto/services/ausencia.py:853`, `app/ponto/services/ausencia.py:945`, `app/ponto/services/ausencia.py:1158`).

**(5) Classes de reprovação e mordida**

- **Segundo juiz:** a guarda consulta a mesma `codigos_validos()` das portas. `faixa_de()` continua respondendo precedência, outra pergunta (`app/ponto/models.py:883`, `app/ponto/services/ausencia.py:202`, `app/ponto/services/ausencia.py:486`, `app/ponto/precedencia.py:76`).
- **Segundo escritor:** não foi identificado escritor produtivo adicional de `tipo`; a porta declarada continua sendo o serviço. O contorno por `update(tipo=...)` está explícito na fixture; execução da catraca estrutural: **[nao verificado]** (`app/core/portas.py:486`, `app/ponto/services/ausencia.py:510`, `app/ponto/tests/test_lastro_folga_save_guarda.py:67`, `app/ponto/tests/test_chokepoint_ausencia_gate.py:53`).
- **Relógio:** a guarda não acrescenta relógio próprio; usa a autoridade com TTL centralizado preexistente (`app/ponto/models.py:881`, `app/ponto/catalogo/ausencias.py:262`, `app/ponto/catalogo/ausencias.py:283`).
- **Dois significados:** choices congeladas continuam distintas da validade viva; a criação exclui `tipo` do `full_clean`, e o teste preserva código vivo fora das choices (`app/ponto/models.py:749`, `app/ponto/services/ausencia.py:395`, `app/ponto/tests/test_lastro_folga_save_guarda.py:134`).

Os cinco testes novos **têm mordida lógica**, com estes limites: [nao verificado]

| Teste | O que detecta / limite | [nao verificado]
|---|---|
| Cache A→B | Reprova cache global compartilhado; usa mocks, sem troca real de schema (`app/ponto/tests/test_lastro_folga_save_guarda.py:157`). |
| Tradução da criação | Exige classe/código e nenhuma linha gravada; força a recusa por mock (`app/ponto/tests/test_lastro_folga_save_guarda.py:169`, `app/ponto/tests/test_lastro_folga_save_guarda.py:175`). |
| Tradução da edição | Exige classe/código; não verifica estado persistido após a recusa (`app/ponto/tests/test_lastro_folga_save_guarda.py:189`). |
| Parcial sem `tipo` | Detecta veto indevido e verifica tipo preservado; não prova literalmente ausência de consulta ao catálogo (`app/ponto/tests/test_lastro_folga_save_guarda.py:203`). |
| Parcial com `tipo` | Detecta retirada da guarda e verifica tipo preservado; aceita qualquer `ValidationError` (`app/ponto/tests/test_lastro_folga_save_guarda.py:215`). |

RED, GREEN e mutantes efetivamente executados: **[nao verificado]**.

Há uma **contradição documental relevante**: a docstring afirma que `folga` nunca esteve no catálogo vivo de produção, mas o relato posterior registra que estava cadastrada e distingue fora das choices de fora do catálogo. O teste continua válido como cenário local porque verifica sua pré-condição; a alegação histórica não está sustentada (`app/ponto/tests/test_lastro_folga_save_guarda.py:3`, `app/ponto/tests/test_lastro_folga_save_guarda.py:48`, `app/docs/RELATO.md:7368`, `app/docs/RELATO.md:7369`).

Quanto ao pré-requisito, o BACKLOG exige medir escritor/frota, mas há registro posterior de censo, sonda e conferência: não cabe declarar descumprimento apenas pela linha ainda aberta. Artefatos originais, escritor histórico exato e cumprimento dos requisitos finais de pouso permanecem **[nao verificado]** (`app/docs/BACKLOG.md:1807`, `app/docs/RELATO.md:7368`, `CLAUDE.md:377`).

## 11/11 15:47 · sessao 682710215ca8 · - · - · origem=Ronald (tmux)

As duas formas novas mordem os respectivos mutantes:

- Tirar `reapontar_resolvedora` de `ESCRITA` faz surgir `z.py::escreve: 1`, contrariando o esperado: teste vermelho. (`app/chamados/tests/test_catraca_data_evento_cru.py:64`; `app/chamados/tests/test_catraca_data_evento_cru.py:227`; `app/chamados/tests/test_catraca_data_evento_cru.py:237`)
- Remover o tratamento de subscrito `Load` faz `escreve_lendo` cair de **3 para 2**: teste vermelho. A forma mínima `M.objects.create(x=p['data_evento'])` agora conta **1**. (`app/chamados/tests/test_catraca_data_evento_cru.py:119`; `app/chamados/tests/test_catraca_data_evento_cru.py:123`; `app/chamados/tests/test_catraca_data_evento_cru.py:235`; `app/chamados/tests/test_catraca_data_evento_cru.py:238`)

Executei em memória com `python3 -B` e substituição de `SimpleTestCase` por `unittest.TestCase`; passaram `test_so_desce`, `test_o_juiz_existe_e_le`, `test_MORDE_cada_forma` e `test_MORDE_template_sem_comentario`. Suíte Django: **[nao verificado]**. O teste que cria temporários não foi executado. (`app/chamados/tests/test_catraca_data_evento_cru.py:185`; `app/chamados/tests/test_catraca_data_evento_cru.py:198`; `app/chamados/tests/test_catraca_data_evento_cru.py:203`; `app/chamados/tests/test_catraca_data_evento_cru.py:240`; `app/chamados/tests/test_catraca_data_evento_cru.py:245`)

**FILA:** remover somente `rechavear_data_evento` de `ESCRITA` sobrevive a `test_MORDE_cada_forma`; a fixture acrescenta `reapontar`, sem exercício de `rechavear`. Isso demonstra lacuna de cobertura, sem demonstrar leitor real omitido. (`app/chamados/tests/test_catraca_data_evento_cru.py:64`; `app/chamados/tests/test_catraca_data_evento_cru.py:219`; `app/chamados/tests/test_catraca_data_evento_cru.py:227`; `app/chamados/tests/test_catraca_data_evento_cru.py:235`)

## 11/11 15:46 · sessao dff7b4a6fb6d · - · - · origem=Ronald (tmux)

Não encontrei leitura real omitida pelas duas portas novas. `ESCRITA` suprime os destinos, enquanto atributos lidos e chamadas internas continuam contados. (`app/chamados/tests/test_catraca_data_evento_cru.py:64`; `app/chamados/tests/test_catraca_data_evento_cru.py:104`; `app/chamados/tests/test_catraca_data_evento_cru.py:137`)

- `reapontar_resolvedora`: os quatro `data_evento=` são pedidos de datação; a porta passa o pedido ao juiz, grava pelo escritor e salva. Não são constantes usadas para selecionar/ordenar o campo. (`app/chamados/reconciliador.py:954`; `app/chamados/reconciliador.py:1642`; `app/chamados/reconciliador.py:1655`; `app/chamados/services/declaracao_texto.py:370`; `app/chamados/services/fio.py:876`; `app/chamados/services/fio.py:879`; `app/chamados/services/fio.py:927`)
- `rechavear_data_evento`: recebe valores de dia, grava `novo_dia` e salva; seus chamadores não passam a constante do campo para filtro/ordenação. (`app/chamados/services/fio.py:1074`; `app/chamados/services/fio.py:1078`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:400`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:492`; `app/chamados/services/fio.py:1037`; `app/chamados/services/fio.py:1043`)
- As leituras reais presentes nas implementações — `p.data_evento` — permanecem visíveis ao ramo `Attribute/Load` do varredor. (`app/chamados/services/fio.py:867`; `app/chamados/services/fio.py:934`; `app/chamados/services/fio.py:1013`; `app/chamados/tests/test_catraca_data_evento_cru.py:104`)

## 11/11 15:46 · sessao 2985e584509f · - · - · origem=Ronald (tmux)

- **B2 curado nas duas portas.** A tradução exige `ValidationError` e `code == 'tipo_fora_do_catalogo'`; outros erros sobem integralmente. A edição usa `raise` para qualquer outro código. Exemplo de erro preservado: fim anterior ao início (`app/ponto/services/ausencia.py:135`, `app/ponto/services/ausencia.py:429`, `app/ponto/services/ausencia.py:523`, `app/ponto/models.py:926`).

- **Atomic/anexo:** criação executa `save` dentro do `atomic`; captura, limpeza e tradução acontecem depois de sair dele. A limpeza vem antes da tradução. Entretanto, a recusa real da guarda precede `super().save`, portanto não grava o upload; nome inalterado faz a limpeza retornar `False`. Não cabe afirmar que o anexo necessariamente “morre” nessa recusa. Arquivo novo com nome alterado é apagado; falha de exclusão é contida (`app/ponto/services/ausencia.py:423`, `app/ponto/services/ausencia.py:425`, `app/ponto/services/ausencia.py:428`, `app/ponto/services/ausencia.py:432`, `app/ponto/models.py:898`, `app/ponto/models.py:938`, `app/ponto/services/ausencia.py:122`, `app/ponto/services/ausencia.py:125`, `app/ponto/services/ausencia.py:127`).

- **Demais saves encontrados:** prazo após rejulgamento; mudança de estado; anexação; carimbos/período da decisão; documento, descrição e prazo da reabertura. Todos fornecem `update_fields` sem `tipo`, de modo que agora retornam antes da consulta ao catálogo. Não demonstram novo escape HTTP da recusa do catálogo (`app/ponto/services/ausencia.py:570`, `app/ponto/services/ausencia.py:853`, `app/ponto/services/ausencia.py:877`, `app/ponto/services/ausencia.py:945`, `app/ponto/services/ausencia.py:1148`, `app/ponto/services/ausencia.py:1158`, `app/ponto/services/ausencia.py:1166`, `app/ponto/models.py:881`).

- **Save completo adicional:** `rederivar_fim` continua podendo levantar a exceção crua se houver desacordo com o tipo persistido. Seu chamador encontrado é o comando `curar_ausencia_sem_fim`, sem captura dessa exceção; uso por porta HTTP não foi encontrado **[não verificado além da busca estática]** (`app/ponto/services/ausencia.py:1314`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:42`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:46`).

- **Admin e signals:** admin de Ausência proíbe adicionar, alterar e apagar. Receivers de Ausência encaminham o fato ao cartório e consumidor, com contenção; não atribuem `tipo` nem chamam `Ausencia.save` diretamente (`app/ponto/admin.py:38`, `app/core/admin.py:59`, `app/core/admin.py:62`, `app/core/admin.py:65`, `app/ponto/signals.py:143`, `app/ponto/signals.py:147`, `app/ponto/signals.py:88`, `app/ponto/signals.py:263`, `app/ponto/signals.py:275`).

- **`update_fields`:** nenhum chamador produtivo de `Ausencia.save` com argumentos posicionais apareceu na inspeção **[não verificado por execução]**. O override só consulta `kwargs`, portanto o formato posicional permaneceria um limite latente. `tipo` é `CharField`, sem alias de FK `tipo_id`. A atribuição produtiva encontrada ocorre na edição e é seguida de save completo, sem expectativa de gravar tipo por lista que o omite (`app/ponto/models.py:898`, `app/ponto/models.py:749`, `app/ponto/services/ausencia.py:510`, `app/ponto/services/ausencia.py:522`).

- **B3 parcialmente curado:** a recusa indevida de escrita parcial foi corrigida; leitura histórica e save completo continuam separados. A classificação da corrida como lost update genérico procede: o diff fornecido preserva o save completo anterior e acrescenta a guarda; ela não criou a possibilidade de sobrescrever uma correção concorrente. Portanto, a corrida continua possível, mas não sustenta sozinha bloqueante novo desta obra (`app/ponto/models.py:881`, `app/ponto/models.py:887`, `app/ponto/models.py:938`; preexistência de `save` completo: diff de CURA fornecido, **[sem arquivo:linha da base disponível]**).

- **Segundo escritor:** os sites encontrados permanecem na porta declarada; existe teste estrutural com allowlist vazia, mas sua execução está **[não verificado]** (`app/core/portas.py:486`, `app/ponto/tests/test_chokepoint_ausencia_gate.py:25`, `app/ponto/tests/test_chokepoint_ausencia_gate.py:53`).

## 11/11 15:46 · sessao e9e3f7e485d4 · - · - · origem=Ronald (tmux)

B1 **curado quanto ao isolamento entre schemas**: `_schema()` consulta `connection.schema_name` em cada leitura; `vigente()` procura/grava a tupla `(agora, v)` usando essa chave, de modo que trocar A→B→A seleciona caches separados (`app/ponto/catalogo/ausencias.py:265`, `app/ponto/catalogo/ausencias.py:284`, `app/ponto/catalogo/ausencias.py:289`). `None` é exclusivamente a ausência desse atributo; um backend tenant-aware em `public` usa a chave literal `'public'` (`app/ponto/catalogo/ausencias.py:267`). O comentário que aproxima “control-plane” de `None` é impreciso, mas a implementação distingue corretamente (`app/ponto/catalogo/ausencias.py:260`, `app/config/settings/saas.py:85`).

Não encontrei acesso externo ao `_CACHE` nem dependência do formato antigo ou do compartilhamento entre tenants na cópia examinada; os acessos efetivos estão somente na definição, limpeza, busca e gravação do próprio módulo (`app/ponto/catalogo/ausencias.py:261`, `app/ponto/catalogo/ausencias.py:272`, `app/ponto/catalogo/ausencias.py:285`, `app/ponto/catalogo/ausencias.py:289`). O único chamador **produtivo** de `invalidar_cache()` encontrado é o receiver de `post_save`/`post_delete` de `TipoAusencia`; os demais chamadores encontrados são testes (`app/ponto/models.py:1265`, `app/ponto/models.py:1268`; exemplos `app/ponto/tests/test_cadastro_tipo.py:12`, `app/ponto/tests/test_lastro_folga_save_guarda.py:46`). A UI chega ao signal através de `gravar_tipo_ausencia`, que usa `save()`/`objects.create()` (`app/ponto/views_tipos_ausencia.py:86`, `app/ponto/views_tipos_ausencia.py:94`, `app/ponto/services/ausencia.py:1326`, `app/ponto/services/ausencia.py:1328`).

Há mudança de schema no mesmo comando: `para_cada_tenant()` itera clientes com `schema_context(cliente.schema_name)`; isso agora seleciona uma entrada por schema em vez de depender de compartilhamento (`app/tenants/cron.py:19`, `app/tenants/cron.py:25`, `app/ponto/catalogo/ausencias.py:284`). Há mudança dentro da chamada de provisionamento do control-plane: a view chama `provision_tenant`, que chama bootstrap dentro de `tenant_context(cliente)` (`app/tenants/views.py:52`, `app/tenants/services.py:13`, `app/tenants/bootstrap.py:19`). Não há leitor de catálogo no próprio bootstrap ou nas views de tenants examinadas; `ponto` é app de tenant, e o backend usa `default` tenant-aware (`app/tenants/bootstrap.py:19`, `app/tenants/bootstrap.py:26`, `app/tenants/views.py:25`, `app/config/settings/saas.py:34`, `app/config/settings/saas.py:40`, `app/config/settings/saas.py:83`). Integração real do pacote `django-tenants`/`tenant_command` nesta execução: **[não verificado]**; o teste usa mocks de `schema_name` e `_do_banco`, não schemas reais (`app/ponto/tests/test_lastro_folga_save_guarda.py:161`, `app/ponto/tests/test_lastro_folga_save_guarda.py:164`).

Leitores encontrados das respostas cacheadas:

| API/conjunto | Consumidores produtivos e referências |
|---|---|
| `vigente` direto | Declaração de texto; atestados em lote (`app/chamados/services/declaracao_texto.py:179`, `app/ponto/services/atestados_lote.py:28`). |
| `codigos_validos` | Form, guarda do modelo, prévia, criação, edição, composição de exigência de documento, vigia e PWA (`app/ponto/forms.py:63`, `app/ponto/models.py:884`, `app/ponto/services/checar_ausencia.py:23`, `app/ponto/services/ausencia.py:202`, `app/ponto/services/ausencia.py:486`, `app/ponto/services/ausencia.py:822`, `app/ponto/services/vigia_ausencia.py:103`, `app/ponto/views.py:2509`). |
| `efeito_vigente` | Propriedade do modelo, dia decidido, status, serviço de ausência e comando diagnóstico (`app/ponto/models.py:842`, `app/ponto/services/dia_decidido.py:468`, `app/ponto/status_dia.py:311`, `app/ponto/services/ausencia.py:629`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:49`). |
| `rotulo_vigente` e indireto por `rotulo_curto` | Prévia, portas, PWA, dia decidido e calendário; `rotulo_curto` consulta o vivo quando não há rótulo estático (`app/ponto/services/checar_ausencia.py:40`, `app/ponto/services/ausencia.py:445`, `app/ponto/services/ausencia.py:505`, `app/ponto/views.py:2550`, `app/ponto/services/dia_decidido.py:215`, `app/ponto/services/dia_decidido.py:216`, `app/colaboradores/services/calendario.py:675`, `app/ponto/catalogo/ausencias.py:175`). |
| `choices_vigentes` / agrupadas | API, intake, drawer, views de colaboradores, telas de ausência e relatórios (`app/api/views.py:1864`, `app/core/intake.py:369`, `app/colaboradores/services/drawer.py:138`, `app/colaboradores/views.py:1959`, `app/ponto/views.py:2085`, `app/ponto/views.py:2248`, `app/ponto/views.py:2462`, `app/ponto/views.py:2561`, `app/relatorios/views.py:242`). |
| `parciais_vigentes` | Portas/telas/API, turnos, escala, materialização, fechamento, crédito e vigias (`app/ponto/services/ausencia.py:214`, `app/ponto/services/ausencia.py:492`, `app/ponto/views.py:2086`, `app/ponto/views.py:2237`, `app/api/views.py:1881`, `app/ponto/turnos.py:2599`, `app/ponto/turnos.py:2687`, `app/escala/utils.py:1187`, `app/chamados/services/materializacao.py:81`, `app/ponto/services/fechamento.py:692`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/services/efeito_ausencia.py:38`, `app/ponto/services/vigia_ausencia_veredito.py:49`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:37`). |
| `abonam` / `descontam` | Exportação, inteligência, precedência e contadores/telas (`app/folha/export.py:140`, `app/folha/export.py:181`, `app/folha/export.py:198`, `app/inteligencia/resumo.py:43`, `app/inteligencia/resumo.py:44`, `app/inteligencia/management/commands/calcular_scores.py:14`, `app/inteligencia/management/commands/gerar_alertas.py:58`, `app/ponto/precedencia.py:131`, `app/ponto/views.py:2198`, `app/ponto/views.py:2200`, `app/ponto/views.py:2872`, `app/relatorios/views.py:276`). |
| Proxies `TIPOS_*` / `EM_ABERTO_VIVOS` | Todos recalculam pelas funções em cada operação do proxy, inclusive os aliases do modelo (`app/ponto/catalogo/ausencias.py:412`, `app/ponto/catalogo/ausencias.py:415`, `app/ponto/catalogo/ausencias.py:418`, `app/ponto/catalogo/ausencias.py:464`, `app/ponto/catalogo/ausencias.py:476`, `app/ponto/models.py:683`, `app/ponto/models.py:689`). Exemplos consumidores diretos: documentos, alertas, bloqueios, vigia e filtros (`app/ponto/services/ausencia.py:646`, `app/ponto/services/ausencia.py:813`, `app/ponto/services/alertas_ausencia.py:50`, `app/ponto/services/aviso_da_batida.py:79`, `app/ponto/services/vigia_ausencia.py:95`, `app/ponto/views.py:2201`, `app/ponto/views.py:2222`, `app/relatorios/views.py:282`). |

Os motivos de triagem **procedem para TTL/fallback**: continuam TTL 60 por `monotonic`, invalidação local e fallback após vazio/exceção; a c2 muda a organização do cache, não esses critérios (`app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:252`, `app/ponto/catalogo/ausencias.py:253`, `app/ponto/catalogo/ausencias.py:262`, `app/ponto/catalogo/ausencias.py:272`, `app/ponto/catalogo/ausencias.py:283`, `app/ponto/catalogo/ausencias.py:288`). Preexistência está também no diff fornecido pelo usuário; prova histórica por Git indisponível nesta cópia **[não verificado]**. Esses comportamentos já alimentam as portas, que consultam a mesma autoridade (`app/ponto/services/ausencia.py:202`, `app/ponto/services/ausencia.py:486`).

Limite: o teste de cache morde a regressão da chave entre A e B; não testa retorno a A, chave public/None, invalidação de todos os schemas ou expiração TTL, nem execução concorrente real (`app/ponto/tests/test_lastro_folga_save_guarda.py:157`, `app/ponto/tests/test_lastro_folga_save_guarda.py:166`).

## 11/11 15:45 · sessao 9bbf5b5e2f97 · - · - · origem=Ronald (tmux)

Os cinco testes novos têm mordida lógica, com limites distintos: [nao verificado]

- **Cache por schema:** carrega A e, sem invalidar, exige catálogo B; o cache antigo global devolveria A e reprovaria. Não exercita retorno B→A, schema `public`, expiração, invalidação nem troca real de schema no banco; usa mocks de `schema_name` e `_do_banco` (`app/ponto/tests/test_lastro_folga_save_guarda.py:157-167`).
- **Tradução na criação:** força recusa da guarda real chamada por `save`, exige `AusenciaInvalida.codigo == 'tipo_invalido'`, falha se escapar `ValidationError` e exige ausência de linha persistida. Remover a tradução reprovaria (`app/ponto/tests/test_lastro_folga_save_guarda.py:169-187`).
- **Tradução na edição:** força a mesma recusa e verifica classe/código da tradução. Remover a tradução reprovaria; não verifica estado persistido ou restauração da instância após recusa (`app/ponto/tests/test_lastro_folga_save_guarda.py:189-201`).
- **`update_fields` sem tipo:** põe tipo morto somente na memória, salva descrição e exige tipo antigo preservado no banco. A guarda c1 reprovaria porque compararia morto em memória com `abono` persistido. O teste prova ausência de veto indevido; não prova literalmente “não consulta catálogo”, pois não conta consultas nem intercepta `codigos_validos` (`app/ponto/tests/test_lastro_folga_save_guarda.py:203-213`; `app/ponto/models.py:881-889`).
- **`update_fields` com tipo:** exige recusa e manutenção de `abono`; remover toda a guarda reprovaria. Aceita qualquer `ValidationError`, sem verificar o código da recusa (`app/ponto/tests/test_lastro_folga_save_guarda.py:215-221`).

Os testes de tradução mockam a guarda para produzir o código específico; não exercitam desativação concorrente real do catálogo, propagação de outro `ValidationError`, limpeza de anexo nem limites transacionais. Esses cenários não estão nos cinco corpos (`app/ponto/tests/test_lastro_folga_save_guarda.py:157-221`). RED executado, GREEN, mutantes executados e suite inteira desta obra: **[não verificado]**. A exigência de evidência RED e suite anterior ao opus está expressa (`CLAUDE.md:28`, `CLAUDE.md:193`, `CLAUDE.md:377-381`).

Sobre as classes:

- **Segundo juiz:** a guarda consulta `codigos_validos`, mesma função das duas portas; não inventa uma lista de códigos. O registro agrupa duração/validade em `fim_de`, portanto não convém afirmar que `codigos_validos` tem registro nominal próprio sem ressalva (`app/ponto/models.py:883-884`; `app/ponto/services/ausencia.py:201-203`, `app/ponto/services/ausencia.py:485-487`; `app/core/juizes.py:269`).
- **Relógio fora da borda:** a cura usa o TTL já centralizado na autoridade; relógio `monotonic()` e 60 segundos estão no catálogo. A guarda não acrescenta relógio próprio (`app/ponto/catalogo/ausencias.py:262`, `app/ponto/catalogo/ausencias.py:282-289`; `app/ponto/models.py:881-894`).
- **Dois significados/listas:** `tipo` conserva choices congeladas, mas a criação exclui esse campo de `full_clean`, e o teste exige admissão de código vivo ausente das choices. Divergência preservada, sem nova autoridade na guarda (`app/ponto/models.py:661-679`, `app/ponto/models.py:749`; `app/ponto/services/ausencia.py:393-395`; `app/ponto/tests/test_lastro_folga_save_guarda.py:134-142`).
- **Pré-requisito:** BACKLOG exige medir escritor/frota antes da cura, mas há registro posterior de censo e sonda com rc0 e conferência; não cabe concluir que nunca foram medidos apenas pela linha ainda aberta (`app/docs/BACKLOG.md:1807`; `app/docs/RELATO.md:7367-7371`). Artefatos originais citados, execução atual e escritor histórico exato: **[não verificado]**.
- **Premissa documental conflitante:** o teste diz que `folga` nunca esteve no catálogo vivo de produção, mas relato posterior diz que estava cadastrada e distingue `fora_choices=127` de `fora_catalogo=6`. O teste continua válido como construção local porque verifica explicitamente que `folga` está fora do catálogo da fixture; a alegação histórica da docstring não está sustentada e conflita com a cópia (`app/ponto/tests/test_lastro_folga_save_guarda.py:3-6`, `app/ponto/tests/test_lastro_folga_save_guarda.py:44-49`; `app/docs/RELATO.md:7368-7370`).

A aprovação técnica desta pré-verificação não certifica pouso: suite inteira verde e opus final continuam requisitos explícitos, com cumprimento nesta obra **[não verificado]** (`CLAUDE.md:377-381`). [nao verificado]

## 11/11 15:44 · sessao 0459ff023fc9 · FIO-E0-V4-E4B · censo · origem=Code

| Identificação e critério | Resultado |
|---|---|
| Árvore solicitada | `fb5b4be463695cf4c5f38d31aa5c044c2b7fb7a7`, cópia c8; identidade Git independente **[nao verificado]**. |
| Unidade de contagem | **Callsite**, não quantidade de execuções. Censo por `rg` e AST, resolvendo imports diretos, aliases de módulo e import relativo; execução em memória, sem relatório persistido **[nao verificado]**. Exemplos de aliases efetivamente lidos: `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:157`; `app/pautas/views.py:12`. |
| Resultado principal | Existem **dois ramos produtivos** que podem tomar DP antes das âncoras: cadastro sem turno e limbo exportado, durante a transição anterior ao agrupador. (`app/chamados/services/fecho_runtime.py:151`; `app/chamados/services/fecho_runtime.py:154`; `app/chamados/services/fabrica_da_celula.py:204`; `app/chamados/services/fabrica_da_celula.py:215`) |
| Limite da conclusão | Alcance pelo código é distinto de ocorrência: transações concorrentes observadas, dados que satisfaçam simultaneamente os guards e deadlock efetivamente ocorrido **[nao verificado]**. |

| Notação usada nas sequências | Recurso e aquisição |
|---|---|
| **C → D → Ch → Q** | Colaborador → DisputaSupervisao → ChamadoColaborador → PerguntaDisputa; cada conjunto explicitamente solicitado é travado por PK crescente. (`app/chamados/models.py:2666`; `app/chamados/models.py:2675`; `app/chamados/models.py:2688`; `app/chamados/models.py:2692`) |
| **A\*** | Todas as âncoras E4B do lote, ordenadas pelo texto da âncora; advisory transacional. (`app/chamados/services/fecho_runtime.py:86`; `app/chamados/services/fecho_runtime.py:89`; `app/chamados/services/fecho_runtime.py:120`) |
| **P → DP** | INSERT Pauta → trilha → obtenção/criação do ContadorPauta → UPDATE do contador DP. A porta `escrever` é atômica. (`app/pautas/services.py:152`; `app/pautas/services.py:180`; `app/pautas/services.py:186`; `app/pautas/services.py:188`; `app/pautas/services.py:352`; `app/pautas/services.py:354`) |
| **FM / PA / Ag / Au / J / HE / Cel / TM** | FechamentoMensal / PeriodoAquisitivo / AgendamentoFerias / Ausencia / Justificativa / DecisaoHE / CelulaDia / TurnoMaterializado. Aquisições correspondentes nas matrizes seguintes. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`; `app/ferias/services.py:364`; `app/ferias/services.py:367`; `app/ponto/portas/celula.py:657`; `app/ponto/services/justificativa.py:142`; `app/ponto/portas/he.py:174`; `app/ponto/portas/celula.py:593`; `app/ponto/nucleo.py:81`) |

| Aquisição explícita de linha | Quem toma, ordem e função que alcança Pauta |
|---|---|
| **C — `select_for_update`** (`app/chamados/models.py:2666`) | `PerguntaDisputa.trava_na_ordem`: primeiro grupo; `_gravar_estado` chama antes do save do chamado, que dispara os receivers. (`app/chamados/models.py:2629`; `app/chamados/models.py:290`; `app/chamados/models.py:296`) |
| **D — `select_for_update`** (`app/chamados/models.py:2675`) | Segundo grupo da mesma porta; com `abertas_do_colaborador=True`, inclui todas as disputas abertas daquele colaborador. (`app/chamados/models.py:2670`; `app/chamados/models.py:2673`; `app/chamados/models.py:290`) |
| **Ch — `select_for_update`** (`app/chamados/models.py:2688`) | Terceiro grupo: chamados pedidos, pais das disputas e, no universo do colaborador, chamados vivos. (`app/chamados/models.py:2679`; `app/chamados/models.py:2683`) |
| **Q — `select_for_update(of=('self',))`** (`app/chamados/models.py:2692`) | Último grupo, somente quando IDs de perguntas são fornecidos; não significa que toda escrita implícita de Q tenha passado por esse SELECT. (`app/chamados/models.py:2691`; `app/chamados/models.py:3152`; `app/chamados/models.py:3423`) |
| **FM — `select_for_update`** (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`) | Command pago/devido: atomic → C → FM → P cabeça → DP → P filhas → DP. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:189`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:211`) |
| **J — quatro sites** (`app/ponto/services/justificativa.py:142`; `app/ponto/services/justificativa.py:277`; `app/ponto/services/justificativa.py:344`; `app/ponto/views.py:1794`) | Aprovar, rejeitar, materializar saída e aprovação em lote pedem C/D/Ch **antes** de J. Save de J pode chamar cartório/fábrica/DP. (`app/ponto/services/justificativa.py:141`; `app/ponto/services/justificativa.py:276`; `app/ponto/services/justificativa.py:343`; `app/ponto/views.py:1793`; `app/ponto/signals.py:165`) |
| **Ag → PA — dois sites** (`app/ferias/services.py:364`; `app/ferias/services.py:367`) | `_travar_agendamento`, dentro dos atomics de cancelar DP, cancelar admin e trocar janela; depois vêm alterações de Ausencia, sync/cartório e eventual Pauta DP. (`app/ferias/services.py:379`; `app/ferias/services.py:402`; `app/ferias/services.py:433`; `app/ferias/services.py:450`; `app/ferias/models.py:451`; `app/ponto/services/ausencia.py:1245`) |
| **Ag — alinhamento de status** (`app/ferias/services.py:600`) | Atomic → Ag → save do status → sync de Ausencia → cartório ou Pauta de recusa. (`app/ferias/services.py:599`; `app/ferias/services.py:604`; `app/ferias/models.py:451`; `app/ponto/services/ausencia.py:1216`) |
| **SolicitacaoFerias** (`app/ferias/views.py:740`) | Atomic da view → solicitação → aprovação → save de novo Ag → sync/Ausencia/possível DP → UPDATE PA. (`app/ferias/views.py:739`; `app/ferias/views.py:753`; `app/ferias/services.py:503`; `app/ferias/services.py:506`; `app/ferias/models.py:451`) |
| **HE — dois sites** (`app/ponto/portas/he.py:174`; `app/ponto/portas/he.py:555`) | Atomic da prévia/confirmação → itens na ordem recebida → HE do dia → `decidir_he`, com reaquisição da mesma linha → eventual Pauta DP. O lote aceita um colaborador. (`app/ponto/services/autorizacao_he_periodo.py:321`; `app/ponto/services/autorizacao_he_periodo.py:343`; `app/ponto/portas/he.py:534`; `app/ponto/portas/he.py:536`; `app/ponto/portas/he.py:546`; `app/ponto/portas/he.py:562`; `app/ponto/services/autorizacao_he_periodo.py:300`) |
| **Au — rederivação H06** (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:138`) | Atomic por linha → Au → edição/save → signal da ausência → cartório/fábrica/DP. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`; `app/ponto/services/ausencia.py:509`; `app/ponto/signals.py:147`) |
| **Cel — editar dia** (`app/ponto/portas/celula.py:593`) | Atomic → Cel → marcação de cobranças órfãs → eventual C/Ch. Cartório vem depois do atomic local; continua no mesmo atomic quando há o exterior da troca. (`app/ponto/portas/celula.py:592`; `app/ponto/portas/celula.py:626`; `app/ponto/management/commands/gerar_celulas.py:255`; `app/chamados/services/contexto.py:103`; `app/chamados/models.py:486`; `app/ponto/portas/celula.py:627`; `app/ponto/portas/celula.py:652`) |
| **Au — trocar plantão** (`app/ponto/portas/celula.py:657`) | Atomic exterior → cartas Au por PK → edições por colaborador/data → Cel → eventual C/Ch → julgamento/fábrica/DP. Chamadores externos encontrados em testes; chamador produtivo **[nao verificado]**. (`app/ponto/portas/celula.py:652`; `app/ponto/portas/celula.py:658`; `app/ponto/portas/celula.py:673`; `app/ponto/portas/celula.py:675`; `app/ponto/tests/test_o10b_editar_dia_troca.py:116`) |

| Advisory | Ordem, duração e alcance |
|---|---|
| **Âncora E4B: bloqueante, transacional** (`app/chamados/services/fecho_runtime.py:89`) | SHA256 de marcador/tipo/âncora; atomic do agrupador → todas A\* → dedup → escrita. Quando aninhado no adaptador, integra o ato exterior. (`app/chamados/services/fecho_runtime.py:86`; `app/chamados/services/fecho_runtime.py:119`; `app/chamados/services/fecho_runtime.py:120`; `app/chamados/services/fecho_runtime.py:123`; `app/chamados/services/fecho_runtime.py:147`; `app/chamados/services/fecho_runtime.py:154`) |
| **Vigia de hora: sessão, tentativa sem espera** (`app/ponto/services/vigia_de_hora.py:99`) | Chave por empresa → se não obtém, retorna → julgamentos → unlock. Não há atomic abrangendo todo esse laço; não constitui espera por advisory no ciclo. (`app/ponto/services/vigia_de_hora.py:97`; `app/ponto/services/vigia_de_hora.py:100`; `app/ponto/services/vigia_de_hora.py:131`; `app/ponto/services/vigia_de_hora.py:136`) |
| **Consumidor: sessão, tentativa sem espera** (`app/chamados/reconciliador.py:1235`) | Chave derivada do schema; atos do consumidor separados, sem atomic geral por colaborador; unlock ao sair do contexto. (`app/chamados/reconciliador.py:1221`; `app/chamados/reconciliador.py:1242`; `app/chamados/reconciliador.py:1364`; `app/chamados/reconciliador.py:1368`; `app/chamados/reconciliador.py:1392`) |

| Escrita que também adquire linha | Posição na sequência |
|---|---|
| **ContadorPauta** (`app/pautas/services.py:352`; `app/pautas/services.py:354`) | A aquisição relevante não é `select_for_update`: é criação concorrente, quando ausente, ou UPDATE da linha do alvo. DP é único por departamento. (`app/pautas/models.py:167`; `app/pautas/services.py:352`; `app/pautas/services.py:354`) |
| **Chamado** (`app/chamados/models.py:296`) | `_gravar_estado` toma C/D/Ch, registra trilha e salva; o save executa os receivers antes da relavra S2. (`app/chamados/models.py:290`; `app/chamados/models.py:292`; `app/chamados/models.py:296`; `app/chamados/models.py:307`) |
| **Pergunta** (`app/chamados/models.py:3152`; `app/chamados/veredito_lavrado.py:275`) | S143 pode carimbar Q antes do cartório; S2 pode atualizar Q **depois** da Pauta da fábrica. `carimbar`/`save` não demonstram, por si, aquisição prévia de C. (`app/chamados/services/disputa_emissao.py:1984`; `app/chamados/models.py:305`; `app/chamados/models.py:312`; `app/chamados/models.py:3422`; `app/chamados/models.py:3423`) |
| **Disputa** (`app/chamados/models.py:1936`) | Porta de fecho toma a ordem antes do save; o receiver S88 pode reconciliar o chamado-pai e o S144 pode materializar pendentes. (`app/chamados/models.py:1910`; `app/chamados/signals.py:42`; `app/chamados/signals.py:164`) |
| **CelulaDia** (`app/ponto/portas/celula.py:905`) | No cartório: lavra em memória → `carimbar` → atomic/save Cel → EventoCelula; fábrica vem depois. (`app/ponto/services/cartorio.py:1825`; `app/ponto/services/cartorio.py:1845`; `app/ponto/portas/celula.py:1072`; `app/ponto/portas/celula.py:904`; `app/ponto/portas/celula.py:907`; `app/ponto/services/cartorio.py:1020`) |
| **TM** (`app/ponto/nucleo.py:81`) | Dentro de atomic: DELETE do recorte → bulk INSERT. No save de Batida, o receiver do núcleo precede o cartório, podendo conservar TM antes de posterior C/DP quando há atomic exterior. (`app/ponto/nucleo.py:57`; `app/ponto/nucleo.py:58`; `app/ponto/nucleo.py:59`; `app/ponto/signals.py:36`; `app/ponto/signals.py:135`) |
| **FM: invalidação síncrona** (`app/ponto/services/fechamento.py:1656`) | Aprovar/materializar justificativa pode passar por cartório/DP e depois invalidar FM no mesmo atomic. Isso é distinto do recálculo por evento em `on_commit`. (`app/ponto/services/justificativa.py:208`; `app/ponto/services/justificativa.py:215`; `app/ponto/services/justificativa.py:386`; `app/ponto/services/justificativa.py:390`; `app/ponto/services/fechamento.py:1824`) |
| **FM e DiaPago: passivo** (`app/ponto/portas/regen_exportada.py:179`; `app/ponto/portas/regen_exportada.py:188`) | Atomic do par → plantio → regeneração, que pode julgar/escrever DP → recálculo FM → retrato DiaPago → Pauta final do passivo. (`app/chamados/services/passivo_validado.py:371`; `app/chamados/services/passivo_validado.py:428`; `app/chamados/services/passivo_validado.py:438`; `app/ponto/portas/celula.py:434`; `app/chamados/services/passivo_validado.py:443`) |
| **PA: agendamento novo/histórico** (`app/ferias/services.py:302`; `app/ferias/services.py:326`) | Save de novo Ag → sync de Ausencia → eventual DP → UPDATE PA, ainda dentro do atomic. (`app/ferias/services.py:277`; `app/ferias/services.py:298`; `app/ferias/services.py:315`; `app/ferias/services.py:321`; `app/ferias/models.py:451`; `app/ponto/services/ausencia.py:1245`) |
| **Pauta existente** (`app/pautas/services.py:215`; `app/pautas/services.py:247`; `app/pautas/services.py:272`; `app/pautas/services.py:294`) | Ler/fazer/fazer pelo sistema/cancelar: UPDATE Pauta → trilha → contador destinatário. (`app/pautas/services.py:218`; `app/pautas/services.py:251`; `app/pautas/services.py:276`; `app/pautas/services.py:297`) |
| **Au: alterações e exclusões** (`app/ponto/services/ausencia.py:835`; `app/ponto/services/ausencia.py:1190`; `app/ponto/services/ausencia.py:1281`) | Save/delete podem alcançar cartório pelos respectivos signals. O UPDATE em massa de rejeição de férias não emite `post_save`; o save posterior de Ag pode excluir Au e emitir `post_delete`. (`app/ponto/signals.py:147`; `app/ponto/signals.py:152`; `app/ferias/services.py:417`; `app/ferias/models.py:451`) |

| Sites de `select_for_update` separados do alcance DP no mesmo atomic lido | Motivo da separação |
|---|---|
| HE, recusar ponta/revogar — dois sites (`app/ponto/portas/he.py:318`; `app/ponto/portas/he.py:445`) | Chamadores próprios encontrados; não percorrem a Pauta do ato de período. (`app/ponto/management/commands/recusar_ponta_pequena.py:170`; `app/ponto/management/commands/lavrar_he_pendente.py:59`) |
| AplicacaoConvencao — um site (`app/core/services/aplicacao_convencao.py:115`) | Atomic lido grava cadastro/trilha e retorna; encadeamento adicional até Pauta **[nao verificado]**. (`app/core/services/aplicacao_convencao.py:100`; `app/core/services/aplicacao_convencao.py:124`; `app/core/services/aplicacao_convencao.py:134`) |
| Lote de holerite — dois sites (`app/holerite/management/commands/processar_lote_holerite.py:54`; `app/holerite/management/commands/processar_lote_holerite.py:124`) | Atomics de estado do lote; motor chamado fora deles. Alcance DP dentro dessas aquisições **[nao verificado]**. (`app/holerite/management/commands/processar_lote_holerite.py:89`) |

| Sequência da transição | Até a saída do atomic |
|---|---|
| **E — ramo com agrupador E4B** (`app/chamados/services/fecho_runtime.py:147`) | Atomic exterior → `transicionar` → atomic de `_gravar_estado` → C/D/Ch → UPDATE Ch → receivers abaixo → eventual S2/Q → retorno da transição → A\* → dedup → P/DP → saída do atomic exterior. (`app/chamados/services/fecho_runtime.py:151`; `app/chamados/models.py:280`; `app/chamados/models.py:290`; `app/chamados/models.py:296`; `app/chamados/models.py:307`; `app/chamados/services/fecho_runtime.py:154`; `app/chamados/services/fecho_runtime.py:120`; `app/chamados/services/fecho_runtime.py:127`) |
| **N — passagem sem agrupador** (`app/chamados/services/fecho_runtime.py:144`) | `transicionar` → atomic de `_gravar_estado` → C/D/Ch → UPDATE Ch → receivers → eventual S2/Q → saída; não adquire A por esse adaptador. Pode escrever DP pelo cartório. (`app/chamados/services/fecho_runtime.py:145`; `app/chamados/models.py:280`; `app/chamados/models.py:290`; `app/chamados/models.py:296`; `app/chamados/models.py:307`; `app/chamados/signals.py:217`) |
| **DP anterior às âncoras, em E** (`app/chamados/services/fecho_runtime.py:151`) | Possível se o cartório chegar a cadastro sem turno ou limbo exportado: P/DP ocorre durante o save, antes do retorno que chama A\*. (`app/chamados/signals.py:217`; `app/ponto/services/cartorio.py:1020`; `app/chamados/services/fabrica_da_celula.py:204`; `app/chamados/services/fabrica_da_celula.py:215`; `app/chamados/services/fecho_runtime.py:154`) |
| **Condições para aquela emissão** (`app/chamados/services/fabrica_da_celula.py:201`) | Cadastro: célula passada, colaborador ativo, dia não dispensado e teto sem turno; limbo: motivos, `trabalha=None` e competência entregue. Se dedup já cobre o caso, não há nova Pauta. (`app/chamados/services/fabrica_da_celula.py:203`; `app/chamados/services/fabrica_da_celula.py:228`; `app/chamados/services/fabrica_da_celula.py:231`; `app/chamados/services/fabrica_da_celula.py:277`; `app/chamados/services/fabrica_da_celula.py:367`) |
| **Alcance das 20 arestas humanas abaixo** (`app/chamados/catalogo/motor.py:139`) | Dois callsites produtivos: detalhe e fio; dependem de permissão e guards do modelo. Exercício real de cada aresta **[nao verificado]**. (`app/chamados/services/acoes_chamado.py:313`; `app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:714`; `app/chamados/services/fio.py:722`; `app/chamados/models.py:132`; `app/chamados/models.py:148`; `app/chamados/models.py:165`; `app/chamados/models.py:181`) |

| De ↓ / para → | aberto | em_analise | resolvido | fechado | registrado |
|---|---|---|---|---|---|
| **aberto** (`app/chamados/catalogo/motor.py:100`) | Recusa: igual (`app/chamados/catalogo/motor.py:148`) | **N**, comum (`app/chamados/catalogo/motor.py:100`) | **E**, comum (`app/chamados/catalogo/motor.py:100`) | **E**, comum (`app/chamados/catalogo/motor.py:100`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:153`) |
| **em_analise** (`app/chamados/catalogo/motor.py:101`) | **N**, comum (`app/chamados/catalogo/motor.py:101`) | Recusa: igual (`app/chamados/catalogo/motor.py:148`) | **E**, comum (`app/chamados/catalogo/motor.py:101`) | **E**, comum (`app/chamados/catalogo/motor.py:101`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:153`) |
| **resolvido** (`app/chamados/catalogo/motor.py:102`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:153`) | **N**, comum (`app/chamados/catalogo/motor.py:102`) | Recusa: igual (`app/chamados/catalogo/motor.py:148`) | **N**, comum (`app/chamados/catalogo/motor.py:102`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:153`) |
| **fechado** (`app/chamados/catalogo/motor.py:103`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:150`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:150`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:150`) | Recusa: igual (`app/chamados/catalogo/motor.py:148`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:150`) |
| **registrado** (`app/chamados/catalogo/motor.py:105`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:153`) | **N**, comum (`app/chamados/catalogo/motor.py:105`) | **E**, admin_ti (`app/chamados/catalogo/motor.py:153`) | **E**, comum (`app/chamados/catalogo/motor.py:105`) | Recusa: igual (`app/chamados/catalogo/motor.py:148`) |

| Complemento da matriz humana | Resultado |
|---|---|
| `cancelado` e `superado` como origem ou destino humano | Recusados, inclusive para admin_ti. (`app/chamados/catalogo/motor.py:142`; `app/chamados/catalogo/motor.py:145`) |
| Total humano | **20 arestas**: 10 da tabela comum e outras 10 liberadas a admin_ti; **6 E e 14 N** pela condição VIVOS→ENCERRADOS. (`app/chamados/catalogo/motor.py:99`; `app/chamados/catalogo/motor.py:153`; `app/chamados/services/fecho_runtime.py:144`) |
| Relavra S2 | Ocorre quando cruza a fronteira ENCERRADOS e existe alvo; portanto não é um passo incondicional de todas as 20 arestas. (`app/chamados/models.py:273`; `app/chamados/models.py:303`; `app/chamados/models.py:305`) |

| Receiver/subcaminho síncrono | Sequência adicional dentro do ato |
|---|---|
| **Rebaixamento de urgência** (`app/chamados/signals.py:54`) | Destino encerrado → ritual de rebaixamento; não adquire âncora E4B. (`app/chamados/signals.py:70`; `app/chamados/signals.py:72`) |
| **S143** (`app/chamados/signals.py:91`) | Somente save de status encerrado → savepoint → resolver retoma C/D/Ch → tenta devolver alcance antes de carimbar → eventuais Q, reabertura de pai e fecho de D. (`app/chamados/signals.py:96`; `app/chamados/signals.py:104`; `app/chamados/signals.py:115`; `app/chamados/services/disputa_emissao.py:1878`; `app/chamados/services/disputa_emissao.py:1945`; `app/chamados/services/disputa_emissao.py:1984`; `app/chamado

[truncado em 20000 de 48666 caracteres]

## 11/11 15:42 · sessao 4a33ac90e100 · DATA-EVENTO-CRU · verificador · origem=Code

**Veredito: BLOQUEANTES.** Os casos específicos da volta 1 foram corrigidos, mas ainda há destinos de escrita contabilizados como leitores. (`app/chamados/tests/test_catraca_data_evento_cru.py:57`; `app/chamados/tests/test_catraca_data_evento_cru.py:127`; `app/chamados/services/fio.py:801`)

1. **Censo confere:** recalculei em memória **216 ocorrências em 120 escopos: 211 Python + 5 templates**, sem diferença para o JSON. `vigia_plantio_overnight` entrou com **3**; `fio.py::<modulo>` entrou com **2**, pelas constantes guardadas nas linhas 1373/1378. (`app/chamados/tests/test_catraca_data_evento_cru.py:152`; `app/chamados/tests/catraca_data_evento_cru.json:2`; `app/chamados/tests/catraca_data_evento_cru.json:119`; `app/chamados/tests/catraca_data_evento_cru.json:116`; `app/chamados/tests/catraca_data_evento_cru.json:68`; `app/chamados/services/fio.py:1373`; `app/chamados/services/fio.py:1378`)

2. **As exclusões pedidas funcionam:** `PerguntaDisputa.Meta`, `abrir` e `abrir_conversa` saíram. As portas `abrir`/`abrir_conversa`/`_sob_contrato`/`_nascer` suprimem o destino; `UniqueConstraint` suprime a subárvore inteira, incluindo os dois `Q`. As três leituras reais dentro de `_nascer` permanecem. (`app/chamados/tests/test_catraca_data_evento_cru.py:57`; `app/chamados/tests/test_catraca_data_evento_cru.py:120`; `app/chamados/tests/test_catraca_data_evento_cru.py:122`; `app/chamados/models.py:3486`; `app/chamados/models.py:3492`; `app/chamados/models.py:2940`; `app/chamados/models.py:2956`; `app/chamados/models.py:2962`)

3. **Bloqueante — escrita ainda conta em pelo menos cinco ocorrências:**

   - `gravar_dia_do_fato` conta **1** pelo `return 'data_evento'`; esse retorno nomeia o campo de `update_fields`, após a atribuição. (`app/chamados/tests/catraca_data_evento_cru.json:79`; `app/chamados/services/fio.py:800`; `app/chamados/services/fio.py:801`; `app/chamados/services/fio.py:1037`; `app/chamados/services/fio.py:1043`)
   - Quatro repasses `reapontar_resolvedora(..., data_evento=...)` continuam contando porque essa porta falta em `ESCRITA`. São pedidos de escrita; a porta atribui por `gravar_dia_do_fato` e salva. (`app/chamados/services/declaracao_texto.py:370`; `app/chamados/reconciliador.py:954`; `app/chamados/reconciliador.py:1642`; `app/chamados/reconciliador.py:1655`; `app/chamados/tests/test_catraca_data_evento_cru.py:57`; `app/chamados/tests/test_catraca_data_evento_cru.py:127`; `app/chamados/services/fio.py:879`; `app/chamados/services/fio.py:927`)

4. **Há também três ocorrências sem leitura do valor cru:** `PerguntaDisputa:1` vem de `CAMPOS_QUE_ACORDAM`, confrontado com nomes de `update_fields`; as duas de `hub_smoke` são nomes de parâmetros. Estas últimas são uma sobrecontagem expressamente admitida pelo varredor. (`app/chamados/tests/catraca_data_evento_cru.json:39`; `app/chamados/models.py:3221`; `app/chamados/signals.py:255`; `app/escala/management/commands/hub_smoke.py:35`; `app/escala/management/commands/hub_smoke.py:51`; `app/chamados/tests/test_catraca_data_evento_cru.py:22`)

5. **Leitor real novo omitido pelas exclusões novas:** não localizei na árvore examinada. As chaves literais encontradas são payload/trilha; o literal sob `fields=` é schema. Há, porém, regressão reproduzida em memória: `M.objects.create(x=p['data_evento'])` resulta em `{}`, pois `escrita=True` também suprime a chave do subscrito **lido**. Ocorrência real dessa forma na cópia: **[nao verificado]**. (`app/api/views.py:1165`; `app/chamados/services/materializacao.py:578`; `app/chamados/services/fio.py:1047`; `app/chamados/models.py:3483`; `app/chamados/tests/test_catraca_data_evento_cru.py:102`; `app/chamados/tests/test_catraca_data_evento_cru.py:123`; `app/chamados/tests/test_catraca_data_evento_cru.py:139`)

Os quatro métodos sem escrita passaram executados com base `unittest` em memória; isso não comprova execução da suíte Django, **[nao verificado]**. O teste que cria arquivos temporários não foi executado. (`app/chamados/tests/test_catraca_data_evento_cru.py:173`; `app/chamados/tests/test_catraca_data_evento_cru.py:186`; `app/chamados/tests/test_catraca_data_evento_cru.py:191`; `app/chamados/tests/test_catraca_data_evento_cru.py:226`; `app/chamados/tests/test_catraca_data_evento_cru.py:234`)

## 11/11 15:41 · sessao 331c29b2b46f · - · - · origem=Ronald (tmux)

Há falsos positivos de escrita remanescentes:

- `gravar_dia_do_fato` entra com 1 no JSON; a ocorrência é `return 'data_evento'`, nome devolvido para `save(update_fields=...)`, após atribuir o campo. (`app/chamados/tests/catraca_data_evento_cru.json:79`; `app/chamados/services/fio.py:800`; `app/chamados/services/fio.py:801`; `app/chamados/services/fio.py:1037`; `app/chamados/services/fio.py:1043`)
- Quatro repasses `reapontar_resolvedora(..., data_evento=...)` ainda contam como leitura: a porta falta em `ESCRITA`, e qualquer keyword correspondente fora dessa lista incrementa o censo. A porta recebe o pedido, consulta o juiz, atribui e salva. (`app/chamados/services/declaracao_texto.py:370`; `app/chamados/reconciliador.py:954`; `app/chamados/reconciliador.py:1642`; `app/chamados/reconciliador.py:1655`; `app/chamados/tests/test_catraca_data_evento_cru.py:57`; `app/chamados/tests/test_catraca_data_evento_cru.py:127`; `app/chamados/services/fio.py:876`; `app/chamados/services/fio.py:879`; `app/chamados/services/fio.py:927`)
- `PerguntaDisputa` entra com 1 por `CAMPOS_QUE_ACORDAM`: metadado de nomes confrontado com `update_fields`, sem ler o valor cru nessa ocorrência. (`app/chamados/tests/catraca_data_evento_cru.json:39`; `app/chamados/models.py:3221`; `app/chamados/signals.py:255`)
- Os dois escopos novos de `hub_smoke` contam nomes de parâmetros, conforme limite expressamente declarado pelo próprio varredor. (`app/chamados/tests/catraca_data_evento_cru.json:112`; `app/chamados/tests/catraca_data_evento_cru.json:113`; `app/escala/management/commands/hub_smoke.py:35`; `app/escala/management/commands/hub_smoke.py:51`; `app/chamados/tests/test_catraca_data_evento_cru.py:22`)

`_nascer` mantém três leituras reais; retirar a função inteira seria incorreto. (`app/chamados/models.py:2940`; `app/chamados/models.py:2956`; `app/chamados/models.py:2962`; `app/chamados/tests/catraca_data_evento_cru.json:40`)

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

_3978 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
