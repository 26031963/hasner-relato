# CODEX-ESTADO

> Ultima resposta processada: **08/11/2026 09:25** · publicado em **08/11/2026 09:25** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 08/11 09:25 · sessao c8d28fdc59f6 · FIO-E0-V4-E1 · verificador · origem=Code

**Parecer: REPROVA.** Há contraexemplo à igualdade entre o universo eleito pela porta e o universo de seus membros; além disso, o reuso admitido pelo juiz continua sujeito a leitores que tratam a data gravada como dia do turno. (`app/chamados/services/fio.py:1391`; `app/chamados/services/declaracao_texto.py:244`; `app/chamados/juizes.py:336`; `app/chamados/juizes.py:1988`.)

Os cenários abaixo são contraprovas construídas sobre o código. Sua ocorrência em produção, execução integrada dos testes e números SQL observados estão **[nao verificado]**. [nao verificado]

1. **Campos e memo — procedente, gravidade alta quanto ao significado de `data_evento`.**

   | Campo/valor | Quem escreve | Quem lê e qual significado usa |
   |---|---|---|
   | `data_evento` | `_nascer` grava o parâmetro; `abrir_conversa` fornece dia civil; `reapontar_resolvedora` data a existente; `rechavear_data_evento` troca a data. (`app/chamados/models.py:2707`; `app/chamados/models.py:2824`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:959`.) | Busca/índice usam a data gravada; o fecho passa a derivar a identidade pelo juiz. C6, validação de destino, plantio e expiração continuam lendo a gravada como dia relevante. (`app/chamados/models.py:2663`; `app/chamados/models.py:3298`; `app/chamados/services/fio.py:1235`; `app/chamados/juizes.py:336`; `app/chamados/juizes.py:1988`; `app/chamados/services/materializacao.py:344`; `app/chamados/veredito_lavrado.py:229`.) |
   | `batida_origem_id` | `_nascer` aceita explícita ou deriva do contexto; o `save` de inserção deriva quando ausente. (`app/chamados/models.py:2751`; `app/chamados/models.py:3254`.) | Identifica episódio no índice, busca, plano e agrupamento; `_dia_do_fato` carrega a Batida para perguntar seu dia ao juiz. É FK do episódio, não uma data. (`app/chamados/models.py:2632`; `app/chamados/models.py:3295`; `app/chamados/services/fio.py:1020`; `app/chamados/services/fio.py:1122`; `app/chamados/services/fio.py:1234`.) |
   | `instancia_marco` | `_chave_de_identidade` deriva do motivo e recusa explícita divergente; há backfill histórico. (`app/chamados/models.py:3243`; `app/chamados/models.py:3252`; `app/chamados/migrations/0051_decisao_pergunta.py:27`.) | Busca/índice/base do fecho usam posição do marco; `_dia_do_fato` usa seu preenchimento para reconhecer linha de marco. (`app/chamados/models.py:2663`; `app/chamados/models.py:3289`; `app/chamados/services/fio.py:1205`; `app/chamados/services/fio.py:1237`.) |
   | `celula_id` | Nascimento e reuso escolhem célula pela **data pedida**; rechaveamento escolhe pela nova data. (`app/chamados/models.py:2748`; `app/chamados/models.py:2782`; `app/chamados/models.py:2825`; `app/chamados/services/fio.py:956`; `app/chamados/services/fio.py:960`.) | C6 só aceita essa alimentação se `celula.data == data_evento`; outbox prefere a FK. `_dia_do_fato` não lê `p.celula.data`: passa `p.data_evento` ao conversor da célula. (`app/chamados/juizes.py:638`; `app/chamados/models.py:3047`; `app/chamados/services/fio.py:1239`.) |
   | `chamado_destino_id` | Nascimento/reuso em `_nascer`; reapontamento em `reapontar_resolvedora`. (`app/chamados/models.py:2825`; `app/chamados/models.py:2796`; `app/chamados/services/fio.py:868`.) | Âncora da listagem, validação da costura e julgamento do destino. O significado de FK de destino é consistente nesses caminhos. (`app/chamados/services/fio.py:117`; `app/chamados/models.py:2786`; `app/chamados/services/fio.py:856`; `app/chamados/services/fio.py:761`.) |

   **Não existe acordo global sobre `data_evento`.** O comentário novo declara chave gravada versus identidade derivada, mas `chave_do_dia` ainda se documenta como identidade, `_dia_de_turno_da_pergunta` devolve a gravada e o plantio combina a hora com ela. A declaração local não migrou esses leitores. (`app/chamados/services/fio.py:1219`; `app/chamados/services/fio.py:1198`; `app/chamados/juizes.py:332`; `app/chamados/services/materializacao.py:414`.)

   **Memo misto — não procedente a suspeita de colisão ou iteração incompatível.** Vizinhos usam chave de três elementos `(pk, chave_do_dia, bo)`; dia e colaborador usam chaves de dois elementos `('dia_do_turno', bo)` e `('colab', cid)`. Os consumidores acessam chaves específicas; não encontrei consumidor que percorra esse dicionário supondo valores exclusivamente de vizinhança. (`app/chamados/services/fio.py:1230`; `app/chamados/services/fio.py:1246`; `app/chamados/services/fio.py:1258`; `app/chamados/services/fio.py:1264`; `app/chamados/services/fio.py:1278`.)

2. **Propriedade fixa entre porta e membros — procedente, gravidade alta.**

   Contraexemplo: mesmo colaborador, motivo `gps_ausente`, nenhuma linha de acervo; alfa e beta são episódios distintos cujo juiz responde **18/10**. Cada episódio tem uma linha gravada em **10/09** e outra em **18/10**. Essa disposição cabe no índice: o horizonte é 21/09 e a unicidade do episódio só inclui linhas a partir dele. A premissa de duas batidas julgadas no mesmo dia está no teste novo. (`app/chamados/models.py:2313`; `app/chamados/models.py:3294`; `app/chamados/tests/test_e1_v13.py:59`.)

   | Consulta | Universo resultante |
   |---|---|
   | `universo_da_linha(A0)` ou `A1` | `{A0, A1}` |
   | `universo_da_linha(B0)` ou `B1` | `{B0, B1}` |
   | `universo_do_fato(colab, gps_ausente, 18/10, bo=None)` | `{A0, A1, B0, B1}` |

   A causa é concreta: sem episódio, `do_fato` traz as duas linhas gravadas em 18/10; ambas entram em `datadas`; o fecho preserva todas as sementes, sem exigir conectividade entre elas. Com episódio, a busca só liga o próprio episódio ao acervo sem episódio, inexistente neste cenário. Os resultados foram reproduzidos em memória com funções extraídas do código e ORM/juiz substituídos por stubs; integração e produção **[nao verificado]**. (`app/chamados/models.py:2630`; `app/chamados/models.py:2632`; `app/chamados/models.py:2636`; `app/chamados/services/fio.py:1320`; `app/chamados/services/fio.py:1381`; `app/chamados/services/fio.py:1391`.)

   **Os consumidores realmente divergem:** `_candidata_reusavel` chama sem episódio; `grupos_de_colisao` detecta as duas chaves de episódio e expande cada uma por fecho individual, produzindo dois grupos. O dedup consome esses grupos; a migração e `_colisao_ao_datar` começam pelo universo da linha. (`app/chamados/services/declaracao_texto.py:244`; `app/chamados/services/fio.py:1479`; `app/chamados/services/fio.py:1507`; `app/chamados/management/commands/dedup_perguntas_fato.py:58`; `app/chamados/services/fio.py:1634`; `app/chamados/services/fio.py:891`.)

   Há caminho de emissão sem episódio inferido: `gerar_perguntas_disputa` admite contexto sem `ultima_batida`, aplica seu dedup local e pode chegar a `abrir` com dia do contexto. Uma disputa nova não fica impedida pelas perguntas de outras disputas. A ocorrência desse cenário em produção está **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:325`; `app/chamados/services/disputa_emissao.py:327`; `app/chamados/services/disputa_emissao.py:372`; `app/chamados/services/disputa_emissao.py:444`.)

   **Assert de dois dias por chamador real — não demonstrado.** O agrupamento expande sementes individuais; `_colisao_ao_datar` começa por uma linha; a migração entrega ao plano membros do universo encontrado. Não obtive contraprova desse assert nesses caminhos. (`app/chamados/services/fio.py:1507`; `app/chamados/services/fio.py:891`; `app/chamados/services/fio.py:1634`; `app/chamados/services/fio.py:1641`.)

3. **Porta e célula — dois achados procedentes, gravidade alta.**

   **Nascimento sem linha anterior não verifica o dia da batida pedida.** Se o episódio ainda não possui perguntas, `universo_do_fato` não encontra linha para julgar. A recusa exige uma linha recusada daquele episódio; ausente essa linha, nasce pergunta com a data pedida e célula dessa data. Portanto `abrir(D0, batida cujo juiz diz DIA)` pode criar em D0; uma chamada direta posterior igual pode recusar a própria linha pelo juiz. (`app/chamados/services/fio.py:1362`; `app/chamados/services/fio.py:1376`; `app/chamados/models.py:2766`; `app/chamados/models.py:2748`; `app/chamados/models.py:2824`.)

   O caller de emissão não garante igualdade: grade/contexto têm precedência, e o juiz da última batida só é consultado quando falta a data anterior. Envolver `_dia_p` em `dia_do_turno_da_celula` não confere essa igualdade: o conversor devolve o valor recebido. (`app/chamados/services/disputa_emissao.py:371`; `app/chamados/services/disputa_emissao.py:378`; `app/chamados/services/disputa_emissao.py:424`; `app/chamados/services/disputa_emissao.py:451`; `app/ponto/turnos.py:2933`.)

   **Reuso correto pelo juiz pode falhar na costura.** Tome alfa gravada em 10/09, juiz em 18/10, como no teste novo; acrescente destino de escopo DIA declarado em 18/10. O proxy do pedido passa. A porta elege alfa e prepara célula de 18/10, mas conserva `ex.data_evento=10/09`. A segunda validação lê essa data antiga e lança `ResolvedoraCruzada`, pois a diferença supera um dia. O teste de reuso não fornece destino. (`app/chamados/tests/test_e1_v13.py:200`; `app/chamados/tests/test_e1_v13.py:203`; `app/chamados/models.py:2737`; `app/chamados/models.py:2782`; `app/chamados/models.py:2786`; `app/chamados/juizes.py:1988`; `app/chamados/juizes.py:2016`; `app/chamados/juizes.py:2023`.)

   Sem essa recusa, o estado persistido pode ficar com célula DIA e data gravada D0; C6 descarta a célula como alimentação e continua julgando pelo dia gravado. Logo atribuir a célula certa não reconcilia os leitores. (`app/chamados/models.py:2820`; `app/chamados/juizes.py:638`; `app/chamados/juizes.py:671`; `app/chamados/juizes.py:679`.)

   **Correção do inventário de callers:** os sítios de produção de `abrir` não passam `batida_origem_id` explicitamente; `_nascer` a infere. `reapontar_resolvedora` e `_colisao_ao_datar` não chamam `abrir`: usam a porta de existente. (`app/chamados/services/disputa_emissao.py:444`; `app/chamados/services/disputa_emissao.py:834`; `app/chamados/services/declaracao_texto.py:348`; `app/chamados/services/declaracao_texto.py:636`; `app/chamados/models.py:2751`; `app/chamados/services/fio.py:849`; `app/chamados/services/fio.py:868`.)

   **Não procede acusar automaticamente o agrupador de passar dia civil:** seu `localtime(...).date()` alimenta a classificação; a criação recebe `resolver_data_evento(...batida_origem=...)`, que consulta o juiz. (`app/chamados/services/disputa_emissao.py:1108`; `app/chamados/services/disputa_emissao.py:1114`; `app/chamados/services/disputa_emissao.py:1123`; `app/escala/servico_jornada.py:488`.)

4. **Juiz, derivação e relógio — não procedente como clonagem nova; inconsistência de leitores permanece.**

   `_dia_do_fato` delega a batida ao recorte do juiz registrado: `dia_do_turno_da_batida → dia_das_batidas → turnos_do_colab`. Não reproduz pareamento nem deriva dia do timestamp por conta própria. A ponte é a implementação expressamente descrita para linhas sem dia próprio. (`app/core/juizes.py:196`; `app/chamados/services/fio.py:1235`; `app/ponto/turnos.py:2925`; `app/ponto/turnos.py:2903`; `app/chamados/services/fio.py:1227`.)

   Os acréscimos de identidade/memo/contador não introduzem leitura direta de relógio. A catraca de calendário cobre `.date()` sobre timestamp e lookups `timestamp__date`; isso não detecta o conflito semântico entre o novo juiz e leitores antigos de `data_evento`. A correspondência histórica dessa catraca com `935c8d96` está **[nao verificado]**. (`app/chamados/services/fio.py:1229`; `app/chamados/services/fio.py:1268`; `app/chamados/services/fio.py:1410`; `app/ponto/tests/test_contract_no_batida_date.py:175`; `app/ponto/tests/test_contract_no_batida_date.py:203`; `app/chamados/juizes.py:336`.)

   Para marco, o código declara `p.data_evento` como dia de célula sem consultar a FK. Erro concreto adicional dependeria de marco com data gravada divergente da célula soberana; presença desse caso está **[nao verificado]**. (`app/chamados/services/fio.py:1237`; `app/chamados/services/fio.py:1239`.)

5. **Custo — procedente como amplificação, gravidade média; estouro de teto [nao verificado].**

   Considere **B** batidas existentes distintas examinadas, incluindo as alcançadas na ponte e recusadas:

   | Operação | Chamadas ao juiz pesado |
   |---|---|
   | Um fecho com memo novo | Uma por batida distinta examinada. |
   | Um `abrir` | Zero com busca vazia; caso contrário, B no universo/ponte/fecho compartilhado. |
   | Recusa `FatoEmOutroDia` | Mais uma para o episódio, pois o rótulo abre outro memo. |
   | `fecho_cruza_dias` | Soma dos B de cada fecho executado; não compartilha memo entre componentes. |

   Essas contagens decorrem das chaves por batida, do compartilhamento local e da criação de `_memo_t` separado. Batida inexistente custa a busca, mas não chama o juiz. (`app/chamados/services/fio.py:1230`; `app/chamados/services/fio.py:1234`; `app/chamados/services/fio.py:1339`; `app/chamados/services/fio.py:1392`; `app/chamados/models.py:2772`; `app/chamados/services/fio.py:1428`.)

   **Memo por batida não basta para compartilhar trabalho por turno/janela:** cada batida distinta reconstrói `dia_das_batidas(d−1,d+1)`, carregando turnos e batidas novamente, mesmo com janelas sobrepostas. (`app/ponto/turnos.py:2925`; `app/ponto/turnos.py:2903`; `app/ponto/turnos.py:2905`; `app/ponto/turnos.py:1863`; `app/ponto/turnos.py:1884`.)

   Os selos citados têm coberturas diferentes: chokepoint impõe tetos SQL; modal monta 330 perguntas sem episódio; `test_porta_chamado_e1` mede atos/no-ops de Chamado; cadeia/ordem verificam transação e precedência de travas, sem impor teto de consultas. Não são prova conjunta do novo custo da porta por episódio. (`app/ponto/tests/test_selo_chokepoint_escrita.py:397`; `app/chamados/tests/test_selo_teto_modal.py:68`; `app/chamados/tests/test_porta_chamado_e1.py:57`; `app/chamados/tests/test_e1_v13_cadeia.py:32`; `app/chamados/tests/test_e1_v13_ordem.py:59`.)

6. **Candidatas de `fecho_cruza_dias` — não procedente a falta de cobertura, sob as invariantes de nascimento.**

   Todo passo entre linhas distintas ocorre por chave gravada ou por episódio/motivo. O contador seleciona chave com múltiplas linhas e episódio com múltiplas chaves; assim cobre ambas as formas de aresta. Colaborador e instância são normalizados no nascimento. (`app/chamados/models.py:2630`; `app/chamados/models.py:2632`; `app/chamados/models.py:2636`; `app/chamados/services/fio.py:1416`; `app/chamados/services/fio.py:1421`; `app/chamados/models.py:3236`; `app/chamados/models.py:3243`.)

   Dois episódios distintos na mesma data, sem acervo, são candidatas excedentes, mas seus fechos não recusam vizinhos e não entram no resultado. A confirmação usa o fecho real. (`app/chamados/models.py:2632`; `app/chamados/services/fio.py:1428`; `app/chamados/services/fio.py:1431`.)

   O contador, porém, acrescenta membros quando existe **qualquer** recusa; não verifica novamente se sua causa foi outro dia, outra base ou ponte ambígua. Garantia irrestrita para acervo que viole as invariantes, ou ocorrência de falso positivo nesse acervo: **[nao verificado]**. (`app/chamados/services/fio.py:1340`; `app/chamados/services/fio.py:1431`.)

