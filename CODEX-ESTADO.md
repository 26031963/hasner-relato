# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 20:17** · publicado em **16/11/2026 20:20** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 16/11 18:51 · sessao 2e39e403ec2d · - · - · origem=Ronald (tmux)

**9a — Não fecha a premissa nem o literal.** O comentário realmente chama os três campos de “UM ato”, mas `_escrever_carimbo` não é escritor exclusivo: `descarimbar_em_lote` executa `update()` direto dos três; `encerrar_por_fato_no_espelho` escreve apenas `via_resolucao`; o selo declara ainda `relavra_via.relavrar` como escritor da via (`app/chamados/models.py:3184`; `app/chamados/models.py:3391`; `app/chamados/models.py:3393`; `app/chamados/models.py:3402`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:44`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:57`).

O corte manda, se residual, **“efeito = o ato de materializacao, valor conservado”**; o registro da re-medição declara precisamente F2 L1 **RESIDUAL, não MOOT**, porque descarimbar apaga e a projeção consulta o campo atual (`app/docs/CORTES.md:487`; `app/docs/RELATO.md:109`). O código confirma: descarimbo apaga o instante; a projeção temporal começa pelo valor atual, apenas escondendo valores posteriores a `agora`, sem reconstruir um carimbo apagado (`app/chamados/models.py:3318`; `app/chamados/juizes.py:1390`; `app/chamados/juizes.py:1396`). “Ler o campo é ler o ato” não demonstra a fonte do instante anterior que permanece residual (`app/docs/RELATO.md:109`).

**9b — Fecha como conservação do resultado atual, não como resolução de 9a.** Descarimbar grava `None` para instante e autor, e `''` para via; portanto dizer genericamente “os três gravam None” seria inexato (`app/chamados/models.py:3318`; `app/chamados/models.py:3322`). Preservar esse resultado presente não fornece o instante apagado referido pela re-medição (`app/chamados/juizes.py:1390`; `app/docs/RELATO.md:109`).

**9c — A premissa confere; o registro lateral não sana 9a.** Os oito tipos de `DecisaoPergunta` não incluem materialização, e a remessa explícita do item 3 do corte é do predicado novo de absorção para E4d (`app/chamados/models.py:4024`; `app/chamados/models.py:4035`; `app/docs/CORTES.md:487`). Retirar a remessa de F2 L1 evita aquele desvio, mas não comprova cumprimento do destino residual do item 1 (`app/docs/CORTES.md:487`; `app/docs/RELATO.md:109`).

**Exportada/dinheiro/`!`:** o corte exige que `supra_juiz` em competência exportada não mude; eventual DIFF divergente exige `PAREI !` com tabela (`app/docs/CORTES.md:487`). Existe alcance indireto concreto: CSF lê `sem_veredito_entre` para pai e destino e decide a permissão de execução pelo resultado (`app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:574`; `app/ponto/supra_juiz.py:588`). Invariância efetiva de frota e efeito monetário: [nao verificado].

## 16/11 18:51 · sessao 3618618bcf91 · - · - · origem=Ronald (tmux)

**2a — confere, no ramo declarado.** Última RESPOSTA com `ato_texto` vazio retorna `respondida_sem_texto` e RESPONDER do COLABORADOR; o juiz exige instante e texto não vazio na projeção. São leituras distintas, coerentes quanto ao vazio, sem equivalência garantida entre ato e projeção (`app/chamados/services/tarefa_do_dia.py:159`; `app/chamados/services/tarefa_do_dia.py:161`; `app/chamados/juizes.py:1546`; `app/chamados/juizes.py:1552`).

**2b — fecha o defeito anterior da conservação da API.** A API calcula `aguarda_admin` exclusivamente de `_estado == RESPONDIDA`; conservar essa expressão elimina a mudança adicional em pares legais que decorreria de consultar o dono (`app/api/views.py:2550`; `app/api/views.py:2553`; `app/chamados/services/tarefa_do_dia.py:176`). A prova efetiva main/cópia, por pk, continua [nao verificado]. O corte F6 trata do dono em F6; o corte F5 mantém resposta vazia fora do app e registra a pergunta de lei, permitindo delimitar a conservação proposta à F2 (`app/docs/CORTES.md:487`; `app/chamados/services/pergunta_no_app.py:81`).

**2c — confere.** Ressuscitar preserva a fila existente e não grava REABERTURA nesse caminho; a lei sobre devolver ao colaborador continua registrada e não decidida, inclusive junto da pergunta sobre resposta vazia (`app/chamados/models.py:1622`; `app/chamados/models.py:1626`; `app/chamados/models.py:1629`; `app/docs/RELATO.md:131`; `app/docs/LEIS.md:151`). Não autoriza correção nova na F2; eventual nova decisão e DIFF são futuro declarado no anexo [nao verificado].

**2d — não fecha como afirmação universal.** Viva com instante e texto vazio satisfaz o predicado ilegal e resulta em `estado=RESPONDIDA`, porque C6 viva exclui validação e o estado usa o instante cru (`app/chamados/juizes.py:1531`; `app/chamados/juizes.py:1534`; `app/chamados/juizes.py:1405`; `app/chamados/models.py:3151`; `app/chamados/models.py:3155`). Porém esse par projetado **não garante** RESPONDER do colaborador: a tarefa lê o último ato; RESPOSTA vazia produz colaborador, DEVOLUCAO produz admin, SILENCIO produz nenhuma ação (`app/chamados/services/tarefa_do_dia.py:158`; `app/chamados/services/tarefa_do_dia.py:161`; `app/chamados/services/tarefa_do_dia.py:165`; `app/chamados/services/tarefa_do_dia.py:176`). Ainda existem guardas anteriores e exposição administrativa específica (`app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/services/tarefa_do_dia.py:306`; `app/chamados/services/tarefa_do_dia.py:309`; `app/chamados/services/tarefa_do_dia.py:267`).

Correção necessária: medir o conjunto I-sem-T e discriminar último ato/resultado real da tarefa; afirmar colaborador apenas no ramo correspondente (`app/chamados/services/tarefa_do_dia.py:159`). Quantidade atual e ocorrência dos contraexemplos: [nao verificado]. Destinar leitores de instante cru ao complemento da B2 corresponde ao escopo documentado B/C/D; mudar a tarefa não corresponde à B2 (`app/chamados/juizes.py:1550`; `app/chamados/juizes.py:1551`; `app/docs/LEIS.md:151`).

O dado fornecido não traz a saída dessa medição; os números históricos dos documentos não certificam a população atual [nao verificado]. `git rev-parse HEAD` falhou porque esta cópia não contém repositório Git; correspondência independente do HEAD com o hash fornecido: [nao verificado].

## 16/11 18:47 · sessao be76af41eae5 · AUSENCIA-EFEITO-PELO-ESTADO · verificador · origem=Code

**AVAL-DEVOLVIDO em a, b e c quanto às condições literais abaixo; d e e conferem.** A identidade do HEAD, a comparação com `71e9576bf` e o cruzamento por `git diff --name-only` ficam **[não verificado]**: esta cópia não contém metadados Git.

1. **As premissas conferem, com correção das linhas dos chips.**

| MUDA | Conferência na cópia |
|---|---|
| **a** | A coluna lê `a.efeito`, inclusive no `data-value`; a propriedade consulta somente `efeito_vigente(self.tipo)`, sem status. O laço existente permite entregar o veredito ao template. (`app/templates/ponto/ausencias.html:158`; `app/ponto/models.py:849-852`; `app/ponto/views.py:2249-2254`.) |
| **b** | Os chips contam por tipo, sem excluir rejeitadas: Abona em **2208**, Desconta em **2210**, Suprime em **2211**. As linhas 2205–2207 contam estados. (`app/ponto/views.py:2205-2211`.) |
| **c** | O filtro de efeito também consulta somente conjuntos de tipos; o filtro de status é aplicado separadamente. (`app/ponto/views.py:2212-2226`.) |
| **d** | Confere: `veredito_de` delega ao núcleo, que trata especialmente apenas `rejeitada`; os demais estados seguem o efeito/default do tipo. (`app/ponto/services/dia_decidido.py:461-481`.) |
| **e** | Os quatro leitores citados existem: turnos, fechamento, coletor de efeitos e calendário. A proteção por status varia conforme a alimentação, detalhada no censo abaixo. (`app/ponto/turnos.py:2704`; `app/ponto/services/fechamento.py:697`; `app/ponto/services/efeito_ausencia.py:58`; `app/colaboradores/services/calendario.py:705`.) |

Os **quatro status reais** são `aguardando_documento`, `aguardando_decisao`, `aprovada` e `rejeitada`. `pendente` na URL significa os dois estados abertos; `cancelada` não integra essas choices, e cancelar pela tela grava `rejeitada`. (`app/ponto/models.py:701-704`; `app/ponto/views.py:2212-2219`; `app/ponto/views.py:2378-2389`.)

`veredito_do_tipo` aceita os quatro — e aceita outros valores sem validá-los. Seus retornos são exatamente:

| Condição | Retorno |
|---|---|
| `status == 'rejeitada'` | `rejeitado` |
| Outro status, efeito DESCONTA | `descontado` |
| Outro status, efeito SUPRIME | `suprimido` |
| Qualquer outro caso | `abonado` |

Essa é a implementação completa, com as quatro strings declaradas no módulo. (`app/ponto/services/dia_decidido.py:26`; `app/ponto/services/dia_decidido.py:461-473`.)

**Quem escreve status e tipo:**

- **Nascimento:** `criar_ausencia` recebe o tipo, calcula o estado inicial e instancia o registro com ambos. (`app/ponto/services/ausencia.py:475-486`; `app/ponto/services/ausencia.py:529-531`.)
- **Alteração de tipo:** `editar_ausencia_registro` valida o catálogo, atribui/salva o tipo e manda rejulgar o estado aberto. (`app/ponto/services/ausencia.py:624-625`; `app/ponto/services/ausencia.py:648-676`.)
- **Transição normal de status:** `mudar_estado` valida a transição e atribui/salva o novo estado. (`app/ponto/services/ausencia.py:969-988`.)
- **Exceção concreta:** o cancelamento do agendamento de férias atribui diretamente `rejeitada`, com transação e trilha obrigatória; portanto não cabe afirmar que toda atribuição passa por `mudar_estado`. (`app/ponto/services/ausencia.py:1484-1507`.)

