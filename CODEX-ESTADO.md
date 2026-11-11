# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 15:46** · publicado em **11/11/2026 15:46** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 15:46 · sessao 2985e584509f · - · - · origem=Ronald (tmux)

- **B2 curado nas duas portas.** A tradução exige `ValidationError` e `code == 'tipo_fora_do_catalogo'`; outros erros sobem integralmente. A edição usa `raise` para qualquer outro código. Exemplo de erro preservado: fim anterior ao início (`app/ponto/services/ausencia.py:135`, `app/ponto/services/ausencia.py:429`, `app/ponto/services/ausencia.py:523`, `app/ponto/models.py:926`).

- **Atomic/anexo:** criação executa `save` dentro do `atomic`; captura, limpeza e tradução acontecem depois de sair dele. A limpeza vem antes da tradução. Entretanto, a recusa real da guarda precede `super().save`, portanto não grava o upload; nome inalterado faz a limpeza retornar `False`. Não cabe afirmar que o anexo necessariamente “morre” nessa recusa. Arquivo novo com nome alterado é apagado; falha de exclusão é contida (`app/ponto/services/ausencia.py:423`, `app/ponto/services/ausencia.py:425`, `app/ponto/services/ausencia.py:428`, `app/ponto/services/ausencia.py:432`, `app/ponto/models.py:898`, `app/ponto/models.py:938`, `app/ponto/services/ausencia.py:122`, `app/ponto/services/ausencia.py:125`, `app/ponto/services/ausencia.py:127`).

- **Demais saves encontrados:** prazo após rejulgamento; mudança de estado; anexação; carimbos/período da decisão; documento, descrição e prazo da reabertura. Todos fornecem `update_fields` sem `tipo`, de modo que agora retornam antes da consulta ao catálogo. Não demonstram novo escape HTTP da recusa do catálogo (`app/ponto/services/ausencia.py:570`, `app/ponto/services/ausencia.py:853`, `app/ponto/services/ausencia.py:877`, `app/ponto/services/ausencia.py:945`, `app/ponto/services/ausencia.py:1148`, `app/ponto/services/ausencia.py:1158`, `app/ponto/services/ausencia.py:1166`, `app/ponto/models.py:881`).

- **Save completo adicional:** `rederivar_fim` continua podendo levantar a exceção crua se houver desacordo com o tipo persistido. Seu chamador encontrado é o comando `curar_ausencia_sem_fim`, sem captura dessa exceção; uso por porta HTTP não foi encontrado **[não verificado além da busca estática]** (`app/ponto/services/ausencia.py:1314`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:42`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:46`).

- **Admin e signals:** admin de Ausência proíbe adicionar, alterar e apagar. Receivers de Ausência encaminham o fato ao cartório e consumidor, com contenção; não atribuem `tipo` nem chamam `Ausencia.save` diretamente (`app/ponto/admin.py:38`, `app/core/admin.py:59`, `app/core/admin.py:62`, `app/core/admin.py:65`, `app/ponto/signals.py:143`, `app/ponto/signals.py:147`, `app/ponto/signals.py:88`, `app/ponto/signals.py:263`, `app/ponto/signals.py:275`).

- **`update_fields`:** nenhum chamador produtivo de `Ausencia.save` com argumentos posicionais apareceu na inspeção **[não verificado por execução]**. O override só consulta `kwargs`, portanto o formato posicional permaneceria um limite latente. `tipo` é `CharField`, sem alias de FK `tipo_id`. A atribuição produtiva encontrada ocorre na edição e é seguida de save completo, sem expectativa de gravar tipo por lista que o omite (`app/ponto/models.py:898`, `app/ponto/models.py:749`, `app/ponto/services/ausencia.py:510`, `app/ponto/services/ausencia.py:522`).

- **B3 parcialmente curado:** a recusa indevida de escrita parcial foi corrigida; leitura histórica e save completo continuam separados. A classificação da corrida como lost update genérico procede: o diff fornecido preserva o save completo anterior e acrescenta a guarda; ela não criou a possibilidade de sobrescrever uma correção concorrente. Portanto, a corrida continua possível, mas não sustenta sozinha bloqueante novo desta obra (`app/ponto/models.py:881`, `app/ponto/models.py:887`, `app/ponto/models.py:938`; preexistência de `save` completo: diff de CURA fornecido, **[sem arquivo:linha da base disponível]**).

- **Segundo escritor:** os sites encontrados permanecem na porta declarada; existe teste estrutural com allowlist vazia, mas sua execução está **[não verificado]** (`app/core/portas.py:486`, `app/ponto/tests/test_chokepoint_ausencia_gate.py:25`, `app/ponto/tests/test_chokepoint_ausencia_gate.py:53`).

## 11/11 15:46 · sessao e9e3f7e485d4 · - · - · origem=Ronald (tmux)

B1 **curado quanto ao isolamento entre schemas**: `_schema()` consulta `connection.schema_name` em cada leitura; `vigente()` procura/grava a tupla `(agora, v)` usando essa chave, de modo que trocar A→B→A seleciona caches separados (`app/ponto/catalogo/ausencias.py:265`, `app/ponto/catalogo/ausencias.py:284`, `app/ponto/catalogo/ausencias.py:289`). `None` é exclusivamente a ausência desse atributo; um backend tenant-aware em `public` usa a chave literal `'public'` (`app/ponto/catalogo/ausencias.py:267`). O comentário que aproxima “control-plane” de `None` é impreciso, mas a implementação distingue corretamente (`app/ponto/catalogo/ausencias.py:260`, `app/config/settings/saas.py:85`).

Não encontrei acesso externo ao `_CACHE` nem dependência do formato antigo ou do compartilhamento entre tenants na cópia examinada; os acessos efetivos estão somente na definição, limpeza, busca e gravação do próprio módulo (`app/ponto/catalogo/ausencias.py:261`, `app/ponto/catalogo/ausencias.py:272`, `app/ponto/catalogo/ausencias.py:285`, `app/ponto/catalogo/ausencias.py:289`). O único chamador **produtivo** de `invalidar_cache()` encontrado é o receiver de `post_save`/`post_delete` de `TipoAusencia`; os demais chamadores encontrados são testes (`app/ponto/models.py:1265`, `app/ponto/models.py:1268`; exemplos `app/ponto/tests/test_cadastro_tipo.py:12`, `app/ponto/tests/test_lastro_folga_save_guarda.py:46`). A UI chega ao signal através de `gravar_tipo_ausencia`, que usa `save()`/`objects.create()` (`app/ponto/views_tipos_ausencia.py:86`, `app/ponto/views_tipos_ausencia.py:94`, `app/ponto/services/ausencia.py:1326`, `app/ponto/services/ausencia.py:1328`).

Há mudança de schema no mesmo comando: `para_cada_tenant()` itera clientes com `schema_context(cliente.schema_name)`; isso agora seleciona uma entrada por schema em vez de depender de compartilhamento (`app/tenants/cron.py:19`, `app/tenants/cron.py:25`, `app/ponto/catalogo/ausencias.py:284`). Há mudança dentro da chamada de provisionamento do control-plane: a view chama `provision_tenant`, que chama bootstrap dentro de `tenant_context(cliente)` (`app/tenants/views.py:52`, `app/tenants/services.py:13`, `app/tenants/bootstrap.py:19`). Não há leitor de catálogo no próprio bootstrap ou nas views de tenants examinadas; `ponto` é app de tenant, e o backend usa `default` tenant-aware (`app/tenants/bootstrap.py:19`, `app/tenants/bootstrap.py:26`, `app/tenants/views.py:25`, `app/config/settings/saas.py:34`, `app/config/settings/saas.py:40`, `app/config/settings/saas.py:83`). Integração real do pacote `django-tenants`/`tenant_command` nesta execução: **[não verificado]**; o teste usa mocks de `schema_name` e `_do_banco`, não schemas reais (`app/ponto/tests/test_lastro_folga_save_guarda.py:161`, `app/ponto/tests/test_lastro_folga_save_guarda.py:164`).

Leitores encontrados das respostas cacheadas:

| API/conjunto | Consumidores produtivos e referências |
|---|---|
| `vigente` direto | Declaração de texto; atestados em lote (`app/chamados/services/declaracao_texto.py:179`, `app/ponto/services/atestados_lote.py:28`). |
| `codigos_validos` | Form, guarda do modelo, prévia, criação, edição, composição de exigência de documento, vigia e PWA (`app/ponto/forms.py:63`, `app/ponto/models.py:884`, `app/ponto/services/checar_ausencia.py:23`, `app/ponto/services/ausencia.py:202`, `app/ponto/services/ausencia.py:486`, `app/ponto/services/ausencia.py:822`, `app/ponto/services/vigia_ausencia.py:103`, `app/ponto/views.py:2509`). |
| `efeito_vigente` | Propriedade do modelo, dia decidido, status, serviço de ausência e comando diagnóstico (`app/ponto/models.py:842`, `app/ponto/services/dia_decidido.py:468`, `app/ponto/status_dia.py:311`, `app/ponto/services/ausencia.py:629`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:49`). |
| `rotulo_vigente` e indireto por `rotulo_curto` | Prévia, portas, PWA, dia decidido e calendário; `rotulo_curto` consulta o vivo quando não há rótulo estático (`app/ponto/services/checar_ausencia.py:40`, `app/ponto/services/ausencia.py:445`, `app/ponto/services/ausencia.py:505`, `app/ponto/views.py:2550`, `app/ponto/services/dia_decidido.py:215`, `app/ponto/services/dia_decidido.py:216`, `app/colaboradores/services/calendario.py:675`, `app/ponto/catalogo/ausencias.py:175`). |
| `choices_vigentes` / agrupadas | API, intake, drawer, views de colaboradores, telas de ausência e relatórios (`app/api/views.py:1864`, `app/core/intake.py:369`, `app/colaboradores/services/drawer.py:138`, `app/colaboradores/views.py:1959`, `app/ponto/views.py:2085`, `app/ponto/views.py:2248`, `app/ponto/views.py:2462`, `app/ponto/views.py:2561`, `app/relatorios/views.py:242`). |
| `parciais_vigentes` | Portas/telas/API, turnos, escala, materialização, fechamento, crédito e vigias (`app/ponto/services/ausencia.py:214`, `app/ponto/services/ausencia.py:492`, `app/ponto/views.py:2086`, `app/ponto/views.py:2237`, `app/api/views.py:1881`, `app/ponto/turnos.py:2599`, `app/ponto/turnos.py:2687`, `app/escala/utils.py:1187`, `app/chamados/services/materializacao.py:81`, `app/ponto/services/fechamento.py:692`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/services/efeito_ausencia.py:38`, `app/ponto/services/vigia_ausencia_veredito.py:49`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:37`). |
| `abonam` / `descontam` | Exportação, inteligência, precedência e contadores/telas (`app/folha/export.py:140`, `app/folha/export.py:181`, `app/folha/export.py:198`, `app/inteligencia/resumo.py:43`, `app/inteligencia/resumo.py:44`, `app/inteligencia/management/commands/calcular_scores.py:14`, `app/inteligencia/management/commands/gerar_alertas.py:58`, `app/ponto/precedencia.py:131`, `app/ponto/views.py:2198`, `app/ponto/views.py:2200`, `app/ponto/views.py:2872`, `app/relatorios/views.py:276`). |
| Proxies `TIPOS_*` / `EM_ABERTO_VIVOS` | Todos recalculam pelas funções em cada operação do proxy, inclusive os aliases do modelo (`app/ponto/catalogo/ausencias.py:412`, `app/ponto/catalogo/ausencias.py:415`, `app/ponto/catalogo/ausencias.py:418`, `app/ponto/catalogo/ausencias.py:464`, `app/ponto/catalogo/ausencias.py:476`, `app/ponto/models.py:683`, `app/ponto/models.py:689`). Exemplos consumidores diretos: documentos, alertas, bloqueios, vigia e filtros (`app/ponto/services/ausencia.py:646`, `app/ponto/services/ausencia.py:813`, `app/ponto/services/alertas_ausencia.py:50`, `app/ponto/services/aviso_da_batida.py:79`, `app/ponto/services/vigia_ausencia.py:95`, `app/ponto/views.py:2201`, `app/ponto/views.py:2222`, `app/relatorios/views.py:282`). |

Os motivos de triagem **procedem para TTL/fallback**: continuam TTL 60 por `monotonic`, invalidação local e fallback após vazio/exceção; a c2 muda a organização do cache, não esses critérios (`app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:252`, `app/ponto/catalogo/ausencias.py:253`, `app/ponto/catalogo/ausencias.py:262`, `app/ponto/catalogo/ausencias.py:272`, `app/ponto/catalogo/ausencias.py:283`, `app/ponto/catalogo/ausencias.py:288`). Preexistência está também no diff fornecido pelo usuário; prova histórica por Git indisponível nesta cópia **[não verificado]**. Esses comportamentos já alimentam as portas, que consultam a mesma autoridade (`app/ponto/services/ausencia.py:202`, `app/ponto/services/ausencia.py:486`).

Limite: o teste de cache morde a regressão da chave entre A e B; não testa retorno a A, chave public/None, invalidação de todos os schemas ou expiração TTL, nem execução concorrente real (`app/ponto/tests/test_lastro_folga_save_guarda.py:157`, `app/ponto/tests/test_lastro_folga_save_guarda.py:166`).

## 11/11 15:45 · sessao 9bbf5b5e2f97 · - · - · origem=Ronald (tmux)

Os cinco testes novos têm mordida lógica, com limites distintos: [nao verificado]

- **Cache por schema:** carrega A e, sem invalidar, exige catálogo B; o cache antigo global devolveria A e reprovaria. Não exercita retorno B→A, schema `public`, expiração, invalidação nem troca real de schema no banco; usa mocks de `schema_name` e `_do_banco` (`app/ponto/tests/test_lastro_folga_save_guarda.py:157-167`).
- **Tradução na criação:** força recusa da guarda real chamada por `save`, exige `AusenciaInvalida.codigo == 'tipo_invalido'`, falha se escapar `ValidationError` e exige ausência de linha persistida. Remover a tradução reprovaria (`app/ponto/tests/test_lastro_folga_save_guarda.py:169-187`).
- **Tradução na edição:** força a mesma recusa e verifica classe/código da tradução. Remover a tradução reprovaria; não verifica estado persistido ou restauração da instância após recusa (`app/ponto/tests/test_lastro_folga_save_guarda.py:189-201`).
- **`update_fields` sem tipo:** põe tipo morto somente na memória, salva descrição e exige tipo antigo preservado no banco. A guarda c1 reprovaria porque compararia morto em memória com `abono` persistido. O teste prova ausência de veto indevido; não prova literalmente “não consulta catálogo”, pois não conta consultas nem intercepta `codigos_validos` (`app/ponto/tests/test_lastro_folga_save_guarda.py:203-213`; `app/ponto/models.py:881-889`).
- **`update_fields` com tipo:** exige recusa e manutenção de `abono`; remover toda a guarda reprovaria. Aceita qualquer `ValidationError`, sem verificar o código da recusa (`app/ponto/tests/test_lastro_folga_save_guarda.py:215-221`).

Os testes de tradução mockam a guarda para produzir o código específico; não exercitam desativação concorrente real do catálogo, propagação de outro `ValidationError`, limpeza de anexo nem limites transacionais. Esses cenários não estão nos cinco corpos (`app/ponto/tests/test_lastro_folga_save_guarda.py:157-221`). RED executado, GREEN, mutantes executados e suite inteira desta obra: **[não verificado]**. A exigência de evidência RED e suite anterior ao opus está expressa (`CLAUDE.md:28`, `CLAUDE.md:193`, `CLAUDE.md:377-381`).

Sobre as classes:

- **Segundo juiz:** a guarda consulta `codigos_validos`, mesma função das duas portas; não inventa uma lista de códigos. O registro agrupa duração/validade em `fim_de`, portanto não convém afirmar que `codigos_validos` tem registro nominal próprio sem ressalva (`app/ponto/models.py:883-884`; `app/ponto/services/ausencia.py:201-203`, `app/ponto/services/ausencia.py:485-487`; `app/core/juizes.py:269`).
- **Relógio fora da borda:** a cura usa o TTL já centralizado na autoridade; relógio `monotonic()` e 60 segundos estão no catálogo. A guarda não acrescenta relógio próprio (`app/ponto/catalogo/ausencias.py:262`, `app/ponto/catalogo/ausencias.py:282-289`; `app/ponto/models.py:881-894`).
- **Dois significados/listas:** `tipo` conserva choices congeladas, mas a criação exclui esse campo de `full_clean`, e o teste exige admissão de código vivo ausente das choices. Divergência preservada, sem nova autoridade na guarda (`app/ponto/models.py:661-679`, `app/ponto/models.py:749`; `app/ponto/services/ausencia.py:393-395`; `app/ponto/tests/test_lastro_folga_save_guarda.py:134-142`).
- **Pré-requisito:** BACKLOG exige medir escritor/frota antes da cura, mas há registro posterior de censo e sonda com rc0 e conferência; não cabe concluir que nunca foram medidos apenas pela linha ainda aberta (`app/docs/BACKLOG.md:1807`; `app/docs/RELATO.md:7367-7371`). Artefatos originais citados, execução atual e escritor histórico exato: **[não verificado]**.
- **Premissa documental conflitante:** o teste diz que `folga` nunca esteve no catálogo vivo de produção, mas relato posterior diz que estava cadastrada e distingue `fora_choices=127` de `fora_catalogo=6`. O teste continua válido como construção local porque verifica explicitamente que `folga` está fora do catálogo da fixture; a alegação histórica da docstring não está sustentada e conflita com a cópia (`app/ponto/tests/test_lastro_folga_save_guarda.py:3-6`, `app/ponto/tests/test_lastro_folga_save_guarda.py:44-49`; `app/docs/RELATO.md:7368-7370`).

A aprovação técnica desta pré-verificação não certifica pouso: suite inteira verde e opus final continuam requisitos explícitos, com cumprimento nesta obra **[não verificado]** (`CLAUDE.md:377-381`). [nao verificado]

## 11/11 15:44 · sessao 0459ff023fc9 · FIO-E0-V4-E4B · censo · origem=Code

| Identificação e critério | Resultado |
|---|---|
| Árvore solicitada | `fb5b4be463695cf4c5f38d31aa5c044c2b7fb7a7`, cópia c8; identidade Git independente **[nao verificado]**. |
| Unidade de contagem | **Callsite**, não quantidade de execuções. Censo por `rg` e AST, resolvendo imports diretos, aliases de módulo e import relativo; execução em memória, sem relatório persistido **[nao verificado]**. Exemplos de aliases efetivamente lidos: `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:157`; `app/pautas/views.py:12`. |
| Resultado principal | Existem **dois ramos produtivos** que podem tomar DP antes das âncoras: cadastro sem turno e limbo exportado, durante a transição anterior ao agrupador. (`app/chamados/services/fecho_runtime.py:151`; `app/chamados/services/fecho_runtime.py:154`; `app/chamados/services/fabrica_da_celula.py:204`; `app/chamados/services/fabrica_da_celula.py:215`) |
| Limite da conclusão | Alcance pelo código é distinto de ocorrência: transações concorrentes observadas, dados que satisfaçam simultaneamente os guards e deadlock efetivamente ocorrido **[nao verificado]**. |

| Notação usada nas sequências | Recurso e aquisição |
|---|---|
| **C → D → Ch → Q** | Colaborador → DisputaSupervisao → ChamadoColaborador → PerguntaDisputa; cada conjunto explicitamente solicitado é travado por PK crescente. (`app/chamados/models.py:2666`; `app/chamados/models.py:2675`; `app/chamados/models.py:2688`; `app/chamados/models.py:2692`) |
| **A\*** | Todas as âncoras E4B do lote, ordenadas pelo texto da âncora; advisory transacional. (`app/chamados/services/fecho_runtime.py:86`; `app/chamados/services/fecho_runtime.py:89`; `app/chamados/services/fecho_runtime.py:120`) |
| **P → DP** | INSERT Pauta → trilha → obtenção/criação do ContadorPauta → UPDATE do contador DP. A porta `escrever` é atômica. (`app/pautas/services.py:152`; `app/pautas/services.py:180`; `app/pautas/services.py:186`; `app/pautas/services.py:188`; `app/pautas/services.py:352`; `app/pautas/services.py:354`) |
| **FM / PA / Ag / Au / J / HE / Cel / TM** | FechamentoMensal / PeriodoAquisitivo / AgendamentoFerias / Ausencia / Justificativa / DecisaoHE / CelulaDia / TurnoMaterializado. Aquisições correspondentes nas matrizes seguintes. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`; `app/ferias/services.py:364`; `app/ferias/services.py:367`; `app/ponto/portas/celula.py:657`; `app/ponto/services/justificativa.py:142`; `app/ponto/portas/he.py:174`; `app/ponto/portas/celula.py:593`; `app/ponto/nucleo.py:81`) |