7. **Classes de reprovação e selos.**

   - **Segundo escritor fora da porta — não procedente no censo AST de `data_evento`.** Executei em memória o scanner existente: retornou somente `_nascer`, `reapontar_resolvedora` e `rechavear_data_evento`, sem sítio fora das portas. Isso verifica localização da escrita, não correção do significado. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:30`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:122`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:143`.)
   - **Ordem de trava — nenhuma inversão nova demonstrada.** `_nascer` trava Colaborador antes do universo; as leituras novas de Batida/Colaborador não pedem `FOR UPDATE`. Execução dos selos de ordem e concorrência: **[nao verificado]**. (`app/chamados/models.py:2724`; `app/chamados/models.py:2764`; `app/chamados/services/fio.py:1234`; `app/chamados/services/fio.py:1252`.)
   - **RED v13 sem mordida — não procedente contra a variante simples por data gravada.** O teste exige B0, gravada D0, dentro do fato DIA; voltar a decidir pela gravada a exclui. Também exige reuso de A0 em outra data e recusa de beta pedido em D0. (`app/chamados/tests/test_e1_v13.py:108`; `app/chamados/tests/test_e1_v13.py:121`; `app/chamados/tests/test_e1_v13.py:157`; `app/chamados/tests/test_e1_v13.py:198`.)
   - **Lacunas dos novos selos — procedente, gravidade média.** O cenário de união de episódios usa acervo como ponte; o teste de reuso omite destino. Não cobrem as contraprovas dos itens 2 e 3. (`app/chamados/tests/test_e1_v13.py:78`; `app/chamados/tests/test_e1_v13.py:203`.)
   - **Preservação byte a byte dos v5/v8/v10/v11, RED executado, mutante executado e passagem dos tetos:** **[nao verificado]**.
   - **Campo com dois significados não reconciliados — procedente, gravidade alta:** a identidade nova usa o juiz, enquanto destino, C6 e plantio continuam usando a data gravada como dia do fato. (`app/chamados/services/fio.py:1235`; `app/chamados/juizes.py:1988`; `app/chamados/juizes.py:336`; `app/chamados/services/materializacao.py:414`.)

## 08/11 09:21 · sessao 7f8fd01fd110 · - · - · origem=Ronald (tmux)

Procedente, alta: a porta ainda une componentes desconectados quando chamada sem episódio. Para `gps_ausente`, use A0/A1 do episódio alfa, gravadas respectivamente em 10/09 e 18/10, B0/B1 do episódio beta nas mesmas datas, ambos julgados como 18/10, sem linha de acervo. Por linha, os universos são `{A0,A1}` e `{B0,B1}`; `universo_do_fato(...,18/10,batida_origem_id=None)` retorna as quatro. A seleção sem episódio traz ambas as linhas de 18/10; `datadas` conserva ambas e `fecho_do_fato` retém todas as sementes independentemente da conectividade. `app/chamados/models.py:2636`, `app/chamados/models.py:2663`, `app/chamados/services/fio.py:1320`, `app/chamados/services/fio.py:1381`, `app/chamados/services/fio.py:1391`.

O chamador real `_candidata_reusavel` omite episódio. O agrupamento real detecta duas chaves de episódio e chama o universo de cada uma separadamente, produzindo dois grupos; portanto porta e backfill/dedup não compartilham o universo anunciado. `app/chamados/services/declaracao_texto.py:244`, `app/chamados/services/fio.py:1122`, `app/chamados/services/fio.py:1479`, `app/chamados/services/fio.py:1501`, `app/chamados/services/fio.py:1507`.

O cenário admite índice ativo: o horizonte é 21/09 e a unicidade de episódio só inclui linhas gravadas a partir dele; cada episódio possui uma linha dentro e outra fora. A premissa de dois episódios no mesmo dia do juiz existe no teste novo. `app/chamados/models.py:2313`, `app/chamados/models.py:3294`, `app/chamados/tests/test_e1_v13.py:59`.

Confirmei os resultados executando somente em memória as funções reais extraídas por AST, com ORM e juiz substituídos por stubs explícitos; execução integrada e presença desse cenário em produção são [nao verificado].

Não procedente para linhas conformes: o universo candidato do contador cobre toda aresta possível do `do_fato`: vizinhança pela chave gravada ou pelo mesmo episódio/motivo. Candidatas excedentes são confirmadas pelo fecho real, evitando contar dois episódios na mesma data sem acervo. `app/chamados/models.py:2630`, `app/chamados/models.py:2632`, `app/chamados/models.py:2636`, `app/chamados/services/fio.py:1415`, `app/chamados/services/fio.py:1421`, `app/chamados/services/fio.py:1428`.

Não encontrei sementes de dois dias vindas dos consumidores internos com grupos normalmente produzidos: o agrupamento parte pelos fechos individuais; `_colisao_ao_datar` começa por uma linha; a migração entrega ao plano membros do universo dessa linha. Outros estados ou concorrência que produzam o assert são [nao verificado]. `app/chamados/services/fio.py:891`, `app/chamados/services/fio.py:1504`, `app/chamados/services/fio.py:1634`, `app/chamados/services/fio.py:1641`.

## 08/11 09:20 · sessao fb39c0000d12 · - · - · origem=Ronald (tmux)

Inventário por campo e achados:

| Campo | Escritores | Leitores / significado |
|---|---|---|
| `data_evento` | Nascimento datado e conversa: `app/chamados/models.py:2696`, `:2707`, `:2824`. Datar existente: `app/chamados/services/fio.py:838`, `:840`, `:870`. Rechaveamento: `app/chamados/services/fio.py:959`, `:965`. Proxy e restauração apenas em memória: `app/chamados/models.py:2737`; `app/chamados/services/fio.py:852`, `:861`. | Chave gravada de busca/índice: `app/chamados/models.py:2630`, `:2663`, `:3289`; `app/chamados/services/fio.py:1193`, `:1205`. Identidade agora derivada pelo juiz: `app/chamados/services/fio.py:1229`, `:1235`, `:1330`, `:1375`. Outros leitores ainda a tratam como dia do turno: `app/chamados/juizes.py:332`, `:336`, `:1988`; `app/chamados/services/materializacao.py:344`, `:414`; `app/chamados/services/pergunta_no_app.py:93`; `app/chamados/services/validacao.py:269`, `:446`; janela: `app/chamados/veredito_lavrado.py:229`; apresentação/divergência: `app/chamados/services/fio.py:87`. |
| `batida_origem_id` | `_nascer` aceita valor explícito ou deriva: `app/chamados/models.py:2751`, `:2753`. `save` de inserção deriva se ausente: `app/chamados/models.py:3226`, `:3254`. Derivador lê foto do nascimento no contexto e valida dono: `app/chamados/models.py:3269`, `:3273`. | Identifica episódio: `app/chamados/models.py:2632`, `:3295`; `app/chamados/services/fio.py:1020`, `:1122`. Alimenta juiz do dia: `app/chamados/services/fio.py:1229`, `:1234`. Forma chave do cache: `app/chamados/services/fio.py:1258`; candidatos do contador: `app/chamados/services/fio.py:1413`, `:1418`. Não representa data nem instante: é FK da batida cujo instante o juiz consulta (`app/chamados/models.py:2350`; `app/chamados/services/fio.py:1234`). |
| `instancia_marco` | Derivação/validação no nascimento: `app/chamados/models.py:3243`, `:3246`, `:3252`. Backfill histórico: `app/chamados/migrations/0051_decisao_pergunta.py:27`. | Posição do marco, derivada do motivo: `app/chamados/models.py:3234`. Chave de busca/índice: `app/chamados/models.py:2663`, `:3289`; `app/chamados/services/fio.py:1205`. Discriminador da linha de marco em `_dia_do_fato`: `app/chamados/services/fio.py:1237`. |
| `celula_id` | Nascimento pela data pedida: `app/chamados/models.py:2748`, `:2825`. Reuso pela data pedida: `app/chamados/models.py:2781`, `:2782`, `:2820`. Rechaveamento pela nova data: `app/chamados/services/fio.py:956`, `:960`, `:965`. | FK da célula da pergunta: `app/chamados/models.py:2323`. C6 verifica se célula corresponde ao `data_evento` antes de alimentar julgamento: `app/chamados/juizes.py:634`, `:638`. Outbox prefere FK e só usa `(colaborador,data_evento)` como fallback: `app/chamados/models.py:3034`, `:3047`, `:3050`. `_dia_do_fato` NÃO lê `p.celula.data`; passa a data gravada à conversão de célula (`app/chamados/services/fio.py:1237`, `:1239`). |
| `chamado_destino_id` | Nascimento: `app/chamados/models.py:2825`. Reuso na porta: `app/chamados/models.py:2796`, `:2820`. Porta de reapontamento: `app/chamados/services/fio.py:868`, `:870`. Simulações sem persistência: `app/chamados/models.py:367`; `app/chamados/services/fio.py:779`, `:783`. | Âncora da listagem, com fallback ao chamado da disputa: `app/chamados/services/fio.py:117`. Gate do destino: `app/chamados/models.py:2784`, `:2786`; `app/chamados/services/fio.py:856`. C6/encerramento/outbox: `app/chamados/models.py:310`, `:361`, `:3028`. Mesmo significado de FK de destino nesses leitores (`app/chamados/models.py:3191`, `:3197`; `app/chamados/services/fio.py:100`). |

**Procedente, alta: a identidade aceita pelo fecho continua podendo ser recusada pelo leitor do destino.** Cenário derivado da própria fixture: episódio alfa gravado `D0=2026-09-10`, juiz retorna `DIA=2026-10-18`; `abrir(...DIA, chamado_destino=chDIA)` encontra e elege alfa, mas conserva `ex.data_evento=D0` (`app/chamados/tests/test_e1_v13.py:200`; `app/chamados/tests/test_e1_v9.py:37`; `app/chamados/models.py:2777`, `:2782`, `:2820`). O proxy do pedido passa, depois o gate de `ex` compara a data gravada com o destino e lança `ResolvedoraCruzada` para esse intervalo superior a um dia (`app/chamados/models.py:2737`, `:2741`, `:2786`, `:2788`; `app/chamados/juizes.py:1988`, `:2007`, `:2016`, `:2023`). O teste novo de reaproveitamento não fornece destino (`app/chamados/tests/test_e1_v13.py:203`).

**Procedente, alta: a porta não valida o dia do episódio no nascimento sem linha preexistente.** A recusa requer universo vazio **e** uma recusada do mesmo episódio; sem linhas, chega ao `create` com a data pedida e célula dessa data (`app/chamados/models.py:2748`, `:2766`, `:2824`). Assim, um pedido `D0` para batida cujo juiz diz `DIA` pode nascer `D0`; a emissão seguinte pode recusar a própria linha pelo juiz (`app/chamados/services/fio.py:1375`, `:1380`; `app/chamados/models.py:2766`). Ocorrência em produção: [nao verificado].

**Callers concretos de `abrir`.** Há quatro sítios de produção; nenhum passa explicitamente `batida_origem_id`, que a porta infere do contexto (`app/chamados/services/disputa_emissao.py:444`, `:834`; `app/chamados/services/declaracao_texto.py:348`, `:636`; `app/chamados/models.py:2751`).

- `gerar_perguntas_disputa`: grade/contexto têm precedência; só consulta juiz da última batida se o fallback estiver vazio; o destino ainda pode fornecer `_data_ev` prioritária (`app/chamados/services/disputa_emissao.py:350`, `:371`, `:378`, `:391`, `:424`, `:451`). Contraexemplo de chamada possível: contexto de `gps_ausente` com `data=D0` e `ultima_batida=b1` de `DIA`; nasce com a divergência acima. O merge preserva chaves antigas ausentes no novo valor (`app/chamados/services/disputa_emissao.py:555`, `:556`). Existência desse contexto em produção: [nao verificado].
- Agrupador: o `localtime(...).date()` serve à classificação, mas o dia passado à criação é o retorno de `resolver_data_evento(...batida_origem=...)`; portanto, não procede afirmar que esse caller passa automaticamente dia civil (`app/chamados/services/disputa_emissao.py:1108`, `:1109`, `:1114`, `:1123`; `app/escala/servico_jornada.py:488`).
- Declaração: ambos passam dia da célula ao conversor registrado (`app/chamados/services/declaracao_texto.py:347`, `:349`, `:635`, `:637`).
- `reapontar_resolvedora` e `_colisao_ao_datar` **não chamam `abrir`**: usam a porta de existente, `fato_livre_para`, `universo_da_linha` e plano; a escrita ocorre no save de reapontamento (`app/chamados/services/fio.py:849`, `:850`, `:891`, `:904`, `:868`, `:870`).

**Procedente, média/alta: dois significados não foram reconciliados em todos os leitores.** `_dia_de_turno_da_pergunta` continua retornando `data_evento` cru para perguntas de fato e é consumido pelo julgamento do C6 e tranca (`app/chamados/juizes.py:332`, `:336`, `:375`, `:671`, `:763`, `:948`, `:1216`). A célula nova `DIA` também deixa de servir à dieta do julgamento se a data gravada permanecer `D0` (`app/chamados/models.py:2782`; `app/chamados/juizes.py:638`). A declaração no comentário novo de `_dia_do_fato` não altera esses leitores (`app/chamados/services/fio.py:1218`; `app/chamados/juizes.py:336`, `:1988`).

**Não demonstrado como erro autônomo: marco usar data gravada.** `_dia_do_fato` não lê a FK da célula; passa `p.data_evento` ao juiz da célula (`app/chamados/services/fio.py:1237`, `:1239`). Isso só produz dia errado se uma linha de marco tiver data gravada divergente do dia soberano pretendido; presença desse caso no dado dado: [nao verificado].

## 08/11 09:19 · sessao cc2c08306ed4 · - · - · origem=Ronald (tmux)

Achados de custo/selos, somente por inspeção:

1. **Procedente, média: memo por batida evita repetição dentro de um fecho, mas não evita recomputar a janela inteira para cada batida.** Cada `bo` novo faz `SELECT Batida` e chama o juiz; este executa `dia_das_batidas(d-1,d+1)`, que carrega turnos e batidas da janela. Duas batidas do mesmo turno pagam duas reconstruções sobre janelas sobrepostas. (`app/chamados/services/fio.py:1229-1236`; `app/ponto/turnos.py:2917-2926`; `app/ponto/turnos.py:2901-2907`; `app/ponto/turnos.py:1861-1888`.)

2. **Contagem de chamadas ao juiz:**
   - Por `fecho_do_fato`: uma por `batida_origem_id` distinta examinada nas sementes, vizinhos e explorações da ponte; inclui vizinhos recusados, pois `_dia_do_fato(q)` precede o filtro de base. (`app/chamados/services/fio.py:1326`; `app/chamados/services/fio.py:1339-1342`; `app/chamados/services/fio.py:1278-1286`.)
   - Por `abrir`: zero se `do_fato` vier vazio; caso contrário, uma por batida distinta examinada pelo universo/ponte/fecho, com o mesmo memo. A batida **pedida**, quando ainda não tem linha, não é julgada por esse caminho. (`app/chamados/services/fio.py:1362-1392`; `app/chamados/models.py:2751-2765`.)
   - Na exceção `FatoEmOutroDia`: **mais uma chamada** para o episódio recusado, pois o rótulo abre `_memo_t = {}` em vez de aproveitar o memo anterior. (`app/chamados/models.py:2771-2776`.)
   - Por `fecho_cruza_dias`: soma das batidas distintas de **cada fecho executado**, sem memo global. O contador chama `fecho_do_fato([p])` sem `_viz`; uma batida alcançada como vizinha recusada pode ser julgada de novo no componente seguinte. (`app/chamados/services/fio.py:1423-1432`; `app/chamados/services/fio.py:1317`.)
   - Número SQL efetivamente observado, tempo e estouro dos tetos: **[nao verificado]**.

3. **Não procedente: os selos citados não são todos selos de custo deste caminho.**
   - Chokepoint efetivamente compara consultas com teto, inclusive ramos FICA de 144/217. (`app/ponto/tests/test_selo_chokepoint_escrita.py:180`; `app/ponto/tests/test_selo_chokepoint_escrita.py:187`; `app/ponto/tests/test_selo_chokepoint_escrita.py:239-250`; `app/ponto/tests/test_selo_chokepoint_escrita.py:397-430`.)
   - Modal mede 330 perguntas, mas a fixture não fornece `batida_origem`; ela não representa o novo custo por episódio. (`app/chamados/tests/test_selo_teto_modal.py:68-79`; `app/chamados/tests/test_selo_teto_modal.py:81-97`.)
   - `test_porta_chamado_e1` mede no-op de atos de campos de **Chamado**, não `PerguntaDisputa.abrir`. (`app/chamados/tests/test_porta_chamado_e1.py:1-5`; `app/chamados/tests/test_porta_chamado_e1.py:57-62`; `app/chamados/tests/test_porta_chamado_e1.py:117-122`.)
   - `test_e1_v13_cadeia` captura SQL para detectar escrita sem trava na transação; `_ordem` verifica precedência de trava/escrita e mutantes do registro. Nenhum desses impõe teto de consultas. (`app/chamados/tests/test_e1_v13_cadeia.py:32-59`; `app/chamados/tests/test_e1_v13_ordem.py:59-82`.)

4. **Não procedente: RED v13 sem mordida contra a comparação por data gravada.** A fixture coloca B0 com `data_evento=D0` e batida beta; o teste exige B0 dentro do fato DIA. A variante simples que volta a decidir por `p.data_evento` necessariamente exclui B0 e falha esse assert. Também há reutilização de A0 gravada em outra data e recusa de beta pedido em D0. (`app/chamados/tests/test_e1_v13.py:108-124`; `app/chamados/tests/test_e1_v13.py:157-165`; `app/chamados/tests/test_e1_v13.py:198-208`.)

5. **Lacuna, média: execução explícita do mutante exigido pelo aval [nao verificado].** O arquivo possui testes semânticos que distinguem a variante, mas termina sem teste que instale o mutante `data_evento`; a execução RED/mutante não veio no dado. (`app/chamados/tests/test_e1_v13.py:116-124`; `app/chamados/tests/test_e1_v13.py:315-320`; aval anexo **[nao verificado quanto à execução]**.)

6. **Ordem de trava: nenhuma nova aquisição explícita no diff inspecionado.** `_nascer` continua travando o colaborador antes de chamar o universo; os acréscimos usam leituras comuns para Batida/Colaborador. O runtime completo e eventual regressão nos selos de ordem são **[nao verificado]**. (`app/chamados/models.py:2720-2724`; `app/chamados/models.py:2762-2765`; `app/chamados/services/fio.py:1234`; `app/chamados/services/fio.py:1248-1252`.)

7. **Preservação dos selos antigos:** o diff fornecido só declara mudanças em `models.py`, `fio.py` e `test_e1_v13.py`; não exibe alteração dos v5/v8/v10/v11. Identidade byte a byte com os commits e execução desses selos: **[nao verificado]**. [nao verificado]

## 08/11 04:49 · sessao 7108433fc78b · CXR-TELA-LE-O-MOTOR · verificador · origem=Code

**A v3 corrige a vacuidade anterior do C11 e unifica o instante usado pelo command e pelo serviço. A prova continua parcial: falta o estado “coberto, com lista vazia”, e o teste de relógio substitui justamente o espelho que precisaria verificar em profundidade.** (`app/ponto/tests/test_cxr_tela_le_o_motor.py:391`, `:417`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:30`; `app/ponto/services/cxr_na_lista.py:183`)

Suite Django completa e execução dos mutantes nessa suite: **[nao verificado]**. As contraprovas abaixo distinguem leitura estática de ensaio isolado em memória. [nao verificado]

**1) C11 chega ao dono; o mutante anterior agora é atingido.**

O caminho está conectado: `lista()` chama `_folga_nao_definida_sem_porta()`, esta chama `beco_em_lote()`, que constrói `InsumosDoDono` com `sem_motor=True, cxr=...` e chama `.dono(d)`. (`app/escala/services/cadastro_realidade.py:864`, `:573`, `:525`, `:531`)

A fixture verifica pergunta viva e ausência de porta; o teste exige exatamente o C11 daquele dia com dono `CADASTRO`, além de zero chamadas ao espelho e a `MotorBase.calcular_mes`. Uma exceção engolida por `lista()` retiraria o C11 esperado e falharia na asserção; o selo deixou de passar com o beco vazio. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:367`, `:371`, `:391`, `:393`; `app/escala/services/cadastro_realidade.py:866`)

Há uma distinção entre os dois casos:

- **Com lavra:** `_cxr` já contém o dia; `dono()` chega a `cxr()`, que devolve o conjunto carregado. (`app/ponto/management/commands/e6_oraculo.py:147`, `:163`, `:195`)
- **Sem lavra:** o juiz substituto devolve `ESTRUTURA`; `dono()` encontra `sem_motor=True` e devolve `None` antes de chamar `cxr()`. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:374`, `:395`; `app/ponto/management/commands/e6_oraculo.py:189`)

Portanto, remover os dois argumentos de `beco_em_lote:526` agora leva ao espelho sob o juiz forçado, contrariando as asserções de conteúdo e chamadas. É conclusão pelo fluxo; execução desse mutante no runner completo: **[nao verificado]**. (`app/escala/services/cadastro_realidade.py:526`; `app/ponto/management/commands/e6_oraculo.py:168`, `:190`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:391`)

**Existe mutação plausível sobrevivente aos dois cenários novos.** Antes da lógica atual de `InsumosDoDono.dono()`, introduzir:

```python
if self._sem_motor and self._cxr == set():
    self._cxr = None
    self._sem_motor = False
```

Isso introduz um fallback para uma lavra válida que respondeu “nenhum dia”. Os testes novos fornecem conjunto não vazio ou `None`; nenhum fornece conjunto vazio. Esse terceiro estado é produzido legitimamente por `cxr_lavrado()`. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:386`, `:397`; `app/ponto/services/cxr_na_lista.py:183`)

No ensaio isolado em memória dos métodos atuais e dessa alteração, os dois cenários novos mantiveram `CADASTRO/0 chamadas` e `None/0 chamadas`; o cenário coberto vazio, com juiz real respondendo `ESTRUTURA`, passou de zero para **uma chamada ao espelho**. Isso demonstra a lacuna dos cenários; aprovação do mutante pela classe Django completa: **[nao verificado]**. (`app/ponto/management/commands/e6_oraculo.py:162`, `:182`)

Também falta uma asserção direta de passagem pela autoridade: substituir a expressão `ins[...].dono(d)` por uma decisão local baseada na presença do dia na lavra reproduziria as duas respostas esperadas. Seria um segundo juiz; os dois resultados finais, sozinhos, não distinguem essa implementação. (`app/escala/services/cadastro_realidade.py:531`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:391`, `:398`)

**2) A fixture serve ao contrato do leitor, mas não representa uma lavra produzida integralmente naquele estado.**

O colaborador do beco é criado separadamente; as batidas da fixture base pertencem aos outros dois colaboradores. O teste injeta cobertura e item para o beco chamando `lavrar()` diretamente. Já `medir()` só percorre colaboradores encontrados em `Batida` dentro da competência. Assim, **esse snapshot não sairia de `medir()` com esse estado da fixture**. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:70`, `:77`, `:342`, `:386`; `app/ponto/services/cxr_na_lista.py:68`, `:71`)

A estrutura necessária ao leitor está correta: `cobertos` contém colaborador, competência e início; os itens contêm colaborador, competência e data. `cxr_lavrado()` usa precisamente esses campos. A escrita passa pela função canônica, embora o conteúdo seja montado pelo teste. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:386`; `app/ponto/services/cxr_na_lista.py:142`, `:176`, `:180`)

Há ainda uma artificialidade deliberada no juiz: o C11 exige `folga_nao_definida`, e o juiz real já responde `CADASTRO` por esse insumo, independentemente da lista do motor. O mock ignora isso para forçar a situação que exigiria consultar CXR. Logo, o teste sem lavra **não demonstra que um beco real dessa fixture receberia `None`**; demonstra o comportamento quando a lista seria necessária à decisão. (`app/escala/services/cadastro_realidade.py:518`; `app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:374`)

Isso dá mordida ao teste de proteção contra motor, mas limita seu alcance como prova de integração produtor → lavra → leitor. A correspondência com algum estado real de produção permanece **[nao verificado]**. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:384`)

**3) Relógio: datas de referência alinhadas; carimbo de persistência separado.**

| Trecho | Comportamento |
|---|---|
| Command | Lê `timezone.now()`, deriva `hoje` desse instante, passa `agora` a `medir` e `hoje` a `lavrar`. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:30`, `:40`) |
| `medir` | Deriva `hoje` de `agora`; usa esse dia nas competências e passa os mesmos valores ao espelho e a `InsumosDoDono`. (`app/ponto/services/cxr_na_lista.py:60`, `:63`, `:78`, `:99`) |
| Janelas | `janela_atual/anterior` calculam a partir da data recebida e do corte da empresa; não consultam relógio. (`app/ponto/janelas.py:60`, `:66`) |
| Espelho | Recebe `agora`, passa-o a `montar_dias`; a autoridade do cálculo recebe datas. A chamada a `calcular_mes` não recebe parâmetro `agora`. (`app/ponto/services/espelho.py:1227`, `:1262`; `app/ponto/services/fechamento.py:286`) |
| `InsumosDoDono` | Guarda o instante recebido; uma eventual consulta ao espelho o reutiliza. Na lavra, recebe também o espelho já calculado. (`app/ponto/management/commands/e6_oraculo.py:132`, `:169`; `app/ponto/services/cxr_na_lista.py:99`) |
| Snapshot | `calculado_em` tem `auto_now=True`; `lavrar` não o preenche com o instante da borda. (`app/inteligencia/models.py:96`; `app/ponto/services/cxr_na_lista.py:145`) |

**Sim:** no command atual, `data_ref`, o dia usado nas competências e o `agora` entregue ao espelho derivam do mesmo instante. **A expressão “agora do motor” é imprecisa:** o cálculo recebe datas; o instante explícito segue pelo espelho para a montagem da grade. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:30`; `app/ponto/services/cxr_na_lista.py:60`; `app/ponto/services/espelho.py:1266`; `app/escala/utils.py:2358`)

**Não há garantia de uma única leitura literal durante toda a persistência:** `calculado_em` é um carimbo automático de salvamento. Além disso, `ultimo_lavrado()` o publica como `medido_em`; esse campo não preserva o instante inicial passado ao espelho. (`app/inteligencia/models.py:96`; `app/ponto/services/cxr_na_lista.py:160`)

O teste de meia-noite tem mordida contra o defeito anterior: exige que todos os argumentos `agora` observados sejam exatamente `t1`, e compara seus dias com `data_ref`. Uma leitura extra depois da primeira não apaga essa exigência. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:412`, `:425`, `:427`)

Mas sua cobertura termina na entrada do espelho: ele substitui a função inteira e devolve lista vazia, impedindo também a criação de `InsumosDoDono` pelo serviço. Uma regressão dentro do espelho ficaria oculta desse teste; a catraca AST examina apenas o command e `cxr_na_lista`. Também não há asserção sobre `ini/fim`, e a virada escolhida é 05→06, sem mudança de competência. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:417`, `:421`, `:447`; `app/ponto/services/cxr_na_lista.py:81`, `:98`)

**4) Quem escreve e quem lê.**

| Elemento | Escritor/origem | Leitores/destino |
|---|---|---|
| `medir(agora=)` | O command captura e fornece o instante. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:30`) | O serviço deriva `hoje` e alimenta espelho e insumos. (`app/ponto/services/cxr_na_lista.py:60`, `:78`, `:99`) |
| `lavrar(d, hoje)` | Recebe a medida e escreve `data_ini`, `data_ref`, contadores e `detalhe` via `update_or_create`. (`app/ponto/services/cxr_na_lista.py:142`) | `ultimo_lavrado()` recupera o snapshot; C12 formata seus itens; `cxr_lavrado()` fornece dias aos leitores do dono. (`app/ponto/services/cxr_na_lista.py:152`, `:174`; `app/escala/services/cadastro_realidade.py:393`, `:523`, `:1021`) |
| `cobertos` | `medir()` acrescenta a chave somente depois de concluir a leitura do colaborador/competência; falhas saem antes. (`app/ponto/services/cxr_na_lista.py:111`, `:118`) | `cxr_lavrado()` distingue ausência de cobertura (`None`) de cobertura sem dias (`set()`). (`app/ponto/services/cxr_na_lista.py:176`, `:183`) |
| `CLASSIFICAR` | Registro estático no teste; a entrada nova pertence a **`B2_menos_B1`**. (`app/core/tests/test_medida_turno_do_par.py:391`, `:452`) | O teste compara esse registro ao censo calculado pelo instrumento. Não é allowlist de execução. (`app/core/tests/test_medida_turno_do_par.py:577`; `app/core/medida_turno_do_par.py:436`) |

No censo textual da cópia, o chamador encontrado de `cxr_na_lista.medir` é o command já atualizado; não encontrei chamada desse serviço com `hoje=`. O `medir(hoje=...)` de `lavrar_teto_do_dia` pertence a outro serviço. Chamadas externas à cópia: **[nao verificado]**. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:17`, `:32`; `app/ponto/management/commands/lavrar_teto_do_dia.py:28`)

**5) Classes de reprovação.**

| Classe | Parecer sobre a cópia |
|---|---|
| Segundo juiz | O código atual continua chamando a autoridade existente. O mock limita a capacidade dos novos testes de detectar sua substituição por decisão local. (`app/escala/services/cadastro_realidade.py:531`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:374`) |
| Segundo escritor fora da porta | O command e o teste novo usam `cxr_na_lista.lavrar`. A escrita direta da célula é preparação de fixture. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:40`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:366`, `:386`) |
| Relógio fora da borda | A dupla leitura command/serviço foi removida. O carimbo automático permanece; os testes não auditam dependências transitivas. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:30`; `app/inteligencia/models.py:96`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:447`) |
| Derivação paralela | A conversão de instante em data usa a mesma entrada; a inclusão em `CLASSIFICAR` é informativa, não cria autoridade. (`app/ponto/services/cxr_na_lista.py:60`; `app/core/tests/test_medida_turno_do_par.py:12`) |
| Selo sem mordida | A vacuidade anterior foi fechada. Continua faltando o cenário de cobertura vazia no request e a verificação do espelho real pelo selo temporal. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:391`, `:417`; `app/ponto/services/cxr_na_lista.py:184`) |
| Campo com dois significados | `None` e conjunto vazio estão separados corretamente no código. `medido_em` representa `calculado_em`, não o instante inicial da medição. (`app/ponto/services/cxr_na_lista.py:160`, `:184`) |

A documentação de L-040 ainda afirma ausência de leitor, embora C12 leia a lavra na implementação. O desenho `escopo_id=None` com a constraint anterior também permanece. Fechamento dos requisitos de produção — col841, divergência zero na frota, idempotência concorrente e conservação das classes do vigia — **[nao verificado]**. (`app/docs/LEIS.md:166`; `app/escala/services/cadastro_realidade.py:871`; `app/ponto/services/cxr_na_lista.py:146`; `app/inteligencia/models.py:103`)

## 08/11 03:36 · sessao 5b1d37290c70 · CXR-TELA-LE-O-MOTOR · verificador · origem=Code

**O delta cura (a)–(d) por inspeção. Não encontrei novo defeito bloqueante nos trechos alterados e nos leitores dos campos. O aval completo de produção permanece [não verificado].** Evidências abaixo.

**1) Curas e mapa de leitura/escrita**

- **(a) Chave corrigida:** os dois chamadores passam `ini`; C11 conserva esse início e corta somente `fim`. Isso elimina a escolha equivocada pelo mês do fim cortado. [cadastro_realidade.py:526](app/escala/services/cadastro_realidade.py:526), [cadastro_realidade.py:568](app/escala/services/cadastro_realidade.py:568), [cadastro_realidade.py:1025](app/escala/services/cadastro_realidade.py:1025).
- **(b) Selo fortalecido:** o teste agora exige contagem `(0, 0)` das chamadas ao espelho e ao motor; capturar o `AssertionError` dentro da lista não basta para deixá-lo passar. [test_cxr_tela_le_o_motor.py:132](app/ponto/tests/test_cxr_tela_le_o_motor.py:132), [test_cxr_tela_le_o_motor.py:138](app/ponto/tests/test_cxr_tela_le_o_motor.py:138).
- **(c) Testes passam pela lavra:** “responde como o espelho” chama efetivamente `cxr_lavrado`; o conflito C9/C12 exige `CADASTRO` em `dono_do_dia`, com juiz sensível ao insumo CXR. [test_cxr_tela_le_o_motor.py:252](app/ponto/tests/test_cxr_tela_le_o_motor.py:252), [test_cxr_tela_le_o_motor.py:311](app/ponto/tests/test_cxr_tela_le_o_motor.py:311).
- **(d) Texto corrigido:** a docstring declara filtro, deduplicação e exclusão por erro; o rótulo conta itens. [cxr_na_lista.py:11](app/ponto/services/cxr_na_lista.py:11), [cxr_na_lista.py:131](app/ponto/services/cxr_na_lista.py:131).

