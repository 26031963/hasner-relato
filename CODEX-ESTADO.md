# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 08:57** · publicado em **30/10/2026 08:57** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 08:57 · sessao aed133b16f47 · O261-NOTURNO · verificador · origem=Code

- **P2 — Ainda pode mostrar dia diferente do texto.** Reproduzi em memória: identidade 21/10, instante 22/10 e pergunta `Confirme o horário da saída no dia 22/10/2026.` → `dia_mostrado` retorna **21/10**. O ponto final quebra a igualdade com o produtor e aciona o fallback; texto ISO com hora em 22/10 também retorna 21/10. A desigualdade não prova que o texto nomeia a identidade. Esse resultado chega ao cabeçalho e à API. (`app/chamados/models.py:2178`; `app/templates/chamados/partials/_card_atendimento.html:90`; `app/api/views.py:1169`). Ocorrência em produção: [não verificado].

- **P2 — O detector ganhou falso negativo em `.format(**dict)`.** Reproduzi `_copias("t.format(**{**ctx})") == []` e `_copias("t.format(**{key: value})") == []`, inclusive quando `ctx` contém `data` ou `key == 'data'`. O filtro considera seguro qualquer `ast.Dict` sem chave constante `'data'`, embora desempacotamentos e chaves dinâmicas possam fornecê-la. (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:168`).

Executei o detector original em memória sobre a cópia: **zero ocorrências em 787 arquivos**, com os exemplos positivos/negativos passando. O perímetro é `app/`, excluindo testes, migrações e caches; os dois donos são dispensados, e a isenção nominal do espelho fica em `chamados/models.py`. (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:142`, `:156`, `:186`, `:195`).

O `select_related('celula')` cobre a relação lida pela propriedade; não encontrei defeito adicional nessa alteração nem no uso de `cached_property`. (`app/api/views.py:1045`; `app/chamados/models.py:2163`, `:2173`).

Correspondência Git ao hash e conteúdo da resposta da volta 3: [não verificado].

## 30/10 08:53 · sessao e05761e8c23b · O261-NOTURNO · verificador · origem=Code

1. **`dia_mostrado` ainda pode contradizer o texto gravado.** Reprodução em memória: `orfao_14h`, célula de 21/10 com saída em 22/10, contexto `data='2026-10-21T19:00:00'`. O produtor grava “saída no dia 2026-10-21T19:00:00”, pois devolve a data bruta quando o parse falha; a propriedade reconstrói “21/10/2026”, a igualdade falha e retorna **22/10**. Esse texto pode ser persistido com `data_evento=21/10`, porque a emissão extrai os primeiros dez caracteres para a identidade. Ocorrência em produção: [nao verificado]. Referências: [query.py:99](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/catalogo/query.py:99), [models.py:2176](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/models.py:2176), [disputa_emissao.py:347](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/services/disputa_emissao.py:347), [disputa_emissao.py:389](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/services/disputa_emissao.py:389).

   **`padrao` e motivo desconhecido não demonstraram esse mesmo erro:** motivo catalogado ignora `padrao`; os quatro motivos com marco estão no catálogo; motivo sem marco retorna a própria `data_evento` antes da comparação. Referências: [query.py:87](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/catalogo/query.py:87), [query.py:64](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/catalogo/query.py:64), [juizes.py:823](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/juizes.py:823), [perguntas.py:34](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/catalogo/perguntas.py:34).

2. **Há N+1 de célula na API.** O queryset carrega `chamado_destino__celula`, mas a propriedade acessa outra relação, `p.celula`: até um SELECT adicional por pergunta datada com essa FK. Referências: [views.py:1043](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/api/views.py:1043), [views.py:1169](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/api/views.py:1169), [models.py:2172](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/models.py:2172).

   Em cobrança/aviso, esse carregamento por pergunta **já acontece no juiz**, antes da propriedade: o helper seleciona apenas `disputa`, e `pergunta_viva` acessa `celula` na chave do memo. A segunda chamada da propriedade é condicional e omite `colaborador`, portanto não executa a query do produtor. Referências: [aviso_pendencia.py:9](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/services/aviso_pendencia.py:9), [juizes.py:757](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/juizes.py:757), [juizes.py:791](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/juizes.py:791), [models.py:2176](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/models.py:2176), [query.py:102](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/catalogo/query.py:102).

3. **O card repete o cálculo:** `dia_mostrado` é propriedade sem cache, acessada no cabeçalho e novamente para cada opção. Nesse caminho a célula já vem carregada; repete processamento, sem acrescentar SELECT da FK. Referências: [models.py:2162](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/models.py:2162), [_card_atendimento.html:90](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/templates/chamados/partials/_card_atendimento.html:90), [_card_atendimento.html:164](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/templates/chamados/partials/_card_atendimento.html:164), [fio.py:222](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/services/fio.py:222).

4. **A API ainda entrega o dia cru junto do rótulo corrigido.** `data_evento` continua D e `data_evento_label` pode ser D+1; exibição do campo cru pelo cliente nativo: [nao verificado]. O atendimento com `base_app` inclui o mesmo card corrigido; cobrança usa `dia_mostrado`; o PDF lê dias do espelho e marcações, o que não comprova outro leitor da data textual da pergunta. Referências: [views.py:1168](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/api/views.py:1168), [meu_atendimento.html:127](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/templates/chamados/meu_atendimento.html:127), [context_processors.py:210](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/core/context_processors.py:210), [cobranca.py:83](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/services/cobranca.py:83), [pdf_espelho.py:598](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/relatorios/pdf_espelho.py:598), [pdf_espelho.py:872](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/relatorios/pdf_espelho.py:872).

5. **O selo AST mantém falsos negativos.** Reproduções retornam `[]`: `t='dia {data}'; k='{data}'; y=t.replace(k,x)` e `f(pergunta='dia {data}')`. O detector só considera certos pais imediatos do literal e não acompanha variáveis. Além disso, exclui `models.py` inteiro e só percorre `chamados`, deixando substituições fora desse perímetro sem fiscalização. Referências: [test_noturno_le_o_dia_do_instante.py:155](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/tests/test_noturno_le_o_dia_do_instante.py:155), [test_noturno_le_o_dia_do_instante.py:137](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/tests/test_noturno_le_o_dia_do_instante.py:137), [test_noturno_le_o_dia_do_instante.py:181](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/tests/test_noturno_le_o_dia_do_instante.py:181).

6. **O selo AST mantém falsos positivos.** `t.format(**{'nome': x})` e `'Olá {nome}'.format_map(c)` são marcados sem substituir `data`; `raise E('placeholder {data} residual' + detalhe)` também é marcado, porque qualquer `BinOp` com esse literal conta como template. Referências: [test_noturno_le_o_dia_do_instante.py:150](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/tests/test_noturno_le_o_dia_do_instante.py:150), [test_noturno_le_o_dia_do_instante.py:158](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/tests/test_noturno_le_o_dia_do_instante.py:158).

## 30/10 08:51 · sessao f438f9b87014 · - · - · origem=Ronald (tmux)

