# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 21:09** · publicado em **16/11/2026 21:09** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 21:09 · sessao 87ecc702a040 · - · - · origem=Ronald (tmux)

- **3(b) PROCEDENTE, como possibilidade demonstrável pelo código:** chips e filtro compartilham os pares classificados uma vez; a coluna chama o juiz novamente. Se o catálogo mudar entre essas leituras, chip/filtro podem classificar `descontado` e a coluna pintar `abonado`. O cache pode expirar ou ser invalidado; ocorrência real em produção é **[nao verificado]**. Referências: `app/ponto/views.py:2229`, `app/ponto/views.py:2240`, `app/ponto/views.py:2254`, `app/ponto/views.py:2287`, `app/ponto/services/dia_decidido.py:482`, `app/ponto/catalogo/ausencias.py:267`, `app/ponto/catalogo/ausencias.py:293`, `app/ponto/models.py:1245`.

- **Tipo desconhecido/inativo, catálogo estável: NAO PROCEDENTE para divergência entre os três sítios.** O catálogo lê somente tipos ativos; tipo ausente devolve `None`, e o juiz devolve `ABONADO` para qualquer efeito diferente de desconta/suprime, salvo rejeitada. Os três leitores usam esse juiz; desconhecido aprovado entra no balde Abona, desconhecido rejeitado no balde rejeitadas. O desaparecimento do `?` produz, portanto, ABONA para desconhecido não rejeitado, por decisão preexistente do juiz. Referências: `app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:293`, `app/ponto/services/dia_decidido.py:475`, `app/ponto/services/dia_decidido.py:483`, `app/ponto/services/dia_decidido.py:487`, `app/templates/ponto/ausencias.html:158`.

- **Total igual à soma dos quatro baldes com desconhecido: NAO PROCEDENTE para quebra, sob dados estáveis.** Todo par passa por juiz que retorna exatamente um dos quatro vereditos; rejeitada vence antes de consultar efeito. O contador rejeitadas é pelo mesmo status literal. Referências: `app/ponto/views.py:2229`, `app/ponto/views.py:2238`, `app/ponto/views.py:2240`, `app/ponto/services/dia_decidido.py:475`, `app/ponto/services/dia_decidido.py:487`.

- **`?efeito=xyz`/`abonado`/`rejeitado`: NAO PROCEDENTE para 500 ou corte indevido direto.** A inversão do mapa aceita somente `abona`, `desconta`, `suprime`; fora dele, não aplica filtro. O contexto ainda conserva o valor recebido. Referências: `app/ponto/services/dia_decidido.py:80`, `app/ponto/views.py:2232`, `app/ponto/views.py:2249`, `app/ponto/views.py:2301`.

- **`?efeito=abona` sem pares: NAO PROCEDENTE para listar tudo.** O acumulador começa em `None` e termina em `qs.none()` quando nenhum par pertence ao veredito pedido. Referências: `app/ponto/views.py:2253`, `app/ponto/views.py:2254`, `app/ponto/views.py:2257`.

- **`?registro=<pk>` fora do mês: comportamento global preservado.** O PK substitui inteiramente o queryset de vigência; colaborador/tipo e depois status/efeito ainda restringem essa busca. `q_universo` conserva o PK e o JS tenta abrir seu painel. Mês/data exibidos não passam a ser a data desse registro. Referências: `app/ponto/views.py:2189`, `app/ponto/views.py:2192`, `app/ponto/views.py:2195`, `app/ponto/views.py:2209`, `app/ponto/views.py:2241`, `app/templates/ponto/ausencias.html:200`.

- **`?de=lixo`: NAO PROCEDENTE para 500 dessa conversão.** `_d` devolve `None`, `de` usa primeiro dia do mês e o trecho dos chips omite a data inválida. O formulário mostra a data efetiva. `_de_pedido`/`_ate_pedido` são datas válidas solicitadas; `de`/`ate` são limites efetivos com defaults. Referências: `app/ponto/views.py:2181`, `app/ponto/views.py:2186`, `app/ponto/views.py:2205`, `app/ponto/views.py:2304`, `app/templates/ponto/ausencias.html:72`.

- **Ordenação no GROUP BY: NAO PROCEDENTE para quebra.** `.order_by()` limpa a ordenação antes de `values/annotate`, inclusive a ordenação herdada do modelo. Referências: `app/ponto/views.py:2229`, `app/ponto/models.py:829`.

- **MUDA c, literal “uma função só usada por b e c”: cumpre em parte.** Existe uma classificação compartilhada em dicionário dentro da view; não foi extraída uma função comum. A mesma estrutura alimenta soma e construção do Q. Referências: `app/ponto/views.py:2228`, `app/ponto/views.py:2240`, `app/ponto/views.py:2254`.

- **3(a), 3(f), 3(g): NAO PROCEDENTE no trecho examinado.** A view chama o juiz e não escreve regra nova de efeito/rejeição; escreve somente atributos transitórios de apresentação, e o mapa inverso é derivado do mapa único. A regra do ANEXO! permanece em autoridade separada, já existente. Referências: `app/ponto/views.py:2230`, `app/ponto/views.py:2232`, `app/ponto/views.py:2287`, `app/ponto/services/ausencia.py:745`, `app/ponto/models.py:867`.

- **3(e): sem relógio novo explícito no diff; existe dependência transitiva do cache.** A data atual é lida na entrada da view; o catálogo consulta `monotonic()` internamente em cada avaliação de validade do cache. Referências: `app/ponto/views.py:2169`, `app/ponto/catalogo/ausencias.py:274`.

- **Leitor JS externo confirmado:** o motor de tabela usa `data-value` para largura e ordenação, mas trata-o como texto genérico, sem exigir os antigos literais `abona/desconta/suprime`. Referências: `app/static/js/hasner-table.js:35`, `app/static/js/hasner-table.js:189`, `app/static/js/hasner-table.js:207`.

- **Semântica após edição na mesma tela:** o JS de salvar altera visualmente Data e Tipo, sem recalcular coluna Efeito ou chips; essa limitação é externa ao diff fornecido e anterior à mudança da fonte da coluna. Referências: `app/templates/ponto/ausencias.html:260`, `app/templates/ponto/ausencias.html:267`, `app/templates/ponto/ausencias.html:268`.

## 16/11 21:08 · sessao 1e152ed22ba8 · - · - · origem=Ronald (tmux)

Censo somente por leitura; execução em banco/log e comportamento real de produção permanecem [nao verificado].

- **Campos persistidos**: `Ausencia.tipo` é `CharField` e `status` tem quatro escolhas; defaults/definições em `app/ponto/models.py:749`, `app/ponto/models.py:700`, `app/ponto/models.py:809`. A porta de criação escreve ambos em `app/ponto/services/ausencia.py:483`; edição escreve `tipo` em `app/ponto/services/ausencia.py:648`; transição escreve `status` em `app/ponto/services/ausencia.py:976`, chamada por decisão em `app/ponto/services/ausencia.py:1088`. Há escritor especial preexistente de rejeição de férias em `app/ponto/services/ausencia.py:1496`. O diff da lista apenas lê esses campos para agrupar/filtrar e julgar: `app/ponto/views.py:2229`, `app/ponto/views.py:2256`, `app/ponto/views.py:2287`.

- **`Ausencia.efeito`**: property sem armazenamento/setter, devolve `efeito_vigente(tipo)`, sem status: `app/ponto/models.py:849`. Leitores externos reais são `app/ponto/turnos.py:2704`, `app/ponto/services/fechamento.py:697`, `app/ponto/services/efeito_ausencia.py:58`, `app/colaboradores/services/calendario.py:705`. O significado permanece efeito do tipo, diferente do vocabulário do veredito da nova célula, que escreve `abonado/descontado/suprimido/rejeitado`: `app/ponto/services/dia_decidido.py:475`, `app/ponto/services/dia_decidido.py:482`, `app/templates/ponto/ausencias.html:158`.

- **Rejeitada NÃO entra no mapa do calendário**: mapa nasce em `_cap(..., para='cobranca')` em `app/colaboradores/services/calendario.py:516`; `_cap` chama `_pelo_efeito` em `app/ponto/turnos.py:2765`; para cobrança, exclui `status='rejeitada'` em `app/ponto/turnos.py:2531`. Fechamento e `efeito_ausencia` pedem cobertura de folha (`app/ponto/services/fechamento.py:683`, `app/ponto/services/efeito_ausencia.py:53`), que só admite aprovada (`app/ponto/turnos.py:2529`). `dias_com_ausencia_de_dia_inteiro` recebe cobertura já filtrada, e seu wrapper pede folha (`app/ponto/turnos.py:2695`, `app/ponto/turnos.py:2709`).

- **`contradiz_documento`**: property delega em `app/ponto/models.py:867`; regra lê documento e efeito vigente, sem considerar status (`app/ponto/services/ausencia.py:753`). Leitores: selo ANEXO! em `app/templates/ponto/ausencias.html:158`; gate de aprovação em `app/ponto/services/ausencia.py:765`; título do chamado em `app/ponto/services/ausencia.py:802`; testes externos em `app/ponto/tests/test_gate_anexo_desconta.py:28`, `app/ponto/tests/test_o137_esmeril_ausencia_le_o_cadastro.py:109`. Como não considera status, uma rejeitada de tipo que desconta COM documento também pode exibir ANEXO!; o fixture novo só nega ANEXO! para rejeitada SEM documento (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:99`, `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:233`).

- **`veredito` / `apresentacao` de linha**: escritor em memória `app/ponto/views.py:2287` / `app/ponto/views.py:2288`; leitor célula `app/templates/ponto/ausencias.html:158`; são atributos transitórios, não campos declarados no trecho do modelo (`app/ponto/models.py:743`, `app/ponto/models.py:849`). `APRESENTACAO_AUSENCIA` é declarado em `app/ponto/services/dia_decidido.py:80`; leitores produtivos encontrados são inversão URL→veredito (`app/ponto/views.py:2232`) e lookup para cada linha (`app/ponto/views.py:2288`). Testes leem mapa em `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:214`, `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:239`, e o alteram temporariamente por mock em `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:307`.

- **Leitor JS externo do `data-value` existe**, mas usa string genérica: ajusta largura em `app/static/js/hasner-table.js:36`; ordenação lê e converte a minúsculas em `app/static/js/hasner-table.js:191`, ordena em `app/static/js/hasner-table.js:207`. Script é carregado em `app/templates/base.html:84`. Assim passa a ordenar/medir pelo veredito novo; não há nesse leitor tabela que exija literalmente `abona/desconta/suprime` ou `"?"` (`app/static/js/hasner-table.js:189`). A busca livre usa o conteúdo da linha, chamada em `app/templates/ponto/ausencias.html:96`.

- **Leitor JS externo de `registro` existe**: autoabre o drawer lendo `location.search`, sem depender da ordem da query string, em `app/templates/ponto/ausencias.html:200`. Alteração de tipo pelo drawer atualiza só célula Tipo/Data; não atualiza Efeito/chips até recarregar — comportamento preexistente em `app/templates/ponto/ausencias.html:260`, `app/templates/ponto/ausencias.html:265`, `app/templates/ponto/ausencias.html:268`.

- **API não lê atributos da nova linha nem HTML/chips**: o GET consulta `Ausencia` diretamente, em `app/api/views.py:1853`; emite tipo/status em `app/api/views.py:1865` e `app/api/views.py:1867`. Dependência de outros clientes externos ao código disponível é [nao verificado].

- **Tipos desconhecidos/inativos não quebram a partição**: catálogo consulta somente ativos (`app/ponto/catalogo/ausencias.py:249`); `efeito_vigente` devolve `None` quando ausente (`app/ponto/catalogo/ausencias.py:293`); juiz decide rejeitada primeiro e, fora desconta/suprime, retorna ABONADO (`app/ponto/services/dia_decidido.py:475`, `app/ponto/services/dia_decidido.py:487`). Portanto registros desconhecidos/inativos não rejeitados entram no balde Abona; coluna e filtro acompanham essa mesma regra enquanto catálogo está estável (`app/ponto/views.py:2230`, `app/ponto/views.py:2254`, `app/ponto/views.py:2287`). É comportamento preexistente do juiz, agora refletido pela lista (`app/ponto/services/dia_decidido.py:467`).

- **Risco real de divergência temporal, classe (b), PROCEDENTE**: chips/filtro usam vereditos dos pares calculados em `app/ponto/views.py:2230`; coluna chama o juiz novamente em `app/ponto/views.py:2287`. Cada chamada vai a `CAT.efeito_vigente` (`app/ponto/services/dia_decidido.py:482`), cujo catálogo tem cache por processo com TTL de 60 segundos e lê `monotonic()` internamente (`app/ponto/catalogo/ausencias.py:257`, `app/ponto/catalogo/ausencias.py:275`). Se catálogo é atualizado e cache vence/invalida entre fases, chips/filtro podem manter ABONADO enquanto célula vira DESCONTADO. Signal invalida em post-save/post-delete no mesmo processo (`app/ponto/models.py:1245`), e outro processo observa atualização após TTL (`app/ponto/catalogo/ausencias.py:276`). Não há snapshot do catálogo/veredito compartilhado entre fases (`app/ponto/views.py:2230`, `app/ponto/views.py:2287`). Ocorrência em produção é [nao verificado].

- **Relógio**: diff não introduz chamada de relógio direta na lista; a borda já lê `date.today()` em `app/ponto/views.py:2169`. A chamada indireta ao juiz chega ao relógio interno preexistente do catálogo (`app/ponto/services/dia_decidido.py:482`, `app/ponto/catalogo/ausencias.py:275`); distinguir “relógio novo no diff” de “dependência temporal preexistente com nova sequência de leituras” evita acusação excessiva.

- **Datas**: `de/ate` de contexto são datas efetivas (fallback mensal), escritas em `app/ponto/views.py:2187`, entregues em `app/ponto/views.py:2304`, lidas nos inputs em `app/templates/ponto/ausencias.html:72`. `_de_pedido/_ate_pedido` são datas válidas explicitamente recebidas e só estas vão à query de chips (`app/ponto/views.py:2186`, `app/ponto/views.py:2207`). São variáveis distintas com significados distintos; não há troca silenciosa entre elas nesses consumidores (`app/ponto/views.py:2186`, `app/ponto/views.py:2304`).

## 16/11 21:08 · sessao 9202c61e1543 · - · - · origem=Ronald (tmux)

Auditoria estática; execução de RED/GREEN/mutantes/suite: **NAO SEI [nao verificado]**. Os testes usam `django.test.TestCase` e criam registros; não os executei, respeitando somente leitura (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:22,80-101`).

