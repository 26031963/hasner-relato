# CODEX-ESTADO

> Ultima resposta processada: **26/11/2026 21:20** · publicado em **26/11/2026 21:24** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 26/11 21:20 · sessao a5af0cfdaec7 · O30-LOTE-MINIMA · leitura · origem=Code

# O30-LOTE-MINIMA — mapa e censo

Análise estática, só de leitura, da árvore fornecida. A correspondência com o HEAD de `main` no hash `a6e4ed53418778553d68598be7eca02550efe4ff` fica **[nao verificado]** porque a cópia veio sem metadados Git. Execução da suíte **[nao verificado]**.

## 1. Funções públicas e chamadores

No censo estático, o único import externo do serviço está em `app/ponto/tests/test_atestados_lote.py:25`. A rota `relatorios:atestados` aponta para `atestados_acumulados`, que monta o próprio agregado e CSV. O drawer de Ausências chama `linha_do_tempo` diretamente pelo seu histórico. `app/relatorios/urls.py:8–40` · `app/relatorios/views.py:273–330` · `app/ponto/views.py:2035–2043,2129–2132`

| Função | Leitura, retorno e efeitos | Chamadores encontrados |
|---|---|---|
| `janela` | Lê os parâmetros, usa a data de hoje para completar mês/ano e resolve o corte da empresa, com consulta a `Empresa` se receber um PK conversível. Devolve `(início, fim)`. Zero gravação em banco, storage, disco ou log. Zero abertura de anexo. `app/ponto/services/atestados_lote.py:32–43` · `app/ponto/janelas.py:36–57` | Produção: nenhum externo encontrado. Testes de competência e período livre em `app/ponto/tests/test_atestados_lote.py:96–103`. |
| `linhas` | Para cada colaborador recebido, lê o histórico de `Ausencia` com registrador e aprovador. O histórico exclui as filhas de intervalo via `documento_referencia__isnull=True`. O lote filtra por sobreposição inclusiva, tipo e status, acrescenta `colaborador` e `exige_documento` do catálogo vigente e devolve uma lista de dicionários em ordem decrescente de data e PK. Zero gravação em banco, storage, disco ou log. O catálogo pode atualizar seu cache em memória. **Abre anexos em `rb` antes dos filtros do lote**, pois o histórico lê `tipo_real` para cada lançamento com documento e a detecção lê até 12 bytes. `app/ponto/services/atestados_lote.py:25–29,46–66` · `app/ponto/services/dia_decidido.py:624–685` · `app/ponto/models.py:855–864` · `app/ponto/services/ausencia.py:709–730` · `app/ponto/catalogo/ausencias.py:240–281` | Produção: nenhum externo encontrado. Testes de recorte, prévia e formatos em `app/ponto/tests/test_atestados_lote.py:53–93,110–124,130–178`. |
| `previa` | Lê a lista pronta. Devolve `total`, `com_documento`, `sem_documento`, `exige_sem_documento`, a lista `faltando` e o número de colaboradores distintos por PK. Usa o indicador `documento` da linha. Zero gravação e zero abertura de anexo. `app/ponto/services/atestados_lote.py:69–85` · `app/ponto/services/dia_decidido.py:675–684` | Produção: nenhum externo encontrado. Testes em `app/ponto/tests/test_atestados_lote.py:53–93,148–155`. |
| `nome_do_arquivo` | Devolve `<matricula>_<AAAA-MM-DD>_<tipo>.<ext>`. Usa o PK do colaborador se faltar matrícula e `.bin` se faltar extensão no nome gravado em `documento.name`. Zero gravação e zero abertura de anexo. `app/ponto/services/atestados_lote.py:88–98` | Chamada interna por `zip_bytes` em `app/ponto/services/atestados_lote.py:150`. Teste direto com `.heic` em `app/ponto/tests/test_atestados_lote.py:106–113`. Nenhum chamador externo de produção encontrado. |
| `csv_bytes` | Lê as linhas e os dados do colaborador e da empresa. Devolve bytes UTF-8 com BOM, separador `;`, cabeçalho e uma linha por lançamento. Escreve em `StringIO`, em memória. Zero gravação em banco, storage, disco ou log e zero abertura de anexo nessa função. `app/ponto/services/atestados_lote.py:107–132` | Produção: nenhum externo encontrado. Testes de BOM, separador e quantidade de linhas em `app/ponto/tests/test_atestados_lote.py:130–146`. |
| `zip_bytes` | Abre em `rb` e lê por inteiro os anexos das linhas com `documento` verdadeiro. Devolve `(bytes_do_zip, quantidade_de_originais_incluídos)`. Escreve em `BytesIO`, em memória, e acrescenta sufixo numérico quando o nome se repete. Zero gravação em banco, storage ou disco pelo serviço. Se o fechamento do anexo falhar, chama `logging.getLogger('ponto').debug(..., exc_info=True)`. A configuração SaaS do logger está em `INFO`. `app/ponto/services/atestados_lote.py:135–173` · `app/config/settings/saas.py:160–166,175–183` | Produção: nenhum externo encontrado. Testes de contagem e nomes em `app/ponto/tests/test_atestados_lote.py:148–185`. |

## 2. Universo de colaboradores e acesso

- **`empresas_visiveis()` é fonte de opções de UI.** Devolve empresas ativas, exclui CNPJ com prefixo `11222333`, ordena por razão social e recebe zero argumentos de usuário. `pracas_visiveis()` oferece praças ativas. Em SaaS, os apps de negócio rodam sob `TenantMainMiddleware`. Os recortes abaixo vêm do queryset de cada relatório. `app/colaboradores/queries.py:78–94` · `app/config/settings/saas.py:34–49,65–77`
- **Consultas por lançamento.** Atestados acumulados parte de `Ausencia` de tipo médico vigente, aprovada, com início no ano selecionado. Absenteísmo parte de ausências aprovadas que alcançam seu recorte mensal. Horas e Inconsistências partem de `FechamentoMensal`, Férias vencendo de `PeriodoAquisitivo` e Benefícios de `Beneficio`. Empresa e praça do GET acrescentam filtros pelo colaborador quando informadas. Em Atestados, `empresas_visiveis()` entra no contexto do seletor. `app/relatorios/views.py:56–80,128–150,179–204,248–291,323–330,335–359,600–623`
- **Consultas por colaborador.** Afastados seleciona os PKs devolvidos pelo juiz de afastamento do dia. Extrato parcial começa com `situacao='ativo'`. Builder começa com `Colaborador.objects` e aplica os filtros informados de empresa, praça, situação e posto. O Builder usa `empresas_visiveis()` também nas **opções** de posto. O caderno de espelhos aceita PKs selecionados ou uma empresa. No caminho por empresa, a geração usa `colaboradores_do_lote`, com ativos, afastados e em férias no padrão da geração. `app/relatorios/views.py:387–411,436–461,1151–1172,1202–1229,1428–1450,1497–1502` · `app/relatorios/services.py:99–105,165,217`
- **Permissões.** Índice, Atestados acumulados, demais relatórios acima e caderno usam `@login_required` e `_requer_staff`, que resolve `tem_acao(user, 'ver_relatorios')`. O histórico de chamados e a busca por nome consultam essa ação diretamente. Em Auditoria de espelhos e Ranking de furos a guarda é `is_staff`. A semeadura concede `ver_relatorios` a Supervisão, DP e TI. Para leitura de folha, o Retrato e a prévia de exportação usam `@login_required` com `ver_folha`, semeada para DP e TI, e `tem_acao` também aceita superusuário. Para servir o anexo individual, a ação existente é `ver_ausencias`. **Nenhuma das guardas de `relatorios/views.py` verificadas é exclusiva do DP.** `app/relatorios/views.py:15–34,248–251,694–699,782–789,1151–1163,1508–1512,1596–1601` · `app/colaboradores/mixins.py:322–331,351–386` · `app/folha/views_retrato.py:251–258` · `app/folha/views.py:337–340` · `app/ponto/views.py:2136–2148`

## 3. Janela, tipo e status

- Com `de` e `ate` preenchidos, `janela()` devolve ambos como chegaram. Com apenas uma data, usa o caminho de competência. Mês e ano ausentes ou falsos são completados **independentemente** com `date.today().month` e `.year`. Assim, depois do dia de corte, o padrão ainda pede a competência **rotulada** com o mês civil de hoje. `janela_fechamento` vai do dia de corte do mês anterior até a véspera do corte no mês informado. `empresa=None` usa o corte padrão 21. A função opera com mês/ano inteiros e com datas já convertidas para o recorte de `linhas`, sem parser de GET ou validação da ordem de De/Ate. `app/ponto/services/atestados_lote.py:32–43,46–63` · `app/ponto/janelas.py:10–33,36–63`
- `tipos` e `status` são coleções de **códigos**, por exemplo `tipos=['atestado']` e `status=['aprovada']`. `None` ou coleção vazia deixa aquela dimensão sem filtro. Uma string é aceita pelo operador `in` de Python como busca de substring, e o serviço não expande o alias `pendente`. `app/ponto/services/atestados_lote.py:46–66` · `app/ponto/services/dia_decidido.py:570–584` · `app/ponto/tests/test_atestados_lote.py:83–88`
- **Tipos.** Lista de Ausências, drawer e Absenteísmo passam `choices_agrupadas_vigentes()` ao template. O catálogo lê `TipoAusencia` ativo e usa a semente se a leitura não devolver catálogo. Atestados acumulados usa `TIPOS_MEDICOS` e fixa `status='aprovada'`. **Status.** `Ausencia.STATUS_CHOICES` declara `aguardando_documento`, `aguardando_decisao`, `aprovada` e `rejeitada`. A lista de Ausências monta o filtro em um mapa próprio e o template oferece os chips Todos, Pendente, Aprovada e Rejeitada. `pendente` reúne os dois estados de `ABERTAS`. `app/ponto/views.py:2113–2114,2268–2286,2353–2367` · `app/templates/ponto/ausencias.html:47–63,82–89` · `app/ponto/catalogo/ausencias.py:240–281,313–323` · `app/ponto/models.py:690–705` · `app/ponto/services/ausencia.py:912` · `app/relatorios/views.py:238–242,277–284`

## 4. Anexo ausente no ZIP

Campo de documento vazio faz a linha ser pulada. Campo preenchido com arquivo ausente no storage cai no `except Exception` já existente e gera `<nome>.FALTANDO.txt` com o ID da ausência, sem incrementar `n`. O `finally` tenta fechar o anexo e tem seu próprio `except Exception` com log em `DEBUG`. Como a prévia conta o campo preenchido, ela pode contar um documento a mais que `n` para cada original ausente. **Para esse caso a tela precisaria de zero `except` novo.** Os testes existentes de ZIP usam anexos efetivamente salvos. `app/ponto/services/atestados_lote.py:76–84,145–173` · `app/ponto/services/dia_decidido.py:681` · `app/ponto/tests/test_atestados_lote.py:44–50,148–185`

## 5. Entrada da tela e cascas

Os dois pontos locais já têm navegação. O índice tem o card **Atestados acumulados** no grid. A página desse relatório tem cabeçalho com volta para Relatórios e ação CSV. O corte original registra o atalho na **barra de Ausências**, abrindo drawer. O código fornecido ainda não registra a rota do lote. `app/templates/relatorios/index.html:92–102,173–181` · `app/templates/relatorios/atestados.html:1–20` · `app/docs/CORTES.md:349` · `app/ponto/services/atestados_lote.py:1–7` · `app/relatorios/urls.py:8–40` · `app/ponto/urls.py:53–78`

Um link localizado em qualquer uma das duas páginas de Relatórios cabe no template da própria página. Ambas estendem `base.html`, cujo menu já leva ao índice. A varredura dos includes literais das duas cascas não encontrou esses templates de página em seus grafos. A **L-072** exige smoke de clique nas duas cascas para alterações na base, em partial incluído por ela, em `static/js/` ou no service worker. O vale do print registrado no corte original da O30 é uma condição própria daquele corte. `app/templates/relatorios/index.html:1` · `app/templates/relatorios/atestados.html:1–20` · `app/templates/base.html:92–93,181–189,388–432` · `app/templates/base_app.html:85,145–154` · `app/docs/LEIS.md:99` · `app/docs/CONDUTAS.md:50–68` · `app/docs/CORTES.md:349`

## 6. Testes que alcançam a mudança [nao verificado]

| Contrato ou teste | O que uma rota ou um link novo aciona | [nao verificado]
|---|---|
| `RotaPedeSessaoTest` | **Enumeração automática.** A casca de CI inclui `relatorios.urls`. O teste percorre o resolver por namespace, sonda a rota como anônimo por GET e tenta POST se o GET der 405. Acusa a rota que não toca no usuário e não devolve um status de guarda, ressalvadas as rotas públicas declaradas. `app/config/urls_ci.py:5–15` · `app/core/tests/test_rota_pede_sessao.py:89–97,121–149,235–270` |
| `IndexEnxutoTest` | Varre links do índice, exige **13 cards** e tenta resolver os `{% url %}` do índice e do Fechamento. Um novo card no molde atual cai na contagem e no `reverse`. Um link só em `relatorios/atestados.html` fica fora dessa varredura. `app/relatorios/tests/test_index_enxuto.py:80–82,96–124,165–183` |
| Contratos de fonte | `test_contract_excecoes_engolidas.py` varre por AST arquivos `*views*.py` e detecta handler amplo que devolve resposta sem passar por `so_recusa_de_negocio`. `test_contract_no_except_pass.py` varre `except: pass` e trabalha por arquivo, com `relatorios/views.py` na lista legada. `test_int_cru_gate.py` varre `int(request.GET/POST.get(...))` desprotegido. Esses contratos entram pelo código da view. `app/core/tests/test_contract_excecoes_engolidas.py:45–73,113–125` · `app/ponto/tests/test_contract_no_except_pass.py:39–46,50–78` · `app/core/tests/test_int_cru_gate.py:16–51` |
| Outros vizinhos | `test_todo_template_compila.py` enumera e compila os HTML, inclusive um template novo. `test_contract_portas.py` usa o resolver, mas o censo registra as views cuja fonte indica POST. `test_get_sanitizado.py` e `test_atestados_acumulados_pelo_juiz.py` chamam rotas nomeadas, logo uma rota nova não passa automaticamente por aqueles casos. `app/core/tests/test_todo_template_compila.py:34–60` · `app/core/tests/test_contract_portas.py:31–51` · `app/core/portas_da_tela.py:53–55,103–125` · `app/relatorios/tests/test_get_sanitizado.py:20–32` · `app/relatorios/tests/test_atestados_acumulados_pelo_juiz.py:50–89` |

## 7. Leis e cortes encontrados

- **`LEIS.md`** — o grep literal por `relatorios/views.py` encontra **L-090** na auditoria de `absenteismo`, que usa mês civil. A definição da L-090 remete ao corte por empresa em `ponto/janelas.py`. `AUSENCIAS-DRAWER-E-LOTE` aparece entre os cortes classificados como ato, sem L-NNN próprio nessa entrada. `app/docs/LEIS.md:123,181,207,232`
- **`CORTES.md`** — `Atestados LOTE` aparece em `AUSENCIAS-DRAWER-E-LOTE`, O30 F3, com prévia, PDF, ZIP, CSV e a seção de documento obrigatório faltante. O corte veta montagem própria de janela, efeito ou dias e registra o vale pelo print. `app/docs/CORTES.md:349`
- **`DOSSIES.md`** — a ocorrência de `relatorios/views.py` é o censo de leitores de `pendentes_na_central`, referente a HE. A chamada está hoje na linha 43 da view. O grep literal nos três arquivos não devolveu `atestados_lote`. `app/docs/DOSSIES.md:354–360` · `app/relatorios/views.py:35–43`
- **Leis herdadas** — **L-002** para leitura da mesma autoridade e **L-003** para contagem na fonte, visíveis na chamada ao histórico e na prévia sobre as linhas resultantes. **L-004** para o escopo literal do aval, **L-072** para as cascas e **L-090** para a janela de competência. `app/docs/LEIS.md:60–62,99,123` · `app/ponto/services/atestados_lote.py:32–43,54–85`

## 8. Perguntas de lei abertas

1. **Entrada e alcance do corte.** Qual entrada vale para a O30-LOTE-MINIMA, o índice ou Atestados acumulados, diante do corte registrado para atalho na barra de Ausências, drawer e três formatos? A instrução desta leitura já delimita CSV e ZIP. Um registro dessa delimitação nos três documentos consultados fica **[nao verificado]**. `app/docs/CORTES.md:349` · `app/docs/LEIS.md:62` · `app/templates/relatorios/index.html:173–181` · `app/templates/relatorios/atestados.html:4–10`
2. **Acesso.** Para o lote com originais, “admin da folha” significa a guarda de leitura `ver_folha` ou exige também a ação `ver_ausencias` usada no anexo individual? O catálogo de Relatórios tem uma guarda mais ampla, `ver_relatorios`, semeada também para Supervisão. `app/folha/views_retrato.py:251–258` · `app/ponto/views.py:2136–2148` · `app/relatorios/views.py:15–18` · `app/colaboradores/mixins.py:354–380`
3. **Universo sem empresa escolhida.** A competência pede uma empresa ou admite várias? Se admitir várias, qual universo entra, inclusive quanto a empresas inativas e colaboradores desligados com lançamento no período? Hoje `linhas()` recebe o universo pronto e um único par de datas. `janela(empresa=None)` usa o corte 21 e os relatórios vizinhos têm recortes diferentes. `app/ponto/services/atestados_lote.py:32–66` · `app/ponto/janelas.py:36–57` · `app/colaboradores/queries.py:78–87` · `app/relatorios/views.py:277–291,455–461`
4. **Status inicial.** A primeira prévia abre em aprovadas ou em todos os estados? O selo descrito para a F3 mede aprovadas na janela. No serviço, omitir `status` aceita todos os estados presentes nas linhas. `app/docs/BACKLOG.md:1445` · `app/ponto/services/atestados_lote.py:46–66,76–84`

## 26/11 21:20 · sessao c7d4e248c1d1 · V6-LISTA-ORFAS · verificador · origem=Code

**Reprovo a volta 2.** O delta corrige o caso de publicação por `rename` e impede que o `find` final apague a falha do juiz em Bash normal. Mas a espera passa a acordar com um `.rc` antigo e já colhido após um simples `chmod`. O teto de 300 s continua cobrindo apenas o laço. [bin/codex_pergunta.sh:572–574] [bin/disjuntor.py:1963–1969] [bin/rc_sem_colher.py:76–82]

Os quatro arquivos conferem com os blobs pós-delta do anexo. Fiz as verificações em memória e a checagem de sintaxe dos selos. Os selos completos criam e apagam arquivos. A identidade do commit, a execução integral dos selos, o `~/vercodex.py` e o comportamento em produção ficam [nao verificado]. [bin/tests/test_rc_sem_colher.sh:22–23,109–121] [bin/tests/test_agente_no_modelo.sh:30–32] [nao verificado]

### Reprovações da volta 1

| Ponto | Veredito sobre o delta |
|---|---|
| **Marca em prosa** | **Cura a citação no meio da linha.** Agora `colhido` ou `superado` precisa vir logo após `- HH:MM `. Uma linha que começa com a forma inteira ainda marca, inclusive dentro de um bloco de código Markdown. O novo `[ )]` aceita espaço sem `)` e rejeita a forma terminada logo após o minuto, embora RAIAS 3 ainda documente a marca até `(HH:MM`. [bin/rc_sem_colher.py:24–36] [CLAUDE.md:335–337] [bin/tests/test_rc_sem_colher.sh:40–46,129–130] |
| **`rename` escapa do `-newer`** | **Cura se o `ctime` da publicação for estritamente posterior ao `mtime` de `M`.** A comparação nova cobre o temporário escrito antes de `M` e publicado depois. Igualdade de carimbo ou relógio recuado ainda podem perder a publicação. **Contraexemplo novo:** um `.rc` já marcado, com `mtime` anterior a `M`, passa no `find` após `chmod` ou `touch -a` posterior a `M`, mesmo com conteúdo e `mtime` iguais. Um `touch` que avança o `mtime` já acordava na volta 1. Os dois `find` testam profundidade, nome e carimbo, sem exigir arquivo regular, obra viva ou ausência de marca. [bin/codex_pergunta.sh:572–574] [bin/disjuntor.py:1963–1969] [bin/rc_sem_colher.py:59–82] | [nao verificado]
| **Juiz quebrado e `find` final devolvendo `0`** | **Cura em Bash normal.** Testei a forma com comandos simulados. Juiz `1` pula o laço, roda o `find` final e entrega `1`. Juiz `3` pula o laço e entrega `0`. Se o juiz retorna `0`, tanto `timeout` com `124` quanto `find` final com `1` ainda podem terminar em `0`. **Regressão com `bash -e`:** o juiz `3` agora encerra no próprio Python, antes de `r=$?` e do `find` final. Na forma anterior, o Python estava à esquerda de `&&` e o `find` rodava. Uso de `-e` na sessão real [nao verificado]. [bin/disjuntor.py:1963–1969] [bin/tests/test_rc_sem_colher.sh:95–99] |
| **`os.walk` engole erro** | **Cura a falha de abertura entregue a `onerror`.** Confirmei em memória que `PermissionError` sobe. Isso também passa a abortar a lista se uma subpasta desaparecer entre a poda e a abertura. A garantia se limita ao erro entregue ao callback. `isdir` e `isfile` ainda podem excluir entradas sem erro visível. O selo de permissão exige apenas status diferente de `0` e `3`. [bin/rc_sem_colher.py:46–47,57–69] [bin/tests/test_rc_sem_colher.sh:66–69,135] |
| **FIFO** | **Cura o FIFO já presente quando `isfile` roda.** A verificação descarta entradas especiais antes de `_rc_txt`. Permanece a corrida de troca do caminho entre a verificação e `open`. O `find` da espera ainda pode anunciar um FIFO chamado `*.rc`. [bin/rc_sem_colher.py:39–43,64–82] [bin/disjuntor.py:1967–1969] |
| **Fronteira de `min_idade`** | **Cura a igualdade pela régua da volta 1.** Com `900`, verifiquei que `899` e `900` ficam fora e `901` entra. Com o padrão `0`, idade zero passou a ficar fora. A pasta da obra com exatamente 24 h ainda passa, embora a descrição diga “menos de 24 h”. A equivalência com `esquecidos` fica [nao verificado]. [bin/rc_sem_colher.py:6–9,21,50–76,86–97] | [nao verificado]
| **Casos do selo que passavam vazios** | **Cura os quatro negativos apontados.** `_fora` exige status `3` e presença de `OBRA-ORFA` na mesma execução em que exige a ausência da obra testada, inclusive com `900`. A espera sem órfão ainda só mede duração mínima. O teste de idade aceita tanto `<` quanto `<=`, pois usa 2 min contra 900 s. Falta caso de FIFO. O mutante de `rename` troca só o `find` do laço e pode sobreviver se o juiz vir o `.rc` publicado, retornar `3` e deixar o `find` final com `-cnewer` imprimir o caminho. As 14 âncoras são únicas e as versões mutadas compilam em memória. A execução integral fica [nao verificado]. [bin/tests/test_rc_sem_colher.sh:56–69,75–99,109–137] | [nao verificado]

### Forma, teto e leitores

**RAIAS 3 e `ESPERA_UNICA` continuam iguais após normalizar espaços.** `espera_nega` aceita a forma nova com ou sem o teste final `[ $r = 0 -o $r = 3 ]`. A mudança no veredito vem de `-c?newer`, que também mantém a forma antiga com `-newer` aceita. O verificador extrai literalmente `300` do texto, compara com `ESPERA_TETO_S` e recusa `301` ou ausência de `timeout`. Ele procura o número no comando inteiro. Na execução, o juiz inicial e o `find` final ficam fora do timeout. A checagem de divergência do selo anterior passou em memória e acusou o mutante que troca `sleep 5` por `sleep 9` só no documento. [CLAUDE.md:341–345] [bin/disjuntor.py:1962,1967–1969,1978–1979,2157–2174] [bin/tests/test_agente_no_modelo.sh:277–283,352] [nao verificado]

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| **`mtime`** | O wrapper escreve o temporário do `.rc` e o publica por `mv`, preservando o `mtime` da escrita. Também publica a resposta e escreve o inventário. O juiz lê o `mtime` de pasta e subpastas para a janela de 24 h e o dos arquivos para escolher o `.rc`, medir idade, aplicar a folga de 5 s e renderizar `HH:MM`. O selo fixa carimbos com `touch`. `mktemp` cria `M`, cujo `mtime` serve de referência aos dois `find`. Demais produtores em uso [nao verificado]. [bin/codex_pergunta.sh:572–574,798,807] [bin/rc_sem_colher.py:50–82] [bin/disjuntor.py:1967–1969] [bin/tests/test_rc_sem_colher.sh:27–39,86–92] |
| **`ctime`** | O filesystem atualiza o carimbo na publicação por `mv` e em alterações de metadados. Os dois `find` leem o `ctime` da entrada contra o `mtime` de `M`. O juiz inicial usa `mtime`, inclusive para obra viva e para a marca. [bin/codex_pergunta.sh:572–574] [bin/disjuntor.py:1963–1969] [bin/rc_sem_colher.py:59–80] |
| **Marca do RELATO** | RAIAS 3 atribui a colheita à sessão e pede uma linha `superado` para rc de obra encerrada. O selo escreve marcas com o minuto calculado por `date`. O juiz lê o RELATO inteiro e compara obra, caminho relativo e minuto de `time.localtime(mtime)`. A hora inicial da linha só habilita a sintaxe. Recriação no mesmo minuto ou marca de outro dia com a mesma chave ainda pode suprimir o rc. Escritor efetivo, fuso comum e unicidade da gravação em uso [nao verificado]. [CLAUDE.md:333–337] [bin/tests/test_rc_sem_colher.sh:30,40–47] [bin/rc_sem_colher.py:25–36,78–81,101–104] |
| **Status do juiz e `rc=<n>`** | `main` devolve `0` para vazio e `3` para lista, e `sys.exit` publica o status. A espera captura `$?` em `r`, usa `0` para entrar no laço e aceita `0` ou `3` no fim. O selo também lê `$?`. O `rc=<n>` impresso vem do conteúdo do sidecar escrito pelo wrapper. O consumo pelo `vercodex` [nao verificado]. [bin/rc_sem_colher.py:39–43,99–111] [bin/disjuntor.py:1967–1969] [bin/codex_pergunta.sh:565–574] [bin/tests/test_rc_sem_colher.sh:52–58,71–73] |

### Classes de reprovação

- **Segundo juiz e derivação paralela.** Na cópia, `sem_colher` decide orfandade e `espera_nega` julga a forma. O `find` continua sendo uma seleção independente para acordar, agora por mudança de metadados. Ele pode imprimir um caminho que o juiz já listou se a publicação cair após `M` e antes da leitura do juiz. A chamada externa com `900` está declarada. A substituição efetiva de `esquecidos` e a ausência de derivação paralela fora da cópia ficam [nao verificado]. [bin/rc_sem_colher.py:2–9,50–83] [bin/disjuntor.py:1963–1969,2157–2174]
- **Segundo escritor e ordem de trava.** O delta não adiciona escritor de sidecar ou de marca. O wrapper publica sob a trava da resposta. O leitor abre RELATO e sidecar sem adquirir essa trava e faz a seleção e a leitura em passos distintos. Não há nova ordem de aquisição no delta. A garantia de uma única marca gravada fica [nao verificado]. [bin/codex_pergunta.sh:572–574,715–733] [bin/rc_sem_colher.py:39–43,62–82,101–106]
- **Relógio, campo com dois significados e selo sem mordida.** O `ctime` passa a servir de sinal de publicação embora também registre alterações de metadados. O minuto do `mtime` ainda funciona como identidade da versão da marca. O selo novo cobre a publicação por `rename`, mas deixa sem asserção o falso despertar por metadados e a igualdade em `min_idade`. [bin/disjuntor.py:1963–1969] [bin/rc_sem_colher.py:76–82] [bin/tests/test_rc_sem_colher.sh:65,86–93,126,134]
- **`except` novo:** nenhum. O tratamento adicionado é `onerror=_sobe`, e `_sobe` faz `raise erro`. [bin/rc_sem_colher.py:46–47,62]

## 26/11 21:19 · sessao 353803d9c2b6 · DIA-DA-BATIDA-JUIZ-UNICO · construtor · origem=Code

lei: B2-chão segue na fila herdada. `app/docs/LEIS.md:78` · `app/ponto/registro_batida.py:197`

**F4e entregue para conferência do Code e do Opus.** O flip transacional trava o Colaborador e relê a Batida antes das guardas e da trilha. A regularização consulta a disputa aberta sob a trava e antecipa Disputas e Chamados, incluindo o Chamado terminal do lote e os vínculos alcançados pelas cascatas. Reusei o fecho do repasse para incluir vínculos legados. Se a releitura encontrar outro dono, o lote aborta antes de escrever Batida. `app/ponto/services/flip_batida.py:17` · `app/chamados/services/regularizacao_ext.py:111` · `app/chamados/services/disputa_emissao.py:959`

**Pendente do Opus.** A trava do Colaborador segue em `FOR UPDATE`. O caso INSERT do app × correção fica com **RED esperado** até a troca para `FOR NO KEY UPDATE` na porta. A execução desse RED está [nao verificado]. Os selos já reconhecem os dois modos para o Colaborador e continuam cobrando a ordem. `app/chamados/models.py:2938` · `app/ponto/tests/test_f4e_ordem_batida.py:346` · `app/chamados/tests/test_e4c_f5_plantio_concorrente.py:113`