**Quem lê status e tipo, por responsabilidade:**

| Leitor | Uso |
|---|---|
| Modelo | Status para abertura/aprovação; tipo para exigência documental e efeito. (`app/ponto/models.py:719-741`; `app/ponto/models.py:849-852`.) |
| Serviço de ausência | Tipo/anexo para estado inicial; tipo/status para rejulgamento e transições. (`app/ponto/services/ausencia.py:687-692`; `app/ponto/services/ausencia.py:937-944`; `app/ponto/services/ausencia.py:969-976`.) |
| Cobertura | Status para folha/cobrança; tipo para cobertura indeterminada. (`app/ponto/turnos.py:2523-2546`.) |
| Lista/template | Tipo/status para filtros, contadores, apresentação e alerta médico. (`app/ponto/views.py:2195-2233`; `app/templates/ponto/ausencias.html:157-179`.) |
| Juiz visual/histórico | Tipo/status para veredito; estados abertos para apresentação do lançamento pendente. (`app/ponto/services/dia_decidido.py:447-481`; `app/ponto/services/dia_decidido.py:610-615`.) |
| Exportação/fechamento | Seleção de aprovadas e classificação por tipo/efeito. (`app/folha/export.py:87-91`; `app/ponto/services/fechamento.py:683-701`.) |

2. **Leis, aval vigente e decisão d.**

**a–c são compatíveis com juiz único** quando apenas transportam/classificam a resposta existente, sem reproduzir a regra de rejeição ou efeito na view. Essa é a exigência da LEI-AKITA 2 e da L2; `veredito_de` já delega ao mesmo núcleo. (`CLAUDE.md:20-22`; `app/docs/LEIS-UI.md:23-25`; `app/ponto/services/dia_decidido.py:453-481`.)

Há uma precisão documental: **`JUIZES.md` não nomeia `dia_decidido` nesse domínio**; ele remete ao registro vivo, que registra `catalogo.efeito_vigente` para “paga, desconta ou suprime”. A autoridade visual compartilhada está declarada no próprio `dia_decidido`. Usá-la não exige nascer outro juiz. (`app/docs/JUIZES.md:10-15`; `app/docs/JUIZES.md:55-58`; `app/core/juizes.py:260-265`; `app/ponto/services/dia_decidido.py:16-24`.)

A grade chama `do_dia`; o espelho publica sua palavra e o PDF consome essa palavra. Portanto a premissa de apresentação compartilhada confere, embora o PDF não precise chamar diretamente `veredito_de`. (`app/colaboradores/services/calendario.py:737-756`; `app/ponto/services/espelho.py:328-345`; `app/relatorios/pdf_espelho.py:923-924`.)

**d não colide com `veredito_de`.** Existe, porém, outra pergunta já resolvida: `veredito_do_lancamento` retorna PENDENTE para os estados abertos, conforme o contrato anterior de pendente amarelo. A decisão d deve permanecer explicitamente sobre a coluna **Efeito**, sem ser apresentada como o estado decidido do pedido. (`app/ponto/services/dia_decidido.py:37-40`; `app/ponto/services/dia_decidido.py:439-450`; `app/ponto/services/dia_decidido.py:461-473`.)

**L-123:** o padrão “juiz unico” já é termo admitido; fazê-lo valer exige que coluna, contador e filtro consumam a autoridade, conforme proposto. (`CLAUDE.md:65-70`; `app/docs/PLANO-PADROES.md:55-58`.)

**L7, em a:** o novo `title` não deve ser prosa colada no template; precisa vir do mapa de apresentação da autoridade. O texto prescrito pode ser preservado sem criar regra de veredito. (`app/docs/LEIS-UI.md:77-84`; `app/ponto/services/dia_decidido.py:476-481`.)

A documentação da cópia registra Repetir em construção e os complementos da UI-Vínculo antes desta obra; a ordem anexada é compatível com esses registros. O cruzamento efetivo entre arquivos das obras em voo permanece **[não verificado]**. (`app/docs/RELATO.md:9`; `app/docs/RELATO.md:20-33`.)

3. **Condições que fecham e condições que precisam de emenda.**

**Resultado 5: SIM, a partição por vereditos fecha para todo par solicitado**, inclusive tipo sem efeito e status `cancelada` ou outro: rejeitada entra exclusivamente em rejeitado; todo restante entra exclusivamente em descontado, suprimido ou abonado. Tipo ausente do catálogo produz `None`, que cai em **abonado**. Isso comprova a partição do juiz atual, sem comprovar que um tipo desconhecido deveria conceder abono. (`app/ponto/catalogo/ausencias.py:293-295`; `app/ponto/services/dia_decidido.py:461-473`.)

**b — o universo precisa ser explicitado:** Total hoje conta registros de `_base`, depois de vigência — ou busca por registro —, colaborador e tipo, **antes de status e efeito**. A lista recebe esses dois cortes depois; portanto Total não significa quantidade de linhas atualmente filtradas. (`app/ponto/views.py:2188-2226`; `app/ponto/views.py:2248-2255`.)

Assim, a soma pode fechar em `_base` e ainda não cumprir a **L1 literal**, que exige o mesmo queryset da lista e da ação. Os links dos chips conservam status/efeito, tornando essa distinção observável na navegação. É necessário resolver essa promessa de universo em b/c, não usar apenas a soma como prova de L1. (`app/docs/LEIS-UI.md:12-15`; `app/templates/ponto/ausencias.html:48-67`; `app/ponto/views.py:2201-2226`.)

**c — a comparação literal não fecha:** a URL pede `abona/desconta/suprime`, mas o juiz devolve `abonado/descontado/suprimido`. É preciso declarar o mapa entre os vocabulários antes de comparar “veredito == pedido”. (`app/templates/ponto/ausencias.html:65-67`; `app/ponto/services/dia_decidido.py:26`.)

O **OR de pares `(tipo,status)` é possível sem juiz novo**: o helper chama o juiz, soma `n` no balde correspondente e reúne exatamente os pares desse veredito. A montagem do predicado apenas transporta a resposta; não precisa testar rejeição ou consultar conjuntos de efeitos novamente. O caso de nenhum par deve produzir filtro vazio. (`app/ponto/services/dia_decidido.py:453-481`; `app/ponto/views.py:2195-2219`; `app/ponto/turnos.py:2773-2774`.)

**a — resultado 6 não fecha como igualdade textual literal:** `ABONA` não é `abonado`, nem `SEM EFEITO` é `rejeitado`. A propriedade deve exigir `data-value == veredito_de(a)` e texto igual à apresentação mapeada desse veredito. A coluna também contém o selo ANEXO!, que não integra o retorno do juiz. (`app/ponto/services/dia_decidido.py:26`; `app/ponto/services/dia_decidido.py:481`; `app/templates/ponto/ausencias.html:158`.)

**O selo é satisfazível:** retirar da contagem/filtro os três conjuntos proibidos e usar `veredito_do_tipo` basta; o núcleo consulta `efeito_vigente`, sem usar aqueles conjuntos. O catálogo pode continuar atendendo perguntas diferentes, como tipos médicos, parcialidade e fim aberto. (`app/ponto/views.py:2208-2211`; `app/ponto/views.py:2222-2253`; `app/ponto/services/dia_decidido.py:460-473`.)

RED executado, mutantes mortos, suite e smoke dos resultados propostos: **[não verificado]**. [nao verificado]

4. **Exportada, dinheiro, `!` e MEDIR.**

**O corpo de `lista_ausencias` é de consulta/renderização; a tela não é só leitura:** ela oferece lançar, editar, aprovar e cancelar, com POSTs. Os itens a–d podem ficar restritos à apresentação/seleção sem escrever dinheiro ou gravado exportado. (`app/ponto/views.py:2165-2270`; `app/templates/ponto/ausencias.html:43`; `app/templates/ponto/ausencias.html:249`; `app/templates/ponto/ausencias.html:277-312`.)

Alterar status pelas ações existentes pode acordar a projeção do fechamento; ampliar esta obra para esses escritores ou para gravado exportado ultrapassaria o escopo e alcançaria as regras de `!`. Conservar a propriedade em e preserva os leitores financeiros existentes. (`app/ponto/services/ausencia.py:987-994`; `CLAUDE.md:265-268`; `app/ponto/services/fechamento.py:697-701`.)

**MEDIR: rejeitada NÃO entra no mapa de `_cap(..., para='cobranca')`.** A cobertura aplica `_pelo_efeito`, que exclui rejeitada antes de construir o mapa; a leitura de `_a.efeito` está protegida a montante. (`app/colaboradores/services/calendario.py:516`; `app/ponto/turnos.py:2530-2531`; `app/ponto/turnos.py:2765-2766`; `app/colaboradores/services/calendario.py:705`.)

Censo dos demais leitores diretos de efeito de registro, distinguindo ausência de filtro local de proteção anterior:

