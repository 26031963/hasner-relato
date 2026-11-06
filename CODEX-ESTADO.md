# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 19:55** · publicado em **06/11/2026 19:55** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 06/11 19:47 · sessao b8ba3deaea45 · - · - · origem=Ronald (tmux)

**Guardas e efeito em FechamentoMensal**

- O comando chama `_cerca` antes do escritor (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:160`). Ela exige competência **inicial** 11/2026 (`:109-112`) e consulta entrega de **cada dia de `data..data_fim`**, ou apenas `data` se fim vazio (`:98-102`, `:114-116`).
- Entrega significa **ExportacaoDominio vigente e não invalidada**, por empresa/competência; não consulta `FechamentoMensal.status` (`app/chamados/services/vigia_fio.py:838-844`; `app/folha/export.py:580-586`).
- **Escrita indireta certa:** depois do `Ausencia.save`, o escritor chama `invalidar_fechamento` (`app/ponto/services/ausencia.py:508-524`), que escreve `FechamentoMensal.derivado_em=None` e `FechamentoMensal.atualizado_em=timezone.now()` em todas as competências do intervalo cujo `derivado_em` ainda não é nulo (`app/ponto/services/fechamento.py:1633-1640`). Não há filtro por status/exportação nessa atualização (`:1636-1640`).
- **Furo de abrangência:** para fim vazio, edição usa `max(data,HOJE)` como término da invalidação (`app/ponto/services/ausencia.py:519-524`), enquanto a cerca consultou somente `data` (`comando:98-102`). Portanto, numa execução posterior ao corte de novembro, pode invalidar FechamentoMensal de outro mês já entregue, sem checagem dessa entrega (`ausencia.py:523`; `fechamento.py:1636-1640`). Na data de 06/11 e corte 21, esse prolongamento ainda termina na comp11 (`app/ponto/janelas.py:24-33`).
- A ausência **#5333** tem range até **28/12/2026**, portanto não cabe integralmente em 21/10–20/11; o comando pode mudar seus minutos se todos os dias passarem pela checagem de entrega (`comando:78`, `:114-116`). Sua invalidação alcança comp11, comp12 e comp01/2027 para corte21 (`ausencia.py:523-524`; `dia_pago.py:712-716`; `janelas.py:24-33`).
- **Concorrência:** trava-se somente a própria Ausencia (`comando:138-139`). Entrega é consultada por SELECT sem lock em ExportacaoDominio (`export.py:580-586`); não há prova de proteção contra exportação concorrente entre check e escrita (`comando:160-170`).

**Recalculo / DiaPago / ExportacaoDominio**

- A edição localizada invalida a projeção, em vez de chamar motor (`app/ponto/services/ausencia.py:510-524`). Seus receivers chamam cartório e reconciliadores (`app/ponto/signals.py:143-147`, `:254-272`); o cartório chama portas de lavra/carimbo (`app/ponto/services/cartorio.py:1988-1997`), que salvam CelulaDia e emitem EventoCelula (`app/ponto/portas/celula.py:894-902`, `:1033-1041`, `:1060-1067`). Não localizei escrita de DiaPago ou ExportacaoDominio nesse percurso; exaustividade de toda ramificação transitive `[nao verificado]`.
- Uma guarda genérica “recalculo não toca exportada” **não existe**: o comentário vigente registra que a recusa caiu e eventos podem recalcular exportadas (`app/ponto/services/fechamento.py:1774-1777`). A guarda temporária explícita é somente **09/2026** (`:36-43`), e não protege `invalidar_fechamento` (`:1633-1640`).

| escrita | arquivo:linha | guarda | reversível? |
|---|---|---|---|
| `FechamentoMensal.derivado_em`, `.atualizado_em` | `app/ponto/services/fechamento.py:1636-1640` | Colaborador + competências do intervalo + `derivado_em` não nulo; sem entrega/status | Rollback da tentativa acompanha atomic do comando (`comando:137`, `:175-187`); snapshot não guarda valores antigos de FM (`comando:120-123`), portanto `--reverter` não os restaura. |
| `DiaPago` | `[nao verificado]` | Não localizei chamada ao lavrador/recalculo no percurso acima | `[nao verificado]` |
| `ExportacaoDominio` | `app/folha/export.py:580-586` | Nesse percurso é somente leitura para a cerca | Não aplicável à consulta. |

## 06/11 19:46 · sessao c5729b08d142 · - · - · origem=Ronald (tmux)

Auditoria estática; não executei `apply`, reversão nem testes. [nao verificado]

- **Snapshot anterior ao escritor:** `diario(_foto(...))` ocorre antes de `editar_ausencia_registro` (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:165`, `:169`). Caminho: `<settings.BASE_DIR>/logs/h12h06b/snapshot_apply_<YYYYmmdd_HHMMSS_microsegundos>.jsonl` (`:215`, `:217`, `:250`), criado exclusivamente com `open(...,'x')` (`:251`); cada registro tem `flush` e `fsync` (`:255-257`). Foto contém identidade, minutos anterior/novo e atualizado_em (`:120-123`).
- **Confirmação posterior:** confirmação com `atualizado_em` e ID da trilha sai depois do retorno da transação (`:189-190`, `:273-274`). Reversão considera apenas fotos confirmadas e PKs pertencentes à lista (`:229-242`); exige identidade, minutos esperados, carimbo e ausência de trilha posterior (`:142-159`).
- **Reversibilidade limitada:** falta não pode restaurar 720 pelo escritor existente: veto `tipo_nao_parcial` (`app/ponto/services/ausencia.py:480-484`). Teste documenta que atestado volta, falta permanece derivada (`app/ponto/tests/test_h12h06b_rederivar_minutos.py:356-367`); há `expectedFailure` específico da reversão da falta (`:371-378`). Snapshot não restaura diretamente campos de modelos indiretos; plano manda somente os minutos ao mesmo escritor (`comando:238-242`, `:169-170`).
- **Trilha completa do ato de Ausencia:** ação `editar`, modelo, PK, descrição fixa H12H06-B com motivo, antes/depois de minutos e usuário `--por` (`comando:182-185`). Usuário deve existir (`:209-214`). `core.observ.evento` cria `LogAuditoria.usuario, acao, modelo, objeto_id, descricao, valor_antes, valor_depois, ip, ator` (`app/core/observ.py:118-137`); `criado_em` automático (`app/core/models.py:307`). Trilha ausente provoca rollback da linha (`comando:186-192`). Snapshot não contém autor/motivo (`:120-123`).
- **Idempotência condicionada:** linha cuja identidade continua igual e minutos já são o alvo retorna antes do snapshot, escritor e evento (`comando:142-148`, `:165`, `:169`, `:182`). Prova de teste com duas fixtures: zero aplicadas, dois já aplicados, carimbos/eventos intactos (`test_h12h06b_rederivar_minutos.py:131-143`). **“2ª rodada escreve zero” literal é falso:** todo `--apply` ainda cria snapshot e resultado em disco (`comando:248-259`, `:280-287`). Zero saves/eventos vale para as linhas já corretas, não para tentativas anteriores recusadas/puladas (`:147-151`, `:165-172`). [nao verificado]
- **Contadores:** `RESUMO aplicadas=N ja_aplicadas=N mudaria=N puladas=N recusadas=N` (`comando:288-289`); não existe contador explícito “lidos”. Soma desses contadores = tentativas concluídas (`:264-272`). Por linha imprime PK/colaborador/tipo/data/resultado/antes/depois/motivo (`:275-276`). JSON resultado tem `linhas`, `conta` e `completo` (`:283-286`). `SNAPSHOT ... sha256=... linhas=N` (`:282`) conta registros JSONL (`:259`), incluindo foto e confirmação (`:165`, `:274`): não é quantidade de ausências. Em exceção, `finally` grava resultado parcial (`:277-287`), mas o resumo final pode não ser atingido (`:288`). [nao verificado]
- **Universo:** plano usa somente `LISTA` (`comando:244`); nela há 30 atos/22 colaboradores, 18 atestados/12 faltas e estados 16 aprovadas/11 aguardando_documento/3 aguardando_decisao, contagem derivada de `:50-81`. Logo não é prova de frota inteira; não enumera empresas/colaboradores/ausências omitidos (`:138-139`, `:244`). Não há filtro por empresa ou situação ativo/desligado no lookup nem no escritor (`comando:138-146`; `ausencia.py:474-509`). Empresa efetiva só alimenta a cerca da competência (`comando:109-116`).
- **Ausência fora da janela existe na própria lista:** PK5333 inicia 30/10 e termina **28/12/2026** (`comando:78`). Cerca exige apenas competência inicial 11/2026 e verifica exportação de cada dia, sem exigir fim até20/11 (`:110-116`). Portanto pode alterar uma Ausencia cujo range extrapola21/10–20/11 se os meses alcançados estiverem não entregues (`:98-102`, `:110-117`). Teste só prova veto quando o range alcança mês entregue (`test_h12h06b_rederivar_minutos.py:334-343`). [nao verificado]
- **Outra extrapolação relevante:** escritor invalida fechamento até hoje quando `data_fim=None`, mesmo para ato de um único dia (`app/ponto/services/ausencia.py:519-524`), embora a cerca do comando examine somente `data` nesse caso (`comando:98-102`). A análise das guardas do fechamento está com o agente de competências.


## 06/11 19:43 · sessao 340537c22241 · ESMERIL-SEM-DIAS-PREVISTOS · censo · origem=Code

**O fechamento atual conta o rótulo da ata, sem exigir `CelulaDia.trabalha=True`. Há outro significado documentado — “todo dia que a célula diz trabalho” — cujo selo consta como não iniciado.** (`app/ponto/services/fechamento.py:1030-1033`; `app/docs/BACKLOG.md:1365`.)

Os dois valores gravados `15=8+7` e `15=9+6` são informação fornecida na pergunta; o anexo não contém a sonda diária que permita reproduzi-los ou demonstrar qual código os gravou. **[nao verificado]**

**1. Significados encontrados**

Há três contagens distintas com o mesmo nome:

- **`FechamentoMensal.dias_previstos`:** quantidade de linhas da grade cujo `tipo_dia` seja `trabalho` ou `ausencia`, inclusive com zero minutos previstos; não consulta `trabalha`. Se a grade falhar, ou todos os dias dentro do vínculo forem `sem_celula`, recebe `None`. (`app/ponto/services/fechamento.py:1027-1033`, `app/ponto/services/fechamento.py:1067-1083`.)
- **`causa_espelho['dias_previstos']`:** quantidade de linhas com `tipo_dia=='trabalho'`; exclui todas as ausências. Esse dict é materializado no fechamento e transportado para a tela. (`app/folha/causas.py:39-45`; `app/ponto/services/fechamento.py:1061-1064`; `app/ponto/services/fechamento_lista.py:120-122`.)
- **Proposta de escala `['dias_previstos']`:** quantidade de células `trabalha=True`, recortadas aos dias atribuídos ao vínculo analisado; não deriva do fechamento mensal. (`app/escala/services/propositor.py:484-488`, `app/escala/services/propositor.py:511`, `app/escala/services/propositor.py:559`.)

O rótulo do campo é “Dias de trabalho previstos”, mas modelo e migration não estabelecem o predicado da contagem. (`app/ponto/models.py:446`; `app/ponto/migrations/0044_fechamentomensal_minutos_previstos.py:21-25`.)

**2. Quem transforma folga em ausência e quem conserva a distinção**

A cadeia semântica é:

1. A cobertura de ausência expande os dias do período, sem filtro de trabalho/folga; para folha, a cobertura exige ausência aprovada. (`app/ponto/turnos.py:2514`, `app/ponto/turnos.py:2753-2766`.)
2. O builder monta a folga com `minutos_previstos=0`; depois aplica o overlay de ausências. (`app/escala/utils.py:1822-1823`, `app/escala/utils.py:1865-1869`.)
3. **`_marcar_ausencias` é o escritor semântico do rótulo:** aceita cobertura sobre `folga` quando não é `falta` e escreve `tipo_dia='ausencia'`. Esse trecho não aumenta os minutos previstos. (`app/escala/utils.py:1231-1244`.)
4. O cartório monta essa grade, copia `tipo_dia` e minutos para `ata_do_dia`, e entrega a ata à lavra nos ramos normal e `nunca_bateu`. (`app/ponto/services/cartorio.py:1126`, `app/ponto/services/cartorio.py:704-706`, `app/ponto/services/cartorio.py:1210`, `app/ponto/services/cartorio.py:1242-1247`, `app/ponto/services/cartorio.py:1800-1807`.)
5. A escrita efetiva de `cel.ata` pertence à porta `lavrar_veredito`. (`app/ponto/portas/celula.py:986-1003`, `app/ponto/portas/celula.py:1028-1035`.)

O juiz de precedência põe ausência acima de folga, mas conserva separadamente `era_dia_previsto`, obtido de `cel.trabalha` quando preenchido. Seu resultado é gravado em **`ata.precedencia`**; esse caminho não é a atribuição direta de **`ata.tipo_dia`**, feita pelo builder/cartório descrito acima. (`app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:347-363`, `app/ponto/precedencia.py:379-407`; `app/ponto/services/cartorio.py:1947-1956`.)

**Sim, há leitores que distinguem as duas situações:**

- `grade_da_celula` preserva o tipo da ata e seus minutos. Se os minutos estiverem ausentes, só consulta o previsto quando `cel.trabalha` for verdadeiro; caso contrário, usa zero. Portanto, **não revalida minutos já lavrados contra `trabalha=False`**. (`app/escala/services/leitor_celula.py:380-405`.)
- O juiz de minutos retorna zero quando `eh_dia_trabalho(...) is not True`; esse método lê `cel.trabalha` quando existe célula. (`app/escala/utils.py:2281-2288`; `app/escala/models.py:1087-1096`.)
- A grade transporta explicitamente o par `trabalha + ata`; `dia_sem_trabalho` consulta `trabalha is False`. Na ausência parcial, trabalho permite cobrar o restante e folga cala a cobrança. (`app/escala/services/leitor_celula.py:408-411`; `app/chamados/juizes.py:422-428`.)
- `por_dia_da_grade` inclui `trabalho/ausencia`, mas calcula valores por **minutos**: previsto do dia, realizado limitado ao previsto e abono da parte descoberta. Uma ausência com previsto zero contribui zero nessas somas, para realizado não negativo. (`app/ponto/services/dia_pago.py:384-400`.)

