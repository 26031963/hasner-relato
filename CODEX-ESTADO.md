# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 07:30** · publicado em **30/10/2026 07:30** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 07:30 · sessao e2aa7d0d8d1d · - · - · origem=Ronald (tmux)

Chamador direto de produção encontrado: somente `julgar_colab` → `perguntar_pela_celula`, em `app/ponto/services/cartorio.py:758` e `app/ponto/services/cartorio.py:770`. Signal e vigia chegam indiretamente pelo cartório: `app/ponto/signals.py:85`, `app/ponto/signals.py:93`, `app/ponto/services/vigia_de_hora.py:130`.

Existem caminhos externos que impedem concluir “NB nunca recebe pergunta”:

- **Lote automático:** `disparar_perguntas_competencia`, encadeado com `--comp-corrente --apply`, chama diretamente `fabrica_da_celula.perguntar`; não chama `perguntar_pela_celula`. Fontes: `app/config/crons.py:1456`, `app/chamados/management/commands/disparar_perguntas_competencia.py:277`, `app/chamados/management/commands/disparar_perguntas_competencia.py:288`.
- **NB passa pelo filtro do lote:** exige célula trabalhando, colaborador ativo, motivos pela ata/teto temporal, veredito vazio ou integrante de `VEREDITOS_ACUSAM` e fato ainda sem pergunta não duplicata. `nunca_bateu` integra explicitamente essa tupla. Fontes: `app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `app/chamados/management/commands/disparar_perguntas_competencia.py:105`, `app/chamados/management/commands/disparar_perguntas_competencia.py:111`, `app/chamados/management/commands/disparar_perguntas_competencia.py:120`, `app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `app/escala/models.py:1735`.
- **O segundo universo do lote é fio mudo:** chamado vivo ligado à célula, sem pergunta viva, motivo pela ata, colaborador ativo, sem pergunta existente do mesmo fato; não contém exclusão explícita de NB. Fontes: `app/chamados/management/commands/disparar_perguntas_competencia.py:155`, `app/chamados/management/commands/disparar_perguntas_competencia.py:163`, `app/chamados/management/commands/disparar_perguntas_competencia.py:167`, `app/chamados/management/commands/disparar_perguntas_competencia.py:178`, `app/chamados/management/commands/disparar_perguntas_competencia.py:220`.
- **Chamado de adesão:** `apurar_furos_diarios` → `sincronizar_adesao`; universo de células NB na janela atual, colaborador ativo; exclui batida não retratada, isenção e ausência cobrindo hoje de tipo diferente de falta; abre pai por colaborador/período com módulo `sem_previsao_escala`, subcategoria `nunca_bateu`. Fontes: `app/config/crons.py:1444`, `app/escala/management/commands/apurar_furos_diarios.py:17`, `app/chamados/services/adesao.py:32`, `app/chamados/services/adesao.py:64`, `app/chamados/services/adesao.py:68`, `app/chamados/services/adesao.py:74`.
- **Reconciliador c2 veta chamado diário NB:** consulta `celula_veta_cobranca` e retorna; NB está na tupla de vetos. Fontes: `app/chamados/reconciliador.py:838`, `app/chamados/reconciliador.py:842`, `app/ponto/marcos.py:119`, `app/ponto/marcos.py:161`.
- **Detector de ausências também veta NB:** antes da emissão/fio, em `app/ponto/management/commands/detectar_ausencias.py:625`.
- **Crons de turno não resgatam NB real sem batidas:** `processar_alertas_turno` exige turno aberto e entrada; `processar_alertas_avancados` exige turno aberto cuja entrada extrai. Podem potencialmente tratar NB desatualizado que já possui entrada; ocorrência efetiva não foi verificada. Fontes: `app/ponto/management/commands/processar_alertas_turno.py:88`, `app/ponto/management/commands/processar_alertas_turno.py:92`, `app/ponto/management/commands/processar_alertas_turno.py:384`, `app/ponto/management/commands/processar_alertas_avancados.py:63`, `app/ponto/management/commands/processar_alertas_avancados.py:66`, `app/ponto/management/commands/processar_alertas_avancados.py:158`.
- **Passivo/manual com chamado existente:** `perguntar_chamados_sem_pergunta --apply` e `cobrar_dia` passam a causa `fabrica_nao_rodou` ao agrupador de perguntas. O caminho pela ata não exclui NB explicitamente; as guardas do escritor são ausência e deduplicação. Fontes: `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:20`, `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:26`, `app/chamados/services/pergunta_no_app.py:153`, `app/chamados/services/pergunta_no_app.py:159`, `app/chamados/services/cobrar_dia.py:40`, `app/chamados/services/cobrar_dia.py:44`, `app/chamados/services/disputa_emissao.py:693`, `app/chamados/services/disputa_emissao.py:969`, `app/chamados/services/disputa_emissao.py:978`, `app/chamados/services/disputa_emissao.py:763`.

## 30/10 07:29 · sessao 347849f0e4f1 · - · - · origem=Ronald (tmux)

- **Cluster espúrio:** usa `out['cods']` indiretamente por `_julgadas = out.get('cods') or {}` e testa apenas se o mapa é não vazio; então chama `_cluster(c, hoje)`, sem passar suas datas ou códigos. Adicionar `{data: []}` habilita essa chamada se antes o mapa estava vazio. A reconciliação continua exigindo colaborador ativo e escala ativa sem fim; a assinatura continua exigindo célula de trabalho, pelo menos duas órfãs, lâmpada apagada e rajada de até 15 minutos. (`app/ponto/services/cartorio.py:759`, `app/ponto/services/cartorio.py:763`, `app/ponto/services/cartorio.py:767`, `app/ponto/services/cluster_espurio.py:22`, `app/ponto/services/cluster_espurio.py:24`, `app/ponto/services/cluster_espurio.py:28`, `app/ponto/services/cluster_espurio.py:30`, `app/ponto/services/cluster_espurio.py:73`, `app/ponto/services/cluster_espurio.py:75`.)

- **O222 / releitura depois da emissão:** o gatilho não consulta `cods`: compara `out.get('emitidos', 0) > _e0`, sendo `_e0` capturado antes do corpo. Quando dispara, cria `_conv`, executa novamente o corpo com toda a lista `cels`, mescla `_conv.get('cods')` em `out['cods']`, soma `_conv['julgadas']` a `relidas_pos_emissao` e corrige `protestos` pelo delta dos objetos. Portanto, adicionar uma data a `cods` não dispara diretamente a releitura. (`app/ponto/services/cartorio.py:743`, `app/ponto/services/cartorio.py:798`, `app/ponto/services/cartorio.py:809`, `app/ponto/services/cartorio.py:810`, `app/ponto/services/cartorio.py:811`, `app/ponto/services/cartorio.py:812`, `app/ponto/services/cartorio.py:813`, `app/ponto/services/cartorio.py:814`.)

- **Contadores:** não derivam seus valores de `len(out['cods'])`. O ramo principal incrementa `julgadas` explicitamente e depois escreve o mapa; `protestos` depende da variável local `cods`; o ramo NB incrementa `nunca_bateu`, além de `carimbadas` quando aplica, ou `pulados` na guarda de idempotência. Somente inserir `{data: []}` não altera esses incrementos. `perguntas_no_julgamento` pode crescer indiretamente se a fábrica retornar motivos, pela quantidade retornada. (`app/ponto/services/cartorio.py:1119`, `app/ponto/services/cartorio.py:1120`, `app/ponto/services/cartorio.py:1292`, `app/ponto/services/cartorio.py:1293`, `app/ponto/services/cartorio.py:943`, `app/ponto/services/cartorio.py:944`, `app/ponto/services/cartorio.py:946`, `app/ponto/services/cartorio.py:1002`, `app/ponto/services/cartorio.py:771`, `app/ponto/services/cartorio.py:772`.)

- **Passivo `furos_sem_cobranca`:** não lê esse mapa; seleciona células persistidas por `veredito='furo'` e pelos protestos cobrados. Inserir somente uma data em `out['cods']`, preservando o veredito NB e `codigos=[]`, não inclui a célula nesse universo. (`app/ponto/services/furos_sem_cobranca.py:35`, `app/ponto/services/furos_sem_cobranca.py:55`, `app/ponto/services/furos_sem_cobranca.py:61`, `app/ponto/services/cartorio.py:980`, `app/ponto/services/cartorio.py:981`.)

- **Ressalva do lote:** `out` é criado uma única vez antes do laço de colaboradores, e cada chamada recebe o mesmo objeto. Como a fábrica testa somente a presença da **data**, uma entrada herdada de colaborador anterior já pode habilitar fábrica e cluster para um colaborador NB, mesmo sem escrita de `cods` pelo ramo NB. (`app/ponto/management/commands/processar_cartorio.py:43`, `app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:759`, `app/ponto/services/cartorio.py:763`, `app/ponto/services/cartorio.py:769`.)

- A referência em `celula_veredito_velho` a `derivar_veredito(out['cods'][data])` descreve a implementação antiga; o código atual intercepta a lavra e chama `julgar_colab(..., out=contadores(), forcar=True)`. (`app/ponto/management/commands/celula_veredito_velho.py:12`, `app/ponto/management/commands/celula_veredito_velho.py:19`, `app/ponto/management/commands/celula_veredito_velho.py:130`, `app/ponto/management/commands/celula_veredito_velho.py:144`.)

## 30/10 05:47 · sessao fa471d9a91b1 · O261-FIO6 · sonda · origem=Code

