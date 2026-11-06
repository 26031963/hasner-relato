# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 20:05** · publicado em **06/11/2026 20:06** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 20:05 · sessao 8a7c282839f5 · ESMERIL-SEM-INSTANTE · verificador · origem=Code

**Não encontrei bloqueante novo no delta fornecido; os três achados procedentes foram corrigidos.** (`app/ponto/tests/test_composicao_de_fatias_e_total.py:281`, `:333`; `app/ponto/tests/test_contract_no_batida_date.py:72`)

1. **As três curas conferem.**

   - **Teste 3:** agora exige exatamente `[utc[0], utc[1], utc[3]]` em A. Revertendo a conversão para `b.timestamp.date()`, A recebe `[utc[2], utc[4]]`: o novo assert falha mesmo isolado da igualdade anterior. Se o filtro entregar `[]`, também falha; se ambos os resultados forem `{}`, ocorre `KeyError` em `A.pk`, portanto não passa silenciosamente. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:325`, `:330`, `:333`; `app/ponto/services/fechamento.py:204`, `:254`, `:261`)
   - **Gravador:** deixou de ordenar e registra a sequência recebida pelo motor. Está curado o mascaramento introduzido pelo próprio capturador. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:279`, `:281`)
   - **ALLOWLIST:** a justificativa agora aponta precisamente para `_com_batida`, que ainda contém `timezone.localtime(b.timestamp).date()` e é reconhecido pelo visitante. (`app/ponto/tests/test_contract_no_batida_date.py:72`, `:208`; `app/ponto/services/fechamento.py:793`)

2. **Não há relógio solto, import morto novo ou dependência indevida de ordem nas alterações fornecidas.** Os instantes são literais, o fuso fica sob `override`, e a ordem esperada vem da lista entregue explicitamente pelo teste. As compreensões da porta preservam essa sequência; não dependem da ordem de conjuntos ou de empates no banco. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:245`, `:325`, `:327`, `:328`, `:333`; `app/ponto/services/fechamento.py:204`, `:253`, `:260`, `:262`)

   **Limite da cobertura:** o gravador foi corrigido, mas o teste ainda não prova preservação de uma entrada arbitrariamente embaralhada: os três sobreviventes já estão em ordem cronológica. Uma ordenação indevida na porta ainda poderia passar neste caso. Isso não invalida a cura do capturador. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:281`, `:325`, `:326`, `:333`)

3. **Não mantenho os descartes como bloqueantes desta fatia.** A restrição para naive existe, mas os três caminhos encontrados carregam listas pelo ORM, com `DateTimeField` e `USE_TZ=True`; não há origem naive demonstrada nesses caminhos. Isso sustenta o contrato aware sem provar os valores efetivamente recebidos em produção **[nao verificado]**. (`app/ponto/services/fechamento.py:633`; `app/ponto/services/espelho.py:1022`; `app/relatorios/pdf_espelho.py:272`; `app/ponto/models.py:23`; `app/config/settings/base.py:95`; `app/ponto/turnos.py:44`)

   `None` continua inválido: a conversão devolve `None`, que falha na comparação com datas; não há tratamento novo que prometa aceitá-lo. (`app/ponto/turnos.py:46`; `app/ponto/services/fechamento.py:254`)

4. **Nenhum bloqueante novo identificado.** O residual de saída após meia-noite sob células alternadas permanece: o filtro ainda pode excluir a saída pelo dia civil de outro vínculo. A correção desse residual não é demonstrada por estes asserts. (`app/ponto/services/fechamento.py:255`, `:261`; `app/ponto/tests/test_composicao_de_fatias_e_total.py:333`)

**Validação:** em memória, com dependências substituídas, os três testes passaram na cópia e falharam nos mutantes `.date()` e filtro vazio; o assert novo também falhou isoladamente. Os quatro testes do contrato passaram isolados. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:291`, `:306`, `:316`, `:333`; `app/ponto/tests/test_contract_no_batida_date.py:236`)

Suíte Django real, correspondência Git com o hash/base e integralidade do delta: **[nao verificado]**. [nao verificado]

## 06/11 20:05 · sessao 2d4a03cfa6b6 · - · - · origem=Ronald (tmux)

**(b) fecha a exceção na triagem, mas não garante uma pergunta posterior.** O retorno de `registrar_batida` pode ser a mesma chave já retratada, sem criação ou `save`; `_T1` não inclui retratadas e devolve `None`; a nova guarda evita `.isoformat()` nesse retorno. Referências: `app/ponto/registro_batida.py:116`, `app/ponto/registro_batida.py:120`, `app/ponto/turnos.py:2905`, `app/ponto/turnos.py:2917`, `app/ponto/turnos.py:2925`, `app/ponto/services/triagem_batida.py:726`, `app/ponto/services/triagem_batida.py:730`.

**Outro emissor que retome especificamente essa tentativa/batida retratada: nao achei.** O retorno antecipado de mesma chave não dispara os sinais de criação, e a triagem segue até informar sucesso; a nova guarda não grava pendência nem emite log próprio. Referências: `app/ponto/registro_batida.py:120`, `app/ponto/services/triagem_batida.py:730`, `app/ponto/services/triagem_batida.py:757`.

Há emissores independentes capazes de perguntar **pelo dia**, sob condições: vigia de hora declarado a cada dez minutos e cartório diário; o cartório chama a fábrica depois do julgamento, e a fábrica exige célula elegível, veredito acusador e motivo ainda sem pergunta. Portanto, não cabe afirmar “fica sempre invisível”, nem “o cron garante recuperação”. Referências: `app/config/crons.py:367`, `app/config/crons.py:616`, `app/ponto/services/cartorio.py:981`, `app/ponto/services/cartorio.py:997`, `app/chamados/services/fabrica_da_celula.py:91`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:164`.

Limites concretos: `julgar_celula` recusa célula inexistente; vigia de hora seleciona `trabalha=True`; a fábrica também pode calar por parcial/lista; o lote `disparar_perguntas_competencia` apenas conta o caminho da célula e só emite no caminho de chamado vivo já existente com célula. Referências: `app/ponto/services/cartorio.py:1976`, `app/ponto/services/vigia_de_hora.py:21`, `app/chamados/services/fabrica_da_celula.py:102`, `app/chamados/management/commands/disparar_perguntas_competencia.py:86`, `app/chamados/management/commands/disparar_perguntas_competencia.py:141`, `app/chamados/management/commands/disparar_perguntas_competencia.py:158`. Recuperação efetiva em produção: **[nao verificado]**.

**Censo `DisputaSupervisao.contexto['saida_sem_entrada']`:**

| Papel | Evidência |
|---|---|
| PWA fornece `data` de `_T1` e hora da tentativa | `app/ponto/services/triagem_batida.py:734`, `app/ponto/services/triagem_batida.py:739` |
| API e clone montam data civil transitória e a substituem por `_T1` antes de persistir | `app/api/views.py:655`, `app/api/views.py:780`, `app/api/views_core.py:652`, `app/api/views_core.py:780` |
| Fábrica fornece data explícita da célula, pelo motivo | `app/chamados/services/fabrica_da_celula.py:139` |
| Agregador persiste/mescla o contexto | `app/chamados/services/disputa_emissao.py:518`, `app/chamados/services/disputa_emissao.py:532`, `app/chamados/services/disputa_emissao.py:560` |
| Texto lê `data` para perguntar entrada naquele dia | `app/chamados/catalogo/query.py:58`, `app/chamados/catalogo/perguntas.py:57` |
| Gerador lê `data` como dia da pergunta datada | `app/chamados/services/disputa_emissao.py:364` |
| Seleção de chamado compara `data` com dia do fato | `app/chamados/services/disputa_emissao.py:191`, `app/chamados/services/disputa_emissao.py:231` |
| Materializador usa como fallback do dia do turno | `app/chamados/services/materializacao.py:343`, `app/chamados/services/materializacao.py:346` |
| Juiz de completude usa como fallback do turno | `app/chamados/juizes.py:363`, `app/chamados/juizes.py:365`, `app/chamados/juizes.py:372` |

**Leitor atual desse `data` especificamente como dia civil da saída: nao achei.** A conversão de overnight usa dia do turno e marcos; o recuo antigo por `hora_tentativa` está removido. Referências: `app/chamados/juizes.py:301`, `app/chamados/juizes.py:312`, `app/chamados/juizes.py:319`, `app/chamados/services/materializacao.py:419`.

Duas ressalvas preexistentes: API/clone acrescentam motivo `orfao_14h`, mas gravam a chave de contexto `saida_sem_entrada`; ambos continuam chamando `.isoformat()` sem guarda de `None`. Não são mudanças do delta fornecido. Referências: `app/api/views.py:654`, `app/api/views.py:655`, `app/api/views.py:782`, `app/api/views_core.py:651`, `app/api/views_core.py:652`, `app/api/views_core.py:782`.

## 06/11 20:04 · sessao be24a03c3f1c · - · - · origem=Ronald (tmux)

**Cura (c): fecha a omissão de inserção retroativa recente; não cobre toda reaplicação recente de pergunta.** O filtro passa a usar `criado_em`, preservando os demais critérios, e consulta o mesmo juiz `dia_das_batidas`; não achei segundo juiz nem escritor novo nessa mudança. Referências: `app/ponto/management/commands/vigia_plantio_overnight.py:73`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`.

**Escrita de `Batida.criado_em`:**

- É `DateTimeField(auto_now_add=True)`: registra nascimento da linha, enquanto `timestamp` registra instante da marcação. A porta insere por `Batida.objects.create`. Referências: `app/ponto/models.py:23`, `app/ponto/models.py:79`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`.
- Reescritor Python específico por atribuição/update/SQL/cópia/restauração de `Batida.criado_em`: **nao achei**. A porta de anotação não aceita esse campo e salva somente os campos pedidos. Referências: `app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:241`, `app/ponto/registro_batida.py:254`.
- Existe carga sintética em lote, restrita a banco de ensaio, construída com os dicionários recebidos. Referências: `app/ponto/registro_batida.py:316`, `app/ponto/registro_batida.py:326`, `app/ponto/registro_batida.py:331`, `app/ponto/registro_batida.py:335`.
- Há restauração genérica de schema: backup por `pg_dump`, restauração por `pg_restore --clean --if-exists`. Esse caminho restaura o valor do dump, sem passar pelo `auto_now_add`; não carimba a hora do restore. Uso efetivo e valores restaurados em produção: **[nao verificado]**. Referências: `scripts/backup_tenant.sh:10`, `scripts/restore_tenant.sh:8`.

**Reabertura/reaproveitamento:**

- Materialização procura batida existente antes de criar; aceita tanto real quanto de disputa. Se reaproveita, apenas pode mudar `pergunta_origem`, mantendo `criado_em` e `origem`. Referências: `app/chamados/juizes.py:230`, `app/chamados/juizes.py:238`, `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:847`, `app/chamados/services/materializacao.py:922`, `app/ponto/registro_batida.py:254`.
- A própria porta também pode devolver linha antiga pelo dedup ±60s ou pela mesma chave, sem inserir. Referências: `app/ponto/registro_batida.py:107`, `app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:120`.
- Portanto, **batida realmente criada na nova aplicação tem `criado_em` desse plantio; batida reaproveitada mantém o nascimento original**. Uma linha `disputa_s84_retro` antiga revinculada hoje pode permanecer fora da janela; uma batida `app` reaproveitada continua fora pelo filtro de origem. Isso é limitação do significado “plantio recente”, não regressão criada pela troca do filtro. Referências: `app/ponto/models.py:79`, `app/chamados/services/materializacao.py:903`, `app/chamados/services/materializacao.py:922`, `app/ponto/management/commands/vigia_plantio_overnight.py:16`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`.
- A reabertura do questionário limpa resposta/validação; não renova nascimento da batida. Referências: `app/chamados/services/acoes_disputa.py:102`, `app/chamados/services/disputa_emissao.py:1434`, `app/chamados/services/disputa_emissao.py:1454`.

**Teste `VigiaContaPeloPlantioNaoPeloInstanteTest`: FALHA sem a cura, por análise estática.** Cria a saída em 01/12, com `timestamp` 23/10, e exige seu PK em janela de 30 dias. No pai, o filtro `timestamp >= 01/11` fornecido no delta a exclui e produz `[]`; no delta, `criado_em` 01/12 a inclui. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:30`, `app/chamados/tests/test_plantio_noturno_mais1.py:32`, `app/chamados/tests/test_plantio_noturno_mais1.py:192`, `app/chamados/tests/test_plantio_noturno_mais1.py:195`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`; código exato do pai: **[nao verificado por Git; disponível apenas no delta fornecido]**. Execução Django/RED real: **[nao verificado]**.

**Leitores de `Batida.criado_em` encontrados:**

| Leitor | Significado |
|---|---|
| Vigia novo | Nascimento recente da linha tomado como plantio recente. `app/ponto/management/commands/vigia_plantio_overnight.py:76`. |
| `_marca_ecos_flush` | Hora de chegada ao servidor versus instante do evento, para identificar flush offline. `app/ponto/turnos.py:355`, `app/ponto/turnos.py:362`, `app/ponto/turnos.py:366`. |
| `sequencia_relogio_x_jornada` | Universo de linhas criadas desde corte. `app/ponto/turnos.py:2378`, `app/ponto/turnos.py:2384`. |
| `marcos_do_lote` | Invalidação de cache por nascimento de linha; retração tem relógio separado. `app/chamados/veredito_lavrado.py:137`, `app/chamados/veredito_lavrado.py:140`. |
| Cartório | Fato novo desde julgamento: nascimento ou retração de batida na janela. `app/ponto/services/cartorio.py:350`, `app/ponto/services/cartorio.py:352`. |

**Classes regressivas pertinentes:** segundo juiz e segundo escritor novo, **nao achei**; relógio dentro do helper permanece (`timezone.now()`), e o delta apenas troca o campo filtrado. O teste novo tem mordida contra essa troca; o teste anterior continua exigindo só `delta >= 1`. Referências: `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`, `app/chamados/tests/test_plantio_noturno_mais1.py:178`, `app/chamados/tests/test_plantio_noturno_mais1.py:195`. A divergência semântica relevante é **nascimento da linha versus última aplicação/revinculação**, demonstrada pelo reaproveitamento sem atualização do carimbo. Referências: `app/ponto/models.py:79`, `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:922`.

## 06/11 20:04 · sessao 7d8006827f3a · - · - · origem=Ronald (tmux)

**Cura (a): fecha o caminho de conversa-hora; caminho legítimo de preencher o dia depois: nao achei.** Todas as perguntas de catálogo `hora` são `orfao_14h`, `saida_sem_entrada`, `intervalo_saida` e `intervalo_volta`; todas pedem `{data}`. Os dois intervalos sem dia já são recusados antes da cura; a guarda nova efetivamente alcança os motivos órfão/SSE que ainda chegam sem dia após os fallbacks. Referências: `app/chamados/catalogo/perguntas.py:48`, `:56`, `:124`, `:132`; `app/chamados/services/disputa_emissao.py:338`, `:387`, `:411`.

O caminho demonstrável é data malformada: o formatador aceita o texto bruto, o parser não produz dia, falta batida válida e falta chamado datado. Antes abriria conversa; agora registra `info` e continua. Referências: `app/chamados/catalogo/query.py:62`, `:66`; `app/chamados/services/disputa_emissao.py:364`, `:379`, `:399`, `:410`, `:415`.

A conversa anterior **já nascia com `localdate()`**; o redatador encontrado só escreve `data_evento` quando a pergunta não tem data. Logo o caminho “nasce conversa, depois outro escritor põe o dia correto” não se sustentava nesse código. Referências: `app/chamados/models.py:2426`; `app/chamados/services/fio.py:835`, `:837`. Outros escritores efetivos encontrados por busca: nao achei além do nascimento e dessa porta; fluxo dinâmico não coberto pela busca: **[nao verificado]**.

Não some toda a acusação: a criação/agregação grava motivos e contexto **antes** de gerar perguntas, além da lavra de abertura da disputa. A recusa específica não gera registro de pergunta. Referências: `app/chamados/services/disputa_emissao.py:511`, `:532`, `:535`, `:557`, `:567`, `:640`, `:415`.

**Efeito novo concreto a registrar:** o chamador ignora a lista retornada e continua enviando push “Toque para responder” mesmo se o gerador recusou a única pergunta. Isso já era possível por outras recusas, mas a cura acrescenta este caso. Referências: `app/chamados/services/disputa_emissao.py:640`, `:641`, `:652`, `:656`, `:1575`, `:1580`. Ocorrência/impacto em produção: **[nao verificado]**.

**Chamadores diretos encontrados fora de testes:** [nao verificado]

| Chamador | Expectativa |
|---|---|
| `criar_ou_atualizar_disputa` | Chama no nascimento e na agregação; ignora retorno e retorna disputa, com push separado. `app/chamados/services/disputa_emissao.py:640`, `:652`, `:658`. |
| `perguntar_entradas_pendentes` | Calcula dia via batida/`_T1`, injeta contexto e soma lista retornada; só salva contexto se recebeu perguntas. `app/chamados/services/disputa_emissao.py:282`, `:292`, `:294`, `:295`. |
| Fábrica `perguntar` | Fornece dia explícito, chama criar/atualizar e chama gerador novamente; ignora lista, retorna número de carimbos desfeitos. `app/chamados/services/fabrica_da_celula.py:129`, `:139`, `:141`, `:142`. |
| Comando `smoke_chamados` | Fornece data válida quando template exige, espera exatamente uma pergunta por motivo e tipo igual ao catálogo. `app/chamados/management/commands/smoke_chamados.py:90`, `:96`, `:102`, `:104`, `:109`. |

O gerador documenta “lista das perguntas criadas”, mas adiciona `p` mesmo quando `_criada=False`, comportamento anterior ao delta. Isso explica que lista não é prova estrita de criação. Referências: `app/chamados/services/disputa_emissao.py:304`, `:423`, `:449`; `app/chamados/models.py:2497`.

**Mordida dos testes — inferência estática, execução Django/RED no pai: [nao verificado].** [nao verificado]

- `PerguntaDeHoraSemDiaDoTurnoTest.test_MORDE...`: **falharia sem a cura (a)**. A fixture fornece motivo `orfao_14h`, data malformada, sem vínculo; o caminho anterior abriria conversa com o dia congelado e violaria `assertFalse(...data_evento=localdate()).exists()`. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:198`, `:203`, `:205`, `:206`, `:207`; `app/chamados/catalogo/query.py:66`; `app/chamados/models.py:2426`; guarda nova `app/chamados/services/disputa_emissao.py:411`.
- `test_contra_exemplo_escolha_sem_dia_segue_conversa`: **passaria sem a cura**. O catálogo dá `escolha`, e o fallback conversa anterior é precisamente o comportamento exigido. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:210`, `:213`, `:214`; `app/chamados/catalogo/perguntas.py:82`, `:84`; `app/chamados/models.py:2426`.
- Caso congelado `test_a2...test_MORDE_batida_sem_linha_nao_inventa_dia_pelo_timestamp`: **passaria na árvore pai em dias locais diferentes de 12/08/2026**; o delta só fixa o relógio em outubro. Não é RED das curas (a)-(c), pois motivo é `escolha` e ausência da linha já levava à conversa. Sem congelamento, no dia 12/08/2026 os asserts `!=12/08` e `==localdate()` eram incompatíveis. Referências: `app/chamados/tests/test_a2_data_evento_batida.py:51`, `:55`, `:56`, `:59`, `:60`; `app/chamados/catalogo/perguntas.py:84`; `app/chamados/services/disputa_emissao.py:379`, `:383`, `:417`.

Limite do teste de hora: ele proíbe somente nascimento **na data de hoje**, não qualquer nascimento errado em outra data; é suficiente para matar precisamente o fallback antigo. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:207`, `:208`; `app/chamados/models.py:2426`.

