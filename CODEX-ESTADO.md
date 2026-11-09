# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 00:35** · publicado em **09/11/2026 00:36** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 00:35 · sessao 50955c5d0d17 · FIO-E0-V4-E1-FILA · sonda · origem=Code

1. Transcrição dos números fornecidos; verificação independente em produção **[nao verificado]**:

```text
S143 P com destino= 38403
S143 destino de outro colab (vs disputa.colaborador) total= 0 alcancavel_pelo_S143= 0
S143 destino de outro colab (vs P.colaborador) total= 0 alcancavel_pelo_S143= 0
REATAR orfas_abertas= 5525
REATAR orfa com chamado_destino terminal= 1060
REATAR orfa com Chamado terminal do colab no dia (alcancavel pelo HIST/renascer)= 1851
```

2. **A equivalência dos filtros da sonda com o código é [nao verificado]**: foi fornecida a saída, sem o corpo de `sonda_v22_acervo.py`; sua implementação não foi localizada na cópia **[nao verificado]**.

Os filtros efetivos do código são:

- **S143, seleção de perguntas:** destino igual ao chamado, `materializada_em IS NULL` e disputa aberta. Não exige igualdade entre os colaboradores do destino, da disputa e da Pergunta; também não exige `validada_em IS NULL` nem data presente. Depois, pula pergunta em voo, viva, de dia trancado ou respondida sem materialização de fato (`app/chamados/services/disputa_emissao.py:1724`, `1738`, `1744`, `1748`, `1757`). O receiver exige chamado em `ENCERRADOS` e gravação do status (`app/chamados/signals.py:96`, `104`).

- **REATAR, seleção do chamador:** destino **nulo**, colaborador da disputa, disputa aberta, materialização nula e motivo entre os quatro do grupo DIA. Não exige validação nula nem data presente (`app/chamados/reconciliador.py:1496`; `app/chamados/models.py:3690`). Portanto, não se pode equiparar automaticamente esse universo às 5525 “órfãs” da saída: o mesmo dado inclui 1060 “órfãs” com destino terminal **[nao verificado]**.

- **HIST:** só é tentado quando a derivação não encontra destino. Usa `p.data_evento`, com fallback para a data do contexto; busca mesmo colaborador, `ENCERRADOS`, igualdade de `contexto_json.data_turno` e categoria diferente de `disputa_supervisao`. Ainda filtra motivo, desvia a viva para c2 dentro do alcance novo e exige exatamente um candidato histórico (`app/chamados/reconciliador.py:1552`, `1563`, `1582`, `1589`, `1600`, `1603`, `1619`). **HIST não chama o renascimento:** reaponta e chama o resolvedor S128 (`app/chamados/reconciliador.py:1638`, `1644`).

- **c2/reemitir:** chega após HIST vazio e recusa de c1. Aplica gates de motivo, data, colaborador ativo, horizonte, isenção, ausência, protesto, parcial só lista, célula, régua e motor; para intervalos, exige também viva em competência aberta. Abre pela chave colaborador + `batida_ausente` + `data_turno`; só costura se o resultado estiver vivo (`app/chamados/reconciliador.py:1615`, `808`, `820`, `835`, `850`, `855`, `869`, `875`, `886`, `900`, `906`, `908`, `910`, `936`).

A condição isolada **“há terminal do colaborador no dia” é mais larga que a elegibilidade efetiva de HIST/c2**, pelos filtros acima. Se a sonda aplica esses filtros adicionais ou restringe indevidamente validação/data/motivos, é **[nao verificado]**. Também há diferença de vocabulário: `TERMINAIS` inclui cancelado/superado; HIST e o dedup que tenta renascer usam `ENCERRADOS`, apenas resolvido/fechado (`app/chamados/catalogo/motor.py:30`; `app/chamados/reconciliador.py:1583`; `app/chamados/models.py:921`).

3. **Existe caminho de Pergunta até tentativa de renascimento fora de HIST/c2; afirmar que a sonda não o conta é [nao verificado].**

Um caminho concreto é:

- Materialização da Pergunta registra Batida (`app/chamados/services/materializacao.py:926`; `app/ponto/registro_batida.py:157`).
- O signal da Batida chama o cartório (`app/ponto/signals.py:131`, `93`).
- Persistindo furo parcial, com aplicação, sem chamado vivo e sem bloqueio de “parcial só lista”, o cartório chama o emissor (`app/ponto/services/cartorio.py:1767`, `1777`).
- O emissor chama `abrir` por colaborador, módulo e dia, sem filtro de Pergunta nessa chave. Encontrando chamado encerrado, o dedup tenta `_renascer_por_premissa_viva`, sujeito às guardas e à aceitação de `reabrir` (`app/ponto/management/commands/detectar_ausencias.py:693`; `app/chamados/models.py:906`, `921`, `1005`, `1019`).

Esse caminho existe no código; sua ocorrência, quantidade e exclusão dos **1851** informados são **[nao verificado]**.

## 09/11 00:35 · sessao 7bc8cd84a86b · - · - · origem=Ronald (tmux)

Há dois pontos produtivos que chamam `_renascer_por_premissa_viva`: o dedup de `ChamadoColaborador.abrir`, ao encontrar `ENCERRADOS` (`app/chamados/models.py:906`, `app/chamados/models.py:921`), e a resposta tardia no fio, se a célula acusa furo (`app/chamados/services/resposta_tardia.py:35`, `app/chamados/services/resposta_tardia.py:45`).

Caminho concreto a partir de Pergunta, adicional ao HIST/c2:

1. Materialização da resposta hora usa a Pergunta e registra Batida (`app/chamados/services/materializacao.py:794`, `app/chamados/services/materializacao.py:926`).
2. A porta cria Batida (`app/ponto/registro_batida.py:157`); o `post_save` julga os dias (`app/ponto/signals.py:131`, `app/ponto/signals.py:93`).
3. O cartório entrega o emissor a `julgar_colab` (`app/ponto/services/cartorio.py:1964`, `app/ponto/services/cartorio.py:1989`).
4. Persistindo furo parcial, sem chamado vivo, com `apply_` e sem bloqueio `parcial_so_lista`, o cartório chama `_abrir_chamado` (`app/ponto/services/cartorio.py:1767`, `app/ponto/services/cartorio.py:1777`).
5. O emissor chama `ChamadoColaborador.abrir` por colaborador, módulo e `data_turno`, sem filtro de Pergunta nem status nessa chave (`app/ponto/management/commands/detectar_ausencias.py:693`); o dedup pode encontrar um encerrado e tentar renascê-lo (`app/chamados/models.py:906`, `app/chamados/models.py:921`).

A tentativa só efetivamente renasce passando pelas guardas: colaborador ativo, sem encerramento por cascata, competência não trancada, ausência de veto de régua e aceitação por `reabrir` (`app/chamados/models.py:1005`, `app/chamados/models.py:1019`).

A resposta tardia também tenta renascer sem selecionar Pergunta ou órfã; a view busca diretamente o Chamado e chama o serviço (`app/chamados/views.py:1046`, `app/chamados/views.py:1053`). Esse caminho é resposta no fio, não resposta direta de `PerguntaDisputa` (`app/chamados/views.py:1041`).

**Não é possível afirmar que o item (2) da sonda deixa esses caminhos fora da contagem sem a fonte do script** [nao verificado]. O próprio dado fornecido admite “órfã com chamado_destino terminal”; portanto, “órfã” não permite inferir `chamado_destino IS NULL` nesse script [nao verificado].

`_ressuscitar_por_resposta` não é prova de renascimento do status: o corpo apenas levanta o silêncio do destino ou pai com `arquivo.levantar` (`app/chamados/services/materializacao.py:711`, `app/chamados/services/materializacao.py:718`).

## 09/11 00:19 · sessao b599f9f2e362 · FIO-E0-V4-E1-FILA · verificador · origem=Code

A cura produtiva está coerente com a normalização da trava; a cura dos testes ficou **parcial na mensagem e na cobertura da conferência**. A mensagem atual imprime as três parcelas corretamente, mas os testes só exigem os IDs da disputa. (`app/chamados/services/fio.py:2092`; `app/chamados/services/fio.py:2098`; `app/chamados/tests/test_e1_fila.py:507`; `app/chamados/tests/test_e1_fila.py:519`)

Execução da suíte, conteúdo de `e1_fila_red5.log`, estado do banco e ocorrência em produção: **[nao verificado]**. [nao verificado]

**1. Triagem: o que procede e o que ficou parcial**

| Item da triagem | Verificação estática |
|---|---|
| `None` comparado, mas não travável | **Curado como alinhamento de conjuntos:** `_pks` ignora `None`; `_fora[2]` agora também. Porém, `PerguntaDisputa.disputa` é FK sem `null=True`: o teste fabrica a segunda leitura, sem gravar NULL. Não demonstra uma ocorrência produtiva. (`app/chamados/models.py:2661`; `app/chamados/services/fio.py:2093`; `app/chamados/models.py:2424`; `app/chamados/tests/test_e1_fila.py:509`) |
| Resultado preenchido sem exigir migração | **Curado:** exige `([p.pk], [], [])` e relê o destino gravado. O MORDE sem laço exige lista vazia e destino antigo. (`app/chamados/tests/test_e1_fila.py:547`; `app/chamados/tests/test_e1_fila.py:555`) |
| Diferença trocada por XOR | **Curado para o encolhimento exercitado:** a segunda leitura vazia deve passar. XOR produziria parcelas antigas e recusaria. (`app/chamados/tests/test_e1_fila.py:521`; `app/chamados/services/fio.py:2092`) |
| Cardinalidade no lugar do conjunto de disputas | **Curado:** substitui a disputa mantendo a pergunta e exige recusa. (`app/chamados/tests/test_e1_fila.py:509`; `app/chamados/tests/test_e1_fila.py:513`) |
| Mensagem sem IDs | **Parcial:** exige somente `disputas [1000000000]`; apagar os IDs de colaboradores ou perguntas da mensagem conserva os asserts existentes. (`app/chamados/tests/test_e1_fila.py:494`; `app/chamados/tests/test_e1_fila.py:507`; `app/chamados/tests/test_e1_fila.py:519`) |
| Duas leituras só conferidas sem exceção | **Curado:** a asserção fica depois do `except`, nos dois ramos. (`app/chamados/tests/test_e1_fila.py:482`) |
| Remoção de `key=str` | **Coerente:** a disputa referencia um `BigAutoField`; `None` sai antes da ordenação. (`app/chamados/migrations/0013_disputasupervisao.py:20`; `app/chamados/services/fio.py:2093`; `app/chamados/services/fio.py:2098`) |

Os dois itens de FILA pertencem à investigação desta obra: são campos usados para escolher as travas, e o escopo inclui seus escritores e leitores. Isso não os transforma em defeitos concorrentes demonstrados; falta um escritor produtivo que altere esses FKs depois do nascimento. (`CLAUDE.md:376`; `app/chamados/services/fio.py:2085`; `app/chamados/models.py:3420`; alcance produtivo **[nao verificado]**)

**2. Quem escreve, quem lê e o que efetivamente se compara**

O conjunto nasce das perguntas apontadas para velho/novo, acrescido de `universo_da_linha` de cada uma. A mesma função é chamada antes e depois da trava. (`app/chamados/services/fio.py:2032`; `app/chamados/services/fio.py:2084`; `app/chamados/services/fio.py:2088`)

| Campo/valor | Escritor | Leitores relevantes ao ato |
|---|---|---|
| `Pergunta.colaborador_id` | No nascimento, `_chave_de_identidade` deriva da disputa ou recusa divergência. Há também o preenchimento histórico da migration 0051. (`app/chamados/models.py:3430`; `app/chamados/migrations/0051_decisao_pergunta.py:25`) | Composição de `_cs`, conferência, trava de Colaborador e juiz da costura. (`app/chamados/services/fio.py:2085`; `app/chamados/services/fio.py:2092`; `app/chamados/models.py:2674`; `app/chamados/juizes.py:2003`) |
| `Pergunta.pk` | Identidade automática `BigAutoField`; a porta insere com `force_insert=True`. (`app/chamados/migrations/0014_perguntadisputa.py:19`; `app/chamados/models.py:3019`) | Lista para travar Perguntas, conferência e composição dos grupos do laço. (`app/chamados/services/fio.py:2087`; `app/chamados/services/fio.py:2092`; `app/chamados/services/fio.py:2143`) |
| `Pergunta.disputa_id` | A disputa entra no construtor usado pelo nascimento; depois ocorre a inserção. (`app/chamados/models.py:3016`; `app/chamados/models.py:3019`) | Lista para travar Disputas, conferência e portas internas de retratação. (`app/chamados/services/fio.py:2086`; `app/chamados/services/fio.py:2093`; `app/chamados/services/fio.py:1330`) |
| Donos de velho/novo | O nascimento do Chamado recebe `colaborador`; `criar` o persiste. (`app/chamados/models.py:1054`; `app/chamados/models.py:1095`) | Entram em `_cs` pelas instâncias recebidas; a conferência não relê esses donos. (`app/chamados/services/fio.py:2085`; `app/chamados/services/fio.py:2092`) |
| Mensagem de `UniversoMudouSobATrava` | Montada com velho/novo e as três parcelas ordenadas. (`app/chamados/services/fio.py:2095`) | `reabrir` captura, registra `str(_e)[:300]` em `depois['erro']` e retorna `False`; o helper de teste captura a string inteira. (`app/chamados/models.py:1417`; `app/chamados/models.py:1424`; `app/chamados/tests/test_e1_fila.py:489`) |

O evento é persistido pela porta `core.observ.evento`, que escreve `descricao` e `valor_depois` em `LogAuditoria`; portanto o registro produtivo pode conter somente os primeiros 300 caracteres do erro. (`app/core/observ.py:94`; `app/core/observ.py:134`; `app/chamados/models.py:1424`)

Se `P0` é a primeira leitura e `P1` a segunda, a relação é esta:

| Classe | IDs pedidos à trava | Parcela que provoca recusa |
|---|---|---|
| Colaborador | `C0 = {dono velho, dono novo} ∪ colaboradores(P0)` | `colaboradores(P1) − C0` |
| Pergunta | `pk(P0)` | `pk(P1) − pk(P0)` |
| Disputa | `disputas(P0) − {None}` após normalização | `(disputas(P1) − disputas(P0)) − {None}` |
| Chamado | `{velho.pk, novo.pk}` | Não há parcela de conferência para Chamado. |

Essas expressões vêm da chamada e da formação de `_fora`; a normalização está em `_pks`. (`app/chamados/services/fio.py:2085`; `app/chamados/services/fio.py:2086`; `app/chamados/services/fio.py:2092`; `app/chamados/models.py:2661`)

Logo:

- **A conferência exige cobertura, não igualdade:** encolher é permitido; linhas da primeira leitura podem continuar travadas mesmo ausentes na segunda. (`app/chamados/services/fio.py:2092`; `app/chamados/tests/test_e1_fila.py:521`)
- **Para `disputa_id` inteiro, não há filtro divergente entre os lados:** `_pks` não descarta zero ou negativos; descarta `None`, assim como a conferência. (`app/chamados/models.py:2661`; `app/chamados/services/fio.py:2093`)
- **Não há valor `None` comparado como nova disputa faltante na v5.** O inverso possível é uma disputa antiga continuar travada após desaparecer da segunda leitura, comportamento deliberadamente aceito. (`app/chamados/services/fio.py:2093`; `app/chamados/tests/test_e1_fila.py:529`)
- **IDs calculados não são comprovantes de linhas adquiridas:** as queries travam linhas existentes, e a migração não usa o retorno da trava para conferir aquisições. Integridade efetiva do banco: **[nao verificado]**. (`app/chamados/models.py:2674`; `app/chamados/models.py:2683`; `app/chamados/models.py:2702`; `app/chamados/services/fio.py:2086`)

A ordem implementada é Colaborador → Disputa → Chamado → Pergunta, por PK em cada classe. (`app/chamados/models.py:2674`; `app/chamados/models.py:2683`; `app/chamados/models.py:2696`; `app/chamados/models.py:2700`)

**3. Mordida dos testes e mutantes residuais** [nao verificado]

Os testes novos distinguem estaticamente retirar `-{None}`, usar XOR, comparar cardinalidade das disputas e suprimir o laço. O instrumento de mutação recompila a função real e exige uma única ocorrência da âncora. (`app/chamados/tests/test_e1_fila.py:48`; `app/chamados/tests/test_e1_fila.py:513`; `app/chamados/tests/test_e1_fila.py:521`; `app/chamados/tests/test_e1_fila.py:526`; `app/chamados/tests/test_e1_fila.py:555`)

Persistem estes mutantes plausíveis **pelos oráculos inspecionados**; sobrevivência executada na suíte inteira: **[nao verificado]**. [nao verificado]

| Mutante | Por que os asserts atuais não o distinguem |
|---|---|
| Na mensagem, trocar `sorted(_fora[0])` ou `sorted(_fora[1])` por `[]` | Colaborador/Pergunta exigem apenas mensagem verdadeira; só Disputa tem conteúdo exigido. (`app/chamados/services/fio.py:2098`; `app/chamados/tests/test_e1_fila.py:507`; `app/chamados/tests/test_e1_fila.py:519`) |
| Imprimir todas as disputas de `_sob`, em vez de somente as externas | No caso que verifica texto, a única disputa foi substituída; o caso que acrescenta disputa só exige recusa. (`app/chamados/tests/test_e1_fila.py:502`; `app/chamados/tests/test_e1_fila.py:509`; `app/chamados/tests/test_e1_fila.py:519`) |
| Trocar a condição `_fora[1]` por crescimento da cardinalidade dos PKs | Há acréscimo de PK e esvaziamento, mas não substituição de PK mantendo cardinalidade. (`app/chamados/services/fio.py:2094`; `app/chamados/tests/test_e1_fila.py:498`; `app/chamados/tests/test_e1_fila.py:523`) |
| Comparar apenas cardinalidades dos colaboradores de `P1` e `P0` | O caso exclusivo de colaborador acrescenta um valor; não substitui o conjunto mantendo tamanho. (`app/chamados/tests/test_e1_fila.py:500`; `app/chamados/tests/test_e1_fila.py:509`) |
| Executar o laço imediatamente antes do `if _fora` | Os casos de corrida usam costura cruzada, já recusada pela porta; depois a exceção continua ocorrendo. O cenário efetivamente migrável só é usado sem corrida. (`app/chamados/tests/test_e1_fila.py:147`; `app/chamados/tests/test_e1_fila.py:547`; `app/chamados/tests/test_e1_fila.py:563`; `app/chamados/services/fio.py:863`) |

O teste “nada migra” comprova recusa e destino final nesse cenário; **não comprova que uma migração possível foi impedida ou desfeita**. A segunda leitura é simulada, enquanto o laço volta a consultar o banco real. (`app/chamados/tests/test_e1_fila.py:474`; `app/chamados/tests/test_e1_fila.py:563`; `app/chamados/services/fio.py:2118`; `app/chamados/services/fio.py:2122`)

**4. Escritores produtivos dos dois itens de FILA**

**Não identifiquei escritor produtivo que troque esses FKs após o nascimento; não há prova universal de ausência — alcance concorrente [nao verificado].**