| Leitor | Proteção por status |
|---|---|
| `Ausencia.efeito` | Nenhuma: consulta só tipo. (`app/ponto/models.py:849-852`.) |
| Coluna atual | Nenhuma obrigatória: a lista admite todos os status por padrão. (`app/templates/ponto/ausencias.html:158`; `app/ponto/views.py:2189-2219`.) |
| `tipo_contradiz_documento` | **Nenhuma:** exige documento e consulta efeito do tipo; alimenta ANEXO!, gate e título do chamado. (`app/ponto/services/ausencia.py:754-757`; `app/ponto/services/ausencia.py:765`; `app/ponto/services/ausencia.py:802`; `app/ponto/models.py:867-869`.) |
| `dias_com_ausencia_de_dia_inteiro` | Sem filtro local; recebe cobertura de folha nos chamadores atuais. (`app/ponto/turnos.py:2704-2709`; `app/ponto/services/fechamento.py:683-687`; `app/ponto/services/efeito_ausencia.py:53-65`; `app/ponto/management/commands/diff_calculador.py:412`.) |
| Fechamento | Cobertura `para='folha'`: somente aprovadas. (`app/ponto/services/fechamento.py:683-697`; `app/ponto/turnos.py:2528-2529`.) |
| Coletor `efeito_ausencia` | Mesma proteção de folha. (`app/ponto/services/efeito_ausencia.py:53-58`; `app/ponto/turnos.py:2528-2529`.) |
| Command `ausencias_sem_efeito_na_celula` | Cobertura de folha antes de `efeito_vigente(a.tipo)`. (`app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:44-49`.) |
| Núcleo `veredito_do_tipo` | Rejeitada retorna antes da consulta ao efeito. (`app/ponto/services/dia_decidido.py:461-468`.) |

O outro leitor sem filtro de status é, portanto, **a contradição documental**, expressamente preservada em e; os leitores financeiros citados recebem somente aprovadas. O calendário não fornece o RED adicional de rejeitada previsto condicionalmente no aval. (`app/ponto/services/ausencia.py:754-757`; `app/ponto/turnos.py:2528-2531`; `app/colaboradores/services/calendario.py:516`.)

VEREDITO: AVAL-DEVOLVIDO em a,b,c -- a: resultado 6 exige distinguir texto e veredito (`app/ponto/services/dia_decidido.py:26`; `app/templates/ponto/ausencias.html:158`); b/c: Total anterior aos filtros não prova L1 (`app/ponto/views.py:2201-2226`; `app/docs/LEIS-UI.md:12-15`); c: pedido e retorno usam vocabulários diferentes (`app/templates/ponto/ausencias.html:65-67`; `app/ponto/services/dia_decidido.py:26`).

## 16/11 18:45 · sessao db86dfe23a64 · BUG-PAR-O-PAR · verificador · origem=Code

A prova que bloqueou a volta 2 está preenchida: os onze mutantes pedidos têm execução Django e morte por asserção nos anexos. (`integ_c4b_todos.txt`, `RESUMO.txt:1–11`; respectivos logs, linhas `Creating test database`, `AssertionError`, `FAILED (failures=1)` e `rc=1`.)

**1. Matriz L1–L5**

O helper examina card e modal, restringindo o botão Validar ao formulário da pergunta examinada; a fotografia persistida inclui perguntas, decisões e identificadores das notas. (`app/chamados/tests/test_bug_par_volta.py:192–215`.)

| Linha | Teste e cobrança efetiva | Mutante e resultado EXECUTADO | [nao verificado]
|---|---|---|
| **L1** | `test_matriz_linha1_entregue_fora163_recusa_estado_proprio_esconde_validar`: estado `bloqueada_competencia_entregue`; detalhe nos dois HTMLs; Validar ausente; porta recusa; nenhuma batida; fotografia inalterada. (`app/chamados/tests/test_bug_par_volta.py:222–235`.) | `c4_fundir_l1_em_l2`: **rc=1; FAILED (failures=1)**. Morreu na asserção do estado: `AssertionError: 'bloqueada_competencia' != 'bloqueada_competencia_entregue'`. (`integ_c4b_todos.txt`, bloco `c4_fundir_l1_em_l2.log`, traceback em `test_bug_par_volta.py:229`.) |
| **L2** | `test_matriz_linha2_fechada_nao_entregue_conserva_o_toque_e_o_motivo`: F6 real `aprovado`; estado `bloqueada_competencia`; detalhe e Validar nos dois HTMLs; `nao_gravou`, devolução; VALIDACAO + DEVOLUCAO; validação/materialização vazias; motivo e nota de competência; nenhuma batida; irmã preservada. (`app/chamados/tests/test_bug_par_volta.py:217–220`, `:237–273`.) | `c4_motivo_l2_paridade`: **rc=1; FAILED (failures=1)**. `AssertionError: 'PARIDADE' != 'COMPETENCIA_APROVADA'`. (`integ_c4b_todos.txt`, bloco `c4_motivo_l2_paridade.log`, traceback em `test_bug_par_volta.py:258`.) |
| **L3** | `test_matriz_linha3_163_declarado_x_gravado_mostra_a_espera`: estado de espera; detalhe/espera visíveis nos dois HTMLs; Validar ausente; porta espera; nenhuma batida; fotografia inalterada. (`app/chamados/tests/test_bug_par_volta.py:275–287`.) | `c4_desligar_portao_l3`: **rc=1; FAILED (failures=1)**. `AssertionError: 'aplicavel' != 'espera_declarado_x_gravado'`. (`integ_c4b_todos.txt`, bloco `c4_desligar_portao_l3.log`, traceback em `test_bug_par_volta.py:281`.) |
| **L4** | `test_matriz_linha4_nada_barra_aplicavel_validar_grava`: `aplicavel`; Validar nos dois HTMLs; porta valida o par; ambas recebem VALIDACAO, carimbo e via de declaração; duas batidas novas e sequência E–S–E–S. (`app/chamados/tests/test_bug_par_volta.py:289–302`; `app/chamados/tests/test_bug_par_projetado.py:103–110`.) | `c4_barrar_l4`: **rc=1; FAILED (failures=1)**. `AssertionError: 'bloqueada_competencia_entregue' != 'aplicavel'`. (`integ_c4b_todos.txt`, bloco `c4_barrar_l4.log`, traceback em `test_bug_par_volta.py:294`.) |
| **L5** | `test_matriz_linha5_fora163_sem_dia_do_fato_estado_proprio`: estado próprio `bloqueada_sem_dia_do_fato`; detalhe nos dois HTMLs; Validar ausente; porta igual; K6 não consultado; nenhuma batida; fotografia inalterada. (`app/chamados/tests/test_bug_par_volta.py:304–321`.) | `c4_fundir_l5_em_l1`: **rc=1; FAILED (failures=1)**. `AssertionError: 'bloqueada_competencia_entregue' != 'bloqueada_sem_dia_do_fato'`. (`integ_c4b_todos.txt`, bloco `c4_fundir_l5_em_l1.log`, traceback em `test_bug_par_volta.py:312`.) |

Nenhuma dessas cinco linhas ficou sem mutante executado; nenhuma morte ocorreu por importação ou fixture: todos os blocos mostram o teste identificado, `AssertionError` e `FAILED (failures=1)`, sem `ERROR`. (`integ_c4b_todos.txt`, blocos dos cinco `c4_*` acima.) [nao verificado]

**2. Achados da volta 1 e integração herdada**

| Mutante | Asserção que o matou | Resultado EXECUTADO |
|---|---|---|
| `c4b_espera_antes_de_f6` | VOLTA primeiro terminou com PARIDADE em vez de COMPETENCIA_APROVADA. (`app/chamados/tests/test_bug_par_volta.py:364–376`.) | **rc=1; FAILED (failures=1)**; `AssertionError: 'PARIDADE' != 'COMPETENCIA_APROVADA'`. (`integ_c4b_todos.txt`, bloco homônimo, traceback `:374 → :355`.) |
| `c4b_motivo_laco` | Laço perdeu o motivo de competência nas metades. (`app/chamados/tests/test_bug_par_volta.py:378–389`.) | **rc=1; FAILED (failures=1)**; mesma divergência de categoria. (`integ_c4b_todos.txt`, bloco homônimo, traceback `:388 → :355`.) |
| `c4b_motivo_passivo` | Passivo contou `recusa:PARIDADE` duas vezes em vez de `recusa:COMPETENCIA_APROVADA`. (`app/chamados/tests/test_bug_par_volta.py:391–403`.) | **rc=1; FAILED (failures=1)**; `AssertionError: Tuples differ`. (`integ_c4b_todos.txt`, bloco homônimo, traceback `:402`.) |
| `fundir_a_em_b` | Estado da testemunha L1 fundido em L2. (`app/chamados/tests/test_bug_par_volta.py:229`.) | **rc=1; FAILED (failures=1)**; `AssertionError` entre os dois estados. (`integ_c4b_todos.txt`, bloco `fundir_a_em_b.log`.) |
| `fundir_a_em_b_porta` | Retorno da porta L1 fundido em L2. (`app/chamados/tests/test_bug_par_volta.py:233`.) | **rc=1; FAILED (failures=1)**; `AssertionError` entre os dois estados. (`integ_c4b_todos.txt`, bloco `fundir_a_em_b_porta.log`.) |
| `f6_esconde` | Testemunha L2 passou a anunciar estado de entrega. (`app/chamados/tests/test_bug_par_volta.py:247`.) | **rc=1; FAILED (failures=1)**; `AssertionError: 'bloqueada_competencia_entregue' != 'bloqueada_competencia'`. (`integ_c4b_todos.txt`, bloco `f6_esconde.log`.) |

As seis mortes também são por asserção, sem erro de importação/fixture. O RED agregado registra **21 testes, três falhas**, precisamente nos três contraexemplos c4b; o verde fornecido registra **108 testes, OK**. (`integ_c4b_todos.txt`, seis blocos acima; `red_c4b.log`, linhas `FAIL:`, `Ran 21 tests` e `FAILED (failures=3)`; `verde_c4b.log`, linhas `Ran 108 tests` e `OK`.) [nao verificado]

O `rc=1` desses logs é o resultado esperado do teste mutado; o harness da cópia considera morte válida somente falha sem erros/skips e retorna zero quando a verificação dos mutantes passa. (`app/chamados/tests/bug_par_mutantes.py:224–229`.)

**3. Contador**

**Existe e chega a zero pela validação do admin:** o teste novo exige `(pendentes_aval, resolvidas, congelados)=(1,0,1)`, chama `validar_pergunta`, confirma o par gravado e exige `(0,1,1)`, conservando a identidade da soma e o resultado de `competencia_do_fato`. (`app/chamados/tests/test_bug_par_volta.py:453–472`.)

