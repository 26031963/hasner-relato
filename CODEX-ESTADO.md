# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 00:21** · publicado em **11/11/2026 00:21** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 00:21 · sessao 5f1efcf877ab · - · - · origem=Ronald (tmux)

**O ator identifica o contexto da gravação, mas não prova o fato que encerrou o dia.** `via=sistema` vem de `reconciliar()`; esse gate também admite agrupamento, deduplicação e furo sem dia identificável, além de premissa regularizada (`app/chamados/models.py:654`, `app/chamados/models.py:727`, `app/chamados/models.py:732`, `app/chamados/models.py:738`, `app/chamados/models.py:748`).

Os fechadores informados somam **47 ocorrências**, enquanto a lista contém 45 dias. Sem a ligação dia → chamado → log, sobreposição ou divergência da contagem permanece `[nao verificado]`.

**Gravação e guarda comuns**

- O escritor de `estado_chamado` grava estado anterior, destino, `via` e passa `user` para `evento()`. Dentro de requisição, `evento()` completa usuário e IP pelo contexto; fora dela, deriva `sistema:<comando>` quando o processo é `manage.py`. Portanto usuário/IP e coluna `ator` têm origens diferentes (`app/chamados/models.py:292`, `app/core/observ.py:22`, `app/core/observ.py:108`, `app/core/observ.py:118`).
- **F1 existe nesta cópia.** No sistema, `reconciliar()` verifica premissa e pergunta viva; reata todas as vivas a uma cobrança válida ou recusa. No humano, `transicionar()` recusa. O escritor ainda possui uma defesa adicional (`app/chamados/models.py:682`, `app/chamados/models.py:685`, `app/chamados/models.py:410`, `app/chamados/models.py:162`, `app/chamados/models.py:221`).
- **A guarda não consulta indiscriminadamente todas as perguntas do dia.** Consulta perguntas não validadas ancoradas pelo destino ou, sem destino, pelo chamado da disputa; considera competência comprovadamente aberta e julga a pergunta no pós-estado do fechamento. Competência entregue ou indecidível fica fora dessa F1 (`app/chamados/models.py:344`, `app/chamados/models.py:356`, `app/chamados/models.py:364`, `app/chamados/models.py:368`).
- A versão e o caminho efetivamente executados nos fechamentos históricos não constam dos anexos `[nao verificado]`.

1. **18 — `sistema:apurar_furos_diarios → resolvido`**

   **Sítios e condições:** o comando executa apuração, sincronização de adesão com `fechar=True` e agrupamento sob LIMBO (`app/escala/management/commands/apurar_furos_diarios.py:13`, `app/escala/management/commands/apurar_furos_diarios.py:17`, `app/escala/management/commands/apurar_furos_diarios.py:19`).

   Há três fechadores explícitos nesses serviços:

   - **Filho de adesão:** grava `agrupado_em=pai` e reconcilia o furo. A condição é o agrupamento sob o chamado de adesão; não exige uma batida que respondeu à pergunta daquele dia (`app/chamados/services/adesao.py:102`, `app/chamados/services/adesao.py:107`, `app/chamados/services/adesao.py:112`, `app/chamados/services/adesao.py:115`).
   - **Pai de adesão:** encontra qualquer batida não retratada do colaborador, sem recorte pelo dia do chamado, e reconcilia o pai. O fato testado é deixar de ser “nunca bateu” (`app/chamados/services/adesao.py:142`, `app/chamados/services/adesao.py:144`, `app/chamados/services/adesao.py:150`).
   - **Furo sob LIMBO:** encontra pai LIMBO vivo, defeito de folga no vínculo do dia e autorização de `pode_calar`; grava agrupamento, tenta reatar perguntas ao pai e reconcilia (`app/chamados/services/limbo_folgas.py:302`, `app/chamados/services/limbo_folgas.py:317`, `app/chamados/services/limbo_folgas.py:321`, `app/chamados/services/limbo_folgas.py:324`, `app/chamados/services/limbo_folgas.py:275`, `app/chamados/services/limbo_folgas.py:281`, `app/chamados/services/limbo_folgas.py:283`).

   **F1:** todos passam pela guarda comum; LIMBO também tenta a reata explicitamente antes do fechamento (`app/chamados/models.py:685`, `app/chamados/services/limbo_folgas.py:281`).

   **Prova por dia:** procurar `agrupado_em`, identidade e estado histórico do pai, notas `agrupado_adesao`, `agrupado_limbo` ou `adesao_fechada_por_batida`; no último caso, conferir a batida preexistente. Esses caminhos deixam notas distintas (`app/chamados/services/adesao.py:118`, `app/chamados/services/adesao.py:152`, `app/chamados/services/limbo_folgas.py:292`). Qual deles explica cada um dos 18 dias: **o codigo nao decide** com os anexos disponíveis `[nao verificado]`.

2. **17 — `via=admin`, usuário #846 → fechado**

   **Sítio compatível:** a tela de detalhe encaminha à resposta livre; conteúdo e novo status são independentes. Com conteúdo vazio, ainda pode chamar `transicionar(novo_status, request.user)`, produzindo `via=admin` e IP da requisição (`app/chamados/views.py:547`, `app/chamados/services/acoes_chamado.py:286`, `app/chamados/services/acoes_chamado.py:289`, `app/chamados/services/acoes_chamado.py:313`, `app/chamados/models.py:450`, `app/core/observ.py:110`).

   **Condição:** para destino `fechado`, exige transição permitida e as guardas de justificativa/disputa/pergunta. A exigência de `premissa_regularizada()` está no ramo **`resolvido`**, não no ramo `fechado`. Assim, esse sítio permite mudança humana de status sem registrar decisão sobre o dia (`app/chamados/models.py:132`, `app/chamados/models.py:136`, `app/chamados/models.py:148`, `app/chamados/models.py:181`).

   **Tela ou lote:** o fio/modal também pode gerar a assinatura, mas seu ramo exige conteúdo e grava nota. Existe comando de lote que chama `ch.fechar()`, porém sua execução normal por `manage.py` ganharia ator de comando. O lote de arquivamento da tela apenas silencia (`app/chamados/services/fio.py:709`, `app/chamados/services/fio.py:718`, `app/chamados/services/fio.py:721`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:98`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:112`, `app/core/observ.py:29`, `app/chamados/views_arquivar.py:140`). A assinatura fornecida não identifica a tela ou ação efetivamente usada `[nao verificado]`.

   **F1:** existe, com recusa e o alcance descrito acima (`app/chamados/models.py:162`, `app/chamados/models.py:344`, `app/chamados/models.py:364`).

   **Prova por dia:** cruzar o fechamento com decisão da pergunta, decisão da célula, batida ou ausência válida anterior ao fechamento. Followup tem autor, conteúdo e criação, mas texto livre não é a condição de regularização exigida por esse destino (`app/chamados/models.py:1668`, `app/chamados/models.py:1674`, `app/chamados/models.py:181`). Se houve ato real nos 17 casos, **o codigo nao decide** `[nao verificado]`.

3. **1 — `via=admin`, usuário #845 → fechado**

   Os mesmos sítios e condições do item anterior podem produzir essa assinatura: transição pela tela de detalhe, inclusive sem conteúdo, ou pelo fio com conteúdo; a gravação usa `via=admin` e o usuário recebido (`app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/fio.py:709`, `app/chamados/services/fio.py:721`, `app/chamados/models.py:450`).

   A F1 existe; `fechado` não exige a premissa regularizada que o ramo `resolvido` exige. A prova necessária é o ato/fato do dia anterior ao log, não o perfil superuser ou o IP (`app/chamados/models.py:162`, `app/chamados/models.py:181`, `app/core/models.py:302`, `app/core/models.py:307`). Qual sítio foi usado e se houve ato real: **o codigo nao decide** `[nao verificado]`.

4. **5 — `via=sistema`, `ator=null`, `usuario=null → resolvido`**

   **Essa assinatura não seleciona um sítio.** `reconciliar()` não passa usuário; sem requisição e fora de processo identificado como `manage.py`, `evento()` pode gravar ator e usuário nulos. Requisição sem usuário autenticado também pode deixar usuário nulo (`app/chamados/models.py:654`, `app/core/observ.py:29`, `app/core/observ.py:108`, `app/core/observ.py:115`, `app/core/observ.py:118`).

   Os seguintes sítios podem chegar ao mesmo escritor, conforme o contexto de execução:

   | Condição ou família do fechamento | Sítios |
   |---|---|
   | Disputa fechada encerra chamado-pai | `app/chamados/signals.py:28`, `app/chamados/signals.py:42` |
   | Batida, entrada sincronizada ou marco aceso | `app/ponto/services/triagem_batida.py:597`, `app/ponto/services/entrada_sincronizada.py:31`, `app/ponto/services/lastro.py:400` |
   | Materialização de resposta ou ausência decidida | `app/chamados/services/materializacao.py:581`, `app/chamados/services/materializacao.py:771` |
   | Ausência aprovada ou solicitação de ausência decidida | `app/chamados/services/silencio_ausencia.py:93`, `app/ponto/services/ausencia.py:692` |
   | Admin decidiu a célula | `app/ponto/services/veredito_celula.py:188`, `app/ponto/services/veredito_celula.py:202` |
   | Cartório: cobrança sem protesto, marco inexistente, dia sem trabalho/sem furo ou deduplicação | `app/ponto/services/cartorio.py:1493`, `app/ponto/services/cartorio.py:1512`, `app/ponto/services/cartorio.py:1525`, `app/ponto/services/cartorio.py:1611`, `app/ponto/services/cartorio.py:1626` |
   | Retratação de premissa | `app/chamados/reconciliador.py:518`, `app/chamados/reconciliador.py:523` |
   | Feriado, folga, vínculo, adesão ou agrupamento | `app/chamados/services/feriado_reavaliar.py:47`, `app/chamados/services/limbo_folgas.py:101`, `app/colaboradores/services/pos_vinculo.py:42`, `app/chamados/services/adesao.py:115`, `app/chamados/services/adesao.py:150` |
   | Cobertura/substituição ou cluster espúrio | `app/ponto/services/cobertura.py:122`, `app/ponto/services/cobertura.py:317`, `app/ponto/services/cluster_espurio.py:124` |

   **Condição efetiva:** além da condição do chamador, o gate comum aceita chamado fora do universo de furo, furo sem dia forte, premissa regularizada, agrupamento ou dedup. Logo não impõe universalmente “batida/ausência/decisão que respondeu à pergunta” (`app/chamados/models.py:727`, `app/chamados/models.py:732`, `app/chamados/models.py:738`, `app/chamados/models.py:748`).

   **F1:** existe em todos esses caminhos por `reconciliar()` (`app/chamados/models.py:685`).

   **Prova por dia:** exige identificar chamado e fechamento, depois recuperar a condição correspondente: disputa e resolução, batida/marco, ausência, decisão da célula, pai de agrupamento ou sobrevivente de dedup. A trilha distingue, por exemplo, `fechado_pela_disputa`, `pergunta_reatada`, `fechamento_por_dedup` e `fechamento_sem_dia_do_fato` (`app/chamados/signals.py:45`, `app/chamados/models.py:440`, `app/chamados/models.py:692`, `app/chamados/models.py:734`). Para os cinco casos concretos, sítio e condição permanecem `[nao verificado]`.

5. **3 — `via=sistema`, usuário #657, IP, `ator=null → resolvido`**

   **Sítio compatível com as ações vizinhas:** `reconciliar_por_ausencia()` chama `ch.reconciliar()`, anexa nota, levanta o silêncio quando marcado e grava `reconciliar_ausencia_lancada`. O levantamento usa precisamente `levantar_silencio_ausencia` (`app/chamados/services/silencio_ausencia.py:72`, `app/chamados/services/silencio_ausencia.py:101`, `app/chamados/services/silencio_ausencia.py:104`, `app/chamados/services/silencio_ausencia.py:107`, `app/chamados/services/silencio_ausencia.py:108`).

   **Por que usuário/IP com via sistema:** o fechamento continua sendo `reconciliar()` sem usuário explícito; executado dentro de requisição, o log herda usuário e IP, mantendo a coluna `ator` nula. A assinatura não demonstra uma transição humana `admin` (`app/chamados/models.py:654`, `app/core/observ.py:108`, `app/core/observ.py:123`).

   **Condição:** ausência cobrindo o dia, status `aprovada`, `_premissa_morre()` verdadeiro e autorização do gate comum. `_premissa_morre()` trata `justificativa` diretamente; nas demais classes verifica catálogo e célula sem veredito acusatório. Ausência aberta apenas silencia nesse serviço (`app/chamados/services/silencio_ausencia.py:28`, `app/chamados/services/silencio_ausencia.py:65`, `app/chamados/services/silencio_ausencia.py:77`, `app/chamados/services/silencio_ausencia.py:93`).

   **F1:** existe pelo gate comum (`app/chamados/models.py:685`).

   **Prova por dia:** `reconciliar_ausencia_lancada.valor_depois` traz data, ausência e tipo. Conferir essa ausência, cobertura, aprovação anterior e a condição histórica da célula (`app/chamados/services/silencio_ausencia.py:44`, `app/chamados/services/silencio_ausencia.py:108`, `app/ponto/models.py:750`, `app/ponto/models.py:768`, `app/ponto/models.py:799`, `app/ponto/models.py:810`). A vizinhança fornecida é compatível; sem IDs e sequência completa, atribuir os três fechamentos a esse encadeamento permanece `[nao verificado]`.

6. **1 — `sistema:reconciliar_chamados → resolvido`**

   **Sítio:** o comando desta cópia **não fecha**. Calcula veredito, consulta `reconciliaria()` e conta resíduos; não chama o escritor nesse percurso (`app/chamados/management/commands/reconciliar_chamados.py:71`, `app/chamados/management/commands/reconciliar_chamados.py:76`, `app/chamados/management/commands/reconciliar_chamados.py:79`, `app/chamados/management/commands/reconciliar_chamados.py:85`).

   O executor preservado no reconciliador é `_retratar() → ch.reconciliar()`, chamado quando o veredito indica premissa morta. O coletor verifica fatos diferentes conforme módulo/subcategoria; ausência de turno aberto, por exemplo, não exige expressamente encontrar uma saída que fechou o par (`app/chamados/reconciliador.py:523`, `app/chamados/reconciliador.py:1458`, `app/chamados/reconciliador.py:1464`, `app/chamados/reconciliador.py:262`, `app/chamados/reconciliador.py:142`, `app/chamados/reconciliador.py:152`).

   **F1:** existe nesse executor atual (`app/chamados/models.py:685`). Qual sítio, condição e guarda produziram o log histórico atribuído ao comando: `[nao verificado]`.

   **Prova por dia:** módulo/subcategoria/contexto, motivo da retratação e fato correspondente anterior ao fechamento. O motivo é transportado à retratação e à nota; o estado atual de `TurnoMaterializado` não fornece sozinho o estado histórico do par (`app/chamados/reconciliador.py:1464`, `app/chamados/reconciliador.py:538`, `app/ponto/models.py:1206`, `app/ponto/models.py:1208`, `app/ponto/models.py:1213`).

7. **1 — `sistema:detectar_ausencias → resolvido`**

   **Sítios e condições possíveis:**

   - LIMBO com previsão novamente calculável, sem defeito atual e sem filhos ainda no defeito (`app/ponto/management/commands/detectar_ausencias.py:145`, `app/ponto/management/commands/detectar_ausencias.py:156`, `app/ponto/management/commands/detectar_ausencias.py:158`).
   - Ausência cobrindo o dia, seguida de fechamento por afastamento. O coletor usa `para='cobranca'`, que aceita ausência não rejeitada; não exige aprovação nesse coletor (`app/ponto/management/commands/detectar_ausencias.py:346`, `app/ponto/management/commands/detectar_ausencias.py:351`, `app/ponto/management/commands/detectar_ausencias.py:818`, `app/ponto/management/commands/detectar_ausencias.py:824`, `app/ponto/turnos.py:2518`).
   - Ordem de substituição em `designada/em_curso/concluida`, no intervalo consultado, e premissa morta (`app/ponto/management/commands/detectar_ausencias.py:365`, `app/ponto/management/commands/detectar_ausencias.py:373`, `app/ponto/management/commands/detectar_ausencias.py:834`).
   - Entrada atribuída ao turno do dia **e** todos os marcos cobrados acesos (`app/ponto/management/commands/detectar_ausencias.py:336`, `app/ponto/management/commands/detectar_ausencias.py:405`, `app/ponto/management/commands/detectar_ausencias.py:845`).

   **F1:** todos passam por `reconciliar()`; a condição do chamador ainda precisa vencer o gate comum (`app/ponto/management/commands/detectar_ausencias.py:158`, `app/ponto/management/commands/detectar_ausencias.py:824`, `app/ponto/management/commands/detectar_ausencias.py:834`, `app/ponto/management/commands/detectar_ausencias.py:845`, `app/chamados/models.py:685`).

   **Prova por dia:** notas `fechado_por_afastamento`, `fechado_por_cobertura` ou `fechado_por_batida` referenciam ausência, ordem ou entrada; conferir existência e validade temporal desses objetos e os marcos da cobrança (`app/ponto/management/commands/detectar_ausencias.py:827`, `app/ponto/management/commands/detectar_ausencias.py:837`, `app/ponto/management/commands/detectar_ausencias.py:848`). Qual ramo fechou o caso fornecido: `[nao verificado]`.

8. **1 — `sistema:fechar_chamados_ausencia → resolvido`**

   **Sítio:** com `--apply`, o comando chama `fechar_chamados_ausencia_decidida()`, que chama `fechar_chamado_da_ausencia() → ch.reconciliar()`. `--alarme` e execução sem `--apply` não fecham (`app/ponto/management/commands/fechar_chamados_ausencia.py:35`, `app/ponto/management/commands/fechar_chamados_ausencia.py:41`, `app/ponto/management/commands/fechar_chamados_ausencia.py:44`, `app/ponto/services/ausencia.py:729`, `app/ponto/services/ausencia.py:694`).

   **Condição:** chamado vivo `ausencia_colab`, ausência existente identificada por `contexto_json.ausencia_id` e status fora de `ABERTAS`. Pode ser aprovada **ou rejeitada**: o fato testado é a solicitação ter saído da decisão, não necessariamente cobertura aprovada do dia; não testa idade (`app/ponto/services/ausencia.py:709`, `app/ponto/services/ausencia.py:714`, `app/ponto/services/ausencia.py:716`, `app/ponto/services/ausencia.py:692`).

   **F1:** existe pelo gate comum (`app/ponto/services/ausencia.py:694`, `app/chamados/models.py:685`).

   **Prova por dia:** ausência ligada, status decidido antes do fechamento, `aprovada_em`, criação, trilha A4 de mudança de estado e nota `ausencia_decidida` (`app/ponto/models.py:810`, `app/ponto/models.py:813`, `app/ponto/services/ausencia.py:836`, `app/ponto/services/ausencia.py:697`). Esses registros individuais não foram fornecidos `[nao verificado]`.

Para todas as provas temporais, o limite deve ser o **`LogAuditoria.criado_em` do fechamento identificado**, com seus antes/depois; `Chamado.resolvido_em` pode ser preservado na passagem `resolvido → fechado` (`app/core/models.py:300`, `app/core/models.py:307`, `app/chamados/models.py:243`).

Batida precisa ter sido registrada até esse limite: `timestamp` é a hora declarada do fato; `criado_em`, sincronização, aprovação e retratação permitem conferir sua existência e validade. Para decisões, há `DecisaoPergunta` com pergunta, ato, instante, autor e vínculos; para célula, há veredito, via, instante e ata. A trilha `veredito_celula` conserva antes/depois do veredito, **sem a ata completa** (`app/ponto/models.py:23`, `app/ponto/models.py:67`, `app/ponto/models.py:73`, `app/ponto/models.py:77`, `app/ponto/models.py:79`, `app/chamados/models.py:3871`, `app/escala/models.py:1770`, `app/ponto/portas/celula.py:1042`). Portanto a fotografia atual desses objetos pode não bastar para provar a condição no fechamento histórico; essa prova por dia permanece `[nao verificado]`.

## 11/11 00:17 · sessao e2708cfe3a07 · - · - · origem=Ronald (tmux)

**Escritor e origem do ator.** `estado_chamado` nasce em `app/chamados/models.py:292`, com status anterior/novo, `via` e `ator=user`; `reconciliar()` chama esse núcleo sem `user`, com via sistema (`app/chamados/models.py:654`; `app/chamados/catalogo/motor.py:95`). Dentro de request, `evento()` supre usuário autenticado e IP; fora dela, calcula `sistema:<comando>` somente quando `argv[0]` termina em `manage.py`; em outro processo, ator e usuário ficam nulos (`app/core/observ.py:22`, `app/core/observ.py:108`, `app/core/observ.py:118`). Logo `ator=null/usuario=null` não identifica o chamador; execução histórica concreta dos cinco casos permanece `[nao verificado]`.

**Famílias de produção que podem produzir via sistema com ator/usuário nulos nesse contexto:**