- **API ainda expõe duas datas diferentes**: `data_evento` permanece o dia de identidade; `data_evento_label` usa `dia_mostrado` (`app/api/views.py:1168-1169`). O endpoint atende o colaborador autenticado (`app/api/views.py:973-979`). Se o cliente nativo usa o campo cru para exibição: **[nao verificado]**.

- **Não confirmei outro leitor público direto por `data_evento`**: o card mostra `dia_mostrado` no cabeçalho e na confirmação (`app/templates/chamados/partials/_card_atendimento.html:90`, `app/templates/chamados/partials/_card_atendimento.html:164`). O mesmo card é incluído no atendimento (`app/templates/chamados/meu_atendimento.html:126-127`), cuja casca pode ser `base_app.html` (`app/templates/chamados/meu_atendimento.html:1`, `app/core/context_processors.py:210`).

- **Push de cobrança acompanha a propriedade**: os dias vêm de `p.dia_mostrado` (`app/chamados/services/cobranca.py:83`), entram no corpo (`app/chamados/services/cobranca.py:122-136`) e são enviados pelos canais FCM/webpush (`app/chamados/services/cobranca.py:105-113`, `app/chamados/services/cobranca.py:186-187`). O push de abertura da disputa só informa o motivo, sem dia (`app/chamados/services/disputa_emissao.py:1491-1498`).

- **PDF do espelho não oferece prova de outro leitor da data da pergunta**: recebe os dias do espelho, recorta por `d['data']` e ordena por essa chave (`app/relatorios/pdf_espelho.py:592-599`); desenha as marcações pelas batidas (`app/relatorios/pdf_espelho.py:872-878`). Equiparar esse dia de turno ao dia textual da pergunta seria **[nao verificado]**.

- **Ocorrências que não devem virar falso achado de audiência**: o modal mostra `data_evento` via `data_do_fato` (`app/templates/chamados/partials/modal_fio.html:159-180`, `app/chamados/services/fio.py:87-88`), mas exige gestor (`app/chamados/views.py:872-874`); a outra serialização crua é a fila de validação do copiloto (`app/api/views_mensageria.py:523-537`, `app/api/views_mensageria.py:568`).

- **Resíduo de template, sem comprovar leitor válido de pergunta**: `meu_atendimento.html:19` usa `p.data_evento`, mas está dentro do laço `reg` (`app/templates/chamados/meu_atendimento.html:10-19`), e a view não entrega `p` no contexto (`app/chamados/views.py:633-652`).

## 30/10 08:51 · sessao 4195d57157cb · - · - · origem=Ronald (tmux)

- **P2 — N+1 na API permanece.** O queryset carrega `chamado_destino__celula`, mas não `PerguntaDisputa.celula`; ao serializar cada pergunta com `data_evento`, `dia_mostrado` acessa essa outra FK. São até N SELECTs adicionais para N perguntas datadas com `celula_id`, inclusive quando várias apontam à mesma célula. Referências: `app/api/views.py:1043-1045`, `app/api/views.py:1168-1169`, `app/chamados/models.py:1847-1848`, `app/chamados/models.py:2169-2172`.

- **Cobrança/aviso também têm carregamento por pergunta, mas ele já ocorre antes da property.** O helper traz só `disputa` e chama `pergunta_viva`; a chave do memo desse juiz lê `p.celula`, carregando a FK. Logo atribuir uma nova query por pergunta ao acesso posterior de `dia_mostrado` nesses dois laços seria incorreto. Referências: `app/chamados/services/aviso_pendencia.py:9-13`, `app/chamados/juizes.py:757`, `app/chamados/juizes.py:791`, `app/chamados/services/aviso_pendencia.py:20`, `app/chamados/services/cobranca.py:82-83`.

- **As duas chamadas são condicionais e não são duas queries.** A property sempre executa `dia_mostrado_do_marco`; só chama `texto_pergunta_com_data` quando o dia calculado difere de `data_evento`. Essa chamada omite `colaborador`, portanto não entra no SELECT de `CelulaDia` do produtor. Referências: `app/chamados/models.py:2172`, `app/chamados/models.py:2176-2177`, `app/chamados/catalogo/query.py:102-105`.

- **O cálculo é repetido por acesso no template.** `dia_mostrado` é `@property` sem cache; o card acessa no cabeçalho e uma vez por opção de confirmação. O fio já faz `select_related('celula')`, então nesse caminho há repetição do cálculo, sem novo SELECT da FK. Referências: `app/chamados/models.py:2162-2179`, `app/templates/chamados/partials/_card_atendimento.html:90`, `app/templates/chamados/partials/_card_atendimento.html:162-164`, `app/chamados/services/fio.py:222-224`.

- **A cobrança efetiva refaz a leitura inteira das pendentes.** `cobrar_colaborador` chama `checar_cobranca`, que busca pendentes, e depois chama `_pendentes` novamente; ambas passam pelo helper sem `select_related('celula')`. Referências: `app/chamados/services/cobranca.py:45-48`, `app/chamados/services/cobranca.py:82`, `app/chamados/services/cobranca.py:155`, `app/chamados/services/cobranca.py:163`, `app/chamados/services/aviso_pendencia.py:9-13`.

## 30/10 08:51 · sessao ebdf4394c212 · - · - · origem=Ronald (tmux)

- **Falso negativo:** o detector ignora literal em atribuição, dicionário, retorno e keyword diferente de `padrao`. Reprodução em memória: `t='Confirme dia {data}'; k='{data}'; y=t.replace(k,x)` retorna `[]`; `PerguntaDisputa.objects.create(pergunta='Confirme dia {data}')` também retorna `[]`. A decisão considera apenas o pai imediato do literal e não acompanha variáveis. Referência: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:155-162`.

- **Falso positivo restante:** `t.format(**{'nome': x})` e `'Olá {nome}'.format_map(c)` retornam `[1]`, embora nenhum substitua `data`; a regra marca qualquer `**kwargs` em `.format` e todo `.format_map`. Referência: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:150-154`. A prosa `raise E('placeholder {data} residual' + detalhe)` também retorna `[1]`, porque todo `BinOp` é classificado como template. Referência: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:158`.

- **Cobertura insuficiente para provar produtor único:** o selo exclui `models.py` inteiro e só percorre arquivos Python sob `chamados`; uma substituição nova em `models.py` escapa mesmo que `_copias` a detectasse. Referências: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:137-138`, `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:175-182`.

## 30/10 08:49 · sessao 5c95096a391b · O261-NOTURNO · verificador · origem=Code

- **O card ainda mistura dias na confirmação de escape.** O cabeçalho usa `p.dia_mostrado`, mas “Falta/Atestado/Folga — dia … — confirmar?” usa `p.data_evento`. Quando o marco cai em D+1, a confirmação anuncia D (`app/templates/chamados/partials/_card_atendimento.html:90`, `app/templates/chamados/partials/_card_atendimento.html:164`). O componente integra o atendimento do colaborador (`app/templates/chamados/meu_atendimento.html:127`).

- **A cobrança push continua anunciando D.** Coleta `p.data_evento`, monta “Sobre o seu ponto nos dias…” e encaminha ao colaborador por FCM/webpush; portanto pode divergir do dia mostrado no card (`app/chamados/services/cobranca.py:83`, `app/chamados/services/cobranca.py:123`, `app/chamados/services/cobranca.py:133`, `app/chamados/services/cobranca.py:186`, `app/chamados/services/cobranca.py:107`, `app/chamados/services/cobranca.py:112`).

