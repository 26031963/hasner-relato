# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 09:25** · publicado em **17/11/2026 09:25** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 09:25 · sessao 0b5e2282f266 · - · - · origem=Ronald (tmux)

- **Juiz específico registrado “código do Domínio válido?”: nenhum.** O registro declara `classificar_export` para aprovação e entrada no TXT, além da leitura de tela; o lookup também aponta apenas quem entra no TXT (`app/core/juizes.py:181`, `app/core/juizes.py:182`, `app/core/juizes.py:329`, `app/core/juizes.py:330`, `app/core/juizes.py:743`; `app/docs/JUIZES.md:118`). O próprio lookup distingue tabela documental de registro vivo (`app/docs/JUIZES.md:10`).

- **Há múltiplos sítios vivos de julgamento.** `classificar_export` rejeita somente código vazio após `(codigo_dominio or '').strip()` (`app/folha/export.py:840`, `app/folha/export.py:872`). O layout exige linha 43 caracteres e matrícula de dez posições com `.isdigit()` (`app/folha/export.py:273`, `app/folha/export.py:277`). `partes_da_linha` repete comprimento e matrícula numérica (`app/folha/export.py:247`). O PDF repete comprimento, constante `11` e matrícula numérica (`app/folha/pdf_export.py:112`). Contagem defensável para **juízos de entrada/forma: 4 sítios**; não confundir com as verificações adicionais de mera completude (`app/api/views_mensageria.py:1281`; `app/colaboradores/services/cadastro.py:71`, `app/colaboradores/services/cadastro.py:83`; `app/templates/colaboradores/admitir.html:204`; `app/templates/colaboradores/editar.html:298`).

- **O layout não verifica duplicidade.** Não há consulta de colaboradores nem conjunto de matrículas no corpo do validador (`app/folha/export.py:266`). Também não reconhece vazio original: `montar_linha_10('')` preenche com zeros; `zfill(10)` não trunca código longo (`app/folha/export.py:36`). Assim, “repetido seria recusado pelo validar_layout” não é sustentado pelo código; política desejada de unicidade: **[não verificado]**.

- **A recusa derruba a geração inteira:** a view retorna erro se houver qualquer erro de layout, antes de produzir conteúdo; o command lança `CommandError` (`app/folha/views.py:275`, `app/folha/views.py:280`; `app/folha/management/commands/regerar_txt_dominio.py:89`).

- **Proposta: “lei 0” para este PREPARA**, limitado ao censo e à reconciliação do contrato existente, sem escolher nova política de duplicados, motivo ou destino individual. Coerência já é lei; consumidor deve ler autoridade; regra nova não pode ser deduzida só do código (`app/docs/LEIS.md:122`, `app/docs/LEIS.md:95`, `app/docs/LEIS.md:136`). O corte PADRAO-NA-CURA já nomeia juiz único como padrão estrutural (`app/docs/CORTES.md:378`). Se a construção criar autoridade nova, há exigência documental própria de frase de nascimento; isto não equivale automaticamente a lei de negócio ausente (`app/docs/LEIS.md:69`).

## 17/11 09:25 · sessao 46e6a122e364 · - · - · origem=Ronald (tmux)

Escritores primários: **2**, contando origens que atribuem um novo `codigo_dominio`:

1. **Admissão:** lê `codigo_folha` com `.strip()` em `app/colaboradores/views.py:1131`; se não vazio, atribui em `:1232` e persiste por `salvar_campos` em `:1237`.
2. **Edição:** lê e aplica `.strip()` em `app/colaboradores/views.py:1353`; só atribui se não vazio em `:1355-1356`; persiste por `salvar_inteiro` em `:1365`. Portanto, enviar vazio preserva o código anterior. `app/colaboradores/views.py:1353-1365`

Persistências e regravações que precisam constar separadamente, sem inflar as duas origens:

- `salvar_campos` executa `save(update_fields=nomes)`: `app/colaboradores/services/colaborador.py:140-148`.
- `salvar_inteiro` executa `save()` completo, incluindo o código carregado: `app/colaboradores/services/colaborador.py:152-160`. Chamadores concretos: vincular posto (`app/colaboradores/views.py:345`), editar cadastro (`:1365`), transferir posto (`:1418`) e desligar (`app/colaboradores/services/desligamento.py:133`).
- Reativação também faz `save()` completo: chamador `app/colaboradores/services/desligamento.py:247`; persistência `app/colaboradores/services/colaborador.py:200`.
- `criar_colaborador` usa `objects.create(**campos)`: `app/colaboradores/services/colaborador.py:204-206`. A admissão primeiro cria sem código em `app/colaboradores/views.py:1204`, recebendo o default vazio do modelo (`app/colaboradores/models.py:312`), e depois atribui o código. O comando `smoke_chamados` cria fixture sem código em `app/chamados/management/commands/smoke_chamados.py:78-80`.
- Portas genéricas também capazes de escrever o campo: `gravar` (`app/colaboradores/services/colaborador.py:126-136`) e `atualizar_em_lote` (`:164-181`), permitido por `OPERACOES['cadastro']` em `:46`. Não encontrei chamador que lhes passe `codigo_dominio`; completude dessa ausência é **[nao verificado]**.
- Migration `0026` adiciona o campo com `default=''`, não contém `RunPython`: `app/colaboradores/migrations/0026_colaborador_codigo_dominio.py:12-18`.
- Django admin é somente leitura: `ColaboradorAdmin(SoLeitura, ...)` em `app/colaboradores/admin.py:48`; bloqueio de add/change/delete e readonly de todos os campos em `app/core/admin.py:59-75`.
- Aliases usados para devolver formulários recusados alteram apenas memória: `app/colaboradores/views.py:1117`, `:1282`; implementação explicitamente sem salvar em `app/core/reexibir.py:21-49`.
- Importação Nexti encontrada escreve somente telefone/email: `app/colaboradores/management/commands/importar_telefones_nexti.py:85-113`; não escreve o código nesse caminho.

Validação:

- Modelo: `CharField(max_length=20, blank=True, default='')`, sem validator específico, sem unicidade; `app/colaboradores/models.py:311-314`. `Colaborador.Meta` só declara nomes e ordenação: `:400-403`. O `save` normaliza CPF e matrícula Hasner, sem validar código: `:415-439`.
- HTML das duas telas usa `type="text"`, sem `pattern` ou `maxlength`: `app/templates/colaboradores/admitir.html:58`; `app/templates/colaboradores/editar.html:62`.
- JS ainda marca `codigo_folha` como obrigatório nas duas telas: `app/templates/colaboradores/admitir.html:204-205`; `app/templates/colaboradores/editar.html:298-299`.
- Backend permite salvar sem código: obrigatórios são nome/CPF, com dispensa do CPF na edição; `app/colaboradores/services/cadastro.py:22`, `:53-65`. Código vazio vira pendência que declara travar folha; `:29-30`, `:74-86`.
- Guarda de tamanho não protege o alias: admissão não inclui o código em `_dados_tam` (`app/colaboradores/views.py:1180-1185`); edição passa POST cru com `codigo_folha` (`:1298-1300`), enquanto helper só examina chaves que coincidam com `f.name` (`app/core/validacao.py:49-53`). Nenhuma dessas duas origens verifica numérico, máximo de 10 dígitos ou duplicidade antes da gravação: `app/colaboradores/views.py:1131`, `:1231-1237`, `:1353-1365`.

Selo relevante: `test_tamanho_campos` só procura chamadas/marcadores da guarda e testa outros campos; não demonstra proteção do código/alias: `app/colaboradores/tests/test_tamanho_campos.py:41-51`, `:76-82`.

## 17/11 09:24 · sessao 7dc300d0891f · - · - · origem=Ronald (tmux)

1. **Mordida do OR: sim, por inspeção.** O SQL da versão anterior não contém o trecho exigido `"chamados_perguntadisputa"."id" IN (SELECT`; a face atual usa `Exists`, e o teste exige literalmente esse trecho. Portanto o mutante OR morre na asserção de forma, mesmo conservando o conjunto. Execução Django [nao verificado]. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:493`; `app/chamados/models.py:3503`; `app/chamados/models.py:355`.)

2. **Mordida da projeção: não.** Preservando a subquery e substituindo a face por `Q(validada_em__isnull=True)`, o teste novo passa por inspeção: cria apenas uma pergunta sem carimbo e outra com `validada_em=T2`, sem registrar decisões divergentes. A fixture cria perguntas diretamente, e a face também classifica essa segunda como passivo validado. Execução Django [nao verificado]. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:484`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:490`; `app/chamados/tests/cortes_f2_f9_apoio.py:58`; `app/chamados/models.py:3505`.)

3. **A suíte já contém proteção de negócio contra esse mutante**, em M13: pergunta validada somente no ato, projeção vazia, precisa ficar fora de `_ancoradas`. Portanto a lacuna é do teste novo isolado; não significa que o mutante sobreviva à suíte inteira. Execução Django [nao verificado]. (`app/chamados/tests/test_cortes_f2_f9_m13.py:38`; `app/chamados/tests/test_cortes_f2_f9_m13.py:43`; `app/chamados/tests/test_cortes_f2_f9_m13.py:45`.)

4. **Regressão concreta na certificação do mutante M13:** `_incluir_validadas` exige exatamente um `PerguntaDisputa.objects.filter`, mas agora encontra dois, nas linhas 355 e 357. Executei somente AST/helper extraído em memória com `python3 -B`; resultado: `RuntimeError: mutante m13_inclui_validadas sem filtro unico; isto nao e mordida`. O caminho com `CORTES_F2_MUTANTE=m13_inclui_validadas` falha antes da asserção de negócio. A execução normal sem seletor não aplica o helper. (`app/chamados/models.py:355`; `app/chamados/models.py:357`; `app/chamados/tests/test_cortes_f2_f9_m13.py:25`; `app/chamados/tests/test_cortes_f2_f9_m13.py:31`; `app/chamados/tests/cortes_f2_f9_apoio.py:84`; `app/chamados/tests/cortes_f2_f9_apoio.py:93`.)

5. **CORPO atualizado corretamente:** comparação AST em memória confirmou os nove tuplos; os sete posteriores a `_ancoradas` correspondem ao deslocamento +4, enquanto `transicionar:205` e `_gravar_estado:302` permanecem correspondentes. `EXCECOES` continua vazio; não houve ampliação nominal do corpo. (`app/chamados/tests/e4b_c12_ast.py:18`; `app/chamados/tests/e4b_c12_ast.py:29`; chamadas em `app/chamados/models.py:205`, `:302`, `:462`, `:667`, `:847`, `:865`, `:1501`, `:1705`, `:1725`.)

6. **Escopo do selo de custo:** ele cobra uma consulta, dois resultados esperados e um fragmento textual; não mede tempo, custo, JIT nem plano. As medições mencionadas no comentário permanecem [nao verificado]. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:476`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:487`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:493`.)