- Disputa fechada encerra chamado pai: `app/chamados/signals.py:28`, `app/chamados/signals.py:42`.
- Batida de entrada/saída, entrada sincronizada ou marco aceso: `app/ponto/services/triagem_batida.py:580`, `app/ponto/services/triagem_batida.py:597`; `app/ponto/services/entrada_sincronizada.py:28`; `app/ponto/services/lastro.py:393`.
- Resposta materializada, destino/clones; ausência decidida absorvendo perguntas: `app/chamados/services/materializacao.py:581`, `app/chamados/services/materializacao.py:761`.
- Ausência lançada/decidida: `app/chamados/services/silencio_ausencia.py:101`; `app/ponto/services/ausencia.py:692`.
- Admin decidiu a célula: `app/ponto/services/veredito_celula.py:188`, `app/ponto/services/veredito_celula.py:202`.
- Retratação/reconciliador e cartório: `app/chamados/reconciliador.py:518`; `app/ponto/services/cartorio.py:974`, `app/ponto/services/cartorio.py:1499`, `app/ponto/services/cartorio.py:1512`, `app/ponto/services/cartorio.py:1533`, `app/ponto/services/cartorio.py:1611`, `app/ponto/services/cartorio.py:1626`.
- Feriado/folga, cadastro e vínculo: `app/chamados/services/feriado_reavaliar.py:33`, `app/chamados/services/feriado_reavaliar.py:47`; `app/chamados/services/limbo_folgas.py:98`, `app/chamados/services/limbo_folgas.py:283`; `app/colaboradores/services/pos_vinculo.py:42`.
- Adesão/agrupamento: `app/chamados/services/adesao.py:112`, `app/chamados/services/adesao.py:144`.
- Cobertura/substituto: `app/ponto/services/cobertura.py:114`, `app/ponto/services/cobertura.py:309`.
- Cluster espúrio: `app/ponto/services/cluster_espurio.py:121`.
- Comandos também chamam o gate: `app/chamados/management/commands/auditar_chamados_orfaos.py:67`; `app/ponto/management/commands/detectar_intervalo_ausente.py:138`; `app/ponto/management/commands/detectar_ausencias.py:158`, `app/ponto/management/commands/detectar_ausencias.py:824`, `app/ponto/management/commands/detectar_ausencias.py:834`, `app/ponto/management/commands/detectar_ausencias.py:845`; `app/ponto/management/commands/curar_residuo_bug115.py:151`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:140`. Na execução atual por `manage.py`, recebem ator de comando; execução que explique ator nulo é `[nao verificado]` (`app/core/observ.py:29`).

**Limite da condição comum.** O gate sistema não exige fato universalmente: chamado fora do universo de furo passa; furo sem dia passa com trilha `fechamento_sem_dia_do_fato`; agrupado sob pai vivo passa; dedup passa com sobrevivente registrado (`app/chamados/models.py:727`, `app/chamados/models.py:732`, `app/chamados/models.py:738`, `app/chamados/models.py:748`). Portanto ator/via não bastam para decidir a condição do caso.

**Guarda atual.** `reconciliar()` passa por premissa e pergunta viva (`app/chamados/models.py:682`, `app/chamados/models.py:685`); a porta reata todas ao sobrevivente legítimo ou recusa (`app/chamados/models.py:410`). O núcleo possui tripwire adicional (`app/chamados/models.py:221`). A guarda alcança perguntas ancoradas no destino ou, sem destino, no pai, julgadas no pós-estado; só competência cuja entrega retorna exatamente `False` participa (`app/chamados/models.py:344`, `app/chamados/models.py:364`, `app/chamados/models.py:368`). Guarda executada nos fechamentos históricos: `[nao verificado]`.

**Dados para prova temporal.** `LogAuditoria` guarda `usuario`, `objeto_id`, antes/depois, IP, ator e `criado_em` (`app/core/models.py:295`). Batida possui hora do fato `timestamp`, chegada `sincronizado_em`, criação `criado_em`, aprovação e retratação; hora do fato anterior ao fechamento, sozinha, não prova que a linha já existia (`app/ponto/models.py:23`, `app/ponto/models.py:67`, `app/ponto/models.py:73`, `app/ponto/models.py:77`). Ausência guarda período, status, origem, aprovação e criação (`app/ponto/models.py:750`, `app/ponto/models.py:786`, `app/ponto/models.py:799`, `app/ponto/models.py:810`). Decisão imutável possui pergunta, ato, `ato_em`, autor, texto/motivo e ligação à ausência (`app/chamados/models.py:3871`, `app/chamados/models.py:3903`). Célula guarda julgamento, veredito, via, instante e ata (`app/escala/models.py:1745`, `app/escala/models.py:1770`); a trilha `veredito_celula` conserva antes/depois do veredito, mas não a ata completa (`app/ponto/portas/celula.py:987`, `app/ponto/portas/celula.py:1042`).

## 11/11 00:17 · sessao ac42788366a1 · - · - · origem=Ronald (tmux)

Referências abaixo relativas à raiz, somente código lido.

**Gravação comum e guarda atual**

- `reconciliar()` decide e grava `via='sistema'` sem passar usuário: `app/chamados/models.py:641`, `app/chamados/models.py:651`, `app/chamados/models.py:654`; mapeamento em `app/chamados/catalogo/motor.py:95`.
- Escritor do `estado_chamado`: `app/chamados/models.py:292`; recebe `ator=user` em `app/chamados/models.py:295`. Fora de request, `core.observ` infere `sistema:<comando>` a partir de `sys.argv` — inclusive `tenant_command` — em `app/core/observ.py:22`, `app/core/observ.py:29`, `app/core/observ.py:31`; dentro de request, herda IP/usuário em `app/core/observ.py:110`, `app/core/observ.py:115`.
- Guarda F1 existe hoje: premissa passa primeiro, seguida de `_gate_pergunta_viva`, em `app/chamados/models.py:682`, `app/chamados/models.py:685`. Sem vivas permite; com vivas, reata todas ou recusa: `app/chamados/models.py:419`, `app/chamados/models.py:422`, `app/chamados/models.py:431`, `app/chamados/models.py:437`.
- Escopo da guarda: perguntas não validadas ancoradas pelo destino ou, sem destino, chamado da disputa (`app/chamados/models.py:344`); competência entregue ou indecidível fica fora (`app/chamados/models.py:364`); C6 é julgado no pós-estado (`app/chamados/models.py:356`, `app/chamados/models.py:368`). Há defesa adicional no escritor antes de terminalizar: `app/chamados/models.py:221`, `app/chamados/models.py:227`. Versão efetivamente executada nos fechamentos dos anexos: `[nao verificado]`.

**18 dias — `sistema:apurar_furos_diarios`**

1. O comando chama `apurar`, `sincronizar_adesao(fechar=True)` e `agrupar_furos_sob_limbo`: `app/escala/management/commands/apurar_furos_diarios.py:13`, `app/escala/management/commands/apurar_furos_diarios.py:17`, `app/escala/management/commands/apurar_furos_diarios.py:19`. A apuração propriamente dita escreve apenas FuroDiario (`app/escala/services/furos_diarios.py:107`, `app/ponto/portas/celula.py:1129`); os fechadores explícitos estão nas duas rotinas seguintes.
2. Caminhos possíveis:
   - Filho de adesão: grava `agrupado_em=pai` e chama `f.reconciliar()`; a condição é agrupamento sob pai de adesão, não demonstração de batida do dia: `app/chamados/services/adesao.py:102`, `app/chamados/services/adesao.py:107`, `app/chamados/services/adesao.py:112`, `app/chamados/services/adesao.py:115`.
   - Pai de adesão: qualquer Batida não retratada do colaborador permite `pai.reconciliar()` quando `fechar=True`, sem recorte de dia na consulta: `app/chamados/services/adesao.py:142`, `app/chamados/services/adesao.py:144`, `app/chamados/services/adesao.py:148`, `app/chamados/services/adesao.py:150`.
   - Furo sob LIMBO: exige pai LIMBO vivo, defeito de folga no vínculo do dia e `pode_calar` (semana inteira sem batida recusa): `app/chamados/services/limbo_folgas.py:302`, `app/chamados/services/limbo_folgas.py:317`, `app/chamados/services/limbo_folgas.py:321`, `app/chamados/services/limbo_folgas.py:324`, `app/chamados/services/limbo_folgas.py:166`. Grava agrupamento, reata perguntas ao pai e tenta reconciliar: `app/chamados/services/limbo_folgas.py:275`, `app/chamados/services/limbo_folgas.py:281`, `app/chamados/services/limbo_folgas.py:283`.
   - O gate comum de furo também aceita premissa regularizada, agrupamento sob pai vivo ou dedup; sem dia forte deixa fechar com trilha específica: `app/chamados/models.py:727`, `app/chamados/models.py:732`, `app/chamados/models.py:738`, `app/chamados/models.py:748`, `app/chamados/models.py:756`.
3. Todos passam pela F1 comum; LIMBO também reata explicitamente antes dela: `app/chamados/models.py:685`, `app/chamados/services/limbo_folgas.py:281`.
4. Para distinguir por dia: `contexto_json.agrupado_em`, pai e estado histórico, notas `agrupado_adesao` / `agrupado_limbo` / `adesao_fechada_por_batida` (`app/chamados/services/adesao.py:118`, `app/chamados/services/adesao.py:152`, `app/chamados/services/limbo_folgas.py:292`); para pai, Batida não retratada preexistente (`app/chamados/services/adesao.py:144`). Batida tem `timestamp`, `criado_em`, `sincronizado_em`, `retratada_em`: `app/ponto/models.py:23`, `app/ponto/models.py:67`, `app/ponto/models.py:77`, `app/ponto/models.py:79`. Só o ator não identifica qual desses caminhos fechou os 18: `[nao verificado]`.

**1 dia — `sistema:reconciliar_chamados`**

1. Na cópia atual o comando não fecha: itera vivos, calcula `_veredito_premissa`, verifica retenção/`reconciliaria` e conta (`app/chamados/management/commands/reconciliar_chamados.py:71`, `app/chamados/management/commands/reconciliar_chamados.py:76`, `app/chamados/management/commands/reconciliar_chamados.py:79`, `app/chamados/management/commands/reconciliar_chamados.py:85`). A documentação registra a conversão em vigia e remoção de `--apply`: `app/chamados/management/commands/reconciliar_chamados.py:16`, `app/chamados/management/commands/reconciliar_chamados.py:29`.
2. O executor preservado fora do comando é `_retratar` → `ch.reconciliar()`, com retenção de irmãs respondidas: `app/chamados/reconciliador.py:503`, `app/chamados/reconciliador.py:518`, `app/chamados/reconciliador.py:523`. Seu chamador só o executa com veredito de premissa morta: `app/chamados/reconciliador.py:1458`, `app/chamados/reconciliador.py:1464`.
3. Coletor atual testa turno aberto, âncora ausente, filhos vivos, par íntegro ou divergência de vínculo conforme classe: `app/chamados/reconciliador.py:262`, `app/chamados/reconciliador.py:264`, `app/chamados/reconciliador.py:290`, `app/chamados/reconciliador.py:292`, `app/chamados/reconciliador.py:294`, `app/chamados/reconciliador.py:296`. “Não há turno aberto” não exige nesse coletor uma batida de saída que fechou par; o código não decide essa equivalência: `app/chamados/reconciliador.py:287`, `app/chamados/reconciliador.py:142`, `app/chamados/reconciliador.py:152`.
4. Prova exigiria módulo/subcategoria/contexto, nota de retratação e respectivos fatos anteriores ao log: motivo transportado em `app/chamados/reconciliador.py:1464`; nota em `app/chamados/reconciliador.py:538`. TurnoMaterializado tem entrada/saída/data/aberto, mas é reconstruído e seu `atualizado_em` não é histórico imutável: `app/ponto/models.py:1201`, `app/ponto/models.py:1205`, `app/ponto/models.py:1206`, `app/ponto/models.py:1208`, `app/ponto/models.py:1210`, `app/ponto/models.py:1213`. Qual versão/sítio produziu o fechamento anexado: `[nao verificado]`.

**1 dia — `sistema:detectar_ausencias`**

1. Fechadores explícitos:
   - LIMBO folga corrigida hoje, sem filho no defeito: `app/ponto/management/commands/detectar_ausencias.py:145`, `app/ponto/management/commands/detectar_ausencias.py:156`, `app/ponto/management/commands/detectar_ausencias.py:158`.
   - Ausência que cobre: `app/ponto/management/commands/detectar_ausencias.py:251`, `app/ponto/management/commands/detectar_ausencias.py:260`, `app/ponto/management/commands/detectar_ausencias.py:346`, `app/ponto/management/commands/detectar_ausencias.py:351`; fechamento em `app/ponto/management/commands/detectar_ausencias.py:824`.
   - Ordem de substituição existente, nos estados designada/em_curso/concluida e intervalo consultado: `app/ponto/management/commands/detectar_ausencias.py:365`, `app/ponto/management/commands/detectar_ausencias.py:368`, `app/ponto/management/commands/detectar_ausencias.py:381`; fechamento em `app/ponto/management/commands/detectar_ausencias.py:834`.
   - Entrada atribuída ao turno do dia **e** `marcos_acesos_do_dia(...) is True`: `app/ponto/management/commands/detectar_ausencias.py:336`, `app/ponto/management/commands/detectar_ausencias.py:403`, `app/ponto/management/commands/detectar_ausencias.py:405`, `app/ponto/management/commands/detectar_ausencias.py:412`; fechamento em `app/ponto/management/commands/detectar_ausencias.py:845`.
2. Esses sítios fecham por condição cadastral/cobertura/batida coletada, não por idade isolada; qual ocorreu no dia do anexo permanece `[nao verificado]`.
3. Todos usam `reconciliar()` e a F1 comum (`app/ponto/management/commands/detectar_ausencias.py:158`, `app/ponto/management/commands/detectar_ausencias.py:824`, `app/ponto/management/commands/detectar_ausencias.py:834`, `app/ponto/management/commands/detectar_ausencias.py:845`, `app/chamados/models.py:685`).
4. Notas permitem localizar o objeto: `fechado_por_afastamento` com ausência PK (`app/ponto/management/commands/detectar_ausencias.py:827`), `fechado_por_cobertura` com ordem PK (`app/ponto/management/commands/detectar_ausencias.py:837`), `fechado_por_batida` com entrada PK (`app/ponto/management/commands/detectar_ausencias.py:848`). Conferir campos temporais de Batida citados acima; Ausencia `criado_em`, `aprovada_em`, status/cobertura (`app/ponto/models.py:799`, `app/ponto/models.py:810`, `app/ponto/models.py:813`); ordem `data_inicio`, `data_fim`, `designada_em`, status (`app/ponto/models.py:1067`, `app/ponto/models.py:1074`, `app/ponto/models.py:1086`). Estado histórico no instante exige trilha, não apenas a linha atual: `app/core/models.py:300`, `app/core/models.py:301`, `app/core/models.py:307`.

**1 dia — `sistema:fechar_chamados_ausencia`**

1. O comando ainda permite escrita com `--apply`, embora sua docstring diga vigia: `app/ponto/management/commands/fechar_chamados_ausencia.py:18`, `app/ponto/management/commands/fechar_chamados_ausencia.py:35`, `app/ponto/management/commands/fechar_chamados_ausencia.py:41`, `app/ponto/management/commands/fechar_chamados_ausencia.py:44`. Chama `fechar_chamados_ausencia_decidida` → `fechar_chamado_da_ausencia` → `ch.reconciliar()`: `app/ponto/services/ausencia.py:729`, `app/ponto/services/ausencia.py:694`.
2. Condição literal: chamado vivo `ausencia_colab`, ligado por `contexto_json.ausencia_id` a Ausencia existente cujo status não pertence a `ABERTAS`; abertas = aguardando_documento/aguardando_decisao: `app/ponto/services/ausencia.py:709`, `app/ponto/services/ausencia.py:714`, `app/ponto/services/ausencia.py:716`, `app/ponto/services/ausencia.py:765`. A condição é decisão/saída das caixas abertas, não idade; não exige status aprovado especificamente (`app/ponto/services/ausencia.py:692`).
3. F1 comum via `reconciliar()`: `app/ponto/services/ausencia.py:694`, `app/chamados/models.py:685`.
4. Conferir ausência ligada, status decidido anterior ao log, `aprovada_em`, `criado_em`, trilha A4 antes/depois e nota `ausencia_decidida`: `app/ponto/models.py:810`, `app/ponto/models.py:813`, `app/ponto/services/ausencia.py:836`, `app/ponto/services/ausencia.py:838`, `app/ponto/services/ausencia.py:697`. O log `estado_chamado.criado_em` fornece o limite temporal: `app/core/models.py:307`.

## 11/11 00:17 · sessao aa01dbd7973a · - · - · origem=Ronald (tmux)

**`via=admin`, usuários #846/#845, IP presente**

- A assinatura é produzida por `transicionar → _aplicar → _gravar_estado`: `app/chamados/models.py:128`, `:199`, `:447-450`, `:292-295`; `TRANSICAO_HUMANA → admin` em `app/chamados/catalogo/motor.py:95`. O IP vem do contexto da requisição: `app/core/observ.py:108-115`.
- O candidato mais direto para **fechar sem texto/motivo/resolução** é a tela de detalhe: `views.py:546-547 → acao_resposta_livre`. Ela lê `conteudo` e `novo_status` separadamente, só cria nota quando há conteúdo, mas admite `gestor + novo_status` com conteúdo vazio: `app/chamados/services/acoes_chamado.py:283-292`, `:313-319`. Esse caminho registra a mudança de status; não exige ato decisório sobre o dia.
- O fio/modal também transiciona com a mesma assinatura, porém esse ramo exige `chamado_id AND conteudo` e grava nota antes: `app/chamados/services/fio.py:708-721`; os campos POST são lidos em `:500-503`.
- Outra tela chama a transição para `fechado` ao rejeitar benefício e anexa nota: `app/colaboradores/views.py:2045-2071`. A pertinência desse caminho aos 18 dias é **[nao verificado]**.
- Existe fechamento **em lote por comando**, `fechar_chamados_meio_periodo`: seleciona superuser e chama `ch.fechar(...resolucao=...)`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:98-112`. Em execução normal por `manage.py`, receberia `ator=sistema:fechar_chamados_meio_periodo`, e não apenas usuário/IP: `app/core/observ.py:22-32`, `:116-123`. A assinatura fornecida não prova que esse comando executou: **[nao verificado]**.
- O lote de arquivo da tela **não fecha**: chama `arquivo.silenciar`, com status inalterado, `app/chamados/views_arquivar.py:140-146`. O lote de validação da tela valida perguntas pela porta de validação: `app/chamados/views_cobrar.py:231-261`.
- Condição do fechamento humano `fechado`: transição permitida, justificativa vinculada não pendente e guardas de disputa/pergunta; não existe a exigência `premissa_regularizada` para esse destino. Essa exigência está exclusivamente no ramo `novo_status == 'resolvido'`: `app/chamados/models.py:132-171`, `:181-198`. Portanto esse sítio admite ação humana sem decisão do dia registrada; se houve fato em cada um dos 18 casos, **o codigo nao decide**.
- Guarda F1: sim, `transicionar` verifica `_vivas_ancoradas` e recusa: `app/chamados/models.py:162-171`. Abrange perguntas não validadas cujo destino é o chamado, ou sem destino cujo pai é o chamado: `:344-347`. Só considera competência comprovadamente aberta (`is False`) e pergunta viva julgada no pós-estado: `:349-369`; ainda há tripwire no escritor: `:219-227`.
- Prova por dia: vincular o log terminal por `objeto_id`, `valor_antes/valor_depois`, `criado_em` (`app/core/models.py:295-307`) a `FollowupChamado` com autor/conteúdo/`criado_em` (`app/chamados/models.py:1668-1674`) e, principalmente, a `DecisaoPergunta` da pergunta/data correta, `ato`, `ato_em`, `ato_por`, motivo/texto (`:3871-3884`). Nota livre, IP e superuser sozinhos não são predicado de regularização nesse caminho: `app/chamados/models.py:181-198`.

**`via=sistema`, usuário #657 superuser, IP presente, `ator=null`; ações vizinhas de ausência**

- `reconciliar()` não passa usuário ao escritor: `app/chamados/models.py:650-654`. O escritor chama `evento(...ator=user)` com `user=None`: `:292-295`. Dentro de request, `evento` herda `request.user` e IP, deixando `ator_sistema=None`: `app/core/observ.py:108-123`. Logo essa assinatura é compatível com fechamento automático disparado dentro de ação humana; não significa que o usuário chamou a transição humana/admin.
- O encadeamento que produz precisamente as ações vizinhas é `reconciliar_por_ausencia`: primeiro `ch.reconciliar()`, depois nota de lastro, levantamento opcional do silêncio e `reconciliar_ausencia_lancada`: `app/chamados/services/silencio_ausencia.py:100-110`. O levantamento usa ação `levantar_silencio_ausencia`: `:68-75`. A origem concreta dos três dias permanece **[nao verificado]**, pois “vizinhas” não traz identificadores/ordem temporal completos.
- Esse serviço pode executar em evento de gravação de ausência: `post_save → _consumidor_ausencia → transaction.on_commit → reconciliar_colab`, `app/ponto/signals.py:250-279`; o reconciliador percorre os chamados vivos do colaborador: `app/chamados/services/silencio_ausencia.py:119-129`.
- Condição: ausência coberta encontrada por `ausencia_cobre(...para='cobranca')`, status **aprovada**, premissa que morre e célula sem veredito acusatório (`justificativa` retorna verdadeiro diretamente): `app/chamados/services/silencio_ausencia.py:28-41`, `:60-65`, `:93-102`. Ausência aberta silencia, não fecha: `:77-92`.
- A porta ainda exige o gate da premissa e a F1 antes de gravar: `app/chamados/models.py:682-689`. A F1 pode reatar as perguntas ao sobrevivente válido do mesmo colaborador/dia ou recusar o fechamento: `:374-394`, `:410-445`. Limite de competência aberta/pós-estado em `:349-369`.
- Prova por dia: `reconciliar_ausencia_lancada.valor_depois` traz `data`, `ausencia_id`, `ausencia_tipo`: `app/chamados/services/silencio_ausencia.py:44-48`, `:108-110`. Conferir a ausência vinculada, colaborador, cobertura `data/data_fim/hora_ini/hora_fim` (`app/ponto/models.py:743-771`), `status`, `aprovado_por`, `aprovada_em`, `criado_em` (`:799-814`) e carimbo anterior ao log terminal (`app/core/models.py:307`). O modelo auto-carimba `aprovada_em` para estado decisório sem carimbo: `app/ponto/models.py:868-873`. O valor atual da célula não prova seu valor no instante histórico; exige trilha anterior correspondente **[nao verificado nos anexos]**.

**Outro sítio em evento de ausência capaz de produzir `via=sistema + usuário/IP + ator null`**

- O mesmo consumidor chama `fechar_chamados_ausencia_decidida(aus)`: `app/ponto/signals.py:266-271`. A porta `fechar_chamado_da_ausencia` encerra `ausencia_colab` quando a ausência existe e saiu de `ABERTAS`; pode ser aprovada **ou rejeitada**, pois testa apenas “não aberta”, antes de `ch.reconciliar()`: `app/ponto/services/ausencia.py:686-700`. Não equivale necessariamente a ausência abonando o dia; o fato é a decisão da solicitação. F1 e gate comum são os mesmos acima: `app/chamados/models.py:682-689`.

## 11/11 00:14 · sessao 8af21c2e7cb6 · FIO-E0-V4-E4B · sonda · origem=Code