**3. Dinheiro: efeito de mudar somente a contagem**

**Mudar apenas o cálculo de `_dias_prev` não altera DSR, falta, abono, banco ou eventos monetários do TXT pelos caminhos encontrados.** Esses cálculos usam outras datas e minutos; a leitura funcional direta de `FechamentoMensal.dias_previstos` está no teste de zero da prontidão. (`app/ponto/services/fechamento.py:722-731`, `app/ponto/services/fechamento.py:1032-1048`; `app/folha/export.py:1042-1043`.)

- **DSR e semanas OK/perdidas:** recebem `datas_previstas_trabalho`, construídas por `esc.eh_dia_trabalho` e exclusão das datas suprimidas. O motor avalia dias obrigatórios sem trabalho e sem justificativa; não usa a cardinalidade `trabalho+ausencia` da grade. (`app/ponto/services/fechamento.py:674-692`, `app/ponto/services/fechamento.py:697-731`; `app/ponto/motor_calculo_v2.py:2593-2630`, `app/ponto/motor_calculo_v2.py:2655`, `app/ponto/motor_calculo_v2.py:3452-3479`.)
- **Reflexo DSR:** usa HE semanal, dias efetivamente trabalhados e dias do período. (`app/ponto/motor_calculo_v2.py:2636-2653`, `app/ponto/motor_calculo_v2.py:3463-3477`.)
- **Desconto de falta:** o `horas_falta` final do fechamento soma `minutos_de_falta_por_dia`; o export expande e rateia ausências que descontam. (`app/ponto/services/fechamento.py:864-866`, `app/ponto/services/fechamento.py:987`; `app/folha/export.py:114-143`, `app/folha/export.py:169-185`.)
- **Proporcionalidade do banco:** já exige `eh_dia_trabalho(...) is True`, contando previstos da semana e a parcela dentro do período. (`app/ponto/motor_calculo_v2.py:3425-3429`.)
- **Prontidão:** redução de uma contagem positiva para outra positiva não muda percentual; o percentual usa minutos. Se a nova contagem chegar a zero com minutos positivos, muda para `cego=True`. O fallback repete `trabalho+ausencia` e também usa a contagem apenas no teste zero. (`app/folha/export.py:1042-1072`, `app/folha/export.py:1087-1105`.)
- **Tela:** faixa e ordenação consomem prontidão; o tique de entrada na folha vem separadamente do juiz do export. (`app/ponto/services/fechamento_lista.py:85-102`, `app/ponto/services/fechamento_lista.py:111-130`.)
- **TXT/PDF do export:** os eventos são enumerados sem `dias_previstos`; faltas seguem suas datas próprias. O PDF traduz o conteúdo do TXT gravado. (`app/folha/export.py:439-461`, `app/folha/export.py:722-736`; `app/folha/pdf_export.py:63-78`.)

**Alterar o filtro `_dt_trab` inteiro tem alcance maior que alterar somente a contagem:** ele também alimenta `minutos_previstos` e `dias_incertos`; os mapas de `DiaPago` possuem filtro próprio. Assim, a primeira mudança pode alterar o fechamento sem alterar os mapas diários correspondentes. (`app/ponto/services/fechamento.py:1030-1048`; `app/ponto/services/dia_pago.py:385-399`.)

Para dias retirados com previsto zero, a soma dos previstos permanece igual; `dias_incertos` pode cair se houver `sem_ancora`. Exigir estritamente `trabalha=True` também exclui `trabalha=None`, além das folgas. Quantidades e impacto nos dois colaboradores reais: **[nao verificado]**, pois faltam as linhas diárias no dado fornecido. (`app/ponto/services/fechamento.py:1032-1034`; `app/escala/models.py:1070-1072`.)

**4. Histórico e testes** [nao verificado]

`git log -S` não pôde ser consultado: o comando retornou `fatal: not a git repository`. Commit e data de introdução efetiva: **[nao verificado]**. O ambiente é documentado como cópia por `git archive`. (`CLAUDE.md:341`; `bin/codex_pergunta.sh:26`.)

Há duas pistas documentais, que **não substituem o histórico Git**:

- O comentário do fechamento atribui a **03/08** a inclusão de ausências para evitar encolher o denominador da prontidão. (`app/ponto/services/fechamento.py:999-1003`.)
- O overlay registra **13/09** para ausência vencer folga; existe teste dessa classificação. A combinação desse overlay com o filtro atual explica mecanicamente a inclusão da ausência em folga na contagem. (`app/escala/utils.py:1227-1241`; `app/escala/tests/test_grade_segue_a_precedencia.py:43-46`; `app/ponto/services/fechamento.py:1030-1033`.)

O documento O155 estabelece o significado baseado na célula e registra o selo como não iniciado; CORTES também registra o selo não construído. (`app/docs/BACKLOG.md:1365`; `app/docs/CORTES.md:200`.)

Referências de testes usadas na tabela abaixo — execução nesta auditoria: **[nao verificado]**: [nao verificado]

- **T1:** `app/escala/tests/test_grade_segue_a_precedencia.py::GradeSegueAPrecedenciaTest.test_MORDE_atestado_aprovado_em_dia_de_folga_e_ausencia`, fixa o rótulo ausência; `::test_falta_aprovada_em_dia_de_folga_segue_folga`, fixa a exceção da falta. (`app/escala/tests/test_grade_segue_a_precedencia.py:43-50`.)
- **T2:** `app/escala/tests/test_leitor_celula_minutos_pela_autoridade.py::LeitorCelulaMinutosPelaAutoridadeTest.test_celula_de_folga_preve_zero` e `::test_minutos_lavrados_na_ata_vencem`, fixam fallback zero e prevalência dos minutos lavrados. (`app/escala/tests/test_leitor_celula_minutos_pela_autoridade.py:45-51`.)
- **T3:** `app/ponto/tests/test_o48_sem_celula_nao_e_zero.py::SemCelulaNaoEZero.test_MORDE_sem_nenhuma_celula_o_objeto_nao_diz_zero_minuto_de_quem_trabalhou`, `::test_MORDE_admitido_no_meio_sem_celula_no_vinculo_tambem_nao_diz_zero` e `::test_fora_do_vinculo_INTEIRO_e_RESPOSTA_zero_nunca_silencio`, fixam `None` versus zero, sem resolver ausência em folga. (`app/ponto/tests/test_o48_sem_celula_nao_e_zero.py:96-116`, `app/ponto/tests/test_o48_sem_celula_nao_e_zero.py:153-207`.)
- **T4:** `app/ponto/tests/test_retrato_exportada.py::PorDiaDaGradeTest.test_MORDE_as_tres_leis`, fixa mapas de minutos por tipo, sem fornecer `trabalha`. (`app/ponto/tests/test_retrato_exportada.py:46-58`.)
- **T5:** `app/ponto/tests/test_fechamento_ausencia.py::test_atestado_semana_nao_perde_dsr` e `::test_afastamento_suprime_previsto`, fixam efeitos de ausência no DSR. (`app/ponto/tests/test_fechamento_ausencia.py:126-164`.)
- **T6:** `app/ponto/tests/test_o208_contador_do_recalculo.py::ContadorDoRecalculoTest.test_MORDE_os_cinco_campos_da_divergencia`, fixa visibilidade da mudança do campo, sem fixar sua derivação. (`app/ponto/tests/test_o208_contador_do_recalculo.py:30-35`, `app/ponto/tests/test_o208_contador_do_recalculo.py:71`.)
- **T7:** `app/folha/tests/test_porta_do_export.py::test_MORDE_o_contador_do_fechamento_NAO_e_cego_a_campo_de_pagamento`, exclui contagens do censo de pagamento. (`app/folha/tests/test_porta_do_export.py:454-481`.)

Teste que confronte diretamente o valor mensal de `dias_previstos` para `ausencia/trabalha=False` contra `ausencia/trabalha=True`: **[nao verificado: não localizado]**. [nao verificado]

Na tabela, “snapshot” significa que o sítio transporta/restaura o número recebido sem decidir o que constitui dia previsto; o comportamento está nos trechos citados em cada linha.

| Sítio | Escritor/leitor | Significado suposto | Dinheiro? | Teste que fixa | [nao verificado]
|---|---|---|---|---|
| `app/ponto/models.py:446`; `app/ponto/migrations/0044_fechamentomensal_minutos_previstos.py:21-25` | Declaração do campo | “Dias de trabalho previstos”; aceita `NULL` | Sem fórmula | T3 fixa parte da nulidade |
| `app/escala/models.py:1087-1096` | Alimentador: `eh_dia_trabalho` | Célula; sem ela, cálculo do vínculo | Alimenta outros cálculos financeiros | Não fixa a contagem mensal `[nao verificado]` |
| `app/escala/utils.py:1822-1831`, `app/escala/utils.py:1865-1869`, `app/escala/utils.py:1231-1244` | Escritor da grade julgada | Folga conserva previsto zero; ausência não-falta vence seu rótulo | Alimenta minutos | T1 |
| `app/ponto/precedencia.py:347-407`; `app/ponto/services/cartorio.py:1947-1956` | Juiz e lavra de `ata.precedencia` | Ausência vence folga; conserva `era_dia_previsto` separado | Não usa a contagem mensal | `app/ponto/tests/test_a2_precedencia.py::FolgaDeTemplateSuprimidaTest.test_MORDE_a_ata_da_celula_diz_que_a_folga_foi_suprimida` (`:466-481`) |
| `app/ponto/services/cartorio.py:704-706`, `app/ponto/services/cartorio.py:1242-1247`, `app/ponto/services/cartorio.py:1800-1807`; `app/ponto/portas/celula.py:1028-1035` | Escritor da ata | Copia tipo e minutos julgados | Alimenta grade/minutos | T1 fixa a classificação de origem |
| `app/escala/services/leitor_celula.py:380-411`; `app/folha/export.py:329-338` | Leitor da ata / grade do fechamento | Tipo da ata vence; minutos lavrados vencem; fallback considera `trabalha` | Alimenta previsto e abono | T2 |
| `app/ponto/services/fechamento.py:1027-1069` | **Escritor semântico de FM** | **Número de linhas `trabalho+ausencia`**, sem exigir `trabalha=True` | Contagem; somas vizinhas são minutos | T3; caso central não localizado `[nao verificado]` |
| `app/ponto/services/fechamento.py:1071-1083` | Escritor de ausência de resposta | `None` por falha/grade sem célula | Também anula minutos | T3 |
| `app/ponto/services/fechamento.py:1121-1128`, `app/ponto/services/fechamento.py:1144-1173` | Persistência por `save`/`update` | Valor derivado acima | Persiste também rubricas | Sem teste semântico específico `[nao verificado]` |
| `app/ponto/services/fechamento.py:881-886`, `app/ponto/services/fechamento.py:1118-1120`, `app/ponto/services/fechamento.py:1743-1745` | Leitura destacada / dict / `fechamento_lido` | Mesma derivação; copia a linha e retorna objeto ou `CAMPOS` | Transporta rubricas também | T3 |
| `app/ponto/services/fechamento.py:1319-1324` | Escrita integral ao aprovar | Preserva o valor já carregado | Carimbo; não recalcula | `[nao verificado]` |
| `app/ponto/services/fechamento.py:1342-1381` | **Escritor genérico de restauração** | Snapshot recebido; aceita `dias_previstos` | Pode restaurar campos financeiros | `[nao verificado]` |
| `bin/restore_relavratura_10_2026.py:34`, `bin/restore_relavratura_10_2026.py:79-81`; `bin/restore_o263_comp10.py:27`, `bin/restore_o263_comp10.py:48-50` | **Escritores diretos adicionais**, por `setattr/save` | Snapshot; não passam por `restaurar_fechamento` | Restauram rubricas também | `[nao verificado]` |
| `app/folha/export.py:1037-1072` | Leitor direto: prontidão | Contagem diferente de zero habilita denominador em minutos | Indicador; não rubrica | Sem caso central `[nao verificado]` |
| `app/folha/export.py:1087-1105` | Escritor/leitor local: fallback `ndias` | Repete `trabalho+ausencia`; testa zero | Indicador em minutos | Sem caso central `[nao verificado]` |
| `app/ponto/services/fechamento_lista.py:85-130` | Tela, faixa e ordenação | Consome prontidão; transporta causa | Apresentação | Não fixa o significado `[nao verificado]` |
| `app/ponto/services/dia_pago.py:384-400`, `app/ponto/services/dia_pago.py:343-358` | Deriva e lavra mapas diários | Mesmo universo `trabalho+ausencia`, mas **minutos**, sem `dias_previstos` | **Sim:** previsto/realizado/abono | T4; T7 exclui contagens |
| `app/ponto/services/dia_pago.py:99-100`, `app/ponto/services/dia_pago.py:403-424` | Retrato da exportada | Transporta três campos de minutos; contagem não integra `DiaPago` | **Sim**, pelos minutos | T4/T7 |
| `app/folha/causas.py:32-45`; `app/ponto/services/fechamento.py:1061-1064` | Escritor do dict homônimo | **Só `tipo_dia=='trabalho'`**; zero sem grade/isento | Diagnóstico | Assert específico da chave `[nao verificado]` |
| `app/escala/services/propositor.py:484-488`, `app/escala/services/propositor.py:511`, `app/escala/services/propositor.py:559` | Escritor do dict homônimo | **Células `trabalha=True`**, dentro dos dias do vínculo | Diagnóstico/proposta | Assert específico da chave `[nao verificado]` |
| `app/ponto/services/fechamento.py:697-731`; `app/ponto/motor_calculo_v2.py:2593-2655`, `app/ponto/motor_calculo_v2.py:3452-3479` | Outro conjunto de previstos / leitor DSR | Dias obrigatórios por célula/vínculo, descontadas supressões | **Sim**, independente da contagem FM | T5 |
| `app/ponto/motor_calculo_v2.py:3425-3429` | Proporcionalidade | `eh_dia_trabalho is True`, semana/período | **Sim**, banco | Teste específico desta pergunta `[nao verificado]` |
| `app/folha/export.py:114-143`, `app/folha/export.py:169-185`; `app/ponto/services/fechamento.py:864-866` | Falta decidida/rateio | Dias e minutos da ausência que desconta | **Sim**, independente da contagem FM | Não fixa `dias_previstos` `[nao verificado]` |
| `app/folha/export.py:439-461`, `app/folha/export.py:722-736`; `app/folha/pdf_export.py:63-78` | TXT e PDF do export | Eventos/datas próprias; PDF lê TXT | **Sim**, sem usar essa contagem | `app/folha/tests/test_export_dsr_reflexo.py::test_dsr_nao_e_emitido` (`:32-36`) |
| `app/ponto/management/commands/aplicar_09_corte_b.py:67-74`, `app/ponto/management/commands/aplicar_09_corte_b.py:184-185`, `app/ponto/management/commands/aplicar_09_corte_b.py:220`, `app/ponto/management/commands/aplicar_09_corte_b.py:266` | Snapshot/diff/restauração | Valor de `CAMPOS`, sem interpretação | Controle/restauração de vários campos | T6 cobre visibilidade |
| `app/ponto/management/commands/aplicar_09_he_intra.py:43-49`, `app/ponto/management/commands/aplicar_09_he_intra.py:125-150` | Leitor de snapshot | Preserva número; `dias_previstos` fica fora do alvo escrito | Controle | `[nao verificado]` |
| `app/ponto/management/commands/recalcular_fechamento.py:34-71`, `app/ponto/management/commands/recalcular_fechamento.py:142-148`; `app/ponto/services/diff_frota.py:62-63`, `app/ponto/services/diff_frota.py:84-88`, `app/ponto/services/diff_frota.py:111-125` | Foto/diff genéricos | Número recebido | Medição | T6 |
| `app/ponto/management/commands/diff_janela_he_total.py:87-115`; `app/ponto/management/commands/diff_passivo_s84.py:42-50`, `app/ponto/management/commands/diff_passivo_s84.py:87-88`; `app/ponto/management/commands/diff_col900.py:38-45`, `app/ponto/management/commands/diff_col900.py:74-78` | Leitores por `CAMPOS`/diff | Snapshot | Medição | `[nao verificado]` |
| `app/ponto/management/commands/carimbo_gravado.py:34-56`, `app/ponto/management/commands/carimbo_gravado.py:87-95`; `app/ponto/management/commands/aplicar_janela_he_total.py:65-88` | Foto e comparação indireta | Snapshot | Auditoria | `[nao verificado]` |
| `app/ponto/management/commands/lavrar_dias_pagos.py:68-78`; `app/ponto/portas/regen_exportada.py:56-84` | Hash e valores do gravado | Incluem o campo dinamicamente | Auditoria/Pauta/retificação | `[nao verificado]` |
| `app/ponto/management/commands/e6_piso_autopsia.py:120-124` | Impressão direta | Valor gravado | Diagnóstico | `[nao verificado]` |
| `bin/snapshot_relavratura_10_2026.py:29-31`; `bin/snapshot_o263_comp10.py:54-58`; `bin/aplicar_o263_comp10.py:28-37`, `bin/aplicar_o263_comp10.py:51-65` | Snapshot/recalculo/comparação | Valor gravado ou derivado pela porta | Medição de vários campos | `[nao verificado]` |
| `bin/diff_o118_col221.py:23-38`; `bin/diff_o119_frota.py:27-50`; `bin/diff_o124_frota.py:31-42`, `bin/diff_o124_frota.py:55-60` | Diffs genéricos | Snapshot, sem interpretação | Medição | `[nao verificado]` |
| `bin/relavrar_o118_col221.py:26-37`, `bin/relavrar_o118_col221.py:49-58`; `bin/relavrar_o119_col221.py:25-36`, `bin/relavrar_o119_col221.py:48-57`; `bin/relavrar_o120_col221_09.py:29-49`, `bin/relavrar_o120_col221_09.py:62-79` | Snapshot/hash/impressão; acionam recálculo | Preservam ou leem derivação canônica | Operação sobre fechamento completo | `[nao verificado]` |
| `app/relatorios/test_espelho_falta.py:28-41` | Helper de teste `_dias_previstos` | `ec.eh_dia_trabalho` | Fixture de falta/abono | `::EspelhoFaltaAbonoTest.test_falta_e_abono_surfam_no_resumo` verifica falta/abono