## 17/11 09:24 · sessao 7adad91dcf6e · - · - · origem=Ronald (tmux)

1. **[bloqueia] O delta invalida o mutante M13 existente.** `_incluir_validadas` exige exatamente um `PerguntaDisputa.objects.filter` no método; a nova versão contém dois. A transformação levanta `RuntimeError` antes de exercitar a asserção de negócio. Reproduzi executando apenas o transformador AST em memória, sem Django, banco ou escrita. Fontes: `app/chamados/tests/test_cortes_f2_f9_m13.py:25-33`; `app/chamados/models.py:355-357`; `app/chamados/tests/cortes_f2_f9_apoio.py:90-100`. O próprio apoio diz que erro de aplicação não conta como mordida: `app/chamados/tests/cortes_f2_f9_apoio.py:81-82`. Sem seletor, o GREEN não detecta isso: `app/chamados/tests/cortes_f2_f9_apoio.py:84-87`.

2. **[nota] Censo dos cinco chamadores diretos de produção:**

   - `_gravar_estado`: `app/chamados/models.py:229`.
   - `_vivas_ancoradas`: `app/chamados/models.py:341`.
   - `_plano_pergunta_viva`: `app/chamados/models.py:393`.
   - `_perguntas_relavradas_ou_ressuscitadas`: `app/chamados/services/disputa_emissao.py:1821`.
   - `_resolver_perguntas_por_chamado_resolvido`: `app/chamados/services/disputa_emissao.py:1962`.

   Chamadas diretas nos testes: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:411,488`; `app/chamados/tests/test_cortes_f2_f9_m13.py:45,54`.

3. **[nota] Os chamadores recebem lista pronta, preservando materialização e relações carregadas.** O `list(...)`, `select_related('disputa', 'disputa__colaborador', 'disputa__colaborador__empresa', 'celula')` e `order_by('pk')` continuam no retorno: `app/chamados/models.py:357-359`. Nenhum chamador direto encadeia `select_for_update` nesse retorno; eles consomem/combinam a lista: `app/chamados/models.py:229-230,341,393-395`; `app/chamados/services/disputa_emissao.py:1821-1824,1962-1965`.

4. **[nota] A query de leitura não adquire as travas do protocolo.** As travas são explícitas e separadas em `trava_na_ordem`, com `SELECT FOR UPDATE` de colaborador, disputas, chamados e perguntas: `app/chamados/models.py:2765,2802,2811,2824,2828`. O escritor de estado chama essa porta antes do save: `app/chamados/models.py:295-302`; o wrapper do S143 trava antes do corpo que chama `_ancoradas`: `app/chamados/services/disputa_emissao.py:1901-1903,1962`. O delta não altera esses pontos: `app/chamados/models.py:355-359`.

5. **[nota] O reaproveitamento da lista gate→núcleo permanece.** O plano guarda `(novo, lista)`; o núcleo recupera e só busca novamente se não houver cache compatível: `app/chamados/models.py:393-394,222-230`. A subquery é passada como QuerySet ao filtro externo, sem materialização própria; existe apenas o `list` externo: `app/chamados/models.py:355-359`. Para chamado sem PK, continuam zero queries: `app/chamados/models.py:348-349`. A contagem efetivamente executada nesta cópia é [nao verificado].

6. **[nota] Há selos que dependem da contagem de queries.** O novo teste exige uma query: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:487-489`; o selo do chokepoint mede `reconciliar` sem perguntas e cruzando a fronteira com três perguntas: `app/ponto/tests/test_selo_chokepoint_escrita.py:360-371`. O histórico do teto identifica `_ancoradas` como um SELECT adicional por S143: `app/ponto/tests/test_selo_chokepoint_escrita.py:116-124`. Resultado desses selos nesta execução: [nao verificado].

## 17/11 09:23 · sessao a78558220a8f · QUEIMA-UTIL-1711 · sonda · origem=Code

- **col265:** em **2026-10-31**, a sonda mostra `passivo_sem_linha`, célula 135811 e `fio='nao_sei'`. [nao verificado] — saída fornecida, sem arquivo:linha.
- **col838:** **nao reproduz hoje**: nenhuma linha de dia `nao_sei` na saída fornecida. [nao verificado]
- **col407:** em **2026-10-21, 2026-10-29 e 2026-11-02**, a sonda mostra `passivo_sem_linha`, respectivamente nas células 133662, 133670 e 133674. [nao verificado] — saída fornecida, sem arquivo:linha.

Para **todos os dias listados de col265 e col407**, o ramo identificado é **decisão/passivo**: `_pela_decisao` devolve `('passivo_sem_linha', None)`; o chamador copia a razão e marca `nao_sei=True`. (`app/chamados/services/tarefa_do_dia.py:154`, `app/chamados/services/tarefa_do_dia.py:155`, `app/chamados/services/tarefa_do_dia.py:334`, `app/chamados/services/tarefa_do_dia.py:339`)

O registro faltante é **resposta e/ou validação na sequência E1**, por correspondência de ato e instante. O código liga explicitamente `passivo_sem_linha` a `registrar_decisao`, executado por `DecisaoPergunta.registrar(...)`. (`app/chamados/models.py:4098`, `app/chamados/models.py:4102`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:98`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`)

Portanto, o escritor pendente identificável é **o registro da decisão E1**. A razão apresentada não identifica falha de `gerar_celulas`, cartório/ata ou lavra do veredito: o teste de lavra do veredito ocorre depois do retorno por passivo. (`app/chamados/services/tarefa_do_dia.py:154`, `app/chamados/services/tarefa_do_dia.py:167`) Qual chamada histórica deixou essa lacuna, e se faltou resposta, validação ou ambas, é **nao recuperavel pela saída**. [nao verificado]

ORIGEM: col265 decisão/passivo_sem_linha → registro E1, DecisaoPergunta.registrar (`app/chamados/management/commands/backfill_fio_morto_e4b.py:141`); col838 nao reproduz hoje [nao verificado]; col407 decisão/passivo_sem_linha → registro E1, DecisaoPergunta.registrar (`app/chamados/management/commands/backfill_fio_morto_e4b.py:141`).

## 17/11 09:23 · sessao 0dbb64bf6991 · - · - · origem=Ronald (tmux)

Recomendo **item 1 CONFERE; item 3 CONFERE**, como instruções de construção condicionadas à prova exigida, sem confundir ausência futura de RED/delta com premissa falsa. (`CLAUDE.md:28`; `CLAUDE.md:37`; `CLAUDE.md:372`.)

- **Veto existente:** `veto_de_competencia(empresa, dias, memo)` recebe empresa, coleção de dias e cache; devolve `None` se houver dia desconhecido, `True` se alguma competência estiver entregue, `False` nos demais casos. Não seleciona perguntas nem restringe motivos/famílias. (`app/chamados/services/fio.py:1357`; `app/chamados/services/fio.py:1365`.)
- **Mesmo K6, outra pergunta que C21:** compõe dia → janela da empresa → `competencia_entregue`; C21 julga instante **e** texto da resposta. Consultar K6 no escritor não duplica C21. (`app/chamados/services/vigia_fio.py:786`; `app/chamados/services/vigia_fio.py:802`; `app/core/juizes.py:306`; `app/core/juizes.py:343`; `app/chamados/juizes.py:1552`.)
- **Armadilha literal concreta:** em `[None, dia_ENTREGUE]`, o veto devolve `None` antes de consultar K6; `if veto:` permitiria continuar. O padrão existente `is not False` recusa também competência desconhecida, sem novo juiz. (`app/chamados/services/fio.py:1365`; `app/chamados/services/fio.py:1116`; `app/chamados/services/fio.py:1118`.)
- **Passar coleção reiterável:** os dois `any` percorrem `dias`; um generator sem `None` seria consumido pelo primeiro e entregaria vazio ao segundo. Usar lista/tupla evita essa perda de universo. (`app/chamados/services/fio.py:1365`; `app/chamados/services/fio.py:1367`.)
- **Não copiar fora163:** `_k6_que_barra` consulta somente metades de intervalo fora163; perderia outras perguntas alcançadas. (`app/chamados/services/materializacao.py:792`; `app/chamados/services/materializacao.py:805`.)
- **Suficiência continua exigível:** a seleção deve cobrir também relavra sem filtro de materialização/disputa aberta e perguntas sem destino em outras disputas abertas do colaborador; limitar por destino ou pela disputa original não fecha o literal “TODAS”. (`app/chamados/models.py:309`; `app/chamados/services/disputa_emissao.py:2048`; `app/chamados/services/disputa_emissao.py:2057`.) Prova do futuro delta: **[nao verificado]**.
- **Item 3:** fluxo verificador → suite inteira → opus final confere; “só do delta” precisa conservar o alcance dos escritores/leitores das funções e campos tocados, mesmo fora do diff. (`CLAUDE.md:372`; `CLAUDE.md:378`; `CLAUDE.md:379`.) [nao verificado]

