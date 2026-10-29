# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 18:22** · publicado em **29/10/2026 18:23** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 18:22 · sessao 0e25a18be1e0 · justif · sonda · origem=Code

Números conforme o dado raspado; confirmação independente em produção: [nao verificado].

- **Classe:** 98 justificativas aprovadas com origem viva. **Alvo em competência aberta:** 45, esperado 45; fora do aval: 0. [nao verificado]
- **Exportadas / Pauta DP:** empresa 2 × 09/2026: **46**; empresa 3 × 09/2026: **6**; empresa 4 × 09/2026: **1**. Total: **53**; as duas apresentações da pauta coincidem. [nao verificado]
- **Guarda `irma_orfa`: 5 dos 45 itens**, com **7 perguntas pendentes**, distribuídas assim: [nao verificado]
  - 2449 → 34824. [nao verificado]
  - 2748 → 39729. [nao verificado]
  - 3201 → 48775, 48776, 48777. [nao verificado]
  - 3273 → 50091. [nao verificado]
  - 3301 → 50392. [nao verificado]

No código, essa guarda consulta perguntas respondidas, não validadas, não materializadas e com resposta não vazia; havendo pendências, impede a retratação. [app/chamados/juizes.py:1471](/home/ronald/codex_ws/cbedeb91b8bf7eeb460546b8cb69b408a01d3bb3/app/chamados/juizes.py:1471), [app/chamados/reconciliador.py:435](/home/ronald/codex_ws/cbedeb91b8bf7eeb460546b8cb69b408a01d3bb3/app/chamados/reconciliador.py:435), [app/chamados/reconciliador.py:454](/home/ronald/codex_ws/cbedeb91b8bf7eeb460546b8cb69b408a01d3bb3/app/chamados/reconciliador.py:454).

As conferências internas fecham: **45 + 53 = 98**; a lista de perguntas-destino contém **45 itens**, somando **10 respondidas e 3 validadas**, diferença **7**, igual à quantidade de perguntas retidas. O dry informa **45 `True`**; a prova de origem viva informa **45**; os vereditos somam **44 + 1 = 45**, todos com antes igual ao depois. **Nenhuma incoerência aritmética aparente.** [nao verificado]

Também constam: M1 já encerrado **92**, em **2026-10-26**; exportadas vigentes intactas **`True (7)`**; **`ERROS=0`** e presença de **`FIM-DA-SONDA`**. O universo do `(7)` não está explicitado, portanto não cabe equipará-lo aos 53 itens exportados. [nao verificado]

A retenção por **`justificativa_alheia` não foi quantificada no anexo**: há somente referência à medição em sombra. Seu resultado permanece [nao verificado].

## 29/10 18:20 · sessao 311e93877dc2 · justif · verificador · origem=Code

**Não aprovaria esta versão:** a retenção pode ser contornada, há ciclos de trava e a materialização pode gerar duas transições para o mesmo ato. (`app/ponto/services/cartorio.py:1163`; `app/ponto/services/justificativa.py:69`; `app/ponto/services/justificativa.py:379`)

As conclusões são por leitura da cópia e análise AST em memória. Reprodução funcional, concorrência em banco e REDs citados nos comentários: **[nao verificado]**.

**1. Defeitos e contraexemplos**

- **Retenção não alcança todo fechamento do ato — outra: guarda contornada.** A regra nova é consultada por `_retratar` e pela cascata, mas o cartório chama `reconciliar()` diretamente; o gate do modelo não consulta essa retenção. (`app/chamados/reconciliador.py:474`; `app/ponto/services/justificativa.py:377`; `app/ponto/services/cartorio.py:1163`; `app/chamados/models.py:474`)

  **Entrada → erro:** C vivo aponta J2 pendente, tem célula/escala e cobra o marco S19; aprovar J1 registra S19. O signal da batida chama o cartório, a lâmpada acesa permite `reconciliar()` e C termina resolvido, embora a regra nova determine preservar C durante esse ato. J2 pode continuar pendente: este desvio não passa pela auto-rejeição de `_retratar`. O save da própria justificativa também aciona o cartório. (`app/ponto/signals.py:135`; `app/ponto/services/cartorio.py:1163`; `app/chamados/models.py:959`; `app/chamados/reconciliador.py:445`; `app/chamados/reconciliador.py:514`; `app/ponto/signals.py:165`)

- **Conjunto antecipado de travas é incompleto — ordem de trava.** `travar_origem_antes` cobre VIVOS + origem; `_encerrar_bloqueio_de_entrada` busca NAO_FECHADOS, que inclui `resolvido`. Portanto, pode adquirir depois uma trava ausente do conjunto antecipado. (`app/ponto/services/justificativa.py:101`; `app/ponto/services/justificativa.py:69`; `app/chamados/catalogo/motor.py:32`)

  **Entrada → erro:** mesmo colaborador sem chamados vivos; C1 e C2 são bloqueios de entrada resolvidos; J1/J2 pendentes apontam respectivamente C1/C2 e não têm saída informada. T1 trava C1/J1; T2 trava C2/J2. O helper da origem retorna sem fechar; o helper do bloqueio exclui a própria origem, fazendo T1 esperar C2 e T2 esperar C1. Há deadlock entre as próprias aprovações, sem nascimento concorrente de chamado. (`app/ponto/services/justificativa.py:106`; `app/ponto/services/justificativa.py:119`; `app/ponto/services/justificativa.py:39`; `app/ponto/services/justificativa.py:72`; `app/ponto/services/justificativa.py:195`)

- **A nova ordem cruza com a regularização — ordem de trava.** A aprovação faz C → J; a regularização atualiza J dentro de `atomic` e depois analisa C, fazendo J → C. (`app/ponto/services/justificativa.py:106`; `app/ponto/services/justificativa.py:119`; `app/ponto/services/regularizacao.py:89`; `app/ponto/services/regularizacao.py:106`; `app/ponto/services/regularizacao.py:137`)

  **Entrada → erro:** C aberto, J pendente com entrada e sem saída informada. A regularização grava J; a aprovação trava C e espera J; a regularização tenta analisar C e espera C. Uma operação precisa abortar. O caminho externo é evidência da interação; o ponto introduzido nesta obra é a nova ordem da aprovação. (`app/ponto/services/regularizacao.py:102`; `app/chamados/models.py:572`; `app/ponto/services/justificativa.py:118`)

- **Materialização pode fechar duas vezes — outra: duas transições para um fato.** A cascata continua incluindo `resolvido`, e `encerrar()` permite `resolvido → fechado`. É comportamento preexistente conservado no trecho tocado. (`app/ponto/services/justificativa.py:371`; `app/ponto/services/justificativa.py:379`; `app/chamados/catalogo/motor.py:32`; `app/chamados/catalogo/motor.py:114`)

  **Entrada → erro:** origem C de `turno_aberto`, ligada à própria J, entrada 07h e saída materializada 19h, sem retenção. A batida dispara o reconciliador e resolve C; depois a cascata transforma C em fechado. Cada passagem grava `estado_chamado`: duas linhas no mesmo ato. O helper posterior da origem ser no-op não elimina a segunda transição anterior. (`app/ponto/services/justificativa.py:338`; `app/ponto/signals.py:215`; `app/chamados/reconciliador.py:1266`; `app/chamados/models.py:444`; `app/ponto/services/justificativa.py:379`; `app/chamados/models.py:1201`; `app/chamados/models.py:227`)

- **“Batida não nula” não garante saída válida — pouso antes do pré-requisito.** Os guards novos recusam `None`, mas `registrar_batida` também pode devolver uma batida de tipo oposto ou retratada quando encontra a mesma chave colaborador/instante. A aceitação já existia; permanece nos trechos tocados e agora pode acompanhar o fechamento novo da origem. (`app/ponto/services/justificativa.py:172`; `app/ponto/services/justificativa.py:347`; `app/ponto/views.py:1822`; `app/ponto/registro_batida.py:120`)

  **Entrada → erro:** J tem entrada às 07h e solicita saída às 19h; já existe uma **E** às 19h, sem vínculo com outra justificativa. A porta devolve essa E; os guards passam; J fica aprovada com `batida_gerada` apontando uma entrada, e a aprovação pode resolver sua origem pela premissa da própria justificativa aprovada. (`app/ponto/registro_batida.py:122`; `app/ponto/services/justificativa.py:174`; `app/ponto/services/justificativa.py:181`; `app/ponto/services/justificativa.py:194`; `app/chamados/models.py:937`)

**2. Escritores não alcançados e segundo fechador**

Há escritor de aprovação não alcançado: **Django admin — FORA-DO-ESCOPO, preexistente**. `JustificativaAdmin` é editável, não delega às portas da aprovação e o campo `status` não é somente leitura. O selo novo declara excluir esse caminho. (`app/ponto/admin.py:18`; `app/ponto/models.py:159`; `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:13`)

As três atribuições explícitas de produção encontradas estão costuradas: aprovação individual, materialização e lote. (`app/ponto/services/justificativa.py:182`; `app/ponto/services/justificativa.py:194`; `app/ponto/services/justificativa.py:360`; `app/ponto/services/justificativa.py:387`; `app/ponto/views.py:1796`; `app/ponto/views.py:1837`)

O censo AST da cópia não encontrou escrita de `status_local` fora das portas reconhecidas; isso conserva os limites de resolução declarados pelo próprio selo, não constitui garantia contra qualquer escrita dinâmica. (`app/chamados/tests/test_selo_status_so_pelas_portas.py:317`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:337`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:17`)

Na aprovação individual, não sobrou o antigo fecho forçado externo: origem e bloqueio delegam a `retratar`, e o seletor do bloqueio exclui a origem. O fechamento forçado permanece na cascata da materialização, responsável pelo caso de dupla transição acima. (`app/ponto/services/justificativa.py:42`; `app/ponto/services/justificativa.py:72`; `app/ponto/services/justificativa.py:75`; `app/ponto/services/justificativa.py:379`)

**3. Mordida dos testes** [nao verificado]

