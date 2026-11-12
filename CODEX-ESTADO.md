# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 15:30** · publicado em **12/11/2026 15:31** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 15:30 · sessao e5f3193debbc · O168 · verificador · origem=Code

**Não considero a O168 encerrada:** os quatro fechamentos diretos receberam a guarda, mas há caminhos indiretos que chegam ao mesmo escritor sem consultá-la; além disso, “44 preservados” conta avaliações recusadas, não demonstra 44 chamados distintos salvos (`app/ponto/services/cartorio.py:1040`, `:1568`, `:1581`, `:1602`, `:963`; `app/chamados/signals.py:43`; `app/chamados/reconciliador.py:524`).

1. **Sítios diretos do cartório — os quatro passam pelo helper.**

   | Sítio | Passa por `_fecham_cobrancas_sem_furo`? | Evidência |
   |---|---|---|
   | Adiados pela lâmpada, depois do E2 | Sim | `app/ponto/services/cartorio.py:1035-1040` |
   | RECONCILIA-MARCO: marcos cobrados acenderam; inclui quem não protesta em dia sem trabalho | Sim | `app/ponto/services/cartorio.py:1562-1575` |
   | MARCO-INEXISTENTE: marco cobrado deixou de existir na escala | Sim | `app/ponto/services/cartorio.py:1576-1593` |
   | B5.3b: `COBRANCA_SEM_FURO` ou `COBRANCA_EM_DIA_SEM_TRABALHO` | Sim | `app/ponto/services/cartorio.py:1594-1606` |

   O helper pergunta a `pode_executar_csf` antes de chamar a porta; só devolve como fechados aqueles cuja transição retornou verdadeiro (`app/ponto/services/cartorio.py:955-966`).

2. **Há fechadores indiretos e externos sem essa guarda.**

   O desvio mais próximo está **antes do helper dos adiados**: cartório → E2 → `DisputaSupervisao.fechar()` → signal S88 → `ch.reconciliar()`. O E2 fecha a disputa quando C7 permite; o save dispara S88, que pede fechamento do chamado-pai sem consultar `pode_executar_csf` (`app/ponto/services/cartorio.py:1035-1040`; `app/chamados/reconciliador.py:609-615`; `app/chamados/models.py:1933-1937`; `app/chamados/signals.py:19-43`).

   A porta central não corrige essa ausência: decide pela matriz, pela premissa e pela pergunta **viva**, sem testar qualquer pergunta anteriormente respondida. Uma resposta já consumida pode, portanto, ficar fora da proteção existente da porta (`app/chamados/models.py:672-694`; `app/ponto/supra_juiz.py:543-549`). Reprodução dinâmica e ocorrência desse desvio em produção: **[não verificado]**.

   | Outro caminho | Helper / guarda de conversa | Evidência |
   |---|---|---|
   | Receiver da batida, posterior ao cartório → premissa morta → `_retratar` | Não / não | `app/ponto/signals.py:206-215`; `app/chamados/reconciliador.py:1433-1442`, `:1454-1465`, `:519-524` |
   | Consumidor de `EventoCelula` → absolvição pela célula | Não / não | `app/chamados/reconciliador.py:1369`, `:997-1010` |
   | Consumidor de `EventoCelula` → premissa morta | Não / não | `app/chamados/reconciliador.py:1373`, `:1465` |
   | Consumidor → lastro: todos os marcos cobrados acenderam | Não / não | `app/chamados/reconciliador.py:1383-1384`; `app/ponto/services/lastro.py:388-403` |
   | `detectar_ausencias`: batida satisfez os marcos cobrados | Não / não | `app/ponto/management/commands/detectar_ausencias.py:404-413`, `:845-849` |
   | Entrada sincronizada/API: marcos cobrados acesos | Não / não | `app/ponto/services/entrada_sincronizada.py:29-32` |
   | Ausência aprovada, premissa morta e lâmpada sem acusação | Não / não | `app/chamados/services/silencio_ausencia.py:29-42`, `:94-106` |
   | Fecho por afastamento ou cobertura | Não / não | `app/ponto/management/commands/detectar_ausencias.py:821-849` |
   | Designação/declaração de cobertura | Não / não | `app/ponto/services/cobertura.py:115-123`, `:311-319` |

   A retenção de `_retratar` não equivale à guarda CSF: protege irmã respondida **sem consumo** e justificativa alheia pendente, enquanto CSF protege qualquer `respondida_em` não nulo nas duas relações (`app/chamados/reconciliador.py:438-455`; `app/ponto/supra_juiz.py:543-549`).

   Materialização também fecha destino e clones sem CSF, mas ocorre no contexto de processamento de resposta; classificá-la integralmente como fechamento automático proibido exige distinguir o ato humano que a autorizou — **[não verificado]** (`app/chamados/services/materializacao.py:740-787`).

3. **Quem escreve e quem lê os campos e relações.**

   | Campo/relação | Quem escreve | Quem lê neste circuito |
   |---|---|---|
   | `ChamadoColaborador.status_local` | No fechamento: helper → `transicionar_com_pauta` → `reconciliar` → `_gravar_estado`; este atribui e salva. Nascimento também escreve pela criação | Seleção de vivos, matriz/gates, cartório e testes | [nao verificado]
   | Evidência | `app/ponto/services/cartorio.py:965`; `app/chamados/services/fecho_runtime.py:157-169`; `app/chamados/models.py:651-655`, `:228-229`, `:296`, `:1096` | `app/ponto/management/commands/supra_juiz.py:212`; `app/chamados/models.py:674`, `:713-716`; `app/ponto/services/cartorio.py:1672`; `app/ponto/tests/test_o168_guarda_csf_no_evento.py:104-106` |
   | `out['csf_com_conversa_preservado']` | Exclusivamente o incremento do helper encontrado neste circuito | Os quatro testes protegidos; retorno de `out` ao chamador | [nao verificado]
   | Evidência | `app/ponto/services/cartorio.py:963` | `app/ponto/tests/test_o168_guarda_csf_no_evento.py:121`, `:135`, `:147`, `:161`; `app/ponto/services/cartorio.py:2068-2071` |
   | `disputa_origem__perguntas` | Não é coluna nova: são relações reversas de `DisputaSupervisao.chamado` e `PerguntaDisputa.disputa`. Emissão cria esses vínculos | Prefetch do helper; juiz CSF percorre ambas |
   | Evidência | `app/chamados/models.py:1750-1755`, `:2417-2420`; `app/chamados/services/disputa_emissao.py:587-592`, `:837-845` | `app/ponto/services/cartorio.py:959`; `app/ponto/supra_juiz.py:543-545` |
   | `perguntas_que_resolvem` | Relação reversa de `PerguntaDisputa.chamado_destino`; nascimento/reuso e costura podem definir ou trocar o destino | Carregadores do cartório/supra e juiz CSF |
   | Evidência | `app/chamados/models.py:3386-3392`, `:2983-3018`; `app/chamados/services/fio.py:926-929` | `app/ponto/services/cartorio.py:127-128`; `app/ponto/management/commands/supra_juiz.py:190-192`; `app/ponto/supra_juiz.py:547-549` |

   **O insumo decisivo de CSF é somente `respondida_em`.** O juiz não consulta texto, `validada_em`, `materializada_em`, `via_resolucao`, booleano `conversa` ou histórico de decisões (`app/ponto/supra_juiz.py:539-550`).

   Escrevem esse carimbo: resposta humana, resposta automática, reparo pela trilha e carimbo com resposta. Reabertura para correção pode zerá-lo, preservando a resposta anterior no histórico, que CSF não lê (`app/chamados/services/disputa_emissao.py:1467-1469`, `:1652-1655`, `:1674-1676`, `:1593-1598`, `:1630-1632`; `app/chamados/models.py:3131-3153`; `app/ponto/supra_juiz.py:543-549`). Efeito dessa reabertura nos casos fornecidos: **[não verificado]**.

   As atribuições temporárias de `status_local` durante a simulação do pós-estado são restauradas; não constituem outro escritor persistente (`app/chamados/models.py:355-371`). Censo completo de todos os leitores de tela/relatório: **[não verificado]**.

4. **Os contadores do supra e do cartório não têm a mesma unidade operacional; 44 não equivalem aos seis fechamentos.**

   No cartório, cada passagem recusada pelo juiz incrementa o contador, sem conjunto de PKs vistos e antes de descobrir se a porta fecharia. Quem foi preservado continua em `vivos`, podendo ser contado novamente em outro sítio ou passada (`app/ponto/services/cartorio.py:961-965`, `:1575`, `:1593`, `:1602`, `:1050`).

   No supra, `CSF_COM_CONVERSA_PRESERVADO` cresce por chamado visitado no ramo da lavra `cobranca_indevida` com código reconhecido. A seleção é diferente da união das avaliações dos quatro sítios do cartório (`app/ponto/management/commands/supra_juiz.py:249-262`).

   O dado informa seis fechamentos com conversa no main — cinco na empresa 2 e um na 3 —, zero na cópia e 44 incrementos de preservação — 34 + 8 + 2. Não fornece a decomposição dos 44 por chamado e avaliação (`diff_sombra.txt:1-2`, anexo fornecido). Logo, quantos chamados distintos esses incrementos representam e quantos realmente seriam fechados sem a guarda: **[não verificado]**.

   O rótulo **“preservados/impacto” não descreve fielmente essa soma de recusas**. Usá-la como número de chamados distintos para o limite percentual mistura unidades, contrariando “rótulo diz o que a conta faz; contador == universo” (`app/ponto/services/cartorio.py:963`; `diff_sombra.txt:9-14`; `CLAUDE.md:35-36`).

   Há ainda avaliações potencialmente omitidas: a releitura pós-emissão usa `_conv`, mas não incorpora seu contador CSF ao `out` principal. O resumo de `processar_cartorio` também não imprime esse campo (`app/ponto/services/cartorio.py:1114-1126`; `app/ponto/management/commands/processar_cartorio.py:66-70`).

5. **Prefetch: até dois SELECTs adicionais por invocação fria; N+1 permanece no supra.**

   Para cada um dos quatro sítios, uma lista não vazia e sem cache pode exigir **um SELECT de disputas + um SELECT de perguntas**. Lista vazia retorna antes; sem disputas não há segundo nível; objetos já carregados podem reaproveitar cache. Trata-se de previsão estrutural, não medição SQL (`app/ponto/services/cartorio.py:956-959`; `app/chamados/models.py:1750-1755`, `:2417-2420`).

   Esse limite é por **invocação/lista**, não duas consultas para o sítio durante toda a rodada: há laços por célula e segunda passada. Sobreposição das listas pode reutilizar os mesmos objetos carregados (`app/ponto/services/cartorio.py:1037-1050`, `:1382`, `:1568-1602`). A relação direta já vem prefetchada no carregador do cartório (`app/ponto/services/cartorio.py:127-128`).

   No supra, o carregador prefetcha somente `perguntas_que_resolvem`; CSF também percorre origem e perguntas por disputa. Permanece, portanto, consulta por chamado para origem e por disputa para perguntas, até eventual retorno antecipado (`app/ponto/management/commands/supra_juiz.py:190-192`; `app/ponto/supra_juiz.py:543-549`). Contagens SQL efetivamente executadas e cumprimento dos tetos: **[não verificado]**.

6. **A lei citada não autoriza uma exclusão geral de FIO-DUP.**

   O literal reproduzido diz: “o cartorio nao fecha cobranca sem furo de chamado que tem conversa”; não contém exceção para FIO-DUP. A exclusão aparece na justificativa escrita pela própria obra (`app/ponto/services/cartorio.py:941-947`).

   Deduplicar enquanto outro chamado continua cobrando um furo existente pode estar fora da premissa “sem furo”. Entretanto, o gate aceita `premissa_regularizada` **antes** de examinar deduplicação: o nome FIO-DUP não garante que todo fechamento dali preserve um furo ainda existente (`app/chamados/models.py:739-750`).

   FIO-DUP e FIO-DUP-C8 não passam pelo helper. Sua isenção lê **texto das perguntas dirigidas**, enquanto CSF lê **carimbo nas perguntas dirigidas e de origem**. São proteções diferentes; conversa apenas na origem não é coberta por essa isenção (`app/ponto/services/cartorio.py:1666-1680`, `:1690-1695`; `app/ponto/supra_juiz.py:543-549`). Ocorrência dessa combinação no dado: **[não verificado]**.

7. **Os testes têm controles úteis, mas não comprovam toda a lei nem a cascata.** [nao verificado]

   Há pares com/sem conversa para os quatro sítios, pela porta real, com recarga do estado final. Isso sustenta a expectativa estática de que retirar individualmente cada guarda faça seu caso falhar; execução dos quatro mutantes: **[não verificado]** (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:100-161`).

   A lacuna concreta está no adiado: a fixture cria perguntas sem `celula` e sem `data_evento`; E2 exige `celula` não nula. Assim, o teste não percorre a cascata real E2 → fechamento da disputa → S88. A pergunta sem data também pode ser desconsiderada pelo gate por competência indecidível (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:80-88`; `app/chamados/reconciliador.py:628-638`; `app/chamados/models.py:364-365`).

   Todos os cenários plantam dia inteiramente batido, e a mesma pergunta integra origem e destino. Faltam provas separadas para dia sem trabalho, marco satisfeito com outro furo restante, conversa apenas na origem/destino e fechador posterior ao cartório. A idempotência não exige estabilidade do contador CSF (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:62-66`, `:80-88`, `:109-175`).

8. **Classes históricas: resultado desta leitura.**

   | Classe | Resultado |
   |---|---|
   | Segundo juiz | O helper reutiliza CSF; não copia sua decisão. FIO mantém predicado de proteção diferente, conforme achado 6 (`app/ponto/services/cartorio.py:955-962`, `:1666-1670`). |
   | Segundo escritor fora da porta | O trecho novo conserva wrapper e escritor existentes. Os desvios encontrados também passam pela porta: falta-lhes a guarda, não a porta (`app/ponto/services/cartorio.py:965`; `app/chamados/signals.py:43`; `app/chamados/models.py:655`). |
   | Relógio fora da borda | O helper novo não lê relógio; as duas ocorrências deslocadas são conversões de timestamps explícitos (`app/ponto/services/cartorio.py:953-967`, `:1234`, `:1387`). |
   | Derivação paralela | Não aparece nova derivação de conversa no helper; a divergência entre texto/FIO e carimbo/CSF permanece (`app/ponto/services/cartorio.py:962`, `:1670`; `app/ponto/supra_juiz.py:545-549`). |
   | RED sem mordida | Há pares comportamentais, mas a cascata E2/S88 escapa à fixture; mutação executada por sítio **[não verificado]** (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:80-88`, `:109-161`; `app/chamados/reconciliador.py:632`). |
   | Ordem de trava | A extração mantém o caminho central de aquisição de travas. A leitura/prefetch da guarda ocorre antes de entrar nessa porta; ausência de corrida concorrente **[não verificado]** (`app/ponto/services/cartorio.py:959-965`; `app/chamados/models.py:651-652`, `:703-713`). |
   | Pouso antes do pré-requisito | A cópia ainda configura supra com `--executar`; o anexo não mede `CSF_RETRATADO`. Remoção posterior da flag, pouso e pré-requisito zero **[não verificado]** (`app/config/crons.py:599`; `diff_sombra.txt:1-2`). |
   | Campo com dois significados | O contador tem unidades diferentes entre chamadores. “Conversa” em CSF significa carimbo de resposta, não o booleano homônimo (`app/ponto/services/cartorio.py:963`; `app/ponto/management/commands/supra_juiz.py:262`; `app/ponto/supra_juiz.py:543-549`; `app/chamados/models.py:2777-2803`). |

9. **55→52 e catraca temporal: ajustes coerentes com a extração, sem afrouxamento demonstrado nesses dois selos.**

   Os quatro pontos físicos de transição viraram uma chamada física no helper; S33 passa a identificá-la, enquanto FIO-DUP/FIO-DUP-C8 continuam cadastrados. O selo segue procurando escritores não cadastrados e exige exceções vazias. Portanto, a redução é consequência da extração escolhida; não demonstra redução de cobertura das quatro rotas de negócio (`diff.txt`, trecho de `e4b_c12_sitios.json`; `app/chamados/tests/e4b_c12_sitios.json:259-280`; `app/chamados/tests/test_e4b_c12_selo.py:22-36`; `app/ponto/services/cartorio.py:965`).

   A catraca apenas registra o deslocamento das mesmas duas expressões de `localdate(timestamp)`. A igualdade integral das ocorrências continua exigida; não houve aumento de tolerância nesse diff (`app/core/tests/test_medida_turno_do_par.py:319-321`, `:546-550`; `app/ponto/services/cartorio.py:1234`, `:1387`).

10. **Limites da verificação.** RED/GREEN executados, mutantes individuais, custo SQL medido, reprodução dos desvios, decomposição dos 44, conservação depois dos receivers/outbox, `CSF_RETRATADO=0`, implantação e identidade Git independente do hash/pai informado: **[não verificado]**. O anexo demonstra somente os resultados declarados das duas rodadas de `julgar_celula`; seu próprio cabeçalho delimita esse ato em rollback (`diff_sombra.txt:1-2`, `:16-20`).

## 12/11 15:27 · sessao 5516d069f335 · - · - · origem=Ronald (tmux)

1. **O selo O168 não prova a cascata indireta E2→S88.** A fixture cria perguntas sem `celula` e sem `data_evento` (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:80`–88; campos nulos em `app/chamados/models.py:2415`, `:2424`). O E2 exige `celula__isnull=False` (`app/chamados/reconciliador.py:628`–638), logo não carimba nem fecha aquelas disputas. Já o caminho real fecha disputa em `app/chamados/reconciliador.py:609`–615 e o S88 chama `reconciliar` sem `pode_executar_csf` (`app/chamados/signals.py:29`–43).

2. **O teste do “adiado” pode fechar no controle sem exercitar esse pré-requisito.** A pergunta viva sem data aciona o adiamento (`app/ponto/services/cartorio.py:1418`, `:1562`–1565), mas o gate desconsidera competência indecidível (`app/chamados/models.py:364`–365). Assim, o controle fechar não demonstra que o E2 tenha resolvido pergunta datada real. O texto promete “lavrar→E2→reconciliar” (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:14`), mas a fixture não cobre essa sequência.

3. **Mordida dos quatro sítios: plausível por análise estática; execução dos mutantes [não verificado].** Há pares com e sem conversa para B53b (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:109`–121), marco aceso (`:126`–135), adiado (`:138`–147) e inexistente (`:152`–161), com estado final recarregado (`:104`–106). Os ramos correspondentes estão em `app/ponto/services/cartorio.py:1602`, `:1568`, `:1040`, `:1581`. Porém o arquivo não implementa mutação individual: somente afirma que cada retirada fica vermelha (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:16`–17). Não rodei testes.

4. **A cobertura é estreita.** Todos os casos plantam dia inteiramente batido (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:62`–66); não há caso específico de dia sem trabalho nem marco satisfeito com outro furo restante. A mesma pergunta pertence à origem e ao destino (`:80`–88), logo não há prova separada das duas relações lidas pelo juiz (`app/ponto/supra_juiz.py:543`–549). Idempotência compara estado, veredito e quantidade de perguntas, mas não exige contador estável (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:166`–175).

5. **55→52 é consolidação de chamadas físicas, sem afrouxamento demonstrado do selo global.** S33 identifica o único fechamento no novo helper (`app/chamados/tests/e4b_c12_sitios.json:259`–264; `app/ponto/services/cartorio.py:965`), enquanto FIO-DUP continua nos S37/S38 (`app/chamados/tests/e4b_c12_sitios.json:267`–280). O selo varre toda a produção e reprova também escritores não cadastrados (`app/chamados/tests/test_e4b_c12_selo.py:22`–32), mantendo exceções vazias (`:34`–36). O manifesto é resolvido por função e texto, não pelo número antigo da linha (`app/chamados/tests/e4b_c12_verificar.py:84`–115). É consequência da extração escolhida; a extração não era a única implementação possível da cura.

6. **A catraca temporal só registra deslocamento.** As duas expressões continuam conversões de timestamps explícitos (`app/ponto/services/cartorio.py:1234`, `:1387`), registradas agora em `app/core/tests/test_medida_turno_do_par.py:319`–321. A igualdade integral das ocorrências segue exigida (`:546`–550); não há relaxamento do teto no diff fornecido.

