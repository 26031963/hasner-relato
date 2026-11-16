# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 08:50** · publicado em **16/11/2026 08:49** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 08:50 · sessao de75e71aae41 · - · - · origem=Ronald (tmux)

1. Por análise estática, o delta distingue as três versões citadas: sem atomic externo e ordem save→trilha, a FK nova permanece visível e falha a asserção de posto antigo; com trilha→save sem atomic comum, o INSERT real permanece visível e falha a contagem; com o atomic atual, ambas as escritas são desfeitas. A porta tem atomic próprio, e o substituto levanta somente depois de retornar do create real. (`app/colaboradores/views.py:333-337`; `app/colaboradores/services/colaborador.py:152-161`; `app/colaboradores/services/historico_vinculo.py:13-17`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:244-255`). Execução independente dessas variantes: [nao verificado].

2. **Mutante plausível sobrevivente:** mover a trilha para antes da porta, envolver **somente a trilha** em atomic e deixar `salvar_inteiro` depois, fora dele. O substituto insere e levanta dentro desse atomic; ele desfaz o histórico; a porta jamais é alcançada; posto antigo, histórico inalterado, HTML antigo e toast esperado satisfazem o teste. Portanto, o delta demonstra os efeitos exigidos nesse erro, mas não exige que a porta tenha sido alcançada nem prova estruturalmente atomic comum. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:244-260`; `app/colaboradores/views.py:334-336`). Execução do mutante: [nao verificado]. Remover a chamada à porta também sobrevive a esse teste isolado pelo mesmo motivo. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:249-260`).

3. **Não há asserção do teste executada dentro do substituto.** Ele só chama a função real, registra retorno e levanta `RuntimeError`; todas as asserções, inclusive JSON, executam depois do POST. Logo, o `except Exception` da view não engole essas asserções. Se a função real levantar antes de retornar, `append` não termina e `len(escritas)==1` falha; se retornar `None`, falha `assertIsNotNone`. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:244-260`; `app/colaboradores/views.py:337-359`).

4. A leitura na mesma transação do `TestCase` não mascara os dois mutantes simples: as queries posteriores consultam o banco novamente, e escritas próprias não desfeitas continuam visíveis nessa conexão. O encerramento do teste ocorre depois dessas asserções. Isso verifica rollback local, sem demonstrar commit real, outra conexão, concorrência ou callbacks de commit. (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:43`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:248-255`). Comportamento em execução independente/PROD: [nao verificado].

5. **A justificativa de um descarte da entrega é incorreta:** `test_MORDE_desligado_no_meio_linha_mostra_o_posto_gravado` também não distingue substituir releitura por `colaborador.posto = _posto_antes`. Seu gancho muda situação/data/motivo, sem mudar posto; suas asserções exigem somente FK antiga, nomes antigo/novo no HTML e histórico vazio. A restauração em memória satisfaz isso. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:40-43`, `:216-228`). O template não mostra situação/data/motivo de desligamento, portanto não expõe o objeto desatualizado nesse cenário. (`app/templates/colaboradores/partials/vinculo_row.html:1-48`). Isso pede correção da alegação de cobertura da entrega; a implementação atual efetivamente relê o banco. (`app/colaboradores/views.py:343`). Execução do mutante: [nao verificado].

## 16/11 08:49 · sessao b1facf76be2c · - · - · origem=Ronald (tmux)

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `HX-Trigger` | Recusa da view serializa `{'showToast': {'msg': f'Erro: {e}', 'tipo': 'erro'}}`. `app/colaboradores/views.py:337`, `:358`. | Teste faz `json.loads` e compara o objeto integral. `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:259-260`. Produção lê cabeçalho/JSON e passa mensagem/tipo ao toast; mensagem vai para `textContent`. `app/templates/core/_dialogo.html:79-85`, `:37`. |
| `HistoricoVinculo` | View fornece os campos do ato; serviço faz `objects.create`. `app/colaboradores/views.py:335-336`; `app/colaboradores/services/historico_vinculo.py:13-17`. Substituto chama a função real e depois levanta. `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:241-246`. | Teste conta antes/depois e exige um retorno não nulo do escritor. `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:240`, `:249-255`. Painel consulta linhas/contagem e exibe os campos. `app/colaboradores/views.py:306-307`; `app/templates/colaboradores/partials/painel_vinculo.html:60-64`. Cadastro e fio também exibem histórico. `app/templates/colaboradores/editar.html:179`, `:187`; `app/inteligencia/resumo.py:51-52`; `app/templates/chamados/fio_colaborador.html:89-91`. |
| `Colaborador.posto_id` | Fixture grava o posto antigo; POST fornece o novo. `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:54-55`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:201`, `:207-209`. View atribui a relação e porta executa `save()`. `app/colaboradores/views.py:322-327`; `app/colaboradores/services/colaborador.py:158-160`; `app/colaboradores/models.py:439`. | Teste consulta diretamente o FK no banco. `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:252-253`. View relê banco e fornece relação ao template; HTML mostra nome/praça, conferidos pelo teste. `app/colaboradores/views.py:343`, `:350`; `app/templates/colaboradores/partials/vinculo_row.html:16-21`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:256-258`. |

Há outro escritor de histórico, `registrar_evento_de_vinculo`, também com `objects.create`; portanto “escritor único = registrar_mudanca” só é adequado se restrito ao caminho examinado. `app/colaboradores/services/historico_vinculo.py:20-27`.

O formulário HTMX recebe a resposta substituindo a linha inteira. `app/templates/colaboradores/partials/painel_vinculo.html:112-115`.

## 16/11 08:49 · sessao fa11404d8245 · - · - · origem=Ronald (tmux)

A triagem tem uma justificativa incorreta: o teste c6 **também não distingue** a releitura do banco de `colaborador.posto = _posto_antes`. Seu gancho muda somente `situacao`, `data_demissao` e `motivo_desligamento`; o posto continua `self.po`, igual ao capturado anteriormente. Portanto restaurar o anterior em memória satisfaz suas asserções de posto, HTML e contagem. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:40-43`, `:216-228`; `app/colaboradores/views.py:326`, `:343`).

Os demais descartes correspondem a limitações presentes no código, fora da falha excepcional que o atomic resolve:

- Igualdade textual descarta histórico; `Posto.__str__` usa nome e empresa. (`app/colaboradores/services/historico_vinculo.py:10-11`; `app/colaboradores/models.py:276-277`).
- Anterior é capturado antes da trava, portanto pode ficar obsoleto sob concorrência, por inferência estática. (`app/colaboradores/views.py:319`, `:326`, `:333-335`; `app/colaboradores/services/colaborador.py:89-91`).
- `sem_posto` aparece nos dois ramos e no painel, com pendência explícita no registro. (`app/colaboradores/views.py:346`, `:365`; `app/templates/colaboradores/partials/painel_vinculo.html:34`; `app/core/juizes.py:931-932`).
- `HistoricoVinculo.data` recebe dia da execução aqui, início de escala no vínculo e demissão futura no agendamento. (`app/colaboradores/views.py:335`; `app/colaboradores/services/vinculo.py:354-357`; `app/colaboradores/services/desligamento.py:113-116`).
- Matrícula ausente provoca trava adicional sobre o colaborador da maior matrícula. (`app/colaboradores/models.py:427-438`).

Para o contrato restrito **“exceção na trilha desfaz posto e trilha”**, nenhum desses itens demonstra bloqueio funcional: as duas portas estão no atomic comum e o `except` está fora. (`app/colaboradores/views.py:333-343`). A afirmação de cobertura da releitura exige correção na entrega; o teste c6 citado não fornece essa prova. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:216-228`).

Não localizei na cópia o conteúdo literal de `respostas_da_manha_1611.txt` nem o laudo opus c6; há referências em comentários, mas a conformidade com seu teor integral fica **[nao verificado]**. (`app/colaboradores/views.py:331`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:231`). Datas/execuções de logs anexados e condições reais de PROD: **[nao verificado]**.

## 16/11 08:48 · sessao 596161895ebe · BUG-PAR-ENSAIO-MERGE · leitura · origem=Code

**Pelo anexo, o merge é textualmente limpo; isso não demonstra compatibilidade de sentido.** A saída contém somente a árvore `135d7d842d90a3ce2a8e0ed907382919c4cafea8`, sem arquivo em conflito; o pedido informa `rc 0`. (`ensaio_mergetree_full.txt:1`; execução independente do merge: [não verificado].)

A comparação abaixo usa a árvore disponível e os hunks B1 fornecidos. O histórico e o diff próprio completo da raia não puderam ser reconstruídos nesta cópia: [não verificado]. Resultado de suíte pós-merge e estado de produção: [não verificado]. [nao verificado]

**1) Os três arquivos tocados pelos dois lados têm alterações separadas.**

| Arquivo | Trecho visível na raia | Hunk B1 fornecido | Relação |
|---|---|---|---|
| `app/chamados/services/fio.py` | O tratamento BUG-PAR transforma falha da testemunha em `TESTEMUNHA_INDISPONIVEL`, retirando a autorização para validar. (`app/chamados/services/fio.py:372`, `app/chamados/services/fio.py:376`.) | `@@ -1592,17 +1592,25 @@`: substitui `recusa_respondida_sem_texto` por `recusa_par_ilegal`, acrescentando a recusa de texto sem instante. A definição antiga está na raia em `app/chamados/services/fio.py:1598`. | **Não se sobrepõem; estão distantes.** O bloco de apresentação e a porta de integridade são funções distintas. (`app/chamados/services/fio.py:372`; `app/chamados/services/fio.py:1598`; hunk B1 citado.) |
| `app/chamados/tests/test_s139.py` | O teste espera ausência de batida e erro `DECLARADO-X-GRAVADO`; a chamada ao materializador está em `:48`, os asserts em `:53` e `:55`. (`app/chamados/tests/test_s139.py:26`, `app/chamados/tests/test_s139.py:48`, `app/chamados/tests/test_s139.py:53`.) | `@@ -42,7 +42,8 @@`: acrescenta `respondida_em=agora` ao `create` que já contém `'02:00'`. A fixture atual está em `app/chamados/tests/test_s139.py:43`. | **Próximos, sem sobreposição das alterações:** B1 corrige a fixture; a raia conserva a expectativa de espera depois da materialização. (`app/chamados/tests/test_s139.py:45`, `app/chamados/tests/test_s139.py:53`; hunk B1 citado.) |
| `app/core/tests/test_medida_turno_do_par.py` | O registro de materialização aponta `_plantio_preenche_furo_na_data` para `[417]`. (`app/core/tests/test_medida_turno_do_par.py:271`.) | `@@ -255,10 +255,10 @@`: atualiza quatro referências de `juizes.py`, de `53/169/274/203` para `54/170/275/204`. Os valores antigos estão em `app/core/tests/test_medida_turno_do_par.py:258`. | **Próximos, sem sobreposição:** são entradas diferentes do mesmo dicionário. Devem permanecer tanto os quatro valores B1 quanto a entrada da materialização correspondente à árvore final. (`app/core/tests/test_medida_turno_do_par.py:258`, `app/core/tests/test_medida_turno_do_par.py:271`; hunk B1 citado.) |

Os trechos acima são os lados visíveis na cópia; os hunks próprios originais da raia, contra seu ancestral, são [não verificado].

**2) Os cruzamentos de sentido exigem distinguir nomeação do juiz, integridade da escrita e destino da tarefa.**

Na B1, “respondida” significa **instante presente E texto não vazio após trim**. A recusa de gravação passa a abranger as duas metades ilegais; a migração dos leitores das classes B/C/D fica expressamente para B2. (B1 `app/chamados/juizes.py`, hunk `@@ -1531,6 +1534,24 @@`.)

| Ponto da raia | Suposição atual e mudança B1 | Coerência necessária na volta |
|---|---|---|
| **C6 e par projetado** | A projeção corta conjuntamente instante e texto posteriores a `agora`; o C6 testa os valores projetados com `I E T`. B1 nomeia essa mesma conjunção por `pergunta_respondida(SimpleNamespace(...))`. (`app/chamados/juizes.py:1388`, `app/chamados/juizes.py:1393`, `app/chamados/juizes.py:1420`; B1 hunk `@@ -1417,7 +1418,9 @@`.) | Preservar a chamada B1 **sobre o par projetado**. Chamar diretamente `pergunta_respondida(p)` nesse degrau perderia o corte temporal. A tabela-verdade desse degrau não precisa ser invertida. (`app/chamados/juizes.py:1393`, `app/chamados/juizes.py:1403`; hunk B1 citado.) |
| **Irmã do intervalo** | A irmã “respondida” é selecionada por instante no SQL e texto no Python, além de materialização nula, identidade e exclusão de pergunta em voo. (`app/chamados/services/materializacao.py:143`, `app/chamados/services/materializacao.py:146`.) B1 passa a nomear essa conjunção pelo juiz. (B1 `app/chamados/juizes.py`, hunk `@@ -1531,6 +1534,24 @@`.) | Para acompanhar a autoridade B1, nomear `pergunta_respondida` ou sua face SQL, conservando identidade, materialização, ambiguidade e voo. A troca equivalente não implica novo conjunto de irmãs. (`app/chamados/services/materializacao.py:141`, `app/chamados/services/materializacao.py:145`, `app/chamados/services/materializacao.py:147`; hunk B1 citado.) |
| **BUG-PAR-103 e situação da irmã** | Ambos repetem `I E T`; o primeiro também exige via, materialização anterior ao corte e lâmpada não comprovadamente acesa. (`app/chamados/services/materializacao.py:275`, `app/chamados/services/materializacao.py:276`, `app/chamados/services/materializacao.py:307`.) | Nomear o juiz nesses dois leitores, preservando os demais filtros e rótulos. Uma pergunta ressuscitada pela B1, com par vazio, deve cair em “não respondida”. (`app/chamados/services/materializacao.py:278`, `app/chamados/services/materializacao.py:305`; B1 `app/chamados/models.py`, hunk `@@ -1609,10 +1611,16 @@`.) |
| **Contador de recusadas** | Já usa `q_respondida_com_texto`; a B1 muda a documentação dessa função para face SQL do juiz, sem alterar seu corpo nesse hunk. (`app/chamados/services/materializacao.py:247`, `app/chamados/services/materializacao.py:249`; B1 `app/chamados/juizes.py`, hunk `@@ -1552,7 +1573,7 @@`.) | Conservar a face SQL e as expectativas de conjunto; não substituir por instante isolado nem por `exclude(resposta_colab='')`. (`app/chamados/services/materializacao.py:249`; hunk B1 citado.) |
| **Leitores de uma só metade** | A metade principal do par testa somente texto; a validação testa somente instante; a partição do app também usa instante. (`app/chamados/services/materializacao.py:188`; `app/chamados/services/validacao.py:521`; `app/chamados/services/pergunta_no_app.py:76`.) | Registrar a dependência, sem afirmar que B1 já migrou esses leitores. Trocar seus predicados pode alterar o tratamento do legado; B1 reserva essas classes para B2. (B1 `app/chamados/juizes.py`, hunk `@@ -1531,6 +1534,24 @@`.) |
| **Porta de integridade em `fio.py`** | A porta atual recusa instante sem texto. B1 renomeia a função e acrescenta `TextoSemCarimbo`. (`app/chamados/services/fio.py:1598`, `app/chamados/services/fio.py:1605`; B1 hunk `@@ -1592,17 +1592,25 @@`.) | Preservar a renomeação e os chamadores B1 em nascimento, save, lote e resposta automática. O tratamento BUG-PAR da testemunha não chama essa porta antiga. (`app/chamados/services/fio.py:372`; B1 `app/chamados/models.py`, hunks `@@ -2918,8 +2936,8 @@`, `@@ -3339,20 +3357,21 @@`, `@@ -3526,44 +3545,40 @@`; B1 `disputa_emissao.py`, hunk `@@ -1664,11 +1664,12 @@`.) |
| **Save e meia escrita do par** | O save atual compara o par carregado em memória; B1 passa a julgar o par efetivamente persistido, lendo do banco a metade omitida por `update_fields` e comparando com o banco atual. (`app/chamados/models.py:3550`; B1 hunk `@@ -3526,44 +3545,40 @@`.) | Fixtures e chamadas que mudam a resposta devem persistir o par completo, ou completar legalmente a metade existente. Ter o par legal apenas na memória deixa de absolver um `update_fields` incompleto. (B1 hunk citado; B1 `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:121`, no anexo.) |
| **Lote e ressuscitação** | A ressuscitação atual zera somente o instante no lote. B1 zera também o texto da simulação e delega a escrita a `reabrir_para_correcao(... REABERTURA)`. (`app/chamados/models.py:1555`, `app/chamados/models.py:1613`; B1 hunks `@@ -1553,13 +1553,15 @@`, `@@ -1609,10 +1611,16 @@`.) | Conservar o par inteiro vazio, a foto anterior na REABERTURA e o evento da célula. Expectativas de ressuscitação sem esse ato precisam acompanhar a nova sequência. (`app/chamados/services/disputa_emissao.py:1609`, `app/chamados/services/disputa_emissao.py:1631`; hunks B1 citados.) |
| **Devolução BUG-PAR** | A devolução103 limpa materialização e validação, preserva a resposta e registra `DEVOLUCAO`. A B1 muda a ressuscitação para uma porta que apaga a resposta e registra `REABERTURA`. (`app/chamados/services/validacao.py:293`, `app/chamados/services/validacao.py:299`, `app/chamados/services/validacao.py:303`; B1 `models.py`, hunk `@@ -1609,10 +1611,16 @@`.) | **Preservar a DEVOLUCAO BUG-PAR.** Substituí-la pela reabertura B1 apagaria a resposta que será validada e mudaria o próximo responsável. (`app/chamados/services/validacao.py:304`; `app/chamados/services/tarefa_do_dia.py:152`, `app/chamados/services/tarefa_do_dia.py:154`.) |
| **Tarefa: decisão e dono** | RESPOSTA com texto deixa validar/admin; RECUSA ou REABERTURA deixa responder/colaborador; DEVOLUCAO deixa validar/admin. (`app/chamados/services/tarefa_do_dia.py:148`, `app/chamados/services/tarefa_do_dia.py:152`, `app/chamados/services/tarefa_do_dia.py:154`.) B1 acrescenta a REABERTURA da ressuscitação. (B1 `models.py`, hunk `@@ -1609,10 +1611,16 @@`.) | Testar a ressuscitação como REABERTURA/colaborador e a devolução BUG-PAR como DEVOLUCAO/admin. Não manter a validação anterior como último ato depois de uma REABERTURA posterior. (`app/chamados/services/tarefa_do_dia.py:147`, `app/chamados/services/tarefa_do_dia.py:152`.) |
| **Tarefa: fila e tempo** | A carga ordena decisões por pergunta, instante e PK, corta por `agora` e consulta a fila no mesmo instante; a porta administrativa só existe se a âncora estiver na fila. (`app/chamados/services/tarefa_do_dia.py:142`, `app/chamados/services/tarefa_do_dia.py:410`, `app/chamados/services/tarefa_do_dia.py:436`.) | Manter esse corte temporal e verificar fila/porta ao testar o dono após o ato novo B1. DEVOLUCAO, isoladamente, não prova exposição no painel. (`app/chamados/services/tarefa_do_dia.py:142`, `app/chamados/services/tarefa_do_dia.py:613`; B1 hunk de ressuscitação citado.) |

Há ainda estes cruzamentos que não devem ser confundidos com mudança de regra:

- **Fio e créditos:** `espera_o_texto_do_colaborador` já equivale à negação de `I E T`; a montagem publica seu inverso em `resposta_enviada`. A B1 não troca esse leitor no diff fornecido. Já `raiox.creditos_perguntas` passa da conjunção sobre tuplas para o juiz sobre `SimpleNamespace`, preservando a mesma separação. (`app/chamados/services/fio.py:177`, `app/chamados/services/fio.py:187`, `app/chamados/services/fio.py:357`; `app/chamados/services/raiox.py:143`; B1 `raiox.py`, hunk `@@ -138,10 +138,13 @@`.)
- **Retenção e property:** a retenção sem veredito continua usando `I OU T`, e `PerguntaDisputa.respondida` continua usando somente instante. Não cabe atribuir à B1 a migração desses dois predicados para o juiz novo. (`app/chamados/models.py:1905`, `app/chamados/models.py:1907`, `app/chamados/models.py:3640`; B1 `juizes.py`, hunk `@@ -1531,6 +1534,24 @@`.)
- **S128:** B1 troca a conjunção inline por `pergunta_respondida(p)` ao preservar resposta ainda não materializada de fato. A proteção da pergunta em voo continua relevante durante o plantio BUG-PAR; a substituição do predicado não autoriza outro escritor a carimbá-la. (`app/chamados/services/disputa_emissao.py:1997`; `app/chamados/services/materializacao.py:1623`, `app/chamados/services/materializacao.py:1755`; B1 `disputa_emissao.py`, hunk `@@ -1997,8 +1998,8 @@`.)
- **I21:** B1 passa a usar a face SQL com trim, excluindo carimbo com somente brancos. `materializada_em__isnull=True` já era lido antes; essa leitura não nasce na B1. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1572`; B1 hunk `@@ -1569,10 +1569,12 @@`.)

Os escritores e consumidores dos campos cruzados ficam assim:

| Campo/ato | Sítios relevantes na raia | Efeito do pouso B1 | [nao verificado]
|---|---|---|
| **`materializada_em`** | `_materializar_uma` chama `carimbar`; o escritor central atribui o carimbo. `_validar_o_par` exige as duas materializadas e o laço relê a pergunta para não repetir a irmã já gravada. (`app/chamados/services/materializacao.py:1812`; `app/chamados/models.py:3182`; `app/chamados/services/validacao.py:490`; `app/chamados/services/materializacao.py:1944`.) | Preservar esses caminhos. A B1 passa a desfazer o carimbo da ressuscitação pela reabertura existente; os leitores de irmã, corte103 e aplicação precisam observar esse estado limpo. (`app/chamados/services/materializacao.py:145`, `app/chamados/services/materializacao.py:276`, `app/chamados/services/materializacao.py:748`; B1 `models.py`, hunk de ressuscitação.) |
| **Instante e texto da resposta** | A resposta real grava ambos; `carimbar` só completa instante quando existe texto; a devolução103 conserva ambos. (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/models.py:3213`; `app/chamados/services/validacao.py:294`.) | Esses caminhos completos ou guardados são compatíveis com a recusa B1. Escrita nova só de texto, ou retirada só do instante de quem tem texto, passa a ser recusada. (B1 `models.py`, hunks de save e lote citados; execução: [não verificado].) |
| **`DecisaoPergunta`** | A validação grava VALIDACAO com texto; a devolução grava DEVOLUCAO; a autoridade de passivo preserva RESPOSTA/VALIDACAO anteriores. (`app/chamados/services/validacao.py:167`, `app/chamados/services/validacao.py:302`; `app/chamados/models.py:4074`, `app/chamados/models.py:4078`.) | B1 acrescenta REABERTURA com foto na ressuscitação. A tarefa lê a sequência; o rótulo BUG-PAR lê especificamente DEVOLUCAO com motivo BUG-PAR-103 e não deve aparecer para a nova REABERTURA. (`app/chamados/services/tarefa_do_dia.py:410`; `app/chamados/services/materializacao.py:291`; B1 `models.py`, hunk de ressuscitação.) |

