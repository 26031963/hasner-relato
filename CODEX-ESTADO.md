# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 17:51** · publicado em **13/11/2026 17:53** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 17:51 · sessao 2860ede427e9 · - · - · origem=Ronald (tmux)

**BLOQUEIA — dinheiro do passivo em competência 10 exportada.** A porta 103 limpa carimbo/validação sem alterar resposta, deixando o admin validar novamente (`app/chamados/services/validacao.py:268-280`). Essa validação agenda recálculo **antes** da materialização (`:544-560`); se a batida for recusada, desfaz o carimbo sem reverter o atomic externo (`:570-577`; devolução `:197-210`). O callback roda no commit e chama o escritor do fechamento (`app/ponto/services/fechamento.py:1807-1824`), que atualiza inclusive campos calculados de aprovado/exportado (`:1145-1180`). A guarda temporária protege somente 09/2026 (`:36-43`, aplicação `:560-575`), portanto não protege 10/2026. Contraexemplo: pergunta 103 da competência 10 exportada, FM gravado divergente da derivação atual; admin valida, plantio recusa por FM exportado (`app/ponto/janelas.py:85-88`; `app/ponto/registro_batida.py:132-137`), mas o callback regrava as horas. Viola o aval raspado “validar não toca o gravado” **[anexo sem arquivo:linha]**. Existência/quantidade desse estado na produção **[não verificado]**.

**NAO-BLOQUEIA — FORA_DE_PIPELINE continua divergente, mas acompanha o problema anterior.** Texto continua remetendo à L-113 (`app/config/crons.py:770-774`), enquanto o aval específico exige preservação do gravado **[anexo sem arquivo:linha]**. Corrigir somente a redação não impede o recálculo acima (`app/chamados/services/validacao.py:553-560`).

**NAO-BLOQUEIA isoladamente — foto de reversão permanece antes da trava.** Comando coleta campos (`app/chamados/management/commands/bug_par_103.py:48-60`), grava JSON (`:77-79`) e depois chama a porta (`:83-85`), cuja trava só ocorre em `app/chamados/services/validacao.py:258-263`. Contraexemplo: A grava snapshot; B devolve a mesma pergunta; A chega à trava e retorna não-caso (`:261-262`). O arquivo A inclui pergunta que A não alterou; usá-lo cegamente desfaria B. Não há aplicador desse JSON demonstrado **[não verificado]**. A trilha efetiva contém antes/depois lidos sob trava (`:263-287`), mitigando reversão manual.

**OK — DRY não escreve pelo comando.** Retorna antes de `makedirs`/`open` (`app/chamados/management/commands/bug_par_103.py:65-79`); o teste correspondente só verifica carimbo preservado/saída DRY (`app/chamados/tests/test_bug_par_projetado.py:544-553`). Execução do teste **[não verificado]**.

**NAO-BLOQUEIA — RED_2c não protege a classificação de competência fechada.** Só injeta COLISAO e exige PARIDADE (`app/chamados/tests/test_bug_par_projetado.py:695-707`). Mutante “qualquer recusa do chokepoint vira PARIDADE” satisfaz essas asserções. O vizinho RED_2b injeta competência fechada, mas cobra apenas rollback/carimbo nulo (`:672-681`), sem cobrar código COMPETENCIA. Execução de mutantes **[não verificado]**.

**RED5/RED7 da triagem:** os testes atuais de modal cobram frases de apresentação (`app/chamados/tests/test_bug_par_projetado.py:766-798`); RED7 executa dois applies e cobra dois arquivos, com conteúdos de tamanhos `[0,1]` (`:838-857`). Scripts/mutações exatas chamados “RED5/RED7” e sua sobrevivência **[não verificado]**; não inferir execução pelo nome dos testes.

**LEI-AKITA do commit:** mensagem/metadados de 4ab30d147 ausentes nesta cópia sem `.git` **[não verificado]**. Não certifiquei presença/veracidade dos campos da linha do commit. A regra exige `origem`, `testemunha`, `RED`, `quem-mais-le`, `juizes novos`, `padrao` (`CLAUDE.md:60-68`); confirmar números de linha desse último trecho pelo parent, pois foram lidos inicialmente sem `nl` **[não verificado]**.

## 13/11 17:51 · sessao e1bf90c104d0 · - · - · origem=Ronald (tmux)

Item 3 — censo de escrita/leitura e efeitos da mensagem nova:

| Campo | Escritores efetivos | Leitores decisórios |
|---|---|---|
| `via_resolucao` | Atribuição central em `app/chamados/models.py:3127`, usada por `carimbar`, `carimbar_identidade`, `descarimbar`; lote em `app/chamados/models.py:3308`; duas portas que só trocam via: `app/chamados/models.py:3317` e `app/chamados/services/relavra_via.py:125`. | C6 lê projeção temporal em `app/chamados/juizes.py:1386` e identidade em `app/chamados/juizes.py:1438`; lastro em `app/chamados/juizes.py:679`; SQL de vivas em `app/chamados/models.py:3398`; estado de aplicação em `app/chamados/services/materializacao.py:692`; universo103 em `app/chamados/services/materializacao.py:276`; flip automático em `app/ponto/services/flip_auto.py:129`. |
| `materializada_em` | Atribuição central `app/chamados/models.py:3125`; lote `app/chamados/models.py:3308`; `BUG-PAR-103` chama descarimbo, sem escrita paralela, em `app/chamados/services/validacao.py:268`. | C6 `app/chamados/juizes.py:1386`, `app/chamados/juizes.py:1438`, `app/chamados/juizes.py:1444`; estado `app/chamados/models.py:3069`; aplicação `app/chamados/services/materializacao.py:688`; fila `app/chamados/services/materializacao.py:1744`; terminalidade COLISAO `app/chamados/services/materializacao.py:1402`; devolução `app/chamados/services/validacao.py:192`; universo103 `app/chamados/services/materializacao.py:276`. |
| `validada_em` | Validação `app/chamados/services/validacao.py:136`; devolução comum `app/chamados/services/validacao.py:201`; devolução103 `app/chamados/services/validacao.py:271`; zeragem na resposta/correção `app/chamados/services/disputa_emissao.py:1480`; zeragem na reabertura `app/chamados/models.py:1508` e lote `app/chamados/models.py:1567`; autovalidação pela porta do carimbo `app/chamados/models.py:3161`; restauração em memória `app/chamados/models.py:3190`. | C6 mata validada em `app/chamados/juizes.py:1401`; estado `app/chamados/models.py:3067`; guarda de já validada `app/chamados/services/validacao.py:498`; exigência da irmã `app/chamados/services/materializacao.py:195`; fila `app/chamados/services/materializacao.py:1743`; projeção histórica `app/chamados/juizes.py:1385`; trilha a partir da projeção `app/chamados/models.py:4021`. |
| `recusa_motivo` | Cinco produtores semânticos: recusa da porta `app/chamados/services/materializacao.py:1411` (ou mesma gravação da identidade via `app/chamados/models.py:3214`); pendência de marco `app/chamados/services/materializacao.py:1125`; recusa humana `app/chamados/services/validacao.py:705`; contestação respondida `app/chamados/services/disputa_emissao.py:1497`; duplicata por porta identidade `app/chamados/services/fio.py:1399`. | Código do motivo decide carimbo terminal em `app/chamados/services/materializacao.py:1400`; universo de paridade/103 em `app/chamados/services/materializacao.py:262`; retorno do passivo em `app/chamados/services/passivo_validado.py:144` e `app/chamados/services/passivo_validado.py:567`; devolução exige presença em `app/chamados/services/validacao.py:194`; regex do flip em `app/ponto/services/flip_auto.py:132`; I18/I19 testam vazio em `app/chamados/management/commands/auditar_invariantes_chamados.py:1075` e `app/chamados/management/commands/auditar_invariantes_chamados.py:1112`. |
| `Batida.retratada_em` | Escrita dinâmica `anotar`: `app/ponto/registro_batida.py:253`; retratação canônica fornece `timezone.now()` em `app/ponto/registro_batida.py:274`; desfazer humano fornece `None` em `app/ponto/services/retratar_humano.py:207`. Chamam retratação: humano `app/ponto/services/retratar_humano.py:179`, geofence `app/chamados/services/acoes_chamado.py:277`, par relâmpago `app/ponto/management/commands/detectar_par_relampago.py:224`, passivos `app/ponto/management/commands/diff_passivo_s84.py:82` e `app/ponto/management/commands/passivo_disputa_retro.py:164`. | Chokepoint filtra retratadas em `app/ponto/registro_batida.py:109` e `app/ponto/registro_batida.py:141`; pareador `app/ponto/turnos.py:1864`; lastro `app/chamados/juizes.py:755`; novo vizinho delega universo em `app/chamados/services/materializacao.py:107`; outros consumidores de apuração usam porta/filtragem em `app/ponto/turnos.py:1902`, `app/ponto/turnos.py:2221`, `app/ponto/turnos.py:2353`, `app/ponto/turnos.py:2384`, `app/ponto/turnos.py:2469`, `app/ponto/turnos.py:2876`, `app/ponto/turnos.py:2915`. |

**OK — carimbo BUG-PAR-103 usa porta e trilha.** A elegibilidade vem do predicado único em `app/chamados/services/materializacao.py:265`; a porta trava e reconsulta em `app/chamados/services/validacao.py:258`; descarimba e zera validação em `app/chamados/services/validacao.py:268`; grava `DecisaoPergunta.DEVOLUCAO` cujo motivo começa com BUG-PAR-103 em `app/chamados/services/validacao.py:276`; grava evento antes/depois em `app/chamados/services/validacao.py:281`; ausência do evento aborta em `app/chamados/services/validacao.py:288`. O leitor do rótulo consulta precisamente esse ato e prefixo em `app/chamados/services/materializacao.py:285`, chamado pela testemunha em `app/chamados/services/materializacao.py:697`.

**BLOQUEIA — texto livre controla terminalidade e agora generaliza COLISAO.** `_carimbar_recusa` trunca em 240 ANTES de traduzir e só carimba terminal se tradução = COLISAO em `app/chamados/services/materializacao.py:1399`. A nova mensagem classifica PARIDADE pela regra `guarda de paridade`/`par quebrado`, antes da regra COLISAO, em `app/chamados/catalogo/recusa.py:34`; isso é o comportamento pretendido para irmã desfeita, mas também transforma colisão ALHEIA persistente em pendente porque o ramo só verifica o código, sem identidade da batida, em `app/chamados/services/materializacao.py:1238`. A devolução então zera validação quando `materializada_em=None`, em `app/chamados/services/validacao.py:192` e `app/chamados/services/validacao.py:201`. O contador passa a contar essa colisão como paridade em `app/chamados/services/materializacao.py:262`. Contraexemplo: registrar a segunda metade recusa por batida alheia que permanece após rollback; qualquer nova validação repete o mesmo ramo e volta ao admin, sem produzir `fato_ocupado` — sequência demonstrável pela composição das linhas citadas; ocorrência em produção `[não verificado]`.

**OK — marca nova não cai numa regra anterior errada.** A única regra anterior à PARIDADE genérica procura `falta a outra metade do intervalo`, também de código PARIDADE, em `app/chamados/catalogo/recusa.py:29`; depois vêm COMPETENCIA em `app/chamados/catalogo/recusa.py:43` e COLISAO em `app/chamados/catalogo/recusa.py:74`. O catálogo usa `any()` e primeira regra correspondente em `app/chamados/catalogo/recusa.py:89`, portanto posição da marca dentro da string não muda prioridade; pô-la no começo apenas protege da truncagem de `app/chamados/services/materializacao.py:1399`.

**NAO-BLOQUEIA isoladamente — censo dos outros leitores textuais.** Tradução para mensagem aparece em `app/chamados/services/fio.py:741`, `app/chamados/services/acoes_disputa.py:34`, `app/chamados/services/acoes_disputa.py:174` e `app/chamados/services/validacao.py:643`; a nova classificação muda a instrução mostrada para “vizinha do MESMO tipo”, embora a causa original seja oposto em menos de 300s, conforme `app/chamados/catalogo/recusa.py:35` e `app/chamados/catalogo/recusa.py:75`. O regex independente do flip exige `batida vizinha #<id> (<tipo>` em `app/ponto/services/flip_auto.py:132`; a nova mensagem não contém essa estrutura (`app/chamados/services/materializacao.py:1240`), mas a própria consulta exige `via_resolucao=VIA_PARIDADE_RECUSADA` em `app/ponto/services/flip_auto.py:129`, via que o ramo novo não grava porque PARIDADE não carimba em `app/chamados/services/materializacao.py:1402`. Portanto o texto novo não cria um flip automático por esse leitor.

**OK quanto a escritor novo do delta — portas já têm tripwires.** O conjunto permitido para carimbo e via está explícito em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:44` e `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`, e o selo compara escritores em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:226` e `app/chamados/tests/test_o261e_f2_porta_carimbo.py:238`. `validada_em` tem dono e exceções por módulo em `app/chamados/tests/test_contract_dono_validada.py:27` e `app/chamados/tests/test_contract_dono_validada.py:30`; a descrição “allowlist vazia” está desatualizada diante das três entradas nesse mesmo arquivo (`app/chamados/tests/test_contract_dono_validada.py:14`, `app/chamados/tests/test_contract_dono_validada.py:34`). Nenhum teste foi executado; verde da suíte `[não verificado]`.

Este censo cobre os produtores semânticos e leitores decisórios dos campos no fluxo BUG-PAR; completude de todos os leitores indiretos/dinâmicos e comandos de toda a árvore `[não verificado]`.

## 13/11 17:50 · sessao 4725a19810ec · - · - · origem=Ronald (tmux)

**Item 2 — OK na troca de universo.** Os cinco usos julgam efeito de plantio, não exibem marcação:

| Chamador | Pergunta respondida |
|---|---|
| `app/chamados/services/materializacao.py:215` | A metade sozinha exige par? |
| `app/chamados/services/materializacao.py:228` | Cada metade aceita a irmã projetada? |
| `app/chamados/services/materializacao.py:712` | A tela pode anunciar plantio aplicável? |
| `app/chamados/services/materializacao.py:1150` | O plantio é permitido pela paridade? |
| `app/chamados/services/materializacao.py:1161` | Após gravar a irmã, a paridade continua válida? |

A lei distingue apuração de exibição crua (`app/docs/LEIS.md:76`); a porta exclui somente retratadas, preservando pendentes de DP (`app/ponto/turnos.py:2856`, `app/ponto/turnos.py:2876`). A troca usa essa porta nos dois lados, sem remontar universo manual (`app/chamados/services/materializacao.py:110`, `app/chamados/services/materializacao.py:113`).

**Contraprova ao possível falso positivo do cru:** `_batida_existente_na_janela` permanece deliberadamente crua (`app/chamados/juizes.py:243`). O contrato explica que sua pergunta é impedir replantio da marca já retratada; ignorá-la faria ciclo cluster → retratação → replantio (`app/chamados/juizes.py:806`). Isso não justifica cru na guarda de vizinhança, que responde outra pergunta (`app/chamados/services/materializacao.py:94`).

**`passivo_validado.py:497` — OK, sem divergência introduzida.** O trecho descreve uma guarda adicional de geometria do dia inteiro; mede antes, chama o mesmo materializador e reprova se aumentam turnos abertos/saídas órfãs (`app/chamados/services/passivo_validado.py:527`, `app/chamados/services/passivo_validado.py:537`, `app/chamados/services/passivo_validado.py:557`). A contagem consulta o pareador, sem construir batidas em paralelo (`app/chamados/services/materializacao.py:453`). Contraexemplo da própria especificação: plantar avulsa pode manter alternância local e transformar `E* S` em `E* S E`; essa guarda adicional continua necessária (`app/chamados/services/passivo_validado.py:492`).

**RED_2d — OK por inspeção: exige as duas curas.** Se somente anterior migrar, a primeira consulta ainda encontrará `E 03:00` retratada no posterior, escondendo `S 06:30`, e falhará em `assertIsNotNone` (`app/chamados/tests/test_bug_par_projetado.py:715`, `app/chamados/tests/test_bug_par_projetado.py:717`). Se somente posterior migrar, o segundo cenário deixa `S 00:30` retratada esconder a `E 18:30` anterior, falhando na segunda consulta (`app/chamados/tests/test_bug_par_projetado.py:721`, `app/chamados/tests/test_bug_par_projetado.py:723`). Execução efetiva/mutante: **[não verificado]**.

**Classes de regressão — nenhum segundo escritor de Batida nesse caminho identificado.** A irmã passa pelo mesmo `_materializar_uma`, com projeção temporária protegida por `finally` (`app/chamados/services/materializacao.py:1440`); o plantio efetivo chama `registrar_batida` (`app/chamados/services/materializacao.py:1219`). `par_do_intervalo` concentra os mesmos critérios para tela, validação e escrita, com ambos os lados projetados (`app/chamados/services/materializacao.py:178`, `app/chamados/services/materializacao.py:227`).

**Exceções — risco existente, não introduzido pelo delta apresentado.** `_materializar_uma` transforma qualquer `Exception` em texto de recusa; o `finally` o envia ao classificador textual (`app/chamados/services/materializacao.py:1636`, `app/chamados/services/materializacao.py:1648`). Já a segunda opinião posicional, em erro, registra warning e retorna falso: restringe a autorização, sem aprovar plantio no escuro (`app/chamados/services/materializacao.py:421`). Um erro concreto engolido em produção é **[não verificado]**.

## 13/11 17:50 · sessao 206e3fa43e2b · - · - · origem=Ronald (tmux)

Censo e achados enviados; principais pontos para consolidação:

- **Cura 2 fecha o julgamento da edição:** único caller produtivo encontrado é `app/ferias/views.py:846`; julgamento usa linha relida e travada em `app/ferias/services.py:731-742`. Admin de férias registra somente Afastamento (`app/ferias/admin.py:2-5`).

- **Escritor público de direito ainda sem trava:** `aplicar_art130(pa, salvar=True)` salva diretamente em `app/ferias/services.py:350-357`, exposto por `app/ferias/models.py:160-164`. O caller produtivo encontrado usa `salvar=False` sob trava (`app/ferias/services.py:327-335`); corrida por endpoint atual **[nao verificado]**.