| Papel | Escritor/leitor |
|---|---|
| Produz competência e cobertura | `medir`: competência pelo fim integral; cobertura `[pk, comp, início ISO]`. [cxr_na_lista.py:61](app/ponto/services/cxr_na_lista.py:61), [cxr_na_lista.py:115](app/ponto/services/cxr_na_lista.py:115) |
| Persiste e recupera | `lavrar` grava o dicionário em `detalhe`; `ultimo_lavrado` recupera-o. [cxr_na_lista.py:142](app/ponto/services/cxr_na_lista.py:142), [cxr_na_lista.py:156](app/ponto/services/cxr_na_lista.py:156) |
| Interpreta `cobertos` | `cxr_lavrado`: `(colab, início) → competência → conjunto de datas`. [cxr_na_lista.py:173](app/ponto/services/cxr_na_lista.py:173), [cxr_na_lista.py:180](app/ponto/services/cxr_na_lista.py:180) |
| Consome a chave nova | `beco_em_lote` e `um_dono_por_dia`. [cadastro_realidade.py:526](app/escala/services/cadastro_realidade.py:526), [cadastro_realidade.py:1025](app/escala/services/cadastro_realidade.py:1025) |
| Mostra C12 | `_cxr_do_motor` lê `itens`, sem interpretar `cobertos`. [cadastro_realidade.py:393](app/escala/services/cadastro_realidade.py:393) |

**Nenhum leitor produtivo encontrado ainda exige o par `[colab, comp]**`: o leitor estrutural exige três elementos. O par antigo aparece no teste negativo e resulta em `None`; portanto, snapshots antigos perdem cobertura até nova lavra, explicitamente. [cxr_na_lista.py:173](app/ponto/services/cxr_na_lista.py:173), [test_cxr_tela_le_o_motor.py:305](app/ponto/tests/test_cxr_tela_le_o_motor.py:305).

**2) Descartes versus aval**

Não promoveria os descartes funcionais a bloqueadores deste delta. Em particular, erro parcial fica contado, exclui cobertura e o comando publica antes de sair 2; sem cobertura, o ramo dependente de CXR retorna `None`, sem motor. [cxr_na_lista.py:108](app/ponto/services/cxr_na_lista.py:108), [lavrar_cxr_do_motor.py:37](app/ponto/management/commands/lavrar_cxr_do_motor.py:37), [e6_oraculo.py:189](app/ponto/management/commands/e6_oraculo.py:189).

**A pendência `l084_pausa` impede declarar comprovado o PRONTO (1).** O leitor copia a publicação; não consegue trazer uma acusação que o produtor não entregou. Isso não demonstra defeito novo no leitor nem autoriza recalcular na tela. Os sete dias efetivamente publicados e exibidos: [não verificado]. [cxr_na_lista.py:75](app/ponto/services/cxr_na_lista.py:75), [motor_calculo_v2.py:2719](app/ponto/motor_calculo_v2.py:2719).

**3) Outro ponto relevante ao aceite**

O PRONTO (5) exige cuidado: **nem toda saída de `d` vai para `e/dia_na_lista_dono_*`**. Dias reconhecidos como mesa do admin podem entrar antes em `fio_do_admin`; os demais dias presentes na lista chegam ao detalhamento de `e`. Portanto, afirmar fechamento somente em `e` sem comparar os destinos seria incorreto. [vigia_fio.py:654](app/chamados/services/vigia_fio.py:654), [vigia_fio.py:677](app/chamados/services/vigia_fio.py:677), [vigia_fio.py:684](app/chamados/services/vigia_fio.py:684). A comparação nominal antes/depois e sua soma: [não verificado].

Executei sete verificações puras da função de leitura, incluindo competências distintas, cobertura vazia, ausência e formato antigo: passaram. [cxr_na_lista.py:162](app/ponto/services/cxr_na_lista.py:162). Suite Django e selos da frota: [não verificado].

## 08/11 03:35 · sessao aab5013a25cc · - · - · origem=Ronald (tmux)

Não encontrei descarte que deva virar **defeito bloqueante novo** no delta; os pontos abaixo limitam a conclusão de aceite:

- **Erro parcial:** CXR exclui a competência com falha de `cobertos`, registra erro e publica as demais; o command grava e termina com código 2. É a mesma política do C9. Sem cobertura, `sem_motor=True` devolve `None` quando a lista do motor seria necessária, sem chamá-lo. `app/ponto/services/cxr_na_lista.py:108`, `app/ponto/services/cxr_na_lista.py:115`, `app/ponto/management/commands/lavrar_cxr_do_motor.py:37`, `app/ponto/management/commands/lavrar_teto_do_dia.py:34`, `app/ponto/management/commands/e6_oraculo.py:189`.

- **Zero/não medido:** `ultimo_lavrado()` distingue ausência de medição, mas C12 só consome `itens`, perdendo essa distinção na apresentação. Isso permanece no molde já escolhido; não encontrei requisito específico novo que transforme o descarte em bloqueio do delta. `app/ponto/services/cxr_na_lista.py:154`, `app/escala/services/cadastro_realidade.py:393`.

- **Item sem dia:** nasce sem dono na lavra, e a ação continuará “a próxima lavra o traz”; próxima lavra não necessariamente resolverá enquanto o motor continuar sem dia. É limitação real já descartada, compartilhada pela apresentação por dono. A afirmação “sem_dia=0 na frota” não pode ser validada pelos anexos fornecidos. `app/ponto/services/cxr_na_lista.py:81`, `app/ponto/services/cxr_na_lista.py:84`, `app/escala/services/cadastro_realidade.py:759`; número de produção [não verificado].

- **Cron:** a lavra está declarada como elo após `vigia_pareador_celula`, com dependência de `processar_cartorio`; a leitura da manhã usa a lavra anterior por decisão documentada no código. Não há evidência nova para promover a enumeração incompleta do placar a bloqueio funcional deste delta. `app/config/crons.py:1757`, `app/config/crons.py:1765`.

- **`l084_pausa`/PRONTO (1):** os escritores examinados publicam `l084_do_dia`, as causas de janela ou item sem `causa`; C12 copia a publicação, portanto não pode produzir por si os sete exemplos ausentes. O atendimento nominal dos sete dias permanece [não verificado], sem caso fornecido que demonstre falha deste leitor. `app/ponto/motor_calculo_v2.py:1623`, `app/ponto/motor_calculo_v2.py:1670`, `app/ponto/motor_calculo_v2.py:1992`, `app/ponto/motor_calculo_v2.py:2719`, `app/ponto/services/cxr_na_lista.py:75`.

**Cuidado com o PRONTO (5):** não declarar genericamente que cada saída de `d` aparecerá em `e/dia_na_lista_dono_*`. O vigia encaminha previamente dias na lista com BATIDA/CADASTRO para `fio_do_admin`; somente os que permanecem sem outro balde podem entrar em `e/dia_na_lista_dono_*`. A nova C12 calcula o dono com o próprio espelho e pode devolver CADASTRO. A soma exigida precisa de comparação nominal antes/depois; ela não está nos anexos. `app/chamados/services/vigia_fio.py:313`, `app/chamados/services/vigia_fio.py:654`, `app/chamados/services/vigia_fio.py:677`, `app/chamados/services/vigia_fio.py:684`, `app/ponto/services/cxr_na_lista.py:96`, `app/ponto/management/commands/e6_oraculo.py:110`; fechamento da soma [não verificado].

## 08/11 03:35 · sessao 8fade55f0993 · - · - · origem=Ronald (tmux)

Não achei regressão concreta na chave nova nem leitor produtivo que ainda desempacote `cobertos` como par; o único leitor estrutural exige três elementos. `app/ponto/services/cxr_na_lista.py:173`.

Mapa dos campos:

- `medir()` produz `comp` pelo fim integral e grava `[pk, comp, ini.isoformat()]`; devolve `cobertos` no resultado. `app/ponto/services/cxr_na_lista.py:61`, `app/ponto/services/cxr_na_lista.py:115`, `app/ponto/services/cxr_na_lista.py:130`.
- `lavrar()` persiste o resultado inteiro em `detalhe`; `ultimo_lavrado()` recupera esse detalhe. `app/ponto/services/cxr_na_lista.py:142`, `app/ponto/services/cxr_na_lista.py:156`.
- `cxr_lavrado()` converte os triplos em `(colab, início) → competência` e busca os dias por `(colab, competência)`. Formatos antigos ficam sem cobertura, retornando `None`. `app/ponto/services/cxr_na_lista.py:173`, `app/ponto/services/cxr_na_lista.py:177`, `app/ponto/services/cxr_na_lista.py:180`.
- Os dois consumidores produtivos passam `ini`: C11 em `beco_em_lote()` e reconciliação em `um_dono_por_dia()`. `app/escala/services/cadastro_realidade.py:526`, `app/escala/services/cadastro_realidade.py:1025`.
- C11 preserva o início da janela e corta somente o fim; a reconciliação deriva a janela da data do item pela autoridade `janela_atual`. `app/escala/services/cadastro_realidade.py:568`, `app/escala/services/cadastro_realidade.py:569`, `app/escala/services/cadastro_realidade.py:1017`.
- `InsumosDoDono` recebe o conjunto ou `None`; sem cobertura e sem motor, o ramo que precisaria do CXR devolve `None`, sem fallback. `app/ponto/management/commands/e6_oraculo.py:147`, `app/ponto/management/commands/e6_oraculo.py:189`.
- C12 lê apenas `itens`, sem interpretar `cobertos`. `app/escala/services/cadastro_realidade.py:393`.
- O único par antigo explícito é a fixture negativa que exige `None`; os testes novos também distinguem os inícios das competências 10 e 11. `app/ponto/tests/test_cxr_tela_le_o_motor.py:302`, `app/ponto/tests/test_cxr_tela_le_o_motor.py:305`.
- O homônimo `cobertos` de `core/views.py` é um contador de postos, independente do snapshot CXR. `app/core/views.py:882`, `app/core/views.py:891`.

Execução dos testes e estado efetivo dos snapshots de produção: [não verificado]. [nao verificado]

## 08/11 03:19 · sessao d8ec781d1406 · CXR-TELA-LE-O-MOTOR · verificador · origem=Code

**Não aprovo a v1 como pronta.** Há publicação de lavra parcial como execução concluída, lacunas nos selos e ausência de prova do caso obrigatório de pausa. O caminho principal da lista, entretanto, passou a usar `sem_motor=True`. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:37`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:130`; `app/escala/services/cadastro_realidade.py:525`; `app/escala/services/cadastro_realidade.py:1024`)

Verificação estática, com contraprova da seleção de competência executada somente em memória. Resultado de suíte, sete dias de col841, equivalência na frota e fechamento da soma d→e: **[nao verificado]**. [nao verificado]

**1. Escritores, leitores e significado dos campos**

O percurso é motor → resultado do espelho → `medir` → `lavrar` → snapshot → formatador C12/portador do dono → lista/PDF/vigia. O resultado entregue pelo espelho vem da janela **visual**, `vis_ini..hoje`, e não exclusivamente de `apur_ini..apur_fim`. (`app/ponto/motor_calculo_v2.py:2719`; `app/ponto/services/espelho.py:1225`; `app/ponto/services/espelho.py:1227`; `app/ponto/services/espelho.py:1294`; `app/ponto/services/cxr_na_lista.py:71`; `app/escala/services/cadastro_realidade.py:393`)

| Campo | Quem escreve; quem lê; significado |
|---|---|
| Snapshot `chave`, `escopo`, `escopo_id` | `lavrar`: `cxr_do_motor`, `frota`, `None`. `ultimo_lavrado` procura chave/escopo, sem filtrar `escopo_id`. (`app/ponto/services/cxr_na_lista.py:137`; `app/ponto/services/cxr_na_lista.py:147`) |
| `data_ini`, `data_ref` | `lavrar` grava **a data da medição** nos dois; não são os limites das competências. `ultimo_lavrado` ordena por `data_ref` e a devolve como texto ISO. (`app/ponto/services/cxr_na_lista.py:138`; `app/ponto/services/cxr_na_lista.py:148`; `app/ponto/services/cxr_na_lista.py:153`) |
| `numerador`, `denominador`, `valor`, `detalhe` | `lavrar` grava respectivamente `total`, `universo`, `None` e o dicionário inteiro. Portanto numerador é quantidade de itens distintos; denominador é quantidade de colab–competência examinados, não dias nem colaboradores únicos. (`app/ponto/services/cxr_na_lista.py:65`; `app/ponto/services/cxr_na_lista.py:118`; `app/ponto/services/cxr_na_lista.py:139`) |
| `calculado_em`, `medido_em` | O modelo atualiza `calculado_em` ao salvar; `ultimo_lavrado` o expõe como `medido_em`. É carimbo da gravação, posterior à captura `_agora_do_ato`. (`app/inteligencia/models.py:96`; `app/ponto/services/cxr_na_lista.py:52`; `app/ponto/services/cxr_na_lista.py:152`) |
| `itens[].colab`, `empresa` | `medir` acrescenta os PKs do colaborador e de sua empresa na seleção. O formatador agrupa por `colab`; a filtragem final da lista usa a empresa **atual**, carregada por `_vinculos`, e não `itens[].empresa`. (`app/ponto/services/cxr_na_lista.py:61`; `app/ponto/services/cxr_na_lista.py:77`; `app/escala/services/cadastro_realidade.py:401`; `app/escala/services/cadastro_realidade.py:769`; `app/escala/services/cadastro_realidade.py:899`) |
| `itens[].competencia` | `medir` escreve `MM/AAAA` do fim da janela canônica. Formatador usa no detalhe; `cxr_lavrado` usa na chave de consulta. O corte vem da empresa; 21→20 é o default, não constante do serviço novo. (`app/ponto/services/cxr_na_lista.py:30`; `app/ponto/services/cxr_na_lista.py:57`; `app/ponto/services/cxr_na_lista.py:170`; `app/ponto/janelas.py:36`; `app/escala/services/cadastro_realidade.py:404`) |
| `itens[].data` | `medir` converte `dia` em ISO e exige intervalo **inclusivo** `ini <= d <= fim`; ausência de `dia` permanece `None`. Lista, reconciliação e vigia usam essa data como identidade do dia. (`app/ponto/services/cxr_na_lista.py:80`; `app/ponto/services/cxr_na_lista.py:88`; `app/ponto/services/cxr_na_lista.py:95`; `app/escala/services/cadastro_realidade.py:1003`; `app/escala/services/cadastro_realidade.py:1061`) |
| `ec`, `template_id` | Acrescentados pela lavra consultando o vínculo **do dia**. O formatador renomeia para `ec_do_dia`/`template_do_dia`; a ação C12 usa o template histórico. O agrupamento e o template mostrado na linha continuam sendo os de hoje. (`app/ponto/services/cxr_na_lista.py:94`; `app/escala/services/cadastro_realidade.py:402`; `app/escala/services/cadastro_realidade.py:923`; `app/escala/services/cadastro_realidade.py:782`) |
| `dono` | Acrescentado pela lavra mediante `InsumosDoDono.dono(d)`, usando o espelho já obtido. O formatador o transforma em `dono_do_dia`; a reconciliação pode posteriormente sobrescrevê-lo. (`app/ponto/services/cxr_na_lista.py:92`; `app/ponto/services/cxr_na_lista.py:96`; `app/escala/services/cadastro_realidade.py:403`; `app/escala/services/cadastro_realidade.py:1028`) |
| `causa`, `real`, `cadastrado`, `minutos_fora` | O produtor HE escreve causa, horários locais `HH:MM` e minutos arredondados. `minutos_fora` é a **sobra após extra declarada**, enquanto `cadastrado` continua sendo o marco cru. A lavra copia; o formatador imprime. (`app/ponto/motor_calculo_v2.py:1662`; `app/ponto/motor_calculo_v2.py:1670`; `app/ponto/motor_calculo_v2.py:1702`; `app/ponto/services/cxr_na_lista.py:79`; `app/escala/services/cadastro_realidade.py:394`) |
| `entrada_real`, `entrada_cadastrada`, `saida_real`, `saida_cadastrada` | Os produtores L-084 escrevem instantes ISO com offset, e não apenas `HH:MM`. Lavra copia como texto; formatador imprime sem conversão. (`app/ponto/motor_calculo_v2.py:1624`; `app/ponto/motor_calculo_v2.py:1998`; `app/ponto/services/cxr_na_lista.py:39`; `app/escala/services/cadastro_realidade.py:398`) |
| `delta_entrada_min`, `delta_saida_min` | Minutos com sinal, arredondados a uma decimal: entrada = real − prevista; saída = prevista − real. Logo saída posterior produz delta negativo. Copiados pela lavra e impressos pelo formatador. (`app/ponto/motor_calculo_v2.py:1973`; `app/ponto/motor_calculo_v2.py:1975`; `app/ponto/motor_calculo_v2.py:2002`; `app/escala/services/cadastro_realidade.py:399`) |
| `cobertos` | `medir` escreve `[colab, competencia]` somente após concluir aquela leitura, inclusive quando nenhum item sobra. `cxr_lavrado` distingue coberto vazio (`set()`) de não coberto (`None`). (`app/ponto/services/cxr_na_lista.py:104`; `app/ponto/services/cxr_na_lista.py:110`; `app/ponto/services/cxr_na_lista.py:166`; `app/ponto/services/cxr_na_lista.py:174`) |
| `fora_da_competencia` | Conta ocorrências descartadas pelo filtro, por leitura de competência. O command imprime; o produto C12 não expõe esse contador. Não representa quantidade de dias distintos. (`app/ponto/services/cxr_na_lista.py:88`; `app/ponto/services/cxr_na_lista.py:109`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:33`; `app/escala/services/cadastro_realidade.py:393`) |
| `repetidos_do_motor` | Conta itens iguais **depois da projeção/conversão**, dentro de um colab–competência. O command imprime; a lista recebe somente o item deduplicado. (`app/ponto/services/cxr_na_lista.py:79`; `app/ponto/services/cxr_na_lista.py:100`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:33`) |
| `total`, `colabs`, `universo`, `sem_dia` | `medir` calcula itens distintos, colaboradores com itens, colab–competência selecionados e itens sem data. O command imprime. O contador C12 da tela é recontado sobre assinaturas efetivamente exibidas. (`app/ponto/services/cxr_na_lista.py:118`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:30`; `app/escala/services/cadastro_realidade.py:933`; `app/escala/services/cadastro_realidade.py:960`) |
| `por_competencia`, `por_dono`, `por_causa` | Agregações dos itens sobreviventes, escritas por `medir`; command imprime as duas primeiras. Causa ausente ganha o rótulo `l084_sem_causa` somente na agregação. (`app/ponto/services/cxr_na_lista.py:111`; `app/ponto/services/cxr_na_lista.py:115`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:32`) |
| `janelas`, `competencias`, `erros`, `erros_amostra`, `rotulo`, `onde_olha` | Escritos por `medir`, armazenados no detalhe. Command usa `erros` para retornar 2; formatador C12 lê apenas `itens`, sem levar cobertura, erro ou frescura à saída da lista. (`app/ponto/services/cxr_na_lista.py:58`; `app/ponto/services/cxr_na_lista.py:121`; `app/ponto/services/cxr_na_lista.py:126`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:39`; `app/escala/services/cadastro_realidade.py:393`) |
| `sem_medida` | Produzido por `ultimo_lavrado` quando não há snapshot; ignorado pelo formatador C12. (`app/ponto/services/cxr_na_lista.py:149`; `app/escala/services/cadastro_realidade.py:393`) |

`InsumosDoDono._cxr` nasce `None`, recebe conjunto de datas por injeção ou pelo espelho e é consultado por pertinência em `dono`. `_sem_motor` é escrito pelo construtor e lido nas duas guardas. Sem cobertura, outros insumos ainda podem decidir CADASTRO/BATIDA; apenas a resposta residual que dependeria de CXR fica `None`. (`app/ponto/management/commands/e6_oraculo.py:133`; `app/ponto/management/commands/e6_oraculo.py:146`; `app/ponto/management/commands/e6_oraculo.py:170`; `app/ponto/management/commands/e6_oraculo.py:189`)

| Assinatura | Escritor original de `dono_do_dia` |
|---|---|
| C9 | Serviço do teto escreve `dono`; formatador renomeia. (`app/ponto/services/teto_na_lista.py:86`; `app/escala/services/cadastro_realidade.py:379`) |
| C10 | Censo pergunta ao mesmo portador com os dias de fase da própria corrida e grava `dono_do_dia`. (`app/ponto/management/commands/censo_fase_12x36.py:287`; `app/ponto/management/commands/censo_fase_12x36.py:300`) |
| C11 | Beco pergunta ao portador durante a leitura; não é dono previamente lavrado diariamente. (`app/escala/services/cadastro_realidade.py:531`) |
| C12 | Lavra pergunta ao portador com o espelho da própria medição; formatador renomeia. (`app/ponto/services/cxr_na_lista.py:92`; `app/ponto/services/cxr_na_lista.py:96`; `app/escala/services/cadastro_realidade.py:403`) |

Todos podem ser sobrescritos por `um_dono_por_dia`: reaproveita C11 ou pergunta novamente ao juiz. Assim, na saída, `dono_do_dia` significa **dono reconciliado**, não necessariamente o dono originalmente gravado naquele snapshot. Ações, contadores e vigia leem esse resultado. (`app/escala/services/cadastro_realidade.py:1009`; `app/escala/services/cadastro_realidade.py:1026`; `app/escala/services/cadastro_realidade.py:1028`; `app/escala/services/cadastro_realidade.py:923`; `app/escala/services/cadastro_realidade.py:1063`)

**Não há significado temporal inteiramente uniforme:** HE escreve `entrada.date()`, enquanto L-084 escreve o dia atribuído pelo juiz. Além disso, motor usa `settings.TIME_ZONE`; lavra/portador usam timezone corrente. O default é São Paulo; divergência efetiva de fuso em produção é **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:1528`; `app/ponto/motor_calculo_v2.py:1997`; `app/ponto/motor_calculo_v2.py:73`; `app/ponto/management/commands/e6_oraculo.py:149`; `app/config/settings/base.py:93`)

**2. Caminhos de request e leitores da lista**

**No grafo da lista CxR, não encontrei caminho sobrevivente que execute o espelho/motor.** As duas montagens potencialmente perigosas usam `sem_motor=True`, cuja guarda antecede a chamada ao espelho. (`app/escala/services/cadastro_realidade.py:525`; `app/escala/services/cadastro_realidade.py:1024`; `app/ponto/management/commands/e6_oraculo.py:166`)

Caminhos localizados:

- View HTML/PDF → `lista`; PDF recebe os dados montados e renderiza. (`app/escala/views.py:487`; `app/escala/views.py:489`; `app/escala/services/cadastro_realidade.py:1067`)
- `lista` → C11 → `beco_em_lote` → portador sem motor. (`app/escala/services/cadastro_realidade.py:864`; `app/escala/services/cadastro_realidade.py:573`; `app/escala/services/cadastro_realidade.py:525`)
- `lista` → `um_dono_por_dia` → portador sem motor. (`app/escala/services/cadastro_realidade.py:885`; `app/escala/services/cadastro_realidade.py:1024`)
- `vigiar_fio_morto` → `ListaDoAdmin` → `lista` → `dias_na_lista`. (`app/chamados/management/commands/vigiar_fio_morto.py:167`; `app/chamados/services/vigia_fio.py:216`; `app/chamados/services/vigia_fio.py:217`)
- Commands `fechar_avisos_regua_na_lista` e `fechar_chamados_vinculo_na_lista` → mesma `lista`. (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`)
- Consumidores externos do beco: pauta de competência exportada e reatador chegam ao mesmo serviço alterado. (`app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:105`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:54`)

Fora desse grafo, as próprias telas de espelho continuam chamando `espelho_do_colab`; portanto “zero motor em qualquer request do sistema” seria falso. (`app/ponto/views.py:158`; `app/api/views.py:1369`)

**3. Classes de reprovação**