A precisão é esta: K6 permanece **mock constante `False`**, herdado da fixture; o teste não altera sua resposta para produzir o zero. Isso atende ao contraexemplo de validação sem mudar o juiz durante o teste; exercício do juiz real e zero das 163 em produção são **[nao verificado]**. (`app/chamados/tests/test_bug_par_projetado.py:46–53`; `app/chamados/tests/test_bug_par_volta.py:461–472`.)

O verde agregado não enumera os testes; sua execução individual sob esse nome não é identificável no anexo. **[nao verificado]** (`verde_c4b.log`, linhas `Found 108 test(s)` e `Ran 108 tests`.) [nao verificado]

**4. `exportado`: escritores, leitores e alcance**

`exportado` é valor de **`FechamentoMensal.status`**. F6 o lê junto de `aprovado`; K6, separadamente, lê `ExportacaoDominio` vigente e não invalidada. Portanto esse status, sozinho, não prova entrega. (`app/ponto/models.py:398–410`; `app/ponto/janelas.py:83–89`; `app/folha/export.py:588–594`.)

Não localizei atribuição literal de produção para `status='exportado'`; a origem de eventuais registros existentes é **[nao verificado]**. Gerar TXT atualmente aprova os fechamentos abertos e registra `ExportacaoDominio`; a porta genérica de restauração aceita campos do modelo, mas uma chamada gravando esse valor não está demonstrada. (`app/folha/views.py:246–252`, `:295–300`; `app/ponto/services/fechamento.py:1329–1332`, `:1373–1393`.)

No caminho examinado:

- **Produção e transporte:** F6 produz o conflito; o chokepoint o devolve; a lista do par conserva conflitos classificados COMPETENCIA_APROVADA; o compositor preserva a mensagem. (`app/ponto/janelas.py:87–88`; `app/ponto/registro_batida.py:159–161`; `app/chamados/services/materializacao.py:1467–1471`, `:1783–1789`.)
- **Escrita:** `_carimbar_recusa` grava `recusa_motivo` e nota; o wrapper o chama novamente após o rollback do par. Competência não recebe carimbo terminal: esse ramo é reservado a COLISAO. (`app/chamados/services/materializacao.py:1648–1664`; `app/chamados/services/validacao.py:554–560`.)
- **Leitura:** devolução usa o motivo, limpa a validação e registra DEVOLUCAO; fotografia/auditoria e telas também o leem. `flip_auto`, leitor capaz de alimentar alteração de batida, exige via terminal de paridade, ausente nessa recusa F6. (`app/chamados/services/validacao.py:181`, `:258–278`, `:293–300`; `app/templates/chamados/partials/modal_fio.html:226`; `app/templates/chamados/partials/_card_atendimento.html:192`; `app/ponto/services/flip_auto.py:128–132`.)

**Não fica somente no payload:** muda pergunta, atos, nota e eventualmente reabertura; retirar `validada_em` também emite eventos de célula. Isso não equivale a provar edição da própria célula. (`app/chamados/services/validacao.py:265–288`; `app/chamados/models.py:3330–3358`; `app/chamados/signals.py:252–259`.)

**Batida:** F6 recusa antes do `create`. **Fechamento:** a porta já agenda recálculo antes da materialização; esse callback herdado pode atualizar `FechamentoMensal` e lavrar `DiaPago`, inclusive com status não aberto; a guarda explícita cobre 09/2026 entregue. Logo, seria incorreto afirmar que essa porta nunca toca fechamento exportado. (`app/ponto/registro_batida.py:159–161`, `:176`; `app/chamados/services/validacao.py:651–663`; `app/ponto/services/fechamento.py:1798–1824`, `:1145–1201`, `:36–43`, `:560–564`.)

O delta não acrescenta esse agendamento nem uma chamada de emissão de TXT; movimento financeiro ou alteração de TXT **causados pela troca espera→recusa** permanecem **[nao verificado]**. (`delta_c4_total.diff`, hunk `validacao.py:647`; `app/chamados/services/materializacao.py:1444–1472`; emissão em `app/folha/views.py:295–300`.)

**Matriz:** sendo K6 falso, o impedimento F6 pode enquadrar-se em **L2**. Sendo efetivamente entregue e U163, nenhuma linha literal cobre esse caso: L1 exige fora163 e L2 exige não entregue; a guarda pula U163. O status fornecido não resolve essa distinção, e o teste F6 usa `aprovado`, não `exportado`. (`anexo complemento2_espinha_bug_par_c4_1611.txt`, item 2, linhas L1/L2; `app/chamados/services/materializacao.py:799–801`; `app/chamados/tests/test_bug_par_volta.py:217–220`.)

**5. PROIBIDO**

Não identifiquei infração nova demonstrada no delta: a guarda compõe K6 existente, F6 continua compartilhado com o chokepoint, e card/modal decidem por **estado**, exibindo o detalhe. O transporte L2 usa o escritor canônico de recusa. (`app/chamados/services/materializacao.py:776–828`, `:1444–1471`, `:1640–1664`; `app/templates/chamados/partials/_pergunta_card.html:44–51`; `app/templates/chamados/partials/modal_fio.html:252–275`.)

Quanto a exportada, há efeito persistido na pergunta e o alcance herdado de recálculo descrito acima; novo movimento de dados financeiros provocado pelo delta não está demonstrado. **[nao verificado]** (`app/chamados/services/validacao.py:662`, `:269–278`; `app/ponto/services/fechamento.py:1810`, `:1160`.)

O JSON atual confere com o SHA esperado e conserva U60/U103 de 60/103 elementos; correspondência histórica independente dos commits, ausência histórica de toque em `motor_calculo_v2.py`, apply/pouso em produção e medição de perguntas vivas sem dia permanecem **[nao verificado]**. (`app/chamados/services/universo_bug_par.py:7–30`; `app/chamados/data/bug_par_163_2026_11_13.json:1`.)

VEREDITO: APROVA -- delta c4 e prova de integração exigida nesta volta: `integ_c4b_todos.txt/RESUMO.txt:1–11`; contador por validação: `app/chamados/tests/test_bug_par_volta.py:453–472`; sem nova infração demonstrada, com os limites de exportada registrados no item 4.

## 16/11 18:44 · sessao d773700f6943 · - · - · origem=Ronald (tmux)

Parecer de leis e escopo:

- **a** pode consumir `veredito_de` sem juiz novo: ele delega integralmente a `veredito_do_tipo`; o módulo declara a resposta visual única para grade/cartão/app (`app/ponto/services/dia_decidido.py:16-24,453-481`). O novo `title` precisa vir da apresentação compartilhada da autoridade: L7 proíbe ajuda hardcoded no template (`app/docs/LEIS-UI.md:77-84`). O anexo prescreve o texto, mas não obriga hardcode.
- **b/c** podem compartilhar um agrupamento por `(tipo,status)` e apenas traduzir respostas do juiz para contadores e predicados ORM; essa tradução não precisa decidir novamente efeito ou rejeição (`app/ponto/services/dia_decidido.py:453-481`). Devem mapear `abona/desconta/suprime` da URL para `abonado/descontado/suprimido` do juiz, pois os vocabulários diferem (`app/ponto/views.py:2220-2226`; `app/ponto/services/dia_decidido.py:26`).
- **d** não colide com `veredito_de`: somente `rejeitada` desvia do efeito/default; outros status seguem o catálogo (`app/ponto/services/dia_decidido.py:461-473`). Existe, entretanto, regra anterior expressa de **estado do lançamento** pendente amarelo, respondida por outra função, `veredito_do_lancamento`; não equiparar as duas perguntas (`app/ponto/services/dia_decidido.py:37-40,439-450`).
- **e** conservar a propriedade e o selo ANEXO! preserva a separação existente: `efeito` consulta só o tipo; `contradiz_documento` delega a `tipo_contradiz_documento` (`app/ponto/models.py:849-869`).

**Resultado 5:** a partição dos quatro vereditos fecha inclusive para tipo desconhecido/sem efeito e status `cancelada`/outro: catálogo pode devolver `None`, mas o juiz termina em `ABONADO` para qualquer valor diferente de DESCONTA/SUPRIME e qualquer status diferente de rejeitada (`app/ponto/catalogo/ausencias.py:293-295`; `app/ponto/services/dia_decidido.py:461-473`). Isso é a classificação literal atual, não prova de que o tipo desconhecido deveria juridicamente abonar.

**Universo/L1:** Total hoje é `_base.count()` depois de vigência/registro/colaborador/tipo e antes dos cortes por status e efeito (`app/ponto/views.py:2188-2226`). Portanto a igualdade pode fechar em `_base` sem ser igual ao comprimento da lista filtrada. Os chips preservam os filtros de status/efeito ao navegar (`app/templates/ponto/ausencias.html:48-67`), enquanto L1 exige o mesmo queryset da lista **e da ação** (`app/docs/LEIS-UI.md:12-15`). O aval deve declarar esse universo; não afirmar cumprimento integral da L1 apenas pela soma.

**Selo:** satisfazível se proibir os conjuntos na contagem/filtro diretamente em `lista_ausencias`; todos esses usos atuais estão em `app/ponto/views.py:2208-2211,2222-2226`. A autoridade pode continuar consultando catálogo, como faz em `app/ponto/services/dia_decidido.py:460-473`. A lista ainda usa legitimamente outras funções/conjuntos do catálogo (`app/ponto/views.py:2232,2247-2253`).

**Registro dos juízes:** `JUIZES.md` não registra `dia_decidido` como juiz de Ausencia; identifica-se como lookup e remete ao registro vivo (`app/docs/JUIZES.md:10-15,55-58`). O registro vivo nomeia `catalogo.efeito_vigente` para “paga, desconta ou suprime” (`app/core/juizes.py:260-265`). A autoridade visual está documentada no próprio módulo (`app/ponto/services/dia_decidido.py:16-24`). L-123 aceita o termo “juiz unico”, já presente no glossário (`CLAUDE.md:65-70`; `app/docs/GLOSSARIO.md:12`).

