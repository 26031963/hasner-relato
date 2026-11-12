# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 08:33** · publicado em **12/11/2026 08:34** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 08:33 · sessao 944391eae9b5 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**REPROVA c11:** a semana do posto transforma a recusa de julgamento em **dia explicado**, retirando-o dos buracos e podendo esconder o posto do contador. O próprio teste exige essa classificação (`app/escala/services/semana_do_posto.py:152`, `app/escala/services/semana_do_posto.py:237`, `app/ponto/tests/test_lastro_c11_sem_banco.py:270`). O par financeiro da competência 11 exigido pelo aval continua **[nao verificado]**.

1. **Achados da c10 e mordida**

| Achado anterior | Conclusão na c11 | Teste e mutante | [nao verificado]
|---|---|---|
| `montar_semanas` relançava a recusa | **Curado no caminho reproduzido:** os dois ramos D4 consultam A2 com `recusas=`; o ramo alimentado lê fatos e recusas recebidos; devolve `erro.rotulo` (`app/ponto/status_dia.py:302`, `app/ponto/status_dia.py:307`, `app/ponto/status_dia.py:314`, `app/ponto/status_dia.py:318`). | O teste passa; retirar `recusas=` nos mutantes `d4_lote` e `d4_unitario` produz falha por asserção, reproduzida nesta verificação (`app/ponto/tests/test_lastro_c11_sem_banco.py:47`, `app/ponto/tests/test_lastro_c11_sem_banco.py:121`, `app/ponto/tests/test_lastro_c11_sem_banco.py:136`). |
| Porta aceitava `codigo` em `campos` | **Curado:** rejeita antes de qualquer `setattr`/`save`, inclusive código repetido; a guarda de faixa consulta a identidade final (`app/ponto/services/ausencia.py:1306`, `app/ponto/services/ausencia.py:1309`, `app/ponto/services/ausencia.py:1314`). | Os pares passam; `porta_codigo` e `porta_final` falharam por asserção nesta execução (`app/ponto/tests/test_lastro_c11_sem_banco.py:53`, `app/ponto/tests/test_lastro_c11_sem_banco.py:152`, `app/ponto/tests/test_lastro_c11_sem_banco.py:162`). |
| Contador com duas unidades | **Curado:** cartório declara/incrementa/imprime `colabs_isolados_tipo_sem_faixa`; vigia conserva o contador de códigos ativos (`app/ponto/services/cartorio.py:1954`, `app/ponto/management/commands/processar_cartorio.py:73`, `app/ponto/services/vigia_ausencia.py:168`). | O teste passa; `contador` restabelece o nome antigo e falhou por asserção (`app/ponto/tests/test_lastro_c11_sem_banco.py:62`, `app/ponto/tests/test_lastro_c11_sem_banco.py:167`). |
| Paridade das órfãs PDF/tela | **Esclarecido e coberto no caso pedido:** tela publica `orfas_grade`; PDF lê essa saída. Sem célula, o caso ensaiado entrega órfãs vazias (`app/ponto/services/espelho.py:522`, `app/ponto/services/espelho.py:853`, `app/relatorios/pdf_espelho.py:477`, `app/ponto/tests/test_lastro_c11_sem_banco.py:299`). | O coletor ensaiado passa; `pdf_builder` injeta `07:02` e falhou em `assertFalse(batida.orfa)`. Esse mutante adultera o mapa, **não executa o builder antigo restaurado** (`app/ponto/tests/test_lastro_c11_sem_banco.py:68`, `app/ponto/tests/test_lastro_c11_sem_banco.py:354`). |

Executei os quatro harnesses: **55 testes, OK, RC 0**; executei os dez mutantes C11 individualmente: todos terminaram **RC 1 por asserção**. São ensaios AST com alimentação externa substituída, não integração HTTP/ORM (`app/ponto/tests/test_lastro_c5_sem_banco.py:1`, `app/ponto/tests/test_lastro_c11_sem_banco.py:1`, `app/ponto/tests/test_lastro_c11_sem_banco.py:42`). Reexecução histórica contra o commit c10: **[nao verificado]**; o carregador depende de `git show` (`app/ponto/tests/test_lastro_c11_sem_banco.py:33`).

O teste PDF substitui o espelho completo, a apuração e `folha_manda`; portanto não certifica renderização binária nem paridade financeira integral (`app/ponto/tests/test_lastro_c11_sem_banco.py:317`, `app/ponto/tests/test_lastro_c11_sem_banco.py:343`, `app/ponto/tests/test_lastro_c11_sem_banco.py:347`).

2. **Censo dos chamadores**

Na tabela: **a** preserva a recusa nomeada; **b** propaga quando alcança a consulta; **c** absorve, omite ou reclassifica. As condições anteriores à consulta importam: ter o tipo legado no cadastro, sozinho, não garante que todos esses ramos sejam alcançados (`app/ponto/precedencia.py:417`, `app/ponto/status_dia.py:283`, `app/ponto/services/faltas_de_hoje.py:59`).

| Chamador direto, fora de tests/migrations | Destino |
|---|---|
| `precedencia.fatos_do_dia` → período | **b**: forma unitária sem `recusas=` (`app/ponto/precedencia.py:286`). |
| `escala.utils._marcar_ausencias` → período | **b**: builder consulta sem destinatário de recusas (`app/escala/utils.py:1209`). |
| `folha.services.sem_furo.dias_com_furo` → período | **b**, se houver marcos apagados; command `colabs_sem_furo` (`app/folha/services/sem_furo.py:23`, `app/folha/services/sem_furo.py:25`, `app/folha/management/commands/colabs_sem_furo.py:13`). |
| Calendário → período | **a**: transporta as duas respostas da A2 ao D4/compositor (`app/colaboradores/services/calendario.py:510`, `app/colaboradores/services/calendario.py:607`, `app/colaboradores/services/calendario.py:719`). |
| Espelho, alias `_fatos_periodo` → período | **a**: recebe recusas e transporta seu rótulo (`app/ponto/services/espelho.py:653`, `app/ponto/services/espelho.py:880`). |
| `dia_do_colab` → período | **a**: rótulo preservado, natureza desconhecida nula (`app/ponto/services/dia_do_colab.py:16`, `app/ponto/services/dia_do_colab.py:94`). |
| D4 → período, sem carga/com carga | **a**, quando alcança A2 (`app/ponto/status_dia.py:302`, `app/ponto/status_dia.py:311`, `app/ponto/status_dia.py:318`). |
| `fatos_do_status` → dia | **b**, somente para veredito `indefinida`; insumo da lavra (`app/ponto/status_dia.py:177`, `app/ponto/status_dia.py:181`, `app/ponto/services/cartorio.py:932`). |
| `feriado_reavaliar.espurios` → dia | **b** no command `reavaliar_ausencias_feriado`; **c** nos eventos protegidos por captura (`app/chamados/services/feriado_reavaliar.py:33`, `app/ponto/management/commands/reavaliar_ausencias_feriado.py:39`, `app/chamados/signals.py:332`, `app/ponto/portas/celula.py:499`). |
| Semana do posto → dia | Rótulo chega, mas **c**: vira explicação válida para classificação/contagem (`app/escala/services/semana_do_posto.py:179`, `app/escala/services/semana_do_posto.py:152`). |
| `detectar_ausencias` → dia | **b**, ao alcançar o gate de feriado (`app/ponto/management/commands/detectar_ausencias.py:616`). |
| `deslogados` → dia | **b**: `lavrar_deslogados` e apuração de pautas (`app/ponto/services/deslogados.py:166`, `app/ponto/management/commands/lavrar_deslogados.py:38`, `app/ponto/services/pautas_dos_deslogados.py:62`). |
| `abriu_nao_bateu` → dia | **b**, com filtro de previsão ativo; command `lavrar_abriu_nao_bateu` (`app/ponto/services/abriu_nao_bateu.py:123`, `app/ponto/management/commands/lavrar_abriu_nao_bateu.py:30`). |
| `faltas_de_hoje` → dia | **b**, se passar pelas guardas anteriores; command `faltas_de_hoje_sem_cobranca` (`app/ponto/services/faltas_de_hoje.py:59`, `app/ponto/services/faltas_de_hoje.py:61`, `app/ponto/management/commands/faltas_de_hoje_sem_cobranca.py:16`). |
| Cartório, `aplicar_precedencia_na_celula` → dia | Levanta localmente; **c** na consulta tardia protegida de `julgar_celula`, que loga e retorna sem novo carimbo (`app/ponto/services/cartorio.py:1998`, `app/ponto/services/cartorio.py:2047`, `app/ponto/services/cartorio.py:2053`). |
| Férias, `_repouso` → dia | Aviso nomeado, retorno `None`, sem concluir repouso (`app/ferias/situacao.py:95`, `app/ferias/situacao.py:98`). |
| `status_do_dia_lavrado` → lote | Delega ao mesmo corpo (`app/ponto/status_dia.py:218`). |
| `montar_semanas` → D4 | **a** no caso reprovado na c10; copia status/rótulo (`app/ponto/services/espelho.py:170`, `app/ponto/services/espelho.py:174`). |
| Calendário → D4 | **a**, com fatos/recusas alimentados (`app/colaboradores/services/calendario.py:606`). |
| API espelho v2 → D4 | Lê somente status; **c** fora da janela da palavra do espelho, detalhado abaixo (`app/api/views.py:1483`, `app/api/views.py:1584`, `app/api/views.py:1586`). |
| Auditoria de furos → D4 | **c no relatório filtrado**: conserva status, cobra apenas `irregular`; recusa não aparece como furo (`app/relatorios/furos.py:123`, `app/relatorios/furos.py:128`, `app/relatorios/furos.py:141`). |
| Painel operacional → D4 | Consulta pares encerrados com entrada; normalmente recebe lavra ou não certificação genérica, sem consultar A2 (`app/core/services/painel_op.py:329`, `app/core/services/painel_op.py:336`, `app/ponto/status_dia.py:295`). |
| Compositor → `dia_decidido.do_dia` | **a**: recusa tem ramo anterior e não entra no tradutor de precedência (`app/ponto/services/espelho.py:339`, `app/ponto/services/espelho.py:343`). |
| API/ficha → `dia_do_colab` | **a**: serializam/transportam o resultado (`app/api/views_mensageria.py:1970`, `app/colaboradores/services/ficha.py:280`). |

Os outros `do_dia` encontrados pertencem ao calculador, vigia de entrada e pergunta no app; são contratos distintos do tradutor de precedência (`app/ponto/calculador/regras.py:576`, `app/ponto/services/vigia_entrada.py:111`, `app/chamados/services/pergunta_no_app.py:211`).

**Destinos indiretos b/c relevantes pelo builder:**

- **b — `/ponto/celula/veredito/`**, ramo `trabalhou`: view → serviço → `grade_dia` → builder sem recusas. É ação que necessita julgamento, não evidência de que devesse fabricar resultado tolerante (`app/ponto/urls.py:58`, `app/ponto/views.py:3340`, `app/ponto/services/veredito_celula.py:241`, `app/ponto/services/simular_dia.py:7`).
- **b — `/ponto/flip-fila/` e command `flip_automatico`**: avaliação usa a mesma grade (`app/ponto/urls.py:63`, `app/ponto/views.py:3305`, `app/ponto/services/flip_auto.py:303`, `app/ponto/management/commands/flip_automatico.py:32`).
- **b — command `detectar_par_relampago`**: usa builder no score (`app/ponto/management/commands/detectar_par_relampago.py:136`).
- **c — command `reconciliar_grade`**: captura, incrementa `GRADE_FAIL` e pula colaborador (`app/ponto/management/commands/reconciliar_grade.py:118`, `app/ponto/management/commands/reconciliar_grade.py:120`).
- **c — grade de apresentação do espelho**: falha pode virar grade vazia; consulta A2 independente ainda preserva a palavra da recusa (`app/ponto/services/espelho.py:572`, `app/ponto/services/espelho.py:653`).
- **Isolamento autorizado — `processar_cartorio`**: captura por colaborador, conta e loga; demais colaboradores seguem (`app/ponto/management/commands/processar_cartorio.py:64`, `app/ponto/management/commands/processar_cartorio.py:72`).

**Achado — fila: rótulo perdido no app futuro.** Contraexemplo: hoje 12/11, ausência legada `fantasma_xyz` em 13/11, sem batida. D4 devolve a recusa; a API `/api/espelho/v2/` descarta `rotulo`. Como o espelho monta dias somente até hoje, a palavra de 13/11 cai no vazio: `status='neutro'`, `status_dia='sem_certidao'`, sem o nome da recusa (`app/ponto/status_dia.py:318`, `app/ponto/services/espelho.py:1272`, `app/api/views.py:1390`, `app/api/views.py:1584`, `app/api/views.py:1594`, `app/api/urls.py:81`). Ocorrência produtiva: **[nao verificado]**.

Não sustento quebra natural pelo fechamento apenas porque seus handlers relançam: a grade financeira identificada lê a célula, sem chamar A2. Propagação se ocorrer é demonstrada; geração natural dessa exceção nesse caminho permanece **[nao verificado]** (`app/folha/export.py:335`, `app/ponto/services/fechamento.py:1089`, `app/ponto/services/fechamento.py:1308`).

3. **`sem_certidao`: não encontrei falsificação semântica por esse valor**

A definição vigente já significa situação não certificada por evidência/decisão insuficiente. Uma faixa que não pode ser julgada cabe nessa definição; não significa exclusivamente “não existe ata” (`app/ponto/status_dia.py:45`).

| Leitor | Sentido observado |
|---|---|
| Contador/calendário | Soma no balde de não certificados, separado de faltas e ausências (`app/colaboradores/services/calendario.py:773`, `app/colaboradores/services/calendario.py:785`, `app/colaboradores/services/calendario.py:789`). |
| Badges/tooltips | Rodapé usa a definição geral; dia pode mostrar rótulo específico (`app/templates/colaboradores/partials/_calendario_grade.html:107`, `app/templates/colaboradores/partials/_calendario_grade.html:177`, `app/templates/ponto/espelho.html:151`). |
| App | Traduz para neutro, não para falta ou irregularidade; há a perda de rótulo futuro já apontada (`app/ponto/status_dia.py:84`, `app/api/views.py:1604`, `app/api/views.py:1607`). |
| Auditoria | Não cobra como furo (`app/relatorios/furos.py:128`). |
| Chamados | Pode oferecer “Resolver dia” no passado, com último recurso; vigia do fio lê essa oferta (`app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:103`, `app/chamados/services/vigia_fio.py:271`). |
| Folha/cartório | Não localizei consumo financeiro desse retorno transitório D4. A escrita do status usa os contratos próprios da lavra/admin (`app/ponto/status_dia.py:198`, `app/ponto/portas/celula.py:958`, `app/ponto/portas/celula.py:970`). |

D4 continua priorizando andamento e status lavrado antes de consultar A2. Portanto, **seu próprio rótulo não é universalmente o da recusa**; no espelho, a palavra separada pode preservá-la (`app/ponto/status_dia.py:283`, `app/ponto/status_dia.py:290`, `app/ponto/services/espelho.py:339`).

4. **Calendário válido: transporte equivalente à chamada da base**

Não encontrei contraexemplo no delta: conserva presença, cobertura, decisão, marcos faltantes e pertinência a `_em_aberto`; o compositor repassa esses argumentos (`app/colaboradores/services/calendario.py:713`, `app/ponto/services/espelho.py:343`).

`pendente` continua `None`: antes era omitido; agora `_pendente_do_dia` também é omitido e lido por `.get`. `ata_sem_turno` retorna inteiro positivo ou zero, preservado pelo `or 0` do compositor (`app/ponto/services/dia_decidido.py:399`, `app/ponto/services/espelho.py:334`, `app/ponto/services/dia_decidido.py:179`, `app/ponto/services/espelho.py:345`). Palavra/cor/veredito são copiados da mesma resposta de `do_dia` (`app/ponto/services/espelho.py:347`, `app/colaboradores/services/calendario.py:723`).

O teste C11 sela a recusa, com tradutor substituído; não sela paridade integral de dias válidos (`app/ponto/tests/test_lastro_c11_sem_banco.py:219`, `app/ponto/tests/test_lastro_c11_sem_banco.py:230`).

5. **Semana do posto: bloqueia — recusa vira explicação**

O consumidor aceita qualquer string diferente de `'trabalho'` como explicação. Assim, `erro.rotulo` ocupa o mesmo canal de uma faixa reconhecida, entra em `explicados`, sai de `buracos` e deixa de participar das propostas (`app/escala/services/semana_do_posto.py:152`, `app/escala/services/semana_do_posto.py:154`, `app/escala/services/semana_do_posto.py:179`, `app/escala/services/semana_do_posto.py:197`).

**Contraexemplo:** posto com um único colaborador previsto, sem batida, com `fantasma_xyz`: `buracos=[]`, `explicados=[recusa]`; contador sem posto omite esse posto; consulta específica publica `dias_sem_explicacao=0`. Recusar o julgamento passou a valer como explicar o dia (`app/escala/services/semana_do_posto.py:237`, `app/api/views_mensageria.py:2083`, `app/api/views_mensageria.py:2121`).

O teste exige precisamente `buracos=[]`: morde o transporte do rótulo, mas **sela a mistura semântica** (`app/ponto/tests/test_lastro_c11_sem_banco.py:270`).