- **Segundo juiz de dono:** não identifiquei duplicação da regra CADASTRO/BATIDA/ESTRUTURA; ela continua no juiz existente. Os novos ramos mudam a obtenção do insumo e a resposta desconhecida. (`app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/management/commands/e6_oraculo.py:110`; `app/ponto/management/commands/e6_oraculo.py:195`)
- **Segundo escritor:** o escritor localizado para a nova chave é `cxr_na_lista.lavrar`, chamado pelo command. Ele não escreve a lista interna do motor. A frase do modelo “escrito só por inteligencia.metricas” ficou documentalmente incompatível com esse padrão, que já é usado por C9. (`app/ponto/services/cxr_na_lista.py:134`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:37`; `app/inteligencia/models.py:82`; `app/ponto/services/teto_na_lista.py:113`)
- **Relógio fora da borda:** o command captura `localdate()` e o serviço captura outro `now()`. Caso: atravessar 20→21 entre as chamadas mantém competência/data_ref de ontem com instante de hoje. A cadeia também conserva o relógio próprio de C2 e outro no PDF. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:28`; `app/ponto/services/cxr_na_lista.py:52`; `app/escala/services/cadastro_realidade.py:252`; `app/escala/services/cadastro_realidade.py:1076`)
- **Derivação paralela:** C12 não recalcula distância, mas filtra, projeta e deduplica a publicação. Portanto “item por item, sem filtro e sem conta” não descreve o código. O censo `CLASSIFICAR` acrescentado pelo diff é informativo, não certificado dessa equivalência. (`app/ponto/services/cxr_na_lista.py:11`; `app/ponto/services/cxr_na_lista.py:79`; `app/ponto/services/cxr_na_lista.py:88`; `app/ponto/services/cxr_na_lista.py:100`; `app/core/tests/test_medida_turno_do_par.py:12`)
- **Ordem de trava:** o elo toma o `flock`, verifica conclusão própria e espera o predecessor antes de executar. Não identifiquei inversão nova nesse caminho; concorrência entre execuções manuais e cron não está provada pelo teste sequencial. (`bin/encadeado.sh:78`; `bin/encadeado.sh:82`; `bin/encadeado.sh:89`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:118`)
- **Pouso antes do pré-requisito:** o corte exige acusador de pausa no motor, e o golden da obra é `l084_pausa`; os produtores localizados nesta cópia são HE, `l084_do_dia` e L-084 sem causa. Os sete dias em produção permanecem **[nao verificado]**. (`app/docs/CORTES.md:304`; `app/docs/BACKLOG.md:775`; `app/ponto/motor_calculo_v2.py:1623`; `app/ponto/motor_calculo_v2.py:1670`; `app/ponto/motor_calculo_v2.py:1992`)
- **Campo com dois significados:** há `dia` civil versus dia do turno; `fim` completo versus fim cortado; dono original versus reconciliado. O segundo caso tem contraprova abaixo. (`app/ponto/motor_calculo_v2.py:1528`; `app/ponto/motor_calculo_v2.py:1997`; `app/escala/services/cadastro_realidade.py:569`; `app/ponto/services/cxr_na_lista.py:173`; `app/escala/services/cadastro_realidade.py:1028`)

Os testes **não mordem tudo o que anunciam**: [nao verificado]

- O selo sem motor arma `AssertionError`, mas a lista captura `Exception`, inclusive em C11. Uma chamada indevida nesse ramo pode ser engolida, C11 desaparecer e o teste continuar verde porque exige apenas C12 e um PDF válido. Não há `assert_not_called`. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:130`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:134`; `app/escala/services/cadastro_realidade.py:866`)
- “`cxr_lavrado` responde como o espelho” **não chama `cxr_lavrado`**: injeta manualmente o mesmo dia nas duas montagens. Não testa `cobertos`, filtro ou competência. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:284`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:286`)
- O teste real cobre HE 18:56×21:00; os demais usam publicações artificiais. Não certifica pausa nem os sete dias obrigatórios. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:92`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:154`)
- Idempotência compara quantidade de snapshots e `itens`; o selo PDF procura cada data esperada no texto, sem excluir datas extras. São verificações parciais das promessas “mesmo estado” e “mesmos dias”. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:122`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:151`)

**4. Filtro, cobertura e equivalência para `dono()`**

**Os conjuntos completos não são iguais.** Para a mesma publicação, o espelho fornece todos os dias; a lavra conserva a interseção com a competência. Entretanto, para um dia consultado **dentro daquela competência**, o filtro sozinho preserva a pertinência. Descartar agosto não é contraexemplo suficiente para `dono(outubro)`. (`app/ponto/management/commands/e6_oraculo.py:170`; `app/ponto/services/cxr_na_lista.py:88`; `app/ponto/management/commands/e6_oraculo.py:195`)

Em `um_dono_por_dia`, a janela vem de `janela_atual(dia)`. Logo há equivalência de pertinência **condicionada à mesma publicação, cobertura e identidade de competência**; o código não prova equivalência entre a lavra diária e um espelho recalculado posteriormente. (`app/escala/services/cadastro_realidade.py:1017`; `app/ponto/services/cxr_na_lista.py:147`; `app/ponto/services/cxr_na_lista.py:174`)

**Contraexemplo efetivo da API:** em 25/10, C11 corta a janela corrente para 21/10–24/10 e passa esse `fim`. `cxr_lavrado` busca `10/2026`, embora a competência corrente termine em 20/11. Com itens 02/10 na competência 10 e 22/10 na 11, a consulta retorna `{02/10}` em vez de `{22/10}`. (`app/escala/services/cadastro_realidade.py:568`; `app/escala/services/cadastro_realidade.py:569`; `app/escala/services/cadastro_realidade.py:526`; `app/ponto/services/cxr_na_lista.py:173`)

**Isso não muda hoje o dono de C11:** o beco exige `folga_nao_definida=True`, e esse insumo já decide CADASTRO antes de CXR. É erro de contrato mascarado pela precedência atual. (`app/escala/services/cadastro_realidade.py:518`; `app/ponto/management/commands/e6_oraculo.py:98`)

Há ainda diferença de universo: cobertura exige batida no intervalo civil `[ini 00h, fim+1 00h)`, enquanto os portadores/autoridade carregam bordas e o juiz pode atribuir batidas à véspera. Assim, cobertura civil não prova cobertura de todos os dias de turno. Um caso real afetado em produção é **[nao verificado]**. (`app/ponto/services/cxr_na_lista.py:59`; `app/ponto/services/cxr_na_lista.py:61`; `app/ponto/management/commands/e6_oraculo.py:150`; `app/ponto/services/espelho.py:1020`; `app/ponto/turnos.py:306`)

**5. Contratos do ENC e registros**

A declaração está integrada: gatilho `vigia_pareador_celula`, dependência `processar_cartorio`, estágio `auditoria`, `--apply` e papel `lavra`. O teto 900 limita a espera pelo predecessor, não a duração do próprio lavrador. (`app/config/crons.py:1765`; `app/config/crons.py:2232`; `bin/encadeado.sh:92`; `bin/encadeado.sh:102`)

Não falta registrar em `FORA_DE_PIPELINE`: `todas_as_entradas()` já inclui `ENCADEADOS`, e o contrato “command tem casa” usa essa fonte. O contrato de duração também alcança encadeados e exige ausência de `sched`. (`app/config/crons.py:1783`; `app/chamados/tests/test_contract_crons.py:220`; `app/core/tests/test_contract_crons_duracao_horario.py:43`)

**Falta cobertura no consumidor `pipeline_placar`:** ele enumera somente `CRONS`. Caso concreto: o novo lavrador nun

[truncado em 20000 de 23391 caracteres]

## 08/11 03:15 · sessao 326fdbbc8265 · - · - · origem=Ronald (tmux)

Mapeamento e achados para consolidação:

- O resultado consumido **não é recortado à competência pedida**: `espelho_do_colab` chama `autoridade_do_periodo(colaborador, vis_ini, hoje)` e devolve seu `resultado`; apenas outras partes visuais/totais recebem `apur_ini/apur_fim`. Portanto o filtro novo é uma transformação efetiva da publicação, não cópia literal. Fontes: `app/ponto/services/espelho.py:1225`, `:1227`, `:1229`, `:1294`; filtro em `app/ponto/services/cxr_na_lista.py:88`.

- **O campo `dia` mistura dia civil e dia do turno**: HE usa `entrada.date()` e publica `j['dia']`; L084 publica o mapa do juiz ou `_dt_b`. A lavra interpreta todos como mesmo `data`, competência e vínculo. Fontes: `app/ponto/motor_calculo_v2.py:1528`, `:1711`, `:1749`, `:1623`, `:1997`; `app/ponto/services/cxr_na_lista.py:80`, `:88`, `:94`. Caso possível: cauda `E03:00→S11:00` em 21/10 de jornada iniciada em 20/10, cadastro 21–09, saída acima do teto; a acusação HE usa 21/10 embora o turno possa pertencer a 20/10. A herança da jornada e o mapa existem no próprio motor: `app/ponto/motor_calculo_v2.py:2403`, `:2407`, `:2455`. Execução desse cenário completo `[nao verificado]`.

- **Não usar a remoção dos dias externos como contraexemplo isolado de `dono(d)`**: para mesma publicação e `d∈[ini,fim]`, o filtro preserva pertinência. `cxr()` retorna os dias de todo o espelho, enquanto a lavra guarda a interseção; a diferença fora da janela não muda a pergunta interna. Fontes: `app/ponto/management/commands/e6_oraculo.py:170`, `:195`; `app/ponto/services/cxr_na_lista.py:88`, `:170`, `:174`.

- **Contraexemplo concreto da API C11**: em 25/10, a janela corrente 21/10–20/11 vira 21/10–24/10; `cxr_do(cid,fim)` procura `10/2026`, embora o dia 22/10 pertença a `11/2026`. Com lavra coberta nas duas competências e item somente 22/10 na 11, a função devolve `[]` para fim24/10 e `['2026-10-22']` para fim20/11. Executei somente a função extraída por AST com `python3 -B`, sem Django ou escrita. Fontes: `app/escala/services/cadastro_realidade.py:569`, `:525`; `app/ponto/services/cxr_na_lista.py:173`. **Impacto no dono C11 é neutralizado hoje**: o chamador exige `folga_nao_definida`, e o juiz retorna CADASTRO antes de ler CXR. Fontes: `app/escala/services/cadastro_realidade.py:518`; `app/ponto/management/commands/e6_oraculo.py:98`.

- **Cobertura também usa outra geometria**: universo da lavra exige qualquer batida no intervalo civil `[ini00h, (fim+1)00h)`, enquanto Insumos e autoridade incluem bordas e atribuem dia pelo turno. Um colaborador com todas as batidas em 21/10, atribuídas ao turno20/10, pode ficar sem cobertura na competência10 apesar de existir dia20 no espelho. A ocorrência desse caso em produção `[nao verificado]`. Fontes: `app/ponto/services/cxr_na_lista.py:59`, `:61`, `:64`, `:110`; `app/ponto/management/commands/e6_oraculo.py:150`; `app/ponto/services/espelho.py:1019`; atribuição à véspera em `app/ponto/turnos.py:299`, `:306`.

- **Unidades e formatos**: HE publica `real/cadastrado` como `HH:MM`, `minutos_fora` inteiro arredondado e já descontada a extra declarada; `cadastrado` continua marco cru. L084 publica timestamps ISO locais com offset e deltas em minutos com uma decimal; entrada é `real-prevista`, saída é `prevista-real`. Fontes: `app/ponto/motor_calculo_v2.py:1670`, `:1662`, `:1702`, `:1742`, `:1624`, `:1974`, `:1976`, `:1998`. Fuso do motor é fixo `settings.TIME_ZONE`, configurado São Paulo; lavra e Insumos usam o timezone corrente. Divergência efetiva de timezone em produção `[nao verificado]`. Fontes: `app/ponto/motor_calculo_v2.py:28`, `:73`; `app/config/settings/base.py:93`; `app/ponto/services/cxr_na_lista.py:59`; `app/ponto/management/commands/e6_oraculo.py:149`.

- **Golden de pausa sem produtor demonstrado**: não há `l084_pausa` nem `acusar_pausa_l084` em Python de produção desta cópia, pela busca completa de `app`. Os escritores efetivos encontrados publicam `l084_do_dia`, HE e L084 sem causa; `resultado` recebe a lista no final. Fontes positivas: `app/ponto/motor_calculo_v2.py:1623`, `:1670`, `:1992`, `:2719`. O prerequisite é nomeado em `app/docs/CORTES.md:304` e o golden em `app/docs/BACKLOG.md:775`; o relato de commit anterior não prova que esse código esteja nesta cópia (`app/docs/RELATO.md:1778`). Existência dos sete dias em produção `[nao verificado]`.

- **Teste real não cobre pausa**: a fixture real e a asserção exercitam HE de entrada18:56 contra21:00; os demais casos publicam dados por stub. Não comprovam o golden `l084_pausa` nem equivalência completa com `cxr_lavrado`: o teste de equivalência passa uma lista manual diretamente a `InsumosDoDono`. Fontes: `app/ponto/tests/test_cxr_tela_le_o_motor.py:42`, `:76`, `:92`, `:154`, `:284`.

- **Falha parcial substitui snapshot completo antes de informar erro**: o comando grava a medida e só depois sai com código2; cada falha de colaborador remove seus itens e sua cobertura. Caso: snapshot bom tem C12 de A e B; rodada seguinte falha apenas B, grava somente A e a tela perde B. Fontes: `app/ponto/management/commands/lavrar_cxr_do_motor.py:37`, `:39`; `app/ponto/services/cxr_na_lista.py:104`, `:106`, `:110`; `ultimo_lavrado` escolhe o mais recente em `:147`.

## 08/11 03:14 · sessao f8b4a879ff68 · - · - · origem=Ronald (tmux)

**Cron: a declaração do elo está integrada, mas há uma lacuna de observabilidade e um problema no tratamento de falha parcial.** (`app/config/crons.py:1765`, `app/core/management/commands/pipeline_placar.py:38`, `app/ponto/management/commands/lavrar_cxr_do_motor.py:37`).

- **Gatilho/estágio/papel:** `lavrar_cxr_do_motor` espera `vigia_pareador_celula`, usa `--apply`, estágio `auditoria`, teto de espera 900 e dependência declarada de `processar_cartorio`; seu papel é `lavra`. (`app/config/crons.py:1765`, `app/config/crons.py:2232`).
- **O teto de 900 não limita a duração do próprio lavrador:** limita a espera pelos arquivos de fim do gatilho; depois o comando é executado sem timeout nesse script. (`bin/encadeado.sh:84`, `bin/encadeado.sh:92`, `bin/encadeado.sh:102`).
- **Ordem por arquivo:** a corrente usa o campo `gatilho`, percorre os elos sequencialmente e cada elo espera o carimbo de seu predecessor; o novo elo fica depois do pareador e, transitivamente, depois do cartório. (`app/config/crons.py:1817`, `app/config/crons.py:1835`, `bin/encadeado.sh:56`, `bin/encadeado.sh:89`).
- **Não falta pôr o command em `FORA_DE_PIPELINE`:** `todas_as_entradas()` já inclui `ENCADEADOS`, e o contrato de command com casa usa essa função; duplicá-lo em `FORA_DE_PIPELINE` seria entrada podre. (`app/config/crons.py:1783`, `app/chamados/tests/test_contract_crons.py:220`, `app/chamados/tests/test_contract_crons.py:230`).
- **Sombra e diagrama recebem o novo elo automaticamente:** `bloco_da_manha()` inclui os encadeados e o gerador do diagrama também; a alteração em `ARQUITETURA.mmd` corresponde ao mecanismo gerador. (`app/config/crons.py:1910`, `app/core/management/commands/gerar_diagrama.py:182`, `app/docs/ARQUITETURA.mmd:61`, `app/docs/ARQUITETURA.mmd:112`).
- **Contrato de duração/horário:** o teste exige que todo encadeado conte em qualquer horário e não tenha `sched`; isso é satisfeito pela forma declarada, e `corrida_no_horario()` devolve `True` para nomes fora de `CRONS`. (`app/core/tests/test_contract_crons_duracao_horario.py:43`, `app/config/crons.py:2303`, `app/config/crons.py:2316`).
- **Não existe duração cadastrada para o novo lavrador nesta cópia:** a declaração não o põe em `_DUR_DECLARADA`, e o JSON passa de `lavrar_badge_navbar` para `lavrar_deslogados`; futuras amostras entram pelo medidor de nomes do placar. Isso não exige inventar duração manual para este último elo. (`app/config/crons.py:125`, `app/config/crons_duracao.json:38`, `bin/crons_duracao.py:73`).

**Achado: o placar de pipeline ignora o novo ENC.** `esperados()` enumera somente `CRONS`, `DEPS` também, e o relatório só classifica esses esperados; registros de encadeados aparecem como `ORFAO no jsonl sem cron`. Caso concreto: o lavrador nunca executa e o placar não produz `NAO_RODOU lavrar_cxr_do_motor`; se executa, sua corrida fica órfã, sem classificação normal de dependência. É defeito preexistente do consumidor que atinge o novo elo, não falta de adicionar outro literal de cron. (`app/core/management/commands/pipeline_placar.py:9`, `app/core/management/commands/pipeline_placar.py:12`, `app/core/management/commands/pipeline_placar.py:38`, `app/core/management/commands/pipeline_placar.py:88`, `app/core/management/commands/pipeline_placar.py:108`, `app/core/management/commands/pipeline_placar.py:128`).

**Achado: erro técnico de leitura vira lavra parcial concluída pelo cron.** O serviço retira completamente o colaborador/competência que falhou, mas continua; o command grava o resultado parcial antes de sair com código 2. O envelope entende 2 como alarme de negócio, escreve carimbo de conclusão e não abre cron quebrado; outra tentativa encadeada no mesmo dia termina imediatamente por esse carimbo. Caso concreto: ontem C12 tinha um dia do colaborador; hoje a leitura desse espelho lança exceção; a nova lavra perde esse dia e é considerada concluída para fins da corrente. (`app/ponto/services/cxr_na_lista.py:101`, `app/ponto/services/cxr_na_lista.py:103`, `app/ponto/services/cxr_na_lista.py:105`, `app/ponto/management/commands/lavrar_cxr_do_motor.py:37`, `app/ponto/management/commands/lavrar_cxr_do_motor.py:39`, `bin/cron_run.sh:19`, `bin/cron_run.sh:22`, `bin/cron_run.sh:32`, `bin/encadeado.sh:81`).

**Relógio com duas leituras:** o command captura `hoje` em `timezone.localdate()` e o serviço captura outro instante com `timezone.now()`; como recebeu `hoje`, não o deriva desse instante. Caso concreto de borda: o command captura 20/11 às 23:59:59 e o serviço entra em 21/11, com janela/data_ref de ontem e instante de hoje. Ocorrência real na produção `[nao verificado]`. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:28`, `app/ponto/services/cxr_na_lista.py:52`, `app/ponto/services/cxr_na_lista.py:53`, `app/ponto/services/cxr_na_lista.py:138`).

**Congelamento de cron:** a marca CRON15 contém proibição literal de “cron novo ou mudança de horário” enquanto existir; o backlog a repete. Entretanto, o aval anexado de 06/11 autoriza expressamente esta obra com cron de lavra, e o backlog da própria CXR registra contador + cron + serviço. Não recomendo reprovar automaticamente por autorização ausente: há autorização específica posterior no dado fornecido; eventual precedência entre os dois cortes além disso é `[nao verificado]`. (`app/core/tests/marca_cron15.py:13`, `app/docs/BACKLOG.md:747`, `app/docs/BACKLOG.md:775`; aval.txt fornecido, sem numeração local `[nao verificado]`).

**Mordida dos contratos de cron é limitada:** vários testes de existência, estágio, DAG e cobertura diária percorrem só `CRONS`; não validam diretamente estes mesmos atributos dos novos `ENCADEADOS`. O contrato de duração percorre todos os encadeados, e “command tem casa” também os alcança pela função canônica; portanto não cabe afirmar que todos os contratos de cron provaram este elo. (`app/chamados/tests/test_contract_crons.py:25`, `app/chamados/tests/test_contract_crons.py:77`, `app/chamados/tests/test_contract_crons.py:269`, `app/chamados/tests/test_contract_crons.py:386`, `app/chamados/tests/test_contract_crons.py:431`, `app/core/tests/test_contract_crons_duracao_horario.py:45`, `app/chamados/tests/test_contract_crons.py:225`).

Não executei suíte nem banco. Uma importação pura com `python3 -B`, sem escrita, confirmou papel `lavra`, presença na corrente do cartório e no bloco da manhã; resultado de suíte GREEN `[nao verificado]`. [nao verificado]

## 08/11 03:14 · sessao 6a0b79b484ad · - · - · origem=Ronald (tmux)

Rastreio de request e donos, somente por leitura estática; execução em banco/testes `[nao verificado]`. [nao verificado]

