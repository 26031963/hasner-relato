# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 18:45** · publicado em **15/11/2026 18:45** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 18:45 · sessao 3ffda51003e4 · - · - · origem=Ronald (tmux)

Adendo confirmado:

- Drawer → `perguntas_vivas_de` → `pergunta_viva` → `_pergunta_viva_calc` → `_pergunta_materializada_de_fato` → `_slot_esperado`: **5 saltos**. `_slot_esperado` aceita `agora=None` em `app/chamados/juizes.py:344`; a chamada em `:755` omite efetivamente `agora` quando ele é `None`, pois o `**` condicional vira `{}`. A borda possui instante em `app/colaboradores/services/drawer.py:21`.
- Nos caminhos através de `contexto_calendario`, o mesmo `**` fornece `agora`, porque a borda o passou e o calendário conserva `_agora` (`app/colaboradores/views.py:1952`; `app/colaboradores/services/drawer.py:124`; `app/colaboradores/services/calendario.py:192`).
- A nova passada AST, considerando `**` condicional, encontrou somente esses candidatos de omissão: `pergunta_viva(hoje)` em `app/colaboradores/services/calendario.py:506`; `pergunta_viva(hoje, agora)` em `app/chamados/juizes.py:1504`; `_pergunta_materializada_de_fato(agora)` em `:1486`; `_slot_esperado(agora)` em `:755`; `motivos_retencao_espelho(hoje)` em `app/ponto/services/fechamento.py:1067`.
- A leitura de `grade_da_celula` e helpers não acrescentou leitor de relógio; o fallback de previsto chama `minutos_previstos_do_dia` em `app/escala/services/leitor_celula.py:411`. Novos leitores por despacho dinâmico fora do fecho lido [não verificado].

## 15/11 18:44 · sessao 3ffda51003e4 · - · - · origem=Ronald (tmux)

Censo das três bordas, por AST com resolução de imports locais/aliases e leitura dos ramos. Caminhos são estáticos; execução em produção permanece [não verificado].

**Bordas já têm instante:** `calendario_ponto` lê `_tz.now()` em `app/colaboradores/views.py:1951`; `contexto_drawer` lê `timezone.now()` em `app/colaboradores/services/drawer.py:21`; `furos_espelho` lê `timezone.now()` em `app/relatorios/views.py:1519`.

**Família compartilhada pelo calendário e drawer**

Prefixos:

- Calendário: `calendario_ponto → contexto_calendario`, chamada `app/colaboradores/views.py:1952`, passa `agora`.
- Drawer: `contexto_drawer → contexto_calendario`, chamada `app/colaboradores/services/drawer.py:123`, passa `agora` na linha 124.

A partir desses prefixos, funções que contêm relógio ou satisfazem o critério de argumento omitido:

1. `contexto_calendario`: `timezone.now()` em `app/colaboradores/services/calendario.py:192`. Fallback **inativo nesses caminhos**, porque ambas as bordas passam `agora` (`views.py:1952`; `drawer.py:124`).
2. `contexto_calendario → status_dos_dias_lavrados`: chamada `app/colaboradores/services/calendario.py:638`; relógio `app/ponto/status_dia.py:253`, guardado por `agora is None` na linha 252. Recebe `_agora`, portanto fallback inativo.
3. `contexto_calendario → pergunta_viva`: alias local `_pv` importado em `app/colaboradores/services/calendario.py:498`, chamada na linha 506. Assinatura `hoje=None, agora=None` em `app/chamados/juizes.py:865`; chamada **omite hoje**, mas passa `agora`. O dia deriva do instante em `app/chamados/juizes.py:888`.
4. `… → pergunta_viva → _pergunta_viva_calc → _dentro_da_janela_viva`: chamadas `app/chamados/juizes.py:897` e `:1468`; leitura `date.today()` em `:595`. Recebe `hoje` derivado do instante; fallback inativo.
5. `… → pergunta_viva → _pergunta_viva_calc → classificar_falta`: import local `_cf` em `app/chamados/juizes.py:1470`, chamada `:1472`, passa `agora` em `:1474`; leitura `_tz.now()` em `app/escala/servico_jornada.py:387`. Fallback inativo porque recebe instante.
6. `contexto_calendario → folha_manda → ate_ontem`: chamadas `app/colaboradores/services/calendario.py:614` e `app/relatorios/cartao_pela_celula.py:413`; leitura `timezone.localdate()` em `app/relatorios/cartao_pela_celula.py:40`. `folha_manda` recebe `_agora` e passa dia derivado dele; fallback inativo.
7. `contexto_calendario → folha_manda → turno_aberto_de`: chamadas `app/colaboradores/services/calendario.py:614` e `app/relatorios/cartao_pela_celula.py:398`; leitura `timezone.now()` em `app/ponto/turnos.py:2291`. Recebe `agora`; fallback inativo. A própria chamada depende de `datas_turno_aberto` não vazio (`cartao_pela_celula.py:396`).
8. `contexto_calendario → folha_manda → totais_da_folha → fechamento_lido → recalcular_fechamento_mes`: chamadas `app/colaboradores/services/calendario.py:614`, `app/relatorios/cartao_pela_celula.py:323`, `:73`, `app/ponto/services/fechamento.py:1759`. **Salto 5** de calendário/drawer. Leituras possíveis `timezone.now()` em `app/ponto/services/fechamento.py:616` e `:1086`; esse caminho não recebe o instante da borda. Exige janela de competência inteira (`cartao_pela_celula.py:321`) e fechamento existente (`:71`). A mesma função contém relógios em `fechamento.py:1143`, `:1161`, `:1189`, mas estes são ramos de escrita **inativos neste caminho**, pois `fechamento_lido` passa `somente_leitura=True` em `:1761` e o ramo de leitura é `:1128`.
9. `… → recalcular_fechamento_mes → motivos_retencao_espelho`: import local `_mre` em `app/ponto/services/fechamento.py:1066`, chamada `:1067`. **Salto 6**; omite `hoje`, cujo default é `None` em `app/folha/export.py:1301`. A função encaminha `hoje=None` à autoridade seguinte em `:1307`; o relógio seguinte ultrapassa o limite de seis saltos.
10. `contexto_calendario → CargaDaJanela.turnos → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado`: chamadas `app/colaboradores/services/calendario.py:252`, `app/ponto/turnos.py:3036`, `:1939`, `:1633`, `app/ponto/juiz_batida.py:270`; leitura `date.today()` em `app/ponto/juiz_batida.py:125`. **Salto 6** de calendário/drawer; leitura própria, embora borda já tenha instante. É chamada de instância `_carga.turnos`, que um grafo limitado a nomes importados perde.

**Ramo adicional do drawer**

11. `contexto_drawer → perguntas_vivas_de → pergunta_viva`: import local alias `_vivas_de` em `app/colaboradores/services/drawer.py:79`, chamada `:80`; reexport de `chamados.models` para `chamados.juizes` em `app/chamados/models.py:4225` e `:4226`; `perguntas_vivas_de` chama `pergunta_viva(p)` em `app/chamados/juizes.py:1504`. **Omite hoje e agora**, ambos `None` por default (`:865`), embora drawer já tenha `agora` (`drawer.py:21`).
12. Esse ramo chega a `_dentro_da_janela_viva` no **salto 4**, via `pergunta_viva:897 → _pergunta_viva_calc:1468`, e pode ler `date.today()` em `app/chamados/juizes.py:595`.
13. Mesmo ramo chega a `classificar_falta` no **salto 4**, chamada `app/chamados/juizes.py:1472`, passa `agora=None` em `:1474`; pode ler `_tz.now()` em `app/escala/servico_jornada.py:387`.
14. `… → _pergunta_viva_calc → _pergunta_materializada_de_fato`: chamada `app/chamados/juizes.py:1486`; assinatura com defaults `hoje=None/agora=None` em `:665`. Passa `hoje=hoje`, mas **omite agora efetivamente nesse ramo**: o `**{'agora': agora}` só existe quando não `None` (`:1487`). A função volta a `_dentro_da_janela_viva` em `:696` e `classificar_falta` em `:701`, leitores no **salto 5**, com dia/instante `None`.
15. `contexto_drawer → turnos_do_colab → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado`: alias local `_tdc` importado `app/colaboradores/services/drawer.py:44`, chamada `:47`; demais chamadas `app/ponto/turnos.py:1915`, `:1939`, `:1633`, `app/ponto/juiz_batida.py:270`; `date.today()` em `:125`. **Salto 5**, leitura própria.

**Furos**

16. `furos_espelho → escalas_em_limbo`: import local em `app/relatorios/views.py:1517`, chamada `:1554`; leitura `timezone.localdate()` em `app/relatorios/furos.py:59`, **salto 1**, leitura própria apesar do `_agora` já disponível (`views.py:1519`).
17. `furos_espelho → dataset_furos → status_dos_dias_lavrados`: chamadas `app/relatorios/views.py:1543`, `app/relatorios/furos.py:123`; leitura guardada em `app/ponto/status_dia.py:253`, **salto 2**. Instante passado nas duas arestas, portanto fallback inativo.
18. `furos_espelho → dataset_furos → contexto_calendario`: chamadas `app/relatorios/views.py:1543`, `app/relatorios/furos.py:160`; passa `agora` em `:164`. Herda os itens 1, 3, 4 e 5 da família compartilhada com um salto extra: relógios guardados de `contexto_calendario:192`, `_dentro_da_janela_viva:595`, `classificar_falta:387`; `pergunta_viva` omite `hoje` em `calendario.py:506`, mas recebe instante.
19. **Somente no grafo sintático** de furos, também aparecem `folha_manda → ate_ontem`, `turno_aberto_de` e `… → recalcular_fechamento_mes` (este no salto 6). O ramo não executa nesta borda: `dataset_furos` fornece `datas_em_aberto=NAO_PERGUNTADO` em `app/relatorios/furos.py:164`, tornando falso `_perguntou` em `app/colaboradores/services/calendario.py:594` e excluindo o bloco que chama `folha_manda` (`:597` até `:614`).
20. O caminho de furos até `intervalo_cadastrado` exige **sete** saltos e está fora do teto: `furos_espelho → dataset_furos → contexto_calendario → CargaDaJanela.turnos → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado`; alternativa pelo D4 chama `_carga.turnos` em `app/ponto/status_dia.py:273`, também totaliza sete.

Não encontrei chamadas de `salario_em`, `custo_mensal`, `vinculos_5x2_6x1_trabalha_feriado` ou `recusa_ancora_futura` no AST de `app/ponto/services/fechamento.py`, `app/ponto/services/espelho.py` ou `app/ponto/motor_calculo_v2.py`; nenhuma dessas arestas diretas deve ser acrescentada [não verificado além dos ASTs lidos].

## 15/11 18:43 · sessao c68596c909d7 · - · - · origem=Ronald (tmux)

Resultados adicionais para consolidar, todos sob `app/`:

- `lavrar_retrato_folha.handle:65 → montar_linhas:1214 → fora_da_folha:205 → _carga:67 → carga_da_tarefa:730 → CargaDaTarefa.__init__:452 → qs_na_fila`; o leitor é `chamados/services/painel.py:190`, mas recebe `agora`, suprimindo seu fallback. A fábrica `_carga_da_tarefa_cls()` devolve a classe; o construtor é chamado pelo corpo de `carga_da_tarefa`, não pela fábrica (`chamados/services/tarefa_do_dia.py:368`, `:392`, `:624`, `:730`).

- `lavrar.handle:65 → montar_linhas:1214 → fora_da_folha:200 → dias_que_seguram:147 → classificar_export:968 → motivos_retencao_espelho`: omite `hoje`, cujo default é `None` (`folha/export.py:1301`). Esta função chama `motivos_retencao_celula` no salto 6 (`folha/export.py:1307`); `_dias_sem_decisao` e seu relógio ficam no salto 7 (`folha/export.py:1212`, `:1238`).

- Outro caminho até `_dias_sem_decisao` está dentro do limite: `lavrar.handle:65 → montar_linhas:1220 → _informativo_rescisao:1288 → informativo_rescisao:1228 → _dias_sem_decisao:1238`. Nele `hoje` é propagado e o fallback não executa (`folha/management/commands/lavrar_retrato_folha.py:65`, `folha/retrato_folha.py:1220`, `:1288`, `folha/export.py:1228`, `:1238`).

- `contadores → linhas_do_retrato → recortar` omite `hoje=None`; também ocorre pelo vigia e pelas três views. É omissão de argumento, mas esses caminhos passam somente eixo `f` ou nenhum grupo, sem consultar dono (`folha/retrato_folha.py:111`, `:120`, `:121`, `:629`, `:632`, `:635`; `folha/vigia_retido_calado.py:65`; `folha/views_retrato.py:198`).

- `vigiar_retido_calado.handle:33 → censo:60 → colabs_nunca_bateu`: possui fallback `timezone.localdate()` em `escala/services/cadastro_realidade.py:161`, suprimido pelo `hoje` passado (`folha/vigia_retido_calado.py:60`).

- `_recorte_da_barra` lê `timezone.now()` em `folha/views_retrato.py:177`; suas três views chamadoras estão em `:258`, `:386`, `:487`. PDF acrescenta `gerar_pdf_auditavel → timezone.now()` (`folha/views_retrato.py:441`, `relatorios/pdf_base.py:133`). Raio-X acrescenta, condicionado a dono `colab`, `placar_aceite → _tz.now()` e `_tz.localdate()` (`folha/views_retrato.py:511`, `:513`, `chamados/services/verbos.py:286`, `:287`).

- `frescor_dos_retratos` exige `agora`; usa conversão com argumento, competências com data explícita e formatação de carimbos com argumento. Nenhum relógio autônomo apareceu nesse fecho (`folha/retrato_folha.py:1386`, `:1408`, `:1411`, `:1441`, `:1444`; `ponto/janelas.py:60`, `:66`).

Para o módulo real `chamados/services/vigia_fio.py`, atenção aos métodos que um grafo apenas de chamadas nomeadas perde:

- `ListaDoAdmin._carregar → lista` passa ambos os valores (`chamados/services/vigia_fio.py:194`). `lista` tem fallback `now` (`escala/services/cadastro_realidade.py:780`), mas chama `_jornada_que_mente(empresas)` em `:791`, cujo `date.today()` é **incondicional** (`:224`).

- `lista:808 → _afastado_sem_registro:592 → afastados_hoje` omite `dia`; este lê `localdate()` (`colaboradores/services/situacao_exibida.py:37`, `:45`). O fallback próprio de `_afastado_sem_registro` está suprimido pelo `hoje` recebido (`escala/services/cadastro_realidade.py:588`, `:808`).

- `_jornada_que_mente:238 → periodos_do_dia:270 → intervalo_cadastrado:125` lê outro `date.today()` quando existe tipo de escala (`ponto/juiz_batida.py:117`, `:125`, `:270`).

- `lista:864 → _nunca_bateu:179 → colabs_nunca_bateu:161` contém fallback de `localdate`, mas propaga `hoje` (`escala/services/cadastro_realidade.py:864`, `:179`, `:161`).

- `censo:359` e `censo_alcance:593` chamam o método `CargaDaTarefa.tarefa`; este chama `_ler_vida` e a função local `ler`, além de `resolver_dia_oferecido_nas_datas(...agora=self.agora)` (`chamados/services/tarefa_do_dia.py:574`, `:578`, `:600`, `:613`). A função local chama `alimentar_fila` com instante (`:588`, `:590`). Esses métodos e a função local precisam integrar o limite de saltos.

## 15/11 18:43 · sessao db3018739a90 · - · - · origem=Ronald (tmux)

Complemento para o módulo real `app/chamados/services/vigia_fio.py`; correspondência com o caminho solicitado `folha/vigia_fio.py` [não verificado].

- `censo_alcance` escolhe empresas visíveis, com interseção opcional de IDs; escolhe competências via import local de `competencias_a_lavrar`, não pelo helper do RetratoFolha. Lê dias vivos de `dias_que_seguram` e deduplica por `(colab, data)`, sem consultar RetratoFolha. `app/chamados/services/vigia_fio.py:508-520`, `:534-544`.
- `competencias_a_lavrar` inclui a atual sempre e a anterior somente se `competencia_entregue` for falso. `app/ponto/services/he_pendente_lavrado.py:218-225`. O juiz considera registro vigente não invalidado da empresa/competência. `app/folha/export.py:580-594`.
- Contracaso derivável: em 25/10/2026, corte 21, com 10/2026 entregue, `competencias_do_retrato` seleciona 10 e 11; alcance seleciona somente 11. Ocorrência em produção [não verificado]. `app/folha/retrato_folha.py:1377-1383`; `app/ponto/services/he_pendente_lavrado.py:220-224`; cálculo das janelas em `app/ponto/janelas.py:24-33`, `:66-72`.
- `censo` antigo escolhe chamados com status vivo e célula, sem janela/competência; `empresas=None` não restringe visibilidade, e o recorte opcional aplica somente empresas fornecidas. Depois seleciona células apontadas com veredito acusando. `app/chamados/services/vigia_fio.py:289-293`, `:331-334`.
- Contracaso derivável: chamado vivo de competência antiga ou empresa oculta pode pertencer ao censo antigo e ficar fora do alcance por janela/visibilidade. Ocorrência em produção [não verificado]. `app/chamados/services/vigia_fio.py:290-292`, `:517-520`, `:534-537`.
- O universo subsidiário de `ListaDoAdmin` cruza sempre empresas visíveis, inclusive quando o censo antigo recebeu `None`; portanto não coincide necessariamente com as empresas dos chamados contados. `app/chamados/services/vigia_fio.py:189-194`, `:290-292`.

## 15/11 18:42 · sessao db3018739a90 · - · - · origem=Ronald (tmux)

Censo por AST/leitura, somente leitura. Os acessos de produção a `RetratoFolha` encontrados são os abaixo; os imports são diretos, incluindo os locais em funções. Não encontrei acesso via alias de módulo/atributo nem via `retratos_folha`; conclusão da varredura AST [não verificado por arquivo:linha].

1. **Universos escolhidos**

- **Fonte de empresas visíveis:** `ativa=True`, exclui CNPJ com prefixo `11222333`, ordena por razão social. Essa ordem também decide a primeira empresa padrão da tela. `app/colaboradores/queries.py:78-87`.
- **Lavrador:** importa localmente `empresas_visiveis`, `RetratoFolha`, `competencias_do_retrato`; empresas = visíveis, com interseção opcional por `--empresa`. Se **ambos** `--mes` e `--ano` forem verdadeiros, usa exclusivamente o par explícito; senão usa o helper por empresa. Um só dos dois argumentos é ignorado para escolha de competência. Grava por `(empresa, mes, ano)`. `app/folha/management/commands/lavrar_retrato_folha.py:44-61`, `:77-79`.
- **Helper compartilhado:** chama `janela_anterior` e `janela_atual`, usa mês/ano da data final e elimina pares repetidos. Import local e funções passadas por variável `_j`: essa chamada indireta consta na AST e na leitura. `app/folha/retrato_folha.py:1369-1383`.
- **Janelas:** `janela_atual(data, empresa)` usa `periodo_apuracao_atual` e corte da empresa; dia maior/igual ao corte desloca competência para mês seguinte. `janela_anterior` aplica `janela_atual` ao dia imediatamente anterior ao início da atual. Corte recebe objeto ou pk, tem padrão 21 e clamp 2–28. `app/ponto/janelas.py:24-33`, `:36-51`, `:60-72`.
- **Frescor:** importa localmente empresas e modelos, transforma o `agora` recebido em dia local, percorre **visíveis × helper por empresa**, consulta retrato por par exato e fechamento pelo mesmo par; ausência vira `nunca lavrado`. Não aceita recorte de empresas/competências explícitas. `app/folha/retrato_folha.py:1401-1418`.
- **Vigia retido:** command passa `None` sem `--empresa`, ou lista de IDs com a flag. `app/folha/management/commands/vigiar_retido_calado.py:30-33`. O censo começa com **todos os retratos**; filtro opcional de IDs não cruza com visíveis. Empresas são construídas **somente a partir dos retratos que existem**; só então consulta helper e separa presentes/ausentes da janela. `app/folha/vigia_retido_calado.py:39-58`. Contadores só percorrem `da_lavra`; `cego` significa nenhuma linha bloqueada ou nenhum retrato escolhido. `:64-89`.
- **Tela/PDF/raiox:** lista visíveis para opções, mas URL ou preferência não `todas` consulta `Empresa.objects.filter(pk=_eid)` sem exigir visibilidade; empresa inválida/ausente cai na primeira visível. Alvo é a empresa encontrada ou todas as visíveis. `app/folha/views_retrato.py:147-164`.
- **Competência da tela:** usa atual da empresa escolhida; em `todas`, usa atual da **primeira visível**, depois aplica um único `(mes, ano)` a **todas**. URL sobrepõe mês/ano independentemente; setas fazem ±1 mês. `app/folha/views_retrato.py:179-193`. `mes_ou` aceita 1–12 e `ano_ou` 2000–2100, com fallback recebido. `app/core/sanitiza.py:30-35`.
- **Aviso de frescor da tela:** chama frescor global, mas retém somente par mostrado e empresas que **já apareceram em `_retratos`**. `app/folha/views_retrato.py:208-212`.
- **Painel lateral do colaborador:** requer mês e ano válidos na URL, empresa cadastrada no colaborador e permissão; consulta par exato e **exige empresa em visíveis**, ao contrário da seleção de pk da tela principal. Imports locais. `app/colaboradores/views.py:975-984`.