## 17/11 09:23 · sessao b4ac020c69a9 · - · - · origem=Ronald (tmux)

Item 6 — **PROCEDE quanto à suficiência literal da medida**. As condições são instrumentáveis, mas nenhuma está demonstrada pelo dado fornecido: opus final do delta, hash de `FechamentoMensal` exportado e resultado do item 4 **[nao verificado]**. A regra exige condição literal fechada, não mera ausência de movimento financeiro (`CLAUDE.md:37`, `CLAUDE.md:265`).

O obstáculo concreto é o pós-commit: `fechar` salva a disputa (`app/chamados/models.py:2027`); o receiver agenda retomada por `on_commit` (`app/chamados/signals.py:122`, `app/chamados/signals.py:146`). Ela seleciona regularizações confirmadas sem materialização de todo o colaborador (`app/chamados/services/regularizacao_ext.py:219`), pode registrar batidas (`app/chamados/services/regularizacao_ext.py:77`) e chegar ao recálculo de competência exportada (`app/ponto/registro_batida.py:180`, `app/ponto/services/fechamento.py:1790`, `app/ponto/services/fechamento.py:1809`, `app/ponto/services/fechamento.py:1824`). Portanto, savepoint+raise simples não demonstra o efeito completo; comparar depois do rollback tampouco demonstra ausência de escrita tentada. A medida precisa incluir os callbacks reais e seus descendentes e comparar antes do desfazimento (`app/chamados/services/foto_e4b.py:30`, `app/chamados/services/foto_e4b.py:65`).