- **RESULTADOS 1–3: PROCEDENTE quanto ao teste escrito.** R1 exige 13 linhas, acessa a rejeitada conhecida e confere texto, title, classe e data-value; R2 exige números concretos; R3 exige lista não vazia, exclui a rejeitada do efeito e inclui três rejeitadas pelo status (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:131-157`).
- **RESULTADO 4: PROCEDENTE quanto ao teste escrito.** Cobre ambos os estados abertos, badge ABONA, contador 4 e interseção Abona+Pendente com duas linhas (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:161-171`).
- **5a/5b/5c: PROCEDENTE na fixture estável escrita.** Total fixado em 13, partição conferida no contexto e no HTML; 5b exige chip positivo e segue os hrefs; 5c exige três rejeitadas (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:175-197`).
- **RESULTADO 6: PROCEDENTE na fixture escrita.** Exige 13 linhas, compara data-value/texto/title do HTML, exige 12 combinações e os quatro vereditos; ANEXO! recebe teste separado (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:213-233`).

**M1–M5 têm mordida estática identificável; mortes efetivamente executadas: NAO SEI [nao verificado].**

| Mutante | Teste que detectaria a restauração literal | [nao verificado]
|---|---|
| M1, coluna volta a `a.efeito` | Regex proíbe a leitura; R1 exige rejeitada SEM EFEITO (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:275-277,131-140`). |
| M2, contador antigo por tipo | R2 espera Abona=4, enquanto fixture tem cinco registros daquele tipo; AST detecta os nomes antigos (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:89-98,142-148,255-266,286-294`). |
| M3, filtro antigo por tipo | R3 exige que a rejeitada conhecida fique fora de Abona, com lista explicitamente não vazia (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:150-157`). |
| M4, badge literal no template | Selo proíbe os literais; teste substitui texto/title do mapa por sentinelas e exige essas sentinelas no HTML (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:279-284,301-312`). |
| M5, link perde de/ate | Exige ambos nos sete chips; segue cinco links e exclui registro do dia 20 (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:318-329`). |

**Vacuidade: NAO PROCEDENTE nos testes centrais escritos.** Existem sentinelas de população: 13 linhas em R1/R6, lista não vazia em R3, chip positivo em 5b, três rejeitadas em 5c, sete chips em C2 (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:133,153,188,197,217,321`). O selo AST exige que encontre a função; o selo da célula exige exatamente uma célula antes da comparação (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:280-288`).

O teste de cobertura do mapa usa apenas diferença de conjuntos, sem assert explícito de catálogo não vazio; porém o caminho atual de `vigente()` usa `CATALOGO` como alternativa não vazia, impedindo esse vazio no caminho normal (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:247-252`; `app/ponto/catalogo/ausencias.py:87-105,249-254,267-281`). O selo AST cobre os três nomes proibidos, não toda possível regra paralela escrita com literais novos (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:255-266`).

**Lacuna material: PROCEDENTE para divergência sob mudança do catálogo durante a requisição.** Contador e filtro guardam o julgamento dos pares, mas a coluna chama novamente `veredito_de` depois; esse juiz consulta novamente `efeito_vigente`, cujo catálogo pode ser recarregado após TTL de 60 segundos ou invalidação. Com efeito alterado no banco e recarga entre esses pontos, chip/filtro ficam com o julgamento anterior e coluna com o posterior (`app/ponto/views.py:2229-2240,2254-2257,2287`; `app/ponto/services/dia_decidido.py:482-495`; `app/ponto/catalogo/ausencias.py:257-281,293-295`). Ocorrência efetiva em produção: **[nao verificado]**.

Nenhum teste deste arquivo encena essa troca, tipo desconhecido ou tipo inativo: a fixture constrói três tipos fixos nos quatro status, e `setUp` invalida o cache antes do pedido (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:32-35,89-109`). Assim, a suíte escrita poderia passar mesmo existindo a divergência descrita (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:213-227`; `app/ponto/views.py:2229,2287`).

**Bordas cobertas por testes escritos:** efeito fora do mapa, efeito válido sem par, corte válido de datas, registro específico, datas inválidas (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:199-209,318-348`). O teste de registro usa um registro do mesmo mês, portanto não demonstra especificamente o caso fora do mês (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:91-93,331-339`).

**Prova histórica de RED e mutantes: NAO SEI [nao verificado].** As entradas desta obra encontradas no RELATO registram revisões de aval e leitura anterior, não a execução dos novos testes; o próprio cabeçalho do teste descreve o defeito, sem anexar uma execução (`app/docs/RELATO.md:4,9,17-18,43,46`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:1-10`).

## 16/11 20:55 · sessao 815f9174063b · ESPINHA-F2 · verificador · origem=Code

**AVAL-DEVOLVIDO no item 2, pelo literal “descarimbou → volta à lista”: o descarimbo não basta para inclusão. Item 1 CONFERE.** A lista também exige ausência de validação, presença de instante ou texto de resposta e tratamento da tranca (`app/chamados/models.py:1923`; `app/chamados/models.py:1924`; `app/chamados/models.py:1926`).

Referências aos anexos contam suas linhas desde o conteúdo de cada arquivo, incluindo vazias. Correspondência independente da cópia com o hash informado: [nao verificado].

**ITEM 1 — 2d**

1. **Premissas conferem.** A função lê `decisoes[-1]`: RESPOSTA com texto vazio exige RESPONDER do colaborador; DEVOLUCAO exige VALIDAR do admin; SILENCIO retorna ações vazias (`app/chamados/services/tarefa_do_dia.py:158`; `app/chamados/services/tarefa_do_dia.py:161`; `app/chamados/services/tarefa_do_dia.py:166`; `app/chamados/services/tarefa_do_dia.py:177`).

   Além desses, existem **VALIDACAO, RECUSA, REABERTURA, RETRATACAO e ABSORCAO**. RECUSA/REABERTURA devolvem ao colaborador; VALIDACAO depende da lavra/efeito; ABSORCAO depende do status da ausência. RETRATACAO é excluída antes desse julgamento; também existem os tratamentos de ausência de decisão, passivo sem linha e ato desconhecido (`app/chamados/models.py:4024`; `app/chamados/models.py:4025`; `app/chamados/services/tarefa_do_dia.py:154`; `app/chamados/services/tarefa_do_dia.py:163`; `app/chamados/services/tarefa_do_dia.py:167`; `app/chamados/services/tarefa_do_dia.py:178`; `app/chamados/services/tarefa_do_dia.py:187`; `app/chamados/services/tarefa_do_dia.py:260`).

2. **Não colide com os pontos preservados do C1.** Medir por último ato corrige a inferência anterior sem mudar dono, tarefa ou `aguarda_admin`; este continua derivado de `estado == RESPONDIDA`. O complemento restringe somente o encaminhamento adicional à B2, sem autorizar cura pela F2 (`complemento2_respostas_f2_estados_1611.txt:2`; `complemento1_respostas_f2_estados_1611.txt:3`; `complemento1_respostas_f2_estados_1611.txt:4`; `app/api/views.py:2553`).

3. **A condição fecha como medida, não como pouso.** A contagem deve distinguir o texto projetado vazio do `ato_texto` da última RESPOSTA: são entradas diferentes. “Sem decisão” também não elimina a precedência de `passivo_sem_linha` ao apurar o resultado efetivo da tarefa (`app/chamados/juizes.py:1552`; `app/chamados/services/tarefa_do_dia.py:154`; `app/chamados/services/tarefa_do_dia.py:156`; `app/chamados/services/tarefa_do_dia.py:160`). Quantidades, execução da sonda real e distribuição em produção: [nao verificado].

4. **Não amplia intervenção em exportada, dinheiro ou `!`.** O item manda medir e registrar, mantendo os ramos existentes; alteração material efetiva em produção: [nao verificado] (`complemento2_respostas_f2_estados_1611.txt:2`; `complemento2_respostas_f2_estados_1611.txt:5`).

**ITEM 2 — 9a**

1. **As referências de código conferem; o inventário de leitores precisa ser delimitado.** Há `update()` direto do trio no descarimbo em lote e escritas somente da via nos outros dois locais. O descarimbo individual apaga o instante; a projeção temporal parte do campo atual; os tipos de decisão não incluem materialização (`app/chamados/models.py:3391`; `app/chamados/models.py:3393`; `app/chamados/models.py:3402`; `app/chamados/services/relavra_via.py:125`; `app/chamados/models.py:3318`; `app/chamados/juizes.py:1390`; `app/chamados/models.py:4024`).

   `sem_veredito_entre` exclui qualquer materializada; `estado` tem precedência de validação, vida e materialização; `supra_juiz` consome aquela mesma lista para pai e destino. Portanto, são três pontos observáveis, com delegação entre eles (`app/chamados/models.py:1923`; `app/chamados/models.py:3151`; `app/chamados/models.py:3154`; `app/chamados/models.py:3156`; `app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:574`).

   **Não são todos os leitores da F2:** também existem `respondida`, `resposta_display` e `q_vivas`; há ainda outro consumidor da lista em `irmas_respondidas_sem_consumo` (`estados.md:1`; `app/chamados/models.py:3665`; `app/chamados/models.py:3688`; `app/chamados/models.py:3496`; `app/chamados/juizes.py:2105`).

2. **A escolha de conservar o campo atual é compatível com o cabeçalho de conservação do corte e com 9b/9c.** Resolve L1 pelo comportamento vigente e remete somente o predicado histórico novo à E4d; não remete a solução atual de L1 para lá (`app/docs/CORTES.md:487`; `complemento1_respostas_f2_estados_1611.txt:9`; `complemento1_respostas_f2_estados_1611.txt:10`; `complemento2_respostas_f2_estados_1611.txt:3`).

   Assim, **RESIDUAL → RESOLVIDO-PELO-CORTE é coerente como resposta à escolha pendente**, anteriormente registrada como “conservar lendo o campo” ou “trazer o ato”; não significa recuperação do instante nem MOOT pela F1 (`app/docs/RELATO.md:24`; `app/docs/RELATO.md:131`). Coerência com todas as demais linhas da matriz, cujo conteúdo integral não foi fornecido: [nao verificado].

3. **O selo é executável, mas a equivalência incondicional não fecha.** Descarimbar não garante retorno à lista: validação remanescente exclui; ausência simultânea de instante/texto exclui; tranca pode excluir. Há um caminho concreto que descarimba e zera também instante e texto: ressuscitar (`app/chamados/models.py:1923`; `app/chamados/models.py:1924`; `app/chamados/models.py:1926`; `app/chamados/models.py:1629`; `app/chamados/models.py:1630`).

   O caso que morde deve ser explicitado: **pergunta dentro do fato, sem validação, com resposta preservada e sem tranca — ou com `ignorar_tranca=True`**. Nessa situação, descarimbar remove a exclusão por materialização. A igualdade compara as implementações antes/depois da F2, tanto no cenário carimbado quanto no descarimbado; não compara esses dois estados entre si (`app/chamados/models.py:3316`; `app/chamados/models.py:3318`; `app/chamados/models.py:3320`; `app/chamados/models.py:1923`).

   O selo desses três pontos não substitui a prova geral dos demais leitores. Igualdade estrita também não contradiz diferenças anteriormente **admitidas**, pois elas não eram obrigatórias (`respostas_registradas.md:5`; `estados.md:1`; `complemento2_respostas_f2_estados_1611.txt:4`). Execução comparativa na sombra inteira: [nao verificado].

   **Censo direto da projeção com instante:**

   | Chamador produtivo | Argumento | Consumo |
   |---|---|---|
   | `_slot_esperado` | `agora`, inclusive vindo da carga | Usa apenas a resposta projetada (`app/chamados/juizes.py:348`; `app/chamados/juizes.py:351`; `app/chamados/juizes.py:352`). |
   | `_pergunta_viva_calc` | `agora` | Usa materialização nos ramos de vida (`app/chamados/juizes.py:1404`; `app/chamados/juizes.py:1416`; `app/chamados/juizes.py:1444`; `app/chamados/juizes.py:1482`). |
   | `beco_em_lote` | `agora` | Usa somente `[1]`, instante da resposta (`app/escala/services/cadastro_realidade.py:502`). |

   Nos testes, chamadas por aliases passam instante em `app/chamados/tests/test_e4b_classe_e_memoria.py:383`, `app/chamados/tests/test_e4b_classe_e_memoria.py:389`, `app/chamados/tests/test_e3_c8c_sem_banco.py:134`, `app/chamados/tests/test_e3_c8c_sem_banco.py:137` e `app/chamados/tests/test_e3_c8c_sem_banco.py:138`; esses casos recortam carimbos futuros, sem demonstrar reconstrução após descarimbo.

   **CENSO = 0 casos comprovados no código do predicado completo pedido.** Os três pontos do selo não fornecem instante histórico: a lista não recebe `agora`, `estado` chama C6 sem ele e `supra_juiz` chama a lista sem ele. Isso não significa ausência de consultas temporais: C6 aceita instante e a tarefa o fornece (`app/chamados/models.py:1910`; `app/chamados/models.py:3154`; `app/ponto/supra_juiz.py:573`; `app/chamados/juizes.py:887`; `app/chamados/services/tarefa_do_dia.py:537`). Ocorrência em produção de consulta anterior a descarimbo: [nao verificado].

4. **Existe alcance concreto sobre o CSF, já declarado pelo corte.** A lista participa da permissão para executá-lo; qualquer diferença em competência exportada continua exigindo `PAREI !` com tabela. O complemento não dispensa essa restrição (`app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:588`; `app/docs/CORTES.md:487`; `complemento2_respostas_f2_estados_1611.txt:5`). Alteração efetiva de exportadas ou dinheiro: [nao verificado].