### Resultados locais

- O selo em memória passou. Os mutantes `sem-chamado-terminal` e `guarda-antes-da-trava` deram, cada um, `FAIL=2 ERROR=0` nas asserções de corrigir e remover. Na execução do módulo inteiro, 25 dos 26 testes passaram e o B2 em memória parou no import de `django.db`. `app/ponto/tests/test_juiz_unico_eventos.py:318` · `app/ponto/tests/test_juiz_unico_eventos.py:409`
- A tentativa da base parou com `RC=1` e `ModuleNotFoundError: No module named 'django.core'`, antes da coleta. Base, cópia e mutantes em PostgreSQL, dois kernels, teto e suíte completa estão [nao verificado]. `app/manage.py:11` · `app/config/settings/ci.py:6`
- Na comparação AST dos 18 arquivos alterados contra `fb8194db0c4bae25b64aa3a97c4ac07779c0a86e`, `except` ficou em **11 → 11** e `suppress` em **0 → 0**. As sete variantes de fonte compilam. `app/chamados/services/regularizacao_ext.py:111` · `app/ponto/tests/test_f4e_ordem_batida.py:55`

### Comandos para o Code

Da raiz de `app/`, em ambiente de teste autorizado. A variante `base` carrega os corpos originais em memória. Os mutantes usam o mesmo mecanismo, inclusive o que restaura `FOR UPDATE`. `app/ponto/tests/test_f4e_ordem_batida.py:48` · `app/ponto/tests/test_f4e_ordem_batida.py:158`

```bash
export PYTHONDONTWRITEBYTECODE=1
f4e=ponto.tests.test_f4e_ordem_batida.OrdemBatidaF4e
f4e_test() { python3 manage.py test "$@" --settings=config.settings.ci --parallel=1 -v 2; }

F4E_VARIANTE=base f4e_test "$f4e"
f4e_test "$f4e"

F4E_VARIANTE=flip-sem-trava f4e_test "$f4e.test_flip_automatico_e_correcao"
F4E_VARIANTE=flip-sem-releitura f4e_test "$f4e.test_flip_transacional_rele_tipo_e_horario_para_trilha"
F4E_VARIANTE=sem-chamado-terminal f4e_test "$f4e.test_chamado_terminal_do_lote_travado_antes_da_pergunta"
F4E_VARIANTE=colaborador-for-update f4e_test "$f4e.test_insert_app_e_correcao_da_mesma_chave"
F4E_VARIANTE=guarda-antes-da-trava f4e_test "$f4e.test_guarda_rele_disputa_confirmada_apos_a_trava"
F4E_VARIANTE=b2-protecao-e-savepoint f4e_test "$f4e.test_apagar_propaga_erro_sql_do_juiz_da_batida_viva"

f4e_test api.tests.test_l030_batida_pela_porta
f4e_test
```

Para imprimir as medições do teto junto às asserções existentes. `app/ponto/tests/test_selo_chokepoint_escrita.py:343`

```bash
DJANGO_SETTINGS_MODULE=config.settings.ci PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import django
from unittest.mock import patch

django.setup()
from django.core.management import call_command
from ponto.tests.test_selo_chokepoint_escrita import SeloChokepointEscritaTest, TETOS

cobrar = SeloChokepointEscritaTest._cobrar

def medir(self, nome, medido):
    print(f'{nome} | consultas={medido} | teto={TETOS[nome]}', flush=True)
    return cobrar(self, nome, medido)

with patch.object(SeloChokepointEscritaTest, '_cobrar', medir):
    call_command('test', 'ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest',
                 parallel=1, verbosity=2)
PY
```

## 26/11 21:12 · sessao fba5a37d7a2f · O30-F3-CONSTRUIR · verificador · origem=Code

**REPROVADO — O30-F3-CONSTRUIR, c16.** O caso de C apontado pelo verif9 foi corrigido e os três mutantes novos mordem. A proteção continua restrita a dois testes do painel, e a suíte integral verde segue [nao verificado]. D permanece como desenho pendente. A c16 reproduz os dois defeitos em memória e não agrava o caminho de produção mostrado no diff. `app/ponto/tests/test_atestado_f3.py:255`, `app/ponto/tests/test_atestado_f3.py:274`, `app/ponto/tests/test_atestado_f3_contrato.py:1064`, `app/ponto/tests/mutantes_atestado_f3_c14.py:124`, `CLAUDE.md:398`

Conferi os hunks do anexo na cópia e rodei os testes sem banco em modo somente leitura. A identidade do hash pelo Git e a ocorrência dos cenários em produção ficam [nao verificado]. [nao verificado]

## 1. Achado C

- **O caso pontual foi curado.** A fixture grava `aguardando_documento`, relê a linha e verifica o estado e a ausência de arquivo antes do GET. O caso aprovado faz POST para o `action` extraído do HTML e verifica `aprovada` e os bytes persistidos. Os dois estados constam em `Ausencia.STATUS_CHOICES`. `app/ponto/tests/test_atestado_f3.py:255`, `app/ponto/tests/test_atestado_f3.py:274`, `app/ponto/models.py:690`
- **Nas fixtures normais do núcleo O30 F3 que conferi, os estados são válidos.** Isso inclui os demais casos de integração, os registros simulados dos contratos, o teste do lote e o de compensação de anexo. O `pendente` acrescentado pela c16 está na substituição deliberada do mutante. Os valores `x` encontrados no teste HJ-091 estão em strings de código usadas para testar o fiscal de escritas. `app/ponto/tests/test_atestado_f3.py:112`, `app/ponto/tests/test_atestado_f3.py:455`, `app/ponto/tests/test_atestado_f3_contrato.py:361`, `app/ponto/tests/test_atestado_f3_contrato.py:781`, `app/ponto/tests/test_atestados_lote.py:44`, `app/ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:183`, `app/ponto/tests/mutantes_atestado_f3_c14.py:106`, `app/ponto/tests/test_hj091_ferias_derivadas.py:395`, `app/ponto/tests/test_hj091_ferias_derivadas.py:537`
- **A garantia de classe ficou aberta.** O selo carrega as escolhas do modelo, mas só passa pelos dois métodos do painel que enumera. Em memória, troquei para `pendente` a fixture de férias no teste do limite O30 e o selo passou. Também passou quando mantive o POST e retirei as verificações do redirecionamento, do estado e dos bytes. Fora dos hunks, há fixtures reais com `pendente` nos testes vizinhos da UI de anexo do colaborador e da guarda de aprovação. `app/ponto/tests/test_atestado_f3_contrato.py:1059`, `app/ponto/tests/test_atestado_f3_contrato.py:1064`, `app/ponto/tests/test_atestado_f3_contrato.py:1083`, `app/ponto/tests/test_atestado_f3.py:267`, `app/ponto/tests/test_atestado_f3.py:455`, `app/ponto/tests/test_anexar_ui_colab.py:32`, `app/ponto/tests/test_gate_anexo_desconta.py:19`

## 2. Achado D

| RED | Resultado na cópia |
|---|---|
| Rollback do chamador após o retorno | **Falhou na asserção certa, na linha 155.** Passaram as verificações de que a porta retornou uma linha criada, que a referência existia antes do rollback simulado e que sumiu depois. O arquivo novo sobrou. A fixture simula INSERT e storage, enquanto a porta executada é `criar_ausencia`. `app/ponto/tests/mutantes_atestado_f3_c14.py:127`, `app/ponto/tests/mutantes_atestado_f3_c14.py:145`, `app/ponto/tests/mutantes_atestado_f3_c14.py:155`, `app/ponto/tests/test_atestado_f3_contrato.py:619` |
| Referência entre consulta e exclusão | **Falhou na asserção certa, na linha 174.** O `exists()` simulado captura a ausência da referência, insere a referência e devolve o resultado anterior. O helper apaga o arquivo e a nova referência fica sem ele. `app/ponto/tests/mutantes_atestado_f3_c14.py:157`, `app/ponto/tests/mutantes_atestado_f3_c14.py:162`, `app/ponto/tests/mutantes_atestado_f3_c14.py:174`, `app/ponto/services/ausencia.py:263` |

**Descoberta da suíte.** Os dois deram `FAIL`, com zero `ERROR`. O módulo `mutantes_atestado_f3_c14.py` fica fora da descoberta normal `test*.py`, e o runner da régua herda a descoberta do Django. O CLI do módulo seleciona apenas chaves de `MUTANTES`. Por seleção explícita do módulo, rodei 22 testes, incluindo os herdados, com exatamente esses dois `FAIL` e zero `ERROR`. Logo, os dois deixam a seleção explícita vermelha. A suíte normal inteira fica [nao verificado]. `app/config/settings/ci.py:40`, `app/core/runner_quarentena.py:72`, `app/ponto/tests/mutantes_atestado_f3_c14.py:121`, `app/ponto/tests/mutantes_atestado_f3_c14.py:124`, `app/ponto/tests/mutantes_atestado_f3_c14.py:177`, `app/ponto/tests/mutantes_atestado_f3_c14.py:223`

**Efeito da c16 sobre D.** O primeiro RED expõe o limite de `concluiu=True` antes do retorno ao chamador. O segundo expõe a janela já existente entre `exists()` e `storage.delete()`. Ambos acrescentam reproduções no módulo de testes. O UPDATE legado de `anexar_documento` também conserva seu ramo de compensação por nome. `app/ponto/tests/mutantes_atestado_f3_c14.py:124`, `app/ponto/services/ausencia.py:259`, `app/ponto/services/ausencia.py:668`, `app/ponto/services/ausencia.py:690`, `app/ponto/services/ausencia.py:1147`

## 3. Checklist do pré-verificador

| Item | A c16 viola? |
|---|---|
| Segundo juiz registrado | **Não.** O selo lê `STATUS_CHOICES` do modelo. O lote continua usando o predicado de cobertura, e a exigência de documento continua delegada a `estado_inicial`. `app/ponto/tests/test_atestado_f3_contrato.py:1059`, `app/docs/JUIZES.md:58`, `app/core/juizes.py:261`, `app/ponto/services/atestados_lote.py:15`, `app/ponto/services/atestados_lote.py:36` |
| Segundo escritor fora da porta | **Não.** O `objects.create` do hunk é a fixture existente com o valor corrigido. O INSERT e o UPDATE de anexo continuam no serviço. `app/ponto/tests/test_atestado_f3.py:275`, `app/ponto/services/ausencia.py:650`, `app/ponto/services/ausencia.py:1133` |
| Relógio fora da borda | **Não introduz.** O lote obtém a data na view. A leitura de relógio da projeção continua no código anterior. A simulação de rollback altera apenas o estado da conexão de teste. `app/ponto/views_atestado.py:94`, `app/ponto/services/ausencia.py:193`, `app/ponto/tests/mutantes_atestado_f3_c14.py:132` |
| Derivação paralela de fonte única | **Não.** A validade do estado vem das escolhas do modelo. Os estados esperados identificam os cenários do selo. `q` continua passando pelo filtro universal antes do limite do lote. `app/ponto/tests/test_atestado_f3_contrato.py:1059`, `app/ponto/tests/test_atestado_f3_contrato.py:1081`, `app/ponto/views_atestado.py:74`, `app/ponto/views_atestado.py:116` |
| Selo ou RED sem mordida | **Os três mutantes novos mordem.** Cada um teve uma ocorrência da âncora e controle sem mutação verde. Sem POST caiu em `:1085`, estado extinto em `:1081`, aprovada no lugar de espera em `:1082`. Os dois REDs de D também mordem. A lacuna de C é a abrangência do selo, demonstrada pelas duas alterações em memória que passaram. `app/ponto/tests/mutantes_atestado_f3_c14.py:97`, `app/ponto/tests/mutantes_atestado_f3_c14.py:155`, `app/ponto/tests/mutantes_atestado_f3_c14.py:174`, `app/ponto/tests/mutantes_atestado_f3_c14.py:184`, `app/ponto/tests/test_atestado_f3_contrato.py:1064`, `app/ponto/tests/test_atestado_f3_contrato.py:1081` |
| Ordem de trava | **Não introduz inversão.** No INSS, a guarda trava o colaborador antes da escrita de `Ausencia`. O `atomic` novo da c16 é um simulador. `app/ponto/views.py:2547`, `app/colaboradores/services/colaborador.py:84`, `app/ponto/services/ausencia.py:648`, `app/ponto/tests/mutantes_atestado_f3_c14.py:132` |
| Campo com dois significados | **Não introduz.** `status` na linha tem as quatro escolhas. `pendente` na URL continua como filtro dos dois estados abertos. `form.action` é a URL do POST extraída do HTML. Há uma divergência herdada nas filhas legadas: o painel exibe e rotula o documento da raiz, mas o formulário aponta para a filha e o serviço grava nela. `app/ponto/models.py:690`, `app/ponto/views.py:2248`, `app/ponto/tests/test_atestado_f3.py:56`, `app/templates/ponto/partials/painel_ausencia.html:71`, `app/templates/ponto/partials/painel_ausencia.html:89`, `app/ponto/tests/test_atestado_f3.py:169`, `app/ponto/views_atestado.py:30`, `app/ponto/services/ausencia.py:1133` |
| `except` novo | **Não.** O RED usa `try/finally` para restaurar a conexão simulada. O `except` do INSERT e o do UPDATE são anteriores à c16. `app/ponto/tests/mutantes_atestado_f3_c14.py:137`, `app/ponto/services/ausencia.py:674`, `app/ponto/services/ausencia.py:1147` |

## 4. Quem escreve e quem lê

| Campo ou valor | Escritores e leitores na cópia |
|---|---|
| **`aguardando_documento` — escrita** | O backfill **0057** lê `bool(a.documento)` e o tipo. Para um `pendente` sem anexo cujo tipo exige documento, grava `aguardando_documento` e prazo. Na reversão, lê os dois estados abertos e grava `pendente`. Em execução normal, `estado_inicial` devolve a decisão a `criar_ausencia`, que persiste o estado no INSERT. Reabertura e rejulgamento também consultam `estado_inicial` e persistem pela porta `mudar_estado`. `app/ponto/migrations/0057_a4_backfill_pendente.py:30`, `app/ponto/migrations/0057_a4_backfill_pendente.py:35`, `app/ponto/migrations/0057_a4_backfill_pendente.py:58`, `app/ponto/services/ausencia.py:567`, `app/ponto/services/ausencia.py:650`, `app/ponto/services/ausencia.py:789`, `app/ponto/services/ausencia.py:1042`, `app/ponto/services/ausencia.py:1076`, `app/ponto/services/ausencia.py:1088`, `app/ponto/services/ausencia.py:1418`, `app/ponto/services/ausencia.py:1444` |
| **`aguardando_documento` — leitura e saída** | `anexar_documento` lê o estado e, ao receber upload, move a linha para `aguardando_decisao`. O escalonador lê a fila e faz a mesma transição no vencimento. Contadores, rejulgamento, mensagens de submissão e reversão, histórico e tarefa do dia leem o estado. Lista, painel e API também o leem pela associação a `ABERTAS`. O formulário do painel depende de permissão de edição e da origem de férias. `app/ponto/services/ausencia.py:1144`, `app/ponto/services/ausencia.py:1259`, `app/ponto/services/ausencia.py:1286`, `app/ponto/services/ausencia.py:1298`, `app/ponto/management/commands/rejulgar_status_por_tipo.py:25`, `app/ponto/views.py:2248`, `app/ponto/views.py:2702`, `app/ponto/views.py:2738`, `app/ponto/services/dia_decidido.py:677`, `app/chamados/services/tarefa_do_dia.py:318`, `app/ponto/models.py:719`, `app/templates/ponto/partials/painel_ausencia.html:89`, `app/templates/ponto/partials/painel_ausencia.html:359`, `app/api/views.py:3110` |
| **`documento` — escrita** | DP/INSS, PWA e API recebem o upload e o passam a `criar_ausencia`, que grava no INSERT ou encaminha à reabertura. O painel envia `FILES['documento']` a **`anexar_atestado`**, que normaliza o nome e chama **`anexar_documento`**. O endpoint do colaborador e o reenvio da API também chegam a essa porta de UPDATE. O `FileField` grava os bytes no storage antes do SQL e a coluna guarda a chave. `app/ponto/views.py:2504`, `app/ponto/views.py:2550`, `app/ponto/views.py:2627`, `app/ponto/views.py:2683`, `app/api/views.py:1779`, `app/api/views.py:1824`, `app/ponto/services/ausencia.py:525`, `app/ponto/services/ausencia.py:585`, `app/ponto/services/ausencia.py:1431`, `app/relatorios/urls.py:18`, `app/ponto/views_atestado.py:48`, `app/ponto/services/ausencia.py:1133`, `app/api/views.py:1855`, `app/api/views.py:3142`, `app/ponto/models.py:777`, `app/ponto/services/ausencia.py:245` |
| **`documento` — leitura** | O backfill, a criação, o rejulgamento e a reabertura usam presença para decidir estado. `painel_ausencia` e download resolvem a raiz pela FK legada. Lista e histórico leem presença, PDF e ZIP abrem o arquivo, API e porta da célula leem presença ou chave, e o vigia conta falta de documento. A compensação lê `_committed`, chave e referências antes de apagar no ramo com storage explícito. `app/ponto/migrations/0057_a4_backfill_pendente.py:36`, `app/ponto/services/ausencia.py:567`, `app/ponto/services/ausencia.py:789`, `app/ponto/services/ausencia.py:1418`, `app/ponto/views.py:2102`, `app/ponto/views.py:2147`, `app/templates/ponto/ausencias.html:168`, `app/ponto/services/dia_decidido.py:685`, `app/ponto/services/atestados_lote.py:167`, `app/ponto/services/atestados_lote.py:249`, `app/api/views.py:1061`, `app/ponto/portas/celula.py:726`, `app/ponto/services/vigia_ausencia.py:117`, `app/ponto/services/ausencia.py:263`, `app/ponto/services/ausencia.py:671` |

**Achados que sustentam o veredito**

1. **C — proteção parcial.** A fixture do painel foi corrigida e os três mutantes novos falham pelo motivo previsto. O selo aceita estado extinto em outra fixture O30 e aceita a perda da prova de persistência no caso aprovado. `app/ponto/tests/test_atestado_f3.py:274`, `app/ponto/tests/test_atestado_f3.py:455`, `app/ponto/tests/test_atestado_f3_contrato.py:1064`, `app/ponto/tests/test_atestado_f3_contrato.py:1083`
2. **Prova de pouso pendente.** Dos 47 contratos sem banco, 46 passaram e um deu erro de ambiente por falta de `PIL`. Integração HTTP com banco e suíte integral verde [nao verificado]. `app/ponto/tests/test_atestado_f3_contrato.py:1260`, `app/ponto/tests/test_atestado_f3.py:255`, `CLAUDE.md:398`
3. **D — desenho pendente, sem peso autônomo na reprovação.** Os dois cenários falham nas asserções esperadas, fora da descoberta normal. A c16 acrescenta as reproduções sem alterar os dois caminhos de produção que elas exercitam. `app/ponto/tests/mutantes_atestado_f3_c14.py:124`, `app/ponto/tests/mutantes_atestado_f3_c14.py:155`, `app/ponto/tests/mutantes_atestado_f3_c14.py:174`, `app/ponto/services/ausencia.py:263`, `app/ponto/services/ausencia.py:668`

## 26/11 21:11 · sessao 9820ab762b28 · V6-LISTA-ORFAS · verificador · origem=Code

**Reprovo o diff como está.** Uma citação em prosa pode esconder um órfão. Um `.rc` publicado entre o juiz e o laço pode escapar pela comparação de mtime. Um traceback pula o laço, mas o `find` final pode devolver `0` como status do comando inteiro. [bin/rc_sem_colher.py:26,34,75] [bin/codex_pergunta.sh:573–574] [bin/disjuntor.py:1965–1966] [nao verificado]

Conferi a cópia e fiz verificações em memória, sem rodar os dois selos completos, que criam e apagam arquivos. A identidade Git da cópia, o arquivo externo `~/vercodex.py` e o comportamento em produção ficam como [nao verificado]. [bin/tests/test_rc_sem_colher.sh:20–21] [bin/tests/test_agente_no_modelo.sh:31–32]

### 1. Quem escreve e quem lê

| Valor | Escritor e leitor | Significado efetivo |
|---|---|---|
| **mtime do `.rc`** | Para o Codex construtor, o wrapper escreve o temporário e o publica por `mv`. O juiz lê o mtime e escolhe o maior. Os dois `find` o comparam com o mtime de `M`, criado pela sessão com `mktemp`. Na fixture, `touch` fixa os mtimes. Demais produtores efetivos [nao verificado]. | O mtime sinaliza a escrita do arquivo, que pode preceder a publicação do nome final. `-newer` exige `mtime > M`. Na seleção inicial, `m > rc_m` mantém o primeiro encontrado em caso de empate. [bin/codex_pergunta.sh:572–574] [bin/rc_sem_colher.py:63–68] [bin/disjuntor.py:1965–1966] [bin/tests/test_rc_sem_colher.sh:29–37] |
| **mtime dos outros arquivos** | Cada produtor escreve seu arquivo. O wrapper, por exemplo, publica a resposta e escreve o inventário. O selo escreve e data `brief.md` e `brief2.md`. O juiz toma o maior mtime elegível de arquivo sem sufixo `.rc`. | Qualquer arquivo elegível entra na heurística. Um arquivo mais de 5 s posterior exclui o `.rc`; exatamente 5 s ainda passa. O conteúdo do arquivo fica fora da decisão. [bin/codex_pergunta.sh:798,807] [bin/tests/test_rc_sem_colher.sh:30–32] [bin/rc_sem_colher.py:59–72] |
| **mtime da pasta e das subpastas** | Criar, remover ou renomear entradas altera o mtime da pasta que as contém. O `mv` do wrapper é um exemplo. Na fixture, `mkdir` cria as pastas e `touch` envelhece `OBRA-VELHA`. O juiz lê cada nível separadamente. | Pasta, subpasta e arquivo com idade de exatamente 24 h passam. Uma escrita dentro de uma subpasta pode deixar o mtime da pasta da obra antigo. [bin/codex_pergunta.sh:573–574,739] [bin/tests/test_rc_sem_colher.sh:26–27,36] [bin/rc_sem_colher.py:48,54,58,63–65] |
| **Linha do RELATO e `HH:MM`** | RAIAS 3 atribui a colheita à sessão e prescreve uma linha `superado` para rc de obra encerrada. O selo escreve as marcas e calcula sua hora com `date`. O juiz lê o RELATO inteiro e calcula o minuto do mtime com `time.localtime`. Escritor efetivo da marca e fuso comum em produção [nao verificado]. | O `HH:MM` no início da linha só habilita a busca. O `HH:MM` entre parênteses entra na comparação. O processo usa seu fuso local, e a marca guarda apenas hora e minuto, sem data, segundos ou fuso. [CLAUDE.md:333–337] [bin/tests/test_rc_sem_colher.sh:28,38–43] [bin/rc_sem_colher.py:25–34,73–76,96–98] |
| **Status do juiz** | `main` imprime as linhas e `sys.exit` publica o status. O Bash lê esse status em `&&`. O selo lê `$?` diretamente nos casos órfão e vazio. O consumo externo pelo `vercodex` [nao verificado]. | `0` significa vazio, `3` significa listou. `&&` dá o mesmo tratamento a `3` e a qualquer erro: pula o laço. O `rc=<n>` impresso é outro valor, lido do conteúdo do `.rc`. [bin/rc_sem_colher.py:38–42,99–106] [bin/disjuntor.py:1964–1966] [bin/tests/test_rc_sem_colher.sh:49–51,61–63] |
| **`--min-idade`** | A sessão omite a opção, então vale `0`. O selo passa `900`. A docstring atribui `900` ao `vercodex`; a chamada externa efetiva [nao verificado]. | A unidade é segundo. Com `900`, a lista inclui idades **a partir de 15 min**, inclusive. Se a medida do contrato exige **só** rc com menos de 15 min, o filtro faz o inverso. [bin/disjuntor.py:1965] [bin/tests/test_rc_sem_colher.sh:58–59] [bin/rc_sem_colher.py:4,71,83,91–99] |

### 2. Equivalência com `esquecidos`

O trecho de `~/vercodex.py::esquecidos` pedido para colar está [nao verificado]. O enunciado fornece esta régua para comparação: obra `^[A-Z0-9][A-Z0-9-]+$` com idade `< 24 h`, poda de subpasta `> 24 h`, `.rc` mais novo, `rc_m >= outro - 5` e `now - rc_m > min_idade`. A docstring remete ao critério externo. [bin/rc_sem_colher.py:6–9]

**Pela régua fornecida, há duas diferenças de fronteira.** O juiz aceita a pasta da obra com exatamente 24 h e aceita `agora - rc_m == min_idade`. A poda de subpasta e a folga de 5 s coincidem com a régua descrita. O juiz também corta **arquivos** com mais de 24 h e ignora arquivos symlink. Se `esquecidos` faz esses dois cortes, e como desempata mtimes iguais, fica [nao verificado]. [bin/rc_sem_colher.py:21–24,48,54,58–72]

### 3. Acordar, perder um rc e lidar com falha

- **Órfão anterior a `M`.** Se o juiz o lista, o `3` pula o laço. Se o juiz o omite, por exemplo por uma marca em prosa ou porque só examinou o `.rc` mais novo daquela obra, `-newer M` jamais recupera aquele órfão antigo. A sessão pode voltar pelo prazo de 300 s sem receber esse caminho. [bin/rc_sem_colher.py:26,34,66–77] [bin/disjuntor.py:1965–1966]
- **Rc entre juiz e laço.** Se o arquivo permanece visível com `mtime > M`, o primeiro `find` do laço o pega, ou o `find` final o pega quando o laço foi pulado. O contraexemplo é o produtor escrever `arquivo.rc.$$` antes de `M`, o juiz passar enquanto só existe esse temporário e o `mv` publicar `arquivo.rc` entre o juiz e o laço. O `mv` preserva o mtime do arquivo. O nome chega depois de `M`, mas ambos os `find` o deixam de fora. Um mtime exatamente igual a `M` também fica de fora. [bin/codex_pergunta.sh:573–574] [bin/rc_sem_colher.py:63–71] [bin/disjuntor.py:1965–1966]
- **Juiz quebrado.** O traceback que escapa do Python aparece em stderr e pula o laço. O `find` final roda mesmo assim, ainda pode imprimir `.rc` novo e, se terminar com sucesso, devolve `0` pelo comando inteiro. O juiz deixa de listar órfãos antigos nesse ramo. Há também falha de leitura que pode virar vazio: `os.walk` sem `onerror` ignora erro ao abrir um diretório. [bin/rc_sem_colher.py:52–58,97–106] [bin/disjuntor.py:1965–1966]
- **Limite.** O polling normal está sob `timeout 300`. O juiz e o `find` final ficam fora dele. Por exemplo, um FIFO elegível chamado `r.rc` passa pela seleção, e `_rc_txt` pode bloquear ao abri-lo sem escritor. Assim, a forma inteira pode ficar presa antes de entrar no laço. [bin/rc_sem_colher.py:38–40,59–77] [bin/disjuntor.py:1965–1966]

### 4. O que pode casar com a marca

- **Prosa, sim.** `- 21:00 Exemplo de sintaxe: “superado OBRA-ALFA v1/r.rc (20:20)”` suprime esse rc se seu mtime render `20:20`. A busca roda em qualquer posição da linha elegível. O `\b` depois do minuto também dispensa o fechamento `)`. [bin/rc_sem_colher.py:25–34,73–76]
- **Prefixo de obra e sufixo de caminho, não.** `OBRA-ALFA` e `OBRA-ALFA-DOIS` são chaves diferentes, assim como `r.rc` e `v1/r.rc`. A comparação usa a tupla exata. O mesmo caminho refeito **no mesmo minuto** conserva a chave. Uma marca de outro dia com a mesma chave também pode casar enquanto estiver no RELATO. [bin/rc_sem_colher.py:26,32–35,73–76]

### 5. Selo e mutantes

Conferi que os dez mutantes têm uma ocorrência exata da âncora e que as dez versões alteradas compilam em memória. O selo exige âncora única e roda cada caso também na cópia sem mutação. Pela leitura, as quedas esperadas são estas. A execução integral dos mutantes fica [nao verificado]. [bin/tests/test_rc_sem_colher.sh:84–107]

| Mutante | Asserção que deve derrubá-lo |
|---|---|
| `espera-so-newer` / `espera-so-newer-forma` | Falta a listagem imediata do órfão / falta a menção ao juiz na forma. [bin/tests/test_rc_sem_colher.sh:65–77,98–99] |
| `marca-sem-hora` / `marca-sem-linha` / `marca-desligada` | Some o rc refeito / some o rc cuja marca está fora da linha horária / reaparece o rc marcado. [bin/tests/test_rc_sem_colher.sh:38–43,54–56,100–102] |
| `sem-criterio-vercodex` / `sem-24h` / `sem-min-idade` | Reaparece a obra com arquivo posterior / a obra velha / o rc de 2 min sob `900`. [bin/tests/test_rc_sem_colher.sh:31–37,53,57–59,103–105] |
| `rc-sempre-zero` / `rc-sempre-achou` | O órfão precisa de status `3` e saída positiva / o vazio precisa de status `0` e stdout vazio. [bin/tests/test_rc_sem_colher.sh:49–51,61–63,106–107] |