- **Dono do Chamado:** sua fotografia realmente não é relida para `_cs`. A atribuição aparente em `pergunta_no_app` apenas compartilha a instância do mesmo colaborador dentro de grupos formados pelo próprio ID. A porta dinâmica de texto aceita somente título, assunto e descrição. (`app/chamados/services/fio.py:2085`; `app/chamados/services/pergunta_no_app.py:133`; `app/chamados/services/pergunta_no_app.py:139`; `app/chamados/services/texto.py:18`; `app/chamados/services/texto.py:25`)
- **Colaborador/disputa da Pergunta:** a possibilidade programática existe: depois de `_state.adding`, `save` não repete a guarda da identidade. Isso prova permissividade da API, não um escritor que efetivamente altere os campos. (`app/chamados/models.py:3420`; `app/chamados/models.py:3423`; `app/chamados/models.py:3430`)
- **Escritor histórico encontrado:** a migration 0051 preenche `colaborador_id` por `Subquery`, via `RunPython`; não é um caminho da execução normal da migração de resolvedoras. (`app/chamados/migrations/0051_decisao_pergunta.py:25`; `app/chamados/migrations/0051_decisao_pergunta.py:101`)
- **Porta ampla encontrada:** `descarimbar_em_lote(**outros)` admite outros campos, mas os chamadores rastreados passam resposta/validação. O outro lote também usa dicionário fixo desses campos. (`app/chamados/models.py:3252`; `app/chamados/models.py:3266`; `app/chamados/models.py:1565`; `app/chamados/services/disputa_emissao.py:1517`; `app/chamados/services/disputa_emissao.py:1554`; `app/chamados/services/disputa_emissao.py:1556`)
- **Escritas deste ato:** reapontamento grava destino/lavra, eventualmente dia; retratação grava carimbo e campos associados. Não alteram os três FKs investigados. (`app/chamados/services/fio.py:866`; `app/chamados/services/fio.py:879`; `app/chamados/services/fio.py:922`; `app/chamados/models.py:3128`; `app/chamados/models.py:3152`)

Portanto, os dois FILA sustentam **riscos latentes da interface**, mas não sustentam, com a evidência disponível, uma corrida produtiva bloqueante. (`app/chamados/models.py:3420`; `app/chamados/services/fio.py:2085`; corrida produtiva **[nao verificado]**)

**5. Classes que já reprovaram obra**

| Classe | Resultado nesta verificação |
|---|---|
| Segundo juiz duplicando registrado | **Não nas linhas tocadas:** a guarda decide cobertura de travas; vitalidade continua delegada a `pergunta_viva`, registrada como autoridade. (`app/chamados/services/fio.py:2026`; `app/chamados/services/fio.py:2113`; `app/core/juizes.py:266`) |
| Segundo escritor fora da porta | **Não nas linhas tocadas:** o diff modifica conjuntos/mensagem; o laço continua usando reapontamento e retratação pelas portas existentes. (`app/chamados/services/fio.py:2092`; `app/chamados/services/fio.py:2165`; `app/chamados/services/fio.py:2171`) |
| Relógio fora da borda | **Não no delta v5:** a conferência não lê relógio. No caminho histórico de expiração, `_hoje` usa `timezone.now` ou o instante recebido. (`app/chamados/services/fio.py:2092`; `app/chamados/veredito_lavrado.py:95`) |
| Derivação paralela de fonte única | **Não na construção do universo:** as duas leituras usam `_conjunto_da_migracao`; esta e o laço usam `universo_da_linha`. (`app/chamados/services/fio.py:2032`; `app/chamados/services/fio.py:2084`; `app/chamados/services/fio.py:2088`; `app/chamados/services/fio.py:2142`) |
| Selo/RED sem mordida | **Sim, residual de cobertura:** IDs de duas classes, substituição de PK e precedência da conferência ainda têm os escapes descritos acima. (`app/chamados/tests/test_e1_fila.py:507`; `app/chamados/tests/test_e1_fila.py:519`; `app/chamados/tests/test_e1_fila.py:563`) |
| Ordem de trava | **Não há inversão demonstrada neste ato:** uma chamada adquire as quatro classes na ordem; crescimento provoca exceção antes do laço. (`app/chamados/services/fio.py:2086`; `app/chamados/services/fio.py:2094`; `app/chamados/models.py:2674`; `app/chamados/models.py:2700`) |
| Pouso antes do pré-requisito | **[nao verificado]**. A regra exige suíte verde antes do opus e opus antes do pouso; os anexos não comprovam essa sequência. (`CLAUDE.md:374`; `CLAUDE.md:378`) | [nao verificado]
| Campo com dois significados | **Não introduzido pelo delta:** `disputa_id` continua sendo FK de Disputa; `None` é ignorado na normalização. A igualdade colaborador da Pergunta/disputa é verificada apenas no nascimento; divergência produtiva posterior **[nao verificado]**. (`app/chamados/models.py:2424`; `app/chamados/models.py:2661`; `app/chamados/models.py:3420`; `app/chamados/models.py:3433`) |

## 09/11 00:17 · sessao aad97d481b14 · - · - · origem=Ronald (tmux)

Não localizei escritor de produção que **mude**, após o nascimento, `ChamadoColaborador.colaborador_id`, `PerguntaDisputa.colaborador_id` ou `PerguntaDisputa.disputa_id`. O alcance concorrente dos dois itens de FILA permanece **[nao verificado]**, sem escritor concreto demonstrado.

Evidências e exclusões:

- **Nascimento da Pergunta:** `disputa` entra no construtor e a inserção ocorre com `force_insert=True` (`app/chamados/models.py:3016`, `app/chamados/models.py:3019`); `colaborador_id` é derivado da disputa ou validado contra ela (`app/chamados/models.py:3430`, `app/chamados/models.py:3432`, `app/chamados/models.py:3433`).
- **Mutabilidade latente existe:** `save()` só chama a guarda da identidade quando `_state.adding` (`app/chamados/models.py:3420`); depois repassa ao `super().save()` (`app/chamados/models.py:3423`). Isso permite escrita programática futura, mas não demonstra um chamador produtivo que a faça.
- **Escritor histórico:** a migração preenche `colaborador_id` por `Subquery` (`app/chamados/migrations/0051_decisao_pergunta.py:25`), registrada em `RunPython` (`app/chamados/migrations/0051_decisao_pergunta.py:101`). Não é escritor da execução normal de `migrar_resolvedoras_vivas`.
- **A atribuição aparente `colaborador_id` no fluxo de abertura é proxy:** o objeto é `_Proxy`, não modelo persistido (`app/chamados/models.py:2888`, `app/chamados/models.py:2890`, `app/chamados/models.py:2894`).
- **A atribuição aparente de dono do Chamado não troca o ID:** os chamados são agrupados pelo próprio `colaborador_id`, e a mesma instância é compartilhada no grupo, sem save (`app/chamados/services/pergunta_no_app.py:133`, `app/chamados/services/pergunta_no_app.py:139`, `app/chamados/services/pergunta_no_app.py:141`).
- **`descarimbar_em_lote(**outros)` seria uma porta ampla:** aceita outros campos e escreve por `update` (`app/chamados/models.py:3252`, `app/chamados/models.py:3266`), mas os chamadores encontrados passam apenas resposta/validação (`app/chamados/models.py:1565`; `app/chamados/services/disputa_emissao.py:1517`, `app/chamados/services/disputa_emissao.py:1554`).
- **Outros lotes:** `reabrir_para_correcao` escreve o dicionário fixo de resposta/validação (`app/chamados/services/disputa_emissao.py:1517`, `app/chamados/services/disputa_emissao.py:1556`); `bulk_update` escreve somente a lavra (`app/chamados/veredito_lavrado.py:217`, `app/chamados/veredito_lavrado.py:230`).
- **Migração:** o reapontamento salva destino/lavra, eventualmente o dia (`app/chamados/services/fio.py:866`, `app/chamados/services/fio.py:879`, `app/chamados/services/fio.py:922`, `app/chamados/services/fio.py:924`); a retratação salva carimbo, resposta/validação e lavra (`app/chamados/models.py:3128`, `app/chamados/models.py:3133`, `app/chamados/models.py:3137`, `app/chamados/models.py:3151`, `app/chamados/models.py:3152`).
- **`setattr` do texto do Chamado:** restrito a `titulo`, `assunto`, `descricao`, com recusa dos demais campos (`app/chamados/services/texto.py:18`, `app/chamados/services/texto.py:25`, `app/chamados/services/texto.py:27`, `app/chamados/services/texto.py:33`).
- **`setattr` genérico de reexibição:** só altera memória, sem salvar (`app/core/reexibir.py:21`, `app/core/reexibir.py:45`, `app/core/reexibir.py:50`); seus chamadores são de colaboradores (`app/colaboradores/views.py:1094`, `app/colaboradores/views.py:1259`).
- **Admin:** `app/chamados/admin.py` está vazio; a ausência também é documentada (`app/core/contratos_estruturais.py:331`). Não encontrei registro desses modelos nos outros admins nem ModelForm/ModelSerializer que os edite **[nao verificado: ausência universal]**. O `ready()` do app só importa signals (`app/chamados/apps.py:8`, `app/chamados/apps.py:10`).

Rodei o censo AST puro em memória, sem Django nem gravação de arquivo. Ele confirma os sítios acima, mas não fornece prova formal de ausência: o próprio censo admite operações não atribuíveis e inferências (`app/core/censo_escritas.py:38`, `app/core/censo_escritas.py:43`, `app/core/censo_escritas.py:49`).

## 09/11 00:17 · sessao 579d17b8fa80 · - · - · origem=Ronald (tmux)

Análise estática; execução e sobrevivência dinâmica dos mutantes: [nao verificado].

- **Curas efetivas:** a segunda leitura ocorre também no ramo com recusa; o helper exige exatamente duas chamadas após capturar a exceção (`app/chamados/tests/test_e1_fila.py:474`, `app/chamados/tests/test_e1_fila.py:489`, `app/chamados/tests/test_e1_fila.py:491`).
- **None:** retirar `- {None}` de `fio.py:2093` volta a recusar a troca Disputa→None e é detectado pelo teste novo (`app/chamados/tests/test_e1_fila.py:526`).
- **XOR:** substituir a diferença por diferença simétrica recusa o conjunto vazio e é detectado (`app/chamados/tests/test_e1_fila.py:521`; cálculo em `app/chamados/services/fio.py:2092`).
- **Cardinalidade da Disputa:** comparar somente tamanhos perde a substituição mantendo uma linha; o novo caso altera a disputa da pergunta existente e exige recusa (`app/chamados/tests/test_e1_fila.py:509`, `app/chamados/tests/test_e1_fila.py:517`).
- **Migração efetiva:** agora exige simultaneamente `migradas=[p.pk]`, demais saídas vazias e FK gravado no destino; remover o laço deixa o FK antigo e a lista vazia (`app/chamados/tests/test_e1_fila.py:539`, `app/chamados/tests/test_e1_fila.py:551`, `app/chamados/tests/test_e1_fila.py:555`).
- **MORDE:** recompila a função real com uma substituição cujo trecho deve aparecer exatamente uma vez; não usa uma função fictícia (`app/chamados/tests/test_e1_fila.py:48`; extração em `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:331`).

Lacunas e mutantes sobreviventes por inspeção:

1. **Mensagem parcialmente coberta.** A única exigência sobre seu conteúdo é a substring `disputas [1000000000]`; os casos exclusivos de Colaborador e Pergunta só exigem mensagem verdadeira. Trocar `sorted(_fora[0])` ou `sorted(_fora[1])` por `[]`, trocar os IDs velho/novo do prefixo, ou ordenar a disputa em ordem inversa conserva os asserts atuais (`app/chamados/tests/test_e1_fila.py:494`, `app/chamados/tests/test_e1_fila.py:507`, `app/chamados/tests/test_e1_fila.py:519`; mensagem em `app/chamados/services/fio.py:2096`).

2. **Mensagem com conjunto maior também escapa.** Substituir o último argumento por `sorted({x.disputa_id for x in _sob} - {None})` imprime todas as disputas presentes, inclusive as já travadas. No único teste que verifica texto, a única disputa foi substituída, portanto o conjunto maior coincide com o esperado; o caso de acréscimo não examina texto (`app/chamados/tests/test_e1_fila.py:502`, `app/chamados/tests/test_e1_fila.py:509`, `app/chamados/tests/test_e1_fila.py:519`; argumento em `app/chamados/services/fio.py:2098`).

3. **Cardinalidade de Pergunta continua sem substituição equivalente.** Trocar a condição `_fora[1]` por `len({x.pk for x in _sob}) > len({x.pk for x in _ps})` conserva os casos presentes: eles acrescentam PK ou esvaziam o conjunto; a troca conservando cardinalidade cobre somente Disputa (`app/chamados/tests/test_e1_fila.py:498`, `app/chamados/tests/test_e1_fila.py:509`, `app/chamados/tests/test_e1_fila.py:523`; guarda em `app/chamados/services/fio.py:2094`).

4. **Colaborador requer cuidado.** O mutante `len(C_sob)>len(_cs)` é detectado: `_cs` já contém os dois donos, e acrescentar o colaborador sintético faz 2 contra 2. Já comparar cardinalidades dos colaboradores das duas leituras de Pergunta, `len(C_sob)>len(C_ps)`, conserva os casos atuais porque não existe substituição exclusiva de colaborador mantendo cardinalidade (`app/chamados/tests/test_e1_fila.py:147`, `app/chamados/tests/test_e1_fila.py:500`; formação de `_cs` em `app/chamados/services/fio.py:2085`).

5. **Precedência da conferência não é exigida.** Mover `_migrar_sob_trava` para imediatamente antes do `if _fora` conserva as verificações atuais: as corridas usam a pergunta de outro colaborador, recusada pela porta antes de gravar, e depois a exceção continua ocorrendo. O teste “nada migra” verifica apenas o destino final desse cenário já recusável; o cenário que efetivamente migra é exercitado sem corrida (`app/chamados/tests/test_e1_fila.py:147`, `app/chamados/tests/test_e1_fila.py:547`, `app/chamados/tests/test_e1_fila.py:563`; recusa anterior à escrita em `app/chamados/services/fio.py:863`; ordem atual em `app/chamados/services/fio.py:2094`).

6. **Corrida simulada, sem escritor concorrente.** O helper modifica somente o retorno da segunda chamada; o laço consulta novamente o banco real. Portanto esses testes demonstram a condição de recusa, mas não demonstram concorrência real, aquisição das travas ou que o mesmo universo continue estável depois da conferência (`app/chamados/tests/test_e1_fila.py:474`, `app/chamados/tests/test_e1_fila.py:486`; releituras em `app/chamados/services/fio.py:2118`, `app/chamados/services/fio.py:2122`).

## 09/11 00:16 · sessao 13dc7743181f · - · - · origem=Ronald (tmux)

Para a pergunta 5, convém separar “violação demonstrada” de “ausência universal”: o escopo inclui escritores e leitores dos campos, mesmo fora do diff, conforme CLAUDE.md:376-377.

| Classe | Resultado estático no caminho auditado |
|---|---|
| 2º juiz duplicando registrado | **Não demonstrado.** A nova guarda verifica cobertura de travas, explicitamente operacional em `app/chamados/services/fio.py:2026-2029`; vitalidade vem de `pergunta_viva` (`:2113-2124`), registrado em `app/core/juizes.py:266`; identidade/canônica continuam em `universo_da_linha`/`canonica_do_grupo` (`fio.py:2142-2149`). |
| 2º escritor fora da porta | **Não introduzido pelas linhas produtivas do v5.** A mudança está em diferença de conjuntos/mensagem (`fio.py:2092-2098`); escrita continua via `reapontar_resolvedora` e `retratar_para_canonica` (`:2165-2171`). A primeira grava destino e lavra em `:922-925`; a segunda delega à retratação em `:1327-1332`, que usa `carimbar_identidade` em `:1350-1353`. Ausência em toda a árvore depende do censo do agente escritores [nao verificado]. |
| Relógio fora da borda | **Não nas linhas v5** (`fio.py:2092-2098`). No escopo histórico FILA, `_hoje` usa `timezone.localdate(agora or timezone.now())` em `app/chamados/veredito_lavrado.py:95-102`; `expira_em` usa `_hoje(agora)` em `:137`; testes distinguem relógio da borda/date.today e duas leituras em `app/chamados/tests/test_e1_fila.py:349-385`. Execução [nao verificado]. |
| Derivação paralela de fonte única | **Não demonstrada na conferência.** Leituras pré/sob trava usam a mesma `_conjunto_da_migracao`, que chama `universo_da_linha` (`fio.py:2032-2039`, `:2084-2088`); o laço chama a mesma função (`:2142`). `universo_da_linha` e `universo_do_grupo` usam `fecho_do_fato` (`:1896-1911`). |
| Selo/RED sem mordida | **Há limite residual demonstrável.** `test_RED_a_porta_recusa_e_nada_migra` conserva `_cenario` cruzado e só observa destino velho (`test_e1_fila.py:563-571`); `_cenario` põe Pergunta do primeiro colaborador em chamados do segundo (`:147-155`), e `reapontar_resolvedora` já recusaria a costura cruzada (`fio.py:862-865`). Portanto esse teste não demonstra rollback de uma migração que ocorreria. O novo sucesso real exige destino novo e saída exata (`test_e1_fila.py:539-553`), curando o antigo “algum resultado”, mas não transformando aquele teste de rollback em cenário migrável. | [nao verificado]
| Ordem de trava | **Não há inversão nova demonstrada.** Uma chamada passa todos C/D/Ch/P (`fio.py:2084-2087`); implementação adquire C, D, Ch, P com ordenação por PK (`app/chamados/models.py:2673-2701`); conferência recusa antes do laço (`fio.py:2092-2099`). Travas internas ficam cobertas se os conjuntos conferidos correspondem às linhas que efetivamente serão usadas; mutabilidade/callers exigem resultado do agente escritores [nao verificado]. |
| Pouso antes do pré-requisito | **[nao verificado].** Regra exige suite verde antes do opus e proíbe Codex aprovar pouso sozinho (`CLAUDE.md:374-378`). Cabeçalho declara aval e marco histórico (`test_e1_fila.py:1-10`), mas isso não prova execução nem sequência de pouso [nao verificado]. | [nao verificado]
| Campo com dois significados | **Não introduzido na mudança v5.** `disputa_id` permanece identificador de Disputa; `None` representa ausência de linha travável, normalizada na guarda (`fio.py:2090-2093`) e em `_pks` (`models.py:2660-2661`). O risco de divergência entre dono direto e dono da Disputa é pré-existente: igualdade só é aplicada no nascimento (`models.py:3420-3435`); alcance por escritor produtivo depende do agente escritores [nao verificado]. | [nao verificado]

Triagem: a retirada de `None` da parcela de Disputa está alinhada à trava, pois `_pks` ignora `None` (`models.py:2660-2661`) e a conferência agora também (`fio.py:2093`). A dispensa de `key=str` é consistente com os IDs inteiros presentes no modelo/caminho, mas valores reais de produção não constam da sonda [nao verificado]. A triagem não traz item denominado `DESCARTE`; traz dois itens `FILA` [nao verificado: texto do anexo sem arquivo:linha].

Observação para consolidar: não classificar genericamente como “fora desta obra” apenas por não estar no diff, pois `CLAUDE.md:376-377` inclui explicitamente escritores e leitores. A relevância concreta dos dois FILA depende de escritor produtivo alcançável, que esta subtarefa não censou [nao verificado].

## 08/11 23:57 · sessao 667030d3457f · FIO-E0-V4-E1-FILA · verificador · origem=Code