Li integralmente o bloco fornecido: **45 linhas de dia**, conforme esperado; soma de `quantidade_vivas` = **131**. Há **43 chamados donos distintos exibidos**. Todas as linhas diárias têm classe `indeterminado` e `erros:[]`. [nao verificado]

O resumo confere: `10/2026/#2=1`, `10/2026/#4=2`, `11/2026/#2=35`, `11/2026/#3=7`; classes `a` e `b` com zero. Das 131 perguntas, **85 estão exibidas e 46 foram omitidas das amostras**. [nao verificado]

Abaixo, o status está transcrito como `depois_status` de `travessia_log`; `acoes_distintas` preserva integralmente a representação recebida, inclusive objetos `n/amostra`. A fonte é o texto colado, sem arquivo:linha, indicada em cada JSON como `[nao verificado]`. [nao verificado]

```jsonl
{"col":"col114","dia":"2026-11-03","quantidade_vivas":2,"donos":[{"chamado":"#36791","travessia_log":{"log":"#894348","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:apurar_furos_diarios","acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":{"n":4,"amostra":["estado_chamado@sistema:apurar_furos_diarios","estado_chamado@sistema:processar_alertas_turno"]}}],"fonte":"[nao verificado]"}
{"col":"col114","dia":"2026-11-06","quantidade_vivas":2,"donos":[{"chamado":"#37417","travessia_log":{"log":"#912715","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:apurar_furos_diarios","acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":{"n":4,"amostra":["estado_chamado@sistema:apurar_furos_diarios","estado_chamado@sistema:processar_alertas_turno"]}}],"fonte":"[nao verificado]"}
{"col":"col209","dia":"2026-10-22","quantidade_vivas":4,"donos":[{"chamado":"#33853","travessia_log":{"log":"#750302","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:apurar_furos_diarios","acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@sistema:apurar_furos_diarios"]}],"fonte":"[nao verificado]"}
{"col":"col209","dia":"2026-10-24","quantidade_vivas":4,"donos":[{"chamado":"#34481","travessia_log":{"log":"#762388","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:apurar_furos_diarios","acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@sistema:apurar_furos_diarios"]}],"fonte":"[nao verificado]"}
{"col":"col258","dia":"2026-10-26","quantidade_vivas":1,"donos":[{"chamado":"#35126","travessia_log":{"log":"#783748","depois_status":"resolvido","via":"sistema","usuario":"#657","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":{"n":6,"amostra":["estado_chamado@None","levantar_silencio_ausencia@None"]}}],"fonte":"[nao verificado]"}
{"col":"col267","dia":"2026-10-24","quantidade_vivas":4,"donos":[{"chamado":"#34447","travessia_log":{"log":"#778802","depois_status":"fechado","via":"admin","usuario":"#846","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@None","reabertura_recusada@None","reabertura_recusada@sistema:processar_cartorio"]}],"fonte":"[nao verificado]"}
{"col":"col268","dia":"2026-10-22","quantidade_vivas":4,"donos":[{"chamado":"#33500","travessia_log":{"log":"#750303","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:apurar_furos_diarios","acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@sistema:apurar_furos_diarios"]}],"fonte":"[nao verificado]"}
{"col":"col268","dia":"2026-10-24","quantidade_vivas":4,"donos":[{"chamado":"#34427","travessia_log":{"log":"#762389","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:apurar_furos_diarios","acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@sistema:apurar_furos_diarios"]}],"fonte":"[nao verificado]"}
{"col":"col298","dia":"2026-10-21","quantidade_vivas":2,"donos":[{"chamado":"#33234","travessia_log":{"log":"#738984","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:apurar_furos_diarios","acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@sistema:apurar_furos_diarios"]}],"fonte":"[nao verificado]"}
{"col":"col355","dia":"2026-10-25","quantidade_vivas":2,"donos":[{"chamado":"#34762","travessia_log":{"log":"#779377","depois_status":"fechado","via":"admin","usuario":"#846","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":{"n":6,"amostra":["estado_chamado@None","estado_chamado@sistema:vigia_de_hora"]}},{"chamado":"#8591","travessia_log":{"log":"#793079","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:reconciliar_chamados","acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["chamado_escalonado_dp@sistema:escalonar_chamados_supervisao","estado_chamado@sistema:reabrir_orfas_enterradas","estado_chamado@sistema:reconciliar_chamados"]}],"fonte":"[nao verificado]"}
{"col":"col427","dia":"2026-10-22","quantidade_vivas":4,"donos":[{"chamado":"#33928","travessia_log":{"log":"#750307","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:apurar_furos_diarios","acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@sistema:apurar_furos_diarios"]}],"fonte":"[nao verificado]"}
{"col":"col427","dia":"2026-10-24","quantidade_vivas":4,"donos":[{"chamado":"#34539","travessia_log":{"log":"#762392","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:apurar_furos_diarios","acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@sistema:apurar_furos_diarios"]}],"fonte":"[nao verificado]"}
{"col":"col427","dia":"2026-10-26","quantidade_vivas":4,"donos":[{"chamado":"#35091","travessia_log":{"log":"#774741","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:apurar_furos_diarios","acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@sistema:apurar_furos_diarios","nascimento_chamado@sistema:detectar_ausencias"]}],"fonte":"[nao verificado]"}
{"col":"col534","dia":"2026-10-23","quantidade_vivas":4,"donos":[{"chamado":"#34117","travessia_log":{"log":"#758132","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:apurar_furos_diarios","acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@sistema:apurar_furos_diarios"]}],"fonte":"[nao verificado]"}
{"col":"col534","dia":"2026-10-24","quantidade_vivas":4,"donos":[{"chamado":"#34439","travessia_log":{"log":"#762393","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:apurar_furos_diarios","acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@sistema:apurar_furos_diarios"]}],"fonte":"[nao verificado]"}
{"col":"col534","dia":"2026-10-25","quantidade_vivas":4,"donos":[{"chamado":"#34753","travessia_log":{"log":"#766572","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:apurar_furos_diarios","acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@sistema:apurar_furos_diarios"]}],"fonte":"[nao verificado]"}
{"col":"col650","dia":"2026-10-23","quantidade_vivas":4,"donos":[{"chamado":"#34098","travessia_log":{"log":"#758133","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:apurar_furos_diarios","acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@sistema:apurar_furos_diarios"]}],"fonte":"[nao verificado]"}
{"col":"col650","dia":"2026-10-25","quantidade_vivas":4,"donos":[{"chamado":"#34739","travessia_log":{"log":"#766573","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:apurar_furos_diarios","acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@sistema:apurar_furos_diarios"]}],"fonte":"[nao verificado]"}
{"col":"col718","dia":"2026-10-22","quantidade_vivas":2,"donos":[{"chamado":"#33930","travessia_log":{"log":"#792218","depois_status":"fechado","via":"admin","usuario":"#846","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@None","reabertura_recusada@None","reabertura_recusada@sistema:processar_cartorio"]}],"fonte":"[nao verificado]"}
{"col":"col718","dia":"2026-10-24","quantidade_vivas":2,"donos":[{"chamado":"#34540","travessia_log":{"log":"#792222","depois_status":"fechado","via":"admin","usuario":"#846","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@None","reabertura_recusada@None","reabertura_recusada@sistema:processar_cartorio"]}],"fonte":"[nao verificado]"}
{"col":"col718","dia":"2026-10-26","quantidade_vivas":2,"donos":[{"chamado":"#35092","travessia_log":{"log":"#792225","depois_status":"fechado","via":"admin","usuario":"#846","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":{"n":5,"amostra":["estado_chamado@None","nascimento_chamado@sistema:detectar_ausencias"]}}],"fonte":"[nao verificado]"}
{"col":"col718","dia":"2026-10-28","quantidade_vivas":2,"donos":[{"chamado":"#35540","travessia_log":{"log":"#792228","depois_status":"fechado","via":"admin","usuario":"#846","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":{"n":4,"amostra":["estado_chamado@None","nascimento_chamado@sistema:detectar_ausencias"]}}],"fonte":"[nao verificado]"}
{"col":"col723","dia":"2026-10-28","quantidade_vivas":4,"donos":[{"chamado":"#35427","travessia_log":{"log":"#792259","depois_status":"fechado","via":"admin","usuario":"#846","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":{"n":7,"amostra":["estado_chamado@None","estado_chamado@sistema:processar_cartorio"]}}],"fonte":"[nao verificado]"}
{"col":"col865","dia":"2026-10-23","quantidade_vivas":1,"donos":[{"chamado":"#34385","travessia_log":{"log":"#779064","depois_status":"fechado","via":"admin","usuario":"#846","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@None","reabertura_recusada@None","reabertura_recusada@sistema:processar_cartorio"]}],"fonte":"[nao verificado]"}
{"col":"col900","dia":"2026-11-01","quantidade_vivas":4,"donos":[{"chamado":"#36289","travessia_log":{"log":"#837155","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:detectar_ausencias","acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@sistema:detectar_ausencias","nascimento_chamado@sistema:detectar_ausencias"]},{"chamado":"#19808","travessia_log":{"log":"#366307","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":null,"acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@None"]}],"fonte":"[nao verificado]"}
{"col":"col900","dia":"2026-11-03","quantidade_vivas":4,"donos":[{"chamado":"#19808","travessia_log":{"log":"#366307","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":null,"acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@None"]}],"fonte":"[nao verificado]"}
{"col":"col900","dia":"2026-11-05","quantidade_vivas":4,"donos":[{"chamado":"#19808","travessia_log":{"log":"#366307","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":null,"acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@None"]}],"fonte":"[nao verificado]"}
{"col":"col900","dia":"2026-11-07","quantidade_vivas":4,"donos":[{"chamado":"#19808","travessia_log":{"log":"#366307","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":null,"acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@None"]}],"fonte":"[nao verificado]"}
{"col":"col900","dia":"2026-11-09","quantidade_vivas":4,"donos":[{"chamado":"#19808","travessia_log":{"log":"#366307","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":null,"acao_bruta":"estado_chamado","ip_presente":false,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@None"]}],"fonte":"[nao verificado]"}
{"col":"col918","dia":"2026-10-25","quantidade_vivas":2,"donos":[{"chamado":"#34771","travessia_log":{"log":"#785855","depois_status":"fechado","via":"admin","usuario":"#845","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@None","reabertura_recusada@None","reabertura_recusada@sistema:processar_cartorio"]}],"fonte":"[nao verificado]"}
{"col":"col921","dia":"2026-10-21","quantidade_vivas":2,"donos":[{"chamado":"#33232","travessia_log":{"log":"#778977","depois_status":"fechado","via":"admin","usuario":"#846","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@None","reabertura_recusada@None","reabertura_recusada@sistema:processar_cartorio"]}],"fonte":"[nao verificado]"}
{"col":"col921","dia":"2026-10-22","quantidade_vivas":2,"donos":[{"chamado":"#33882","travessia_log":{"log":"#778980","depois_status":"fechado","via":"admin","usuario":"#846","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@None","reabertura_recusada@None","reabertura_recusada@sistema:processar_cartorio"]}],"fonte":"[nao verificado]"}
{"col":"col921","dia":"2026-10-23","quantidade_vivas":2,"donos":[{"chamado":"#34182","travessia_log":{"log":"#778983","depois_status":"fechado","via":"admin","usuario":"#846","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@None","reabertura_recusada@None","reabertura_recusada@sistema:processar_cartorio"]}],"fonte":"[nao verificado]"}
{"col":"col921","dia":"2026-10-25","quantidade_vivas":2,"donos":[{"chamado":"#34815","travessia_log":{"log":"#778986","depois_status":"fechado","via":"admin","usuario":"#846","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@None","reabertura_recusada@None","reabertura_recusada@sistema:processar_cartorio"]}],"fonte":"[nao verificado]"}
{"col":"col921","dia":"2026-10-26","quantidade_vivas":2,"donos":[{"chamado":"#35032","travessia_log":{"log":"#778989","depois_status":"fechado","via":"admin","usuario":"#846","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@None","reabertura_recusada@None","reabertura_recusada@sistema:processar_cartorio"]}],"fonte":"[nao verificado]"}
{"col":"col921","dia":"2026-10-27","quantidade_vivas":2,"donos":[{"chamado":"#35280","travessia_log":{"log":"#801831","depois_status":"fechado","via":"admin","usuario":"#846","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":{"n":4,"amostra":["estado_chamado@None","nascimento_chamado@sistema:detectar_ausencias"]}}],"fonte":"[nao verificado]"}
{"col":"col921","dia":"2026-10-28","quantidade_vivas":2,"donos":[{"chamado":"#35490","travessia_log":{"log":"#801834","depois_status":"fechado","via":"admin","usuario":"#846","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":{"n":4,"amostra":["estado_chamado@None","nascimento_chamado@sistema:vigia_de_hora"]}}],"fonte":"[nao verificado]"}
{"col":"col921","dia":"2026-10-29","quantidade_vivas":2,"donos":[{"chamado":"#35756","travessia_log":{"log":"#801837","depois_status":"fechado","via":"admin","usuario":"#846","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":{"n":4,"amostra":["estado_chamado@None","nascimento_chamado@sistema:vigia_de_hora"]}}],"fonte":"[nao verificado]"}
{"col":"col923","dia":"2026-10-25","quantidade_vivas":4,"donos":[{"chamado":"#34888","travessia_log":{"log":"#778631","depois_status":"fechado","via":"admin","usuario":"#846","usuario_perfil":{"superuser":true,"staff":true,"conta_de_sistema":false},"ator":null,"acao_bruta":"estado_chamado","ip_presente":true,"motivo_presente":false,"resolucao_presente":false},"acoes_distintas":["estado_chamado@None","reabertura_recusada@None","reabertura_recusada@sistema:processar_cartorio"]}],"fonte":"[nao verificado]"}
{"col":"col1005","dia":"2026-10-28","quantidade_vivas":4,"donos":[{"chamado":"#35423","travessia_log":{"log":"#788507","depois_status":"resolvido","via":"sistema","usuario":null,"usuario_perfil":null,"ator":"sistema:

[truncado em 20000 de 24941 caracteres]

## 11/11 00:11 · sessao da9cc878c659 · - · - · origem=Ronald (tmux)

Conferência integral do texto colado: **45 linhas de dia**, **131 perguntas em `quantidade_vivas`**; 5 dias com quantidade 1, 17 com quantidade 2 e 23 com quantidade 4. Há 43 donos distintos visíveis e 47 ocorrências de dono por dia. [nao verificado]

| # | col | dia | quantidade_vivas | donos visíveis |
|---:|---|---|---:|---|
| 1 | col114 | 2026-11-03 | 2 | #36791 |
| 2 | col114 | 2026-11-06 | 2 | #37417 |
| 3 | col209 | 2026-10-22 | 4 | #33853 |
| 4 | col209 | 2026-10-24 | 4 | #34481 |
| 5 | col258 | 2026-10-26 | 1 | #35126 |
| 6 | col267 | 2026-10-24 | 4 | #34447 |
| 7 | col268 | 2026-10-22 | 4 | #33500 |
| 8 | col268 | 2026-10-24 | 4 | #34427 |
| 9 | col298 | 2026-10-21 | 2 | #33234 |
| 10 | col355 | 2026-10-25 | 2 | #34762, #8591 |
| 11 | col427 | 2026-10-22 | 4 | #33928 |
| 12 | col427 | 2026-10-24 | 4 | #34539 |
| 13 | col427 | 2026-10-26 | 4 | #35091 |
| 14 | col534 | 2026-10-23 | 4 | #34117 |
| 15 | col534 | 2026-10-24 | 4 | #34439 |
| 16 | col534 | 2026-10-25 | 4 | #34753 |
| 17 | col650 | 2026-10-23 | 4 | #34098 |
| 18 | col650 | 2026-10-25 | 4 | #34739 |
| 19 | col718 | 2026-10-22 | 2 | #33930 |
| 20 | col718 | 2026-10-24 | 2 | #34540 |
| 21 | col718 | 2026-10-26 | 2 | #35092 |
| 22 | col718 | 2026-10-28 | 2 | #35540 |
| 23 | col723 | 2026-10-28 | 4 | #35427 |
| 24 | col865 | 2026-10-23 | 1 | #34385 |
| 25 | col900 | 2026-11-01 | 4 | #36289, #19808 |
| 26 | col900 | 2026-11-03 | 4 | #19808 |
| 27 | col900 | 2026-11-05 | 4 | #19808 |
| 28 | col900 | 2026-11-07 | 4 | #19808 |
| 29 | col900 | 2026-11-09 | 4 | #19808 |
| 30 | col918 | 2026-10-25 | 2 | #34771 |
| 31 | col921 | 2026-10-21 | 2 | #33232 |
| 32 | col921 | 2026-10-22 | 2 | #33882 |
| 33 | col921 | 2026-10-23 | 2 | #34182 |
| 34 | col921 | 2026-10-25 | 2 | #34815 |
| 35 | col921 | 2026-10-26 | 2 | #35032 |
| 36 | col921 | 2026-10-27 | 2 | #35280 |
| 37 | col921 | 2026-10-28 | 2 | #35490 |
| 38 | col921 | 2026-10-29 | 2 | #35756 |
| 39 | col923 | 2026-10-25 | 4 | #34888 |
| 40 | col1005 | 2026-10-28 | 4 | #35423 |
| 41 | col1005 | 2026-10-29 | 4 | #35679 |
| 42 | col1005 | 2026-10-30 | 4 | #35889 |
| 43 | col42 | 2026-10-08 | 1 | #29676 |
| 44 | col42 | 2026-10-15 | 1 | #31699 |
| 45 | col258 | 2026-10-02 | 1 | #28094 |

Toda a tabela provém do prompt, sem arquivo:linha: [nao verificado].

O resumo confere: `10/2026/#2=1`, `10/2026/#4=2`, `11/2026/#2=35`, `11/2026/#3=7`; soma 45. Todas essas contagens estão em `indeterminado`; `a` e `b` têm zero. [nao verificado]

Nenhuma linha de dia tem classe `ERRO-NAO-MEDIDO`; todas exibem `erros:[]`. As quatro linhas de resumo dessa classe são: [nao verificado]

```json
{"resumo":"dias","classe":"ERRO-NAO-MEDIDO","comp_anexo":"10/2026","empresa_anexo":"#2","n":0,"n_parcial_conhecido":0,"integral":true,"resultado":"MEDIDO"}
{"resumo":"dias","classe":"ERRO-NAO-MEDIDO","comp_anexo":"10/2026","empresa_anexo":"#4","n":0,"n_parcial_conhecido":0,"integral":true,"resultado":"MEDIDO"}
{"resumo":"dias","classe":"ERRO-NAO-MEDIDO","comp_anexo":"11/2026","empresa_anexo":"#2","n":0,"n_parcial_conhecido":0,"integral":true,"resultado":"MEDIDO"}
{"resumo":"dias","classe":"ERRO-NAO-MEDIDO","comp_anexo":"11/2026","empresa_anexo":"#3","n":0,"n_parcial_conhecido":0,"integral":true,"resultado":"MEDIDO"}
```

Limite para agregação: 23 linhas usam `vivas:{"n":4,"amostra":[duas perguntas]}`. São 85 perguntas explicitamente exibidas e **46 perguntas omitidas**, cujos donos e grupos não podem ser atribuídos pela saída. [nao verificado]

| via | ator | perfil (superuser,staff,conta_de_sistema) | IP | dias com grupo visível | perguntas exibidas |
|---|---|---|---|---:|---:|
| sistema | sistema:apurar_furos_diarios | null | false | 18 | 36 |
| admin | null | true,true,false | true | 18 | 34 |
| sistema | null | true,true,false | true | 3 | 3 |
| sistema | sistema:reconciliar_chamados | null | false | 1 | 1 |
| sistema | sistema:detectar_ausencias | null | false | 1 | 1 |
| sistema | null | null | false | 5 | 9 |
| sistema | sistema:fechar_chamados_ausencia | null | false | 1 | 1 |

Tabela de conferência baseada exclusivamente no prompt: [nao verificado].

A soma de `quantidade_vivas` das linhas que **contêm** cada grupo visível, sem repartir o dia entre donos, é respectivamente **66, 41, 3, 2, 4, 20, 1**. Esses valores se sobrepõem: col355/2026-10-25 contém admin e reconciliar; col900/2026-11-01 contém detectar e ator null. Não representam uma partição das 131 perguntas. [nao verificado]

## 11/11 00:11 · sessao 459709e686e8 · - · - · origem=Ronald (tmux)

Conferência do dado colado: **45 dias-colaborador**, `sum(quantidade_vivas)=131`, **85 perguntas exibidas**, **46 não exibidas** em 23 linhas com `vivas={"n":4,"amostra":[duas perguntas]}`. [nao verificado]

| via | ator | usuario_perfil | ip_presente | Dias com ocorrência exibida | Perguntas exibidas |
|---|---|---|---|---:|---:|
| sistema | sistema:apurar_furos_diarios | null | false | 18 | 36 |
| sistema | null | `{superuser:true,staff:true,conta_de_sistema:false}` | true | 3 | 3 |
| admin | null | `{superuser:true,staff:true,conta_de_sistema:false}` | true | 18 | 34 |
| sistema | sistema:reconciliar_chamados | null | false | 1 | 1 |
| sistema | sistema:detectar_ausencias | null | false | 1 | 1 |
| sistema | null | null | false | 5 | 9 |
| sistema | sistema:fechar_chamados_ausencia | null | false | 1 | 1 |

Todas as células da tabela são provenientes exclusivamente do prompt. [nao verificado]

Há 47 incidências de grupo em 45 dias distintos: `col355/2026-10-25` contém admin + reconciliar; `col900/2026-11-01` contém detectar + ator null. [nao verificado]

A soma integral de `quantidade_vivas`, agrupada **pelos conjuntos de grupos exibidos no dia**, sem atribuir perguntas ocultas, é: [nao verificado]

| Grupos exibidos | Dias | Soma quantidade_vivas |
|---|---:|---:|
| apurar | 18 | 66 |
| sistema/null/perfil superuser/IP true | 3 | 3 |
| admin | 17 | 39 |
| admin + reconciliar | 1 | 2 |
| detectar + sistema/null/perfil null/IP false | 1 | 4 |
| sistema/null/perfil null/IP false | 4 | 16 |
| fechar_chamados_ausencia | 1 | 1 |
| Total | 45 | 131 |

