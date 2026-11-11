# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 15:59** · publicado em **11/11/2026 16:02** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 11/11 15:38 · sessao 5e6e5744386e · TRIAGEM-OFFLINE-SEM-CAMERA · verificador · origem=Code

**APROVADO para o delta no caminho com `JulianiPWA`.** Os tetos de armazenamento/POST e a prova de conclusão da transação foram introduzidos; permanecem as limitações discriminadas abaixo. (`app/static/pwa.js:55`, `app/static/pwa.js:80`, `app/static/pwa.js:230`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:142`)

1. **(a), (b) e (d): curados nesse caminho.** Abertura tem teto de 2 s e rejeição por bloqueio; transação tem teto, `onabort` e resolução somente em `oncomplete`; o gap tem envelope externo de 4 s. POST e corpo de erro compartilham o orçamento de 30 s. As telas capturam a rejeição da gravação e mostram erro. (`app/static/pwa.js:60`, `app/static/pwa.js:74`, `app/static/pwa.js:84`, `app/static/pwa.js:91`, `app/static/pwa.js:213`, `app/static/pwa.js:232`; `app/templates/ponto/triagem/apto.html:308`; `app/templates/ponto/bater_ponto_v2.html:329`)

   Os estados residuais têm consequências diferentes:

   - **Sync/indicador pendurados:** continuam com pedidos/transações fora de `_transacao`; fetch e corpos do sync também continuam sem teto. Isso pode paralisar envio/indicador, mas não desfaz a fila nem segura o retorno do escritor, que dispara o indicador sem `await`. (`app/static/pwa.js:223`, `app/static/pwa.js:248`, `app/static/pwa.js:271`, `app/static/pwa.js:277`, `app/static/pwa.js:306`)
   - **Sync recebe HTTP 400 e conclui a exclusão:** esse caminho deixa a fila vazia com apenas `console.warn`, sem erro visível. É uma perda silenciosa ainda presente; já consta como pendência própria na cópia. (`app/static/pwa.js:284`; `app/docs/BACKLOG.md:1944`)
   - **`tx.abort()` lança durante commit:** a promessa rejeita mesmo assim, portanto há erro visível. Porém o código não confirma rollback: pode haver gravação tardia após o erro. A ocorrência real desse estado é [nao verificado]. (`app/static/pwa.js:84`; `app/templates/ponto/bater_ponto_v2.html:329`)
   - **Conexão chega tarde:** a abertura já rejeitou; o sucesso tardio fecha a conexão e não retoma aquele escritor. (`app/static/pwa.js:60`, `app/static/pwa.js:72`, `app/static/pwa.js:215`)
   - **Sem `postarBatidaNoTeto`:** permanece `fetch/json` cru; uma promessa pendente deixa botão desabilitado, sem fila e sem erro. O ensaio explicitamente trata ausência de PWA como pendência própria, exigindo apenas preservar o envio online. (`app/templates/ponto/bater_ponto_v2.html:294`, `app/templates/ponto/bater_ponto_v2.html:312`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:552`)

   **(d)** agora marca a fila na conclusão, desfaz linhas no aborto e exige promessa resolvida mais exatamente uma linha persistida. Commit em curso e abertura tardia não são modelados pelo falso IndexedDB. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:142`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:203`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:605`)

2. **(c): permanece sem identidade original, mas isso sozinho não sustenta bloqueante no reenvio de 30 s.** Um clique online pode produzir o POST inicial e uma linha de fila; essa linha pode gerar **N retransmissões**, sem limite declarado, enquanto respostas/exclusões falharem. Quantidade de requisições não equivale à quantidade de batidas gravadas. (`app/templates/ponto/bater_ponto_v2.html:307`, `app/templates/ponto/bater_ponto_v2.html:323`; `app/static/pwa.js:256`, `app/static/pwa.js:276`, `app/static/pwa.js:295`)

   A porta deduplica **mesmo colaborador/mesmo tipo, ±60 s inclusivos**, entre não retratadas. A API usa **mesmo colaborador/mesmo tipo, ±5 minutos inclusivos**, sobre `ts_efetivo`; não sobre a chegada quando existe hora válida do aparelho. (`app/ponto/registro_batida.py:106`; `app/api/views.py:519`, `app/api/views.py:539`)

   Portanto, reenvio **sequencial**, primeira gravação já visível, mesmo tipo e instantes separados por até 30 s é absorvido. Garantia universal permanece [nao verificado]: o teto limita espera da resposta, não o prazo da sincronização; além disso, a dedup precede a eventual correção de tipo. Assim, identidade original fica como pergunta de desenho, sem duplicação demonstrada pelos anexos. (`app/static/pwa.js:230`, `app/static/pwa.js:244`; `app/api/views.py:539`, `app/api/views.py:582`)

3. **Não encontrei nova barreira nem segundo juiz da selfie nesses caminhos.** A lei fixa escrita em `registro_batida.py`; as telas consultam o juiz central e seguem com foto nula. No servidor, `selfie_ausente` acrescenta motivo de revisão, sem recusar a batida. (`CLAUDE.md:143`; `app/static/pwa.js:170`; `app/templates/ponto/triagem/apto.html:248`, `app/templates/ponto/triagem/apto.html:275`; `app/templates/ponto/bater_ponto_v2.html:269`; `app/api/views.py:449`)

4. **O instante único é a entrada do escritor, não necessariamente o primeiro clique.** `Date.now()` alimenta os dois timestamps antes das esperas; na recuperação online, o escritor só é chamado depois do POST frustrado, potencialmente 30 s depois. (`app/static/pwa.js:211`; `app/templates/ponto/bater_ponto_v2.html:293`, `app/templates/ponto/bater_ponto_v2.html:323`)

   O sync envia o timestamp **cru do aparelho**, sem enviar o timestamp corrigido/gap. A API o entrega a `hora_do_aparelho`: atraso entre 0 e 48 h preserva o instante; acima de 3 minutos acrescenta trilha; futuro ou antigo demais usa `agora`. Logo, 30 s de atraso, com relógio compatível, é aceito. O teto de POST não garante sincronização dentro de 30 s. (`app/static/pwa.js:265`; `app/api/views.py:519`; `app/api/hora_do_aparelho.py:62`, `app/api/hora_do_aparelho.py:141`)

5. **Nenhuma das quatro ausências de mutante demonstra, isoladamente, bloqueante de produto.** Fechamento tardio protege conexões futuras; catch interno do gap está protegido, para a fila, pelo envelope externo; catch do indicador é posterior à gravação e sem `await`; instante único está implementado, mas sem prova de regressão temporal. Os mutantes existentes cobrem outras guardas, não essas quatro. (`app/static/pwa.js:72`, `app/static/pwa.js:134`, `app/static/pwa.js:166`, `app/static/pwa.js:212`, `app/static/pwa.js:223`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:810`)