## 06/11 20:03 · sessao 65cdc82cca83 · - · - · origem=Ronald (tmux)

- **Sem relógio solto/import morto novos no delta:** as alterações executáveis só capturam timestamps sem `sorted` e acrescentam o assert; todos os instantes do caso 3 são literais aware e o fuso é fixado pelo `override`. O delta não altera imports. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:243`, `:280`, `:281`, `:324`, `:325`, `:327`, `:333`; diff fornecido [nao verificado] quanto à integralidade Git)
- **A ordem exigida é sustentada pelo chamador do teste:** `_rodar` recebe explicitamente a lista `utc` nessa sequência e a porta usa compreensões que preservam a ordem recebida. O assert espera os sobreviventes `[utc[0], utc[1], utc[3]]`. Não depende da ordem de `set`/dict nem de empate do banco. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:325`, `:328`, `:333`; `app/ponto/services/fechamento.py:204`, `:253`, `:260`, `:262`)
- **Ressalva não bloqueante:** retirar `sorted` cura o mascaramento no gravador, mas este novo assert ainda não detecta uma ordenação cronológica introduzida na porta: os três sobreviventes já estão em ordem crescente. O elemento final fora de ordem é excluído pelo filtro. Portanto, “gravador não ordena” está curado; “teste prova preservação de ordem para entrada arbitrária” não está demonstrado. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:281`, `:325`, `:326`, `:333`; `app/ponto/services/fechamento.py:254`)
- **Censo de produção:** os três chamadores encontrados carregam listas por `batidas_apuraveis`, cujo retorno usa `order_by('timestamp')`. O motor deduplica sequencialmente pela batida anterior, justificando preservar a ordem. (`app/ponto/services/fechamento.py:633`, `:754`; `app/ponto/services/espelho.py:1022`, `:1164`; `app/relatorios/pdf_espelho.py:272`, `:419`; `app/ponto/turnos.py:2871`; `app/ponto/motor_calculo_v2.py:905`)
- **Naive não sustentado como bloqueante:** a nova porta chama `data_local`, documentado para aware; a origem estática dos três caminhos é ORM, com `DateTimeField` sem `null=True` e `USE_TZ=True`. Valores realmente devolvidos em produção: **[nao verificado]**. Nada no delta reabre essa interface. (`app/ponto/services/fechamento.py:203`, `:204`; `app/ponto/turnos.py:43`, `:46`; `app/ponto/models.py:23`; `app/config/settings/base.py:95`)
- **Comentário da ALLOWLIST agora confere:** a ofensa restante é `_com_batida = {timezone.localtime(b.timestamp).date() ...}`. (`app/ponto/tests/test_contract_no_batida_date.py:72`; `app/ponto/services/fechamento.py:793`)
- **Novo bloqueante identificado: nenhum dentro deste delta**, limitado às três alterações fornecidas; comparação integral do commit/base permanece **[nao verificado]**. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:281`, `:333`; `app/ponto/tests/test_contract_no_batida_date.py:72`)

## 06/11 20:02 · sessao 99c2d369a999 · H12H06-B · sonda · origem=Code

Não executei a sonda: o invólucro cria arquivos e grava log, contrariando a restrição de somente leitura (`bin/sonda_leitura.sh:216`, `bin/sonda_leitura.sh:217`, `bin/sonda_leitura.sh:250`). Segue a transcrição integral do dado fornecido, sem nova execução: [nao verificado]

```text
sonda_leitura: pre.py [f35cf407c2faf784] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

HASH_FM {"05": [615, "e29ff62650eebac5"], "06": [704, "9400c1a360842d79"], "07": [629, "2edced101a39a6c3"], "08": [618, "22459b830139a009"], "09": [607, "6e69a4d396e9f407"], "10": [624, "1cc01147a16e856b"], "11": [579, "d5a3a67bf065738d"], "12": [87, "afb9cb5ee9e2741c"]}
EXP {"pk": 2, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "80f417a64296887d8c1f58d580947e40aae78af2c21945c14485c0a96f9814c7", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 3, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 4, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 5, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 6, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 7, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 8, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "71570435dbbe5246f59b19986230ad265eece33da6015177bf5b806b75d45e6b", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 9, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "1f2b81f241a8f8cd344a4cd211911f20ab4cdd4ae25d65b416072bff7df2ac02", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 10, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "3c51aae49a29ab84f6b2c21885f796beca6038b483394138c44715743fa95a18", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 11, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "3c51aae49a29ab84f6b2c21885f796beca6038b483394138c44715743fa95a18", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 12, "empresa_id": 4, "mes": 7, "ano": 2026, "hash_sha256": "68d299ff806aceb36e414b75fd18e6ea9043d556b351645072448cf024af010c", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 13, "empresa_id": 4, "mes": 7, "ano": 2026, "hash_sha256": "491f6fff352cc5102cd0d0a5f24f004edc433e792d114563a4e77b529a216c2f", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 14, "empresa_id": 4, "mes": 7, "ano": 2026, "hash_sha256": "491f6fff352cc5102cd0d0a5f24f004edc433e792d114563a4e77b529a216c2f", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 15, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "1897e08736e104ef3d9eb81917bb26c2849f60a21e6f3141bfc2b35acdc08f63", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 16, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "1897e08736e104ef3d9eb81917bb26c2849f60a21e6f3141bfc2b35acdc08f63", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 17, "empresa_id": 3, "mes": 7, "ano": 2026, "hash_sha256": "7d54b0e6602bf4ea8bf9615009cb4e470398f96a9e66b8623dfc03ec4874524e", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 18, "empresa_id": 2, "mes": 8, "ano": 2026, "hash_sha256": "790d8cd7ea0c631c1b76cb462bc9ebadc163787ffd7e854b1d5b9ea0b0d73f01", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 19, "empresa_id": 2, "mes": 8, "ano": 2026, "hash_sha256": "2b3ac743a5782a5122fc89fb917405d9e9529d141f8bb6d81e0c61610b043dd0", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 20, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "8a63e3baabc501af7d63aa3845c6a9221d7f18ac091b5f7038650a23aaa59dbc", "invalidada_em": "2026-09-30 20:07:06.058663+00:00", "substituida_por_id": 23}
EXP {"pk": 21, "empresa_id": 3, "mes": 9, "ano": 2026, "hash_sha256": "6e0f1048428293078bc82c21aa8756630446c394ef2c2a5b16cbb1efbb6626d8", "invalidada_em": "2026-09-30 20:07:07.142119+00:00", "substituida_por_id": 24}
EXP {"pk": 22, "empresa_id": 4, "mes": 9, "ano": 2026, "hash_sha256": "cb7f00fe59f84e505fce92aab2769242aec6671b688d7c514311518345c29773", "invalidada_em": "2026-09-30 20:07:07.385295+00:00", "substituida_por_id": 25}
EXP {"pk": 23, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "0e3e008b49b1ff73a89556a24e495f0e05f566cad644d8354f224300bbc5a703", "invalidada_em": "2026-10-01 14:57:23.407593+00:00", "substituida_por_id": 26}
EXP {"pk": 24, "empresa_id": 3, "mes": 9, "ano": 2026, "hash_sha256": "5c503b95f9f9cd35541db29a45a6447f026e272b9eedf27b346cda26b92810a6", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 25, "empresa_id": 4, "mes": 9, "ano": 2026, "hash_sha256": "84c78cd0871f5f5251db047a6d12054930f91c05a39ec46d6669fddc1d97a73b", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 26, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "0ae5da67364cabfb5dcadde9481ec75d63b702ca95956627adbbb37079664d53", "invalidada_em": "2026-10-01 22:44:56.762182+00:00", "substituida_por_id": 27}
EXP {"pk": 27, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "361d0f9685f86d3a51d49b8f5194df469db8e4818c9cae0e9bd9f1461f9af0cc", "invalidada_em": null, "substituida_por_id": null}
AUS5198 {"pk": 5198, "data": "2026-10-24", "minutos": 420, "tipo": "falta"}
REG_PEND {"n": 0, "itens": []}
REG_PEND_FROTA 2
DISP_ABERTAS {"n": 23}
HASH_DP 9 11501 54c6419c61520d6a
HASH_DP 10 21418 9d68332a36ef63eb
ERROS {}
```

REG_PEND n=0, itens=[]; DISP_ABERTAS n=23. [nao verificado]  
HASH_FM: 09=[607,"6e69a4d396e9f407"]; 10=[624,"1cc01147a16e856b"]. [nao verificado]  
HASH_DP: 9=11501/54c6419c61520d6a; 10=21418/9d68332a36ef63eb. [nao verificado]  
AUS5198: 420 minutos. [nao verificado]  
ERROS={}; nenhuma exceção aparece no dado fornecido; ocorrência fora dele: [nao verificado].

## 06/11 20:01 · sessao 1988589b9388 · - · - · origem=Ronald (tmux)

(a) Fecha o nascimento indevido de conversa `hora`: a guarda recusa apenas quando as duas fontes finais de dia são `None`; antes, esse caminho caía em `abrir_conversa`, que grava `localdate()`. Referências: `app/chamados/services/disputa_emissao.py:410`, `app/chamados/services/disputa_emissao.py:411`, `app/chamados/services/disputa_emissao.py:417`, `app/chamados/models.py:2426`.

**Caminho legítimo “nasce conversa hora e recebe dia depois”: nao achei.** A conversa já nasce com data preenchida; o escritor posterior `reapontar_resolvedora` só atribui data quando a pergunta está sem data. Referências: `app/chamados/models.py:2426`, `app/chamados/services/fio.py:835`, `app/chamados/services/fio.py:837`.

Os quatro motivos `hora` são `orfao_14h`, `saida_sem_entrada`, `intervalo_saida` e `intervalo_volta`. Os intervalos sem dia já retornam antes da nova guarda; seu fluxo não era o fallback de conversa. Referências: `app/chamados/catalogo/perguntas.py:48`, `app/chamados/catalogo/perguntas.py:56`, `app/chamados/catalogo/perguntas.py:124`, `app/chamados/catalogo/perguntas.py:132`, `app/chamados/services/disputa_emissao.py:387`.

**Ressalva concreta:** o novo caminho pode deixar disputa sem pergunta e enviar push mandando responder: o chamador ignora a lista retornada e envia push após gerar; a mensagem promete “Toque para responder”. Uma disputa sem perguntas também trava o encerramento do chamado. Referências: `app/chamados/services/disputa_emissao.py:640`, `app/chamados/services/disputa_emissao.py:641`, `app/chamados/services/disputa_emissao.py:652`, `app/chamados/services/disputa_emissao.py:656`, `app/chamados/services/disputa_emissao.py:1575`, `app/chamados/juizes.py:2084`.

**Não desaparece todo rastro além do log:** motivos/contexto já foram persistidos na disputa antes da tentativa de geração. O ramo novo, porém, não persiste um ato de recusa da pergunta: apenas `logger.info` e `continue`. Uma chamada posterior com contexto corrigido tenta gerar novamente, mesmo com motivo já existente. Referências: `app/chamados/services/disputa_emissao.py:557`, `app/chamados/services/disputa_emissao.py:511`, `app/chamados/services/disputa_emissao.py:529`, `app/chamados/services/disputa_emissao.py:415`, `app/chamados/services/disputa_emissao.py:652`.

Chamadores diretos encontrados fora dos testes: [nao verificado]

| Chamador | Expectativa observável |
|---|---|
| `perguntar_entradas_pendentes` | Injeta dia obtido por `_T1`, acumula a lista retornada e salva contexto somente se a lista não estiver vazia. `app/chamados/services/disputa_emissao.py:282`, `app/chamados/services/disputa_emissao.py:292`, `app/chamados/services/disputa_emissao.py:294`, `app/chamados/services/disputa_emissao.py:295`. |
| `criar_ou_atualizar_disputa`, nascimento e atualização | Ignora lista, envia push por nascimento/motivo novo e retorna disputa. `app/chamados/services/disputa_emissao.py:640`, `app/chamados/services/disputa_emissao.py:642`, `app/chamados/services/disputa_emissao.py:652`, `app/chamados/services/disputa_emissao.py:656`, `app/chamados/services/disputa_emissao.py:658`. |
| `fabrica_da_celula.perguntar` | Fornece dia explícito, ignora a lista e retorna quantidade de carimbos desfeitos. `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/fabrica_da_celula.py:141`, `app/chamados/services/fabrica_da_celula.py:142`. |
| `smoke_chamados._bloco_a_motivos` | Fornece data válida para templates datados e exige exatamente uma pergunta por motivo. `app/chamados/management/commands/smoke_chamados.py:96`, `app/chamados/management/commands/smoke_chamados.py:102`, `app/chamados/management/commands/smoke_chamados.py:104`. |

`models.py` apenas reexporta a função; a declaração de hora usa sua própria porta e documenta que não chama o gerador. Referências: `app/chamados/models.py:3350`, `app/chamados/services/disputa_emissao.py:585`.

**Testes, por análise contrafactual do diff; execução no pai [nao verificado]:** [nao verificado]

- `PerguntaDeHoraSemDiaDoTurnoTest.test_MORDE_orfao_sem_dia_nao_nasce_como_conversa_de_hoje` **falha no pai**: data malformada produz texto, o parser de dia falha, não há batida nem chamado para fornecer outra data, e o antigo fallback cria conversa em `localdate()`, contrariando `assertFalse`. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:203`, `app/chamados/tests/test_plantio_noturno_mais1.py:206`, `app/chamados/tests/test_plantio_noturno_mais1.py:207`, `app/chamados/catalogo/query.py:62`, `app/chamados/services/disputa_emissao.py:364`, `app/chamados/services/disputa_emissao.py:238`, `app/chamados/models.py:2426`.
- Seu contraexemplo `escolha` **passa também no pai**: esse tipo não é atingido pela nova guarda. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:210`, `app/chamados/catalogo/perguntas.py:82`, `app/chamados/services/disputa_emissao.py:411`.
- O caso congelado de `test_a2` **passaria no pai com o mesmo congelamento transplantado**: usa motivo `escolha`, cujo comportamento não mudou. Sem congelamento, passaria nos dias diferentes de 12/08/2026; em 12/08, os dois asserts são incompatíveis. Congelar cura instabilidade do teste, não produz RED de implementação. Referências: `app/chamados/tests/test_a2_data_evento_batida.py:51`, `app/chamados/tests/test_a2_data_evento_batida.py:57`, `app/chamados/tests/test_a2_data_evento_batida.py:59`, `app/chamados/tests/test_a2_data_evento_batida.py:60`, `app/chamados/catalogo/perguntas.py:84`.

O teste novo de `hora` rejeita somente pergunta com data de hoje; não demonstra ausência de pergunta em qualquer outra data. Referência: `app/chamados/tests/test_plantio_noturno_mais1.py:207`.

**Escritores encontrados de `PerguntaDisputa.data_evento`:** nascimento real em `_nascer`; atribuição posterior e desfazimento em memória em `reapontar_resolvedora`; `_Proxy.data_evento` é insumo transitório de validação. Outro escritor real: **nao achei**. Referências: `app/chamados/models.py:2498`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:846`, `app/chamados/models.py:2444`.

**ESMERIL:** permanece a divergência já declarada fora do escopo: conversa grava dia da pergunta, enquanto `do_fato` e fábrica usam a mesma data como identidade de fato/célula, sem discriminador. A guarda nova elimina a conversa `hora` deste emissor, mas não resolve essa colisão geral. Referências: `app/chamados/models.py:2422`, `app/chamados/models.py:2396`, `app/chamados/models.py:2453`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/disputa_emissao.py:411`.

## 06/11 20:01 · sessao 0ca929e8dcf8 · - · - · origem=Ronald (tmux)

**(c) fecha a omissão para batidas novas; não transforma `criado_em` em data da última materialização.** O filtro agora usa nascimento da linha, enquanto a comparação continua pelo juiz `_T1`. Referências: `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`.

- **Escritor normal:** `Batida.criado_em` é `auto_now_add=True`; as criações passam por `Batida.objects.create`. A porta de anotação não permite alterar esse campo. Referências: `app/ponto/models.py:79`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:241`.
- **Reescritor dedicado do campo em produção: nao achei.** A restauração de tenant usa `pg_restore`; a sombra incremental copia `ponto_batida` com `SELECT *` e repõe as colunas por `COPY`. Por essas operações, o valor é preservado do registro original, não recalculado pela hora da cópia — inferência do SQL. Referências: `scripts/restore_tenant.sh:8`, `scripts/backup_tenant.sh:10`, `bin/sombra_incremental.sh:34`, `bin/sombra_incremental.sh:57`, `bin/sombra_incremental.sh:62`. Conteúdo real dos dumps e restaurações executadas: **[nao verificado]**.
- **Plantio depois de reabertura:** quando cria outra linha, recebe o nascimento novo; quando reaproveita linha existente, conserva o nascimento antigo. O materializador primeiro busca batida no marco e somente cria se não encontrou; depois pode apenas trocar `pergunta_origem`. Referências: `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:847`, `app/chamados/services/materializacao.py:903`, `app/chamados/services/materializacao.py:922`, `app/chamados/services/materializacao.py:924`.
- **Outro reaproveitamento:** `registrar_batida` devolve linha existente pelo mesmo tipo na janela ou pela mesma chave colaborador/instante, inclusive retratada; não altera `criado_em`. Referências: `app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:120`.
- **Restauração de retratação:** a porta `desfazer` só grava `aprovada=True` e `retratada_em=None`; conserva `criado_em`. Referência: `app/ponto/services/retratar_humano.py:207`.

**Limite semântico:** “plantada recentemente” coincide com `criado_em` para criação nova; não coincide com reaproveitamento/revinculação recente de linha antiga. Assim, uma linha antiga de `origem='disputa_s84_retro'` vinculada hoje a outra pergunta continua fora da janela por criação. Referências: `app/ponto/management/commands/vigia_plantio_overnight.py:67`, `app/ponto/management/commands/vigia_plantio_overnight.py:73`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/chamados/services/materializacao.py:924`. Isso não demonstra nova regressão: se seu `timestamp` também era antigo, o filtro do pai já a omitia — comparação com o delta fornecido; execução no pai **[nao verificado]**.