Já existe fotografia de FM/DiaPago/células, mas seu executor de callbacks **recusa** precisamente a retomada, não a executa: não basta reutilizá-lo e concluir “nada de folha muda” (`app/chamados/services/foto_e4b.py:18`, `app/chamados/services/foto_e4b.py:37`, `app/chamados/services/foto_e4b.py:56`, `app/chamados/services/foto_e4b.py:75`). Há precedente de execução explícita de callbacks em teste (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:556`).

Item 7 — **CONFERE como autorização nominal para continuar e recertificar**, sem conflito textual demonstrado com uma cerca localizada apenas em `_retratar`. A comparação em leitura mostrou `reconciliador.py` idêntico entre esta B2 e `wt-f2-073854`, incluindo o ponto de inserção (`app/chamados/reconciliador.py:520`; `/home/ronald/wt-f2-073854/app/chamados/reconciliador.py:520`). Conflito de merge com o delta futuro **[nao verificado]**.

Existe interação semântica relevante: F2 troca a seleção da relavra de coluna `validada_em` para `q_nao_validada_pelo_ato`, e faz o mesmo em `_ancoradas` (`/home/ronald/wt-f2-073854/app/chamados/models.py:315`, `/home/ronald/wt-f2-073854/app/chamados/models.py:351`). Também muda a retenção para ler `DecisaoPergunta.leitura`, conservando C21 e `materializada_em` da coluna (`/home/ronald/wt-f2-073854/app/chamados/models.py:1945`). A enumeração da cerca precisa acompanhar esse universo após o rebase; editar `models.py` para compartilhá-la criaria interseção de arquivo, mas isso ainda não é um conflito provado (`CLAUDE.md:309`, `CLAUDE.md:320`).

## 17/11 09:23 · sessao cb0cd920166d · - · - · origem=Ronald (tmux)

`passivo_sem_linha` identifica **projeção de resposta/validação sem a linha correspondente da sequência E1** (`app/chamados/models.py:4098`, `app/chamados/models.py:4102`).

- `_pela_decisao` retorna `('passivo_sem_linha', None)` em `app/chamados/services/tarefa_do_dia.py:154`; o chamador grava a razão e levanta `nao_sei` em `app/chamados/services/tarefa_do_dia.py:334` e `app/chamados/services/tarefa_do_dia.py:338`.
- A ligação explícita ao escritor existe: `passivo_sem_linha` → `registrar_decisao` em `app/chamados/management/commands/backfill_fio_morto_e4b.py:98`; execução por `DecisaoPergunta.registrar(linhas_da_projecao_sem_linha(ps))` em `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`.
- A porta única que escreve essa sequência é `DecisaoPergunta.registrar`, usando `bulk_create` (`app/chamados/models.py:4110`, `app/chamados/models.py:4120`).
- O ramo de lavra do veredito é outro: `veredito_sem_lavra_vigente` em `app/chamados/services/tarefa_do_dia.py:167`; `passivo_sem_linha` retorna antes dele (`app/chamados/services/tarefa_do_dia.py:154`).
- A saída fornecida permite identificar a falta de registro E1, mas não discrimina se falta resposta, validação ou ambas, nem qual escritor histórico originou a lacuna: **não recuperável [não verificado]**.

## 17/11 09:22 · sessao 463517ba4a9f · - · - · origem=Ronald (tmux)

Remedida dos leitores e autoridades:

| Citação do brief | Neste HEAD | Resultado |
|---|---|---|
| `ListaDoAdmin:188-255` | `app/chamados/services/vigia_fio.py:166` | Classe preservada; `dia_do_admin:200`, `dia_na_lista:208`, `colab_na_lista:226`. |
| `dono_fora_do_fio:274-315` | `app/chamados/services/vigia_fio.py:240` | Continua consultando a lista; balde `fio_do_admin` em `:261-262`. |
| `censo_alcance:542` | `app/chamados/services/vigia_fio.py:487` | Universo via `dias_que_seguram:537`; perguntas vivas `:566`, `ps:597`; causa d `:638-639`; causa e de dono ausente `:642-645`; contador `:656-659`; rótulos `:483`. |
| Chamadores do censo | `app/chamados/management/commands/vigiar_fio_morto.py:175`, `:181`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:78` | Conferem. |
| `porta_da_tarefa:638`, `carga_da_tarefa:648` | `app/chamados/services/tarefa_do_dia.py:712`, `:728` | Nomes e contrato preservados; desconhecimento sem rota em `:719-722`. |
| `alimentar_portas:1595` | `app/chamados/juizes.py:1616` | Nome preservado. |
| `dias_que_seguram:109` | `app/folha/fora_da_folha.py:125` | Universo sem consultar porta; contrato em `:126-139`. |
| Juiz e6 | `app/ponto/management/commands/e6_oraculo.py:53` | Definição começa em 53; corpo decisório continua `:98-110`. `InsumosDoDono:113`, `.dono:182`, `sem_motor` sem CXR retorna `None:193-194`. |
| Gate e6 | `app/ponto/management/commands/e6_oraculo.py:351` | Continua descartando zero nos dois lados em `:351-353`; distinto da montagem de dono na lista em `app/escala/services/cadastro_realidade.py:510`. |
| Consumidores do beco | `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:82`, `:105`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:51`, `:54` | Conferem. |
| Chave C5 em pergunta_no_app | `app/chamados/services/pergunta_no_app.py:35` | Confere. |
| Motor: sentidos de dia | `app/ponto/motor_calculo_v2.py:1528`, `:1623`, `:1997` | Conferem os sítios mencionados. |
| Padrão lâmpada | `app/docs/GLOSSARIO.md:13` | Confere. |
| Selo de custo | `app/escala/tests/test_f3_beco1_folga_na_lista.py:395` | Teste citado existe. |

Duas inexatidões materiais do brief:

- Os leitores diretos não leem ambos o dicionário por dia: `contador_nunca_bateu` expande seus pares em `app/chamados/management/commands/disparar_perguntas_competencia.py:56-57`; `vigia_retido_calado.censo` usa somente o conjunto de colaboradores, `app/folha/vigia_retido_calado.py:71-72`.
- O agregado `por_ass['C5']['ocorrencias']` existe em `app/escala/services/cadastro_realidade.py:946-950`, mas a etiqueta da tela lê `colabs`, `app/templates/escala/cadastro_x_realidade.html:19`. As linhas da tela/PDF mostram `detalhe` e só usam `ocorrencias` como alternativa, respectivamente `app/templates/escala/cadastro_x_realidade.html:28` e `app/escala/services/cadastro_realidade.py:1090-1092`. Portanto não procede afirmar que esse agregado já determina o número mostrado no PDF/tela; a mudança efetiva será sobretudo multiplicar assinaturas e trocar seus detalhes, conforme a implementação futura [nao verificado].

O recorte local da LU2-07 continua por construir: `censo_alcance` usa `empresas_visiveis()` em `app/chamados/services/vigia_fio.py:517-520`; essa consulta filtra apenas ativa/CNPJ em `app/colaboradores/queries.py:85-87`. Cadastro e rótulo continuam `app/colaboradores/models.py:45` e `app/templates/colaboradores/empresa_form.html:35`; terceiro sítio manual em `app/config/crons.py:17`. Alteração global alcançaria o lavrador em `app/folha/management/commands/lavrar_retrato_folha.py:51`.

C11 continua exigindo perguntas vivas, `app/escala/services/cadastro_realidade.py:464-472`, e o ramo d continua exigindo ausência delas, `app/chamados/services/vigia_fio.py:638`; a disjunção D1 persiste.

`competencia_ja_paga` segue obra aberta em `app/docs/BACKLOG.md:1992`; implementação com esse nome não encontrada [nao verificado]. Existem a composição de entregue, `app/chamados/services/vigia_fio.py:785-807`, o juiz `competencia_entregue`, `app/folha/export.py:556`, e o veto `app/chamados/services/fio.py:1353-1363`.

## 17/11 09:22 · sessao 6d9ab2b82ba4 · - · - · origem=Ronald (tmux)

Não há `.git` nesta cópia; não executei git. `logs/CLASSE-D/brief/brief.md` também não está nela. Os cruzamentos abaixo usam exclusivamente os anexos transcritos; sua correspondência aos commits não pode ser autenticada aqui [nao verificado].

| Escopo do brief | Arquivo | B2+delta | F2 |
|---|---|---:|---:|
| R1 | `app/escala/services/cadastro_realidade.py` | não | não |
| RED R1 | teste em `app/escala/tests/`, nome não definido [nao verificado] | indeterminado pelo nome; diretório ausente das listas fornecidas [nao verificado] | idem [nao verificado] | [nao verificado]
| Complemento LU2-07, recorte local no vigia | `app/chamados/services/vigia_fio.py` | **sim** [nao verificado: lista anexa] | **sim** [nao verificado: lista anexa] |
| LU2-08, caso incorporado | `app/ponto/management/commands/e6_oraculo.py` | não [nao verificado: lista anexa] | não [nao verificado: lista anexa] |
| RED dos complementos | nomes não definidos [nao verificado] | [nao verificado] | [nao verificado] |

O sítio atual do recorte é `censo_alcance` em `app/chamados/services/vigia_fio.py:487`, que obtém `empresas_visiveis()` em `:517` e intersecta o parâmetro `empresas` em `:518`. Essa consulta global filtra `ativa` e CNPJ, sem consultar `em_rollout`: `app/colaboradores/queries.py:85`.

**Contagem nominal conhecida do escopo integral:** B2+delta **1**, F2 **1**; arquivo comum `app/chamados/services/vigia_fio.py` [nao verificado: cruzamento calculado dos anexos]. R1 isolado teria **0/0**, mas omitir o complemento LU2-07 do brief produziria conclusão incompleta [nao verificado: escopo do anexo].

O delta explicitado só acrescenta `app/chamados/reconciliador.py` e testes sem nome fornecido; esse escritor não pertence ao escopo R1/complemento local do brief [nao verificado: anexos]. Seu sítio existe: `_retratar`, `app/chamados/reconciliador.py:504`; a transição está em `:524`. A identidade dos arquivos de testes do delta fica [nao verificado].

A tela não obriga outro arquivo: a etiqueta agregada imprime **colaboradores**, `a.colabs`, em `app/templates/escala/cadastro_x_realidade.html:19`. O detalhe imprime `a.detalhe`, com `a.ocorrencias` apenas como fallback, em `:28`. O PDF está no próprio produtor, `app/escala/services/cadastro_realidade.py:1053`, e usa a mesma preferência detalhe/fallback em `:1090`. Logo, o novo detalhe pode dizer “dia” no produtor; a afirmação do brief de que mudar `por_ass['C5']['ocorrencias']` muda o contador visível da tela não confere literalmente: o agregado é montado em `app/escala/services/cadastro_realidade.py:949`, mas a tela lê `colabs` em `app/templates/escala/cadastro_x_realidade.html:19`.

A pergunta de pouso R1 isolado já tem resposta registrada: `app/docs/RELATO.md:421` diz decisão técnica de pousar sozinho, sem esperar LU2-07/08, mantendo D1/D2 em espera. O complemento LU2-08 e sua pergunta sobre precedência do insumo frente a BATIDA estão registrados em `app/docs/RELATO.md:422`.

`app/config/crons.py` não deve entrar na interseção desta obra: o anexo manda apenas registrar sua futura migração, não fazê-la [nao verificado: anexo]. O literal continua em `app/config/crons.py:17`.

## 17/11 09:22 · sessao e6d2517028d1 · - · - · origem=Ronald (tmux)

Item 1: o ponto correto é dentro de `_retratar` no `transaction.atomic()` de `app/chamados/reconciliador.py:524`, antes de `transicionar_com_pauta` em `:525`. O bloco reúne transição, rejeição da justificativa, resolver e ocorrência (`app/chamados/reconciliador.py:525`, `:531`, `:538`, `:540`). A retenção anterior fica fora dele e pode registrar auditoria antes da cerca (`app/chamados/reconciliador.py:520`, `:522`).

`transicionar_com_pauta` está em `app/chamados/services/fecho_runtime.py:146`; recebe `ato=ch.reconciliar`, abre atomic próprio, coleta recusas e pode escrever Pautas antes de devolver o resultado (`app/chamados/services/fecho_runtime.py:157`, `:161`, `:165`, `:168`). Logo, barrar depois dela seria tarde também para Pauta (`app/chamados/services/fecho_runtime.py:168`).

A enumeração suficiente não é só `ch._ancoradas()`:

- Diretas do resolver: destino igual ao chamado, materialização nula, disputa aberta; inclui validadas (`app/chamados/services/disputa_emissao.py:1928`, `:1950`).
- Ancoradas: não validadas, destino igual ao chamado OU destino nulo com pai da disputa igual ao chamado (`app/chamados/models.py:336`, `:344`).
- Relavra: destino igual ao chamado, validação nula; não filtra materialização nem disputa aberta (`app/chamados/models.py:303`, `:309`).
- S143 inclui todas as disputas abertas do colaborador, alcançando perguntas sem destino de outras disputas órfãs; pode carimbar e fechar essas disputas (`app/chamados/services/disputa_emissao.py:2048`, `:2057`, `:2082`, `:2113`).
- Fechar disputa pode reconciliar seu pai e disparar outra rodada de S143/relavra (`app/chamados/signals.py:19`, `:43`; `app/chamados/models.py:296`, `:309`).
- Reabrir credor também pode relavrar/ressuscitar perguntas; existe enumerador desse ramo, que não constitui enumerador de toda `_retratar` (`app/chamados/services/disputa_emissao.py:1814`, `:1820`, `:1969`).

Assim, o literal “TODAS” requer prova da cobertura da cascata; um único filtro por destino ou por disputa do chamado é insuficiente (`app/chamados/services/disputa_emissao.py:2048`, `:2050`). Suficiência do futuro delta: [nao verificado].

Item 2(b): é montável conceitualmente por relavra, sem depender da criação de batida: pergunta ENTREGUE com destino=chamado, `validada_em=NULL`, `materializada_em` preenchida e disputa já fechada. Ela está fora do resolver direto e da pré-vassoura do consumidor, mas dentro da relavra (`app/chamados/services/disputa_emissao.py:1928`; `app/chamados/reconciliador.py:629`; `app/chamados/models.py:309`). Na travessia vivo→resolvido a relavra escreve `julgado_em` mesmo se o veredito permanecer igual; relógio congelado posterior ao snapshot torna esse efeito observável (`app/chamados/models.py:273`, `:303`; `app/chamados/veredito_lavrado.py:270`, `:275`). Competência entregue não bloqueia por si o gate de vivas (`app/chamados/models.py:364`). Execução RED/mutante desse cenário: [nao verificado].

Outro cenário indireto é a pergunta sem destino de uma disputa órfã diferente, mesmo colaborador: S143 a alcança pela varredura global de disputas abertas (`app/chamados/services/disputa_emissao.py:2048`). A fixture existente já constrói escolha sem destino, prova C6 morto e espera carimbo/fecho da órfã; falta adaptar essa pergunta para competência entregue e para a entrada `_retratar` (`app/chamados/tests/test_e4b_c6_orm.py:62`, `:82`, `:86`, `:120`). Execução adaptada: [nao verificado].

Limite literal importante: “evento … nada muda” não pode significar todo o consumidor. Antes de `_retratar`, ele já rejunta células e carimba perguntas absolvidas; depois roda órfãs e lastro (`app/chamados/reconciliador.py:1366`, `:1368`, `:1371`, `:1376`, `:1385`). A cerca de `_retratar` garante apenas sua unidade; teste via evento deve controlar esses outros escritores e medir a fronteira correta (`app/chamados/reconciliador.py:524`, `:1368`).

Limite de atomicidade: callbacks `on_commit` de retomada de regularizações e fechamento de becos são posteriores à transação; uma sonda que termina em rollback não os executa (`app/chamados/signals.py:122`, `:146`, `:229`, `:240`). A unidade SQL não é garantia automática sobre esses efeitos posteriores (`app/chamados/signals.py:129`, `:146`).

## 17/11 09:21 · sessao d9205bf7e500 · CONTADOR-CODEX-OCIOSO · verificador · origem=Code

1. **(a) está implementado, mas não num sítio só; (b) só está comprovado no leitor.**

   `_em_curso` exige `ini` sem `fim` da mesma chave e PID vivo. `ultimas_chamadas` repete essa decisão, sem chamar `_em_curso`, contrariando sua própria documentação de predicado único. Ambos conferem o PID; nesses leitores de chamadas não permanece o antigo rótulo incondicional. (`bin/codex_limite_estado.py:85`, `bin/codex_limite_estado.py:111`, `bin/codex_limite_estado.py:131`)

   O leitor da fila exige data+hora em ABERTO; ausência, data inválida ou futuro produzem SEM DADO. Isso não prova que a linha **nasça** carimbada: o escritor real da fila e seu procedimento de abertura são [nao verificado]. (`bin/codex_limite_estado.py:45`, `bin/codex_limite_estado.py:159`, `bin/codex_limite_estado.py:173`)

2. **Escritores, leitores e significados não formam um contrato uniforme.**

   | Campo/fonte | Quem escreve | Quem lê |
   |---|---|---|
   | `ini`: horário, obra, papel, hash, PID | Wrapper grava horário de `date`, hash abreviado e `$$`, antes da espera por slots. (`bin/codex_pergunta.sh:308`, `bin/codex_pergunta.sh:364`) | Parser comum; contagem por papel; últimas chamadas; ociosidade. (`bin/codex_limite_estado.py:82`, `bin/codex_limite_estado.py:98`, `bin/codex_limite_estado.py:107`, `bin/codex_limite_estado.py:194`) |
   | `fim`: horário, mesma chave, sessão, rc, duração | `_saida`, pelo trap EXIT, depois de `INI=1`. (`bin/codex_pergunta.sh:269`, `bin/codex_pergunta.sh:282`) | Parser valida o horário, mas guarda somente sessão/rc/duração; últimas chamadas usam rc; ociosidade usa existência da chave. (`bin/codex_limite_estado.py:82`, `bin/codex_limite_estado.py:89`, `bin/codex_limite_estado.py:111`, `bin/codex_limite_estado.py:133`) |
   | PID | É o PID do wrapper, incluindo espera por vaga/trava; reutilização pelo SO é limitação declarada. (`bin/codex_pergunta.sh:308`, `bin/codex_limite_estado.py:19`) | `_pid_vivo`, chamado separadamente pelas últimas chamadas e pelo contador. (`bin/codex_limite_estado.py:111`, `bin/codex_limite_estado.py:123`, `bin/codex_limite_estado.py:133`) |
   | `fila.md`: estado e data+hora | Escritor demonstrado somente nas fixtures; produtor real: [nao verificado]. (`bin/tests/test_codex_ociosidade.sh:32`) | `ler_fila_reserva`; resultado consumido por `ler_ociosidade`. (`bin/codex_limite_estado.py:136`, `bin/codex_limite_estado.py:203`) |

   Outros consumidores do log são a procedência das sessões, tokens por papel e censo do disjuntor; eles reaproveitam o parser. (`bin/codex_limite_estado.py:220`, `bin/codex_limite_estado.py:315`, `bin/gerar_codex_estado.py:176`, `bin/disjuntor.py:2126`)

   **Fuso:** o formato gravado não contém offset, e o parser produz datetime ingênuo; o contrato declara hora local ingênua. Igualdade efetiva entre os fusos dos produtores e leitores em produção: [nao verificado]. (`bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:308`, `bin/codex_limite_estado.py:27`, `bin/codex_limite_estado.py:82`)

   **30 minutos:** a idade é truncada antes da comparação. Assim, 30min59s vira `30` e não acusa; o primeiro minuto inteiro acusável é 31. Isso não corresponde ao literal de duração `>30 min`. (`bin/codex_limite_estado.py:169`, `bin/codex_limite_estado.py:212`)

   **24 horas:** o contexto novo usa `(agora−24h, agora]`; por papel, a condição inclui exatamente 24h atrás **e qualquer horário futuro**. O censo usa ambas as bordas inclusivas. (`bin/codex_limite_estado.py:199`, `bin/codex_limite_estado.py:99`, `bin/disjuntor.py:2132`)

3. **Não: há erro convertido em zero/ok, e o caminho completo pode derrubar o ESTADO.**

   Linhas de chamadas curtas ou com timestamp inválido são descartadas silenciosamente. Um log inteiro corrompido pode resultar em `em_curso=0`; com fila sem ABERTO, termina `ok`. PID inválido e `OSError` diferente de `PermissionError` também viram “não vivo”, sem SEM DADO. (`bin/codex_limite_estado.py:79`, `bin/codex_limite_estado.py:83`, `bin/codex_limite_estado.py:118`, `bin/codex_limite_estado.py:126`, `bin/codex_limite_estado.py:195`, `bin/codex_limite_estado.py:212`)

   O `try` de `ler_ociosidade` protege suas operações internas, mas `ler()` chama antes `ler_chamadas` e `ultimas_chamadas`, cujas leituras não têm esse tratamento. `chamadas.log` existente e sem permissão pode levantar `PermissionError` antes de chegar à ociosidade. `gerar_estado.py` chama esse conjunto sem proteção local; `relato.sh` encerra com 1 se o gerador falhar. (`bin/codex_limite_estado.py:67`, `bin/codex_limite_estado.py:77`, `bin/codex_limite_estado.py:186`, `bin/gerar_estado.py:270`, `bin/relato.sh:38`)

4. **O executor dos mutantes não garante morte por asserção.**

   Ele considera **qualquer saída** de `_casos`, com stderr agregado, como “mordeu”; não exige `FALHA:` nem separa traceback de comparação divergente. Portanto, uma exceção escapada também certifica falsamente o mutante. (`bin/tests/test_codex_ociosidade.sh:117`, `bin/tests/test_codex_ociosidade.sh:118`)

   Na reprodução em memória dos casos, com E/S e PIDs simulados, os 11 mutantes produziram falhas de comparação; execução integral do selo original no host: [nao verificado]. As comparações estão em `bin/tests/test_codex_ociosidade.sh:42`; as mutações, em `bin/tests/test_codex_ociosidade.sh:122`.

   Há duas ressalvas relevantes:

   - **M2:** enfraquecer `b>=1` faz `None > 30` levantar `TypeError`, capturado como SEM DADO; a comparação contra `ok` mata o mutante por consequência dessa exceção interna. (`bin/tests/test_codex_ociosidade.sh:70`, `bin/tests/test_codex_ociosidade.sh:123`, `bin/codex_limite_estado.py:177`, `bin/codex_limite_estado.py:212`)
   - **M11:** continua retornando SEM DADO; morre somente porque o motivo muda de “chamadas.log ilegivel” para “erro ao ler (PermissionError)”. Não demonstra perda da propriedade SEM DADO. (`bin/tests/test_codex_ociosidade.sh:95`, `bin/tests/test_codex_ociosidade.sh:131`, `bin/codex_limite_estado.py:214`)

   **Vacuidade:** “fila é diretório” não exercita captura de exceção: `isfile` devolve falso e retorna “fila ausente”. Também faltam os casos de 30min59s e início exatamente em `agora`; o teste da janela cobre a borda inferior e futuro, mas não demonstra inclusão da borda superior. (`bin/tests/test_codex_ociosidade.sh:67`, `bin/tests/test_codex_ociosidade.sh:76`, `bin/tests/test_codex_ociosidade.sh:87`, `bin/codex_limite_estado.py:140`)

5. **Classes citadas no pedido:**

   - **Segundo juiz:** demonstrado pela decisão duplicada em `ultimas_chamadas` e `_em_curso`. (`bin/codex_limite_estado.py:111`, `bin/codex_limite_estado.py:133`)
   - **Relógio fora da borda:** no caminho principal da ociosidade, `agora` é capturado uma vez e repassado. Entretanto, log e PIDs são relidos entre componentes, permitindo números e rótulos de observações diferentes; no consumidor do disjuntor, `ler_chamadas(raiz)` ainda captura outro relógio. (`bin/codex_limite_estado.py:49`, `bin/codex_limite_estado.py:67`, `bin/codex_limite_estado.py:107`, `bin/codex_limite_estado.py:194`, `bin/disjuntor.py:2036`)
   - **Derivação paralela:** contagens de inícios por papel e por hora calculam separadamente a mesma janela nominal, com bordas divergentes. (`bin/codex_limite_estado.py:99`, `bin/codex_limite_estado.py:199`)
   - **Campo com dois significados:** o elemento publicado como `rc` contém ora código de saída, ora estado sintético “em curso”/“sem fim”. É sobrecarga já existente em parte, ampliada pelo diff. (`bin/codex_limite_estado.py:111`, `bin/codex_limite_estado.py:391`)

VEREDITO: REPROVA -- predicado duplicado, erro de entrada pode virar zero/ok e a leitura completa pode derrubar o ESTADO (bin/codex_limite_estado.py:111, bin/codex_limite_estado.py:79, bin/codex_limite_estado.py:67; bin/gerar_estado.py:270).

## 17/11 09:21 · sessao 988c3a4c30d7 · - · - · origem=Ronald (tmux)

- `logs/LU2-CORTES/08` não está disponível na cópia; seu conteúdo integral é [nao verificado].
- LU2-08 continua dividida: `PARCIAL_SEM_PERIODO` tem resposta CADASTRO; `FATO_EM_AUSENCIA` continua com alternativas CADASTRO/BATIDA/ESTRUTURA sem corte registrado. Fonte: `app/docs/RELATO.md:352`.
- O complemento atinge `InsumosDoDono.dono()` e o juiz existente. O registro informa 14 parciais ativos, dos quais 10 da CLASSE-D, e alerta que carregar o insumo depois do teste BATIDA muda a classificação; esses números atuais são [nao verificado]. Fonte documental: `app/docs/RELATO.md:422`.
- A precedência da parcial sem período já está respondida pela lei: vai ao admin como CADASTRO, sem julgar a batida dentro/fora e nunca como FATO_EM_AUSENCIA. A referência antiga `CORTES.md:370` passou para `app/docs/CORTES.md:386`.
- Hoje o juiz verifica CADASTRO inicial, depois BATIDA, depois fase/CxR; não recebe insumo específico de parcial sem período. Fonte: `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:104`, `app/ponto/management/commands/e6_oraculo.py:110`, `app/ponto/management/commands/e6_oraculo.py:185`.
- A pergunta do brief sobre R1 pousar sozinho já ganhou decisão técnica: sozinho, sem esperar LU2-07/08; D1/D2 continuam em espera como desenho. Fonte: `app/docs/RELATO.md:421`.
- Para **R1 estrito**, a sugestão é **lei 0**: origem, autoridade única e dono estão escritos em `app/docs/LEIS.md:59`, `app/docs/LEIS.md:60`, `app/docs/LEIS.md:122`; o pouso independente consta em `app/docs/RELATO.md:421`.
- Se incluir a resolução do complemento LU2-08, registrar **lei: qual dono L-099 deve responder por FATO_EM_AUSENCIA**. O próprio registro mantém essa escolha aberta: `app/docs/RELATO.md:352`, `app/docs/RELATO.md:422`.

## 17/11 09:19 · sessao c5a0e7256b78 · ENSAIO-DA-FOLHA-11 · verificador · origem=Code

**AVAL-DEVOLVIDO — itens 1 e 4:** o caminho do lote existe e o defeito do export confere, mas faltam fechar a execução completa de B1 e as condições de abertura da obra lateral. (`app/relatorios/lote_espelho.py:132`; `bin/sombra.sh:490`; `app/folha/export.py:872`; `app/docs/BACKLOG.md:2088`.)

*Referências a `respostas_1711_0915.txt` são ao anexo colado, contando “aval Ronald” como linha 1.*

**Item 1 — B1 na sombra**

1. **As premissas conferem parcialmente.** A view chama `pedir` e `disparar`; este lança o command, que chama `executar` e o mesmo gerador de PDF do lote. O command exige lote previamente criado; não cria o pedido sozinho. (`app/relatorios/views.py:1227`; `app/relatorios/views.py:1246`; `app/relatorios/lote_espelho.py:127`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:44`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:57`; `app/relatorios/lote_espelho.py:205`.)

   **O filho herda as settings da sombra, por inferência do código:** `Popen` não fornece `env`; o container define `DJANGO_SETTINGS_MODULE=config.settings.sombra`; `manage.py` preserva esse valor com `setdefault`. Essas settings fixam banco `sombra`, host `sombra_db`; o subprocesso recebe o schema da conexão do pai. Configuração efetivamente usada numa execução: **[nao verificado]**. (`app/relatorios/lote_espelho.py:126`; `app/relatorios/lote_espelho.py:132`; `bin/sombra.sh:184`; `app/manage.py:9`; `app/config/settings/sombra.py:29`; `app/config/settings/sombra.py:47`; `app/config/settings/sombra.py:49`.)

2. **O novo aval resolve os pontos de autorização anteriores no escopo declarado.** Autoriza função/command sem POST, trava inteira, saída por `/sombra` e as duas operações da lavra. A exigência específica de smoke HTTP em banco `juliani_ref_*` não se aplica à chamada direta proposta; continuam valendo banco da sombra, CPUSET de teste e escopo literal. (`respostas_1711_0915.txt:2`, anexo; `CLAUDE.md:205`; `CLAUDE.md:206`; `CLAUDE.md:87`; `CLAUDE.md:37`.) [nao verificado]

3. **A espera e a retirada do PDF ainda não fecham automaticamente.**

   - **`--rodar` não espera especificamente o filho do `Popen`.** `disparar` abre uma nova sessão e devolve apenas o PID, sem `wait`; `--rodar` executa o script principal dentro de `sh -c`. Não há nesse caminho uma espera adicional pelo job destacado. `start_new_session=True` não demonstra sobrevivência ao encerramento do container `--rm`. (`app/relatorios/lote_espelho.py:132`; `app/relatorios/lote_espelho.py:146`; `bin/sombra.sh:183`; `bin/sombra.sh:191`; `bin/sombra.sh:490`.)
   - **Como segurar a trava inteira:** usar o próprio `--rodar`, que adquire o `flock` antes da lavra, e manter o script em primeiro plano até terminar geração, conferência e retirada do PDF. O descritor 8 permanece aberto no processo do host durante essa chamada. O caminho concreto mais direto é `pedir` seguido de chamada síncrona ao command, com `schema='juliani'`, em vez de destacar o job. Isso é uma receita possível pelo código; script B1 que a implemente: **[nao verificado]**. (`bin/sombra.sh:487`; `bin/sombra.sh:489`; `bin/sombra.sh:490`; `bin/sombra_trava.sh:26`; `bin/sombra_trava.sh:28`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:37`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:57`.)
   - **O PDF não sai automaticamente por `/sombra`.** O gerador grava em `MEDIA_ROOT/cartoes_lote`, dentro do tmpfs. É preciso copiar o arquivo para `/sombra` antes de encerrar o container e depois encaminhá-lo à pasta do ensaio; `/sombra` corresponde a `logs/sombra` do host. (`app/config/settings/base.py:102`; `app/relatorios/models.py:125`; `app/relatorios/lote_espelho.py:250`; `bin/sombra.sh:81`; `bin/sombra.sh:189`; `bin/sombra.sh:190`.)
   - **Conclusão do lote não prova B1 auditável:** o informacional também incrementa `n_ok`, e o lote pode terminar `CONCLUIDO`. Além disso, `--rodar` termina com um `echo` depois da pipeline, sem propagar explicitamente seu resultado; o retorno da porta sozinho não certifica sucesso. (`app/relatorios/services.py:244`; `app/relatorios/services.py:252`; `app/relatorios/lote_espelho.py:251`; `bin/sombra.sh:45`; `bin/sombra.sh:492`; `bin/sombra.sh:493`.)

4. **Há escritas, agora autorizadas no banco sombra, e artefatos a declarar.** A lavra faz dois `update_or_create`, podendo alterar snapshots existentes, e escreve `lavra_prod.json` e `repor_lavra.py`; a porta também escreve `logs/<rotulo>.out`. Não envolver `--rodar` em `--com-a-sombra`: ambas chamam a lavra, duplicando as operações aceitas. (`bin/sombra.sh:144`; `bin/sombra.sh:151`; `bin/sombra.sh:157`; `bin/sombra.sh:160`; `bin/sombra.sh:460`; `bin/sombra.sh:489`; `bin/sombra.sh:492`; `respostas_1711_0915.txt:2`, anexo.)

   O caminho registra lote e auditoria de PDF; não demonstra alteração de dinheiro ou de exportada em produção. Efeito real de uma execução: **[nao verificado]**. A raspagem continua necessária porque o PDF imprime nome e CPF; exportar dado pessoal segue proibido. (`app/relatorios/lote_espelho.py:103`; `app/relatorios/pdf_base.py:152`; `app/relatorios/pdf_espelho.py:739`; `CLAUDE.md:206`; `CLAUDE.md:265`.)

**Item 4 — EXPORT-UM-JUIZ-DO-CODIGO**

1. **O defeito confere como possibilidade do código.** O classificador começa com `entra`, normaliza com `strip` e, nesse crivo, exclui apenas código vazio. A montagem preenche com zeros até dez posições; o validador exige linha de 43 caracteres e campo `matric` numérico. Se os demais crivos permitirem entrada e houver evento gerado, um código não numérico pode provocar recusa do TXT inteiro. Caso real de col1041 e dimensão na frota: **[nao verificado]**. (`app/folha/export.py:840`; `app/folha/export.py:841`; `app/folha/export.py:872`; `app/folha/export.py:728`; `app/folha/export.py:273`; `app/folha/export.py:277`; `app/folha/views.py:275`; `app/folha/views.py:279`.)

   **Precisão para o censo:** esse validador não verifica duplicidade; código numérico curto é preenchido com zeros; vazio já sai pelo classificador. Portanto essas classes não podem ser todas rotuladas como “recusadas pelo layout”. (`app/folha/export.py:266`; `app/folha/export.py:307`; `app/folha/export.py:728`; `app/folha/export.py:872`.)

2. **Registrar e preparar são compatíveis; “juiz único” não autoriza automaticamente registrar outro juiz.** Não encontrei autoridade específica registrada para “código do Domínio válido”; existem `classificar_export` para entrada no TXT e aprovação, também apontado em `JUIZES.md`. Declarar autoridade existente exige medir unicidade, mesmo predicado e universo igual; autoridade nova exige corte. (`app/core/juizes.py:181`; `app/core/juizes.py:182`; `app/core/juizes.py:329`; `app/core/juizes.py:330`; `app/docs/JUIZES.md:118`; `CLAUDE.md:74`; `CLAUDE.md:278`; `CLAUDE.md:314`.)

3. **A abertura futura não fecha apenas com “censo fechado e lei 0”.** O anexo menciona “4 condições” sem enumerá-las. A cópia reserva vaga 2 para obras da espinha, exige cruzamento por arquivo/juiz/vaga e condiciona laterais à disponibilidade; a B2 exemplifica que censo fechado e lei zero não bastam quando há arquivos comuns. Cumprimento das quatro condições e vaga disponível: **[nao verificado]**. (`respostas_1711_0915.txt:5`, anexo; `app/docs/BACKLOG.md:1875`; `app/docs/BACKLOG.md:2088`; `app/docs/BACKLOG.md:824`.)

   A CLASSE-D estava posicionada após F1 e antes de F2; há registro posterior de que F2 pode abrir sem esperá-la. O novo aval pode estabelecer prioridade relativa desta lateral sobre CLASSE-D, mas isso não comprova vaga livre nem permite deslocar a espinha, que o próprio anexo preserva. (`app/docs/BACKLOG.md:1990`; `app/docs/PROMPTS.md:2448`; `respostas_1711_0915.txt:5`, anexo.)

4. **Somente registrar/preparar não autoriza gerar TXT, corrigir cadastro ou aplicar dinheiro.** Usar o gerador como sonda teria escrita: ele pode aprovar fechamentos antes da validação final do layout. Uma futura cura precisa respeitar exportada e os demais `!`; neste aval a construção está expressamente excluída. (`app/folha/views.py:250`; `app/folha/views.py:252`; `app/folha/views.py:275`; `CLAUDE.md:265`; `CLAUDE.md:411`; `respostas_1711_0915.txt:5`, anexo.)

VEREDITO: item 1 PROCEDE; item 4 PROCEDE -- B1 precisa fechar espera e retirada do PDF antes do container sair; o item 4 permite preparar, mas não demonstra as quatro condições nem a disponibilidade da segunda vaga. (`app/relatorios/lote_espelho.py:132`; `bin/sombra.sh:189`; `bin/sombra.sh:490`; `respostas_1711_0915.txt:5`, anexo; `app/docs/BACKLOG.md:2088`.)

## 17/11 09:18 · sessao 0ef5ca9a8648 · - · - · origem=Ronald (tmux)

Item 4 — evidências para consolidação:

1. **Defeito confere como possibilidade do código.** `classificar_export` inicia em `entra`, aplica `.strip()` e só exclui código vazio; não exige número nem limite de dez posições. A montagem usa `.zfill(10)`, que preserva texto e códigos longos; o layout exige registro de 43 caracteres e `matric` numérico. Havendo linha gerada, qualquer erro recusa o TXT inteiro da empresa. (`app/folha/export.py:840`; `app/folha/export.py:841`; `app/folha/export.py:872`; `app/folha/export.py:728`; `app/folha/export.py:36`; `app/folha/export.py:273`; `app/folha/export.py:277`; `app/folha/views.py:275`; `app/folha/views.py:279`.) Ocorrência real do caso col1041 e dimensão na frota: **[nao verificado]**.

   Precisão importante: o layout **não verifica duplicidade**; código numérico com menos de dez posições é preenchido com zeros. Portanto “o validar_layout recusaria vazio, repetido, fora do tamanho” não descreve literalmente todos esses casos: vazio já é excluído pelo classificador; repetido não é recusado por esse validador; tamanho menor é admitido por preenchimento. (`app/folha/export.py:266`; `app/folha/export.py:307`; `app/folha/export.py:728`; `app/folha/export.py:872`.)

2. **Não encontrei juiz registrado para “código do Domínio válido”.** O registro declara `classificar_export` para aprovação e entrada no TXT; a tabela documental também aponta esse juiz, sem autoridade específica para validade do código. Isso não transforma automaticamente o validador de linhas em juiz já registrado desse novo predicado. (`app/core/juizes.py:319`; `app/core/juizes.py:329`; `app/core/juizes.py:330`; `app/docs/JUIZES.md:118`.) Registro de autoridade nova exige corte; a autonomia para declarar autoridade existente exige função já existente, única autoridade medida, mesmo predicado e universo igual. (`CLAUDE.md:74`; `CLAUDE.md:278`; `CLAUDE.md:280`.)

3. **Preparar censo cabe; abertura automática ainda não está demonstrada.** A segunda vaga tem fila: E4C-F1-B2 ocupa a vaga 2 e espera BUG-PAR por cruzar oito arquivos; a ordem anterior também protege R3/F1. Laterais exigem conferir cruzamento por arquivo/juiz/vaga e só andam com a suíte livre. (`app/docs/BACKLOG.md:824`; `app/docs/BACKLOG.md:1875`; `app/docs/BACKLOG.md:2088`.) CLASSE-D estava explicitamente após E4c F1 e antes de F2; o novo anexo pode priorizar esta lateral sobre CLASSE-D, mas não prova disponibilidade nem autoriza deslocar a espinha. (`app/docs/BACKLOG.md:1990`; `app/docs/BACKLOG.md:2265`; anexo `respostas_1711_0915.txt`, item 4 **[nao verificado: arquivo ausente na cópia]**.) As “4 condições” não são enumeradas no anexo; identificá-las e demonstrá-las permanece **[nao verificado]**. Há ainda portão documentado da segunda vaga por carga, horário e arquivos comuns. (`app/docs/BACKLOG.md:2187`.)

4. **Somente registrar/preparar não contém apply de exportada ou dinheiro.** Uma futura execução do gerador não seria leitura: aprova fechamentos antes da validação final do layout; o defeito não autoriza usar esse caminho para medir. (`app/folha/views.py:250`; `app/folha/views.py:252`; `app/folha/views.py:275`.) Exportada, escala/vínculo e regras fora do pedido continuam sob `!`. (`CLAUDE.md:265`; `CLAUDE.md:268`.)

Complemento útil ao censo: o modelo aceita texto de até vinte caracteres, sem validator numérico no campo; a admissão e a edição recebem `codigo_folha` com `.strip()` e gravam o valor não vazio. A porta de persistência confere operação/dono, sem validar o código nessas funções. (`app/colaboradores/models.py:311`; `app/colaboradores/models.py:312`; `app/colaboradores/views.py:1131`; `app/colaboradores/views.py:1231`; `app/colaboradores/views.py:1237`; `app/colaboradores/views.py:1353`; `app/colaboradores/views.py:1365`; `app/colaboradores/services/colaborador.py:140`; `app/colaboradores/services/colaborador.py:148`; `app/colaboradores/services/colaborador.py:152`; `app/colaboradores/services/colaborador.py:160`.)

## 17/11 09:17 · sessao bf379d6c6987 · ENSAIO-DA-FOLHA-11 · leitura · origem=Code

1. **Quem grava:** `folha.porta_export.lavrar()` escreve em **`MetricaSnapshot`**, chave **`porta_export_leitores`**, com escopo `empresa`, `escopo_id=empresa.pk` e `data_ref=hoje`; guarda competência em `detalhe.mes/ano` e falhas em `valor`. (`app/folha/porta_export.py:18`, `app/folha/porta_export.py:768-796`) Quem chama essa gravação é **`selo_leitores_no_mesmo_numero`**, após medir cada empresa ativa, com mês/ano obrigatórios. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27-35`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-92`)

2. **Cron:** o selo está em **`FORA_DE_PIPELINE`**, explicitamente destinado à execução manual antes do export; portanto **não tem horário nem cobertura automática da aberta 11/2026**. (`app/config/crons.py:769`, `app/config/crons.py:887-897`) O universo geral do cron é empresas `(2, 3, 4)`, schema `juliani`; o comando do selo percorre todas as empresas ativas do tenant e mede exatamente a competência passada em `--mes/--ano`. (`app/config/crons.py:17-18`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:35`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-92`) Apesar do comentário dizer “roda no ATO do export”, o POST **somente confere**. (`app/config/crons.py:893-894`, `app/folha/views.py:128-133`) Crontab instalado e execução efetiva de hoje: [nao verificado].

3. **O que o admin vê:** sem carimbo, `gerar_dominio_txt` interrompe o fluxo, passa a recusa como erro e redireciona para a tela de exportação, que exibe a mensagem em alerta. (`app/folha/views.py:128-133`, `app/core/respostas.py:50-51`, `app/core/respostas.py:68-71`, `app/templates/folha/exportar.html:10`) A mensagem manda literalmente **`Rode: docker exec saas_core python manage.py tenant_command selo_leitores_no_mesmo_numero --schema=juliani --mes 11 --ano 2026`**; não orienta uma ação disponível na tela. (`app/folha/porta_export.py:809-813`) **Não há botão/rota para produzir esse carimbo** no fluxo: há seleção, Raio-X, PDFs e geração do TXT; o botão de gerar aparece por haver elegíveis, sem exigir previamente o selo. (`app/folha/urls.py:6-29`, `app/templates/folha/exportar.html:25-54`, `app/folha/views.py:356`, `app/folha/views.py:406-428`) Que esse admin específico não tenha shell: [nao verificado].

4. **Escopo e validade:** a porta exige carimbo da **mesma empresa, mesma competência, data de hoje e valor zero**. (`app/folha/porta_export.py:807-821`, `app/folha/porta_export.py:865-867`) Porém, a chave persistida não inclui competência: medir outra competência da mesma empresa no mesmo dia **sobrescreve** o carimbo anterior. (`app/folha/porta_export.py:772-776`, `app/inteligencia/models.py:102-105`) “Hoje” é **data civil local**, via `timezone.localdate()`, com fuso padrão `America/Sao_Paulo`: vale até a virada da data, não por 24 horas desde a medição. (`app/folha/porta_export.py:21-23`, `app/folha/porta_export.py:807-808`, `app/config/settings/base.py:93-95`)

BECO: sim -- a recusa exige comando de shell e o fluxo da tela não oferece ação para gerar o carimbo. (`app/folha/porta_export.py:809-813`, `app/folha/urls.py:6-29`, `app/folha/views.py:406-428`)

## 17/11 09:17 · sessao cb7662e893ce · - · - · origem=Ronald (tmux)

Item 1 tem caminho viável, mas o `--rodar` não garante sozinho a espera do filho nem a retirada do PDF; essas condições dependem do script B1, cujo conteúdo não foi fornecido. (`bin/sombra.sh:490-493`; `app/relatorios/lote_espelho.py:132-146`; [nao verificado].)

1. **Premissas.** A view usa `pedir` e `disparar`; o filho executa o command que chama o mesmo gerador de lote. (`app/relatorios/views.py:1227-1246`; `app/relatorios/lote_espelho.py:127-133`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:44-64`; `app/relatorios/lote_espelho.py:205-207`.)

   Por inferência do código, o filho herda a configuração da sombra: `Popen` não passa `env`, o container fixa `DJANGO_SETTINGS_MODULE=config.settings.sombra`, e `manage.py` usa `setdefault`, preservando esse valor; o settings fixa banco `sombra`, host `sombra_db`, e o comando recebe o schema do pai. Execução efetiva: [nao verificado]. (`app/relatorios/lote_espelho.py:126-133`; `bin/sombra.sh:184`; `app/manage.py:9`; `app/config/settings/sombra.py:29`; `app/config/settings/sombra.py:47-51`.)