| Aquisição explícita de linha | Quem toma, ordem e função que alcança Pauta |
|---|---|
| **C — `select_for_update`** (`app/chamados/models.py:2666`) | `PerguntaDisputa.trava_na_ordem`: primeiro grupo; `_gravar_estado` chama antes do save do chamado, que dispara os receivers. (`app/chamados/models.py:2629`; `app/chamados/models.py:290`; `app/chamados/models.py:296`) |
| **D — `select_for_update`** (`app/chamados/models.py:2675`) | Segundo grupo da mesma porta; com `abertas_do_colaborador=True`, inclui todas as disputas abertas daquele colaborador. (`app/chamados/models.py:2670`; `app/chamados/models.py:2673`; `app/chamados/models.py:290`) |
| **Ch — `select_for_update`** (`app/chamados/models.py:2688`) | Terceiro grupo: chamados pedidos, pais das disputas e, no universo do colaborador, chamados vivos. (`app/chamados/models.py:2679`; `app/chamados/models.py:2683`) |
| **Q — `select_for_update(of=('self',))`** (`app/chamados/models.py:2692`) | Último grupo, somente quando IDs de perguntas são fornecidos; não significa que toda escrita implícita de Q tenha passado por esse SELECT. (`app/chamados/models.py:2691`; `app/chamados/models.py:3152`; `app/chamados/models.py:3423`) |
| **FM — `select_for_update`** (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`) | Command pago/devido: atomic → C → FM → P cabeça → DP → P filhas → DP. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:189`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:211`) |
| **J — quatro sites** (`app/ponto/services/justificativa.py:142`; `app/ponto/services/justificativa.py:277`; `app/ponto/services/justificativa.py:344`; `app/ponto/views.py:1794`) | Aprovar, rejeitar, materializar saída e aprovação em lote pedem C/D/Ch **antes** de J. Save de J pode chamar cartório/fábrica/DP. (`app/ponto/services/justificativa.py:141`; `app/ponto/services/justificativa.py:276`; `app/ponto/services/justificativa.py:343`; `app/ponto/views.py:1793`; `app/ponto/signals.py:165`) |
| **Ag → PA — dois sites** (`app/ferias/services.py:364`; `app/ferias/services.py:367`) | `_travar_agendamento`, dentro dos atomics de cancelar DP, cancelar admin e trocar janela; depois vêm alterações de Ausencia, sync/cartório e eventual Pauta DP. (`app/ferias/services.py:379`; `app/ferias/services.py:402`; `app/ferias/services.py:433`; `app/ferias/services.py:450`; `app/ferias/models.py:451`; `app/ponto/services/ausencia.py:1245`) |
| **Ag — alinhamento de status** (`app/ferias/services.py:600`) | Atomic → Ag → save do status → sync de Ausencia → cartório ou Pauta de recusa. (`app/ferias/services.py:599`; `app/ferias/services.py:604`; `app/ferias/models.py:451`; `app/ponto/services/ausencia.py:1216`) |
| **SolicitacaoFerias** (`app/ferias/views.py:740`) | Atomic da view → solicitação → aprovação → save de novo Ag → sync/Ausencia/possível DP → UPDATE PA. (`app/ferias/views.py:739`; `app/ferias/views.py:753`; `app/ferias/services.py:503`; `app/ferias/services.py:506`; `app/ferias/models.py:451`) |
| **HE — dois sites** (`app/ponto/portas/he.py:174`; `app/ponto/portas/he.py:555`) | Atomic da prévia/confirmação → itens na ordem recebida → HE do dia → `decidir_he`, com reaquisição da mesma linha → eventual Pauta DP. O lote aceita um colaborador. (`app/ponto/services/autorizacao_he_periodo.py:321`; `app/ponto/services/autorizacao_he_periodo.py:343`; `app/ponto/portas/he.py:534`; `app/ponto/portas/he.py:536`; `app/ponto/portas/he.py:546`; `app/ponto/portas/he.py:562`; `app/ponto/services/autorizacao_he_periodo.py:300`) |
| **Au — rederivação H06** (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:138`) | Atomic por linha → Au → edição/save → signal da ausência → cartório/fábrica/DP. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`; `app/ponto/services/ausencia.py:509`; `app/ponto/signals.py:147`) |
| **Cel — editar dia** (`app/ponto/portas/celula.py:593`) | Atomic → Cel → marcação de cobranças órfãs → eventual C/Ch. Cartório vem depois do atomic local; continua no mesmo atomic quando há o exterior da troca. (`app/ponto/portas/celula.py:592`; `app/ponto/portas/celula.py:626`; `app/ponto/management/commands/gerar_celulas.py:255`; `app/chamados/services/contexto.py:103`; `app/chamados/models.py:486`; `app/ponto/portas/celula.py:627`; `app/ponto/portas/celula.py:652`) |
| **Au — trocar plantão** (`app/ponto/portas/celula.py:657`) | Atomic exterior → cartas Au por PK → edições por colaborador/data → Cel → eventual C/Ch → julgamento/fábrica/DP. Chamadores externos encontrados em testes; chamador produtivo **[nao verificado]**. (`app/ponto/portas/celula.py:652`; `app/ponto/portas/celula.py:658`; `app/ponto/portas/celula.py:673`; `app/ponto/portas/celula.py:675`; `app/ponto/tests/test_o10b_editar_dia_troca.py:116`) |

| Advisory | Ordem, duração e alcance |
|---|---|
| **Âncora E4B: bloqueante, transacional** (`app/chamados/services/fecho_runtime.py:89`) | SHA256 de marcador/tipo/âncora; atomic do agrupador → todas A\* → dedup → escrita. Quando aninhado no adaptador, integra o ato exterior. (`app/chamados/services/fecho_runtime.py:86`; `app/chamados/services/fecho_runtime.py:119`; `app/chamados/services/fecho_runtime.py:120`; `app/chamados/services/fecho_runtime.py:123`; `app/chamados/services/fecho_runtime.py:147`; `app/chamados/services/fecho_runtime.py:154`) |
| **Vigia de hora: sessão, tentativa sem espera** (`app/ponto/services/vigia_de_hora.py:99`) | Chave por empresa → se não obtém, retorna → julgamentos → unlock. Não há atomic abrangendo todo esse laço; não constitui espera por advisory no ciclo. (`app/ponto/services/vigia_de_hora.py:97`; `app/ponto/services/vigia_de_hora.py:100`; `app/ponto/services/vigia_de_hora.py:131`; `app/ponto/services/vigia_de_hora.py:136`) |
| **Consumidor: sessão, tentativa sem espera** (`app/chamados/reconciliador.py:1235`) | Chave derivada do schema; atos do consumidor separados, sem atomic geral por colaborador; unlock ao sair do contexto. (`app/chamados/reconciliador.py:1221`; `app/chamados/reconciliador.py:1242`; `app/chamados/reconciliador.py:1364`; `app/chamados/reconciliador.py:1368`; `app/chamados/reconciliador.py:1392`) |

| Escrita que também adquire linha | Posição na sequência |
|---|---|
| **ContadorPauta** (`app/pautas/services.py:352`; `app/pautas/services.py:354`) | A aquisição relevante não é `select_for_update`: é criação concorrente, quando ausente, ou UPDATE da linha do alvo. DP é único por departamento. (`app/pautas/models.py:167`; `app/pautas/services.py:352`; `app/pautas/services.py:354`) |
| **Chamado** (`app/chamados/models.py:296`) | `_gravar_estado` toma C/D/Ch, registra trilha e salva; o save executa os receivers antes da relavra S2. (`app/chamados/models.py:290`; `app/chamados/models.py:292`; `app/chamados/models.py:296`; `app/chamados/models.py:307`) |
| **Pergunta** (`app/chamados/models.py:3152`; `app/chamados/veredito_lavrado.py:275`) | S143 pode carimbar Q antes do cartório; S2 pode atualizar Q **depois** da Pauta da fábrica. `carimbar`/`save` não demonstram, por si, aquisição prévia de C. (`app/chamados/services/disputa_emissao.py:1984`; `app/chamados/models.py:305`; `app/chamados/models.py:312`; `app/chamados/models.py:3422`; `app/chamados/models.py:3423`) |
| **Disputa** (`app/chamados/models.py:1936`) | Porta de fecho toma a ordem antes do save; o receiver S88 pode reconciliar o chamado-pai e o S144 pode materializar pendentes. (`app/chamados/models.py:1910`; `app/chamados/signals.py:42`; `app/chamados/signals.py:164`) |
| **CelulaDia** (`app/ponto/portas/celula.py:905`) | No cartório: lavra em memória → `carimbar` → atomic/save Cel → EventoCelula; fábrica vem depois. (`app/ponto/services/cartorio.py:1825`; `app/ponto/services/cartorio.py:1845`; `app/ponto/portas/celula.py:1072`; `app/ponto/portas/celula.py:904`; `app/ponto/portas/celula.py:907`; `app/ponto/services/cartorio.py:1020`) |
| **TM** (`app/ponto/nucleo.py:81`) | Dentro de atomic: DELETE do recorte → bulk INSERT. No save de Batida, o receiver do núcleo precede o cartório, podendo conservar TM antes de posterior C/DP quando há atomic exterior. (`app/ponto/nucleo.py:57`; `app/ponto/nucleo.py:58`; `app/ponto/nucleo.py:59`; `app/ponto/signals.py:36`; `app/ponto/signals.py:135`) |
| **FM: invalidação síncrona** (`app/ponto/services/fechamento.py:1656`) | Aprovar/materializar justificativa pode passar por cartório/DP e depois invalidar FM no mesmo atomic. Isso é distinto do recálculo por evento em `on_commit`. (`app/ponto/services/justificativa.py:208`; `app/ponto/services/justificativa.py:215`; `app/ponto/services/justificativa.py:386`; `app/ponto/services/justificativa.py:390`; `app/ponto/services/fechamento.py:1824`) |
| **FM e DiaPago: passivo** (`app/ponto/portas/regen_exportada.py:179`; `app/ponto/portas/regen_exportada.py:188`) | Atomic do par → plantio → regeneração, que pode julgar/escrever DP → recálculo FM → retrato DiaPago → Pauta final do passivo. (`app/chamados/services/passivo_validado.py:371`; `app/chamados/services/passivo_validado.py:428`; `app/chamados/services/passivo_validado.py:438`; `app/ponto/portas/celula.py:434`; `app/chamados/services/passivo_validado.py:443`) |
| **PA: agendamento novo/histórico** (`app/ferias/services.py:302`; `app/ferias/services.py:326`) | Save de novo Ag → sync de Ausencia → eventual DP → UPDATE PA, ainda dentro do atomic. (`app/ferias/services.py:277`; `app/ferias/services.py:298`; `app/ferias/services.py:315`; `app/ferias/services.py:321`; `app/ferias/models.py:451`; `app/ponto/services/ausencia.py:1245`) |
| **Pauta existente** (`app/pautas/services.py:215`; `app/pautas/services.py:247`; `app/pautas/services.py:272`; `app/pautas/services.py:294`) | Ler/fazer/fazer pelo sistema/cancelar: UPDATE Pauta → trilha → contador destinatário. (`app/pautas/services.py:218`; `app/pautas/services.py:251`; `app/pautas/services.py:276`; `app/pautas/services.py:297`) |
| **Au: alterações e exclusões** (`app/ponto/services/ausencia.py:835`; `app/ponto/services/ausencia.py:1190`; `app/ponto/services/ausencia.py:1281`) | Save/delete podem alcançar cartório pelos respectivos signals. O UPDATE em massa de rejeição de férias não emite `post_save`; o save posterior de Ag pode excluir Au e emitir `post_delete`. (`app/ponto/signals.py:147`; `app/ponto/signals.py:152`; `app/ferias/services.py:417`; `app/ferias/models.py:451`) |

| Sites de `select_for_update` separados do alcance DP no mesmo atomic lido | Motivo da separação |
|---|---|
| HE, recusar ponta/revogar — dois sites (`app/ponto/portas/he.py:318`; `app/ponto/portas/he.py:445`) | Chamadores próprios encontrados; não percorrem a Pauta do ato de período. (`app/ponto/management/commands/recusar_ponta_pequena.py:170`; `app/ponto/management/commands/lavrar_he_pendente.py:59`) |
| AplicacaoConvencao — um site (`app/core/services/aplicacao_convencao.py:115`) | Atomic lido grava cadastro/trilha e retorna; encadeamento adicional até Pauta **[nao verificado]**. (`app/core/services/aplicacao_convencao.py:100`; `app/core/services/aplicacao_convencao.py:124`; `app/core/services/aplicacao_convencao.py:134`) |
| Lote de holerite — dois sites (`app/holerite/management/commands/processar_lote_holerite.py:54`; `app/holerite/management/commands/processar_lote_holerite.py:124`) | Atomics de estado do lote; motor chamado fora deles. Alcance DP dentro dessas aquisições **[nao verificado]**. (`app/holerite/management/commands/processar_lote_holerite.py:89`) |

| Sequência da transição | Até a saída do atomic |
|---|---|
| **E — ramo com agrupador E4B** (`app/chamados/services/fecho_runtime.py:147`) | Atomic exterior → `transicionar` → atomic de `_gravar_estado` → C/D/Ch → UPDATE Ch → receivers abaixo → eventual S2/Q → retorno da transição → A\* → dedup → P/DP → saída do atomic exterior. (`app/chamados/services/fecho_runtime.py:151`; `app/chamados/models.py:280`; `app/chamados/models.py:290`; `app/chamados/models.py:296`; `app/chamados/models.py:307`; `app/chamados/services/fecho_runtime.py:154`; `app/chamados/services/fecho_runtime.py:120`; `app/chamados/services/fecho_runtime.py:127`) |
| **N — passagem sem agrupador** (`app/chamados/services/fecho_runtime.py:144`) | `transicionar` → atomic de `_gravar_estado` → C/D/Ch → UPDATE Ch → receivers → eventual S2/Q → saída; não adquire A por esse adaptador. Pode escrever DP pelo cartório. (`app/chamados/services/fecho_runtime.py:145`; `app/chamados/models.py:280`; `app/chamados/models.py:290`; `app/chamados/models.py:296`; `app/chamados/models.py:307`; `app/chamados/signals.py:217`) |
| **DP anterior às âncoras, em E** (`app/chamados/services/fecho_runtime.py:151`) | Possível se o cartório chegar a cadastro sem turno ou limbo exportado: P/DP ocorre durante o save, antes do retorno que chama A\*. (`app/chamados/signals.py:217`; `app/ponto/services/cartorio.py:1020`; `app/chamados/services/fabrica_da_celula.py:204`; `app/chamados/services/fabrica_da_celula.py:215`; `app/chamados/services/fecho_runtime.py:154`) |
| **Condições para aquela emissão** (`app/chamados/services/fabrica_da_celula.py:201`) | Cadastro: célula passada, colaborador ativo, dia não dispensado e teto sem turno; limbo: motivos, `trabalha=None` e competência entregue. Se dedup já cobre o caso, não há nova Pauta. (`app/chamados/services/fabrica_da_celula.py:203`; `app/chamados/services/fabrica_da_celula.py:228`; `app/chamados/services/fabrica_da_celula.py:231`; `app/chamados/services/fabrica_da_celula.py:277`; `app/chamados/services/fabrica_da_celula.py:367`) |
| **Alcance das 20 arestas humanas abaixo** (`app/chamados/catalogo/motor.py:139`) | Dois callsites produtivos: detalhe e fio; dependem de permissão e guards do modelo. Exercício real de cada aresta **[nao verificado]**. (`app/chamados/services/acoes_chamado.py:313`; `app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:714`; `app/chamados/services/fio.py:722`; `app/chamados/models.py:132`; `app/chamados/models.py:148`; `app/chamados/models.py:165`; `app/chamados/models.py:181`) |

| De ↓ / para → | aberto | em_analise | resolvido | fechado | registrado |
|---|---|---|---|---|---|
| **aberto** (`app/chamados/catalogo/motor.py:100`) | Recusa: igual (`app/chamados/catalogo/motor.py:148`) | **N**, comum (`app/chamados/catalogo/motor.py:100`) | **E**, comum (`app/chamados/catalogo/motor.py:100`) | **E**, comum (`app/chamados/catalogo/motor.py:100`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:153`) |
| **em_analise** (`app/chamados/catalogo/motor.py:101`) | **N**, comum (`app/chamados/catalogo/motor.py:101`) | Recusa: igual (`app/chamados/catalogo/motor.py:148`) | **E**, comum (`app/chamados/catalogo/motor.py:101`) | **E**, comum (`app/chamados/catalogo/motor.py:101`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:153`) |
| **resolvido** (`app/chamados/catalogo/motor.py:102`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:153`) | **N**, comum (`app/chamados/catalogo/motor.py:102`) | Recusa: igual (`app/chamados/catalogo/motor.py:148`) | **N**, comum (`app/chamados/catalogo/motor.py:102`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:153`) |
| **fechado** (`app/chamados/catalogo/motor.py:103`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:150`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:150`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:150`) | Recusa: igual (`app/chamados/catalogo/motor.py:148`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:150`) |
| **registrado** (`app/chamados/catalogo/motor.py:105`) | **N**, admin_ti (`app/chamados/catalogo/motor.py:153`) | **N**, comum (`app/chamados/catalogo/motor.py:105`) | **E**, admin_ti (`app/chamados/catalogo/motor.py:153`) | **E**, comum (`app/chamados/catalogo/motor.py:105`) | Recusa: igual (`app/chamados/catalogo/motor.py:148`) |

| Complemento da matriz humana | Resultado |
|---|---|
| `cancelado` e `superado` como origem ou destino humano | Recusados, inclusive para admin_ti. (`app/chamados/catalogo/motor.py:142`; `app/chamados/catalogo/motor.py:145`) |
| Total humano | **20 arestas**: 10 da tabela comum e outras 10 liberadas a admin_ti; **6 E e 14 N** pela condição VIVOS→ENCERRADOS. (`app/chamados/catalogo/motor.py:99`; `app/chamados/catalogo/motor.py:153`; `app/chamados/services/fecho_runtime.py:144`) |
| Relavra S2 | Ocorre quando cruza a fronteira ENCERRADOS e existe alvo; portanto não é um passo incondicional de todas as 20 arestas. (`app/chamados/models.py:273`; `app/chamados/models.py:303`; `app/chamados/models.py:305`) |

| Receiver/subcaminho síncrono | Sequência adicional dentro do ato |
|---|---|
| **Rebaixamento de urgência** (`app/chamados/signals.py:54`) | Destino encerrado → ritual de rebaixamento; não adquire âncora E4B. (`app/chamados/signals.py:70`; `app/chamados/signals.py:72`) |
| **S143** (`app/chamados/signals.py:91`) | Somente save de status encerrado → savepoint → resolver retoma C/D/Ch → tenta devolver alcance antes de carimbar → eventuais Q, reabertura de pai e fecho de D. (`app/chamados/signals.py:96`; `app/chamados/signals.py:104`; `app/chamados/signals.py:115`; `app/chamados/services/disputa_emissao.py:1878`; `app/chamados/services/disputa_emissao.py:1945`; `app/chamados/services/disputa_emissao.py:1984`; `app/chamado

[truncado em 20000 de 48666 caracteres]

## 11/11 15:42 · sessao 4a33ac90e100 · DATA-EVENTO-CRU · verificador · origem=Code

**Veredito: BLOQUEANTES.** Os casos específicos da volta 1 foram corrigidos, mas ainda há destinos de escrita contabilizados como leitores. (`app/chamados/tests/test_catraca_data_evento_cru.py:57`; `app/chamados/tests/test_catraca_data_evento_cru.py:127`; `app/chamados/services/fio.py:801`)

1. **Censo confere:** recalculei em memória **216 ocorrências em 120 escopos: 211 Python + 5 templates**, sem diferença para o JSON. `vigia_plantio_overnight` entrou com **3**; `fio.py::<modulo>` entrou com **2**, pelas constantes guardadas nas linhas 1373/1378. (`app/chamados/tests/test_catraca_data_evento_cru.py:152`; `app/chamados/tests/catraca_data_evento_cru.json:2`; `app/chamados/tests/catraca_data_evento_cru.json:119`; `app/chamados/tests/catraca_data_evento_cru.json:116`; `app/chamados/tests/catraca_data_evento_cru.json:68`; `app/chamados/services/fio.py:1373`; `app/chamados/services/fio.py:1378`)

2. **As exclusões pedidas funcionam:** `PerguntaDisputa.Meta`, `abrir` e `abrir_conversa` saíram. As portas `abrir`/`abrir_conversa`/`_sob_contrato`/`_nascer` suprimem o destino; `UniqueConstraint` suprime a subárvore inteira, incluindo os dois `Q`. As três leituras reais dentro de `_nascer` permanecem. (`app/chamados/tests/test_catraca_data_evento_cru.py:57`; `app/chamados/tests/test_catraca_data_evento_cru.py:120`; `app/chamados/tests/test_catraca_data_evento_cru.py:122`; `app/chamados/models.py:3486`; `app/chamados/models.py:3492`; `app/chamados/models.py:2940`; `app/chamados/models.py:2956`; `app/chamados/models.py:2962`)

3. **Bloqueante — escrita ainda conta em pelo menos cinco ocorrências:**

   - `gravar_dia_do_fato` conta **1** pelo `return 'data_evento'`; esse retorno nomeia o campo de `update_fields`, após a atribuição. (`app/chamados/tests/catraca_data_evento_cru.json:79`; `app/chamados/services/fio.py:800`; `app/chamados/services/fio.py:801`; `app/chamados/services/fio.py:1037`; `app/chamados/services/fio.py:1043`)
   - Quatro repasses `reapontar_resolvedora(..., data_evento=...)` continuam contando porque essa porta falta em `ESCRITA`. São pedidos de escrita; a porta atribui por `gravar_dia_do_fato` e salva. (`app/chamados/services/declaracao_texto.py:370`; `app/chamados/reconciliador.py:954`; `app/chamados/reconciliador.py:1642`; `app/chamados/reconciliador.py:1655`; `app/chamados/tests/test_catraca_data_evento_cru.py:57`; `app/chamados/tests/test_catraca_data_evento_cru.py:127`; `app/chamados/services/fio.py:879`; `app/chamados/services/fio.py:927`)

4. **Há também três ocorrências sem leitura do valor cru:** `PerguntaDisputa:1` vem de `CAMPOS_QUE_ACORDAM`, confrontado com nomes de `update_fields`; as duas de `hub_smoke` são nomes de parâmetros. Estas últimas são uma sobrecontagem expressamente admitida pelo varredor. (`app/chamados/tests/catraca_data_evento_cru.json:39`; `app/chamados/models.py:3221`; `app/chamados/signals.py:255`; `app/escala/management/commands/hub_smoke.py:35`; `app/escala/management/commands/hub_smoke.py:51`; `app/chamados/tests/test_catraca_data_evento_cru.py:22`)

5. **Leitor real novo omitido pelas exclusões novas:** não localizei na árvore examinada. As chaves literais encontradas são payload/trilha; o literal sob `fields=` é schema. Há, porém, regressão reproduzida em memória: `M.objects.create(x=p['data_evento'])` resulta em `{}`, pois `escrita=True` também suprime a chave do subscrito **lido**. Ocorrência real dessa forma na cópia: **[nao verificado]**. (`app/api/views.py:1165`; `app/chamados/services/materializacao.py:578`; `app/chamados/services/fio.py:1047`; `app/chamados/models.py:3483`; `app/chamados/tests/test_catraca_data_evento_cru.py:102`; `app/chamados/tests/test_catraca_data_evento_cru.py:123`; `app/chamados/tests/test_catraca_data_evento_cru.py:139`)

Os quatro métodos sem escrita passaram executados com base `unittest` em memória; isso não comprova execução da suíte Django, **[nao verificado]**. O teste que cria arquivos temporários não foi executado. (`app/chamados/tests/test_catraca_data_evento_cru.py:173`; `app/chamados/tests/test_catraca_data_evento_cru.py:186`; `app/chamados/tests/test_catraca_data_evento_cru.py:191`; `app/chamados/tests/test_catraca_data_evento_cru.py:226`; `app/chamados/tests/test_catraca_data_evento_cru.py:234`)

## 11/11 15:41 · sessao 331c29b2b46f · - · - · origem=Ronald (tmux)

Há falsos positivos de escrita remanescentes:

- `gravar_dia_do_fato` entra com 1 no JSON; a ocorrência é `return 'data_evento'`, nome devolvido para `save(update_fields=...)`, após atribuir o campo. (`app/chamados/tests/catraca_data_evento_cru.json:79`; `app/chamados/services/fio.py:800`; `app/chamados/services/fio.py:801`; `app/chamados/services/fio.py:1037`; `app/chamados/services/fio.py:1043`)
- Quatro repasses `reapontar_resolvedora(..., data_evento=...)` ainda contam como leitura: a porta falta em `ESCRITA`, e qualquer keyword correspondente fora dessa lista incrementa o censo. A porta recebe o pedido, consulta o juiz, atribui e salva. (`app/chamados/services/declaracao_texto.py:370`; `app/chamados/reconciliador.py:954`; `app/chamados/reconciliador.py:1642`; `app/chamados/reconciliador.py:1655`; `app/chamados/tests/test_catraca_data_evento_cru.py:57`; `app/chamados/tests/test_catraca_data_evento_cru.py:127`; `app/chamados/services/fio.py:876`; `app/chamados/services/fio.py:879`; `app/chamados/services/fio.py:927`)
- `PerguntaDisputa` entra com 1 por `CAMPOS_QUE_ACORDAM`: metadado de nomes confrontado com `update_fields`, sem ler o valor cru nessa ocorrência. (`app/chamados/tests/catraca_data_evento_cru.json:39`; `app/chamados/models.py:3221`; `app/chamados/signals.py:255`)
- Os dois escopos novos de `hub_smoke` contam nomes de parâmetros, conforme limite expressamente declarado pelo próprio varredor. (`app/chamados/tests/catraca_data_evento_cru.json:112`; `app/chamados/tests/catraca_data_evento_cru.json:113`; `app/escala/management/commands/hub_smoke.py:35`; `app/escala/management/commands/hub_smoke.py:51`; `app/chamados/tests/test_catraca_data_evento_cru.py:22`)

`_nascer` mantém três leituras reais; retirar a função inteira seria incorreto. (`app/chamados/models.py:2940`; `app/chamados/models.py:2956`; `app/chamados/models.py:2962`; `app/chamados/tests/catraca_data_evento_cru.json:40`)

## 11/11 15:41 · sessao 321d419c4e23 · - · - · origem=Ronald (tmux)

Não identifiquei leitor real atual omitido pelas novas exclusões de chaves de dict ou `DESTINO_KW`: as chaves encontradas são payload/trilha; o literal sob `fields=` pertence ao índice. (`app/api/views.py:1165`; `app/api/views_mensageria.py:568`; `app/chamados/canal.py:79`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:354`; `app/chamados/services/materializacao.py:578`; `app/chamados/services/validacao.py:671`; `app/chamados/services/fio.py:1047`; `app/chamados/services/fio.py:1048`; `app/chamados/models.py:3483`)

Há uma lacuna demonstrável na regra de destino: `M.objects.create(x=p['data_evento'])` retorna censo vazio em simulação em memória; o contexto `escrita=True` suprime a constante do subscrito lido. Ocorrência real dessa forma na árvore: **[nao verificado]**. (`app/chamados/tests/test_catraca_data_evento_cru.py:102`; `app/chamados/tests/test_catraca_data_evento_cru.py:122`; `app/chamados/tests/test_catraca_data_evento_cru.py:139`)

`fields=['data_evento']` é suprimido em qualquer chamada, inclusive eventual serialização; isso constitui limite potencial, sem leitor atual localizado nessa forma. (`app/chamados/tests/test_catraca_data_evento_cru.py:62`; `app/chamados/tests/test_catraca_data_evento_cru.py:137`; `app/chamados/tests/test_catraca_data_evento_cru.py:28`)

## 11/11 15:39 · sessao 97538cdfafe8 · - · - · origem=Ronald (tmux)

Correção à conclusão anterior: **não há garantia geral de C previamente adquirido pelo passivo**. `carimbar` salva Q diretamente em `app/chamados/models.py:3152`; seu `save` apenas abre atomic e executa `super().save` em `app/chamados/models.py:3422`/`:3423`. Os receivers encontrados agendam becos por `on_commit` (`app/chamados/signals.py:239`) e inserem eventos (`app/chamados/signals.py:258`; `app/chamados/models.py:3248`), sem adquirir C nesses trechos.

| Candidatura | Sequências lidas | Classificação |
|---|---|---|
| C ↔ Q | Transição: C `app/chamados/models.py:290` → S143 → Q `app/chamados/services/disputa_emissao.py:1984` → `app/chamados/models.py:3152`. Passivo: atomic `app/chamados/services/passivo_validado.py:371` → Q `app/chamados/services/materializacao.py:1277` → regeneração `app/chamados/services/passivo_validado.py:438` → órfãs `app/ponto/portas/celula.py:289` → C `app/chamados/services/contexto.py:103`; `app/chamados/models.py:486`. | Inversão estrutural; deadlock exige a **mesma Q** elegível nos dois atos. S143 pula pergunta viva ou resposta sem lastro (`app/chamados/services/disputa_emissao.py:1964`; `:1977`). Elegibilidade simultânea **[nao verificado]**. |
| DP ↔ FM | Passivo pode emitir DP no cartório da regeneração (`app/ponto/portas/celula.py:434`; `app/chamados/services/fabrica_da_celula.py:279`) antes do FM (`app/ponto/portas/regen_exportada.py:179`). Pago/devido: FM `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196` → DP `:207`. | C prévio não pode ser presumido globalmente. Entretanto a possibilidade de emitir DP antes de C, conservar DP e alcançar **o mesmo FM** sem bloqueio anterior depende dos dados/ramificações: **[nao verificado]**. |
| Celula ↔ C | Passivo retém outer `app/chamados/services/passivo_validado.py:371`; regeneração retém outer `app/ponto/portas/regen_exportada.py:169`; UPDATE Celula `app/ponto/portas/celula.py:276` → órfãs/C `:289`. Oposto: C `app/chamados/models.py:290` → cartório → Celula `app/ponto/portas/celula.py:905`. | Candidatura runtime adicional; não excluir por suposta pré-trava universal de C. Compatibilidade dos ramos para os mesmos recursos **[nao verificado]**. |
| DP ↔ Q via S2/passivo | DP durante save do Chamado → S2 posterior `app/chamados/models.py:305`/`:312`. Passivo pode Q antes de outro DP `app/chamados/services/materializacao.py:1277`; `app/chamados/services/passivo_validado.py:443`. | **Não certificar esse par:** S2 seleciona Q com `validada_em IS NULL` (`app/chamados/models.py:310`), enquanto o universo do passivo é validado (`app/chamados/services/passivo_validado.py:3`). Sobreposição da mesma Q por essas duas seleções **[nao verificado]**. |

No ramo `data_hora`, `ch=None` e clones vazios permitem que a função auxiliar não reconcilie chamado algum: guarda de destino em `app/chamados/services/materializacao.py:773`, lista de clones em `:775`/`:776`, chamada auxiliar em `:1059`. O signal de Batida chama o reconciliador (`app/ponto/signals.py:215`), porém os atos dependem de candidatos e premissas; por exemplo, o laço pode apenas continuar em `app/chamados/reconciliador.py:1459`. Isso impede afirmar C obrigatório para todo plantio.

## 11/11 15:38 · sessao 5e6e5744386e · TRIAGEM-OFFLINE-SEM-CAMERA · verificador · origem=Code

**APROVADO para o delta no caminho com `JulianiPWA`.** Os tetos de armazenamento/POST e a prova de conclusão da transação foram introduzidos; permanecem as limitações discriminadas abaixo. (`app/static/pwa.js:55`, `app/static/pwa.js:80`, `app/static/pwa.js:230`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:142`)

1. **(a), (b) e (d): curados nesse caminho.** Abertura tem teto de 2 s e rejeição por bloqueio; transação tem teto, `onabort` e resolução somente em `oncomplete`; o gap tem envelope externo de 4 s. POST e corpo de erro compartilham o orçamento de 30 s. As telas capturam a rejeição da gravação e mostram erro. (`app/static/pwa.js:60`, `app/static/pwa.js:74`, `app/static/pwa.js:84`, `app/static/pwa.js:91`, `app/static/pwa.js:213`, `app/static/pwa.js:232`; `app/templates/ponto/triagem/apto.html:308`; `app/templates/ponto/bater_ponto_v2.html:329`)

   Os estados residuais têm consequências diferentes:

   - **Sync/indicador pendurados:** continuam com pedidos/transações fora de `_transacao`; fetch e corpos do sync também continuam sem teto. Isso pode paralisar envio/indicador, mas não desfaz a fila nem segura o retorno do escritor, que dispara o indicador sem `await`. (`app/static/pwa.js:223`, `app/static/pwa.js:248`, `app/static/pwa.js:271`, `app/static/pwa.js:277`, `app/static/pwa.js:306`)
   - **Sync recebe HTTP 400 e conclui a exclusão:** esse caminho deixa a fila vazia com apenas `console.warn`, sem erro visível. É uma perda silenciosa ainda presente; já consta como pendência própria na cópia. (`app/static/pwa.js:284`; `app/docs/BACKLOG.md:1944`)
   - **`tx.abort()` lança durante commit:** a promessa rejeita mesmo assim, portanto há erro visível. Porém o código não confirma rollback: pode haver gravação tardia após o erro. A ocorrência real desse estado é [nao verificado]. (`app/static/pwa.js:84`; `app/templates/ponto/bater_ponto_v2.html:329`)
   - **Conexão chega tarde:** a abertura já rejeitou; o sucesso tardio fecha a conexão e não retoma aquele escritor. (`app/static/pwa.js:60`, `app/static/pwa.js:72`, `app/static/pwa.js:215`)
   - **Sem `postarBatidaNoTeto`:** permanece `fetch/json` cru; uma promessa pendente deixa botão desabilitado, sem fila e sem erro. O ensaio explicitamente trata ausência de PWA como pendência própria, exigindo apenas preservar o envio online. (`app/templates/ponto/bater_ponto_v2.html:294`, `app/templates/ponto/bater_ponto_v2.html:312`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:552`)

   **(d)** agora marca a fila na conclusão, desfaz linhas no aborto e exige promessa resolvida mais exatamente uma linha persistida. Commit em curso e abertura tardia não são modelados pelo falso IndexedDB. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:142`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:203`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:605`)

2. **(c): permanece sem identidade original, mas isso sozinho não sustenta bloqueante no reenvio de 30 s.** Um clique online pode produzir o POST inicial e uma linha de fila; essa linha pode gerar **N retransmissões**, sem limite declarado, enquanto respostas/exclusões falharem. Quantidade de requisições não equivale à quantidade de batidas gravadas. (`app/templates/ponto/bater_ponto_v2.html:307`, `app/templates/ponto/bater_ponto_v2.html:323`; `app/static/pwa.js:256`, `app/static/pwa.js:276`, `app/static/pwa.js:295`)

   A porta deduplica **mesmo colaborador/mesmo tipo, ±60 s inclusivos**, entre não retratadas. A API usa **mesmo colaborador/mesmo tipo, ±5 minutos inclusivos**, sobre `ts_efetivo`; não sobre a chegada quando existe hora válida do aparelho. (`app/ponto/registro_batida.py:106`; `app/api/views.py:519`, `app/api/views.py:539`)

   Portanto, reenvio **sequencial**, primeira gravação já visível, mesmo tipo e instantes separados por até 30 s é absorvido. Garantia universal permanece [nao verificado]: o teto limita espera da resposta, não o prazo da sincronização; além disso, a dedup precede a eventual correção de tipo. Assim, identidade original fica como pergunta de desenho, sem duplicação demonstrada pelos anexos. (`app/static/pwa.js:230`, `app/static/pwa.js:244`; `app/api/views.py:539`, `app/api/views.py:582`)

3. **Não encontrei nova barreira nem segundo juiz da selfie nesses caminhos.** A lei fixa escrita em `registro_batida.py`; as telas consultam o juiz central e seguem com foto nula. No servidor, `selfie_ausente` acrescenta motivo de revisão, sem recusar a batida. (`CLAUDE.md:143`; `app/static/pwa.js:170`; `app/templates/ponto/triagem/apto.html:248`, `app/templates/ponto/triagem/apto.html:275`; `app/templates/ponto/bater_ponto_v2.html:269`; `app/api/views.py:449`)

4. **O instante único é a entrada do escritor, não necessariamente o primeiro clique.** `Date.now()` alimenta os dois timestamps antes das esperas; na recuperação online, o escritor só é chamado depois do POST frustrado, potencialmente 30 s depois. (`app/static/pwa.js:211`; `app/templates/ponto/bater_ponto_v2.html:293`, `app/templates/ponto/bater_ponto_v2.html:323`)

   O sync envia o timestamp **cru do aparelho**, sem enviar o timestamp corrigido/gap. A API o entrega a `hora_do_aparelho`: atraso entre 0 e 48 h preserva o instante; acima de 3 minutos acrescenta trilha; futuro ou antigo demais usa `agora`. Logo, 30 s de atraso, com relógio compatível, é aceito. O teto de POST não garante sincronização dentro de 30 s. (`app/static/pwa.js:265`; `app/api/views.py:519`; `app/api/hora_do_aparelho.py:62`, `app/api/hora_do_aparelho.py:141`)

5. **Nenhuma das quatro ausências de mutante demonstra, isoladamente, bloqueante de produto.** Fechamento tardio protege conexões futuras; catch interno do gap está protegido, para a fila, pelo envelope externo; catch do indicador é posterior à gravação e sem `await`; instante único está implementado, mas sem prova de regressão temporal. Os mutantes existentes cobrem outras guardas, não essas quatro. (`app/static/pwa.js:72`, `app/static/pwa.js:134`, `app/static/pwa.js:166`, `app/static/pwa.js:212`, `app/static/pwa.js:223`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:810`)

6. **O template apagado não tem referência executável encontrada na cópia.** Restam comentário do guard, comentário histórico do teste e tabela documental. URLs apontam à view, que escolhe v2 ou triagem. O SW referencia `/ponto/bater/`, não o nome do template; seu ramo de navegação retorna antes do trecho que alimentaria esse cache. (`app/scripts/check_a_lei_batida.py:20`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:673`; `app/docs/TICKETS.md:693`; `app/ponto/urls.py:11`; `app/ponto/views.py:225`, `app/ponto/views.py:356`; `app/static/sw.js:45`, `app/static/sw.js:67`)

   Há lacunas nos instrumentos alterados: o scanner não inclui apto/v2; o guard não inclui v2 e pula caminhos inexistentes. Elas não demonstram quebra causada pela exclusão. (`app/tools/check_frontend_urls.py:25`; `app/scripts/check_a_lei_batida.py:14`, `app/scripts/check_a_lei_batida.py:21`, `app/scripts/check_a_lei_batida.py:31`)

Suíte verde e smoke nesta cópia: **[nao verificado]**. Não executei o ensaio Chromium porque ele cria arquivos/perfil e depois os apaga. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:344`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:360`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:372`)

## 11/11 15:38 · sessao 86de3be02df9 · LASTRO-FOLGA-FORA-DO-CATALOGO · verificador · origem=Code

**Veredito: BLOQUEANTES B1–B3.** A guarda consulta a autoridade existente, mas herda um cache sem isolamento por tenant, introduz uma exceção sem tradução em portas HTTP e permite reintroduzir legado numa corrida entre leitura e gravação (`app/ponto/catalogo/ausencias.py:257`, `app/ponto/models.py:883`, `app/ponto/services/ausencia.py:418`, `app/api/views.py:1825`).

**1. Quem escreve `Ausencia.tipo`**

A construção e persistência estão em `criar_ausencia`; a troca do campo está em `editar_ausencia_registro`. Ambas validam pelo catálogo antes de salvar (`app/ponto/services/ausencia.py:194`, `app/ponto/services/ausencia.py:342`, `app/ponto/services/ausencia.py:418`, `app/ponto/services/ausencia.py:474`, `app/ponto/services/ausencia.py:498`, `app/ponto/services/ausencia.py:509`).

| Chamador produtivo encontrado | Valor fornecido |
|---|---|
| Lançamento administrativo | Variável do formulário (`app/ponto/views.py:2415`) |
| Solicitação PWA | Variável da solicitação (`app/ponto/views.py:2536`) |
| API | Variável, após tradução dos aliases legados (`app/api/views.py:1765`, `app/api/views.py:1817`) |
| Copiloto | Variável encaminhada ao produtor (`app/ponto/services/checar_ausencia.py:46`) |
| Veredito da célula | Literal `falta` ou variável `tipo_ausencia` (`app/ponto/services/veredito_celula.py:332`) |
| Materialização de declaração | `falta` ou `atestado`, pelo mapa (`app/chamados/services/materializacao.py:490`, `app/chamados/models.py:3716`) |
| Materialização de resposta | `falta` ou `atestado`, pela tabela de escapes (`app/chamados/services/materializacao.py:526`, `app/chamados/services/materializacao.py:602`) |
| Espelho de férias | Literal `ferias`, pelo produtor (`app/ponto/services/ausencia.py:1202`) |
| Regularização de retorno | Literal `falta`, por `garantir_ausencia` (`app/ponto/services/regularizacao.py:118`, `app/ponto/services/ausencia.py:461`) |
| Comando de abandono provisório | Literal `falta`, por `garantir_ausencia` (`app/ponto/management/commands/marcar_abandono_provisorio.py:76`) |
| Edição pela UI | POST ou tipo atual, pela porta de edição (`app/ponto/views.py:2794`) |
| Comando de rederivação H06 | Tipo atual, pela porta de edição (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`) |