`explicados` tem consumidor por transporte integral até o contexto do copiloto; não encontrei leitura tipada específica nem template/JS dessa chave. A interpretação final pelo modelo é **[nao verificado]** (`mensageria/nucleo/core_client.py:308`, `mensageria/nucleo/ferramentas.py:884`, `mensageria/nucleo/ferramentas.py:422`, `mensageria/nucleo/copiloto.py:379`).

6. **`dia_do_colab`: sem leitor identificado que quebre com esses `None`**

O ramo recusado entrega rótulo, natureza nula e evita `_veredito_do_dia` (`app/ponto/services/dia_do_colab.py:94`, `app/ponto/services/dia_do_colab.py:108`). Os leitores encontrados serializam o dicionário inteiro ou o incluem na ficha; `_json` preserva `None` (`app/api/views_mensageria.py:1970`, `app/colaboradores/services/ficha.py:253`, `app/colaboradores/services/ficha.py:280`). A mensageria transporta o resultado, sem iterar `escala.ausencias` nem desreferenciar `escala.tipo` (`mensageria/nucleo/ferramentas.py:817`, `mensageria/nucleo/ferramentas.py:1081`).

HTTP completo e consumidores externos à cópia: **[nao verificado]**.

7. **Rename: sem órfão operacional localizado na árvore**

Cartório declara/incrementa/imprime exclusivamente o contador de colaboradores; teste exige ausência do alias antigo (`app/ponto/services/cartorio.py:1954`, `app/ponto/management/commands/processar_cartorio.py:73`, `app/ponto/management/commands/processar_cartorio.py:80`, `app/ponto/tests/test_lastro_c11_sem_banco.py:169`).

O nome antigo preservado pertence ao vigia: códigos ativos. API e revisão semanal leem explicitamente esse vigia; não passaram a ler o contador do cartório (`app/ponto/services/vigia_ausencia.py:160`, `app/api/views_mensageria.py:1416`, `app/api/views_mensageria.py:1422`, `app/ponto/services/vigia_ausencia.py:259`). A descrição documental encontrada também se refere ao vigia; cron não interpreta esse campo (`app/docs/RELATO.md:37`, `app/config/crons.py:616`). Parser externo à cópia: **[nao verificado]**.

8. **Classes de reprovação**

- **Campo com dois significados — bloqueia:** faixa reconhecida e recusa compartilham o canal que significa “dia explicado”; contraexemplo do posto acima (`app/escala/services/semana_do_posto.py:152`, `app/escala/services/semana_do_posto.py:179`).
- **Rótulo descartado — fila:** app futuro converte recusa nomeada em neutro sem palavra (`app/api/views.py:1584`, `app/api/views.py:1586`).
- **Fallback — fila, preexistente:** catálogo troca falha/vazio por catálogo congelado. Contraexemplo: `abono` cadastrado para descontar pode voltar ao efeito congelado após falha de leitura; ficou expressamente fora desta volta (`app/ponto/catalogo/ausencias.py:254`, `app/ponto/catalogo/ausencias.py:283`, `app/ponto/catalogo/ausencias.py:298`).
- **Except que engole — fila:** semana converte outros erros em `'trabalho'`; reconciliação converte em `GRADE_FAIL`; cartório captura falha na consulta tardia. Para esta última, alcançar a recusa apenas na segunda consulta exige mudança de alimentação entre consultas, **[nao verificado]** (`app/escala/services/semana_do_posto.py:180`, `app/ponto/management/commands/reconciliar_grade.py:119`, `app/ponto/services/cartorio.py:2048`).
- **Selo sem mordida:** não procede como acusação geral aos testes novos; os mutantes falharam. Permanecem os limites do PDF substituído e o teste da semana que exige o resultado semanticamente errado (`app/ponto/tests/test_lastro_c11_sem_banco.py:42`, `app/ponto/tests/test_lastro_c11_sem_banco.py:270`, `app/ponto/tests/test_lastro_c11_sem_banco.py:347`).
- **Segundo juiz/segundo escritor/relógio novo/derivação paralela:** sem achado novo nos sítios lidos. Faixa continua por indexação na autoridade; cadastro passa pela porta; calendário usa compositor comum; PDF lê órfãs da tela e deriva `_hj` do instante recebido (`app/ponto/precedencia.py:108`, `app/ponto/views_tipos_ausencia.py:87`, `app/ponto/views_tipos_ausencia.py:98`, `app/colaboradores/services/calendario.py:719`, `app/relatorios/pdf_espelho.py:475`, `app/relatorios/pdf_espelho.py:477`).

## 12/11 08:30 · sessao 18c53079f346 · - · - · origem=Ronald (tmux)

Retiro o achado de quebra natural por `folha_manda`: não localizei ligação fechamento → A2. O fechamento chama `grade_do_fechamento`, que apenas lê a célula (`app/ponto/services/fechamento.py:1014`, `app/folha/export.py:335`, `app/folha/export.py:338`).

As capturas comprovam propagação **se** `TipoSemFaixa` ocorrer; não comprovam que o tipo legado provoque essa exceção nesse caminho (`app/ponto/services/fechamento.py:1089`, `app/ponto/services/fechamento.py:1308`). Alcance natural: [nao verificado].

## 12/11 08:30 · sessao 18c53079f346 · - · - · origem=Ronald (tmux)

Censo direto concluído, fora tests/migrations; os homônimos de `do_dia` no calculador, vigia de entrada e pergunta no app têm contratos distintos (`app/ponto/calculador/regras.py:576`, `app/ponto/services/vigia_entrada.py:111`, `app/chamados/services/pergunta_no_app.py:211`).

**Achado principal confirmado — bloqueia a promessa espelho/PDF rotular:** a chamada posterior a `folha_manda` reentra no fechamento online e levanta. Cadeia: tela chama espelho (`app/ponto/views.py:158`); espelho chama `_folha_manda` sem captura (`app/ponto/services/espelho.py:1294`); competência exata chama totais (`app/relatorios/cartao_pela_celula.py:321`); existindo fechamento gravado, totais chama `fechamento_lido` (`app/relatorios/cartao_pela_celula.py:71`, `app/relatorios/cartao_pela_celula.py:73`); leitura recalcula e propaga `TipoSemFaixa` (`app/ponto/services/fechamento.py:1766`, `app/ponto/services/fechamento.py:1089`, `app/ponto/services/fechamento.py:1308`). PDF entra no próprio espelho e ainda chama folha novamente (`app/relatorios/pdf_espelho.py:475`, `app/relatorios/pdf_espelho.py:587`). URL da tela `ponto/espelho/`, app `api/espelho/v2/`, PDF avulso `relatorios/espelho/avulso/` (`app/ponto/urls.py:10`, `app/api/urls.py:81`, `app/relatorios/urls.py:28`). No PDF informacional, geração sem captura está em `app/relatorios/views.py:1098`; o caminho mensal passa pelo serviço (`app/relatorios/services.py:70`). Lote tenta auditável, depois informacional e finalmente registra colaborador pulado: não garante PDF com rótulo (`app/relatorios/services.py:244`, `app/relatorios/services.py:250`, `app/relatorios/services.py:254`).

**Outro achado concreto — fila, recusa some no app fora da janela do espelho.** Hoje 12/11, ausência legada aprovada `fantasma_xyz` em 13/11 sem batida: D4 obtém a recusa, mas API descarta seu `rotulo`, serializando só status (`app/ponto/status_dia.py:318`, `app/api/views.py:1584`). Palavra vem do espelho, que monta somente até hoje (`app/ponto/services/espelho.py:1272`). Para 13/11, `.get` usa palavra vazia e resposta vira `status='neutro'`, `status_dia='sem_certidao'`, sem nome da recusa (`app/api/views.py:1390`, `app/api/views.py:1586`, `app/api/views.py:1594`, `app/api/views.py:1604`). Endpoint: `app/api/urls.py:81`. Ocorrência produtiva [nao verificado].

Censo direto:

| Chamador | Resultado ao alcançar o ramo |
|---|---|
| `fatos_do_dia` → período | Propaga por definição unitária (`app/ponto/precedencia.py:286`). |
| `_marcar_ausencias` → período | **b**, consulta sem `recusas` (`app/escala/utils.py:1209`). Alimenta todos os builders (`app/escala/utils.py:807`, `app/escala/utils.py:1008`, `app/escala/utils.py:1863`). |
| `dias_com_furo` → período | **b**, command `colabs_sem_furo`, condicionado a ter marco apagado (`app/folha/services/sem_furo.py:23`, `app/folha/services/sem_furo.py:25`, `app/folha/management/commands/colabs_sem_furo.py:13`). |
| calendário → período | **a**, transporta recusas; outras exceções logadas e relançadas (`app/colaboradores/services/calendario.py:507`, `app/colaboradores/services/calendario.py:510`, `app/colaboradores/services/calendario.py:515`, `app/colaboradores/services/calendario.py:719`). |
| ficha/API `dia_do_colab` → período | **a**, campos desconhecidos nulos, rótulo preservado (`app/ponto/services/dia_do_colab.py:16`, `app/ponto/services/dia_do_colab.py:85`, `app/ponto/services/dia_do_colab.py:105`). |
| espelho `_fatos_periodo` → período | **a no miolo**; chamada posterior à folha pode quebrar, como acima (`app/ponto/services/espelho.py:653`, `app/ponto/services/espelho.py:880`, `app/ponto/services/espelho.py:1294`). |
| D4, sem carga → período | **a**, quando alcança A2 (`app/ponto/status_dia.py:302`, `app/ponto/status_dia.py:318`). |
| D4, lote com carga sem fatos → período | **a**, quando alcança A2 (`app/ponto/status_dia.py:311`, `app/ponto/status_dia.py:318`). |
| `feriado_reavaliar.espurios` → dia | **b** no command `reavaliar_ausencias_feriado`; em eventos é **c**, loga e deixa efeito pendente (`app/chamados/services/feriado_reavaliar.py:33`, `app/ponto/management/commands/reavaliar_ausencias_feriado.py:39`, `app/chamados/signals.py:332`, `app/ponto/portas/celula.py:499`). |
| semana do posto → dia | **a** devolvida como string de fato; recusa não vira `'trabalho'`; outros erros seguem fallback (`app/escala/services/semana_do_posto.py:177`, `app/escala/services/semana_do_posto.py:178`, `app/escala/services/semana_do_posto.py:180`). |
| `fatos_do_status` → dia | **b**, só para veredito `indefinida`; usado na lavra do cartório (`app/ponto/status_dia.py:177`, `app/ponto/status_dia.py:181`, `app/ponto/services/cartorio.py:932`, `app/ponto/services/cartorio.py:1292`, `app/ponto/services/cartorio.py:1853`). |
| `detectar_ausencias` → dia | **b** ao chegar ao gate de feriado (`app/ponto/management/commands/detectar_ausencias.py:616`). |
| deslogados → dia | **b**, commands `lavrar_deslogados` e serviço de pautas; API lê snapshot (`app/ponto/services/deslogados.py:166`, `app/ponto/management/commands/lavrar_deslogados.py:38`, `app/ponto/services/pautas_dos_deslogados.py:62`). |
| abriu/não bateu → dia | **b**, command `lavrar_abriu_nao_bateu`, salvo `--todos` desativar filtro (`app/ponto/services/abriu_nao_bateu.py:123`, `app/ponto/management/commands/lavrar_abriu_nao_bateu.py:30`). |
| faltas de hoje → dia | **b**, command `faltas_de_hoje_sem_cobranca`, só se sobreviver às guardas anteriores (`app/ponto/services/faltas_de_hoje.py:59`, `app/ponto/services/faltas_de_hoje.py:61`, `app/ponto/management/commands/faltas_de_hoje_sem_cobranca.py:16`). |
| aplicar precedência na célula → dia | Levanta localmente; `julgar_celula` captura/loga a consulta tardia e retorna out, **c** sem carimbo novo (`app/ponto/services/cartorio.py:1998`, `app/ponto/services/cartorio.py:2046`, `app/ponto/services/cartorio.py:2053`). No dado estável, recusa normalmente ocorre antes no builder; para alcançar captura tardia seria alimentação diferente entre consultas [nao verificado]. |
| férias `_repouso` → dia | **a** aviso, devolve `None` e não conclui repouso (`app/ferias/situacao.py:95`, `app/ferias/situacao.py:96`, `app/ferias/situacao.py:98`). |
| forma unitária D4 → lote D4 | Delega, sem nova regra (`app/ponto/status_dia.py:218`). |
| espelho `montar_semanas` → D4 | **a** no ramo antes reprovado; atualiza status/rótulo (`app/ponto/services/espelho.py:170`, `app/ponto/services/espelho.py:174`). |
| calendário → D4 | **a** com fatos e recusas alimentados (`app/colaboradores/services/calendario.py:606`). |
| API espelho v2 → D4 | lê status, traduz neutro; palavra separada preserva recusa somente onde espelho fornece dia (`app/api/views.py:1483`, `app/api/views.py:1500`, `app/api/views.py:1534`, `app/api/views.py:1586`). |
| auditoria furos → D4 | **c no relatório principal**, retém só status e cobra só irregular; recusa não é furo e colaborador pode sair do relatório filtrado (`app/relatorios/furos.py:123`, `app/relatorios/furos.py:127`, `app/relatorios/furos.py:128`, `app/relatorios/furos.py:141`). Calendário anexado preserva rótulo quando card incluído (`app/relatorios/furos.py:160`). |
| painel op → D4 | Só pares encerrados com entrada; normalmente não alcança A2, mostra lavrado ou sem-certidão normal (`app/core/services/painel_op.py:329`, `app/core/services/painel_op.py:334`, `app/core/services/painel_op.py:336`, `app/ponto/status_dia.py:295`). |
| compositor espelho → `dia_decidido.do_dia` | Não chama para recusa; **a** (`app/ponto/services/espelho.py:339`, `app/ponto/services/espelho.py:343`). |

Outros **b/c indiretos pelo builder**, relevantes à completude do censo:

- **b** POST `ponto/celula/veredito/`, veredito `trabalhou`: view chama serviço sem captura → `_grade(reais)` → `simular_dia.grade_dia` → `grade_espelho_janela` → builder sem recusas (`app/ponto/urls.py:58`, `app/ponto/views.py:3340`, `app/ponto/services/veredito_celula.py:241`, `app/ponto/services/simular_dia.py:7`, `app/escala/utils.py:2352`). É avaliação de ação, não deve inventar julgamento tolerante.
- **b** GET `ponto/flip-fila/` e command `flip_automatico`, candidato com marcos e tipo legado desconhecido: mesma grade sem captura (`app/ponto/urls.py:63`, `app/ponto/views.py:3305`, `app/ponto/services/flip_auto.py:303`, `app/ponto/management/commands/flip_automatico.py:32`).
- **c** command `reconciliar_grade`: recusa vira `GRADE_FAIL` e pula colaborador (`app/ponto/management/commands/reconciliar_grade.py:118`, `app/ponto/management/commands/reconciliar_grade.py:119`).
- **c** grade de apresentação do espelho: recusa do builder pode virar grade vazia, mas consulta independente posterior A2 preserva rótulo; não vira faixa válida (`app/ponto/services/espelho.py:534`, `app/ponto/services/espelho.py:572`, `app/ponto/services/espelho.py:653`).
- **b** command `detectar_par_relampago` usa builder no score (`app/ponto/management/commands/detectar_par_relampago.py:136`).
- **c** eventos de feriado e regeneração: recusa é log/contador de engolidos e reconciliação pendente, não uma nova natureza (`app/chamados/signals.py:332`, `app/ponto/portas/celula.py:499`, `app/core/observ.py:85`, `app/core/observ.py:90`).
- Chamado sem célula/ata conhecida chega indiretamente à A2 pela premissa regularizada; propagação depende do chamador da ação (`app/chamados/models.py:1305`, `app/chamados/services/feriado_reavaliar.py:33`). Usos na resolução, fechamento e reabertura: `app/chamados/models.py:183`, `app/chamados/models.py:738`, `app/chamados/models.py:1349`.

Sobre **`sem_certidao` e ESMERIL**: não vi prova de campo mentindo “falta de ata” versus “tipo sem faixa”. A definição vigente é ampla: sem evidência/decisão suficiente para afirmar cumprimento/falta/irregularidade; inclui impossibilidade de julgar a faixa (`app/ponto/status_dia.py:45`). Consumos:

- Contador calendário soma recusados junto com demais dias não certificados; não soma como falta/ausência (`app/colaboradores/services/calendario.py:773`, `app/colaboradores/services/calendario.py:785`, `app/colaboradores/services/calendario.py:789`).
- Badge/rodapé diz não certificado pela tag da autoridade, tooltip do dia mostra rótulo específico (`app/templates/colaboradores/partials/_calendario_grade.html:107`, `app/templates/colaboradores/partials/_calendario_grade.html:177`, `app/templates/ponto/espelho.html:151`).
- App traduz `sem_certidao` para `neutro`; não acusa falta/irregularidade (`app/ponto/status_dia.py:84`, `app/api/views.py:1501`, `app/api/views.py:1607`). Perde nome fora da janela conforme achado.
- Auditoria de furos não cobra nem conta `sem_certidao` como furo (`app/relatorios/furos.py:128`).
- “Resolver dia” é oferecido se passado + último recurso + `sem_certidao`; o vigia do fio lê esse mesmo campo (`app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:103`, `app/chamados/services/vigia_fio.py:271`). A ação `trabalhou` pode então levantar conforme cadeia acima.
- Não encontrei leitor desse **valor transitório** no domínio financeiro ou cartório: o D4 retorna dict em memória, a escrita de status é a porta, usando `status_da_lavra`/`status_do_admin`; folha não consulta D4 nesse caminho (`app/ponto/status_dia.py:198`, `app/ponto/portas/celula.py:958`, `app/ponto/portas/celula.py:970`). Persistência do estado de produção [nao verificado].
- D4 dá prioridade a turno aberto, batida em andamento e status lavrado antes de consultar A2; logo não promete que o próprio `status_rotulo` sempre seja recusa (`app/ponto/status_dia.py:283`, `app/ponto/status_dia.py:286`, `app/ponto/status_dia.py:290`). No espelho/app a palavra separada pode preservar a recusa; o painel restrito a dias com entrada pode mostrar só o estado lavrado, não o tipo desconhecido (`app/core/services/painel_op.py:329`, `app/core/services/painel_op.py:338`).

## 12/11 08:29 · sessao 736c4747c31e · - · - · origem=Ronald (tmux)

Itens 4 e 6: sem achado específico no delta, com os limites abaixo.

**4. Calendário sem recusa preserva os argumentos e a saída.**

| Argumento | Transporte atual e equivalência |
|---|---|
| `fatos`, `houve_trabalho`, `cobertura`, `decisao`, `falta_marcos` | O dicionário conserva os valores e o compositor os entrega a `do_dia` (`app/colaboradores/services/calendario.py:713`, `app/ponto/services/espelho.py:331`, `app/ponto/services/espelho.py:343`). |
| `pendente` | A chamada antiga omitia o argumento; o atual dicionário também omite `_pendente_do_dia`. O compositor lê `None`, igual ao default de `do_dia` (`app/ponto/services/espelho.py:334`, `app/ponto/services/dia_decidido.py:399`). |
| `ata_sem_turno` | O calendário conserva a mesma chamada ao helper; este devolve inteiro positivo ou zero. O `or 0` do compositor não altera esses resultados (`app/colaboradores/services/calendario.py:717`, `app/ponto/services/dia_decidido.py:179`, `app/ponto/services/espelho.py:345`). |
| `em_aberto` | Continua sendo pertinência da mesma data ao mesmo conjunto; `_em_aberto` já é `set` antes da chamada (`app/colaboradores/services/calendario.py:563`, `app/colaboradores/services/calendario.py:584`, `app/ponto/services/espelho.py:329`, `app/ponto/services/espelho.py:343`). |
| palavra/cor/veredito | O compositor apenas copia os três valores devolvidos por `do_dia`; o calendário lê essas novas chaves (`app/ponto/services/espelho.py:347`, `app/colaboradores/services/calendario.py:723`). |

Executei em memória 216 combinações de dias válidos — trabalho/folga/feriado, presença, aberto, ata, pareamento e marcos faltantes — comparando a chamada direta informada da base com o dicionário/compositor reais atuais; todas iguais. Não existe arquivo do ensaio: [nao verificado]. A equivalência estrutural está nos pontos acima. [nao verificado]

O teste C11 do calendário cobre **recusa**, substitui `do_dia` por `Mock` e exige que ele não seja chamado; portanto não sela a paridade de dias válidos (`app/ponto/tests/test_lastro_c11_sem_banco.py:189`, `app/ponto/tests/test_lastro_c11_sem_banco.py:219`, `app/ponto/tests/test_lastro_c11_sem_banco.py:230`).

**6. Não encontrei leitor da árvore que quebre especificamente com os campos `None`.**

O serviço entrega rótulo e campos de natureza nulos e evita `_veredito_do_dia` no dia recusado (`app/ponto/services/dia_do_colab.py:94`, `app/ponto/services/dia_do_colab.py:108`). Os leitores encontrados são:

- `/api/mensageria/dia-do-colab/`: serializa o dicionário inteiro, sem operar sobre os campos nulos (`app/api/urls.py:42`, `app/api/views_mensageria.py:1970`).
- `/api/mensageria/ficha-do-colab/`: inclui o dicionário em `hoje`; `_json` recorre por listas/dicionários e devolve `None` intacto (`app/api/urls.py:27`, `app/api/views_mensageria.py:1131`, `app/colaboradores/services/ficha.py:280`, `app/colaboradores/services/ficha.py:253`).
- Mensageria: transporta o JSON completo, envolve-o em contexto e serializa o contexto; não itera `escala.ausencias` nem desreferencia `escala.tipo` (`mensageria/nucleo/core_client.py:352`, `mensageria/nucleo/ferramentas.py:817`, `mensageria/nucleo/ferramentas.py:1081`, `mensageria/nucleo/ferramentas.py:1771`, `mensageria/nucleo/ferramentas.py:1800`, `mensageria/nucleo/copiloto.py:379`).

O teste existente verifica o rótulo, `tipo is None` e ausência de chamada ao veredito, com ORM substituído; HTTP completo e comportamento de consumidores externos: [nao verificado] (`app/ponto/tests/test_lastro_c11_sem_banco.py:245`, `app/ponto/tests/test_lastro_c11_sem_banco.py:251`).

Captura ampla adicional para o censo: o calendário captura falha da consulta de dias em aberto, loga e entrega conjunto vazio com `em_aberto_perguntado=False`; alcançar esse `except` especificamente com `TipoSemFaixa` precisa confirmação da cadeia do motor/grade (`app/colaboradores/services/calendario.py:568`, `app/colaboradores/services/calendario.py:585`, `app/colaboradores/services/calendario.py:592`, `app/ponto/services/espelho.py:1174`).

## 12/11 08:28 · sessao 0c94d4952dc7 · - · - · origem=Ronald (tmux)

**Item 5 — bloqueia: recusa vira “dia explicado”.** `_fato_do_dia` devolve `erro.rotulo`; o único teste do consumidor é `fato != 'trabalho'`. Assim, a recusa entra em `explicados`/`explicado_por`, deixa de entrar em `buracos`, não participa das propostas e pode fazer o posto desaparecer do contador. Há dois significados no mesmo canal: faixa reconhecida e impossibilidade de reconhecer a faixa (`app/escala/services/semana_do_posto.py:152`, `app/escala/services/semana_do_posto.py:154`, `app/escala/services/semana_do_posto.py:179`, `app/escala/services/semana_do_posto.py:197`, `app/escala/services/semana_do_posto.py:237`).

Contraexemplo reproduzido em memória, com A2, `grade_do_posto`, `propostas_cruzadas` e `contador_por_posto` reais: um único `col17` previsto, sem batida, com ausência `fantasma_xyz` → `buracos=[]`, `explicados=[recusa]`, `propostas=[]`, `contador=[]`. A API com posto publica `dias_sem_explicacao=0`; sem posto, o contador omite esse posto (`app/api/views_mensageria.py:2083`, `app/api/views_mensageria.py:2121`, `app/api/views_mensageria.py:2123`). O teste atual exige `buracos=[]`, portanto certifica esse comportamento semântico problemático; seu mutante comprova transporte do rótulo, não distinção entre recusa e explicação válida (`app/ponto/tests/test_lastro_c11_sem_banco.py:270`, `app/ponto/tests/test_lastro_c11_sem_banco.py:271`, `app/ponto/tests/test_lastro_c11_sem_banco.py:80`).

A chave nova `explicados` **tem consumidor por transporte integral**: cliente retorna todo JSON; ferramenta devolve o bloco inteiro; montagem agrega esse bloco ao contexto; copiloto serializa todo contexto. Não encontrei leitura tipada de `explicados`, template ou JavaScript específico; a fala efetiva do modelo sobre a recusa é [nao verificado] (`mensageria/nucleo/core_client.py:308`, `mensageria/nucleo/ferramentas.py:879`, `mensageria/nucleo/ferramentas.py:884`, `mensageria/nucleo/ferramentas.py:422`, `mensageria/nucleo/copiloto.py:379`). A instrução específica continua pedindo número de dias sem explicação e pares prováveis, sem tratamento específico dessa recusa (`mensageria/nucleo/copiloto.py:280`).

Existe ainda fallback preexistente para outros erros: `Exception` vira `'trabalho'`, portanto `RuntimeError` do juiz converte um dia desconhecido em buraco. A captura específica impede isso para `TipoSemFaixa` nesta volta (`app/escala/services/semana_do_posto.py:178`, `app/escala/services/semana_do_posto.py:180`).

**Item 7 — cura consistente no universo pesquisado; sem órfão operacional localizado.** Cartório declara, incrementa e imprime `colabs_isolados_tipo_sem_faixa`; sua unidade é colaborador isolado por passada, sem filtro de ativo (`app/ponto/services/cartorio.py:1954`, `app/ponto/management/commands/processar_cartorio.py:64`, `app/ponto/management/commands/processar_cartorio.py:73`, `app/ponto/management/commands/processar_cartorio.py:75`, `app/ponto/management/commands/processar_cartorio.py:80`).

O nome antigo operacional permanece somente no vigia e no rótulo da pauta: conta códigos ativos, publica esse universo na API e na revisão semanal. O consumidor da API lê explicitamente `vigia_ausencia.contadores`, não `cartorio.contadores`; não passou a ler outro universo (`app/ponto/services/vigia_ausencia.py:160`, `app/ponto/services/vigia_ausencia.py:168`, `app/api/views_mensageria.py:1416`, `app/api/views_mensageria.py:1422`, `app/ponto/services/vigia_ausencia.py:259`, `app/pautas/semanal.py:64`).

Os contratos do cartório foram migrados; o teste novo exige ausência do alias antigo tanto no dicionário como no stdout. Os testes que preservam o nome antigo pertencem ao vigia (`app/ponto/tests/test_lastro_c10_sem_banco.py:218`, `app/ponto/tests/test_lastro_c10_sem_banco.py:220`, `app/ponto/tests/test_lastro_c11_sem_banco.py:169`, `app/ponto/tests/test_lastro_c11_sem_banco.py:172`, `app/ponto/tests/test_lastro_faixa_sem_default.py:90`, `app/ponto/tests/test_a6_vigia_familia_ausencia.py:128`).

A ocorrência documental atual do nome antigo descreve o vigia, não o contador do cartório; o cron cadastra a execução e o arquivo de saída sem analisar esse campo. Buscas globais pelos dois nomes não localizaram parser adicional em bin, placar ou crons. Leitor externo à cópia: [nao verificado] (`app/docs/RELATO.md:37`, `app/config/crons.py:616`).

## 12/11 08:22 · sessao b901d43194d7 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

Cura deixada na árvore, com censos e resposta do PDF em [LASTRO_C11.txt:6](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/LASTRO_C11.txt:6).

- 55 testes sem banco passaram; Ruff passou ([registro:243](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/LASTRO_C11.txt:243)). [nao verificado]
- RED contra c10 reproduzido; 10 mutantes falharam por asserção, com comandos registrados ([registro:209](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/LASTRO_C11.txt:209)).
- Par financeiro da competência 11: **[nao verificado]** ([limites:334](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/LASTRO_C11.txt:334)).

## 12/11 08:12 · sessao ee40839f9d72 · - · - · origem=Ronald (tmux)

Censo na base c10, somente leitura:

| Chamador direto da A2/faixa | Destino e tratamento |
|---|---|
| `ponto/status_dia.py:298`, `:304` | Leitura D4 sem `recusas=`; levanta em dia sem batida/lavra. Corrigir os dois ramos. |
| `colaboradores/services/calendario.py:507` | Tela consulta A2 sem `recusas=` e propaga (`:515`); indexa `_fatos[data]` (`:661`) e chama `_do_dia` (`:716`). Precisa transportar a recusa ao D4 (`:606`) e à palavra, preservando `KeyError` para fato ausente sem recusa. |
| `ponto/services/dia_do_colab.py:15` | Leitor da API (`api/views_mensageria.py:1970`) e ficha (`colaboradores/services/ficha.py:280`). Levanta; deve devolver rótulo da A2 sem concluir precedência. |
| `escala/services/semana_do_posto.py:177` | API chama grade (`api/views_mensageria.py:2113`) e propostas (`:2114`). Relança em `semana_do_posto.py:178`; precisa resposta nomeada sem afirmar faixa válida. |
| `ponto/services/espelho.py:653` | Já consulta com `recusas=` (`:654`); palavra recebe `erro.rotulo` (`:880`, `:341`) sem chamar `do_dia` no recusado. |
| `ferias/situacao.py:95` | Captura específica (`:96`) e devolve aviso nomeado sem concluir descanso (`:98`). |
| `ponto/services/vigia_ausencia.py:162` | Captura específica (`:163`) e conta códigos ativos (`:160`, `:168`); manter. |
| `escala/utils.py:1209` | Builder compartilhado; julgamento/grade que alimenta dinheiro. Manter recusa, sem inventar fato válido. |
| `ponto/status_dia.py:181` | Insumo da lavra, chamado pelo cartório (`cartorio.py:932`, `:1292`, `:1853`); manter recusa. |
| `ponto/services/cartorio.py:1998` | Escrita de precedência na célula (`:1999`); manter recusa. |
| `chamados/services/feriado_reavaliar.py:33` | Julga premissa antes de reconciliar (`:43`, `:94`); manter recusa. |
| `ponto/management/commands/detectar_ausencias.py:616` | Julgamento do emissor; manter recusa. |
| `ponto/services/ausencia.py:1310` | Porta converte em `AusenciaInvalida` (`:1312`); corrigir brecha de `campos` conforme pedido. |
| `ponto/precedencia.py:544`, `:565` | Veto operacional; manter recusa. Chamadores: `ausencia.py:380`, `declarar_dia.py:120`, `escala/folgas.py:310`, `:336`, `ferias/views.py:254`. |
| `folha/services/sem_furo.py:25` | Apuração de comando (`folha/management/commands/colabs_sem_furo.py:13`); nova semântica numérica da recusa não autorizada. |
| `ponto/services/faltas_de_hoje.py:61` | Apuração de comando (`ponto/management/commands/faltas_de_hoje_sem_cobranca.py:16`); não é chamada de request identificada. |
| `ponto/services/deslogados.py:166` | Apuração/lavra (`ponto/management/commands/lavrar_deslogados.py:38`, `ponto/services/pautas_dos_deslogados.py:62`). |
| `ponto/services/abriu_nao_bateu.py:123` | Apuração/lavra (`ponto/management/commands/lavrar_abriu_nao_bateu.py:30`); API lê snapshot (`api/views_mensageria.py:139`), não executa A2 no request. |

D4 chega a leitores por `ponto/services/espelho.py:170`, `api/views.py:1483`, `colaboradores/services/calendario.py:606`, `relatorios/furos.py:123` e `core/services/painel_op.py:336`. A forma unitária delega ao lote (`ponto/status_dia.py:218`).

Fechamento continua propagando nas três capturas (`ponto/services/fechamento.py:1081`, `:1089`, `:1308`); TXT chama a leitura antes de gerar (`folha/views.py:208`), e simulação chama fechamento (`folha/management/commands/simular_folha.py:123`, `:130`).

`dia_decidido.do_dia` tem dois chamadores operacionais: espelho (`ponto/services/espelho.py:343`) e calendário (`colaboradores/services/calendario.py:716`). Os `do_dia` de calculador, pergunta no app e vigia são homônimos distintos (`ponto/calculador/regras.py:576`, `chamados/services/pergunta_no_app.py:211`, `ponto/services/vigia_entrada.py:111`).

Execução HTTP/ORM e ocorrência produtiva: [nao verificado].

## 12/11 08:05 · sessao 553a6d49936c · FIO-E0-V4-E4B · construtor · origem=Code

Cura aplicada. A [entrega completa](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c14_entrega.txt:7) contém as 69 classificações R/C, escritores/leitores e comandos individuais. [nao verificado]

- GREEN: [158 testes de memória](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c14_memoria.txt:59) e [47 contratos estáticos](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c14_selos.txt:6). [nao verificado]
- [21 mutantes c14](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c14_entrega.txt:123) morderam por assertion, sem ERROR.

Suíte inteira, ORM e medição SQL do C8: **[nao verificado]**; Django está ausente e banco foi vedado. Sem commit, conforme a proibição explícita das REGRAS. [Pendências para o Code](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c14_entrega.txt:194). [nao verificado]

## 12/11 08:02 · sessao f2aeed7d1ba0 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**REPROVA c10:** o rótulo da recusa não garante que a tela renderize: uma consulta posterior volta a levantar `TipoSemFaixa` (`app/ponto/views.py:184`, `app/ponto/services/espelho.py:170`, `app/ponto/status_dia.py:304`). O par financeiro da competência 11 exigido pelo aval não está demonstrado no dado fornecido [nao verificado].