**A volta 4 corrige a omissão de Disputa na conferência, mas b2 ficou parcialmente curado: o teste novo exige resultado preenchido, sem exigir migração efetiva. A equivalência literal entre IDs comparados e IDs traváveis também falha para `None`.** (`app/chamados/services/fio.py:2091`; `app/chamados/tests/test_e1_fila.py:511`; `app/chamados/models.py:2660`)

Execução da suíte, concorrência real e incidência em produção: **[nao verificado]**. [nao verificado]

1. **Triagem: descartes, filas e curas**

| Item | Verificação |
|---|---|
| **a — FILA dos chamadores que ignoram `False`** | **Procedente como defeito pendente.** Adesão incrementa sempre; reconciliador devolve `None` após ignorar a recusa e seu chamador conta `CREATE`; ação administrativa ignora o retorno e anuncia sucesso; view atribui qualquer recusa a irmão vivo. (`app/chamados/services/adesao.py:92`; `app/ponto/management/commands/reconciliar_grade.py:529`; `app/ponto/management/commands/reconciliar_grade.py:254`; `app/chamados/services/acoes_disputa.py:267`; `app/chamados/services/acoes_disputa.py:296`; `app/chamados/views_arquivar.py:70`) A justificativa histórica “anteriores à obra”: **[nao verificado]**. |
| **b1 — `or` → `and`** | **Curado no alvo.** Há crescimento isolado de Pergunta, Colaborador e Disputa, além do mutante explícito. (`app/chamados/tests/test_e1_fila.py:489`; `app/chamados/tests/test_e1_fila.py:504`) |
| **b2 — supressão do laço** | **Cura parcial.** O par novo distingue a supressão literal, mas aceita só `recusadas`. Seu cenário põe uma Pergunta do primeiro colaborador em chamados do segundo; o reapontamento recusa esse cruzamento antes da escrita. Logo “o laço produziu resultado” foi coberto; “migração efetiva sob trava” não foi exigida pelo novo RED. (`app/chamados/tests/test_e1_fila.py:147`; `app/chamados/tests/test_e1_fila.py:511`; `app/chamados/tests/test_e1_fila.py:517`; `app/chamados/services/fio.py:863`) |
| **b3 — captura ampliada para `RuntimeError`** | **Curado.** O teste exige propagação da exceção genérica; a captura produtiva continua específica. (`app/chamados/tests/test_e1_fila.py:566`; `app/chamados/models.py:1417`) |
| **b4 — modelo/objeto do evento** | **Curado.** Agora exige exatamente `('ChamadoColaborador', velho.pk)`. (`app/chamados/tests/test_e1_fila.py:564`) |
| **b5 — DESCARTE do retorno de `aplicar_declaracao`** | **Procedente para a promessa de trava.** O RED exige canônica, destino e ordem; a proposta é selecionada antes da chamada de trava. Isso não certifica sucesso funcional completo da aplicação. (`app/chamados/tests/test_e1_fila.py:442`; `app/chamados/services/declaracao_texto.py:643`) |
| **b6 — trava para só um motivo** | **DESCARTE procedente na implementação atual.** O helper recebe motivo/dia e consulta o universo correspondente, sem restringir motivo; ambas as portas o chamam após selecionar o fato. (`app/chamados/services/declaracao_texto.py:282`; `app/chamados/services/declaracao_texto.py:346`; `app/chamados/services/declaracao_texto.py:650`) |
| **c — mudança de `disputa_id`** | **Curado para entrada de ID novo não nulo.** A diferença de disputas participa da recusa e ganhou caso isolado; resta a assimetria de `None` descrita abaixo. (`app/chamados/services/fio.py:2091`; `app/chamados/tests/test_e1_fila.py:497`) |
| **d — relógio e derivação da janela** | **FILA tecnicamente procedente.** O juiz ainda usa `date.today()`, enquanto `expira_em` reproduz a condição da janela em implementação própria. (`app/chamados/juizes.py:575`; `app/chamados/juizes.py:601`; `app/chamados/veredito_lavrado.py:120`; `app/chamados/veredito_lavrado.py:136`) O registro prévio na fila citada: **[nao verificado]**. |

2. **Quem escreve, quem lê e o universo conferido**

| Campo/valor | Escritor comprovado | Leitor relevante |
|---|---|---|
| Pergunta `pk` | `BigAutoField`; nascimento insere com `save(force_insert=True)`. (`app/chamados/migrations/0014_perguntadisputa.py:19`; `app/chamados/models.py:3019`) | Lista pedida à trava e diferença de Perguntas. (`app/chamados/services/fio.py:2087`; `app/chamados/services/fio.py:2091`) |
| Pergunta `colaborador_id` | `_chave_de_identidade` deriva da Disputa e rejeita valor divergente; migration preenche o acervo. (`app/chamados/models.py:3430`; `app/chamados/migrations/0051_decisao_pergunta.py:25`) | `_cs`, conferência e consulta da identidade do fato. (`app/chamados/services/fio.py:2085`; `app/chamados/services/fio.py:2091`; `app/chamados/models.py:2762`) |
| Pergunta `disputa_id` | Nascimento recebe `disputa` e insere a Pergunta. (`app/chamados/models.py:3016`) | Pedido à trava, diferença nova e trava interna da retratação. (`app/chamados/services/fio.py:2086`; `app/chamados/services/fio.py:2092`; `app/chamados/services/fio.py:1329`) |
| Donos de `velho`/`novo` | A migração recebe as instâncias e lê seus `colaborador_id`; não os escreve. (`app/chamados/services/fio.py:2042`; `app/chamados/services/fio.py:2085`) | Entram em `_cs`, mesmo sem Pergunta daquele dono. (`app/chamados/services/fio.py:2085`) |
| `_conjunto_da_migracao` | É lista calculada: Perguntas destinadas a velho/novo, somadas ao universo de cada uma; pode conter repetição. (`app/chamados/services/fio.py:2037`) | Duas leituras: cálculo inicial e conferência após a trava. (`app/chamados/services/fio.py:2084`; `app/chamados/services/fio.py:2088`) |

**Quem altera a pertinência ao conjunto:** nascimento e reapontamento/reuso escrevem `chamado_destino`; rechaveamento altera dia/célula; carimbo altera a via que a consulta do fato exclui. A expansão delega a `universo_da_linha`/`fecho_do_fato`. (`app/chamados/models.py:3016`; `app/chamados/models.py:2971`; `app/chamados/services/fio.py:922`; `app/chamados/services/fio.py:1028`; `app/chamados/models.py:3182`; `app/chamados/models.py:2732`; `app/chamados/services/fio.py:1906`)

**O pedido desta chamada é exatamente:**

| Classe | IDs solicitados |
|---|---|
| Colaborador | Donos de velho/novo ∪ colaboradores das `_ps`. (`app/chamados/services/fio.py:2085`) |
| Disputa | `disputa_id` das `_ps`. (`app/chamados/services/fio.py:2086`) |
| Chamado | Somente `velho.pk` e `novo.pk`. (`app/chamados/services/fio.py:2086`) |
| Pergunta | `pk` das `_ps`. (`app/chamados/services/fio.py:2087`) |

`trava_na_ordem` elimina repetidos e `None`, consulta linhas existentes e executa **C → D → Ch → P**, cada classe por PK crescente. Nesta chamada, os pais das Disputas não são acrescentados automaticamente: falta a flag `abertas_do_colaborador`. (`app/chamados/models.py:2660`; `app/chamados/models.py:2674`; `app/chamados/models.py:2683`; `app/chamados/models.py:2687`; `app/chamados/models.py:2696`; `app/chamados/models.py:2700`; `app/chamados/services/fio.py:2086`)

**Para IDs existentes e não nulos, a conferência cobre corretamente inclusão por classe.** Encolhimento e troca de associações entre IDs já travados podem passar: não exigem outra linha. Exigir igualdade dos conjuntos recusaria desnecessariamente esses casos. (`app/chamados/services/fio.py:2091`)

**Não há uma quarta coleção dinâmica de PKs de Chamado omitida:** os dois PKs vêm dos argumentos e permanecem fixos. Entretanto, seus donos são lidos das instâncias recebidas e não são relidos sob trava; uma troca persistida de dono não seria conferida aqui. Escritor produtivo dessa troca e sua alcançabilidade: **[nao verificado]**. (`app/chamados/services/fio.py:2085`; `app/chamados/services/fio.py:2088`)

A imutabilidade de `Pergunta.colaborador_id`/`disputa_id` após nascimento também não é garantida pelo `save`: a validação da identidade só roda quando `adding`; depois ele delega ao ORM. Escrita indireta que explore isso: **[nao verificado]**. (`app/chamados/models.py:3420`)

3. **Mordida e mutantes sobreviventes**

Os casos isolados mordem a ausência de cada parcela da guarda, mas simulam mudança **acrescentando** um `SimpleNamespace`; nos casos de colaborador/disputa, acrescentam outro objeto com PK já existente, em vez de substituir a versão anterior. Não demonstram corrida entre transações. (`app/chamados/tests/test_e1_fila.py:473`; `app/chamados/tests/test_e1_fila.py:495`)

O helper `_recusa_com` retorna imediatamente na exceção: sua asserção de duas leituras só executa quando não houve recusa. O teste separado de recusa original, porém, exige as duas leituras. (`app/chamados/tests/test_e1_fila.py:484`; `app/chamados/tests/test_e1_fila.py:529`)

Os seguintes mutantes não são distinguidos pelos formatos dos novos casos; sobrevivência na **suíte inteira**: **[nao verificado]**. [nao verificado]

| Mutante | Falha que escapa |
|---|---|
| Diferença de disputas `A - B` → `A ^ B` | Recusa também encolhimento; os casos acrescentam uma linha e não exercitam remoção. (`app/chamados/services/fio.py:2092`; `app/chamados/tests/test_e1_fila.py:476`) |
| Parcela de disputas → `{len(_sob)} - {len(_ps)}` | Passa no crescimento artificial, mas perde troca real de disputa mantendo o número de Perguntas. (`app/chamados/services/fio.py:2092`; `app/chamados/tests/test_e1_fila.py:473`) |
| Remover `key=str` | A mensagem pode explodir com diferença contendo simultaneamente `None` e inteiro; nenhum caso novo produz essa diferença mista. (`app/chamados/services/fio.py:2097`; `app/chamados/tests/test_e1_fila.py:469`; `app/chamados/tests/test_e1_fila.py:492`) |
| Imprimir a parcela errada na mensagem | Os testes conferem ocorrência da exceção, sem exigir os IDs de disputas no texto. (`app/chamados/services/fio.py:2097`; `app/chamados/tests/test_e1_fila.py:502`; `app/chamados/tests/test_e1_fila.py:527`) |

A fragilidade de b2 é dos testes novos: já existe teste antigo que exige `migradas`, relê a Pergunta e verifica destino persistido. (`app/chamados/tests/test_e1_v15g_portas.py:364`)

4. **`disputa_id=None`**

**`None` nunca é travado como Disputa.** `_pks` o remove; com `disputas=[None]`, sem universo adicional, nem há consulta de Disputa. A diferença nova, entretanto, conserva `None` como elemento comparável. (`app/chamados/models.py:2660`; `app/chamados/models.py:2682`; `app/chamados/services/fio.py:2092`)

| Disputas antes → depois | Resultado da guarda |
|---|---|
| `{7}` → `{None}` | Recusa por `{None}`, embora nenhuma Disputa nova precise de trava. (`app/chamados/services/fio.py:2092`; `app/chamados/models.py:2661`) |
| `{7, None}` → `{None}` | Aceita: `None` já estava no conjunto inicial. (`app/chamados/services/fio.py:2092`) |
| `{None}` → `{8}` | Recusa corretamente por `{8}`. (`app/chamados/services/fio.py:2092`) |
| `{7, 8}` → `{8}` | Aceita corretamente o encolhimento. (`app/chamados/services/fio.py:2092`) |

Portanto, **comparado e travável não são literalmente equivalentes**. Contudo, `Pergunta.disputa` é FK obrigatória tanto no modelo quanto na migration de criação. `None` persistido no esquema real de produção: **[nao verificado]**; nos testes, ele é explicitamente sintético. (`app/chamados/models.py:2424`; `app/chamados/migrations/0014_perguntadisputa.py:27`; `app/chamados/tests/test_e1_fila.py:469`)

5. **Classes de reprovação**

| Classe | Resposta com escopo |
|---|---|
| **Segundo juiz duplicando registrado** | **Não nas linhas novas:** conferem cobertura de IDs; o laço continua consultando `pergunta_viva`, autoridade C6 registrada. (`app/chamados/services/fio.py:2091`; `app/chamados/services/fio.py:2123`; `app/core/juizes.py:266`) |
| **Segundo escritor fora da porta** | **Não acrescentado pela V4:** a escrita permanece depois da conferência, pelo laço existente. Ausência global de qualquer escritor indireto: **[nao verificado]**. (`app/chamados/services/fio.py:2098`) |
| **Relógio fora da borda** | **Sim no contexto**, no `date.today()` do juiz; a lavra usa `_hoje` com `timezone.now`. (`app/chamados/juizes.py:575`; `app/chamados/veredito_lavrado.py:102`) |
| **Derivação paralela de fonte única** | **Sim no contexto:** `expira_em` reproduz a condição da janela do juiz. (`app/chamados/veredito_lavrado.py:120`; `app/chamados/veredito_lavrado.py:141`; `app/chamados/juizes.py:601`) |
| **Selo/RED sem mordida** | **Parcial em b2:** morde supressão literal, mas não exige migração efetiva. (`app/chamados/tests/test_e1_fila.py:511`; `app/chamados/tests/test_e1_fila.py:517`) |
| **Ordem de trava** | **Não há inversão acrescentada pela V4:** mantém pedido único no topo e recusa antes do laço quando falta ID. Ordem global sob concorrência real: **[nao verificado]**. (`app/chamados/services/fio.py:2086`; `app/chamados/services/fio.py:2093`) |
| **Pouso antes do pré-requisito** | **Não no miolo novo:** conferência precede laço; aprovação do destino precede retratação. Pouso/deploy e cumprimento dos pré-requisitos externos: **[nao verificado]**. (`app/chamados/services/fio.py:2098`; `app/chamados/services/fio.py:2164`; `app/chamados/services/fio.py:2170`) |
| **Campo com dois significados** | **Sim, declarado no contexto:** `data_evento` representa dia do turno para fato e dia civil para conversa; há discriminador e tratamento explícitos. A V4 não altera esse campo. (`app/chamados/models.py:2820`; `app/chamados/juizes.py:2006`; `app/chamados/services/fio.py:2091`) |

## 08/11 23:57 · sessao e1c9c1085810 · BUG-DEPLOY-PORTAO-ANTES-DO-MIGRATE · verificador · origem=Code

**REPROVA para o contrato absoluto informado.** A volta 4 restaura a recusa exigida para cron vivo, mas conserva um portão depois do migrate; o caso 11 agora exige justamente banco alterado e restart recusado (`bin/deploy.sh:301,326`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:163–165`; `CLAUDE.md:90`). Autorização para excepcionar esse caminho do contrato: [nao verificado].

A análise abaixo é estática. Execução dos 13 casos, RED dos mutantes e ocorrência desses caminhos em produção: [nao verificado].

**1. Triagem da volta 3**

| Item | Veredito nesta obra |
|---|---|
| **PROCEDENTE 1** | Curou a violação da lei do restart: a recusa voltou a ser incondicional. Não curou simultaneamente o contrato de portões antes do migrate (`bin/deploy.sh:294–301,323–327`; `CLAUDE.md:90`). |
| **PROCEDENTE 2** | “UM leitor” agora corresponde ao único teste do valor de `APLICAR_MIGRATE`. “UM escritor” exige distinguir a inicialização da armação; literalmente há duas atribuições (`bin/deploy.sh:59–61,223,299`). |
| **PROCEDENTE 3** | Curado estaticamente: `PUB_OK` compara apply com a primeira publicação e é exigido nos casos 3 e 13 (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:110–114,132,172`). RED efetivo: [nao verificado]. |
| **PROCEDENTE 4** | **Mal curado:** o caso 12 cria cron antes de iniciar o deploy; a guarda inicial já recusa. Remover somente a rechecagem imediatamente anterior ao apply continua passando esse cenário (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:97–98,167–169`; `bin/deploy.sh:115–118,300`). |
| **PROCEDENTE 5** | A dependência de PID fixo foi retirada: as marcas usam `$$` do selo, exportado para o falso migrate (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:75,96–97`). |
| **NÃO PROCEDE 6** | Defensável **na localização declarada**: a configuração agenda deploy e envelope sob `/home/ronald/saas-hasner`. Não é equivalência universal: o envelope escreve em endereço absoluto e o deploy lê relativamente à própria localização (`app/config/crons.py:19,326,1475,1486`; `bin/cron_run.sh:9`; `bin/deploy.sh:53,108`). Caminhos efetivos em produção: [nao verificado]. |
| **FILA 7** | **Pertence à promessa desta obra:** a 1c afirma que mensageria já respondeu. Hoje sua guarda precede o apply, mas o falso deste selo sempre responde sucesso para ela; o vizinho testa recusa por mensageria, sem exigir banco core intocado (`bin/deploy.sh:194–203,292,301`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`; `bin/tests/test_deploy_reload_gracioso.sh:150–156`). |
| **FILA 8** | **Pertence à promessa:** faltam cenários verdes de auth com pendência em agendado/`--sem-migrate`. O caso 4 para antes, por auth; o caso 10 não tem pendência. Remover as recusas desses modos sobreviveria aos 13 por inspeção (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:135–137,159–161`; `bin/deploy.sh:211–218`). |
| **FILA 9** | Lacuna de cobertura, sem defeito atual demonstrado: `--reload-copia` passa pela mesma chamada de auth, mas não aparece nos 13 cenários (`bin/deploy.sh:82,126,145`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:123–173`). |
| **DESCARTE 10** | **Improcedente como descarte.** Consultar novamente pode obter outra resposta e encerrar o deploy após o apply. Além disso, a consulta também apaga marcas consideradas mortas (`bin/deploy.sh:92,108–113,301,326`). |
| **FILA 11** | O contador realmente não prova os alvos, mas o vizinho exige a linha exata do restart das três cascas. Não é bloqueio adicional demonstrado aqui (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:109,172`; `bin/tests/test_deploy_reload_gracioso.sh:134–139`). |
| **DESCARTE 12** | **Justificativa exagerada:** o vizinho verifica troca de workers e sobrevivência do exec, mas simula reciclagem verde, rotas 200 e sucesso dos demais exec. Não demonstra mordida de falhas de readiness/rotas/reciclagem (`bin/tests/test_deploy_reload_gracioso.sh:40–41,57–58,107–112`). Provas de resultado da publicação continuam distintas de pré-requisitos verificáveis antes dela (`bin/deploy.sh:344–357,365–403`). |
| **DESCARTE 13** | **Procede para reabertura sob o contrato literal.** Reciclagem vermelha encerra o deploy depois de migrate/publicação; verifica inclusive configuração e crontab, condições que podem já estar erradas antes do apply (`bin/deploy.sh:301,327,344,410`; `bin/selo_reciclagem.sh:45–48,53–78`). |
| **PERGUNTA 14** | Continua material: as consultas não estabelecem exclusão com o nascimento de cron. Restaurar a última consulta preserva a recusa observada, mas não fecha a corrida (`bin/deploy.sh:108–117,300,326–327`; `bin/cron_run.sh:9–11`). |