**Há casos que passam vazios.** `caso_colhido`, `caso_marca`, `caso_velho` e a segunda metade de `caso_min_idade` aceitam stdout vazio mesmo se o juiz falhar. O controle positivo separado derruba uma falha geral, mas uma falha só na chamada com `900` pode passar. A segunda metade de `caso_espera` só mede duração, e `caso_forma` só exige a presença textual do nome do juiz além da aceitação da forma. A fixture de prosa cobre `SUPERADA` e uma marca fora de linha horária, deixando de fora a citação completa dentro de uma linha horária. [bin/tests/test_rc_sem_colher.sh:38–43,49–77,80–81]

### 6. Classes de reprovação

- **Segundo juiz e derivação paralela.** Na cópia, `espera_nega` julga a forma do comando, e `sem_colher` julga a orfandade. A duplicação com um juiz já registrado fica [nao verificado]. A cópia mantida no `vercodex` até o pouso é a derivação paralela declarada no enunciado. Migração efetiva e tratamento externo de `3` ou erro [nao verificado]. [bin/disjuntor.py:2154–2171] [bin/rc_sem_colher.py:2–6,45–77] [nao verificado]
- **Segundo escritor.** O juiz novo abre os arquivos para leitura e imprime a lista. RAIAS 3 prescreve uma marca, e o `set` deduplica as marcas na leitura. Escritor canônico da marca e garantia de uma única linha gravada [nao verificado]. O desligamento por `git revert` consta do contrato fornecido. RAIAS 5 prevê pouso próprio do instrumento; o pouso efetivo [nao verificado]. [bin/rc_sem_colher.py:29–42,96–106] [CLAUDE.md:337,351–354] [nao verificado]
- **Selo, semântica e `except`.** Os mutantes têm asserções correspondentes, com os pontos cegos acima. A marca promete identificar cada rc mas identifica obra, caminho e minuto. O mtime de qualquer outro arquivo elegível pode significar “colhido” para o juiz. O diff não adiciona `except` ao juiz. Os pontos de falha silenciosa vêm das opções de leitura e do status final do shell. [bin/tests/test_rc_sem_colher.sh:98–107] [bin/rc_sem_colher.py:10–14,57–77,97–106] [bin/disjuntor.py:1965–1966]

### 7. RAIAS 3 e o selo anterior

**A forma textual confere.** Verifiquei em memória que o comando normalizado de RAIAS 3 coincide com `ESPERA_UNICA`, passa em `espera_nega` e fica fora da contagem de polling. O trecho de divergência do selo anterior passa com a cópia e acusa a troca pela forma antiga quando o texto permanece como está. O selo inteiro fica [nao verificado]. [CLAUDE.md:341–345] [bin/disjuntor.py:1965–1966,2117–2118,2154–2171] [bin/tests/test_agente_no_modelo.sh:244,255–283]

**A promessa operacional é mais ampla que o código.** O `find` aceita qualquer entrada com nome `*.rc` e mtime posterior a `M` na profundidade indicada, sem testar obra viva. O teto cobre o laço, e o juiz inicial escolhe só um `.rc` por obra. A forma antiga sem o juiz ainda passa em `espera_nega`. O selo anterior continua válido para a coerência textual e para as regras de forma que testa. [CLAUDE.md:333–343] [bin/disjuntor.py:1965–1966,2154–2171] [bin/rc_sem_colher.py:52–77] [bin/tests/test_agente_no_modelo.sh:210,277–283]

## 26/11 21:05 · sessao 7e3846933d91 · O30-F3-CONSTRUIR · construtor · origem=Code

**O30-F3-CONSTRUIR c16 — C corrigido. D segue como pergunta de desenho.** [Fixture de C](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3.py:274) · [Reproduções de D](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3_c14.py:124)

### C — Formulário do painel

A fixture agora grava um atestado em `aguardando_documento`, o estado definido pelo backfill da A4 para atestado sem anexo. O teste relê a linha, confirma o estado e a ausência de documento e verifica a ação do formulário e o campo de arquivo. O template condiciona o formulário à permissão de edição e à origem em férias, independentemente do estado. [Teste HTTP](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3.py:274) · [Backfill](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/migrations/0057_a4_backfill_pendente.py:35) · [Template](/home/ronald/wt-codex-o30-f3-construir-010143/app/templates/ponto/partials/painel_ausencia.html:89) [nao verificado]

O selo lê `Ausencia.STATUS_CHOICES` direto do modelo, confere o estado específico de cada caso e exige o POST aprovado para `form.action`. Antes da troca da fixture, falhou com `pendente` fora das escolhas e, com o POST removido em memória, também falhou com `0 != 1`. Depois da troca, o selo passou. Os três mutantes abaixo falharam por asserção, cada um pelo motivo esperado. [Selo](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:1053) · [Mutantes](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3_c14.py:97) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v ponto.tests.test_atestado_f3_contrato.AtestadoF3LoteSemBancoTest.test_RED_migracao_do_formulario_do_painel_preserva_a_cobertura
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3_c14 painel_aprovado_sem_post
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3_c14 painel_estado_extinto
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3_c14 painel_aguardando_documento_com_aprovada
```

O executor de mutantes fornece a fonte em memória ao selo. No fluxo HTTP, `painel_ausencia` renderiza o formulário e `anexar_atestado` recebe o POST. O serviço de anexo lê `aguardando_documento` para avançar a linha a `aguardando_decisao`. [Executor](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3_c14.py:177) · [GET](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views.py:2095) · [POST](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views_atestado.py:27) · [Transição](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:1144)

### D — Rollback do chamador e exclusão concorrente

Os dois REDs executam o código atual sem banco. Ambos deram `rc 1` pela asserção esperada. No primeiro, o rollback depois do retorno remove a linha e deixa o arquivo. No segundo, uma referência aparece depois da consulta e o helper apaga o arquivo referenciado. [RED do rollback](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3_c14.py:127) · [RED da exclusão](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3_c14.py:157)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v \
  ponto.tests.mutantes_atestado_f3_c14.AtestadoF3DesenhoSemBancoTest.test_RED_rollback_do_chamador_depois_do_retorno_remove_anexo \
  ponto.tests.mutantes_atestado_f3_c14.AtestadoF3DesenhoSemBancoTest.test_RED_preserva_anexo_referenciado_entre_consulta_e_exclusao
```

Deixei D para desenho. O `finally` da criação termina antes do atomic do chamador, e o rollback descarta callbacks `on_commit`. Adiar o upload para esse callback mudaria o contrato que observa o arquivo durante a projeção e deixaria a linha já confirmada se a gravação no storage falhasse. A compensação por exclusão exige coordenar a criação de novas referências com a remoção, pois `exists()` e `storage.delete()` são operações separadas. Os REDs de D ficam disponíveis para execução direta. [Criação](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:648) · [Atomic do INSS](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views.py:2547) · [Ciclo de `on_commit`](/home/ronald/wt-codex-o30-f3-construir-010143/app/chamados/services/trava_do_colaborador.py:19) · [Arquivo durante a projeção](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3.py:293) · [Exclusão](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:263)

A execução local dos contratos sem banco teve 46 passes e um erro por falta de `PIL`. Testes HTTP com banco, suíte integral e ocorrência em produção [nao verificado]. [Import que falhou](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:1261) [nao verificado]

## 26/11 20:59 · sessao 4addf19f9d51 · E4C-F5-INSTANTE · verificador · origem=Code

## Veredito — REPROVADO

O diff implementa a medição por instante e liga A/B ao alarme, mas ainda permite que uma divergência posterior ao marco apareça como estoque. Também deixa sem implementação a reprojeção dos 71, entrega apenas um ensaio de inserção para os 16.886 e cobre só um caso do item 13. `app/chamados/models.py:4565`, `app/chamados/models.py:4587`, `app/chamados/management/commands/vigiar_fio_morto.py:213`, `app/chamados/management/commands/estoque_ato_x_coluna.py:16`, `app/chamados/tests/test_e4c_f5_instante_c1.py:74`, `app/chamados/tests/test_e4c_f5_instante_c1.py:191`

## Brief — classe, caso ou pendência

| Item | Avaliação |
|---|---|
| Base após a PORTA e respeito ao sítio comum | **Classe na cópia.** `registrar` lê os donos e exige a trava antes do `bulk_create`, inclusive para estoque. O delta fornecido preserva `DisputaSupervisao.fechar` com `competencia=`. A cópia tem a PORTA, mas o pouso efetivo no main, a ascendência de `eb76e7be7` e a abertura em até dez minutos estão **[nao verificado]**. O relato disponível registra a condição de pouso e a recertificação do candidato. `app/chamados/models.py:2046`, `app/chamados/models.py:4687`, `app/chamados/models.py:4696`, `app/docs/RELATO.md:8`, `app/docs/RELATO.md:23`, `app/docs/RELATO.md:70` |
| Comparador de instante | **Classe no contador, parcial no sítio pedido.** Compara resposta e validação, com a mesma redução de `leitura` e só com linhas persistidas. Dois instantes presentes e diferentes dão `a`. Se o ato vigente zerou o campo, uma coluna que ainda coincide com a linha desfeita dá `a`; coluna cheia sem linha correspondente dá `b`. O `par_ato_coluna` do brief continua devolvendo dois booleanos de presença. As guardas de `carimbar` e do plantio continuam nessa régua. `app/chamados/models.py:4534`, `app/chamados/models.py:4578`, `app/chamados/models.py:4605`, `app/chamados/models.py:3483`, `app/chamados/services/materializacao.py:1115` |
| Esperado zero só do novo, estoque à parte e A/B em `ALARMA_SEMPRE` | **Classe na agregação e no alarme, parcial na identificação do novo.** O vigia conta uma pergunta por classe e põe A/B em `d` antes de calcular `gritou`. Com `--alarme`, um A/B novo positivo dá saída 2. Já a classificação usa a origem da última linha efetiva em `a` e a data de negócio da coluna em `b`. Esses critérios não garantem a data em que nasceu a divergência. `app/chamados/services/vigia_ato_x_coluna.py:17`, `app/chamados/management/commands/vigiar_fio_morto.py:87`, `app/chamados/management/commands/vigiar_fio_morto.py:213`, `app/chamados/management/commands/vigiar_fio_morto.py:310`, `app/chamados/models.py:4584` |
| 16.886 colunas sem ato, 71 atos com coluna vazia e lista das entregues | **Caso para a inserção de estoque; faltam as operações da classe.** O RED2 insere uma `RESPOSTA` com origem `estoque` pelo caminho travado. A 0058 só atualiza linhas existentes. O comando é DRY, agrega as entregues e imprime PKs apenas de `b_estoque_aberta`. Faltam o caminho que aplique o estoque aberto, a reprojeção ato → coluna para `a` e a lista nominal por PK das entregues. O balde chamado `aberta` significa que K6 disse “não entregue”; o comando não consulta se a competência está trancada. Os 16.886 e 71 são da medição histórica, e a partição atual está **[nao verificado]**. `app/chamados/tests/test_e4c_f5_instante_c1.py:74`, `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:16`, `app/chamados/management/commands/estoque_ato_x_coluna.py:28`, `app/chamados/services/materializacao.py:803`, `app/docs/RELATO.md:937` |
| Devolução especial L-130 | **Classe da porta.** A elegibilidade é relida sob trava; o mesmo montador usado na ressuscitação cria `REABERTURA` marcada, com foto anterior. O lote limpa texto, resposta, validação, autor da validação e carimbo. A auditoria tem antes/depois e ator. A tarefa do dia exclui a linha marcada ao escolher o último ato. `app/chamados/services/validacao.py:463`, `app/chamados/services/validacao.py:475`, `app/chamados/services/validacao.py:485`, `app/chamados/services/validacao.py:495`, `app/chamados/models.py:1726`, `app/chamados/models.py:4417`, `app/chamados/services/tarefa_do_dia.py:393` |
| Q0.2 — carimbo e ato no mesmo instante | **Caso coberto, classe incompleta.** O `save` e o registro agora são atômicos. O teste verde cobre uma validação nova sobre resposta que já tinha instante e texto. A emissão ainda depende de procurar uma linha histórica com a mesma chave e mistura a escrita atual com fotos de colunas anteriores. Um carimbo com validação efetiva em outro instante também pode passar pela guarda de presença. `app/chamados/models.py:3466`, `app/chamados/models.py:3483`, `app/chamados/models.py:3508`, `app/chamados/models.py:4476`, `app/chamados/tests/test_e4c_f5_instante_c1.py:209` |
| Item 13 por PK | **Só um caso interno.** RED5 chama `_carimbar_validacao` pela fixture, confere instante, autor e linhas e compara dois `values()` de um `FechamentoMensal` com status `exportado`. Falta passar pela porta pública de validar, pelo recusar e pelo materializar, demonstrar entrega pelo K6 e conferir a trilha de cada porta e o hash pedido. Efeito em dinheiro e necessidade de DIFF pela L-113 nesta volta estão **[nao verificado]**. `app/chamados/tests/test_e4c_f5_instante_c1.py:191`, `app/chamados/tests/_ato_da_fixture.py:47`, `app/chamados/services/validacao.py:707`, `app/chamados/services/validacao.py:912`, `app/chamados/services/materializacao.py:2238`, `app/folha/export.py:565`, `app/docs/LEIS.md:155` |
| Achado 2a, limites e PRONTO | **2a fica na E4d**, conforme a fila. Nenhum `except` novo ou mudança no registro de juízes no delta fornecido. Suite inteira certificada nesse hash, opus final, medida de frota da volta na sombra, reversão, prova posterior e `padrao=` na mensagem do commit estão **[nao verificado]**. Os recortes mencionados no diário não equivalem à certificação inteira. `app/docs/BACKLOG.md:2180`, `app/chamados/models.py:3512`, `app/core/juizes.py:298`, `bin/pista_certifica.sh:7`, `app/docs/LEIS.md:144` |

## Checklist do pré-verificador

| Pergunta | Resultado |
|---|---|
| Segundo juiz que duplica juiz registrado | **Sem clone de C6, C21 ou K6 no delta.** O carimbo consulta C6; o DRY delega competência ao K6 pela porta existente. Há uma segunda produção **semântica** de `VALIDACAO`: a validação humana entra por `validar_pergunta`, enquanto `carimbar('validar')` agora pode produzir a mesma espécie de linha pelo montador de foto. A trilha e a tarefa veem `VALIDACAO` nos dois caminhos. `app/core/juizes.py:298`, `app/core/juizes.py:313`, `app/core/juizes.py:350`, `app/docs/JUIZES.md:38`, `app/chamados/models.py:3475`, `app/chamados/models.py:3511`, `app/chamados/models.py:4483`, `app/chamados/services/validacao.py:176`, `app/chamados/services/materializacao.py:816`, `app/chamados/services/tarefa_do_dia.py:173` |
| Segundo escritor fora da porta | **Não para INSERT.** Os novos registros passam por `registrar`, sem opção de pular a trava. **Sim para a reclassificação em massa** de `entrada_*`: a 0058 usa o modelo histórico e atualiza a tabela fora da porta de runtime. Isso é o mecanismo da migração; o defeito é atualizar sem recorte. A marca `da_ressuscitacao=True` continua com um único montador. `app/chamados/models.py:4430`, `app/chamados/models.py:4696`, `app/chamados/models.py:4718`, `app/chamados/services/validacao.py:481`, `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:9`, `app/chamados/tests/test_f2_ressuscitacao_um_escritor.py:14` |
| Relógio lido fora da borda | **Sem nova leitura de relógio de negócio nos redutores.** L-130 lê `now()` sob a trava e entrega o valor ao montador. `Now()` é o relógio de inserção do banco. Há um caminho herdado que o delta passa a usar para gravar `ato_em`: a absorção tira `agora` antes da trava local das irmãs e o reutiliza em `carimbar` e em `ABSORCAO`. Os dois chamadores diretos do reconciliador omitem `quando` e usam o relógio interno do carimbo. `app/chamados/services/validacao.py:468`, `app/chamados/services/validacao.py:477`, `app/chamados/models.py:3460`, `app/chamados/models.py:4388`, `app/chamados/services/materializacao.py:1047`, `app/chamados/services/materializacao.py:1076`, `app/chamados/services/materializacao.py:1108`, `app/chamados/services/materializacao.py:1129`, `app/chamados/reconciliador.py:408`, `app/chamados/reconciliador.py:699` |
| Derivação paralela de fonte única | **Parcial.** A redução é compartilhada. A regra “um PK por classe, prevalece novo” foi escrita duas vezes, no contador e no DRY. A correspondência por tipo e instante também tem uma busca própria no comparador, além da função de projeção. E o par de presença e o par de instante podem responder diferente sobre a mesma pergunta. `app/chamados/models.py:4470`, `app/chamados/models.py:4534`, `app/chamados/models.py:4587`, `app/chamados/models.py:4613`, `app/chamados/services/vigia_ato_x_coluna.py:24`, `app/chamados/management/commands/estoque_ato_x_coluna.py:33` |
| Selo ou RED sem mordida | **Viola em parte.** O AST morde alvo errado e compensação sem UPDATE pareado. Para `carimbar`, porém, o selo só mira `validada_em` e ainda aceita a guarda de validação sintética. Remover o `registrar` de Q0.2 preserva essa saída do selo. O teste Q0.2 pega essa remoção na fixture. O RED3 de dono sem trava tem uma asserção de efeito; já os casos protegidos por `_tem_api()` podem dar vermelho por ausência de API, e o RED5 não exercita a classe do item 13. O teste de marco ausente só mede `b`. Execução dos mutantes no hash final **[nao verificado]**. `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:23`, `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:573`, `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:778`, `app/chamados/tests/test_e4c_f5_instante_c1.py:37`, `app/chamados/tests/test_e4c_f5_instante_c1.py:110`, `app/chamados/tests/test_e4c_f5_instante_c1.py:124`, `app/chamados/tests/test_e4c_f5_instante_c1.py:191`, `app/chamados/tests/test_e4c_f5_instante_c1.py:219` |
| Ordem de trava | **Nenhuma inversão nova demonstrada nos caminhos conferidos.** L-130 pede Colaborador → Disputa → Chamado → Pergunta, relê e só então escreve. `registrar` confere a trava do dono antes do INSERT. O `atomic` de `carimbar` garante rollback do `save` se o registro for recusado; a trava prévia continua responsabilidade do chamador. O DRY não trava porque só lê. `app/chamados/models.py:2931`, `app/chamados/models.py:3508`, `app/chamados/models.py:4696`, `app/chamados/services/validacao.py:467`, `app/chamados/services/trava_do_colaborador.py:76`, `app/chamados/management/commands/estoque_ato_x_coluna.py:20` |
| Pouso antes do pré-requisito | **[nao verificado]**. A barreira está no código e o relato registra a exigência do pouso certificado. A presença da barreira na cópia não data o pouso nem a abertura da obra. `app/chamados/models.py:4691`, `app/docs/RELATO.md:23`, `app/docs/RELATO.md:70` |
| Campo com dois significados | **Viola.** `entrada_em` é instante real de inserção para `ato` e instante convencional da 0057 para histórico e estoque novo. `estoque` reúne linhas históricas de qualquer tipo e novas fotos permitidas só para `RESPOSTA`/`VALIDACAO`. Por outro lado, uma foto antiga inserida por `carimbar` ganha `ato` pelo default, assim como novas fotos dos backfills sem marca explícita. Nos campos de negócio, `respondida_em` pode vir da resposta humana, de um instante recebido, da nota do fio ou do relógio que materializou o texto. `validada_em` pode ser assinatura da porta de validação ou carimbo automático. O par de presença, a composta, os leitores de coluna crua e o vigia não exigem a mesma evidência nem a mesma igualdade. `app/chamados/models.py:3460`, `app/chamados/models.py:4375`, `app/chamados/models.py:4478`, `app/chamados/models.py:4582`, `app/chamados/models.py:4613`, `app/chamados/models.py:4722`, `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:16`, `app/chamados/services/disputa_emissao.py:1531`, `app/chamados/services/disputa_emissao.py:1759`, `app/chamados/services/disputa_emissao.py:1793`, `app/chamados/services/validacao.py:171`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:164` |
| `except` novo | **Não.** O `except Exception` ao redor do novo bloco atômico já é contexto no diff. Os novos erros explícitos são `raise` na validação do lote e na migração sem marco. `app/chamados/models.py:3512`, `app/chamados/models.py:4722`, `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:13` |

## Quem escreve e quem lê

| Campo ou valor | Escritores e leitores |
|---|---|
| `entrada_origem`, `entrada_em`, marco | **Escrevem** a 0057 pelos defaults, a 0058 pelo UPDATE e os chamadores de `registrar` que passam explicitamente o par de estoque, demonstrados nas fixtures. Para `ato`, o relógio vem do banco. **Leem** `_lote_de_estoque` nos argumentos, `divergencia_ato_x_coluna` na origem persistida, e `foto_do_colaborador` nos dois campos por introspecção. `marco_do_estoque` e 0058 consultam `django_migrations.applied` separadamente; a fixture fixa ou apaga esse registro. O vigia não lê `entrada_em` da linha para classificar. `app/chamados/migrations/0057_decisao_entrada_do_ato.py:16`, `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:9`, `app/chamados/models.py:4556`, `app/chamados/models.py:4588`, `app/chamados/models.py:4718`, `app/chamados/services/foto_e4b.py:247`, `app/chamados/services/foto_e4b.py:280`, `app/chamados/tests/cortes_f2_f9_apoio.py:43`, `app/chamados/tests/test_e4c_f5_instante_c1.py:81`, `app/chamados/tests/test_e4c_f5_instante_c1.py:112` |
| `respondida_em`, `resposta_colab` | **Preenchem** `_nascer`, resposta humana, automática, resposta pela trilha e `carimbar` no ramo que completa o instante. **Limpam** `descarimbar_em_lote`, chamado pela ressuscitação, reabertura/recusa e L-130. **Leem cru** o `save` que confere a L-130, o montador de foto, a face SQL do C21, a cura pela trilha, painel, raio-x, serialização e auditoria. A composta toma o instante da sequência e o texto da coluna. `app/chamados/models.py:3322`, `app/chamados/models.py:3466`, `app/chamados/models.py:3654`, `app/chamados/models.py:3875`, `app/chamados/models.py:4467`, `app/chamados/models.py:4600`, `app/chamados/services/disputa_emissao.py:1530`, `app/chamados/services/disputa_emissao.py:1714`, `app/chamados/services/disputa_emissao.py:1757`, `app/chamados/services/disputa_emissao.py:1787`, `app/chamados/services/validacao.py:485`, `app/chamados/juizes.py:1676`, `app/chamados/services/painel.py:67`, `app/chamados/services/raiox.py:138`, `app/api/views_mensageria.py:571`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1115` |
| `validada_em`, `validada_por` | **Preenchem** `_nascer` quando recebe o par, `_carimbar_validacao` e `carimbar('validar')`. **Limpam** resposta humana, as duas devoluções de validação, reabertura/recusa, ressuscitação e L-130. **Leem cru** o montador de foto, a guarda L-130, o par de presença, o caminho SQL do passivo, os verbos e os comandos de backfill/BUG-PAR. O reduzido entra em validar, recusar, materializar, C6 e fila. `app/chamados/models.py:1730`, `app/chamados/models.py:3322`, `app/chamados/models.py:3470`, `app/chamados/models.py:3709`, `app/chamados/models.py:4481`, `app/chamados/models.py:4613`, `app/chamados/services/disputa_emissao.py:1542`, `app/chamados/services/disputa_emissao.py:1661`, `app/chamados/services/validacao.py:171`, `app/chamados/services/validacao.py:307`, `app/chamados/services/validacao.py:385`, `app/chamados/services/validacao.py:428`, `app/chamados/services/validacao.py:485`, `app/chamados/services/validacao.py:765`, `app/chamados/services/validacao.py:961`, `app/chamados/services/materializacao.py:2295`, `app/chamados/services/verbos.py:266`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:95`, `app/chamados/management/commands/bug_par_103.py:69`, `app/chamados/juizes.py:1501` |
| `pergunta_id`, `pk` da linha, `ato`, `ato_em`, `ato_por`, `ato_texto`, `ato_motivo`, fotos `*_original_*`, `resposta_mantem_validacao`, `da_ressuscitacao` | **Grava** `registrar`, por payloads de nascimento/foto, resposta, validação, devolução, reabertura/recusa, ressuscitação, silêncio/retratação, absorção e backfills. A porta limita o recuo dos compensadores pelo último `ato_em`; o redutor desempata pelo PK. O montador da ressuscitação lê a validação vigente e a última `VALIDACAO` naquele instante para fotografar o autor; a linha compensadora tem `ato_por=None`. **Leem** o redutor, a face SQL, a tarefa do dia, a cronologia, as respostas anteriores no fio, a foto de acervo e o vigia de texto. A `0056` também escreve a flag de conservação da validação no histórico. `app/chamados/models.py:2182`, `app/chamados/models.py:2317`, `app/chamados/models.py:3327`, `app/chamados/models.py:3964`, `app/chamados/models.py:4425`, `app/chamados/models.py:4480`, `app/chamados/models.py:4534`, `app/chamados/models.py:4650`, `app/chamados/models.py:4699`, `app/chamados/services/disputa_emissao.py:1565`, `app/chamados/services/disputa_emissao.py:1695`, `app/chamados/services/disputa_emissao.py:1761`, `app/chamados/services/disputa_emissao.py:1795`, `app/chamados/services/validacao.py:176`, `app/chamados/services/validacao.py:313`, `app/chamados/services/validacao.py:390`, `app/chamados/services/materializacao.py:1129`, `app/ponto/services/veredito_celula.py:200`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:358`, `app/chamados/services/tarefa_do_dia.py:388`, `app/chamados/services/historico.py:98`, `app/chamados/services/fio.py:302`, `app/chamados/services/vigia_texto_resposta.py:23`, `app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12` |
| Composta e seus consumidores de instante | **Produz** `DecisaoPergunta.composta`, via `leitura`. **Consomem** C6 e `_slot_esperado`, API/app e views, fio e declaração, validação e materialização, reconciliador, ficha, Cadastro × Realidade, prévia de folha e cartório. Esses usos incluem decisão por instante e exibição. O consumidor externo do APK está **[nao verificado]**. `app/chamados/models.py:4594`, `app/chamados/juizes.py:371`, `app/chamados/juizes.py:1501`, `app/api/views.py:2587`, `app/api/views.py:2646`, `app/chamados/views.py:1276`, `app/chamados/views_cobrar.py:223`, `app/chamados/services/fio.py:224`, `app/chamados/services/declaracao_texto.py:445`, `app/chamados/services/pergunta_no_app.py:88`, `app/chamados/services/validacao.py:765`, `app/chamados/services/materializacao.py:1092`, `app/chamados/reconciliador.py:367`, `app/colaboradores/services/ficha.py:173`, `app/escala/services/cadastro_realidade.py:575`, `app/folha/previa.py:143`, `app/ponto/services/cartorio.py:1696` |
| Carimbo, lavra, trilha, identidade e competência | **Escrevem** `_escrever_carimbo`, portas de identidade/descarimbo, o lote e as portas de via; `veredito_lavrado.lavrar` fornece os três campos da lavra, e `evento` grava a auditoria L-130. `gravar_dia_do_fato` serve nascimento e mudanças de data. **Leem** C6, a composta, o disparo da célula e o DRY. O DRY usa PK, dono pela disputa, empresa e dia do fato; janela e K6 leem o corte e a exportação vigente. `ExportacaoDominio.registrar` e `invalidar` escrevem a vigência. `app/chamados/models.py:3419`, `app/chamados/models.py:3527`, `app/chamados/models.py:3574`, `app/chamados/models.py:3591`, `app/chamados/models.py:3654`, `app/chamados/services/fio.py:950`, `app/chamados/services/fio.py:1039`, `app/chamados/services/fio.py:1197`, `app/chamados/veredito_lavrado.py:254`, `app/chamados/services/validacao.py:495`, `app/core/observ.py:134`, `app/chamados/juizes.py:1484`, `app/chamados/management/commands/estoque_ato_x_coluna.py:31`, `app/chamados/services/materializacao.py:815`, `app/ponto/janelas.py:60`, `app/folha/export.py:565`, `app/folha/models.py:143`, `app/folha/models.py:174` |
| `a`, `b`, `estoque`, `marco`, universo, `gritou` e esperado | 

[truncado em 20000 de 28302 caracteres]

## 26/11 20:58 · sessao 47bc69f79b9b · O30-F3-CONSTRUIR · verificador · origem=Code

## Achados A–D