VEREDITO por item: item 1 CONFERE; item 2 PROCEDE; CENSO = 0.

## 16/11 20:54 · sessao 2c9681d29de9 · AUSENCIA-EFEITO-PELO-ESTADO · verificador · origem=Code

**AVAL-DEVOLVIDO:** há conflito com o universo preservado pelo C1, erro na associação M-a → RED 1 e ressalvas no literal de consultas e fonte única. (`complemento1_ausencia_efeito_1611.txt:5-6,9-10`; `complemento3_ausencia_efeito_1611.txt:4-9`.)

Nos anexos, as referências abaixo contam linhas desde `aval Ronald`, excluindo o cabeçalho `== anexo... ==`.

1. **Premissas, consultas e REDs**

O `_cnt` contém **sete chamadas `.count()`**: Total, Pendente, Aprovada, Rejeitada, Abona, Desconta e Suprime. Existe **uma oitava contagem**, `pendentes`, imediatamente antes dele. Portanto, “sete contagens no `_cnt`” confere; “sete consultas na view inteira” não confere. (`app/ponto/views.py:2197-2211`.)

Além dessas oito contagens, há a avaliação de `_cand`, a carga `list(qs)` e a consulta dos colaboradores consumida pelo formulário: **onze avaliações ORM identificáveis nesse caminho**, antes de acrescentar consultas dos serviços, catálogo e renderização. Cada candidato chama `resumo_atestados`, que busca sua cobertura; o catálogo consulta o banco quando seu cache vence. O total SQL efetivamente executado depende desses caminhos e dos dados/cache; um número exato em produção é **[não verificado]**. (`app/ponto/views.py:2231-2235,2248,2257`; `app/templates/ponto/ausencias.html:78-80`; `app/ponto/services/alertas_ausencia.py:44-50`; `app/ponto/turnos.py:2765-2766,2783`; `app/ponto/catalogo/ausencias.py:249-251,267-281`.)

`pendentes` é lido ao montar o contexto, mas **não alimenta o chip Pendente**: esse chip lê `count_pendente`, derivado de `_cnt['pendente']`. Não há referência a `pendentes` no template da lista; outro consumidor desse campo específico é **[não verificado; não localizado]**. (`app/ponto/views.py:2256,2267`; `app/templates/ponto/ausencias.html:52-54`.)

**RED 1 fecha como consequência da facetada:** Suprime conta depois de aplicar `status=rejeitada`; rejeitada recebe `REJEITADO`, nunca `SUPRIMIDO`. O número e o clique serão zero. Na cópia atual, o contador ignora status e conta pelo tipo, enquanto o link preserva `status=rejeitada`. É um contraexemplo estático, sem ocorrência produtiva demonstrada. (`complemento3_ausencia_efeito_1611.txt:4,6`; `app/ponto/services/dia_decidido.py:461-473`; `app/ponto/views.py:2211-2226`; `app/templates/ponto/ausencias.html:67`.)

**RED 2 também fecha**, para a requisição indicada, sem efeito: o chip Rejeitada substitui o status da própria dimensão e lista a rejeitada, embora a tela original estivesse em Aprovada. Aplicar Aprovada ao contador Rejeitada faria zero e mataria M-b. (`complemento3_ausencia_efeito_1611.txt:4,6-7`; `app/templates/ponto/ausencias.html:56-62`.)

**M-a está associado ao RED errado:** contar **status** sobre `_base` cru não altera o chip **Suprime**, observado pelo RED 1. Para matar M-a, use, por exemplo, `?efeito=suprime` com uma rejeitada na base: o contador correto Rejeitada é zero; o mutante conta a rejeitada. Alternativamente, redefina M-a como contador de **efeito** sobre `_base` cru. (`complemento3_ausencia_efeito_1611.txt:6-7`; `app/templates/ponto/ausencias.html:60-67`; `app/ponto/services/dia_decidido.py:461-473`.)

2. **Compatibilidade com o aval vigente**

O predicado e o resultado 6 continuam compatíveis: `veredito_de` delega ao núcleo por tipo/status; rejeitada vence o efeito, e as filas abertas continuam recebendo o efeito do tipo. A facetada não exige alterar badge, mapa ou juiz. (`complemento1_ausencia_efeito_1611.txt:2-4`; `app/ponto/services/dia_decidido.py:453-481`.)

**O universo dos contadores, porém, muda.** O C1 preserva expressamente todos os chips e o Total sobre `_base`, proíbe mudar esse universo e exclui o cruzamento efeito × status do escopo. O C3 manda aplicar esse cruzamento, embora diga manter o C1 inteiro. A frase “RESULTADO 5 substitui o texto anterior” precisa esclarecer essa revogação, inclusive das cláusulas de universo e proibição. (`complemento1_ausencia_efeito_1611.txt:5-6,9-10`; `complemento3_ausencia_efeito_1611.txt:1,4-5`.)

**A igualdade 5a entre os números exibidos não vale para toda R — nem para toda R sem status.** Pela facetada:

- Os três chips de efeito contam sobre a base filtrada pelo status.
- Rejeitada e Todos contam sobre a base filtrada pelo efeito.

Logo, eles podem representar universos diferentes. (`complemento3_ausencia_efeito_1611.txt:4`; `app/templates/ponto/ausencias.html:48-67`.)

Dois contraexemplos derivados dessas regras: com um aprovado que suprime e uma rejeitada, `?status=rejeitada` produz soma dos efeitos `0`, Rejeitada `1` e Todos `2`; sem status, com um aprovado que abona e outro que suprime, `?efeito=suprime` produz soma dos efeitos `2`, Rejeitada `0` e Todos `1`. São cenários lógicos, não dados de produção. (`complemento3_ausencia_efeito_1611.txt:4`; `app/ponto/services/dia_decidido.py:461-473`.)

A partição **calculada integralmente sobre `_base`** continua válida pelo juiz, mas deixa de ser uma identidade geral dos chips facetados. Como garantia geral dos números exibidos, ela permanece quando **nenhuma das duas dimensões aplica corte**; outras requisições podem satisfazê-la apenas conforme os dados. (`complemento1_ausencia_efeito_1611.txt:6`; `complemento3_ausencia_efeito_1611.txt:4`; `app/ponto/services/dia_decidido.py:461-473`.)

Portanto, **Todos = quantidade de `por_efeito(_base, efeito)`** acompanha corretamente seu href, que remove status e conserva efeito, mas **deixa de ser o Total irrestrito do 5a** quando efeito recorta a base. (`complemento3_ausencia_efeito_1611.txt:4`; `app/templates/ponto/ausencias.html:48-50`.)

3. **Condições literais**

**A facetada não exige quatorze consultas.** Sete métricas podem sair de agregação condicional ou de sete contagens com predicados SQL já disponíveis. Entretanto, Python não pode ser chamado diretamente pelo SQL: `veredito_de` é uma função Python e consulta o catálogo vigente. (`complemento3_ausencia_efeito_1611.txt:4,9`; `app/ponto/services/dia_decidido.py:453-481`; `app/ponto/catalogo/ausencias.py:293-295`.)

O caminho já autorizado é agrupar `_base` por `(tipo,status)`, classificar cada par pelo juiz e usar essa mesma classificação para somar e construir o filtro. **É viável não aumentar consultas substituindo avaliações existentes por essa alimentação compartilhada.** Acrescentar a consulta agrupada e conservar intactas as sete contagens acrescentaria uma avaliação; “sete números” não significa “sete consultas obrigatórias”. A implementação e seu orçamento demonstrado são **[não verificado]**. (`ausencia_efeito_pelo_estado_1611.txt:9-10`; `app/ponto/views.py:2202-2211`; `app/ponto/services/dia_decidido.py:453-481`.)

Há ainda uma exceção literal a declarar: “ninguém repete filtro de status fora das duas funções” conflita com manter `pendentes` **intocado**, pois ele já executa seu próprio `.filter(status__in=...)`. Pode permanecer como exceção expressa; sem essa ressalva, as duas exigências não fecham simultaneamente. (`complemento3_ausencia_efeito_1611.txt:4,9`; `app/ponto/views.py:2197`.)

`pendente` deve continuar significando **a união das duas filas abertas**, tanto no filtro quanto no chip; não é um quinto estado persistido. Os estados reais são aguardando documento, aguardando decisão, aprovada e rejeitada. (`app/ponto/services/ausencia.py:912`; `app/ponto/models.py:690-704`; `app/ponto/views.py:2212-2219`; `app/templates/ponto/ausencias.html:52-54`.)

Com **`?status=` vazio**, `por_status` não corta. Todos conta a base filtrada pelo efeito; sem efeito efetivo, conta `_base`. Seu href omite status, e o template o marca ativo quando `filtro_status` está vazio. Desconhecido também não corta pela regra proposta, embora um desconhecido não vazio não ative visualmente Todos no template atual. (`complemento3_ausencia_efeito_1611.txt:4`; `app/templates/ponto/ausencias.html:48-50`.)

**RED 3 deve incluir** `aguardando_documento` e `aguardando_decisao` como requisições de origem, além do alias `pendente`: ambos são aceitos pela view, embora não tenham chips próprios. Em cada origem, o laço verifica os sete chips realmente renderizados; os chips de efeito preservam inclusive esses status específicos. (`complemento3_ausencia_efeito_1611.txt:4,6`; `app/ponto/views.py:2212-2219`; `app/templates/ponto/ausencias.html:48-67`.)

4. **Exportada, dinheiro e `!`**

No escopo de classificar, contar e filtrar a lista, **não aparece escrita em competência exportada nem alteração de cálculo financeiro**: o caminho consulta, prepara objetos e renderiza. Existe também selo exigindo que `lista_ausencias` permaneça sem leitura de fechamento. (`app/ponto/views.py:2165-2270`; `app/ponto/tests/test_k8_tela_abre_na_competencia.py:272-295`.)

O `ANEXO!` continua separado do badge de efeito e usa seu próprio predicado. Alterar transições, gravações ou projeção da folha ultrapassaria essa mudança: essas operações existem em caminhos próprios e podem acordar a projeção financeira. Não há necessidade adicional de `!` demonstrada para o escopo restrito; as reservas da casa continuam aplicáveis. (`complemento1_ausencia_efeito_1611.txt:3`; `app/templates/ponto/ausencias.html:158`; `app/ponto/services/ausencia.py:745-759,987-994`; `CLAUDE.md:265-268`.)

Identidade da cópia com o hash informado, total real de SQL, implementação, REDs executados, mutantes mortos e comportamento em produção: **[não verificado]**.

VEREDITO: PROCEDE (item 2 substituído pelo C3: conflito com universo/5a preservados no C1 — complemento1_ausencia_efeito_1611.txt:5-6,9-10; complemento3_ausencia_efeito_1611.txt:1,4-5; M-a não é morto pelo RED 1 — complemento3_ausencia_efeito_1611.txt:6-7; ressalvas literais de consultas e `pendentes` — app/ponto/views.py:2197-2211,2231-2257; complemento3_ausencia_efeito_1611.txt:4,9).

## 16/11 20:53 · sessao 3d950d86a9bf · - · - · origem=Ronald (tmux)

- **Os três citados são um recorte de L1, não todos os leitores F2.** `supra_juiz` delega ao mesmo `sem_veredito_entre`; há também `respondida`, `resposta_display` e `q_vivas` (`app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:574`; `app/chamados/models.py:3665`; `app/chamados/models.py:3688`; `app/chamados/models.py:3496`). Outro consumidor direto do juiz é `irmas_respondidas_sem_consumo` (`app/chamados/juizes.py:2105`).

- **RESIDUAL → RESOLVIDO-PELO-CORTE pode ser coerente como decisão de conservar o comportamento atual.** O registro anterior apresenta precisamente a escolha pendente: conservar lendo o campo e aceitando perder o instante, ou trazer o ato; C2 escolhe a primeira alternativa (`app/docs/RELATO.md:24`; anexo `complemento2_respostas_f2_estados_1611.txt:3`). A cópia ainda documenta RESIDUAL pós-F1 (`app/docs/RELATO.md:131`). Coerência com **todo o resto da matriz F2**, cujo conteúdo não foi fornecido nem está disponível nesta cópia: **[nao verificado]**.

- **O selo estrito dos três recortes não colide, por si, com diferenças “admitidas” nos pares ilegais:** diferenças permitidas não são obrigatórias; contudo esse selo não cobre os demais leitores F2 (anexo `respostas_registradas.md:5`; `app/chamados/models.py:3665`; `app/chamados/models.py:3688`; `app/chamados/models.py:3496`).

- **“Descarimbou = volta à lista” não é universal.** Precisa continuar sem validação, com instante ou texto de resposta, e sem tranca — salvo `ignorar_tranca=True` (`app/chamados/models.py:1923`; `app/chamados/models.py:1924`; `app/chamados/models.py:1926`). Ressuscitar efetivamente descarimba **e zera instante/texto**, portanto não volta a essa lista (`app/chamados/models.py:1629`; `app/chamados/models.py:1630`). Descarimbo pode ser recusado para pergunta fora do fato (`app/chamados/models.py:3316`; `app/chamados/models.py:3392`).

- **O caso elegível que morde é executável com as portas existentes:** selecionar resposta sem validação, dia não trancado, materializada e dentro do fato; descarimbar preservando resposta; comparar exclusão antes/inclusão depois do descarimbo nas duas árvores (`app/chamados/models.py:3318`; `app/chamados/models.py:3322`; `app/chamados/models.py:1923`; `app/chamados/models.py:1926`). Isso é diferente de exigir que toda descarimbada entre na lista (`app/chamados/models.py:1629`).

- **Sem conflito necessário com C1 9c:** C2 resolve L1 pela conservação do campo e registra apenas o predicado histórico novo em E4d, sem deslocar para lá a solução atual; a porta continua apagando o campo e a projeção parte do valor presente (`app/chamados/models.py:3318`; `app/chamados/juizes.py:1390`; anexo `complemento1_respostas_f2_estados_1611.txt:10`; anexo `complemento2_respostas_f2_estados_1611.txt:3`).