**2. Escritores e leitores**

| Campo/protocolo | Quem escreve | Quem lê |
|---|---|---|
| `APLICAR_MIGRATE` | Deploy inicializa `0` e arma `1` quando há pendência core em modo elegível (`bin/deploy.sh:61,204–223`). O selo injeta `1` herdado no caso 10, sobrescrito pela inicialização (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:98,159`). | Único leitor operacional: decisão de executar apply (`bin/deploy.sh:299`). |
| `RESTART` | Deploy inicializa `0`; somente `MODO=--restart` escreve `1`, exigindo motivo (`bin/deploy.sh:70–75`). | Guarda inicial; exceção da pendência de mensageria; rechecagem pré-apply; escolha restart/HUP; mensagem final (`bin/deploy.sh:115,200,300,323,429`). Fora dele, o contrato lê o **texto** do ramo (`bin/contrato_deploy.sh:41–47`). |
| Marcas `logs/crons_em_curso` | Envelope cria diretório/nome `N_E.$$`, grava timestamp e remove no `EXIT` (`bin/cron_run.sh:9–10`). **Deploy também modifica o conjunto**, apagando marcas sem PID existente (`bin/deploy.sh:108–109`). | Deploy enumera marcas, extrai PID do sufixo, consulta `/proc`, devolve nomes vivos e decide a recusa (`bin/deploy.sh:108–113`). A mesma função é chamada em três pontos (`bin/deploy.sh:117,300,326`). |

O deploy **não lê o timestamp gravado dentro da marca**: seu protocolo efetivo usa existência do arquivo, nome e PID do sufixo (`bin/cron_run.sh:10`; `bin/deploy.sh:108–109`). O contrato externo apenas procura os textos dos caminhos, sem consumir marcas (`bin/contrato_deploy.sh:81–82`).

Os escritores de marcas dos testes são o falso migrate e a preparação do cron anterior, além do selo vizinho com PID de seu exec falso (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:75,96–97`; `bin/tests/test_deploy_reload_gracioso.sh:90–91`). Os elos da corrente também entram pelo envelope, depois de esperar carimbos de fim (`bin/encadeado.sh:89,102–104`).

Portanto, o comentário sobre `APLICAR_MIGRATE` bate quanto ao **leitor único** e ao **único ponto que arma a decisão**; não descreve literalmente uma única atribuição. A consulta das marcas tampouco é somente leitura (`bin/deploy.sh:59–61,109,223,299`).

**3. Mordida dos casos 1–13**

A tabela indica mutantes detectáveis pelas asserções, por inspeção; execução desses mutantes: [nao verificado].

| Caso | Mutante que detecta |
|---|---|
| **1** | Auth removida, ignorada ou posterior ao apply: exige recusa sem migrate/HUP (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:123–125`). |
| **2** | Sombra removida, ignorada ou posterior ao apply (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:127–129`). |
| **3** | Apply ausente/duplicado; auth, sombra `--conferir` ou casca ausentes/depois dele; publicação antes do apply; HUP ausente/duplicado (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:103–117,131–133`). |
| **4** | Agendado deixa de **chamar** auth. Não isola respeito ao retorno: a pendência também poderia recusar (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:135–137`). |
| **5** | `--conferir` aplica/publica, consulta auth/sombra ou deixa de terminar com sucesso (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:139–141`). |
| **6** | Prova de casca removida, ignorada ou posterior ao apply (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:143–145`). |
| **7** | `--sem-sombra` também dispensa auth ou migra antes dela (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:147–149`). |
| **8** | Apply incondicional sem pendência, ou ausência de HUP nesse caminho (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:151–153`). |
| **9** | Agendado chama auth, mas ignora a recusa; sem pendência para mascarar o erro (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:155–157`). |
| **10** | Retirada da inicialização permite `APLICAR_MIGRATE=1` herdado aplicar no `--sem-migrate` (`bin/deploy.sh:61`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:98,159–161`). |
| **11** | Retirada da recusa final ou retorno à condição da volta 3, permitindo restart sobre cron nascido no apply (`bin/deploy.sh:326`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:75,163–165`). |
| **12** | Retirada das barreiras anteriores ao apply para cron **já vivo no início**. Não distingue qual delas respondeu (`bin/deploy.sh:117,300`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:97,167–169`). |
| **13** | Apply ausente/duplicado; restart ausente/duplicado; restart antes do apply (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:110–114,171–173`). |

**Sobreviventes relevantes:**

- **Na linha efetivamente alterada `bin/deploy.sh:326`:** substituir a chamada por `[ "$APLICAR_MIGRATE" != 1 ] || recusa_restart_com_cron`. Preserva a recusa com apply, satisfazendo 11/13; o 12 para na guarda inicial. Deixa desprotegido cron tardio em restart **sem pendência core**, cenário inexistente nos 13 (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:163,167,171`; `bin/deploy.sh:117,326–327`).
- **Na mesma linha alterada:** condicionar a recusa a `PEND_MSG=0`. Sobrevive porque o falso sempre devolve sucesso para mensageria; falha justamente quando há pendência dela e restart autorizado (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`; `bin/deploy.sh:194–203,326`).
- **Na proteção que o novo caso 12 promete selar:** remover somente `bin/deploy.sh:300`. A marca anterior já é barrada em `117`; a marca do caso 11 só nasce dentro do migrate. Falta nascimento entre essas guardas (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:75,97,167–169`; `bin/deploy.sh:117,300–301`).

Há também erro no comentário novo: anuncia comparação apply/publicação nos casos **3,8,11**, mas ela é exigida nos **3,13**. Pelas próprias definições, 8 não tem apply e 11 não tem publicação (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:36,110–114,132,152,164,172`).

**4. Comentário da seção 1c e caminhos adicionais**

A descrição acerta que auth, sombra aplicável, guardas de pendência e prova de casca ficam antes do apply; a prova de casca usa container sem rede (`bin/deploy.sh:145,151–162,194–218,286,301`; `bin/prova_de_casca.sh:30–34`). Acerta também que o envelope executa o comando uma vez, sem retry nesse fluxo, e que a corrente espera arquivos de conclusão (`bin/cron_run.sh:11,22–25,38`; `bin/encadeado.sh:89–96`).

**A janela residual não se limita à duração do migrate.** Este caminho é permitido pela sequência do código:

1. A consulta pré-apply termina sem marca viva (`bin/deploy.sh:300`; `bin/deploy.sh:108–111`).
2. O migrate termina com sucesso (`bin/deploy.sh:301`).
3. Um envelope cria sua marca **depois desse término** (`bin/cron_run.sh:9–11`).
4. A seção 2 consulta novamente, encontra o cron e recusa (`bin/deploy.sh:324–327`).

Logo, **sim**, há banco andado e recusa na seção 2 fora do residual literalmente descrito como cron que nasce “durante o migrate”. Também cabe nascimento depois da observação prévia e antes de começar o apply (`bin/deploy.sh:295–301,326`; `bin/cron_run.sh:9`).

Existe ainda a corrida inversa: cron nasce depois da última observação e antes do restart; a consulta não impede que esse cron seja atingido pelo restart (`bin/deploy.sh:326–327`; `bin/cron_run.sh:9–11`). Ocorrência real: [nao verificado].

O comentário promete pergunta no topo do RELATO, mas o topo disponível trata de outras obras; registro da pergunta prometida: [nao verificado] (`bin/deploy.sh:298`; `app/docs/RELATO.md:2–12`).

**5. Classes de reprovação**

| Classe | Sim/não e prova nesta versão |
|---|---|
| **Segundo juiz duplicando o existente** | **Não identificado na cura:** as três consultas chamam a mesma função; auth também permanece delegada ao mesmo script (`bin/deploy.sh:110–117,145,300,326`). |
| **Segundo escritor fora da porta** | **Sim para mutação das marcas:** além do envelope, o deploy apaga marcas. É limpeza já existente, não novo aplicador de migration/publicador (`bin/cron_run.sh:9–10`; `bin/deploy.sh:109,301,327,344`). |
| **Relógio fora da borda** | **Não introduzido pelas alterações mostradas:** a decisão temporal permanece em `janela_auth.sh`. Há limite preexistente: dia/hora são lidos separadamente e não são revalidados na publicação (`bin/janela_auth.sh:49–56`; `bin/deploy.sh:145,301,327,344`). Ocorrência de atravessar a janela: [nao verificado]. |
| **Derivação paralela de fonte única** | **Não para a regra de recusa:** há função única reutilizada. O endereço das marcas tem duas bases de construção, equivalentes apenas na instalação declarada (`bin/deploy.sh:53,108–117,300,326`; `bin/cron_run.sh:9`; `app/config/crons.py:19`). |
| **Selo/RED sem mordida** | **Sim, parcialmente:** caso 12 não protege isoladamente a rechecagem pré-apply; o comentário de cobertura também aponta casos errados (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:36,97,167–169`; `bin/deploy.sh:117,300`). RED executado: [nao verificado]. |
| **Ordem de trava** | **Sim quanto à falta de exclusão comum**, não há deadlock demonstrado: envelope cria marca diretamente e deploy amostra sem impedir novos nascimentos (`bin/cron_run.sh:9–11`; `bin/deploy.sh:108–113,300,326–327`). |
| **Pouso antes do pré-requisito** | **Sim para apply antes da decisão final de recusa**; não para a ordem atual apply→HUP/restart (`bin/deploy.sh:301,326–327,344`). O selo aceita expressamente o primeiro caminho (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:164`). |
| **Campo com dois significados** | **Não demonstrado em `APLICAR_MIGRATE`/`RESTART`:** o primeiro é decisão de aplicar, não comprovante de aplicação; o segundo é modo manual, não comprovante de restart concluído. Seus leitores atuais respeitam isso (`bin/deploy.sh:61,70–75,115,200,223,299–300,323,429`). As marcas compartilham o protocolo de nome/PID; o timestamp não participa da decisão (`bin/cron_run.sh:9–10`; `bin/deploy.sh:108–113`). |

O reload agendado **lê atualmente o mesmo portão de auth**, sem isenção por modo; a configuração chama esse mesmo deploy com `--reload-agendado` (`bin/deploy.sh:65,126–145`; `app/config/crons.py:326`). Instalação efetiva desse cron e resultado em produção: [nao verificado].

## 08/11 23:56 · sessao 08a5d54a032d · - · - · origem=Ronald (tmux)

**Conferência e trava.** Para IDs não nulos existentes, a condição é de inclusão correta: todo Colaborador/Pergunta/Disputa da segunda leitura precisa pertencer à respectiva classe solicitada na primeira. Trocas de associação entre linhas já cobertas e encolhimentos podem passar: não exigem adquirir outra linha. `_cs` inclui também os donos de `velho` e `novo`, portanto aceitar Pergunta cujo colaborador passou a ser um desses donos é correto: ele já foi solicitado à trava. (`app/chamados/services/fio.py:2084–2098`)

| Valor | Escritor comprovado | Leitor relevante |
|---|---|---|
| Pergunta `pk` | Definição `BigAutoField(primary_key=True)` na criação do modelo; inserção por `p.save(force_insert=True)`. (`app/chamados/migrations/0014_perguntadisputa.py:19`; `app/chamados/models.py:3016–3019`) | Pedido da trava e conferência. (`app/chamados/services/fio.py:2086–2092`) |
| Pergunta `disputa_id` | Nascimento por `p = cls(disputa=disputa, ...)`, seguido de inserção. (`app/chamados/models.py:3016–3019`) | Pedido da trava e diferença nova; retratação solicita novamente as disputas da canônica/perdedoras. (`app/chamados/services/fio.py:2086–2092`; `app/chamados/services/fio.py:1328–1330`) |
| Pergunta `colaborador_id` | `_chave_de_identidade` deriva do dono da disputa e recusa valor explícito divergente; migration preenche o acervo. (`app/chamados/models.py:3430–3435`; `app/chamados/migrations/0051_decisao_pergunta.py:25`) | Construção de `_cs`, conferência, identidade/dedup do fato. (`app/chamados/services/fio.py:2085–2091`; `app/chamados/models.py:2761–2763`) |
| `_conjunto_da_migracao` | Não é coluna: a função monta lista das Perguntas cujo destino é velho/novo e soma o universo de cada semente. (`app/chamados/services/fio.py:2032–2039`) | Leitura antes e depois da trava. (`app/chamados/services/fio.py:2084–2088`) |
| Destino que determina sementes | Nascimento e reapontamento/reuso. (`app/chamados/models.py:3016`; `app/chamados/services/fio.py:922–925`; `app/chamados/models.py:2971–3009`) | Filtro das sementes. (`app/chamados/services/fio.py:2037`) |
| Identidade que determina expansão | Dia/célula podem mudar no rechaveamento; via de resolução muda na porta do carimbo. (`app/chamados/services/fio.py:1028–1034`; `app/chamados/models.py:3182–3190`) | `universo_da_linha` delega ao fecho; o passo do fato exclui vias fora do fato. (`app/chamados/services/fio.py:1906–1911`; `app/chamados/models.py:2730–2736`) |

**O que efetivamente é solicitado à trava:** C = `_cs`; D = disputas das `_ps`; Ch = `{velho.pk, novo.pk}`; P = pks das `_ps`. A implementação elimina `None` e repetidos, executa cada classe por pk crescente e na ordem C→D→Ch→P. Os Chamados pais das disputas **não** entram automaticamente nesta chamada: isso depende de `abertas_do_colaborador`, ausente aqui. (`app/chamados/services/fio.py:2085–2087`; `app/chamados/models.py:2660–2702`)

**Chamado não conferido:** seu conjunto de pks é fixo pelos argumentos `velho`/`novo`; não depende de associação mutável das Perguntas. Logo não há quarta classe de pks dinâmica omitida nesta chamada. Contudo, os donos de `velho`/`novo` vêm das instâncias recebidas e não são relidos sob trava; `_sob` apenas relê Perguntas. Uma eventual troca persistida de dono dos endpoints não seria detectada por esta conferência. Escritor concreto dessa troca após nascimento não foi localizado; ocorrência e alcançabilidade: **[nao verificado]**. (`app/chamados/services/fio.py:2085–2092`)

O censo AST em memória de Python de produção não encontrou atribuição explícita de `Pergunta.disputa/disputa_id` após nascimento, nem atualização literal dessas chaves. Não transforma imutabilidade em garantia: o `save` genérico só chama `_chave_de_identidade` em inserções e depois delega ao ORM; persistência por escritor indireto/dinâmico continua **[nao verificado]**. (`app/chamados/models.py:3420–3423`)

**None quebra a equivalência literal entre conjunto comparado e conjunto travável.** `_pks` elimina `None`; `disputas=[None]` não executa consulta de Disputa. Entretanto, `_fora[2]` compara conjuntos crus. Portanto, se `None` aparece apenas na segunda leitura, ocorre recusa embora nenhuma nova linha de Disputa corresponda a esse valor. Se `None` já estava na primeira leitura, não há recusa por ele; qualquer ID real novo continua sendo recusado. (`app/chamados/models.py:2660–2661`; `app/chamados/models.py:2679–2684`; `app/chamados/services/fio.py:2091–2093`)

Exemplos deduzidos da expressão:

- Antes `{7}`, depois `{None}` → recusa por `{None}`, embora `None` não seja travado. (`app/chamados/services/fio.py:2092–2093`; `app/chamados/models.py:2661`)
- Antes `{None, 7}`, depois `{None, 8}` → recusa por `{8}`. (`app/chamados/services/fio.py:2092–2093`)
- Antes `{7, 8}`, depois `{8}` → aceita, corretamente para inclusão de linhas necessárias. (`app/chamados/services/fio.py:2092–2093`)

A FK `Pergunta.disputa` é obrigatória tanto no modelo atual quanto na migration de criação: não declara `null=True`. Assim, a diferença de semântica existe na função, mas a presença de `None` em Pergunta persistida pelo esquema esperado não foi demonstrada; estado real do schema/dados de produção: **[nao verificado]**. (`app/chamados/models.py:2424–2428`; `app/chamados/migrations/0014_perguntadisputa.py:27`)

## 08/11 23:55 · sessao 460631968e5b · - · - · origem=Ronald (tmux)

Leitura estática; nenhuma execução de teste. Resultado de produção e RED/GREEN efetivo: [nao verificado]. [nao verificado]

1. **Contrato absoluto ainda não está cumprido.** O apply ocorre em `bin/deploy.sh:301`, mas o portão de cron ainda pode recusar em `bin/deploy.sh:326`; o caso 11 passa a exigir exatamente `1 migrate / 0 restart / rc!=0`, aceitando esse residual no selo (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:163-165`). A lei local exige recusa do restart com cron vivo (`CLAUDE.md:90`); restaurá-la corrige a troca de lei da volta 3, mas não resolve simultaneamente a promessa “todo portão antes do migrate” (`bin/deploy.sh:294-298`).

2. **PROCEDENTE 4 mal curado: caso 12 não protege a recusa imediatamente anterior ao apply.** A marca é criada antes da execução do deploy (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:97`), portanto a primeira consulta já recusa em `bin/deploy.sh:115-117`. Apagar somente `bin/deploy.sh:300` continua atendendo ao caso 12 (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:167-169`); o caso 11 só cria a marca dentro do apply (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:75`). Falta cenário com marca surgindo entre a 0e e a 1c (`bin/deploy.sh:117`, `bin/deploy.sh:300`).

3. **DESCARTE 10 procede para esta obra.** “Só ler” não torna a segunda consulta inofensiva: `recusa_restart_com_cron` chama `falhou`, que termina com exit 1 (`bin/deploy.sh:92`, `bin/deploy.sh:110-113`), e a consulta após o apply está explicitamente presente (`bin/deploy.sh:301`, `bin/deploy.sh:326`).

4. **O comentário 1c estreita demais o residual.** A janela real vai da observação de marcas feita na chamada de `bin/deploy.sh:300` até a observação feita na chamada de `bin/deploy.sh:326`, ambas por `bin/deploy.sh:111`. Pode surgir marca **depois que migrate terminou**, antes da consulta da seção 2; a criação de marca não toma trava compartilhada (`bin/cron_run.sh:9-11`). Logo há caminho estático banco andado + seção 2 recusando fora da expressão literal “cron que NASCE durante o migrate” (`bin/deploy.sh:295-297`). Ocorrência em produção: [nao verificado].

5. **FILA 7 e 8 são dependências da promessa, mesmo preexistentes.** A 1c afirma que mensageria e pendências já responderam (`bin/deploy.sh:292`), e hoje de fato suas guardas estão antes do apply (`bin/deploy.sh:194-218`, `bin/deploy.sh:301`). Entretanto o falso nunca faz a mensageria recusar (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`); os cenários agendados com pendência têm auth vermelho e param antes dessa guarda (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:135-137`), e o `--sem-migrate` do caso 10 não tem pendência (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:159-161`). Remover as recusas de `bin/deploy.sh:211-218` sobrevive aos 13 por inspeção; execução do mutante: [nao verificado].

6. **FILA 9 é lacuna de modo, sem bug atual demonstrado.** A cópia altera os indicadores em `bin/deploy.sh:82`, mas passa pela mesma chamada de auth, sem ramo de isenção por modo (`bin/deploy.sh:126-145`). Os 13 cenários não chamam `--reload-copia` (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:123-173`).

7. **FILA 11: o limite do contador é real, mas há cobertura vizinha dos alvos.** Os 13 contam linhas iniciadas por restart (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:109`), enquanto o vizinho exige a linha exata `restart saas_core saas_ui mensageria` (`bin/tests/test_deploy_reload_gracioso.sh:135-139`). A lista atual é essa (`bin/deploy.sh:327`); resultado de execução do vizinho: [nao verificado].