**Aval vigente/fila:** a documentação da cópia registra AUSENCIA-EFEITO aguardando novo aval e o editor Repetir em construção (`app/docs/RELATO.md:9,20-24`); os complementos 3/4 do painel estão descritos em `app/docs/RELATO.md:28-33`. Cruzamento por `git diff --name-only` não é verificável nesta cópia sem `.git` [não verificado].

**Somente leitura:** o corpo de `lista_ausencias` faz consultas e renderização (`app/ponto/views.py:2165-2270`); a **tela** tem lançar, editar, aprovar e cancelar (`app/templates/ponto/ausencias.html:43,117-121,249,273-282,304-312`). Cancelar chama a porta de decisão para gravar rejeitada (`app/ponto/views.py:2371-2389`). O escopo a–d não exige escrever dinheiro/exportada; ampliar para esses atos entra no rol de `!` de `CLAUDE.md:265-268`.

## 16/11 18:44 · sessao b205a16d7f54 · - · - · origem=Ronald (tmux)

Conferência do juiz e dos estados:

- Os quatro status reais são `aguardando_documento`, `aguardando_decisao`, `aprovada`, `rejeitada` (`app/ponto/models.py:701-704`). `pendente` é alias da URL para os dois estados `ABERTAS`, não status gravado (`app/ponto/services/ausencia.py:912`; `app/ponto/views.py:2212-2219`). `cancelada` não integra as choices nem a tabela de transições (`app/ponto/models.py:701-704`; `app/ponto/services/ausencia.py:914-923`).
- `veredito_do_tipo` aceita os quatro estados e qualquer outro valor sem validação: somente `status == 'rejeitada'` retorna `rejeitado`; nos demais casos consulta o catálogo, devolve `descontado` para DESCONTA, `suprimido` para SUPRIME e `abonado` para todo resto (`app/ponto/services/dia_decidido.py:460-473`). As strings são exatamente `abonado`, `descontado`, `suprimido`, `rejeitado` (`app/ponto/services/dia_decidido.py:26`).
- Tipo ausente do catálogo gera `None` no efeito e portanto `abonado` quando não rejeitado; vale igualmente para `cancelada`, `pendente`, outro status ou `None` (`app/ponto/catalogo/ausencias.py:293-295`; `app/ponto/services/dia_decidido.py:461-473`). Isso preserva a partição matemática por **vereditos**, embora não signifique que o catálogo concedeu abono (`mesmas referências`).
- `veredito_de(a)` só delega tipo/status a esse núcleo, logo d não colide com ele (`app/ponto/services/dia_decidido.py:476-481`).
- Há juiz diferente para o estado de **um lançamento**: `veredito_do_lancamento` devolve PENDENTE para ABERTAS (`app/ponto/services/dia_decidido.py:439-450`), com corte F2 que expressamente critica pintar de verde um atestado ainda não decidido (`app/ponto/services/dia_decidido.py:37-40`). O histórico o usa para mudar palavra/cor (`app/ponto/services/dia_decidido.py:598-615`). Isso é tensão semântica a declarar; a coluna “Efeito” pode continuar perguntando o efeito do tipo conforme d, mas a pergunta “em que pé está o pedido?” já tem outra resposta autorizada (`app/ponto/services/dia_decidido.py:442-445`).

Quem escreve:

| Campo | Escritor efetivo |
|---|---|
| tipo/status inicial | `criar_ausencia`: valida tipo no catálogo, calcula estado inicial, monta kwargs e instancia/salva (`app/ponto/services/ausencia.py:325-327`, `:475-486`, `:529-531`). |
| tipo editado | `editar_ausencia_registro`: valida catálogo e atribui/salva; depois rejulga o status aberto se mudou tipo (`app/ponto/services/ausencia.py:624-625`, `:646-659`, `:674-676`). |
| status por transição | `mudar_estado`: lê atual, valida transição e atribui/salva (`app/ponto/services/ausencia.py:969-988`). |
| status de férias canceladas | **Exceção concreta à frase “escritor único”**: `rejeitar_ferias_do_agendamento` atribui diretamente `'rejeitada'`, salva e exige trilha, sem chamar `mudar_estado` (`app/ponto/services/ausencia.py:1488-1507`). |

`Ausencia.save` não atribui status/tipo: lê os campos para carimbo de decisão, minutos de falta aprovada e duração/range (`app/ponto/models.py:882-887`, `:912-918`). O admin é `SoLeitura` (`app/ponto/admin.py:38`); API e views encaminham entradas às portas, por exemplo API `criar_ausencia` (`app/api/views.py:1812-1818`), edição de tela (`app/ponto/views.py:2873`), decisão/cancelamento (`app/ponto/views.py:2714`, `:2775`, `:2811`).

Leitores de status e tipo, com papéis concretos:

- Modelo: abertura lê status (`app/ponto/models.py:719-720`), pode aprovar lê status (`:741`), exige documento lê tipo (`:728-729`), efeito lê tipo (`:849-852`), save lê ambos (`:882-887`).
- Máquina de estado: estado inicial lê tipo/anexo (`app/ponto/services/ausencia.py:937-944`); rejulgamento lê status e tipo (`:687-692`); anexação lê status para transição (`:1033-1034`); reversão exige rejeitada (`:1275-1281`).
- Cobertura: `_pelo_efeito` lê status, aprovadas para folha e não rejeitadas para cobrança (`app/ponto/turnos.py:2523-2532`); cobertura/range lê tipo para NULL indeterminado (`:2543-2546`, `:2566-2570`). `ausencias_vigentes` lê tipo/range e **não filtra status** (`:3170-3178`).
- Lista: lê tipo em filtro (`app/ponto/views.py:2195-2196`), status em contadores/filtros (`:2197`, `:2205-2219`), tipo em contadores/filtros de efeito (`:2208-2211`, `:2221-2226`), tipo/status em alerta médico (`:2232`), tipo no laço (`:2253`).
- Template lista: tipo exibido (`app/templates/ponto/ausencias.html:157`), status pintado (`:177-179`), efeito atual por tipo (`:158`).
- Juiz visual lê tipo/status (`app/ponto/services/dia_decidido.py:481`); juiz de lançamento lê status (`:447-450`); histórico lê tipo/status (`:614-615`).
- Folha/export: queryset exige tipos e `status='aprovada'` (`app/folha/export.py:87-91`); fechamento lê status aprovado e tipo (`app/ponto/services/fechamento.py:662`, `:694`).
- Sinais leem tipo para expandir dias afetados (`app/ponto/signals.py:124`).

O Total atual conta `_base`, antes dos filtros de status/efeito, depois de vigência ou busca por registro, colaborador e tipo (`app/ponto/views.py:2188-2202`, `:2212-2226`). Portanto não é tamanho da lista já filtrada por status/efeito (`mesmas referências`).

## 16/11 18:44 · sessao c608cc5d094c · - · - · origem=Ronald (tmux)

MEDIR: **rejeitada não entra no mapa** de `calendario.py:516`. A chamada usa `para='cobranca'`; `cobertura_ausencia_periodo` aplica `_pelo_efeito`, que exclui `status='rejeitada'`, antes de construir o mapa; `_a.efeito` em `calendario.py:705` recebe esse universo protegido. (`app/colaboradores/services/calendario.py:516`; `app/ponto/turnos.py:2765`; `app/ponto/turnos.py:2530`; `app/ponto/turnos.py:2531`; `app/ponto/turnos.py:2766`; `app/colaboradores/services/calendario.py:705`)

Censo dos leitores diretos de registro, fora testes: [nao verificado]

| Leitor | Status a montante / consequência |
|---|---|
| `app/templates/ponto/ausencias.html:158` | Lê `a.efeito`; a lista admite registros de qualquer status por padrão e só filtra quando solicitado. (`app/ponto/views.py:2189`; `app/ponto/views.py:2212`) |
| `app/ponto/services/fechamento.py:697` | Recebe cobertura `para='folha'`, portanto somente aprovada; calcula justificadas/suprimidas. (`app/ponto/services/fechamento.py:683`; `app/ponto/turnos.py:2528`; `app/ponto/turnos.py:2529`; `app/ponto/services/fechamento.py:698`) |
| `app/ponto/services/efeito_ausencia.py:58` | Recebe cobertura `para='folha'`, portanto somente aprovada; produz justificadas e suprimidas. (`app/ponto/services/efeito_ausencia.py:53`; `app/ponto/turnos.py:2529`; `app/ponto/services/efeito_ausencia.py:59`) |
| `app/ponto/turnos.py:2704` | Não verifica status localmente, mas todos os chamadores atuais fornecem cobertura `para='folha'`. (`app/ponto/turnos.py:2709`; `app/ponto/services/efeito_ausencia.py:53`; `app/ponto/services/efeito_ausencia.py:65`; `app/ponto/services/fechamento.py:683`; `app/ponto/services/fechamento.py:687`; `app/ponto/management/commands/diff_calculador.py:412`) |
| `app/colaboradores/services/calendario.py:705` | Cobrança exclui rejeitada; aberto continua possível. (`app/colaboradores/services/calendario.py:516`; `app/ponto/turnos.py:2531`) |
| `app/ponto/services/ausencia.py:757` | `tipo_contradiz_documento` chama `efeito_vigente(ausencia.tipo)` **sem condição de status**; só exige documento. É a fonte de `contradiz_documento` e do `ANEXO!`, preservados pelo aval. (`app/ponto/services/ausencia.py:754`; `app/ponto/models.py:867`; `app/ponto/models.py:869`; `app/templates/ponto/ausencias.html:158`) |
| `app/ponto/services/dia_decidido.py:468` | `veredito_do_tipo` já retorna rejeitado antes da leitura do catálogo quando recebe `status='rejeitada'`. (`app/ponto/services/dia_decidido.py:461`; `app/ponto/services/dia_decidido.py:462`) |
| `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:49` | `efeito_vigente(a.tipo)` recebe cobertura `para='folha'`; só aprovada. (`app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:44`; `app/ponto/turnos.py:2529`) |