1. Não encontrei chamada a `espelho_do_colab` nem a `MotorBase.calcular_mes` nas cadeias da lista depois do diff. As duas construções de `InsumosDoDono` dentro desse módulo passam `sem_motor=True`; a única chamada interna ao espelho fica protegida por essa flag. `app/escala/services/cadastro_realidade.py:525`, `app/escala/services/cadastro_realidade.py:1024`, `app/ponto/management/commands/e6_oraculo.py:166`.

   Cadeias localizadas:

   - HTML/PDF: `cadastro_x_realidade → lista`; PDF recebe o resultado já montado e só o renderiza. `app/escala/views.py:487`, `app/escala/views.py:489`, `app/escala/services/cadastro_realidade.py:1067`.
   - Vigia: `vigiar_fio_morto → ListaDoAdmin → lista → dias_na_lista`. `app/chamados/management/commands/vigiar_fio_morto.py:167`, `app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:217`.
   - Outros leitores diretos de `lista()`: comandos `fechar_avisos_regua_na_lista` e `fechar_chamados_vinculo_na_lista`. `app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`.
   - Leitores do beco fora da lista: pauta da competência exportada e reatador do beco; ambos chegam ao mesmo `beco_em_lote`. `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:105`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:54`, `app/escala/services/cadastro_realidade.py:439`, `app/escala/services/cadastro_realidade.py:573`.

2. **Selo C3 tem brecha de mordida.** O teste arma `AssertionError` e verifica apenas que C12 continua presente e PDF existe. A lista captura `Exception` nos ramos, inclusive C11; se uma chamada indevida ao motor for introduzida no beco, a exceção pode ser engolida, C11 desaparecer, C12 continuar e esse teste passar. Caso concreto: recolocar uma chamada ao espelho no ramo C11; a fixture não exige C11 e não há `assert_not_called`. `app/ponto/tests/test_cxr_tela_le_o_motor.py:130`, `app/ponto/tests/test_cxr_tela_le_o_motor.py:134`, `app/escala/services/cadastro_realidade.py:864`, `app/escala/services/cadastro_realidade.py:866`.

   O teste de conflito C9×C12 é mais forte contra essa chamada: a reconciliação ocorre fora dos `try/except` da lista, portanto uma chamada indevida nesse caminho propagaria a exceção. `app/ponto/tests/test_cxr_tela_le_o_motor.py:247`, `app/escala/services/cadastro_realidade.py:885`, `app/escala/services/cadastro_realidade.py:1026`.

3. **O teste intitulado “cxr_lavrado responde como o espelho” não chama `cxr_lavrado`.** Ele injeta manualmente `[DIA]` em um lado e um espelho artificial com o mesmo único dia no outro. Não alcança `cobertos`, partição por competência, filtro, ausência de cobertura ou conversão de `fim`. `app/ponto/tests/test_cxr_tela_le_o_motor.py:284`, `app/ponto/tests/test_cxr_tela_le_o_motor.py:286`, `app/ponto/tests/test_cxr_tela_le_o_motor.py:287`.

4. **Q4 exige distinguir igualdade do conjunto completo de igualdade de pertinência dos dias perguntados.** O conjunto completo não é igual em geral: `medir` descarta datas externas a `[ini,fim]`, mas `InsumosDoDono.cxr()` guarda todos os `dia` publicados. O próprio teste com agosto/outubro comprova a diferença de construção. `app/ponto/services/cxr_na_lista.py:88`, `app/ponto/management/commands/e6_oraculo.py:170`, `app/ponto/tests/test_cxr_tela_le_o_motor.py:226`.

   Entretanto, `um_dono_por_dia` escolhe `janela_atual(dia)`. Para um dia perguntado que pertence àquela janela, o filtro sozinho preserva a pertinência, supondo mesma publicação e cobertura correta; não sustenta um contraexemplo de dono apenas porque os conjuntos completos diferem. `app/escala/services/cadastro_realidade.py:1017`, `app/ponto/services/cxr_na_lista.py:88`, `app/ponto/services/cxr_na_lista.py:174`.

5. **`fim` tem dois significados na ligação C11→CXR.** C11 passa o final da janela cortado em ontem; `cxr_lavrado` interpreta o mês/ano desse parâmetro como a identidade da competência. Exemplo: em 22/10, a competência corrente 21/10–20/11 é cortada em 21/10, e a consulta busca `10/2026`, embora a lavra corrente tenha `11/2026`. `app/escala/services/cadastro_realidade.py:568`, `app/escala/services/cadastro_realidade.py:569`, `app/escala/services/cadastro_realidade.py:526`, `app/ponto/services/cxr_na_lista.py:173`.

   **Não atribuir mudança efetiva de dono C11 a esse defeito:** o beco só chama o juiz depois de verificar `folga_nao_definida=True`, e esse insumo já produz `CADASTRO` antes de CXR. Hoje o erro de competência fica mascarado por essa precedência. `app/escala/services/cadastro_realidade.py:518`, `app/escala/services/cadastro_realidade.py:531`, `app/ponto/management/commands/e6_oraculo.py:98`.

6. Fluxo de `dono_do_dia`:

   - C9: escrito pelo serviço do teto chamando `InsumosDoDono.dono`; formatador converte `dono` para `dono_do_dia`. `app/ponto/services/teto_na_lista.py:86`, `app/escala/services/cadastro_realidade.py:379`.
   - C10: escrito pelo censo, com os dias de fase da própria corrida injetados no mesmo portador; formatador lê `dono_do_dia` diretamente. `app/ponto/management/commands/censo_fase_12x36.py:287`, `app/ponto/management/commands/censo_fase_12x36.py:300`, `app/escala/services/cadastro_realidade.py:424`.
   - C11: escrito no request pelo beco chamando o mesmo portador; não é dono diário previamente lavrado. `app/escala/services/cadastro_realidade.py:531`.
   - C12: escrito na lavra usando o espelho da própria medição; formatador converte `dono` para `dono_do_dia`. `app/ponto/services/cxr_na_lista.py:92`, `app/ponto/services/cxr_na_lista.py:96`, `app/escala/services/cadastro_realidade.py:403`.
   - Em conflito, `um_dono_por_dia` pode sobrescrever todos esses valores: reaproveita C11 presente ou pergunta ao mesmo juiz com CXR lavrado. Assim, o `dono_do_dia` final significa “dono reconciliado pela lista”, não necessariamente “dono original daquele snapshot”. `app/escala/services/cadastro_realidade.py:1009`, `app/escala/services/cadastro_realidade.py:1026`, `app/escala/services/cadastro_realidade.py:1028`.
   - A apresentação deriva `dono`, ação e contadores; `dias_na_lista` e vigia leem o `dono_do_dia` reconciliado. `app/escala/services/cadastro_realidade.py:923`, `app/escala/services/cadastro_realidade.py:930`, `app/escala/services/cadastro_realidade.py:1063`, `app/chamados/services/vigia_fio.py:228`.

7. `_cxr`: inicializado como `None`; recebe conjunto ISO por injeção ou pela leitura do espelho e é lido por `dono`. `_sem_motor`: escrito no construtor e lido nas guardas de `cxr` e `dono`. `None` representa “não carregado/não coberto”; conjunto vazio representa “coberto, motor não publicou dia”; sem cobertura, somente o ramo residual `ESTRUTURA` vira `None`, enquanto outros insumos ainda podem decidir CADASTRO/BATIDA. `app/ponto/management/commands/e6_oraculo.py:133`, `app/ponto/management/commands/e6_oraculo.py:146`, `app/ponto/management/commands/e6_oraculo.py:148`, `app/ponto/management/commands/e6_oraculo.py:170`, `app/ponto/management/commands/e6_oraculo.py:189`.

8. **Relógios paralelos preexistentes permanecem na cadeia:** C2 lê `date.today()` e crava corte 21; a lista lê um instante próprio; PDF chamado pela view lê outro instante porque a view não o repassa. Caso concreto: pedido atravessando meia-noite pode medir C2 com data distinta e imprimir outro carimbo. Isso não é introduzido pelas linhas do diff, mas impede afirmar “um relógio por request” para a cadeia inteira. `app/escala/services/cadastro_realidade.py:252`, `app/escala/services/cadastro_realidade.py:253`, `app/escala/services/cadastro_realidade.py:794`, `app/escala/services/cadastro_realidade.py:1076`, `app/escala/views.py:489`.

9. **C12 não propaga ausência/erro/frescura da lavra para o produto.** `ultimo_lavrado` possui `sem_medida`, `medido_em`, `data_ref` e conserva `erros`, mas `_cxr_do_motor` só consome `itens`; a saída da lista publica metadados do previsto cego e esmeril, sem equivalente C12. Caso concreto: lavra inexistente e lavra válida com zero itens produzem a mesma ausência de C12; lavra parcial com erros também não ganha indicação própria. `app/ponto/services/cxr_na_lista.py:149`, `app/ponto/services/cxr_na_lista.py:151`, `app/escala/services/cadastro_realidade.py:393`, `app/escala/services/cadastro_realidade.py:970`, `app/escala/services/cadastro_realidade.py:975`.

10. Não identifiquei novo predicado concorrente de dono: a decisão continua em `dono_da_divergencia`, que recebe paridade, cadastro, fase e pertinência CXR. Os novos ramos mudam carregamento e ausência de informação, não repetem a regra CADASTRO/BATIDA. `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:110`, `app/ponto/management/commands/e6_oraculo.py:195`.

## 08/11 01:34 · sessao 58c2a164fdf2 · FIO-E0-V4-E1 · verificador · origem=Code

**PROCEDENTE: a garantia “exportada fica FORA e não é escrita” está incompleta.** Há caminho de carimbagem indireta após o commit, veto anterior à trava no migrador e exclusão de perdedoras abertas quando compartilham plano com uma exportada. (`app/chamados/signals.py:225`; `app/chamados/services/disputa_emissao.py:1838`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:294`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:285`)

A análise abaixo é estática. Execução dos testes, morte efetiva dos mutantes, publicação do DIFF/RELATO, estado das migrações e ocorrência desses caminhos em produção: **[nao verificado]**. [nao verificado]

**1(a). Quem define e quem chama os vetos — NAO PROCEDE haver outro juiz novo no helper acrescentado.**

O registro vivo aponta K6 para `competencia_entregue`; `JUIZES.md` declara que é consulta e remete ao registro vivo. O helper novo compõe autoridades existentes. (`app/core/juizes.py:312`; `app/docs/JUIZES.md:10`; `app/chamados/services/fio.py:1077`)

| Função definida | Chamadores diretos de produção |
|---|---|
| `fio.veto_de_competencia`, definição em `app/chamados/services/fio.py:1070` | `aplicar`, por plano e por silêncio: `app/chamados/management/commands/backfill_decisoes_pergunta.py:281`, `:294`; classificação e migração: `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:257`, `:294`. |
| `vigia_fio.competencia_do_dia_entregue`, definição em `app/chamados/services/vigia_fio.py:822` | `_vivas_entre`: `app/chamados/models.py:364`; `_costura_nao_mata_o_fio`: `app/chamados/services/fio.py:776`; `veto_de_competencia`: `app/chamados/services/fio.py:1080`; `_dia_fora_do_alcance_c`: `app/chamados/reconciliador.py:744`; `limbo_vai_a_pauta`: `app/chamados/services/fabrica_da_celula.py:198`; `carimbos_sem_lastro_por_dono`: `app/chamados/services/vigia_fio.py:816`; `_cerca`: `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:115`. |
| `folha.export.competencia_entregue`, definição em `app/folha/export.py:556` | Chamadores discriminados na tabela seguinte. |

| Chamador direto de `competencia_entregue` | Local |
|---|---|
| `competencia_do_dia_entregue`, consulta sem memo e alimentação do memo | `app/chamados/services/vigia_fio.py:841`, `:843` |
| Backfill: relatório das projeções sem linha | `app/chamados/management/commands/backfill_decisoes_pergunta.py:399` |
| Backfill: `_exportada`, para agrupar/rotular o acervo | `app/chamados/management/commands/backfill_decisoes_pergunta.py:426` |
| `competencia_fechada_para_celula` | `app/folha/export.py:610` |
| `txt_vigente_x_gravado` | `app/folha/export.py:647` |
| `preview_exportacao` | `app/folha/views.py:354` |
| Ranking HE, `_carimbo` | `app/folha/services/ranking_he.py:89` |
| `marcos_da_competencia` | `app/ponto/services/fechamento.py:1530` |
| `competencias_a_lavrar` | `app/ponto/services/he_pendente_lavrado.py:223` |
| Espelho, `janela` | `app/ponto/services/esmeril_espelho.py:58` |
| `lavrar_status_competencia.handle` | `app/ponto/management/commands/lavrar_status_competencia.py:43` |
| Pauta folga não definida, `_rodar` | `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:93` |
| Pauta pago × devido, `_medir` | `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:124` |

As três funções consultadas não gravam o estado de entrega: K6 lê `ExportacaoDominio`, aplica vigência e rejeita o vigente invalidado. Os escritores desse estado são `registrar` e `invalidar`. (`app/folha/export.py:580`; `app/folha/export.py:583`; `app/folha/models.py:158`; `app/folha/models.py:182`)

**Há outros caminhos que usam entrega para decidir atos sobre perguntas/chamados:**

| Caminho | Consequência |
|---|---|
| `_costura_nao_mata_o_fio` | A competência aberta participa da recusa de costurar pergunta viva em destino terminal; exportada passa por essa guarda. (`app/chamados/services/fio.py:776`; `app/chamados/services/fio.py:784`) |
| `_vivas_entre` | Exportadas são retiradas das perguntas que seguram o encerramento do chamado. (`app/chamados/models.py:364`; `app/chamados/models.py:383`) |
| `_dia_fora_do_alcance_c` | Entrega deixa a pergunta fora do alcance novo da reemissão/costura do reconciliador. (`app/chamados/reconciliador.py:744`; `app/chamados/reconciliador.py:755`) |
| `limbo_vai_a_pauta` | No limbo exportado, emite pauta e retorna antes de perguntar ao colaborador. (`app/chamados/services/fabrica_da_celula.py:181`; `app/chamados/services/fabrica_da_celula.py:198`) |

Esses caminhos leem o compositor existente; não são novas cópias do predicado K6. O backfill também mantém uma composição para **relatório**, mas a classificação autoritativa do apply vem da porta. (`app/chamados/services/vigia_fio.py:836`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:422`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:463`)

**1(b). Contadores e retorno de `aplicar`.**

| Dado | Quem escreve | Quem lê |
|---|---|---|
| `CONTADORES_DO_APPLY` | Declaração inclui os dois novos contadores. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:129`) | Dedup separa contadores de recusas; resumo do backfill percorre a tupla. (`app/chamados/management/commands/dedup_perguntas_fato.py:107`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:514`) |
| `exportada_fora` | Plano vetado acrescenta **todas as retratadas**; silêncio acrescenta seu PK; `_fora` ordena/deduplica. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:285`; `:298`; `:345`) | Renderizador do backfill, resumo, dedup e testes. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:378`; `:514`; `app/chamados/management/commands/dedup_perguntas_fato.py:121`; `app/chamados/tests/test_e1_v13d_exportada_fora.py:65`; `:80`; `:93`; `:103`; `:111`; `app/chamados/tests/test_e1_v6_horizonte.py:347`; `:355`) |
| `exportada_fora_diff` | Dicionários de retratação/silêncio; `_fora` conserva a primeira entrada por PK. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:286`; `:299`; `:340`) | Renderizadores do backfill/dedup e testes do conteúdo. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:372`; `app/chamados/management/commands/dedup_perguntas_fato.py:116`; `app/chamados/tests/test_e1_v13d_exportada_fora.py:67`; `:68`; `:81`; `:82`) |
| `sem_competencia_recusada` | `_recusas`; ramo `None` dos planos e dos silêncios. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:140`; `:283`; `:296`) | Impede escrita e provoca rollback geral; CLI/dedup traduzem para saída 3; teste força esse retorno. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:304`; `:327`; `:500`; `app/chamados/management/commands/dedup_perguntas_fato.py:110`; `app/chamados/tests/test_e1_v13d_exportada_fora.py:103`) |
| Antigo `exportada_recusada` | Não integra o contrato atual de retorno, que usa `_fora` e `_recusas`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:331`; `:333`) | Resta a referência histórica e a asserção de ausência da chave no teste novo. (`app/chamados/tests/test_e1_v13d_exportada_fora.py:6`; `:66`) |

O retorno preserva `exportada_fora` e seu DIFF mesmo quando outra recusa desfaz as escritas; os contadores de escrita voltam a zero. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:329`)

**PROCEDENTE — preservar no retorno não garante publicar:** dedup sai por recusa antes de renderizar o DIFF; o backfill também trata `horizonte_diverge` antes de chamar seu renderizador. (`app/chamados/management/commands/dedup_perguntas_fato.py:107`; `:116`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:494`; `:499`)

| Consumidor solicitado | Resultado |
|---|---|
| Backfill `handle` | DRY chama a porta em rollback e publica os FORA; apply chama a porta e traduz recusas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:484`; `:487`; `:493`; `:500`) |
| Dedup | Apply consome o retorno; seu DRY apenas conta o plano e não passa por `aplicar` nem publica essa classificação de exportadas. (`app/chamados/management/commands/dedup_perguntas_fato.py:91`; `:99`; `:105`; `:116`) |
| Migração 0052 | **NAO PROCEDE** como leitora desses contadores: apenas adiciona `batida_origem` anulável. (`app/chamados/migrations/0052_pergunta_batida_origem.py:17`) |
| Migração 0053 | **NAO PROCEDE** como leitora do retorno: conta colisões diretamente e recusa sua própria execução. (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:36`) |
| `_migrar_sob_trava` | **NAO PROCEDE** como leitor de `aplicar`: consome `plano_de_retratacao`/`NAO_RETRATA`, preenche seu próprio `out` e chama a porta de retratação. (`app/chamados/services/fio.py:1568`; `:1574`; `:1590`) |
| ESTADO | O caminho demonstrado lê o carimbo de `vigiar_fio_morto`, com outro conjunto de contadores; não consome o retorno do apply. (`bin/gerar_estado.py:253`; `bin/fio_estado.py:65`; `app/chamados/management/commands/vigiar_fio_morto.py:88`; `:96`) Outra integração: **[nao verificado]**. |

**1(c). Campos e artefatos do migrador.**

| Dado | Quem escreve | Quem lê |
|---|---|---|
| `ln['fora']` | Inicialização; motivos de classificação; desconhecida/exportada. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:132`; `:158`; `:259`; `:261`) | Seleção migra/FORA, saída, partição da medida e guarda de classificação. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:196`; `:202`; `:224`; `:255`) |
| `ln['comp']` | String de competências formatadas, ordenadas por ano/mês. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:250`; `:254`) | Saídas MIGRA, FORA e EXPORTADA_FORA. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:199`; `:202`; `:264`) |
| `exportada_fora` | Texto calculado por `len(fora)` na classificação; não é o retorno do backfill. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:247`; `:262`; `:267`) | Testes de saída. (`app/chamados/tests/test_e1_v13d_exportada_fora.py:173`; `:187`; `:197`; `app/chamados/tests/test_migrar_data_evento_dia_do_turno.py:204`) |
| `descarimbaria` / `desc` | `_descarimbaria` conta perguntas elegíveis que o juiz considera vivas; atribuição da medida comum. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:225`; `:324`; `:327`) | Impressão e bloqueio do apply. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:231`; `:234`) |
| `descarimbaria_exportada_fora` / `desc_exp` | Mesma função, alimentada pelas linhas classificadas como exportadas. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:224`; `:226`) | Impressão e testes; não participa do bloqueio. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:232`; `:234`; `app/chamados/tests/test_e1_v13d_exportada_fora.py:185`; `app/chamados/tests/test_migrar_data_evento_dia_do_turno.py:207`) |
| Arquivo de reversão | Apply, antes do atomic; JSON exclusivo, permissão 600. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:216`; `:219`; `:276`; `:278`; `:281`) | `--reverter`, que lê o JSON, chama a porta, confere célula e desfaz tudo se houver falha. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:183`; `:340`; `:350`; `:355`; `:359`) |

**PROCEDENTE — classificação e resultado final podem divergir:** uma entrega detectada apenas no segundo veto entra em `recusas`, mas não atualiza `ln['fora']`. Portanto não entra no `exportada_fora` já publicado nem na partição `desc_exp`. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:267`; `:294`; `:296`; `:224`)

A reversão contém datas/células das candidatas iniciais, inclusive das depois recusadas; não contém fotografias dos efeitos sobre irmãs, disputas ou chamados. O arquivo também sobrevive ao rollback do banco, porque nasce antes do atomic. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:216`; `:219`; `:276`; `:294`)

**2. Exportada escrita ou aberta bloqueada por exportada — PROCEDENTE, com estes recortes.**

- **Escrita direta de retratação/silêncio:** quando o veto classifica o item como exportado, ele é retirado das listas que serão escritas. A exportação, sozinha, deixou de ser recusa global do apply. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:284`; `:291`; `:297`; `:303`; `:306`; `:312`)
- **Aberta bloqueada dentro de plano misto:** basta uma retratada exportada para `v=True`; todas as retratadas daquele plano ficam FORA, inclusive as abertas. É tudo-ou-nada **do plano**, embora não seja rollback do apply inteiro. (`app/chamados/services/fio.py:1080`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:281`; `:285`)
- **Exportada carimbada indiretamente:** retratar uma aberta salva a pergunta; o signal agenda fechamento de beco; depois do commit, o beco chama o resolvedor, que alcança outras perguntas e outras disputas abertas do colaborador sem veto de entrega nesses ramos. (`app/chamados/services/fio.py:1107`; `app/chamados/models.py:2993`; `app/chamados/signals.py:225`; `app/chamados/services/becos.py:54`; `app/chamados/services/disputa_emissao.py:1764`; `:1805`; `:1838`)
- **Continuação da cascata:** fechar a disputa também aciona a materialização de pendentes, que pode carimbar outras perguntas aprovadas. (`app/chamados/services/disputa_emissao.py:1865`; `app/chamados/signals.py:163`; `app/chamados/services/materializacao.py:1418`; `:1431`)
- **`data_evento` pela reversão:** `--reverter` desvia antes da classificação e chama `rechavear_data_evento` diretamente; essa porta trava e escreve sem consultar o veto de competência. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:183`; `:350`; `app/chamados/services/fio.py:947`; `:965`)

Um contraexemplo construtível para a cascata é: aberta escrita numa disputa com pai/destinos terminais, sem pergunta visível e fechável; irmã exportada morta para C6, ainda sem materialização e elegível ao resolvedor. A classificação de beco e a carimbagem permitem esse percurso. (`app/chamados/services/becos.py:25`; `:33`; `:45`; `:54`; `app/chamados/services/disputa_emissao.py:1724`; `:1744`; `:1764`) Existência desse arranjo em produção: **[nao verificado]**.

O laço de migração não descarimba diretamente: sua gravação altera data, célula e lavra. O descarimbo real continua na porta de julgamento, que pode ser acionada por cascatas e não contém separação por exportada nesse trecho. (`app/chamados/services/fio.py:959`; `:965`; `app/chamados/signals.py:203`; `app/chamados/services/fabrica_da_celula.py:175`; `app/chamados/services/relavra_via.py:238`; `:246`) Um descarimbo exportado efetivamente provocado por esta execução: **[nao verificado]**.

**3. Canônica e retratada em competências diferentes.**

| Situação | Resultado direto da porta |
|---|---|
| Canônica aberta; retratada exportada | Retratação fica FORA. A canônica não é salva por esse ato; a retratada permanece sem a nova marca de duplicata. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:281`; `:285`; `app/chamados/services/fio.py:1107`) |
| Canônica exportada; retratada aberta | A canônica não participa do veto. A aberta pode receber o carimbo de duplicata; o PK da canônica entra no motivo. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:281`; `:314`; `app/chamados/services/fio.py:1107`; `:1110`) |
| Canônica também candidata a silêncio | É um item independente. Se aberta, pode receber SILENCIO mesmo com a retratação exportada FORA; se exportada, seu silêncio fica FORA. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:293`; `:297`; `:307`) |

**NAO PROCEDE exigir a canônica no veto para impedir uma escrita direta dela pela retratação:** esse caminho só salva a perdedora. Também não registra uma `DecisaoPergunta` de retratação; o registro de SILENCIO é outro caminho. (`app/chamados/services/fio.py:1107`; `app/chamados/models.py:2985`; `:2993`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:307`)

**PROCEDENTE exigir proteção dos alvos indiretos:** incluir somente a canônica no veto ainda deixaria irmãs e outras disputas expostas à cascata. (`app/chamados/services/disputa_emissao.py:1805`; `:1813`; `:1838`)

**4. Veto novamente dentro do atomic e sob a trava.**

| Caminho | Veredito |
|---|---|
| Backfill | **PROCEDENTE a insuficiência da releitura:** o veto roda sob as travas de colaborador/disputas/perguntas, mas `_memo` é único para todo o apply. A segunda pergunta da mesma competência pode receber o resultado antigo sem nova consulta a K6. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:203`; `:215`; `:220`; `:281`; `:294`; `app/chamados/services/vigia_fio.py:842`) |
| Migrador | **NAO PROCEDE dizer que não repete:** repete com memo novo dentro do atomic da linha. **PROCEDENTE dizer que não é sob a trava:** o veto vem antes de chamar a porta que adquire a trava. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:257`; `:291`; `:294`; `:298`; `app/chamados/services/fio.py:947`) |
| Estado de entrega | **PROCEDENTE a janela:** K6 faz leitura comum; `registrar` insere a entrega sem compartilhar a trava do colaborador usada nesses caminhos. Não há nova consulta entre o veto e o save da migração. (`app/folha/export.py:581`; `app/folha/models.py:182`; `app/chamados/services/fio.py:965`) |

Além disso, o migrador pergunta sobre `ln['velho']` e a empresa capturados na classificação; a porta depois relê a pergunta e obtém seu dia efetivamente gravado, sem refazer o veto para essa releitura. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:294`; `app/chamados/services/fio.py:949`; `:950`)

**5. DRY do backfill e alcance do rollback.**

**NAO PROCEDE haver escrita durável no caminho ORM normal demonstrado:** o DRY envolve `aplicar` em atomic explicitamente desfeito; isso abrange decisão, pergunta, outbox síncrono e auditoria no banco padrão. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:484`; `:486`; `app/chamados/models.py:3748`; `:3225`; `app/chamados/signals.py:237`; `app/core/observ.py:133`)