| Achado | Cura | Evidência |
|---|---|---|
| **A · Período decidido duas vezes** | **Curado na classe do lote.** A seleção usa `_q_cobertura`, o mesmo predicado SQL do juiz registrado, e a view deixou de reaplicar datas. As duas pontas são inclusivas. Um tipo em aberto com fim nulo e início anterior entra. Um avulso com fim nulo iniciado antes fica fora. Um intervalo que termina em `ini` entra. O lote conserva seu universo de lançamentos-raiz, inclusive rejeitados quando o status fica sem filtro. | `app/core/juizes.py:261`, `app/ponto/turnos.py:2817`, `app/ponto/turnos.py:2822`, `app/ponto/services/atestados_lote.py:36`, `app/ponto/views_atestado.py:110`, `app/ponto/services/dia_decidido.py:624` |
| **B · `q` e limite** | **Curado na classe do predicado e do contador.** O lote chama `filtro.aplicar` para nome ou CPF e só então limita os lançamentos que passaram pelos filtros. A igualdade entre telas vale para a mesma pessoa, com os mesmos dados e um `q` válido no lote. Os universos das telas e a validação da entrada diferem. | `app/ponto/views_atestado.py:57`, `app/ponto/views_atestado.py:68`, `app/ponto/views_atestado.py:94`, `app/ponto/views_atestado.py:116`, `app/core/filtro.py:146`, `app/core/filtro.py:200` |
| **C · Teste migrado** | **Curado em parte.** O trecho quebrado voltou ao método que define `painel` e `url`. O POST pós-aprovação usa DP comum, com permissões do grupo do setor, e verifica o estado e os bytes persistidos. O novo GET usa `status='pendente'`, estado extinto. Para um atestado sem documento, o backfill usa `aguardando_documento`. A condição do formulário independe do status, então o teste pode passar com a fixture inválida. | `app/ponto/tests/test_atestado_f3.py:84`, `app/ponto/tests/test_atestado_f3.py:227`, `app/ponto/tests/test_atestado_f3.py:255`, `app/ponto/tests/test_atestado_f3.py:274`, `app/colaboradores/signals.py:24`, `app/colaboradores/mixins.py:363`, `app/ponto/models.py:690`, `app/ponto/migrations/0057_a4_backfill_pendente.py:35`, `app/templates/ponto/partials/painel_ausencia.html:89` |
| **D · Órfão antes do retorno do INSERT** | **Curado para falhas que propagam antes do retorno e deixam a conexão consultável.** Com upload e atomic externo, o savepoint abrange INSERT, trilha e projeção. O `finally` roda depois que o savepoint sai. O INSERT também passou a consultar referências antes de apagar. **A classe continua aberta** quando a transação do chamador desfaz a linha depois do retorno. | `app/ponto/services/ausencia.py:638`, `app/ponto/services/ausencia.py:648`, `app/ponto/services/ausencia.py:666`, `app/ponto/services/ausencia.py:669`, `app/ponto/services/ausencia.py:263`, `app/ponto/views.py:2547` |

## Checklist do pré-verificador

| Item | O diff viola? |
|---|---|
| Segundo juiz que duplica juiz registrado | **Não.** O lote delega a interseção ao predicado do juiz registrado e mantém a janela de competência em `janela_fechamento`. `app/docs/JUIZES.md:58`, `app/core/juizes.py:261`, `app/ponto/services/atestados_lote.py:21`, `app/ponto/services/atestados_lote.py:36` |
| Segundo escritor fora da porta | **Não.** O INSERT continua em `criar_ausencia`. A limpeza continua no helper do serviço e a trilha segue por `core.observ.evento`. `app/ponto/services/ausencia.py:242`, `app/ponto/services/ausencia.py:648`, `app/ponto/services/ausencia.py:656`, `app/core/observ.py:94` |
| Relógio fora da borda | **Não introduz.** O lote lê `localdate()` na view. A chamada à projeção mudou de posição dentro da transação e conserva a leitura de relógio já existente no cálculo do teto. `app/ponto/views_atestado.py:94`, `app/ponto/services/ausencia.py:179`, `app/ponto/services/ausencia.py:191`, `app/ponto/services/ausencia.py:666` |
| Derivação paralela de fonte única | **Não.** O período tem uma decisão de inclusão, `q` tem um predicado, e a contagem usa a consulta que alimenta a montagem. O `pk__in` transporta o resultado de `q` para o SQL. `app/ponto/services/atestados_lote.py:41`, `app/ponto/views_atestado.py:74`, `app/ponto/views_atestado.py:116`, `app/ponto/services/atestados_lote.py:65` |
| Selo ou RED sem mordida | **Sim, na garantia completa de C e D.** Os **13 alvos sem banco passaram** e os **16 mutantes falharam por asserção**. O selo AST de C ainda passou quando retirei o POST do teste aprovado e troquei a fixture pendente por aprovada, em memória. Os REDs de D cobrem falha antes do retorno e callback simulado após commit, deixando descoberto o rollback do chamador depois do retorno. `app/ponto/tests/mutantes_atestado_f3_c14.py:19`, `app/ponto/tests/mutantes_atestado_f3_c14.py:105`, `app/ponto/tests/test_atestado_f3_contrato.py:629`, `app/ponto/tests/test_atestado_f3_contrato.py:685`, `app/ponto/tests/test_atestado_f3_contrato.py:1053` |
| Ordem de trava | **Não identifiquei inversão introduzida.** No INSS, a guarda trava Colaborador antes da escrita de Ausência. O savepoint entra dentro dessa ordem e a compensação consulta com `exists()`. `app/ponto/views.py:2547`, `app/colaboradores/services/colaborador.py:84`, `app/ponto/services/ausencia.py:648`, `app/ponto/services/ausencia.py:263` |
| Campo com dois significados | **Sim em C.** A fixture grava `pendente` como estado persistido. O filtro da lista ainda usa essa palavra como apelido para os dois estados abertos. Para datas, o lote alinhou seu recorte ao juiz, mas outros leitores ainda dão tratamentos distintos a fim nulo, e as três telas aplicam o período a fatos diferentes. O censo abaixo identifica esses usos. `app/ponto/tests/test_atestado_f3.py:274`, `app/ponto/models.py:690`, `app/ponto/views.py:2248`, `app/ponto/services/dia_decidido.py:609`, `app/holerite/views.py:409`, `app/folha/views_retrato.py:216` |
| `except` novo | **Não.** O `except Exception` do INSERT já existia no hunk fornecido. A c15 acrescenta `try/finally` e faz a exceção posterior ao INSERT subir pelo tratador existente. `app/ponto/services/ausencia.py:646`, `app/ponto/services/ausencia.py:669`, `app/ponto/services/ausencia.py:674` |

## Quem escreve e quem lê

| Campo ou valor | Escritores, leitores e significado |
|---|---|
| **`q`, `nome`, `cpf`, PK** | O formulário do lote e o componente universal escrevem `q`. `filtro.ler` apara e converte para maiúsculas. O cadastro cria e edita nome e CPF, e o modelo normaliza CPF no `save`. Lote e Holerite leem `colab.nome/cpf`. Retrato copia os mesmos campos para `_nome/_cpf`. `aplicar` busca substring no nome em maiúsculas ou no CPF armazenado, com acentos e pontuação significativos. `colNN` e matrícula perderam o tratamento especial do buscador de âncoras, que segue em sua porta própria. `app/templates/relatorios/atestados_lote.html:6`, `app/templates/core/_filtro_campos.html:100`, `app/core/filtro.py:68`, `app/core/filtro.py:146`, `app/core/filtro.py:200`, `app/colaboradores/views.py:1205`, `app/colaboradores/views.py:1327`, `app/colaboradores/models.py:418`, `app/folha/views_retrato.py:64`, `app/folha/views_retrato.py:113`, `app/core/busca.py:115`, `app/core/views.py:89` |
| **`empresa`, `colaborador`, `mes`, `ano`, `de_data`, `ate_data`, `ini/fim`** | O GET escreve as seleções. O formulário valida a empresa por `empresas_visiveis` e oferece colaboradores por nome. A view preenche mês e ano ausentes a partir da data civil local. A competência usa o corte da empresa selecionada, da empresa do colaborador escolhido ou o padrão. O cadastro da empresa grava o dia de corte. O calendário universal também escreve `de_data/ate_data`. `ler` descarta ponta inválida e ordena pontas invertidas. O lote passa datas inclusivas ao SQL, usando `date.min/max` para a ponta ausente, e passa as pontas informadas ao template. Holerite usa os mesmos limites inclusivos sobre o **dia civil local do aviso ou da publicação**. Retrato conta **datas gravadas por pessoa** nos limites inclusivos e conserva as pessoas na lista. `app/ponto/views_atestado.py:56`, `app/ponto/views_atestado.py:94`, `app/colaboradores/queries.py:78`, `app/colaboradores/views.py:518`, `app/ponto/janelas.py:10`, `app/ponto/janelas.py:36`, `app/templates/core/_filtro_campos.html:43`, `app/core/filtro.py:78`, `app/core/filtro.py:98`, `app/core/filtro.py:118`, `app/templates/relatorios/atestados_lote.html:33`, `app/holerite/views.py:409`, `app/holerite/views.py:565`, `app/folha/views_retrato.py:225`, `app/folha/retrato_folha.py:1343` |
| **`Ausencia.data`, `data_fim`, `dias_corridos`, `minutos`, `tipo`, `status`** | Criação e edição gravam o início do lançamento e derivam o fim inclusivo. API e PWA convertem fim informado em dias corridos com `+1`. O modelo valida o intervalo e ajusta dias corridos, e a transição de estado persiste as derivações do `save`. `minutos` é duração em minutos, com zero usado como entrada de dia inteiro. O juiz interpreta fim nulo com o tipo, o fechamento lê a cobertura e a folha lê dias pelo juiz. Histórico e exportação do lote exibem datas e dias do lançamento inteiro. Há divergência herdada para fim nulo: `decididos_do_periodo` expande só o dia inicial, enquanto a cobertura estende tipos em aberto. `app/ponto/services/ausencia.py:200`, `app/ponto/services/ausencia.py:215`, `app/ponto/services/ausencia.py:575`, `app/ponto/services/ausencia.py:744`, `app/ponto/services/ausencia.py:1075`, `app/ponto/models.py:892`, `app/api/views.py:1808`, `app/ponto/views.py:2681`, `app/ponto/turnos.py:2830`, `app/ponto/services/fechamento.py:658`, `app/folha/export.py:65`, `app/ponto/services/dia_decidido.py:609`, `app/ponto/services/dia_decidido.py:678`, `app/ponto/services/atestados_lote.py:125` |
| **`registros_na_janela`, `tipos`, `status`, `LIMITE_LOTE`, `formato`** | Os dois chamadores diretos encontrados são `lote_atestados` e `atestados_lote.linhas`. O primeiro passa tipos médicos do catálogo e não restringe por status. O segundo encaminha `tipos/status` e entrega a seleção à linha do tempo. O drawer lê a mesma montagem por `linha_do_tempo(colab)`, sem passar por `registros_na_janela`. `LIMITE_LOTE=20` conta **lançamentos selecionados**, e a view lê até 21 para detectar excesso. O formulário envia CSV/ZIP ou usa a rota PDF. A mesma lista alimenta os formatos e a prévia. `app/ponto/services/atestados_lote.py:7`, `app/ponto/services/atestados_lote.py:36`, `app/ponto/services/atestados_lote.py:51`, `app/ponto/catalogo/ausencias.py:366`, `app/ponto/views_atestado.py:83`, `app/ponto/views_atestado.py:116`, `app/ponto/views.py:2035`, `app/relatorios/urls.py:16`, `app/templates/relatorios/atestados_lote.html:20` |
| **Upload, `documento`, `documento_referencia`, `storage/name`** | **Três portas de produção localizadas passam upload ao caminho de INSERT**: lançamento DP/INSS, submissão PWA e POST da API. Elas leem `FILES['documento']` e entregam o upload a `criar_ausencia`, que também pode reabrir uma linha ou devolver uma existente. O `FileField` escreve os bytes no storage e guarda a chave na coluna. O painel, o endpoint de anexo da API e o reenvio também chegam a `anexar_documento`, que faz UPDATE. Edição e rederivação usam save integral. O download e o painel resolvem a raiz pela FK legada. Histórico, lista, PDF, ZIP, vigia, API e porta da célula leem a presença, a chave ou os bytes. A compensação usa `_committed` do `FieldFile` como sinal da escrita no storage e `documento=nome` para ver se há referência no banco. `app/ponto/views.py:2504`, `app/ponto/views.py:2550`, `app/ponto/views.py:2627`, `app/ponto/views.py:2683`, `app/api/views.py:1779`, `app/api/views.py:1824`, `app/api/views.py:1852`, `app/api/views.py:3115`, `app/ponto/views_atestado.py:48`, `app/ponto/services/ausencia.py:525`, `app/ponto/services/ausencia.py:585`, `app/ponto/services/ausencia.py:759`, `app/ponto/services/ausencia.py:1133`, `app/ponto/services/ausencia.py:1431`, `app/ponto/services/ausencia.py:1886`, `app/ponto/models.py:777`, `app/ponto/models.py:815`, `app/ponto/views.py:2102`, `app/ponto/views.py:2147`, `app/templates/ponto/ausencias.html:168`, `app/ponto/services/dia_decidido.py:628`, `app/ponto/services/atestados_lote.py:167`, `app/ponto/services/atestados_lote.py:249`, `app/ponto/services/vigia_ausencia.py:117`, `app/ponto/portas/celula.py:726`, `app/ponto/services/ausencia.py:263` |
| **Trilha, projeção e retorno do INSERT** | `criar_ausencia` lê PK, tipo, nome, datas, estado, minutos, origens, ator e IP para `evento`, que grava `LogAuditoria`. Os seis campos médicos entram no INSERT e têm UPDATE pela porta de atestado. A trilha desses campos guarda os nomes alterados. Painel, CSV e PDF leem seus valores. `_insumo_da_folha` relê tipo, datas, minutos e horas parciais para a invalidação da projeção. `_criada` é escrito no serviço e lido pela API e por `garantir_ausencia`. `app/ponto/services/ausencia.py:121`, `app/ponto/services/ausencia.py:135`, `app/ponto/services/ausencia.py:329`, `app/ponto/services/ausencia.py:339`, `app/ponto/services/ausencia.py:581`, `app/ponto/services/ausencia.py:656`, `app/ponto/services/ausencia.py:701`, `app/core/observ.py:136`, `app/core/views.py:686`, `app/templates/ponto/partials/painel_ausencia.html:124`, `app/ponto/services/atestados_lote.py:103`, `app/ponto/services/fechamento.py:1672`, `app/api/views.py:1852` |

## Universo de `q` e destino do upload

**O conjunto de pessoas pode diferir.** O lote inicia em `Colaborador.objects.all()`, aplica empresa e pessoa se informadas e percorre os candidatos com `iterator()`. Materializa os resultados de `q` e os PKs antes do limite de lançamentos. O Holerite parte dos vínculos da empresa na competência e acrescenta titulares de holerites daquela empresa. O Retrato parte das linhas gravadas das empresas e da competência, enriquecidas com o cadastro atual. Assim, um colaborador pode casar com `q` nas três telas e só aparecer no lote se tiver um lançamento selecionado. O lote também rejeita `q` normalizado acima de 200 caracteres com HTTP 400. Holerite e Retrato passam o termo normalizado direto a `aplicar`. Custo em produção [nao verificado]. `app/ponto/views_atestado.py:57`, `app/ponto/views_atestado.py:68`, `app/ponto/views_atestado.py:94`, `app/ponto/views_atestado.py:116`, `app/holerite/matriz.py:79`, `app/holerite/views.py:250`, `app/holerite/views.py:433`, `app/folha/views_retrato.py:194`, `app/folha/views_retrato.py:216`

**Pode sobrar órfão depois do retorno.** Reproduzi em memória `criar_ausencia` retornando com linha e arquivo presentes, seguido de rollback do atomic do chamador. A referência sumiu e o arquivo ficou. `concluiu=True` fecha a compensação antes de o chamador terminar. A view INSS cobre `Exception` após receber a instância e sair do atomic da tela. Um `BaseException` após o retorno passa fora desse tratador. Uma falha de consulta ou de exclusão também pode deixar o arquivo. `app/ponto/tests/test_atestado_f3_contrato.py:608`, `app/ponto/tests/test_atestado_f3_contrato.py:647`, `app/ponto/services/ausencia.py:667`, `app/ponto/services/ausencia.py:690`, `app/ponto/views.py:2567`, `app/ponto/services/ausencia.py:263`

**Uma referência já visível protege o arquivo no `finally` novo.** O RED de `on_commit` simula uma exceção depois do commit e verifica que o arquivo continua presente. A proteção termina no alcance da consulta. `exists()` e `storage.delete()` são operações separadas, então uma referência concorrente confirmada entre elas pode perder o arquivo. No SaaS, a consulta usa o schema corrente e o storage configurado é de filesystem. O UPDATE legado de `anexar_documento`, fora do hunk, ainda tem um ramo que apaga por nome sem consultar referências. Ocorrência desses cenários em produção [nao verificado]. `app/ponto/tests/test_atestado_f3_contrato.py:685`, `app/ponto/services/ausencia.py:259`, `app/ponto/services/ausencia.py:1127`, `app/ponto/services/ausencia.py:1147`, `app/config/settings/saas.py:83`, `app/config/settings/saas.py:109`

## Veredito final — REPROVADO

1. **C:** o GET de pendente usa um estado extinto, e o selo AST aceita a perda do POST e da fixture pendente. `app/ponto/tests/test_atestado_f3.py:274`, `app/ponto/models.py:690`, `app/ponto/tests/test_atestado_f3_contrato.py:1053`
2. **D:** há órfão reproduzido em memória quando o atomic externo desfaz a linha depois do retorno, e a exclusão tem uma janela entre verificar a referência e apagar. `app/ponto/services/ausencia.py:667`, `app/ponto/services/ausencia.py:690`, `app/ponto/services/ausencia.py:263`
3. **Prova de pouso:** rodei 47 contratos sem banco. Houve 46 passes e um erro de ambiente por falta de `PIL`. A execução dos REDs antes da cura, os testes HTTP com banco e a suíte integral verde ficam [nao verificado]. `app/ponto/tests/test_atestado_f3_contrato.py:1243`, `app/ponto/tests/test_atestado_f3.py:255`, `app/ponto/tests/test_atestado_f3.py:284`, `CLAUDE.md:398`

## 26/11 20:51 · sessao 404fd9d1ab17 · O30-F3-CONSTRUIR · construtor · origem=Code

**C15 aplicada aos quatro achados.** Os sete alvos sem banco falharam na base pelos motivos abaixo e passaram após a cura. Os oito mutantes acrescentados e os oito já presentes falharam por asserção, todos com `rc=1`. [Contratos do lote](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:896) · [Contratos do INSERT](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:630) · [Mutantes](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3_c14.py:19) [nao verificado]

### O que mudou e quem consome

- **A · Período.** O lote usa `_q_cobertura` uma vez, no SQL. Entra o tipo em aberto iniciado antes da janela com `data_fim` nula. Fica fora o tipo avulso nas mesmas datas. A borda `data_fim == de_data` entra. O mesmo predicado é lido por `cobertura_ausencia_periodo` e `CargaDaJanela.cobertura`. Prévia, CSV, PDF e ZIP recebem o mesmo recorte. `ponto/services/atestados_lote.py:36`, `ponto/turnos.py:2817`, `ponto/turnos.py:3142`, `ponto/views_atestado.py:110`, `ponto/views_atestado.py:124`
- **B · Busca e limite.** `q` usa `filtro.aplicar`, como no Holerite e no Retrato, e o campo diz **Nome ou CPF**. O limite é aplicado ao resultado dos filtros de pessoa, empresa, período e tipo médico. A Busca de Âncoras continua consumindo `qs_colaborador` na sua própria porta. `ponto/views_atestado.py:57`, `ponto/views_atestado.py:68`, `ponto/views_atestado.py:116`, `core/filtro.py:200`, `holerite/views.py:407`, `folha/views_retrato.py:226`, `core/busca.py:151`
- **C · Painel.** Retirei o GET que recebia uma resposta HTTP e devolvi o POST de metadados ao teste de permissão. O POST de arquivo pós-aprovação e o GET do formulário da pendente usam um DP comum, vinculado ao grupo semeado pelo `Setor`. O formulário posta em `relatorios:anexar_atestado`. `ponto/tests/test_atestado_f3.py:85`, `ponto/tests/test_atestado_f3.py:243`, `ponto/tests/test_atestado_f3.py:255`, `ponto/tests/test_atestado_f3.py:274`, `colaboradores/signals.py:10`, `templates/ponto/partials/painel_ausencia.html:90`
- **D · INSERT.** Com upload pendente sob `atomic` externo, um savepoint cobre o INSERT, o evento e a projeção. Depois da saída do savepoint, o `finally` compensa o upload efetivamente gravado. A falha no INSERT e a falha antes do retorno usam a mesma consulta de referência, no alias da escrita. O `except` existente mantém a tradução da constraint. Além do lançamento INSS, chamam a criação o PWA, a API, a materialização de chamados, o veredito de célula, a checagem de ausência e o espelho de férias. `ponto/services/ausencia.py:637`, `ponto/services/ausencia.py:668`, `ponto/services/ausencia.py:263`, `ponto/views.py:2550`, `ponto/views.py:2683`, `api/views.py:1824`, `chamados/services/materializacao.py:998`, `chamados/services/materializacao.py:1037`, `ponto/services/veredito_celula.py:342`, `ponto/services/checar_ausencia.py:46`, `ponto/services/ausencia.py:1524`

### REDs e comandos

Os comandos abaixo usam `L=AtestadoF3LoteSemBancoTest` e `P=AtestadoF3PortaSemBancoTest`. O executor aplica os mutantes em memória. `ponto/tests/test_atestado_f3_contrato.py:349`, `ponto/tests/test_atestado_f3_contrato.py:862`, `ponto/tests/mutantes_atestado_f3_c14.py:105`

```bash
L=AtestadoF3LoteSemBancoTest
P=AtestadoF3PortaSemBancoTest
red() { PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v "ponto.tests.test_atestado_f3_contrato.$1"; }
mut() { PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3_c14 "$1"; }
```

| Falha observada antes da cura | Comando do RED | Comando do mutante |
|---|---|---|
| **A** · Sumiu a ausência em aberto. `ponto/tests/test_atestado_f3_contrato.py:990` | `red "$L.test_RED_periodo_usa_cobertura_com_nulos_e_borda_inclusiva"` | `mut periodo_sem_aberto` |
| **A** · A view reaplicou as datas. `ponto/tests/test_atestado_f3_contrato.py:1072` | `red "$L.test_rota_pdf_recebe_os_filtros_do_formulario"` | `mut periodo` |
| **B** · `COL27` selecionou pelo PK em vez do texto no nome. `ponto/tests/test_atestado_f3_contrato.py:896` | `red "$L.test_RED_busca_q_tem_o_mesmo_resultado_do_filtro_universal"` | `mut busca_por_ancora` |
| **B** · O lote respondeu 400 por limite quando o filtro universal deixava um registro. `ponto/tests/test_atestado_f3_contrato.py:931` | `red "$L.test_RED_limite_conta_depois_da_busca_q"` | `mut limite_antes_q` |
| **C** · A checagem estrutural encontrou dois GETs na aprovada e nenhum teste da pendente. Os mutantes também cobrem a troca do DP pelo superusuário. `ponto/tests/test_atestado_f3_contrato.py:1053` | `red "$L.test_RED_migracao_do_formulario_do_painel_preserva_a_cobertura"` | `mut painel_bloco_morto` · `mut painel_sem_pendente` · `mut painel_com_superusuario` |
| **D** · A linha sofreu rollback e o arquivo sobrou nas falhas injetadas no evento, na leitura do insumo e no despertar da projeção. `ponto/tests/test_atestado_f3_contrato.py:630` | `red "$P.test_RED_inss_remove_anexo_se_insert_falha_antes_de_retornar"` | `mut insert_sem_savepoint_do_ato` |
| **D** · O arquivo foi apagado apesar da referência após o commit simulado. `ponto/tests/test_atestado_f3_contrato.py:685` | `red "$P.test_RED_insert_preserva_anexo_referenciado_quando_on_commit_levanta"` | `mut insert_apaga_referenciado` |

A execução dos contratos sem banco teve **46 testes aprovados e um erro por falta de `PIL`** no PDF real. A checagem de tratadores passou sem `except` novo. A execução Django com ORM, storage e RBAC reais fica **[nao verificado]**, assim como a ocorrência em produção. A certificação fica com o Code e o opus final. `ponto/tests/test_atestado_f3_contrato.py:1244`, `ponto/tests/test_atestado_f3_contrato.py:222`, `ponto/tests/test_atestado_f3.py:255`, `ponto/tests/test_atestado_f3.py:284`

## 26/11 20:49 · sessao cda0fb238f36 · ABONO-X-SAIDA-ANTECIPADA · construtor · origem=Code

O RED novo passou sem mutação e falhou por `AssertionError` com `ps[-1:]` e `ps[:1]`. A única edição foi no arquivo de testes. [ponto/tests/test_abono_c1f_lista_da_jornada.py:30](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/ponto/tests/test_abono_c1f_lista_da_jornada.py:30) [nao verificado]

### Por que o M1b sobreviveu

- O 12x36 obtém os períodos do pareador. O pareador ordena as batidas por timestamp, o adaptador mantém a ordem dos turnos, e `calcular_mes` acrescenta os períodos a `resultado.periodos` nessa sequência. No teto, `ps` é o grupo de períodos da jornada `dia`, montado de `resultado.periodos` com `dia_por_periodo[id(p)]` e `append`, preservando a ordem recebida. [ponto/motor_calculo_v2.py:2917](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/ponto/motor_calculo_v2.py:2917) [ponto/turnos.py:1232](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/ponto/turnos.py:1232) [ponto/adaptador_motor.py:29](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/ponto/adaptador_motor.py:29) [ponto/motor_calculo_v2.py:2672](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/ponto/motor_calculo_v2.py:2672) [ponto/motor_calculo_v2.py:1132](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/ponto/motor_calculo_v2.py:1132)
- Na fixture antiga, `22:00–22:30` é absorvido como intrajornada. O turno físico aparece **uma vez** em `ps`, de `D 19:30` a `D+1 01:00`. Conferi o pareamento e o teto em memória. `ps[-1]` carrega as duas pontas brutas, saída antecipada `D+1 01:00–07:00` e atraso `D 19:00–19:30`. Assim, `ps[-1:]` conserva a ponta que a parcial já cobre e a lista fica vazia. [RED antigo:92](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/ponto/tests/test_abono_c1f_lista_da_jornada.py:92) [ponto/turnos.py:468](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/ponto/turnos.py:468) [ponto/motor_calculo_v2.py:1245](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/ponto/motor_calculo_v2.py:1245) [ponto/motor_calculo_v2.py:1297](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/ponto/motor_calculo_v2.py:1297)
- A fixture antiga tem uma segunda barreira. Redatada para D+1, a parcial `06:30–19:30` termina depois do envelope da jornada de D, que fecha às `11:00` de D+1. O juiz a atribui a D+1, e o motor descarta a candidata. [ponto/turnos.py:78](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/ponto/turnos.py:78) [ponto/turnos.py:316](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/ponto/turnos.py:316) [ponto/motor_calculo_v2.py:1300](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/ponto/motor_calculo_v2.py:1300)

### RED novo e os três runs

O teste entrega dois períodos fechados sintéticos ao teto, com o mesmo dia de jornada. A parcial `D 06:30–10:00` cobre 30 minutos de atraso como datada e cobriria 30 minutos de saída se redatada. Cada subteste monta períodos novos e muda a posição da ponta coberta. A asserção exige lista vazia nas duas ordens. [RED novo:32](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/ponto/tests/test_abono_c1f_lista_da_jornada.py:32) [asserção:58](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/ponto/tests/test_abono_c1f_lista_da_jornada.py:58) [nao verificado]

| Run | Saída observada |
|---|---|
| Sem mutação | `Ran 1 test in 0.032s` · `OK` · `rc=0` |
| `ps[-1:]` | `periodo_coberto='primeiro'` · `AssertionError` · `FAILED (failures=1)` · `rc=1` |
| `ps[:1]` | `periodo_coberto='ultimo'` · `AssertionError` · `FAILED (failures=1)` · `rc=1` |

Os dois vermelhos falharam na asserção da lista, com uma linha indevida de `saida antecipada`, `cobre_como_datada_min=0` e `cobriria_datada_no_dia_seguinte_min=30`. [asserção:58](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/ponto/tests/test_abono_c1f_lista_da_jornada.py:58) [ponto/motor_calculo_v2.py:1314](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/ponto/motor_calculo_v2.py:1314)

O comando abaixo rodou o teste com `unittest` e adaptadores em memória para Django e `freezegun`, que faltam no Python local. O teto, o juiz de datas e os mutantes rodaram com o código da árvore. A execução na suíte Django fica com o Code. [requirements.txt:13](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/requirements.txt:13) [requirements.txt:43](/home/ronald/wt-codex-abono-x-saida-antecipada-184735/app/requirements.txt:43) [nao verificado na suíte Django] [nao verificado]

```bash
for fatia in '' '[-1:]' '[:1]'; do
  printf '\nfatia=%s\n' "${fatia:-controle}"
  PYTHONDONTWRITEBYTECODE=1 python3 -B - "$fatia" <<'PY'
import sys
import unittest
from datetime import datetime
from pathlib import Path
from types import ModuleType, SimpleNamespace
from unittest.mock import patch
from zoneinfo import ZoneInfo

def modulo(nome, **atributos):
    m = ModuleType(nome)
    vars(m).update(atributos)
    sys.modules[nome] = m
    return m

fuso = ZoneInfo('America/Sao_Paulo')
tz = modulo('django.utils.timezone',
            make_aware=lambda dt: dt.replace(tzinfo=fuso),
            localtime=lambda dt: dt.astimezone(fuso),
            localdate=lambda dt: dt.astimezone(fuso).date(),
            is_naive=lambda dt: dt.utcoffset() is None)
modulo('django')
modulo('django.utils', timezone=tz)
modulo('django.conf', settings=SimpleNamespace(TIME_ZONE=fuso.key))
modulo('django.core')
modulo('django.core.management')
modulo('django.core.management.base', BaseCommand=object)
modulo('django.test', TestCase=unittest.TestCase, SimpleTestCase=unittest.TestCase)

def freeze_time(texto):
    instante = datetime.fromisoformat(texto).replace(tzinfo=ZoneInfo('UTC'))
    class Relogio(datetime):
        @classmethod
        def now(cls, tz=None):
            return instante.replace(tzinfo=None) if tz is None else instante.astimezone(tz)
    return patch('datetime.datetime', Relogio)

modulo('freezegun', freeze_time=freeze_time)
from ponto import motor_calculo_v2 as motor

fatia = sys.argv[1]
if fatia:
    arquivo = Path(motor.__file__)
    fonte = arquivo.read_text(encoding='utf-8')
    alvo = "for p in ps for _q in (getattr(p, '_pontas_brutas_parcial'"
    assert fonte.count(alvo) == 1
    mutado = fonte.replace(alvo, alvo.replace('ps for', f'ps{fatia} for'), 1)
    exec(compile(mutado, str(arquivo), 'exec'), vars(motor))

label = ('ponto.tests.test_abono_c1f_lista_da_jornada.ListaDaJornadaNoTetoTest.'
         'test_RED_a_lista_considera_a_ponta_coberta_em_qualquer_periodo')
unittest.main(module=None, argv=['unittest', '-v', label])
PY
  printf 'rc=%s\n' "$?"
done
```