`app/docs/JUIZES.md` diz que o registro vivo manda e que divergências documentais devem ser corrigidas; documenta tarefa por última decisão/fila e tranca C20, mas não contém a entrada C21 nesse trecho. A B1 acrescenta C21 ao registro vivo; sua alteração efetiva de documentação é [não verificado], pois o anexo exclui docs. A volta precisa reconciliar a tabela com `pergunta_respondida`, a recusa simétrica e a ressuscitação por REABERTURA, preservando a diferença da DEVOLUCAO ao admin. (`app/docs/JUIZES.md:12`, `app/docs/JUIZES.md:14`, `app/docs/JUIZES.md:78`, `app/docs/JUIZES.md:94`; B1 `app/core/juizes.py`, hunks `@@ -163,6 +163,11 @@` e `@@ -298,6 +303,7 @@`.)

**3) Não há quebra necessária demonstrada dos testes próprios BUG-PAR quando todos os hunks B1 são preservados.** Há falhas previsíveis se a volta perder correções B1 ou confundir os atos; aprovação da suíte completa permanece [não verificado]. (`app/chamados/tests/test_bug_par_projetado.py:78`, `app/chamados/tests/test_bug_par_projetado.py:253`, `app/chamados/tests/test_bug_par_projetado.py:308`.)

| Teste/assert da raia | Quebra previsível e condição | [nao verificado]
|---|---|
| `test_s139.py`, asserts de ausência de batida e espera em `:53` e `:55` | **Sem a correção de fixture B1**, o `create` de texto sem instante em `:43` será recusado antes dos asserts. **Com a correção B1**, manter os asserts da raia; não há quebra demonstrada dessa expectativa. (`app/chamados/tests/test_s139.py:43`, `app/chamados/tests/test_s139.py:53`; B1 hunk `@@ -42,7 +42,8 @@`.) |
| `test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit`, assert `:550` | Falha se ficarem os quatro números antigos de `juizes.py`, ou se a consolidação perder a referência correta da materialização. B1 já fornece os quatro deslocamentos; novas alterações da volta exigem conferir novamente as linhas finais. (`app/core/tests/test_medida_turno_do_par.py:258`, `app/core/tests/test_medida_turno_do_par.py:271`, `app/core/tests/test_medida_turno_do_par.py:550`; B1 hunk `@@ -255,10 +255,10 @@`.) |
| BUG-PAR-103: texto preservado e exatamente uma DEVOLUCAO, asserts `:526` e `:528` | Falhariam se a devolução fosse substituída pela reabertura B1. Essa substituição apagaria o texto e produziria outro ato; não é consequência necessária do merge fornecido. (`app/chamados/tests/test_bug_par_projetado.py:526`, `app/chamados/tests/test_bug_par_projetado.py:528`; B1 `models.py`, hunk de ressuscitação.) |
| Tarefa por DEVOLUCAO: assert `:229`; matriz de atos `:733` | A expectativa DEVOLUCAO→validar/admin continua coerente. Para ressuscitação real B1, a expectativa deve ser REABERTURA→responder/colaborador; misturar os dois fluxos causaria falha de razão/dono. (`app/chamados/tests/test_e2_tarefa_do_dia.py:229`; `app/chamados/tests/test_e3_portas_tarefa.py:733`; `app/chamados/services/tarefa_do_dia.py:152`; B1 hunk de ressuscitação.) |
| Ambientes que extraem o corpo do C6 | Aplicar somente a mudança de `juizes.py` deixaria dependências ausentes nos extratores. **A B1 completa já acrescenta o juiz e `SimpleNamespace` aos apoios correspondentes**; não é correto classificar esse erro parcial como quebra do merge completo. (`app/chamados/tests/e3_c8b_apoio.py:109`; `app/chamados/tests/e3_v8_apoio.py:182`; `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:158`; `app/chamados/tests/test_e4b_classe_e_memoria.py:304`; hunks B1 desses arquivos.) |

As fixtures próprias centrais respondem pela porta real ou escrevem instante e texto juntos; os testes de travas verificam ordem, sem congelar um teto de consultas. Portanto, nem a recusa simétrica nem o SELECT adicional do save demonstram, por si, quebra desses asserts BUG-PAR. (`app/chamados/tests/test_bug_par_projetado.py:78`, `app/chamados/tests/test_bug_par_projetado.py:253`, `app/chamados/tests/test_bug_par_projetado.py:308`, `app/chamados/tests/test_bug_par_projetado.py:417`, `app/chamados/tests/test_bug_par_projetado.py:588`; B1 `models.py`, hunk `@@ -3526,44 +3545,40 @@`; execução: [não verificado].)

## 16/11 08:48 · sessao 38a79e603691 · COMPLEMENTO-LAUDO-[nome]-1611 · censo · origem=Code

**Geofence e feriado podem usar postos diferentes:** a geofence da batida normal usa `Colaborador.posto`; o feriado usa `EscalaColaborador.posto` do vínculo que responde pelo dia. O previsto vem do vínculo/tipo/célula, com participação do posto do vínculo no feriado. (`app/ponto/models.py:100`; `app/ponto/services/geofence.py:258`; `app/core/feriados.py:131`; `app/core/feriados.py:161`; `app/escala/models.py:1095`; `app/escala/models.py:1395`)

O estado relatado do `col237` — cadastro 444, vínculo 204/posto 487, troca em 22/10 — não foi consultado em banco nesta cópia: **[nao verificado]**. A quantidade atual de divergências também é **[nao verificado]**. O código contém comentário sobre a medição histórica de 05/11: 45 colaboradores com postos divergentes, dos quais 11 com município diferente. (`app/core/feriados.py:120`)

A correspondência da árvore ao HASH informado é **[nao verificado]**.

**1. Censo de escritores**

Nas tabelas, **C** significa `Colaborador.posto` e **E** significa `EscalaColaborador.posto`. São FKs independentes; C permite nulo, E exige posto. (`app/colaboradores/models.py:322`; `app/escala/models.py:873`)

| Campo | Função e escrita | Trilha | Grava o outro posto no mesmo ato? |
|---|---|---|---|
| C | `vincular_posto`: atribuição e `salvar_inteiro`. `app/colaboradores/views.py:327`, `app/colaboradores/views.py:329` | `registrar_mudanca`, tipo posto. `app/colaboradores/views.py:331` | Não no corpo desse caminho. `app/colaboradores/views.py:325–332` |
| C | `posto_add_colab`: `gravar({'posto': posto})`. `app/colaboradores/views.py:645` | `HistoricoVinculo`, motivo vínculo, no mesmo `atomic`. `app/colaboradores/views.py:643–651` | Não. `app/colaboradores/views.py:643–651` |
| C | `posto_remove_colab`: `gravar({'posto': None})`. `app/colaboradores/views.py:670` | `HistoricoVinculo`, motivo desvínculo, no mesmo `atomic`. `app/colaboradores/views.py:668–675` | Não. `app/colaboradores/views.py:668–675` |
| C | `admitir_colaborador`: criação com `posto_id`. `app/colaboradores/views.py:1185`, `app/colaboradores/views.py:1190` | Evento de admissão quando há `posto_id`. `app/colaboradores/views.py:1219–1227` | Não cria E nesse caminho. `app/colaboradores/views.py:1185–1229` |
| C | `transferir_posto`: atribui posto e empresa, salva inteiro. `app/colaboradores/views.py:1388–1391` | Evento de transferência, no mesmo `atomic`. `app/colaboradores/views.py:1387`, `app/colaboradores/views.py:1396–1402` | Não. `app/colaboradores/views.py:1387–1403` |
| C, regravação | `editar_colaborador`: `salvar_inteiro`, sem atribuição nova de posto no trecho. `app/colaboradores/views.py:1314–1345` | Sem chamada de trilha no trecho de persistência/retorno. `app/colaboradores/views.py:1343–1355` | Não nesse trecho. `app/colaboradores/views.py:1343–1355` |
| C, regravação | `executar_desligamento`, ramo efetivo: salva objeto inteiro após mudar situação/demissão/motivo. `app/colaboradores/services/desligamento.py:129–133` | `HistoricoVinculo` e log de desligamento. `app/colaboradores/services/desligamento.py:139`, `app/colaboradores/services/desligamento.py:229` | Fecha vínculos; não altera E.posto. `app/colaboradores/services/desligamento.py:147–149` |
| C, regravação | `executar_reativacao`: salva inteiro após mudar situação/demissão/motivo. `app/colaboradores/services/desligamento.py:242–246` | Evento de reativação. `app/colaboradores/services/desligamento.py:251–257` | Não escreve E.posto nem reativa escala nessa função. `app/colaboradores/services/desligamento.py:235–258` |
| C, persistência delegada | `onboarding_salvar_selfie`: `colab.foto.save(..., save=True)`. `app/core/views.py:1061` | Sem chamada de trilha nesse bloco. `app/core/views.py:1048–1064` | Sem escrita explícita de E. A extensão interna da regravação por `FieldFile.save` é **[nao verificado]**. |
| E | `executar_vinculo`, existente: copia C.posto e faz `save()` completo. `app/colaboradores/services/vinculo.py:275`, `app/colaboradores/services/vinculo.py:283` | Histórico condicionado a `acao != 'no_op'`; log de diferenças inclui posto. `app/colaboradores/services/vinculo.py:36`, `app/colaboradores/services/vinculo.py:349–358`, `app/colaboradores/services/vinculo.py:371–379` | Lê C, não o escreve. `app/colaboradores/services/vinculo.py:274–283` |
| E | `executar_vinculo`, novo: cria copiando C.posto. `app/colaboradores/services/vinculo.py:287–296` | Histórico acima e log de criação. `app/colaboradores/services/vinculo.py:354`, `app/colaboradores/services/vinculo.py:381–383` | Não escreve C. `app/colaboradores/services/vinculo.py:287–297` |
| E | `criar_trecho_retroativo`: cria vínculo inativo copiando `ec.posto`. `app/colaboradores/services/vinculo.py:967–974` | O comando chamador registra log de criação; a foto desse log não inclui posto. `app/colaboradores/management/commands/corrigir_escala_retroativa.py:196–201` | Não escreve C. `app/colaboradores/services/vinculo.py:970–974` |
| E | `ajustar_vinculo_pelo_sistema`: cria copiando `ec.posto`. `app/colaboradores/services/vinculo.py:1084–1092` | Chamador `aplicar_no_vinculo` registra `HistoricoVinculo`, com IDs antigo/novo. `app/escala/services/escala_auto_executor.py:101–110` | Não escreve C. `app/colaboradores/services/vinculo.py:1087–1092` |
| E | `desfazer_ajuste_do_sistema`, recriação: copia `antigo.posto`. `app/colaboradores/services/vinculo.py:1110–1113` | Chamador `desfazer` registra `HistoricoVinculo`. `app/escala/services/escala_auto_executor.py:144–149` | Não escreve C. O outro ramo apenas remove o novo e reabre o antigo. `app/colaboradores/services/vinculo.py:1101–1113` |

Os pontos de persistência abaixo completam o censo das portas:

| Sítio | Alcance comprovado |
|---|---|
| `colaborador.gravar` | Atribui campos e chama `save(update_fields=...)`; operação posto permite `posto` e `empresa`. Não contém histórico nem sincronização de E. `app/colaboradores/services/colaborador.py:33`, `app/colaboradores/services/colaborador.py:55–62` |
| `colaborador.salvar_campos` | Salva os campos informados; pode aceitar posto pela operação correspondente. Não contém histórico nem sincronização de E. `app/colaboradores/services/colaborador.py:65–70` |
| `colaborador.salvar_inteiro` | Faz `colab.save()` completo; confere a operação, não restringe os campos do objeto. `app/colaboradores/services/colaborador.py:73–77` |
| `colaborador.criar_colaborador` | Encaminha os campos para `Colaborador.objects.create`. `app/colaboradores/services/colaborador.py:91–93` |
| `Colaborador.save` | Termina em `super().save(*args, **kwargs)`; não atribui posto nessa implementação. `app/colaboradores/models.py:415–439` |
| `colaborador.atualizar_em_lote` | Rejeita expressamente `posto` e `posto_id`. `app/colaboradores/services/colaborador.py:80–88` |
| `vinculo.gravar_regua` | Porta genérica com `qs.update(**campos)`; os quatro chamadores consultados passam campos da régua, sem posto. `app/colaboradores/services/vinculo.py:1048–1070`; `app/escala/regua_defesa.py:662`, `app/escala/regua_defesa.py:719`, `app/escala/regua_defesa.py:742`, `app/escala/regua_defesa.py:765` |

Admin, importação, comandos e signals:

- Os admins de C e E herdam `SoLeitura`, que bloqueia inclusão/alteração/exclusão e torna os campos somente leitura. (`app/colaboradores/admin.py:48`; `app/escala/admin.py:19`; `app/core/admin.py:59–75`)
- O importador Nexti consultado salva telefone/email, sem posto. (`app/colaboradores/management/commands/importar_telefones_nexti.py:113`)
- `smoke_chamados._fixture` cria C com posto, mas o comando exige banco com `test` no nome e desfaz a fixture por rollback. (`app/chamados/management/commands/smoke_chamados.py:33–45`; `app/chamados/management/commands/smoke_chamados.py:78`)
- Os receivers consultados não sincronizam C.posto e E.posto: tratam onboarding/grupo, eventos de posto, regeneração, previsto, disputas e folgas. (`app/colaboradores/signals.py:9`, `app/colaboradores/signals.py:49`, `app/colaboradores/signals.py:102`, `app/colaboradores/signals.py:119`; `app/escala/signals.py:19`, `app/escala/signals.py:35`, `app/escala/signals.py:87`, `app/escala/signals.py:138`, `app/escala/signals.py:156`, `app/escala/signals.py:186`)

Outros escritores por mecanismo dinâmico não resolvido na leitura estática: **[nao verificado]**.

**2. Censo de leitores**

As referências incluem acesso ao atributo, filtros/projeções ORM, relações reversas e carregamento explícito por `select_related`. Nas referências com várias linhas, todas pertencem ao arquivo indicado.

**Cadastro e serviços de colaboradores**

| Campo | Função | Arquivo:linha |
|---|---|---|
| C | `vincular_colaboradores` | `app/colaboradores/views.py:41,42,46,72,80,105,139` |
| C | `passe_livre` | `app/colaboradores/views.py:205` |
| C | `painel_vinculo` | `app/colaboradores/views.py:267` |
| C | `vincular_posto` | `app/colaboradores/views.py:319,326,338,342,357,361` |
| C | `vincular_escala` / `_render_row` | `app/colaboradores/views.py:386,396,401` |
| C | `detalhe_colaborador` | `app/colaboradores/views.py:462` |
| C | `painel_posto` | `app/colaboradores/views.py:602` |
| C | `buscar_colabs_posto` | `app/colaboradores/views.py:619,625` |
| C | `posto_add_colab` | `app/colaboradores/views.py:642` |
| C | `posto_remove_colab` | `app/colaboradores/views.py:667` |
| C | `toggle_posto`, relação reversa | `app/colaboradores/views.py:884` |
| C | `painel_colaborador` | `app/colaboradores/views.py:924` |
| C | `lista_colaboradores` | `app/colaboradores/views.py:1021,1026,1052` |
| C | `transferir_posto` | `app/colaboradores/views.py:1381` |
| C | `gestao_usuarios` | `app/colaboradores/views.py:1450` |
| C | `lista_beneficios` | `app/colaboradores/views.py:1683,1687,1692,1696,1752,1753` |
| C | `calendario_ponto`, carregamento | `app/colaboradores/views.py:1947` |
| C | `agenda_modal_view`, carregamento | `app/colaboradores/views.py:2206` |
| C | `painel_situacional` | `app/colaboradores/views.py:2599,2625` |
| C | `painel_situacional_linhas` | `app/colaboradores/views.py:2785,2789` |
| C | `disponibilidade` | `app/colaboradores/views.py:2852,2858,2862,2891,2902,2908,2918,2928` |
| C | `Posto.colaboradores_ativos`, reversa | `app/colaboradores/models.py:280` |
| C | `ColaboradorAdmin`, exibição | `app/colaboradores/admin.py:49` |
| C | `get_postos_visiveis` | `app/colaboradores/mixins.py:231,232` |
| C | `get_colaboradores_visiveis` | `app/colaboradores/mixins.py:260` |
| C | `posto_post_save_revalida_disputas` | `app/colaboradores/signals.py:72` |
| C | `montar_linha_colab` / `_cell_colab_status` | `app/colaboradores/painel_cell.py:40,56,57,58,77` |
| C | `pendencias_do_colaborador` / `contador` | `app/colaboradores/services/cadastro.py:80,105,106,111` |
| C | `executar_desligamento` / `executar_reativacao` | `app/colaboradores/services/desligamento.py:114,141,253` |
| C | `contexto_detalhe` | `app/colaboradores/services/detalhe.py:92,93,94,133,134,137` |
| C | `montar_situacional` | `app/colaboradores/services/situacional.py:179,201,202,203` |
| C | `registrar_mudanca`, posto padrão da trilha | `app/colaboradores/services/historico_vinculo.py:16` |
| C | `ficha_do_colab` | `app/colaboradores/services/ficha.py:211,212,218` |
| E | `linha_do_vinculo` / carga em `ficha_do_colab` | `app/colaboradores/services/ficha.py:95,123` |
| E e C | `contexto_drawer`: prefere E, fallback C | `app/colaboradores/services/drawer.py:27,35` |
| C | `julgar_vinculo` | `app/colaboradores/services/vinculo.py:74` |
| C | `executar_vinculo` | `app/colaboradores/services/vinculo.py:275,289,296` |
| E | `_foto_da_trilha` | `app/colaboradores/services/vinculo.py:36` |
| E | `reavaliar_pela_troca_de_municipio` | `app/colaboradores/services/vinculo.py:953,954` |
| E | `criar_trecho_retroativo` | `app/colaboradores/services/vinculo.py:971` |
| E | `ajustar_vinculo_pelo_sistema` | `app/colaboradores/services/vinculo.py:1090` |
| E | `desfazer_ajuste_do_sistema` | `app/colaboradores/services/vinculo.py:1111` |
| E | `presenca_em_feriado` | `app/colaboradores/services/feriado_do_posto.py:96,105` |
| E | `raios_fora_do_padrao` | `app/colaboradores/services/geofence_raios.py:63,64` |
| E | `marcar_feriado_do_posto.Command._marcar` | `app/colaboradores/management/commands/marcar_feriado_do_posto.py:104,110` |
| C | `pauta_aparelhos_compartilhados.Command.handle` | `app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:48,58,63,78,109` |

**Escala, célula e previsto**

| Campo | Função | Arquivo:linha |
|---|---|---|
| C | `escala_buscar_colabs` | `app/escala/views.py:248,249` |
| C | `propostas` | `app/escala/services/propositor.py:443,542` |
| C | `EIXOS` / `ranking` | `app/escala/services/furos_diarios.py:118,119,135,137,145,148,156,159` |
| C | `ranking_colabs` | `app/escala/services/furos_diarios.py:180,182,183,237` |
| C | `universo_empresa` | `app/escala/services/furos_diarios.py:259,279,280,325,326,327` |
| E | `minha_escala` | `app/escala/views.py:17,20,25` |
| E | `EscalaColaboradorAdmin` | `app/escala/admin.py:27,31` |
| E | `plano_folgas`, reversa `Posto.escalas` | `app/escala/views_plano_folgas.py:43,44`; FK em `app/escala/models.py:873` |
| E | `_grid_ctx` | `app/escala/views_plano_folgas.py:58,59` |
| E | `escalas_do_periodo_por_colab`, carga posto/município | `app/escala/alimentacao.py:148,152,157,158` |
| E | `vinculos_5x2_6x1_trabalha_feriado` | `app/escala/models.py:928,932` |
| E, indireto | `feriado_vira_folga` → `lugar_do_vinculo` | `app/escala/models.py:1394,1395`; `app/core/feriados.py:131,132` |
| E | `observado_meio_periodo` | `app/escala/services/meio_periodo.py:153,187,188` |
| E | `beco_em_lote` | `app/escala/services/cadastro_realidade.py:513,517` |
| E | `_vinculos` | `app/escala/services/cadastro_realidade.py:761,762,765,768` |
| E | `_colabs_do_posto` | `app/escala/services/semana_do_posto.py:76,77,79` |

`vinculo_do_dia` não compara C.posto com E.posto: lê a geradora da célula, procura aquele vínculo e verifica suas datas; sem geradora, escolhe entre os vínculos que cobrem a data por `(ativa, data_inicio)`. (`app/escala/alimentacao.py:269–286`)

Os caminhos afins que alimentam essa seleção são:

| Função | Campo/caminho | Arquivo:linha |
|---|---|---|
| `vinculo_do_dia_carregado` | Compõe carregadores; chega a E pelo vínculo selecionado | `app/escala/alimentacao.py:316,317` |
| `vinculos_do_dia_carregado` | Mesma seleção para vários colaboradores | `app/escala/alimentacao.py:329,330` |
| `vinculos_dos_dias` | Carrega células/vínculos e chama `vinculo_do_dia` | `app/escala/alimentacao.py:355,357,358` |
| `escala_vigente` | Célula/geradora e mesma seleção | `app/escala/servico_jornada.py:46,48,61,63,64,66` |
| `_esc_vigente_do_dia` | Mesma seleção para o previsto | `app/escala/utils.py:2180,2181,2191,2194` |
| `minutos_previstos_do_dia` | Vínculo escolhido, trabalho e marcos; E participa pelo feriado | `app/escala/utils.py:2260,2266,2269`; `app/escala/models.py:1395` |
| `previstas_do_periodo` | Percorre fatias de vínculo e consulta `eh_dia_trabalho` | `app/escala/alimentacao.py:389–397` |

Há consumidores de valores de posto já derivados em `cadastro_realidade.acao`, `lista` e `pdf_bytes`, além do template e da busca do Plano de Folgas. (`app/escala/services/cadastro_realidade.py:713,725,905,931,962,1083,1084`; `app/templates/escala/cadastro_x_realidade.html:21,23`; `app/templates/escala/plano_folgas.html:245,247,251,294,296`)

**API, core, chamados e comunicados**