- **Direito editado pode ser imediatamente sobrescrito:** editar para 35 com consumo 30 gera status adquirido (`app/ferias/services.py:739-742`); resposta HTMX chama contexto (`app/ferias/views.py:854-855`), que sincroniza períodos (`app/ferias/views.py:795-802`); sincronização recalcula direito pelo art.130 (`app/ferias/services.py:333-344`). Sem faltas, volta a 30 pelo juiz (`app/ferias/models.py:54-55`). Contraprova estática; execução **[nao verificado]**.

- **Concorrência entre PAs diferentes não é serializada por colaborador:** cada criação trava apenas seu PA (`app/ferias/services.py:492-503`). As duas requisições podem passar pelo veto antes dos inserts (`app/ferias/models.py:423-442`). A constraint da Ausencia não necessariamente desfaz o Agendamento: INSERT tem savepoint (`app/ponto/services/ausencia.py:417-418`), duplicata vira AusenciaInvalida (`app/ponto/services/ausencia.py:430-435`), e espelho captura por dia e continua (`app/ponto/services/ausencia.py:1210-1218`). Interleaving estático possível; reprodução runtime **[nao verificado]**.

- **Ordem usual:** AG→PA (`app/ferias/services.py:560-564`), criação apenas PA (`app/ferias/services.py:493`), Ausencia depois. Exclusão inverte para PA→Ausencia→AG (`app/ferias/services.py:756-762`, `app/ferias/models.py:456-460`), mas os caminhos para cancelados recusam antes de pedir PA (`app/ferias/services.py:561-562`); deadlock desse caso **[nao verificado]**.

- **Fontes da folha mudam sem consultar exportada:** troca apaga Ausencias antigas e grava datas novas (`app/ferias/services.py:629-651`), histórico cria e materializa (`app/ferias/services.py:443-477`). A porta declara que período fechado não recusa férias (`app/ponto/services/ausencia.py:1169-1170`). Folha lê cobertura aprovada (`app/ponto/turnos.py:2516-2517`, `app/ponto/services/fechamento.py:683-687`); export lê datas inclusivas de Agendamento (`app/folha/export.py:491-493`) e altera retenção de quem teve batida (`app/folha/export.py:876-884`). Alteração de TXT já emitido ou de FechamentoMensal gravado **[nao verificado]**; a proteção deste último é distinta (`CLAUDE.md:411`).

- **Leitor com status errado:** KPI/lista próximos 30 dias filtra `aprovado` (`app/inteligencia/views.py:94`, `app/inteligencia/views.py:278`, `app/inteligencia/views.py:286`), enquanto escolhas de Agendamento são agendado/em_gozo/concluido/cancelado (`app/ferias/models.py:296-300`). Agendamento normal fica invisível nesse leitor.

- **Trilha fora do commit de correção:** correção de datas executa porta e só depois registra_log (`app/ferias/views.py:525`, `app/ferias/views.py:536`); atomic da porta termina antes (`app/ferias/services.py:649-652`). Falha posterior de trilha deixa mudança aplicada; `_trilha` obrigatório aparece somente no realinhamento/reversão (`app/ferias/services.py:161`, `app/ferias/services.py:182`).

Censo Ausencia ferias: cria aprovada por dia (`app/ponto/services/ausencia.py:1203-1208`), apaga no espelho (`app/ponto/services/ausencia.py:1190-1192`, `app/ponto/services/ausencia.py:1212-1215`), apaga janela (`app/ponto/services/ausencia.py:1263-1271`), rejeita admin (`app/ponto/services/ausencia.py:1282-1284`). Leitores diretos adicionais: guarda de sobreposição (`app/ferias/models.py:433-436`), lista com batida (`app/ferias/management/commands/listar_ferias_com_batida.py:40-64`), vigia de ausência (`app/ponto/services/vigia_ausencia.py:214-220`). Cobertura comum inclusiva e efeitos por destinatário: `app/ponto/turnos.py:2511-2535`, `app/ponto/turnos.py:2735-2782`.

## 13/11 17:50 · sessao aee2833cbbc9 · - · - · origem=Ronald (tmux)

1. **Cura 1: fecha a falha original de write/flush/fsync, mas a reversão continua incompleta.** O registro é escrito e sincronizado dentro do atomic externo antes da saída que confirma o período; erro desfaz o banco (`app/ferias/management/commands/realinhar_concedidos_ferias.py:123`, `:127`, `:129`, `:130`). Os testes mordem as quatro falhas de escrita e preservação do primeiro período (`app/ferias/test_hj090_volta3.py:98`, `:111`). Execução desses testes: [nao verificado].

2. **BLOQUEIA — intenção não aplicada pode reverter um apply posterior.** Uma falha em fsync depois do flush pode deixar linha completa, com rollback do período; o teste reconhece essa possibilidade (`app/ferias/management/commands/realinhar_concedidos_ferias.py:129`; `app/ferias/test_hj090_volta3.py:107`). Contraexemplo: apply A falha; apply B, posterior, realinha o mesmo período e mesmos agendamentos; `--reverter` do arquivo A passa porque a guarda só compara concedidos e `[pk,dias]`, desfazendo B (`app/ferias/services.py:176`, `:179`). Também ocorre com apply A → reverter A → apply B → repetir reverter A. Não há identidade do ato nem conferência da trilha nessa guarda (`app/ferias/services.py:176`). Os testes existentes só alteram concedidos ou trocam o PK dos agendamentos (`app/ferias/test_hj090_um_juiz.py:149`, `:212`).

3. **BLOQUEIA — remarcação com os mesmos dias não invalida a reversão.** O contrato promete recusar período cujo agendamento foi remarcado (`app/ferias/services.py:170`), mas a identidade contém somente PK e dias (`app/ferias/services.py:134`, `:137`). Contraexemplo: direito 30, concedidos 10, agendamento de 20; realinha para 20; remarca o mesmo agendamento para outra janela mantendo 20 dias; a remarcação preserva concedidos e `[pk,dias]` (`app/ferias/services.py:635`, `:636`); reverter o registro anterior passa e grava 10 (`app/ferias/services.py:176`, `:179`). O teste troca o agendamento por outro PK e não cobre esse caso (`app/ferias/test_hj090_um_juiz.py:212`).

4. **NAO-BLOQUEIA/limitação de integridade — parser valida sintaxe, não estrutura.** Cauda sem newline só pode ser a última linha; JSON sintaticamente inválido em linha completa provoca recusa antes de qualquer reversão (`app/ferias/management/commands/realinhar_concedidos_ferias.py:43`, `:49`, `:51`, `:81`). Portanto a hipótese de ignorar JSON inválido no meio **não procede nesse sentido**. Contudo `{}` com newline no meio é JSON válido, entra no registro e causa `KeyError` apenas no processamento; períodos anteriores já podem ter sido revertidos (`app/ferias/management/commands/realinhar_concedidos_ferias.py:49`, `:82`, `:83`). O teste cobre somente `{invalido}\n` (`app/ferias/test_hj090_volta3.py:150`, `:155`).

5. **Rollback ancestral: possível na API, caller produtivo adicional não demonstrado.** O command usa atomic comum, sem impedir aninhamento; seu arquivo permanece se um atomic ancestral posteriormente desfizer o banco (`app/ferias/management/commands/realinhar_concedidos_ferias.py:123`, `:130`). A documentação o chama expressamente de intenção sem confirmação (`:13`). O teste herda de `TestCase`, logo não prova commit efetivo por período (`app/ferias/test_hj090_volta3.py:81`; `app/ferias/test_hj090_um_juiz.py:63`). Caller produtivo que envolva esse command em outro atomic: [nao verificado]. A porta pública escreve sem arquivo, mas grava trilha obrigatória; único caller produtivo localizado é o command (`app/ferias/services.py:150`, `:161`, `:164`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:124`).

6. **Mordida dos testes:** curas de direito, trava, foto velha, vendidos, contador nos dois sentidos e exceção específica possuem casos que atingem seus mutantes naturais (`app/ferias/test_hj090_volta3.py:166`, `:173`, `:181`, `:206`, `:234`, `:241`). Não há mordida para retirar fsync dos diretórios: a injeção só falha no descritor do arquivo (`app/ferias/test_hj090_volta3.py:71`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:116`). A cura da mensageria não é exercitada por volta3; retirar o repasse não muda seus asserts (`mensageria/nucleo/ferramentas.py:2114`; `app/ferias/test_hj090_volta3.py:206`, `:245`).

7. **Costura do placar: não identifiquei perda de prova anterior.** A costura acrescenta somente a nova derivação externa; mantém command, gravação real, argumentos do contador e asserts anteriores (`app/core/tests/test_fio_mudo_placar.py:116`, `:129`, `:131`, `:135`, `:136`). A integração de férias tem teste separado com contador real envolvido por `wraps`, dados divergentes 1×2 e conferência do carimbo (`app/ferias/test_hj090_volta3.py:206`, `:215`, `:219`, `:224`).