- **Outras respostas da API ainda nomeiam o questionário por D.** `aviso_texto_livre` monta “questionario do(s) dia(s)” usando `p.data_evento`; esse texto retorna no campo `aviso` de dois endpoints (`app/chamados/services/aviso_pendencia.py:20`, `app/chamados/services/aviso_pendencia.py:21`, `app/api/views.py:1001`, `app/api/views.py:2279`). Sua exibição efetiva pelo cliente é **[nao verificado]**.

- **`dia_mostrado` pode discordar do texto já gravado.** A propriedade calcula o dia pelo DNA atual; ao reutilizar uma pergunta existente, `abrir` devolve o registro sem atualizar `pergunta`. Assim, uma pergunta antiga cujo texto nomeia D pode ganhar cabeçalho D+1 (`app/chamados/models.py:2172`, `app/chamados/models.py:2062`, `app/chamados/models.py:2084`). O próprio classificador contempla perguntas com texto antigo nomeando D (`app/chamados/services/validacao.py:819`).

- **O detector AST tem falso positivo e falso negativo nos papéis escolhidos.** Qualquer keyword contendo `{data}` é acusado: `raise E(message='placeholder {data} residual')` cai no detector, embora a variante posicional seja aceita como prosa (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:154`, `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:166`). Já `t.format(**{'data': x})` escapa: a expansão tem `arg=None`, enquanto a regra exige `arg == 'data'` (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:147`). Além disso, a varredura cobre somente `chamados/`, sem garantir exclusividade global (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:133`, `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:171`).

A comparação de texto gravado especificamente com **f0d319ba** permanece **[nao verificado]**. A constante atual contém `Confirme o horario no dia {data}` (`app/chamados/catalogo/query.py:45`); o anexo mostra esse mesmo literal antes da substituição, mas não fornece a versão de f0d319ba **[nao verificado]**.

## 30/10 08:48 · sessao 9f402510778f · - · - · origem=Ronald (tmux)

Achados do item (1):

- **Confirmação do escape ainda mostra D:** `app/templates/chamados/partials/_card_atendimento.html:164` interpola `p.data_evento` no modal, enquanto o cabeçalho usa `p.dia_mostrado` em `:90`. Para marco noturno em D+1, o mesmo card mostra duas datas. O card integra o atendimento do colaborador em `app/templates/chamados/meu_atendimento.html:127`; a casca app disponibiliza o diálogo em `app/templates/base_app.html:145`, e ele exibe a mensagem recebida em `app/templates/core/_dialogo.html:233`.

- **Push de cobrança ainda anuncia D:** `app/chamados/services/cobranca.py:83` coleta os dias de `p.data_evento`; `:123` e `:133` formam “Sobre o seu ponto nos dias…”. `:186`–`:187` enviam esse texto ao colaborador, por FCM em `:107` ou webpush em `:112`. Assim, a cobrança continua apontando D quando o card passa a mostrar D+1 (`app/templates/chamados/partials/_card_atendimento.html:90`).

- **Aviso devolvido ao colaborador ainda identifica o questionário por D:** `app/chamados/services/aviso_pendencia.py:20` coleta `p.data_evento` e `:21`–`:24` monta “questionario do(s) dia(s)…”. Esse aviso entra na resposta das APIs em `app/api/views.py:996`–`:1001` e `:2271`–`:2279`, além da resposta web em `app/chamados/views.py:1119`–`:1124`. A exibição efetiva do campo `aviso` pelo cliente nativo é [não verificado].

Não há outro achado concreto para acrescentar: `api_disputa_perguntas` serve o texto salvo sem campo de data em `app/api/views.py:2622`–`:2635`; o PDF de chamados renderiza descrição e followups em `app/relatorios/pdf_chamados.py:129`–`:147`, sem leitura direta de `PerguntaDisputa.data_evento` nesse trecho.

## 30/10 08:48 · sessao d73817e3db6c · - · - · origem=Ronald (tmux)

Achados no detector:

- **Falso positivo de prosa:** `raise E(message='placeholder {data} residual')` produz `[1]`, embora a variante posicional seja explicitamente considerada prosa e aceita. O teste classifica qualquer `ast.keyword` com `{data}`, independentemente do argumento ou função: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:154`; caso negativo posicional: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:166`.

- **Falso negativo no papel `.format`:** `t.format(**{'data': x})` produz `[]`. A verificação exige `k.arg == 'data'`; na expansão `**`, o AST tem `arg=None`, e o dicionário contém `'data'`, sem o literal `'{data}'` exigido pela outra verificação: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:147`, `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:151`.

- **Falso negativo no papel `%`:** `x = f'no dia {{data}}' % ()` produz `[]`. O literal aparece sob `ast.JoinedStr`, pai não aceito pelo detector: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:154`, `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:156`.

- **Falso positivo em concatenação de prosa:** `'placeholder {data} residual' + detalhe` produz `[1]`; a seleção aceita todo `ast.BinOp`, sem restringir ao operador `%` anunciado: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:139`, `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:154`.

As reproduções acima executaram somente `_copias`, extraído por AST em memória, sem importar Django ou escrever arquivos. [não verificado por arquivo:linha]

Não encontrei produtor de `{data}` fora de `chamados/`; o teste, porém, só percorre `app/chamados`, portanto não garante ausência global: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:133`, `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:171`.

## 30/10 08:47 · sessao 7a5bab0cc3cc · - · - · origem=Ronald (tmux)

Nenhum achado concreto na guarda.

A chamada em `app/chamados/services/validacao.py:834` corresponde à assinatura `registrar_engolido(logger_nome, operacao, exc, **contexto)` em `app/core/observ.py:73`.

Quando `_ts_tipo_do_plantio` levanta, a classificação retorna `LOTE_B` com `sem_instante_do_plantio=True` (`app/chamados/services/validacao.py:830-835`). A fila preserva o item nessa classe e continua a iteração (`app/chamados/services/validacao.py:885-888`); suas contagens incluem B, enquanto `refs_lote` contém apenas A (`app/chamados/services/validacao.py:895-898`). A validação em massa reclassifica cada pergunta e recusa qualquer classe diferente de A (`app/chamados/services/validacao.py:681-683`).

## 30/10 08:43 · sessao 03024d698e5a · O261-V-FASE · verificador · origem=Code

Não encontrei falha procedente adicional no diff apresentado, pela inspeção do código atual. A C10 consome o leitor existente e só formata sua resposta. (`app/escala/services/cadastro_realidade.py:336`)

As duas triagens procedem:

- **Rótulo 24x48: tratado.** A C10 nomeia os dois ciclos, e o teste percorre `CICLOS` da autoridade. (`app/escala/services/cadastro_realidade.py:113`; `app/escala/tests/test_o261v_fase_na_lista.py:116`)
- **Dívida da ata: permanece, fora deste complemento.** O pareamento continua no leitor; o inventário aponta `turnos_do_colab`. A alteração apresentada apenas acrescenta campos à resposta, preservando quem é acusado. (`app/escala/services/fase_12x36.py:175`; `app/core/juizes.py:905`; `app/escala/services/fase_12x36.py:187`)

Conferi também:

- **Regras e contagens:** as datas vêm das mesmas listas já contadas; o piso e `n=len(achados)` permanecem no leitor. (`app/escala/services/fase_12x36.py:187`; `app/escala/services/fase_12x36.py:202`; `app/escala/services/fase_12x36.py:207`; `app/escala/services/fase_12x36.py:216`)
- **Mesma guarda dos vizinhos:** C10 repete o `try/except` de C9. O filtro de empresas antecede a inclusão nas linhas e contadores. (`app/escala/services/cadastro_realidade.py:595`; `app/escala/services/cadastro_realidade.py:602`; `app/escala/services/cadastro_realidade.py:629`)
- **Juiz, escritor e relógio:** o complemento chama `fora_de_fase(hoje)`, sem cálculo próprio de fase, gravação ou nova leitura de relógio; a ação aponta à tela existente de vínculo. (`app/escala/services/cadastro_realidade.py:331`; `app/escala/services/cadastro_realidade.py:491`)
- **L-040 e mordida:** tela e PDF exibem o mesmo detalhe; os testes verificam datas no conteúdo e usam respostas com um/zero achados para detectar abandono do leitor. (`app/templates/escala/cadastro_x_realidade.html:28`; `app/escala/services/cadastro_realidade.py:733`; `app/escala/tests/test_o261v_fase_na_lista.py:134`; `app/escala/tests/test_o261v_fase_na_lista.py:180`; `app/escala/tests/test_o261v_fase_na_lista.py:192`)

Execução da suíte, RED histórico e resultado real do col451: **[não verificado]**. Comparação Git independente `f0d319ba..HEAD`: **[não verificado]**, pois a cópia disponibilizada não contém repositório Git.

## 30/10 08:43 · sessao d705f3594a03 · - · - · origem=Ronald (tmux)

Não encontrei falha nova procedente nos testes e no recorte de leis examinado. [nao verificado]

- Triagem do rótulo procede e está tratada: C10 nomeia 12x36 e 24x48; o teste consulta `CICLOS` da autoridade, evitando validar só uma frase fixa. `app/escala/services/cadastro_realidade.py:113`; `app/escala/tests/test_o261v_fase_na_lista.py:116`.
- Há mordida contra abandonar o leitor: os mocks com um achado e nenhum achado determinam exatamente quem aparece na C10. `app/escala/tests/test_o261v_fase_na_lista.py:180`; `app/escala/tests/test_o261v_fase_na_lista.py:192`.
- Os casos conferem datas esperadas pela regra, identificação do vínculo, contagens das datas, filtro por empresa e sobrevivência das demais assinaturas quando o leitor falha. `app/escala/tests/test_o261v_fase_na_lista.py:106`; `app/escala/tests/test_o261v_fase_na_lista.py:127`; `app/escala/tests/test_o261v_fase_na_lista.py:157`; `app/escala/tests/test_o261v_fase_na_lista.py:161`.
- A guarda da C10 repete a dos vizinhos; não proponho reprovação por ausência de sinal explícito de erro, pois isso contrariaria a instrução literal recebida. `app/escala/services/cadastro_realidade.py:595`; `app/escala/services/cadastro_realidade.py:602`.
- Tela/PDF têm verificação de C10 e datas no conteúdo extraído, além do cabeçalho `%PDF`. `app/escala/tests/test_o261v_fase_na_lista.py:134`.

Execução real e evidência histórica de RED/GREEN: [não verificado].

## 30/10 08:43 · sessao 2eb8ca1ab920 · - · - · origem=Ronald (tmux)

Não encontrei falha nova procedente no leitor e nos consumidores examinados; o enxerto expõe o mesmo resultado já julgado. (`app/escala/services/fase_12x36.py:187`, `app/escala/services/fase_12x36.py:207`, `app/escala/services/cadastro_realidade.py:338`)

- `ec_id` lê `ec.pk`; as duas listas de datas serializam diretamente `trabalho_mudo` e `folga_trabalhada`, as mesmas listas das contagens. Piso, seleção dos achados e `n=len(achados)` continuam no leitor. (`app/escala/services/fase_12x36.py:187`, `app/escala/services/fase_12x36.py:192`, `app/escala/services/fase_12x36.py:202`, `app/escala/services/fase_12x36.py:207`, `app/escala/services/fase_12x36.py:216`)
- `_fase_deslocada` apenas chama o leitor com `hoje` e formata seus campos; não introduz leitura de relógio, pareamento, cálculo de fase nem gravação de estado. (`app/escala/services/cadastro_realidade.py:331`)
- Os consumidores anteriores continuam lendo o mesmo leitor: pauta, propositor, fila por causa e API da mensageria. A mensageria recebe os vínculos crus, portanto as datas ISO e o identificador inteiro cabem na serialização existente. (`app/escala/services/fase_12x36.py:223`, `app/escala/services/propositor.py:455`, `app/chamados/services/fila_por_causa.py:173`, `app/api/views_mensageria.py:1669`, `app/api/views_mensageria.py:1675`)
- A triagem do rótulo procede e está tratada: C10 nomeia `12x36` e `24x48`; o teste percorre `CICLOS` para exigir ambos. (`app/escala/services/cadastro_realidade.py:113`, `app/escala/services/fase_12x36.py:29`, `app/escala/tests/test_o261v_fase_na_lista.py:117`)
- A dívida do pareador está efetivamente inventariada com destino `turnos_do_colab`; as chamadas atuais a `parear_turnos` permanecem na função. Ela é dívida preexistente, não regra criada pelo enxerto aditivo apresentado. (`app/core/juizes.py:905`, `app/core/juizes.py:906`, `app/escala/services/fase_12x36.py:146`, `app/escala/services/fase_12x36.py:175`)

Verificação estática; não executei testes. [não verificado] [nao verificado]

## 30/10 08:41 · sessao 1ef69f84e37d · O261-V-FASE · verificador · origem=Code

Encontrei um defeito novo e uma triagem de escopo que não procede:

1. **[P2] C10 apresenta achados de 24x48 como “12x36”.** O leitor admite ambos os ciclos, mas o adaptador transporta todos os achados para uma assinatura com texto fixo “12x36”. Uma 24x48 deslocada pode satisfazer os dois pisos do detector. Isso viola “rótulo diz o que a conta faz”; basta corrigir a apresentação, preservando o leitor existente. (app/escala/services/fase_12x36.py:29,106,187; app/escala/models.py:1264; app/escala/services/cadastro_realidade.py:112,337; CLAUDE.md:35)

2. **A dívida da ata é anterior, mas “sítio não tocado” não procede.** O inventário está em `core/juizes.py`, porém o mecanismo está em `fora_de_fase`, função alterada neste diff. Ela continua chamando o pareador sem alimentar o papel da ata; nesse caminho o papel sai do `tipo`, enquanto o caminho canônico consulta a ata. O escopo escrito inclui o sítio tocado, e a 4a exige leitura da lâmpada. Portanto, essa exclusão precisa ser corrigida na triagem; não estou classificando o mecanismo como juiz criado agora. (app/core/juizes.py:905; app/escala/services/fase_12x36.py:146,175,192,207; app/ponto/turnos.py:1212,1911,1973; CLAUDE.md:276,600)

As demais triagens:

- **Vínculo acusado: procede como correção de apresentação.** `ec_id` chega ao item e ao detalhe, exibido na tela e no PDF. (app/escala/services/fase_12x36.py:192; app/escala/services/cadastro_realidade.py:340,344,732; app/templates/escala/cadastro_x_realidade.html:28)
- **Falha do leitor: permanece sem aviso na lista.** A guarda realmente repete os vizinhos; isso confirma o comportamento declarado, sem comprovar a atribuição “o aval pede a mesma guarda”. (app/escala/services/cadastro_realidade.py:550–614) Essa exigência atribuída ao aval: **[não verificado]**.
- **Datas, PDF e MORDE-zero: procedem por inspeção.** Há datas esperadas literais, extração textual do PDF e presença real exigida antes do mock vazio. (app/escala/tests/test_o261v_fase_na_lista.py:29,106,144,190)

Execução da suíte, RED histórico e comparação Git com `f0d319ba`: **[não verificado]**.

## 30/10 08:40 · sessao e3bb5ee2fac5 · - · - · origem=Ronald (tmux)

A triagem “sítio não tocado” não procede: o registro da dívida permanece em `core/juizes.py`, mas o mecanismo executável está em `fora_de_fase`, justamente a função alterada para expor `ec_id` e datas. O escopo exige examinar todo sítio tocado, não apenas as linhas acrescentadas. `CLAUDE.md:600`; `app/core/juizes.py:905`; `app/escala/services/fase_12x36.py:192`, `:207`.

A dívida é anterior, portanto não deve ser descrita como “segundo juiz criado neste diff”; contudo, segue dentro de função tocada e passa a alimentar a C10: batidas cruas entram no pareador, enquanto o registro aponta `turnos_do_colab` como autoridade. `app/escala/services/fase_12x36.py:128`, `:146`, `:175`; `app/core/juizes.py:667`, `:905`; `app/escala/services/cadastro_realidade.py:335`.

O aval de reutilizar o leitor é compatível com corrigir esse leitor existente pela autoridade da casa; não autoriza preservar a dívida [não verificado: interpretação do aval]. Uma discordância concreta entre pareamento cru e ata ainda precisa ser demonstrada para fundamentar o impacto funcional; o registro comprova a dívida declarada, não uma reprodução executada. `app/core/juizes.py:905`; [não verificado: reprodução dinâmica].

## 30/10 08:40 · sessao a11bf3d62b06 · - · - · origem=Ronald (tmux)

Achei uma falha nova de rótulo: **C10 chama qualquer achado de “12x36”, mas o leitor inclui também 24x48**. O universo admite os dois ciclos (`app/escala/services/fase_12x36.py:29`, `:106`); uma 24x48 deslocada um dia pode atingir os dois pisos, pois sua regra é `% 3` (`app/escala/models.py:1264`) e o detector exige três dias de cada lado (`app/escala/services/fase_12x36.py:187`). `_fase_deslocada` transporta todos os achados (`app/escala/services/cadastro_realidade.py:337`), mas a assinatura os descreve obrigatoriamente como “12x36” (`:112`). A correção pode ser apenas de apresentação, sem outro juiz.

As triagens conferidas:

- **Identidade do vínculo tratado:** leitor expõe `ec_id` (`app/escala/services/fase_12x36.py:192`); adaptador guarda `ec` e inclui esse identificador no detalhe (`app/escala/services/cadastro_realidade.py:340`, `:344`); tela e PDF apresentam o detalhe (`app/templates/escala/cadastro_x_realidade.html:28`; `app/escala/services/cadastro_realidade.py:732`).
- **Falha da leitura continua sem aviso visual:** C10 registra exceção e segue (`app/escala/services/cadastro_realidade.py:601`). Isso realmente repete C2 (`:550`), C4 (`:567`), C6 (`:574`), C7 (`:581`), C8 (`:588`), C9 (`:595`) e C5 (`:609`). O teste confirma ausência de C10 preservando A4 (`app/escala/tests/test_o261v_fase_na_lista.py:158`).
- **Datas, PDF e MORDE-zero ganharam mordida no código do teste:** datas literais comparadas com saída real (`app/escala/tests/test_o261v_fase_na_lista.py:106`, `:121`); PDF extraído com `pypdf` e conteúdo exigido (`:144`); MORDE-zero exige primeiro o positivo real (`:190`). Execução dos testes: **[não verificado]**.
- **Filtro de empresa permanece efetivo:** é aplicado ao construir a linha, depois da coleta C10 (`app/escala/services/cadastro_realidade.py:626`).
- **Uma lista para tela/PDF:** a view chama `lista` e passa os mesmos dados a `pdf_bytes` (`app/escala/views.py:490`); PDF percorre `dados['postos']` e assinaturas (`app/escala/services/cadastro_realidade.py:725`).
- **Porta existente preservada:** C10 retorna apenas o link de cadastro (`app/escala/services/cadastro_realidade.py:490`); nenhuma escrita foi acrescentada nesse ramo (`:492`).
- **Dívida de batida crua permanece anterior:** o leitor chama `parear_turnos` diretamente (`app/escala/services/fase_12x36.py:146`, `:175`), e o registro aponta que a API deveria ler `turnos_do_colab` (`app/core/juizes.py:905`).

## 30/10 08:39 · sessao cdebc79f1e64 · - · - · origem=Ronald (tmux)

Achei um candidato novo de rótulo/universo: C10 diz “12x36”, mas importa todos os achados de `fora_de_fase`, inclusive `24x48`. Isso não exige juiz novo para corrigir, mas pede alinhar o rótulo ao universo do leitor. Fontes: `app/escala/services/cadastro_realidade.py:112`, `app/escala/services/cadastro_realidade.py:337`, `app/escala/services/fase_12x36.py:29`, `app/escala/services/fase_12x36.py:106`; lei: `CLAUDE.md:35-36`.

O caso é possível pelo código: o ciclo `24x48` trabalha quando `delta % 3 == 0`; se os turnos vierem um dia deslocados, haverá pelo menos três dias de trabalho sem turno e três folgas com turno dentro de 14 dias, satisfazendo o leitor. Fontes: `app/escala/models.py:1264-1266`, `app/escala/services/fase_12x36.py:179-187`. Não executei reprodução no banco. [não verificado]

As triagens dos testes procedem por inspeção: [nao verificado]

- Datas agora têm expectativas literais independentes e comparação tanto do leitor quanto da C10; todas as datas precisam estar no detalhe. `app/escala/tests/test_o261v_fase_na_lista.py:29-30`, `:106-112`, `:121-129`.
- PDF agora é extraído com `pypdf` e precisa conter C10 e datas dos dois lados. `app/escala/tests/test_o261v_fase_na_lista.py:144-148`. A dependência já existe. `app/requirements.txt:23`.
- MORDE-zero exige positivo real antes do mock vazio; MORDE-um substitui o achado real por colaborador em fase. `app/escala/tests/test_o261v_fase_na_lista.py:177-193`.
- Isso comprova mordida potencial por leitura; execução verde e RED histórico não foram verificados. [não verificado] [nao verificado]

A dívida da ata permanece: o leitor chama `parear_turnos` diretamente sem `papel_por_minuto`, tanto na janela ampla quanto na detecção. `app/escala/services/fase_12x36.py:146`, `:175`. O pareador documenta que sem alimentação o papel sai do `tipo`; o caminho canônico carrega o papel da ata e o repassa. `app/ponto/turnos.py:1212-1213`, `:1911`, `:1973-1975`. Ela está registrada em `app/core/juizes.py:905-906`; o diff apresentado só expõe ID e datas já contadas nesse leitor. `app/escala/services/fase_12x36.py:192`, `:207-208`.

A cópia não disponibilizou metadados Git; comparação real com `f0d319ba` e ordem temporal de teste/código ficaram não verificadas. [não verificado]

## 30/10 08:39 · sessao e3bb5ee2fac5 · - · - · origem=Ronald (tmux)

Achei uma falha nova de rótulo: C10 declara sempre “12x36”, mas consome integralmente um leitor cujo universo inclui `24x48`; a regra desse ciclo pode alcançar os dois pisos da detecção, portanto um achado `24x48` será apresentado como `12x36` na lista e no PDF. Isso contraria “rótulo diz o que a conta faz”; não exige juiz novo para corrigir a apresentação. Evidências: `app/escala/services/cadastro_realidade.py:112`, `:337`, `:643`, `:732`; `app/escala/services/fase_12x36.py:29`, `:106`, `:179`, `:187`; `app/escala/models.py:1264`; `CLAUDE.md:35`.

As triagens:

- Identificação do vínculo: procede como correção de apresentação; o leitor entrega `ec_id` e a C10 o preserva em `ec` e no detalhe. `app/escala/services/fase_12x36.py:192`; `app/escala/services/cadastro_realidade.py:340`, `:344`.
- Falha do leitor: permanece silenciosa para o consumidor da lista; a igualdade com o comportamento dos vizinhos é factual. O aval literal fornecido não contém exigência explícita de “mesma guarda” [não verificado]. Não encontrei esse complemento nos documentos pesquisados [não verificado]. `app/escala/services/cadastro_realidade.py:550`, `:567`, `:574`, `:581`, `:588`, `:595`, `:602`, `:609`.
- Selos: as datas esperadas são literais; há extração textual do PDF; MORDE-zero exige antes a presença sem mock. Não executei testes nesta inspeção [não verificado]. `app/escala/tests/test_o261v_fase_na_lista.py:29`, `:106`, `:145`, `:189`.
- Pareamento cru: a dívida está documentada e o leitor continua chamando `parear_turnos`; esse mecanismo não foi introduzido pelo diff apresentado. `app/core/juizes.py:905`; `app/escala/services/fase_12x36.py:146`, `:175`.

Não identifiquei novo escritor, novo relógio ou derivação de fase no adaptador: ele somente projeta a resposta do leitor existente. `app/escala/services/cadastro_realidade.py:330`.

## 30/10 08:38 · sessao 302bc57ad3d7 · O261-NOTURNO · verificador · origem=Code

**O achado principal é a contradição entre o novo texto e o dia destacado nas telas.** Há também custo adicional antes da deduplicação e lacunas no selo. (`app/templates/chamados/partials/_card_atendimento.html:90`; `app/chamados/catalogo/query.py:100`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:128`)