**As seis são ESTRUTURA quanto à contradição do fio:** essa classificação é explícita no próprio vigia e corresponde à L-099. Isso não determina o dono de eventual divergência entre DNA e batidas. (`app/chamados/services/vigia_fio.py:9`; `CLAUDE.md:306`)

**A sonda permite identificar a morte por validação em quatro células, mas não permite atribuir um escritor causal específico às seis.** Faltam ata, DNA, contexto dos chamados e trilha dos carimbos. [nao verificado]

Os IDs e estados de produção abaixo vêm exclusivamente do anexo, sem arquivo local para citação. [nao verificado]

| Célula / dia / chamado | Por que não há fio vivo; qual pergunta deveria viver | Dono L-099 | Chamado deveria continuar vivo? |
|---|---|---|---|
| **143031 / col1005 / 29-10 / #35679** [nao verificado] | Nenhuma pergunta apresentada. Motivo que deveria ser emitido e bloqueio efetivamente percorrido: **[nao verificado]**. Há uma lacuna demonstrável no caminho `nunca_bateu`: o corpo lavra a célula e retorna sem registrar a data em `out['cods']`; a fábrica é chamada somente para datas presentes nesse mapa. (`app/ponto/services/cartorio.py:981`, `:1003`, `:759`, `:768`; publicação no ramo normal em `:1120`) | **ESTRUTURA do fio**. Atribuição deste caso à lacuna acima: **[nao verificado]**. | O reconciliador preserva `batida_ausente` pela regra **R** abaixo. Porém, uma nova emissão com `nunca_bateu` seria vetada: esse veredito integra `VEREDITOS_QUE_VETAM`, consumido pelo emissor. (`app/ponto/marcos.py:119`, `:161`; `app/ponto/management/commands/detectar_ausencias.py:625`) Motivo concreto de o estoque não ter sido encerrado/agrupado: **[nao verificado]**. |
| **129745 / col451 / 21-10 / #33286** [nao verificado] | **P46703 `saida_sem_entrada` e P46704 `orfao_14h` morrem por `validada_em` preenchido**, antes de consultar a célula. (`app/chamados/juizes.py:1005`) Pela regra atual, nenhuma delas deve continuar viva apenas porque o dia acusa. O emissor também retira motivos que já possuem pergunta. (`app/chamados/services/fabrica_da_celula.py:101`, `:109`) Ambas estão em `com_registro` no anexo. [nao verificado] | **ESTRUTURA do fio**, classe **V**, compartilhada por quatro células. Não há prova de erro no DNA ou falta/sobra de batida. [nao verificado] | **R** conserva o chamado. Premissa material regularizada e motivo concreto de não fechamento pelo lastro: **[nao verificado]**, conforme **L** abaixo. |
| **129747 / col451 / 23-10 / #34254** [nao verificado] | **P49422 `saida_sem_entrada` e P49423 `orfao_14h` morrem pela mesma validação prioritária.** (`app/chamados/juizes.py:1005`) **P49422 apresenta adicionalmente `sem_plantio_nenhum`**, enquanto P49423 apresenta `com_registro`. [nao verificado] A primeira classe significa que o juiz de materialização não confirmou lastro, o slot é derivável e não há plantio vivo vinculado. (`app/chamados/services/vigia_carimbo.py:106`, `:110`, `:113`) | **ESTRUTURA do fio**, classe **V**; também classe **P**, presente em uma célula. Escritor que produziu o carimbo sem lastro: **[nao verificado]**. | **R** conserva o chamado. `sem_plantio_nenhum` de P49422 não demonstra, sozinho, quais marcos #34254 cobra nem o resultado do fechador: **[nao verificado]**. |
| **129749 / col451 / 25-10 / #34850** [nao verificado] | **P50179 `saida_sem_entrada` e P50180 `orfao_14h` morrem por validação**, no primeiro teste do juiz. (`app/chamados/juizes.py:1005`) Ambas apresentam `com_registro`. [nao verificado] A existência delas impede nova emissão dos mesmos motivos pela fábrica. (`app/chamados/services/fabrica_da_celula.py:101`, `:109`) | **ESTRUTURA do fio**, classe **V**. Dono de eventual divergência DNA/batidas: **[nao verificado]**. | **R** conserva o chamado; premissa material e motivo concreto de não fechamento por **L**: **[nao verificado]**. |
| **129751 / col451 / 27-10 / #35331** [nao verificado] | **P50883 `saida_sem_entrada` e P51025 `orfao_14h` morrem por validação**, antes do veredito acusador. (`app/chamados/juizes.py:1005`) Ambas apresentam `com_registro`. [nao verificado] A fábrica exclui os motivos já existentes. (`app/chamados/services/fabrica_da_celula.py:101`, `:109`) | **ESTRUTURA do fio**, classe **V**. Dono de eventual divergência DNA/batidas: **[nao verificado]**. | **R** conserva o chamado; premissa material e motivo concreto de não fechamento por **L**: **[nao verificado]**. |
| **140114 / col129 / 25-10 / #35101** [nao verificado] | **P50169 `saida_sem_entrada` morre por validação.** (`app/chamados/juizes.py:1005`) Para **P50170 `orfao_14h`**, sem validação nem resposta no anexo, a cláusula exata que devolveu `False` é **[nao verificado]**: faltam a leitura da tranca e a lâmpada do marco, consultadas antes do veredito acusador. (`app/chamados/juizes.py:1010`, `:1050`, `:1054`) `via=fantasma` não integra as vias de identidade que encerram incondicionalmente. (`app/chamados/juizes.py:1039`) | **ESTRUTURA do fio**, classe **M**, uma célula. Não há prova de que P50170 deveria estar viva. [nao verificado] | O chamado vivo **#35101** é conservado por **R**. O fechamento de #35056 e o destino #3227 de P50169 não demonstram a regularização dos marcos de #35101. [nao verificado] Resultado concreto de **L**: **[nao verificado]**. |

As classes demonstráveis e seus escritores são:

- **V — perguntas encerradas por validação: quatro das seis células integralmente nessa classe**, as quatro de col451; P50169 também morre pelo mesmo predicado. [nao verificado] O C6 encerra imediatamente por `validada_em`; o emissor chama o fato existente de **“fio decidido”**, sem pedir nova pergunta. (`app/chamados/juizes.py:1005`; `app/chamados/management/commands/disparar_perguntas_competencia.py:210`) **Não há um escritor único demonstrado para esses registros**: existem atribuições positivas em validação, materialização e resolução por chamado. (`app/chamados/services/validacao.py:127`; `app/chamados/services/materializacao.py:514`, `:1194`; `app/chamados/services/disputa_emissao.py:1574`) Qual delas escreveu cada pergunta: **[nao verificado]**.

- **P — validada sem lastro confirmado e sem plantio vivo vinculado: uma das seis**, P49422 da célula 129747. [nao verificado] A classificação é produzida pelo leitor `vigia_carimbo.classificar`, não identifica o escritor histórico. (`app/chamados/services/vigia_carimbo.py:97`, `:106`, `:116`) A devolução da validação recusada exige `materializada_em` nulo; uma pergunta já carimbada fica fora desse mecanismo. (`app/chamados/services/validacao.py:180`) Isso explica a exclusão pelo código atual, mas não prova a sequência histórica desse caso. [nao verificado]

- **N — ausência de pergunta: uma das seis**, célula 143031. [nao verificado] O caminho `nunca_bateu` escreve veredito/ata e retorna sem alimentar o mapa exigido para chamar `perguntar_pela_celula`; esse mapa é alimentado no ramo normal. (`app/ponto/services/cartorio.py:950`, `:981`, `:1003`, `:1120`, `:768`) **A lacuna é comprovada no código; a execução responsável pela ausência nesta célula é [nao verificado]**. O motivo devido depende dos marcos faltantes e vencidos, ausentes do anexo. (`app/chamados/services/fabrica_da_celula.py:49`, `:59`)

- **M — uma pergunta validada e outra fantasma sem validação: uma das seis**, célula 140114. [nao verificado] A porta que grava `fantasma` é `PerguntaDisputa.encerrar_fantasma`; o consumidor atual exige a lâmpada do próprio marco acesa antes de chamá-la. (`app/chamados/models.py:2162`; `app/chamados/reconciliador.py:1032`, `:1092`) **Não se pode atribuir o carimbo de P50170 a esse consumidor atual** sem sua trilha histórica. [nao verificado]

**R — por que o reconciliador conserva os seis `batida_ausente`.** O catálogo associa esse módulo a `FATO_FALTA_DESCOBERTA`, mas `_veredito_premissa` não coleta esse fato; o catálogo devolve `viva=True` quando o fato não foi coletado, e o laço pula a retratação. Portanto, aqui “premissa viva” é **preservação conservadora**, não comprovação de furo material. (`app/chamados/catalogo/premissa.py:58`, `:128`; `app/chamados/reconciliador.py:262`, `:301`, `:1360`) A retratação pela célula também não age enquanto seu veredito acusa. (`app/chamados/reconciliador.py:975`)