8. **Possível confusão entre tenants — evidência de desenho, ocorrência não verificada.** O registro não contém schema, apenas período/antes/depois; reversão procura o PK no tenant corrente (`app/ferias/services.py:164`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:83`). Se dois tenants tiverem PKs e `[pk,dias]` iguais, arquivo do primeiro pode alterar o segundo (`app/ferias/services.py:176`, `:179`). Invocação equivocada em produção: [nao verificado].

## 13/11 17:50 · sessao 7f1417515870 · - · - · origem=Ronald (tmux)

Achados e evidências do recorte:

1. **OK — triagem2 #3, contador no carimbo.** O produtor usa `ferias_concedidos_x_soma`, grava ambos sentidos e declara esperado zero; o leitor compara todas as chaves de `esperado` e publica divergências sem limiar próprio. Contraexemplo removido: soma>concedidos agora aparece no ESTADO. `app/chamados/management/commands/vigiar_fio_morto.py:98-99,227-246`; `bin/fio_estado.py:121-126,191-194`; `bin/gerar_estado.py:260-261`.

2. **OK — triagem2 #4, mensageria.** `saldo_negativo_dias` passa do resultado do juiz para API e ferramenta, preservando inclusive o número negativo. `app/ferias/situacao.py:297-302`; `app/api/views_mensageria.py:835-836`; `mensageria/nucleo/ferramentas.py:2113-2115`. O novo teste `CarimboFerias` cobre contadores distintos `(1,2)`, esperado zero e chamada ao contador real; morde omissão, zero fixo e troca de sentidos. `app/ferias/test_hj090_volta3.py:206-229`. Execução dos testes [nao verificado].

3. **NAO-BLOQUEIA — relógio ainda escapa no vigia, preexistente.** O comentário promete uma leitura; `handle` captura `agora`, mas não o transmite a `censo_alcance`. Este chama `alimentar_portas` sem `agora`, que chega como `None` à fila e provoca novo `timezone.now()`. Contraexemplo: instante capturado 23:59:59, chamado silenciado até 00:00:00; consulta posterior já o considera porta, carimbo assina instante/dia anterior. `app/chamados/management/commands/vigiar_fio_morto.py:162-176,232-233`; `app/chamados/services/vigia_fio.py:604`; `app/chamados/juizes.py:1576,1531-1543`; `app/chamados/services/painel.py:190`. O contador novo de férias não usa relógio. `app/ferias/services.py:116-127`.

4. **NAO-BLOQUEIA — dois relógios no command novo.** `listar_ferias_com_batida` lê `localdate()` e `now()` separadamente. Contraexemplo: virada da meia-noite entre chamadas entrega `hoje` do dia anterior e `agora` do seguinte ao carregador e à ordenação de competências. `app/ferias/management/commands/listar_ferias_com_batida.py:36,52,61`.

5. **NAO-BLOQUEIA — motivo incompleto em FORA_DE_PIPELINE.** `listar_ferias_dp` é descrito como “só leitura, sem efeito em dado”, porém `--simular` cancela via porta e escreve dentro de transação desfeita. A própria recusa da flag admite a escrita; só aceita `DATABASES.default.NAME == 'sombra'`. Persistência dessas alterações não demonstrada: há rollback explícito e tratamento depois do atomic. `app/config/crons.py:774-776`; `app/ferias/management/commands/listar_ferias_dp.py:52-53,98-115`. A guarda não confere `HOST`, embora a configuração da sombra declare nome e host como requisitos de isolamento. `app/config/settings/sombra.py:29-50`.

   Os motivos dos outros dois são compatíveis: lista exige `--desde`, consulta ausências/batidas e imprime; realinhamento sai antes da escrita no DRY e exige autor para apply/reverter. `app/config/crons.py:771-779`; `app/ferias/management/commands/listar_ferias_com_batida.py:24-25,40-69`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:65-85,95-109`. Não há fundamento no desenho 4a para transformar reparo pontual em rotina; o desenho determina vigia de varredura, sem reparo. `CLAUDE.md:169-173`.

6. **OK — costura de fio mudo preserva a prova anterior identificável.** A costura nova dubla somente a derivação de férias; o contador de fio mudo continua com chamada e valores afirmados, incluindo esperado diferente de zero, dia civil local e alarme no placar. `app/core/tests/test_fio_mudo_placar.py:129-139,230-247`. O placar continua consumindo exclusivamente as chaves fio mudo e nunca bateu; essa costura não prova alarme de férias no placar, nem o código atual oferece esse alarme. `app/core/management/commands/pipeline_placar.py:85-107,135-136`. Antes/depois da execução da suite [nao verificado].

7. **NAO-BLOQUEIA — leitores antigos ainda divergem do significado declarado de férias vigentes.** A autoridade inclui qualquer agendamento não cancelado coberto pelas datas, explicitamente inclusive concluído com data viva; `situacao_de_ferias` e `alertar_retornos` filtram só agendado/em_gozo. Contraexemplo: concluído com datas no presente é férias para o juiz e folha, mas some da lista da API e do aviso de retorno. `app/ferias/services.py:49-64`; `app/ferias/situacao.py:165-169,303`; `app/api/views_mensageria.py:843-846`; `app/ferias/management/commands/alertar_retornos.py:20-22`.

8. **NAO-BLOQUEIA — derivação paralela de vencimento, preexistente.** `periodos_vencendo` exige `status='adquirido'` persistido; o juiz `vencendo` pergunta `status_de_periodo` ao vivo. Contraexemplo: aquisição completou e cron ainda não atualizou em_curso: modelo diz vencendo, service omite. `app/ferias/services.py:201-209`; `app/ferias/models.py:255-260`. Janelas de 60 dias também continuam separadas em `services`, `situacao` e `pautas_de_ferias`, apesar de existir `DIAS_VENCENDO` no modelo. `app/ferias/services.py:45`; `app/ferias/situacao.py:55,206-209`; `app/ferias/management/commands/pautas_de_ferias.py:68-74`; `app/ferias/models.py:249,255`.

9. **NAO-BLOQUEIA — command escreve sem --apply, já listado na triagem2.** `alertar_ferias_vencendo` chama sincronização que pode gravar status e direito; não declara argumentos de apply e está no cron sem flag. `app/ferias/management/commands/alertar_ferias_vencendo.py:14-32`; `app/ferias/models.py:152-158`; `app/config/crons.py:708`. A mesma rotina também envia push e relata falha individual sem apagar o erro da saída. `app/ferias/management/commands/alertar_ferias_vencendo.py:84-88`.

## 13/11 17:46 · sessao a92e0d0adb4b · E3-FORMA-SQL · leitura · origem=Code

**Resultado: 0 regressões demonstradas pelos casos E3 escritos; os 79 ficam em “sem caso escrito para a combinação observada”.** Isso deixa a decisão normativa desses 79 em aberto; a perda de porta está demonstrada, mas sua classificação como violação não. (`v5_det.txt:1–79`; `app/chamados/tests/test_e3_portas_tarefa.py:21–127`)

Referências abreviadas:

- **V:n** = `v5_det.txt:n`, contando **somente as linhas `V5_DET`** fornecidas.
- **T:n** = [app/chamados/tests/test_e3_portas_tarefa.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/tests/test_e3_portas_tarefa.py):n.
- **S:n** = [app/chamados/services/tarefa_do_dia.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py):n.
- **J:n** = [app/chamados/juizes.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py):n.
- **F:n** = [app/chamados/tests/e3_fixtures.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/tests/e3_fixtures.py):n.

**1–2. Classes, contagens, exemplos e cobertura.**

A classificação abaixo agrupa multiplicidade e ordem das razões; preserva seus conjuntos, módulo/status e diferenças de âncora e ações. `P=passivo_sem_linha`, `A=aceso`, `R=aguarda_resposta`, `W=dia_sem_trabalho`, `V=veredito_sem_lavra_vigente`, `J=encerrada_pelo_juiz`. (`V:1–79`)

Na coluna âncora: **C** = presente, `fila_ids=true/fila_cel=true`; **G** = presente, `true/false`; **X** = presente, `false/false`; **—** = nenhuma pergunta/âncora exposta nas instâncias. Isso não comprova inexistência de perguntas suprimidas pela tarefa. (`V:1–79`; `S:262–271`)

Módulos: **TA**=`turno_aberto`, **BA**=`batida_ausente`, **DS**=`disputa_supervisao`, **AC**=`ausencia_colab`, **PR**=`par_relampago`, **TA24**=`turno_aberto_24h`; status **a**=`aberto`, **e**=`em_analise`. A coluna descreve os chamados relacionados no V5; módulo/status de âncoras não relacionadas ali permanecem **[não verificado]**. (`V:1–79`)

**A coluna de pks é exaustiva; seus primeiros dois elementos são os exemplos. Classes unitárias só têm um exemplo disponível.** (`V:1–79`)

| Classe | Rota ATUAL | Módulo/status | Razões; âncora; ações | n | celula_pk — lista completa e evidência | Cobertura |
|---|---|---|---|---:|---|---|
| C01 | fila | TA/e | A,R; X; responder/colaborador/null | 1 | 141714 (V:1) | SC-R |
| C02 | fila | TA/e | A,P; C; vazias | 2 | 130162 (V:2), 135778 (V:17) | SC-P |
| C03 | fila | BA/e | A,P,R; R em X, P em X e C; responder/colaborador/null | 1 | 118931 (V:3) | SC-RP |
| C04 | fila | BA/a | A,P; C; vazias | 6 | 140522 (V:4), 140524 (V:5), 140707 (V:7), 119517 (V:10), 122073 (V:65), 128516 (V:69) | SC-P |
| C05 | fila | BA/a | P; C; vazias | 12 | 140928 (V:8), 104784 (V:9), 129384 (V:12), 104040 (V:13), 116436 (V:23), 106963 (V:33), 124364 (V:43), 124370 (V:44), 123033 (V:46), 132144 (V:61), 106350 (V:64), 124774 (V:73) | SC-P |
| C06 | fila | BA/e + DS/a | A,P; C e G, incluindo linha sintética; vazias | 1 | 139432 (V:6) | SC-P |
| C07 | fila | TA/e | A; —; vazias | 9 | 140331 (V:11), 135067 (V:18), 110506 (V:25), 138672 (V:26), 138912 (V:39), 138914 (V:40), 138916 (V:41), 140580 (V:42), 106594 (V:63) | SC-A |
| C08 | fila | BA/e | A,P; C; vazias | 14 | 133364 (V:14), 111346 (V:15), 111845 (V:16), 116432 (V:22), 129745 (V:28), 129749 (V:29), 129751 (V:30), 137166 (V:31), 33784 (V:47), 128519 (V:70), 141797 (V:74), 143313 (V:77), 143393 (V:78), 143395 (V:79) | SC-P |
| C09 | fila | AC/a | A; —; vazias | 2 | 129083 (V:19), 129090 (V:20) | SC-A |
| C10 | fila | AC/e | A; —; vazias | 1 | 129093 (V:21) | SC-A |
| C11 | fila | TA/a | A; —; vazias | 1 | 137154 (V:24) | SC-A |
| C12 | pergunta | DS/e | A,P; G; vazias | 1 | 113307 (V:27) | SC-P |
| C13 | fila | TA/a | A,R; X; responder/colaborador/null | 1 | 129598 (V:32) | SC-R |
| C14 | fila | BA/a | A; —; vazias | 9 | 130931 (V:34), 130933 (V:35), 130935 (V:36), 130939 (V:38), 129567 (V:54), 129569 (V:55), 129573 (V:57), 129577 (V:58), 130781 (V:60) | SC-A |
| C15 | fila | BA/e | A; —; vazias | 4 | 130937 (V:37), 129559 (V:50), 129561 (V:51), 130026 (V:66) | SC-A |
| C16 | fila | TA/e + DS/a | A; —; vazias | 1 | 129217 (V:45) | SC-A |
| C17 | pergunta | DS/a | P,W; G; vazias | 2 | 130346 (V:48), 130348 (V:49) | SC-PW |
| C18 | fila | BA/e + PR/a | A; —; vazias | 1 | 129563 (V:52) | SC-A |
| C19 | fila | BA/a | A,P; C, em linhas sintéticas; vazias | 1 | 129565 (V:53) | SC-P |
| C20 | fila | BA/e + DS/a | A; —; vazias | 2 | 129571 (V:56), 130044 (V:67) | SC-A |
| C21 | fila | BA/e | A,V; C; vazias | 1 | 130771 (V:59) | SC-V |
| C22 | fila | BA/a + TA24/a | A,P; C; vazias | 1 | 132052 (V:62) | SC-P |
| C23 | fila | BA/e | P; C e G; vazias | 1 | 120325 (V:68) | SC-P |
| C24 | pergunta | DS/a | P; G; vazias | 2 | 128533 (V:71), 127477 (V:72) | SC-P |
| C25 | fila | BA/e | A,R; X; responder/colaborador/null | 1 | 141810 (V:75) | SC-R |
| C26 | fila | TA/e + BA/e | A,J; C; vazias | 1 | 142013 (V:76) | SC-J |
| **Total** | | | | **79** | **V:1–79** | |

Os códigos de cobertura significam:

| Cobertura | Caso escrito confrontado | O fato que falta para aplicar o caso | Decisão normativa pelos testes E3 | [nao verificado]
|---|---|---|---|
| SC-A | `test_rota_so_fila_sem_pergunta`, T:32–38 | O teste monta marco **apagado** e exige `perguntar/sistema/emissor`; aqui todas as instâncias expostas estão acesas, sem ações. | **Sem caso escrito** para estes fatos; ATUAL/FUTURO indeterminado. | [nao verificado]
| SC-P | `test_porta_fila_quando_ancora_e_da_celula`, T:67–73; caso A/B, T:21–47 | Falta um caso que imponha porta **apesar de passivo sem linha e ações vazias**. A fixture do teste cria pergunta sem resposta/validação; não constrói passivo. (`F:65–78`) | **Sem caso escrito** para esta combinação; ATUAL/FUTURO indeterminado. | [nao verificado]
| SC-R | `test_fila_A_pergunta_sob_B`, T:21–47 | O caso exige B como âncora viva e ação com porta B; no V5 a âncora está fora de `fila_ids`, com porta da ação `null`. | **Sem caso escrito** para B fora da fila; ATUAL/FUTURO indeterminado. |
| SC-RP | Mesmos casos, T:21–47,67–73 | Faltam simultaneamente o caso de B fora da fila e o de passivo com ações vazias no mesmo dia. | **Sem caso escrito** para a combinação. |
| SC-PW | Caso A/B, T:21–47; âncora da célula, T:67–73 | Falta cenário de dia sem trabalho com passivo, âncora global e nenhuma ação; também não há A residual na fila da célula. | **Sem caso escrito** para a combinação. |
| SC-V | T:67–73 | Falta cenário de validação sem lavra vigente que determine porta mesmo sem ação. | **Sem caso escrito** para a combinação. |
| SC-J | T:67–73 | Falta cenário de encerramento pelo juiz que determine porta mesmo sem ação. | **Sem caso escrito** para a combinação. |

O caso de isenção não se aplica aos fatos expostos: ele exige `razao='isento_ponto'`, ausente nos 79. Os testes de relógio também exigem valores de silêncio e instantes específicos não fornecidos para essas âncoras. (`T:57–65,84–111`; `V:1–79`) [nao verificado]

**3. REGRESSÃO: lista completa demonstrável = `[]`; quantidade = 0.** Nenhuma classe reproduz integralmente um caso E3 que imponha porta. Aplicar T:67–73 apenas pela presença da âncora descartaria o passivo e as decisões que distinguem os fatos. (`T:21–127`; `F:65–78`; `V:1–79`)

O caminho concreto que elimina a porta está identificado, mesmo sem obrigação normativa demonstrada:

| Classes | Onde a cópia deixa de publicar porta |
|---|---|
| C07, C09–C11, C14–C16, C18, C20 | A linha nasce com ações vazias; marco aceso recebe `aceso` e encerra sua passagem pelo laço. (`S:258–271`) |
| C02, C04–C06, C08, C12, C17, C19, C22–C24 | Passivo devolve `acoes=None`; a tarefa mantém a lista vazia e marca `nao_sei`. Exposição administrativa segue a mesma recusa de inventar ação. (`S:130–131,249–255,308–313`) |
| C01, C13, C25 | A âncora só vira porta se estiver na fila; fora dela, a ação `responder` recebe `porta=None`. (`S:128–133`) |
| C03 | Combina os dois caminhos anteriores: resposta sem porta e passivo sem ação. (`S:128–133,308–313`; `V:3`) |
| C21 | Sem lavra vigente, retorna razão e `acoes=None`. (`S:143–146,308–313`) |
| C26 | Encerramento pelo juiz retorna ações vazias. (`S:149–151,313`) |
| Todas | `_portas_da_tarefa` aceita somente portas inteiras pertencentes a `fila_ids`; sem portas, escreve `rota=None`; `porta_da_tarefa` devolve `None`. (`S:547–565`) |

Há contratos E2 que corroboram essas distinções: passivo exige ações vazias; marco aceso dispensa ação; âncora silenciada admite `responder` com `porta=None`. O motivo concreto de exclusão das quatro âncoras V5 da fila permanece **[não verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:396–409,487–495,867–873`; `V:1,3,32,75`)

**4. Restantes: 79 sem caso escrito completo; 0 demonstrados como “caso E3 que manda FUTURO”.** A lista completa dos 79 é a coluna de pks das classes C01–C26, sem exclusões; a lista do segundo conjunto é `[]`. (`V:1–79`; `T:21–127`)

A conferência factual fecha em **30 somente acesos; 41 com passivo e eventual aceso; 2 com passivo e dia sem trabalho; 3 com aceso e resposta; 1 com aceso, passivo e resposta; 1 sem lavra vigente; 1 encerrado pelo juiz = 79**. Há **75 dias com ações vazias e 4 com ações não vazias**. (`V:1–79`)

**5. REGRESSÃO demonstrada com dono ESTRUTURA: 0; lista `[]`.** Isso decorre do conjunto de regressões demonstradas estar vazio, sem necessidade de redistribuir os donos registrados na volta 4. (`V:1–79`; `T:21–127`)

O agregado **ESTRUTURA 44 / CADASTRO 6 / BATIDA 29** não fornece o pareamento individual dos 79 com seus donos. Uma lista individual dos 44 ESTRUTURA, derivada apenas desse agregado, seria **[não verificado]**. (`a1v4_julga.r:16`, contando as linhas textuais do anexo fornecido)

**6. Quem escreve, quem lê e diferenças de significado.**

| Campo | Quem escreve/calcula | Quem lê |
|---|---|---|
| `ancora` exibida no V5 | A tarefa **não escreve esse campo nas instâncias**; escreve `pergunta_id` e `acoes`. Calcula localmente `anc=chamado_que_lista(p)`. A implementação do enriquecimento da sonda é **[não verificado]**. (`S:128,258–260,318–320`) | A tarefa resolve a âncora pela pergunta, sem ler `instancia['ancora']`. (`S:127–129`) |
| Identidade da âncora | `chamado_que_lista` retorna `chamado_destino_id`, com fallback para `disputa.chamado_id`. (`app/chamados/services/fio.py:97–117`) | A carga consulta a fila dessas âncoras; `_pela_decisao` usa sua pertença para preencher a porta. (`S:415–418,128–133`) |
| `acoes[].acao/de/porta` | `_acao` monta os campos; `_pela_decisao` escolhe ações; `_tarefa_pura` as publica nas instâncias. (`S:114–162,249–255,302–313`) | `_portas_da_tarefa` lê porta e dono; o backfill lê razões e ações, inclusive humanos sem porta. (`S:547–556`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:97–117`) |
| `fila_ids` / `fila_cel` | `alimentar_fila` constrói a fila global com verbo e o agrupamento por célula. (`J:1558–1567`) | C17 admite chamado da célula e âncora global; tarefa usa a fila global para autorizar porta e a fila da célula para escolher rota/subconjunto `fila`. (`J:1677–1687`; `S:548–555`) |
| Verbo do ATUAL | `particionar_verbos` calcula o mapa por módulo, créditos das perguntas e silêncio; `alimentar_fila` executa a partição sobre `qs_vivos()`. (`app/chamados/services/verbos.py:30–52`; `J:1558`) | C17 copia o mapa; a tarefa copia o mesmo mapa para as portas selecionadas pelas ações. (`J:1678,1682`; `S:549`) |
| Porta publicada | `_portas_da_tarefa` escreve rota/fila/verbos/fio; `porta_da_tarefa` projeta esses campos ou `None`. (`S:547–565`) | Folha, pauta, vigia, beco e furo sem canal consomem essa projeção. (`app/folha/fora_da_folha.py:71–89`; `app/ponto/services/pautas_dos_discordantes.py:79–84`; `app/chamados/services/vigia_fio.py:640–654`; `app/escala/services/cadastro_realidade.py:492–503`; `app/chamados/services/furo_sem_canal.py:93–101`) |

As diferenças semânticas verificáveis são:

- **Chamado do dia versus chamado da pergunta:** a folha publica identificadores somente de `porta['fila']`, isto é, chamados da célula; as demais chaves de `verbos` podem incluir âncoras externas. Isso está explicitado no leitor. (`app/folha/fora_da_folha.py:235–242`; `S:549–551`)
- **Fila global versus fila da célula:** uma âncora G pode autorizar `porta_pergunta`; não precisa pertencer a `fila_cel`. Esses conjuntos têm funções distintas no código. (`S:548–555`; `J:1677–1687`)
- **Verbo versus dono:** C17 calcula fio pelos verbos; a tarefa calcula fio pelos donos das ações. O teste admite expressamente `responder` junto de verbo `validar`; o vigia preserva essa distinção. (`J:1633–1639,1688`; `S:552–556`; `T:75–82`; `app/chamados/services/vigia_fio.py:649–653`)
- **Ausência de porta versus ninguém esperando:** a documentação da pauta diz “não estão esperando NINGUÉM”, mas seu predicado efetivo lê ausência de porta. A tarefa pode ter ação humana sem porta ou `nao_sei`; a folha já descreve essa limitação. (`app/ponto/services/pautas_dos_discordantes.py:47–56,79–84`; `S:129–133,544,562–565`; `app/folha/fora_da_folha.py:52–58`)
- **Rótulo do instrumento versus caso escrito:** `casos_do_par` chama de `so_fila_sem_pergunta` qualquer `fila and not vivas`; não exige marco apagado nem ação `perguntar`. Esse rótulo, sozinho, não comprova cobertura por T:32–38. (`app/chamados/management/commands/diff_e3_sombra.py:63–70`; `T:32–38`)

## 13/11 17:45 · sessao a0cfb94c0d12 · - · - · origem=Ronald (tmux)

Contagem conferida: **79 colab-dias, 75 com ações vazias e 4 com ações não vazias**. Os quatro são `141714`, `118931`, `129598`, `141810`. (`v5_det.txt:1,3,32,75`, contando somente linhas `V5_DET`; demais linhas V5:1–79)

A tabela preserva módulo/status e separa misturas e linhas sintéticas. Ignora somente multiplicidade e ordem das razões. Abreviações: `A=aceso`, `P=passivo_sem_linha`, `R=aguarda_resposta`, `W=dia_sem_trabalho`, `V=veredito_sem_lavra_vigente`, `J=encerrada_pelo_juiz`; `e=em_analise`, `a=aberto`. `mesma` significa âncora em ambas as filas; `global` significa `fila_ids=True/fila_cel=False`; `fora` significa ambos falsos. Módulos abreviados por prefixo inequívoco. (`v5_det.txt:1–79`)

| # | Rota | Módulo/status | Razões/âncora/ações | n | Lista completa com linha V5 |
|---|---|---|---|---:|---|
| 1 | fila | turno_aberto/e | A,R; R fora; responder/null | 1 | 141714:1 |
| 2 | fila | turno_aberto/e | A,P; mesma; vazias | 2 | 130162:2, 135778:17 |
| 3 | fila | batida_ausente/e | A,P,R; R fora; P fora e mesma; responder/null | 1 | 118931:3 |
| 4 | fila | batida_ausente/a | P ou A,P; mesma; vazias | 18 | 140522:4, 140524:5, 140707:7, 140928:8, 104784:9, 119517:10, 129384:12, 104040:13, 116436:23, 106963:33, 124364:43, 124370:44, 123033:46, 132144:61, 106350:64, 122073:65, 128516:69, 124774:73 |
| 5 | fila | batida_ausente/e + disputa_supervisao/a | A,P; mesma+global, sintética; vazias | 1 | 139432:6 |
| 6 | fila | turno_aberto/e | A; sem âncora; vazias | 9 | 140331:11, 135067:18, 110506:25, 138672:26, 138912:39, 138914:40, 138916:41, 140580:42, 106594:63 |
| 7 | fila | batida_ausente/e | P ou A,P; mesma; vazias | 14 | 133364:14, 111346:15, 111845:16, 116432:22, 129745:28, 129749:29, 129751:30, 137166:31, 33784:47, 128519:70, 141797:74, 143313:77, 143393:78, 143395:79 |
| 8 | fila | ausencia_colab/a | A; sem âncora; vazias | 2 | 129083:19, 129090:20 |
| 9 | fila | ausencia_colab/e | A; sem âncora; vazias | 1 | 129093:21 |
| 10 | fila | turno_aberto/a | A; sem âncora; vazias | 1 | 137154:24 |
| 11 | pergunta | disputa_supervisao/e | A,P; global; vazias | 1 | 113307:27 |
| 12 | fila | turno_aberto/a | A,R; fora; responder/null | 1 | 129598:32 |
| 13 | fila | batida_ausente/a | A; sem âncora; vazias | 9 | 130931:34, 130933:35, 130935:36, 130939:38, 129567:54, 129569:55, 129573:57, 129577:58, 130781:60 |
| 14 | fila | batida_ausente/e | A; sem âncora; vazias | 4 | 130937:37, 129559:50, 129561:51, 130026:66 |
| 15 | fila | turno_aberto/e + disputa_supervisao/a | A; sem âncora; vazias | 1 | 129217:45 |
| 16 | pergunta | disputa_supervisao/a | P,W; global; vazias | 2 | 130346:48, 130348:49 |
| 17 | fila | batida_ausente/e + par_relampago/a | A; sem âncora; vazias | 1 | 129563:52 |
| 18 | fila | batida_ausente/a | A,P; mesma, sintéticas; vazias | 1 | 129565:53 |
| 19 | fila | batida_ausente/e + disputa_supervisao/a | A; sem âncora; vazias | 2 | 129571:56, 130044:67 |
| 20 | fila | batida_ausente/e | A,V; mesma; vazias | 1 | 130771:59 |
| 21 | fila | batida_ausente/a + turno_aberto_24h/a | A,P; mesma; vazias | 1 | 132052:62 |
| 22 | fila | batida_ausente/e | P; mesma+global; vazias | 1 | 120325:68 |
| 23 | pergunta | disputa_supervisao/a | P; global; vazias | 2 | 128533:71, 127477:72 |
| 24 | fila | batida_ausente/e | A,R; fora; responder/null | 1 | 141810:75 |
| 25 | fila | turno_aberto/e + batida_ausente/e | A,J; mesma; vazias | 1 | 142013:76 |
| **Total** | | | | **79** | V5:1–79 |

**Atenção de precisão:** classes 4 e 7 agrupam conjuntos `P` e `A,P`; se “padrão” exigir igualdade exata do conjunto de razões, devem ser subdivididas. A tabela preserva os fatos no texto de cada classe e permite duas amostras nos grupos de n≥2; grupos unitários não têm segundo exemplo. (`v5_det.txt:4–10,12–16,22–23,28–31,33,43–47,61,64–65,69–70,73–74,77–79`)

Consolidação ampla útil: **30 somente A; 41 com P e eventual A; 2 P,W; 3 A,R; 1 A,P,R; 1 A,V; 1 A,J =79**. (`v5_det.txt:1–79`)

## 13/11 17:43 · sessao 0fcc9646b8a7 · - · - · origem=Ronald (tmux)

| Campo | Escritor | Leitor e significado |
|---|---|---|
| Âncora da pergunta | `app/chamados/services/fio.py:97–117`: `chamado_destino_id`, com fallback para `disputa.chamado_id`. A tarefa a calcula em variável local, `app/chamados/services/tarefa_do_dia.py:127–129`. | `_pela_decisao` atribui essa âncora à porta da ação somente quando pertence à fila recebida, `tarefa_do_dia.py:129`. A carga forma essa fila consultando `qs_na_fila` sobre as âncoras das perguntas, `tarefa_do_dia.py:415–418`. |
| Campo `ancora` do V5 | Não consta da construção das instâncias em `tarefa_do_dia.py:258–260,318–320`. O V5 o descreve como enriquecimento da sonda; implementação dessa escrita **[não verificado]**. | A tarefa não lê um campo `instancia['ancora']`; calcula a âncora pelo objeto pergunta, `tarefa_do_dia.py:127–129`. |
| `acoes[].acao/de/porta` | `_acao` monta os três campos, `tarefa_do_dia.py:114–115`; `_pela_decisao` escolhe ações conforme decisão/passivo, `:128–162`; `_tarefa_pura` publica nas instâncias, `:249–255,302–313`. | `_portas_da_tarefa` achata as ações, mantém portas inteiras pertencentes à fila global, lê o verbo do painel e calcula fio pelos donos, `:547–556`. O backfill lê razões/ações/portas, inclusive humanos com `porta=None`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:97–117`. |
| `fila_ids` e `fila_cel` | `app/chamados/juizes.py:1558–1567`: `fila_ids` contém a fila global com verbo; `fila_cel` agrupa chamados por `celula_id` nas células pedidas. | C17 lê chamados da célula e soma âncoras da pergunta pertencentes à fila global, `juizes.py:1677–1682`. A tarefa usa `fila_ids` para autorizar porta e `fila_cel` somente para determinar `porta_fila` e o subconjunto `fila`, `tarefa_do_dia.py:548–555`. |
| Verbo do ATUAL | `app/chamados/services/verbos.py:30–52`: partição usa módulo, créditos de perguntas e silêncio. `juizes.py:1558` executa sobre `qs_vivos()`. | C17 copia esses verbos em `juizes.py:1678,1682`; traduz verbo em fio por `:1633–1639,1688`. A tarefa copia o mesmo mapa para portas escolhidas pelas ações, mas calcula fio pelos donos das ações, `tarefa_do_dia.py:549,552–556`. |

Leitores de produção da porta publicada: [nao verificado]

- Folha lê `porta_da_tarefa`, fio e `fila`, sem reconstruir âncora: `app/folha/fora_da_folha.py:71–89,233–242`.
- Pauta lê o fio projetado pela folha: `app/ponto/services/pautas_dos_discordantes.py:79–84`.
- Vigia lê rota/fio; explicita que verbo do painel não decide quem age: `app/chamados/services/vigia_fio.py:640–654`.
- Beco exige ausência de porta da tarefa e mantém a âncora da pergunta separadamente: `app/escala/services/cadastro_realidade.py:492–503`.
- Furo sem canal lê presença de porta/rota e distingue `nao_sei`: `app/chamados/services/furo_sem_canal.py:93–101`.
- Instrumento de sombra compara C17 com a projeção `porta_da_tarefa`: `app/chamados/management/commands/diff_e3_sombra.py:127–130`.

Há descrição mais ampla que o predicado na pauta: “não estão esperando NINGUÉM” e “pergunta viva [...] chamado vivo JA tem dono esperando”, embora o código efetivo teste ausência de porta publicada; esta pode existir com ações humanas sem porta. (`app/ponto/services/pautas_dos_discordantes.py:47–56,79–84`; `app/chamados/services/tarefa_do_dia.py:129,133,548–565`)

A folha limita o identificador nomeado ao chamado da célula publicado em `fila`, preservando a distinção entre chamado do dia e âncora da pergunta; não nomeia todas as chaves de `verbos`. (`app/folha/fora_da_folha.py:235–242`; `app/chamados/juizes.py:1654–1657`)

Cuidado normativo: `test_porta_fila_quando_ancora_e_da_celula` constrói pergunta sem resposta/passivo e exige verbo `cobrar`; não escreve o cenário `passivo_sem_linha + acoes=[] + validar/decidir`. O caso A/B exige B vivo na fila e ação com porta B. Os quatro dias humanos do V5 têm âncora fora de ambas as filas, portanto não reproduzem esse caso. (`app/chamados/tests/test_e3_portas_tarefa.py:21–47,67–73`; `v5_det.txt:1,3,32,74`, contando somente `V5_DET`; conferir numeração com a recontagem do agente principal)

## 13/11 17:43 · sessao ae8ca03f0083 · - · - · origem=Ronald (tmux)

Os fatos V5 não demonstram regressão coberta integralmente por `test_e3_portas_tarefa.py`:

| Padrão V5 | Aplicabilidade estrita do caso E3 |
|---|---|
| `passivo_sem_linha`, âncora na fila/célula, ações vazias | **Sem caso escrito E3 completo.** O teste de âncora da célula cria pergunta sem resposta/validação; não cria passivo. (`app/chamados/tests/test_e3_portas_tarefa.py:67–73`; `app/chamados/tests/e3_fixtures.py:65–78`) |
| Tudo `aceso`, perguntas/âncoras ausentes, ações vazias | **Sem caso escrito E3 completo.** “Só fila sem pergunta” monta marco apagado e exige ação `perguntar/sistema/emissor`; os dias inteiramente acesos não reproduzem essa entrada. (`app/chamados/tests/test_e3_portas_tarefa.py:32–38`) |
| `aguarda_resposta`, âncora existente mas fora `fila_ids`, `responder/colaborador/None` | **Sem caso escrito E3 completo.** O caso A/B exige B como âncora **viva** e publica porta B; B fora da fila não reproduz esse fato. (`app/chamados/tests/test_e3_portas_tarefa.py:21–28,40–47`) |
| `veredito_sem_lavra_vigente` ou `encerrada_pelo_juiz`, ações vazias | **Sem caso escrito E3 completo:** os testes de porta não constroem essas decisões/lavras. (`app/chamados/tests/test_e3_portas_tarefa.py:21–111`) |

Há contratos E2 que explicitamente impedem tratar esses resultados como regressões apenas pela existência de âncora:

- Passivo: `passivo_sem_linha`, ações vazias e `nao_sei=True`. (`app/chamados/tests/test_e2_tarefa_do_dia.py:487–495`)
- Marco aceso: nenhuma ação, inclusive havendo pergunta respondida. (`app/chamados/tests/test_e2_tarefa_do_dia.py:867–873`)
- Âncora silenciada: permanece `responder/colaborador`, porém `porta=None`. (`app/chamados/tests/test_e2_tarefa_do_dia.py:396–409`)

O caminho de ausência de porta é explícito: passivo devolve `None` antes de publicar ação; marco aceso encerra a instância; âncora fora da fila vira `porta=None`; a publicação aceita apenas portas inteiras presentes em `fila_ids`. (`app/chamados/services/tarefa_do_dia.py:128–133,262–271,308–313,547–565`)

Portanto, pela leitura estrita dos casos E3 solicitados, **zero regressões demonstradas**, sem concluir que toda perda esteja normativamente correta. A existência de um caso E3 que determine porta para essas combinações permanece **[não verificado]**.

## 13/11 17:42 · sessao 2c7360c1be05 · O168 · verificador · origem=Code

**REPROVA.** Há dois bloqueantes: a constância de custo continua sem prova para várias respostas pendentes, e CSF/retratação ainda respondem diferente ao mesmo caso trancado. (`app/chamados/tests/test_o168_custo_da_guarda_constante.py:47`; `app/chamados/models.py:1902`; `app/chamados/juizes.py:2035`)

1. **Os seis itens do opus v6**

| Item | Resultado da cura |
|---|---|
| 1 — mapa desatualizado | **Curado na cópia.** Os dois endereços estão no mapa; executei o selo, que passou, e comparei o resultado do gerador em memória: igualdade exata. Estarem no mesmo commit é [não verificado]. (`app/docs/MAPA-JUIZES.md:106`, `:114`; `bin/tests/test_mapa_juizes.sh:66`; `bin/gerar_mapa_juizes.py:66`) |
| 2 — frota não medida | **Há medição fornecida**, com universo não vazio e categorias compatíveis com B. A execução pelo juiz real permanece [não verificado]: o script da sonda não acompanha os anexos nem foi localizado na cópia. (anexos `frota_b_head.out:4`; `frota_b_v8.out:4`) |
| 3 — contador lendo só trilha | **Curado para o contraexemplo da resposta recusada.** Agora exige também `not pode_executar_csf(_ch)`; existe teste que mantém a trilha antiga, recusa a resposta e exige contador zero. Execução Django desse teste [não verificado]. (`app/chamados/juizes.py:2530`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:187`, `:195`) |
| 4 — custo constante | **Não curado integralmente.** A consulta por disputa saiu, mas permanecem consultas por pergunta pendente; o teste mantém apenas uma pendente quando aumenta N. Contraexemplo no item 9. (`app/ponto/supra_juiz.py:558`; `app/chamados/models.py:1902`; `app/chamados/tests/test_o168_custo_da_guarda_constante.py:47`) |
| 5 — “MESMO predicado” falso | **Curado documentalmente.** MD e JSON marcam expressamente a afirmação como superada. (`app/docs/CORTES.md:448`; `app/docs/CORTES.json:1405`) |
| 6 — nota contraditória/redundância | **Redundância removida; sequência explicada.** A nota só nasce após `_pend` não vazio. O comentário distingue recusa da cascata e fechamento posterior pelo desligamento; esse fechamento continua fora da guarda. (`app/chamados/models.py:720`, `:735`; `app/colaboradores/services/desligamento.py:151`, `:163`) |