| Campo | Função | Arquivo:linha |
|---|---|---|
| C | `_validar_assinatura_keystore` | `app/api/views.py:191`; `app/api/views_core.py:114` |
| C | `api_me`, nome do posto | `app/api/views.py:344`; `app/api/views_core.py:332` |
| C, indireto | `api_me` → `posto_geo_do_colab` | `app/api/views.py:351`; `app/api/views_core.py:339`; `app/ponto/services/geofence.py:223,241` |
| E | `api_escala_v2` | `app/api/views.py:957` |
| C | `api_mensageria_sinais`, reversa | `app/api/views_mensageria.py:449` |
| C | `api_mensageria_situacao_ferias` | `app/api/views_mensageria.py:807,833` |
| E | `_onde_trabalha` | `app/api/views_mensageria.py:979–982` |
| C | `api_mensageria_pessoa` | `app/api/views_mensageria.py:1063` |
| C | `_cadastro_incompleto` | `app/api/views_mensageria.py:1287,1288` |
| C | `detectar_anomalias` | `app/chamados/detector_anomalias.py:89,101,104,105,109` |
| C | `abrir_chamado_local` / `abrir_chamado_para_colaborador` | `app/chamados/services/abertura.py:62,187` |
| C | `_texto` / `sincronizar_adesao` | `app/chamados/services/adesao.py:15,63` |
| C | `montar_qs_filtrado` / `montar_contexto_painel` | `app/chamados/services/painel.py:99,130,266` |
| C | `fio_colaborador` | `app/chamados/views.py:695` |
| C | `auditar_invariantes_chamados.Command.handle`, carga | `app/chamados/management/commands/auditar_invariantes_chamados.py:1218` |
| C | `para_colaborador` / `audiencia_de` | `app/comunicados/services.py:23,63` |
| C | `candidato` / `parecidos` | `app/core/busca.py:89,104` |
| C | `qs_colaborador` / `_colab` | `app/core/busca.py:127,154` |
| C | `cell_ultimas_batidas` | `app/core/cell.py:11` |
| C | `montar_painel_operacional` | `app/core/services/painel_op.py:69,260,261` |
| C | `dashboard` | `app/core/views.py:176,195,213` |
| C | `dashboard_supervisor` | `app/core/views.py:352,353,362,412` |
| C | `painel_tv` | `app/core/views.py:797,801,831` |
| C | `batida_detalhe_json` | `app/core/views.py:979,1012` |
| C | `dashboard_dados` | `app/core/views.py:1164,1180,1194` |
| E | `lugar_do_vinculo` | `app/core/feriados.py:131,132` |
| E, indireto | `feriados_do_colab` | `app/core/feriados.py:161` |
| E | `regua_para`, praça do vínculo do dia | `app/core/regua_cct.py:329,330` |
| C | `lavrar_placar_situacional.Command.handle`, carga | `app/core/management/commands/lavrar_placar_situacional.py:55` |

**Ponto: modelos, serviços, telas e comandos**

| Campo | Função | Arquivo:linha |
|---|---|---|
| C | `Batida.posto_para_calculo`, após `posto_efetivo` | `app/ponto/models.py:100` |
| C | `AlertaGeofence.ainda_vale` | `app/ponto/models.py:265` |
| C | `publicar_batida` | `app/ponto/realtime.py:28` |
| C | `bloquear_se_sem_posto` | `app/ponto/utils.py:25` |
| C | `abriu_nao_bateu.com_nomes` | `app/ponto/services/abriu_nao_bateu.py:113,115,143` |
| C | `deslogados.com_nomes` | `app/ponto/services/deslogados.py:202,206` |
| C | `dia_do_colab` | `app/ponto/services/dia_do_colab.py:78` |
| C | `diagnostico_escala` | `app/ponto/services/diagnostico_escala.py:196` |
| C | `passivo_da_competencia` | `app/ponto/services/dias_em_aberto.py:109,127` |
| C | `recalcular_fechamento_mes`, carga | `app/ponto/services/fechamento.py:619` |
| E, indireto | `recalcular_fechamento_mes`, feriados | `app/ponto/services/fechamento.py:648` |
| C | `montar_resumos` | `app/ponto/services/fechamento_lista.py:79` |
| E e C | `resumo_escala`, cabeçalho: E ou C | `app/ponto/services/espelho.py:203,207` |
| E, indireto | `autoridade_do_periodo`, feriados | `app/ponto/services/espelho.py:1029` |
| C | `vinculo_contagem` | `app/ponto/services/geofence.py:48` |
| C | `posto_do_colab_agora`, sem turno aberto | `app/ponto/services/geofence.py:223` |
| C, indireto | `posto_do_colab_agora`, com turno aberto | `app/ponto/services/geofence.py:222`; `app/ponto/models.py:100` |
| C, indireto | `verificar_geofence` | `app/ponto/services/geofence.py:258`; `app/ponto/models.py:100` |
| C | `reavaliar_chamados_geofence` | `app/ponto/services/geofence.py:422` |
| C | `gestao_he._cadastro` / `enriquecer` | `app/ponto/services/gestao_he.py:60,264,288,289,324` |
| C | `alvo_da_lavra` | `app/ponto/services/pautas_do_esmeril.py:31` |
| C | `triagem_bater_ponto` | `app/ponto/services/triagem_batida.py:80,120` |
| C | `vigia_entrada.do_dia` | `app/ponto/services/vigia_entrada.py:121,136` |
| C | `vinculo_na_lista.achar` | `app/ponto/services/vinculo_na_lista.py:65,66,76,79,80,83,88` |
| C | `vinculo_na_lista.item` / `colabs_em_revisao` | `app/ponto/services/vinculo_na_lista.py:106,153,154` |
| E | `regenerar_futuras_por_feriado`, carga da geradora | `app/ponto

[truncado em 20000 de 36765 caracteres]

## 16/11 08:47 · sessao baa774361d2c · LOTE7-C7 · verificador · origem=Code

A correção de A está demonstrada estaticamente; o bloco J1 saiu da regularização, e os asserts do item 6 ganharam mordida. Isso não certifica o diff inteiro da obra nem o verde executado: a cópia disponível não tem histórico Git, árvores base/c6 ou os logs citados. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`; `app/ponto/services/regularizacao.py:102`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:457`; `[nao verificado]`)

Nas referências abaixo, todos os caminhos são relativos à cópia.

1. **Achado A — motivo da falha, caminho verde e mutantes.** [nao verificado]

   Considerando o comportamento da base informado no enunciado — `update()` que rejeita sem trilha —, o teste c7 falharia por **asserção na linha 243**, com a mensagem da linha 244: existem ausências rejeitadas persistidas. Não depende mais de importar a classe nova; importa somente a função. A execução contra a base permanece `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:235`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`)

   Na c7, o caminho é: abre `atomic` → salva a primeira rejeição → `evento`, substituído pelo mock, retorna `None` → levanta `TrilhaDaRejeicaoNaoGravada` dentro do bloco → desfaz a escrita. O teste captura sua classe-base `RuntimeError`, exige ausência de rejeição persistida, exige exceção e confere seu nome. Esse é o caminho estático esperado de aprovação; execução verde `[nao verificado]`. (`app/ponto/services/ausencia.py:122`; `app/ponto/services/ausencia.py:1483`; `app/ponto/services/ausencia.py:1497`; `app/ponto/services/ausencia.py:1505`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:241`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`)

   | Mutante | Resultado inferido pela leitura |
   |---|---|
   | Levantar outra `RuntimeError` antes de gravar | Falha na conferência do nome concreto. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:246`) |
   | Levantar depois de confirmar as rejeições | Falha porque ficaram linhas rejeitadas. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`) |
   | Não levantar e não rejeitar | Falha porque `levantou` continua `None`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:245`) |
   | Levantar exceção que não herda de `RuntimeError` | Escapa do handler e produz erro. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:241`) |
   | Levantar a própria exceção antes de tentar escrever | **Passa neste teste**: ele não exige tentativa de escrita. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:245`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:246`) |
   | Alterar outro campo, ou remover linhas, confirmar e levantar a exceção esperada | **Pode passar neste teste**: ele verifica inexistência de rejeitadas, sem comparar o conjunto original nem outros campos. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`) |

   A sobrevivência desses mutantes é inferência estática; mutação executada `[nao verificado]`.

2. **Achados B/C — retirada do J1 e caracterização.**

   O bloco retirado não permanece na atualização da Justificativa: ela grava `pendente` e segue diretamente para o passo 5b. Há outras invalidações por **aprovação** de Justificativa, que respondem a outra transição; não são a reaparição do bloco aprovada→pendente. (`app/ponto/services/regularizacao.py:102`; `app/ponto/services/regularizacao.py:108`; `app/ponto/services/justificativa.py:205`; `app/ponto/services/justificativa.py:215`; `app/ponto/services/justificativa.py:383`; `app/ponto/services/justificativa.py:390`)

   Igualdade byte a byte de `regularizacao.py` com `f8ee770a5`: **`[nao verificado]`**. O patch fornecido mostra a retirada, mas a árvore base não está disponível para comparação independente. `[nao verificado]`

   Permanecem referências ao J1 na docstring do módulo, na classe, no nome do teste e nas mensagens. São referências à **caracterização da ausência de invalidação**, sem exigir o comportamento removido. O selo monitora a porta de Ausência. Ausência de qualquer outra dependência no **diff inteiro base..c7**: `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:20`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:266`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:272`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:286`; `app/ponto/tests/test_lote7_selo_escritor_acorda.py:35`)

   O teste J1 afirma: retorno `ok`, Justificativa agora `pendente`, zero chamadas de `invalidar_fechamento` e carimbo preservado. Ele **não cobre Ausência intermediária**: hoje é 05/11, a entrada é de 04/11 e o laço interrompe antes de criar ausência no próprio dia 05. Passar na base e na c7 é compatível com o código e com o relato anexado; execução nas duas árvores `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:56`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:275`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:283`; `app/ponto/services/regularizacao.py:111`)

3. **Kernel — alcance, banco e invalidação.**

   A entrada e a chamada ao serviço conferem. A enumeração **exaustiva dos sítios pertencentes ao diff base..c7** permanece `[nao verificado]`; o mapa abaixo cobre os ganchos LOTE7 identificáveis na árvore e os caminhos descritos na entrega. (`app/api/views_core.py:1134`; `app/api/views_core.py:1151`; `app/api/views_core.py:1179`)

   | Sítio/caminho | Banco | Pode chamar `invalidar_fechamento`? |
   |---|---|---|
   | `regularizar_turno_orfao` | Lê batida; lê/grava Justificativa, ausências, chamados e auditoria. | O bloco J1 não chama mais. (`app/ponto/services/regularizacao.py:52`; `app/ponto/services/regularizacao.py:91`; `app/ponto/services/regularizacao.py:118`; `app/ponto/services/regularizacao.py:130`; `app/ponto/services/regularizacao.py:150`) |
   | `garantir_ausencia` | Delega; não tem consulta própria. | Indiretamente, pela criação. (`app/ponto/services/ausencia.py:602`; `app/ponto/services/ausencia.py:610`) |
   | Criação nova → gancho em `:597` | A função grava ausência e trilha; o gancho acrescenta um SELECT do insumo. | Neste fluxo, não: falta nasce aguardando, fora da folha; os mapas são `{},{}`. (`app/ponto/services/ausencia.py:475`; `app/ponto/services/ausencia.py:562`; `app/ponto/services/ausencia.py:585`; `app/ponto/services/ausencia.py:597`; `app/ponto/turnos.py:2528`) |
   | Rejeitada existente → `_reabrir` → `mudar_estado` | Pode gravar descrição; grava estado e trilha. O gancho acrescenta dois SELECTs de insumo. | Neste fluxo, não: rejeitada→aguardando fica fora da folha nos dois lados. (`app/ponto/services/ausencia.py:433`; `app/ponto/services/ausencia.py:1297`; `app/ponto/services/ausencia.py:1315`; `app/ponto/services/ausencia.py:1319`; `app/ponto/services/ausencia.py:987`; `app/ponto/services/ausencia.py:993`; `app/ponto/turnos.py:2528`) |
   | Linha existente devolvida | Já houve consultas para localizar conflito; retorna antes do gancho novo. | Não por esse gancho. (`app/ponto/services/ausencia.py:427`; `app/ponto/services/ausencia.py:456`) |
   | `_insumo_da_folha` → `_pelo_efeito` | SELECT; devolve apenas linhas contadas pela folha. | Não. (`app/ponto/services/ausencia.py:138`; `app/ponto/services/ausencia.py:139`; `app/ponto/turnos.py:2529`) |
   | `_acordar_projecao` | Com `{},{}`, não consulta fechamento nem lê relógio; genericamente pode fazê-lo. | **Sim no grafo genérico**, em `:195`; zero chamadas nos estados acima. (`app/ponto/services/ausencia.py:183`; `app/ponto/services/ausencia.py:191`; `app/ponto/services/ausencia.py:195`) |
   | `_teto_da_linha_aberta` | SELECT de fechamento; eventual leitura da empresa. | Não diretamente; não é executado pelos mapas vazios acima. (`app/ponto/services/ausencia.py:147`; `app/ponto/services/ausencia.py:151`; `app/ponto/services/ausencia.py:183`) |
   | `invalidar_fechamento` → `competencias_do_fato` | Resolve competências e escreve `derivado_em=NULL`. | É o próprio invalid validador; não é executado pelo gancho nesses fluxos. (`app/ponto/services/fechamento.py:1599`; `app/ponto/services/fechamento.py:1649`; `app/ponto/services/fechamento.py:1656`) |

   Há efeitos indiretos de escrita: os `save()` de Justificativa/Ausência acionam cartório; o cartório pode gravar célula e evento. `carimbar`→`salvar_e_emitir` não chama recálculo imediatamente. Portanto “só leitura” descreve apenas o **acréscimo dos ganchos LOTE7 nesses estados**, não o serviço nem toda a execução do endpoint. (`app/ponto/signals.py:143`; `app/ponto/signals.py:161`; `app/ponto/signals.py:93`; `app/ponto/services/cartorio.py:1314`; `app/ponto/portas/celula.py:1201`; `app/ponto/portas/celula.py:962`)

   A seção Kernel confere nos caminhos citados e nos custos **adicionais** de um/dois SELECTs. “Zero sítios de invalidação real” é correto como resultado desses fluxos; não significa ausência de alcance sintático da função invalidante. Já “regularização, só leitura”, na abertura da entrega, é impreciso se qualificar o serviço inteiro. (`app/ponto/services/ausencia.py:597`; `app/ponto/services/ausencia.py:987`; `app/ponto/services/ausencia.py:993`; `app/ponto/services/ausencia.py:195`; `app/ponto/services/regularizacao.py:106`)

4. **Item 6 — mordida atual e mutantes sobreviventes.**

   O teste da célula agora mata: `save` substituído por `update` sem signal; juiz que retorna vazio/contadores zero; ausência de carimbo persistido; carimbo que conserva a FK das férias rejeitadas. A fixture começa com essa FK, o espião chama o juiz real, exige contador positivo para cada dia e confere os resultados persistidos. (`app/ponto/services/ausencia.py:1497`; `app/ponto/signals.py:143`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:446`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:451`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:457`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:461`)

   Ainda pode passar neste teste: [nao verificado]

   - Quebrar somente o ramo com adesão/batidas: a fixture percorre `nunca_bateu`, que incrementa `carimbadas` e retorna antes do ramo normal de `julgadas`. (`app/ponto/services/cartorio.py:1317`; `app/ponto/services/cartorio.py:1318`; `app/ponto/services/cartorio.py:1476`)
   - Gravar veredito/ata/impressão errados, ou timestamp incorreto porém não nulo, conservando contador positivo e FK nula: esses conteúdos não são comparados. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:457`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:461`; `app/ponto/portas/celula.py:1194`)

   `_nada_mudou` agora mata alteração de status/obs, exclusão ou substituição de ausência, mudança de data/status, alteração/remoção das trilhas antigas nos campos fotografados e alteração do saldo. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:510`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:531`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:533`)

   Ainda passam alterações em campos omitidos — por exemplo, `Ausencia.minutos/data_fim/descricao`, datas/dias do agendamento e descrição/IP da auditoria — e inserção de trilha nova indevida. A comparação das trilhas considera somente as PKs anteriores; “INTEIRO” significa o retrato enumerado, não a linha inteira. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:505`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:510`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:535`)

5. **Quem escreve, quem lê e significado.**

   Este é o mapa dos campos e valores comportamentais conferidos no patch e nos caminhos tocados. Censo exaustivo de **todos** os leitores/escritores do repositório: `[nao verificado]`.

   | Campo/valor | Quem escreve | Quem lê / significado |
   |---|---|---|
   | `Ausencia.pk`, vínculo ao agendamento, `tipo`, `data`, `status` | Espelho cria férias aprovadas; rejeição grava `rejeitada`. (`app/ponto/services/ausencia.py:1388`; `app/ponto/services/ausencia.py:1488`; `app/ponto/services/ausencia.py:1495`) | Snapshot e asserts identificam conjunto/status/data; folha conta aprovadas, cobrança exclui rejeitadas. São efeitos explicitamente diferentes. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:511`; `app/ponto/turnos.py:2528`; `app/ponto/turnos.py:2530`) |
   | `Ausencia.atualizado_em` | `save` da rejeição inclui o campo. (`app/ponto/services/ausencia.py:1496`) | Serve como toque/fato novo; não é carimbo da projeção. (`app/ponto/services/ausencia.py:1496`; `app/ponto/models.py:457`) |
   | `CelulaDia.colaborador/data/trabalha/origem/dna` | Fixture cria pessoa/dia, `trabalha=True`, origem gerada e marcos 08–17. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:446`) | Cartório lê dia, trabalho e DNA no julgamento/impressão. (`app/ponto/services/cartorio.py:1252`; `app/ponto/services/cartorio.py:1257`; `app/ponto/services/cartorio.py:1354`) |
   | `CelulaDia.ausencia` | Fixture semeia FK; porta `carimbar` grava cobertura. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:446`; `app/ponto/portas/celula.py:1196`) | Teste exige FK nula após rejeição; produção deriva cobertura para **cobrança**, não exclusivamente folha/aprovadas. Há concordância nesse caso de rejeição. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:462`; `app/ponto/services/cartorio.py:1228`; `app/ponto/services/cartorio.py:1356`) |
   | `CelulaDia.julgada_em` | `carimbar` grava `agora`. (`app/ponto/portas/celula.py:1194`) | Cartório usa no frescor; teste exige apenas preenchimento, não valor correto. (`app/ponto/services/cartorio.py:1260`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:461`) |
   | `julgadas` / `carimbadas` | Cartório inicializa ambos; incrementa após classificação / carimbo. (`app/ponto/services/cartorio.py:1955`; `app/ponto/services/cartorio.py:1476`; `app/ponto/services/cartorio.py:1317`; `app/ponto/services/cartorio.py:1875`) | Espião soma ambos como evidência de atividade. **Não são sinônimos**; o ramo normal pode incrementar ambos. A soma não prova dois julgamentos nem distingue os ramos. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:453`; `app/ponto/services/cartorio.py:1476`; `app/ponto/services/cartorio.py:1875`) |
   | `AgendamentoFerias.status`, inclusive `concluido/cancelado` | Cancelamento grava `cancelado`; teste de correção prepara `concluido`. (`app/ferias/services.py:688`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:562`) | Guardas recusam estados terminais; snapshot exige preservação. (`app/ferias/services.py:637`; `app/ferias/models.py:307`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:533`) |
   | `AgendamentoFerias.obs` | Cancelamento concatena assinatura/data/lote; criação aceita observação. (`app/ferias/services.py:584`; `app/ferias/services.py:685`; `app/ferias/services.py:687`) | Snapshot exige igualdade. Outro leitor infere origem por substrings: observação também carrega metadado interpretado, dívida preexistente à alteração apresentada. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:510`; `app/ferias/management/commands/listar_ferias_dp.py:27`; autoria temporal `[nao verificado]`) |
   | `PeriodoAquisitivo.dias_concedidos` | Cancelamento subtrai dias e salva. (`app/ferias/services.py:680`; `app/ferias/services.py:682`) | Helper compara com o saldo fotografado; representa dias concedidos, não diretamente saldo restante. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:501`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:531`) |
   | `LogAuditoria.pk/acao/modelo/objeto_id/valor_antes/valor_depois` | `evento` cria registro; rejeição fornece status anterior e rejeitado+agendamento posterior. (`app/core/observ.py:134`; `app/ponto/services/ausencia.py:1498`; `app/ponto/services/ausencia.py:1501`) | Teste positivo confere de/para por ausência; snapshot preserva registros anteriores. Os JSONs são envelopes genéricos contextualizados por ação/modelo, não esquema universal de status. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:218`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:512`; `app/core/models.py:296`) |
   | `Justificativa.status` | Fixture prepara aprovada; regularização grava pendente; portas de decisão gravam aprovada. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:278`; `app/ponto/services/regularizacao.py:105`; `app/ponto/services/justificativa.py:205`) | Folha lê aprovada; caracterização exige pendente sem invalidar. O significado do status concorda; permanece a lacuna de atualização da projeção. (`app/ponto/services/fechamento.py:662`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:285`) |
   | `Justificativa.saida_informada/descricao/atualizado_em` | Regularização preenche saída/descrição e salva os três junto do status. (`app/ponto/services/regularizacao.py:102`; `app/ponto/services/regularizacao.py:106`) | `saida_informada` condiciona a reabertura; o teste J1 confere somente status e efeito no fechamento. (`app/ponto/services/regularizacao.py:102`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:285`) |
   | `FechamentoMensal.derivado_em`, pessoa/mês/ano | Fixture carimba; derivação preenche; invalidação/restauração limpam. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:83`; `app/ponto/services/fechamento.py:1143`; `app/ponto/services/fechamento.py:1189`; `app/ponto/services/fechamento.py:1391`; `app/ponto/services/fechamento.py:1656`) | `_viva` testa não nulo; teto lê competências carimbadas; contador encontra nulos. Significado consistente: carimbo da derivação, NULL para nunca derivada/invalidada. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:97`; `app/ponto/services/ausencia.py:147`; `app/ponto/services/fechamento.py:1679`) |
   | Retorno `ok`, quantidade `5`, zero chamadas, datas fixas | Serviço retorna sucesso; rejeição retorna quantidade; fixtures fixam datas. (`app/ponto/services/regularizacao.py:157`; `app/ponto/services/ausencia.py:1510`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:56`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:192`) | Asserts exigem sucesso, cinco rejeições e ausência de invalidação J1; não significam validação de produção. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:283`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:286`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:456`; produção `[nao verificado]`) |

6. **Classes de reprovação.**

   - **Segundo juiz:** não identifiquei no acordador inspecionado. Ele pergunta ao predicado existente da folha e ao juiz dos dias; competências delegam à derivação existente. Ausência de duplicação no diff inteiro `[nao verificado]`. (`app/ponto/services/ausencia.py:138`; `app/ponto/services/ausencia.py:192`; `app/ponto/services/fechamento.py:1599`; `app/core/juizes.py:194`)
   - **Segundo escritor fora da porta:** a rejeição especial continua dentro de `ausencia.py`, embora seja exceção textual à afirmação “estado só muda por mudar_estado”. Escritores externos no diff inteiro `[nao verificado]`. (`app/ponto/services/ausencia.py:16`; `app/ponto/services/ausencia.py:1495`)
   - **Relógio fora da borda:** permanece. O acordador lê `localdate()` internamente e consulta fechamento para qualquer linha mudada com fim nulo, inclusive tipo de dia único; o juiz só prolonga tipos em aberto. (`app/ponto/services/ausencia.py:190`; `app/ponto/services/ausencia.py:147`; `app/ponto/turnos.py