1. **A tela pode continuar induzindo a resposta pelo dia D.** O card destaca “Estamos falando do dia” usando `data_evento`, imediatamente antes de imprimir a pergunta. Para a saída noturna, passa a apresentar cabeçalho **21/10** e texto **22/10**. O modal também imprime “Refere-se ao dia” pela identidade; seu aviso de divergência compara identidade com dia do chamado, sem comparar o dia textual com o instante do marco. (`app/templates/chamados/partials/_card_atendimento.html:87`; `app/templates/chamados/partials/_card_atendimento.html:93`; `app/templates/chamados/partials/modal_fio.html:157`; `app/templates/chamados/partials/modal_fio.html:180`; `app/chamados/services/fio.py:85`)

   A API igualmente entrega `data_evento_label` junto do texto gravado, preservando essa combinação D/D+1; como o cliente nativo a apresenta é **[não verificado]**. (`app/api/views.py:1168`; `app/api/views.py:1170`)

2. **Há mudança adicional no tratamento de data com hora.** O helper agora corta qualquer entrada nos primeiros dez caracteres: `2026-10-21T19:00:00` vira `21/10/2026`, inclusive sem colaborador. Isso amplia a normalização além da escolha do dia da lâmpada e altera a preservação do valor bruto descrita no diff anterior. (`app/chamados/catalogo/query.py:95`; `app/chamados/catalogo/query.py:97`)

   **Não encontrei divergência textual entre os dois catálogos:** a comparação estática dos 16 motivos deu textos iguais. Motivos sem `{data}` retornam o texto diretamente; motivo desconhecido sem `padrao` retorna `None`; `padrao=''` permanece vazio; um `padrao` fornecido não substitui motivo conhecido. Portanto, a troca de catálogo nos seis sítios não demonstra outra mudança de redação. (`app/chamados/models.py:2355`; `app/chamados/catalogo/perguntas.py:34`; `app/chamados/catalogo/query.py:83`)

