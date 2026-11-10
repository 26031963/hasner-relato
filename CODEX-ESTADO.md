# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 18:22** · publicado em **10/11/2026 18:22** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 18:22 · sessao a76dbf51bc37 · - · - · origem=Ronald (tmux)

- **Novo selo morde por inspeção:** testa zeros float/int/Decimal/`0E-7`, ausentes e parciais, exige ambos os vereditos na grade e rejeita nulidade (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:31`, `:65`, `:72`, `:82`). O filtro é provado por retorno sentinela e argumentos exatos (`:97`).
- **Mutante exigido existe:** cada leitor e cada casca têm controle verde, mutante de nulidade com coordenada válida ainda mostrando link, zero causando a mesma asserção vermelha e leitor vizinho intocado verde (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:128`). A substituição exige exatamente uma ocorrência da guarda; âncora inexistente não passa silenciosamente (`app/ponto/tests/test_bo0611_05_templates_geo.py:51`).
- **Duas cascas são renderizadas:** `CASCAS` enumera `base.html` e `base_app.html`; o loader substitui somente a base e usa templates reais. Feed usa página de ensaio, offline usa sua página consumidora; isso não equivale a smoke de clique em produção (`app/ponto/tests/test_bo0611_05_templates_geo.py:31`, `:100`, `:107`, `:145`).
- **Sem falha engolida pelo `subTest` nos mutantes críticos:** o novo selo e o de templates não abrem `subTest` dentro do método cuja falha é capturada; render e controles ficam fora de `assertRaises` (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:132`, `:137`; `app/ponto/tests/test_bo0611_05_templates_geo.py:190`). Nos testes puros antigos, os `subTest` são externos à asserção esperada (`app/ponto/tests/test_bo0611_05_leitores_geo.py:142`; `app/ponto/tests/test_l030_forma_js.py:88`).
- **Offline sair de `leitores_geo` não afrouxou a cobertura funcional:** o substituto extrai a expressão real de `com_gps` por AST, testa completos/zeros/parciais/mistura, mata nulidade e exige import do juiz sem redefinição (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:170`, `:184`, `:193`, `:198`). O selo antigo explica a transferência e mantém três sítios (`app/ponto/tests/test_bo0611_05_leitores_geo.py:3`, `:23`).
- **Contrato e API foram fortalecidos:** contrato compara o registro completo incluindo a nova autoridade; API continua provando uma escrita e coordenadas preservadas, acrescentando leitura de zero como `False` (`app/ponto/tests/test_contract_juiz_batida.py:164`; `app/api/tests/test_l030_batida_pela_porta.py:40`, `:56`).
- **Mudança do selo modal:** deixou de exigir `naoBloquear` quando não existe chamada `hxConfirmar`; permanece exigindo-o quando existe. A nova cobertura JS proíbe confirmação antes de POST/fila, inclusive com comentário `naoBloquear` enganoso (`app/core/tests/test_selo_modal_nao_nasce_vazio.py:135`; `app/ponto/tests/test_l030_forma_js.py:95`).
- **Executei 14 testes puros, todos verdes**, incluindo controles/mutantes de porta, JS, leitores e detector; os testes correspondentes estão em `app/api/tests/test_l030_selo_porta.py:68`, `app/ponto/tests/test_l030_forma_js.py:71`, `app/ponto/tests/test_bo0611_05_leitores_geo.py:114`, `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:88`. Resultado desta execução não tem arquivo de log por proibição de escrita: `[nao verificado]`. Django não está instalado neste ambiente; render/POST não foram reexecutados: `[nao verificado]`.
- **Ressalva da obra inteira:** o selo do detector exige que zero gere evidência de vínculo divergente, inclusive `0/0`; é divergência residual explicitamente codificada, anterior à volta 7, não mudança comprovada de semântica nesta volta (`app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:135`). Os selos analisados não estão entre os três testes listados em quarentena (`app/core/testes_em_quarentena.txt:6`).

## 10/11 18:22 · sessao 14926ad172a8 · - · - · origem=Ronald (tmux)

3. Os três leitores curados consultam o juiz: `_d2_feed.html:16`, `batidas_offline_pendentes.html:96`, `ponto/views.py:485`; filtro sem regra própria em `core/templatetags/hasner_filters.py:33-34`. O `load` está no próprio arquivo consumidor, então não depende da raiz: `_d2_feed.html:1`; offline:4. Todos os caminhos de produção encontrados entregam model: feed usa `list[Batida]` em `core/cell.py:9-20` e `core/views.py:1162-1171`; offline usa QuerySet de Batida em `ponto/views.py:441-446,489` e itera em template:53. Não há dict/values nesses caminhos. O filtro aceita apenas objetos com atributos; dict ou None causa AttributeError em `hasner_filters.py:34`. Não há catch na view offline ao renderizar (`ponto/views.py:488`) nem no render do feed (`core/live_cell.py:22,27-28`, `core/views.py:1171`). Framework Django instalado/propagação efetivamente executada: [nao verificado]. Se propagar, offline perde a resposta da página; feed perde a resposta do fragmento/JSON, enquanto o dashboard já aberto mostra “Erro ao carregar” no drawer (`dashboard2.html:169-174`), portanto não afirmar que sempre derruba toda a página.

4. Universo de `pendentes`: todas Batidas `origem='offline_pendente', aprovada=False`, ordenadas por timestamp; nenhum teto, paginação, período, filtro explícito por empresa ou colaborador nessa view (`ponto/views.py:441-446`). Há COUNT SQL total e iteração Python completa das coordenadas (`:484-485`), depois queryset completo para render (`:489`, template:53). `sem_gps = total - com_gps` usa a mesma decisão (`:486`); cards e linhas coerentes para mesmo conjunto estável (`template:19,23,27,96-100`). Concorrrência entre consultas e volume real: [nao verificado]. Não há filtros GPS próprios na página inspecionada (`templates/ponto/batidas_offline_pendentes.html:1-127`).

5. Permanecem predicados próprios do censo, por grupo:
- Truthiness das duas: drawer `colaboradores/services/drawer.py:63`; JS dashboard `templates/core/dashboard2.html:242,287`.
- Truthiness individual/serialização: geofence `:349-350,391-392`; painel `core/services/painel_op.py:227-228`; painel cell `colaboradores/painel_cell.py:54-55`; situacional `colaboradores/services/situacional.py:199-200`.
- Nulidade conjunta: `chamados/views.py:692`; `ponto/services/geofence.py:52,425`; `ponto/services/vinculo_na_lista.py:49-50`; `colaboradores/views.py:53,337,356,395,2474`; `colaboradores/services/detalhe.py:22,53`; templates modal:480 e relatório:30.
- Nulidade individual de serialização: `ponto/views.py:468-469`; `core/views.py:1024-1025`.
- `isNaN`: `static/js/geo-colab-map.js:39`; `templates/chamados/fio_colaborador.html:313`; `templates/chamados/painel_gestao.html:583`.
- Cópias AlertaGeofence: `ponto/models.py:263`; comando reconciliar:42; detalhe template:142; supervisor:90.

Há leitores na MESMA página do feed: o clique da linha abre drawer (`_d2_feed.html:4`), usando JSON com floats/nulos (`core/views.py:1024-1025`) e novo julgamento JS (`dashboard2.html:242,287`). Para floats usuais, inclusive zero, o resultado atual coincide; dependência do juiz está ausente nesses dois sítios. Não encontrou segundo julgamento GPS na tela offline (`template:1-127`).

Alimentam alerta/decisão: `geofence.vinculo_contagem` aceita zero por nulidade e calcula distância (`:52,58-61`), `vinculo_diverge` o consome (`:64-67`); `vinculo_na_lista` inclui zero e calcula centroides/distâncias/classificação (`:49-50,57,73-90`), lavra snapshot (`:112-122`) que o geofence lê por `_cadastro_em_revisao` (`geofence.py:70-83`). `reavaliar_chamados_geofence` inclui zero por nulidade (`:425`) e pode retratar chamados conforme distâncias (`:430-440`). Dinheiro diretamente a partir desses leitores: [nao verificado].

BACKLOG: busca por BO-0611-05, tem_gps ou censo_geo7 no arquivo encontrou somente a linha antiga de obra/censo1 (`app/docs/BACKLOG.md:1873`), sem linha específica das divergências v7. Ausência em outros destinos: [nao verificado].

8. Escritor de fato: `ponto/registro_batida.py:152-158`; sanitização `:5-18`, chamada antes da escrita `:105`. Valida via `float(v)` mas preserva valor original (`:15`); `0.0` fica intacto, vazio/nulo/não numérico vira None (`:11-17`). Semeadura também dentro da mesma porta, guardada para ensaio (`:331-335`). Encaminhadores com geo: `api/views.py:648,654-655`; `api/views_core.py:640,646-647`; `ponto/services/triagem_batida.py:701,707-708`. Campos model DecimalField sete casas (`ponto/models.py:58-59`).

Resultado nomeado `tem_gps`: consumidores produção em geofence:269, filtro:34, contador view:485. Outra chave `tem_gps` é produzida com conta própria em drawer:63, lida duas vezes em `templates/colaboradores/partials/_drawer_diagnostico.html:49`; não é chamada ao juiz.

Sim, leitores ainda aceitam zero:
- Detalhe seleciona por nulidade (`colaboradores/services/detalhe.py:22,53`), emite floats (`:82-83`) e oferece link (`templates/colaboradores/detalhe.html:188`) e mapa/pino (`:231-239`).
- `_geo_do_pino` só recusa nulos (`colaboradores/views.py:2474`), calcula distância de zero (`:2479`) e classifica (`:2486-2487`).
- Vínculo e reavaliação descritos acima.
- Teste NOVO da obra exige zero como amostra geográfica numérica no detector (`ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:134-139`).

Juiz aceita strings não vazias como truthy, inclusive `"0.0"`; essa é inferência direta do `bool(latitude and longitude)` (`geofence.py:146`). Model carregado do banco usa DecimalField (`models.py:58-59`); tipo exato de objeto recém-criado antes de refresh pode conservar float/string recebido porque sanitização não converte (`registro_batida.py:15`). Geofence principal avalia zero numérico como sem_gps e gera alerta com coordenadas None (`geofence.py:269-278`), contextos usam truthiness individual (`:349-350,391-392`). `floatdot` converte Decimal/float/string numérica para `float` e formata seis casas com ponto (`hasner_filters.py:22-25`); string inválida retorna intacta (`:26-27`). Logo não valida presença e não resolve `"0.0"` truthy; conversão para float pode perder precisão antes das seis casas (`:25`).

## 10/11 18:13 · sessao 05b0ac66455f · BO-DISPUTA-SEM-PERGUNTA · verificador · origem=Code

**Não liberaria a obra inteira ainda:** restam geração fora da transação do núcleo, reuso estrangeiro ignorado na agregação, derivação paralela do dia no auditor e ampliação de travas na recuperação. (`app/chamados/services/fabrica_da_celula.py:174`; `app/chamados/services/disputa_emissao.py:596`; `app/chamados/management/commands/auditar_invariantes_chamados.py:46`; `app/chamados/services/disputa_emissao.py:659`)

1. **Caminho (a): a geração principal foi protegida, mas ainda existe geração posterior ao commit do núcleo.** Nascimento e agregação chamam o gerador dentro de `atomic`. Porém o wrapper chama `perguntar_entradas_pendentes` depois desse retorno; ela também chama `gerar_perguntas_disputa`. (`app/chamados/services/disputa_emissao.py:534`, `:596`, `:625`, `:748`, `:294`)

   Existe caminho operacional sem transação externa: consumidor → fábrica → núcleo → segunda geração. A fábrica chama o wrapper em `:172` e diretamente o gerador em `:174`; o consumidor só abre seu próximo `atomic` depois dessas chamadas e captura eventual exceção sem desfazer o núcleo concluído. Portanto **sim**, pode haver DS commitada antes de uma geração posterior levantar; isso, sozinho, não demonstra que ela ficou sem pergunta. (`app/chamados/services/fabrica_da_celula.py:172`, `:174`, `:219`, `:301`; `app/chamados/reconciliador.py:1387`, `:1388`, `:1392`)

   Censo dos chamadores operacionais diretos:

   | Porta | Chamador | Transação da pergunta |
   |---|---|---|
   | `_abrir_ou_agregar_disputa` | Recuperação recursiva, `app/chamados/services/disputa_emissao.py:659` | Geração dentro dos atomics da recuperação e do núcleo, `:651`, `:596`. |
   | Mesma | `disputa_para_declaracao`, `app/chamados/services/disputa_emissao.py:691` | Desliga geração, `:695`; consumidores criam a pergunta na transação deles. |
   | Mesma | Wrapper, `app/chamados/services/disputa_emissao.py:727` | Principal dentro; pendentes depois, `:748`. |
   | Mesma | Grade, `app/ponto/management/commands/reconciliar_grade.py:424` | Passa motivos vazios, `:425`; pode nascer sem pergunta, pois o laço não executa, `app/chamados/services/disputa_emissao.py:312`. |
   | Mesma | Smoke, `app/chamados/management/commands/smoke_chamados.py:99` | Repete geração em `:102`, mas tudo está no atomic externo com rollback, `:39`, `:43`. |
   | `criar_ou_atualizar_disputa` | APIs, `app/api/views.py:794`; `app/api/views_core.py:794` | Principal no núcleo; ressalva dos pendentes em `app/chamados/services/disputa_emissao.py:748`. |
   | Mesma | PWA, `app/ponto/services/triagem_batida.py:734` | Mesma situação do wrapper. |
   | Mesma | Retro de selfie, `app/ponto/management/commands/marcar_foto_ausente_retro.py:96` | Mesma situação do wrapper. |
   | Mesma | Fábrica, `app/chamados/services/fabrica_da_celula.py:172` | Segunda geração explícita em `:174`. |

   Os consumidores da declaração mantêm DS e pergunta na mesma transação: `declarar_hora_colab` abre atomic, chama a porta e cria pergunta; `aplicar_declaracao` faz o mesmo. (`app/chamados/services/declaracao_texto.py:348`, `:360`, `:376`, `:654`, `:659`, `:666`)

   Os outros nascimentos operacionais são: **agrupador**, DS e perguntas dentro do atomic, preservando os `continue` sem ata; **âncora**, DS e `abrir_conversa` dentro do mesmo atomic. Ambos continuam caminhos próprios; a âncora não inspeciona o retorno estrangeiro da porta. A grade usa o núcleo, sem quarto `objects.create`. (`app/chamados/services/disputa_emissao.py:1115`, `:1186`, `:1216`, `:1238`, `:1276`, `:1307`, `:1385`, `:1421`, `:1434`; `app/ponto/management/commands/reconciliar_grade.py:424`)

   Chamadas diretas adicionais em testes estão em `app/chamados/tests/test_3b_reuso_746.py:23`, `:31`; `test_a2_batida_sem_linha.py:30`; `test_a2_data_evento_batida.py:24`, `:36`, `:43`, `:49`, `:54`; `test_bo_disputa_sem_pergunta.py:29`; `test_e1_fila_v2_2_integracao.py:132`; `test_e1_v6_horizonte.py:268`, `:269`, `:275`, `:451`, `:452`; `test_e1_volta2.py:171`; `test_plantio_noturno_mais1.py:206`, `:213`; `test_regressao_etapa7.py:226`, `:241`, `:244`, `:260`, `:264`; `test_smoke_ciclo_disputa.py:69`, `:70`; `app/ponto/tests/test_selfie_ausente_juiz.py:122`. A execução ORM dessas chamadas é [nao verificado].

2. **Caminho (b): correto no nascimento; incompleto na agregação.** No nascimento, o detector identifica perguntas de outras disputas e levanta dentro do atomic. A captura fica fora dele; depois reentra no ramo existente de agregação para cada dona. Isso desfaz o ensaio antes da agregação definitiva. (`app/chamados/services/disputa_emissao.py:624`, `:628`, `:636`, `:648`, `:651`, `:659`)

   A dona **fechada** é selecionada explicitamente, sem filtro de aberta. O ramo salva somente motivos/contexto, preservando `fechada_em` e a FK da pergunta. Não introduz segundo escritor desses campos fora da porta. (`app/chamados/services/disputa_emissao.py:562`, `:570`, `:591`, `:594`, `:659`; `app/chamados/models.py:3008`)

   **Falha residual:** quando já existe DS aberta, o ramo ignora o retorno estrangeiro do gerador. Uma aberta vazia — possível pela grade — recebe o motivo, enquanto a dona da canônica não recebe essa agregação. A sonda com funções reais extraídas por AST e doubles confirmou aberta com motivo, dona sem ele e I7a=1; PostgreSQL/signals são [nao verificado]. (`app/chamados/services/disputa_emissao.py:564`, `:570`, `:596`, `:597`; `app/ponto/management/commands/reconciliar_grade.py:424`; infraestrutura da sonda em `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:82`)

3. **Batida: ficou fora do atomic novo; sucesso da resposta não está protegido.** As APIs registram a batida antes de chamar a disputa; o atomic novo é interno ao núcleo. O PWA também registra primeiro. (`app/api/views_core.py:681`, `:794`; `app/api/views.py:684`, `:794`; `app/ponto/services/triagem_batida.py:701`, `:734`; `app/chamados/services/disputa_emissao.py:534`)

   O chokepoint insere a batida e protege a etapa posterior de fechamento contra exceções. Já as chamadas de disputa nas APIs não possuem captura envolvente, e o núcleo captura somente `_PerguntaDeOutraDisputa`: uma exceção do gerador pode impedir a resposta normal depois da gravação. Portanto o patch não envolve a batida no rollback novo, mas também não prova “batida nunca barrada” como sucesso HTTP. Persistência efetiva/HTTP em runtime são [nao verificado]. (`app/ponto/registro_batida.py:157`, `:175`, `:179`; `app/api/views_core.py:794`; `app/api/views.py:794`; `app/chamados/services/disputa_emissao.py:648`)