**L — quem encerraria por lastro e por que não é possível provar a recusa individual.** `fechar_cobranca_com_lastro` delega a `lastro.fechar`; este só tenta reconciliar os chamados classificados em `com_lastro`. (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:52`; `app/ponto/services/lastro.py:306`) A classificação depende dos **marcos cobrados no contexto do chamado**, de suas lâmpadas e, no recuo, das batidas; depois ainda passa pelo gate da premissa. (`app/ponto/services/lastro.py:250`, `:264`, `:291`, `:317`; `app/chamados/models.py:470`) Esses insumos e a execução do fechador não constam da sonda: dizer que cada chamado deveria fechar, ou nomear por que concretamente não fechou, é **[nao verificado]**.

## 30/10 05:35 · sessao fb2eb8b68907 · ORCAMENTO · verificador · origem=Code

**Ainda reprovo: o novo fallback permite que o TETO engula a expiração do CLEAR e sua válvula continua condicional.** `bin/hook_stop_fila1.py:763`, `bin/hook_stop_fila1.py:893`, `bin/hook_stop_fila1.py:1102`.

1. **[P1 — novo] CLEAR vencido volta a liberar pelo TETO.** Cenário: disparo ativo, `clear_parei_desde` há 31 minutos, marco inalterado, HEAD igual e `blocks=5`; a trava fica ocupada por mais de um segundo ou ocorre uma exceção no orçamento. Ambos os caminhos retornam `None`; o CLEAR é recusado pelo PAREI genérico e o contador chega a 6, liberando por `ALARME teto`. Não saem o `siga` explícito da expiração, o reset nem o `SEGUIU`: quebra v2(b/c). Antes dos 30 minutos, o mesmo fallback pode bloquear uma espera que deveria liberar pelo item 2. `bin/hook_stop_fila1.py:761`, `bin/hook_stop_fila1.py:888`, `bin/hook_stop_fila1.py:953`, `bin/hook_stop_fila1.py:1095`, `bin/hook_stop_fila1.py:1102`.

   **O selo novo aprova essa substituição:** exige `ALLOW ALARME teto`; o caso ocupado nem estabelece `clear_parei_desde`, e o caso de exceção apaga o estado ativo. Portanto, esses casos não provam a precedência de um CLEAR vencido sob falha — classe **selo/RED sem mordida** nesse requisito. `bin/tests/test_orcamento_contexto.sh:395`, `bin/tests/test_orcamento_contexto.sh:404`, `bin/tests/test_orcamento_contexto.sh:414`, `bin/tests/test_orcamento_contexto.sh:420`.

2. **[P1 — novo, dependendo do caminho antigo] Ainda existe laço sem válvula.** Cenário: CLEAR no topo, falha ou trava persistente do orçamento, HEAD estável e `ps` mostrando continuamente `manage.py test` ou `git push`, inclusive de outra sessão. O fallback chega ao caminho antigo, mas esse caminho congela `blocks` e proíbe aplicar o TETO enquanto detectar trabalho. Cada Stop pode devolver `siga: ITEM-VIVO` indefinidamente: a afirmação nova de que “o bloqueio CONTA” não vale nesse estado. `bin/hook_stop_fila1.py:389`, `bin/hook_stop_fila1.py:733`, `bin/hook_stop_fila1.py:889`, `bin/hook_stop_fila1.py:1090`, `bin/hook_stop_fila1.py:1102`.

   Os novos casos de falha persistente usam o `ps` forjado sem trabalho e não exercitam essa combinação; o caso separado de suíte só verifica `PENDENTE` com orçamento funcionando. `bin/tests/test_orcamento_contexto.sh:35`, `bin/tests/test_orcamento_contexto.sh:342`, `bin/tests/test_orcamento_contexto.sh:398`, `bin/tests/test_orcamento_contexto.sh:416`.

3. **[P1 — dependência do acumulado] I/O bloqueante impede alcançar qualquer fallback.** Cenário: `orcamento_estado.json` é um FIFO sem escritor. A abertura para leitura fica esperando enquanto o hook segura a trava do orçamento; não ocorre exceção para `_orcamento_contido` capturar e o TETO não é alcançado. Um FIFO sem leitor em `orcamento.log` também pode prender o próprio registro de `OCUPADO`/`FALHA`. A proteção de arquivo regular foi aplicada à trava, mas não a essas aberturas: quebra a contenção exigida por v2(e)/item 3. `bin/hook_stop_fila1.py:526`, `bin/hook_stop_fila1.py:753`, `bin/hook_stop_fila1.py:777`, `bin/hook_stop_fila1.py:691`, `bin/hook_stop_fila1.py:892`.

4. **[P2 — dependência do acumulado] A hora do marco pode regredir por ordem de trava.** Cenário: A captura hora `t0` para `compact` e é suspenso; B captura `t1 > t0`, obtém a trava e grava `clear`; A retoma, obtém a trava e sobrescreve o marco com `t0`. O histórico conserva o clear de `t1`, mas a base corrente volta para `t0`, antecipando o gatilho de seis horas. É um problema de **relógio antes da seção protegida / ordem de trava**, contra a medida desde o último marco do item 1(b). `bin/marco_de_clear.py:33`, `bin/marco_de_clear.py:43`, `bin/marco_de_clear.py:53`, `bin/marco_de_clear.py:57`, `bin/orcamento_contexto.py:184`, `bin/orcamento_contexto.py:196`.

5. **[P2 — dependência do acumulado] Estado gravado com trilha falha perde o evento definitivamente.** Cenário: o estado é gravável, mas `orcamento.log` é um diretório ou recusa append. `_orc_salva` confirma o estado, `_orc_trilha` engole a falha e a lista de eventos é apagada. Se o evento era `SEGUIU`, o desfecho fica concluído e não será reapendado; o ESTADO subconta porque lê os contadores exclusivamente da trilha. Quebra o item 4, “uma linha por disparo e por desfecho”. O selo cobre a falha inversa, estado não gravável. `bin/hook_stop_fila1.py:715`, `bin/hook_stop_fila1.py:723`, `bin/hook_stop_fila1.py:694`, `bin/hook_stop_fila1.py:725`, `bin/hook_stop_fila1.py:837`, `bin/orcamento_contexto.py:219`, `bin/tests/test_orcamento_contexto.sh:406`.

Nas demais áreas lidas, sem achado adicional:

- **Prova do RELATO, PAREI comum e ESPERANDO:** a prova continua antes do orçamento; o PAREI reutiliza seu juiz; a espera compartilha uma única avaliação por Stop, preservando a prova por avanço de arquivo. `bin/hook_stop_fila1.py:1027`, `bin/hook_stop_fila1.py:916`, `bin/hook_stop_fila1.py:660`, `bin/hook_stop_fila1.py:1049`.
- **Fila vazia:** há um PREPARA-CLEAR por ciclo; mantendo o topo neutro, o Stop seguinte alcança a liberação por fila vazia. `bin/hook_stop_fila1.py:848`, `bin/hook_stop_fila1.py:876`, `bin/hook_stop_fila1.py:1063`.
- **Segundo juiz, segundo escritor e derivação paralela:** no recorte lido, a decisão usa o avaliador importado, o marco tem seu escritor próprio e o gerador do ESTADO chama os leitores compartilhados. `bin/hook_stop_fila1.py:772`, `bin/hook_stop_fila1.py:820`, `bin/marco_de_clear.py:57`, `bin/gerar_estado.py:274`.
- **Falha aberta externa:** o `except` final continua liberando; ele não resolve as esperas bloqueantes do achado 3. `bin/hook_stop_fila1.py:1117`.

Execução integral dos selos, instalação dos matchers no host e ocorrência desses cenários em produção: **[nao verificado]**.

## 30/10 04:11 · sessao 438094092922 · O261-C · verificador · origem=Code

**Achei uma brecha estrutural em (b): a dispensa do veto não acompanha a decisão de renascer.** `_costura_no_vivo` é calculado antes da trava e faz o novo veto ser pulado; depois, `abrir` trava, relê o chamado e pode encontrar um encerrado, renascendo-o sem consultar a régua. (app/chamados/reconciliador.py:798–799,843–848; app/chamados/models.py:666–687,764–770,989–1021)

O encerramento intermediário pode passar pelas portas atuais: um chamado cujo marco cobrado acendeu pode fechar, enquanto o renascimento recebe outro marco apagado do mesmo dia. Portanto, as guardas de premissa não eliminam essa brecha. (app/chamados/models.py:495–496,959–960,687; app/chamados/reconciliador.py:673–677; app/ponto/services/lastro.py:174–180) Reprodução concorrente em banco: **[não verificado]**.

- **(a) Segundo juiz/vínculo/dia: nada novo.** A impressão usa exatamente a mesma composição de leitores; o emissor consulta os mesmos predicados para vínculo presente. Existe assimetria preexistente com `ec=None`, mas os caminhos normais do emissor pulam sem vínculo. (app/ponto/services/cartorio.py:1007–1014,1347–1348,1446–1448; app/ponto/services/furos_vetados.py:25–31; app/ponto/management/commands/detectar_ausencias.py:599–605)

- **(b) Encerrado já na primeira leitura: nada.** Não satisfaz `_costura_no_vivo`; recebe o veto antes de chegar a `abrir`. A brecha acima exige mudança entre as leituras. (app/chamados/reconciliador.py:798–799,843–853)

- **(c) C1: muda, sim.** O veto alcança `orfao_14h` e `saida_sem_entrada`, inclusive fora da cerca do alcance novo. Isso bloqueia cobranças C1 antes possíveis; coincide com a regra geral contra cobrança por régua suspeita e com o alcance declarado dos quatro motivos. Nada adicional identificado como regressão funcional. (app/chamados/reconciliador.py:332,770,840–851; app/escala/regua_defesa.py:5–8)

- **Selo sem mordida: nada identificado.** R2b exige veto real e ausência de emissão; R2 fornece o controle positivo. RED executado: **[não verificado]**. (app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:174–198)

- **Relógio fora da borda: nada. Escrita fora da porta: nada. Inversão de travas: nada novo.** O acréscimo consulta dia e estado gravado, sem escrita ou aquisição de trava; permanece a brecha de decisão anterior à trava descrita acima. (app/ponto/services/furos_vetados.py:19–32; app/escala/regua_defesa.py:554–562,641–644; app/escala/servico_jornada.py:38–58; app/chamados/models.py:666–670)

## 30/10 04:10 · sessao 5604ba6bee6c · - · - · origem=Ronald (tmux)

(b) Há janela de corrida: o c2 guarda `_costura_no_vivo` antes da trava e usa esse booleano para pular o veto; `abrir` só adquire a trava depois, relê o chamado e pode renascê-lo já encerrado. O renascimento não consulta o veto da régua. Referências: `app/chamados/reconciliador.py:798`, `app/chamados/reconciliador.py:843`, `app/chamados/models.py:666`, `app/chamados/models.py:685`, `app/chamados/models.py:764`, `app/chamados/models.py:989`.

O encerramento concorrente pode ser válido: um chamado com seus marcos antigos acesos fecha por `reconciliar`, enquanto o c2 pede outro marco apagado do mesmo dia. O fechamento usa os marcos próprios; o renascimento recebe os marcos novos de `_ctx_c2`. Assim, a mesma autoridade pode permitir fechar o antigo e renascer pelo novo, com o veto omitido pelo booleano anterior. Referências: `app/chamados/models.py:495`, `app/chamados/models.py:959`, `app/chamados/models.py:687`, `app/chamados/reconciliador.py:673`, `app/ponto/services/lastro.py:174`, `app/chamados/juizes.py:435`. Não reproduzi a concorrência em banco. [não verificado]

(b), execução sequencial: nada. Encerrado desde a primeira leitura não satisfaz `_costura_no_vivo` e passa pelo veto antes de `abrir`. Referências: `app/chamados/reconciliador.py:799`, `app/chamados/reconciliador.py:843`, `app/chamados/reconciliador.py:853`.

(c): o veto alcança C1, inclusive fora do alcance novo, pois a limitação pela cerca só vale para motivos fora de `MOTIVOS_C1`. Isso impede emissões C1 vetadas anteriormente possíveis, mas é coerente com a regra existente contra cobrança por régua suspeita e com o comentário novo que declara os quatro motivos; não identifiquei regressão funcional adicional. Referências: `app/chamados/reconciliador.py:332`, `app/chamados/reconciliador.py:770`, `app/chamados/reconciliador.py:840`, `app/chamados/reconciliador.py:851`, `app/escala/regua_defesa.py:5`.

## 30/10 04:09 · sessao b9b06cd6af78 · - · - · origem=Ronald (tmux)

- **Selo sem mordida: nada identificado.** R2b chama o leitor real, exige `VETO_REGUA`, verifica retorno `False` e ausência de chamado; R2 fornece controle positivo do mesmo cenário sem veto (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:174–198`). RED executado: **[não verificado]**.
- **Relógio fora da borda: nada no acréscimo.** O dia é recebido como `_d_hz`; os predicados consultados usam template e escore gravado, sem consultar hora atual (`app/chamados/reconciliador.py:845`; `app/ponto/services/furos_vetados.py:25–32`; `app/escala/regua_defesa.py:16–33`, `:554–562`, `:641–644`).
- **Escrita fora da porta: nada no acréscimo.** O carregador consulta célula/vínculo e os predicados devolvem valores; a emissão continua em `ChamadoColaborador.abrir` (`app/escala/servico_jornada.py:38–58`; `app/ponto/services/furos_vetados.py:19–32`; `app/chamados/reconciliador.py:853`).
- **Inversão da ordem de travas: nada introduzido pelo leitor.** Ele usa SELECT comum, sem adquirir trava; `abrir` mantém `atomic` e trava do colaborador (`app/escala/servico_jornada.py:46–58`; `app/chamados/models.py:666–670`).