Os literais efetivamente produzidos pelos caminhos fixos encontrados estão semeados:

| Literal | Semente |
|---|---|
| `falta` | Sim (`app/ponto/migrations/0052_seed_tipoausencia.py:10`) |
| `atestado` | Sim (`app/ponto/migrations/0052_seed_tipoausencia.py:17`) |
| `ferias` | Sim (`app/ponto/migrations/0052_seed_tipoausencia.py:23`) |
| `declaracao`, destino do alias `atestado_acompanhante` | Sim (`app/api/views.py:1766`, `app/ponto/migrations/0052_seed_tipoausencia.py:18`) |
| `licenca_luto`, destino do alias `obito` | Sim (`app/api/views.py:1767`, `app/ponto/migrations/0052_seed_tipoausencia.py:20`) |

`atestado_medico` é traduzido para `atestado`; os aliases não são escritos literalmente pela API (`app/api/views.py:1765`). Não há incompatibilidade demonstrada entre esses literais e a semente; estarem ativos hoje em produção é **[não verificado]**.

O admin de Ausência bloqueia adicionar, alterar e apagar; os receivers de Ausência consomem o fato, sem atribuir `tipo` (`app/ponto/admin.py:38`, `app/core/admin.py:59`, `app/core/admin.py:62`, `app/core/admin.py:65`, `app/ponto/signals.py:143`, `app/ponto/signals.py:275`). O `update` produtivo de cancelamento de férias altera somente status e carimbo (`app/ponto/services/ausencia.py:1281`). A cópia CSV encontrada contorna o modelo, mas pertence ao pipeline de ensaio (`bin/sombra_incremental.sh:58`, `bin/sombra_incremental.sh:62`).