O outro leitor **sem filtro de status**, além da coluna atual e do núcleo de propriedade `Ausencia.efeito`, é `tipo_contradiz_documento`; seus consumidores adicionais são o gate de aprovação e o título do chamado. (`app/ponto/models.py:852`; `app/ponto/services/ausencia.py:757`; `app/ponto/services/ausencia.py:765`; `app/ponto/services/ausencia.py:802`)

`status_dia.py:311` lê `efeito_vigente` de `f['tipo_do_dia']`, um fato da precedência, e não diretamente de registro de Ausencia. (`app/ponto/status_dia.py:298`; `app/ponto/status_dia.py:308`; `app/ponto/status_dia.py:311`)

Para R5: tipo ausente do catálogo produz `None` em `efeito_vigente`; isso cai em **ABONADO** no juiz. Qualquer status diferente de rejeitada, inclusive cancelada ou desconhecido, segue o mesmo caminho; por isso a classificação por todos os pares não perde esses registros. (`app/ponto/catalogo/ausencias.py:293`; `app/ponto/catalogo/ausencias.py:295`; `app/ponto/services/dia_decidido.py:461`; `app/ponto/services/dia_decidido.py:468`; `app/ponto/services/dia_decidido.py:473`)

Atenção de semântica: há também `veredito_do_lancamento`, que devolve PENDENTE para ABERTAS; seu próprio texto distingue “em que pé está o pedido” de `veredito_de`, a pergunta de efeito. Assim d não contradiz o corpo de `veredito_de`, embora deliberadamente escolha efeito na coluna. (`app/ponto/services/dia_decidido.py:439`; `app/ponto/services/dia_decidido.py:442`; `app/ponto/services/dia_decidido.py:448`; `app/ponto/services/dia_decidido.py:450`; `app/ponto/services/dia_decidido.py:481`)

O registro vivo aponta `efeito_vigente` para “a ausência paga, desconta ou suprime o dia?”. (`app/core/juizes.py:261`)

## 16/11 18:43 · sessao 40dba8c4b7d0 · - · - · origem=Ronald (tmux)

- **`status='exportado'` pertence ao `FechamentoMensal`, não à pergunta nem ao juiz K6.** O modelo aceita esse valor; F6 lê `aprovado/exportado`. K6 lê `ExportacaoDominio` vigente e não invalidada. Portanto `status=exportado` não prova competência entregue. (`app/ponto/models.py:398-410`; `app/ponto/janelas.py:83-88`; `app/folha/export.py:588-594`)

- **Escritor desse status:** não localizei atribuição literal de produção para `exportado`; origem dos registros existentes **[nao verificado]**. Gerar TXT atualmente aprova os abertos e registra `ExportacaoDominio`, sem atribuir esse status. A porta genérica de restauração aceita campos do modelo, mas uma chamada que escreva `exportado` não está demonstrada. (`app/folha/views.py:246-252`, `:295-300`; `app/ponto/services/fechamento.py:1329-1332`, `:1373-1393`)

- **Leitores relevantes:** F6 lê o status e produz mensagem contendo `exportado`; `_competencia_que_barra` a reutiliza na testemunha e na espera tardia; `registrar_batida` consulta o mesmo F6 e retorna antes do `create`. A guarda K6 pula U163. (`app/ponto/janelas.py:86-88`; `app/chamados/services/materializacao.py:799-801`, `:824-828`, `:927`, `:1444-1452`; `app/ponto/registro_batida.py:159-161`, `:176`)

- **Quem escreve a recusa:** o chokepoint produz o conflito; o registro do par guarda conflitos classificados `COMPETENCIA_APROVADA`; o compositor preserva esse texto; `_carimbar_recusa` grava `recusa_motivo` e a nota no fio, sem carimbo terminal de competência. O wrapper refaz isso após rollback do par. (`app/chamados/services/materializacao.py:1462-1472`, `:1783-1789`, `:1648-1664`, `:1960`; `app/chamados/services/validacao.py:554-560`)

- **Quem lê a recusa:** devolução verifica motivo, limpa validação e registra DEVOLUCAO usando-o; fotógrafo/auditoria e telas também o apresentam. O leitor potencialmente ligado a alteração de batida, `flip_auto`, restringe-se à via terminal de paridade, ausente na recusa F6. (`app/chamados/services/validacao.py:258-278`, `:293-300`; `app/templates/chamados/partials/modal_fio.html:226`; `app/templates/chamados/partials/_card_atendimento.html:192`; `app/ponto/services/flip_auto.py:128-132`; `app/chamados/services/materializacao.py:1650-1660`)

- **Não é somente payload da porta:** a troca espera→recusa altera pergunta, motivo, atos, nota e eventualmente reabertura; limpar `validada_em` também emite eventos de célula. Isso não demonstra edição da própria `CelulaDia`. (`app/chamados/services/validacao.py:265-288`; `app/chamados/models.py:3330-3358`; `app/chamados/signals.py:252-259`)

- **Ressalva essencial sobre fechamento:** antes de materializar, a validação já agenda `recalcular_por_evento`; esse trecho é herdado, não acrescentado pelo delta fornecido. `_EsperaNaCriacao` desfazia apenas o atomic interno da unidade; o agendamento externo permanecia. O callback pode atualizar `FechamentoMensal`, inclusive status não aberto, e lavrar `DiaPago`; só há guarda explícita para 09/2026 entregue. Assim, não afirmar “essa porta nunca toca fechamento exportado”. O delta financeiro efetivamente causado pela troca de estado é **[nao verificado]**. (`app/chamados/services/validacao.py:651-663`; `app/chamados/services/materializacao.py:1881`, `:1944-1964`; `app/ponto/services/fechamento.py:1798-1824`, `:1145-1189`, `:1191-1201`, `:36-43`, `:560-564`)

- **TXT:** a emissão persiste `ExportacaoDominio` na porta específica; não há chamada nova dessa porta no ramo F6 examinado. Movimento executado de TXT neste caso **[nao verificado]**. (`app/folha/views.py:295-300`; `app/chamados/services/materializacao.py:1444-1472`, `:1640-1664`)

- **Linha da matriz, condicional:** se K6 falso, `exportado` pode cair em L2 pelo mesmo F6, pois status não prova entrega. Se K6 verdadeiro e pergunta U163, nenhuma linha literal cobre: L1 exige fora163, L2 exige não entregue; U163 pula a guarda K6. Os testes fornecidos usam F6 `aprovado`, sem prova executada do caso `exportado`. (`app/chamados/services/materializacao.py:799-801`; `app/folha/export.py:588-594`; `app/chamados/tests/test_bug_par_volta.py:213-216`; anexo COMPLEMENTO-2, item 2, linhas L1/L2)

**Conclusão para PROIBIDO:** há mudança comprovável na resposta persistida de perguntas associadas a F6 `exportado`; não há prova de novo escritor de competência/exportação nem de movimento financeiro causado pelo delta. A igualdade absoluta “só resposta da porta, nenhum efeito indireto” também não está provada. Os callbacks e suas permissões de escrita precisam aparecer como ressalva herdada, sem transformar possibilidade em dano executado. (`app/chamados/services/materializacao.py:1444-1472`, `:1659-1664`; `app/chamados/services/validacao.py:662`, `:269-278`; `app/ponto/services/fechamento.py:1810`, `:1160`)

## 16/11 18:42 · sessao 37963ea76ea3 · - · - · origem=Ronald (tmux)

Cobertura da matriz confirmada no código completo; o helper renderiza modal/card e restringe a busca de Validar ao formulário cujo `pergunta_id` é o da pergunta examinada. (`app/chamados/tests/test_bug_par_volta.py:192-210`)

| Linha | Cobrança efetiva |
|---|---|
| L1 | `test_matriz_linha1_entregue_fora163_recusa_estado_proprio_esconde_validar`: estado próprio, detalhe K6, Validar ausente nos dois HTMLs, retorno da porta, nenhuma batida e fotografia de perguntas/atos/notas inalterada. (`app/chamados/tests/test_bug_par_volta.py:222-235`; fotografia em `:212-215`) |
| L2 | `test_matriz_linha2_fechada_nao_entregue_conserva_o_toque_e_o_motivo`: FechamentoMensal real `aprovado`; estado, detalhe, Validar nos dois HTMLs; `nao_gravou` e devolução; nenhuma batida/carimbo; `validada_em=None`; motivo, DEVOLUCAO e nota classificados COMPETENCIA_APROVADA; irmã sem validação/materialização/recusa/ato novo. (`app/chamados/tests/test_bug_par_volta.py:217-220`, `:237-273`) |
| L3 | `test_matriz_linha3_163_declarado_x_gravado_mostra_a_espera`: estado/espera/detalhe, Validar ausente nos dois HTMLs, porta em espera, nenhuma batida e fotografia inalterada. (`app/chamados/tests/test_bug_par_volta.py:275-287`) |
| L4 | `test_matriz_linha4_nada_barra_aplicavel_validar_grava`: aplicável, Validar nos dois HTMLs, porta valida o par, ambas recebem VALIDACAO; helper exige validação/carimbo/via declaração e as duas batidas E-S-E-S. (`app/chamados/tests/test_bug_par_volta.py:289-302`; `app/chamados/tests/test_bug_par_projetado.py:103-110`) |
| L5 | `test_matriz_linha5_fora163_sem_dia_do_fato_estado_proprio`: estado próprio e detalhe, Validar ausente nos dois HTMLs; porta igual; K6 não consultado; nenhuma batida e fotografia inalterada. (`app/chamados/tests/test_bug_par_volta.py:304-321`) |