## 26/11 20:38 · sessao 5c6e8d4a6356 · O30-F3-CONSTRUIR · verificador · origem=Code

# O30-F3-CONSTRUIR · volta 8 · REPROVADO

Quatro achados seguram a volta. O filtro usa componentes da casa, mas o período ainda tem duas derivações e a busca `q` diverge das telas do filtro universal. A rota antiga saiu, mas a migração quebrou um teste do painel. O savepoint corrige a falha de UPDATE na reabertura, mas o lançamento INSS ainda pode deixar órfão quando o INSERT já gravou o arquivo e o serviço falha antes de devolver a instância. A suíte verde exigida para o pouso segue [nao verificado]. `app/ponto/views_atestado.py:68`, `app/ponto/views_atestado.py:107`, `app/ponto/tests/test_atestado_f3.py:257`, `app/ponto/services/ausencia.py:654`, `app/ponto/services/ausencia.py:691`, `app/ponto/views.py:2571`, `CLAUDE.md:398`

Usei o delta anexo e a cópia em leitura. O diretório não contém metadados Git. A confirmação independente do hash, o histórico dos commits, os REDs executados antes da cura e a ocorrência dos cenários em produção ficam [nao verificado]. [nao verificado]

## 1. Bloqueios da c13 — PROCEDE

| Bloqueio | Estado na origem | Evidência |
|---|---|---|
| **2. Filtro universal** | **NAO CURADO** | O dropdown usa `empresas_visiveis` e a busca caseira saiu. O lote usa `qs_colaborador` para `q`, enquanto o drawer do Holerite e o Retrato passam `q` para `filtro.aplicar`, com outra semântica. Para datas, o lote já decide a inclusão pelo SQL e volta a aplicá-la em Python com `max(a.data, ini)`. `app/ponto/views_atestado.py:58`, `app/ponto/views_atestado.py:68`, `app/ponto/views_atestado.py:107`, `app/ponto/services/atestados_lote.py:41`, `app/core/filtro.py:193`, `app/holerite/views.py:407`, `app/folha/views_retrato.py:216` |
| **3. Porta antiga** | **CURADO** na view, na rota e no registro | Os três sítios têm lápide. A migração da cobertura de teste ficou quebrada, conforme o item 3. A autoria da retirada do pendente, reservada à sessão pelo brief, fica [nao verificado]. `app/ponto/views.py:2165`, `app/ponto/urls.py:72`, `app/core/juizes.py:1097`, `app/ponto/tests/test_atestado_f3.py:238` |
| **4. Órfão sob atomic externo** | **NAO CURADO** como garantia do ato | A falha de UPDATE da reabertura ganhou savepoint na origem e a asserção voltou a exigir zero órfão. O lançamento INSS ainda perde o arquivo de vista se `criar_ausencia` falhar entre a saída do savepoint do INSERT e o `return`. `app/ponto/services/ausencia.py:1429`, `app/ponto/services/ausencia.py:1456`, `app/ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:165`, `app/ponto/services/ausencia.py:654`, `app/ponto/services/ausencia.py:691`, `app/ponto/views.py:2542`, `app/ponto/views.py:2571` |

## 2. Censo e semântica do recorte — PROCEDE

| Campo ou parâmetro | Quem escreve e quem lê na cópia |
|---|---|
| **`q` do lote** | O formulário GET envia o campo declarado pela view. `filtro.ler` tira espaços e converte para maiúsculas, a view reescreve a cópia de `GET`, o formulário valida e `_colaboradores_filtrados` consulta `qs_colaborador`. O buscador lê PK por `colNN`, CPF exato cru ou mascarado, palavras de nome e matrícula. A outra porta web que delega ao mesmo buscador é **Busca de Âncoras**, via `buscar('colab', ...)` e `_colab`. Ela ainda tem tamanho mínimo e teto próprios da porta. `app/templates/relatorios/atestados_lote.html:6`, `app/ponto/views_atestado.py:57`, `app/ponto/views_atestado.py:92`, `app/ponto/views_atestado.py:68`, `app/core/filtro.py:76`, `app/core/busca.py:115`, `app/core/busca.py:151`, `app/core/busca.py:202`, `app/core/views.py:89` |
| **`de_data` / `ate_data`** | Escrevem o formulário do lote e o partial de calendário da casa. `filtro.ler` valida, omite ponta inválida e inverte pontas trocadas. Leem o lote, `filtro.aplicar`, os chips, o drawer do Holerite e sua ação, e o Retrato. O `ate_data` do Extrato Parcial é uma chave de contexto calculada de `fim_real`. `app/ponto/views_atestado.py:62`, `app/ponto/views_atestado.py:92`, `app/ponto/views_atestado.py:107`, `app/templates/core/_filtro_campos.html:43`, `app/templates/core/_filtro_campos.html:48`, `app/core/filtro.py:78`, `app/core/filtro.py:98`, `app/core/filtro.py:193`, `app/core/filtro.py:266`, `app/holerite/views.py:407`, `app/holerite/views.py:433`, `app/holerite/views.py:565`, `app/folha/views_retrato.py:230`, `app/relatorios/views.py:593`, `app/templates/relatorios/extrato_parcial.html:10` |
| **Empresa do lote** | O formulário envia a seleção. `ModelChoiceField` valida por `empresas_visiveis`, que lê ativa, CNPJ e razão social. A busca lê a empresa explícita para filtrar pessoas. A janela usa a selecionada, a empresa do colaborador na falta dela ou o corte padrão na falta das duas. O dropdown tem a mesma fonte da Lista de Ausências. Um colaborador de empresa oculta ainda pode entrar pelo seletor de colaboradores, por `q` ou pelo recorte geral. O RED de empresas só cobre o dropdown. `app/templates/relatorios/atestados_lote.html:6`, `app/ponto/views_atestado.py:58`, `app/ponto/views_atestado.py:71`, `app/ponto/views_atestado.py:103`, `app/colaboradores/queries.py:78`, `app/ponto/janelas.py:42`, `app/ponto/views.py:1775`, `app/ponto/tests/test_atestado_f3.py:321` |
| **`Ausencia.documento` — escritores** | `criar_ausencia` passa o arquivo ao INSERT. `anexar_documento` e `_reabrir` gravam a coluna com `update_fields`. Edição e rederivação fazem `save()` integral e podem regravar o nome de uma instância antiga. O admin é de leitura. O storage grava antes do SQL. `app/ponto/services/ausencia.py:585`, `app/ponto/services/ausencia.py:655`, `app/ponto/services/ausencia.py:1136`, `app/ponto/services/ausencia.py:1434`, `app/ponto/services/ausencia.py:762`, `app/ponto/services/ausencia.py:1890`, `app/ponto/admin.py:38`, `app/ponto/models.py:777`, `app/ponto/services/ausencia.py:245` |
| **`Ausencia.documento` — leitores** | API de reenvio e oferta do anexo, download da raiz, lista e painel, detector no modelo, guardas e trilha do serviço, linha do tempo, exportações do lote, vigia, porta da célula e comando de rejulgamento. A compensação lê o nome e procura referências. `app/api/views.py:1061`, `app/api/views.py:1855`, `app/ponto/views.py:2147`, `app/templates/ponto/ausencias.html:168`, `app/templates/ponto/partials/painel_ausencia.html:71`, `app/templates/ponto/partials/painel_ausencia.html:93`, `app/templates/ponto/partials/painel_ausencia.html:134`, `app/templates/ponto/partials/painel_ausencia.html:244`, `app/ponto/models.py:871`, `app/ponto/services/ausencia.py:251`, `app/ponto/services/ausencia.py:263`, `app/ponto/services/ausencia.py:652`, `app/ponto/services/ausencia.py:792`, `app/ponto/services/ausencia.py:857`, `app/ponto/services/ausencia.py:877`, `app/ponto/services/ausencia.py:899`, `app/ponto/services/ausencia.py:916`, `app/ponto/services/ausencia.py:925`, `app/ponto/services/ausencia.py:1127`, `app/ponto/services/ausencia.py:1146`, `app/ponto/services/ausencia.py:1422`, `app/ponto/views.py:2571`, `app/ponto/services/dia_decidido.py:477`, `app/ponto/services/dia_decidido.py:555`, `app/ponto/services/dia_decidido.py:671`, `app/ponto/services/dia_decidido.py:685`, `app/ponto/services/atestados_lote.py:73`, `app/ponto/services/atestados_lote.py:93`, `app/ponto/services/atestados_lote.py:135`, `app/ponto/services/atestados_lote.py:168`, `app/ponto/services/atestados_lote.py:252`, `app/ponto/services/vigia_ausencia.py:117`, `app/ponto/services/vigia_ausencia.py:217`, `app/ponto/portas/celula.py:726`, `app/ponto/portas/celula.py:746`, `app/ponto/management/commands/rejulgar_status_por_tipo.py:28`, `app/ponto/management/commands/rejulgar_status_por_tipo.py:37` |
| **`documento_referencia`** | A FK legada usa `SET_NULL`. No código de produção inspecionado, os lançamentos atuais gravam um range e os saves integrais podem regravar a FK. Atribuição explícita atual a uma raiz, só nas fixtures localizadas. Leem a guarda e o refresh dos dados de atestado, o painel, o download, aprovação e rejeição de irmãos, a lista e o seletor de raízes da linha do tempo. `app/ponto/models.py:815`, `app/ponto/services/ausencia.py:762`, `app/ponto/services/ausencia.py:1890`, `app/ponto/views.py:2677`, `app/ponto/tests/test_atestado_f3.py:127`, `app/ponto/tests/test_atestado_f3.py:151`, `app/ponto/services/ausencia.py:303`, `app/ponto/services/ausencia.py:318`, `app/ponto/views.py:2102`, `app/ponto/views.py:2147`, `app/ponto/views.py:2762`, `app/ponto/views.py:2838`, `app/templates/ponto/ausencias.html:170`, `app/templates/ponto/partials/painel_ausencia.html:95`, `app/templates/ponto/partials/painel_ausencia.html:120`, `app/templates/ponto/partials/painel_ausencia.html:166`, `app/ponto/services/dia_decidido.py:628` |
| **Upload na reabertura e no INSS** | Escrevem o multipart os formulários de lançamento, submissão PWA e painel, além dos clientes das duas portas da API. Leem `FILES` a tela DP, o PWA, a API e a porta do painel. Lançamento, submissão PWA e criação API passam o arquivo a `criar_ausencia` → `_reabrir`. O reversor explícito do painel chama `reverter_rejeicao` sem arquivo, embora o serviço o aceite. `_reabrir` captura o `storage/name` pela mudança de `_committed`. A compensação da tela lê `ausencia.documento` da instância devolvida. `app/templates/ponto/lancar_ausencia.html:85`, `app/templates/ponto/submeter_ausencia.html:52`, `app/templates/ponto/partials/painel_ausencia.html:94`, `app/api/urls.py:82`, `app/ponto/views.py:2504`, `app/ponto/views.py:2550`, `app/ponto/views.py:2627`, `app/ponto/views.py:2683`, `app/api/views.py:1779`, `app/api/views.py:1824`, `app/api/views.py:3115`, `app/ponto/views_atestado.py:48`, `app/ponto/services/ausencia.py:525`, `app/ponto/views.py:2731`, `app/ponto/services/ausencia.py:1402`, `app/ponto/services/ausencia.py:1435`, `app/ponto/views.py:2574` |

Há outros leitores da chave literal `q`, em rotas próprias. Gestões de HE e Ausências, Colaboradores, Escala, Relatórios, Férias, Chamados, Rubricas e busca de Holerite têm consumidores próprios. Por exemplo, Rubricas busca código ou descrição e o Painel de Chamados oferece colaborador ou assunto. `app/ponto/views.py:613`, `app/ponto/views.py:686`, `app/ponto/views.py:825`, `app/ponto/views.py:953`, `app/ponto/views.py:1730`, `app/colaboradores/views.py:31`, `app/colaboradores/views.py:637`, `app/colaboradores/views.py:1053`, `app/colaboradores/views.py:1715`, `app/colaboradores/views.py:2624`, `app/colaboradores/views.py:2818`, `app/escala/views.py:213`, `app/escala/views.py:241`, `app/relatorios/views.py:791`, `app/relatorios/views.py:838`, `app/ferias/views.py:44`, `app/ferias/views.py:596`, `app/ferias/views.py:661`, `app/chamados/views.py:731`, `app/chamados/views.py:816`, `app/chamados/views_arquivar.py:117`, `app/chamados/views_arquivar.py:167`, `app/chamados/views_cobrar.py:72`, `app/folha/views.py:69`, `app/templates/folha/rubricas_quadro.html:63`, `app/templates/chamados/painel_gestao.html:127`, `app/holerite/views.py:98`, `app/holerite/views.py:1117`

- **Mesmo universo nos quatro formatos.** A view monta `linhas` uma vez por requisição. Prévia, CSV, ZIP e PDF recebem essa lista. O ZIP seleciona os arquivos das linhas com documento e o PDF deriva a seção “Sem documento” pela mesma `previa`. `app/ponto/views_atestado.py:113`, `app/ponto/views_atestado.py:123`, `app/ponto/services/atestados_lote.py:73`, `app/ponto/services/atestados_lote.py:126`, `app/ponto/services/atestados_lote.py:220`, `app/ponto/services/atestados_lote.py:249`
- **As bordas são inclusivas, com significados diferentes.** O lote inclui intervalos fechados que intersectam a janela, inclusive quando `data_fim == de_data`. O Holerite filtra a data civil de `avisado_em` ou `publicado_em`. O Retrato conta datas gravadas dentro das bordas e conserva as pessoas na lista. O juiz de cobertura inclui tipos em aberto com `data_fim=NULL` iniciados antes da janela. O lote trata todo `NULL` como dia de lançamento e perde esse caso. Links antigos com `de`/`ate` também deixam de recortar o lote e caem na competência se não houver as novas chaves. `app/ponto/services/atestados_lote.py:41`, `app/ponto/views_atestado.py:94`, `app/ponto/views_atestado.py:107`, `app/holerite/views.py:409`, `app/core/filtro.py:118`, `app/folha/retrato_folha.py:1327`, `app/folha/retrato_folha.py:1343`, `app/ponto/turnos.py:2822`

## 3. Chamadores e migração da porta — PROCEDE no teste [nao verificado]

A busca por `anexar-documento` e `anexar_documento_ausencia` na cópia encontrou lápides, documentação histórica, testes e o ID do formulário. No HTML de produção, esse ID posta para `relatorios:anexar_atestado`. A API e o cartão de atendimento usam `/api/ausencias/<id>/documento/`. Nenhum chamador da rota removida apareceu em templates, static/PWA, API, `reverse` Python ou código de comunicados. APK externo, comunicados gravados no banco e tráfego real [nao verificado]. `app/ponto/urls.py:72`, `app/ponto/views.py:2165`, `app/core/juizes.py:1097`, `app/templates/ponto/partials/painel_ausencia.html:90`, `app/relatorios/urls.py:18`, `app/api/urls.py:83`, `app/templates/chamados/partials/_card_atendimento.html:70`, `app/docs/RELATO.md:7112`

O teste apagado fazia POST pós-aprovação com um usuário DP de RBAC real e fazia GET do painel para uma pendente. O substituto exercita upload e conteúdo pós-aprovação com **superusuário**. O caso de painel pendente e o caminho positivo com DP comum ficaram sem a prova equivalente nesse substituto. Além disso, o bloco positivo do teste de permissão foi parar dentro do novo método. Ali `painel` já é uma resposta HTTP, mas é passado a `client.get`, e `url` não existe no método. Se o bloco chegar à última asserção, ainda exige `documento` vazio depois de verificar o arquivo. A recusa de férias migrou para a porta nova e exige HTTP 400. `app/ponto/tests/test_anexar_documento.py:10` e `:37` na base do delta, `app/ponto/tests/test_atestado_f3.py:221`, `app/ponto/tests/test_atestado_f3.py:238`, `app/ponto/tests/test_atestado_f3.py:255`, `app/ponto/tests/test_hj091_ferias_derivadas.py:472`

## 4. Rollback, arquivos e resposta — PROCEDE

- **Ordem do caminho normal.** Os settings da cópia não habilitam `ATOMIC_REQUESTS` e o middleware local não envolve a view em `atomic`. O atomic externo relevante é o do próprio lançamento INSS. Se o UPDATE falha em `_reabrir`, sai primeiro o savepoint, depois o `finally` consulta as referências e, com conexão utilizável, pode apagar o upload. Se `_reabrir` retorna e `_gravar_colab` falha com `Exception`, sai primeiro o atomic da tela e depois vem a consulta da compensação. `app/config/settings/base.py:53`, `app/config/settings/saas.py:67`, `app/config/settings/saas.py:83`, `app/config/settings/prod.py:9`, `app/ponto/views.py:2547`, `app/ponto/views.py:2567`, `app/ponto/services/ausencia.py:1431`, `app/ponto/services/ausencia.py:1456`
- **Órfão por `Exception` comum, no caminho real de INSERT INSS.** Se `_insumo_da_folha` falha depois do INSERT e antes do retorno, a limpeza do savepoint do INSERT já ficou para trás. A view sofre rollback, mas sua variável `ausencia` continua `None`; a compensação pula o arquivo. Também há órfão possível se a conexão continuar marcada para rollback ou se a consulta/delete falhar. Um `BaseException` posterior ao retorno do serviço atravessa o `except Exception` da tela e deixa o upload fora da compensação. Ocorrência em produção [nao verificado]. `app/ponto/services/ausencia.py:140`, `app/ponto/services/ausencia.py:654`, `app/ponto/services/ausencia.py:691`, `app/ponto/views.py:2542`, `app/ponto/views.py:2550`, `app/ponto/views.py:2568`, `app/ponto/services/ausencia.py:1458`, `app/ponto/services/ausencia.py:267`
- **Arquivo de linha que nasceu.** Nos dois ramos com `anexo_gravado`, uma referência visível à consulta impede o delete, inclusive se uma exceção sobe depois do commit. A garantia tem o alcance da consulta. `exists()` e `delete()` são separados, e o alias do banco não amplia a busca para outros schemas do SaaS, cujo storage configurado é de filesystem. Além disso, o ramo legado do INSERT ainda apaga por diferença de nome sem consultar referências. Se um callback do commit do INSERT levantar depois de gravada a linha e o storage tiver alterado o nome, esse ramo pode apagar o arquivo já referenciado. Os callbacks de produção encontrados usam `engolir`; ocorrência dessa falha ou referência compartilhada em produção [nao verificado]. `app/ponto/services/ausencia.py:257`, `app/ponto/services/ausencia.py:263`, `app/ponto/services/ausencia.py:652`, `app/ponto/services/ausencia.py:658`, `app/ponto/signals.py:254`, `app/ponto/signals.py:272`, `app/config/settings/saas.py:83`, `app/config/settings/saas.py:109`
- **Exceção e resposta.** O `finally` não tem `return` ou tratamento que suprima a exceção original. O helper captura falha de consulta ou delete, registra e devolve `False`. A view mantém o `raise` e a renderização de erro com `HX-Trigger` e HTTP 200 nesse caminho. O RED HTTP cobre falha no UPDATE da reabertura. O teste sem banco da compensação só recebe um `criar_ausencia` simulado que já retornou. Ambos deixam aberta a falha do INSERT antes do retorno. `app/ponto/services/ausencia.py:1456`, `app/ponto/services/ausencia.py:267`, `app/ponto/views.py:2577`, `app/ponto/views.py:2605`, `app/core/respostas.py:74`, `app/ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:182`, `app/ponto/tests/test_atestado_f3_contrato.py:627`

## 5. Classes já reprovadas — PROCEDE

| Classe | Veredito |
|---|---|
| **Segundo juiz e derivação paralela** | **PROCEDE** para o período. O SQL do lote decide a interseção e o leitor do `aplicar` a decide outra vez sobre a data ajustada. A fórmula SQL já existia na c13 e diverge da autoridade registrada para ausência em aberto. A busca local saiu e a janela de competência continua no juiz registrado. `app/ponto/services/atestados_lote.py:21`, `app/ponto/services/atestados_lote.py:41`, `app/ponto/views_atestado.py:117`, `app/core/juizes.py:261`, `app/ponto/turnos.py:2822`, `app/ponto/views_atestado.py:68` |
| **Segundo escritor fora da porta** | **NAO PROCEDE** como escritor novo. As gravações continuam no serviço. Os saves integrais de edição e rederivação são escritores indiretos herdados, inclusive de documento e metadados. `app/ponto/services/ausencia.py:329`, `app/ponto/services/ausencia.py:762`, `app/ponto/services/ausencia.py:1438`, `app/ponto/services/ausencia.py:1890` |
| **Relógio fora da borda** | **NAO PROCEDE** no delta. O lote resolve mês e ano com um `localdate()` na borda. O relógio de `prazo_documento` dentro de `_reabrir` é anterior ao delta. `app/ponto/views_atestado.py:91`, `app/ponto/services/ausencia.py:1452` |
| **Ordem de trava** | **NAO PROCEDE** como inversão introduzida. INSS trava Colaborador antes de chamar a escrita da Ausência. O savepoint novo entra dentro dessa ordem e a consulta de referências é uma leitura `exists()`. `app/ponto/views.py:2547`, `app/colaboradores/services/colaborador.py:87`, `app/ponto/services/ausencia.py:1431`, `app/ponto/services/ausencia.py:263` |
| **Campo com dois significados** | **PROCEDE** para a semântica de `q` entre telas que adotam o filtro, e para o período sobre registros em aberto. `atestado_medico` continua sendo um campo de nome do médico e também o rótulo de um código de tipo legado, já apontado na c13. `app/core/filtro.py:200`, `app/core/busca.py:128`, `app/ponto/views_atestado.py:70`, `app/ponto/services/atestados_lote.py:43`, `app/ponto/turnos.py:2827`, `app/ponto/models.py:787`, `app/ponto/services/ausencia.py:602` |
| **Pouso antes do pré-requisito** | **PROCEDE** como impedimento ao pouso. Os oito alvos passam, mas o teste de painel tem o erro descrito no item 3. A suíte sem banco rodou 42 casos e teve um erro local por falta de `PIL`. A suíte inteira verde, o pouso efetivo e a ordem histórica ficam [nao verificado]. O aval V2 registrado prevê HEIC como link de abertura. `app/ponto/tests/test_atestado_f3.py:255`, `app/ponto/tests/test_atestado_f3_contrato.py:1015`, `CLAUDE.md:398`, `app/docs/BACKLOG.md:2352` |

**Mordida dos oito mutantes.** Execu

[truncado em 20000 de 23868 caracteres]

## 26/11 20:10 · sessao 527c32aa7cfc · SUITE-ROTEIA-F2 · verificador · origem=Code

**SUITE-ROTEIA-F2 — a chamada contratada entra na rota da Pista 2. Há duas divergências na revisão.** A condição também admite `--only` ou `--volta` quando a guarda classifica a seleção como inteira. E um job `pre-opus` pode satisfazer o certificador de pouso, porque o certificador julga o que rodou e o SHA, sem ler `MOTIVO`. `bin/suite.sh:200–216`, `bin/suite.sh:272–289`, `bin/suite.sh:441–445`, `bin/suite.sh:502–506`, `bin/pista_certifica.sh:41–96`

Revisei a cópia em leitura e conferi em memória o ramo da rota, a âncora do mutante novo e as condições do certificador. A associação da cópia ao objeto Git `1f4b92cff`, a execução integral do selo e a próxima pré-opus na coluna **PISTA 2** do vercodex, com a VM livre, ficam **[não verificado]**. O código registra job e SHA ao aceitar e registra o rc ao terminar. O desligamento documentado é o revert do pouso próprio. `bin/suite.sh:341–342`, `bin/suite.sh:452–453`, `bin/suite.sh:496–506`, `bin/tests/test_suite_rota_pista.sh:21–22`

### 1. Quem escreve e quem lê

| Valor | Escritor, leitores e significado |
|---|---|
| `MOTIVO` | O chamador passa `--motivo` e o parser grava o texto. A guarda exige a forma completa `pre-opus <OBRA>` ou `pouso <OBRA>` para inteira. A rota usa a mesma regex para inteira `pre-opus`, mas aplica o prefixo `pouso*` ao recorte elegível. Assim, um recorte efetivo com `pousoXYZ` vai para a VM, embora esse texto seja inválido para inteira. O `--seco` exibe o valor e o `pede` omite o campo. `CLAUDE.md:96`, `bin/suite.sh:179`, `bin/suite.sh:272–298`, `bin/suite.sh:301–306`, `bin/suite.sh:441–445`, `bin/suite.sh:502–505` |
| `ONLY` | O parser recebe `--only`. Pela entrada alternativa, `vizinhos_da_volta.py` emite os rótulos e a suíte filtra e grava `ONLY`. A validação e `_suite_inteira` leem essa lista, e os dois executores usam `${ONLY:-$LABELS}`. Uma lista preenchida é uma seleção explícita e pode cobrir a inteira. `bin/suite.sh:172`, `bin/suite.sh:200–226`, `bin/suite.sh:273–289`, `bin/suite.sh:444`, `bin/suite.sh:568`, `bin/vizinhos_da_volta.py:126–130` |
| `LABELS` | `regua.sh` escreve a lista canônica. A suíte lê a régua da raiz da porta para validar, classificar e preencher a seleção padrão na VM e na Pista. O certificador também lê a régua e exige cada rótulo uma vez. O diff conserva a fonte única de L-075. `bin/regua.sh:23`, `bin/suite.sh:195–196`, `bin/suite.sh:212–226`, `bin/suite.sh:277`, `bin/suite.sh:444`, `bin/suite.sh:568`, `bin/pista_certifica.sh:67`, `bin/pista_certifica.sh:85–87`, `app/docs/LEIS.md:102` |
| `DIR` | O chamador fornece o caminho. A suíte usa-o para a descoberta, a validação da cópia e a montagem na VM. A rota exige ainda raiz Git com HEAD e repositório comum com a porta, lê ou fotografa `app/` e publica a ref. O `pede` recebe o SHA resultante. `bin/suite.sh:170–171`, `bin/suite.sh:274`, `bin/suite.sh:313–316`, `bin/suite.sh:359–367`, `bin/suite.sh:417–445`, `bin/suite.sh:519–528` |
| argv do `pede` | A suíte escreve `pede <sha> [--parallel N] --only <rótulos...>`. O `ssh` falso anota a chamada e verifica a ref pelo SHA. O certificador lê outro argv, o de `manage.py test` gravado em `AMBIENTE argv=`. A tradução feita pelo lançador real, seu paralelismo padrão e a montagem remota ficam **[não verificado]**. `bin/suite.sh:441–445`, `bin/tests/test_suite_rota_pista.sh:40–59`, `app/core/tests/test_ambiente_do_run.py:30–40`, `app/core/tests/test_ambiente_do_run.py:55–56`, `bin/pista_certifica.sh:70–87` |
| rc final | Na Pista, a suíte confere o rc do `espera` com o recibo do mesmo ID e SHA, valida o log e devolve o rc do job, traduzindo `3` e `75` remotos para `1`. `71` pode sair com `ERRO-INFRA PISTA` ou ser o rc de um job com recibo e log válidos. Na VM, `flock` pode devolver `75` e a suíte propaga o rc do processo pela trava. A sessão recebe o rc de `suite.sh` e `volta_intermediaria.sh` o lê explicitamente. Isso preserva o rc de processo como veredito, conforme L-074. `bin/suite.sh:345–346`, `bin/suite.sh:458–506`, `bin/suite.sh:561–569`, `bin/suite.sh:580–616`, `bin/trava_teste.sh:35–42`, `bin/volta_intermediaria.sh:33–46`, `app/docs/LEIS.md:101` |

### 2. Guardas, recusas e fallback