2. **Compatibilidade.** Sem POST HTTP, a restrição específica de smoke de porta não se aplica ao caminho proposto; o aval fornecido aceita função/command e as duas escritas da lavra. Permanecem o escopo literal, banco da sombra e CPUSET de teste. (`CLAUDE.md:37-38`; `CLAUDE.md:87`; `CLAUDE.md:205-206`; `bin/sombra.sh:183-184`; `respostas_1711_0915.txt:2`, anexo, contando linhas desde “aval Ronald”.)

3. **Condições literais.**
   
   - **Espera:** `disparar` cria processo com `start_new_session=True` e devolve somente PID; não chama `wait`. `--rodar` espera o comando principal do container, sem supervisão adicional desse filho. O script precisa permanecer executando até o job terminar. (`app/relatorios/lote_espelho.py:132-146`; `bin/sombra.sh:490-493`.)
   - **Trava inteira:** `--rodar` já adquire `flock`; a shell do host mantém o descritor 8 enquanto aguarda o container. Assim, basta que o script não termine antes de concluir geração, conferência e retirada do artefato; não é necessário um segundo `--com-a-sombra`. (`bin/sombra.sh:487-492`; `bin/sombra_trava.sh:26-39`.)
   - **Alternativa síncrona:** dentro do script B1, criar/reutilizar o lote pela porta e chamar o command de forma síncrona evita o `Popen`; o command assume e executa o lote antes de retornar. O command precisa de lote já existente e schema correto. (`app/relatorios/lote_espelho.py:83-114`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:27-38`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:44-64`.)
   - **PDF:** o lote grava em `cartoes_lote/`, dentro de media; `/app/media` é tmpfs, e `/sombra` corresponde a `logs/sombra`. O script precisa copiar o PDF para `/sombra` antes do container encerrar e depois encaminhá-lo à pasta do ensaio; essa cópia não é automática. (`app/relatorios/models.py:125`; `app/relatorios/lote_espelho.py:250-253`; `bin/sombra.sh:81`; `bin/sombra.sh:183`; `bin/sombra.sh:189-190`.)
   - **Resultado:** `CONCLUIDO` e quantidade gerada não provam ausência de fallback: o informacional também incrementa `n_ok`, e o lote termina concluído. (`app/relatorios/services.py:235-255`; `app/relatorios/lote_espelho.py:251-253`.)