**Divergências concretas deduzidas do código; ocorrência em produção [não verificado]:**

- Empresa visível sem **nenhum** retrato: lavrador/frescor a incluem; frescor produz ausências, mas vigia nunca cria sua entrada nem `competencias_sem_retrato`, porque deriva empresas dos retratos existentes. `app/folha/management/commands/lavrar_retrato_folha.py:51-61`; `app/folha/retrato_folha.py:1410-1418`; `app/folha/vigia_retido_calado.py:46-58`. A fixture vazia só verifica `retratos=[]`, `calados=0`, `cego=1`; não exige competências ausentes. `app/chamados/tests/test_e3_r3_c16_sem_banco.py:111-113`.
- Empresa inativa/diagnóstico com retrato de janela: `censo(None)` a inclui; lavrador/frescor/tela em `todas` a excluem. `app/colaboradores/queries.py:85-87`; `app/folha/vigia_retido_calado.py:46-56`; `app/folha/views_retrato.py:147-164`.
- A mesma empresa oculta pode ser aberta por `?empresa=<pk>`/preferência na tela principal, mas seu frescor não vem e seu bloco no painel lateral não encontra retrato. `app/folha/views_retrato.py:153-164`, `:210-212`; `app/folha/retrato_folha.py:1410`; `app/colaboradores/views.py:982-983`.
- `25/10/2026`, corte 21: automático/helper = competências 10 e 11; lavra explícita 09 pode existir/ser mostrada via URL, mas frescor e vigia não a avaliam. O caso 09 explícita versus 10/11 automáticas está materializado na fixture. `app/chamados/tests/test_e3_r3_c16_sem_banco.py:35-37`, `:86-93`; escolhas em `app/folha/management/commands/lavrar_retrato_folha.py:57-61`, `app/folha/vigia_retido_calado.py:55-58`, `app/folha/views_retrato.py:181-193`.
- `15/11/2026`, empresa A corte 21, B corte 10: helper A = 10/11; B = 11/12. Se A vier primeiro em `todas`, tela mostra 11 para ambas e não mostra a atual 12 de B. Se B vier primeiro, tela pede 12 também para A, embora automática de A cubra 10/11. É consequência aritmética do código; empresas reais com esses cortes [não verificado]. `app/ponto/janelas.py:24-33`, `:66-72`; `app/folha/retrato_folha.py:1377-1383`; `app/folha/views_retrato.py:179-193`.
- Empresa visível sem retrato no par mostrado: frescor diz `nunca lavrado`; aviso da tela a elimina porque `_no_recorte` só contém empresas com retratos. `app/folha/retrato_folha.py:1412-1418`; `app/folha/views_retrato.py:209-212`.
- Lavrador explícito de uma empresa/par não equivale ao universo global do frescor: o filtro `--empresa` e o par explícito pertencem somente ao lavrador, enquanto frescor sempre percorre todas as visíveis e janelas. `app/folha/management/commands/lavrar_retrato_folha.py:51-61`; `app/folha/retrato_folha.py:1410-1412`.

2. **O que os selos de relógio contam**

- **c14 `test_RED_o_que_fazer_sem_hoje_falha_e_nao_le_relogio_proprio`: CHAMADAS interceptadas**, exige zero com `Mock.assert_not_called()`. O mesmo mock substitui `localdate` e `now`; não é contagem de “uma leitura”. `app/chamados/tests/test_e3_r3_c14_sem_banco.py:270-283`.
- **c14 `test_painel_entrega_o_dia_do_instante_do_ato`: não conta chamadas nem nomes de relógio**; somente verifica que o keyword `hoje` de uma chamada encontrada a `o_que_fazer` tem AST textual `hoje.date()`, apesar da docstring dizer “lido UMA vez”. `app/chamados/tests/test_e3_r3_c14_sem_banco.py:292-299`.
- **c15 `_le_relogio`: NOMES distintos**. É `sorted({n.func.attr ...})`; dois `.now()` produzem `['now']`. Reconhece atributos `now`, `today` e `localdate` sem argumentos posicionais; não reconhece `localtime()` sem argumentos nem alias direto `now()`. `app/chamados/tests/test_e3_r3_c15_sem_banco.py:159-163`.
- **c15 `_instante_da_borda`: quantidade de alvos `ast.Name` de `Assign` cujo RHS é atributo `.now()`**, sem deduplicação. Não conta todas as chamadas; um `.now()` solto adicional não entra, e `a = b = tz.now()` produz dois alvos para uma chamada. `app/chamados/tests/test_e3_r3_c15_sem_banco.py:180-184`.
- **c15 selo recorte:** combina `len(_instante_da_borda)==1` com conjunto de nomes `['now']`, e verifica passagem do instante/dia. Não prova total de chamadas. `app/chamados/tests/test_e3_r3_c15_sem_banco.py:203-211`.
- **c15 selo calendário:** mesma combinação e limitação. `app/chamados/tests/test_e3_r3_c15_sem_banco.py:213-219`.
- **c15 leitores sem relógio:** exige conjunto de nomes vazio; qualquer chamada reconhecida torna o conjunto não vazio, mas não segue chamadas transitivas. `app/chamados/tests/test_e3_r3_c15_sem_banco.py:196-201`, `:159-163`.
- **c15 G1/carimbo único:** usa iterador de instantes e `now=lambda: next(relogio)`; compara **conjunto de `lavrado_em` gravados** com o primeiro instante. Não verifica `call_count`. `app/chamados/tests/test_e3_r3_c15_sem_banco.py:35-44`, `:62-63`, `:97-99`.
- **c16 drawer:** importa o `_le_relogio` de c15 e usa só `['now']`: **NOMES distintos**, portanto dois `.now()` continuam aprováveis por essa asserção. `app/chamados/tests/test_e3_r3_c16_sem_banco.py:26`, `:149-152`.
- **c16 relatório:** importa os dois helpers e combina contagem de alvos de atribuição com nomes distintos, mesma limitação dos selos de view c15. `app/chamados/tests/test_e3_r3_c16_sem_banco.py:26`, `:154-159`.

## 15/11 18:36 · sessao 5f5716b33138 · E3-RESTO-R3 · verificador · origem=Code

**Ocorrência dos casos abaixo em produção: [nao verificado]. São contraprovas construídas a partir do código.**

**A. Cura incompleta.**

As curas chegaram aos sítios de produção: seleção das competências, dia do drawer, instante do relatório e documentação de `lavrado_em`. Entretanto, restam falhas de universo, propagação do relógio e documentação, detalhadas em B, D e G. (`app/folha/vigia_retido_calado.py:46–58`; `app/colaboradores/services/drawer.py:21–22`; `app/relatorios/views.py:1519–1543`; `app/folha/models.py:315–321`)

Os oito mutantes registrados do c16 morreram por asserção na execução em memória; eles cobrem V/U/R/N, mas não os contraexemplos abaixo. E5 foi corrigido no instrumento `censo_none_literal`; a API de `dataset_furos` continua aceitando `agora=None`. (`app/chamados/tests/e3_r3_c16_mutantes.py:25–38`; `app/chamados/tests/e3_r3_c15_mutantes.py:65–83`; `app/chamados/tests/test_e3_r3_c16_sem_banco.py:162–185`; `app/relatorios/furos.py:103`)

**B. Dois achados de universo.**

- **Alta — empresa sem nenhum retrato desaparece.** O vigia descobre empresas pelos próprios retratos. Em **25/10/2026**, empresa visível E2, corte 21, sem retratos: deveria nomear `(E2,10,2026)` e `(E2,11,2026)`, mas devolve nenhuma falta para E2. Lavrador e frescor incluem essa empresa. Havendo outra empresa com bloqueados, `cego=0`, ocultando a cobertura ausente. (`app/folha/vigia_retido_calado.py:46–58`; `app/folha/vigia_retido_calado.py:89`; `app/folha/management/commands/lavrar_retrato_folha.py:51–61`; `app/folha/retrato_folha.py:1410–1418`)

- **Média — inclui empresas fora do universo automático.** `censo(None)` inclui retratos de empresas inativas ou de diagnóstico; lavrador e frescor usam `empresas_visiveis()`. Exemplo: E3 inativa, retrato 11/2026, hoje 25/10/2026: o vigia conta esse retrato e declara falta da competência 10; os outros dois excluem E3. (`app/folha/vigia_retido_calado.py:46–58`; `app/colaboradores/queries.py:85–87`; `app/folha/management/commands/lavrar_retrato_folha.py:51`; `app/folha/retrato_folha.py:1410`)

Nos demais casos pedidos, nenhum erro encontrado: em **21/10/2026**, corte 21 produz competências **10/11**, enquanto corte 26 produz **09/10**; competência futura fora dessas duas é excluída. Retrato duplicado por empresa/competência é vedado pela constraint. (`app/ponto/janelas.py:24–33`; `app/ponto/janelas.py:66–72`; `app/folha/retrato_folha.py:1377–1383`; `app/folha/vigia_retido_calado.py:56`; `app/folha/models.py:365–367`)

**C. Escritores, leitores e significado.**

| Campo/lista | Quem escreve | Quem lê |
|---|---|---|
| `competencias_sem_retrato` | `censo`, `app/folha/vigia_retido_calado.py:53–61` | `linhas`, no mesmo arquivo:94–97; command imprime em `app/folha/management/commands/vigiar_retido_calado.py:34–35` |
| Lista de `competencias_do_retrato` | `app/folha/retrato_folha.py:1377–1383` | Lavrador: `app/folha/management/commands/lavrar_retrato_folha.py:61`; frescor: `app/folha/retrato_folha.py:1411`; vigia: `app/folha/vigia_retido_calado.py:55–58` |
| `RetratoFolha.lavrado_em` | Instante capturado e gravado pelo lavrador: `app/folha/management/commands/lavrar_retrato_folha.py:49–50`, `:77–79` | Frescor/rótulo: `app/folha/retrato_folha.py:1412–1445`; mínimo da tela: `app/folha/views_retrato.py:203`; apresentação: `app/templates/folha/_retrato_barra.html:22` |

As competências significam **mês/ano do fim da janela da empresa**, não mês civil. `lavrado_em` significa **instante do início da rodada**; o vigia deixou de usá-lo para selecionar universo. A divergência encontrada é de empresas abrangidas, conforme B. (`app/ponto/janelas.py:10–21`; `app/folha/retrato_folha.py:1380–1382`; `app/folha/retrato_folha.py:1399`; `app/folha/vigia_retido_calado.py:55–58`)

`competencias_sem_retrato` é publicado, mas não participa do alarme: este consulta apenas `cego`, `calados` e `dias_sem_leitura_sistema`. (`app/folha/management/commands/vigiar_retido_calado.py:16–17`; `app/folha/management/commands/vigiar_retido_calado.py:37–41`)

**D. Dois relógios ainda fora da borda.**

- **Média — relatório.** `furos_espelho` chama `escalas_em_limbo(empresa)`, que lê outro `localdate()`. Caso: borda **15/11 às 23:59:59**; limbo executado em **16/11 às 00:00:01**; células de col7 em 15/16/17 com `trabalha=None,None,True`. Pelo dia da borda, col7 entra no limbo; pelo relógio interno, sai: **1 → 0**. (`app/relatorios/views.py:1554`; `app/relatorios/furos.py:59–68`; `app/escala/models.py:1092–1095`)

- **Média — drawer.** `_vivas_de(disp)` não recebe instante; seu wrapper chama `pergunta_viva(p)` sem `hoje/agora`, permitindo `date.today()` e `_tz.now()`. Caso: pergunta materializada de **20/09/2026**, via `completude_turno`, sem resposta/validação/célula, sem tranca, marco sem fato e falta ainda acusada. Em **20/11**, permanece viva; em **21/11**, sai da janela e é considerada materializada: **1 → 0 perguntas**, embora o calendário ainda receba o instante anterior da borda. (`app/colaboradores/services/drawer.py:79–80`; `app/colaboradores/services/drawer.py:123–124`; `app/chamados/juizes.py:1502–1504`; `app/chamados/juizes.py:595–621`; `app/chamados/juizes.py:696–703`; `app/escala/servico_jornada.py:387`)

**E. `padrao=` dos dois commits: [nao verificado].** [nao verificado]

A cópia não disponibiliza o histórico Git para consultar as mensagens ou o diff do c15b. No delta c16 apresentado, a centralização das competências corresponde a **Single Source of Truth**; a propagação do instante corresponde a **Clock Injection**. Não é possível afirmar se os campos declarados coincidem sem lê-los. (`app/folha/retrato_folha.py:1369–1383`; `app/relatorios/views.py:1519–1543`; `app/docs/GLOSSARIO.md:11`; `app/docs/GLOSSARIO.md:19`; [nao verificado])

**F. Nenhum achado estrutural na migration.**

A 0013 altera somente `help_text`, preservando `DateTimeField(db_index=True)` da definição anterior; não acrescenta coluna, índice nem transformação de dados. Depende expressamente da 0012. Ser esta a última migration de folha no `main`: **[nao verificado]**. (`app/folha/migrations/0013_retrato_lavrado_em_help_text_e3r3.py:8–18`; `app/folha/migrations/0011_retratofolha.py:22`)

**G. Dois achados adicionais.**

- **Média — selo de “um relógio” aceita dois.** `_le_relogio` usa um conjunto, eliminando chamadas repetidas pelo nome. Substituir a linha do drawer por `_hoje_da_borda = timezone.localtime(timezone.now()).date()` mantém o teste verde, embora introduza uma segunda leitura. Na virada **20/10 23:59:59 → 21/10 00:00:01**, dia e calendário podem divergir. O mutante registrado só testa a introdução de `localdate()`. (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:162–163`; `app/chamados/tests/test_e3_r3_c16_sem_banco.py:149–152`; `app/colaboradores/services/drawer.py:21–22`; `app/chamados/tests/e3_r3_c16_mutantes.py:34`)

- **Baixa — G3 permanece parcialmente incorreto no texto curado.** A docstring ainda afirma que o legado cai em `sem_causa`/sem saldo. Contraexemplos: legado `sem_codigo_dominio` produz **tipo `cadastro`, saldo `cadastro`**; legado `furo_espelho`, `dias=2`, produz **saldo `2a5`**. A referência “letra → fio em `LETRA_DO_FIO`” também está invertida: essa direção é `FIO_DA_LETRA`. (`app/folha/models.py:308–316`; `app/folha/retrato_folha.py:180–181`; `app/folha/retrato_folha.py:388–389`; `app/folha/retrato_folha.py:424–432`; `app/folha/retrato_folha.py:496–513`)

Nenhum segundo juiz ou escritor fora da porta identificado no delta apresentado: a função nova delega às janelas, e a gravação continua no lavrador de `RetratoFolha`. Alteração histórica de asserções e valores reais de TXT/FechamentoMensal/dinheiro: **[nao verificado]**. (`app/folha/retrato_folha.py:1377–1383`; `app/folha/management/commands/lavrar_retrato_folha.py:77–79`)

**VEREDITO: REPROVA.**

## 15/11 18:35 · sessao d251dcf115c8 · - · - · origem=Ronald (tmux)

Achados D/G:

1. **Média — relatório ainda consulta outro relógio.** `furos_espelho` fixa `_agora` em `app/relatorios/views.py:1519`, mas chama `escalas_em_limbo(empresa)` em `:1554`; esta lê `timezone.localdate()` em `app/relatorios/furos.py:59`. Caso sintético: borda em **15/11/2026 23:59:59**, execução do limbo em **16/11 00:00:01**, células de col7 com `trabalha=None,None,True` em 15,16,17/11. O vínculo devolve esses valores diretamente (`app/escala/models.py:1092-1095`): o teste de dois dias incluiria col7 usando 15/16 e exclui usando 16/17 (`app/relatorios/furos.py:67-68`). Instrumento em memória confirmou contagem **1 → 0**; ocorrência em produção [nao verificado].

2. **Média — drawer perde o instante ao consultar perguntas vivas.** `contexto_drawer` chama `_vivas_de(disp)` sem instante (`app/colaboradores/services/drawer.py:79-80`); o wrapper chama `pergunta_viva(p)` sem `agora`/`hoje` (`app/chamados/juizes.py:1502-1504`). A cadeia pode ler `date.today()` (`:595`) e `_tz.now()` (`app/escala/servico_jornada.py:387`).

   Caso sintético executado com os corpos reais do C6 e janelas reais: pergunta de **20/09/2026**, materializada, via `completude_turno`, sem resposta/validação/célula, tranca falsa, marco sem fato e autoridade de falta acusando `orfao_14h`. Com relógio em **20/11**, wrapper retorna **1 pergunta viva**; em **21/11**, retorna **0**. A janela deixa de alcançar 20/09 (`app/chamados/juizes.py:595-621`) e o lastro absolve fora dela (`:696-697`). Assim, uma borda de 20/11 pode produzir calendário de 20/11 e banner de 21/11; ocorrência em produção [nao verificado].

3. **Lacuna dos selos — cadeia não coberta.** Os testes R examinam apenas o AST de `contexto_drawer` e `furos_espelho` (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:149-159`), permitindo os dois caminhos acima. Os mutantes R alteram somente esses corpos (`app/chamados/tests/e3_r3_c16_mutantes.py:34-37`).

As suítes c15+c16 sem banco passaram **19/19**, executadas com `PYTHONDONTWRITEBYTECODE=1`; saída não persistida [nao verificado]. Na cadeia principal do calendário, o instante recebido segue para pergunta viva, folha e D4 (`app/colaboradores/services/calendario.py:506`, `:614`, `:638`); nenhum outro relógio nessa cadeia foi comprovado [nao verificado].

## 15/11 18:34 · sessao e42efb08fe33 · - · - · origem=Ronald (tmux)

- **G/A — média: selo de relógio aceita regressão equivalente.** `_le_relogio` elimina duplicatas com `set`; duas chamadas `now()` aparecem como `['now']` (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:162`). O teste do drawer exige justamente esse resultado (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:149-152`). Mutação em memória de `timezone.localtime(agora).date()` para `timezone.localtime(timezone.now()).date()` em `app/colaboradores/services/drawer.py:22` **sobreviveu** ao teste. Caso: primeiro instante 20/10 23:59:59, segundo 21/10 00:00:01; calendário recebe um instante e os recortes recebem outro dia (`app/colaboradores/services/drawer.py:21-22`, `app/colaboradores/services/drawer.py:132`). O mutante registrado cobre `localdate()`, mas essa leitura duplicada de `now()` escapa (`app/chamados/tests/e3_r3_c16_mutantes.py:34`).