[truncado em 20000 de 22884 caracteres]

## 16/11 08:46 · sessao 3fbea08596d5 · - · - · origem=Ronald (tmux)

**Testes próprios BUG-PAR: não encontrei quebra necessária causada pelo diff B1 fornecido; execução pós-merge [não verificado].** As fixtures principais respondem pela porta real, que grava o par (`test_bug_par_projetado.py:78-81`); as fixtures diretas já levam texto e instante juntos (`app/chamados/tests/test_bug_par_projetado.py:251-254`, `app/chamados/tests/test_bug_par_projetado.py:306-312`).

1. **`test_s139.py`: fixture B1 e expectativa RAIA devem coexistir.** A raia cria `resposta_colab='02:00'` sem instante no `create`, depois carimba por `update` (`app/chamados/tests/test_s139.py:43-47`). Com a recusa simétrica B1, o `create` antigo seria recusado antes dos asserts `app/chamados/tests/test_s139.py:53-56`; **porém o próprio B1 corrige exatamente essa fixture**, adicionando `respondida_em=agora` (anexo B1, hunk `app/chamados/tests/test_s139.py @@ -42,7 +42,8`). Portanto não relatar como quebra do merge completo. A raia mudou o teste para esperar zero batida e erro `DECLARADO-X-GRAVADO`; manter esses asserts, que são posteriores/próximos, mas distintos do argumento adicionado ao `create` (`app/chamados/tests/test_s139.py:48-56`; hunk B1 citado).

2. **`test_medida_turno_do_par.py`: conservar os dois registros independentes.** A raia congela os quatro sítios de `juizes.py` em 53/169/274/203 (`app/core/tests/test_medida_turno_do_par.py:258-261`); o B1 muda para 54/170/275/204 por seu novo import (anexo B1, `app/chamados/juizes.py @@ -2,6 +2,7`; `app/core/tests/test_medida_turno_do_par.py @@ -255,10 +255,10`). A raia também congela `_plantio_preenche_furo_na_data` em 417 (`app/core/tests/test_medida_turno_do_par.py:270-271`). São regiões distintas: hunk B1 no início do dicionário, entrada da materialização logo depois. Sem os quatro offsets B1, falharia o assert de igualdade das linhas `app/core/tests/test_medida_turno_do_par.py:550-553`; **com ambos os registros preservados, não há quebra demonstrada**. O hunk próprio original da raia não pôde ser reconstruído [não verificado].

3. **Nascer `pergunta_respondida` não acrescenta, por si, item ao censo do turno.** O instrumento seleciona de `JUIZES` somente o destino terminado em `::turnos_do_colab` (`app/core/medida_turno_do_par.py:246-258`), inicia o fechamento apenas nele e em `ponto.turnos.parear_turnos` (`app/core/medida_turno_do_par.py:345`). Seu scanner conta formas de pareamento e extração de data (`app/core/medida_turno_do_par.py:197-228`), ausentes do corpo B1 de `pergunta_respondida` (anexo B1, `app/chamados/juizes.py @@ -1531,6 +1534,24`). Não há razão estática para aumentar os contadores por esse nascimento.

4. **Os asserts de travas BUG-PAR não congelam número de queries.** Verificam travas antes da primeira batida/escrita e ordem das quatro entidades (`app/chamados/tests/test_bug_par_projetado.py:417-428`, `app/chamados/tests/test_bug_par_projetado.py:579-589`, `app/chamados/tests/test_bug_par_projetado.py:740-743`, `app/chamados/tests/test_bug_par_projetado.py:757-759`). O SELECT simples introduzido no save B1 não muda essas relações, porque não usa `FOR UPDATE` (anexo B1, `app/chamados/models.py @@ -3526,44 +3545,40`). Quebra por teto de queries nesses testes não foi identificada.

5. **Devolução BUG-PAR-103 deve continuar preservando resposta e produzindo `DEVOLUCAO`.** A porta só limpa materialização/validação, conserva o par e registra `DEVOLUCAO` (`app/chamados/services/validacao.py:293-306`). O teste exige materialização e validação nulas, texto `'01:00'` preservado e um ato `DEVOLUCAO` (`app/chamados/tests/test_bug_par_projetado.py:523-529`). Não substituir essa chamada pela reabertura B1, que zera o par e usa `REABERTURA`: é outro ato (anexo B1, `app/chamados/models.py @@ -1609,10 +1611,16`). Seria necessário reforçar o teste para verificar também o instante preservado; hoje ele verifica somente o texto (`app/chamados/tests/test_bug_par_projetado.py:523-529`).

6. **Helpers C6 herdados: B1 já traz as dependências novas.** A raia extrai C6 sem `pergunta_respondida`/`SimpleNamespace` em `app/chamados/tests/e3_c8b_apoio.py:109-116`, `app/chamados/tests/e3_v8_apoio.py:182-187`, `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:158-161`, `app/chamados/tests/test_e4b_classe_e_memoria.py:304-309`; os hunks B1 desses quatro arquivos adicionam essas dependências. Não apresentar o `NameError` que ocorreria ao aplicar somente `juizes.py` como quebra do B1 completo (anexo B1, hunks desses arquivos). Os testes próprios BUG-PAR usam imports normais, não extração AST (`app/chamados/tests/test_bug_par_projetado.py:19-23`, `app/chamados/tests/test_bug_par_projetado.py:78-81`).

7. **Outro contrato próprio da raia continua pertinente.** As entradas de `par_do_intervalo`, irmã, contador, classe 103 e `_validar_o_par` declaram leituras de `materializada_em`/`validada_em` como portas ou identidade do ato, não vitalidade (`app/chamados/tests/test_contract_leitores_vitalidade.py:104-124`). B1 não altera esses corpos no diff fornecido; não há incompatibilidade necessária demonstrada entre essas declarações e a recusa simétrica do par [não verificado quanto à execução].

A cópia disponível não contém repositório Git utilizável; ancestral e diff próprio exato da raia não puderam ser reconstruídos [não verificado]. Nenhum teste foi executado; nenhum arquivo foi escrito [não verificado]. [nao verificado]

## 16/11 08:46 · sessao 4f345ea7519a · - · - · origem=Ronald (tmux)

A extração não contém repositório Git acessível; a autoria/base exata das mudanças da raia é [não verificado]. A comparação abaixo usa somente a árvore lida e os hunks B1 fornecidos.

| Cruzamento | O que a raia supõe e o que B1 muda | Coerência da volta |
|---|---|---|
| C6 e par projetado | `projecao_temporal_da_pergunta` corta conjuntamente instante e texto futuros, além dos carimbos de validação/materialização; `_pergunta_viva_calc` testa o par retornado, com `respondida is not None and strip(resposta)`. B1 apenas nomeia esse mesmo teste por `pergunta_respondida(SimpleNamespace(...))`. **Não muda a tabela-verdade nem deve julgar o objeto atual no lugar do par projetado.** (`app/chamados/juizes.py:1388`, `:1393`, `:1403`, `:1420`; B1 `juizes.py @@ -1417,7 +1418,9 @@`, `@@ -1531,6 +1534,24 @@`.) | Preservar chamada nova B1, `SimpleNamespace`, `texto_vazio` e o juiz nos ambientes que extraem corpos reais; manter expectativa de corte temporal do par. (`app/chamados/juizes.py:1393`, `:1420`; B1 `e3_c8b_apoio.py @@ -107,7 +107,7 @@`, `e3_v8_apoio.py @@ -182,8 +182,9 @@`, `test_e3_tarefa_admin_sem_banco.py @@ -157,8 +157,9 @@`.) |
| Vida lida pela tarefa | A tarefa recebe `viva_c6`; a exposição administrativa não inventa vitalidade nem converte `RESPONDER/colaborador` em `VALIDAR/admin`. A carga chama C6 com `agora=self.agora`. B1 conserva a semântica de C6, portanto não há inversão demonstrada de dono por nascimento do juiz. (`app/chamados/services/tarefa_do_dia.py:256`, `:260`, `:268`, `:521`, `:525`; B1 `juizes.py @@ -1417,7 +1418,9 @@`.) | Conservar `viva_c6` e o mesmo relógio; os testes que carregam C6 por AST precisam carregar também o novo juiz e `SimpleNamespace`. (`app/chamados/services/tarefa_do_dia.py:521`, `:525`; B1 hunks dos apoios citados.) |
| Decisão, devolução e ressuscitação | `_pela_decisao` já diferencia: `RESPOSTA` vazia → responder/colaborador; resposta com texto → validar/admin; `RECUSA` ou `REABERTURA` → responder/colaborador; `DEVOLUCAO` → validar/admin. B1 faz ressuscitação passar por `reabrir_para_correcao(... ato=REABERTURA)`, zerando par inteiro e guardando foto. (`app/chamados/services/tarefa_do_dia.py:148`, `:152`, `:154`; B1 `models.py @@ -1609,10 +1611,16 @@`.) | Preservar a nova REABERTURA de ressuscitação; não reutilizar essa porta para a devolução BUG-PAR ao admin. BUG-PAR conserva resposta e usa DEVOLUCAO. (`app/chamados/services/validacao.py:269`, `:272`, `:293`, `:299`, `:303`; `app/chamados/services/disputa_emissao.py:1609`, `:1631`; B1 hunk de ressuscitação.) |
| Consumidor dos atos novos | A carga lê toda sequência em ordem `(pergunta_id, ato_em, pk)`, corta atos por `agora` e passa ao corpo puro; a consulta de passivo é pela autoridade `linhas_da_projecao_sem_linha`. B1 acrescentará a REABERTURA na ressuscitação, que pode se tornar a última decisão e trocar uma antiga validação/estado sem ato por `devolvida_ao_colaborador`. (`app/chamados/services/tarefa_do_dia.py:410`, `:412`, `:419`, `:557`; `app/chamados/services/tarefa_do_dia.py:152`; B1 `models.py @@ -1609,10 +1611,16 @@`.) | Testar a sequência resultante da ressuscitação real, a foto da REABERTURA e o novo último ato; não esperar a ausência de ato ou continuar tratando a validação anterior como última decisão. (`app/chamados/services/disputa_emissao.py:1631`, `:1635`, `:1645`; B1 `test_f1nova_b1_juiz_respondida.py:260–289` no anexo.) |
| Fila e dono | A fila vem de `qs_na_fila(agora=...)`; ação admin só recebe porta se âncora está na fila. O dono colab exige pergunta pendente no app e ação RESPONDER; dono admin exige porta de ação na fila; prioridade é colab, admin, chamado, sistema. B1 não contém alteração direta nessas regras. (`app/chamados/services/tarefa_do_dia.py:142`, `:436`, `:610`, `:613`, `:622`; diff B1 fornecido.) | Não inferir `fio='admin'` apenas de uma pergunta viva ou DEVOLUCAO: precisa da fila/porta. Preservar distinção entre nova REABERTURA B1 e DEVOLUCAO BUG-PAR no teste integrado de fila. (`app/chamados/services/tarefa_do_dia.py:154`, `:610`, `:613`, `:629`.) |
| Pendente no app | A partição ainda pergunta só `respondida_em is None` e disputa aberta; a tarefa a consome antes do C6. B1 não migra esse leitor para `pergunta_respondida`, de modo que legado “instante sem texto” continua podendo ficar fora dos pendentes do app embora o juiz novo diga não respondida. (`app/chamados/services/pergunta_no_app.py:76`, `:81`; `app/chamados/services/tarefa_do_dia.py:513`, `:523`; B1 docstring `test_f1nova_b1_juiz_respondida.py:8–9` e `pergunta_respondida` no hunk `juizes.py @@ -1531,6 +1534,24 @@`.) | Não apresentar migração da partição como efeito já entregue por B1; é leitor de classe posterior e o anexo expressamente reserva B/C/D para B2. (Mesmas referências.) |

Os leitores específicos BUG-PAR têm estes cruzamentos adicionais:

- `metade_irma_do_intervalo` define a irmã “respondida” por filtro SQL de instante e teste Python de texto, com `materializada_em` nulo. É a mesma conjunção do juiz novo: a volta deve nomear `pergunta_respondida` no teste Python preservando o filtro de materialização, identidade, ambiguidade e pergunta em voo; não há mudança de universo demonstrada por essa troca. (`app/chamados/services/materializacao.py:137`, `:143`, `:145`, `:146`, `:147`; B1 `juizes.py @@ -1531,6 +1534,24 @@`.)
- `par_do_intervalo` testa somente texto da metade principal antes de procurar irmã; isso é leitor só-texto que B1 declara não migrar ainda. Alterá-lo para o juiz pode ser exigência futura de B2, mas não é quebra demonstrada por B1; fixtures que tentem nascer com texto sem instante passarão a ser recusadas. (`app/chamados/services/materializacao.py:186`, `:188`, `:190`; B1 `models.py @@ -3526,44 +3545,40 @@`, docstring de `pergunta_respondida`.)
- `_qs_recusadas_paridade_de_intervalo` já usa a face SQL `q_respondida_com_texto`, cujo corpo B1 não altera; B1 muda a autoridade documentada para o juiz novo. A expectativa de conjunto deve permanecer. (`app/chamados/services/materializacao.py:246`, `:249`; `app/chamados/juizes.py:1554`, `:1559`; B1 `juizes.py @@ -1552,7 +1573,7 @@`.)
- `paridade_absolvida_bug_par_103` e `_situacao_da_irma` repetem I∧T em Python; são pontos da volta a nomear pelo juiz, sem trocar materialização/corte/via/lâmpada ou os rótulos da partição. (`app/chamados/services/materializacao.py:274`, `:275`, `:276`, `:278`, `:305`, `:307`; B1 `juizes.py @@ -1531,6 +1534,24 @@`.)
- A DEVOLUCAO de BUG-PAR chama `linhas_da_projecao_sem_linha` antes de limpar materialização/validação, preserva resposta e grava DEVOLUCAO com texto e motivo BUG-PAR-103; `rotulo_bug_par_103` lê especificamente esse ato. Trocar por REABERTURA apagaria o par e também perderia o gatilho do rótulo, além de mudar o dono na tarefa. (`app/chamados/services/validacao.py:293`, `:294`, `:299`, `:303`, `:304`; `app/chamados/services/materializacao.py:281`, `:291`; `app/chamados/services/tarefa_do_dia.py:152`, `:154`; B1 `models.py @@ -1609,10 +1611,16 @@`.)
- A autoridade de passivo materializa atos RESPOSTA e VALIDACAO conforme os carimbos existentes; B1 passa a usá-la antes da ressuscitação e depois registra a foto na REABERTURA. A tarefa lê tanto o resultado dessa autoridade quanto a sequência e, portanto, é consumidora direta dos atos que B1 passa a produzir. (`app/chamados/models.py:4067`, `:4074`, `:4076`, `:4078`, `:4080`; `app/chamados/services/tarefa_do_dia.py:410`, `:419`; B1 `models.py @@ -1609,10 +1611,16 @@`.)

`app/docs/JUIZES.md` descreve a tarefa como decisão pela última `DecisaoPergunta`, efeito pela lâmpada e chamado pela fila, mas não registra C21 no trecho de CHAMADO; o registro atual vai de tarefa C19 a tranca C20. B1 inclui `_C21` e seu registro em `core/juizes.py`, enquanto o diff recebido exclui docs: a atualização concreta de JUIZES.md pela B1 é [não verificado]. A volta precisa reconciliar a documentação com C21, par I∧T, recusa simétrica e ressuscitação pela REABERTURA, preservando a distinção da DEVOLUCAO ao admin. (`app/docs/JUIZES.md:78`, `:80`, `:82`, `:94`, `:100`; `app/core/juizes.py:165`, `:299`, `:300`; B1 `core/juizes.py @@ -163,6 +163,11 @@`, `@@ -298,6 +303,7 @@`.)

Os testes de tarefa já distinguem os dois sentidos e não devem ser classificados como quebra automática: `Caso3MaterializacaoRecusada` espera `('materializacao_recusada', [('validar', 'admin')])` para DEVOLUCAO; a matriz E3 separa `recusa`, `reabertura` e `devolucao`, esperando razões distintas. O risco está nas fixtures/ambientes de C6 e nas expectativas de ressuscitação sem REABERTURA, não nesses asserts por si sós. (`app/chamados/tests/test_e2_tarefa_do_dia.py:225`, `:227`, `:229`; `app/chamados/tests/test_e3_portas_tarefa.py:725`, `:733`, `:734`; B1 hunks de apoios e ressuscitação citados.)

## 16/11 08:46 · sessao f62520c12708 · LOTE3-C7 · verificador · origem=Code

**O aval literal está atendido no código: as duas chamadas estão no mesmo `atomic`, com o `except` fora dele. Isso resolve a falha excepcional da trilha; não garante trilha para toda troca possível, porque permanece o descarte por igualdade dos textos dos postos.** (`app/colaboradores/views.py:333-337`; `app/colaboradores/services/historico_vinculo.py:10-11`).

**1. Escopo transacional e releitura**

- **Procedente — atomic comum.** Trecho: `with _tx.atomic(): salvar_inteiro(...); registrar_mudanca(...)`. O atomic interno de `salvar_inteiro` fica aninhado no externo; terminar a porta não confirma isoladamente o posto. Uma exceção propagada da trilha desfaz ambas as escritas antes de alcançar o `except`. (`app/colaboradores/views.py:333-337`; `app/colaboradores/services/colaborador.py:105-107`, `:152-160`).
- **Não procedente — trilha persistida apesar de falha anterior da porta.** A chamada da trilha vem depois de `salvar_inteiro`; se a guarda ou o save levantam exceção, ela não é alcançada. (`app/colaboradores/views.py:334-336`).
- **Procedente — caminho residual com posto gravado sem trilha.** Trecho: `if de == para: return None`. A comparação recebe `str` dos postos; `Posto.__str__` contém somente nome e empresa, sem identidade por PK. Dois postos distintos com mesmo nome/empresa podem trocar a FK sem produzir histórico. É preexistente, não uma falha excepcional escapando ao rollback; existência desses dados em PROD: **[nao verificado]**. (`app/colaboradores/services/historico_vinculo.py:10-11`; `app/colaboradores/views.py:335`; `app/colaboradores/models.py:220`, `:271-277`).
- **Releitura correta.** Trecho: `Colaborador.objects.select_related(...).get(pk=cid)`. Ela consulta novamente colaborador, posto, praça e empresa **depois de sair do atomic**; depois consulta escala ativa e existência de batidas com coordenadas. Não restaura o objeto atribuído em memória. (`app/colaboradores/views.py:327`, `:337-355`).
- **Limite:** essa consulta lê o estado disponível naquele momento, não um snapshot do instante anterior ao ato. Alteração por outra conexão entre rollback e releitura: **[nao verificado]**. (`app/colaboradores/views.py:333-343`).

**2. O teste distingue as versões?** [nao verificado]

**Sim, por análise estática, distingue a retirada simples do atomic externo.** O mock levanta `RuntimeError('trilha fora')` depois do save; sem atomic comum, a transação própria da porta já terminou, e a asserção que exige o posto antigo falha. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:237-243`; `app/colaboradores/views.py:334-336`; `app/colaboradores/services/colaborador.py:158-160`).

| Mutante | O teste distingue? | Fundamentação/trecho | [nao verificado]
|---|---|---|
| Remover o atomic externo, mantendo a ordem | **Sim** | `assertEqual(...posto_id, self.po.pk)` exige rollback do posto. `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:241-242`. |
| Atomic só na porta ou só na trilha, mantendo a ordem | **Sim** | A falha mockada vem depois do save já encerrado. `app/colaboradores/views.py:334-336`; `app/colaboradores/services/colaborador.py:158-160`. |
| Chamar a trilha antes do save, sem atomic comum | **Não** | O mock falha antes de qualquer escrita; o teste não exige ordem nem passagem pela porta. `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:235-247`. |
| Capturar esse `RuntimeError` dentro do atomic, sem marcar rollback | **Sim** | A saída normal preservaria o posto novo, contrariando a asserção do banco. `app/colaboradores/views.py:334`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:241`. |
| Trocar a releitura por `colaborador.posto = _posto_antes` | **Não** | Após rollback, banco e atributo restaurado mostrariam o antigo; as asserções de HTML verificam apenas os nomes. `app/colaboradores/views.py:326`, `:343`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:244-247`. |
| Apenas apagar a releitura, conservando o objeto com posto novo | **Sim** | O HTML conteria o novo nome, proibido pela asserção. `app/colaboradores/views.py:327`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:245`. |