## 16/11 20:53 · sessao d9a566933b26 · - · - · origem=Ronald (tmux)

Censo direto da projeção na cópia:

| Chamador produtivo | Instante passado | Depende de materialização? |
|---|---|---|
| `_slot_esperado` — `app/chamados/juizes.py:351` | `agora`, eventualmente obtido da carga em `:348-349` | Não nesse consumo: só testa `respondida`, em `:352`. |
| `_pergunta_viva_calc` — `app/chamados/juizes.py:1404` | `agora` | Sim: materialização participa em `:1416`, `:1444`, `:1450`, `:1482`. |
| `beco_em_lote` — `app/escala/services/cadastro_realidade.py:502` | `agora` recebido do chamador (`:414`, `:449`) | Não nesse consumo: lê apenas `[1]`, instante de resposta. |

Chamadas diretas em testes, mediante aliases da função real: `app/chamados/tests/test_e4b_classe_e_memoria.py:383`, `:389`; `app/chamados/tests/test_e3_c8c_sem_banco.py:134`, `:137`, `:138`. Exercitam corte de carimbos futuros; não exercitam reconstrução de instante apagado por descarimbo (`test_e4b_classe_e_memoria.py:378-391`; `test_e3_c8c_sem_banco.py:126-139`).

Os três pontos citados pelo complemento não fazem consulta histórica explícita: `sem_veredito_entre` não recebe `agora` (`app/chamados/models.py:1910`), lê campos atuais (`:1923-1926`); `estado` chama `pergunta_viva(self)` sem instante (`:3154`); `supra_juiz` chama `sem_veredito_entre` sem instante (`app/ponto/supra_juiz.py:573-574`).

Há consumidores indiretos de C6 com instante: `app/chamados/services/pergunta_no_app.py:61`; `app/chamados/services/tarefa_do_dia.py:537`, `:541`; `app/chamados/juizes.py:1649`; `app/chamados/services/cobranca.py:241`. A carga também passa seu instante ao auxiliar de slot (`app/chamados/services/carga_pergunta_viva.py:119`; `app/chamados/juizes.py:348-351`).

**Número estrito encontrado: 0** chamadores explicitamente demonstrados pelo código que consultem instante anterior a um descarimbo e dependam do instante apagado. **Não significa “zero consultas temporais”: existem os três sítios acima.** Ocorrência efetiva em produção é [nao verificado]. A possibilidade de chamar C6 com instante passado existe (`app/chamados/juizes.py:879-881`, `:887-898`); a projeção continua partindo do carimbo atual e não o reconstrói após apagado (`:1389-1400`).

## 16/11 20:30 · sessao ad33c6a2dd5d · UI-VINCULO-MOSTRA-O-CALENDARIO · verificador · origem=Code

1. **O template atende ao comportamento pedido.** O título do calendário escolhido fica acima dos dias; a opção vazia com padrão reutiliza os mesmos `meses/dias` de `folgas_projecao`, com “Folgas do calendario padrao”; sem padrão, não cria esse bloco. A proposta em três linhas também está registrada. (`app/templates/colaboradores/partials/painel_vinculo.html:204`; `app/templates/colaboradores/partials/painel_vinculo.html:211`; `app/docs/RELATO.md:28`)

   A alternância fecha pela leitura do código:

   - **Abertura com FK vazia:** blocos numéricos ocultos; bloco `""` visível somente havendo padrão na projeção. (`app/templates/colaboradores/partials/painel_vinculo.html:205`; `app/templates/colaboradores/partials/painel_vinculo.html:211`; `app/templates/colaboradores/partials/painel_vinculo.html:212`)
   - **Abertura com FK preenchida:** aparece o bloco daquele PK; o bloco `""` fica oculto. (`app/templates/colaboradores/partials/painel_vinculo.html:200`; `app/templates/colaboradores/partials/painel_vinculo.html:205`; `app/templates/colaboradores/partials/painel_vinculo.html:212`)
   - **Troca:** o `onchange` mostra somente os elementos cujo atributo coincide exatamente com o valor selecionado. (`app/templates/colaboradores/partials/painel_vinculo.html:196`)
   - **Padrão também oferecido na lista, inclusive selecionado explicitamente:** os blocos têm identificadores distintos, `""` e PK; mostram os mesmos dias com o título correspondente à opção. Não aparecem simultaneamente pela lógica existente. (`app/templates/colaboradores/partials/painel_vinculo.html:196`; `app/templates/colaboradores/partials/painel_vinculo.html:205`; `app/templates/colaboradores/partials/painel_vinculo.html:212`)

2. **Escritores, leitores e semântica:**

   | Campo/valor | Quem escreve | Quem lê |
   |---|---|---|
   | `folgas_projecao` | `painel_vinculo`, consultando `datas_que_o_calendario_manda` por calendário/mês. (`app/colaboradores/views.py:304`) | Os dois laços de apresentação. (`app/templates/colaboradores/partials/painel_vinculo.html:204`; `app/templates/colaboradores/partials/painel_vinculo.html:211`) |
   | `tipo_escala.folga_calendario_padrao(_id)` | Cadastro do tipo atribui ID ou `None`. (`app/escala/services/cadastro_tipo.py:192`) | Herança do vínculo, resolução do POST vazio e novo bloco. (`app/colaboradores/services/vinculo.py:157`; `app/colaboradores/views.py:236`; `app/templates/colaboradores/partials/painel_vinculo.html:211`) |
   | `folga_calendario_id` | Na tela, o select fornece o valor; a view resolve vazio; a porta persiste a FK. Há também herança na criação/reutilização e cópia em ajustes. (`app/templates/colaboradores/partials/painel_vinculo.html:195`; `app/colaboradores/views.py:230`; `app/colaboradores/services/vinculo.py:461`; `app/colaboradores/services/vinculo.py:161`; `app/colaboradores/services/vinculo.py:1092`; `app/colaboradores/services/vinculo.py:1113`) | Seleção/visibilidade do painel, foto, divergência e vigia. (`app/templates/colaboradores/partials/painel_vinculo.html:200`; `app/escala/folgas.py:158`; `app/escala/folgas.py:99`; `app/escala/folgas.py:127`) |
   | `pode_abrir_calendario` | A view pergunta a `tem_acao(..., 'editar_escalas')`. (`app/colaboradores/views.py:309`) | Ambos os links; o destino exige a mesma ação. (`app/templates/colaboradores/partials/painel_vinculo.html:208`; `app/templates/colaboradores/partials/painel_vinculo.html:215`; `app/escala/views.py:315`) |
   | `data-folgas-cal` | O template escreve PK ou `""`. (`app/templates/colaboradores/partials/painel_vinculo.html:205`; `app/templates/colaboradores/partials/painel_vinculo.html:212`) | O `onchange` e as sondas dos testes. (`app/templates/colaboradores/partials/painel_vinculo.html:196`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:37`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:278`) |

   **ESMERIL-SEMÂNTICA preexistente:** a docstring de `trocar_calendario_de_folga` diz “None = padrao”, mas o corpo grava literalmente o argumento. Quem resolve vazio para padrão é a view; foto e fonte não fazem essa substituição. O diff não introduz essa divergência documental. (`app/colaboradores/services/vinculo.py:451`; `app/colaboradores/services/vinculo.py:461`; `app/colaboradores/views.py:236`; `app/escala/folgas.py:52`; `app/escala/folgas.py:157`)

   **O padrão exibido é o mesmo calendário que salvar vazio grava:** ambos usam o padrão do tipo da escala ativa. Isso apresenta a projeção do calendário, sem afirmar que FK nula significa ausência de folgas efetivas — a previsão também considera foto, ciclo e âncora. (`app/templates/colaboradores/partials/painel_vinculo.html:211`; `app/colaboradores/views.py:236`; `app/colaboradores/views.py:239`; `app/escala/models.py:1280`; `app/escala/models.py:1289`)

   **Se o padrão faltar em `folgas_projecao`, o bloco vazio não será produzido**, embora o POST continue resolvendo para o padrão. Na view atual, porém, a projeção percorre todos os `FolgaCalendario`, sem filtro de oferta que exclua algum padrão existente; ocorrência dessa ausência em produção: **[não verificado]**. (`app/templates/colaboradores/partials/painel_vinculo.html:211`; `app/colaboradores/views.py:236`; `app/colaboradores/views.py:301`; `app/colaboradores/views.py:304`)

3. **Classes de reprovação:**

   - **Derivação paralela/segundo juiz:** não aparecem no trecho novo; ele apenas seleciona o calendário e formata os dias já projetados pela fonte. (`app/templates/colaboradores/partials/painel_vinculo.html:211`; `app/templates/colaboradores/partials/painel_vinculo.html:214`; `app/colaboradores/views.py:304`)
   - **RED:** os testes novos exigem título no escolhido e bloco/dias do padrão; essas assertions reprovam o template anterior mostrado no diff. O caso “sem calendário nenhum”, isoladamente, conserva um comportamento que o template anterior já atendia; não demonstra o RED da novidade. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:149`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:165`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:177`)
   - **Mutante `forloop.first`:** substituir a guarda do padrão por ele criaria bloco indevido sem padrão, contrariando `_padrao(html) == []`; também escolheria `UVC-MES` antes de `UVC-SD`, contrariando os dias esperados do padrão. Isso demonstra capacidade de mordida por análise estática; execução/morte desse mutante: **[não verificado]**. (`app/templates/colaboradores/partials/painel_vinculo.html:211`; `app/colaboradores/views.py:301`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:73`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:77`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:157`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:184`)
   - **Relógio e cascas:** ambas as classes que renderizam têm relógio congelado; o navegador cobre `base.html/vincular`, `base.html/drawer` e `base_app.html/drawer`, medindo estilo computado e geometria. Execução dos testes: **[não verificado]**. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:117`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:314`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:279`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:362`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:371`)
   - **Lacuna de cobertura:** o browser com padrão troca para outro calendário; não exercita explicitamente escolher o próprio padrão nem abrir com FK igual a ele. A lógica estática desses casos fecha, conforme item 1. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:86`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:89`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:376`)

4. **No diff fornecido, não há mudança de modelo, migração, `escala/folgas.py`, diálogo nativo, cálculo monetário ou escrita de vínculo/escala pela aplicação:** o acréscimo é apresentação e testes; a persistência continua na porta existente. Não há CSS novo como regra/arquivo: reutiliza `.font-semibold` e os estilos inline do bloco existente. A exclusividade dos dois arquivos no intervalo Git não pôde ser conferida nesta cópia: **[não verificado]**. (`app/templates/colaboradores/partials/painel_vinculo.html:205`; `app/templates/colaboradores/partials/painel_vinculo.html:211`; `app/static/css/hasner-ponto.css:514`; `app/colaboradores/services/vinculo.py:461`)

VEREDITO: APROVA -- sem bloqueio funcional identificado no recorte; ressalvas de cobertura e docstring acima; execução RED, mutantes e suites [não verificado].

## 16/11 20:29 · sessao 1dacffdb9902 · - · - · origem=Ronald (tmux)

- **Padrão do tipo:** a FK admite `NULL` e sua semântica declarada é cópia/exibição; a engine não lê diretamente esse campo. O wizard lê a escolha e a envia a `salvar_tipo`, que valida o calendário, atribui o ID ou `None` e persiste o tipo. (`app/escala/models.py:87-93`; `app/templates/escala/_wizard_cal_select.html:1-6`; `app/escala/views_wizard.py:42-43`; `app/escala/views_wizard.py:108`; `app/escala/services/cadastro_tipo.py:192-203`; `app/escala/services/cadastro_tipo.py:307`)

- **Escritores da FK do vínculo:** `_copiar_folga_padrao` copia o padrão apenas quando existe padrão e a FK está vazia; é chamado na criação/reutilização do vínculo. `trocar_calendario_de_folga` grava o argumento recebido e sincroniza fotos futuras quando muda. Aplicação em lote chama essa mesma porta; ajuste automático e seu desfazimento copiam a FK antiga para os vínculos novos. (`app/colaboradores/services/vinculo.py:151-166`; `app/colaboradores/services/vinculo.py:284-298`; `app/colaboradores/services/vinculo.py:459-465`; `app/escala/views.py:404-412`; `app/colaboradores/services/vinculo.py:1084-1092`; `app/colaboradores/services/vinculo.py:1110-1113`)

- **Salvar vazio:** a view resolve vazio para `ec.tipo_escala.folga_calendario_padrao`, ou `None` se não houver tipo, e passa o resultado à porta. Logo, o padrão comparado no novo template é exatamente a FK usada para resolver o POST vazio, desde que esse calendário esteja em `folgas_projecao`. (`app/colaboradores/views.py:230-239`; `app/templates/colaboradores/partials/painel_vinculo.html:211-215`)

- **ESMERIL-SEMÂNTICA existente:** a docstring da porta afirma `None = padrao do template`, mas o corpo salva `None` literalmente; a resolução do padrão pertence à view. A materialização lê a FK do vínculo, e `datas_que_o_calendario_manda(None, ...)` devolve conjunto vazio, sem buscar o padrão. (`app/colaboradores/services/vinculo.py:450-462`; `app/colaboradores/views.py:235-239`; `app/escala/folgas.py:52-56`; `app/escala/folgas.py:157-158`)

- **Leitores relevantes:** painel lê FK para seleção/visibilidade; trilha registra o ID; divergência foto/calendário, vigia de calendário vazio, materialização e sincronização futura usam a FK do vínculo. O universo da lavra exclui FK nula; o endpoint de pendências também lista vínculos 6x1 com FK nula como `sem_plano`. Nenhum desses caminhos resolve `NULL` pelo padrão do tipo. (`app/templates/colaboradores/partials/painel_vinculo.html:200-215`; `app/colaboradores/services/vinculo.py:28-42`; `app/escala/folgas.py:74-79`; `app/escala/folgas.py:99`; `app/escala/folgas.py:126-129`; `app/escala/folgas.py:157-158`; `app/escala/folgas.py:627-629`; `app/api/views_mensageria.py:1221-1231`)

- **FK nula não significa ausência de folgas efetivas:** previsão consulta `FolgaDia`, ciclo e âncora; com foto, consulta suas datas e combina com a resposta do ciclo, e sem foto pode usar a âncora. O título novo limita corretamente a apresentação às datas mandadas pelo calendário, sem prometer folga efetiva da pessoa. (`app/escala/models.py:1280-1295`; `app/escala/models.py:1306-1321`; `app/escala/models.py:1365-1371`; `app/templates/colaboradores/partials/painel_vinculo.html:206-214`)

- **Padrão ausente da projeção:** o template não cria bloco vazio se nenhum `fc.pk` coincidir; nesse contexto não exibirá o padrão embora salvar vazio ainda resolva a FK padrão. Na view atual a lista vem de todos os `FolgaCalendario`, sem filtro de oferta/ativo, portanto um padrão existente no mesmo universo consultado entra na projeção; ocorrência real de ausência ou concorrência entre consultas é **[não verificado]**. (`app/templates/colaboradores/partials/painel_vinculo.html:211-217`; `app/colaboradores/views.py:236`; `app/colaboradores/views.py:301-305`)

## 16/11 20:29 · sessao cee5c7a7c695 · - · - · origem=Ronald (tmux)

- Os três casos estão escritos: escolhido exige título antes dos dias; vazio com padrão exige título, dias pela regra e pela fonte, link correto; vazio sem padrão exige nenhum bloco numérico visível e nenhum bloco/título do padrão. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:149`, `:161`, `:170`)
- O caso “sem calendário nenhum” continua passando no template anterior apresentado no anexo: o anterior já ocultava todos os blocos com FK nula, e não tinha bloco nem título do padrão. Portanto esse caso isolado comprova conservação do comportamento, não o RED da novidade. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:149`; template anterior do diff anexo: [não verificado por execução])
- O RED da novidade tem assertions capazes de reprovar o template anterior: título escolhido ausente e bloco padrão ausente. Execução RED real: [não verificado]. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:165`, `:177`)
- Não há mutante `forloop.first` no arquivo de testes desta obra; o mutante implementado retira o `onchange` do HTML já renderizado. Sua execução real e uma morte do mutante `forloop.first`: [não verificado]. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:346`, `:380`)
- Se o gate do bloco padrão fosse substituído por `forloop.first`, o caso sem padrão passaria a gerar bloco vazio visível e seria reprovado pela assertion `_padrao(html) == []`; se for usado para selecionar o primeiro calendário como padrão, a fixture ordena `UVC-MES` antes de `UVC-SD`, mas exige os dias do segundo domingo, portanto detectaria a fonte errada. Isso é análise estática, não prova de execução. (`app/templates/colaboradores/partials/painel_vinculo.html:211`; `app/colaboradores/views.py:301`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:73`, `:77`, `:157`, `:184`)
- O mutante de fonte usa sentinelas diferentes por calendário/mês e exige uma chamada por calendário/mês, mas renderiza `colab_mes`, cujo tipo não tem padrão; ele não interroga diretamente o novo bloco padrão com a sentinela. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:67`, `:205`)
- As duas classes que renderizam estão congeladas em 10/12/2026; a classe restante varre arquivos. Não identifiquei relógio solto no recorte de testes. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:117`, `:314`, `:392`)
- O navegador usa páginas renderizadas com a casca confirmada, os carregadores reais e estilo computado/geometria para medir visibilidade; cobre `base.html/vincular`, `base.html/drawer`, `base_app.html/drawer` tanto sem padrão quanto vazio com padrão. Execução browser: [não verificado]. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:269`, `:328`, `:362`, `:371`)
- Os carregadores reais injetam o partial por `innerHTML` e processam HTMX; o drawer está incluído nas duas cascas. (`app/templates/colaboradores/vincular.html:143`; `app/templates/core/_drawer_generico.html:19`; `app/templates/base.html:399`; `app/templates/base_app.html:154`)
- Lacuna de cobertura: o navegador do caso padrão troca para `cal_mes`, enquanto o padrão é `cal_sd`; não exercita seleção explícita do próprio padrão, nem abertura com FK já igual ao padrão. A fixture de FK preenchida com padrão no tipo usa outro calendário e só tem teste de markup. Isso não prova bug: o `onchange` compara exatamente `""` ou o PK, e o template distingue esses atributos, portanto a lógica estática separa corretamente os dois blocos. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:86`, `:89`, `:188`, `:376`; `app/templates/colaboradores/partials/painel_vinculo.html:196`, `:205`, `:212`)
- A proposta anterior ao código está registrada em três linhas, incluindo o comportamento sem padrão. (`app/docs/RELATO.md:28`)