2. **O diff realiza B e somente B?**

No gate de `reconciliar`, implementa a seleção de B: resposta validada ou materializada deixa de segurar; resposta sem ambos segura, ressalvada a tranca. Defesa 1 chega a esse gate. O fechamento ainda depende também das guardas existentes de premissa e pergunta viva. (`app/chamados/models.py:683`, `:686`, `:689`, `:1898`; `app/chamados/signals.py:43`)

**BLOQUEANTE — a “mesma pergunta” da retratação continua divergente.** Contraexemplo: pergunta creditada ao chamado, respondida, não validada, não materializada, com dia trancado. O CSF libera porque respeita a tranca; a retratação segura porque passa `ignorar_tranca=True`. O executor consulta o primeiro e depois chama a segunda. (`app/chamados/models.py:1902`; `app/chamados/juizes.py:2033`, `:2035`; `app/chamados/reconciliador.py:448`, `:519`; `app/ponto/management/commands/supra_juiz.py:261`, `:286`)

Executei o predicado extraído em memória: esse caso devolveu **zero pendentes** normalmente e **uma pendente** com `ignorar_tranca=True`. Portanto houve unificação do corpo, mas não da resposta efetiva exigida pelo aval. (`app/chamados/models.py:1898`; `app/chamados/juizes.py:2035`; anexo `aval.txt:2`)

Há fechamentos fora desse juiz: desligamento, aceite de vínculo, regularização externa e competência trancada usam outras portas. `encerrar` consulta pergunta viva, sem consultar a guarda da conversa. Existe também desligamento por cron. (`app/colaboradores/services/desligamento.py:163`; `app/colaboradores/services/vinculo.py:388`; `app/chamados/services/regularizacao_ext.py:206`; `app/chamados/services/competencia_trancada.py:209`; `app/chamados/models.py:1664`; `app/config/crons.py:440`)

O selo declara essas exceções por nome e cita outro aval; a autorização literal dessas exceções é **[não verificado]**, pois esse aval não foi fornecido. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:3`, `:27`)

3. **Frota B e dia trancado**

A soma fornecida fecha: **862 com conversa = 513 segurados + 349 liberados**; os liberados se dividem em **345 toda julgada + 4 pendentes**. Isso é compatível com o predicado atual, que exclui validadas, materializadas e perguntas trancadas. Não comprova execução pelo juiz real nem fechamento efetivo. (anexo `frota_b_v8.out:4`; `app/chamados/models.py:1898`; `app/ponto/management/commands/supra_juiz.py:286`)

A classificação dos quatro como “dia trancado” não aparece nos campos do JSON fornecido: **[não verificado]**. Se forem os casos descritos, podem ser liberados pelo juiz e novamente retidos pela retratação, conforme o contraexemplo anterior. (`app/chamados/juizes.py:2035`; `app/chamados/reconciliador.py:519`)

A exceção não está enumerada no aval B literal. Ela é **herdada do juiz existente**, documentada como regra anterior da tranca; não nasceu nesta cura. (anexo `aval.txt:2`; `app/chamados/models.py:1896`; `app/chamados/juizes.py:978`)

4. **Quem escreve e quem lê**

| Campo/valor | Escritores | Leitores e significado |
|---|---|---|
| `resposta_colab`, `respondida_em` | Resposta normal grava o par; resposta automática por outro canal também; recuperação pela trilha escreve só o carimbo; reabertura limpa ambos. (`app/chamados/services/disputa_emissao.py:1467`, `:1593`, `:1652`, `:1674`) | O juiz aceita carimbo **OU texto**. Já `estado`/`respondida` dependem do carimbo; materialização exige texto. (`app/chamados/models.py:1900`, `:3101`, `:3552`; `app/chamados/services/materializacao.py:844`) |
| `validada_em` | Validação escreve; devolução e nova resposta limpam; `carimbar(resposta='validar')` também pode preencher automaticamente. (`app/chamados/services/validacao.py:157`, `:222`; `app/chamados/services/disputa_emissao.py:1480`; `app/chamados/models.py:3190`) | Juiz considera qualquer preenchimento suficiente para não segurar; materializador usa o campo para selecionar trabalho. Logo o campo isolado não prova julgamento humano. (`app/chamados/models.py:1899`; `app/chamados/services/materializacao.py:1375`) |
| `materializada_em` | Atribuidor central `_escrever_carimbo`, usado pelas portas de carimbo e desfazimento. Também recebe encerramento por identidade/admin sem criar batida. (`app/chamados/models.py:3153`, `:3120`, `:3241`, `:3259`) | Juiz deixa de segurar quando preenchido. Portanto “materializada” não equivale necessariamente a batida criada. (`app/chamados/models.py:1899`, `:3106`) |
| `fechamento_recusado` | `_registrar_recusa` grava a trilha; o gate fornece motivo `conversa_exige_decisao_humana`. (`app/chamados/models.py:397`, `:408`, `:723`) | Contador usa a trilha como universo histórico e consulta o juiz atual. A nota visível é outra escrita, pela porta de notas. (`app/chamados/juizes.py:2525`, `:2531`; `app/chamados/models.py:735`; `app/chamados/services/nota.py:75`) |
| `pais_segurados_pela_conversa` | Não persiste contador: calcula fila ∩ histórico de recusa ∩ juiz atual segurando. (`app/chamados/juizes.py:2525`, `:2530`) | Leitor encontrado é o teste; não representa toda a frota segurada, pois exige recusa histórica e presença na fila. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:75`; `app/chamados/juizes.py:2515`) |
| `respostas_sem_veredito` / `sem_veredito_entre` | Não gravam estado; o método registrado delega ao corpo compartilhado. (`app/chamados/models.py:1886`, `:1889`) | Fechamento/prontidão, desligamento, ficha, reconciliador, fila por causa, tranca, retratação e CSF. Alguns passam `ignorar_tranca=True`. (`app/chamados/models.py:1909`, `:1942`; `app/colaboradores/services/desligamento.py:141`; `app/colaboradores/services/ficha.py:169`; `app/chamados/reconciliador.py:1164`; `app/chamados/services/fila_por_causa.py:226`; `app/chamados/services/competencia_trancada.py:126`; `app/chamados/juizes.py:2035`; `app/ponto/supra_juiz.py:567`) |
| `perguntas_que_seguram_csf` | Calcula lista ordenada, unindo origem e destino; não grava. (`app/ponto/supra_juiz.py:567`) | Bool registrado e gate/nota leem a mesma lista. (`app/ponto/supra_juiz.py:582`; `app/chamados/models.py:719`, `:738`) |

**Há incompatibilidade concreta de significado:** texto `'07:00'` sem `respondida_em`, sem validação/materialização, segura pelo juiz, mas validar retorna `sem_resposta` e recusar retorna `ja_pendente`. A nota nova manda justamente validar/devolver. Existe reparo pela trilha; sua disponibilidade nesse fluxo de tela é [não verificado]. (`app/chamados/models.py:1900`, `:736`; `app/chamados/services/validacao.py:416`, `:603`; `app/chamados/services/disputa_emissao.py:1661`)

5. **LEI-AKITA e `padrao=`**

O histórico `main..ac8fbe5cc` não está acessível: a pasta não é um repositório Git. Linhas de cada commit, `juizes novos=0`, `padrao=` e presença das curas no mesmo commit: **[não verificado]**.

Pelo critério funcional proposto, **há funções novas compartilhadas**: `sem_veredito_entre` e `perguntas_que_seguram_csf`. Entretanto são extração do corpo existente e composição dos universos; não encontrei segunda cópia da condição material de resposta sem veredito. O padrão realizado é centralização/delegação, com a divergência parametrizada da tranca já apontada. (`app/chamados/models.py:1886`, `:1898`; `app/ponto/supra_juiz.py:561`; `app/chamados/juizes.py:2035`)

6. **Corte na cópia**

**Sim.** Lendo o arquivo da cópia, a frase pedida de nascimento de `DisputaSupervisao.respostas_sem_veredito` está na linha O168-JUIZ-DO-VEREDITO. O registro vivo aponta para o mesmo endereço: `chamados/models.py::DisputaSupervisao.respostas_sem_veredito`. (`app/docs/CORTES.md:449`; `app/core/juizes.py:294`)

7. **Dinheiro**

Não há prova de alteração de entra/fora da folha, rubrica ou valor dos 349; competências e efeitos reais: **[não verificado]**. A classificação de exportação consulta outros insumos, que não foram comparados nos anexos. (`app/folha/export.py:765`, `:830`)