[truncado em 20000 de 20663 caracteres]

## 06/11 19:43 · sessao 92a8abebbd5b · ESMERIL-SEM-INSTANTE · censo · origem=Code

O filtro de fatias corta o **timestamp cru**; o motor, depois, recorta períodos pelo **dia da jornada**. São critérios diferentes (`app/ponto/services/fechamento.py:245`, `app/ponto/services/fechamento.py:252`, `app/ponto/motor_calculo_v2.py:2403`, `app/ponto/motor_calculo_v2.py:2429`).

A correspondência da árvore com o hash informado permanece **[nao verificado]**: o snapshot disponível não contém metadados Git.

**1. Quem escreve `_meus`, `fatia_ini` e `fatia_fim`**

| Variável | Escritor e origem |
|---|---|
| `fatia_ini`, inicialmente | `max(ini, esc.data_inicio, DATA_INICIO_OPERACAO)` — `app/ponto/services/fechamento.py:204`. |
| `fatia_fim`, inicialmente | `min(fim, esc.data_fim if esc.data_fim else fim)` — `app/ponto/services/fechamento.py:205`. |
| `_dono_do_dia` | Percorre as chaves `(colaborador_id, data)` de `celulas`; quando existe geradora e `vinculo_do_dia` responde, associa essa data ao vínculo resolvido — `app/ponto/services/fechamento.py:189-193`. |
| `_meus`, inicialmente | Datas de `_dono_do_dia` pertencentes a `esc.pk`, dentro dos limites iniciais — `app/ponto/services/fechamento.py:207-208`. |
| `_meus`, complemento | Acrescenta dias sem dono cobertos por exatamente um vínculo, quando há `_julgados` — `app/ponto/services/fechamento.py:221-231`. |
| Limites finais | Havendo `_julgados`, pula a fatia sem `_meus`; caso contrário, escreve `fatia_ini, fatia_fim = _meus[0], _meus[-1]` — `app/ponto/services/fechamento.py:232-236`. |
| Limites de busca | Derivados dos limites finais por −1/+1 dia — `app/ponto/services/fechamento.py:242-243`. |

O significado das fontes é:

- **Célula:** a chave vem de `CelulaDia.data`, um `DateField`; o carregador monta `{(c.colaborador_id, c.data): c}` (`app/escala/models.py:1685`, `app/escala/alimentacao.py:73-78`). Representa o **dia nominal do turno previsto, ancorado no calendário local**: o gerador declara instantes locais e coloca os marcos após meia-noite em D+1, mantendo a data nominal D (`app/ponto/management/commands/gerar_celulas.py:36-40`, `app/ponto/management/commands/gerar_celulas.py:67-69`).
- **Vigência:** `esc.data_inicio` e `esc.data_fim` são `DateField`, datas cadastrais comparadas ao dia da célula, sem conversão de instante; a geradora fora da vigência é recusada (`app/escala/models.py:875-876`, `app/escala/alimentacao.py:269-284`). A interpretação é de calendário cadastral/turno, não de dia UTC — inferência dessas comparações.
- **Piso operacional:** `datetime.date(2026, 5, 21)`, uma data fixa, sem instante ou fuso (`app/core/constants.py:5`).
- **`ini/fim` do chamador:** são limites de calendário, com as origens abaixo.

| Chamador | Origem de `ini/fim` |
|---|---|
| Fechamento | `janela_fechamento(mes, ano, empresa)`; constrói início/fim aware locais e passa `inicio.date(), fim.date()` (`app/ponto/services/fechamento.py:612-614`, `app/ponto/services/fechamento.py:745`). A janela nasce de `datetime.date` pelo corte da empresa (`app/ponto/janelas.py:15-21`, `app/ponto/janelas.py:54-57`). |
| Espelho | `autoridade_do_periodo` recebe e repassa `ini/fim` (`app/ponto/services/espelho.py:971`, `app/ponto/services/espelho.py:1164`). Na tela, recebe `min(piso_visual, apur_ini)` até `hoje` (`app/ponto/services/espelho.py:1225-1227`); `hoje` é data civil local e a competência vem de `janela_fechamento` (`app/ponto/views.py:97-98`, `app/ponto/views.py:153-158`). |
| PDF | Período livre: datas recebidas do chamador (`app/relatorios/pdf_espelho.py:1038-1046`). Mensal: janela da competência (`app/relatorios/pdf_espelho.py:661-663`, `app/relatorios/cartao_pela_celula.py:21-24`). Antes de apurar, limita o fim a ontem local, preservando o piso `data_ini` (`app/relatorios/pdf_espelho.py:285-288`, `app/relatorios/cartao_pela_celula.py:37-41`). |

**2. Quem escreve as listas e qual o `tzinfo`**

| Chamador | Construção da lista passada à apuração |
|---|---|
| Fechamento | `batidas = list(_bap(colab, inicio_busca, fim_busca))`; busca de início local 00:00−1 dia até fim local 23:59:59+1 dia (`app/ponto/services/fechamento.py:612-624`). Passagem em `app/ponto/services/fechamento.py:745`. |
| Espelho | `batidas = list(batidas_apuraveis(colaborador, _lo, _hi))`; mesmo envelope local de ±1 dia (`app/ponto/services/espelho.py:1019-1022`). Passagem em `app/ponto/services/espelho.py:1164`. |
| PDF | `batidas_periodo = list(_bap(colaborador, inicio, fim))`; início pedido 00:00−12h até fim pedido 00:00+1d12h (`app/relatorios/pdf_espelho.py:264-272`). Passagem em `app/relatorios/pdf_espelho.py:419`. A carga acontece **antes** do teto em ontem (`app/relatorios/pdf_espelho.py:287`). |

Nos três caminhos, `batidas_apuraveis` retorna objetos do ORM: consulta `Batida.objects`, exclui retratadas, aplica limites de timestamp/colaborador e ordena por timestamp; não localiza nem substitui os timestamps (`app/ponto/turnos.py:2863-2871`).

`Batida.timestamp` é `DateTimeField`; a base configura `USE_TZ=True` e `TIME_ZONE='America/Sao_Paulo'`; produção herda essa base e usa PostgreSQL (`app/ponto/models.py:23`, `app/config/settings/base.py:93-95`, `app/config/settings/prod.py:1`, `app/config/settings/prod.py:9-17`).

**O esperado nesse fluxo ORM é timestamp aware em UTC**, coerente com o contrato documentado de banco UTC (`app/docs/LEIS.md:107`). A classe concreta do `tzinfo` e os objetos efetivamente retornados em execução permanecem **[nao verificado]**; não houve consulta ao banco.

**3. Censo das expressões que extraem data**

Na tabela, **local** significa o fuso corrente do Django, cujo padrão configurado é `America/Sao_Paulo` (`app/config/settings/base.py:93`). `.date()` cru usa a data já representada no objeto; portanto, no fluxo ORM UTC esperado acima, produz dia UTC, sem conversão local (`app/ponto/services/fechamento.py:245`).

| Local | Forma | Dia produzido |
|---|---|---|
| `app/ponto/services/fechamento.py:245` | `b.timestamp.date()` | Do timestamp cru; **UTC no fluxo ORM esperado**. |
| `app/ponto/services/fechamento.py:252` | `b.timestamp.date()` | Mesmo critério; pertencimento a `_ok`. |
| `app/ponto/services/fechamento.py:253` | `b.timestamp.date()` — **duas ocorrências** | Mesmo critério; comparação com os limites da fatia. |
| `app/ponto/services/fechamento.py:784` | `timezone.localtime(b.timestamp).date()` | **Civil local**, para dias com batida. |
| `app/ponto/services/fechamento.py:830` | `timezone.localtime(_p_orf.entrada).date()` | **Civil local da entrada** do período órfão. |
| `app/ponto/services/fechamento.py:926` | `timezone.localtime(p.entrada).date()` | **Civil local da entrada**, na consulta de escala certa. |
| `app/ponto/services/fechamento.py:938` | `timezone.localtime(_p_f.entrada).date()` | **Civil local da entrada**, na classificação de feriado. |
| `app/ponto/services/fechamento.py:1553` | `timezone.localtime(quando).date()` se aware; senão `quando.date()` | Aware: **local**. Naive: data crua, sem fuso demonstrável. |
| `app/ponto/services/espelho.py:84` | `timezone.localtime(p.entrada).date()` | **Civil local da entrada**, no recorte de `somar_periodos`. |
| `app/ponto/services/espelho.py:426` | `timezone.localtime(p.entrada).date()` | **Civil local da entrada**, no contador de turnos da competência. |

`_so_a_data(quando)` também alcança timestamps de batida: registro/retratação envia o timestamp ao evento, que resolve a competência por `competencia_do_fato` → `competencias_do_fato` → `_so_a_data` (`app/ponto/registro_batida.py:160`, `app/ponto/registro_batida.py:281`, `app/ponto/services/fechamento.py:1793`, `app/ponto/services/fechamento.py:1588`, `app/ponto/services/fechamento.py:1579`).

No **PDF**, as extrações executáveis encontradas são de **relógio ou limites**, não de timestamp de batida:

| Local | Forma | Origem |
|---|---|---|
| `app/relatorios/pdf_espelho.py:287` | `timezone.localtime(agora).date()` | Hoje local para o teto. |
| `app/relatorios/pdf_espelho.py:298` | `timezone.localtime(inicio).date()` e `timezone.localtime(fim).date()` | Limites locais da busca, para feriados. |
| `app/relatorios/pdf_espelho.py:597` | `timezone.localtime(agora).date()` | Hoje local. |