## 16/11 20:17 · sessao 7afc540c893c · AUSENCIA-EFEITO-PELO-ESTADO · verificador · origem=Code

**AVAL-DEVOLVIDO no item 2:** preservar a base corrige a perda de parâmetros, mas não garante “número do chip == tamanho da lista” para qualquer requisição: os links mantêm o filtro da outra dimensão, enquanto os contadores ignoram status e efeito. (`app/ponto/views.py:2201-2226`; `app/templates/ponto/ausencias.html:48-67`.)

Identidade da cópia com o hash informado: **[não verificado]**.

**1. Premissas e parâmetros de `_base`**

A composição real é esta; a referência a `2186-2192` não abrange os filtros posteriores de colaborador e tipo. (`app/ponto/views.py:2186-2201`.)

| Parâmetro | Participação em `_base` | Links dos chips hoje |
|---|---|---|
| `mes`, `ano` | Sanitizados; fornecem as bordas padrão do período. | Levam ambos. |
| `de`, `ate` | Cada data válida substitui sua respectiva borda padrão. | Omitem ambos. |
| `registro` | Quando reconhecido como número, substitui a consulta de vigência por consulta ao `pk`. | Omitem. |
| `colaborador`, `tipo` | Filtram a consulta resultante, inclusive depois da substituição por `pk`. | Levam quando preenchidos. |

Fontes da tabela: `app/ponto/views.py:2171-2196`; `app/templates/ponto/ausencias.html:48-67`.

**Não há outro parâmetro GET que altere `_base` nesse corpo de função.** `status` e `efeito` filtram depois dos contadores; os chips de status preservam efeito e os de efeito preservam status. (`app/ponto/views.py:2171-2226`; `app/templates/ponto/ausencias.html:48-67`.)

`voltar` também se perde no clique, mas serve ao retorno de navegação, sem participar de `_base`. (`app/templates/ponto/ausencias.html:31`; `app/templates/ponto/ausencias.html:48-67`; `app/ponto/views.py:2171-2201`.)

**2. Compatibilidade e quem mais usa os links**

O item 1 do C2 é compatível com preservar o universo e corrige uma perda relevante para L1. Entretanto, o item 2 universaliza a igualdade justamente sobre o cruzamento efeito × status excluído pelo C1 fornecido: os links preservam esse cruzamento, e os contadores continuam anteriores a ele. (`app/docs/LEIS-UI.md:12-15`; `app/ponto/views.py:2201-2226`; `app/templates/ponto/ausencias.html:48-67`.)

Para manter o restante do C1, a promessa precisa conservar sua condição: **mesma base, somente o chip considerado ligado** — efeito sem status; rejeitada sem efeito. Garantir a igualdade para qualquer combinação exigiria mudar também a navegação ou os contadores, ultrapassando a mera inclusão de `de/ate/registro`. (`app/ponto/views.py:2201-2226`; `app/templates/ponto/ausencias.html:48-67`.)

Consumidores e produtores relevantes:

- Calendário gera `registro` e acrescenta `voltar`; chamados redirecionam por `registro`; o serviço de ausência produz esse endereço para notificação. (`app/templates/colaboradores/partials/_calendario_grade.html:97`; `app/chamados/views.py:1512-1519`; `app/ponto/services/ausencia.py:826-828`.)
- O JavaScript da lista lê `registro` para abrir automaticamente o painel. (`app/templates/ponto/ausencias.html:200-205`.)
- Cadastro de realidade monta `colaborador/tipo`; a vigia monta URLs com status ou tipo. (`app/escala/services/cadastro_realidade.py:18`; `app/ponto/services/vigia_ausencia.py:46-47`; `app/ponto/services/vigia_ausencia.py:166-195`.)
- Na própria tela, setas mensais e “Limpar” reconstruem URLs; o formulário GET envia período, registro, colaborador, tipo e eventualmente status, sem campos para efeito, mês ou ano. (`app/templates/ponto/ausencias.html:38-40`; `app/templates/ponto/ausencias.html:71-93`.)
- A paginação trabalha no DOM, com 50 linhas por padrão; a busca local oculta linhas. A view entrega `list(qs)` integralmente. Portanto, a igualdade deve contar registros da resposta inteira. (`app/static/js/hasner-table.js:282-325`; `app/static/js/hasner-filter.js:43-51`; `app/ponto/views.py:2248-2255`.)
- Não há ramo de exportação na view nem controle de exportação no template da lista. (`app/ponto/views.py:2165-2270`; `app/templates/ponto/ausencias.html:31-197`.)

Os testes existentes conferem período, registro e status pelos registros do contexto; outro exige `registro` no redirecionamento do chamado. Não demonstram a igualdade após clicar nos chips. (`app/ponto/tests/test_lista_ausencias.py:19-31`; `app/ponto/tests/test_chamado_ausencia_botao.py:40-45`.) Teste que fixe literalmente o `href` dos chips: **[não verificado; não localizado na busca]**.

**3. Condições literais**

**Registro preservado:** sem o filtro da outra dimensão, conservar um `registro` válido mantém a mesma base de zero ou um registro; aplicar o predicado correspondente ao contador permite a igualdade. O registro substitui somente a vigência: colaborador e tipo continuam aplicáveis, e status/efeito continuam podendo excluir a linha. (`app/ponto/views.py:2190-2226`.)

**Contraexemplo estático ao item 2:** suponha um registro aprovado cujo veredito seja `suprimido`, consultado com `registro=<pk>&status=rejeitada`. `_base` contém esse registro e o chip Suprime conta 1; clicar Suprime mantendo registro **também mantém `status=rejeitada`**, produzindo lista vazia. Isso permanece verdadeiro com a classificação pelo juiz prevista no aval. É um cenário derivado do código, sem ocorrência de produção demonstrada. (`app/ponto/views.py:2190-2226`; `app/templates/ponto/ausencias.html:67`; `app/ponto/services/dia_decidido.py:461-473`.)

**Datas e mês/ano:** a view sanitiza mês/ano e executa `_d` separadamente para cada borda. Data válida prevalece sobre a borda mensal; ausente, vazia ou inválida resulta em `None` e usa a borda correspondente do mês. Uma borda válida pode coexistir com outra mensal; não existe validação ou reordenação de `de > ate` nesse trecho. (`app/core/sanitiza.py:11-35`; `app/ponto/views.py:2171-2189`.)

Assim, **`?de=&ate=` literalmente vazio não recorta o período**: usa o mês inteiro. Para o RED demonstrar perda de recorte, precisa fornecer datas válidas que efetivamente alterem a janela; usar `registro` válido nesse RED retiraria a dependência do período. (`app/ponto/views.py:2181-2192`.)

**Trecho compartilhado:** ele ainda não existe na cópia; os links continuam montados individualmente e o contexto entrega `de/ate`, sem o fragmento proposto. Portanto, sua implementação repetir ou compartilhar a leitura é **[não verificado]**. Para cumprir o desenho, deve consumir os valores já lidos, respeitando a mesma normalização, sem uma segunda interpretação das datas. (`app/ponto/views.py:2254-2270`; `app/templates/ponto/ausencias.html:48-67`.)

**4. Exportada, dinheiro ou `!`**

No escopo declarado de transportar parâmetros nos links, não aparece escrita em competência exportada nem cálculo financeiro: a função consulta, prepara a lista e renderiza. Há inclusive selo existente que exige que ela permaneça sem leitura de fechamento. (`app/ponto/views.py:2165-2270`; `app/ponto/tests/test_k8_tela_abre_na_competencia.py:272-295`.)

As ações POST da tela são outro caminho; transições de ausência podem salvar dados e acordar a projeção da folha. Alterá-las extrapolaria este complemento. Nenhum `!` adicional está demonstrado para a mudança restrita dos links. (`app/templates/ponto/ausencias.html:278-282`; `app/templates/ponto/ausencias.html:308-312`; `app/ponto/services/ausencia.py:987-994`; `CLAUDE.md:265-268`.)

Implementação, RED executado, M5 morto, suíte, deploy, smoke e efeito concreto em produção: **[não verificado]**. [nao verificado]

VEREDITO: PROCEDE (item 2 — preservar de/ate/registro não garante igualdade para qualquer R, pois os links mantêm o filtro cruzado: app/templates/ponto/ausencias.html:48-67; app/ponto/views.py:2201-2226; RED literalmente vazio não recorta período: app/ponto/views.py:2181-2187).

## 16/11 20:16 · sessao 472e469899ff · - · - · origem=Ronald (tmux)

Censo de produtores/leitores da URL:

- O calendário gera `?registro=<id>` e acrescenta `voltar` com a URL de origem. A lista usa `voltar` somente para o retorno; seu JavaScript lê `registro` para abrir o painel automaticamente. (`app/templates/colaboradores/partials/_calendario_grade.html:97`; `app/templates/ponto/ausencias.html:31`; `app/templates/ponto/ausencias.html:200-205`.)
- Chamados redirecionam para `?registro=<id>`; o serviço de ausência também produz esse endereço. Há teste que exige `registro` no redirecionamento do chamado, sem fixar href de chip. (`app/chamados/views.py:1512-1519`; `app/ponto/services/ausencia.py:828`; `app/ponto/tests/test_chamado_ausencia_botao.py:40-45`.)
- O cadastro de realidade produz `?colaborador=<id>&tipo=afastamento_inss`. A vigia produz URLs com `status` ou `tipo`. (`app/escala/services/cadastro_realidade.py:18`; `app/ponto/services/vigia_ausencia.py:46-47`; `app/ponto/services/vigia_ausencia.py:166`; `app/ponto/services/vigia_ausencia.py:179-195`.)
- O menu, os retornos de lançamento e o histórico de atestados apontam para a lista sem query string. (`app/templates/base.html:246`; `app/templates/ponto/lancar_ausencia.html:4`; `app/templates/ponto/lancar_ausencia.html:89`; `app/templates/ponto/historico_atestados.html:52`.)
- A própria tela reconstrói URLs nos chips, nas setas mensais e em “Limpar”. O formulário GET envia `de/ate/registro/colaborador/tipo` e eventualmente `status`; não contém campo de `efeito`, `mes`, `ano` ou `voltar`. Isso é comportamento anterior ao complemento. (`app/templates/ponto/ausencias.html:38-40`; `app/templates/ponto/ausencias.html:48-67`; `app/templates/ponto/ausencias.html:71-93`.)

Paginação, busca e export:

- A view materializa integralmente `list(qs)`; o template percorre todos os registros. A paginação acontece no DOM, com 50 linhas por página por padrão, sem URL de página. A busca também oculta linhas no navegador. Assim, cardinalidade precisa significar registros da resposta integral. (`app/ponto/views.py:2248`; `app/templates/ponto/ausencias.html:142-185`; `app/static/js/hasner-table.js:282-325`; `app/static/js/hasner-filter.js:43-51`.)
- Não há controle de exportação no template da lista nem ramo de exportação no corpo da view; consumidor externo desses hrefs em produção: [não verificado]. (`app/templates/ponto/ausencias.html:31-197`; `app/ponto/views.py:2165-2270`.)

Testes: [nao verificado]

- Os testes da lista conferem vigência por `de/ate`, busca por `registro` e filtro por status mediante pks no contexto; não clicam chips nem verificam igualdade de seus contadores com a resposta de destino. (`app/ponto/tests/test_lista_ausencias.py:19-31`.)
- Há teste de GET inválido para `mes=13&ano=lixo`; ele exige apenas resposta inferior a 500. (`app/ponto/tests/test_get_sanitizado_ponto.py:31-34`; `app/ponto/tests/test_get_sanitizado_ponto.py:44-46`.)
- Não encontrei teste que fixe literalmente o href dos chips na busca por `lista_ausencias`, `ausencias.html`, `chip-efeito` e URLs de status/efeito; exaustividade semântica dessa busca: [não verificado]. [nao verificado]

Risco literal principal:

- Adicionar os três parâmetros não resolve a promessa “qualquer chip a partir de qualquer R”: chips de status preservam `efeito`, chips de efeito preservam `status`, enquanto todos os contadores vêm de `_base` antes de ambos. Esse cruzamento continua anterior/excluído pelo C1 e conflita com a generalização literal do C2. (`app/templates/ponto/ausencias.html:48-67`; `app/ponto/views.py:2201-2226`; anexo COMPLEMENTO-1: [não verificado — sem numeração de arquivo].)

## 16/11 19:37 · sessao 7901657f82ee · BUG-PAR-O-PAR · sonda · origem=Code

1. **Saída fornecida:** universo 163/163; 60 vivas sem `validada_em`; nenhuma sem `data_evento`; **1** com competência do fato ENTREGUE; desta, **1** com FechamentoMensal `aberto`; `total_erros=0`. Valores transcritos, sem validação independente. [nao verificado]

2. **Equivalência do script: [nao verificado].** Não localizei `sonda_c4_u163.py` na cópia; o conteúdo não acompanha o dado fornecido. Portanto, não posso confirmar suas chamadas, filtros ou tratamento de erros. [nao verificado] As autoridades conferidas são:

   - Vida: `pergunta_viva`, em `app/chamados/juizes.py:866`; a implementação rejeita pergunta com `validada_em` preenchido em `app/chamados/juizes.py:1405`.
   - Entrega: `competencia_do_dia_entregue` consulta a janela da empresa e `competencia_entregue`, usando mês/ano do fim da janela. `app/chamados/services/vigia_fio.py:799–807`.
   - Fechamento: os estados são `aberto`, `aprovado`, `exportado`, com padrão `aberto`. `app/ponto/models.py:399–410`.

3. **Sem linha passa pela F6: sim.** A F6 só bloqueia quando existe fechamento `aprovado` ou `exportado`; sem linha retorna `''`, permitindo passar essa guarda. Isso equivale operacionalmente a aberto, sem atribuir status a uma linha inexistente. `app/ponto/janelas.py:75–89`. A porta chama essa autoridade em `app/ponto/registro_batida.py:132–137`; o registro da F6 está em `app/core/juizes.py:315`.

Os números finais reproduzem exclusivamente a saída fornecida. [nao verificado]

NUMERO: validaveis_entregue=1 | aberto=1 | total_erros=0.

## 16/11 19:36 · sessao ecdcbcf268f1 · BUG-PAR-O-PAR · sonda · origem=Code

1. A saída fornecida informa **0 vivas com `data_evento` vazio**, **0 com o campo vazio** e universo de **19.630 perguntas de intervalo**. Validação independente em produção: [nao verificado].

2. **Não é possível certificar que o script mede o solicitado**: o texto contém somente a saída; não localizei `sonda_3e_sem_dia.py` nem `_PAR_DO_INTERVALO` da c4 na cópia. [nao verificado] Falta conferir no script:
   - Universo filtrado exatamente pelas chaves de `_PAR_DO_INTERVALO` da c4. [nao verificado]
   - Seleção de `data_evento` nulo, compatível com o campo declarado em `app/chamados/models.py:2528`.
   - Contagem das selecionadas por `chamados.juizes.pergunta_viva(p)`, cuja entrada está em `app/chamados/juizes.py:866` e delegação em `app/chamados/juizes.py:898`.

3. A saída informa **`total_erros=0`**; a abrangência desse contador depende do script ausente. [nao verificado]

NUMERO: 0 de 0 de 19630 | total_erros=0

## 16/11 19:34 · sessao 79ef3a77c18a · AUSENCIA-EFEITO-PELO-ESTADO · verificador · origem=Code

**AVAL-DEVOLVIDO em b/c, quanto à afirmação de que a navegação fecha L1:** os links descartam parâmetros que definem `_base`. Os resultados 5b/5c podem fechar na mesma requisição, mas isso não garante igualdade entre o número antes do clique e a lista de destino. (`app/ponto/views.py:2186-2202`; `app/templates/ponto/ausencias.html:48-67`; `app/docs/LEIS-UI.md:12-15`.)

Identidade da cópia com o HEAD/hash informado: **[não verificado]**.

1. **MAPA — premissas conferem; desenho compatível com juiz único.**

   `CORES` existe em `app/ponto/services/dia_decidido.py:45-58`; `PALAVRA` existe em `app/ponto/services/dia_decidido.py:67-71`. Entretanto, `PALAVRA` contém somente `EM_ABERTO`, `FOLGA` e `FERIADO`, não a apresentação dos quatro efeitos de ausência. (`app/ponto/services/dia_decidido.py:67-71`.)

   `PALAVRA` é lida por `palavra_do_dia`; `CORES`, por `do_dia`, `cartao_do_dia` e `linha_do_tempo`. Calendário e espelho consomem os resultados dessas funções; PDF e API recebem palavra/cor/veredito. (`app/ponto/services/dia_decidido.py:211-213`; `app/ponto/services/dia_decidido.py:425-436`; `app/ponto/services/dia_decidido.py:518`; `app/ponto/services/dia_decidido.py:613`; `app/colaboradores/services/calendario.py:747-756`; `app/ponto/services/espelho.py:340-345`; `app/relatorios/pdf_espelho.py:923-924`; `app/api/views.py:1541-1543`.)

   Há também leitores diretos de `CORES` nos testes de calendário decidido, linha do tempo, palavra do dia e API. (`app/ponto/tests/test_rotulo_do_dia_decidido.py:59`; `app/ponto/tests/test_linha_do_tempo.py:73`; `app/relatorios/tests/test_palavra_do_dia.py:133`; `app/api/tests/test_o21_app_diz_o_que_foi_decidido.py:112`.)

   Um dicionário indexado pelo veredito, contendo filtro/texto/ajuda, apenas apresenta a resposta existente: não acrescenta julgamento de tipo/status nem função de veredito fora do módulo. Isso atende à leitura da autoridade e à retirada da ajuda do template. (`app/ponto/services/dia_decidido.py:453-481`; `CLAUDE.md:20-22`; `app/docs/LEIS-UI.md:77-84`.)

   Na busca pelas frases indicadas, as ocorrências de apresentação estão na célula e nos chips; há ainda um comentário de cálculo, sem mapa de apresentação. Não apareceu outro dicionário dessas frases. Os chips continuam sendo duplicação de ajuda, expressamente excluída pelo complemento. (`app/templates/ponto/ausencias.html:65-67`; `app/templates/ponto/ausencias.html:158`; `app/ponto/calculador/alimentacao.py:63`.)

2. **a — confere, inclusive a separação do ANEXO!.**

   O texto literal atual é **`ABONA` / `Nao desconta`**, **`DESCONTA` / `Desconta na folha`**, **`SUPRIME` / `Suprime a jornada`**. O arquivo escreve **`Nao` sem acento**; citar `Não desconta` como texto atual seria inexato. (`app/templates/ponto/ausencias.html:158`.)

   O `?` aparece hoje no `else` dessa mesma célula, como `<span class="hx-muted">?</span>`. O selo `ANEXO!` vem depois, em condicional independente, embora dentro do mesmo `td`. (`app/templates/ponto/ausencias.html:158`.)

   Entregar `veredito_de(a)` no laço existente e buscar texto/title pelo veredito é compatível com o juiz atual e com L7. A classe do badge pode apresentar o veredito sem recalcular seu resultado; a coluna Status tem seus próprios ramos, separados. (`app/ponto/views.py:2248-2254`; `app/ponto/services/dia_decidido.py:476-481`; `app/docs/LEIS-UI.md:77-84`; `app/templates/ponto/ausencias.html:176-179`.)

3. **Resultado 6 — a nova igualdade fecha como contrato.**

   Os quatro status reais e os retornos esperados, derivados estaticamente do núcleo, são estes; não se trata de teste executado. (`app/ponto/models.py:701-704`; `app/ponto/services/dia_decidido.py:461-481`.)

   | Status | Tipo ABONA | Tipo DESCONTA | Tipo SUPRIME |
   |---|---|---|---|
   | `aguardando_documento` | `abonado` | `descontado` | `suprimido` |
   | `aguardando_decisao` | `abonado` | `descontado` | `suprimido` |
   | `aprovada` | `abonado` | `descontado` | `suprimido` |
   | `rejeitada` | `rejeitado` | `rejeitado` | `rejeitado` |

   Portanto, comparar `data-value` com o veredito e texto/title com sua apresentação elimina a antiga comparação entre vocabulários diferentes. A comparação deve alcançar o badge de efeito, preservando o `ANEXO!` independente. (`app/ponto/services/dia_decidido.py:26`; `app/ponto/services/dia_decidido.py:481`; `app/templates/ponto/ausencias.html:158`.)

4. **b/c — universo confirmado, mas a promessa sobre L1 ainda precisa de emenda.**

   O comentário **HX-CHIPS-EFEITO está em 2198-2199**, e `_base = qs` em **2201**; 2196 aplica tipo e 2197 conta pendentes. (`app/ponto/views.py:2195-2202`.)

   A base real incorpora **vigência por `de/ate`, ou busca por `registro`**, além de colaborador e tipo, antes de status/efeito. Assim, “mês + colaborador + tipo” é uma descrição incompleta dos parâmetros efetivos. (`app/ponto/views.py:2186-2201`.)

   O problema restante não exige cruzar efeito com status: os links dos chips omitem `de`, `ate` e `registro`. Uma busca por registro pode contar aquele `pk`, mas clicar um único chip refaz o universo sem essa restrição. A possibilidade decorre do código; ocorrência concreta em produção: **[não verificado]**. (`app/ponto/views.py:2190-2202`; `app/templates/ponto/ausencias.html:48-67`.)

   Logo, preservar os links não sustenta a promessa geral “número do chip == lista alcançada pelo clique”. É preciso enfrentar essa perda de parâmetros ou delimitar expressamente o contrato à mesma base/requisição, sem declarar a navegação inteiramente conforme L1. (`app/docs/LEIS-UI.md:12-15`; `app/templates/ponto/ausencias.html:48-67`.)

5. **Resultado 5 — fecha para a cardinalidade integral da mesma base.**

   **5a:** o núcleo particiona qualquer par tipo/status entre exatamente quatro retornos; rejeitada retorna antes de consultar efeito. Somar os três efeitos e rejeitadas fecha a partição proposta. (`app/ponto/services/dia_decidido.py:461-473`.)

   **5b:** sem status, contar e filtrar pelos mesmos pares/vereditos sobre `_base` pode produzir exatamente `cnt[X]` registros. Não há corte posterior no servidor: a view materializa `list(qs)` integralmente. A implementação futura dessa classificação: **[não verificado]**. (`app/ponto/views.py:2201-2226`; `app/ponto/views.py:2248-2255`.)

   **5c:** com somente `status=rejeitada`, sem efeito e conservando os demais parâmetros, contador e lista já usam o mesmo predicado `status='rejeitada'`. O agregado `pendente` não é necessário para essa condição. (`app/ponto/views.py:2207`; `app/ponto/views.py:2212-2226`.)

   Não há status default nem vigência aplicada exclusivamente à lista depois da contagem. A busca válida por registro substitui a consulta antes de `_base`; a ordenação não limita registros. (`app/ponto/views.py:2186-2202`; `app/ponto/views.py:2212-2219`; `app/ponto/turnos.py:3170-3178`.)

   **Há paginação no navegador:** a tabela recebe todos os registros, mas o componente mostra **50 por página** por padrão. Busca local também oculta linhas. Portanto, “tamanho da lista” deve significar registros da resposta integral, não linhas simultaneamente visíveis; no vazio, a linha “Nenhuma ausência encontrada” não é registro. (`app/templates/ponto/ausencias.html:127-142`; `app/templates/ponto/ausencias.html:183-185`; `app/static/js/hasner-table.js:284`; `app/static/js/hasner-table.js:293-301`; `app/templates/ponto/ausencias.html:94-97`; `app/static/js/hasner-filter.js:43-51`.)