Os cinco mutantes são precisamente os contratados: `c4_fundir_l1_em_l2`, `c4_motivo_l2_paridade`, `c4_desligar_portao_l3`, `c4_barrar_l4`, `c4_fundir_l5_em_l1`. (`app/chamados/tests/bug_par_mutantes.py:123-137`) Cada log individual fornecido termina em `rc=1`, `FAILED (failures=1)`, e exibe `AssertionError` no teste correspondente; nenhuma dessas cinco mortes foi por import/fixture. (`integ_c4b_todos.txt`, blocos dos respectivos `.log`, linhas `AssertionError`, `FAILED` e `rc=1`; referências exatas desses logs estão no texto do usuário)

O novo teste F4 existe e demonstra a passagem `(pendentes_aval,resolvidas,congelados)=(1,0,1)→(0,1,1)` chamando `validar_pergunta` com o admin, confirmando o par gravado e comparando `competencia_do_fato` antes/depois. (`app/chamados/tests/test_bug_par_volta.py:453-472`) A ressalva precisa: seu juiz de entrega é **mock constante False**, herdado de `_Col960Fixture`; ele não é alternado durante esse teste. (`app/chamados/tests/test_bug_par_projetado.py:46-53`) Logo, satisfaz “zera pela validação sem mudar a resposta do juiz”; execução do juiz real ou zero do universo de produção permanece [nao verificado]. O teste antigo zerava ao trocar o mock para True, diferença agora corrigida pelo novo contraexemplo. (`app/chamados/tests/test_bug_par_103.py:87-93`; `app/chamados/tests/test_bug_par_volta.py:461-472`)

Para L5, o código põe uma U103 carimbada sem dia em `bug_par_103_sem_dia` antes do ramo entregue; o comando imprime “sem dia do fato” tanto para U103 quanto para fora163. (`app/chamados/services/materializacao.py:330-336`; `app/chamados/management/commands/bug_par_103.py:58-63`, `:74-77`, `:137-140`) O teste complementar cobra as duas linhas falantes e nenhuma escrita da porta de devolução, mas não asserta diretamente o balde do contador. (`app/chamados/tests/test_bug_par_103.py:101-123`) A pergunta L5 da matriz está fora163, portanto nem participa do contador cuja consulta é exclusivamente por U163. (`app/chamados/tests/test_bug_par_volta.py:24-34`, `:304-321`; `app/chamados/services/materializacao.py:321-324`)

## 16/11 18:41 · sessao 5075b3e1d8b9 · UI-EDITOR-CAL-REPETIR-CONFIRMA · verificador · origem=Code

A análise abaixo é estática. Execução dos testes, mutantes efetivamente executados e correspondência da cópia com o hash informado: **[não verificado]**. O recorte adotado é o delta anexado; sua equivalência ao histórico Git: **[não verificado]**. [nao verificado]

**1) Quem escreve e quem lê**

| Valor | Escritor → leitor |
|---|---|
| `ESCAPE`, `FUNDO`, `resposta`, `_responde` | As constantes contêm JavaScript; os dois testes passam a constante ao helper, que a encaminha a `repete`. `_responde` preserva exatamente essas duas constantes; para as demais respostas, produz clique no elemento cujo ID recebeu. O resultado entra em `%(responde)s` e só executa quando o fundo está aberto. (`app/escala/tests/test_smoke_clique_folga_calendario.py:227`, `app/escala/tests/test_smoke_clique_folga_calendario.py:231`, `app/escala/tests/test_smoke_clique_folga_calendario.py:239`, `app/escala/tests/test_smoke_clique_folga_calendario.py:148`, `app/escala/tests/test_smoke_clique_folga_calendario.py:340`, `app/escala/tests/test_smoke_clique_folga_calendario.py:343`) |
| `SIM`, `NAO` | São IDs dos botões. `repete` assume `NAO`; o teste novo de fevereiro também o fornece explicitamente. `_responde` transforma esse ID em clique. (`app/escala/tests/test_smoke_clique_folga_calendario.py:189`, `app/escala/tests/test_smoke_clique_folga_calendario.py:232`, `app/escala/tests/test_smoke_clique_folga_calendario.py:235`, `app/escala/tests/test_smoke_clique_folga_calendario.py:348`) |
| `pagina`, `porta`, `args` | `_repete` resolve a rota, faz GET, exige HTTP 200 e entrega o HTML a `repete`; `_dom` injeta a sonda antes de `</body>` e abre o HTML no Chromium. Criar e editar apontam para a mesma view. (`app/escala/tests/test_smoke_clique_folga_calendario.py:312`, `app/escala/tests/test_smoke_clique_folga_calendario.py:192`, `app/escala/urls.py:35`) |
| `id`, `preenche`, `tam`, `comum`, `responde` | `repete` monta esses parâmetros: `id=VEREDITO`, `comum=COMUM`, e os demais vêm do chamador ou de `_responde`. A sonda usa `id` no elemento de resultado; executa `preenche`; usa `tam` para selecionar botão, campos, contador e grade; incorpora as funções de visibilidade e linhas de `COMUM`; executa `responde` após registrar o modal. (`app/escala/tests/test_smoke_clique_folga_calendario.py:239`, `app/escala/tests/test_smoke_clique_folga_calendario.py:126`, `app/escala/tests/test_smoke_clique_folga_calendario.py:142`, `app/escala/tests/test_smoke_clique_folga_calendario.py:144`, `app/escala/tests/test_smoke_clique_folga_calendario.py:105`, `app/escala/tests/test_smoke_clique_folga_calendario.py:153`) |
| `antes` | A sonda lê os valores dos inputs `dias_<tam>`, converte para número e ordena, depois de preencher e antes de clicar em Repetir. Fevereiro o afirma; os dois testes novos de saída não o afirmam. (`app/escala/tests/test_smoke_clique_folga_calendario.py:129`, `app/escala/tests/test_smoke_clique_folga_calendario.py:143`, `app/escala/tests/test_smoke_clique_folga_calendario.py:333`, `app/escala/tests/test_smoke_clique_folga_calendario.py:349`) |
| `logo` | A sonda lê os mesmos inputs imediatamente após o clique em Repetir. Fevereiro exige `[9]`; os dois testes de saída não o afirmam. (`app/escala/tests/test_smoke_clique_folga_calendario.py:144`, `app/escala/tests/test_smoke_clique_folga_calendario.py:146`, `app/escala/tests/test_smoke_clique_folga_calendario.py:333`, `app/escala/tests/test_smoke_clique_folga_calendario.py:352`) |
| `depois`, `grade`, `conta`, `aberto` | Após responder e esperar outro timer, a sonda captura: inputs ordenados; células visíveis com fundo azul; texto do contador; e existência do fundo sem `hidden`. Os testes de saída afirmam os quatro; fevereiro afirma `depois` e `grade`, mas deixa `conta` e `aberto` sem asserção. (`app/escala/tests/test_smoke_clique_folga_calendario.py:129`, `app/escala/tests/test_smoke_clique_folga_calendario.py:133`, `app/escala/tests/test_smoke_clique_folga_calendario.py:135`, `app/escala/tests/test_smoke_clique_folga_calendario.py:156`, `app/escala/tests/test_smoke_clique_folga_calendario.py:335`, `app/escala/tests/test_smoke_clique_folga_calendario.py:352`) |
| `modal`, `texto`, `linhas`, `sim`, `nao` | `modal` começa `null`; com fundo aberto, a sonda registra `textContent`, linhas obtidas por `innerText` com verificação de visibilidade, e rótulos dos botões. Os testes novos exigem presença do modal; fevereiro exige também “Fevereiro” no texto. Eles não afirmam linhas nem rótulos. (`app/escala/tests/test_smoke_clique_folga_calendario.py:143`, `app/escala/tests/test_smoke_clique_folga_calendario.py:148`, `app/escala/tests/test_smoke_clique_folga_calendario.py:114`, `app/escala/tests/test_smoke_clique_folga_calendario.py:335`, `app/escala/tests/test_smoke_clique_folga_calendario.py:350`) |
| Resultado da sonda | A sonda serializa `r` no elemento `HX-CLIQUE`. Chromium entrega o DOM em `stdout`; `repete` extrai, desfaz escape HTML e desserializa o JSON. `_repete` exige `dict` antes de devolver aos testes. (`app/escala/tests/test_smoke_clique_folga_calendario.py:48`, `app/escala/tests/test_smoke_clique_folga_calendario.py:157`, `app/escala/tests/test_smoke_clique_folga_calendario.py:197`, `app/escala/tests/test_smoke_clique_folga_calendario.py:241`, `app/escala/tests/test_smoke_clique_folga_calendario.py:316`) |

A cadeia dos valores reais da página é esta:

- **Seed → seleção:** a view lê os tamanhos persistidos e fornece `marcados`, tamanho e rótulo; o template serializa esses dados e inicializa `sel[tamanho]` como `Set`. Os cliques da sonda chamam os handlers das células, que alteram essa seleção. (`app/escala/views.py:362`, `app/escala/views.py:369`, `app/templates/escala/folga_calendario_editar.html:74`, `app/templates/escala/folga_calendario_editar.html:78`, `app/templates/escala/folga_calendario_editar.html:89`, `app/templates/escala/folga_calendario_editar.html:119`)
- **Seleção → testemunhas:** `renderGrade` escreve células e contador; `syncHidden` escreve os inputs `dias_<tam>`. São esses elementos que a sonda lê, não uma cópia própria da regra de repetição. (`app/templates/escala/folga_calendario_editar.html:85`, `app/templates/escala/folga_calendario_editar.html:95`, `app/templates/escala/folga_calendario_editar.html:110`, `app/escala/tests/test_smoke_clique_folga_calendario.py:129`, `app/escala/tests/test_smoke_clique_folga_calendario.py:135`)
- **Repetir → diálogo → seleção:** `fcRepetir` copia imediatamente quando o alvo está vazio; caso contrário, fornece texto e verbo a `hxConfirmar` e só copia com resposta verdadeira. A cópia retém os dias da grade de 31 que cabem no alvo. (`app/templates/escala/folga_calendario_editar.html:124`)
- **Diálogo:** `hxConfirmar` escreve texto, verbo, visibilidade e promessa pendente; `fechar` oculta o fundo e resolve essa promessa. Sim resolve verdadeiro; Cancelar, fundo direto e Escape resolvem falso. O fundo só fecha quando `e.target === fundo`. (`app/templates/core/_dialogo.html:149`, `app/templates/core/_dialogo.html:157`, `app/templates/core/_dialogo.html:172`, `app/templates/core/_dialogo.html:188`)
- **Inclusão:** o editor estende `base.html`, que inclui o componente da casa. (`app/templates/escala/folga_calendario_editar.html:1`, `app/templates/base.html:388`)