**Lacuna procedente:** o mock substitui a função inteira antes do `INSERT`; a contagem inalterada não demonstra rollback de histórico já inserido. Também só exige a substring do erro no cabeçalho, sem validar o JSON e `tipo='erro'`. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:237-247`; `app/colaboradores/services/historico_vinculo.py:13`).

**Limite de evidência:** usa `TestCase`, horário congelado e POST HTMX com superusuário; concorrência entre conexões, commit efetivo, falha real de banco e execução dos mutantes: **[nao verificado]**. (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:43`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:193-209`).

**3. Escritores e leitores dos valores envolvidos**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `Colaborador.posto` | View atribui `colaborador.posto = posto`; porta executa save completo. `app/colaboradores/views.py:327`; `app/colaboradores/services/colaborador.py:160`. | Anterior em `app/colaboradores/views.py:326`; recusa relê em `:343`; contexto em `:350`; template mostra nome/praça em `app/templates/colaboradores/partials/vinculo_row.html:16-21`. |
| `Colaborador.situacao` | A view conserva o valor carregado; `salvar_inteiro` também o grava pelo save completo. `app/colaboradores/views.py:319`, `:324-336`; `app/colaboradores/services/colaborador.py:155-160`. | Guarda compara destino e linha gravada pelo mesmo juiz. `app/colaboradores/services/colaborador.py:63-66`, `:159`; `app/colaboradores/services/desligamento.py:32`. |
| Histórico `de/para` | View forma textos; serviço converte e limita a 200 caracteres. `app/colaboradores/views.py:335`; `app/colaboradores/services/historico_vinculo.py:14`. | Painel exibe `de → para`. `app/templates/colaboradores/partials/painel_vinculo.html:62`. |
| Histórico `posto` | View passa explicitamente o destino; serviço persiste. `app/colaboradores/views.py:336`; `app/colaboradores/services/historico_vinculo.py:16`. | Alternativa a `para` no painel; cadastro e fio também exibem. `app/templates/colaboradores/partials/painel_vinculo.html:62`; `app/templates/colaboradores/editar.html:187`; `app/templates/chamados/fio_colaborador.html:91`. |
| Histórico `motivo` | Literal `'transferencia'`, persistido pelo serviço. `app/colaboradores/views.py:336`; `app/colaboradores/services/historico_vinculo.py:17`. | `get_motivo_display` no cadastro e fio. `app/templates/colaboradores/editar.html:187`; `app/templates/chamados/fio_colaborador.html:90`. |
| Histórico `data` | View lê `timezone.localdate()`; serviço repassa. `app/colaboradores/views.py:335`; `app/colaboradores/services/historico_vinculo.py:15`. | Painel/cadastro/fio exibem; modelo e resumo ordenam por ela. `app/templates/colaboradores/partials/painel_vinculo.html:60`; `app/templates/colaboradores/editar.html:187`; `app/templates/chamados/fio_colaborador.html:89`; `app/colaboradores/models.py:589`; `app/inteligencia/resumo.py:51-52`. |
| Histórico `registrado_por` | `request.user`; serviço aceita autor com PK, senão `None`. `app/colaboradores/views.py:336`; `app/colaboradores/services/historico_vinculo.py:12`, `:17`. | Painel mostra username ou `"sistema"`. `app/templates/colaboradores/partials/painel_vinculo.html:64`. |
| Linha HTML | View monta contexto e renderiza após releitura. `app/colaboradores/views.py:343-357`. | Template lê posto, escala e indicadores; formulário solicita troca da linha com `outerHTML`. `app/templates/colaboradores/partials/vinculo_row.html:16-36`; `app/templates/colaboradores/partials/painel_vinculo.html:112-115`. |
| Toast | View escreve `HX-Trigger`, `msg=f'Erro: {e}'`, `tipo='erro'`. `app/colaboradores/views.py:358`. | `_dialogo.html` lê cabeçalho/JSON, chama `showToast` e insere mensagem por `textContent`. `app/templates/core/_dialogo.html:79-85`, `:26-38`. |

**O sucesso HTMX retorna somente a linha, sem escrever esse toast; o sucesso não HTMX passa pela resposta de ação.** (`app/colaboradores/views.py:375-377`).

**4. Erro engolido dentro do novo atomic**

**Não procedente no encadeamento examinado.** `Colaborador.save` não captura exceções; `HistoricoVinculo` não possui override de save; `registrar_mudanca` faz o `create` sem captura. (`app/colaboradores/models.py:415-439`, `:548-592`; `app/colaboradores/services/historico_vinculo.py:7-17`).

O receiver pertinente de `Colaborador` registra `_silenciar` em `transaction.on_commit`; o `engolir` fica **dentro do callback**, após confirmação da transação envolvente, e não durante as duas escritas. O callback do registro de trava apenas retorna `None`. (`app/chamados/signals.py:270-281`; `app/chamados/services/trava_do_colaborador.py:22-23`, `:47`).

`engolir` isoladamente não abre savepoint; `evento`, porém, envolve seu `INSERT` em `transaction.atomic()`. Esse `evento` não aparece no save/trilha examinados. (`app/core/observ.py:85-91`, `:105`, `:133-138`; `app/colaboradores/models.py:415-439`; `app/colaboradores/services/historico_vinculo.py:7-17`).

**5. Varredura das classes**

| Classe | Juízo e trecho |
|---|---|
| Segundo juiz duplicando o registrado | **Não procedente neste caminho.** `if esta_desligado(gravada)` usa o juiz registrado. `app/colaboradores/services/colaborador.py:62-66`; `app/core/juizes.py:271`. |
| Segundo escritor fora da porta | **Não procedente no diff.** São chamadas as duas portas declaradas, sem save/create direto na view. `app/colaboradores/views.py:334-336`; `app/core/portas.py:450`, `:461`. |
| Relógio fora da borda | **Não procedente.** `timezone.localdate()` está na view; o serviço recebe `data`. `app/colaboradores/views.py:335`; `app/colaboradores/services/historico_vinculo.py:7`, `:15`. |
| Derivação paralela | **Procedente como passivo preexistente de apresentação:** `sem_posto` é calculado nos dois ramos e repetido no painel; o registro já aponta esse pendente. `app/colaboradores/views.py:346`, `:365`; `app/templates/colaboradores/partials/painel_vinculo.html:34`; `app/core/juizes.py:931-932`. |
| Selo/RED sem mordida | **Não procedente para a retirada simples do atomic; procedente a insuficiência para provar forma e releitura.** Trechos: mock da função inteira e asserções finais. `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:237-247`. Execuções alegadas nos anexos: **[nao verificado]**. |
| Ordem e duração de trava | **Não procedente como inversão nova; duração ampliada.** A porta continua travando Colaborador primeiro, mas a trava agora cobre também a trilha. `app/colaboradores/services/colaborador.py:84-91`, `:158-160`; `app/chamados/models.py:2743-2746`; `app/colaboradores/views.py:333-336`. |
| Trava adicional condicional | **Procedente, preexistente:** matrícula ausente faz o override travar também o colaborador da maior matrícula, depois do alvo. Deadlock ou ocorrência em PROD: **[nao verificado]**. `app/colaboradores/models.py:427-438`. |
| Pouso antes do pré-requisito | **Não procedente neste ato:** guarda/trava precedem save; trilha segue a porta; exceção desfaz o conjunto. `app/colaboradores/services/colaborador.py:158-160`; `app/colaboradores/views.py:333-337`. |
| Campo com dois significados | **Procedente, preexistente:** aqui `data` é dia civil local da execução; na escala recebe `data_inicio`; no desligamento agendado recebe data futura de demissão. `app/colaboradores/views.py:335`; `app/colaboradores/services/vinculo.py:354-357`; `app/colaboradores/services/desligamento.py:113-116`. O painel chama essa coluna “Quando” e separa “Vigência”. `app/templates/colaboradores/partials/painel_vinculo.html:56-63`. |

**Outra ressalva preexistente:** `_posto_antes` é capturado antes da trava; a guarda reconsulta situação, sem atualizar esse anterior. Por inferência estática, uma troca concorrente pode deixar o texto `de` obsoleto mesmo com escrita e trilha atômicas. Ocorrência real: **[nao verificado]**. (`app/colaboradores/views.py:319`, `:326`, `:333`; `app/colaboradores/services/colaborador.py:65-66`, `:84-91`).

**6. Escopo literal**

**Não procedente, considerando o diff fornecido.** As mudanças são a delimitação transacional, o comentário da releitura e a inversão/fortalecimento do teste. Permissão, mensagens e caminhos de resposta permanecem no código apresentado. (`app/colaboradores/views.py:313-316`, `:328-342`, `:357-377`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:230-247`).

Comparação independente com `26caf921f..HEAD`, identidade Git do hash e resultados RED/verde/mutante/ruff: **[nao verificado]**; a cópia disponibilizada não contém repositório Git, e não executei testes nem acessei banco/logs. **[nao verificado]**

## 16/11 08:45 · sessao 39fd6a3f0a9c · - · - · origem=Ronald (tmux)

Leitura da raia e cruzamento com os hunks B1 fornecidos:

- **Limite de autoria:** a árvore não contém repositório Git utilizável; `git status/log/merge-base` retornaram “not a git repository”. Não reconstruí o diff próprio da raia nem os hunks que ela alterou versus ancestral. [não verificado]

- **C6 deve conservar o par projetado.** A raia projeta `respondida_em` **junto com** `resposta_colab`: resposta posterior a `agora` vira `(None, '')`; também projeta validação e materialização (`app/chamados/juizes.py:1388-1399`). Hoje `_pergunta_viva_calc` consulta `respondida is not None and (resposta or '').strip()` (`app/chamados/juizes.py:1420`). B1 substitui isso por `pergunta_respondida(SimpleNamespace(respondida_em=respondida, resposta_colab=resposta))`, hunk `juizes.py @@ -1417,7 +1418,9`. A volta precisa manter a projeção e consultar o juiz sobre os valores projetados; usar `pergunta_respondida(p)` diretamente perderia o corte temporal. (`app/chamados/juizes.py:1393`; anexo B1, hunk citado.)

- **Leitores novos BUG-PAR que ainda nomeiam “respondida” por fórmula própria.** A irmã usa carimbo no filtro e texto no Python (`app/chamados/services/materializacao.py:143-146`); o predicado BUG-PAR-103 usa instante E texto (`:275`); a situação da irmã repete instante E texto (`:307`). São equivalentes ao juiz novo nos pares esperados, mas a volta deveria chamar `pergunta_respondida`/sua face SQL nesses três lugares para acompanhar a nomeação da classe A em B1 (`juizes.py @@ -1531,6 +1534,24`). `_qs_recusadas_paridade_de_intervalo` já usa `q_respondida_com_texto`, portanto já acompanha a face SQL nomeada em B1 (`materializacao.py:247-250`; B1 `juizes.py @@ -1552,7 +1573,7`).

- **Leitores somente de texto exigem distinção de escopo.** `par_do_intervalo` barra somente texto vazio da metade principal (`materializacao.py:188-189`); `_recusa_e_de_paridade` também consulta texto (`:260-262`); `_materializar_resposta_hora` exige texto como anuência (`:1236`). `validar_pergunta` verifica só o instante (`app/chamados/services/validacao.py:521-523`). B1 declara que classes B/C/D ficam para B2 no docstring novo do juiz (`anexo B1, juizes.py @@ -1531,6 +1534,24`). Portanto registrar essas dependências na volta; não afirmar que B1 já migrou esses leitores nem que necessariamente passam a quebrar. (`materializacao.py:188`; `validacao.py:521`; hunk B1 citado.)

- **Fio continua com leitor anterior.** `espera_o_texto_do_colaborador` retorna instante ausente OU `respondida_sem_texto` (`app/chamados/services/fio.py:177-187`), e a montagem publica seu inverso como `resposta_enviada` (`:354-357`). A alteração B1 em `fio.py @@ -1592,17 +1592,25` é na porta de escrita renomeada para `recusa_par_ilegal`, com duas recusas. O novo bloco BUG-PAR no fio é o tratamento de testemunha indisponível (`:372-379`), que não chama a porta antiga. Não identifiquei chamada BUG-PAR direta que precise trocar esse nome. (`fio.py:376-379`; B1 hunk citado.)

- **Devolução BUG-PAR não equivale à ressuscitação B1.** BUG-PAR-103 lê a projeção anterior, descarimba materialização e validação, preserva o par da resposta e grava `DecisaoPergunta.DEVOLUCAO` com texto (`app/chamados/services/validacao.py:289-306`). B1 troca a ressuscitação por `reabrir_para_correcao(... ato=REABERTURA)`, zerando o par inteiro e mantendo a foto no ato (`models.py @@ -1609,10 +1611,16`). A volta deve preservar a distinção entre esses atos: converter a devolução BUG-PAR em reabertura alteraria texto e dono. (`validacao.py:303-306`; B1 hunk citado.)

- **Dono e fila da tarefa tornam essa distinção observável.** `RESPOSTA` com texto deixa VALIDAR/admin; `RECUSA`/`REABERTURA` deixa RESPONDER/colaborador; `DEVOLUCAO` deixa VALIDAR/admin (`app/chamados/services/tarefa_do_dia.py:148-155`). `porta_admin` só existe quando a âncora está na fila (`:140-142`). A carga lê atos ordenados por pergunta, instante e PK, corta por `agora`, e calcula passivo pela autoridade existente (`:410-420`); lê a fila por `qs_na_fila(... agora=agora)` (`:434-437`). Assim, expectativa de ressuscitação “sem ato novo” deve mudar para REABERTURA/colaborador; expectativa BUG-PAR continua DEVOLUCAO/admin, dependente da fila. (`tarefa_do_dia.py:152-155`; hunk B1 `models.py @@ -1609,10 +1611,16`.)

- **Escritores BUG-PAR que alimentam leituras preservadas/nomeadas pela B1.** `_validar_o_par` valida a irmã pela porta canônica e chama `_materializar_uma`, exigindo as duas materializadas (`app/chamados/services/validacao.py:487-491`); `_carimbar_validacao` grava validação e ato VALIDACAO com o texto (`:161-169`). `_materializar_uma` chama `p.carimbar(... resposta='validar')` (`app/chamados/services/materializacao.py:1812-1813`), cujo escritor central atribui `materializada_em` (`app/chamados/models.py:3180-3184`) e só completa instante da resposta se há texto (`:3213-3216`). B1 passa a julgar o par persistido no `save`, incluindo a metade do banco omitida em `update_fields` (`models.py @@ -3526,44 +3545,40`). Esses caminhos completos/guardados são compatíveis em leitura; execução pós-merge não verificada. (`models.py:3213-3216`; hunk B1 citado; [não verificado] para execução.)

- **Materialização não é campo novo lido pela B1.** O audit I21 já filtrava `materializada_em__isnull=True`; B1 altera o reconhecimento da resposta para `q_respondida_com_texto`, retirando a aceitação de somente brancos (`app/chamados/management/commands/auditar_invariantes_chamados.py:1572-1577`; anexo B1 `@@ -1569,10 +1569,12`). A volta deve manter materialização/validação coerentes ao devolver, sem atribuir à B1 uma nova leitura desse campo. (`validacao.py:294-299`; hunk citado.)

- **Leitores BUG-PAR dos campos que B1 passa a limpar/gravar:** irmã exige materialização nula (`materializacao.py:145`), BUG-PAR-103 exige materialização anterior ao corte e par respondido (`:275-278`), `_situacao_da_irma` distingue materializada de pendente e respondida (`:305-309`), rótulo da devolução lê especificamente ato DEVOLUCAO e motivo BUG-PAR-103 (`:281-294`). A nova REABERTURA B1 não deve ganhar esse rótulo; a irmã ressuscitada com par vazio não deve continuar classificada como respondida. (`materializacao.py:291-294`; `:307`; B1 `models.py @@ -1609,10 +1611,16`.)

- **Docs:** `app/docs/JUIZES.md:78-93` documenta tarefa por última decisão e fila; `:94-98` documenta C6/tranca. A lista pula para cadastro/conversa em `:99-100`, sem entrada do juiz novo. B1 anexa registro `_C21` em `core/juizes.py @@ -163,6 +163,11` e `@@ -298,6 +303,7`, mas o diff fornecido exclui docs. Coerência da volta: documentar o juiz novo e manter a distinção REABERTURA/DEVOLUCAO; alteração B1 real em JUIZES.md é [não verificado].

- **Asserts BUG-PAR que precisam ser preservados, não “corrigidos” automaticamente:** a devolução deixa texto `'01:00'` (`app/chamados/tests/test_bug_par_projetado.py:526`), grava exatamente um DEVOLUCAO (`:527-529`), move universo de carimbada para não carimbada (`:534`) e pode validar o par sem nova resposta dessa metade (`:542-544`). B1, pelos hunks fornecidos, não demonstra quebra desses asserts; eles quebrariam se a volta confundisse devolução com a nova reabertura. (`:526-529`; B1 `models.py @@ -1609,10 +1611,16`.)

- **Fixtures próprias BUG-PAR verificadas:** a pergunta avulsa nasce com texto+instante (`test_bug_par_projetado.py:253-254`); a semeadura de recusa grava texto+instante juntos (`:308-310`). Não são exemplos de gravação de texto sem instante que a B1 recusaria. (`test_bug_par_projetado.py:253-254`; `:308-310`.)

## 16/11 08:45 · sessao 35bad7614b29 · - · - · origem=Ronald (tmux)

Itens 2/3 — revisão da árvore:

- O bloco J1 foi retirado do fluxo atual: a atualização da Justificativa termina em `j.status='pendente'` e `j.save(...)`, seguindo diretamente para 5b; não há chamada de invalidação nesse bloco (`app/ponto/services/regularizacao.py:102-118`). Igualdade byte a byte com `f8ee770a5`: `[nao verificado]`, pois a cópia não dispõe do repositório Git/base.
- Permanecem referências J1 de caracterização, sem exigir o comportamento retirado: docstring do módulo (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:20-22`), classe/docstring/nome do teste (`:266-272`), mensagens das asserções (`:286-288`). O selo estrutural monitora exclusivamente a porta de Ausencia e não depende de J1 (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:35`, `:241-288`). Auditoria de **todo o diff** base..c7: `[nao verificado]`.
- O teste J1 exige retorno OK, status pendente, zero chamadas de `invalidar_fechamento` e carimbo preservado (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:280-288`). Execução verde em base/c7: `[nao verificado]`; anexos relatam o resultado, mas logs e base não estão disponíveis.
- Esse teste não exercita nenhuma Ausencia intermediária: o relógio é 05/11 (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:56`), turno 04/11 (`:275-278`), e o primeiro dia iterado é hoje, portanto o laço termina antes de `_garantir_ausencia` (`app/ponto/services/regularizacao.py:111-118`).

Tabela do caminho novo LOTE7 visível:

| Sítio | Banco / invalidação |
|---|---|
| Entrada API → serviço | Invoca o serviço; leitura do colaborador pode ocorrer (`app/api/views_core.py:1154-1157`, `:1179-1186`). |
| `regularizar_turno_orfao` → `garantir_ausencia` | O serviço inteiro **lê e escreve**; grava Justificativa (`app/ponto/services/regularizacao.py:91-106`), chama porta de Ausencia (`:118-122`), altera chamados (`:130-141`) e grava auditoria (`:150-154`). Não é um serviço somente leitura. |
| `garantir_ausencia` → `criar_ausencia` | Delegação sem consulta própria (`app/ponto/services/ausencia.py:602-611`). |
| Criação nova: `criar_ausencia:597` | A função já escreveu Ausencia (`app/ponto/services/ausencia.py:562`) e trilha (`:585-593`). Novo gancho lê o insumo em 1 SELECT (`:138-139`). A falta nasce aguardando (`:475-487`, `:926-944`), fora da folha (`app/ponto/turnos.py:2528-2529`), portanto passa `{},{}` ao acordador. |
| Reabertura: `criar_ausencia:433` → `_reabrir:1319` → `mudar_estado` | `_reabrir` pode gravar descrição (`app/ponto/services/ausencia.py:1315-1318`); `mudar_estado` grava estado (`:988`) e trilha (`:995-998`). Gancho acrescenta 2 SELECT de insumo (`:987`, `:993`). Rejeitada → aguardando fica fora da folha dos dois lados (`:1297-1298`, `:926-944`; `app/ponto/turnos.py:2528-2529`). |
| Linha existente devolvida | Retorna antes do gancho novo (`app/ponto/services/ausencia.py:427`, `:456`). |
| `_insumo_da_folha` | Apenas SELECT (`app/ponto/services/ausencia.py:138-139`), sem invalidação. |
| `_acordar_projecao` | Com `{},{}`, não entra no laço e retorna soma vazia; zero relógio, zero leitura de fechamento e zero invalidação (`app/ponto/services/ausencia.py:183-195`). Genericamente a função **pode** chamar `invalidar_fechamento` em `:195`; não ocorre nos estados acima. |
| `_teto_da_linha_aberta` / `invalidar_fechamento` | Genericamente leem fechamento (`app/ponto/services/ausencia.py:147-151`) e leem/escrevem fechamento (`app/ponto/services/fechamento.py:1648-1656`), mas não são executados pelo novo gancho nesses fluxos (`app/ponto/services/ausencia.py:183-195`). |

A seção Kernel da entrega confere quanto às referências `regularizacao.py:118`, `ausencia.py:602/610/597/433/1319/987/993/427/456`, ao filtro `turnos.py:2528-2529` e aos custos **adicionais** de 1/2 SELECT (`app/ponto/services/ausencia.py:138-139`). “Zero sítios de invalidação real” é defensável como resultado dos estados produzidos pela entrada; seria falso se interpretado como ausência de chamada no grafo sintático (`app/ponto/services/ausencia.py:195`).

Chequei também os signals: Justificativa e Ausencia acordam cartório (`app/ponto/signals.py:143-147`, `:161-165`, `:85-93`). O cartório carimba/lavra a célula (`app/ponto/services/cartorio.py:1314-1317`, `:1873-1875`); `carimbar` chama `salvar_e_emitir`, que grava Celula/EventoCelula (`app/ponto/portas/celula.py:1181-1202`, `:957-965`). Isso **não** chama `_o_chao_do_dia_mudou`: a recalculação por evento está nesse outro rabo, chamado por regenerar/editar/reverter (`app/ponto/portas/celula.py:392`, `:665`, `:858`, `:483-496`). Assim, não há a suposta ponte imediata `cartorio → carimbar → recalcular_por_evento` nesses caminhos (`app/ponto/portas/celula.py:1181-1202`, `:957-965`).

## 16/11 08:45 · sessao fe3c3b27458d · - · - · origem=Ronald (tmux)

Conclusões para itens 6/7, somente por leitura:

- **Não identifiquei segundo juiz de ausência no acordador:** `_insumo_da_folha` chama `_pelo_efeito(...,'folha')`, sem repetir o literal de status (`app/ponto/services/ausencia.py:126`, `:138`); o juiz filtra aprovada em `app/ponto/turnos.py:2523`, `:2528`. Os dias vêm de `dias_da_ausencia` (`ausencia.py:180`, `:192`), cuja regra está em `turnos.py:2798`; autoridade registrada em `app/core/juizes.py:255`, `:257` e `app/docs/JUIZES.md:55`. Ausência de duplicação **no diff inteiro**: `[nao verificado]`, pois a cópia não tem Git/base.
- **Não há segundo escritor fora da porta nos caminhos inspecionados:** a rejeição especial escreve dentro de `app/ponto/services/ausencia.py:1495`/`:1497`; é exceção ao “status só por mudar_estado” da docstring (`:16`), mas permanece na porta. O gate da família varre árvore e exige porta (`app/ponto/tests/test_chokepoint_ausencia_gate.py:53`), com allowlist vazia (`:25`); não executei esse gate `[nao verificado]`.
- **Relógio continua fora da borda e com leitura desnecessária em dia único:** `_acordar_projecao` chama `_tz_ac.localdate()` para toda linha mudada com `data_fim=None` (`app/ponto/services/ausencia.py:190`/`:191`). Porém `dias_da_ausencia` só prolonga sem fim para `TIPOS_EM_ABERTO`; os outros tipos ocupam um dia (`app/ponto/turnos.py:2803`–`:2808`). Também lê `FechamentoMensal` nesse caso (`ausencia.py:147`–`:151`). Portanto E do laudo continua, sem alterar o resultado do juiz dos dias (`ausencia.py:192`).
- **D do laudo continua: atomic + engolir sem savepoint do cartório.** `rejeitar_ausencias_de_ferias` abre atomic (`app/ponto/services/ausencia.py:1483`), salva cada ausência (`:1497`), o receiver chama `_julgar_dias` (`app/ponto/signals.py:143`–`:147`), que chama cartório sob `engolir`, sem atomic/savepoint próprio (`:85`–`:93`; `app/core/observ.py:85`–`:91`). Uma exceção de banco marcada para rollback pode ser engolida e envenenar o ato externo; manifestação runtime não reproduzida `[nao verificado]`. **Não atribuir isso a `evento`: ele já tem savepoint próprio** (`app/core/observ.py:124`–`:138`); o consumidor da ausência também é diferente, pois roda via `on_commit` (`app/ponto/signals.py:254`, `:272`).
- **Falha da invalidação pode deixar selo falso que repetição não cura:** `invalidar_fechamento` protege suas operações com savepoints (`app/ponto/services/fechamento.py:1648`–`:1656`), engole falha e retorna zero (`:1661`–`:1665`); `_acordar_projecao` ignora insumos iguais (`app/ponto/services/ausencia.py:183`–`:185`). O contador só encontra `derivado_em=NULL` (`fechamento.py:1695`/`:1696`), portanto não descobre carimbo antigo que não chegou a ser apagado. Esta é a ressalva já declarada no laudo, não defeito novo demonstrado de c7 `[nao verificado]`.
- **Selo estrutural tem mordida, mas seu alcance é limitado:** lê somente `PORTA=ausencia.py` (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:35`, `:291`–`:293`), cobra chamada sintaticamente viva do acordador (`:222`–`:224`, `:287`) e possui mutantes de remoção/inclusão (`:333`–`:369`). A verificação `_acorda=False` também recebe só o fonte da porta (`:227`–`:238`), e não todos os chamadores. Um mutante que mantenha uma chamada viva porém entregue `{},{}` pode satisfazer esse selo estrutural; resultados da suíte comportamental nesse mutante: `[nao verificado]`.
- **Travas:** cancelamento trava agendamento → período (`app/ferias/services.py:629`–`:640`) e então chama rejeição (`:679`–`:683`); esta trava ausências em PK e só a tabela self (`app/ponto/services/ausencia.py:1484`–`:1489`). Backfill também trava agendamentos antes das ausências (`ausencia.py:1672`–`:1679`). Não demonstrei inversão nessas cadeias; deadlock/concorrência global `[nao verificado]`.
- **`derivado_em` conserva um significado nestes leitores/escritores:** carimbo de derivação no modelo (`app/ponto/models.py:464`), preenchido ao derivar (`app/ponto/services/fechamento.py:1143`, `:1189`), limpo ao invalidar (`:1656`), NULL lido como vencida (`:1679`–`:1681`, `:1695`–`:1696`).
- **`CelulaDia.ausencia` é cobertura de cobrança, não necessariamente aprovada/folha:** cartório pergunta `ausencia_cobre(..., para='cobranca')` (`app/ponto/services/cartorio.py:1228`, `:1356`), passa esse objeto para `carimbar` (`:1314`–`:1316`, `:1873`), e a porta grava `ausencia`, `julgada_em`, `insumos_em` juntos (`app/ponto/portas/celula.py:1194`–`:1201`). A fixture férias aprovada satisfaz ambos efeitos; a nova asserção `ausencia=None` após rejeição concorda com `cobranca` excluir rejeitada (`app/ponto/turnos.py:2530`–`:2531`).
- **`julgadas` e `carimbadas` são contadores diferentes:** `julgadas` sobe depois de classificar (`app/ponto/services/cartorio.py:1464`–`:1476`); `carimbadas` depois de gravar no ramo normal (`:1873`–`:1875`) e no ramo sem adesão (`:1314`–`:1317`). A soma no novo teste aceita qualquer um; não prova que o ramo com adesão foi exercitado. A nova comprovação persistida de `julgada_em` e ausência removida é o que impede retorno “positivo” sem carimbo/lavra (teste `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:462`–`:465`).
- **Comentário contraditório remanescente:** `_decidir_no_ato` ainda afirma “competência futura não tem projeção gravada para vencer” (`app/ponto/services/ausencia.py:1119`), enquanto o próprio acordador explica e trata exatamente a projeção futura gravada (`:168`–`:172`, `:147`–`:151`). É dívida documental verificável; autoria no intervalo c6..c7 `[nao verificado]`.

## 16/11 08:44 · sessao b100c0cd63b6 · - · - · origem=Ronald (tmux)

Q3 — mapa de escrita e leitura:

| Campo/valor | Escritor | Leitor |
|---|---|---|
| `Colaborador.posto` | Atribuição em `app/colaboradores/views.py:327`; persistência por `salvar_inteiro` em `app/colaboradores/services/colaborador.py:158-160` e `super().save` em `app/colaboradores/models.py:439`. | Anterior em `views.py:326`; releitura de recusa em `views.py:343`; `posto_atual` em `views.py:350/369`; nome/praça na linha em `app/templates/colaboradores/partials/vinculo_row.html:16-21`. |
| `Colaborador.situacao` | O `save()` completo também a grava: `app/colaboradores/services/colaborador.py:155-160`; não há atribuição de novo valor em `vincular_posto`: `app/colaboradores/views.py:324-336`. | Guarda em `services/colaborador.py:159`, leitura da linha gravada em `:65-66`; predicado único em `app/colaboradores/services/desligamento.py:23-32`, registrado em `app/core/juizes.py:271`. |
| Histórico `de/para` | View forma strings em `views.py:335`; serviço normaliza/trunca para 200 caracteres em `app/colaboradores/services/historico_vinculo.py:14`. | `app/templates/colaboradores/partials/painel_vinculo.html:62`; representação textual do modelo em `app/colaboradores/models.py:592`. |
| Histórico `posto` | Argumento explícito `posto=posto` em `views.py:336`, persistido em `services/historico_vinculo.py:16`. | Painel usa como alternativa a `para` em `painel_vinculo.html:62`; `editar.html:187`; `app/templates/chamados/fio_colaborador.html:91`. |
| Histórico `motivo` | Literal `'transferencia'` em `views.py:336`, persistido em `services/historico_vinculo.py:17`. | `app/templates/colaboradores/editar.html:187` e `app/templates/chamados/fio_colaborador.html:90` usam `get_motivo_display`. |
| Histórico `data` | `timezone.localdate()` em `views.py:335`; repasse em `services/historico_vinculo.py:15`. | `painel_vinculo.html:60`; `editar.html:187`; `fio_colaborador.html:89`; ordenação do modelo em `models.py:589` e da consulta em `app/inteligencia/resumo.py:51-52`. |
| Histórico `registrado_por` | `request.user` em `views.py:336`, reduzido a usuário com `pk` em `services/historico_vinculo.py:12`, gravado em `:17`. | `painel_vinculo.html:64` exibe `username` ou `"sistema"`; carregamento relacionado em `views.py:306`. |
| HTML de recusa | Contexto da releitura em `views.py:343-357`. | Template lê `posto_atual`, `sem_posto`, `sem_escala` em `vinculo_row.html:16-36`. |
| Toast | `HX-Trigger` com `msg=f'Erro: {e}'`, `tipo='erro'` em `views.py:358`. | `app/templates/core/_dialogo.html:79-85` lê o cabeçalho, JSON e campos; `:26-38` escolhe cores e usa `textContent`; incluído na base em `app/templates/base.html:388`. |

Q5 — classificação dos pontos atribuídos:

- **Segundo juiz: não procedente no trecho alterado.** A view chama a porta em `app/colaboradores/views.py:334`; a guarda delega ao juiz em `app/colaboradores/services/colaborador.py:62-66`; o juiz registrado é `app/core/juizes.py:271`. Trecho: `if esta_desligado(gravada):`.
- **Segundo escritor fora da porta: não procedente no trecho alterado.** Portas declaradas em `app/core/portas.py:450/461`; view usa `salvar_inteiro` e `registrar_mudanca` em `app/colaboradores/views.py:334-336`. Trecho do escritor histórico: `return HistoricoVinculo.objects.create(...)`, `services/historico_vinculo.py:13-17`.
- **Relógio fora da borda: não procedente nesta chamada.** A leitura está na view, `app/colaboradores/views.py:335`; o serviço recebe `data` e a repassa sem relógio em `app/colaboradores/services/historico_vinculo.py:7/15`. O relógio é lido depois de `salvar_inteiro`, mas antes da inserção da trilha: `views.py:334-336`.
- **Derivação paralela: não procedente como novidade do diff.** `de/para` são apresentação textual em `app/colaboradores/services/historico_vinculo.py:14`, exibida em `painel_vinculo.html:62`; o código rejeita usar essa trilha como autoridade de posto no dia em `app/colaboradores/services/vinculo.py:935-938`. Trecho: `"quem estava em P no dia D" NAO tem autoridade na casa`.
- **Data com dois sentidos: procedente, preexistente.** Aqui é o dia civil local da execução (`views.py:335`); no agendamento de desligamento é a data futura de desligamento (`app/colaboradores/services/desligamento.py:113-116`); na escala é `data_inicio`, também passada como `vigencia_ini` (`app/colaboradores/services/vinculo.py:354-358`). O painel rotula `data` como “Quando”, mas separa “Vigencia” (`painel_vinculo.html:56/60/63`). Uso produtivo de `HistoricoVinculo.data` como autoridade para calcular vigência: **[não verificado]**.
- **Anterior relido antes da trava: procedente como possibilidade estática, preexistente.** A view carrega o colaborador em `views.py:319` e captura `_posto_antes` em `:326`; a trava só chega em `services/colaborador.py:159 → :90`. Portanto a trilha pode receber um anterior obsoleto se houver troca concorrente entre essas etapas; ocorrência em produção **[não verificado]**.
- **Postos homônimos sem trilha: procedente, preexistente.** `Posto.__str__` usa somente `nome` e `empresa` em `app/colaboradores/models.py:276-277`; o serviço faz `if de == para: return None` em `app/colaboradores/services/historico_vinculo.py:10-11`. Dois IDs com a mesma representação textual podem mudar `posto_id` sem criar linha; existência desses registros em produção **[não verificado]**.

## 16/11 08:44 · sessao a8aeb70aa95b · - · - · origem=Ronald (tmux)

1. O teste c7 não importa a classe nova: importa apenas a função em `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:235`. Considerando o comportamento da base informado pelo usuário — `update()` rejeita sem trilha — ele falha por **asserção na linha 243**, mensagem na 244, pois existem linhas rejeitadas; execução/base local `[nao verificado]`.

Na c7, captura o `RuntimeError` em `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:241`, verifica ausência de rejeição persistida em `:243`, presença da exceção em `:245` e nome concreto em `:246`. O caminho implementado é atomic `app/ponto/services/ausencia.py:1483` → save `:1497` → evento retorna `None` `:1498` → exceção `:1505`; sair com exceção desfaz o atomic. A classe herda de `RuntimeError` em `app/ponto/services/ausencia.py:122`; verde executado `[nao verificado]`.

- Outra `RuntimeError`, antes de gravar: falha no nome em `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:246`.
- Exceção depois de confirmar rejeições: falha na persistência em `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`.
- Nem levantar nem rejeitar: falha em `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:245`.
- Outra exceção sem herdar de `RuntimeError`: escapa do handler `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:241`.
- **Sobrevive** levantar `TrilhaDaRejeicaoNaoGravada` antes de tentar escrever: as três verificações passam, pois não exigem tentativa de escrita nem retrato das linhas anteriores (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`, `:245`, `:246`). É conclusão estática; mutação executada `[nao verificado]`.

4. O teste da célula agora nasce com FK para férias em `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:446`, chama juiz real em `:451`, soma contadores em `:453`, exige cobertura dos cinco dias em `:457`, cinco rejeições em `:459`, cinco timestamps em `:461` e nenhuma FK remanescente em `:462`.

Isso morde trocar `save()` por `update()` sem signal, juiz no-op/contador zero e carimbo que mantém a FK: o `save` atual está em `app/ponto/services/ausencia.py:1497`, o receiver em `app/ponto/signals.py:143` chama o julgamento em `:147`, e a chamada efetiva está em `:93`. Mutantes executados `[nao verificado]`.

**Ainda passa neste teste** quebrar o ramo de colaboradores com adesão: a fixture não cria batidas (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:64`, `:194`), e o ramo sem batida termina após incrementar `carimbadas` (`app/ponto/services/cartorio.py:1317`, `:1318`), antes do incremento normal `julgadas` em `:1476`. Também passa lavrar veredito/ata incorretos mantendo contador, timestamp e FK nula: o teste não consulta esses campos (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:457`, `:461`, `:462`), enquanto `carimbar` escreve timestamp/FK junto dos campos recebidos em `lavrados` (`app/ponto/portas/celula.py:1194`, `:1196`, `:1200`).

`_nada_mudou` agora morde alteração de status/obs, exclusão/substituição de Ausência, mudança de status/data e alteração/remoção de trilha antiga nos campos selecionados: retrato em `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:510`, `:511`, `:512`; comparação em `:533`, `:534`, `:535`; saldo em `:531`.