7. **Classes estruturais no trecho novo:** o helper importa e chama o juiz existente (`app/ponto/services/cartorio.py:955`, `:962`), conserva a porta real (`:954`, `:965`) e não introduz relógio ou `select_for_update` (`:953`–967). O fechamento continua adquirindo a ordem central de trava (`app/chamados/models.py:651`–652, `:703`–713; ordem implementada em `:2667`, `:2676`, `:2689`, `:2693`). Ausência de corrida entre prefetch/guarda e aquisição dessas travas: **[não verificado]**. Pouso e execução do supra após pré-requisito: **[não verificado]**; os dados fornecidos não demonstram `CSF_RETRATADO=0`.

## 12/11 15:27 · sessao 6dbf9ebf2159 · - · - · origem=Ronald (tmux)

1. **O contador do cartório conta recusas por avaliação, não chamados distintos.** O incremento ocorre em cada passagem pelo helper, sem conjunto de PKs vistos: `app/ponto/services/cartorio.py:959-964`. Chamado preservado continua em `vivos`, pois só quem fechou é removido: `app/ponto/services/cartorio.py:1575`, `app/ponto/services/cartorio.py:1593`. Assim, pode ser contado em RECONCILIA-MARCO e novamente em B5.3b: `app/ponto/services/cartorio.py:1568`, `app/ponto/services/cartorio.py:1602`; também pode reaparecer na segunda passada: `app/ponto/services/cartorio.py:1050`. Os 44 informados não demonstram 44 chamados distintos salvos; decomposição efetiva por PK/sítio [não verificado].

2. **No supra, seleção e unidade operacional diferem.** O contador nasce apenas quando a lavra é `cobranca_indevida` e contém código reconhecido, percorrendo uma vez os chamados de `vivos`: `app/ponto/management/commands/supra_juiz.py:249-262`. É contador por chamado visitado nesse ramo da varredura; o cartório conta cada recusa em qualquer dos quatro sítios. Portanto, o mesmo rótulo não garante a mesma conta: `app/ponto/services/cartorio.py:946`, `app/ponto/management/commands/supra_juiz.py:262`.

3. **Há avaliação potencialmente omitida do total devolvido.** A releitura pós-emissão usa outro dicionário `_conv`; copia códigos e alguns contadores, mas não `csf_com_conversa_preservado`: `app/ponto/services/cartorio.py:1114-1126`.

4. **Escritores/leitores do contador.** Escritor de cartório: helper em `app/ponto/services/cartorio.py:963`; leitores nos quatro testes protegidos: `app/ponto/tests/test_o168_guarda_csf_no_evento.py:121`, `:135`, `:147`, `:161`. `contadores()` não declara essa chave: `app/ponto/services/cartorio.py:1983`. O comando cria `out` por lote e o passa a cada colaborador, mas não imprime esse contador no resumo: `app/ponto/management/commands/processar_cartorio.py:43`, `:65-70`. No supra, escritor: `app/ponto/management/commands/supra_juiz.py:262`; leitor para impressão: `app/ponto/management/commands/supra_juiz.py:297-300`.

5. **O juiz lê somente relações e `respondida_em`.** Percorre `ch.disputa_origem.all()`, cada `d.perguntas.all()` e `ch.perguntas_que_resolvem.all()`; qualquer carimbo não nulo impede execução: `app/ponto/supra_juiz.py:539-550`. Não consulta texto, `conversa`, validação, materialização, via ou histórico `DecisaoPergunta`: `app/ponto/supra_juiz.py:543-549`. Uma pergunta de fato nasce com `conversa=False`; pergunta de conversa nasce pela outra porta com `True`: `app/chamados/models.py:2777-2803`. Logo “conversa” no rótulo da guarda não significa o campo booleano `conversa`.

6. **Origem e perguntas são relações reversas, não campos novos escritos pelo diff.** `disputa_origem` decorre de `DisputaSupervisao.chamado`: `app/chamados/models.py:1750-1755`; `perguntas`, de `PerguntaDisputa.disputa`: `app/chamados/models.py:2417-2420`; destino direto, de `PerguntaDisputa.chamado_destino`: `app/chamados/models.py:3386-3392`. Emissão escreve o vínculo da disputa em `app/chamados/services/disputa_emissao.py:587-592`, e pede nascimento da pergunta com disputa/destino em `app/chamados/services/disputa_emissao.py:837-845`; a inserção efetiva está em `app/chamados/models.py:3015-3018`. Reuso pode trocar destino em `app/chamados/models.py:2983-3007`; reapontamento explícito escreve por `app/chamados/services/fio.py:926-929`.

7. **Quem escreve o insumo da guarda.** Resposta humana: `app/chamados/services/disputa_emissao.py:1467-1469`; resposta automática: `app/chamados/services/disputa_emissao.py:1652-1655`; reparo pela trilha: `app/chamados/services/disputa_emissao.py:1674-1676`; carimbo com resposta: `app/chamados/models.py:3131-3153`. Reabrir para correção zera resposta/carimbo atual, preservando foto no histórico: `app/chamados/services/disputa_emissao.py:1593-1598`, `:1630-1632`. Ressuscitação também pode zerar `respondida_em`: `app/chamados/models.py:1567`. O juiz não lê a foto histórica: `app/ponto/supra_juiz.py:543-549`; ocorrência/efeito em produção [não verificado].

8. **`status_local` mantém a porta existente.** O helper chama `transicionar_com_pauta(..., ato=x.reconciliar)`: `app/ponto/services/cartorio.py:965`; o wrapper executa esse ato e devolve o resultado: `app/chamados/services/fecho_runtime.py:157-169`. `reconciliar` obtém decisão e chama `_gravar_estado`: `app/chamados/models.py:651-656`. A atribuição persistida é `self.status_local = novo`, seguida do `save`: `app/chamados/models.py:228-229`, `:296`. Nascimento escreve estado por `abrir`/`criar`, com exemplo do segundo em `app/chamados/models.py:1096`; atribuições em `:356` e `:371` são simulação temporária em memória. Leitores relevantes: seleção de vivos do supra `app/ponto/management/commands/supra_juiz.py:212`; motor/gates `app/chamados/models.py:674`, `:713-716`; a auditoria não enumerou todos os consumidores de tela/relatório [não verificado].

9. **Prefetch: custo previsto de até dois SELECTs adicionais por chamada fria não vazia do helper.** O caminho tem duas relações reversas (`disputas`, depois `perguntas`): `app/chamados/models.py:1750-1755`, `:2417-2420`; o helper prefetcha ambas de uma vez: `app/ponto/services/cartorio.py:959`. Lista vazia retorna antes: `app/ponto/services/cartorio.py:956-958`; sem disputas, não há segundo nível para carregar; cache já preenchido pode evitar consultas. Destino direto já vem prefetchado pelo carregador: `app/ponto/services/cartorio.py:127-128`. Quantidade SQL executada, cache em cada sítio e limites de teste [não verificado]; isto é leitura estrutural, não medição.

10. **N+1 permanece no supra.** Seu carregador prefetcha somente `perguntas_que_resolvem`: `app/ponto/management/commands/supra_juiz.py:190-192`; o juiz consulta também origem e perguntas por disputa: `app/ponto/supra_juiz.py:543-544`. Portanto, nesse caminho há consultas por chamado para origem e por disputa para perguntas, até eventual retorno antecipado: `app/ponto/supra_juiz.py:545-546`. Nos quatro sítios novos, o helper evita esse padrão para as relações que ele carrega: `app/ponto/services/cartorio.py:959`.

## 12/11 15:27 · sessao 18e177eeea5c · - · - · origem=Ronald (tmux)

Transições comuns da célula futura:

| Estado | Comportamento atual |
|---|---|
| Não existe | Suplemento não cria: seleciona somente células existentes, `origem='gerada'`, de amanhã ao fim da janela; `app/ponto/services/cadastro_futuro.py:18-31`. A chamada direta de regeneração cria ausentes em dias cobertos; `app/ponto/portas/celula.py:280-284`. |
| Gerada | Se trabalha, geradora ou DNA divergir, reescreve e registra `dna_anterior`, `regenerada_em`, `regeneracoes`; `app/ponto/portas/celula.py:285-299`. |
| Editada | Suplemento exclui na seleção; regeneração também só altera origem gerada; `app/ponto/services/cadastro_futuro.py:23`; `app/ponto/portas/celula.py:285`. |
| Carimbada com lâmpada | Não existe guarda por julgamento/carimbo: pode mudar DNA e `dna.lampadas`, conservando ATA/veredito/impressão/julgada_em; `app/ponto/portas/celula.py:285-299`; montagem das lâmpadas em `app/ponto/management/commands/gerar_celulas.py:79-104`. Rejulgamento pula futuras; `app/ponto/portas/celula.py:448-454`. |
| Regenerada | Continua `origem='gerada'`, não é estado exclusivo; recebe o mesmo tratamento e novo rastro se mudar; `app/escala/models.py:1690-1691`; `app/ponto/portas/celula.py:285-299`. |

Fora da vigência, a regeneração limpa a futura gerada cuja geradora é o vínculo encerrado, sem outro vínculo cobrindo e sem lavra: `trabalha=False`, geradora nula, DNA sem marcos e rastro de regeneração; `app/ponto/portas/celula.py:244-259`. Havendo sucessor cobrindo, conserva até o evento que regenere pelo sucessor; `app/ponto/portas/celula.py:247`; `app/colaboradores/services/vinculo.py:330-335`.

Inativa sem fim é recusada pela porta; `app/ponto/portas/celula.py:146-155`. O vigia consulta `vinculo_do_dia` e separa esse caso em C6; `app/ponto/services/vigia_cadastro_futuro.py:31-45`. A lista C6 inclui vínculos inativos sem fim, sem exigir referência de célula; `app/escala/services/cadastro_realidade.py:320-328`.

| Evento | Futuro existente | Passado |
|---|---|---|
| Salvar estrutura: município do posto | `atualizar_posto` e `definir_municipio_posto` disparam evento atomicamente; `app/colaboradores/services/estrutura.py:65-96`. Evento percorre vínculos do posto e suplementa futuras; `app/colaboradores/services/vinculo.py:931-941`. | Só hoje, se vínculo cobre hoje; `app/colaboradores/services/vinculo.py:933-937`. |
| Salvar estrutura: outros campos | Empresa/praca/posto ativo/opera_em_feriado apenas salvam, sem suplemento explícito; `app/colaboradores/services/estrutura.py:18-22`; `:36-40`; `:75-85`. Necessidade para cada campo depende de seu efeito; anterioridade da omissão [nao verificado]. | Sem escrita direta em célula nessas funções; mesmas referências. |
| Vínculo novo / reativar pelo executor | Executor atômico, cria ou reativa e suplementa desde início; `app/colaboradores/services/vinculo.py:169`; `:253-291`; `:330-335`. | Regenera início..hoje, sem extensão; `app/colaboradores/services/vinculo.py:319-325`. |
| Encerrar por `fechar_vigencia` | Suplementa apenas após `min(fim anterior,fim novo)+1`, evitando tocar dias cujo cadastro não mudou; `app/colaboradores/services/vinculo.py:640-650`. | Nenhum suplemento passado; `app/colaboradores/services/vinculo.py:640-641`. |
| Encerrar por `encerrar_vinculo` | Save dispara signal e suplemento; `app/colaboradores/services/vinculo.py:821-824`; `app/escala/signals.py:80-83`. | Signal regenera desde menor início até hoje; `app/escala/signals.py:60-75`. |
| Reativar por `reabrir_vigencia_impossivel` | `update` de ativa/fim, sem signal ou suplemento; `app/colaboradores/services/vinculo.py:715-732`. Omissão verificada; introdução pela c4b [nao verificado]. | Também não regenera; mesma referência. |
| Alterar âncora | Signal suplementa futuras quando muda para valor não nulo; `app/escala/signals.py:111-134`. | Desde início da competência corrente até hoje; `app/escala/signals.py:119-127`. |
| Trocar template via `salvar_tipo` | Todos os vínculos do tipo, inclusive encerrados; `app/escala/services/cadastro_tipo.py:368-376`. | Só vínculos ativos, janela anterior até hoje; `app/escala/services/cadastro_tipo.py:337-353`. |
| Template pelo admin | Só suplemento futuro; `app/escala/services/cadastro_tipo.py:380-387`. | Preserva; mesma referência. |
| Trocar calendário do vínculo | Sincroniza foto futura e regenera existentes; `app/colaboradores/services/vinculo.py:455-460`; `app/escala/folgas.py:616-640`. | Esse suplemento preserva; `app/escala/folgas.py:632`. |
| Trabalha em feriado | Atualiza flag e suplementa futuras; `app/colaboradores/services/vinculo.py:869-892`. | Janela anterior..hoje pela regeneração; `app/colaboradores/services/vinculo.py:878-887`. |
| ESCALA-AUTO | Continua chamando regeneração direta, que cria ausentes e estende horizonte; `app/escala/services/escala_auto_executor.py:104-105`; `:143-144`; `app/ponto/portas/celula.py:187-192`; `:280-284`. | Aplicação exige início futuro; `app/escala/services/escala_auto_executor.py:99-100`. |
| Cron `gerar_celulas` | Mede antes, regenera hoje e gera ausentes; conserva futuras existentes; `app/ponto/management/commands/gerar_celulas.py:337-344`; `app/ponto/portas/celula.py:892-897`. | **PROIBIDO** pela regra estrita do pedido: cria ausente passada e enriquece DNA nulo fora de `regenerar_celulas_vinculo`; `app/ponto/portas/celula.py:875-888`. |

Outros **PROIBIDO** condicionais: restauração de snapshot apaga vínculos e deixa `SET_NULL` alterar células passadas geradas fora da porta; recusa apenas referências editadas. `app/colaboradores/services/vinculo.py:1003-1007`; FK em `app/escala/models.py:1692-1694`. Desfazer ajuste remove vínculo de modo equivalente; `app/colaboradores/services/vinculo.py:1077-1080`. Ocorrência real de referências passadas nesses caminhos [nao verificado].

Lâmpadas: a lei exige evento carimbar/julgar e consumidor ler a lâmpada; `CLAUDE.md:170-173`. O leitor apresenta a hora prevista da ATA; `app/escala/services/leitor_celula.py:114-123`. Outro consumidor cruza marco do DNA com luz da ATA; `app/chamados/juizes.py:1337-1345`. Portanto o estado futuro já carimbado precisa fechar a transição entre DNA novo e ATA antiga. O cartório também recusa data futura; `app/ponto/services/cartorio.py:2025`. Se a lei existente não define invalidação sem julgamento antecipado: **lei: qual estado da ATA/lâmpada futura conserva coerência após o cadastro mudar**.

Janela: suplemento usa união das competências das empresas ativas, enquanto a autoridade individual respeita o corte da empresa; `app/ponto/services/cadastro_futuro.py:16-18`; `app/ponto/management/commands/gerar_celulas.py:281-291`; `app/ponto/janelas.py:60-63`. Existência em produção de cortes diferentes [nao verificado].

Não localizei identificadores `redefinir_config` e `trocar_tipo`; caminho exato [nao verificado].

## 12/11 15:26 · sessao 3e23a5af05bc · - · - · origem=Ronald (tmux)

1. Os quatro sítios diretos foram encaminhados ao helper: adiados `app/ponto/services/cartorio.py:1040`; RECONCILIA-MARCO `:1568`; MARCO-INEXISTENTE `:1581`; B5.3b (`COBRANCA_EM_DIA_SEM_TRABALHO`/`COBRANCA_SEM_FURO`) `:1594-1602`. O helper consulta o juiz antes da porta (`:962-965`).

2. **Há bypass indireto dentro do próprio ato do cartório.** Antes do helper dos adiados, `julgar_colab` chama E2 (`app/ponto/services/cartorio.py:1035-1040`). E2 fecha disputas aprovadas pelo C7 (`app/chamados/reconciliador.py:609-615`). `DisputaSupervisao.fechar` salva o fechamento (`app/chamados/models.py:1933-1937`), disparando `disputa_fechada_fecha_chamado_pai`, que chama `transicionar_com_pauta(..., ato=ch.reconciliar)` sem consultar `pode_executar_csf` (`app/chamados/signals.py:19-43`). A porta consulta premissa e pergunta viva, não a guarda de qualquer conversa (`app/chamados/models.py:672-694`). Portanto a guarda local dos quatro sítios não constitui guarda de toda a cascata. Reprodução e ocorrência nesse caminho em produção: [não verificado].

3. **Há fechadores posteriores à porta do cartório sem a guarda.** O receiver posterior da batida chama `reconciliar_chamados_colab` (`app/ponto/signals.py:206-215`), que executa E2 e `retratar_premissa_morta_colab` (`app/chamados/reconciliador.py:1433-1442`). Este percorre chamados vivos e chama `_retratar` quando a premissa morre (`:1454-1465`). `_retratar` chama a porta sem `pode_executar_csf` (`:519-524`); sua retenção cobre irmã respondida **sem consumo**, não toda pergunta que já foi respondida (`:438-455`).

4. O consumidor de `EventoCelula` chama três fechadores adicionais: absolvidos pela célula (`app/chamados/reconciliador.py:1369`), premissa morta (`:1373`) e lastro (`:1383-1384`). Os absolvidos chamam `_retratar` (`:997-1010`), conforme a célula lavrada sem acusação e sem marco pendente (`:1019-1032`). O lastro percorre `batida_ausente` na fila e chama a porta quando todos os marcos cobrados acenderam, sem a guarda de conversa (`app/ponto/services/lastro.py:388-403`). Assim o DIFF do ato isolado `julgar_celula` não prova conservação no consumidor posterior; execução real desse consumidor nos 44 casos: [não verificado].

5. Outros fechadores do mesmo tipo, todos sem helper/juiz de conversa:

   - `detectar_ausencias`: exige marcos acesos antes de chamar `_fechar_chamado` (`app/ponto/management/commands/detectar_ausencias.py:404-413`); o método chama diretamente a porta (`:845-849`).
   - Sync/API: busca a cobrança viva do dia, exige `marcos_acesos_do_dia` e chama a porta (`app/ponto/services/entrada_sincronizada.py:29-32`).
   - Ausência/cobertura: chamadas diretas da porta (`app/ponto/management/commands/detectar_ausencias.py:821-849`).
   - Materialização: `_reconciliar_destino_e_clones` fecha destino e clones após verificar somente irmãs sem consumo (`app/chamados/services/materializacao.py:740-777`). Aqui há ato de materialização/decisão humana, portanto equipará-lo integralmente ao fechamento automático proibido exige análise contextual [não verificado].

6. **FIO-DUP e FIO-DUP-C8 continuam fora.** O primeiro exige que todas as perguntas dirigidas sejam duplicatas e fecha via porta (`app/ponto/services/cartorio.py:1674-1683`); o segundo pode fechar outros vivos do dia (`:1690-1695`). A isenção desses blocos verifica texto em `perguntas_que_resolvem`, não `respondida_em` nem perguntas da disputa de origem (`:1666-1670`). Já o juiz legal verifica `respondida_em` nas duas relações (`app/ponto/supra_juiz.py:543-549`). Logo as guardas têm extensão diferente.

7. A exclusão FIO-DUP está declarada pelo comentário da obra (`app/ponto/services/cartorio.py:946-947`), mas a lei literal reproduzida não contém essa exceção (`:941-942`). Além disso o gate aceita primeiro `premissa_regularizada` e só depois tenta deduplicação (`app/chamados/models.py:739-750`); chamar o sítio de FIO-DUP não garante, por si, que todo fechamento dali ocorreu enquanto o furo persistia. A hipótese de pergunta respondida na origem e somente perguntas dirigidas sem texto passa pela isenção estreita do FIO e seria barrada pelo juiz de conversa (`cartorio.py:1674-1680`; `supra_juiz.py:543-549`). Ocorrência dessa combinação no dado raspado: [não verificado].

8. No supra, o comando consulta o mesmo juiz antes de retratar (`app/ponto/management/commands/supra_juiz.py:257-263`), e `CSF_RETRATADO` só cresce quando `_retratar` retorna verdadeiro (`:284-290`). A porta central `reconciliar` segue sem consulta ao juiz de conversa (`app/chamados/models.py:642-694`), permitindo que os demais chamadores acima fechem o que o supra preservaria.