**O escritor histórico de #5356 permanece [não verificado].** Os testes demonstram que `QuerySet.update(tipo=...)` consegue fabricar esse legado, mas isso não identifica a origem produtiva (`app/ponto/tests/test_lastro_folga_save_guarda.py:67`).

**2. Catálogo, cache e cadastro pela UI — B1**

Os leitores diretos de `codigos_validos()` são formulário, prévia do copiloto, criação, edição, composição dos tipos que exigem documento, vigia, solicitação PWA e agora a guarda do modelo (`app/ponto/forms.py:63`, `app/ponto/services/checar_ausencia.py:23`, `app/ponto/services/ausencia.py:194`, `app/ponto/services/ausencia.py:474`, `app/ponto/services/ausencia.py:804`, `app/ponto/services/vigia_ausencia.py:103`, `app/ponto/views.py:2509`, `app/ponto/models.py:879`).

O cache é um único dicionário de módulo, com TTL de **60 segundos medidos por `monotonic()`**. A criação/edição pela UI salva `TipoAusencia`, cujo signal invalida esse dicionário no processo local (`app/ponto/catalogo/ausencias.py:257`, `app/ponto/catalogo/ausencias.py:258`, `app/ponto/catalogo/ausencias.py:275`, `app/ponto/views_tipos_ausencia.py:94`, `app/ponto/services/ausencia.py:1310`, `app/ponto/models.py:1260`).