As referências a `timestamp.date()` e `p.entrada.date()` no PDF são comentários históricos (`app/relatorios/pdf_espelho.py:483`, `app/relatorios/pdf_espelho.py:488`). No espelho, as referências históricas também estão em comentários/docstring (`app/ponto/services/espelho.py:57`, `app/ponto/services/espelho.py:444-468`).

Outros `.date()` encontrados recortam **limites**, não batidas: `inicio/fim` no fechamento (`app/ponto/services/fechamento.py:646`, `:648`, `:654`, `:674`, `:745`, `:842`) e `_lo/_hi` no espelho (`app/ponto/services/espelho.py:1029`). `datetime.date(_a, _mm, _dd)` constrói uma data de calendário (`app/ponto/services/espelho.py:258`).

Não foram encontradas chamadas executáveis de `localdate`, `data_local` ou lookups `timestamp__date` nesses três arquivos; cobertura de formas indiretas/dinâmicas **[nao verificado]**.

**4. O que `motor.calcular_mes` faz fora de `[ini, fim]`**

Ele **não recorta primeiro cada batida pelo seu dia civil**. Copia os objetos recebidos, entrega as batidas ao juiz e depois remove duplicatas/monta períodos (`app/ponto/motor_calculo_v2.py:2263-2268`, `app/ponto/motor_calculo_v2.py:2346-2354`). O juiz recebe a lista sem corte por dia nesse adaptador (`app/ponto/motor_calculo_v2.py:882`).

O recorte inclusivo acontece **por período**, usando `data_jornada`:

1. Obtém o `data_turno` do juiz; sem correspondência, usa `localizar(entrada).date()` (`app/ponto/motor_calculo_v2.py:889-894`, `app/ponto/motor_calculo_v2.py:2403-2404`).
2. Pode herdar o dia da jornada anterior quando a entrada ocorre entre 0 e 6 horas depois da saída anterior (`app/ponto/motor_calculo_v2.py:2405-2409`, `app/ponto/motor_calculo_v2.py:2423`).
3. Descarta o período se `data_jornada < data_ini` ou `data_jornada > data_fim` (`app/ponto/motor_calculo_v2.py:2429-2432`).

Portanto, uma saída em dia civil posterior a `fim` pode contribuir para uma jornada dentro da janela; uma jornada fora dela é descartada após o pareamento (`app/ponto/motor_calculo_v2.py:2354`, `app/ponto/motor_calculo_v2.py:2429-2432`). O fallback é **dia civil local**, porque `localizar` converte para `settings.TIME_ZONE` e interpreta naive nesse fuso (`app/ponto/motor_calculo_v2.py:28`, `app/ponto/motor_calculo_v2.py:73-79`).

As especializações `MotorTurnoPartido` e `MotorComercial` passam pelo cálculo base e posteriormente agrupam também pelo dia do turno, com fallback local (`app/ponto/motor_calculo_v2.py:2908`, `app/ponto/motor_calculo_v2.py:2941-2942`, `app/ponto/motor_calculo_v2.py:3301`, `app/ponto/motor_calculo_v2.py:3312-3314`).

**5. Funções públicas candidatas e chamadores**

| Função | Contrato | Chamadores de produção |
|---|---|---|
| `ponto.turnos.data_local(ts)` | Retorna `timezone.localtime(ts).date()` ou `None`; exige instante aware, sem defesa para naive (`app/ponto/turnos.py:43-46`). | Signal de recomposição (`app/ponto/signals.py:21-23`); comando `detectar_par_relampago` (`app/ponto/management/commands/detectar_par_relampago.py:302-303`); chamadas internas abaixo. |
| `core.filtro.dia_de(valor)` | Datetime aware → dia local; se a conversão falha, usa `.date()` cru. Aceita também `date`/`None` (`app/core/filtro.py:118-138`). | `core.filtro.aplicar` (`app/core/filtro.py:194`); caminho de holerite fornece `avisado_em`/`publicado_em` (`app/holerite/views.py:407-411`). |

Chamadores internos de `data_local`:

| Chamador | Local |
|---|---|
| `_data_do_turno` | `app/ponto/turnos.py:284` |
| `_mk` | `app/ponto/turnos.py:320` |
| `_saida_pela_posicao_entre_turnos` | `app/ponto/turnos.py:1013` |
| `_fechar_aberto_com_saida_seguinte` | `app/ponto/turnos.py:1098` |
| `_fechar_aberto_na_pausa_sem_volta` | `app/ponto/turnos.py:1186` |
| `_vespera`, dentro de `parear_turnos` | `app/ponto/turnos.py:1294` |
| `par_completo` | `app/ponto/turnos.py:2198` |
| `_dia_das_batidas_puro` | `app/ponto/turnos.py:2918` |

`dia_das_batidas` responde outra pergunta: **dia atribuído à batida**, usando `data_turno` quando reclamada por turno e data civil local para as restantes (`app/ponto/turnos.py:2874-2881`, `app/ponto/turnos.py:2912-2918`).

**6. Contraexemplo naive**

**Não localizei contraexemplo comprovado chegando a `apurar_por_fatia`; impossibilidade geral [nao verificado].** Os três caminhos identificados recarregam as listas do ORM antes da chamada (`app/ponto/services/fechamento.py:624`, `app/ponto/services/espelho.py:1022`, `app/relatorios/pdf_espelho.py:272`).

A função aceita uma coleção sem validar `tzinfo`; um objeto artificial naive passaria pelos `.date()` crus. Isso é possibilidade da interface, sem origem de produção demonstrada (`app/ponto/services/fechamento.py:132`, `app/ponto/services/fechamento.py:244-253`).

As cinco chamadas diretas de teste passam `batidas=[]` (`app/ponto/tests/test_composicao_de_fatias_e_total.py:86-88`, `:104-106`, `:127-129`, `:165`, `:197`). Existem stubs naive, mas entregues diretamente a `_periodos_pelo_marco`, não a essa porta (`app/ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:94-99`).

O motor e o pareador possuem normalização defensiva de naive, **posterior aos filtros da fatia**; isso comprova suporte defensivo, não uma origem naive alcançando a apuração (`app/ponto/motor_calculo_v2.py:73-79`, `app/ponto/turnos.py:1524-1529`, `app/ponto/services/fechamento.py:277-282`).

## 06/11 19:42 · sessao 34c0fb2b6f22 · LEI-FIO-MUDO-B · censo · origem=Code

**O reaponte muda a composição operacional das disputas, mas também atinge uma associação histórica real:** a absorção de perguntas por ausência usa `disputa_id` como parte da identidade do ato. Portanto, “sem `resposta_colab` e em disputa fechada” não basta para demonstrar que a transferência preserva todos os leitores. (`app/chamados/services/materializacao.py:585-618`)

O efeito em produção, as quantidades atingidas e a execução dos testes permanecem **[nao verificado]**. As consequências abaixo são inferências do código para transferência entre disputas do mesmo colaborador, preservando o PK da pergunta; o gate identifica o fato por colaborador, motivo e data. (`app/chamados/models.py:2392-2395`)

**Respostas diretas:**

- **Escritor existente da FK:** encontrei a criação em `PerguntaDisputa.abrir`. O reapontamento existente no gate escreve `celula` e `chamado_destino`; `fio.reapontar_resolvedora` escreve destino e, quando ausente, data. Nenhum desses reaponta `disputa`. Outro escritor da FK fora da criação não foi localizado no código censado; escrita dinâmica ou externa permanece **[nao verificado]**. (`app/chamados/models.py:2431-2477`; `app/chamados/services/fio.py:831-848`)
- **Integridade declarada:** a FK é obrigatória, com `CASCADE` e `related_name='perguntas'`; o `Meta` da pergunta não declara `unique`, `UniqueConstraint` ou `CheckConstraint` por disputa. O `save` envolve a persistência em transação, sem impedir mudança da FK. O esquema efetivamente instalado é **[nao verificado]**. (`app/chamados/models.py:2211-2215`; `app/chamados/models.py:2847-2860`; `app/chamados/migrations/0014_perguntadisputa.py:27`)
- **A fechada que perde a única pergunta continua fechada nesses caminhos automáticos.** O signal da pergunta captura somente o ID **novo**; o detector de becos ignora fechadas; o fechamento não reclassifica uma disputa já fechada como `vazia`. (`app/chamados/signals.py:191-202`; `app/chamados/services/becos.py:25-33`; `app/chamados/models.py:1820-1824`)
- **Ela pode aparecer vazia quando consultada diretamente**, mas fica fora do relatório de pendências, que só coleta abertas. `disputa_vazia()` não detecta esse esvaziamento: conta o motivo de fechamento gravado, sem recontar perguntas. I6 também parte das perguntas, deixando a fechada sem perguntas fora desse universo. (`app/chamados/relatorios_disputa.py:12-14`; `app/chamados/relatorios_disputa.py:40-43`; `app/chamados/services/vigia_fio.py:727-734`; `app/chamados/management/commands/auditar_invariantes_chamados.py:715-740`)
- **Reabertura humana continua possível**, inclusive de uma fechada sem perguntas; não é consequência automática da perda da pergunta. (`app/chamados/models.py:2040-2075`; `app/chamados/tests/test_reabrir_disputa_admin_sem_aberta.py:22-44`)

Há cinco cuidados materiais para o desenho:

1. **Absorção por ausência:** uma irmã muda pode estar materializada sem ter resposta. Transferi-la separa seu `(disputa_id, materializada_em)` daquele do escape respondido; a rejeição da ausência pode deixar de devolvê-la. O teste atual devolve ambas, mantendo a associação original. (`app/chamados/services/materializacao.py:531-549`; `app/chamados/services/materializacao.py:605-618`; `app/chamados/services/materializacao.py:647-665`; `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:88-103`)
2. **Contexto e caches:** materialização lê `disputa.contexto[motivo]`, inclusive `hora_tentativa` no ajuste overnight. A chave do memo de `pergunta_viva` não contém disputa nem contexto; `esta_stale` tampouco detecta a troca da FK por si. Relavra e invalidação dos memos precisam ser verificadas no pós-estado da transferência. (`app/chamados/services/materializacao.py:343-347`; `app/chamados/services/materializacao.py:420`; `app/chamados/juizes.py:811-847`; `app/chamados/veredito_lavrado.py:179-190`)
3. **Transferir não ressuscita:** validação, tranca e vias de identidade continuam podendo matar a pergunta. Uma muda encerrada por admin não passa a ser viva apenas porque recebeu disputa aberta. (`app/chamados/juizes.py:1135-1172`)
4. **Trilha:** o gate retorna a existente antes de registrar `pergunta_emitida`; esse log só nasce na criação. O histórico reconstrói carimbos pelo destino atual, sem evento específico de transferência da FK. Continuidade histórica exige teste próprio de antes/depois e idempotência. (`app/chamados/models.py:2475-2485`; `app/chamados/services/historico.py:86-114`)
5. **Guarda da costura:** `resolvedora_valida` confere dia/turno; `_costura_nao_mata_o_fio` impede ancorar pergunta viva em destino terminal. Nenhuma substitui a elegibilidade específica da transferência de disputa. (`app/chamados/juizes.py:1963-1999`; `app/chamados/services/fio.py:758-780`)

Na tabela, “atual” significa que o leitor usa a associação presente, sem guardar a disputa natal. Os testes citados fixam comportamentos próximos; cobertura específica da **troca da FK** permanece **[nao verificado]**. [nao verificado]