Não executei testes, não consultei banco/logs e não escrevi arquivos. Prova dinâmica dos bypasses e universo completo de todas as variantes de fechamento no repositório: [não verificado]. [nao verificado]

## 12/11 15:26 · sessao dfd10d4e98f4 · - · - · origem=Ronald (tmux)

Censo e semântica das portas de estrutura, somente por leitura:

| Evento | Alcance e comportamento no código |
|---|---|
| Atualizar estrutura/Posto | `atualizar_posto` e `definir_municipio_posto` são atômicos e disparam evento somente quando muda `municipio_id`; demais campos de Posto não disparam regeneração por essa porta. `app/colaboradores/services/estrutura.py:53`, `:65`, `:88`. |
| Município do posto | Para cada vínculo do posto, regenera HOJE se o vínculo o cobre; futuro usa o suplemento para todos os vínculos do posto; passado anterior a hoje fica fora. `app/colaboradores/services/vinculo.py:931`, `:933`, `:938`. |
| Salvar template pela porta comum | Passado: ativos do template, desde o máximo entre início do vínculo e competência anterior, até hoje, pela porta de regeneração. Futuro: todos os vínculos do template, inclusive encerrados, pelo suplemento. `app/escala/services/cadastro_tipo.py:337`, `:351`, `:373`. |
| Salvar template no Django admin | Salva template e propaga somente futuras, dentro do mesmo atomic. `app/escala/services/cadastro_tipo.py:380`. |
| Salvar calendário de folga | Grava definição e sincroniza vínculos vinculados ao calendário no mesmo atomic. `app/escala/folgas.py:578`, `:597`. |
| Sincronizar calendário | Seleciona células futuras existentes, inclusive editadas para determinar meses; restringe à cobertura do vínculo; reconcilia fotos futuras mensais sem sobrescrever folgas manuais; relavra somente células `origem='gerada'` pelo suplemento. `app/escala/folgas.py:619`, `:622`, `:625`, `:638`; preservação manual em `:187`, `:194`. |
| Lançar/alterar/remover foto de folga | O caminho comum relavra passado até hoje pela porta e acrescenta futuras existentes; eventual rejulgamento de mês segue a mesma porta. `app/escala/folgas.py:283`, `:290`, `:293`, `:298`. |
| Trocar calendário do vínculo | Salva o FK e sincroniza futuras quando o calendário mudou, no mesmo atomic. `app/colaboradores/services/vinculo.py:454`. |
| Feriado salvo | `pre_save` captura data antiga pela autoridade de consultas; `post_save` passa datas antiga/nova ao suplemento; `raw=True` não propaga. `app/core/signals.py:14`, `:22`; `app/core/feriados.py:86`. |
| Feriado apagado | `post_delete` propaga a data apagada. `app/core/signals.py:30`. |
| Suplemento por feriado | Normaliza texto ISO usando `isinstance`; limita a datas estritamente futuras na janela corrente; só células geradas; pergunta `vinculo_do_dia` alimentado com a geradora; se a autoridade cala, não escolhe sucessor. `app/ponto/services/cadastro_futuro.py:48`, `:49`, `:53`, `:59`, `:62`. |

O suplemento comum não cria célula ausente: trava e seleciona somente futuras `origem='gerada'` existentes na janela do gerador, depois chama `regenerar_celulas_vinculo` um dia por chamada com horizonte desligado. Editada fica fora; regenerada continua elegível porque sua origem permanece `gerada`. `app/ponto/services/cadastro_futuro.py:16`, `:22`, `:27`, `:30`; `app/escala/models.py:1709`.

**Problema de desenho demonstrável estaticamente:** template e município chamam o suplemento com cada vínculo, mas o suplemento seleciona todas as células geradas da pessoa sem restringir à geradora. A porta troca o carimbo sempre que aquele vínculo cobre a data, sem perguntar `vinculo_do_dia`. Vínculos sobrepostos do mesmo template são percorridos por início decrescente; portanto o mais antigo pode sobrescrever por último o carimbo do mais novo. Incidência em produção: [nao verificado]. `app/escala/services/cadastro_tipo.py:373`; `app/escala/models.py:1003`; `app/ponto/services/cadastro_futuro.py:22`, `:26`; `app/ponto/portas/celula.py:277`, `:285`, `:294`; autoridade preserva carimbo em `app/escala/alimentacao.py:279`.

**Vigia e candidata (b):** lê a autoridade existente; geradora carimbada inativa sem fim passa para `na_lista_do_admin_c6`, sem alterar a juíza ou a célula. A lista C6 usa o predicado existente e já enumera todos os vínculos inativos sem fim. O N exigido pelo aval antes da construção não está no dado fornecido: [nao verificado]. `app/ponto/services/vigia_cadastro_futuro.py:31`, `:40`; `app/escala/services/cadastro_realidade.py:320`; `app/escala/alimentacao.py:199`.

**Encerramento sem sucessor:** o novo ramo limpa `escala_geradora`, põe `trabalha=False`, recompõe DNA e grava os três campos de denúncia; `montar_dna` mantém `vinculo_id` do vínculo encerrado dentro do blob. O vigia conta esse estado em `sem_geradora`, fora dos dois contadores esperado-zero. Isso é comportamento comprovado, mas sua conformidade semântica não decorre literalmente do aval fornecido: lei: representação de célula futura existente cujo vínculo terminou sem sucessor, distinguindo proveniência e autoridade. `app/ponto/portas/celula.py:244`, `:249`, `:252`; `app/ponto/management/commands/gerar_celulas.py:95`; `app/ponto/services/vigia_cadastro_futuro.py:34`.

**Campos e semântica:**

- `data_inicio`/`data_fim` são inclusivos na cobertura e no ramo carimbado de `vinculo_do_dia`; não confundir `data_fim` com primeiro dia do sucessor. `app/escala/alimentacao.py:214`, `:282`.
- Vínculo sem vigência declarada = inativo sem fim; a cobertura o recusa, mas a juíza carimbada o devolve se não há violação explícita de início/fim, conforme candidata (b). `app/escala/alimentacao.py:189`, `:214`, `:279`.
- Janela do suplemento é a união das competências correntes das empresas ativas; o piso efetivo é amanhã ou `desde`, o que for maior. `app/ponto/management/commands/gerar_celulas.py:288`; `app/ponto/services/cadastro_futuro.py:18`.
- `dna.marcos` guarda HH:MM; `dna.lampadas[*].instante` guarda data e hora local naive. Nos turnos que cruzam meia-noite, horas anteriores à entrada ficam em D+1; a célula continua identificada pelo dia D do turno. `app/ponto/management/commands/gerar_celulas.py:36`, `:55`, `:67`, `:81`.
- `trabalha`, `escala_geradora`, `dna`, `dna_anterior`, `regenerada_em`, `regeneracoes`: escritor da alteração é a porta; leitores incluem autoridade de vínculo, métodos do vínculo e vigia. `app/ponto/portas/celula.py:294`; `app/escala/alimentacao.py:269`; `app/escala/models.py:1094`, `:1124`, `:1189`, `:1235`; `app/ponto/services/vigia_cadastro_futuro.py:28`, `:48`.
- `dna.lampadas` não é o carimbo de julgamento. Regeneração futura altera DNA e preserva `julgada_em`, `impressao`, `ata`, `veredito` e `status_dia`; o rabo da porta pula julgamento de datas futuras. Estado futura já carimbada permanece com julgamento anterior; regra de frescura aplicável a esse estado: [nao verificado]. `app/ponto/portas/celula.py:298`, `:448`, `:453`; campos de julgamento em `app/escala/models.py:1738`.
- O leitor de marcos ainda cai no template quando DNA sem marcos; leitores de pausas e extra declarada distinguem célula existente sem declaração e respondem vazio/zero. São contratos distintos existentes. `app/escala/models.py:1124`, `:1133`, `:1202`, `:1234`.

Não encontrei artefato local do diff específico ou dos hashes da obra pelo censo de nomes; atribuir cada mudança ao intervalo histórico exato permanece [nao verificado]. O RELATO documenta etapas anteriores, mas não substitui o diff: `app/docs/RELATO.md:39`, `:50`, `:60`.

## 12/11 15:25 · sessao 7f4b1508da7d · E3-10-CURA-NO-ESCRITOR · sonda · origem=Code

**PARA antes de qualquer apply:** zerar `respondida_em` deslocaria **142 perguntas do histórico para pendentes**; **col331/#22394 permaneceria no histórico**, porque sua disputa está fechada. A partição exige simultaneamente ausência de carimbo e disputa aberta. [app/chamados/services/pergunta_no_app.py:71](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/pergunta_no_app.py:71) [nao verificado: estado de produção fornecido]

Não executei a sonda de produção: não há acesso a banco/logs, e `sonda_app.py` não foi localizado na cópia. Conferi a partição em memória usando a função extraída do código e os IDs fornecidos, sem criar, editar ou apagar arquivos. [nao verificado: execução local] [nao verificado]

**(1) Saída integral da sonda recebida**, reproduzida abaixo; não representa uma nova execução. [nao verificado]

```text
sonda_leitura: sonda_app.py [019c12f6f89be675] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

agora 2026-11-12T15:22:30.115249-03:00
universo: perguntas 143 colabs 17 erros 0
col60 alvo=1 pendentes=0 historico=1 fora_do_app=0 disputa_aberta=1 | app_total pend=1 hist=21 badge={'count': 1, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[31436] pks_fora=[]
col191 alvo=17 pendentes=0 historico=17 fora_do_app=0 disputa_aberta=17 | app_total pend=43 hist=17 badge={'count': 43, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[29167, 29168, 29169, 31724, 31725, 31726, 31727, 31728, 31729, 31730, 31731, 32909, 32910, 33593, 33594, 34182, 34183] pks_fora=[]
col204 alvo=4 pendentes=0 historico=4 fora_do_app=0 disputa_aberta=4 | app_total pend=14 hist=14 badge={'count': 14, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[31584, 31585, 31586, 31587] pks_fora=[]
col224 alvo=9 pendentes=0 historico=9 fora_do_app=0 disputa_aberta=9 | app_total pend=44 hist=9 badge={'count': 44, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[32291, 32292, 32293, 32294, 32295, 32296, 33925, 33926, 33927] pks_fora=[]
col310 alvo=6 pendentes=0 historico=6 fora_do_app=0 disputa_aberta=6 | app_total pend=18 hist=6 badge={'count': 18, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[32243, 32244, 32245, 32246, 34141, 34142] pks_fora=[]
col331 alvo=1 pendentes=0 historico=1 fora_do_app=0 disputa_aberta=0 | app_total pend=30 hist=5 badge={'count': 30, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[22394] pks_fora=[]
col349 alvo=10 pendentes=0 historico=10 fora_do_app=0 disputa_aberta=10 | app_total pend=54 hist=12 badge={'count': 54, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[29502, 29503, 32131, 32132, 32133, 32134, 32135, 32136, 33749, 33750] pks_fora=[]
col437 alvo=16 pendentes=0 historico=16 fora_do_app=0 disputa_aberta=16 | app_total pend=20 hist=18 badge={'count': 20, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[29565, 32224, 32225, 32226, 32227, 32228, 32229, 32230, 32231, 32232, 32233, 32234, 32235, 32236, 32237, 32238] pks_fora=[]
col446 alvo=16 pendentes=0 historico=16 fora_do_app=0 disputa_aberta=16 | app_total pend=8 hist=33 badge={'count': 8, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[29872, 32589, 32590, 32591, 32592, 32593, 32594, 32595, 32596, 32597, 32598, 32599, 32600, 32601, 33146, 33147] pks_fora=[]
col592 alvo=11 pendentes=0 historico=11 fora_do_app=0 disputa_aberta=11 | app_total pend=49 hist=13 badge={'count': 49, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[28357, 31708, 31709, 31710, 31711, 31712, 31713, 31714, 31715, 31716, 31717] pks_fora=[]
col607 alvo=3 pendentes=0 historico=3 fora_do_app=0 disputa_aberta=3 | app_total pend=270 hist=3 badge={'count': 270, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[29114, 31334, 31335] pks_fora=[]
col782 alvo=12 pendentes=0 historico=12 fora_do_app=0 disputa_aberta=12 | app_total pend=36 hist=19 badge={'count': 36, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[32279, 32280, 32281, 32282, 32283, 32284, 32285, 32286, 32287, 32288, 32289, 32290] pks_fora=[]
col789 alvo=6 pendentes=0 historico=6 fora_do_app=0 disputa_aberta=6 | app_total pend=22 hist=31 badge={'count': 22, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[31839, 31840, 31841, 31842, 33549, 33550] pks_fora=[]
col869 alvo=13 pendentes=0 historico=13 fora_do_app=0 disputa_aberta=13 | app_total pend=32 hist=13 badge={'count': 32, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[28443, 29270, 31863, 31864, 31865, 31866, 31867, 31868, 31869, 31870, 31871, 31872, 31873] pks_fora=[]
col893 alvo=12 pendentes=0 historico=12 fora_do_app=0 disputa_aberta=12 | app_total pend=136 hist=14 badge={'count': 136, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[32459, 32460, 32461, 32462, 32463, 32464, 32465, 32466, 33577, 33578, 34171, 34172] pks_fora=[]
col900 alvo=2 pendentes=0 historico=2 fora_do_app=0 disputa_aberta=2 | app_total pend=85 hist=2 badge={'count': 85, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[38251, 38252] pks_fora=[]
col903 alvo=4 pendentes=0 historico=4 fora_do_app=0 disputa_aberta=4 | app_total pend=14 hist=5 badge={'count': 14, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[31472, 31473, 31476, 31477] pks_fora=[]
TOTAL {'pend': 0, 'hist': 143, 'fora': 0, 'disputa_aberta': 142}
total_erros 0
```

**(2) Tabela por colaborador.** As quatro contagens centrais referem-se ao alvo das 143; as duas últimas reproduzem o total do app informado pela sonda. Todas correspondem ao instante fornecido, **12/11/2026 às 15:22:30 −03:00**. [nao verificado]

| Colab | Alvo | Alvo pendentes | Alvo histórico | Alvo fora do app | Alvo disputa aberta | App total pendentes | App total histórico | Fonte |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| col60 | 1 | 0 | 1 | 0 | 1 | 1 | 21 | [nao verificado] |
| col191 | 17 | 0 | 17 | 0 | 17 | 43 | 17 | [nao verificado] |
| col204 | 4 | 0 | 4 | 0 | 4 | 14 | 14 | [nao verificado] |
| col224 | 9 | 0 | 9 | 0 | 9 | 44 | 9 | [nao verificado] |
| col310 | 6 | 0 | 6 | 0 | 6 | 18 | 6 | [nao verificado] |
| col331 | 1 | 0 | 1 | 0 | 0 | 30 | 5 | [nao verificado] |
| col349 | 10 | 0 | 10 | 0 | 10 | 54 | 12 | [nao verificado] |
| col437 | 16 | 0 | 16 | 0 | 16 | 20 | 18 | [nao verificado] |
| col446 | 16 | 0 | 16 | 0 | 16 | 8 | 33 | [nao verificado] |
| col592 | 11 | 0 | 11 | 0 | 11 | 49 | 13 | [nao verificado] |
| col607 | 3 | 0 | 3 | 0 | 3 | 270 | 3 | [nao verificado] |
| col782 | 12 | 0 | 12 | 0 | 12 | 36 | 19 | [nao verificado] |
| col789 | 6 | 0 | 6 | 0 | 6 | 22 | 31 | [nao verificado] |
| col869 | 13 | 0 | 13 | 0 | 13 | 32 | 13 | [nao verificado] |
| col893 | 12 | 0 | 12 | 0 | 12 | 136 | 14 | [nao verificado] |
| col900 | 2 | 0 | 2 | 0 | 2 | 85 | 2 | [nao verificado] |
| col903 | 4 | 0 | 4 | 0 | 4 | 14 | 5 | [nao verificado] |
| **TOTAL alvo** | **143** | **0** | **143** | **0** | **142** | — | — | [nao verificado] |

**(3) `total_erros = 0`**, conforme a saída recebida. Não há erro reportado que acione a condição “medida NÃO conclusiva”; a execução de produção não foi verificada independentemente. [nao verificado]

**(4) Efeito de zerar somente `respondida_em`, mantendo os demais campos e fatos do instante:**

- `respondida_sem_texto` exige carimbo presente e texto vazio após `strip()`. Portanto, estar nesse predicado não significa estar em pendentes: a consulta do app testa o carimbo diretamente. [app/chamados/juizes.py:1510](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1510) [app/chamados/juizes.py:1515](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1515) [app/chamados/services/pergunta_no_app.py:71](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/pergunta_no_app.py:71)
- `perguntas_do_app` seleciona pelo colaborador, por `q_vivas()` e por `pergunta_viva`. Como o texto desses alvos já é vazio, o ramo de vitalidade por resposta com texto já era falso; zerar o carimbo não altera esse resultado, preservados os demais fatos. [app/chamados/services/pergunta_no_app.py:45](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/pergunta_no_app.py:45) [app/chamados/models.py:3349](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3349) [app/chamados/models.py:3360](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3360) [app/chamados/juizes.py:1407](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1407)
- A regra efetiva é `p.respondida_em is None and p.disputa.fechada_em is None`: verdadeira → pendentes; falsa → histórico. [app/chamados/services/pergunta_no_app.py:69](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/pergunta_no_app.py:69)
- `projecao_do_app` usa essa consulta e calcula a contagem e o badge a partir das pendentes; a mudança alcança a apresentação do app. [app/chamados/services/pergunta_no_app.py:90](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/pergunta_no_app.py:90) [app/chamados/services/pergunta_no_app.py:98](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/pergunta_no_app.py:98)

A **lista exata das 142 que mudariam** é a união de todos os `pks_historico` publicados na saída acima, **excluindo somente col331/#22394**. Abaixo, os mesmos IDs explicitados como mudança **histórico → pendentes**, pela regra citada. [app/chamados/services/pergunta_no_app.py:71](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/pergunta_no_app.py:71) [nao verificado: IDs e estado de produção fornecidos]

```text
col60: #31436
col191: #29167 #29168 #29169 #31724 #31725 #31726 #31727 #31728 #31729 #31730 #31731 #32909 #32910 #33593 #33594 #34182 #34183
col204: #31584 #31585 #31586 #31587
col224: #32291 #32292 #32293 #32294 #32295 #32296 #33925 #33926 #33927
col310: #32243 #32244 #32245 #32246 #34141 #34142
col349: #29502 #29503 #32131 #32132 #32133 #32134 #32135 #32136 #33749 #33750
col437: #29565 #32224 #32225 #32226 #32227 #32228 #32229 #32230 #32231 #32232 #32233 #32234 #32235 #32236 #32237 #32238
col446: #29872 #32589 #32590 #32591 #32592 #32593 #32594 #32595 #32596 #32597 #32598 #32599 #32600 #32601 #33146 #33147
col592: #28357 #31708 #31709 #31710 #31711 #31712 #31713 #31714 #31715 #31716 #31717
col607: #29114 #31334 #31335
col782: #32279 #32280 #32281 #32282 #32283 #32284 #32285 #32286 #32287 #32288 #32289 #32290
col789: #31839 #31840 #31841 #31842 #33549 #33550
col869: #28443 #29270 #31863 #31864 #31865 #31866 #31867 #31868 #31869 #31870 #31871 #31872 #31873
col893: #32459 #32460 #32461 #32462 #32463 #32464 #32465 #32466 #33577 #33578 #34171 #34172
col900: #38251 #38252
col903: #31472 #31473 #31476 #31477
```

**col331/#22394 permanece no histórico**: zerar o carimbo não satisfaz a condição de disputa aberta. Entre os 143 alvos, a partição passaria de **0 pendentes / 143 histórico / 0 fora** para **142 pendentes / 1 histórico / 0 fora**. Assim, o requisito do aval “apply só se o app não muda” não é atendido. [app/chamados/services/pergunta_no_app.py:71](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/pergunta_no_app.py:71) [nao verificado: estado de produção e aval fornecidos]

## 12/11 15:25 · sessao 09e311b52f21 · - · - · origem=Ronald (tmux)