Consequências:

- No processo invalidado, a próxima leitura consulta novamente o banco; outro processo com cache quente pode recusar o tipo recém-criado até expirar o TTL. Também pode continuar aceitando um tipo desativado nesse intervalo (`app/ponto/catalogo/ausencias.py:263`, `app/ponto/catalogo/ausencias.py:276`, `app/ponto/models.py:879`).
- **B1:** a chave não inclui tenant/schema. Com `ponto` instalado por tenant, uma leitura no tenant B pode reutilizar o catálogo anteriormente carregado no tenant A, admitindo código alheio ou recusando código próprio (`app/config/settings/saas.py:34`, `app/config/settings/saas.py:40`, `app/config/settings/saas.py:68`, `app/ponto/catalogo/ausencias.py:257`, `app/ponto/catalogo/ausencias.py:276`).
- Tabela vazia, todos os tipos inativos **ou qualquer exceção na consulta** fazem retornar a semente congelada. Portanto, vazio não significa “nenhum código permitido”; inclusive desativar todos reabilita os códigos da semente na autoridade (`app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:252`, `app/ponto/catalogo/ausencias.py:253`, `app/ponto/catalogo/ausencias.py:278`).

A guarda herda esses comportamentos de uma autoridade preexistente; ocorrência concreta entre workers/tenants de produção é **[não verificado]** (`app/ponto/models.py:878`).

**3. Consulta histórica, corrida e contornos — B3**

Com cache quente, código válido não exige a consulta histórica. Código fora do catálogo em instância persistida acrescenta **um SELECT por chamada de save**; cache frio acrescenta também a consulta do catálogo. Uma decisão pode chamar save duas vezes, pagando duas consultas históricas para legado (`app/ponto/models.py:879`, `app/ponto/models.py:882`, `app/ponto/catalogo/ausencias.py:249`, `app/ponto/services/ausencia.py:911`, `app/ponto/services/ausencia.py:927`). Latência e SQL efetivamente emitido são **[não verificado]**.

**B3 — comparação e gravação não são atômicas.** Interleaving permitido pelo código: A consulta e encontra `folga`; B corrige para `abono`; A termina seu save completo e recoloca `folga`. A guarda não usa lock nem UPDATE condicionado ao tipo observado (`app/ponto/models.py:883`, `app/ponto/models.py:885`, `app/ponto/models.py:933`). Ocorrência produtiva é **[não verificado]**.

Quanto aos contornos:

- `update_fields` **não desliga** a guarda. Mesmo salvando só documento/status, ela compara o tipo em memória com o banco; pode recusar uma operação que nem escreveria `tipo`, caso outra operação já tenha corrigido o legado (`app/ponto/models.py:891`, `app/ponto/models.py:893`, `app/ponto/services/ausencia.py:835`, `app/ponto/services/ausencia.py:859`).
- `QuerySet.update(tipo=...)` contorna a guarda, demonstrado expressamente pelas fixtures (`app/ponto/tests/test_lastro_folga_save_guarda.py:67`).
- O override não contém proteção em `save_base`, bulk ou SQL direto. A implementação dessas APIs Django não está demonstrada pelos arquivos examinados; confirmação de `save_base(raw=True)`/`loaddata`, `bulk_create` e `bulk_update` nesta cópia: **[não verificado]**. Não há exceção explícita para `raw` dentro da guarda (`app/ponto/models.py:868`, `app/ponto/models.py:891`).

**4. Exceção HTTP e batida — B2**

**B2 — a nova exceção não é traduzida consistentemente.** Na criação, somente o `ValidationError` do `full_clean` vira `AusenciaInvalida`; o save posterior limpa eventual anexo e relança a exceção original. A API captura apenas `AusenciaInvalida` (`app/ponto/services/ausencia.py:387`, `app/ponto/services/ausencia.py:418`, `app/ponto/services/ausencia.py:419`, `app/ponto/services/ausencia.py:429`, `app/api/views.py:1825`).

Cenário concreto permitido: o código passa na validação inicial; outra thread desativa o tipo e invalida o cache; o save consulta o catálogo atualizado e lança o novo `ValidationError`, que escapa da API. O caminho de edição tem a mesma janela e sua view também não captura essa exceção (`app/ponto/services/ausencia.py:194`, `app/ponto/models.py:1263`, `app/ponto/models.py:887`, `app/ponto/services/ausencia.py:474`, `app/ponto/services/ausencia.py:509`, `app/ponto/views.py:2818`). Resposta 500 observada em produção: **[não verificado]**.

Outros propagadores são veredito da célula, espelho de férias, mudanças de estado, decisão e anexação; nenhum deles traduz especificamente o novo erro do catálogo (`app/ponto/services/veredito_celula.py:340`, `app/ponto/services/ausencia.py:1209`, `app/ponto/services/ausencia.py:835`, `app/ponto/services/ausencia.py:927`, `app/ponto/services/ausencia.py:865`). Legado inalterado passa normalmente; a falha exige mudança concorrente ou outro desacordo entre instância e persistência (`app/ponto/models.py:885`).

Lançamento e submissão web têm captura externa de `Exception`, portanto não cabe afirmar 500 indistintamente nessas telas (`app/ponto/views.py:2458`, `app/ponto/views.py:2558`).

**Não foi demonstrado caminho da batida até `Ausencia.save`.** O chokepoint cria `Batida`; recompute, julgamento da célula, reconciliação e fechamento por evento têm contenção de exceções nos caminhos examinados (`app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:175`, `app/ponto/signals.py:19`, `app/ponto/signals.py:85`, `app/ponto/signals.py:131`, `app/ponto/signals.py:206`). Violação nova da lei “batida nunca barrada” por esta guarda: **[não verificado]**.

**5. Segundo juiz e dois significados**

A guarda é **leitora da mesma autoridade de validade**, pois chama `codigos_validos`, exatamente como criar e editar. `faixa_de` responde outra pergunta, a precedência; seu default continua sendo `atestado` para código desconhecido (`app/ponto/models.py:878`, `app/ponto/services/ausencia.py:194`, `app/ponto/services/ausencia.py:474`, `app/ponto/precedencia.py:76`).

A divergência estrutural com `TIPO_CHOICES` permanece: o campo declara choices congeladas, enquanto o serviço exclui `tipo` do `full_clean` para aceitar códigos vivos adicionais. A guarda não cria essa segunda lista; o teste novo preserva explicitamente código vivo fora das choices (`app/ponto/models.py:661`, `app/ponto/models.py:749`, `app/ponto/services/ausencia.py:387`, `app/ponto/tests/test_lastro_folga_save_guarda.py:134`).

**6. Fixtures e mordida**

As alterações preservam as asserções originais relevantes:

- Carimbo: mantém `obito` legado e exige `aprovada_em` na aprovação/rejeição (`app/ponto/tests/test_decisao_carimba.py:22`, `app/ponto/tests/test_decisao_carimba.py:32`, `app/ponto/tests/test_decisao_carimba.py:39`).
- Espelho: mantém `obito` e compara a apresentação com o juiz do status (`app/ponto/tests/test_caracterizacao_espelho.py:53`, `app/ponto/tests/test_caracterizacao_espelho.py:89`).
- Fechamento: mantém `atestado_medico` desconhecido e exige DSR perdido igual a zero (`app/ponto/tests/test_fechamento_ausencia_pela_autoridade.py:83`, `app/ponto/tests/test_fechamento_ausencia_pela_autoridade.py:86`).
- Cadastro/dropdown: acrescentam cleanup de cache e mantêm admissão de novo código, retirada do inativo e fallback vazio (`app/ponto/tests/test_cadastro_tipo.py:15`, `app/ponto/tests/test_cadastro_tipo.py:23`, `app/ponto/tests/test_cadastro_tipo.py:35`, `app/ponto/tests/test_dropdown_vivo.py:21`, `app/ponto/tests/test_dropdown_vivo.py:29`, `app/ponto/tests/test_dropdown_vivo.py:34`).

Há uma diferença: as fixtures agora disparam primeiro os efeitos de `abono` e depois os do legado. O segundo save executa os receivers, mas equivalência completa dos efeitos intermediários é **[não verificado]** (`app/ponto/tests/test_caracterizacao_espelho.py:50`, `app/ponto/tests/test_caracterizacao_espelho.py:55`, `app/ponto/signals.py:143`, `app/ponto/signals.py:275`).

O selo novo tem asserções que detectariam remoção da guarda e quebra da preservação histórica; não cobre os cenários B1–B3. Execução verde, RED anterior e mutantes executados são **[não verificado]** (`app/ponto/tests/test_lastro_folga_save_guarda.py:73`, `app/ponto/tests/test_lastro_folga_save_guarda.py:94`, `app/ponto/tests/test_lastro_folga_save_guarda.py:102`, `app/ponto/tests/test_lastro_folga_save_guarda.py:134`).

**7. Classes de reprovação**

Os bloqueantes são: **B1**, cache usado pela barreira sem isolamento por tenant e sem invalidação entre processos; **B2**, exceção nova sem tradução nas portas; **B3**, leitura histórica separada da gravação, permitindo reintrodução concorrente do código fora do catálogo (`app/ponto/catalogo/ausencias.py:257`, `app/ponto/models.py:1263`, `app/api/views.py:1825`, `app/ponto/models.py:883`, `app/ponto/models.py:933`).

Não foi demonstrado segundo produtor produtivo de `tipo`; há contorno por `update` nas fixtures e duplicidade preexistente entre choices e catálogo (`app/ponto/tests/test_lastro_folga_save_guarda.py:67`, `app/ponto/models.py:749`). O diff não acrescenta relógio, mas passa a depender do TTL preexistente na decisão de persistência (`app/ponto/models.py:878`, `app/ponto/catalogo/ausencias.py:275`). Origem de #5356, censo real da frota, suite completa e pré-requisitos de pouso permanecem **[não verificado]**.

## 11/11 15:37 · sessao d270727d20b6 · - · - · origem=Ronald (tmux)

Meu julgamento é **APROVADO no escopo da cura com `JulianiPWA`**, sem afirmar que todos os caminhos de rede/armazenamento ficaram limitados. A abertura rejeita no teto/bloqueio; a transação rejeita no teto/aborto; o POST e seu corpo de erro têm teto. (`app/static/pwa.js:60-74`; `app/static/pwa.js:83-94`; `app/static/pwa.js:230-241`)

- **Aborto lançando durante commit:** a exceção é engolida, mas a promessa rejeita mesmo assim e a tela mostra erro. Pode restar gravação tardia, contrariando a garantia de “nenhum fantasma”; não produz clique pendente ou perda silenciosa. Ocorrência real desse estado: [nao verificado]. (`app/static/pwa.js:84-93`; `app/templates/ponto/bater_ponto_v2.html:329-331`)
- **Conexão tardia:** a abertura já rejeitou; `onsuccess` tardio fecha a conexão. Não volta a executar a gravação daquele escritor. (`app/static/pwa.js:60-72`; `app/static/pwa.js:215-216`)
- **Sync/indicador:** os pedidos e transações continuam fora de `_transacao`, assim como o fetch/corpos do sync. Pendência neles não desfaz a fila já gravada nem segura o retorno do escritor. Há perda silenciosa explícita se o sync recebe HTTP 400 e a exclusão completa: só há `console.warn`. (`app/static/pwa.js:223`; `app/static/pwa.js:248-253`; `app/static/pwa.js:271-310`)
- **Exceção importante de (b):** sem `postarBatidaNoTeto`, a v2 continua fazendo `fetch` e `json` sem teto; resposta pendente deixa botão desabilitado, sem fila e sem erro. É caminho residual demonstrável pelo código. O próprio ensaio declara PWA ausente como outra fila e só exige preservar o envio online; portanto não o apresentaria como regressão nova desta cura. (`app/templates/ponto/bater_ponto_v2.html:294-295`; `app/templates/ponto/bater_ponto_v2.html:312-314`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:552-557`)
- **(c):** um clique pode gerar POST inicial e uma linha reenviável repetidamente. Isso não equivale a múltiplas linhas no servidor: reenvio sequencial do mesmo tipo, com instantes separados por 30 s e primeira gravação visível, cabe em ±5 min da API e ±60 s da porta. Falta de identidade isolada não sustenta bloqueante nesse cenário; garantia para relógio incompatível, correção de tipo ou concorrência permanece [nao verificado]. (`app/templates/ponto/bater_ponto_v2.html:307-323`; `app/static/pwa.js:256-298`; `app/api/views.py:539-555`; `app/ponto/registro_batida.py:106-115`)
- **(d)/mordidas:** o ensaio agora marca fila na conclusão e exige promessa resolvida/persistência, mas seu falso IDB não modela commit em curso nem fechamento tardio. Essas limitações impedem certificação universal; sozinhas não demonstram novo bloqueante de produto. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:142-153`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:195-215`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:605-616`)

## 11/11 15:37 · sessao b199bd514621 · - · - · origem=Ronald (tmux)