O selo estrutural **morde a retirada das três costuras**: removendo as chamadas em memória, acusa os três escritores. Entretanto, seu critério verifica ancestral `atomic`, receptor e posição textual; aceita chamada sob `if False`, falha engolida por `except` e fechamento colocado antes da saída. A limitação do ramo morto está declarada. (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:94`; `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:102`; `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:8`)

Há estes pontos de **selo/RED sem mordida suficiente**:

- `test_J2_nao_e_rejeitada_pela_BATIDA_do_proprio_ato` exige uma batida vinculada, mas não exige que o receiver/reconciliador tenha executado. Sem esse receiver, a costura explícita ainda retém C, permitindo sucesso por ausência do caminho anunciado. (`app/ponto/tests/test_justificativa_encerra_origem.py:413`; `app/ponto/tests/test_justificativa_encerra_origem.py:422`; `app/ponto/services/justificativa.py:194`)
- Esse teste e o da cascata usam fixture sem célula/escala; o cartório retorna antes de julgar. Portanto, não cobrem o desvio do primeiro defeito. (`app/ponto/tests/test_justificativa_encerra_origem.py:392`; `app/ponto/tests/test_justificativa_encerra_origem.py:426`; `app/ponto/services/cartorio.py:1547`)
- O teste positivo da materialização escolhe origem `justificativa_colab` e não conta trilhas; não acusa a dupla transição da origem de turno. (`app/ponto/tests/test_justificativa_encerra_origem.py:203`; `app/ponto/tests/test_justificativa_encerra_origem.py:213`; `app/ponto/tests/test_justificativa_encerra_origem.py:229`)
- `test_erro_de_banco_repetido_nas_perguntas_derruba_a_aprovacao_INTEIRA` pode passar sem o savepoint S143, porque a segunda chamada ao resolver já propaga a falha. O teste específico do savepoint é `test_erro_de_banco_engolido_no_receiver_S143_nao_mente_o_fechamento`. (`app/ponto/tests/test_justificativa_encerra_origem.py:256`; `app/chamados/reconciliador.py:521`; `app/ponto/tests/test_justificativa_encerra_origem.py:231`)
- Os tetos aceitam contagens menores; retirar o savepoint reduz queries e continua passando. Isso é adequado para um teto, mas não certifica a existência da cura. (`app/ponto/tests/test_selo_chokepoint_escrita.py:213`)

Há sentinelas úteis contra ausência de sinal: chamada obrigatória da porta no teste de rollback, chamada obrigatória de `registrar_batida` e trilha obrigatória da recusa. Execução desses funcionais contra árvores sem cada cura: **[nao verificado]**. (`app/ponto/tests/test_justificativa_encerra_origem.py:170`; `app/ponto/tests/test_justificativa_encerra_origem.py:197`; `app/ponto/tests/test_justificativa_encerra_origem.py:159`)

**4. Ordem de trava e duração do contexto**

O caminho comum deixou de começar pela trava de J: as portas antecipam chamados por PK antes de travar a justificativa. Isso acompanha a ordem efetiva de `_retratar`, que escreve C e depois pode atualizar J pendente. Não elimina os dois ciclos concretos descritos acima. (`app/ponto/services/justificativa.py:106`; `app/ponto/services/justificativa.py:119`; `app/chamados/reconciliador.py:508`; `app/chamados/reconciliador.py:520`)

O `ContextVar` tem limpeza por `finally`, mas `with atomic(), no_ato_de_aprovar(...)` remove a marca **antes** da saída do `atomic` e dos callbacks de commit. Assim, cobre a cadeia síncrona que efetivamente consulta a regra, porém não permanece nos callbacks registrados por perguntas/retomada. Dano adicional concreto desses callbacks ou de troca de thread: **[nao verificado]**. (`app/chamados/reconciliador.py:430`; `app/ponto/services/justificativa.py:117`; `app/ponto/services/justificativa.py:319`; `app/ponto/views.py:1792`; `app/chamados/signals.py:121`; `app/chamados/signals.py:202`)

**5. Efeitos colaterais dentro da aprovação**

**Sim, uma falha persistente na resolução das perguntas pode desfazer a aprovação inteira.** S143 agora captura a falha dentro de savepoint, mas `_retratar` chama o resolver novamente sem captura. Essa segunda falha atravessa o `atomic` da aprovação: rollback integral, compatível com a proibição de meio-estado. (`app/chamados/signals.py:88`; `app/chamados/reconciliador.py:507`; `app/chamados/reconciliador.py:521`; `app/ponto/services/justificativa.py:196`)

O receiver de rebaixamento não captura exceções; uma exceção propagaria. No caminho normal, porém, o núcleo já rebaixou a urgência antes do save, e o receiver retorna sem escrever. (`app/chamados/signals.py:72`; `app/chamados/models.py:210`; `app/chamados/models.py:278`; `app/chamados/models.py:424`)

O cartório é protegido por `engolir`, que captura exceções sem criar savepoint. Não confirmei um novo caso adicional de sucesso silencioso com rollback nesse percurso; isolamento completo de toda falha de banco: **[nao verificado]**. O erro concreto demonstrável é ele fechar C sem consultar a retenção, conforme o primeiro achado. (`app/chamados/signals.py:172`; `app/core/observ.py:85`; `app/ponto/services/cartorio.py:1163`)

Em `invalidar_fechamento`, leitura e updates ficaram em savepoints próprios, com captura externa; não identifiquei defeito novo seguro nessa alteração. Gravação incorreta causada apenas por ler os próprios dados ainda não commitados, além do desvio de retenção descrito: **[nao verificado]**. (`app/ponto/services/fechamento.py:1616`; `app/ponto/services/fechamento.py:1619`; `app/ponto/services/fechamento.py:1629`)

## 29/10 18:19 · sessao f208ba29631a · justif · leitura · origem=Code

**O chamado `entrada_bloqueada_turno_aberto` VIVO, por si só, não impede a batida.** No web, ele é reutilizado para atualizar o contexto e sinalizar revisão; o fluxo continua até o registro. Na API, a anomalia é coletada, a batida passa pelo chokepoint e depois pode alimentar uma disputa. **Isso não significa criação incondicional:** existem recusas, confirmação e retornos de batida existente. (`app/ponto/views.py:274–314`, `345–353`; `app/api/views_core.py:432–458`, `679–718`, `769–781`.)

`VIVOS` inclui `aberto`, `em_analise` e `registrado`. (`app/chamados/catalogo/motor.py:24`.)

**Caminho do app/PWA pela API**

| Passo | Código e resultado |
|---|---|
| Entrada HTTP | O proxy declarado encaminha `/api/ponto/*` ao core; `/api/ponto/bater/` chega a `views_core.api_bater_ponto`. (`Caddyfile:8–9`; `app/config/urls_core.py:6`; `app/api/urls_core.py:5`, `14`.) |
| Autenticação/colaborador | Endpoint POST autenticado; colaborador inexistente retorna **404**, antes do registro. (`app/api/views_core.py:385–391`.) |
| Situação cadastral | Situação fora de `ativo`/`afastado` retorna **403**, incluindo os ramos `desligado` e `ferias`. (`app/api/views_core.py:402–410`.) |
| Tipo | Tipo diferente de `E`/`S` retorna **400**. (`app/api/views_core.py:419–426`.) |
| Turno órfão | O detector consulta entradas sem saída; havendo bloqueio e nenhuma regularização pendente, acrescenta `orfao_14h`. Esse teste consulta a geometria/justificativas, não a existência do chamado desse módulo. (`app/api/views_core.py:432–433`; `app/chamados/detector_anomalias.py:29–53`; `app/ponto/utils_turno_orfao.py:28–58`.) |
| GPS | Latitude ou longitude ausente retorna **403 `gps_obrigatorio`**, sem chegar ao chokepoint. (`app/api/views_core.py:440–458`.) |
| Foto/assinatura | Foto ausente e assinatura inválida acrescentam anomalias; esses blocos continuam o processamento. (`app/api/views_core.py:459–500`.) |
| Instante | `hora_do_aparelho` fornece `ts_efetivo` e o timestamp recebido do aparelho. (`app/api/views_core.py:528–531`.) |
| Deduplicação | Mesmo tipo em **±5 minutos** retorna **`ja_registrada`**, com ID existente e `sucesso=True`; não cria outra batida. (`app/api/views_core.py:549–568`.) |
| Escolha E/S | Ao vivo, `proximo_tipo_de` pode corrigir o tipo enviado; no retroativo, os marcos podem corrigi-lo. (`app/api/views_core.py:592–623`.) |
| Entrada/saída anômala | Saída sem entrada ou nova entrada sobre turno aberto acrescentam motivo/contexto; esses blocos não retornam recusa. (`app/api/views_core.py:632–672`.) |
| Escrita | Chama `registrar_batida(..., origem='app', autoridade_humana=False, canal=...)`. (`app/api/views_core.py:679–694`.) |
| Chokepoint recusou | `batida is None` retorna **`ignorada_intervalo`**, com `sucesso=True` e referência à oposta existente; não cria nova batida. (`app/api/views_core.py:695–718`.) |
| Depois do registro | Havendo anomalias, abre/atualiza disputa e marca `em_disputa=True`; a resposta normal contém `status='ok'` e ID da batida. Portanto, **disputa não substitui a criação já realizada**. (`app/api/views_core.py:769–781`, `823–834`.) |

A outra casca também expõe `/api/ponto/bater/`, por `api.views.api_bater_ponto`, com os mesmos ramos relevantes: situação/tipo, GPS, deduplicação, chokepoint e disputa. (`app/config/urls.py:37`; `app/api/urls.py:67`; `app/api/views.py:398–466`, `557–576`, `683–721`, `769–781`.)

**Caminho do formulário web/PWA**

1. `/ponto/bater/` e `/ponto/bater-v2/` chegam a `bater_ponto`; onboarding incompleto redireciona antes do processamento. A triagem reconstrói entradas sem saída e calcula `tem_bloqueio_turno` por **14 ≤ horas < 96**. (`app/ponto/urls.py:11–12`; `app/ponto/views.py:240–259`; `app/ponto/services/triagem_batida.py:17–30`, `175–206`.)

2. Regularização pendente significa justificativa `pendente`, vinculada à entrada, com `saida_informada`; **não significa simplesmente chamado aberto**. (`app/ponto/services/triagem_batida.py:208–223`.)

3. No POST `E`, com bloqueio e sem regularização pendente, procura o chamado desse módulo em `VIVOS`. Encontrando, renova seu contexto; não encontrando, cria. Depois define `_sinal_acesso` e continua. **Não há `return` de recusa nesse ramo.** (`app/ponto/views.py:274–314`.)

4. Existe um desvio sem gravar: o `elif` de entrada com `turno_aberto` e sem `confirmar` salva GPS/foto na sessão e **retorna a tela de confirmação**. A confirmação reenvia `E` com `confirmar=sim`. (`app/ponto/views.py:315–329`; `app/templates/ponto/triagem/turno_confirmar.html:8–17`.)

5. Para `E`/`S`, chama `ramo_registrar_es`. Mesmo tipo nos cinco minutos anteriores retorna `ja_registrada`, sem criação. Fora disso, o sinal de acesso torna a batida **`aprovada=False`**, com justificativa de revisão, e chama `registrar_batida`. (`app/ponto/views.py:345–353`; `app/ponto/services/triagem_batida.py:669–720`.)

6. Se o chokepoint devolver `None`, retorna `ignorada_intervalo`; se devolver batida, prepara o contexto de sucesso. (`app/ponto/services/triagem_batida.py:721–753`.)

A UI v2 envia pelo endpoint da API e tem barreiras próprias: sem selfie ou GPS, aborta o envio. Também mostra sucesso para qualquer HTTP OK, sem interpretar o corpo — incluindo respostas `ja_registrada`/`ignorada_intervalo`. (`app/templates/ponto/bater_ponto_v2.html:285–300`, `325–340`.)

No PWA offline, guardar na IndexedDB é registro **local**; a criação no servidor depende da sincronização posterior pelo mesmo `/api/ponto/bater/`. (`app/static/pwa.js:107–157`.)

**O que decide definitivamente em `ponto/registro_batida.py`**

| Condição | Resultado |
|---|---|
| Mesmo tipo não retratado em ±60 segundos | Retorna a existente; **não cria**. (`app/ponto/registro_batida.py:106–115`.) |
| Mesma chave colaborador/timestamp | Retorna a existente; **não cria**. (`app/ponto/registro_batida.py:120–122`.) |
| `fato_de_chao=False` e competência fechada | Retorna `None, erro`; **não cria**. App/web passam `autoridade_humana=False` e omitem `fato_de_chao`, portanto este deriva para `True` nesses caminhos. (`app/ponto/registro_batida.py:130–137`; `app/api/views_core.py:680–694`; `app/ponto/services/triagem_batida.py:708–720`.) |
| Batida oposta na janela, sem casar marco do próprio tipo, e sem autoridade humana | Retorna `None, conflito`; **não cria**. A janela é 300 segundos com escala/marcos, senão 60 segundos. (`app/ponto/registro_batida.py:21–50`, `138–151`.) |
| Passou pelas condições anteriores | Executa **`Batida.objects.create`**, chama fechamento por evento e retorna a batida. (`app/ponto/registro_batida.py:157–161`.) |

**Portanto, há ramos reais que recusam ou não gravam.** O chamado vivo não é a condição dessas recusas; os pontos decisivos são os retornos dos endpoints e as guardas acima. (`app/ponto/views.py:274–329`; `app/api/views_core.py:402–458`, `549–568`, `695–718`; `app/ponto/registro_batida.py:106–161`.)

Quanto ao **“relógio”**, o código denomina `origem='app'` como `DO_RELOGIO`, e tanto API quanto formulário web gravam essa origem; `device_id` distingue os canais APK/PWA. Um fluxo separado de aparelho físico até esse chokepoint é **[não verificado]**. A rota AEJ existente devolve HTTP 410. (`app/ponto/services/procedencia_batida.py:30`; `app/ponto/registro_batida.py:53–61`; `app/api/views_core.py:680–684`; `app/ponto/services/triagem_batida.py:708–712`; `app/ponto/urls.py:9`; `app/ponto/views.py:56–71`.)

**Quem fecha o chamado**

As portas convergem principalmente para:

- **`encerrar()` → `fechado`**, sem guarda de premissa. (`app/chamados/models.py:1192–1202`.)
- **`reconciliar()` → `resolvido`**, condicionado ao motor e ao gate da premissa. (`app/chamados/models.py:434–472`; `app/chamados/catalogo/motor.py:111`.)
- **`transicionar()`**, decisão humana com guardas de perfil, justificativa, disputa e regularização. (`app/chamados/models.py:122–183`.)

| Porta/função | Quem chama e condição de fechamento |
|---|---|
| **Aprovação DP — `aprovar_justificativa_dp`** | `ponto.views.aprovar_justificativa` chama o serviço. Ele seleciona **somente o chamado mais recente** desse módulo, do colaborador, em `NAO_FECHADOS`, **sem filtro de data**, e chama `encerrar()`. O fechamento é best-effort depois da aprovação. (`app/ponto/views.py:1894–1899`; `app/ponto/services/justificativa.py:121–142`.) |
| **Evento de batida — `reconciliar_chamados_colab`** | Receiver `_reconciliador_post_save_batida` e comando `reconciliar_chamados`. Varre VIVOS; premissa morta chama `_retratar` → `reconciliar()`. Esse módulo usa `FATO_TURNO_ABERTO`; com dia, consulta o turno do chamado; sem dia, consulta globalmente. (`app/ponto/signals.py:206–218`; `app/chamados/management/commands/reconciliar_chamados.py:51–52`; `app/chamados/reconciliador.py:264–289`, `1203–1214`; `app/chamados/catalogo/premissa.py:48`.) |
| **`processar_alertas_turno._resolver_chamado_do_turno`** | `handle`, quando o turno materializado está fechado. Busca explicitamente `turno_aberto`/`entrada_bloqueada_turno_aberto`, por dia ou fallback legado; consulta outros turnos abertos e chama `retratar()`. (`app/ponto/management/commands/processar_alertas_turno.py:88–89`, `153–184`, `203–218`.) |
| **`reconciliar_fantasmas.handle`** | Com `--apply`, deriva datas do chamado, inclusive de `pendentes`; exige nenhuma delas aberta no núcleo e premissa morta; chama `retratar()`. O cron declarado usa **`--alarme`**, não aplicação. (`app/ponto/management/commands/reconciliar_fantasmas.py:147–158`, `178–184`, `247–279`; `app/config/crons.py:373`.) |
| **`reconciliar_grade.handle`** | Inclui o módulo por `FURO_MODULES`; quando todas as datas passam às condições de retratação e o catálogo concorda, com aplicação chama `retratar()`. (`app/chamados/catalogo/furo_modulos.py:15–16`; `app/ponto/management/commands/reconciliar_grade.py:206–208`, `281–300`.) |
| **`retratar_chamados_lei_cala.handle`** | Com `--modulo entrada_bloqueada_turno_aberto --apply`, seleciona VIVOS com dia passado em que `classificar_falta` não aponta falta; chama `retratar()`. (`app/chamados/management/commands/retratar_chamados_lei_cala.py:20–24`, `33–59`.) |
| **Absolvição pela célula — `retratar_chamados_absolvidos_por_celula`** | Consumidor do outbox chama a função; o módulo está em `MODULOS_DE_TURNO`. Exige célula com veredito que não acusa e sem marcos pendentes; chama `_retratar()`. (`app/chamados/reconciliador.py:336–337`, `792–827`, `1145`; `app/config/crons.py:375`.) |
| **Cartório — `_julgar_colab_corpo`** | Entradas `julgar_colab`/`julgar_celula`, acionadas por signals e `processar_cartorio`. Reconcilia por marcos acesos, marcos inexistentes, cobrança sem furo/dia sem trabalho ou perguntas todas duplicatas. O universo inclui esse módulo por `FURO_MODULES`. (`app/ponto/services/cartorio.py:70–94`, `709–744`, `1163`, `1175`, `1188–1196`, `1257–1266`, `1531–1560`; `app/ponto/signals.py:88–93`; `app/chamados/signals.py:166–174`; `app/ponto/management/commands/processar_cartorio.py:65`.) |
| **Decisão do dia — `fechar_o_fio_do_dia`** | Chamado por `veredito_celula` e `declarar_dia`. Seleciona VIVOS pela data ou célula e chama `reconciliar()`. (`app/ponto/services/veredito_celula.py:148–186`, `324`; `app/ponto/services/declarar_dia.py:197–198`.) |
| **Cobertura — `designar_cobertura` / `declarar_cobertura_passada`** | A view chama `designar_cobertura`; o serviço considera `chamado_origem_id`. A declaração passada considera VIVOS do dia. Ambos passam pelo catálogo e por `reconciliar()`, que pode recusar. (`app/ponto/views.py:3013–3020`; `app/ponto/services/cobertura.py:95`, `114–122`, `218–230`, `246`, `310–319`.) |
| **Competência trancada** | `trancar_periodo` chama `disparar_no_ato`; execução direta ou comando `encerrar_por_competencia_trancada` chama a porta homônima do model. Seleciona VIVOS pela competência e grava fechamento com via própria. (`app/ponto/services/fechamento.py:1395–1396`; `app/chamados/services/competencia_trancada.py:88–103`, `174–177`, `269–285`; `app/chamados/management/commands/encerrar_por_competencia_trancada.py:19–29`; `app/chamados/models.py:1204–1215`.) |
| **Desligamento — `executar_desligamento`** | View de desligamento ou comando de desligamentos agendados; encerra **todos** os chamados do colaborador em `NAO_FECHADOS`. (`app/colaboradores/views.py:1430–1434`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:35`, `57`; `app/colaboradores/services/desligamento.py:132–139`.) |
| **Transição humana genérica** | Detalhe → `acao_resposta_livre`; fio → `processar_post_fio`; ambos chamam `transicionar(novo_status, user)` sem filtro desse módulo, sujeitos às guardas. Outras ações do roteador também convergem para `transicionar('resolvido')`. (`app/chamados/views.py:546–547`, `669–674`; `app/chamados/services/acoes_chamado.py:70`, `222`, `277`, `313–321`; `app/chamados/services/fio.py:703–717`; `app/chamados/models.py:122–183`.) |
| **Materialização de pergunta — destino e clones** | Materializador, chamado por ações de disputa, fio e validação, reconcilia destino/clones por PK, sem filtro de módulo. Retém enquanto houver irmãs respondidas sem consumo. Resposta de ausência já decidida também reconcilia o destino; ausência aberta não o faz. (`app/chamados/services/materializacao.py:537–539`, `679–721`, `919`, `1004`, `1024`, `1255`; `app/chamados/services/acoes_disputa.py:139`; `app/chamados/services/fio.py:664`; `app/chamados/services/validacao.py:423`.) |
| **Fechamento da disputa vinculada — `disputa_fechada_fecha_chamado_pai`** | Signal de `DisputaSupervisao` fechada; considera o chamado-pai por FK, sem filtro de módulo, e chama `reconciliar()`. (`app/chamados/signals.py:18–45`.) |

**Fechar automaticamente não é garantido.** `_retratar` retém o chamado quando há irmãs respondidas sem consumo. `reconciliar()` pode recusar quando o dia segue furado para a folha, sem regularização, agrupamento ou outra cobrança viva que permita deduplicação. Sem dia apurável, o gate permite fechamento e registra essa ausência. O chamado emitido pelo web tem categoria `turno_aberto`, que o inclui na guarda de furo. (`app/chamados/reconciliador.py:435–457`; `app/chamados/models.py:484–525`, `915–921`, `2472`; `app/ponto/views.py:296–298`.)

**Enviar a regularização não fecha esse chamado:** no formulário, cria/atualiza justificativas e eleva a urgência; no serviço de regularização, passa para análise. A lista do fechamento explícito por saída retroativa também não contém esse módulo. (`app/ponto/services/triagem_batida.py:258–293`; `app/ponto/services/regularizacao.py:126–141`; `app/ponto/services/justificativa.py:274–285`.)

## 29/10 18:18 · sessao 1c944a938d1d · - · - · origem=Ronald (tmux)

Achados confirmados por leitura:

1. **Retenção de J2 não cobre cartório — outra / costura incompleta.** A regra só é consultada por `_retratar` e pela cascata (`app/chamados/reconciliador.py:474`; `app/ponto/services/justificativa.py:377`). O cartório chama `reconciliar()` diretamente (`app/ponto/services/cartorio.py:1163`, `:1175`, `:1196`, `:1266`), e o gate não consulta a retenção (`app/chamados/models.py:474`).

   Contraexemplo: C vivo aponta J2 pendente, cobra S19 em `marcos_faltantes`, existe célula/escala do dia, e aprovação de J1 registra S19. O signal julga a célula (`app/ponto/signals.py:131`); a lâmpada acesa satisfaz `marcos_acesos` (`app/ponto/services/lastro.py:170`), e o cartório fecha C diretamente (`app/ponto/services/cartorio.py:1163`), autorizado pela premissa por marco (`app/chamados/models.py:957`). Resultado: J2 pendente com C resolvido, contrariando a retenção declarada (`app/chamados/reconciliador.py:445`). O próprio save da justificativa também entra nesse desvio (`app/ponto/signals.py:161`).

2. **Deadlock entre duas aprovações — ordem de trava.** A pré-trava cobre VIVOS e a origem (`app/ponto/services/justificativa.py:100`), mas `_encerrar_bloqueio_de_entrada` seleciona NAO_FECHADOS (`app/ponto/services/justificativa.py:69`), conjunto que também contém `resolvido` (`app/chamados/catalogo/motor.py:32`).

   Contraexemplo: mesmo colaborador sem chamados VIVOS; C1 e C2 `entrada_bloqueada_turno_aberto`, ambos resolvidos; J1/J2 pendentes, respectivas origens C1/C2, sem saída informada. T1 pré-trava C1 e J1; T2 pré-trava C2 e J2 (`app/ponto/services/justificativa.py:106`, `:119`). O helper da origem retorna sem fechar (`:39`). O helper do bloqueio exclui a própria origem (`:72`): T1 tenta C2, T2 tenta C1. Uma aprovação precisa abortar; a prometida serialização entre aprovações não cobre esse conjunto (`:83`).

3. **Outra inversão real com regularização.** A regularização abre atomic, atualiza J existente sem saída e depois analisa chamados: J → C (`app/ponto/services/regularizacao.py:89`, `:106`, `:137`). A aprovação nova faz C → J (`app/ponto/services/justificativa.py:106`, `:119`). Com C aberto e J pendente sem saída, as duas transações podem esperar uma pela outra; `analisar()` chega ao escritor de estado (`app/chamados/models.py:566`). Trata-se de limite remanescente no sistema, já admitido genericamente na documentação da nova pré-trava (`app/ponto/services/justificativa.py:89`).

Sobre testes e contexto: [nao verificado]

- Os testes de J2 pela batida e pela cascata usam fixture sem célula nem escala (`app/ponto/tests/test_justificativa_encerra_origem.py:380`, `:404`, `:419`). O cartório retorna antes de julgar quando a célula não existe (`app/ponto/services/cartorio.py:1547`). Portanto, não demonstram cobertura do desvio do item 1.
- A marca é restaurada por `finally`, inclusive em exceções (`app/chamados/reconciliador.py:430`). Com `with atomic(), no_ato_de_aprovar(...)`, ela é removida antes da saída do atomic e dos callbacks de commit (`app/ponto/services/justificativa.py:117`, `:319`; `app/ponto/views.py:1792`). Existem callbacks de perguntas e retomada (`app/chamados/signals.py:121`, `:202`); dano adicional específico desses callbacks: [nao verificado]. Nenhuma troca de thread foi identificada nesses corpos; comportamento em threads externas: [nao verificado].

Sobre efeitos colaterais:

- O receiver de rebaixamento não captura exceções (`app/chamados/signals.py:68`), mas o núcleo já rebaixa a urgência antes do save, tornando a execução normal do receiver um no-op (`app/chamados/models.py:210`, `:278`).
- S143 agora tem savepoint e captura a falha (`app/chamados/signals.py:88`). `_retratar` chama o resolver novamente, sem captura; falha persistente desfaz a aprovação inteira (`app/chamados/reconciliador.py:507`, `:521`; `app/ponto/services/justificativa.py:196`).
- O cartório é chamado dentro de `engolir`, que não cria savepoint (`app/chamados/signals.py:171`; `app/core/observ.py:85`). Entretanto, as escritas centrais examinadas têm seus atomics (`app/ponto/portas/celula.py:639`; `app/chamados/models.py:226`); não confirmei contraexemplo adicional de sucesso silencioso com rollback causado pelo cartório. A leitura dos próprios dados ainda não commitados, isoladamente, não demonstrou gravação incorreta; o desvio concreto é o fechamento sem consultar a retenção, item 1.

## 29/10 18:18 · sessao bf4283445a42 · - · - · origem=Ronald (tmux)

Inventário enviado: 12 portas específicas/por dia, mais transição humana, materialização de destino/clones e fechamento do pai por disputa. As três genéricas não filtram módulo; dependem do vínculo e das guardas da porta (`app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/fio.py:703`, `app/chamados/services/materializacao.py:702`, `app/chamados/signals.py:28`).

Exclusões confirmadas: EB não integra `CALADOS_POR_ISENCAO`, a lista da varredura LIMBO nem o universo de `lastro.fechar` (`app/chamados/catalogo/modulos.py:235`, `app/chamados/services/limbo_folgas.py:117`, `app/ponto/services/lastro.py:216`).

## 29/10 18:18 · sessao d3f044fc1ef0 · - · - · origem=Ronald (tmux)

Achados e censo:

- **Ordem de trava — deadlock entre duas aprovações, sem chamado novo.** `travar_origem_antes` trava VIVOS + origem, por PK, mas `_encerrar_bloqueio_de_entrada` seleciona NAO_FECHADOS, que inclui `resolvido`; resolvido não-origem fica fora da trava antecipada. Contraexemplo: mesmo colaborador; C1/pk1 `resolvido`, módulo `entrada_bloqueada_turno_aberto`; C100/pk100 vivo. J1 aponta C100, J2 aponta C1. T1 aprovar J1 trava C100; T2 aprovar J2 trava C1 e espera C100; T1 tenta `_encerrar_bloqueio_de_entrada` e espera C1. Ciclo C100→C1/C1→C100. Sítios: `app/ponto/services/justificativa.py:101`, `:106`, `:117`, `:119`, `:194`, `:195`, `:69`; universo em `app/chamados/catalogo/motor.py:32`. A declaração “duas aprovações ... se serializam” em `app/ponto/services/justificativa.py:85` não cobre esse caso.

- **Outra — duas transições/trilhas na materialização da origem de turno.** Entrada 07h + saída materializada 19h, C de `turno_aberto` vivo apontando a própria J: criar Batida dispara reconciliador, que retrata C para `resolvido`; cascata posterior inclui `resolvido` e o passa a `fechado`. São duas linhas `estado_chamado` para o ato. Caminho: `app/ponto/services/justificativa.py:338`; `app/ponto/registro_batida.py:157`; `app/ponto/signals.py:206`; `:215`; `app/chamados/reconciliador.py:286`, `:1255`, `:1266`, `:508`; `app/chamados/models.py:444`; depois `app/ponto/services/justificativa.py:371`, `:379`; `app/chamados/catalogo/motor.py:32`, `:114`; `app/chamados/models.py:1201`, `:227`. A cascata já existia, mas é sítio tocado e continua contrariando o contrato de uma trilha. O teste novo de materialização escolhe `justificativa_colab` e não mede trilhas: `app/ponto/tests/test_justificativa_encerra_origem.py:203`, `:213`, `:229`.

- **Escritor de aprovada não alcançado: Django admin, preexistente — FORA-DO-ESCOPO.** `JustificativaAdmin` herda diretamente `ModelAdmin`, sem restrição de edição ou delegação; `status` é campo editável. Admin está exposto nas URLs. `app/ponto/admin.py:18`, `:19`, `:20`; `app/ponto/models.py:159`; `app/config/urls.py:24`. O selo novo exclui expressamente esse escritor: `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:13`. As três atribuições explícitas são alcançadas: `app/ponto/services/justificativa.py:182`, `:194`, `:360`, `:387`; `app/ponto/views.py:1796`, `:1837`.

- **status_local fora das portas:** censo AST executado em memória, sem Django/DB/escrita, percorreu 739 arquivos e retornou zero violações. Usou as funções em `app/chamados/tests/test_selo_status_so_pelas_portas.py:317`, `:322`; allowlist em `:37`. Escritores detectados: `_gravar_estado`, `abrir`, `criar` (e as três portas de DisputaSupervisao para o outro campo). Limites de resolução declarados pelo selo em `app/chamados/tests/test_selo_status_so_pelas_portas.py:17`. Nenhuma escrita direta nova do diff fora da porta: origem e bloqueio delegam `retratar`, cascata usa `encerrar`: `app/ponto/services/justificativa.py:42`, `:75`, `:379`.

- **Segundo fechador em aprovar DP:** o fecho antigo externo foi substituído pela chamada interna `_encerrar_bloqueio_de_entrada`; origem é excluída desse seletor e ambos passam pela unidade `retratar`: `app/ponto/services/justificativa.py:72`, `:75`, `:194`, `:195`. Não encontrei segundo fecho forçado remanescente nessa função; o forçado permanece na cascata da materialização: `app/ponto/services/justificativa.py:379`.

- **invalidar_fechamento:** as operações suscetíveis a erro estão em savepoints próprios, com captura fora deles: leitura `app/ponto/services/fechamento.py:1616`, UPDATE `:1619`, captura `:1629`. Não identifiquei defeito novo seguro nessa alteração; prova de execução transacional contra banco não realizada [nao verificado].

## 29/10 18:17 · sessao 6bc00e83b369 · - · - · origem=Ronald (tmux)

**Resultado:** chamado `entrada_bloqueada_turno_aberto` vivo não é uma guarda de recusa na API: `api_bater_ponto` coleta anomalias, chama `registrar_batida`, depois abre/atualiza disputa. No formulário web, o chamado vivo é procurado para renovar seu contexto; o fluxo continua para registrar. (`app/api/views_core.py:432-433`, `app/api/views_core.py:679-694`, `app/api/views_core.py:769-781`; `app/ponto/views.py:274-314`, `app/ponto/views.py:345-353`.)

**Rotas e canais**

- Proxy declarado: `/api/ponto/*` → `core:8000` (`Caddyfile:8-9`); URLconf core inclui `api.urls_core` (`app/config/urls_core.py:6`), que importa `views_core` e liga `ponto/bater/` a `api_bater_ponto` (`app/api/urls_core.py:5`, `app/api/urls_core.py:14`). A outra casca inclui `api.urls` (`app/config/urls.py:37`), cuja rota aponta `api.views.api_bater_ponto` (`app/api/urls.py:2`, `app/api/urls.py:67`).
- `/ponto/bater/` e `/ponto/bater-v2/` → `ponto.views.bater_ponto` (`app/config/urls.py:29`; `app/ponto/urls.py:11-12`). V2 renderiza `bater_ponto_v2.html` (`app/ponto/views.py:355-357`), cujo JS faz POST `/api/ponto/bater/` (`app/templates/ponto/bater_ponto_v2.html:328-338`).
- PWA offline grava IndexedDB local, sem criar `Batida` no servidor (`app/static/pwa.js:107-122`); sincroniza por POST `/api/ponto/bater/` (`app/static/pwa.js:126-157`). HTTP OK remove o item local (`app/static/pwa.js:158-165`); HTTP400 também remove, como erro definitivo (`app/static/pwa.js:166-175`).
- Formulário web antigo faz POST à própria URL (`app/templates/ponto/triagem/apto.html:86-91`, `app/templates/ponto/triagem/apto.html:210-235`), passando por `ponto.views.bater_ponto` (`app/ponto/views.py:267-272`).
- O próprio código chama `origem='app'` de relógio: `DO_RELOGIO = 'app'` (`app/ponto/services/procedencia_batida.py:30`). App e web passam essa origem ao chokepoint (`app/api/views_core.py:680-682`; `app/ponto/services/triagem_batida.py:708-710`). `canal_da_batida(device_id)` distingue APK/PWA (`app/ponto/registro_batida.py:53-61`). Integração atual de um relógio físico separado: **[não verificado]**. A porta AEJ cadastrada (`app/ponto/urls.py:9`) apenas retorna HTTP410 (`app/ponto/views.py:56-71`).

**Passos API até chokepoint**

1. POST autenticado, obtém colaborador; sem colaborador →404 (`app/api/views_core.py:385-391`).
2. Situação diferente de ativo/afastado →403; inclui desligado e ferias (`app/api/views_core.py:402-410`).
3. Tipo fora E/S →400 (`app/api/views_core.py:419-426`).
4. Detector consulta geometria de turno órfão, acrescentando `orfao_14h` quando há bloqueio e não há regularização pendente (`app/api/views_core.py:432-433`; `app/chamados/detector_anomalias.py:29-51`). Não consulta EB vivo para recusar (`app/chamados/detector_anomalias.py:19-121`).
5. GPS ausente →403 `gps_obrigatorio`, sem chamar chokepoint (`app/api/views_core.py:440-458`).
6. Sem foto usa juiz de selfie para adicionar anomalia, sem recusa (`app/api/views_core.py:459-483`; `app/ponto/registro_batida.py:80-91`).
7. Timestamp vem de `hora_do_aparelho` (`app/api/views_core.py:536-539` — **confirmar faixa: nesta casca o bloco começa antes; referência exata desta chamada não preservada no output**).
8. Mesmo tipo em ±5min → HTTP200 `ja_registrada`, devolve existente, sem criar (`app/api/views_core.py:549-568`).
9. Tipo ao vivo pode ser corrigido pelo `proximo_tipo_de`; retroativo pode ser corrigido por marcos (`app/api/views_core.py:592-623`).
10. Saída sem entrada ou entrada sobre turno aberto adicionam motivo/contexto; não retornam erro (`app/api/views_core.py:632-672`).
11. `registrar_batida(colab, ts_efetivo, tipo, 'app', autoridade_humana=False, canal=...)` (`app/api/views_core.py:679-694`).
12. Retorno `batida is None` →HTTP200 `ignorada_intervalo`, devolvendo oposta existente e `sucesso=True`, sem criar nova (`app/api/views_core.py:695-721`).
13. Após gravação, anomalias alimentam `criar_ou_atualizar_disputa`, `em_disputa=True` (`app/api/views_core.py:769-781`); resposta normal `status='ok'`, ID e sucesso (`app/api/views_core.py:823-834`).

Clone UI possui decisões equivalentes em `app/api/views.py:398-439`, `app/api/views.py:449-466`, `app/api/views.py:557-576`, `app/api/views.py:600-631`, `app/api/views.py:683-721`, `app/api/views.py:769-781`, `app/api/views.py:823-834`.

**Formulário web e EB**

- Triagem reconstrói entradas sem saída por `batidas_apuraveis`/`parear_turnos` (`app/ponto/services/triagem_batida.py:137`, `app/ponto/services/triagem_batida.py:175-192`); `tem_bloqueio_turno` usa `14 <= horas < 96` (`app/ponto/services/triagem_batida.py:17-30`, `app/ponto/services/triagem_batida.py:206`).
- Regularização pendente depende de justificativa pendente com saída informada, não de chamado EB aberto (`app/ponto/services/triagem_batida.py:208-223`).
- POST E + bloqueio + sem regularização: procura EB vivo (`app/ponto/views.py:274-284`); se existe renova contexto (`app/ponto/views.py:285-288`), senão cria (`app/ponto/views.py:290-302`); apenas produz sinal e continua (`app/ponto/views.py:311-314`).
- **Ramo que desvia sem gravar:** POST E + `context['turno_aberto']` + sem confirmar: salva confirmação e retorna tela (`app/ponto/views.py:315-329`). O formulário dessa tela reenvia E com `confirmar=sim` (`app/templates/ponto/triagem/turno_confirmar.html:8-17`). Este `elif` também pode ser alcançado quando existe regularização pendente, pois o primeiro `if` exige ausência dela (`app/ponto/views.py:274`, `app/ponto/views.py:315`).
- POST E/S → `ramo_registrar_es` (`app/ponto/views.py:345-353`); duplica mesmo tipo nos 5min anteriores →`ja_registrada`, sem criar (`app/ponto/services/triagem_batida.py:669-677`).
- Sinal de acesso/bloqueio torna `aprovada=False` e escreve justificativa de revisão (`app/ponto/services/triagem_batida.py:692-705`); chama chokepoint (`app/ponto/services/triagem_batida.py:707-720`).
- Chokepoint retorna None →JSON `ignorada_intervalo`, sem nova batida (`app/ponto/services/triagem_batida.py:721-732`). Se retorna batida, define sucesso no contexto (`app/ponto/services/triagem_batida.py:745-753`).
- Fora de horário cria sinal e continua, sem impedir gravação (`app/ponto/services/triagem_batida.py:478-496`).

**Chokepoint: criação não é incondicional**

- Mesmo tipo não retratado em ±60s → devolve existente (`app/ponto/registro_batida.py:106-115`).
- Mesma chave colaborador/timestamp → devolve existente (`app/ponto/registro_batida.py:120-122`).
- Competência fechada recusa somente quando `fato_de_chao=False`; chamadas app/web acima omitem a flag e passam autoridade humana false, então deriva `fato_de_chao=True` (`app/ponto/registro_batida.py:130-137`).
- Oposta não retratada na janela e sem casar marco do próprio tipo, com autoridade humana false →`None, conflito`, sem criar (`app/ponto/registro_batida.py:138-151`). A janela é 300s com escala e marcos, senão60s (`app/ponto/registro_batida.py:21-32`); exceção de marco próprio está em `app/ponto/registro_batida.py:35-50`.
- Fora dessas saídas, cria `Batida` em `objects.create` (`app/ponto/registro_batida.py:157-159`), chama fechamento por evento e retorna (`app/ponto/registro_batida.py:160-161`); exceção do agendamento de fechamento é capturada (`app/ponto/registro_batida.py:175-183`).

**Desvios reais da UI v2 antes de POST**

- `bloqueio_horario` ou `aguardando_confirmacao` substitui formulário por aviso/link para tela antiga (`app/templates/ponto/bater_ponto_v2.html:64-75`).
- Sem selfie, `garantirRequisitos` retorna false (`app/templates/ponto/bater_ponto_v2.html:285-290`); sem GPS também (`app/templates/ponto/bater_ponto_v2.html:292-300`); confirmação aborta envio (`app/templates/ponto/bater_ponto_v2.html:325`).
- Qualquer HTTP OK é mostrado como sucesso sem ler o corpo; isso inclui `ja_registrada` e `ignorada_intervalo` (`app/templates/ponto/bater_ponto_v2.html:335-340`; `app/api/views_core.py:555-568`, `app/api/views_core.py:695-721`).
- Onboarding incompleto desvia antes da batida web (`app/ponto/views.py:240-243`); comunicado desvia somente GET (`app/ponto/views.py:249-254`).

## 29/10 18:17 · sessao 86a7408a2cd1 · - · - · origem=Ronald (tmux)

Achados dos testes, por leitura e AST em memória, sem banco: [nao verificado]

- **Selo tem mordida estrutural limitada.** Removendo em memória as chamadas reais, ele acusa `aprovar_justificativa_dp`, `materializar_saida_dp` e `aprovar_lote_justificativas`; o reconhecimento e a checagem ficam em `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:47` e `:94`. Baseline dos dois arquivos: nenhuma violação. Execução funcional das mutações: [nao verificado].
- **Selo aceita fechamento sem efeito ou falha engolida.** Provei com fontes sintéticos: `if False: encerrar_chamado_de_origem(j)`, `try: encerrar... except Exception: pass` e fechamento antes de `registrar_batida(j)` retornam `[]`; `_chama` apenas compara posição textual, receptor e ancestral `atomic`, em `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:99`. O ramo morto é limitação declarada em `:8`; captura da falha e pré-requisito não são verificados em `:102`.
- **Ausência de sinal não reprova o teste da batida.** `test_J2_nao_e_rejeitada_pela_BATIDA_do_proprio_ato` exige `batida_gerada_id`, J2 pendente e chamado vivo, mas não exige invocação do receiver/reconciliador, em `app/ponto/tests/test_justificativa_encerra_origem.py:413`. Sem `_reconciliador_post_save_batida`, a chamada explícita ainda retém o chamado, em `app/ponto/services/justificativa.py:194` e `app/chamados/reconciliador.py:453`. Portanto esse teste pode passar com o caminho anunciado ausente; comparação executada contra tal mutante: [nao verificado].
- **O caso de falha repetida das perguntas não prova o savepoint S143.** Ele exige rollback completo em `app/ponto/tests/test_justificativa_encerra_origem.py:256`; o resolver sem captura da unidade S128 já produz esse rollback em `app/chamados/reconciliador.py:521`. O teste específico que mira a cura do savepoint é `test_erro_de_banco_engolido_no_receiver_S143_nao_mente_o_fechamento`, em `app/ponto/tests/test_justificativa_encerra_origem.py:231`.
- **Tetos não provam a existência do savepoint.** `_cobrar` aceita qualquer contagem menor ou igual, em `app/ponto/tests/test_selo_chokepoint_escrita.py:213`; retirar o savepoint diminui queries e permanece dentro dos novos tetos de `:114` e `:157`. É comportamento adequado de teto, mas não é prova funcional da cura. As medições alegadas em `:150` dependem de logs não fornecidos: [nao verificado].
- **Sem ensaio novo de concorrência ou pós-commit.** Os novos funcionais usam `TestCase`, em `app/ponto/tests/test_justificativa_encerra_origem.py:19` e `:38`; não há teste de threads, `TransactionTestCase` ou execução explícita de callbacks no módulo. Segurança da ordem de locks e propagação do contexto em callbacks, por teste: [nao verificado].
- **Há sentinelas úteis contra vacuidade.** Rollback do fechamento exige `m.assert_called_once()` em `app/ponto/tests/test_justificativa_encerra_origem.py:170`; falha da saída idem em `:197`; recusa do coletor exige trilha em `:159`; retenção por irmã exige trilha em `:356`; o caso J2 direto exige chamada do helper em `:290`.
- **O positivo da materialização com wrapper é bem escolhido contra fechamento prévio pelo reconciliador da batida.** A fixture usa `justificativa_colab`, em `app/ponto/tests/test_justificativa_encerra_origem.py:213`; esse módulo não aparece no catálogo de premissas, em `app/chamados/catalogo/premissa.py:45`, e o reconciliador retorna `None` para classe fora do catálogo, em `app/chamados/reconciliador.py:257`. Assim o caminho comum da batida não substitui ali a nova chamada da origem.
- **O selo não cobre todos os escritores possíveis.** Ele reconhece somente `ast.Assign` com literal `'aprovada'`, em `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:47`; `update`, `create` e admin são exclusões declaradas em `:13`. Não usá-lo como prova de inexistência desses escritores.

## 29/10 18:17 · sessao 439554232df8 · - · - · origem=Ronald (tmux)

- `VIVOS = ('aberto', 'em_analise', 'registrado')`; `ENCERRADOS = ('resolvido', 'fechado')`. Fonte: `app/chamados/catalogo/motor.py:24`, `:30`.

- `Batida.objects.create` dispara o receiver `_nucleo_post_save_batida`, que recompõe os turnos; `_recompute_around` captura exceções. Fonte: `app/ponto/signals.py:19`, `:29`, `:34`.

- Em seguida `_cartorio_post_save_batida` chama `_julgar_dias` para véspera e dia da batida; este chama `cartorio.julgar_celula` dentro de `engolir`, que captura exceções. Fonte: `app/ponto/signals.py:85`, `:88`, `:93`, `:108`, `:131`; captura em `app/core/observ.py:85`.

- `_reconciliador_post_save_batida` chama `reconciliar_chamados_colab(instance.colaborador)` e captura exceções; esse reconciliador percorre todos os chamados VIVOS do colaborador. Fonte: `app/ponto/signals.py:206`, `:213`; `app/chamados/reconciliador.py:1203`.

- O módulo `entrada_bloqueada_turno_aberto` pertence à premissa `FATO_TURNO_ABERTO`. `_veredito_premissa` coleta o turno específico do chamado quando há dia; sem dia, consulta globalmente `TurnoMaterializado` com entrada e sem saída. Premissa viva segue no laço; morta chama `_retratar`. Fontes: `app/chamados/catalogo/premissa.py:48`; `app/chamados/reconciliador.py:264`, `:286`, `:139`, `:170`, `:1208`.

- `_retratar` pode **recusar o fechamento do chamado** se houver irmãs respondidas sem consumo; senão abre `transaction.atomic`, chama `ch.reconciliar()`, rejeita justificativa vinculada ainda pendente, resolve perguntas e anexa ocorrência. Isso é fechamento de chamado após o evento da Batida, não uma consulta prévia que barre a criação da Batida. Fontes: `app/chamados/reconciliador.py:435`, `:446`, `:455`, `:462`, `:469`, `:471`; chamada pelo receiver em `app/ponto/signals.py:215`.

- `ChamadoColaborador.reconciliar()` consulta `_decisao_do_fechamento`; o motor transforma VIVOS em `resolvido`, e `_gravar_estado` grava com via `sistema`. Fontes: `app/chamados/models.py:434`, `:440`, `:444`; `app/chamados/catalogo/motor.py:95`, `:111`; escritor em `app/chamados/models.py:185`, `:226`.

- O gate de premissa do fechamento pode recusar: se é chamado de furo com dia e o dia segue furado para folha, sem pai agrupador ou sobrevivente vivo, devolve `False` e registra `fechamento_recusado`. Sem dia, permite e registra `fechamento_sem_dia_do_fato`; também permite premissa regularizada ou dedup com outra cobrança viva do dia. Fonte: `app/chamados/models.py:484`, `:488`, `:495`, `:498`, `:522`, `:525`.

- Outra porta automática é `retratar_chamados_absolvidos_por_celula`: inclui EB em `MODULOS_DE_TURNO`, exige célula com veredito que não acusa e sem marcos pendentes, e chama a mesma `_retratar`. Não roda inline no signal: consumidor do outbox chama essa função em `app/chamados/reconciliador.py:1145`. Fontes: `app/chamados/reconciliador.py:336`, `:757`, `:792`, `:800`, `:814`, `:823`, `:825`.

- `reconciliar_chamados_colab` também é chamado pelo comando `reconciliar_chamados`, em `app/chamados/management/commands/reconciliar_chamados.py:52`.

- Não generalizar que todos os receivers são incapazes de propagar erro: `batida_marca_onboarding` não envolve a chamada de gravação em `try/except`; ele chama a porta do colaborador, que executa `colab.save`. Não há consulta ao chamado EB nesse receiver. Fontes: `app/colaboradores/signals.py:86`, `:94`, `:101`; `app/colaboradores/services/colaborador.py:47`, `:53`. Ocorrência concreta de exceção/rollback: **[não verificado]**.

## 29/10 17:58 · sessao 23e552545147 · grupo1p1 · verificador · origem=Code

- **Classe 3 — instante perdido no resumo.** `espelho_do_colab` e o coletor PDF recebem `agora`, mas chamam `_folha_manda` sem propagá-lo. Essa cadeia relê o relógio em `ate_ontem`, em `turno_aberto_de` quando há turno aberto e no fechamento online quando há folha. Referências: `app/ponto/services/espelho.py:1288`, `app/relatorios/pdf_espelho.py:624`, `app/relatorios/cartao_pela_celula.py:412`, `app/relatorios/cartao_pela_celula.py:40`, `app/relatorios/cartao_pela_celula.py:398`, `app/ponto/turnos.py:2255`, `app/relatorios/cartao_pela_celula.py:73`, `app/ponto/services/fechamento.py:1718`, `app/ponto/services/fechamento.py:595`.

- **Classe 3 — releitura na classificação do export.** Os atos capturam seu instante, mas continuam chamando `classificar_export` sem ele; a classificação chega a `motivos_retencao_celula` sem `hoje`, provocando outra leitura. Sítios: `app/folha/porta_export.py:469`, `app/ponto/services/dias_em_aberto.py:104`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:53`, `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:58` e `app/ponto/management/commands/e6_oraculo.py:269`. Cadeia: `app/folha/export.py:942`, `app/folha/export.py:1249`, `app/folha/export.py:1179`.

- **Classe 3 — ficha com vários instantes.** Depois da captura própria, `ficha_do_colab` chama `particionar_verbos`, que lê outro relógio; com propostas habilitadas, também alcança a leitura de `_recusadas_recentes`. Referências: `app/colaboradores/services/ficha.py:117`, `app/colaboradores/services/ficha.py:155`, `app/chamados/services/verbos.py:32`, `app/colaboradores/services/ficha.py:176`, `app/escala/services/propositor.py:538`, `app/escala/services/propositor.py:622`.

- **Classe 3 — avaliação de flips com três leituras.** `avaliar_fila` captura `agora`, mas `fila_flip` consulta duas fontes que leem seus próprios relógios para os cortes da fila. Referências: `app/ponto/services/flip_auto.py:263`, `app/ponto/services/flip_auto.py:268`, `app/ponto/services/flip_auto.py:146`, `app/ponto/services/flip_auto.py:148`, `app/ponto/services/flip_auto.py:34`, `app/ponto/services/flip_auto.py:126`.

- **Classe 3 — PDF auditável perde o instante.** O gerador captura o instante para os dados, mas chama `gerar_pdf_auditavel` sem propagá-lo; o pipeline lê outro instante para o rodapé. Referências: `app/relatorios/pdf_espelho.py:1088`, `app/relatorios/pdf_espelho.py:1099`, `app/relatorios/pdf_base.py:133`, `app/relatorios/pdf_base.py:139`.

- **Classe 3 — reconciliação consulta outro relógio no julgamento.** O comando captura `_agora_do_ato`, mas chama `classificar_falta` sem esse instante; o juiz compara o fim previsto com um novo `now()`. Referências: `app/ponto/management/commands/reconciliar_grade.py:75`, `app/ponto/management/commands/reconciliar_grade.py:243`, `app/escala/servico_jornada.py:376`.

- **Classe 3 — duas leituras no teste do contrato.** A janela usa `timezone.localdate()` e a grade recebe um `timezone.now()` separado. Referências: `app/escala/tests/test_f1c_contrato.py:33`, `app/escala/tests/test_f1c_contrato.py:35`.

- **Classe 3 — duas leituras em `_pares`.** A mesma chamada obtém a referência por `localdate()` e o instante por `now()`, separadamente, mesmo sob congelamento. Referência: `app/ponto/tests/test_par_relampago_pelo_turno.py:55`.

- **Classe 3 — teste do cartão usa relógios separados.** O coletor recebe `now()`, mas a expectativa chama `ate_ontem` sem `hoje` e depois lê `localdate()` novamente. Referências: `app/relatorios/tests/test_cartao_pela_celula.py:58`, `app/relatorios/tests/test_cartao_pela_celula.py:61`, `app/relatorios/tests/test_cartao_pela_celula.py:62`, `app/relatorios/cartao_pela_celula.py:40`.

- **Classe 5 — selo deixa voltar o relógio interno do builder.** Repor `ref = ref or timezone.localdate()` no builder por turno mantém vazios os resultados dos três verificadores AST: a inspeção de relógios só examina funções que chamam um `ALVO`, e esse builder não chama nenhum. A verificação de assinatura permanece satisfeita. Referências: `app/escala/utils.py:1234`, `app/escala/tests/test_selo_grade_pede_o_instante.py:103`, `app/escala/tests/test_selo_grade_pede_o_instante.py:106`, `app/escala/tests/test_selo_grade_pede_o_instante.py:191`.

Total por classe: 1=0; 2=0; 3=9; 4=0; 5=1; 6=0; 7=0; 8=0.

## 29/10 17:53 · sessao 0710e4f763be · grupo1p1 · verificador · origem=Code

1. **Classe 3 — espelho/PDF perdem o instante antes de `folha_manda`.** As chamadas em `app/ponto/services/espelho.py:1288` e `app/relatorios/pdf_espelho.py:624` não repassam `agora`. `folha_manda` chama `ate_ontem` sem referência temporal (`app/relatorios/cartao_pela_celula.py:412`), provocando outra leitura em `app/relatorios/cartao_pela_celula.py:40`. Havendo turno aberto, também consulta o juiz sem instante (`app/relatorios/cartao_pela_celula.py:398`), acionando `timezone.now()` em `app/ponto/turnos.py:2255`.

2. **Classe 3 — ficha lê vários relógios no mesmo ato.** Captura `agora` em `app/colaboradores/services/ficha.py:117`, mas chama `particionar_verbos` em `app/colaboradores/services/ficha.py:155`, que relê o relógio em `app/chamados/services/verbos.py:32`. Com propostas habilitadas, a chamada de `app/colaboradores/services/ficha.py:176` chega a outra leitura em `app/escala/services/propositor.py:622`, via `app/escala/services/propositor.py:538`.

3. **Classe 3 — fila e simulações usam instantes distintos.** `avaliar_fila` captura `agora` em `app/ponto/services/flip_auto.py:263`, mas chama `fila_flip` em `app/ponto/services/flip_auto.py:268`. Esta consulta duas fontes (`app/ponto/services/flip_auto.py:146`, `app/ponto/services/flip_auto.py:148`) que releem o relógio em `app/ponto/services/flip_auto.py:34` e `app/ponto/services/flip_auto.py:126`.

4. **Classe 3 — classificação do universo relê o dia.** `medir` captura o instante em `app/folha/porta_export.py:318`, mas chama `classificar_export` sem transmiti-lo em `app/folha/porta_export.py:469`. A cadeia `app/folha/export.py:942` → `app/folha/export.py:1249` chega a `localdate()` em `app/folha/export.py:1179`. A mesma falha alcança os chamadores tocados em `app/ponto/services/dias_em_aberto.py:104`, `app/ponto/management/commands/e6_oraculo.py:269`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:53` e `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:58`.

5. **Classe 3 — PDF auditável ganha outro instante.** O instante capturado em `app/relatorios/pdf_espelho.py:1088` alimenta os dados e o texto de geração, mas não segue na chamada de `app/relatorios/pdf_espelho.py:1099`. O pipeline relê o relógio em `app/relatorios/pdf_base.py:133` e usa esse segundo valor no rodapé em `app/relatorios/pdf_base.py:139`.

6. **Classe 5 — selo deixa voltar a segunda leitura da API.** O verificador ignora funções sem parâmetro `agora` (`app/escala/tests/test_selo_grade_pede_o_instante.py:73`) e examina apenas chamadas aos alvos enumerados (`app/escala/tests/test_selo_grade_pede_o_instante.py:80`). Restaurar `timezone.now() < _fim_hj` em `app/api/views.py:1635`, após a captura de `app/api/views.py:1350`, continua produzindo listas vazias nos dois verificadores usados pelo selo (`app/escala/tests/test_selo_grade_pede_o_instante.py:102`, `app/escala/tests/test_selo_grade_pede_o_instante.py:108`).

Total por classe, agrupado por causa: **1=0; 2=0; 3=5; 4=0; 5=1; 6=0; 7=0; 8=0.**

## 29/10 17:52 · sessao 6c4ab6f050bb · - · - · origem=Ronald (tmux)

Achados verificáveis da classe 3, agrupados por causa:

- `app/ponto/services/espelho.py:1288` e `app/relatorios/pdf_espelho.py:624`: ambos recebem `agora`, mas chamam `folha_manda` sem propagá-lo. Ela chama `ate_ontem` sem `hoje` (`app/relatorios/cartao_pela_celula.py:412`), que lê `timezone.localdate()` (`:40`). Havendo turno aberto, também chama `turno_aberto_de` sem instante (`:398`), acionando `timezone.now()` em `app/ponto/turnos.py:2255`. Havendo fechamento, chama `totais_da_folha` (`:323`), depois `fechamento_lido` (`:73`), que chama o recalculador (`app/ponto/services/fechamento.py:1718`), cujo instante é novamente lido em `:595`. Portanto, o instante injetado não governa integralmente a mesma leitura.

- `app/colaboradores/services/ficha.py:117`: captura `agora`, mas `:155` chama `particionar_verbos`, que relê em `app/chamados/services/verbos.py:32`. Com propostas habilitadas, `ficha.py:176` chama `propostas`; esta chama `_recusadas_recentes` em `app/escala/services/propositor.py:538`, que relê em `:622`. A ficha pode usar três instantes no mesmo ato.

- `app/ponto/services/flip_auto.py:263`: captura `agora`, mas `:268` chama `fila_flip`. Esta chama `pares_mesmo_tipo` em `:146`, que lê outro instante em `:34`, e `fila_flip_recusas` em `:148`, que lê novamente em `:126`. São três leituras no ato que fornece `agora` às simulações tocadas em `:303` e `:304`.

- `app/folha/porta_export.py:469`: depois de capturar o instante em `:318`, chama `classificar_export` sem referência temporal. Este chama `motivos_retencao_espelho` em `app/folha/export.py:942`, que delega a `motivos_retencao_celula` em `:1249`; ela lê `localdate()` em `:1179`. A mesma causa aparece nos sítios tocados `app/ponto/services/dias_em_aberto.py:104`, `app/ponto/management/commands/e6_oraculo.py:269`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:53` e `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:58`.

- `app/relatorios/pdf_espelho.py:1099`: após capturar o instante em `:1088` e usá-lo para dados e texto de geração (`:1089`, `:1090`), chama `gerar_pdf_auditavel` sem esse instante. O pipeline relê o relógio em `app/relatorios/pdf_base.py:133` e usa o segundo valor no rodapé (`:139`).

Adicional, se o escopo incluir o chamador intocado da função tocada: `app/colaboradores/services/ficha.py:275` chama `ficha_do_colab`, que captura `agora` em `:117`; depois `ficha_para_o_copiloto` relê `localdate()` em `:280` quando `hoje` não é informado.

Não incluiria como achado de leitor a releitura de `app/ponto/management/commands/reconciliar_grade.py:458`: é o carimbo de uma escrita, chamado em `:228`, e não o instante empregado para julgar a grade.

## 29/10 17:51 · sessao 054f4626c827 · justif · verificador · origem=Code

**Não aprovaria o v6.** Há guardas alcançadas depois do fechamento e um ciclo de travas ainda possível. (`app/ponto/services/justificativa.py:176`, `app/ponto/services/cartorio.py:1163`, `app/ponto/services/veredito_celula.py:307`)

Os contraexemplos abaixo são inferências da árvore. Ocorrência em produção, reprodução concorrente, testes funcionais e números citados nos comentários: **[nao verificado]**. [nao verificado]

**1. Defeitos encontrados**

- **F1 — A guarda das irmãs pode chegar depois do fechamento. Classe: pouso antes do pré-requisito.** A aprovação salva J antes do helper; esse `save()` dispara o cartório, que pode chamar `reconciliar()` diretamente, sem passar pela guarda S128. Depois, o helper encontra C encerrado e retorna. (`app/ponto/services/justificativa.py:167`, `app/ponto/services/justificativa.py:176`, `app/ponto/signals.py:165`, `app/ponto/services/cartorio.py:1163`, `app/ponto/services/justificativa.py:38`)

  **Entrada → erro:** escala E07/S19, E07 real, C vivo cobrando S19, J própria pendente, pergunta S19 respondida e ainda não consumida; célula de trabalho com `julgada_em=None` antes da aprovação. J aprovada faz a grade marcar o dia justificado e a ata dispensar S19; `marcos_acesos` permite o fechamento direto. Resultado: C terminal com resposta aguardando consumo. A célula não é pulada porque ainda não foi julgada. (`app/escala/utils.py:1198`, `app/ponto/services/cartorio.py:608`, `app/ponto/services/lastro.py:170`, `app/ponto/services/cartorio.py:1092`, `app/chamados/services/disputa_emissao.py:1558`)

- **F2 — A guarda contra decidir J2 também chega tarde. Classe: pouso antes do pré-requisito.** Na aprovação individual e na materialização, registrar a saída antecede o helper. O signal da batida chama o reconciliador, cuja unidade auto-rejeita a justificativa relacionada pendente. (`app/ponto/services/justificativa.py:147`, `app/ponto/services/justificativa.py:328`, `app/ponto/signals.py:214`, `app/chamados/reconciliador.py:1214`, `app/chamados/reconciliador.py:468`)

  **Entrada → erro:** C `turno_aberto` de D aponta J2 pendente de D+1; J1 pendente de D aponta C e informa E/S que fecham o turno; sem célula e sem irmãs. A batida fecha C pelo fallback dos turnos e auto-rejeita J2. Depois J1 é aprovada, e o helper retorna pelo estado terminal antes de consultar a guarda nova. Resultado: aprovar J1 decidiu J2. (`app/chamados/reconciliador.py:264`, `app/chamados/models.py:978`, `app/chamados/reconciliador.py:462`, `app/ponto/services/justificativa.py:38`, `app/ponto/services/justificativa.py:44`)

  A cascata também contorna essa guarda: se C é alvo da cascata e aponta outra pendente, `encerrar()` força seu fechamento antes do helper. Nesse ramo J2 pode continuar pendente sob C fechado. (`app/ponto/services/justificativa.py:369`, `app/ponto/services/justificativa.py:377`, `app/chamados/models.py:1192`)

- **F3 — Permanece o ciclo chamado ↔ `TurnoMaterializado`. Classe: ordem de trava.** A aprovação segura C e J, depois registra batida e tenta reconstruir TM. `veredito_celula` pode segurar TM na sua transação e depois tentar fechar C. Escalonamento: B segura TM; A segura C; A espera TM; B espera C. (`app/ponto/services/justificativa.py:89`, `app/ponto/services/justificativa.py:101`, `app/ponto/services/justificativa.py:147`, `app/ponto/signals.py:36`, `app/ponto/nucleo.py:59`, `app/ponto/services/veredito_celula.py:203`, `app/ponto/services/veredito_celula.py:307`, `app/ponto/services/veredito_celula.py:324`)

- **F4 — A pré-trava não garante o conjunto após nascimento ou vinculação concorrente. Classe: ordem de trava.** Ela lê o ponteiro sem travar J e depois trava os chamados encontrados naquela consulta. A API pode criar C e preencher a origem anteriormente nula entre essa consulta e a trava de J. Assim, a promessa de cobrir nascimento concorrente não é garantida. (`app/ponto/services/justificativa.py:74`, `app/ponto/services/justificativa.py:80`, `app/ponto/services/justificativa.py:89`, `app/api/views.py:2033`, `app/api/views.py:2055`)

  **Interleaving → erro:** A termina a pré-trava; B cria C, liga J nos dois sentidos e conclui; A trava J; R começa a retratar C, lê J ainda pendente e espera sua escrita; A espera C no helper. Existe entrada pública de retratação por módulo, além da unidade que escreve C antes de J. Esse ciclo específico não foi reproduzido em banco **[nao verificado]**. (`app/api/views.py:2069`, `app/ponto/services/justificativa.py:101`, `app/chamados/reconciliador.py:455`, `app/chamados/reconciliador.py:468`, `app/ponto/services/justificativa.py:37`, `app/chamados/management/commands/retratar_chamados_lei_cala.py:24`, `app/chamados/management/commands/retratar_chamados_lei_cala.py:59`)

- **F5 — O lote ainda aprova quando a batida pertence a outra justificativa. Classe: outra, inconsistência do ato. Remanescente no trecho tocado.** `_outra is not None` apenas evita vincular a batida; o lote continua, fecha a origem e conta aprovação. (`app/ponto/views.py:1825`, `app/ponto/views.py:1833`, `app/ponto/views.py:1834`)

  **Entrada → erro:** J1 pendente com saída informada; a porta deduplica para S já vinculada a J2 aprovada. Resultado: J1 aprovada, origem encerrada, `batida_gerada` de J1 nula e resposta de sucesso. A porta individual recusa precisamente essa situação. (`app/ponto/registro_batida.py:113`, `app/ponto/views.py:1826`, `app/ponto/services/justificativa.py:157`)

- **F6 — A materialização pode produzir duas transições de estado. Classe: outra, violação da trilha única. Remanescente anterior ao v6.** A saída pode resolver C pelo signal; a cascata o seleciona novamente porque `NAO_FECHADOS` inclui `resolvido`, e força `resolvido → fechado`. Cada passagem grava `estado_chamado`. (`app/ponto/signals.py:214`, `app/chamados/catalogo/motor.py:32`, `app/ponto/services/justificativa.py:61`, `app/ponto/services/justificativa.py:369`, `app/chamados/catalogo/motor.py:115`, `app/chamados/models.py:227`)

  **Entrada → erro:** origem `turno_aberto`, J própria, E existente, S materializada, sem irmãs. Resultado: duas linhas de estado no primeiro ato; o helper posterior ser idempotente não elimina essas duas linhas. (`app/ponto/services/justificativa.py:328`, `app/chamados/reconciliador.py:456`, `app/ponto/services/justificativa.py:377`)

- **F7 — O fecho legado continua aplicando uma regra própria a outro chamado. Classe: outra, leitor com regra própria de fechar. Remanescente no sítio tocado.** O `exclude` protege a origem, mas a busca continua sem filtro de dia e fecha outro `entrada_bloqueada_turno_aberto`, fora da transação da aprovação e sem guarda das irmãs. (`app/ponto/services/justificativa.py:203`, `app/ponto/services/justificativa.py:207`, `app/ponto/services/justificativa.py:209`)

  **Entrada → erro:** aprovar J de D1 enquanto o chamado mais recente desse módulo pertence a D2, ainda irregular ou com irmã respondida. Resultado: C de D2 é forçado a fechado pelo ato sobre D1. (`app/ponto/services/justificativa.py:204`, `app/ponto/services/justificativa.py:209`, `app/chamados/models.py:1193`)

**2. Escritores não alcançados**

Sim: **FORA-DO-ESCOPO**, o Django admin continua editável para Justificativa, incluindo `status='aprovada'`, sem costura explícita de fechamento. O próprio selo declara essa lacuna. (`app/ponto/admin.py:18`, `app/ponto/models.py:159`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:13`)

As três atribuições explícitas encontradas estão alcançadas: individual, materialização e lote. (`app/ponto/services/justificativa.py:164`, `app/ponto/services/justificativa.py:350`, `app/ponto/views.py:1795`)

Não encontrei escritor explícito de `status_local` fora das portas nas formas cobertas pelo censo AST; isso não prova ausência de escrita dinâmica ou genérica. As portas autorizadas e o universo da varredura estão no selo. (`app/chamados/tests/test_selo_status_so_pelas_portas.py:17`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:37`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:322`)

**3. Mordida dos testes e tetos** [nao verificado]

- Os positivos de individual, lote e materialização verificam aprovação e fechamento; o de falha do fechamento exige que a porta tenha sido chamada antes de conferir rollback. Portanto há cobertura da costura nesses cenários. (`app/ponto/tests/test_justificativa_encerra_origem.py:73`, `app/ponto/tests/test_justificativa_encerra_origem.py:85`, `app/ponto/tests/test_justificativa_encerra_origem.py:171`, `app/ponto/tests/test_justificativa_encerra_origem.py:226`)

- **Ausência de sinal:** `test_aprovar_J1_nao_rejeita_a_J2_pendente_que_o_chamado_aponta` passaria sem chamar o fechamento: J1 aprovada, J2 pendente e C aberto são também o resultado de nunca tentar fechar. Ele cobre a guarda mantendo a costura, mas não demonstra sua execução nesse cenário. **Classe: selo/RED sem mordida da costura.** (`app/ponto/tests/test_justificativa_encerra_origem.py:272`, `app/ponto/tests/test_justificativa_encerra_origem.py:289`)

- O teste da irmã na aprovação exige a trilha de retenção, portanto não passa apenas porque ninguém tentou fechar. Porém, os novos casos de irmãs não criam célula e deixam de exercitar F1. O teste da cascata também passaria retirando todo o fechamento; retirando somente sua guarda, deve falhar. (`app/ponto/tests/test_justificativa_encerra_origem.py:352`, `app/ponto/tests/test_justificativa_encerra_origem.py:299`, `app/ponto/tests/test_justificativa_encerra_origem.py:340`, `app/ponto/services/cartorio.py:1548`, `app/ponto/tests/test_justificativa_encerra_origem.py:315`)

- O RED direto do S143 verifica chamada, retorno verdadeiro, estado persistido e uma trilha: está desenhado para detectar o rollback silencioso sem seu savepoint. Já o teste de erro repetido continuaria passando sem esse savepoint, pois exige rollback integral. (`app/ponto/tests/test_justificativa_encerra_origem.py:250`, `app/ponto/tests/test_justificativa_encerra_origem.py:254`, `app/ponto/tests/test_justificativa_encerra_origem.py:256`, `app/ponto/tests/test_justificativa_encerra_origem.py:270`)

- Os testes de erro de banco por `tipo='x'*40` dependem de o backend rejeitar esse valor. O teste do UPDATE injeta uma data inválida; não demonstra uma falha emitida pelo servidor nem verifica explicitamente o estado de rollback. A mordida real desses testes no backend instalado ficou **[nao verificado]**. (`app/ponto/tests/test_justificativa_encerra_origem.py:242`, `app/ponto/tests/test_justificativa_encerra_origem.py:362`, `app/ponto/tests/test_justificativa_encerra_origem.py:380`, `app/ponto/tests/test_justificativa_encerra_origem.py:382`)

- **Tetos:** o custo acrescentado é por execução do receiver em `post_save` terminal, inclusive saves que não mudam estado; “por chamado que fecha” é impreciso. A aritmética declarada é compatível com dois comandos por execução, mas as contagens medidas permanecem **[nao verificado]**. Como os testes cobram `<= teto`, retirar o savepoint não os reprova; essa responsabilidade é do RED funcional. (`app/chamados/signals.py:81`, `app/chamados/signals.py:91`, `app/ponto/tests/test_selo_chokepoint_escrita.py:150`, `app/ponto/tests/test_selo_chokepoint_escrita.py:210`)

- **F7 anterior corrigido:** o selo compara o receptor da atribuição, do `save()` e do argumento do fechamento; existe teste explícito com outro objeto. A limitação de não provar execução do ramo continua declarada. (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:102`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:103`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:207`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:9`)

**4. Ordem das travas**

No conjunto já existente, individual, rejeição individual, materialização e lote agora pretravam chamados antes de J. Isso corrige a inversão direta apontada na rejeição individual. (`app/ponto/services/justificativa.py:100`, `app/ponto/services/justificativa.py:244`, `app/ponto/services/justificativa.py:310`, `app/ponto/views.py:1792`)

O reconciliador percorre os vivos por PK, mas não os trava antecipadamente: a escrita de C ocorre em `reconciliar()`, seguida da possível escrita de J pendente. Portanto a ordem C→J continua existindo do outro lado; F3 e F4 impedem concluir que todos os ciclos foram eliminados. (`app/chamados/reconciliador.py:1203`, `app/chamados/reconciliador.py:456`, `app/chamados/reconciliador.py:468`)

**5. Efeitos dentro da transação**

**Sim, podem desfazer a aprovação.** S143 agora captura a falha após sair de seu savepoint, incluindo a obtenção do usuário sistema. Entretanto, S128 chama o resolver novamente sem captura; uma falha persistente, ou falha da nota subsequente, atravessa a transação da aprovação. Nesse caminho, desfazer tudo atende à exigência de atomicidade. (`app/chamados/signals.py:91`, `app/chamados/signals.py:93`, `app/chamados/reconciliador.py:469`, `app/chamados/reconciliador.py:471`, `app/ponto/services/justificativa.py:99`, `app/ponto/services/justificativa.py:177`)

O rebaixamento também não captura exceções. Normalmente o núcleo já normalizou a urgência antes do signal, tornando o segundo rebaixamento um no-op. (`app/chamados/signals.py:72`, `app/chamados/models.py:210`, `app/chamados/models.py:424`)

O cartório captura exceções com `engolir`, que não cria savepoint por si. Logo não há garantia geral demonstrada de isolamento de todo erro de banco desse receiver; um sucesso falso por esse caminho específico ficou **[nao verificado]**. (`app/chamados/signals.py:172`, `app/core/observ.py:85`)

Ler a própria J aprovada ainda não commitada é necessário para o gate. O erro concreto encontrado é a **ordem dos efeitos antes das guardas**, descrita em F1/F2. Os novos savepoints de `invalidar_fechamento` envolvem corretamente a coleta e cada UPDATE, com captura externa; não identifiquei ali um novo meio-estado da aprovação. (`app/chamados/models.py:937`, `app/ponto/services/fechamento.py:1616`, `app/ponto/services/fechamento.py:1619`, `app/ponto/services/fechamento.py:1629`)

## 29/10 17:51 · sessao 643d6293abcc · - · - · origem=Ronald (tmux)

- **Classe 5 — selo não detecta a volta da segunda leitura de relógio.** Em `app/escala/tests/test_selo_grade_pede_o_instante.py:73`, o verificador ignora funções sem parâmetro `agora`; em `:80`, examina somente chamadas aos `ALVOS`. Restaurar em `app/api/views.py:1635` o trecho anterior `timezone.now() < _fim_hj`, após a leitura de `:1350`, produz `[]` nos dois verificadores do selo (`test_selo_grade_pede_o_instante.py:102` e `:108`). Confirmei essa mutação em memória, executando os auxiliares AST, sem gravar arquivos. A assinatura permanece igual, portanto as verificações de assinatura em `:139` e `:145` também não detectam a regressão.

## 29/10 17:50 · sessao 5ecb2ed6553f · - · - · origem=Ronald (tmux)

Nenhum achado verificável de classe 7 no censo dos chamadores das assinaturas alteradas.

O aparente chamador sem `agora=` em `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:53` recebe o argumento via `**kw`, preenchido em `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:52`.

Os chamadores de `Command.apurar` passam `agora` em `app/ponto/management/commands/pautas_discordantes.py:112` e `app/ponto/tests/test_o320_pauta_dos_discordantes.py:189`.

## 29/10 17:48 · sessao edaf1101b86e · - · - · origem=Ronald (tmux)

Refinamento confirmado: a célula do contraexemplo deve estar elegível para julgamento, por exemplo `julgada_em=None` imediatamente antes do POST; assim o pulo por impressão igual não ocorre (`app/ponto/services/cartorio.py:1092`).

Entrada concreta: escala E07/S19, E07 real, C vivo cobrando S19, J própria pendente e pergunta S19 respondida sem validação/materialização; célula de trabalho criada após os saves preparatórios. Ao aprovar, o signal de J julga (`app/ponto/signals.py:161`–`:165`), a grade marca justificado (`app/escala/utils.py:1198`–`:1203`), a ata acende S19 por dispensa (`app/ponto/services/cartorio.py:602`–`:610`) e o cartório chama `reconciliar()` diretamente (`:1163`). O helper encontra C terminal e não consulta S128 (`app/ponto/services/justificativa.py:37`–`:39`). A proteção das irmãs fica contornada.

O savepoint S143 inclui corretamente `get_user_sistema()` (`app/chamados/signals.py:91`–`:92`), com captura após a saída do atomic (`:93`–`:94`).

Existe risco estrutural residual de banco engolido no cartório: o receiver usa `engolir` sem savepoint (`app/chamados/signals.py:171`–`:180`; `app/core/observ.py:85`–`:91`), e pode alcançar `ressuscitar_perguntas` (`app/ponto/services/cartorio.py:1271`), cujo `update` não tem atomic próprio (`app/chamados/models.py:1162`–`:1163`). Uma reprodução de aprovação retornando sucesso com C vivo nesse caminho específico ficou [nao verificado]; não acrescentaria como defeito demonstrado do diff.

Não identifiquei segundo juiz autônomo na guarda J2: ela impede decidir outra justificativa (`app/ponto/services/justificativa.py:40`–`:45`), enquanto S128 usa “pendente” para executar auto-rejeição (`app/chamados/reconciliador.py:462`–`:468`). Não há no dado contrato exigindo trilha para esse veto [nao verificado].

## 29/10 17:47 · sessao 1e7f75cfae7d · grupo1p1 · verificador · origem=Code

- **Classe 3 — espelho perde o instante na fonte dos totais.** `app/ponto/services/espelho.py:1288` chama `folha_manda` sem repassar `agora`. Ela consulta turno vivo e véspera com novos relógios em `app/relatorios/cartao_pela_celula.py:398` e `app/relatorios/cartao_pela_celula.py:412`, acionando `app/ponto/turnos.py:2255` e `app/relatorios/cartao_pela_celula.py:40`.

- **Classe 3 — fila e simulação usam relógios diferentes.** `app/ponto/services/flip_auto.py:263` captura `agora`, mas a fila chamada em `app/ponto/services/flip_auto.py:268` lê novamente em `app/ponto/services/flip_auto.py:34` e `app/ponto/services/flip_auto.py:126`.

- **Classe 3 — ficha ainda lê novamente para classificar pendências.** Após capturar o instante em `app/colaboradores/services/ficha.py:117`, chama `particionar_verbos` em `app/colaboradores/services/ficha.py:155`; essa função lê outro instante em `app/chamados/services/verbos.py:32`, usado para silêncio e idade em `app/chamados/services/verbos.py:48` e `app/chamados/services/verbos.py:54`.

- **Classe 3 — PDF auditável mantém segundo relógio.** O instante capturado em `app/relatorios/pdf_espelho.py:1088` não acompanha a chamada em `app/relatorios/pdf_espelho.py:1099`. O pipeline captura outro em `app/relatorios/pdf_base.py:133`, usado no rodapé em `app/relatorios/pdf_base.py:139`.

- **Classe 3 — reconciliação relê no ramo de aplicação.** O comando captura o instante em `app/ponto/management/commands/reconciliar_grade.py:75`, mas chama `_relavrar_orfa` em `app/ponto/management/commands/reconciliar_grade.py:228`, que lê novamente para `relavrada_em` em `app/ponto/management/commands/reconciliar_grade.py:458`.

- **Classe 5 — selo aceita restaurar leitura interna.** O detector em `app/escala/tests/test_selo_grade_pede_o_instante.py:57` apenas exige `agora=` diferente de literal `None`. Trocar a propagação em `app/escala/utils.py:2277` por `agora=timezone.now()` mantém essa verificação aprovada; a asserção em `app/escala/tests/test_selo_grade_pede_o_instante.py:78` não distingue os dois casos.

- **Classe 8 — migração muda os cenários temporais dos testes.** `app/escala/tests/test_f1c_contrato.py:36` injeta 2100 numa janela construída em torno do hoje real em `app/escala/tests/test_f1c_contrato.py:34`; o contrato usa 2100 como hoje em `app/escala/utils.py:2272`, retirando o cenário futuro que a asserção pretende verificar. Também `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:206` injeta 2100 no cenário congelado em julho/2026 de `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:328`: o teto do coletor em `app/relatorios/pdf_espelho.py:287` passa a admitir a competência inteira, embora `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:330` exija excluir o dia corrente.

Total por classe: 1=0; 2=0; 3=5; 4=0; 5=1; 6=0; 7=0; 8=1.

## 29/10 17:47 · sessao b48a37d3b9a3 · - · - · origem=Ronald (tmux)

Achados estáticos para consolidar; execução concorrente em banco `[nao verificado]`.

1. **F3 permanece — ordem de trava C → TurnoMaterializado contra TurnoMaterializado → C.** A aprovação trava chamados em `app/ponto/services/justificativa.py:89`, trava J em `:101`, e registra saída em `:147`; o signal recomputa turnos em `app/ponto/signals.py:34`, com DELETE/INSERT de `TurnoMaterializado` em `app/ponto/nucleo.py:57`. O concorrente `veredito_celula` mantém atomic em `app/ponto/services/veredito_celula.py:203`, registra batidas em `:300`, recomputa em `:307`, e depois fecha chamados em `:324`, via `:182`. Escalonamento: B segura TM; A segura C; A espera TM; B espera C. A pretrava nova não abrange TM (`app/ponto/services/justificativa.py:84`).

2. **F5 permanece no ramo que registra saída — guarda J2 chega depois do efeito.** Entrada concreta: C `turno_aberto` do dia D, sem CelulaDia, aponta J2 pendente de D+1; J1 de D aponta C e informa entrada/saída. A saída registrada em `app/ponto/services/justificativa.py:147` recomputa TM e dispara reconciliador (`app/ponto/signals.py:214`); sem célula, cartório retorna em `app/ponto/services/cartorio.py:1547`; o reconciliador verifica turno fechado em `app/chamados/reconciliador.py:264`, gate aceita pelo fallback dos turnos em `app/chamados/models.py:978`, e `_retratar` rejeita J2 em `app/chamados/reconciliador.py:462`. A guarda nova só aparece depois, em `app/ponto/services/justificativa.py:44`, mas C já encerrado retorna antes em `:38`. Resultado: aprovar J1 rejeita J2. O teste novo usa wrapper sem batida e não cobre esse ramo (`app/ponto/tests/test_justificativa_encerra_origem.py:277`, `:284`).

3. **A pretrava não elimina novos chamados ou novo ponteiro depois da consulta.** O ponteiro é lido sem trava em `app/ponto/services/justificativa.py:80`; o conjunto é travado em `:89`. Produção pode preencher origem nula depois: `app/ponto/views.py:1631` ou `app/api/views.py:2055`; a API pode criar wrapper em `app/api/views.py:2032` e apontar reverso em `:2067`. Escalonamento possível: A conclui pretrava; B cria C e liga J; A trava J; R retrata C e espera J (`app/chamados/reconciliador.py:455`, `:468`); A espera C (`app/ponto/services/justificativa.py:37`). O comentário que promete cobrir nascimento concorrente não corresponde ao conjunto fixado pela consulta (`app/ponto/services/justificativa.py:74`). Reprodução `[nao verificado]`.

4. **Possível bypass adicional das irmãs, para confirmação no consolidado.** O `save()` de J precede helper (`app/ponto/services/justificativa.py:167`, `:353`), dispara cartório (`app/ponto/signals.py:161`) e esse chama `reconciliar()` diretamente em `app/ponto/services/cartorio.py:1163`, `:1175`, `:1196`, sem unidade S128. Os testes novos de irmãs não criam CelulaDia (`app/ponto/tests/test_justificativa_encerra_origem.py:299`, `:340`); portanto não medem esse fechamento anterior. Contraexemplo completo com os protestos calculados `[nao verificado]`.

`rejeitar_lote_justificativas` não usa atomic próprio (`app/ponto/views.py:1868`), então não afirmo inversão persistente J → C ali em autocommit. `JustificativaAdmin` continua ModelAdmin editável (`app/ponto/admin.py:18`); atomic interno do framework não está na cópia consultada `[nao verificado]`.

## 29/10 17:47 · sessao 1842696154ae · - · - · origem=Ronald (tmux)

- **Escritor não alcançado: Django admin, FORA-DO-ESCOPO.** `JustificativaAdmin` é um `admin.ModelAdmin` genérico, sem bloqueio/override do save; `status` é editável e aceita `aprovada`. Entrada concreta: admin altera J pendente para aprovada → save genérico sem `encerrar_chamado_de_origem`. O selo admite expressamente essa lacuna. (`app/ponto/admin.py:18`, `app/ponto/models.py:130`, `app/ponto/models.py:159`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:13`)

- **Censo de atribuições de aprovação:** os escritores de Justificativa encontrados são um a um, materialização e lote; os três chamam o fechamento dentro do atomic. A atribuição de férias é outro modelo. (`app/ponto/services/justificativa.py:164`, `app/ponto/services/justificativa.py:176`, `app/ponto/services/justificativa.py:350`, `app/ponto/services/justificativa.py:377`, `app/ponto/views.py:1795`, `app/ponto/views.py:1838`, `app/ferias/services.py:419`)

- **`status_local`:** executei em memória o censo AST do selo, que percorreu 739 arquivos e encontrou zero violações; isso cobre as formas declaradas pelo próprio selo, com os limites de tipagem/dinamismo registrados nele. As portas permitidas estão na allowlist. (`app/chamados/tests/test_selo_status_so_pelas_portas.py:10`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:17`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:37`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:322`)

- **Ausência de sinal no teste de J1/J2:** `test_aprovar_J1_nao_rejeita_a_J2_pendente_que_o_chamado_aponta` passaria retirando a chamada do fechamento da aprovação: ele só exige J1 aprovada, J2 pendente e chamado aberto, precisamente o resultado de nunca tentar fechar. Como teste específico da guarda, morde removendo a guarda e mantendo a chamada; como teste da costura, não morde. Classe: **selo/RED sem mordida**, limitada a esse cenário. (`app/ponto/tests/test_justificativa_encerra_origem.py:272`, `app/ponto/tests/test_justificativa_encerra_origem.py:286`, `app/ponto/tests/test_justificativa_encerra_origem.py:289`)

- **Irmãs:** o teste da aprovação exige a trilha `retratacao_retida_irma_orfa`, portanto não passa simplesmente por ausência do fechamento. O teste da cascata exige aprovação efetiva e chamado vivo; removendo só a guarda da cascata, o `encerrar()` passa a fechar e contraria a asserção. Ele ainda passaria removendo todo o fechamento da cascata e da origem. (`app/ponto/tests/test_justificativa_encerra_origem.py:313`, `app/ponto/tests/test_justificativa_encerra_origem.py:315`, `app/ponto/tests/test_justificativa_encerra_origem.py:349`, `app/ponto/tests/test_justificativa_encerra_origem.py:352`, `app/ponto/services/justificativa.py:367`, `app/ponto/services/justificativa.py:369`)

- **Erro de banco S143:** o teste direto exige que o mock seja chamado, retorno verdadeiro, chamado encerrado e uma trilha; está desenhado para distinguir o rollback silencioso. O teste da falha repetida exige chamada e rollback integral, mas, isoladamente, também passaria sem o savepoint S143, pois esse resultado continuaria sendo rollback integral. (`app/ponto/tests/test_justificativa_encerra_origem.py:241`, `app/ponto/tests/test_justificativa_encerra_origem.py:250`, `app/ponto/tests/test_justificativa_encerra_origem.py:254`, `app/ponto/tests/test_justificativa_encerra_origem.py:261`, `app/ponto/tests/test_justificativa_encerra_origem.py:267`)

- **Teste UPDATE do carimbo:** injeta uma data inválida, não uma falha emitida pelo servidor; só exige que `timezone.now` seja chamado e o ato sobreviva. Ausência de `FechamentoMensal` não basta para declarar vacuidade, pois o UPDATE precisa preparar os valores mesmo sem linhas correspondentes. Se a preparação ocorre dentro de `mark_for_rollback_on_error` na versão instalada, o teste morde sem savepoint; essa implementação Django e a execução com banco ficaram **[nao verificado]**. (`app/ponto/tests/test_justificativa_encerra_origem.py:379`, `app/ponto/tests/test_justificativa_encerra_origem.py:382`, `app/ponto/tests/test_justificativa_encerra_origem.py:385`)

- **Tetos:** o custo acrescentado é por execução do receiver em `post_save` de chamado terminal, pois ele não verifica transição nem `update_fields`; portanto “por chamado que fecha” é uma descrição incompleta. O próprio comentário prevê dois disparos no cenário de um chamado. A alteração é compatível com SAVEPOINT+RELEASE por execução; os números medidos permanecem **[nao verificado]** sem banco. Os tetos usam `<=`, portanto remover o savepoint não os reprovaria; a garantia funcional depende do RED separado. (`app/chamados/signals.py:75`, `app/chamados/signals.py:81`, `app/chamados/signals.py:91`, `app/ponto/tests/test_selo_chokepoint_escrita.py:148`, `app/ponto/tests/test_selo_chokepoint_escrita.py:150`, `app/ponto/tests/test_selo_chokepoint_escrita.py:210`)

- **F7 corrigido:** o selo agora compara o receptor da atribuição, do save e do primeiro argumento do fechamento; há mutação explícita para outro objeto. Executei os dois selos em memória, substituindo somente `SimpleTestCase` por `unittest.TestCase`: 22 testes passaram, sem Django nem banco; execução Django funcional **[nao verificado]**. (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:81`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:102`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:103`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:207`)

## 29/10 17:46 · sessao edaf1101b86e · - · - · origem=Ronald (tmux)

Achados confirmados por leitura:

1. **Guarda das irmãs pode chegar tarde.** `j.save()` ocorre antes de `encerrar_chamado_de_origem` (`app/ponto/services/justificativa.py:167`, `:176`; lote `app/ponto/views.py:1798`, `:1833`). O signal da justificativa julga a célula imediatamente (`app/ponto/signals.py:161`–`:165`, `:88`–`:93`). A grade reconhece a justificativa aprovada e marca o dia justificado (`app/escala/utils.py:1157`–`:1159`, `:1198`–`:1203`); o cartório dispensa as lâmpadas (`app/ponto/services/cartorio.py:602`–`:610`) e fecha chamados diretamente por `reconciliar`, sem a guarda S128 (`:1163`, `:1175`, `:1196`).

   Contraexemplo: célula de trabalho com saída S19 apagada; chamado vivo com `marcos_faltantes=[{'hora':'19:00','tipo':'S'}]`; justificativa própria pendente; pergunta respondida “19:00”, não validada/materializada. Aprovar a justificativa acende a lâmpada por dispensa; `marcos_acesos` retorna verdadeiro (`app/ponto/services/lastro.py:170`–`:181`), o gate aceita a justificativa própria aprovada (`app/chamados/models.py:937`–`:938`) e fecha antes da guarda. O helper encontra chamado terminal e retorna (`app/ponto/services/justificativa.py:37`–`:39`). O resolver preserva a resposta ainda não consumida (`app/chamados/services/disputa_emissao.py:1558`–`:1560`). **Classe: pouso antes do pré-requisito / guarda contornada.**

2. **Guarda J2 pendente também pode chegar tarde.** As portas unitária e materialização registram a saída antes de chamar o helper (`app/ponto/services/justificativa.py:147`–`:153`, `:328`–`:335`, `:176`, `:377`). O signal da batida chama o reconciliador (`app/ponto/signals.py:206`–`:215`); ele encontra a premissa de turno morta e chama `_retratar` (`app/chamados/reconciliador.py:264`–`:289`, `:1203`–`:1214`), que auto-rejeita a justificativa relacionada pendente (`:462`–`:468`).

   Contraexemplo: J1 aprovada com saída que fecha o turno; origem C aponta J2 pendente; sem irmãs. A batida resolve C e rejeita J2 antes da guarda `app/ponto/services/justificativa.py:44`. Depois o helper retorna porque C já está encerrado (`:38`). **Classe: pouso antes do pré-requisito / outra: decisão colateral de J2.**

3. **A cascata antecede a guarda J2.** Ela chama `encerrar()` em cada alvo (`app/ponto/services/justificativa.py:361`–`:369`), antes do helper (`:377`). `encerrar` força fechamento sem gate da premissa (`app/chamados/models.py:1192`–`:1202`). Se a origem pertence à cascata e aponta outra justificativa pendente, termina fechada antes da guarda de `:44`; a cascata não auto-rejeita J2, mas contorna o resultado prometido pela guarda. **Classe: pouso antes do pré-requisito.**

4. **Residual de duas trilhas na materialização:** `NAO_FECHADOS` contém `resolvido` (`app/chamados/catalogo/motor.py:32`). A saída pode fazer o signal resolver C antes da cascata; `_cascata_do_turno` volta a selecionar esse C (`app/ponto/services/justificativa.py:61`, `:361`) e `encerrar()` faz `resolvido → fechado` (`app/chamados/catalogo/motor.py:114`–`:115`). São duas transições e duas linhas `estado_chamado` (`app/chamados/models.py:227`–`:231`). Esse mecanismo da cascata já existia no trecho fornecido; apontar como residual preexistente, não atribuir sua introdução ao v6.  

Sobre os efeitos:

- Rebaixamento não captura exceções (`app/chamados/signals.py:68`–`:72`). No caminho normal `_gravar_estado` já normaliza a urgência antes do save (`app/chamados/models.py:210`), de modo que `_rebaixar_sem_post_save` retorna sem update (`:424`–`:426`). Uma exceção propagada nessa execução atravessa o atomic da aprovação (`app/ponto/services/justificativa.py:99`–`:181`).
- S143 agora tem savepoint próprio e captura fora dele (`app/chamados/signals.py:88`–`:94`), portanto o erro direto do resolver fica isolado. S128 chama o resolver novamente sem captura (`app/chamados/reconciliador.py:455`–`:473`); erro repetido desfaz a aprovação inteira.
- O cartório usa `engolir` (`app/chamados/signals.py:171`–`:180`), cujo corpo somente captura, sem savepoint próprio (`app/core/observ.py:85`–`:91`). Suas portas de persistência da célula têm atomic próprio (`app/ponto/portas/celula.py:634`–`:642`). Uma garantia geral de isolamento de todo erro de banco no cartório não foi demonstrada [nao verificado].
- O risco concreto demonstrável pela leitura é **ordem de efeitos síncronos** antes das guardas, acima; ler as próprias alterações ainda não commitadas é necessário para que o gate veja a justificativa aprovada (`app/chamados/models.py:937`–`:938`), não constitui sozinho erro.
- `invalidar_fechamento` protege a coleta da competência e cada update com atomic separado (`app/ponto/services/fechamento.py:1616`–`:1624`), capturando fora (`:1629`–`:1633`). Se falhar na segunda competência, a primeira permanece invalidada e a função devolve zero; é o comportamento documentado no novo comentário (`:1614`–`:1615`), não evidência de aprovação parcialmente gravada.

Não executei banco nem testes; ocorrências reais desses contraexemplos em produção estão [nao verificado]. [nao verificado]

_205 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