4. **Efeitos adicionais.** As duas operações de `MetricaSnapshot` podem atualizar registros existentes, e ocorrem antes do script B1, em outro container. Também são escritos `lavra_prod.json`, `repor_lavra.py` e `logs/<rotulo>.out`; esses artefatos devem constar da descrição do ensaio. (`bin/sombra.sh:144`; `bin/sombra.sh:151`; `bin/sombra.sh:157-167`; `bin/sombra.sh:489-492`.)

   Não há, nesses trechos, demonstração de alteração de competência exportada ou pagamento; a lavra deixou de decidir dinheiro segundo o comentário vigente. Efeitos reais em produção: [nao verificado]. (`app/ponto/services/esmeril_espelho.py:351-352`; `CLAUDE.md:265-268`.)

   **Cuidado concreto:** envolver `--rodar` em `--com-a-sombra` executaria a lavra nas duas portas, excedendo as duas operações declaradas. (`bin/sombra.sh:460`; `bin/sombra.sh:489`; `bin/sombra.sh:157-163`.)

## 17/11 09:17 · sessao 59845f30321f · - · - · origem=Ronald (tmux)

- O comando **não tem horário em `CRONS` nem elo em `ENCADEADOS`**: está em `FORA_DE_PIPELINE`, mapa iniciado em `app/config/crons.py:769`, com justificativa explícita de execução sob ato/manual em `app/config/crons.py:887` e `app/config/crons.py:891`.
- A declaração “roda no ATO do export” em `app/config/crons.py:894` é comentário/descritivo: o próprio comando explica que ele mede e lavra, enquanto o TXT apenas confere, em `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:36`.
- **Competência é fornecida explicitamente**: `--mes` e `--ano` são obrigatórios, em `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:28`. O comando usa esses valores em `:35`, percorre todas as empresas ativas em `:86`, mede em `:88` e lavra em `:92`. Logo `--mes 11 --ano 2026` mede essa competência, mas não há cobertura automática de 11/2026 por esse selo; evidências: `app/config/crons.py:891` e `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:28`.
- O universo padrão do cron é empresas `(2, 3, 4)`, tenant `juliani`, em `app/config/crons.py:17`; isso **não restringe** o selo manual, que percorre `Empresa.objects.filter(ativa=True)` em `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86`.
- O render percorre `CRONS` em `app/config/crons.py:1889`, expande empresas em `:1894`, acrescenta schema em `:1899` e conecta correntes em `:1913`. `bin/crons.sh` somente renderiza/check/instala esse resultado em `bin/crons.sh:24` e `bin/crons.sh:27`.
- Há cron parecido, **`vigiar_txt_dominio`**, declarado como elo em `app/config/crons.py:1615`, mas ele compara o acervo de exportações e não grava o selo, conforme `app/folha/management/commands/vigiar_txt_dominio.py:8` e chamada de `censo` em `:59`. O `lavrar_retrato_folha`, outro elo em `app/config/crons.py:1655`, é retrato para a tela, descrito em `app/config/crons.py:1619`; não substitui esse selo.
- `cartao_x_txt_divergentes` é chamado separadamente pelo placar em `bin/placar_code.sh:330`, sem chamada a `lavrar`; os contadores PDF/cartão são explicitamente tratados como manuais por competência em `app/config/crons.py:1139`, e o cartão está em `FORA_DE_PIPELINE` em `app/config/crons.py:1282`.
- Cron instalado e execução real em produção hoje: **[não verificado]**.