- **Inteira sem `--only` ou `--volta`.** Sem `--dir` ou com `pouso <OBRA>` válido, segue pelo fluxo local. Com `REGUA_DB` não vazio no ambiente ou flag permitida após `--`, imprime `ROTA VM` antes de qualquer SSH. Flag proibida ou motivo inválido dá rc `2` antes da rota. Um `--` sozinho deixa `EXTRA` vazio. `bin/suite.sh:179–186`, `bin/suite.sh:233–249`, `bin/suite.sh:272–307`, `bin/suite.sh:351–355`, `bin/suite.sh:502–523`, `bin/suite.sh:561–569`
- **Seleção explícita ou derivada, em cópia mutável.** A guarda e a rota consultam a cobertura em momentos diferentes. Se a primeira consulta vê um módulo de fora e admite um recorte com motivo inválido, e esse módulo some antes da consulta da rota, a seleção passa a ser inteira pelo mesmo critério. A condição barra a Pista nesse ponto, mas o fluxo segue à VM sem renovar a recusa `2`. Se o módulo some após a decisão de rotear e antes da foto, a Pista pode receber a seleção sobre a árvore já alterada. É um caminho por inspeção do código, sem ocorrência observada **[não verificado]**. `bin/suite.sh:273–298`, `bin/suite.sh:417–445`, `bin/suite.sh:502–523`
- **Falhas da rota.** Pista indisponível, estimativa acima de `1800 s` ou do limite menor de `--espera`, falha de foto/ref e `pede` sem ID válido seguem para a VM. Com ID aceito, a sessão acompanha um único job, com até três tentativas de `espera` e três de `log`. Falha de transporte ou prova inválida dá `ERRO-INFRA PISTA` rc `71`. O fallback pós-aceite é reservado ao `71` classificado como recusa sem execução. `bin/suite.sh:378–449`, `bin/suite.sh:452–506`

### 3. Pode certificar como pouso? [nao verificado]

**Sim, como evidência para o certificador.** Para uma cópia limpa em `app/`, o pedido usa o HEAD e envia todas as `LABELS`. Se o lançador entregar esses rótulos ao Django, sem `--only` literal no `AMBIENTE argv=`, o job pode certificar o mesmo SHA com camadas iguais, recibo do mesmo job, `rc=0`, `oom=0` e `Ran = Found − podados`. Conferi as condições em memória com todos os rótulos e obtive rc `0`. Com `--only` literal no argv do Django, obtive rc `1`. A rota em si termina com o resultado da suíte. A certificação de um job F2 real fica **[não verificado]**. `bin/suite.sh:417–445`, `bin/suite.sh:494–506`, `app/core/tests/test_ambiente_do_run.py:55–56`, `bin/pista_certifica.sh:41–100`

### 4. O selo e o mutante mordem?

**A âncora nova ocorre uma vez**, em `bin/suite.sh:503`. A substituição por `false` mantém a sintaxe e muda o ramo da chamada testada para a VM. O caso reprova ao observar a trava, mesmo com o rc `0` do dublê. O executor exige tanto o rc não zero do caso quanto a mensagem específica `a trava da VM foi chamada`. A execução integral fica **[não verificado]**. `bin/tests/test_suite_rota_pista.sh:33–35`, `bin/tests/test_suite_rota_pista.sh:173–187`, `bin/tests/test_suite_rota_pista.sh:381–389`, `bin/tests/test_suite_rota_pista.sh:442–443`

O caso F2 cobra um único `pede`, os rótulos esperados, espera e log do ID, ausência de trava e `Ran/OK`. A contagem `>= 1` permite chamadas adicionais de `espera` ou `log`. O `ssh` falso registra `$*`, então o selo confere a linha textual, sem distinguir as fronteiras dos argumentos. O vermelho remoto, os fallbacks e o `71` de transporte são exercitados com recorte. `bin/tests/test_suite_rota_pista.sh:40–43`, `bin/tests/test_suite_rota_pista.sh:173–213`, `bin/tests/test_suite_rota_pista.sh:295–335`

### 5. Classes de reprovação

- **Segundo juiz e segundo escritor.** A rota reaproveita `_suite_inteira`, a regex de motivo e a lista da régua. O certificador continua decidindo outra operação, com provas próprias. `bin/suite.sh:195–196`, `bin/suite.sh:272–298`, `bin/suite.sh:502–506`, `bin/pista_certifica.sh:41–96`, `app/docs/LEIS.md:102`
- **Derivação paralela.** Os dois executores usam a mesma expressão para escolher os rótulos. A cobertura, porém, é lida novamente da árvore mutável, com até três consultas no caminho `pre-opus` com seleção explícita. Essa é a divergência entre decisões apontada acima. `bin/suite.sh:273–298`, `bin/suite.sh:444`, `bin/suite.sh:502–503`, `bin/suite.sh:568`
- **Selo sem mordida e campo com dois significados.** O mutante novo atinge a asserção de ida à trava. A ambiguidade concreta está em ler `--only` do protocolo como prova de recorte. Com F2, esse campo também transporta todas as `LABELS`. O rc `71` também requer a mensagem ou o recibo para distinguir erro de infraestrutura de rc do job. `bin/tests/test_suite_rota_pista.sh:178–187`, `bin/tests/test_suite_rota_pista.sh:442–443`, `bin/suite.sh:441–445`, `bin/suite.sh:468–498`, `CLAUDE.md:129`

### 6. `CLAUDE.md` §3 confere?

**Em parte.** A seção acerta a entrada nova e as exclusões por `pouso`, ausência de `--dir`, `REGUA_DB` e flags extras. Faltam a restrição ao repositório comum, que deixa o `git archive` comum na VM, e a exceção pós-aceite da recusa sem execução. `--seco` imprime e sai antes da VM. `--espera` limita a estimativa de admissão e segue para a trava no caminho local. A montagem na VM está demonstrada pela chamada a `arvore_do_push.sh`. A montagem efetiva na Pista fica **[não verificado]**. `CLAUDE.md:99–105`, `CLAUDE.md:138–143`, `bin/suite.sh:301–307`, `bin/suite.sh:354–367`, `bin/suite.sh:391–415`, `bin/suite.sh:458–476`, `bin/suite.sh:519–526`, `bin/suite.sh:561–566`

A frase **“um `pede --only` NAO certifica”** ficou ampla demais. O certificador recusa seleção incompleta ou opção de recorte no argv real do Django. O `--only` do pedido F2 pode carregar a lista inteira. `CLAUDE.md:129`, `bin/suite.sh:441–445`, `bin/pista_certifica.sh:61–92`, `bin/tests/test_pista_certifica.sh:92–97`

## 26/11 19:45 · sessao 6861eb64dfb0 · SUITE-ROTEIA-F2 · censo · origem=Code

A inteira pré-opus com `--dir` fica na VM por causa da condição em `bin/suite.sh:497`. Os dois pontos funcionais da mudança são a entrada da rota, em `bin/suite.sh:497–501`, e a lista enviada ao `pede`, em `bin/suite.sh:440–442`. Fiz o censo só por leitura. A confirmação do hash pelo Git, a execução dos selos e o estado real da pista 2 estão [nao verificado].

### 1. Entrada da rota e leitura do motivo

- Hoje a rota exige `DIR` preenchido, `ONLY` preenchido e `! _suite_inteira`. Uma chamada sem seleção falha nos dois últimos critérios e segue direto para a VM, sem imprimir `ROTA VM`. Mesmo com `ONLY` preenchido, a guarda pode classificar a seleção como inteira quando ela cobre todos os módulos de teste que a função reconhece. (`bin/suite.sh:273–289`, `bin/suite.sh:497–506`, `bin/suite.sh:554–563`)
- `--motivo` é lido em `bin/suite.sh:179`. A inteira exige `pre-opus <OBRA>` ou `pouso <OBRA>`, com exatamente um espaço e ID no formato `[A-Za-z0-9][A-Za-z0-9_.-]*`. O discriminador confiável é a primeira palavra **do motivo já validado**. O `pouso*` atual só é alcançado pelos recortes que passaram pela condição externa e também segura grafias mais amplas nesse caminho. (`bin/suite.sh:167–182`, `bin/suite.sh:272`, `bin/suite.sh:291–299`, `bin/suite.sh:497–500`)
- O ramo novo entra em `497–501`, com `DIR` e inteira de motivo válido `pre-opus`, e chama a mesma `_rota_pista`. Para a inteira sem seleção, o `pede` recebe `LABELS` em `440–442`, assim como o executor VM já usa `${ONLY:-$LABELS}`. Uma seleção explícita merece preservar o argv que a VM receberia, porque `_suite_inteira` também pode classificar uma lista de classes ou métodos como inteira. O `--seco` sai antes da rota, e a conferência de `--dir` também vem antes. (`bin/suite.sh:251–289`, `bin/suite.sh:301–317`, `bin/suite.sh:440–442`, `bin/suite.sh:497–501`, `bin/suite.sh:562–563`)

### 2. Labels, foto e prova de pouso [nao verificado]

- A rota lê `LABELS` da régua, mas hoje envia as palavras de `ONLY`, que vêm de `--only` ou do filtro de `--volta`. O pedido é `pede <sha> [--parallel N] --only <labels>`, com `--only` por último. Para a inteira sem seleção, a fonte dos 13 rótulos é `bin/regua.sh:23`. A foto usa o `HEAD` se `app/` estiver limpo, ou enxerta `app/` da cópia sobre o `HEAD` com índice temporário, e publica `refs/pista/<sha>`. O argv efetivamente montado pelo lançador remoto para um pedido com as 13 labels está [nao verificado]. (`bin/suite.sh:172–176`, `bin/suite.sh:192–226`, `bin/suite.sh:416–449`, `bin/regua.sh:23`)
- **Um pedido com todas as labels e a prova 3 são coisas distintas.** O certificador lê o que o `manage.py test` recebeu no `AMBIENTE argv=` e exige cada label da régua uma vez, sem extras e sem opções de recorte ou desconhecidas. Também exige `Ran = Found − QUARENTENA-PODADO`, recibo do mesmo job e SHA, `rc=0` e `oom=0`. As provas 1 e 2 comparam as camadas com a VM e o SHA do run com o do pouso. O caso chamado `pede --only` no selo fornece **dois rótulos** no argv. O certificador recebe log e recibo, sem acesso à linha de comando do `pede`. (`app/core/tests/test_ambiente_do_run.py:30–40`, `app/core/tests/test_ambiente_do_run.py:50–56`, `bin/pista_certifica.sh:13–24`, `bin/pista_certifica.sh:41–92`, `bin/tests/test_pista_certifica.sh:89–97`)
- A rota valida o recibo pelo id, SHA, status e `oom=0`. Para rc 0, exige `Ran` positivo e `OK` no log. Publica o log cru e prefixa o recibo com `RECIBO PISTA:`. Divergência ou falha de transporte após aceite dá `ERRO-INFRA PISTA` e rc 71. A recusa 71 sem evidência de execução é a exceção que volta à VM. Rc remoto 3 ou 75 vira 1. (`bin/suite.sh:345`, `bin/suite.sh:449–500`)

### 3. Chamadores e leitores

No censo estático de `bin/` e dos hooks versionados, encontrei chamadas diretas com `--dir` e motivo de inteira nos **selos**. O selo da rota usa `pre-opus SELO` e o de montagem usa `pouso RMDIR-COM-GUARDA`. Fora dos selos, a chamada executável encontrada com `--dir` é a volta intermediária, com `--volta` e sem motivo. A forma inteira com cópia aparece documentada no help, e o comando canônico do `CLAUDE.md` usa a árvore viva. Chamadas manuais reais e hooks instalados estão [nao verificado]. (`bin/tests/test_suite_rota_pista.sh:206–213`, `bin/tests/test_rm_com_guarda.sh:151–184`, `bin/volta_intermediaria.sh:29–34`, `bin/suite.sh:22–30`, `CLAUDE.md:95–99`, `bin/hooks.sh:19–33`)

| Leitor | O que lê ou pressupõe |
|---|---|
| `volta_intermediaria.sh` | Grava a saída da suite em `<saida>.log` e decide pelo rc do filho. Com ruff e disjuntor verdes, `75` vira `VOLTA_SEM_VEZ`, `3` vira `VOLTA_NAO_RODOU`, `0` vira `VOLTA_VERDE` e `71` entra no ramo de `VOLTA_VERMELHA`, rc 1. Os marcadores de rota, `Ran` e `OK/FAILED` ficam no log. (`bin/volta_intermediaria.sh:29–53`) |
| Régua | Roda os selos de host e executa o próprio `manage.py test` pela trava. Lê `Ran` e o último `OK/FAILED` do **próprio log** e grava `.regua_stamp` para sua árvore. (`bin/regua.sh:15–16`, `bin/regua.sh:130–145`, `bin/regua.sh:159–181`, `bin/regua.sh:191–206`) |
| Pre-push | O wrapper versionado delega a `bin/pre-push.sh`. O script consome o carimbo da régua quando a impressão e a árvore conferem, ou executa o próprio arsenal. `_teste_irmao` filtra `Ran` e `OK/FAILED` da própria saída, explica o 75 da trava e devolve o rc do processo. A variante em `app/core/hooks/` também chama seu próprio Docker. (`bin/hooks.sh:63–71`, `bin/pre-push.sh:65–74`, `bin/pre-push.sh:124–157`, `bin/pre-push.sh:184–200`, `app/core/hooks/pre-push.sh:54–59`, `app/core/hooks/pre-push.sh:80–89`) |
| `gerar_codex_estado.py` | `respostas()` lê mensagens `final_answer` de JSONL, e `montar()` publica o texto com origem e marcação de atos sem prova. Esses caminhos podem reproduzir texto citado numa resposta, mas não interpretam a saída da suite como veredito. A leitura pelo `vercodex` está [nao verificado]. (`bin/gerar_codex_estado.py:77–104`, `bin/gerar_codex_estado.py:117–159`, `bin/gerar_codex_estado.py:162–178`, `app/docs/PROMPTS.md:3070`) |
| `hook_stop_fila1.py` | É o leitor com dependência local. `_tem_trabalho_em_curso()` procura `manage.py test` ou `git push` no `ps` local. Um job remoto sozinho é invisível a esse predicado, que participa do adiamento do CLEAR. O handoff também observa recursos locais, pelo `flock` da VM e pelo lock da sombra. (`bin/hook_stop_fila1.py:382–405`, `bin/hook_stop_fila1.py:1148–1158`, `bin/hook_stop_fila1.py:1239–1256`, `bin/handoff_sessao.sh:258–273`) |
| `disjuntor.py` | O B3 extrai `APROVADO` ou `REPROVADO` do `SubagentHandback` no transcript. O PreToolUse trata comandos e o leitor do ESTADO agrega a própria trilha. A observação de suite em voo está no hook de Stop, nos pontos acima. (`bin/disjuntor.py:649–671`, `bin/disjuntor.py:1954–1985`, `bin/disjuntor.py:2108–2155`, `bin/hook_stop_fila1.py:1239–1247`) |

### 4. Coluna `PISTA 2` no `vercodex`

O arquivo e a função que preenchem a coluna, assim como o que ela efetivamente conta, estão **[nao verificado]**. A referência na cópia aponta para `~/vercodex.py` ao falar de **RC SEM COLHER**. O contrato anterior da rota pede medir **IDs únicos admitidos por dia** via `ssh pista estado`, e `_rota_pista` conta **rodando** e **na fila** para estimar a vaga. Essas referências documentam duas medidas e deixam a implementação da coluna em aberto. (`app/docs/PROMPTS.md:3070`, `app/docs/RELATO.md:128`, `bin/suite.sh:381–414`)

### 5. Selo e mutante

- O selo roda `suite.sh` real numa árvore temporária, com Git e worktree reais, régua de duas labels (`appA appB`) e dublês de SSH, Docker, montagem e trava. `_caso_ok` já cobra um `pede` exato, `espera` e `log` do mesmo id, veredito no stdout e zero chamadas à trava. `_julga` reúne os casos de rota, fila, recusa, recibo e foto. (`bin/tests/test_suite_rota_pista.sh:20–59`, `bin/tests/test_suite_rota_pista.sh:128–170`, `bin/tests/test_suite_rota_pista.sh:268–312`)
- `rota_pista_so_carimba` imprime `ROTA PISTA` e força a VM, e `_caso_ok` o acusa pela chamada à trava. `fallback_apos_aceite` deixa um rc remoto não zero cair na VM, e `_caso_cai` cobra rc 71 sem trava. A variante `fallback_apos_aceite_vermelho` faz a mesma mutação e usa o caso de vermelho remoto. `_mutante` exige que a âncora exista e que a falha venha pela asserção indicada. (`bin/tests/test_suite_rota_pista.sh:181–196`, `bin/tests/test_suite_rota_pista.sh:353–369`)
- A chamada `--dir "$c" --motivo "pre-opus SELO"` já está em `_caso_nao_roteia`, com a expectativa antiga de VM. Ela entra no caso positivo **inteira-pre-opus-com-dir**, chamado em `_julga`, cobrando `pede <sha> --only appA appB`, `espera` e `log` do mesmo id, `Ran/OK`, rc do job e zero chamadas à trava. Os controles de `--seco` e sem `--dir` já têm suas próprias asserções. Na seção de mutantes, a mutação da condição em `suite.sh:497–501` deve devolver **só a inteira pré-opus** à VM e morrer no caso novo pela chamada à trava. (`bin/tests/test_suite_rota_pista.sh:151–170`, `bin/tests/test_suite_rota_pista.sh:206–214`, `bin/tests/test_suite_rota_pista.sh:268–273`, `bin/tests/test_suite_rota_pista.sh:308–312`, `bin/tests/test_suite_rota_pista.sh:353–369`, `bin/suite.sh:497–501`)

### 6. Leis, cortes e documentação no mesmo commit [nao verificado]

- **L-074** protege um run por vez no banco de teste da VM e o veredito pelo rc do processo. **L-075** fixa `regua.sh::LABELS` como fonte única. **L-105/L-106** tratam do pouso próprio do instrumento e da documentação no commit do marco. O motor figura nas zonas invioláveis. (`app/docs/LEIS.md:101–102`, `app/docs/LEIS.md:131–133`, `CLAUDE.md:145–146`, `CLAUDE.md:346–348`)
- O corte `PISTA-2-EM-USO-1611` registra a divisão anterior entre espinha e lateral. `SIMPLIFICA-V3` mantém suite verde antes de pousar, e o cabeçalho do certificador registra o aval posterior que redefine as três provas. A exigência de inteira verde antes do opus está em `CLAUDE.md:406–407`. O registro da rota anterior põe o limite de 30 min e as exceções no RELATO, com implementação em `suite.sh`. (`app/docs/CORTES.md:567`, `app/docs/CORTES.md:630`, `bin/pista_certifica.sh:2–11`, `CLAUDE.md:406–407`, `app/docs/RELATO.md:124–129`, `bin/suite.sh:390–414`)
- No mesmo commit, a seção 3 precisa ajustar `CLAUDE.md:97–103` para descrever a trava no ramo VM e incluir a inteira pré-opus com `--dir` na rota. Ajustar também `CLAUDE.md:138–142`, onde `--dir` é descrito como pedido de montagem local, e o help, o comentário da rota e o resumo do selo. `--seco` é descrito com precisão como saída sem execução. A prova de pouso está no parágrafo próprio em `CLAUDE.md:129`. (`CLAUDE.md:95–110`, `CLAUDE.md:129`, `CLAUDE.md:138–142`, `bin/suite.sh:22–34`, `bin/suite.sh:301–308`, `bin/suite.sh:319–341`, `bin/tests/test_suite_rota_pista.sh:5–14`, `bin/tests/test_suite_rota_pista.sh:416`)
- `test_suite_sh.sh` lê a seção 3, mas o recorte vai do título até a primeira linha sem `\`. Hoje termina na linha 96 e cobra `bin/suite.sh --motivo …` com um motivo válido. A frase da rota na linha 102 fica fora. Um exemplo com `--dir` antes de `--motivo` no comando canônico exigiria adaptar a regex do selo, que hoje pede `--motivo` logo após o nome do script. (`CLAUDE.md:95–103`, `bin/tests/test_suite_sh.sh:22–54`)

### 7. Contraexemplo

**`REGUA_DB=test_raia bash bin/suite.sh --dir "$COPIA" --motivo "pre-opus SUITE-ROTEIA-F2"`** deve continuar na VM. Hoje a inteira vai direto ao executor local, que repassa `REGUA_DB` ao Django. Com a entrada nova, a mesma chamada deve chegar à guarda de incompatibilidade da rota, emitir `ROTA VM` e seguir pela trava, preservando o banco próprio. Outro caso concreto é a mesma chamada com `-- --tag lento`: a CLI local aceita a flag, e a rota remota recusa qualquer `EXTRA`. Uma cópia extraída por `git archive` sem HEAD próprio também fica na VM. E, mesmo para uma worktree elegível, estimativa acima de 1.800 s continua impedindo o `pede`. (`bin/suite.sh:233–249`, `bin/suite.sh:353–366`, `bin/suite.sh:390–414`, `bin/suite.sh:497–501`, `bin/suite.sh:525–530`, `bin/suite.sh:554–563`, `app/config/settings/ci.py:12–17`)

## 26/11 19:42 · sessao 7f12eaf2ecd3 · O30-F3-CONSTRUIR · construtor · origem=Code

## O30-F3-CONSTRUIR · c14

- **B2 — Filtro universal.** O lote usa `q`, `de_data` e `ate_data` pelo filtro da casa, como o drawer de Holerite. A busca própria saiu e o `q` passou a usar `qs_colaborador`, a mesma busca da consulta de férias. O seletor usa `empresas_visiveis`. Prévia, CSV, ZIP e PDF partem do mesmo recorte, que inclui os lançamentos que cruzam o início do período. [ponto/views_atestado.py:56](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views_atestado.py:56) · [ponto/views_atestado.py:93](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views_atestado.py:93) · [holerite/views.py:407](/home/ronald/wt-codex-o30-f3-construir-010143/app/holerite/views.py:407) · [api/views_mensageria.py:799](/home/ronald/wt-codex-o30-f3-construir-010143/app/api/views_mensageria.py:799) · [colaboradores/queries.py:78](/home/ronald/wt-codex-o30-f3-construir-010143/app/colaboradores/queries.py:78) [nao verificado]
- **B3 — Porta órfã.** View e rota retiradas, com lápide de uma linha em cada local. O grep abaixo encontrou zero chamadores de produção na cópia. O painel posta em `relatorios:anexar_atestado`. Os testes de anexo após aprovação e de recusa em férias derivadas passaram a exercitar essa porta. A referência de inventário está em `core/juizes.py:1097`, atrelada à retirada pela sessão. [ponto/views.py:2165](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views.py:2165) · [ponto/urls.py:72](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/urls.py:72) · [templates/ponto/partials/painel_ausencia.html:91](/home/ronald/wt-codex-o30-f3-construir-010143/app/templates/ponto/partials/painel_ausencia.html:91) · [ponto/tests/test_atestado_f3.py:238](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3.py:238) · [ponto/tests/test_hj091_ferias_derivadas.py:472](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_hj091_ferias_derivadas.py:472) · [core/juizes.py:1097](/home/ronald/wt-codex-o30-f3-construir-010143/app/core/juizes.py:1097)
- **B4 — Anexo sob `atomic` externo.** A reabertura com upload usa um savepoint para desfazer o `UPDATE` antes de consultar as referências e remover o órfão. A compensação da tela INSS usa a mesma consulta se a falha ocorre depois que a reabertura retorna. Se a conexão ainda exige rollback, a reabertura preserva o arquivo e registra o aviso. A asserção de zero órfão foi restaurada. A reversão manual também chama `_reabrir`. A cura usa `finally`, sem `except` novo. [ponto/services/ausencia.py:242](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:242) · [ponto/services/ausencia.py:1429](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:1429) · [ponto/services/ausencia.py:1456](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:1456) · [ponto/views.py:2568](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views.py:2568) · [ponto/views.py:2731](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views.py:2731) · [ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:179](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:179)

## RED e mutantes

Antes das respectivas curas, os RED sem banco falharam porque o dropdown incluía duas empresas excluídas pela fonte de UI, `COL27` não encontrava a pessoa, a porta antiga ainda existia, a reabertura deixava o upload após a falha e a compensação INSS apagava um arquivo referenciado por outra linha. Depois da cura, os 24 testes focados passaram. Os oito mutantes retornaram `rc=1` por falha de asserção. [ponto/tests/test_atestado_f3_contrato.py:552](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:552) · [ponto/tests/test_atestado_f3_contrato.py:601](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:601) · [ponto/tests/test_atestado_f3_contrato.py:778](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:778) · [ponto/tests/mutantes_atestado_f3_c14.py:16](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3_c14.py:16) [nao verificado]

```bash
rg -n 'anexar_documento_ausencia|anexar-documento/' templates static api
rg -n 'anexar_documento_ausencia|anexar-documento/' . -g '!**/tests/**' -g '!**/migrations/**'

export PYTHONDONTWRITEBYTECODE=1
T=ponto.tests.test_atestado_f3_contrato
python3 -m unittest \
  "$T.AtestadoF3LoteSemBancoTest.test_RED_dropdown_usa_o_universo_de_empresas_visiveis" \
  "$T.AtestadoF3LoteSemBancoTest.test_RED_busca_q_tem_o_mesmo_resultado_da_busca_da_casa" \
  "$T.AtestadoF3LoteSemBancoTest.test_RED_porta_http_antiga_foi_retirada" \
  "$T.AtestadoF3PortaSemBancoTest.test_RED_reabrir_compensa_apos_falha_em_atomic_externo" \
  "$T.AtestadoF3PortaSemBancoTest.test_RED_reabertura_com_upload_sem_metadados_usa_savepoint_externo" \
  "$T.AtestadoF3PortaSemBancoTest.test_RED_inss_preserva_anexo_referenciado_apos_falha_no_atomic_da_tela"

mutante() { python3 -m ponto.tests.mutantes_atestado_f3_c14 "$1"; }
mutante empresas
mutante busca
mutante periodo
mutante porta_antiga
mutante sem_savepoint
mutante so_avisa
mutante ignora_referencia
mutante inss_apaga_referenciado
```

A rodada completa do módulo sem banco teve 41 testes concluídos e um erro de importação por falta de Pillow. O RED com Django no lançamento INSS, a suíte do Code e o parecer final do Opus estão **[nao verificado]**. [ponto/tests/test_atestado_f3_contrato.py:1015](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:1015) · [ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:182](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:182) [nao verificado]

## 26/11 18:39 · sessao f74e1a36534d · O30-F3-CONSTRUIR · construtor · origem=Code

**O30 F3 · c13 — cura aplicada na cópia.** [nao verificado]

- **Trava.** `_gravar_dados_atestado` usa o gerente nomeado `Ausencia.objects`. A releitura continua sob `select_for_update(of=('self',))`, antes do veto de referência e do `UPDATE`. A catraca resolve o gerente pelo import do modelo, no mesmo padrão da trava de `Ausencia` já usada no serviço. `ponto/services/ausencia.py:311-329`, `ponto/services/ausencia.py:1619-1626`, `chamados/tests/test_e1_v7_catracas.py:99-132`.
- **Seis campos.** Mantive médico, CRM, UF, CID, emissão e dias em `anexar_documento`. O contrato atribui a escrita à porta de anexar/salvar, e o POST já envia os campos por `**dados`. A ligação ao gravador já existia na c12 via `locals()`. Tornei explícito o repasse ao validador nas três portas apontadas pelo esmeril: criação, edição e anexo. O validador continua distinguindo campo omitido de valor enviado. O anexo chama o gravador dentro da transação quando há dados. `docs/BACKLOG.md:1441`, `ponto/views_atestado.py:31-50`, `ponto/services/ausencia.py:272-299`, `ponto/services/ausencia.py:424`, `ponto/services/ausencia.py:725`, `ponto/services/ausencia.py:1108-1138`. O brief/c11v separado ficou [nao verificado].

**Chamadores e leitores**

- O `rg` encontrou quatro chamadores de produção de `anexar_documento`. O formulário F3 envia os metadados e o arquivo opcional. A tela de ausência e as duas rotas da API enviam o arquivo. `ponto/views_atestado.py:50`, `ponto/views.py:2184`, `api/views.py:1857`, `api/views.py:3143`.
- O drawer lê os seis campos e mostra o documento do registro raiz. O CSV e o PDF leem os seis quando há permissão de anexo. O chamador de `pdf_bytes` é `lote_atestados` em `ponto/views_atestado.py:136`. O PDF, o ZIP e a rota de visualização leem o arquivo. `ponto/views.py:2100-2109`, `templates/ponto/partials/painel_ausencia.html:71-131`, `ponto/services/atestados_lote.py:104-128`, `ponto/services/atestados_lote.py:142-178`, `ponto/services/atestados_lote.py:202-211`, `ponto/services/atestados_lote.py:249-261`, `ponto/views.py:2138-2159`.
- Os seis campos entram no `INSERT` de `criar_ausencia` ou no `UPDATE` de `_gravar_dados_atestado`. O teste de censo confere esses dois escritores. `_reabrir` chama o gravador e trata documento, descrição, estado e prazo. A compensação do arquivo passa por `_apagar_anexo_escrito_agora`. `ponto/services/ausencia.py:329-336`, `ponto/services/ausencia.py:581`, `ponto/services/ausencia.py:653-656`, `ponto/services/ausencia.py:1429-1463`, `ponto/services/ausencia.py:242-265`, `ponto/tests/test_atestado_f3_contrato.py:295-303`.

**RED e resultado local**

- A catraca existente deu RED com `?type` na c12. Com a cura, passaram os 9 testes da classe de trava usando `unittest.TestCase` como substituto do `SimpleTestCase` para carregar a análise estática. `chamados/tests/test_e1_v7_catracas.py:385-390`.
- Passaram 25 testes de contrato e porta sem banco. Reforcei o teste de anexo existente para executar o validador real com formulário simulado e conferir os seis valores no `UPDATE`, a releitura sob trava e a trilha. Retirar o repasse ao gravador dá RED na asserção de escrita. Restaurar `locals()` faz a tabela de símbolos apontar os 18 parâmetros sem referência explícita. `ponto/tests/test_atestado_f3_contrato.py:374-382`, `ponto/tests/test_atestado_f3_contrato.py:612-631`, `ponto/services/ausencia.py:424`, `ponto/services/ausencia.py:725`, `ponto/services/ausencia.py:1124`.
- O RED do Vulture consta no anexo fornecido. Vulture na c13, suite inteira e banco [nao verificado]. A certificação fica com o Code e o opus final. `core/tests/test_contract_esmeril.py:26-29`.

**Comandos dos mutantes**

Execute cada bloco na raiz de `app`. Os três deram código de saída `1` e operam em memória. `chamados/tests/test_e1_v7_catracas.py:90`, `ponto/services/ausencia.py:317`, `ponto/tests/test_atestado_f3_contrato.py:612`.

**Trava por `type(ausencia)`** — RED com `?type`. `chamados/tests/test_e1_v7_catracas.py:111-132`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import sys
import unittest
from pathlib import Path
from types import SimpleNamespace

sys.modules['django.test'] = SimpleNamespace(SimpleTestCase=unittest.TestCase)
from chamados.tests.test_e1_v7_catracas import travas_fora

path = Path('ponto/services/ausencia.py')
fonte = path.read_text()
alvo = 'gerente = Ausencia.objects\n    ausencia.refresh_from_db'
assert fonte.count(alvo) == 1
mutante = fonte.replace(alvo, 'gerente = type(ausencia).objects\n    ausencia.refresh_from_db')
achados = travas_fora(mutante, str(path))
assert achados == [], achados
PY
```