6. **c — mapa pedido→veredito resolve a diferença de vocabulário.**

   A URL usa `abona/desconta/suprime`; o juiz retorna `abonado/descontado/suprimido`. Inverter o campo `filtro` do mesmo mapa transporta essa correspondência sem outro julgamento. Valor desconhecido atualmente não aplica filtro de efeito, comportamento compatível com o proposto. (`app/templates/ponto/ausencias.html:65-67`; `app/ponto/services/dia_decidido.py:26`; `app/ponto/views.py:2220-2226`.)

7. **Selo das chaves e M4 — satisfazíveis, ainda sem execução comprovada.**

   As quatro constantes de ausência são exatamente `ABONADO='abonado'`, `DESCONTADO='descontado'`, `SUPRIMIDO='suprimido'` e `REJEITADO='rejeitado'`. Não se deve comparar as chaves com todas as entradas de `CORES`: o módulo também contém vereditos de dia e `PENDENTE`, este usado pela pergunta sobre o andamento do lançamento. (`app/ponto/services/dia_decidido.py:26-58`; `app/ponto/services/dia_decidido.py:439-450`.)

   M4 exige provar a **origem** do texto no mapa: igualdade dos textos renderizados, sozinha, também aceitaria literais idênticos escritos no template. O selo por AST/texto precisa prender esse consumo na célula atualmente hardcoded. M4 executado e morto: **[não verificado]**. (`app/templates/ponto/ausencias.html:158`; `app/docs/LEIS-UI.md:77-80`.)

Quanto à pergunta 4, o desenho permanece em consulta/apresentação/seleção, sem escritor financeiro no corpo de `lista_ausencias`. A tela possui ações POST; ampliar a mudança para status alcançaria gravação e projeção do fechamento. (`app/ponto/views.py:2165-2270`; `app/templates/ponto/ausencias.html:278-282`; `app/templates/ponto/ausencias.html:308-312`; `app/ponto/services/ausencia.py:987-994`.)

Preservar `Ausencia.efeito` e `tipo_contradiz_documento` mantém os leitores existentes: calendário exclui rejeitada antes da leitura; fechamento/coletor recebem somente aprovadas. O `!` local cobre gravado exportado e regra fora do pedido; não há necessidade adicional demonstrada nesse desenho restrito. (`app/ponto/models.py:849-852`; `app/ponto/services/ausencia.py:754-759`; `app/colaboradores/services/calendario.py:516`; `app/ponto/turnos.py:2528-2531`; `app/ponto/turnos.py:2765`; `app/ponto/services/fechamento.py:683-697`; `app/ponto/services/efeito_ausencia.py:53-58`; `CLAUDE.md:265-268`.)

RED, implementação, mutantes mortos, suite, deploy e smoke em produção: **[não verificado]**. [nao verificado]

VEREDITO: AVAL-DEVOLVIDO em b/c universo — 5b/5c fecham na mesma requisição, mas os links descartam de/ate/registro que definem _base; a promessa geral de fechamento da L1 não fecha (`app/ponto/views.py:2186-2202`; `app/templates/ponto/ausencias.html:48-67`; `app/docs/LEIS-UI.md:12-15`).

## 16/11 19:32 · sessao ad6770ca52c5 · - · - · origem=Ronald (tmux)

- **Universo confirmado, linhas corrigidas:** o comentário HX-CHIPS-EFEITO está em `app/ponto/views.py:2198-2199`, e `_base = qs` em `app/ponto/views.py:2201`; `2196` ainda aplica filtro de tipo e `2197` conta pendentes. A base incorpora vigência ou busca por registro, colaborador e tipo antes de status/efeito. (`app/ponto/views.py:2186-2226`.)

- **Vigência/defaults:** mês/ano ausentes usam hoje; `de/ate` ausentes ou inválidos usam início/fim desse mês. `_vigentes` aplica apenas datas/cobertura, sem filtro de status. Não há filtro default de status nem vigência aplicada exclusivamente depois de contar. (`app/ponto/views.py:2169-2189`; `app/ponto/turnos.py:3154-3178`; `app/ponto/views.py:2212-2219`.)

- **Busca por registro:** `registro` numérico, admitindo `#`, substitui a consulta de vigência por `pk`; colaborador/tipo ainda se aplicam depois. A substituição antecede `_base`, valendo igualmente para contador e lista. Valor inválido não faz essa substituição. (`app/ponto/views.py:2190-2202`.)

- **Ordenação:** consulta regular usa `-data,colaborador__nome`; consulta por registro preserva a ordenação do modelo, de mesmos campos. Ordenação não recorta registros. (`app/ponto/views.py:2189-2192`; `app/ponto/models.py:826-829`.) O ordenamento no navegador rearranja linhas visíveis, sem eliminar registros do DOM. (`app/static/js/hasner-table.js:189-215`.)

- **5b como cardinalidade da resposta/contexto:** atualmente, sem status, cada contador de efeito e seu filtro consultam o mesmo conjunto de tipos sobre a mesma base; a proposta pode preservar a igualdade usando os mesmos pares/vereditos. Não existe paginação/limite no servidor: a view materializa `list(qs)` e passa essa lista integral ao template. A igualdade futura por veredito ainda não está implementada. (`app/ponto/views.py:2201-2226`; `app/ponto/views.py:2248-2255`.)

- **5c como cardinalidade da resposta/contexto:** GET somente com `status=rejeitada`, conservando os mesmos demais parâmetros da base e sem efeito, já conta e lista pelo mesmo predicado `status='rejeitada'`; nenhum corte posterior na view reduz a lista. `pendente` agrega os dois estados abertos, mas não é necessário para essa condição. (`app/ponto/views.py:2207`; `app/ponto/views.py:2212-2226`; `app/ponto/views.py:2248-2255`.)

- **Ressalva literal necessária sobre paginação:** embora o servidor entregue todos os registros e o template crie uma linha por ausência, existe paginação universal no navegador, default **50** por página; linhas fora da página recebem `display:none`. Assim, 5b/5c podem exigir tamanho do contexto ou total de linhas de registros no HTML, mas não “todas simultaneamente visíveis” quando N > 50. (`app/templates/ponto/ausencias.html:127-142`; `app/templates/ponto/ausencias.html:182-185`; `app/templates/base.html:84`; `app/static/js/hasner-table.js:284`; `app/static/js/hasner-table.js:293-301`; `app/static/js/hasner-table.js:357-362`.) A busca local também oculta linhas sem alterar o queryset/contadores. (`app/templates/ponto/ausencias.html:94-97`; `app/static/js/hasner-filter.js:43-51`.) Em universo vazio, existe ainda a linha “Nenhuma ausência encontrada”, que não pode ser contada como registro. (`app/templates/ponto/ausencias.html:183-185`.)

- **Links e L1 não ficam universalmente resolvidos:** links dos chips preservam o filtro cruzado de status/efeito, enquanto os números continuam anteriores aos dois filtros; isso é a divergência expressamente excluída pelo complemento, mas permanece frente à L1 literal. (`app/templates/ponto/ausencias.html:48-67`; `app/ponto/views.py:2201-2226`; `app/docs/LEIS-UI.md:12-15`.) Há também outro problema: todos esses links omitem `de`, `ate` e `registro`, embora os três alterem `_base` antes de contar. Desde busca por registro ou janela personalizada, clicar mesmo um único chip pode recriar outro universo; “links não mudam” não garante que o número de origem corresponda à lista de destino. (`app/templates/ponto/ausencias.html:48-67`; `app/templates/ponto/ausencias.html:72-74`; `app/ponto/views.py:2186-2202`.) Divergência concreta em dados reais: **[não verificado]**.

**Recomendação ao consolidar:** aceitar 5a/5b/5c como contratos propostos sobre o mesmo `_base` e cardinalidade integral da resposta; devolver a afirmação de que isso sozinho “fecha L1” para a navegação vigente, e pedir precisão de “tamanho” em presença da paginação de 50. (`app/docs/LEIS-UI.md:12-15`; `app/templates/ponto/ausencias.html:48-67`; `app/static/js/hasner-table.js:284-301`.)

## 16/11 19:32 · sessao cc701b003112 · - · - · origem=Ronald (tmux)

**MAPA:** `CORES` existe em `app/ponto/services/dia_decidido.py:45-58`; `PALAVRA` existe em `app/ponto/services/dia_decidido.py:67-71`, mas este contém apenas `EM_ABERTO`, `FOLGA` e `FERIADO`, não os quatro vereditos de ausência.

- `PALAVRA` é lida por `palavra_do_dia` (`app/ponto/services/dia_decidido.py:211-213`).
- `CORES` é lida por `do_dia`, `cartao_do_dia` e `linha_do_tempo` (`app/ponto/services/dia_decidido.py:425`; `app/ponto/services/dia_decidido.py:429`; `app/ponto/services/dia_decidido.py:436`; `app/ponto/services/dia_decidido.py:518`; `app/ponto/services/dia_decidido.py:613`).
- Os consumidores externos de produção leem funções/resultados: calendário (`app/colaboradores/services/calendario.py:737`; `app/colaboradores/services/calendario.py:747-756`), espelho (`app/ponto/services/espelho.py:328`; `app/ponto/services/espelho.py:340-345`), histórico (`app/ponto/views.py:2017-2018`) e lote (`app/ponto/services/atestados_lote.py:54-57`). PDF consome `palavra_dia`; API publica palavra/cor/veredito (`app/relatorios/pdf_espelho.py:923-924`; `app/api/views.py:1541-1543`).

**Texto literal:** são exatamente `ABONA` / `Nao desconta` — **sem acento em `Nao`** —, `DESCONTA` / `Desconta na folha`, `SUPRIME` / `Suprime a jornada`. Não há correção de acentuação a fazer na citação literal (`app/templates/ponto/ausencias.html:158`).

**“?” e ANEXO!:** o desconhecido aparece no `else` da própria célula Efeito, como `<span class="hx-muted">?</span>`; ANEXO! é outro `span`, depois do ramo completo do efeito, no mesmo `td` (`app/templates/ponto/ausencias.html:158`).

**Outro mapa:** a busca exata pelas quatro variantes/frases encontrou as ajudas dos chips, a célula e um comentário financeiro sem apresentação de ausência (`app/templates/ponto/ausencias.html:65-67`; `app/templates/ponto/ausencias.html:158`; `app/ponto/calculador/alimentacao.py:63`). Não apareceu outro dicionário com essas frases; a evidência positiva da duplicação atual é apenas a ajuda dos chips, explicitamente excluída do complemento (`app/templates/ponto/ausencias.html:65-67`).

**Juiz único:** o novo dicionário proposto apresenta uma resposta já decidida; não precisa julgar tipo/status. A autoridade atual retorna primeiro `REJEITADO` quando o status é rejeitada, depois consulta efeito, retornando `DESCONTADO`, `SUPRIMIDO` ou `ABONADO`; `veredito_de` somente delega (`app/ponto/services/dia_decidido.py:461-481`). Isso é compatível com “testemunha lê, não recalcula” e com ajuda fora do template (`CLAUDE.md:20-22`; `app/docs/LEIS-UI.md:77-80`).

**Selo das quatro chaves:** as constantes de ausência são exatamente `ABONADO='abonado'`, `DESCONTADO='descontado'`, `SUPRIMIDO='suprimido'`, `REJEITADO='rejeitado'` (`app/ponto/services/dia_decidido.py:26`). `PENDENTE` existe separadamente e pertence à pergunta de estado do lançamento; `veredito_do_lancamento` pode devolvê-lo, enquanto `veredito_de` retorna pelo núcleo dos quatro (`app/ponto/services/dia_decidido.py:37-41`; `app/ponto/services/dia_decidido.py:439-450`; `app/ponto/services/dia_decidido.py:461-481`).

**M4:** para matar texto hardcoded, o selo precisa comprovar que a célula recebe o texto do mapa; igualdade textual contra os valores fixos do mapa, sozinha, também passa com hardcode idêntico. O alvo atual é precisamente o texto escrito nos três ramos da célula (`app/templates/ponto/ausencias.html:158`). Mutante executado e morto: **[não verificado]**.

## 16/11 19:32 · sessao e665bb4a60cd · - · - · origem=Ronald (tmux)

**Escopo financeiro e `!`: não há exigência adicional demonstrada no desenho proposto, desde que a mudança permaneça em apresentação/seleção e preserve os escritores e os leitores atuais.** O corpo de `lista_ausencias` consulta, materializa e renderiza; não grava status nem fechamento. A tela, porém, oferece POSTs, de modo que não cabe qualificá-la inteira como somente leitura. (`app/ponto/views.py:2165-2270`; `app/ponto/views.py:2248`; `app/ponto/views.py:2254`; `app/templates/ponto/ausencias.html:249`; `app/templates/ponto/ausencias.html:278-282`; `app/templates/ponto/ausencias.html:308-312`.)