3. **O disparo pode pagar duas consultas novas por alvo, inclusive na tentativa deduplicada.** Cada formatação com colaborador e data válida executa um `SELECT CelulaDia`. A fábrica chama `criar_ou_atualizar_disputa`, que já gera perguntas, e depois chama o gerador novamente. Para motivos datados, a formatação acontece antes do dedup; a segunda tentativa ainda consulta a célula. (`app/chamados/catalogo/query.py:98`; `app/chamados/services/fabrica_da_celula.py:122`; `app/chamados/services/fabrica_da_celula.py:124`; `app/chamados/services/disputa_emissao.py:606`; `app/chamados/services/disputa_emissao.py:618`; `app/chamados/services/disputa_emissao.py:317`; `app/chamados/services/disputa_emissao.py:370`; `app/chamados/services/disputa_emissao.py:386`)

   Isso está dentro do laço de frota, embora a célula já tenha sido carregada na seleção dos alvos. Volume e latência efetivos são **[não verificado]**. Não demonstrei essa mesma multiplicação em toda passagem do reconciliador: os caminhos rastreados reatam perguntas existentes e reemitem chamados. (`app/chamados/management/commands/disparar_perguntas_competencia.py:104`; `app/chamados/management/commands/disparar_perguntas_competencia.py:123`; `app/chamados/management/commands/disparar_perguntas_competencia.py:279`; `app/chamados/reconciliador.py:862`; `app/chamados/reconciliador.py:902`; `app/chamados/reconciliador.py:1347`)