4. **HX-ATA-NAO-SABE: comportamento preservado no código atual; igualdade byte a byte com a base [nao verificado].** A cópia não fornece histórico Git para comparação independente; o patch anexado não declara hunks no agrupador nem no teste HX, enquanto as entregas alegam comparação AST do agrupador — isso não equivale a comparação de bytes. [nao verificado] [nao verificado]

   A abstenção atual continua marcando falta de ata e evitando o fechamento; o teste continua exigindo zero perguntas, disputa aberta e WARNING `auto_fecho_disputa`, com cenários de contraste com ata. (`app/chamados/services/disputa_emissao.py:1234`, `:1235`, `:1272`, `:1273`, `:1303`, `:1304`, `:1342`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51`, `:58`, `:63`, `:77`)

5. **I7a/I7b: universo e estado da ata estão corretos; a fonte do dia não está inteiramente unificada.** O filtro significa DS aberta com **zero linhas** de pergunta, sem filtro de vitalidade. “SEM ata” usa literalmente `_marcos_faltantes(...)[0] == 'sem_celula'`; dia indeterminável e `fora_vinculo` caem em I7a. Não nasceu outro predicado de presença de ata. (`app/chamados/management/commands/auditar_invariantes_chamados.py:67`, `:73`, `:74`; `app/chamados/services/declaracao_texto.py:91`)

   Para órfãos identificados por IDs, reaproveita os leitores dos ramos correspondentes: `data_do_chamado`, `resolver_data_evento` com batida-origem e `data_evento_do_contexto`. Não troca diretamente pelo dia civil da abertura. (`app/chamados/management/commands/auditar_invariantes_chamados.py:33`; `app/chamados/services/disputa_emissao.py:1232`, `:1268`, `:1295`)

   **Há derivação paralela fora desse ramo:** o gerador pode resolver por `_data_furo_da_grade`, que funde contexto do pai e prioriza `entrada_sem_saida.entrada_id`/`entrada_aberta_em`. O auditor ignora essas fontes e segue para `ultima_batida` ou pai/contexto. Além disso, havendo data explícita em algum motivo, retorna imediatamente, enquanto o gerador resolve separadamente cada motivo. (`app/chamados/services/disputa_emissao.py:53`, `:59`, `:68`, `:70`, `:346`, `:371`; `app/chamados/management/commands/auditar_invariantes_chamados.py:48`, `:50`, `:51`, `:58`)

   A divergência foi reproduzida com AST real e doubles: origem no turno 08/07, contexto com 09/07; fonte do gerador devolve 08/07 e auditor consulta 09/07. Assim pode classificar como I7b pela ata do dia diferente. Ocorrência em produção é [nao verificado]. (`app/chamados/services/disputa_emissao.py:68`, `:76`; `app/chamados/management/commands/auditar_invariantes_chamados.py:58`, `:74`)

6. **QuerySet: uma avaliação no caminho novo; o risco apontado não é dupla avaliação.** `_universo` fica verdadeiro, portanto `if _universo or _pks(disputas)` faz curto-circuito: `_pks` só itera `disputas` em `:2671`. Não há duas avaliações entre as quais uma dona possa nascer nesse caminho. Mesmo fora dele, reiterar o mesmo QuerySet normal pode usar seu cache; comportamento Django em runtime aqui é [nao verificado]. (`app/chamados/services/disputa_emissao.py:553`; `app/chamados/models.py:2669`, `:2671`, `:2674`)

   A entrada inicial pede C → IDs → D por PK → Ch por PK; perguntas entram depois pela porta. (`app/chamados/models.py:2666`, `:2675`, `:2688`, `:2692`)

   **A recuperação deixa uma lacuna na ordem por transação:** primeiro trava donas retornadas + abertas + chamados; a recursão volta a pedir **todas** as DS. Uma fechada adicional pode entrar depois de Ch já travado. A sonda da volta 2 não cobre esse caminho: prepara aberta e substitui o gerador por Mock. (`app/chamados/services/disputa_emissao.py:652`, `:659`, `:551`; `app/chamados/models.py:2688`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:29`, `:133`)

   Isso não demonstra deadlock: C retido serializa escritores conformes. Deadlock efetivo com outro escritor, ou prova global de ausência dele, é [nao verificado]. A disciplina declarada é C primeiro e ordem crescente por tabela. (`app/chamados/models.py:2631`, `:2666`)

7. **Tetos/C8: justificativa corresponde à leitura adicional, mas não certifica os totais.** Existe a seleção de PKs de todas as DS do colaborador, inclusive fechadas, depois da trava C; a justificativa está junto dos dois tetos, como exige C8. (`app/chamados/services/disputa_emissao.py:551`; `app/chamados/models.py:2666`, `:2671`; `app/ponto/tests/test_selo_chokepoint_escrita.py:180`, `:193`; `PLANO_PISCADA.md:10`)

   É uma consulta adicional **por invocação do núcleo**. Linhas transferidas/travadas crescem com o histórico de DS; custo total não é constante. A fábrica também pode chamar a porta várias vezes, em laço por motivo. (`app/chamados/services/disputa_emissao.py:551`; `app/chamados/services/fabrica_da_celula.py:168`, `:172`)

   A sonda varia chamados vivos, mantém duas DS e simula consultas; não mede o SQL completo de `registrar_batida`. Totais finais **145/219 são [nao verificado]**. (`app/chamados/tests/test_bo_disputa_volta2_memoria.py:24`, `:28`, `:30`, `:133`, `:142`)

8. **Os erros do RED informado não provam o bug (a).** ImportError/KeyError do contador provam ausência do símbolo; não observam I7a=1. O teste ORM importa esse símbolo na carga, e os testes de exceção em memória consultam o contador antes das contagens diretas. (`app/chamados/tests/test_bo_disputa_sem_pergunta.py:9`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:280`, `:296`)

   O modo `--base` atual usa **emissor antigo + auditor novo como sonda**, resolvendo essa deficiência metodológica. Nesse arranjo, o teste de exceção antes da pergunta confronta I7a com zero em `:280`; o teste após inserção confronta diretamente DS/pai/pergunta/log em `:297`; o teste de profundidade confronta `[1,1]` em `:381`. Reprodução desse modo contra o Git da base nesta cópia é [nao verificado]. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:28`, `:201`, `:204`, `:275`, `:285`, `:381`)

   AST recompila os **corpos extraídos da cópia**, sem reimplementação manual do núcleo, gerador ou contador; entretanto ORM, atomic, juiz da ata e porta da pergunta são doubles. Isso testa lógica Python, sem certificar SQL, signals ou concorrência. Os wrappers exigem controle verde e morte por assertion, recusando erro de montagem. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:36`, `:69`, `:88`, `:176`, `:224`, `:449`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:174`)

   Há testes com mordida positiva — pergunta existe antes da falha, lote exige DS=2/PD=3 — e um teste fraco: declaração apenas exige `assert_not_called`, passando se a função virar no-op. A sonda de travas ignora predicados no double e devolve sempre as duas DS, portanto não comprova a seleção SQL. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:290`, `:336`, `:384`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:74`, `:84`)

9. **Escritores e leitores dos campos centrais:**

   | Campo | Escritores | Leitores relevantes |
   |---|---|---|
   | `DS.motivos` | Núcleo, agrupador, âncora; portas de remoção/reabertura. (`app/chamados/services/disputa_emissao.py:570`, `:620`, `:1146`, `:1194`, `:1318`, `:1429`; `app/chamados/models.py:1966`, `:2063`, `:2159`) | Gerador, auditor e relatório. (`app/chamados/services/disputa_emissao.py:312`; `app/chamados/management/commands/auditar_invariantes_chamados.py:48`; `app/chamados/relatorios_disputa.py:52`) |
   | `DS.contexto` | Núcleo, pendentes, agrupador/âncora, remoção/anotar/renovar. (`app/chamados/services/disputa_emissao.py:591`, `:621`, `:293`, `:1189`, `:1424`; `app/chamados/models.py:1967`, `:2064`, `:2104`, `:2122`) | Fonte da grade, gerador, pendentes e auditor. (`app/chamados/services/disputa_emissao.py:54`, `:273`, `:345`, `:372`, `:387`; `app/chamados/management/commands/auditar_invariantes_chamados.py:29`) |
   | `DS.fechada_em` | Fechar e reabrir admin/sistema. (`app/chamados/models.py:1932`, `:2149`, `:2173`) | Seleção do núcleo, auditor e S127. (`app/chamados/services/disputa_emissao.py:558`, `:562`; `app/chamados/management/commands/auditar_invariantes_chamados.py:69`; `app/chamados/juizes.py:2316`) |
   | `DS.chamado` | Três nascimentos; FK admite SET_NULL. (`app/chamados/services/disputa_emissao.py:622`, `:1190`, `:1425`; `app/chamados/models.py:1749`) | Auditor, fonte da grade, S127 e signal do pai. (`app/chamados/management/commands/auditar_invariantes_chamados.py:30`; `app/chamados/services/disputa_emissao.py:55`; `app/chamados/juizes.py:2316`; `app/chamados/signals.py:32`) |
   | `Pergunta.disputa` | `_nascer`; reuso devolve existente preservando FK. (`app/chamados/models.py:3014`, `:3008`) | Detector estrangeiro, auditor pela relação inversa, S127 e relatório. (`app/chamados/services/disputa_emissao.py:628`; `app/chamados/management/commands/auditar_invariantes_chamados.py:69`; `app/chamados/juizes.py:2319`; `app/chamados/relatorios_disputa.py:12`) |
   | Saída do auditor | `add`, apresentação stdout e HTML. (`app/chamados/management/commands/auditar_invariantes_chamados.py:636`, `:1915`, `:1930`) | Cron pede HTML; placar depende da execução. Leitura humana efetiva é [nao verificado]. (`app/config/crons.py:542`, `:747`) |

   As chaves de origem também têm produtores existentes: API escreve `ultima_batida` e `entrada_sem_saida`; agrupador escreve `chamados_orfaos_ids`; fábrica escreve data por motivo. O auditor lê essas informações parcialmente, conforme o conflito do item 5. (`app/api/views_core.py:669`, `:788`; `app/chamados/services/disputa_emissao.py:1179`, `:1189`; `app/chamados/services/fabrica_da_celula.py:172`; `app/chamados/management/commands/auditar_invariantes_chamados.py:33`, `:48`, `:51`)

   **Pergunta morta contar como linha não conflita com os leitores existentes:** relatório distingue lista vazia de lista sem vivas; fechamento distingue `VAZIA` de `VEREDITO`; S127 bloqueia zero linhas ou fio vivo. O conflito encontrado é a escolha do dia, não esse significado de “sem pergunta”. (`app/chamados/relatorios_disputa.py:12`, `:18`; `app/chamados/models.py:1903`; `app/chamados/juizes.py:2320`)

10. **Classes reprovadoras e metadados.** Estão presentes a **derivação paralela de fonte única** e a **lacuna na ordem de trava da recuperação**; há ainda geração posterior ao núcleo e cobertura incompleta do reuso na agregação. (`app/chamados/management/commands/auditar_invariantes_chamados.py:46`; `app/chamados/services/disputa_emissao.py:652`, `:659`, `:596`; `app/chamados/services/fabrica_da_celula.py:174`)

   Não aparece novo predicado concorrente de “SEM ata”, nem segundo escritor de motivos no tratamento novo: ele chama o juiz existente e reentra na porta existente. Também não aparece leitura nova de relógio na classificação I7a/I7b; `ultima_em` continua sendo timestamp da agregação. (`app/chamados/management/commands/auditar_invariantes_chamados.py:74`; `app/chamados/services/disputa_emissao.py:659`, `:587`)

   **Linhas LEI-AKITA dos commits, ordem real de pouso e cumprimento do pré-requisito BO-0611-05: [nao verificado].** O molde exigido inclui origem, testemunha, RED, leitores, juízes e padrão; o ciclo exige suite e revisão antes do pouso. (`CLAUDE.md:62`, `:63`, `:65`; `app/docs/LEIS.md:147`)

   Para fundamentar o pouso, cabem **L-001/L-002** — origem e fonte única —, **L-003** — medição —, **L-007** — escritor único —, **L-030** — batida — e **L-123/L-126** — padrão e certificação. O índice fornecido também aponta **L-050**, cuja fronteira exportada não pode ser reintroduzida. (`app/docs/LEIS.md:59`, `:60`, `:61`, `:65`, `:78`, `:91`, `:144`, `:147`)

   Isso é diferente da cobrança automática por sítio: o selo exige citação quando o hunk cruza função da coluna **PROTEGE**; uma menção ao arquivo em DOSSIES/CORTES não basta para afirmar cobrança mecânica. Resultado desse selo para o pouso é [nao verificado]. (`bin/tests/test_lei_protege_sitio.sh:13`, `:77`, `:83`, `:176`, `:180`)

   Não identifiquei afirmação específica de **LEIS.md/JUIZES.md que a cura torne falsa**; identifiquei código novo contrariando a exigência de fonte única. JUIZES continua sendo lookup, com correção obrigatória se divergir do código; não justifica cadastrar outro juiz para legitimar a derivação paralela. (`app/docs/LEIS.md:60`; `app/docs/JUIZES.md:7`, `:14`; `app/chamados/management/commands/auditar_invariantes_chamados.py:46`)

## 10/11 18:11 · sessao 9d5fc4a2156d · - · - · origem=Ronald (tmux)

Achado principal: há derivação paralela do dia no auditor, com divergência reproduzida em memória. O comentário “Mesma precedencia do gerador” não corresponde integralmente ao código: auditor usa datas explícitas → `ultima_batida` da DS → dia forte do pai/contexto; gerador pode antes usar `_data_furo_da_grade`, que funde contexto do pai e prioriza `entrada_sem_saida.entrada_id`/`entrada_aberta_em`. (`app/chamados/management/commands/auditar_invariantes_chamados.py:46`; `app/chamados/services/disputa_emissao.py:53`; `app/chamados/services/disputa_emissao.py:68`; `app/chamados/services/disputa_emissao.py:346`.)

Executei AST real de `_data_furo_da_grade` e `_dias_da_disputa_sem_pergunta`, com ORM/turno/ata doubles do próprio teste: DS com `entrada_sem_saida.entrada_id` apontando turno 08/07 e `data_turno=09/07`; fonte do gerador respondeu 08/07, auditor respondeu 09/07. Com juiz double `sem_celula` apenas em 09/07, contador publicou `(0,1)`. A cena depende dos doubles; ocorrência em produção [nao verificado]. A divergência decorre das linhas citadas e do fallback final do auditor. (`app/chamados/management/commands/auditar_invariantes_chamados.py:51`; `app/chamados/management/commands/auditar_invariantes_chamados.py:58`; `app/chamados/services/disputa_emissao.py:76`.)

Há outra diferença: se **algum** motivo tem data explícita, auditor retorna imediatamente o conjunto dessas datas e ignora o fallback para motivos sem data; gerador decide data separadamente dentro do laço de motivos. (`app/chamados/management/commands/auditar_invariantes_chamados.py:48`; `app/chamados/services/disputa_emissao.py:312`; `app/chamados/services/disputa_emissao.py:371`.)

4. Integridade HX:

- A cópia não possui `.git`; comparação direta byte a byte contra base não foi possível [nao verificado].
- O `diff_obra.patch` fornecido não contém hunk no agrupador nem no teste HX; isso demonstra apenas que o anexo não declara alteração ali. Base independente e igualdade byte a byte [nao verificado]. [nao verificado]
- O código atual mantém a abstenção: `_ata_do_dia` lê `_marcos_faltantes`, com `sem_celula` e escala vigente acende/recusa; agrupador marca `_absteve_por_ata` e evita fecho quando ela estiver marcada. (`app/chamados/services/disputa_emissao.py:958`; `app/chamados/services/disputa_emissao.py:965`; `app/chamados/services/disputa_emissao.py:1234`; `app/chamados/services/disputa_emissao.py:1272`; `app/chamados/services/disputa_emissao.py:1303`; `app/chamados/services/disputa_emissao.py:1342`.)
- O teste atual exige zero perguntas, DS aberta e WARNING `auto_fecho_disputa`; há contraste com ata, dia completo e fechamento. (`app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:58`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:63`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:77`.)

5. Contadores:

- Universo correto para zero **linhas**: `fechada_em IS NULL` e relação `perguntas IS NULL`; pergunta morta continua excluindo DS desse universo, pois nenhuma coluna de vitalidade é filtrada. (`app/chamados/management/commands/auditar_invariantes_chamados.py:67`.)
- “SEM ata” é literalmente `_marcos_faltantes(...)[0] == 'sem_celula'`, sem novo julgamento de presença de ata; `fora_vinculo` e dia indeterminável ficam no I7a. (`app/chamados/management/commands/auditar_invariantes_chamados.py:70`; `app/chamados/services/declaracao_texto.py:91`.)
- Para IDs dos órfãos, usa leitores existentes: `cobra_marcos_da_ata` + `data_do_chamado`; batida origem + `resolver_data_evento`; senão `data_evento_do_contexto`. São os leitores usados nos ramos correspondentes do agrupador, com a observação de que o auditor chama resolver com motivo fixo `orfao_14h`; resolver aceita os dois motivos e delega ao mesmo turno. (`app/chamados/management/commands/auditar_invariantes_chamados.py:33`; `app/chamados/services/disputa_emissao.py:1232`; `app/chamados/services/disputa_emissao.py:1268`; `app/chamados/services/disputa_emissao.py:1295`; `app/escala/servico_jornada.py:474`.)
- Isso não reproduz literalmente o booleano de abstenção: agrupador passa por regularizabilidade, base, classificação e exceção “sem escala vigente”; contador pergunta apenas estado atual do juiz para todos os IDs existentes. A especificação dada exige estado `sem_celula`, portanto não proponho adicionar os gates ao contador. (`app/chamados/services/disputa_emissao.py:1129`; `app/chamados/services/disputa_emissao.py:1263`; `app/chamados/services/disputa_emissao.py:1298`; `app/chamados/services/disputa_emissao.py:971`; `app/chamados/management/commands/auditar_invariantes_chamados.py:73`.)
- I7a continua violação; I7b sempre INFO, inclusive zero, e HTML usa cor neutra/cabeçalho “numero (I7b: publicado)”. (`app/chamados/management/commands/auditar_invariantes_chamados.py:77`; `app/chamados/management/commands/auditar_invariantes_chamados.py:81`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1915`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1928`.)

9. Inventário dos campos no caminho analisado:

| Campo | Escritores | Leitores relevantes |
|---|---|---|
| `DS.motivos` | núcleo agrega/insere (`app/chamados/services/disputa_emissao.py:570`, `:620`); agrupador/âncora (`:1146`, `:1188`, `:1194`, `:1223`, `:1318`, `:1423`, `:1429`); portas aparar/remover/reabrir (`app/chamados/models.py:1966`, `:2063`, `:2159`) | gerador usa lista recebida (`app/chamados/services/disputa_emissao.py:312`); auditor deriva dias dos motivos (`app/chamados/management/commands/auditar_invariantes_chamados.py:48`); relatório imprime rótulos (`app/chamados/relatorios_disputa.py:52`) |
| `DS.contexto` | núcleo merge/nascimento (`app/chamados/services/disputa_emissao.py:577`, `:591`, `:621`); entradas pendentes (`:293`); agrupador/âncora (`:1189`, `:1424`); portas de remoção/anotar/renovar (`app/chamados/models.py:1967`, `:2064`, `:2104`, `:2122`) | gerador/fonte da grade (`app/chamados/services/disputa_emissao.py:54`, `:345`, `:372`, `:387`); entradas pendentes (`:273`); auditor (`app/chamados/management/commands/auditar_invariantes_chamados.py:29`) |
| `DS.fechada_em` | fechamento (`app/chamados/models.py:1932`); reaberturas admin/sistema (`:2149`, `:2173`) | núcleo seleciona abertas ou dona explícita (`app/chamados/services/disputa_emissao.py:558`, `:562`); auditor (`app/chamados/management/commands/auditar_invariantes_chamados.py:69`); S127 (`app/chamados/juizes.py:2316`); signal do pai (`app/chamados/signals.py:28`) |
| `DS.chamado` | três nascimentos do emissor (`app/chamados/services/disputa_emissao.py:622`, `:1190`, `:1425`); FK admite SET_NULL (`app/chamados/models.py:1749`) | auditor (`app/chamados/management/commands/auditar_invariantes_chamados.py:30`); fonte da grade (`app/chamados/services/disputa_emissao.py:55`); S127 (`app/chamados/juizes.py:2316`); signal encerra pai (`app/chamados/signals.py:32`) |
| `Pergunta.disputa` | `_nascer` insere (`app/chamados/models.py:3014`); reuso retorna `ex` sem mudar FK (`:3008`) | detector estrangeiro (`app/chamados/services/disputa_emissao.py:628`); relação inversa do auditor (`app/chamados/management/commands/auditar_invariantes_chamados.py:69`); S127 (`app/chamados/juizes.py:2319`); relatório (`app/chamados/relatorios_disputa.py:12`) |
| saída auditor | `add` monta total/amostra (`app/chamados/management/commands/auditar_invariantes_chamados.py:636`); stdout/HTML (`:1915`, `:1930`) | cron publica com `--html` (`app/config/crons.py:542`); placar depende da execução do auditor (`app/config/crons.py:747`); leitor humano efetivo [nao verificado] |

Leitores existentes já distinguem zero linhas de perguntas mortas: relatório retorna SEM_PERGUNTA só com lista vazia, PRONTA_FECHAR quando há linhas mas nenhuma viva; fechamento usa `vazia` só para zero linhas; S127 bloqueia zero linhas ou fio vivo. Portanto não há conflito nesse significado do novo contador. (`app/chamados/relatorios_disputa.py:12`; `app/chamados/relatorios_disputa.py:18`; `app/chamados/models.py:1903`; `app/chamados/juizes.py:2320`.)

A derivação paralela de dia descrita acima é o conflito concreto encontrado; exaustividade de todo escritor dinâmico/JSON e leitores externos [nao verificado].

## 10/11 18:10 · sessao 5b97b39adce8 · - · - · origem=Ronald (tmux)

