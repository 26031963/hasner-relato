# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 11:56** · publicado em **10/11/2026 11:58** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 11:56 · sessao 0a7c8a61e347 · FIO-E0-V4-E4A-2 · verificador · origem=Code

1. **Não é idêntica ao main informado.** A diferença integral da fábrica é: novo leitor da lâmpada `dia_nunca_bateu_sem_adesao` e duas guardas que retornam antes de emitir pergunta ou pauta. A barreira de veredito não acusador continua incondicional; a forma (B) saiu. (`app/chamados/services/fabrica_da_celula.py:87`, `app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/fabrica_da_celula.py:120`, `app/chamados/services/fabrica_da_celula.py:183`.)

   Conferi os blobs em memória: atual `8ab8001ca…`; retirando somente essas três adições, resulta `8a4052f6c…`, exatamente os índices do diff integral fornecido. Comparação direta com o commit main, sem `.git` nesta cópia: [nao verificado]. (`app/chamados/services/fabrica_da_celula.py:87`, `app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/fabrica_da_celula.py:183`.)

   Quem alcança a diferença:

   - Cartório → `perguntar_pela_celula` → guarda de `nunca_bateu`. (`app/ponto/services/cartorio.py:1008`, `app/chamados/services/fabrica_da_celula.py:183`.)
   - Reconciliador → `perguntar_por_eventos` → `perguntar_pela_celula`. (`app/chamados/reconciliador.py:1386`, `app/chamados/services/fabrica_da_celula.py:301`.)
   - `contador_fio_mudo` lê o mesmo predicado para separar retidos; para os demais, pergunta a `motivos_a_perguntar`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:111`, `app/chamados/management/commands/disparar_perguntas_competencia.py:115`.)
   - Repasse também lê o predicado e retém `nunca_bateu`. (`app/chamados/services/disputa_emissao.py:757`.)

2. **Sim: a obra inteira muda emissão em produção.** Retira a escrita do lote e impede emissão para `nunca_bateu` na fábrica e no repasse; isso não deve ser confundido com a forma (B), retirada nas voltas finais. (`app/chamados/management/commands/disparar_perguntas_competencia.py:189`, `app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/disputa_emissao.py:758`.)

   Inventário dos arquivos produtivos alterados e dos escritores alcançados:

   | Sítio | Efeito e escrita |
   |---|---|
   | `disparar_perguntas_competencia` | O escritor anterior foi removido pelo diff fornecido. Agora seleciona, conta, imprime e eventualmente retorna 2; não cria/reabre/fecha pergunta, chamado ou notificação. (`app/chamados/management/commands/disparar_perguntas_competencia.py:99`, `app/chamados/management/commands/disparar_perguntas_competencia.py:189`, `app/chamados/management/commands/disparar_perguntas_competencia.py:219`.) |
   | Fábrica: `motivos_a_perguntar` | Leitura; `nunca_bateu` retorna vazio. (`app/chamados/services/fabrica_da_celula.py:105`.) |
   | Fábrica: `perguntar_pela_celula` | **Pode escrever**, mas agora `nunca_bateu` sai antes de pauta, descarimbo e pergunta. Demais casos continuam pelas portas existentes, com push silenciado. (`app/chamados/services/fabrica_da_celula.py:183`, `app/chamados/services/fabrica_da_celula.py:204`, `app/chamados/services/fabrica_da_celula.py:209`, `app/chamados/services/fabrica_da_celula.py:218`.) |
   | Fábrica: `perguntar` | Escritor alcançado: desfaz carimbo, cria/atualiza disputa e gera perguntas; seu corpo não mudou no diff integral. (`app/chamados/services/fabrica_da_celula.py:169`, `app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:174`.) |
   | Repasse | Novo corte em `motivo_fora_da_fabrica`, usado antes dos caminhos de complemento e criação. Os caminhos restantes **podem escrever** chamado pai, disputa, perguntas, transição do chamado, fechamento e push. (`app/chamados/services/disputa_emissao.py:758`, `app/chamados/services/disputa_emissao.py:1066`, `app/chamados/services/disputa_emissao.py:1088`, `app/chamados/services/disputa_emissao.py:1104`, `app/chamados/services/disputa_emissao.py:1175`, `app/chamados/services/disputa_emissao.py:1284`, `app/chamados/services/disputa_emissao.py:1290`.) |
   | `pergunta_no_app` | Acrescenta descrição/dono da retenção; não emite pergunta ou push. (`app/chamados/services/pergunta_no_app.py:35`.) |
   | `resposta_tardia` | Diff apenas de formatação. Continua **podendo reabrir** pela porta existente, ou escrever nota, auditoria e pauta; não ganhou corte de `nunca_bateu`. (`app/chamados/services/resposta_tardia.py:45`, `app/chamados/services/resposta_tardia.py:53`, `app/chamados/services/resposta_tardia.py:55`, `app/chamados/services/resposta_tardia.py:63`.) |
   | `vigiar_fio_morto` | **Escreve arquivo JSON**, não pergunta/chamado/push. Acrescenta os dois contadores ao carimbo. (`app/chamados/management/commands/vigiar_fio_morto.py:135`, `app/chamados/management/commands/vigiar_fio_morto.py:233`, `app/chamados/management/commands/vigiar_fio_morto.py:241`.) |
   | Crons / placar | Cron troca `--apply` por `--alarme`; placar lê o carimbo e alarma, sem emitir entidades de atendimento. (`app/config/crons.py:1521`, `app/core/management/commands/pipeline_placar.py:92`, `app/core/management/commands/pipeline_placar.py:159`.) |

   Portanto, não nasceu escritor novo de notificações; foram retirados caminhos que poderiam provocá-las, inclusive o antigo `--com-push` do lote. (`app/chamados/management/commands/disparar_perguntas_competencia.py:132`, `app/chamados/services/fabrica_da_celula.py:218`, `app/chamados/services/disputa_emissao.py:1289`.)

3. **O `FIO MUDO: 1` vem do juiz real, com entradas controladas.** O teste chama diretamente `motivos_a_perguntar` e exige exatamente `['intervalo_volta']` às 13:40; o contador também chama essa função. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:454`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:475`, `app/chamados/management/commands/disparar_perguntas_competencia.py:115`.)

   **Há escolhas explícitas na fixture:** grava `furo`/`concorde`, usa `bulk_create` para evitar sinais e dubla `_ata_do_dia` com faltas prescritas. Ela não substitui o resultado do juiz, mas também não prova que a lavra real produziria aquele veredito/ata. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:52`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:445`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:448`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:456`.)

   **Concorde → 0 tem mordida contra (B):** exige vazio diretamente do juiz, com intervalo vencido e turno ainda em curso. A forma (B) liberaria esse caminho; execução Django dessa mutação: [nao verificado]. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:17`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:479`, `app/chamados/services/fabrica_da_celula.py:139`.)

4. **As alterações dos testes existentes não são somente cosméticas.** Inventário versus o main representado pelo diff fornecido; comparação Git independente: [nao verificado]. [nao verificado]

   | Teste/arquivo atual | Alteração da asserção | [nao verificado]
   |---|---|
   | `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:48`, `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49`, `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:54` | Rótulos `[EMITE]`→`VIGIA` e ALVOS novo; números 1/1 e 2/2 preservados. |
   | `app/chamados/tests/test_fabrica_pela_ata.py:51`, `app/chamados/tests/test_fabrica_pela_ata.py:64`, `app/chamados/tests/test_fabrica_pela_ata.py:71` | Só texto ALVOS, zero preservado. |
   | `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:140`, `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:141` | Alvo continua 1; perguntas **1→0**. |
   | `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:151`, `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:158` | Retratada: primeira pergunta passa a nascer pelo evento; estoque final **2→1**. Acrescenta pendência da célula 1 e rc 2. |
   | `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:168` | Limbo: perguntas **1→0**; pendência 1 e rc 2 permanecem. |
   | `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:177` | Retira diagnóstico `PARCIAL NA LISTA DO ADMIN 1`; exige total `VIGIA FIO MUDO: 0`. |
   | `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:26` | Exige `--alarme` em lugar de `--apply`; acrescenta proibição de `--apply`. |
   | `app/chamados/tests/test_o261_fabrica_le_a_lampada.py:103` | Para `nunca_bateu`, `None`→causa `celula_nunca_bateu`; demais acusadores mantêm `None`. |
   | `app/chamados/tests/test_o261_vigia_fio.py:815`, `app/chamados/tests/test_o261_vigia_fio.py:825` | Igualdades completas de censo/esperado ganham os dois contadores. |
   | `app/chamados/tests/test_pergunta_do_dia_em_curso.py:106` | ALVOS zero preservado, texto novo. |
   | `app/chamados/tests/test_pergunta_do_dia_em_curso.py:424` | `assertNotIn` da linha zero→regex exigindo linha existente com número positivo: **reforço**. |
   | `app/chamados/tests/test_pergunta_do_dia_em_curso.py:475`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:477` | Igualdade das perguntas emitidas `['intervalo_volta']`→ausência de perguntas; adiciona a mesma lista como resultado direto do juiz. |
   | `app/chamados/tests/test_selo_emissor_fio_mudo.py:84`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:119`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:173` | ALVOS novo, número 1 preservado. |
   | `app/chamados/tests/test_selo_emissor_fio_mudo.py:91`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:147` | Zero preservado; retiradas as exigências `desligado 1` e `FIO DECIDIDO 1`. |
   | `app/chamados/tests/test_selo_emissor_fio_mudo.py:133`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:164` | Só `[EMITE]`→`VIGIA`, mesmos números; o método de `:164` já é `_test_`, não coletável. |
   | `app/chamados/tests/test_selo_emissor_fio_mudo.py:188`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:201` | Cron exige alarme; mensagem dry-run substituída por alvo 1 e ausência de DRY/APLICADO. |
   | `app/chamados/tests/test_selo_emissor_fio_mudo.py:270` | `assertTrue(vivas)`→`assertFalse(vivas)`; acrescenta conservação do estoque, contadores e rc. |
   | `app/core/tests/test_medida_turno_do_par.py:268` | Posição registrada **1196→1199**, ajuste de localização. |
   | `app/escala/tests/test_termometro_regua_prazo.py:19`, `app/ponto/tests/test_selfie_ausente_juiz.py:148`, `app/ponto/tests/test_varredura_lote_prazo.py:62` | Juízes por varredura **1→0**; acrescenta papel `vigia`. |
   | `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:338`, `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:420` | Catraca troca obrigação de leitura direta no handle por delegação obrigatória do contador ao juiz. |
   | `bin/tests/test_papel_prazo_nao_deriva.sh:305`, `bin/tests/test_papel_prazo_nao_deriva.sh:384` | Exceção real fica vazia; prova nominal usa exceção sintética e acrescenta verificação contra vazamento. |

   **Há perda de cobertura diagnóstica nos três casos nomeados acima:** total zero não comprova a causa do bloqueio. Isso merece ser registrado como retirada de asserção, embora acompanhe a remoção desses diagnósticos do comando. (`app/chamados/tests/test_selo_emissor_fio_mudo.py:91`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:147`, `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:177`.)

   No diff fornecido, não identifiquei `assertEqual`→`assertTrue`, encurtamento de `assertIn` para ocultar incompatibilidade, nem remoção integral de caso coletável; há inversões explícitas do contrato escritor→vigia e acréscimos de conservação de estoque. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:475`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:477`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:270`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:271`.)

   As demais mudanças preexistentes são de fixture/chamada, preservando asserções: COBPARC, resposta tardia e desligado; há novas verificações de ausência de escrita. (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:198`, `app/chamados/tests/test_resposta_tardia.py:34`, `app/chamados/tests/test_selo_disparo_nao_cobra_desligado.py:90`, `app/chamados/tests/test_fabrica_por_evento.py:84`.)

5. **Matriz e bancos 1–4 permanecem realizados no código examinado; não encontrei regressão deles no delta 9e/9f.** A restauração da barreira para não acusadores atende à última errata. (`app/chamados/services/fabrica_da_celula.py:120`, `app/chamados/tests/test_e4a2_matriz_memoria.py:198`.)

   | Requisito | Realização |
   |---|---|
   | (a) Retido | Lê a lâmpada, sem recalcular adesão; fábrica retorna antes da emissão. Contador compara pares únicos com a C5 real, na interseção declarada; desligado somente nos pares conservados pela C5. (`app/chamados/services/fabrica_da_celula.py:89`, `app/chamados/services/fabrica_da_celula.py:183`, `app/chamados/management/commands/disparar_perguntas_competencia.py:54`, `app/chamados/management/commands/disparar_perguntas_competencia.py:59`, `app/chamados/management/commands/disparar_perguntas_competencia.py:66`.) |
   | (b) Alvo | Pergunta ao juiz inteiro, deduplica todos os motivos; esperado 0, dono ESTRUTURA. Alvo isolado não produz rc 2 no disparar. (`app/chamados/management/commands/disparar_perguntas_competencia.py:115`, `app/chamados/management/commands/disparar_perguntas_competencia.py:122`, `app/chamados/management/commands/disparar_perguntas_competencia.py:219`.) |
   | (c) Materializada | Pergunta viva exclui antes da separação dos dois contadores; materializada+retratada não é equiparada automaticamente a viva. (`app/chamados/management/commands/disparar_perguntas_competencia.py:109`, `app/chamados/services/fabrica_da_celula.py:145`, `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:153`.) |
   | (d) Tranca | Contador seleciona VIVOS; prova de integração exige que a tranca encerre primeiro e depois ambos zerem. Execução ORM: [nao verificado]. (`app/chamados/management/commands/disparar_perguntas_competencia.py:99`, `app/chamados/tests/test_e4a2_nunca_bateu.py:164`.) |
   | Errata de unidade | Fronteira recebe iterável, normaliza para **PKs de empresa**, inclusive na chamada direta da C5; escalar é erro. A C5 filtra `Empresa.pk`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:25`, `app/chamados/management/commands/disparar_perguntas_competencia.py:52`, `app/chamados/management/commands/disparar_perguntas_competencia.py:104`, `app/escala/services/cadastro_realidade.py:177`.) |
   | Banco 1 | Fora da lista de juízes; parser sem `--apply`; cron em alarme. (`app/config/crons.py:2044`, `app/chamados/management/commands/disparar_perguntas_competencia.py:132`, `app/config/crons.py:1521`.) |
   | Bancos 2/3 | Mesma função fornece os dois números/esperados ao carimbo; placar lê ambos e compara. Totais C5 ficam informativos. (`app/chamados/management/commands/vigiar_fio_morto.py:181`, `app/chamados/management/commands/vigiar_fio_morto.py:233`, `app/chamados/management/commands/vigiar_fio_morto.py:242`, `app/core/management/commands/pipeline_placar.py:135`.) |
   | Banco 4 | Lista real vazia, mordida sintética com isolamento. Executei o selo: **OK, rc 0, juízes por varredura=0**. (`bin/tests/test_papel_prazo_nao_deriva.sh:305`, `bin/tests/test_papel_prazo_nao_deriva.sh:384`, `bin/tests/test_papel_prazo_nao_deriva.sh:406`.) |