As fixtures relevantes são:

| Fixture | Escritor → leitor |
|---|---|
| `admin` | `setUpTestData` cria usuário staff/superuser; `setUp` o usa em `force_login`, antes do GET protegido por `editar_escalas`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:255`, `app/escala/tests/test_smoke_clique_folga_calendario.py:265`, `app/escala/views.py:314`) |
| `com_trinta` | A fixture grava grade 31 com `[3,10]` e grade 30 com `[8]`. Os dois testes de saída passam seu PK; a view lê esses dados para o seed. `dia(20)` acrescenta 20 à origem de 31, tornando a substituição indevida observável como `[3,10,20]`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:259`, `app/escala/tests/test_smoke_clique_folga_calendario.py:334`, `app/escala/views.py:319`, `app/escala/views.py:362`, `app/templates/escala/folga_calendario_editar.html:127`) |
| Fevereiro | O teste usa criação sem calendário existente e constrói o estado pelos cliques: origem `[5,30]`, alvo 28 `[9]`; repetir produziria `[5]`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:348`, `app/escala/views.py:319`, `app/templates/escala/folga_calendario_editar.html:127`) |
| `existente`, `sistema` | São preparados pelo mesmo `setUpTestData`, mas os três testes novos não os referenciam diretamente. Leitores indiretos de `sistema`: **[não verificado]**. (`app/escala/tests/test_smoke_clique_folga_calendario.py:254`, `app/escala/tests/test_smoke_clique_folga_calendario.py:256`, `app/escala/tests/test_smoke_clique_folga_calendario.py:333`) |

Os três testes novos param na observação do editor: não chamam `_reenvia` nem exercitam a gravação pela porta. Essa gravação permanece na view e em `salvar_calendario_de_folga`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:333`, `app/escala/tests/test_smoke_clique_folga_calendario.py:275`, `app/escala/views.py:354`, `app/escala/folgas.py:571`)

**2) Mutantes por teste — previsão pela leitura, não execução** [nao verificado]

| Teste novo | Derruba | Passaria | [nao verificado]
|---|---|---|
| `test_MORDE_Escape_vale_Cancelar_no_repetir` | Escape resolvendo verdadeiro: substituiria `[8]` por `[3,10,20]`. Escape sem chegar ao listener: deixaria `aberto=true`. Guarda somente para 28: faltaria modal no alvo 30. (`app/escala/tests/test_smoke_clique_folga_calendario.py:334`, `app/escala/tests/test_smoke_clique_folga_calendario.py:335`, `app/escala/tests/test_smoke_clique_folga_calendario.py:340`, `app/templates/escala/folga_calendario_editar.html:127`) | Guarda somente para 30; fundo resolvendo verdadeiro; clique interno fechando. O teste responde exclusivamente com Escape no alvo 30. (`app/escala/tests/test_smoke_clique_folga_calendario.py:227`, `app/escala/tests/test_smoke_clique_folga_calendario.py:334`, `app/escala/tests/test_smoke_clique_folga_calendario.py:340`) |
| `test_MORDE_clique_fora_da_caixa_vale_Cancelar_no_repetir` | Fundo resolvendo verdadeiro: mesma substituição indevida. Fundo sem efeito: falha no fechamento. Guarda somente para 28: faltaria modal. (`app/escala/tests/test_smoke_clique_folga_calendario.py:228`, `app/escala/tests/test_smoke_clique_folga_calendario.py:334`, `app/escala/tests/test_smoke_clique_folga_calendario.py:335`, `app/escala/tests/test_smoke_clique_folga_calendario.py:343`) | Guarda somente para 30; Escape resolvendo verdadeiro; clique interno fechando. O estímulo é clique direto no fundo. (`app/escala/tests/test_smoke_clique_folga_calendario.py:228`, `app/escala/tests/test_smoke_clique_folga_calendario.py:334`, `app/escala/tests/test_smoke_clique_folga_calendario.py:343`) |
| `test_MORDE_repetir_sobre_fevereiro_preenchido_tambem_pergunta` | Guarda somente para 30; Cancelar resolvendo verdadeiro; cópia antes da resposta. Exige modal, `antes=[9]` e `logo/depois/grade=[9]`, enquanto a cópia seria `[5]`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:348`, `app/templates/escala/folga_calendario_editar.html:127`) | Guarda somente para 28; Escape verdadeiro; fundo verdadeiro; clique interno fechando; Cancelar que preserve os dias mas deixe modal aberto ou contador errado. Não estimula Escape/fundo nem afirma `aberto/conta`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:348`, `app/escala/tests/test_smoke_clique_folga_calendario.py:352`) |

**Em conjunto, os novos testes cobrem a guarda nos dois tamanhos**, mas Escape e fundo apenas no tamanho 30. (`app/escala/tests/test_smoke_clique_folga_calendario.py:334`, `app/escala/tests/test_smoke_clique_folga_calendario.py:348`)

O mutante que remove `if(e.target === fundo)` **sobrevive aos três**. Nenhum clica caixa ou texto; nos cliques dos botões, o handler já resolve e limpa `pendente`, fazendo a chamada posterior por borbulhamento retornar sem efeito. (`app/templates/core/_dialogo.html:149`, `app/templates/core/_dialogo.html:157`, `app/escala/tests/test_smoke_clique_folga_calendario.py:153`, `app/escala/tests/test_smoke_clique_folga_calendario.py:227`)

**Não há desencontro de alvo do Escape nesta cópia:** a sonda despacha em `document` e o componente escuta `document`. Se o evento fosse despachado num alvo não ouvido, o teste reprovaria por modal aberto. A simulação, porém, não percorre o caminho de um evento originado no botão focado; um listener que aceitasse apenas `target === document` passaria por ela. (`app/escala/tests/test_smoke_clique_folga_calendario.py:227`, `app/escala/tests/test_smoke_clique_folga_calendario.py:336`, `app/templates/core/_dialogo.html:160`, `app/templates/core/_dialogo.html:195`)

**3) Relógio, propagação e mordida**

- **Não usam relógio físico solto:** há dois timers fixos de 200 ms, mas Chromium recebe orçamento de tempo virtual de 4000 ms; os 120 segundos são o timeout externo do processo. Continua sendo uma janela fixa de observação: mudança posterior à captura pode escapar. (`app/escala/tests/test_smoke_clique_folga_calendario.py:147`, `app/escala/tests/test_smoke_clique_folga_calendario.py:155`, `app/escala/tests/test_smoke_clique_folga_calendario.py:199`)
- **Não há `subTest` nesses testes:** os métodos chamam diretamente o helper, cujas asserções propagam normalmente. (`app/escala/tests/test_smoke_clique_folga_calendario.py:333`)
- **As asserções principais mordem:** os estados preservados diferem dos estados que uma confirmação indevida produziria; ausência de execução da sonda também não aprova, pois `_repete` exige `dict`. As lacunas são específicas: fevereiro não afirma fechamento/contador; Escape e fundo não afirmam `antes/logo`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:260`, `app/escala/tests/test_smoke_clique_folga_calendario.py:316`, `app/escala/tests/test_smoke_clique_folga_calendario.py:333`, `app/escala/tests/test_smoke_clique_folga_calendario.py:348`)
- `_dom` não afirma `returncode` nem erros de console; portanto resultado válido da sonda não equivale a processo sem erros. (`app/escala/tests/test_smoke_clique_folga_calendario.py:197`, `app/escala/tests/test_smoke_clique_folga_calendario.py:241`)

**4) Prosa do Repetir contra L7**

**O estado atual não cumpre L7:** a explicação “Repetir troca esses dias…” e o verbo “Substituir” estão escritos no template. Usar `hxConfirmar` atende ao componente, mas não à exigência de origem no mesmo mapa usado pelo juiz. (`app/templates/escala/folga_calendario_editar.html:131`, `app/docs/LEIS-UI.md:77`)

**O escopo permite corrigir essa confirmação local.** Quem decide se deve perguntar e quando executar a cópia já é `fcRepetir`, na UI. Uma fonte compatível seria um módulo canônico em `static/` com o mapa da ação — decisão de confirmar, mensagem e verbo — consumido pela própria decisão e pela apresentação. Isso é proposta de correção; implementação existente dessa fonte: **[não verificado]**. Apenas transportar a frase para outro arquivo, mantendo uma decisão independente, não satisfaz o compartilhamento exigido pela lei. (`app/templates/escala/folga_calendario_editar.html:124`, `app/docs/LEIS-UI.md:78`)

A view fornece rótulos e dados das grades, mas seu contexto não fornece a explicação do Repetir; `folgas.py` grava o calendário e sincroniza seus efeitos, enquanto a confirmação examinada antecede essa gravação e pertence ao rascunho da tela. Não é necessário mudar essas funções para centralizar a decisão local com sua mensagem. (`app/escala/views.py:318`, `app/escala/views.py:363`, `app/escala/folgas.py:571`, `app/templates/escala/folga_calendario_editar.html:124`)

**VEREDITO: DEVOLVE -- a prosa do Repetir continua hardcoded fora do mapa exigido pela L7; adicionalmente, o mutante de clique interno fechando permanece sem cobertura nos testes novos.** (`app/docs/LEIS-UI.md:78`, `app/templates/escala/folga_calendario_editar.html:131`, `app/templates/core/_dialogo.html:159`, `app/escala/tests/test_smoke_clique_folga_calendario.py:333`)

_6160 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