**Teste do vigia: morde por análise contrafactual.** Cria a linha em 01/12, com instante em 23/10; na janela de 30 dias, o filtro anterior por `timestamp` exclui a única candidata e o resultado seria `[]`, contrariando `[b.pk]`. O teste cobre criação nova, não reaproveitamento. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:30`, `app/chamados/tests/test_plantio_noturno_mais1.py:32`, `app/chamados/tests/test_plantio_noturno_mais1.py:192`, `app/chamados/tests/test_plantio_noturno_mais1.py:193`, `app/chamados/tests/test_plantio_noturno_mais1.py:195`. RED executado: **[nao verificado]**.

**Demais leitores de `Batida.criado_em` encontrados:** o pareador usa chegada para distinguir eco de flush; o contador de sequência seleciona batidas criadas desde um marco; o cache de veredito usa o máximo de criação e trata retratação separadamente. Esses usos conservam o significado de criação/chegada. Referências: `app/ponto/turnos.py:355`, `app/ponto/turnos.py:366`, `app/ponto/turnos.py:368`, `app/ponto/turnos.py:2384`, `app/chamados/veredito_lavrado.py:137`, `app/chamados/veredito_lavrado.py:140`.

## 06/11 20:01 · sessao 5275b260cc32 · - · - · origem=Ronald (tmux)

1. **(b) fecha o `None.isoformat()` local do PWA.** A nova guarda só chama a disputa se `_dia_sse` vier preenchido; não altera o registro nem o retorno ao usuário (`app/ponto/services/triagem_batida.py:726`, `app/ponto/services/triagem_batida.py:730`, `app/ponto/services/triagem_batida.py:757`).

   O caminho concreto de `None` existe: `registrar_batida` exclui retratadas no primeiro dedup, mas depois devolve a mesma chave `(colaborador,timestamp)` mesmo retratada, sem gravá-la novamente; `_T1` não inclui retratadas no mapa (`app/ponto/registro_batida.py:107`, `app/ponto/registro_batida.py:120`, `app/ponto/turnos.py:2905`, `app/ponto/turnos.py:2925`).

   **Cura equivalente nos gêmeos API: nao achei.** Ambos continuam chamando `.isoformat()` diretamente no retorno de `_dtb_sse`; é vizinho preexistente, não introduzido pelo delta (`app/api/views.py:780`, `app/api/views.py:782`, `app/api/views_core.py:780`, `app/api/views_core.py:782`).

2. **Emissor posterior que recupere especificamente essa tentativa SSE/reanime automaticamente essa batida retratada: nao achei.** A devolução da mesma chave não executa `save`; os gatilhos de julgamento/reconciliação ligados a `post_save` não são disparados por esse retorno (`app/ponto/registro_batida.py:120`, `app/ponto/signals.py:131`, `app/ponto/signals.py:206`).

   Há emissão posterior **da falta do dia**, condicional: `processar_cartorio` percorre células passadas e chama `julgar_colab`; para células julgadas, este chama `perguntar_pela_celula`, que pode criar disputa e pergunta datadas pela célula (`app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:995`, `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/fabrica_da_celula.py:174`).

   Isso **não garante pergunta dessa tentativa**: exige célula, colaborador ativo, dia permitido, ata/veredito e marcos cobrados; pode ir à pauta em vez de pergunta. A batida retratada continua fora da apuração (`app/chamados/services/fabrica_da_celula.py:91`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/fabrica_da_celula.py:102`, `app/chamados/services/fabrica_da_celula.py:171`, `app/ponto/turnos.py:2905`).

   `disparar_perguntas_competencia` tem caminho escritor para **chamado vivo sem pergunta**; o caminho das células é vigia. Se a guarda não criou disputa/chamado dessa tentativa, esse caminho não a recupera por si (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`, `app/chamados/management/commands/disparar_perguntas_competencia.py:238`, `app/chamados/management/commands/disparar_perguntas_competencia.py:268`).

   A existência/execução efetiva desses crons em produção é **[nao verificado]**. A restituição explícita por humano existe e limpa `retratada_em`, devolvendo a batida à apuração (`app/ponto/services/retratar_humano.py:190`, `app/ponto/services/retratar_humano.py:207`).

3. **Inventário de `contexto['saida_sem_entrada']`:**

   - **Escritor direto do delta:** PWA grava `data` pelo `_T1` da batida e `hora_tentativa` pelo relógio da tentativa (`app/ponto/services/triagem_batida.py:729`, `app/ponto/services/triagem_batida.py:738`).
   - **APIs gêmeas:** montam inicialmente `data` civil, mas a sobrescrevem por `_T1` antes de chamar o persistidor; exceção no `_T1` interrompe esse trajeto (`app/api/views.py:655`, `app/api/views.py:780`, `app/api/views_core.py:652`, `app/api/views_core.py:780`).
   - **Fábrica genérica do dia:** para motivo `saida_sem_entrada`, grava `{motivo: {'data': dia.isoformat()}}`, com dia da célula (`app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/fabrica_da_celula.py:176`).
   - **Persistidor:** nascimento copia `contexto_nascimento`; agregação faz merge por motivo, preservando campos anteriores e sobrescrevendo os recebidos (`app/chamados/services/disputa_emissao.py:518`, `app/chamados/services/disputa_emissao.py:525`, `app/chamados/services/disputa_emissao.py:557`, `app/chamados/services/disputa_emissao.py:632`).
   - **Leitor de texto:** usa `contexto[motivo].data`; o texto canônico SSE pergunta pela **entrada** daquele dia (`app/chamados/catalogo/query.py:58`, `app/chamados/catalogo/query.py:64`, `app/chamados/catalogo/perguntas.py:56`).
   - **Leitor de nascimento:** transforma esse `data` em `data_evento` (`app/chamados/services/disputa_emissao.py:364`, `app/chamados/services/disputa_emissao.py:367`, `app/chamados/services/disputa_emissao.py:410`, `app/chamados/services/disputa_emissao.py:423`).
   - **Leitor de destino:** usa a data da pergunta prioritariamente; contexto é fallback para casar o dia do chamado (`app/chamados/services/disputa_emissao.py:191`, `app/chamados/services/disputa_emissao.py:194`, `app/chamados/services/disputa_emissao.py:231`).
   - **Materializador:** prioriza `p.data_evento`, depois contexto por motivo, e trata o valor como dia do marco; SSE materializa tipo `E` (`app/chamados/services/materializacao.py:341`, `app/chamados/services/materializacao.py:343`, `app/chamados/services/materializacao.py:347`).

   **Leitor específico de SSE que ainda trate `data` como dia civil da saída: nao achei.** O par PWA→leitores está alinhado em dia do turno; a leitura de `hora_tentativa` para recuar entrada saiu do derivador, conforme a delegação/comentário atual (`app/ponto/services/triagem_batida.py:739`, `app/chamados/services/materializacao.py:419`, `app/chamados/juizes.py:302`).

   Ressalva preexistente: APIs adicionam motivo `orfao_14h` enquanto escrevem a subchave `saida_sem_entrada`; o leitor genérico por motivo não acessa diretamente essa chave para `orfao_14h`, embora haja fallback pela `ultima_batida.id` (`app/api/views_core.py:651`, `app/api/views_core.py:652`, `app/api/views_core.py:773`, `app/chamados/services/disputa_emissao.py:364`, `app/chamados/services/disputa_emissao.py:379`).

## 06/11 19:55 · sessao 3d97092632e9 · BUG-PLANTIO-NOTURNO-MAIS-1 · verificador · origem=Code

**O aval não está integralmente atendido:** a conversa ainda pode ser confundida com pergunta de turno no dedup e na fábrica da célula; há datas de contexto que chegam à porta sem passar por `_T1`; e o vigia pode omitir plantio retroativo recente. Referências: `app/chamados/models.py:2396`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/disputa_emissao.py:1101`, `app/ponto/management/commands/vigia_plantio_overnight.py:75`.

**1. Quem escreve e quem lê**

Os escritores efetivos encontrados são `_nascer` no nascimento e `reapontar_resolvedora` na costura. A atribuição ao `_Proxy` é transitória, para validação; não grava uma pergunta. Referências: `app/chamados/models.py:2444`, `app/chamados/models.py:2498`, `app/chamados/services/fio.py:835`.

| Emissor que fornece o valor à porta | Origem do valor |
|---|---|
| `gerar_perguntas_disputa` | Dia fornecido pela grade após localizar batida e consultar `_T1`; alternativamente, `contexto[motivo].data`, batida de `ultima_batida.id` via `_T1`, ou dia do chamado. O resultado é embrulhado por `dia_do_turno_da_celula`. `app/chamados/services/disputa_emissao.py:68`, `app/chamados/services/disputa_emissao.py:76`, `app/chamados/services/disputa_emissao.py:364`, `app/chamados/services/disputa_emissao.py:383`, `app/chamados/services/disputa_emissao.py:400`, `app/chamados/services/disputa_emissao.py:424`. |
| `perguntar_entradas_pendentes` | Localiza cada entrada por PK, consulta `_T1` e injeta seu dia no contexto do gerador. `app/chamados/services/disputa_emissao.py:282`, `app/chamados/services/disputa_emissao.py:292`. |
| Agrupador de chamados órfãos | Com batida-origem: `resolver_data_evento` → `_T1`. Sem origem: parser do contexto do chamado. Para intervalo: lê `marcos_faltantes[].data`. `app/chamados/services/disputa_emissao.py:1017`, `app/chamados/services/disputa_emissao.py:1074`, `app/chamados/services/disputa_emissao.py:1101`, `app/chamados/services/disputa_emissao.py:1114`. |
| `_complementar_marcos_faltantes` | Dia da célula devolvido pelo leitor dos marcos faltantes. `app/chamados/services/declaracao_texto.py:123`, `app/chamados/services/disputa_emissao.py:842`. |
| `declarar_hora_colab` e `aplicar_declaracao` | Dia do marco faltante da célula. `app/chamados/services/declaracao_texto.py:299`, `app/chamados/services/declaracao_texto.py:327`, `app/chamados/services/declaracao_texto.py:617`. |
| `abrir_conversa` | Dia civil em que foi feita: `localdate()`. Chamadores: reabertura administrativa, âncora ausente e fallback do gerador. `app/chamados/models.py:2426`, `app/chamados/services/acoes_disputa.py:260`, `app/chamados/services/disputa_emissao.py:412`, `app/chamados/services/disputa_emissao.py:1230`. |
| Reconciliador → `reapontar_resolvedora` | Dia do chamado/contexto, embrulhado como dia de célula. A costura só preenche pergunta sem data. `app/chamados/reconciliador.py:941`, `app/chamados/reconciliador.py:1624`, `app/chamados/reconciliador.py:1641`, `app/chamados/services/fio.py:835`. |
| Declaração → `reapontar_resolvedora` | Passa diretamente o dia do marco faltante, sem o construtor tipado. `app/chamados/services/declaracao_texto.py:320`. |

Detector, API e PWA escrevem **contextos que alimentam esses emissores**: detector via `resolver_data_evento`; kernels e triagem via batida gravada → `_T1`. Referências: `app/chamados/detector_anomalias.py:47`, `app/api/views.py:780`, `app/api/views_core.py:782`, `app/ponto/services/triagem_batida.py:734`.

Inventário dos leitores de produção encontrados, agrupado pelo significado:

| Leitura | Consumidores e significado |
|---|---|
| Instante do plantio | `_ts_tipo_do_plantio` lê como **dia do turno** e combina com a hora; `_ajustar_saida_overnight` escolhe D ou D+1 pelos marcos. `app/chamados/services/materializacao.py:344`, `app/chamados/services/materializacao.py:414`, `app/chamados/juizes.py:312`, `app/chamados/juizes.py:319`. |
| Slot, prova do plantio e lote | `_slot_esperado` delega ao plantio; `_plantio_na_hora_do_slot` usa D/D+1; `classe_do_lote` verifica conflito no instante do slot. `app/chamados/juizes.py:346`, `app/chamados/juizes.py:266`, `app/chamados/juizes.py:271`, `app/chamados/services/validacao.py:825`. |
| Turno, célula e julgamento | `_turno_tem_saida`, `_celulas_da_pergunta`, `_pergunta_materializada_de_fato`, `_pergunta_viva_calc`, `_marco_esperado_da_grade` e `resolvedora_valida` usam a data como **chave do turno/fato**. `app/chamados/juizes.py:363`, `app/chamados/juizes.py:623`, `app/chamados/juizes.py:666`, `app/chamados/juizes.py:1202`, `app/chamados/juizes.py:1524`, `app/chamados/juizes.py:1964`. |
| Dedup e emissão | `do_fato`, `_nascer`, gerador, `_ja_perguntou`, declaração, migração de resolvedoras, fábrica da célula e diagnóstico de pergunta no app usam **identidade por dia/motivo**. `app/chamados/models.py:2396`, `app/chamados/models.py:2452`, `app/chamados/services/disputa_emissao.py:389`, `app/chamados/services/disputa_emissao.py:1272`, `app/chamados/services/declaracao_texto.py:227`, `app/chamados/services/declaracao_texto.py:539`, `app/chamados/services/fio.py:941`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/pergunta_no_app.py:109`. |
| Dia do chamado e competência | Derivação do dia pela disputa, tranca, vias de resolução, competência trancada, passivo validado e expiração usam **dia do fato/turno para competência**. `app/chamados/juizes.py:76`, `app/chamados/juizes.py:209`, `app/chamados/juizes.py:932`, `app/chamados/juizes.py:1476`, `app/chamados/juizes.py:1480`, `app/chamados/models.py:351`, `app/chamados/models.py:377`, `app/chamados/services/competencia_trancada.py:107`, `app/chamados/services/passivo_validado.py:113`, `app/chamados/veredito_lavrado.py:229`. |
| Exceção civil explícita | O ramo de adesão de `via_pelo_fato` monta **o dia civil inteiro** a partir da chave, para perguntar se houve qualquer batida naquele dia. É diferente de consultar batidas pertencentes ao turno. `app/chamados/juizes.py:1450`, `app/chamados/juizes.py:1461`. |
| Ausência e validação | Declaração/materialização de ausência, perguntas absorvidas, aceitação de folga e recálculo usam **dia do fato**. `app/chamados/services/materializacao.py:473`, `app/chamados/services/materializacao.py:517`, `app/chamados/services/materializacao.py:607`, `app/chamados/services/validacao.py:250`, `app/chamados/services/validacao.py:407`. |
| Reconciliação | Julgamento de órfãs, encerramento do dia, semeadura de eventos e reatamento usam **dia do turno/célula**. `app/chamados/reconciliador.py:370`, `app/chamados/reconciliador.py:1040`, `app/chamados/reconciliador.py:1080`, `app/chamados/reconciliador.py:1297`, `app/chamados/reconciliador.py:1543`. |
| Costura e eventos | Comparação com o dia do chamado, guarda de costura e localização da célula pelo par colaborador/data. `app/chamados/services/fio.py:87`, `app/chamados/services/fio.py:773`, `app/chamados/models.py:2690`, `app/chamados/models.py:2706`. |
| Folha, calendário e fechamento do fio | Agrupam a pergunta no **dia da célula**. `app/folha/previa.py:65`, `app/colaboradores/services/calendario.py:473`, `app/ponto/services/dia_do_colab.py:65`, `app/ponto/services/veredito_celula.py:176`, `app/chamados/services/relavra_via.py:152`, `app/chamados/services/limbo_folgas.py:202`. |
| Canal, cobrança e vigias | Associam pergunta ao dia do chamado/célula ou comparam com o turno da batida. `app/chamados/canal.py:72`, `app/chamados/services/cobranca.py:83`, `app/chamados/services/aviso_pendencia.py:20`, `app/chamados/services/vigia_fio.py:816`, `app/ponto/management/commands/vigia_plantio_overnight.py:89`. |
| API e apresentação | Serializam, ordenam ou imprimem a data, sem converter instante. `app/api/views.py:1174`, `app/api/views.py:2654`, `app/api/views_mensageria.py:568`, `app/chamados/views_cobrar.py:176`, `app/chamados/services/explicador.py:70`, `app/chamados/services/explicador.py:126`, `app/chamados/services/historico.py:108`, `app/templates/chamados/partials/_card_atendimento.html:90`, `app/templates/chamados/partials/_lote_preview.html:24`, `app/templates/chamados/meu_atendimento.html:19`. |
| Comandos operacionais | Dedup por fato, seleção por competência, reabertura, backfill, cruzamento de furos e auditorias leem **dia do fato/turno**; alguns apenas o imprimem. `app/chamados/management/commands/dedup_perguntas_fato.py:54`, `app/chamados/management/commands/disparar_perguntas_competencia.py:213`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:68`, `app/chamados/management/commands/backfill_veredito_lavrado.py:111`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:74`, `app/ponto/management/commands/mapa_divergencia.py:272`, `app/chamados/management/commands/auditar_invariantes_chamados.py:73`, `app/chamados/management/commands/auditar_invariantes_chamados.py:892`. |

**Divergências encontradas:** conversa versus identidade de turno, detalhada no item 2; contexto civil versus dia do turno, detalhado no item 7; e apresentação que ainda promete “é aqui que a batida nasce” ao mostrar `data_evento`, embora a saída noturna nasça em D+1. Referências: `app/chamados/models.py:2426`, `app/chamados/models.py:2453`, `app/chamados/data_evento.py:27`, `app/templates/chamados/partials/modal_fio.html:166`, `app/chamados/juizes.py:319`.

**2. Conversa: há leitores que a tratam como turno**

**Sim.** O parâmetro `conversa` só controla o nascimento; não é persistido como discriminador. Referências: `app/chamados/models.py:2450`, `app/chamados/models.py:2498`.

Caminho concreto de colisão:

- A reabertura aceita qualquer motivo válido e cria conversa `escolha` com esse motivo e `localdate()`. Referências: `app/chamados/services/acoes_disputa.py:245`, `app/chamados/services/acoes_disputa.py:247`, `app/chamados/services/acoes_disputa.py:260`, `app/chamados/models.py:2426`.
- Uma abertura datada posterior procura somente colaborador/motivo/data. Pode encontrar essa conversa, atribuir-lhe a célula e devolvê-la, sem criar a pergunta `hora`. Referências: `app/chamados/models.py:2396`, `app/chamados/models.py:2453`, `app/chamados/models.py:2457`, `app/chamados/models.py:2497`.
- A fábrica da célula também pode considerar o marco “já perguntado” por causa dessa conversa. Referências: `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/fabrica_da_celula.py:126`.
- O C6 passa a data a `classificar_falta`; o comando de dedup e a migração de resolvedoras também não distinguem conversa. Referências: `app/chamados/juizes.py:1202`, `app/chamados/management/commands/dedup_perguntas_fato.py:54`, `app/chamados/services/fio.py:941`.

As conversas normais `escolha` não entram no slot; `data_hora` usa a data declarada na resposta; o vigia novo seleciona apenas `hora`. Referências: `app/chamados/juizes.py:332`, `app/chamados/juizes.py:334`, `app/ponto/management/commands/vigia_plantio_overnight.py:73`.

Entretanto, há caminho para conversa **`hora`**: data malformada ainda produz texto, falha no parser e, sem outra data derivável, chega a `abrir_conversa` mantendo o tipo do catálogo. Depois, plantio e vigia leem seu `localdate()` como turno. Referências: `app/chamados/catalogo/query.py:62`, `app/chamados/services/disputa_emissao.py:365`, `app/chamados/services/disputa_emissao.py:410`, `app/chamados/services/disputa_emissao.py:413`, `app/chamados/services/materializacao.py:344`, `app/ponto/management/commands/vigia_plantio_overnight.py:89`. Ocorrência em produção: **[nao verificado]**.

**3. Escritor fora das portas invisível ao selo**

**Escritor real adicional: nao achei.** O censo AST executado em memória encontrou somente proxy/create de `_nascer` e atribuição/desfazimento de `reapontar_resolvedora`. Referências: `app/chamados/models.py:2444`, `app/chamados/models.py:2498`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:846`.

O selo vê `qs.update(data_evento=d)` mesmo com queryset em variável. Não acompanha, porém, `campos={'data_evento': d}; qs.update(**campos)`, nome variável em `setattr`, SQL ou escrita implícita por formulário. Sua análise procura constantes no próprio argumento e não acompanha fluxo de valores. Referências: `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:54`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:86`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:92`.

Existe uma abertura genérica: `descarimbar_em_lote(**outros)` encaminha campos ao `update`; não achei chamador atual passando `data_evento`. Referências: `app/chamados/models.py:2714`, `app/chamados/models.py:2721`, `app/chamados/models.py:1508`, `app/chamados/services/disputa_emissao.py:1449`.

**4. Consumidores dos contextos do kernel**

| Campo | Quem lê |
|---|---|
| `entrada_sem_saida.entrada_id` | Só achei `_data_furo_da_grade`: busca por PK e consulta `_T1`. `app/chamados/services/disputa_emissao.py:68`, `app/chamados/services/disputa_emissao.py:76`. |
| `ultima_batida.id` | `_data_furo_da_grade`, fallback do gerador via `_T1` e resolvedor de foto como identidade da batida. `app/chamados/services/disputa_emissao.py:72`, `app/chamados/services/disputa_emissao.py:379`, `app/api/foto_recebida.py:67`. |
| `saida_sem_entrada.data` | Texto, data da pergunta, seleção do chamado e fallback do materializador. `app/chamados/catalogo/query.py:58`, `app/chamados/services/disputa_emissao.py:364`, `app/chamados/services/disputa_emissao.py:196`, `app/chamados/services/materializacao.py:343`. |

**Leitor que ainda interprete especificamente `saida_sem_entrada.data` como dia civil da saída: nao achei.** O texto canônico pergunta pela entrada naquele dia. Referência: `app/chamados/catalogo/perguntas.py:56`.

Há duas ressalvas: o kernel acrescenta motivo `orfao_14h`, mas grava contexto sob `saida_sem_entrada`, portanto a leitura genérica pelo motivo não pega diretamente essa chave; e o vizinho `_dia_do_contexto_fraco` ainda transforma `ultima_batida.timestamp` em `.date()` sem conversão de fuso ou `_T1`. Referências: `app/api/views.py:654`, `app/api/views.py:655`, `app/api/views_core.py:651`, `app/api/views_core.py:652`, `app/chamados/juizes.py:155`, `app/chamados/juizes.py:162`.

**5. Triagem: disputa depois de `registrar_batida`**

**Sim, há caminhos em que antes nascia e agora não:** `dupla`, retorno `batida is None`/`ignorada_intervalo` e exceções entre o predicado e o nascimento. O predicado está antes desses caminhos; a disputa agora vem depois deles. Referências: `app/ponto/services/triagem_batida.py:654`, `app/ponto/services/triagem_batida.py:670`, `app/ponto/services/triagem_batida.py:701`, `app/ponto/services/triagem_batida.py:714`, `app/ponto/services/triagem_batida.py:729`; posição anterior: diff fornecido **[nao verificado por comparação Git]**.

**Outro sítio que exija explicitamente a disputa prévia para dupla/ignorada: nao achei.** Contudo, os sinais da gravação já executam cartório e reconciliador antes de a nova disputa nascer; essa passagem não vê o fio futuro. Referências: `app/ponto/signals.py:131`, `app/ponto/signals.py:206`, `app/chamados/reconciliador.py:1418`, `app/ponto/services/triagem_batida.py:701`, `app/ponto/services/triagem_batida.py:729`.

Há ainda um caminho de exceção novo: `registrar_batida` pode devolver uma mesma chave retratada; o wrapper de `_T1` pode retornar `None`, mas a triagem chama `.isoformat()` sem guarda. Para ultrapassar o guard de dupla de saída, a chave retratada pode ser de outro tipo. Referências: `app/ponto/registro_batida.py:116`, `app/ponto/registro_batida.py:120`, `app/ponto/turnos.py:2925`, `app/ponto/services/triagem_batida.py:665`, `app/ponto/services/triagem_batida.py:734`. Ocorrência em produção: **[nao verificado]**.

**6. Classes que já reprovaram obra**

| Classe | Conferência |
|---|---|
| Segundo juiz | **Juiz geométrico novo: nao achei.** O wrapper consulta o juiz existente; a autoridade registrada continua `turnos_do_colab`. `app/ponto/turnos.py:2925`, `app/core/juizes.py:196`. |
| Segundo escritor | **Escritor real extra: nao achei.** Há as lacunas de detecção do item 3. `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:86`. |
| Relógio fora da borda | `abrir_conversa` lê o relógio no modelo, e o contador lê dentro do helper, sem receber instante do chamador. A primeira leitura foi deslocada pelo diff; a segunda é nova. `app/chamados/models.py:2426`, `app/ponto/management/commands/vigia_plantio_overnight.py:75`. |
| Derivação paralela | **Ainda existe entrada por parsers de contexto**, sem `_T1`; o construtor de célula apenas devolve a data recebida. `app/chamados/data_evento.py:27`, `app/chamados/services/disputa_emissao.py:1101`, `app/ponto/turnos.py:2933`. |
| Selo/RED sem mordida | Há asserts que atingem o defeito principal: escritor retorna D, saída planta em D+1, detector grava D e sem marcos não recua. `app/chamados/tests/test_plantio_noturno_mais1.py:78`, `app/chamados/tests/test_plantio_noturno_mais1.py:83`, `app/chamados/tests/test_plantio_noturno_mais1.py:90`, `app/chamados/tests/test_plantio_noturno_mais1.py:112`. Não cobrem a colisão conversa/datada. `app/chamados/tests/test_b2_data_conversa.py:14`, `app/chamados/tests/test_b2_data_conversa.py:26`. Execução Django, RED contra o pai e mutação real: **[nao verificado]**. |
| Relógio dos testes | O teste novo exige simultaneamente data diferente de 12/08/2026 e igual a `localdate()`, sem congelamento: no próprio dia 12/08/2026 as duas condições são incompatíveis. O arquivo já está na lista de exceções. `app/chamados/tests/test_a2_data_evento_batida.py:50`, `app/chamados/tests/test_a2_data_evento_batida.py:57`, `app/chamados/tests/test_a2_data_evento_batida.py:58`, `app/core/tests/testes_sem_relogio.txt:26`. |
| Ordem de trava | **Inversão comprovada: nao achei.** A costura mantém atomic até o save; a emissão trava disputa. A mudança verificável é gravação/sinais antes da nova disputa, conforme item 5. `app/chamados/services/fio.py:840`, `app/chamados/services/fio.py:850`, `app/chamados/services/disputa_emissao.py:494`. Deadlock: **[nao verificado]**. |
| Pouso antes do pré-requisito | Suite, pouso e saneamento do passivo: **[nao verificado]**. A costura não converte datas já preenchidas, pois só data perguntas sem dia. `app/chamados/services/fio.py:835`. |
| Campo com dois significados | **Permanece alcançável:** conversa grava dia da pergunta; leitores de identidade/célula o recebem como dia do turno. `app/chamados/models.py:2426`, `app/chamados/models.py:2396`, `app/chamados/services/fabrica_da_celula.py:118`. |

A porta possui anotação `DiaDoTurno`, mas sua guarda executável verifica apenas `None`; `NewType` não estabelece uma validação semântica nessa porta. Os testes novos verificam ausência/`None`, não proveniência do dia. Referências: `app/core/dia_do_turno.py:13`, `app/chamados/models.py:2402`, `app/chamados/models.py:2413`, `app/chamados/tests/test_b2_data_conversa.py:26`.

**7. Fallback disfarçado restante**

**Sim, nestes caminhos:**

- Sem batida-origem, o agrupador usa `data_evento_do_contexto`, que aceita `data_batida`, prefixo de `entrada_timestamp`, texto `pendentes` e `horario_previsto`. Não consulta `_T1` nem converte fuso antes de extrair esses dias. Referências: `app/chamados/services/disputa_emissao.py:1101`, `app/chamados/services/disputa_emissao.py:1114`, `app/chamados/data_evento.py:27`, `app/chamados/data_evento.py:31`, `app/chamados/data_evento.py:39`.
- O derivador aceita o dia do chamado; esse juiz tem degraus de dia civil da batida e timestamp cru do contexto. Referências: `app/chamados/services/disputa_emissao.py:231`, `app/chamados/services/disputa_emissao.py:247`, `app/chamados/juizes.py:53`, `app/chamados/juizes.py:162`.
- O gerador aceita `contexto[motivo].data` antes de consultar a batida, e depois o declara dia de célula sem conferir a origem. Referências: `app/chamados/services/disputa_emissao.py:364`, `app/chamados/services/disputa_emissao.py:4

[truncado em 20000 de 22829 caracteres]

## 06/11 19:55 · sessao ed5c1cb9db74 · ESMERIL-SEM-INSTANTE · verificador · origem=Code

**Parecer: a troca corrige o referencial UTC → dia civil local no filtro, mas não resolve a atribuição de batidas ao dia do turno.** Há uma restrição nova para timestamps naive e um residual real nas células alternadas; nenhum deles foi demonstrado ocorrendo em produção pelo dado fornecido. (`app/ponto/services/fechamento.py:204`, `:254`, `:261`; `app/ponto/turnos.py:46`)

A correspondência da cópia com o hash informado e o diff integral contra `5a3f8d0e` permanece **[nao verificado]**: não há metadados Git disponíveis. A execução completa da suíte Django também permanece **[nao verificado]**: Django não está instalado neste ambiente.

1. **Correção e contraexemplos**

   - **Caminho identificado de produção:** fechamento, espelho e PDF entregam listas carregadas por `batidas_apuraveis`; essa função consulta batidas não retratadas e ordena por timestamp. O campo não declara `null=True`, e a configuração usa `USE_TZ=True`. Isso sustenta o contrato esperado de instante aware; os objetos efetivamente devolvidos em produção continuam **[nao verificado]**. (`app/ponto/services/fechamento.py:633`; `app/ponto/services/espelho.py:1022`; `app/relatorios/pdf_espelho.py:272`; `app/ponto/turnos.py:2864`, `:2871`; `app/ponto/models.py:23`; `app/config/settings/base.py:95`)
   - **`timestamp=None`:** `data_local` devolve `None`, mas a comparação seguinte com `date` levanta `TypeError`. A porta inteira não se tornou None-safe; o código antigo também quebrava, ao chamar `.date()` sobre `None`. Não encontrei origem comprovada desse valor chegando pelos três carregadores. (`app/ponto/turnos.py:46`; `app/ponto/services/fechamento.py:204`, `:254`; filtro antigo no diff fornecido)
   - **Naive:** existe regressão da interface: antes `.date()` aceitava naive; agora `localtime` o rejeita. A conversão acontece inclusive antes de saber se alguma fatia rodará: uma batida naive com `escalas=[]` passa a falhar. A normalização defensiva do motor acontece depois dessa porta, portanto não protege esse caso. Origem naive de produção chegando aqui: **[nao verificado]**. (`app/ponto/services/fechamento.py:204`, `:205`, `:242`; `app/ponto/turnos.py:44`; `app/ponto/motor_calculo_v2.py:77`; `app/ponto/turnos.py:1528`)
   - **Iterador consumível:** o novo código consome `batidas` uma vez e reutiliza a lista materializada em todas as fatias. O antigo percorria o iterador novamente por fatia, deixando as posteriores sem batidas. Portanto, nesse ponto o diff melhora o comportamento. Os chamadores encontrados já usam listas. (`app/ponto/services/fechamento.py:204`, `:253`, `:633`; `app/ponto/services/espelho.py:1022`; `app/relatorios/pdf_espelho.py:272`)
   - **Ordem e identidade:** as compreensões preservam a ordem relativa e devolvem os mesmos objetos; não há ordenação ou deduplicação nova. Isso importa porque `remover_duplicatas` percorre a ordem recebida. (`app/ponto/services/fechamento.py:204`, `:253`, `:260`, `:262`; `app/ponto/motor_calculo_v2.py:905`)
   - **Leitores do comportamento antigo:** os três consumidores compartilham a porta, portanto recebem a alteração. Não encontrei contrato nesses chamadores exigindo dia UTC; suas buscas são construídas por limites locais. Dependência externa ou dinâmica do comportamento antigo: **[nao verificado]**. (`app/ponto/services/fechamento.py:622`, `:754`; `app/ponto/services/espelho.py:1019`, `:1164`; `app/relatorios/pdf_espelho.py:264`, `:419`)

2. **Quem escreve, quem lê e qual “dia”**

   | Valor | Escritor/origem e leitores |
   |---|---|
   | `b.timestamp` | Persistido por `registrar_batida`; também corrigido diretamente pela regularização externa. O carregador ORM o entrega; a porta lê e passa o objeto ao motor. **Instante**, não data de jornada. (`app/ponto/registro_batida.py:153`, `:158`; `app/chamados/services/regularizacao_ext.py:154`; `app/ponto/turnos.py:2871`; `app/ponto/services/fechamento.py:204`, `:287`) |
   | `fatia_ini`, `fatia_fim` | Escritos pelo encontro entre competência, vigência e piso operacional; depois podem ser reduzidos ao primeiro/último `_meus`. Lidos pelo filtro, cobertura, fábrica do motor, carimbo e cálculo. Limites **inclusivos** de datas nominais. (`app/ponto/services/fechamento.py:206`, `:207`, `:238`, `:261`, `:267`, `:279`, `:283`, `:287`) |
   | `fatia_ini_busca`, `fatia_fim_busca` | Escritos exclusivamente como limites finais −1/+1 dia; lidos no primeiro filtro, com ambas as pontas inclusivas. São envelope civil de contexto, não julgamento da jornada. (`app/ponto/services/fechamento.py:244`, `:245`, `:254`) |
   | `_meus` | Derivado das datas de células cujo vínculo é resolvido por `vinculo_do_dia`; complementado por dias sem dono cobertos exclusivamente pelo vínculo. Lido para pular/reduzir a fatia, filtrar batidas e declarar cobertura. (`app/ponto/services/fechamento.py:189`, `:209`, `:229`, `:233`, `:235`, `:238`, `:259`, `:267`) |
   | `_batidas_do_dia` | Novo cache de pares `(objeto, dia civil local do instante)`, escrito antes do laço e lido por cada fatia. (`app/ponto/services/fechamento.py:204`, `:253`) |
   | `batidas_fatia` | Escrita pelo envelope, restringida por `_meus`, desempacotada para objetos e entregue exclusivamente ao cálculo da fatia. (`app/ponto/services/fechamento.py:253`, `:260`, `:262`, `:287`) |
   | `data_local` | Implementada em `turnos.py`; devolve dia civil local de aware ou `None`. Não calcula `data_turno`. A nova porta reutiliza essa implementação. (`app/ponto/turnos.py:43`, `:46`; `app/ponto/services/fechamento.py:203`) |

   **Não são todos o mesmo significado de dia.** A célula possui uma data nominal D, mas seu marco de saída pode estar em D+1. O motor julga períodos por `data_jornada`, obtida do juiz de turno, com fallback local e possível herança da jornada anterior. Portanto, o comentário novo alinha o calendário/fuso, mas não demonstra equivalência entre **dia civil da batida** e **dia nominal da célula/jornada**. (`app/escala/models.py:1685`; `app/ponto/management/commands/gerar_celulas.py:67`; `app/ponto/services/fechamento.py:247`; `app/ponto/motor_calculo_v2.py:2403`, `:2408`, `:2429`)

   As competências e vigências também são datas cadastrais inclusivas, não dias UTC; o corte da competência é parametrizado, apesar do “21..20” usado na explicação dos testes. (`app/escala/models.py:875`, `:876`; `app/escala/alimentacao.py:214`; `app/ponto/janelas.py:15`, `:21`, `:57`; `app/ponto/tests/test_composicao_de_fatias_e_total.py:252`)

   Há ainda uma distinção de fuso: `data_local` chama `timezone.localtime` sem fuso explícito; o motor fixa `TZ_LOCAL` a partir de `settings.TIME_ZONE`. Igualdade entre fuso ativo e fuso configurado em toda execução de produção: **[nao verificado]**. (`app/ponto/turnos.py:46`; `app/ponto/motor_calculo_v2.py:28`, `:79`)

   Outros leitores diretos de `data_local`: signal, detector de par relâmpago e os auxiliares internos de turno, cruzamento de meia-noite e apresentação. (`app/ponto/signals.py:23`; `app/ponto/management/commands/detectar_par_relampago.py:303`; `app/ponto/turnos.py:284`, `:320`, `:1013`, `:1098`, `:1186`, `:1294`, `:2198`, `:2918`)

3. **Os testes novos mordem, com lacunas** [nao verificado]

   No ensaio em memória, usando os corpos da cópia e substitutos de `localtime/override`, os três passaram no novo e os três falharam com o filtro antigo reconstruído do anexo. Isso confirma a distinção dos cenários; execução na suíte Django real: **[nao verificado]**. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:290`, `:305`, `:315`)

   - O primeiro exige duas inclusões e uma exclusão; o segundo exige inclusão na borda e exclusão antes dela. Uma implementação que entrega tudo vazio falha nesses testes. Os `assertNotIn`, isoladamente, seriam vacuosos, mas estão acompanhados de inclusão positiva. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:301`, `:302`, `:303`, `:312`, `:313`)
   - O terceiro é **individualmente vacuável**: dois resultados vazios — inclusive `{}` — satisfazem a igualdade. Não exige conteúdo esperado. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:327`, `:328`, `:329`)
   - O capturador usa `sorted`, portanto esconde regressões de ordem. Os testes não exigem a distribuição correta em B, cardinalidade exata, identidade dos objetos ou dinheiro calculado pelo motor real. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:274`, `:280`, `:301`, `:303`, `:329`)
   - Não há relógio solto nos três casos: datas e instantes são literais. O `override` envolve toda execução da porta, cobrindo a conversão local necessária; não congela relógio, mas esses casos não precisam disso. Não cobre outros fusos nem a distinção fuso ativo/configurado. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:245`, `:268`, `:299`, `:310`, `:326`; `app/ponto/motor_calculo_v2.py:28`)
   - A cronologia “RED antes do código”, afirmada na docstring, não é demonstrada pelos anexos: **[nao verificado]**. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:254`)

4. **Classes históricas de reprovação**

   | Classe | Conferência |
   |---|---|
   | Segundo juiz | `data_local` é conversor existente; não aparece como juiz registrado. O juiz de atribuição ao turno é `turnos_do_colab`. `dia_das_batidas` responde dia atribuído para apresentação, usando `data_turno` e depois dia civil para não reclamadas. Para o envelope civil, `data_local` é adequado; para preservar uma saída pertencente à jornada anterior, essa conversão é insuficiente. (`app/core/juizes.py:196`; `app/ponto/turnos.py:43`, `:2875`, `:2878`, `:2916`, `:2918`) |
   | Segundo escritor fora da porta | O diff apenas lê timestamps. **Existe escritor externo na cópia:** regularização externa atribui e salva `timestamp` diretamente. Não foi introduzido pelo diff fornecido. (`app/ponto/services/fechamento.py:204`; `app/chamados/services/regularizacao_ext.py:149`, `:154`, `:155`) |
   | Relógio fora da borda | O trecho novo fornece explicitamente o timestamp; `None` não dispara leitura implícita de “agora”, pois `data_local` o intercepta. (`app/ponto/services/fechamento.py:204`; `app/ponto/turnos.py:46`) |
   | Derivação paralela | A conversão civil é delegada à função existente. O julgamento por jornada continua posterior, no motor; a divergência civil/jornada permanece no filtro de pertencimento. (`app/ponto/services/fechamento.py:203`, `:261`; `app/ponto/motor_calculo_v2.py:2403`) |
   | Selo/RED sem mordida | Há mordida para a troca de referencial, mas o terceiro assert aceita vazio e o capturador mascara ordem. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:280`, `:301`, `:312`, `:329`) |
   | Ordem de trava | O trecho alterado não acrescenta trava/transação; continua antes do contexto de alimentação e da chamada ao motor. Ordem global de travas da aplicação: **[nao verificado]**. (`app/ponto/services/fechamento.py:204`, `:278`, `:286`) |
   | Pouso antes do pré-requisito | O diff não demonstra impacto de frota nem atendimento às condições de pouso de cura monetária. A L-113 alterou a L-092 e preserva DIFF antes de ato de frota; não cabe tratar a antiga recusa de exportada como lei intacta. Atendimento operacional: **[nao verificado]**. (`app/docs/LEIS.md:112`, `:150`; `app/ponto/services/fechamento.py:485`, `:495`) |
   | Campo com dois significados | Nenhum campo persistido recebe significado novo neste trecho; permanece a comparação entre dia civil do instante e data nominal da célula, que divergem no cruzamento da meia-noite. (`app/ponto/services/fechamento.py:204`, `:261`; `app/ponto/management/commands/gerar_celulas.py:67`) |