8. **DESCARTE 12 tem justificativa exagerada.** O vizinho verifica troca de workers e sobrevivência do exec nos casos verdes (`bin/tests/test_deploy_reload_gracioso.sh:107-112`), mas transforma `selo_reciclagem` em `exit 0` (`bin/tests/test_deploy_reload_gracioso.sh:40-41`), responde sempre `STATUS 200` às provas de rota (`bin/tests/test_deploy_reload_gracioso.sh:57`) e devolve sucesso aos outros exec, incluindo readiness (`bin/tests/test_deploy_reload_gracioso.sh:58`). Assim não demonstra mordida das recusas de readiness/rotas/reciclagem (`bin/deploy.sh:366-367`, `bin/deploy.sh:403`, `bin/deploy.sh:410`).

9. **DESCARTE 13 exige ressalva para o contrato literal.** Reciclagem não é apenas contador: vermelho chama `falhou` após a publicação (`bin/deploy.sh:410`), e mede configurações do disco, cmdline/compose e crontab instalado (`bin/selo_reciclagem.sh:45-48`, `bin/selo_reciclagem.sh:53-78`). Essas verificações podem recusar por condição já existente antes do apply (`bin/deploy.sh:301`, `bin/deploy.sh:410`). A afirmação “todo portão respondeu” precisa de uma exclusão explícita desse selo ou de sua execução anterior; a exclusão não está na frase da 1c (`bin/deploy.sh:291-298`).

10. **Registro de desenho prometido não localizado.** O comentário manda ao topo do RELATO (`bin/deploy.sh:298`), mas o topo da cópia trata de outras obras (`app/docs/RELATO.md:2-12`); busca em todo o arquivo por `deploy.?portao|portao.?antes` retornou zero ocorrências. Existência da pergunta prometida: [nao verificado].

Classes arquiteturais, dentro deste delta:

- **Segundo juiz: não demonstrado.** As três consultas usam a mesma função (`bin/deploy.sh:110`, `bin/deploy.sh:117`, `bin/deploy.sh:300`, `bin/deploy.sh:326`); auth continua em um script (`bin/deploy.sh:145`).
- **Segundo escritor de publicação: não introduzido pelo delta.** Restart permanece no ramo explícito e HUP no outro ramo (`bin/deploy.sh:323-344`); a recusa restaurada não acrescenta publicação (`bin/deploy.sh:326`).
- **Relógio fora da borda: nenhum novo `date` no delta fornecido [nao verificado quanto ao histórico].** O juiz lê dia e hora separadamente (`bin/janela_auth.sh:49-56`) e o deploy o consulta antes de checks, collectstatic, prova e migrate (`bin/deploy.sh:145`, `bin/deploy.sh:184`, `bin/deploy.sh:266`, `bin/deploy.sh:286`, `bin/deploy.sh:301`); atravessar a janela temporal antes da publicação é possível por escalonamento, ocorrência [nao verificado].
- **Derivação paralela: não na recusa de cron.** Repetição chama a função única (`bin/deploy.sh:110-117`, `bin/deploy.sh:300`, `bin/deploy.sh:326`).
- **Selo sem mordida: sim, parcialmente.** Caso 12 não distingue a retirada da linha 300; configuração de marca anterior e primeira recusa provam a máscara (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:97`, `bin/deploy.sh:115-117`). RED efetivamente rodado: [nao verificado].
- **Ordem/exclusão de trava: sim, falta exclusão comum; deadlock não demonstrado.** O envelope cria marca e roda comando diretamente (`bin/cron_run.sh:9-11`); o deploy somente amostra a marca (`bin/deploy.sh:108-113`, `bin/deploy.sh:300-326`).
- **Pouso antes do pré-requisito: não para apply antes de publicação nos caminhos atuais** (`bin/deploy.sh:301`, `bin/deploy.sh:327`, `bin/deploy.sh:344`); **sim para apply antes da decisão final de recusa**, no residual aceito (`bin/deploy.sh:301`, `bin/deploy.sh:326`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:164`).

## 08/11 23:54 · sessao d3cf7e7093bb · - · - · origem=Ronald (tmux)

- **b1 e c curados no alvo dos testes**: casos isolados de pk, colaborador e disputa exigem recusa, e o mutante `or → and` é construído explicitamente; isso distingue a ausência de cada parcela da guarda. (`app/chamados/tests/test_e1_fila.py:489`, `app/chamados/tests/test_e1_fila.py:504`)

- **b2 parcialmente curado**: retirar literalmente `_migrar_sob_trava` é distinguido pelo novo par RED/MORDE, mas o RED aceita apenas `recusadas` preenchida. Seu cenário usa Pergunta do colaborador base e chamados de outro colaborador, que o juiz recusa por cruzamento; portanto esse RED não exige migração efetiva nem escrita no banco. (`app/chamados/tests/test_e1_fila.py:147`, `app/chamados/tests/test_e1_fila.py:511`, `app/chamados/tests/test_e1_fila.py:517`, `app/chamados/juizes.py:1999`) Teste antigo exige migração efetiva de conversa, inclusive refresh e destino persistido. (`app/chamados/tests/test_e1_v15g_portas.py:364`)

- **b3 e b4 curados diretamente**: RuntimeError genérica deve subir; modelo e pk do evento agora são conferidos. A captura produtiva permanece específica de `UniversoMudouSobATrava`. (`app/chamados/tests/test_e1_fila.py:566`, `app/chamados/tests/test_e1_fila.py:564`, `app/chamados/models.py:1417`)

- **b5 DESCARTE defensável para a promessa de trava**: RED exige canônica e destino travados, além da ordem. A porta seleciona proposta antes de chamar `_trava_do_ato`; resposta e validação posteriores determinam seu retorno, uma exigência diferente. (`app/chamados/tests/test_e1_fila.py:442`, `app/chamados/services/declaracao_texto.py:643`, `app/chamados/services/declaracao_texto.py:681`)

- **b6 DESCARTE por construção atual**: `_trava_do_ato` recebe motivo/dia, usa `universo_do_fato` e não restringe motivo. Ambas as portas chamam o helper incondicionalmente após selecionar o fato. Os testes próprios exercitam somente `saida_sem_entrada`, portanto uma futura guarda que limitasse a trava a esse motivo escaparia desses casos. (`app/chamados/services/declaracao_texto.py:282`, `app/chamados/services/declaracao_texto.py:339`, `app/chamados/services/declaracao_texto.py:646`, `app/chamados/tests/test_e1_fila.py:180`, `app/chamados/tests/test_e1_fila.py:438`)

- **Mutantes plausíveis ainda não distinguidos pelos novos casos**:
  - Trocar a diferença de disputas por diferença simétrica: recusaria também encolhimento, embora nenhuma disputa usada estivesse fora da trava. A sonda só acrescenta uma linha; falta caso de remoção/encolhimento. (`app/chamados/services/fio.py:2092`, `app/chamados/tests/test_e1_fila.py:473`)
  - Remover `key=str` da ordenação da mensagem: falharia quando a diferença contivesse simultaneamente `None` e um inteiro; cada caso injeta somente uma linha adicional e não forma essa diferença mista. (`app/chamados/services/fio.py:2097`, `app/chamados/tests/test_e1_fila.py:469`, `app/chamados/tests/test_e1_fila.py:492`)
  - Substituir o efeito real do laço por preenchimento artificial de `recusadas` satisfaria o novo RED que promete “o laço roda”; o teste só consulta o retorno. Outros testes antigos podem barrar isso. (`app/chamados/tests/test_e1_fila.py:511`, `app/chamados/tests/test_e1_v15g_portas.py:364`)

- **Limites da prova**: a corrida é simulada por patch de `_conjunto_da_migracao` com `SimpleNamespace`, inclusive duplicando um pk com outro colaborador/disputa; prova a guarda por conjuntos, sem demonstrar concorrência real. O helper retorna imediatamente na exceção, antes da asserção das duas leituras; essa asserção só acontece quando não houve recusa. (`app/chamados/tests/test_e1_fila.py:466`, `app/chamados/tests/test_e1_fila.py:479`) Execução dos testes e sobrevivência na suíte inteira: [nao verificado].

## 08/11 23:54 · sessao 3cf5076efdda · - · - · origem=Ronald (tmux)

Triagem **a: FILA defensável apenas como delimitação do diff; defeitos reais permanecem**. O contador de adesão ignora o retorno e incrementa sempre (`app/chamados/services/adesao.py:89-92`); o reconciliador ignora a recusa (`app/ponto/management/commands/reconciliar_grade.py:529-531`) e o chamador conta `CREATE` (`app/ponto/management/commands/reconciliar_grade.py:254-259`); a ação administrativa reabre Disputa e cria conversa antes de chamar `reabrir`, ignora retorno e anuncia sucesso (`app/chamados/services/acoes_disputa.py:256-267`, `:294-296`); a view atribui todo `False` a irmão vivo (`app/chamados/views_arquivar.py:70-72`). Outros motivos de `False` existem atualmente (`app/chamados/models.py:1391-1407`, `:1437-1438`), mas anterioridade à obra não se prova na cópia sem histórico: **[nao verificado]**.

Triagem **d: FILA técnica real, não cura concluída**. O juiz ainda lê `date.today()` (`app/chamados/juizes.py:569-575`) e tem leitores (`:674`, `:764`, `:1217`). `expira_em` diz reproduzir a regra (`app/chamados/veredito_lavrado.py:120-122`) e conserva implementação própria (`:136-143`), comparável ao juiz (`app/chamados/juizes.py:595-601`). A cura f5 faz `expira_em` usar a borda (`app/chamados/veredito_lavrado.py:95-102`, `:137`), com instante passado pela lavra (`:251-253`). Registro prévio da FILA/cronologia: **[nao verificado]**.

Para a pergunta 5, separar **diff v4** de **contexto da obra**:

| Classe | Resultado e prova |
|---|---|
| Segundo juiz que duplica juiz registrado | **Não identificado nas linhas v4**: elas conferem conjuntos antes de executar o laço (`app/chamados/services/fio.py:2091-2098`); o laço consulta `pergunta_viva` e `canonica_do_grupo` (`:2112`, `:2123`, `:2145`). A autoridade C6 registrada é `pergunta_viva` (`app/core/juizes.py:266`). Ausência absoluta na árvore: **[nao verificado]**. |
| Segundo escritor fora da porta | **Não introduzido pelo diff v4**: ele não acrescenta gravações e mantém `_migrar_sob_trava` depois da conferência (`app/chamados/services/fio.py:2091-2098`). Censo global completo: **[nao verificado]**. |
| Relógio fora da borda | **Sim no contexto**, em `app/chamados/juizes.py:575`; f5 cura somente o leitor da lavra (`app/chamados/veredito_lavrado.py:95-102`, `:137`). |
| Derivação paralela da fonte única | **Sim no contexto**, regra da janela repetida por `expira_em` (`app/chamados/veredito_lavrado.py:120-143`) frente ao juiz (`app/chamados/juizes.py:569-601`). |
| Ordem de trava | **Porta central executa C→D→Ch→P**, respectivamente `app/chamados/models.py:2674`, `:2683`, `:2696`, `:2700`. V4 preserva chamada única de topo (`app/chamados/services/fio.py:2086-2087`). Exatidão do universo depende dos achados do explorador locks. |
| Pouso antes do pré-requisito | **Sim no contexto da ação administrativa**: disputa e conversa mudadas antes de consultar o resultado da reabertura (`app/chamados/services/acoes_disputa.py:256-267`). **Não nas linhas v4**: conferência precede laço (`app/chamados/services/fio.py:2093-2098`); destino aprovado precede retratação (`:2164-2170`). |
| Campo com dois significados | Não encontrei exemplo adicional comprovado nesta exploração; **[nao verificado]**. |

Para **b5**, o descarte é procedente no contrato específico de ordem: o RED exige canônica e destino presentes na sequência e ausência de inversões (`app/chamados/tests/test_e1_fila.py:442-447`), enquanto a porta toma a trava depois de encontrar proposta (`app/chamados/services/declaracao_texto.py:643-650`). O teste não prova sucesso funcional da aplicação; isso fica **[nao verificado]**.

## 08/11 23:54 · sessao 7d1c7808290c · - · - · origem=Ronald (tmux)

- `APLICAR_MIGRATE`: escritores de valor são inicialização `0` e armação `1` (`bin/deploy.sh:61`, `bin/deploy.sh:223`); único leitor operacional é a decisão do apply (`bin/deploy.sh:299`). O comentário “UM escritor” é correto somente contando a armação e excluindo a inicialização explicitamente mencionada na linha seguinte; literalmente existem duas atribuições (`bin/deploy.sh:59`, `bin/deploy.sh:60`). O selo também injeta `APLICAR_MIGRATE=1` no ambiente do subprocesso, sobreposto pela inicialização (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:98`, `bin/deploy.sh:61`).

- `RESTART`: escritores `0` e `1`, este último exclusivamente para `MODO=--restart` (`bin/deploy.sh:70`, `bin/deploy.sh:72`, `bin/deploy.sh:73`). Leitores: guarda inicial de cron (`bin/deploy.sh:115`), exceção da pendência da mensageria (`bin/deploy.sh:200`), guarda imediatamente anterior ao apply (`bin/deploy.sh:300`), escolha restart/HUP (`bin/deploy.sh:323`) e mensagem final (`bin/deploy.sh:429`). Fora dele, o contrato lê o texto do ramo, não o valor em execução (`bin/contrato_deploy.sh:41`).

- Marcas: o envelope cria diretório/nome `N_E.$$`, escreve timestamp e registra remoção no `EXIT` (`bin/cron_run.sh:9`, `bin/cron_run.sh:10`). O deploy enumera arquivos, lê PID do sufixo, consulta `/proc`, devolve o nome dos vivos **e apaga marcas mortas**; portanto também é escritor de limpeza (`bin/deploy.sh:108`, `bin/deploy.sh:109`). A recusa consome esses nomes (`bin/deploy.sh:111`, `bin/deploy.sh:112`), sendo chamada em três pontos (`bin/deploy.sh:117`, `bin/deploy.sh:300`, `bin/deploy.sh:326`). O conteúdo ISO não é lido pelo deploy; o protocolo efetivo depende do nome e da existência do PID (`bin/cron_run.sh:10`, `bin/deploy.sh:109`).

- Escritores dos selos: marca inicial e marca durante o migrate no selo desta obra (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:97`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:75`), ambas usando PID vivo exportado pelo selo (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:96`); selo vizinho escreve marca com PID de seu processo falso (`bin/tests/test_deploy_reload_gracioso.sh:90`, `bin/tests/test_deploy_reload_gracioso.sh:91`). O contrato apenas procura os textos dos caminhos nos scripts (`bin/contrato_deploy.sh:81`, `bin/contrato_deploy.sh:82`).

- O endereço do escritor é absoluto (`bin/cron_run.sh:9`); o leitor deriva a raiz da localização do script (`bin/deploy.sh:53`, `bin/deploy.sh:108`). A configuração agenda ambos sob `/home/ronald/saas-hasner`, tornando-os equivalentes **nessa localização declarada** (`app/config/crons.py:19`, `app/config/crons.py:326`, `app/config/crons.py:1475`, `app/config/crons.py:1486`). Equivalência universal ou resolução real do caminho em produção: [nao verificado].

- Os elos também passam pelo mesmo escritor de marcas: `encadeado.sh` executa `cron_run.sh` depois de esperar arquivos de fim (`bin/encadeado.sh:89`, `bin/encadeado.sh:102`, `bin/encadeado.sh:104`). O envelope executa o comando uma vez, carimba fim apenas para rc `0`/`2`, e termina com o rc; não há retry nesse fluxo (`bin/cron_run.sh:11`, `bin/cron_run.sh:22`, `bin/cron_run.sh:25`, `bin/cron_run.sh:38`).

- O comentário limita demais o residual ao cron que “NASCE durante o migrate” e à “duração do migrate” (`bin/deploy.sh:295`, `bin/deploy.sh:296`). Sem exclusão entre envelope e deploy, a marca pode nascer **depois da varredura prévia terminar, antes do apply começar**, ou **depois do apply retornar, antes da varredura final**; nesses caminhos o banco pode andar e a seção 2 recusar (`bin/deploy.sh:300`, `bin/deploy.sh:301`, `bin/deploy.sh:324`, `bin/deploy.sh:326`, `bin/cron_run.sh:9`). Uma marca também pode nascer depois da varredura final e antes do restart, permitindo matar seu comando (`bin/deploy.sh:326`, `bin/deploy.sh:327`, `bin/cron_run.sh:9`, `bin/cron_run.sh:11`). Ocorrência real desses interleavings: [nao verificado].

## 08/11 23:54 · sessao ff715dfd3348 · - · - · origem=Ronald (tmux)

Auditoria estática, sem executar o selo; resultados de execução ou RED dos mutantes continuam [nao verificado].

Os casos matam estas famílias de mutante, conforme suas asserções:

| Caso | Mutante detectado | Prova |
|---|---|---|
| 1 | Auth removida/ignorada ou consultada depois de aplicar migration. | `bin/tests/test_deploy_portoes_antes_do_migrate.sh:123-125` |
| 2 | Sombra removida/ignorada ou consultada depois do apply. | `bin/tests/test_deploy_portoes_antes_do_migrate.sh:127-129` |
| 3 | Apply ausente/duplicado; algum dos três portões ausente ou depois do apply; publicação antes do apply; HUP ausente/duplicado. | `bin/tests/test_deploy_portoes_antes_do_migrate.sh:103-117`, `:131-133` |
| 4 | Agendado dispensa **chamar** auth; conta sua passagem mesmo que a pendência também recusasse. Não isola o respeito ao retorno de auth. | `bin/tests/test_deploy_portoes_antes_do_migrate.sh:135-137` |
| 5 | `--conferir` aplica/publica, passa por auth ou sombra, ou deixa de terminar com sucesso. | `bin/tests/test_deploy_portoes_antes_do_migrate.sh:139-141` |
| 6 | Prova de casca removida/ignorada ou depois do apply. | `bin/tests/test_deploy_portoes_antes_do_migrate.sh:143-145` |
| 7 | `--sem-sombra` também dispensa auth ou aplica antes dela. | `bin/tests/test_deploy_portoes_antes_do_migrate.sh:147-149` |
| 8 | Apply incondicional sem pendência; não publicar quando não precisa migrar. | `bin/tests/test_deploy_portoes_antes_do_migrate.sh:151-153` |
| 9 | Agendado chama auth mas ignora sua recusa: aqui não existe a pendência que mascarava isso no caso 4. | `bin/tests/test_deploy_portoes_antes_do_migrate.sh:155-157` |
| 10 | Inicialização de `APLICAR_MIGRATE=0` removida, deixando o valor herdado aplicar migration em `--sem-migrate`. | `bin/deploy.sh:61`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:98`, `:159-161` |
| 11 | Recusa final removida ou condicionada como na volta 3, permitindo restart depois do apply com cron vivo. | `bin/deploy.sh:326`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:75`, `:163-165` |
| 12 | Remoção de **todas** as recusas que impediriam migrar com cron inicialmente vivo. Não identifica qual das recusas anteriores respondeu. | `bin/deploy.sh:115-118`, `:299-301`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:96-98`, `:167-169` |
| 13 | Apply ausente/duplicado, restart ausente/duplicado ou restart antes do apply. | `bin/tests/test_deploy_portoes_antes_do_migrate.sh:110-114`, `:171-173` |

Achados centrais:

- **Caso 12 não morde a remoção isolada da recusa imediatamente antes do apply**, `deploy.sh:300`: o cron nasce antes de iniciar o script (`test:97`) e já é recusado em `deploy.sh:117`. Remover só `117` também sobrevive porque `300` continua recusando. Falta criar cron entre 0e e 1c para provar a redução dessa janela (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:96-98`, `:167-169`; `bin/deploy.sh:115-118`, `:299-301`).
- **Sobrevivente de linha efetivamente tocada pelo diff:** substituir `deploy.sh:326` por `[ "$APLICAR_MIGRATE" != 1 ] || recusa_restart_com_cron`. Mantém a recusa quando apply=1, passando 11/13; caso12 cai em0e. Pula a recusa final quando apply=0, permitindo matar cron nascido depois de0e num `--restart` sem migration core. Todos os casos de restart do selo usam `FAKE_PEND_CORE=1` (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:163`, `:167`, `:171`).
- Outro sobrevivente dessa linha: condicionar `326` a `PEND_MSG=0`. O falso de mensageria retorna sempre0, impedindo distinguir a variante; em `PEND_MSG=1`, `--restart` é permitido e precisaria preservar a recusa final (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`; `bin/deploy.sh:194-203`, `:326`).
- O comentário novo anuncia PUB_OK nos casos **3,8,11**, mas o código exige apenas nos **3,13**; 8 não tem apply e11 não tem publicação, portanto PUB_OK seria0 em ambos (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:36`, `:110-114`, `:132`, `:152`, `:164`, `:172`).
- O selo não prova alvos do restart: só conta linhas `^restart`; o vizinho efetivamente compara `restart saas_core saas_ui mensageria` no caminho verde (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:109`, `:172`; `bin/tests/test_deploy_reload_gracioso.sh:134-139`).
- A alegação de que o vizinho prova readiness/rotas/reciclagem está exagerada: ele falsifica `selo_reciclagem` como sucesso, responde `STATUS 200` incondicionalmente e não tem injeção de falha de readiness/rota; prova principalmente exec sobrevivente, HUP, troca de workers e alguns modos/recusas (`bin/tests/test_deploy_reload_gracioso.sh:40-42`, `:57-58`, `:107-112`, `:116-156`).

## 08/11 23:36 · sessao 1256d4b89cb3 · BUG-DEPLOY-PORTAO · verificador · origem=Code

A volta 3 elimina a **recusa por cron após um apply bem-sucedido do core**, mas passa a permitir o restart sobre um cron recém-nascido. A afirmação “morto e refeito” não está sustentada por mecanismo de repetição; além disso, o selo de configuração continua podendo reprovar depois da publicação. (`bin/deploy.sh:300`, `bin/deploy.sh:325`, `bin/deploy.sh:296`, `bin/cron_run.sh:11`, `bin/cron_run.sh:38`, `bin/deploy.sh:409`)

A análise abaixo é estática. RED/VERDE, execução dos mutantes e efeitos reais em produção: **[nao verificado]**. Não executei o selo porque ele cria e remove arquivos. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:44`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:107`)

1) É preciso separar **passar pela posição da seção 1c** de **ter executado seu apply com sucesso**. O apply é condicional; sua falha encerra o script imediatamente. (`bin/deploy.sh:298`, `bin/deploy.sh:300`, `bin/deploy.sh:92`)

| Modo | Apply da 1c e caminhos que podem terminar sem completar a publicação das três cascas |
|---|---|
| Normal | Com pendência, faz apply; pode terminar por falha do próprio migrate, do HUP ou da troca dos workers. Sem pendência, pula apply e permanece sujeito às falhas de HUP/troca. (`bin/deploy.sh:223`, `bin/deploy.sh:300`, `bin/deploy.sh:343`, `bin/deploy.sh:356`) |
| `--sem-migrate` | Com pendência, recusa **antes** da 1c. Se chega à seção 2, não fez apply e pode falhar no HUP/troca. (`bin/deploy.sh:215`, `bin/deploy.sh:216`, `bin/deploy.sh:61`, `bin/deploy.sh:343`, `bin/deploy.sh:356`) |
| `--sem-sombra` | Pode migrar normalmente; depois disso, tem os mesmos caminhos de falha de migrate/HUP/troca do normal. O modo dispensa sombra por motivo, não migration nem auth. (`bin/deploy.sh:87`, `bin/deploy.sh:89`, `bin/deploy.sh:143`, `bin/deploy.sh:152`, `bin/deploy.sh:300`, `bin/deploy.sh:343`, `bin/deploy.sh:356`) |
| `--reload-agendado` | Com pendência, recusa antes da 1c. Sem pendência, não aplica migrate; pode falhar no HUP/troca. (`bin/deploy.sh:65`, `bin/deploy.sh:211`, `bin/deploy.sh:212`, `bin/deploy.sh:343`, `bin/deploy.sh:356`) |
| `--reload-copia` | Com pendência, recusa antes da 1c. Sem pendência, não aplica migrate; pode falhar no HUP/troca. (`bin/deploy.sh:82`, `bin/deploy.sh:208`, `bin/deploy.sh:209`, `bin/deploy.sh:343`, `bin/deploy.sh:356`) |
| `--restart` | Com apply bem-sucedido, pula a recusa de cron na seção 2 e tenta restart; pode terminar por falha do migrate ou do restart. **Sem apply**, pode terminar por **RECUSA de cron** na seção 2, antes de chamar restart. (`bin/deploy.sh:300`, `bin/deploy.sh:325`, `bin/deploy.sh:326`, `bin/deploy.sh:112`) |
| `--conferir` | Não alcança a 1c: termina antes, sem publicação. (`bin/deploy.sh:227`, `bin/deploy.sh:229`) |

Depois de uma chamada de HUP/restart, ainda existem estas saídas explícitas:

| Saída | O que permite concluir |
|---|---|
| Timeout de disponibilidade de qualquer casca | A publicação foi tentada; não se confirmou que as três ficaram disponíveis. (`bin/deploy.sh:365`, `bin/deploy.sh:366`, `bin/deploy.sh:368`) |
| Uma ou mais rotas reprovadas | A publicação foi tentada; a prova funcional falhou. Não demonstra, por si só, ausência de recarga. (`bin/deploy.sh:399`, `bin/deploy.sh:400`, `bin/deploy.sh:401`, `bin/deploy.sh:402`) |
| Selo de reciclagem vermelho | Reprova depois da tentativa de publicação por configuração/cmdline/compose/crontab. É uma guarda de conformidade posterior, não apenas erro operacional de HUP/restart. (`bin/deploy.sh:409`, `bin/selo_reciclagem.sh:46`, `bin/selo_reciclagem.sh:54`, `bin/selo_reciclagem.sh:64`, `bin/selo_reciclagem.sh:72`, `bin/selo_reciclagem.sh:78`) |
| Final “OK” sem PIDs anteriores conhecidos | O código admite continuar apenas com aviso quando não consegue listar os workers anteriores. Nesse caminho, não exige prova da troca deles. Recarga efetiva: **[nao verificado]**. (`bin/deploy.sh:341`, `bin/deploy.sh:346`, `bin/deploy.sh:431`) |

Portanto: **há RECUSA depois da posição da 1c**, no `--restart` **sem apply**, pela marca de cron. **Não há essa recusa depois de apply bem-sucedido**; o curto-circuito a desabilita. (`bin/deploy.sh:325`, `bin/deploy.sh:112`)

Falhas do apply, HUP ou restart não garantem ausência de efeitos parciais; o script encerra sem executar recuperação nesses ramos. Efeito parcial real: **[nao verificado]**. (`bin/deploy.sh:300`, `bin/deploy.sh:326`, `bin/deploy.sh:343`, `bin/deploy.sh:92`)

A frase literal “todo portão antes do migrate” continua mais forte que o código: o selo de reciclagem permanece depois dele, embora esse selo já não impeça a tentativa de publicação. (`bin/deploy.sh:300`, `bin/deploy.sh:409`)

2) O censo de escritores e leitores é este:

| Estado | Quem escreve | Quem lê |
|---|---|---|
| `APLICAR_MIGRATE` | Inicialização `0`; seleção elegível `1`. São dois pontos de atribuição no mesmo script. (`bin/deploy.sh:61`, `bin/deploy.sh:223`) | Decide o apply e decide dispensar a recusa posterior de cron. Portanto são **dois leitores**, contrariando o comentário “UM leitor”. (`bin/deploy.sh:298`, `bin/deploy.sh:325`, `bin/deploy.sh:59`) |
| `RESTART` | Inicialização `0`; somente o primeiro argumento `--restart` seleciona `1`. (`bin/deploy.sh:70`, `bin/deploy.sh:72`, `bin/deploy.sh:73`) | Recusa inicial de cron; exceção da pendência da mensageria; recusa pré-apply; seleção restart/HUP; mensagem final. (`bin/deploy.sh:115`, `bin/deploy.sh:200`, `bin/deploy.sh:299`, `bin/deploy.sh:322`, `bin/deploy.sh:428`) |
| Marcas de produção | O envelope cria diretório, escreve `<nome>_<empresa>.<PID>` e remove no `EXIT`. **Deploy também escreve no ciclo de vida: apaga marcas consideradas mortas.** (`bin/cron_run.sh:9`, `bin/cron_run.sh:10`, `bin/deploy.sh:109`) | Deploy enumera arquivos, testa `/proc/<PID>`, extrai o nome e usa o resultado para recusar. Não lê o timestamp do conteúdo. (`bin/deploy.sh:108`, `bin/deploy.sh:109`, `bin/deploy.sh:111`, `bin/deploy.sh:112`) |

Fora do deploy: [nao verificado]

- O selo desta obra remove `APLICAR_MIGRATE` herdado e o injeta novamente quando `FAKE_HERDA_APLICAR` está presente; o caso 10 seleciona essa injeção. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:90`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:146`)
- O contrato estrutural lê o **texto** do teste de `RESTART`, não o valor da variável. Também procura textualmente a presença do mecanismo de marcas nos dois scripts. (`bin/contrato_deploy.sh:41`, `bin/contrato_deploy.sh:81`, `bin/contrato_deploy.sh:82`)
- O selo gracioso escreve marca com PID do processo falso; o selo desta obra escreve `cron_falso.1`. As respectivas limpezas removem as árvores falsas. (`bin/tests/test_deploy_reload_gracioso.sh:90`, `bin/tests/test_deploy_reload_gracioso.sh:91`, `bin/tests/test_deploy_reload_gracioso.sh:104`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:107`)

**Os valores originais de `APLICAR_MIGRATE` e `RESTART` herdados do ambiente não alcançam seus leitores:** ambos são sobrescritos primeiro. (`bin/deploy.sh:61`, `bin/deploy.sh:70`, `bin/deploy.sh:298`, `bin/deploy.sh:115`)

Isso não significa ambiente neutralizado em geral. Continuam sendo entradas deliberadas: `DEPLOY_SEM_SOMBRA`, `SEM_JANELA_AUTH_MOTIVO`, `SEM_PROVA_DE_CASCA`, `HOST_TENANT` e as costuras `JANELA_AUTH_STAMP/DOW/HHMM`. Elas alcançam leitores e podem mudar o comportamento correspondente. (`bin/deploy.sh:85`, `bin/deploy.sh:143`, `bin/deploy.sh:282`, `bin/deploy.sh:376`, `bin/janela_auth.sh:36`, `bin/janela_auth.sh:49`, `bin/janela_auth.sh:50`)

O escritor das marcas usa caminho absoluto, enquanto deploy lê relativo à sua `RAIZ`. A equivalência desses diretórios em produção é **[nao verificado]**. (`bin/cron_run.sh:9`, `bin/deploy.sh:53`, `bin/deploy.sh:108`)

3) No cenário perguntado, com apply do core bem-sucedido:

- **Volta 2:** consultava novamente as marcas e podia recusar antes do restart, deixando o apply executado sem publicar as cascas. A chamada incondicional removida estava em `bin/deploy.sh` da volta 2:318, conforme o hunk `-315/+321` do anexo `codex_deployportao3.diff`.
- **Volta 3:** `APLICAR_MIGRATE=1` dispensa essa consulta; o restart é chamado apesar do cron recém-nascido. (`bin/deploy.sh:325`, `bin/deploy.sh:326`)
- Se esse cron estiver executando por `docker exec` numa casca reiniciada, ele fica exposto à interrupção que o próprio script documenta. Morte concreta e código de saída nesta produção: **[nao verificado]**. (`bin/deploy.sh:311`, `bin/deploy.sh:314`)

“Idempotente” não implica “automaticamente reexecutado”. O envelope executa o comando uma vez, carimba conclusão apenas para rc `0/2`, tenta alarmar nas outras falhas e sai; não contém retry. A idempotência universal é requisito textual, não prova apresentada. (`bin/cron_run.sh:11`, `bin/cron_run.sh:22`, `bin/cron_run.sh:32`, `bin/cron_run.sh:38`, `CLAUDE.md:246`)

A interrupção pode ainda deixar dependentes sem carimbo de conclusão: eles aguardam o arquivo e alarmam ao atingir o teto. A recuperação efetiva da corrente é **[nao verificado]**. (`bin/cron_run.sh:25`, `bin/encadeado.sh:89`, `bin/encadeado.sh:92`, `bin/encadeado.sh:94`)

Uma ordem concebível seria **bloquear novas admissões de cron → drenar os existentes → manter o bloqueio durante migrate/publicação → liberar**. Isso exigiria uma exclusão compartilhada entre envelope e deploy, ausente na sequência atual; sua implementação e viabilidade aqui são **[nao verificado]**. (`bin/cron_run.sh:9`, `bin/cron_run.sh:11`, `bin/deploy.sh:299`, `bin/deploy.sh:300`, `bin/deploy.sh:326`)

Para prometer também ausência absoluta de atendimento com Python velho sobre schema novo, seria necessário tratar o tráfego durante migrate/publicação, por pausa/drenagem ou compatibilidade das migrations. Essa alternativa não está sendo proposta como correta para esta aplicação: **[nao verificado]**.

4) Estes são os mutantes mortos **por inspeção das asserções**, não por execução:

| Caso | Mutante que distingue |
|---|---|
| **1** | Migrar antes da recusa de auth, ou ignorá-la no normal: exige falha, zero apply e zero HUP. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:110`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:111`) |
| **2** | Migrar antes da recusa de sombra, ou ignorá-la: mesmas exigências. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:114`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:115`) |
| **3** | Omitir/duplicar apply; aplicar antes da primeira auth/sombra/casca; trocar `sombra --conferir` por outra operação; omitir HUP. Exige contagens e primeira passagem dos três portões anterior ao apply. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:95`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:97`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:103`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:119`) |
| **4** | Isentar o agendado da **chamada** à auth. Ignorar apenas seu retorno pode escapar deste caso, porque a pendência já oferece outra recusa. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:122`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:123`, `bin/deploy.sh:211`) |
| **5** | Perguntar auth/sombra, aplicar migrate ou enviar HUP em `--conferir`. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:126`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:127`) |
| **6** | Aplicar antes da prova de casca recusar, ou ignorar sua recusa. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:130`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:131`) |
| **7** | Fazer `--sem-sombra` também dispensar auth, ou migrar antes da recusa dela. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:134`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:135`) |
| **8** | Aplicar sem pendência; deixar de enviar HUP no caminho sem pendência. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:138`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:139`) |
| **9** | Consultar auth no agendado e ignorar seu retorno: não há pendência para mascarar o erro. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:142`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:143`) |
| **10** | Deixar `APLICAR_MIGRATE=1` herdado provocar apply em `--sem-migrate`. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:90`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:146`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:147`) |
| **11** | Restaurar a recusa incondicional da volta 2 depois do apply. A mordida depende de `/proc/1` existir: a marca falsa usa PID fixo `1`, e o leitor consulta `/proc`. Essa precondição ambiental é **[nao verificado]**. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:150`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:151`, `bin/deploy.sh:109`) |

Mutantes plausíveis que **ainda sobrevivem aos 11 por inspeção**; execução: **[nao verificado]**:

- **Mover o bloco inteiro da 1c para depois da seção 2.** `ORDEM_OK` compara portões com apply, mas não compara apply com HUP/restart. No caso 11, o restart passa a acontecer antes de a marca nascer, e as contagens continuam aceitas. É o sobrevivente mais diretamente contrário ao pré-requisito schema → publicação. (`bin/deploy.sh:298`, `bin/deploy.sh:358`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:103`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:119`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:151`)
- **Remover as recusas prévias de cron.** Nenhum cenário instala cron antes delas; a única marca deste selo nasce durante o apply falso. (`bin/deploy.sh:117`, `bin/deploy.sh:299`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:45`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`)
- **Remover a guarda de pendência da mensageria.** O falso responde sempre sucesso ao check dela. (`bin/deploy.sh:200`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:69`)
- **Remover a recusa de pendência do agendado ou de `--sem-migrate`.** O agendado com pendência já encontra auth vermelha; `--sem-migrate` só é exercitado sem pendência. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:122`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:142`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:146`, `bin/deploy.sh:211`, `bin/deploy.sh:215`)
- **Dispensar auth/sombra no `--reload-copia`.** Nenhum dos onze casos seleciona esse modo. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:110`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:118`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:122`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:126`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:130`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:134`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:138`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:142`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:146`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:150`)
- **Acrescentar uma segunda consulta de auth/sombra/casca depois do apply.** Os falsos mantêm a mesma resposta durante toda a corrida; os vermelhos já encerraram antes, e os verdes continuam verdes. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:54`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:55`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:56`)
- **Reiniciar somente `saas_core`.** `N_RST` conta chamadas, não alvos; restart e rotas falsos continuam verdes. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:70`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:101`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:151`)
- **Retirar provas de workers/readiness/rotas, ignorar falha de apply/restart ou retirar o selo de reciclagem.** As asserções não exigem essas provas, os comandos relevantes não oferecem falha na fixture, e o selo é substituído por sucesso constante. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:51`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:70`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:119`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:151`)

5) Para as classes históricas, distinguindo alteração nova de problema já presente:

| Classe | Sim/não e prova |
|---|---|
| Segundo juiz que duplica juiz registrado | **Não no diff.** Deploy continua perguntando à auth existente; o selo substitui respostas, sem reproduzir a regra de horário. (`bin/deploy.sh:145`, `bin/janela_auth.sh:53`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:54`) |
| Segundo escritor fora da porta | **Sim no ciclo de vida das marcas, já existente:** envelope escreve/remove e deploy também remove. **Não aparece novo aplicador/publicador no diff.** (`bin/cron_run.sh:10`, `bin/deploy.sh:109`, `bin/deploy.sh:300`, `bin/deploy.sh:326`, `bin/deploy.sh:343`) |
| Relógio lido fora da borda | **Não introduzido pelo diff.** A decisão temporal de auth continua usando suas costuras declaradas; o novo cron falso não consulta relógio. (`bin/janela_auth.sh:49`, `bin/janela_auth.sh:50`, `bin/janela_auth.sh:55`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`) |
| Derivação paralela de fonte única | **Não no diff.** A elegibilidade do apply é decidida no ramo da seção 1; os dois pontos seguintes consomem a mesma variável. (`bin/deploy.sh:204`, `bin/deploy.sh:223`, `bin/deploy.sh:298`, `bin/deploy.sh:325`) |
| Selo/RED sem mordida | **Sim para partes do contrato, não para o selo inteiro.** Há mordida contra a recusa pós-apply, mas falta contra publicação antes do apply e contra a promessa de cron refeito. RED realmente demonstrado: **[nao verificado]**. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:103`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:151`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`) |
| Ordem de trava | **Sim: falta reservar o intervalo.** Observar marcas antes de migrar não impede novos crons entre observação e restart; o envelope marca/executa sem exclusão compartilhada. (`bin/deploy.sh:299`, `bin/deploy.sh:300`, `bin/deploy.sh:325`, `bin/deploy.sh:326`, `bin/cron_run.sh:9`, `bin/cron_run.sh:11`) |
| Pouso antes do pré-requisito | **Sim quanto ao pré-requisito ainda escrito “restart recusa com cron vivo”.** A nova exceção publica sem essa condição no cenário 11. Quanto à ordem core migrate → publicação, **não no código atual**, embora o selo aceite sua inversão. (`CLAUDE.md:90`, `bin/deploy.sh:325`, `bin/deploy.sh:326`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:150`, `bin/deploy.sh:300`) |
| Campo com dois significados | **Não demonstrado nos flags alterados.** O novo uso de `APLICAR_MIGRATE` depende corretamente do fluxo anterior; os limites das marcas e dos contadores estão discriminados abaixo. (`bin/deploy.sh:223`, `bin/deploy.sh:298`, `bin/deploy.sh:300`, `bin/deploy.sh:325`) |

O censo semântico das variáveis lidas/escritas pelas linhas alteradas é:

| Campos/variáveis | Concordância entre escritores e leitores |
|---|---|
| `APLICAR_MIGRATE` | `1` nasce como intenção elegível. Ao alcançar o leitor da seção 2, o comando já retornou sucesso, pois falha termina o script. Logo a inferência nesse leitor é válida; não prova alteração material do banco. O comentário de “UM leitor” está errado. (`bin/deploy.sh:223`, `bin/deploy.sh:300`, `bin/deploy.sh:92`, `bin/deploy.sh:325`, `bin/deploy.sh:59`) |
| `RESTART`, `RESTART_MOTIVO` | Significam seleção manual e justificativa textual. Escrita e leituras concordam; o motivo não prova necessidade técnica do boot. (`bin/deploy.sh:70`, `bin/deploy.sh:73`, `bin/deploy.sh:74`, `bin/deploy.sh:75`, `bin/deploy.sh:322`, `bin/deploy.sh:429`) |
| `FAKE_CRON_NO_MIGRATE`, `FAKE_HERDA_APLICAR` | São habilitadores por **presença não vazia**, não booleanos numéricos gerais. Os casos fornecidos usam `1`, coerentemente. (`bin/tests/test_deploy_portoes_antes_do_migrate.s

[truncado em 20000 de 22901 caracteres]

## 08/11 23:35 · sessao 4ff4663f72a0 · FIO-E0-V4-E1-FILA · verificador · origem=Code

**A captura está no lugar correto para isolar a recusa da migração, mas não torna atômico o ato maior dos chamadores. Há retornos `False` ignorados e contadores que anunciam trabalho recusado.** (`app/chamados/models.py:1414-1425`; `app/chamados/services/adesao.py:92`; `app/ponto/management/commands/reconciliar_grade.py:254-259`, `:529-531`.)

Análise estática da cópia. Execução dos testes, comportamento efetivo do banco e ocorrências em produção: **[nao verificado]**. [nao verificado]

**1. Quem chama `ChamadoColaborador.reabrir`**

No quadro, “sem atomic” significa sem envoltório explícito no caminho identificado. Os dicionários de banco da cópia não configuram `ATOMIC_REQUESTS`; configuração efetiva de produção: **[nao verificado]**. (`app/config/settings/prod.py:9-18`; `app/config/settings/saas.py:83-92`.)

| Chamador produtivo direto | Atomic externo à migração | Tratamento de `False` |
|---|---|---|
| `_renascer_por_premissa_viva` — `app/chamados/models.py:1019` | **Sim via `abrir`**, que abre atomic antes de chamar o renascimento. **Sem atomic identificado via resposta tardia**. (`app/chamados/models.py:902`, `:922`; `app/chamados/services/resposta_tardia.py:31-45`.) | Propaga `False`. `abrir` descarta esse resultado e retorna `(existente, False)`: esse segundo booleano significa **criado**, não reaberto. Resposta tardia registra nota/log e devolve `tardia`. (`app/chamados/models.py:1019-1024`, `:922-924`; `app/chamados/services/resposta_tardia.py:45-67`.) |
| Desarquivar — `app/chamados/views_arquivar.py:70` | Sem atomic identificado. (`app/chamados/views_arquivar.py:51-70`.) | Retorna `ok=False`, mas atribui **qualquer recusa a irmão vivo**. Para universo alterado, apresenta razão incorreta. (`app/chamados/views_arquivar.py:70-72`.) |
| Adesão — `app/chamados/services/adesao.py:92` | Sem atomic identificado no serviço/comando. (`app/chamados/services/adesao.py:44-92`; `app/escala/management/commands/apurar_furos_diarios.py:6-21`.) | **Ignora e incrementa `pais_reabertos` incondicionalmente**; o comando apresenta esse contador. (`app/chamados/services/adesao.py:92`; `app/escala/management/commands/apurar_furos_diarios.py:21`.) |
| Reabrir disputa administrativamente — `app/chamados/services/acoes_disputa.py:267` | Sem atomic identificado no serviço, dispatcher ou views de entrada. (`app/chamados/services/acoes_disputa.py:232-296`; `app/chamados/services/fio.py:583`; `app/chamados/views.py:658-674`, `:868-954`.) | **Ignora**; segue com ocorrência, pushes, auditoria e sucesso. As mensagens anunciam a **disputa**, efetivamente reaberta antes; não revelam que o chamado recusou. (`app/chamados/services/acoes_disputa.py:256-296`.) |
| Reabrir competência — `app/chamados/services/competencia_trancada.py:265` | **Sim**, atomic por chamado. (`app/chamados/services/competencia_trancada.py:264`.) | Só escreve nota/evento e incrementa se `True`; `False` pula esses efeitos. (`app/chamados/services/competencia_trancada.py:265-272`.) |
| Resolver perguntas de chamado resolvido — `app/chamados/services/disputa_emissao.py:1812` | **Sim**: o wrapper aproveita atomic existente ou abre um. O desvio da competência trancada retorna antes de alcançar essa chamada. (`app/chamados/services/disputa_emissao.py:1715-1722`, `:1742-1743`.) | Só anexa nota de reabertura se `True`; segue o processamento depois da recusa. (`app/chamados/services/disputa_emissao.py:1812-1830`.) |
| Reabrir órfãs — `app/chamados/management/commands/reabrir_orfas_enterradas.py:172` | **Sim**, atomic por pergunta. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:146`.) | Descarta o booleano, **relê o estado** e registra `pai_falhou` se continua encerrado; imprime falhas. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:172-189`.) |
| Ausência ainda aberta — `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49` | Sem atomic identificado. (`app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:22-55`.) | Só nota e incremento se `True`; nenhuma explicação própria para `False`. (`app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49-55`.) |
| Reconciliar grade — `app/ponto/management/commands/reconciliar_grade.py:530` | Sem atomic identificado nesse caminho. (`app/ponto/management/commands/reconciliar_grade.py:488-531`.) | **Ignora e retorna `None`**; o chamador entende ausência de veto e incrementa `CREATE`, incluindo a entrada em `creates`. (`app/ponto/management/commands/reconciliar_grade.py:529-531`, `:254-259`.) |
| Detectar ausências/limbo — `app/ponto/management/commands/detectar_ausencias.py:301` | Sem atomic identificado nesse caminho. (`app/ponto/management/commands/detectar_ausencias.py:271-304`.) | Ignora; não anuncia reabertura nem registra explicação própria nesse trecho. (`app/ponto/management/commands/detectar_ausencias.py:300-304`.) |

Para a **nova exceção**, esses chamadores não eliminam a tentativa de trilha: a própria porta chama `evento` antes de devolver `False`. A persistência é best-effort, portanto não há garantia estática de linha gravada. Outras recusas, como `agrupado` e `transicao`, continuam retornando sem evento próprio nesses ramos. (`app/chamados/models.py:1421-1425`, `:1406-1407`, `:1437-1438`; `app/core/observ.py:105-139`.)

**2. Savepoint, conexão utilizável e escritas anteriores**

**Sob atomic externo saudável, sim, por desenho:** a migração entra em `transaction.atomic()` padrão; a exceção sai desse bloco antes de ser capturada por `reabrir`. O rollback pertence ao savepoint interno, preservando o atomic externo. O repositório documenta o mesmo mecanismo de isolamento na implementação de `evento`. Verificação efetiva da conexão após essa sequência: **[nao verificado]**. (`app/chamados/services/fio.py:2074-2094`; `app/chamados/models.py:1414-1417`; `app/core/observ.py:124-133`.)

Isso cobre os caminhos com atomic da tabela: `abrir`, competência, resolver perguntas e reabrir órfãs. Nos demais caminhos, sob autocommit normal, o atomic da migração é a **transação externa**, não savepoint; sua saída excepcional encerra essa transação antes da captura. Configuração operacional de autocommit: **[nao verificado]**. (`app/chamados/models.py:902`; `app/chamados/services/competencia_trancada.py:264`; `app/chamados/services/disputa_emissao.py:1720`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:146`; `app/chamados/services/fio.py:2074`; `app/chamados/models.py:1417`.)

**Não há migração parcialmente escrita nesse ramo:** o `raise` ocorre antes de `_migrar_sob_trava`; até ali há leituras e aquisição de travas, sem alteração de dados de negócio pela migração. (`app/chamados/services/fio.py:2084-2094`; `app/chamados/models.py:2674-2701`.)

**Há atos maiores que permanecem parcialmente realizados:**

- **Admin, sem atomic abrangente:** a disputa é reaberta e nasce pergunta de conversa **antes** da tentativa sobre o chamado; `False` não desfaz essas escritas e não impede os efeitos posteriores. (`app/chamados/services/acoes_disputa.py:256-296`.)
- **Órfãs, mesmo com atomic:** pergunta e disputa já foram reabertas; a recusa do pai vira `pai_falhou`, sem rollback dessas etapas anteriores. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:157-177`.)
- **Competência:** `PeriodoFechado` é salvo antes do gancho best-effort que devolve a fila; os fechamentos mensais são atualizados depois. Não há transação nova para o conjunto. (`app/ponto/services/fechamento.py:1435-1452`.)

Logo, o comentário “o ato inteiro desfez” vale para **a migração**, não para todo trabalho anteriormente executado pelo chamador. (`app/chamados/services/fio.py:2026-2029`, `:2074-2094`; `app/chamados/services/acoes_disputa.py:256-267`.)

**3. Escritores e leitores de `reabertura_recusada` / `depois['motivo']`**

Os emissores produtivos encontrados são:

| Emissor | Conteúdo de `depois` |
|---|---|
| `_trilha_do_veto`, para renascimento/reabertura | `modulo_origem`, `dia`, **`veto`**, `via`, `colaborador`. (`app/chamados/models.py:1166-1188`.) |
| Admin sem fato novo | `status_local`, `via_encerramento`. (`app/chamados/models.py:1396-1398`.) |
| Premissa regularizada | `status_local`. (`app/chamados/models.py:1402-1404`.) |
| Universo alterado sob trava | `irmao`, **`motivo='universo_mudou_sob_trava'`**, `erro`. (`app/chamados/models.py:1421-1424`.) |

O leitor específico encontrado é a **deduplicação do veto**, que exige também `modulo_origem`, `dia`, `veto` e `via`. O novo evento não satisfaz essa chave e não suprime aquela trilha. (`app/chamados/models.py:1172-1183`, `:1424`.)

Não localizei leitor produtivo específico de `motivo` para essa ação; o teste lê explicitamente esse campo. Completude diante de acesso dinâmico/reflexão: **[nao verificado]**. (`app/chamados/tests/test_e1_fila.py:517`.)

Os leitores genéricos relevantes têm estes comportamentos:

- A auditoria consulta todas as ações, filtra ação literal e conta registros, sem classificar motivos. Seu seletor usa `ACOES`, que **não oferece** `reabertura_recusada`; o filtro por GET continua aceitando a ação. (`app/core/views.py:705-738`; `app/core/models.py:286-294`.)
- O vigia de escritas por cron conta **linhas de auditoria**, independentemente da ação. A recusa entra nessa contagem, coerente com o rótulo declarado; não significa reabertura realizada. (`app/chamados/services/vigia_loop.py:68-96`.)
- Os vigias de transição e confronto trilha/banco, e o histórico de estados, filtram `estado_chamado`; não interpretam essa recusa como mudança de estado. (`app/chamados/services/vigia_loop.py:28-34`, `:114-121`; `app/chamados/services/historico.py:44`, `:86-95`.)

Não há prova encontrada de leitor contando errado por presumir um conjunto antigo de motivos. Há **payloads heterogêneos**, sem `motivo` obrigatório em todas as recusas; o sentido comum do evento continua sendo recusa de reabertura. (`app/chamados/models.py:1187`, `:1398`, `:1404`, `:1424`.)

**4. Mordida dos testes novos e mutantes sobreviventes** [nao verificado]

**`A3AplicarDeclaracaoTest`: morde a retirada simples da chamada naquele cenário.** A prova exige presença da pergunta canônica e do destino na sequência, e ausência de inversão; o mutante remove a chamada real e exige inversão. O cenário usa disputa canônica fechada e destino encerrado de PK menor. (`app/chamados/tests/test_e1_fila.py:182-191`, `:436-454`.)

O alcance é menor que uma prova funcional completa: a proposta é mockada, `_r` é ignorado, linhas nascidas durante o ato são excluídas e o rastreador não cobre `executemany` nem travas de FK do INSERT. (`app/chamados/tests/test_e1_fila.py:63-71`, `:438-447`; `app/chamados/tests/test_e1_v9.py:46-53`.)

**`A2UniversoMudouSobATravaTest`: morde a retirada da conferência e as alterações simples no tratamento da recusa.** Retirar o `try/except`, trocar `False`, trocar o motivo ou o irmão viola as asserções explícitas. (`app/chamados/tests/test_e1_fila.py:479-497`, `:508-518`.)

Entretanto, a “corrida” é sintética; o teste de `reabrir` substitui tanto a migração quanto `evento`. Não verifica concorrência real, rollback seguido de escrita no chamador, nem INSERT real da trilha. (`app/chamados/tests/test_e1_fila.py:466-477`, `:499-505`.)

Mutantes que as asserções dessas classes deixam escapar **por inspeção**; sobrevivência executada: **[nao verificado]**:

| Mutante de produção | Por que escapa |
|---|---|
| Executar `_trava_do_ato` em `aplicar_declaracao` **somente para `saida_sem_entrada`**, omitindo outros motivos | A proposta testada sempre usa esse motivo; a retirada simples ainda seria detectada. (`app/chamados/tests/test_e1_fila.py:180`, `:438`, `:451-454`; `app/chamados/services/declaracao_texto.py:650`.) |
| Trocar o retorno final de `aplicar_declaracao` por resultado incorreto, preservando as operações | O resultado `_r` não é conferido. (`app/chamados/services/declaracao_texto.py:690-691`; `app/chamados/tests/test_e1_fila.py:444-447`.) |
| Trocar `if _fora[0] or _fora[1]` por **`and`** | O objeto extra introduz simultaneamente colaborador **e** PK novos; não há casos separados para cada diferença. (`app/chamados/services/fio.py:2089-2090`; `app/chamados/tests/test_e1_fila.py:469-476`.) |
| Suprimir `_migrar_sob_trava`, preservando conferência e travas | A classe não exige migração nominal bem-sucedida nem conteúdo de `out`; os testes herdados verificam aquisição/ordem das travas. (`app/chamados/services/fio.py:2094`; `app/chamados/tests/test_e1_fila.py:158-171`, `:479-497`.) |
| Emitir a recusa com **modelo ou objeto_id errados** | O mock confere ação, quantidade, motivo e irmão; não confere modelo/objeto. (`app/chamados/models.py:1421`; `app/chamados/tests/test_e1_fila.py:515-518`.) |
| Ampliar a captura para `RuntimeError`, engolindo falhas adicionais | Só a exceção específica é injetada. (`app/chamados/models.py:1417`; `app/chamados/tests/test_e1_fila.py:501-504`.) |
| Retirar a implementação real de persistência de `evento`, mantendo a chamada | Nesse teste, `evento` é mockado. (`app/chamados/tests/test_e1_fila.py:504`; `app/core/observ.py:133-138`.) |

A conferência também **não compara mudanças de `disputa_id` em perguntas já conhecidas**, embora esse ID componha a trava inicial. Possibilidade de escritor concorrente produtivo realizar essa mudança: **[nao verificado]**. (`app/chamados/services/fio.py:2086-2089`.)

**5. Outra porta capaz de levantar `UniversoMudouSobATrava`**

**Nenhuma outra porta produtiva nominal encontrada fora de `reabrir`.** O `raise` está em `migrar_resolvedoras_vivas`; a chamada produtiva identificada está no ramo `irmao_vivo`, com captura específica. Os caminhos indiretos são os chamadores da pergunta 1. Chamadas dinâmicas em execução: **[nao verificado]**. (`app/chamados/services/fio.py:2091`; `app/chamados/models.py:1411-1417`.)

**6. Classes de reprovação**

O quadro distingue o delta desta volta dos resíduos relacionados na cópia; ausência universal na árvore ou no ambiente não está provada. **[nao verificado]**