**Atenção para consolidar com (b):** a dispensa por `_costura_no_vivo` é decidida antes da trava (`app/chamados/reconciliador.py:798–799`, `:843`). Dentro da trava, `abrir` reconsulta e pode renascer um encerrado (`app/chamados/models.py:669–687`). Uma mudança concorrente de vivo para encerrado entre essas leituras escaparia do novo veto; reprodução desse intercalamento: **[não verificado]**.

## 30/10 04:09 · sessao 612a84432b06 · - · - · origem=Ronald (tmux)

(a) **Nada no diff** quanto a segundo juiz, vínculo ou dia: c2 e impressão chamam a mesma composição `motivo_do_veto(c, vinculo_do_emissor(c, dia), dia)` (`app/chamados/reconciliador.py:844-845`; `app/ponto/services/cartorio.py:1007-1014,1083`). O vínculo delega a `escala_vigente` (`app/ponto/services/furos_vetados.py:10-16`), também usado pelo detector e pelos dois ramos de emissão do cartório (`app/ponto/management/commands/detectar_ausencias.py:139`; `app/ponto/services/cartorio.py:35,1347,1446`).

Os predicados coincidem para vínculo presente: degeneração e suspeita são consultadas pelos mesmos leitores (`app/ponto/services/furos_vetados.py:25-31`; `app/ponto/management/commands/detectar_ausencias.py:599-605`; `app/escala/regua_defesa.py:45-46`).

Há diferença contratual preexistente em `ec=None`: `motivo_do_veto` retorna `None`, enquanto `vetar_cobranca_se_degenerada` considera ausência de template degeneração (`app/ponto/services/furos_vetados.py:26-27`; `app/escala/regua_defesa.py:23-24,45-46`). Os caminhos normais do emissor pulam antes do funil quando falta vínculo (`app/ponto/management/commands/detectar_ausencias.py:140-141`; `app/ponto/services/cartorio.py:1348,1447-1448`).

Não encontrei reprodução estática desse caso como falha nova: julgamento atual sem vínculo produz `PREVISTO_COM_DIA_INDEFINIDO`, fora dos protestos que permitem nascer no c2 (`app/ponto/supra_juiz.py:304-305,382-383,396-397`; `app/ponto/services/furos_sem_cobranca.py:35`; `app/chamados/reconciliador.py:825-828`).

## 30/10 03:44 · sessao a081963814b3 · O261-C · verificador · origem=Code

**Não encontrei mudança de retorno ou de escrita de domínio causada pelo reordenamento do c2. Há uma ressalva concreta na justificativa: o veto de célula chama `_log.info` antes da cerca; portanto, “conjunções sem efeito colateral” é uma afirmação excessiva.** O caminho continua retornando `False`, mas agora pode alcançar essa chamada de log antes de consultar a cerca. (app/chamados/reconciliador.py:765; app/chamados/reconciliador.py:828–836)

A configuração SaaS versionada filtra esse `INFO` pela raiz em `WARNING`; emissão efetiva em produção: [não verificado]. (app/config/settings/saas.py:169–183)

Nos seis pontos anteriores:

1. **LEI-AKITA no corpo de `22c08b8e`: [não verificado].** Esta cópia não disponibiliza o histórico Git nem o corpo do commit. [não verificado]
2. **Pauta DP:** a prosa agora atribui expressamente sua geração ao script de passivo depois do pouso. Esse trecho não cria Pauta. Corpo do commit: [não verificado]. (app/chamados/reconciliador.py:688–694)
3. **Custo:** a cerca está depois dos gates existentes; `CustoDaCercaTest` exige zero chamadas às duas funções da cerca no veto por protesto, e o novo caso mede a batida com teto 31. Permanece a ressalva do log acima. Valores medidos e execução dos testes: [não verificado]. (app/chamados/reconciliador.py:797–836; app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:315–337; app/ponto/tests/test_selo_chokepoint_escrita.py:169; app/ponto/tests/test_selo_chokepoint_escrita.py:273–283)
4. **Guarda removida:** não está no c2 atual. Os dois chamadores de produção restringem a entrada a pergunta sem destino; o teste do cron exige sucesso na primeira passagem e compara pergunta, chamados e auditoria depois da segunda. (app/chamados/reconciliador.py:860–877; app/chamados/reconciliador.py:1375–1398; app/chamados/services/disputa_emissao.py:404–409; app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:296–308)
5. **R1/R1b:** agora têm mordida no código do teste: R1 exige ausência de recusa e o mesmo chamado renascido em estado vivo; R1b exige `admin_sem_fato_novo`, retorno falso e pergunta sem destino. (app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:149–169)
6. **Composição única:** os dois consumidores leem `competencia_do_dia_entregue`; a cerca exclui dia ausente antes da chamada, enquanto o vigia classifica `None` como `aberta`. (app/chamados/reconciliador.py:705–716; app/chamados/services/vigia_fio.py:413–441)

**Vigia:** não encontrei mudança de comportamento pela extração. Permanecem o universo, o filtro C6, a janela da empresa, o memo por empresa/mês/ano e a classificação sem data como `aberta`; o `bool` acrescentado preserva o retorno booleano de K6. (app/chamados/services/vigia_fio.py:404–441; app/ponto/janelas.py:60–63; app/folha/export.py:569–575)

Por classe, nos sítios do diff:

- **Segundo juiz:** nada; os novos caminhos consultam C6, C7, K6 e `motivos_atendidos`. (app/chamados/reconciliador.py:711–727; app/chamados/reconciliador.py:1465–1477)
- **Segundo escritor fora da porta:** nada; emissão e costura continuam por `abrir` e `reapontar_resolvedora`. (app/chamados/reconciliador.py:837; app/chamados/reconciliador.py:877; app/chamados/reconciliador.py:1513)
- **Relógio fora da borda:** nada nas funções novas nem nos trechos alterados. (app/chamados/reconciliador.py:685–727; app/chamados/reconciliador.py:1443–1478; app/chamados/services/vigia_fio.py:419–441)
- **Derivação paralela:** nada novo; a composição foi centralizada. (app/chamados/services/vigia_fio.py:419–441; app/chamados/reconciliador.py:711–713)
- **Selo/RED sem mordida:** nada concreto nas asserções examinadas; RED executado e suíte verde: [não verificado]. (app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:149–169; app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:296–337)
- **Ordem de trava:** nada; os gates antecipados fazem leituras/comparações, e `abrir`, que trava o colaborador, permanece depois da cerca. (app/ponto/turnos.py:2516–2523; app/ponto/marcos.py:154–163; app/chamados/reconciliador.py:824–837; app/chamados/models.py:666–669)
- **Pouso antes do pré-requisito:** nada demonstrado pelo diff; o RELATO registra B pousado e passivo aplicado. Comprovação operacional e remedida exigida: [não verificado]. (app/docs/RELATO.md:3–18)

## 30/10 03:42 · sessao 4da357248cbf · - · - · origem=Ronald (tmux)

c2: nenhum gate anterior à cerca escreve no banco ou adquire trava. Piso, isenção, ausência, protesto, célula e motor apenas leem/comparam; não dependem de algo que a cerca inicializava. O retorno e o estado persistido permanecem iguais, considerando os mesmos dados durante a execução. Referências: `app/ponto/services/cartorio.py:244`, `app/core/constants.py:18`, `app/chamados/catalogo/modulos.py:277`, `app/ponto/turnos.py:2518`, `app/chamados/reconciliador.py:824`, `app/ponto/marcos.py:154`, `app/chamados/catalogo/motor.py:130`.

Existe, porém, uma diferença observável no fluxo: o veto de célula chama `_log.info(...)` antes da cerca (`app/chamados/reconciliador.py:828-835`). Um intervalo que sairia pela cerca antecipada agora pode chegar ao log do veto. Portanto, a afirmação “conjunções sem efeito colateral” é inexata (`app/chamados/reconciliador.py:765`); isso não demonstra mudança de retorno nem escrita de domínio. Emissão efetiva do INFO na configuração ativa: [não verificado].

Ordem de trava: nada introduzido pelo reordenamento; `abrir` e `reapontar_resolvedora` continuam depois da cerca (`app/chamados/reconciliador.py:835-837`, `app/chamados/reconciliador.py:877`).

## 30/10 03:41 · sessao c7555ec23ce1 · - · - · origem=Ronald (tmux)

Não encontrei mudança de resultado no vigia pela extração: mantém universo, filtro pelo C6, memo local por empresa/competência e classificação sem data como `aberta`; a janela continua sendo `janela_atual`, e K6 retorna booleano (`app/chamados/services/vigia_fio.py:404-441`; `app/ponto/janelas.py:60-63`; `app/folha/export.py:569-575`).

Os testes novos têm verificações materiais: [nao verificado]

- R1 exige pergunta viva, nenhuma recusa de reabertura, mesmo chamado renascido e destino vivo (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:149-158`).
- R1b exige recusa `admin_sem_fato_novo`, retorno falso, pergunta sem destino e chamado resolvido (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:162-169`).
- Idempotência exige primeira passagem com destino vivo antes de comparar pergunta, chamados e trilha após a segunda passagem (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:296-308`).
- Custo verifica ausência de chamadas às cercas e teto de seis queries; o novo chokepoint mede `registrar_batida` com órfã sem célula sob teto 31 (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:315-337`; `app/ponto/tests/test_selo_chokepoint_escrita.py:169`; `app/ponto/tests/test_selo_chokepoint_escrita.py:273-283`).

Não identifiquei selo vazio concreto nesses trechos; execução dos testes e medições/RED alegados: [não verificado]. [nao verificado]

## 30/10 03:13 · sessao 96c72512bd21 · ORCAMENTO · verificador · origem=Code

Encontrei os seguintes defeitos e lacunas. Os cenários descrevem comportamento do código; ocorrência em produção: [não verificado].

1. **A expiração ainda pode ser engolida pelo TETO quando a gravação falha.**  
   Entrada: prova do RELATO aprovada, `PAREI: ! CLEAR` há 31 minutos, contador com cinco bloqueios no mesmo HEAD; estado do orçamento legível, mas diretório sem permissão para criar o temporário. `_orc_salva` falha antes de devolver a expiração; `_orcamento_contido` retorna `None`; o caminho antigo incrementa para seis e libera pelo TETO. Quebra **v2(b)**: faltam o `siga` explícito e o contador zerado. Reproduzido em memória.  
   Fontes: `bin/hook_stop_fila1.py:821`, `bin/hook_stop_fila1.py:867`, `bin/hook_stop_fila1.py:1071`, `bin/hook_stop_fila1.py:1078`.

2. **Marco ilegível é tratado como marco novo e permite redisparo.**  
   Estado: ciclo já terminou com `disparo.desfecho="seguiu"` e `marco_ref` conhecido. Sem nenhum clear/compact, a leitura de `ultimo_clear.json` passa a falhar; contexto continua em 60%. `ler_marco` devolve `None`; a diferença de referência apaga o disparo anterior e permite outro `DISPARO`. Quebra **§3: sem redisparo até clear ou compact**. Reproduzido em memória.  
   Fontes: `bin/orcamento_contexto.py:136`, `bin/hook_stop_fila1.py:783`, `bin/hook_stop_fila1.py:800`, `bin/hook_stop_fila1.py:855`.

3. **O desligamento depende de conseguir a trava primeiro.**  
   Entrada: `ORCAMENTO_CONTEXTO=0`, topo CLEAR e trava do orçamento ocupada por mais de cinco segundos. O hook devolve `siga: orçamento ocupado`, embora o instrumento esteja desligado; nem chega à consulta da variável ou ao registro de desligamento. Quebra **§5** e expõe uma precedência errada entre corte e trava. Reproduzido em memória.  
   Fontes: `bin/hook_stop_fila1.py:739`, `bin/hook_stop_fila1.py:748`, `bin/hook_stop_fila1.py:772`.

4. **Fila vazia passa a bloquear quando existe gatilho.**  
   Entrada: todos os itens fechados, topo neutro, nenhum produtor e contexto em 60%. O orçamento devolve PREPARA-CLEAR antes de alcançar a liberação por fila vazia. É regressão objetiva do **caminho antigo de fila vazia**; o contrato não declara expressamente uma exceção ao gatilho para esse caso. O selo cobre fila vazia somente com medida ilegível, sem gatilho. Reproduzido em memória.  
   Fontes: `bin/hook_stop_fila1.py:855`, `bin/hook_stop_fila1.py:1014`, `bin/hook_stop_fila1.py:1039`; `bin/tests/test_orcamento_contexto.sh:145`.

5. **Estado confirmado com append falho perde definitivamente o evento.**  
   Estado: JSON gravável, mas `orcamento.log` sem permissão. `_orc_salva` substitui o estado; `_orc_trilha` engole a falha; a lista de eventos é descartada. Num ATENDIDO/SEGUIU, o próximo Stop já encontra o desfecho consumido e não repõe a linha: o ESTADO subconta permanentemente. Quebra **§4: trilha por disparo/desfecho e contadores pelo log**. O selo verifica apenas a direção inversa, quando o estado não grava.  
   Fontes: `bin/hook_stop_fila1.py:694`, `bin/hook_stop_fila1.py:715`, `bin/hook_stop_fila1.py:723`, `bin/hook_stop_fila1.py:725`; `bin/orcamento_contexto.py:223`; `bin/tests/test_orcamento_contexto.sh:395`.

6. **Um segundo clear apaga a prova do primeiro atendimento.**  
   Sequência: PAREI em t=0; `/clear` em +10 minutos; outro `/clear` em +40; nenhum Stop entre eles. O escritor conserva apenas +40 e o hook registra `SEGUIU`, apesar do atendimento em +10. Quebra a definição de **ATENDIDO do §4**. Classificação reproduzida em memória.  
   Fontes: `bin/marco_de_clear.py:38`, `bin/marco_de_clear.py:47`; `bin/hook_stop_fila1.py:789`, `bin/hook_stop_fila1.py:792`.

7. **A atualização do marco não serializa clear e compact concorrentes.**  
   Intercalação: compact lê o marco antigo; clear publica o novo; compact publica depois com o `clear_hora` antigo. O `replace` atômico não protege a sequência leitura–atualização, e o atendimento pode desaparecer. Quebra **§4** e a conservação prometida pelo escritor. Intercalação reproduzida em memória; existência dessa concorrência no harness: [não verificado].  
   Fontes: `bin/marco_de_clear.py:36`, `bin/marco_de_clear.py:41`, `bin/marco_de_clear.py:43`, `bin/marco_de_clear.py:52`.

8. **Falha ao ler HEAD produz testemunha inválida sem aviso — e o selo aceita.**  
   Entrada: provas do deploy passaram, mas o `git rev-parse HEAD` do append falha. `printf` ainda grava `data hora  0` com sucesso; o aviso não executa. O leitor descarta os três campos restantes: três deploys assim contam zero. Quebra **v2(g), testemunha `hora head rc`**. A regex do selo aceita HEAD vazio por usar `[0-9a-f]*`.   [nao verificado]
   Fontes: `bin/deploy.sh:347`, `bin/deploy.sh:348`; `bin/orcamento_contexto.py:149`; `bin/tests/test_orcamento_contexto.sh:278`, `bin/tests/test_orcamento_contexto.sh:280`.