5. **Residual de saída em D+1 pertencente a B**

   Considere as células dos testes: 01→A, 02→B, 03→A; entrada **01/10 às 22h locais** e saída **02/10 às 07h locais**. Ambas têm data UTC 02/10. No filtro antigo, A perde ambas; no novo, A recebe a entrada e continua perdendo a saída. B pode receber ambas, pois a entrada local de 01 está no pad externo de sua fatia reduzida ao dia 02. Portanto, **o defeito já existia; o diff melhora a entrada desse exemplo, mas não recupera o par em A**. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:294`; `app/ponto/services/fechamento.py:238`, `:244`, `:254`, `:261`; filtro antigo no diff fornecido)

   O motor não tem recuperação garantida: pareia as batidas recebidas; o caminho da ata exige encontrar a batida real correspondente à saída. Se B atribuir corretamente a jornada ao dia 01, seu recorte iniciado em 02 descarta esse período. O tratamento de dias órfãos tampouco cobre automaticamente o caso, pois D já foi declarado coberto por A. (`app/ponto/motor_calculo_v2.py:882`, `:2354`, `:640`, `:641`, `:2429`; `app/ponto/services/fechamento.py:267`, `:270`, `:796`)

   Não é correto afirmar que o diff “nunca toca” esse residual: saídas locais entre 21h e 23h59 mudam de chave UTC para local e podem mudar de pertinência. O que permanece é a exclusão por **dia civil de outro vínculo dentro da fatia**, sem consultar antes a propriedade da jornada. Impacto monetário desses casos em produção: **[nao verificado]**. (`app/ponto/services/fechamento.py:254`, `:261`; `app/ponto/turnos.py:2878`)

6. **Selo e ALLOWLIST**

   Os quatro asserts do contrato passaram em execução isolada do visitante/censo original, substituindo somente `SimpleTestCase` por `unittest.TestCase`. Execução completa sob Django: **[nao verificado]**. (`app/ponto/tests/test_contract_no_batida_date.py:236`, `:242`, `:251`, `:270`)

   **A entrada de `fechamento.py` continua não podre**, especificamente por `timezone.localtime(b.timestamp).date()` na linha 793. As extrações de `.entrada` dos períodos não são, sozinhas, justificativa para essa entrada: o visitante procura encadeamento/referência a `timestamp`. (`app/ponto/services/fechamento.py:793`, `:839`, `:935`, `:947`; `app/ponto/tests/test_contract_no_batida_date.py:185`, `:203`, `:208`, `:266`)

   O verde desse selo não certifica o filtro novo: o visitante não reconhece chamadas a `data_local`, e a exceção é por arquivo inteiro. Ele comprova ausência de novos ofensores reconhecidos e ausência de entradas podres, não correção da atribuição de jornada. (`app/ponto/tests/test_contract_no_batida_date.py:183`, `:201`, `:238`, `:266`)

## 06/11 19:54 · sessao b60fce496d2e · H12H06-B · sonda · origem=Code

Transcrição integral do dado fornecido [nao verificado]. Não executei novamente: o lançador grava arquivos em `bin/sonda_leitura.sh:132` e `bin/sonda_leitura.sh:250`, contrariando a regra de somente leitura.

```text
sonda_leitura: pre.py [f35cf407c2faf784] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