Também **não é demonstrável impacto financeiro zero**: retratar pode resolver perguntas e fechar disputa; fechar disputa agenda retomada de regularizações confirmadas; essa retomada pode criar, corrigir ou remover batidas. (`app/chamados/reconciliador.py:537`; `app/chamados/services/disputa_emissao.py:1987`; `app/chamados/signals.py:139`, `:146`; `app/chamados/services/regularizacao_ext.py:78`, `:137`, `:225`)

Os anexos não informam quantos dos 349 percorrem essa cadeia, nem se atingem competência 10 ou 11. **[não verificado]**

8. **Classes de reprovação**

Há **parâmetro que produz duas respostas à mesma pergunta**, **campo com significados distintos entre leitores** e **selo novo que deixa escapar mutante óbvio**. Os dois primeiros estão nos contraexemplos da tranca e do texto sem carimbo. (`app/chamados/juizes.py:2035`; `app/chamados/models.py:1902`; `app/chamados/services/validacao.py:416`)

No selo da regularização, executei em memória o método AST com original e mutante `if _disp` → `if not _disp`: **ambos passaram**. Ele confere ordem e presença de `return`, sem conferir a polaridade. Há, porém, teste comportamental existente incompatível com esse mutante; não afirmo fuga da suíte inteira. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:217`; `app/chamados/services/regularizacao_ext.py:62`; `app/chamados/tests/test_guard_disputa_reg_externa.py:40`)

Há ainda relógio transitivo fora da borda no contador novo: não recebe `agora`, chama a fila sem instante, e esta usa `timezone.now()`. Contraexemplo de composição: fila avaliada em `t` e contador depois do vencimento de `silenciado_ate` dão universos diferentes. Divergência ocorrida em produção é [não verificado]. (`app/chamados/juizes.py:2511`, `:2530`; `app/chamados/services/painel.py:190`; `app/docs/PLANO-PADROES.md:92`)

Não identifiquei nova escrita direta de estado fora das portas nem nova aquisição de trava no trecho acrescentado; isso não certifica concorrência executada. (`app/chamados/models.py:723`, `:735`; `app/ponto/supra_juiz.py:552`; `app/chamados/juizes.py:2530`)

9. **Custo — bloqueante remanescente**

**O teste não prova constância para três disputas pendentes.** Ele deixa pendente apenas `i == 0`; as demais ficam validadas e pulam a leitura da tranca. (`app/chamados/tests/test_o168_custo_da_guarda_constante.py:47`; `app/chamados/models.py:1899`)

Contraexemplo: mantenha o fixture e deixe as **N respostas pendentes**. Cada uma chama a tranca, que acessa `p.disputa.colaborador`; os universos são carregados sem `select_related`. Mesmo sem data, essas FKs são avaliadas antes do retorno por dia ausente. Executei o predicado em memória: uma pendente chamou a tranca uma vez; três chamaram três vezes. O número exato de SQL em banco é [não verificado], mas o caminho por item permanece. (`app/ponto/supra_juiz.py:558`, `:568`; `app/chamados/models.py:1902`; `app/chamados/juizes.py:991`, `:1024`)

Os tetos passaram para **219/279**, com justificativa textual e referência a medições; os artefatos dessas medições não foram fornecidos, portanto seus resultados são [não verificado]. O próprio comentário limita o cenário medido a zero respostas que consultam tranca: ele não cobre o contraexemplo pendente. (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`)

## 13/11 17:42 · sessao 3fd4fd38ee19 · BO-[nome]-1311-ROTEIRO · sonda · origem=Code

`BYTES_DA_SAIDA 214375` e o aval anexo chegaram. A correspondência 01→col1019, 02→col996, 03→col1032, 04→col246 e 05→col265 é consistente entre UNIVERSO, FICHA e EXTRA recebidos. [nao verificado]

**Não houve nova execução nesta sessão, nem reproduzo abaixo a saída integral.** O arquivo executável da sonda não foi localizado na cópia; recebi sua transcrição raspada. [nao verificado] O wrapper executa a sonda antes de chamar o Codex e entrega seu resultado pelo prompt (`bin/codex_pergunta.sh:15`; `bin/codex_pergunta.sh:324`; `bin/codex_pergunta.sh:357`).

O extrato diagnóstico recebido foi este; ele **não substitui os blocos FICHA/EXTRA integrais**. [nao verificado]

```text
2026-11-13 17:32:55,831 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=46757 resposta=16:01 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 17:32:56,007 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=55375 resposta=14:30 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 17:32:56,026 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=55411 resposta=15:50 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
ERROS {}
BYTES_DA_SAIDA 214375 (teto do wrapper 300000 com os anexos)
```

O mapa final veio vazio, mas existem três diagnósticos `ERROR`; não certifico aplicação correta das perguntas 46757, 55375 e 55411. [nao verificado] O código registra a divergência e continua escolhendo `t1 or t2`; portanto, receber uma previsão depois do diagnóstico não comprova concordância entre as fontes (`app/chamados/services/materializacao.py:302`; `app/chamados/services/materializacao.py:310`; `app/core/observ.py:73`).

Os dados individuais abaixo estão somente na transcrição recebida, sem arquivo local numerado; por isso levam `[nao verificado]`. As referências de código identificam o comportamento conferido na cópia.

**Critério comum:** nenhuma pergunta será classificada **a — VALIDA-HOJE**. `estado_aplicacao_pergunta` prevê derivação, reuso e paridade, mas não projeta a nova lavra nem a retenção posterior (`app/chamados/services/materializacao.py:449`; `app/chamados/services/materializacao.py:463`). O juiz real da retenção é `motivos_retencao_celula`, pelo laço `_dias_sem_decisao`: lê veredito acusador, protestos e exceção de pausa (`app/folha/export.py:1212`; `app/folha/export.py:1273`; `app/folha/export.py:1293`). A exceção exige protestos exatamente `{'FURO_PARCIAL'}` e confirmação de que só faltam marcos de pausa (`app/folha/export.py:1173`; `app/folha/export.py:1148`).

“Não medido” também será usado quando faltar declaração ou diagnóstico de cadastro: validar pergunta sem resposta é recusado, e isso não comprova falta de vínculo ou de marcos (`app/chamados/services/validacao.py:393`). Assim, a classe **d — CADASTRO** não será presumida.

## Caso 01 -- col1019

**Comp 11: nenhum dia retido; nenhuma pergunta viva.** [nao verificado]

| Pergunta | Dia | Declarado | `estado_aplicacao` | Disputa | Classe |
|---|---|---|---|---|---|
| 54117 | 03/11 | hora / nulo | nulo | 7877 fechada | não medido; histórica. [nao verificado] |
| 54118 | 03/11 | hora / nulo | nulo | 7877 fechada | não medido; histórica. [nao verificado] |
| 54119 | 05/11 | hora / nulo | nulo | 7877 fechada | não medido; histórica. [nao verificado] |

As três estão com `viva=false`, via `celula_absolveu`, materializadas em 13/11 às 07:16:02; o chamado 37070 e a disputa 7877 estão encerrados. [nao verificado]

O recorte não contém batidas. Há abonos aprovados nos dias 03, 05, 07, 09, 10 e 12/11; as células correspondentes aparecem como `ausencia`, com veredito `concorde`. [nao verificado]

**Roteiro:** não há dia retido a retirar nesta fotografia. Os vínculos e as regenerações constam do dado, mas a queixa de tela permanece sem print e sem diagnóstico adicional. [nao verificado]

## Caso 02 -- col996

**Comp 11: quatro dias retidos — 21/10, 30/10, 03/11 e 12/11 — todos por `espelho_furo`. Há 16 perguntas vivas, todas na disputa 7263 aberta. A pergunta 46751 é de 20/10, fora da comp 11, e continua incluída por estar viva.** [nao verificado]

Nesta tabela, **D** significa **b — ESPERA OBRA DECLARADO-X-GRAVADO**, existente em `app/docs/BACKLOG.md:1936`.

| Pergunta | Dia / motivo | Tipo / declarado | `estado_aplicacao` recebido | Classe exclusiva |
|---|---|---|---|---|
| 46751 | 20/10 · saída sem entrada | hora / 07:50 | `bloqueada_paridade`: vizinha E 07:00 | **c — DECISÃO DO ADMIN**. [nao verificado] |
| 46756 | 21/10 · intervalo saída | hora / 15:00 | `aplicavel`: E 15:00 | **b — D**. [nao verificado] |
| 46757 | 21/10 · intervalo volta | hora / 16:01 | `aplicavel`: E 14:30; diagnóstico de divergência | **b — D**. [nao verificado] |
| 46758 | 21/10 · `orfao_14h` | hora / 21:05 | `aplicavel`: S 21:05 | **não medido**: saída da retenção não projetada. [nao verificado] |
| 48773 | 22/10 · intervalo saída | hora / nulo | nulo | **não medido**: falta declaração do colaborador. [nao verificado] |
| 57211 | 22/10 · intervalo volta | hora / nulo | nulo | **não medido**: falta declaração do colaborador. [nao verificado] |
| 52035 | 30/10 · `orfao_14h` | hora / 15:50 | `aplicavel`: S 15:50 | **não medido**: saída da retenção não projetada. [nao verificado] |
| 53565 | 03/11 · intervalo volta | hora / nulo | nulo | **não medido**: falta declaração; existe E 14:29:51 no dado. [nao verificado] |
| 53644 | 03/11 · `orfao_14h` | hora / nulo | nulo | **não medido**: falta declaração do colaborador. [nao verificado] |
| 53920 | 04/11 · intervalo saída | hora / 13:01 | `ja_satisfeita`: S 12:54 existente | **c — DECISÃO DO ADMIN**. [nao verificado] |
| 55313 | 08/11 · saída sem entrada | hora / 07:00 | `ja_satisfeita`: E 06:58 existente | **c — DECISÃO DO ADMIN**. [nao verificado] |
| 55375 | 08/11 · intervalo saída | hora / 14:30 | `aplicavel`: S 13:00; diagnóstico de divergência | **b — D**. [nao verificado] |
| 55411 | 08/11 · intervalo volta | hora / 15:50 | `aplicavel`: E 14:30; diagnóstico de divergência | **b — D**. [nao verificado] |
| 56923 | 11/11 · intervalo volta | hora / nulo | nulo | **não medido**: falta declaração do colaborador. [nao verificado] |
| 57210 | 11/11 · intervalo saída | hora / 13:00 | `aplicavel`: S 13:00 | **não medido**: o dia já não retém na fotografia. [nao verificado] |
| 57214 | 12/11 · `orfao_14h` | hora / nulo | nulo | **não medido**: falta declaração do colaborador. [nao verificado] |

**DECLARADO-X-GRAVADO.** O código substitui a hora respondida pela hora do marco nos intervalos (`app/chamados/services/materializacao.py:409`). O tipo prioriza o marco do chamado-destino sobre o da ata, inclusive quando divergem (`app/chamados/services/materializacao.py:274`; `app/chamados/services/materializacao.py:302`). A saída de pausa tem papel S e a volta, E (`app/escala/models.py:1652`). Na 46756, o dado contém declaração de hora, sem declaração separada de E/S; o desencontro demonstrado é entre o motivo de saída de pausa e a previsão E. [nao verificado]

Há também passivo já gravado:

| Pergunta | Declarado | Batida vinculada recebida |
|---|---|---|
| 52619 | 14:28 | 153845 · E 14:30. [nao verificado] |
| 53947 | 14:29 | 153848 · E 14:30. [nao verificado] |
| 55006 | 14:31 | 153851 · E 14:30. [nao verificado] |

Esses exemplos cabem no complemento de **DECLARADO-X-GRAVADO-PASSIVO-11**, obra existente em `app/docs/BACKLOG.md:1937`. O estado `aplicada` não comprova fidelidade ao declarado: nesse ramo a função retorna apenas pela presença de `materializada_em` (`app/chamados/services/materializacao.py:440`; `app/chamados/services/materializacao.py:448`).

**Decisões do admin.**

- **46751:** conferir o fato de 20/10 e a entrada E 07:00; a previsão recusa a aplicação por paridade. [nao verificado] A tela **Resolver dia** oferece Trabalhou, Faltou e Justificado; a view chama a porta de decisão do dia (`app/templates/ponto/partials/_veredito_dia.html:6`; `app/templates/ponto/partials/_veredito_dia.html:21`; `app/ponto/views.py:3340`). O êxito de uma dessas decisões neste caso não foi medido. [nao verificado]
- **53920 e 55313:** decidir se aceita o fato já registrado ou se precisa corrigir o fato. [nao verificado] Validar pelo fio pode quitar a pergunta, mas `ja_satisfeita` expressamente preserva o espelho; não grava a hora declarada (`app/chamados/services/materializacao.py:453`; `app/templates/chamados/partials/modal_fio.html:279`; `app/chamados/services/fio.py:585`).

**Dias retidos e roteiro.**

| Dia / célula | Dependência identificada | O que efetivamente tira a retenção |
|---|---|---|
| 21/10 · 141797 | 46756/46757: **b — DECLARADO-X-GRAVADO**; 46758 sem efeito posterior medido. [nao verificado] | Tratar o desencontro antes de validar; depois conferir nova lavra e retentor. A combinação que libera o dia não foi medida. [nao verificado] |
| 30/10 · 141806 | 52035 prevê S 15:50; existe S 21:28:20 no dado. [nao verificado] | **Não medido**: não foi calculado o veredito/retentor após o plantio. [nao verificado] |
| 03/11 · 141810 | 53565 e 53644 sem declaração. [nao verificado] | O colaborador informa o ocorrido e os horários; depois se confere aplicação e nova retenção. **Não medido** qual resposta libera o dia. [nao verificado] |
| 12/11 · 141819 | 57214 sem declaração. [nao verificado] | O colaborador informa o ocorrido e o horário pedido. **Não medido** o desfecho do dia. [nao verificado] |

A porta de resposta pertence ao colaborador e exige interação com o relógio para resposta de hora (`app/chamados/views.py:1228`; `app/chamados/views.py:1265`; `app/chamados/views.py:1274`).

**08/11 e 11/11 não retêm**, embora tenham perguntas vivas e células com `furo`; não acrescento esses dias à contagem. [nao verificado]

Para cobrir também as **71 perguntas históricas citadas**, todas recebem **não medido para nova validação**; permanecem na disputa 7263 aberta, mas individualmente estão `viva=false`. [nao verificado]

- **Hora, estado `aplicada` — pk=declarado:** 46755=12:55; 49271=15:51; 49272=07:00; 49273=13:00; 49650=13:00; 57212=14:30; 50103=15:50; 50104=07:00; 50105=13:00; 51082=FOLGA; 51124=13:00; 51142=14:30; 51218=21:18; 51747=15:50; 52221=07:03; 52265=13:00; 52283=14:30; 52367=15:50; 52542=06:57; 52594=13:00; 52619=14:28; 52727=15:56; 53947=14:29; 54029=15:52; 54245=07:00; 54292=13:01; 54317=14:29; 54393=15:51; 54592=FOLGA; 54634=FOLGA; 54664=FOLGA; 54936=06:57; 54980=13:00; 55006=14:31; 55104=15:55; 55518=15:52; 55786=13:00; 55812=14:30; 55950=15:50; 56313=13:00; 56342=14:30; 56466=15:50; 57131=07:00; 57184=13:00; 57199=14:30. [nao verificado] [nao verificado]
- **Hora, declarado nulo, estado nulo:** 48771, 48772, 49648, 49649, 50399, 50400, 50401, 50402, 50734, 50786, 50906, 51748, 51913, 53496, 53544, 53880, 54748, 55739, 56249, 56724, 56922 e 57553. [nao verificado]
- **Escolha, declarado nulo, estado nulo:** 53040. [nao verificado]
- **20/10, hora, históricas citadas apenas na FICHA:** 46752=13:01, 46753=07:00 e 46754=15:55. Estão materializadas via declaração; seu `estado_aplicacao` não veio no EXTRA. [nao verificado]

## Caso 03 -- col1032

**Comp 11: nenhum dia retido; nenhuma pergunta viva.** [nao verificado]

| Perguntas | Dia | Tipo / declarado | `estado_aplicacao` | Disputa | Classe |
|---|---|---|---|---|---|
| 57457, 57458, 57459, 57460 | 11/11 | hora / nulo | nulo | 8120 fechada | não medido; históricas. [nao verificado] |
| 57019 | 12/11 | escolha / nulo | nulo | 8097 fechada | não medido; histórica. [nao verificado] |
| 57129 | 12/11 | escolha / nulo | nulo | 8100 fechada | não medido; histórica. [nao verificado] |
| 57157 | 12/11 | escolha / nulo | nulo | 8102 fechada | não medido; histórica. [nao verificado] |
| 57279 | 12/11 | escolha / nulo | nulo | 8108 fechada | não medido; histórica. [nao verificado] |
| 57446 | 13/11 | hora / nulo | nulo | 8118 fechada | não medido; histórica. [nao verificado] |

As nove foram materializadas via `celula_absolveu`. As quatro batidas de 11/11 estão retratadas e não aprovadas; há abono aprovado 5574. Em 12/11 há quatro batidas aprovadas, e a célula consta `ok/concorde`; 13/11 consta `folga/concorde`. [nao verificado]

**Roteiro:** nenhum dia retido a retirar nesta fotografia. A divergência de tela aguardando print permanece sem diagnóstico adicional. [nao verificado]

## Caso 04 -- col246

**Comp 11: retém somente 01/11, célula 136215, por `espelho_cobranca_indevida`. Há duas perguntas vivas na disputa 7755 fechada.** [nao verificado]

| Pergunta | Motivo | Tipo / declarado | `estado_aplicacao` | Disputa | Classe exclusiva |
|---|---|---|---|---|---|
| 52485 | batida fora de escala grave | escolha / nulo | nulo | 7755 fechada | **b — ESPERA OBRA FIO-E0-V4-E3-LEITORES**, com conferência do passivo E4B. [nao verificado] |
| 52715 | `orfao_14h` | hora / nulo | nulo | 7755 fechada | **b — ESPERA OBRA FIO-E0-V4-E3-LEITORES**, com conferência do passivo E4B. [nao verificado] |

O complemento cabe na **E3-LEITORES**, que reúne fio, tela e app sob `tarefa_do_dia`, e na **E4B**, que cobre o passivo do fio morto (`app/docs/BACKLOG.md:1899`; `app/docs/BACKLOG.md:1921`). A linha da E4B registra pouso; isso não demonstra execução ou cura deste caso.