A numeração abaixo corresponde à ordem do anexo. O resultado da suíte c4b e a comparação histórica exata com eb7bdd1a5 não foram verificados; a classificação descreve o mecanismo presente na cópia. [nao verificado]

| Falha | Classe / célula da matriz | Tratamento presente em c4b |
|---|---|---|
| 1. `Cancelar_na_caixa...` | Feriado salvo × futura gerada: incompatibilidade da representação da data recebida pelo evento. | **Adaptação local por `isinstance`**: ISO string vira `date.fromisoformat` antes da comparação; não muda o `DateField` nem o payload do signal. Elimina a operação `date < str`, mas mantém duas representações admitidas na entrada. `app/ponto/services/cadastro_futuro.py:42-49`; `app/core/signals.py:27`; `app/core/models.py:12`. |
| 2. `a_tela_de_feriado_novo...` | A mesma classe da falha 1; o erro ocorre no preparo do cenário, não no botão. [nao verificado] | A mesma conversão local de `app/ponto/services/cadastro_futuro.py:48`; um teste específico agora envia a string ISO pelo evento: `app/ponto/tests/test_cadastro_futuro_c4.py:146-152`. |
| 3. `Excluir_confirmado...` | A mesma classe da falha 1; o erro precede o clique de exclusão. [nao verificado] | A mesma conversão local: `app/ponto/services/cadastro_futuro.py:48`. O evento de apagar passa `instance.data` pelo mesmo serviço: `app/core/signals.py:30-33`. |
| 4. `Salvar_do_editar...` | A mesma classe da falha 1; futura gerada na data antiga/nova do feriado. | A mesma conversão local: `app/ponto/services/cadastro_futuro.py:48`. A data antiga é capturada no `pre_save` e as duas datas seguem no `post_save`: `app/core/signals.py:14-27`. |
| 5. `dna_congelado_sobrevive_edicao_de_template` | Template alterado × futura com carimbo de vínculo encerrado versus sucessor; conservação do passado. | **Mudou o cenário do teste**, não apenas o código de produção: agora primeiro exige que não reste futura nomeando o encerrado (`:163-164`), chama explicitamente o suplemento para o sucessor (`:165`) e fabrica um evento perdido por `TipoEscala.objects...update(...)` (`:169-170`). O `assertGreater` continua (`:177`); o teste depois verifica a porta `salvar_tipo` e a conservação do passado (`:183-201`). Portanto não é `skip`/allowlist/exceção, mas substituição da testemunha que produzia a divergência. `app/escala/tests/test_celula_dia.py:159-201`. |
| 6. `template_derivado_trilha_e_passado_intocado` | Encerrar com sucessor × futura anterior ao início do efeito, ainda coberta pelo vínculo antigo. | **Cura na origem do alcance**: `fechar_vigencia` começa no primeiro dia cuja cobertura mudou, `min(fim_antigo,fim_novo)+1`, em vez de regenerar toda futura do vínculo. O selo original continua comparando a foto de tudo anterior ao início. `app/colaboradores/services/vinculo.py:640-650`; `app/escala/tests/test_escala_auto_executor.py:69-73`. RED específico preserva até DNA antigo de um dia ainda coberto: `app/ponto/tests/test_cadastro_futuro_c4.py:133-144`. |
| 7. `ninguem_consulta_o_modelo_por_fora` | Feriado salvo × seleção das datas antiga/nova: propriedade da consulta ao cadastro. | **Centralização na origem autorizada da consulta**: signal chama `core.feriados.data_gravada`; a consulta reside nesse módulo. Não há allowlist nova no contrato, que continua excluindo apenas dono e CRUD. `app/core/signals.py:18-19`; `app/core/feriados.py:86-93`; `app/core/tests/test_contract_feriado.py:27-30,58-64`. |
| 8. `o_censo_que_IMPEDE...` | Cron vigia × futura carimbada: geradora usada como resposta local, fora do juiz. | **Cura do uso na origem**: vigia entrega geradora e célula a `vinculo_do_dia`, testa ausência e usa a resposta `ec`; não compara mais a resposta com o carimbo. `app/ponto/services/vigia_cadastro_futuro.py:28-49`. `TOMADORES` continua vazio, sem adicionar o vigia: `app/escala/tests/test_contract_juiz_escala.py:69-83`. |
| 9. `sitio_novo_e_lista_velha...` | Cron × célula gerada de hoje: escolha do vínculo por `ativa=True`. | **Cura na origem**: `_regenerar_hoje_existentes` usa `vinculos_do_dia_carregado`, depois a mesma porta da célula. `app/ponto/management/commands/gerar_celulas.py:303-316`. Não foi adicionado à lista `HOJE`; o teste continua exigindo exatamente o sítio artificial na lista de novos. `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:417-422`. |
| 10. `a_arvore_nao_tem_sitio_novo...` | Mesma classe da falha 9; é a fiscalização da árvore, em vez da mordida do instrumento. | Mesma cura de `app/ponto/management/commands/gerar_celulas.py:303-316`; contrato permanece exigindo listas novas/velhas vazias: `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:364-369`. |
| 11. `vulture_zero` | Porta do admin de template × assinatura do callback; questão de instrumento, sem transição de célula própria. | **Convenção `_form` para parâmetro não usado**, na própria assinatura; não elimina o parâmetro, pois o callback o recebe. Não há inclusão de `form` no ignore: continuam os seis nomes de framework já enumerados. `app/escala/admin.py:11-14`; `app/core/tests/test_contract_esmeril.py:13-15,26-29`. |

Observação para a matriz: as quatro falhas do feriado não fornecem quatro provas independentes de transição; o anexo mostra o mesmo `TypeError` durante `Feriado.objects.create`. Não demonstram o comportamento dos cliques após o preparo. [nao verificado]

O tratamento do vínculo inativo sem fim está explícito e não altera o juiz: o vigia o conta em `na_lista_do_admin_c6`, deixando os dois contadores de estrutura em zero; o juiz continua aceitando a geradora carimbada dentro das datas, enquanto `tem_vigencia_declarada` a recusa para regeneração. Isso materializa a candidata (b), não a candidata (a). `app/ponto/services/vigia_cadastro_futuro.py:31-45`; `app/escala/alimentacao.py:189-196,270-286`; `app/ponto/tests/test_cadastro_futuro_c4.py:183-200`.

O N de vínculos em produção que deveria ter sido publicado antes da construção não aparece no dado raspado e não foi medido nesta leitura. [nao verificado] [nao verificado]

## 12/11 15:24 · sessao 7fb82434cf82 · - · - · origem=Ronald (tmux)

Censo PRE-cura salvo em `docs/lastro_c18_censo.txt`. Único arquivo alterado por este agente; referências preservam as linhas da base c17.

## 12/11 15:23 · sessao 7fb82434cf82 · - · - · origem=Ronald (tmux)

Censo somente leitura, referências da base c17 antes da edição. Nenhuma afirmação sobre frota/banco/log foi verificada.

**Escritores da grade e unidade**

| Sítio | Unidade e operação |
|---|---|
| `escala/utils.py:621`, `:662` | `_match_marcos`: instante absoluto → lista HH:MM; dois pontos originais de perda da data. |
| `escala/utils.py:762`, `:1008`, `:1037`, `:1826` | Inicializam dia sem órfãs: lista vazia. |
| `escala/utils.py:776`, `:788` | Builder simples recebe e publica HH:MM de `_match_marcos`. |
| `escala/utils.py:799` | Sobra: `locs[_k][0]` absoluto → HH:MM anexada ao dia. |
| `escala/utils.py:803`, `:1862` | Ordenam a lista HH:MM; lista paralela deverá acompanhar a mesma permutação. |
| `escala/utils.py:963`, `:967` | Clip da janela: apagam órfãs de dia futuro/anterior ao piso. |
| `escala/utils.py:1046` | Legacy remapeia/copía `sd['orfas']` HH:MM. |
| `escala/utils.py:1156` | Overlay de folga apaga a lista. |
| `escala/utils.py:1234`, `:1277`, `:1282` | Falta, ausência integral e justificativa apagam a lista. |
| `escala/utils.py:1462`, `:1591`, `:1782` | Mapa intermediário por data: inicialização, dia intermitente vazio, lista HH:MM retornada pelo alinhamento. |
| `escala/utils.py:1725`, `:1746`, `:1757` | Builder por turno recebe HH:MM; ponta sem coluna e intervalo excedente acrescentam `celula['hora']` HH:MM. Aqui também é necessário preservar o instante antes da perda. |
| `escala/utils.py:1835` | Publica cópia HH:MM do mapa na linha do dia. |
| `escala/utils.py:1856`, `:1858` | Sobras sem slot/anônimas: instante `locs[_k][0]` → HH:MM. |
| `escala/utils.py:1993`, `:1998`, `:2021`, `:2064` | Reconciliação cria aliases `_orf_t`, `_orf_saida`, `_lista_orf` e remove uma ocorrência HH:MM da lista original. A remoção correspondente do instante é obrigatória. |

**Escritores de ata e lavra/relavra**

| Sítio | Unidade e operação |
|---|---|
| `ponto/services/cartorio.py:729`, `:733`, `:736` | `ata_do_dia`, produtor persistido: `n_orfas` é contagem; `orfas` é lista HH:MM convertida por `str`. |
| `ponto/services/cartorio.py:1247`, `:1289` | Caminho NB chama o mesmo `ata_do_dia`, aplica dispensa e encaminha para lavra. |
| `ponto/services/cartorio.py:1432`, `:1433` | Ata de julgamento em memória chama o mesmo produtor com dispensa/teto. |
| `ponto/services/cartorio.py:1847`, `:1849` | Lavra/relavra normal chama o mesmo produtor, dispensa/teto e `lavrar_veredito(ata=...)`. |
| `escala/utils.py:1912`, `:1920` | **Outro escritor de objeto `.ata`, temporário em memória:** cópia da célula usada pelo juiz de realizado recebe `lampadas` e `orfas` HH:MM. Não persiste. Deve ser citado no censo; não é o escritor persistido do cartório. |

**Leitores principais**

| Sítio | Unidade lida/usada |
|---|---|
| `escala/services/leitor_celula.py:180`, `:187`, `:190` | `_orfas_da_ata`: lista HH:MM; ata sem horas vira `['?'] * n_orfas`, preservando contagem/ignorância. |
| `escala/services/leitor_celula.py:390`, `:420` | Inicializa lista vazia e transporta as horas da ata para o dia. |
| `escala/utils.py:427` | Chama `_match_marcos`, mas descarta o retorno `_orfas`. |
| `escala/utils.py:1113` | `montar_realizado`: repassa cópia HH:MM, sem reinterpretar. |
| `ponto/services/espelho.py:699`, `:702`, `:854` | Recebe/publica HH:MM como `orfas_grade`; ausência de montagem → lista vazia. |
| `colaboradores/services/calendario.py:380`, `:381`, `:622` | Converte HH:MM em conjunto por dia/mês e consulta para desenhar a batida. |
| `ponto/services/cartorio.py:590`, `:619`, `:727` | `lampadas_do_dia`: HH:MM → minutos do dia para lâmpadas; não lê instante de órfã. |
| `ponto/services/cartorio.py:733`, `:836`, `:1422` | Contagem/presença: `len(orfas)` e `n_orfas`. |
| `relatorios/pdf_espelho.py:477`, `:478`, `:482`, `:485` | S1 c17 inventa `(data da linha, HH:MM)` e casa com timestamp formatado; chave a eliminar. |
| `relatorios/pdf_espelho.py:174`, `:178`, `:522`, `:574` | Recebem conjunto de instantes: identidade da entrada espúria, supressão de turno e marca na batida. |
| `templates/ponto/espelho.html:151`, `:240` | Exibem diretamente cada HH:MM de `orfas_grade`. Nenhum outro acesso de template a essas chaves apareceu na busca. |

**Demais leitores HH:MM**

- `relatorios/cartao_pela_celula.py:288`: conjunto de horas da ata por data.
- `ponto/juiz_batida.py:161`, `:164`: copia as horas para o contrato de períodos.
- `ponto/services/cluster_espurio.py:29`, `:31`, `:34`: valida lista, converte HH:MM em minutos e detecta rajada; `:206`, `:210` usa as horas em texto/contexto.
- `ponto/services/vigia_pareador_celula.py:74`, `:78`, `:111`: conjunto HH:MM da ata, compara a hora da batida.
- `ponto/services/esmeril_espelho.py:125`: HH:MM → minutos/faixas de meia hora.
- `ponto/turnos.py:2667`: HH:MM → minutos do dia para cobertura parcial.
- `ponto/propositor.py:39`, `:53`, `:55`: valida e ordena horas reais em minutos.
- `escala/orfa_classe.py:58`, `:74`: lista HH:MM, classificação/rajadas.
- `ponto/management/commands/propor_reorganizacao.py:104`: acrescenta horas órfãs às marcas reais.
- `ponto/management/commands/tripwire_celulas.py:165`, `:264`: horas da ata → minutos, comparadas aos marcos.
- `chamados/services/materializacao.py:180`: acrescenta horas órfãs às marcas usadas pelo propositor.

**Demais leitores de contagem/presença**

- `escala/services/furos_diarios.py:22`: `len(orfas)`.
- `ponto/status_dia.py:188`: contagem de batidas = casadas + órfãs.
- `ponto/supra_juiz.py:379`: presença de fato; `:514`, `:517`, `:520`: presença, contagem e texto das horas.
- `folha/causas.py:43`, `:100`: presença de órfãs nos dias úteis/folgas.
- `ponto/services/bordas_realizado.py:49` e `ponto/services/simular_dia.py:12`: presença.
- `ponto/management/commands/detectar_par_relampago.py:140`: soma contagens.
- `chamados/juizes.py:510`: presença de `orfas` ou `n_orfas`.
- `chamados/detector_anomalias.py:212`: recebe `_orfas` de `_match_marcos`; retorno não é consumido naquele sítio.

**Juiz existente para o contador**

Use o inverso de `folha/export.py:597::competencia_fechada_para_celula`, passando `empresa_id`, `mes`, `ano` e `colaborador_id`. A autoridade documenta aberta como lista de fontes vazia; compõe exportação vigente e holerite publicado (`folha/export.py:614`, `:618`). O enumerador existente `_competencias_exportadas` já usa esse juiz e converte competência em janela (`ponto/management/commands/gerar_celulas.py:147`, `:152`, `:173`, `:176`).

Não deduzir abertura por `FechamentoMensal.status`: a própria guarda rejeita essa equivalência (`ponto/management/commands/gerar_celulas.py:131`).

## 12/11 15:14 · sessao 87d57a6fc2f3 · E3-LEITORES · sonda · origem=Code

Referências `DADO RASPADO:n` contam desde `sonda_leitura` como linha 1, preservando a linha vazia seguinte.

A saída é conclusiva para a coorte atual: `total_erros=0` e `sonda_coorte_atual_conclusiva=1`. (`DADO RASPADO:25`, `DADO RASPADO:26`)

| Medida | Resultado | Fonte |
|---|---:|---|
| Coorte atual de vencidos em COBRANCA | 11 | `DADO RASPADO:5` |
| É 12? | Não (`mesma_quantidade_12=0`) | `DADO RASPADO:6` |
| Chamados medidos | 11 | `DADO RASPADO:7` |
| Visíveis ao colaborador no MAIN | 0 | `DADO RASPADO:8` |
| Visíveis com a b4 | 3 | `DADO RASPADO:9` |
| Passam a aparecer com a b4 | 3 | `DADO RASPADO:10` |
| Chamados de colaboradores isentos | 8 | `DADO RASPADO:11` |
| Isentos visíveis com a b4 | 1 | `DADO RASPADO:12` |
| Isentos que passam a aparecer | 1 | `DADO RASPADO:13` |

| Chamado | MAIN | b4 | Isento | Fonte |
|---|---:|---:|---:|---|
| col41/#3057 | 0 | 0 | 1 | `DADO RASPADO:14` |
| col41/#3403 | 0 | 0 | 1 | `DADO RASPADO:15` |
| col440/#11658 | 0 | 0 | 1 | `DADO RASPADO:16` |
| col291/#11935 | 0 | 1 | 1 | `DADO RASPADO:17` |
| col331/#16444 | 0 | 1 | 0 | `DADO RASPADO:18` |
| col368/#16621 | 0 | 1 | 0 | `DADO RASPADO:19` |
| col216/#23030 | 0 | 0 | 1 | `DADO RASPADO:20` |
| col427/#23031 | 0 | 0 | 0 | `DADO RASPADO:21` |
| col257/#23037 | 0 | 0 | 1 | `DADO RASPADO:22` |
| col225/#23049 | 0 | 0 | 1 | `DADO RASPADO:23` |
| col142/#27206 | 0 | 0 | 1 | `DADO RASPADO:24` |

A saída **não prova identidade com os 12 históricos**: `identidade_com_12_historicos_verificada=0`. (`DADO RASPADO:27`)

Na cópia, a lista do colaborador exige `silenciado_ate__isnull=True`. ([janela_colab.py:73](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/janela_colab.py:73))

Para a rodada 2, os números acima são os da saída fornecida. Uma nova execução da sonda sobre `wt-e3-b6-114947`/`c70b7511c` nesta sessão: [nao verificado].

## 12/11 14:56 · sessao a3191175b1ce · AUDITORIA-DE-TRAVAS · leitura · origem=Code

**A v6 não fecha: há um bloqueante novo no universo da semente e permanece o bloqueante APP/O168 do v5.** A exclusão por estado continua autorizada na semente, e a precedência de APP é reinterpretada sem novo aval demonstrado. (`contrato_item3_v6.txt:17`, `:21`, `:26`, `:30`; `app/docs/BACKLOG.md:1916`; novo aval: [nao verificado])

Conto linhas físicas dos anexos. “FECHADO” abaixo significa fechado contratualmente; a implementação desta cópia ainda usa marcador e coluna. (`bin/hook_stop_fila1.py:209–227`, `:250–252`; `bin/disjuntor.py:1634`)

**1. Achados da leitura v5**