4. **O plantio em leitura não introduz escrita de banco no derivador rastreado, mas acrescenta consultas.** A persistência da pendência está no materializador, depois da derivação; o lote passa apenas uma lista descartável de erros. Exceções engolidas e divergências podem, contudo, incrementar contadores e emitir log. (`app/chamados/services/validacao.py:830`; `app/chamados/services/materializacao.py:379`; `app/chamados/services/materializacao.py:786`; `app/chamados/services/materializacao.py:790`; `app/core/observ.py:73`)

   Para `07:00`/`00:00`, a derivação pode consultar a grade no crivo e novamente na escolha do tipo, além de carregar vínculo e marcos no ajuste overnight. Em diurno, quando os dois instantes são iguais e não há conflito, ainda executa dois `exists()` iguais. (`app/chamados/services/materializacao.py:355`; `app/chamados/services/materializacao.py:266`; `app/chamados/services/materializacao.py:404`; `app/chamados/juizes.py:301`; `app/chamados/services/validacao.py:831`; `app/chamados/services/validacao.py:834`)

5. **Ausência de destino/contexto não quebra por si; existem outros limites de exceção.** Havendo `data_evento`, contexto vazio é aceito, e os leitores de marco toleram chamado `None`. O teste novo monta precisamente essa situação. (`app/chamados/services/materializacao.py:323`; `app/chamados/services/materializacao.py:327`; `app/chamados/juizes.py:461`; `app/chamados/juizes.py:475`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:86`)

   Entretanto, o ajuste overnight fica fora do `try` da hora e alcança o parse desprotegido de `dna.marcos`; uma hora inválida nesse DNA pode propagar exceção até a classificação da fila. Ocorrência desse dado em produção é **[não verificado]**. (`app/chamados/services/materializacao.py:396`; `app/chamados/services/materializacao.py:404`; `app/chamados/juizes.py:303`; `app/escala/models.py:1104`; `app/chamados/services/validacao.py:881`)

6. **O selo morde as duas formas antigas, mas não garante “um produtor”.** Ele procura somente `.format(data=...)` e `.replace('{data}', ...)`, em arquivos Python sob `chamados/`. Executei o detector extraído em memória: f-string, `%`, formato posicional, `str.format(t, data)` e `format_map` escapam; as formas cobertas tiveram zero ocorrências atuais. (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:120`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:123`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:140`)

   Existe produtor administrativo com `%`, ignorado pelo selo, embora não formate data. Outro interpolador atual de `{data}` fora de `chamados/` não foi localizado; inexistência absoluta é **[não verificado]**. (`app/chamados/services/acoes_disputa.py:259`)

   Os testes comportamentais têm controles úteis — conflito em D+1, conflito em D e ausência de conflito —, mas não exercitam os seis emissores, a identidade persistida, os intervalos, `padrao` ou data com hora. O DNA de teste deixa os intervalos vazios. Suíte Django e RED histórico são **[não verificado]**. (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:36`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:60`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:96`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:110`)