Atribuir as 46 perguntas não exibidas a grupos individuais permanece [nao verificado]. A amostra de `col900/2026-11-01` já contém dois grupos distintos; não permite repartir suas quatro perguntas. [nao verificado]

Nenhuma linha diária tem classe `ERRO-NAO-MEDIDO`; as quatro linhas de resumo dessa classe têm `n=0`, `n_parcial_conhecido=0`, `integral=true`, `resultado="MEDIDO"`. [nao verificado]

## 11/11 00:08 · sessao 029ce9b0e625 · FIO-E0-V4-E4B · sonda · origem=Code

1) Resumo, transcrito como fornecido. A fonte é o bloco colado na mensagem, sem referência `arquivo:linha`; por isso, as transcrições e contagens recebem `[nao verificado]`.

- `{"resumo": "dias", "classe": "a", "comp_anexo": "10/2026", "empresa_anexo": "#2", "n": 0, "n_parcial_conhecido": 0, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "b", "comp_anexo": "10/2026", "empresa_anexo": "#2", "n": 0, "n_parcial_conhecido": 0, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "indeterminado", "comp_anexo": "10/2026", "empresa_anexo": "#2", "n": 1, "n_parcial_conhecido": 1, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "ERRO-NAO-MEDIDO", "comp_anexo": "10/2026", "empresa_anexo": "#2", "n": 0, "n_parcial_conhecido": 0, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "a", "comp_anexo": "10/2026", "empresa_anexo": "#4", "n": 0, "n_parcial_conhecido": 0, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "b", "comp_anexo": "10/2026", "empresa_anexo": "#4", "n": 0, "n_parcial_conhecido": 0, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "indeterminado", "comp_anexo": "10/2026", "empresa_anexo": "#4", "n": 2, "n_parcial_conhecido": 2, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "ERRO-NAO-MEDIDO", "comp_anexo": "10/2026", "empresa_anexo": "#4", "n": 0, "n_parcial_conhecido": 0, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "a", "comp_anexo": "11/2026", "empresa_anexo": "#2", "n": 0, "n_parcial_conhecido": 0, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "b", "comp_anexo": "11/2026", "empresa_anexo": "#2", "n": 0, "n_parcial_conhecido": 0, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "indeterminado", "comp_anexo": "11/2026", "empresa_anexo": "#2", "n": 35, "n_parcial_conhecido": 35, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "ERRO-NAO-MEDIDO", "comp_anexo": "11/2026", "empresa_anexo": "#2", "n": 0, "n_parcial_conhecido": 0, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "a", "comp_anexo": "11/2026", "empresa_anexo": "#3", "n": 0, "n_parcial_conhecido": 0, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "b", "comp_anexo": "11/2026", "empresa_anexo": "#3", "n": 0, "n_parcial_conhecido": 0, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "indeterminado", "comp_anexo": "11/2026", "empresa_anexo": "#3", "n": 7, "n_parcial_conhecido": 7, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "ERRO-NAO-MEDIDO", "comp_anexo": "11/2026", "empresa_anexo": "#3", "n": 0, "n_parcial_conhecido": 0, "integral": true, "resultado": "MEDIDO"}` [nao verificado]

2) Li integralmente até `FIM DO DADO`: **45 linhas de dia**, numeradas abaixo na ordem recebida. Há **47 combinações dia × dono exibidas**, pois os dias 10 e 25 mostram dois donos cada. Outros donos eventualmente existentes nas perguntas omitidas estão `[nao verificado]`. [nao verificado]

Convenções da tabela:

- `NV` significa `[nao verificado]`: o log identificado por `historico.travessia` não aparece na amostra. Não significa `null` nem `false`. `Ator/M/R` transcreve, nessa ordem, `ator_processo_presente`, `motivo_presente` e `resolucao_presente` **desse log**. [nao verificado]
- `Q vis./dia`: perguntas efetivamente exibidas para aquele dono / `quantidade_vivas` do dia. O denominador pertence ao dia, inclusive quando há dois donos. `Logs vis./n`: quantidade de logs exibidos / quantidade informada. `Dec./Notas`: contagens de `decisoes_vinculos_atuais` / `notas`, sem multiplicar pela repetição do dono nas perguntas. [nao verificado]
- Ações: `E` = `estado_chamado`; `N` = `nascimento_chamado`; `O` = `OUTRO-NAO-EXIBIDO`; `R` = `reconciliar_ausencia_lancada`. A coluna reúne os nomes distintos **exibidos**, inclusive nas amostras. [nao verificado]

| Nº | Colaborador | Dia | Chamado dono | Status | Via | Log da travessia | Usuário desse log | Ator/M/R | Dec./Notas | Q vis./dia | Logs vis./n | Ações exibidas | Fonte |
|---:|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | col114 | 2026-11-03 | #36791 | resolvido | sistema | #894348 | NV | NV/NV/NV | 0/2 | 2/2 | 2/4 | N, E | [nao verificado] |
| 2 | col114 | 2026-11-06 | #37417 | resolvido | sistema | #912715 | NV | NV/NV/NV | 0/2 | 2/2 | 2/4 | N, E | [nao verificado] |
| 3 | col209 | 2026-10-22 | #33853 | resolvido | sistema | #750302 | null | true/false/false | 0/2 | 2/4 | 1/1 | E | [nao verificado] |
| 4 | col209 | 2026-10-24 | #34481 | resolvido | sistema | #762388 | null | true/false/false | 0/2 | 2/4 | 1/1 | E | [nao verificado] |
| 5 | col258 | 2026-10-26 | #35126 | resolvido | sistema | #783748 | NV | NV/NV/NV | 0/1 | 1/1 | 2/8 | N, O | [nao verificado] |
| 6 | col267 | 2026-10-24 | #34447 | fechado | admin | #778802 | #846 | false/false/false | 0/2 | 2/4 | 2/4 | E, O | [nao verificado] |
| 7 | col268 | 2026-10-22 | #33500 | resolvido | sistema | #750303 | null | true/false/false | 0/2 | 2/4 | 1/1 | E | [nao verificado] |
| 8 | col268 | 2026-10-24 | #34427 | resolvido | sistema | #762389 | null | true/false/false | 0/2 | 2/4 | 1/1 | E | [nao verificado] |
| 9 | col298 | 2026-10-21 | #33234 | resolvido | sistema | #738984 | null | true/false/false | 0/2 | 2/2 | 1/1 | E | [nao verificado] |
| 10 | col355 | 2026-10-25 | #34762 | fechado | admin | #779377 | NV | NV/NV/NV | 0/6 | 1/2 | 2/684 | E | [nao verificado] |
| 10 | col355 | 2026-10-25 | #8591 | resolvido | sistema | #793079 | null | true/false/false | 0/8 | 1/2 | 3/3 | E, O | [nao verificado] |
| 11 | col427 | 2026-10-22 | #33928 | resolvido | sistema | #750307 | null | true/false/false | 0/2 | 2/4 | 1/1 | E | [nao verificado] |
| 12 | col427 | 2026-10-24 | #34539 | resolvido | sistema | #762392 | null | true/false/false | 0/2 | 2/4 | 1/1 | E | [nao verificado] |
| 13 | col427 | 2026-10-26 | #35091 | resolvido | sistema | #774741 | null | true/false/false | 0/2 | 2/4 | 2/2 | N, E | [nao verificado] |
| 14 | col534 | 2026-10-23 | #34117 | resolvido | sistema | #758132 | null | true/false/false | 0/2 | 2/4 | 1/1 | E | [nao verificado] |
| 15 | col534 | 2026-10-24 | #34439 | resolvido | sistema | #762393 | null | true/false/false | 0/2 | 2/4 | 1/1 | E | [nao verificado] |
| 16 | col534 | 2026-10-25 | #34753 | resolvido | sistema | #766572 | null | true/false/false | 0/2 | 2/4 | 1/1 | E | [nao verificado] |
| 17 | col650 | 2026-10-23 | #34098 | resolvido | sistema | #758133 | null | true/false/false | 0/2 | 2/4 | 1/1 | E | [nao verificado] |
| 18 | col650 | 2026-10-25 | #34739 | resolvido | sistema | #766573 | null | true/false/false | 0/2 | 2/4 | 1/1 | E | [nao verificado] |
| 19 | col718 | 2026-10-22 | #33930 | fechado | admin | #792218 | #846 | false/false/false | 0/2 | 2/2 | 3/3 | E, O | [nao verificado] |
| 20 | col718 | 2026-10-24 | #34540 | fechado | admin | #792222 | #846 | false/false/false | 0/2 | 2/2 | 3/3 | E, O | [nao verificado] |
| 21 | col718 | 2026-10-26 | #35092 | fechado | admin | #792225 | NV | NV/NV/NV | 0/2 | 2/2 | 2/5 | N, O | [nao verificado] |
| 22 | col718 | 2026-10-28 | #35540 | fechado | admin | #792228 | #846 | false/false/false | 0/2 | 2/2 | 2/4 | N, E | [nao verificado] |
| 23 | col723 | 2026-10-28 | #35427 | fechado | admin | #792259 | NV | NV/NV/NV | 0/7 | 2/4 | 2/12 | N, E | [nao verificado] |
| 24 | col865 | 2026-10-23 | #34385 | fechado | admin | #779064 | #846 | false/false/false | 0/2 | 1/1 | 2/4 | E, O | [nao verificado] |
| 25 | col900 | 2026-11-01 | #36289 | resolvido | sistema | #837155 | null | true/false/false | 0/1 | 1/4 | 2/2 | N, E | [nao verificado] |
| 25 | col900 | 2026-11-01 | #19808 | resolvido | sistema | #366307 | null | false/false/false | 0/2 | 1/4 | 1/1 | E | [nao verificado] |
| 26 | col900 | 2026-11-03 | #19808 | resolvido | sistema | #366307 | null | false/false/false | 0/2 | 2/4 | 1/1 | E | [nao verificado] |
| 27 | col900 | 2026-11-05 | #19808 | resolvido | sistema | #366307 | null | false/false/false | 0/2 | 2/4 | 1/1 | E | [nao verificado] |
| 28 | col900 | 2026-11-07 | #19808 | resolvido | sistema | #366307 | null | false/false/false | 0/2 | 2/4 | 1/1 | E | [nao verificado] |
| 29 | col900 | 2026-11-09 | #19808 | resolvido | sistema | #366307 | null | false/false/false | 0/2 | 2/4 | 1/1 | E | [nao verificado] |
| 30 | col918 | 2026-10-25 | #34771 | fechado | admin | #785855 | #845 | false/false/false | 0/2 | 2/2 | 2/4 | E, O | [nao verificado] |
| 31 | col921 | 2026-10-21 | #33232 | fechado | admin | #778977 | #846 | false/false/false | 0/2 | 2/2 | 2/4 | E, O | [nao verificado] |
| 32 | col921 | 2026-10-22 | #33882 | fechado | admin | #778980 | #846 | false/false/false | 0/2 | 2/2 | 2/4 | E, O | [nao verificado] |
| 33 | col921 | 2026-10-23 | #34182 | fechado | admin | #778983 | #846 | false/false/false | 0/2 | 2/2 | 2/4 | E, O | [nao verificado] |
| 34 | col921 | 2026-10-25 | #34815 | fechado | admin | #778986 | #846 | false/false/false | 0/2 | 2/2 | 2/4 | E, O | [nao verificado] |
| 35 | col921 | 2026-10-26 | #35032 | fechado | admin | #778989 | #846 | false/false/false | 0/1 | 2/2 | 2/4 | E, O | [nao verificado] |
| 36 | col921 | 2026-10-27 | #35280 | fechado | admin | #801831 | #846 | false/false/false | 0/2 | 2/2 | 2/4 | N, E | [nao verificado] |
| 37 | col921 | 2026-10-28 | #35490 | fechado | admin | #801834 | #846 | false/false/false | 0/2 | 2/2 | 2/4 | N, E | [nao verificado] |
| 38 | col921 | 2026-10-29 | #35756 | fechado | admin | #801837 | #846 | false/false/false | 0/1 | 2/2 | 2/4 | N, E | [nao verificado] |
| 39 | col923 | 2026-10-25 | #34888 | fechado | admin | #778631 | #846 | false/false/false | 0/3 | 2/4 | 2/4 | E, O | [nao verificado] |
| 40 | col1005 | 2026-10-28 | #35423 | resolvido | sistema | #788507 | null | true/false/false | 0/2 | 2/4 | 2/2 | N, E | [nao verificado] |
| 41 | col1005 | 2026-10-29 | #35679 | resolvido | sistema | #797647 | null | true/false/false | 0/2 | 2/4 | 2/2 | N, E | [nao verificado] |
| 42 | col1005 | 2026-10-30 | #35889 | resolvido | sistema | #806725 | null | true/false/false | 0/2 | 2/4 | 2/2 | N, E | [nao verificado] |
| 43 | col42 | 2026-10-08 | #29676 | resolvido | sistema | #724326 | null | true/false/false | 0/2 | 1/1 | 1/1 | E | [nao verificado] |
| 44 | col42 | 2026-10-15 | #31699 | resolvido | sistema | #769684 | NV | NV/NV/NV | 0/2 | 1/1 | 2/11 | O | [nao verificado] |
| 45 | col258 | 2026-10-02 | #28094 | resolvido | sistema | #639802 | #657 | false/false/false | 0/2 | 1/1 | 2/2 | E, R | [nao verificado] |

Quanto às ações específicas pedidas:

| Categoria | Encontrado no conteúdo exibido | Fonte |
|---|---|---|
| `reconciliar_*` | Dia 45, chamado #28094: `reconciliar_ausencia_lancada`, log #639803, usuário #666, ator=false, motivo=false, resolução=false | [nao verificado] |
| `dedup` | Nenhuma ação com esse nome/trecho exibida; conteúdo dos logs omitidos: NV | [nao verificado] |
| `sem_dia_do_fato` | Nenhuma ação com esse nome/trecho exibida; conteúdo dos logs omitidos: NV | [nao verificado] |
| `competencia_trancada` | Nenhuma ação com esse nome/trecho exibida; conteúdo dos logs omitidos: NV | [nao verificado] |
| `OUTRO` | Literal `OUTRO-NAO-EXIBIDO`: dias 5, 6, 10/chamado #8591, 19, 20, 21, 24, 30, 31, 32, 33, 34, 35, 39 e 44 | [nao verificado] |

No dia 5, o apoio da disputa #7547 informa `motivo="veredito"`, `resolucao_presente=true` e usuário #666. Esses são campos de `disputas_vinculos_atuais`; os campos do log de travessia #783748 permanecem NV. Nos demais apoios exibidos, a disputa tem `motivo="OUTRO-NAO-EXIBIDO"` e `resolucao_presente=false`. [nao verificado]

3) Agregados usando **via do dono** × **usuário do log da travessia**. Conto cada dia uma vez dentro de cada grupo e somente as perguntas efetivamente exibidas; perguntas omitidas não recebem a classificação das amostras. [nao verificado]

| Via | Usuário da travessia | Dias com ocorrência exibida | Perguntas exibidas | Fonte |
|---|---|---:|---:|---|
| sistema | null | 23 | 44 | [nao verificado] |
| sistema | #pk | 1 | 1 | [nao verificado] |
| admin | null | 0 | 0 | [nao verificado] |
| admin | #pk | 15 | 29 | [nao verificado] |

O grupo `sistema × #pk` contém #657: 1 dia e 1 pergunta. O grupo `admin × #pk` contém #846: 14 dias e 27 perguntas; e #845: 1 dia e 2 perguntas. [nao verificado]

| Parcela sem classificação completa | Dias envolvidos | Perguntas | Fonte |
|---|---:|---:|---|
| Via sistema exibida; usuário da travessia NV | 4: dias 1, 2, 5 e 44 | 6 | [nao verificado] |
| Via admin exibida; usuário da travessia NV | 3: dias 10, 21 e 23 | 5 | [nao verificado] |
| Perguntas omitidas de `vivas.amostra`; via e usuário NV | 23 | 46 | [nao verificado] |

A soma de `quantidade_vivas` dos 45 dias é **131 perguntas**: **85 exibidas**, sendo **74 classificáveis** na primeira tabela e **11 com usuário da travessia NV**, mais **46 omitidas**. Os totais completos por via × usuário permanecem `[nao verificado]`. [nao verificado]

As contagens de dias dos grupos não são somáveis: o dia 10 aparece em `sistema × null` e em `admin × NV`. [nao verificado]

4) Todas as linhas contendo `ERRO-NAO-MEDIDO` são estas quatro linhas do resumo. Nenhuma das 45 linhas de dia tem essa classe; todas exibem `erros=[]`. [nao verificado]

- `{"resumo": "dias", "classe": "ERRO-NAO-MEDIDO", "comp_anexo": "10/2026", "empresa_anexo": "#2", "n": 0, "n_parcial_conhecido": 0, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "ERRO-NAO-MEDIDO", "comp_anexo": "10/2026", "empresa_anexo": "#4", "n": 0, "n_parcial_conhecido": 0, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "ERRO-NAO-MEDIDO", "comp_anexo": "11/2026", "empresa_anexo": "#2", "n": 0, "n_parcial_conhecido": 0, "integral": true, "resultado": "MEDIDO"}` [nao verificado]
- `{"resumo": "dias", "classe": "ERRO-NAO-MEDIDO", "comp_anexo": "11/2026", "empresa_anexo": "#3", "n": 0, "n_parcial_conhecido": 0, "integral": true, "resultado": "MEDIDO"}` [nao verificado]

## 11/11 00:03 · sessao cbda6a0a85db · FIO-E0-V4-E4B · sonda · origem=Code

O dado termina no meio do registro `col718 / 2026-10-26`; não contém o resumo consolidado da sonda. Abaixo, as recontagens são **parciais**, limitadas aos registros completos. As referências ao texto colado levam `[nao verificado]` por não haver arquivo:linha disponível. [nao verificado]

**(1) Classe × competência × empresa**

A contagem consolidada “como a sonda imprimiu” não aparece no trecho. As classes `a` e `b` não estão exibidas; suas contagens não podem ser transcritas como zero. [nao verificado]

| Classe | comp_anexo | empresa_anexo | Dias-colab completos recontados | Perguntas em `vivas` | Referência |
|---|---|---|---:|---:|---|
| indeterminado | 11/2026 | #2 | 18 | 61 | [nao verificado] |
| indeterminado | 11/2026 | #3 | 2 | 4 | [nao verificado] |
| **Total parcial** | 11/2026 | — | **20** | **65** | [nao verificado] |

**(2) Indeterminados: via e evidência impressa**

Nos registros completos, as 65 perguntas apontam para 21 chamados donos distintos; o dia de `col355 / 2026-10-25` contém dois donos, um por `admin` e outro por `sistema`. [nao verificado]

| Via do dono (`dono_terminal.via`) | Donos distintos | Perguntas | Referência |
|---|---:|---:|---|
| sistema | 17 | 56 | [nao verificado] |
| admin | 4 | 9 | [nao verificado] |

Para o `estado_chamado` indicado por `historico.travessia`, a distribuição do ator impresso é: [nao verificado]

| Via | `usuario` no log | `ator_processo_presente` | Donos distintos | Perguntas | Referência |
|---|---|---|---:|---:|---|
| sistema | null | true | 16 | 55 | [nao verificado] |
| sistema | #657 | false | 1 | 1 | [nao verificado] |
| admin | #846 | false | 4 | 9 | [nao verificado] |

O caso `sistema / usuario #657` é o chamado `#35126`, pergunta `#50429`, travessia `#783748`. Os quatro donos por `admin / usuario #846` são `#34447`, `#34762`, `#33930` e `#34540`, com travessias `#778802`, `#779377`, `#792218` e `#792222`, respectivamente. [nao verificado]

| Evidência | Distribuição nos registros completos | Referência |
|---|---|---|
| `estado_chamado` da travessia terminal | Presente nos 21 donos, associados às 65 perguntas; todos com `motivo_presente:false` e `resolucao_presente:false` nesse log | [nao verificado] |
| Decisões nos vínculos atuais | `decisoes_vinculos_atuais:[]` nos 21 donos | [nao verificado] |
| Notas | 51 IDs distintos, distribuídos pelos 21 donos; todas com `texto_presente:true`, sem conteúdo textual exibido | [nao verificado] |
| `reconciliar_ausencia_lancada` | Um log explícito: `#783753`, chamado `#35126`, usuário `#666`, ausência `#5211`; associado à pergunta `#50429` | [nao verificado] |
| Disputa com resolução presente | No apoio de `#35126`: disputa `#7547`, usuário `#666`, motivo `veredito`, `resolucao_presente:true` | [nao verificado] |
| `reconciliar_feriado`, dedup, `sem_dia_do_fato` | Nenhuma ocorrência explicitamente identificada com esses nomes; há ações `OUTRO-NAO-EXIBIDO` | [nao verificado] |
| `competencia_trancada` | Nenhuma ocorrência explicitamente identificada; o estado da competência não foi exibido | [nao verificado] |

A duração efetiva da pergunta viva não foi impressa. O código descreve `data_evento` como data do evento: [app/chamados/models.py:2423](/home/ronald/codex_ws/fb9269ea9a60bb7748c777764d6e3d57a6b54402/app/chamados/models.py:2423). A tabela seguinte calcula somente **11/11/2026 − `data_evento`**, para as perguntas listadas em `vivas`; não comprova tempo contínuo de vida. [nao verificado]

| `data_evento` | Dias até 11/11/2026 | Dias-colab completos | Perguntas | Referência |
|---|---:|---:|---:|---|
| 2026-10-21 | 21 | 1 | 2 | [nao verificado] |
| 2026-10-22 | 20 | 4 | 14 | [nao verificado] |
| 2026-10-23 | 19 | 2 | 8 | [nao verificado] |
| 2026-10-24 | 18 | 6 | 22 | [nao verificado] |
| 2026-10-25 | 17 | 3 | 10 | [nao verificado] |
| 2026-10-26 | 16 | 2 | 5 | [nao verificado] |
| 2026-11-03 | 8 | 1 | 2 | [nao verificado] |
| 2026-11-06 | 5 | 1 | 2 | [nao verificado] |
| **Total parcial** | — | **20** | **65** | [nao verificado] |