| Achado v5 | Situação na v6 |
|---|---|
| G1 inclusivo; repetição do token genérico `<!--` | **FECHADO.** Termina antes da primeira linha de comentário; unicidade aplica-se somente aos quatro delimitadores nomeados. (`contrato_item3_v6.txt:6–7`) |
| G2 sem inserção/lápide | **FECHADO por incorporação expressa da migração v5.** A cópia ainda contém o marcador antigo. (`contrato_item3_v6.txt:5`; `app/docs/BACKLOG.md:8`) |
| G3 sem `GERADO:FIM`; manual começando em 2371 | **FECHADO por incorporação da partição v5.** Nesta cópia, BATI-02 termina em 2370 e SELO-MORDE começa em 2371. (`contrato_item3_v6.txt:5`; `app/docs/BACKLOG.md:2370–2371`) |
| Autoria por zonas; BATIDA obsoleta | **FECHADO por incorporação da solução v5:** G3 pode ser substituída pelo render. (`contrato_item3_v6.txt:5`; `item3_contrato_v5.r`, seção 1, linha física: [nao verificado]) |
| Preservação byte a byte versus recomposição textual | **FECHADO contratualmente; construção pendente.** O gerador ainda lê texto, usa `rstrip/split/join` e reescreve o arquivo. (`contrato_item3_v6.txt:5`; `bin/gerar_backlog.py:111`, `:127`, `:138–140`) |
| Omissão da mudança de universo do censo | **FECHADO.** A perda da exclusão por coluna/tachado está declarada, inclusive o caso FECHADA presente em `[ORDEM]`. (`contrato_item3_v6.txt:10–14`; `bin/hook_stop_fila1.py:168`, `:180`; `bin/disjuntor.py:1634`) |
| Segundo classificador | **FECHADO.** Permanece `_pertence_fila1`; a igualdade da célula no fabricante não substitui o matcher. (`contrato_item3_v6.txt:4`, `:10`; `bin/disjuntor.py:1688–1695`) |
| TRAVA confundida com veto do censo | **FECHADO.** O contrato distingue aprovação do brief de autorização de construção. (`contrato_item3_v6.txt:14`; `bin/disjuntor.py:1721–1727`) |
| Seis pousadas citadas no v5 | **FECHADO para esses seis IDs.** São explicitamente retirados do restante da ordem. (`contrato_item3_v6.txt:21`; `semente_item3_v6.py:17`, `:61–63`) |
| X1 parcialmente pousada | **FECHADO contratualmente.** A v6 distingue pouso parcial e trabalho restante, mantendo a trava `X1-6-DE-7`. (`contrato_item3_v6.txt:42`; `app/docs/RELATO.md:248`, `:266`) |
| Quatorze faltantes documentais listados no v5 | **FECHADO quanto à representação.** CODEX-2-RAIAS/ALIVIA entram pela cabeça; os outros doze entram pelo restante calculado. (`contrato_item3_v6.txt:20`, `:36`; `semente_item3_v6.py:12`, `:62–63`; `app/docs/BACKLOG.md:772–774`, `:827`, `:887`, `:1213`, `:1831–1835`, `:1850`, `:1865–1866`, `:1986–1987`) |
| Identidades de SAIDA e AUDITORIA-ITEM6-CONTADOR-124 | **ABERTO nesta cópia.** Não há primeiras células correspondentes; os despachos não suprem a identidade exigida, e a semente recusa a ausência. (`app/docs/BACKLOG.md:1–2425`; `app/docs/RELATO.md:9`, `:29`; `contrato_item3_v6.txt:3`; `semente_item3_v6.py:55–57`, `:64–66`, `:82`) |
| Travas LASTRO/F0F1 e ESTACIONADA | **FECHADO contratualmente.** LASTRO recebe pergunta de desenho; F0F1 não recebe ESTACIONADA; esta permanece em BO-DISPUTA. (`contrato_item3_v6.txt:31`, `:33`, `:41`; `semente_item3_v6.py:10–11`, `:16`) |
| E3 obrigada a esperar pouso QUARENTENA | **FECHADO contratualmente:** substituído pelo impedimento relativo ao item #4 aberto. A comprovação externa do aval citado não está no dado. (`contrato_item3_v6.txt:28`; comprovação externa: [nao verificado]) | [nao verificado]
| SONDA/MATRIZ e ausência de CODEX-2-RAIAS | **FECHADO.** CODEX entra na cabeça; o texto registra pouso de CODEX-2-RAIAS mais rebase como condição da MATRIZ. (`contrato_item3_v6.txt:36–38`) | [nao verificado]
| AUDITORIA esperando SONDA; BO estacionada | **FECHADO.** As dependências permanecem explícitas. (`contrato_item3_v6.txt:39`, `:41`; `app/docs/BACKLOG.md:1981`; `app/docs/RELATO.md:373`) |
| APP depois de O168/LASTRO | **ABERTO — B2.** A cabeça ainda coloca O168 antes de APP, contrariando a precedência documental citada pelo próprio contrato. (`contrato_item3_v6.txt:26`, `:30`; `app/docs/BACKLOG.md:1916`) |
| SAIDA sem distinguir dependência de construção | **FECHADO contratualmente:** a v6 declara novo aval para abrir já. Sua existência externa permanece sem comprovação fornecida. (`contrato_item3_v6.txt:25`; comprovação externa: [nao verificado]) |
| Referências erradas de BUG-CADASTRO/MATRIZ | **FECHADO.** As notas passam a identificar por ID. (`contrato_item3_v6.txt:43`; `app/docs/BACKLOG.md:2408`, `:1980`) |
| ESMERIL-SEM-INSTANTE pousada | **ABERTO, incorporado ao problema B1.** Continua entrando como andável porque a linha está aberta, embora o RELATO registre pouso. (`app/docs/BACKLOG.md:1830`; `app/docs/RELATO.md:1005`; `semente_item3_v6.py:17`, `:62`) |
| Reconciliação LASTRO-FOLGA | **ABERTO.** Entra sem trava pelo estado da linha; a v6 não reconcilia sua relação com LASTRO-FAIXA e com a parada documentada. (`app/docs/BACKLOG.md:1812–1813`; `app/docs/RELATO.md:175`; `semente_item3_v6.py:62`) |
| Linhas de plano | **FECHADO quanto ao destino mecânico:** META fica explicitamente fora; ORDEM-ATE-O-ZERO e RAIA-2 entram pelo restante. Isso não comprova autonomia ou conclusão dessas obras. (`contrato_item3_v6.txt:20`, `:22`; `app/docs/BACKLOG.md:1883`, `:1906`, `:2406`; autonomia/conclusão: [nao verificado]) |

**2. Bloqueios de contrato**

**B1 — NOVO: o universo automatizado continua dependendo da coluna para excluir, contrariando o aval de exclusões por ID.** A v6 define entrada como “não `_item_fechado`” e dispensa as fechadas de constarem em `[POUSADAS]`. O predicado vivo decide por expressão na coluna `[3]` ou tachado na descrição `[2]`; a semente elimina essas linhas antes da reconciliação por ID. (`contrato_item3_v6.txt:17`, `:21`; `bin/hook_stop_fila1.py:168`; `semente_item3_v6.py:40–41`)

A declaração de mudança do censo não resolve essa seleção inicial. O construtor teria de presumir equivalência entre FECHADA/tachada e pousada, ou reconciliar os IDs além das seis pousadas prescritas. A violação inversa já tem exemplo documental: ESMERIL-SEM-INSTANTE pousou, mas volta à ordem porque a coluna permanece aberta. Não afirmo que as 208 fechadas estejam incorretamente excluídas: seu pouso individual é [nao verificado]. (`contrato_item3_v6.txt:12`, `:17`, `:21`; `app/docs/BACKLOG.md:1830`; `app/docs/RELATO.md:1005`; `semente_item3_v6.py:62`)

**B2 — HERDADO: APP/O168 continua sem fechar.** A v6 transforma “antes de O168/LASTRO” em “vale para LASTRO” porque O168 já abriu. Isso é alteração da precedência do aval documental, não correção de referência. A autorização adicional para essa alteração não foi fornecida. (`contrato_item3_v6.txt:26`, `:30`; `app/docs/BACKLOG.md:1916`; autorização adicional: [nao verificado])

**NAO BLOQUEIA**

- As identidades ausentes mantêm a semente inexequível nesta cópia, mas não criam duas interpretações: a exigência é identidade única e a resposta prescrita é recusar. Não as conto como novo bloqueante contratual. (`contrato_item3_v6.txt:3`; `semente_item3_v6.py:55–57`, `:82`)
- A fórmula de conservação esquece descontar os três IDs da cabeça fora do bloco: `1039 + 13 + 49 + 5 = 1106`. O script já desconta esse conjunto; a correção da redação é determinada. (`contrato_item3_v6.txt:22`, `:44`, `:46`; `semente_item3_v6.py:72`)
- OBRAS:FIM em “2051 no vivo” não corresponde a esta cópia, onde está em 2042; a obrigação de ancorar por conteúdo elimina a escolha. (`contrato_item3_v6.txt:8`; `app/docs/BACKLOG.md:2042`)
- A referência `test_hook_estado_e_a_coluna.sh:53–59` recorta os casos incompletamente; a substituição semântica está clara e permite ajustar o selo inteiro. (`contrato_item3_v6.txt:15`; `bin/tests/test_hook_estado_e_a_coluna.sh:51–63`, `:80–104`)
- Os sufixos curam unicidade de célula, mas podem produzir `NAO SEI` no matcher preservado. Isso é consequência determinada das regras, não autorização para alterar `_pertence_fila1`. (`contrato_item3_v6.txt:10`, `:23`; `bin/disjuntor.py:1663–1669`, `:1688–1695`)
- A v6 exige selo e mantém referência ao RED; a disponibilidade do commit RED e o conteúdo integral dos casos herdados do v5 não foram comprovados nesta cópia. Isso não autoriza inverter selo e código. (`contrato_item3_v6.txt:15`; disponibilidade/conteúdo integral: [nao verificado]) [nao verificado]

**3. Quem escreve e quem lê hoje e depois da v6**

| Componente | Hoje | Depois da v6 |
|---|---|---|
| Sessão / publicador | Marcador manual; gerador preserva e reescreve fisicamente seu conteúdo. (`app/docs/BACKLOG.md:8`; `bin/gerar_backlog.py:113`, `:119–140`) | Sessão escreve `.ids`; `gerar_backlog` publica a projeção, sem virar segundo decisor da ordem. (`contrato_item3_v6.txt:3`, `:5–7`) |
| Hook: topo | Lê marcador, procura no bloco e veta pela coluna. (`bin/hook_stop_fila1.py:209–227`) | Consulta o leitor único estruturado, mantendo adaptadores para seus consumidores. (`contrato_item3_v6.txt:3–4`, `:15`) |
| Hook: andáveis/parados/travas | Calcula pela tabela e coluna. (`bin/hook_stop_fila1.py:231–273`, `:323–382`) | Precisa migrar também essas funções para IDs/TRAVA; trocar apenas o topo deixa autoridade antiga ativa. (`contrato_item3_v6.txt:3`, `:14–15`; `bin/hook_stop_fila1.py:1298`, `:1400`) |
| Disjuntor, Agent e wrapper | `_fila1_viva` recebe andáveis do hook; ambos usam o mesmo matcher. (`bin/disjuntor.py:1634`, `:1707`, `:2306–2307`) | Migração expressa para `ordem_viva.ids_ordem()`, com matcher intacto. (`contrato_item3_v6.txt:10`) |
| Handoff | Pergunta topo, próximo e travas ao hook. (`bin/handoff_sessao.sh:64`, `:71`, `:294`, `:310`) | Migra pelos adaptadores; seus textos sobre marcador/primeiro aberto precisam acompanhar a mudança. (`contrato_item3_v6.txt:3–4`, `:15`; `bin/handoff_sessao.sh:314–316`) |
| Interrompidas / ESTADO | Topo via hook, publicado pelo medidor. (`bin/interrompidas_estado.py:54`, `:93`; `bin/gerar_estado.py:236–243`) | Migração transitiva pelo mesmo adaptador; não precisam de outro parser da ordem. (`contrato_item3_v6.txt:4`) |
| `backlog_estado` | Escreve coluna; consulta próximo/parados antes e depois. (`bin/backlog_estado.py:56–63`) | Continua escritor de metadado, sem mover a ordem; consultas precisam acompanhar o hook migrado. (`contrato_item3_v6.txt:3`, `:15`) |
| Fabricante | Lê linhas, ordem física e estado do BACKLOG. (`bin/fabricante_alvo.py:238`, `:251–283`) | Deve aplicar a exclusão da fila 1 pela igualdade da célula via fonte estruturada. (`contrato_item3_v6.txt:4`) |

**Não identifiquei leitor adicional do topo que exija outro juiz:** handoff, interrompidas e ESTADO podem acompanhar o hook. Entretanto, a migração precisa alcançar também suas funções de andáveis/parados/travas. (`bin/handoff_sessao.sh:64`, `:294`; `bin/interrompidas_estado.py:93`; `bin/gerar_estado.py:236–243`; `bin/hook_stop_fila1.py:267`, `:273`, `:344`, `:380–382`)

Há um consumidor independente que permanece fora da migração expressa: `alarme_sessao_ociosa` conta “fila 1” diretamente no BACKLOG e usa essa contagem para cobrar ou dispensar trabalho. Ele não lê a ordem/topo; por isso não o conto como bloqueante da ordem única. Já `gerar_backlog_topo` publica um índice numérico de obras O-número, também sem representar a ordem viva. (`bin/alarme_sessao_ociosa.py:89–116`, `:40–45`; `bin/gerar_backlog_topo.py:19`, `:33–40`; `contrato_item3_v6.txt:3–4`)

VEREDITO: NAO FECHA (2 bloqueantes)

## 12/11 14:55 · sessao dc04510a551f · ACERVO-TRANCA-ANTES-DO-SISTEMA · censo · origem=Code

A tranca de acervo **não aprova a folha nem comprova exportação**: `trancar_periodo` escreve `PeriodoFechado` e dispara o tratamento da fila; K7/K4 consultam somente a linha com `reaberto_em IS NULL`. A aprovação individual é uma escrita separada. (`app/ponto/services/fechamento.py:1323`, `app/ponto/services/fechamento.py:1400`, `app/ponto/services/fechamento.py:1470`, `app/ponto/services/fechamento.py:1480`.)

Os dez pares e a ausência futura de aprovação são premissas do enunciado, sem confirmação em banco [nao verificado]. Quantidades de células, perguntas, chamados, disputas, toques humanos, TXT e holerites desses pares não constam do dado fornecido [nao verificado].