7. **Não vejo um segundo juiz novo nesses trechos; há fontes paralelas preexistentes.** O novo leitor usa o mapa `MARCO_DA_PERGUNTA` e lê o instante gravado; o lote chama o derivador existente. Porém, seu envelope continua dependendo de um fallback que escolhe escala ativa e lê o template, enquanto o plantio usa vínculo do dia e DNA congelado. Essa divergência permanece possível. Nos trechos novos, não há leitura de “agora”. (`app/chamados/catalogo/query.py:59`; `app/chamados/catalogo/query.py:67`; `app/chamados/services/validacao.py:812`; `app/chamados/services/validacao.py:830`; `app/chamados/juizes.py:1251`; `app/chamados/juizes.py:1256`; `app/chamados/juizes.py:301`; `app/escala/models.py:1099`)

8. **Os consumidores existentes não fiscalizam o novo contrato textual.** INV4 só pergunta se o helper retorna algo, sem colaborador; a auditoria de cegas aceita qualquer ocorrência `dd/mm/aaaa`. Nenhum desses controles confere o dia do DNA. O teste de correção cega também chama sem colaborador e simula a aplicação com atribuição/save direto; não prova a ação real nem o noturno. O handler funcional dessa ação não foi localizado: **[não verificado]**. (`app/chamados/management/commands/auditar_disputas.py:52`; `app/chamados/management/commands/auditar_disputas.py:74`; `app/chamados/management/commands/auditar_disputas.py:191`; `app/chamados/tests/test_corrigir_pergunta_cega.py:54`; `app/templates/chamados/partials/modal_fio.html:187`)

   Há dependência do texto integral: a cura de limbo procura a pergunta inteira no followup, e a nota de resposta inclui esse texto no hash de deduplicação. Uma correção retrospectiva pode afetar esse casamento. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:31`; `app/chamados/services/disputa_emissao.py:1314`)

As contagens de produção presentes na docstring não foram confirmadas por registros fornecidos; alcance real, comparação histórica dos commits e execução RED/VERDE permanecem **[não verificado]**. (`app/chamados/catalogo/query.py:51`)

## 30/10 08:36 · sessao 2ca5c49cf01b · O261-V-FASE · verificador · origem=Code

Encontrei uma incompatibilidade de vínculo e duas falhas de proteção nos testes. A integração reutiliza o leitor existente, mas isso não garante que a linha mostre o vínculo acusado. (`app/escala/services/cadastro_realidade.py:336`, `:338`; `app/escala/services/fase_12x36.py:114`.)

1. **C10 pode acusar um vínculo sob o cabeçalho de outro. Classe: derivação paralela de fonte única.**  
   O detector colapsa os vínculos por colaborador; com a ordenação `-data_inicio`, o último sobrescreve os anteriores, conservando o mais antigo. A lista escolhe separadamente o vínculo vigente pela juíza. O adaptador C10 não transporta a identidade do vínculo acusado. Resultado possível: template/âncora antigos no detalhe, cabeçalho do vínculo novo e ação sem identificar qual corrigir. Reproduzi as duas seleções com objetos sintéticos em memória: detector escolheu vínculo 1; juíza, vínculo 2. O estado com dois ativos é admitido pelo modelo atual. (`app/escala/services/fase_12x36.py:114`; `app/escala/models.py:1003`, `:1019`; `app/escala/services/cadastro_realidade.py:338`, `:488`, `:521`; `app/escala/alimentacao.py:254`.) Isso compromete o agrupamento **posto → vínculo → assinatura → ação** exigido pela L-040. (`app/docs/LEIS.md:88`.)

2. **Falha do leitor desaparece como ausência de C10. Classe: ausência de sinal lida como sinal bom; selo que protege essa omissão.**  
   A exceção apenas gera log; o retorno não informa que a fase ficou sem leitura. Assim, para tela/PDF, “leitor falhou” e “nenhum achado de fase” ficam indistinguíveis. C3 já oferece o precedente correto: `sem_medida` explícito, apresentado na tela. O teste novo exige justamente C10 vazia após a exceção, sem exigir aviso de leitura incompleta. (`app/escala/services/cadastro_realidade.py:599`, `:684`; `:556`; `app/templates/escala/cadastro_x_realidade.html:17`; `app/escala/tests/test_o261v_fase_na_lista.py:139`.) A própria lista identifica essa classe de erro como “ausência de sinal lida como sinal bom”. (`app/escala/services/cadastro_realidade.py:540`.)

3. **Os selos não protegem integralmente os dias nem a igualdade do PDF. Classe: selo/RED sem mordida.**
   - As datas da lista são comparadas ao próprio leitor; a verificação do leitor exige apenas comprimento, strings e ISO. Executei esse método isoladamente com datas repetidas de `1900-01-01`: passou. Portanto, uma alteração que preserve comprimentos pode publicar dias errados sem essa proteção detectar. (`app/escala/tests/test_o261v_fase_na_lista.py:94`, `:100`, `:111`.)
   - O teste do PDF exige somente o prefixo `%PDF`; um PDF que omitisse C10 e suas datas satisfaria essa asserção, contrariando o contrato protegido pela L-040. O código atual imprime `detalhe`, mas o teste não prende esse conteúdo. (`app/escala/tests/test_o261v_fase_na_lista.py:128`; `app/escala/services/cadastro_realidade.py:727`; `app/docs/LEIS.md:88`.)

Também conferi os pontos pedidos:

- **Remover a integração quebra testes positivos.** O caso real exige C10 e o mock positivo exige exclusivamente o colaborador devolvido pelo leitor. Já o mock vazio continuaria verde sem a integração; a frase “os dois ficam vermelhos” está exagerada. (`app/escala/tests/test_o261v_fase_na_lista.py:97`, `:158`, `:170`, `:12`.)
- **`empresas` filtra corretamente linhas e contadores C10**, porém só depois da varredura global. Cada HTML/PDF e os commands C7/C8 passam a pagar a leitura de toda a frota, incluindo duas janelas de batidas e dois pareamentos por colaborador com batidas. Não reproduzi a medição de “1,0 s”; latência atual **[não verificado]**. (`app/escala/services/cadastro_realidade.py:336`, `:601`, `:626`, `:647`; `app/escala/services/fase_12x36.py:128`, `:140`, `:146`, `:175`; `app/escala/views.py:490`; `app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`.)
- **As chaves novas são aditivas para os consumidores encontrados:** API encaminha os vínculos; fila e propositor extraem IDs; pauta usa campos nomeados. Os commands continuam exigindo especificamente C7/C8 para fechar. (`app/api/views_mensageria.py:1675`; `app/chamados/services/fila_por_causa.py:173`; `app/escala/services/propositor.py:455`; `app/escala/services/fase_12x36.py:225`; `app/escala/management/commands/fechar_avisos_regua_na_lista.py:32`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:40`.)
- **Há dívida anterior no leitor reutilizado:** ele pareia batidas cruas sem o papel da ata; esse caminho já está registrado como classe C, devendo ler `turnos_do_colab`. C10 amplia o consumo dessa dívida, embora não introduza regra própria de fase. (`app/escala/services/fase_12x36.py:146`, `:175`; `app/ponto/turnos.py:1199`, `:1212`, `:1911`; `app/core/juizes.py:905`.)