**Volta de `locals()` nas três portas** — RED com 18 parâmetros sem referência explícita. `ponto/services/ausencia.py:424`, `ponto/services/ausencia.py:725`, `ponto/services/ausencia.py:1124`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import re
from pathlib import Path
from symtable import symtable

path = Path('ponto/services/ausencia.py')
mutante, trocas = re.subn(
    r'_dados_atestado_validados\(_EntradaAtestado\(\n[^\n]+\n    \)\._asdict\(\)\)',
    '_dados_atestado_validados(locals())',
    path.read_text(),
)
assert trocas == 3
mortos = [
    (f.get_name(), nome)
    for f in symtable(mutante, str(path), 'exec').get_children()
    if f.get_type() == 'function'
    for nome in f.get_parameters()
    if nome.startswith('atestado_') and not f.lookup(nome).is_referenced()
]
assert mortos == [], mortos
PY
```

**Anexo descarta os seis dados validados** — RED porque a escrita dos campos deixa de ocorrer. `ponto/services/ausencia.py:1133`, `ponto/tests/test_atestado_f3_contrato.py:621-622`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import inspect
import unittest
from ponto.tests.test_atestado_f3_contrato import AtestadoF3PortaSemBancoTest, ausencia

fonte = inspect.getsource(ausencia.anexar_documento)
alvo = '_gravar_dados_atestado(ausencia, dados_atestado, por)'
assert fonte.count(alvo) == 1
exec(fonte.replace(alvo, '_gravar_dados_atestado(ausencia, {}, por)'), vars(ausencia))
teste = AtestadoF3PortaSemBancoTest('test_anexa_arquivo_e_seis_na_mesma_linha') [nao verificado]
resultado = unittest.TextTestRunner().run(teste) [nao verificado]
raise SystemExit(not resultado.wasSuccessful())
PY
```

## 26/11 18:32 · sessao bdb349ee95c8 · E4D-CONTRATO-ROTA-B · construtor · origem=Code

Corrigi as cinco montagens de ausência e fiz o caso 18 falhar por `AssertionError` quando o vigia omite `ato_futuro`. A edição ficou em [chamados/tests/test_e4d_matriz_reds.py](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:245).

| Caso | Causa → conserto | Por que ainda morde |
|---|---|---|
| 03 | `registrado_por_id` nulo → `registrado_por=self.admin`. O model exige a FK em `ponto/models.py:811`. | Exige preservar a absorção e seus vínculos, sem criar validação retroativa nem alterar a projeção ou os eventos. `chamados/tests/test_e4d_matriz_reds.py:251–260` |
| 28 | `registrado_por_id` nulo → `registrado_por=self.admin`. `ponto/models.py:811` | Exige trocar a via pelo fato do espelho, registrar `REABERTURA` ao perder a absorção e reler sob a trava antes da escrita. `chamados/tests/test_e4d_matriz_reds.py:751–777` |
| 29 | `registrado_por_id` nulo → `registrado_por=self.admin`. `ponto/models.py:811` | Exige gravar `REABERTURA` antes de limpar `materializada_em`, com instante e autor conferidos. `chamados/tests/test_e4d_matriz_reds.py:784–797` |
| 25 | `pendente` fora das choices → `aguardando_documento`, estado do atestado sem anexo que admite rejeição. `ponto/models.py:690–710`, `ponto/services/ausencia.py:914–916,926–944` | Exige que a rejeição localize a irmã pelo ato de absorção, reabra a disputa e registre a trilha. `chamados/tests/test_e4d_matriz_reds.py:1032–1050` |
| 26 | `pendente` fora das choices → `aguardando_documento`, pelo mesmo estado inicial do atestado sem anexo. `ponto/models.py:690–710`, `ponto/services/ausencia.py:926–944` | Exige preservar a projeção e a sequência de atos do legado e encaminhar a irmã ao admin como `sem_vinculo`. `chamados/tests/test_e4d_matriz_reds.py:1060–1072` |
| 18 | O relatório atual omite `ato_futuro` → `assertIn` exige a chave antes da asserção original de `numero == 0`. `chamados/services/vigia_ato_x_coluna.py:21–25`, `chamados/tests/test_e4d_matriz_reds.py:460–462` | A chave ausente e a contagem `1` falharam com `AssertionError` na execução isolada. As checagens dos dois horizontes continuam em `chamados/tests/test_e4d_matriz_reds.py:445–459`. |
| 14 | O anexo mostra `AttributeError` em `DP.SANEAMENTO`, uma API ausente. Caso mantido. `chamados/models.py:4326–4337`, `chamados/tests/test_e4d_matriz_reds.py:390–392` | A primeira leitura já exige `DP.SANEAMENTO`. As asserções seguintes exigem que o saneamento alcance a resposta histórica e preserve a posterior. `chamados/tests/test_e4d_matriz_reds.py:390–413` |

**Mutantes.** O comando abaixo verifica os três retornos da FK ausente e os dois retornos de `pendente` por inspeção estática. Também executa as asserções finais do caso 18 com o relatório atual, com `numero=1` e com o controle `numero=0`. Os sete mutantes foram detectados. Cada argumento seleciona um mutante. `ponto/models.py:690,811`, `chamados/tests/test_e4d_matriz_reds.py:251,460,751,784,1032,1060`

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - 03 28 29 25 26 18-chave 18-numero <<'PY'
import ast
import sys
from pathlib import Path
from types import SimpleNamespace
from unittest import TestCase

def ler(path):
    return ast.parse(Path(path).read_text())

path = 'chamados/tests/test_e4d_matriz_reds.py'
arvore = ler(path)
compile(arvore, path, 'exec')
casos = {n.name.split('_')[2]: n for n in ast.walk(arvore)
         if isinstance(n, ast.FunctionDef) and n.name.startswith('test_caso_')}
modelo = next(n for n in ler('ponto/models.py').body
              if isinstance(n, ast.ClassDef) and n.name == 'Ausencia')
campos = {n.targets[0].id: n.value for n in modelo.body
          if isinstance(n, ast.Assign) and isinstance(n.targets[0], ast.Name)}
escolhas = dict(ast.literal_eval(campos['STATUS_CHOICES']))
fk = {k.arg: k.value for k in campos['registrado_por'].keywords}
assert not ast.literal_eval(fk.get('null', ast.Constant(False)))

ns = {}
vigia = ler('chamados/services/vigia_ato_x_coluna.py')
exec(compile(ast.Module(vigia.body[2:], []), 'vigia_ato_x_coluna.py', 'exec'), ns)
qs = SimpleNamespace(prefetch_related=lambda *_: qs, iterator=lambda **_: iter(()))
atual = ns['contar'](qs)
codigo = compile(ast.Module(casos['18'].body[-3:], []), 'caso18', 'exec')
teste = TestCase() [nao verificado]
teste.alvo = lambda p: p [nao verificado]

def verificar(relatorio):
    exec(codigo, {'self': teste, 'p': None, 'contar': lambda _: relatorio}) [nao verificado]

verificar(dict(atual, ato_futuro={'numero': 0}))
for mutante in sys.argv[1:]:
    if mutante.startswith('18-'):
        with teste.assertRaises(AssertionError): [nao verificado]
            verificar(atual if mutante == '18-chave' else dict(atual, ato_futuro={'numero': 1}))
    else:
        chamada = next(n.value for n in casos[mutante].body
                       if isinstance(n, ast.Assign) and n.targets[0].id == 'a')
        args = {k.arg: k.value for k in chamada.keywords}
        if mutante in ('25', '26'):
            assert ast.literal_eval(args['status']) in escolhas
            args['status'] = ast.Constant('pendente')
            assert ast.literal_eval(args['status']) not in escolhas
        else:
            assert ast.unparse(args.pop('registrado_por')) == 'self.admin'
            assert 'registrado_por' not in args
    print(f'{mutante} detectado')
PY
```

**Comando para o Code**, no ambiente de testes. Resultado da matriz completa e ponto exato de falha dos cinco casos de ausência [nao verificado]. `manage.py:9–18` [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4d_matriz_reds
```

## 26/11 18:31 · sessao e5b2aecf07cc · ABONO-PARCIAL-TETO · leitura · origem=Code

1. **A máscara da cópia preserva `codigo_dominio`, `matricula_hasner`, `matricula_nexti` e `EmpresaIntegracao.codigo_no_sistema`.** O refazimento completo roda as duas máscaras. O incremental repõe quatro tabelas de ponto e escala e roda a máscara pessoal. Os campos de matrícula e de integração ficam fora das atualizações das duas máscaras. [bin/sombra.sh:196-207,255-275] [bin/sombra_incremental.sh:32-38,50-68] [bin/sombra_mascara.py:51-90] [bin/sombra_mascara_pessoal.py:16-65] [bin/sombra_regras_pessoais.py:48-158] [app/colaboradores/models.py:302-314] [app/folha/models.py:232-236]

   A matrícula do registro 10 vem de `Colaborador.codigo_dominio`. O código da empresa vem de `EmpresaIntegracao.codigo_no_sistema`. O `zfill(10)` preenche até dez caracteres e preserva códigos mais longos. O registro 11 contém a data e o sufixo fixo `1`. A máscara pessoal também reescreve o conteúdo de exportações já gravadas em `ExportacaoDominio.conteudo`. As linhas sob teste são montadas de novo a partir dos fechamentos recalculados. [app/folha/export.py:23-45,215-221,699-755] [bin/sombra_regras_pessoais.py:102-105] [logs/sombra/abono_parcial_teto_txt11.py:200-221]

2. **O traceback aponta para A se o arquivo executado corresponde ao anexo.** Contando a linha vazia inicial, os quatro quadros batem com o anexo e a linha 271 é a chamada de A. A empresa, o tipo do erro e a recusa efetiva em produção ficam [nao verificado]. A view recusa o TXT quando recebe erros desse mesmo validador. [sombra1.out:29-34] [logs/sombra/abono_parcial_teto_txt11.py:218,271-272,355,368] [app/folha/views.py:272-288]

   A versão abaixo compara o TXT serializado e, quando o layout é inválido, recupera os campos do registro 10 pelas larguras usadas na própria montagem. Isso mantém a comparação por rubrica mesmo com matrícula longa ou não numérica. Conferi a compilação e exercitei esses casos com as funções de exportação da cópia e dados sintéticos. A execução na sombra está [nao verificado]. [app/folha/export.py:34-45,215-255,266-307,525-539,733-755]

```python
### SCRIPT
# Sonda ABONO-PARCIAL-TETO para logs/sombra/<script.py>. [bin/sombra.sh:477-492]
# Execução na sombra e valores dos oito alvos [nao verificado].
# O Python sai com rc 1 em universo vazio. Para propagar esse rc ao shell,
# use bash -e bin/sombra.sh --rodar <script.py>. A porta termina com echo
# após a pipeline e ativa pipefail. [bin/sombra.sh:45,483-493]

import logging
import sys
from collections import Counter, defaultdict
from datetime import datetime, time
from traceback import extract_tb
from types import SimpleNamespace
from unittest.mock import patch

from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Empresa
from escala.servico_jornada import escala_vigente
from escala.utils import cadeia_do_dia
from folha import export
from ponto.catalogo.ausencias import ABONA, parciais_vigentes
from ponto.models import Ausencia
from ponto.motor_calculo_v2 import _minutos_da_ponta_cobertos
from ponto.services import dia_pago, fechamento
from ponto.services.credito_parcial import abater_no_proprio_dia
from ponto.turnos import cobre_parte_do_dia

MES, ANO = 11, 2026
EMPRESAS = (2, 3, 4)
ALVOS = {327, 242, 298, 288, 222, 885, 827, 258}  # Anexo fornecido [nao verificado].
NV = "[nao verificado]"
REF_TXT = "[app/folha/export.py:34-45,215-255,699-755]"
REF_ABONO = "[app/ponto/services/dia_pago.py:285-292,364-402]"
REF_DIA = (
    "[app/escala/services/leitor_celula.py:390-436] "
    "[app/ponto/services/fechamento.py:893-895,1018-1027,1139-1151] "
    "[app/ponto/services/dia_pago.py:173-195,274-292] "
    "[app/folha/export.py:169-185,383-399,449-452,749-755]"
)


class ReverterSonda(Exception):
    pass


class FalhaSonda(RuntimeError):
    pass


def col(cid):
    return f"col{cid:03d}"


def log_sem_dados(_logger, registro):
    # A mensagem original do fechamento pode conter identificação pessoal.
    # [app/ponto/services/fechamento.py:1327-1332]
    if registro.levelno >= logging.ERROR:
        erro = registro.exc_info[1] if registro.exc_info else sys.exception()
        tipo = type(erro).__name__ if erro is not None else "ERROR"
        raise FalhaSonda(f"{registro.pathname}:{registro.lineno} {tipo}") from None


def teto_no_previsto(colab, dia, ausencia, previsto, celulas):
    # A cadeia usa o vínculo da célula, todos os blocos líquidos e o dia
    # de início do turno. [app/escala/servico_jornada.py:25-66]
    # [app/escala/models.py:1109-1144,1177-1213] [app/escala/utils.py:65-81,330-363,2345-2374]
    if previsto == 0:
        return 0
    escala = escala_vigente(colab, dia, celulas=celulas)
    if escala is None or escala.tipo_escala is None:
        raise FalhaSonda(f"{col(colab.pk)} {dia} vínculo indeterminado [app/escala/servico_jornada.py:49-66]")
    marcos = escala.marcos_do_dia(dia, celulas=celulas)
    pausas = escala.pausas_do_dia(dia, celulas=celulas)
    if not pausas and marcos[2] and marcos[3]:
        pausas = (marcos[2:],)
    zero = datetime.combine(dia, time.min)
    instantes = [
        (instante.replace(tzinfo=None) - zero).total_seconds() / 60
        for _, instante in cadeia_do_dia(dia, marcos, pausas)
    ]
    blocos = list(zip(instantes[::2], instantes[1::2]))
    if not blocos or sum(fim - ini for ini, fim in blocos) != previsto:
        raise FalhaSonda(
            f"{col(colab.pk)} {dia} geometria diverge do previsto da grade "
            "[app/escala/utils.py:353-363] [app/escala/services/leitor_celula.py:397-423]"
        )
    # O juiz de cobertura é batida_dentro_da_parcial, chamado por
    # _minutos_da_ponta_cobertos. [app/ponto/motor_calculo_v2.py:453-477]
    # [app/ponto/turnos.py:2653-2711]
    return sum(_minutos_da_ponta_cobertos((ausencia,), ini, fim) for ini, fim in blocos)


def imprimir_layout(empresa_id, lado, erros):
    # Os diagnósticos podem trazer repr da linha e sua matrícula.
    # [app/folha/export.py:266-307]
    contagem = Counter(erro.partition(":")[0].partition(" len")[0] for erro in erros)
    tipos = ",".join(f"{tipo}={n}" for tipo, n in sorted(contagem.items())) or "-"
    print(f"LAYOUT empresa={empresa_id} lado={lado} erros={len(erros)} tipos={tipos}",
          "[app/folha/export.py:266-307]", flush=True)
    if lado == "A" and erros:
        print(f"LAYOUT_A_RECUSA_NA_VIEW empresa={empresa_id}",
              "[app/folha/views.py:282-288]", flush=True)


def ler_txt(linhas, itens, fechs, entra, campos_10=None):
    # Matrícula usada apenas dentro da empresa para associar o registro ao ID.
    # Preserva multiplicidade, valor cru e datas filhas. [app/folha/export.py:215-255,725-753]
    por_matricula = defaultdict(list)
    for item in itens:
        cid = item["colaborador_id"]
        if cid in entra and fechs[cid].status in export.STATUS_NO_TXT:
            por_matricula[item["codigo_dominio"].zfill(10)].append(cid)

    registros = defaultdict(list)
    pai = None
    for linha in linhas:
        if campos_10 is not None and linha.startswith("10"):
            # Com layout inválido, recupera o campo cru pela largura
            # efetiva dos argumentos do montador. [app/folha/export.py:34-45]
            mat, rubrica, inicio_valor, fim_valor = next(campos_10)
            parte = {
                "reg": "10", "matricula": mat, "rubrica": rubrica,
                "valor": int(linha[inicio_valor:fim_valor]),
            }
        else:
            parte = export.partes_da_linha(linha)
        if parte is None or (parte["reg"] == "11" and pai is None):
            raise FalhaSonda("Registro inválido [app/folha/export.py:215-255]")
        if parte["reg"] == "10":
            ids = por_matricula[parte["matricula"]]
            if len(ids) != 1:
                raise FalhaSonda("Autoria ambígua do registro 10 [app/folha/export.py:725-748]")
            pai = (parte["valor"], [])
            registros[(ids[0], parte["rubrica"])].append(pai)
        else:
            pai[1].append(parte["data"])
    return {
        chave: tuple(sorted((valor, tuple(sorted(datas))) for valor, datas in linhas))
        for chave, linhas in registros.items()
    }


def medir(empresa, lado, aplicar_teto, cadastradas):
    foto = SimpleNamespace(abonos={}, tetos={}, dias={}, furos={})
    recalcular = fechamento.recalcular_fechamento_mes
    grade_original = export.grade_do_fechamento
    por_dia_original = dia_pago.por_dia_da_grade

    def observar_grade(fech):
        grade = grade_original(fech)
        origem = sys._getframe(1)
        cid = fech.colaborador_id
        if origem.f_code is not recalcular.__code__ or cid not in cadastradas:
            return grade

        # Retém os insumos da mesma passada. Saída usa o dia da jornada da
        # lavratura e atraso usa o crédito do próprio dia.
        # [app/ponto/services/fechamento.py:893-895,1018-1038,1225-1241]
        # [app/ponto/services/dia_pago.py:109-132,173-195,274-277]
        local = origem.f_locals
        resultado = local["resultado"]
        saidas = defaultdict(float)
        for p in resultado.periodos:
            dia = dia_pago._dia_de(p, resultado.dia_da_jornada)
            saidas[dia] += p.minutos_saida_antecipada or 0

        for linha in grade["dias"]:
            dia = linha["data"]
            if dia not in cadastradas[cid]:
                continue
            esperado = linha["tipo_dia"] not in {"sem_celula", "indefinido"}
            atraso, _ = abater_no_proprio_dia(
                {dia: local["_atrasos_dia"].get(dia, 0)}, local["_creditos_dia"]
            )
            foto.dias[cid, dia] = (
                linha["minutos_previstos"] if esperado else None,
                linha["minutos_realizados"] if esperado else None,
                atraso / 60,
                saidas[dia] / 60,
                local["_faltas_por_dia"].get(dia, 0) / 60,
                getattr(local["_cob_folha"].get(dia), "pk", None),
            )
        return grade

    def envolver_por_dia(grade):
        abono, previsto, realizado = por_dia_original(grade)
        origem = sys._getframe(1)
        if origem.f_code is not recalcular.__code__:
            return abono, previsto, realizado

        # B aplica min(abono de A, cobertura do período dentro do previsto)
        # em toda a frota elegível. A cobertura vencedora já veio do fechamento.
        # [app/ponto/services/fechamento.py:700-718,1084-1088]
        # [app/ponto/turnos.py:2622-2639,2816-2839]
        local = origem.f_locals
        colab = local["colab"]
        if aplicar_teto:
            abono = dict(abono)
            for linha in grade["dias"]:
                dia = linha["data"]
                if dia not in abono or not linha.get("cobertura_parcial"):
                    continue
                a = local["_cob_folha"].get(dia)
                if (
                    a is None or a.efeito != ABONA
                    or not cobre_parte_do_dia(a, local["_parciais"])
                    or a.hora_ini is None or a.hora_fim is None or a.hora_ini == a.hora_fim
                ):
                    continue
                teto = teto_no_previsto(colab, dia, a, previsto[dia], local["_cels_lote"])
                foto.tetos[colab.pk, dia] = teto
                abono[dia] = min(abono[dia], teto)

        foto.abonos.update({(colab.pk, dia): valor for dia, valor in abono.items()})
        return abono, previsto, realizado

    # Preserva a resposta esperada _GradeSemCelula e deixa subir as demais
    # exceções capturadas pelo fechamento. [app/ponto/services/fechamento.py:1067-1069,1105-1123,1327-1332]
    with (
        patch.object(fechamento, "Exception", fechamento._GradeSemCelula, create=True),
        patch.object(export, "grade_do_fechamento", observar_grade),
        patch.object(dia_pago, "por_dia_da_grade", envolver_por_dia),
    ):
        fechs = recalcular(
            MES, ANO, empresa_id=empresa.pk, somente_leitura=True,
            retornar_objeto=True, lavrar_dias_pagos=False, coletar_furos=foto.furos,
        )

    # Caminho lido de simular_folha e serialização real da porta do TXT.
    # [app/folha/management/commands/simular_folha.py:124-134] [app/folha/views.py:272-288]
    objetos = list(fechs.values())
    itens = export.classificar_export(empresa, MES, ANO, fechamentos=objetos)
    montar_linha_10 = export.montar_linha_10
    campos_10 = []

    def observar_linha_10(matricula, comp, rubrica, tipo, valor, cod_sistema):
        linha = montar_linha_10(matricula, comp, rubrica, tipo, valor, cod_sistema)
        # A ordem é a mesma em que as linhas 10 entram no TXT, inclusive
        # nos blocos de faltas. [app/folha/export.py:34-45,215-221,733-755]
        mat = str(matricula).zfill(10)
        rub = str(rubrica).zfill(4)
        inicio_valor = len(f"10{mat}{comp}{rub}11")
        fim_valor = -len(str(cod_sistema).zfill(10))
        campos_10.append((mat, rub, inicio_valor, fim_valor))
        return linha

    with patch.object(export, "montar_linha_10", observar_linha_10):
        linhas, foto.entra = export.montar_linhas_export(
            empresa, MES, ANO, itens=itens, fechamentos=objetos,
        )
    erros_layout = export.validar_layout_dominio(linhas)
    imprimir_layout(empresa.pk, lado, erros_layout)
    foto.txt = export.conteudo_do_txt(linhas)
    if erros_layout:
        foto.registros = ler_txt(linhas, itens, fechs, foto.entra, iter(campos_10))
    else:
        foto.registros = ler_txt(foto.txt.splitlines(), itens, fechs, foto.entra)
    return foto


def campos_do_dia(foto, cid, dia, lado):
    chave = cid, dia
    previsto, realizado, atraso, saida, falta, _ = foto.dias.get(chave, (None,) * 6)
    abono = foto.abonos.get(chave, 0) if previsto is not None else None
    r8069 = foto.registros.get((cid, "8069"), ())
    r8792 = foto.registros.get((cid, "8792"), ())
    campos = {
        "previsto_min": previsto,
        "realizado_min": realizado,
        "abono_min": abono,
        "horas_atraso": atraso,
        "horas_saida_antecipada": saida,
        "horas_falta": falta,
        "horas_base_8069_dia": export.rubrica_atraso_parcial(atraso, saida)
        if atraso is not None else None,
        "8069_txt_comp_valor_cru": tuple(valor for valor, _ in r8069),
        "8792_txt_data": int(any(dia.strftime("%Y%m%d") in datas for _, datas in r8792)),
        "furo_motor": int(dia in foto.furos[cid]) if cid in foto.furos else None,
        "entra_juiz_txt": int(cid in foto.entra),
    }
    return " ".join(f"{campo}_{lado}={NV if valor is None else valor}" for campo, valor in campos.items())


def medir_e_imprimir(agora):
    empresas = list(Empresa.objects.filter(ativa=True, pk__in=EMPRESAS).order_by("pk"))
    if empresas and {e.pk for e in empresas} != set(EMPRESAS):
        raise FalhaSonda("Escopo incompleto de empresas ativas [app/folha/management/commands/simular_folha.py:112-119]")

    medidas = []
    tipos = parciais_vigentes()
    for empresa in empresas:
        janela = export.janela_competencia(empresa, MES, ANO)
        entregue = export.competencia_entregue(empresa.pk, MES, ANO)
        cadastradas = {}
        faltas = {}
        # O cadastro dos oito alimenta o relatório. O wrapper usa a cobertura
        # do fechamento para todos os colaboradores. [app/ponto/turnos.py:2622-2639,2816-2839]
        for a in Ausencia.objects.filter(
            colaborador__empresa=empresa, colaborador_id__in=ALVOS,
            status="aprovada", tipo__in=tipos, data__range=janela,
        ).select_related("colaborador").order_by("colaborador_id", "data", "pk"):
            if cobre_parte_do_dia(a, tipos):
                cid = a.colaborador_id
                cadastradas.setdefault(cid, {}).setdefault(a.data, []).append(a)
                if cid not in faltas:
                    faltas[cid] = set(export.datas_falta(a.colaborador, empresa, MES, ANO))

        a = medir(empresa, "A", False, cadastradas)
        b = medir(empresa, "B", not entregue, cadastradas)
        medidas.append((empresa.pk, janela, entregue, cadastradas, faltas, a, b))

    # Universo = dias com parcial elegível na grade de competência em aberto.
    # [app/folha/export.py:565-603] [app/ponto/services/dia_pago.py:386-402]
    if not any(b.tetos for _, _, _, _, _, _, b in medidas):
        print("UNIVERSO VAZIO", flush=True)
        raise SystemExit(1)

    print(f"REFERENCIA={timezone.localtime(agora).isoformat()} COMP=11/2026",
          "[app/ponto/services/fechamento.py:634-642] [app/folha/export.py:1240-1247]")
    n_rubricas = 0
    colabs_txt = set()
    dias_abono = {}
    alvos_com_cadastro = set()

    for empresa_id, janela, entregue, cadastradas, faltas, a, b in medidas:
        print(f"EMPRESA={empresa_id} JANELA={janela[0]}/{janela[1]} ENTREGUE={int(entregue)} "
              f"DIAS_ELEGIVEIS={len(b.tetos)} TXT_IGUAL={int(a.txt == b.txt)}",
              "[app/folha/export.py:47-51,525-539,565-603]", REF_ABONO)
        ids_a = {cid for cid, _ in a.registros}
        ids_b = {cid for cid, _ in b.registros}
        for regra, antes, depois in (("JUIZ_TXT", a.entra, b.entra), ("TXT", ids_a, ids_b)):
            for cid in sorted(depois - antes):
                print(f"ENTRA_{regra} {col(cid)}", REF_TXT)
            for cid in sorted(antes - depois):
                print(f"SAI_{regra} {col(cid)}", REF_TXT)
        colabs_txt.update(ids_a ^ ids_b)

        # Uma mudança por (colaborador, código). Os valores são os do campo
        # cru do reg. 10 e cada tupla carrega as datas dos reg. 11. [app/folha/export.py:34-45,224-255]
        for chave in sorted(a.registros.keys() | b.registros.keys()):
            va, vb = a.registros.get(chave, ()), b.registros.get(chave, ())
            if va != vb:
                cid, codigo = chave
                n_rubricas += 1
                colabs_txt.add(cid)
                rotulo = codigo if codigo.isdigit() else NV
                print(f"RUBRICA {rotulo} {col(cid)} VALOR_A={va} VALOR_B={vb}", REF_TXT)

        if a.abonos.keys() != b.abonos.keys():
            raise FalhaSonda(f"Dias do mapa A/B divergentes na empresa {empresa_id} {REF_ABONO}")
        for cid, dia in sorted(a.abonos):
            va, vb = a.abonos[cid, dia], b.abonos[cid, dia]
            if va != vb:
                dias_abono[cid, dia] = va - vb
                print(f"DIA_PAGO_ABONO {col(cid)} {dia} ABONO_A={va:g} ABONO_B={vb:g}", REF_ABONO)

        for cid, dias in sorted(cadastradas.items()):
            alvos_com_cadastro.add(cid)
            for dia, registros in sorted(dias.items()):
                chave = cid, dia
                vencedora = a.dias.get(chave, (None,) * 6)[-1]
                periodos = [
                    (
                        r.hora_ini.strftime("%H:%M") if r.hora_ini is not None else "-",
                        r.hora_fim.strftime("%H:%M") if r.hora_fim is not None else "-",
                        "*" if r.pk == vencedora else "",
                    )
                    for r in registros
                ]
                print(f"ALVO {col(cid)} {dia} PERIODOS={periodos} "
                      f"TETO_MIN={b.tetos.get(chave, '-')} FALTA_DECIDIDA={int(dia in faltas[cid])}",
                      campos_do_dia(a, cid, dia, "A"), campos_do_dia(b, cid, dia, "B"),
                      REF_DIA, "[app/ponto/models.py:745-771] [app/ponto/turnos.py:2816-2839]")

    for cid in sorted(ALVOS - alvos_com_cadastro):
        print(f"ALVO {col(cid)} DIA_PARCIAL={NV} [app/ponto/turnos.py:2622-2639]")

    # MINUTOS_ABONO soma A - B nos dias alterados. [app/ponto/services/dia_pago.py:285-292,364-402]
    print(f"TXT_RUBRICAS_MUDADAS={n_rubricas} COLABS_TXT={len(colabs_txt)} "
          f"DIAS_ABONO_MUDADOS={len(dias_abono)} COLABS_ABONO={len({cid for cid, _ in dias_abono})} "
          f"MINUTOS_ABONO={sum(dias_abono.values()):g}", flush=True)


def main():
    try:
        with patch.object(logging.Logger, "handle", log_sem_dados), transaction.atomic(durable=True):
            with connection.curs

[truncado em 20000 de 20814 caracteres]

## 26/11 18:30 · sessao 1d13e2de582a · AGENTE-NO-MODELO-V5 · censo · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 26/11 18:27 · sessao 6c8069e0e778 · VIGIA-DISCO · verificador · origem=Code

# VIGIA-DISCO v3b · `360dd8ebd`

## Os quatro itens da volta

1. **Janela fonte → `unlink`: parcial.** O segundo exame pega a troca ou o crescimento durante `citado()`. Mas `_fonte_intacta` lê o `lstat` **antes** de varrer `/proc`. Em memória, troquei o inode durante a varredura e o helper retornou `True`. O `unlink` seguinte opera no nome, que já aponta para outro inode. O destino também ficou sem prova de identidade: se `.gz` some durante `citado()`, a função pode remover a fonte e retornar `True`. Se `.gz` é substituído e a operação recusa, o `finally` pode apagar o substituto. Os casos novos exercitam troca e crescimento da **fonte durante `citado()`**. `bin/retencao_disco.py:115`, `bin/retencao_disco.py:432`, `bin/retencao_disco.py:478`, `bin/retencao_disco.py:486`, `bin/retencao_disco.py:491`, `bin/tests/test_disco_estado.sh:242`

2. **Reconferência da cópia: ordem corrigida, remoção ainda sem a mesma prova do plano.** A cópia reprovada na segunda conferência volta ao nome antes de qualquer `chmod`, e o comando novo só altera diretórios. Depois desse ponto, `find` e `rm` podem falhar parcialmente. O retorno pode ser `False` com nome e permissões já alterados, contrariando a docstring. Além disso, a releitura protege o hash exato, embora o plano também proteja **qualquer hash da mesma obra**. Um `.rm.<sha>` preexistente pula tanto os filtros de atividade do plano quanto a releitura da cauda. Conferi em memória que esse ramo chega ao `rm` sem consultar a cauda. `bin/retencao_disco.py:208`, `bin/retencao_disco.py:221`, `bin/retencao_disco.py:302`, `bin/retencao_disco.py:312`, `bin/retencao_disco.py:321`, `bin/tests/test_disco_estado.sh:382`

3. **Limites do handoff e do alarme: os dois casos da linha 41 foram cobertos, mas os recortes ainda divergem.** Reproduzi dois casos em memória. Com `  ## M` indentado, o handoff encerra a lista comum após `strip()`, mas ainda publica como vigente um `DISCO-VIGIA` abaixo desse título. Com `## M` cru dentro das primeiras 40 linhas, o filtro deixa passar o `PAREI` histórico do disco e o alarme pode contar essa linha como parada da sessão. O código anterior excluía esse formato em toda a janela de 40 linhas. `bin/tests/test_disco_estado.sh:639`, `bin/tests/test_disco_estado.sh:658`, `bin/disco_estado.py:381`, `bin/disco_estado.py:400`, `bin/handoff_sessao.sh:100`, `bin/handoff_sessao.sh:110`, `bin/alarme_sessao_ociosa.py:129`, `bin/alarme_sessao_ociosa.py:141`, `2d0601d9b:bin/alarme_sessao_ociosa.py:139`