**Ainda passa** alterar `Ausencia.minutos/data_fim/descricao`, `AgendamentoFerias.dias/datas` ou campos não fotografados de auditoria; o retrato seleciona somente as tuplas declaradas (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:510`, `:511`, `:512`). Inserir trilha nova indevida também passa, pois a comparação projeta somente as PKs antigas (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:535`). Portanto “INTEIRO” da docstring significa os campos enumerados, não linhas completas (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:505`).

## 16/11 08:44 · sessao 1d773139ce22 · - · - · origem=Ronald (tmux)

Q2 — **Procedente: o teste morde a retirada simples do atomic; não prova toda a forma da cura.** [nao verificado]

- O teste injeta `RuntimeError('trilha fora')` substituindo **a função inteira** `registrar_mudanca`, depois verifica posto antigo no banco, contagem da trilha inalterada, HTML antigo e erro no toast. Trecho: `side_effect=RuntimeError('trilha fora')` — `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:237`; asserções: linhas 240–247.
- Na view, `salvar_inteiro` antecede `registrar_mudanca` — `app/colaboradores/views.py:334` e `:335`. Sem atomic comum, a porta possui apenas seu atomic próprio, terminado antes da chamada da trilha: `with _transacao_da_guarda(True)` / `colab.save()` — `app/colaboradores/services/colaborador.py:158` e `:160`. Por inferência estática, o posto novo sobreviveria e a asserção da linha 241 falharia.
- **Mutante que passa:** substituir a consulta de releitura por `colaborador.posto = _posto_antes`. O rollback já devolveu o banco ao posto antigo; as asserções só exigem os nomes antigo/novo e o toast, sem exigir consulta ou dados frescos — `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:241`–`:247`; releitura atual em `app/colaboradores/views.py:343`. Apenas eliminar a consulta, sem restaurar o atributo em memória, falharia no HTML — atribuição do posto novo em `app/colaboradores/views.py:327`.
- **Mutante que passa:** chamar `registrar_mudanca` antes de `salvar_inteiro`, sem atomic comum. A função mockada falha antes de qualquer escrita; todas as asserções finais continuam satisfeitas. Não existe asserção de ordem ou de chamada da porta no trecho do teste — `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:235`–`:247`.
- **Mutante que falha:** mover o `except` para dentro do atomic e capturar esse `RuntimeError` sem marcar rollback. O save anterior permaneceria, contrariando a asserção do banco — `app/colaboradores/views.py:334`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:241`. Inferência estática; não executei mutantes.
- A contagem inalterada não prova rollback de um INSERT de histórico já realizado: o mock impede a entrada em `registrar_mudanca`, cujo INSERT fica em `app/colaboradores/services/historico_vinculo.py:13` — mock em `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:237`.
- O teste usa `TestCase`, horário congelado e requisição HTMX autenticada com superusuário — `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:43`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:193`, `:200`, `:204`–`:209`. Concorrência real, commit efetivo e falha real de banco: **[não verificado]**.

Q6 — **Não procedente, considerando o diff fornecido:** as mudanças estão na delimitação transacional/comentário da view e na inversão do teste — `app/colaboradores/views.py:328`–`:342`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:230`–`:247`. Comparação independente `26caf921f..HEAD`: **[não verificado]**, cópia sem repositório Git.

Alerta para Q1/Q5 — **Procedente como ressalva preexistente:** existe caminho de troca real de FK sem trilha, por igualdade dos textos. `registrar_mudanca` executa `if de == para: return None` — `app/colaboradores/services/historico_vinculo.py:10`. A view passa `str(_posto_antes)` e `str(posto)` — `app/colaboradores/views.py:335`; `Posto.__str__` retorna `nome — empresa` — `app/colaboradores/models.py:276`. `nome` não é único e o `Meta` só ordena por nome — `app/colaboradores/models.py:220`, `:271`–`:274`. Assim, postos de PKs distintos com mesmo nome/empresa podem mudar o cadastro sem inserir histórico; existência desses dados em PROD: **[não verificado]**.

## 16/11 08:44 · sessao 011080a54dd1 · - · - · origem=Ronald (tmux)

Censo enviado nos blocos C1–C6 e T1, incluindo acessos diretos, filtros ORM, hidratação explícita, reversas, `getattr`, administração e templates.

Conclusões principais para consolidar:

- **Geofence do registro:** `Batida.posto_para_calculo` devolve `posto_efetivo or Colaborador.posto`; `verificar_geofence` usa esse posto para raio e coordenadas. `app/ponto/models.py:100`; `app/ponto/services/geofence.py:258`; `app/ponto/services/geofence.py:259`; `app/ponto/services/geofence.py:284`.
- **Feriado:** o posto vem do vínculo do dia e o município vem desse posto; municipal/estadual usam município/UF. `app/core/feriados.py:131`; `app/core/feriados.py:132`; `app/core/feriados.py:161`; `app/core/feriados.py:62`; `app/core/feriados.py:65`.
- **Apuração:** espelho e fechamento alimentam feriados pela composição anterior; a praça da régua CCT vem do vínculo do dia. `app/ponto/services/espelho.py:1029`; `app/ponto/services/fechamento.py:648`; `app/core/regua_cct.py:329`; `app/core/regua_cct.py:330`.
- **Leitores dos dois campos com preferência pelo vínculo:** cabeçalho do espelho, drawer, retrato da folha, pessoas do holerite e relatório diário da frota. `app/ponto/services/espelho.py:207`; `app/colaboradores/services/drawer.py:35`; `app/folha/views_retrato.py:115`; `app/holerite/views.py:310`; `app/relatorios/dia_da_frota.py:56`.
- **Frontend da batida:** também lê diretamente coordenadas e raio de `Colaborador.posto`. `app/templates/ponto/bater_ponto_v2.html:139`; `app/templates/ponto/bater_ponto_v2.html:140`.

Correções finais dos blocos: `app/core/busca.py:104` pertence a `parecidos`; `app/core/busca.py:127` pertence a `qs_colaborador`. As chamadas de `posto_geo` estão em `app/api/views.py:351` e `app/api/views_core.py:339`. Ambos kernels têm entrada de URLs na cópia: `app/api/urls.py:2`; `app/api/urls_core.py:5`.

## 16/11 08:44 · sessao 99223b10b130 · - · - · origem=Ronald (tmux)

- **Cura transacional procedente:** `with _tx.atomic()` contém `salvar_inteiro` e `registrar_mudanca`; o `except` está depois do bloco. Erro propagado da trilha desfaz também o posto antes da releitura. Trechos: `app/colaboradores/views.py:333-343`.
- **Atomic interno:** `salvar_inteiro` abre `_transacao_da_guarda(True)`, chama guarda e `colab.save()`. A guarda trava a linha e lê a situação gravada; o atomic externo mantém essa trava também durante a trilha. Trechos: `app/colaboradores/services/colaborador.py:105-107`, `:152-160`, `:84-91`; trava real em `app/chamados/models.py:2743-2746`.
- **Releitura:** o objeto recebeu posto novo antes da porta; o `except` consulta novamente `Colaborador` com posto/praça/empresa e então calcula escala, geolocalização e contexto da linha. Não restaura objeto nem usa snapshot. Trechos: `app/colaboradores/views.py:326-327`, `:343-358`. Outro escritor entre rollback e releitura: [não verificado].
- **Assimetria residual procedente, preexistente:** `registrar_mudanca` faz `if de == para: return None` (`app/colaboradores/services/historico_vinculo.py:10-11`). A view passa nomes formatados, não IDs (`app/colaboradores/views.py:335`). `Posto.__str__` usa nome e empresa (`app/colaboradores/models.py:276-277`), e `nome` não tem `unique` (`:220`, Meta `:271-274`). Dois IDs distintos com mesma representação podem permitir troca gravada sem linha de histórico; não é falha excepcional da trilha. Ocorrência em PROD: [não verificado].
- **Trilha com `de` antigo sob concorrência, limite procedente:** colaborador e `_posto_antes` são lidos antes do atomic (`app/colaboradores/views.py:319`, `:326`, `:333`). A guarda reconsulta situação, não o posto anterior (`app/colaboradores/services/colaborador.py:65-66`, `:84-91`). Inferência estática: `de` pode não representar o posto imediatamente anterior ao UPDATE concorrente; ocorrência em PROD: [não verificado].
- **Erro engolido que envenena esse atomic: não encontrado:** override de `Colaborador.save` não captura exceções (`app/colaboradores/models.py:415-439`); `HistoricoVinculo` não tem override (`:548-592`); escritor da trilha não captura exceções (`app/colaboradores/services/historico_vinculo.py:7-17`). Único receiver encontrado para `Colaborador` registra callback com `transaction.on_commit`; o `engolir` está dentro do callback (`app/chamados/signals.py:270-281`). Logo esse best-effort roda após o commit externo, e não antes do INSERT da trilha.
- **Outro callback sem risco de banco:** registro da trava usa `conn.on_commit(_TravaDoColaborador(cid))`, cujo `__call__` apenas retorna `None` (`app/chamados/services/trava_do_colaborador.py:22-23`, `:37-47`).
- **`evento` já possui savepoint:** `engolir` captura exceções sem abrir atomic (`app/core/observ.py:85-91`), mas `evento` envolve o INSERT em `with transaction.atomic()` (`:105`, `:133-138`). Não é chamado diretamente pelo save/trilha examinados (`app/colaboradores/models.py:415-439`; `app/colaboradores/services/historico_vinculo.py:7-17`).
- **Ordem das travas:** novo bloco não acrescenta trava anterior ao Colaborador; começa pela porta existente (`app/colaboradores/views.py:333-334`). A duração aumenta até terminar a trilha (`:335-336`). Caso preexistente: ausência de `matricula_hasner` faz `save` também travar outro Colaborador, o da maior matrícula, depois do alvo (`app/colaboradores/models.py:427-438`); deadlock/ocorrência em PROD: [não verificado].
- **Segundo juiz: não procedente neste caminho:** guarda chama `esta_desligado` (`app/colaboradores/services/colaborador.py:62-66`), mesmo juiz registrado em `app/core/juizes.py:271`, implementado em `app/colaboradores/services/desligamento.py:23-32`.
- **Relógio fora da borda: não procedente:** `timezone.localdate()` fica na própria view (`app/colaboradores/views.py:335`). Código configura São Paulo/USE_TZ (`app/config/settings/base.py:93-95`); configuração efetiva de PROD: [não verificado].
- **Pré-requisito:** guarda/trava precedem o save (`app/colaboradores/services/colaborador.py:158-160`), e a trilha depende do sucesso da porta (`app/colaboradores/views.py:334-336`); falta de pré-requisito nesse encadeamento: não procedente.

## 16/11 08:43 · sessao f3b4c1f2ea3c · COMPLEMENTO-LAUDO-[nome]-1611 · leitura · origem=Code

**(a) Há escolhas de dias diferentes no mesmo caminho:** a guarda `_plantio_preenche_furo_na_data` consulta **22/10, dia civil local do plantio**; a derivação do horário e o juiz do lastro partem de **21/10, `data_evento` da pergunta**. O primeiro sítio é precisamente o corte de 19/10, “véspera não absolve”. (`app/chamados/services/materializacao.py:157-182`; `app/chamados/services/materializacao.py:347-350`; `app/chamados/juizes.py:274-282`.)

Os números e horários do caso são premissas do dado fornecido; os valores efetivos de `celula_id`, DNA, ATA, vínculos e demais batidas dessas perguntas não foram fornecidos. **[nao verificado]**

Há um limite da cópia: o materializador existente trabalha por pergunta; a composição futura por `par_do_intervalo`/`_gravar_irma_do_par`, mencionada no brief, não está disponível para certificar seus guardas e consumidores adicionais. O caminho implementado chama `_materializar_uma` para cada pergunta; a implementação futura do par permanece **[nao verificado]**. (`app/chamados/services/materializacao.py:1381-1383`.)

Na tabela, “coincide” significa coincidir com a origem **21/10** indicada na pergunta; a correspondência efetiva entre `p.celula.data` e `p.data_evento` neste caso precisa ser medida. **[nao verificado]**

| Guarda/juiz do caminho | De onde tira a data ou célula; decisão para o caso | Coincide com a origem? |
|---|---|---|
| **`_plantio_preenche_furo_na_data`** | Recebe somente colaborador e `ts`; calcula `localtime(ts)` e filtra `CelulaDia.data=loc.date()`: **22/10**. Usa a ATA dessa célula, `dia_sem_trabalho`, lâmpadas e órfãs para chamar `plantio_preenche_furo`. Não recebe a pergunta. (`app/chamados/services/materializacao.py:143-182`.) | **Não. É o sítio do corte de 19/10**, documentado literalmente antes do filtro. (`app/chamados/services/materializacao.py:161-171`.) |
| **Implausibilidade na validação** | Compara a resposta com `chamado_destino.contexto_json.marcos_faltantes`; sem marco, usa `p.data_evento`, **21/10**, com o template da escala ativa. O destino efetivo e seus marcos neste caso: **[nao verificado]**. (`app/chamados/services/validacao.py:429-432`; `app/chamados/juizes.py:2022-2042`; `app/chamados/juizes.py:1853-1873`.) | No fallback, coincide pela data; a leitura usa escala ativa, não diretamente a FK da pergunta. (`app/chamados/juizes.py:1853-1862`.) |
| **`DATA_EM_DISPUTA`** | Compara `p.data_evento` com `dia_do_fato` do chamado que lista a pergunta. Esse juiz prioriza a célula do chamado antes do contexto. Havendo divergência sem `data_conferida_em`, recusa; não escolhe silenciosamente um dia. (`app/chamados/services/fio.py:77-88`; `app/chamados/juizes.py:98-121`; `app/chamados/services/materializacao.py:1197-1220`.) | Depende do chamado efetivo e da conferência: **[nao verificado]**. |
| **Anuência, escape e crivo do relógio** | Exige texto do colaborador; rejeita resposta de escape. Para os defaults `00:00`/`07:00`, consulta a régua pela data derivada de `p.data_evento`, com fallback `disputa.contexto[motivo]['data']`. (`app/chamados/services/materializacao.py:835-847`; `app/chamados/services/materializacao.py:345-381`.) | A régua parte da origem, **21/10**. (`app/chamados/services/materializacao.py:347-350`; `app/chamados/services/materializacao.py:374-375`.) |
| **Tipo, marco fixo e ajuste overnight** | O tipo prioriza o marco do destino; a segunda fonte é a ATA por `p.data_evento`. Para intervalo fixo, a hora vem do marco cobrado. Combina essa hora com **21/10** e consulta vínculo/marcos de **21/10**; o ajuste pode devolver um instante em **22/10** dentro da cadeia noturna. (`app/chamados/services/materializacao.py:275-285`; `app/chamados/services/materializacao.py:409-427`; `app/chamados/juizes.py:319-334`.) | **Sim na origem consultada**; o timestamp resultante pode pertencer ao civil seguinte. (`app/chamados/juizes.py:320-332`.) |
| **S137: reuso da batida** | Procura colaborador/tipo em janela absoluta de **±20 minutos de `ts`**. Não escolhe célula. (`app/chamados/services/materializacao.py:865`; `app/chamados/juizes.py:238-250`.) | Não se aplica: decide por instante/tipo. (`app/chamados/juizes.py:244-250`.) |
| **Vizinha do mesmo tipo / paridade** | Examina a vizinha cronológica anterior ou posterior, até **12 horas de `ts`**, sem filtro por célula. A absolvição posicional chama o primeiro helper da tabela, portanto **22/10**. A testemunha de apresentação usa o mesmo par de verificações. (`app/chamados/services/materializacao.py:86-107`; `app/chamados/services/materializacao.py:881-883`; `app/chamados/services/materializacao.py:458-459`.) | A vizinhança não escolhe célula; a absolvição consulta **22/10**, divergindo da origem. (`app/chamados/services/materializacao.py:171`; `app/chamados/services/materializacao.py:882-883`.) |
| **Lâmpada que autoriza encerrar por recusa de paridade** | Se a guarda anterior barrar, consulta `fato_do_marco_no_espelho(p)`. Esse juiz lê **`p.celula`**, seu DNA e sua ATA; somente `True` permite carimbar `paridade_recusada`. (`app/chamados/services/materializacao.py:906-930`; `app/chamados/juizes.py:944-974`.) | Usa a célula carregada pela pergunta, portanto origem **se a FK for a informada**; identidade efetiva: **[nao verificado]**. |
| **Dedup da porta de batida** | Antes de F6, procura batida viva do mesmo tipo em **±60 segundos**, depois a chave exata colaborador/timestamp. Não consulta célula. (`app/ponto/registro_batida.py:107-122`.) | Não se aplica. (`app/ponto/registro_batida.py:107-122`.) |
| **F6 `competencia_fechada`** | Como o materializador passa `fato_de_chao=False`, consulta F6. Ele usa `localtime(timestamp).date()`: **22/10**, deriva a competência e lê `FechamentoMensal.status`. (`app/chamados/services/materializacao.py:933-940`; `app/ponto/registro_batida.py:132-137`; `app/ponto/janelas.py:84-88`.) | **Não pela data-base**. Isso não prova competências diferentes: depende do corte da empresa. (`app/ponto/janelas.py:24-33`; `app/ponto/janelas.py:84-85`.) |
| **Colisão com tipo oposto** | `_janela_oposto` consulta vínculo/marcos do civil **22/10** para escolher 60/300 segundos. A exceção `_casa_marco_proprio` procura marco do mesmo tipo no civil **22/10 e na véspera 21/10**. (`app/ponto/registro_batida.py:21-50`; `app/ponto/registro_batida.py:138-151`.) | O primeiro usa 22; a exceção admite 21. **Esse sítio não é o corte de 19/10.** (`app/ponto/registro_batida.py:28-48`; `app/chamados/services/materializacao.py:161-171`.) |
| **Juiz pós-plantio `_pergunta_materializada_de_fato`** | Deriva o slot pela mesma `_ts_tipo_do_plantio`; procura batidas vinculadas à pergunta e não retratadas. Para hora, aceita a janela de **±20 minutos** na hora esperada em `data_evento` ou `data_evento+1`: **21 ou 22/10**. Sem essa prova, há fallback de batida viva no slot ou retratação auditada. Reprovação provoca rollback. (`app/chamados/juizes.py:374-377`; `app/chamados/juizes.py:274-282`; `app/chamados/juizes.py:755-764`; `app/chamados/juizes.py:815-818`; `app/chamados/services/materializacao.py:1238-1245`.) | **Sim como âncora**, admitindo a cauda noturna em 22. (`app/chamados/juizes.py:274-282`.) |
| **Carimbo/C6 e lavra** | `carimbar` consulta `pergunta_viva` com o carimbo aplicado. No caminho já validado, C6 retorna imediatamente “não viva” por `validada_em`, antes de consultar célula. A lavra volta a chamar o juiz do lastro e calcula expiração por `p.data_evento`. (`app/chamados/models.py:3212-3235`; `app/chamados/juizes.py:1402-1405`; `app/chamados/veredito_lavrado.py:254-275`.) | C6, nesse ramo, **não decide por célula**; a lavra mantém a origem. (`app/chamados/juizes.py:1403-1405`; `app/chamados/veredito_lavrado.py:273`.) |
| **Devolução, reconciliação e fecho** | A validação chama a devolução após a unidade; o destino retém por irmãs respondidas sem consumo; a disputa fecha por C7, ausência de respostas sem veredito e de fio vivo. Esses testes não são `dia_perfeito`. (`app/chamados/services/validacao.py:486-494`; `app/chamados/services/materializacao.py:764-777`; `app/chamados/models.py:1905-1917`.) | O fecho opera sobre perguntas/chamados. Quando consulta tranca, usa o dia da pergunta, `data_evento`. (`app/chamados/models.py:1905-1917`; `app/chamados/juizes.py:994-995`.) |

Há ainda dois percursos relevantes após ou ao redor do plantio:

- **Evento da batida:** o sinal pede julgamento tanto da véspera quanto do civil da batida — aqui **21 e 22/10** — e depois reconcilia os chamados. Julgar 21 por esse sinal não altera o filtro exclusivo de 22 da guarda do corte. (`app/ponto/signals.py:85-93`; `app/ponto/signals.py:108-112`; `app/ponto/signals.py:132-135`; `app/ponto/signals.py:207-215`; `app/chamados/services/materializacao.py:171`.)
- **Passivo:** `_plantar_o_dia` mede geometria antes e depois do grupo, por `turnos_sem_par`, em janela de ±2 dias; recusa se piorar. A competência do passivo parte de `p.data_evento`, usando o civil do timestamp somente como fallback. (`app/chamados/services/passivo_validado.py:98-113`; `app/chamados/services/passivo_validado.py:518-543`; `app/chamados/services/materializacao.py:217-222`.)

**Retratação precisa ser distinguida.** A retratação de perguntas pela célula usa `p.celula`, C6 e tranca; seu universo exclui perguntas já validadas ou materializadas. O retratador de batidas `par_relampago` possui guarda de procedência que recusa retratar marca cuja origem começa por `disputa`; esse retratador usa `_score_geometria`, não `simular_dia.dia_perfeito`. (`app/chamados/reconciliador.py:628-668`; `app/ponto/management/commands/detectar_par_relampago.py:185-189`; `app/ponto/management/commands/detectar_par_relampago.py:129-151`.)

Quanto ao **K6 proposto no brief**, `competencia_entregue` recebe empresa/mês/ano e não escolhe célula; quem chama precisa derivar a competência. O contrato futuro de devolução por origem, apresentado no anexo, não pode ser certificado como implementado nesta leitura. **[nao verificado]** O juiz existente verifica exportação vigente não invalidada, enquanto F6 consulta `FechamentoMensal`. (`app/folha/export.py:556`; `app/folha/export.py:580-594`; `app/ponto/janelas.py:86-88`.)

**(b) `grade_dia` devolve `(marcos, dd)`; `dia_perfeito` devolve um booleano e não busca batidas.** Portanto, primeiro é necessário especificar a lista passada à grade e o estado consultado por ela. (`app/ponto/services/simular_dia.py:5-23`.)

Para a origem **21/10**, o caminho por turno funciona assim:

1. `grade_dia(..., data=21/10)` chama `grade_espelho_janela` com início=fim=21/10 e `dias=1`, extraindo a primeira linha. O dispatcher escolhe o builder por `GRADE_POR_TURNO`; seu default configurado é verdadeiro, mas o ambiente efetivo do caso é **[nao verificado]**. (`app/ponto/services/simular_dia.py:5-8`; `app/escala/utils.py:2337-2338`; `app/escala/utils.py:2084-2090`; `app/config/settings/base.py:165`.)

2. Se o argumento contém **E21/10 18:32, S22/10 01:00, E22/10 02:00 e S22/10 06:30**, as quatro entram em `locs`: o filtro inicial aceita E/S, converte para hora local, ordena e deduplica mesmo tipo consecutivo a menos de dez minutos. **Não filtra `date()==21/10`.** (`app/escala/utils.py:1322-1340`.)

3. O builder examina a origem e sua véspera; busca vínculo, DNA e pausas, e encadeia os marcos em instantes que atravessam a meia-noite. Assim, a linha de **21/10 pode consumir as três marcas civis de 22/10**. (`app/escala/utils.py:1409-1412`; `app/escala/utils.py:1598-1619`; `app/escala/utils.py:1701-1708`.)

4. Para intervalo declarado, tenta alinhar pelo papel das batidas no único turno fechado de `data_turno=21/10`. Nesse caminho, as bordas são entrada/saída e o miolo pode ser o par S01/E02. Se o papel não puder ser transcrito, segue o alinhamento por distância e suas guardas. O reconhecimento efetivo desse turno/par no caso é **[nao verificado]**. (`app/escala/utils.py:1581-1596`; `app/escala/utils.py:437-474`; `app/escala/utils.py:585-665`.)

5. **A grade também lê o banco por dentro:** as chamadas a `turnos_do_colab` não recebem a lista simulada; carregam batidas não retratadas na janela com pad e leem o papel da ATA. Por isso, inserir apenas duas marcas em uma lista em memória não reproduz necessariamente o estado após materializá-las. (`app/escala/utils.py:1581`; `app/escala/utils.py:1898`; `app/ponto/turnos.py:1867-1871`; `app/ponto/turnos.py:1944`.)

6. A linha devolvida contém células e órfãs, recebendo também overlays. Uma marca não consumida vira órfã pelo seu **dia civil**; se essa sobra for de 22 e só houver linha de 21, ela não é acrescentada às órfãs dessa linha. (`app/escala/utils.py:1807-1811`; `app/escala/utils.py:1816-1834`; `app/escala/utils.py:1844-1850`.)

Sobre esse retorno, `dia_perfeito` faz exatamente:

- `False` se `tipo_dia != 'trabalho'` ou houver órfãs;
- ignora posições `None`;
- `False` para qualquer célula `missing`;
- `False` quando `tipo_real` existir e divergir de `marcos[i]['tipo']`;
- `True` se nenhuma dessas condições reprovar. Não exige `status=='ok'` nem testa o delta. (`app/ponto/services/simular_dia.py:11-23`.)

Logo, **com quatro posições E/S/E/S preenchidas, dia `trabalho`, sem órfãs e sem divergência de tipo, retorna `True`**. Entretanto, o retorno concreto do caso é **[nao verificado]**: o dado não fornece DNA, ATA, tipo de intervalo, overlays, demais batidas e flag efetiva. Há inclusive um ramo de intervalo livre com colunas `I` preservando `tipo_real=S/E`, capaz de reprovar a comparação literal apesar do preenchimento. (`app/ponto/services/simular_dia.py:11-23`; `app/escala/utils.py:1383-1390`; `app/escala/utils.py:1724-1729`.)

**Os consumidores implementados desses dois retornos são:**

| Consumidor | Uso |
|---|---|
| `veredito_celula(..., veredito='trabalhou')` | Carrega `[21/10 00:00, 23/10 00:00)`, sem filtro de retratação nessa consulta; testa se já está perfeito e avalia combinações de flips/preenchimentos. (`app/ponto/services/veredito_celula.py:235-245`; `app/ponto/services/veredito_celula.py:294-305`.) |
| `flip_auto.avaliar_fila` | Carrega a mesma janela de dois dias, calcula perfeição antes/depois dos flips e entrega `p0/p1` a `veredito_flip_auto`. (`app/ponto/services/flip_auto.py:290-305`.) |

O caminho atual da validação/materialização consulta **lastro, C6 e C7** nos pontos descritos; não há nele consumidor certificado desses dois helpers para aprovar conjuntamente as metades. O consumidor futuro do par proposto permanece **[nao verificado]**. (`app/chamados/services/materializacao.py:1238-1287`; `app/chamados/services/materializacao.py:1319-1322`; `app/chamados/models.py:1911-1917`.)

**Escritores e leitores dos campos de data, no caminho examinado:**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `PerguntaDisputa.data_evento` | Atribuição em `gravar_dia_do_fato`; nascimento, costura e rechaveamento passam por essa porta. Para marco, a derivação pergunta pelo dia da célula. (`app/chamados/services/fio.py:829-840`; `app/chamados/models.py:3096-3100`; `app/chamados/services/fio.py:915-918`; `app/chamados/services/fio.py:1076-1082`; `app/chamados/services/fio.py:1682-1692`.) | Comparação de datas, derivação do plantio, lastro e competência do passivo. (`app/chamados/services/fio.py:87-88`; `app/chamados/services/materializacao.py:347-350`; `app/chamados/juizes.py:274`; `app/chamados/services/passivo_validado.py:113`.) |
| `PerguntaDisputa.celula` | Nascimento busca pelo dia do fato e associa a FK; reuso e rechaveamento também a escrevem. (`app/chamados/models.py:2976-2979`; `app/chamados/models.py:3047-3050`; `app/chamados/models.py:3097`; `app/chamados/services/fio.py:1073-1082`.) | Lâmpada e C6 leem a FK; alimentação da lei só a reutiliza quando data e colaborador coincidem. (`app/chamados/juizes.py:947-974`; `app/chamados/juizes.py:1417-1453`; `app/chamados/juizes.py:654-662`.) |
| Data da batida plantada | É o **`timestamp`**: o materializador deriva/passa `ts`, e `registrar_batida` cria a batida. `data_turno` pertence ao turno derivado. (`app/ponto/models.py:23`; `app/chamados/services/materializacao.py:419-427`; `app/chamados/services/materializacao.py:933-940`; `app/ponto/registro_batida.py:152-161`; `app/ponto/turnos.py:323-324`.) | Guarda do corte, F6, dedup/colisão e grade. (`app/chamados/services/materializacao.py:157-171`; `app/ponto/janelas.py:84-88`; `app/ponto/registro_batida.py:107-150`; `app/escala/utils.py:1323`.) |
| `Turno.data_turno` | `_mk` chama `_data_do_turno`; `recompute_turnos` persiste o resultado em `TurnoMaterializado`. (`app/ponto/turnos.py:248-309`; `app/ponto/turnos.py:312-324`; `app/ponto/nucleo.py:47-59`.) | Filtro final dos turnos e seleção do turno da grade/origem da pergunta. (`app/ponto/turnos.py:1997-2000`; `app/escala/utils.py:1581-1596`; `app/chamados/juizes.py:401-403`.) |
| `Chamado.contexto_json['data_turno']` | Há emissores distintos: grade, alerta de turno e reemissão. Escritor do chamado concreto: **[nao verificado]**. (`app/ponto/management/commands/reconciliar_grade.py:507-519`; `app/ponto/management/commands/processar_alertas_turno.py:335-352`; `app/chamados/reconciliador.py:691`.) | `dia_do_fato`, depois da célula e da chave do catálogo; seu resultado alimenta `data_do_fato_de`. (`app/chamados/juizes.py:98-121`; `app/chamados/services/fio.py:84-88`.) |

**Para confirmar na sombra**, a medição deve usar o plantio real das duas metades dentro de `atomic+raise`, com o mesmo instante de julgamento; lista simulada sozinha é insuficiente pelas leituras internas de turnos. Execução dessa medição: **[nao verificado]**. (`app/escala/utils.py:1581`; `app/escala/utils.py:1898`; `app/ponto/turnos.py:1867-1871`.)

Deve registrar, antes, entre as metades e depois:

- Perguntas: `pk`, `motivo`, `data_evento`, `celula_id/celula.data`, destino/contexto, resposta, conferência e carimbos; chamar `data_do_fato_de`, `_ts_tipo_do_plantio` e `estado_aplicacao_pergunta`. (`app/chamados/services/fio.py:40-94`; `app/chamados/services/materializacao.py:333-465`.)
- Células de 20/21/22: DNA, ATA, lâmpadas, órfãs, `trabalha` e veredito; comparar separadamente `_plantio_preenche_furo_na_data` e `fato_do_marco_no_espelho`. (`app/chamados/services/materializacao.py:171-182`; `app/chamados/juizes.py:933-974`; `app/escala/utils.py:1409-1412`.)
- Batidas: `pk`, `timestamp`, `tipo`, `origem`, `is_intervalo`, aprovação, `retratada_em` e `pergunta_origem_id`; turnos: `data_turno`, bordas, batidas, abertura e intervalos. (`app/chamados/services/materializacao.py:933-956`; `app/ponto/services/simular_dia.py:32`; `app/ponto/turnos.py:312-324`; `app/escala/utils.py:437-474`.)
- Retornos integrais de `grade_dia(...,21/10,agora=...)` e `dia_perfeito`, flag efetiva, `_pergunta_materializada_de_fato`, C6/C7 e F6; para a pergunta de competência entregue, derivar a competência pela origem e chamar K6 separadamente. (`app/ponto/services/simular_dia.py:5-23`; `app/escala/utils.py:2084-2090`; `app/chamados/services/materializacao.py:1238-1287`; `app/chamados/models.py:1911-1917`; `app/ponto/janelas.py:75-89`; `app/chamados/services/passivo_validado.py:86-113`; `app/folha/export.py:556-594`.)

## 16/11 08:43 · sessao d0f2ab38a165 · - · - · origem=Ronald (tmux)

Censo dos escritores na cópia, distinguindo alteração explícita de regravação por `save()` completo.

| Campo | Função / sítio | Escrita | Trilha no caminho | Escreve outro campo no mesmo ato? |
|---|---|---|---|---|
| `Colaborador.posto` | `vincular_posto` | Atribui `posto`, chama `salvar_inteiro`: `app/colaboradores/views.py:327`, `app/colaboradores/views.py:329` | `registrar_mudanca(... tipo='posto')`: `app/colaboradores/views.py:331`; criação de `HistoricoVinculo`: `app/colaboradores/services/historico_vinculo.py:13`. Se `de == para`, não cria histórico: `app/colaboradores/services/historico_vinculo.py:10`. | Não grava `EscalaColaborador.posto` no corpo do ato: `app/colaboradores/views.py:325`. |
| `Colaborador.posto` | `posto_add_colab` | `gravar(... {'posto': posto})`: `app/colaboradores/views.py:645` | `registrar_evento_de_vinculo`, motivo `vinculo`, no mesmo `atomic`: `app/colaboradores/views.py:643`, `app/colaboradores/views.py:647`; cria `HistoricoVinculo`: `app/colaboradores/services/historico_vinculo.py:25`. | Não grava EC.posto: `app/colaboradores/views.py:643`. |
| `Colaborador.posto` | `posto_remove_colab` | `gravar(... {'posto': None})`: `app/colaboradores/views.py:670` | `registrar_evento_de_vinculo`, motivo `desvinculo`, no mesmo `atomic`: `app/colaboradores/views.py:668`, `app/colaboradores/views.py:672`. | Não grava EC.posto: `app/colaboradores/views.py:668`. |
| `Colaborador.posto` | `admitir_colaborador` | Criação com `posto_id`: `app/colaboradores/views.py:1185`, `app/colaboradores/views.py:1190`; porta `Colaborador.objects.create(**campos)`: `app/colaboradores/services/colaborador.py:93`. | Quando há `posto_id`, cria evento de admissão: `app/colaboradores/views.py:1219`, `app/colaboradores/views.py:1221`. | Não cria EC nesse caminho: `app/colaboradores/views.py:1185`, `app/colaboradores/views.py:1229`. |
| `Colaborador.posto` | `transferir_posto` | Atribui novo posto e empresa; `salvar_inteiro`: `app/colaboradores/views.py:1388`, `app/colaboradores/views.py:1389`, `app/colaboradores/views.py:1391`. | Evento `transferencia`, no mesmo `atomic`: `app/colaboradores/views.py:1387`, `app/colaboradores/views.py:1396`. | Não grava EC.posto: `app/colaboradores/views.py:1387`. |
| `Colaborador.posto` — regravação | `editar_colaborador` | Chama `salvar_inteiro`, que executa `colab.save()` sem `update_fields`: `app/colaboradores/views.py:1345`, `app/colaboradores/services/colaborador.py:76`. A edição não atribui posto: `app/colaboradores/views.py:1314`, `app/colaboradores/views.py:1341`. | Não há chamada de trilha nesse trecho de persistência e retorno: `app/colaboradores/views.py:1343`, `app/colaboradores/views.py:1355`. | Não grava EC.posto nesse trecho: `app/colaboradores/views.py:1343`. |
| `Colaborador.posto` — regravação | `executar_desligamento`, ramo efetivo | Altera situação/demissão/motivo e salva inteiro: `app/colaboradores/services/desligamento.py:129`, `app/colaboradores/services/desligamento.py:133`. | Evento de desligamento: `app/colaboradores/services/desligamento.py:139`; `registrar_log` com situação/demissão: `app/colaboradores/services/desligamento.py:229`. | Fecha vínculos por `fechar_vinculos_ativos`, sem escrever EC.posto: `app/colaboradores/services/desligamento.py:147`. Chamadores incluem view e comando: `app/colaboradores/views.py:1437`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`. |
| `Colaborador.posto` — regravação | `executar_reativacao` | Altera situação/demissão/motivo e salva inteiro: `app/colaboradores/services/desligamento.py:242`, `app/colaboradores/services/desligamento.py:246`. | Evento `reativacao`: `app/colaboradores/services/desligamento.py:251`. | Não escreve EC.posto nem reativa escala nessa função: `app/colaboradores/services/desligamento.py:235`. |
| Persistência delegada de `Colaborador` | `onboarding_salvar_selfie` | `colab.foto.save(..., save=True)`: `app/core/views.py:1061`; `foto` é `ImageField`: `app/colaboradores/models.py:355`. Implementação interna de `FieldFile.save` e extensão exata da regravação: **[nao verificado]**, ausente na cópia consultada. | Nenhuma chamada de trilha no bloco: `app/core/views.py:1048`, `app/core/views.py:1062`. | Nenhuma escrita explícita de EC.posto no bloco: `app/core/views.py:1048`. |
| `EscalaColaborador.posto` | `executar_vinculo`, existente | Copia `colaborador.posto`, `save()` completo: `app/colaboradores/services/vinculo.py:275`, `app/colaboradores/services/vinculo.py:283`. | `HistoricoVinculo` quando ação não é `no_op`: `app/colaboradores/services/vinculo.py:349`, `app/colaboradores/services/vinculo.py:354`; `registrar_log('editar')` com diferenças, incluindo posto na foto: `app/colaboradores/services/vinculo.py:36`, `app/colaboradores/services/vinculo.py:371`, `app/colaboradores/services/vinculo.py:376`. | Lê C.posto para escrever EC.posto; não escreve C.posto: `app/colaboradores/services/vinculo.py:275`. |
| `EscalaColaborador.posto` | `executar_vinculo`, novo | Cria copiando `colaborador.posto`: `app/colaboradores/services/vinculo.py:287`, `app/colaboradores/services/vinculo.py:289`. | Mesmo `HistoricoVinculo` acima; log `criar` com foto do vínculo: `app/colaboradores/services/vinculo.py:354`, `app/colaboradores/services/vinculo.py:381`. | Não escreve C.posto: `app/colaboradores/services/vinculo.py:287`. |
| `EscalaColaborador.posto` | `criar_trecho_retroativo` | Cria vínculo inativo copiando `ec.posto`: `app/colaboradores/services/vinculo.py:970`, `app/colaboradores/services/vinculo.py:971`. | Chamador `corrigir_escala_retroativa --novo_trecho` registra log `criar`; foto não inclui posto: `app/colaboradores/management/commands/corrigir_escala_retroativa.py:196`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:198`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:201`. | Não escreve C.posto: `app/colaboradores/services/vinculo.py:970`. |
| `EscalaColaborador.posto` | `ajustar_vinculo_pelo_sistema` | Cria vínculo copiando `ec.posto`: `app/colaboradores/services/vinculo.py:1089`, `app/colaboradores/services/vinculo.py:1090`. | Chamador `aplicar_no_vinculo` grava `HistoricoVinculo`, com IDs antigo/novo na observação, no mesmo `atomic`: `app/escala/services/escala_auto_executor.py:101`, `app/escala/services/escala_auto_executor.py:103`, `app/escala/services/escala_auto_executor.py:110`. | Não escreve C.posto: `app/colaboradores/services/vinculo.py:1089`. |
| `EscalaColaborador.posto` | `desfazer_ajuste_do_sistema`, ramo de recriação | Cria vínculo copiando `antigo.posto`: `app/colaboradores/services/vinculo.py:1110`, `app/colaboradores/services/vinculo.py:1111`. | Chamador `desfazer` grava `HistoricoVinculo` no mesmo `atomic`: `app/escala/services/escala_auto_executor.py:144`, `app/escala/services/escala_auto_executor.py:145`, `app/escala/services/escala_auto_executor.py:149`. | Não escreve C.posto. O outro ramo apenas apaga o novo e reabre o antigo, sem escrever posto: `app/colaboradores/services/vinculo.py:1105`, `app/colaboradores/services/vinculo.py:1106`. |

Portas e fronteiras adicionais:

- `gravar` e `salvar_campos` permitem posto na operação `'posto'` e fazem `save(update_fields=...)`; a própria porta não registra histórico nem altera vínculo: `app/colaboradores/services/colaborador.py:33`, `app/colaboradores/services/colaborador.py:55`, `app/colaboradores/services/colaborador.py:61`, `app/colaboradores/services/colaborador.py:65`, `app/colaboradores/services/colaborador.py:69`.
- `salvar_inteiro` confere somente operação e faz `save()` completo; permite as regravações acima mesmo quando operação não é `'posto'`: `app/colaboradores/services/colaborador.py:73`, `app/colaboradores/services/colaborador.py:75`, `app/colaboradores/services/colaborador.py:76`.
- `atualizar_em_lote` rejeita explicitamente `posto` e `posto_id`: `app/colaboradores/services/colaborador.py:86`.
- `gravar_regua` é uma porta genérica de EC que executa `qs.update(**campos)` sem lista restritiva no corpo; seus quatro chamadores consultados passam somente campos da régua, nenhum posto: `app/colaboradores/services/vinculo.py:1048`, `app/colaboradores/services/vinculo.py:1070`, `app/escala/regua_defesa.py:662`, `app/escala/regua_defesa.py:719`, `app/escala/regua_defesa.py:742`, `app/escala/regua_defesa.py:765`.
- Demais `save` de EC usam `update_fields` sem posto: `app/colaboradores/services/vinculo.py:162`, `app/colaboradores/services/vinculo.py:462`, `app/colaboradores/services/vinculo.py:845`, `app/colaboradores/services/vinculo.py:894`, `app/colaboradores/services/vinculo.py:983`. Updates de fechamento/restauração/âncora também não escrevem posto: `app/colaboradores/services/vinculo.py:641`, `app/colaboradores/services/vinculo.py:730`, `app/colaboradores/services/vinculo.py:799`, `app/colaboradores/services/vinculo.py:995`, `app/colaboradores/services/vinculo.py:1031`, `app/colaboradores/services/vinculo.py:1106`.
- Admin de C e EC herda `SoLeitura`, que bloqueia inclusão/alteração/exclusão e torna todos os campos readonly: `app/colaboradores/admin.py:48`, `app/escala/admin.py:19`, `app/core/admin.py:59`, `app/core/admin.py:62`, `app/core/admin.py:65`, `app/core/admin.py:68`.
- Importador Nexti grava apenas telefone/email com `salvar_campos`, sem posto: `app/colaboradores/management/commands/importar_telefones_nexti.py:113`.
- `smoke_chamados._fixture` cria C com posto, mas `handle` recusa banco sem `'test'` no nome e faz rollback da fixture: `app/chamados/management/commands/smoke_chamados.py:78`, `app/chamados/management/commands/smoke_chamados.py:33`, `app/chamados/management/commands/smoke_chamados.py:40`, `app/chamados/management/commands/smoke_chamados.py:44`.
- Os receivers consultados não sincronizam C.posto↔EC.posto: colaboradores grava onboarding via campos restritos e trata Setor/reativação do Posto; escala captura âncora/vigência e regenera células, invalida previsto, revalida disputas e reparenta folgas: `app/colaboradores/signals.py:9`, `app/colaboradores/signals.py:49`, `app/colaboradores/signals.py:102`, `app/colaboradores/signals.py:119`, `app/escala/signals.py:19`, `app/escala/signals.py:35`, `app/escala/signals.py:87`, `app/escala/signals.py:138`, `app/escala/signals.py:156`, `app/escala/signals.py:186`.

Não encontrei outro escritor explícito de posto via create/update/bulk, ModelForm ou SQL na varredura estática da cópia; completude de mecanismos dinâmicos não resolvidos estaticamente: **[nao verificado]**.

## 16/11 08:42 · sessao d1a57fbc56f2 · - · - · origem=Ronald (tmux)

Censo de leitores em `app/escala`, incluindo templates em `app/templates/escala`. `C` = `Colaborador.posto`; `EC` = `EscalaColaborador.posto` (definição EC em `app/escala/models.py:873`).

| Campo | Função/sítio | Referências |
|---|---|---|
| C | `views.escala_buscar_colabs`: testa existência do posto cadastral e carrega relação | `app/escala/views.py:248`, `app/escala/views.py:249` |
| C | `services.propositor.propostas`: carrega cadastro e apresenta nome do posto do colaborador na proposta | `app/escala/services/propositor.py:443`, `app/escala/services/propositor.py:542` |
| C | `services.furos_diarios.EIXOS` e `ranking`: agrupamento por posto/praça, apresentação por colaborador e atribuição a supervisor | `app/escala/services/furos_diarios.py:118`, `app/escala/services/furos_diarios.py:119`, `app/escala/services/furos_diarios.py:135`, `app/escala/services/furos_diarios.py:137`, `app/escala/services/furos_diarios.py:145`, `app/escala/services/furos_diarios.py:148`, `app/escala/services/furos_diarios.py:156`, `app/escala/services/furos_diarios.py:159` |
| C | `services.furos_diarios.ranking_colabs`: filtros posto/praça e projeção/apresentação do nome | `app/escala/services/furos_diarios.py:180`, `app/escala/services/furos_diarios.py:182`, `app/escala/services/furos_diarios.py:183`, `app/escala/services/furos_diarios.py:237` |
| C | `services.furos_diarios.universo_empresa`: carrega posto e define pertencimento posto/praça das linhas já apuradas e das adicionadas ao universo | `app/escala/services/furos_diarios.py:259`, `app/escala/services/furos_diarios.py:279`, `app/escala/services/furos_diarios.py:280`, `app/escala/services/furos_diarios.py:325`, `app/escala/services/furos_diarios.py:326`, `app/escala/services/furos_diarios.py:327` |
| EC | `views.minha_escala`: carrega posto do vínculo ativo; template mostra | `app/escala/views.py:17`, `app/escala/views.py:20`, `app/escala/views.py:25`, `app/templates/escala/minha_escala.html:28` |
| EC | `EscalaColaboradorAdmin`: coluna e detalhe do posto | `app/escala/admin.py:27`, `app/escala/admin.py:31` |
| EC | `views_plano_folgas.plano_folgas`: relação reversa `Posto.escalas`, definida pelo FK de EC; lista postos com vínculos ativos da empresa | `app/escala/views_plano_folgas.py:43`, `app/escala/views_plano_folgas.py:44`, `app/escala/models.py:873` |
| EC | `views_plano_folgas._grid_ctx`: filtro, carga e ordenação por posto do vínculo; template apresenta | `app/escala/views_plano_folgas.py:58`, `app/escala/views_plano_folgas.py:59`, `app/templates/escala/plano_folgas.html:77` |
| EC | `alimentacao.escalas_do_periodo_por_colab`: carrega posto/município dos vínculos candidatos e dos adicionais nomeados pelas células | `app/escala/alimentacao.py:148`, `app/escala/alimentacao.py:152`, `app/escala/alimentacao.py:157`, `app/escala/alimentacao.py:158` |
| EC | `EscalaColaborador.vinculos_5x2_6x1_trabalha_feriado`: conta declaração `Posto.opera_em_feriado` | `app/escala/models.py:928`, `app/escala/models.py:932` |
| EC | `EscalaColaborador.feriado_vira_folga`: entrega vínculo ao leitor de município `lugar_do_vinculo` | `app/escala/models.py:1394`, `app/escala/models.py:1395`, `app/core/feriados.py:131`, `app/core/feriados.py:132` |
| EC | `services.meio_periodo.observado_meio_periodo`: carrega e agrupa sinal pelo nome do posto do vínculo | `app/escala/services/meio_periodo.py:153`, `app/escala/services/meio_periodo.py:187`, `app/escala/services/meio_periodo.py:188` |
| EC | `services.cadastro_realidade.beco_em_lote`: identifica vínculo/posto do dia na saída C11 | `app/escala/services/cadastro_realidade.py:513`, `app/escala/services/cadastro_realidade.py:517` |
| EC | `services.cadastro_realidade._vinculos`: carga por vigência, escolha pela juíza e apresentação do posto escolhido | `app/escala/services/cadastro_realidade.py:761`, `app/escala/services/cadastro_realidade.py:762`, `app/escala/services/cadastro_realidade.py:765`, `app/escala/services/cadastro_realidade.py:768` |
| EC | `services.semana_do_posto._colabs_do_posto`: filtra vínculos pelo posto na janela semanal | `app/escala/services/semana_do_posto.py:76`, `app/escala/services/semana_do_posto.py:77`, `app/escala/services/semana_do_posto.py:79` |

Consumos posteriores de valores já derivados do posto: `cadastro_realidade.acao` usa `v['posto_id']` nos destinos do Plano de Folgas (`app/escala/services/cadastro_realidade.py:713`, `app/escala/services/cadastro_realidade.py:725`); `lista` prioriza `posto_do_dia`, agrupa por `v['posto']` e retorna os grupos (`app/escala/services/cadastro_realidade.py:905`, `app/escala/services/cadastro_realidade.py:931`, `app/escala/services/cadastro_realidade.py:962`); o template mostra esses grupos (`app/templates/escala/cadastro_x_realidade.html:21`, `app/templates/escala/cadastro_x_realidade.html:23`); `pdf_bytes` imprime os mesmos grupos (`app/escala/services/cadastro_realidade.py:1083`, `app/escala/services/cadastro_realidade.py:1084`). JS do Plano de Folgas lê texto já renderizado do posto para busca/seleção (`app/templates/escala/plano_folgas.html:245`, `app/templates/escala/plano_folgas.html:247`, `app/templates/escala/plano_folgas.html:251`, `app/templates/escala/plano_folgas.html:294`, `app/templates/escala/plano_folgas.html:296`).

**Seletores de vínculo e apuração**

- `vinculo_do_dia` não escolhe por nenhum dos dois postos: consulta `CelulaDia.escala_geradora_id`; havendo geradora, encontra aquele EC e verifica limites de vigência; sem geradora, escolhe entre vínculos que cobrem a data por `(ativa, data_inicio)` (`app/escala/alimentacao.py:269`, `app/escala/alimentacao.py:279`, `app/escala/alimentacao.py:282`, `app/escala/alimentacao.py:284`, `app/escala/alimentacao.py:285`, `app/escala/alimentacao.py:286`). `vinculo_cobre_a_data` verifica datas e vigência declarada (`app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:215`, `app/escala/alimentacao.py:216`).
- `vinculo_do_dia_carregado` → `vinculos_do_dia_carregado` → `vinculos_dos_dias` são carregadores da mesma seleção; carregam células, geradoras adicionais e vínculos e chamam `vinculo_do_dia` (`app/escala/alimentacao.py:316`, `app/escala/alimentacao.py:317`, `app/escala/alimentacao.py:329`, `app/escala/alimentacao.py:330`, `app/escala/alimentacao.py:355`, `app/escala/alimentacao.py:357`, `app/escala/alimentacao.py:358`).
- `escala_vigente` carrega célula/geradora; com geradora pergunta à mesma juíza, sem ela carrega candidatos e pergunta à mesma juíza (`app/escala/servico_jornada.py:46`, `app/escala/servico_jornada.py:48`, `app/escala/servico_jornada.py:61`, `app/escala/servico_jornada.py:63`, `app/escala/servico_jornada.py:64`, `app/escala/servico_jornada.py:66`).
- `_esc_vigente_do_dia`, usado pelo previsto, carrega os vínculos e a geradora e delega à mesma juíza (`app/escala/utils.py:2180`, `app/escala/utils.py:2181`, `app/escala/utils.py:2191`, `app/escala/utils.py:2194`).
- `EC.eh_dia_trabalho` devolve `CelulaDia.trabalha`; se não há célula, usa `eh_dia_trabalho_calculado` (`app/escala/models.py:1089`, `app/escala/models.py:1092`, `app/escala/models.py:1094`, `app/escala/models.py:1095`, `app/escala/models.py:1096`). O cálculo usa `self.tipo_escala` e `self.data_ancora_colaborador` (`app/escala/models.py:1271`, `app/escala/models.py:1272`); ao final pode transformar trabalho em folga pelo feriado (`app/escala/models.py:1369`, `app/escala/models.py:1370`).
- `EC.marcos_do_dia` usa `CelulaDia.dna['marcos']`; sem marcos usa `self.tipo_escala.marcos_do_dia` (`app/escala/models.py:1119`, `app/escala/models.py:1122`, `app/escala/models.py:1124`, `app/escala/models.py:1131`, `app/escala/models.py:1133`).
- `minutos_previstos_do_dia` escolhe EC do dia, exige trabalho e obtém marcos daquele EC (`app/escala/utils.py:2260`, `app/escala/utils.py:2266`, `app/escala/utils.py:2269`). `previstas_do_periodo` percorre fatias de EC e consulta `esc.eh_dia_trabalho` (`app/escala/alimentacao.py:389`, `app/escala/alimentacao.py:390`, `app/escala/alimentacao.py:391`, `app/escala/alimentacao.py:396`, `app/escala/alimentacao.py:397`).
- A autoridade do espelho injeta feriados pela janela, escolhe EC pela juíza, obtém `tipo_escala` e previstos; cada motor de fatia recebe `esc_fatia.tipo_escala`, o conjunto de feriados e a lista de previstos (`app/ponto/services/espelho.py:1029`, `app/ponto/services/espelho.py:1065`, `app/ponto/services/espelho.py:1066`, `app/ponto/services/espelho.py:1069`, `app/ponto/services/espelho.py:1111`, `app/ponto/services/espelho.py:1112`, `app/ponto/services/espelho.py:1113`, `app/ponto/services/espelho.py:1115`).
- O motor consulta o memo de previsto; sem memo busca colaborador por pk e chama `minutos_previstos_do_dia`, alimentado por células/folgas (`app/ponto/motor_calculo_v2.py:1232`, `app/ponto/motor_calculo_v2.py:1239`, `app/ponto/motor_calculo_v2.py:1240`, `app/ponto/motor_calculo_v2.py:1251`, `app/ponto/motor_calculo_v2.py:1253`).

**Conclusão precisa sobre campo de origem**

O vínculo/tipo/célula governa a escala e o previsto; a participação de `EC.posto` nesse caminho é o calendário de feriado, via município do posto do vínculo. Não é correto dizer que o ritmo inteiro sai diretamente de `EC.posto`: `tipo_escala`, âncora, célula e DNA são lidos separadamente (`app/escala/models.py:1095`, `app/escala/models.py:1133`, `app/escala/models.py:1271`, `app/escala/models.py:1272`, `app/escala/models.py:1395`). Já o pertencimento das apurações de furos a posto/praça usa `C.posto` (`app/escala/services/furos_diarios.py:118`, `app/escala/services/furos_diarios.py:119`, `app/escala/services/furos_diarios.py:279`, `app/escala/services/furos_diarios.py:280`).

O feriado municipal/estadual usa `EC.posto.municipio`: `lugar_do_vinculo` lê `vinculo.posto` e `posto.municipio` (`app/core/feriados.py:131`, `app/core/feriados.py:132`); `feriados_do_colab` resolve o vínculo para cada data (`app/core/feriados.py:161`); `alcanca` compara UF no estadual e UF+município no municipal (`app/core/feriados.py:62`, `app/core/feriados.py:63`, `app/core/feriados.py:64`, `app/core/feriados.py:65`, `app/core/feriados.py:66`). No motor, o conjunto injetado responde sozinho; sem conjunto ocorre fallback sem município (`app/ponto/motor_calculo_v2.py:963`, `app/ponto/motor_calculo_v2.py:965`, `app/ponto/motor_calculo_v2.py:966`, `app/ponto/motor_calculo_v2.py:967`, `app/ponto/motor_calculo_v2.py:41`).

_5957 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