Não identifiquei no trecho novo segundo escritor, nova leitura de relógio ou alteração da ordem de travas. (`app/escala/services/cadastro_realidade.py:330`, `:488`, `:599`; `app/escala/services/fase_12x36.py:204`.) Suíte Django completa, presença nominal dos quatro dias do col451 e identidade Git da cópia: **[não verificado]**.

## 30/10 08:35 · sessao 5216099b546d · - · - · origem=Ronald (tmux)

Achados concretos:

- **Dia contraditório na tela:** o texto novo pode nomear D+1, mas o modal continua mostrando “Refere-se ao dia D”; a badge deriva de `data_evento`, e o aviso de divergência compara somente `data_evento` com o dia do chamado. O conflito novo entre texto/DNA e badge passa sem aviso. `app/templates/chamados/partials/modal_fio.html:157`, `:159`, `:180`; `app/chamados/services/fio.py:85-88`. O preview do lote e o explicador também mostram identidade junto do texto: `app/templates/chamados/partials/_lote_preview.html:24`; `app/chamados/services/explicador.py:70-71`.

- **Mesmo par divergente exposto pela API:** `data_evento_label` preserva D ao lado do texto D+1; a fila de mensageria expõe a mesma combinação. Impacto na renderização do cliente nativo [não verificado]. `app/api/views.py:1168-1170`; `app/api/views_mensageria.py:564-568`.

- **Selo tem alcance menor que a promessa:** a raiz é só `chamados/`, exclui templates e só lê `.py`; reconhece exclusivamente chamadas atributo `.format(data=...)` e `.replace('{data}', ...)`. F-string, `%`, formato posicional, `str.format(t, data)` e `format_map` passam. `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:120-131`, `:140-149`. Extraí e executei somente `_copias` em memória: confirmou as duas formas reconhecidas e todas essas fugas; o scanner atual devolveu zero ocorrências nas formas cobertas. A cobertura limitada é demonstrável pelo corpo citado; suíte Django/RED histórico [não verificado].

- **Teste não cobre contratos importantes:** chama diretamente o helper, não os seis emissores, e `_dna` deixa os intervalos vazios; não comprova manutenção de `data_evento`, texto integral, motivos externos aos quatro, `padrao`, hora no contexto, DNA ausente/corrompido ou turno 24h com horas coincidentes. `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:36-38`, `:60-82`, `:85-113`.

- **INV4 não verifica o dia mostrado:** chama helper sem colaborador e só compara retorno com `None`; a auditoria de cegas procura qualquer data por regex. Portanto o dia errado no texto permanece fora dessas detecções. `app/chamados/management/commands/auditar_disputas.py:69-81`, `:188-194`.

- **Correção cega sem prova de integração:** o teste simula a aplicação atribuindo/salvando `pergunta`; não exercita handler da ação que o template envia. `app/chamados/tests/test_corrigir_pergunta_cega.py:54-63`; `app/templates/chamados/partials/modal_fio.html:185-190`. Handler funcional da ação nesta árvore [não verificado].

- **Leitor depende do texto exato gravado:** `nota_da_resposta` localiza followup contendo a pergunta inteira; as notas de resposta gravam o texto da pergunta e o incluem no hash de deduplicação. Alterar retrospectivamente esse texto pode afetar a localização; alteração retrospectiva pelo diff [não verificado]. `app/chamados/management/commands/curar_perguntas_em_limbo.py:26-34`; `app/chamados/services/disputa_emissao.py:1314-1321`.

Rastreio adicional:

- Há outro produtor de pergunta com `%`, ignorado pelo selo, na reabertura administrativa; ele não interpola data e é preexistente ao diff fornecido. `app/chamados/services/acoes_disputa.py:259-263`. Outro interpolador atual de `{data}` fora de `chamados/` não foi localizado; inexistência absoluta [não verificado].

- Caso de duas pausas merece ressalva: ambos os pares usam os mesmos motivos, e o juiz de fato lê todos os pares; o novo helper toma somente `hii`/`hfi`. Se os pares estiverem em dias distintos, escolhe o dia do primeiro par. Existência desse caso no dado fornecido [não verificado]. `app/ponto/marcos.py:19-28`, `:35-40`; `app/chamados/juizes.py:823-827`, `:850-869`; `app/chamados/catalogo/query.py:60-67`.

_412 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
