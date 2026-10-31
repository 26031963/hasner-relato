# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 22:36** · publicado em **30/10/2026 22:37** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 22:36 · sessao ce5c5ea4f5f2 · - · - · origem=Ronald (tmux)

Achados do cartório, por leitura estática; execução e contagens reais permanecem [não verificado].

- **A segunda passada pode deixar trabalho pendente e impressão velha.** Um chamado adiado entra em `rejulgar`; na segunda passada, quando `_porm` finalmente o fecha, a célula é novamente enfileirada. O wrapper descarta essa fila e incrementa `rejulgar_descartado`. A impressão foi calculada com o chamado vivo antes do fechamento e é gravada depois; sem emissão nova, a releitura final não roda. Caminho: `app/ponto/services/cartorio.py:1075`, `:1089`, `:1181`, `:1187`, `:1190`, `:1434`, `:1449`, `:751`, `:757`, `:804`. Isso ocorre quando E2 mata a pergunta mas o destino continua vivo até `_porm`; E2 fecha o **pai da disputa**, que pode ser outro chamado (`app/chamados/reconciliador.py:623`, `:632`; `app/chamados/signals.py:32`, `:42`). Número de ocorrências [não verificado].

- **O ciclo `cobranca_indevida` permanece, incluindo `COBRANCA_SEM_FURO`.** O delta só altera o ramo `not cods` de `cobrado`; o suprajuiz continua emitindo `COBRANCA_SEM_FURO` quando o dia completo tem chamado vivo, e `COBRANCA_EM_DIA_SEM_TRABALHO` na folga com chamado vivo. Ambos derivam `cobranca_indevida`, que mantém C6 vivo; E2 exclui vereditos acusatórios. Fontes: `app/ponto/services/cartorio.py:1141`, `:1164`; `app/ponto/supra_juiz.py:418`, `:441`; `app/escala/models.py:1581`, `:1735`; `app/chamados/juizes.py:1054`; `app/chamados/reconciliador.py:598`. O teste específico da porta é `app/colaboradores/tests/test_contract_porta_2ato.py:147`, com exigência terminal em `:158`; vermelho segundo o anexo, execução independente [não verificado].

- **E2 dentro do julgamento tem efeitos além do carimbo e alcance além das células julgadas.** Sua consulta abrange perguntas absolvidas de todo o colaborador; fecha disputas, cujo receiver reconcilia o pai e escreve nota. Tudo isso roda com `_tl.ativo=True`; o julgamento provocado pela mudança do pai é suprimido. A segunda passada só recebe a fila selecionada, sem incorporar automaticamente outros dias alterados por E2. Fontes: `app/ponto/services/cartorio.py:738`, `:744`, `:750`, `:751`; `app/chamados/reconciliador.py:584`, `:632`; `app/chamados/signals.py:41`, `:174`. Ocorrência concreta em produção [não verificado].

- **Não há prova suficiente de terminal com pergunta viva pela relavra posterior.** Para esse caminho, a primeira passada precisa absolver, E2 carimbar e a segunda voltar a acusar. Com insumos e janela iguais, isso não decorre apenas da ordem. A janela efetivamente pode mudar: a segunda passada recebe subconjunto e `_julgar_colab_corpo` deriva a janela dos extremos desse subconjunto (`app/ponto/services/cartorio.py:752`, `:835`, `:861`, `:880`), embora a documentação exija lista inteira justamente para evitar pareamento diferente (`:725`). Contraexemplo concreto [não verificado].

Mapa principal:

| Estado | Escritor | Leitores |
|---|---|---|
| `CelulaDia.veredito` | Porta `lavrar_veredito`, `app/ponto/portas/celula.py:647`, atribuição `:692`; cartório chama em `app/ponto/services/cartorio.py:1434` | C6: `app/chamados/juizes.py:1017`, `:1054`; E2: `app/chamados/reconciliador.py:594`; impressão/pulo: `app/ponto/services/cartorio.py:1098` |
| `materializada_em` / `via_resolucao` neste E2 | Porta existente `app/chamados/reconciliador.py:617`, `:619`, persistência `:620` | C6: `app/chamados/juizes.py:1044`, `:1050`, `:1076`; lavra S2 delega ao juiz em `app/chamados/veredito_lavrado.py:220` |