| Sítio | Escritor/leitor | Supõe pertença fixa? | Efeito do reaponte | Teste que fixa | [nao verificado]
|---|---|---|---|---|
| `app/chamados/models.py:2392`, `app/chamados/models.py:2398` | L/E: identidade e nascimento | Não: identidade é pessoa/motivo/data | Conserva dedup global e PK; acrescentaria uma escrita de FK ao ramo existente | `app/chamados/tests/test_porta_pergunta_identidade.py:164`; `app/chamados/tests/test_contract_pergunta_fato.py:10` |
| `app/chamados/models.py:327`, `app/chamados/models.py:345` | L: perguntas ancoradas no chamado | Pai atual é fallback | Sem destino, pergunta deixa de reter o pai antigo e passa ao novo | `app/chamados/tests/test_guarda_disputa_status.py:42`; FK móvel **[nao verificado]** |
| `app/chamados/models.py:1434`, `app/chamados/models.py:1500`, `app/chamados/models.py:1531` | L/E: ressuscitar perguntas e reabrir disputas | Usa FK atual | Ressurreição posterior reabre a disputa atual; não reencontra a origem esvaziada | FK móvel **[nao verificado]** |
| `app/chamados/models.py:1736` | L: badge de pendências | Atual; exige aberta e pergunta viva | Pergunta passa ao conjunto da disputa nova se continuar viva; conta disputas distintas | `app/chamados/tests/test_badge_disputa_pendente.py:29`; `app/chamados/tests/test_fio_le_o_juiz.py:262` |
| `app/chamados/models.py:1783`, `app/chamados/models.py:1794`, `app/chamados/models.py:1836` | L/E: respostas sem veredito, C7 e fechamento | Composição atual | Origem perde a pergunta; destino passa a considerá-la. Fechar depende de ausência de vivas **e** de respostas sem veredito | `app/chamados/tests/test_fechar_disputa_materializada.py:51` |
| `app/chamados/models.py:1976` | L/E: remover motivo e apagar suas perguntas | Composição atual | Remoção no contêiner antigo deixa de alcançar a pergunta movida; na nova, pode alcançá-la | `app/chamados/tests/test_invariantes_disputa.py:139`; FK móvel **[nao verificado]** |
| `app/chamados/models.py:2040`, `app/chamados/models.py:2077`, `app/chamados/models.py:2142` | L/E: reaberturas e contador do colab | Estado/quota pertencem à disputa | Quotas não acompanham a pergunta; correção passa a operar sobre os membros da nova disputa | `app/chamados/tests/test_porta_disputa_ciclo.py:283`; `app/chamados/tests/test_disputa_transicoes.py:131` |
| `app/chamados/models.py:2660`, `app/chamados/models.py:2668` | L: campos que acordam a célula e identidade do evento | Pessoa atual | `disputa`/`disputa_id` já acordam outbox quando salvos por `save`; evento usa pessoa/célula atuais | FK móvel **[nao verificado]** |
| `app/chamados/signals.py:191`; `app/chamados/services/becos.py:29`, `app/chamados/services/becos.py:67` | L/E: fechamento de beco | Só conhece FK atual | Agenda análise da nova disputa; não captura origem. Fechada anterior é ignorada | `app/chamados/tests/test_varredura_becos_evento_pergunta.py:47`, `app/chamados/tests/test_varredura_becos_evento_pergunta.py:94` |
| `app/chamados/signals.py:214` | L/E: outbox da pergunta | Pessoa/célula atuais | Save contendo FK emite evento; não há evento separado para “disputa anterior perdeu membro” | FK móvel **[nao verificado]** |
| `app/chamados/signals.py:18`, `app/chamados/signals.py:124` | Gatilhos de fechamento do pai/materialização | Composição no fechamento | Transferência da pergunta não salva a disputa antiga; fechamento posterior da nova usa seus membros atuais | `app/chamados/tests/test_disputa_transicoes.py:71` |
| `app/chamados/services/fio.py:74`, `app/chamados/services/fio.py:116` | L: data memoizada e âncora | Pai atual como fallback | Sem destino, âncora muda; `_dfd_memo` não compara campos antes de retornar | `app/chamados/tests/test_fio_ancora_destino.py:72`; invalidação por FK **[nao verificado]** |
| `app/chamados/services/fio.py:135` | L: disputa do chamado pelo destino | Exige aberta atual | Passa a localizar a nova disputa pela pergunta transferida | `app/chamados/tests/test_fio_disputa_por_destino.py:21` |
| `app/chamados/services/fio.py:218`, `app/chamados/services/fio.py:229`, `app/chamados/services/fio.py:252` | L: montagem do fio/cards | Universo por contêiner atual | Sai do conjunto de fechadas, perde flag `em_disputa_fechada`, entra no universo de abertas do colab, sujeito aos demais filtros | `app/chamados/tests/test_fio_le_o_juiz.py:105`, `app/chamados/tests/test_fio_le_o_juiz.py:121` |
| `app/chamados/services/fio.py:392` | L: mapa pai → destinos | Relação atual representa filiação | Pai antigo perde filho no mapa; novo ganha | FK móvel **[nao verificado]** |
| `app/chamados/services/fio.py:591`, `app/chamados/services/fio.py:629` | L/E: validar, recusar, corrigir | Estado atual autoriza ação | Deixa a via especial de validação em fechada; pode entrar em recusa/correção de aberta | `app/chamados/tests/test_fio_le_o_juiz.py:192`, `app/chamados/tests/test_fio_le_o_juiz.py:228` |
| `app/chamados/services/fio.py:769`, `app/chamados/services/fio.py:831`, `app/chamados/services/fio.py:931` | L/E: guarda, reaponte e migração de resolvedoras | Conserva FK no código atual | **Escreve destino, não disputa**; guarda terminal e relavra continuam necessárias | `app/chamados/tests/test_o261e_f1_porta_encerramento.py:252`; `app/chamados/tests/test_porta_reapontamento_pergunta.py:245` |
| `app/chamados/juizes.py:72`, `app/chamados/juizes.py:205`, `app/chamados/juizes.py:2029` | L: dias da disputa/chamado e competência | Perguntas atuais são evidência do contêiner | Origem perde datas; nova ganha nos degraus que dependem dessas perguntas | FK móvel **[nao verificado]** |
| `app/chamados/juizes.py:346`, `app/chamados/juizes.py:361` | L: slot/completude | Contexto da disputa é insumo | Contexto novo pode mudar derivação/julgamento; data da pergunta prevalece em alguns fallbacks | FK móvel entre contextos **[nao verificado]** |
| `app/chamados/juizes.py:664`, `app/chamados/juizes.py:752`, `app/chamados/juizes.py:774`, `app/chamados/juizes.py:932`, `app/chamados/juizes.py:1200`, `app/chamados/juizes.py:1528` | L: pessoa, empresa, grade e provas | Identidade consistente da pessoa | Mesma pessoa preserva esses insumos; não exige disputa natal | FK móvel **[nao verificado]** |
| `app/chamados/juizes.py:811`, `app/chamados/juizes.py:835` | L: memo de `pergunta_viva` | FK/contexto não integram chave | Troca isolada da FK na mesma instância pode reutilizar julgamento anterior | `app/chamados/tests/test_selo_memo_pergunta_viva.py:117`; troca da FK **[nao verificado]** |
| `app/chamados/juizes.py:1021`, `app/chamados/juizes.py:1300` | L: mapa de identidade/alimentação por célula | Pessoa/célula | Mantém universo se pessoa/célula forem preservadas; atribuição de `d.colaborador` não escreve FK da pergunta | FK móvel **[nao verificado]** |
| `app/chamados/juizes.py:1230`, `app/chamados/juizes.py:1392`, `app/chamados/juizes.py:1399` | L: vivas, corrigíveis e fila global | Composição atual; fila global por pergunta | Conjuntos locais migram; seleção global por pergunta pode permanecer | FK móvel **[nao verificado]** |
| `app/chamados/juizes.py:1751` | L: irmãs respondidas, fallback ao pai | Pai atual concede crédito | Resposta futura passa a poder ser creditada ao pai novo | FK móvel **[nao verificado]** |
| `app/chamados/juizes.py:1963` | L: `resolvedora_valida` | Não exige disputa natal | Confere dia/turno; não garante identidade da disputa/pessoa para uma transferência arbitrária | `app/chamados/tests/test_selo_resolvedora_do_dia.py:67`, `app/chamados/tests/test_selo_resolvedora_do_dia.py:94` |
| `app/chamados/juizes.py:2080` | L: disputa viva bloqueadora do chamado | Aberta atual; vazia também bloqueia | Origem fechada vazia fica fora; destino passa a considerar pergunta movida | `app/chamados/tests/test_guarda_disputa_status.py:58`, `app/chamados/tests/test_guarda_disputa_status.py:64` |
| `app/chamados/veredito_lavrado.py:162`, `app/chamados/veredito_lavrado.py:187` | L/E: marcos, stale e lavra | Marcos por pessoa; FK não é marco | Mesma pessoa mantém marcos; mudança de contexto exige relavra explícita | `app/chamados/tests/test_porta_veredito_pergunta.py:259`; FK móvel **[nao verificado]** |
| `app/chamados/services/materializacao.py:343`, `app/chamados/services/materializacao.py:420`, `app/chamados/services/materializacao.py:471` | L: data/contexto/overnight/ausência | Contexto atual da disputa | Pode mudar fallback de data e `hora_tentativa`, mesmo preservando pessoa | FK móvel **[nao verificado]** |
| `app/chamados/services/materializacao.py:531`, `app/chamados/services/materializacao.py:1289`, `app/chamados/services/materializacao.py:1344`, `app/chamados/services/materializacao.py:1393` | L/E: irmãs, materialização, contagem e fechamento | Composição atual | Unidade de materialização e quitação migra à nova; fechada antiga não é refechada | `app/chamados/tests/test_fechar_disputa_materializada.py:51`; `app/chamados/tests/test_validar_materializa_wiring.py:51` |
| `app/chamados/services/materializacao.py:609`, `app/chamados/services/materializacao.py:614` | L: grupo histórico de absorção | **Sim: FK integra identidade do ato** | Mover irmã muda pode separá-la do escape e impedir devolução após rejeição | `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:88`; com reaponte **[nao verificado]** |
| `app/chamados/services/materializacao.py:660`, `app/chamados/services/materializacao.py:686` | L/E: devolver absorvidas/resgatar pai | FK/pai atuais | Reabre disputa atual; não recupera automaticamente origem vazia | `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:99` |
| `app/chamados/services/materializacao.py:931`, `app/chamados/services/materializacao.py:942` | E: trilha de materialização | Relata pai atual | Materialização futura registra pai novo; não reescreve trilha anterior | FK móvel **[nao verificado]** |
| `app/chamados/services/validacao.py:191`, `app/chamados/services/validacao.py:355`, `app/chamados/services/validacao.py:424`, `app/chamados/services/validacao.py:526`, `app/chamados/services/validacao.py:795` | L/E: validação, recusa, materialização e reabertura | Autoriza por FK atual | Pedido amarrado à disputa antiga retorna `nao_encontrada`; próximos atos usam disputa/pai novos | `app/chamados/tests/test_validar_materializa_wiring.py:51`; FK móvel **[nao verificado]** |
| `app/chamados/services/acoes_disputa.py:95`, `app/chamados/services/acoes_disputa.py:129`, `app/chamados/services/acoes_disputa.py:260`, `app/chamados/services/acoes_disputa.py:338`, `app/chamados/services/acoes_disputa.py:375` | L/E: ações, contadores, reabertura admin | Membros atuais | Contagem migra; origem vazia pode informar “Nenhuma pergunta cadastrada”. Reabertura admin chama gate | `app/chamados/tests/test_recobrar_questionario.py:80`; `app/chamados/tests/test_reabrir_disputa_admin_sem_aberta.py:22` |
| `app/chamados/services/acoes_chamado.py:342`, `app/chamados/services/acoes_chamado.py:388` | L: detalhe e “todas validadas” | Membros atuais | Origem perde lista; guarda `bool(_todas_perg)` evita tratar vazio como todas validadas | FK móvel **[nao verificado]** |
| `app/chamados/services/disputa_emissao.py:300`, `app/chamados/services/disputa_emissao.py:371`, `app/chamados/services/disputa_emissao.py:387`, `app/chamados/services/disputa_emissao.py:909`, `app/chamados/services/disputa_emissao.py:1237` | L: dedup local/complementação | Associação atual | Origem perde satisfação do dedup local; gate global continua segurando fato único | `app/chamados/tests/test_selo_repergunta_premissa_viva.py:64`; FK móvel **[nao verificado]** |
| `app/chamados/services/disputa_emissao.py:1088`, `app/chamados/services/disputa_emissao.py:1112` | E: motivos e autofecho | Metadados separados da FK | Emissão pode atualizar motivos da nova; simples troca da FK não recompõe motivos/contexto da origem | `app/chamados/tests/test_hx_disputa_muda.py:24`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51` |
| `app/chamados/services/disputa_emissao.py:1344`, `app/chamados/services/disputa_emissao.py:1485`, `app/chamados/services/disputa_emissao.py:1580`, `app/chamados/services/disputa_emissao.py:1621`, `app/chamados/services/disputa_emissao.py:1657`, `app/chamados/services/disputa_emissao.py:1715` | L/E: resposta, ocorrência, recobrança e resolução | Pai/composição atuais | Resposta futura escreve no pai novo; contadores e fechamento usam nova disputa; vários ramos pulam fechadas | `app/chamados/tests/test_b2d_resposta_visivel.py:66`; FK móvel **[nao verificado]** |
| `app/chamados/services/declaracao_texto.py:227`, `app/chamados/services/declaracao_texto.py:325`, `app/chamados/services/declaracao_texto.py:536`, `app/chamados/services/declaracao_texto.py:614`, `app/chamados/services/declaracao_texto.py:644` | L/E: reutilização, gate e validação | Busca local por disputa; global por fato | Reutilização local migra; pessoa/motivo/data preservam identidade global | FK móvel **[nao verificado]** |
| `app/chamados/services/relavra_via.py:152`, `app/chamados/services/relavra_via.py:181`, `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:243` | L/E: relavra/descarimbo/reabertura | Estado atual da FK | Entra em recortes de aberta; eventual reabertura visita somente a disputa atual | `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:135`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:161` |
| `app/chamados/services/passivo_validado.py:333`, `app/chamados/services/passivo_validado.py:354`, `app/chamados/services/passivo_validado.py:516`, `app/chamados/services/passivo_validado.py:521` | L/E: materialização e quitação em lote | FK atual agrupa fechamento | Crédito de materialização/contagem passa à nova disputa | FK móvel **[nao verificado]** |
| `app/chamados/services/cobranca.py:61`, `app/chamados/services/cobranca.py:91`, `app/chamados/services/cobranca.py:230` | L/E: pais, prazo e agrupamento | Pai atual é dono do prazo | Cobrança passa a consultar prazo e pai novos; quota da origem não migra | `app/chamados/tests/test_recobrar_questionario.py:88`; FK móvel **[nao verificado]** |
| `app/chamados/services/explicador.py:122`, `app/chamados/services/explicador.py:124`, `app/chamados/services/explicador.py:137` | L: explicação por disputa | Membros atuais | Origem vazia produz “Tudo resolvido — a disputa pode fechar”; não escreve estado | FK móvel **[nao verificado]** |
| `app/chamados/services/raiox.py:31`, `app/chamados/services/raiox.py:55`, `app/chamados/services/raiox.py:60`, `app/chamados/services/raiox.py:135` | L: stale/crédito/placar | Pai atual como fallback | Marcos da pessoa permanecem; crédito pelo pai pode migrar | `app/chamados/tests/test_selo_defer_json_creditos.py:12`; FK móvel **[nao verificado]** |
| `app/chamados/services/painel.py:262`, `app/chamados/services/painel.py:264`; `app/chamados/services/verbos.py:310` | L: empresa/praça/atividade | Pessoa, não origem | Mesma pessoa preserva recortes desses leitores | FK móvel **[nao verificado]** |
| `app/chamados/services/pergunta_no_app.py:46`, `app/chamados/services/pergu

[truncado em 20000 de 35196 caracteres]

## 06/11 19:41 · sessao 896c447650ad · - · - · origem=Ronald (tmux)

1. **Escritores dos limites e de `_meus`:**

- `fatia_ini = max(ini, esc.data_inicio, DATA_INICIO_OPERACAO)`; `fatia_fim = min(fim, esc.data_fim ou fim)`: `app/ponto/services/fechamento.py:204-205`. Os campos da vigência são `DateField`: `app/escala/models.py:875-876`. O piso operacional é `date(2026,5,21)`: `app/core/constants.py:5`.
- As chaves `(colaborador_id, data)` das células geram `_dono_do_dia`, após consulta a `vinculo_do_dia`: `app/ponto/services/fechamento.py:189-193`. A origem dessas chaves é `CelulaDia.data`: `app/escala/alimentacao.py:73-78`, campo `DateField` em `app/escala/models.py:1685`.
- `_meus` inicial contém as datas atribuídas ao vínculo dentro dos limites: `app/ponto/services/fechamento.py:207-208`. Recebe também dias sem dono cobertos por exatamente um vínculo: `app/ponto/services/fechamento.py:221-231`. Havendo `_julgados`, substitui os limites por seus extremos: `app/ponto/services/fechamento.py:232-236`. O pad deriva desses limites por ±1 dia: `app/ponto/services/fechamento.py:242-243`.
- A célula usa **dia nominal do turno previsto, ancorado no calendário local**: o gerador declara instantes “ISO local naive” e atribui marcos pós meia-noite a D+1: `app/ponto/management/commands/gerar_celulas.py:36-40`, `app/ponto/management/commands/gerar_celulas.py:67-69`. Exemplo codificado: célula 21/08 pode conter saída 22/08 07:02: `app/ponto/juiz_batida.py:206-222`.
- Vigências são datas cadastrais comparadas diretamente nesse domínio de dias; a resolução da célula recusa geradora fora da vigência: `app/escala/alimentacao.py:269-286`. Não há conversão de timestamp UTC para produzir essas datas nesses pontos: `app/ponto/services/fechamento.py:189-236`.

**Origem dos `ini/fim` dos chamadores:**

| Chamador | Fonte |
|---|---|
| Fechamento | `janela_fechamento(mes,ano,empresa)` gera datas; transforma-as em meia-noite/final de dia aware e passa `.date()` a `apurar_por_fatia`: `app/ponto/services/fechamento.py:612-614`, `app/ponto/services/fechamento.py:745-746`. A janela constrói `date` por corte da empresa, sem timestamp: `app/ponto/janelas.py:15-21`, `app/ponto/janelas.py:54-57`. |
| Espelho | `autoridade_do_periodo` recebe `ini/fim` e os repassa: `app/ponto/services/espelho.py:971`, `app/ponto/services/espelho.py:1164`. No caminho da tela, `ini=vis_ini=min(piso_visual,apur_ini)` e `fim=hoje`: `app/ponto/services/espelho.py:1225-1227`. `hoje` deriva de `localtime(now).date()` e `apur_ini/fim` da janela da empresa: `app/ponto/views.py:97-98`, `app/ponto/views.py:153-158`. O piso deriva de DIO/início do vínculo, limitado a hoje: `app/ponto/services/espelho.py:39-48`. |
| PDF | Recebe datas livres: `app/relatorios/pdf_espelho.py:187`, `app/relatorios/pdf_espelho.py:1038-1046`. No mensal, recebe janela da competência: `app/relatorios/pdf_espelho.py:654-663` → `app/relatorios/cartao_pela_celula.py:21-24` → `app/folha/export.py:47-51` → `app/ponto/janelas.py:54-57`. Antes de apurar, limita `fim` a ontem local, com piso em `data_ini`: `app/relatorios/pdf_espelho.py:285-288`, `app/relatorios/cartao_pela_celula.py:37-41`. |

2. **Escritores das listas `batidas`:**

| Chamador | Construção e janela |
|---|---|
| Fechamento | `batidas = list(_bap(colab,inicio_busca,fim_busca))`, desde início local 00:00−1 dia até fim local 23:59:59+1 dia: `app/ponto/services/fechamento.py:612-624`; lista passada em `app/ponto/services/fechamento.py:745`. |
| Espelho | `batidas = list(batidas_apuraveis(...))`, mesmo envelope local de ±1 dia: `app/ponto/services/espelho.py:1019-1022`; lista passada em `app/ponto/services/espelho.py:1164`. |
| PDF | `batidas_periodo = list(_bap(...))`, início local 00:00−12h até fim pedido local 00:00+1d12h: `app/relatorios/pdf_espelho.py:264-272`; lista passada em `app/relatorios/pdf_espelho.py:419-420`. Atenção: essa carga antecede o teto em ontem de `app/relatorios/pdf_espelho.py:285-288`. |

Nos três, `_bap` consulta `Batida.objects`, filtra `retratada_em IS NULL`, limites por `timestamp` e colaborador, e ordena pelo timestamp, sem reescrever seu fuso: `app/ponto/turnos.py:2863-2871`. O campo é `DateTimeField`: `app/ponto/models.py:23`. `USE_TZ=True` e `TIME_ZONE='America/Sao_Paulo'`: `app/config/settings/base.py:93-95`; produção herda essas configurações e usa PostgreSQL: `app/config/settings/prod.py:1`, `app/config/settings/prod.py:9-17`.

**UTC aware é o contrato esperado do ORM nesse arranjo; a classe concreta do `tzinfo` dos objetos atuais não foi observada [nao verificado].** O próprio repositório declara timestamps UTC no banco: `app/docs/LEIS.md:107`; há registro histórico de observação do banco em UTC em 29/09: `app/docs/RELATO-ARQUIVO.md:21309-21310`. Dependências declaradas: Django 5.2.14 e psycopg 3.2.3: `app/requirements.txt:13`, `app/requirements.txt:21`.

**Nuance para consolidação:** o comentário que diz que o motor muta as batidas do chamador está em `app/ponto/services/fechamento.py:790-799`, mas `calcular_mes` agora faz cópia rasa de cada objeto antes de trabalhar: `app/ponto/motor_calculo_v2.py:2262-2268`. A conversão naive→aware local existe em `app/ponto/motor_calculo_v2.py:458-461`, depois da entrada no motor; portanto ela não normaliza os objetos antes dos filtros de `apurar_por_fatia`: `app/ponto/services/fechamento.py:244-253`, `app/ponto/services/fechamento.py:277-282`.

## 06/11 19:41 · sessao 540b03e22600 · SILENCIADO-ATE-E-PRAZO · censo · origem=Code

A cópia contém **duas regras incompatíveis para o vencido**: `qs_na_fila` devolve chamados de cobrança à fila quando o prazo passa; `particionar_verbos` os transforma em `HISTORICO`, excluído da lista padrão. Há testes exigindo cada comportamento. (`app/chamados/services/painel.py:186-188`; `app/chamados/services/verbos.py:14-18,45-51`; `app/chamados/tests/test_f9_contador_bate_com_a_lista.py:90-96`; `app/chamados/tests/test_f7_janela_arquivo.py:41-46`.)

O anexo fornecido não traz linhas de chamados: quantidade afetada, situação atual de produção e resultado de execução dos testes são **[nao verificado]**. Abaixo, todos os caminhos são relativos a `app/`; “muda” descreve o efeito inferido do código. [nao verificado]

Convenções: **(1)** prazo temporário; **(2)** presença persistente que retira da lista/cobrança; **outro** = auditoria, exibição, consulta histórica ou guarda de escrita, sem decidir por si a fila.

**Testes que efetivamente fixam o vencido como arquivo**, usados como referências na tabela final: [nao verificado]

| Referência | arquivo::teste | O que fixa | [nao verificado]
|---|---|---|
| T1 | `chamados/tests/test_f1_arquivar.py::test_prazo_vencido_vira_historico_e_nao_volta_pra_fila`, linha 48 | Vencido vira `historico`; `decidir=0` nas linhas 52-56. |
| T2 | `chamados/tests/test_f7_janela_arquivo.py::test_vencida_a_janela_vira_historico_e_NAO_volta_pra_fila`, linha 41 | Vencido não conta em `decidir` nem em `arquivado`, linhas 42-46. |
| T3 | `chamados/tests/test_f8_lista_esconde.py::test_lista_padrao_esconde_arquivado_e_historico`, linha 36 | Vencido fica fora da lista padrão, linhas 39-43. |
| T4 | `colaboradores/tests/test_ficha_do_colab.py::test_MORDE_arquivado_e_historico_dao_o_dono_arquivado`, linha 98 | Tanto data passada como futura produzem dono `arquivado`, linhas 109-114. |
| T5 | `chamados/tests/test_o261e_f5b_juiz_porta.py::test_MORDE_silenciado_na_celula_nao_e_numero_da_fila`, linha 144 | Vencido aparece no mapa como `historico`, mas `porta['fila']` fica vazia, linhas 152-165. |
| T6 | `chamados/tests/test_f9_busca_historico.py::test_so_arquivados_nunca_o_vivo`, linha 51; `::test_busca_por_colab`, linha 58 | Vencido continua pesquisável no arquivo. Isso fixa **consulta histórica**, não exclusão da cobrança; fixture passada na linha 32. |

A cura da cobrança contraria **T1–T5**. **T6 pode permanecer**: consultar um silêncio antigo e cobrar novamente são compatíveis. O teste `test_f8_lista_esconde.py::test_pilula_arquivados_mostra_so_os_da_janela`, linha 45, também pode permanecer: ele exige somente futuros na pílula Arquivados. (`chamados/services/verbos.py:48-51`; `chamados/services/painel.py:318-329`; `chamados/views_arquivar.py:153-180`.)

**A janela do colaborador não tem, nos casos localizados, teste que fixe a exclusão do vencido [nao verificado].** Os testes existentes usam silêncio **futuro**: `test_c1_janela_colab.py::test_arquivado_dentro_da_janela_tambem_sai`, linha 55; `test_c2_endpoints_janela.py::test_fio_com_so_janela_recorta`, linha 41; `test_c3_meus_arquivados.py::test_traz_fora_da_janela_e_arquivado_mas_nao_o_da_lista`, linha 45. (`chamados/tests/test_c1_janela_colab.py:58-60`; `chamados/tests/test_c2_endpoints_janela.py:27,44-48`; `chamados/tests/test_c3_meus_arquivados.py:33,48-52`.)

O censo dos testes fica separado da produção: [nao verificado]

| Arquivo de teste | Escrita/leitura localizada | Semântica coberta | [nao verificado]
|---|---|---|
| `api/tests/test_api_mensageria_fila.py:178` | Escrita de fixture futura | API/fila. |
| `chamados/tests/test_agir_por_dono.py:56` | Leitura por assert | Recusa não silenciou. |
| `chamados/tests/test_c1_janela_colab.py:28-34,55-60` | Escrita de fixture; consulta de IDs | Janela; só futuro. |
| `chamados/tests/test_c2_endpoints_janela.py:21-28,41-65` | Escrita; consulta/inspeção de consumidores | Fio e delegação da API; só futuro. |
| `chamados/tests/test_c3_meus_arquivados.py:28-34,45-52` | Escrita; consulta do drawer | Complemento da lista; só futuro. |
| `chamados/tests/test_c4_resgate.py:33,44-71` | Fixture e asserts do campo | Resposta/resgate limpa silêncio e protege contra prescrição. |
| `chamados/tests/test_f1_arquivar.py:35-56` | Escritas; leitura da partição | Futuro e vencido; T1. |
| `chamados/tests/test_f3_arquivar_porta.py:42-50,65-72` | Fixture e asserts | Porta de arquivar/desarquivar e prazo. |
| `chamados/tests/test_f3_beco1_reata_limbo.py:113,140,157` | Escritas futura/passada | Reatamento distingue silêncio vigente de vencido. |
| `chamados/tests/test_f4_arquivar_lote.py:44-45,57` | Asserts do campo | Lote e corte de idade. |
| `chamados/tests/test_f6_prescricao.py:41-60` | Asserts e leitura do primeiro prazo | Dry/apply/idempotência; não exercita vencido. |
| `chamados/tests/test_f7_janela_arquivo.py:26-51` | Escritas; leitura da partição | T2; futuro Arquivado e passado Histórico. |
| `chamados/tests/test_f8_lista_esconde.py:26-51` | Escritas; leitura da lista | T3 e pílula Arquivados. |
| `chamados/tests/test_f9_busca_historico.py:29-41,51-61` | Escritas; leitura da busca | T6. |
| `chamados/tests/test_f9_contador_bate_com_a_lista.py:50,90-96` | Fixture; leitura de `qs_na_fila` | **(1): vencido volta**. |
| `chamados/tests/test_fio_contador_ativos_pela_fila.py:56,64-65` | Fixtures futura/passada | Contador acompanha fila temporal. |
| `chamados/tests/test_o261e_f5_alcance.py:88` | Fixture futura | Alcance da porta. |
| `chamados/tests/test_o261e_f5b_juiz_porta.py:157,202,233` | Fixtures; retornos do juiz | T5 e exclusão de futuro. |
| `chamados/tests/test_porta_arquivo_silencio.py:136,153,181,202,226,264,314-402` | Escritas pela porta e asserts | Autor, motivo, limpeza, trilha, idempotência e prazo. |
| `chamados/tests/test_porta_arquivo_silencio.py:422-505` | Leitura estrutural; fontes sintéticas nas linhas 444-448 e 499-500 | Exclusividade da porta e da soma do prazo; fontes sintéticas não são escritores executados do modelo. |
| `chamados/tests/test_s4_pendente_suspende_cobranca.py:58-59,79,98-113` | Fixtures e asserts | Ausência suspende; rejeição levanta; SLA respeita silêncio futuro. |
| `chamados/tests/test_selo_i6_ouve_a_lei.py:54-55,143-144` | Escritas futuras | Silêncio nas fixtures do selo. |
| `chamados/tests/test_silenciar_isentos.py:12` | Leitura textual do código | Presença do nome do campo; não prova tratamento de vencido. |
| `chamados/tests/test_sla_pela_fila.py:72-74` | Fixture futura; leitura do SLA | Futuro fora da cobrança SLA. |
| `chamados/tests/test_varredura_isentos_evento.py:43-107` | Leitura por asserts; atos pela porta | Eventos silenciam; resgate de isento recebe novo véu; cron vigia. |
| `colaboradores/tests/test_ficha_do_colab.py:110-114,118-130` | Fixtures passada/futura; retorno e AST | T4 e uso de `FORA_DA_LISTA_PADRAO`. |
| `core/tests/test_contract_escritores_estado.py:35-38` | Leitura estrutural | Escritor permitido dos quatro campos. |
| `core/tests/test_registrado_nao_e_pendencia.py:108` | Fixture futura | Contagem de pendências. |
| `escala/tests/test_ranking_chamados_pela_fila.py:54,63-64` | Fixtures futura/passada | **(1): vencido volta** ao ranking. |
| `ponto/tests/test_contract_juiz_feriado.py:55-72` | Fonte sintética e AST | Definição única de `PRAZO_ARQUIVO_DIAS`. |

Sobre o prazo: **`PRAZO_ARQUIVO_DIAS = 30` está em `chamados/catalogo/motor.py:84`; a leitura numérica em produção encontrada está centralizada em `arquivo.prazo_padrao`, linhas 117-118.** Seus consumidores são as duas views de arquivamento, prescrição, isentos e ausência; os dois lotes especiais passam datas próprias de dez e cinco anos. (`chamados/views_arquivar.py:36,129`; `chamados/management/commands/arquivar_prescritos.py:84`; `chamados/services/silencio_isentos.py:39`; `chamados/services/silencio_ausencia.py:88`; `chamados/management/commands/arquivar_competencia_encerrada.py:274-288`; `chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:30,88-94`.)

O registro da autoridade e a catraca também mencionam o prazo, sem consumir seu valor para negócio: `core/juizes.py:285,1387`. Os testes específicos são `test_porta_arquivo_silencio.py::test_MORDE_sem_ate_a_porta_usa_o_juiz_do_prazo`, linha 386; `::test_MORDE_o_teto_do_prazo_mora_SO_na_porta`, linha 456; `::test_MORDE_o_proprio_varredor_do_teto_ve_CODIGO`, linha 492; e `test_contract_juiz_feriado.py::test_MORDE_prazo_de_arquivo_definido_uma_vez`, linha 66. (`chamados/tests/test_porta_arquivo_silencio.py:386-402,456-505`; `ponto/tests/test_contract_juiz_feriado.py:66-72`.)

**Existe outra forma concreta de arquivo:** encerramento com `via_encerramento='competencia_trancada'`, sem exigir silêncio. Não há status `arquivado` nem campo `arquivado_em` na declaração de `ChamadoColaborador`. `silenciado_por`/`silenciado_motivo` são metadados; `resgatado_em` protege contra novo arquivamento. (`chamados/models.py:22-95`; `chamados/catalogo/motor.py:90-94`; `chamados/models.py:1552-1569`; `chamados/views_arquivar.py:163-166`.)

Quem lê essa via: busca do arquivo e resgate (`chamados/views_arquivar.py:64,165`), reabertura da competência (`chamados/services/competencia_trancada.py:259`) e reconciliação de perguntas, que evita tratar a tranca como resolução comum (`chamados/services/disputa_emissao.py:1572-1573`). Os testes são `chamados/tests/test_competencia_trancada.py::test_o_arquivo_lista_o_encerrado_pela_tranca`, linha 203, e `::test_MORDE_desarquivar_devolve_e_a_segunda_passada_respeita_o_admin`, linha 164. Essa modalidade não muda apenas com a cura do relógio de `silenciado_ate`. (`chamados/tests/test_competencia_trancada.py:164-175,203-209`.)

A serialização localizada é manual: a API do colaborador recebe o queryset da janela e monta dicionários; a mensageria publica a partição. Serializer dedicado ou `annotate` diretamente sobre `silenciado_ate`: **[nao verificado: não localizado]**. Há agregações que recebem a fila já filtrada, discriminadas abaixo. (`api/views.py:1016,1121-1145`; `api/views_mensageria.py:395,411-422`; `core/services/painel_op.py:119`; `escala/services/furos_diarios.py:212`.)

Na tabela final, **“sem caso vencido localizado” significa [nao verificado]**. Guardas por presença recebem **outro**, pois impedir repetir uma escrita não equivale a decidir que o vencido deve ficar fora da cobrança.

| Sítio | Escritor/leitor | Supõe (1)/(2)/outro | Teste que fixa | Muda com a cura? | [nao verificado]
|---|---|---|---|---|
| `chamados/models.py:84-95`; `chamados/migrations/0031_chamadocolaborador_silenciado_ate_and_more.py:18-29` | Declaração do campo/schema | Outro; comentário do modelo declara retorno após vencer, linhas 81-83 | Contrato em `core/tests/test_contract_escritores_estado.py:35` | Sem mudança de schema necessária para ler o relógio. |
| `chamados/services/arquivo.py:166-171` — `silenciar` | **Escritor direto:** atribui e salva data, autor e motivo | Outro: executa decisão do chamador; não decide merecimento, linhas 16-18 | `test_porta_arquivo_silencio.py::test_MORDE_silenciar_2x_com_a_mesma_entrada_nao_escreve_de_novo`, linha 314 | Não exige mudar a escrita. |
| `chamados/services/arquivo.py:201-208` — `levantar` | **Escritor direto:** limpa data/autor/motivo; pode carimbar resgate | Outro: limpeza explícita | `test_porta_arquivo_silencio.py::test_MORDE_levantar_2x_nao_recarimba_o_resgate`, linha 336 | Não exige limpeza para o relógio vencer; resgate continua ato distinto. |
| `chamados/services/arquivo.py:123,130-138` | Leitor para fotografia antes/depois e trilha | Outro: auditoria | `test_porta_arquivo_silencio.py::test_MORDE_levantar_pela_porta_deixa_trilha`, linha 233 | Não. |
| `chamados/services/arquivo.py:161-164,193-196` | Leitor de igualdade/presença para idempotência | Outro | Testes de idempotência nas linhas 314 e 336 | Não substituir por validade temporal: igualdade da entrada e limpeza são perguntas diferentes. |
| `chamados/services/arquivo.py:150-152` | Contrato da renovação, implementada nas linhas 160-171 | Outro: ausência pode renovar a cada passada | `test_s4_pendente_suspende_cobranca.py:52-59`; vencimento sem renovação: sem caso localizado | Uma guarda “silêncio ainda válido → não escrever” mudaria a renovação; não é exigência da cura. |
| `chamados/services/arquivo.py:117-118` | Leitor de `PRAZO_ARQUIVO_DIAS` | Outro: converte dias em data | Teste do prazo, linha 386 | Não. |
| `chamados/models.py:476-480` — `silenciado_em` | Leitor direto | **(1):** somente `> agora` | Caso direto do método: sem caso localizado | Já temporal; vencido retorna `False`. |
| `chamados/services/painel.py:186-188` — `qs_na_fila` | Leitor `.exclude(__gt=now)` | **(1)**, condicionado a `COBRANCA` | `test_f9_contador_bate_com_a_lista.py::test_silencio_VENCIDO_volta_sozinho`, linha 90 | Já é a regra pretendida. |
| `chamados/services/painel.py:213-214`; `chamados/services/fila.py:122-127` | Leitores `.filter/.exclude(__gt)` para contadores | **(1)** | `test_f9_contador_bate_com_a_lista.py:90-96` cobre a fila-base | Vencido já não conta como Arquivado. |
| `chamados/services/verbos.py:45-51` | Leitor `values_list`; troca verbo pela presença/data | **(2)**, quando verbo original não é `None` | **T1, T2** | Vencido conserva o verbo efetivo; sai do bucket `historico` e volta à pílula correspondente. |
| `chamados/services/painel.py:228,318-329` | Leitor indireto da partição e de `FORA_DA_LISTA_PADRAO` | **(2)** | **T3** | Vencido elegível reaparece na lista padrão e no filtro do verbo. `f_status` explícito segue seu ramo próprio. |
| `chamados/services/verbos.py:171-195` — `coletar_lente` | Leitor indireto da partição | **(2)** | T1/T2 fixam a dependência; lente com vencido: sem caso localizado | Vencido passa ao grupo/filtro do verbo efetivo. |
| `chamados/services/fila.py:55-67` — `por_verbo` | Leitor indireto para contagens e rótulos | **(2)** | T1/T2; consumidor com vencido: sem caso localizado | Redistribui `historico` para o verbo efetivo. |
| `chamados/services/janela_colab.py:67-74` | Leitor `.filter(silenciado_ate__isnull=True)` | **(2)** | `test_c1_janela_colab.py::test_arquivado_dentro_da_janela_tambem_sai`, linha 55, **só futuro** | Trocar por exclusão de futuros admite vencidos que satisfaçam janela/pergunta e módulos permitidos; não devolve indiscriminadamente todo chamado antigo. |
| `chamados/services/fio.py:169-176`; `chamados/views.py:569` | Leitor indireto da janela com `so_janela=True` | **(2)** | `test_c2_endpoints_janela.py::test_fio_com_so_janela_recorta`, linha 41, **só futuro** | Fio do colaborador passa a incluir vencidos elegíveis. Fio sem a flag já usa outro universo. |
| `api/views.py:1016,1121-1145` | Leitor indireto; serialização manual | **(2)** pela janela | `test_c2_endpoints_janela.py::test_api_v2_usa_o_juiz_unico`, linha 58, estrutural | JSON recebe os vencidos admitidos pela janela curada. |
| `chamados/views_arquivar.py:195-197` — `meus_arquivados` | Leitor indireto: complemento da janela | **(2)** herdado | `test_c3_meus_arquivados.py::test_traz_fora_da_janela_e_arquivado_mas_nao_o_da_lista`, linha 45, **só futuro** | Vencido que retorna à lista deixa esse complemento; demais excluídos continuam consultáveis. |
| `chamados/views_arquivar.py:165-166` — busca | Leitor `Q(isnull=False)` e ordenação `F(silenciado_ate)` | **Outro: acervo histórico**, inclui vencidos e via de tranca | **T6** | Pode permanecer igual com Histórico só como exibição. Trocar aqui presença por futuro retiraria vencidos da pesquisa e contrariaria T6. |
| `templates/chamados/partials/_busca_arquivados.html:46-49` | Leitor direto de exibição/data | Outro: **já lê relógio** | T6 cobre inclusão; texto exato: sem caso localizado | Pode conservar “Histórico” para vencido, sem determinar cobrança. |
| `templates/chamados/partials/_meus_arquivados.html:33-34` | Leitor de presença para rótulo `Arquivado` | Outro: exibição persistente | Vencido/rótulo: sem caso localizado | Ler relógio retiraria `Arquivado` do vencido; poderia exibir Histórico. |
| `templates/chamados/partials/_lista_chamados.html:85` | Leitor de presença para botão Resgatar | Outro: ação de limpeza/resgate | Vencido/botão: sem caso localizado | Com guarda temporal, vencido perde o botão; isso também impede esse caminho de limpeza/carimbo do resgate. |
| `chamados/views_arquivar.py:36-37,77` | Escritores indiretos: arquivar e levantar pela porta | Outro | `test_f3_arquivar_porta.py:42-50,65-72`; `test_c4_resgate.py:61-71` | Escritas podem permanecer. |
| `chamados/views_arquivar.py:124-140` — lote | Leitor da partição, guarda `isnull=True`, escritor indireto | **(2)** pela partição; **outro** na guarda de escrita | T1/T2 na dependência; `test_f4_arquivar_lote.py:44-57` não fixa vencido | Curar só verbos ainda deixa vencido protegido por `isnull=True`; curar também a guarda permite silenciá-lo novamente. |
| `chamados/management/commands/arquivar_prescritos.py:50,53-60,93-95` | Leitor da partição e duas guardas `isnull`; escritor indireto | **(2)** pela partição; **outro** nas guardas | `test_f6_prescricao.py::test_idempotente`, linha 54, só prazo ainda futuro | Curar partição e guardas torna vencidos sem resgate candidatos a novo silêncio; cura só da partição não basta para isso. |
| `chamados/management/commands/arquivar_competencia_encerrada.py:100,274-288` | Leitor `isnull`; escritor indireto com horizonte de dez anos | Outro: seleção/idempotência por presença | Vencido: sem caso localizado | Guarda temporal reabre seleção dos expirados e permite novo arquivamento, sujeitos aos demais critérios. |
| `chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:59,88-94` | Leitor `isnull`; escritor indireto com horizonte de cinco anos | Outro: seleção/idempotência por presença | Vencido: sem caso localizado | Mesmo efeito: expirados voltam a ser selecionáveis. |
| `chamados/services/silencio_isentos.py:30,39-47,55-58` | Leitor `isnull`; escritor indireto pela porta | Outro: não retoca quem já tem data, mesmo vencida | `test_varredura_isentos_evento.py::test_idempotente_uma_linha_de_trilha`, linha 87; vencido: sem caso localizado | Trocar guarda por relógio transforma expirados em novos alvos; execução renova silêncio. |
| `chamados/signals.py:256-262` | Leitor do nome em `update_fields` e da presença no objeto | Outro: evento/guarda contra repetição | `test_varredura_isentos_evento.py::test_MORDE_o_resgate_de_isento_volta_ao_veu_pelo_evento`, linha 57; vencido: sem caso localizado | Alterar só linha 261 permite avançar com vencido, mas `silencio_isentos.alvos` ainda o barra. |
| `chamados/signals.py:242,269` | Escritores indiretos via `silenciar_do_colab` | Outro: eventos de colaborador/chamado | `test_varredura_isentos_evento.py:41-68` | Herdam eventual alteração da seleção dos isentos. |
| `chamados/management/commands/silenciar_chamados_isentos.py:36-49` | Leitor/escritor indireto | Outro: contador/vigia ou `--apply` | `test_varredura_isentos_evento.py::test_o_cron_virou_VIGIA_alarme_nao_escreve_e_sai_2`, linha 96 | Se alvos passar a ler relógio, expirados alarmam; com `--apply`, recebem novo silêncio. |
| `chamados/services/silencio_ausencia.py:66,80-91` | Leitor temporal, do marcador; escritor indireto de suspensão/renovação | **(1)**: silêncio futuro alheio manda; vencido pode ser substituído | `test_s4_pendente_suspende_cobranca.py:52-59,93-107`; vencido específico: sem caso localizado | Guarda já temporal. Ausência aberta renova prazo pela porta; não se deve confundir renovação com arquivo persistente. |
| `chamados/services/silencio_ausencia.py:68-75,93-115,127` | Escritor indireto: levanta ou reconcilia | Outro: propriedade pelo motivo e decisão da ausência | `test_s4_pendente_suspende_cobranca.py::test_MORDE_rejeitar_levanta_so_o_marcador`, linha 93 | Aprovação/rejeição seguem seus critérios; relógio não substitui essa reconciliação. |
| `ponto/signals.py:265-269`; `ponto/management/commands/reavaliar_ausencias_lancadas.py:35,54` | Chamadores indiretos da reconciliação de ausência | **(1)/outro**, herdados | `test_s4_pendente_su