**Qual chamado faz a cobrança?** O dado lista o **36424**, `turno_aberto`, em análise, associado a 01/11; a justificativa 3432 está rejeitada. O pai 36324 está resolvido. [nao verificado] O cartório obtém as cobranças pelos módulos de furo e pelo dia do contexto, não simplesmente pelo pai da disputa (`app/ponto/services/cartorio.py:127`; `app/ponto/services/cartorio.py:135`).

**O elo exato pergunta→chamado não foi medido:** faltam `chamado_destino`, relacionados, contexto, ata e protestos. Não certifico a FK **52715→36424**, nem que 52485 seja a autora da cobrança. [nao verificado] O veredito `cobranca_indevida` pode derivar de dois códigos diferentes; a sonda não os discriminou (`app/escala/models.py:1613`).

**Por que `celula_absolveu` não encerrou:**

1. `cobranca_indevida` é veredito acusador (`app/escala/models.py:1767`).
2. A seleção de perguntas absolvíveis exclui células acusadoras (`app/chamados/reconciliador.py:642`).
3. O juiz C6 mantém viva a pergunta sob esse veredito (`app/chamados/juizes.py:1450`).
4. **A disputa fechada não é a causa dessa exclusão:** `celula_absolveu` alcança também disputas fechadas (`app/chamados/reconciliador.py:572`). Já a alternativa de órfãs por lâmpada filtra somente disputas abertas (`app/chamados/reconciliador.py:1050`).

**Sítio que deveria encerrar a cobrança:** quando reconhece `COBRANCA_SEM_FURO` ou `COBRANCA_EM_DIA_SEM_TRABALHO`, o cartório chama a reconciliação canônica e agenda rejulgamento (`app/ponto/services/cartorio.py:1565`; `app/ponto/services/cartorio.py:1573`; `app/ponto/services/cartorio.py:1576`). Há também encerramento por marco satisfeito (`app/ponto/services/cartorio.py:1539`).

**Roteiro do dia 01/11 — não medido:** conferir a âncora das duas perguntas e o resultado de `tarefa_do_dia`/alcance E4B; identificar a cobrança efetiva, conferir sua reconciliação e o rejulgamento; então consultar novamente o retentor. O resultado dessas portas e a razão histórica de não terem encerrado 36424 não constam do dado. [nao verificado] O backfill E4B não executa rejulgamento: registra `rejulgar_janela`, portanto não basta presumir que ele liberará a folha (`app/chamados/management/commands/backfill_fio_morto_e4b.py:19`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:148`).

As nove históricas citadas têm declarado e estado nulos, `viva=false`, classe **não medido para nova validação**. [nao verificado]

| Perguntas | Tipo | Disputa fechada |
|---|---|---|
| 49603; 49645 | escolha; hora | 7452. [nao verificado] |
| 52482 | hora | 7755. [nao verificado] |
| 54190; 54192 | hora; escolha | 7891. [nao verificado] |
| 54880; 54882; 54986; 55097 | hora; escolha; hora; hora | 7956. [nao verificado] |

## Caso 05 -- col265

**Comp 11: retém somente 31/10, célula 135811, por `espelho_cobrado`. Há três perguntas vivas.** [nao verificado]

| Pergunta | Dia / motivo | Tipo / declarado | `estado_aplicacao` | Disputa | Classe exclusiva |
|---|---|---|---|---|---|
| 52184 | 31/10 · batida fora de escala grave | escolha / Confirma | nulo | 7715 fechada | **c — DECISÃO DO ADMIN**. [nao verificado] |
| 52362 | 31/10 · `orfao_14h` | hora / nulo | nulo | 7715 fechada | **b — ESPERA OBRA RAIZ-DA-DISPUTA**. [nao verificado] |
| 55248 | 08/11 · batida fora de escala grave | escolha / Confirma | nulo | 7984 aberta | **c — DECISÃO DO ADMIN**. [nao verificado] |

**52184 e 55248:** o admin decide se aceita a confirmação, pelo botão **Validar** no fio (`app/templates/chamados/partials/modal_fio.html:279`). A disputa fechada não impede, por si, validar uma pergunta viva: a porta recupera sua própria disputa e permite o ato sem reabrir a raiz (`app/chamados/services/fio.py:585`; `app/chamados/services/fio.py:600`).

Para escolhas, `estado_aplicacao=None` significa fora do escopo dessa previsão (`app/chamados/services/materializacao.py:438`). A materialização da escolha não cria nem remove batidas e reconcilia destino/clones (`app/chamados/services/materializacao.py:1069`; `app/chamados/services/materializacao.py:1082`). Portanto, nenhuma das duas recebe classe a.

**52362:** falta declaração e a dona está fechada. [nao verificado] A consulta do app coloca em pendentes somente perguntas sem resposta **e** em disputa aberta; as demais vivas ficam no histórico (`app/chamados/services/pergunta_no_app.py:61`; `app/chamados/services/pergunta_no_app.py:71`). O complemento cabe em **RAIZ-DA-DISPUTA**, que já contém a decisão sobre dona fechada — reabrir, nova ou Pauta — sem criar id (`app/docs/BACKLOG.md:1931`).

**Roteiro do dia 31/10 — não medido:** decidir 52184 e restabelecer o caminho exigível da 52362 são ações identificadas; não foi demonstrado que elas bastem para liberar a célula. [nao verificado] O cartório só escolhe `cobrado` nesse ramo quando há fio que protesta; isso depende dos marcos e da ata (`app/ponto/services/cartorio.py:1458`; `app/ponto/services/cartorio.py:1479`; `app/ponto/services/cartorio.py:1502`). Esses insumos e o julgamento posterior não vieram na sonda. [nao verificado]

Não atribuo automaticamente a retenção ao chamado 36213 nem à pergunta 52362: o dado não fornece os vínculos e a ata necessários para essa identificação. [nao verificado]

**08/11 não retém**, apesar da 55248 viva em disputa aberta. O atestado 5594 de 13–14/11 está aguardando decisão, mas não aparece como retenção no resultado fornecido. [nao verificado]

As nove históricas citadas recebem classe **não medido para nova validação**:

| Perguntas | Tipo / declarado | `estado_aplicacao` | Disputa fechada |
|---|---|---|---|
| 50066 | escolha / Confirma | nulo | 7504. [nao verificado] |
| 50096 | hora / nulo | nulo | 7504. [nao verificado] |
| 51554, 51648, 51662, 51744 | hora / nulo | nulo | 7639. [nao verificado] |
| 52174 | hora / nulo | nulo | 7715. [nao verificado] |
| 55244, 55505 | hora / nulo | nulo | 7984, atualmente aberta. [nao verificado] |

## Contagem do lote

Contagem das **perguntas vivas**, com uma classe por pergunta. [nao verificado]

| Caso | a | b | c | d | não medido | Total |
|---|---:|---:|---:|---:|---:|---:|
| 01 | 0 | 0 | 0 | 0 | 0 | 0 |
| 02 | 0 | 4 | 3 | 0 | 9 | 16 |
| 03 | 0 | 0 | 0 | 0 | 0 | 0 |
| 04 | 0 | 2 | 0 | 0 | 0 | 2 |
| 05 | 0 | 1 | 2 | 0 | 0 | 3 |
| **Total** | **0** | **7** | **5** | **0** | **9** | **21** |

Contagem dos **colab-dias retidos**, classificando a dependência demonstrada; o efeito de liberação permanece não medido onde indicado no roteiro. [nao verificado]

| Caso | a | b | c | d | não medido | Total |
|---|---:|---:|---:|---:|---:|---:|
| 01 | 0 | 0 | 0 | 0 | 0 | 0 |
| 02 | 0 | 1 | 0 | 0 | 3 | 4 |
| 03 | 0 | 

[truncado em 20000 de 20509 caracteres]

## 13/11 17:40 · sessao e9788c95541f · - · - · origem=Ronald (tmux)

Classe presente: o contador novo não recebe `agora` e chama `qs_na_fila` sem passá-lo (`app/chamados/juizes.py:2511`, `2530`); a função então lê `timezone.now()` (`app/chamados/services/painel.py:190`). Isso contraria o transporte de instante exigido para julgamento (`app/docs/PLANO-PADROES.md:92-94`).

Não o classificaria como bloqueante principal: seu único chamador encontrado é o teste (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:75`); divergência em produção **[não verificado]**.