Nas tabelas, “independe de FM” significa que o leitor não exige existência nem aprovação de `FechamentoMensal`; a competência é obtida pela janela da empresa, não pelo mês civil. (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:54`, `app/chamados/juizes.py:1062`.)

**Leitores executáveis de K7, K4 e `PeriodoFechado` direto:**

| Porta | Arquivo:linha — função; importação | Classe e efeito da tranca | Dependência de FM |
|---|---|---|---|
| K7 | `app/chamados/juizes.py:1066` — `dias_trancados`; import local em `:1029` | **Chamado/pergunta/disputa:** alimenta C20; usa `fechado_em` para verificar toque humano posterior. (`:1070`, `:1078`.) | Independe de FM. |
| K7 | `app/chamados/services/competencia_trancada.py:83` — `medir`; import local em `:76` | **Chamado/pergunta/disputa:** sem tranca retorna vazio; com tranca seleciona chamados/disputas elegíveis e respostas para decisão do DP. Pergunta não recebe carimbo pela tranca. (`:84`, `:126`, `:147`, `:159`.) | Independe de FM. |
| K7 | `app/ponto/views.py:1027` — `painel_fechamento`; módulo apelidado `_fech_svc`, importado em `:969` | **Tela de fechamento:** fornece `periodo_fechado`; o painel oculta aprovação individual. (`app/templates/ponto/partials/painel_fechamento.html:80`.) | A ocultação alcança linha existente, com leitura disponível e `status != 'aprovado'`. |
| K7 | `app/ponto/views.py:1126` — `fechamento_mensal`; import local `periodo_trancado as _periodo_trancado`, em `:1125` | **Tela de fechamento:** oculta “Fechar período”, mostra “Período fechado” e reabertura; também alimenta a ocultação de reabrir colaborador aprovado. (`app/templates/ponto/fechamento.html:208`, `:221`; `app/templates/ponto/_fech_linha.html:42`.) | Selo/reabertura independem de aprovado; “Fechar período” também exige `pode_aprovar`; reabrir colaborador exige aprovado. |
| K7 | `app/ponto/views.py:1540` — `reabrir_periodo`; módulo `_fech`, import local em `:1539` | **Tela/folha/chamados:** ausência de tranca dá 404; presença permite chamar a reabertura. (`:1541`, `:1543`.) | Acesso independe de aprovado; o serviço só muda status dos FM aprovados. (`app/ponto/services/fechamento.py:1451`.) |
| K4 | `app/chamados/models.py:1045` — `ChamadoColaborador._dia_em_competencia_trancada`; import local em `:1041` | **Chamado/pergunta/disputa:** responde pela competência do chamado, preservando módulos fora da tranca; o renascimento automático é recusado. (`:1042`, `:1010`.) | Independe de FM. |
| K4 | `app/escala/services/furos_diarios.py:77` — `apurar`; import local `competencia_trancada as _trancada`, em `:75` | **Outro — FuroDiario:** deixa de incluir a janela anterior na reapuração/gravação/limpeza; a corrente permanece. (`:68`, `:78`, `:107`.) | Independe de FM. |
| K4 | `app/ponto/services/fechamento.py:1538` — `marcos_da_competencia` | **Outro — marcos/contador:** passa `trancada=True`; agrega `marco` e violações de predecessor. (`:1542`, `:1543`.) | Aprovação e exportação são flags separadas, não pré-condições da consulta. (`:1533`, `:1537`.) |
| PF direto | `app/chamados/juizes.py:2043` — `dia_em_competencia_trancada` | **Outro:** `filter(empresa_id=..., reaberto_em__isnull=True)`; testa se o dia pertence à janela de alguma tranca viva. Exceção resulta em falso. (`:2040`, `:2045`, `:2048`.) | Independe de FM. |
| PF direto | `app/chamados/services/competencia_trancada.py:312` — `medir_frota` | **Chamado/pergunta/disputa — contador:** `filter(reaberto_em__isnull=True)`; inclui o par na medição e agrega resultados de `medir`. (`:313`, `:319`.) | Independe de FM. |
| PF leitura/escrita | `app/ponto/services/fechamento.py:1408` — `trancar_periodo` | **Outro:** `get_or_create`, além do recorte literal `filter/exists`; lê `reaberto_em` para distinguir criação, retranca e repetição sem novo ato. (`:1411`, `:1412`.) | Não valida nem aprova FM. (`:1400`.) |

O gancho não é apenas informativo: havendo ações, executa `encerrar` até o limite ou inicia processo próprio. O encerramento escreve chamado/disputa sob `cartorio.calado`; não escreve célula, ata ou folha. A Pauta DP está depois da guarda de ações e, portanto, não é garantida por qualquer tranca isoladamente. (`app/chamados/services/competencia_trancada.py:168`, `:178`, `:185`, `:205`, `:208`, `:277`.)

As exceções do encerramento são módulos fora da tranca, chamado resgatado depois de `fechado_em`, pergunta viva fora do alcance e disputa cujo pai continua vivo fora do encerramento. (`app/chamados/services/competencia_trancada.py:101`, `:105`, `:143`, `:156`.)

**Leitores de C20, incluindo a forma em lote e o resultado carregado:**

| Arquivo:linha — função | Classe e efeito da tranca |
|---|---|
| `app/chamados/juizes.py:1395` — `_pergunta_viva_calc` | **Pergunta:** retorna falsa antes de considerar a resposta; não planta batida nem absolve o dia. Usa C20 avulso ou `carga.trancada(p)`. (`:1397`.) |
| `app/chamados/models.py:1847` — `DisputaSupervisao.respostas_sem_veredito` | **Disputa:** exclui respostas trancadas da lista impeditiva de fechamento. `ignorar_tranca=True` conserva-as para o levantamento do DP. (`app/chamados/services/competencia_trancada.py:114`.) |
| `app/chamados/reconciliador.py:668` — `perguntas_absolvidas_pela_celula` | **Pergunta:** pula a pergunta, evitando carimbo definitivo de absolvição sobre o silêncio reversível da tranca. (`:664`.) |
| `app/chamados/reconciliador.py:748` — `_dia_fora_do_alcance_c` | **Pergunta:** retira a pergunta do alcance da cura/reemissão O261(C); a consulta de entregue precede a consulta da tranca. (`:743`, `:756`.) |
| `app/chamados/services/disputa_emissao.py:1970` — `_resolver_perguntas_por_chamado_resolvido` | **Pergunta:** pula carimbo definitivo da pergunta silenciada pela tranca. (`:1968`.) |
| `app/chamados/juizes.py:991` — `dia_da_pergunta_trancado` | **Alimentação:** delega a `dias_trancados` com um par; conversa não fornece dia de turno trancável. (`:983`, `:989`.) |
| `app/chamados/juizes.py:1145` — `tranca_da_consulta` | **Alimentação:** pré-carrega C20 por identidade da pergunta; o avulso lê esse resultado em `:987`. |
| `app/chamados/services/tarefa_do_dia.py:422` — `CargaDaTarefa.__init__` | **Pergunta/tarefa:** carrega dias trancados; `_tarefa_pura` devolve razão `competencia_trancada` e ações vazias. (`:195`, `:202`.) |
| `app/chamados/services/carga_pergunta_viva.py:91` — `CargaDaVida.__init__`; `:147` — `CargaDaVida.trancada` | **Alimentação:** reaproveita os dias trancados da tarefa, consumidos por C6 em `app/chamados/juizes.py:1395`. |

Esses leitores independem de FM aprovado. C20 desconta toque humano posterior à tranca: cobrar dia, resolver dia ou desarquivar, com trilha humana; escrita do sistema não basta. Sem dia, colaborador ou empresa, o lote devolve falso para o par. (`app/chamados/juizes.py:1024`, `:1071`, `:1083`, `:1237`, `:1253`.)

O alimentador também é chamado por `perguntas_do_app`, que filtra pelo C6; por `_resolver_perguntas_por_chamado_resolvido`, inclusive mediante `partial` com memo inicial; e novamente com consulta fresca antes do fechamento da disputa. (`app/chamados/services/pergunta_no_app.py:57`, `:58`; `app/chamados/services/disputa_emissao.py:1922`, `:1942`, `:2081`.)

Outros consumidores dos resultados:

| Arquivo:linha — função | Efeito |
|---|---|
| `app/ponto/services/furos_sem_cobranca.py:85` — `medir` | **Outro — contador:** exclui dia trancado. Seu universo é a janela corrente, não todo o passado. (`:60`, `:61`.) |
| `app/ponto/services/cluster_espurio.py:254` — `tratar_legado_encerrado_sem_lastro` | **Chamado:** troca a ação de reabrir por relavrar a via da tranca; registra `PAROU_SEM_PORTA` e não executa essa escrita. (`:237`, `:258`, `:272`.) |
| `app/folha/management/commands/pre_fechamento.py:74` — `Command._marcos_da_anterior` | **Outro — relatório:** imprime marco e violações da competência anterior. (`:75`, `:77`.) |
| `app/ponto/services/fechamento.py:1515` — `empresas_exportadas_no_escopo` | **Folha:** consulta os marcos, mas consome somente `exportada`; a nova tranca não muda esse resultado. |
| `app/ponto/services/fechamento.py:1555` — `marco_da_competencia` | **Outro:** devolve o marco agregado. Chamador produtivo adicional não localizado [nao verificado]. |

A tranca não amplia as janelas dessas rotinas para abril–julho: `furos_diarios.apurar` percorre corrente/anterior, e `furos_sem_cobranca.medir` somente corrente. (`app/escala/services/furos_diarios.py:68`, `:76`; `app/ponto/services/furos_sem_cobranca.py:60`.)

**Célula, regeneração, folha, TXT e batida:**

| Classe | Resultado demonstrado pelo código |
|---|---|
| **Célula — gerar/regenerar/editar** | A nova PF não constitui entrada de K9: a guarda de lavra da célula consulta entrega de TXT ou holerite publicado do colaborador. PF viva não demonstra nenhuma dessas condições. (`app/folha/export.py:597`, `:618`, `:622`.) |
| **Regeneração** | A porta usa o levantamento de competências lavradas; a guarda e a exceção `apesar_da_lavra` são próprias desse caminho. Tranca de PF não equivale a lavra por TXT/holerite. (`app/ponto/portas/celula.py:56`, `:110`, `:134`; `app/folha/export.py:597`.) |
| **Folha — recálculo** | Continua calculando; aberto usa `save`, demais estados recebem atualização dos campos calculados preservando status/carimbo. Não consulta tranca como veto. (`app/ponto/services/fechamento.py:1137`, `:1146`; `app/core/juizes.py:533`.) |
| **TXT/ExportacaoDominio** | Aberto e aprovado são aceitos pelo montador. Gerar TXT pode aprovar abertos elegíveis, mas a tranca isolada não gera TXT nem registro de exportação. (`app/folha/export.py:687`, `:717`; `app/folha/views.py:246`; `app/ponto/services/fechamento.py:1400`.) |
| **Batida/ajuste** | A guarda F6 lê FM aprovado/exportado por colaborador. PF viva com FM aberto não bloqueia por esse critério; batida de chão não passa por essa recusa. (`app/ponto/janelas.py:86`, `:87`; `app/ponto/registro_batida.py:129`, `:132`; `app/ponto/services/flip_batida.py:22`.) |
| **Regularização externa** | A porta atual removeu a consulta à tranca pela O44; conserva suas guardas próprias de confirmação e disputa. (`app/chamados/services/regularizacao_ext.py:14`, `:52`, `:59`.) |

**Escritores e leitores dos campos de `PeriodoFechado`:**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `ativo` | Default `True`, `app/ponto/models.py:933`; criação em `trancar_periodo`, `app/ponto/services/fechamento.py:1410`; retranca `:1413`; reabertura `:1437`. | K7 declara que é documental e não o usa no predicado. (`:1466`, `:1470`.) Outro consumidor produtivo do valor não localizado [nao verificado]. |
| `fechado_por` | Criação `app/ponto/services/fechamento.py:1410`; retranca `:1414`; FK em `app/ponto/models.py:922`. | Consumidor produtivo do valor não localizado [nao verificado]. |
| `fechado_em` | Criação automática em `app/ponto/models.py:926`; retranca explícita em `app/ponto/services/fechamento.py:1415`. | `dias_trancados`, `app/chamados/juizes.py:1070`, para toque posterior; `medir`, `app/chamados/services/competencia_trancada.py:105`, para resgate posterior. |
| `reaberto_em` | Nasce nulo, `app/ponto/models.py:931`; retranca limpa em `app/ponto/services/fechamento.py:1417`; reabertura escreve agora em `:1439`. | `trancar_periodo`, `:1411`, `:1412`; K7 `:1470`; K4 `:1480`; consultas diretas em `app/chamados/juizes.py:2043` e `app/chamados/services/competencia_trancada.py:312`. |

Os predicados executáveis de tranca viva concordam: `reaberto_em IS NULL`. Os leitores de `fechado_em` usam o momento da última tranca como fronteira temporal; uma retranca substitui esse momento. (`app/ponto/services/fechamento.py:1415`, `:1470`, `:1480`; `app/chamados/juizes.py:1070`; `app/chamados/services/competencia_trancada.py:105`.)

**Escritores de `FechamentoMensal.status`:**

| Escritor | Escrita |
|---|---|
| Modelo, `app/ponto/models.py:410` | Default `aberto`. |
| `recalcular_fechamento_mes`, `app/ponto/services/fechamento.py:898` | Criação com `aberto`; o objeto apenas de leitura também recebe esse default quando não há gravado. (`:895`.) |
| `aprovar_lote`, `app/ponto/services/fechamento.py:1329` | `aprovado`, usuário e instante. |
| `aprovar`, `app/ponto/services/fechamento.py:1337` | `aprovado`, usuário e instante, por `save`. |
| `reabrir_lote`, `app/ponto/services/fechamento.py:1346` | `aberto`, limpando aprovação. |
| `reabrir`, `app/ponto/services/fechamento.py:1351` | `aberto`, limpando aprovação. |
| `reabrir_periodo`, `app/ponto/services/fechamento.py:1451` | Somente aprovados da competência voltam a aberto. |
| `restaurar_fechamento`, `app/ponto/services/fechamento.py:1374` | Aceita campos concretos e não exclui `status` da atualização. A docstring diz que não o toca, mas o corpo permite. (`:1370`, `:1380`, `:1393`.) Execução concreta restaurando status [nao verificado]. |

`trancar_periodo` não contém escrita de `FM.status`; o caminho normal associa os atos apenas porque a view chama aprovação e depois tranca. (`app/ponto/services/fechamento.py:1400`; `app/ponto/views.py:1362`, `:1367`.)

**Leitores de `FM.status`, por função e uso:**

| Arquivo:linha — função/template | Uso |
|---|---|
| `app/ponto/models.py:497` — `FechamentoMensal.save` | Recusa `save` aprovado→aprovado. (`:498`.) |
| `app/ponto/models.py:506` — `FechamentoMensal.__str__` | Apresentação do estado. |
| `app/ponto/services/fechamento.py:890` — `recalcular_fechamento_mes` | Copia o status gravado para a leitura online; escolhe forma de persistência por aberto/demais. (`:892`, `:1137`.) |
| `app/ponto/services/fechamento.py:1451` — `reabrir_periodo` | Seleção dos aprovados que reabre. |
| `app/ponto/services/fechamento.py:1533` — `marcos_da_competencia` | Marco `aprovada` por existência de aprovado **ou exportado**. |
| `app/ponto/janelas.py:87` — `competencia_fechada` | Veto F6 de ajuste por aprovado/exportado. |
| `app/ponto/views.py:1157`, `:1169` — `fechamento_mensal` | Contagem de aprovados e disponibilidade de aprovação dos abertos. |
| `app/ponto/views.py:1270`, `:1362` — `aprovar_fechamento` | Universo e carimbo apenas de abertos. |
| `app/ponto/views.py:1412` — `reavaliar_vazios` | Seleciona aprovados para reabrir. |
| `app/ponto/views.py:1447` — `reabrir_colaborador` | Exige aprovado. |
| `app/folha/views.py:152`, `:248` — `gerar_dominio_txt` | Guarda de existência aberto/aprovado; aprovação dos abertos elegíveis. |
| `app/folha/export.py:717`, `:721` — `montar_linhas_export` | Filtra aberto/aprovado em memória ou ORM. |
| `app/folha/services/ranking_he.py:152`, `:157` — `ranking`; `:223`, `:229` — `apuracao_colab` | Retorna status e indica existência de apuração; “lavrado” vem da entrega, não do status. (`:77`.) |
| `app/relatorios/views.py:95` — `horas_mes`; `:160` — `inconsistencias` | Exportação do status em CSV. |
| `app/ponto/management/commands/checklist_golive.py:118` — `Command.handle` | Conta abertos da competência corrente. |
| `app/ponto/management/commands/projecoes_vencidas.py:67` — `Command.handle` | Imprime status de projeções vencidas. |
| `app/templates/ponto/_fech_linha.html:2`, `:4`, `:40` | Atributo da linha, seleção de abertos e selo aprovado. |
| `app/templates/ponto/fechamento.html:111` | JavaScript lê `data-status` para filtrar a apresentação. |
| `app/templates/ponto/partials/painel_fechamento.html:10`, `:11`, `:80`, `:107` | Selo, decisão disponível e apresentação da aprovação. |
| `app/templates/core/dashboard_supervisor.html:123` | Selo aprovado. |
| `app/templates/relatorios/horas_mes.html:75`; `app/templates/relatorios/inconsistencias.html:50` | Selo aprovado. |

Também leem status por fotografia/hash, sem atribuir significado de tranca: `hash_do_gravado`, `app/ponto/portas/regen_exportada.py:56`, `:71`; `foto`, `app/ponto/management/commands/carimbo_gravado.py:34`, `:53`; `Command._hash_dos_fechamentos`, `app/ponto/management/commands/lavrar_dias_pagos.py:68`; `Command.handle._foto`, `app/ponto/management/commands/aplicar_09_he_intra.py:49`, `:128`; `foto_do_colaborador.guardar`, `app/chamados/services/foto_e4b.py:251`, `:290`, e fotografia online em `:330`; `_hash`/fotografia de `bin/relavrar_o118_col221.py:26`, `:37`, `bin/relavrar_o119_col221.py:25`, `:36`, `bin/relavrar_o120_col221_09.py:29`, `:66`; fotografia de `bin/r5_idempotencia_frota.py:75`.

Portanto, os campos **não expressam uma única condição**: PF registra tranca da empresa; FM registra ato individual; entrega do TXT é outra autoridade. A opção `FM.status='exportado'` existe, mas o juiz de entrega consulta `ExportacaoDominio`, não esse status. (`app/ponto/models.py:402`; `app/ponto/services/fechamento.py:1474`, `:1533`; `app/folha/export.py:556`.)

**Divergências e decisões:**

DECISAO: `app/templates/ponto/fechamento.html:208` | a tranca oculta “Fechar período” mesmo havendo abertos que a porta aprovaria; o backend não contém esse veto | lei já responde: nenhuma trava de tela por trancada, `CLAUDE.md:422`; porta em `app/ponto/views.py:1362`.

DECISAO: `app/templates/ponto/partials/painel_fechamento.html:80` | PF viva com FM não aprovado oculta aprovação individual, embora a porta possa aprovar | lei já responde: `CLAUDE.md:422`; porta em `app/ponto/views.py:1509`, `:1515`. Quais linhas concretas precisam dessa escrita [nao verificado].

DECISAO: `app/templates/ponto/_fech_linha.html:42` | tranca oculta reabrir colaborador aprovado, embora a porta permita; esse ramo exige aprovado e não é alcançado pela premissa de ausência desse status | O44 já responde em `app/ponto/views.py:1444`.

DECISAO: `app/ponto/services/fechamento.py:1542` | o contador conserva a ordem dos marcos e pode relatar “trancada sem exportada”; não exige aprovado para consultar a tranca nem bloqueia escrita | o próprio contrato define contador, não veto, em `:1524`; recalcular/TXT sem consultar tranca já está respondido em `app/core/juizes.py:533`. Existência dessa violação nos dez pares [nao verificado].

DECISAO: `app/chamados/juizes.py:1395` e `app/chamados/services/tarefa_do_dia.py:195` | perguntas e tarefas passam ao silêncio automático sem aprovação da folha | regra própria já responde: leitura reversível da tranca e exceção por toque humano, `app/chamados/juizes.py:978`, `:1071`; consulta do anexo em `app/docs/JUIZES.md:86`.

DECISAO: `app/chamados/services/competencia_trancada.py:83` | a tranca pode encerrar chamados/disputas sem FM aprovado, preservadas as exceções | regra já responde em `:9`, `:13`, `:25`; não é inferência “trancada implica aprovada”.

DECISAO: `app/escala/services/furos_diarios.py:77` | tranca retira a janela anterior da reapuração de FuroDiario, independentemente de aprovação | a lei de ausência de barreira por tranca está em `CLAUDE.md:422`; esse leitor mantém a barreira. O código não o transforma em reapuração retrospectiva dos dez pares. (`app/escala/services/furos_diarios.py:68`, `:76`.)

DECISAO: `app/ponto/services/cluster_espurio.py:254` | trancada impede reabertura automática nessa cura e produz `PAROU_SEM_PORTA` para relavrar somente a via | regra existente declarada em `:237`; falta de porta é implementação, não pergunta nova de lei.

DECISAO: `app/ponto/models.py:913`, `app/chamados/services/competencia_trancada.py:4` e `app/ponto/services/fechamento.py:1457` | textos dizem “read-only”, “imutável” ou “nada mais muda”; isso não corresponde a bloqueio geral no vivo | O44 e a lei de corre

[truncado em 20000 de 23681 caracteres]

## 12/11 14:55 · sessao aba12b074178 · E3-10-CURA-NO-ESCRITOR · sonda · origem=Code

**O censo fecha em 143 perguntas, 17 colaboradores, todas em “SEM TRILHA” para autoria de `respondida_em`.** A sonda fornecida informa `erros: 0` e `total_erros 0`; a recontagem dos PKs fecha em 143 distintos. [não verificado — dado fornecido, sem nova consulta à produção]

“SEM TRILHA” significa que os eventos apresentados **não identificam o escritor da resposta**. `pergunta_emitida` registra o nascimento; `carimbo_desfeito_por_lastro` registra a remoção da materialização, preservando resposta e validação. `app/chamados/models.py:3018`; `app/chamados/models.py:3026`; `app/chamados/services/relavra_via.py:244`; `app/chamados/services/relavra_via.py:248`; `app/chamados/models.py:3209`.

Para compactar as tabelas: **R** = `respondida_em`; **T** = `resposta_colab`; **V** = `validada_em`; **M** = `materializada_em`.

Os escritores encontrados na cópia são estes:

| Escritor | Escrita e campos juntos | Assinatura deixada |
|---|---|---|
| `responder_pergunta_disputa_core` — `app/chamados/services/disputa_emissao.py:1467` | Atribui **T + R**; retira validação anterior e eventual carimbo sem lastro; salva em `:1508`. Vazio após `strip()` é recusado em `:1432`. | Decisão **`resposta`**, com texto, instante e autor, em `app/chamados/services/disputa_emissao.py:1511`; log **`resp_pergunta_gravada / PerguntaDisputa`** em `:1529`; se havia validação, **`validacao_desfeita_por_resposta / PerguntaDisputa`** em `:1520`. |
| `responder_automaticamente` — `app/chamados/services/disputa_emissao.py:1652` | Copia o argumento `texto` para **T**, atribui **R** e salva os dois em `:1655`. Preserva texto preexistente, mas não exige argumento novo não vazio em `:1647`. | Decisão **`resposta`**, com motivo “resposta automatica…”, em `app/chamados/services/disputa_emissao.py:1656`; sem LogAuditoria próprio nesse helper. |
| `carimbar_resposta_pela_trilha` — `app/chamados/services/disputa_emissao.py:1674` | Grava somente **R**, usando o instante recebido; exige **T não vazio e R nulo** em `:1670`; save em `:1676`. | Decisão **`resposta`** em `app/chamados/services/disputa_emissao.py:1677`; log **`carimbar_resposta_pela_trilha / PerguntaDisputa`** em `:1679`. |
| `PerguntaDisputa.carimbar` — `app/chamados/models.py:3133` | Acrescenta **R somente se solicitado, T não vazio e R nulo**. Sempre grava **M, materializada_por e via_resolucao**. Com `resposta='validar'`, acrescenta **V e validada_por**, quando nulos; opcionalmente grava **veredito_lavrado, julgado_em e janela_expira_em**. `app/chamados/models.py:3129`; `:3131`; `:3135`; `:3150`; `:3153`. | Não registra decisão nem LogAuditoria próprios; a assinatura depende do chamador. `app/chamados/models.py:3104`. |
| Nascimento `_nascer` — `app/chamados/models.py:3015` | Recebe `**campos`, portanto pode inserir **R/T/V recebidos**, com `save(force_insert=True)` em `:3018`. | Registra decisões das assinaturas recebidas em `app/chamados/models.py:3020`, usando a projeção de `:3932`; log **`pergunta_emitida / PerguntaDisputa`** em `:3026`. Chamador concreto passando R para alguma das 143: [não verificado]. |

A porta `carimbar` tem quatro chamadores que solicitam escrita de R:

| Chamador | Campos e trilha específicos |
|---|---|
| `_materializar_ausencia_de_resposta` — `app/chamados/services/materializacao.py:567` | Pede `resposta='validar'`; via declaração ou ausência do dia em `:555`. Registra decisão **`absorcao`**, ligando pergunta, escape, ausência, texto e instante, em `app/chamados/services/materializacao.py:569`. |
| `_materializar_uma` — `app/chamados/services/materializacao.py:1283` | Pede `resposta='validar'` e `lavrar_veredito=True`; via declaração ou via do fato. Sem decisão/log de sucesso próprios nesse ramo. `app/chamados/services/materializacao.py:1267`; `:1283`. |
| `resolver_perguntas_por_saida`, ramo principal — `app/chamados/services/disputa_emissao.py:1986` | Pede `resposta='validar'` e `lavrar_veredito=True`, pela via do fato. Sem decisão/log por pergunta nesse ramo. `app/chamados/services/disputa_emissao.py:1986`; `:1989`. |
| Mesmo serviço, ramo sem credor — `app/chamados/services/disputa_emissao.py:2060` | Pede `resposta='responder'`; log **`fecho_sem_credor / PerguntaDisputa`**, com antes/depois de R, M, via, presença de texto e chamado. `app/chamados/services/disputa_emissao.py:2051`; `:2063`; `:2068`. |

Também há escritores que **zeram** R, além de atribuições somente em memória:

| Sítio | Efeito e trilha |
|---|---|
| `ressuscitar_perguntas` — `app/chamados/models.py:1567` | Zera **R, V, validada_por, M, materializada_por e via** por `descarimbar_em_lote`; o `update()` efetivo está em `app/chamados/models.py:3265`. Seleciona previamente T nulo/vazio em `:1487`. Preserva decisões projetadas em `:1564`; log **`perguntas_ressuscitadas / ChamadoColaborador`**, contendo PKs, em `:1590`. |
| `reabrir_para_correcao` — `app/chamados/services/disputa_emissao.py:1593` | Zera **R, T, V e validada_por**; opcionalmente desfaz também a materialização. Registra decisão **`reabertura` ou `recusa`** com foto anterior em `:1615`; grava por `update()` em `:1632` ou pela porta de lote em `:1630`. |
| Cópia para julgamento — `app/chamados/models.py:1509` | Atribui `R=None` numa cópia usada para decidir a ressuscitação; não persiste essa cópia. `app/chamados/models.py:1495`; `:1510`. |
| Rollback da porta — `app/chamados/models.py:3164` | Restaura R em memória quando o carimbo é recusado ou falha; não é uma nova resposta persistida. `app/chamados/models.py:3142`; `:3156`. |

O `save()` geral apenas deriva a identidade na inserção e chama `super().save()` dentro de transação; não fabrica R nem valida o par R/T. `app/chamados/models.py:3414`; `app/chamados/models.py:3421`; `app/chamados/models.py:3424`.

Nas bordas solicitadas:

| Borda | Caminho efetivo |
|---|---|
| Views web | Delega ao core de resposta. `app/chamados/views.py:1283`. |
| API de resposta | Recusa vazio e delega ao mesmo core; acrescenta logs `resp_pergunta_recusada`/`resp_pergunta_ok`, modelo `PerguntaDisputa`. `app/api/views.py:2594`; `app/api/views.py:2614`; `app/api/views.py:2620`; `app/api/views.py:2624`. |
| Correção pelo admin e declaração de horário | Delegam ao mesmo core; correção acrescenta `corrigir_resposta_admin`. `app/chamados/services/acoes_disputa.py:371`; `:375`; `app/chamados/services/declaracao_texto.py:419`; `:693`. |
| Upload de foto | Chama resposta automática com texto constante não vazio. `app/api/foto_recebida.py:25`; `app/api/foto_recebida.py:48`. |
| Management command de cura do limbo | Delega a `carimbar_resposta_pela_trilha` ou a `reabrir_para_correcao`. `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`; `:69`. |
| Migrations | Foi encontrada a criação do campo, sem atribuição de dados a R. `app/chamados/migrations/0014_perguntadisputa.py:25`. Ausência de outro escritor em migrations na varredura: [não verificado — resultado de busca estática]. |
| Bulk | O `bulk_update` encontrado de perguntas grava apenas a lavra: `veredito_lavrado`, `julgado_em`, `janela_expira_em`. `app/chamados/veredito_lavrado.py:237`; `:250`. Não foi encontrado bulk de R: [não verificado — resultado de busca estática]. |

`data_conferida_em` é escrito separadamente, junto de `data_conferida_por`, com log **`conferir_data / PerguntaDisputa`**; esse save não grava R. `app/chamados/services/validacao.py:664`; `:666`; `:668`.

O casamento por pergunta resulta nesta única autoria consolidada. Todos os números, colaboradores e PKs desta tabela vêm exclusivamente da sonda fornecida. [não verificado]

| Escritor | N perguntas | N colabs | PKs — todos, agrupados por colaborador | Evidência na trilha |
|---|---:|---:|---|---|
| **SEM TRILHA** — escritor de R não identificado | **143** | **17** | **col60:** 31436<br>**col191:** 29167, 29168, 29169, 31724, 31725, 31726, 31727, 31728, 31729, 31730, 31731, 32909, 32910, 33593, 33594, 34182, 34183<br>**col204:** 31584, 31585, 31586, 31587<br>**col224:** 32291, 32292, 32293, 32294, 32295, 32296, 33925, 33926, 33927<br>**col310:** 32243, 32244, 32245, 32246, 34141, 34142<br>**col331:** 22394<br>**col349:** 29502, 29503, 32131, 32132, 32133, 32134, 32135, 32136, 33749, 33750<br>**col437:** 29565, 32224, 32225, 32226, 32227, 32228, 32229, 32230, 32231, 32232, 32233, 32234, 32235, 32236, 32237, 32238<br>**col446:** 29872, 32589, 32590, 32591, 32592, 32593, 32594, 32595, 32596, 32597, 32598, 32599, 32600, 32601, 33146, 33147<br>**col592:** 28357, 31708, 31709, 31710, 31711, 31712, 31713, 31714, 31715, 31716, 31717<br>**col607:** 29114, 31334, 31335<br>**col782:** 32279, 32280, 32281, 32282, 32283, 32284, 32285, 32286, 32287, 32288, 32289, 32290<br>**col789:** 31839, 31840, 31841, 31842, 33549, 33550<br>**col869:** 28443, 29270, 31863, 31864, 31865, 31866, 31867, 31868, 31869, 31870, 31871, 31872, 31873<br>**col893:** 32459, 32460, 32461, 32462, 32463, 32464, 32465, 32466, 33577, 33578, 34171, 34172<br>**col900:** 38251, 38252<br>**col903:** 31472, 31473, 31476, 31477 | Todas têm `decisoes=[]`. **30** só apresentam `pergunta_emitida`; **113** apresentam também `carimbo_desfeito_por_lastro`. Nenhuma apresenta ato identificador da escrita original de R. [não verificado — sonda fornecida] |
| **Total** | **143** | **17 distintos** | **143 PKs distintos** | **30 + 113 = 143.** [não verificado — recontagem do dado fornecido] |

Os motivos para não atribuir por assinatura são concretos:

- **`pergunta_emitida` não prova autoria da resposta:** o evento nasce depois da inserção e descreve emissão do questionário. `app/chamados/models.py:3018`; `app/chamados/models.py:3026`.
- **Os 113 desfazimentos identificam outro ato:** o log fotografa somente M/via e chama `descarimbar`, que salva M, autor da materialização e via. Não escreve R/T. `app/chamados/services/relavra_via.py:244`; `:246`; `:248`; `app/chamados/models.py:3211`.
- **A via anterior `chamado_encerrado` não é exclusiva:** pode ser escolhida pelo materializador e pelo fecho sem credor. `app/chamados/services/materializacao.py:1274`; `app/chamados/services/disputa_emissao.py:2059`.
- **Os cinco com via anterior `sem_turno_aberto`** são 29167, 29168, 29169, 29502 e 29503. [não verificado — sonda fornecida] O lavrador atual dessa via chama `carimbar` sem solicitar resposta; seu comentário registra a remoção do antigo bloco de R/V. Isso não demonstra quem escreveu R nesses cinco PKs. `app/chamados/reconciliador.py:402`; `:406`.
- **Igualdade de timestamps e contagens por disputa não identificam o ato escritor por PK.** Atribuir pelas coincidências seria [não verificado].
- **Decisões ausentes não excluem automaticamente escritores históricos:** o código prevê projeções anteriores à sequência ou carimbos sem ato. Essa recuperação guarda texto/instante, com motivo genérico, sem recuperar o escritor histórico. `app/chamados/models.py:3912`; `:3918`; `:3934`.

Quanto à entrada do vazio, **nenhum caminho histórico foi demonstrado para qualquer uma das 143**. [não verificado — trilha fornecida] O que o código permite afirmar é:

| Caminho | Conclusão sustentada pelo código |
|---|---|
| Colaborador enviou vazio pelo core atual | O core recusa após `strip()`, antes de escrever R/T. `app/chamados/services/disputa_emissao.py:1432`. |
| Escritor carimba sem copiar texto | `carimbar` e a cura pela trilha não copiam T; exigem texto já existente antes de acrescentar R. `app/chamados/models.py:3131`; `app/chamados/services/disputa_emissao.py:1670`. |
| Resposta automática recebe vazio | O helper não valida o argumento novo; porém o chamador encontrado passa texto constante não vazio. Não há prova desse caminho nas 143. `app/chamados/services/disputa_emissao.py:1652`; `app/api/foto_recebida.py:25`; `:48`; [não verificado — execução histórica]. |
| Texto apagado depois | O apagador explícito atual zera **T e R juntos**, deixando decisão com foto anterior. A recusa do admin usa essa porta. Portanto, esse caminho sozinho não deixa T vazio com R preservado. `app/chamados/services/disputa_emissao.py:1593`; `:1615`; `:1632`; `app/chamados/services/validacao.py:593`. |
| R recebido no nascimento | `_nascer` aceita campos recebidos e os persiste; registra as assinaturas pela projeção. Uso desse caminho para produzir o estado das 143: [não verificado]. `app/chamados/models.py:3015`; `:3018`; `:3020`. |
| Antigo fecho sem credor | O comentário documenta que esse ramo carimbava R sem texto. Isso registra histórico do código, sem vincular qualquer PK da sonda ao ramo. `app/chamados/services/disputa_emissao.py:2053`; [não verificado — autoria das 143]. |

Os leitores que usam R/T para decidir comportamento incluem:

| Leitor | Decisão |
|---|---|
| Predicado pedido | `respondida_sem_texto` exige R não nulo e T vazio após `strip()`. `app/chamados/juizes.py:1510`; `:1515`. |
| Vitalidade C6 e espelho SQL | O C6 considera resposta com R **e texto**; `q_vivas` espelha a existência de resposta. `app/chamados/juizes.py:1375`; `:1407`; `app/chamados/models.py:3349`. |
| Estado e propriedade de resposta | `estado` distingue PENDENTE/RESPONDIDA pelo carimbo nos ramos aplicáveis; `respondida` testa somente R. `app/chamados/models.py:3042`; `:3046`; `:3049`; `:3497`. |
| Fechamento da disputa | `respostas_sem_veredito` aceita **R ou texto**, sem V/M e respeitando tranca; `pode_fechar` combina essa lista com fio vivo. `app/chamados/models.py:1843`; `:1854`. |
| Consulta do app | Viva com R preenchido vai para **histórico**; pendentes exigem R nulo e disputa aberta. Badge usa o tamanho dessa lista. `app/chamados/services/pergunta_no_app.py:71`; `:74`; `:91`; `:98`. |
| API do app | Usa a consulta; calcula `aguarda_admin` pelo estado RESPONDIDA e inclui carimbadas em `respondidas`. `app/api/views.py:2530`; `:2543`; `:2570`. |
| Possibilidade de solicitar correção | Exige todas as perguntas com R e limite de reaberturas. `app/api/views.py:1172`; `app/chamados/services/fio.py:340`. |
| Fechamento manual | Barra quando alguma viva não tem R. `app/chamados/services/fio.py:660`; `app/chamados/services/acoes_disputa.py:130`. |
| Validação e recusa | Validação exige R; recusa sem R devolve “já pendente”. `app/chamados/services/validacao.py:393`; `:576`. |
| Seleção e execução da materialização | Seleciona R/V preenchidos e M nulo; o plantio recusa T vazio. `app/chamados/services/materializacao.py:1375`; `:835`; `:842`. |
| Correção e fila de validação | Corrigíveis começam nas carimbadas; fila global exige R e exclui texto vazio, depois julga vitalidade. `app/chamados/juizes.py:1663`; `:1670`. |
| Declaração, delta e plausibilidade | T decide se houve declaração de hora, delta, classe e plausibilidade. `app/chamados/juizes.py:1699`; `:1846`; `:1950`; `:1972`. |
| Respostas de irmãs ainda não consumidas | Exige R, texto e ausência de V/M. `app/chamados/juizes.py:2020`; `:2024`. |
| Porta alcançável e vigias | A porta recusa `respondida_sem_texto`; o vigia classifica a causa e conta ocorrências pelo instante de R. `app/chamados/juizes.py:1651`; `app/chamados/services/vigia_fio.py:673`; `:783`. |
| Cobrança, recobrança e becos | Selecionam pendência pelo carimbo nulo ou classificam carimbadas para validação. `app/chamados/views_cobrar.py:169`; `app/chamados/views.py:1416`; `app/chamados/services/acoes_disputa.py:95`; `app/chamados/services/cobranca.py:229`; `app/chamados/services/furo_sem_canal.py:80`; `app/chamados/services/becos.py:29`. |
| Reabertura de credor | Recusa pergunta carimbada; reconhece limbo por T sem R. `app/chamados/services/disputa_emissao.py:1821`; `:1839`. |
| Proteção contra resolução automática | Texto bloqueia cura sem turno; R/T protegem resposta sem materialização de fato. `app/chamados/reconciliador.py:365`; `:1083`; `app/chamados/services/disputa_emissao.py:1979`. |
| Deduplicação | Prioriza texto e protege mérito do admin associado a texto. `app/chamados/services/fio.py:1233`; `:1244`. |
| Retratação automática de cobrança | Recusa se existe pergunta ligada com R. `app/ponto/supra_juiz.py:545`; `:548`. |
| Métrica, ficha e prévia da folha | R determina resposta posterior à cobrança; T participa da classificação da ficha e do material disponível para o dia. `app/chamados/services/verbos.py:335`; `:342`; `app/colaboradores/services/ficha.py:169`; `app/folha/previa.py:142`. |
| Templates | R controla “aguardando validação”, resposta exibida e enviar/reenviar. `app/templates/chamados/partials/_pergunta_card.html:24`; `:29`; `app/templates/chamados/partials/_card_atendimento.html:94`; `:155`; `app/templates/chamados/partials/modal_fio.html:217`. |

**A condição “o app não muda” não foi medida no dado fornecido.** [não verificado] Pelo código, retirar R pode alterar a partição entre histórico e pendentes quando a disputa está aberta, além do estado e do badge. `app/chamados/services/pergunta_no_app.py:71`; `:91`; `app/chamados/models.py:3046`. Não há base no anexo para declarar o passivo alinhamento interno sem mudança visível, nem para escolher um escritor histórico como alvo da cura. [não verificado]

## 12/11 14:55 · sessao 617654d42634 · - · - · origem=Ronald (tmux)

Consolidado enviado ao pai, com referências físicas dos anexos.

- Os 14 faltantes v5 entram na ordem; as seis pousadas antigas saem; X1 passa a distinguir o restante. (`semente_item3_v6.py:12`, `:16–17`, `:61–63`; `contrato_item3_v6.txt:20–21`, `:36`, `:42`)
- ESMERIL-SEM-INSTANTE continua aberta no BACKLOG e entra sem trava, apesar do pouso documentado: viola concretamente o universo “não pousada”. (`app/docs/BACKLOG.md:1830`; `app/docs/RELATO.md:1005`; `semente_item3_v6.py:62`; `contrato_item3_v6.txt:2`, `:21`)
- SAIDA e AUD6 continuam sem identidade de tabela nesta cópia; o script exato, executado apenas em memória com sufixos simulados, retorna rc2. (`semente_item3_v6.py:55–57`, `:64–66`, `:82`; identidade presente: [nao verificado])
- APP continua atrás de O168, contrariando o aval literal citado na própria linha; a exceção não tem aval adicional demonstrado. (`app/docs/BACKLOG.md:1916`; `contrato_item3_v6.txt:26`, `:30`; aval adicional: [nao verificado])
- LASTRO-FOLGA permanece sem reconciliação; os planos ORDEM-ATE-O-ZERO e RAIA-2 entram mecanicamente, enquanto META é excluído explicitamente. (`app/docs/BACKLOG.md:1812`, `:1883`, `:1906`, `:2406`; `app/docs/RELATO.md:175`; `contrato_item3_v6.txt:20`, `:22`)
- Não bloqueia: a fórmula esquece descontar três IDs fora do bloco, mas o script já desconta; sufixos geram NAO SEI determinístico no matcher que o contrato manda preservar. (`contrato_item3_v6.txt:4`, `:10`, `:22–23`, `:44`, `:46`; `semente_item3_v6.py:72`; `bin/disjuntor.py:1688–1695`)

## 12/11 14:54 · sessao 4b2f9e7f47af · - · - · origem=Ronald (tmux)

Eu separaria assim:

1. **Novo bloqueante normativo:** exclusão geral por `_item_fechado` antes da reconciliação por ID. A v6:17,21 autoriza descartar fechadas pela coluna sem inventariá-las como pousadas; a semente:40–41 executa isso. O predicado atual também aceita tachado da descrição; `bin/hook_stop_fila1.py:168`. Isso não demonstra que alguma das 208 fechadas esteja indevidamente excluída — pouso delas: **[nao verificado]** —, mas o contrato permite exclusão sem a prova por ID exigida pelo aval. A v6:12 declara a mudança do censo e não resolve essa definição da semente. Corrigir exige reconciliar os IDs ou obter autorização para o critério por estado.

2. **ESMERIL-SEM-INSTANTE:** achado herdado **ABERTO**, com pouso documental já apontado pela leitura v5; `item3_contrato_v5.r`, seção 4; `app/docs/RELATO.md:1005`. Se permanece em ORDEM, contraria “não pousada”, mas a correção por ID é inequívoca; não é novo dilema contratual.

3. **Identidades ausentes:** **ABERTO operacional**, sem contar como novo bloqueante contratual pelo limiar solicitado. O contrato exige identidade única; v6:3. A semente recusa a inexistência; `semente_item3_v6.py:56–58`, `:85`. O contador fechar não substitui essa guarda. Não há licença contratual para omitir esses IDs silenciosamente.

4. **APP:** **ABERTO herdado**, agora com tentativa explícita de reinterpretação. A v6:30 reduz a LASTRO a precedência “antes de O168/LASTRO” registrada em `app/docs/BACKLOG.md:1916`; autorização nova para isso: **[nao verificado]**. Apresentaria no item 1 e não como defeito novo no item 2. Se o total final incluir qualquer aberto que requer aval, soma mais um.

5. **V5 integral ausente:** não contar como bloqueante; registrar detalhes herdados não demonstrados como **[nao verificado]**.

Portanto: **1 novo bloqueante** pelo critério estrito do item 2; **APP permanece aberto** e pode elevar para **2 impedimentos de fechamento** se o veredito contabilizar também os herdados que dependem de aval.

## 12/11 14:52 · sessao 211c513607a0 · - · - · origem=Ronald (tmux)

A trilha fornecida não identifica o escritor de `respondida_em` de nenhuma das 143 perguntas: **143 em SEM TRILHA, 17 colabs**; isso é conclusão sobre a suficiência do dado fornecido, não atribuição histórica. [não verificado]

- `pergunta_emitida` é gravado no nascimento, em `app/chamados/models.py:3026`, após inserir a pergunta em `app/chamados/models.py:3015`; esse log não descreve escrita posterior de resposta.
- `carimbo_desfeito_por_lastro` identifica a remoção do carimbo de materialização, em `app/chamados/services/relavra_via.py:244`, `app/chamados/services/relavra_via.py:246` e `app/chamados/services/relavra_via.py:248`. O antes/depois contém `materializada_em` e `via_resolucao`, não o ato que gravou `respondida_em`.
- A remoção chama `PerguntaDisputa.descarimbar`, que salva apenas `via_resolucao`, `materializada_em` e `materializada_por`, em `app/chamados/models.py:3209` e `app/chamados/models.py:3213`. Logo, o removedor não é o escritor da resposta.
- `chamado_encerrado` não identifica escritor único: é usado por `app/chamados/services/disputa_emissao.py:2058` e `app/chamados/services/materializacao.py:1269`.
- `sem_turno_aberto` tem lavrador atual em `app/chamados/reconciliador.py:341` e `app/chamados/reconciliador.py:406`, mas esse caminho não pede escrita de resposta: chama `carimbar` sem `resposta`. O comentário do bloco removido não prova autoria dos cinco PKs em produção, em `app/chamados/reconciliador.py:402`. Atribuir esses cinco ao escritor de `respondida_em` seria [não verificado].
- Igualdade entre `materializada_em` e `respondida_em`, bem como contagens de logs por disputa, não fornece identidade do ato escritor por PK. [não verificado]

Inventário das portas API/web e suas assinaturas:

| Porta | Escritor efetivo | Trilha e campos |
|---|---|---|
| Web `app/chamados/views.py:1265`, `app/chamados/views.py:1283` | `app/chamados/services/disputa_emissao.py:1467` | Core recusa vazio em `:1432–1434`; grava texto+timestamp em `:1467–1469`/`:1508`; decisão `resposta` em `:1511`; log `resp_pergunta_gravada`/`PerguntaDisputa` em `:1529`. |
| API `app/api/views.py:2614`, `app/api/views.py:2620` | Mesmo core | API recusa vazio em `app/api/views.py:2615`; logs `resp_pergunta_recusada`/`resp_pergunta_ok` via `app/api/views.py:2594`, `app/api/views.py:2624`; modelo `PerguntaDisputa`. |
| Correção admin `app/chamados/services/acoes_disputa.py:371` | Mesmo core | Texto HH:MM obrigatório em `:336`; log adicional `corrigir_resposta_admin` em `:375`; depois valida em `:379`. |
| Declaração de horário `app/chamados/services/declaracao_texto.py:419`, `app/chamados/services/declaracao_texto.py:693` | Mesmo core | Resposta passa pelo mesmo escritor e sua decisão/log; notas em `app/chamados/services/declaracao_texto.py:423`. |
| Upload foto `app/api/views.py:856`, `app/api/views_core.py:840` → `app/api/foto_recebida.py:48` | `app/chamados/services/disputa_emissao.py:1652` | Texto constante não vazio em `app/api/foto_recebida.py:25`; grava par em `:1652–1655`; decisão `resposta` com motivo automático em `:1656`; sem log próprio nesse helper. |
| Cura limbo `app/chamados/management/commands/curar_perguntas_em_limbo.py:67` | `app/chamados/services/disputa_emissao.py:1674` | Exige texto existente e timestamp nulo em `:1670`; salva só timestamp em `:1676`; decisão `resposta` em `:1677`; log `carimbar_resposta_pela_trilha`/`PerguntaDisputa` em `:1679`. |

Detalhes relevantes ao inventário do agente principal:

- `responder_automaticamente` não valida se seu argumento `texto` é vazio; o único chamador executável encontrado passa a constante não vazia da foto. `app/chamados/services/disputa_emissao.py:1647`, `app/chamados/services/disputa_emissao.py:1652`, `app/api/foto_recebida.py:25`, `app/api/foto_recebida.py:48`.
- O core limpa validação velha junto à resposta (`validada_em`, `validada_por`) em `app/chamados/services/disputa_emissao.py:1471`, `app/chamados/services/disputa_emissao.py:1480`; limpa carimbo sem lastro através de `reabrir_por_resposta` em `app/chamados/services/disputa_emissao.py:1485`; grava log adicional `validacao_desfeita_por_resposta` quando aplicável em `app/chamados/services/disputa_emissao.py:1520`.
- `PerguntaDisputa.carimbar` só atribui `respondida_em` se `resposta` foi pedida e o texto existente não é vazio, em `app/chamados/models.py:3131`. Não copia texto. Atribui timestamp em `:3133`; se `resposta='validar'`, atribui `validada_em`/`validada_por` em `:3135`; sempre escreve materialização/via em `:3130`; pode lavrar `veredito_lavrado`, `julgado_em`, `janela_expira_em` em `:3150`; salva em `:3153`. Não grava decisão nem LogAuditoria próprios nesse método.
- Chamadores que pedem resposta ao carimbo: absorção de ausência em `app/chamados/services/materializacao.py:567` (`validar`, decisão `absorcao` em `:569`); materialização em `app/chamados/services/materializacao.py:1283` (`validar`, lavra); resolução por chamado em `app/chamados/services/disputa_emissao.py:1986` (`validar`, lavra); fecho sem credor em `app/chamados/services/disputa_emissao.py:2060` (`responder`, log `fecho_sem_credor` em `:2063`).
- Nascimento genérico aceita `**campos`, insere e registra as assinaturas recebidas, em `app/chamados/models.py:3015`, `app/chamados/models.py:3018`, `app/chamados/models.py:3020`. É um caminho potencial de entrada de `respondida_em`; chamadas concretas com esse campo precisam ser verificadas pelo inventário principal.
- O método geral `PerguntaDisputa.save` apenas deriva identidade na inserção e chama `super().save` dentro de transação; não fabrica resposta/timestamp, em `app/chamados/models.py:3414`, `app/chamados/models.py:3421`, `app/chamados/models.py:3424`.
- O próprio código prevê projeção antiga sem linha de decisão: `MOTIVO_PROJECAO_SEM_LINHA`, em `app/chamados/models.py:3912`; descrição em `app/chamados/models.py:3918`; recuperação de resposta em `app/chamados/models.py:3932`. Portanto, ausência de decisões não exclui escritores históricos por si só.
- Comentário que reconhece carimbo antigo sem texto em `app/chamados/services/disputa_emissao.py:2053` é evidência do histórico descrito no código; não comprova que esse escritor produziu qualquer PK da sonda. [não verificado]

## 12/11 14:52 · sessao 939b40b206eb · - · - · origem=Ronald (tmux)

Leitores que decidem usando `respondida_em`/`resposta_colab`:

| Leitor | Decisão |
|---|---|
| `app/chamados/juizes.py:1510` e `:1515` | `texto_vazio` usa `(t or '').strip()`; `respondida_sem_texto` exige carimbo não nulo e texto vazio. |
| `app/chamados/juizes.py:1375` e `:1407` | Projeção temporal corta resposta futura; vitalidade dá precedência à resposta somente com carimbo **e texto**. |
| `app/chamados/models.py:3349` | Espelho SQL `q_vivas` considera resposta com carimbo e texto diferente de `''`/NULL; a decisão final continua no juiz. |
| `app/chamados/models.py:3042` | `estado`: validada/materializada prevalecem; nos demais ramos o carimbo sozinho distingue PENDENTE/RESPONDIDA (`:3046`, `:3049`). |
| `app/chamados/models.py:3497` | Propriedade `respondida` é somente `respondida_em is not None`. |
| `app/chamados/models.py:1843` | `respostas_sem_veredito` inclui sem validação/materialização quando existe carimbo **ou** texto, respeitando tranca. |
| `app/chamados/models.py:1854` | `pode_fechar` exige ausência de respostas sem veredito e de fio vivo. |
| `app/chamados/services/pergunta_no_app.py:69` | **App:** viva com carimbo entra em `historico`; apenas carimbo nulo + disputa aberta entra em `pendentes` (`:71-75`). |
| `app/chamados/services/pergunta_no_app.py:90` | Badge/contador usam tamanho de `pendentes` (`:91`, `:98-103`). |
| `app/api/views.py:2530` | API lê essa consulta; `aguarda_admin` depende de estado RESPONDIDA (`:2543-2546`); `respondidas` contém histórico com carimbo (`:2570`). |
| `app/api/views.py:1172` | Oferece solicitar correção quando todas perguntas possuem carimbo e reaberturas <2. |
| `app/chamados/services/fio.py:340` | Todas respondidas/possibilidade de correção usam carimbo sozinho. |
| `app/chamados/services/fio.py:660` e `app/chamados/services/acoes_disputa.py:130` | Fechamento manual barra se alguma viva tem carimbo nulo. |
| `app/chamados/services/validacao.py:393` | Validação exige carimbo; sem ele retorna “Aguardando resposta do colab”. |
| `app/chamados/services/validacao.py:576` | Recusa sem carimbo retorna “já pendente”. |
| `app/chamados/services/materializacao.py:1375` | Seleção para materializar: respondida, validada e não materializada. |
| `app/chamados/services/materializacao.py:835` | Plantio recusa texto vazio como “sem resposta do colaborador”, chamando porta de recusa (`:842`). |
| `app/chamados/juizes.py:1663` | Perguntas corrigíveis começam pelas carimbadas, depois juiz de lastro/vitalidade. |
| `app/chamados/juizes.py:1670` | Fila global de validação exige carimbo e exclui texto `''`, depois verifica viva. |
| `app/chamados/juizes.py:1699` | `declarou_hora` exige texto e exclui escapes maiúsculos/`[descartado`. |
| `app/chamados/juizes.py:1846`, `:1950`, `:1972` | Texto determina delta, classe e plausibilidade da resposta. |
| `app/chamados/juizes.py:2020` | Irmãs respondidas não consumidas exigem carimbo, ausência de validação/materialização e texto (`:2024`). |
| `app/chamados/juizes.py:1651` | Porta alcançável recusa `respondida_sem_texto`. |
| `app/chamados/services/vigia_fio.py:673` | Viva respondida sem texto recebe causa `b`; `:675` classifica carimbada sob âncora terminal como causa `c`. |
| `app/chamados/services/vigia_fio.py:783` | Contador seleciona pelo instante do carimbo e conta textos vazios (`:786`). |
| `app/chamados/views_cobrar.py:169` | Preview de validação seleciona vivas carimbadas; sem elas busca perguntas com carimbo nulo (`:175`). |
| `app/chamados/views.py:1416` | Reabrir/recobrar conta sem carimbo e vivas; se não há corrigíveis, recobra (`:1424-1425`). |
| `app/chamados/services/acoes_disputa.py:95` | Mensagem “questionário já pendente” conta carimbo nulo. |
| `app/chamados/services/cobranca.py:229` | Silenciosos: vivas sem carimbo em disputa aberta. |
| `app/chamados/services/furo_sem_canal.py:80` | Pergunta aberta para diagnóstico significa carimbo nulo. |
| `app/chamados/services/becos.py:29` | Pergunta visível para classificar beco exige carimbo nulo e destino não resolvido. |
| `app/chamados/services/fio.py:1233`, `:1244` | Dedup prioriza texto; protege mérito do admin com texto. |
| `app/chamados/reconciliador.py:365`, `:1083` | Texto bloqueia cura automática sem turno e resolução automática do marco. |
| `app/chamados/services/disputa_emissao.py:1821` | Reabrir credor recusa pergunta carimbada; `:1839` reconhece limbo por texto sem carimbo. |
| `app/chamados/services/disputa_emissao.py:1979` | Protege resposta com texto/carimbo sem materialização de fato contra vassoura automática. |
| `app/ponto/supra_juiz.py:545`, `:548` | Executor de cobrança sem furo recusa retratação automática se há carimbo em qualquer pergunta ligada. |
| `app/chamados/services/verbos.py:335`, `:342` | Métrica de resposta pós-cobrança compara carimbo com instante da cobrança. |
| `app/colaboradores/services/ficha.py:169` | Ficha usa respostas sem veredito; classifica sem resposta quando não está entre essas e não há texto (`:171`). |
| `app/folha/previa.py:142` | Material disponível para o dia distingue “resposta do colaborador” por texto, depois pergunta viva. |

Templates também tomam decisões pelo carimbo: `app/templates/chamados/partials/_pergunta_card.html:24` mostra “aguardando validação”; `:29` mostra resposta e controles; `app/templates/chamados/partials/_card_atendimento.html:94` mostra “você respondeu”, `:155` escolhe reenviar/enviar; `app/templates/chamados/partials/modal_fio.html:217` exibe bloco de resposta.

Caminhos de entrada e apagamento:

- Core web/API recusa vazio após strip em `app/chamados/services/disputa_emissao.py:1432-1434`, copia texto e carimbo em `:1467-1469`. API ainda recusa vazio na casca em `app/api/views.py:2614-2617`; web chama core em `app/chamados/views.py:1283`.
- `responder_automaticamente` protege texto existente em `app/chamados/services/disputa_emissao.py:1647`, mas **não valida texto recebido como não vazio**: atribui ambos em `:1652-1653`, salva e registra decisão `resposta` em `:1655-1657`. Chamador histórico das 143 [não verificado].
- `carimbar_resposta_pela_trilha` só carimba quando já existe texto e não há carimbo: `app/chamados/services/disputa_emissao.py:1670`; registra decisão `resposta` e evento homônimo em `:1677-1681`.
- Apagador explícito encontrado: `reabrir_para_correcao` zera **texto e carimbo juntos**, além da validação, em `app/chamados/services/disputa_emissao.py:1593`; registra foto `reabertura`/`recusa` antes em `:1615-1628`, aplica update em `:1630-1632`. Portanto esse caminho atual não preserva carimbo deixando texto vazio.
- Recusa do admin chama essa porta com ato `recusa` em `app/chamados/services/validacao.py:593-594`, registra evento e `LogAuditoria` `recusar_pergunta/PerguntaDisputa` em `:602-610`.

Aplicação às 143: pelo código da consulta, perguntas vivas com carimbo não nulo seguem para histórico, mesmo com texto vazio (`app/chamados/services/pergunta_no_app.py:69-75`); a API as inclui em `respondidas` e `historico` (`app/api/views.py:2559-2571`). O anexo não contém a medição efetiva do app de cada colaborador, portanto resultado real por colaborador e garantia “app não muda” [não verificado].

## 12/11 14:52 · sessao dd082d0c62f2 · - · - · origem=Ronald (tmux)

Inventário dos escritores de `respondida_em` encontrados no código executável:

| Escritor | Persistência e campos juntos | Trilha |
|---|---|---|
| `app/chamados/services/disputa_emissao.py:1468`, `responder_pergunta_disputa_core` | `resposta_colab` + `respondida_em`; limpa validação anterior e eventual carimbo sem lastro; salva em `:1508`. Texto vazio é recusado em `:1432–1434`. | `DecisaoPergunta.RESPOSTA` com instante/texto/autor em `:1511`; `resp_pergunta_gravada / PerguntaDisputa` em `:1529`; eventual `validacao_desfeita_por_resposta` em `:1520`; followup em `:1555`. |
| `app/chamados/services/disputa_emissao.py:1653`, `responder_automaticamente` | `resposta_colab=texto` + `respondida_em=instante`, save em `:1655`. Não valida que o argumento `texto` seja não vazio; apenas preserva texto que já existia em `:1647`. | `DecisaoPergunta.RESPOSTA` em `:1656`, motivo “resposta automatica…”. |
| `app/chamados/services/disputa_emissao.py:1674`, `carimbar_resposta_pela_trilha` | Salva somente `respondida_em` em `:1676`; exige texto já presente em `:1670`. | `DecisaoPergunta.RESPOSTA` em `:1677`; `carimbar_resposta_pela_trilha / PerguntaDisputa` em `:1679`. |
| `app/chamados/models.py:3133`, `PerguntaDisputa.carimbar` | Só escreve se `resposta` foi solicitada, texto não é vazio e carimbo estava nulo (`:3131–3134`). Sempre salva materialização/autor/via (`:3129`); com `resposta='validar'`, acrescenta validação/autor quando nulos (`:3135–3138`); opcionalmente veredito/julgado/janela (`:3150–3153`). | A porta não registra decisão nem log de sucesso em seu corpo (`:3104–3158`); depende do chamador. |
| `app/chamados/models.py:1567`, `ressuscitar_perguntas` | Zera `respondida_em`, validação/autor e os três campos do carimbo por `descarimbar_em_lote`; update real em `:3265–3267`. Só seleciona resposta null/vazia em `:1487`. | Preserva decisões legadas em `:1564`; log `perguntas_ressuscitadas / ChamadoColaborador` em `:1590`, com pks em `:1593–1594`. |
| `app/chamados/services/disputa_emissao.py:1593`, `reabrir_para_correcao` | Zera conjuntamente `respondida_em`, validação/autor e texto; update em `:1632`, ou `descarimbar_em_lote` em `:1630` quando desfaz absorção. | Decisão com foto anterior em `:1615–1628`, ato padrão `reabertura`, podendo ser `recusa` pelo parâmetro (`:1562`, `:1597–1598`). |

Chamadores da porta `carimbar` que podem escrever `respondida_em`:

| Chamador | Assinatura |
|---|---|
| `app/chamados/services/materializacao.py:567`, `_materializar_ausencia_de_resposta` | `resposta='validar'`, via declaração ou ausência (`:555`); `DecisaoPergunta.ABSORCAO` com escape/ausência/texto em `:569–572`. |
| `app/chamados/services/materializacao.py:1283`, `_materializar_uma` | `resposta='validar'`, `lavrar_veredito=True`; via declaração ou via do fato (`:1267–1284`); não registra decisão/log de sucesso nesse trecho. |
| `app/chamados/services/disputa_emissao.py:1986`, resolução pelo encerramento | `resposta='validar'`, `lavrar_veredito=True`; via do fato. Não registra decisão/log por pergunta nesse ramo (`:1986–1990`). |
| `app/chamados/services/disputa_emissao.py:2060`, ramo sem credor | `resposta='responder'`; log `fecho_sem_credor / PerguntaDisputa` em `:2063`, antes/depois com `respondida_em`, materialização, via e presença de texto em `:2051–2052`, `:2068–2070`. |

Outras atribuições são somente em memória: cópia usada para julgar ressuscitação em `app/chamados/models.py:1509`; rollback da porta em `app/chamados/models.py:3164`.

API e command delegam: foto fornece texto fixo não vazio em `app/api/foto_recebida.py:25`, chama automático em `:48`; cura do limbo chama a porta pela trilha em `app/chamados/management/commands/curar_perguntas_em_limbo.py:67` e reabertura em `:69`. As migrations de dados consultadas escrevem identidade em `app/chamados/migrations/0051_decisao_pergunta.py:25–27` e verificam colisões em `0053_pergunta_unica_por_familia.py:36–46`; `respondida_em` aparece como criação de campo em `0014_perguntadisputa.py:25`.

Pontos decisivos para o casamento:

- `carimbo_desfeito_por_lastro` identifica o **desfazedor da materialização**, cujo log é emitido em `app/chamados/services/relavra_via.py:248`; a foto anterior só inclui via/materialização em `:244–245`, e a porta conserva resposta/validação conforme `:206–207`. Não identifica o escritor anterior de `respondida_em`.
- Via anterior `sem_turno_aberto` tampouco identifica escritor da resposta: `app/chamados/reconciliador.py:406` chama `carimbar` sem `resposta=...`; comentário documenta a retirada do antigo bloco em `:402–403`.
- Via `chamado_encerrado` não é assinatura exclusiva: pode sair de `_materializar_uma` em `app/chamados/services/materializacao.py:1274`, do ramo sem credor em `app/chamados/services/disputa_emissao.py:2059`, além da derivação por fato em `:1986`.
- Carimbos históricos sem decisões são previstos por `DecisaoPergunta.linhas_da_projecao_sem_linha`, que só projeta resposta atual/instante e motivo genérico em `app/chamados/models.py:3912–3935`. Essa projeção não recupera o escritor histórico.

Caminho do vazio:

- O core rejeita envio vazio (`disputa_emissao.py:1433`); a porta de materialização exige texto (`models.py:3131`); a cura pela trilha exige texto (`disputa_emissao.py:1670`).
- O automático aceita argumento vazio em sua assinatura/corpo (`disputa_emissao.py:1638–1656`), mas seu chamador encontrado entrega a constante não vazia (`api/foto_recebida.py:25`, `:48`).
- O apagador encontrado limpa texto **e** `respondida_em` juntos (`disputa_emissao.py:1593`), portanto não explica sozinho o par vazio/carimbado.
- `data_conferida_em` tem escritor separado, `app/chamados/services/validacao.py:664`, com save apenas de conferência/autor em `:666`.
- Atribuição histórica de cada uma das 143 a um desses caminhos: [não verificado].

## 12/11 14:52 · sessao 4b2f9e7f47af · - · - · origem=Ronald (tmux)

Conferência não-semente:

- **FECHADOS contratualmente:** G1 antes do primeiro comentário e guarda sem unicidade global de `<!--`; v6:6–7. G2/lápide, G3/`GERADO:FIM`, manual em 2371, autoria por zonas e cópia byte a byte são mantidos pela incorporação do v5; v6:5. A cópia continua com leitura/recomposição textual, a substituir na construção; `bin/gerar_backlog.py:111`, `:127`, `:138–140`.
- **FECHADA a omissão semântica do censo:** v6:12 declara expressamente que a coluna e o tachado deixam de excluir IDs da fonte; corresponde ao comportamento atual em `bin/hook_stop_fila1.py:168`, `:180`, `:250–252` e `bin/disjuntor.py:1634`.
- **FECHADOS segundo classificador e TRAVA/censo:** `_pertence_fila1` permanece; v6:4,10. O matcher atual está em `bin/disjuntor.py:1688–1695`. A v6:14 distingue APROVADO de autorização de construção; o código aprova o brief em `bin/disjuntor.py:1721–1727`.
- **Referência incorreta não bloqueia:** v6:8 menciona OBRAS:FIM em 2051 no vivo; nesta cópia está em `app/docs/BACKLOG.md:2042`. A própria v6:8 exige âncora por conteúdo; não há escolha contratual.
- **Referência estreita de selo não bloqueia:** v6:15 substitui `test_hook_estado_e_a_coluna.sh:53–59`, mas os casos completos abrangem `:51–63`, e o contador continua em `:80–108`. A obrigação “a coluna não move o topo; só o .ids move” na v6:15 permite migrar os pontos coerentemente sem decisão nova.

**Possível bloqueante por violação de aval já documentado:** APP continua após C20-O168 na cabeça; v6:26,30. `app/docs/BACKLOG.md:1916` registra o aval “depois da TRIAGEM, antes de O168/LASTRO”. A v6:30 admite a discrepância e reduz a frase a LASTRO alegando abertura anterior de O168. O anexo não fornece autorização nova de Ronald para esse corte; autorização: **[nao verificado]**. Se “aval literal” inclui esse aval citado pelo próprio contrato, é bloqueante; se a leitura pedida restringe o aval literal exclusivamente ao aval de origem reproduzido no pedido, deixá-lo como achado v5 ainda aberto.

O contrato v5 completo não foi encontrado na árvore pesquisada; as obrigações incorporadas por “como no v5” só puderam ser cotejadas com a leitura v5 fornecida, não com o documento integral: **[nao verificado]**.

_4313 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