HASH_FM {"05": [615, "e29ff62650eebac5"], "06": [704, "9400c1a360842d79"], "07": [629, "2edced101a39a6c3"], "08": [618, "22459b830139a009"], "09": [607, "6e69a4d396e9f407"], "10": [624, "1cc01147a16e856b"], "11": [579, "5f2fd1dd77f3498b"], "12": [87, "afb9cb5ee9e2741c"]}
EXP {"pk": 2, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "80f417a64296887d8c1f58d580947e40aae78af2c21945c14485c0a96f9814c7", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 3, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 4, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 5, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 6, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 7, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 8, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "71570435dbbe5246f59b19986230ad265eece33da6015177bf5b806b75d45e6b", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 9, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "1f2b81f241a8f8cd344a4cd211911f20ab4cdd4ae25d65b416072bff7df2ac02", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 10, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "3c51aae49a29ab84f6b2c21885f796beca6038b483394138c44715743fa95a18", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 11, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "3c51aae49a29ab84f6b2c21885f796beca6038b483394138c44715743fa95a18", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 12, "empresa_id": 4, "mes": 7, "ano": 2026, "hash_sha256": "68d299ff806aceb36e414b75fd18e6ea9043d556b351645072448cf024af010c", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 13, "empresa_id": 4, "mes": 7, "ano": 2026, "hash_sha256": "491f6fff352cc5102cd0d0a5f24f004edc433e792d114563a4e77b529a216c2f", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 14, "empresa_id": 4, "mes": 7, "ano": 2026, "hash_sha256": "491f6fff352cc5102cd0d0a5f24f004edc433e792d114563a4e77b529a216c2f", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 15, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "1897e08736e104ef3d9eb81917bb26c2849f60a21e6f3141bfc2b35acdc08f63", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 16, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "1897e08736e104ef3d9eb81917bb26c2849f60a21e6f3141bfc2b35acdc08f63", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 17, "empresa_id": 3, "mes": 7, "ano": 2026, "hash_sha256": "7d54b0e6602bf4ea8bf9615009cb4e470398f96a9e66b8623dfc03ec4874524e", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 18, "empresa_id": 2, "mes": 8, "ano": 2026, "hash_sha256": "790d8cd7ea0c631c1b76cb462bc9ebadc163787ffd7e854b1d5b9ea0b0d73f01", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 19, "empresa_id": 2, "mes": 8, "ano": 2026, "hash_sha256": "2b3ac743a5782a5122fc89fb917405d9e9529d141f8bb6d81e0c61610b043dd0", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 20, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "8a63e3baabc501af7d63aa3845c6a9221d7f18ac091b5f7038650a23aaa59dbc", "invalidada_em": "2026-09-30 20:07:06.058663+00:00", "substituida_por_id": 23}
EXP {"pk": 21, "empresa_id": 3, "mes": 9, "ano": 2026, "hash_sha256": "6e0f1048428293078bc82c21aa8756630446c394ef2c2a5b16cbb1efbb6626d8", "invalidada_em": "2026-09-30 20:07:07.142119+00:00", "substituida_por_id": 24}
EXP {"pk": 22, "empresa_id": 4, "mes": 9, "ano": 2026, "hash_sha256": "cb7f00fe59f84e505fce92aab2769242aec6671b688d7c514311518345c29773", "invalidada_em": "2026-09-30 20:07:07.385295+00:00", "substituida_por_id": 25}
EXP {"pk": 23, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "0e3e008b49b1ff73a89556a24e495f0e05f566cad644d8354f224300bbc5a703", "invalidada_em": "2026-10-01 14:57:23.407593+00:00", "substituida_por_id": 26}
EXP {"pk": 24, "empresa_id": 3, "mes": 9, "ano": 2026, "hash_sha256": "5c503b95f9f9cd35541db29a45a6447f026e272b9eedf27b346cda26b92810a6", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 25, "empresa_id": 4, "mes": 9, "ano": 2026, "hash_sha256": "84c78cd0871f5f5251db047a6d12054930f91c05a39ec46d6669fddc1d97a73b", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 26, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "0ae5da67364cabfb5dcadde9481ec75d63b702ca95956627adbbb37079664d53", "invalidada_em": "2026-10-01 22:44:56.762182+00:00", "substituida_por_id": 27}
EXP {"pk": 27, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "361d0f9685f86d3a51d49b8f5194df469db8e4818c9cae0e9bd9f1461f9af0cc", "invalidada_em": null, "substituida_por_id": null}
AUS5198 {"pk": 5198, "data": "2026-10-24", "minutos": 720, "tipo": "falta"}
REG_PEND {"n": 0, "itens": []}
REG_PEND_FROTA 2
DISP_ABERTAS {"n": 23}
HASH_DP 9 11501 54c6419c61520d6a
HASH_DP 10 21418 9d68332a36ef63eb
ERROS {}
```

- REG_PEND: n=0; itens=[] [nao verificado].
- DISP_ABERTAS: n=23 [nao verificado].
- HASH_FM: 09=[607, "6e69a4d396e9f407"]; 10=[624, "1cc01147a16e856b"] [nao verificado].
- HASH_DP: 9=11501, 54c6419c61520d6a; 10=21418, 9d68332a36ef63eb [nao verificado].
- AUS5198: 720 minutos; ERROS={}; exceção não indicada no dado fornecido [nao verificado].

## 06/11 19:52 · sessao 1e760cda5396 · - · - · origem=Ronald (tmux)

Achados centrais:

- **Conversa continua sendo lida como dia do turno.** `abrir_conversa` grava `localdate()` (`app/chamados/models.py:2426`), mas `conversa` é só parâmetro de `_nascer`; o `create` não persiste essa distinção (`app/chamados/models.py:2498`). O C6 passa qualquer `p.data_evento`, inclusive de `escolha`, a `classificar_falta` antes do ramo de materialização (`app/chamados/juizes.py:1198`, `app/chamados/juizes.py:1202`).
- **Dedup e célula têm caminho concreto de colisão.** Reabertura administrativa aceita qualquer motivo do catálogo (`app/chamados/services/acoes_disputa.py:245`, `app/chamados/services/acoes_disputa.py:247`) e abre conversa `escolha` com esse motivo (`app/chamados/services/acoes_disputa.py:260`). Depois, abertura datada chama `do_fato`, que filtra apenas colaborador/motivo/data (`app/chamados/models.py:2396`, `app/chamados/models.py:2453`): pode recuperar essa conversa, atribuir-lhe a célula do turno (`app/chamados/models.py:2457`) e devolvê-la sem criar a pergunta `hora` (`app/chamados/models.py:2497`). A fábrica da célula também considera qualquer pergunta do mesmo dia/motivo como já perguntada, sem distinguir conversa (`app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/fabrica_da_celula.py:126`). Ocorrência em produção: [nao verificado].
- **A tela ainda chama o campo de dia civil do plantio.** O modal lê a data pelo filtro (`app/templates/chamados/partials/modal_fio.html:161`; `app/chamados/templatetags/chamados_extras.py:31`; `app/chamados/services/fio.py:87`) e exibe “(e aqui que a batida nasce)” (`app/templates/chamados/partials/modal_fio.html:166`). Isso contradiz o noturno: combina D com a hora e ajusta a saída para D+1 (`app/chamados/services/materializacao.py:414`, `app/chamados/services/materializacao.py:421`; `app/chamados/juizes.py:319`).

Inventário de leitores, agrupado. “Turno” abaixo significa que a leitura usa a data como chave do fato/célula/turno/competência; “exibição” apenas imprime/ordena a data, sem converter instante.

| Família e significado | Leitores de produção encontrados |
|---|---|
| Plantio, slot e turno | `_ts_tipo_do_plantio`, `_tipo_pelo_marco`: `app/chamados/services/materializacao.py:344`, `app/chamados/services/materializacao.py:279`; `_ajustar_saida_overnight`: `app/chamados/juizes.py:312`; `_slot_esperado` delega ao plantio em `app/chamados/juizes.py:346`; `_plantio_na_hora_do_slot`: `app/chamados/juizes.py:266`; `_turno_tem_saida`: `app/chamados/juizes.py:363`; `classe_do_lote` usa slot em `app/chamados/services/validacao.py:825`. |
| Julgamento de pergunta, célula e janela | `_celulas_da_pergunta`: `app/chamados/juizes.py:623`; `_pergunta_materializada_de_fato`: `app/chamados/juizes.py:659`, `app/chamados/juizes.py:666`, `app/chamados/juizes.py:750`, `app/chamados/juizes.py:754`; `_pergunta_viva_calc`: `app/chamados/juizes.py:1198`, `app/chamados/juizes.py:1202`; `dia_da_pergunta_trancado`: `app/chamados/juizes.py:932`; `_marco_esperado_da_grade`: `app/chamados/juizes.py:1524`; `resolvedora_valida`: `app/chamados/juizes.py:1964`; `_chave_memo_pv` somente memoriza o insumo: `app/chamados/juizes.py:815`. |
| Dia do chamado e competência | `_dia_da_disputa`: `app/chamados/juizes.py:76`; `dias_do_fato`: `app/chamados/juizes.py:209`; `_dias_das_perguntas_vivas`: `app/chamados/juizes.py:2030`; `_vivas_entre` / `_plano_pergunta_viva`: `app/chamados/models.py:351`, `app/chamados/models.py:377`; `via_pelo_fato` recebe a data em `app/chamados/services/disputa_emissao.py:1652`, `app/chamados/services/disputa_emissao.py:1724`, `app/chamados/services/relavra_via.py:101`, `app/chamados/services/materializacao.py:1241` e consulta ausência/turno em `app/chamados/juizes.py:1476`, `app/chamados/juizes.py:1480`. O ramo adesão recorta explicitamente o **dia civil correspondente à chave**, por desenho anterior, em `app/chamados/juizes.py:1461`. |
| Identidade e dedup | `do_fato` / `_nascer`: `app/chamados/models.py:2396`, `app/chamados/models.py:2452`; `gerar_perguntas_disputa` / `_ja_perguntou`: `app/chamados/services/disputa_emissao.py:389`, `app/chamados/services/disputa_emissao.py:405`, `app/chamados/services/disputa_emissao.py:1272`; `_candidata_reusavel` / `insumos_em_regularizacao`: `app/chamados/services/declaracao_texto.py:227`, `app/chamados/services/declaracao_texto.py:539`, `app/chamados/services/declaracao_texto.py:542`; `migrar_resolvedoras_vivas`: `app/chamados/services/fio.py:941`; `motivos_a_perguntar` / `perguntar`: `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:137`; `retencao` / `_marco_ja_perguntado`: `app/chamados/services/pergunta_no_app.py:92`, `app/chamados/services/pergunta_no_app.py:109`. |
| Materialização de ausência e validação | `_declarar_ausencia_de_pergunta` / `_materializar_ausencia_de_resposta` / `perguntas_absorvidas_por`: `app/chamados/services/materializacao.py:473`, `app/chamados/services/materializacao.py:517`, `app/chamados/services/materializacao.py:607`; `_aceitar_a_folga`: `app/chamados/services/validacao.py:250`; `validar_pergunta` manda recalcular a data em `app/chamados/services/validacao.py:407`; `conferir_data_do_fato` usa `data_do_fato_de` e registra o valor em `app/chamados/services/validacao.py:624`. |
| Reconciliador | `lavrar_sem_turno_aberto`: `app/chamados/reconciliador.py:370`; `_dia_fora_do_alcance_c`: `app/chamados/reconciliador.py:726`; `_orfas_abertas`: `app/chamados/reconciliador.py:1040`; `veredito_orfa`: `app/chamados/reconciliador.py:1072`, `app/chamados/reconciliador.py:1080`; `semear_eventos_celula`: `app/chamados/reconciliador.py:1293`, `app/chamados/reconciliador.py:1297`; `reatar_pergunta_orfa`: `app/chamados/reconciliador.py:1543`, `app/chamados/reconciliador.py:1553`; `encerrar_orfas_pela_lampada` imprime data em `app/chamados/reconciliador.py:1142`. |
| Costura, eventos e lavra | `data_do_fato_de`: `app/chamados/services/fio.py:87`; `_costura_nao_mata_o_fio`: `app/chamados/services/fio.py:773`; `reapontar_resolvedora` só escreve quando a leitura detecta data ausente: `app/chamados/services/fio.py:835`; `linhas_do_evento` / `acordar_celulas` ligam por colaborador/data: `app/chamados/models.py:2690`, `app/chamados/models.py:2706`; `lavrar` calcula expiração: `app/chamados/veredito_lavrado.py:229`; `expira_em` lê a data como chave de competência: `app/chamados/veredito_lavrado.py:112`. |
| Outros serviços de dia/competência | `medir` / `reabrir`: `app/chamados/services/competencia_trancada.py:107`, `app/chamados/services/competencia_trancada.py:252`; `_dia_do_plantio` / `_plantar_o_dia`: `app/chamados/services/passivo_validado.py:113`, `app/chamados/services/passivo_validado.py:519`; `filhos_no_defeito`: `app/chamados/services/limbo_folgas.py:202`; `relavrar_dia`: `app/chamados/services/relavra_via.py:152`; `carimbos_sem_lastro_por_dono`: `app/chamados/services/vigia_fio.py:816`. |
| Folha, célula e calendário | `montar_previa` agrupa pergunta por dia da célula: `app/folha/previa.py:65`, `app/folha/previa.py:67`, `app/folha/previa.py:95`; `contexto_calendario`: `app/colaboradores/services/calendario.py:473`, `app/colaboradores/services/calendario.py:477`; `dia_do_colab`: `app/ponto/services/dia_do_colab.py:65`; `fechar_o_fio_do_dia`: `app/ponto/services/veredito_celula.py:176`. |
| Canal e cobrança | `perguntas_mudas_com_resposta` casa data da pergunta com dia do chamado em `app/chamados/canal.py:72`, cujo dia vem de `dia_do_fato` em `app/chamados/canal.py:45`; `checar_cobranca` imprime “sobre o seu ponto nos dias…” a partir do campo em `app/chamados/services/cobranca.py:83`, `app/chamados/services/cobranca.py:133`; `aviso_texto_livre`: `app/chamados/services/aviso_pendencia.py:20`. |
| API, textos, templates: exibição/ordenação | `api_chamados_v2`: `app/api/views.py:1174`; `api_disputa_responder` log: `app/api/views.py:2654`; `_item_validar`: `app/api/views_mensageria.py:568`; `validar_inline`: `app/chamados/views_cobrar.py:176`, `app/chamados/views_cobrar.py:185`; `explicar_pergunta` / `por_que_nao_fecha`: `app/chamados/services/explicador.py:70`, `app/chamados/services/explicador.py:122`, `app/chamados/services/explicador.py:126`; `_eventos_dos_chamados`: `app/chamados/services/historico.py:108`; resposta/log: `app/chamados/services/disputa_emissao.py:1355`, `app/chamados/services/disputa_emissao.py:1376`; duplicata/log: `app/chamados/services/fio.py:897`; templates diretos: `app/templates/chamados/partials/_card_atendimento.html:90`, `app/templates/chamados/partials/_card_atendimento.html:164`, `app/templates/chamados/partials/_lote_preview.html:24`, `app/templates/chamados/meu_atendimento.html:19`; filtros indiretos: `app/chamados/templatetags/chamados_extras.py:31`, `app/chamados/templatetags/chamados_extras.py:44`, `app/chamados/templatetags/chamados_extras.py:58`; modal: `app/templates/chamados/partials/modal_fio.html:161`. |
| Comandos | `dedup_perguntas_fato`: `app/chamados/management/commands/dedup_perguntas_fato.py:54`, `app/chamados/management/commands/dedup_perguntas_fato.py:57`, `app/chamados/management/commands/dedup_perguntas_fato.py:69`; `disparar_perguntas_competencia`: `app/chamados/management/commands/disparar_perguntas_competencia.py:213`; `reabrir_orfas_enterradas`: `app/chamados/management/commands/reabrir_orfas_enterradas.py:68`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:86`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:144`; `backfill_veredito_lavrado`: `app/chamados/management/commands/backfill_veredito_lavrado.py:111`; `encerrar_perguntas_com_fato` só imprime: `app/chamados/management/commands/encerrar_perguntas_com_fato.py:74`; `mapa_divergencia` cruza furo/pergunta por dia: `app/ponto/management/commands/mapa_divergencia.py:272`; vigia compara turno da batida com pergunta: `app/ponto/management/commands/vigia_plantio_overnight.py:85`, `app/ponto/management/commands/vigia_plantio_overnight.py:89`. |
| Auditor de invariantes | `_candidatos_i8`: `app/chamados/management/commands/auditar_invariantes_chamados.py:63`, `app/chamados/management/commands/auditar_invariantes_chamados.py:73`; `_contraprova_i8bx`: `app/chamados/management/commands/auditar_invariantes_chamados.py:148`; `_cobre_ausencia_i21`: `app/chamados/management/commands/auditar_invariantes_chamados.py:162`; `_piso_f3_antes_do_gerador`: `app/chamados/management/commands/auditar_invariantes_chamados.py:282`; `_contraprova_f3g`: `app/chamados/management/commands/auditar_invariantes_chamados.py:298`; `contraprova_piso`: `app/chamados/management/commands/auditar_invariantes_chamados.py:325`, `app/chamados/management/commands/auditar_invariantes_chamados.py:348`, `app/chamados/management/commands/auditar_invariantes_chamados.py:361`, `app/chamados/management/commands/auditar_invariantes_chamados.py:373`; `handle`: `app/chamados/management/commands/auditar_invariantes_chamados.py:892`, `app/chamados/management/commands/auditar_invariantes_chamados.py:914`, `app/chamados/management/commands/auditar_invariantes_chamados.py:926`, `app/chamados/management/commands/auditar_invariantes_chamados.py:930`, `app/chamados/management/commands/auditar_invariantes_chamados.py:957`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1662`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1705`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1775`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1801`. |

Sobre plantio de conversa: as portas normais de reabertura usam `escolha` (`app/chamados/services/acoes_disputa.py:263`) e âncora usa `data_hora` (`app/chamados/catalogo/perguntas.py:118`). `_slot_esperado` de `data_hora` lê a data declarada na resposta, não `data_evento` (`app/chamados/juizes.py:334`). O vigia novo só inclui `hora`, portanto exclui essas conversas normais (`app/ponto/management/commands/vigia_plantio_overnight.py:74`).

Porém `abrir_conversa` não restringe tipo/motivo (`app/chamados/models.py:2419`), e o gerador passa o tipo do catálogo ao fallback de conversa (`app/chamados/services/disputa_emissao.py:412`). Um contexto `data` malformado ainda produz texto (`app/chamados/catalogo/query.py:62`), falha no parse da data (`app/chamados/services/disputa_emissao.py:365`) e pode chegar como conversa `hora` quando não houver data derivável (`app/chamados/services/disputa_emissao.py:410`). Nesse caso slot/plantio/vigia lerão `localdate` como turno (`app/chamados/services/materializacao.py:344`; `app/ponto/management/commands/vigia_plantio_overnight.py:89`). Existência de contexto assim em produção: [nao verificado].

Os testes novos de conversa só verificam nascimentos distintos/localdate e recusa de `abrir` sem data; não cobrem a conversa já gravada sendo encontrada por abertura datada ou fábrica da célula (`app/chamados/tests/test_b2_data_conversa.py:14`, `app/chamados/tests/test_b2_data_conversa.py:26`).

## 06/11 19:52 · sessao e55731efd96a · H12H06-B · leitura · origem=Code

**O `--apply` não tem uma cerca global que limite todos os efeitos à competência 11/2026.** Ele limita a seleção direta às 30 ausências, mas chama escritores e signals capazes de alcançar chamados, disputas, regularizações, batidas e recálculos de outros meses. O snapshot também não reverte essa árvore inteira. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:138–185`; `app/ponto/signals.py:143–147,250–272`; `app/chamados/signals.py:97–121`)