| Efeito | Tratamento |
|---|---|
| `DecisaoPergunta` | `bulk_create` dentro da transação. (`app/chamados/models.py:3746`) |
| Carimbo da perdedora | Save transacional da pergunta. (`app/chamados/models.py:2993`; `:3225`) |
| `EventoCelula` | Signal síncrono chama a porta do evento; participa da transação. (`app/chamados/signals.py:244`; `app/chamados/models.py:3051`; `app/ponto/portas/celula.py:890`) |
| `LogAuditoria` | INSERT em atomic aninhado, sem commit independente no código mostrado. (`app/core/observ.py:133`) |
| Usuário sistema, se criado | `get_or_create` comum, sem alias explícito de outro banco. (`app/core/utils.py:18`) |
| Beco em `on_commit` | É agendado pelo save; não chega ao commit no DRY desfeito. (`app/chamados/signals.py:225`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:486`) |

**PROCEDENTE contra a frase absoluta “rollback desfaz tudo”:** falha da auditoria pode incrementar `_ENGOLIDOS` em memória e emitir ERROR imediatamente. O perfil saas encaminha esse ERROR a `errors.log`; transação de banco não desfaz esses efeitos. (`app/core/observ.py:75`; `:79`; `:105`; `app/config/settings/saas.py:139`; `:183`)

O perfil saas declara somente `default`; configuração efetivamente implantada, outro banco ou extensão externa não presente na cópia: **[nao verificado]**. (`app/config/settings/saas.py:83`; `:93`)

**6. O que mede `descarimbaria_exportada_fora`.**

**PROCEDENTE: não é DIFF causal da migração da exportada.** Ela não migrou; a função conta, no estado atual, carimbos elegíveis que C6 considera vivos. Mede o que **um ato posterior de descarimbo** derrubaria, não quanto esta migração alterou nessa linha. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:196`; `:226`; `:324`; `:327`; `app/c

[truncado em 20000 de 29916 caracteres]

## 08/11 01:28 · sessao ddc6f16190eb · - · - · origem=Ronald (tmux)

Achados para itens 2–5, por leitura estática:

1. **PROCEDENTE — veto do migrar ocorre antes da trava.** `_migrar` chama o veto dentro de `atomic`, mas só depois chama `rechavear_data_evento`; a trava Colaborador→Disputa→Pergunta acontece dentro dessa porta. Portanto existe intervalo entre consulta e aquisição da trava, e a porta relê `p`, mas não repete o veto para essa leitura. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:291`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:294`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:298`, `app/chamados/services/fio.py:947`, `app/chamados/services/fio.py:949`.

2. **PROCEDENTE — backfill não consulta entrega novamente para cada item.** Os alvos são relidos sob a trava, mas `_memo` é criado antes do atomic e compartilhado por todos os colaboradores/planos; o compositor devolve resposta guardada por empresa/mês/ano. O juiz de entrega faz consulta comum, sem trava de `ExportacaoDominio`. `app/chamados/management/commands/backfill_decisoes_pergunta.py:203`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:281`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:294`, `app/chamados/services/vigia_fio.py:839`, `app/chamados/services/vigia_fio.py:842`, `app/folha/export.py:581`.

3. **PROCEDENTE — há caminho indireto para carimbar outra pergunta exportada.** Retratar salva a perdedora; o signal agenda fechamento de beco após commit; esse fechamento chama o resolvedor, que alcança outras perguntas/disputas do colaborador e carimba elegíveis sem consultar entrega. Fechar disputa também dispara materialização de pendentes aprovadas, novamente sem esse veto. Trata-se de caminho demonstrável; existência de alvo real elegível em produção é **[nao verificado]**. `app/chamados/services/fio.py:1107`, `app/chamados/models.py:2993`, `app/chamados/signals.py:214`, `app/chamados/signals.py:225`, `app/chamados/services/becos.py:41`, `app/chamados/services/becos.py:54`, `app/chamados/services/disputa_emissao.py:1764`, `app/chamados/services/disputa_emissao.py:1805`, `app/chamados/services/disputa_emissao.py:1838`, `app/chamados/services/disputa_emissao.py:1865`, `app/chamados/signals.py:163`, `app/chamados/services/materializacao.py:1418`, `app/chamados/services/materializacao.py:1431`.

4. **Granularidade direta da canônica:**

| Situação | Resultado direto |
|---|---|
| Canônica aberta, retratada exportada | Plano fica FORA; retratada não recebe carimbo; canônica não é escrita pela retratação; não nasce Decisão por retratar. `app/chamados/management/commands/backfill_decisoes_pergunta.py:281`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:285`, `app/chamados/services/fio.py:1107` |
| Canônica exportada, retratada aberta | Canônica não participa do veto; retratada pode ser carimbada duplicata; canônica só fornece pk/texto; não nasce Decisão por essa retratação. `app/chamados/management/commands/backfill_decisoes_pergunta.py:281`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:314`, `app/chamados/services/fio.py:1107`, `app/chamados/services/fio.py:1110`, `app/chamados/models.py:2985`, `app/chamados/models.py:2993` |
| Canônica também candidata a SILENCIO | É item independente: veto por pk; aberta pode receber SILENCIO mesmo com retratada exportada fora; exportada fica fora. `app/chamados/management/commands/backfill_decisoes_pergunta.py:293`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:307` |
| Várias retratadas, uma exportada | Todas as retratadas do plano ficam FORA, inclusive abertas; isso é exclusão por plano, não rollback do apply inteiro. `app/chamados/management/commands/backfill_decisoes_pergunta.py:281`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:285` |

Não é necessário incluir a canônica no veto para proteger uma escrita **direta** dela, pois a retratação não a salva; é necessário proteger todos os alvos indiretos das cascatas. Incluir só a canônica não cobre outras irmãs. `app/chamados/services/fio.py:1107`, `app/chamados/models.py:2993`, `app/chamados/services/disputa_emissao.py:1805`, `app/chamados/services/disputa_emissao.py:1838`.

5. **DRY: rollback cobre as escritas ORM observadas no caminho normal, mas “desfaz tudo que toca” é falso em sentido absoluto.** O atomic externo é explicitamente desfeito; SILENCIO usa `bulk_create`, retratação salva Pergunta, outbox cria `EventoCelula` sincronamente e auditoria cria `LogAuditoria` em savepoint aninhado. O callback de beco fica agendado, portanto não executa com esse rollback. `app/chamados/management/commands/backfill_decisoes_pergunta.py:484`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:486`, `app/chamados/models.py:3748`, `app/chamados/models.py:3225`, `app/chamados/signals.py:237`, `app/ponto/portas/celula.py:890`, `app/core/observ.py:133`, `app/chamados/signals.py:225`.

   Se auditoria falha, `engolir` altera contador Python e emite ERROR imediatamente; esse efeito não pertence à transação. O perfil saas encaminha o ERROR ao arquivo `errors.log`. A ocorrência concreta durante DRY é **[nao verificado]**. `app/core/observ.py:75`, `app/core/observ.py:79`, `app/core/observ.py:90`, `app/config/settings/saas.py:136`, `app/config/settings/saas.py:139`, `app/config/settings/saas.py:183`.

   `get_user_sistema` faz `get_or_create` comum, sem cache ou alias de outro banco; o perfil saas declara somente `default`. Configuração efetivamente implantada é **[nao verificado]**. `app/core/utils.py:18`, `app/config/settings/saas.py:83`, `app/config/settings/saas.py:93`.

6. **PROCEDENTE — ordem da transação com vários colaboradores.** `aplicar` mantém atomic único, trava C1→Disputa/Pergunta e só depois adquire C2. Ordena colaboradores por pk, mas não trava todos os colaboradores antes das tabelas seguintes, como exige a própria documentação da trava. Deadlock ocorrido é **[nao verificado]**. `app/chamados/management/commands/backfill_decisoes_pergunta.py:209`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:210`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:220`, `app/chamados/models.py:2564`.

7. **Testes deixam as cascatas após commit sem cobertura.** Os casos do backfill herdam `_Base(TestCase)` e verificam estado direto; não executam explicitamente os callbacks. A prova “a exportada não foi escrita” nesses casos não cobre a carimbagem indireta posterior ao commit. `app/chamados/tests/test_e1_v6_horizonte.py:63`, `app/chamados/tests/test_e1_v13d_exportada_fora.py:31`, `app/chamados/tests/test_e1_v13d_exportada_fora.py:58`, `app/chamados/tests/test_e1_v13d_exportada_fora.py:70`, `app/chamados/signals.py:225`.

Não executei testes; resultado executado e mutantes reais são **[nao verificado]**. [nao verificado]

## 08/11 01:28 · sessao 594aeb1122f1 · - · - · origem=Ronald (tmux)

**1(b) — tabela de escritores/leitores dos contadores**

| Campo/contrato | Quem escreve | Quem lê |
|---|---|---|
| `CONTADORES_DO_APPLY` | Tupla declarada em `app/chamados/management/commands/backfill_decisoes_pergunta.py:129` | Dedup distingue contador de recusa por essa tupla (`app/chamados/management/commands/dedup_perguntas_fato.py:104` e `:107`); resumo do backfill a percorre, excluindo `_diff` da contagem textual (`app/chamados/management/commands/backfill_decisoes_pergunta.py:514`). |
| `exportada_fora` | `aplicar` acumula todos os membros de um plano vetado em `app/chamados/management/commands/backfill_decisoes_pergunta.py:285`; acumula pk de silêncio em `:298`; `_fora` ordena e deduplica em `:338`–`:345`; integra retorno de sucesso e recusa em `:329`–`:335`. | Renderizador do backfill, `:370`–`:378`, chamado no DRY `:487` e apply `:499`; resumo `:514`; dedup `app/chamados/management/commands/dedup_perguntas_fato.py:121`; testes novos `app/chamados/tests/test_e1_v13d_exportada_fora.py:65`, `:80`, `:93`, `:103`, `:111`; testes anteriores adaptados `app/chamados/tests/test_e1_v6_horizonte.py:347` e `:355`. |
| `exportada_fora_diff` | Dicionários de retratação em `app/chamados/management/commands/backfill_decisoes_pergunta.py:286`, de silêncio em `:299`; `_fora` mantém a primeira entrada por pk em `:340`–`:345`; ambos os retornos carregam em `:332` e `:335`. | Renderizador `app/chamados/management/commands/backfill_decisoes_pergunta.py:372`; dedup `app/chamados/management/commands/dedup_perguntas_fato.py:116`; testes novos `app/chamados/tests/test_e1_v13d_exportada_fora.py:67`, `:68`, `:81`, `:82`; tupla impede classificá-lo como recusa (`backfill_decisoes_pergunta.py:131`, `dedup_perguntas_fato.py:107`). |
| `sem_competencia_recusada` | Lista inicial `app/chamados/management/commands/backfill_decisoes_pergunta.py:140`; retratação `:283`; silêncio `:296`; retorno `:332` ou lista vazia por `_recusas()` em `:335`. | `any(rec.values())` impede escrita/força rollback em `:304` e `:327`; CLI sai com 3 em `:500`–`:503`; dedup sai com 3 em `app/chamados/management/commands/dedup_perguntas_fato.py:110`; teste `app/chamados/tests/test_e1_v13d_exportada_fora.py:103`. |
| Antigo `exportada_recusada` | Na cópia atual, resta referência histórica na docstring do teste, não escritor do retorno (`app/chamados/tests/test_e1_v13d_exportada_fora.py:6`); ausência absoluta de referência dinâmica fora do censo textual [nao verificado]. | Teste exige ausência da chave em `app/chamados/tests/test_e1_v13d_exportada_fora.py:66`; leitores de produção atuais usam a substituta em `backfill_decisoes_pergunta.py:500` e `dedup_perguntas_fato.py:110`. |

**PROCEDENTE — o retorno preserva o FORA numa recusa, mas nem todo leitor o publica:** dedup levanta `SystemExit` antes do laço do DIFF (`app/chamados/management/commands/dedup_perguntas_fato.py:107`–`:116`); backfill faz o mesmo quando `horizonte_diverge` (`app/chamados/management/commands/backfill_decisoes_pergunta.py:494`–`:499`).

**Migrações e `_migrar_sob_trava`:**

- **NÃO PROCEDE como leitores desses contadores:** 0052 apenas adiciona `batida_origem` anulável (`app/chamados/migrations/0052_pergunta_batida_origem.py:17`–`:21`); 0053 mede colisões diretamente no acervo e recusa sua própria execução (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:36`–`:46`). `_migrar_sob_trava` lê as categorias de `plano_de_retratacao`/`NAO_RETRATA` e escreve seu próprio `out`, sem consumir o retorno de `aplicar` (`app/chamados/services/fio.py:1568`–`:1590`).
- **FILA — pré-requisito de 0053 continua aberto quando houver colisão exportada:** a migração não exclui exportada do índice nem de `_guarda` (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:31`–`:46`, `:55`–`:66`); o teste novo preserva duas linhas desse grupo (`app/chamados/tests/test_e1_v13d_exportada_fora.py:73`–`:80`). Existência dessa colisão na produção e qualquer pouso efetivo [nao verificado].
- **ESTADO não recebe esses novos contadores pelo caminho demonstrado:** seu fio lê `bin/fio_estado.py` (`bin/gerar_estado.py:253`–`:261`), cujo dado é o carimbo de `vigiar_fio_morto` (`bin/fio_estado.py:62`–`:65`); o contrato de esperados desse escritor não inclui os contadores do apply (`app/chamados/management/commands/vigiar_fio_morto.py:88`–`:99`). Outra integração fora desse caminho [nao verificado].

**7 — selos/RED: leitura estática de cada teste novo; execução e mutantes mortos [nao verificado].** [nao verificado]

| Teste, em `app/chamados/tests/test_e1_v13d_exportada_fora.py` | Julgamento da mordida demonstrada |
|---|---|
| `:58` aberta aplica/exportada fora | **NÃO PROCEDE** selo vazio: exige decisão na aberta, nenhuma decisão na exportada, pk e ato no DIFF (`:62`–`:68`). Não compara todos os campos das perguntas (`:62`–`:68`). |
| `:70` colisão exportada + silêncio aberto | **NÃO PROCEDE** selo vazio: exige silêncio aberto e ambas as vias exportadas preservadas, além da canônica no DIFF (`:77`–`:82`). Não cobre plano com perdedoras em competências distintas (`:73`–`:74`). |
| `:84` mérito | **NÃO PROCEDE** selo vazio: exige recusa com zero silêncio e ausência de decisão na aberta (`:91`–`:94`). O teste declara todas as competências não exportadas pelo mock (`:91`). | [nao verificado]
| `:96` sem dia | **PROCEDENTE** alcance menor que o nome: usa pergunta com data `ABERTA` e força `veto=None`; testa a tradução de `None`, não o trajeto real de pergunta sem dia (`:99`–`:104`). |
| `:106` idempotência | **NÃO PROCEDE** para duplicação de decisão: verifica segunda escrita zero e uma decisão total (`:109`–`:112`); igualdade do estado completo [nao verificado]. |
| `:117` CLI apply | **NÃO PROCEDE** para comportamento principal: exige aberta escrita, exportada sem decisão, contador/DIFF e ausência de recusa (`:121`–`:127`). |
| `:129` CLI DRY | **PROCEDENTE** para pretensão “não escreve tudo”: observa somente `DecisaoPergunta`, não auditoria, outbox, usuário de sistema, arquivos ou logs (`:133`–`:138`). |
| `:140` dedup | **NÃO PROCEDE** para tudo-ou-nada antigo: exige retratação aberta, duas vias exportadas preservadas e contador (`:145`–`:148`). |
| `:164` migrar mista | **NÃO PROCEDE** para tudo-ou-nada antigo: compara datas de duas abertas e da exportada e exige DIFF/contador (`:167`–`:174`). Imutabilidade completa da exportada não é testada (`:170`). |
| `:176` materializada exportada | **NÃO PROCEDE** para trava global pelo descarimbo: exige `descarimbaria=0`, medida separada 1, aberta migrada e datas exportadas intactas (`:183`–`:191`). Não compara campos de carimbo (`:190`–`:191`). |
| `:193` migrar DRY | **PROCEDENTE** para a abrangência anunciada: confere somente a data de `p_mat` e saída (`:196`–`:198`). O teste anterior é mais amplo: compara fotografias de todas as perguntas, totais de batidas/auditoria e ausência de arquivo (`app/chamados/tests/test_migrar_data_evento_dia_do_turno.py:136`–`:147`). |
| `:200` veto único | **PROCEDENTE** selo insuficiente para unicidade e trava: exige apenas cinco chamadas (`:203`–`:207`); deslocar chamadas para fora da trava ou acrescentar decisão paralela pode preservar esse número [nao verificado por mutante]. |
| `:209` virou exportada | **NÃO PROCEDE** selo vazio para classificação→linha: altera a resposta após terminar `_competencias`, exige recusa e datas preservadas (`:216`–`:224`). Não exercita mudança depois do veto e antes da trava/escrita, pois a bandeira já está alterada antes de `_migrar` (`:218`–`:221`). |

**Relógio — PROCEDENTE no trajeto exercitado pelos testes novos de backfill:** `_Caso` herda `_Base` sem congelamento declarado nessas classes (`app/chamados/tests/test_e1_v13d_exportada_fora.py:32`; `app/chamados/tests/test_e1_v6_horizonte.py:63`–`:80`); `_sem_ato` chama `fechar_por_admin` sem instante (`app/chamados/tests/test_e1_v13d_exportada_fora.py:38`–`:40`), e essa porta lê `_tz.now()` (`app/chamados/models.py:2867`). Os testes chamam `aplicar` sem `agora` (`app/chamados/tests/test_e1_v13d_exportada_fora.py:61`, `:76`, `:145`); a retratação recebe `None` (`app/chamados/management/commands/backfill_decisoes_pergunta.py:314`) e lê `timezone.now()` no service (`app/chamados/services/fio.py:1100`–`:1101`). A classe de migração está congelada explicitamente (`app/chamados/tests/test_e1_v13d_exportada_fora.py:151`). Esses relógios de produção são anteriores ao diff fornecido; serem introduzidos por este commit [nao verificado].

**Segundo escritor — NÃO PROCEDE no ato específico “retratar duplicata”:** `retratar_para_canonica` reaplica o plano e é quem chama `retratar_duplicata` (`app/chamados/services/fio.py:1083`–`:1090`); backfill e `_migrar_sob_trava` passam por ela (`app/chamados/management/commands/backfill_decisoes_pergunta.py:314`; `app/chamados/services/fio.py:1590`). A catraca declara e testa essa exclusividade, com limites explícitos de formas indiretas/SQL (`app/chamados/tests/test_e1_v7_catracas.py:16`–`:18`, `:39`–`:44`, `:309`–`:347`).

**Juiz registrado — NÃO PROCEDE para nascimento de outro juiz de “entregue”:** K6 aponta a `folha/export.py::competencia_entregue` (`app/core/juizes.py:303`–`:312`); o helper novo chama `competencia_do_dia_entregue`, sem consultar exportação diretamente (`app/chamados/services/fio.py:1077`–`:1080`). Censo de todos os caminhos alternativos fica com o agente principal [nao verificado por este subagente].

## 08/11 01:28 · sessao 93b692bdbfb0 · - · - · origem=Ronald (tmux)

Achados principais — somente leitura; ocorrência real em produção [nao verificado].

| Tema | Veredito | Evidência |
|---|---|---|
| Veto novamente dentro do atomic | **NAO PROCEDE** dizer que não repete: classificação chama em `migrar_data_evento_dia_do_turno.py:257`; `_migrar` repete com memo novo em `:291-298`. |
| Veto sob a trava | **PROCEDENTE**: o veto ocorre em `:294`; a trava só vem dentro de `rechavear_data_evento`, em `fio.py:947-949`. Atomic não equivale a trava. |
| Janela exportar → escrever | **PROCEDENTE**: a consulta entregue é SELECT sem trava em `folha/export.py:581-586`; entre o veto `migrar...py:294` e o save `fio.py:965` não há nova consulta. Também se verifica `ln['velho']`/empresa da classificação, enquanto a porta relê a pergunta depois, em `fio.py:949-950`. |
| Reversão escreve exportada | **PROCEDENTE**: `--reverter` desvia antes da classificação em `migrar...py:183-184`, lê JSON em `:340-341`, chama diretamente a porta em `:350-351`; `rechavear_data_evento` não consulta veto no corpo `fio.py:941-970`. |
| Escrita indireta após commit | **PROCEDENTE**, como caminho construtível: save `fio.py:965` agenda `becos` por `on_commit` em `signals.py:214-225`; `becos.py:45-54` varre destinos das irmãs e chama o resolver; este carimba sem veto de exportação em `disputa_emissao.py:1724-1765`, e varre outras disputas abertas do colaborador em `:1805-1838`. | [nao verificado]
| `descarimbaria_exportada_fora` é DIFF da migração | **PROCEDENTE** a objeção: não é diferença antes/depois da migração. A exportada não migra (`migrar...py:196`); a medida relê estado atual e conta carimbo elegível que o juiz diz vivo (`:309-333`). Mede um eventual **ato de descarimbo** separado. |
| Cobertura completa por `desc` | **PROCEDENTE** a insuficiência: mede antes do commit (`:219-238`), portanto antes dos callbacks `signals.py:225`; mede só dias velho/novo dos registros recebidos (`:317-324`), enquanto o resolver pode carimbar outras perguntas/disputas (`disputa_emissao.py:1805-1838`). | [nao verificado]
| Exclusão automática de candidato compartilhado | **NAO PROCEDE**: a partição é de linhas de entrada, mas a query mede perguntas por `(colaborador,dia)`, não por PK (`:317-324`). Se uma pergunta cai no recorte aberto e no exportado, pode aparecer em ambos os números; não se garante disjunção. |
| Ordem de trava por transação inteira | **PROCEDENTE**: atomic externo em `:219`, loop em `:289`, cada porta trava um colaborador em `fio.py:947`; entrada ordenada por PK de pergunta (`:189`), não PK de colaborador. Não há trava inicial do conjunto, embora `models.py:2564-2576` preveja esse caso. Reversão repete o padrão (`:344-350`). |

Contraexemplo indireto construtível: pergunta aberta migra numa disputa cujo pai e destinos são terminais, sem perguntas visíveis ao colaborador, e cuja irmã exportada está morta para C6, sem materialização e sem tranca. O save agenda `fechar_beco_da_disputa`; após commit, a classificação de beco passa (`becos.py:25-33`), `fechar_beco` chama resolver para os destinos (`:45-54`), e a irmã exportada pode receber carimbo (`disputa_emissao.py:1724-1765`). Não há filtro de competência exportada nesse caminho; existência desse arranjo em produção [nao verificado]. [nao verificado]

Tabela completa local dos campos:

| Dado | Escritores | Leitores |
|---|---|---|
| `ln['fora']` | Inicial `None`: `migrar...py:132-133`; motivos de fonte/ambiguidade/colisão/aval: `:151-158`; sem competência/exportada: `:257-262`. | Seleção migra/fora: `:196-197`; saída: `:201-204`; partição `desc`: `:224-226`; evita reclassificação: `:255-256`. |
| `ln['comp']` | String `MM/AAAA[,MM/AAAA]`, derivada só para rótulo: `:250-254`. | Saída MIGRA `:199-200`, FORA `:202-203`, EXPORTADA_FORA `:264-266`. |
| `exportada_fora` do migrar | Não é retorno de `aplicar`: é texto calculado de `len(fora)` local em `_competencias`, `:247`, `:262`, `:267-268`. | Testes: `test_e1_v13d_exportada_fora.py:173`, `:187`, `:197`; `test_migrar_data_evento_dia_do_turno.py:204`. | [nao verificado]
| `desc` / `descarimbaria` | `_descarimbaria` incrementa por pergunta viva elegível: `:324-329`; resultado atribuído em `:225`. | Saída `:231`; bloqueio e rollback `:234-238`. |
| `desc_exp` / `descarimbaria_exportada_fora` | Resultado separado em `:226`, mesma função `:309-333`. | Apenas saída em `:232`; testes `test_e1_v13d_exportada_fora.py:184-186`, `test_migrar_data_evento_dia_do_turno.py:206-207`; não bloqueia o apply (`migrar...py:234`). | [nao verificado]
| Arquivo de reversão | Só apply autorizado localmente e havendo migra: `:215-218`; pasta/nome/JSON/permissão: `:270-282`. | `--reverter`: `:183-184`, `:335-363`; teste conteúdo/permissão e restauração: `test_migrar_data_evento_dia_do_turno.py:162-174`. | [nao verificado]

Detalhes do arquivo: é escrito **antes** do atomic de migração (`:216-219`), por isso não é desfeito pelo rollback. Inclui todas as linhas classificadas em `migra`, mesmo as depois recusadas pelo segundo veto (`:276-280`, `:294-297`). Guarda apenas PK, colaborador rotulado, data antiga, célula antiga e novo dia (`:276-277`); não guarda efeitos posteriores em irmãs, disputas, chamados, carimbos ou novas auditorias. A reversão devolve data pela porta e confere célula (`:350-356`), com rollback geral se falhar (`:359-360`).

A exportação que surgir entre classificação e segundo veto vira `RECUSA`, mas não atualiza `ln['fora']` (`:294-297`): o `exportada_fora` publicado anteriormente (`:267-268`) e o agrupamento `desc_exp` (`:224-226`) continuam refletindo a classificação inicial. [nao verificado]

O caminho direto da porta altera data, célula e lavra (`fio.py:959-965`), emite evento para célula velha (`:966-967`) e auditoria (`:968-970`); não altera diretamente o carimbo nessa gravação. Contudo, o save dispara os callbacks e o outbox (`signals.py:214-225`, `:237-244`); o outbox inclui dia/célula escritos (`models.py:3023-3025`) e é síncrono (`signals.py:232-244`).

Sobre semântica: `comp` local do migrar é string declarada e usada só como rótulo (`:244`, `:254`, `:264-266`); o fato de outro dict de outro fluxo usar tupla não prova colisão semântica por si só. `fora` é um discriminador de motivos, incluindo texto detalhado e sentinela exata `competencia_exportada` (`:158`, `:259-261`); a igualdade textual controla a partição da proteção em `:224-226`, portanto mudança de motivo sem migração desse leitor seria relevante.

## 08/11 00:31 · sessao d24aa19353cc · FIO-E0-V4-E1 · verificador · origem=Code

**NAO PROCEDE considerar os dois avais integralmente atendidos.** Há escritas alcançadas antes/depois da região que trava C, divergência entre universos multissemente e unitário, contador sem integração no DRY e incompatibilidade entre o agrupamento novo e a guarda 0053. (`app/chamados/services/validacao.py:585`; `app/chamados/services/disputa_emissao.py:1439`; `app/chamados/services/fio.py:1312`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:414`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:42`.)

A cópia não contém `.git`: sua correspondência exata com `ec06aa21` é `[nao verificado]`. Não executei testes com banco; resultados efetivos da suíte, números de produção e medições dos logs citados são `[nao verificado]`. As referências abaixo apontam para as linhas da cópia fornecida.

**1(a) — Fila `on_commit` e sentinela: PROCEDE o mecanismo local; NAO VERIFICADO o contrato completo de ciclo de vida.**

| Escritor/leitor | Local | Efeito sobre o registro |
|---|---|---|
| Construção do sentinela | `app/chamados/models.py:2290` | Guarda `cid`. |
| Execução do sentinela | `app/chamados/models.py:2293` | Retorna `None`; não remove a própria entrada. |
| `_registrar_travados` | `app/chamados/models.py:2318` | Registra pelo método `conn.on_commit`, depois de consultar os C já registrados em `:2313`. |
| `trava_na_ordem` | `app/chamados/models.py:2622`, `:2623` | Executa o SELECT de C e depois registra os IDs pedidos. |
| `colaboradores_travados` | `app/chamados/models.py:2303`, `:2305` | Fora de `atomic`, devolve vazio; dentro, lê sentinelas da fila. |
| `exigir_colaboradores_travados` | `app/chamados/models.py:2331`, `:2333` | Compara donos pedidos ao registro e levanta a exceção pelos ausentes. |
| Contador do chokepoint | `app/ponto/tests/test_selo_chokepoint_escrita.py:215` | Lê a fila excluindo sentinelas. |
| Mutante | `app/chamados/tests/test_e1_v13_ordem.py:33` | Impede a escrita do registro; não produz outro estado. |

Os demais registros de callbacks encontrados em produção são estes; acrescentam trabalho à fila, sem escrever o registro de C travado. (`app/chamados/models.py:2305`.)

| Escritor da fila | Callback registrado |
|---|---|
| `app/chamados/signals.py:144` | `_retomar` |
| `app/chamados/signals.py:225` | `_fechar` |
| `app/chamados/signals.py:266`, `:293` | `_silenciar` |
| `app/chamados/signals.py:322` | `_reavaliar` |
| `app/chamados/signals.py:356` | `_reconciliar` |
| `app/ponto/signals.py:272` | `_depois` |
| `app/ponto/signals.py:307` | `_reavaliar` |
| `app/ponto/portas/celula.py:502` | `_reavaliar_o_dia` |
| `app/ponto/services/fechamento.py:1817` | `_rodar` |

Os patches abaixo substituem a função `transaction.on_commit`; a escrita do sentinela usa outro ponto, `conn.on_commit`, e portanto não passa por esses patches. (`app/chamados/models.py:2318`.)

| Patch “roda agora” | Referência |
|---|---|
| Teste novo do registro | `app/chamados/tests/test_e1_v13_ordem.py:124` |
| Abono de feriado | `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:185` |
| Ordem regeneração/julgamento | `app/ponto/tests/test_ordem_regen_julga_antes_do_dinheiro.py:84` |
| Edição do dia | `app/ponto/tests/test_o10b_editar_dia_troca.py:115`, `:121`, `:192`, `:367`, `:370` |

O inventário de `captureOnCommitCallbacks` é o seguinte. Todos pedem `execute=True`, salvo as duas exceções indicadas; executar o sentinela só realiza o no-op de `__call__`. A implementação do Django que determina retenção/esvaziamento da fila não está na cópia: esse efeito é `[nao verificado]`. (`app/chamados/models.py:2293`; `app/requirements.txt:13`.)

| Arquivo | Linhas |
|---|---|
| `app/api/tests/test_atestado_na_resposta.py` | 51 |
| `app/chamados/tests/test_jv1_volta4_posto_do_dia.py` | 46, 119, 135 |
| `app/chamados/tests/test_jv3_reconciliar_vira_vigia.py` | 71, 118, 144 |
| `app/chamados/tests/test_varredura_becos_evento_pergunta.py` | 55, 65, 76, 86, 88 |
| `app/chamados/tests/test_e2_celula_regenerada_reavalia.py` | 70, 146, 202, 204; **226 com `execute=False`** |
| `app/chamados/tests/test_jv1_volta3_municipio_e_quem_chegou.py` | 33, 39, 66 |
| `app/chamados/tests/test_jv1_feriado_vira_vigia.py` | 87, 163 |
| `app/chamados/tests/test_laco_ausencia_aberta_nao_vai_e_volta.py` | 66 |
| `app/chamados/tests/test_s4_pendente_suspende_cobranca.py` | 86 |
| `app/chamados/tests/test_pep_reentrada.py` | 101 |
| `app/chamados/tests/test_c20_juiz3_outbox.py` | 242 |
| `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py` | 57, 68, 110, 134, 176 |
| `app/chamados/tests/test_reg_externa_disputa_beco.py` | 156 |
| `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py` | 87, 131, 182, 188, 242, 260, 265; **328 com `execute=False`** |
| `app/chamados/tests/test_varredura_isentos_evento.py` | 38, 52, 65, 71, 82 |
| `app/chamados/tests/test_varredura_feriado_evento.py` | 58, 83 |
| `app/ponto/tests/test_reconciliar_geofence.py` | 102 |
| `app/ponto/tests/test_evento_justificativa_rejulga_o_dia.py` | 31, 42 |
| `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py` | 115, 122, 136, 155, 174 |
| `app/ponto/tests/test_varredura_classe_b_ausencia.py` | 103, 114, 124 |
| `app/ponto/tests/test_d3_impressao_cobre_insumos.py` | 274 |

Há contagem filtrada pelo nome do callback em `app/chamados/tests/test_e2_celula_regenerada_reavalia.py:149`, `:228`, `:229`. Já `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:331` conta todos e executa o primeiro em `:333`, sem excluir sentinelas; interferência efetiva nesse cenário é `[nao verificado]`.

| Operação de término/rollback | Evidência e limite |
|---|---|
| Rollback de `atomic` interno | O teste exige desaparecimento do registro em `app/chamados/tests/test_e1_v13_ordem.py:107`, `:113`. Resultado executado: `[nao verificado]`. |
| Saída normal de `atomic` interno | O teste exige sobrevivência até a transação externa em `app/chamados/tests/test_e1_v13_ordem.py:114`, `:116`. Resultado executado: `[nao verificado]`. |
| Savepoint manual em produção | C é travado **antes** do savepoint: `app/chamados/services/limbo_folgas.py:274`, `:277`; rollback em `:285`. |
| Outros rollbacks manuais | `app/chamados/tests/test_e1_v9.py:62`; `app/chamados/tests/test_matriz_guarda_clt.py:100`; `app/ponto/tests/test_d3_impressao_cobre_insumos.py:132`, `:294`. |
| Registro adquirido dentro de savepoint manual | O teste novo usa `atomic`, não cobre esse caso: `app/chamados/tests/test_e1_v13_ordem.py:107`. Equivalência alegada: `[nao verificado]`. |
| Commit, rollback completo e `run_and_clear_commit_hooks` | A docstring afirma limpeza, mas a implementação do Django não está na cópia; funcionamento interno e ausência de chamadas indiretas: `[nao verificado]`. (`app/chamados/models.py:2284`.) |

**1(b) — Escritores/leitores do selo: PROCEDE localmente; NAO PROCEDE como selo geral D/CH/P.**

| Elemento | Escritor/chamador | Leitor/efeito |
|---|---|---|
| `colaboradores_travados` | Não mantém armazenamento próprio; lê a fila. `app/chamados/models.py:2305` | `_registrar_travados` em `:2313`; `exigir_colaboradores_travados` em `:2331`. |
| `_registrar_travados` | Chamado por `trava_na_ordem`, após o SELECT. `app/chamados/models.py:2623` | Acrescenta sentinelas em `:2318`. |
| `exigir_colaboradores_travados` | Lote em `app/chamados/models.py:3104`; resposta em `app/chamados/services/disputa_emissao.py:1393`; correção em `:1510`; segunda instância em `app/chamados/reconciliador.py:593` | Recusa por diferença de conjuntos, sem inspecionar nome de chamador. `app/chamados/models.py:2331`. |
| `ColaboradorNaoTravado` | Levantada em `app/chamados/models.py:2333` | Esperada nos testes em `app/chamados/tests/test_e1_v13_ordem.py:80`, `:88`, `:96`; engolida pelo `except Exception` da segunda instância em `app/chamados/reconciliador.py:600`. |
| `descarimbar_em_lote` | Chamado pela ressuscitação em `app/chamados/models.py:1555` e pela correção em `app/chamados/services/disputa_emissao.py:1526` | Exige registro antes do UPDATE em `app/chamados/models.py:3104`, `:3109`. |

Os testes também leem o registro nas premissas e nas verificações de aquisição, rollback e patch. (`app/chamados/tests/test_e1_v13_ordem.py:56`, `:66`, `:109`, `:113`, `:116`, `:126`, `:144`, `:151`.)

Não há segundo armazenamento no mecanismo apresentado: o estado vem da fila. Entretanto, o registro atesta os **IDs pedidos**, pois o resultado do SELECT é descartado e `_cids` é registrado integralmente; não atesta quais linhas foram efetivamente retornadas. (`app/chamados/models.py:2305`, `:2622`, `:2623`.)

**1(c) — Mapa do fecho: PROCEDE a centralização parcial; NAO PROCEDE que todos os leitores usem o mesmo universo.**

| Função | Quem chama/lê |
|---|---|
| `chave_do_dia` | Vizinhos, base, chave do cache, pré-carga e sementes: `app/chamados/services/fio.py:1180`, `:1197`, `:1203`, `:1308`, `:1312`. |
| `_base_da_chave` | Caminhada sem dia, limite do fecho e sementes: `app/chamados/services/fio.py:1224`, `:1277`, `:1281`, `:1312`. |
| `_chave_do_passo` | Consulta e pré-carga do cache: `app/chamados/services/fio.py:1208`, `:1307`. |
| `_vizinhos_em_cache` | Caminhada sem dia e fecho: `app/chamados/services/fio.py:1223`, `:1278`. |
| `_dias_alcancados_sem_dia` | Inferência do dia, veto da ponte e pré-seleção de sementes: `app/chamados/services/fio.py:1273`, `:1283`, `:1313`. |
| `fecho_do_fato` | Plano, universo da porta, contador, universo do grupo e da linha: `app/chamados/services/fio.py:1051`, `:1317`, `:1344`, `:1357`, `:1365`. |
| `recusadas` | Escrito pelo fecho em `app/chamados/services/fio.py:1288`; agregado pelo universo em `:1316`, `:1320`; lido pelo contador em `:1345` e por `_nascer` em `app/chamados/models.py:2809`. |
| `dia` | Passado pela porta em `app/chamados/services/fio.py:1317`; combinado com os dias das sementes em `:1269`. |
| `_viz` | Criado/reutilizado em `app/chamados/services/fio.py:1261`; pré-carregado em `:1307`; compartilhado com caminhada e fecho em `:1313`, `:1317`; preenchido por consulta real em `:1210`. |
| `universo_do_fato` | `_nascer` em `app/chamados/models.py:2807`; `_candidata_reusavel` em `app/chamados/services/declaracao_texto.py:244`. |
| `_candidata_reusavel` | Declaração do colaborador e aplicação de texto: `app/chamados/services/declaracao_texto.py:335`, `:632`. |
| `universo_do_grupo` | Partição em `app/chamados/services/fio.py:1421`; backfill antes das travas e antes da eleição em `app/chamados/management/commands/backfill_decisoes_pergunta.py:213`, `:226`. |
| `universo_da_linha` | `_colisao_ao_datar` e migração de resolvedoras: `app/chamados/services/fio.py:891`, `:1548`. |
| `grupos_de_colisao` | Backfill DRY em `app/chamados/management/commands/backfill_decisoes_pergunta.py:360`; dedup em `app/chamados/management/commands/dedup_perguntas_fato.py:58`. |
| `fecho_cruza_dias` | Chamadas explícitas nos testes: `app/chamados/tests/test_e1_v13.py:114`, `:116`, `:195`, `:208`, `:216`; o resumo DRY prometido não o inclui em `app/chamados/management/commands/backfill_decisoes_pergunta.py:414`. |
| Migração 0052 | Adiciona FK nula; não calcula fecho. `app/chamados/migrations/0052_pergunta_batida_origem.py:18`. |
| Migração 0053 | Conta colisões diretamente pelas chaves dos índices. `app/chamados/migrations/0053_pergunta_unica_por_familia.py:39`, `:42`. |

Há caminhos que continuam lendo um passo/domínio, sem calcular o fecho:

| Caminho | Referência |
|---|---|
| `fato_livre_para` lê `do_fato` do episódio, inclusive outro dia | `app/chamados/models.py:2725`; diferença explicitada em `app/chamados/tests/test_e1_v13.py:148`, `:153`. |
| Projeção e propostas da tela leem `do_fato` | `app/chamados/services/declaracao_texto.py:446`, `:606`. |
| Lote da tela lê `linhas_dos_fatos` | `app/chamados/services/declaracao_texto.py:559`. |
| Gatilho de migração de data consulta `do_fato` sem episódio | `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:152`. |
| Outros leitores de domínio por marco | `app/chamados/management/commands/disparar_perguntas_competencia.py:215`; `app/chamados/services/pergunta_no_app.py:113`. |

A seleção de sementes repete a classificação de linha nula/ponte antes de chamar o fecho; não é outro algoritmo de caminhada, mas são duas aplicações da condição. (`app/chamados/services/fio.py:1312`, `:1313`, comparadas com `:1281`, `:1283`.)

A eleição continua usando `canonica_do_grupo` nos caminhos examinados; o problema está no universo entregue a ela. (`app/chamados/models.py:2817`; `app/chamados/services/declaracao_texto.py:247`; `app/chamados/services/fio.py:893`, `:1552`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:237`; `app/chamados/management/commands/dedup_perguntas_fato.py:76`.)