1. **PORTA e escritores**

   A criação, inclusive inativa, e a ativação explícita consultam `faixa_de` antes da escrita; a recusa usa `AusenciaInvalida(codigo='tipo_sem_faixa')` (`app/ponto/services/ausencia.py:1308`, `app/ponto/services/ausencia.py:1312`).

   Não encontrei segundo escritor operacional de `TipoAusencia` nas views, commands ou importadores pesquisados. O admin Django não registra esse modelo; os dois ramos da view passam pela porta. A migration 0052 escreve diretamente código/efeito/ativo, mas suas sementes têm faixa; 0062/0063 alteram flags de tipos existentes (`app/ponto/admin.py:11`, `app/ponto/admin.py:37`, `app/ponto/views_tipos_ausencia.py:87`, `app/ponto/views_tipos_ausencia.py:98`, `app/ponto/migrations/0052_seed_tipoausencia.py:33`, `app/ponto/precedencia.py:52`, `app/ponto/migrations/0062_fim_dos_17_tipos.py:51`, `app/ponto/migrations/0063_atestado_aceita_horas.py:20`).

   **Achado — fila, brecha da própria porta.** Ela valida `tipo.codigo` antes de aplicar indiscriminadamente `campos`. Contraexemplo reproduzido em memória: um objeto `abono` aceita `campos={'codigo':'fantasma_xyz','ativo':True}` e chama `save()` com o código desconhecido ativo. A view atual não fornece `codigo` nesse dicionário; exploração por essa tela não foi demonstrada (`app/ponto/services/ausencia.py:1310`, `app/ponto/services/ausencia.py:1314`, `app/ponto/views_tipos_ausencia.py:67`).

   A view não lê `.codigo`: entrega `str(erro)` ao feedback. Para `fantasma_xyz`, o texto é **“Tipo sem faixa de precedencia: fantasma_xyz”**; o JavaScript escreve na caixa de erro e no toast (`app/ponto/precedencia.py:100`, `app/ponto/views_tipos_ausencia.py:89`, `app/ponto/views_tipos_ausencia.py:100`, `app/core/respostas.py:65`, `app/templates/ponto/tipos_ausencia.html:73`, `app/templates/ponto/tipos_ausencia.html:86`).

   Os leitores gerais de `AusenciaInvalida.codigo` encontrados são API, formulário de ausência e comando de rederivação; eles não são os chamadores do cadastro de tipos. O novo valor tem assert específico no ensaio c10 (`app/api/views.py:1819`, `app/ponto/views.py:2806`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:172`, `app/ponto/tests/test_lastro_c10_sem_banco.py:86`).

2. **Cartório: isolamento, persistência e contadores**

   O `except` envolve **`julgar_colab` inteiro**, não cada dia nem a empresa inteira. Captura somente `TipoSemFaixa`, incrementa uma vez por colaborador recusado, loga pk/código e continua (`app/ponto/management/commands/processar_cartorio.py:64`, `app/ponto/management/commands/processar_cartorio.py:70`, `app/ponto/management/commands/processar_cartorio.py:72`).

   No caminho normal identificado, a grade do período consulta a A2 antes da primeira lavra. Um tipo desconhecido interrompe essa preparação: nenhuma ata nova daquele colaborador é escrita por esse caminho; as antigas permanecem (`app/ponto/services/cartorio.py:1168`, `app/escala/utils.py:1209`, `app/ponto/precedencia.py:419`, `app/ponto/services/cartorio.py:1288`).

   Isso **não constitui rollback por colaborador**. Se a exceção surgir depois de uma gravação, ela pode permanecer: o `finally` limpa contexto/filas, sem desfazer persistência. Há `atomic()` por salvamento de célula/evento, não envolvendo o colaborador inteiro (`app/ponto/services/cartorio.py:1093`, `app/ponto/services/cartorio.py:1116`, `app/ponto/portas/celula.py:899`, `app/ponto/portas/celula.py:904`). Nesse eventual estado parcial, as atas já salvas continuam disponíveis ao leitor da célula (`app/escala/services/leitor_celula.py:418`).

   Não há fundamento para atribuir conexão SQL quebrada à recusa normal: `TipoSemFaixa` é `ValueError` levantado pelo mapa. Continuidade com ORM e verificação de `TransactionManagementError` não foram executadas [nao verificado] (`app/ponto/precedencia.py:96`, `app/ponto/precedencia.py:108`).

   **Achado — fila, campo com dois significados.** `tipos_ativos_sem_faixa` conta colaboradores recusados no cartório e códigos ativos no vigia; o cartório sequer filtra `ativo`. Contraexemplos: um código em três colaboradores pode produzir **3/1**; um código inativo com ausência pode produzir **1/0** (`app/ponto/management/commands/processar_cartorio.py:73`, `app/ponto/services/cartorio.py:1952`, `app/ponto/services/vigia_ausencia.py:160`, `app/ponto/services/vigia_ausencia.py:168`, `app/ponto/precedencia.py:419`).

   Ambos chegam à mesma função real `faixa_de`: diretamente no vigia e pela grade/A2 no cartório. Portanto, são universos diferentes, sem evidência de segundo juiz de faixa (`app/ponto/services/vigia_ausencia.py:162`, `app/escala/utils.py:1209`, `app/ponto/precedencia.py:419`). O cartório publica seu contador no stdout; API/revisão semanal leem o contador do vigia (`app/ponto/management/commands/processar_cartorio.py:80`, `app/api/views_mensageria.py:1422`, `app/ponto/services/vigia_ausencia.py:259`).

3. **Espelho/PDF**

   A A2 recebe `recusas=`, exclui o dia recusado dos fatos e não ordena sua precedência; o compositor publica o rótulo pronto sem chamar `do_dia` para ele. O PDF reutiliza esse compositor (`app/ponto/precedencia.py:420`, `app/ponto/precedencia.py:425`, `app/ponto/services/espelho.py:339`, `app/relatorios/pdf_espelho.py:597`).

   **Achado — bloqueia, recusa tratada no miolo e relançada na tela.** Após obter o espelho, a view chama `montar_semanas`; este consulta `status_dos_dias_lavrados` sem fornecer fatos/recusas. Quando chega a um dia sem batida e sem status lavrado, consulta novamente a A2, agora sem `recusas=` (`app/ponto/views.py:158`, `app/ponto/views.py:184`, `app/ponto/services/espelho.py:170`, `app/ponto/status_dia.py:287`, `app/ponto/status_dia.py:296`, `app/ponto/status_dia.py:304`).

   **Contraexemplo:** ausência aprovada `fantasma_xyz` em dia passado sem batida e sem lavra. A primeira passagem produz “Tipo sem faixa: fantasma_xyz”; `montar_semanas` levanta novamente antes do `render`. Reproduzi essa sequência em memória com A2, D4 e `montar_semanas` reais; a execução HTTP completa permanece [nao verificado] (`app/ponto/precedencia.py:421`, `app/ponto/views.py:193`).

   **Achado — fila, paridade do PDF para dias válidos não demonstrada.** A troca de órfãs do builder por `orfas_grade` troca efetivamente a fonte: a tela normalmente lê a ata. Sem célula, esse leitor entrega órfãs vazias; o builder conserva batidas não consumidas como órfãs (`app/relatorios/pdf_espelho.py:477`, `app/ponto/services/espelho.py:522`, `app/escala/services/leitor_celula.py:394`, `app/escala/services/leitor_celula.py:420`, `app/escala/utils.py:1849`).

   **Contraexemplo de código:** builder com órfã `07:02`, ata/tela sem ela, período aberto iniciado às `07:02`. A substituição altera sua inclusão em `turnos_abertos`, depois `datas_turno_aberto`, podendo alterar dias em aberto e palavra. Nenhum tipo sem faixa é necessário; ocorrência em produção e magnitude são [nao verificado] (`app/relatorios/pdf_espelho.py:178`, `app/relatorios/pdf_espelho.py:515`, `app/relatorios/pdf_espelho.py:576`, `app/relatorios/cartao_pela_celula.py:418`, `app/relatorios/pdf_espelho.py:597`).

   Órfãs e palavra passam pela autoridade da tela, mas o PDF ainda apura/soma seu resumo e calcula saldo antes de `folha_manda`; essas derivações já existiam no recorte da base (`app/relatorios/pdf_espelho.py:418`, `app/relatorios/pdf_espelho.py:504`, `app/relatorios/pdf_espelho.py:531`, `app/ponto/tests/lastro_c9_recortes.json:27`). Paridade integral de horas/saldo/totais com a base: [nao verificado].

   Sem achado específico na remoção de `_realizado`: era helper local sem chamada no recorte fornecido da base; sua remoção isolada não explica mudança de saída (`app/ponto/tests/lastro_c9_recortes.json:27`).

4. **Fechamento e competência**

   As três capturas alteradas relançam `TipoSemFaixa`. Se ela ocorrer, interrompe o restante da chamada — empresa inteira ou subconjunto escolhido —, sem representar uma tranca formal da competência (`app/ponto/services/fechamento.py:555`, `app/ponto/services/fechamento.py:558`, `app/ponto/services/fechamento.py:1081`, `app/ponto/services/fechamento.py:1089`, `app/ponto/services/fechamento.py:1308`).

   Não há atomicidade de lote: A pode estar salvo, B pode ganhar linha `aberto` antes da falha, e C não ser processado. Isso é consequência de propagação, não uma decisão nova de pagar/abonar/zerar B (`app/ponto/services/fechamento.py:899`, `app/ponto/services/fechamento.py:1015`, `app/ponto/services/fechamento.py:1149`, `app/ponto/services/fechamento.py:1308`).

   Os chamadores relevantes têm estes destinos:

   - View e command de recalcular deixam a recusa subir (`app/ponto/views.py:1252`, `app/ponto/management/commands/recalcular_fechamento.py:118`).
   - Exportação chama leitura da empresa antes de classificar/gerar TXT; uma recusa interrompe esse fluxo (`app/folha/views.py:208`, `app/folha/views.py:212`).
   - `simular_folha` chama o fechamento antes do seu `try` posterior (`app/folha/management/commands/simular_folha.py:123`, `app/folha/management/commands/simular_folha.py:130`, `app/folha/management/commands/simular_folha.py:132`).
   - O callback por evento captura `Exception` e loga após o fato gravado: nesse chamador a recusa não chega ao usuário (`app/ponto/services/fechamento.py:1817`, `app/ponto/services/fechamento.py:1825`).
   - O cadastro declara ausência de cron ordinário de recalcular (`app/config/crons.py:1130`).

   Deixar levantar corresponde à instrução D do pedido; o isolamento por colaborador foi exigido especificamente para o cartório. A condição financeira de pouso, item 6, continua sem par medido no dado fornecido [nao verificado]. [nao verificado]

5. **Fonte única e defaults**

   Sem achado de default de **faixa** para tipo desconhecido: o mapa é consultado por indexação em `faixa_de`; A2/veto passam por essa função. O fallback `'trabalho'` da semana do posto relança expressamente `TipoSemFaixa` (`app/ponto/precedencia.py:108`, `app/ponto/precedencia.py:110`, `app/ponto/precedencia.py:419`, `app/ponto/precedencia.py:544`, `app/escala/services/semana_do_posto.py:178`).

   **Achado — fila, fallback de efeito preexistente.** `_do_banco` captura qualquer exceção e `vigente` substitui falha/resultado vazio pelo catálogo congelado; `efeito_vigente` lê esse resultado. Contraexemplo: `abono` alterado para descontar, seguido de falha na leitura, pode voltar a responder `ABONA`. Isso é fallback de efeito, não de faixa (`app/ponto/catalogo/ausencias.py:102`, `app/ponto/catalogo/ausencias.py:254`, `app/ponto/catalogo/ausencias.py:283`, `app/ponto/catalogo/ausencias.py:298`).

6. **Mordida dos testes** [nao verificado]

   Executei os três harnesses juntos: **43 testes, OK, RC 0**; seus mecanismos executam AST do fonte com alimentação substituída (`app/ponto/tests/test_lastro_c5_sem_banco.py:108`, `app/ponto/tests/test_lastro_c10_sem_banco.py:75`, `app/ponto/tests/test_lastro_c7_sem_banco.py:70`).

   Os mutantes executados ficaram vermelhos por asserção: retirar a guarda da porta, relançar no isolamento do cartório, restaurar o default `'atestado'` e retirar o rótulo. Portanto, há mordida de comportamento, além de inspeção textual (`app/ponto/tests/test_lastro_c10_sem_banco.py:31`, `app/ponto/tests/test_lastro_c10_sem_banco.py:36`, `app/ponto/tests/test_lastro_c10_sem_banco.py:43`, `app/ponto/tests/test_lastro_c10_sem_banco.py:280`, `app/ponto/tests/test_lastro_c5_sem_banco.py:50`).

   **Limite que bloqueia a conclusão de integração:** o teste B substitui `julgar_colab`; o teste PDF executa somente um prefixo com espelho mockado; o teste D injeta a exceção nas capturas, sem motor/ORM. Eles não certificam persistência, PDF completo nem o caminho posterior de `montar_semanas` que falha (`app/ponto/tests/test_lastro_c10_sem_banco.py:180`, `app/ponto/tests/test_lastro_c10_sem_banco.py:311`, `app/ponto/tests/test_lastro_c10_sem_banco.py:319`, `app/ponto/tests/test_lastro_c10_sem_banco.py:349`).

   O `test_05` cria `reciclagem` pela porta, aprova ausência e confere RD; os testes 07/08 são o par de criação/ativação recusadas. O código escolhido tem faixa e não está na semente 0052 (`app/relatorios/tests/test_rotulo_ausencia_pdf.py:52`, `app/relatorios/tests/test_rotulo_ausencia_pdf.py:64`, `app/relatorios/tests/test_rotulo_ausencia_pdf.py:73`, `app/ponto/precedencia.py:71`, `app/ponto/migrations/0052_seed_tipoausencia.py:10`). Execução ORM desses testes: [nao verificado].

   Os RED B/C também falharam com `--red-c9`, mas esse mecanismo substitui recortes congelados; não executa um checkout independente da c9 (`app/ponto/tests/test_lastro_c10_sem_banco.py:22`, `app/ponto/tests/test_lastro_c10_sem_banco.py:28`).

7. **Classes de reprovação**

   Os achados concretos são: **integração da recusa incompleta — bloqueia**; **contador com duas unidades, brecha interna da porta e fallback de efeito — fila**. Não encontrei novo juiz de faixa nem segundo escritor operacional de tipos; a consulta duplicada na tela usa a própria A2, mas perde o tratamento de recusa (`app/ponto/status_dia.py:304`, `app/ponto/management/commands/processar_cartorio.py:73`, `app/ponto/services/vigia_ausencia.py:168`, `app/ponto/services/ausencia.py:1314`, `app/ponto/catalogo/ausencias.py:283`).

   Sem achado de relógio novo no delta do PDF: `_hj` deriva do `agora` recebido. Os `except` que ainda absorvem falhas foram identificados no catálogo e no callback financeiro; as três capturas c10 do fechamento propagam a exceção (`app/relatorios/pdf_espelho.py:475`, `app/ponto/catalogo/ausencias.py:254`, `app/ponto/services/fechamento.py:1825`, `app/ponto/services/fechamento.py:1089`).

## 12/11 07:59 · sessao c2fbd7863c76 · - · - · origem=Ronald (tmux)

1. **Bloqueia — tela admin ainda pode levantar `TipoSemFaixa` antes de renderizar.** A view obtém o espelho com a recusa rotulada (`app/ponto/views.py:158`), mas depois chama `montar_semanas` (`app/ponto/views.py:184`). Esta chama `status_dos_dias_lavrados` sem fornecer fatos ou recusas (`app/ponto/services/espelho.py:170`). Ao alcançar dia sem batida e sem status lavrado, o leitor pergunta novamente à A2 pela janela inteira, sem `recusas=` (`app/ponto/status_dia.py:296`, `app/ponto/status_dia.py:304`). A A2 relança nessa modalidade (`app/ponto/precedencia.py:421`). Contraexemplo: ausência aprovada `legado_rd` em dia passado sem batida e célula sem `status_dia`; a palavra pode estar pronta no miolo, mas a view falha antes do `render` (`app/ponto/views.py:193`). O PDF não chama `montar_semanas`; chama somente o miolo (`app/relatorios/pdf_espelho.py:476`).

2. **Fila — paridade PDF sem tipo inválido não está provada; há mudança concreta de fonte.** Agora órfãs vêm da tela (`app/relatorios/pdf_espelho.py:477`), que normalmente lê a ATA (`app/ponto/services/espelho.py:522`, `app/escala/services/leitor_celula.py:420`). Antes vinham diretamente do builder, conforme o diff fornecido `[nao verificado: checkout independente da base]`. Essas listas não têm contrato de igualdade: sem célula, o leitor entrega dia sem órfãs (`app/escala/services/leitor_celula.py:390`, `app/escala/services/leitor_celula.py:394`); o builder conserva batidas não consumidas como órfãs (`app/escala/utils.py:1832`, `app/escala/utils.py:1849`).

   Contraexemplo condicional: builder classifica `07:02` como órfã e a ATA lida pela tela não; um período aberto iniciado às `07:02` passa a entrar em `_abertos_ts` (`app/relatorios/pdf_espelho.py:515`), muda `datas_turno_aberto` (`app/relatorios/pdf_espelho.py:576`) e pode mudar `dias_em_aberto`, seu badge e a palavra da linha (`app/relatorios/cartao_pela_celula.py:418`, `app/relatorios/pdf_espelho.py:750`, `app/relatorios/pdf_espelho.py:597`). Nenhum tipo sem faixa é necessário. Existência desse estado em produção e delta de horas/saldo/dinheiro: `[nao verificado]`.

3. **O selo de PDF não mede essa paridade.** Executa somente o prefixo até `_orfas_hhmm` (`app/ponto/tests/test_lastro_c10_sem_banco.py:302`, `app/ponto/tests/test_lastro_c10_sem_banco.py:319`), com `espelho_do_colab` substituído por `Mock` (`app/ponto/tests/test_lastro_c10_sem_banco.py:311`). Não executa os consumidores de órfãs, o resumo final nem a renderização; portanto o verde desse teste não certifica os efeitos descritos acima (`app/ponto/tests/test_lastro_c10_sem_banco.py:324`).

4. **Fonte única melhorou para órfãs e palavra, mas o PDF ainda calcula seu próprio resumo.** Lê os dias do espelho (`app/relatorios/pdf_espelho.py:561`), reutiliza a mesma porta de composição da palavra (`app/relatorios/pdf_espelho.py:597`) e apenas imprime `palavra_dia` (`app/relatorios/pdf_espelho.py:882`). Ainda executa `apurar_por_fatia` (`app/relatorios/pdf_espelho.py:418`), soma períodos (`app/relatorios/pdf_espelho.py:504`) e calcula saldo (`app/relatorios/pdf_espelho.py:531`) antes de `folha_manda` (`app/relatorios/pdf_espelho.py:587`). Esses caminhos já aparecem na base congelada (`app/ponto/tests/lastro_c9_recortes.json:27`); não são novos no delta.

5. **Sem achado na remoção de `_realizado`.** Era função local sem chamadas no recorte da base congelada (`app/ponto/tests/lastro_c9_recortes.json:27`); o PDF atual recebe minutos e pago pelo dict da tela (`app/ponto/services/espelho.py:792`, `app/ponto/services/espelho.py:820`). A remoção do helper, isoladamente, não muda saída; fidelidade do JSON ao commit real `[nao verificado]`.

6. **Sem relógio novo nesse delta.** `_hj` deriva do `agora` recebido e a chamada continua terminando em ontem (`app/relatorios/pdf_espelho.py:475`, `app/relatorios/pdf_espelho.py:476`). A mesma chamada foi apenas antecipada segundo o diff fornecido `[nao verificado: checkout independente da base]`.

## 12/11 07:59 · sessao 0214359824a2 · - · - · origem=Ronald (tmux)

2. O isolamento envolve **o colaborador inteiro**, na chamada única de `julgar_colab`; captura exclusivamente `TipoSemFaixa`, incrementa uma vez e registra pk/código antes de continuar (`app/ponto/management/commands/processar_cartorio.py:64`, `:70`, `:72`–`:80`).

No caminho normal desse erro, a grade inteira é montada antes de qualquer lavra (`app/ponto/services/cartorio.py:1168`); ela chama A2 (`app/escala/utils.py:1209`), que levanta em `faixa_de` (`app/ponto/precedencia.py:419`–`:422`). Portanto, nesse caminho, **nenhuma ata nova daquele colaborador é gravada**, e as anteriores permanecem; a primeira lavra/carimbo vem depois (`app/ponto/services/cartorio.py:1288`, `:1310`). Não existe rollback de colaborador: se a exceção ocorrer numa fase posterior, escritas anteriores podem permanecer; o `finally` limpa filas e contexto, mas não desfaz persistência (`app/ponto/services/cartorio.py:1093`–`:1118`).

Não há `atomic()` envolvendo o comando nem `julgar_colab`; existe atomicidade menor na porta que salva célula e evento (`app/ponto/portas/celula.py:899`–`:907`). `TipoSemFaixa` é `ValueError` puro (`app/ponto/precedencia.py:96`), levantado pela consulta ao mapa (`:105`–`:110`): esse caminho não evidencia SQL abortado ou `TransactionManagementError`. Ensaio ORM real da continuidade: **[nao verificado]**.

Os contadores são diferentes: cartório mede **colaboradores recusados na passada** (`app/ponto/services/cartorio.py:1952`–`:1954`; comando `:73`), vigia mede **códigos ativos sem faixa** (`app/ponto/services/vigia_ausencia.py:159`–`:168`). Ambos chegam à autoridade `faixa_de`: diretamente no vigia (`:162`), por grade/A2 no cartório (`app/escala/utils.py:1209`; `app/ponto/precedencia.py:419`). Leitores: resumo stdout do comando (`:75`–`:80`); API do vigia (`app/api/views_mensageria.py:1416`–`:1424`) e revisão semanal (`app/ponto/services/vigia_ausencia.py:259`–`:262`). Consumo automático do stdout em produção: **[nao verificado]**.

**Achado, fila — campo com dois significados:** `tipos_ativos_sem_faixa` designa unidades diferentes e o cartório nem exige que o cadastro esteja ativo. Contraexemplo: um código X usado por três colaboradores produz cartório=3 e vigia=1; X inativo, mas com ausência aprovada, pode produzir cartório=1 e vigia=0, porque a A2 consulta o código sem consultar `.ativo` (`app/ponto/management/commands/processar_cartorio.py:73`; `app/ponto/services/vigia_ausencia.py:160`; `app/ponto/precedencia.py:419`). Isso não introduz segundo juiz de faixa, mas cria ambiguidade semântica no nome compartilhado.

4. A recusa sobe pelas três capturas alteradas (`app/ponto/services/fechamento.py:1081`, `:1089`, `:1308`), interrompendo **todo o restante do universo daquela chamada**, que pode ser empresa inteira ou subconjunto (`:555`–`:558`, `:620`). Não é uma tranca de competência: são interrompidos o cálculo e o retorno, sem alteração específica de status por essa recusa (`:1308`–`:1318`).

**Persistência parcial concreta:** não existe atomicidade de lote. A pode ser salvo (`app/ponto/services/fechamento.py:1149`), B pode ganhar uma linha `status='aberto'` pelo `get_or_create` (`:899`–`:902`) e só depois falhar ao montar sua grade (`:1015`); C não será processado (`:1308`–`:1309`). Sem fotografia anterior para B, pode sobrar linha com defaults; com fotografia anterior, ela pode permanecer velha. Não houve regra de dinheiro nova para resolver B (`:1089`–`:1104`).

Chamadores relevantes:

- Botão de recalcular: chamada sem captura da recusa; deixa de retornar sucesso (`app/ponto/views.py:1252`–`:1253`). Resposta HTTP efetivamente observada: **[nao verificado]**.
- Gerar TXT: recalcula empresa inteira em leitura antes de classificar/aprovar; a recusa impede chegar à geração e ao carimbo (`app/folha/views.py:207`–`:212`, `:250`–`:252`).
- Comando `recalcular_fechamento --apply`: propaga antes de fotografar o depois (`app/ponto/management/commands/recalcular_fechamento.py:118`–`:122`).
- `simular_folha`, modos recalcular/lido: chamadas fora do `try` posterior; propaga (`app/folha/management/commands/simular_folha.py:121`–`:135`).
- Evento de batida/célula/HE: callback **captura qualquer Exception e loga**, preservando o fato já gravado; a recusa não sobe ao usuário por esse chamador (`app/ponto/services/fechamento.py:1814`–`:1828`).
- Regeneração em exportada: chama dentro de `transaction.atomic()`, portanto a recusa propagada desfaz o ato envolvente (`app/ponto/portas/regen_exportada.py:169`–`:180`).

Não encontrei cron ordinário de `recalcular_fechamento`; o cadastro de crons declara explicitamente que não há cron dessa operação (`app/config/crons.py:1130`–`:1137`). O comentário do callback dizendo que “o cron rega” não demonstra um agendamento real (`app/ponto/services/fechamento.py:1827`).

A propagação financeira corresponde à instrução D do pedido fornecido — deixar levantar quando seria necessário inventar regra. O item 4 do aval exige isolamento especificamente no **cartório**, não no fechamento; o item 6 exige par medido da competência 11. O dado fornecido não contém esse par: **[nao verificado]**.

## 12/11 07:59 · sessao 7bde65f716ea · - · - · origem=Ronald (tmux)

Itens 1 e 5, com achados separados de limites:

- **Fila — brecha na própria porta, anterior à c10.** `gravar_tipo_ausencia` valida o código atual e depois aplica qualquer chave de `campos` por `setattr`. Contraexemplo: `gravar_tipo_ausencia(tipo_com_codigo_abono, campos={'codigo': 'fantasma_xyz', 'ativo': True})` valida `abono` e salva `fantasma_xyz` ativo, sem faixa. A view atual não fornece `codigo` em `campos`, portanto não demonstrei exploração pelo HTTP. Evidências: `app/ponto/services/ausencia.py:1310`, `app/ponto/services/ausencia.py:1314`, `app/ponto/views_tipos_ausencia.py:67`.

- **Não encontrei segundo escritor runtime de `TipoAusencia` fora da porta.** O admin Django registra outros cinco modelos, sem `TipoAusencia`; a view de cadastro chama a porta nos dois ramos. A migration 0052 escreve diretamente código, efeito e ativo, mas seus 17 códigos estão todos no mapa. As migrations 0062 e 0063 alteram flags de tipos existentes, sem criar/ativar código novo. Evidências: `app/ponto/admin.py:11`, `app/ponto/admin.py:37`, `app/ponto/views_tipos_ausencia.py:87`, `app/ponto/views_tipos_ausencia.py:98`, `app/ponto/migrations/0052_seed_tipoausencia.py:10`, `app/ponto/migrations/0052_seed_tipoausencia.py:33`, `app/ponto/precedencia.py:52`, `app/ponto/migrations/0062_fim_dos_17_tipos.py:51`, `app/ponto/migrations/0063_atestado_aceita_horas.py:20`. Censo literal global não encontrou comando/importador/seed adicional que escreva esse modelo; caminhos dinâmicos externos à cópia: [nao verificado].

- **A recusa chega nomeada à tela.** A porta levanta `AusenciaInvalida(..., codigo='tipo_sem_faixa')`; a view lê somente `str(erro)`, não `.codigo`. `resposta_acao` envia `showToast` com tipo erro; o JavaScript escreve o texto na caixa `ta-erro` e no toast. Contraexemplo esperado: código `fantasma_xyz` exibe `Tipo sem faixa de precedencia: fantasma_xyz`. Evidências: `app/ponto/services/ausencia.py:1312`, `app/ponto/views_tipos_ausencia.py:89`, `app/ponto/views_tipos_ausencia.py:100`, `app/core/respostas.py:51`, `app/core/respostas.py:65`, `app/templates/ponto/tipos_ausencia.html:72`, `app/templates/ponto/tipos_ausencia.html:83`, `app/templates/ponto/tipos_ausencia.html:86`.

- **Leitores de `AusenciaInvalida.codigo`.** A API distingue `sobreposicao` → 409, demais códigos → 400; edição de ausência escolhe campo do formulário pelo código; um comando de rederivação registra o código. Esses consumidores não chamam `gravar_tipo_ausencia`; o novo valor `tipo_sem_faixa` tem assert específico no ensaio c10. Evidências: `app/api/views.py:1819`, `app/api/views.py:1823`, `app/ponto/views.py:2806`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:172`, `app/ponto/tests/test_lastro_c10_sem_banco.py:86`.

- **Fonte da faixa preservada.** `_FAIXA_DE_TIPO` só é acessado em `faixa_de`, que faz indexação e levanta `TipoSemFaixa` quando falta a chave. A2 e veto consultam essa função; nenhum default executável `'atestado'` substitui tipo desconhecido. `ROTULOS.get` é fallback de texto depois da classificação; `ICONE_PADRAO='atestado'` é desenho. Evidências: `app/ponto/precedencia.py:52`, `app/ponto/precedencia.py:108`, `app/ponto/precedencia.py:110`, `app/ponto/precedencia.py:419`, `app/ponto/precedencia.py:544`, `app/ponto/precedencia.py:565`, `app/ponto/catalogo/ausencias.py:119`, `app/ponto/catalogo/ausencias.py:181`.

- **Fila — fallback de efeito/cadastro sobrevive, anterior à c10.** `_do_banco` engole qualquer exceção e `vigente` troca resultado vazio/falha pelo `CATALOGO` congelado; `efeito_vigente` lê isso. Contraexemplo: `abono` configurado como `desconta`, seguido de falha de leitura do cadastro, pode voltar a responder `ABONA` do congelado. É fallback de efeito, não de faixa, e não foi introduzido pela c10. Evidências: `app/ponto/catalogo/ausencias.py:254`, `app/ponto/catalogo/ausencias.py:283`, `app/ponto/catalogo/ausencias.py:298`, `app/ponto/catalogo/ausencias.py:102`.

- **Outro fallback encontrado, sem engolir `TipoSemFaixa`.** Semana do posto usa `'trabalho'` quando não recebe faixa ou ocorre outra exceção, mas relança expressamente `TipoSemFaixa`. Portanto não demonstra sobrevivência do default para tipo sem faixa. Evidências: `app/escala/services/semana_do_posto.py:177`, `app/escala/services/semana_do_posto.py:178`, `app/escala/services/semana_do_posto.py:180`.

- **ESMERIL-SEMANTICA: mesmo nome, universos distintos.** O vigia conta códigos ativos de cadastro sem faixa; cartório conta colaboradores recusados na passada. Contraexemplo estático: um código ativo usado por três colaboradores pode resultar em vigia=1/cartório=3; código ativo sem lançamento pode resultar em vigia=1/cartório=0. Ambos consultam a mesma autoridade, portanto isso não prova segundo juiz. Evidências: `app/ponto/services/vigia_ausencia.py:155`, `app/ponto/services/vigia_ausencia.py:162`, `app/ponto/services/vigia_ausencia.py:168`, `app/ponto/services/cartorio.py:1952`, `app/ponto/management/commands/processar_cartorio.py:73`.

`TipoSemFaixa.codigo` significa identificador do tipo; `AusenciaInvalida.codigo` significa categoria da recusa. São classes distintas, e seus leitores encontrados respeitam esses sentidos. Evidências: `app/ponto/precedencia.py:101`, `app/ponto/services/ausencia.py:63`, `app/ponto/management/commands/processar_cartorio.py:74`, `app/api/views.py:1819`.

## 12/11 07:57 · sessao bbca41c9563d · BO-ROTULO-REABRIR-SO-NO-DETALHE · leitura · origem=Code

A diferença está confirmada no código: os dois detalhes incluem `Reabrir`; fio, modal e painel do turno omitem esse destino. Há também uma diferença no processamento: `detalhe.html` ignora o status enviado; `detalhe_local.html` chega ao motor. (`app/templates/chamados/detalhe.html:135`; `app/templates/chamados/detalhe_local.html:398`; `app/templates/chamados/fio_colaborador.html:268–270`; `app/templates/chamados/partials/modal_fio.html:668–670`; `app/templates/chamados/partials/painel_chamado_turno.html:76–78`; `app/chamados/views.py:292–310`; `app/chamados/services/acoes_chamado.py:313–321`.)

**1. Oferta por tela e responsável pela lista**

| Tela | Oferta de `novo_status=aberto` / “Reabrir” |
|---|---|
| `detalhe.html` | **Sim**, para `is_gestor`, quando `status_local != fechado`. Não filtra os outros estados. (`app/templates/chamados/detalhe.html:117`, `:127–135`.) |
| `detalhe_local.html` | **Sim**, para `is_gestor` com `pode_agir`, quando `status_local != fechado`. (`app/templates/chamados/detalhe_local.html:375–398`.) |
| `fio_colaborador.html` | **Não**. Oferece somente `em_analise`, `resolvido`, `fechado`; formulário condicionado a chamado não fechado, sem GLPI e com `pode_agir_map`. (`app/templates/chamados/fio_colaborador.html:254–270`.) |
| `modal_fio.html` | **Não**. Mesmos três destinos; exige chamado não fechado, sem GLPI, fora de `modo_beneficio` e com `pode_agir_map`. (`app/templates/chamados/partials/modal_fio.html:651–670`.) |
| `painel_chamado_turno.html` | **Não**. Mesmos três destinos, quando não fechado; a view exige gestor. (`app/templates/chamados/partials/painel_chamado_turno.html:65–78`; `app/chamados/views.py:347–350`.) |

Nos dois detalhes, portanto, a opção é oferecida em **`aberto`, `em_analise`, `resolvido`, `registrado`, `cancelado` e `superado`**: são todos os estados cadastrados, exceto `fechado`. (`app/chamados/models.py:28–35`; `app/templates/chamados/detalhe.html:117–135`; `app/templates/chamados/detalhe_local.html:375–398`.)

`is_gestor` significa `is_staff` ou pertencimento aos grupos `dp_rh`, `gestor`, `admin_ti`. No detalhe local, `pode_agir` consulta o departamento dono do chamado, incluindo as liberações para gestor geral e para usuários sem competências declaradas. (`app/chamados/views.py:24`, `:35–38`; `app/chamados/services/acoes_chamado.py:403–404`; `app/chamados/utils.py:32–72`.)

**Cada template enumera seus destinos.** O código compartilhado fornece a apresentação: `MOVER_PARA['aberto'] = 'Reabrir'`; `rotulo_de_movimento` retorna texto/title; `opcao_mover` gera a `<option>`. Essa tag recebe somente o destino, sem chamado, estado atual ou papel. (`app/chamados/services/verbos.py:132–146`; `app/chamados/templatetags/chamados_tags.py:67–78`.)

“Reabrir questionario”, existente no modal, é outra ação, enviada ao endpoint `reabrir_disputa_supervisao`; não é a opção `novo_status=aberto`. (`app/templates/chamados/partials/modal_fio.html:368–372`, `:665–670`.)

**2. Caminho da requisição e decisão do motor**

No **detalhe local**, para os seis estados acima, o caminho do formulário é:

1. POST para `chamados:detalhe_local` → rota `local/<chamado_id>/` → view `detalhe_chamado_local`. (`app/templates/chamados/detalhe_local.html:385`; `app/chamados/urls.py:14`; `app/chamados/views.py:359`.)
2. Guarda de departamento → `acao_resposta_livre`, que lê `novo_status` e chama `chamado.transicionar`. (`app/chamados/views.py:418–421`, `:547`; `app/chamados/services/acoes_chamado.py:287`, `:313–315`.)
3. `transicionar` obtém `get_perfil(user)` → `pode_transicionar` → `motor.decidir(estado, 'TRANSICAO_HUMANA', destino='aberto', perfil=perfil)`. (`app/chamados/models.py:119–134`.)

O motor **aceita `em_analise → aberto` para ambos os grupos de perfil**; aceita `resolvido → aberto` e `registrado → aberto` somente quando o perfil efetivo é `admin_ti`. As demais recusas estão discriminadas abaixo. (`app/chamados/catalogo/motor.py:101–105`, `:142–156`.)

Essa exceção usa o resultado de `get_perfil`, não `is_gestor`: superuser resulta em `admin_ti`; `is_staff`, sozinho, tem fallback `dp_rh`. (`app/chamados/models.py:130–132`; `app/colaboradores/mixins.py:154–178`.)

No **detalhe GLPI (`detalhe.html`)**, em qualquer dos seis estados, o formulário envia ao endpoint `followup`; a view lê apenas `conteudo`, chama `GLPIClient.adicionar_followup` e retorna o resultado da mensagem. **Não lê `novo_status` nem consulta o motor.** Em sucesso, responde “Mensagem enviada.”; em `GLPIError`, “Nao foi possivel enviar a mensagem. Tente novamente.” (`app/templates/chamados/detalhe.html:122`; `app/chamados/urls.py:12`; `app/chamados/views.py:292–310`; `app/chamados/glpi_client.py:219–230`.)

**As linhas do censo anterior continuam corretas:** `motor.py:102` exclui `aberto` dos destinos humanos de `resolvido`; `motor.py:153` aplica essa restrição para perfil diferente de `admin_ti`; a mensagem está em `:154–155`. (`app/chamados/catalogo/motor.py:102`, `:153–155`.)

**3. Pares em que a tela oferece e o motor recusa**

| Par (tela, status atual) | Perfil que recebe recusa | Mensagem |
|---|---|---|
| (`detalhe_local.html`, `aberto`) | Todos | `Chamado ja esta neste status.` (`app/chamados/catalogo/motor.py:148–149`.) |
| (`detalhe_local.html`, `resolvido`) | Diferente de `admin_ti` | `Transicao resolvido -> aberto nao permitida.` (`app/chamados/catalogo/motor.py:102`, `:153–155`.) |
| (`detalhe_local.html`, `registrado`) | Diferente de `admin_ti` | `Transicao registrado -> aberto nao permitida.` (`app/chamados/catalogo/motor.py:105`, `:153–155`.) |
| (`detalhe_local.html`, `cancelado`) | Todos | `cancelado e terminal absoluto - nao reabre (revival = re-emissao).` (`app/chamados/catalogo/motor.py:142–144`.) |
| (`detalhe_local.html`, `superado`) | Todos | `superado e terminal absoluto - nao reabre (revival = re-emissao).` (`app/chamados/catalogo/motor.py:142–144`.) |

Esses estados recebem a opção porque o template exclui apenas `fechado`; para gestor autorizado, inclui `aberto` sem consultar a matriz. (`app/templates/chamados/detalhe_local.html:375–398`.)

O motivo vira `ValueError`, é capturado sem tradução e segue como erro ao usuário. No formulário comum, há mensagem Django e redirect, e o detalhe renderiza a mensagem; em HTMX, o helper emite `showToast`. (`app/chamados/models.py:132–134`; `app/chamados/services/acoes_chamado.py:320–321`; `app/chamados/views.py:410–415`; `app/core/respostas.py:63–71`; `app/templates/chamados/detalhe_local.html:39–42`.)

`detalhe.html` não entra nessa lista de **recusas do motor**, pois seu endpoint não executa a transição. (`app/chamados/views.py:292–310`.)

**4. Origem da diferença entre telas**

A disponibilidade vem da **enumeração feita em cada template**; o mapa compartilhado centraliza rótulos, não decide quais destinos oferecer conforme estado/papel. Isso aparece diretamente nas cinco listas e na assinatura de `opcao_mover(status_local)`. (`app/templates/chamados/detalhe.html:132–135`; `app/templates/chamados/detalhe_local.html:395–398`; `app/templates/chamados/fio_colaborador.html:268–270`; `app/templates/chamados/partials/modal_fio.html:668–670`; `app/templates/chamados/partials/painel_chamado_turno.html:76–78`; `app/chamados/templatetags/chamados_tags.py:68–78`.)

A diferença também está explicitamente preservada no teste existente: `DESTINOS` inclui `aberto` apenas nos dois detalhes, e a asserção compara conjunto e ordem de cada seletor. (`app/chamados/tests/test_bo_rotulo_encerrado.py:71–79`, `:136–141`.)

## 12/11 07:56 · sessao 4d1d3242e9fe · - · - · origem=Ronald (tmux)

O caminho que chega ao motor é o de `detalhe_local.html`; o de `detalhe.html` envia apenas um followup ao GLPI, ignorando `novo_status`. [`app/templates/chamados/detalhe_local.html:385`, `app/chamados/views.py:547`, `app/chamados/services/acoes_chamado.py:315`; `app/templates/chamados/detalhe.html:122`, `app/chamados/views.py:292`, `app/chamados/views.py:304`]

- `detalhe_local.html` oferece `aberto`/“Reabrir” para `is_gestor`, com `pode_agir`, sempre que o status não é `fechado`; a lista é fixa e inclui a própria situação `aberto`, além de `em_analise`, `resolvido`, `registrado`, `cancelado` e `superado`. [`app/templates/chamados/detalhe_local.html:375`, `app/templates/chamados/detalhe_local.html:380`, `app/templates/chamados/detalhe_local.html:390`, `app/templates/chamados/detalhe_local.html:398`, `app/chamados/models.py:28`]
- `is_gestor` significa `is_staff` ou grupo `dp_rh`, `gestor`, `admin_ti`; `pode_agir` vem do controle por departamento dono do chamado. [`app/chamados/views.py:24`, `app/chamados/views.py:35`, `app/chamados/services/acoes_chamado.py:403`, `app/chamados/utils.py:68`, `app/chamados/utils.py:52`]

Caminho do clique local: `POST chamados:detalhe_local` → rota `local/<chamado_id>/` → `detalhe_chamado_local` → guarda de departamento → `acao_resposta_livre` → `ChamadoColaborador.transicionar` → `pode_transicionar` → `motor.decidir(..., 'TRANSICAO_HUMANA', destino='aberto', perfil=get_perfil(user))`. [`app/templates/chamados/detalhe_local.html:385`, `app/chamados/urls.py:14`, `app/chamados/views.py:359`, `app/chamados/views.py:420`, `app/chamados/views.py:547`, `app/chamados/services/acoes_chamado.py:313`, `app/chamados/models.py:128`, `app/chamados/models.py:119`]

| Status de origem no detalhe local | Motor com perfil diferente de `admin_ti` | Motor com perfil `admin_ti` |
|---|---|---|
| `aberto` | Recusa: `Chamado ja esta neste status.` | Mesma recusa. [`app/chamados/catalogo/motor.py:148`] |
| `em_analise` | Aceita → `aberto`. | Aceita → `aberto`. [`app/chamados/catalogo/motor.py:101`, `app/chamados/catalogo/motor.py:153`, `app/chamados/catalogo/motor.py:156`] |
| `resolvido` | Recusa: `Transicao resolvido -> aberto nao permitida.` | Aceita → `aberto`. [`app/chamados/catalogo/motor.py:102`, `app/chamados/catalogo/motor.py:153`] |
| `registrado` | Recusa: `Transicao registrado -> aberto nao permitida.` | Aceita → `aberto`. [`app/chamados/catalogo/motor.py:105`, `app/chamados/catalogo/motor.py:153`] |
| `cancelado` | Recusa: `cancelado e terminal absoluto - nao reabre (revival = re-emissao).` | Mesma recusa. [`app/chamados/catalogo/motor.py:142`] |
| `superado` | Recusa: `superado e terminal absoluto - nao reabre (revival = re-emissao).` | Mesma recusa. [`app/chamados/catalogo/motor.py:142`] |

As referências antigas continuam corretas: a linha **102** exclui `aberto` dos destinos humanos de `resolvido`; a linha **153** aplica a recusa, com exceção de `admin_ti`; a mensagem está nas linhas 154–155. [`app/chamados/catalogo/motor.py:102`, `app/chamados/catalogo/motor.py:153`]

Cada recusa vira `ValueError(motivo)`, é capturada sem tradução e entregue ao usuário com o mesmo texto: `resp('erro', str(_ve))` → `_resp` → `resposta_acao`; formulário comum recebe mensagem Django e redirect, HTMX recebe `showToast`. [`app/chamados/models.py:132`, `app/chamados/services/acoes_chamado.py:320`, `app/chamados/views.py:410`, `app/core/respostas.py:63`, `app/core/respostas.py:68`]

O perfil do motor é o resultado efetivo de `get_perfil`, não o booleano `is_gestor`: superuser resulta em `admin_ti`; depois entram competência de grupos, perfil do colaborador e fallback `is_staff → dp_rh`. [`app/chamados/models.py:130`, `app/colaboradores/mixins.py:154`]

No `detalhe.html`, o seletor também oferece `Reabrir` para gestor quando `status_local != fechado`, mas o caminho termina em `adicionar_followup`, sem consulta ao motor e sem alteração de status. A mensagem é `Mensagem enviada.` se o GLPI aceitar o followup, ou `Nao foi possivel enviar a mensagem. Tente novamente.` se ocorrer `GLPIError`. [`app/templates/chamados/detalhe.html:117`, `app/templates/chamados/detalhe.html:127`, `app/templates/chamados/detalhe.html:135`, `app/chamados/views.py:292`, `app/chamados/views.py:301`, `app/chamados/views.py:305`]

Há ainda uma diferença literal de identificador nesse caminho: o formulário passa `chamado.pk`, a rota chama esse valor de `ticket_id`, e a view o encaminha diretamente ao GLPI; o GET encontra o registro local por `ticket_id_glpi`. O efeito concreto para um registro depende dos valores dos dois identificadores. [`app/templates/chamados/detalhe.html:122`, `app/chamados/urls.py:12`, `app/chamados/views.py:304`, `app/chamados/views.py:252`; efeito em produção: **[não verificado]**]

A lista não é gerada pela matriz do motor: `MOVER_PARA['aberto']` define somente o rótulo `Reabrir`, `rotulo_de_movimento` retorna rótulo/title e `opcao_mover` produz a `<option>` sem receber chamado, status atual ou papel. Cada template escolhe explicitamente quais destinos incluir. [`app/chamados/services/verbos.py:132`, `app/chamados/services/verbos.py:140`, `app/chamados/templatetags/chamados_tags.py:68`]

## 12/11 07:56 · sessao e99ba5fece4f · - · - · origem=Ronald (tmux)

Oferta de `novo_status=aberto`:

| Tela | Condição e destino |
|---|---|
| `detalhe.html` | `status_local != fechado` e `is_gestor`; lista inclui `aberto`. `app/templates/chamados/detalhe.html:117`, `app/templates/chamados/detalhe.html:127`, `app/templates/chamados/detalhe.html:135` |
| `detalhe_local.html` | `status_local != fechado`, `is_gestor` e `pode_agir`; lista inclui `aberto`. `app/templates/chamados/detalhe_local.html:375`, `app/templates/chamados/detalhe_local.html:380`, `app/templates/chamados/detalhe_local.html:390`, `app/templates/chamados/detalhe_local.html:398` |
| `fio_colaborador.html` | Não inclui `aberto`; só `em_analise`, `resolvido`, `fechado`. Formulário exige chamado não fechado, sem GLPI e `pode_agir_map`. `app/templates/chamados/fio_colaborador.html:254`, `app/templates/chamados/fio_colaborador.html:255`, `app/templates/chamados/fio_colaborador.html:268` |
| `modal_fio.html` | Não inclui `aberto`; mesmos três destinos, sob chamado não fechado, sem GLPI, fora de `modo_beneficio` e `pode_agir_map`. `app/templates/chamados/partials/modal_fio.html:651`, `app/templates/chamados/partials/modal_fio.html:652`, `app/templates/chamados/partials/modal_fio.html:668` |
| `painel_chamado_turno.html` | Não inclui `aberto`; mesmos três destinos sob chamado não fechado. A view restringe acesso a gestor. `app/templates/chamados/partials/painel_chamado_turno.html:65`, `app/templates/chamados/partials/painel_chamado_turno.html:76`, `app/chamados/views.py:349` |

Os seis status oferecidos nos detalhes, por exclusão somente de `fechado`, são `aberto`, `em_analise`, `resolvido`, `cancelado`, `registrado`, `superado`; o universo de choices está em `app/chamados/models.py:28`.

`is_gestor` corresponde a `is_staff` ou grupo `dp_rh`, `gestor`, `admin_ti`. `app/chamados/views.py:24`, `app/chamados/views.py:35`.

`pode_agir` do detalhe local vem do contexto (`app/chamados/services/acoes_chamado.py:404`) e consulta departamento do chamado (`app/chamados/utils.py:68`), com gestor geral superuser/admin_ti (`app/chamados/utils.py:47`), compatibilidade por competências ou perfil e liberação legado sem competências (`app/chamados/utils.py:52`).

A lista de destinos é enumerada em cada template; o código comum fornece apenas rótulo/title e HTML da opção: `MOVER_PARA['aberto']='Reabrir'` em `app/chamados/services/verbos.py:136`; retorno do par em `app/chamados/services/verbos.py:146`; geração de `<option>` em `app/chamados/templatetags/chamados_tags.py:67`. A divergência das listas também é congelada no teste em `app/chamados/tests/test_bo_rotulo_encerrado.py:71` e conferida em `app/chamados/tests/test_bo_rotulo_encerrado.py:136`.

**Ressalva relevante para caminho:** `detalhe.html` posta em `chamados:followup` (`app/templates/chamados/detalhe.html:122`), cujo handler lê apenas `conteudo` (`app/chamados/views.py:292`), envia followup GLPI (`app/chamados/views.py:303`) e retorna “Mensagem enviada.” em sucesso (`app/chamados/views.py:301`, `app/chamados/views.py:310`); não lê `novo_status` nem chama motor no corpo da função (`app/chamados/views.py:273`).

O modal contém outras reaberturas: “Reabrir questionario”, para disputa existente e `pode_agir_map`, disputa aberta, em endpoint próprio (`app/templates/chamados/partials/modal_fio.html:123`, `app/templates/chamados/partials/modal_fio.html:326`, `app/templates/chamados/partials/modal_fio.html:368`); “Reabertura administrativa”, para disputa fechada, admin TI e menos de duas reaberturas (`app/templates/chamados/partials/modal_fio.html:376`, `app/templates/chamados/partials/modal_fio.html:382`, `app/templates/chamados/partials/modal_fio.html:383`, `app/templates/chamados/partials/modal_fio.html:390`). São formulários distintos do seletor `novo_status` (`app/templates/chamados/partials/modal_fio.html:665`).

## 12/11 07:53 · sessao 1ae2b1829e34 · - · - · origem=Ronald (tmux)

Censo em memória confirmou:

- Totais: pareadores 9, derivações 85; critérios A1=6/B1=82, conforme `core/tests/test_medida_turno_do_par.py:104`.
- Identidades/sítios, composição e CLASSIFICAR permanecem iguais ao registro, conferidos pelo censo de `core/medida_turno_do_par.py:448`.
- Apenas quatro entradas de linhas diferem:
  - `chamados/services/materializacao.py:171`: `[170] → [171]`.
  - `ponto/management/commands/detectar_intervalo_ausente.py:83`: `[82] → [83]`.
  - `ponto/services/triagem_batida.py:425`: `[424] → [425]`.
  - `ponto/services/triagem_batida.py:248`, `:255`, `:261`: `[247,254,260] → [248,255,261]`.

Os três arquivos têm import de `fecho_runtime` antes das ocorrências, respectivamente em `materializacao.py:43`, `detectar_intervalo_ausente.py:18`, `triagem_batida.py:6`. Retirar os imports de topo em `ponto` pode devolver suas ocorrências às linhas registradas; recalcule após a cura A16. O conteúdo histórico anterior dessas ocorrências, sem ler a base, é [nao verificado].

Execução usada: `PYTHONDONTWRITEBYTECODE=1 python3 -B -`, carregando `core/tests/test_medida_turno_do_par.py` em memória com a substituição documentada em `core/tests/test_medida_turno_do_par.py:20`, e comparando `m.censo(_app())` com os registros. Nenhum arquivo foi alterado.

## 12/11 07:53 · sessao 3b060935ddd6 · - · - · origem=Ronald (tmux)

(C) O selo não reconhece o encaminhamento por `ato=`: classifica `ato=ch.reconciliar` como “método preso a apelido” em [test_selo_reconciliar_le_o_retorno.py:57](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_selo_reconciliar_le_o_retorno.py:57).

A porta preserva o retorno: devolve `ato()` diretamente em [fecho_runtime.py:160](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/fecho_runtime.py:160), captura `resultado = ato()` em [fecho_runtime.py:165](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/fecho_runtime.py:165) e devolve esse valor em [fecho_runtime.py:169](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/fecho_runtime.py:169).

Os cinco sítios de `_julgar_colab_corpo` continuam lendo o booleano em filtros/condicional, antes de contar ou registrar fechamento: [cartorio.py:1538](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/cartorio.py:1538), [cartorio.py:1551](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/cartorio.py:1551), [cartorio.py:1572](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/cartorio.py:1572), [cartorio.py:1650](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/cartorio.py:1650), [cartorio.py:1665](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/cartorio.py:1665).

Correção recomendada do selo: reconhecer o atributo somente como argumento `ato=` da porta real, conferir igualdade entre receptor e primeiro argumento e verificar descarte sobre a chamada da porta. Preservar detecção de apelidos, `getattr` e portas desconhecidas. A allowlist existente deve continuar contendo apenas os dois descartes deliberados: [detectar_ausencias.py:159](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/management/commands/detectar_ausencias.py:159) e [materializacao.py:588](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/materializacao.py:588).

Mutantes recomendados: chamada da porta em statement; porta falsa; receptor diferente; remoção da leitura no chamador; `return resultado` substituído por valor constante na porta. O teste existente já exige a preservação exata de `False` em [test_e4b_c12_memoria.py:31](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c12_memoria.py:31). [nao verificado]

Nenhum arquivo foi escrito; execução dos testes e dos mutantes [não verificado]. [nao verificado]

## 12/11 07:14 · sessao 1db4d2d51bc8 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

Cura c10 escrita na cópia. A–D, funções alteradas, escritores, leitores e os dois contadores estão detalhados no [relato](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/LASTRO_C10.txt:5).

- **43 testes locais verdes**, incluindo P1–P5: [execuções](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_c10_execucoes.txt:12). [nao verificado]
- **RED/GREEN A–D e nove mutantes mortos por asserção**, com cada comando e saída integral; RED executado sobre recortes confirmados da c9: [provas](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/LASTRO_C10.txt:113).
- Teste 05 atualizado para criar `reciclagem` pela porta; pares de criar/ativar sem faixa escritos: [testes](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/relatorios/tests/test_rotulo_ausencia_pdf.py:52). [nao verificado]

**Sem commit**, conforme a proibição inicial. [Auditoria](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_c10_auditoria.txt:10). [nao verificado]

**Suite ORM e par `simular_folha` comp11: [não verificado]** — banco e Docker permaneceram proibidos. Não afirmo delta financeiro nem certificação; seguem para Code e opus. [Limites](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/LASTRO_C10.txt:254). [nao verificado]

## 12/11 03:03 · sessao 2fd88cf92d26 · AUDITORIA-DE-TRAVAS · leitura · origem=Code

| trava | a — disparos | b — defeito real confirmado | c — custo sem defeito de comportamento | d — suites inteiras / minutos | proposta | fonte |
|---|---|---|---|---|---|---|
| **1. Preverif Codex** | Total não medido; **15 voltas triadas**, 12 procedentes e 3 descartadas. | **≥2 episódios com cura**, detalhados abaixo; 12 procedentes não equivalem a 12 defeitos. | **3 consultas descartadas**; voltas adicionais causadas por elas: não medido. | Consultas triadas: **80,58 min**, sendo **13,52 min** descartadas. Suites atribuíveis: não medido. | **MUDA** a medição, conserva a revisão. Retirar perde achados comprovados; manter custa as consultas, inclusive descartadas. | `anexo:triagem.log:1–15`; `app/docs/RELATO.md:1446,322,327–328`; cruzamento C descrito no §1. |
| **2. Opus final** | Total de leituras não medido; **48 disparos B3** têm rótulo de reprovação final. | **≥3 episódios comprovados**: PAR-NAO-ADJACENTE, SELOS-CANON-I1 e E3. | **≥5 reprovações por forma/prova/documento**. | **5 suites posteriores associadas / 88,11 min**; incremento causal não medido. Duração das leituras finais: não medido. | **FICA**. Retirar perde descobertas posteriores ao preverif; manter conserva revisão e custos formais documentados. | `app/docs/RELATO.md:539,543,548–550,138,1275–1278,1415,1426`; D e S do §2. |
| **3. Disjuntor B3** | **50 disparos; 48 liberações**, sendo 22 desenho e 26 erro-bobo; **6 rearmes e 7 migrações**. | Ganho próprio do disjuntor: **não medido**; ele recebe resultados das revisões. | Os mesmos **≥5 episódios formais** do item anterior; não somar novamente. | Mesmas **5 suites / 88,11 min associados**; espera imposta pelo B3: não medida. | **MUDA** a escalada: distinguir defeito, prova/forma e desenho. Retirar perde a interrupção de ciclos; manter custa parada e liberação sem custo completo medido. | `anexo:disjuntor_14d.log`, linhas por evento; `bin/disjuntor.py:834–848,955–957,1004–1014`; §3. |
| **4. Hook Stop** | **1.006 registros BLOCK fila**, 2 BLOCK RELATO e 3 BLOCK ORCAMENTO; **237 liberações por teto**. | Não medido. | Não medido; teto não comprova falso positivo. | Suites e minutos atribuíveis: não medido. | **MUDA** a elegibilidade. Retirar perde contenção do encerramento; manter conserva cobrança incompatível com a lista e a série vigentes. | `anexo:hook_stop_por_dia.tsv:1–78`; `bin/hook_stop_fila1.py:238–252,270–287,1404,1439–1448`; §4. |
| **5. Hook PreToolUse** | É **`bin/disjuntor.py pretool`**. Invocações e negativas próprias: não medidas. | Não medido. | Não medido. | Suites/minutos: não medido. | **FICA**, com medição e conferência da instalação. Retirar perde as verificações B1/B3/B4/CENSO; manter tem custo por ferramenta e negativas não quantificados. | `.claude/settings.json:62–69`; `bin/disjuntor.py:1820–1872,1901–1907`; §5. |
| **6. ESPERANDO** | **1.839 liberações com prova + 590 registros sem prova**; não são esperas únicas. | Não medido. | **≥1 espera improdutiva documental**, de **100 min**, após morte do produtor. | **100 min documentais** nesse incidente; suites e total do período não medidos. | **MUDA** a reavaliação após o produtor terminar. Retirar perde liberação durante trabalho comprovado; manter conserva o risco de espera vencida. | `anexo:hook_stop_por_dia.tsv:1–78`; `app/docs/BACKLOG.md:1933`; `bin/hook_stop_fila1.py:789–838,1383–1390`. |
| **7. Uma vaga de construtor por repo** | Não medido: recusa por vaga ocupada ocorre antes do registro da chamada. | Colisões realmente evitadas: não medido. | Recusas desnecessárias: não medido. | Suites/minutos de serialização ou retomada: não medido. | **FICA**. Retirar perde exclusão entre construtores Codex; manter custa serialização não medida. | `bin/codex_pergunta.sh:235–260,291–293`; §7. |
| **8. Catraca de número de linha** | **≥4 disparos documentados como único vermelho de suite inteira**. | Defeitos de comportamento pegos por essa subtrava: não medido. | **≥4 episódios de deslocamento de linha**. | **4 falhas / 88,64 min**, seguidas de **4 repetições / 89,89 min**; **8 suites / 178,53 min observados**. | **MUDA** para identidade estrutural/conteúdo. Retirar posição absoluta perde aviso de deslocamento; manter conserva repetições documentadas. Economia efetiva de uma mudança: não medida. | `app/core/tests/test_medida_turno_do_par.py:539–543`; `app/docs/RELATO.md:46,547,553,560`; S do §8. |
| **9. Timeout do construtor** | **3 chamadas construtor rc=124**. | Defeito real evitado pelo encerramento: não medido. | Não medido; **2 entregas parciais preservadas** estão documentadas. | **45,27 min nas 3 chamadas**; suites e retrabalho adicional não medidos. | **MUDA** o teto mediante duração/progresso medidos. Retirar perde limite de ocupação; manter pode interromper trabalho aproveitável. | `bin/codex_pergunta.sh:83,401,419–433`; `app/docs/RELATO.md:296,519`; C do §9. |
| **10. Selo `padrao=`** | Total não medido; **2 commits só de comentário/anotação barrados** estão documentados. | Defeitos de comportamento pegos pelo campo: não medido. | **2 commits de comentário**, com **19 pares de árvores idênticas** após reescrita; outro episódio tem **3 rewords de instrumento**. | Suites/minutos atribuíveis: não medido. | **MUDA** a classificação por conteúdo, preservando a declaração. Retirar perde cobrança de termo/padrão; manter conserva reescrita formal sem mudança de árvore. | `app/docs/BACKLOG.md:1906`; `app/docs/RELATO.md:319,483`; `bin/tests/test_lei_akita.sh:122–123,307–325`. |
| **11. Suite, trava e vigias** | Negativas/esperas totais: não medidas. | Defeitos e colisões evitados no período: não medidos. | Não medido. | Construção TRAVA-SELO: **8 verificadores + 1 censo / 45,52 min**. Custo recorrente em suites/espera: não medido. Documento registra vigia e réguas atualmente em **0 suites/dia**, sem log anexo que confirme toda a janela. | **FICA** a exclusão; **MUDA** a observabilidade e a cobertura das entradas. Retirar perde serialização; manter conserva fila não medida e entradas fora da guarda de motivo. | `bin/trava_teste.sh:21–41`; `bin/suite.sh:173–175,221–226`; `app/docs/BACKLOG.md:2385,2390–2391`; C do §11. |
| **12. SUITE-SO-NO-FIM** | Execuções atribuíveis ao piloto: não medidas; foi substituído **06:28 → 06:29, “sem efeito”**. | Descobertas causadas pelo piloto: não medidas. | Suites evitáveis: não medidas. | E3: **7 inteiras / 161,44 min**; E4B-ALCANCE: **6 / 128,48 min**. **13 / 289,92 min observados**, sem atribuição causal ao piloto. | **MUDA** a avaliação: medir fases da conduta substituta. Mais inteiras custam repetição; adiá-las pode atrasar descoberta global, risco não quantificado. | `app/docs/PROMPTS.md:2086–2087`; S do §12; `bin/suite.sh:80–98,133–175`. |

Nas referências abaixo, **C[hora, obra]** identifica a linha exata de `anexo:chamadas_fim.tsv`; **S[caminho]**, a linha de `anexo:suites_14d.tsv` pelo campo `log`; **D[hora, evento/obra]**, a linha de `anexo:disjuntor_14d.log`; **G[hash]**, a linha de `anexo:gitlog_14d.txt`. São seletores de linha, sem inventar numeração. As propostas são insumo para decisão; custos compartilhados entre travas não devem ser somados novamente.

**1. Preverif Codex**

As triagens procedentes são as linhas **1–6, 8–9 e 11–14**; descartadas, **7, 10 e 15**. O escritor define uma volta como uma chamada, independentemente de quantos achados ela contém. Portanto, o anexo sustenta **12 voltas procedentes**, não uma contagem de defeitos. (`anexo:triagem.log:1–15`; `bin/disjuntor.py:870–872`.)

Há cura comprovada em **F1C4-3-EMISSORES**, volta 13, commit **`e548d8ca`**, após triagem procedente do preverif v12; e em **FIO-E0-V4-E4A-2**, preverif9, com reversão da mudança indevida do juiz e ajuste da prova nas voltas **`ea76f9f16` / `858bc0527`**. (`app/docs/RELATO.md:1446,322,327–328`; G[`ea76f9f16`], G[`858bc0527`].)

O cruzamento pela obra e pelo instante `fim` embutido na triagem soma **4.024 s procedentes + 811 s descartados = 4.835 s / 80,58 min**. As descartadas correspondem a C[10/11 02:26:14, REVERTER-DIA-SEM-RECALCULO], C[10/11 08:47:56, O169-FECHADA] e C[10/11 11:56:50, FIO-E0-V4-E4A-2]: **231 + 229 + 351 s**. Não há ligação suficiente para atribuir suites ou espera adicional a essas consultas. (`anexo:triagem.log:1–15`; respectivas linhas C.)

Para medir o total faltam classificação explícita de cada chamada como preverif, achado individual, triagem e commit da cura; `chamadas_fim.tsv` preserva papel e duração, e `triagem.log` preserva veredito por volta. (`bin/codex_pergunta.sh:253–260`; `bin/disjuntor.py:900–912`.)

**2. Opus final**

Três episódios comprovam utilidade posterior ao preverif:

- **PAR-NAO-ADJACENTE:** revisão final confirmou conflito entre pausa abaixo do piso e intervalo; cura **`d361dfee`**. (`app/docs/RELATO.md:548–550`.)
- **SELOS-CANON-I1:** comparação com célula crua deixava obra nascer sem censo; cura registrada na cópia **`ff47644a`**. (`app/docs/RELATO.md:539,543–544`.)
- **FIO-E0-V4-E3:** marcador sem horizonte alcançaria competências fora do escopo; cura **`2e0810cd0`**, pouso **`072556ec8`**. (`app/docs/RELATO.md:138`; G[`2e0810cd0`], G[`072556ec8`].)

Os **cinco episódios formais** são F1C4 v11, GRADE-FOLGA-JUIZ de 01/11, TOAST e duas reprovações MODO-COMUM. As suites posteriores associadas são:

| Linha S | segundos |
|---|---:|
| `f1c4emis/v13/suite.log` | 1.173,218 |
| `grade_juiz/vb/suite_60258b80.log` | 1.036,669 |
| `toast/suite_36768b29.log` | 1.165,072 |
| `mc/suite_2d78b6ec.log` | 961,538 |
| `mc/suite_a25e2149.log` | 950,240 |
| **Soma calculada das linhas acima** | **5.286,737 = 88,11 min** |

A classificação formal está em `app/docs/RELATO.md:1426,1415,1278,1275–1276`. A soma mede execuções associadas às curas; não demonstra economia contrafactual de remover a revisão.

Faltam início/fim, modelo efetivo e identificação das leituras finais para medir seu custo total. A configuração do agente usa **`model: inherit`**: o nome do papel sozinho não comprova execução em opus. (`.claude/agents/verificador.md:3–7`.)

**3. B3**

Os **50 disparos**, por data, são: 31/10 **6**; 01/11 **6**; 02/11 **5**; 04/11 **4**; 05/11 **7**; 06/11 **1**; 07/11 **3**; 08/11 **3**; 09/11 **2**; 10/11 **4**; 11/11 **7**; 12/11 **2**. A contagem considera exclusivamente linhas `DISJUNTOR B3`; liberações, rearmes e migrações são eventos separados. (`anexo:disjuntor_14d.log`, linhas com esses eventos e datas.)

Dois disparos explicitam **três voltas Codex procedentes**, ambos E4A-2: **10/11 02:22:34 e 11:49:24**. DATA-EVENTO-CRU apresenta inconsistência: o disparo diz revisão final, enquanto a liberação relata terceira volta Codex; origem consistente [nao verificado]. (D[10/11 02:22:34, B3 E4A-2], D[10/11 11:49:24, B3 E4A-2], D[11/11 17:31:19, B3 DATA-EVENTO-CRU], D[11/11 17:31:55, LIBERA DATA-EVENTO-CRU].)

**As 26 liberações erro-bobo não são 26 falsos positivos.** Há defeitos de implementação nesse grupo, inclusive H12H06 e OTIMIZA-3; a liberação por texto tampouco certifica cura. (D[05/11 20:48:58, LIBERA H12H06], D[05/11 00:47:21, LIBERA OTIMIZA-3]; `bin/disjuntor.py:1025–1031`.)

Para medir espera e ganho próprios faltam negativa individual, intervalo efetivamente ocioso, trabalho que continuou durante a parada e ligação à cura. O registro atual anuncia a transição e o motivo, sem esses campos. (`bin/disjuntor.py:193–201,1007–1014`.)

**4. Stop e os “937 andáveis”**

A causa está no código: `_itens_andaveis()` percorre **toda a tabela OBRAS**, excluindo itens pelo estado reconhecido por `_item_parado()`; `_andaveis_fora_do_em_curso()` somente remove o ID em curso; o recado imprime o tamanho restante. A lista fechada CONGELA e a ordem serial não participam desse cálculo. (`bin/hook_stop_fila1.py:95–97,166–180,238–252,270–287`; `app/docs/CORTES.md:446`; `app/docs/BACKLOG.md:1758`.)

Assim, um item pode continuar contado por estar aberto na tabela mesmo estando fora da vez pela lista fechada ou pela série. O ramo manda pegar o próximo da ordem, mas calcula a quantidade a partir da tabela ampla. (`bin/hook_stop_fila1.py:270–287,1391–1393`.)

**Quantas mensagens desse tipo houve: não medido.** Existem **1.006 registros da classe ampla** `BLOCK fila 1 com item aberto`; o agregado não preserva a subclasse nem o número anunciado. O episódio específico “937”, às 02:4x, não tem sua saída crua anexada [nao verificado]. (`anexo:hook_stop_por_dia.tsv:1–78`.)

Além disso, `BLOCK fila` é registrado antes de possível liberação por teto: **1.006 registros não equivalem a 1.006 decisões finais bloqueadas**. Faltam log/transcript cru, identificação da execução e decisão final para separar bloqueio útil, repetição e recado de elegibilidade incorreta. (`bin/hook_stop_fila1.py:1404,1439–1448`.)

**5. PreToolUse**

A instalação versionada aponta para **`bin/disjuntor.py pretool`**. Eventos `via=wrapper` no anexo não podem ser contabilizados como invocações desse hook: o wrapper também chama a porta CENSO diretamente. (`.claude/settings.json:62–69`; `bin/codex_pergunta.sh:229–234`.)

Há falha de instalação documentada em sessões iniciadas em `app/`, com B3 observado depois do pouso pelo Stop; isso é evidência documental, não censo completo das falhas do período. A instalação efetiva atual no host permanece [nao verificado]. (`app/docs/RELATO.md:7539,7676`.)

Para medir a–d faltam registros próprios de entrada, permissão/negação, regra responsável e início/fim. As negativas atuais registram o motivo, sem duração nem desfecho da obra. (`bin/disjuntor.py:1901–1907`.)

**6. ESPERANDO**

As **1.839 aceitações e 590 registros sem prova** não distinguem primeiras observações, tentativas repetidas e esperas únicas. Uma recusa pode decorrer de PID morto, campo inválido ou ausência de avanço observado; não comprova falso positivo. (`anexo:hook_stop_por_dia.tsv:1–78`; `bin/hook_stop_fila1.py:721–723,798–835`.)

O incidente documentado é preciso: produtor terminou, a linha deixou de ser válida e a esteira ficou **1h40** parada; ESPERANDO só era reavaliado no Stop. O log do incidente não foi anexado, portanto os **100 min** são medida documental, não reconstrução independente. (`app/docs/BACKLOG.md:1933`.)

Faltam identidade da espera, término do produtor, próxima reavaliação e retomada para medir o restante do período. A memória de mtime atual não constitui esse histórico. (`bin/hook_stop_fila1.py:681–723`.)

**7. Uma vaga por repositório**

O wrapper toma **`flock -n`** em `logs/construtor.vaga`, usando o repositório compartilhado pelas worktrees. A recusa por ocupação ocorre **antes** do trap e do registro `ini`; por isso, ausência em `chamadas_fim.tsv` não significa ausência de recusas. (`bin/codex_pergunta.sh:69–78,235–269,291–293`.)

A vaga cobre construtores Codex; sozinha não demonstra cumprimento da ordem serial entre todos os tipos de construtor. Para medir colisões evitadas e custo de ficar faltam tentativa, ocupante, recusa e retomada por obra. (`bin/codex_pergunta.sh:235–240`; `app/docs/BACKLOG.md:1758`.)

**8. Catraca de linha**

O selo pedido é **`test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit`**. Ele compara literalmente `LINHAS_REGISTRADAS` com as posições retornadas pelo medidor; a identidade dos sítios já tem comparação separada imediatamente acima. (`app/core/tests/test_medida_turno_do_par.py:533–543`.)

| Obra; commit de registro | Suite falha → repetição verde | minutos calculados | [nao verificado]
|---|---|---:|
| E2; `93a48702` | S[`fio_e0_v4/e2_rebase/suite_v2.log`] → S[`fio_e0_v4/e2_rebase/suite_v3.log`] | 19,23 → 19,19 |
| PAR-NAO-ADJACENTE; `795cafdf` | S[`par_nao_adjacente/suite_v3.log`] → S[`par_nao_adjacente/suite_v4.log`] | 23,05 → 26,32 |
| SELOS-CANON-I1; `f0103bef` | S[`selos_canon/i1_suite_v4.log`] → S[`selos_canon/i1_suite_v5.log`] | 24,64 → 18,65 |
| UI-PAINEL; `2ca185e0e` | S[`UI-PAINEL/suite_v3.log`] → S[`UI-PAINEL/suite_v4.log`] | 21,73 → 25,74 |

Os relatos classificam esses vermelhos como deslocamentos de posição: texto preservado nos três primeiros e linhas movidas pelo encurtamento de função no UI-PAINEL. (`app/docs/RELATO.md:560,553,547,46`.)

As **quatro repetições / 89,89 min** são custo observado após esses bloqueios, não economia garantida: por exemplo, E2 também incorporou main antes da repetição. Misturas com defeitos reais em outras suites não entram nessa conta exclusiva. (`app/docs/RELATO.md:560,223,275,316`; linhas S acima.)

A proposta preserva o controle de sítios novos/alterados, retirando posição absoluta como motivo isolado de reprovação. Sua eficácia ainda é [nao verificado]; o próprio medidor declara não provar semântica, fluxo ou chamadas dinâmicas. (`app/core/medida_turno_do_par.py:18–20,32–38`.)

**9. Timeout de 900 s**

As linhas de construtor com `rc=124` são:

- C[09/11 07:47:50, FIO-E0-V4-E3]: **907 s**.
- C[10/11 00:30:33, PAUTA-SEM-TURNO-PREFIXO-RESERVADO]: **904 s**.
- C[10/11 17:12:03, BO-DISPUTA-SEM-PERGUNTA]: **905 s**.

A soma é **2.716 s / 45,27 min**. O default é configurável; construtor usa `timeout -k 30`, e a duração registrada inclui preparação/espera do wrapper. Portanto, duração acima de 900 s não demonstra sozinha falha do timeout. (`bin/codex_pergunta.sh:83,252,360–378,401`.)

E3 deixou código aproveitado no commit **`1394b61eb`**; BO-DISPUTA deixou arquivos tocados e entrega parcial. Isso comprova interrupção de trabalho aproveitável, sem comprovar que as chamadas estavam livres de defeitos. (`app/docs/RELATO.md:519,296`.)

Separadamente, as **8 recusas de limite**, somando **609 s / 10,15 min**, são `rc=75`, não timeout do construtor. (`anexo:limite.log:1–8`; C[05/11 15:05:08–15:05:46, respectivas obras]; `bin/codex_pergunta.sh:430–442`.)

**10. `padrao=` / PADRAO-NA-CURA**

O selo classifica negócio por caminho `app/`, ressalvando testes/docs; `n/a (instrumento)` não ganha análise que reconheça comentário apenas. A exceção de medida em Python é outra regra, limitada ao registro de módulos/dados. (`bin/tests/test_lei_akita.sh:122–137,316–321`.)

O caso pedido está registrado: **`11f82cfec` e `6c6689036`**, só comentário/anotação, viraram **`d75e5f2e1` e `ea1f0cab8`** alterando apenas `padrao=`; foram refeitos **17 descendentes**, com árvores iguais nos **19 pares**. A auditoria está aberta como **AUDITORIA-TRAVAS-POS-RESET-PADRAO-NA**. (`app/docs/BACKLOG.md:1906`; `app/docs/RELATO.md:319`.)

Outro episódio registra **3 rewords de instrumento**, também com árvores idênticas. Minutos e suites decorrentes não são medidos pelos anexos; faltam execução do portão, commit, instante, duração e retomada. (`app/docs/RELATO.md:483`; `bin/codex_pergunta.sh:253–260`.)

O campo verifica termo e admissibilidade; conferir se o diff realiza o padrão continua sendo leitura do verificador. (`bin/tests/test_lei_akita.sh:307–325`; `.claude/agents/verificador.md:8–15`.)

**11. Suite, trava, vigia e réguas** [nao verificado]

`bin/suite.sh` recusa inteira sem motivo válido antes do custo de montagem/docker; inteiras e recortes passam pela trava. A trava espera até **3.600 s**, retorna **75** se não obtiver a vez e mantém apenas ocupante atual; o histórico solicitado permanece pendente. (`bin/suite.sh:154,173–175,221–226`; `bin/trava_teste.sh:21–41`; `app/docs/BACKLOG.md:2391`.)

No anexo, TRAVA-SELO tem **8 chamadas verificador**, durações **432, 229, 225, 293, 266, 435, 238 e 229 s**, e **1 censo de 384 s**: **2.731 s / 45,52 min**, custo de construção do instrumento. O resumo documental “3 preverif, 1 opus” não deve substituir essa contagem observada. (C[11/11 06:40:31–08:02:16, TRAVA-SELO]; `app/docs/RELATO.md:141,154,157`.)

Para vigia/réguas, o documento registra **0 suites/dia atuais**, mas os logs citados não vieram anexados: o custo dos 14 dias permanece **não medido**. A implementação permite contar **1 inteira por chamada da régua relógio**, pela trava, e **3 por chamada da régua calendário**, fora dela. (`app/docs/BACKLOG.md:2390`; `bin/regua_relogio.sh:51–56`; `bin/regua_calendario.sh:41–46`.)

A unidade autorizada da guarda de motivo ficou restrita a **`suite.sh`**; régua e pre-push co

[truncado em 20000 de 24577 caracteres]

## 12/11 02:58 · sessao 8dc5a164915d · - · - · origem=Ronald (tmux)

Travas 1, 2, 3 e 9 — evidências para consolidação:

| Trava | Medição sustentada | Limitação / proposta |
|---|---|---|
| Preverif Codex | Anexo `triagem.log`: 15 linhas, 12 procedentes e 3 descartadas. Cruzamento com `chamadas_fim.tsv`: 80,58 min de consultas, sendo 67,07 min procedentes e 13,52 min descartadas. | Procedente conta uma **volta**, independentemente do número de achados; não prova sozinho defeito curado (`bin/disjuntor.py:870`, `bin/disjuntor.py:900`, `bin/disjuntor.py:911`). Proposta **MUDA**: conservar revisão e distinguir chamada, achado, cura e descarte; custo e economia completos não medidos. |
| Opus final | Pelo menos dois exemplos documentados de defeito confirmado após preverif: PAR-NAO-ADJACENTE, cura `d361dfee`; SELOS-CANON-I1, cura `ff47644a` na cópia, correspondente à volta 5 `f56dc9628` no gitlog fornecido (`app/docs/RELATO.md:548`, `app/docs/RELATO.md:550`, `app/docs/RELATO.md:539`, `app/docs/RELATO.md:543`). | Total de leituras e minutos não medidos: faltam registros de início/fim dos agentes finais. A definição usa `model: inherit`, portanto o nome do papel não prova modelo opus em toda chamada (`.claude/agents/verificador.md:5`). Proposta **FICA**: há falhas documentadas do preverif (`app/docs/RELATO.md:544`, `app/docs/RELATO.md:549`). |
| B3 | Contagem manual do anexo: 50 disparos, 48 liberações — 22 desenho e 26 erro-bobo —, 6 rearmes e 7 migrações. Dois disparos explicitam terceira volta Codex: E4A-2, 10/11 02:22:34 e 11:49:24. | B3 recebe reprovação ou terceira volta procedente; não diagnostica defeito (`bin/disjuntor.py:834`, `bin/disjuntor.py:955`, `bin/disjuntor.py:1007`). **ERRO-BOBO não significa sem defeito**. Proposta **MUDA**: separar confirmação de defeito, forma e pergunta de desenho; economia causal não medida. |
| Timeout construtor | Três linhas `construtor rc=124`: E3 09/11 07:47:50, 907 s; PAUTA 10/11 00:30:33, 904 s; BO-DISPUTA 10/11 17:12:03, 905 s. Soma 2.716 s = 45,27 min (`chamadas_fim.tsv`, linhas identificadas pelos horários). | Default 900 é configurável (`bin/codex_pergunta.sh:83`); execução construtor usa `timeout -k 30` (`bin/codex_pergunta.sh:401`). E3 teve entrega parcial preservada e commitada; BO-DISPUTA também teve arquivos tocados (`app/docs/RELATO.md:519`, `app/docs/RELATO.md:296`). Proposta **MUDA**: teto por trabalho medido; ganho de um novo teto não medido. |

Contagem manual dos 50 disparos B3 por data, para conferir: 31/10=6; 01/11=6; 02/11=5; 04/11=4; 05/11=7; 06/11=1; 07/11=3; 08/11=3; 09/11=2; 10/11=4; 11/11=7; 12/11=2 (`disjuntor_14d.log`, linhas `DISJUNTOR B3` nessas datas). O disparo DATA-EVENTO-CRU de 11/11 17:31:19 diz reprovação final, mas sua liberação às 17:31:55 relata terceira volta Codex; classificação consistente da origem fica [nao verificado].

Defeito real do preverif com cura explícita: F1C4-3-EMISSORES, triagem v12 procedente, volta 13 `e548d8ca`, com correção do teste nunca_bateu e documentação; isso demonstra pelo menos uma volta procedente com cura, sem transformar as 12 procedentes em 12 defeitos (`app/docs/RELATO.md:1446`). E4A-2 preverif9 confirmou criação indevida de perguntas por escritores em veredito não acusador; a errata exigiu reverter a mudança do juiz e ajustar a prova (`app/docs/RELATO.md:328`, `app/docs/RELATO.md:327`, `app/docs/RELATO.md:322`; commits `ea76f9f16` e `858bc0527` no anexo gitlog).

Há pelo menos cinco episódios documentados de reprovação por forma/prova/documento: F1C4 v11, GRADE-FOLGA-JUIZ, TOAST e duas voltas MODO-COMUM (`app/docs/RELATO.md:1426`, `app/docs/RELATO.md:1428`, `app/docs/RELATO.md:1278`, `app/docs/RELATO.md:1271`, `app/docs/RELATO.md:1276`). Cinco suites inteiras posteriores associadas somam **88,11 min**, sem comprovar que remover a trava evitaria essas execuções:

- `f1c4emis/v13/suite.log`: 1.173,218 s.
- `grade_juiz/vb/suite_60258b80.log`: 1.036,669 s.
- `toast/suite_36768b29.log`: 1.165,072 s.
- `mc/suite_2d78b6ec.log`: 961,538 s.
- `mc/suite_a25e2149.log`: 950,240 s.

Fonte dos cinco números: linhas com esses caminhos no anexo `suites_14d.tsv`. São custo associado, não incremento causal medido.

Escritores, leitores e cegueiras:

- **Chamadas**: escritor wrapper, linhas `ini` e `fim` ligadas por obra/hash/pid (`bin/codex_pergunta.sh:253`, `bin/codex_pergunta.sh:259`, `bin/codex_pergunta.sh:292`); leitor `codex_limite_estado._linhas_chamadas` (`bin/codex_limite_estado.py:49`), usado pelo gerador (`bin/gerar_codex_estado.py:41`). A duração começa antes de cópia, slots e trava da resposta, portanto inclui preparação/espera (`bin/codex_pergunta.sh:252`, `bin/codex_pergunta.sh:295`, `bin/codex_pergunta.sh:360`, `bin/codex_pergunta.sh:376`). Recusas anteriores ao `ini` ficam fora do anexo de encerradas (`bin/codex_pergunta.sh:234`, `bin/codex_pergunta.sh:240`, `bin/codex_pergunta.sh:258`).
- **Triagem**: escritor único `--triagem`, após conferir sidecar, papel e SHA (`bin/disjuntor.py:869`, `bin/disjuntor.py:892`, `bin/disjuntor.py:910`); leitor `_triagens`, consumido por `decide_b3` (`bin/disjuntor.py:921`, `bin/disjuntor.py:981`). Não registra classe, commit da cura nem custo de cada achado (`bin/disjuntor.py:911`).
- **Disjuntor**: escritor `trilha`; detalhe limitado a 300 caracteres e falha de escrita absorvida (`bin/disjuntor.py:193`, `bin/disjuntor.py:199`, `bin/disjuntor.py:200`). B3 anuncia após gravar estado (`bin/disjuntor.py:1043`); leitor do ESTADO agrega eventos e tipos de liberação (`bin/disjuntor.py:1971`, `bin/disjuntor.py:1979`, `bin/disjuntor.py:1999`). Liberação por texto válido não certifica cura (`bin/disjuntor.py:1025`, `bin/disjuntor.py:1031`).
- **Limite**: escritor wrapper (`bin/codex_pergunta.sh:434`, `bin/codex_pergunta.sh:439`); leitor conta linhas válidas (`bin/codex_limite_estado.py:27`, `bin/codex_limite_estado.py:41`). Oito recusas no anexo, 609 s = 10,15 min nas chamadas correspondentes; são limite do serviço, não timeout do construtor (`limite.log:1-8`; `chamadas_fim.tsv`, 05/11 15:05:08–15:05:46).
- **Gitlog anexo**: produtor da exportação e consumidor específico não identificados na cópia [nao verificado]. O código contém leitores de histórico como `tickets_placar.sh`, mas isso não identifica quem produziu este anexo (`bin/tickets_placar.sh:54`). Assunto truncado não prova conteúdo do patch [nao verificado].

A regra de preverif, descarte anexado, suite antes do final e retorno ao preverif após reprovação está em `CLAUDE.md:372-381`; a própria regra exige postmortem por achado e distingue falha do Codex, triagem, ausência de pergunta e falta de alcance (`CLAUDE.md:382-388`). [nao verificado]

_4096 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