Estado efetivo do banco, ocorrência dessas ramificações nas 30 ausências e quantidades realmente escritas: **[nao verificado]**.

1. **Escritas diretas e indiretas**

   O comando chama `editar_ausencia_registro(..., minutos=None)`. O escritor zera os minutos e executa **`save()` completo**, sem `update_fields`; a falta aprovada deriva minutos pela grade. A conferência posterior permite diferenças apenas em `minutos` e `atualizado_em`, mas compara **somente a própria Ausencia**, não as entidades laterais. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:84,165–180`; `app/ponto/services/ausencia.py:498–509`; `app/ponto/models.py:877–908`)

   Há três momentos de efeitos: cartório síncrono durante o `save`, callbacks após o commit e processamento posterior de `EventoCelula` por outro comando. O contador `aplicadas` não conta essas escritas laterais. (`app/ponto/signals.py:85–93,143–147,250–272`; `app/chamados/management/commands/consumir_eventos_celula.py:31`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:269–274`)

2. **Pode alcançar competência exportada ou datas externas?**

   - **Guarda direta existente:** exige competência inicial 11/2026 e consulta entrega de cada dia de `data..data_fim`; sem fim, consulta somente `data`. “Entregue” significa `ExportacaoDominio` vigente e não invalidada, por empresa; não é uma checagem de `FechamentoMensal.status='exportado'`. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:98–116`; `app/chamados/services/vigia_fio.py:838–844`; `app/folha/export.py:580–586`)
   - **Outro FechamentoMensal:** pode ser invalidado. Para `data_fim=None`, o escritor estende a invalidação até **hoje**, enquanto a cerca examinou apenas o dia inicial. O UPDATE não filtra entrega nem status. Com corte 21, em 06/11 isso ainda termina na comp11; numa execução posterior ao corte, pode alcançar outro mês exportado. (`app/ponto/services/ausencia.py:519–524`; `app/ponto/services/fechamento.py:1633–1640`; `app/ponto/janelas.py:24–33`)
   - **Recálculo de outro mês exportado:** há caminho condicional: fechar disputa → retomar regularizações confirmadas de qualquer data do colaborador → criar batida → recalcular competência da batida. A regularização omite `fato_de_chao=False`; com `autoridade_humana=False`, a porta deriva `True` e pula a guarda de competência. O recálculo permite exportadas, salvo a guarda específica de **09/2026**. (`app/chamados/signals.py:106–121`; `app/chamados/services/regularizacao_ext.py:77–82,218–224`; `app/ponto/registro_batida.py:130–137,157–160`; `app/ponto/services/fechamento.py:36–43,530–565,1791–1808`)
   - **Ausencia fora de 21/10–20/11:** a seleção direta não aceita outra identidade/data inicial, mas a própria lista contém `#5333`, de **30/10 a 28/12/2026**. Não existe guarda exigindo término até 20/11; basta que todos os dias do range estejam não entregues. Com corte 21, esse range alcança também comp12 e comp01/2027. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:78,110–116,142–146`; `app/ponto/janelas.py:24–33`)
   - **Outra Ausencia fora da lista:** não identifiquei escrita nos caminhos examinados; a materialização acionada pelo fechamento de disputa apenas carimba perguntas. Garantia universal sobre qualquer caminho adicional: **[nao verificado]**. (`app/chamados/services/materializacao.py:1393–1407`)
   - **ExportacaoDominio:** nos percursos localizados é consultada para cerca, warning e guarda09; não encontrei mutação dessa entidade. Escrita por algum caminho adicional: **[nao verificado]**. (`app/folha/export.py:580–586`; `app/ponto/services/fechamento.py:530–565,1491–1499`)

   A checagem de entrega também não é uma trava contra exportação concorrente: somente a Ausencia recebe `select_for_update`; a consulta de exportação não recebe lock. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:138–139,160–170`; `app/folha/export.py:581–586`)

3. **Snapshot anterior e trilha**

   **Sim, antes de chamar o escritor**, em `<settings.BASE_DIR>/logs/h12h06b/snapshot_apply_<YYYYmmdd_HHMMSS_microsegundos>.jsonl`. Cada foto recebe `write`, `flush` e `fsync` antes da edição. Guarda identidade, minutos encontrados, alvo e `atualizado_em`; não guarda as entidades indiretas. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:120–123,165–170,215–217,249–259`)

   A trilha principal registra usuário de `--por`, ação, PK, antes/depois de minutos e motivo fixo “rederivado pela regra”. Usuário inexistente impede execução; falha dessa trilha provoca rollback da tentativa. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182–192,209–214`; `app/core/observ.py:134–137`)

   **Reversão parcial:** somente fotos confirmadas após o commit entram no plano; exige identidade, minutos e carimbos compatíveis. Restaurar 720 na falta é recusado pelo escritor como `tipo_nao_parcial`; isso está explicitamente documentado nos testes. A reversão não restaura carimbos antigos, FechamentoMensal, DiaPago, batidas ou notificações. Uma falha na gravação da confirmação pode deixar alteração já commitada sem foto elegível à reversão. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:152–159,229–242,267–274`; `app/ponto/services/ausencia.py:480–484`; `app/ponto/tests/test_h12h06b_rederivar_minutos.py:356–378`)

4. **Idempotência: segunda rodada escreve zero?**

   **Zero saves/eventos para cada ausência já correta e com identidade preservada:** o retorno `ja_aplicada` ocorre antes do snapshot da linha, escritor e evento. Existe teste com duas fixtures verificando carimbos e eventos intactos. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:142–148,165–185`; `app/ponto/tests/test_h12h06b_rederivar_minutos.py:131–143`)

   **Zero escrita literal, não:** todo `--apply` ainda cria snapshot e arquivo de resultado. Linhas anteriormente recusadas ou puladas podem voltar a ser tentadas; portanto não há prova de “segunda execução inteira = zero escritas” sem conhecer o resultado da primeira. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:147–172,248–259,280–287`)

5. **Saída que serve como prova**

   O comando imprime por linha `pk`, colaborador, tipo, data, resultado, antes, depois e motivo; ao final imprime:

   ```text
   RESUMO aplicadas=N ja_aplicadas=N mudaria=N puladas=N recusadas=N [nao verificado]
   ```

   Não existe contador explícito `lidos`; a soma dos cinco contadores corresponde às tentativas concluídas. `aplicadas` conta ausências aceitas, não linhas SQL nem efeitos indiretos. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:261–276,288–289`)

   O JSON `resultado_apply_<timestamp>.json` contém `linhas`, `conta` e `completo`. O stdout informa caminho e SHA-256 do snapshot. **`SNAPSHOT ... linhas=N` conta registros JSONL**, incluindo foto e confirmação — normalmente dois por aplicada, além de fotos de tentativas desfeitas. Em exceção, o `finally` grava resultado parcial; o `RESUMO` posterior pode não ser atingido. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:253–259,273–289`)

   Isso prova o processamento da **lista fechada**, não a cobertura de toda a frota nem a conclusão do outbox. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:244,285`; `app/chamados/reconciliador.py:1304–1316`)

6. **Universos incluídos e excluídos**

   A lista tem **30 atos, 22 colaboradores, 18 atestados e 12 faltas**; por status, **16 aprovadas, 11 aguardando_documento e 3 aguardando_decisao** — contagem derivada dos tuples. Tudo fora desses PKs fica fora da seleção direta. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:50–81,244`)

   Não há filtro direto de empresa nem de colaborador ativo/desligado: busca por PK e confere identidade. Ausência não aprovada **não está genericamente excluída**, pois há 14 delas na lista. Empresa efetiva alimenta a cerca; não integra a identidade fotografada. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:109–116,120–123,138–146`; `app/ponto/services/ausencia.py:474–509`)

A tabela abaixo reúne os escritores alcançáveis identificados; efeitos condicionais efetivamente ocorridos: **[nao verificado]**. “Reversível” significa **pelo `--reverter` desse comando**; o snapshot contém apenas Ausencia. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:120–123,238–242`)