Contraexemplo de composição: julgamento captura instante `t`; um pai está silenciado até `t+1s`; contador roda em `t+2s`. A fila em `t` exclui o pai, mas o contador inclui. Sua assinatura impede receber `t` (`app/chamados/juizes.py:2511`; `app/chamados/services/painel.py:190`). O congelamento global no teste oculta essa diferença, advertência expressa do padrão (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:35`; `app/docs/PLANO-PADROES.md:91`).

## 13/11 17:39 · sessao 2ff33ed395d2 · BO-[nome]-1311-ROTEIRO · sonda · origem=Code

```text
sonda_leitura: sonda_roteiro.py nao existe
```

Essa é a saída inteira da tentativa local; terminou com código 1, antes da execução da sonda, na verificação de existência do arquivo. `bin/sonda_leitura.sh:71`

Recebi `BYTES_DA_SAIDA 232192`, `ERROS {}` e os anexos de aval e juízes. Esses são registros recebidos na mensagem, sem arquivo local com linhas; abaixo ficam identificados por **[nao verificado]**. Não houve nova leitura de produção nem saída integral de `sonda_roteiro_b.py` produzida nesta sessão. **[nao verificado]**

A correspondência recebida é 06→col982, 07→col1004, 08→col523, 09→col866 e 10→col859; FICHA e EXTRA não apresentam troca entre esses pares. A identidade subjacente não foi conferida. **[nao verificado]**

O juiz da retenção é `motivos_retencao_celula`, que chama `_dias_sem_decisao`: lê veredito, protestos e a exceção de furo somente de pausa. Portanto, pergunta encerrada ou `estado_aplicacao=aplicada` não demonstra, sozinho, que o dia deixou de reter. `app/folha/export.py:1176`, `app/folha/export.py:1212`, `app/folha/export.py:1273`, `app/folha/export.py:1293`

## Caso 06 -- col982

As cinco perguntas vivas recebidas estão abaixo; quatro pertencem à comp 11 e uma, de 20/10, está fora dela. A disputa #6546 permanece aberta. **[nao verificado]**

| Pergunta | Dia | Tipo / declarado | estado_aplicacao | Classe |
|---|---|---|---|---|
| #47164 | 20/10 | hora / FOLGA | invalida: escape FOLGA_CONTESTA | **c — DECISAO DO ADMIN** |
| #47163 | 21/10 | hora / FOLGA | invalida: escape FOLGA_CONTESTA | **c — DECISAO DO ADMIN** |
| #50866 | 27/10 | hora / FOLGA | invalida: escape FOLGA_CONTESTA | **c — DECISAO DO ADMIN** |
| #50968 | 27/10 | hora / FOLGA | invalida: escape FOLGA_CONTESTA | **c — DECISAO DO ADMIN** |
| #51712 | 29/10 | hora / FOLGA | invalida: escape FOLGA_CONTESTA | **c — DECISAO DO ADMIN** |

Os valores individuais da tabela vêm do EXTRA recebido. **[nao verificado]** A classe c decorre da porta real: FOLGA_CONTESTA segue para aceite humano de folga antes da materialização de hora. O admin decide se aceita a contestação da escala; o aceite pode declarar folga ou reconhecer uma folga já lançada. `app/chamados/services/validacao.py:234`, `app/chamados/services/validacao.py:239`, `app/chamados/services/validacao.py:280`, `app/chamados/services/validacao.py:320`, `app/chamados/services/validacao.py:412`

A tela é o modal do fio: ação de validar a resposta, ou rejeitá-la com motivo obrigatório. A disponibilidade efetiva para o usuário de produção não foi medida. `app/templates/chamados/partials/modal_fio.html:279`, `app/templates/chamados/partials/modal_fio.html:289` **[nao verificado]**

**Retenção:** `retem_comp11=[]`; nenhum colab-dia retido neste caso no retrato recebido. Não há dia da comp 11 a liberar. **[nao verificado]**

**Regeneração e os três dias.** As referências antigas do aval não apontam para recusas nesta cópia: `leitor_celula.py:235` é a docstring de `remapear_na_regua`; `portas/celula.py:88` é comentário sobre a ata. `app/escala/services/leitor_celula.py:234`, `app/ponto/portas/celula.py:87`

A guarda atual de competência lavrada, em `regenerar_celulas_vinculo`, testa se o dia pertence a uma janela de `_exp` e o pula. Essas janelas vêm de `_competencias_exportadas`. `app/ponto/portas/celula.py:221`, `app/ponto/portas/celula.py:284`

O cálculo usa `janela_competencia` → `janela_fechamento` → `corte_da_empresa`, que lê `Empresa.dia_inicio_competencia`. Com corte 21, comp 10 significa **21/09–20/10**, e comp 11 significa **21/10–20/11**; essa guarda não usa mês de calendário. `app/ponto/management/commands/gerar_celulas.py:185`, `app/folha/export.py:47`, `app/ponto/janelas.py:10`, `app/ponto/janelas.py:50`, `app/ponto/janelas.py:54`

O dado não identifica os três dias recusados, o retorno da chamada nem o sítio efetivamente percorrido. Logo, **recusa concreta: nao medido; bug de mês de calendário: não provado**. A ausência desses dias em `celulas_regen` também não fornece o motivo de uma recusa. **[nao verificado]**

**Reatividade das quatro perguntas da comp 11.** As células de 21/10, 27/10 e 29/10 aparecem regeneradas, como folga/concorde, enquanto as quatro respostas FOLGA continuam vivas. **[nao verificado]** Isso é compatível com o C6: resposta preenchida é examinada antes da absolvição pela célula; a exceção de concordância com a luz depende de uma hora comparável, e sem ela retorna `None`. `app/chamados/juizes.py:1368`, `app/chamados/juizes.py:1417`

Portanto, a classe demonstrável é **resposta textual preservada para decisão humana após mudança da escala**; falha do evento de regeneração não foi medida. Já existe reação por evento e a obra **CONTRATO-20-DOIS-EVENTOS**, documentada como E2 no ar. `app/ponto/portas/celula.py:528`, `app/docs/BACKLOG.md:1711` **[nao verificado]**

Há também a obra de tela **FOLGA-CONTESTA-DIZ-MANDAR-PARA-CADASTRO**. Ela cabe como complemento sobre a apresentação dessas respostas; uma obra específica que determine encerrá-las automaticamente pela escala corrigida permanece **[nao verificado]**. `app/docs/BACKLOG.md:1517`

## Caso 07 -- col1004

Não há pergunta viva nem dia retido na comp 11 no retrato recebido. A disputa #7574 fechou em 13/11, por `veredito`; a #7565 já estava fechada como `vazia`. **[nao verificado]**

| Perguntas citadas | Dia | Tipo / declarado | estado_aplicacao | Situação / classe hoje |
|---|---|---|---|---|
| #50873 | 27/10 | hora / null | null | não viva; **nao medido**, sem validação pendente demonstrada |
| #50874, #50875 | 27/10 | hora / null | null | não vivas; **nao medido**, sem validação pendente demonstrada |
| #50879 | 27/10 | hora / null | null | não viva; **nao medido**, sem validação pendente demonstrada |
| #52386 | 31/10 | hora / null | null | não viva; **nao medido**, sem validação pendente demonstrada |
| #53312 | 02/11 | hora / null | null | não viva; **nao medido**, sem validação pendente demonstrada |

Os estados da tabela são os recebidos; não entram na contagem de perguntas vivas. **[nao verificado]**

**“Pergunta invalida”.** O detalhe local tem o sítio que recusa perguntas de disputa fechada: filtra `disputa__fechada_em__isnull=True` e devolve essa mensagem quando não encontra a pergunta. O GET também seleciona disputa aberta. `app/chamados/views.py:493`, `app/chamados/views.py:499`, `app/chamados/services/acoes_chamado.py:345`

A outra tela é o fio/modal: carrega perguntas vivas inclusive em disputas fechadas e permite a validação nessa condição se `pergunta_viva` confirmar. O formulário fica no modal. `app/chamados/services/fio.py:212`, `app/chamados/services/fio.py:595`, `app/templates/chamados/partials/modal_fio.html:279`

Assim, **a divergência entre portas está provada no código; qual tela respondeu historicamente ao usuário não foi medida**. O complemento cabe em **FIO-LE-O-JUIZ**, na fila 2 indicada pelo aval recebido. `app/docs/BACKLOG.md:793` **[nao verificado]**

**27/10 — JUSTIFICATIVA-PAGA-O-PERIODO.** O dado mostra justificativa #3331 `esqueci_saida`, aprovada, sem saída informada nem batida gerada; mostra somente a batida S #139122, às 18:59:53, e célula #143004 `ok`. Minutos realizados, período pago e marcos dispensados não vieram na sonda. **[nao verificado]**

O enquadramento desse dia já está documentado em **HJ-093** e **JUSTIFICATIVA-PAGA-O-PERIODO**, incluindo a afirmação documental de realizado zero. Isso sustenta **espera da obra para o problema do período pago**, sem converter as perguntas já encerradas em pendências atuais. `app/docs/BACKLOG.md:742`, `app/docs/BACKLOG.md:1872`

Há ainda código que coleta justificativas aprovadas por data e limpa os marcos ausentes do dia de trabalho. O cálculo financeiro efetivo de 27/10 continua **nao medido** neste dado. `app/escala/utils.py:1196`, `app/escala/utils.py:1254` **[nao verificado]**

## Caso 08 -- col523

| Pergunta | Dia | Tipo / declarado | estado_aplicacao | Disputa | Classe |
|---|---|---|---|---|---|
| #50057 | 25/10 | escolha / null | null | #7497 aberta | **b — ESPERA OBRA CONTRATO-DE-ENTRADA-DO-FIO** |

A pergunta está viva, com `celula_data=null` e `batida_origem=null`; o chamado é #34736. **[nao verificado]** O id da obra já existe no BACKLOG. `app/docs/BACKLOG.md:2100`

`foto_ausente` pertence à família BATIDA e pergunta pela presença no posto. `estado_aplicacao=null` aqui significa fora do escopo da função de aplicação de resposta de hora; não demonstra possibilidade de gravar ou validar. `app/chamados/catalogo/motivos.py:43`, `app/chamados/catalogo/perguntas.py:93`, `app/chamados/services/materializacao.py:438`

O defeito de contrato da família está localizado: o emissor retroativo grava `foto_ausente.batida_id`, enquanto o nascimento procura `ultima_batida.id`; o gerador consulta esse juiz de nascimento. `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`, `app/chamados/models.py:3461`, `app/chamados/services/disputa_emissao.py:325`

O contexto individual de #50057 e seu emissor não foram fornecidos; não está provado que essa pergunta nasceu naquele comando específico. **[nao verificado]**

**Retenção:** `retem_comp11=[]`; nenhum colab-dia retido a liberar. A pergunta sem episódio identificado fica como complemento da obra existente, sem id novo. **[nao verificado]** `app/docs/BACKLOG.md:2100`

## Caso 09 -- col866

As três perguntas vivas recebidas têm resposta vazia e pertencem a disputas fechadas. **[nao verificado]**

| Pergunta | Dia | Tipo / declarado | estado_aplicacao | Disputa | Classe |
|---|---|---|---|---|---|
| #46494 | 21/10 | hora / null | null | #7238 fechada | **nao medido** |
| #49113 | 23/10 | hora / null | null | #7382 fechada por desligamento | **nao medido** |
| #49440 | 23/10 | hora / null | null | #7382 fechada por desligamento | **nao medido** |

Os valores são os recebidos. **[nao verificado]** Não há declarado para comparar com uma gravação; validar sem resposta é recusado pela porta com “Aguardando resposta do colab”. O destino correto de cada pergunta — nova declaração, decisão sobre o dia ou correção cadastral — não foi medido. `app/chamados/services/validacao.py:393` **[nao verificado]**

As perguntas citadas sobre novembro estão assim: **[nao verificado]**

| Pergunta | Dia | Declarado | estado_aplicacao / lastro recebido | Classe hoje |
|---|---|---|---|---|
| #53718 | 02/11 | hora / 18:00 | aplicada; batida S #154171 às 18:00, vinculada à pergunta; não viva | **nao medido**, já aplicada | [nao verificado]
| #53714 | 04/11 | hora / 18:00 | aplicada, via `celula_absolveu`; não viva; sem batida vinculada a ela no dado | **nao medido** | [nao verificado]

A disputa #7840 está fechada. O dado de #53718 demonstra a saída declarada já gravada; não constitui uma validação a fazer hoje. **[nao verificado]**

Para #53714, `aplicada` não prova gravação de 18:00: a função retorna esse estado pela existência de `materializada_em`, salvo a exceção de paridade, antes de conferir o plantio. `app/chamados/services/materializacao.py:440`

**Família TIPO.** Em 04/11 há três batidas S: #145440 às 05:59:55, #145450 às 06:06:53 e #146051 às 17:55:28. O dado não demonstra qual delas deveria ser E, nem traz os marcos da ata ou trilha de inversão; isso é **nao medido**. **[nao verificado]**

O complemento de tipo E/S cabe na família já registrada por **O213** e nos leitores de tipo; a troca de tipo sem aviso possui a obra de fila 2 **TIPO-DE-BATIDA-DE-PERGUNTA-SEM-AVISO**. **TIPO-NA-FONTE** trata formato/acesso da ata, não determina por si a inversão dessas batidas. `app/docs/BACKLOG.md:1462`, `app/docs/BACKLOG.md:1070`, `app/docs/BACKLOG.md:741`, `app/docs/BACKLOG.md:1833`

**Calendário que oculta saídas.** O serviço separa entradas e saídas pelo tipo gravado, mas o template abre o bloco das marcações somente quando há entrada ou retratada. Um dia só com saídas, sem retratadas, não alcança o laço que mostra horários e “Inverter tipo”. Esse defeito está provado no código. `app/colaboradores/services/calendario.py:658`, `app/templates/colaboradores/partials/_calendario_grade.html:118`, `app/templates/colaboradores/partials/_calendario_grade.html:121`

A tela é `calendario_ponto`, que renderiza `calendario_partial.html`; este inclui a grade. O complemento cabe em **E4-CALENDARIO-L1**, com fila 2 conforme o aval recebido. `app/colaboradores/views.py:1938`, `app/colaboradores/views.py:1965`, `app/templates/colaboradores/calendario_partial.html:62`, `app/docs/BACKLOG.md:855` **[nao verificado]**

**Retenção:** `retem_comp11=[]`; nenhum colab-dia retido neste caso. **[nao verificado]**

## Caso 10 -- col859

A disputa #2391 está aberta. Há **41 perguntas vivas**: 24 anteriores à comp 11 e 17 dentro dela. **[nao verificado]**

As 24 históricas têm `tipo=hora`, `declarado=null`, `estado_aplicacao=null`, e permanecem vivas no EXTRA: **[nao verificado]**

| Dia | Perguntas vivas | Classe |
|---|---|---|
| 22/08 | #27518, #27519, #27520, #27521 | **nao medido**, as quatro |
| 24/08 | #27514, #27515, #27516, #27517 | **nao medido**, as quatro |
| 26/08 | #20961, #31399, #31400, #24778 | **nao medido**, as quatro |
| 28/08 | #27510, #27511, #27512, #27513 | **nao medido**, as quatro |
| 30/08 | #29885, #31397, #31398, #24748 | **nao medido**, as quatro |
| 15/09 | #41395, #41396, #41397, #41398 | **nao medido**, as quatro |

Não há declaração nem ficha desses dias que permita escolher o encaminhamento individual. A porta de validação exige resposta; não se pode prometer gravação ou liberação com esses campos vazios. `app/chamados/services/validacao.py:393` **[nao verificado]**

As perguntas da comp 11 estão assim: **[nao verificado]**

| Dia | Pergunta(s) / motivo | Tipo / declarado | estado_aplicacao | Classe |
|---|---|---|---|---|
| 05/11 | #54426 intervalo_saida; #54440 intervalo_volta | hora / FOLGA | invalida: escape | **c**, ambas |
| 07/11 | #55087 saida_sem_entrada; #55143 intervalo_saida; #55157 intervalo_volta; #55225 orfao_14h | hora / FOLGA | invalida: escape | **c**, as quatro |
| 09/11 | #55927 saida_sem_entrada; #55991 intervalo_saida; #56014 intervalo_volta; #56103 orfao_14h | hora / FOLGA | invalida: escape | **c**, as quatro |
| 11/11 | #56960 intervalo_saida; #56974 intervalo_volta; #57049 orfao_14h | hora / FOLGA | aplicada; via admin_decidiu; vivas | **c**, as três | [nao verificado]
| 12/11 | #57278; #57422 batida_fora_escala_grave | escolha / null | null | **nao medido**, ambas |
| 12/11 | #57587 intervalo_saida; #57588 intervalo_volta | hora / null | null | **nao medido**, ambas |

Os estados individuais vêm do dado recebido. **[nao verificado]** As treze respostas FOLGA são **DECISAO DO ADMIN**: decidir se aceita a contestação ou a rejeita com motivo. A porta de aceite antecede a materialização de hora, pode recusar a declaração ou declarar folga. `app/chamados/services/validacao.py:239`, `app/chamados/services/validacao.py:305`, `app/chamados/services/validacao.py:320`, `app/chamados/services/validacao.py:412`

O local é o modal do fio, nos controles de validar e rejeitar resposta com motivo. Nas três de 11/11, a tarefa é conferir e concluir o julgamento das respostas que a sonda ainda declara vivas; `aplicada/admin_decidiu` não autoriza contá-las como encerradas. `app/templates/chamados/partials/modal_fio.html:279`, `app/templates/chamados/partials/modal_fio.html:289`, `app/chamados/juizes.py:1417` **[nao verificado]**

A citada #56904, também de 11/11, declara FOLGA, consta aplicada por `admin_decidiu`, validada e **não viva**; fica como decisão já registrada, fora da contagem de vivas. **[nao verificado]** [nao verificado]

Para as quatro de 12/11, falta resposta; o resultado da escolha ou os horários de intervalo não foram medidos. O modal apresenta espera da resposta e a validação sem resposta é recusada. `app/templates/chamados/partials/modal_fio.html:318`, `app/chamados/services/validacao.py:393` **[nao verificado]**

**Dias que retêm e o que os tira:**

| Dia / célula | Retenção recebida | Encaminhamento |
|---|---|---|
| 07/11 / #127789 | espelho_cobrado | **c — decisão sobre a contestação FOLGA**, representada pelas quatro perguntas desse dia |
| 09/11 / #127791 | espelho_furo | **c — decisão sobre a contestação FOLGA**, representada pelas quatro perguntas desse dia |

Somente esses dois dias constam em `retem_comp11`. **[nao verificado]** O caminho de aceite é declarar folga pela porta; rejeitar a resposta não demonstra liberação do dia. Depois do ato, quem decide a saída da retenção continua sendo `motivos_retencao_celula`, por `_dias_sem_decisao`. **O efeito posterior não foi medido; nenhum dos dois é classe a.** `app/chamados/services/validacao.py:320`, `app/folha/export.py:1212`, `app/folha/export.py:1293` **[nao verificado]**

Em 05/11 e 12/11 há célula acusando, mas o retorno recebido não inclui esses dias na retenção. Os protestos necessários para explicar individualmente a isenção não vieram; a causa precisa é **nao medido**. **[nao verificado]** O código admite exceções para protestos não cobráveis e furo somente de pausa. `app/folha/export.py:1285`, `app/folha/export.py:1293`

**Complemento DECLARADO-X-GRAVADO.** A obra existe, com passivo de comp 11 em id próprio. O código substitui a hora livre de intervalo pela hora do marco. `app/docs/BACKLOG.md:1936`, `app/docs/BACKLOG.md:1937`, `app/chamados/services/materializacao.py:409`

Neste lote, porém, os intervalos vivos têm FOLGA ou resposta vazia. FOLGA é barrada pelo escape antes da substituição de horário; `aplicada` nas duas respostas de intervalo de 11/11 deriva do carimbo e não prova batida. Portanto, **não há delta declarado-versus-gravado demonstrado para esses intervalos**. O complemento é registrar essa limitação da amostra, sem classificá-los automaticamente como espera dessa obra. `app/chamados/services/materializacao.py:359`, `app/chamados/services/materializacao.py:438`, `app/chamados/services/materializacao.py:440` **[nao verificado]**

## Contagem do lote

Contagem sobre os campos `viva` e `retem_comp11` recebidos; perguntas citadas já não vivas ficam fora do denominador de perguntas vivas. **[nao verificado]**

| Caso | a | b | c | d | nao medido | Perguntas vivas |
|---|---:|---:|---:|---:|---:|---:|
| 06 | 0 | 0 | 5 | 0 | 0 | 5 |
| 07 | 0 | 0 | 0 | 0 | 0 | 0 |
| 08 | 0 | 1 | 0 | 0 | 0 | 1 |
| 09 | 0 | 0 | 0 | 0 | 3 | 3 |
| 10 | 0 | 0 | 13 | 0 | 28 | 41 |
| **Total** | **0** | **1** | **18** | **0** | **31** | **50** |

A soma é **0 + 1 + 18 + 0 + 31 = 50** perguntas vivas: 25 na comp 11 e 25 anteriores. **[nao verificado]**

| Classe | Colab-dias retidos na comp 11 |
|---|---:|
| a — VALIDA-HOJE | 0 |
| b — ESPERA OBRA | 0 |
| c — DECISAO DO ADMIN | 2 |
| d — CADASTRO | 0 |
| nao medido | 0 |
| **Total** | **2** |

Os dois colab-dias são col859–07/11 e col859–09/11; a classificação c identifica a decisão necessária, **sem afirmar que sua execução já foi aceita ou liberou a retenção**. **[nao verificado]**

## 13/11 17:39 · sessao e355e2da10f6 · - · - · origem=Ronald (tmux)

- **Gate e signal implementam B:** `_decisao_do_fechamento` chama `_gate_conversa_do_pai`, que consulta a mesma lista do CSF e deixa passar respostas validadas/materializadas; Defesa 1 usa `ato=ch.reconciliar`. (`app/chamados/models.py:686`, `app/chamados/models.py:718`, `app/chamados/models.py:1898`, `app/chamados/signals.py:43`)
- **B não vale globalmente:** desligamento, aceite de vínculo, regularização externa e tranca fecham por `encerrar`/`encerrar_por_competencia_trancada`, sem o gate da conversa; `encerrar` pergunta apenas pela pergunta viva. (`app/colaboradores/services/desligamento.py:163`, `app/colaboradores/services/vinculo.py:388`, `app/chamados/services/regularizacao_ext.py:206`, `app/chamados/services/competencia_trancada.py:209`, `app/chamados/models.py:1664`)
- Esses caminhos estão declarados em lista de exceções, citando outro aval; o literal desse outro aval não foi fornecido. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:3`, `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:27`) [não verificado]
- Há cron efetivo de desligamento: agenda chama comando, comando chama serviço, serviço fecha fora da guarda O168. (`app/config/crons.py:440`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`, `app/colaboradores/services/desligamento.py:163`)
- **Contraponto concreto à frota:** pergunta respondida, sem validação/materialização, com dia trancado e creditada ao chamado produz `pode_executar_csf=True`; porém o executor chama `retratar`, cuja retenção usa `ignorar_tranca=True` e devolve `False`. Portanto “liberado pelo juiz” não significa “efetivamente fechado”; os quatro trancados podem continuar retidos. (`app/chamados/models.py:1902`, `app/chamados/juizes.py:2033`, `app/chamados/juizes.py:2035`, `app/chamados/reconciliador.py:448`, `app/chamados/reconciliador.py:519`, `app/ponto/management/commands/supra_juiz.py:286`)
- A tranca é regra herdada do juiz existente, não novo predicado criado na obra; a retratação opta explicitamente por ignorá-la. (`app/chamados/models.py:1896`, `app/chamados/juizes.py:974`, `app/chamados/juizes.py:2024`)
- **Selo novo fraco demonstrado em memória:** executei o método AST `test_regularizacao_ext_so_fecha_sem_disputa_aberta` extraído, com `open` substituído por `StringIO`: original passou e mutante `if _disp` → `if not _disp` passou. O teste só confere ordem de linhas e presença de `Return`, sem conferir polaridade da condição. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:207`, `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:217`, `app/chamados/services/regularizacao_ext.py:62`)
- Essa lacuna isolada **não prova fuga da suíte inteira**: já existe teste comportamental exigindo zero batidas com disputa aberta, além do caso sem disputa que exige uma batida; o mutante seria incompatível com ambos. Não executei Django. (`app/chamados/tests/test_guard_disputa_reg_externa.py:40`, `app/chamados/tests/test_guard_disputa_reg_externa.py:47`)

## 13/11 17:39 · sessao e9788c95541f · - · - · origem=Ronald (tmux)

**Bloqueante: item 4 do v6 permanece aberto.** A cura elimina a consulta por disputa para carregar perguntas, mas mantém consultas por resposta pendente: o queryset não usa `select_related` (`app/ponto/supra_juiz.py:558`); cada pendente chama a tranca (`app/chamados/models.py:1898-1902`), que acessa `p.disputa.colaborador` (`app/chamados/juizes.py:991`). O teste mantém exatamente **uma** pendente tanto com 1 quanto com 3 disputas (`app/chamados/tests/test_o168_custo_da_guarda_constante.py:47-48`), portanto não demonstra constância quando as três disputas possuem resposta pendente.

Contraexemplo: mesmo fixture, alterando apenas para deixar **todas** as N respostas sem validação. Cada pergunta carregada tem suas FKs frias; haverá leitura da disputa e do colaborador para cada uma. Mesmo sem `data_evento` — campo nullable (`app/chamados/models.py:2479`) e omitido no fixture (`app/chamados/tests/test_o168_custo_da_guarda_constante.py:43`) — essas duas FKs são avaliadas antes de a tranca devolver falso (`app/chamados/juizes.py:991-992`, `1024-1027`). Inferência estática: 2 consultas de universo + 2 por pendente, isto é, 4 versus 8 no caso 1/3; números executados no banco **[não verificado]**.

Os tetos estão em **219/279**, com justificativa textual de +2 e referências a medição (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `210`). A medição referenciada **[não verificado]**; o texto reconhece “tranca só lida para resposta sem veredito; neste cenário 0”, logo não prova o caso pendente do contraexemplo (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`).

Há funções novas compartilhadas que satisfazem o critério literal proposto pelo pedido: `sem_veredito_entre` responde quais perguntas aguardam veredito e é lida pelo método registrado, pela retratação e pelo CSF (`app/chamados/models.py:1886`, `1889`; `app/chamados/juizes.py:2035`; `app/ponto/supra_juiz.py:567-568`); `perguntas_que_seguram_csf` responde a lista que segura e é lida pelo gate e pelo bool registrado (`app/ponto/supra_juiz.py:561`, `582`; `app/chamados/models.py:719`). Isso é extração de corpo e composição, **sem duplicação do predicado**, pois a condição material vive somente em `sem_veredito_entre` (`app/chamados/models.py:1898-1902`). Histórico e `padrao=` dos commits **[não verificado]**.

Verificações sem escrita executadas: `bin/tests/test_mapa_juizes.sh` passou; seu contrato cobra presença das referências no mapa (`bin/tests/test_mapa_juizes.sh:21-53`). O censo AST atual também coincidiu com sua lista fechada, usando as funções extraídas em memória (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:133-166`).

Limites dos selos: o censo só reconhece duas portas e não reconhece `ch.encerrar()` direto; o mutante sintético desse fechamento devolveu `Counter()` (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:68-87`, `124`). O detector do juiz acompanha auxiliares por chamadas diretas, mas não por alias local; um alias para auxiliar que lê `respondida_em`, acompanhado de chamada-isca ao juiz, escapou dos dois predicados AST (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:295-361`). Isso demonstra alcance limitado das catracas; bypass atual de produção por essas formas **[não verificado]**.

Não há nova leitura de relógio nas funções de produção acrescentadas auditadas: elas consultam modelos e autoridades existentes (`app/ponto/supra_juiz.py:552-582`; `app/chamados/models.py:699-740`, `1889-1902`; `app/chamados/juizes.py:2511-2531`).

## 13/11 17:38 · sessao 828a4e1c1441 · - · - · origem=Ronald (tmux)

Escritores/leitores auditados:

| Campo | Escritores | Leitores e significado |
|---|---|---|
| `resposta_colab` / `respondida_em` | Resposta normal grava par; resposta nova limpa validação anterior: `app/chamados/services/disputa_emissao.py:1467-1482`. Resposta automática de fato entregue por outro canal também grava par: `:1638-1657`. Reposição pela trilha grava somente carimbo: `:1661-1681`. Reabertura limpa texto, resposta e validação: `:1593-1633`. | Juiz conta carimbo **OU** texto: `app/chamados/models.py:1899-1902`. `respondida` e `estado` ainda exigem carimbo: `:3552-3554`, `:3097-3104`. Materialização exige texto como anuência: `app/chamados/services/materializacao.py:828-845`. |
| `validada_em` | Decisão humana: `app/chamados/services/validacao.py:150-164`; devolução desfaz: `:218-225`. Resposta nova desfaz: `app/chamados/services/disputa_emissao.py:1470-1482`. Porta de carimbo também preenche automaticamente por simetria, com texto e `resposta='validar'`: `app/chamados/models.py:3186-3193`. Reaberturas também limpam: `:1554-1555`, `:1613`. | Juiz considera qualquer valor como veredito: `app/chamados/models.py:1899`. Materializador seleciona resposta e validação preenchidas, materialização nula: `app/chamados/services/materializacao.py:1375-1385`. Portanto “validada” não comprova autor humano isoladamente. |
| `materializada_em` | Atribuidor único `_escrever_carimbo`: `app/chamados/models.py:3153-3157`; portas `carimbar`, `carimbar_identidade`, `descarimbar`: `:3159`, `:3224`, `:3259`. | Juiz deixa de segurar se preenchido: `:1899`. Campo também recebe encerramento por identidade/admin, sem criar batida: `:3106-3120`, `:3241-3246`; seu `help_text` “batida criada + chamado fechado” é mais estreito que os escritores: `:3449-3451`. |

Contra-exemplo concreto de significado/porta: `resposta_colab='07:00'`, `respondida_em=None`, `validada_em=None`, `materializada_em=None` segura pelo juiz (`models.py:1899-1902`), mas `validar_pergunta` devolve `sem_resposta` (`validacao.py:416-418`) e `recusar_pergunta` devolve `ja_pendente` (`:603-606`). O rótulo `respondida` também é falso (`models.py:3552-3554`). Existe reparo pela trilha (`disputa_emissao.py:1661-1681`); disponibilidade desse reparo na tela [não verificado]. A nova nota orienta ações incapazes de resolver esse estado (`models.py:731-735`).

Leitores do juiz existente: fechamento e prontidão da disputa (`models.py:1909`, `:1942-1950`), desligamento (`app/colaboradores/services/desligamento.py:141-143`), ficha (`app/colaboradores/services/ficha.py:169`), fila por causa (`app/chamados/services/fila_por_causa.py:225-227`), reconciliador (`app/chamados/reconciliador.py:1164-1173`) e competência trancada, esta explicitamente usando `ignorar_tranca=True` (`app/chamados/services/competencia_trancada.py:124-138`).

DINHEIRO: liberar no juiz não equivale a executar CSF. Executor consulta juiz (`app/ponto/management/commands/supra_juiz.py:257-263`), exige execução e desativação da trava T6.1 antes de retratar (`:279-295`). Retratação reconcilia, pode rejeitar justificativa, resolve perguntas e anexa nota (`app/chamados/reconciliador.py:519-541`).

O resolver não fabrica batida diretamente (`app/chamados/services/disputa_emissao.py:1884-1892`), mas carimba perguntas (`:1982-1987`) e pode fechar disputa. Fechar disputa agenda retomada de regularização externa (`app/chamados/signals.py:122-146`); a retomada materializa regularizações confirmadas (`app/chamados/services/regularizacao_ext.py:211-227`) e pode escrever batidas por `registrar_batida` (`:71-83`). Portanto não se pode certificar impacto financeiro zero apenas pela ausência de alteração no cálculo. Quantos dos 349 chegam a essa cadeia, quais competências e quais valores mudam [não verificado].

O juiz do export lê fechamento, escala e demais critérios de exportação (`app/folha/export.py:765-815`, `:830-878`); O168 não fornece evidência de antes/depois desses insumos. Competência 10 exportada versus 11 aberta dos 349 [não verificado].

## 13/11 17:37 · sessao a05fd392c053 · - · - · origem=Ronald (tmux)

O juiz de retenção é `motivos_retencao_celula`, que delega a `_dias_sem_decisao`; a decisão usa veredito acusador, protestos e a exceção da pausa, e não o simples estado `aplicavel` da pergunta (`app/folha/export.py:1212`; `app/folha/export.py:1273`).

- A exceção da pausa exige protestos exatamente `{'FURO_PARCIAL'}` e confirmação por `periodos_do_dia`: sem desalinhamento, sem ponta aberta, sem desconhecimento e com lâmpadas apagadas somente de pausa (`app/folha/export.py:1173`; `app/folha/export.py:1148`).
- `estado_aplicacao_pergunta` verifica derivação de tipo/hora, reutilização e paridade. Não projeta uma nova ata, veredito ou resultado de retenção (`app/chamados/services/materializacao.py:431`).
- A validação agenda recálculo; o cartório consulta `classificar_dia`, deriva e lavra o veredito. Só depois o retentor consegue julgar o novo estado (`app/chamados/services/validacao.py:444`; `app/ponto/services/fechamento.py:1807`; `app/ponto/services/cartorio.py:1464`; `app/ponto/services/cartorio.py:1851`; `app/ponto/portas/celula.py:970`).

Classificação proposta para as 16 perguntas vivas de col996; dados individuais abaixo vêm somente do texto fornecido, sem arquivo correspondente. [nao verificado]

| Pergunta | Classe proposta | Motivo |
|---|---|---|
| 46751 | c — DECISÃO DO ADMIN | `bloqueada_paridade`: conferir o fato de 20/10 e a vizinha E 07:00. [nao verificado] |
| 46756 | b — DECLARADO-X-GRAVADO | Intervalo de saída declarado 15:00; previsão E 15:00, incompatível com o papel S da saída da pausa. [nao verificado] |
| 46757 | b — DECLARADO-X-GRAVADO | Declarado 16:01; previsão E 14:30. [nao verificado] |
| 46758 | não medido | Previsão S 21:05 coincide com a hora; liberação do dia não foi projetada. [nao verificado] |
| 48773 | não medido / d se cadastro incluir resposta | Falta resposta da saída de intervalo de 22/10. [nao verificado] |
| 57211 | não medido / d se cadastro incluir resposta | Falta resposta da volta de intervalo de 22/10. [nao verificado] |
| 52035 | não medido | Previsão S 15:50 coincide; liberação de 30/10 não foi projetada. [nao verificado] |
| 53565 | não medido / d se cadastro incluir resposta | Falta resposta da volta de intervalo de 03/11; existe E 14:29:51 no dado. [nao verificado] |
| 53644 | não medido / d se cadastro incluir resposta | Falta resposta de `orfao_14h` de 03/11. [nao verificado] |
| 53920 | c — DECISÃO DO ADMIN | Declarado 13:01; `ja_satisfeita` por S 12:54. Aceitar a batida existente apenas quita a pergunta; corrigir o fato exige decisão. [nao verificado] |
| 55313 | c — DECISÃO DO ADMIN | Declarado 07:00; `ja_satisfeita` por E 06:58. Mesma distinção. [nao verificado] |
| 55375 | b — DECLARADO-X-GRAVADO | Declarado 14:30; previsão S 13:00. [nao verificado] |
| 55411 | b — DECLARADO-X-GRAVADO | Declarado 15:50; previsão E 14:30. [nao verificado] |
| 56923 | não medido / d se cadastro incluir resposta | Falta resposta da volta de intervalo de 11/11. [nao verificado] |
| 57210 | não medido | Previsão S 13:00 coincide; 11/11 já não consta retido. [nao verificado] |
| 57214 | não medido / d se cadastro incluir resposta | Falta resposta de `orfao_14h` de 12/11. [nao verificado] |

`DECLARADO-X-GRAVADO` existe em `app/docs/BACKLOG.md:1936`; o passivo de comp 11 tem obra própria em `app/docs/BACKLOG.md:1937`. O código efetivamente substitui a hora declarada pela hora do marco nos intervalos (`app/chamados/services/materializacao.py:409`). O tipo da pausa é S para saída e E para volta (`app/escala/models.py:1652`).

Para perguntas sem resposta, validar retorna `sem_resposta`; o colaborador informa a hora pela porta de resposta. Isso não prova ausência de cadastro de escala — recomendo **não medido** se a classe d significa estritamente cadastro de vínculo/marcos (`app/chamados/services/validacao.py:393`; `app/chamados/views.py:1228`; `app/chamados/views.py:1265`; `app/chamados/views.py:1274`).

A tela da decisão humana é **Resolver dia**, com Trabalhou/Faltou/Justificado; a view chama o despachante por data (`app/templates/ponto/partials/_veredito_dia.html:6`; `app/templates/ponto/partials/_veredito_dia.html:21`; `app/ponto/views.py:3312`; `app/ponto/views.py:3340`). A aceitação de batida existente sem alterar espelho está explícita em `app/chamados/services/materializacao.py:453`.

Os quatro dias retidos são 21/10, 30/10, 03/11 e 12/11. 08/11 e 11/11 não retêm no EXTRA, embora tenham perguntas vivas/furo. [nao verificado]

**Contagem conservadora das perguntas:** a=0, b=4, c=3, d=0, não medido=9; total=16. A contagem deriva das linhas acima e dos dados textuais fornecidos. [nao verificado]

## 13/11 17:36 · sessao 82f11e3d3f22 · - · - · origem=Ronald (tmux)

Caso 09: o defeito de tela está provado no código. O serviço separa `entradas` e `saidas` pelo tipo gravado (`app/colaboradores/services/calendario.py:658`); o template só abre o bloco de horários quando há entrada ou retratada (`app/templates/colaboradores/partials/_calendario_grade.html:118`). Portanto um dia somente com saídas fica sem horários e sem o botão “Inverter tipo”, situado dentro desse bloco (`app/templates/colaboradores/partials/_calendario_grade.html:121`). A view é `calendario_ponto` (`app/colaboradores/views.py:1938`), renderiza `calendario_partial.html` (`app/colaboradores/views.py:1965`), que inclui a grade (`app/templates/colaboradores/calendario_partial.html:62`). Complemento cabe em `E4-CALENDARIO-L1` (`app/docs/BACKLOG.md:855`), com fila 2 determinada pelo aval recebido. [nao verificado]

A família de tipo E/S deve ser distinguida de `TIPO-NA-FONTE`, que trata formato/acesso da ata (`app/docs/BACKLOG.md:1833`). Para E/S existem `O213`, sobre tipo do motivo contra tipo da ata (`app/docs/BACKLOG.md:1462`), e `GRUPO-1-LISTA-CODEX-T11`, sobre os leitores de tipo (`app/docs/BACKLOG.md:1070`). `HJ-092` foi encerrada como BATIDA e preserva o marco cobrado (`app/docs/BACKLOG.md:740`); o aviso da troca ficou em `TIPO-DE-BATIDA-DE-PERGUNTA-SEM-AVISO`, fila 2 (`app/docs/BACKLOG.md:741`). A sonda não fornece os marcos da ata nem trilha de flip de 04/11; qual saída estaria errada é “não medido”. [nao verificado]

`estado_aplicacao_pergunta` retorna `aplicada` para pergunta de hora com resposta e `materializada_em`, salvo a via específica de recusa por paridade (`app/chamados/services/materializacao.py:438`). Assim, `53714 = aplicada` não demonstra que declarou e gravou a mesma batida: a função encerra a leitura antes de derivar timestamp/tipo (`app/chamados/services/materializacao.py:448`). No dado, a via é `celula_absolveu`, e não há batida com `pergunta_origem=53714`. [nao verificado]

Caso 10: `DECLARADO-X-GRAVADO` já existe (`app/docs/BACKLOG.md:1936`), com passivo de comp 11 em obra própria (`app/docs/BACKLOG.md:1937`). O código efetivamente substitui a hora declarada do intervalo pela hora do marco (`app/chamados/services/materializacao.py:409`). Entretanto, as respostas `FOLGA` deste lote caem antes no bloqueio de escape, sem derivar batida (`app/chamados/services/materializacao.py:359`). Nenhum delta de hora pode ser provado para essas respostas. [nao verificado]

Crucial: `estado_aplicacao=invalida` não descreve toda a porta “Validar”. O validador reconhece `FOLGA_CONTESTA` (`app/chamados/services/validacao.py:234`) e desvia para `_aceitar_a_folga` antes da materialização (`app/chamados/services/validacao.py:408`). Essa é decisão humana sobre contestação da escala (`app/chamados/services/validacao.py:239`), compatível com classe **c**, sem afirmar classe a. Dia já folga recebe carimbo/fecho sem nova escrita do dia (`app/chamados/services/validacao.py:280`); nos demais, a porta consulta `checar_declaracao` e pode recusar (`app/chamados/services/validacao.py:305`), ou declara folga pela porta existente (`app/chamados/services/validacao.py:320`). O resultado posterior no juiz de retenção não foi medido. [nao verificado]

A tela do fio oferece validar (`app/templates/chamados/partials/modal_fio.html:279`) e rejeitar com motivo obrigatório (`app/templates/chamados/partials/modal_fio.html:289`); a apresentação chama o validador único (`app/chamados/services/acoes_disputa.py:13`). Para os retidos 07/11 e 09/11, o admin decide se aceita a contestação de folga ou a rejeita com motivo; a sonda não mede o resultado dessa decisão. [nao verificado]

“Resolver dia” no calendário possui opções e efeitos no template (`app/templates/ponto/partials/_veredito_dia.html:8`), e a view chama o serviço (`app/ponto/views.py:3340`). Porém sua exibição depende de `resolver_dia_oferecido` (`app/templates/colaboradores/partials/_calendario_grade.html:125`), falso nos dados enviados; portanto não afirmar que o botão está disponível nesses dias. [nao verificado]

A porta fecha perguntas e chamados pela função `fechar_o_fio_do_dia` (`app/ponto/services/veredito_celula.py:160`), mas resposta do colaborador pode continuar viva antes da leitura de via de identidade, salvo decisão explícita do admin na célula ou concordância com a luz (`app/chamados/juizes.py:1417`). Isso explica por que se deve preservar o `viva` da sonda, inclusive nas perguntas carimbadas `admin_decidiu`, sem recalcular sua classe a partir apenas dessa via. (`app/chamados/juizes.py:1425`)

## 13/11 17:36 · sessao 717053fe02e8 · - · - · origem=Ronald (tmux)

Caso05: recomendação de classes exclusivas: **52184=c; 52362=b RAIZ-DA-DISPUTA; 55248=c; dia retido31/10=nao medido quanto à ação que efetivamente o libera**. O fundamento de cada classificação segue abaixo; o dado de produção está apenas na mensagem, sem arquivo:linha [nao verificado].

- **52184**, escolha `Confirma`, viva, disputa7715 fechada; **55248**, escolha `Confirma`, viva, disputa7984 aberta: o admin decide se aceita a confirmação. A tela do fio oferece **Validar** em `app/templates/chamados/partials/modal_fio.html:279`; o processamento aceita viva de disputa fechada sem reabrir em `app/chamados/services/fio.py:585`, especialmente `:600-609`. A fechada oculta somente rejeitar/corrigir, conforme `app/templates/chamados/partials/modal_fio.html:289`. Portanto, não são bloqueadas pela mera raiz fechada. **Classe c** para ambas.
- Para escolhas, `estado_aplicacao_pergunta=None` significa fora do escopo dessa previsão: o retorno precoce está em `app/chamados/services/materializacao.py:438`. Validar escolha resolve **sem criar/remover batida** e reconcilia destino/clones, conforme `app/chamados/services/materializacao.py:1069` e `:1082-1086`. Não classificar a nem prometer que a célula deixe de reter.
- **52362**, hora, declarado nulo, viva na disputa7715 fechada: sem resposta, a validação recusa em `app/chamados/services/validacao.py:393`. A consulta do app coloca em pendentes somente pergunta sem resposta **e disputa aberta**; as outras vivas vão ao histórico, em `app/chamados/services/pergunta_no_app.py:61`, especialmente `:70-75`. **Classe b: ESPERA OBRA RAIZ-DA-DISPUTA**, por canal da pergunta em dona fechada; a obra já inclui decidir reabrir/nova/Pauta em `app/docs/BACKLOG.md:1931`. Não dizer que a sonda provou qual dessas opções será adotada.
- O complemento de05 cabe na obra existente **RAIZ-DA-DISPUTA**, elo raiz-filhos e reação do agregado, `app/docs/BACKLOG.md:1931`; **RAIZ-DA-DISPUTA-F0F1** registra que o bug de produção continua atéF2, `app/docs/BACKLOG.md:1909`. Não precisa de id novo.

**Dia31/10:** o dado informa célula135811, `cobrado`, único dia retido, mas não informa ata, protestos, `chamado_destino`, relacionados nem julgamento projetado [nao verificado]. Logo, não provar qual filho causou esse `cobrado` nem que validar52184 encerra52362 e libera o dia.

O juiz real da retenção lê célula acusadora e as exceções de protestos/pausa em `app/folha/export.py:1273`, com retenção em `:1293-1294`. O cartório escolhe `cobrado` quando não há códigos e há fio que protesta, conforme `app/ponto/services/cartorio.py:1479` e `:1502-1503`; `_protesta` consulta os marcos acesos em `:1458-1460`. A previsão da transição desse dia não consta da sonda: **nao medido**.

Existe alternativa humana de decidir o fato do dia em **Resolver dia** — trabalhou/faltou/justificado — `app/templates/ponto/partials/_veredito_dia.html:6`, opções `:21-25`, view `app/ponto/views.py:3312` e chamada do juiz `:3340`. Isso é porta existente; não afirmar sucesso antecipado para31/10.

A vitalidade não depende de disputa aberta: resposta sem validação pode permanecer viva em `app/chamados/juizes.py:1417-1429`, e célula acusadora também mantém viva em `:1450-1451`. Fechar a raiz deveria consultar o C7 em `app/chamados/models.py:1849`; não podemos reconstruir o estado das perguntas no instante do fechamento apenas pelos carimbos atuais [nao verificado].

_4721 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