Sonda em memória executada sem criar arquivo, com `python3 -B`, rc=0; usa `Ambiente` e funções extraídas da cópia por AST (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:82`, `:213`).

Resultado confirmado por assertions:

- Dona fechada `pk=1`: motivos continuam `['foto_ausente']`.
- DS aberta inicialmente vazia `pk=2`: recebe `['batida_fora_escala_grave']`.
- Retorno da emissão: DS `pk=2`.
- Pergunta canônica permanece ligada à dona `pk=1`.
- Total DS: 2; contadores `(I7a, I7b) = (1, 0)`.

A causa é o ramo de agregação ignorar o retorno do gerador (`app/chamados/services/disputa_emissao.py:596`) e retornar a DS aberta (`:597`); o detector de dona estrangeira existe apenas no nascimento (`:624`, `:628`, `:636`). Portanto há classe residual de (b) quando a DS aberta já existe vazia; a nova DS não nasce nessa chamada, mas o motivo fica na DS errada e I7a acusa 1 (`app/chamados/services/disputa_emissao.py:564`, `:570`, `:596`).

PostgreSQL, signals e concorrência: [nao verificado].

## 10/11 18:10 · sessao 6a680ef2adf3 · - · - · origem=Ronald (tmux)

Itens 6–8, referências relativas à raiz.

**6. QuerySet e ordem**

- No caminho novo, `_pks(disputas)` executa **uma vez**: o núcleo passa `abertas_do_colaborador=True` e um colaborador, tornando `_universo=True`; a expressão `if _universo or _pks(disputas)` faz curto-circuito. Portanto a ocorrência de `:2674` não reitera o QuerySet nesse caminho. Não existem duas avaliações entre as quais uma dona possa nascer. `app/chamados/services/disputa_emissao.py:553`; `app/chamados/models.py:2669`; `app/chamados/models.py:2671`; `app/chamados/models.py:2674`.
- A sequência inicial é C → leitura de IDs → D ordenadas por pk → Ch ordenados por pk; P entra posteriormente pela porta. `app/chamados/models.py:2666`; `app/chamados/models.py:2675`; `app/chamados/models.py:2688`; `app/chamados/models.py:2692`.
- **Ressalva concreta na ordem por transação:** o tratamento de reuso trava somente donas retornadas + abertas + Ch vivos; a recursão amplia novamente para **todas** as DS, inclusive fechadas não retornadas. Havendo outra DS fechada X, a recursão pode adquirir X depois de Ch já adquirido no tratamento. Isso contradiz a ordem literal global por transação. `app/chamados/services/disputa_emissao.py:651`; `app/chamados/services/disputa_emissao.py:653`; `app/chamados/services/disputa_emissao.py:659`; `app/chamados/services/disputa_emissao.py:551`; `app/chamados/models.py:2688`.
- Não decorre automaticamente um deadlock: C continua retido e serializa os escritores que seguem a mesma disciplina. Deadlock real ou escritor concorrente que contorne C: [nao verificado]. A porta declara C primeiro e ordenação crescente por tabela. `app/chamados/models.py:2631`; `app/chamados/models.py:2666`.
- A sonda da volta 2 não cobre essa reentrada: cria DS aberta para entrar em agregação e substitui o gerador por Mock. `app/chamados/tests/test_bo_disputa_volta2_memoria.py:29`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:133`.

**7. Tetos/C8**

- A justificativa corresponde à expressão ORM real: `values_list('pk', flat=True)` de **todas** as DS do colaborador, avaliada após C. `app/chamados/services/disputa_emissao.py:551`; `app/chamados/models.py:2666`; `app/chamados/models.py:2671`; `app/ponto/tests/test_selo_chokepoint_escrita.py:180`; `app/ponto/tests/test_selo_chokepoint_escrita.py:193`.
- É uma leitura adicional **por chamada do núcleo**; não é custo constante em linhas transferidas/travadas, pois cresce com o histórico de disputas. Também não significa uma única chamada por julgamento: a fábrica chama o núcleo em laço por motivo. `app/chamados/services/disputa_emissao.py:551`; `app/chamados/services/fabrica_da_celula.py:168`; `app/chamados/services/fabrica_da_celula.py:172`.
- A sonda varia quantidade de Ch vivos, mantém duas DS e simula as consultas; prova número de avaliações nesse recorte, não os totais SQL 145/219. `app/chamados/tests/test_bo_disputa_volta2_memoria.py:24`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:28`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:30`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:142`. Totais SQL finais: [nao verificado].
- C8 exige justificativa escrita junto ao número; isso existe. `PLANO_PISCADA.md:10`; `app/ponto/tests/test_selo_chokepoint_escrita.py:180`; `app/ponto/tests/test_selo_chokepoint_escrita.py:193`.

**8. Mordida e limites**

- O RED informado de ImportError/KeyError comprova falta do símbolo, **não** a observação de I7a=1 para o bug (a). O teste ORM importa o símbolo durante carga; os testes de exceção em memória consultam o contador antes de conferir as tabelas diretamente. `app/chamados/tests/test_bo_disputa_sem_pergunta.py:9`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:280`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:296`.
- O modo `--base` atual resolve esse problema metodológico ao carregar somente o emissor antigo e manter o auditor atual como sonda. Nesse arranjo, `test_a_excecao_do_gerador_desfaz_agregado_e_preserva_batida` pode falhar por ASSERT em `:280`, observando `(1,0)` contra `(0,0)`; o teste após inserção pode falhar nas contagens `:297`. Isso é prova híbrida explicitada, não execução integral da base. `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:28`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:201`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:204`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:275`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:285`.
- Os testes extraem e recompilam **o corpo real do código**, sem reimplementação manual do núcleo/gerador/contador. Criam novos objetos-função em namespace substituído: atomic, ORM, juiz da ata e porta P são doubles. Não certificam signals, SQL nem concorrência PostgreSQL. `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:36`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:69`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:88`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:176`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:224`.
- A sonda volta 2 extrai a implementação real de `trava_na_ordem`, mas `Trava.filter` ignora o predicado e o resultado D contém sempre as duas DS fabricadas. Portanto comprova ordem de chamadas e argumentos, não que o SQL efetivamente seleciona o conjunto correto. `app/chamados/tests/test_bo_disputa_volta2_memoria.py:74`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:84`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:97`.
- Não há vacuidade geral nos cenários principais: existe assert positivo de pergunta antes da exceção e o lote exige DS=2/PD=3. `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:290`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:336`.
- **Teste individual fraco:** `test_declaracao_delega_pergunta_ao_consumidor` só exige `gerar.assert_not_called()`, sem conferir nascimento/retorno; passaria se o wrapper virasse no-op. A checagem estática de settings usa glob sem assert de universo não vazio, embora os arquivos existam nesta cópia. `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:384`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:427`.

Executei somente Python `-B`: controles atuais passaram 14+2 testes; mutantes gerador/reuso/contador foram mortos com 5/2/5 assertions e trava repetida/consulta antecipada com 3/1 assertions. Evidência de execução desta sessão sem arquivo persistido: [nao verificado]. Os wrappers exigem controle verde e assertions, recusando erros de montagem. `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:449`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:174`.

Comparação e execução direta da base indisponíveis porque `git show` retorna 128 nesta cópia; não substituí o RED informado por esse erro ambiental. [nao verificado]

## 10/11 18:10 · sessao 5b97b39adce8 · - · - · origem=Ronald (tmux)

1. **Há geração depois do atomic do núcleo.** O núcleo agora gera dentro de `atomic` tanto no nascimento quanto na agregação (`app/chamados/services/disputa_emissao.py:534`, `:596`, `:625`), mas o wrapper ainda chama `perguntar_entradas_pendentes` depois do retorno (`:748`), e essa função chama novamente o gerador (`:294`). Além disso, `fabrica_da_celula.perguntar` chama o wrapper (`app/chamados/services/fabrica_da_celula.py:172`) e depois chama diretamente o gerador (`:174`). Existe um caminho concreto sem atomic externo: consumidor `app/chamados/reconciliador.py:1387` → fábrica `:301` → `:219` → `:172/:174`; o atomic do consumidor só começa depois, em `app/chamados/reconciliador.py:1392`. A exceção da segunda geração é capturada em `:1388`, sem desfazer o núcleo anteriormente concluído. A reprodução SQL/produção é [nao verificado].

2. **Censo de chamadores operacionais diretos.**

   | Porta chamada | Chamador | Situação da pergunta |
   |---|---|---|
   | `_abrir_ou_agregar_disputa` | Recursão de reuso, `app/chamados/services/disputa_emissao.py:659` | Geração no atomic recursivo, dentro do atomic do tratamento `:651/:596`. |
   | `_abrir_ou_agregar_disputa` | `disputa_para_declaracao`, `app/chamados/services/disputa_emissao.py:691` | Desliga geração `:695`; consumidores abaixo constroem pergunta no atomic deles. |
   | `_abrir_ou_agregar_disputa` | Wrapper, `app/chamados/services/disputa_emissao.py:727` | Geração principal dentro `:625/:596`; pendentes depois `:748/:294`. |
   | `_abrir_ou_agregar_disputa` | Grade, `app/ponto/management/commands/reconciliar_grade.py:424` | Passa `motivos=[]` `:425`; o laço do gerador não executa (`app/chamados/services/disputa_emissao.py:312`). Pode nascer DS sem pergunta. |
   | `_abrir_ou_agregar_disputa` | Smoke, `app/chamados/management/commands/smoke_chamados.py:99` | Gera de novo `:102`, mas o `handle` envolve tudo em atomic com rollback `:39/:43`; não é exemplo de DS commitada antes da segunda geração. |
   | `criar_ou_atualizar_disputa` | APIs, `app/api/views.py:794`, `app/api/views_core.py:794` | Geração principal no núcleo; pendentes podem gerar depois (`app/chamados/services/disputa_emissao.py:748`). |
   | `criar_ou_atualizar_disputa` | PWA, `app/ponto/services/triagem_batida.py:734` | Mesma porta e ressalva dos pendentes (`app/chamados/services/disputa_emissao.py:748`). |
   | `criar_ou_atualizar_disputa` | Retro de selfie, `app/ponto/management/commands/marcar_foto_ausente_retro.py:96` | Mesma porta e ressalva dos pendentes (`app/chamados/services/disputa_emissao.py:748`). |
   | `criar_ou_atualizar_disputa` | Fábrica, `app/chamados/services/fabrica_da_celula.py:172` | Segunda geração explícita fora do atomic interno `:174`. |

   Os únicos consumidores operacionais de `disputa_para_declaracao` encontrados são `declarar_hora_colab` e `aplicar_declaracao`: abrem atomic em `app/chamados/services/declaracao_texto.py:348/:654`, chamam a porta em `:360/:659` e abrem pergunta em `:376/:666`. Assim DS e pergunta pertencem à mesma transação desses consumidores; o retorno estrangeiro da pergunta nesses consumidores não passa pelo detector novo de nascimento do núcleo (`app/chamados/services/disputa_emissao.py:624`).

3. **Outros nascimentos de DS.** O agrupador cria em `app/chamados/services/disputa_emissao.py:1186`, dentro do atomic `:1115`; perguntas de intervalo, ata, origem e fallback ficam dentro dele (`:1216`, `:1238`, `:1276`, `:1307`). A ausência de ata conserva os `continue` (`:1235`, `:1273`, `:1304`) e não fecha a DS quando `_absteve_por_ata` (`:1342`). A âncora cria DS em `:1421` e chama `PerguntaDisputa.abrir_conversa` em `:1434`, ambas dentro do atomic `:1385`; ignora o retorno da porta `:1434`. A grade já usa o núcleo com lista vazia (`app/ponto/management/commands/reconciliar_grade.py:424`), não um quarto `objects.create`.

4. **Reuso no nascimento atende o mecanismo pedido.** O gerador devolve as perguntas obtidas da porta (`app/chamados/services/disputa_emissao.py:439/:444/:470`). No nascimento, o detector identifica `pergunta.disputa_id != disputa.pk`, distribui motivos por dona e levanta dentro do atomic (`:628/:635/:636`). A captura fica fora desse atomic (`:648`); o tratamento abre outro atomic (`:651`) e reentra no próprio ramo de agregação (`:659`). A dona é selecionada explicitamente sem filtro de aberta (`:562`); o save grava apenas `motivos`/`contexto` (`:570/:591/:594`), preservando `fechada_em` e FK da pergunta. Não há novo escritor de motivos fora do núcleo nesse tratamento (`:659`).

5. **Mas o ramo de agregação ignora a pergunta estrangeira.** Quando já existe DS aberta, o código salva os motivos nela e chama `gerar_perguntas_disputa` sem inspecionar o retorno (`app/chamados/services/disputa_emissao.py:564/:594/:596/:597`). Se essa DS aberta estiver vazia — o caminho da grade permite isso (`app/ponto/management/commands/reconciliar_grade.py:424`) — e a porta devolver canônica de outra DS, ela pode continuar sem pergunta própria e com o motivo, enquanto a dona não recebe a agregação pelo mecanismo novo. O teste de estrangeira ORM só prepara dona fechada e ausência de DS aberta nova (`app/chamados/tests/test_bo_disputa_sem_pergunta.py:70`); a reprodução desse cenário adicional é [nao verificado].

6. **Batida não entrou no atomic novo, mas a resposta pode falhar.** APIs gravam antes de chamar a disputa (`app/api/views_core.py:681/:794`, `app/api/views.py:684/:794`); o atomic novo pertence somente ao núcleo (`app/chamados/services/disputa_emissao.py:534`). PWA também registra antes (`app/ponto/services/triagem_batida.py:701/:734`). `registrar_batida` faz o `Batida.objects.create` em `app/ponto/registro_batida.py:157` e protege a etapa de fechamento posterior com `except Exception` (`:175/:179`). Porém a chamada de disputa das APIs não está protegida por um `try` envolvente (`app/api/views_core.py:794`, `app/api/views.py:794`); o núcleo captura apenas `_PerguntaDeOutraDisputa` (`app/chamados/services/disputa_emissao.py:648`). Logo um erro do gerador pode subir e impedir a resposta normal depois da gravação. Essa propagação já existia quando o wrapper gerava fora do atomic na base; novidade de runtime, estado HTTP efetivo e garantias PostgreSQL são [nao verificado].

## 10/11 17:59 · sessao 3960e7546cc4 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Volta 2 entregue: [ENTREGA, com comandos, RED e mutantes](chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:9).

- Linhas registradas: **778, 779 e 1262**. [Registro](core/tests/test_medida_turno_do_par.py:267)
- Trava duplicada removida; tetos **145/219**, com justificativa C8. [Cura](chamados/services/disputa_emissao.py:548), [tetos](ponto/tests/test_selo_chokepoint_escrita.py:180)
- Sonda isolada: **6 → 5 consultas**, igualmente com 1 e 3 furos. [Medida](chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:54)

SQL real e suite ORM completa: **[nao verificado]**. [nao verificado]

## 10/11 17:53 · sessao 33840901b286 · - · - · origem=Ronald (tmux)

As duas queries vêm de [disputa_emissao.py:548](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:548) e [:549](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:549):

- `SELECT ... FOR UPDATE` do Colaborador pela chamada separada de `trava_na_ordem`; implementação em [models.py:2666](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/models.py:2666).
- `SELECT pk` de todas as disputas do colaborador, incluindo fechadas.

O `atomic` já existia na BASE; a comparação `git diff 6c0517eb9 69f1a8b4 -- chamados/services/disputa_emissao.py` mostra que o acréscimo está nessas leituras. Quantidades ORM locais: [nao verificado].

É possível remover **uma** query: passe `donas` como QuerySet lazy e retire a chamada separada de `trava_na_ordem(colaborador)`. A própria porta trava o Colaborador em [models.py:2665](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/models.py:2665) antes de avaliar os IDs em [:2671](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/models.py:2671). O QuerySet guarda o resultado das avaliações seguintes; runtime com ORM: [nao verificado].

A query restante lê o conjunto de donas necessário para incluir as disputas fechadas na trava antes de Chamado/Pergunta: [disputa_emissao.py:545](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:545). Ela fica fora dos loops de motivos/furos: [:543](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:543). Portanto, o acréscimo estrutural passa de **+2 para +1 por invocação do núcleo**; comparação ORM efetiva de 1 versus 3 furos: [nao verificado].

Nenhum arquivo alterado.

## 10/11 17:16 · sessao af554e746a14 · BO-0611-05 · censo · origem=Code

O geofence decide pela **truthiness das duas coordenadas**:

```python
if not batida.latitude or not batida.longitude:
```

Essa é a expressão exata em `app/ponto/services/geofence.py:247`. Havendo posto, o ramo cria alerta `tipo='sem_gps'` e grava coordenadas **do alerta** como `None`; não altera latitude/longitude da Batida (`app/ponto/services/geofence.py:248`, `app/ponto/services/geofence.py:252`, `app/ponto/services/geofence.py:253`).

O censo abaixo reúne **37 sítios diretos de Batida/contexto**, mais **4 sobre coordenadas copiadas em AlertaGeofence**, totalizando **41**, conforme as referências das tabelas. Incidência desses casos em produção: **[nao verificado]**.

Convenção de contagem: cada expressão independente conta um sítio; cada filtro conjunto das duas coordenadas conta um. Mantive **N/N1 na sintaxe solicitada**: guardas inversas com `is None` e cadeias `.exclude(...=None)` aparecem como **O**, embora sejam testes de nulidade equivalentes. [nao verificado]

**(i) Leitores de Batida gravada ou contexto de chamado — Python**

| Arquivo:linha | Expressão literal | Forma | O que decide |
|---|---|---|---|
| `app/ponto/services/geofence.py:247` | `not batida.latitude or not batida.longitude` | T | Ramo de ausência que pode emitir `sem_gps`. |
| `app/colaboradores/services/drawer.py:63` | `bool(b.latitude and b.longitude)` | T | Resultado `tem_gps` do drawer. |
| `app/ponto/services/geofence.py:327` | `float(batida.latitude) if batida.latitude else None` | T1 | Latitude ou ausência na ocorrência do chamado. |
| `app/ponto/services/geofence.py:328` | `float(batida.longitude) if batida.longitude else None` | T1 | Longitude ou ausência na ocorrência. |
| `app/ponto/services/geofence.py:369` | `float(batida.latitude) if batida.latitude else None` | T1 | Latitude ou ausência no contexto legado. |
| `app/ponto/services/geofence.py:370` | `float(batida.longitude) if batida.longitude else None` | T1 | Longitude ou ausência no contexto legado. |
| `app/core/services/painel_op.py:227` | `entrada.latitude if entrada and entrada.latitude else None` | T1 | Latitude disponível no contexto do painel. |
| `app/core/services/painel_op.py:228` | `entrada.longitude if entrada and entrada.longitude else None` | T1 | Longitude disponível no contexto do painel. |
| `app/colaboradores/painel_cell.py:54` | `str(coord[0] or '')` | T1 | Latitude ou vazio no atributo da linha do painel. |
| `app/colaboradores/painel_cell.py:55` | `str(coord[1] or '')` | T1 | Longitude ou vazio. |
| `app/colaboradores/services/situacional.py:199` | `str((batida_coord.get(c.pk) or (None, None))[0] or '')` | T1 | Latitude ou vazio no painel em lote. |
| `app/colaboradores/services/situacional.py:200` | `str((batida_coord.get(c.pk) or (None, None))[1] or '')` | T1 | Longitude ou vazio. |
| `app/chamados/views.py:692` | `_BatidaGeo.objects.filter(colaborador=colaborador, latitude__isnull=False, longitude__isnull=False)` | N | Seleciona última localização e histórico para o fio. |
| `app/ponto/views.py:485` | `pendentes.filter(latitude__isnull=False, longitude__isnull=False).count()` | N | Conta `com_gps`; `sem_gps` é o complemento em `app/ponto/views.py:486`. |
| `app/ponto/services/geofence.py:52` | `colaborador=colaborador, latitude__isnull=False, longitude__isnull=False` | N | Seleciona batidas consideradas “COM GPS” por `vinculo_contagem`. |
| `app/ponto/services/vinculo_na_lista.py:49` | `Batida.objects.filter(timestamp__date__gte=ini, latitude__isnull=False, longitude__isnull=False, gps_accuracy__isnull=False)` | N | Seleciona evidências geográficas para avaliar vínculo. |
| `app/ponto/services/geofence.py:403` | `la is not None and lo is not None` | N | Seleciona ocorrências do contexto para reavaliar chamados. |
| `app/ponto/views.py:468` | `str(batida.latitude) if batida.latitude is not None else None` | N1 | Latitude ou ausência no snapshot de auditoria offline. |
| `app/ponto/views.py:469` | `str(batida.longitude) if batida.longitude is not None else None` | N1 | Longitude ou ausência no snapshot. |
| `app/core/views.py:1024` | `float(b.latitude) if b.latitude is not None else None` | N1 | Latitude ou ausência no JSON do detalhe. |
| `app/core/views.py:1025` | `float(b.longitude) if b.longitude is not None else None` | N1 | Longitude ou ausência no JSON. |
| `app/colaboradores/views.py:53` | `Batida.objects.exclude(latitude=None).exclude(longitude=None)` | O | Conjunto de colaboradores com geo para vínculo. |
| `app/colaboradores/views.py:337` | `Batida.objects.filter(colaborador=colaborador).exclude(latitude=None).exclude(longitude=None).exists()` | O | `tem_geo` no retorno HTMX de erro ao vincular posto. |
| `app/colaboradores/views.py:356` | `Batida.objects.filter(colaborador=colaborador).exclude(latitude=None).exclude(longitude=None).exists()` | O | `tem_geo` no retorno HTMX de sucesso ao vincular posto. |
| `app/colaboradores/views.py:395` | `Batida.objects.filter(colaborador=colaborador).exclude(latitude=None).exclude(longitude=None).exists()` | O | `tem_geo` ao renderizar linha de vínculo de escala. |
| `app/colaboradores/services/detalhe.py:22` | `.exclude(latitude=None).exclude(longitude=None)` | O | Histórico geo e pontos do mapa. |
| `app/colaboradores/services/detalhe.py:53` | `_todas.exclude(latitude=None).exclude(longitude=None).count()` | O | Contador total de batidas com geo. |
| `app/colaboradores/views.py:2474` | `not coord or coord[0] is None or coord[1] is None` | O | Retorna pino `sem_gps`, cinza, para coordenadas da última Batida. |

No último helper, uma falha de cálculo/conversão também retorna `sem_gps` pelo `except (TypeError, ValueError)`; é tratamento de exceção, separado da expressão direta de presença contabilizada acima (`app/colaboradores/views.py:2479`, `app/colaboradores/views.py:2481`).

**(i) Leitores — templates e JavaScript**

| Arquivo:linha | Expressão literal | Forma | O que decide |
|---|---|---|---|
| `app/templates/core/_d2_feed.html:16` | `b.latitude is not None and b.longitude is not None` | N | Exibe pino/link Google Maps. |
| `app/templates/ponto/batidas_offline_pendentes.html:96` | `batida.latitude is not None and batida.longitude is not None` | N | Exibe link Google Maps. |
| `app/templates/chamados/partials/modal_fio.html:480` | `chamado.modulo_origem == "gps_fora_geofence" and chamado.contexto_json.batida_lat is not None and chamado.contexto_json.batida_lon is not None` | N | Exibe bloco GPS e botão de mapa do chamado. |
| `app/templates/chamados/partials/_relatorio_geo.html:30` | `chamado.contexto_json.batida_lat is not None and chamado.contexto_json.batida_lon is not None` | N | Exibe coordenadas e rota posto→batida. |
| `app/templates/core/dashboard2.html:242` | `(b.latitude && b.longitude)` | T | Escolhe conteúdo GPS ou “Sem localizacao”. |
| `app/templates/core/dashboard2.html:287` | `b.latitude && b.longitude` | T | Inicializa mapa e pino do drawer. |
| `app/static/js/geo-colab-map.js:39` | `isNaN(blat) || isNaN(blon)` | O | Mostra ausência e retorna, ou prossegue com mapa. |
| `app/templates/chamados/fio_colaborador.html:313` | `!window.L||isNaN(blat)||isNaN(blon)` | O | Mostra “Sem coordenada de GPS.” e retorna; inclui indisponibilidade da biblioteca. |
| `app/templates/chamados/painel_gestao.html:583` | `isNaN(blat) || isNaN(blon)` | O | Mostra “Sem coordenada de GPS nesta batida.” e retorna, ou prossegue com mapa. |

**(i) Equivalentes: coordenadas copiadas em AlertaGeofence**

| Arquivo:linha | Expressão literal | Forma | O que decide |
|---|---|---|---|
| `app/ponto/models.py:263` | `self.latitude is None or self.longitude is None` | O | Em `ainda_vale`, ausência conserva a validade do alerta. |
| `app/ponto/management/commands/reconciliar_alertas_geofence.py:42` | `lat is None or lon is None or not alvo` | O | Conta alerta como sem dado e pula reconciliação. |
| `app/templates/colaboradores/detalhe.html:142` | `a.latitude` | T1 | Exibe link de mapa do alerta. |
| `app/templates/core/dashboard_supervisor.html:90` | `a.latitude` | T1 | Exibe link de mapa do alerta ou “—”. |

As coordenadas do alerta vêm da Batida nos ramos geográficos, por exemplo em `app/ponto/services/geofence.py:275` e `app/ponto/services/geofence.py:302`; no ramo `sem_gps`, recebem `None` (`app/ponto/services/geofence.py:253`).

A soma dos sítios documentados nessas tabelas é:

| Universo | T | T1 | N | N1 | O | Total |
|---|---:|---:|---:|---:|---:|---:|
| Batida/contexto | 4 | 10 | 9 | 4 | 10 | **37** |
| Cópias em alerta | 0 | 2 | 0 | 0 | 2 | **4** |
| **Total operacional** | **4** | **12** | **9** | **4** | **12** | **41** |

**Divergem da forma T do geofence todos os 37 sítios classificados T1, N, N1 ou O nas tabelas.** Os quatro T são `app/ponto/services/geofence.py:247`, `app/colaboradores/services/drawer.py:63`, `app/templates/core/dashboard2.html:242` e `app/templates/core/dashboard2.html:287`.

A divergência de **forma** não implica o mesmo efeito em todos: T1 serializa/testa um componente; N/N1 e os O de nulidade preservam zero; os O com `isNaN` aceitam zero quando ele chega como número parseável (`app/ponto/services/geofence.py:327`, `app/core/views.py:1024`, `app/colaboradores/views.py:2474`, `app/static/js/geo-colab-map.js:29`, `app/static/js/geo-colab-map.js:39`). Se os nove O de nulidade forem agrupados semanticamente como N, o total equivalente fica **T=4, T1=12, N=18, N1=4, O=3** — os mesmos sítios, apenas outra convenção.

Há consumidores de resultados prontos, sem novo cálculo sobre coordenadas; ficam fora dos 41:

| Arquivo:linha | Expressão | Uso |
|---|---|---|
| `app/templates/colaboradores/partials/_drawer_diagnostico.html:49` | `b.tem_gps`, duas vezes | Cor e texto `ok/sem`; produtor em `app/colaboradores/services/drawer.py:63`. |
| `app/templates/colaboradores/partials/vinculo_row.html:12` | `d.tem_geo` | Selo 📍; produtor em `app/colaboradores/views.py:84`. |
| `app/templates/colaboradores/partials/_ps_linha.html:16` | `l.geo == 'verde'` | Apresentação do pino. |
| `app/templates/colaboradores/partials/_ps_linha.html:17` | `l.geo == 'vermelho'` | Apresentação do pino. |
| `app/templates/chamados/fio_colaborador.html:112` | `ultima_geo` | Bloco de última localização; seleção por coordenadas em `app/chamados/views.py:692`. |
| `app/templates/colaboradores/detalhe.html:226` | `!pontos.length` | Ausência de pontos no mapa; seleção por coordenadas em `app/colaboradores/services/detalhe.py:22`. |

Quanto a um nome reutilizável para “tem geo”: existe a **chave de dicionário** `'tem_gps': bool(b.latitude and b.longitude)`; não é método/propriedade da Batida (`app/colaboradores/services/drawer.py:63`). Existem variáveis locais `tem_geo` (`app/colaboradores/views.py:337`, `app/colaboradores/views.py:356`, `app/colaboradores/views.py:395`) e a função classificadora `_geo_do_pino(coord, posto)`, cujo guard de presença é próprio (`app/colaboradores/views.py:2454`, `app/colaboradores/views.py:2474`).

O registro declara `classificar_posicao` para **“a batida esta dentro do geofence?”**, não um predicado de presença (`app/core/juizes.py:1191`). Essa função recebe distância, accuracy e raio (`app/ponto/services/geofence.py:95`). Não localizei função/propriedade/método booleano específico `tem_gps`, `tem_geo` ou `tem_coordenada`; inexistência absoluta sob qualquer nome: **[nao verificado]**.

**(ii) Captura no navegador antes do POST — somente listagem**

| Arquivo:linha | Captura/teste | [nao verificado]
|---|---|
| `app/templates/ponto/bater_ponto.html:293` | Geolocation e preenchimento das coordenadas. |
| `app/templates/ponto/bater_ponto.html:482` | Captura com timeout. |
| `app/templates/ponto/bater_ponto.html:503` | `!lat || !gpsCapturedAt || (Date.now() - gpsCapturedAt) > 60000` |
| `app/templates/ponto/bater_ponto.html:505` | `if (gps)` |
| `app/templates/ponto/bater_ponto.html:509` | `!lat || !lng` |
| `app/templates/ponto/triagem/base_triagem.html:56` | Geolocation e preenchimento de `.campo-lat/.campo-lng`. |
| `app/templates/ponto/triagem/apto.html:200` | Captura com timeout. |
| `app/templates/ponto/triagem/apto.html:217` | `!lat || !gpsCapturedAt || (Date.now() - gpsCapturedAt) > 60000` |
| `app/templates/ponto/triagem/apto.html:219` | `if (gps)` |
| `app/templates/ponto/triagem/apto.html:221` | `lat && lng` |
| `app/templates/ponto/triagem/apto.html:276` | `gpsLat == null || gpsLng == null` |
| `app/templates/ponto/bater_ponto_v2.html:177` | `gpsLat!=null && gpsAccuracy!=null && acc>gpsAccuracy && (Date.now()-gpsCapturedAt)<120000` |
| `app/templates/ponto/bater_ponto_v2.html:184` | Suporte/API de geolocation. |
| `app/templates/ponto/bater_ponto_v2.html:189` | `gpsLat!=null` |
| `app/templates/ponto/bater_ponto_v2.html:204` | `gpsLat==null` |
| `app/templates/ponto/bater_ponto_v2.html:273` | Captura com timeout. |
| `app/templates/ponto/bater_ponto_v2.html:292` | `gpsLat==null || gpsLng==null || !gpsCapturedAt || (Date.now()-gpsCapturedAt)>60000` |
| `app/templates/ponto/bater_ponto_v2.html:294` | `!(g && g.lat!=null)` |
| `app/templates/ponto/bater_ponto_v2.html:295` | `g && g.lat!=null` |
| `app/static/pwa.js:289` | `capturarGPS`, coordenadas ou `null`. |
| `app/static/pwa.js:394` | Captura para POST de ping geográfico. |
| `app/templates/core/onboarding.html:264` | Geolocation para permissão, sem leitura de lat/lon no callback. |

Também antes do POST, mas lendo uma batida **já guardada localmente no IndexedDB**, há dois sítios adicionais, fora dos 41 do servidor:

| Arquivo:linha | Expressão literal | Forma | Decisão |
|---|---|---|---|
| `app/static/pwa.js:145` | `b.latitude != null` | O | Inclui latitude no POST de sincronização. |
| `app/static/pwa.js:146` | `b.longitude != null` | O | Inclui longitude no POST de sincronização. |

A origem é `batidas_pendentes` do IndexedDB (`app/static/pwa.js:116`, `app/static/pwa.js:131`). São dois testes individuais de não nulidade em JS, classificados O pela legenda literal.

**Escritores de latitude/longitude da Batida**

| Arquivo:linha | Escrita/origem | Tratamento |
|---|---|---|
| `app/ponto/registro_batida.py:152` | `Batida.objects.create(..., **campos)` | Caminho com conflito autorizado; sanitização anterior em `app/ponto/registro_batida.py:105`. |
| `app/ponto/registro_batida.py:157` | `Batida.objects.create(..., **campos)` | Caminho normal; mesma sanitização. |
| `app/api/views.py:654` | `latitude=latitude`, seguido de longitude | Encaminha `request.data` à porta; leitura em `app/api/views.py:416`, chamada em `app/api/views.py:648`. |
| `app/api/views_core.py:646` | `latitude=latitude`, seguido de longitude | Encaminha `request.data`; leitura em `app/api/views_core.py:403`, chamada em `app/api/views_core.py:640`. |
| `app/ponto/services/triagem_batida.py:707` | `latitude=latitude`, seguido de longitude | Encaminha POST/sessão à porta; chamada em `app/ponto/services/triagem_batida.py:701`. |
| `app/ponto/registro_batida.py:335` | `Batida.objects.bulk_create([Batida(**d) for d in linhas], batch_size=500)` | Semeadura de ensaio, sem sanitização; guard em `app/ponto/registro_batida.py:333`. |

A sanitização percorre latitude/longitude (`app/ponto/registro_batida.py:3`, `app/ponto/registro_batida.py:8`):

- **`0.0` é preservado:** `float(v)` apenas valida, sem substituir o valor (`app/ponto/registro_batida.py:15`).
- **`''` e `None` viram `None`**, assim como valores não numéricos (`app/ponto/registro_batida.py:11`, `app/ponto/registro_batida.py:17`).
- A entrada web já aplica `request.POST.get('latitude') or None` e equivalente de longitude (`app/ponto/views.py:270`, `app/ponto/views.py:271`).
- O chamador localizado da semeadura omite geo no dicionário; os campos são nullable (`app/ponto/management/commands/gerar_batidas_smoke_motor.py:107`, `app/ponto/management/commands/gerar_batidas_smoke_motor.py:182`, `app/ponto/models.py:58`, `app/ponto/models.py:59`).
- A porta de anotação não inclui latitude/longitude nos campos permitidos e rejeita campos externos (`app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:241`).

Estes produtores chamam `registrar_batida` **sem geo**; numa criação nova, latitude/longitude ficam nulas pelos campos do modelo (`app/ponto/models.py:58`, `app/ponto/models.py:59`):

| Arquivo:linha | Produtor |
|---|---|
| `app/ponto/views.py:1815` | Saída aprovada em lote. |
| `app/ponto/services/justificativa.py:188` | Saída após aprovação. |
| `app/ponto/services/justificativa.py:361` | Ajuste retroativo. |
| `app/ponto/services/veredito_celula.py:322` | Materialização manual. |
| `app/chamados/services/regularizacao_ext.py:77` | Regularização externa. |
| `app/chamados/services/materializacao.py:927` | `disputa_s84_retro`. |
| `app/chamados/services/materializacao.py:1007` | `disputa_ancora`. |
| `app/chamados/services/acoes_chamado.py:44` | `excecao_autorizada`. |

Há teste existente que espera persistência de `0.0/0.0`, conversão de `''/''` para `None/None` e preservação de coordenada parcial (`app/api/tests/test_l030_batida_pela_porta.py:58`, `app/api/tests/test_l030_batida_pela_porta.py:63`, `app/api/tests/test_l030_batida_pela_porta.py:68`). Resultado de execução: **[nao verificado]**.

Para delimitar a contagem operacional: fixtures também contêm decisões de nulidade ao montar dados sintéticos, fora dos 41 — `lat is None` e `lon is None` em `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:101` e `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:102` (**O**, dois); `lat is not None` e `lon is not None` em `app/ponto/tests/test_bo0611_05_templates_geo.py:115` e `app/ponto/tests/test_bo0611_05_templates_geo.py:116` (**N1**, dois).

Há ainda leituras de entrada no servidor, anteriores à gravação: normalização do POST (`app/ponto/views.py:270`, `app/ponto/views.py:271`), truthiness individual para assinatura (`app/api/views.py:464`, `app/api/views.py:465`, `app/api/views_core.py:451`, `app/api/views_core.py:452`), recuperação individual da sessão (`app/ponto/services/triagem_batida.py:507`, `app/ponto/services/triagem_batida.py:509`) e sanitização (`app/ponto/registro_batida.py:11`). Elas não entram no subtotal de leitores de Batida gravada.

Os testes de coordenadas **do Posto** e os pings de outro modelo também ficam fora desse subtotal; exemplos concretos são `app/ponto/services/geofence.py:259`, `app/templates/ponto/bater_ponto_v2.html:139`, `app/ponto/views.py:2941`, `app/api/views.py:2331` e `app/api/views_core.py:865`.

## 10/11 17:13 · sessao 0ca338b4e83a · - · - · origem=Ronald (tmux)

Censo Python: **28 sítios diretos** de Batida/contexto, por condição/expressão, agrupando cada cadeia de exclusão das duas coordenadas em um sítio. Na convenção estrita solicitada pelo pai: **T=2, T1=10, N=5, N1=4, O=7**. Os sete O usam exclusão de `None` ou guarda inversa `is None`; semanticamente são nulidade das duas coordenadas.

| Arquivo:linha | Expressão literal | Forma | Decisão |
|---|---|---|---|
| `app/ponto/services/geofence.py:247` | `not batida.latitude or not batida.longitude` | T | Entra em ausência GPS; com posto cria alerta `sem_gps` em :249–256. |
| `app/colaboradores/services/drawer.py:63` | `bool(b.latitude and b.longitude)` | T | Produz chave `tem_gps` para batida do drawer. |
| `app/ponto/services/geofence.py:327` | `float(batida.latitude) if batida.latitude else None` | T1 | Latitude ou ausência no contexto da ocorrência do chamado. |
| `app/ponto/services/geofence.py:328` | `float(batida.longitude) if batida.longitude else None` | T1 | Longitude ou ausência no contexto da ocorrência. |
| `app/ponto/services/geofence.py:369` | `float(batida.latitude) if batida.latitude else None` | T1 | Latitude ou ausência no contexto legado do chamado. |
| `app/ponto/services/geofence.py:370` | `float(batida.longitude) if batida.longitude else None` | T1 | Longitude ou ausência no contexto legado. |
| `app/core/services/painel_op.py:227` | `entrada.latitude if entrada and entrada.latitude else None` | T1 | Coordenada disponível no contexto do painel. |
| `app/core/services/painel_op.py:228` | `entrada.longitude if entrada and entrada.longitude else None` | T1 | Idem longitude. |
| `app/colaboradores/painel_cell.py:54` | `str(coord[0] or '')` | T1 | Latitude ou string vazia na linha do painel. |
| `app/colaboradores/painel_cell.py:55` | `str(coord[1] or '')` | T1 | Longitude ou string vazia. |
| `app/colaboradores/services/situacional.py:199` | `str((batida_coord.get(c.pk) or (None, None))[0] or '')` | T1 | Latitude ou string vazia na linha do painel em lote. |
| `app/colaboradores/services/situacional.py:200` | `str((batida_coord.get(c.pk) or (None, None))[1] or '')` | T1 | Longitude ou string vazia. |
| `app/chamados/views.py:692` | `_BatidaGeo.objects.filter(colaborador=colaborador, latitude__isnull=False, longitude__isnull=False)` | N | Seleciona última geolocalização e histórico para fio. |
| `app/ponto/views.py:485` | `pendentes.filter(latitude__isnull=False, longitude__isnull=False).count()` | N | Contador `com_gps`; `sem_gps` é complemento em :486. |
| `app/ponto/services/geofence.py:52` | `colaborador=colaborador, latitude__isnull=False, longitude__isnull=False` | N | Seleciona últimas batidas COM GPS em `vinculo_contagem`. |
| `app/ponto/services/vinculo_na_lista.py:49` | `Batida.objects.filter(timestamp__date__gte=ini, latitude__isnull=False, longitude__isnull=False, gps_accuracy__isnull=False)` | N | Seleciona evidência geo de cadastro versus realidade. |
| `app/ponto/services/geofence.py:403` | `la is not None and lo is not None` | N | Seleciona ocorrências do contexto de chamado que entram na reavaliação. |
| `app/ponto/views.py:468` | `str(batida.latitude) if batida.latitude is not None else None` | N1 | Snapshot de auditoria antes de aprovar/apagar offline. |
| `app/ponto/views.py:469` | `str(batida.longitude) if batida.longitude is not None else None` | N1 | Snapshot longitude. |
| `app/core/views.py:1024` | `float(b.latitude) if b.latitude is not None else None` | N1 | JSON do detalhe da batida. |
| `app/core/views.py:1025` | `float(b.longitude) if b.longitude is not None else None` | N1 | JSON longitude. |
| `app/colaboradores/views.py:53` | `Batida.objects.exclude(latitude=None).exclude(longitude=None)` | O | Conjunto de colaboradores com geo para vínculo. |
| `app/colaboradores/views.py:337` | `Batida.objects.filter(colaborador=colaborador).exclude(latitude=None).exclude(longitude=None).exists()` | O | `tem_geo` no retorno de erro HTMX ao vincular posto. |
| `app/colaboradores/views.py:356` | mesma expressão de :337 | O | `tem_geo` no retorno de sucesso HTMX ao vincular posto. |
| `app/colaboradores/views.py:395` | mesma expressão de :337 | O | `tem_geo` na linha ao vincular escala. |
| `app/colaboradores/services/detalhe.py:22` | `).exclude(latitude=None).exclude(longitude=None)` | O | Histórico geo e pontos do mapa. |
| `app/colaboradores/services/detalhe.py:53` | `_todas.exclude(latitude=None).exclude(longitude=None).count()` | O | Contador total geo. |
| `app/colaboradores/views.py:2474` | `not coord or coord[0] is None or coord[1] is None` | O | Pino `sem_gps`, cinza; coordenada vem da última Batida em `painel_cell.py:33–40` e `situacional.py:137–145,179`. |

**Equivalentes em coordenadas copiadas de alerta**, se incluídos pelo escopo, acrescentam **O=2**, total Python **30**:

| Arquivo:linha | Expressão literal | Forma | Decisão |
|---|---|---|---|
| `app/ponto/models.py:263` | `self.latitude is None or self.longitude is None` | O | `AlertaGeofence.ainda_vale`: ausência conserva o alerta. |
| `app/ponto/management/commands/reconciliar_alertas_geofence.py:42` | `lat is None or lon is None or not alvo` | O | Classifica alerta sem dado e pula reconciliação. |

Não contei guardas da mera existência da tupla como novo juiz de latitude/longitude: `b_co or (None, None)` em `painel_cell.py:39`, e os `batida_coord.get(c.pk) or (None, None)` internos em `situacional.py:199–200`.

Não achei função/propriedade/método Python de produção nomeado `tem_gps`, `tem_geo` ou `tem_coordenada` que responda essa pergunta. Existem a chave `tem_gps` em `drawer.py:63` e variáveis `tem_geo` em `colaboradores/views.py:337,356,395`; `_geo_do_pino` existe em `colaboradores/views.py:2454`, mas retorna classificação/cor e testa nulidade em :2474. `classificar_posicao`, em `geofence.py:95`, recebe distância, accuracy e raio, não latitude/longitude. Busca em `core/juizes.py` por geo/gps/coordenada não encontrou registro; ausência global de implementação com nome arbitrário: **[nao verificado]**.

A varredura AST de produção feita por este explorador terminou sem erro de parse; o pai informou parse total de 3299/3299 Python.

Leituras Python de **entrada anterior à gravação**, separadas dos 28:

- `app/ponto/views.py:270–271`: `request.POST.get('latitude') or None` e longitude.
- `app/api/views.py:464–465`, `app/api/views_core.py:451–452`: `latitude or ''` e `longitude or ''`, assinatura.
- `app/ponto/services/triagem_batida.py:507,509`: `not latitude` / `not longitude`, recuperação de confirmação na sessão.
- `app/ponto/registro_batida.py:11`: `v in ('', None)`, com campos latitude/longitude declarados em :3; sanitização antes de criação.
- Pings de outro modelo: `lat is None or lng is None` em `app/ponto/views.py:2941`, `app/api/views.py:2331` e `app/api/views_core.py:865`.

As formas Python N/N1 e as sete O de nulidade diferem da forma T do geofence; os T1 consultam apenas um campo e não constituem o mesmo predicado conjunto de `geofence.py:247`.

## 10/11 17:12 · sessao e2df1f633553 · - · - · origem=Ronald (tmux)

Censo de escritores por AST (`3299` Python lidos, sem erro de parse) e grep; nenhum arquivo alterado. Execução de testes e persistência real em produção: [nao verificado]. [nao verificado]

| Sítio | Expressão/origem | Valores especiais |
|---|---|---|
| `app/ponto/registro_batida.py:152` e `:157` | `Batida.objects.create(..., **campos)` | Os dois caminhos recebem `_sanitiza_geo(campos)` em `:105`. |
| `app/ponto/registro_batida.py:11` | `if v in ('', None): campos[k] = None` | `''` e `None` viram `None`. |
| `app/ponto/registro_batida.py:15` | `float(v)` | Só valida; não altera o valor numérico, preservando `0.0`. Não numérico vira `None` em `:16-17`. |
| `app/api/views.py:648` | `registrar_batida(... latitude=latitude, longitude=longitude ...)` (`:654-655`) | Origem `request.data.get(...)` em `:416-417`; zero segue à porta; vazio/ausência seguem à sanitização. |
| `app/api/views_core.py:640` | Mesmo encaminhamento (`:646-647`) | Origem `request.data.get(...)` em `:403-404`; mesma sanitização. |
| `app/ponto/services/triagem_batida.py:701` | Mesmo encaminhamento (`:707-708`) | Origem POST em `app/ponto/views.py:270-271`: `request.POST.get(...) or None`; recuperação de sessão em `triagem_batida.py:507-510`, chamada em `:659`. String `'0'` é preservada; `''` vira `None`. |
| `app/ponto/registro_batida.py:335` | `Batida.objects.bulk_create([Batida(**d) for d in linhas], batch_size=500)` | Caminho `semear`, guardado para banco de ensaio em `:333`; não chama `_sanitiza_geo`. Único chamador localizado: `app/ponto/management/commands/gerar_batidas_smoke_motor.py:107`; seu dict omite geo em `:182-192`, produzindo campos nulos (`app/ponto/models.py:58-59`). |

Outros produtores chamam a porta **sem** latitude/longitude; quando criam uma nova Batida, os campos são nulos (`app/ponto/models.py:58-59`):

- `app/ponto/views.py:1815`: saída aprovada em lote.
- `app/ponto/services/justificativa.py:188`: saída após aprovação.
- `app/ponto/services/justificativa.py:361`: ajuste retroativo.
- `app/ponto/services/veredito_celula.py:322`: materialização manual.
- `app/chamados/services/regularizacao_ext.py:77`: regularização externa.
- `app/chamados/services/materializacao.py:927`: `disputa_s84_retro`.
- `app/chamados/services/materializacao.py:1007`: `disputa_ancora`.
- `app/chamados/services/acoes_chamado.py:44`: `excecao_autorizada`.

O caminho de anotação não permite alterar geo: `CAMPOS_ANOTAVEIS` omite latitude/longitude em `app/ponto/registro_batida.py:203`, e o guard rejeita campos externos em `:241-243`.

O teste existente caracteriza explicitamente `0.0/0.0` persistido em `app/api/tests/test_l030_batida_pela_porta.py:58`, `''/'' → None/None` em `:63`, e coordenada parcial em `:68`. Resultado de execução desses testes: [nao verificado].

Expressão exata do geofence: `if not batida.latitude or not batida.longitude:` em `app/ponto/services/geofence.py:247`. Se houver posto (`:248`), cria **AlertaGeofence**, não altera Batida: `tipo='sem_gps'` em `:252`, latitude/longitude do alerta `None` em `:253-254`. O contexto do chamado normaliza cada componente separadamente: `float(batida.latitude) if batida.latitude else None` em `:327` e `:369`; equivalente de longitude em `:328` e `:370`.

Não localizei função/propriedade/método booleano nomeado que seja a autoridade de “Batida tem coordenadas”; inexistência absoluta: [nao verificado]. Os candidatos concretos são:

- `app/colaboradores/services/drawer.py:63`: chave de dict `'tem_gps': bool(b.latitude and b.longitude)`, não método/propriedade.
- `app/colaboradores/views.py:337`, `:356`, `:395`: variáveis locais `tem_geo` derivadas de queryset.
- `app/colaboradores/views.py:2454`: função classificadora `_geo_do_pino(coord, posto)`, com presença por `if not coord or coord[0] is None or coord[1] is None:` em `:2474`.
- `app/core/juizes.py:1191`: autoridade registrada é `classificar_posicao` para “a batida esta dentro do geofence?”, não para presença de coordenadas.

## 10/11 17:12 · sessao ca2040610eb0 · - · - · origem=Ronald (tmux)

Censo HTML/JS direto de presença: **11 sítios** para Batida/contexto/cópia em Alerta: **N=4, T=2, T1=2, O=3, N1=0** (linhas da tabela).

| arquivo:linha | expressão literal | forma | decisão |
|---|---|---|---|
| `app/templates/core/_d2_feed.html:16` | `b.latitude is not None and b.longitude is not None` | N | Exibe pino/link Google Maps; link em :17. |
| `app/templates/ponto/batidas_offline_pendentes.html:96` | `batida.latitude is not None and batida.longitude is not None` | N | Exibe link Google Maps; link em :97. |
| `app/templates/chamados/partials/modal_fio.html:480` | `chamado.modulo_origem == "gps_fora_geofence" and chamado.contexto_json.batida_lat is not None and chamado.contexto_json.batida_lon is not None` | N | Habilita bloco GPS e botão mapa do chamado; dados do botão em :484. |
| `app/templates/chamados/partials/_relatorio_geo.html:30` | `chamado.contexto_json.batida_lat is not None and chamado.contexto_json.batida_lon is not None` | N | Exibe coordenadas e rota posto→batida; :31–32. |
| `app/templates/core/dashboard2.html:242` | `(b.latitude && b.longitude)` | T | Define `gpsBlock`: mapa/coordenadas/link ou “Sem localizacao”; :242–246. |
| `app/templates/core/dashboard2.html:287` | `b.latitude && b.longitude` | T | Inicializa mapa e pino do drawer; :288–301. |
| `app/templates/colaboradores/detalhe.html:142` | `a.latitude` | T1 | Exibe link mapa da cópia em Alerta; :128, :135, :143. |
| `app/templates/core/dashboard_supervisor.html:90` | `a.latitude` | T1 | Exibe link mapa do Alerta, senão “—”; :77, :84, :91–94. |
| `app/static/js/geo-colab-map.js:39` | `isNaN(blat) || isNaN(blon)` | O | Dados de batida recente: informa ausência e retorna ou desenha mapa/pino; parse em :29, efeito :40–41, pino :65. Reutilizado pelo chamado via `abrirGeoBatida`, :94–106. |
| `app/templates/chamados/fio_colaborador.html:313` | `!window.L||isNaN(blat)||isNaN(blon)` | O | Informa “Sem coordenada de GPS.” e retorna ou desenha mapa/pino; parse :301, pino :318. Mistura disponibilidade da biblioteca com validade numérica. |
| `app/templates/chamados/painel_gestao.html:583` | `isNaN(blat) || isNaN(blon)` | O | Informa “Sem coordenada de GPS nesta batida.” e retorna ou desenha mapa/pino; parse :571, pino :592. |

Os quatro N, dois T1 e três O divergem da expressão de truthiness das duas coordenadas do geofence. O significado exato para cada entrada Python/JSON precisa acompanhar tipagem/conversão: JS `isNaN(parseFloat(...))` aceita zero numérico; a origem do parse está nas linhas citadas.

**Armazenamento local no navegador, antes da sincronização**, separado de Batida já gravada no banco:

| arquivo:linha | expressão literal | forma | decisão |
|---|---|---|---|
| `app/static/pwa.js:145` | `b.latitude != null` | O pela legenda literal do pedido; equivalente JS a teste não nulo de uma coordenada | Inclui latitude no POST de sincronização. |
| `app/static/pwa.js:146` | `b.longitude != null` | O pela legenda literal do pedido; equivalente JS a teste não nulo de uma coordenada | Inclui longitude no POST de sincronização. |

`b` vem de `batidas_pendentes` do IndexedDB (`app/static/pwa.js:130–138`); essas coordenadas são guardadas em `app/static/pwa.js:116`. Se o consolidado decidir agrupar equivalentes JS `!= null` como N1, serão **dois N1 locais**, explicitando a convenção.

Há consumidores de resultados já calculados, sem ler/refazer latitude/longitude; **não somar aos 11 juízos diretos**:

- `app/templates/colaboradores/partials/_drawer_diagnostico.html:49`: `{% if b.tem_gps %}` aparece **duas vezes**; cor e texto `ok/sem`. Produtor é chave de dict, `'tem_gps': bool(b.latitude and b.longitude)`, em `app/colaboradores/services/drawer.py:63`; não prova método/propriedade de Batida.
- `app/templates/colaboradores/partials/vinculo_row.html:12`: `{% if d.tem_geo %}` controla selo 📍; chave produzida em `app/colaboradores/views.py:84`.
- `app/templates/colaboradores/partials/_ps_linha.html:16–18`: `l.geo == 'verde'` / `l.geo == 'vermelho'` / else escolhe selo; coordenadas chegam como `l.b_lat/l.b_lon` na :1. Há decisões `or ''` nos produtores Python `app/colaboradores/painel_cell.py:54–55` e `app/colaboradores/services/situacional.py:199–200`, a consolidar pelo explorador Python.
- `app/templates/chamados/fio_colaborador.html:112`: `ultima_geo` controla bloco de última localização; histórico em :119. A escolha por coordenadas ocorre no produtor Python, não nesses testes de objetos.
- `app/templates/colaboradores/detalhe.html:165` e :222 usam `batidas_geo`; JS :226 testa `!pontos.length`. A filtragem por coordenadas ocorre no produtor Python.

**Captura antes do POST — somente listar:**

- `app/templates/ponto/bater_ponto.html:293–305`: suporte/API de geolocation, captura e preenchimento; timeout em :482–487; `!lat || !gpsCapturedAt || (Date.now() - gpsCapturedAt) > 60000` em :503; `if (gps)` em :505; `!lat || !lng` em :509.
- `app/templates/ponto/triagem/base_triagem.html:56–68`: suporte/API e captura para `.campo-lat/.campo-lng`.
- `app/templates/ponto/triagem/apto.html:200–205`: suporte/API e captura com timeout; `!lat || !gpsCapturedAt || (Date.now() - gpsCapturedAt) > 60000` em :217; `if (gps)` em :219; `lat && lng` em :221; `gpsLat == null || gpsLng == null` em :276.
- `app/templates/ponto/bater_ponto_v2.html:177`: `gpsLat!=null && gpsAccuracy!=null && acc>gpsAccuracy && (Date.now()-gpsCapturedAt)<120000`; suporte/API em :184–195; `gpsLat!=null` em :189; `gpsLat==null` em :204; suporte/timeout em :273–278; `gpsLat==null || gpsLng==null || !gpsCapturedAt || (Date.now()-gpsCapturedAt)>60000` em :292; `!(g && g.lat!=null)` em :294; `g && g.lat!=null` em :295.
- `app/static/pwa.js:289–301`: `capturarGPS`, suporte/API, resultado com coordenadas ou `null`.
- `app/static/pwa.js:394–411`: suporte/API e captura para POST `/ponto/ping-geo/`.
- `app/templates/core/onboarding.html:264–268`: teste de suporte e chamada geolocation para permissão; callback não lê latitude/longitude nessa chamada.

Excluídos das contagens Batida: testes de coordenadas **do Posto**, por exemplo `app/templates/ponto/bater_ponto_v2.html:139`, `app/templates/colaboradores/posto_form.html:103/:107/:211/:214`, `app/templates/colaboradores/postos.html:71`, `app/templates/colaboradores/partials/painel_posto.html:28`, `_postos_tabela.html:23`, `app/templates/core/painel_tv.html:154`; e testes `plat/plon` nos mapas (`geo-colab-map.js:66`, `fio_colaborador.html:319`, `painel_gestao.html:594`).

## 10/11 17:05 · sessao e54c7972b4a6 · - · - · origem=Ronald (tmux)

Encontrei quatro pontos materiais:

- **I7b aparece como violação.** O renderizador marca qualquer contador positivo com `VIOL` e vermelho, contrariando “número publicado sem esperado”. A descrição sozinha não altera essa classificação. `auditar_invariantes_chamados.py:74`, `:1901-1915`. [nao verificado]

- **União de dias pode atribuir HX ao dia errado.** O helper reúne datas do contexto, pai e turno da batida; basta qualquer uma responder `sem_celula`. Num conflito entre dia civil D+1 e turno D, a ausência de ata em D+1 pode esconder a violação de D com ata. O teste atual cobre somente `ultima_batida`, sem conflito entre fontes. `auditar_invariantes_chamados.py:32-43`, `:69-71`; `test_bo_disputa_sem_pergunta_memoria.py:366-370`. [nao verificado]

- **Vários motivos com donas diferentes vão todos para a primeira dona.** A primeira pergunta estrangeira interrompe a seleção; a recursão passa `motivos_lista` inteira à dona encontrada, e a agregação ignora os retornos seguintes do gerador. O teste cobre um único motivo. `disputa_emissao.py:555-586`, `:616-637`; `test_bo_disputa_sem_pergunta.py:68-93`. [nao verificado]

- **A tentativa inicial pode adquirir D depois de Ch.** O núcleo segura chamados vivos antes de gerar. Uma canônica de disputa fechada que precise rechaveamento adquire a dona em `models.py:2947-2950`, antes de o núcleo detectar o reuso e desfazer o ensaio. A segunda tentativa trava a dona corretamente, mas isso não corrige a ordem da primeira. Os testes substituem a porta e não exercitam esse caminho. `disputa_emissao.py:543-545`, `:615-618`; `test_bo_disputa_sem_pergunta.py:81`. [nao verificado]

A prova AST da API é razoável para a fronteira estática solicitada: verifica ordem das chamadas e ausência de `atomic` envolvendo o registro, além de `ATOMIC_REQUESTS` nos settings. Não é prova de execução da API. `test_bo_disputa_sem_pergunta_memoria.py:374-389`.

## 10/11 17:00 · sessao f212df288509 · BO-0611-05 · censo · origem=Code

Numeração dos anexos: primeira linha de conteúdo = 1, contando linhas vazias.

1. **(b) Falha da triagem.** O tema estava perguntado: “supoe outro significado (None x 0.0 x '', Decimal x float” (`preverif1.q:23`). O Codex viu: “geofence usa truthiness, enquanto os leitores alterados usam `is not None`; zero permanece na exclusão declarada” (`preverif1.r:37`). A triagem descartou literalmente: **“- 0.0 truthiness x is not None: fora do escopo declarado (brief volta 3); anexo ao opus.”** Motivo registrado: exclusão de escopo (`triagem_preverif1.txt:13`).

2. **(c) Fora do alcance — não conta.** O achado cobra evidência do RED **executado contra a base**, não apenas mutantes ou inspeção (`opus_final1.txt:5`). A pergunta exigia: “RED/GREEN exigidos (aval): POST /api/ponto/bater/ sem coordenadas, colab ativo, nos DOIS kernels -> HOJE 403 e 0 Batida” (`construtor1.q:33`). A resposta delimitou: “POSTs reais, renderizações Django e os 100/100 relatados não foram reexecutados aqui: **[nao verificado]**” (`preverif1.r:66`). Não há trecho de resposta comprovando esse RED na base; RED executado pertence expressamente à classe (c) (`CLAUDE.md:383`).

3. **(e) NÃO PERGUNTADO — falha de quem perguntou.** Não há trecho de pergunta solicitando conferir `LEI-AKITA`/`L-030` nas mensagens dos commits. O trecho próximo é “pouso antes do pre-requisito”, dentro da pergunta sobre classes de defeitos; isso pergunta por ordem do pouso, sem formular a conferência das marcações legais (`preverif1.q:24–26`; tema do achado em `opus_final1.txt:6`). Não há trecho correspondente nas `.r` fornecidas nem descarte correspondente na triagem (`triagem_preverif1.txt:1–13`). Respostas `construtor1.r`–`construtor6.r`: **[nao verificado]**, não fornecidas. [nao verificado]

4. **(e) NÃO PERGUNTADO — falha de quem perguntou.** A pergunta cita “PELA-METADE em app/docs/LEIS.md:78 e :165” como contexto do aval; não pergunta se esses registros ficariam falsos após a cura (`censo1.q:1–3`). A construção também usa “L-030 PELA-METADE” como especificação (`construtor1.q:2`). Não há trecho `.q` pedindo atualização documental por L-106, trecho `.r` apontando essa desatualização ou descarte correspondente na triagem (`opus_final1.txt:7`; `triagem_preverif1.txt:1–13`).

Evidência de avaliação por opus anterior, necessária para atribuir **(d)**: **[nao verificado]**, não fornecida.

| achado | classe | trecho .q | trecho .r | linha da triagem |
|---|---|---|---|---|
| 1 — zero/nulo | **(b)** falha da triagem | “supoe outro significado (None x 0.0 x ''” — `preverif1.q:23` | “geofence usa truthiness, enquanto os leitores alterados usam `is not None`” — `preverif1.r:37` | “- 0.0 truthiness x is not None: fora do escopo declarado (brief volta 3); anexo ao opus.” — `triagem_preverif1.txt:13` |
| 2 — RED na base | **(c)** não conta | “RED/GREEN exigidos […] HOJE 403 e 0 Batida” — `construtor1.q:33` | “POSTs reais […] não foram reexecutados aqui” — `preverif1.r:66`; não há comprovação do RED na base | Não há trecho correspondente — `triagem_preverif1.txt:1–13` |
| 3 — marcações legais dos commits | **(e)** não perguntado | Não há trecho específico; “pouso antes do pre-requisito” — `preverif1.q:25` — não formula essa conferência | Não há trecho correspondente nas respostas fornecidas | Não há trecho correspondente — `triagem_preverif1.txt:1–13` | [nao verificado]
| 4 — atualização de LEIS.md | **(e)** não perguntado | Não há pergunta de atualização; “PELA-METADE em app/docs/LEIS.md:78 e :165” é contexto — `censo1.q:2` | Não há trecho correspondente nas respostas fornecidas | Não há trecho correspondente — `triagem_preverif1.txt:1–13` |

## 10/11 16:58 · sessao e54c7972b4a6 · - · - · origem=Ronald (tmux)

Para HX :58, o dia vem do chamado órfão: o teste grava `contexto_json.data_turno`; o agrupador preserva seu ID em `DS.contexto.chamados_orfaos_ids` e no contexto do pai. O leitor exato do ramo sem origem é `data_evento_do_contexto`. ([teste](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_hx_disputa_muda_sem_ata.py:46), [IDs](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:1126), [leitura](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:1232)) [nao verificado]

Os outros leitores já usados para decidir a abstenção são `data_do_chamado`, no ramo por ata, e `resolver_data_evento`, no ramo com batida de origem. Este último devolve o dia do turno pela autoridade `_T1`. ([por ata](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:1169), [por origem](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:1205), [resolver](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/escala/servico_jornada.py:474))

Evite `dia_do_fato(..., fortes=False)` como substituto universal: seus últimos degraus aceitam o dia civil do instante observado. `fortes=True` encerra antes desses degraus. ([guarda](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/juizes.py:127), [significado civil](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/juizes.py:141))

O reuso conserva `ex.disputa_id`, mas pode costurar destino/célula antes de retornar. Portanto, o rollback precisa abranger toda a geração, inclusive perguntas próprias emitidas anteriormente na mesma lista. ([costura e retorno](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/models.py:2958))

A agregação na dona exige seleção explícita: o ramo atual consulta apenas disputas abertas. Uma dona fechada deve receber o motivo sem reabertura paralela; a necessidade dessa seleção decorre do filtro atual. ([seleção](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:532))

A dona fechada também deve entrar na trava antes de chamado/pergunta. A ordem declarada é Colaborador → Disputa → Chamado → Pergunta; após rollback do nascimento, reentrar na porta com a dona permite respeitá-la. ([contrato da trava](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/models.py:2629))

Nenhum arquivo escrito, banco ou rede acessado nesta exploração. [nao verificado]

## 10/11 16:57 · sessao e439e679001b · - · - · origem=Ronald (tmux)

**Achado 3: (e) NÃO PERGUNTADO.** Nenhuma `.q` solicita auditar mensagens de commit por `LEI-AKITA`/`L-030`, nem conferir `test_lei_protege_sitio` para esse pouso. O trecho mais próximo, em `preverif1.q`, é “**pouso antes do pre-requisito**”: pergunta sobre ordem/dependência do pouso, sem perguntar pelas marcações legais dos commits. Não há trecho `.r` que identifique a falta dessas marcações. A resposta apenas declara: “**Ordem efetiva das travas, liberação da vaga, pouso, smoke nas duas cascas com col677 e certificação independente do hash/base: [nao verificado]**” (`preverif1.r`, último parágrafo). Não há descarte correspondente na triagem fornecida. As linhas devem ser numeradas pelo texto do anexo; não conferi numeração. [nao verificado]

**Achado 4: (e) NÃO PERGUNTADO.** Citar o conteúdo de `LEIS.md` como especificação não equivale a perguntar se a documentação deveria ser atualizada após a obra. `censo1.q` abre com “**L-030 PELA-METADE em app/docs/LEIS.md:78 e :165**”; `construtor1.q` diz “**lei CLAUDE.md secao 4 / L-030 (app/docs/LEIS.md, linha L-030 PELA-METADE)**”. Nenhum desses trechos pede conferir a veracidade final de “BARRA em 4 casos”/“PELA-METADE” ou atualizar `LEIS.md` por L-106. Não há trecho `.r` diagnosticando documentação desatualizada, nem descarte desse diagnóstico na triagem fornecida.

**Ressalva para a consolidação:** a menção ao selo “VERMELHO” no achado 3 é uma alegação do opus anexado; sua execução não foi demonstrada por log nos anexos. Isso não torna o tema específico de marcação legal dos commits uma pergunta previamente feita. Não há evidência de opus anterior suficiente para sustentar classe (d). [nao verificado]

## 10/11 16:25 · sessao c29c5b84bc96 · DISPUTA-ZUMBI-1110 · censo · origem=Code

**A cópia contradiz a ausência de leitor após `sem_turno_aberto`:** o carimbo ocorre em `app/chamados/reconciliador.py:405`; depois, a mesma função consulta `pode_fechar()` em `:411` e chama `fechar()` em `:412`. O chamador indicado está em `app/chamados/reconciliador.py:1619`. A execução histórica desses trechos nas 18 disputas do anexo é [nao verificado].

Os números **18 abertas fecháveis, 10 com última resolução por `sem_turno_aberto`, pais 7 abertos/7 resolvidos/4 fechados** são os dados fornecidos pelo Ronald; não houve conferência independente em banco ou logs. [nao verificado]

C7 significa **nenhuma resposta sem veredito e nenhuma pergunta viva pelo C6**; não exige pai terminal nem disputa aberta. Essas condições adicionais pertencem aos chamadores. A disputa está aberta quando `fechada_em is None`; `estado` da pergunta é propriedade calculada, não coluna escrita. (`app/chamados/models.py:1842–1853`, `app/chamados/models.py:1822–1824`, `app/chamados/models.py:3033–3048`)

Para ler as tabelas, **B** significa o caminho comum de fechamento condicionado: `PerguntaDisputa.save()` → `post_save` → `on_commit` → `fechar_beco_da_disputa`. Ele **só chega a C7** se a disputa estiver aberta, o pai estiver em `TERMINAIS` e não existir pergunta com `respondida_em=None` cujo destino não seja terminal; há também guarda de reentrada. Portanto, B não significa consulta garantida a C7 após cada carimbo. (`app/chamados/signals.py:227–238`, `app/chamados/services/becos.py:25–33`, `app/chamados/services/becos.py:66–78`)

**1–2. Escritores persistentes dos campos da pergunta e fechamento posterior**

| Escritor físico | O que grava | Depois da escrita |
|---|---|---|
| `_carimbar_validacao` — `app/chamados/services/validacao.py:134–137` | `validada_em/por` | **B**, mais o fluxo normal de validação/materialização que termina em C7, detalhado abaixo. (`app/chamados/services/validacao.py:435–469`, `app/chamados/services/materializacao.py:1378`) |
| `devolver_validacao_recusada` — `app/chamados/services/validacao.py:199–202` | Zera `validada_em/por` | **B**. Nenhum fechamento direto na função; pode reabrir a disputa. O chamador normal depois materializa a disputa. (`app/chamados/services/validacao.py:209–230`, `app/chamados/services/validacao.py:464–469`) |
| `responder_pergunta_disputa_core` — `app/chamados/services/disputa_emissao.py:1467–1508` | Resposta, `respondida_em`, desfaz validação anterior e carimbo obsoleto | **B**. Nenhum C7 direto até o retorno; resposta de ausência chama materialização que consulta C7. (`app/chamados/services/disputa_emissao.py:1538–1559`, `app/chamados/services/materializacao.py:585–586`) |
| `responder_automaticamente` — `app/chamados/services/disputa_emissao.py:1652–1655` | Resposta e `respondida_em` | **B**. Chamador da foto valida pelo dono logo depois, alcançando a materialização. (`app/api/foto_recebida.py:43–51`) |
| `carimbar_resposta_pela_trilha` — `app/chamados/services/disputa_emissao.py:1674–1676` | `respondida_em` | **B**. Nenhum C7 direto na função ou no chamador `curar_perguntas_em_limbo`, que carimba/reabre e imprime o resultado. (`app/chamados/services/disputa_emissao.py:1677–1682`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:64–70`) |
| `PerguntaDisputa.carimbar` — `app/chamados/models.py:3128–3152` | `materializada_em/por`, `via_resolucao`; opcionalmente `respondida_em` e `validada_em/por` | Consulta **C6 antes de salvar**, não C7. Depois: **B**; fechamento explícito depende do chamador, discriminado na próxima tabela. (`app/chamados/models.py:3139–3157`) |
| `carimbar_identidade` — `app/chamados/models.py:3182–3190` | Carimbo/via; opcionalmente `recusa_motivo` | **B**. Nenhum C7 direto no corpo; fechamento depende do chamador. (`app/chamados/models.py:3168–3191`) |
| `descarimbar` — `app/chamados/models.py:3208–3212` | Zera materialização/autoria/via | **B**, quando `salvar=True`. Nenhum C7 direto no corpo; `salvar=False` apenas devolve campos para o chamador persistir. (`app/chamados/models.py:3203–3213`) |
| `descarimbar_em_lote` — `app/chamados/models.py:3264–3266` | `QuerySet.update`: zera materialização/autoria/via e recebe outros campos | **Nenhum** no corpo; retorna diretamente o `update`. Chamadores atuais: ressuscitação e reabertura com desfazimento de absorção. (`app/chamados/models.py:1566`, `app/chamados/services/disputa_emissao.py:1630`) |
| `reabrir_para_correcao` — `app/chamados/services/disputa_emissao.py:1593`, `:1629–1634` | `QuerySet.update`: zera resposta, resposta-em, validação; opcionalmente descarimba absorção pela porta anterior | **Nenhum C7/B próprio**: a escrita não passa por `post_save`; emite evento de célula explicitamente. (`app/chamados/services/disputa_emissao.py:1607–1609`, `:1629–1634`) |
| `encerrar_por_fato_no_espelho` — `app/chamados/models.py:3275–3276` | Troca a via para `fato_materializado` | **B** e fechamento explícito pelo comando chamador, após consultar `fio_vivo`. (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`, `:80–93`) |
| `relavrar` — `app/chamados/services/relavra_via.py:125–128` | Troca `via_resolucao` e atualiza lavra no mesmo `save` | **B**. Nenhum C7 direto na função nem no comando chamador. (`app/chamados/services/relavra_via.py:129–139`, `app/chamados/management/commands/relavrar_via_i6.py:100–104`) |
| `_nascer`, reuso — `app/chamados/models.py:2965–3006` | Pode trocar célula e destino; atualiza lavra | **B**. Após salvar, acorda célula anterior e retorna; nenhum C7 direto nessa cauda. (`app/chamados/models.py:3006–3008`) |
| `_nascer`, inserção — `app/chamados/models.py:3014–3017` | Identidade, disputa, motivo, destino, célula, conversa, dia e `**campos`, que podem conter assinaturas recebidas | **B**, inclusive no nascimento. Nenhum C7 direto antes do retorno. (`app/chamados/models.py:3018–3031`, `app/chamados/signals.py:228–229`) |
| `reapontar_resolvedora` — `app/chamados/services/fio.py:879`, `:925–928` | Destino e, ao datar linha sem dia, `data_evento`; atualiza lavra | **B**. Nenhum C7 direto na cauda; acorda também a célula anterior. (`app/chamados/services/fio.py:929–935`) |
| `rechavear_data_evento` — `app/chamados/services/fio.py:1037–1043` | Dia, célula e lavra | **B**. Nenhum C7 direto na cauda; acorda célula anterior e registra trilha. (`app/chamados/services/fio.py:1044–1049`) |

O ponto que atribui `data_evento` é `gravar_dia_do_fato`; ele apenas modifica memória, e os três fluxos persistentes estão na tabela acima. `_escrever_carimbo` também apenas atribui em memória; quem persiste são `carimbar`, `carimbar_identidade` e `descarimbar`. (`app/chamados/services/fio.py:790–801`, `app/chamados/models.py:3097–3101`)

Abaixo estão os **gatilhos de carimbo**, distinguindo os caminhos que compartilham o mesmo escritor físico:

| Carimbador/gatilho | Consulta C7 ou chama fechamento depois? Onde e condições |
|---|---|
| `lavrar_sem_turno_aberto` → `carimbar` — `app/chamados/reconciliador.py:405` | **Sim, direto:** aberta + C7 em `:411`; `fechar` em `:412`. Sem condição adicional de status do pai, flag ou tranca nesse fechamento. (`app/chamados/reconciliador.py:411–414`) |
| `retratar_perguntas_absolvidas_por_celula` → `carimbar(celula_absolveu)` — `app/chamados/reconciliador.py:595` | **Sim:** após o laço, relê `fechada_em`, consulta C7 e fecha. (`app/chamados/reconciliador.py:608–616`) |
| `encerrar_orfas_pela_lampada` → `encerrar_fantasma` → `carimbar` — `app/chamados/reconciliador.py:1147`, `app/chamados/models.py:3287` | **Sim:** após o laço; barra fechada e resposta sem veredito antes de C7/`fechar`. (`app/chamados/reconciliador.py:1158–1174`) |
| Validação normal → `_carimbar_validacao` — `app/chamados/services/validacao.py:435` | **Sim pelo fluxo:** unidade individual, eventual devolução da validação e materialização da disputa; esta chama `_fechar_disputa_se_quitada`, que consulta C7 e fecha. (`app/chamados/services/validacao.py:461–469`, `app/chamados/services/materializacao.py:1375–1379`, `:1316–1333`) |
| Aceite de folga → validação + `fechar_por_admin` — `app/chamados/services/validacao.py:288–296`, `:320–329` | **B**; também fecha o fio do dia, reconcilia chamados e, quando há transição a encerrado, alcança S143. Não há consulta direta a C7 nesse ramo de aceite. (`app/ponto/services/veredito_celula.py:179–205`, `app/chamados/signals.py:90–115`) |
| `_materializar_ausencia_de_resposta` → carimbo das irmãs — `app/chamados/services/materializacao.py:549–566` | **Sim, direto:** C7/`fechar` em `:585–586`, sem exigir pai terminal. A reconciliação dos destinos tem condição separada: ausência fora de `ABERTAS`. (`app/chamados/services/materializacao.py:580–586`) |
| `_materializar_uma` → declaração/via do fato — `app/chamados/services/materializacao.py:1277–1278` | **Sim pela cauda dos chamadores:** laço normal em `:1378`; validação em `app/chamados/services/validacao.py:469`; passivo em `app/chamados/services/passivo_validado.py:405`. Todos chegam a C7 em `app/chamados/services/materializacao.py:1316`. |
| Recusa de paridade → identidade `paridade_recusada` — `app/chamados/services/materializacao.py:924` | **B** e cauda da materialização nos chamadores anteriores; a unidade não consulta C7 por si. (`app/chamados/services/materializacao.py:1299`, `:1375–1379`) |
| `_carimbar_recusa`, colisão → identidade `fato_ocupado` — `app/chamados/services/materializacao.py:1093–1100` | **B**. Nenhum C7 dentro da função; integra a materialização. Há também gravação de recusa pelo passivo após desfazer o grupo por geometria. (`app/chamados/services/materializacao.py:1101–1107`, `:1296–1299`, `app/chamados/services/passivo_validado.py:543–546`) |
| `materializar_pendentes_aprovadas` → `juiz_aprovou` — `app/chamados/services/materializacao.py:1431` | **B**, nenhum C7 direto na função. Seu gatilho é o signal de disputa **já fechada**; pula pergunta em voo e lastro reprovado. (`app/chamados/signals.py:147–165`, `app/chamados/services/materializacao.py:1425–1433`) |
| S143, `_resolver_perguntas_por_chamado_resolvido`, ramo por destino — `app/chamados/services/disputa_emissao.py:1863–1864` | **Sim pela cauda:** C7/`fechar` em `:1963–1964`; pula fechamento se houver pergunta em voo. O encerramento pela tranca retorna antes desse trabalho. (`app/chamados/services/disputa_emissao.py:1818–1819`, `:1961–1964`) |
| Mesmo S143, ramo sem credor — `app/chamados/services/disputa_emissao.py:1935–1937` | **Sim, mesma cauda.** Esse ramo de carimbo exige pai e destinos fora de `VIVOS`. (`app/chamados/services/disputa_emissao.py:1909–1915`, `:1961–1964`) |
| `DisputaSupervisao.fechar`, exceções admin/desligamento → `fechar_por_admin` — `app/chamados/models.py:1918–1921` | **Já está na porta de fechamento:** após carimbar perguntas, grava o fechamento da disputa. As exceções permitem pergunta viva, mas continuam recusando resposta sem veredito. (`app/chamados/models.py:1886–1897`, `:1932–1936`) |
| `remover_motivo` → identidade `motivo_retratado` — `app/chamados/models.py:2061–2062` | **B**; diretamente, só chama `fechar` quando não resta motivo. Se restarem, retorna sem fechamento direto. (`app/chamados/models.py:2066–2072`) |
| `retratar_duplicata` → identidade `duplicata` — `app/chamados/services/fio.py:1359–1362` | **B**. Nenhum C7/fechamento direto na função nem em seu chamador imediato `retratar_para_canonica`. (`app/chamados/services/fio.py:1336–1342`, `:1345–1363`) |
| `ressuscitar_perguntas` → descarimbo em lote — `app/chamados/models.py:1566` | **Nenhum fechamento nesse caminho lido:** faz `update`, emite evento e reabre disputas fechadas; não passa por B. (`app/chamados/models.py:1565–1575`) |
| `desfazer_carimbo_sem_lastro` → `descarimbar` — `app/chamados/services/relavra_via.py:246` | **B**. Nenhum fechamento direto; reabre disputa fechada. (`app/chamados/services/relavra_via.py:259–263`) |
| `reabrir_fantasma` → `descarimbar` — `app/chamados/models.py:3298` | **B**. Nenhum fechamento direto; o comando chamador reabre disputa e pai. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:157–172`) |
| `reabrir_por_competencia` → `descarimbar` — `app/chamados/models.py:3322` | **B**. Nenhum fechamento direto no caminho lido; o serviço chama essa porta e reabre chamados da competência. (`app/chamados/services/competencia_trancada.py:251–273`) |
| `encerrar_perguntas_com_fato` → troca de via — `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68` | **Sim, porta diretamente:** sob `--apply`, consulta `fio_vivo` e chama `fechar`; contabiliza recusa da porta. Não chama nominalmente C7. (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:53–55`, `:80–93`) |

Há ainda estes saves auxiliares, para não confundi-los com coluna de estado ou deixar `bulk_update` fora do inventário:

| Escritor auxiliar | Campos e consequência |
|---|---|
| `_materializar_resposta_hora` — `app/chamados/services/materializacao.py:850–851` | `pendencia_admin/recusa_motivo`; **B**, mais o fluxo da unidade/materialização. (`app/chamados/services/materializacao.py:1375–1379`) |
| `_carimbar_recusa`, sem colisão — `app/chamados/services/materializacao.py:1101–1103` | Só motivo; **B**, nenhum C7 na função. (`app/chamados/services/materializacao.py:1104–1107`) |
| `recusar_pergunta` — `app/chamados/services/validacao.py:584–594` | Salva motivo e reabre resposta por `update`; **B** do save do motivo observa o estado final no commit. Nenhum fechamento direto nessa cauda. (`app/chamados/services/validacao.py:595–610`) |
| `conferir_data` — `app/chamados/services/validacao.py:664–666` | Assinatura da conferência; **B**, sem C7 direto até retornar. (`app/chamados/services/validacao.py:667–675`) |
| `veredito_lavrado.lavrar` — `app/chamados/veredito_lavrado.py:270–275` | Cache `veredito_lavrado/julgado_em/janela_expira_em`; save individual dispara **B**. C6 consulta o juiz do fato, não esse cache. (`app/chamados/juizes.py:1405–1406`) |
| `gravar_lavras_em_lote` — `app/chamados/veredito_lavrado.py:237–250` | **`bulk_update`** dos três campos do cache; nenhum C7/fechamento/signal nessa porta. (`app/chamados/veredito_lavrado.py:240–251`) |
| Migração `_preencher_chave` — `app/chamados/migrations/0051_decisao_pergunta.py:25–27` | Dois `QuerySet.update`: colaborador e instância de marco; nenhum C7 ou fechamento no corpo. (`app/chamados/migrations/0051_decisao_pergunta.py:22–27`) |

O outro signal da pergunta emite `EventoCelula` para os campos relevantes; **seu corpo não consulta C7 nem fecha disputa**. A lista inclui resposta, validação, materialização, via, dia e FKs. (`app/chamados/signals.py:250–257`, `app/chamados/models.py:3220–3222`)

**3. Escritores do fechamento da disputa**

O **único escritor físico de fechamento encontrado é `DisputaSupervisao.fechar`**: atribui `fechada_em`, autoria, resolução e motivo, salvando os quatro juntos. Não há coluna `status` de disputa sendo fechada por outra porta. (`app/chamados/models.py:1932–1936`, `app/chamados/models.py:1822–1824`)

Os chamadores encontrados dessa porta são:

| Origem | Sítios |
|---|---|
| Reconciliador | `app/chamados/reconciliador.py:412`, `:612`, `:1171` |
| Materialização | `app/chamados/services/materializacao.py:586`, `:1333` |
| Emissão/S143 | `app/chamados/services/disputa_emissao.py:1284`, `:1964` |
| Beco | `app/chamados/services/becos.py:57` |
| Competência trancada | `app/chamados/services/competencia_trancada.py:204` |
| Entrada sincronizada | `app/ponto/services/entrada_sincronizada.py:59` |
| Encerramento administrativo e retirada do último motivo | `app/chamados/models.py:1950`, `:2072` |
| Foto recebida, após validar pelo dono | `app/api/foto_recebida.py:79–80` |
| Desligamento, por exceção nomeada | `app/colaboradores/services/desligamento.py:139–142` |
| Comando de perguntas com fato | `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` |

`fechar()` **não chama literalmente `pode_fechar()`**: consulta respostas sem veredito e julga perguntas por C6, aplicando as exceções nomeadas. Exige usuário e toma as travas antes de persistir. (`app/chamados/models.py:1876–1912`)

Os outros escritores de `fechada_em` são **reabridores**, que gravam `None`: `reabrir_admin` e `reabrir_sistema`. (`app/chamados/models.py:2149–2161`, `app/chamados/models.py:2173–2177`)

**4. Cron/vigia**

Existe **vigia parcial**, `fechar_becos_disputa --alarme`, cadastrado para 07:10. Ele percorre abertas, mas usa o recorte de beco — pai terminal e nenhuma pergunta visível pelo filtro — antes de consultar C7. (`app/config/crons.py:663`, `app/chamados/management/commands/fechar_becos_disputa.py:32–43`, `app/chamados/services/becos.py:25–33`)

Com `--alarme`, **conta/lista/alerta, sem fechar**. Com `--apply`, o mesmo comando chama a porta de fechamento de beco. (`app/chamados/management/commands/fechar_becos_disputa.py:30`, `:54–70`)

Não encontrei varredor do universo completo **“disputa aberta com C7=True”**, sem aquele recorte. O comando antigo `reconciliar_disputa_zumbi` consta como removido; `disputa_vazia` conta fechadas pelo motivo `vazia`, e a auditoria de abertas sem motivo também não é esse contador. (`app/config/crons.py:840`, `app/chamados/services/vigia_fio.py:727–734`, `app/chamados/management/commands/auditar_disputas.py:166–177`)

**5. Todos os chamadores nominais de `pode_fechar()` encontrados fora de `tests/`**

| Chamador | Leitura e uso |
|---|---|
| `lavrar_sem_turno_aberto` | Fecha se aberta e C7. (`app/chamados/reconciliador.py:411–412`) |
| Retratação por célula | Fecha após o laço. (`app/chamados/reconciliador.py:611–612`) |
| Órfãs pela lâmpada | Fecha, após barrar resposta sem veredito. (`app/chamados/reconciliador.py:1163–1171`) |
| Ausência da resposta | Fecha. (`app/chamados/services/materializacao.py:585–586`) |
| Cauda da materialização | Fecha. (`app/chamados/services/materializacao.py:1316–1333`) |
| Emissão a partir de órfãos | Se houve abstenção por falta de ata, só acende aviso; caso contrário, fecha. (`app/chamados/services/disputa_emissao.py:1278–1284`) |
| S143 | Fecha, salvo pergunta em voo. (`app/chamados/services/disputa_emissao.py:1961–1964`) |
| `competencia_trancada.medir` | Seleciona; pai vivo fora do conjunto a encerrar impede inclusão. (`app/chamados/services/competencia_trancada.py:153–158`) |
| `competencia_trancada.encerrar` | Fecha sob `aplicar`, competência trancada e ações existentes. (`app/chamados/services/competencia_trancada.py:167–168`, `:202–205`) |
| `becos.classificar` | Só classifica, condicionado ao recorte. (`app/chamados/services/becos.py:25–33`) |
| `becos.fechar_beco` | Resolve perguntas dos chamados terminais, relê e fecha. (`app/chamados/services/becos.py:41–57`) |
| Entrada sincronizada | Fecha disputa do pai manual que agrupa somente aquele órfão. (`app/ponto/services/entrada_sincronizada.py:47–61`) |

Há **outro significado publicado com o nome `pode_fechar`**: o explicador calcula somente `not fio_vivo(ps)` e devolve essa chave, sem a trava adicional `respostas_sem_veredito()` do C7. Seus consumidores dizem “Tudo validado e gravado” e oferecem “Fechar/regularizar”. (`app/chamados/services/explicador.py:134–163`, `app/chamados/models.py:1853`, `app/chamados/services/desenlace.py:63–65`, `app/templates/chamados/partials/modal_fio.html:434–445`)

**6. Chamado-pai**

Quem reage ao fechamento da disputa é `disputa_fechada_fecha_chamado_pai`: busca o pai e chama **`ch.reconciliar()`**, salvo criação, ausência de pai, ausência de fechamento ou pai já em `ENCERRADOS`. O receiver engole falhas; também aceita que o gate recuse a reconciliação. (`app/chamados/signals.py:18–50`)

O fluxo normal leva o pai a **`resolvido`**, pelo motor e por `_gravar_estado`, que atribui e salva `status_local`. A reconciliação pode recusar por premissa ou pergunta viva. (`app/chamados/catalogo/motor.py:111`, `app/chamados/models.py:641–655`, `app/chamados/models.py:682–689`, `app/chamados/models.py:228`, `app/chamados/models.py:296`)

**A relação inversa não exige disputa fechada como condição u

[truncado em 20000 de 21254 caracteres]

## 10/11 16:22 · sessao a50c10442bff · - · - · origem=Ronald (tmux)

Censo complementar, fora de `models.py`/`reconciliador.py`, para consolidação:

**Regra comum aos `save()` abaixo:** o `post_save` de PerguntaDisputa agenda `fechar_beco_da_disputa` em `on_commit` (`app/chamados/signals.py:227-238`). Essa porta **só consulta C7 se** a disputa estiver aberta, não houver pergunta com `respondida_em=None` e destino não terminal, e o pai estiver em `TERMINAIS` (`app/chamados/services/becos.py:21-33`). Tem guarda de reentrada (`app/chamados/services/becos.py:70-78`). Portanto, “tem signal” não equivale a “sempre pergunta C7” (`app/chamados/services/becos.py:29-33`).

| Carimbador / campos | Depois do carimbo, fechamento no fluxo |
|---|---|
| `_carimbar_validacao`: `validada_em/por`, save (`app/chamados/services/validacao.py:134-141`) | Fluxo normal: `validar_pergunta` chama materialização (`app/chamados/services/validacao.py:435-469`), cujo laço termina na cauda (`app/chamados/services/materializacao.py:1375-1379`) que pergunta C7 e fecha (`app/chamados/services/materializacao.py:1316-1333`); também signal condicionado acima. |
| Aceite de folga usa o mesmo carimbo e `fechar_por_admin` (`app/chamados/services/validacao.py:288-296`, `:320-329`) | Chama `fechar_o_fio_do_dia` diretamente ou por `declarar_dia`; essa porta fecha perguntas por admin e reconcilia chamados (`app/ponto/services/veredito_celula.py:179-205`), acionando S143 quando status é encerrado (`app/chamados/signals.py:90-115`); também signal condicionado. |
| `devolver_validacao_recusada`: zera `validada_em/por` (`app/chamados/services/validacao.py:195-208`) | Nenhum C7 direto na função; pode reabrir a disputa (`app/chamados/services/validacao.py:209-230`). Seu chamador normal depois materializa a disputa (`app/chamados/services/validacao.py:464-469`); também signal condicionado. |
| `responder_pergunta_disputa_core`: `resposta_colab`, `respondida_em`, zera validação e descarimba carimbo obsoleto (`app/chamados/services/disputa_emissao.py:1467-1508`) | Nenhum C7 direto até retorno (`app/chamados/services/disputa_emissao.py:1513-1559`); signal condicionado. Se resposta vira ausência, chama `_materializar_ausencia_de_resposta` (`app/chamados/services/disputa_emissao.py:1538-1541`), que pergunta C7/fecha (`app/chamados/services/materializacao.py:585-586`). Chamador de correção administrativa também chama `validar_pergunta` após responder (`app/chamados/services/acoes_disputa.py:370-380`). |
| `responder_automaticamente`: `resposta_colab/respondida_em` (`app/chamados/services/disputa_emissao.py:1647-1658`) | Signal condicionado; chamador direto da foto depois invoca `validar_pergunta` (`app/api/foto_recebida.py:43-51`) e alcança cauda C7 da materialização. |
| `carimbar_resposta_pela_trilha`: `respondida_em` (`app/chamados/services/disputa_emissao.py:1670-1682`) | Nenhum C7 direto; signal condicionado. Chamador `curar_perguntas_em_limbo` só executa essa porta/reabertura e imprime resultado (`app/chamados/management/commands/curar_perguntas_em_limbo.py:64-70`). |
| `reabrir_para_correcao`: `respondida_em=None`, `validada_em/por=None`, `resposta_colab=''`, opcionalmente descarimba absorção (`app/chamados/services/disputa_emissao.py:1593`, `:1629-1634`) | **QuerySet.update**, ou `descarimbar_em_lote`: não passa pelo signal de `save`; não consulta C7, emite evento de célula explicitamente (`app/chamados/services/disputa_emissao.py:1607-1609`, `:1630-1634`). Chamadores de UI seguem com nota/push/log (`app/chamados/views.py:1435-1457`; `app/chamados/services/acoes_disputa.py:102-114`). |
| `recusar_pergunta`: grava `recusa_motivo`, depois usa reabertura acima (`app/chamados/services/validacao.py:584-594`) | O save do motivo agenda signal condicionado; no commit ele observa o estado após a reabertura. Nenhum C7 direto até retorno (`app/chamados/services/validacao.py:595-610`). |
| `_materializar_ausencia_de_resposta`: carimba irmãs, via declaração/ausência, `resposta='validar'` (`app/chamados/services/materializacao.py:549-566`) | C7 e `fechar` explícitos (`app/chamados/services/materializacao.py:585-586`), sem exigir pai terminal. Destino só reconcilia se ausência não está em `ABERTAS` (`app/chamados/services/materializacao.py:580-582`). |
| `_materializar_uma`: carimbo declaração/via do fato, resposta validada, lavra de veredito (`app/chamados/services/materializacao.py:1277-1282`); ramo paridade usa identidade (`app/chamados/services/materializacao.py:924`) | Chamadores: laço normal chama cauda C7 (`app/chamados/services/materializacao.py:1375-1379`); validação individual depois chama o laço (`app/chamados/services/validacao.py:463-469`); passivo chama unidade (`app/chamados/services/passivo_validado.py:516`) e cauda uma vez por disputa (`app/chamados/services/passivo_validado.py:401-406`). Também signal condicionado. |
| `_carimbar_recusa`: colisão → identidade `fato_ocupado`; demais → `recusa_motivo` (`app/chamados/services/materializacao.py:1089-1107`) | Nenhum C7 direto dentro da função; integra unidade/materialização acima; também signal condicionado. Passivo pode gravar recusa de geometria após rollback (`app/chamados/services/passivo_validado.py:543-546`). |
| `_materializar_resposta_hora`: `pendencia_admin/recusa_motivo` (`app/chamados/services/materializacao.py:845-851`) | Nenhum C7 direto nesse sítio; pertence à unidade/laço/cauda acima; signal condicionado. |
| `materializar_pendentes_aprovadas`: carimbo `juiz_aprovou` (`app/chamados/services/materializacao.py:1418-1433`) | Nenhum C7 direto na função. Chamador é signal de disputa **já fechada**, condicionado a `fechada_em` preenchido (`app/chamados/signals.py:147-165`); seus saves também agendam Beco. Guarda pergunta em voo e lastro (`app/chamados/services/materializacao.py:1425-1431`). |
| `_resolver_perguntas_por_chamado_resolvido`: carimbo por fato com `resposta='validar'` (`app/chamados/services/disputa_emissao.py:1863-1867`); ramo sem credor carimba com `resposta='responder'` (`:1935-1947`) | Cauda explícita C7/fechar (`app/chamados/services/disputa_emissao.py:1963-1964`). Não roda para encerramento por tranca (`:1818-1819`); não fecha disputa com pergunta em voo (`:1961-1962`). Ramo sem credor exige pai e destinos fora de `VIVOS` (`:1909-1915`). Também signal condicionado. |
| `relavrar`: troca `via_resolucao` e lavra veredito no mesmo save (`app/chamados/services/relavra_via.py:125-139`) | Nenhum C7 direto; signal condicionado. Chamador command só chama `relavrar` e imprime (`app/chamados/management/commands/relavrar_via_i6.py:100-104`). |
| `desfazer_carimbo_sem_lastro`: `p.descarimbar()` (`app/chamados/services/relavra_via.py:242-247`) | Nenhum C7 direto; se disputa fechada reabre (`app/chamados/services/relavra_via.py:259-263`); signal condicionado do descarimbo via save. |
| `retratar_duplicata`: identidade `duplicata`, conserva carimbo anterior, grava motivo (`app/chamados/services/fio.py:1357-1363`) | Nenhum C7 direto na função ou chamador imediato `retratar_para_canonica` (`app/chamados/services/fio.py:1336-1342`); signal condicionado. |
| Command `encerrar_perguntas_com_fato`: `encerrar_por_fato_no_espelho` (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:65-76`) | Após carimbos, testa `fio_vivo`, então chama `d.fechar()`; sem chamada nominal a C7 no command (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:80-93`). |
| `conferir_data`: `data_conferida_em/por` (`app/chamados/services/validacao.py:664-666`) | Nenhum C7 direto até retorno (`app/chamados/services/validacao.py:667-674`); signal condicionado. |

Outros gatilhos de reversão: ausência rejeitada chama reabertura com `desfazer_absorcao=True` e reabre disputas (`app/chamados/services/materializacao.py:679-690`); reabrir órfãs usa `reabrir_fantasma` e reabre disputa/pai (`app/chamados/management/commands/reabrir_orfas_enterradas.py:157-172`); competência reaberta chama `reabrir_por_competencia` e reabre chamados (`app/chamados/services/competencia_trancada.py:251-265`).

`veredito_lavrado.lavrar` tem save individual (`app/chamados/veredito_lavrado.py:270-275`), enquanto `gravar_lavras_em_lote` usa `bulk_update` (`:237-250`): o segundo não tem signal/C7 dentro da porta (`:240-251`). O cron de stale chama `lavrar(p)` individualmente (`app/chamados/management/commands/lavrar_perguntas_stale.py:87-92`).

Verificação estática adicional: executei em memória o próprio censo AST, sem importar Django e com `python3 -B`; seus critérios de atribuição e limites estão documentados em `app/core/censo_escritas.py:6-51`. Ele reconhece os saves de PD em `models.py:3006,3152,3190,3212,3276`, update em `:3264`, e os sítios externos da tabela; não deve ser apresentado como prova sem limites porque seu balde não atribuível é explícito (`app/core/censo_escritas.py:43-51`).

Não encontrei escritor de PerguntaDisputa em `flip_auto.py`: as referências ao modelo nesse arquivo apenas consultam recusas de paridade (`app/ponto/services/flip_auto.py:125-140`).

## 10/11 16:22 · sessao 8ca827f1088e · - · - · origem=Ronald (tmux)

Censo de fechamento/leitores concluído sobre arquivos acessíveis, sem executar Django, banco ou logs. As contagens e situações de produção permanecem exclusivamente as fornecidas pelo usuário. [não verificado]

**Escritores físicos do estado da disputa**

| Ato | Escritor |
|---|---|
| Fechar | **Único sítio encontrado:** `DisputaSupervisao.fechar`: atribui `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` e salva os quatro campos — `app/chamados/models.py:1932-1936`. |
| Reabrir administrativamente | Zera os quatro campos; salva em `app/chamados/models.py:2149-2161`. |
| Reabrir pelo sistema | Zera os quatro campos; salva em `app/chamados/models.py:2173-2177`. |
| Definição do estado | A disputa não usa campo `status`: `aberta` deriva de `fechada_em is None` — `app/chamados/models.py:1822-1824`; timestamp declarado em `:1757`. |

A porta canônica exige usuário, recusa resposta sem veredito em qualquer modalidade e, sem exceção nomeada, recusa perguntas vivas; exceções são desligamento, admin encerra e competência trancada. Ela toma a ordem global de travas antes de gravar. `app/chamados/models.py:1876-1912`.

O antigo bulk update de desligamento **já saiu**: o código atual chama a porta `fechar(..., motivo=FECHOU_DESLIGAMENTO)` e pula disputa com respostas sem veredito. `app/colaboradores/services/desligamento.py:132-142`. O comentário de exceção no instrumento `bin/esmeril_chamado_censo.py:64-71` está desatualizado frente a esse corpo.

**Todos os 12 chamadores reais de `.pode_fechar()` fora de `tests/` encontrados por AST**

| Função | Leitura / consequência |
|---|---|
| `lavrar_sem_turno_aberto` | `app/chamados/reconciliador.py:411` → `fechar` em `:412`. **Há chamada explícita nesta cópia**; distinguir os retornos anteriores, com o agente específico. |
| `retratar_perguntas_absolvidas_por_celula` | `app/chamados/reconciliador.py:611` → `fechar` em `:612`. |
| `encerrar_orfas_pela_lampada` | `app/chamados/reconciliador.py:1170` → `fechar` em `:1171`; respostas sem veredito são barradas previamente em `:1163-1169`. |
| `_materializar_ausencia_de_resposta` | `app/chamados/services/materializacao.py:585` → `fechar` em `:586`. |
| `_fechar_disputa_se_quitada` | `app/chamados/services/materializacao.py:1316` → `fechar` em `:1333`. |
| `criar_disputa_a_partir_de_chamados_orfaos` | `app/chamados/services/disputa_emissao.py:1278`; se `_absteve_por_ata`, apenas acende aviso `:1279-1281`; caso contrário fecha em `:1284`. |
| `_resolver_perguntas_por_chamado_resolvido` | `app/chamados/services/disputa_emissao.py:1963` → `fechar` em `:1964`; pergunta em voo impede a leitura/fecho em `:1961-1962`. |
| `competencia_trancada.medir` | `app/chamados/services/competencia_trancada.py:153`: apenas seleciona; pai vivo fora do conjunto a encerrar impede inclusão em `:155-158`. |
| `competencia_trancada.encerrar` | `app/chamados/services/competencia_trancada.py:202` → porta de exceção em `:204-205`; depende de `aplicar`, competência trancada e ações em `:167-168`. |
| `becos.classificar` | `app/chamados/services/becos.py:33`: apenas classifica; **só chega ao C7 se pai terminal e nenhuma pergunta “visível”** em `:27-32`. |
| `becos.fechar_beco` | `app/chamados/services/becos.py:56` → `fechar` em `:57`; toma travas, resolve perguntas dos chamados terminais e relê disputa em `:41-55`. |
| `resolver_cobranca_da_entrada` | `app/ponto/services/entrada_sincronizada.py:58` → `fechar` em `:59`; recorte é pai manual com somente o órfão atual em `:47-56`. |

`fechar()` não chama literalmente `.pode_fechar()`: aplica a mesma composição por `respostas_sem_veredito()` e perguntas julgadas por `pergunta_viva`, com as exceções nomeadas. `app/chamados/models.py:1886-1903`. O método C7 reúne resposta sem veredito e fio vivo. `app/chamados/models.py:1848-1853`.

**Leitor indireto que deve constar na tabela de TODOS os carimbadores por `save()`**

Cada `PerguntaDisputa.save()` alcança `_becos_post_save_pergunta`, que agenda `on_commit` → `fechar_beco_da_disputa`. `app/chamados/signals.py:227-238`. Essa entrada relê a disputa e exige classificação `FECHAVEL`. `app/chamados/services/becos.py:66-78`.

Logo, para carimbador por `save()`, escrever “nenhum” absoluto seria incorreto: existe leitor indireto **condicional**. O classificador sai antes do C7 se houver qualquer pergunta com `respondida_em is None` e destino não terminal, ou se o pai não for terminal; não considera `materializada_em`/`validada_em` nesse pré-filtro. `app/chamados/services/becos.py:25-33`. O próprio comentário declara que `update()` direto nasce sem signal. `app/chamados/management/commands/fechar_becos_disputa.py:9-11`.

Há também post-save síncrono emitindo evento da célula quando os campos acordadores mudam, mas seu corpo não chama C7 nem fecha a disputa: chama `PerguntaDisputa.acordar_celulas(...)`. `app/chamados/signals.py:250-257`.

**Cron/vigia**

Existe o vigia parcial de becos: a configuração agenda `fechar_becos_disputa --alarme` às 07:10. `app/config/crons.py:663`. Ele varre disputas abertas, porém classifica pelo recorte de beco acima. `app/chamados/management/commands/fechar_becos_disputa.py:32-43`; `app/chamados/services/becos.py:25-33`.

Com `--alarme`, apenas lista/conta e termina com erro se achar candidato; **não fecha**. `app/chamados/management/commands/fechar_becos_disputa.py:30`, `:54-70`. O comando preserva `--apply` e aí chama `becos.fechar_beco`. `:23`, `:57-64`.

**Não encontrei varredor de todo o universo “aberta + C7 True”, sem o recorte de beco.** A lista exaustiva de chamadas acima contém somente o classificador parcial como leitura usada pelo cron. O antigo comando `reconciliar_disputa_zumbi` está registrado como removido. `app/config/crons.py:840`. O contador `disputa_vazia` mede **fechadas com motivo vazia**, não abertas fecháveis. `app/chamados/services/vigia_fio.py:727-734`. `auditar_disputas` conta abertas sem motivos, não C7. `app/chamados/management/commands/auditar_disputas.py:166-177`.

**Outro significado chamado “pode_fechar”**

O explicador **não chama o método C7**: calcula somente `not fio_vivo(ps)` e devolve chave `pode_fechar`; não acrescenta a lista `respostas_sem_veredito()` do C7. `app/chamados/services/explicador.py:134-140`, `:163`; comparar `app/chamados/models.py:1853`.

Essa chave alimenta a afirmação “Tudo validado e gravado” / “Fechar/regularizar” em `app/chamados/services/desenlace.py:63-65` e os textos/cores do modal em `app/templates/chamados/partials/modal_fio.html:434`, `:445`. O fio monta esse explicador em `app/chamados/services/fio.py:304-312`.

A guarda S127 também usa outro recorte: disputa aberta sem nenhuma pergunta, ou `fio_vivo(perguntas)`; não chama C7. `app/chamados/juizes.py:2308-2322`.

**Todos os chamadores de fechamento encontrados (delegam ao escritor único)**

- `app/chamados/models.py:1950` (`admin_encerra`), `:2072` (`remover_motivo`, apenas se não sobra motivo, `:2066`).
- `app/chamados/reconciliador.py:412`, `:612`, `:1171`.
- `app/chamados/services/materializacao.py:586`, `:1333`.
- `app/chamados/services/disputa_emissao.py:1284`, `:1964`.
- `app/chamados/services/becos.py:57`.
- `app/chamados/services/competencia_trancada.py:204`.
- `app/ponto/services/entrada_sincronizada.py:59`.
- `app/api/foto_recebida.py:80`: valida pelo dono antes; fecha diretamente quando não restam motivos além de foto (`:70-80`), sem consulta explícita ao C7.
- `app/colaboradores/services/desligamento.py:142`.
- `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`: sob `--apply` (`:53-55`), consulta `fio_vivo` em `:83`, chama porta e contabiliza recusa `ValueError` em `:92-93`.

**Chamado-pai**

Quando a disputa salva fechada, `disputa_fechada_fecha_chamado_pai` procura o pai e chama `ch.reconciliar()`, em atomic; não atua se criado, sem pai, sem timestamp ou pai já resolvido/fechado; falhas são engolidas com registro. `app/chamados/signals.py:18-50`.

`reconciliar()` resolve pelo motor e `_gravar_estado`; o destino normal é `resolvido`, não `fechado`. `app/chamados/models.py:641-655`; `app/chamados/catalogo/motor.py:111`. Pode recusar por premissa ou pergunta viva. `app/chamados/models.py:682-689`.

O escritor físico do pai é `_gravar_estado`: atribui `status_local` em `app/chamados/models.py:228`, salva em `:296`. O fechamento da disputa **não garante** fechamento do pai, por essas recusas e pelo `except` do receiver. `app/chamados/signals.py:42-50`.

O inverso também não é uma trava universal: o humano bloqueia encerramento usando S127 (`fio_vivo`), sem exigir que `fechada_em` esteja preenchida. `app/chamados/models.py:148-171`; `app/chamados/juizes.py:2316-2322`. Resolver o pai dispara a resolução de perguntas em best effort; falha desse trabalho não desfaz o estado do pai. `app/chamados/signals.py:90-117`. O código tem tratamento explícito da combinação disputa aberta/pai terminal no serviço de becos. `app/chamados/services/becos.py:25-33`. O anexo fornecido afirma 7 pais resolvidos e 4 fechados entre as 18, sem confirmação independente. [não verificado]

Não encontrei arquivo inacessível entre os caminhos rastreados; não li banco nem logs. Execução efetiva dos crons, callbacks e código em produção não é verificável nesta cópia. [não verificado]

_3433 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