| escrita | arquivo:linha | guarda | reversível? |
|---|---|---|---|
| **Ausencia — save completo:** `colaborador,tipo,data,data_fim,minutos,dias_corridos,hora_ini,hora_fim,hora_extra_100,descricao,documento,prazo_documento,chamado_origem,documento_referencia,status,registrado_por,aprovado_por,aprovada_em,criado_em,atualizado_em`. Diferenças aceitas somente em `minutos,atualizado_em`; normalização de `aprovada_em/dias_corridos/data_fim` causa rollback se alterar outro campo. | `app/ponto/services/ausencia.py:498–509`; `app/ponto/models.py:743–814,868–908`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:175–180` | PK, identidade, minutos esperados, cerca e conferência posterior. | **Parcial:** minutos de atestado, sujeito às guardas; falta→720 é recusada. Carimbo antigo não volta. |
| `FechamentoMensal.derivado_em=NULL, atualizado_em=now` | `app/ponto/services/ausencia.py:519–524`; `app/ponto/services/fechamento.py:1633–1640` | Colaborador + competências do intervalo + `derivado_em` não nulo; **sem guarda de exportação/status**. | Não. |
| `CelulaDia.veredito,veredito_via,veredito_em,protestos,ata,status_dia,status_rotulo` | `app/ponto/portas/celula.py:961–966,995–1041` | Célula existente, teto temporal; lavra igual poupa escrita; decisão admin preserva veredito. | Não. |
| `CelulaDia.julgada_em,impressao,ausencia,tem_entrada,tem_saida,insumos_em`; `ata.precedencia` | `app/ponto/portas/celula.py:1055–1082` | Insumos/precedência iguais retornam sem save. | Não. |
| **INSERT EventoCelula:** `celula,colaborador,data`, PK e defaults `criado_em,processado_em,tentativas,erro` | `app/ponto/portas/celula.py:890–902`; `app/escala/models.py:1857–1863`; `app/chamados/signals.py:214–221` | Escrita de ata/veredito ou campos de pergunta que acordam; exige célula. | Não. |
| **Consumidor posterior:** `EventoCelula.tentativas,erro,processado_em`; **DELETE** de eventos antigos processados | `app/chamados/reconciliador.py:1377–1393` | Eventos pendentes por lote; retenção consulta eventos antigos, sem limitar aos PKs deste apply. | Não. |
| **INSERT ChamadoColaborador:** `colaborador,modulo_origem,status_local,celula,assunto,titulo,descricao,area,categoria,subcategoria,urgencia,prazo_sla,contexto_json`, demais defaults da linha | `app/ponto/management/commands/detectar_ausencias.py:693–713`; `app/ponto/services/cluster_espurio.py:130–147`; `app/chamados/models.py:899–941` | Funil do emissor, dedup, célula não absolvendo e veto da régua. Cluster exige ativo/escala e usa janela própria. | Não. |
| `ChamadoColaborador.status_local,resolvido_em,via_encerramento,atualizado_em,urgencia,urgencia_original` — fechar/reabrir/reconciliar | `app/chamados/models.py:195–295,607–620`; `app/ponto/services/cartorio.py:1488–1525` | Motor, premissa e pergunta viva; pode alcançar outros chamados do colaborador. | Não. |
| `ChamadoColaborador.contexto_json,atualizado_em`; `celula,atualizado_em` | `app/chamados/services/contexto.py:81–102,128–159`; `app/chamados/services/vinculos.py:92–99`; `app/chamados/reconciliador.py:1352–1353` | Contexto diferente/fusão nova; célula ainda não apontada. | Não. |
| `ChamadoColaborador.silenciado_ate,silenciado_por,silenciado_motivo,atualizado_em` | `app/chamados/services/silencio_ausencia.py:77–91,119–127`; `app/chamados/services/arquivo.py:159–172` | Ausência aberta cobrindo fato; preserva silêncio vivo de outro motivo. Pode renovar prazo. | Não. |
| Limpeza dos mesmos campos de silêncio | `app/chamados/services/silencio_ausencia.py:68–75,93–115`; `app/chamados/services/arquivo.py:193–209` | Marcador de ausência; `resgatado=False` nesse caminho, portanto não carimba `resgatado_em`. | Não. |
| **INSERT DisputaSupervisao:** `colaborador,motivos,contexto,chamado`, demais defaults; **UPDATE** `motivos,contexto` | `app/chamados/services/disputa_emissao.py:476–528,923–999,1090` | Reutilização/merge de disputa; contexto pode incrementar `qtd/ultima_em`. | Não. |
| `DisputaSupervisao.fechada_em,fechada_por,resolucao,motivo_fechamento` | `app/chamados/models.py:1820–1869,2081–2091`; `app/chamados/services/becos.py:36–54` | Fechamento exige ausência de pergunta viva/resposta sem veredito; reabertura tem porta própria. | Não. |
| **INSERT PerguntaDisputa:** `disputa,motivo,data_evento,chamado_destino,celula,pergunta,tipo_resposta,opcoes,chamados_relacionados_ids`, demais defaults | `app/chamados/models.py:2398–2477`; `app/chamados/services/disputa_emissao.py:389–397,767–778` | Identidade global colaborador/motivo/data, resolvedora válida e dedup. | Não. |
| `PerguntaDisputa.celula,chamado_destino,data_evento,veredito_lavrado,julgado_em,janela_expira_em` | `app/chamados/models.py:2433–2474`; `app/chamados/services/fio.py:831–848`; `app/chamados/veredito_lavrado.py:227–231` | Reapontamento validado; data somente quando fornecida e ausente; relavra pode escrever os três carimbos. | Não. |
| `PerguntaDisputa.materializada_em,materializada_por,via_resolucao`; conforme ramo, `respondida_em,validada_em,validada_por,veredito_lavrado,julgado_em,janela_expira_em` | `app/chamados/models.py:2585–2609,2646–2652,2699–2700`; `app/chamados/services/disputa_emissao.py:1617–1618` | C6 pode recusar carimbo; ressuscitação/descarimbo limpa campos; S144 também carimba perguntas. | Não. |
| **INSERT FollowupChamado:** `chamado,autor_tipo,autor_user,conteudo,anexo_hash,evento_chave`, PK/defaults/carimbo | `app/chamados/services/nota.py:88–109`; `app/chamados/models.py:1598–1620` | Chave deduplica quando há `objeto_ref`; outras notas dependem da transição guardada. | Não. |
| **INSERT LogAuditoria:** `usuario,acao,modelo,objeto_id,descricao,valor_antes,valor_depois,ip,ator,criado_em` | `app/core/observ.py:118–137`; `app/core/models.py:295–307` | Principal obrigatória; trilhas dos efeitos laterais geralmente best effort. | Não; histórico permanece. |
| **INSERT Pauta:** `remetente_departamento,destinatario_departamento,destinatario_user,ancora_tipo,ancora_id,texto,prazo,pai,autor_user`, PK/defaults/carimbo | `app/chamados/services/fabrica_da_celula.py:171–172,225–241`; `app/pautas/services.py:180–188` | Limbo em competência entregue, motivos ainda sem pauta; possível em dias externos alcançados pela árvore. | Não. |
| **INSERT/UPDATE ContadorPauta:** `departamento,user,lavrado_em,nao_lidas,atrasadas` | `app/pautas/services.py:344–357` | Lavratura do destinatário da pauta criada. | Não. |
| **INSERT auth.User**, `username='sistema'` e defaults | `app/core/utils.py:18` | Somente se usuário sistema não existir, quando efeito indireto o pede. | Não. |
| **DELETE PushSubscription**; envio externo de push | `app/ponto/management/commands/detectar_ausencias.py:774–783`; `app/chamados/push_utils.py:40–51,110–111` | Subscription ativa; DELETE se resposta 404/410. | Não; push enviado não é recolhido. |
| `Colaborador.fcm_token='',fcm_removido_em,fcm_removido_motivo` | `app/chamados/services/disputa_emissao.py:1123–1124,1547–1548`; `app/chamados/fcm_utils.py:89–94`; `app/colaboradores/fcm.py:55–67` | Envio FCM acusa token inválido; token ainda preenchido. | Não. |
| `RegularizacaoExterna.status,materializada_em,materializada_por` | `app/chamados/signals.py:106–121`; `app/chamados/services/regularizacao_ext.py:197–224` | Confirmada, não materializada, mesmo colaborador e sem disputa aberta; **sem recorte de data**. | Não. |
| `RegularizacaoItemBatida.batida_alvo,aplicada_em,batida_entrada,batida_saida` | `app/chamados/services/regularizacao_ext.py:125–188` | Item ainda não aplicado/FK não preenchida. | Não. |
| **INSERT Batida:** `colaborador,timestamp,tipo,origem,aprovada,aprovada_por,aprovada_em,justificativa`, demais defaults; **UPDATE** `tipo,timestamp`; **DELETE** Batida | `app/chamados/services/regularizacao_ext.py:77–82,136,153–155`; `app/ponto/registro_batida.py:157–160` | Regularização/item; dedup/colisão no INSERT. Guarda de competência é pulada no plantio desse ramo. | Não. |
| Por DELETE Batida: `Justificativa.batida_entrada,batida_gerada=NULL`; `AlertaGeofence.batida=NULL`; FKs `RegularizacaoItemBatida.batida_entrada,batida_saida,batida_alvo=NULL`; **DELETE CASCADE PingGeo,FotoBatida,TurnoMaterializado** | `app/ponto/models.py:139–140,167–168,238,1000–1002,1027–1028,1206–1209`; `app/chamados/models.py:3267–3281` | Dependentes da batida removida. Exclusão de arquivos físicos: **[nao verificado]**. | Não. |
| **DELETE/recriação TurnoMaterializado:** `colaborador,data_turno,entrada,saida,aberto,cross_meianoite,dt_fim_previsto,atualizado_em` | `app/ponto/signals.py:19–36,48–50`; `app/ponto/nucleo.py:48–66,80–83`; `app/ponto/models.py:1213` | Recorte D−2…D+1 da batida; sem cerca de exportação nesse escritor. | Não. |
| `Colaborador.onboarding_completo`; condicionalmente `onboarding_pendente` | `app/colaboradores/signals.py:86–102`; `app/colaboradores/services/colaborador.py:55–61`; `app/colaboradores/models.py:419–426` | Batida criada, flag falsa, usuário com login; modelo acrescenta campos se etapas concluídas. | Não. |
| `Justificativa.status,descricao` | `app/chamados/reconciliador.py:522–540` | Justificativa vinculada pendente e chamado efetivamente reconciliado. | Não. |
| **Recálculo FechamentoMensal:** INSERT se ausente; save completo se aberto; demais estados recebem `atualizado_em,horas_trabalhadas,horas_noturnas,horas_extras,horas_extras_50,horas_extras_50_noturna,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_folga_trabalhada,horas_atraso,horas_saida_antecipada,horas_falta,horas_intra_indenizada,turnos_abertos,inconsistencias,semanas_dsr_ok,semanas_dsr_perdido,horas_reflexo_dsr,saldo_banco_horas,minutos_previstos,dias_previstos,dias_incertos,minutos_abonados,minutos_realizados,motivos_espelho,causa_espelho,previsto_em,derivado_em` | `app/ponto/services/fechamento.py:889–892,1121–1174,1791–1808` | Competência da nova batida; universo do recálculo; guarda exportada específica somente 09/2026. | Não. |
| **DiaPago DELETE + INSERT**, versões motor/oráculo: `colaborador,mes,ano,data,tipo,versao,versao_motor,versao_dna,evento_origem,detalhe,furo_apurado,lavrado_em`; rubricas `horas_trabalhadas,horas_noturnas,horas_extras_50,horas_extras_50_noturna,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_folga_trabalhada,horas_atraso,horas_saida_antecipada,horas_falta,horas_intra_indenizada,minutos_abonados,minutos_previstos,minutos_realizados,horas_reflexo_dsr,saldo_banco_horas` | `app/ponto/services/dia_pago.py:34–44,333–358`; `app/ponto/services/fechamento.py:1185–1221,1249–1280`; `app/ponto/models.py:380` | Substitui todas as linhas do colaborador/mês/ano/versão; não faz checagem própria de entrega no DELETE/INSERT. | Não. |
| `ExportacaoDominio` — nenhuma mutação localizada | `app/folha/export.py:580–586`; `app/ponto/services/fechamento.py:530–565,1491–1499` | Somente consultas nos percursos identificados; caminho adicional **[nao verificado]**. | Não se aplica. |
| Diretório, `snapshot_apply_*.jsonl`, `resultado_apply_*.json` | `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:249–259,280–287` | Todo apply cria arquivos; foto por tentativa que chega ao escritor, confirmação por aplicada. | Não são apagados 

[truncado em 20000 de 20344 caracteres]

## 06/11 19:51 · sessao 6398487201a4 · - · - · origem=Ronald (tmux)

3. **Escritor real extra: nao achei.** O censo AST, executado isoladamente em memória, achou somente proxy e `create` em `_nascer`, mais atribuição e desfazimento em `reapontar_resolvedora` (`app/chamados/models.py:2444`, `app/chamados/models.py:2498`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:846`). Não achei escrita de `data_evento` em SQL, admin, form ou serializer [nao verificado quanto a caminhos dinâmicos].

O selo **vê** `qs.update(data_evento=d)` mesmo com queryset em variável. **Não vê** `campos={'data_evento': d}; qs.update(**campos)`, construtor com `**campos`, `setattr` com nome do campo em variável ou SQL. Confirmei esses limites chamando o analisador puro: ele procura constante no próprio argumento, sem seguir variáveis, e não trata `execute` (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:54`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:83`). Varre somente `.py` dentro de `app`, exclui diretórios e ignora `SyntaxError` (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:112`). Existe porta genérica `descarimbar_em_lote(**outros)` que permitiria passar o campo, mas seus chamadores atuais não o passam (`app/chamados/models.py:2714`, `app/chamados/models.py:1508`, `app/chamados/services/disputa_emissao.py:1428`, `app/chamados/services/disputa_emissao.py:1449`).

6. **RED tem mordida estática identificável**, mas execução Django, RED contra o pai e mutação real permanecem [nao verificado]. Há asserts para retorno D do escritor, saída em D+1, contexto do detector, SSE, ausência de recuo sem marcos, diurno e dois turnos independentes (`app/chamados/tests/test_plantio_noturno_mais1.py:78`, `app/chamados/tests/test_plantio_noturno_mais1.py:83`, `app/chamados/tests/test_plantio_noturno_mais1.py:90`, `app/chamados/tests/test_plantio_noturno_mais1.py:102`, `app/chamados/tests/test_plantio_noturno_mais1.py:112`, `app/chamados/tests/test_plantio_noturno_mais1.py:123`, `app/chamados/tests/test_plantio_noturno_mais1.py:134`). O teste positivo do vigia usa o derivador real; o negativo cria timestamp manual e exige apenas `delta >= 1`, sem exigir exatamente +1 (`app/chamados/tests/test_plantio_noturno_mais1.py:153`, `app/chamados/tests/test_plantio_noturno_mais1.py:167`). O selo testa vários formatos, mas não os desvios por variável mencionados acima (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:144`).

9. **Há diferença entre o rótulo temporal e o filtro.** A descrição diz “plantada nos últimos dias”; o filtro usa o instante da batida, `timestamp__gte`, e não sua criação. Portanto, uma batida criada hoje retroativamente para data anterior ao corte fica fora, mesmo se estiver no turno errado (`app/ponto/management/commands/vigia_plantio_overnight.py:66`, `app/ponto/management/commands/vigia_plantio_overnight.py:75`, `app/ponto/models.py:79`). Incidência concreta em produção [nao verificado].

O universo efetivo exige origem exatamente `disputa_s84_retro`, aprovação, ausência de retratação, FK de pergunta, resposta `hora` e `data_evento` preenchida; qualquer batida fora desses filtros não entra (`app/ponto/management/commands/vigia_plantio_overnight.py:16`, `app/ponto/management/commands/vigia_plantio_overnight.py:72`). Plantio pode reaproveitar batida real e lhe vincular pergunta sem mudar a origem; esse reaproveitamento fica fora do contador se a origem continuar `app` (`app/chamados/services/materializacao.py:833`, `app/chamados/services/materializacao.py:922`).

A janela do mapa inclui tanto o dia da pergunta quanto o dia local da batida, com um dia de margem: deslocamento +1 não causa exclusão por distância (`app/ponto/management/commands/vigia_plantio_overnight.py:85`). Batida sem resposta no mapa é silenciosamente pulada (`app/ponto/management/commands/vigia_plantio_overnight.py:88`); em leitura estável, a composição de `dia_das_batidas` fornece ao menos o dia local das batidas vivas da janela (`app/ponto/turnos.py:2904`, `app/ponto/turnos.py:2944`). Não achei caso estável dentro desse universo que produza ausência de chave [nao verificado por execução].

**Extra para item 2:** conversa não deixa discriminador persistido no `create`; só o argumento transitório escolhe se deduplica ao nascer (`app/chamados/models.py:2450`, `app/chamados/models.py:2498`). `do_fato` filtra colaborador/motivo/data sem excluir conversa (`app/chamados/models.py:2396`). O comando de dedup também aceita toda data não nula anterior a `--ate`, agrupa e retrata; seu default antigo limita o alcance, mas `--ate` posterior pode alcançar conversas atuais (`app/chamados/management/commands/dedup_perguntas_fato.py:44`, `app/chamados/management/commands/dedup_perguntas_fato.py:54`, `app/chamados/management/commands/dedup_perguntas_fato.py:87`). `migrar_resolvedoras_vivas` também trata motivo+data iguais como conflito, sem distinguir conversa (`app/chamados/services/fio.py:934`, `app/chamados/services/fio.py:940`). Portanto, existe caminho de leitura do dia da conversa como identidade de fato; ocorrência em produção [nao verificado].

## 06/11 19:50 · sessao 7cab90f6fe79 · - · - · origem=Ronald (tmux)

**Item 4 — consumidores dos contextos**

- `entrada_sem_saida.entrada_id`: **só achei `_data_furo_da_grade`**, que carrega a batida pelo `pk`, filtra por colaborador e consulta `_T1`; se não houver `entrada_id`, tenta o instante exato `entrada_aberta_em`, depois `ultima_batida.id` (`app/chamados/services/disputa_emissao.py:59`, `:68`, `:76`).
- `ultima_batida.id`: além desse caminho, `gerar_perguntas_disputa` consulta a batida e `_T1` como último recurso para datar a pergunta (`app/chamados/services/disputa_emissao.py:379`). `resolver_disputas_de_foto` lê o mesmo `id` como identidade da batida, sem derivar dia (`app/api/foto_recebida.py:67`).
- `saida_sem_entrada.data` tem leitores genéricos: interpolação do texto (`app/chamados/catalogo/query.py:58`); leitura da data para a pergunta (`app/chamados/services/disputa_emissao.py:364`, `:424`); escolha do chamado cujo `dia_do_fato` coincide (`app/chamados/services/disputa_emissao.py:191`, `:231`); fallback de data no materializador quando `p.data_evento` falta (`app/chamados/services/materializacao.py:343`).
- **Não achei leitor que ainda trate especificamente `saida_sem_entrada.data` como dia civil da saída.** O texto canônico é “entrada no dia {data}”, compatível com o dia do turno (`app/chamados/catalogo/perguntas.py:56`).
- Atenção ao caminho efetivo da API: o kernel acrescenta motivo **`orfao_14h`**, mas grava a chave **`saida_sem_entrada`**; portanto a leitura genérica por motivo não pega essa chave naquele ramo (`app/api/views.py:652`, `:655`; gêmeo `app/api/views_core.py:649`, `:652`). Quando precisa resolver o dia, `_data_furo_da_grade` pode chegar a `ultima_batida.id` e `_T1` (`app/chamados/services/disputa_emissao.py:72`).
- Subsiste um leitor vizinho de instante: `dia_do_fato` termina em `_dia_do_contexto_fraco`, que lê `ultima_batida.timestamp` e faz `datetime.fromisoformat(...).date()` **sem conversão de fuso ou `_T1`** (`app/chamados/juizes.py:132`, `:155`, `:162`). Esse caminho não lê o novo `saida_sem_entrada.data`; pode alimentar `_derivar_chamado_destino`, que usa `dia_do_fato` (`app/chamados/services/disputa_emissao.py:231`). O kernel registra esse timestamp com `agora.isoformat()`, enquanto o `id` identifica a batida (`app/api/views.py:773`).

**Item 5 — nascimento depois de registrar**

- **Sim, mudou nos caminhos `dupla` e `ignorada_intervalo`:** o predicado fica pendente em `:654`; `dupla` retorna em `:670`; `batida is None` retorna em `:720`; a disputa só nasce em `:729` (`app/ponto/services/triagem_batida.py`). Pelo diff fornecido, antes ela nascia imediatamente após o predicado, antes desses retornos; ocorrência em produção **[nao verificado]**.
- Também qualquer exceção entre o predicado e o novo nascimento impede a disputa; por exemplo a confirmação decodifica base64 sem `try` local (`app/ponto/services/triagem_batida.py:659`, `:499`, `:515`) e `registrar_batida` está antes do nascimento (`:701`, `:729`). Ocorrência real **[nao verificado]**.
- A declaração “batida não gravada (dupla, oposta) não abre disputa” não cobre todos os retornos da porta: `registrar_batida` pode devolver batida existente por mesmo tipo e por mesma chave, sem criar outra (`app/ponto/registro_batida.py:113`, `:120`). A triagem ignora o segundo retorno e continua com qualquer objeto devolvido (`app/ponto/services/triagem_batida.py:701`, `:714`, `:726`).
- **Caminho de exceção concreto no novo código:** a busca de mesma chave inclui retratada (`app/ponto/registro_batida.py:116`, `:120`); `_T1` não dá dia à retratada e o wrapper pode retornar `None` (`app/ponto/turnos.py:2917`, `:2925`); a triagem chama `.isoformat()` sem guarda (`app/ponto/services/triagem_batida.py:734`). Exige `_sse_pendente=True` e devolução dessa chave retratada; ocorrência em produção **[nao verificado]**.
- A nova batida dispara cartório e reconciliador **antes** de a disputa nascer: receivers em `app/ponto/signals.py:131` e `:206`; chamada da gravação em `app/ponto/services/triagem_batida.py:701`, disputa em `:729`. `reconciliar_chamados_colab` passa pelas perguntas/chamados existentes (`app/chamados/reconciliador.py:1418`, `:1427`), logo o fio recém-criado não participou daquela passagem.
- **Não achei outro sítio que exija explicitamente a disputa prévia para `dupla`/`ignorada`.** O chamador da view só devolve o `JsonResponse` recebido (`app/ponto/views.py:350`, `:353`). O efeito de perder a passagem reativa inicial deve ser distinguido de prova de fio incorreto: esse último efeito em produção é **[nao verificado]**.

## 06/11 19:50 · sessao c5729b08d142 · - · - · origem=Ronald (tmux)

DELETE de Batida em `app/chamados/services/regularizacao_ext.py:136` alcança:

| Efeito | Modelo.campo | Referência |
|---|---|---|
| SET_NULL | `Justificativa.batida_entrada`, `.batida_gerada` | `app/ponto/models.py:139`, `:167` |
| SET_NULL | `AlertaGeofence.batida` | `app/ponto/models.py:238` |
| SET_NULL | `RegularizacaoItemBatida.batida_entrada`, `.batida_saida`, `.batida_alvo` | `app/chamados/models.py:3267`, `:3271`, `:3280` |
| CASCADE DELETE | `PingGeo` — nome correto do modelo | `app/ponto/models.py:995`, `:1000` |
| CASCADE DELETE | `FotoBatida` | `app/ponto/models.py:1026`, `:1027` |
| CASCADE DELETE | `TurnoMaterializado`, pelas FKs `.entrada` ou `.saida` | `app/ponto/models.py:1206`, `:1208` |

Após o DELETE, receivers ainda recomputam núcleo e cartório (`app/ponto/signals.py:48-50`, `:138-140`). Outras FKs para Batida não apareceram na varredura do código de produção `[nao verificado quanto ao schema efetivo do banco]`. Exclusão do arquivo físico de `FotoBatida.foto` não foi demonstrada `[nao verificado]`.

## 06/11 19:49 · sessao 03109f34636c · - · - · origem=Ronald (tmux)