- **F — nenhum problema estrutural encontrado na cópia.** A 0013 apenas altera o `help_text`, preservando `DateTimeField(db_index=True)` (`app/folha/migrations/0013_retrato_lavrado_em_help_text_e3r3.py:12-18`); a definição anterior tem o mesmo tipo e índice (`app/folha/migrations/0011_retratofolha.py:22`). Depende expressamente da 0012 (`app/folha/migrations/0013_retrato_lavrado_em_help_text_e3r3.py:8-10`), que altera somente o `help_text` de `linhas` (`app/folha/migrations/0012_retrato_linhas_help_text_o256.py:12-18`). Ser a última migration do **main**: **[nao verificado]**.

- **G — outros escritores/juízes no delta:** nenhum encontrado. A única gravação identificada nos módulos alterados continua sendo `RetratoFolha.objects.update_or_create`, na porta do lavrador (`app/folha/management/commands/lavrar_retrato_folha.py:77-79`). A lista nova delega às autoridades existentes de janela (`app/folha/retrato_folha.py:1378-1384`).

- Os 65 testes sem banco c4/c5/c6/c14/c15 passaram nesta execução; comparação histórica para determinar se alguma asserção foi enfraquecida: **[nao verificado]**. Os testes do vigia documentam a mudança de expectativa para janelas e preservam a verificação do instante entregue à lista (`app/folha/tests/test_vigia_retido_calado.py:131-141`, `app/folha/tests/test_vigia_retido_calado.py:154-190`).

## 15/11 18:33 · sessao 4e252e0981b3 · - · - · origem=Ronald (tmux)

Dois achados concretos:

1. **Alta — empresa sem qualquer retrato desaparece do universo.** `censo` descobre empresas pelos próprios retratos; sem retrato, não chega a calcular nem registrar competências faltantes (`app/folha/vigia_retido_calado.py:46-58`). Em 25/10/2026, empresa visível E2 com corte 21 deveria produzir faltas `(E2,10,2026)` e `(E2,11,2026)`, pois lavrador e frescor percorrem `empresas_visiveis()` (`app/folha/management/commands/lavrar_retrato_folha.py:51-61`; `app/folha/retrato_folha.py:1410-1418`). Se E1 tiver bloqueados, `cego=0`, deixando E2 silenciosa (`app/folha/vigia_retido_calado.py:89`). Os testes de universo vazio não verificam `competencias_sem_retrato` (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:111-113`; `app/folha/tests/test_vigia_retido_calado.py:31-33`).

2. **Média — universo inclui empresas que lavrador/frescor excluem.** `censo(None)` lê todos os retratos, inclusive empresas inativas ou de diagnóstico (`app/folha/vigia_retido_calado.py:46-52`). Os outros dois usam empresas visíveis, que excluem inativas e CNPJ `11222333*` (`app/colaboradores/queries.py:78-87`). Exemplo: empresa inativa E3 com retrato 11/2026 entra no contador em 25/10/2026 e ainda gera falta de 10/2026; não entra na rodada automática nem no frescor (`app/folha/vigia_retido_calado.py:55-58`; `app/folha/retrato_folha.py:1410-1411`).

Demais verificações:

- Cortes diferentes e virada do dia 21 usam corretamente o mesmo derivador. Em 21/10/2026: corte 21 → competências 10/11; corte 26 → 09/10 (`app/ponto/janelas.py:24-33`; `app/folha/retrato_folha.py:1377-1383`).
- Competência futura fora dessas duas é excluída (`app/folha/vigia_retido_calado.py:56`). Duplicidade por empresa/competência é vedada por constraint (`app/folha/models.py:365-367`; `app/folha/migrations/0011_retratofolha.py:29`).
- `competencias_sem_retrato`: escritor `censo:53-61`; leitor de produção `linhas:94-97`. O alarme não consulta esse campo (`app/folha/management/commands/vigiar_retido_calado.py:16-17`, `:37-41`).
- `competencias_do_retrato`: escritor `app/folha/retrato_folha.py:1377-1383`; leitores lavrador `:61`, frescor `:1411`, vigia `:55-58`, nos respectivos arquivos.
- `lavrado_em`: escritor único encontrado captura o início da rodada e grava esse instante (`app/folha/management/commands/lavrar_retrato_folha.py:49-50`, `:77-79`). Leitores usam comparação de instantes ou apresentação: frescor (`app/folha/retrato_folha.py:1412-1426`), rótulo (`:1441-1445`), mínimo da tela (`app/folha/views_retrato.py:203`) e apresentação (`app/templates/folha/_retrato_barra.html:22`).
- Executados sem banco e sem escrita de bytecode: 10 testes c16 passaram; 8/8 mutantes morreram por asserção, sem erros. Definições dos testes/mutantes: `app/chamados/tests/test_e3_r3_c16_sem_banco.py:77-185`; `app/chamados/tests/e3_r3_c16_mutantes.py:25-38`. Testes com banco: [nao verificado].

## 15/11 18:18 · sessao 132e7f2bb6ed · E3-RESTO-R3 · verificador · origem=Code

**A. As curas existem, mas G1 permanece incompleto.**

- **G1 — média, procede parcialmente.** O escritor grava o mesmo `_agora` em todos os retratos da execução. Os dois mutantes que restauram `timezone.now()` por retrato morrem por asserção. Porém o vigia continua identificando a rodada pelo **dia civil**, contrariando a cura completa pedida na triagem. Os testes c15 criam apenas uma rodada e não distinguem os dois agrupamentos. (`app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/folha/management/commands/lavrar_retrato_folha.py:83`, `app/folha/vigia_retido_calado.py:48`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:35`, `app/chamados/tests/e3_r3_c15_mutantes.py:28`)

- **G2 — cura real nos sítios alterados.** O instante chega ao frescor, à retenção e ao informativo; existem mutantes que retiram essa propagação e morrem. Sobram caminhos de produção com relógios divergentes, detalhados em C. (`app/folha/views_retrato.py:210`, `app/folha/fora_da_folha.py:121`, `app/folha/fora_da_folha.py:301`, `app/chamados/tests/e3_r3_c15_mutantes.py:30`)

- **G3 — baixa, cura documental incompleta e sem mordida própria.** Os textos principais foram corrigidos no código, inclusive em `models.py`; portanto não ficaram apenas na entrega. Contudo os testes e mutantes c15 cobrem exclusivamente G1/G2. Reintroduzir os textos antigos em memória conserva os nove testes verdes. Permanecem contradições documentais, listadas em E6. (`app/chamados/services/tarefa_do_dia.py:32`, `app/folha/fora_da_folha.py:77`, `app/folha/retrato_folha.py:474`, `app/folha/models.py:306`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:81`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:185`, `app/chamados/tests/e3_r3_c15_mutantes.py:27`)

Execução local somente em memória: **9 testes c15 OK; 15/15 mutantes mortos**, por asserção e sem erros de infraestrutura. O instrumento rejeita erros como prova de morte. (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:81`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:185`, `app/chamados/tests/e3_r3_c15_mutantes.py:80`)

**B. G1 ainda permite universo errado — média.**

Os casos abaixo foram reproduzidos com o `censo` real e os leitores reais do retrato, substituindo apenas dependências externas por dados em memória. A ocorrência desses casos em produção é **[nao verificado]**.

| Caso concreto, horários locais | Resultado |
|---|---|
| **Legado:** comp10 em 14/11, 23:59:59, com um bloqueado `ninguem` e um dia sem leitura; comp11 em 15/11, 00:00:01, com um bloqueado `colab` e nenhum dia sem leitura. | Seleciona somente comp11: **calados=0, dias_sem_leitura_sistema=0, cego=0**. Apaga o positivo conhecido da comp10 na seleção anterior à soma. (`app/folha/vigia_retido_calado.py:47`, `app/folha/vigia_retido_calado.py:65`, `app/folha/vigia_retido_calado.py:80`) |
| **Duas rodadas novas no mesmo dia:** às06h, lavra explícita da comp09 com positivo1; às07h, lavra automática das comp10/11, cada uma com bloqueado `colab` e zero dias sem leitura. | Seleciona **09+10+11**, publicando **calados=1, dias_sem_leitura_sistema=1, cego=0**, embora a rodada completa mais recente tenha zero nesses contadores. A competência explícita é permitida e não é apagada pela execução automática. (`app/folha/management/commands/lavrar_retrato_folha.py:57`, `app/folha/management/commands/lavrar_retrato_folha.py:62`, `app/folha/management/commands/lavrar_retrato_folha.py:81`, `app/folha/vigia_retido_calado.py:49`) |

Uma rodada nova completa que atravessa meia-noite já conserva suas competências juntas. Isso não resolve o legado nem a mistura de rodadas do mesmo dia. (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:85`, `app/folha/vigia_retido_calado.py:48`)

O falso positivo acima vem de **outra competência conservada da rodada anterior**. Não há duas versões físicas da mesma chave empresa/competência: existe unicidade e `update_or_create`. (`app/folha/models.py:363`, `app/folha/management/commands/lavrar_retrato_folha.py:81`)

O teste vizinho usado para justificar a conservação do agrupamento cria comp10 em T0 e comp11 em T0+1min e exige ambas como uma lavra. Essa fixture representa o escritor antigo; não demonstra que dia civil identifica corretamente uma rodada após a mudança. (`app/folha/tests/test_vigia_retido_calado.py:42`, `app/folha/tests/test_vigia_retido_calado.py:47`, `app/folha/tests/test_vigia_retido_calado.py:51`)

Leitores de **`RetratoFolha.lavrado_em`**:

| Leitor | Significado assumido |
|---|---|
| `vigia_retido_calado.censo` | Dia civil local como identidade da última lavra. (`app/folha/vigia_retido_calado.py:47`) |
| `frescor_dos_retratos` | Instante comparável ao maior `FechamentoMensal.atualizado_em`; somente `fechamento > lavrado_em` desatualiza. Assume explicitamente início da rodada, conservador para alterações durante sua execução. (`app/folha/retrato_folha.py:1382`, `app/folha/retrato_folha.py:1403`, `app/folha/retrato_folha.py:1408`) |
| `_rotulo_admin` | Instante apresentado no horário local como “lavrado em”. (`app/folha/retrato_folha.py:1430`) |
| `_recorte_da_barra` e barra | Mínimo dos carimbos dos retratos exibidos, apresentado como “atualizado HH:MM”. (`app/folha/views_retrato.py:203`, `app/templates/folha/_retrato_barra.html:22`) |

O escritor localizado é `lavrar_retrato_folha`; homônimos como o de `DiaPago` não pertencem a esse campo. (`app/folha/management/commands/lavrar_retrato_folha.py:81`, `app/folha/porta_export.py:283`)

**C. G2: argumentos presentes não garantem instante preservado.**

Censo atual de chamadas de produção, incluindo aliases:

| Função | Chamadas | Sítios |
|---|---:|---|
| `frescor_dos_retratos` | 2 | `app/folha/views_retrato.py:210`; `app/folha/retrato_folha.py:1443` |
| `retratos_desatualizados` | 0 | Definição em `app/folha/retrato_folha.py:1437`; órfão também documentado em `app/docs/BACKLOG.md:1772` |
| `o_que_segura` | 0 | Definição em `app/folha/fora_da_folha.py:257`; ferramenta humana HAIKU declarada em `app/docs/BACKLOG.md:1772` |
| `dias_que_seguram` | 2 | `app/folha/fora_da_folha.py:200`; `app/chamados/services/vigia_fio.py:537` |
| `_dias_que_retem` | 2 | `app/folha/fora_da_folha.py:165`; `app/folha/fora_da_folha.py:275` |
| `contexto_calendario` | 4 | `app/colaboradores/views.py:1952`; `app/colaboradores/services/calendario.py:157`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:160` |

Nenhuma dessas chamadas omite sintaticamente o parâmetro correspondente. Entretanto:

- **Drawer — média:** captura `agora`, depois relê o dia três vezes. Exemplo com corte21: instante inicial **20/11 23:59:59**, leitura posterior em21/11. A janela do drawer passa de **21/09–20/11** para **21/10–20/12**, mas a grade recebe o instante de20/11; o `hoje` devolvido também vem de outro relógio. (`app/colaboradores/services/drawer.py:19`, `app/colaboradores/services/drawer.py:43`, `app/colaboradores/services/drawer.py:111`, `app/colaboradores/services/drawer.py:122`, `app/colaboradores/services/drawer.py:157`, `app/ponto/janelas.py:24`, `app/colaboradores/services/calendario.py:192`)

- **Relatório de furos — média:** a view lê `hoje`, mas não transmite instante ao dataset. O default `None` chega ao D4 e ao calendário, que leem seus relógios. Uma execução atravessando meia-noite pode combinar o recorte do dia anterior com julgamentos de instantes diferentes por colaborador. (`app/relatorios/views.py:1518`, `app/relatorios/views.py:1541`, `app/relatorios/furos.py:103`, `app/relatorios/furos.py:123`, `app/relatorios/furos.py:164`, `app/ponto/status_dia.py:253`, `app/colaboradores/services/calendario.py:192`)

- **Backfill, medição posterior — média:** após os atos, relê o instante mantendo o `hoje` inicial. Se começar em14/11 e medir depois em15/11, entrega dia14 com instante15 ao `censo_alcance`, que não valida essa divergência. O outro `censo` valida. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:218`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:352`, `app/chamados/services/vigia_fio.py:516`, `app/chamados/services/vigia_fio.py:283`)

Esses caminhos não aparecem no delta fornecido; sua situação executável nos hashes anteriores é **[nao verificado]**.

As três bordas nomeadas estão corretas no caminho normal: calendário do perfil, barra e comando do vigia derivam `agora`/`hoje` do mesmo instante. (`app/colaboradores/views.py:1951`, `app/colaboradores/views.py:1965`, `app/folha/views_retrato.py:177`, `app/folha/views_retrato.py:210`, `app/chamados/management/commands/vigiar_fio_morto.py:171`, `app/chamados/management/commands/vigiar_fio_morto.py:181`)

O resíduo financeiro declarado continua existindo: `classificar_export` e `recalcular_fechamento_mes` omitem `hoje`; o wrapper transmite `None` até `_dias_sem_decisao`, que lê `localdate()`. Esses arquivos estão fora do delta fornecido; comparação independente por Git é **[nao verificado]**. (`app/folha/export.py:968`, `app/ponto/services/fechamento.py:1067`, `app/folha/export.py:1307`, `app/folha/export.py:1238`)

**D. `retratos_desatualizados`: zero chamador de produção no hash atual.**

O delta fornecido altera sua assinatura, sem remover chamador. Há registro documental de zero chamador datado de27/10, portanto não cabe atribuir o abandono à c15. A contagem executável nos hashes **57c1c9b94** e **77dfa94fa** permanece **[nao verificado]**. (`app/folha/retrato_folha.py:1437`, `app/docs/BACKLOG.md:1772`)

**E. Checklist.**

**1. Escritores, leitores e unidades pertinentes ao delta:**

| Campo/valor | Origem/escritor | Leitura e significado |
|---|---|---|
| Empresa, mês e ano do retrato | Lavrador escolhe competência explícita ou janelas do juiz; persiste pela chave única. (`app/folha/management/commands/lavrar_retrato_folha.py:57`, `app/folha/management/commands/lavrar_retrato_folha.py:62`, `app/folha/management/commands/lavrar_retrato_folha.py:81`) | View filtra pela competência; frescor usa anterior/atual; vigia acrescenta o filtro divergente por dia civil descrito em B. (`app/folha/views_retrato.py:192`, `app/folha/retrato_folha.py:1396`, `app/folha/vigia_retido_calado.py:48`) |
| `linhas.status/motivo` | Classificação do export, copiada pelo montador e persistida pelo lavrador. (`app/folha/export.py:969`, `app/folha/retrato_folha.py:1224`, `app/folha/retrato_folha.py:1240`, `app/folha/management/commands/lavrar_retrato_folha.py:83`) | `_balde` distingue entram/bloqueado/módulo próprio; fio legado usa status/motivo. (`app/folha/retrato_folha.py:89`, `app/folha/retrato_folha.py:449`) |
| `dias/datas` | Datas do retentor; contagem e ISO publicados por `fora_da_folha`, copiados pelo montador. (`app/folha/export.py:1261`, `app/folha/fora_da_folha.py:235`, `app/folha/fora_da_folha.py:246`, `app/folha/retrato_folha.py:1242`) | Recorte inclusivo nas duas pontas; balde Hoje usa `data >= hoje`. Datas são dias da célula/turno, não instantes. (`app/folha/retrato_folha.py:1343`, `app/folha/retrato_folha.py:491`, `app/folha/export.py:1180`) |
| `dono`, `dias_sem_leitura`, `dias_sem_porta` | Projeção da porta e agrupamento dos dias; persistidos no retrato. (`app/folha/fora_da_folha.py:217`, `app/folha/fora_da_folha.py:231`, `app/folha/retrato_folha.py:1244`, `app/folha/retrato_folha.py:1248`) | `dono` guarda **fio**; o balde vem do mapa. Vigia conta pessoas por retrato e dias sem leitura separadamente; template lê datas sem porta. (`app/folha/retrato_folha.py:194`, `app/folha/retrato_folha.py:447`, `app/folha/vigia_retido_calado.py:58`, `app/folha/vigia_retido_calado.py:65`, `app/templates/folha/_retrato_o_que_fazer.html:46`) |
| `acao/rota/fio/nao_sei` da tarefa | Publicados pelo juiz e projetados pela porta; ações copiadas à linha. (`app/chamados/services/tarefa_do_dia.py:708`, `app/chamados/services/tarefa_do_dia.py:722`, `app/folha/fora_da_folha.py:223`) | Folha lê fio; UI traduz ação em endereço. `resolver_dia` transporta dia do turno. (`app/folha/fora_da_folha.py:86`, `app/folha/retrato_folha.py:1044`, `app/folha/retrato_folha.py:1054`) |
| `tipo/saldo/informativo` | Montador lê causa gravada, aplica os mapas e copia informativo da autoridade. (`app/folha/retrato_folha.py:1217`, `app/folha/retrato_folha.py:1220`, `app/folha/retrato_folha.py:1250`) | Leitores dos eixos e template; informativo de rescisão não entra em `datas/dias` retentores. (`app/folha/retrato_folha.py:496`, `app/folha/retrato_folha.py:506`, `app/folha/retrato_folha.py:1252`, `app/templates/folha/_retrato_o_que_fazer.html:56`) |
| `FechamentoMensal.atualizado_em/causa_espelho/motivos_espelho` | Carimbo `auto_now`; recalculador atribui causa/motivos e salva. (`app/ponto/models.py:479`, `app/ponto/services/fechamento.py:1067`, `app/ponto/services/fechamento.py:1080`, `app/ponto/services/fechamento.py:1144`) | Frescor compara carimbos por instante; montador lê causa; export consulta a autoridade de retenção. (`app/folha/retrato_folha.py:1408`, `app/folha/retrato_folha.py:1170`, `app/folha/export.py:968`) |
| `CelulaDia.veredito/protestos/status_dia` | Porta da célula grava o julgamento e status. (`app/ponto/portas/celula.py:1092`, `app/ponto/portas/celula.py:1096`) | Retentor lê veredito/protestos; calendário lê status pelo D4. (`app/folha/export.py:1273`, `app/folha/export.py:1286`, `app/colaboradores/services/calendario.py:638`, `app/colaboradores/services/calendario.py:695`) |

O retentor percorre `[ini,fim]` inclusivo; ausência de célula passa a reter apenas em `d < hoje`. As conversões locais usam o fuso configurado `America/Sao_Paulo`. A divergência encontrada não é inclusão de extremidade: é **instante versus identidade de rodada/dia civil**. (`app/folha/export.py:1265`, `app/folha/export.py:1271`, `app/config/settings/base.py:93`, `app/folha/vigia_retido_calado.py:48`)