[truncado em 20000 de 27428 caracteres]

## 06/11 19:41 · sessao ab4f7e154a11 · - · - · origem=Ronald (tmux)

O histórico por `git log -S` não está disponível nesta cópia: o comando retorna `fatal: not a git repository`; commit/data de inclusão de `'ausencia'` na contagem ficam `[nao verificado]`. A migration somente documenta materialização do previsto e cria o campo com rótulo “Dias de trabalho previstos”; não estabelece filtro por `trabalha` (`app/ponto/migrations/0044_fechamentomensal_minutos_previstos.py:5`, `:21`).

Há documento explícito para o significado **célula diz trabalho**: O155 determina `FechamentoMensal.dias_previstos == dias que a célula diz trabalho`, mas registra o selo como “nao comecada” (`app/docs/BACKLOG.md:1365`); CORTES registra o selo não construído (`app/docs/CORTES.md:200`). Não encontrei teste que compare diretamente `ausencia + trabalha=False` contra `ausencia + trabalha=True` para `FechamentoMensal.dias_previstos` `[nao verificado]`.

| Sítio | Escritor/leitor | Significado suposto | Dinheiro? | Teste | [nao verificado]
|---|---|---|---|---|
| `app/ponto/services/fechamento.py:1342`, `:1358`, `:1364`, `:1375` | Escritor genérico `restaurar_fechamento` | Valor conhecido recebido; não deriva semântica | Pode restaurar outros campos financeiros; `dias_previstos` é snapshot | `[nao verificado]` |
| `app/ponto/management/commands/aplicar_09_corte_b.py:67`, `:184`, `:231`, `:265` | Lê/fotografa/compara e pode restaurar `dias_previstos` | Sem significado próprio; preserva valor recebido | Mudança pode separar colaborador e provocar restauração integral | `[nao verificado]` |
| `app/ponto/management/commands/aplicar_09_he_intra.py:43`, `:125` | Leitor da fotografia | Sem significado próprio | `dias_previstos` está fora de `ALVO` (`:39`); escrita só do alvo (`:137`) | `[nao verificado]` |
| `app/ponto/management/commands/recalcular_fechamento.py:34`, `:54`, `:57`, `:142` | Leitor genérico `foto`/diff | Campo numérico recebido | Medição; não fórmula financeira | `test_o208_contador_do_recalculo.py::ContadorDoRecalculoTest.test_MORDE_os_cinco_campos_da_divergencia` (`:32`, `:71`) fixa visibilidade da mudança, não significado |
| `app/ponto/services/diff_frota.py:62`, `:84`, `:111` | Leitor genérico | Copia/compara campos solicitados | Medição | `[nao verificado]` |
| `app/ponto/management/commands/diff_janela_he_total.py:87`, `:101`, `:110`; `diff_passivo_s84.py:42`, `:50`, `:87`; `diff_col900.py:38`, `:45`, `:74` | Leitores por `CAMPOS`/`diff_frota` | Sem significado próprio | Medição | `[nao verificado]` |
| `app/ponto/management/commands/carimbo_gravado.py:34`, `:53`, `:87` | Leitor/fotografia/diff | Valor gravado | Auditoria | `[nao verificado]` |
| `app/ponto/management/commands/aplicar_janela_he_total.py:65`, `:75`, `:87` | Leitor indireto por `carimbo_gravado.foto` | Valor gravado | Auditoria do ato | `[nao verificado]` |
| `app/ponto/management/commands/lavrar_dias_pagos.py:68`, `:73` | Leitor em hash de todo campo concreto | Valor gravado; não conta dias | Protege fotografia | `[nao verificado]` |
| `app/ponto/portas/regen_exportada.py:56`, `:71`, `:77`, `:84` | Leitor em `hash_do_gravado` e `valores_do_gravado` | Valor gravado | Auditoria/Pauta/retificação | `[nao verificado]` |
| `app/ponto/management/commands/e6_piso_autopsia.py:120` | Leitor/impressão | Valor gravado | Medição | `[nao verificado]` |
| `app/escala/services/propositor.py:484`, `:511`, `:559` | Escritor de dict homônimo independente | **CelulaDia.trabalha=True**, recortado aos dias do vínculo | Conta para proposta de escala, sem leitura do FM | `[nao verificado]` |
| `app/ponto/services/dia_pago.py:34`, `:43`, `:47`, `:99` | Não possui `dias_previstos` em `CAMPOS`; comenta contagem | Contagem derivada das linhas, distinta dos minutos | Excluído do censo de pagamento | `app/folha/tests/test_porta_do_export.py::test_MORDE_o_contador_do_fechamento_NAO_e_cego_a_campo_de_pagamento` (`:454`, `:467`, `:479`) exclui contagens; não fixa filtro |