**O `Ausencia.save()` aciona uma árvore de escritas condicionais; a conferência do comando só limita os campos da própria Ausencia, não os efeitos laterais.** O comando faz `_estado(a)` antes/depois e compara somente essa instância (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:166`, `:173`, `:175`). Ocorrência concreta dos efeitos abaixo nas 30 linhas: `[nao verificado]`.

| Escrita condicional | Arquivo:linha | Guarda / alcance |
|---|---|---|
| `CelulaDia.veredito, veredito_via, veredito_em, protestos, ata, status_dia, status_rotulo` | `app/ponto/portas/celula.py:961`, `:995`, `:1027`, `:1033` | Lavra igual não escreve (`:1013`–`:1016`); decisão admin preserva o veredito, podendo mudar ata/status (`:997`–`:1004`). |
| `CelulaDia.julgada_em, impressao, ausencia, tem_entrada, tem_saida, insumos_em` | `app/ponto/portas/celula.py:1060`–`:1067` | Mesmo insumo sem lavra retorna sem save (`:1055`–`:1059`). |
| `CelulaDia.ata['precedencia']` | `app/ponto/portas/celula.py:1075`–`:1082` | Precedência igual não escreve (`:1076`). |
| INSERT `EventoCelula(celula_id,colaborador_id,data)`; defaults `criado_em,processado_em=NULL,tentativas=0,erro=''` | `app/ponto/portas/celula.py:890`–`:891`; `app/escala/models.py:1857`–`:1863` | Save de ata/veredito emite no mesmo atomic (`app/ponto/portas/celula.py:894`–`:902`). |
| INSERT `LogAuditoria(usuario,acao,modelo,objeto_id,descricao,valor_antes,valor_depois,ip,ator)` | `app/core/observ.py:118`–`:137` | Lavra deixa trilha (`app/ponto/portas/celula.py:1021`, `:1036`). Best effort no helper (`app/core/observ.py:105`). |
| INSERT `ChamadoColaborador` de batida ausente, com `colaborador,modulo_origem,status_local,celula,assunto,descricao,area,categoria,subcategoria,urgencia,prazo_sla,contexto_json` | `app/ponto/management/commands/detectar_ausencias.py:693`–`:713`; `app/chamados/models.py:899`, `:919`, `:932` | Emissão do cartório passa pelo detector (`app/ponto/services/cartorio.py:1740`, `:1778`); várias guardas do funil (`app/ponto/management/commands/detectar_ausencias.py:599`, `:604`, `:616`, `:628`, `:631`, `:659`); dedup/reabertura (`app/chamados/models.py:867`–`:885`). |
| `ChamadoColaborador.contexto_json`, ocorrência/nota ao espelhar marcos | `app/ponto/management/commands/detectar_ausencias.py:558`–`:567` | Mesma lista retorna sem escrita (`:556`). |
| INSERT chamado `cluster_batida_espuria`, defaults `categoria,area,assunto,titulo,descricao,contexto_json` mais campos do gate | `app/ponto/services/cluster_espurio.py:130`–`:147` | Só colab ativo com escala ativa sem fim (`:117`–`:120`); abre na janela própria de 14 dias (`:12`, `:44`–`:46`); fechamento consulta chamados de qualquer idade (`:53`–`:77`). Cartório chama o cluster quando houve julgamento (`app/ponto/services/cartorio.py:990`–`:994`). |
| INSERT `DisputaSupervisao(colaborador,motivos,contexto,chamado)` | `app/chamados/services/disputa_emissao.py:523`–`:528`, `:962`–`:967` | Reutiliza disputa aberta (`:467`–`:469`, `:935`–`:937`). |
| `DisputaSupervisao.motivos,contexto` | `app/chamados/services/disputa_emissao.py:476`–`:501`; motivos também `:923`, `:971`, `:999`, `:1090` | O merge de contexto pode incrementar `qtd` e `ultima_em` (`:484`–`:495`). |
| `DisputaSupervisao.fechada_em,fechada_por,resolucao,motivo_fechamento` para reabrir | `app/chamados/models.py:2087`–`:2091` | Só disputa fechada (`:2081`). Ressuscitação pode chamar essa porta (`:1531`–`:1532`). |
| Mesmos quatro campos para fechar disputa | `app/chamados/models.py:1865` | Fábrica de órfãos pode fechar (`app/chamados/services/disputa_emissao.py:1112`–`:1121`); resolvedor pode fechar (`:1717`–`:1718`); beco pode fechar (`app/chamados/services/becos.py:51`–`:52`). |
| INSERT `PerguntaDisputa(disputa,motivo,data_evento,chamado_destino,celula,pergunta,tipo_resposta,opcoes[,chamados_relacionados_ids])` | `app/chamados/models.py:2476`–`:2477`; `app/chamados/services/disputa_emissao.py:389`–`:397`, `:767`–`:778` | Identidade global por colab/motivo/data encontra existente (`app/chamados/models.py:2428`–`:2432`); fábrica deduplica (`app/chamados/services/disputa_emissao.py:299`, `:370`, `:386`). |
| `PerguntaDisputa.celula,chamado_destino,veredito_lavrado,julgado_em,janela_expira_em` | `app/chamados/models.py:2433`–`:2474` | Reaponta existente; valida resolvedora e preserva fio (`:2440`, `:2448`). |
| `PerguntaDisputa.materializada_em,materializada_por,via_resolucao` para descarimbar | `app/chamados/models.py:2646`–`:2652` | `desfazer_carimbo_sem_lastro` relê travado e só escreve pergunta viva (`app/chamados/services/relavra_via.py:217`–`:231`). Fábrica do julgamento usa `reabrir_fechada=False` (`app/chamados/services/fabrica_da_celula.py:164`–`:166`). |
| Descarimbo em lote + `PerguntaDisputa.validada_em,validada_por,respondida_em=NULL` | `app/chamados/models.py:1504`, `:2699`–`:2700` | Universo calculado pela ressuscitação; sem ids retorna zero (`:1492`–`:1498`). |
| Carimbo `PerguntaDisputa.materializada_em,materializada_por,via_resolucao`; condicionalmente `respondida_em,validada_em,validada_por,veredito_lavrado,julgado_em,janela_expira_em` | `app/chamados/models.py:2585`–`:2609` | C6 pode recusar (`:2596`–`:2602`); resolvedor usa resposta validar/lavra (`app/chamados/services/disputa_emissao.py:1617`–`:1618`); S144 só carimba (`app/chamados/services/materializacao.py:1402`–`:1407`). |
| Mais INSERTs `EventoCelula` por mudança de PerguntaDisputa | `app/chamados/signals.py:214`–`:221`; `app/chamados/models.py:2671`–`:2688` | Nascimento não emite; campos que acordam são filtrados (`app/chamados/signals.py:216`–`:219`). Sem célula não emite (`app/chamados/models.py:2685`). |
| INSERT `Pauta` do limbo exportado, pela porta `escrever` | `app/chamados/services/fabrica_da_celula.py:225`–`:241` | Somente limbo exportado (`:167`–`:173`); dedup de motivos (`:234`–`:235`). Colunas internas de Pauta: `[nao verificado]`. |
| DELETE `PushSubscription` | `app/chamados/push_utils.py:49`–`:51` | Resposta push 404/410. Detector manda push supervisão (`app/ponto/management/commands/detectar_ausencias.py:774`–`:783`). |
| `Colaborador.fcm_token='',fcm_removido_em,fcm_removido_motivo` + auditoria | `app/colaboradores/fcm.py:60`–`:67` | FCM recusou token (`app/chamados/fcm_utils.py:89`–`:94`, `:39`–`:42`); alcançável pelo fio do detector com `notificar=True` (`app/chamados/services/disputa_emissao.py:876`, `:1123`–`:1124`, `:1547`–`:1548`). |
| INSERT/UPDATE/DELETE `Batida` pela retomada de regularização | `app/chamados/services/regularizacao_ext.py:77`–`:82`, `:136`, `:153`–`:155`; INSERT real `app/ponto/registro_batida.py:157`–`:160` | Fechamento de disputa agenda retomada (`app/chamados/signals.py:97`–`:121`). Filtro retomada só `status='confirmada',materializada_em=NULL` e colab (`app/chamados/services/regularizacao_ext.py:218`–`:224`), sem data. |
| `RegularizacaoItemBatida.batida_alvo,aplicada_em,batida_entrada,batida_saida` | `app/chamados/services/regularizacao_ext.py:137`–`:139`, `:162`–`:163`, `:171`–`:188` | Guardas por item já aplicado ou FK já preenchida (`:125`, `:171`, `:175`, `:180`, `:184`). |
| `RegularizacaoExterna.status,materializada_em,materializada_por` | `app/chamados/services/regularizacao_ext.py:197`–`:200` | Somente confirmada e sem disputa aberta (`:51`–`:65`). |
| DELETE e INSERT `TurnoMaterializado(colaborador,data_turno,entrada,saida,aberto,cross_meianoite,dt_fim_previsto,atualizado_em)` | `app/ponto/nucleo.py:48`–`:59`, DELETE SQL `:80`–`:82`; timestamp `app/ponto/models.py:1213` | Batida save/delete recomputa d−2…d+1 (`app/ponto/signals.py:19`–`:36`, `:48`–`:50`). |
| INSERT `EventoCelula` por turno que fechou | `app/ponto/nucleo.py:64`–`:66`, `:86`–`:93` | Só turno antes aberto que deixou de existir e célula existente. |

A entrada principal é `Ausencia.post_save → _dias_ausencia → _julgar_dias → cartorio.julgar_celula` (`app/ponto/signals.py:143`–`:147`, `:115`–`:128`, `:85`–`:93`). O juiz barra futuro e célula inexistente, sem verificar exportação nesse ponto (`app/ponto/services/cartorio.py:1974`–`:1978`). Impressão igual pode pular o julgamento (`:1199`–`:1204`, `:1348`–`:1367`).

A fábrica do julgamento silencia **seu próprio** push de disputa (`app/chamados/services/fabrica_da_celula.py:174`–`:176`), mas o detector faz push supervisão e monta fio de outros chamados órfãos do colaborador (`app/ponto/management/commands/detectar_ausencias.py:774`–`:801`). Portanto não existe garantia geral de ausência de push nesse caminho (`mesmas referências`).

A cauda de regularização pode alcançar datas fora da lista: a retomada não filtra competência/data (`app/chamados/services/regularizacao_ext.py:218`–`:224`). A criação chama `registrar_batida` com `autoridade_humana=False`, sem `fato_de_chao` (`:77`–`:82`), que deriva `fato_de_chao=True` e evita a guarda de competência (`app/ponto/registro_batida.py:130`–`:137`). Essa criação agenda recálculo monetário (`:160`, `:175`–`:178`).

Não identifiquei criação/edição de outra Ausencia nos caminhos lidos de cartório, fábrica, beco e regularização; a materialização S144 só carimba PerguntaDisputa (`app/chamados/services/materializacao.py:1393`–`:1407`). Prova universal de inexistência de outro caminho transitivo: `[nao verificado]`.

O comando não consome o outbox diretamente: o chamador produtivo encontrado está no comando separado `app/chamados/management/commands/consumir_eventos_celula.py:31`; o consumidor atualiza tentativas/erro/processado_em e faz retenção (`app/chamados/reconciliador.py:1380`, `:1386`, `:1393`). Esses efeitos futuros devem ser distinguidos das escritas síncronas e `on_commit` acima (`mesmas referências`).

O snapshot do comando não captura nenhuma dessas entidades laterais: `_foto` só fotografa Ausencia (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:120`–`:123`), e a reversão só monta plano para seus pks (`:229`–`:242`). Portanto “reversível pelo snapshot” = **não**, para todas as entidades laterais desta tabela (`mesmas referências`); push enviado tampouco é recolhido pelo rollback (`:20`–`:21`).

## 06/11 19:49 · sessao c5729b08d142 · - · - · origem=Ronald (tmux)

Ramo transitivo `regularizacao_ext.py:77 → registrar_batida`:

| Escrita/efeito | Evidência | Guarda/limite |
|---|---|---|
| `TurnoMaterializado`: apaga recorte e recria `colaborador,data_turno,entrada,saida,aberto,cross_meianoite,dt_fim_previsto,atualizado_em` | `app/ponto/signals.py:19-36`; `app/ponto/nucleo.py:48-59`, `:80-83`; automático `app/ponto/models.py:1213` | Recorte D−2 a D+1; **sem guarda de exportação** nesse escritor (`signals.py:26-28`, `nucleo.py:57-66`). |
| `EventoCelula`: INSERT `celula_id,colaborador_id,data` quando turno antes aberto deixa de existir | `app/ponto/nucleo.py:64-66`, `:86-93`; `app/ponto/portas/celula.py:890-891` | Só dias com CelulaDia existente (`nucleo.py:92`); mesma transação do DELETE/rebuild (`:57-66`). |
| `Colaborador.onboarding_completo=True` | `app/colaboradores/signals.py:86-102`; `app/colaboradores/services/colaborador.py:55-61` | Só Batida criada, flag ainda falsa e usuário com last_login (`signals.py:94-100`). |
| Por save do Colaborador isento: `ChamadoColaborador.silenciado_ate,silenciado_por,silenciado_motivo,atualizado_em`; `LogAuditoria` | `app/chamados/signals.py:232-243`; `app/chamados/services/silencio_isentos.py:20-31`, `:41-47`, `:52-58`; `app/chamados/services/arquivo.py:165-172`, `:130-138` | Isento; chamados vivos sem silêncio; roda `on_commit`. Sem recorte por mês. **Não escreve `resgatado_em` nesse ato** (`arquivo.py:170-171`). |
| Adesão: `ChamadoColaborador.status_local,resolvido_em,via_encerramento,atualizado_em`, eventualmente `urgencia,urgencia_original`; `FollowupChamado` e trilha do gate | `app/ponto/signals.py:233-234`; `app/colaboradores/services/pos_vinculo.py:36-45`; `app/chamados/models.py:607-620`, `:195-247`; nota `app/chamados/services/nota.py:88-103` | Chamado vivo `sem_previsao_escala/nunca_bateu`; Batida apurável no período do próprio chamado. Não limita a comp11 (`pos_vinculo.py:36-40`). |
| Reconciliação de chamados de todo colaborador → possíveis `Justificativa.status='rejeitada'` e `descricao` com sufixo | `app/ponto/signals.py:206-215`; `app/chamados/reconciliador.py:1417-1426`, `:1438-1449`, `:529-535` | Justificativa vinculada pendente e chamado reconciliado; save da Justificativa volta ao cartório (`app/ponto/signals.py:161-165`). |
| Dois `pg_notify`: canal do posto + agregado | `app/ponto/signals.py:39-45`; `app/ponto/realtime.py:25-43` | Só criada e colaborador com posto; efeito sem linha de modelo persistida. |

Conflito não produz modelo próprio nesse caminho: `registrar_batida` com `autoridade_humana=False` devolve `(None, conflito)` antes do INSERT (`app/ponto/registro_batida.py:145-151`); regularização conta pulada e acrescenta erro (`app/chamados/services/regularizacao_ext.py:83-88`). A existência de algum modelo chamado `BatidaAudit`/`ConflitoBatida` não foi demonstrada `[nao verificado]`; o caminho concreto usa string de conflito nessas linhas.

A criação nessa regularização **omite `fato_de_chao`** (`regularizacao_ext.py:77-82`); com `autoridade_humana=False`, ele deriva `True` e pula `competencia_fechada` (`registro_batida.py:130-137`). Logo o chokepoint não acrescenta uma cerca de competência a esse ramo.

Não localizei escrita persistente direta de cache ou geofence dentro de `registrar_batida`/núcleo. Os receivers explícitos de Batida examinados são núcleo, realtime, cartório, reconciliador e onboarding (`app/ponto/signals.py:34`, `:39`, `:131`, `:206`; `app/colaboradores/signals.py:86`). Efeitos adicionais via grafo completo da reconciliação/cartório permanecem com a auditoria dos demais agentes `[nao verificado]`.

## 06/11 19:48 · sessao b8ba3deaea45 · - · - · origem=Ronald (tmux)

**O ramo transitivo existe e pode alcançar competência exportada de outro mês.** O consumidor do outbox pode fechar DisputaSupervisao (`app/chamados/reconciliador.py:1364`, `:1159-1161`); seu signal agenda `retomar_regularizacoes_confirmadas` após commit (`app/chamados/signals.py:106-121`), que busca **todas** as regularizações confirmadas e não materializadas do colaborador, sem recorte de data (`app/chamados/services/regularizacao_ext.py:218-224`).

O plantio chama `registrar_batida` com `autoridade_humana=False`, **omitindo `fato_de_chao=False`** (`app/chamados/services/regularizacao_ext.py:77-82`). A porta deriva `fato_de_chao=True` e pula a guarda de competência (`app/ponto/registro_batida.py:130-137`). Batida criada agenda recálculo (`:157-160`, `:175-178`) da competência do timestamp, após commit (`app/ponto/services/fechamento.py:1791-1808`). A presença dessas regularizações na frota real está `[nao verificado]`.

**Escritas resultantes em FechamentoMensal:**

- `get_or_create` pode criar linha com colaborador, mês, ano e status aberto (`app/ponto/services/fechamento.py:889-892`).
- Linha aberta recebe `derivado_em` e `save()` completo (`:1121-1128`).
- Linha aprovada/exportada recebe `update()` nos campos: `atualizado_em`, `horas_trabalhadas`, `horas_noturnas`, `horas_extras`, `horas_extras_50`, `horas_extras_50_noturna`, `horas_extras_100`, `horas_extras_100_feriado`, `horas_extras_100_noturna`, `horas_folga_trabalhada`, `horas_atraso`, `horas_saida_antecipada`, `horas_falta`, `horas_intra_indenizada`, `turnos_abertos`, `inconsistencias`, `semanas_dsr_ok`, `semanas_dsr_perdido`, `horas_reflexo_dsr`, `saldo_banco_horas`, `minutos_previstos`, `dias_previstos`, `dias_incertos`, `minutos_abonados`, `minutos_realizados`, `motivos_espelho`, `causa_espelho`, `previsto_em`, `derivado_em` (`app/ponto/services/fechamento.py:1144-1174`).
- **Não existe guarda geral de exportada:** há warning e continuação (`:530-538`), além da guarda temporária somente **09/2026** (`:36-43`, `:551-565`). A proteção de `FechamentoMensal.save` para aprovado não cobre o `queryset.update()` desse ramo (`app/ponto/models.py:496-503`; `fechamento.py:1144`).

**Escritas resultantes em DiaPago:**

- O recálculo chama lavratura `versao='motor'` (`app/ponto/services/fechamento.py:1185-1221`) e também `versao='oraculo'` para competências ≥10/2026 (`:1249-1280`).
- Cada lavratura **apaga todas as linhas do colaborador/mês/ano/versão e insere novas** (`app/ponto/services/dia_pago.py:357-358`).
- Campos de identidade/proveniência: `colaborador`, `mes`, `ano`, `data`, `tipo`, `versao`, `versao_motor`, `versao_dna`, `evento_origem`, `detalhe`, `furo_apurado` (`:333-356`), mais PK nova e `lavrado_em` automático (`app/ponto/models.py:380`).
- Campos monetários: `horas_trabalhadas`, `horas_noturnas`, `horas_extras_50`, `horas_extras_50_noturna`, `horas_extras_100`, `horas_extras_100_feriado`, `horas_extras_100_noturna`, `horas_folga_trabalhada`, `horas_atraso`, `horas_saida_antecipada`, `horas_falta`, `horas_intra_indenizada`, `minutos_abonados`, `minutos_previstos`, `minutos_realizados`, `horas_reflexo_dsr`, `saldo_banco_horas` (`app/ponto/services/dia_pago.py:34-44`).
- A proteção de exportação é somente a guarda anterior do recálculo; `lavrar` não faz consulta de entrega no ato de apagar/inserir (`:333-358`; `fechamento.py:551-565`).

**ExportacaoDominio:** não localizei escrita nesse ramo; a consulta serve para warning/guarda09 (`app/ponto/services/fechamento.py:530-538`, `:1491-1499`). O escritor de invalidação existe em `app/folha/models.py:143-170`, mas não é chamado pelo recálculo localizado (`app/ponto/services/fechamento.py:476-538`, `:1108-1295`).

**Reversão:** o snapshot contém identidade/minutos/carimbo apenas da Ausencia (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:120-123`); não contém estados anteriores de FechamentoMensal, DiaPago ou Batida. Estes efeitos após commit não são revertidos pelo rollback da tentativa original nem pelo `--reverter` de minutos (`comando:137`, `:165-190`; `chamados/signals.py:121`; `fechamento.py:1808`).

_2501 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