| Recurso adicional | Ordem demonstrada no código | Classificação do par |
|---|---|---|
| TurnoMaterializado | Passivo: atomic do par → materializador → INSERT Batida → receiver do núcleo → DELETE/INSERT TurnoMaterializado → cartório/fábrica → eventual C. (`app/chamados/services/passivo_validado.py:371`; `:427`; `:511`; `:516`; `app/chamados/services/materializacao.py:1220`; `:1007`; `app/ponto/registro_batida.py:157`; `app/ponto/signals.py:36`; `app/ponto/nucleo.py:58`; `:59`; `:81`; `app/ponto/signals.py:135`; `app/chamados/services/fabrica_da_celula.py:365`) | **Candidato TM→C contra C→TM**: aprovação de justificativa toma C antes de registrar Batida e recomputar os mesmos turnos. Compatibilidade das travas implícitas de FK e execução concorrente **[nao verificado]**. (`app/ponto/services/justificativa.py:140`; `:141`; `:129`; `:187`; `app/ponto/signals.py:36`; `app/ponto/nucleo.py:81`) |
| Justificativa | C/D/Ch → Justificativa FOR UPDATE → eventual Batida/TM → cartório/Pauta DP → UPDATE Justificativa → UPDATE FM. (`app/ponto/services/justificativa.py:129`; `:140`; `:142`; `:187`; `:208`; `:215`; `app/ponto/services/fechamento.py:1656`) | **Inversão abstrata DP→FM contra FM→DP**, mas a aprovação e o pago/devido adquirem C antes; para mesma linha FM, o mesmo colaborador serializa os dois atos. (`app/ponto/services/justificativa.py:129`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`; `:196`; `:207`) |
| Justificativa na materialização retroativa | Atomic → C/D/Ch → Justificativa FOR UPDATE → registrar Batida/TM → UPDATE Justificativa/cartório → UPDATE FM. (`app/ponto/services/justificativa.py:342`; `:343`; `:344`; `:361`; `:386`; `:390`; `app/ponto/services/fechamento.py:1656`) | Mesma ressalva de serialização por C para o par DP/FM. (`app/ponto/services/justificativa.py:129`; `:343`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`) |
| Ausencia | Aprovação de intervalo: atomic → UPDATE Ausencia → cartório síncrono → possível Pauta DP → invalidar FM. A porta da ausência não adquire C antes do UPDATE. (`app/ponto/views.py:2641`; `:2643`; `app/ponto/services/ausencia.py:911`; `:835`; `app/ponto/signals.py:143`; `:147`; `app/ponto/services/ausencia.py:946`; `app/ponto/services/fechamento.py:1656`) | **Candidato DP→FM sem C**, quando a fábrica segue por limbo exportado, que escreve DP diretamente; satisfatibilidade conjunta dos guards com aprovação parcial **[nao verificado]**. (`app/chamados/services/fabrica_da_celula.py:214`; `:228`; `:231`; `:279`; `app/ponto/services/ausencia.py:924`) |
| Ausencia em rejeição/cancelamento | A porta também invalida FM após rejeitar, mas as views lidas não envolvem o ato inteiro em atomic. (`app/ponto/views.py:2700`; `:2701`; `:2734`; `:2351`; `app/ponto/services/ausencia.py:946`) | **Não comprova retenção de DP até FM** nessas entradas. `Ausencia.save` termina em `super().save`, sem atomic próprio envolvendo a decisão inteira. (`app/ponto/models.py:868`; `:908`; `app/ponto/services/ausencia.py:835`; `:946`) |
| Ausencia de férias | Cancelamento: Agendamento → PeriodoAquisitivo → UPDATE/DELETE Ausencia de tipo `ferias` → signals/cartório → eventual C/DP. (`app/ferias/services.py:364`; `:367`; `:402`; `:411`; `app/ponto/services/ausencia.py:1281`; `:1283`; `app/ferias/models.py:451`; `app/ponto/services/ausencia.py:1183`; `:1190`; `app/ponto/signals.py:151`) | **Não demonstrar C↔Ausencia usando escapes de resposta**: esses escapes escrevem apenas `falta`/`atestado`, não a mesma linha `ferias`. (`app/chamados/services/materializacao.py:597`; `:598`; `:599`; `app/ponto/services/ausencia.py:1183`) |

No ramo ESCAPE, a trava C do `responder_pergunta_disputa_core` está em um atomic que termina antes da chamada de materialização da ausência. O caminho web lido, portanto, não comprova C retido durante esse UPDATE de Ausencia. (`app/chamados/services/disputa_emissao.py:1503`; `:1511`; `:1541`; `app/chamados/views.py:1228`; `:1283`)

O recálculo FM por evento de Batida continua em `on_commit`; ele não demonstra FM síncrono depois de DP no atomic anterior. A invalidação FM das duas portas de justificativa acima é síncrona e constitui caminho distinto. (`app/ponto/registro_batida.py:175`; `app/ponto/services/fechamento.py:1824`; `app/ponto/services/justificativa.py:215`; `:390`; `app/ponto/services/fechamento.py:1656`)

## 11/11 15:36 · sessao e3b00d6965c5 · - · - · origem=Ronald (tmux)

Censo de escritores concluído, somente por leitura.

| Sítio | `tipo` que fornece | Seed |
|---|---|---|
| `app/ponto/services/ausencia.py:341`, `:385`, `:418` | Variável: constrói e salva a linha nova | Validada em `:193-195` |
| `app/ponto/services/ausencia.py:498`, `:509` | Variável: troca do tipo existente | Validada em `:473-475` |
| `app/ponto/views.py:2415` | Variável do lançamento administrativo | Passa pelo produtor |
| `app/ponto/views.py:2536` | Variável da solicitação PWA | Passa pelo produtor |
| `app/api/views.py:1817` | Variável da API | Passa pelo produtor |
| `app/ponto/services/checar_ausencia.py:46` | Variável do copiloto; chamada HTTP em `app/core/views_ia.py:985` | Passa pelo produtor |
| `app/ponto/services/veredito_celula.py:332`, `:336` | `'falta'` ou variável `tipo_ausencia` | `'falta'`: `app/ponto/migrations/0052_seed_tipoausencia.py:10` |
| `app/chamados/services/materializacao.py:490` | `'falta'`/`'atestado'`, mapa em `app/chamados/models.py:3716` | Seed `0052:10`, `:17` |
| `app/chamados/services/materializacao.py:526` | `'falta'`/`'atestado'`, mapa em `:602`; consumidor `app/chamados/services/disputa_emissao.py:1541` | Seed `0052:10`, `:17` |
| `app/ponto/services/ausencia.py:1202` | `'ferias'` | Seed `0052:23` |
| `app/ponto/services/regularizacao.py:118` | `'falta'`, por `garantir_ausencia` | Seed `0052:10` |
| `app/ponto/management/commands/marcar_abandono_provisorio.py:76` | `'falta'` | Seed `0052:10` |
| `app/ponto/views.py:2794` | Edição: POST ou tipo atual | Usa `editar_ausencia_registro` |
| `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169` | Edição: `a.tipo` atual | Usa `editar_ausencia_registro` |

Os únicos literais produtores encontrados foram `falta`, `atestado` e `ferias`, todos semeados; o catálogo da seed grava `ativo=True`. `app/ponto/migrations/0052_seed_tipoausencia.py:10`, `:17`, `:23`, `:36`.

A API recebe nomes legados, mas traduz antes de escrever: `atestado_medico → atestado`, `atestado_acompanhante → declaracao`, `obito → licenca_luto`; os destinos estão semeados. Isso não constitui escritor de literal desconhecido. `app/api/views.py:1765`; `app/ponto/migrations/0052_seed_tipoausencia.py:17`, `:18`, `:20`.

Férias materializam pela porta: `_sync_ausencia → espelhar_ferias → criar_ausencia(tipo='ferias')`. `app/ferias/models.py:386`, `:391`; `app/ponto/services/ausencia.py:1202`.

O admin de Ausencia é somente leitura, com bloqueio de adicionar/alterar/apagar; os signals de Ausencia encontrados consomem o fato. `app/ponto/admin.py:38`; `app/core/admin.py:59`, `:62`, `:65`; `app/ponto/signals.py:143`, `:275`.

Há `queryset.update` produtivo de Ausencia para rejeitar férias, mas altera apenas `status`/`atualizado_em`, sem escrever `tipo`. `app/ponto/services/ausencia.py:1281`.

Fora do app, `sombra_incremental.sh` copia `ponto_ausencia` incluindo `tipo`, por CSV, contornando o modelo; o destino declarado é o banco de ensaio. `bin/sombra_incremental.sh:14`, `:36`, `:57`, `:62`. A máscara genérica usa bulk, mas os campos de Ausencia declarados são `documento` e `descricao`. `bin/sombra_regras_pessoais.py:64`, `:109`.

Não encontrei outro escritor produtivo/importador de `tipo` na cópia; os contratos existentes fiscalizam construção direta e escritas fora da porta, mas não foram executados nesta auditoria. `app/ponto/tests/test_contract_um_produtor_de_ausencia.py:49`, `:74`, `:88`; `app/ponto/tests/test_chokepoint_ausencia_gate.py:53`.

A origem específica da escrita de #5356 permanece **[não verificado]**.

## 11/11 15:36 · sessao 62b4dee2cfd9 · - · - · origem=Ronald (tmux)

(4) **Há caminhos HTTP que não traduzem o novo `ValidationError`, mas o legado sem troca de tipo passa normalmente.** A guarda permite tipo existente idêntico (`app/ponto/models.py:882–889`); portanto, não é correto afirmar que decidir/cancelar qualquer ausência legada passará a dar 500.

- `criar_ausencia`: traduz apenas `ValidationError` de `full_clean` (`app/ponto/services/ausencia.py:385–391`). O `save()` está depois, em `:418`; o `except Exception` limpa anexo e relança exceções diferentes da constraint conhecida (`:419–429`). API captura apenas `AusenciaInvalida` (`app/api/views.py:1817–1844`). Se o catálogo mudar/expirar entre a validação inicial e o save, o novo `ValidationError` escapa dessa API; ocorrência real [não verificado].
- `editar_ausencia_registro`: valida vivo em `:474–475`, atribui tipo em `:498`, salva sem tradução em `:509`. A view captura somente `AusenciaInvalida`, `ValueError`, `TypeError` (`app/ponto/views.py:2818`). Mesma janela de corrida; ocorrência real [não verificado].
- `veredito_celula`: chama produtor e captura só `AusenciaInvalida` (`app/ponto/services/veredito_celula.py:331–341`); a view chama o serviço sem catch (`app/ponto/views.py:3339–3341`).
- Decisão/status: `mudar_estado` salva sem catch (`app/ponto/services/ausencia.py:835`), `decidir` pode salvar novamente (`:927`). Lotes aprovação/rejeição capturam apenas `DoesNotExist` (`app/ponto/views.py:2293`, `:2324`); lote cancelamento não envolve a chamada com catch (`:2351`). Reverter captura domínio, não Django VE (`:2588`); período captura domínio/ValueError/DoesNotExist (`:2674`); cancelar individual só DoesNotExist (`:2745`). **Sem troca concorrente de tipo/remoção da linha, a exceção histórica da guarda permite esses saves** (`app/ponto/models.py:882–889`).
- Anexo: save é protegido apenas para limpeza e re-raise (`app/ponto/services/ausencia.py:859–865`); chamada da UI sem catch (`app/ponto/views.py:2151`) e reenvio da API fora do try de criação (`app/api/views.py:1848–1850`). Mesmo limite de concorrência para linha legada.
- Lançar/submeter pela web capturam `Exception` (`app/ponto/views.py:2458`, `:2558`); não classificá-los genericamente como 500.
- Django admin não escreve Ausência: usa `SoLeitura` (`app/ponto/admin.py:37–47`), com add/change/delete negados (`app/core/admin.py:59–66`).

**Batida:** não encontrei caminho produtor de Ausência nos receivers examinados; o chokepoint cria `Batida`, não `Ausencia` (`app/ponto/registro_batida.py:152–160`). Pós-evento protegido por catch genérico (`:175–183`). Núcleo recompõe somente `TurnoMaterializado` (`app/ponto/nucleo.py:40–67`), protegido no signal (`app/ponto/signals.py:19–31`). Cartório da batida entra em `engolir` (`app/ponto/signals.py:85–93`, `:131–135`); reconciliador e adesão protegidos separadamente (`:206–237`). Realtime é protegido internamente (`app/ponto/realtime.py:22–46`). Onboarding grava `Colaborador` (`app/colaboradores/signals.py:86–102`); seu signal downstream trata silêncio de chamados, não ausência (`app/chamados/signals.py:268–279`). **Nenhuma barreira nova de Ausencia à batida demonstrada nesses caminhos.**

O mapeamento não prova ausência de qualquer outra falha antiga na batida: onboarding não tem catch próprio (`app/colaboradores/signals.py:101–102`); isso não é efeito da guarda nova.

Férias: espelho usa `criar_ausencia(tipo='ferias')`, captura só domínio e registra recusas (`app/ponto/services/ausencia.py:1201–1216`); `AgendamentoFerias.save` chama sync após salvar agendamento (`app/ferias/models.py:446–451`). A criação com `ferias` inativo já era recusada no precheck; só uma mudança entre precheck/save introduz o novo VE bruto. Remarcação da view captura apenas `AgendamentoNaoRemarcavel` (`app/ferias/views.py:686–690`).

## 11/11 15:36 · sessao 61e4e870505c · - · - · origem=Ronald (tmux)

Fixtures preservam a mordida original na leitura estática:

- **Carimbo:** continua usando `obito` legado, depois executa `save()` de aprovação/rejeição e exige `aprovada_em`; o estado pendente continua exigindo ausência do carimbo. A mudança foi na montagem da linha, sem retirar asserções. `app/ponto/tests/test_decisao_carimba.py:16-43`.
- **Espelho:** mantém `obito` e o compara com o status lavrado pelo juiz; preserva verificações de turnos, batidas e resumo. `app/ponto/tests/test_caracterizacao_espelho.py:48-55`, `:89-101`.
- **Fechamento:** mantém o desconhecido `atestado_medico` e exige DSR perdido igual a zero; os testes de ordem entre falta e atestado continuam presentes. `app/ponto/tests/test_fechamento_ausencia_pela_autoridade.py:57-87`.
- **Cadastro/dropdown:** apenas acrescentam limpeza do cache ao fim; continuam exigindo admissão imediata de código novo, remoção do inativo e fallback quando a tabela está vazia. `app/ponto/tests/test_cadastro_tipo.py:11-37`; `app/ponto/tests/test_dropdown_vivo.py:16-49`.
- **Diferença de fixture:** agora há um primeiro julgamento com `abono`, seguido de julgamento com legado. Os receivers de Ausencia não distinguem `created`, portanto o segundo `save()` executa seus consumidores; equivalência completa de todos os efeitos intermediários [não verificado]. `app/ponto/tests/test_caracterizacao_espelho.py:50-55`; `app/ponto/signals.py:143-147`, `:275-279`.

O selo novo tem asserções que falhariam sem a guarda: rejeita criação direta, `objects.create`, troca de vivo para morto e troca entre mortos; também protege história, desativação posterior e código vivo fora de choices. `app/ponto/tests/test_lastro_folga_save_guarda.py:73-142`. Execução verde, RED histórico e mutantes efetivamente executados [não verificado].

Lacunas relevantes:

1. **Corrida:** leitura do tipo e persistência não estão protegidas pela própria guarda com lock/transação. Interleaving possível: A lê legado `folga`; B corrige para `abono`; A passa na comparação e salva novamente `folga`. Ocorrência em produção [não verificado]. `app/ponto/models.py:879-893`, `:933`.
2. **`update_fields` não contorna:** a guarda roda antes de analisar os argumentos do `save`; até atualização apenas de descrição valida o `self.tipo` em memória e consulta o legado. Pode recusar uma alteração em memória que nem seria persistida. `app/ponto/models.py:891-893`, `:933`.
3. **`QuerySet.update` contorna de fato:** a própria fixture demonstra a gravação do tipo morto, seguida de leitura confirmando o valor. `app/ponto/tests/test_lastro_folga_save_guarda.py:63-69`.
4. **`raw`/bulk:** semântica de `save_base(raw=True)` e de bulk não foi confirmada no código instalado, pois Django não está disponível no host; dependência declarada é Django 5.2.14. `app/requirements.txt:13`; comportamento executado [não verificado].
5. **Multi-db:** a guarda ignora `using` na consulta histórica. As configurações examinadas só declaram alias `default`, portanto impacto real multi-db [não verificado]. `app/ponto/models.py:883-884`; `app/config/settings/saas.py:83-93`; `app/config/settings/prod.py:9-18`.
6. **Custo:** fora do vivo e com instância persistida, há uma consulta extra ao tipo; com cache expirado também há leitura do catálogo. O `Meta.ordering` inclui `colaborador__nome`, e a consulta histórica não remove a ordenação; SQL efetivamente emitido [não verificado]. `app/ponto/models.py:819`, `:882-884`; `app/ponto/catalogo/ausencias.py:249-252`, `:275-280`.

Os novos testes não exercitam concorrência, `update_fields`, processos/tenants diferentes ou bulk/raw. `app/ponto/tests/test_lastro_folga_save_guarda.py:73-142`.

## 11/11 15:36 · sessao 05d6cdb86f93 · - · - · origem=Ronald (tmux)

- **(d) curado no desenho do ensaio:** a marca de fila ocorre em `_fimTx`, imediatamente antes de `oncomplete`; `add` apenas anota linha provisória; `abort` desfaz essa linha; o verificador exige promessa resolvida e exatamente uma linha persistida. `tx_lento` e Mi2 detectam gravação tardia quando o aborto é retirado. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:142`, `:152`, `:178`, `:203`, `:605`, `:613`, `:823`.) Execução verde nesta cópia: [nao verificado].

- **Limite da prova:** o falso IndexedDB abre em 1 ms e `close()` é vazio; `abort()` só lança quando `_feito` já está verdadeiro. Portanto não prova fechamento de conexão tardia nem a situação “commit iniciado, aborto lança, `oncomplete` ainda pendente”. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:195`, `:204`, `:210`, `:215`.)

- **As quatro guardas sem mutante:** os mutantes existentes cobrem abertura/teto/bloqueio, transação/teto/aborto, guarda externa do gap, POST e corpo de erro; não há mutante para as quatro guardas indicadas. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:810`, `:830`, `:842`.)  
  - `db.close()` tardio protege futuras aberturas/versões; a abertura atual já rejeitou. (`app/static/pwa.js:60`, `:72`.)  
  - Catch interno do gap não é indispensável à fila, pois esta envolve `obterGap()` em `_noTeto`, que absorve rejeição. (`app/static/pwa.js:134`, `:166`, `:213`.)  
  - `.catch` do indicador ocorre depois da transação de gravação e sem `await`; sua retirada gera rejeição acessória, não desfaz a fila. (`app/static/pwa.js:216`, `:223`.)  
  - Instante único está correto no código, alimentando ambos os timestamps, mas falta prova de regressão temporal. (`app/static/pwa.js:212`, `:214`, `:218`.)  
  Não demonstrei bloqueante de produto **só** pela ausência desses quatro mutantes; a prova para “aborto lança durante commit” permanece ausente. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:204`; `CLAUDE.md:375`.)

- **Referências ao apagado:** a busca literal encontra somente a lápide no guard, comentário histórico de teste e tabela de órfãos; nenhum desses executa render/include. (`app/scripts/check_a_lei_batida.py:20`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:673`; `app/docs/TICKETS.md:693`.) As URLs vivas continuam apontando à view; ela renderiza v2 ou um template da triagem. (`app/ponto/urls.py:11`; `app/ponto/urls.py:12`; `app/ponto/views.py:225`; `app/ponto/views.py:356`.)

- **SW:** guarda URLs, não nomes de templates; `/ponto/bater/` permanece na lista. Para navegação, porém, o ramo `navigate` retorna antes do bloco que alimenta esse cache; é comportamento presente na cópia, não referência quebrada causada pela exclusão. (`app/static/sw.js:45`, `:50`, `:67`, `:74`.)

- **Ferramentas alteradas:** `check_frontend_urls.py` lista somente pwa.js, sw.js e dashboard antigo; apto/v2 ficam fora dessa cobertura. O guard da Lei retirou o arquivo morto, mas não incluiu v2 e conserva caminhos `app/apps/ponto/views.py`, pulando arquivos inexistentes. São lacunas desses instrumentos; não provam quebra de rota nem novo bloqueio de selfie. (`app/tools/check_frontend_urls.py:25`; `app/scripts/check_a_lei_batida.py:14`, `:20`, `:21`, `:31`.)

Não executei o Chromium: o próprio ensaio cria arquivos/perfil e os apaga, contrariando a regra somente leitura. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:344`, `:351`, `:360`, `:363`, `:372`.)

## 11/11 15:36 · sessao 4ab2dacb6e2c · - · - · origem=Ronald (tmux)

(2) Há **até um POST inicial mais uma linha de fila por clique online**; a linha pode provocar novos POSTs sem limite declarado enquanto a resposta/remoção falhar. Isso não equivale ao mesmo número de linhas `Batida`: a sincronização mantém a linha em erro e só a remove após resposta 2xx e transação de exclusão completa (`app/templates/ponto/bater_ponto_v2.html:307-331`; `app/static/pwa.js:256-298`).

- O chokepoint deduplica **mesmo colaborador e mesmo tipo, ±60 s inclusivos**, entre batidas não retratadas, comparando o `timestamp` passado à porta (`app/ponto/registro_batida.py:106-115`).
- A API deduplica **mesmo colaborador e mesmo tipo, ±5 min inclusivos**, sobre `ts_efetivo`, e responde `ja_registrada`, sem criar outra linha. Não é janela sobre horário da chegada quando o aparelho forneceu hora válida (`app/api/views.py:518-555`).
- Portanto, **reenvio sequencial, mesmo tipo, diferença de até 30 s e primeira gravação já visível** cabe nas duas janelas. A falta de identidade original, isoladamente, não prova duplicação nesse cenário (`app/static/pwa.js:230-240`; `app/api/views.py:539-555`; `app/ponto/registro_batida.py:106-115`).
- A garantia universal fica **[nao verificado]**: a API deduplica antes de eventualmente corrigir o tipo; a consulta usa o tipo recebido, enquanto a primeira gravação pode ter usado outro (`app/api/views.py:539-558`; `app/api/views.py:578-613`). Há também vários gatilhos de sincronização e nenhuma trava na entrada da função mostrada (`app/static/pwa.js:16`; `app/static/pwa.js:244-253`; `app/static/pwa.js:441-449`).
- Com `tx.abort()` lançando no teto, o código rejeita mesmo assim; ele não consegue assegurar que aquela transação não concluirá posteriormente. Isso pode deixar uma linha que será reenviada apesar do erro mostrado, mas não demonstra duas linhas no servidor, devido às deduplicações acima (`app/static/pwa.js:84-93`; `app/static/pwa.js:216-223`; `app/templates/ponto/bater_ponto_v2.html:329-331`).

(3) A lei copiada declara batida de chão sem barreira e escrita por `ponto/registro_batida.py` (`CLAUDE.md:143-144`). Os caminhos examinados consultam o juiz central de câmera/captura e seguem com foto nula; não encontrei segundo juiz de admissibilidade da selfie nesses caminhos (`app/static/pwa.js:170-204`; `app/templates/ponto/triagem/apto.html:248-264`; `app/templates/ponto/triagem/apto.html:275-289`; `app/templates/ponto/bater_ponto_v2.html:269-284`). No servidor, ausência de foto consulta `selfie_ausente` e acrescenta motivo, sem retorno de recusa (`app/api/views.py:449-465`; `app/ponto/registro_batida.py:80-91`). Existem barreiras anteriores de situação e colisão oposta; não são introduzidas por esse delta (`app/api/views.py:415-423`; `app/ponto/registro_batida.py:145-151`).

(4) O escritor lê `Date.now()` uma vez **na entrada de `registrarBatidaOffline`**, antes do gap e do IndexedDB; grava esse instante cru em `timestamp_dispositivo` (`app/static/pwa.js:211-222`). Isso não é necessariamente o instante do primeiro clique: na v2 online, a chamada acontece depois do POST sem resposta, que pode consumir 30 s (`app/templates/ponto/bater_ponto_v2.html:293-323`; `app/static/pwa.js:230-237`).

A fila envia `timestamp_dispositivo`; não envia seu `timestamp` corrigido nem o gap (`app/static/pwa.js:261-270`). O leitor é `api_bater_ponto`, que passa esse campo a `hora_do_aparelho`; sem campo, vale `agora` (`app/api/views.py:509-521`; `app/api/hora_do_aparelho.py:135-139`). A função aceita a hora do aparelho com atraso de **0 a 48 h**, usando-a como hora efetiva; acima de 3 min acrescenta trilha de offline aceito. Hora futura ou antiga demais usa `agora`, preservando a hora lida no campo do aparelho (`app/api/hora_do_aparelho.py:62-67`; `app/api/hora_do_aparelho.py:141-153`). Assim, atraso de 30 s é aceito sem descarte quando o relógio está correto (`app/api/hora_do_aparelho.py:141-146`).

## 11/11 15:35 · sessao 36452142e014 · DATA-EVENTO-CRU · verificador · origem=Code

**Veredito: bloqueantes — há leitores reais fora do censo e ocorrências de escrita/schema registradas como leitores.** (`app/ponto/management/commands/vigia_plantio_overnight.py:75`; `app/chamados/models.py:2789`; `app/chamados/models.py:3486`)

1. **O selo MORDE nas formas reconhecidas, mantendo o JSON fixo.** Chave nova, aumento em escopo existente e redução sem baixar a allowlist ficam vermelhos; desaparecimento também exige baixa. Isso vale para Python e templates. Confirmei esses cinco cenários em memória usando a comparação existente. (`app/chamados/tests/test_catraca_data_evento_cru.py:133`)

   O teste denominado `test_MORDE_leitor_novo_no_censo` prova descoberta, mas não executa a comparação que reprova o leitor novo. Não executei esse caso, pois cria e apaga arquivos temporários. (`app/chamados/tests/test_catraca_data_evento_cru.py:178`)

   A catraca compara apenas com o JSON atual: aumentar código **e** allowlist juntos pode passar; não há comparação com o baseline anterior. Substituir uma leitura por outra no mesmo escopo também preserva a contagem, limite já declarado. (`app/chamados/tests/test_catraca_data_evento_cru.py:134`; `app/chamados/tests/test_catraca_data_evento_cru.py:26`)

2. **Falsos negativos.** O varredor reconhece `getattr` literal, `values/values_list/only('data_evento')`, `F/OuterRef('data_evento')`, `filter(data_evento__...)`, `annotate(d=F(...))`, subscrito literal e `.get('data_evento')`. Um `{% with d=p.data_evento %}` também entra. (`app/chamados/tests/test_catraca_data_evento_cru.py:82`; `app/chamados/tests/test_catraca_data_evento_cru.py:174`)

   Escapam nomes calculados, argumentos expandidos por `*`/`**`, campos declarados em listas/tuplas, `source='data_evento'`, serialização genérica e template `|attr`. A detecção depende de atributo, subscrito literal, argumento posicional literal ou keyword que comece pelo campo. (`app/chamados/tests/test_catraca_data_evento_cru.py:23`; `app/chamados/tests/test_catraca_data_evento_cru.py:82`)

   **Leituras verdadeiras omitidas hoje:**

   - `pergunta_origem__data_evento__isnull=False` lê o campo pela relação, mas não casa com a regex ancorada em `data_evento`. (`app/ponto/management/commands/vigia_plantio_overnight.py:75`; `app/chamados/tests/test_catraca_data_evento_cru.py:50`)
   - A chave da família marco contém `data_evento`, usado por `values(*chave)`, `order_by(*chave)`, `g[k]` e `filter(**dict(...))`; essas ocorrências escapam. (`app/chamados/services/fio.py:1373`; `app/chamados/services/fio.py:1948`; `app/chamados/services/fio.py:1949`; `app/chamados/services/fio.py:1950`; `app/chamados/services/fio.py:1970`)
   - O lookup dinâmico de nulidade também escapa. (`app/chamados/services/fio.py:1378`; `app/chamados/services/fio.py:1946`)
   - Fora de `app/`, mensageria lê o valor serializado com `i.get('data_evento')`; essa exclusão está declarada. (`mensageria/nucleo/ferramentas.py:127`; `app/chamados/tests/test_catraca_data_evento_cru.py:25`)

   Existência atual de serializer ou template `|attr` lendo esse campo: **[nao verificado]**.

3. **Falsos positivos.** Atribuição simples e `create(data_evento=...)` são excluídos; uma leitura no lado direito continua contando. (`app/chamados/tests/test_catraca_data_evento_cru.py:48`; `app/chamados/tests/test_catraca_data_evento_cru.py:82`)

   Contudo, qualquer outra chamada com keyword `data_evento=` conta. Assim, `abrir` e `abrir_conversa` entram apenas por encaminharem o valor ao escritor, sem ler campo de pergunta. (`app/chamados/tests/test_catraca_data_evento_cru.py:96`; `app/chamados/models.py:2789`; `app/chamados/models.py:2801`)

   Os dois `Q` internos às declarações de índices também contam, apesar da exclusão de `UniqueConstraint`: a recursão visita seus filhos. A allowlist registra essas duas ocorrências em `PerguntaDisputa.Meta`. (`app/chamados/tests/test_catraca_data_evento_cru.py:99`; `app/chamados/models.py:3486`; `app/chamados/models.py:3492`; `app/chamados/tests/catraca_data_evento_cru.json:39`)

   Não há resolução de modelo: um atributo homônimo de outro objeto seria contado igualmente. Outro modelo concreto afetado hoje: **[nao verificado]**. (`app/chamados/tests/test_catraca_data_evento_cru.py:82`)

4. **Juiz excluído pelo nome exato.** A exclusão é `out.pop(JUIZ, 0)`. Renomeá-lo faz surgir chave nova e deixa a contagem do juiz isento em zero, reprovando também a prova de existência; não passa vazio. (`app/chamados/tests/test_catraca_data_evento_cru.py:44`; `app/chamados/tests/test_catraca_data_evento_cru.py:127`; `app/chamados/tests/test_catraca_data_evento_cru.py:146`)

5. **JSON e contratos.** O leitor é `test_so_desce`; a mensagem exige baixa manual. Escritor automatizado/autoria histórica: **[nao verificado]**. (`app/chamados/tests/test_catraca_data_evento_cru.py:134`; `app/chamados/tests/test_catraca_data_evento_cru.py:143`)

   Não há colisão nos enumeradores examinados: O20 seleciona `test_contract_*.py`; P13 exclui `tests/` e exige `.py`; o selo de relógio seleciona `test*.py`, excluindo o JSON. Ausência universal de colisões na suite: **[nao verificado]**. (`app/core/tests/test_o20_contrato_julga_codigo.py:164`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:147`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:150`; `app/core/tests/test_selo_teste_sem_relogio.py:53`)

   O JSON soma **217 ocorrências em 120 chaves: 212 Python + 5 templates**, coincidindo com o censo implementado. Publicação desse total no BACKLOG/ESTADO: **[nao verificado]**. (`app/chamados/tests/catraca_data_evento_cru.json:2`; `app/chamados/tests/catraca_data_evento_cru.json:119`)

6. **Classes anteriores de reprovação.** Há mordida efetiva na comparação, porém cobertura incompleta nas formas acima. Os arquivos novos implementam contagem e comparação, sem introduzir juiz do dia, leitura de relógio ou derivação paralela do dia. Execução da suite Django/pre-push neste hash: **[nao verificado]**. (`app/chamados/tests/test_catraca_data_evento_cru.py:68`; `app/chamados/tests/test_catraca_data_evento_cru.py:112`; `app/chamados/tests/test_catraca_data_evento_cru.py:133`)