Os scripts genéricos também carregam o campo sem interpretar seu significado:

- Snapshots: `bin/snapshot_relavratura_10_2026.py:29`, `bin/snapshot_o263_comp10.py:54`.
- **Escritores diretos de snapshot**: `bin/restore_relavratura_10_2026.py:34`, `:79`; `bin/restore_o263_comp10.py:27`, `:48` — fazem `setattr` e `save(update_fields=VALOR)`.
- Recalcula e compara: `bin/aplicar_o263_comp10.py:28`, `:34`, `:51`, `:59`.
- Diffs genéricos: `bin/diff_o118_col221.py:23`, `:37`; `bin/diff_o119_frota.py:27`, `:42`; `bin/diff_o124_frota.py:31`, `:38`, `:55`.
- Snapshot/hash/impressão/recalculo: `bin/relavrar_o118_col221.py:26`, `:30`, `:37`, `:49`, `:53`; `bin/relavrar_o119_col221.py:25`, `:29`, `:36`, `:48`, `:52`; `bin/relavrar_o120_col221_09.py:29`, `:32`, `:41`, `:62`, `:66`, `:74`, `:79`.

Os testes explícitos sobre o valor de `dias_previstos` fixam **ausência de resposta versus zero**, não os dois significados discutidos: [nao verificado]