O novo teste de evento confere terminal e ausência de pergunta viva, mas não confere `rejulgar_descartado`, impressão estável nem segunda chamada sem escrita (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:385`). O selo existente mede queries e callbacks exatos; seus resultados nesta cópia são [não verificado], com limites em `app/ponto/tests/test_selo_chokepoint_escrita.py:157`, `:177`, `:212`.

## 30/10 22:36 · sessao 5581f0c9f9f7 · LAUDO-3010 · leitura · origem=Code

**O achado é uma diferença entre “pergunta viva”, “materialização comprovada” e “porta alcançável pelo admin”. As duas funções nomeadas respondem às duas primeiras coisas; nenhuma verifica integralmente a alcançabilidade exigida pela O261 (A).** (`app/folha/fora_da_folha.py:66`; `app/chamados/services/raiox.py:173`; `app/docs/BACKLOG.md:767`)

Vínculo da cópia com o hash informado/main: **[não verificado]**. Estado persistido da p#34375: **[não verificado]**.

**1. Predicados, insumos, consumidores e registro**

**`o_que_segura(colab, mes, ano)`** devolve os dias retentores com `fio`, `destrava` e chamados; a decisão sobre quem deve agir está em **`_fio_do_dia`**, nesta ordem exata:

| Condição | Resultado |
|---|---|
| Existe pergunta viva da célula com `estado == PENDENTE` | `colab` |
| Senão, existe alguma pergunta viva | `admin` |
| Senão, existe chamado da célula com status em `VIVOS` | `chamado` |
| Senão | `nada` |

Sem `celula_id`, `o_que_segura` força `nada`. Portanto, **`admin` exige alguma viva e nenhuma viva pendente**; `colab` significa aguardar o colaborador, não ação imediata de validação pelo admin. (`app/folha/fora_da_folha.py:66`; `app/folha/fora_da_folha.py:67`; `app/folha/fora_da_folha.py:69`; `app/folha/fora_da_folha.py:71`; `app/folha/fora_da_folha.py:204`; `app/folha/fora_da_folha.py:39`)

Os insumos são colaborador, empresa e competência; `classificar_export` informa entrada/motivo; `motivos_retencao_celula(com_dias=True)` fornece os dias; `_alimentar` carrega perguntas pela FK da célula e chamados em `VIVOS`. Não exige que a pergunta esteja ancorada num chamado vivo **da fila**. (`app/folha/fora_da_folha.py:188`; `app/folha/fora_da_folha.py:191`; `app/folha/fora_da_folha.py:93`; `app/folha/fora_da_folha.py:51`; `app/folha/fora_da_folha.py:54`)

A `.viva` delega a **`pergunta_viva`**. Sua precedência atual é:

- Validação ou tranca tornam a pergunta morta; resposta com texto, sem decisão administrativa do dia, mantém viva salvo concordância comprovada com a luz num dia que não acusa. (`app/chamados/models.py:2369`; `app/chamados/juizes.py:1006`; `app/chamados/juizes.py:1010`; `app/chamados/juizes.py:1020`; `app/chamados/juizes.py:1032`)
- Depois vêm encerramento por identidade, marco materializado aceso e veredito da célula: acusador mantém viva; absolvição sem marcos pendentes mata. (`app/chamados/juizes.py:1039`; `app/chamados/juizes.py:1050`; `app/chamados/juizes.py:1054`)
- Sem decisão nesses degraus, consulta a classificação de falta na janela viva; sem carimbo vive; com carimbo, devolve o inverso de `_pergunta_materializada_de_fato`; exceção nesse último degrau resulta em morta. (`app/chamados/juizes.py:1066`; `app/chamados/juizes.py:1076`; `app/chamados/juizes.py:1083`)

A `.estado` distingue pendente/respondida por **`respondida_em`**, sem exigir texto nesse rótulo. Assim, pergunta viva com timestamp de resposta e texto vazio pode produzir `fio=admin`; esse caso está documentado no laudo anterior. (`app/chamados/models.py:2207`; `app/docs/RELATO.md:363`)

**`pergunta_sem_acao_possivel(p, marcos=None)`** tem outro predicado, exatamente:

```text
materializada_em é None                  → False
com carimbo e veredito_lavrado preenchido → veredito_lavrado == VL_SIM
com carimbo e veredito vazio              → bool(_pergunta_materializada_de_fato(p))
exceção no bloco                         → False
```

O parâmetro `marcos` não é consumido pelo corpo; não há checagem de frescor nele. O retorno `False` significa que esse teste **não comprovou materialização sem ação**, não que exista porta administrativa. (`app/chamados/services/raiox.py:151`; `app/chamados/services/raiox.py:173`; `app/chamados/services/raiox.py:182`; `app/chamados/services/raiox.py:186`)

O fallback usa via/carimbo, data/janela, classificação de falta, tipo de resposta, slot esperado, batidas plantadas e não retratadas, destino encerrado em determinados ramos e completude do turno. Portanto, o destino pode entrar como testemunha indireta, mas não existe requisito geral “âncora viva em `qs_na_fila`”. (`app/chamados/juizes.py:603`; `app/chamados/juizes.py:628`; `app/chamados/juizes.py:674`; `app/chamados/juizes.py:699`; `app/chamados/juizes.py:730`)

**Chamadores de produção, distinguindo função pública e núcleo efetivamente usado:**

| Função | Chamadores diretos |
|---|---|
| `o_que_segura` | **0 chamadas estáticas localizadas**; preservada como ferramenta HAIKU de uso humano. (`app/docs/BACKLOG.md:1484`) |
| `_fio_do_dia` | **3 funções / 3 chamadas**: `fora_da_folha`, `o_que_segura`, `dias_sem_fio`. (`app/folha/fora_da_folha.py:154`; `app/folha/fora_da_folha.py:204`; `app/ponto/services/pautas_dos_discordantes.py:75`) |
| `pergunta_sem_acao_possivel` | **1 função / 1 chamada**: `montar_chamados_fio`, pelo alias `_sem_acao`. (`app/chamados/services/fio.py:275`; `app/chamados/services/fio.py:280`) |

O núcleo da folha chega à **tela do retrato** por `fora_da_folha → montar_linhas → lavrar_retrato_folha`; também seleciona dias para pautas dos discordantes. O vigia do fio usa outra redução: `fio_do_dia → fio_vivo → any(pergunta_viva(...))`. (`app/folha/retrato_folha.py:1157`; `app/folha/management/commands/lavrar_retrato_folha.py:65`; `app/folha/views_retrato.py:246`; `app/ponto/services/pautas_dos_discordantes.py:75`; `app/chamados/services/vigia_fio.py:164`; `app/chamados/juizes.py:1111`)

O raio-X compõe `tem_pergunta_viva = any(p.viva and not _sem_acao(p))`. Essa flag é consumida pelo **formulário genérico do colaborador**; o “Validar” do modal administrativo tem condições próprias. A montagem é chamada por atendimento do colab, fio administrativo, modal e render parcial. (`app/chamados/services/fio.py:279`; `app/templates/chamados/partials/_card_atendimento.html:203`; `app/templates/chamados/partials/_card_atendimento.html:205`; `app/templates/chamados/partials/modal_fio.html:210`; `app/templates/chamados/partials/modal_fio.html:272`; `app/chamados/views.py:569`; `app/chamados/views.py:677`; `app/chamados/views.py:889`; `app/chamados/services/fio.py:442`)

O irmão SQL **`creditos_perguntas`** também exclui carimbada com `VL_SIM`, mas **não chama** `pergunta_sem_acao_possivel`; alimenta a partição por verbos. Nela, `validar` e `decidir` são os verbos contabilizados como exigência do admin. (`app/chamados/services/raiox.py:133`; `app/chamados/services/verbos.py:31`; `app/chamados/services/verbos.py:19`; `app/chamados/services/verbos.py:24`)

Pela regra, há ainda um teste explícito de ação administrativa no dia: **`resolver_dia_oferecido`** exige último recurso, dia de trabalho, passado e status em `falta/irregular/aberto/sem_certidao`. O calendário grava esse resultado; template e vigia o leem. (`app/colaboradores/services/calendario.py:196`; `app/colaboradores/services/calendario.py:204`; `app/colaboradores/services/calendario.py:945`; `app/templates/colaboradores/partials/_calendario_grade.html:125`; `app/chamados/services/vigia_fio.py:294`)

**Registro:** as duas funções nomeadas não estão individualizadas nos catálogos consultados; estão registrados os componentes `VIVOS`/C1, `pergunta_viva`/C6, `fio_vivo`/C15 e `resolver_dia_oferecido`/TT11. O próprio registro alerta que **chamado vivo não responde se há cobrança/fio**; `JUIZES.md` manda prevalecer o registro vivo. (`app/core/juizes.py:100`; `app/core/juizes.py:233`; `app/core/juizes.py:238`; `app/core/juizes.py:247`; `app/core/juizes.py:684`; `app/docs/JUIZES.md:11`; `app/docs/JUIZES.md:14`)

**2. Onde discordam — e onde apenas deixam de verificar a porta**

Os estados pedidos não bastam para determinar a discordância: é necessário acrescentar **carimbo de materialização e veredito lavrado**. A folha consulta vitalidade; o raio-X consulta esses dois insumos. (`app/folha/fora_da_folha.py:66`; `app/chamados/services/raiox.py:173`)

Inferências abaixo consideram uma única pergunta relevante na célula:

| Combinação | Folha | Raio-X |
|---|---|---|
| Viva, respondida, não validada; materializada e `VL_SIM` | `admin` | **Sem ação = True**: discordância direta. (`app/folha/fora_da_folha.py:69`; `app/chamados/services/raiox.py:183`) |
| Viva, pendente; materializada e `VL_SIM` | `colab` | **Sem ação = True**: divergem sobre continuidade do fio, sem a folha prometer validação imediata. (`app/folha/fora_da_folha.py:67`; `app/chamados/services/raiox.py:183`) |
| Viva, respondida; sem materialização; âncora terminal ou viva fora da fila | `admin` | **Sem ação = False**; ambos deixam passar esse aspecto, embora falte a alcançabilidade da O261. (`app/folha/fora_da_folha.py:69`; `app/chamados/services/raiox.py:173`; `app/docs/BACKLOG.md:767`) |
| Morta/validada, sem materialização, sem outro chamado vivo | `nada` | **Sem ação = False**; inverter esse booleano fabricaria uma possibilidade que a vitalidade não reconhece. (`app/chamados/juizes.py:1006`; `app/folha/fora_da_folha.py:73`; `app/chamados/services/raiox.py:173`) |
| Nenhuma viva; existe chamado `registrado` ou silenciado | `chamado`, com instrução de resolver | A fila não o cobra; `registrado` é vivo, mas não pede ação. (`app/folha/fora_da_folha.py:71`; `app/folha/fora_da_folha.py:41`; `app/chamados/catalogo/motor.py:24`; `app/chamados/services/painel.py:186`) |

**Disputa fechada não elimina automaticamente a porta do admin nesta árvore.** A montagem administrativa inclui perguntas vivas de disputas fechadas; o POST de validação exige vitalidade nesse caso e segue sem reabrir a disputa. (`app/chamados/services/fio.py:213`; `app/chamados/services/fio.py:234`; `app/chamados/services/fio.py:594`; `app/chamados/services/fio.py:603`)

Também não cabe afirmar “âncora terminal = impossível agir em qualquer tela”: o modal administrativo carrega todos os chamados do colaborador, e o POST verifica permissão e correspondência da âncora, sem exigir participação na fila. **Estar acessível manualmente e cumprir o caminho exigido pela O261 são condições distintas.** (`app/chamados/services/fio.py:174`; `app/chamados/services/fio.py:117`; `app/chamados/services/fio.py:557`; `app/chamados/services/fio.py:592`; `app/docs/BACKLOG.md:767`)

Para **p#34375**, não localizei testemunho do estado na árvore **[não verificado]**. Condicionalmente: se viva/respondida e carimbada com `VL_SIM`, reproduz a primeira discordância; se viva sob terminal/fora da fila, reproduz a lacuna de alcançabilidade. O caso documental de **col292, 21/09**, nomeia outras perguntas, **p#34412/p#51322**; não há base para substituir esses identificadores por 34375. (`app/folha/fora_da_folha.py:69`; `app/chamados/services/raiox.py:183`; `app/docs/RELATO.md:359`)

**3. Proximidade da lei e alcance**

**Quanto à forma “consumidor lê o veredito gravado”, o raio-X é mais próximo:** normalmente lê `veredito_lavrado`; porém ainda chama o juiz quando o campo está vazio. A folha delega à vitalidade, cujo caminho pode descer a classificação de falta e julgamento de lastro. (`CLAUDE.md:276`; `app/chamados/services/raiox.py:182`; `app/chamados/services/raiox.py:184`; `app/chamados/juizes.py:1068`; `app/chamados/juizes.py:1083`)

**Quanto à regra completa da O261 — porta viva alcançável — nenhuma das duas basta.** A folha aceita viva sem conferir âncora/fila; o raio-X não verifica sequer vitalidade no próprio corpo. A exigência documental acrescenta chamado vivo em `qs_na_fila` e presença no app. Alcance direto: **`o_que_segura` 0; raio-X 1; núcleo `_fio_do_dia` 3**, conforme os sítios acima. (`app/folha/fora_da_folha.py:66`; `app/chamados/services/raiox.py:173`; `app/docs/BACKLOG.md:767`; `app/docs/BACKLOG.md:1484`; `app/chamados/services/fio.py:280`; `app/folha/fora_da_folha.py:154`; `app/folha/fora_da_folha.py:204`; `app/ponto/services/pautas_dos_discordantes.py:75`)

**4. “T3” da O261 (A), existência e tela**

A identificação literal **“T3 da O261 (A)” não foi localizada nos cinco documentos indicados [não verificado]**. Há três divisões documentais diferentes:

- **O261 original:** FASES 1–3; a terceira é invariante, vigia/contador, caso que morde e passivo. (`app/docs/BACKLOG.md:1259`; `app/docs/TICKETS.md:146`)
- **O261-COMPLEMENTO-[nome]:** itens **(A) alcançabilidade, (B) materializada sem resposta, (C) pai terminal, (D) vigia e link**. (`app/docs/BACKLOG.md:767`)
- **Proposta numerada (1)–(4):** o **(3)** pede que reabrir levante o véu; o documento não o chama de T3. (`app/docs/RELATO.md:370`)

**A alcançabilidade de (A) não está implementada no caminho atual do censo:** ele parte de chamados vivos com célula e aceita `fio_vivo` sobre as perguntas da célula, sem exigir a combinação âncora/fila/app. O contador existente aparece no **placar ESTADO, “Fio morto (O261)”**. (`app/chamados/services/vigia_fio.py:186`; `app/chamados/services/vigia_fio.py:270`; `app/chamados/services/vigia_fio.py:286`; `bin/gerar_estado.py:313`; `bin/fio_estado.py:101`)

**A FASE 3 original tem implementação**, com comando e elo do cron; isso não comprova o complemento posterior de alcançabilidade. (`app/chamados/management/commands/vigiar_fio_morto.py:160`; `app/chamados/management/commands/vigiar_fio_morto.py:195`; `app/config/crons.py:1654`; `app/docs/BACKLOG.md:767`)

**O (3) numerado não aparece no corpo atual de `reabrir`:** após as guardas, ele grava o estado e ressuscita perguntas, sem chamada para levantar o véu. (`app/chamados/models.py:1182`; `app/chamados/models.py:1194`)

Na tela administrativa, existe **“O que fazer / O que destrava” no painel do colaborador**, com texto de `DESTRAVA` e botões genéricos. O Raio-X do retrato abre esse painel; as datas são `<span>`, sem link por dia/pergunta. Portanto, o link específico exigido no complemento (D) não é o que esses templates entregam. (`app/templates/colaboradores/partials/painel_colaborador.html:26`; `app/folha/retrato_folha.py:1068`; `app/templates/folha/_retrato_o_que_fazer.html:46`; `app/templates/folha/_retrato_o_que_fazer.html:61`; `app/templates/folha/_retrato_raiox.html:68`; `app/docs/BACKLOG.md:767`)

## 30/10 22:36 · sessao f69ca6868bc3 · - · - · origem=Ronald (tmux)

- **LIMBO: ordem solicitada presente.** `agrupar_furo` chama `_vivas_ancoradas('resolvido')`, reaponta pela porta existente e só depois pede `reconciliar()`; a porta consulta E4-c e a guarda de costura, atribui `chamado_destino`, relavra S2 e salva. O teste exige pergunta viva ancorada no pai vivo e segunda execução sem novo agrupamento. `app/chamados/services/limbo_folgas.py:228-230`; `app/chamados/services/fio.py:821-831`; `app/chamados/tests/test_t34_furo_sob_limbo.py:43-61`.

- **Quem escreve/lê `chamado_destino`:** a porta `reapontar_resolvedora` escreve em `fio.py:828-831`; `PerguntaDisputa.abrir` também escreve no dedup e no nascimento em `models.py:2370-2397`. As atribuições de `models.py:342` e `fio.py:758-762` servem à simulação em memória. A autoridade da âncora retorna destino ou container em `fio.py:97-117`; a montagem do fio carrega os destinos em `fio.py:199-228`. Todos os caminhos citados estão em `app/chamados/`.

- **O199: reaproveita o fechador existente.** No ramo “dia já folga”, carimba a validação, fecha a pergunta e chama `fechar_o_fio_do_dia`; esse fechador percorre as perguntas e pede reconciliação dos chamados, anotando apenas quando fecham. A validação mata a pergunta no primeiro degrau de C6; irmã respondida ainda não validada pode continuar viva antes da leitura do carimbo. `app/chamados/services/validacao.py:261-271`; `app/ponto/services/veredito_celula.py:174-187`; `app/chamados/juizes.py:1006-1007`; `app/chamados/juizes.py:1020-1032`. O teste das quatro irmãs cobra uma folga, chamado resolvido e uma nota. `app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:167-192`.

- **Vermelho reportado e mecanismo confirmado por leitura:** `PortaSegundoAtoTest.test_MORDE_derruba_a_cobranca_viva_do_dia_que_virou_folga` exige que o chamado saia de VIVOS. `app/colaboradores/tests/test_contract_porta_2ato.py:147-160`. O protesto `COBRANCA_EM_DIA_SEM_TRABALHO` depende do chamado vivo; vira `cobranca_indevida`, que continua em `VEREDITOS_ACUSAM`; C6 mantém a pergunta viva e E2 exclui essa célula. O cartório só pede rejulgamento desse ramo quando alguma reconciliação fecha. `app/ponto/supra_juiz.py:441-445`; `app/escala/models.py:1581`; `app/escala/models.py:1735`; `app/chamados/juizes.py:1054-1057`; `app/chamados/reconciliador.py:598`; `app/ponto/services/cartorio.py:1213-1225`. Execução local do teste: **[não verificado]**.

- **Não repetir como vermelhos comprovados os testes HX/B2.d.** A cópia já planta explicitamente o passivo terminal por `update`, em vez de depender de `encerrar()` produzir esse estado. `app/chamados/tests/test_laco_hx_x_bug31.py:37-43`; `app/chamados/tests/test_b2d_resposta_visivel.py:16-22`. Resultado atual dessas suítes, das órfãs e dos tetos de queries: **[não verificado]**.

- **Limite do agrupamento, preexistente no trecho fornecido:** o contexto é salvo antes da reconciliação; se esta não fechar, nova chamada com o mesmo `agrupado_em` retorna antes de tentar novamente. `app/chamados/services/limbo_folgas.py:217-231`. Não encontrei cenário normal novo de recusa da reata com os pais vivos selecionados pelos chamadores; ocorrência em produção: **[não verificado]**. Os chamadores selecionam pai vivo em `app/chamados/services/limbo_folgas.py:243-255` e `app/ponto/management/commands/detectar_ausencias.py:755-756`.

## 30/10 22:34 · sessao f86cff5c627c · - · - · origem=Ronald (tmux)

Não encontrei **“T3 da O261 (A)”** nos cinco documentos indicados; a correspondência exata fica **[não verificado]**. Há divisões diferentes que não devem ser confundidas:

- **O261 original:** usa **FASE 1, FASE 2, FASE 3**. A FASE 3 pede invariante, caso que morde, allowlist zero, hoje fora, contador no placar e passivo pela porta. Não contém divisão (A)..(D). — `app/docs/BACKLOG.md:1259`; `app/docs/TICKETS.md:146`.
- **O261-COMPLEMENTO-[nome]:** contém **(A)..(D)**. (A) é **alcancabilidade real**: pergunta viva sob chamado vivo em `qs_na_fila`, aparecendo no app; pergunta viva sob chamado terminal conta como fio morto. (B) materializada sem resposta; (C) nascimento com pai terminal; (D) vigia e link do retrato levando à pergunta pelo chamado vivo. — `app/docs/BACKLOG.md:767`; `app/docs/RELATO.md:355`; `app/docs/RELATO.md:359`.
- **Proposta de quatro curas:** (1) vigia pelo universo `o_que_segura`; (2) resposta vazia; **(3) reabertura levanta o véu**; (4) link por dia. Chamar o terceiro de “T3” seria inferência, pois o documento o chama somente “(3)”. — `app/docs/RELATO.md:370`.

**Existência atual:**

- A **FASE 3 original tem implementação**: `censo` lê chamados vivos e células que acusam; elimina dias com fio vivo, hoje e decisão do dia pendente. O comando executa, imprime e carimba; o cron o agenda. — `app/chamados/services/vigia_fio.py:168`, `app/chamados/services/vigia_fio.py:186`, `app/chamados/services/vigia_fio.py:229`, `app/chamados/services/vigia_fio.py:286`, `app/chamados/services/vigia_fio.py:294`, `app/chamados/management/commands/vigiar_fio_morto.py:160`, `app/chamados/management/commands/vigiar_fio_morto.py:195`, `app/config/crons.py:1654`.
- Seu lugar visível é o **placar ESTADO**, seção **“Fio morto (O261)”**. — `bin/gerar_estado.py:313`; `bin/fio_estado.py:101`.
- O **(A) de alcancabilidade não aparece aplicado nesse censo**: ele agrupa perguntas pela FK da célula e aceita `fio_do_dia(ps)`; esse trecho não verifica `qs_na_fila`, âncora viva nem presença no app. — `app/chamados/services/vigia_fio.py:270`, `app/chamados/services/vigia_fio.py:286`. O próprio relatório registra perguntas vivas visíveis no app, sob chamado terminal ausente da fila admin. — `app/docs/RELATO.md:359`.
- O link atual do **Raio-X do retrato** abre o **painel do colaborador**; as datas do bloco “O que fazer” são apenas `<span>`, sem link por dia/pergunta. — `app/templates/folha/_retrato_raiox.html:68`; `app/templates/folha/_retrato_o_que_fazer.html:43`.
- O bloco **“O que fazer / O que destrava”** existe no drawer/painel do colaborador, lê a frase genérica `DESTRAVA`, e não interpola um chamado específico nessa frase. — `app/folha/retrato_folha.py:1049`, `app/folha/retrato_folha.py:1057`, `app/folha/retrato_folha.py:1068`; `app/templates/colaboradores/partials/painel_colaborador.html:26`; `app/templates/folha/_retrato_o_que_fazer.html:28`, `app/templates/folha/_retrato_o_que_fazer.html:33`.

## 30/10 22:33 · sessao fc7f73f9089d · - · - · origem=Ronald (tmux)

Achados de `raiox.pergunta_sem_acao_possivel`, somente leitura:

- **Predicado atual:** se `materializada_em is None`, devolve `False`; com carimbo, se `veredito_lavrado` estiver preenchido, devolve exclusivamente `veredito_lavrado == VL_SIM`; sem veredito, executa `_pergunta_materializada_de_fato(p)`; qualquer exceção devolve `False`. `marcos` é recebido, mas não usado. Não verifica staleness, vitalidade, `validada_em`, disputa aberta, status da âncora ou fila. `app/chamados/services/raiox.py:151`, `app/chamados/services/raiox.py:173`, `app/chamados/services/raiox.py:182`, `app/chamados/services/raiox.py:186`.
- **Insumos do fallback:** via de resolução/carimbo, data e janela viva/classificação de falta; tipo escolha; para hora, slot esperado e batidas plantadas/vivas, destino encerrado e retratação auditada quando não há slot; para texto, turno completo ou retratação auditada. `app/chamados/juizes.py:603`, `app/chamados/juizes.py:628`, `app/chamados/juizes.py:674`, `app/chamados/juizes.py:699`, `app/chamados/juizes.py:730`, `app/chamados/juizes.py:734`.
- **Um chamador direto de produção / um call site:** `montar_chamados_fio`, alias `_sem_acao`, compõe `ch.tem_pergunta_viva = any(p.viva and not _sem_acao(p, _marcos_s2))`. `app/chamados/services/fio.py:275`, `app/chamados/services/fio.py:279`.
- **Quatro chamadores de `montar_chamados_fio`:** `meu_atendimento`, `fio_colaborador`, closure `_resp_modal_full` do modal, `contexto_parcial_fio`. `app/chamados/views.py:569`, `app/chamados/views.py:677`, `app/chamados/views.py:889`, `app/chamados/services/fio.py:442`.
- **O uso efetivo da flag é COLAB:** único consumo de template encontrado é o formulário genérico de hora do `_card_atendimento`, condicionado a chamado aberto/em análise, furo e ausência de `tem_pergunta_viva`. O questionário usa `p.viva` diretamente. `app/templates/chamados/partials/_card_atendimento.html:203`, `app/templates/chamados/partials/_card_atendimento.html:205`, `app/templates/chamados/partials/_card_atendimento.html:77`.
- **ADMIN não lê essa flag para o Validar:** modal exige disputa + permissão; com resposta, `pendencia_admin` ou `validada_em` mostram estado e retiram o Validar; caso contrário mostra Validar. Disputa fechada só retira Rejeitar/Corrigir. `app/templates/chamados/partials/modal_fio.html:120`, `app/templates/chamados/partials/modal_fio.html:210`, `app/templates/chamados/partials/modal_fio.html:215`, `app/templates/chamados/partials/modal_fio.html:221`, `app/templates/chamados/partials/modal_fio.html:242`, `app/templates/chamados/partials/modal_fio.html:272`, `app/templates/chamados/partials/modal_fio.html:282`.
- **Âncora terminal/fora da fila ainda pode ter porta manual no modal:** listagem administrativa traz todos os chamados do colaborador; a âncora é destino ou pai sem ler status. Backend exige RBAC, correspondência da âncora e, em disputa fechada, pergunta viva; não exige chamado na fila. Portanto não afirmar “terminal = sem porta em toda tela”. `app/chamados/services/fio.py:174`, `app/chamados/services/fio.py:117`, `app/chamados/services/fio.py:557`, `app/chamados/services/fio.py:560`, `app/chamados/services/fio.py:592`, `app/chamados/services/fio.py:594`, `app/chamados/services/fio.py:599`.
- **Validar já validada não age:** porta responde `ja_validada`; sem resposta, `sem_resposta`. `app/chamados/services/validacao.py:356`, `app/chamados/services/validacao.py:359`.
- **Fila não equivale a vivo:** `qs_na_fila` exige `COBRANCA` e exclui silêncio futuro; `registrado` é vivo, mas não cobrança; terminais são resolvido, fechado, cancelado, superado. `app/chamados/services/painel.py:186`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`, `app/chamados/catalogo/motor.py:31`.
- **Não confundir o irmão SQL:** `creditos_perguntas` exclui carimbada + `VL_SIM`, sem chamar `pergunta_sem_acao_possivel`; tem dois chamadores diretos, `fio` e `particionar_verbos`. `app/chamados/services/raiox.py:133`, `app/chamados/services/fio.py:386`, `app/chamados/services/verbos.py:31`.
- **Registro:** buscas não encontraram os nomes `pergunta_sem_acao_possivel`, `o_que_segura` ou `_pergunta_materializada_de_fato` no registro/mapa. O registro próximo é C6 → `pergunta_viva`, C4 → permissão contextual, C15 → `fio_vivo`. `app/core/juizes.py:236`, `app/core/juizes.py:238`, `app/core/juizes.py:247`; mapa aponta FIO/DISPUTA para `_disputa_do_chamado`, e permissão contextual para `pode_agir_chamado`. `app/docs/JUIZES.md:40`, `app/docs/JUIZES.md:46`. Ausência é resultado de busca [não verificado por referência positiva].
- **p#34375:** nenhuma ocorrência localizada na árvore; estado persistido do caso [não verificado].

## 30/10 22:33 · sessao 803483fec8e6 · - · - · origem=Ronald (tmux)

`o_que_segura` não devolve booleano “há ação admin”; devolve classificação `fio` e instrução `destrava` para cada dia retentor. O predicado está em `_fio_do_dia`, e a ordem exata é: se alguma pergunta **viva** da célula está `PENDENTE`, `colab`; senão, se existe alguma viva, `admin`; senão, se existe chamado com `status_local∈VIVOS`, `chamado`; senão, `nada`. Sem `celula_id`, o chamador força `nada`. Referências: `app/folha/fora_da_folha.py:59`, `:66`, `:67`, `:69`, `:71`, `:73`, `:204`, `:209`.

Os textos são: `admin` → “validar (ou recusar) a resposta ... na mesa de disputa”; `colab` → esperar resposta e depois validar; `chamado` → “resolver o chamado N”; `nada` → “SEM FIO ... este dia precisa de alguém para olhar”. Referências: `app/folha/fora_da_folha.py:38`–`:42`; para IDs reais do chamado e texto substituído, `:198`–`:210`.

Insumos e fontes:

- Recebe colaborador/mês/ano, obtém empresa e janela, lê `classificar_export` para informar entra/motivo e `_dias_que_retem` para universo dos dias. Referências: `app/folha/fora_da_folha.py:179`, `:188`–`:194`.
- `_dias_que_retem` delega a `motivos_retencao_celula(..., com_dias=True)`. Referência: `app/folha/fora_da_folha.py:90`–`:94`.
- Alimentação carrega **todas** as perguntas por célula e chamados com status `VIVOS`, sem filtro de disputa aberta/fechada, âncora viva/terminal, fila, destino ou validação na query. Referência: `app/folha/fora_da_folha.py:46`–`:56`.
- A pergunta é filtrada por `.viva`, que delega integralmente a `pergunta_viva`; `.estado` traduz validação/materialização/vitalidade/resposta em rótulo. Referências: `app/chamados/models.py:2358`–`:2369`; `:2197`–`:2211`.
- `validada_em != None` torna a pergunta morta no juiz; portanto pergunta validada não pode produzir `admin`/`colab` nesse caminho. Referência: `app/chamados/juizes.py:1005`–`:1007`.
- O retentor lê célula soberana: isento retorna vazio; ajusta admissão/demissão; sem célula em dia passado com vínculo retém; veredito acusador retém salvo todos protestos não cobráveis ou isenção de pausa; sem veredito, trabalha definido e dia passado retém. Referências: `app/folha/export.py:1174`, `:1180`–`:1196`, `:1206`–`:1238`.

**Contagem de chamadores estáticos de produção:** `o_que_segura`: **0**; seu juiz efetivo `_fio_do_dia`: **3 funções / 3 chamadas** — `fora_da_folha`, `o_que_segura`, `dias_sem_fio`. Referências dos três: `app/folha/fora_da_folha.py:154`, `:204`; `app/ponto/services/pautas_dos_discordantes.py:75`. A ausência de chamadores de `o_que_segura` resulta da busca global, não tem linha de prova positiva; marcar `[não verificado]` se exigir referência documental estrita.

Cadeias de consumo:

1. `_fio_do_dia` → `fora_da_folha` (`app/folha/fora_da_folha.py:154`) → `montar_linhas` (`app/folha/retrato_folha.py:1157`) → comando `lavrar_retrato_folha` (`app/folha/management/commands/lavrar_retrato_folha.py:65`, `:74`) → tela que lê retratos gravados (`app/folha/views_retrato.py:246`–`:260`), rota `exportar/retrato/` (`app/folha/urls.py:23`). Drawer lê `DESTRAVA` literalmente via `o_que_fazer`, `app/folha/retrato_folha.py:1057`, `:1068`.
2. `_fio_do_dia` → `dias_sem_fio`, que escolhe células `discordante` e só aceita fio `nada` (`app/ponto/services/pautas_dos_discordantes.py:67`–`:78`) → `Command.apurar` (`app/ponto/management/commands/pautas_discordantes.py:56`, `:68`).
3. `vigia_fio` **não chama** `_fio_do_dia`: seu `fio_do_dia` delega a `fio_vivo` (`app/chamados/services/vigia_fio.py:155`–`:165`); essa redução é `any(pergunta_viva(p) ...)` (`app/chamados/juizes.py:1102`–`:1111`). A documentação reconhece a mesma autoridade `.viva` do outro leitor (`app/chamados/services/vigia_fio.py:129`–`:137`).

**Discrepância estrutural importante para a matriz:** `VIVOS=('aberto','em_analise','registrado')`, mas o catálogo declara que **`registrado` não pede ação**, não entra na fila/placar; `COBRANCA` contém só aberto/em_análise. Referência: `app/chamados/catalogo/motor.py:24`–`:29`. Assim, sem pergunta viva e com único chamado `registrado`, `_fio_do_dia` classifica `chamado` e oferece “resolver chamado”, embora o catálogo diga ausência de ação devida. Referências: `app/folha/fora_da_folha.py:71`–`:72`, `:41`.

A UI não pode ser confundida com porta viva: junta tanto `chamado` como `ninguem` no dono `admin`; isso é agrupamento explícito, não prova de uma ação endereçável. Referência: `app/folha/retrato_folha.py:189`–`:192`. A regra `dono_da_linha` ainda desvia `ninguem` para `hoje` quando todas as datas são hoje/futuras (`:474`–`:481`).

Nenhum dos nomes `o_que_segura`, `_fio_do_dia` ou `pergunta_sem_acao_possivel` está registrado diretamente nos dois catálogos pesquisados `[não verificado]`. O registro contém suas autoridades componentes: `_C1→VIVOS`, `_C6→pergunta_viva`, `_C15→fio_vivo`. Referências: `app/core/juizes.py:233`, `:238`, `:247`. O texto do registro distingue expressamente **chamado vivo** de **fio vivo**, para evitar autorizar `VIVOS` como resposta a “alguém cobra?”. Referência: `app/core/juizes.py:100`–`:106`. `app/docs/JUIZES.md:51`–`:54` ainda traz o lookup antigo “vivo/cobra = VIVOS/COBRANCA”; o próprio documento manda o registro vivo prevalecer (`:11`–`:15`).

Outra função realmente relevante pela **regra**, independente do nome: `resolver_dia_oferecido(dia, hoje)` exige `pode_ultimo_recurso && eh_trabalho && data<hoje && status∈('falta','irregular','aberto','sem_certidao')`; permissão de quem vê fica no template. Referências: `app/colaboradores/services/calendario.py:196`, `:199`–`:205`. `resolver_dia_oferecido_na_data` monta o calendário pelo mesmo caminho da tela e lê esse campo (`:208`–`:216`). Está registrada `_TT11` em `app/core/juizes.py:684`.

O vigia distingue esse clique real de mero status: `resolver_dia_oferecido_na_data` → `decisao_do_dia_pendente`, com comentário explícito de que `em_analise` sozinho não é ação nomeada (`app/chamados/services/vigia_fio.py:294`–`:300`); separa ausência aberta (`:301`–`:306`), FOLGA_CONTESTA validada (`:307`–`:313`) e fio do admin por presença **e dono BATIDA/CADASTRO** na lista (`:314`–`:318`, `:266`).

Lei exata: “CONSUMIDOR (chamado, pergunta, tela, folha) le a lampada, nunca re-julga”; cron vigia só alarma, nunca juiz. Referências: `CLAUDE.md:275`–`:276`.

## 30/10 22:28 · sessao 6542be814925 · O261-V-FASE · verificador · origem=Code

**Achei uma lacuna procedente de cobertura: o teste do `hoje` não morde `lista()`.** O repasse está correto na cópia atual. (`app/escala/tests/test_o261_v_fase_na_lista.py:344–349`; `app/escala/services/cadastro_realidade.py:611`)

A cópia está sem `.git`; a comparação efetiva `33bb7451..0b1d91f2` e a atribuição exata de cada mudança ao delta ficam **[não verificado]**.

**1. Quem escreve e quem lê**

- O censo fornece os dias recém-julgados em `dias_de_fase`, pergunta o dono e grava `dono_do_dia` nos itens. O escritor persistente é `fase_12x36.lavrar`. (`app/ponto/management/commands/censo_fase_12x36.py:260–276`; `app/escala/services/fase_12x36.py:84–92`)
- `InsumosDoDono` recebe esses dias ou lê `ultimo_lavrado(self.hoje)`, filtrando pelo colaborador. `dono()` transforma a presença do dia em `fase_deslocada=True`; o juiz lê esse argumento depois dos ramos de BATIDA. (`app/ponto/management/commands/e6_oraculo.py:135`; `:159–176`; `:103–108`)
- No pré-filtro das pautas, `_fase = _ins.fase()` alimenta `fase_deslocada`; somente a resposta `ESTRUTURA` provoca `_ins.cxr()`. O julgamento final passa por `_ins.dono()`. (`app/ponto/management/commands/pautas_discordantes.py:96–109`)
- `DONO_PELO_JUIZ = {'C9', 'C10'}` é lido por `_dono_resolvido`, chamado por `dias_na_lista`; donos divergentes nesse nível devolvem `None`. (`app/escala/services/cadastro_realidade.py:710–721`; `:744`)
- `lista()` passa seu `hoje` a `_fase_deslocada`, que o repassa a `ultimo_lavrado`; o leitor aplica `data_ref__lte=ate`. (`app/escala/services/cadastro_realidade.py:611`; `:340`; `app/escala/services/fase_12x36.py:103–105`)
- O censo captura `_agora` uma vez, deriva `_hoje` dele e repassa o mesmo instante ao carregador do dono. (`app/ponto/management/commands/censo_fase_12x36.py:62–64`; `:260`)

**2. Falhas e limites encontrados**

- **C9/C10 divergentes por horário: proteção efetiva.** O contraexemplo C9=`ESTRUTURA`, C10=`CADASTRO` resolve para `None`; o vigia só retira do alarme donos `BATIDA`/`CADASTRO`. Portanto, esse conflito permanece em `fio_morto`. (`app/escala/tests/test_o261_v_fase_na_lista.py:326–335`; `app/chamados/services/vigia_fio.py:266`; `:314–324`)
- **Resíduo no pré-filtro:** dia par, sem fase e sem vínculo retorna provisoriamente `ESTRUTURA`, porque o pré-filtro não passa `sem_vinculo_no_dia`. Pede motor; se falhar, descarta o colaborador, embora o julgamento completo desse dia fosse `CADASTRO`. O caminho existe; ser introduzido pelo delta é **[não verificado]**. (`app/ponto/management/commands/pautas_discordantes.py:97–106`; `app/ponto/management/commands/e6_oraculo.py:97–98`; `:169–171`)
- **Resíduo temporal em C9:** uma consulta com `hoje=29/10` pode combinar C10 limitada a 29/10 com C9 lavrada em 30/10, pois C9 continua lendo a última lavra sem corte. Ocorrência em produção e pertencimento ao delta: **[não verificado]**. (`app/escala/services/cadastro_realidade.py:319`; `:340`; `app/ponto/services/teto_na_lista.py:120–124`)
- **Custo:** a fase é carregada por instância de `InsumosDoDono`, consultando a lavra e percorrendo seus itens; C9 instancia por colaborador/competência. O censo evita essa consulta ao injetar os dias. Impacto real de custo: **[não verificado]**. (`app/ponto/management/commands/e6_oraculo.py:159–164`; `app/ponto/services/teto_na_lista.py:67–72`; `app/ponto/management/commands/censo_fase_12x36.py:260–261`)
- Não encontrei outro caminho de julgamento ignorando a fase. O comando E6 ainda pede espelho antecipadamente, mas o utiliza para comparar os minutos da tela; isso não demonstra pedido indevido do motor. (`app/ponto/management/commands/e6_oraculo.py:297–309`; `:323`; `:359`)

**3. Os testes mordem o chamador?** [nao verificado]

- **Pautas: sim.** Executa `Command.apurar`, faz `cxr()` explodir e exige a permanência do colaborador como `CADASTRO`. (`app/escala/tests/test_o261_v_fase_na_lista.py:385–396`)
- **Conflito C9/C10: sim, em `dias_na_lista`.** Testa ambas as ordens e donos concordantes. (`app/escala/tests/test_o261_v_fase_na_lista.py:326–335`)
- **Relógio: sim, por AST.** O selo inclui `InsumosDoDono` entre os alvos e examina funções chamadoras para detectar leituras adicionais. (`app/escala/tests/test_selo_grade_pede_o_instante.py:27`; `:93–120`; `:154–170`)
- **C10/hoje: não no chamador `lista()`.** O teste chama diretamente o auxiliar. Confirmei em execução isolada, em memória: retirar apenas `hoje` da chamada de `lista()` mantém esse teste aprovado e permite trazer a lavra posterior no contraexemplo acima. Os testes integrados também congelam o relógio na mesma data de `HOJE`, sem distinguir os dois caminhos. (`app/escala/services/cadastro_realidade.py:611`; `app/escala/tests/test_o261_v_fase_na_lista.py:344–349`; `:37`; `:84–86`; `:177`)

Resultado da suíte Django e impacto em produção: **[não verificado]**. [nao verificado]

## 30/10 22:27 · sessao 7befcc06756a · - · - · origem=Ronald (tmux)

- **C9/C10 divergentes:** a C9 pergunta ao juiz com a lavra disponível (`app/ponto/services/teto_na_lista.py:72`, `:86`); a C10 passa os dias julgados na própria corrida (`app/ponto/management/commands/censo_fase_12x36.py:260`). Lavras em horários diferentes podem divergir; isso tem tratamento explícito: ambos pertencem a `DONO_PELO_JUIZ`, e conflito retorna `None` (`app/escala/services/cadastro_realidade.py:707`, `:718`). O vigia mantém o dia alarmado porque só aceita `BATIDA`/`CADASTRO` (`app/chamados/services/vigia_fio.py:266`). Teste cobre ambas as ordens (`app/escala/tests/test_o261_v_fase_na_lista.py:326`).
- **Relógio do censo:** `handle` captura `_agora` uma vez, deriva `_hoje` dele e passa esse instante a `InsumosDoDono` (`app/ponto/management/commands/censo_fase_12x36.py:62`, `:63`, `:260`). Não encontrei segunda leitura do relógio nesse `handle`; `_curar` possui relógio próprio, fora da corrida normal `--lavrar` (`:341`).
- **Custo estático:** cada instância de `InsumosDoDono` sem override faz uma consulta da lavra e percorre todos os seus itens para selecionar um colaborador (`app/ponto/management/commands/e6_oraculo.py:159`). A C9 cria instâncias por colaborador/competência (`app/ponto/services/teto_na_lista.py:67`). Há repetição proporcional ao número de instâncias × tamanho da lavra; impacto e duração em produção [não verificado]. O censo fornece override e evita essa consulta (`app/ponto/management/commands/censo_fase_12x36.py:260`).
- **Hoje:** `lista` passa `hoje` ao helper C10, e este o repassa a `ultimo_lavrado` (`app/escala/services/cadastro_realidade.py:611`, `:340`). O teste novo chama somente o helper; não prova esse repasse por `lista` (`app/escala/tests/test_o261_v_fase_na_lista.py:344`).
- **Porta C10:** continua baseada em `aplicavel`, independentemente do novo dono (`app/escala/services/cadastro_realidade.py:490`, `:651`). Não classificaria como defeito: a lei registrada exige precisamente a proposta aceita no fio (`app/escala/tests/test_o261_v_fase_na_lista.py:6`).
- **Relógio adjacente:** `lavrar_teto_do_dia.handle` lê `localdate`, e `medir` captura outro instante (`app/ponto/management/commands/lavrar_teto_do_dia.py:27`; `app/ponto/services/teto_na_lista.py:49`). Inclusão disso no delta [não verificado].

Não identifiquei falha adicional procedente no recorte C9/C10; comparação efetiva com `33bb7451` [não verificado].

## 30/10 22:26 · sessao c8f5af10b82b · - · - · origem=Ronald (tmux)

Cobertura parcial do chamador:

- **C10/hoje: selo insuficiente.** O teste chama `_fase_deslocada` diretamente e só verifica o repasse a `ultimo_lavrado`; não exerce `lista()` (`app/escala/tests/test_o261_v_fase_na_lista.py:344-349`). Contra-exemplo de mutação: retirar `hoje` da chamada em `app/escala/services/cadastro_realidade.py:611` preserva esse teste. Os testes integrados também usam `lista(hoje=HOJE)` com relógio congelado no mesmo dia, portanto não distinguem o default incorreto (`app/escala/tests/test_o261_v_fase_na_lista.py:37`, `84-86`, `177`). Execução dessa mutação: [não verificado].
- **Pautas: morde o chamador.** Executa `Command().apurar`, injeta um dia lavrado, faz `InsumosDoDono.cxr` explodir e exige que o colaborador permaneça com dono `CADASTRO` (`app/escala/tests/test_o261_v_fase_na_lista.py:385-396`). Remover a fase do pré-filtro faz o ramo pedir o motor e descartar o colaborador (`app/ponto/management/commands/pautas_discordantes.py:96-106`).
- **C9/C10 divergentes: morde `dias_na_lista`.** Exercita ambas as ordens com `ESTRUTURA`/`CADASTRO` e exige `None`, além do controle positivo com donos iguais (`app/escala/tests/test_o261_v_fase_na_lista.py:326-335`).
- **Instante único: selo tem mordida no censo.** `InsumosDoDono` integra os alvos; o selo conta leituras de relógio em funções chamadoras, incluindo funções aninhadas (`app/escala/tests/test_selo_grade_pede_o_instante.py:27`, `93-120`, `154-170`). Executei apenas o analisador AST em memória: original sem achados; `_tz.localdate()` em lugar da derivação de `_agora` acusa linhas 62/63; `agora=_tz.now()` no repasse acusa linhas 62/260 (`app/ponto/management/commands/censo_fase_12x36.py:62-63`, `260`). Nenhum arquivo foi gravado.

A suíte Django e a identificação exata dos testes introduzidos pelo delta permanecem [não verificado]; a cópia não contém `.git`. [nao verificado]

## 30/10 22:26 · sessao 74f5cacdedef · - · - · origem=Ronald (tmux)

Não achei leitor de produção que passe pelo juiz e ignore a fase: os quatro caminhos usam `InsumosDoDono`, cujo `dono()` transmite `fase_deslocada` ao juiz. (`app/ponto/management/commands/e6_oraculo.py:159-176`; `app/ponto/management/commands/pautas_discordantes.py:82,109`; `app/ponto/services/teto_na_lista.py:72,86`; `app/ponto/management/commands/censo_fase_12x36.py:260-262`.)

O comando E6 continua pedindo o motor antecipadamente, mas precisa do espelho para a própria comparação; isso sozinho não demonstra defeito. (`app/ponto/management/commands/e6_oraculo.py:297-309,323`.)

O pré-filtro das pautas transmite a fase, porém ainda omite os demais insumos do dono: um dia par sem vínculo, sem fase, retorna provisoriamente ESTRUTURA e pede motor; se esse motor falhar, descarta o colaborador antes de descobrir CADASTRO. O mesmo vale para turno aberto com contagem par. A omissão parece herdada, mas pertencimento ao delta é [não verificado]. (`app/ponto/management/commands/pautas_discordantes.py:97-106`; `app/ponto/management/commands/e6_oraculo.py:97-109,169-171`.)

A divergência entre C9 e C10 por horários distintos resolve para `None`, e o consumidor mantém o dia no alarme: apenas BATIDA/CADASTRO entram em `fio_do_admin`; os demais seguem para `fio_morto`. (`app/escala/services/cadastro_realidade.py:707-721`; `app/chamados/services/vigia_fio.py:266,314-324`.)

Há uma limitação temporal residual em C9: `_teto_do_dia()` lê a última lavra sem limitar pelo `hoje`, enquanto C10 agora limita. Contra-exemplo: consulta histórica de 29/10 pode combinar C9 de 30/10 com C10 até 29/10. A existência do caminho está comprovada; atribuição ao delta e ocorrência em produção são [não verificado]. (`app/escala/services/cadastro_realidade.py:315-322,335-340`; `app/ponto/services/teto_na_lista.py:120-124`.)

O custo estático inclui uma leitura da lavra por instância de `InsumosDoDono`; C9 instancia por competência e colaborador. O censo injeta seus próprios dias e evita essa leitura. Custo real é [não verificado]. (`app/ponto/management/commands/e6_oraculo.py:132-135,159-164`; `app/ponto/services/teto_na_lista.py:67-72`; `app/ponto/management/commands/censo_fase_12x36.py:260-261`.)

## 30/10 22:20 · sessao 7af13b19947f · E1-BATERIA · red · origem=Code

Criado somente [app/ponto/tests/test_e1_lote3.py](/home/ronald/codex_ws/red_21b9db3852a8_20261030_221425_3149041/app/ponto/tests/test_e1_lote3.py), com 20 casos. Sintaxe e estrutura verificadas sem gerar arquivos auxiliares. Suite Django não executada; RED ainda não medido. [nao verificado]

Valores abaixo em minutos, salvo DSR.

| ID | Esperado | Regra | Origem |
|---|---|---|---|
| L01 | Trabalhado 480; atraso/antecipada 0; dia na lista | L-084 | S14 corrigido |
| L02 | Skip: status não definido | L-084; ADR | S14 corrigido |
| L03 | `ok` nos sete leitores | CLT 58/74; ADR | S01 ampliado |
| L04 | `folga` | CLT 67; L-024 | S05 ampliado |
| L05 | `ausencia` | Lei 605/49 art.6; L-089 | S07 ampliado |
| L06 | `falta` decidida | L-039; Lei 605/49 art.6 | S08 ampliado |
| L07 | Início noturno: `ok` | CLT 59-A; L-085; D3 | S15 ampliado |
| L08 | Cauda no dia seguinte: `folga` | L-085; L-024; D3 | S16 ampliado |
| L09 | Pré-admissão: `sem_dados` | CLT 4; L-112; D3 | S20 ampliado |
| L10 | Sem batida, antes da entrada: `previsto` | D4 | S03 ampliado |
| L11 | Skip: regime 24x48 integral | CLT 58/59/71/73 | Novo |
| L12 | Skip: 24x48 com saída às 06h | CLT 58/59/71/73 | Novo |
| L13 | Sábado: trabalhado 240; HE 0 | CLT 58/59/71; L-024 | Novo |
| L14 | Sábado: trabalhado 300; HE50 60 | CLT 59 §1; 71 §1 | Novo |
| L15 | Intermitente: trabalhado 180; descontos 0 | CLT 452-A/4 | Novo |
| L16 | Intermitente sem convocação: trabalho/falta 0 | CLT 452-A | Novo |
| L17 | Municipal local: trabalhado 480; dobra 480 | Lei 605/49 arts.8/9; Súmula 146 | Novo |
| L18 | Outra praça: trabalhado 480; dobra/HE 0 | Alcance municipal; Lei 605/49 | Novo |
| L19 | DSR: 0 semanas mantidas; 1 perdida | Lei 605/49 art.6; L-039 | Novo |
| L20 | DSR: 1 semana mantida; 0 perdidas | Lei 605/49 arts.1/6; CLT 67 | Novo |

A grade de 44h usa ciclo **semanal**, com cinco jornadas de 8h e sábado de 4h: são seis dias de trabalho.

Perguntas registradas como skips:

- **L02:** qual palavra e retenção correspondem ao cadastro × realidade?
- **L11/L12:** qual norma autoriza o 24x48 e define intervalo, noturno e HE? As alternativas numéricas estão abertas nas docstrings.
- **Subtests dos leitores:** qual tradução corresponde aos vocabulários paralelos sem mapa declarado?
- **L07, subtests específicos:** como consultar o dia histórico em `minha_escala` e no painel, cujas portas não oferecem esse recorte?

## 30/10 22:19 · sessao fee802eca215 · O261-V-FASE · verificador · origem=Code

**Há achados procedentes de integração: a chamada direta das pautas ignora a fase, e a resolução da lista ainda rebaixa C10 apesar de seu dono agora vir do e6.** Há também diferença temporal entre as lavras lidas pelo juiz e pela C10. (`app/ponto/management/commands/pautas_discordantes.py:94`; `app/escala/services/cadastro_realidade.py:708`; `app/ponto/management/commands/e6_oraculo.py:162`; `app/escala/services/cadastro_realidade.py:339`.)

1. **Pautas: falta repassar a fase, mas só acrescentar o argumento não resolve.** O pré-filtro chama `dono_da_divergencia(horas, False)` e pede o motor para qualquer resposta diferente de BATIDA. Se `cxr()` falhar, descarta o colaborador antes da decisão completa por `_ins.dono()`. (`app/ponto/management/commands/pautas_discordantes.py:93-104`.)

   Contraexemplo: único dia discordante, fase lavrada, cadastro válido, nenhuma batida e motor indisponível. O juiz completo devolveria CADASTRO sem motor; o pré-filtro força `cxr()` e produz apuração vazia. Mesmo passando `fase_deslocada=True`, `CADASTRO != BATIDA` continua verdadeiro: é preciso consumir a decisão com os insumos completos e sua economia existente. (`app/ponto/management/commands/e6_oraculo.py:107`; `app/ponto/management/commands/e6_oraculo.py:169-176`; `app/ponto/management/commands/pautas_discordantes.py:94-101`.)

   Em `--apply`, essa ausência pode fechar pauta existente, pois a sincronização fecha as chaves que saíram do alvo. (`app/ponto/management/commands/pautas_discordantes.py:113`; `app/ponto/services/pautas_dos_discordantes.py:165-179`.)

2. **C10 agora é resposta do e6, mas `dias_na_lista` continua privilegiando C9.** O censo grava `dono_do_dia` perguntando ao juiz; entretanto, `DONO_PELO_JUIZ` contém somente C9 e o comentário ainda atribui C10 à função apagada. Isso contradiz o contrato local de que donos divergentes do mesmo nível produzem `None`. (`app/ponto/management/commands/censo_fase_12x36.py:260-274`; `app/escala/services/cadastro_realidade.py:706-719`.)

   Contraexemplos: C9=CADASTRO/C10=BATIDA resulta CADASTRO; C9=ESTRUTURA/C10=CADASTRO resulta ESTRUTURA. O segundo mantém o alarme mesmo diante de outra resposta do mesmo juiz. A combinação ESTRUTURA/CADASTRO é inclusive exigida pelo selo antigo; ocorrência dessas combinações na frota real **[nao verificado]**. (`app/escala/services/cadastro_realidade.py:716-719`; `app/escala/tests/test_o261_v_fase_na_lista.py:326-332`; `app/chamados/services/vigia_fio.py:266`.)

3. **C10 perde o corte temporal recebido pela lista.** O juiz lê `ultimo_lavrado(self.hoje)`; C10 chama `ultimo_lavrado()` sem limite, embora `lista()` receba `hoje` e o vigia o encaminhe. (`app/ponto/management/commands/e6_oraculo.py:162`; `app/escala/services/fase_12x36.py:103-105`; `app/escala/services/cadastro_realidade.py:339`; `app/escala/services/cadastro_realidade.py:610`; `app/chamados/services/vigia_fio.py:259`.)

   Contraexemplo: existem lavras de 30 e 31/10, com conjuntos de dias diferentes. E6 com `--hoje 30/10` usa até 30/10; vigia com `hoje=30/10` pode consumir C10 de 31/10. O caminho de código permite essa divergência; ocorrência real **[nao verificado]**. (`app/ponto/management/commands/e6_oraculo.py:278`; `app/escala/services/fase_12x36.py:103-105`; `app/escala/services/cadastro_realidade.py:339`.)

O mapa de escrita e leitura ficou assim:

| Insumo ou leitor | Quem escreve / quem lê / referência temporal |
|---|---|
| `fase_deslocada` | `InsumosDoDono.dono()` monta o booleano pela presença da data em `fase()`; o juiz apenas consome o booleano, sem medir fase. (`app/ponto/management/commands/e6_oraculo.py:107`; `:159-170`.) |
| `fase()` / `dias_de_fase` | Sem injeção, lê os itens do colaborador na última lavra com `data_ref <= hoje`. O censo injeta os dias desta corrida, dispensando essa leitura. (`app/ponto/management/commands/e6_oraculo.py:135`; `:159-164`; `app/ponto/management/commands/censo_fase_12x36.py:260-261`.) |
| `dono_do_dia` da lavra | O censo obtém cada dono por `_ins.dono()` e o grava nos itens; `fase_12x36.lavrar()` persiste uma fotografia por dia. (`app/ponto/management/commands/censo_fase_12x36.py:262-276`; `app/escala/services/fase_12x36.py:84-92`.) |
| E6 / R1 | Usa `HOJE` do argumento ou do instante do ato; consulta o dono pela montagem única. R1 aponta para esse comando. (`app/ponto/management/commands/e6_oraculo.py:266`; `:278`; `:309`; `:359`; `app/core/placar_estrutural.py:115-138`.) |
| `pautas_discordantes` | A decisão final usa `InsumosDoDono(...hoje...)`; o pré-filtro direto é a lacuna descrita acima. (`app/ponto/management/commands/pautas_discordantes.py:82`; `:94`; `:104`.) |
| C9 | Na medição, cria insumos por competência do dia, com o `hoje` da medição, e grava o dono. Depois a lista lê a lavra C9 mais recente, sem limite temporal. (`app/ponto/services/teto_na_lista.py:49-50`; `:67-86`; `:120-129`; `app/escala/services/cadastro_realidade.py:319-322`.) |
| C10 / `dias_na_lista` / vigia | C10 copia o dono já gravado; `dias_na_lista` resolve os donos por colaborador/data; o vigia aceita BATIDA ou CADASTRO desse mapa, sem chamar novamente e6. (`app/escala/services/cadastro_realidade.py:339-342`; `:735-742`; `app/chamados/services/vigia_fio.py:259-266`.) |

**O novo ramo realiza BATIDA antes de fase**, nesta ordem: exceções CADASTRO de cadastro → batida aberta real → contagem ímpar → fase → lista do motor/resíduo. Portanto, “BATIDA primeiro” vale perante o novo ramo, mas não é absoluto. Por exemplo, ímpar + `sem_vinculo_no_dia=True` + fase retorna CADASTRO; fase + contagem par + batida aberta real retorna BATIDA. São regras anteriores explicitamente documentadas, não uma inversão introduzida pelo ramo de fase. (`app/ponto/management/commands/e6_oraculo.py:77-109`.)

**Não encontrei circularidade operacional no censo:** ele injeta os dias recém-julgados, inicializa `_fase` e só depois persiste a fotografia; esse caminho não lê a lavra anterior. `fase_12x36.dono_do_dia` foi apagado com lápide L-111. A arbitragem problemática restante é a prioridade por assinatura em `_dono_resolvido`, não outra medição da fase. (`app/ponto/management/commands/censo_fase_12x36.py:255-276`; `app/ponto/management/commands/e6_oraculo.py:135`; `:160`; `app/escala/services/fase_12x36.py:78-81`; `app/escala/services/cadastro_realidade.py:716-719`.)

O custo adicional da **leitura de fase** é uma query por instância consultada, com cache; com `dias_de_fase`, zero queries de lavra. Isso não significa uma query total por `InsumosDoDono`: há outras cargas de batidas, turnos e cadastro. A seleção é pela fotografia de `hoje`, não pela fotografia histórica de cada dia julgado; o censo mede corrente e anterior. (`app/ponto/management/commands/e6_oraculo.py:135-170`; `app/escala/services/fase_12x36.py:102-105`; `app/ponto/management/commands/censo_fase_12x36.py:83`.)

Os selos novos **têm mordida** na delegação censo→e6, na leitura da lavra e na prioridade da batida ímpar. Não exercem os contraexemplos das pautas nem duas lavras temporais; os selos antigos ainda exigem a hierarquia C9>C10. (`app/escala/tests/test_o261_v_fase_na_lista.py:345-379`; `:319-332`.)

`JUIZES.md` manda prevalecer o código vivo, mas sua tabela não registra e6 nem o censo como autoridades dessas perguntas; ela termina com vínculo e cobertura da data. (`app/docs/JUIZES.md:14-15`; `:74-80`.)

As reproduções foram em memória, com dependências simuladas, sem escrita de arquivos; suite Django e efeitos na base real **[nao verificado]**. [nao verificado]

## 30/10 22:17 · sessao 6384ea4d6b62 · - · - · origem=Ronald (tmux)

Achados nos leitores:

1. **Pautas omite fase no chamador direto e pode apagar a pauta válida.** O pré-filtro chama `dono_da_divergencia(horas, False)` sem `fase_deslocada`, exige `cxr()` para qualquer dia par e descarta o colaborador se o motor falhar; a decisão final só acontece depois desse descarte. Referências: `app/ponto/management/commands/pautas_discordantes.py:93`, `:97`, `:101`, `:104`. Contraexemplo executado somente em memória, com AST real e dependências simuladas: dia sem batida, fase lavrada, motor indisponível → `apurar=[]`; o juiz com fase devolve `CADASTRO` sem motor (`app/ponto/management/commands/e6_oraculo.py:107`, `:169`, `:173`). O pré-filtro repetido já estava registrado em `app/docs/BACKLOG.md:847`, mas agora a nova fase torna essa omissão relevante ao corte.

2. **C10 não herda o corte temporal.** `lista(hoje=...)` chama `_fase_deslocada(empresas)` sem hoje; ela consulta `ultimo_lavrado()` sem `ate`. Referências: `app/escala/services/cadastro_realidade.py:542`, `:548`, `:610`, `:335`, `:339`. Já o juiz usa `ultimo_lavrado(self.hoje)` (`app/ponto/management/commands/e6_oraculo.py:159`, `:162`), cujo filtro é `data_ref__lte=ate` (`app/escala/services/fase_12x36.py:103`). Contraexemplo: lavra de 31/10 existente; vigia rodado com `hoje=30/10` lê C10 de 31/10, enquanto e6 com `--hoje 30/10` lê até 30/10. O vigia propaga hoje para lista (`app/chamados/services/vigia_fio.py:259`), mas C10 o perde.

3. **Resolução C9/C10 conserva hierarquia anterior ao corte.** C10 agora grava dono pelo e6 (`app/ponto/management/commands/censo_fase_12x36.py:260`, `:262`, `:273`), porém `DONO_PELO_JUIZ` contém somente C9 (`app/escala/services/cadastro_realidade.py:706`, `:708`). `_dono_resolvido` dá precedência a C9, embora declare que divergência entre donos do mesmo nível deve produzir `None` (`:714`, `:716`, `:719`). O teste ainda exige que C9 `ESTRUTURA` vença C10 `CADASTRO` (`app/escala/tests/test_o261_v_fase_na_lista.py:326`). Reprodução em memória confirmou esse resultado. **Ressalva:** C9 `ESTRUTURA` é contraexemplo sintético usado pelo próprio teste; alcançabilidade na geometria real `[nao verificado]`.

Mapa temporal e de leitores:

- **e6/R1:** instante capturado e HOJE por argumento/default em `app/ponto/management/commands/e6_oraculo.py:266`, `:278`; `InsumosDoDono(...HOJE, agora...)` em `:309`; dono em `:359`. A referência R1 aponta para esse comando em `app/core/placar_estrutural.py:115`, `:138`.
- **Pautas:** `InsumosDoDono(...hoje...)` em `app/ponto/management/commands/pautas_discordantes.py:82`; captura do instante/default de hoje em `:110`.
- **C9:** `medir` captura instante/default em `app/ponto/services/teto_na_lista.py:49`; cria insumos por competência do dia, sempre com o hoje da medição (`:67`, `:72`), e grava dono (`:86`). A leitura da C9 pega a lavra mais recente sem limite temporal (`:120`, `:123`; `app/escala/services/cadastro_realidade.py:319`).
- **C10:** lê diretamente os `itens` e o `dono_do_dia` gravado, sem chamar novamente e6 (`app/escala/services/cadastro_realidade.py:339`, `:342`).
- **dias_na_lista/vigia:** agrupa assinaturas por colaborador/data e resolve os donos (`app/escala/services/cadastro_realidade.py:735`); vigia consulta esse mapa e aceita `BATIDA`/`CADASTRO` (`app/chamados/services/vigia_fio.py:255`, `:259`, `:266`).

`app/docs/JUIZES.md:14` declara que o código vivo prevalece e o lookup deve acompanhar a mudança; a tabela termina em `:80` sem registrar o juiz de dono e6 nem o censo de fase (`app/docs/JUIZES.md:19`).

## 30/10 22:17 · sessao 7961abb2b164 · - · - · origem=Ronald (tmux)

Achado procedente: **a C10 passou a lavrar o dono pelo e6, mas `dias_na_lista` continua classificando apenas C9 como decisão do juiz**. O censo pergunta ao e6 e grava a resposta em `dono_do_dia` (`app/ponto/management/commands/censo_fase_12x36.py:260`, `:273`); porém `DONO_PELO_JUIZ = {'C9'}` permanece, com comentário atribuindo a C10 à função apagada (`app/escala/services/cadastro_realidade.py:706`). A resolução continua privilegiando C9 em qualquer divergência (`:716`–`:719`).

Contraexemplos confirmados executando em memória as funções extraídas por AST, sem Django e sem escrita:

- C9=`CADASTRO`, C10=`BATIDA` → `CADASTRO`; assim uma classificação BATIDA agora lavrada pelo e6 na C10 pode ser descartada (`app/escala/services/cadastro_realidade.py:716`).
- C9=`ESTRUTURA`, C10=`CADASTRO` → `ESTRUTURA`, preservando alarme mesmo quando a C10 contém resposta do mesmo juiz (`:716`). A existência dessas combinações na base real não foi medida `[nao verificado]`.

Os testes antigos cristalizam a prioridade: exigem C9=`BATIDA` vencer C10=`CADASTRO` e C9=`ESTRUTURA` vencer C10=`CADASTRO`, apesar da nova origem da C10 (`app/escala/tests/test_o261_v_fase_na_lista.py:319`, `:326`). É lacuna da atualização do consumidor, não segunda medição da fase (`app/escala/services/cadastro_realidade.py:711`).

O caminho de gravação **evita circularidade operacional**: o censo calcula os dias em desvio e os injeta explicitamente em `dias_de_fase` (`app/ponto/management/commands/censo_fase_12x36.py:255`–`:262`); isso inicializa `_fase` e impede `fase()` de ler a lavra anterior (`app/ponto/management/commands/e6_oraculo.py:135`, `:159`–`:164`). O escritor persiste uma fotografia por dia em `MetricaSnapshot`, chave `desvio_de_cadastro_12x36`, escopo tenant (`app/escala/services/fase_12x36.py:75`, `:84`–`:93`).

A função antiga `dono_do_dia` foi removida e recebeu lápide L-111 (`app/escala/services/fase_12x36.py:78`–`:81`). O novo selo verifica a delegação por mock e exige pelo menos uma chamada com `fase_deslocada=True` (`app/escala/tests/test_o261_v_fase_na_lista.py:345`–`:353`); os selos seguintes verificam leitura real da lavra, prioridade da batida ímpar e juiz puro (`:365`–`:379`). Não executei os testes Django `[nao verificado]`.

`app/docs/JUIZES.md` não registra o e6 nem o censo como autoridade: sua tabela termina com as perguntas de escala, vínculo e cobertura da data (`app/docs/JUIZES.md:74`–`:80`). O próprio documento exige reconciliar a consulta quando divergir do código (`:14`–`:15`); os endereços vivos desta obra estão documentados nos módulos (`app/escala/services/fase_12x36.py:3`–`:8`, `app/ponto/management/commands/e6_oraculo.py:90`–`:95`).

## 30/10 22:16 · sessao 808aaa25ab96 · - · - · origem=Ronald (tmux)

Achado procedente: **`pautas_discordantes` omite fase na pré-decisão e pode excluir um dia que já tem dono CADASTRO.** O comando chama diretamente `dono_da_divergencia(horas, False)` para decidir se roda o motor; em dia par de fase lavrada isso retorna ESTRUTURA, força `cxr()`, e uma falha do espelho descarta o colaborador antes da classificação final. Já `InsumosDoDono.dono()` receberia `fase_deslocada=True` e retornaria CADASTRO sem consultar o motor. Contraexemplo: único item discordante em 25/10, fase lavrada, `horas=[]`, cadastro válido, sem batida aberta e espelho levantando exceção ⇒ `apurado=[]`. Reproduzido executando em memória a AST real de `apurar` com dependências simuladas: `cxr=1`, `dono=0`; não executei Django/DB. Âncoras: `app/ponto/management/commands/pautas_discordantes.py:93`, `:97`, `:101`, `:104`; `app/ponto/management/commands/e6_oraculo.py:169`, `:173`, `:175`.

O novo ramo respeita BATIDA antes de fase: batida aberta real ⇒ BATIDA; paridade ímpar ⇒ BATIDA; fase ⇒ CADASTRO; lista do motor/resíduo por último. Entretanto, BATIDA não é absolutamente o primeiro ramo: folga indefinida e ausência de vínculo retornam CADASTRO antes dele. Contraexemplo executado em memória: `dono_da_divergencia(['07:00'], False, sem_vinculo_no_dia=True, fase_deslocada=True)` ⇒ CADASTRO. Isso é regra anterior preservada pelo delta, com documentação e testes explícitos; recomendo apresentar como ressalva da leitura literal, não como regressão autônoma. Também `horas=[]`, fase e batida aberta real ⇒ BATIDA, embora não haja contagem ímpar, preservando outro aval anterior. Fontes: `app/ponto/management/commands/e6_oraculo.py:77`, `:85`, `:97`, `:103`, `:105`, `:107`; `app/ponto/tests/test_dono_le_a_tela.py:32`; `app/ponto/tests/test_e6_janela_declarada_nao_e_divergencia.py:164`, `:172`.

Não há circularidade operacional lavra→juiz→lavra: o censo injeta `dias_de_fase` calculados nesta corrida; isso inicializa `_fase` e impede a leitura da lavra anterior. Só depois são montados os itens e chamada `lavrar`. A função antiga `fase_12x36.dono_do_dia` foi removida, com lápide. Fontes: `app/ponto/management/commands/censo_fase_12x36.py:255`, `:260`, `:261`, `:262`, `:272`; `app/ponto/management/commands/e6_oraculo.py:135`, `:160`; `app/escala/services/fase_12x36.py:78`.

O custo adicional da fase é **uma consulta por instância** de `InsumosDoDono` que consulta `dono`, não por dia: `fase()` guarda o conjunto em `_fase`; `ultimo_lavrado` termina em uma `.first()`. Com dias injetados, são zero consultas de lavra. O custo por instância inclui percorrer todos os itens da lavra para filtrar o colaborador. Fontes: `app/ponto/management/commands/e6_oraculo.py:135`, `:159`, `:162`, `:163`, `:170`; `app/escala/services/fase_12x36.py:102`, `:105`.

Os selos novos têm mordida para a ligação censo→juiz e para a leitura da lavra pelo carregador: um troca o retorno do juiz e exige esse retorno gravado; outro lavra de verdade, exige CADASTRO no dia sem batida e BATIDA após inserir batida ímpar. Não cobrem a pré-decisão de `pautas_discordantes`, seleção temporal entre lavras, ausência de circularidade com lavra antiga conflitante nem contagem de consultas. Fontes: `app/escala/tests/test_o261_v_fase_na_lista.py:345`, `:348`, `:352`, `:353`, `:365`, `:369`, `:375`.

`JUIZES.md` determina que o código vivo prevalece e aponta o registro `core/juizes.py`; sua tabela não documenta o juiz da fase nem o dono e6. Fontes: `app/docs/JUIZES.md:11`, `:14`, `:19`, `:80`.

## 30/10 22:15 · sessao c18b7a251a62 · O261-E-CELULA-20 · verificador · origem=Code

**1. (a) Sim; (b) depende de qual instância é lida ao final. O teste não garante, sozinho, identidade das listas.** Ele verifica quantidade de carregamentos, cegueira e valor no carimbo. ([test_o261e_f5_alcance.py:254](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/tests/test_o261e_f5_alcance.py:254))

O `censo()` fica efetivamente cego: busca chamados vivos vinculados à célula e, com universo vazio, retorna `cego=True`, preservando `fio_do_admin_sem_resposta=0`, antes de consultar a lista. ([vigia_fio.py:285](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:285), [vigia_fio.py:316](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:316), [vigia_fio.py:323](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:323))

Valores deduzidos do código para `reg['censo']['fio_do_admin_sem_resposta']`:

| Costura | Valor | Resultado previsto |
|---|---:|---|
| Atual: uma lista, leitura depois dos dois universos | **1** | Satisfaz a assertiva; o command transfere o contador da lista para `d`. |
| (a) Apagar a leitura final, ou antecipá-la para antes do alcance | **0** | Vermelho na assertiva que exige 1. |
| (b) Listas separadas; leitura final observa a lista entregue ao `censo()` | **0** | Vermelho: essa instância permaneceu intocada. |
| (b) Listas separadas; leitura final observa a lista entregue ao alcance | **1** | Escapa deste teste: apenas o alcance carrega, portanto `m.call_count` continua 1. | [nao verificado]

A fundamentação da tabela é o retorno antecipado do `censo`, o carregamento tardio, o registro da falha pelo alcance e a leitura final do command. ([vigia_fio.py:187](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:187), [vigia_fio.py:208](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:208), [vigia_fio.py:567](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:567), [vigiar_fio_morto.py:166](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/management/commands/vigiar_fio_morto.py:166))

O contraexemplo concreto é trocar somente a chamada do `censo()` por `censo(..., lista=ListaDoAdmin(hoje, empresas))`, mantendo `lista` no alcance e na leitura final. São duas instâncias, mas as assertivas dessa fixture continuam satisfeitas por inferência estática. ([vigiar_fio_morto.py:167](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/management/commands/vigiar_fio_morto.py:167), [test_o261e_f5_alcance.py:254](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/tests/test_o261e_f5_alcance.py:254))

O RED fornecido mostra `1 != 0`, mas não identifica a alteração aplicada; comprovação experimental individual dos dois mutantes: **[não verificado]**. [nao verificado]

**2. A linha explica parcialmente os números: explicita o segundo, mas omite o universo do primeiro.** ([vigia_fio.py:793](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:793))

- **`FIO_DO_ADMIN`** conta dias classificados nesse balde pelo **censo antigo**, cujo universo parte de chamados vivos com célula; `celulas` lista IDs desse mesmo balde, limitada por `limite`. ([vigia_fio.py:285](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:285), [vigia_fio.py:380](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:380))
- **`sem_resposta_da_RODADA`** conta pares distintos `(colaborador, data)` que chegaram ao predicado e encontraram a lista sem resposta, nos dois universos. Não conta carregamentos: usa um conjunto e retorna seu tamanho. ([vigia_fio.py:185](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:185), [vigia_fio.py:204](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:204), [vigia_fio.py:227](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:227))
- O alcance possui **`alcance_fio_do_admin` separado**, publicado na linha `VFM|ALCANCE`; ele não é somado ao primeiro número acima. ([vigia_fio.py:569](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:569), [vigia_fio.py:623](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:623), [vigia_fio.py:795](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:795))

Quanto aos leitores:

- O command imprime a linha e grava o dicionário no carimbo; a chave JSON continua **`fio_do_admin_sem_resposta`**. ([vigiar_fio_morto.py:169](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/management/commands/vigiar_fio_morto.py:169), [vigiar_fio_morto.py:216](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/management/commands/vigiar_fio_morto.py:216))
- Os testes leem essas chaves diretamente e pelo carimbo. ([test_o261_vigia_fio.py:579](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/tests/test_o261_vigia_fio.py:579), [test_o261e_f5_alcance.py:256](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/tests/test_o261e_f5_alcance.py:256))
- **O ESTADO ainda não expõe essas chaves.** `fio_estado.py` abre o carimbo, mas omite ambas da seleção devolvida; `gerar_estado.py` usa esse leitor. A inclusão está registrada como pendência. ([fio_estado.py:82](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/bin/fio_estado.py:82), [gerar_estado.py:260](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/bin/gerar_estado.py:260), [BACKLOG.md:785](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/docs/BACKLOG.md:785))
- Outro parser específico da linha literal, ou consumidor externo à cópia: **[não verificado]**.

## 30/10 22:15 · sessao 39a15541c049 · - · - · origem=Ronald (tmux)

- A linha é **parcialmente explícita**: descreve `sem_resposta_da_RODADA` como dias distintos dos dois universos, mas não identifica `FIO_DO_ADMIN` e `celulas` como exclusivos do `censo()` antigo. Eles vêm de `d['fio_do_admin']` e `d['celulas_fio_do_admin']`, incrementados somente naquele censo; seu universo começa por chamados vivos vinculados à célula. `app/chamados/services/vigia_fio.py:793`, `app/chamados/services/vigia_fio.py:795`, `app/chamados/services/vigia_fio.py:380`, `app/chamados/services/vigia_fio.py:285`.
- O segundo número conta pares distintos `(colab_id, data_iso)` que **chegaram ao predicado** `dia_do_admin()` e encontraram a lista sem resposta; não conta tentativas de carregar nem todos os dias dos universos. O `set` deduplica e a propriedade devolve seu tamanho. `app/chamados/services/vigia_fio.py:185`, `app/chamados/services/vigia_fio.py:204`, `app/chamados/services/vigia_fio.py:208`, `app/chamados/services/vigia_fio.py:227`.
- O alcance mantém o próprio `alcance_fio_do_admin`, publicado na linha `VFM|ALCANCE|porta_fila=...|fio_do_admin=...`; não soma esse valor ao primeiro número de `VFM|FIO_DO_ADMIN`. `app/chamados/services/vigia_fio.py:471`, `app/chamados/services/vigia_fio.py:569`, `app/chamados/services/vigia_fio.py:623`, `app/chamados/services/vigia_fio.py:627`, `app/chamados/services/vigia_fio.py:795`.
- O command consome `linhas(d)` para stdout e guarda o dicionário inteiro em `reg['censo']`, preservando o nome JSON `fio_do_admin_sem_resposta`; só o rótulo textual virou `sem_resposta_da_RODADA`. `app/chamados/management/commands/vigiar_fio_morto.py:169`, `app/chamados/management/commands/vigiar_fio_morto.py:173`, `app/chamados/management/commands/vigiar_fio_morto.py:216`, `app/chamados/services/vigia_fio.py:795`.
- O consumidor do carimbo `bin/fio_estado.py` abre o JSON, mas sua seleção devolvida não inclui essas duas chaves; a comparação genérica considera somente `esperado`, e o command não inclui nenhuma delas no esperado. Assim o ESTADO atual não as mostra nem as avalia; `bin/gerar_estado.py` usa esse leitor/compositor. `bin/fio_estado.py:64`, `bin/fio_estado.py:72`, `bin/fio_estado.py:82`, `bin/fio_estado.py:92`, `app/chamados/management/commands/vigiar_fio_morto.py:87`, `app/chamados/management/commands/vigiar_fio_morto.py:95`, `bin/gerar_estado.py:260`.
- Há leitores de teste: verificam `fio_do_admin`, `fio_do_admin_sem_resposta`, deduplicação e carimbo; o teste do carimbo antigo compara todo `reg['censo']`. `app/chamados/tests/test_o261_vigia_fio.py:565`, `app/chamados/tests/test_o261_vigia_fio.py:579`, `app/chamados/tests/test_o261_vigia_fio.py:588`, `app/chamados/tests/test_o261_vigia_fio.py:780`, `app/chamados/tests/test_o261e_f5_alcance.py:256`.
- A inclusão dessas chaves no ESTADO está **registrada como pendência**, não implementada no leitor atual. `app/docs/BACKLOG.md:785`, `bin/fio_estado.py:82`.
- Não localizei parser específico da linha `VFM|FIO_DO_ADMIN` na busca integral com arquivos ocultos/ignorados; inexistência de outros consumidores externos à cópia permanece **[não verificado]**.

## 30/10 22:14 · sessao d200e8aa973f · - · - · origem=Ronald (tmux)

A leitura permite afirmar:

- Fixture sem `_chamado()`: `app/chamados/tests/test_o261e_f5_alcance.py:248`; `setUp` apenas cria/lavra célula acusando (`:58–63`), enquanto criação explícita de chamado está no helper separado (`:66–73`). A confirmação no próprio teste é `assertTrue(reg['censo']['cego'])`, em `:255`. O RED anexado chega à assertiva seguinte (`:256`), portanto passou pela de cegueira; identificação exata do mutante usado nesse RED: **[não verificado]**.
- `censo()` parte de chamados vivos vinculados à célula (`app/chamados/services/vigia_fio.py:284–288`); sem eles, retorna `cego=True` antes de acessar a lista (`:323–325`), preservando `fio_do_admin_sem_resposta=0`, inicializado em `:316`. Portanto ele está cego nessa fixture.
- Fluxo correto: command cria objeto único, passa aos dois censos e só então lê `sem_resposta` (`app/chamados/management/commands/vigiar_fio_morto.py:166–169`). Alcance consulta lei/lista (`app/chamados/services/vigia_fio.py:567–568`); `dia_do_admin` registra `(colab_id,data)` no conjunto quando a lista falha (`:204–209`), e `sem_resposta` retorna o tamanho (`:225–227`). Carimbo conserva `d` (`app/chamados/management/commands/vigiar_fio_morto.py:216`).
- Mutante (a), apagar leitura final `:169`: carimbo `reg['censo']['fio_do_admin_sem_resposta']=0`; teste vermelho em `test_o261e_f5_alcance.py:256`. Leitura final antecipada antes do alcance tem mesmo resultado, pela preguiça do carregador (`vigia_fio.py:188–190`) e retorno cego anterior. [nao verificado]
- Mutante (b), listas separadas: **não há garantia geral de vermelho**. Se `lista` continua sendo objeto passado ao censo e alcance recebe `ListaDoAdmin(...)` novo, leitura final vê lista intocada → carimbo **0** e vermelho. O loader será chamado **uma vez**, pelo alcance, então assertiva `m.call_count==1` (`test:254`) passa.
- Contraexemplo do mutante (b): alterar `command:167` para `censo(..., lista=ListaDoAdmin(hoje, empresas))`, mantendo `command:168–169` como está. Cada universo recebe sua instância; a instância do censo jamais carrega, e `lista` do alcance contabiliza falha → carimbo **1**, `m.call_count==1`, `cego=True`, e teste pode ficar verde. Evidência: retorno cedo (`vigia_fio.py:323–325`), carregamento preguiçoso (`:187–190`), assertivas apenas comportamentais (`test:254–258`). **Não executar esse contraexemplo; resultado é inferência estática.** [nao verificado]

O teste vizinho que põe o dia em ambos universos exige um único carregamento (`app/chamados/tests/test_o261e_f5_alcance.py:206–215`), mas ele fornece a lista compartilhada diretamente às funções, sem exercitar a costura do command; portanto não fecha esse contraexemplo no command.

## 30/10 21:51 · sessao 03e08bc51dac · O261-E-CELULA-20 · verificador · origem=Code

**1. Carregador, predicado e igualdade**

Há **1 sítio que chama `lista(...)`**, seguido de `dias_na_lista(...)`, e **1 sítio que aplica o predicado BATIDA/CADASTRO**. Os dois censos chegam a ele por `dono_fora_do_fio`. ([vigia_fio.py:198](app/chamados/services/vigia_fio.py:198), [vigia_fio.py:210](app/chamados/services/vigia_fio.py:210), [vigia_fio.py:263](app/chamados/services/vigia_fio.py:263))

O command passa a mesma `ListaDoAdmin` aos dois. Isso garante **o mesmo resultado do predicado para o mesmo par colaborador/data**, com no máximo uma tentativa de carregamento, inclusive quando falha. ([vigiar_fio_morto.py:166](app/chamados/management/commands/vigiar_fio_morto.py:166), [vigia_fio.py:187](app/chamados/services/vigia_fio.py:187))

Isso **não garante o mesmo balde final**: `censo` aceita pergunta viva antes de consultar a lista; `censo_alcance` exige uma porta pela fila/app e pode continuar até a lista. Portanto, compartilhar a lista elimina divergência nessa consulta, mas não torna os universos equivalentes. ([vigia_fio.py:359](app/chamados/services/vigia_fio.py:359), [vigia_fio.py:556](app/chamados/services/vigia_fio.py:556), [vigia_fio.py:567](app/chamados/services/vigia_fio.py:567))

Sem `lista`, cada chamada cria sua instância: mesmo algoritmo, caches independentes, até dois carregamentos. A igualdade depende de obterem a mesma resposta da fonte; não há snapshot compartilhado. ([vigia_fio.py:339](app/chamados/services/vigia_fio.py:339), [vigia_fio.py:550](app/chamados/services/vigia_fio.py:550))

**2. Quem escreve e quem lê `fio_do_admin_sem_resposta`**

| Papel | Local |
|---|---|
| Acumula dias distintos sem resposta em um conjunto | [vigia_fio.py:208](app/chamados/services/vigia_fio.py:208) |
| Inicializa a chave com zero | [vigia_fio.py:316](app/chamados/services/vigia_fio.py:316) |
| Copia o tamanho do conjunto ao terminar `censo` | [vigia_fio.py:425](app/chamados/services/vigia_fio.py:425) |
| **Sobrescreve após os dois censos**, incluindo consultas exclusivas do alcance | [vigiar_fio_morto.py:169](app/chamados/management/commands/vigiar_fio_morto.py:169) |
| Lê para imprimir `FIO_DO_ADMIN…sem_resposta` | [vigia_fio.py:795](app/chamados/services/vigia_fio.py:795) |
| Transporta a chave no dicionário do carimbo | [vigiar_fio_morto.py:216](app/chamados/management/commands/vigiar_fio_morto.py:216) |
| Testes leem diretamente esperando 0, 1 e 3; outro compara o dicionário inteiro | [test_o261_vigia_fio.py:571](app/chamados/tests/test_o261_vigia_fio.py:571), [579](app/chamados/tests/test_o261_vigia_fio.py:579), [588](app/chamados/tests/test_o261_vigia_fio.py:588), [780](app/chamados/tests/test_o261_vigia_fio.py:780) |

O teste novo de falha lê **`lista.sem_resposta`**, sem conferir a chave final do command. ([test_o261e_f5_alcance.py:226](app/chamados/tests/test_o261e_f5_alcance.py:226))

`bin/fio_estado.py` não publica essa chave; sua comparação genérica só considera campos de `esperado`, onde ela também não consta. Assim, mudar somente esse contador no carimbo atual não muda sua saída. ([fio_estado.py:69](bin/fio_estado.py:69), [fio_estado.py:82](bin/fio_estado.py:82), [vigiar_fio_morto.py:95](app/chamados/management/commands/vigiar_fio_morto.py:95))

**Contra `21b9db38`: [não verificado]**, pois essa versão não está no material disponível. Contra o pai descrito no patch, existe mudança observável: se a lista falha e apenas o alcance consulta um dia, texto/carimbo passam de zero para um nessa chave; agora ela recebe a união dos dias consultados pelos dois censos. ([vigiar_fio_morto.py:167](app/chamados/management/commands/vigiar_fio_morto.py:167), [vigia_fio.py:208](app/chamados/services/vigia_fio.py:208))

**3. Classificação e força dos testes** [nao verificado]

Para **os mesmos resultados das fontes**, o delta fornecido preserva `(d)/(e)`: `(d)` exige colaborador na lista e dia fora dela; dia presente com ESTRUTURA/nenhum continua em `(e)` com detalhe. A mudança explícita é o rótulo publicado de `(d)`, agora `colab_na_lista_cxr_dia_fora`. ([vigia_fio.py:474](app/chamados/services/vigia_fio.py:474), [vigia_fio.py:588](app/chamados/services/vigia_fio.py:588), [vigia_fio.py:592](app/chamados/services/vigia_fio.py:592))

A causa `(b)` também preserva a expressão anterior: carimbo presente e texto vazio após `.strip()`. Houve extração do predicado, usado tanto na recusa da porta quanto na causa. ([vigia_fio.py:559](app/chamados/services/vigia_fio.py:559), [vigia_fio.py:584](app/chamados/services/vigia_fio.py:584), [vigia_fio.py:675](app/chamados/services/vigia_fio.py:675))

**Sim, testes novos passam com código errado:** [nao verificado]

- Criar outra `ListaDoAdmin` no command para o alcance, ou remover a atualização final do contador, escapa aos novos testes: eles passam a instância manualmente e não executam essa ligação do command. ([test_o261e_f5_alcance.py:206](app/chamados/tests/test_o261e_f5_alcance.py:206), [217](app/chamados/tests/test_o261e_f5_alcance.py:217), [vigiar_fio_morto.py:168](app/chamados/management/commands/vigiar_fio_morto.py:168))
- O selo AST aceita trocar `CADASTRO` por `ESTRUTURA`: suas contagens continuam iguais, embora o predicado fique errado. Testes anteriores rejeitam esse mutante; isso não significa sobrevivência à suíte inteira. ([test_o261e_f5_alcance.py:234](app/chamados/tests/test_o261e_f5_alcance.py:234), [vigia_fio.py:168](app/chamados/services/vigia_fio.py:168), [test_o261_vigia_fio.py:553](app/chamados/tests/test_o261_vigia_fio.py:553))

Ocorrência de dias produtivos afetados e resultado da suíte Django: **[não verificado]**. [nao verificado]

## 30/10 21:51 · sessao 17ddc5b11cdf · - · - · origem=Ronald (tmux)

Há lacunas nos testes novos, sem evidência de falha atual do código nesses pontos. [app/chamados/tests/test_o261e_f5_alcance.py:206-240] [nao verificado]

- **O selo AST aceita predicado errado.** Ele conta uma constante `BATIDA`, uma leitura de `DONOS_DA_MESA_DO_ADMIN` e chamadas simples `lista`/`dias_na_lista`; não verifica `CADASTRO`, o operador de pertinência nem a regra aplicada. Trocar a constante de `('BATIDA', 'CADASTRO')` para `('BATIDA', 'ESTRUTURA')` preserva todas as suas asserções, embora inverta indevidamente a aceitação de CADASTRO/ESTRUTURA. Demonstrei isso em memória, extraindo a classe e reproduzindo as contagens, sem gravar arquivos. [app/chamados/tests/test_o261e_f5_alcance.py:234-240; app/chamados/services/vigia_fio.py:168; app/chamados/services/vigia_fio.py:210]
- **Esse mutante não sobreviveria à suíte inteira:** testes anteriores exigem CADASTRO e rejeitam ESTRUTURA; o teste F5 anterior também rejeita ESTRUTURA. [app/chamados/tests/test_o261_vigia_fio.py:553-570; app/chamados/tests/test_o261e_f5_alcance.py:173-182]
- **A integração do command fica fora dos testes novos.** Eles criam `ListaDoAdmin` e a passam manualmente aos dois censos. Um mutante que crie outra lista no `censo_alcance` do command deixa esses testes intactos, mas reintroduz duas cargas na rodada real. [app/chamados/tests/test_o261e_f5_alcance.py:206-227; app/chamados/management/commands/vigiar_fio_morto.py:166-168]
- **A atualização final do contador no command também não é cobrada pelo teste novo de falha.** Remover `d['fio_do_admin_sem_resposta'] = lista.sem_resposta` não muda seu caminho de execução: ele verifica o objeto compartilhado e os contadores de fio morto, sem executar o command. Para revelar a diferença, seria necessário um dia sem resposta exclusivo do alcance, acrescentado depois do retorno de `censo`. [app/chamados/management/commands/vigiar_fio_morto.py:167-169; app/chamados/tests/test_o261e_f5_alcance.py:222-227]
- **O caso novo de falha mede apenas um dia compartilhado.** Uma implementação errada que devolvesse apenas `int(bool(_sem_resposta))` também satisfaria suas asserções; o teste anterior de três dias a reprovaria. [app/chamados/tests/test_o261e_f5_alcance.py:219-227; app/chamados/services/vigia_fio.py:225-227; app/chamados/tests/test_o261_vigia_fio.py:581-588]

A suíte Django não foi executada; execução e resultados completos permanecem **[não verificado]**. [nao verificado]

_781 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