Separadamente, o registro truncado mostra `col718 / 2026-10-26`, competência `11/2026`, empresa `#2`, classe `indeterminado`, pergunta `#50647`, dono `#35092`, status `fechado`, via `admin`, `usuario_gravado:#846` e travessia `#792225`. A diferença desde `data_evento` é 16 dias. O restante das perguntas, das evidências e `quantidade_vivas` desse registro não está disponível; ele foi excluído das tabelas anteriores. [nao verificado]

**(3) ERRO-NAO-MEDIDO**

Nenhuma linha `ERRO-NAO-MEDIDO` aparece no trecho fornecido. Os 20 registros completos exibem `erros:[]`; o registro truncado também exibe `erros:[]` antes do corte. Isso não informa o conteúdo da parte ausente. [nao verificado]

## 10/11 23:59 · sessao a8e3ccca6aa3 · FIO-E0-V4-E4B · leitura · origem=Code

**Não é possível separar os 45 em a/b com o anexo fornecido.** Ele identifica dias, mas não informa chamados, autores ou atos de fechamento. Distribuição efetiva em produção: **[não verificado]**. Além disso, `via_encerramento='admin'` não comprova decisão real: houve backfill desse valor apenas pela presença de um usuário não sistêmico. `app/chamados/migrations/0039_a3_via_backfill.py:14`, `app/chamados/migrations/0039_a3_via_backfill.py:26`.

**1. Caminhos para estado terminal**

Terminal compreende `resolvido`, `fechado`, `cancelado` e `superado`; a investigação precisa cobrir os quatro. `app/chamados/catalogo/motor.py:30`.

Nos gates de transição abaixo, o ritual comum escreve `status_local`, `via_encerramento`, `resolvido_em`, `atualizado_em` e rebaixa a urgência preservando a original. Escreve `respondido_por` somente quando `set_user=True`. `app/chamados/models.py:228`, `app/chamados/models.py:233`, `app/chamados/models.py:459`.

A auditoria comum é `LogAuditoria.acao='estado_chamado'`, com antes `{status_local}` e depois `{status_local, via}`. O usuário pode vir do argumento/request; fora de request existe o ator de processo `sistema:<comando>`. A gravação da auditoria é best-effort. `app/chamados/models.py:292`, `app/core/observ.py:94`, `app/core/observ.py:108`.

| Entrada | Escrita específica | Lastro ou limite |
|---|---|---|
| `transicionar → _aplicar` | Destino humano; via `admin`; substitui `respondido_por`. | Pode acompanhar decisão real, mas também mera mudança de status sem texto. `app/chamados/models.py:128`, `app/chamados/models.py:447`, `app/chamados/services/acoes_chamado.py:283`, `app/chamados/services/acoes_chamado.py:313`. |
| `fechar(user, novo_status, resolucao)` | Delega a `transicionar`; resolução opcional vira nota no fio depois da transição. | Não exige resolução preenchida. `app/chamados/models.py:452`. |
| `reconciliar` | `resolvido`, via `sistema`; preserva o usuário anteriormente gravado. | Hoje consulta as guardas da premissa e da pergunta viva. Permite também agrupamento, dedup e fechamento sem dia identificável, este último com trilha específica. `app/chamados/models.py:641`, `app/chamados/models.py:682`, `app/chamados/models.py:717`, `app/chamados/models.py:732`, `app/chamados/models.py:738`, `app/chamados/models.py:750`. |
| `cancelar(motivo, user)` | `cancelado`, via `cancelado`; troca usuário somente se fornecido; motivo opcional vira nota. | Descarte administrativo/emissor aposentado; a porta não demonstra por si uma premissa factual morta. `app/chamados/models.py:773`, `app/chamados/models.py:788`. |
| `encerrar` | `fechado`, via `cascade`; preserva usuário. | Não consulta a guarda da premissa usada por `reconciliar`; consulta a guarda de pergunta viva. A prova substantiva depende do chamador. `app/chamados/models.py:1605`, `app/chamados/models.py:1617`. |
| `encerrar_por_competencia_trancada` | `fechado`, via `competencia_trancada`; serviço acrescenta nota e evento com competência/empresa. | O código denomina isso lastro de tranca efetiva, mas esclarece que não é resolução pela realidade. Não equivale a simples idade/janela. `app/chamados/models.py:1622`, `app/chamados/services/competencia_trancada.py:3`, `app/chamados/services/competencia_trancada.py:12`, `app/chamados/services/competencia_trancada.py:184`. |
| Migração 0039 | Cancelado com irmão único vira `superado`, via `superado`, `superado_por_id`. | Escrita histórica direta; esse trecho não grava ator, log nem timestamp. A porta corrente `superar()` foi removida. `app/chamados/migrations/0039_a3_via_backfill.py:15`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/models.py:794`. |

Há ainda **nascimento admitido com status terminal**, fora do ritual de transição: `criar(status=...)` aceita status explícito; `abrir(defaults=...)` permite sobrescrever o status inicial. Ambos deixam `nascimento_chamado`, não necessariamente uma travessia vivo→terminal. `app/chamados/models.py:1054`, `app/chamados/models.py:1063`, `app/chamados/models.py:1095`, `app/chamados/models.py:938`, `app/chamados/models.py:971`, `app/chamados/models.py:1218`. O chamador explícito encontrado de `criar(status=...)` é o smoke, limitado a banco de teste e com rollback. `app/chamados/management/commands/smoke_chamados.py:31`, `app/chamados/management/commands/smoke_chamados.py:39`, `app/chamados/management/commands/smoke_chamados.py:123`.

Os chamadores encontrados dos gates terminais são:

- **Transição humana:** aprovação de exceção, aceite/recusa de agenda e escala, autorização/recusa de geofence e resposta livre com mudança opcional de status. `app/chamados/services/acoes_chamado.py:70`, `app/chamados/services/acoes_chamado.py:108`, `app/chamados/services/acoes_chamado.py:149`, `app/chamados/services/acoes_chamado.py:222`, `app/chamados/services/acoes_chamado.py:277`, `app/chamados/services/acoes_chamado.py:315`. Também resposta pelo fio e decisão de benefício. `app/chamados/services/fio.py:721`, `app/colaboradores/views.py:2068`.
- **Fechamento assinado automaticamente como admin:** comando de meio período escolhe o primeiro superuser e chama `fechar`. É mais uma razão para não interpretar `via=admin` como identidade do agente real. `app/ponto/management/commands/fechar_chamados_meio_periodo.py:103`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:109`.
- **Cascade:** desligamento, substituição de aceite antigo, regularização externa e fecho com retenção usado pela justificativa. `app/colaboradores/services/desligamento.py:155`, `app/colaboradores/services/vinculo.py:387`, `app/chamados/services/regularizacao_ext.py:205`, `app/chamados/reconciliador.py:485`, `app/ponto/services/justificativa.py:87`, `app/ponto/services/justificativa.py:400`.
- **Cancelamento:** avisos de régua e revisão de vínculo transferidos para a lista Cadastro x Realidade. `app/escala/management/commands/fechar_avisos_regua_na_lista.py:47`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57`.
- **Reconciliação por disputa/retratacão/materialização:** signal da disputa, reconciliador, materialização de ausência/pergunta, adesão, pós-vínculo e agrupamento/folga. `app/chamados/signals.py:42`, `app/chamados/reconciliador.py:523`, `app/chamados/services/materializacao.py:582`, `app/chamados/services/materializacao.py:771`, `app/chamados/services/adesao.py:115`, `app/chamados/services/adesao.py:150`, `app/colaboradores/services/pos_vinculo.py:42`, `app/chamados/services/limbo_folgas.py:101`, `app/chamados/services/limbo_folgas.py:283`, `app/chamados/services/limbo_folgas.py:286`.
- **Reconciliação por fatos do dia:** feriado/folga, ausência decidida/aprovada, cobertura, cluster, marcos acesos, entrada sincronizada, triagem e decisão administrativa da célula. `app/chamados/services/feriado_reavaliar.py:47`, `app/chamados/services/silencio_ausencia.py:101`, `app/ponto/services/ausencia.py:694`, `app/ponto/services/cobertura.py:122`, `app/ponto/services/cobertura.py:317`, `app/ponto/services/cluster_espurio.py:124`, `app/ponto/services/lastro.py:400`, `app/ponto/services/entrada_sincronizada.py:31`, `app/ponto/services/triagem_batida.py:597`, `app/ponto/services/veredito_celula.py:202`.
- **Cartório:** reconciliações nas passadas de lavra, marcos satisfeitos/inexistentes, ausência de protesto e duplicidade do fio. `app/ponto/services/cartorio.py:974`, `app/ponto/services/cartorio.py:1499`, `app/ponto/services/cartorio.py:1512`, `app/ponto/services/cartorio.py:1533`, `app/ponto/services/cartorio.py:1611`, `app/ponto/services/cartorio.py:1626`.
- **Comandos adicionais:** auditoria de órfãos, feriado do posto, intervalo ausente, cura BUG115 e detecção de ausências por premissa/afastamento/cobertura/batida. `app/chamados/management/commands/auditar_chamados_orfaos.py:67`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:140`, `app/ponto/management/commands/detectar_intervalo_ausente.py:138`, `app/ponto/management/commands/curar_residuo_bug115.py:151`, `app/ponto/management/commands/detectar_ausencias.py:158`, `app/ponto/management/commands/detectar_ausencias.py:824`, `app/ponto/management/commands/detectar_ausencias.py:834`, `app/ponto/management/commands/detectar_ausencias.py:845`.

**Fechamento por idade antigo:** há lápide expressa da expiração de disputa parada por 30 dias, revogada pela C7. O cron de prescrição corrente somente silencia, sem terminalizar. `app/chamados/management/commands/arquivar_competencia_encerrada.py:30`, `app/chamados/management/commands/arquivar_competencia_encerrada.py:61`, `app/chamados/management/commands/arquivar_prescritos.py:6`, `app/chamados/management/commands/arquivar_prescritos.py:94`. O cluster também documenta a antiga confusão “fora da janela = premissa falsa”; hoje consulta a célula do próprio dia, sem limite etário para fechar. `app/ponto/services/cluster_espurio.py:53`. Campos e trilhas exatos das versões removidas: **[não verificado]**.

**2. O que o banco permite distinguir retrospectivamente**

O chamado possui `status_local`, `via_encerramento`, `respondido_por`, `resolvido_em`, `atualizado_em` e `superado_por`. `fechada_por`, `motivo_fechamento` e `resolucao` pertencem à **Disputa**, não ao chamado. `app/chamados/models.py:52`, `app/chamados/models.py:58`, `app/chamados/models.py:1766`, `app/chamados/models.py:1773`, `app/chamados/models.py:1793`.

| Evidência gravada | O que permite concluir |
|---|---|
| Via `admin`, usuário preenchido | Identifica uma via/atribuição, sem provar decisão substantiva; pode ser backfill ou comando assinado por superuser. `app/chamados/migrations/0039_a3_via_backfill.py:26`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:103`. |
| Via `sistema`, `cascade`, `cancelado`, `legado` | Não identifica, sozinha, a prova que autorizou o fechamento. O evento genérico guarda somente estado/via. `app/chamados/catalogo/motor.py:86`, `app/chamados/models.py:292`. |
| `respondido_por` | Pode ser resíduo de ato/ciclo anterior, pois os fechamentos automáticos não o substituem. `app/chamados/models.py:230`, `app/chamados/models.py:654`, `app/chamados/models.py:1619`, `app/chamados/models.py:1639`. |
| `resolvido_em` | Marca a saída de vivo, mas `resolvido→fechado` preserva esse instante e sobrescreve a via. A última linha terminal pode não ser quem matou a vida. `app/chamados/models.py:242`. |
| `fechamento_por_dedup` | Prova que foi invocada a sobrevivência de outra cobrança; traz dia e chamado sobrevivente. Não demonstra premissa factual morta. `app/chamados/models.py:750`. |
| `fechamento_sem_dia_do_fato` | Declara que a guarda da premissa não conseguiu julgar. Não prova, sozinho, nem lastro nem inexistência de outro ato. `app/chamados/models.py:732`. |
| `reconciliar_feriado` / `reconciliar_ausencia_lancada` | Evidência específica com dia e, no segundo caso, ausência identificada; precisa ser atribuída ao ciclo correto. `app/chamados/services/feriado_reavaliar.py:52`, `app/chamados/services/silencio_ausencia.py:108`. |
| Disputa `admin_encerra`, resolução, autor, `disputa_fechada` e decisões das perguntas | Registram ato explícito sobre a disputa/perguntas. O fechamento do chamado-pai é outro passo, tentado pelo signal. `app/chamados/models.py:1918`, `app/chamados/models.py:1932`, `app/chamados/models.py:1943`, `app/chamados/signals.py:42`. |
| `DecisaoPergunta` | Registra ato, instante, autor e motivo sobre a pergunta; não possui vínculo explícito com o evento que terminalizou determinado ciclo do chamado. `app/chamados/models.py:3839`, `app/chamados/models.py:3871`. |
| Nota do fio | Guarda conteúdo/autor/instante, mas não persiste o parâmetro `tipo` nem `objeto_ref` como campos recuperáveis. `app/chamados/models.py:1668`, `app/chamados/models.py:1688`, `app/chamados/services/nota.py:87`, `app/chamados/services/nota.py:95`. |
| Ausência de auditoria/decisão | Não prova ausência de ato: a auditoria é best-effort e existe legado anterior à sequência de decisões. `app/core/observ.py:94`, `app/chamados/models.py:3913`. |

Portanto, a investigação deve localizar a **travessia vivo→terminal do ciclo corrente**, conferir sua cadeia com o estado do banco e procurar a prova substantiva correspondente. Sem prova positiva de fechamento por idade/ausência de lastro, não é correto transformar lacuna de trilha em classe **b**. Sem vínculo suficiente entre decisão/prova e fechamento, permanece **indeterminado**. `app/chamados/models.py:242`, `app/chamados/models.py:292`, `app/core/observ.py:98`.

**3. Portas existentes**

- **O261-E F1 existe como guarda do encerramento do chamado.** Busca perguntas ancoradas, consulta C6 no pós-estado e restringe o alcance à competência reconhecida como aberta. No sistema, reata todas a uma cobrança sobrevivente válida ou recusa; na transição humana, recusa. Há tripwire no núcleo e guarda equivalente na costura. `app/chamados/models.py:319`, `app/chamados/models.py:374`, `app/chamados/models.py:410`, `app/chamados/models.py:148`, `app/chamados/models.py:208`, `app/chamados/services/fio.py:745`. O BACKLOG distingue F1 da porta F2 de `materializada_em`. `app/docs/BACKLOG.md:874`.
- **O encerramento gravado da pergunta passa por F2:** `carimbar` aplica os campos em memória, consulta C6 e somente grava se a pergunta morrer; `carimbar_identidade` trata as vias de identidade. `fechar_por_admin` usa `admin_decidiu`. `app/chamados/models.py:3050`, `app/chamados/models.py:3097`, `app/chamados/models.py:3103`, `app/chamados/models.py:3168`.
- **Não há via genérica denominada `premissa_morta`.** Há vias distintas como `celula_absolveu`, `chamado_encerrado`, `admin_decidiu` e `motivo_retratado`; não cabe escolher uma delas apenas pelo rótulo do aval. `app/chamados/models.py:2525`.
- **Outros escritores dos campos da pergunta:** limpeza por `descarimbar`/`descarimbar_em_lote`; troca isolada da via por `encerrar_por_fato_no_espelho`; e `relavra_via.relavrar`. `app/chamados/models.py:3203`, `app/chamados/models.py:3251`, `app/chamados/models.py:3268`, `app/chamados/services/relavra_via.py:106`.
- **A reabertura do chamado existe:** `reabrir` consulta `recusa_da_reabertura`, grava `em_analise`, limpa via/instante pelo núcleo e chama `ressuscitar_perguntas`. Há recusas por veto, admin sem fato novo, premissa regularizada, agrupamento, irmão vivo e transição impossível. `app/chamados/models.py:1318`, `app/chamados/models.py:1362`, `app/chamados/models.py:1439`.
- **L-125 é a lei da família cluster, não uma API especial de reabertura.** Ela exige leitura da célula do próprio dia para fechar; ausência de célula/insumos não vira premissa falsa. `app/docs/LEIS.md:146`, `app/ponto/services/cluster_espurio.py:15`, `app/ponto/services/cluster_espurio.py:53`. Além de `reabrir`, `analisar` e as transições humanas escrevem os mesmos campos de estado pelo núcleo; nascimentos e migração 0039 são as exceções descritas acima. `app/chamados/models.py:800`, `app/chamados/models.py:447`, `app/chamados/models.py:1095`, `app/chamados/migrations/0039_a3_via_backfill.py:24`.

A guarda atual de reabertura pode tratar `via='admin'` como impedimento sem procurar um ato substantivo registrado. Logo, ela também não serve como juiz histórico de a/b. `app/chamados/models.py:1342`.

**4. Classe e e relação com o DRY**

Não localizei `backfill_fio_morto_e4b` nesta árvore. Sua implementação e sua chamada exata: **[não verificado]**. A obra consta pelo nome no BACKLOG. `app/docs/BACKLOG.md:1875`.

O sítio disponível que produz o detalhe `pergunta_viva_sob_chamado_terminal` é `vigia_fio.censo_alcance`:

1. Obtém os dias retidos por empresa/competência e encontra a célula por `(colaborador_id, data)`. `app/chamados/services/vigia_fio.py:581`, `app/chamados/services/vigia_fio.py:593`.
2. Chama `alimentar_portas`; suas vivas vêm de **todas** as perguntas da célula, passando pelo juiz `pergunta_viva`, com mapa de identidade do colaborador. `app/chamados/services/vigia_fio.py:603`, `app/chamados/juizes.py:1501`.
3. Resolve a âncora por `chamado_que_lista(p)`: destino, senão chamado da disputa; consulta o status dessas âncoras. `app/chamados/services/fio.py:116`, `app/chamados/services/vigia_fio.py:617`.
4. Somente depois das precedências anteriores atribui classe e e o detalhe terminal. Uma pergunta respondida sob terminal pode cair antes em **c**; “viva sob terminal” isoladamente não reproduz toda a classificação do censo. `app/chamados/services/vigia_fio.py:667`, `app/chamados/services/vigia_fio.py:675`, `app/chamados/services/vigia_fio.py:687`.

**5. Sonda de leitura**

O script abaixo mede as perguntas pelo **mesmo C6 e pela mesma consulta da alimentação disponível**, e resolve o dono pela mesma função do fio. Não reexecuta o universo inteiro nem afirma reproduzir a seleção do comando ausente. `app/chamados/juizes.py:1501`, `app/chamados/services/fio.py:116`. Execução no tenant: **[não verificado]**.

**Limitação deliberada:** esta sonda não inventa um juiz retrospectivo a/b. Ela imprime as evidências e deixa a classificação como `indeterminado`, indicando o campo/lacuna que impediu a conclusão. Os baldes a/b contam classificações comprovadas pela sonda, não presumem que o universo real tenha zero casos. O predicado faltante é: **“qual ato/prova substantiva autorizou a travessia vivo→terminal deste ciclo?”** O evento genérico existente não registra essa relação. `app/chamados/models.py:292`, `app/chamados/models.py:3871`.

As competências/empresas do resumo são as do anexo; a empresa atual é impressa separadamente. A lista fornecida e sua correspondência atual no banco: **[não verificado]**.

```python
# Colar no manage.py shell JA no schema do tenant.
# Lista: anexo classe_e_45.txt fornecido; banco [nao verificado].
# C6/consulta: app/chamados/juizes.py:1501.
# Ancora: app/chamados/services/fio.py:116.
# Trilha: app/chamados/models.py:292.
# Limite historico: migrations/0039_a3_via_backfill.py:26.

import json
import logging
from collections import Counter
from contextlib import ExitStack
from datetime import date

from django.db import connection, connections, transaction
from django.contrib.auth import get_user_model
from django.db.models import Q
from django.utils import timezone

from chamados.models import (
    ChamadoColaborador, PerguntaDisputa, DisputaSupervisao,
    DecisaoPergunta, FollowupChamado,
)
from chamados.catalogo.motor import (
    ESTADOS, VIVOS, TERMINAIS, VIAS_ENCERRAMENTO,
)
from chamados.juizes import (
    pergunta_viva, um_colaborador_por_identidade, tranca_em_lote,
)
from chamados.services.fio import chamado_que_lista
from colaboradores.models import Colaborador
from escala.models import CelulaDia
from core.observ import engolidos

DADOS = """
114 2026-11-03 11/2026 3
114 2026-11-06 11/2026 3
209 2026-10-22 11/2026 2
209 2026-10-24 11/2026 2
258 2026-10-26 11/2026 2
267 2026-10-24 11/2026 2
268 2026-10-22 11/2026 2
268 2026-10-24 11/2026 2
298 2026-10-21 11/2026 2
355 2026-10-25 11/2026 2
427 2026-10-22 11/2026 2
427 2026-10-24 11/2026 2
427 2026-10-26 11/2026 2
534 2026-10-23 11/2026 2
534 2026-10-24 11/2026 2
534 2026-10-25 11/2026 2
650 2026-10-23 11/2026 2
650 2026-10-25 11/2026 2
718 2026-10-22 11/2026 2
718 2026-10-24 11/2026 2
718 2026-10-26 11/2026 2
718 2026-10-28 11/2026 2
723 2026-10-28 11/2026 2
865 2026-10-23 11/2026 2
900 2026-11-01 11/2026 3
900 2026-11-03 11/2026 3
900 2026-11-05 11/2026 3
900 2026-11-07 11/2026 3
900 2026-11-09 11/2026 3
918 2026-10-25 11/2026 2
921 2026-10-21 11/2026 2
921 2026-10-22 11/2026 2
921 2026-10-23 11/2026 2
921 2026-10-25 11/2026 2
921 2026-10-26 11/2026 2
921 2026-10-27 11/2026 2
921 2026-10-28 11/2026 2
921 2026-10-29 11/2026 2
923 2026-10-25 11/2026 2
1005 2026-10-28 11/2026 2
1005 2026-10-29 11/2026 2
1005 2026-10-30 11/2026 2
42 2026-10-08 10/2026 4
42 2026-10-15 10/2026 4
258 2026-10-02 10/2026 2
"""