9. **O RED não prova que reprovação da casca interrompe o deploy.**   [nao verificado]
   Mutação concreta: substituir `falhou` por `echo` na linha 219 do deploy. O teste continua encontrando o `if ! bash`, a guarda e o append esperados; porém o deploy passa a recarregar depois da reprovação. `_julga_deploy` executa fragmentos isolados e verifica a porta por texto, sem provar sua consequência. Lacuna de **v2(g), classe “RED sem mordida”**. A porta do deploy atual funciona; o defeito aqui é sua proteção pelo selo.   [nao verificado]
   Fontes: `bin/deploy.sh:70`, `bin/deploy.sh:219`; `bin/tests/test_orcamento_contexto.sh:265`, `bin/tests/test_orcamento_contexto.sh:284`, `bin/tests/test_orcamento_contexto.sh:289`.

Nas demais áreas solicitadas:

- **Prova do RELATO:** não encontrei bypass introduzido; continua antes da consulta ao orçamento. `bin/hook_stop_fila1.py:1003`, `bin/hook_stop_fila1.py:1011`.
- **PAREI comum e ESPERANDO provado:** não encontrei segundo juiz; reutiliza `_relato_parou` e memoriza uma única avaliação da espera, preservando a prova por avanço de arquivo. `bin/hook_stop_fila1.py:653`, `bin/hook_stop_fila1.py:845`, `bin/hook_stop_fila1.py:847`, `bin/hook_stop_fila1.py:1025`.
- **Falha aberta antiga:** permanece no `except` principal; as exceções próprias do orçamento ficam contidas. A interação defeituosa dessa contenção com a expiração está no achado 1. `bin/hook_stop_fila1.py:863`, `bin/hook_stop_fila1.py:1093`.
- **Relógio, derivação paralela e segundo escritor:** não encontrei defeito adicional: avaliador recebe `agora`, gerador usa `ler`/`linha`, e o orçamento chama os juízes existentes. `bin/orcamento_contexto.py:174`, `bin/orcamento_contexto.py:207`; `bin/gerar_estado.py:274`; `bin/hook_stop_fila1.py:848`.
- **Ordem das travas existentes:** não encontrei aquisição inversa à sequência orçamento → memória da espera; a trava da espera é liberada ao terminar sua avaliação. A ausência de serialização do marco é o achado 7. `bin/hook_stop_fila1.py:743`, `bin/hook_stop_fila1.py:845`, `bin/hook_stop_fila1.py:572`, `bin/hook_stop_fila1.py:602`.
- **Matchers e medição:** o selo vincula os comandos a `compact` e `clear`; o avaliador declara janela/fonte e contém falhas de leitura do transcript. Instalação efetiva no host e correção da fonte externa: [não verificado]. `bin/tests/test_handoff_sessao.sh:60`, `bin/tests/test_handoff_sessao.sh:78`; `bin/orcamento_contexto.py:32`, `bin/orcamento_contexto.py:85`, `bin/orcamento_contexto.py:105`.

## 30/10 03:12 · sessao 229461fbb71f · - · - · origem=Ronald (tmux)

Achados confirmados:

1. **Marco ilegível rearma ciclo já encerrado.** Com `disparo.desfecho="seguiu"` e `marco_ref` existente, uma falha ao ler `ultimo_clear.json` vira `None`; o hook interpreta isso como mudança de marco, apaga o disparo e pode emitir outro `DISPARO` pelos mesmos 60%. Reproduzi em memória. Quebra §3, “não redispara até próximo marco de clear ou compact”. (`bin/orcamento_contexto.py:125-137`; `bin/hook_stop_fila1.py:781-800`, `855-858`.)

2. **Dois clears entre Stops classificam atendimento correto como expiração.** PAREI em t=0; `/clear` em +10 min; outro `/clear` em +40 min; próximo Stop depois disso. O escritor conserva somente +40; o hook registra `SEGUIU`, embora o clear de +10 tenha atendido dentro da janela. Reproduzi a classificação pela função real com mocks em memória. Quebra §4, definição de ATENDIDO. (`bin/marco_de_clear.py:38-47`; `bin/hook_stop_fila1.py:789-796`.)

3. **Concorrência clear/compact pode perder o clear atendido.** Compact lê o marco antigo; clear publica o novo; compact publica depois, trazendo `clear_hora` antigo. `os.replace` protege a substituição, mas não serializa a leitura e atualização. Reproduzi esse interleaving com as duas chamadas reais e armazenamento em memória. Quebra §4 e a conservação declarada pelo próprio escritor. Concorrência real em produção: [não verificado]. (`bin/marco_de_clear.py:33-52`.)

4. **Falha no append perde definitivamente eventos dos contadores.** Estado gravável e `orcamento.log` sem permissão: `_orc_salva` persiste DISPARO/desfecho, `_orc_trilha` engole o erro, e os eventos são descartados. O próximo Stop lê o evento como consumido, enquanto o ESTADO continua contando somente o log. Quebra §4; o selo cobre apenas o sentido contrário, estado que falha antes da trilha. (`bin/hook_stop_fila1.py:687-695`, `708-725`; `bin/orcamento_contexto.py:217-234`; `bin/tests/test_orcamento_contexto.sh:395-399`.)

Áreas lidas sem defeito adicional identificado:

- Relógio da medição recebido por argumento; leituras de relógio nas bordas Stop, CLI e gerador. (`bin/orcamento_contexto.py:174-178`, `207`, `257`; `bin/hook_stop_fila1.py:763`; `bin/gerar_estado.py:276`.)
- Último assistente fora de sidechain, soma dos três campos, ausência de usage/modelo desconhecido e contenção das falhas de leitura. (`bin/orcamento_contexto.py:62-106`.)
- Gerador usa `ler`/`linha` do avaliador; não introduz segundo juiz nem segundo escritor do orçamento. (`bin/gerar_estado.py:271-276`; `bin/orcamento_contexto.py:207-250`.)
- Janela tem número e fonte declarados; correção da fonte externa e autenticidade histórica da fixture: [não verificado]. (`bin/orcamento_contexto.py:32-43`; `bin/tests/fixtures/orcamento_transcript.jsonl:1-8`.)

Observação de menor força: `contar_marcos` conta também linhas `rc=1` ou `rc=erro`; confirmei em memória que duas dessas linhas mais uma `rc=0` resultam em três marcos. Entretanto, o escritor fornecido publica literalmente `0`; a presença dessas linhas na testemunha de produção é [não verificado]. (`bin/orcamento_contexto.py:149-165`; `bin/deploy.sh:347`.)

## 30/10 03:11 · sessao 86cb32d779ac · - · - · origem=Ronald (tmux)

Achados nas áreas delegadas:

1. **O selo do deploy não prova que reprovação da casca interrompe o deploy.** `_julga_deploy` executa somente o bloco de append e a guarda isolados; para a porta da casca, exige apenas a presença textual do `if ! bash ...`. Mutação concreta: trocar `falhou` por `echo` em `bin/deploy.sh:219` mantém esses checks, embora passe a recarregar e registrar marco após reprovação. Quebra a prova exigida por v2(g), classe “RED sem mordida”. No deploy atual, a porta funciona: `falhou` termina com `exit 1`. (`bin/tests/test_orcamento_contexto.sh:265`, `bin/tests/test_orcamento_contexto.sh:280`, `bin/tests/test_orcamento_contexto.sh:284`, `bin/tests/test_orcamento_contexto.sh:289`; `bin/deploy.sh:70`, `bin/deploy.sh:218`.)

2. **Falha na leitura do HEAD produz testemunha inválida e subcontagem silenciosa.** Se o `git rev-parse HEAD` do append falhar, `printf` ainda escreve `data hora  0` e retorna sucesso; o aviso não aparece. O leitor descarta essa linha porque possui somente três campos. Três deploys provados nessas condições contam como zero e não disparam (c), contrariando a testemunha `hora head rc` de v2(g). O selo aceita o defeito: sua regex permite HEAD vazio (`[0-9a-f]*`) e executa o fragmento numa raiz temporária sem repositório. Ocorrência no host: [não verificado]. (`bin/deploy.sh:347`; `bin/orcamento_contexto.py:149`; `bin/tests/test_orcamento_contexto.sh:278`, `bin/tests/test_orcamento_contexto.sh:280`.)

3. **O marco tem corrida de atualização.** Compact lê o marco anterior; clear grava marco novo; compact substitui o arquivo com o `clear_hora` antigo que havia lido. Isso perde a conservação do clear e pode perder o ATENDIDO exigido pelo contrato 4. A escrita é atômica, mas a sequência leitura–modificação–substituição não tem trava. Concorrência efetiva de SessionStart no host: [não verificado]. (`bin/marco_de_clear.py:39`, `bin/marco_de_clear.py:43`, `bin/marco_de_clear.py:52`.)

Sem defeito encontrado na guarda atual do append: distingue normal/cópia de agendado, exclui casca pulada e fica depois das provas de casca e rotas. (`bin/deploy.sh:218`, `bin/deploy.sh:321`, `bin/deploy.sh:324`, `bin/deploy.sh:345`.)