6. **Escritores e leitores relevantes aos campos do diff:**

   | Campo/valor | Quem escreve / quem lê |
   |---|---|
   | `veredito` | Porta `lavrar_veredito` escreve; fábrica, repasse, resposta tardia e C5 leem. (`app/ponto/portas/celula.py:1035`, `app/chamados/services/fabrica_da_celula.py:89`, `app/chamados/services/fabrica_da_celula.py:120`, `app/chamados/services/disputa_emissao.py:758`, `app/chamados/services/resposta_tardia.py:45`, `app/escala/services/cadastro_realidade.py:181`.) |
   | `ata` | Lavra escreve a ata; `carimbar_precedencia` possui sua chave. Fábrica lê pelo predicado parcial e por `_ata_do_dia`. (`app/ponto/portas/celula.py:1033`, `app/ponto/portas/celula.py:1084`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/fabrica_da_celula.py:64`.) |
   | `julgada_em` | `carimbar` escreve. Cartório captura o anterior e o entrega à fábrica; fora do ato, fábrica usa o persistido. (`app/ponto/portas/celula.py:1065`, `app/ponto/services/cartorio.py:954`, `app/ponto/services/cartorio.py:1008`, `app/chamados/services/fabrica_da_celula.py:124`.) |
   | Linha `ALVOS` | Disparar imprime motivos devidos, não promessa de perguntas emitidas; admite encaminhamento ao DP. (`app/chamados/management/commands/disparar_perguntas_competencia.py:215`.) |
   | `fio_mudo_alvos` | Contador produz; vigiar persiste; pipeline e leitor genérico do ESTADO comparam número/esperado. (`app/chamados/management/commands/disparar_perguntas_competencia.py:122`, `app/chamados/management/commands/vigiar_fio_morto.py:241`, `app/core/management/commands/pipeline_placar.py:98`, `bin/fio_estado.py:125`.) |
   | `nunca_bateu_sem_adesao` | Contador produz pares/interseção; vigiar publica número, esperado e detalhes; pipeline/ESTADO comparam. (`app/chamados/management/commands/disparar_perguntas_competencia.py:66`, `app/chamados/management/commands/vigiar_fio_morto.py:234`, `app/chamados/management/commands/vigiar_fio_morto.py:242`, `app/core/management/commands/pipeline_placar.py:136`, `bin/fio_estado.py:125`.) |
   | `rc` | Disparar: 2 somente com alarme e pendência da célula. Pipeline: 2 com alarme e divergência. Corrente registra erro e continua os elos. (`app/chamados/management/commands/disparar_perguntas_competencia.py:219`, `app/core/management/commands/pipeline_placar.py:159`, `bin/encadeado.sh:64`.) |
   | ESTADO | Vigiar grava o carimbo; gerador chama `fio_estado`, que inclui divergências genéricas no documento final. (`app/chamados/management/commands/vigiar_fio_morto.py:135`, `bin/gerar_estado.py:260`, `bin/fio_estado.py:191`, `bin/gerar_estado.py:390`.) |

   **Não localizei raspador produtivo do texto antigo de ALVOS na cópia.** O raspador identificado usa `FABRICA-POR-EVENTO: o julgamento deixou N`, preservado na saída. Leitores externos e logs reais: [nao verificado]. (`bin/placar_code.sh:390`, `app/chamados/management/commands/disparar_perguntas_competencia.py:209`.)

   `nunca_bateu` continua acusador para outros contratos; a obra altera seu destino de emissão, não a tupla geral. A causa `celula_nunca_bateu` e o contador de pares têm nomes/unidades separados. (`app/escala/models.py:1767`, `app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/pergunta_no_app.py:35`, `app/chamados/management/commands/disparar_perguntas_competencia.py:66`.)

7. **Os mutantes mordem com controle verde e sem passagem vazia.** O executor exige uma única ocorrência da âncora, executa o controle e depois exige `AssertionError` com a mensagem pretendida. As âncoras dos três mutantes pedidos existem na fábrica atual. (`app/chamados/tests/test_e4a2_matriz_memoria.py:61`, `app/chamados/tests/test_e4a2_matriz_memoria.py:372`, `app/chamados/services/fabrica_da_celula.py:120`.)

   Executei matriz+C5 em memória: **29 testes OK, rc 0**, cobrindo os catálogos de 19 e 13 mutantes. Também foram executados avulsos `juiz_reintroduz_forma_b`, `juiz_libera_concorde_em_curso` e `juiz_libera_concorde_noturno`: um teste OK/rc 0 cada, significando controle verde e mutante rejeitado. (`app/chamados/tests/test_e4a2_matriz_memoria.py:85`, `app/chamados/tests/test_e4a2_matriz_memoria.py:365`, `app/chamados/tests/test_e4a2_c5_memoria.py:68`, `app/chamados/tests/test_e4a2_c5_memoria.py:204`.)

   O juiz nesses casos é a função real por AST, envolvida por `Mock(wraps=...)`; ORM, ata e outras bordas são dublados. O `dia_encerrado=False` é sentinela, e o caminho correto deve nunca chamá-lo. (`app/chamados/tests/test_e4a2_matriz_memoria.py:143`, `app/chamados/tests/test_e4a2_matriz_memoria.py:154`, `app/chamados/tests/test_e4a2_matriz_memoria.py:160`, `app/chamados/tests/test_e4a2_matriz_memoria.py:188`.)

8. **Classes de reprovação:**

   - **Segundo juiz:** não encontrei no cont

[truncado em 20000 de 22595 caracteres]

## 10/11 11:56 · sessao b29a0f87403e · - · - · origem=Ronald (tmux)

Concordo: regularização vazia fica em **(b), “aceite nao mensuravel sem escrita”**. A porta existe, mas `pode_fechar()` mede apenas a disputa; concluir o chamado depende de `reconciliar()` no signal (`app/templates/chamados/partials/modal_fio.html:352`; `app/chamados/models.py:1848`; `app/chamados/signals.py:42`).

Corrijo minha sugestão anterior: `reconciliaria()` chega a `trava_na_ordem()` dentro do `atomic`, que executa `SELECT FOR UPDATE`; não deve ser chamada nesse envelope (`app/chamados/models.py:661`; `app/chamados/models.py:672`; `app/chamados/models.py:707`; `app/chamados/models.py:711`; `app/chamados/models.py:2666`).

## 10/11 11:53 · sessao b29a0f87403e · - · - · origem=Ronald (tmux)

Complemento: há uma alternativa mensurável para disputa vazia que a transição genérica recusa: regularizar pelo modal, sem materialização pendente, usando `DisputaSupervisao.pode_fechar()` e `ChamadoColaborador.reconciliaria()`; o botão aparece porque `todas_respondidas=True` quando não há perguntas (`app/chamados/juizes.py:2308`; `app/chamados/services/fio.py:339`; `app/templates/chamados/partials/modal_fio.html:325`; `app/chamados/services/materializacao.py:1369`; `app/chamados/services/materializacao.py:1378`; `app/chamados/models.py:1848`; `app/chamados/models.py:657`).

O item precisa ser o chamado-pai da disputa: o signal só reconcilia esse pai (`app/chamados/signals.py:28`; `app/chamados/signals.py:32`; `app/chamados/signals.py:42`).

As demais alternativas inspecionadas dependem de escrita para demonstrar o fechamento:

| Alternativa | Evidência / tratamento |
|---|---|
| Ausência: Abrir registro → Aprovar/Rejeitar | Link no detalhe; aprovação/rejeição decide a ausência e o evento depois pede `ch.reconciliar()`. `gate_aprovacao_ausencia` sozinho não prova conclusão do chamado; usar motivo “aceite nao mensuravel sem escrita” se genéricas recusarem (`app/templates/chamados/detalhe_local.html:233`; `app/templates/ponto/partials/painel_ausencia.html:309`; `app/templates/ponto/partials/painel_ausencia.html:330`; `app/ponto/services/ausencia.py:616`; `app/ponto/services/ausencia.py:686`; `app/ponto/services/ausencia.py:911`) |
| Aceitar folga limbo | Grava template antes de reconciliar chamados; guardas iniciais não provam fechamento (`app/templates/chamados/partials/modal_fio.html:543`; `app/chamados/services/limbo_folgas.py:81`; `app/chamados/services/limbo_folgas.py:91`; `app/chamados/services/limbo_folgas.py:101`) |
| Aprovar exceção | Registra batida antes de transicionar chamado (`app/templates/chamados/detalhe_local.html:244`; `app/chamados/services/acoes_chamado.py:44`; `app/chamados/services/acoes_chamado.py:70`) |
| Aplicar declaração textual | Cria/reusa pergunta, responde e chama validação escritora (`app/chamados/services/declaracao_texto.py:641`; `app/chamados/services/declaracao_texto.py:659`; `app/chamados/services/declaracao_texto.py:693`; `app/chamados/services/declaracao_texto.py:696`) |
| Corroborar régua | Grava defesa e ocorrência; não encerra chamado diretamente (`app/escala/regua_defesa.py:647`; `app/escala/regua_defesa.py:662`; `app/escala/regua_defesa.py:670`) |
| Realocar régua | `julgar_vinculo` é leitura, mas execução e efeito sobre chamados dependem da escrita (`app/escala/regua_defesa.py:316`; `app/escala/regua_defesa.py:321`; `app/escala/regua_defesa.py:326`) |
| Benefício | Quando vinculado a chamado, tenta primeiro o mesmo `transicionar('resolvido'/'fechado')`; não contorna as recusas genéricas (`app/colaboradores/views.py:2066`; `app/colaboradores/views.py:2068`) |

`ir_para_ausencia` não deve ser chamada pela sonda: até o GET escreve ciência e ocorrência (`app/chamados/views.py:1513`; `app/chamados/views.py:1516`).

## 10/11 11:53 · sessao bab3a66b07ee · - · - · origem=Ronald (tmux)

Itens 2 e 6 — leitura da cópia:

- **Sim, a obra inteira altera quem recebe pergunta em produção.** `nunca_bateu` agora retorna sem perguntar em `app/chamados/services/fabrica_da_celula.py:105` e `:183`; isso alcança cartório (`app/ponto/services/cartorio.py:1008`) e consumidor (`app/chamados/reconciliador.py:1386` → `app/chamados/services/fabrica_da_celula.py:301`). O caminho segue silenciando push em `app/chamados/services/fabrica_da_celula.py:218`.
- **Repasse também muda:** `app/chamados/services/disputa_emissao.py:757` devolve causa para `nunca_bateu`; `:796` a transforma em “não regularizável”; filtros `:1066` e `:1088` o retiram antes do pai (`:1104`), perguntas (`:837`, `:1213`, `:1244`) e push (`:1289`). O catálogo apenas nomeia a retenção/admin (`app/chamados/services/pergunta_no_app.py:35`), lida por `:99`.
- **Disparar deixa de escrever:** contador apenas seleciona/deduplica (`app/chamados/management/commands/disparar_perguntas_competencia.py:94`–`:124`), comando apenas imprime e eventualmente sai 2 (`:189`–`:223`); cron passa `--alarme` (`app/config/crons.py:1521`). A prova da remoção do escritor é o diff fornecido; execução real em produção [nao verificado].
- **Vigiar grava arquivo, sem escrever perguntas/chamados:** chama contador em `app/chamados/management/commands/vigiar_fio_morto.py:181`; publica números/esperados em `:233` e `:241`; `_carimbar` grava JSON e faz replace em `:135`–`:137`. Só frota carimba (`:227`); recorte declara ausência de carimbo (`:249`).
- **Resposta tardia:** alteração do diff é apenas quebra de linha; comportamento permanece consultando renascimento (`app/chamados/services/resposta_tardia.py:45`–`:48`), podendo gravar nota/log/pauta (`:53`, `:55`, `:63`). Não foi adicionado corte de `nunca_bateu` nessa porta (`:45`).
- **Placar:** lê arquivo e compara dados, sem derivar alvos (`app/core/management/commands/pipeline_placar.py:92`–`:106`); consulta ambos contadores (`:135`–`:136`) e sai 2 com chave (`:158`–`:161`).

Mapa dos campos:

| Campo | Escritor | Leitor relevante |
|---|---|---|
| `veredito` | `app/ponto/portas/celula.py:1035`, persistência `:1040`; chamada delegada `app/escala/models.py:1826` | fábrica `:89`, `:120`; repasse `app/chamados/services/disputa_emissao.py:758`; resposta tardia `:45`; C5 `app/escala/services/cadastro_realidade.py:181` |
| `ata` | lavra `app/ponto/portas/celula.py:1033`; chave precedência `:1084`–`:1087` | fábrica `app/chamados/services/fabrica_da_celula.py:123`; motivos por `_ata_do_dia` `:64`, leitor `app/chamados/services/disputa_emissao.py:895` |
| `julgada_em` | `app/ponto/portas/celula.py:1065`, persistência `:1072` | cartório captura anterior `app/ponto/services/cartorio.py:954`, repassa `:1008`; fábrica usa `app/chamados/services/fabrica_da_celula.py:124` |
| `ALVOS` | stdout `app/chamados/management/commands/disparar_perguntas_competencia.py:215` | raspador antigo em produção externa [nao verificado]; não encontrei leitor do texto antigo na cópia |
| `fio_mudo_alvos` | retorno contador `app/chamados/management/commands/disparar_perguntas_competencia.py:122`; JSON `app/chamados/management/commands/vigiar_fio_morto.py:241` | placar `app/core/management/commands/pipeline_placar.py:85`, `:135`; ESTADO genérico abaixo |
| `nunca_bateu_sem_adesao` | pares/interseção C5 `app/chamados/management/commands/disparar_perguntas_competencia.py:66`–`:72`; JSON `app/chamados/management/commands/vigiar_fio_morto.py:234`, `:241`–`:242` | placar `app/core/management/commands/pipeline_placar.py:136`; ESTADO genérico |
| `rc` | disparar `:219`–`:223`; placar `:158`–`:161` | corrente captura e registra, continua para próximo elo: `bin/encadeado.sh:64`–`:68` |
| ESTADO | carimbo acima; publicação final `bin/gerar_estado.py:390` | `bin/gerar_estado.py:260`–`:261` chama `bin/fio_estado.py`; este compara **todas** as chaves `esperado`/`censo` (`:125`–`:126`) e imprime divergências (`:191`–`:194`) |

- **Raspagem preservada:** `bin/placar_code.sh:390` raspa exclusivamente prefixo `FABRICA-POR-EVENTO: o julgamento deixou N`; saída permanece em `app/chamados/management/commands/disparar_perguntas_competencia.py:209`. Não achei raspador antigo `ALVOS: N perguntas`, `[EMITE] FIO MUDO`, `FIO DECIDIDO` ou `PARCIAL NA LISTA DO ADMIN` em código de produção da cópia; leitores externos/logs de produção [nao verificado].
- **Significados separados:** `nunca_bateu` continua em `VEREDITOS_ACUSAM` (`app/escala/models.py:1767`), mas seu destino de pergunta é explicitamente cortado pela fábrica (`app/chamados/services/fabrica_da_celula.py:105`) e repasse (`app/chamados/services/disputa_emissao.py:758`); causa `celula_nunca_bateu` (`app/chamados/services/pergunta_no_app.py:35`) não se confunde com contador de pares (`app/chamados/management/commands/disparar_perguntas_competencia.py:66`).

## 10/11 11:53 · sessao 03582047d95e · - · - · origem=Ronald (tmux)

Entrega bloqueada: o caminho autorizado `/home/ronald/wt-codex-fio-e0-v4-e4b-114449/app` desapareceu durante o trabalho. A persistência dos arquivos está [nao verificado].

Os casos a–e foram escritos e o RED inicial acusou módulo ausente; a gravação da implementação falhou depois. Cura, mutantes, Ruff e suite ORM não foram concluídos. [nao verificado] [nao verificado]

Restaure essa mesma cópia para eu continuar dentro da cerca autorizada.

## 10/11 11:53 · sessao 48f77b56afe0 · - · - · origem=Ronald (tmux)

Item 4 — inventário das asserções preexistentes alteradas no `diff_obra_v9f` fornecido; os valores anteriores vêm do anexo, sem comparação Git independente [nao verificado].

| Arquivo/linhas atuais | Alteração versus main informado |
|---|---|
| `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:48`, `:49`, `:54` | Rótulo `[EMITE]`→`VIGIA`; ALVOS recebe descrição nova, preservando 1 motivo/1 pessoa e 2/2. Acrescenta veto a escrita em `:41`. |
| `app/chamados/tests/test_fabrica_pela_ata.py:51`, `:64`, `:71` | Só texto de ALVOS, conservando zero. Adiciona igualdade de contagem antes/depois em `:41`; renomeia caso positivo sem mudar sua asserção 1 (`:44`, `:47`). |
| `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:140` | Rótulo mais explícito, mesmo FIO MUDO 1. |
| Mesmo, `:141` | Número/sentido: perguntas 1→0; vigia deve conservar estoque. |
| Mesmo, `:143` | rc continua 0; mensagem passa a dizer que alarme é do placar. |
| Mesmo, `:151`, `:152` | Primeira pergunta agora nasce explicitamente pelo evento; quantidade 1 mantida, acrescida precondição positiva. |
| Mesmo, `:158`, `:159`, `:160` | Retratada: perguntas 2→1; pendência da célula 0→1; rc 0→2. A fixture passa de concorde a acusadora. |
| Mesmo, `:168` | Limbo: perguntas 1→0; FIO MUDO1, pendência1 e rc2 permanecem (`:166`, `:167`, `:169`). |
| Mesmo, `:177` | Remove exigência diagnóstica `PARCIAL NA LISTA DO ADMIN 1`; substitui por total `VIGIA FIO MUDO: 0`. Não prova mais o número do motivo de bloqueio; pergunta0/pendência0/rc0 permanecem (`:175`, `:178`, `:179`). |
| `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:26`, `:27` | Sentido: cron deve ter `--alarme`, passa a proibir `--apply`; não é mudança cosmética. |
| `app/chamados/tests/test_o261_fabrica_le_a_lampada.py:103`, `:105` | Para nunca_bateu, `assertIsNone` vira igualdade com causa `celula_nunca_bateu`; demais acusadores conservam `None`. |
| `app/chamados/tests/test_o261_vigia_fio.py:815`, `:825` | Igualdades completas de censo/esperado recebem as duas novas chaves. Sem redução para igualdade parcial ou truthiness. |
| `app/chamados/tests/test_pergunta_do_dia_em_curso.py:106` | ALVOS continua zero; texto novo. |
| Mesmo, `:424` | `assertNotIn` da linha zero vira regex exigindo linha existente com número estritamente positivo: reforço. Acrescenta mesma regex no outro cenário (`:429`). |
| Mesmo, `:476`, `:477` | Guarda preserva FIO MUDO1; igualdade de perguntas `['intervalo_volta']` é substituída por ausência de perguntas. Mudança de contrato escritor→vigia, não só texto. Acrescenta prova direta do juiz `['intervalo_volta']` em `:475`. |
| `app/chamados/tests/test_selo_emissor_fio_mudo.py:84`, `:119`, `:173` | Texto ALVOS novo, número1 preservado. |
| Mesmo, `:91` | `[EMITE] FIO MUDO0`→`VIGIA FIO MUDO0`; remove `desligado 1`. Perda explícita da cobertura do diagnóstico do motivo. |
| Mesmo, `:133` | Mesmo zero, rótulo VIGIA. |
| Mesmo, `:147` | Mesmo zero, rótulo VIGIA; remove `FIO DECIDIDO 1`. Perda explícita da cobertura desse diagnóstico. |
| Mesmo, `:164` | Apenas rótulo, mesmo1; método já era `_test_lapide...`, portanto já não era teste coletável (`:149`). | [nao verificado]
| Mesmo, `:188`, `:189` | Exige `--alarme`, proíbe `--apply`. |
| Mesmo, `:201`, `:202`, `:203`, `:204` | Troca exigência da mensagem dry-run por alvo1 e ausência de DRY/APLICADO; igualdade de estoque preservada. | [nao verificado]
| Mesmo, `:270` | `assertTrue(vivas)`→`assertFalse(vivas)`: inversão de sentido autorizada pela transformação em vigia; acrescenta igualdade do estoque e contadores/rc (`:271`–`:274`). |
| `app/core/tests/test_medida_turno_do_par.py:268`, `:540` | Registro de posição1196→1199; igualdade do censo continua, é ajuste de localização. |
| `app/escala/tests/test_termometro_regua_prazo.py:19`, `app/ponto/tests/test_selfie_ausente_juiz.py:148`, `app/ponto/tests/test_varredura_lote_prazo.py:62` | Número de juízes1→0. Último adiciona igualdade de papel vigia (`test_varredura_lote_prazo.py:63`). |
| `bin/tests/test_papel_prazo_nao_deriva.sh:305`, `:308`, `:384` | Lista real da exceção fica vazia; teste do cron nominal passa a testar exceção sintética com isolamento da real; acrescenta teste de não vazamento (`:389`). Mudança do objeto da prova, não remoção da mordida. |

Não vi no diff fornecido troca de `assertEqual` por `assertTrue`, encurtamento puro de `assertIn` para fazer caber saída incompatível, ou eliminação integral de caso coletável; há os três diagnósticos retirados acima, renomeações e inversões de contrato bem visíveis. Ausência integral de outros casos removidos depende da completude do diff fornecido [nao verificado].

Arquivos preexistentes modificados apenas em fixture/chamada, sem substituir asserção existente: `test_cobparc5_parcial_sem_periodo_so_lista.py:198`, `test_resposta_tardia.py:34`, `test_selo_disparo_nao_cobra_desligado.py:90`; foram acrescentadas novas asserções/testes em `test_fabrica_por_evento.py:84`, `test_pergunta_do_dia_em_curso.py:194`, `test_resposta_tardia.py:93`. A catraca COBPARC deixa de exigir leitura direta do antigo handle e exige delegação do contador ao juiz, incluindo delegação na própria verificação de obrigatórios (`test_cobparc5_parcial_sem_periodo_so_lista.py:324`, `:338`, `:420`, `:474`). [nao verificado]

Item 5 — realização estática e provas presentes:

- (a) A autoridade lê a lâmpada nunca_bateu, sem afirmar pertença C5 (`fabrica_da_celula.py:87`); juiz retorna vazio (`:105`) e escritor retorna antes de pauta/pergunta (`:183`). Contador separa retidos antes de alvos (`disparar_perguntas_competencia.py:111`), deduplica pares e compara com interseção da C5 (`:54`, `:65`, `:66`); desligados só entram nos pares realmente retidos por C5 (`:59`). A delimitação é declarada: chamados vivos do recorte ∩ piso anterior por empresa até ontem (`:43`, `:64`, `:71`).
- (b) Todos os motivos vêm de `motivos_a_perguntar(cel, hoje, agora)` (`disparar...py:115`), dedup por pessoa/dia/motivo (`:116`), esperado0 e donoESTRUTURA (`:122`); relatório retorna2 somente para pendência da célula e chave alarme (`:219`).
- (c) Pergunta viva exclui de ambos os contadores antes da separação de retidos (`disparar...py:109`). Integração da materialização está escrita em `test_e4a2_nunca_bateu.py:149`; não executei integração [nao verificado]. [nao verificado]
- (d) Universo filtra VIVOS (`disparar...py:99`); teste exige fechamento pela porta real da tranca antes de zerar ambos os contadores (`test_e4a2_nunca_bateu.py:164`). Execução [nao verificado]. [nao verificado]
- Errata unidade: fronteira normaliza iterável de empresas/PKs e rejeita escalar (`disparar...py:25`, `:30`, `:36`); aplica na entrada de ambos contadores (`:52`, `:88`), chama C5 com PKs (`:54`), ORM usa PKs (`:104`). C5 real filtra `Empresa.pk__in`, confirmando a unidade (`cadastro_realidade.py:177`).
- Banco1: parser só oferece ini/fim/comp-corrente/alarme/colab (`disparar...py:132`); retirada do cadastro de varredura documentada em `config/crons.py:2044`; selo específico verifica parser e cadastro (`test_e4a2_matriz_memoria.py:90`).
- Bancos2/3: vigia usa mesmo contador (`vigiar_fio_morto.py:181`), publica numero/esperado para ambos (`:233`, `:241`) e totais C5 informativos fora de censo/esperado (`:242`). Placar lê os dados e compara número≠esperado (`pipeline_placar.py:98`, `:105`), invocando os dois nomes (`:135`, `:136`).
- Banco4: exceções reais vazias (`bin/tests/test_papel_prazo_nao_deriva.sh:305`), sintética prova exceção nominal, não vazamento e não contaminação da real (`:384`–`:390`); resultado executado [nao verificado].
- Pelo delta9e+9f fornecido, as alterações produtivas são restauração da barreira não acusadora, enquanto retidos, fronteiras, carimbo, parser e retorno não foram alterados; não há regressão estática encontrada nos bancos1–4. A barreira atual rejeita concorde incondicionalmente (`fabrica_da_celula.py:120`), conforme errata. Execução geral/produção [nao verificado].

## 10/11 11:52 · sessao 0471b4770d31 · - · - · origem=Ronald (tmux)

3. **O 1 vem do juiz real, com insumos controlados pela fixture.** `_motivos_do_juiz` importa e chama `motivos_a_perguntar`, sem substituí-lo; exige exatamente `['intervalo_volta']` às 13:40 antes de conferir `FIO MUDO: 1`. O contador também chama esse juiz. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:454`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:475`, `app/chamados/management/commands/disparar_perguntas_competencia.py:117`.)

A fixture escolhe `furo` ou `concorde` por `update`, relê a célula, monta chamado aberto por `bulk_create` sem sinais e substitui `_ata_do_dia` por duas faltas prescritas. Portanto, prova a decisão do juiz sobre esses insumos; não prova que a lavra real produziria esse veredito/ata. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:52`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:442`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:456`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:462`.)

O `concorde -> 0` próprio exige `[]` diretamente do juiz antes do comando: a reintrodução de (B) deve falhar nessa primeira asserção, pois o turno das 08–18 ainda está em curso às 13:40 e há intervalo vencido na ata fornecida. A execução Django dessa mordida está [nao verificado]. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:17`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:479`, `app/chamados/services/fabrica_da_celula.py:139`.)

7. **Os três mutantes pedidos mordem com controle verde; não passam por alvo ausente.** Os alvos estão declarados e o executor exige uma única ocorrência, executa o controle antes da mutação e depois exige `AssertionError` com a mensagem pretendida. (`app/chamados/tests/test_e4a2_matriz_memoria.py:61`, `app/chamados/tests/test_e4a2_matriz_memoria.py:365`.)

Executei, com `PYTHONDONTWRITEBYTECODE=1`, a suíte de memória matriz+C5: **29 testes, OK, rc 0**; também cada mutante avulso: `juiz_reintroduz_forma_b`, `juiz_libera_concorde_em_curso`, `juiz_libera_concorde_noturno`: **um teste OK, rc 0 cada**. O mecanismo que dá significado a esse resultado está em `app/chamados/tests/test_e4a2_matriz_memoria.py:372`; saída persistida em arquivo [nao verificado].

Nesses casos, o juiz é a função real carregada por AST e envolvida por `Mock(wraps=...)`; `_ata_do_dia`, vitalidade, ORM, parcial e outras bordas são dublados. O `dia_encerrado=False` funciona como sentinela: o caminho correto deve nunca chamá-lo. (`app/chamados/tests/test_e4a2_matriz_memoria.py:143`, `app/chamados/tests/test_e4a2_matriz_memoria.py:154`, `app/chamados/tests/test_e4a2_matriz_memoria.py:160`, `app/chamados/tests/test_e4a2_matriz_memoria.py:188`.)

Limite relevante: `test_RED_selos_dos_catalogos_reais_sem_django` verifica o mecanismo de rejeição de catálogos vazios/encolhidos; não executa os cenários ORM dos catálogos de integração. Não confundir seu verde com integração Django verificada. (`app/chamados/tests/test_e4a2_matriz_memoria.py:321`, `app/chamados/tests/test_e4a2_matriz_memoria.py:340`.)

## 10/11 11:51 · sessao 68011ca7a87c · - · - · origem=Ronald (tmux)

A validação e a regularização podem concluir o chamado, mas o aceite completo depende de estado produzido pela própria escrita; os predicados individuais de leitura não provam esse desfecho (`app/chamados/services/validacao.py:435`; `app/chamados/services/validacao.py:463`; `app/chamados/services/materializacao.py:1233`; `app/chamados/services/materializacao.py:1277`).

| Porta real | Encadeamento | Parte somente leitura / limite |
|---|---|---|
| Validar pergunta | `acao_validar_pergunta` → `validar_pergunta` (`app/chamados/services/acoes_disputa.py:13`; `app/chamados/services/validacao.py:373`) | Seleção, resposta, carimbo anterior e implausibilidade são conferidos antes da escrita (`app/chamados/services/validacao.py:385`; `app/chamados/services/validacao.py:393`; `app/chamados/services/validacao.py:404`). O aceite final só aparece depois de carimbar, materializar e reler (`app/chamados/services/validacao.py:435`; `app/chamados/services/validacao.py:469`; `app/chamados/services/validacao.py:490`). |
| Validar inline | `validar_inline` → `acao_validar_pergunta` (`app/chamados/views_cobrar.py:145`; `app/chamados/views_cobrar.py:208`) | Resolve disputa aberta e perguntas vivas respondidas em leitura (`app/chamados/views_cobrar.py:160`; `app/chamados/views_cobrar.py:169`); herda a dependência de escrita da validação (`app/chamados/views_cobrar.py:208`). |
| Validar lote | `validar_lote_classe_a` → `acao_validar_pergunta` (`app/chamados/services/validacao.py:695`; `app/chamados/services/validacao.py:746`) | `classe_do_lote(perg)` é seleção real somente leitura, mas o resultado contado vem de `refresh_from_db` posterior à ação (`app/chamados/services/validacao.py:735`; `app/chamados/services/validacao.py:752`; `app/chamados/services/validacao.py:845`). |
| Regularizar disputa e fechar | Formulário condicionado a disputa aberta, sem `ancora_ausente` e com todas respondidas (`app/templates/chamados/partials/modal_fio.html:325`; `app/templates/chamados/partials/modal_fio.html:333`; `app/templates/chamados/partials/modal_fio.html:352`) → `acao_regularizar_disputa` (`app/chamados/services/acoes_disputa.py:118`) | Guarda de anuência usa `perguntas_vivas_de(d)` (`app/chamados/services/acoes_disputa.py:129`). Depois materializa; só então obtém `disputa_fechada` (`app/chamados/services/acoes_disputa.py:139`; `app/chamados/services/acoes_disputa.py:143`). |
| Aceitar geofence | Formulário `autorizar_geofence` (`app/templates/chamados/partials/modal_fio.html:505`) → `acao_autorizar_geofence` (`app/chamados/services/acoes_chamado.py:165`) | Recusa terminal em leitura; escreve nota antes de chamar `transicionar('resolvido')` (`app/chamados/services/acoes_chamado.py:176`; `app/chamados/services/acoes_chamado.py:218`; `app/chamados/services/acoes_chamado.py:222`). A medição segura deve usar somente o prefixo real de `transicionar`, sem executar essa ação. |
| Recusar geofence | Formulário `recusar_geofence` (`app/templates/chamados/partials/modal_fio.html:527`) → `acao_recusar_geofence` (`app/chamados/services/acoes_chamado.py:243`) | Recusa terminal, motivo e pertencimento da batida são lidos antes de `retratar_batida`; esta escreve antes da transição final (`app/chamados/services/acoes_chamado.py:252`; `app/chamados/services/acoes_chamado.py:254`; `app/chamados/services/acoes_chamado.py:262`; `app/chamados/services/acoes_chamado.py:269`; `app/chamados/services/acoes_chamado.py:277`). |

Para medir a porta genérica, a chamada real `ch.transicionar(destino, user)` pode ser interrompida por sentinela em `_aplicar`, executando as guardas originais sem entrar no escritor: `_aplicar` é o último chamado de `transicionar` e só encaminha a `_gravar_estado` (`app/chamados/models.py:199`; `app/chamados/models.py:447`). Os destinos oferecidos na tela são `resolvido` e `fechado` (`app/templates/chamados/partials/modal_fio.html:668`; `app/templates/chamados/partials/modal_fio.html:669`).

**É necessário experimentar ambos:** a guarda S122 da premissa do furo só barra `resolvido`; `fechado` tem a guarda de justificativa pendente e compartilha as guardas de disputa e perguntas ancoradas (`app/chamados/models.py:136`; `app/chamados/models.py:148`; `app/chamados/models.py:165`; `app/chamados/models.py:181`).

`estado_aplicacao_pergunta(disputa, p)` é uma leitura real, mas não prova conclusão: não cobre o gate `DATA_EM_DISPUTA`, a avaliação do lastro após plantar, o carimbo C6 nem retenção por irmãs sem consumo (`app/chamados/services/materializacao.py:425`; `app/chamados/services/materializacao.py:1192`; `app/chamados/services/materializacao.py:1233`; `app/chamados/services/materializacao.py:1277`; `app/chamados/services/materializacao.py:761`).

Quando ambas as transições genéricas forem recusadas e restarem somente validar/materializar/regularizar, o resultado seguro é coluna **(b)** com **`aceite nao mensuravel sem escrita`**, preservando também as recusas medidas das transições: essas ações só conhecem o aceite definitivo depois dos efeitos descritos acima (`app/chamados/services/validacao.py:490`; `app/chamados/services/materializacao.py:1376`; `app/chamados/services/materializacao.py:1378`).

## 10/11 11:51 · sessao b29a0f87403e · - · - · origem=Ronald (tmux)

A porta genérica existe para chamados locais não fechados, sem restrição por fonte: modal da Central oferece `resolvido` (“Encerrar”) e `fechado` (“Encerrar e arquivar”), condicionado a `pode_agir_map`; detalhe local oferece os mesmos destinos (`app/templates/chamados/partials/modal_fio.html:649`; `app/templates/chamados/partials/modal_fio.html:668`; `app/templates/chamados/detalhe_local.html:374`; `app/templates/chamados/detalhe_local.html:396`).

| Fontes | Porta / função real | Parte somente leitura |
|---|---|---|
| Todas as fontes pedidas, quando chamado local | Modal → `processar_post_fio` → `chamado.transicionar` (`app/chamados/services/fio.py:708`; `app/chamados/services/fio.py:721`) | Corpo real de `transicionar`, antes de `_aplicar` (`app/chamados/models.py:128`; `app/chamados/models.py:199`) |
| Mesmas fontes no detalhe local | `detalhe_chamado_local` → `acao_resposta_livre` → `transicionar` (`app/chamados/views.py:547`; `app/chamados/services/acoes_chamado.py:313`) | Mesmo corpo real (`app/chamados/models.py:128`) |
| `gps_fora_geofence` | Autorizar, autorizar com advertência, recusar (`app/templates/chamados/partials/modal_fio.html:480`; `app/templates/chamados/partials/modal_fio.html:505`; `app/templates/chamados/partials/modal_fio.html:527`) → funções de `acoes_chamado` (`app/chamados/services/fio.py:533`; `app/chamados/services/fio.py:546`) | Autorizar escreve ocorrência antes de chamar transição: não pode medir aceite completo dessa ação sem escrita; a porta genérica permanece mensurável (`app/chamados/services/acoes_chamado.py:217`; `app/chamados/services/acoes_chamado.py:222`) |
| Fontes com perguntas listadas | Validar pergunta no modal → `acao_validar_pergunta` → `validar_pergunta` (`app/templates/chamados/partials/modal_fio.html:278`; `app/chamados/services/fio.py:608`; `app/chamados/services/acoes_disputa.py:19`) | Pré-condições até `validacao.py:413` são leitura; aceite completo depende de materialização depois de carimbar a validação (`app/chamados/services/validacao.py:393`; `app/chamados/services/validacao.py:435`; `app/chamados/services/validacao.py:452`) |
| Fontes com disputa | Regularizar disputa (`app/templates/chamados/partials/modal_fio.html:352`) → materialização (`app/chamados/services/fio.py:646`; `app/chamados/services/fio.py:670`) | Verificação de perguntas sem resposta é leitura, mas sucesso depende do materializador escritor (`app/chamados/services/fio.py:659`; `app/chamados/services/fio.py:670`) |
| Chamado GLPI, independentemente da fonte | Modal só linka detalhe (`app/templates/chamados/partials/modal_fio.html:680`). Detalhe mostra dropdown de status (`app/templates/chamados/detalhe.html:130`) | A view que recebe esse formulário ignora `novo_status` e apenas envia followup ao GLPI: não há conclusão efetiva por esse formulário no código da cópia (`app/chamados/views.py:292`; `app/chamados/views.py:303`; `app/chamados/views.py:309`) |

Para medir a guarda completa da transição sem replicá-la, chamar `ch.transicionar(destino, user)` real e substituir **somente `_aplicar`** por sentinela `BaseException`: atingir essa fronteira significa que o corpo real de recusa passou; nenhum método escritor é executado (`app/chamados/models.py:128`; `app/chamados/models.py:199`; `app/chamados/models.py:447`).

Guardas executadas nesse corpo: matriz/perfil, justificativa pendente para `fechado`, disputa viva, perguntas vivas ancoradas, premissa de furo para `resolvido` (`app/chamados/models.py:132`; `app/chamados/models.py:136`; `app/chamados/models.py:153`; `app/chamados/models.py:166`; `app/chamados/models.py:181`). **Tentar os dois destinos**: `fechado` não executa a guarda de premissa restrita a `resolvido` (`app/chamados/models.py:181`).

A verificação de perguntas vivas apenas altera estado em memória e o restaura em `finally`; não grava (`app/chamados/models.py:349`; `app/chamados/models.py:355`; `app/chamados/models.py:370`). O núcleo escritor repete esse tripwire antes de escrever; a decisão já passou pela mesma leitura para o item vivo (`app/chamados/models.py:165`; `app/chamados/models.py:221`; `app/chamados/models.py:224`).

Permissão real: `pode_agir_chamado(user, ch)`; superuser é gestor geral e passa por esse gate, e seu perfil é `admin_ti` (`app/chamados/utils.py:47`; `app/chamados/utils.py:57`; `app/chamados/utils.py:68`; `app/colaboradores/mixins.py:163`).

## 10/11 11:50 · sessao 6ebdcd75cedc · - · - · origem=Ronald (tmux)

O juiz real está em `app/ponto/management/commands/e6_oraculo.py:53`:

`dono_da_divergencia(horas_do_dia, em_cadastro_x_realidade, folga_nao_definida=False, sem_vinculo_no_dia=False, sem_vinculo_em_dia_de_trabalho=False, batida_fora_de_turno=False, fase_deslocada=False)`.

Ele recebe os horários do dia e os insumos das autoridades existentes; retorna CADASTRO por folga não definida/sem vínculo, BATIDA por aberto com batida real ou quantidade ímpar, CADASTRO por fase deslocada/CXR, e ESTRUTURA no resíduo (`app/ponto/management/commands/e6_oraculo.py:98`).

A composição oficial, sem motor, é:

```python
from ponto.janelas import janela_atual
from ponto.management.commands.e6_oraculo import InsumosDoDono
from ponto.services.cxr_na_lista import cxr_lavrado

cxr_do = cxr_lavrado()
ini, fim = janela_atual(dia, colaborador.empresa)
insumos = InsumosDoDono(
    colaborador, ini, fim, HOJE,
    agora=AGORA, sem_motor=True,
    cxr=cxr_do(colaborador.pk, ini),
)
dono = insumos.dono(dia)
```

Essa é a própria receita da casa (`app/escala/services/cadastro_realidade.py:1017`; `app/escala/services/cadastro_realidade.py:1024`). Cachear por `(colaborador.pk, ini)` evita reconstruir a competência (`app/escala/services/cadastro_realidade.py:1018`).

`InsumosDoDono.dono(data)` monta cadastro, fase e batida fora de turno, e chama o juiz real (`app/ponto/management/commands/e6_oraculo.py:182`). O cadastro vem de `cadastro_do_dia_carregado(colaborador_id, data)` (`app/ponto/supra_juiz.py:166`). A CXR vem da lavra; `None` significa competência não coberta, enquanto conjunto vazio significa cobertura sem dias CXR (`app/ponto/services/cxr_na_lista.py:165`; `app/ponto/services/cxr_na_lista.py:182`).

Atenção: sem cobertura CXR, `sem_motor=True` pode devolver `None`; nunca transformar esse retorno em ESTRUTURA ou lista vazia (`app/ponto/management/commands/e6_oraculo.py:189`; `app/ponto/management/commands/e6_oraculo.py:193`).

O dia do chamado deve vir de `dia_do_fato(chamado, fortes=False)`, como na sonda2; retorna `date | None`, observando célula, contexto do catálogo/data_turno, batida, disputa e carimbos fracos (`app/chamados/juizes.py:87`; `app/chamados/juizes.py:98`; `app/chamados/juizes.py:129`; `app/chamados/juizes.py:138`). O balde **“sem vínculo” da sonda2 é dia ausente**, não equivale ao flag cadastral `sem_vinculo_no_dia`; este último vem da autoridade do vínculo do dia (`app/chamados/juizes.py:44`; `app/ponto/supra_juiz.py:172`; `app/ponto/supra_juiz.py:193`). Não chamar `InsumosDoDono` com dia `None`.

O envelope usa `ROLE=leitor`, monta código e sonda em leitura e define `PYTHONDONTWRITEBYTECODE=1` (`bin/sonda_leitura.sh:42`; `bin/sonda_leitura.sh:237`; `bin/sonda_leitura.sh:238`). Contudo, o próprio envelope escreve credencial/temporário/trilha e reafirma role/GRANTs; a ausência de escrita deve ser afirmada especificamente sobre a **sonda Python**, sem prometer que o shell inteiro não escreve (`bin/sonda_leitura.sh:126`; `bin/sonda_leitura.sh:140`; `bin/sonda_leitura.sh:157`; `bin/sonda_leitura.sh:216`; `bin/sonda_leitura.sh:248`).

## 10/11 11:47 · sessao f8b606e0ca44 · - · - · origem=Ronald (tmux)

Exploração somente leitura concluída; nenhuma escrita ou acesso a banco/rede/log executado. [nao verificado]

- **Autoridade exportada:** `competencia_do_dia_entregue` compõe `ponto.janelas.janela_atual` e `folha.export.competencia_entregue` (K6); `None` significa dia indeterminado. Reconsultar sem memo imediatamente antes de operar. `chamados/services/vigia_fio.py:822`
- **Competência real:** `competencia_do_fato(colaborador, dia)` delega à autoridade de competências do intervalo; usar para o rótulo empresa/competência, sem mês civil. `ponto/services/fechamento.py:1602`
- **Inventário:** `censo_alcance(..., limite=sys.maxsize)` mantém todos os dias. `limite=0` ainda inclui todo `sem_porta`, mas omite dias com porta; não serve ao inventário integral. `chamados/services/vigia_fio.py:627`
- **Classes reais:** a=âncora/chamado vivo fora da fila; b=respondida sem texto; c=respondida sob terminal; d=colaborador na lista, dia ausente; e=demais, inclusive sem célula. Não atribuir CADASTRO à classe d. `chamados/services/vigia_fio.py:667`
- **História:** chamar `DecisaoPergunta.linhas_da_projecao_sem_linha` e então `registrar`, sob trava, preserva correspondência pergunta+ato+instante. `registrar` não deduplica sozinho. `chamados/models.py:3907`
- **Travas:** usar `PerguntaDisputa.trava_na_ordem` antes da primeira escrita, com universo de disputas/chamados/perguntas atingido. Ordem C→D→Ch→P; múltiplos colaboradores devem ser travados previamente por PK. `chamados/models.py:2629`
- **Faltante:** `perguntar_pela_celula` atravessa teto, lista, dedup factual e fábrica; retorno vazio pode ser recusa legítima, e não autoriza alterar filtros para zerar. `chamados/services/fabrica_da_celula.py:162`
- **Terminal/âncora errada:** `reemitir_chamado_do_dia(p,colab,dia)` reúne os gates canônicos e termina pela porta de costura; pode recusar sem escrever. `chamados/reconciliador.py:761`
- **Órfã:** a porta real nesta cópia é `reatar_pergunta_orfa`, com derivação, histórico único, lavra ou reemissão. `chamados/reconciliador.py:1533`
- **Carimbo:** `desfazer_carimbo_sem_lastro` relê travado, consulta C6, preserva R/V e registra evento somente quando efetivamente desfaz. `chamados/services/relavra_via.py:187`
- **Batida/Ausência:** `_materializar_uma` é unidade canônica; não consulta exportada. Ausência passa por `_declarar_ausencia_de_pergunta`→`criar_ausencia(se_existir='retornar_mesmo_tipo')`. `chamados/services/materializacao.py:1158`, `chamados/services/materializacao.py:462`
- **Decisão não é efeito:** tarefa autoriza MATERIALIZAR após DP VALIDACAO e lavra vigente negativa; lavra ausente resulta em desconhecimento. `chamados/services/tarefa_do_dia.py:140`
- **Sem invenção:** classe b não permite reconstruir texto inexistente; classe a não prova véu temporário e não autoriza levantar silêncio automaticamente. `chamados/services/vigia_fio.py:666`, `chamados/services/tarefa_do_dia.py:132`
- **Precedente inadequado:** `passivo_validado.reparar` declara DRY que não chama a unidade; não reutilizá-lo como implementação de C3.5. `chamados/services/passivo_validado.py:359`

## 10/11 11:44 · sessao 808a5b13142c · URGENCIA-E4B · sonda · origem=Code

**Entre os valores informados, há empate: badge lavrado e Validar+Decidir valem 1.224, ambos a 9 de 1.233.** A identidade dos conjuntos permanece **[nao verificado]**: a saída declara que a lavra não conserva IDs. O código lê o badge armazenado e define a soma corrente separadamente (`app/core/context_processors.py:84`; `app/chamados/services/verbos.py:25`).

**(1) Contadores no retrato fornecido de 10/11/2026**

Os números abaixo vêm de `NAVBAR_EXIBIDA` e `ATENDIMENTO_CONTEXTO_REAL`; as referências indicam os critérios no código, não uma nova medição.

| Contador | Valor | Distância de 1.233 | Referência |
|---|---:|---:|---|
| Badge lavrado, às 11:35:43 | **1.224** | **9** | `app/core/context_processors.py:84` |
| Central — total da lista | 3.616 | 2.383 | `app/chamados/services/painel.py:333` |
| Central — controle de ativos/cobrança | 3.601 | 2.368 | `app/chamados/services/painel.py:222` |
| Validar | 543 | 690 | `app/chamados/services/verbos.py:30` |
| Cobrar | 2.355 | 1.122 | `app/chamados/services/verbos.py:30` |
| Decidir | 681 | 552 | `app/chamados/services/verbos.py:30` |
| **Validar + Decidir corrente** | **1.224** | **9** | `app/chamados/services/verbos.py:25` |
| Justificativas pendentes | **[nao verificado]** | **[nao verificado]** | Passo de medição ausente; critério em `app/ponto/views.py:1736` |

O empate é entre os contadores **conhecidos**; a ausência da medição de Justificativas impede afirmar o vencedor entre todos os candidatos (`app/ponto/views.py:1736`; **[nao verificado]**, passo de Justificativas).

**(2) Partição por classe/verbo e dono**

**Badge lavrado:** partição por classe, verbo, atribuído, pilha, departamento e empresas 2/3/4 **[nao verificado]**, passo `NAVBAR_EXIBIDA/decomposicao_da_lavra`. O contexto gravado é agregado, sem mapa de IDs (`app/chamados/services/painel.py:284`).

**Validar+Decidir corrente:** a tabela fecha pelo filtro desses verbos nos blocos completos `QUEBRA_CLASSE_DONO/lista_empresa_None` e `lista_empresa_2`, apesar do truncamento posterior. Fonte de **todas as células**: saída desses blocos, referências declaradas `app/chamados/services/painel.py:198`; `app/chamados/services/verbos.py:30`; `app/chamados/utils.py:32`.

| Classe/origem | Departamento | Validar global | Decidir global | Validar empresa 2 | Decidir empresa 2 |
|---|---|---:|---:|---:|---:|
| acesso_bloqueado | ti | 0 | 3 | 0 | 2 |
| agenda_dia | supervisao | 0 | 2 | 0 | 2 |
| ausencia_colab | dp | 0 | 26 | 0 | 18 |
| batida_ausente | supervisao | 303 | 228 | 237 | 162 |
| batida_fora_horario | supervisao | 0 | 1 | 0 | 0 |
| chamados | dp | 0 | 32 | 0 | 21 |
| chamados | hasner | 0 | 10 | 0 | 8 |
| chamados | rh | 0 | 5 | 0 | 3 |
| chamados | supervisao | 0 | 105 | 0 | 79 |
| chamados | ti | 0 | 3 | 0 | 1 |
| cluster_batida_espuria | supervisao | 0 | 1 | 0 | 1 |
| colaborador_em_ferias | dp | 0 | 2 | 0 | 1 |
| colaborador_sem_posto | supervisao | 0 | 2 | 0 | 2 |
| disputa_supervisao | supervisao | 74 | 88 | 58 | 72 |
| disputa_supervisao_manual | supervisao | 16 | 6 | 12 | 4 |
| geofence | supervisao | 0 | 10 | 0 | 7 |
| gps_fora_geofence | supervisao | 123 | 0 | 81 | 0 |
| par_relampago | supervisao | 0 | 23 | 0 | 19 |
| ping_geo_fora | supervisao | 0 | 1 | 0 | 0 |
| sem_previsao_escala | supervisao | 3 | 69 | 1 | 57 |
| turno_aberto | supervisao | 24 | 55 | 15 | 38 |
| turno_aberto_24h | supervisao | 0 | 9 | 0 | 6 |
| **Total** | | **543** | **681** | **404** | **503** |
| **Validar+Decidir** | | **1.224** | | **907** | |

A partição por dono fecha assim; todas as linhas têm `atribuido_a_pk=null` nos mesmos blocos fornecidos. Fonte de toda a tabela: `app/chamados/services/painel.py:198`; `app/chamados/utils.py:32`.

| Verbo | Pilha | Departamento | Global | Empresa 2 |
|---|---|---|---:|---:|
| validar | admin_validar | supervisao | 543 | 404 |
| decidir | sem_pergunta | supervisao | 600 | 449 |
| decidir | sem_pergunta | dp | 60 | 40 |
| decidir | sem_pergunta | hasner | 10 | 8 |
| decidir | sem_pergunta | rh | 5 | 3 |
| decidir | sem_pergunta | ti | 6 | 3 |
| **Total** | | | **1.224** | **907** |

Empresas **3 e 4 individualmente: [nao verificado]**, passos `ATENDIMENTO_CONTEXTO_REAL`, `TOTAL_POPULACAO` e `QUEBRA_CLASSE_DONO` dessas empresas ausentes do trecho. A diferença global−empresa 2 não identifica sua distribuição entre elas **[nao verificado]**.

**(3) Censo `sem_porta` e cruzamento**

O recorte considera a competência corrente e a anterior ainda não entregue; a competência segue a janela da empresa. Portanto, o zero entregue abaixo vale para esse alcance, sem medir todo o histórico entregue (`app/ponto/services/he_pendente_lavrado.py:209`; `app/chamados/services/vigia_fio.py:838`).

Fonte de todas as contagens: `UNIVERSO_E4B` e `E4B_EXPORTACAO` fornecidos (`app/chamados/services/vigia_fio.py:628`; `app/chamados/services/vigia_fio.py:822`). Rótulos das classes: `app/chamados/services/vigia_fio.py:538`.

| Classe E4B | Rótulo | Corrente 11/2026, não entregue | Anterior 10/2026, não entregue | Entregue/exportada | Total |
|---|---|---:|---:|---:|---:|
| a | chamado_vivo_com_veu | 2 | 0 | 0 | 2 |
| b | respondida_sem_resposta | 0 | 0 | 0 | 0 |
| c | respondida_destino_fechado | 4 | 12 | 0 | 16 |
| d | colab_na_lista_cxr_dia_fora | 156 | 101 | 0 | 257 |
| e | outra | 47 | 5 | 0 | 52 |
| **Total** | | **209** | **118** | **0** | **327** |

Os cruzamentos confirmados são todos da competência **11/2026, não entregue**; suas parcelas anterior e entregue são zero. Fonte de todas as células: `TOTAL_POPULACAO`, `CENTRAL_DENTRO_E4B` e `QUEBRA_CLASSE_DONO` dos respectivos recortes (`app/chamados/services/painel.py:198`; `app/chamados/services/verbos.py:30`; `app/chamados/services/vigia_fio.py:542`).

| População | Total | Dentro E4B: a/b/c/d/e | Dentro E4B total | Fora do universo atual E4B | Vínculo indeterminado | Fora empresas 2/3/4 |
|---|---:|---|---:|---:|---:|---:|
| Central — lista global | 3.616 | 0 / 0 / 1 / 1 / 0 | **2** | 3.422 | 191 | 1 |
| Central — cobrança global | 3.601 | 0 / 0 / 1 / 1 / 0 | **2** | 3.408 | 190 | 1 |
| **Validar+Decidir global** | **1.224** | **0 / 0 / 1 / 0 / 0** | **1** | **1.044** | **179** | **0** |
| Central — lista empresa 2 | 2.808 | 0 / 0 / 1 / 1 / 0 | **2** | 2.668 | 138 | 0 |
| Central — cobrança empresa 2 | 2.795 | 0 / 0 / 1 / 1 / 0 | **2** | 2.655 | 138 | 0 |
| **Validar+Decidir empresa 2** | **907** | **0 / 0 / 1 / 0 / 0** | **1** | **777** | **129** | **0** |

O item de **Validar+Decidir** dentro do E4B é `disputa_supervisao`, verbo `validar`, pilha `admin_validar`, departamento `supervisao`, atribuído nulo, classe E4B **c**, empresa **2**, competência **11/2026**, não exportada. O outro item da lista está em `cobrar`, classe E4B **d** (`QUEBRA_CLASSE_DONO/lista_empresa_2`, fonte `app/chamados/services/painel.py:198`; `app/chamados/services/vigia_fio.py:542`).

| Fechamento restante | Resultado / passo |
|---|---|
| Badge lavrado × E4B, por classe/competência/empresa | **[nao verificado]** — `NAVBAR_EXIBIDA/decomposicao_da_lavra`; igualdade numérica não recupera IDs |
| Validar+Decidir empresas 3 e 4 × E4B | **[nao verificado]** — recortes individuais ausentes |
| Continuação da sonda / eventual cadeia `ERRO_SONDA` | **[nao verificado]** — trecho termina em `QUEBRA_CLASSE_DONO`, `admin_atual_empresa_2`, classe `par_relampago`, campo `dono_pilha` |

## 10/11 11:42 · sessao 72ef5087b3f5 · - · - · origem=Ronald (tmux)

O badge e Validar+Decidir têm **1.224**, ambos a **9** de 1.233 no dado fornecido; Justificativas pendentes permanece **[nao verificado]**, pois seu passo não consta antes do truncamento. O código define a soma em `app/chamados/services/verbos.py:22-27`; Justificativas conta `status='pendente'` global em `app/ponto/views.py:1736`.

O badge é lido da lavra em `app/core/context_processors.py:68-84`. Seu escritor usa `so_placar=True` e grava `n_admin` e contexto agregado (`app/core/management/commands/lavrar_badge_navbar.py:39-41`, `:72-93`); esse contexto não conserva mapa/ids (`app/chamados/services/painel.py:284-299`). Portanto, igualdade numérica não prova igualdade de conjuntos; a decomposição e o cruzamento E4B **do badge lavrado** continuam **[nao verificado]**, passo `NAVBAR_EXIBIDA/decomposicao_da_lavra`.

A quebra **corrente** de Validar+Decidir pode ser fechada integralmente para global e empresa 2 filtrando as quebras completas `lista_empresa_None` e `lista_empresa_2`, apesar do truncamento posterior de `admin_atual_empresa_2`. Referências dos registros fornecidos: `app/chamados/services/painel.py:198`; `app/chamados/services/verbos.py:30`.

| Classe | Validar global | Decidir global | Validar empresa 2 | Decidir empresa 2 |
|---|---:|---:|---:|---:|
| acesso_bloqueado | 0 | 3 | 0 | 2 |
| agenda_dia | 0 | 2 | 0 | 2 |
| ausencia_colab | 0 | 26 | 0 | 18 |
| batida_ausente | 303 | 228 | 237 | 162 |
| batida_fora_horario | 0 | 1 | 0 | 0 |
| chamados | 0 | 155 | 0 | 112 |
| cluster_batida_espuria | 0 | 1 | 0 | 1 |
| colaborador_em_ferias | 0 | 2 | 0 | 1 |
| colaborador_sem_posto | 0 | 2 | 0 | 2 |
| disputa_supervisao | 74 | 88 | 58 | 72 |
| disputa_supervisao_manual | 16 | 6 | 12 | 4 |
| geofence | 0 | 10 | 0 | 7 |
| gps_fora_geofence | 123 | 0 | 81 | 0 |
| par_relampago | 0 | 23 | 0 | 19 |
| ping_geo_fora | 0 | 1 | 0 | 0 |
| sem_previsao_escala | 3 | 69 | 1 | 57 |
| turno_aberto | 24 | 55 | 15 | 38 |
| turno_aberto_24h | 0 | 9 | 0 | 6 |
| **Total** | **543** | **681** | **404** | **503** |

Dono corrente, derivado dos mesmos registros com aquelas referências:

| Verbo | Pilha | Departamento | Global | Empresa 2 |
|---|---|---|---:|---:|
| validar | admin_validar | supervisao | 543 | 404 |
| decidir | sem_pergunta | supervisao | 600 | 449 |
| decidir | sem_pergunta | dp | 60 | 40 |
| decidir | sem_pergunta | hasner | 10 | 8 |
| decidir | sem_pergunta | rh | 5 | 3 |
| decidir | sem_pergunta | ti | 6 | 3 |
| **Total** | | | **1.224** | **907** |

Todas essas linhas têm `atribuido_a_pk=null` no dado; isso preserva os donos por pilha/departamento, não autoriza chamá-las “sem dono” (`app/chamados/services/painel.py:198`, fonte das `QUEBRA_CLASSE_DONO` fornecidas).

Empresas **3 e 4 individualmente**: **[nao verificado]**, passos `ATENDIMENTO_CONTEXTO_REAL`/`QUEBRA_CLASSE_DONO` dessas empresas ausentes do trecho. Não distribuir o residual global−empresa2 entre elas: o escopo informa empresa1 também visível, sem sua quebra individual.

## 10/11 11:42 · sessao bd5e46f79c29 · FIO-E0-V4-E4A-2 · construtor · origem=Code

Alteradas **nove asserções**, que são todas as enumeradas no pedido. Números preservados; nenhuma asserção removida. O diff é:

```diff
--- app/chamados/tests/test_colab_nao_vaza_fio_mudo.py
+++ app/chamados/tests/test_colab_nao_vaza_fio_mudo.py
@@ -49 +49 @@
-        self.assertIn('ALVOS: 1 perguntas em 1 colaboradores', s)
+        self.assertIn('ALVOS: 1 motivos devidos sem pergunta viva em 1 colaboradores', s)
@@ -54 +54 @@
-        self.assertIn('ALVOS: 2 perguntas em 2 colaboradores', s)
+        self.assertIn('ALVOS: 2 motivos devidos sem pergunta viva em 2 colaboradores', s)
--- app/chamados/tests/test_fabrica_pela_ata.py
+++ app/chamados/tests/test_fabrica_pela_ata.py
@@ -51 +51 @@
-        self.assertIn('ALVOS: 0 perguntas', saida)
+        self.assertIn('ALVOS: 0 motivos devidos sem pergunta viva', saida)
@@ -64 +64 @@
-        self.assertIn('ALVOS: 0 perguntas', saida)
+        self.assertIn('ALVOS: 0 motivos devidos sem pergunta viva', saida)
@@ -71 +71 @@
-        self.assertIn('ALVOS: 0 perguntas', saida)
+        self.assertIn('ALVOS: 0 motivos devidos sem pergunta viva', saida)
--- app/chamados/tests/test_pergunta_do_dia_em_curso.py
+++ app/chamados/tests/test_pergunta_do_dia_em_curso.py
@@ -106 +106 @@
-        self.assertIn('ALVOS: 0 perguntas', out.getvalue())
+        self.assertIn('ALVOS: 0 motivos devidos sem pergunta viva', out.getvalue())
--- app/chamados/tests/test_selo_emissor_fio_mudo.py
+++ app/chamados/tests/test_selo_emissor_fio_mudo.py
@@ -84 +84 @@
-        self.assertIn('ALVOS: 1 perguntas', saida)
+        self.assertIn('ALVOS: 1 motivos devidos sem pergunta viva', saida)
@@ -119 +119 @@
-        self.assertIn('ALVOS: 1 perguntas', saida)
+        self.assertIn('ALVOS: 1 motivos devidos sem pergunta viva', saida)
@@ -173 +173 @@
-        self.assertIn('ALVOS: 1 perguntas', saida)
+        self.assertIn('ALVOS: 1 motivos devidos sem pergunta viva', saida)
```

Conferência executada em `app/`:

```bash
rg -n -uu -g '!.git*' -g '!**/.git*' \
  -g '!**/__pycache__/**' -g '!*.pyc' -g '!.env*' \
  'ALVOS: [0-9]+ perguntas' .
```

Saída vazia; **rc 1**, sem ocorrências. `git diff --check` também retornou saída vazia, **rc 0**.

Quem escreve e quem lê:

- Escritor: [disparar_perguntas_competencia.py:215](chamados/management/commands/disparar_perguntas_competencia.py:215).
- Leitores alterados: os nove pontos identificados no diff.
- Outros leitores do rótulo atual: [test_e4a2_matriz_memoria.py:311](chamados/tests/test_e4a2_matriz_memoria.py:311) e [test_e4a2_disparar_e_vigia.py:222](chamados/tests/test_e4a2_disparar_e_vigia.py:222). As ocorrências antigas com `%d` pertencem a mutantes/base: `:80` do primeiro e `:36,642` do segundo.
- Encadeamento/log: [config/crons.py:1521](config/crons.py:1521), com `fio_mudo.log` em `:1522`.
- Raspador externo: `../bin/placar_code.sh:390–391` lê **FABRICA-POR-EVENTO** do log. Leitura de `ALVOS` em produção: **[nao verificado]**.
- Outro comando preservado: [retratar_chamados_lei_cala.py:50](chamados/management/commands/retratar_chamados_lei_cala.py:50).

A verificação **textual em memória**, usando o formato extraído do escritor `:215` e as nove asserções acima, apresentou RED com nove falhas antes da edição, GREEN depois e rejeitou 20 mutantes: nove rótulos antigos, nove contagens N incorretas e duas contagens M incorretas. Comando reproduzível para os três:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import ast, re, subprocess, unittest
from pathlib import Path

base = 'bd807a034ea73cf4bcd65dc02e22f45258ea26dc'
writer = Path('chamados/management/commands/disparar_perguntas_competencia.py')
fmt, = [n.value for n in ast.walk(ast.parse(writer.read_text()))
        if isinstance(n, ast.Constant) and isinstance(n.value, str)
        and n.value.startswith('ALVOS: ')]
case = unittest.TestCase()
red = green = killed = 0

for rel in subprocess.check_output(
        ['git', 'diff', '--name-only'], text=True).splitlines():
    assert rel.startswith('app/chamados/tests/')
    old = subprocess.check_output(['git', 'show', f'{base}:{rel}'], text=True)
    new = Path(rel.removeprefix('app/')).read_text()
    assert new == re.sub(
        r'(ALVOS: [0-9]+) perguntas',
        r'\1 motivos devidos sem pergunta viva', old)
    for source, phase in ((old, 'RED'), (new, 'GREEN')):
        for node in ast.walk(ast.parse(source)):
            if not (isinstance(node, ast.Call)
                    and isinstance(node.func, ast.Attribute)
                    and node.func.attr == 'assertIn' and node.args):
                continue
            expected = (ast.literal_eval(node.args[0])
                        if isinstance(node.args[0], ast.Constant) else None)
            if not isinstance(expected, str) or not expected.startswith('ALVOS: '):
                continue
            nums = list(map(int, re.findall(r'\d+', expected)))
            n, m = nums[0], nums[1] if len(nums) == 2 else 1
            if phase == 'RED':
                with case.assertRaises(AssertionError):
                    case.assertIn(expected, fmt % (n, m))
                red += 1
                continue
            case.assertIn(expected, fmt % (n, m))
            green += 1
            mutants = [
                fmt.replace('motivos devidos sem pergunta viva', 'perguntas')
                % (n, m),
                fmt % (n + 1, m),
            ]
            if len(nums) == 2:
                mutants.append(fmt % (n, m + 1))
            for mutant in mutants:
                with case.assertRaises(AssertionError):
                    case.assertIn(expected, mutant)
                killed += 1
assert (red, green, killed) == (9, 9, 20)
print(f'Contrato textual: RED={red}; GREEN={green}; mutantes rejeitados={killed}')
PY
```

Não rodei os testes Django nem a suite: execução comportamental **[nao verificado]**. Não houve certificação pelo Code ou pelo opus final **[nao verificado]**. [nao verificado]

## 10/11 11:41 · sessao 1f54129ccaf4 · - · - · origem=Ronald (tmux)

Pelo agregado fornecido `E4B_EXPORTACAO`, o recorte E4B fecha assim; “anterior” = total não exportado menos novembro/2026, pois as competências declaradas são somente 10/2026 e 11/2026 (`app/ponto/services/he_pendente_lavrado.py:209`; `app/chamados/services/vigia_fio.py:583`; fonte declarada do agregado: `app/chamados/services/vigia_fio.py:822`).

| Classe | Corrente 11/2026, não entregue | Anterior 10/2026, não entregue | Entregue/exportada | Total |
|---|---:|---:|---:|---:|
| a | 2 | 0 | 0 | 2 |
| b | 0 | 0 | 0 | 0 |
| c | 4 | 12 | 0 | 16 |
| d | 156 | 101 | 0 | 257 |
| e | 47 | 5 | 0 | 52 |
| **Total** | **209** | **118** | **0** | **327** |

O zero entregue vale para este recorte: `competencias_a_lavrar` seleciona a corrente e acrescenta a anterior somente se não entregue; não cobre todo o histórico exportado (`app/ponto/services/he_pendente_lavrado.py:220`; `app/ponto/services/he_pendente_lavrado.py:223`). A identificação da competência de cada dia usa a janela da empresa; a entrega usa `competencia_entregue`, portanto mês civil da data não substitui competência (`app/chamados/services/vigia_fio.py:822`; `app/chamados/services/vigia_fio.py:838`).

As classes são exclusivas pela ordem dos ramos:

| Classe | Critério | Fonte |
|---|---|---|
| a | Chamado vivo do dia ou âncora viva fora da fila, após exclusão das portas e baldes anteriores | `app/chamados/services/vigia_fio.py:663`, `:669` |
| b | Pergunta viva respondida sem texto | `app/chamados/services/vigia_fio.py:673` |
| c | Pergunta viva respondida, com chamado âncora terminal | `app/chamados/services/vigia_fio.py:675` |
| d | Sem perguntas vivas; dia fora da lista; colaborador presente na lista | `app/chamados/services/vigia_fio.py:677` |
| e | Restante, incluindo ausência de célula e detalhes do ramo residual | `app/chamados/services/vigia_fio.py:667`, `:679` |

O universo é de dias que seguram a folha nas empresas visíveis intersectadas com o filtro, deduplicados por colaborador e data; o `sem_porta` é registrado após excluir porta viva, hoje/futuro e os quatro baldes legais (`app/chamados/services/vigia_fio.py:567`; `:581`; `:642`; `:648`; `:654`; `:699`). `limite` nunca omite uma linha `sem_porta`; as 2.130 “outras rotas omitidas” não são complemento de E4B a adicionar aos 327 (`app/chamados/services/vigia_fio.py:628`).

Somando `E4B_CLASSE_DONO_COMPETENCIA` fornecido, a quebra por dono do dia também fecha em 327; fonte declarada inclui `app/ponto/management/commands/e6_oraculo.py:182` e `app/chamados/services/vigia_fio.py:542`.

| Dono do dia | Corrente | Anterior | Total |
|---|---:|---:|---:|
| BATIDA | 10 | 3 | 13 |
| CADASTRO | 38 | 6 | 44 |
| ESTRUTURA | 22 | 14 | 36 |
| `[nao verificado]` | 139 | 95 | 234 |
| **Total** | **209** | **118** | **327** |

Os 234 com dono `[nao verificado]` não autorizam substituição por ESTRUTURA: `InsumosDoDono.dono` pode devolver `None` no modo sem motor (`app/ponto/management/commands/e6_oraculo.py:189`; `:193`).

A quebra E4B por empresa não foi emitida nos agregados completos apresentados: `[nao verificado]`, passo `E4B_CLASSE_DONO_COMPETENCIA`, que não contém `empresa_pk`. A lista `CELULA_E4B` pode permitir contagem somente se sua completude for conferida contra 327; essa conferência não foi realizada neste subtrabalho `[nao verificado]`.

## 10/11 11:37 · sessao 112a44ad6ecd · URGENCIA-E4B · sonda · origem=Code

**O badge lavrado, 1.228, é o contador conhecido mais próximo de 1.233: diferença de 5.** A comparação com Justificativas pendentes permanece **[nao verificado]**, pois seu valor não aparece no trecho recebido. Fonte: saída `NAVBAR_EXIBIDA`, `app/core/context_processors.py:68`.

Esta resposta consolida a saída fornecida. Nova execução, role efetiva, transação READ ONLY e rollback: **[nao verificado]**. O dado termina durante um `ITEM_CENTRAL`; conclusão da execução, eventual `ERRO_SONDA`, passo e cadeia de locais: **[nao verificado]**.

**1. Contadores — retrato fornecido**

| Contador | Valor | Distância de 1.233 | Registro da saída → arquivo:linha |
|---|---:|---:|---|
| **Badge lavrado** | **1.228** | **5** | `NAVBAR_EXIBIDA` → `app/core/context_processors.py:68` |
| Central de Atendimento — total da lista | 3.606 | 2.373 | `ATENDIMENTO_CONTEXTO_REAL` → `app/chamados/services/painel.py:198` |
| Validar | 543 | 690 | `ATENDIMENTO_CONTEXTO_REAL` → `app/chamados/services/verbos.py:30` |
| Cobrar | 2.346 | 1.113 | Mesmo registro → `app/chamados/services/verbos.py:30` |
| Decidir | 680 | 553 | Mesmo registro → `app/chamados/services/verbos.py:30` |
| Soma Validar + Cobrar + Decidir | 3.569 | 2.336 | Cálculo sobre o mesmo registro → `app/chamados/services/verbos.py:30` |
| Controle atual do admin: Validar + Decidir | 1.223 | 10 | `BADGE_EXIBIDO_VS_ADMIN_ATUAL` → `app/chamados/services/verbos.py:25` |
| Justificativas pendentes | **[nao verificado]** | **[nao verificado]** | Contagem definida em `app/ponto/views.py:1736`; resultado ausente |

O badge foi lavrado às **11:15:46 de 10/11/2026**; a saída informa diferença de **5** para o controle atual. A equivalência dos conjuntos e a decomposição dos 1.228 itens são **[nao verificado]**. Fonte: `NAVBAR_EXIBIDA` e `BADGE_EXIBIDO_VS_ADMIN_ATUAL` → `app/core/context_processors.py:68`; `app/chamados/services/verbos.py:30`.

Os controles adicionais são **3.591 ativos**, compostos por **2.010 abertos + 1.581 em análise**. A conciliação item a item entre esses controles, a lista de 3.606 e as pilhas de 3.569 é **[nao verificado]**. Fonte: `ATENDIMENTO_CONTEXTO_REAL` → `app/chamados/services/painel.py:198`.

**2. Partição por empresa, classe/verbo e dono**

| Empresa | Classe de origem × verbo | Atribuído | Pilha | Departamento |
|---|---|---|---|---|
| 2 | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |
| 3 | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |
| 4 | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |

A listagem parcial não permite fechar essas partições sobre os **3.606 itens**, nem sobre os **1.223 atuais do admin**; a partição do **badge lavrado de 1.228** também é **[nao verificado]**.

O código distingue pilha e departamento: `admin_validar → validar`, `colab → cobrar`, `sem_pergunta/cadastro → decidir`, `registro → sem verbo`; o departamento tem outra função de classificação. Fontes: `app/chamados/catalogo/natureza.py:58`; `app/chamados/utils.py:32`.

**3. `censo_alcance`: somente rota `sem_porta`, empresas 2, 3 e 4**

| Classe | Significado | Corrente 11/2026, não entregue | Anterior 10/2026, não entregue | Entregue/exportada no recorte | Total | Registro → arquivo:linha |
|---|---|---:|---:|---:|---:|---|
| a | Chamado vivo com véu | 2 | 0 | 0 | 2 | `E4B_EXPORTACAO` → `app/chamados/services/vigia_fio.py:822` |
| b | Respondida sem resposta | 0 | 0 | 0 | 0 | Mesmo registro/fonte |
| c | Respondida com destino fechado | 4 | 12 | 0 | 16 | Mesmo registro/fonte |
| d | Colaborador na lista; dia fora | 161 | 101 | 0 | 262 | Mesmo registro/fonte |
| e | Outra | 47 | 5 | 0 | 52 | Mesmo registro/fonte |
| **Total** | | **214** | **118** | **0** | **332** | Soma de `E4B_EXPORTACAO`; `UNIVERSO_E4B` → `app/chamados/services/vigia_fio.py:628` |

Os rótulos das classes estão em `app/chamados/services/vigia_fio.py:538`. As colunas “anterior” foram calculadas como total menos corrente; o recorte informado contém apenas as competências **11/2026 e 10/2026**. Fonte: `UNIVERSO_E4B` → `app/chamados/services/vigia_fio.py:628`.

Esse censo cobre a competência corrente e a anterior enquanto não entregue; **não cobre todo o histórico exportado**. Portanto, o zero de entregues da tabela vale apenas para esse recorte. Fonte: `app/ponto/services/he_pendente_lavrado.py:209`; `app/chamados/services/vigia_fio.py:583`.

| Cruzamento solicitado | Quantidade |
|---|---|
| Itens da Central de Atendimento que caem nas 332 células sem porta | **[nao verificado]** |
| Itens de Validar/Decidir que caem nelas | **[nao verificado]** |
| Itens do badge lavrado que caem nelas | **[nao verificado]** |

O controle antigo informa **4 fios mortos**, mas representa outro censo; não substitui os **332 sem porta por alcançabilidade**. Fontes: `CONTROLE_CENSO_ANTIGO` → `app/chamados/services/vigia_fio.py:449`; `UNIVERSO_E4B` → `app/chamados/services/vigia_fio.py:628`.

## 10/11 11:36 · sessao c4bcf884eb66 · FIO-E0-V4-E4A-2 · construtor · origem=Code

A regra foi restaurada; o **item 5 permanece pendente**.

Arquivos tocados:

- [fabrica_da_celula.py:120](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/chamados/services/fabrica_da_celula.py:120): não-acusador retorna `[]`; arquivo inteiro idêntico a `e13fa92e3`. Diff vazio, rc 0 ([prova](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/RELATO_FIO_E0_V4_E4A_2_9e.txt:8)).
- [test_e4a2_matriz_memoria.py:201](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/chamados/tests/test_e4a2_matriz_memoria.py:201): concorde exige zero; mutantes ajustados.
- [test_pergunta_do_dia_em_curso.py:442](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/chamados/tests/test_pergunta_do_dia_em_curso.py:442): guarda usa `furo`, consulta diretamente o juiz e inclui caso próprio `concorde → 0`.
- [Relato completo](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/RELATO_FIO_E0_V4_E4A_2_9e.txt:1): comandos, provas e limites.

Validação local: **RED reproduzido contra a 9d; 29 testes passaram; 19 mutantes rejeitados**, com comandos individuais no [relato:55](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/RELATO_FIO_E0_V4_E4A_2_9e.txt:55). `_pks_de_empresa`, rótulo ALVOS e seus testes foram preservados ([prova:43](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/RELATO_FIO_E0_V4_E4A_2_9e.txt:43)). [nao verificado]

Os 14 testes mantêm seus corpos intactos, mas **10 exigem o rótulo antigo**, incompatível com o item 2 ([evidências:109](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/RELATO_FIO_E0_V4_E4A_2_9e.txt:109)). Seus resultados Django e Ruff limpo estão **[não verificado]**: ambos indisponíveis neste Python ([limites:194](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/RELATO_FIO_E0_V4_E4A_2_9e.txt:194)). [nao verificado]

O censo de **quem escreve e quem lê cada campo** está no [relato:134](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/RELATO_FIO_E0_V4_E4A_2_9e.txt:134).

## 10/11 11:24 · sessao ac3391eb6fde · FIO-E0-V4-E4A-2 · verificador · origem=Code

**A 9d não está pronta: há nove asserções incompatíveis com o novo rótulo `ALVOS` e uma divergência entre a fábrica e o leitor de pergunta viva que precisa ser resolvida.** (`app/chamados/management/commands/disparar_perguntas_competencia.py:215`; `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49`; `app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/juizes.py:1386`.)

1. **Chamadores e mudança de comportamento**

   O censo imediato de produção é:

   | Chamador/cadeia | Efeito da mudança |
   |---|---|
   | `perguntar_pela_celula → motivos_a_perguntar` | Pode criar pergunta antes bloqueada. (`app/chamados/services/fabrica_da_celula.py:210`; `app/chamados/services/fabrica_da_celula.py:222`.) |
   | `cartorio.julgar_colab → perguntar_pela_celula` | Leva a mudança ao julgamento real das células desta passada. (`app/ponto/services/cartorio.py:1006`; `app/ponto/services/cartorio.py:1008`.) |
   | `reconciliador → perguntar_por_eventos → perguntar_pela_celula` | Leva a mudança ao consumidor de eventos. O arquivo existente é `app/chamados/reconciliador.py`. (`app/chamados/reconciliador.py:1386`; `app/chamados/services/fabrica_da_celula.py:304`.) |
   | `motivos_que_o_evento_deixou → motivos_a_perguntar` | Muda a pendência contada pelo vigia da célula; não emite. (`app/chamados/services/fabrica_da_celula.py:245`; `app/chamados/management/commands/disparar_perguntas_competencia.py:185`.) |
   | `contador_fio_mudo → motivos_a_perguntar` | Muda os motivos contados; não emite. (`app/chamados/management/commands/disparar_perguntas_competencia.py:115`; `app/chamados/management/commands/disparar_perguntas_competencia.py:120`.) |

   **Sim, existe caminho que passa a criar pergunta:** veredito preenchido não acusador, colaborador ativo, dia permitido pelo juiz de trabalho, turno ainda em curso, marco vencido faltante na ata, sem barreira parcial e sem pergunta ocupando o fato. A alteração alcança os escritores compartilhados, não somente o contador. (`app/chamados/services/fabrica_da_celula.py:112`; `app/chamados/services/fabrica_da_celula.py:119`; `app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:126`; `app/chamados/services/fabrica_da_celula.py:142`; `app/chamados/services/fabrica_da_celula.py:160`.)

   | Estado | Resultado da 9d |
   |---|---|
   | `nunca_bateu` | Continua retido antes da nova condição. (`app/chamados/services/fabrica_da_celula.py:105`.) |
   | Veredito acusador | Não passa pelo novo teste de encerramento. (`app/chamados/services/fabrica_da_celula.py:120`.) |
   | Não acusador + encerrado | Continua sem motivos. (`app/chamados/services/fabrica_da_celula.py:123`.) |
   | Não acusador + em curso + falta vencida | Pode devolver motivo e alcançar a emissão. (`app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:222`.) |
   | Noturno de ontem ainda em curso | Recebe o mesmo tratamento; ontem civil não implica encerramento. (`app/chamados/services/fabrica_da_celula.py:123`; `app/ponto/turnos.py:2111`.) |
   | Marco batido, sem falta lida | Não gera motivo. (`app/chamados/services/fabrica_da_celula.py:64`; `app/chamados/tests/test_pergunta_do_dia_em_curso.py:485`.) |
   | Sem ata suficiente ou sem marco vencido | Não gera motivo. (`app/chamados/services/fabrica_da_celula.py:58`; `app/chamados/services/fabrica_da_celula.py:65`.) |

   A emissão permanece **com push silenciado**; a porta de push retorna quando esse contexto está ativo. (`app/chamados/services/fabrica_da_celula.py:221`; `app/chamados/services/disputa_emissao.py:1753`.)

   Incluir hoje e conservar o teto no juiz atende ao ALVO. Porém a condição abrange **todos** os não acusadores, inclusive `trabalhou`, `faltou` e `justificado`, sem distinguir `veredito_via='admin'`. O aval específico dessa extensão administrativa é [nao verificado]. (`aval_alvo_v9c_guarda_1110.txt:1`, anexo; `app/chamados/services/fabrica_da_celula.py:120`; `app/escala/models.py:1759`.)

2. **Predicado e autoridade de `dia_encerrado`**

   O predicado efetivo é:

   `sem turno aberto vivo AND fim previsto existe AND agora >= fim previsto`.

   Sem fim previsto, responde falso. Uma aberta pode continuar viva até a expiração definida pelo fim da janela do template mais quatro horas; portanto ultrapassar o fim previsto, sozinho, não basta. (`app/ponto/turnos.py:2000`; `app/ponto/turnos.py:2030`; `app/ponto/turnos.py:2115`; `app/ponto/turnos.py:2124`.)

   A fábrica passa **`cel.data`, dia do turno**, e o `agora` recebido. `dia_encerrado_motivo` lê `timezone.now()` somente se não receber `agora`; a própria fábrica tem seu fallback anterior em `timezone.localtime()`. (`app/chamados/services/fabrica_da_celula.py:116`; `app/chamados/services/fabrica_da_celula.py:123`; `app/ponto/turnos.py:2096`; `app/ponto/turnos.py:2098`.)

   **É autoridade registrada**, para “o dia está encerrado?”. Os cortes corrigidos permitem esse uso e proíbem substituí-lo por `data < hoje`. `par_completo` responde outra pergunta; não é substituto obrigatório aqui. (`app/core/juizes.py:235`; `app/core/juizes.py:236`; `app/docs/CORTES.md:55`; `app/docs/CORTES.md:62`; `CLAUDE.md:185`.)

   Há, entretanto, outro critério já consumido pelo fio: `lastro.marcos_pendentes` identifica o caráter provisório da ata e é lido pelo C6. Esse critério não equivale necessariamente a `dia_encerrado`; a consequência está no item 6. (`app/ponto/services/lastro.py:138`; `app/chamados/juizes.py:1385`.)

3. **Guarda, corpos congelados e asserções alteradas**

   A guarda continua exigindo **`FIO MUDO: 1` às 13:40 e nenhuma pergunta escrita**. No delta 9d fornecido, seu corpo não foi alterado. Comparação independente byte a byte dos demais “corpos congelados”, sem baseline identificado na cópia: [nao verificado]. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:460`.)

   As mudanças nas verificações existentes da 9d são:

   - Troca do texto esperado de `ALVOS` e exigência da menção à Pauta DP: correspondem ao P3 da triagem. (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:222`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:224`; `triagem_preverif8.txt:4`, anexo.)
   - Mensagens específicas, mínimo de catálogo, trecho mutado único e `assertRaisesRegex`: fortalecem a rejeição dos mutantes e atendem ao P1. (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:242`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:248`; `triagem_preverif8.txt:2`, anexo.)
   - O helper passa a exigir rejeição de `concorde` quando `encerrado=True`. O teste anterior conserva esse padrão e continua exigindo zero; os casos em curso são acrescentados separadamente. (`app/chamados/tests/test_e4a2_matriz_memoria.py:100`; `app/chamados/tests/test_e4a2_matriz_memoria.py:169`; `app/chamados/tests/test_e4a2_matriz_memoria.py:194`; `app/chamados/tests/test_e4a2_matriz_memoria.py:199`.)

   **Regressão certa:** nove asserções ficaram esperando `ALVOS: N perguntas`, incompatível com a saída atual `ALVOS: N motivos devidos...`:

   - `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49`, `:54`;
   - `app/chamados/tests/test_selo_emissor_fio_mudo.py:84`, `:119`, `:173`;
   - `app/chamados/tests/test_fabrica_pela_ata.py:51`, `:64`, `:71`;
   - `app/chamados/tests/test_pergunta_do_dia_em_curso.py:106`.

   A saída conflitante está em `app/chamados/management/commands/disparar_perguntas_competencia.py:215`.

4. **`_pks_de_empresa`**

   Os dois chamadores de produção são `contador_fio_mudo` e `contador_nunca_bateu`. O disparar entra com `None`; o vigiar entra com `None` ou lista materializada de instâncias `Empresa`. Não há escalar acidental nesses caminhos. (`app/chamados/management/commands/disparar_perguntas_competencia.py:52`; `app/chamados/management/commands/disparar_perguntas_competencia.py:88`; `app/chamados/management/commands/disparar_perguntas_competencia.py:189`; `app/chamados/management/commands/vigiar_fio_morto.py:156`; `app/chamados/management/commands/vigiar_fio_morto.py:160`.)

   Nos testes há listas de pks/instâncias, tuplas, iteradores e `None`. Empresa única, inteiro e texto aparecem **deliberadamente como entradas negativas**, esperando `TypeError`. (`app/chamados/tests/test_e4a2_nunca_bateu.py:119`; `app/chamados/tests/test_e4a2_nunca_bateu.py:125`; `app/chamados/tests/test_e4a2_nunca_bateu.py:310`; `app/chamados/tests/test_e4a2_c5_memoria.py:89`; `app/chamados/tests/test_e4a2_matriz_memoria.py:214`.)

   Não há consumo duplo do gerador original: a primeira normalização produz lista, reutilizada no ORM e repassada à segunda normalização. Queryset é compatível com esse contrato de iteração; um chamador efetivo passando queryset é [nao verificado]. (`app/chamados/management/commands/disparar_perguntas_competencia.py:33`; `app/chamados/management/commands/disparar_perguntas_competencia.py:36`; `app/chamados/management/commands/disparar_perguntas_competencia.py:104`; `app/chamados/management/commands/disparar_perguntas_competencia.py:121`.)

5. **Matriz (a)–(d) e bancos 1–4**

   A estrutura do contador permanece:

   | Contrato | Realização |
   |---|---|
   | (a) Retido `nunca_bateu` | Separado dos alvos; número de pares e esperado pela interseção com C5. (`app/chamados/management/commands/disparar_perguntas_competencia.py:66`; `:111`.) |
   | (b) Motivo devido | Consulta o juiz e deduplica `(colab,dia,motivo)`; esperado zero. (`app/chamados/management/commands/disparar_perguntas_competencia.py:115`; `:116`; `:122`.) |
   | (c) Pergunta viva | Qualquer pergunta viva ligada exclui o chamado inteiro, conforme o descarte P2. (`app/chamados/management/commands/disparar_perguntas_competencia.py:109`; `triagem_preverif8.txt:3`, anexo.) |
   | (d) Tranca | Universo `VIVOS`, sem filtro paralelo para esconder chamado que a tranca não encerrou, conforme P4. (`app/chamados/management/commands/disparar_perguntas_competencia.py:99`; `triagem_preverif8.txt:5`, anexo.) |

   O corte até ontem pertence à comparação C5; **não foi introduzido no universo do fio mudo**. (`app/chamados/management/commands/disparar_perguntas_competencia.py:64`; `app/chamados/management/commands/disparar_perguntas_competencia.py:99`.)

   Bancos 1–4 continuam implementados: saída dos juízes por varredura e remoção de `--apply`; publicação dos dois contadores; leitura pelo pipeline; exceção real vazia com controles sintéticos no selo. (`app/config/crons.py:2032`; `app/chamados/management/commands/disparar_perguntas_competencia.py:132`; `app/chamados/management/commands/vigiar_fio_morto.py:233`; `app/core/management/commands/pipeline_placar.py:135`; `bin/tests/test_papel_prazo_nao_deriva.sh:305`; `bin/tests/test_papel_prazo_nao_deriva.sh:384`.)

   A regressão comprovada é na compatibilidade dos testes do **banco 5**. A errata declara a medição de produção `06h35 FIO MUDO 0, ALVOS 0`; sua reprodução ou certificação para a semântica nova da 9d é [nao verificado]. (`app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49`; `aval_errata_b3_matriz_1110.txt:1`, anexo.)

6. **Quem escreve, quem lê e significado**

   | Campo/valor | Escritor/fonte e leitores relevantes |
   |---|---|
   | `veredito` | Porta `lavrar_veredito`; o cartório fornece o julgamento e a via administrativa prevalece. Leem fábrica, C6, reconciliador e C5. (`app/ponto/portas/celula.py:974`; `:1003`; `:1035`; `app/chamados/services/fabrica_da_celula.py:120`; `app/chamados/juizes.py:1372`; `app/chamados/reconciliador.py:1027`; `app/escala/services/cadastro_realidade.py:181`.) |
   | `ata` | Cartório monta e aplica o teto; porta persiste. Fábrica lê faltantes pela cadeia `_ata_do_dia → _marcos_faltantes → grade_da_celula`; C6 lê pendentes. (`app/ponto/services/cartorio.py:1811`; `app/ponto/portas/celula.py:1033`; `app/chamados/services/disputa_emissao.py:895`; `app/chamados/services/declaracao_texto.py:100`; `app/chamados/juizes.py:1385`.) |
   | `julgada_em` | `carimbar` grava o instante. O cartório captura o valor anterior e o entrega à fábrica para `parcial_so_lista`; contador/avulso usam o persistido. (`app/ponto/portas/celula.py:1065`; `app/ponto/services/cartorio.py:954`; `:1008`; `app/chamados/services/fabrica_da_celula.py:126`.) |
   | `ALVOS` | Disparar imprime quantidade de motivos e pessoas distintas. Os leitores incompatíveis encontrados são os nove testes do item 3. (`app/chamados/management/commands/disparar_perguntas_competencia.py:215`; `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49`.) |
   | `fio_mudo_alvos` | Contador deriva; vigiar publica número/esperado; pipeline compara; `fio_estado` também compara dinamicamente o carimbo inteiro. (`app/chamados/management/commands/disparar_perguntas_competencia.py:122`; `app/chamados/management/commands/vigiar_fio_morto.py:233`; `:241`; `app/core/management/commands/pipeline_placar.py:98`; `bin/fio_estado.py:125`.) |
   | `nunca_bateu_sem_adesao` | Número e esperado são pares; totais C5 são informativos. No JSON, o nome aparece como número em `censo/esperado` e como objeto detalhado no topo; os leitores acessam esses níveis explicitamente. (`app/chamados/management/commands/disparar_perguntas_competencia.py:66`; `:69`; `app/chamados/management/commands/vigiar_fio_morto.py:234`; `:241`; `:242`.) |
   | `rc` | Disparar: 2 somente por `--alarme` e pendência da célula. Vigiar: pelos próprios gritos. Pipeline: por seus alarmes com chave. (`app/chamados/management/commands/disparar_perguntas_competencia.py:219`; `app/chamados/management/commands/vigiar_fio_morto.py:252`; `app/core/management/commands/pipeline_placar.py:160`.) |

   `cel.data` é dia do turno; `hoje` do carimbo é dia civil local da medição. O pipeline compara o dia civil solicitado com esse `hoje`, sem reinterpretá-lo como dia de cada alvo. (`app/ponto/turnos.py:2111`; `app/chamados/management/commands/vigiar_fio_morto.py:167`; `:230`; `app/core/management/commands/pipeline_placar.py:95`.)

   **Divergência de significado encontrada:** a fábrica admite não acusador quando `dia_encerrado=False`; o C6 mata a pergunta não respondida desse não acusador se não houver `pendente` na ata. Assim, com falta vencida, turno em curso e ata sem pendentes, a fábrica pode produzir motivo enquanto o leitor considera a pergunta morta. O reconciliador também considera absolvido o não acusador sem pendentes. Ocorrência desse estado na produção fornecida: [nao verificado]. (`app/chamados/services/fabrica_da_celula.py:123`; `:142`; `app/chamados/juizes.py:1386`; `app/chamados/reconciliador.py:1029`.)

7. **Mordida e vacuidade**

   Os catálogos principais agora exigem mínimos: **11 NuncaBateu, 12 placar, 10 relatório, 19 matriz e 13 C5**. Os laços verificam controle verde, trecho único e falha pela mensagem pretendida; há provas específicas para catálogo vazio/encolhido. (`app/chamados/tests/test_e4a2_nunca_bateu.py:66`; `:318`; `:329`; `app/core/tests/test_fio_mudo_placar.py:53`; `:294`; `:319`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:39`; `:235`; `:245`; `app/chamados/tests/test_e4a2_matriz_memoria.py:82`; `:352`; `:365`; `app/chamados/tests/test_e4a2_c5_memoria.py:71`; `:201`; `:214`.)

   O selo por AST dos catálogos Django prova sua defesa contra esvaziamento; **não executa os mutantes ORM nem seus `setUp`**. Os testes de memória do encerramento usam resposta dublada; a integração noturna está em teste Django separado. (`app/chamados/tests/test_e4a2_matriz_memoria.py:314`; `:325`; `:334`; `:151`; `app/chamados/tests/test_pergunta_do_dia_em_curso.py:297`.)

   Nesta auditoria, os dois módulos de memória executaram **28 testes OK**, e o selo shell terminou **OK, rc 0**. Comprovantes persistidos dessas execuções e execução da integração/suíte inteira: [nao verificado]. [nao verificado]