**Bloqueantes para aprovação:** incluir os leitores reais omitidos e retirar os repasses de escrita/schema do censo, recalibrando a allowlist. (`app/ponto/management/commands/vigia_plantio_overnight.py:75`; `app/chamados/services/fio.py:1948`; `app/chamados/models.py:2789`; `app/chamados/models.py:3486`)

## 11/11 15:34 · sessao 97538cdfafe8 · - · - · origem=Ronald (tmux)

Inventário estático de `select_for_update`: **23 sites fora de testes/migrations; 18 com alcance por código até Pauta DP**, dos quais **16 têm borda produtiva identificada e 2 pertencem às portas editar/trocar usadas só pelos testes encontrados**. Contagem por AST de `Call(Attribute(..., 'select_for_update'))`; inclui as linhas enumeradas abaixo. A execução do censo não deixou arquivo de saída: **[nao verificado]**. [nao verificado]

| Recurso / função | Sites de aquisição | Ordem e alcance até DP |
|---|---|---|
| Colaborador, Disputa, Chamado, Pergunta — `PerguntaDisputa.trava_na_ordem` | `app/chamados/models.py:2666`, `:2675`, `:2688`, `:2692` | **C → D → Ch → P**, cada conjunto por PK; `_gravar_estado` pede universo antes de salvar, e o save acorda cartório/fábrica. `app/chamados/models.py:290`; `app/chamados/models.py:296`; `app/chamados/signals.py:217`; `app/ponto/services/cartorio.py:1020`. |
| FechamentoMensal — command pago/devido | `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196` | Atomic `:189` → C `:193` → FM `:196` → Pauta cabeça `:207` → DP → filhas `:211` → DP. |
| Justificativa — aprovação/rejeição/materialização | `app/ponto/services/justificativa.py:142`, `:277`, `:344`; `app/ponto/views.py:1794` | **C/D/Ch antes da Justificativa**, pelos pedidos `:141`, `:276`, `:343` e view `:1793`; depois save de J → signal cartório → fábrica → DP. `app/ponto/services/justificativa.py:208`; `app/ponto/services/justificativa.py:287`; `app/ponto/signals.py:165`; `app/ponto/services/cartorio.py:1020`. |
| AgendamentoFerias e PeriodoAquisitivo — `_travar_agendamento` | `app/ferias/services.py:364`, `:367` | Ag → PA → Ausencia por UPDATE/DELETE/INSERT → cartório ou pauta de férias recusadas → DP. `_trocar_janela`: atomic `:433`, locks `:434`, exclusão `:445`, save `:450`; sync em `app/ferias/models.py:449` e `:451`; pauta direta em `app/ponto/services/ausencia.py:1216` e `:1245`. |
| AgendamentoFerias — alinhamento por data | `app/ferias/services.py:600` | Atomic `:599` → Ag → save de status `:604` → sync Ausencia `app/ferias/models.py:451` → cartório/DP, ou pauta de recusa `app/ponto/services/ausencia.py:1245`. |
| SolicitacaoFerias — view avaliar | `app/ferias/views.py:740` | Atomic `:739` → Solicitação → aprovação `:753` → criação/save Agendamento `app/ferias/services.py:503` → sync Ausencia `app/ferias/models.py:451` → DP; depois UPDATE PA `app/ferias/services.py:506`. |
| DecisaoHE — decisão/período | `app/ponto/portas/he.py:174`, `:555` | Prévia atomic `app/ponto/services/autorizacao_he_periodo.py:321` ou confirmação `:343` → `_ato` → `autorizar_em_lote` `:277` → HE `app/ponto/portas/he.py:555` → decisão HE `:562`/`:174` → pauta DP `app/ponto/services/autorizacao_he_periodo.py:300`/`:244`. |
| Ausencia — rederivação H06 | `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:138` (chamada continua em `:139`) | Atomic `:137` → Ausencia → editar `:169` → save `app/ponto/services/ausencia.py:509` → signal `app/ponto/signals.py:147` → cartório/fábrica/DP. |
| CelulaDia — editar dia | `app/ponto/portas/celula.py:593` | Atomic `:592` → Celula → órfãs `:626` → `app/ponto/management/commands/gerar_celulas.py:255` → contexto `app/chamados/services/contexto.py:103` → C/Ch `app/chamados/models.py:486`. Julgamento/DP vem após saída desse atomic, em `app/ponto/portas/celula.py:627` e `:434`; fica na mesma transação quando herda o atomic de trocar `:652`. Chamador encontrado: `app/ponto/portas/celula.py:675`; bordas externas encontradas só testes, por exemplo `app/ponto/tests/test_o10b_editar_dia_troca.py:116`. |
| Ausencia — trocar plantão | `app/ponto/portas/celula.py:657` | Atomic `:652` → Ausencias por PK `:657` → células por colaborador/data `:673` → editar `:675` → C/Ch por órfãs → julgamento → DP. Uso identificado só em teste, por exemplo `app/ponto/tests/test_o10b_editar_dia_troca.py:116`; chamador produtivo **[nao verificado]**. |

**Cinco sites excluídos do alcance DP no mesmo atomic lido:**

| Sites | Razão |
|---|---|
| `app/ponto/portas/he.py:318`, `:445` | Recusar ponta/revogar têm os commands próprios como chamadores encontrados; não alcançam a pauta do ato de período. Chamadas: `app/ponto/management/commands/recusar_ponta_pequena.py:170`; `app/ponto/management/commands/lavrar_he_pendente.py:59`. |
| `app/core/services/aplicacao_convencao.py:115` | O atomic cadastra/altera AplicacaoConvencao e grava trilha; retorna em `:124`/`:134`. Não foi encontrado receiver que leve esse model ao cartório; alcance indireto **[nao verificado]**. |
| `app/holerite/management/commands/processar_lote_holerite.py:54`, `:124` | Atomics cuidam de estado do lote; chamada ao motor fica fora deles, em `:89`. Alcance de DP dentro desses dois atomics **[nao verificado]**. |

| Advisory | Aquisição / duração |
|---|---|
| Âncora E4B | `app/chamados/services/fecho_runtime.py:89`: `pg_advisory_xact_lock`, dentro atomic `:119`; todas as âncoras ordenadas em `:120`, antes de escrever `:127`. |
| Vigia de hora | `app/ponto/services/vigia_de_hora.py:99`: `pg_try_advisory_lock`, sessão; falha retorna em `:100`/`:101`; julgamento em `:131`, unlock em `:136`. Não é aquisição bloqueante; não há atomic amplo em `rodar`. |
| Consumidor de eventos | `app/chamados/reconciliador.py:1235`: `pg_try_advisory_lock`, sessão; libera em `:1242`. O consumidor usa atos separados por chamado, sem atomic de lote, conforme implementação/chamadas `:1364`, `:1365`, `:1368`; delimitação descrita em `:1325`/`:1326`. |

**Escritas que também precisam entrar na sequência, embora não sejam `select_for_update`:**

- `Pauta INSERT → trilha → Contador.get_or_create → Contador UPDATE`: `app/pautas/services.py:180`, `:186`, `:188`, `:352`, `:354`. `lavrar` **não tem decorador atomic** em `:344`; chamado avulso em autocommit não conserva DP entre alvos; comportamento efetivo do banco **[nao verificado]**.
- Pauta existente UPDATE → trilha → DP: ler `app/pautas/services.py:215`/`:218`; fazer `:247`/`:251`; fazer pelo sistema `:272`/`:276`; cancelar `:294`/`:297`.
- Cartório escreve Celula antes de chamar a fábrica: `app/ponto/services/cartorio.py:1845` → `app/ponto/portas/celula.py:1072` → atomic/save `:904`/`:905`; depois fábrica `app/ponto/services/cartorio.py:1020`. Na lavra do cartório, o argumento é `save=False` em `app/ponto/services/cartorio.py:1825`, portanto citar somente `celula.py:1040` não prova o UPDATE desse caminho.
- Núcleo de Batida: signal `app/ponto/signals.py:36` → `app/ponto/nucleo.py:57` → DELETE de TurnoMaterializado `:58`/`:81` → bulk INSERT `:59`; o cartório posterior do mesmo save vem em `app/ponto/signals.py:135`.
- Invalidar FM é **síncrono**: `app/ponto/services/fechamento.py:1656`. Exemplo DP pode precedê-lo: aprovação `app/ponto/services/justificativa.py:208` → signal/cartório/fábrica → DP → `invalidar_fechamento` `:215`. O recálculo, separadamente, é `on_commit` em `app/ponto/services/fechamento.py:1824`.
- Passivo: atomic do par `app/chamados/services/passivo_validado.py:371` → plantio `:428` → regeneração exportada `:438` → outer da porta `app/ponto/portas/regen_exportada.py:169` → regeneração `:174` → FM `:179` → DiaPago `:188` → Pauta passivo `app/chamados/services/passivo_validado.py:443`. A regeneração pode acionar cartório/fábrica antes do FM por `app/ponto/portas/celula.py:354`/`:434`.
- Matrícula sem número pede C do maior matriculado em `app/colaboradores/models.py:436`; seu atomic `:429` termina antes do `super().save` `:439`, salvo atomic herdado. Alcance DP naquele atomic isolado **[nao verificado]**.

**Pares e limites:**

| Par | Avaliação estática |
|---|---|
| DP → âncora / âncora → DP | Inversão demonstrada no runtime pelo ato externo: transição antes do agrupador `app/chamados/services/fecho_runtime.py:151`/`:154`; fábrica DP `app/chamados/services/fabrica_da_celula.py:369` ou `:279`; agrupador âncora `app/chamados/services/fecho_runtime.py:121` → DP `:127`/`app/pautas/services.py:354`. Recursos compartilháveis entre colaboradores diferentes pela âncora empresa/competência `app/chamados/services/fecho_runtime.py:116` e contador por departamento `app/pautas/services.py:352`. |
| C → Celula / Celula → C | Oposto efetivo de transição: C `app/chamados/models.py:2666` → cartório `app/chamados/signals.py:217` → Celula `app/ponto/portas/celula.py:905`. Inverso em trocar: outer `app/ponto/portas/celula.py:652` → Celula `:593` → órfãs `:626` → C `app/chamados/models.py:486`. Código permite o par para mesmos C/Celula; chamador de trocar encontrado só testes `app/ponto/tests/test_o10b_editar_dia_troca.py:116`. Runtime concorrente **[nao verificado]**. |
| DP → FM / FM → DP | Há as duas ordens **de classes**: aprovação J `app/ponto/services/justificativa.py:208`/`:215` e pago/devido `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`/`:207`. **Não certificar deadlock desse par:** aprovação já toma C em `app/ponto/services/justificativa.py:141`/`:129`, e pago toma o mesmo C antes do FM em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`. Para o mesmo FM, esse C serializa os dois caminhos; outro ato sem esse serializador **[nao verificado]**. |
| Regeneração Celula → C contra transição C → Celula | Regeneração tem UPDATE Celula `app/ponto/portas/celula.py:276` antes de órfãs/C `:289`; entretanto cartório vem depois do atomic local `:354`. A porta exportada conserva ambos no outer `app/ponto/portas/regen_exportada.py:169`/`:174`, mas seu único chamador produtivo encontrado é passivo `app/chamados/services/passivo_validado.py:438`, cujo plantio já carimba pela porta em `app/chamados/services/materializacao.py:1277` antes da regeneração. Não certificar aquisição nova de C nem ciclo runtime sem resolver essa pré-trava. |
| Justificativa → C / C → Justificativa | Não demonstrado no código lido: as quatro bordas pedem C/D/Ch antes de J (`app/ponto/services/justificativa.py:141`, `:276`, `:343`; `app/ponto/views.py:1793`). |

Correção ao meu informe intermediário: `rejeitar_ausencias_de_ferias` usa UPDATE sem signal (`app/ponto/services/ausencia.py:1281`), portanto o alcance cartório/DP do cancelamento-admin vem do save posterior do Agendamento (`app/ferias/services.py:417` → `app/ferias/models.py:451`), que remove Ausencias e dispara `post_delete` (`app/ponto/services/ausencia.py:1190`; `app/ponto/signals.py:152`).

## 11/11 15:34 · sessao b65421d650d3 · - · - · origem=Ronald (tmux)

Achados para consolidação:

- **Falso negativo atual, sem reflexão:** `Batida.objects.filter(pergunta_origem__data_evento__isnull=False)` consulta o campo de `PerguntaDisputa`, mas o prefixo relacional impede casar com `^data_evento...$`. O vínculo de `pergunta_origem` é `PerguntaDisputa`. — `app/ponto/management/commands/vigia_plantio_overnight.py:73-75`; `app/ponto/models.py:41`; `app/chamados/tests/test_catraca_data_evento_cru.py:50,97`.
- **Leituras dinâmicas atuais omitidas:** a chave da família MARCO inclui `'data_evento'`; `grupos_de_colisao` a usa em `values(*chave)`, `order_by(*chave)`, `g[k]` e `filter(**dict(zip(chave,ch)))`. Nenhuma dessas ocorrências casa com os predicados de constantes diretas do varredor. O queryset vem de `PerguntaDisputa`. — `app/chamados/services/fio.py:1373,1430-1433,1945,1948-1950,1970`; varredor `app/chamados/tests/test_catraca_data_evento_cru.py:64-65,84-98`.
- **Filtros dinâmicos atuais omitidos:** `_NULO_FORA_DO_INDICE['marco']` é `'data_evento__isnull'`; seus usos em `exclude(**{...})` e `filter(**{...})` ficam fora. — `app/chamados/services/fio.py:1378,1946,2000`; varredor `app/chamados/tests/test_catraca_data_evento_cru.py:96-98`.
- `getattr(p,'data_evento')`, `values/values_list/only('data_evento')`, `F/OuterRef('data_evento')`, `filter(data_evento__...)`, `annotate(d=F('data_evento'))`, `d['data_evento']` e até `d.get('data_evento')` **são vistos**: constantes posicionais de quaisquer chamadas não excluídas entram. — `app/chamados/tests/test_catraca_data_evento_cru.py:82-98`.
- `fields=['data_evento']`, `source='data_evento'`, constantes em tuplas/listas passadas por `*`, argumentos por `**`, `getattr(p,nome)` e `|attr` **não são vistos**; presença atual de serializers ou templates `|attr` de `PerguntaDisputa`: **[nao verificado]**. Os limites de reflexão/serialização/`|attr` já estão declarados. — `app/chamados/tests/test_catraca_data_evento_cru.py:23-25,64-65,89-98,105-109`.
- `{% with d=q.data_evento %}` **é visto**, inclusive no teste sintético. — `app/chamados/tests/test_catraca_data_evento_cru.py:174-176`.
- Fora de `app/`, existe consumo do valor serializado: `mensageria/nucleo/ferramentas.py:127` lê `i.get('data_evento')`; a API fornece o campo de `p` em `app/api/views_mensageria.py:568`. Esse serviço está explicitamente fora do censo. — `app/chamados/tests/test_catraca_data_evento_cru.py:25,41,112`; transporte `mensageria/nucleo/core_client.py:42-55`.
- A única declaração de campo modelo encontrada é `PerguntaDisputa.data_evento`; **não há evidência para afirmar segundo modelo existente**. O comentário diz “UM modelo”, sem dizer “outro”. — `app/chamados/models.py:2406,2423`; `app/chamados/tests/test_catraca_data_evento_cru.py:26`. Outro modelo hoje: **[nao verificado]**.
- Os `getattr` dinâmicos de histórico e reversão não demonstram leitores omitidos de `data_evento`: suas listas não incluem o campo. — `app/chamados/services/historico.py:98-105`; `app/chamados/services/passivo_validado.py:64-66,83`.

_3966 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