## 17/11 09:16 · sessao b9ab0ef52323 · - · - · origem=Ronald (tmux)

(3) O clique usa formulário POST comum para `folha:gerar_txt`, com empresa/mês/ano ocultos e botão “Gerar arquivo dos N elegiveis”; a prévia decide mostrar esse botão apenas por `n_entra > 0`, sem consultar carimbo (`app/folha/views.py:356`, `app/folha/views.py:416-428`).

`gerar_dominio_txt` chama `conferir(empresa, mes, ano)`; se houver recusa, devolve a mensagem integral por `resposta_acao(ok=False, ..., redirect_url=folha:exportar)` (`app/folha/views.py:128-133`). Esse helper classifica `ok=False` como erro, grava Django message e retorna redirect 302 para a tela (`app/core/respostas.py:50-51`, `app/core/respostas.py:68-71`). O template exibe a mensagem em um `alert`; a base também a passa ao toast, cuja variante erro é vermelha e desaparece após dez segundos (`app/templates/folha/exportar.html:10`, `app/templates/base.html:334`, `app/templates/core/_dialogo.html:26-43`, `app/templates/core/_dialogo.html:46-54`).

Sem carimbo, o texto é: “A PORTA DO EXPORT NAO FOI MEDIDA HOJE para %s. O TXT nao sai sem a prova de que a tela, o PDF, o fechamento e o TXT estao no mesmo numero. Rode: %s”; o `%s` final contém `docker exec saas_core python manage.py tenant_command selo_leitores_no_mesmo_numero --schema=juliani --mes %02d --ano %s` (`app/folha/porta_export.py:809-813`).