O selo vincula os comandos exatos aos matchers `compact` e `clear`; instalação efetiva no host permanece [não verificado], pois depende de `.claude/settings.json`, deliberadamente externo ao versionamento. (`bin/tests/test_handoff_sessao.sh:15`, `bin/tests/test_handoff_sessao.sh:60`, `bin/tests/test_handoff_sessao.sh:70`, `bin/tests/test_handoff_sessao.sh:75`, `bin/tests/test_handoff_sessao.sh:78`; `.gitignore:66`.)

## 30/10 03:09 · sessao 183f01bf09fa · O261-C · verificador · origem=Code

1. **2º juiz / derivação paralela de fonte única:** o HIST tocado ainda decide o dia exclusivamente por `contexto_json__data_turno`, enquanto `dia_do_fato` prioriza `celula.data`. Um encerrado com célula do dia e JSON sem essa chave é descartado antes de `motivos_atendidos`. Para pergunta morta de intervalo, c1 e c2 recusam, deixando-a órfã. O filtro é anterior, mas está no sítio tocado. (`app/chamados/reconciliador.py:1452`, `app/chamados/reconciliador.py:1454`, `app/chamados/juizes.py:88`, `app/chamados/reconciliador.py:363`, `app/chamados/reconciliador.py:764`, `app/chamados/reconciliador.py:727`)