6. **O template apagado não tem referência executável encontrada na cópia.** Restam comentário do guard, comentário histórico do teste e tabela documental. URLs apontam à view, que escolhe v2 ou triagem. O SW referencia `/ponto/bater/`, não o nome do template; seu ramo de navegação retorna antes do trecho que alimentaria esse cache. (`app/scripts/check_a_lei_batida.py:20`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:673`; `app/docs/TICKETS.md:693`; `app/ponto/urls.py:11`; `app/ponto/views.py:225`, `app/ponto/views.py:356`; `app/static/sw.js:45`, `app/static/sw.js:67`)

   Há lacunas nos instrumentos alterados: o scanner não inclui apto/v2; o guard não inclui v2 e pula caminhos inexistentes. Elas não demonstram quebra causada pela exclusão. (`app/tools/check_frontend_urls.py:25`; `app/scripts/check_a_lei_batida.py:14`, `app/scripts/check_a_lei_batida.py:21`, `app/scripts/check_a_lei_batida.py:31`)

Suíte verde e smoke nesta cópia: **[nao verificado]**. Não executei o ensaio Chromium porque ele cria arquivos/perfil e depois os apaga. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:344`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:360`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:372`)

## 11/11 15:38 · sessao 86de3be02df9 · LASTRO-FOLGA-FORA-DO-CATALOGO · verificador · origem=Code

**Veredito: BLOQUEANTES B1–B3.** A guarda consulta a autoridade existente, mas herda um cache sem isolamento por tenant, introduz uma exceção sem tradução em portas HTTP e permite reintroduzir legado numa corrida entre leitura e gravação (`app/ponto/catalogo/ausencias.py:257`, `app/ponto/models.py:883`, `app/ponto/services/ausencia.py:418`, `app/api/views.py:1825`).

**1. Quem escreve `Ausencia.tipo`**

A construção e persistência estão em `criar_ausencia`; a troca do campo está em `editar_ausencia_registro`. Ambas validam pelo catálogo antes de salvar (`app/ponto/services/ausencia.py:194`, `app/ponto/services/ausencia.py:342`, `app/ponto/services/ausencia.py:418`, `app/ponto/services/ausencia.py:474`, `app/ponto/services/ausencia.py:498`, `app/ponto/services/ausencia.py:509`).