itens = [x.split() for x in DADOS.splitlines() if x.strip()]
saida = []
bloqueios = [0]
ERRO = "ERRO-NAO-MEDIDO"

ACOES = {
    "nascimento_chamado", "estado_chamado",
    "fechament

[truncado em 20000 de 32480 caracteres]

## 10/11 23:54 · sessao bf810b28509c · - · - · origem=Ronald (tmux)

Complemento de exceções ao núcleo:

- `criar(status=...)` aceita explicitamente status terminal, sem validar contra o motor; grava por `objects.create` e deixa `nascimento_chamado`, sem ritual terminal do `_gravar_estado`. `app/chamados/models.py:1054`, `app/chamados/models.py:1063`, `app/chamados/models.py:1095`, `app/chamados/models.py:1218`.
- `abrir(defaults=...)` também admite sobrescrever status inicial: `_def={'status_local':motor...}` recebe `_def.update(defaults)` e segue ao `get_or_create`. `app/chamados/models.py:938`, `app/chamados/models.py:940`, `app/chamados/models.py:971`.
- Busca AST em memória encontrou um chamador explícito de `criar(status=...)`: smoke usa estados do grafo, incluindo terminais. Esse comando recusa banco sem `test` no nome e força rollback. `app/chamados/management/commands/smoke_chamados.py:31`, `app/chamados/management/commands/smoke_chamados.py:39`, `app/chamados/management/commands/smoke_chamados.py:123`, `app/chamados/management/commands/smoke_chamados.py:130`.
- Nenhum chamador atual de `abrir` com defaults terminal foi localizado; possibilidades de argumentos externos ao código inspecionado são **[não verificado]**.
- `app/chamados/admin.py` está vazio; não encontrei registro de `ChamadoColaborador` em Django admin na árvore. Registro em runtime: **[não verificado]**. A existência genérica de `/admin/` está em `app/config/urls.py:24`; não prova uma tela do Chamado.

## 10/11 23:52 · sessao bf810b28509c · - · - · origem=Ronald (tmux)

Auditoria somente leitura. Resultado consolidável:

| Caminho terminal | Escrita específica | Lastro/limite |
|---|---|---|
| `transicionar → _aplicar` | `via_encerramento='admin'`, `respondido_por=user`; destino humano `resolvido/fechado` | Pode representar decisão explícita, mas também troca de status sem texto e comando assinado pelo primeiro superuser. Não classificar automaticamente por via/usuário. `app/chamados/models.py:128`, `app/chamados/models.py:199`, `app/chamados/models.py:447`, `app/chamados/services/acoes_chamado.py:283`, `app/chamados/services/acoes_chamado.py:313`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:103` |
| `fechar(user, ..., resolucao)` | Delega a `transicionar`; resolução vira Followup após transição | Resolução opcional; não há campo motivo próprio do chamado. `app/chamados/models.py:452` |
| `reconciliar` | Destino `resolvido`, via `sistema`; não substitui `respondido_por` | Guarda atual pergunta premissa do furo e pergunta viva; permite agrupamento, dedup e chamado sem dia com trilha específica. A via não preserva qual prova matou a premissa. `app/chamados/models.py:641`, `app/chamados/models.py:673`, `app/chamados/models.py:717`, `app/chamados/models.py:732`, `app/chamados/models.py:738`, `app/chamados/models.py:750` |
| `cancelar(motivo, user)` | `cancelado`, via `cancelado`; substitui usuário somente se fornecido; motivo opcional vira Followup | Cancelamento administrativo/emissor aposentado; motivo não é persistido em coluna tipada. `app/chamados/models.py:773`, `app/chamados/models.py:788` |
| `encerrar` | `fechado`, via `cascade`; não substitui usuário | Usado por desligamento, ajuste retroativo, substituição de aceite antigo e regularização externa. O gate não exige prova de premissa morta; só pergunta viva atual. `app/chamados/models.py:1605`, `app/colaboradores/services/desligamento.py:155`, `app/ponto/services/justificativa.py:400`, `app/colaboradores/services/vinculo.py:387`, `app/chamados/services/regularizacao_ext.py:205` |
| `encerrar_por_competencia_trancada` | `fechado`, via `competencia_trancada`; serviço acrescenta nota e evento com competência/empresa | Código chama de lastro, mas esclarece que não significa resolução pela realidade. Tranca efetiva é diferente de mera idade/janela. `app/chamados/models.py:1622`, `app/chamados/services/competencia_trancada.py:3`, `app/chamados/services/competencia_trancada.py:12`, `app/chamados/services/competencia_trancada.py:181` |
| Migração 0039 | Cancelado com irmão único vira `superado`, via `superado`, `superado_por_id`; demais cancelados recebem via `cancelado` | Acervo histórico; não grava log, ator nem timestamps nesse backfill. Porta atual `superar()` foi removida. `app/chamados/migrations/0039_a3_via_backfill.py:15`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/models.py:794` |

Ritual comum dos gates atuais: escreve `status_local`, `via_encerramento`, `resolvido_em`, `atualizado_em`, rebaixa urgência preservando a original; `respondido_por` só muda quando `set_user=True`. Registra `estado_chamado` com antes `{status_local}`, depois `{status_local, via}`, ator indicado ou contexto da request/processo. `app/chamados/models.py:228`, `app/chamados/models.py:240`, `app/chamados/models.py:292`, `app/core/observ.py:94`, `app/core/observ.py:108`.

Pontos forenses essenciais:

- Não existe `fechado_por` nem campo próprio de motivo no Chamado; existem `respondido_por`, `via_encerramento`, `resolvido_em`. `fechada_por`, `motivo_fechamento`, `resolucao` pertencem à Disputa. `app/chamados/models.py:52`, `app/chamados/models.py:58`, `app/chamados/models.py:1757`, `app/chamados/models.py:1766`, `app/chamados/models.py:1773`, `app/chamados/models.py:1793`.
- **`via='admin'` não prova ato real:** migração 0039 inferiu esse valor exclusivamente de `respondido_por` diferente dos usernames sistema/system/cron; o comando meio período também produz via admin assinada por superuser escolhido automaticamente. `app/chamados/migrations/0039_a3_via_backfill.py:14`, `app/chamados/migrations/0039_a3_via_backfill.py:26`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:103`.
- **Usuário pode ser resíduo de outro ciclo:** reconciliar/cascade/tranca não o substituem. `resolvido → fechado` preserva `resolvido_em` e sobrescreve via; a última linha terminal pode não ser o ato que matou a vida. Necessário reconstruir a travessia `VIVOS → TERMINAIS` do ciclo corrente e verificar coerência da cadeia com o banco. `app/chamados/models.py:230`, `app/chamados/models.py:242`, `app/chamados/models.py:654`, `app/chamados/models.py:1619`, `app/chamados/models.py:1639`.
- **Followup não persiste `tipo` nem `objeto_ref`.** Só autor/tipo de autor/conteúdo/instante/hash; `tipo` participa de hash apenas quando há referência. Sonda não pode filtrar `followups__tipo`. `app/chamados/models.py:1668`, `app/chamados/models.py:1688`, `app/chamados/services/nota.py:87`, `app/chamados/services/nota.py:95`.
- Auditoria é best-effort: ausência de linha não prova ausência de ato, logo não basta para classificar **b**. `app/core/observ.py:94`, `app/core/observ.py:105`, `app/core/observ.py:133`.
- Eventos estruturados auxiliares: `fechamento_por_dedup` traz dia/sobrevivente/motivo; `fechamento_sem_dia_do_fato` declara julgamento impossível; `disputa_fechada` traz motivo/resolução/usuário/perguntas vivas anteriores; serviço da tranca traz competência/empresa. Nenhum fornece FK do evento substantivo para o evento genérico que encerrou o chamado. `app/chamados/models.py:690`, `app/chamados/models.py:732`, `app/chamados/models.py:750`, `app/chamados/models.py:1937`, `app/chamados/services/competencia_trancada.py:190`.

Lastros específicos e caminhos indiretos:

- Disputa `fechar()` exige usuário, recusa resposta sem veredito; sem exceção exige C6 vazio. Exceções tipadas: `desligamento`, `admin_encerra`, `competencia_trancada`; admin exige motivo. Escreve `fechada_em/por`, `resolucao`, `motivo_fechamento`; em silêncio de admin/desligamento grava decisão por pergunta e trilha. Signal tenta reconciliar pai e acrescenta nota Defesa 1. `app/chamados/models.py:1855`, `app/chamados/models.py:1884`, `app/chamados/models.py:1895`, `app/chamados/models.py:1918`, `app/chamados/models.py:1932`, `app/chamados/models.py:1943`, `app/chamados/signals.py:42`.
- Decisão da célula fecha pergunta, registra `DecisaoPergunta.SILENCIO`, reconcilia chamados e deixa Followup; também lavra célula/log do veredito. `app/ponto/services/veredito_celula.py:179`, `app/ponto/services/veredito_celula.py:194`, `app/ponto/services/veredito_celula.py:201`, `app/ponto/services/veredito_celula.py:345`.
- Marcos acesos: `lastro.py:394`, `lastro.py:400`; sync: `entrada_sincronizada.py:29`, `entrada_sincronizada.py:31`; todos em `app/ponto/services/`.
- Ausência decidida: `app/ponto/services/ausencia.py:692`, `app/ponto/services/ausencia.py:694`; ausência aprovada/cobranca já sem premissa: `app/chamados/services/silencio_ausencia.py:93`, `app/chamados/services/silencio_ausencia.py:108`.
- Feriado/folga: juiz e evento `reconciliar_feriado` em `app/chamados/services/feriado_reavaliar.py:23`, `app/chamados/services/feriado_reavaliar.py:43`.
- Cluster: a janela recorta abertura; fechamento hoje lê célula do próprio dia. Lápide confirma que antes fechamento por janela confundia não medido com falso. `app/ponto/services/cluster_espurio.py:40`, `app/ponto/services/cluster_espurio.py:53`, `app/ponto/services/cluster_espurio.py:124`.
- Cron de prescrição atual **não encerra**: silencia; `arquivar_competencia_encerrada` tem lápide da expiração antiga por conversa parada 30 dias, revogada na C7. Estado/campos históricos exatos desse código removido: **[não verificado]**. `app/chamados/management/commands/arquivar_prescritos.py:6`, `app/chamados/management/commands/arquivar_prescritos.py:94`, `app/chamados/management/commands/arquivar_competencia_encerrada.py:30`, `app/chamados/management/commands/arquivar_competencia_encerrada.py:61`.

Inventário completo de chamadas diretas a `reconciliar` enviado nas mensagens anteriores; todas convergem ao mesmo núcleo. O código não contém um juiz retrospectivo completo de **a/b**. Uma sonda rigorosa deve imprimir evidências estruturadas e manter indeterminado quando não houver prova suficiente de autoria substantiva e ligação ao fechamento daquele ciclo. `app/chamados/models.py:292`, `app/chamados/migrations/0039_a3_via_backfill.py:26`, `app/core/observ.py:98`.

## 10/11 23:50 · sessao 0d6967cc6e14 · - · - · origem=Ronald (tmux)

**Portas existentes e limites, para Q3.**

- **O261-E F1 é a guarda no encerramento do chamado**, não um método genérico “encerre a pergunta porque o chamado morreu”. Ela compõe `pergunta_viva` (C6), `competencia_do_dia_entregue` e a âncora `chamado_que_lista`; busca perguntas não validadas cujo destino é o chamado ou, sem destino, cujo pai da disputa é o chamado. Julga no pós-estado terminal e só alcança competência reconhecida como aberta (`False`); entregue ou indeterminada fica fora. `app/chamados/models.py:319-369`.
- Pelo sistema, `_gate_pergunta_viva` reata **todas** as vivas a uma cobrança sobrevivente válida do mesmo colaborador/dia ou recusa o fechamento com `fechamento_recusado`, motivo `pergunta_viva_na_competencia_aberta`. O reaponte usa `fio.reapontar_resolvedora` e registra `pergunta_reatada`. `app/chamados/models.py:374-445`.
- Pela transição humana, `transicionar` recusa com mensagem se houver pergunta viva ancorada; não reata automaticamente. `app/chamados/models.py:148-171`.
- `_gravar_estado` contém tripwire adicional antes de tocar nos campos: travessia vivo→terminal com pergunta viva levanta `ValueError`. `app/chamados/models.py:208-227`.
- A guarda equivalente na **costura** é `fio._costura_nao_mata_o_fio`; lê status fresco do banco e recusa costurar pergunta viva da competência aberta em terminal. `app/chamados/services/fio.py:745-787`.
- O BACKLOG identifica F1 “porta de encerramento”, F2 “porta de materializada_em”, e declara F1/F2 no ar. `app/docs/BACKLOG.md:874`.

**A porta que efetivamente grava o encerramento da pergunta é o carimbo F2.**

- `carimbar(via, autor, ...)` aplica em memória `materializada_em`, `materializada_por_id`, `via_resolucao`; opcionalmente `respondida_em` e validação com texto; consulta C6; se continuar viva, desfaz e retorna `False`; só persiste se morta. `app/chamados/models.py:3097-3157`.
- `carimbar_identidade` trata as vias cujo próprio ato decide identidade: `admin_decidiu`, `duplicata`, `fato_ocupado`, `paridade_recusada`, `motivo_retratado`. Não pergunta ao C6 porque circular; grava os mesmos três campos e opcional `recusa_motivo`. `app/chamados/models.py:2503-2510`, `app/chamados/models.py:3168-3191`.
- `fechar_por_admin` é wrapper idempotente da via `admin_decidiu`. `app/chamados/models.py:3050-3064`.
- `fechar_o_fio_do_dia` chama esse wrapper e registra `DecisaoPergunta.SILENCIO` com `ato_em=materializada_em`, autor e motivo `dia_decidido_pelo_admin: DATA`; depois reconcilia os chamados vivos daquele dia. `app/ponto/services/veredito_celula.py:160-206`.
- Não há nessa relação uma via chamada `premissa_morta`; as vias declaradas incluem `celula_absolveu`, `chamado_encerrado` e `legado_sem_lastro`, cujos significados são distintos. Não se deve inventar uma via para aplicar o aval. `app/chamados/models.py:2525-2542`.
- A antiga porta de escrita da via pergunta `competencia_trancada` foi removida; hoje a tranca mata por leitura no C6. `app/chamados/models.py:3066-3071`.

**Quem escreve esses campos além da aplicação normal do carimbo.**

- O ponto de atribuição conjunta é `_escrever_carimbo`; limpeza individual vai por `descarimbar`; limpeza em lote por `descarimbar_em_lote`. `app/chamados/models.py:3097-3101`, `app/chamados/models.py:3203-3213`, `app/chamados/models.py:3251-3266`.
- `via_resolucao` tem dois escritores próprios adicionais: `encerrar_por_fato_no_espelho` troca só a via para `fato_materializado`; `relavra_via.relavrar` reescreve via derivada (`declaracao`/`completude_turno`) e grava trilha antes/depois. `app/chamados/models.py:3268-3276`, `app/chamados/services/relavra_via.py:76-78`, `app/chamados/services/relavra_via.py:106-138`.
- O selo AST explicita essas exceções; `RegularizacaoExterna.materializada_em` é homônimo de outro modelo. O selo não alcança `__dict__`, campo vindo de outro módulo/atributo, nem o `SET_NULL` que apaga o autor quando usuário é removido. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:43-58`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:111-115`.

**Reabertura do chamado e L-125.**

- A porta existente é `ChamadoColaborador.reabrir`, que consulta a guarda pura `recusa_da_reabertura`, passa por `decidir(..., 'REABRIR')`, grava `em_analise` pelo núcleo e chama `ressuscitar_perguntas`. `app/chamados/models.py:1362-1452`.
- As guardas incluem veto do dia, `admin_sem_fato_novo`, premissa regularizada, agrupamento, irmão vivo e transição não permitida. São leituras **do estado atual**, não classificação histórica de quem fechou. `app/chamados/models.py:1318-1360`.
- `via_encerramento='admin'` com `resolvido_em` pode bloquear reabertura se não houve fato novo; a guarda não procura ato humano registrado. `app/chamados/models.py:1342-1347`.
- Núcleo de escrita comum `_gravar_estado`: vivo limpa `resolvido_em` e via; terminal carimba/preserva instante, via e urgência; opcionalmente grava `respondido_por`; trilha `estado_chamado` com antes/depois e ator. `app/chamados/models.py:228-253`, `app/chamados/models.py:292-296`.
- **L-125 é lei da família cluster**, não nome de API de reabertura: cluster morre somente pelo lastro da célula do próprio dia; janela recorta abertura. `app/docs/LEIS.md:146`, `app/ponto/services/cluster_espurio.py:53-78`.
- A rotina atual de cluster mede ausência de assinatura pela ata, distinguindo `None` (“não medido”), `()` (“medido sem assinatura”) e assinatura; reconcilia fechamentos sem limite etário. Isso mede premissa atual, não prova o que havia no fechamento antigo. `app/ponto/services/cluster_espurio.py:15-37`, `app/ponto/services/cluster_espurio.py:53-78`, `app/ponto/services/cluster_espurio.py:121-125`.

**Achado forense crítico para Q2/sonda:** `via_encerramento='admin'` **não comprova lastro**, porque a migration legada preencheu esse valor por mera existência de `respondido_por` que não estivesse na lista de usuários sistema; os demais terminais receberam `legado`. `app/chamados/migrations/0039_a3_via_backfill.py:26-28`. O catálogo atual também diz apenas por qual evento a via sai (`TRANSICAO_HUMANA→admin`, `RECONCILIAR→sistema`, etc.), não certifica a existência de decisão real. `app/chamados/catalogo/motor.py:86-96`.

## 10/11 23:50 · sessao c39da2d246d7 · - · - · origem=Ronald (tmux)

- `backfill_fio_morto_e4b` não está entre os arquivos localizados; o comando da cópia e sua chamada exata não podem ser certificados. **[não verificado]** O BACKLOG registra a obra como backfill DRY das classes a–e, pela porta canônica, em `app/docs/BACKLOG.md:1875`.

- O sítio disponível que mede a classe **e / pergunta_viva_sob_chamado_terminal** é `censo_alcance`, em `app/chamados/services/vigia_fio.py:542`. O universo vem de `dias_que_seguram` por empresa/competência, em `:581–587`; a célula é ligada por `(colaborador_id, data)`, em `:593–597`; a alimentação vem de `alimentar_portas`, em `:603–610`.

- **Consulta fiel das perguntas vivas**: `PerguntaDisputa.objects.filter(celula_id__in=_ids).select_related('disputa', 'celula').order_by('pk')`; percorre `um_colaborador_por_identidade` e conserva somente `pergunta_viva(p)`, em `app/chamados/juizes.py:1501–1505`. Não usar somente `q_vivas`: ele é filtro generoso, com diferenças documentadas que exigem avaliação do juiz, em `app/chamados/models.py:3336–3343`.

- **Dono/âncora fiel**: `chamado_que_lista(p)` retorna `p.chamado_destino_id` ou, na falta dele, `p.disputa.chamado_id`, em `app/chamados/services/fio.py:116–117`. O vigia consulta o status dessas âncoras em `app/chamados/services/vigia_fio.py:617–618`. Terminal significa `resolvido`, `fechado`, `cancelado` ou `superado`, em `app/chamados/catalogo/motor.py:30–31`.

- A classe **e** depende da precedência: ausência de célula, véu, resposta vazia, respondida sob destino terminal e cadastro são tratados antes; somente depois o vigia testa `any(s in TERMINAIS for s in anc_st)` e dá o detalhe pedido, em `app/chamados/services/vigia_fio.py:667–688`. Portanto “alguma viva sob terminal” isoladamente não reproduz a classificação completa do censo. `ps` já é a lista julgada por C6, em `:640` e `:671–672`.

- Para a sonda dos dias, buscar `CelulaDia` pelo `(colaborador_id, data)` é fiel ao vigia; a unicidade é declarada em `app/escala/models.py:1833`. Buscar somente `PerguntaDisputa.data_evento` pode divergir: o código reconhece que a célula vinculada pode não ser a de `data_evento`, em `app/chamados/juizes.py:627–641`.

- **Erro engolido é medida inválida**: C6 captura falhas e retorna fallback em `app/chamados/juizes.py:1396–1398` e `:1407–1410`; a janela também captura falhas em `:599–602`. Comparar `core.observ.engolidos()['total']` antes/depois de cada avaliação detecta esses erros: `engolidos()` devolve cópia do contador em `app/core/observ.py:60–63`; `registrar_engolido` incrementa-o em `:73–77`. Se aumentar, emitir `ERRO-NAO-MEDIDO`, nunca contar como falsa/morta/zero.

- Evitar imprimir `str(exc)`, `descricao`, respostas e JSON brutos: tais campos são texto livre (`app/core/models.py:299–301`, `app/chamados/models.py:2447`). Exibir apenas classes de exceção e identificadores numéricos permite a saída raspada solicitada. A presença efetiva de nomes nesses campos dos 45 é **[não verificado]**.

Esqueleto de leitura equivalente ao trecho de alimentação do vigia, sem a fila desnecessária à pergunta específica:

```python
from chamados.juizes import pergunta_viva, um_colaborador_por_identidade
from chamados.models import PerguntaDisputa
from chamados.services.fio import chamado_que_lista
from core.observ import engolidos

# cel e col já lidos pelo (colaborador_id, data).
qs = (
    PerguntaDisputa.objects.filter(celula_id=cel.pk)
    .select_related("disputa", "celula")
    .order_by("pk")
)
vivas = []
for p in um_colaborador_por_identidade(qs, {col.pk: col}):
    antes = engolidos()["total"]
    viva = pergunta_viva(p)
    if engolidos()["total"] != antes:
        raise RuntimeError("ERRO-NAO-MEDIDO")
    if viva:
        vivas.append((p, chamado_que_lista(p)))
```

Esse esqueleto mede o estado atual pelo juiz disponível; reconstruir a vitalidade na data do DRY sem snapshot/trilha suficiente é **[não verificado]**.

## 10/11 23:47 · sessao b1b98eda2f6e · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Cura K3 aplicada: seed preservado na nova/vazia, exceto quando `orfao_14h` do ato é estrangeiro ([fonte](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/services/disputa_emissao.py:1451)). [nao verificado]

- [Entrega com censo e comandos](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5k_entrega.txt:1).
- [Diff completo](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5k.diff:1).
- 114 testes de memória verdes; RED anterior e mutantes documentados nas [saídas](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5k_saidas.txt:1). [nao verificado]

Suíte ORM e certificação final: **[nao verificado]**, reservadas ao Code e opus. [nao verificado]

## 10/11 23:38 · sessao 808c2ef83d03 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

[Entrega K1–K5 e arquivos tocados](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5j_entrega.txt:1), com [comandos e saídas integrais](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5j_saidas.txt:8).

147 testes em memória passaram; 37 mutantes produziram RED com controle verde ([registro](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5j_entrega.txt:5)). [nao verificado]

K3 permanece ACHADO. ORM e ruff=0: **[nao verificado]**, por ausência de Django e ruff ([limites](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5j_entrega.txt:9)).

## 10/11 23:24 · sessao fd6adff78494 · - · - · origem=Ronald (tmux)

K4: `27 != 28` compara **PKs**, não contagens (`chamados/tests/test_bo_disputa_volta5b_orfaos.py:138`). A porta seleciona a primeira disputa aberta (`chamados/services/disputa_emissao.py:1290`), ordenada por `-aberta_em` (`chamados/models.py:1804`). O campo usa `auto_now_add=True`; não há evidência de defeito no patch de relógio (`chamados/models.py:1756`).

A local vazia nasce antes dos chamados (`chamados/tests/test_bo_disputa_volta5b_orfaos.py:130`). Criar chamados aciona o cartório (`chamados/signals.py:188`); um encerramento aciona S143 (`chamados/signals.py:91`), cujo resolver varre todas as disputas abertas e fecha as vazias (`chamados/services/disputa_emissao.py:2130`, `chamados/services/disputa_emissao.py:2178`). **Qual chamado efetivamente provocou esse caminho no traceback: [nao verificado].** Correção defensável da fixture: criar a local vazia por último e verificar, antes da chamada, que continua aberta, sem motivos/perguntas e posterior à dona; preservar todas as asserções finais.

K5: o repasse chama `ch.analisar()` (`chamados/services/disputa_emissao.py:1492`, `chamados/services/disputa_emissao.py:1496`), acordando o cartório; ele ressuscita perguntas (`ponto/services/cartorio.py:1633`), cuja porta reabre a dona (`chamados/models.py:1573`). A reabertura limpa somente `fechada_em`, `fechada_por`, `resolucao` e `motivo_fechamento`, anotando o chamado quando existe (`chamados/models.py:2173`). O teste deve exigir essa transformação exata e preservar os demais campos.

Na segunda passada do replay, a dona já aberta pode ser selecionada, receber o motivo e ser retornada com push; exigir novamente `None` e nenhum push conflita com esse caminho (`chamados/services/disputa_emissao.py:1290`, `chamados/services/disputa_emissao.py:1453`, `chamados/services/disputa_emissao.py:1516`). Separar as expectativas da primeira passagem das da segunda.

O `SystemExit(2)` do auditor é um alarme deliberado quando F1/F2 são positivos; o teste deve capturá-lo, exigir código 2 e continuar verificando I23 (`chamados/management/commands/auditar_invariantes_chamados.py:1942`).

## 10/11 23:13 · sessao cc7466557084 · FIO-E0-V4-E4B · leitura · origem=Code

**Os dois resultados vêm de perguntas diferentes no código:** o command consulta ações exigíveis por instância de marco; o vigia usado pelo command consulta retenção da folha e alcançabilidade de uma porta. O vigia não consulta `tarefa_do_dia`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:67–76`; `app/chamados/services/tarefa_do_dia.py:235–288`; `app/chamados/services/vigia_fio.py:635–699`)