**1(d) — `data_evento=NULL`: PROCEDE que ainda é persistível; NAO PROCEDE tratar NULL, conversa e dia conhecido como equivalentes.**

| Escritor/capacidade | O que permite |
|---|---|
| Schema | `DateField(null=True)`: permite ausência de data. `app/chamados/models.py:2379`; introduzido assim em `app/chamados/migrations/0025_perguntadisputa_data_evento.py:16`. |
| `PerguntaDisputa.save` | Não contém recusa de data nula antes de gravar. `app/chamados/models.py:3258`, `:3268`. |
| `_nascer` privado | Recebe `data_evento` e o passa ao `create`, sem sua própria validação de NULL. `app/chamados/models.py:2756`, `:2864`. |
| `abrir` público | Recusa NULL antes de `_nascer`. `app/chamados/models.py:2739`. |
| `abrir_conversa` público | Preenche dia civil; conversa atual não significa NULL. `app/chamados/models.py:2752`. |
| `reapontar_resolvedora` | Atribui dia conhecido à linha nula; as atribuições de volta a `None` ficam nos retornos de recusa, antes do save. `app/chamados/services/fio.py:840`, `:852`, `:861`, `:870`. |
| `rechavear_data_evento` | Recusa novo dia nulo e grava dia conhecido. `app/chamados/services/fio.py:941`, `:959`, `:965`. |
| Fixture usada pelos testes novos | `_p(..., dia=None)` persiste NULL pelo ORM. `app/chamados/tests/test_e1_v9.py:138`, `:139`; chamadas novas em `app/chamados/tests/test_e1_v13.py:192`, `:201`. |
| Fixtures por UPDATE | Gravam explicitamente NULL. `app/chamados/tests/test_e1_v5_portas_backfill.py:62`, `:101`; `app/chamados/tests/test_porta_reapontamento_pergunta.py:258`. |

Os leitores novos fazem três tratamentos: inferem um dia alcançável pela parte nula; aceitam a nula no fato desse dia; recusam a ponte que alcança dois dias. A coluna continua nula durante essas operações. (`app/chamados/services/fio.py:1213`, `:1273`, `:1282`, `:1312`.)

Isso não equivale ao tratamento do backfill de decisão: uma linha nula continua sem competência e fora daquele apply. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:94`, `:102`.)

Também não equivale ao domínio exato do dia: `linhas_dos_fatos` exige igualdade de `data_evento`, enquanto o fecho admite irmã nula. A igualdade prometida na docstring é, portanto, falsa nesse caso. (`app/chamados/models.py:2708`; `app/chamados/services/fio.py:1251`; `app/chamados/tests/test_e1_v13.py:194`.)

`data_evento` é data, não instante; os instantes estão em campos como `respondida_em`. Para fato datado, o contrato é dia do turno; para conversa, dia civil, distinguido por `conversa`. (`app/chamados/models.py:2379`, `:2404`, `:2735`, `:2752`; `app/chamados/juizes.py:333`, `:336`.)

Há leitores antigos ainda reconhecidos como problemáticos: `data_do_fato_de` usa a data crua e `expira_em` documenta conversa como NULL. (`app/chamados/services/fio.py:87`, `:1133`, `:1142`; `app/chamados/veredito_lavrado.py:107`, `:109`.)

**1(e) — `FatoEmOutroDia`: PROCEDE o escape possível pelo caminho da batida; perda da batida é NAO VERIFICADO.**

| Etapa | Referência e comportamento |
|---|---|
| Classe | Herda `ResolvedoraCruzada`. `app/chamados/models.py:2337`. |
| Raise | `_nascer` levanta quando o universo está vazio e há recusada do episódio solicitado. `app/chamados/models.py:2809`, `:2813`. |
| Porta | `abrir` retorna `_nascer`, sem captura. `app/chamados/models.py:2741`. |
| Chamadores diretos de `abrir` | Emissão em `app/chamados/services/disputa_emissao.py:444`, `:834`; declarações em `app/chamados/services/declaracao_texto.py:348`, `:636`. |
| Cadeia BATIDA | `gerar_perguntas_disputa` chama `abrir`; seu try de reatar começa depois. `app/chamados/services/disputa_emissao.py:444`, `:462`. |
| Emissor superior | Chama geração no nascimento e na agregação. `app/chamados/services/disputa_emissao.py:670`, `:682`. |
| Bordas | Os dois kernels chamam esse emissor sem captura naquele caminho. `app/api/views.py:794`; `app/api/views_core.py:794`. |
| Teste novo | Captura diretamente a exceção e verifica P, sem exercitar a borda da batida. `app/chamados/tests/test_e1_v13.py:142`, `:146`. |

Não encontrei captura nominal de `FatoEmOutroDia`/`ResolvedoraCruzada` em produção `[nao verificado]`; a cadeia acima permite propagação até a borda. O registro da batida precede essa chamada, portanto não se pode concluir que a batida seria perdida: o fluxo de sucesso e os efeitos seguintes podem ser interrompidos; HTTP final, rollback externo e ocorrência real são `[nao verificado]`. (`app/api/views.py:684`, `:794`, `:799`; `app/ponto/registro_batida.py:157`.)

**2 — Aval E1-FECHO-NA-IDENTIDADE: NAO PROCEDE integralmente.**

- **PROCEDE o bloqueio de vizinho datado de outro dia na expansão normal.** O teste compara também colaborador, motivo e instância pela base; não é apenas `data_evento` solto. Para NULL, porém, usa identidade contextual inferida, não igualdade literal da chave completa. (`app/chamados/services/fio.py:1195`, `:1277`, `:1281`, `:1283`.)

- **NAO PROCEDE a simetria universal declarada.** Contraexemplo deduzido: apenas A1(alfa,d1) e B1(beta,d1), mesmo colaborador/motivo, sem acervo. `universo_do_fato(..., batida_origem_id=None)` semeia ambas; o fecho mantém essas sementes. Entretanto, `universo_da_linha(A1)` vê só alfa e o de B1 só beta. Assim, membros do universo multissemente `{A1,B1}` veem universos unitários diferentes. (`app/chamados/services/fio.py:1303`, `:1312`, `:1262`, `:1365`; `app/chamados/models.py:2675`, `:2677`.)

- **NAO PROCEDE a validação completa das sementes.** Elas entram antes do filtro de vizinhos; a assertiva verifica dias, mas não identidade completa nem conexão entre sementes. (`app/chamados/services/fio.py:1262`, `:1269`, `:1270`.)

- **PROCEDE a chave do cache para os parâmetros escalares examinados.** Ela contém PK, chave do dia e episódio; mudar o dia em memória muda a chave. A pré-carga exige os parâmetros da consulta real; a instância consultada deriva do motivo. Não encontrei contraexemplo de vizinhos incorretos nesse percurso; execução Django/DB é `[nao verificado]`. (`app/chamados/services/fio.py:1203`, `:1180`, `:1307`, `:1308`; `app/chamados/models.py:2707`, `:2708`.)

- **NAO PROCEDE a compatibilidade de `universo_do_fato` com colaborador objeto.** `alvo` conserva o objeto, mas `chave_do_dia` contém o ID: a seleção rejeita as linhas e a pré-carga também não casa. Isso é falha de normalização/seleção, não devolução de vizinhos errados pelo cache. Os dois chamadores atuais passam ID. (`app/chamados/services/fio.py:1302`, `:1191`, `:1308`, `:1312`; `app/chamados/models.py:2807`; `app/chamados/services/declaracao_texto.py:244`.)

- **NAO PROCEDE a recusa declarada com contador integrado.** O contador existe e chama o fecho real, mas o resumo DRY não o chama/imprime; plano e universos normalmente descartam `recusadas`. (`app/chamados/services/fio.py:1344`, `:1051`, `:1357`, `:1365`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:414`.)