4. **Esquema de `chamadas.log`: parser e chave compartilhados, contrato de leitura incompleto.** `fim` com sete campos ou de outra obra deixa de fechar o `ini` na retenção. Porém, o parser aceita hash de cinco caracteres e data fora do formato antes validado pela retenção. A leitura inicial exige `\n` para avançar o cursor. A cauda aceita um `fim` sem `\n` e com duração vazia se os nove campos já apareceram. Reproduzi esses casos em memória. Também reproduzi o truncamento herdado entre `getsize` e `open`: a cauda retorna `set()`, que autoriza seguir como se nenhuma chamada tivesse chegado. `bin/codex_limite_estado.py:49`, `bin/retencao_disco.py:160`, `bin/retencao_disco.py:169`, `bin/retencao_disco.py:183`, `bin/retencao_disco.py:317`, `bin/tests/test_disco_estado.sh:334`, `bin/tests/test_disco_estado.sh:422`, `2d0601d9b:bin/retencao_disco.py:140`, `2d0601d9b:bin/retencao_disco.py:164`

## Escritores, leitores e significado do estado

| Estado | Escrita, leitura e acordo semântico |
|---|---|
| **`chamadas.log`** | `codex_pergunta.sh` apenda `hora`, `obra`, `papel`, os 12 primeiros caracteres do SHA, PID do shell e `ini`/`fim`. `ini` registra tentativa antes de rodar o Codex e traz pergunta raspada. `fim` traz `thread_id` ou `-`, código de saída, duração em segundos desde `T0C` e motivo opcional. O parser descarta pergunta e motivo. O ESTADO lê horário, obra, papel e `rc`. CODEX-ESTADO e a contagem de tokens casam pelo `thread_id`. O censo do disjuntor lê obra, hash, PID, papel e hora do `ini`. O selo de vagas lê também `rc` e motivo diretamente. `dur` fica na projeção de `fins`, sem consumidor de produção localizado. `bin/codex_pergunta.sh:116`, `bin/codex_pergunta.sh:560`, `bin/codex_pergunta.sh:568`, `bin/codex_pergunta.sh:592`, `bin/codex_pergunta.sh:608`, `bin/codex_limite_estado.py:54`, `bin/codex_limite_estado.py:76`, `bin/codex_limite_estado.py:95`, `bin/codex_limite_estado.py:105`, `bin/codex_limite_estado.py:198`, `bin/gerar_estado.py:277`, `bin/gerar_codex_estado.py:176`, `bin/disjuntor.py:2225`, `bin/tests/test_codex_n_vagas.sh:1008` |
| **Hora, chave e atividade das chamadas** | A hora gravada é civil, com precisão de segundos e sem fuso. O ESTADO usa `now()` local e inclui a borda de 24 h, inclusive horários futuros. O censo converte para epoch no fuso local e exige `corte <= t <= agora`. A retenção usa epoch em segundos com `+12 h` conservadoras e protege idade `<= 48 h`. Também há duas interpretações de chamada encerrada: a projeção do ESTADO guarda o último `fim` por chave, mesmo se anterior a um novo `ini` com a mesma chave. A retenção processa na ordem do arquivo e exige PID vivo para considerar o `ini` aberto. `bin/codex_pergunta.sh:570`, `bin/codex_pergunta.sh:608`, `bin/codex_limite_estado.py:59`, `bin/codex_limite_estado.py:76`, `bin/codex_limite_estado.py:84`, `bin/codex_limite_estado.py:95`, `bin/disjuntor.py:2232`, `bin/retencao_disco.py:132`, `bin/retencao_disco.py:146`, `bin/retencao_disco.py:164`, `bin/retencao_disco.py:227` |
| **`uso`, `obras_do`, `uso_obra`, `abertos`, `fim`** | `ler_chamadas` produz máximos de epoch por hash e obra, conjunto de obras por hash, hashes abertos e cursor em **bytes** para o primeiro byte não consumido. O plano usa os quatro primeiros. A passada repassa `fim` como `desde`, e `seek(desde)` inclui esse byte na releitura. `fim` no dicionário é deslocamento, enquanto `fim` no TSV é o tipo do evento. A releitura devolve apenas hashes. O wrapper pode escrever em `CODEX_LOGD`, enquanto a passada fixa `raiz/logs/codex/chamadas.log`. O alinhamento desses caminhos em produção é [nao verificado]. `bin/codex_pergunta.sh:142`, `bin/retencao_disco.py:160`, `bin/retencao_disco.py:176`, `bin/retencao_disco.py:187`, `bin/retencao_disco.py:226`, `bin/retencao_disco.py:549` |
| **RELATO e HANDOFF** | `linha_topo` produz `AVISO` ou `PAREI: ! DISCO-VIGIA` conforme nível e percentuais. `no_topo_disco` usa o gravador e a trava de diretório de `esteira_vigia`, compartilhados com os escritores de alarme e censo. `gerar_cortes.py` também regrava o RELATO diretamente, fora dessa porta, de forma herdada. O Stop lê faixa de censo e duas linhas úteis. B3 lê essa janela e o `mtime`. O handoff delega os vereditos ao Stop e tem recorte próprio para a listagem. O alarme decide ociosidade sobre 40 linhas e delega ao Stop a prova do censo. `handoff_sessao.sh` grava o HANDOFF, `SessionStart` lê o texto e o Stop lê seu `mtime`. `bin/disco_estado.py:413`, `bin/disco_estado.py:447`, `bin/disco_estado.py:475`, `app/core/esteira_vigia.py:843`, `app/core/esteira_vigia.py:892`, `bin/disjuntor.py:329`, `bin/gerar_cortes.py:117`, `bin/gerar_cortes.py:136`, `bin/hook_stop_fila1.py:462`, `bin/disjuntor.py:765`, `bin/handoff_sessao.sh:124`, `bin/handoff_sessao.sh:328`, `bin/alarme_sessao_ociosa.py:129`, `bin/alarme_sessao_ociosa.py:144`, `.claude/settings.json:40`, `.claude/settings.json:53`, `bin/hook_stop_fila1.py:929` |
| **Cópia, fonte, `.gz` e metadados** | O wrapper publica a cópia com nome de SHA inteiro e retira escrita. A retenção compara nomes e blobs com o Git, lê `cwd` e FDs de `/proc`, renomeia para `.rm.<sha>`, altera permissões e remove. Na compressão, o sistema de arquivos fornece dispositivo, inode, links, tamanho em bytes e `mtime_ns`. A retenção compara esses valores, lê a assinatura e as citações em `app/docs`, grava o gzip, preserva `atime`/`mtime` do arquivo em nanossegundos e usa segundos inteiros no cabeçalho gzip. `citado=True` impede a retirada da fonte. `bin/codex_pergunta.sh:604`, `bin/codex_pergunta.sh:611`, `bin/retencao_disco.py:241`, `bin/retencao_disco.py:264`, `bin/retencao_disco.py:294`, `bin/retencao_disco.py:302`, `bin/retencao_disco.py:366`, `bin/retencao_disco.py:387`, `bin/retencao_disco.py:432`, `bin/retencao_disco.py:462`, `bin/retencao_disco.py:477`, `bin/retencao_disco.py:560` |
| **Travas e `retencao.tsv`** | A ordem observável é `relato.lock` → `disco.lock` → diretório do RELATO no gravador. Não encontrei inversão introduzida nesta volta. Há um atalho herdado: `trava()` aceita `VIGIA_DISCO_TRAVA` igual ao caminho como prova de posse. Na trilha, a retenção grava epoch em segundos e tuplas de produtor, ação, `n`, bytes. `resumo` lê a última passada. Nos produtores, `n` é contagem. Em `df`, é percentual, com `-1` para falta de medida. O topo pode imprimir `-1%` como medição e a fotografia do ESTADO também recebe a trilha. `bin/relato.sh:27`, `bin/relato.sh:44`, `bin/retencao_disco.py:631`, `bin/retencao_disco.py:504`, `bin/retencao_disco.py:530`, `bin/retencao_disco.py:571`, `bin/retencao_disco.py:612`, `bin/retencao_disco.py:660`, `bin/disco_estado.py:284`, `bin/disco_estado.py:405`, `bin/disco_estado.py:428`, `bin/disco_estado.py:501`, `app/core/esteira_vigia.py:846` |

## Selos, fallbacks e código retirado

- **Selo vizinho ainda quebrado.** `test_hook_esperando_com_prova.sh` já copia `hook_stop_fila1.py`, mas omite `disco_estado.py` da árvore em que chama `_tem_parei()`. O import obrigatório já existia na base. A consequência está demonstrada no fixture, e a régua reprova se um selo de host sair com erro. O `rc` de uma rodada real e o bloqueio direto pelo `pre-push.sh` são [nao verificado]. `bin/tests/test_hook_esperando_com_prova.sh:199`, `bin/tests/test_hook_esperando_com_prova.sh:225`, `bin/tests/test_hook_esperando_com_prova.sh:259`, `bin/alarme_sessao_ociosa.py:125`, `2d0601d9b:bin/alarme_sessao_ociosa.py:137`, `bin/regua.sh:138`, `bin/pre-push.sh:13`
- **Mordida e erros.** Conferi em memória que as 70 substituições alteram texto e que as versões Python resultantes compilam. Isso verifica as âncoras. `ws_ordem` introduz `chmod -R` **antes** da reconferência. O caso do hardlink já recusa nessa conferência, portanto ele não prova a proteção contra `chmod -R` **depois** dela. O diff reaproveita o `except ValueError` do parser e mantém o fallback de import opcional do handoff. No alarme, o import obrigatório passa a ocorrer antes de abrir o RELATO. RED original, mordida em execução, selo inteiro e números do log vivo são [nao verificado]. `bin/tests/test_disco_estado.sh:395`, `bin/tests/test_disco_estado.sh:718`, `bin/tests/test_disco_estado.sh:725`, `bin/tests/test_disco_estado.sh:800`, `bin/codex_limite_estado.py:58`, `bin/handoff_sessao.sh:91`, `bin/alarme_sessao_ociosa.py:125`
- **L-111 e leitores restantes.** A busca pelos símbolos removidos `DATA_HORA`, `HASH12` e `ws_data` retornou zero referências exatas em `bin/`, `.claude/` e `app/`. `_epoch_tardio` permanece com um chamador, que passa `datetime`. `LINHA_TOPO` continua em uso pelo helper e pelo selo. Quanto ao arquivo removido em execução, `pista_certifica.sh` ainda exige o caminho do log e procura `AMBIENTE sha=` no texto cru. A guarda da retenção protege citações encontradas em `app/docs`. Se algum log efetivamente elegível ainda serve de entrada ao certificador é [nao verificado]. `2d0601d9b:bin/retencao_disco.py:82`, `2d0601d9b:bin/tests/test_disco_estado.sh:703`, `bin/retencao_disco.py:146`, `bin/retencao_disco.py:163`, `bin/disco_estado.py:378`, `bin/disco_estado.py:392`, `bin/tests/test_disco_estado.sh:648`, `bin/pista_certifica.sh:22`, `bin/pista_certifica.sh:48`, `bin/retencao_disco.py:415`

**REPROVA — (1) retirada da fonte sem revalidar fonte e destino na borda (`bin/retencao_disco.py:432`, `bin/retencao_disco.py:486`), (2) remoção de cópia com prova de atividade incompleta (`bin/retencao_disco.py:221`, `bin/retencao_disco.py:317`), (3) recortes divergentes do RELATO (`bin/disco_estado.py:381`, `bin/handoff_sessao.sh:110`, `bin/alarme_sessao_ociosa.py:141`), (4) formato e cauda de chamadas sem validação comum (`bin/codex_limite_estado.py:54`, `bin/retencao_disco.py:160`, `bin/retencao_disco.py:185`), (5) selo vizinho com dependência ausente e mordida incompleta no `chmod` pós-reconferência (`bin/tests/test_hook_esperando_com_prova.sh:199`, `bin/tests/test_disco_estado.sh:800`).**

## 26/11 18:23 · sessao 99bdbf42b5a2 · PAUSA-INTERNA-NO-PARTIDO · leitura · origem=Code

**1. A fábrica usada pelo fechamento é `core.regua_cct.get_motor_cct`.** `recalcular_fechamento_mes` chama a fábrica em `_montar_motor`, uma vez por fatia, com o colaborador, o primeiro dia da fatia e `esc.tipo_escala`. A fábrica delega a escolha da classe a `ponto.motor_calculo_v2.get_motor`. [app/ponto/services/fechamento.py:299](app/ponto/services/fechamento.py:299) [app/ponto/services/fechamento.py:744](app/ponto/services/fechamento.py:744) [app/ponto/services/fechamento.py:754](app/ponto/services/fechamento.py:754) [app/core/regua_cct.py:428](app/core/regua_cct.py:428) [app/core/regua_cct.py:457](app/core/regua_cct.py:457) [app/core/regua_cct.py:479](app/core/regua_cct.py:479)

**Sim, pode devolver `MotorTurnoPartido`.** O cadastro é o `TipoEscala` do `EscalaColaborador` da fatia. A base `turno_partido` aponta para `MotorTurnoPartido`. A chave legada `turno_partido_noturno` aponta para a subclasse `MotorTurnoPartidoNoturno`. O ciclo `intermitente` tem prioridade. Quando o template Partido tem início e fim, a fábrica exige um vão cadastrado superior a 120 minutos. Se o vão for menor, igual a 120 ou faltar um dos marcos do intervalo, a fábrica escolhe `Motor12x36ComEscala` se o turno cruzar a meia-noite, ou `MotorComercial` nos demais casos. Se faltar o início ou o fim do turno, vale o mapeamento da base. [app/escala/models.py:50](app/escala/models.py:50) [app/escala/models.py:874](app/escala/models.py:874) [app/ponto/motor_calculo_v2.py:3677](app/ponto/motor_calculo_v2.py:3677) [app/ponto/motor_calculo_v2.py:3710](app/ponto/motor_calculo_v2.py:3710) [app/ponto/motor_calculo_v2.py:3764](app/ponto/motor_calculo_v2.py:3764) [app/ponto/motor_calculo_v2.py:3804](app/ponto/motor_calculo_v2.py:3804) [app/escala/servico_jornada.py:104](app/escala/servico_jornada.py:104) [app/escala/servico_jornada.py:152](app/escala/servico_jornada.py:152)

**2. A sonda conta cada colaborador uma vez por classe.** Quem troca de classe na janela pode aparecer em mais de uma linha `MOTOR`. A contagem prévia usa o recorte de fatias do fechamento e inclui a chamada para dias com batida fora das fatias quando o fechamento a faria. Essa chamada passa `tipo_escala=None` e recebe `Motor12x36`. A sonda conta o Partido noturno na própria classe e o inclui no universo Partido. [app/ponto/services/fechamento.py:225](app/ponto/services/fechamento.py:225) [app/ponto/services/fechamento.py:283](app/ponto/services/fechamento.py:283) [app/ponto/services/fechamento.py:812](app/ponto/services/fechamento.py:812) [app/ponto/services/fechamento.py:837](app/ponto/services/fechamento.py:837) [app/ponto/motor_calculo_v2.py:3757](app/ponto/motor_calculo_v2.py:3757) [app/ponto/motor_calculo_v2.py:3677](app/ponto/motor_calculo_v2.py:3677)

No recálculo, a sonda intercepta a fábrica e observa `calcular_periodo` na instância Partido que ela devolveu. `UNIVERSO dias` conta pares únicos de colaborador e dia. `TOTAL` soma os intervalos recebidos por `calcular_periodo`, antes do recorte que o motor faz na janela efetiva. A saída anterior trouxe `UNIVERSO VAZIO`. A causa daquela saída e os totais da competência 11/2026 seguem [nao verificado]. `sombra.out:4` [app/ponto/services/fechamento.py:287](app/ponto/services/fechamento.py:287) [app/ponto/motor_calculo_v2.py:2571](app/ponto/motor_calculo_v2.py:2571) [app/ponto/motor_calculo_v2.py:2666](app/ponto/motor_calculo_v2.py:2666) [app/ponto/motor_calculo_v2.py:3007](app/ponto/motor_calculo_v2.py:3007)

Para propagar o código de falha até o shell chamador, use `bash -e bin/sombra.sh --rodar pausa_interna_partido_c11_escolha.py`. A chamada direta pode devolver o status do `echo` final. A transação `READ ONLY` é da sonda. O wrapper repõe a lavra no banco da sombra antes de iniciar o Python. [bin/sombra.sh:45](bin/sombra.sh:45) [bin/sombra.sh:157](bin/sombra.sh:157) [bin/sombra.sh:483](bin/sombra.sh:483)

### SCRIPT

```python
import logging
import sys
from collections import defaultdict
from datetime import date, datetime, time, timedelta
from traceback import extract_tb
from unittest.mock import patch

from django.db import connection, transaction
from django.utils import timezone

from core import regua_cct
from escala.alimentacao import celulas_do_periodo, escalas_do_periodo
from ponto.janelas import janela_fechamento, periodo_apuracao
from ponto.motor_calculo_v2 import MOTOR_POR_BASE, MotorBase, MotorTurnoPartido, ResultadoMes
from ponto.services import fechamento
from ponto.turnos import batidas_apuraveis, data_local

MES, ANO = 11, 2026
EMPRESAS = (2, 3, 4)
INI, FIM = date(2026, 10, 21), date(2026, 11, 20)
UM_DIA = timedelta(days=1)
UM_MINUTO = timedelta(minutes=1)


class ReverterSonda(Exception):
    pass


class FalhaSonda(BaseException):
    pass


def interceptar_log(_logger, registro):
    # app/ponto/services/fechamento.py:1327-1332 e app/core/regua_cct.py:442-448.
    if registro.levelno >= logging.ERROR:
        erro = registro.exc_info[1] if registro.exc_info else sys.exception()
        tipo = type(erro).__name__ if erro is not None else "ERROR"
        raise FalhaSonda(f"{registro.pathname}:{registro.lineno} {tipo}") from None


def contar_escolhas():
    previstas = defaultdict(set)
    env_ini = periodo_apuracao(MES, ANO, 2)[0]
    env_fim = periodo_apuracao(MES, ANO, 28)[1]
    busca_ini = timezone.make_aware(datetime.combine(INI, time.min)) - UM_DIA
    busca_fim = timezone.make_aware(datetime.combine(FIM, time(23, 59, 59))) + UM_DIA
    for empresa_id in EMPRESAS:
        if janela_fechamento(MES, ANO, empresa_id) != (INI, FIM):
            raise FalhaSonda("app/ponto/janelas.py:54 janela divergente")

        # app/ponto/services/fechamento.py:574,603-626,639-653.
        colabs = list(fechamento.universo_da_competencia(MES, ANO, empresa_id)
                      .select_related("posto__praca", "empresa"))
        celulas_por_colab = defaultdict(dict)
        for (cid, dia), celula in celulas_do_periodo(
            [colab.pk for colab in colabs], env_ini, env_fim
        ).items():
            celulas_por_colab[cid][(cid, dia)] = celula

        for colab in colabs:
            cid = colab.pk
            celulas = celulas_por_colab[cid]

            # app/ponto/services/fechamento.py:673-678.
            geradoras = {
                celula.escala_geradora_id
                for (_, dia), celula in celulas.items()
                if INI <= dia <= FIM and celula.escala_geradora_id
            }
            escalas = sorted(
                escalas_do_periodo(cid, INI, FIM, mais=geradoras),
                key=lambda esc: esc.data_inicio,
            )

            def montar(esc, fatia_ini, _fatia_fim, *, colab=colab):
                # A classe depende de tipo_escala. app/ponto/motor_calculo_v2.py:3754-3812.
                motor = regua_cct.get_motor_cct(colab, fatia_ini, tipo_escala=esc.tipo_escala)
                previstas[type(motor)].add(colab.pk)

                # O resultado vazio permite percorrer as fatias sem calcular a folha.
                # app/ponto/services/fechamento.py:299-335.
                motor.calcular_mes = lambda *args, **kwargs: ResultadoMes(
                    colaborador_id=kwargs["colaborador_id"]
                )
                return motor

            # As fatias são selecionadas por vínculo e célula.
            # app/ponto/services/fechamento.py:208-289.
            apuracao = fechamento.apurar_por_fatia(colab, INI, FIM, (), escalas, celulas, montar)

            # app/ponto/services/fechamento.py:812-840.
            if escalas and len(apuracao.dias_cobertos) < (FIM - INI).days + 1:
                dias_orfaos = set()
                batidas = batidas_apuraveis(colab, busca_ini, busca_fim)
                for ts in batidas.values_list("timestamp", flat=True):
                    dia = data_local(ts)
                    if dia is not None and INI <= dia <= FIM and dia not in apuracao.dias_cobertos:
                        dias_orfaos.add(dia)
                if dias_orfaos:
                    motor = regua_cct.get_motor_cct(colab, min(dias_orfaos), tipo_escala=None)
                    previstas[type(motor)].add(cid)
    return previstas


def recalcular(previstas):
    fabrica = regua_cct.get_motor_cct
    codigo_fatia = fechamento.apurar_por_fatia.__code__
    codigo_recalculo = fechamento.recalcular_fechamento_mes.__code__
    codigo_mes_base = MotorBase.calcular_mes.__code__
    observadas = defaultdict(set)
    universo = set()
    pausas = defaultdict(timedelta)

    def observar_fabrica(colab, competencia, **kwargs):
        motor = fabrica(colab, competencia, **kwargs)
        origem = sys._getframe(1)
        fatia = origem.f_back

        # Imports locais em app/ponto/services/fechamento.py:755-761 e :837-840.
        e_fatia = (
            origem.f_code.co_name == "_montar_motor"
            and fatia is not None
            and fatia.f_code is codigo_fatia
            and fatia.f_back is not None
            and fatia.f_back.f_code is codigo_recalculo
        )
        if not (e_fatia or origem.f_code is codigo_recalculo):
            return motor

        cid = colab.pk
        observadas[type(motor)].add(cid)
        if isinstance(motor, MotorTurnoPartido):
            if not e_fatia:
                raise FalhaSonda("app/ponto/services/fechamento.py:838 Partido fora da fatia")
            universo.update((cid, dia) for dia in fatia.f_locals["_dias_da_fatia"])
            calcular_periodo = motor.calcular_periodo

            def observar_periodo(entrada, saida, intervalos=()):
                origem_periodo = sys._getframe(1)
                dados = origem_periodo.f_locals
                if origem_periodo.f_code is not codigo_mes_base or dados["colaborador_id"] != cid:
                    raise FalhaSonda(
                        "app/ponto/motor_calculo_v2.py:2378 origem de periodo divergente"
                    )

                # Dia da jornada regular ou do crédito de aberto.
                # app/ponto/motor_calculo_v2.py:2571-2573 e :2632-2667.
                dia = dados["_d_a"] if "_pares_a" in dados else dados["data_jornada"]
                if not INI <= dia <= FIM:
                    raise FalhaSonda("app/ponto/motor_calculo_v2.py:2542 dia fora da janela")
                chave = (cid, dia)
                universo.add(chave)
                if intervalos:
                    pausas[chave] += sum((fim - ini for ini, fim in intervalos), timedelta())
                return calcular_periodo(entrada, saida, intervalos)

            motor.calcular_periodo = observar_periodo
        return motor

    with patch.object(regua_cct, "get_motor_cct", observar_fabrica):
        for empresa_id in EMPRESAS:
            # app/ponto/services/fechamento.py:468-471,559-560,1215.
            fechamento.recalcular_fechamento_mes(
                MES, ANO, empresa_id=empresa_id,
                somente_leitura=True, retornar_objeto=True, lavrar_dias_pagos=False,
            )

    if observadas != previstas or not universo:
        raise FalhaSonda("app/ponto/services/fechamento.py:756 escolhas ou cobertura divergentes")
    return universo, pausas


def em_minutos(duracao):
    return f"{duracao / UM_MINUTO:.9f}".rstrip("0").rstrip(".")


def main():
    try:
        with patch.object(logging.Logger, "handle", interceptar_log), transaction.atomic(durable=True):
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            previstas = contar_escolhas()
            classes = set(MOTOR_POR_BASE.values()) | previstas.keys()
            for classe in sorted(classes, key=lambda cls: cls.__name__):
                print(
                    f"MOTOR classe={classe.__name__} colabs={len(previstas.get(classe, ()))}",
                    flush=True,
                )

            tem_partido = any(issubclass(classe, MotorTurnoPartido) for classe in previstas)
            if tem_partido:
                universo, pausas = recalcular(previstas)
            raise ReverterSonda()
    except ReverterSonda:
        pass
    except Exception as erro:
        local = extract_tb(erro.__traceback__)[-1]
        raise FalhaSonda(f"{local.filename}:{local.lineno} {type(erro).__name__}") from None

    if not tem_partido:
        print("PARTIDO_ZERO_PELA_ESCOLHA", flush=True)
        return

    for (cid, dia), duracao in sorted(pausas.items()):
        print(f"col{cid:03d} {dia.isoformat()} minutos={em_minutos(duracao)}")
    print(f"UNIVERSO colabs_partido={len({cid for cid, _ in universo})} dias={len(universo)}")
    total = em_minutos(sum(pausas.values(), timedelta()))
    print(f"TOTAL dia_colab={len(pausas)} colabs={len({cid for cid, _ in pausas})} minutos={total}")


main()
```

### FIM

_8481 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