2. **Selo/RED:** há uma incoerência no teste novo de idempotência. A segunda chamada direta exige igualdade de todos os campos, mas percorre novamente o c2 e `reapontar_resolvedora`, que sempre relavra e renova `julgado_em`. Sem congelamento externo, a asserção deve falhar nesse campo; não demonstra o contrato anunciado. (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:263`, `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:264`, `app/chamados/services/disputa_emissao.py:209`, `app/chamados/reconciliador.py:1487`, `app/chamados/services/fio.py:798`, `app/chamados/veredito_lavrado.py:228`)

- **2º escritor fora da porta: nada.** A costura permanece em `reapontar_resolvedora`. (`app/chamados/reconciliador.py:871`, `app/chamados/reconciliador.py:1507`)
- **Relógio fora da borda: nada nos trechos novos.** (`app/chamados/reconciliador.py:703`, `app/chamados/reconciliador.py:721`)
- **Ordem de trava: nada.** A emissão continua pelo `abrir`, com trava de Colaborador antes do dedup. (`app/chamados/reconciliador.py:831`, `app/chamados/models.py:666`, `app/chamados/models.py:669`)
- **Pouso antes do pré-requisito: nada demonstrável.** O RELATO declara B no ar e passivo aplicado; a confirmação externa desses registros está [não verificado]. (`app/docs/CORTES.md:399`, `app/docs/RELATO.md:4`, `app/docs/RELATO.md:7`)

Resultado efetivo da suíte: [não verificado]; os achados acima são de leitura estática. [nao verificado]

## 30/10 03:08 · sessao aaca92fde8a5 · - · - · origem=Ronald (tmux)

- **Relógio fora da borda: nada novo.** Os helpers novos consultam janela, K6, C7 e C6 sem ler `now()`/`today()` diretamente (`app/chamados/reconciliador.py:703`, `app/chamados/reconciliador.py:710`, `app/chamados/reconciliador.py:724`).

- **Selo/RED: problema concreto no teste novo de idempotência.** `test_reatar_duas_vezes_e_idempotente` chama a porta diretamente pela segunda vez e exige igualdade de **todos** os campos da pergunta (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:263`, `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:264`). A segunda passagem exclui o próprio chamado em `_derivar_chamado_destino`, pois ele já tem pergunta não materializada (`app/chamados/services/disputa_emissao.py:209`); segue por HIST vazio ao c2 (`app/chamados/reconciliador.py:1477`, `app/chamados/reconciliador.py:1487`), reutiliza o chamado vivo (`app/chamados/reconciliador.py:793`, `app/chamados/models.py:670`) e reaponta pela porta (`app/chamados/reconciliador.py:871`). Essa porta sempre relavra e salva (`app/chamados/services/fio.py:797`), renovando `julgado_em` (`app/chamados/veredito_lavrado.py:228`). Portanto, o teste novo exige uma idempotência que o caminho atual não oferece; a igualdade da linha falha por esse timestamp. A escrita redundante de produção é anterior e não constitui achado independente no diff; o sítio novo problemático é a asserção do teste (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:264`).

Execução da suíte e resultado efetivo: [não verificado]. [nao verificado]

## 30/10 03:07 · sessao fe94d09b3594 · - · - · origem=Ronald (tmux)

Achado na classe **2º juiz / derivação paralela do dia**: o HIST tocado ainda seleciona encerrados exclusivamente por `contexto_json__data_turno`, descartando chamado cujo dia está na célula, embora `dia_do_fato` priorize justamente `celula.data`. O ramo vivo já consulta esse juiz. (`app/chamados/reconciliador.py:1452`, `app/chamados/reconciliador.py:1454`, `app/chamados/juizes.py:88`, `app/chamados/services/disputa_emissao.py:213`)

Contraexemplo: pergunta morta de intervalo, competência aberta, encerrado com célula do dia e contexto sem `data_turno`. O HIST não encontra o candidato; c1 recusa intervalo; c2 recusa pergunta morta, mantendo-a órfã. Ausência dessa chave em chamados históricos é reconhecida no próprio código. (`app/chamados/reconciliador.py:1454`, `app/chamados/reconciliador.py:363`, `app/chamados/reconciliador.py:764`, `app/chamados/reconciliador.py:727`, `app/chamados/juizes.py:1713`, `app/chamados/catalogo/modulos.py:19`)

O filtro já existia, mas está em sítio tocado pelo diff fornecido; o escopo registrado inclui sítios tocados. (`app/docs/CORTES.md:385`; diff fornecido [não verificado por Git])

Nas chamadas novas a `motivos_atendidos`, `pergunta_viva`, `competencia_entregue` e `dia_da_pergunta_trancado`: **nada adicional** de juiz duplicado; elas delegam às autoridades existentes. (`app/chamados/reconciliador.py:713`, `app/chamados/reconciliador.py:716`, `app/chamados/reconciliador.py:727`, `app/chamados/reconciliador.py:1470`)

## 30/10 03:07 · sessao d5f4610bd274 · - · - · origem=Ronald (tmux)

- **2º escritor fora da porta: nada.** O diff encaminha os novos casos ao c2 e ao HIST; ambos continuam usando `reapontar_resolvedora`, cuja escrita ocorre na porta. (`app/chamados/reconciliador.py:764`, `app/chamados/reconciliador.py:1470`, `app/chamados/reconciliador.py:871`, `app/chamados/reconciliador.py:1507`, `app/chamados/services/fio.py:792`).
- **Ordem de trava: nada.** A emissão continua por `ChamadoColaborador.abrir`, que toma a trava de Colaborador antes do dedup; nenhum novo bloco de trava foi introduzido nos trechos examinados. (`app/chamados/reconciliador.py:831`, `app/chamados/models.py:666`, `app/chamados/models.py:669`, `app/chamados/reconciliador.py:685`, `app/chamados/reconciliador.py:1459`).
- **Pouso antes do pré-requisito: nada demonstrável.** O corte exige B antes de C; o RELATO declara B no ar, passivo aplicado e C remedido após B. Os logs citados não estão comprovados nesta leitura. (`app/docs/CORTES.md:399`, `app/docs/RELATO.md:3`, `app/docs/RELATO.md:4`, `app/docs/RELATO.md:7`; execução externa: [não verificado]).

## 30/10 03:03 · sessao c6d879dd914a · ORCAMENTO · verificador · origem=Code

Encontrei os seguintes contraexemplos no código. Ocorrência desses cenários em produção: [nao verificado].

1. **A expiração pode ser engolida pelo TETO quando a trava está ocupada.** Cenário: CLEAR ativo há 31 minutos, contador do HEAD atual em `blocks=5`, trava do orçamento ocupada por mais de cinco segundos. O orçamento retorna `None`; o caminho antigo incrementa para seis e libera pelo TETO, sem o `siga` explícito nem zerar o contador. Quebra **v2(b)**. — [hook_stop_fila1.py:724](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:724), [hook_stop_fila1.py:1040](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:1040), [hook_stop_fila1.py:1056](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:1056).

2. **Transcript truncado pode disparar sozinho com usage antigo.** Cenário: penúltima linha é um assistant com 600000 tokens; a última começa `{"type":"assistant","message":{"usage":` e está truncada; marco recente, nenhum gatilho temporal ou de deploy. O parser ignora a linha ilegível e retorna os 60% anteriores. Quebra **§1(a)**, último turno, e **§3/v2(e)**, medida ilegível deve virar “não sei”. — [orcamento_contexto.py:76](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/orcamento_contexto.py:76).

3. **O gatilho de deploy pode disparar com apenas dois deploys posteriores ao clear.** Cenário: deploy às `10:00:00.100`, clear às `10:00:00.900`, depois mais dois deploys. O histórico elimina frações e o contador compara com `int(desde)`: inclui o deploy anterior e conta três. Quebra **§1(c)/§4**, disparo somente no limiar. O selo exige essa inclusão, portanto preserva a sobrecontagem. — [deploy.sh:347](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/deploy.sh:347), [orcamento_contexto.py:147](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/orcamento_contexto.py:147), [test_orcamento_contexto.sh:325](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/tests/test_orcamento_contexto.sh:325).

4. **Duas leituras do marco permitem liberar um CLEAR velho depois de um clear novo.** Cenário: disparo ativo e topo CLEAR; `SessionStart clear` substitui o marco entre a leitura do ciclo pelo hook e a releitura pelo avaliador. A decisão usa o ciclo antigo, mas as medidas usam o marco novo; pode retornar `libera` e salvar essa combinação. O escritor do marco não participa da trava do orçamento. Quebra o encerramento/rearme pelo marco novo: é inconsistência de snapshot e sincronização. — [hook_stop_fila1.py:753](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:753), [hook_stop_fila1.py:770](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:770), [orcamento_contexto.py:159](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/orcamento_contexto.py:159), [marco_de_clear.py:40](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/marco_de_clear.py:40).

5. **O flock não torna trilha e estado uma transação.** Cenário: append de `DISPARO` funciona, mas `os.replace` do estado falha; o próximo Stop relê o estado anterior e apenda outro `DISPARO` no mesmo ciclo. A mesma ordem permite duplicar desfechos e inflar os contadores publicados. Quebra **§3**, sem redisparo, e **§4**, uma linha por evento/desfecho. — [hook_stop_fila1.py:704](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:704), [hook_stop_fila1.py:765](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:765), [hook_stop_fila1.py:826](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:826).

6. **`ATENDIDO` pode ser contado sem prova dos 30 minutos.** Cenário: disparo em `t=1000`, nenhum Stop observou o topo CLEAR, e clear em `t=10000`. Sem `clear_parei_desde`, a condição de atraso não vale e o código registra `ATENDIDO`, apesar dos 150 minutos decorridos. Quebra a definição do contador em **§4**: não existe evidência da janela exigida. — [hook_stop_fila1.py:758](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:758), [hook_stop_fila1.py:764](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:764).

7. **Um clear atendido pode desaparecer antes de ser contado.** Cenário: CLEAR liberado, `/clear` dentro do prazo, depois `compact` antes do próximo Stop. O segundo marco sobrescreve o primeiro; o hook vê apenas `compact` e registra `RESET`, perdendo o `ATENDIDO`. Quebra **§4**: o desfecho depende de um evento que não foi conservado. — [marco_de_clear.py:40](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/marco_de_clear.py:40), [hook_stop_fila1.py:753](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:753), [hook_stop_fila1.py:767](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:767).

8. **O selo do deploy aceita histórico sem linha e prova não executada.** Seu juiz verifica posições textuais e executa somente a guarda. Aceita trocar o `printf` por `:` mantendo o redirecionamento; também aceita tornar a prova de casca inalcançável com `if false`. Os mutantes existentes alteram apenas a guarda. É **selo sem mordida** para **v2(g)/§4**: não prova a escrita efetiva nem sua dependência da casca OK. — [test_orcamento_contexto.sh:251](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/tests/test_orcamento_contexto.sh:251), [test_orcamento_contexto.sh:266](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/tests/test_orcamento_contexto.sh:266), [test_orcamento_contexto.sh:272](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/tests/test_orcamento_contexto.sh:272).

9. **O selo do handoff aceita `clear` sem carregar o handoff.** Cenário: matcher `clear` contém somente `python3 .../marco_de_clear.py clear`. A validação passa: exige `cat HANDOFF-SESSAO.md` apenas em `compact`. Falha contra **v2(h)**, na exigência de vincular o handoff aos matchers `compact` **e** `clear`. Configuração realmente instalada: [nao verificado]. — [test_handoff_sessao.sh:69](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/tests/test_handoff_sessao.sh:69), [test_handoff_sessao.sh:72](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/tests/test_handoff_sessao.sh:72).

10. **Não há a função pura prometida; o avaliador ainda lê o relógio internamente.** Cenário: a CLI chama `avaliar` sem `agora`; com os mesmos arquivos e argumentos, duas chamadas em lados diferentes das seis horas produzem decisões diferentes por uma leitura interna do relógio. É a classe **relógio fora da borda**, contra o desenho “função pura + CLI”. O caminho Stop passa `agora` explicitamente e evita essa leitura específica. — [orcamento_contexto.py:157](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/orcamento_contexto.py:157), [orcamento_contexto.py:238](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/orcamento_contexto.py:238), [hook_stop_fila1.py:770](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:770).

Nas demais áreas solicitadas:

- **RELATO, PAREI comum e ESPERANDO:** não achei quebra funcional nesses caminhos. A prova do RELATO continua primeiro; os juízes existentes são reutilizados; o memo evita consumir duas vezes a prova de mtime. — [hook_stop_fila1.py:972](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:972), [hook_stop_fila1.py:814](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:814), [hook_stop_fila1.py:653](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:653).
- **Fila vazia:** sem resposta própria do orçamento, continua liberando. Com gatilho, PREPARA-CLEAR antecede essa saída; isso muda o caminho antigo, mas §2 manda disparar sem declarar exceção para fila vazia. — [hook_stop_fila1.py:983](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:983), [hook_stop_fila1.py:1008](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:1008).
- **Falha aberta:** não achei exceção própria do orçamento escapando para o `try` principal; a contenção devolve o caminho antigo. O fail-open principal permanece. — [hook_stop_fila1.py:832](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:832), [hook_stop_fila1.py:1062](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:1062).
- **Segundo juiz/escritor e ordem das travas:** não achei juiz paralelo da fila/PAREI, escritor externo do TETO ou inversão ORC↔ESPERA. Há, porém, a corrida com o escritor do marco descrita no achado 4. — [hook_stop_fila1.py:816](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:816), [hook_stop_fila1.py:841](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:841), [hook_stop_fila1.py:572](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:572).
- **Deploy e selos na régua:** não achei falha na guarda atual do append após as provas, nem selo novo órfão. As falhas estão na precisão da testemunha e na força do selo, conforme achados 3 e 8. — [deploy.sh:218](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/deploy.sh:218), [deploy.sh:324](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/deploy.sh:324), [deploy.sh:345](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/deploy.sh:345), [regua.sh:138](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/regua.sh:138).

## 30/10 03:00 · sessao 8c639f7673c9 · - · - · origem=Ronald (tmux)

Achados nos selos, provados em memória, sem escrever arquivos:

1. **O selo aceita `/clear` sem carregar o handoff.** O `cat HANDOFF-SESSAO.md` só é exigido para `compact`; para `clear`, basta o comando de escrever o marco. Cenário: `SessionStart.clear` contém apenas `python3 .../marco_de_clear.py clear`. A validação passa, mas a sessão nova não recebe o handoff. Quebra v2(h), considerando sua exigência de vincular o comando aos matchers `compact` **e** `clear`. A configuração efetivamente instalada é [nao verificado]. — `bin/tests/test_handoff_sessao.sh:69`, `bin/tests/test_handoff_sessao.sh:72`.

2. **O selo do deploy aceita prova de casca inalcançável e histórico sem linha.** Seu juiz verifica posição textual das referências e executa apenas a guarda do append. Duas mutações em memória foram aceitas: [nao verificado]
   - Embrulhar a prova de casca de `bin/deploy.sh:214` em `if false; then ... fi`.
   - Trocar o `printf` de `bin/deploy.sh:347` por `:` mantendo `>> historico.log`: nenhuma linha é apendada.
   
   Os mutantes existentes só alteram a guarda. Logo o selo não prova a propriedade anunciada de v2(g)/item 4: testemunha escrita somente com casca OK e que efetivamente permita contar deploys. — `bin/tests/test_orcamento_contexto.sh:251`, `bin/tests/test_orcamento_contexto.sh:254`, `bin/tests/test_orcamento_contexto.sh:263`, `bin/tests/test_orcamento_contexto.sh:272`.

3. **O novo selo não cobre cruzamentos importantes com os caminhos antigos.** A fixture tem um único item; os casos de PAREI comum não incluem outro item andável, nem RELATO sem prova com gatilho/CLEAR ativo. O selo antigo do PAREI chama somente `_relato_parou()`, portanto não cobre a integração nova em `main`. Isso é lacuna de regressão de v2(i), não prova isolada de defeito funcional nesses caminhos. — `bin/tests/test_orcamento_contexto.sh:39`, `bin/tests/test_orcamento_contexto.sh:214`, `bin/tests/test_hook_parei_com_andavel.sh:36`, `bin/tests/test_relato_prova_no_stop.sh:20`.

4. **A validação nova do SessionStart ignora o tipo do hook.** Executei o trecho real de validação com `type="prompt"` e as strings de comando exigidas: retornou zero. `_cmds` só recolhe `command`; portanto não garante que os comandos de marco/handoff serão executados como comandos. Quebra a força probatória exigida em v2(h); ocorrência na configuração real é [nao verificado]. — `bin/tests/test_handoff_sessao.sh:60`, `bin/tests/test_handoff_sessao.sh:74`.

Áreas lidas sem defeito encontrado neste recorte:

- Os novos selos entram no loop existente da régua; não são selos órfãos. — `bin/regua.sh:138`.
- A prova do RELATO permanece antes da consulta ao orçamento. — `bin/hook_stop_fila1.py:972`, `bin/hook_stop_fila1.py:980`.
- O selo novo verifica motivos na trilha e reinicia a árvore de logs entre cenários principais. — `bin/tests/test_orcamento_contexto.sh:88`, `bin/tests/test_orcamento_contexto.sh:118`, `bin/tests/test_orcamento_contexto.sh:165`.
- Instalação real de `SessionStart`/`Stop` não é demonstrada pela cópia: o próprio selo declara que consulta `.claude/settings.json` vivo, fora do git. — `bin/tests/test_handoff_sessao.sh:15`; [nao verificado].

_350 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