A mensagem oferece uma instrução de shell, sem ação correspondente na tela: o inventário de rotas da folha não contém medir/lavrar o carimbo; o template oferece seleção de empresa/competência e Raio-X; a prévia oferece PDFs e gerar TXT (`app/folha/urls.py:6-29`, `app/templates/folha/exportar.html:25-54`, `app/folha/views.py:406-430`). O próprio POST declara que só confere, pois medir custa aproximadamente cem segundos para 205 colaboradores e quem mede é o comando (`app/folha/views.py:124-126`). Que o administrador específico não tenha acesso ao shell: [nao verificado].

Conclusão para consolidar: **BECO: sim**, no sentido definido pelo usuário, pois a recusa manda executar comando operacional e não oferece pela tela uma ação para produzir o carimbo (`app/folha/porta_export.py:809-813`, `app/folha/urls.py:6-29`, `app/folha/views.py:406-430`).

## 17/11 09:16 · sessao 786cf13e0f0a · - · - · origem=Ronald (tmux)

- **Gravador real:** `folha.porta_export.lavrar(empresa, mes, ano, r)` faz `MetricaSnapshot.objects.update_or_create`; chave literal **`porta_export_leitores`**, `escopo='empresa'`, `escopo_id=empresa.pk`, `data_ref=hoje`; guarda falhas em `valor` (`-1` quando não mediu), competência em `detalhe['mes'/'ano']` e `data_ini=hoje` (`app/folha/porta_export.py:18`, `app/folha/porta_export.py:768-776`).
- **Comando escritor:** `selo_leitores_no_mesmo_numero` exige `--mes` e `--ano`, importa `medir, lavrar`, percorre `Empresa.objects.filter(ativa=True).order_by('id')`, mede e lavra para cada empresa (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27-35`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-92`). Pode interromper a sequência se medição levantar exceção ou `cartao_x_txt is None` (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:89-95`).
- **Leitura/recusa:** `conferir` consulta chave+empresa+`data_ref=_hoje()`; ausência produz “A PORTA DO EXPORT NAO FOI MEDIDA HOJE...” com shell `docker exec saas_core python manage.py tenant_command selo_leitores_no_mesmo_numero --schema=juliani --mes %02d --ano %s`; em seguida compara a competência armazenada com a pedida, rejeita `valor is None`, negativo ou positivo; só zero retorna vazio/libera (`app/folha/porta_export.py:800-821`, `app/folha/porta_export.py:865-867`).
- **Nuance importante:** a validação é por empresa **e competência**, mas a identidade persistida é somente **chave+escopo+empresa+data**, sem mês/ano. Logo medir outra competência da mesma empresa no mesmo dia **sobrescreve** o carimbo anterior; não coexistem carimbos independentes de 10 e 11 no mesmo dia (`app/folha/porta_export.py:772-776`; constraint confirma `app/inteligencia/models.py:102-105`).
- **Validade:** “hoje” é `timezone.localdate()`, isto é, data civil local, e não janela móvel de 24 h; o carimbo deixa de atender assim que a data local muda. Configuração padrão: `TIME_ZONE='America/Sao_Paulo'`, `USE_TZ=True` (`app/folha/porta_export.py:21-23`, `app/folha/porta_export.py:807-808`, `app/config/settings/base.py:92-95`).
- **Comentário enganoso no modelo:** docstring de `MetricaSnapshot` diz “Escrito so por inteligencia.metricas”, porém a escrita efetiva aqui é `folha.porta_export.lavrar` (`app/inteligencia/models.py:81-82`; `app/folha/porta_export.py:770-776`).

_6432 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