**2. Segundo juiz ou regra manual de status:** nenhum novo identificado no delta. A tarefa continua publicando a porta; calendário consulta D4; a classificação do export permanece na autoridade registrada. O agrupamento errado do vigia é seleção de universo, já apontada em B. (`app/core/juizes.py:299`, `app/core/juizes.py:323`, `app/chamados/services/tarefa_do_dia.py:722`, `app/colaboradores/services/calendario.py:638`)

**3. Segundo escritor:** nenhum introduzido pelo delta identificado para Chamado, Pauta, CelulaDia, DiaPago ou FechamentoMensal. A escrita alterada continua sendo a de RetratoFolha, pelo lavrador. (`app/chamados/services/tarefa_do_dia.py:735`, `app/chamados/services/vigia_fio.py:490`, `app/folha/management/commands/lavrar_retrato_folha.py:81`)

**4. Relógio fora da borda:** procede nos caminhos de C e no resíduo financeiro; não foi eliminado globalmente. (`app/colaboradores/services/drawer.py:111`, `app/relatorios/furos.py:164`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:352`, `app/folha/export.py:1238`)

**5. Selos/asserções:** G1 não protege o agrupamento do leitor; G3 não possui mutante c15. O censo G2 aceita a presença sintática de `agora=None`, e a lista contra relógios próprios não inclui drawer/dataset/backfill. (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:35`, `app/chamados/tests/e3_r3_c15_mutantes.py:27`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:140`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:168`)

Nos vizinhos de desligamento há lei explícita para a inversão, gêmeo ativo preservado e asserção do informativo. A medida atual conserva os critérios9/85 e A1=6/B1=82. Enfraquecimento relativo dos arquivos de apoio e testes entre os hashes é **[nao verificado]**. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:1`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:102`, `app/folha/tests/test_lei_desligado_nao_trava.py:104`, `app/core/tests/test_medida_turno_do_par.py:100`)

**6. Documentação incorreta — baixa:**

- `lavrado_em` ainda é descrito como momento em que a linha foi escrita; agora contém o início da rodada, anterior à montagem daquela competência. (`app/folha/models.py:356`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/folha/management/commands/lavrar_retrato_folha.py:69`)
- Modelo afirma que linha antiga cai em `ninguem`; `{status:'fora', motivo:'sem_codigo_dominio'}` sem `dono` retorna `admin`. (`app/folha/models.py:315`, `app/folha/retrato_folha.py:449`, `app/folha/retrato_folha.py:374`)
- Schema documentado omite saldo `cadastro` e grupoE, ambos produzidos pelo código. (`app/folha/models.py:307`, `app/folha/models.py:308`, `app/folha/retrato_folha.py:424`, `app/folha/fora_da_folha.py:112`)

O contrato dos cinco retornos de `_fio_do_dia` foi corrigido e corresponde à projeção atual. (`app/folha/fora_da_folha.py:77`, `app/folha/fora_da_folha.py:89`)

**7. ESMERIL-SEMANTICA:** procede para `lavrado_em`: início da rodada no escritor/frescor, momento da escrita no modelo e dia identificador de rodada no vigia. `dono` guardar fio está explicitamente documentado e possui mapa próprio para o balde; não demonstra outro conflito novo. (`app/folha/management/commands/lavrar_retrato_folha.py:83`, `app/folha/retrato_folha.py:1382`, `app/folha/models.py:356`, `app/folha/vigia_retido_calado.py:48`, `app/folha/retrato_folha.py:170`, `app/folha/retrato_folha.py:194`)

**8. TXT/FechamentoMensal/dinheiro:** nenhuma mudança de cálculo ou escrita financeira identificada no delta fornecido. A alteração persiste o retrato e muda leituras de retenção/apresentação; o caminho financeiro conserva as chamadas sem `hoje`. Igualdade numérica em produção é **[nao verificado]**. (`app/folha/management/commands/lavrar_retrato_folha.py:83`, `app/folha/fora_da_folha.py:121`, `app/folha/export.py:968`, `app/ponto/services/fechamento.py:1067`)

**VEREDITO: REPROVA.**

## 15/11 18:14 · sessao 4b9631d94182 · - · - · origem=Ronald (tmux)

Achados de G2/C/E4:

- **Média — relógios divergentes no drawer, fora do delta fornecido.** `contexto_drawer` captura `agora` em `app/colaboradores/services/drawer.py:19`, mas lê datas novas em `:43` e `:111`; entrega o instante antigo ao calendário em `:121–122`. Caso concreto, corte 21: `agora=20/11/2026 23:59:59`, leitura de `:111=21/11`. A janela apresentada passa de **21/09–20/11** para **21/10–20/12**, enquanto a grade julga com `hoje=20/11` derivado do instante antigo (`app/ponto/janelas.py:24`, `app/colaboradores/services/calendario.py:192`). Pré-existência no hash anterior: [nao verificado].

- **Média — instante não chega ao calendário do relatório.** A borda lê `hoje` em `app/relatorios/views.py:1518`, mas chama `dataset_furos` sem `agora` em `:1541`; o default `None` (`app/relatorios/furos.py:103`) desce ao D4 em `:123` e ao calendário em `:164`, cujo fallback lê o relógio em `app/colaboradores/services/calendario.py:192`. Uma frota atravessando meia-noite pode receber dias de julgamento diferentes, com recorte escolhido pelo dia antigo (`app/relatorios/views.py:1536–1537`). Fora do delta fornecido; pré-existência: [nao verificado].

- **Baixa — proteção dos testes não cobre esses relógios.** O censo exige presença sintática do argumento (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:140–143`); portanto aceita `agora=None`. A proibição de relógio próprio restringe-se às seis funções de `LEITORES_SEM_RELOGIO`, sem drawer/dataset (`:168–170`).

Censo atual de chamadas de produção, incluindo aliases:

| Função | Chamadas | Locais |
|---|---:|---|
| `frescor_dos_retratos` | 2 | `app/folha/views_retrato.py:210`; `app/folha/retrato_folha.py:1443` |
| `retratos_desatualizados` | 0 | Definição em `app/folha/retrato_folha.py:1437`; nenhuma referência executável externa localizada [nao verificado] |
| `o_que_segura` | 0 | Definição em `app/folha/fora_da_folha.py:257`; uso humano HAIKU declarado em `app/docs/BACKLOG.md:1772` |
| `dias_que_seguram` | 2 | `app/chamados/services/vigia_fio.py:537`; `app/folha/fora_da_folha.py:200` |
| `_dias_que_retem` | 2 | `app/folha/fora_da_folha.py:165`, `:275` |
| `contexto_calendario` | 4 | `app/colaboradores/views.py:1952`; `app/colaboradores/services/calendario.py:157`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:160` |

Todos esses chamadores presentes entregam o keyword correspondente; o relatório entrega `None` por default (`app/relatorios/furos.py:103`, `:164`).

As curas diretas são reais:

- Barra: único `_agora`, deriva `_hoje` e transmite o instante ao frescor (`app/folha/views_retrato.py:177`, `:178`, `:210`).
- Calendário de perfil: captura `_agora`, passa à grade e deriva `hoje` dele (`app/colaboradores/views.py:1951`, `:1952`, `:1965`).
- Vigia normal: comando captura `agora/hoje` juntos e transmite aos dois censos (`app/chamados/management/commands/vigiar_fio_morto.py:171`, `:172`, `:180`, `:181`); `censo_alcance` deriva o default e passa `hoje` à retenção (`app/chamados/services/vigia_fio.py:516`, `:537`).
- `_dias_que_retem` transmite `hoje` à autoridade (`app/folha/fora_da_folha.py:121`); `o_que_segura` deriva uma data para retenção e informativo (`:270`, `:275`, `:301`).

Nuance do vigia: `censo_alcance` aceita `hoje` explícito divergente sem validar (`app/chamados/services/vigia_fio.py:516`), ao contrário de `censo` (`:283–284`). Backfill permite `--hoje` histórico em DRY/LISTA (`app/chamados/management/commands/backfill_fio_morto_e4b.py:218–221`, `:237`); após o ato relê `timezone.localtime()` preservando o `hoje` antigo (`:352`). É caminho externo ao delta fornecido.

Resíduo dinheiro:

- Exatamente duas chamadas a `motivos_retencao_espelho`: `app/folha/export.py:968`; `app/ponto/services/fechamento.py:1067`.
- Ambas omitem `hoje`; o wrapper transmite `None` (`app/folha/export.py:1307`), chegando ao relógio interno (`:1238`).
- `motivos_retencao_celula` tem quatro chamadas: export `:1307`, fora_da_folha `:121`, previa `app/folha/previa.py:170`, vigia_pausa `app/folha/services/vigia_pausa.py:125`. As duas últimas já fornecem `hoje`.
- Export/fechamento estão ausentes do delta de produção fornecido; comparação por git indisponível nesta cópia [nao verificado].

D/histórico: `app/docs/BACKLOG.md:1772` já documenta zero chamador de `retratos_desatualizados` em 27/10. Isso é evidência documental anterior; não prova contagem executável nos hashes `57c1c9b94`/`77dfa94fa` [nao verificado]. O delta fornecido não remove chamada dessa função; muda sua assinatura (`app/folha/retrato_folha.py:1437`).

Testes: executei em memória, com `python3 -B`: **9/9 OK e 15/15 mutantes mortos**, sem escrita; resultado transitório sem log arquivado [nao verificado]. O instrumento efetivamente troca fontes em memória (`app/chamados/tests/e3_r3_c15_mutantes.py:73–78`), exige falha de asserção e rejeita erro de infraestrutura (`:80–83`). G2 possui mutantes das bordas, propagação e defaults (`:30–47`); não é cura só textual.

E2: no calendário, status/rotulo vêm do D4 (`app/colaboradores/services/calendario.py:638`, `:695–696`); precedência vem de `fatos_do_periodo` (`:538`). Não localizei clone novo de juiz no delta [nao verificado].

## 15/11 18:13 · sessao e3b4179907c7 · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **G3 sem selo que morda a regressão textual — baixa.** O teste c15 contém apenas G1 e G2 (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:81`, `:185`); os 15 mutantes também cobrem exclusivamente essas duas curas (`app/chamados/tests/e3_r3_c15_mutantes.py:27`). Reintroduzi, apenas em memória, textos antigos contraditórios em tarefa_do_dia, fora_da_folha, retrato_folha e models: os nove testes c15 continuam verdes. O selo textual c14 cobre outros sítios explicitamente enumerados (`app/chamados/tests/test_e3_r3_c14_sem_banco.py:409`). A cura documental existe nos arquivos vivos, portanto não foi feita só no teste/texto de entrega; falta a mordida pedida para G3 (`app/chamados/services/tarefa_do_dia.py:32`; `app/folha/fora_da_folha.py:77`; `app/folha/retrato_folha.py:474`; `app/folha/models.py:306`).

2. **Contrato documental de RetratoFolha ainda contraditório — baixa.**
   - Afirma que linha antiga sem as três chaves cai em fio `ninguem` (`app/folha/models.py:315`), mas `{status:'fora', motivo:'sem_codigo_dominio'}` sem `dono` cai em `admin`: `fio_da_linha` usa `_fio_cru` (`app/folha/retrato_folha.py:449`), que retorna admin para motivo diferente de furo (`:374`).
   - Enumeração de grupos ainda diz A/B/C/D (`app/folha/models.py:308`), embora `_grupo(['nao_sei'])` retorne E (`app/folha/fora_da_folha.py:112`).
   - Schema de saldo ainda enumera somente `1|2a5|6mais|None` (`app/folha/models.py:307`), embora motivo cadastral produza `cadastro` (`app/folha/retrato_folha.py:424`).

Resultados executados sem gravação:

- c15: nove testes verdes, quinze mutantes mortos por asserção, sem erro de infraestrutura; o runner rejeita morte por erro (`app/chamados/tests/e3_r3_c15_mutantes.py:80`).
- c5+c6+nome_indefinido: 34 testes verdes (`app/chamados/tests/test_e3_r3_c5_sem_banco.py:24`; `app/chamados/tests/test_e3_r3_c6_sem_banco.py:87`; `app/chamados/tests/test_e3_r3_c6_nome_indefinido.py:29`).
- Vizinhos de desligamento têm lei explícita justificando inversão, preservam gêmeo ativo e verificam informativo (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:1`, `:102`, `:116`; `app/folha/tests/test_lei_desligado_nao_trava.py:79`, `:103`).
- Medida mantém números registrados 9/85 e critérios A1=6/B1=82 (`app/core/tests/test_medida_turno_do_par.py:100`); os três sítios novos aparecem na classificação informativa (`:391`, `:415`).
- Enfraquecimento relativo dos testes entre hashes anteriores: **[nao verificado]**, porque esta cópia não disponibiliza histórico Git acessível. [nao verificado]

## 15/11 18:13 · sessao 091be4eebcf9 · - · - · origem=Ronald (tmux)

- **G1, média: cura parcial.** O escritor grava `_agora` em todos os retratos (`app/folha/management/commands/lavrar_retrato_folha.py:49`, `:83`), mas o vigia ainda identifica rodada por dia civil local (`app/folha/vigia_retido_calado.py:47-50`). O próprio comentário promete ler a última lavra (`:20-25`).

  Casos concretos reproduzidos executando `censo` real com dependências em memória:
  1. Legado: comp10 carimbada em **14/11 23:59:59**, com um dia sem leitura; comp11 em **15/11 00:00:01**, sem linhas bloqueadas. Retorna apenas comp11, `dias_sem_leitura_sistema=0`, `calados=0`, `cego=1`; o positivo de comp10 desaparece pela seleção em `:48-50`, antes das somas em `:65-80`.
  2. Duas rodadas novas no mesmo dia: às06h, execução explícita de comp09 grava positivo1; às07h, execução completa grava comp10 e comp11, ambas zero. O vigia retorna **09+10+11 e positivo1**, embora a rodada completa mais recente contenha apenas10/11. Execução explícita é permitida em `lavrar_retrato_folha.py:57-58`; universo automático vem de `:62-69`; competências antigas permanecem porque `update_or_create` só substitui sua própria chave (`:81-83`). Não é versão física antiga da mesma competência: a unicidade está em `app/folha/models.py:363-364`.
  3. Rodada completa comp10/11 às06h, comp10 positivo1; relavra parcial de comp11 às07h, zero: continuam ambas e positivo1. Isso confirma que o resultado depende da interpretação “união do dia”, não “última rodada” (`vigia_retido_calado.py:47-50`). A entrega assume deliberadamente essa interpretação, apesar da cura pedida na triagem. [nao verificado: ocorrência desses casos em produção]

- **Mordida G1:** os dois mutantes registrados em `app/chamados/tests/e3_r3_c15_mutantes.py:28-29` foram executados e morreram com falha de asserção, sem erro de infraestrutura; sustentam a cura do escritor nos testes `app/chamados/tests/test_e3_r3_c15_sem_banco.py:85-92`. O leitor não é protegido: substituir em memória `vigia_retido_calado.py:48` o dia pelo instante manteve os três testes G1 verdes. A fixture só produz uma rodada (`test_e3_r3_c15_sem_banco.py:35-78`) e não distingue agrupamento por dia de agrupamento por instante. O teste vizinho exige dois carimbos diferentes, T0 e T0+1min, como uma lavra (`app/folha/tests/test_vigia_retido_calado.py:42-52`); essa fixture representa o escritor antigo e não justifica conservar a ambiguidade do leitor após a mudança.

- **Leitores de `RetratoFolha.lavrado_em`:**
  - Vigia: dia civil local identifica a última lavra (`app/folha/vigia_retido_calado.py:47-50`).
  - Frescor: instante comparado ao maior `FechamentoMensal.atualizado_em`; fechamento estritamente posterior significa desatualizado, igualdade significa atualizado (`app/folha/retrato_folha.py:1401-1411`). Assume início da rodada, explicitamente conservador (`:1382`).
  - Rótulo: converte o instante para horário local e apresenta “retrato lavrado em” (`app/folha/retrato_folha.py:1430-1434`).
  - View: usa o mínimo dos instantes dos retratos exibidos (`app/folha/views_retrato.py:201-203`), repassa como `atualizado` (`:241`, `:319`), e a barra mostra “atualizado HH:MM” (`app/templates/folha/_retrato_barra.html:22`).
  - Escritor único encontrado: `app/folha/management/commands/lavrar_retrato_folha.py:81-83`. Outros campos homônimos encontrados pertencem a DiaPago/Placar/Pauta, não a RetratoFolha (`app/folha/porta_export.py:283-287`, `app/chamados/views.py:843-848`).

- **G3/semântica, baixa:** o help_text ainda define o campo como “Quando o lavrador escreveu esta linha” (`app/folha/models.py:354-357`), mas ele passou a conter o início da rodada, possivelmente anterior à montagem e à gravação daquela competência (`app/folha/management/commands/lavrar_retrato_folha.py:49`, `:69`, `:83`). Há três sentidos concorrentes: início da rodada, momento da escrita e dia identificador de rodada; os locais acima demonstram a divergência.

**Recomendação do recorte B/G1: REPROVA**, pela seleção incorreta do universo ainda em `app/folha/vigia_retido_calado.py:47-50`.

## 15/11 18:03 · sessao 6740547b38e3 · JUIZ-RESPONDIDA · censo · origem=Code

Há critérios diferentes para perguntas diferentes: a fila global usa **instante E texto**, a retenção sem veredito usa **instante OU texto**, e a entrada de `validar_pergunta` verifica **só instante**. Não escolhi entre eles. (`app/chamados/juizes.py:1725`; `app/chamados/models.py:1907`; `app/chamados/services/validacao.py:418`.)

**Árvore:** examinei a cópia fornecida. Correspondência com `HEAD` de `main` e com o hash informado: **[nao verificado]**, pois não há metadados Git acessíveis. Execução dos testes: **[nao verificado]**; abaixo descrevo suas asserções estaticamente. [nao verificado]

**Convenções do censo:** `I` = instante presente; `T` = texto que sobra de `(texto or '').strip()`. Onde o código usa verdade crua, igualdade com `''`, comparação de conteúdo ou intervalo temporal, isso está explicitado. (`app/chamados/juizes.py:1523`; `app/chamados/juizes.py:1531`.)

Classes: **A** = `instante E texto`; **B** = `instante OU texto`; **C** = `so instante`; **D** = `so texto`; **E** = `outro`, detalhado na célula; **F** = `exibicao (so mostra)`. A classificação considera o uso dos dois campos naquele sítio; os demais filtros continuam indicados.

**(a) Leitores**

Os números R identificam usos separados por função/finalidade, inclusive quando reunidos numa linha. Q identifica a finalidade normalizada para a contagem final. Os delegados explícitos aparecem além do corpo que consultam.

| R | Arquivo:linha — função | Campos | Classe / predicado aplicado | Pergunta/finalidade | [nao verificado]
|---|---|---|---|---|
| 1 | `app/api/views.py:1163`, `:1165` — `api_chamados_v2` | I,T | **F** — serializa resposta e data | Q01 Payload do app |
| 2 | `app/api/views.py:1179` — `api_chamados_v2` | I,T, delegados | **E** — permite correção quando `not aguarda_o_colaborador(...)`, além dos outros limites | Q02 Correção/reenvio |
| 3 | `app/api/views.py:2561`, `:2563` — `api_disputa_perguntas._item` | I,T | **F** — serialização | Q01 Payload do app |
| 4 | `app/api/views.py:2577` — `api_disputa_perguntas` | I | **C** — `I` seleciona `respondidas` dentro do histórico | Q03 Partição pendente/histórico |
| 5 | `app/api/views.py:2620` — `api_disputa_responder` | I | **C** — `ja_respondida = I` | Q02 Correção/reenvio |
| 6 | `app/api/views.py:2637` — `api_disputa_responder` | I | **F** — devolve instante gravado | Q01 Payload do app |
| 7 | `app/api/views_mensageria.py:567`, `:569`, `:570` — `_item_validar` | I,T | **F** — resposta, data e dias de espera | Q04 Fila de validação |
| 8 | `app/chamados/canal.py:65` — `perguntas_mudas_com_resposta` | T | **D** — SQL `resposta_colab=''`, sem trim | Q05 Resposta encontrada no fio |
| 9 | `app/chamados/juizes.py:351`, `:363` — `_slot_esperado` | I,T | **E** — rejeita resposta posterior ao instante consultado; interpreta `data_hora` ou delega ao plantio | Q06 Slot/materialização |
| 10 | `app/chamados/juizes.py:855`, `:856` — `_chave_memo_pv` | I,T | **E** — valores integram chave de memo; `getattr` literal | Q07 Cache/invalidação — técnica |
| 11 | `app/chamados/juizes.py:1359`, `:1371`, `:1376` — `_resposta_concorda_com_a_luz` | T | **E** — parse da hora e concordância classe A com luz do marco | Q08 Vitalidade |
| 12 | `app/chamados/juizes.py:1388`, `:1393` — `projecao_temporal_da_pergunta` | I,T | **E** — se instante posterior ao teto, projeta localmente `(None, '')` | Q09 Projeção temporal — técnica |
| 13 | `app/chamados/juizes.py:1403`, `:1420` — `_pergunta_viva_calc` | I,T, via projeção | **A** — degrau da resposta: `I and T`; a vitalidade completa segue os demais degraus | Q08 Vitalidade |
| 14 | `app/chamados/juizes.py:1531` — `respondida_sem_texto` | I,T | **E** — `I ∧ ¬T` | Q10 Integridade do par — técnica |
| 15 | `app/chamados/juizes.py:1551` — `q_respondida_sem_texto` | I,T | **E** — face SQL de `I ∧ vazio(strip)`; contempla NULL | Q10 Integridade do par — técnica |
| 16 | `app/chamados/juizes.py:1559` — `q_respondida_com_texto` | I,T | **A** — instante não nulo e regex de texto após trim | Q10 Integridade do par — técnica |
| 17 | `app/chamados/juizes.py:1706` — `porta_alcancavel_do_dia` | I,T, delegados | **E** — exclui `respondida_sem_texto`; exige fila e presença no app | Q11 Alcance/porta |
| 18 | `app/chamados/juizes.py:1718` — `perguntas_corrigiveis` | I | **C** — `I`, mais `pergunta_sem_lastro_ou_viva` | Q02 Correção/reenvio |
| 19 | `app/chamados/juizes.py:1725`, `:1728` — `perguntas_respondidas_a_validar` | I,T | **A** — `q_respondida_com_texto`, `q_vivas` e confirmação Python de vitalidade | Q04 Fila de validação |
| 20 | `app/chamados/juizes.py:1753`, `:1756` — `declarou_hora` | T | **E** — texto não vazio que não seja maiúsculo nem `[descartado...` | Q06 Slot/materialização |
| 21–23 | `app/chamados/juizes.py:1900` — `delta_da_resposta`; `:2004` — `classificar_resposta_validacao`; `:2026` — `motivo_implausivel_validacao` | T | **E**, em cada função — interpreta conteúdo/hora, classe ou plausibilidade | Q12 Classificação da resposta |
| 24 | `app/chamados/juizes.py:2079` — `irmas_respondidas_sem_consumo` | I,T, delegados | **B** — `sem_veredito_entre(..., ignorar_tranca=True)`; universo do crédito | Q13 Retenção por resposta sem veredito |
| 25–26 | `app/chamados/management/commands/auditar_invariantes_chamados.py:177` — `_contraprova_i21a`; `:592` — `Command.handle`, I2 | I | **C**, em ambos — falta/presença do instante, com outros filtros | Q14 Auditoria de invariantes |
| 27 | `app/chamados/management/commands/auditar_invariantes_chamados.py:984` — `Command.handle`, I12 | I,T, delegados | **B** — consulta `irmas_respondidas_sem_consumo` | Q14 Auditoria de invariantes |
| 28 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1105`, `:1106` — `Command.handle`, I20 | I,T | **E** — instante nulo e SQL `texto != ''`; sem trim | Q14 Auditoria de invariantes |
| 29 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1572`, `:1575` — `Command.handle`, I21 | I,T | **A** — instante presente e SQL `texto != ''`; sem trim; não validada/materializada | Q14 Auditoria de invariantes |
| 30 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1603` — `Command.handle`, C2 | I | **C** — instante nulo | Q14 Auditoria de invariantes |
| 31 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1655` — `Command.handle`, F1 | T | **D** — existência de texto após strip interrompe o ramo | Q14 Auditoria de invariantes |
| 32 | `app/chamados/management/commands/backfill_decisoes_pergunta.py:56`, `:60` — `linhas_a_criar` | I,T | **E** — `I OU validada_em`; carrega o par por `.only()` e delega reconstrução | Q15 Preservação/recomposição da trilha |
| 33 | `app/chamados/management/commands/backfill_decisoes_pergunta.py:326` — `aplicar` | T | **E** — copia texto para ato `SILENCIO` | Q15 Preservação/recomposição da trilha |
| 34 | `app/chamados/management/commands/backfill_decisoes_pergunta.py:504`, `:506` — `Command.handle` | T | **D** — `.only()` fornece texto à eleição de canônica | Q16 Deduplicação/mérito |
| 35 | `app/chamados/management/commands/curar_perguntas_em_limbo.py:32` — `nota_da_resposta` | T | **E** — procura no followup o conteúdo literal da resposta, após strip | Q17 Diagnóstico/cura de limbo |
| 36 | `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `:69` — `Command.handle` | I,T | **E** — instante nulo e SQL `texto != ''`; limpeza mantém filtro de instante nulo | Q17 Diagnóstico/cura de limbo |
| 37 | `app/chamados/management/commands/dedup_perguntas_fato.py:75` — `Command.handle` | T | **D** — texto após strip | Q16 Deduplicação/mérito |
| 38 | `app/chamados/management/commands/validar_classe_a.py:54` — `Command.handle` | I,T, delegados | **B** — `respostas_sem_veredito()` | Q04 Fila de validação |
| 39 | `app/chamados/management/commands/validar_classe_a.py:80` — `Command.handle` | T | **F** — amostra truncada no relatório do comando | Q20 Exibição da resposta |
| 40 | `app/chamados/models.py:1533` — `ChamadoColaborador._universo_da_ressuscitacao` | T | **D** — SQL texto NULL ou `''`, sem trim | Q18 Ressuscitação |
| 41 | `app/chamados/models.py:1610`, `:1611` — `ChamadoColaborador.ressuscitar_perguntas` | I,T | **E** — carrega par por `.only()` para preservar projeções sem linha | Q15 Preservação/recomposição da trilha |
| 42 | `app/chamados/models.py:1907`, `:1908` — `DisputaSupervisao.sem_veredito_entre` | I,T | **B** — `I or T`; sem validação/materialização; tranca conforme parâmetro | Q13 Retenção por resposta sem veredito |
| 43–45 | `app/chamados/models.py:1889` — `respostas_sem_veredito`; `:1917` — `pode_fechar`; `:1950` — `fechar` | I,T, delegados | **B**, nos três — consultam a mesma lista; `pode_fechar` também exige ausência de fio vivo | Q13 Retenção por resposta sem veredito |
| 46 | `app/chamados/models.py:1990` — `DisputaSupervisao.fechar` | T | **E** — copia texto para ato `SILENCIO` | Q15 Preservação/recomposição da trilha |
| 47 | `app/chamados/models.py:2122` — `DisputaSupervisao.remover_motivo` | T | **E** — copia texto para ato `RETRATACAO` | Q15 Preservação/recomposição da trilha |
| 48 | `app/chamados/models.py:2922`, `:2923` — `PerguntaDisputa._nascer` | I,T | **E** — recusa nascimento com `I ∧ ¬T` | Q10 Integridade do par — técnica |
| 49 | `app/chamados/models.py:3128`, `:3131` — `PerguntaDisputa.estado` | I | **C** — pendente/respondida pelo instante, após precedências de validação/vitalidade/materialização | Q19 Estado apresentado |
| 50 | `app/chamados/models.py:3210`, `:3213`, `:3214` — `PerguntaDisputa.carimbar` | I,T | **E** — guarda foto anterior; só completa instante se houver texto e faltar instante | Q10 Integridade do par — técnica |
| 51 | `app/chamados/models.py:3343`, `:3351`, `:3354` — `PerguntaDisputa.descarimbar_em_lote` | I,T | **E** — consulta metade não recebida por `.values_list()` e recusa par resultante ilegal | Q10 Integridade do par — técnica |
| 52 | `app/chamados/models.py:3454`, `:3468` — `PerguntaDisputa.q_vivas` | I,T, delegados | **A** — degrau de resposta usa `q_respondida_com_texto`; queryset completo combina outros degraus | Q08 Vitalidade |
| 53 | `app/chamados/models.py:3534` — `PerguntaDisputa.save` | I,T | **E** — guarda par carregado depois do save | Q10 Integridade do par — técnica |
| 54 | `app/chamados/models.py:3555`, `:3557` — `PerguntaDisputa._par_lido` | I,T | **E** — lê par de `__dict__` somente se ambas as colunas estiverem carregadas | Q10 Integridade do par — técnica |
| 55 | `app/chamados/models.py:3562`, `:3566` — `PerguntaDisputa._recusa_par_ilegal` | I,T | **E** — compara foto e recusa transição para `I ∧ ¬T` | Q10 Integridade do par — técnica |
| 56 | `app/chamados/models.py:3641` — `PerguntaDisputa.respondida` | I | **C** — `respondida_em is not None` | Q19 Estado apresentado |
| 57 | `app/chamados/models.py:3664`, `:3669`, `:3677`, `:3684`, `:3686` — `PerguntaDisputa.resposta_display` | T | **F** — resolve rótulo de posto/escala/opção ou devolve texto | Q20 Exibição da resposta |
| 58 | `app/chamados/models.py:4067`, `:4074`, `:4077` — `DecisaoPergunta.linhas_da_projecao_sem_linha` | I,T | **C** — ato de resposta exige instante; texto é copiado, sem exigir presença; validação tem ramo próprio | Q15 Preservação/recomposição da trilha |
| 59 | `app/chamados/reconciliador.py:365` — `lavrar_sem_turno_aberto` | T | **D** — texto após strip impede lavratura | Q21 Reconciliação de órfãs |
| 60 | `app/chamados/reconciliador.py:448` — `retencao_da_retratacao` | I,T, delegados | **B** — irmãs sem consumo | Q13 Retenção por resposta sem veredito |
| 61 | `app/chamados/reconciliador.py:1083` — `veredito_orfa` | T | **D** — texto após strip impede o ramo | Q21 Reconciliação de órfãs |
| 62 | `app/chamados/reconciliador.py:1164` — `encerrar_orfas_pela_lampada` | I,T, delegados | **B** — lista sem veredito | Q13 Retenção por resposta sem veredito |
| 63 | `app/chamados/relatorios_disputa.py:24`, `:26` — `classificar` | I,T, delegados | **E** — C7 primeiro; depois existência de viva com `¬I ∨ ¬T` distingue colaborador/supervisão | Q22 Cobrança/dono da espera |
| 64 | `app/chamados/services/acoes_disputa.py:95` — `acao_reabrir_disputa` | I | **C** — conta instante nulo | Q02 Correção/reenvio |
| 65 | `app/chamados/services/acoes_disputa.py:130` — `acao_regularizar_disputa` | I | **C** — alguma viva sem instante impede regularizar | Q23 Entrada da regularização |
| 66 | `app/chamados/services/acoes_disputa.py:353`, `:359` — `acao_corrigir_resposta` | T | **E** — preserva texto anterior em nota de correção | Q15 Preservação/recomposição da trilha |
| 67 | `app/chamados/services/becos.py:30` — `classificar` | I | **C** — instante nulo e destino não resolvido | Q21 Reconciliação de órfãs |
| 68 | `app/chamados/services/cobranca.py:237`, `:241` — `_calados` | I,T, delegados | **E** — viva que espera texto: `¬I ∨ ¬T`; disputa aberta | Q22 Cobrança/dono da espera |
| 69 | `app/chamados/services/competencia_trancada.py:126` — `medir` | I,T, delegados | **B** — sem veredito ignorando tranca | Q13 Retenção por resposta sem veredito |
| 70 | `app/chamados/services/declaracao_texto.py:417` — `declarar_hora_colab` | I,T | **E** — instante verdadeiro e texto exatamente igual à hora solicitada | Q06 Slot/materialização |
| 71 | `app/chamados/services/declaracao_texto.py:419` — `declarar_hora_colab` | I | **C** — `bool(instante)` informa reenvio ao escritor | Q02 Correção/reenvio |
| 72 | `app/chamados/services/declaracao_texto.py:687` — `aplicar_declaracao` | I | **C** — ausência de instante dispara gravação de resposta | Q23 Entrada da regularização |
| 73 | `app/chamados/services/disputa_emissao.py:1635`, `:1646` — `reabrir_para_correcao` | I,T | **E** — cria foto se texto OU instante OU validação OU materialização; exclui fora do fato | Q15 Preservação/recomposição da trilha |
| 74 | `app/chamados/services/disputa_emissao.py:1663` — `responder_automaticamente` | T | **D** — texto após strip já existente impede resposta automática | Q24 Resposta automática |
| 75 | `app/chamados/services/disputa_emissao.py:1691` — `carimbar_resposta_pela_trilha` | I,T | **E** — só age com `¬I ∧ T` | Q17 Diagnóstico/cura de limbo |
| 76 | `app/chamados/services/disputa_emissao.py:1734` — `recobrar_questionario` | I | **C** — conta instante nulo | Q22 Cobrança/dono da espera |
| 77 | `app/chamados/services/disputa_emissao.py:1842` — `reabrir_credor_de_pergunta_viva` | I | **C** — instante presente exclui esta porta; também exige viva | Q11 Alcance/porta |
| 78 | `app/chamados/services/disputa_emissao.py:1860` — `reabrir_credor_de_pergunta_viva` | I,T | **E** — carga contém limbo se `¬I ∧ T` | Q17 Diagnóstico/cura de limbo |
| 79 | `app/chamados/services/disputa_emissao.py:2000` — `_resolver_perguntas_por_chamado_resolvido` | I,T | **A** — par com texto não materializado de fato é preservado | Q13 Retenção por resposta sem veredito |
| 80 | `app/chamados/services/disputa_emissao.py:2030` — mesma função | I,T, delegados | **B** — irmãs sem consumo protegem pai | Q13 Retenção por resposta sem veredito |
| 81 | `app/chamados/services/disputa_emissao.py:2078` — mesma função | T | **D** — `_tem_resp = bool(strip(texto))` | Q13 Retenção por resposta sem veredito |
| 82 | `app/chamados/services/disputa_emissao.py:2073`, `:2090` — mesma função | I | **E** — fotos antes/depois para auditoria | Q15 Preservação/recomposição da trilha |
| 83–84 | `app/chamados/services/explicador.py:32` — `_explicar_validada`; `:79`, `:80` — `explicar_pergunta` | T | **F**, em ambos — texto compõe explicação ao usuário | Q20 Exibição da resposta |
| 85 | `app/chamados/services/fila_por_causa.py:226` — `_gaveta_de` | I,T, delegados | **B** — existência de resposta sem veredito seleciona gaveta | Q04 Fila de validação |
| 86 | `app/chamados/services/fio.py:174` — `aguarda_o_colaborador` | I,T, delegados | **E** — existe viva dentro do recorte `¬I ∨ ¬T` | Q22 Cobrança/dono da espera |
| 87 | `app/chamados/services/fio.py:187` — `espera_o_texto_do_colaborador` | I,T | **E** — `¬I or respondida_sem_texto(p)`, equivalente ao recorte `¬I ∨ ¬T` | Q22 Cobrança/dono da espera |
| 88 | `app/chamados/services/fio.py:357`, `:378` — `montar_chamados_fio` | I,T, delegados | **E** — deriva `resposta_enviada` e `todas_resp` pelos dois leitores anteriores | Q22 Cobrança/dono da espera |
| 89 | `app/chamados/services/fio.py:698` — `processar_post_fio` | I | **C** — alguma viva sem instante impede regularização | Q23 Entrada da regularização |
| 90–91 | `app/chamados/services/fio.py:1271` — `canonica_do_grupo`; `:1283` — `merito_perdedor` | T | **D**, em ambos — texto após strip; segundo também exige via admin | Q16 Deduplicação/mérito |
| 92 | `app/chamados/services/fio.py:1602` — `recusa_respondida_sem_texto` | I,T | **E** — recusa `I ∧ ¬T` pelo predicado delegado | Q10 Integridade do par — técnica |
| 93 | `app/chamados/services/foto_e4b.py:179`, `:180` — `nascimentos` | I | **E** — transição antes não nulo/depois nulo em qualquer assinatura listada | Q18 Ressuscitação |
| 94 | `app/chamados/services/historico.py:99`, `:105`, `:106` — `_eventos_dos_chamados` | I | **C** — `getattr(p, campo)` seleciona evento pelo instante e marco `desde` | Q25 Linha do tempo |
| 95 | `app/chamados/services/historico.py:109` — mesma função | T | **F** — texto truncado ou “sem resposta” no evento | Q25 Linha do tempo |
| 96 | `app/chamados/services/janela_colab.py:63` — `chamados_visiveis_colab` | I,T, delegados | **A** — degrau de resposta herdado de `q_vivas`, dentro da consulta abrangente | Q11 Alcance/porta |
| 97 | `app/chamados/services/materializacao.py:280` — `_tipo_pelo_marco` | T | **E** — converte resposta em minutos para identificar marco/tipo | Q06 Slot/materialização |
| 98 | `app/chamados/services/materializacao.py:359`, `:373`, `:412` — `_ts_tipo_do_plantio` | T | **E** — escape/default do relógio, régua e parse da hora | Q06 Slot/materialização |
| 99 | `app/chamados/services/materializacao.py:438` — `estado_aplicacao_pergunta` | T | **D** — exige texto após strip, além de tipo hora | Q06 Slot/materialização |
| 100 | `app/chamados/services/materializacao.py:474` — `_declarar_ausencia_de_pergunta` | T | **E** — conteúdo da resposta determina ausência/opção | Q26 Escape/ausência/folga |
| 101 | `app/chamados/services/materializacao.py:571` — `_materializar_ausencia_de_resposta` | T | **E** — copia texto para ato `ABSORCAO` | Q15 Preservação/recomposição da trilha |
| 102 | `app/chamados/services/materializacao.py:633` — `perguntas_absorvidas_por` | T | **E** — SQL `resposta_colab__in=escapes` | Q26 Escape/ausência/folga |
| 103 | `app/chamados/services/materializacao.py:674`, `:675` — `devolver_absorvidas_da_ausencia_rejeitada` | I,T | **E** — foto do estado anterior à devolução | Q15 Preservação/recomposição da trilha |
| 104 | `app/chamados/services/materializacao.py:768` — `_reconciliar_destino_e_clones._fecha` | I,T, delegados | **B** — irmãs sem consumo impedem fecho | Q13 Retenção por resposta sem veredito |
| 105 | `app/chamados/services/materializacao.py:835` — `_materializar_resposta_hora` | T | **D** — sem texto após strip recusa plantio | Q06 Slot/materialização |
| 106 | `app/chamados/services/materializacao.py:1001`, `:1010` — `_materializar_resposta_data_hora` | T | **E** — parse de data/hora; conteúdo também entra no erro/auditoria | Q06 Slot/materialização |
| 107 | `app/chamados/services/materializacao.py:1376` — `materializar_perguntas_validadas_da_disputa` | I | **C** — instante presente, validada e não materializada | Q06 Slot/materialização |
| 108 | `app/chamados/services/painel.py:67` — `marcador_do_placar` | I | **E** — `Max('respondida_em')` para marcador | Q07 Cache/invalidação — técnica |
| 109 | `app/chamados/services/passivo_validado.py:66`, `:83` — `_reversao_de` | I | **E** — `getattr` dirigido por `CAMPOS_REVERSAO`, serializado em ISO | Q15 Preservação/recomposição da trilha |
| 110 | `app/chamados/services/pergunta_no_app.py:53`, `:61` — `perguntas_do_app` | I,T, delegad

[truncado em 20000 de 54902 caracteres]

## 15/11 17:51 · sessao 24efca2a894e · - · - · origem=Ronald (tmux)

| Sítio escritor | Função / valor | `DecisaoPergunta` e transação |
|---|---|---|
| `app/chamados/models.py:3097`, `:3100` | `PerguntaDisputa._nascer`: instancia com `**campos`, salva inserção; recebe o par opcionalmente; defaults `resposta_colab=''`, `respondida_em=NULL` (`:2525`, `:2526`). Recusa instante sem texto antes da escrita (`:2922`). | Mesma transação: `registrar(linhas_da_projecao_sem_linha([p]))` (`:3102`), sob `atomic(savepoint=False)` (`:2924`) e `atomic` de `_sob_contrato` (`:2897`). Atos condicionais `resposta` / `validacao` conforme assinaturas (`:4074`, `:4078`); nascimento pendente produz lista vazia (`:4067`). |
| `app/chamados/services/disputa_emissao.py:1481`, `:1482`, `:1522` | `responder_pergunta_disputa_core`: texto normalizado por `.strip()` (`:1446`), instante `_tz.now()` (`:1476`); `save(update_fields=_campos)`. | Mesma transação `atomic` (`:1517`): ato `RESPOSTA` em `:1525`; também preserva projeção anterior sem linha (`:1480`, `:1525`). |
| `app/chamados/services/disputa_emissao.py:1673`, `:1674`, `:1676` | `responder_automaticamente`: `resposta_colab=texto`, `respondida_em=instante`, `save(update_fields=['resposta_colab','respondida_em'])`; guarda texto existente (`:1663`) e recusa par ilegal (`:1671`). | Mesma transação (`:1675`): ato `RESPOSTA` (`:1677`), mais projeção anterior sem linha (`:1672`). Chamador de foto passa literal não vazio definido em `app/api/foto_recebida.py:25`, chamada `:48`. |
| `app/chamados/services/disputa_emissao.py:1695`, `:1697` | `carimbar_resposta_pela_trilha`: `respondida_em=instante`; apenas com texto e sem instante (`:1691`); `save(update_fields=['respondida_em'])`. | Mesma transação (`:1696`): ato `RESPOSTA` (`:1698`). Instante vem de `nota.criado_em` no comando (`app/chamados/management/commands/curar_perguntas_em_limbo.py:67`). |
| `app/chamados/models.py:3215`, `:3235` | `PerguntaDisputa.carimbar`: `respondida_em=quando`, somente se `resposta` habilitada, texto não vazio e instante ausente (`:3213`, `:3214`); acrescenta campo à lista salva (`:3216`). `quando` recebido ou `now()` (`:3207`). | A porta não registra ato; `save` abre transação própria (`:3532`). A garantia de decisão depende do chamador — ver linhas seguintes. |
| `app/chamados/services/materializacao.py:567` | `_materializar_ausencia_de_resposta`: chama `carimbar(..., resposta='validar')`; pode preencher instante de irmã com texto. | Mesma transação (`:563`): ato `ABSORCAO` (`:569`), condicionado ao sucesso do carimbo (`:568`). |
| `app/chamados/services/materializacao.py:1283` | `_materializar_uma`: chama `carimbar(..., resposta='validar')`; pode preencher instante com texto. | Corpo tem `atomic` (`:1226`) mas não registra decisão própria. Na rota `validar_pergunta`, compartilha transação externa (`app/chamados/services/validacao.py:450`) com ato `VALIDACAO` (`:460` → `:164`) quando unidade chamada em `:488`. O lote posterior é chamado fora desse bloco (`:494`); não há garantia geral de ato nesta escrita. |
| `app/chamados/services/disputa_emissao.py:2007` | `_resolver_perguntas_por_chamado_resolvido`: `carimbar(..., resposta='validar')`. | Nenhum ato próprio de `DecisaoPergunta` no corpo. Transação garantida pelo wrapper `resolver_perguntas_por_chamado_resolvido` (`:1900`); existência de ato incidental em todo chamador: **[nao verificado]**. |
| `app/chamados/services/disputa_emissao.py:2081` | Mesma função: `carimbar(..., resposta='responder')`; somente instante com texto pela porta (`models.py:3213`). | Nenhum ato próprio de `DecisaoPergunta` no corpo; escreve evento de auditoria (`:2084`), que não é decisão. Transação do wrapper (`:1900`). |
| `app/chamados/services/disputa_emissao.py:1609`, `:1648` → `app/chamados/models.py:3363` | `reabrir_para_correcao`: passa `respondida_em=None`, `resposta_colab=''` em `campos`; porta `descarimbar_em_lote` executa `update(..., **outros)` (`models.py:3365`). | Mesma transação (`disputa_emissao.py:1621`): ato parametrizado `ato` (`:1632`), default `reabertura` (`:1576`); `recusa` via `validacao.py:629`. Registra somente linhas das quais retira texto/carimbo/validação/materialização (`:1645`, `:1646`); exclui vias fora do fato na decisão e na escrita (`models.py:3364`). |
| `app/chamados/models.py:1613` → `:3363` | `ChamadoColaborador.ressuscitar_perguntas`: zera somente `respondida_em` do par; não apaga texto; seleção prévia admite texto NULL ou `''` (`:1533`). | Mesma transação (`:1604`): preserva decisões projetadas ainda sem linha (`:1610`). Atos `resposta`/`validacao` condicionais (`:4074`, `:4078`); não cria ato novo de reabertura, podendo registrar nenhum se não há projeção órfã. |
| `app/chamados/services/validacao.py:628` | `recusar_pergunta`: delega zeragem do par a `reabrir_para_correcao`, com `ato='recusa'` (`:629`). | Mesma transação externa (`:623`) e interna da porta (`disputa_emissao.py:1621`), ato `recusa` (`:1632`). |
| `app/chamados/services/materializacao.py:688` | `devolver_absorvidas_da_ausencia_rejeitada`: delega zeragem a `reabrir_para_correcao`. | Mesma transação externa (`:681`), ato default `reabertura` (`disputa_emissao.py:1576`, `:1632`). |
| `app/chamados/management/commands/curar_perguntas_em_limbo.py:69` | `Command.handle`: delega limpeza de texto sem instante à reabertura; só executa após `--apply` (`:64`). | Mesma transação da porta, ato `reabertura` conforme condição da foto (`disputa_emissao.py:1621`, `:1632`, `:1646`). |
| `app/chamados/models.py:1555` | `_universo_da_ressuscitacao`: `_q.respondida_em=None` em cópia (`:1541`). | **Só memória**, sem persistência nem ato desta atribuição (`:1556`). |
| `app/chamados/models.py:3246` | `_desfazer_em_memoria`: restaura `self.respondida_em=respondida` da foto anterior (`:3244`). | **Só memória**, sem persistência; chamado ao recusar ou falhar carimbo (`:3224`, `:3227`, `:3238`). |
| `bin/sombra_mascara_pessoal.py:57`, `:62`, `:64` | Escritor operacional **da cópia sombra**: `setattr(o,c,novo)` e `_base_manager.bulk_update`; tabela inclui `PerguntaDisputa.resposta_colab` (`bin/sombra_regras_pessoais.py:85`). `novo=texto(v)` (`sombra_mascara_pessoal.py:55`); transformação preserva formatos aceitos ou produz marcador/hash (`sombra_regras_pessoais.py:29`). | Nenhum ato próprio da alteração; transação da máscara (`sombra_mascara_pessoal.py:16`). Banco exigido `sombra` (`:10`); não contar como escritor de produção. |

Observações verificadas: `PerguntaDisputa.descarimbar()` só salva os três campos de materialização, sem tocar o par (`app/chamados/models.py:3293`); o `bulk_update` de lavras salva apenas `veredito_lavrado`, `julgado_em`, `janela_expira_em` (`app/chamados/veredito_lavrado.py:237`, `:250`). `devolver_validacao_recusada` conserva a resposta e salva somente `validada_em`/`validada_por` (`app/chamados/services/validacao.py:204`, `:227`), embora registre ato `devolucao` (`:230`).

Script AST inicial executado via stdin, sem arquivo:

```python
import ast, pathlib
fields = {'respondida_em', 'resposta_colab'}

for p in pathlib.Path('.').rglob('*.py'):
    if 'tests' in p.parts or 'migrations' in p.parts:
        continue
    try:
        t = ast.parse(p.read_text())
    except (SyntaxError, UnicodeError):
        continue
    parent = {c: n for n in ast.walk(t)
              for c in ast.iter_child_nodes(n)}

    def fn(n):
        while n in parent:
            n = parent[n]
            if isinstance(n, (ast.FunctionDef, ast.AsyncFunctionDef)):
                return n.name
        return '<module>'

    for n in ast.walk(t):
        hit = False
        if isinstance(n, (ast.Assign, ast.AnnAssign, ast.AugAssign)):
            targets = n.targets if isinstance(n, ast.Assign) else [n.target]
            hit = any(
                isinstance(x, ast.Attribute) and x.attr in fields
                for target in targets for x in ast.walk(target)
            )
        if isinstance(n, ast.Call):
            name = ast.unparse(n.func)
            if name.endswith((
                '.update', '.create', '.get_or_create',
                '.update_or_create', '.bulk_update', '.save'
            )) or name == 'setattr':
                hit = any(
                    isinstance(x, ast.Constant)
                    and isinstance(x.value, str)
                    and x.value in fields
                    for x in ast.walk(n)
                ) or any(k.arg in fields for k in n.keywords)
        if hit:
            print(f'{p}:{n.lineno} {fn(n)} {ast.unparse(n)}')
```

Foram executados complementos AST para: chamadas com keywords/literais dos campos (achou `dict(...)` e chamada indireta de lote); todas as chamadas `carimbar`, `descarimbar_em_lote`, `reabrir_para_correcao`, `bulk_update`; `setattr`/`getattr` literais e subscripts de escrita; constantes contendo campos e termos SQL de escrita. O primeiro script sozinho **não prova completude**, pois não faz fluxo de dados interprocedural/aliases; a conferência dos sítios indiretos acima foi manual a partir da AST. Nenhum SQL literal de escrita dos campos foi encontrado pelos complementos; SQL gerado dinamicamente arbitrário permanece **[nao verificado]**.

## 15/11 17:51 · sessao dfb8f9bc785a · - · - · origem=Ronald (tmux)

Inventário de testes obtido por AST e conferência das asserções. Os testes não foram executados; resultado em execução: [nao verificado]. [nao verificado]

| Teste / asserção | Predicado efetivamente fixado | Corte/lei citado no próprio teste | [nao verificado]
|---|---|---|
| `app/chamados/tests/test_e3_forma_sql_do_juiz.py:91`, `:95` — `test_face_sem_texto_igual_ao_juiz` / `test_face_com_texto_igual_ao_juiz` | As faces SQL equivalem, em `filter`, `exclude` e negação, a **instante E vazio(strip)** / **instante E texto(strip)**; comparação definida em `:69-80`. | `E3-FORMA-SQL-DO-JUIZ`, aval Ronald 12/11 19:1x, itens 1 e 2, em `:1-15`. |
| `app/chamados/tests/test_e3_forma_sql_do_juiz.py:185`, `:190` | `q_vivas`: instante + brancos não entra pelo degrau da resposta; instante + `' 14:20 '` entra. | Mesma origem `:1-15`; especificação de `q_vivas` em `:160-164`. |
| `app/chamados/tests/test_e3_forma_sql_do_juiz.py:259`, `:260` | Fila de validar inclui par com texto; exclui instante + espaços. | Mesma origem `:1-15`. |
| `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:283`, `:287`, `:289` — `test_RED_retratacao_pergunta_o_MESMO_juiz_do_veredito` | `irmas_respondidas_sem_consumo`: **instante OU texto**; instante sem texto segura, texto sem instante segura, com validação libera. | Docstring `:276-279`: aval `O168-JUIZ-DO-VEREDITO`, literalmente contrapõe antigo E ao atual OU; medição 13/11. |
| `app/chamados/tests/test_e3_10_cura_no_escritor.py:132`, `:133`, `:134`, `:140`, `:141`, `:147` | `respostas_sem_veredito` inclui instante + vazio/espaços/par; impede fechamento; `sem_veredito_entre` inclui instante sem texto. | Docstring da classe `:122-124`: `E3-V8-TEXTO-VAZIO`, 14/11 07:4x, “caracterizacao invertida do E3-10 item 2”. O header antigo `:10-12` não descreve essa caracterização atual. |
| `app/chamados/tests/test_service_validar_puro.py:101`, `:102`, `:103` — `test_05_sem_resposta_nao_valida` | Texto `'17:00'` sem instante é recusado por `validar_pergunta`: `sem_resposta`; fixa necessidade do instante, sem provar sozinho suficiência. Fixture `:49-51`, retirada do instante `:98`. | Header `:1-13`: peça 1 da fatia 2-A, 09/08. Corte específico para esse predicado: [nao verificado]. |
| `app/chamados/tests/test_pergunta_estado.py:59`, `:60` — `test_reabertura_volta_a_ser_viva` | `.estado`: texto `'19:00'` sem instante resulta `pendente`; junto de `:35-37`, par resulta `respondida`. | Comentário `:57`: reabertura limpa instante/validação e mantém texto; sem corte nomeado nesse teste [nao verificado]. |
| `app/chamados/tests/test_o261e_f5_alcance.py:194`, `:196`, `:197`, `:199`, `:200` | Instante sem texto sai dos pendentes do app; tarefa preserva `respondida_sem_texto`; vigia mede fio morto causa `b`, sem porta. | Docstring `:185-189`: aval `E3-V10-DEGRAU-DONO-E-PORTA` 14/11 16:16; explicita diferença entre app e crédito. |
| `app/chamados/tests/test_o261e_f5_alcance.py:378`, `:380`, `:386`, `:388` | Sem resposta: porta app/colab e verbo cobrar; depois de gravar par + decisão (`:381-384`): porta pergunta/admin, validar. | Docstring `:373-374`: `E3 b3`, aval `E3-TRIAGEM-FABLE` 12/11; comentário `:382`: E1. |
| `app/chamados/tests/test_e4b_c6_orm.py:130`, `:132` — `_provar`, exercitado por `test_RED_limbo_misto_fecha_mortas_orfa_e_entrega_motivo_proprio` `:153-156` | Texto sem instante na irmã morta produz recusa `credor_carrega_limbo`, materializa mortas e conserva fechamento do chamado. Fixture: texto `:77`, instante nulo `:86`; assert de materialização `:115`. | Comentário `:154-155`: F1 e S143; header `:1`: c6, admin fecha, S143 recusa só reabertura. |
| `app/chamados/tests/test_e4b_c11_orm.py:121-122` — `test_RED_A6_beneficio_rejeitado_limbo` | Repete caracterização de limbo de c6 pelo caminho de benefício rejeitado: herda classe c6 em `:27`, chama `_provar(limbo=...)` em `:99-101`, com asserts efetivos em `test_e4b_c6_orm.py:130-132`. | Header c11 `:1-4`: provas ORM c11, S143 e C6 reais; nomes F1/S143 no comentário herdado c6 `:154-155`. |
| `test_e4c_f1v6_orm.py` | Arquivo não encontrado na árvore disponibilizada; conteúdo, predicado e origem: **[nao verificado]**. | **[nao verificado]**. |
| `app/chamados/tests/test_e3_item1b.py:94`, `:108`, `:114` | Relatório: viva com instante sem texto → `AGUARDA_COLAB`; par → `AGUARDA_SUPERVISAO`; sem resposta → `AGUARDA_COLAB`. | Header `:1-12`: E3 item1b, LEI-AKITA 13, casos pela regra. |
| `app/chamados/tests/test_e3_item1b.py:124`, `:130`, `:136` | Cobrança: instante sem texto conta como calado; par não conta; morta pelo C6 não conta. | Header `:3-10`: LEI-AKITA 13, item 1(c). |
| `app/chamados/tests/test_e3_item1b.py:154`, `:155`, `:156`, `:162`, `:163`, `:171` | Template do colaborador: instante sem texto não exibe “Resposta enviada”/“Reenviar resposta”; par exibe; contexto derivado pode apagar caixa mesmo com par. | Header `:11-12`: item 1(d); teste de contexto `:165-166`. |
| `app/chamados/tests/test_e3_leitores_fio_disputa.py:131`, `:136`, `:141`, `:146` | `todas_respondidas`: morta sem resposta não fica devendo; viva sem resposta ou instante sem texto fica devendo; viva com par não fica. | Header `:2-11`: `FIO-E0-V4-E3-LEITORES`, aval `CASO-7445-COMPLEMENTO-E3`; C6, C7, `respondida_sem_texto`. |
| `app/chamados/tests/test_e3_leitores_fio_disputa.py:161`, `:167` | API: instante sem texto não pode solicitar correção; com par pode. | Mesma origem `:2-11`; comentário `:149`: API lê mesma derivação. |
| `app/chamados/tests/test_sem_resposta_nao_planta_batida.py:99`, `:104`, `:111`, `:125` | Materialização recusa instante sem texto; par com texto produz batida. Fixação efetiva distingue texto, com os carimbos mantidos nas duas fixtures (`:72-86`). | Aval Ronald 30/09 ~13:3x em `:2`; lei “motor NUNCA fabrica batida” em `:9-10`; medição dos 8.622 carimbos sem texto em `:116-120`. **A frase OU do header `:14-15` diverge dessas asserções.** |
| `app/chamados/tests/test_o261_resposta_vazia.py:85`, `:95`, `:109`, `:113`, `:149`, `:151`, `:153`, `:155` | Escritor não carimba vazio nem espaços; recusa carimbar texto se C6 o daria vivo; contador conta instante sem texto apenas após marco, espaços equivalem a vazio. | Header `:2-17`: `O261-RESPOSTA-VAZIA-COM-LEITOR-VIVO`, aval 29/10 17:2x; comentário `:97-102`: inversão O261-E-F2, mandato F2 e LEI-AKITA 2. |
| `app/chamados/tests/test_e3_10_cura_no_escritor.py:63-69`, `:77`, `:106-107`, `:168-170`, `:176-184`, `:205-212` | Guardas de escrita: resposta automática/nascimento/save/lote recusam transição para instante sem texto; par legal passa; legado ilegal pode alterar outros campos (`:186-197`). | Header `:1-13`: E3-10, aval Ronald 12/11 item 2; classe `:152-155`: E3 item1b item 3, LEI-AKITA 7. |
| `app/chamados/tests/test_reabrir_nao_destroi.py:49-52`, `:58`, `:64`, `:72` | Devolução: zera texto/instante/validação; guarda texto e instante originais na decisão `reabertura`; pendente não fabrica versão; duas devoluções geram uma versão. | Header `:1-5`: `REABRIR-NAO-DESTROI`, aval 21/10 RAIA D, resposta é fato. |
| `app/chamados/tests/test_e3_porta_A_reabrir.py:41-42`, `:51`, `:62-64`, `:82-83` | Reabertura comum zera par/materialização/via; carimbo sozinho deixa ato `reabertura`; fora do fato conserva par e não gera decisão; recusa comum zera par e gera ato `recusa`. | Header `:1-10`: aval `E3-PORTA-DA-RESPOSTA = (A)`, item 2, Single Writer Principle. |
| `app/chamados/tests/test_contract_par_resposta.py:90-95`, `:111-113`; `app/chamados/tests/test_contract_texto_com_carimbo.py:119`, `:124`, `:136`, `:138`, `:166-172` | Contratos estáticos de escrita do par; não provam equivalência dos leitores nem atomicidade por execução. | Primeiro `:1-2`: HX-PAR-RESPOSTA, E2 item 2, 03/09; segundo `:1-5`: BUG121, 12/09, ordem “MESMO atomic”. |

Script efetivamente usado para localizar candidatos Python (a interpretação acima foi conferida lendo os corpos e asserções):

```bash
python3 -B - <<'PY'
import ast, pathlib
for p in pathlib.Path('app').rglob('test*.py'):
    try:
        t = ast.parse(p.read_text())
    except (SyntaxError, UnicodeError):
        continue
    hits = []
    for n in ast.walk(t):
        if isinstance(n, ast.Attribute) and n.attr in {
            'respondida_em', 'resposta_colab'
        }:
            hits.append(n.lineno)
        if isinstance(n, ast.keyword) and n.arg and any(
            x in n.arg.split('__')
            for x in ('respondida_em', 'resposta_colab')
        ):
            hits.append(n.lineno)
        if isinstance(n, ast.Constant) and isinstance(n.value, str) \
                and n.value.split('__')[-1] in (
                    'respondida_em', 'resposta_colab'
                ):
            hits.append(n.lineno)
    if hits:
        print(p, ','.join(map(str, sorted(set(hits)))))
PY
```

Script adicional efetivamente usado para localizar testes dos helpers OR, inclusive arquivos sem acesso direto aos campos: [nao verificado]

```bash
python3 -B - <<'PY'
import ast, pathlib
targets = {
    'sem_veredito_entre',
    'irmas_respondidas_sem_consumo',
    'respostas_sem_veredito',
}
for p in pathlib.Path('app').rglob('test*.py'):
    try:
        t = ast.parse(p.read_text())
    except (SyntaxError, UnicodeError):
        continue
    for n in ast.walk(t):
        if isinstance(n, (ast.Attribute, ast.Name)) and \
                getattr(n, 'attr', getattr(n, 'id', '')) in targets:
            print(p, n.lineno, ast.unparse(n))
PY
```

Limite: esses scripts descobrem candidatos e helpers pelo AST; não fazem resolução de tipos/imports nem provam cobertura exaustiva de predicados exercitados indiretamente. O script central do censo deve incorporar a resolução de aliases solicitada pelo usuário; estes são os scripts auxiliares de investigação de testes. [nao verificado]

## 15/11 17:51 · sessao 834316ebacdd · JUIZ-RESPONDIDA · sonda · origem=Code

1. O universo informado não é vazio: **50292** perguntas, **14790** candidatas em `q_vivas` e **899** com ato `RESPOSTA`. A soma fecha: **8192 + 9626 + 0 + 32474 = 50292**. E3 é zero por diferença; não há linha `CONT E3` na saída. [nao verificado]

2. Tabela consolidada do dado fornecido. Nas quatro colunas booleanas, cada célula apresenta **False / True**; os cruzamentos completos estão na transcrição abaixo. [nao verificado]

| Estado | Situação | Total | validada F/T | materializada F/T | viva F/T | ato RESPOSTA F/T | Fonte |
|---|---|---:|---:|---:|---:|---:|---|
| E1 instante E texto | ABERTA | 4011 | 1100 / 2911 | 974 / 3037 | 3059 / 952 | 3193 / 818 | [nao verificado] |
| E1 instante E texto | ENTREGUE | 4181 | 346 / 3835 | 139 / 4042 | 4014 / 167 | 4124 / 57 | [nao verificado] |
| E2 instante sem texto | ABERTA | 1502 | 214 / 1288 | 2 / 1500 | 1500 / 2 | 1502 / 0 | [nao verificado] |
| E2 instante sem texto | ENTREGUE | 8124 | 703 / 7421 | 111 / 8013 | 7983 / 141 | 8124 / 0 | [nao verificado] |
| E3 texto sem instante¹ | ABERTA | 0 | 0 / 0 | 0 / 0 | 0 / 0 | 0 / 0 | [nao verificado] |
| E3 texto sem instante¹ | ENTREGUE | 0 | 0 / 0 | 0 / 0 | 0 / 0 | 0 / 0 | [nao verificado] |
| E4 nenhum | ABERTA | 19017 | 19017 / 0 | 7244 / 11773 | 11847 / 7170 | 18993 / 24 | [nao verificado] |
| E4 nenhum | ENTREGUE | 13457 | 13457 / 0 | 3808 / 9649 | 10652 / 2805 | 13457 / 0 | [nao verificado] |

¹ E3 deduzido por diferença, sem contagem `CONT` explícita. As somas internas da tabela conferem com os totais e marginais fornecidos; o total com ato é **875 + 24 = 899**. [nao verificado]

3. **As expressões exatas usadas pelo script não são verificáveis:** `sonda_quadro.py` não foi localizado na cópia e seu conteúdo não acompanha a saída fornecida. [nao verificado]

A correspondência lógica dos rótulos é a seguinte, tomando `I = p.respondida_em is not None` e `T = not texto_vazio(p.resposta_colab)`; atribuir essas expressões ao script permanece [nao verificado].

| Estado | Expressão lógica do rótulo | Evidência na cópia |
|---|---|---|
| E1 | `I and T` | Predicado descrito e face SQL `q_respondida_com_texto()` em [app/chamados/juizes.py:1554](/home/ronald/codex_ws/64d09171097ada8e10f851c6358e63ca293d643f/app/chamados/juizes.py:1554) |
| E2 | `I and not T` | Predicado em [app/chamados/juizes.py:1531](/home/ronald/codex_ws/64d09171097ada8e10f851c6358e63ca293d643f/app/chamados/juizes.py:1531); face SQL em [app/chamados/juizes.py:1551](/home/ronald/codex_ws/64d09171097ada8e10f851c6358e63ca293d643f/app/chamados/juizes.py:1551) |
| E3 | `not I and T` | [nao verificado] |
| E4 | `not I and not T` | [nao verificado] |

O juiz define `texto_vazio(t)` como `not (t or '').strip()`. [app/chamados/juizes.py:1525](/home/ronald/codex_ws/64d09171097ada8e10f851c6358e63ca293d643f/app/chamados/juizes.py:1525)

Sobre E3 ter face no juiz: o enunciado informa que o script declara que não; essa declaração não aparece na saída transcrita, e não foi possível conferir o script. [nao verificado]

4. `PKS_VIVAS_ABERTA`: **E2 = 2**, PKs **38251 e 38252**; **E3 = 0**, lista vazia. Total das duas listas: **2**. [nao verificado]

Transcrição integral da saída fornecida, sem nova execução nem verificação em banco ou logs. [nao verificado]

```text
sonda_leitura: sonda_quadro.py [8d93c0f8e754674b] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

UNIVERSO PerguntaDisputa total 50292 | candidatas q_vivas 14790 | com ato RESPOSTA 899
SOMA_DOS_ESTADOS 50292 TOTAL 50292 FECHA
CONT E1_instante_E_texto ABERTA 4011
CONT E1_instante_E_texto ABERTA validada False materializada False viva True ato False 497
CONT E1_instante_E_texto ABERTA validada False materializada False viva True ato True 428
CONT E1_instante_E_texto ABERTA validada False materializada True viva False ato False 106
CONT E1_instante_E_texto ABERTA validada False materializada True viva False ato True 42
CONT E1_instante_E_texto ABERTA validada False materializada True viva True ato False 24
CONT E1_instante_E_texto ABERTA validada False materializada True viva True ato True 3
CONT E1_instante_E_texto ABERTA validada True materializada False viva False ato False 47
CONT E1_instante_E_texto ABERTA validada True materializada False viva False ato True 2
CONT E1_instante_E_texto ABERTA validada True materializada True viva False ato False 2519
CONT E1_instante_E_texto ABERTA validada True materializada True viva False ato True 343
CONT E1_instante_E_texto ABERTA viva False 3059
CONT E1_instante_E_texto ABERTA viva True 952
CONT E1_instante_E_texto ENTREGUE 4181
CONT E1_instante_E_texto ENTREGUE validada False materializada False viva False ato False 4
CONT E1_instante_E_texto ENTREGUE validada False materializada False viva True ato False 55
CONT E1_instante_E_texto ENTREGUE validada False materializada False viva True ato True 30
CONT E1_instante_E_texto ENTREGUE validada False materializada True viva False ato False 174
CONT E1_instante_E_texto ENTREGUE validada False materializada True viva False ato True 1
CONT E1_instante_E_texto ENTREGUE validada False materializada True viva True ato False 82
CONT E1_instante_E_texto ENTREGUE validada True materializada False viva False ato False 49
CONT E1_instante_E_texto ENTREGUE validada True materializada False viva False ato True 1
CONT E1_instante_E_texto ENTREGUE validada True materializada True viva False ato False 3760
CONT E1_instante_E_texto ENTREGUE validada True materializada True viva False ato True 25
CONT E1_instante_E_texto ENTREGUE viva False 4014
CONT E1_instante_E_texto ENTREGUE viva True 167
CONT E1_instante_E_texto ato_RESPOSTA False 7317
CONT E1_instante_E_texto ato_RESPOSTA True 875
CONT E1_instante_E_texto materializada False 1113
CONT E1_instante_E_texto materializada True 7079
CONT E1_instante_E_texto validada False 1446
CONT E1_instante_E_texto validada True 6746
CONT E1_instante_E_texto viva False 7073
CONT E1_instante_E_texto viva True 1119
CONT E1_instante_E_texto 8192
CONT E2_instante_sem_texto ABERTA 1502
CONT E2_instante_sem_texto ABERTA validada False materializada False viva True ato False 2
CONT E2_instante_sem_texto ABERTA validada False materializada True viva False ato False 212
CONT E2_instante_sem_texto ABERTA validada True materializada True viva False ato False 1288
CONT E2_instante_sem_texto ABERTA viva False 1500
CONT E2_instante_sem_texto ABERTA viva True 2
CONT E2_instante_sem_texto ENTREGUE 8124
CONT E2_instante_sem_texto ENTREGUE validada False materializada False viva True ato False 111
CONT E2_instante_sem_texto ENTREGUE validada False materializada True viva False ato False 562
CONT E2_instante_sem_texto ENTREGUE validada False materializada True viva True ato False 30
CONT E2_instante_sem_texto ENTREGUE validada True materializada True viva False ato False 7421
CONT E2_instante_sem_texto ENTREGUE viva False 7983
CONT E2_instante_sem_texto ENTREGUE viva True 141
CONT E2_instante_sem_texto ato_RESPOSTA False 9626
CONT E2_instante_sem_texto materializada False 113
CONT E2_instante_sem_texto materializada True 9513
CONT E2_instante_sem_texto validada False 917
CONT E2_instante_sem_texto validada True 8709
CONT E2_instante_sem_texto viva False 9483
CONT E2_instante_sem_texto viva True 143
CONT E2_instante_sem_texto 9626
CONT E4_nenhum ABERTA 19017
CONT E4_nenhum ABERTA validada False materializada False viva False ato False 115
CONT E4_nenhum ABERTA validada False materializada False viva False ato True 1
CONT E4_nenhum ABERTA validada False materializada False viva True ato False 7109
CONT E4_nenhum ABERTA validada False materializada False viva True ato True 19
CONT E4_nenhum ABERTA validada False materializada True viva False ato False 11727
CONT E4_nenhum ABERTA validada False materializada True viva False ato True 4
CONT E4_nenhum ABERTA validada False materializada True viva True ato False 42
CONT E4_nenhum ABERTA viva False 11847
CONT E4_nenhum ABERTA viva True 7170
CONT E4_nenhum ENTREGUE 13457
CONT E4_nenhum ENTREGUE validada False materializada False viva False ato False 1221
CONT E4_nenhum ENTREGUE validada False materializada False viva True ato False 2587
CONT E4_nenhum ENTREGUE validada False materializada True viva False ato False 9431
CONT E4_nenhum ENTREGUE validada False materializada True viva True ato False 218
CONT E4_nenhum ENTREGUE viva False 10652
CONT E4_nenhum ENTREGUE viva True 2805
CONT E4_nenhum ato_RESPOSTA False 32450
CONT E4_nenhum ato_RESPOSTA True 24
CONT E4_nenhum materializada False 11052
CONT E4_nenhum materializada True 21422
CONT E4_nenhum validada False 32474
CONT E4_nenhum viva False 22499
CONT E4_nenhum viva True 9975
CONT E4_nenhum 32474
PKS_VIVAS_ABERTA E2_instante_sem_texto 2 38251 38252
PKS_VIVAS_ABERTA E3_texto_sem_instante 0 
pergunta_viva chamada 14790 vezes; segundos 53.1
```

## 15/11 17:50 · sessao 9f06cb853912 · - · - · origem=Ronald (tmux)

Item (d), pronto para consolidar. Os literais abaixo são trechos exatos; não interpretei qual predicado deveria prevalecer.

| Tema | Literal | Fonte |
|---|---|---|
| Lei geral do escritor e trilha | “UM ESCRITOR POR ESTADO, NADA EM BRANCO: campo de estado tem 1 escritor canonico + tripwire; acao critica grava usuario, antes/depois e motivo” | `app/docs/LEIS.md:65` |
| Divisão documental das portas | “Q1 identidade/via admin = models.py (abrir, fechar_por_admin, reabrir_por_competencia; desde a O261-E-F2 a PORTA UNICA DO CARIMBO carimbar/carimbar_identidade/descarimbar” e “Q2 resposta = services/disputa_emissao.py (responder_pergunta_disputa_core e irmas; absorve api/foto_recebida.py:50)” | `app/docs/DOSSIES.md:46` |
| Resposta vazia com instante | “(b) respondida_em com resposta vazia” e “(2) escritor de respondida_em sem resposta (p#36272/36273)” | `app/docs/BACKLOG.md:1053` |
| Predicado sem texto duplicado | “o predicado 'respondida sem resposta' tem copia em `chamados/juizes.py:1020` (dentro do C6) e em `chamados/services/vigia_fio.py::respondida_sem_texto` (:680, o unico do vigia apos o F5) -- um so juiz” | `app/docs/BACKLOG.md:1043` |
| Texto sem instante | “(3) texto sem respondida_em: o juiz segura (models.py:1900-1901, juizes.py:2035) mas validar/recusar exigem carimbo (validacao.py:416-418), nota models.py:736-738 pede ato impossivel” | `app/docs/BACKLOG.md:1478` |
| Instante sem texto: tratamento do passivo | “142 passivas NAO reabrem no app, vao a LISTA DO ADMIN "respondida pelo sistema sem o colab" (L-099) sem zerar respondida_em; col331/#22394 fica; cura na porta segue” | `app/docs/BACKLOG.md:2030` |
| Instante sem texto: encaminhamento | “LU-06 = regra unica = particao_do_app; respondida sem texto e da E3-10-CURA-NO-ESCRITOR” | `app/docs/CORTES.md:438` |
| Face SQL e vazio com espaços | “face SQL de respondida_sem_texto (trim de texto_vazio) troca a forma inline de models.py q_vivas e os 9 sitios respondida_em__isnull; selo de equivalencia com mutante sem trim; catraca pela forma de USO; os 17 do passivo medidos na sombra antes (app do colab ou supra mudou = PARA)” | `app/docs/BACKLOG.md:2035` |
| Duas portas dos campos | “PERGUNTA DE DESENHO (2 portas declaradas Q1+Q2 gravam respondida_em)” e “duas portas Q1+Q2, selo AST por campo + mutante 8o sitio” | `app/docs/BACKLOG.md:1978` |
| Resposta automática sobre validada | “responder_automaticamente (chamados/services/disputa_emissao.py ~1640-1666) so recusa quando ja ha texto; nao olha validada_em; chamador api/foto_recebida.py:48. Sem cura ate o censo” | `app/docs/BACKLOG.md:743` |
| Preservar resposta na reabertura | “**Lei do fato unico: resposta e FATO, e reabrir nao apaga fato.**” e “Reabrir tem de criar versao nova preservando a anterior consultavel no proprio chamado, e "Cobrar resposta" so vale para pendentes.” | `app/docs/BACKLOG.md:1149` |
| Zeragem histórica documentada | “o corpo de `disputa_emissao.py:1285-1310` e UM update (`respondida_em=None, validada_em=None, validada_por=None, resposta_colab=''`) e NAO toca `materializada_em`, `recusa_motivo`, `via_resolucao`.” | `app/docs/BACKLOG.md:1149` |
| Regra de preservação e cobrança | “preservar as respostas e cobrar so a pergunta pendente, com trilha; depois do export” e “PROIBIDO apagar resposta de colaborador; PROIBIDO juiz novo.” | `app/docs/BACKLOG.md:2549` |
| Devolução e contagem de atos | “D04 = ATOS humanos ocorridos (2 validacoes com devolucao no meio = 2); D05 = contrato publico CONSERVA as fases (app compilado nao muda), exigibilidade = campo NOVO aditivo (Expand-Contract, L-127); aguarda_admin com validacao sem efeito segue aguarda_admin ate o efeito” | `app/docs/BACKLOG.md:2032` |
| Devolver ao admin com gravação divergente | “PROIBIDO devolver ao admin resposta cuja validacao grava hora/tipo diferente do declarado” | `app/docs/BACKLOG.md:2027` |
| Instante não representa anuência | “sem o texto do colaborador nao se registra o horario dele, e `respondida_em` nao e anuencia” | `app/docs/BACKLOG.md:1667` |
| Passivo sem texto: testemunha e escritor | “158 passivos sem texto em que a testemunha diz 'espera' e a escrita planta a hora do marco (testemunha x escritor discordam).” | `app/docs/BACKLOG.md:1939` |
| Resposta sem veredito protege o pai | “`ponto/supra_juiz.py::pode_executar_csf` passa a PERGUNTAR a ele em vez de ler `respondida_em` cru: pai/destino seguro so com resposta SEM veredito; validada/materializada fecha normal pela Defesa 1.” | `app/docs/CORTES.md:415` |
| Admin não dá primeira resposta | “LU-18 = nao (admin nao da a primeira resposta pelo colab)” | `app/docs/CORTES.md:450` |
| Encerramento por desligamento, corte anterior | “a porta desligamento/admin_encerra deixa de carimbar via admin_decidiu em pergunta JA respondida (o carimbo diria uma decisao que nao houve -- L-007)” | `app/docs/CORTES.md:393` |
| Encerramento por desligamento, fila posterior | “desligamento.py::executar_desligamento fecha tambem a pergunta respondida sem veredito com via propria de desligamento (molde VIA_COMPETENCIA_TRANCADA), lista dia/pergunta/resposta no chamado desligamento_revisao” | `app/docs/BACKLOG.md:732` |

Não localizei texto específico sobre os quatro estados pedidos em `app/docs/LAPIDES.md`; ausência exaustiva de regra nesse documento: **[nao verificado]**. Em `LEIS.md`, o literal diretamente pertinente que encontrei foi a regra geral L-007 acima, sem equação específica entre instante e texto: **[nao verificado]** quanto à inexistência de outra formulação.

Comandos usados, todos somente leitura:

```bash
rg -n -i \
'respondida_em|resposta_colab|resposta.{0,35}vazi|texto.{0,30}instante|instante.{0,30}texto|devol.{0,50}resposta|resposta.{0,50}devol|descarimb.{0,50}resposta|JUIZ-RESPONDIDA|E4B-C6|E4B-C11|E4C-F1V6' \
app/docs/LEIS.md app/docs/CORTES.md app/docs/LAPIDES.md \
app/docs/BACKLOG.md app/docs/DOSSIES.md

rg -n -i \
'(devolv|devolu|descarimb|reabri|recus).{0,100}(respost|respondid)|(respost|respondid).{0,100}(devolv|devolu|descarimb|reabri|recus)|texto_vazio|sem.texto|vazi.{0,40}respost|respost.{0,40}vazi' \
app/docs/LEIS.md app/docs/CORTES.md app/docs/LAPIDES.md \
app/docs/BACKLOG.md app/docs/DOSSIES.md
```

Também executei consultas mais amplas por `respost|respondid|devol|descarimb|colab` e li faixas numeradas com `nl -ba ... | sed -n ...`; alguns resultados amplos foram truncados na saída da ferramenta, motivo para não certificar ausência. [nao verificado]

## 15/11 17:35 · sessao ef5c0b40f83d · E3-RESTO-R3 · verificador · origem=Code

**REPROVA.** As curas pontuais existem, mas permanece um caminho que publica zero apagando um positivo conhecido, além de relógios internos e contratos documentais contraditórios (`app/folha/vigia_retido_calado.py:44`; `app/folha/fora_da_folha.py:117`; `app/chamados/services/tarefa_do_dia.py:32`).

Identidade Git do hash/merge-base, incidência em produção e smoke nas duas cascas: **[nao verificado]**.

**Achados**

1. **Média — o vigia ainda pode apagar positivo conhecido, antes da soma corrigida.** O lavrador fixa `_agora`, calcula anterior e corrente nesse instante, mas grava outro `timezone.now()` para cada retrato (`app/folha/management/commands/lavrar_retrato_folha.py:49`; `app/folha/management/commands/lavrar_retrato_folha.py:69`; `app/folha/management/commands/lavrar_retrato_folha.py:80`). O vigia aceita somente retratos do mesmo **dia civil local** do mais recente da empresa (`app/folha/vigia_retido_calado.py:44`).

   Caso concreto: mesma empresa, competência anterior gravada às 23:59:59 com um dia sem leitura; corrente às 00:00:01 com zero. A anterior desaparece do universo; a soma corrigida recebe apenas a corrente e publica `dias_sem_leitura_sistema=0`, `cego=0` (`app/folha/vigia_retido_calado.py:47`; `app/folha/vigia_retido_calado.py:77`; `app/folha/vigia_retido_calado.py:78`). Os testes entre retratos usam o mesmo `AGORA` e não exercitam essa fronteira (`app/chamados/tests/test_e3_r3_c14_sem_banco.py:325`). Incidência em produção: **[nao verificado]**.

2. **Média — a passagem do relógio para a borda está incompleta nos caminhos de produção.**

   - A view captura `_hoje`, mas chama `frescor_dos_retratos()` sem fornecer o instante; o leitor consulta outro relógio e escolhe novamente as competências (`app/folha/views_retrato.py:176`; `app/folha/views_retrato.py:208`; `app/folha/retrato_folha.py:1386`). Caso: request iniciado no dia 20, consultando a competência anterior; o frescor executa após a virada para 21 e deixa essa competência fora do universo consultado, podendo suprimir seu aviso (`app/folha/retrato_folha.py:1390`; `app/folha/views_retrato.py:209`).
   - `fora_da_folha(..., agora=...)` carrega a tarefa nesse instante, mas a lista de dias passa por `_dias_que_retem` sem `hoje`; `_dias_sem_decisao` lê `localdate()` internamente (`app/folha/fora_da_folha.py:188`; `app/folha/fora_da_folha.py:193`; `app/folha/fora_da_folha.py:117`; `app/folha/export.py:1238`). Caso: ato iniciado antes da meia-noite, leitura depois dela; um dia sem célula passa a reter por `d < hoje`, embora o juiz da tarefa continue no instante anterior (`app/folha/export.py:1271`).

   O selo c14 não cobre esses relógios: sua lista de alvos não inclui `frescor_dos_retratos`, `_dias_que_retem` ou `_dias_sem_decisao` (`app/chamados/tests/test_e3_r3_c14_sem_banco.py:217`).

3. **Baixa — F6 permanece incompleto; a própria c14 deixou afirmações falsas.**

   - “fila residual nao cria acao” contradiz a publicação de `acao='decidir'` sem ação de instância (`app/chamados/services/tarefa_do_dia.py:32`; `app/chamados/services/tarefa_do_dia.py:705`; `app/chamados/tests/test_e3_r3_c14_sem_banco.py:103`).
   - `_fio_do_dia` documenta três retornos, mas também pode devolver `chamado` e `nao_sei` (`app/folha/fora_da_folha.py:77`; `app/folha/fora_da_folha.py:90`).
   - `dono_da_linha` promete comparação “POR INSTANTE”, enquanto compara dias ISO e a c14 recusa `datetime` (`app/folha/retrato_folha.py:474`; `app/folha/retrato_folha.py:490`; `app/folha/retrato_folha.py:462`).
   - O contrato do modelo ainda enumera quatro fios/grupos e diz que `ninguem` aparece como Admin, contrariando o balde atual (`app/folha/models.py:306`; `app/folha/models.py:319`; `app/folha/retrato_folha.py:483`).

4. **Baixa — texto técnico exposto na tela, preexistente no leitor auditado.** Com `tem_retrato=False`, o admin recebe `manage.py lavrar_retrato_folha --apply`, sem uma ação de produto correspondente (`app/templates/folha/_retrato_lista.html:17`; `app/templates/folha/_retrato_lista.html:20`).

**A. F1–F6 e mordida**

| Item | Resultado |
|---|---|
| **F1** | **Curado na fonte.** A exceção registra os itens em `sem_leitura`; a sincronização retém suas pautas. O teste atravessa esse caminho e o mutante remove justamente o registro (`app/ponto/management/commands/pautas_discordantes.py:113`; `app/ponto/services/pautas_dos_discordantes.py:198`; `app/chamados/tests/test_e3_r3_c14_sem_banco.py:194`; `app/chamados/tests/e3_r3_c14_mutantes.py:41`). |
| **F2** | **Curado para a mistura descrita.** Soma conhecida e ausência de campo ficam separadas; os mutantes retiram essa preservação. Não resolve a exclusão prévia do achado 1 (`app/folha/vigia_retido_calado.py:61`; `app/folha/vigia_retido_calado.py:77`; `app/chamados/tests/test_e3_r3_c14_sem_banco.py:306`; `app/chamados/tests/e3_r3_c14_mutantes.py:53`). |
| **F3** | **Curado no chamado original, incompleto no entorno.** O drawer fornece `hoje.date()` e os leitores alterados propagam o dia. Há testes e mutantes para retirar essa passagem, mas restam os caminhos do achado 2 (`app/colaboradores/views.py:986`; `app/folha/views_retrato.py:228`; `app/chamados/tests/test_e3_r3_c14_sem_banco.py:292`; `app/chamados/tests/e3_r3_c14_mutantes.py:42`). |
| **F4** | **Curado estruturalmente na fonte.** Raiz própria e seis alvos relativos substituem o elemento inteiro. Os mutantes retiram raiz/alvo; o teste não executa navegador (`app/templates/colaboradores/calendario_partial.html:39`; `app/templates/colaboradores/calendario_partial.html:43`; `app/chamados/tests/test_e3_r3_c14_sem_banco.py:76`; `app/chamados/tests/e3_r3_c14_mutantes.py:33`). Smoke: **[nao verificado]**. |
| **F5** | **Cura funcional real.** A tarefa publica `decidir` e o leitor mostra “Decidir no fio”; os mutantes retiram publicação e tradução. A fundamentação pela LU-01 tem a ressalva de B (`app/chamados/services/tarefa_do_dia.py:705`; `app/folha/retrato_folha.py:1017`; `app/chamados/tests/e3_r3_c14_mutantes.py:36`). |
| **F6** | **Trechos apontados corrigidos, cura documental incompleta.** Numerais, promessa de marcação e comentários de cron têm mutantes; sobrevivem as contradições do achado 3 (`app/folha/retrato_folha.py:164`; `app/folha/fora_da_folha.py:98`; `app/config/crons.py:2261`; `app/chamados/tests/e3_r3_c14_mutantes.py:58`). |

Reexecutei os **18 testes c14** e os **19 mutantes**: controles verdes, mutantes mortos por asserção; as definições e o executor estão em `app/chamados/tests/test_e3_r3_c14_sem_banco.py:68` e `app/chamados/tests/e3_r3_c14_mutantes.py:74`. A suíte de 14.584 testes relatada na entrega: **[nao verificado]**.

**B. `acao='decidir'` versus `None`**

A **LU-01 não determina o valor de `acao`**: determina “residual decidir: dono admin; chamado e rota”. Portanto, ela não obriga `None`, tampouco basta para afirmar que exige `'decidir'` (`app/docs/CORTES.md:433`).

A mudança não é apenas rótulo: `'decidir'` passa a entrar na coleção de ações e no JSON persistido; `None` era descartado dessa coleção (`app/folha/fora_da_folha.py:211`; `app/folha/retrato_folha.py:1248`).

| Escritor/leitor | Efeito |
|---|---|
| `_portas_da_tarefa` → `porta_da_tarefa` | Publica e copia `acao`; somente `resolver_dia` acrescenta o dia do turno (`app/chamados/services/tarefa_do_dia.py:706`; `app/chamados/services/tarefa_do_dia.py:720`). |
| Folha por pessoa/dia | `decidir` entra em `acoes`; `None` não gera ação para os links (`app/folha/fora_da_folha.py:211`; `app/folha/fora_da_folha.py:271`). |
| Lavrador | Copia as ações para `RetratoFolha.linhas` e persiste pela porta do command (`app/folha/retrato_folha.py:1248`; `app/folha/management/commands/lavrar_retrato_folha.py:78`). |
| UI | `decidir` → “Decidir no fio”; ausência de ação com fio admin → “Validar no fio”. Ambos levam a `modal_fio` (`app/folha/retrato_folha.py:1017`; `app/folha/retrato_folha.py:1065`; `app/folha/retrato_folha.py:989`). |
| Drawer | Desenha os links prontos (`app/colaboradores/views.py:987`; `app/templates/folha/_retrato_o_que_fazer.html:72`). |
| Pauta, vigia, furo e beco | Leem fio, rota, desconhecimento ou presença do envelope; a troca de `acao` não altera esses critérios (`app/ponto/services/pautas_dos_discordantes.py:87`; `app/chamados/services/vigia_fio.py:599`; `app/chamados/services/furo_sem_canal.py:98`; `app/escala/services/cadastro_realidade.py:472`). |
| API localizada | Lê o **verbo do chamado**, não `acao` da tarefa (`app/api/views_mensageria.py:395`; `app/api/views_mensageria.py:1152`). APK nativo: **[nao verificado]**. |

Os pins atuais exigem explicitamente o novo valor; isso verifica a implementação escolhida, não demonstra uma determinação da LU-01 (`app/chamados/tests/test_e3_portas_tarefa.py:256`; `app/chamados/tests/test_e3_r3_c12_sem_banco.py:17`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:245`). O comparativo com C17 **não certifica essa equivalência**, porque sua projeção omite `acao` (`app/chamados/management/commands/diff_e3_sombra.py:57`).

**C. Checklist**

1. **Escritores/leitores e significado.** `verbos` nasce da partição do painel; fila e pertinência vêm da alimentação; fio/rota/ação publicados nascem na tarefa (`app/chamados/services/verbos.py:30`; `app/chamados/juizes.py:1573`; `app/chamados/services/tarefa_do_dia.py:706`). `dias_sem_leitura`, `dias_sem_porta` e `datas` são produzidos por dia na Folha, copiados para o retrato e consumidos pelo vigia/tela (`app/folha/fora_da_folha.py:207`; `app/folha/fora_da_folha.py:234`; `app/folha/retrato_folha.py:1243`; `app/folha/vigia_retido_calado.py:61`). As divergências temporais estão nos achados 1–2; o recorte de datas usa limites inclusivos (`app/folha/retrato_folha.py:1342`).

2. **Segundo juiz:** nenhum novo confirmado no delta. A publicação consulta a escada registrada; pauta e vigia leem a porta. A seleção por dia civil de lavra permanece uma regra própria do vigia, com o defeito do achado 1 (`app/chamados/services/tarefa_do_dia.py:703`; `app/ponto/services/pautas_dos_discordantes.py:87`; `app/chamados/services/vigia_fio.py:601`; `app/folha/vigia_retido_calado.py:45`).

3. **Segundo escritor/porta:** nenhum novo confirmado nos caminhos alterados. Pauta continua passando por `escrever`/`marcar_feita_pelo_sistema`, sob `if apply`; falha do espelho só acrescenta testemunho à lista. **Dry por padrão preservado** (`app/ponto/services/pautas_dos_discordantes.py:201`; `app/ponto/services/pautas_dos_discordantes.py:205`; `app/ponto/management/commands/pautas_discordantes.py:54`; `app/ponto/management/commands/pautas_discordantes.py:114`).

4. **Relógio fora da borda:** achados 1–2. Há também `contexto_calendario` chamado sem `agora` e outro `localdate()` na view; efeito dessa segunda leitura no template: **[nao verificado]** (`app/colaboradores/views.py:1949`; `app/colaboradores/services/calendario.py:192`; `app/colaboradores/views.py:1963`).

5. **Crons:** no delta fornecido mudaram comentários. Os papéis continuam coerentes: vigia lê/alarma; lavrador grava métrica somente com `--apply` (`app/config/crons.py:2261`; `app/config/crons.py:2289`; `app/folha/management/commands/vigiar_retido_calado.py:33`; `app/chamados/management/commands/lavrar_furo_sem_canal.py:42`).

6. **Selo/RED:** há mordida das seis curas pontuais. Limitações: C1 é estrutural; C4 não cobre todos os relógios; D2 ignora ação; os pins de `decidir` fixam a escolha sem provar sua autorização normativa (`app/chamados/tests/test_e3_r3_c14_sem_banco.py:60`; `app/chamados/tests/test_e3_r3_c14_sem_banco.py:217`; `app/chamados/management/commands/diff_e3_sombra.py:57`; `app/chamados/tests/test_e3_r3_c14_sem_banco.py:111`).

7. **Templates:** nenhum cálculo paralelo de números encontrado nos examinados: barra lê `it.numero`, lista lê `l.dias`; a contagem vem de `len(recortar(...))` (`app/templates/core/_barra_gestao.html:67`; `app/templates/folha/_retrato_lista.html:66`; `app/folha/retrato_folha.py:839`). Ambas as bases carregam HTMX e incluem o drawer; quebra adicional não confirmada, smoke **[nao verificado]** (`app/templates/base.html:19`; `app/templates/base.html:399`; `app/templates/base_app.html:19`; `app/templates/base_app.html:154`). Texto técnico: achado 4.

8. **Docstrings/comentários:** achado 3; F6 não está integralmente saneado (`app/chamados/services/tarefa_do_dia.py:32`; `app/folha/fora_da_folha.py:77`; `app/folha/models.py:319`).

9. **ESMERIL-SEMANTICA:** `lavrado_em` funciona como instante de persistência no escritor e como identificador de rodada por dia civil no vigia — incompatibilidade concreta do achado 1 (`app/folha/management/commands/lavrar_retrato_folha.py:80`; `app/folha/vigia_retido_calado.py:45`). `acao` agora reúne ação exigível de instância e verbo residual do painel, sem atualizar o contrato que negava essa segunda origem (`app/chamados/services/tarefa_do_dia.py:699`; `app/chamados/services/tarefa_do_dia.py:705`; `app/chamados/services/tarefa_do_dia.py:32`).

**VEREDITO: REPROVA.**

## 15/11 17:33 · sessao 1a8cc5f5e33a · - · - · origem=Ronald (tmux)

Achados deste recorte:

- **Baixa — F6 ainda incompleto:** `_fio_do_dia` documenta somente `'admin' | 'colab' | 'nada'`, mas delega a `_fio_da_porta`, que também devolve `'nao_sei'` e o `'chamado'` publicado. Caso: envelope sem rota com `nao_sei=True` devolve `'nao_sei'`; porta de chamado devolve `'chamado'`. `app/folha/fora_da_folha.py:77`, `:82`, `:88`, `:90`; universo publicado em `:38`.
- **Baixa — comentário contradiz o contrato temporal novo:** `dono_da_linha` afirma “POR INSTANTE … nunca por data solta”, mas compara datas ISO contra `_hoje_iso(hoje)`; c14 passa a recusar expressamente `datetime`. `app/folha/retrato_folha.py:474`, `:490`, `:462`. Caso: passar instante falha; passar dia é justamente o caminho aceito. O teste confirma a recusa do instante: `app/chamados/tests/test_e3_r3_c14_sem_banco.py:290`.
- **Baixa — comentário antigo no template tocado:** afirma que falta o núcleo e que o botão nasce desligado, embora o núcleo já implemente o modo e sempre entregue `competencia_rotulo`. `app/templates/colaboradores/calendario_partial.html:7`, `:31`; implementação em `app/colaboradores/services/calendario.py:205`, `:206`, `:841`.
- **Baixa — texto técnico no produto, preexistente:** sem retrato, a tela do admin publica o comando `manage.py lavrar_retrato_folha --apply`. Caso concreto: `tem_retrato=False`. `app/templates/folha/_retrato_lista.html:17`, `:19`, `:20`. Drawer também publica “Datas (ISO)” e explicação do lavrador: `app/templates/folha/_retrato_o_que_fazer.html:43`.

F4: cura real na fonte. Raiz própria em `app/templates/colaboradores/calendario_partial.html:39`; seis links usam `closest [data-calendario-raiz]` e `outerHTML` em `:43`, `:47`, `:51`, `:54`, `:57`, `:60`. A resposta é esse fragmento em `app/colaboradores/views.py:1965`; o drawer processa HTMX após inserir o HTML em `app/templates/core/_drawer_generico.html:33`. As duas bases carregam HTMX e incluem o drawer: `app/templates/base.html:19`, `:399`; `app/templates/base_app.html:19`, `:154`. Quebra adicional das cascas: nenhum achado; smoke de clique [nao verificado].

Executei C1+C7: seis testes verdes; os sete mutantes correspondentes morreram por asserção. Referências dos testes: `app/chamados/tests/test_e3_r3_c14_sem_banco.py:76`, `:93`, `:406`, `:424`, `:438`; mutantes: `app/chamados/tests/e3_r3_c14_mutantes.py:33`, `:35`, `:58`, `:60`, `:62`, `:63`. O teste de navegação é estrutural, sem navegador: `app/chamados/tests/test_e3_r3_c14_sem_banco.py:60`, `:76`.

F6 original: numerais, promessa de marcação e comentários de cron foram corrigidos na fonte, com mordida dos mutantes acima. `app/folha/retrato_folha.py:164`, `:525`; `app/folha/fora_da_folha.py:98`; `app/config/crons.py:2261`, `:2289`.

C5/crons: só comentários mudaram no delta fornecido [nao verificado no histórico Git]. Os papéis continuam coerentes: vigia sem `--apply`, leitura e alarme em `app/folha/management/commands/vigiar_retido_calado.py:25`, `:33`, `:38`; lavra grava `MetricaSnapshot` somente com `--apply` em `app/chamados/management/commands/lavrar_furo_sem_canal.py:42`, `:50`; a medida lê a porta publicada em `app/chamados/services/furo_sem_canal.py:98`, `:99`.

Barra/retrato: nenhum número derivado no HTML examinado. A barra mostra `it.numero` em `app/templates/core/_barra_gestao.html:67`, calculado por `len(recortar(...))` na autoridade em `app/folha/retrato_folha.py:839`; a lista mostra `l.dias` em `app/templates/folha/_retrato_lista.html:66`.

Relógio remanescente: `calendario_ponto` chama `contexto_calendario` sem `agora` em `app/colaboradores/views.py:1949`; este lê `timezone.now()` em `app/colaboradores/services/calendario.py:192`. A view ainda lê `_tz.localdate()` em `app/colaboradores/views.py:1963`, mas o `ctx.hoje` sobrescrito não é consumido nos templates examinados; efeito concreto dessa segunda leitura [nao verificado].

_5723 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