- **NAO PROCEDE a concordância DRY/0053.** Para o mesmo episódio em dois dias distintos, ambos no horizonte, o agrupador divide em dois fechos unitários e os descarta; a guarda 0053 continua contando duplicidade de episódio sem dia e recusa subir. Os testes novos usam D0 abaixo do horizonte, evitando esse caso. (`app/chamados/services/fio.py:1393`, `:1421`, `:1427`; `app/chamados/migrations/0053_pergunta_unica_por_fa

[truncado em 20000 de 27970 caracteres]

## 08/11 00:27 · sessao 27af10e89625 · - · - · origem=Ronald (tmux)

**Item 1(c): mapa de produção**

| Função/campo | Leitores/chamadores |
|---|---|
| `chave_do_dia` | `_vizinhos_do_fato` `app/chamados/services/fio.py:1180`; `_base_da_chave` `:1197`; `_chave_do_passo` `:1203`; pré-carga de cache `:1308`; seleção de sementes `:1312`. |
| `_base_da_chave` | caminhada sem dia `fio.py:1224`; limite do fecho `:1277`, `:1281`; seleção de sementes `:1312`. |
| `_chave_do_passo` | `_vizinhos_em_cache` `fio.py:1208`; pré-carga em `universo_do_fato` `:1307`. |
| `_vizinhos_em_cache` | `_dias_alcancados_sem_dia` `fio.py:1223`; `fecho_do_fato` `:1278`. |
| `_dias_alcancados_sem_dia` | inferência do dia do fecho `fio.py:1273`; veto da ponte sem dia `:1283`; seleção prévia de sementes `:1313`. |
| `fecho_do_fato` | veto `episodios_unidos` de `plano_de_retratacao` `fio.py:1051`; `universo_do_fato` `:1317`; contador `:1344`; `universo_do_grupo` `:1357`; `universo_da_linha` `:1365`. |
| `recusadas` | coletor opcional escrito em `fio.py:1288-1290`; `universo_do_fato` agrega recusas `:1316-1320`; `_nascer` lê recusas para `FatoEmOutroDia` `app/chamados/models.py:2806-2815`; contador lê lista não vazia `fio.py:1343-1346`. |
| `dia` | porta passa o dia pedido `fio.py:1317`; fecho o une aos dias das sementes e exige no máximo um `:1269-1274`. |
| `_viz` | criado/reutilizado `fio.py:1261`, pré-carregado `:1307-1309`, passado à caminhada sem dia `:1313` e ao fecho `:1317`; consulta real de cache ausente `:1208-1211`. |
| `universo_do_fato` | `_nascer` `models.py:2807`; `_candidata_reusavel` `app/chamados/services/declaracao_texto.py:244`. |
| `universo_do_grupo` | partição de grupos `fio.py:1421`; apply backfill lê universo antes das travas de D/P `app/chamados/management/commands/backfill_decisoes_pergunta.py:213`, e relê antes de eleger canônica `:226`. |
| `universo_da_linha` | `_colisao_ao_datar` `fio.py:891`; `_migrar_sob_trava` `:1548`. |
| `grupos_de_colisao` | backfill DRY `backfill_decisoes_pergunta.py:360`; dedup `app/chamados/management/commands/dedup_perguntas_fato.py:58`. |
| `fecho_cruza_dias` | **Nenhum chamador de produção encontrado**; os chamadores reais são testes em `app/chamados/tests/test_e1_v13.py:114`, `:116`, `:195`, `:208`, `:216`. A doc promete o RESUMO DRY `fio.py:1329`, mas o resumo real não inclui a função/contador `backfill_decisoes_pergunta.py:414-422`. |
| Migração 0052 | Só adiciona FK nula `app/chamados/migrations/0052_pergunta_batida_origem.py:18-21`; não chama fecho. |
| Migração 0053 | Juízo SQL congelado próprio por família, sem chamar fecho `app/chamados/migrations/0053_pergunta_unica_por_familia.py:36-46`; episódio agrupado **sem dia** `:42-43`; índice idem `:64-66`. |

**Outros leitores por caminho de um passo, sem `fecho_do_fato`:**

- `fato_livre_para` consulta diretamente `do_fato` e exclui a própria PK — deliberadamente preserva a identidade do índice do episódio, inclusive outro dia (`models.py:2721-2725`; teste declara a diferença `test_e1_v13.py:151-158`). [nao verificado]
- `declaracao_texto` tela/projeção consulta `do_fato` em `:446`, `:606`; lote usa `linhas_dos_fatos` em `:559-562`; escolhe canônica via `_segura_da_lista` `:264-269`.
- Gatilho de migração de data detecta colisão por `do_fato` **sem episódio** `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:152-154`, enquanto a porta usa episódio por `fato_livre_para` (`models.py:2725`).
- `disparar_perguntas_competencia` usa `linhas_dos_fatos` `app/chamados/management/commands/disparar_perguntas_competencia.py:215`; `pergunta_no_app` usa o mesmo domínio `app/chamados/services/pergunta_no_app.py:113`; ambos para motivos de marco.

**Item 2: achados**

1. **NÃO PROCEDE a propriedade universal “todo membro vê o mesmo universo”.** Com `A1(alfa,d1)` e `B1(beta,d1)`, sem acervo legado, `universo_do_fato(cid,gps,d1,batida_origem_id=None)` consulta ambas e semeia ambas (`fio.py:1303`, `:1312-1317`); o fecho aceita sementes incondicionalmente (`:1262-1266`). Porém `universo_da_linha(A1)` só consulta episódio alfa + legado sem episódio (`models.py:2675-2679`) e vê `{A1}`; B1 vê `{B1}`. Portanto o universo multissemente `{A1,B1}` não é o componente visto por cada membro. A assertiva verifica apenas dias das sementes, sem identidade completa/conectividade (`fio.py:1269-1270`). Contraexemplo reproduzido extraindo as funções AST, com `do_fato` simulado fiel; execução Django/DB `[nao verificado]`.

2. **PROCEDE parcialmente o limite por chave:** vizinhos são filtrados pela base `(colaborador,motivo,instancia)` e pelo dia escolhido (`fio.py:1277-1285`); conversa é excluída da consulta de vizinhos (`:1177-1178`) e isolada em `universo_da_linha` (`:1363-1365`). Entretanto a identidade BATIDA do índice é `(batida_origem,motivo)`, sem dia (`models.py:2392-2395`; `0053:64-66`), e o novo fecho acrescenta corte por dia expressamente (`fio.py:1188-1189`). Não afirmar que todos os fatos de BATIDA no mesmo dia passam a ter universo único: contraexemplo acima.

3. **NÃO PROCEDE a igualdade documentada com `linhas_dos_fatos` em todo caso:** `fio.py:1251-1253` promete igualdade; uma linha datada e sua irmã sem dia entram juntas no fecho (`test_e1_v13.py:190-194`), mas `linhas_dos_fatos` compara `data_evento` exatamente ao dia pedido (`models.py:2708-2710`), excluindo a irmã nula.

4. **Cache: nenhum erro concreto encontrado no percurso normal com PK escalar.** A chave incorpora PK, chave do dia e episódio (`fio.py:1203`); dia alterado em memória gera outra chave; a pré-carga exige os mesmos parâmetros efetivos do `do_fato` (`:1307-1309` frente `:1180`). Omitir `instancia_marco` da condição de pré-carga não muda a consulta: a instância consultada deriva do motivo em `models.py:2707-2708`. Teste AST de 511 subconjuntos (NULL/d1/d2 × sem episódio/alfa/beta) não encontrou assimetria nos **fechos unitários**; execução DB `[nao verificado]`. [nao verificado]

5. **NÃO PROCEDE compatibilidade com colaborador objeto em `universo_do_fato`.** `do_fato` recebe argumento para filtro de FK (`models.py:2709`), mas `alvo` mantém o objeto cru (`fio.py:1302`), comparado com `chave_do_dia` que contém ID (`:1191-1192`, `:1312`). Resultado: linhas consultadas são recusadas e universo fica vazio. O cache não devolve vizinhos errados: a pré-carga também não casa (`:1308`). Os dois chamadores de produção passam `disputa.colaborador_id`, escalar (`models.py:2807`; `declaracao_texto.py:244`), logo impacto nesses chamadores `[nao verificado]`.

6. **NÃO PROCEDE “recusa declarada com contador” na integração de produção.** A função contador não é chamada pelo DRY (`backfill_decisoes_pergunta.py:414-422`); as outras portas leem fecho descartando `recusadas` (`fio.py:1051`, `:1357`, `:1365`, `:1421`). Só `_nascer` transforma determinada recusa em exceção (`models.py:2809-2815`).

**Pouso/pré-requisito e regressão** [nao verificado]

- **NÃO PROCEDE equivalência DRY/guarda 0053.** Mesmo episódio em dois dias distintos, ambos no horizonte, sem legado: `grupos_de_colisao` detecta duplicação por episódio (`fio.py:1393-1395`), divide pelos fechos (`:1421`) e descarta as duas partes unitárias (`:1427-1428`), devolvendo zero colisão. O guarda 0053 ainda conta um grupo duplicado de episódio sem dia e recusa a migração (`0053:42-46`). Os testes novos usam `D0` abaixo do horizonte (`test_e1_v13.py:24`) e não cobrem este impasse. [nao verificado]
- **NÃO PROCEDE coerência da suite existente:** `test_e1_v10.py:89-91` exige o fecho antigo atravessando DIA/DIA2; `test_e1_v11.py:86-91` exige a ponte pelo outro dia para vetar retratação. Essas expectativas contradizem o novo corte em `fio.py:1281`. Falha efetivamente executada `[nao verificado]`. [nao verificado]
- A seleção de sementes replica a classificação de linha sem dia/ponte antes da função de fecho (`fio.py:1312-1313` versus `:1281-1283`); isto é uma segunda aplicação da regra, embora ambas usem `_dias_alcancados_sem_dia`. Não há segundo algoritmo independente para eleger canônica nos caminhos auditados: todos continuam usando `canonica_do_grupo` (`models.py:2817`; `declaracao_texto.py:247`; `fio.py:893`, `:1552`; backfill `:237`; dedup `:76`).

## 08/11 00:27 · sessao 3770bb7fc195 · - · - · origem=Ronald (tmux)

Item 3 — **NAO PROCEDE como garantia por transação de todas as portas D/CH/P**; os quatro pontos novos têm guarda local, mas caminhos alcançados deixam escritas antes/depois da região protegida. (`app/chamados/services/validacao.py:585`, `app/chamados/services/disputa_emissao.py:1439`, `app/chamados/models.py:2227`)

| Porta nomeada | C antes da escrita local | Limite |
|---|---|---|
| `descarimbar_em_lote` | Exige os donos antes do UPDATE; não adquire trava, depende do chamador. `app/chamados/models.py:3104`, `:3109` | `ressuscitar_perguntas` adquire C no atomic antes de Decisão/Pergunta. `app/chamados/models.py:1547`, `:1552`, `:1553`, `:1555` |
| `reabrir_para_correcao` | Atomic próprio, reúne donos, trava, exige e só então insere Decisão e atualiza Pergunta. `app/chamados/services/disputa_emissao.py:1501`, `:1505`, `:1508`, `:1509`, `:1510`, `:1511`, `:1528` | Caller `recusar_pergunta` escreve P antes de entrar nessa porta; caller `reabrir_por_colab` escreve D depois de sair dela. `app/chamados/services/validacao.py:585`, `:588`; `app/chamados/models.py:2224`, `:2227` |
| `views:1283 → responder_core` | Atomic, trava e exige C antes de `p.save` e Decisão. `app/chamados/views.py:1283`; `app/chamados/services/disputa_emissao.py:1389`, `:1392`, `:1393`, `:1394`, `:1397` | Ramo `FOLGA_CONTESTA`/`OUTRO_MOTIVO` salva P fora desse atomic, sem readquirir C. `app/chamados/services/disputa_emissao.py:1428`, `:1439` |
| Segunda instância da batida | `cartorio` chama reconciliador; cada carimbo entra em atomic, trava e exige C antes de `carimbar`. `app/ponto/services/cartorio.py:958`; `app/chamados/reconciliador.py:591`, `:592`, `:593`, `:594` | Exceção é capturada por pergunta; fechamento D posterior usa `.fechar()`, que abre atomic e adquire C próprio. `app/chamados/reconciliador.py:600`, `:611`; `app/chamados/models.py:1897`, `:1902` |

**Contraexemplos concretos no escopo expandido:**

1. **Escrita antes da trava:** `detalhe_chamado_local → acao_recusar_pergunta → validacao.recusar_pergunta` salva `recusa_motivo` antes de chamar `reabrir_para_correcao`; esse save de Pergunta apenas abre atomic e grava, sem selo nem trava C. (`app/chamados/views.py:359`, `:518`; `app/chamados/services/acoes_disputa.py:80`; `app/chamados/services/validacao.py:533`, `:585`, `:588`; `app/chamados/models.py:3267`, `:3268`)

2. **Escrita em nova transação depois da trava anterior:** resposta `FOLGA_CONTESTA`/`OUTRO_MOTIVO` salva `recusa_motivo` depois que termina o atomic da resposta; a view HTML tem somente `@login_required`. (`app/chamados/services/disputa_emissao.py:1389`, `:1397`, `:1428`, `:1439`; `app/chamados/views.py:1227`, `:1228`, `:1283`; banco configurado sem `ATOMIC_REQUESTS`: `app/config/settings/prod.py:9`, `app/config/settings/saas.py:83`)

3. **Outra escrita posterior:** `reabrir_por_colab` chama helper que termina seu atomic e depois salva `Disputa.reaberturas_colab`; as bordas web/API não abrem atomic. (`app/chamados/models.py:2224`, `:2227`; `app/chamados/services/disputa_emissao.py:1501`, `:1530`; `app/chamados/views.py:1317`, `:1334`; `app/api/views.py:2700`, `:2718`)

4. **Reabertura D fechada depende de atomic externo:** `ressuscitar_perguntas` encerra atomic após descarimbar e depois chama `reabrir_sistema`; esta salva D sem adquirir/exigir C. O caminho fica protegido quando existe atomic externo, mas a função isolada não garante isso. (`app/chamados/models.py:1547`, `:1555`, `:1582`, `:1583`, `:2155`, `:2169`)

5. **Absorção alcançada pela resposta:** depois do atomic da resposta, o ramo ausência chama materializador; cada `q.carimbar` abre atomic próprio sem adquirir/exigir C; `carimbar` salva diretamente e `PerguntaDisputa.save` também não tem o selo. (`app/chamados/services/disputa_emissao.py:1424`, `:1427`; `app/chamados/services/materializacao.py:557`, `:558`; `app/chamados/models.py:2998`, `:3267`, `:3268`)

6. **Porta de conversa escreve sem C:** `_nascer` trava somente `not conversa`; `abrir_conversa` passa `conversa=True` e chega ao create. (`app/chamados/models.py:2752`, `:2766`, `:2767`, `:2864`)

**Dois colaboradores:** o lote recebido por uma chamada de `trava_na_ordem` é ordenado por pk na consulta; `reabrir_para_correcao` usa essa forma. Não existe ali recusa de aquisição incremental invertida na mesma transação, por exemplo `trava_na_ordem(C_maior)` seguida de `trava_na_ordem(C_menor)`: o método ordena somente cada chamada. O teste verifica uma chamada de lote, não essa propriedade global. (`app/chamados/models.py:2614`, `:2615`, `:2622`, `:2623`; `app/chamados/services/disputa_emissao.py:1509`; `app/chamados/tests/test_e1_v13_ordem.py:59`, `:71`, `:73`)

**Selo/mutantes:** as quatro guardas são runtime, por conjunto de ids, não por nome de caller; mutantes exercitam reabrir, responder e reconciliador. O caso descarimbar só chama a porta sem registro — o patch atuou na fixture, não durante a porta — e não verifica estado depois da exceção. Todos os casos usam `TestCase`, portanto não demonstram a separação entre transações das escritas posteriores listadas. (`app/chamados/models.py:2326`, `:2330`, `:2331`, `:2333`; `app/chamados/tests/test_e1_v13_ordem.py:33`, `:38`, `:75`, `:80`, `:86`, `:89`, `:91`, `:96`, `:130`, `:153`, `:156`)

Item 1(e) — **PROCEDE: há escape possível da exceção pelo caminho da batida; perda de batida não demonstrada.** (`app/chamados/models.py:2813`; `app/api/views.py:794`; `app/ponto/registro_batida.py:157`)

| Elemento | Local e efeito |
|---|---|
| Classe | `FatoEmOutroDia` herda `ResolvedoraCruzada`. `app/chamados/models.py:2337` |
| Quem levanta | Único raise, `_nascer`, quando universo pedido está vazio e há recusada com o episódio solicitado. `app/chamados/models.py:2807`, `:2809`, `:2813` |
| Porta | `abrir → _nascer`, sem captura. `app/chamados/models.py:2741` |
| Emissão BATIDA | `gerar_perguntas_disputa → abrir`; o try existente começa só depois, no reatar de pergunta já criada. `app/chamados/services/disputa_emissao.py:444`, `:462`, `:466` |
| Emissor superior | `criar_ou_atualizar_disputa → gerar_perguntas_disputa`, tanto nascimento quanto agregação, sem captura. `app/chamados/services/disputa_emissao.py:670`, `:682` |
| Bordas da batida | Os dois kernels chamam emissor sem try naquele trecho. `app/api/views.py:794`; `app/api/views_core.py:794` |
| Escrita da batida | O kernel já chamou `registrar_batida`; o chokepoint já executou `Batida.objects.create`. `app/api/views.py:684`; `app/api/views_core.py:681`; `app/ponto/registro_batida.py:157` |
| Testemunha | Teste sintético exige `FatoEmOutroDia` diretamente da porta e ausência de criação/reapontamento P. `app/chamados/tests/test_e1_v13.py:137`, `:142`, `:146` |

Não encontrei captura nominal de `FatoEmOutroDia` ou `ResolvedoraCruzada` em produção; os raises de `ResolvedoraCruzada` já existentes também ficam em `_nascer`. (`app/chamados/models.py:2786`, `:2828`; captura global externa: `[nao verificado]`)

Assim, o novo raise pode interromper o retorno de sucesso e os efeitos posteriores da API, embora a batida já tenha sido escrita; ocorrência real, HTTP final e qualquer rollback externo em produção são `[nao verificado]`. (`app/api/views.py:684`, `:794`, `:799`; `app/ponto/registro_batida.py:157`)

Classes de reprova pertinentes: **pouso antes do pré-requisito** em `validacao:585`; **selo parcial** nos quatro locais, sem proteção dos saves alcançados; **ordem por transação incompleta** nas escritas posteriores e na ordenação apenas por chamada. (`app/chamados/services/validacao.py:585`; `app/chamados/models.py:3267`; `app/chamados/services/disputa_emissao.py:1439`; `app/chamados/models.py:2622`)

## 08/11 00:25 · sessao 140db4b0f5ed · - · - · origem=Ronald (tmux)

Resultado da leitura, sem executar testes nem escrever arquivos. A cópia não contém `.git`; a correspondência do snapshot com o hash informado é `[nao verificado]`. [nao verificado]

**1(a,b) — registro e fila**

| Escritor/leitor | Local | Efeito |
|---|---|---|
| Escritor do sentinela | `app/chamados/models.py:2318` | `_registrar_travados` chama **o método da conexão**, com `_TravaDoColaborador(cid)`; não usa `transaction.on_commit`. |
| Chamador único de `_registrar_travados` | `app/chamados/models.py:2623` | Depois do SELECT de C em `app/chamados/models.py:2622`. |
| Conteúdo do sentinela | `app/chamados/models.py:2290` | Armazena `cid`; executar o callback retorna `None`, sem escrita nem remoção própria (`app/chamados/models.py:2293`). |
| Leitor do registro | `app/chamados/models.py:2305` | Varre `conn.run_on_commit`, seleciona instâncias do sentinela; fora de atomic retorna vazio (`app/chamados/models.py:2303`). |
| Leitor para deduplicar registro | `app/chamados/models.py:2313` | Evita registrar C já presente. |
| Selo | `app/chamados/models.py:2331` | Subtrai os registrados dos C pedidos e levanta `ColaboradorNaoTravado` (`app/chamados/models.py:2333`). |
| Porta lote | `app/chamados/models.py:3104` | Exige donos pela FK da disputa. |
| Porta resposta | `app/chamados/services/disputa_emissao.py:1393` | Exige dono antes de salvar P (`:1394`). |
| Porta correção | `app/chamados/services/disputa_emissao.py:1510` | Exige conjunto de donos. |
| Fachada segunda instância | `app/chamados/reconciliador.py:593` | Exige dono antes de `carimbar` (`:594`). |
| Contador chokepoint | `app/ponto/tests/test_selo_chokepoint_escrita.py:215` | Lê a mesma fila, excluindo sentinelas. |
| Mutante | `app/chamados/tests/test_e1_v13_ordem.py:33` | Troca `_registrar_travados` por no-op; não escreve segundo estado. |

Não encontrei outro escritor de `_TravaDoColaborador`/registro na árvore; a busca negativa, sem uma linha que a represente, é `[nao verificado]`. Há outros escritores da **fila**, listados abaixo; eles registram trabalho, não o sentinela.

| Escritores da fila em produção | Callback |
|---|---|
| `app/chamados/signals.py:144` | `_retomar` |
| `app/chamados/signals.py:225` | `_fechar` |
| `app/chamados/signals.py:266`, `app/chamados/signals.py:293` | `_silenciar` |
| `app/chamados/signals.py:322` | `_reavaliar` |
| `app/chamados/signals.py:356` | `_reconciliar` |
| `app/ponto/signals.py:272` | `_depois` |
| `app/ponto/signals.py:307` | `_reavaliar` |
| `app/ponto/portas/celula.py:502` | `_reavaliar_o_dia` |
| `app/ponto/services/fechamento.py:1817` | `_rodar` |

O teste do contador registra adicionalmente um `lambda: None` e exige incremento de um (`app/ponto/tests/test_selo_chokepoint_escrita.py:287`, `:288`).

**Manipulações de fila/testes** [nao verificado]

- Os patches de `transaction.on_commit` não interceptam `conn.on_commit`: isso decorre da chamada em `app/chamados/models.py:2318`; o novo teste explicita e verifica essa pretensão (`app/chamados/tests/test_e1_v13_ordem.py:124`, `:126`).
- Outros patches “roda agora”: `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:185`; `app/ponto/tests/test_ordem_regen_julga_antes_do_dinheiro.py:84`; `app/ponto/tests/test_o10b_editar_dia_troca.py:115`, `:121`, `:192`, `:367`, `:370`.
- A pretensão de queda no rollback de savepoint e sobrevivência no release está coberta pelo teste de atomic interno (`app/chamados/tests/test_e1_v13_ordem.py:107`, `:113`, `:114`, `:116`).
- Uso de savepoint manual em produção: a trava de C acontece **antes** da criação desse savepoint (`app/chamados/services/limbo_folgas.py:274`, `:277`); o rollback ocorre em `:285`. Portanto esse caso não testa registro criado **dentro** de savepoint manual.
- Outros rollbacks manuais explícitos: `app/chamados/tests/test_e1_v9.py:62`; `app/chamados/tests/test_matriz_guarda_clt.py:100`; `app/ponto/tests/test_d3_impressao_cobre_insumos.py:132`, `:294`.
- `run_and_clear_commit_hooks` não aparece na árvore pesquisada `[nao verificado]`. Sua implementação, assim como a implementação de `captureOnCommitCallbacks`, commit e rollback do Django, não está nesta cópia `[nao verificado]`; a versão exigida é Django 5.2.14 (`app/requirements.txt:13`). Não substituir essa lacuna pela docstring que afirma equivalência de vida com a trava (`app/chamados/models.py:2284`).
- Particularmente, **savepoint manual** merece ressalva: o novo teste só usa `atomic()`, não `transaction.savepoint()` (`app/chamados/tests/test_e1_v13_ordem.py:107`). A equivalência geral alegada para ambos fica `[nao verificado]`.

**Inventário de `captureOnCommitCallbacks`** — cada sítio abaixo captura callbacks; quando `execute=True`, solicita executá-los, incluindo o no-op se capturado. A retenção/remoção efetiva da fila pela implementação Django permanece `[nao verificado]`.

| Arquivo | Linhas |
|---|---|
| `app/api/tests/test_atestado_na_resposta.py` | 51 |
| `app/chamados/tests/test_jv1_volta4_posto_do_dia.py` | 46, 119, 135 |
| `app/chamados/tests/test_jv3_reconciliar_vira_vigia.py` | 71, 118, 144 |
| `app/chamados/tests/test_varredura_becos_evento_pergunta.py` | 55, 65, 76, 86, 88 |
| `app/chamados/tests/test_e2_celula_regenerada_reavalia.py` | 70, 146, 202, 204; **execute=False** em 226 |
| `app/chamados/tests/test_jv1_volta3_municipio_e_quem_chegou.py` | 33, 39, 66 |
| `app/chamados/tests/test_jv1_feriado_vira_vigia.py` | 87, 163 |
| `app/chamados/tests/test_laco_ausencia_aberta_nao_vai_e_volta.py` | 66 |
| `app/chamados/tests/test_s4_pendente_suspende_cobranca.py` | 86 |
| `app/chamados/tests/test_pep_reentrada.py` | 101 |
| `app/chamados/tests/test_c20_juiz3_outbox.py` | 242 |
| `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py` | 57, 68, 110, 134, 176 |
| `app/chamados/tests/test_reg_externa_disputa_beco.py` | 156 |
| `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py` | 87, 131, 182, 188, 242, 260, 265; **execute=False** em 328 |
| `app/chamados/tests/test_varredura_isentos_evento.py` | 38, 52, 65, 71, 82 |
| `app/chamados/tests/test_varredura_feriado_evento.py` | 58, 83 |
| `app/ponto/tests/test_reconciliar_geofence.py` | 102 |
| `app/ponto/tests/test_evento_justificativa_rejulga_o_dia.py` | 31, 42 |
| `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py` | 115, 122, 136, 155, 174 |
| `app/ponto/tests/test_varredura_classe_b_ausencia.py` | 103, 114, 124 |
| `app/ponto/tests/test_d3_impressao_cobre_insumos.py` | 274 |

Há contagem **filtrada pelo nome** do callback de E2 (`app/chamados/tests/test_e2_celula_regenerada_reavalia.py:149`, `:228`, `:229`), preservada frente a sentinelas sem esse nome. Já `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:331` conta **todos** os callbacks e chama o primeiro (`:333`); ele não exclui sentinelas. Se uma trava nova ocorrer nesse ato capturado, pode alterar a premissa desse teste `[nao verificado]`.

**Achados úteis para item 3**

1. **NAO PROCEDE a afirmação de selo geral das portas D/CH/P.** As quatro verificações estão nas fachadas acima, não nas portas gerais. Os próprios testes criam Disputa e Pergunta com o mutante ativo, sem recusa (`app/chamados/tests/test_e1_v13_ordem.py:44`, `:47`, `:48`, `:53`). Isso prova que retirar o registro não impede genericamente escrita D/P.
2. **Registro é atestado dos IDs pedidos**, não dos IDs efetivamente retornados pelo banco: descarta o resultado do SELECT e registra `_cids` (`app/chamados/models.py:2622`, `:2623`). Um ID inexistente seria marcado como travado sem linha adquirida; aplicabilidade a dono de FK válido `[nao verificado]`.
3. O mutante da segunda instância verifica retorno zero e ausência do carimbo (`app/chamados/tests/test_e1_v13_ordem.py:156`, `:157`, `:159`); a exceção é engolida pela fachada (`app/chamados/reconciliador.py:600`).
4. O mutante do lote não verifica estado posterior e não mantém `_mutante` no ato; verifica somente falta inicial de registro e exceção (`app/chamados/tests/test_e1_v13_ordem.py:86`, `:88`). Não prova aquisição adequada pelo seu chamador.
5. O teste multicolaborador exige ORDER BY e trava anterior à primeira escrita (`app/chamados/tests/test_e1_v13_ordem.py:68`, `:71`, `:72`, `:73`).

**5 — tetos**

- **PROCEDE quanto à correspondência estrutural:** os dois números novos, 211 e 265, justificam +3 por um carimbo (`app/ponto/tests/test_selo_chokepoint_escrita.py:161`, `:162`). O código acrescenta atomic e SELECT de C antes de carimbar (`app/chamados/reconciliador.py:591`, `:592`, `:594`); o SELECT está em `app/chamados/models.py:2622`.
- **NAO VERIFICADO quanto aos valores medidos e a “um carimbo” nesses cenários:** os logs citados nos comentários não foram fornecidos como resultados observáveis. Os testes exigem que 1 e 3 chamados fechem antes de cobrar seus tetos (`app/ponto/tests/test_selo_chokepoint_escrita.py:389`, `:390`, `:394`, `:395`), mas não contam especificamente quantos carimbos ocorreram.
- **NAO PROCEDE que a filtragem esconda callback funcional atual:** só exclui objetos `_TravaDoColaborador` (`app/ponto/tests/test_selo_chokepoint_escrita.py:215`), cujo `__call__` retorna `None` (`app/chamados/models.py:2293`). Todos os demais callbacks continuam contados; o teste mantém mordida sobre lambda comum (`app/ponto/tests/test_selo_chokepoint_escrita.py:285`, `:287`, `:288`).
- A filtragem deixa de medir alocação, varredura e execução dos próprios sentinelas; esse custo existe no código, embora não seja trabalho de negócio (`app/chamados/models.py:2318`, `:2305`, `:2293`). Não esconderia suas queries porque as queries de aquisição permanecem no CaptureQueriesContext (`app/ponto/tests/test_selo_chokepoint_escrita.py:380`, `:381`).

_2835 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