| Classe | Constatação com prova |
|---|---|
| **Segundo juiz duplicando juiz registrado** | **Não identificado no delta.** A captura traduz falha técnica em recusa; a migração continua consultando `pergunta_viva`, autoridade registrada. (`app/chamados/models.py:1417-1425`; `app/chamados/services/fio.py:2108-2119`; `app/core/juizes.py:266`.) |
| **Segundo escritor fora da porta** | **Não identificado no delta.** O novo evento passa pelo escritor de auditoria `evento`; não acrescenta escrita direta de estado. (`app/chamados/models.py:1421-1425`; `app/core/observ.py:94-138`.) |
| **Relógio fora da borda** | **Sim, residual na cadeia do juiz:** `_dentro_da_janela_viva` ainda lê `date.today()`. A cura de `expira_em` usa `timezone.now`, mas não elimina essa leitura do juiz. (`app/chamados/juizes.py:569-575`; `app/chamados/veredito_lavrado.py:95-102`.) |
| **Derivação paralela de fonte única** | **Sim, fórmula temporal reproduzida:** `expira_em` reimplementa o intervalo da janela para derivar sua expiração. Divergência observada entre as duas implementações: **[nao verificado]**. (`app/chamados/veredito_lavrado.py:117-143`; `app/chamados/juizes.py:575-601`.) |
| **Selo/RED sem mordida** | **Não para os alvos estreitos declarados; insuficiente para garantias amplas.** Há mutantes explícitos de remoção, mas integração transacional/persistência é substituída por mocks e sobrevivem os mutantes listados acima. (`app/chamados/tests/test_e1_fila.py:449-454`, `:489-518`.) |
| **Ordem de trava** | **Ordem local implementada; ordem de toda chamada composta [nao verificado].** Há chamadores que já seguram C ou C/D/Ch/P antes de a migração adquirir seu conjunto, inclusive colaboradores adicionais. A prova nova não percorre essas composições. (`app/chamados/models.py:905`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:153-156`; `app/chamados/services/disputa_emissao.py:1721`, `:1787`, `:1812`; `app/chamados/services/fio.py:2084-2087`; `app/chamados/tests/test_e1_fila.py:499-505`.) |
| **Pouso antes do pré-requisito** | **Pouso/deploy [nao verificado].** Na ordem do ato administrativo, há efeitos anteriores à confirmação de reabertura do chamado, e o fluxo continua após recusa. (`app/chamados/services/acoes_disputa.py:256-296`.) |
| **Campo com dois significados** | **Sim nos contadores:** `pais_reabertos` pode contar tentativa recusada; `CREATE` pode incluir emissão cujo chamado permaneceu encerrado. **Não demonstrado em `depois['motivo']`**, que tem um único valor emitido identificado. (`app/chamados/services/adesao.py:92`; `app/ponto/management/commands/reconciliar_grade.py:254-259`, `:529-531`; `app/chamados/models.py:1424`.) |

## 08/11 23:34 · sessao 7f2da60deb0c · FIO-E0-V4-E1-FILA · censo · origem=Code

Nos anexos, conto as linhas desde a primeira linha de cada bloco, incluindo linhas vazias e excluindo o cabeçalho `== anexo: ... ==`.

| Achado | Classe | Trecho literal da pergunta | Trecho literal da resposta | Linha de triagem |
|---|---|---|---|---|
| **1 — Falta de RED próprio para `aplicar_declaracao`** | **(a), na leitura de cobertura:** o tema dos testes estava perguntado; a resposta reconheceu a porta, mas não apontou sua falta de teste. (`codex_e1fila.q:7`; `codex_e1fila.r:72`) | “Os testes novos (test_e1_fila.py) mordem o que dizem morder?” (`codex_e1fila.q:7`). Na volta 2: “selo/RED sem mordida” (`codex_e1fila_v2.q:9`). | Sobre `aplicar_declaracao`: “Mesmo prefixo no atomic da emissão/resposta.” (`codex_e1fila.r:72`). A avaliação de A3 aponta outra lacuna: “Não exige a Disputa fechada na sequência.” (`codex_e1fila.r:31`). **Esses trechos não denunciam a ausência de caso para a porta irmã.** | Não há descarte específico dessa ausência. O registro próximo diz: “o ramo do rechaveamento por dia diferente continua sem caso proprio (fila)” — outro problema, não a cobertura de `aplicar_declaracao`. (`triagem_e1_fila.txt:29`, `triagem_e1_fila.txt:30`) | [nao verificado]
| **2 — `UniversoMudouSobATrava`: sem teste, observabilidade e tratamento produtivo** | **(b), pelo critério solicitado de algum trecho tocar o achado:** Codex viu a falta de teste e a propagação da exceção; a triagem colocou a lacuna de teste em FILA. Isso não demonstra que tenha identificado todos os componentes do bloqueante. (`codex_e1fila.r:30`; `codex_e1fila.r:90`; `triagem_e1_fila.txt:27`) | “Os testes novos [...] mordem o que dizem morder?” (`codex_e1fila.q:7`); “ou regra de negocio nova?” (`codex_e1fila.q:8`). Na volta 2: “Os descartes e as linhas de FILA da triagem: algum deles e procedente para ESTA obra” (`codex_e1fila_v2.q:7`). | A2: “Não testa crescimento do universo entre leitura e trava, nem entrada por chamador já travado.” (`codex_e1fila.r:30`). Item 8: “crescimento detectado aborta o atomic por exceção, propagada ao chamador, sem retry nessa porta.” (`codex_e1fila.r:90`). | “A2 nao testa crescimento do universo nem chamador ja travado -> FILA (UniversoMudouSobATrava sem teste de corrida; o chamador travado e o item 5).” **Não apresenta justificativa própria para adiar o teste de crescimento.** (`triagem_e1_fila.txt:27`, `triagem_e1_fila.txt:28`) | [nao verificado]

**Limites da classificação:**

- **Achado 1:** adoto “tema presente” como cobertura dos testes nas portas tocadas. Mencionar a trava de `aplicar_declaracao` não equivale a detectar seu RED ausente; também não autoriza atribuir à triagem um descarte que ela não registrou. A afirmação experimental “apagar a chamada sobrevive à suíte inteira” é **(c)** quanto à confirmação por execução nesta auditoria: **[nao verificado]**. (`codex_e1fila.q:5`; `codex_e1fila.q:7`; `codex_e1fila.r:72`; `triagem_e1_fila.txt:29`) [nao verificado]
- **Achado 2:** o **(b) é parcial**. A resposta registra propagação, mas não denuncia literalmente “sem trilha nem contador”; tampouco há descarte específico desses componentes. Os motivos “sem ocorrência verificada” e mudança de desenho pertencem ao problema do **chamador já travado**, não constituem justificativa expressa para deixar a exceção crua ou sem observabilidade. (`codex_e1fila.r:90`; `triagem_e1_fila.txt:27`; `triagem_e1_fila.txt:59`)
- A cópia atual já contém alterações identificadas como curas posteriores desses bloqueantes; elas não provam o que foi detectado nas duas verificações anteriores. ([test_e1_fila.py](/home/ronald/codex_ws/602a7d739b34f2a8524a4210bb4423a807f40331/app/chamados/tests/test_e1_fila.py:430); [models.py](/home/ronald/codex_ws/602a7d739b34f2a8524a4210bb4423a807f40331/app/chamados/models.py:1418))

## 08/11 23:33 · sessao 0f34790ec36d · - · - · origem=Ronald (tmux)

Confirmado por inspeção: **mover todo o bloco 1c para depois da seção 2 sobrevive aos 11 casos**; execução [nao verificado]. (`bin/deploy.sh:298-301`, `:358`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:110-152`)

- `ORDEM_OK` exige somente auth/sombra/casca **antes do apply**; não compara apply com HUP/restart. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:95-104`)
- O caso 3 aceita migrate=1 e HUP=1, independentemente da ordem entre eles. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:119`)
- No caso 11, `APLICAR_MIGRATE=1` já dispensa a recusa da seção 2; o restart falso acontece primeiro, depois o migrate cria a marca. A asserção aceita as mesmas contagens e rc=0. (`bin/deploy.sh:223`, `:325-326`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`, `:76`, `:150-152`)

Assim, o mutante publica antes do pré-requisito de schema e transforma “cron nascido durante migrate antes do restart” em “marca criada depois do restart”, sem ser denunciado pelas asserções. (`bin/deploy.sh:24-28`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:95-104`, `:151`)

## 08/11 23:32 · sessao 3dbe97426882 · - · - · origem=Ronald (tmux)

Censo estático: **10 chamadas produtivas diretas** a `ChamadoColaborador.reabrir`; não inclui `ponto.services.fechamento.reabrir`, que opera `FechamentoMensal` (`app/ponto/services/fechamento.py:1349`).

| Chamador | Atomic externo à migração | O que faz com False |
|---|---|---|
| `_renascer_por_premissa_viva` | Via `abrir`, sim: atomic `app/chamados/models.py:902`, chamada transitiva `:922`. Via resposta tardia, nenhum atomic local: `app/chamados/services/resposta_tardia.py:31`, `:45`; view `app/chamados/views.py:1039`, `:1053`. | Propaga False em `app/chamados/models.py:1019`. `abrir` descarta esse bool e retorna `(existente, False)`, cujo segundo elemento significa “criado”, não “reaberto” (`:922–924`). Resposta tardia faz nota/log/Pauta e devolve estado `tardia`, sem anunciar reabertura (`app/chamados/services/resposta_tardia.py:45`, `:52`, `:54`, `:58`, `:67`). |
| Desarquivar | Sem atomic local, view apenas `login_required` (`app/chamados/views_arquivar.py:51–70`). | Devolve JSON `ok=False`, mas texto atribui **qualquer recusa** a irmão vivo (`:70–72`); para universo mudou, razão exibida é errada. |
| Sincronizar adesão | Sem atomic local; comando chamador também não envolve atomic (`app/chamados/services/adesao.py:44`, `app/escala/management/commands/apurar_furos_diarios.py:6–21`). | **Conta como reaberto mesmo False**: `pai.reabrir(); r['pais_reabertos'] += 1` (`app/chamados/services/adesao.py:92`), depois imprime contador (`app/escala/management/commands/apurar_furos_diarios.py:21`). |
| Reabrir disputa admin | Sem atomic no serviço (`app/chamados/services/acoes_disputa.py:232–296`), no dispatcher (`app/chamados/services/fio.py:485`, `:583`) ou nas duas views de entrada (`app/chamados/views.py:658–674`, `:868–954`). | **Ignora False** (`app/chamados/services/acoes_disputa.py:267`); anexa ocorrência, envia pushes e responde sucesso (`:271`, `:275`, `:287`, `:296`). A mensagem fala da disputa, efetivamente reaberta, mas o chamado pode continuar encerrado. |
| Reabrir competência | Sim, atomic por chamado (`app/chamados/services/competencia_trancada.py:264`). | Só nota/log/incremento se True (`:265–272`); False pula efeitos, não levanta erro. |
| Resolver perguntas por chamado resolvido | Sim: wrapper usa atomic existente ou abre um (`app/chamados/services/disputa_emissao.py:1720–1722`). Ramo da tranca sai antes de chegar à chamada (`:1715–1716`, `:1742–1743`). | Só anexa nota de reabertura se True (`:1812–1814`). |
| Comando reabrir órfãs | Sim, atomic por pergunta (`app/chamados/management/commands/reabrir_orfas_enterradas.py:146`). | Ignora bool, mas relê estado, registra `pai_falhou` ou incrementa reabertos (`:172–177`), imprime falhas (`:185–189`). **Não desfaz** pergunta/disputa já reabertas (`:157–167`). |
| Comando ausência aberta | Sem atomic local (`app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:22–55`). | Só nota e incremento se True (`:49–54`). |
| Reconciliar grade | Sem atomic no caminho `_emitir_furo` (`app/ponto/management/commands/reconciliar_grade.py:488–531`); atomic `:472` é de outro método e termina antes. | **Ignora False**, retorna None (`:530–531`). Chamador interpreta None como ausência de veto e incrementa `plan['CREATE']` / `creates` (`:254–259`). |
| Detectar ausências, limbo | Sem atomic no caminho do comando (`app/ponto/management/commands/detectar_ausencias.py:61`, `:271–303`). | Ignora False (`:301`), sem contador específico nem aviso da recusa nesse chamador (`:300–304`). |

**Fronteira transacional:** `migrar_resolvedoras_vivas` abre `transaction.atomic()` com configuração padrão (`app/chamados/services/fio.py:2074`); levanta em `:2091` **antes** de executar `_migrar_sob_trava` em `:2094`. Antes da recusa há leituras e `select_for_update`; a função de trava não grava dados de negócio (`app/chamados/models.py:2674`, `:2683`, `:2696`, `:2700`). `reabrir` captura **depois da saída** desse atomic (`app/chamados/models.py:1415–1417`). Sob atomic externo saudável, a conclusão contratual é rollback ao savepoint e conexão utilizável; o próprio repositório documenta esse mecanismo em `app/core/observ.py:124–133`. Sem atomic externo, esse bloco abre uma transação e a encerra com rollback; a ocorrência da recusa entra depois em outro atomic próprio (`app/chamados/models.py:1421`, `app/core/observ.py:133`). Validação efetiva da conexão/Django em execução: **[nao verificado]**.

Configurações da cópia não ativam `ATOMIC_REQUESTS` nem desativam autocommit nos dicionários de produção/on-prem e SaaS (`app/config/settings/prod.py:9–18`, `app/config/settings/saas.py:83–92`); configuração efetiva carregada em produção: **[nao verificado]**.

**Há escrita anterior que fica:** no fluxo admin, `d_fec.reabrir_admin` salva a disputa (`app/chamados/services/acoes_disputa.py:256`, `app/chamados/models.py:2169`), abre Pergunta de conversa (`app/chamados/services/acoes_disputa.py:260`) e só depois tenta reabrir o chamado (`:267`); a recusa não desfaz essas escritas, nem bloqueia os efeitos posteriores (`:271–296`). No modal ainda há marcação de notas lidas antes do dispatcher (`app/chamados/views.py:879–880`, `:954`). Isso é parcialidade do **ato maior**, não migração pela metade (`app/chamados/services/fio.py:2091–2094`).

Mesmo com savepoint externo há parcialidade de negócio intencional/concreta: órfãs já alteraram Pergunta/Disputa quando pai recusa (`app/chamados/management/commands/reabrir_orfas_enterradas.py:157–175`); competência grava `PeriodoFechado` antes de tentar devolver chamados, em gancho best-effort, e depois reabre fechamentos mensais (`app/ponto/services/fechamento.py:1435–1452`).

A trilha de `universo_mudou_sob_trava` é tentada pela própria porta (`app/chamados/models.py:1421–1425`), inclusive nos chamadores que ignoram False. Persistência não é garantida: `evento` é best-effort e retorna None se falhar (`app/core/observ.py:94–105`, `:133–139`); sucesso real do INSERT sem banco/log: **[nao verificado]**.

**Lacuna dos testes para esta pergunta:** o teste de `reabrir` substitui migração por `side_effect` e substitui `evento` por mock (`app/chamados/tests/test_e1_fila.py:499–505`); verifica False e argumentos (`:514–518`), mas não executa a integração rollback/savepoint → INSERT da trilha → escrita posterior do chamador. Essa garantia integrada permanece **[nao verificado]**.

**Possível ordem de trava transitiva a investigar:** atomic de `abrir` já segura um Colaborador (`app/chamados/models.py:905`), órfãs já seguram C/D/Ch/P (`app/chamados/management/commands/reabrir_orfas_enterradas.py:153–156`), resolver já trava universo e pode ter escrito Pergunta antes de reabrir pai (`app/chamados/services/disputa_emissao.py:1721`, `:1787`, `:1812`). A migração então pode adquirir conjunto adicional de Colaboradores (`app/chamados/services/fio.py:2084–2087`), inclusive de pergunta alheia, caso expressamente admitido em comentário (`:2080–2081`). A ordem local ordenada não prova a ordem da transação inteira; existência/alcançabilidade de dados que realizem esse caminho: **[nao verificado]**.

## 08/11 23:31 · sessao ec542bc3f363 · - · - · origem=Ronald (tmux)

No escopo do diff, com distinção entre defeito novo e limite já existente:

| Classe | Veredito e prova |
|---|---|
| Segundo juiz duplicando juiz registrado | **Não.** O novo teste substitui auth/sombra/casca por respostas falsas, sem reimplementar suas regras; deploy continua chamando `janela_auth.sh`. O juiz de horário permanece `proibido()`. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:54-56`; `bin/deploy.sh:145`; `bin/janela_auth.sh:53-61`. |
| Segundo escritor fora da porta | **Não introduzido.** `APLICAR_MIGRATE=0` inicializa estado local e `=1` decide apply; não há publicador novo: seguem um restart e um HUP. A exclusão de marca morta por deploy é escritor de limpeza **já existente**, além do envelope produtor/removedor. `bin/deploy.sh:61,108-109,223,326,343`; `bin/cron_run.sh:9-10`. |
| Relógio fora da borda | **Não introduzido.** O diff não acrescenta leitura de relógio; auth concentra `date` nas costuras `_dow/_hhmm`, com substituição explícita de teste. O caso 11 usa `/proc/1`, não relógio. `bin/janela_auth.sh:45-56`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`; `bin/deploy.sh:109`. |
| Derivação paralela de fonte única | **Não no patch.** A decisão de aplicar continua derivada do `--check` e modo, em um ramo; a seção 2 consome a mesma variável. Não nasce regra auth paralela. `bin/deploy.sh:184-188,204-224,298-300,325`; `bin/deploy.sh:145`. |
| Selo/RED sem mordida | **Não é inteiramente sem mordida**, mas **sim, há lacunas relevantes**. Caso 11 mata a recusa incondicional pós-apply, porém só cria `cron_falso.1`; o Docker falso não mata nem reexecuta cron. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68,76,150-152`. RED efetivamente executado nesta volta: **[nao verificado]**. |
| Ordem de trava | **Sim.** Checar cron antes de migrate e depois dispensar a checagem não impede admissão concorrente: envelope marca e executa sem coordenação com deploy. `bin/deploy.sh:299-300,325-326`; `bin/cron_run.sh:9-11`. Ausência de cron no instante observado não reserva o intervalo. |
| Pouso antes do pré-requisito | **Sim, pela regra ainda escrita “restart recusa com cron vivo”.** Depois de migrate, o novo ramo permite restart apesar da marca recém-criada, contrariando a promessa vigente. `CLAUDE.md:90`; `bin/deploy.sh:325-326`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:150-152`. **Outro resto do contrato literal “todo portão antes de migrate”:** selo de reciclagem continua posterior a migrate/reload e pode reprovar por configuração/crontab. `bin/deploy.sh:300,326,343,409`; `bin/selo_reciclagem.sh:46-78`. |
| Campo com dois significados | **Não demonstrado em `APLICAR_MIGRATE`, apesar da prosa incorreta.** Na escrita `=1` significa “apply planejado”; no leitor `:325`, o controle de fluxo permite inferir “apply retornou 0”, pois falha em `:300` termina por `falhou`, definido em `:92`. Portanto o novo leitor tem pré-condição válida; não lê intenção como sucesso antes de executar. `bin/deploy.sh:92,223,298-300,325`. O comentário “UM leitor” é falso: há dois, `:298` e `:325`, além de dois pontos de atribuição, `:61` e `:223`. |

Outras observações úteis para consolidar:

- O selo estrutural antigo só busca o texto `recusa_restart_com_cron` antes do restart; a linha nova mantém esse texto mesmo quando o curto-circuito dispensa sua execução. Assim, esse selo aceita a perda da garantia universal. `bin/contrato_deploy.sh:45-47`; `bin/deploy.sh:325`.
- O teste de reload gracioso verifica cron existente **no começo**; não verifica o nascido durante migrate. `bin/tests/test_deploy_reload_gracioso.sh:125-131`.
- “Todo cron é idempotente” é requisito escrito, não prova de reexecução: o envelope executa uma vez, mede RC, carimba somente 0/2 e alerta falha; não contém repetição automática do comando interrompido. `CLAUDE.md:246`; `bin/cron_run.sh:11-13,22-25,29-38`.
- Semântica dos campos novos do teste: `FAKE_CRON_NO_MIGRATE` é presença habilitadora, `FAKE_HERDA_APLICAR` injeta ambiente, `N_RST` conta linhas de chamadas restart. Seus escritores e leitores concordam nos usos fornecidos, mas `N_RST=1` não prova três cascas reiniciadas nem cron preservado. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68,90,101,146,150-151`.
- A marca de cron registra PID do **envelope do host**, não identidade do processo `docker exec`; deploy usa apenas existência de `/proc/<pid>`, sem conferir identidade ou conteúdo do arquivo. Isso admite falso “vivo” por PID reutilizado e não é novidade do diff. `bin/cron_run.sh:9-11`; `bin/deploy.sh:108-109`.

_2946 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