Os números 253, 116, 64, 1 resolvido e 385 → 384 são as medições fornecidas no enunciado. A distribuição dos 116 e dos 64 entre os ramos abaixo, e os insumos de cada dia, permanecem **[nao verificado]**.

**P1. Quem diz “nada” e quem diz “morto”.**

O caminho da tarefa é `ato_do_dia` → `tarefa_do_dia` → `CargaDaTarefa.tarefa` → `_tarefa_pura`. A função pública exige `agora`; na chamada avulsa do command, carrega exatamente `[dia, dia]`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:73–76`; `app/chamados/services/tarefa_do_dia.py:396–406`; `app/chamados/services/tarefa_do_dia.py:442–452`)

Há uma distinção relevante: **`tarefa_nada` é um rótulo construído pelo command**, não o nome de um retorno direto do juiz. Ele aparece quando:

- não foram acumulados `registrar_decisao`, `perguntar`, `materializar` ou `rejulgar`;
- não foi encontrada ação com `porta is None`;
- `t['nao_sei']` é falso.

Ações humanas **com porta preenchida** não entram na lista `humanos`; portanto, isoladamente, `tarefa_nada` não prova que todas as listas `acoes` estavam vazias. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:78–98`)

| Aspecto | Predicado da tarefa |
|---|---|
| Universo de perguntas | `PerguntaDisputa` pela coluna `colaborador_id`, `conversa=False`, `data_evento` dentro de `[ini,fim]`, ordenadas por colaborador e PK. Não filtra por pergunta viva, disputa aberta, validação ou materialização. (`app/chamados/services/tarefa_do_dia.py:323–329`) |
| Identidade considerada | Somente motivos de marco; exclui vias fora do fato, com a exceção temporal da retratação posterior a `agora`; exclui última decisão de retratação. Agrupa por `data_evento == dia` e posição/instância do marco. (`app/chamados/services/tarefa_do_dia.py:223–234`) |
| Decisão considerada | Sequência de `DecisaoPergunta`, ordenada por `(ato_em,pk)`, considerando somente `ato_em <= agora`; a última linha dessa sequência governa a ação. (`app/chamados/services/tarefa_do_dia.py:337–342`; `app/chamados/services/tarefa_do_dia.py:129–159`) |
| Dispensas do dia | `fora_vinculo`, isenção e competência trancada retornam sem ação. Sem célula retorna `nao_sei`. (`app/chamados/services/tarefa_do_dia.py:180–201`) |
| Ausência de lâmpadas | Em dia que não é dispensado por trabalho, sem instâncias: havendo marcos e nenhum vencido, retorna `nao_venceu`; caso contrário, `nao_sei/ata_sem_lampadas`. (`app/chamados/services/tarefa_do_dia.py:202–210`) |
| Dispensas por instância | Sem ação quando dia sem trabalho, lâmpada `True`, marco não vencido ou ausência aprovada. Lâmpada `None` ou teto desconhecido produzem `nao_sei`. (`app/chamados/services/tarefa_do_dia.py:241–260`) |
| Marco que exige pergunta | Lâmpada apagada, marco vencido, sem cobertura que desvie a ação e sem pergunta da posição: exige `PERGUNTAR`. (`app/chamados/services/tarefa_do_dia.py:258–279`) |
| Encerramentos pela sequência | Validação com lavra vigente `sim` e sem Batida de efeito: `encerrada_pelo_juiz`; silêncio: `silencio_decidido`; absorção em ausência aprovada: `absorvida_aprovada`. Todos retornam ações vazias. (`app/chamados/services/tarefa_do_dia.py:140–154`) |
| Demais ações | Resposta, recusa, reabertura, devolução e absorção não final podem exigir ação humana; validação com lavra `nao` exige materialização; efeito presente com lâmpada ainda apagada exige rejulgamento. (`app/chamados/services/tarefa_do_dia.py:129–159`) |

O `dia_sem_trabalho` compartilhado considera `trabalha is False` **ou** `ata.tipo_dia` preenchido e diferente de `trabalho`. Para ausência parcial que cobra o resto, somente `trabalha is False` dispensa o dia inteiro. (`app/chamados/juizes.py:413–441`; `app/ponto/supra_juiz.py:65–71`)

O vigia usado neste backfill é **`censo_alcance`**, selecionando `alcance_dias` com rota `sem_porta`; não é o `censo` antigo que começa em chamado vivo com célula. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:67–70`; `app/chamados/services/vigia_fio.py:493–499`)

| Aspecto | Predicado do vigia usado |
|---|---|
| Empresas | Empresas visíveis; o recorte solicitado é uma interseção. A fonte de visibilidade exige empresa ativa e exclui o prefixo de CNPJ declarado nela. (`app/chamados/services/vigia_fio.py:566–570`; `app/colaboradores/queries.py:78–87`) |
| Competências | A atual entra sempre no enumerador; a anterior entra se não estiver entregue. Não há teste de entrega da **atual** nesse enumerador. (`app/ponto/services/he_pendente_lavrado.py:218–225`) |
| Universo inicial | `dias_que_seguram`: lê `classificar_export`, seleciona colaboradores cujo motivo é `furo_espelho` e obtém seus dias pela retenção da célula. O censo deduplica `(colaborador,data)`. (`app/folha/fora_da_folha.py:95–99`; `app/folha/fora_da_folha.py:118–137`; `app/chamados/services/vigia_fio.py:581–588`) |
| Modelos/status da folha | `classificar_export` começa em `FechamentoMensal` da empresa/mês/ano, sem filtro de status; `furo_espelho` só é atribuído quando a cadeia chega ao teste da retenção. (`app/folha/export.py:757–780`; `app/folha/export.py:960–965`) |
| Retenção do dia | Exclui isento, recorta admissão/demissão e lê `CelulaDia` por `data__range`. Retém acusação, salvo protestos inteiramente não cobráveis ou exceção da pausa; também retém cegueira passada nas condições expressas. (`app/folha/export.py:1192–1206`; `app/folha/export.py:1224–1257`) |
| Vereditos acusadores | `furo`, `discordante`, `indefinida`, `cobranca_indevida`, `cobrado`, `nunca_bateu`, `fato_em_ausencia`. (`app/escala/models.py:1767–1768`) |
| Perguntas examinadas | Por **`celula_id`**, sem filtro explícito `conversa=False` ou `data_evento`; seleciona as vivas pelo C6 `pergunta_viva`. (`app/chamados/juizes.py:1502–1505`) |
| Porta alcançável, C17 | Chamado da célula na fila **ou** pergunta viva da célula, presente no universo do app, com âncora na fila e sem resposta vazia ilegal. (`app/chamados/juizes.py:1574–1589`) |
| Fila/status | `VIVOS = aberto, em_analise, registrado`; a fila exige `COBRANCA = aberto, em_analise` e exclui `silenciado_ate > agora`. (`app/chamados/catalogo/motor.py:24–31`; `app/chamados/services/painel.py:188–190`) |
| Exclusões antes de contar morto | Havendo porta, sai; hoje/futuro sai; depois saem Resolver dia oferecido, ausência aberta cobrindo, aceite de folga validado e dia na lista CxR com dono BATIDA/CADASTRO. (`app/chamados/services/vigia_fio.py:635–660`; `app/chamados/services/vigia_fio.py:306–315`) |

O C6 lê projeção/carimbos, tranca, vias, resposta, veredito da célula, fato do marco e materialização. Seu corpo não decide pela última `DecisaoPergunta`, como faz a tarefa. (`app/chamados/juizes.py:1324–1410`; `app/chamados/services/tarefa_do_dia.py:129–159`)

A condição exata da divergência é a conjunção:

- **“Morto”**: dia no universo retido, C17 sem porta, `data < hoje` e nenhum dos quatro baldes de dono.
- **`tarefa_nada`**: redução do command sem ato de sistema/passivo, sem ação com porta nula e `nao_sei=False`.

Assim, retenção agregada e ausência de porta podem coexistir com dispensa ou encerramento das ações por marco. O código desses dois caminhos não exige equivalência entre as respostas. (`app/chamados/services/vigia_fio.py:635–699`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:78–98`; `app/chamados/services/tarefa_do_dia.py:180–288`)

Para ser especificamente **classe `d`**, ainda é necessário: célula existente; nenhum `veu` de chamado vivo da célula ou âncora viva fora da fila; nenhuma pergunta C6 viva ligada à célula; dia fora da lista CxR; colaborador presente na lista CxR. Isso não significa “nenhuma pergunta histórica existe para o fato”. (`app/chamados/services/vigia_fio.py:661–678`; `app/chamados/services/fabrica_da_celula.py:145–157`)

Um caminho suficiente demonstrável é: dia que permanece retido e sem porta/dono, mas cujo juiz de tranca responde verdadeiro; a tarefa retorna sem ação por `competencia_trancada`, enquanto a retenção continua lendo veredito/protestos. Ocorrência desse caminho nos 116: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:190–198`; `app/folha/export.py:1233–1256`; `app/chamados/services/vigia_fio.py:635–699`)

**Existe juiz registrado:** `_C19` aponta para `tarefa_do_dia`; o command chama essa autoridade. O vigia consulta C17/C6 e a cadeia de dono, sem chamar C19 no corpo de `censo_alcance`. O registro declara que os cinco respondedores anteriores continuam vivos durante essa etapa. (`app/core/juizes.py:150–155`; `app/core/juizes.py:289–292`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:75–76`; `app/chamados/services/vigia_fio.py:555–564`; `app/chamados/services/vigia_fio.py:635–656`)

**P2. Ramos da fábrica sem inserção de `PerguntaDisputa`.**

Primeiro, o limite da medição: **`segue_sem_porta` não prova zero perguntas criadas nem zero perguntas vivas depois do ato**. O command descarta o retorno da fábrica e classifica pelo reaparecimento do `celula_id` no recenso `sem_porta`; a classe/detalhe impressos são os guardados no plano anterior. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:116–117`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:195–196`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:240–258`)

O caminho é `perguntar_pela_celula` → `motivos_a_perguntar` → `motivos_pela_ata`; com motivos, segue por `perguntar` → `criar_ou_atualizar_disputa` → `gerar_perguntas_disputa`. Depois de obter a disputa, `perguntar` chama o gerador novamente. (`app/chamados/services/fabrica_da_celula.py:160–175`; `app/chamados/services/fabrica_da_celula.py:201–220`; `app/chamados/services/disputa_emissao.py:669–688`)

**Nenhum retorno normal abaixo é distinguido pelo command:** tanto `[]` quanto uma lista de motivos são descartados. Exceção que escapa recebe tratamento diferente: rollback do dia e `recusa:<tipo>`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:116–117`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:236–239`)

| Ramo | Condição e saída |
|---|---|
| Sem adesão | `veredito == 'nunca_bateu'`: retorno imediato `[]`, inclusive antes da pauta de cadastro. Esse veredito pertence ao conjunto acusador da retenção. (`app/chamados/services/fabrica_da_celula.py:87–89`; `app/chamados/services/fabrica_da_celula.py:183–184`; `app/escala/models.py:1767–1768`) |
| Passado sem turno previsto | Dia passado, colaborador ativo, não dispensado por `dia_sem_trabalho`, e nenhum marco vencido **nem pendente**: `vencidos_no_instante` devolve `None`; tenta pauta de cadastro e retorna `[]`. Vínculo/tipo ausente ou cadeia vazia podem produzir ausência de marcos. (`app/chamados/services/fabrica_da_celula.py:45–49`; `app/chamados/services/fabrica_da_celula.py:201–205`; `app/ponto/services/cartorio.py:840–859`) |
| Pauta de cadastro já entregue | Após a trava, a pauta não é repetida se existe cabeça viva do colaborador ou pauta fechada em data civil local `>= dia`. O booleano do escritor é ignorado; com pauta nova ou antiga a fábrica retorna `[]`. (`app/chamados/services/fabrica_da_celula.py:327–335`; `app/chamados/services/fabrica_da_celula.py:364–373`; `app/chamados/services/fabrica_da_celula.py:204–205`) |
| Guardas gerais | `dia_sem_trabalho`, situação atual diferente de `ativo` ou dia futuro: motivos vazios. Veredito preenchido fora de `VEREDITOS_ACUSAM` também devolve vazio; veredito vazio não cai nessa guarda. (`app/chamados/services/fabrica_da_celula.py:105–121`) |
| Parcial somente na lista | `cobertura_parcial` com `periodo_parcial == 'sem_periodo'`; ou parcial com julgamento anterior e sem fato novo desde ele. O command não passa `julgada_antes`, portanto a fábrica usa `cel.julgada_em`. Retorna motivos vazios. (`app/ponto/services/cartorio.py:395–400`; `app/chamados/services/fabrica_da_celula.py:123–125`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:117`) |
| Nenhum marco venceu | `motivos_pela_ata` retorna `[]` para teto vazio ou `None`. Havendo somente marcos pendentes, não abre pauta de falta de turno: existem marcos, apenas nenhum venceu. (`app/chamados/services/fabrica_da_celula.py:45–49`; `app/chamados/services/fabrica_da_celula.py:58–61`) |
| Exceção engolida ao ler ata | `_ata_do_dia` captura a exceção e retorna `(True,None)`. Apesar do comentário “falha de leitura não barra”, este chamador barra porque `faltantes is None`. (`app/chamados/services/disputa_emissao.py:894–901`; `app/chamados/services/fabrica_da_celula.py:64–66`) |
| Ata ausente/agregada | Estado `sem_celula` com escala vigente retorna `(False,None)`; sem escala retorna `(True,None)`. Ambos impedem motivos neste caminho. A ausência de pares de marcos no leitor também pode produzir `sem_celula`. (`app/chamados/services/disputa_emissao.py:902–913`; `app/chamados/services/declaracao_texto.py:121–127`; `app/chamados/services/fabrica_da_celula.py:65–66`) |
| Nenhum faltante elegível | O leitor exclui isento/fora do vínculo, dia não trabalho sem parcial cobrando resto, marcos sem E/S ou hora e células sem `missing`. A fábrica exige adicionalmente marco vencido, data igual à célula e posição/tipo mapeados. A contribuição da véspera não passa por `f[0] == dia`. (`app/chamados/services/declaracao_texto.py:101–143`; `app/chamados/services/fabrica_da_celula.py:68–74`) |
| Dedup anterior à emissão | Remove motivos que já têm linha do fato: colaborador, dia, motivo, instância, `conversa=False`, fora `duplicata/motivo_retratado`. **Não exige vitalidade, disputa aberta ou ausência de validação/materialização.** Se todos foram removidos, não chama `perguntar`; ainda pode tentar desfazer carimbos existentes. (`app/chamados/services/fabrica_da_celula.py:145–157`; `app/chamados/models.py:2746–2755`; `app/chamados/models.py:2393`; `app/chamados/services/fabrica_da_celula.py:207–220`) |
| Limbo entregue | `trabalha is None`, motivos presentes e competência entregue: pauta DP e `[]`. Dedup da pauta considera cabeça do dia, DP e marcador, inclusive feita/cancelada. (`app/chamados/services/fabrica_da_celula.py:214–231`; `app/chamados/services/fabrica_da_celula.py:257–285`) |

Esses ramos não têm todos a mesma possibilidade de explicar os 64: dia futuro sai do vigia antes de `sem_porta`; entrega detectada pelo backfill sai como `bang:*` antes da fábrica. Em estado estável, essas condições não explicam um `segue_sem_porta` executado. A participação das demais guardas nos 64 é **[nao verificado]**. (`app/chamados/services/vigia_fio.py:648–651`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:194–197`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:227–233`)

Mesmo recebendo motivos, o gerador ainda pode terminar sem inserir:

| Ramo interno | Condição |
|---|---|
| Catálogo/texto/data | Motivo sem pergunta no catálogo; texto datado sem data e sem fallback da grade; pergunta de hora sem dia do turno: `continue`. A leitura de fallback da grade também captura exceção e devolve `None`. (`app/chamados/services/disputa_emissao.py:75–89`; `app/chamados/services/disputa_emissao.py:314–316`; `app/chamados/services/disputa_emissao.py:346–366`; `app/chamados/services/disputa_emissao.py:425–430`) |
| Dedup genérico | Motivo não datado sem episódio já existente; intervalo sem data ou já existente na disputa/dia; conversa repetida: `continue`. Os quatro motivos normais da fábrica são datados, de hora, e recebem contexto ISO do dia; os ramos não datados/conversa não são seu caminho normal. (`app/chamados/services/disputa_emissao.py:26`; `app/chamados/services/disputa_emissao.py:325–329`; `app/chamados/services/disputa_emissao.py:395–401`; `app/chamados/services/disputa_emissao.py:431–438`; `app/chamados/services/fabrica_da_celula.py:172`; `app/chamados/services/declaracao_texto.py:42–47`; `app/chamados/catalogo/perguntas.py:48–58`; `app/chamados/catalogo/perguntas.py:124–134`) |
| Dedup com destino não derivado | Para motivos que exigem destino, zero ou múltiplos candidatos devolvem `(None,None)`. O dedup seguinte usa **`data_evento=_data_ev`**, antes de escolher o fallback. Assim, uma pergunta legada da mesma disputa/motivo com data nula pode impedir a emissão de um dia cujo fallback é válido. Ocorrência nos 64: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:238–244`; `app/chamados/services/disputa_emissao.py:410–424`) |
| Reuso na porta final | `PerguntaDisputa.abrir` pode encontrar uma canônica do fato, inclusive em outra disputa, e devolver a existente com `criada=False`. O gerador acrescenta essa pergunta à lista `criadas` mesmo sem inserção; a fábrica devolve motivos, não essa lista. (`app/chamados/models.py:2776–2781`; `app/chamados/models.py:2905–2922`; `app/chamados/models.py:3008`; `app/chamados/services/disputa_emissao.py:444–471`; `app/chamados/services/fabrica_da_celula.py:219–220`) |

Não achei filtro geral de período de apuração em `motivos_a_perguntar` ou `gerar_perguntas_disputa`; procurei os corpos completos dessas funções. Os recortes encontrados são dia/teto, parcial, identidade/dedup e encaminhamento específico do limbo entregue; no reuso há também a porta de rechaveamento. (`app/chamados/services/fabrica_da_celula.py:92–157`; `app/chamados/services/fabrica_da_celula.py:214–231`; `app/chamados/services/disputa_emissao.py:300–471`; `app/chamados/models.py:2940–2956`)

Recusa de destino cruzado, fato em outro dia, batida retratada, rechaveamento recusado ou contrato de saída **levanta exceção**; quando escapa, produz `recusa:<tipo>`, não `segue_sem_porta`. (`app/chamados/models.py:2818–2822`; `app/chamados/models.py:2875–2890`; `app/chamados/models.py:2909–2926`; `app/chamados/models.py:2952–2956`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:236–239`)