- **ANEXO! permanece independente:** o selo consulta `a.contradiz_documento` depois do badge de efeito; o modelo delega ao juiz documental, que exige documento e compara `efeito_vigente(tipo)` com DESCONTA, sem consultar status. Preservá-lo como prescrito mantém uma pergunta diferente daquela respondida pelo veredito visual. (`app/templates/ponto/ausencias.html:158`; `app/ponto/models.py:867-869`; `app/ponto/services/ausencia.py:745-759`.)
- **Calendário não admite rejeitada nessa alimentação:** chama cobertura com `para='cobranca'`; a cobertura aplica `_pelo_efeito`, que exclui rejeitada, antes do uso de `_a.efeito`. (`app/colaboradores/services/calendario.py:516`; `app/ponto/turnos.py:2765-2766`; `app/ponto/turnos.py:2530-2531`; `app/colaboradores/services/calendario.py:705`.)
- **Fechamento e coletor financeiro recebem somente aprovadas:** ambos solicitam cobertura `para='folha'`, cujo filtro é `status='aprovada'`; depois leem `.efeito`. A seleção da exportação citada também exige aprovada. (`app/ponto/services/fechamento.py:683-697`; `app/ponto/services/efeito_ausencia.py:53-58`; `app/ponto/turnos.py:2528-2529`; `app/folha/export.py:87-91`.)
- **A propriedade preservada continua sem status:** `Ausencia.efeito` chama apenas `efeito_vigente(self.tipo)`. O helper de dias inteiros lê essa propriedade sobre a cobertura financeira recebida; os chamadores financeiros já entregam a cobertura de folha. (`app/ponto/models.py:849-852`; `app/ponto/turnos.py:2692-2709`; `app/ponto/services/fechamento.py:683-687`; `app/ponto/services/efeito_ausencia.py:53-65`.)
- **Alterar escritores ultrapassaria essa avaliação:** `mudar_estado` grava e pode acordar a projeção do fechamento. Tocar gravado de competência exportada ou regra fora do pedido exige `!` pela regra local. (`app/ponto/services/ausencia.py:987-994`; `CLAUDE.md:265-268`.)

**Leis aplicáveis:** transportar a resposta do juiz é coerente com “testemunha lê, não recalcula”; um mapa puramente apresentacional não acrescenta predicado de veredito. L7 exige ajuda no mapa, e L-123 admite o padrão “juiz unico”. (`CLAUDE.md:20-22`; `app/docs/LEIS-UI.md:77-84`; `CLAUDE.md:65-70`; `app/docs/PLANO-PADROES.md:55-59`; `app/ponto/services/dia_decidido.py:453-481`.)

Execução da proposta, impacto real em produção, dados exportados, RED, mutantes, suite e smoke: **[não verificado]**. [nao verificado]

## 16/11 18:53 · sessao dbc9a9749f8d · ESPINHA-F2 · verificador · origem=Code

**AVAL-DEVOLVIDO em 2d e 9a:** o par ilegal não determina sozinho o dono da tarefa, e `_escrever_carimbo` não é escritor exclusivo dos três campos (`app/chamados/services/tarefa_do_dia.py:165`; `app/chamados/models.py:3393`).

As referências aos anexos usam suas linhas fornecidas, incluindo vazias; correspondência independente da cópia com o HEAD/hash informado: [nao verificado].

**2a — Fecha no ramo declarado.** Última RESPOSTA com `ato_texto` vazio retorna `respondida_sem_texto`, com RESPONDER do COLABORADOR; o juiz exige instante e texto não vazio (`app/chamados/services/tarefa_do_dia.py:159`; `app/chamados/services/tarefa_do_dia.py:161`; `app/chamados/juizes.py:1552`). A coerência quanto ao vazio confere, mas são fontes distintas: texto do último ato e texto projetado (`app/chamados/services/tarefa_do_dia.py:160`; `app/chamados/juizes.py:1552`).

Conservar essa fonte na F2 é compatível com conservar a exigibilidade e deixar a lei sobre devolução pendente; não revoga os destinos específicos de F5/F6 (`respostas_f2_estados_1611.txt:5`; `app/docs/RELATO.md:131`; `app/docs/CORTES.md:487`). Não autoriza mudança de tarefa nem intervenção em exportada (`complemento1_respostas_f2_estados_1611.txt:11`; `app/docs/CORTES.md:487`).

**2b — Fecha o defeito anterior de desenho; a prova efetiva continua pendente.** A API realmente calcula `aguarda_admin` por `estado == RESPONDIDA`; conservar essa fonte elimina a mudança adicional em pares legais que surgiria ao trocar estado por dono (`app/api/views.py:2550`; `app/api/views.py:2553`; `app/chamados/services/tarefa_do_dia.py:176`).

Isso é compatível com a prova que admite somente diferenças dos pares ilegais: essas diferenças são permitidas, não obrigatórias (`respostas_registradas.md:5`). Portanto, **essa alteração proposta já não introduz a diferença não admitida apontada anteriormente**; comprovar que nenhuma outra diferença existe exige a comparação por pk, cujo resultado é [nao verificado] (`respostas_registradas.md:5`). A conservação vale na F2; o destino pelo dono previsto para F6 continua registrado (`app/docs/CORTES.md:487`). Não há autorização adicional sobre exportada/dinheiro/`!` (`app/docs/CORTES.md:487`).

**2c — Fecha.** Ressuscitar não grava REABERTURA nesse caminho, e o código declara expressamente que devolver ao colaborador permanece lei não decidida; a porta efetiva zera a projeção pelo descarimbo em lote (`app/chamados/models.py:1622`; `app/chamados/models.py:1626`; `app/chamados/models.py:1629`).

Manter a discussão fora da F2 é compatível com a resposta 8 vigente e com o registro da lei; “ressuscitada sem ação” permanece restrito ao caso de último SILENCIO, não universal (`respostas_f2_estados_1611.txt:12`; `app/docs/RELATO.md:131`; `app/chamados/services/tarefa_do_dia.py:156`; `app/chamados/services/tarefa_do_dia.py:176`). A eventual obra futura depende do corte declarado, sem autorizar agora alteração de fila ou de exportadas (`complemento1_respostas_f2_estados_1611.txt:5`; `app/docs/LEIS.md:151`).

**2d — Não fecha como afirmação universal.** Pergunta viva com instante e sem texto realmente resulta em `estado=RESPONDIDA`, pois esse ramo lê `respondida_em` cru (`app/chamados/models.py:3154`; `app/chamados/models.py:3155`). Porém isso **não implica que a tarefa mande o colaborador responder**: a tarefa consulta o último ato; RESPOSTA vazia manda responder, DEVOLUCAO manda o admin validar e SILENCIO não exige ação (`app/chamados/services/tarefa_do_dia.py:158`; `app/chamados/services/tarefa_do_dia.py:161`; `app/chamados/services/tarefa_do_dia.py:165`; `app/chamados/services/tarefa_do_dia.py:176`).

A medição precisa separar o par projetado, o último ato e o resultado efetivo da tarefa; a conclusão sobre colaborador deve ficar condicionada ao ramo correspondente (`app/chamados/services/tarefa_do_dia.py:159`). Quantidade atual e ocorrência desses contraexemplos em produção: [nao verificado]. O encaminhamento de leitores de instante cru à B2 confere com o escopo documentado, mas não autoriza cura pela F2; estoque entregue continua exigindo `!` próprio (`app/chamados/juizes.py:1551`; `app/docs/LEIS.md:151`).

**9a — Não fecha a premissa nem demonstra o literal.** O comentário chama o trio de “UM ato”, e `_escrever_carimbo` atribui os três campos em memória; isso não equivale a escritor exclusivo de qualquer deles (`app/chamados/models.py:3184`; `app/chamados/models.py:3207`).

| Outro escritor/mecanismo | Escrita comprovada |
|---|---|
| `descarimbar_em_lote` | `update()` direto de instante, autor e via (`app/chamados/models.py:3391`; `app/chamados/models.py:3393`). |
| `encerrar_por_fato_no_espelho` | Atribui e salva somente a via (`app/chamados/models.py:3402`; `app/chamados/models.py:3403`). |
| `relavra_via.relavrar` | Atribui e salva somente a via, junto da lavra (`app/chamados/services/relavra_via.py:125`; `app/chamados/services/relavra_via.py:127`). |
| Exclusão do autor | A FK permite zerar `materializada_por` por `SET_NULL` (`app/chamados/models.py:3534`; `app/chamados/models.py:3536`). |

Os dois escritores exclusivos de via têm chamadores em comandos, e o próprio selo admite expressamente essas exceções (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`; `app/chamados/management/commands/relavrar_via_i6.py:102`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:44`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`). Incluindo testes, também existem `update()` diretos; nas migrações citadas há adição de campos de esquema, sem que isso sustente exclusividade do helper (`app/chamados/tests/test_e1_volta2.py:204`; `app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:21`; `app/chamados/migrations/0033_perguntadisputa_via_resolucao.py:13`). Inexistência absoluta de outros mecanismos dinâmicos: [nao verificado]; o scanner declara limites para `__dict__` e nomes externos (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:111`).

O corte exige literalmente **“efeito = o ato de materializacao, valor conservado”** (`app/docs/CORTES.md:487`). A cópia registra F2 L1 como **RESIDUAL, não MOOT**, precisamente porque descarimbar apaga e a projeção lê o campo atual (`app/docs/RELATO.md:109`). O código confirma essa perda: o descarimbo elimina o instante, e a projeção temporal parte do valor atual, sem reconstruí-lo após apagado (`app/chamados/models.py:3318`; `app/chamados/juizes.py:1390`; `app/chamados/juizes.py:1396`). Portanto, “ler o campo é ler o ato” não fornece a fonte do instante anterior nem resolve o residual registrado (`app/docs/RELATO.md:109`).

**9b — Fecha como conservação do resultado presente; não sana 9a.** O descarimbo grava instante e autor como `None`, via como `''`, e salva esses campos quando solicitado (`app/chamados/models.py:3318`; `app/chamados/models.py:3320`; `app/chamados/models.py:3322`). Conservar esse resultado é compatível com a prova exigida, mas a execução comparativa antes/depois é [nao verificado] (`respostas_registradas.md:5`). Conservar `None` no presente não recupera o instante anterior referido pelo residual (`app/chamados/juizes.py:1390`; `app/docs/RELATO.md:109`). Não amplia autorização de intervenção em exportada (`app/docs/CORTES.md:487`).

**9c — Fecha como registro lateral; não sana 9a.** Os tipos de `DecisaoPergunta` não incluem materialização (`app/chamados/models.py:4024`; `app/chamados/models.py:4026`; `app/chamados/models.py:4035`). Retirar a remessa de F2 L1 à E4d elimina o desvio anterior; o item 3 do corte destina explicitamente à E4d o ato de absorção, enquanto o item 1 mantém o destino próprio do residual F2 L1 (`app/docs/CORTES.md:487`). Registrar a ausência de materialização não autoriza construir esse ato nem comprova o fechamento de 9a (`complemento1_respostas_f2_estados_1611.txt:10`; `complemento1_respostas_f2_estados_1611.txt:11`; `app/docs/RELATO.md:109`).

Quanto ao alcance material, existe ligação concreta com o CSF: ele consome `sem_veredito_entre` para pai e destino e decide sua permissão pelo resultado (`app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:574`; `app/ponto/supra_juiz.py:588`). Assim, as exceções da prova não dispensam a restrição de exportadas: mudança exige `PAREI !` com tabela (`app/docs/CORTES.md:487`). Alteração efetiva de exportadas ou dinheiro pelo complemento: [nao verificado].

VEREDITO: AVAL-DEVOLVIDO em 2d, 9a -- 2d deduz dono apenas do par projetado, embora a tarefa dependa do último ato (`app/chamados/services/tarefa_do_dia.py:165`); 9a afirma escritor exclusivo inexistente e não resolve a fonte do instante apagado (`app/chamados/models.py:3393`; `app/docs/RELATO.md:109`; `app/docs/CORTES.md:487`).

## 16/11 18:52 · sessao 080c94e20fc7 · - · - · origem=Ronald (tmux)

A alegação literal de `_escrever_carimbo` como único escritor de qualquer dos três campos é **falsa**; o próprio selo permite quatro funções de escrita em `PerguntaDisputa` (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:43-58`):

- `_escrever_carimbo`: atribui os três campos em memória (`app/chamados/models.py:3207-3211`).
- `descarimbar_em_lote`: executa `UPDATE` direto dos três campos, sem chamar `_escrever_carimbo` (`app/chamados/models.py:3391-3393`).
- `encerrar_por_fato_no_espelho`: atribui e salva somente `via_resolucao`, preservando instante e autor (`app/chamados/models.py:3395-3403`).
- `relavra_via.relavrar`: atribui e salva somente `via_resolucao`, juntamente com a lavra (`app/chamados/services/relavra_via.py:118-128`). A documentação distingue expressamente “escritor único da re-lavra” de escritor único **do campo** (`app/chamados/services/relavra_via.py:58-64`).

Essas exceções têm chamadores de comando: `encerrar_perguntas_com_fato` chama a porta que troca somente a via; `relavrar_via_i6 --apply` chama `relavrar` (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:65-76`; `app/chamados/management/commands/relavrar_via_i6.py:96-102`).

Há ainda a zeragem declarativa de `materializada_por` por exclusão de usuário, porque a FK usa `SET_NULL`; o próprio selo registra que esse mecanismo fica fora de seu alcance (`app/chamados/models.py:3534-3537`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:113-115`).

Em testes há escritas diretas por `UPDATE`, inclusive dos três campos simultaneamente; portanto “em todo o repo” também não fecha se incluir testes (`app/chamados/tests/test_e1_volta2.py:204-206`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:169-170`).

As migrações encontradas para esses campos adicionam/alteram esquema: a 0016 adiciona instante e autor nulos, e a 0033 adiciona via com `default=''`; execução efetiva dessas migrações nesta base: [nao verificado] (`app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:21-29`; `app/chamados/migrations/0033_perguntadisputa_via_resolucao.py:13-16`).

A simulação da ressuscitação chama `descarimbar(salvar=False)` em cópia; a ressuscitação efetiva chama `descarimbar_em_lote` (`app/chamados/models.py:1544-1557`; `app/chamados/models.py:1628-1630`). O homônimo de `RegularizacaoExterna` também recebe instante e autor, mas pertence a outro modelo (`app/chamados/services/regularizacao_ext.py:198-201`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:47-49`).

O levantamento por AST incluindo migrações, com tratamento de aliases/dicionários conforme o scanner existente, encontrou os quatro escritores de `PerguntaDisputa` acima; inexistência absoluta de outros mecanismos dinâmicos fora do alcance do scanner: [nao verificado] (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:99-115`). A identidade Git HEAD/hash não pôde ser confirmada porque a cópia não contém repositório Git: [nao verificado].

_6170 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