8. **Classes de reprovação**

   - **Segundo juiz registrado duplicado:** não identificado na nova chamada; ela usa a autoridade existente. (`app/core/juizes.py:235`; `app/chamados/services/fabrica_da_celula.py:123`.)
   - **Segundo escritor fora da porta:** não identificado no contador; a emissão segue pela fábrica existente. (`app/chamados/management/commands/disparar_perguntas_competencia.py:115`; `app/chamados/services/fabrica_da_celula.py:222`.)
   - **Novo relógio fora da borda nesse caminho:** não identificado; contador repassa `agora`, e a nova chamada o conserva. (`app/chamados/management/commands/disparar_perguntas_competencia.py:115`; `app/chamados/services/fabrica_da_celula.py:123`.)
   - **Selo vazio:** os mínimos corrigem o problema apontado; não certificam a suíte, que contém a incompatibilidade textual demonstrada. (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:242`; `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49`.)
   - **Campo interpretado por critérios diferentes:** presente no caráter provisório do não acusador — encerramento do turno na fábrica versus pendência da ata no C6/reconciliador. (`app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/juizes.py:1386`; `app/chamados/reconciliador.py:1029`.)
   - **Nova inversão de trava ou pouso antes do pré-requisito:** não demonstrados pelo material fornecido; execução de pouso, ordem dinâmica e comprovação operacional dos pré-requisitos: [nao verificado]. [nao verificado]

## 10/11 11:22 · sessao c72719cc1e36 · BO-0611-05 · censo · origem=Code

A classe excede o 403: há barreiras por GPS, selfie, situação, confirmação de turno e proximidade temporal. A própria L-030 já declara GPS, desligado/férias e tipo oposto como pendências; nesta cópia, a referência correta é `app/docs/LEIS.md:165`, além da lei em `CLAUDE.md:140`.

Execução da suíte, smoke nas duas cascas, comportamento do APK instalado e causalidade individual de marcação ausente de col456: **[nao verificado]**. [nao verificado]

**1. Caminhos e barreiras**

As portas de batida de chão encontradas por AST são:

| Porta | Caminho até o escritor |
|---|---|
| API do kernel | `/api/ponto/bater/` → `views_core.api_bater_ponto` → `registrar_batida`. `app/api/urls_core.py:14`; `app/api/views_core.py:681`. |
| API da outra casca | Mesma URL → `views.api_bater_ponto` → `registrar_batida`. `app/api/urls.py:67`; `app/api/views.py:684`. |
| Web clássico e V2 | `/ponto/bater/` e `/ponto/bater-v2/` → mesma view → `ramo_registrar_es` → escritor; o JS da V2 envia diretamente à API. `app/ponto/urls.py:11`; `app/ponto/urls.py:12`; `app/ponto/views.py:349`; `app/ponto/services/triagem_batida.py:701`; `app/templates/ponto/bater_ponto_v2.html:335`. |
| Offline antigo | Autenticado recebe **410**, `codigo=gone`; não chama escritor. O PWA atual sincroniza pela API. `app/ponto/views.py:417`; `app/ponto/views.py:426`; `app/static/pwa.js:153`. |

V2, legado `bater_ponto.html` e base da triagem herdam **literalmente `base.html`**. O processor fornece `base_tpl`, mas essas heranças não o usam; renderização dessas telas sob `base_app.html`: **[nao verificado]**. Ambas as bases carregam `pwa.js` e o diálogo. `app/templates/ponto/bater_ponto_v2.html:1`; `app/templates/ponto/bater_ponto.html:1`; `app/templates/ponto/triagem/base_triagem.html:1`; `app/core/context_processors.py:210`; `app/templates/base.html:20`; `app/templates/base.html:388`; `app/templates/base_app.html:20`; `app/templates/base_app.html:145`.

O clássico seleciona os templates de triagem, normalmente `triagem/apto.html`; `bater_ponto.html` não é escolhido pelos retornos dessa view. `app/ponto/views.py:225`; `app/ponto/views.py:235`; `app/ponto/views.py:356`; `app/ponto/views.py:358`.

Barreiras da API e do escritor:

| Condição e localização | Resposta / gravação | L-030 e teste que exige hoje | [nao verificado]
|---|---|---|
| `not latitude or not longitude`, incluindo zero numérico: `app/api/views_core.py:440`; `app/api/views.py:449`. | **403**, `gps_obrigatorio`, “Sua batida NAO sera registrada sem o GPS ativado”; auditoria de recusa; não chega ao escritor. `app/api/views_core.py:442`; `app/api/views_core.py:454`; `app/api/views.py:462`. | **Sim, explicitamente.** Exige 403/código: `app/api/tests/test_gps_obrigatorio.py:21`; `app/api/tests/test_gps_obrigatorio.py:22`. Exige zero gravação: `app/api/tests/test_gps_obrigatorio.py:38`. Exige auditoria: `app/api/tests/test_gps_obrigatorio.py:28`. Outro teste: `app/ponto/tests/test_batida_sideeffects.py:130`. |
| Situação fora de `ativo/afastado`: `app/api/views_core.py:402`; `app/api/views.py:415`. | **403**: desligado → `colab_desligado`; férias → `colab_ferias`; demais → `colab_situacao_invalida`. `app/api/views_core.py:403`; `app/api/views_core.py:410`; `app/api/views.py:423`. | **Sim, explicitamente para desligado/férias.** Desligado403: `app/api/tests/test_api_bater_ponto.py:65`; selo literal nos dois kernels: `app/ponto/tests/test_afastado_nunca_bloqueia.py:25`; `app/ponto/tests/test_afastado_nunca_bloqueia.py:36`. Férias: `app/api/tests/test_api_bater_ponto.py:88`; `app/ponto/tests/test_afastado_na_batida.py:115`. Teste para demais valores: **[nao verificado]**. |
| Mesmo tipo em **±5 minutos**, antes do escritor: `app/api/views_core.py:549`; `app/api/views.py:557`. | **200**, `ja_registrada`, ID anterior, `sucesso=True`; nenhuma nova linha, mesmo com instante diferente. `app/api/views_core.py:555`; `app/api/views_core.py:558`. | **É supressão temporal, distinta da duplicata exata.** Teste exige colapsar instantes separados por um minuto: `app/api/tests/test_bug127_dedup_unico.py:121`; `app/api/tests/test_bug127_dedup_unico.py:124`. Pela literalidade de `CLAUDE.md:140`, precisa entrar na cura da classe para fatos distintos. |
| Mesmo tipo em **±60 segundos**, dentro do escritor, excluindo retratadas: `app/ponto/registro_batida.py:106`; `app/ponto/registro_batida.py:109`; `app/ponto/registro_batida.py:113`. | Retorna **`(existente, None)`**, sem criar. `app/ponto/registro_batida.py:115`. | **Mesma distinção:** não exige chave exata. Teste exige colapso a +20s: `app/ponto/tests/test_registrar_batida.py:72`; `app/ponto/tests/test_registrar_batida.py:76`; `app/ponto/tests/test_registrar_batida.py:78`. |
| Mesma chave **colaborador+timestamp**, inclusive outro tipo ou retratada: `app/ponto/registro_batida.py:120`. | Retorna **`(existente, None)`**. `app/ponto/registro_batida.py:122`. | **Não é recusa de fato novo: idempotência exata.** Constraint: `app/ponto/models.py:106`. Testes: `app/ponto/tests/test_bater_mesma_chave_idempotente.py:48`; `app/ponto/tests/test_bater_mesma_chave_idempotente.py:57`. |
| Tipo oposto próximo, sem casar marco próprio, `autoridade_humana=False`: `app/ponto/registro_batida.py:145`; `app/ponto/registro_batida.py:150`. | **`(None, conflito)`**, sem gravar; APIs respondem **200**, `ignorada_intervalo`, `sucesso=True`, ID da oposta e “Aguarde…”. `app/ponto/registro_batida.py:151`; `app/api/views_core.py:709`; `app/api/views.py:712`. | **Sim, explicitamente.** API: `app/api/tests/test_api_bater_ponto.py:639`; `app/api/tests/test_api_bater_ponto.py:642`. Com template, +120s → None: `app/ponto/tests/test_dedup_envelope_template.py:43`; `app/ponto/tests/test_dedup_envelope_template.py:46`. |
| Competência fechada, **somente `not fato_de_chao`**: `app/ponto/registro_batida.py:132`. | **`(None, mensagem)`**. `app/ponto/registro_batida.py:137`. | **Não é barreira de chão.** As três portas passam `autoridade_humana=False`; o padrão deriva `fato_de_chao=True`. `app/api/views_core.py:683`; `app/api/views.py:686`; `app/ponto/services/triagem_batida.py:703`; `app/ponto/registro_batida.py:130`. Testes preservam chão e recusam materialização institucional: `app/ponto/tests/test_ajuste_retroativo_folha_fechada.py:40`; `app/ponto/tests/test_ajuste_retroativo_folha_fechada.py:61`. |

A janela oposta é **300s** com escala vigente/tipo/marcos; **60s** nos demais casos. Os limites da consulta são inclusivos, apesar da mensagem usar `<`. A exceção aceita marco do próprio tipo a **≤5 minutos**, no dia ou véspera. `app/ponto/registro_batida.py:30`; `app/ponto/registro_batida.py:32`; `app/ponto/registro_batida.py:142`; `app/ponto/registro_batida.py:143`; `app/ponto/registro_batida.py:147`; `app/ponto/registro_batida.py:42`; `app/ponto/registro_batida.py:48`.

Barreiras no web, templates e JS:

| Condição e localização | Resposta / efeito | Classe e teste exigindo | [nao verificado]
|---|---|---|
| V2 sem selfie após captura: `app/templates/ponto/bater_ponto_v2.html:287`. | Toast para habilitar câmera/tirar selfie, `false`; clique retorna antes de POST/fila. `app/templates/ponto/bater_ponto_v2.html:289`; `app/templates/ponto/bater_ponto_v2.html:290`; `app/templates/ponto/bater_ponto_v2.html:325`. | **Mesma classe pela lei:** prova ausente impede fato. `CLAUDE.md:140`. Teste exigindo: **[nao verificado]**. |
| V2 sem latitude ou longitude após novas tentativas: `app/templates/ponto/bater_ponto_v2.html:292`; `app/templates/ponto/bater_ponto_v2.html:297`. | Toast localização/Justificativa, `false`; não envia. `app/templates/ponto/bater_ponto_v2.html:299`; `app/templates/ponto/bater_ponto_v2.html:300`; `app/templates/ponto/bater_ponto_v2.html:325`. | **Sim.** `app/docs/LEIS.md:165`. Teste específico do template: **[nao verificado]**. |
| V2: fetch lança exceção e coordenada nula. `app/templates/ponto/bater_ponto_v2.html:349`; `app/templates/ponto/bater_ponto_v2.html:350`. | Toast “Nao foi possivel bater agora”; retorna antes da fila. `app/templates/ponto/bater_ponto_v2.html:352`; `app/templates/ponto/bater_ponto_v2.html:353`. | **Sim**, guarda adicional já antecedida pelo requisito acima. `CLAUDE.md:140`; `app/templates/ponto/bater_ponto_v2.html:325`. Teste: **[nao verificado]**. |
| V2 offline e coordenada nula. `app/templates/ponto/bater_ponto_v2.html:368`. | Toast GPS/Justificativa; retorna antes da fila. `app/templates/ponto/bater_ponto_v2.html:370`; `app/templates/ponto/bater_ponto_v2.html:371`. | **Sim.** `CLAUDE.md:140`. Teste: **[nao verificado]**. |
| V2 recebe HTTP não-2xx. `app/templates/ponto/bater_ponto_v2.html:340`. | GPS403 ganha toast específico; demais erros mostram mensagem/HTTP e retornam **sem enfileirar**. `app/templates/ponto/bater_ponto_v2.html:342`; `app/templates/ponto/bater_ponto_v2.html:345`; `app/templates/ponto/bater_ponto_v2.html:347`; `app/templates/ponto/bater_ponto_v2.html:348`. | É tratamento que encerra a tentativa recusada; a causa é a guarda do servidor correspondente. Teste exigindo ausência de fila: **[nao verificado]**. |
| Triagem: botões inicialmente ocultos; câmera bem-sucedida exige confirmar selfie para exibi-los; refazer oculta novamente. `app/templates/ponto/triagem/apto.html:93`; `app/templates/ponto/triagem/apto.html:182`; `app/templates/ponto/triagem/apto.html:185`; `app/templates/ponto/triagem/apto.html:191`. | Impede acesso ao botão enquanto aguarda a etapa; câmera falhando **libera** sem selfie. `app/templates/ponto/triagem/apto.html:156`; `app/templates/ponto/triagem/apto.html:163`. | Condicionamento por prova pertence à classe de `CLAUDE.md:140`. Teste exigindo: **[nao verificado]**. |
| Triagem offline: câmera falha. `app/templates/ponto/triagem/apto.html:253`. | “Camera indisponivel — contate o supervisor”; esconde captura, sem fila nesse ramo. `app/templates/ponto/triagem/apto.html:254`; `app/templates/ponto/triagem/apto.html:255`. | **Mesma classe**, selfie impedindo fato. `CLAUDE.md:140`. Teste: **[nao verificado]**. |
| Triagem offline: sem GPS, primeiro aviso da sessão e confirmação falsa. `app/templates/ponto/triagem/apto.html:277`; `app/templates/ponto/triagem/apto.html:278`; `app/templates/ponto/triagem/apto.html:281`. | Retorna antes de `registrarBatidaOffline`. `app/templates/ponto/triagem/apto.html:284`; `app/templates/ponto/triagem/apto.html:289`. | Confirmação adicional condicionada à falta de geo; mesma classe enquanto impede envio. `CLAUDE.md:140`. Teste: **[nao verificado]**. |
| POST web E com turno aberto, sem `confirmar`, após não entrar no ramo anterior de órfão. `app/ponto/views.py:275`; `app/ponto/views.py:316`. | Guarda GPS/foto em sessão e devolve **HTML 200**, sem escritor. `app/ponto/views.py:319`; `app/ponto/views.py:329`; `app/ponto/views.py:330`. | **Mesma classe por condição de jornada.** `CLAUDE.md:140`. Teste exige nenhuma linha nova e tela de confirmação: `app/ponto/tests/test_f7_ramos_frios.py:128`; `app/ponto/tests/test_f7_ramos_frios.py:141`; `app/ponto/tests/test_f7_ramos_frios.py:142`. |
| POST web mesmo tipo nos **5 minutos anteriores**. `app/ponto/services/triagem_batida.py:662`; `app/ponto/services/triagem_batida.py:663`. | **200**, JSON `ja_registrada`, sem escritor. `app/ponto/services/triagem_batida.py:669`; `app/ponto/services/triagem_batida.py:670`. | Supressão temporal, distinta da chave exata; alcançada pela literalidade de `CLAUDE.md:140`. Teste específico dessa janela web: **[nao verificado]**. |
| Escritor devolve None por oposta no web. `app/ponto/services/triagem_batida.py:714`. | **200**, `ignorada_intervalo`, “Aguarde antes de bater de novo”. A busca de resposta usa ±60s mesmo quando o escritor usou 300s. `app/ponto/services/triagem_batida.py:717`; `app/ponto/services/triagem_batida.py:720`; `app/ponto/services/triagem_batida.py:724`. | **Sim.** Testes: `app/ponto/tests/test_bater_colisao_oposto.py:42`; `app/ponto/tests/test_f9d_t1_caracterizacao_ramo_quente.py:115`; `app/ponto/tests/test_f9d_t1_caracterizacao_ramo_quente.py:120`. |
| `bloqueio_horario` recuperado da sessão. `app/ponto/views.py:263`; `app/ponto/views.py:266`. | Clássico seleciona tela de justificativa; V2 não desenha botão e manda “Abrir ponto”. `app/ponto/views.py:231`; `app/templates/ponto/triagem/fora_horario.html:20`; `app/templates/ponto/bater_ponto_v2.html:64`; `app/templates/ponto/bater_ponto_v2.html:71`. | **Barreira de acesso ao próximo registro**, embora a avaliação de horário grave o fato corrente. `app/ponto/services/triagem_batida.py:486`; `app/ponto/services/triagem_batida.py:487`; `app/ponto/services/triagem_batida.py:701`. Teste exigindo esse impedimento: **[nao verificado]**. |

`hxConfirmar(..., naoBloquear=true)` continua cancelável por Não, clique fora e Escape; o parâmetro apenas permite continuar se o markup não montar. O selo exige esse fail-open, sem exigir recusa por GPS. `app/templates/core/_dialogo.html:157`; `app/templates/core/_dialogo.html:161`; `app/templates/core/_dialogo.html:184`; `app/core/tests/test_selo_modal_nao_nasce_vazio.py:135`; `app/core/tests/test_selo_modal_nao_nasce_vazio.py:141`.

O legado também oculta botões até selfie/refazer; seu uso por uma rota atual é **[nao verificado]**. Seu envio online aceita ausência de GPS e falha HTTP/rede cai para fila; offline também passa coordenadas nulas à fila. `app/templates/ponto/bater_ponto.html:246`; `app/templates/ponto/bater_ponto.html:473`; `app/templates/ponto/bater_ponto.html:509`; `app/templates/ponto/bater_ponto.html:540`; `app/templates/ponto/bater_ponto.html:543`; `app/templates/ponto/bater_ponto.html:597`.

Há ainda barreiras de acesso e de contrato:

| Condição | Resposta e exigência atual |
|---|---|
| Onboarding incompleto | Redirect antes de GET/POST web. É cadastro condicionando acesso, não autenticação. `app/ponto/views.py:243`; `app/ponto/views.py:244`. Teste específico: **[nao verificado]**. |
| Comunicado pendente | **Só GET** redireciona para ciência; POST não entra no ramo. Teste exige 302. `app/ponto/views.py:251`; `app/ponto/views.py:255`; `app/comunicados/tests.py:181`; `app/comunicados/tests.py:186`. |
| Plataforma: carimbo `apk`; admin em mobile; navegador Android | **403** com interstitial. `/api/` é isenta; as views web de bater não declaram a isenção. `app/core/middleware.py:247`; `app/core/middleware.py:268`; `app/core/middleware.py:270`; `app/core/middleware.py:283`; `app/core/middleware.py:287`; `app/ponto/views.py:241`. Testes gerais exigem esses gates, sem testar especificamente POST de batida: `app/core/tests/test_plataforma_gate.py:31`; `app/core/tests/test_plataforma_gate.py:49`; `app/core/tests/test_plataforma_gate.py:66`. |
| Sem autenticação / credencial rejeitada | API exige `IsAuthenticated`; web exige login; JWT rejeita credencial trocada. `app/api/views_core.py:386`; `app/api/views.py:399`; `app/ponto/views.py:241`; `app/api/credencial.py:189`; `app/api/credencial.py:198`. Testes do mecanismo exigem401: `app/api/tests/test_401_diz_qual.py:45`; `app/api/tests/test_401_diz_qual.py:80`. |
| Sem colaborador vinculado | API **404**, “Colaborador não encontrado”; templates não oferecem botão. `app/api/views_core.py:391`; `app/api/views.py:404`; `app/templates/ponto/bater_ponto_v2.html:39`; `app/templates/ponto/triagem/base_triagem.html:20`. Teste específico: **[nao verificado]**. |
| Tipo fora de E/S | API **400**, “Tipo inválido”; web só encaminha E/S ao ramo de registro. `app/api/views_core.py:425`; `app/api/views_core.py:426`; `app/api/views.py:438`; `app/ponto/views.py:346`. Testes exigem400: `app/api/tests/test_api_bater_ponto.py:121`; `app/api/tests/test_api_bater_ponto.py:127`. |

Login/refresh têm guardas próprias, anteriores ao POST de batida: situação diferente de ativo é recusada inclusive para afastado; empresa inativa também é recusada. Login responde403; refresh401. Isso não é uma guarda geo, e não equivale apenas a validar senha/token. `app/api/login_helpers.py:141`; `app/api/login_helpers.py:143`; `app/api/login_helpers.py:38`; `app/api/login_helpers.py:59`; `app/api/views_core.py:173`; `app/api/views_core.py:179`; `app/api/views.py:116`; `app/api/views.py:122`. Testes exigem recusa por situação/empresa: `app/api/tests/test_api_login.py:137`; `app/api/tests/test_api_login.py:151`; `app/api/tests/test_api_login.py:243`; `app/api/tests/test_api_login.py:252`. Participação de renovação numa tentativa concreta do dado fornecido: **[nao verificado]**.

Na fila e service worker:

- A fila aceita lat/lng nulos; sync retorna sem enviar quando offline ou sem meta `user-perfil=colaborador`. Ambas as bases emitem essa meta apenas para não-staff. `app/static/pwa.js:107`; `app/static/pwa.js:116`; `app/static/pwa.js:127`; `app/static/pwa.js:128`; `app/templates/base.html:16`; `app/templates/base_app.html:16`.
- Sync omite coordenadas nulas, envia à API e apaga item em **2xx ou400**. Portanto também apaga respostas `ignorada_intervalo`/`ja_registrada` recebidas como200. Outros erros permanecem para nova sincronização. `app/static/pwa.js:145`; `app/static/pwa.js:153`; `app/static/pwa.js:158`; `app/static/pwa.js:162`; `app/static/pwa.js:166`; `app/static/pwa.js:172`; `app/api/views_core.py:558`; `app/api/views_core.py:709`. Teste exigindo esses descartes da fila: **[nao verificado]**.
- O SW **não intercepta POST**. Background sync delega à página; sem página apenas notifica para abrir o app. `app/static/sw.js:35`; `app/static/sw.js:36`; `app/static/sw.js:117`; `app/static/sw.js:119`; `app/static/sw.js:121`.
- `_enviando`/`_confirmandoOffline` impedem clique concorrente; erro de IndexedDB rejeita a gravação local. São controle de operação/falha técnica, não julgamento de geo/jornada. `app/templates/ponto/bater_ponto_v2.html:321`; `app/templates/ponto/triagem/apto.html:262`; `app/static/pwa.js:121`.

Não há barreira de raio, precisão ou `tolerante_offline` nesses caminhos:

- V2: raio apenas badge/mapa; precisão >200m apenas aviso. `app/templates/ponto/bater_ponto_v2.html:214`; `app/templates/ponto/bater_ponto_v2.html:220`; `app/templates/ponto/bater_ponto_v2.html:228`.
- API/web verificam cerca **depois** do escritor; testes exigem200 fora da cerca, inclusive sem accuracy. `app/api/views_core.py:811`; `app/api/views.py:811`; `app/ponto/services/triagem_batida.py:752`; `app/ponto/tests/test_batida_sideeffects.py:106`; `app/ponto/tests/test_batida_sideeffects.py:152`.
- `tolerante_offline` existe no Posto, mas perdeu o leitor e está somente leitura no admin; não é parâmetro desses handlers. `app/colaboradores/models.py:225`; `app/core/configuracao_efeito.py:123`; `app/colaboradores/admin.py:44`.
- Mock, assinatura inválida, liveness, falta de foto na API, sem posto/escala e órfão são sinais, não retornos de recusa nesses ramos. `app/api/views_core.py:471`; `app/api/views_core.py:480`; `app/api/views_core.py:497`; `app/api/views_core.py:504`; `app/ponto/services/triagem_batida.py:123`; `app/ponto/views.py:312`. Afastado200 e mock200 são exigidos por testes. `app/api/tests/test_api_bater_ponto.py:75`; `app/api/tests/test_api_bater_ponto.py:159`.

**2. Como “sem geo” já é representado**

O sinal existente é **`Batida.latitude`/`longitude` nulos**. Não há flag própria `sem_geo` em Batida; `gps_accuracy` nulo significa precisão ausente, não necessariamente posição ausente. `app/ponto/models.py:8`; `app/ponto/models.py:58`; `app/ponto/models.py:59`; `app/ponto/models.py:60`.

O escritor sanitiza `''`, `None` e valores não numéricos para `None`, por campo, antes do create. As três portas já entregam esses campos ao escritor. `app/ponto/registro_batida.py:3`; `app/ponto/registro_batida.py:11`; `app/ponto/registro_batida.py:16`; `app/ponto/registro_batida.py:105`; `app/api/views_core.py:687`; `app/api/views.py:690`; `app/ponto/services/triagem_batida.py:707`.

Existe também o sinal secundário **`AlertaGeofence.tipo='sem_gps'`**. Após gravar, `verificar_geofence` reconhece coordenada ausente e, **havendo posto**, cria alerta com coordenadas nulas; depois abre/anexa ocorrência `gps_ausente` e tenta avisar supervisão. Sem posto,

[truncado em 20000 de 29209 caracteres]

## 10/11 11:20 · sessao e4e84d07c69f · - · - · origem=Ronald (tmux)

1. **Chamadores e efeito da 9d.** As três chamadas diretas de produção a `motivos_a_perguntar` são `perguntar_pela_celula`, `motivos_que_o_evento_deixou` e `contador_fio_mudo`; o cartório chama a primeira; o consumidor do reconciliador chama `perguntar_por_eventos`, que chama a primeira. O caminho informado como `chamados/services/reconciliador.py` corresponde nesta cópia a `app/chamados/reconciliador.py`. Referências: `app/chamados/services/fabrica_da_celula.py:210`, `app/chamados/services/fabrica_da_celula.py:245`, `app/chamados/management/commands/disparar_perguntas_competencia.py:115`, `app/ponto/services/cartorio.py:1008`, `app/chamados/reconciliador.py:1386`, `app/chamados/services/fabrica_da_celula.py:304`.

   A mudança alcança **escritores reais**, não apenas o contador: um veredito preenchido fora de `VEREDITOS_ACUSAM` deixa de bloquear incondicionalmente e bloqueia somente quando `dia_encerrado=True`. Exige ainda colaborador ativo, dia não futuro, dia não dispensado, vencimento do marco, falta lida da ata, ausência de barreira parcial e ausência de pergunta que ocupe o fato. Referências: `app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/fabrica_da_celula.py:112`, `app/chamados/services/fabrica_da_celula.py:119`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/fabrica_da_celula.py:126`, `app/chamados/services/fabrica_da_celula.py:142`, `app/chamados/services/fabrica_da_celula.py:160`.

   Portanto existe caminho executável em que passa a nascer pergunta antes bloqueada: não acusador + dia em curso + marco vencido faltante. A regra também vale para ontem noturno enquanto o dia do turno não encerrou. Se a ata não apresentar falta vencida, não nasce motivo. Nunca-bateu continua bloqueado antes desse ramo. Referências: `app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/fabrica_da_celula.py:119`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/fabrica_da_celula.py:58`, `app/chamados/services/fabrica_da_celula.py:64`, `app/chamados/services/fabrica_da_celula.py:69`.

   Esse alargamento cobre todos os vereditos preenchidos não acusadores, inclusive os administrativos que não estão na tupla acusadora; não está limitado a `concorde`. Referências: `app/chamados/services/fabrica_da_celula.py:120`, `app/escala/models.py:1759`, `app/escala/models.py:1767`.

   A emissão pela fábrica permanece sob silêncio de push: `perguntar` roda dentro de `push_disputa_silenciado`, e o emissor de push retorna imediatamente quando o contexto está ativo. Referências: `app/chamados/services/fabrica_da_celula.py:220`, `app/chamados/services/disputa_emissao.py:1742`, `app/chamados/services/disputa_emissao.py:1753`.

   A coleta de carimbadas pode agora ser alcançada nesses estados; seu desfazimento ocorre fora do bloco de silêncio, mas recebe `reabrir_fechada=False`, relê somente disputa aberta e executa `descarimbar`; os receivers de Pergunta observados agendam fechamento de beco e outbox, sem envio de push nesses corpos. Referências: `app/chamados/services/fabrica_da_celula.py:159`, `app/chamados/services/fabrica_da_celula.py:211`, `app/chamados/services/relavra_via.py:238`, `app/chamados/services/relavra_via.py:246`, `app/chamados/models.py:3203`, `app/chamados/signals.py:227`, `app/chamados/signals.py:250`.

2. **`dia_encerrado` é a autoridade registrada, não um segundo juiz introduzido pela 9d.** Registro explícito: pergunta “o dia está encerrado?” → `ponto/turnos.py::dia_encerrado`. Referência: `app/core/juizes.py:235`.

   O predicado efetivo é: não existe turno aberto ainda vivo **e** existe fim previsto **e** `agora >= fim`; sem previsto retorna falso. Turno aberto vivo pode expirar depois do fim da janela do template mais quatro horas; enquanto vivo, bloqueia o encerramento. Referências: `app/ponto/turnos.py:2000`, `app/ponto/turnos.py:2010`, `app/ponto/turnos.py:2024`, `app/ponto/turnos.py:2030`, `app/ponto/turnos.py:2115`, `app/ponto/turnos.py:2124`.

   O novo chamador passa `cel.data` e o `agora` recebido; `dia_encerrado_motivo` só chama `timezone.now()` se `agora is None`. O carregamento de turnos é filtrado pelo `data_turno` do dia solicitado. Referências: `app/chamados/services/fabrica_da_celula.py:123`, `app/ponto/turnos.py:2096`, `app/ponto/turnos.py:2098`, `app/ponto/turnos.py:2111`.

   Os cortes corrigidos autorizam esse juiz por fato encerrado e proíbem `data < hoje`; não exigem chamar outro juiz no seu lugar. O contrato admite instante ou fato encerrado. Referências: `app/docs/CORTES.md:55`, `app/docs/CORTES.md:62`, `CLAUDE.md:185`.

3. **Leitor com semântica diferente a destacar no item 6/8.** `pergunta_viva` trata não acusador como provisório pela **ata**: consulta `lastro.marcos_pendentes`, e, se não há pendente, retorna falso. A fábrica passa a decidir essa exceção pelo **encerramento geométrico/temporal do turno**. São critérios distintos em código, embora possam coincidir nos casos usuais. Referências: `app/chamados/juizes.py:1372`, `app/chamados/juizes.py:1385`, `app/chamados/juizes.py:1386`, `app/ponto/services/lastro.py:138`, `app/chamados/services/fabrica_da_celula.py:123`.

   Consequência dedutível: uma célula não acusadora, com falta vencida lida da ata, `dia_encerrado=False` e **sem `pendente` na ata** pode produzir motivo na fábrica enquanto o C6 considera a pergunta não viva. Existência desse estado no dado de produção: **[não verificado]**. Referências do caminho: `app/chamados/services/fabrica_da_celula.py:119`, `app/chamados/services/fabrica_da_celula.py:142`, `app/chamados/services/fabrica_da_celula.py:160`, `app/chamados/juizes.py:1386`.

   O leitor de faltas não consulta o veredito nesse corpo: lê `missing` da grade e converte os marcos faltantes; assim o veredito não elimina por si só essa possibilidade. Referências: `app/chamados/services/declaracao_texto.py:100`, `app/chamados/services/declaracao_texto.py:119`, `app/chamados/services/declaracao_texto.py:131`.

4. **Limites da conclusão normativa.** O aval ALVO fornecido manda incluir hoje no contador e deixar o teto no juiz, o que justifica corrigir o juiz compartilhado; autorização específica para todos os não acusadores administrativos e para a divergência fábrica/C6 acima não está demonstrada no texto fornecido. **[não verificado]**. A nova condição em si chama autoridade registrada e respeita o teto por instante antes de produzir motivos. Referências: `app/core/juizes.py:235`, `app/chamados/services/fabrica_da_celula.py:119`, `app/chamados/services/fabrica_da_celula.py:123`.

_3391 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