As exceções engolidas de reatar órfã e de registrar nascimento ocorrem **depois** das inserções. A primeira pode deixar a pergunta órfã, mas não é um ramo de ausência de criação. (`app/chamados/services/disputa_emissao.py:459–470`; `app/chamados/services/disputa_emissao.py:587–603`; `app/chamados/models.py:3014–3031`)

**P3. Escritores, leitores e significado dos insumos.**

A tabela identifica as portas escritoras e os leitores pertinentes aos caminhos acima; não atribui a elas a autoria dos valores concretos da medição, que é **[nao verificado]**.

| Campo/família | Quem escreve ou produz | Quem lê / significado |
|---|---|---|
| `CelulaDia.data`, `trabalha`, DNA/geradora | Geração/regeneração e edição pelas portas da célula. (`app/ponto/portas/celula.py:243–272`; `app/ponto/portas/celula.py:534`; `app/ponto/portas/celula.py:603–604`; `app/ponto/portas/celula.py:816–875`) | Retenção lê por data; tarefa e fábrica usam o vínculo/marcos do dia. `trabalha=None` não equivale a folga no juiz de cobrança. (`app/folha/export.py:1206`; `app/ponto/turnos.py:3049–3062`; `app/ponto/services/cartorio.py:840–859`; `app/chamados/juizes.py:426–441`) |
| `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata` | `lavrar_veredito`; a ata é produzida por `ata_do_dia`. (`app/ponto/portas/celula.py:974–1040`; `app/ponto/services/cartorio.py:696–738`) | Retenção lê acusação/protestos; C6 lê veredito/via; tarefa e fábrica leem a ata por finalidades distintas. (`app/folha/export.py:1233–1256`; `app/chamados/juizes.py:1338–1387`; `app/chamados/services/tarefa_do_dia.py:84–102`; `app/chamados/services/fabrica_da_celula.py:120–139`) |
| `ata.lampadas`: tipo/hora/acesa/luz | `ata_do_dia` chama o produtor das lâmpadas e as inclui na ata. (`app/ponto/services/cartorio.py:701–713`) | O leitor produz `missing` e `acesa_lavrada`; fábrica usa o primeiro, tarefa usa o segundo. `acesa=None` vira `missing=Fals

[truncado em 20000 de 33148 caracteres]

## 10/11 23:10 · sessao f3099e60799a · - · - · origem=Ronald (tmux)

Matriz complementar P3, somente leitura; escritores abaixo são os encontrados nos caminhos examinados, sem afirmar censo exaustivo de todos os chamadores da árvore. [não verificado]

| Campo/função | Quem escreve/produz | Quem lê | Significado e diferenças |
|---|---|---|---|
| `Ausencia.tipo/data/data_fim/status` | Nascimento em `criar_ausencia`: deriva fim e monta `kwargs`, incluindo `status` explícito ou inicial; instancia e salva a ausência (`app/ponto/services/ausencia.py:197`, `:331`, `:341`, `:385`, `:418`). Edição de tipo/range em `editar_ausencia_registro` (`:465`, `:477`, `:498`, `:509`). Transição de status em `mudar_estado` (`:808`, `:824`, `:835`). | Carga da tarefa chama `_pelo_efeito` + `_q_cobertura` e `_cobertura_pura` (`app/ponto/turnos.py:3064`, `:3072`); tarefa examina status (`app/chamados/services/tarefa_do_dia.py:258`). Vigia consulta cobertura na cadeia de dono (`app/chamados/services/vigia_fio.py:274` e bloco correspondente abaixo, já no agente P1). | Para cobrança, exclui apenas `rejeitada`; para folha, exige `aprovada` (`app/ponto/turnos.py:2511`). Cobertura por data é inclusiva; sem fim, estende só tipos abertos; empate vence maior `(data,pk)` pela ordem e sobrescrita (`:2753`, `:2757`, `:2765`, `:2780`). `aguardando_documento` é bola colaborador; `aguardando_decisao` é DP (`app/ponto/services/ausencia.py:779`, `:791`, `:795`); `ABERTAS` contém esses dois, sem `pendente` (`:765`). |
| `PeriodoFechado` / tranca | `trancar_periodo` cria/retranca e limpa reabertura (`app/ponto/services/fechamento.py:1400`, `:1408`, `:1412`). `reabrir_periodo` grava `ativo=False` e `reaberto_em` (`:1432`, `:1437`). | `periodo_trancado` procura `reaberto_em IS NULL` (`:1464`, `:1470`); `dias_trancados` mapeia dia do turno à competência com empresa e chama esse juiz (`app/chamados/juizes.py:966`, `:1024`, `:1028`); tarefa carrega resultado em lote (`app/chamados/services/tarefa_do_dia.py:365`). | `ativo` é documento; a leitura da tranca usa `reaberto_em` (`app/ponto/services/fechamento.py:1464`). Tranca e exportação são perguntas distintas; a própria descrição separa aprovada/exportada/trancada/paga (`:1455`). |
| `ExportacaoDominio` / entregue | `ExportacaoDominio.registrar` cria histórico por geração (`app/folha/models.py:174`, `:182`). `invalidar` carimba invalidada/substituta (`:143`, `:158`, `:162`). | `competencia_entregue` usa vigente por empresa/mês/ano e recusa vigente invalidada (`app/folha/export.py:556`, `:580`). Composição dia→competência chama `janela_atual(dia, empresa)` (`app/chamados/services/vigia_fio.py:822`, `:838`). Backfill lê antes e novamente sob trava (`app/chamados/management/commands/backfill_fio_morto_e4b.py:194`, `:227`); fábrica lê para limbo (`app/chamados/services/fabrica_da_celula.py:223`, `:230`). | Competência é do dia, não mês civil, e usa corte da empresa (`app/chamados/services/vigia_fio.py:838`; `app/ponto/janelas.py:60`). |
| Lista CxR / `dono_do_dia` / presença colab e dia | `cadastro_realidade.lista` monta dicionários de fontes atuais e lavras, unifica donos em memória e retorna `postos` (`app/escala/services/cadastro_realidade.py:787`, `:800`, `:885`, `:886`, `:926`, `:970`). `um_dono_por_dia` reconsulta o juiz quando assinaturas divergem e atribui `a['dono_do_dia']` em memória (`:985`, `:1017`, `:1024`, `:1028`). Não achei persistência da **lista montada** nesses corpos; suas fontes incluem lavras persistidas (`:793`, `:886`). | `dias_na_lista` extrai somente assinaturas com `data`, resolvendo dono (`:1044`, `:1058`). `ListaDoAdmin` lê lista, guarda mapa de dias e conjunto de colaboradores (`app/chamados/services/vigia_fio.py:205`, `:216`, `:218`); `dia_na_lista` e `colab_na_lista` são leituras separadas (`:230`, `:248`). | Colaborador constar na lista não implica que o dia conste: assinatura de vínculo inteiro não nomeia dia (`app/escala/services/cadastro_realidade.py:1044`). Classe `d` usa precisamente ausência do dia mais presença do colab (`app/chamados/services/vigia_fio.py:677`). |
| Cadastro: `situacao`, `data_demissao`, `data_admissao`, `isento_ponto`, empresa | Porta central declara operações e salva: `gravar`, `salvar_inteiro`, `criar_colaborador` (`app/colaboradores/services/colaborador.py:23`, `:55`, `:73`, `:91`). Desligamento/reativação atribuem situação/data_demissão e salvam pela porta (`app/colaboradores/services/desligamento.py:103`, `:210`). Tela atribui admissão e empresa (`app/colaboradores/views.py:1313`); nascimento recebe admissão e ativo (`:1188`, `:1205`); Passe Livre grava flag pela porta (`:176`). | Leitor da célula lê admissão/demissão e flag (`app/escala/services/leitor_celula.py:358`, `:365`); tarefa usa esse leitor (`app/chamados/services/tarefa_do_dia.py:380`); fábrica exige situação atual ativo (`app/chamados/services/fabrica_da_celula.py:112`). | Leitor considera fora antes da admissão ou após demissão; no dia da demissão, depende de fato lavrado (`app/escala/services/leitor_celula.py:385`, `:389`). Fábrica usa status cadastral atual, sem reconstruir situação no dia (`app/chamados/services/fabrica_da_celula.py:112`). Portanto os dois não aplicam o mesmo predicado de elegibilidade (`app/escala/services/leitor_celula.py:389`; `app/chamados/services/fabrica_da_celula.py:112`). |

Pontos temporais que merecem aparecer na consolidação:

- O backfill separa `agora=timezone.localtime()` de `hoje=--hoje` opcional: vigia recebe `hoje`, tarefa recebe `agora`, e fábrica recebe ambos. Portanto `--hoje` não rebobina o relógio da tarefa/teto; se isso foi usado na medição, [não verificado]. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:169`, `:176`, `:193`, `:117`.)
- Vigia `censo_alcance` chama `alimentar_portas` sem `agora`; alimentação chama C6 sem `hoje`; `ListaDoAdmin` criada dentro desse censo também não recebe `agora`. A tarefa injeta `agora` na fila e corta decisões por ele. (`app/chamados/services/vigia_fio.py:604`, `:623`; `app/chamados/juizes.py:1497`, `:1504`; `app/chamados/services/tarefa_do_dia.py:337`, `:363`.)
- Janela da competência é fechada nos dois extremos: corte do mês anterior até corte−1 do mês nomeado (`app/ponto/janelas.py:10`); tarefa exige `ini <= dia <= fim` (`app/chamados/services/tarefa_do_dia.py:398`); retenção folha usa `data__range` e laço `<= fim` (`app/folha/export.py:1206`, `:1225`). Não achei borda exclusiva nesses três caminhos examinados; teto de marco é inclusivo em `marco + 30min <= agora` (`app/ponto/services/cartorio.py:828`, `:858`).
- C6 fallback `_dentro_da_janela_viva` chama janelas **sem empresa**, portanto usa corte default 21; a expiração da lavra faz o mesmo. Já entregue e tranca usam empresa. Isso é diferença verificável de parâmetros, sem demonstrar diferença efetiva para as empresas da medição. (`app/chamados/juizes.py:578`, `:597`; `app/chamados/veredito_lavrado.py:143`; `app/ponto/janelas.py:36`, `:50`; `app/chamados/services/vigia_fio.py:838`; `app/chamados/juizes.py:1024`.)
- C6 fallback sem `hoje` usa `datetime.date.today()`; bordas e expiração usam data local Django. Settings-base define São Paulo e `USE_TZ=True`; fuso efetivo do processo/prod e coincidência de datas nesta rodada são [não verificado]. (`app/chamados/juizes.py:577`; `app/chamados/veredito_lavrado.py:95`, `:104`; `app/config/settings/base.py:93`.)
- `CargaDaTarefa` declara mistura explícita de horizonte: decisões/lavra/efeito cortados por `agora`, mas identidade/pergunta/ata/cobertura/status/fila/cadastro/vínculo/tranca atuais. Logo não é fotografia histórica integral. (`app/chamados/services/tarefa_do_dia.py:301`.)

## 10/11 23:09 · sessao 60d5ae8dd978 · - · - · origem=Ronald (tmux)

P1 — rastreamento do código, sem inferir a distribuição interna dos 116 dias:

- O command usa **`tarefa_do_dia`**, chamada por `ato_do_dia`; o rótulo `tarefa_nada` é construído pelo próprio command quando não acumulou atos de sistema/passivo, não acumulou ações com `porta is None` e `t['nao_sei']` é falso. Portanto o rótulo, isoladamente, não prova que `acoes == []`: ações humanas **com porta preenchida** também não entram em `humanos`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:73–98`)
- A tarefa efetivamente julgada é `_tarefa_pura`, chamada por `CargaDaTarefa.tarefa`, com cadastro, grade, instâncias de marco, cobertura, perguntas, decisões, efeitos, fila, passivo, lavra e tranca. (`app/chamados/services/tarefa_do_dia.py:396–406`)
- O universo das perguntas da tarefa é `PerguntaDisputa(colaborador_id__in=ids, conversa=False, data_evento__gte=ini, data_evento__lte=fim)`, ordenado por colaborador e pk; não exige pergunta viva, disputa aberta ou carimbo não validado. (`app/chamados/services/tarefa_do_dia.py:323–329`)
- Dentro desse universo, só considera motivos de marco; exclui `VIAS_FORA_DO_FATO`, salvo retratação posterior ao `agora`, e exclui última decisão de retratação; associa pelo `data_evento == dia` e pela instância/posição do motivo. (`app/chamados/services/tarefa_do_dia.py:223–234`)
- A decisão considerada é a última linha em ordem `(ato_em, pk)` com `ato_em <= agora`; os atos posteriores só alimentam o conjunto de retratações posteriores. (`app/chamados/services/tarefa_do_dia.py:337–342`)

Predicado da tarefa:

- Retorna nada para `fora_vinculo`, isento ou competência trancada; sem célula retorna `nao_sei`. (`app/chamados/services/tarefa_do_dia.py:180–201`)
- Em dia de trabalho sem instâncias de lâmpada, retorna nada se existem marcos e nenhum venceu; caso contrário retorna `nao_sei/ata_sem_lampadas`. (`app/chamados/services/tarefa_do_dia.py:202–210`)
- Por instância, não exige ação se `dia_sem_trabalho`, lâmpada `True`, marco ainda não vencido ou ausência aprovada; lâmpada `None` ou marco sem teto produzem `nao_sei`. (`app/chamados/services/tarefa_do_dia.py:241–260`)
- Para marco apagado, vencido e não coberto, sem pergunta da identidade exige `PERGUNTAR`; havendo pergunta, julga a última decisão. (`app/chamados/services/tarefa_do_dia.py:268–288`)
- Última validação com lavra vigente `sim` e sem efeito de Batida produz `encerrada_pelo_juiz/[]`; silêncio produz `silencio_decidido/[]`; absorção com ausência aprovada produz `absorvida_aprovada/[]`. (`app/chamados/services/tarefa_do_dia.py:140–154`)
- Última resposta, recusa, reabertura, devolução ou absorção não final gera ação humana; validação sem efeito gera materialização, e efeito sem lâmpada gera rejulgamento. (`app/chamados/services/tarefa_do_dia.py:129–159`)
- `dia_sem_trabalho` lê `trabalha is False` ou `ata.tipo_dia` preenchido e diferente de trabalho; ausência parcial com resto cobravel só é calada por `trabalha is False`. (`app/chamados/juizes.py:426–441`)

Predicado do vigia usado pelo backfill:

- O command usa **`censo_alcance`**, selecionando `alcance_dias` cuja rota é `sem_porta`; não usa o `censo` legado para definir seu plano. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:67–70`)
- O universo são os dias retornados por `dias_que_seguram` para cada empresa visível e cada competência de `competencias_a_lavrar`, deduplicados por `(colab_id, data)`. (`app/chamados/services/vigia_fio.py:581–588`)
- Empresas visíveis são ativas, excluindo CNPJ iniciado por `11222333`. (`app/colaboradores/queries.py:78–87`)
- Competências são a atual e a anterior se esta não estiver entregue; a atual entra sem teste de entregue nesse enumerador. (`app/ponto/services/he_pendente_lavrado.py:218–225`)
- `dias_que_seguram` parte de `classificar_export`, retém somente colaboradores com motivo `furo_espelho`, depois obtém seus dias pela mesma `motivos_retencao_celula`. (`app/folha/fora_da_folha.py:118–137`; `app/folha/fora_da_folha.py:95–99`)
- `classificar_export` lê `FechamentoMensal` da empresa/mês/ano sem filtro de status; sua cadeia pode classificar antes por rescisão, cadastro etc., e só atribui `furo_espelho` quando chega ao teste dos motivos de retenção. (`app/folha/export.py:757–780`; `app/folha/export.py:960–965`)
- Retenção usa `CelulaDia` por `data__range=(ini,fim)`, recorta admissão/demissão e exclui isento; retém célula acusadora salvo todos os protestos não cobraveis ou exceção da pausa; também retém cegueira passada, sob as condições do código. (`app/folha/export.py:1192–1206`; `app/folha/export.py:1224–1257`)
- Acusadores são `furo`, `discordante`, `indefinida`, `cobranca_indevida`, `cobrado`, `nunca_bateu`, `fato_em_ausencia`; o conjunto não cobravel contém `FURO_PRE_ADOCAO`. (`app/escala/models.py:1767–1768`; `app/ponto/supra_juiz.py:34–36`)
- O vigia pergunta ao C17 `porta_alcancavel_do_dia`; havendo porta, sai do morto; dia de hoje/futuro também sai; depois saem os quatro baldes de `dono_fora_do_fio`. O restante incrementa `fio_morto_alcance` e recebe `sem_porta`. (`app/chamados/services/vigia_fio.py:635–660`; `app/chamados/services/vigia_fio.py:695–699`)
- Os quatro baldes são Resolver dia oferecido; ausência cobrindo com status em `ABERTAS`; aceite de folga validado; dia na lista CxR com dono BATIDA/CADASTRO. (`app/chamados/services/vigia_fio.py:297–315`)
- C17 considera porta um chamado da célula na fila ou pergunta viva da célula, no app, com âncora na fila e sem resposta vazia ilegal. (`app/chamados/juizes.py:1574–1589`)
- A fila exige status em `COBRANCA` e exclui silêncio no futuro; `COBRANCA=('aberto','em_analise')`, enquanto `VIVOS` também inclui `registrado`. (`app/chamados/services/painel.py:188–190`; `app/chamados/catalogo/motor.py:24–31`)
- As perguntas do vigia são carregadas **por `celula_id`**, sem `conversa=False` nem filtro de `data_evento`; a seleção de vivas usa C6. (`app/chamados/juizes.py:1502–1505`)
- C6 aplica estado da projeção/carimbos, tranca, vias, resposta, veredito da célula, lâmpada do marco, classificador e materialização; ele não usa a sequência de decisões da tarefa para responder. (`app/chamados/juizes.py:1324–1410`)

Condição exata de divergência:

`morto = dia retido ∧ sem C17 ∧ data < hoje ∧ nenhum balde de dono`; simultaneamente `tarefa_nada = sem ato de sistema/passivo ∧ sem ação com porta None ∧ nao_sei=False`. O primeiro testa retenção agregada e alcançabilidade por célula; o segundo testa exigibilidade por instância/decisão e depois passa pela redução do command. Não existe equivalência imposta entre os predicados. (`app/chamados/services/vigia_fio.py:635–699`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:78–98`; `app/chamados/services/tarefa_do_dia.py:235–288`)

Para a **classe d**, além do predicado de morto: existe célula; não existe `veu` de chamado vivo da célula ou âncora viva fora da fila; `ps` vivas está vazio; `dia_na_lista == (False,None)`; o colaborador aparece na lista CxR. Isso não identifica a razão da tarefa dos 116 dias; não achei essa distribuição no dado fornecido. (`app/chamados/services/vigia_fio.py:661–678`; [não verificado quanto à distribuição dos 116])

Juízes:

- C19 está registrado para a pergunta das ações exigíveis e aponta a `tarefa_do_dia`; o command o lê. (`app/core/juizes.py:150–155`; `app/core/juizes.py:291`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:75–76`)
- O vigia lê C17, C6 e os baldes de dono; não chama C19 em `censo_alcance`. (`app/chamados/services/vigia_fio.py:555–564`; `app/chamados/services/vigia_fio.py:604–610`; `app/chamados/services/vigia_fio.py:635–656`)
- O registro declara que os cinco respondedores anteriores seguem vivos na E2; não prova que todos os leitores do HEAD já migraram. (`app/core/juizes.py:153–154`)
- C17, C6, C15 e C18 também estão registrados. (`app/core/juizes.py:278–291`)

Nuances de significado/relógio para P3:

- A tarefa usa dia do turno em `data_evento`; calcula `data_civil` separadamente a partir do instante local do marco. (`app/chamados/services/tarefa_do_dia.py:213–215`; `app/chamados/services/tarefa_do_dia.py:91–102`)
- A tarefa lê perguntas por colaborador/data do turno; o vigia lê perguntas ligadas à célula. Uma pergunta com chave ou FK divergente pode entrar num universo e não no outro; ocorrência nos dados fornecidos é [não verificado]. (`app/chamados/services/tarefa_do_dia.py:324–329`; `app/chamados/juizes.py:1502–1505`)
- O backfill captura `agora=timezone.localtime()` uma vez; `hoje` pode ser sobrescrito independentemente por `--hoje`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:169–170`)
- Esse `agora` chega à tarefa e à fábrica, mas `_alcance` recebe apenas `hoje`; `censo_alcance` chama `alimentar_portas` sem `agora`, C6 sem `hoje`, e cria `ListaDoAdmin` sem `agora`. Logo o vigia não está inteiramente congelado no mesmo instante da tarefa. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:67–69`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:117`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:193`; `app/chamados/services/vigia_fio.py:604`; `app/chamados/services/vigia_fio.py:623`; `app/chamados/juizes.py:1504`)
- Sem `agora`, a fila usa `timezone.now()`; a retenção chamada pelo vigia também não recebe `hoje`, usando `timezone.localdate()` internamente. Isso permite diferença temporal sem demonstrá-la nos 116/64 casos. (`app/chamados/services/painel.py:188–190`; `app/folha/fora_da_folha.py:95–99`; `app/folha/export.py:1197`)
- Os intervalos de dias da tarefa e da retenção são inclusivos; os marcos podem cair em outro dia civil. (`app/chamados/services/tarefa_do_dia.py:325–326`; `app/chamados/services/tarefa_do_dia.py:398–399`; `app/folha/export.py:1206`; `app/folha/export.py:1225`; `app/chamados/services/tarefa_do_dia.py:102`)
- O leitor da grade preserva a lâmpada triestado `acesa_lavrada`, enquanto seu campo antigo `missing` trata `acesa=None` como acesa; a tarefa escolhe explicitamente o triestado. (`app/escala/services/leitor_celula.py:119–137`; `app/chamados/services/tarefa_do_dia.py:84–88`)
- O leitor pode declarar `fora_vinculo` no dia da demissão sem fato lavrado, enquanto a retenção ainda retém célula acusadora nesse dia; este é um caminho de divergência permitido pelo código, participação nos 116 dias [não verificado]. (`app/escala/services/leitor_celula.py:385–393`; `app/folha/export.py:1229–1254`)

_3538 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