- `app/ponto/tests/test_o48_sem_celula_nao_e_zero.py::SemCelulaNaoEZero.test_MORDE_sem_nenhuma_celula_o_objeto_nao_diz_zero_minuto_de_quem_trabalhou`: sem célula → `None` (`:96`, `:110`).
- Mesmo arquivo `::test_MORDE_admitido_no_meio_sem_celula_no_vinculo_tambem_nao_diz_zero`: mistura `fora_vinculo + sem_celula` → `None` (`:153`, `:178`).
- Mesmo arquivo `::test_fora_do_vinculo_INTEIRO_e_RESPOSTA_zero_nunca_silencio`: janela inteira `fora_vinculo` → `0` (`:185`, `:207`).
- `app/relatorios/test_espelho_falta.py::EspelhoFaltaAbonoTest.test_falta_e_abono_surfam_no_resumo` usa `_dias_previstos = ec.eh_dia_trabalho` (`:28`, `:33`, `:41`), mas verifica somente faltas e abonos (`:55`).

Há testes da **classificação**, sem fixar contagem mensal: [nao verificado]

- `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py::AusenciaSemVinculoPorTurno.test_MORDE_propriedade_ausencia_nao_falta_com_trabalha_false_nao_vira_folga_na_ata`, também herdado por `AusenciaSemVinculoLegacy`: confirma `trabalha=False` e ata `'ausencia'` (`:83`, `:89`, `:91`, `:94`, `:99`).
- `app/ponto/tests/test_retrato_exportada.py::PorDiaDaGradeTest.test_MORDE_as_tres_leis` fixa inclusão de `tipo_dia='ausencia'` em mapa de **minutos previstos**, exclusão de `'folga'` e cap realizado (`:46`, `:49`, `:58`); a fixture não fornece `trabalha`.
- `app/escala/tests/test_montador_realizado_pela_autoridade.py::test_MORDE_a_folga_trabalhada_NAO_entra_no_dinheiro` fixa exclusão de `'folga'` dos três mapas e conservação das somas (`:454`, `:479`, `:484`, `:489`).

## 06/11 19:40 · sessao 7c3ed4001837 · - · - · origem=Ronald (tmux)

**4. `calcular_mes` recebe bordas e filtra períodos depois do pareamento.**

- `MotorBase.calcular_mes` copia os objetos de entrada (`app/ponto/motor_calculo_v2.py:2268`; cópia rasa de cada objeto em `:2263`), entrega a lista ao juiz (`:2346`) e a `remover_duplicatas`/`montar_periodos` (`:2351`, `:2354`). `remover_duplicatas` filtra tipos E/S e duplicatas, sem recorte por data (`:902`, `:908-911`).
- O recorte inclusivo `[data_ini, data_fim]` ocorre **sobre cada período**, descartando se `data_jornada < data_ini` ou `data_jornada > data_fim` (`app/ponto/motor_calculo_v2.py:2429-2432`).
- `data_jornada` parte do `data_turno` fornecido pelo juiz; sem chave, usa `localizar(entrada).date()` (`app/ponto/motor_calculo_v2.py:2403-2404`). O mapa liga cada E ao `t.data_turno` (`:889-894`).
- Depois pode herdar o dia do período anterior se o intervalo entre sua entrada e a saída anterior for de 0 a 6 horas (`app/ponto/motor_calculo_v2.py:2405-2409`); só registra saída anterior quando existe (`:2423`).
- Portanto, uma batida com dia civil fora da janela pode contribuir para um período cujo **dia da jornada** esteja dentro; a guarda não recorta cada batida por seu `.date()` (`app/ponto/motor_calculo_v2.py:2354`, `:2403-2409`, `:2429-2432`).
- O fallback é **dia civil local**, não UTC: `TZ_LOCAL = pytz.timezone(settings.TIME_ZONE)` (`app/ponto/motor_calculo_v2.py:28`); `localizar` atribui o fuso local ao naive e converte aware com `astimezone` (`:73-79`).
- `MotorTurnoPartido.calcular_mes` e `MotorComercial.calcular_mes` passam primeiro pelo `super().calcular_mes` (`app/ponto/motor_calculo_v2.py:2908`, `:3301`). Seus agrupamentos posteriores também usam `data_turno`, com fallback na data local da entrada (`:2941-2942`, `:3312-3314`).

**5. Funções públicas candidatas.**

| Função | Contrato/retorno | Chamadores diretos de produção |
|---|---|---|
| `ponto.turnos.data_local(ts)` | `timezone.localtime(ts).date()`, ou `None`; docstring exige instante aware (`app/ponto/turnos.py:43-46`). Não protege naive (`:46`). | `ponto.signals._recompute_around`: `app/ponto/signals.py:21-23`; comando de produção `detectar_par_relampago`: `app/ponto/management/commands/detectar_par_relampago.py:302-303`; internos abaixo. |
| `core.filtro.dia_de(valor)` | Aware → data local (`app/core/filtro.py:131-133`); naive/erro de conversão → `valor.date()` (`:134-135`); aceita também `date` e `None` (`:129-130`, `:136-138`). | `core.filtro.aplicar`, `app/core/filtro.py:194`. O caminho efetivamente usado para datetimes de holerite vem de `avisado_em`/`publicado_em`, `app/holerite/views.py:407-411`. |

Chamadas internas de `data_local`, todas em produção:

- `_data_do_turno`: `app/ponto/turnos.py:284`.
- `_mk`: `app/ponto/turnos.py:320`.
- `_saida_pela_posicao_entre_turnos`: `app/ponto/turnos.py:1013`.
- `_fechar_aberto_com_saida_seguinte`: `app/ponto/turnos.py:1098`.
- `_fechar_aberto_na_pausa_sem_volta`: `app/ponto/turnos.py:1186`.
- Função aninhada `_vespera` de `parear_turnos`: `app/ponto/turnos.py:1294`.
- `par_completo`: `app/ponto/turnos.py:2198`.
- `_dia_das_batidas_puro`: `app/ponto/turnos.py:2918`.

Candidatas próximas, mas com contrato diferente:

- `core.fmt.hora_local(dt, fmt=...)` converte aware para hora local e devolve **string**, não `date` (`app/core/fmt.py:10-15`). Exemplos de chamadores: `app/chamados/services/materializacao.py:450`, `app/ponto/services/regularizacao.py:139`, `app/ponto/services/justificativa.py:386`, `app/ponto/models.py:1024`.
- `ponto.turnos.dia_das_batidas` responde **dia atribuído à batida**: `data_turno` quando reclamada por turno e data civil local para as restantes (`app/ponto/turnos.py:2874-2881`, `:2912-2918`), logo não é helper puro “dia local de instante”. Chamadores diretos: `app/ponto/services/espelho.py:450-451`, `app/api/views.py:1445-1446`; há também import com alias em `app/api/views.py:2037`.

**Complemento útil para a pergunta 6:** o motor prevê explicitamente timestamps naive, normalizando-os no seu caminho de pareamento (`app/ponto/motor_calculo_v2.py:458-461`). `turnos_de_batidas` também normaliza naive nas cópias (`app/ponto/turnos.py:1524-1529`). Isso comprova suporte defensivo a naive; não comprova, sozinho, uma origem naive alcançando `apurar_por_fatia` [nao verificado].

_2492 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