| Chamador produtivo encontrado | Valor fornecido |
|---|---|
| Lançamento administrativo | Variável do formulário (`app/ponto/views.py:2415`) |
| Solicitação PWA | Variável da solicitação (`app/ponto/views.py:2536`) |
| API | Variável, após tradução dos aliases legados (`app/api/views.py:1765`, `app/api/views.py:1817`) |
| Copiloto | Variável encaminhada ao produtor (`app/ponto/services/checar_ausencia.py:46`) |
| Veredito da célula | Literal `falta` ou variável `tipo_ausencia` (`app/ponto/services/veredito_celula.py:332`) |
| Materialização de declaração | `falta` ou `atestado`, pelo mapa (`app/chamados/services/materializacao.py:490`, `app/chamados/models.py:3716`) |
| Materialização de resposta | `falta` ou `atestado`, pela tabela de escapes (`app/chamados/services/materializacao.py:526`, `app/chamados/services/materializacao.py:602`) |
| Espelho de férias | Literal `ferias`, pelo produtor (`app/ponto/services/ausencia.py:1202`) |
| Regularização de retorno | Literal `falta`, por `garantir_ausencia` (`app/ponto/services/regularizacao.py:118`, `app/ponto/services/ausencia.py:461`) |
| Comando de abandono provisório | Literal `falta`, por `garantir_ausencia` (`app/ponto/management/commands/marcar_abandono_provisorio.py:76`) |
| Edição pela UI | POST ou tipo atual, pela porta de edição (`app/ponto/views.py:2794`) |
| Comando de rederivação H06 | Tipo atual, pela porta de edição (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`) |

Os literais efetivamente produzidos pelos caminhos fixos encontrados estão semeados:

| Literal | Semente |
|---|---|
| `falta` | Sim (`app/ponto/migrations/0052_seed_tipoausencia.py:10`) |
| `atestado` | Sim (`app/ponto/migrations/0052_seed_tipoausencia.py:17`) |
| `ferias` | Sim (`app/ponto/migrations/0052_seed_tipoausencia.py:23`) |
| `declaracao`, destino do alias `atestado_acompanhante` | Sim (`app/api/views.py:1766`, `app/ponto/migrations/0052_seed_tipoausencia.py:18`) |
| `licenca_luto`, destino do alias `obito` | Sim (`app/api/views.py:1767`, `app/ponto/migrations/0052_seed_tipoausencia.py:20`) |

`atestado_medico` é traduzido para `atestado`; os aliases não são escritos literalmente pela API (`app/api/views.py:1765`). Não há incompatibilidade demonstrada entre esses literais e a semente; estarem ativos hoje em produção é **[não verificado]**.

O admin de Ausência bloqueia adicionar, alterar e apagar; os receivers de Ausência consomem o fato, sem atribuir `tipo` (`app/ponto/admin.py:38`, `app/core/admin.py:59`, `app/core/admin.py:62`, `app/core/admin.py:65`, `app/ponto/signals.py:143`, `app/ponto/signals.py:275`). O `update` produtivo de cancelamento de férias altera somente status e carimbo (`app/ponto/services/ausencia.py:1281`). A cópia CSV encontrada contorna o modelo, mas pertence ao pipeline de ensaio (`bin/sombra_incremental.sh:58`, `bin/sombra_incremental.sh:62`).

**O escritor histórico de #5356 permanece [não verificado].** Os testes demonstram que `QuerySet.update(tipo=...)` consegue fabricar esse legado, mas isso não identifica a origem produtiva (`app/ponto/tests/test_lastro_folga_save_guarda.py:67`).

**2. Catálogo, cache e cadastro pela UI — B1**

Os leitores diretos de `codigos_validos()` são formulário, prévia do copiloto, criação, edição, composição dos tipos que exigem documento, vigia, solicitação PWA e agora a guarda do modelo (`app/ponto/forms.py:63`, `app/ponto/services/checar_ausencia.py:23`, `app/ponto/services/ausencia.py:194`, `app/ponto/services/ausencia.py:474`, `app/ponto/services/ausencia.py:804`, `app/ponto/services/vigia_ausencia.py:103`, `app/ponto/views.py:2509`, `app/ponto/models.py:879`).

O cache é um único dicionário de módulo, com TTL de **60 segundos medidos por `monotonic()`**. A criação/edição pela UI salva `TipoAusencia`, cujo signal invalida esse dicionário no processo local (`app/ponto/catalogo/ausencias.py:257`, `app/ponto/catalogo/ausencias.py:258`, `app/ponto/catalogo/ausencias.py:275`, `app/ponto/views_tipos_ausencia.py:94`, `app/ponto/services/ausencia.py:1310`, `app/ponto/models.py:1260`).

Consequências:

- No processo invalidado, a próxima leitura consulta novamente o banco; outro processo com cache quente pode recusar o tipo recém-criado até expirar o TTL. Também pode continuar aceitando um tipo desativado nesse intervalo (`app/ponto/catalogo/ausencias.py:263`, `app/ponto/catalogo/ausencias.py:276`, `app/ponto/models.py:879`).
- **B1:** a chave não inclui tenant/schema. Com `ponto` instalado por tenant, uma leitura no tenant B pode reutilizar o catálogo anteriormente carregado no tenant A, admitindo código alheio ou recusando código próprio (`app/config/settings/saas.py:34`, `app/config/settings/saas.py:40`, `app/config/settings/saas.py:68`, `app/ponto/catalogo/ausencias.py:257`, `app/ponto/catalogo/ausencias.py:276`).
- Tabela vazia, todos os tipos inativos **ou qualquer exceção na consulta** fazem retornar a semente congelada. Portanto, vazio não significa “nenhum código permitido”; inclusive desativar todos reabilita os códigos da semente na autoridade (`app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:252`, `app/ponto/catalogo/ausencias.py:253`, `app/ponto/catalogo/ausencias.py:278`).

A guarda herda esses comportamentos de uma autoridade preexistente; ocorrência concreta entre workers/tenants de produção é **[não verificado]** (`app/ponto/models.py:878`).

**3. Consulta histórica, corrida e contornos — B3**

Com cache quente, código válido não exige a consulta histórica. Código fora do catálogo em instância persistida acrescenta **um SELECT por chamada de save**; cache frio acrescenta também a consulta do catálogo. Uma decisão pode chamar save duas vezes, pagando duas consultas históricas para legado (`app/ponto/models.py:879`, `app/ponto/models.py:882`, `app/ponto/catalogo/ausencias.py:249`, `app/ponto/services/ausencia.py:911`, `app/ponto/services/ausencia.py:927`). Latência e SQL efetivamente emitido são **[não verificado]**.

**B3 — comparação e gravação não são atômicas.** Interleaving permitido pelo código: A consulta e encontra `folga`; B corrige para `abono`; A termina seu save completo e recoloca `folga`. A guarda não usa lock nem UPDATE condicionado ao tipo observado (`app/ponto/models.py:883`, `app/ponto/models.py:885`, `app/ponto/models.py:933`). Ocorrência produtiva é **[não verificado]**.

Quanto aos contornos:

- `update_fields` **não desliga** a guarda. Mesmo salvando só documento/status, ela compara o tipo em memória com o banco; pode recusar uma operação que nem escreveria `tipo`, caso outra operação já tenha corrigido o legado (`app/ponto/models.py:891`, `app/ponto/models.py:893`, `app/ponto/services/ausencia.py:835`, `app/ponto/services/ausencia.py:859`).
- `QuerySet.update(tipo=...)` contorna a guarda, demonstrado expressamente pelas fixtures (`app/ponto/tests/test_lastro_folga_save_guarda.py:67`).
- O override não contém proteção em `save_base`, bulk ou SQL direto. A implementação dessas APIs Django não está demonstrada pelos arquivos examinados; confirmação de `save_base(raw=True)`/`loaddata`, `bulk_create` e `bulk_update` nesta cópia: **[não verificado]**. Não há exceção explícita para `raw` dentro da guarda (`app/ponto/models.py:868`, `app/ponto/models.py:891`).

**4. Exceção HTTP e batida — B2**

**B2 — a nova exceção não é traduzida consistentemente.** Na criação, somente o `ValidationError` do `full_clean` vira `AusenciaInvalida`; o save posterior limpa eventual anexo e relança a exceção original. A API captura apenas `AusenciaInvalida` (`app/ponto/services/ausencia.py:387`, `app/ponto/services/ausencia.py:418`, `app/ponto/services/ausencia.py:419`, `app/ponto/services/ausencia.py:429`, `app/api/views.py:1825`).

Cenário concreto permitido: o código passa na validação inicial; outra thread desativa o tipo e invalida o cache; o save consulta o catálogo atualizado e lança o novo `ValidationError`, que escapa da API. O caminho de edição tem a mesma janela e sua view também não captura essa exceção (`app/ponto/services/ausencia.py:194`, `app/ponto/models.py:1263`, `app/ponto/models.py:887`, `app/ponto/services/ausencia.py:474`, `app/ponto/services/ausencia.py:509`, `app/ponto/views.py:2818`). Resposta 500 observada em produção: **[não verificado]**.

Outros propagadores são veredito da célula, espelho de férias, mudanças de estado, decisão e anexação; nenhum deles traduz especificamente o novo erro do catálogo (`app/ponto/services/veredito_celula.py:340`, `app/ponto/services/ausencia.py:1209`, `app/ponto/services/ausencia.py:835`, `app/ponto/services/ausencia.py:927`, `app/ponto/services/ausencia.py:865`). Legado inalterado passa normalmente; a falha exige mudança concorrente ou outro desacordo entre instância e persistência (`app/ponto/models.py:885`).

Lançamento e submissão web têm captura externa de `Exception`, portanto não cabe afirmar 500 indistintamente nessas telas (`app/ponto/views.py:2458`, `app/ponto/views.py:2558`).

**Não foi demonstrado caminho da batida até `Ausencia.save`.** O chokepoint cria `Batida`; recompute, julgamento da célula, reconciliação e fechamento por evento têm contenção de exceções nos caminhos examinados (`app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:175`, `app/ponto/signals.py:19`, `app/ponto/signals.py:85`, `app/ponto/signals.py:131`, `app/ponto/signals.py:206`). Violação nova da lei “batida nunca barrada” por esta guarda: **[não verificado]**.

**5. Segundo juiz e dois significados**

A guarda é **leitora da mesma autoridade de validade**, pois chama `codigos_validos`, exatamente como criar e editar. `faixa_de` responde outra pergunta, a precedência; seu default continua sendo `atestado` para código desconhecido (`app/ponto/models.py:878`, `app/ponto/services/ausencia.py:194`, `app/ponto/services/ausencia.py:474`, `app/ponto/precedencia.py:76`).

A divergência estrutural com `TIPO_CHOICES` permanece: o campo declara choices congeladas, enquanto o serviço exclui `tipo` do `full_clean` para aceitar códigos vivos adicionais. A guarda não cria essa segunda lista; o teste novo preserva explicitamente código vivo fora das choices (`app/ponto/models.py:661`, `app/ponto/models.py:749`, `app/ponto/services/ausencia.py:387`, `app/ponto/tests/test_lastro_folga_save_guarda.py:134`).

**6. Fixtures e mordida**

As alterações preservam as asserções originais relevantes:

- Carimbo: mantém `obito` legado e exige `aprovada_em` na aprovação/rejeição (`app/ponto/tests/test_decisao_carimba.py:22`, `app/ponto/tests/test_decisao_carimba.py:32`, `app/ponto/tests/test_decisao_carimba.py:39`).
- Espelho: mantém `obito` e compara a apresentação com o juiz do status (`app/ponto/tests/test_caracterizacao_espelho.py:53`, `app/ponto/tests/test_caracterizacao_espelho.py:89`).
- Fechamento: mantém `atestado_medico` desconhecido e exige DSR perdido igual a zero (`app/ponto/tests/test_fechamento_ausencia_pela_autoridade.py:83`, `app/ponto/tests/test_fechamento_ausencia_pela_autoridade.py:86`).
- Cadastro/dropdown: acrescentam cleanup de cache e mantêm admissão de novo código, retirada do inativo e fallback vazio (`app/ponto/tests/test_cadastro_tipo.py:15`, `app/ponto/tests/test_cadastro_tipo.py:23`, `app/ponto/tests/test_cadastro_tipo.py:35`, `app/ponto/tests/test_dropdown_vivo.py:21`, `app/ponto/tests/test_dropdown_vivo.py:29`, `app/ponto/tests/test_dropdown_vivo.py:34`).

Há uma diferença: as fixtures agora disparam primeiro os efeitos de `abono` e depois os do legado. O segundo save executa os receivers, mas equivalência completa dos efeitos intermediários é **[não verificado]** (`app/ponto/tests/test_caracterizacao_espelho.py:50`, `app/ponto/tests/test_caracterizacao_espelho.py:55`, `app/ponto/signals.py:143`, `app/ponto/signals.py:275`).

O selo novo tem asserções que detectariam remoção da guarda e quebra da preservação histórica; não cobre os cenários B1–B3. Execução verde, RED anterior e mutantes executados são **[não verificado]** (`app/ponto/tests/test_lastro_folga_save_guarda.py:73`, `app/ponto/tests/test_lastro_folga_save_guarda.py:94`, `app/ponto/tests/test_lastro_folga_save_guarda.py:102`, `app/ponto/tests/test_lastro_folga_save_guarda.py:134`).

**7. Classes de reprovação**

Os bloqueantes são: **B1**, cache usado pela barreira sem isolamento por tenant e sem invalidação entre processos; **B2**, exceção nova sem tradução nas portas; **B3**, leitura histórica separada da gravação, permitindo reintrodução concorrente do código fora do catálogo (`app/ponto/catalogo/ausencias.py:257`, `app/ponto/models.py:1263`, `app/api/views.py:1825`, `app/ponto/models.py:883`, `app/ponto/models.py:933`).

Não foi demonstrado segundo produtor produtivo de `tipo`; há contorno por `update` nas fixtures e duplicidade preexistente entre choices e catálogo (`app/ponto/tests/test_lastro_folga_save_guarda.py:67`, `app/ponto/models.py:749`). O diff não acrescenta relógio, mas passa a depender do TTL preexistente na decisão de persistência (`app/ponto/models.py:878`, `app/ponto/catalogo/ausencias.py:275`). Origem de #5356, censo real da frota, suite completa e pré-requisitos de pouso permanecem **[não verificado]**.

## 11/11 15:37 · sessao d270727d20b6 · - · - · origem=Ronald (tmux)

Meu julgamento é **APROVADO no escopo da cura com `JulianiPWA`**, sem afirmar que todos os caminhos de rede/armazenamento ficaram limitados. A abertura rejeita no teto/bloqueio; a transação rejeita no teto/aborto; o POST e seu corpo de erro têm teto. (`app/static/pwa.js:60-74`; `app/static/pwa.js:83-94`; `app/static/pwa.js:230-241`)

- **Aborto lançando durante commit:** a exceção é engolida, mas a promessa rejeita mesmo assim e a tela mostra erro. Pode restar gravação tardia, contrariando a garantia de “nenhum fantasma”; não produz clique pendente ou perda silenciosa. Ocorrência real desse estado: [nao verificado]. (`app/static/pwa.js:84-93`; `app/templates/ponto/bater_ponto_v2.html:329-331`)
- **Conexão tardia:** a abertura já rejeitou; `onsuccess` tardio fecha a conexão. Não volta a executar a gravação daquele escritor. (`app/static/pwa.js:60-72`; `app/static/pwa.js:215-216`)
- **Sync/indicador:** os pedidos e transações continuam fora de `_transacao`, assim como o fetch/corpos do sync. Pendência neles não desfaz a fila já gravada nem segura o retorno do escritor. Há perda silenciosa explícita se o sync recebe HTTP 400 e a exclusão completa: só há `console.warn`. (`app/static/pwa.js:223`; `app/static/pwa.js:248-253`; `app/static/pwa.js:271-310`)
- **Exceção importante de (b):** sem `postarBatidaNoTeto`, a v2 continua fazendo `fetch` e `json` sem teto; resposta pendente deixa botão desabilitado, sem fila e sem erro. É caminho residual demonstrável pelo código. O próprio ensaio declara PWA ausente como outra fila e só exige preservar o envio online; portanto não o apresentaria como regressão nova desta cura. (`app/templates/ponto/bater_ponto_v2.html:294-295`; `app/templates/ponto/bater_ponto_v2.html:312-314`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:552-557`)
- **(c):** um clique pode gerar POST inicial e uma linha reenviável repetidamente. Isso não equivale a múltiplas linhas no servidor: reenvio sequencial do mesmo tipo, com instantes separados por 30 s e primeira gravação visível, cabe em ±5 min da API e ±60 s da porta. Falta de identidade isolada não sustenta bloqueante nesse cenário; garantia para relógio incompatível, correção de tipo ou concorrência permanece [nao verificado]. (`app/templates/ponto/bater_ponto_v2.html:307-323`; `app/static/pwa.js:256-298`; `app/api/views.py:539-555`; `app/ponto/registro_batida.py:106-115`)
- **(d)/mordidas:** o ensaio agora marca fila na conclusão e exige promessa resolvida/persistência, mas seu falso IDB não modela commit em curso nem fechamento tardio. Essas limitações impedem certificação universal; sozinhas não demonstram novo bloqueante de produto. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:142-153`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:195-215`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:605-616`)

## 11/11 15:37 · sessao b199bd514621 · - · - · origem=Ronald (tmux)

| Recurso adicional | Ordem demonstrada no código | Classificação do par |
|---|---|---|
| TurnoMaterializado | Passivo: atomic do par → materializador → INSERT Batida → receiver do núcleo → DELETE/INSERT TurnoMaterializado → cartório/fábrica → eventual C. (`app/chamados/services/passivo_validado.py:371`; `:427`; `:511`; `:516`; `app/chamados/services/materializacao.py:1220`; `:1007`; `app/ponto/registro_batida.py:157`; `app/ponto/signals.py:36`; `app/ponto/nucleo.py:58`; `:59`; `:81`; `app/ponto/signals.py:135`; `app/chamados/services/fabrica_da_celula.py:365`) | **Candidato TM→C contra C→TM**: aprovação de justificativa toma C antes de registrar Batida e recomputar os mesmos turnos. Compatibilidade das travas implícitas de FK e execução concorrente **[nao verificado]**. (`app/ponto/services/justificativa.py:140`; `:141`; `:129`; `:187`; `app/ponto/signals.py:36`; `app/ponto/nucleo.py:81`) |
| Justificativa | C/D/Ch → Justificativa FOR UPDATE → eventual Batida/TM → cartório/Pauta DP → UPDATE Justificativa → UPDATE FM. (`app/ponto/services/justificativa.py:129`; `:140`; `:142`; `:187`; `:208`; `:215`; `app/ponto/services/fechamento.py:1656`) | **Inversão abstrata DP→FM contra FM→DP**, mas a aprovação e o pago/devido adquirem C antes; para mesma linha FM, o mesmo colaborador serializa os dois atos. (`app/ponto/services/justificativa.py:129`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`; `:196`; `:207`) |
| Justificativa na materialização retroativa | Atomic → C/D/Ch → Justificativa FOR UPDATE → registrar Batida/TM → UPDATE Justificativa/cartório → UPDATE FM. (`app/ponto/services/justificativa.py:342`; `:343`; `:344`; `:361`; `:386`; `:390`; `app/ponto/services/fechamento.py:1656`) | Mesma ressalva de serialização por C para o par DP/FM. (`app/ponto/services/justificativa.py:129`; `:343`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`) |
| Ausencia | Aprovação de intervalo: atomic → UPDATE Ausencia → cartório síncrono → possível Pauta DP → invalidar FM. A porta da ausência não adquire C antes do UPDATE. (`app/ponto/views.py:2641`; `:2643`; `app/ponto/services/ausencia.py:911`; `:835`; `app/ponto/signals.py:143`; `:147`; `app/ponto/services/ausencia.py:946`; `app/ponto/services/fechamento.py:1656`) | **Candidato DP→FM sem C**, quando a fábrica segue por limbo exportado, que escreve DP diretamente; satisfatibilidade conjunta dos guards com aprovação parcial **[nao verificado]**. (`app/chamados/services/fabrica_da_celula.py:214`; `:228`; `:231`; `:279`; `app/ponto/services/ausencia.py:924`) |
| Ausencia em rejeição/cancelamento | A porta também invalida FM após rejeitar, mas as views lidas não envolvem o ato inteiro em atomic. (`app/ponto/views.py:2700`; `:2701`; `:2734`; `:2351`; `app/ponto/services/ausencia.py:946`) | **Não comprova retenção de DP até FM** nessas entradas. `Ausencia.save` termina em `super().save`, sem atomic próprio envolvendo a decisão inteira. (`app/ponto/models.py:868`; `:908`; `app/ponto/services/ausencia.py:835`; `:946`) |
| Ausencia de férias | Cancelamento: Agendamento → PeriodoAquisitivo → UPDATE/DELETE Ausencia de tipo `ferias` → signals/cartório → eventual C/DP. (`app/ferias/services.py:364`; `:367`; `:402`; `:411`; `app/ponto/services/ausencia.py:1281`; `:1283`; `app/ferias/models.py:451`; `app/ponto/services/ausencia.py:1183`; `:1190`; `app/ponto/signals.py:151`) | **Não demonstrar C↔Ausencia usando escapes de resposta**: esses escapes escrevem apenas `falta`/`atestado`, não a mesma linha `ferias`. (`app/chamados/services/materializacao.py:597`; `:598`; `:599`; `app/ponto/services/ausencia.py:1183`) |

No ramo ESCAPE, a trava C do `responder_pergunta_disputa_core` está em um atomic que termina antes da chamada de materialização da ausência. O caminho web lido, portanto, não comprova C retido durante esse UPDATE de Ausencia. (`app/chamados/services/disputa_emissao.py:1503`; `:1511`; `:1541`; `app/chamados/views.py:1228`; `:1283`)

O recálculo FM por evento de Batida continua em `on_commit`; ele não demonstra FM síncrono depois de DP no atomic anterior. A invalidação FM das duas portas de justificativa acima é síncrona e constitui caminho distinto. (`app/ponto/registro_batida.py:175`; `app/ponto/services/fechamento.py:1824`; `app/ponto/services/justificativa.py:215`; `:390`; `app/ponto/services/fechamento.py:1656`)

## 11/11 15:36 · sessao e3b00d6965c5 · - · - · origem=Ronald (tmux)

Censo de escritores concluído, somente por leitura.

| Sítio | `tipo` que fornece | Seed |
|---|---|---|
| `app/ponto/services/ausencia.py:341`, `:385`, `:418` | Variável: constrói e salva a linha nova | Validada em `:193-195` |
| `app/ponto/services/ausencia.py:498`, `:509` | Variável: troca do tipo existente | Validada em `:473-475` |
| `app/ponto/views.py:2415` | Variável do lançamento administrativo | Passa pelo produtor |
| `app/ponto/views.py:2536` | Variável da solicitação PWA | Passa pelo produtor |
| `app/api/views.py:1817` | Variável da API | Passa pelo produtor |
| `app/ponto/services/checar_ausencia.py:46` | Variável do copiloto; chamada HTTP em `app/core/views_ia.py:985` | Passa pelo produtor |
| `app/ponto/services/veredito_celula.py:332`, `:336` | `'falta'` ou variável `tipo_ausencia` | `'falta'`: `app/ponto/migrations/0052_seed_tipoausencia.py:10` |
| `app/chamados/services/materializacao.py:490` | `'falta'`/`'atestado'`, mapa em `app/chamados/models.py:3716` | Seed `0052:10`, `:17` |
| `app/chamados/services/materializacao.py:526` | `'falta'`/`'atestado'`, mapa em `:602`; consumidor `app/chamados/services/disputa_emissao.py:1541` | Seed `0052:10`, `:17` |
| `app/ponto/services/ausencia.py:1202` | `'ferias'` | Seed `0052:23` |
| `app/ponto/services/regularizacao.py:118` | `'falta'`, por `garantir_ausencia` | Seed `0052:10` |
| `app/ponto/management/commands/marcar_abandono_provisorio.py:76` | `'falta'` | Seed `0052:10` |
| `app/ponto/views.py:2794` | Edição: POST ou tipo atual | Usa `editar_ausencia_registro` |
| `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169` | Edição: `a.tipo` atual | Usa `editar_ausencia_registro` |

Os únicos literais produtores encontrados foram `falta`, `atestado` e `ferias`, todos semeados; o catálogo da seed grava `ativo=True`. `app/ponto/migrations/0052_seed_tipoausencia.py:10`, `:17`, `:23`, `:36`.

A API recebe nomes legados, mas traduz antes de escrever: `atestado_medico → atestado`, `atestado_acompanhante → declaracao`, `obito → licenca_luto`; os destinos estão semeados. Isso não constitui escritor de literal desconhecido. `app/api/views.py:1765`; `app/ponto/migrations/0052_seed_tipoausencia.py:17`, `:18`, `:20`.

Férias materializam pela porta: `_sync_ausencia → espelhar_ferias → criar_ausencia(tipo='ferias')`. `app/ferias/models.py:386`, `:391`; `app/ponto/services/ausencia.py:1202`.

O admin de Ausencia é somente leitura, com bloqueio de adicionar/alterar/apagar; os signals de Ausencia encontrados consomem o fato. `app/ponto/admin.py:38`; `app/core/admin.py:59`, `:62`, `:65`; `app/ponto/signals.py:143`, `:275`.

Há `queryset.update` produtivo de Ausencia para rejeitar férias, mas altera apenas `status`/`atualizado_em`, sem escrever `tipo`. `app/ponto/services/ausencia.py:1281`.

Fora do app, `sombra_incremental.sh` copia `ponto_ausencia` incluindo `tipo`, por CSV, contornando o modelo; o destino declarado é o banco de ensaio. `bin/sombra_incremental.sh:14`, `:36`, `:57`, `:62`. A máscara genérica usa bulk, mas os campos de Ausencia declarados são `documento` e `descricao`. `bin/sombra_regras_pessoais.py:64`, `:109`.

Não encontrei outro escritor produtivo/importador de `tipo` na cópia; os contratos existentes fiscalizam construção direta e escritas fora da porta, mas não foram executados nesta auditoria. `app/ponto/tests/test_contract_um_produtor_de_ausencia.py:49`, `:74`, `:88`; `app/ponto/tests/test_chokepoint_ausencia_gate.py:53`.

A origem específica da escrita de #5356 permanece **[não verificado]**.

_3973 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
