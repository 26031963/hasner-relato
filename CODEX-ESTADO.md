# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 12:19** · publicado em **11/11/2026 12:20** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 12:19 · sessao ba041376f50f · - · - · origem=Ronald (tmux)

Q3 — `DESTINOS` reproduz os cinco seletores e suas ordens: quatro destinos nos dois detalhes e três nos demais; o patch fornecido preserva os `value` e a ordem da base. Fontes atuais: `app/chamados/tests/test_bo_rotulo_encerrado.py:64`, `app/templates/chamados/detalhe.html:130`, `app/templates/chamados/detalhe_local.html:393`, `app/templates/chamados/fio_colaborador.html:265`, `app/templates/chamados/partials/modal_fio.html:665`, `app/templates/chamados/partials/painel_chamado_turno.html:74`. Comparação da base decorre do anexo fornecido; checkout da base `[nao verificado]`.

Isso **não equivale a uma lista de transições sempre aceitas**. Os receptores locais reconhecem todo `STATUS_CHOICES` e delegam ao modelo (`app/chamados/services/acoes_chamado.py:287`, `app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/fio.py:502`, `app/chamados/services/fio.py:719`). O modelo delega a decisão geométrica ao motor e aplica guardas de conteúdo (`app/chamados/models.py:119`, `app/chamados/models.py:128`, `app/chamados/models.py:135`, `app/chamados/models.py:148`, `app/chamados/models.py:181`).

**Fila — “Reabrir” pode ser recusado:** o rótulo envia `aberto`; `resolvido → aberto` não pertence à matriz humana comum, embora `em_analise → aberto` pertença. [nome] tem exceção; destino igual à origem continua recusado (`app/chamados/services/verbos.py:136`, `app/chamados/catalogo/motor.py:99`, `app/chamados/catalogo/motor.py:148`, `app/chamados/catalogo/motor.py:153`).

**Fila — detalhe GLPI oferece status sem aplicá-lo:** o formulário envia ao followup, cujo receptor lê somente conteúdo e envia a mensagem ao GLPI; não lê `novo_status` (`app/templates/chamados/detalhe.html:122`, `app/chamados/urls.py:12`, `app/chamados/views.py:292`, `app/chamados/views.py:304`, `app/chamados/views.py:310`).

Q4 — escritores/leitores:

- **Código/value:** chamadas literais dos cinco templates → `opcao_mover(status_local)` → `value=status_local`; receptores leem o código, verificam `STATUS_CHOICES`, chamam `transicionar` e finalmente `_gravar_estado` grava esse código. Nenhum desses caminhos interpreta o rótulo como código (`app/chamados/templatetags/chamados_tags.py:68`, `app/chamados/templatetags/chamados_tags.py:77`, `app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/fio.py:719`, `app/chamados/models.py:199`, `app/chamados/models.py:228`, `app/chamados/models.py:447`).
- **Rótulo da opção:** definição única em `MOVER_PARA`; `rotulo_de_movimento` lê pelo código e a tag escreve o texto HTML (`app/chamados/services/verbos.py:132`, `app/chamados/services/verbos.py:146`, `app/chamados/templatetags/chamados_tags.py:76`).
- **Title:** definição compartilhada `ESTADO_TITLE`; lida por `rotulo_de_movimento`, `rotulo_de_linha` e `rotulo_para_pessoa`; a tag escreve o atributo, enquanto as pílulas usam `esp.1` (`app/chamados/services/verbos.py:100`, `app/chamados/services/verbos.py:146`, `app/chamados/services/verbos.py:155`, `app/chamados/services/verbos.py:166`, `app/templates/chamados/detalhe_local.html:70`).
- **Filtro `espera`:** permanece delegado a `rotulo_para_pessoa`, que usa `ESPERA_COLAB`, onde resolvido/fechado compartilham “Encerrado”; continua nas pílulas e deixou as opções alteradas pelo patch. Isso responde à apresentação do estado atual, distinta da opção de movimento (`app/chamados/templatetags/chamados_tags.py:53`, `app/chamados/templatetags/chamados_tags.py:64`, `app/chamados/services/verbos.py:114`, `app/chamados/services/verbos.py:162`, `app/templates/chamados/detalhe.html:36`, `app/templates/chamados/detalhe_local.html:68`, `app/templates/chamados/fio_colaborador.html:188`, `app/templates/chamados/partials/modal_fio.html:102`, `app/templates/chamados/partials/painel_chamado_turno.html:18`).

Q5 — não vejo **segundo juiz de transição** introduzido: o registrado continua `motor.decidir`, e a tag só consulta apresentação (`app/core/juizes.py:274`, `app/chamados/models.py:123`, `app/chamados/templatetags/chamados_tags.py:74`). `DESTINOS` congela a oferta de UI; não decide transições (`app/chamados/tests/test_bo_rotulo_encerrado.py:62`, `app/chamados/tests/test_bo_rotulo_encerrado.py:127`). Os mapas de movimento/pílula respondem a perguntas diferentes e seus consumidores permanecem separados, sem sobrecarga nova demonstrada (`app/chamados/services/verbos.py:125`, `app/chamados/services/verbos.py:140`, `app/chamados/services/verbos.py:162`).

O censo não pode ficar vazio e manter verde: há mínimo de cinco arquivos e comparação exata com `DESTINOS` (`app/chamados/tests/test_bo_rotulo_encerrado.py:95`, `app/chamados/tests/test_bo_rotulo_encerrado.py:131`). Entretanto, há candidato forte a **BLOQUEIA — selo sem mordida suficiente**: manter todas as tags e acrescentar, antes de `</select>` no detalhe local, duas opções com fechamento implícito:

```html
<option value="resolvido">Encerrado
<option value="fechado">Encerrado
```

O reconhecedor exige `</option>` e `DESTINOS` conta somente tags `opcao_mover`, logo esses acréscimos escapam à lógica estática (`app/templates/chamados/detalhe_local.html:398`, `app/chamados/tests/test_bo_rotulo_encerrado.py:23`, `app/chamados/tests/test_bo_rotulo_encerrado.py:104`, `app/chamados/tests/test_bo_rotulo_encerrado.py:111`, `app/chamados/tests/test_bo_rotulo_encerrado.py:127`). Execução da mutação e renderização em navegador `[nao verificado]`.

## 11/11 12:19 · sessao e9c852c5cec4 · - · - · origem=Ronald (tmux)

Censo direto da cópia: cinco seletores oferecem `status_local`, todos com `{% opcao_mover %}`, e todos entram em `DESTINOS` e nas verificações de literal/conjunto/ordem: `app/chamados/tests/test_bo_rotulo_encerrado.py:64`, `:99`, `:127`.

| Controle | Destinos na ordem | Receptor e cobertura |
|---|---|---|
| `app/templates/chamados/detalhe.html:130` | `em_analise,resolvido,fechado,aberto` | POST para `followup` em `:122`; `app/chamados/urls.py:12` → `app/chamados/views.py:292` e `:304`, que só envia conteúdo ao GLPI. **Fila:** status exibido mas ignorado nesse caminho. |
| `app/templates/chamados/detalhe_local.html:393` | mesmos quatro | `app/chamados/views.py:547` → `app/chamados/services/acoes_chamado.py:287`, `:313`, `:315`; testa código em `STATUS_CHOICES` e chama `transicionar`. |
| `app/templates/chamados/fio_colaborador.html:265` | três primeiros | `app/chamados/views.py:669` → `app/chamados/services/fio.py:502`, `:709`, `:719`, `:721`; exige conteúdo e permissão contextual, depois transiciona. |
| `app/templates/chamados/partials/modal_fio.html:665` | três primeiros | `app/chamados/views.py:953` → mesmo processador do fio. |
| `app/templates/chamados/partials/painel_chamado_turno.html:74` | três primeiros | Form envia ao detalhe local em `:68`; mesmo receptor do detalhe local. |

A cobertura percorre **todos os arquivos `.html` sob `app/templates/`**, mas só inspeciona fonte: `_html` em `app/chamados/tests/test_bo_rotulo_encerrado.py:33`, censo em `:55` e duplicidade textual em `:120`. A proibição de opções literais fora de seletores `novo_status` só percorre `templates/chamados` (`:110`). Não percorre `.js`, Python ou forms Django (`:36`); portanto não protege futuros status construídos nesses lugares (`:33`, `:52`, `:117`).

Busca produtiva no repositório não encontrou outro seletor de destinos, JS que ofereça esses destinos nem choices montadas em views/forms; ausência absoluta de construção indireta: `[nao verificado]`. Os receptores encontrados leem somente o código (`acoes_chamado.py:287`; `fio.py:502`).

Controles de ações com efeito sobre o estado, **fora do selo de opções**, incluindo omissões do censo da v1:

| Ação | Controle e caminho |
|---|---|
| Aprovar/rejeitar benefício | `app/templates/colaboradores/painel_solicitacao_beneficio.html:23`, `:24`; também JS em `app/templates/colaboradores/fila_solicitacoes_beneficio.html:138`, `:139` → `app/colaboradores/views.py:2068`, `resolvido`/`fechado`. |
| Aceitar/recusar escala | `app/templates/chamados/detalhe_local.html:163`, `:169` → `app/chamados/services/acoes_chamado.py:149`, `resolvido`. |
| Aceitar/recusar agenda | mesmo template `:200`, `:209` → `acoes_chamado.py:108`, `resolvido`. |
| Aprovar exceção | detalhe local `:256`; modal `:598` → `acoes_chamado.py:70`, `resolvido`. |
| Autorizar geofence, autorizar com advertência, recusar | detalhe local `:285`, `:286`, `:287`; modal `:506`, `:517`, `:528` → `acoes_chamado.py:222`, `:277`, `resolvido`; preparação de `registrado → em_analise` em `:232`. |
| Abrir registro de ausência | detalhe local `:233` → `app/chamados/views.py:1513`, `analisar()`. |
| Reabrir administrativamente disputa | modal `:390` → `app/chamados/services/acoes_disputa.py:269`, `:270`, `reabrir(humano=True)`. |
| Resgatar chamado encerrado pela tranca | `app/templates/chamados/partials/_lista_chamados.html:85` → `app/chamados/views_arquivar.py:64`, `:70`, `reabrir(humano=True)`. |
| **Aceitar folga sugerida** | modal `:550` → `app/chamados/views.py:431` → `app/chamados/services/limbo_folgas.py:98`, `:101`, reconcilia os chamados do mesmo template. |
| **Validar resposta / validar assim mesmo** | modal `:265`, `:281`; também detalhe local roteia em `app/chamados/views.py:483` → `app/chamados/services/acoes_disputa.py:18` → `app/chamados/services/validacao.py:295`, `:296` → `app/ponto/services/veredito_celula.py:202`, reconcilia chamados do dia. |
| **Validar inline** | `_lista_chamados.html:82` → `app/chamados/views_cobrar.py:208`, mesma `acao_validar_pergunta`. |
| **Validar em lote** | `app/templates/chamados/painel_gestao.html:14`, `app/templates/chamados/partials/_lote_preview.html:21`; `app/chamados/views_cobrar.py:247`, `:261`, delega ao validador de lote. |
| **Regularizar disputa e fechar** | modal `:355` → `app/chamados/services/acoes_disputa.py:139`, materializa perguntas validadas; fio tem processamento em `app/chamados/services/fio.py:695`. Efeito exato de todo caso sobre `status_local`: `[nao verificado]`. |

Filtros de estado não são controles de transição: histórico em `app/templates/relatorios/historico_chamados.html:54` oferece `aberto/em_analise/resolvido/fechado` (`:56`–`:59`), e o receptor aplica filtro em `app/relatorios/views.py:920`. O teste global também enxerga essas opções, pois reconhece qualquer `value` presente em `ESTADOS` (`app/chamados/tests/test_bo_rotulo_encerrado.py:46`), independentemente de ser filtro.

## 11/11 12:19 · sessao c7e0251bc2ef · FIO-E0-V4-E4B · censo · origem=Code

**O achado 1 procede quanto à interrupção dos carimbos e da varredura de fecho. Há duas correções: o status do chamado já foi salvo quando S143 recebe `{0,0}`; e uma disputa que ainda contém pergunta viva já não poderia fechar por C7.** O retorno antecipado também impede examinar outras disputas abertas do colaborador, inclusive órfãs fecháveis. (`app/chamados/models.py:296`; `app/chamados/signals.py:115`; `app/chamados/models.py:1853`; `app/chamados/services/disputa_emissao.py:1946`, `:2021`, `:2086`)

**1. Censo dos chamadores e alcance da reabertura**

Há cinco chamadas de produção do resolver; `models.py` apenas reexporta a função. (`app/chamados/signals.py:115`; `app/chamados/services/becos.py:54`; `app/chamados/reconciliador.py:536`, `:1648`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`; `app/chamados/models.py:4093`)

| Chamador | Gatilho e estado ao entrar | Pode alcançar reabertura/ressuscitação/S2? |
|---|---|---|
| `app/chamados/signals.py:115` | `post_save` de chamado em `ENCERRADOS`, quando o save escreveu `status_local`. Inclui encerramento novo e save nomeando o mesmo status encerrado. (`:75`, `:85`, `:90`, `:96`, `:104`) | Sim, passa pelo resolver após o save do chamado. (`app/chamados/models.py:296`; `app/chamados/services/disputa_emissao.py:1945`) |
| `app/chamados/services/becos.py:54` | Evento de pergunta ou command: reúne pai/destinos **já terminais**, incluindo cancelado/superado. (`:11`, `:21`, `:42`, `:46`) | Condicional. O evento exige `FECHAVEL`; uma viva na própria disputa impede essa entrada. O resolver pode encontrar perguntas de outras disputas ancoradas no mesmo chamado. (`:33`, `:74`; `app/chamados/models.py:344`; `app/chamados/services/disputa_emissao.py:1938`) |
| `app/chamados/reconciliador.py:536` | `_retratar`: só chega depois de `ch.reconciliar()` retornar verdadeiro. Portanto entra após o encerramento feito nessa operação; o save já acionou também S143. (`:522`, `:523`, `:536`; `app/chamados/models.py:654`, `:296`) | Sim, sujeito às guardas comuns abaixo. Não é, por si, varredura de passivo já encerrado: exige uma reconciliação aceita antes. (`app/chamados/reconciliador.py:523`; `app/chamados/services/disputa_emissao.py:1945`) |
| `app/chamados/reconciliador.py:1648` | `reatar_pergunta_orfa`: candidato histórico único vem de consulta a `ENCERRADOS`; exige reaponte aceito. É entrada explícita com chamado histórico resolvido/fechado. (`:1586`, `:1587`, `:1623`, `:1642`, `:1648`) | Sim, sujeito às guardas comuns. (`app/chamados/services/disputa_emissao.py:1945`) |
| `app/ponto/management/commands/reconciliar_fantasmas.py:281` | Somente apply; consulta inicial contém **apenas VIVOS**. Primeiro tenta `retratar`; chama explicitamente o resolver somente se `rec=False`. O comentário menciona chamado já resolvido, mas a consulta não seleciona esse passivo. (`:153`, `:157`, `:159`, `:269`, `:273`, `:280`, `:281`) | Pode executar a porta, mas não é uma varredura inicial de chamados fechados. O cron declarado usa `--alarme`, que desliga apply. (`:153`; `app/config/crons.py:392`) |

A porta comum exige pergunta sem `respondida_em`, viva pelo C6 e credor fora de `VIVOS`. Mede a carga inteira de relavra/ressuscitação, recusa competência exportada **ou indeterminada**, recusa limbo e consulta as guardas existentes de reabertura. Só então chama `credor.reabrir()`. (`app/chamados/services/disputa_emissao.py:1792`, `:1821`, `:1827`, `:1835`, `:1837`, `:1846`, `:1849`)

Quando aceita, a cadeia é efetiva: `reabrir` grava o estado e chama `ressuscitar_perguntas`; a mudança de fronteira de `ENCERRADOS` permite relavra S2 das perguntas por destino; a ressuscitação limpa carimbos e pode reabrir disputas fechadas. O resolver usa `executar=True`, sem DRY próprio. (`app/chamados/models.py:1440`, `:1452`, `:273`, `:303`, `:312`, `:1562`, `:1575`; `app/chamados/services/disputa_emissao.py:1805`, `:1945`)

Os gatilhos indiretos encontrados são:

| Entrada | Caminho |
|---|---|
| Batida salva | Signal → reconciliação do colaborador → retratação e reata de órfãs. (`app/ponto/signals.py:206`, `:214`; `app/chamados/reconciliador.py:1432`, `:1464`, `:1526`) |
| Filho deixa `VIVOS` | Signal agenda a mesma reconciliação após commit. (`app/chamados/signals.py:349`, `:366`, `:369`) |
| Pergunta salva | Signal agenda `fechar_beco_da_disputa` após commit. (`app/chamados/signals.py:227`, `:237`, `:238`) |
| Emissão de pergunta órfã | Emissor chama `reatar_pergunta_orfa`. (`app/chamados/services/disputa_emissao.py:459`, `:465`) |
| Outbox de célula | Consumidor chama retratação por célula e por premissa; cron declarado a cada minuto. (`app/chamados/reconciliador.py:1368`, `:1372`; `app/config/crons.py:394`) |
| Ação humana no detalhe/fio | Serviços transitam o chamado → núcleo salva → S143. (`app/chamados/views.py:423`; `app/chamados/services/acoes_chamado.py:70`, `:108`, `:149`, `:222`, `:277`, `:315`; `app/chamados/services/fio.py:719`; `app/chamados/models.py:450`, `:296`) |
| Justificativa e geofence | Aprovação de justificativa e aplicação de geofence delegam a `retratar`. (`app/ponto/services/justificativa.py:46`; `app/ponto/services/geofence.py:441`) |
| Outros commands | Alertas de turno, grade, vínculo e lei-cala delegam a `retratar`; os três últimos condicionam a escrita a apply. (`app/ponto/management/commands/processar_alertas_turno.py:216`; `app/ponto/management/commands/reconciliar_grade.py:305`, `:309`; `app/ponto/management/commands/reconciliar_vinculo.py:58`, `:68`; `app/chamados/management/commands/retratar_chamados_lei_cala.py:54`, `:59`) |

Os crons declarados de **fantasmas**, **reconciliar chamados** e **becos** usam `--alarme`; essa declaração não demonstra execução mutante do resolver. Alertas de turno têm agendamento `*/5`. Execução real desses crons: **[nao verificado]**. (`app/config/crons.py:362`, `:392`, `:395`, `:663`; `app/ponto/management/commands/reconciliar_fantasmas.py:153`; `app/chamados/management/commands/fechar_becos_disputa.py:30`, `:54`)

Há ainda um chamador direto da **porta de reabertura**, sem passar pelo resolver: o backfill E4B. Ele planeja com `executar=False`, executa em `_executar_dia` e possui DRY com rollback e apply cercado. Portanto esse command não deve ser confundido com os eventos comuns. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:90`, `:135`, `:175`, `:300`, `:308`, `:353`)

**ANTES versus DEPOIS:** a comparação literal com `fb9269ea9`, inclusive por chamador, fica **[nao verificado]**: o corpo dessa árvore base não está disponível no material consultado. O comentário atual declara que a base já carregava S128; o recibo descreve diferenças de escopo de `tranca_da_consulta`, mas não substitui o código antigo. (`app/chamados/services/disputa_emissao.py:1926`; `app/docs/FIO-E0-V4-E4B-c4-v14.txt:17`)

Na cópia atual, a ordem comprovada é **tentar devolver alcance → eventualmente retornar `{0,0}` → somente depois viriam S128, HX-RESPOSTA-VISIVEL, HX-DISPUTA-ORFA e C7**. Atribuir precisamente a introdução de cada trecho ao diff informado permanece **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1945`, `:1947`, `:1981`, `:1998`, `:2021`, `:2086`)

**2. Todos os consumidores do retorno e comportamento com `{0,0}`**

| Consumidor | O que faz |
|---|---|
| S143 | Descarta o retorno. O status já foi salvo; a recusa não provoca rollback. (`app/chamados/signals.py:115`; `app/chamados/models.py:296`) |
| `fechar_beco` | Descarta o retorno, relê a disputa e ainda tenta fechá-la se C7 permitir. Portanto a recusa do resolver não impede necessariamente esse fecho posterior. (`app/chamados/services/becos.py:54`, `:55`, `:56`, `:57`) |
| `_retratar` | Descarta o retorno, anexa ocorrência de retratação e retorna `True`. (`app/chamados/reconciliador.py:536`, `:538`, `:541`) |
| `reatar_pergunta_orfa` | Descarta o retorno e retorna `True` pelo reaponte aceito; isso não comprova carimbo nem fecho de disputa. (`app/chamados/reconciliador.py:1642`, `:1648`, `:1649`) |
| `reconciliar_fantasmas` | **Único leitor das contagens:** com `rec=False` e `{0,0}`, calcula `mudou=False`, pula ocorrência/evento e apenas imprime. Não grava status depois desse retorno. (`app/ponto/management/commands/reconciliar_fantasmas.py:281`, `:282`, `:283`, `:306`) |

O wrapper público apenas devolve o dict do corpo, sem interpretar recusa. (`app/chamados/services/disputa_emissao.py:1873`, `:1879`)

Assim, **sim, o código permite conservar chamado encerrado com disputas abertas após essa recusa**. A ordem principal, porém, é “salvou status → S143 → retorno ignorado”, e não “leu zero → depois gravou fechado”. O núcleo ainda pode executar sua relavra S2 após o receiver. (`app/chamados/models.py:296`, `:303`; `app/chamados/signals.py:115`; `app/chamados/services/disputa_emissao.py:1947`)

`{0,0}` também não significa ausência de escrita: a porta anexa ocorrência de recusa antes de devolvê-la. As duas contagens representam carimbos S128 e fechos; os carimbos do ramo HX de órfãs nem incrementam `perguntas_resolvidas`. (`app/chamados/services/disputa_emissao.py:1855`, `:1859`, `:1984`, `:2055`, `:2088`)

O fechamento comum pode chegar à recusa: F1 exclui exportadas/indeterminadas da lista que bloquearia o fechamento; a porta de reabertura recusa precisamente essa carga. O teste existente exige que a recusa interrompa outros carimbos e fechos. Ocorrência concreta em produção: **[nao verificado]**. (`app/chamados/models.py:364`, `:365`; `app/chamados/services/disputa_emissao.py:1835`, `:1946`; `app/chamados/tests/test_e4b_classe_e_memoria.py:960`, `:970`, `:972`)

A correção do contraexemplo é esta: **viva exportada + mortas na mesma disputa demonstra mortas sem carimbo; não demonstra que essa mesma disputa deveria fechar**, porque C7 já barra sua pergunta viva. O alcance adicional perdido é a varredura das demais disputas abertas do colaborador. (`app/chamados/models.py:1853`; `app/chamados/services/disputa_emissao.py:1947`, `:1981`, `:2022`, `:2086`)

**3. Contadores/vigias e seus limites**

| Detector | O que perceberia |
|---|---|
| Auditor **I2** | Pergunta sem materialização apontando a chamado terminal; exclui do alarme a respondida em disputa aberta. Detecta o resíduo com destino terminal, mas não a órfã sem destino. (`app/chamados/management/commands/auditar_invariantes_chamados.py:587`, `:591`, `:594`) |
| Vigia de becos | Varre abertas e lista `FECHAVEL`/`BARRADA`; `--alarme` falha somente para `FECHAVEL`. Órfã sem destino e sem resposta fica fora do recorte; disputa com viva pode ser apenas listada como barrada. (`app/chamados/management/commands/fechar_becos_disputa.py:32`, `:39`, `:44`, `:67`; `app/chamados/services/becos.py:29`, `:33`) |
| Relatório de pendências | Aberta sem nenhuma pergunta viva aparece em `PRONTA_FECHAR`; é relatório, não guarda de fechamento. (`app/chamados/relatorios_disputa.py:18`, `:20`, `:41`) |
| `auditar_disputas` | Conta toda disputa aberta há mais de sete dias, sem identificar a causa da recusa. (`app/chamados/management/commands/auditar_disputas.py:132`, `:135`) |
| Alerta de pendências | Seleciona abertas e alerta colaborador/supervisão; cron declarado às 08h. Não identifica especificamente “órfã causada pela recusa”. (`app/ponto/management/commands/alertar_disputas_pendentes.py:33`, `:48`, `:73`, `:103`; `app/config/crons.py:745`) |
| `vigia_fio.censo_alcance` | Pode classificar viva sob chamado terminal, dentro dos dias retidos/competências a lavrar; não cobre universalmente órfãs mortas ou exportadas. (`app/chamados/services/vigia_fio.py:577`, `:587`, `:687`) |

Portanto, **não procede dizer que não existe detector algum**; os detectores têm recortes distintos e não comprovam cobertura universal dessa recusa. Execução e resultados na frota: **[nao verificado]**. (`app/chamados/management/commands/auditar_invariantes_chamados.py:587`; `app/chamados/management/commands/fechar_becos_disputa.py:67`)

**Quem escreve e quem lê os campos omitidos**

O quadro abaixo identifica os escritores do caminho interrompido e os leitores funcionais relevantes; não é inventário de toda referência textual a esses campos. (`app/chamados/services/disputa_emissao.py:1981`, `:2055`, `:2087`)

| Campos | Quem escreveria | Quem lê |
|---|---|---|
| `materializada_em`, `materializada_por`, `via_resolucao` | S128 e HX chamam `carimbar`; a atribuição canônica fica em `_escrever_carimbo`. (`app/chamados/services/disputa_emissao.py:1981`, `:2055`; `app/chamados/models.py:3097`) | C6 lê carimbo/via; estado da pergunta lê materialização; materializador seleciona materialização nula. O autor também é lido em registros de auditoria. (`app/chamados/juizes.py:1342`, `:1368`, `:1405`; `app/chamados/models.py:3046`; `app/chamados/services/materializacao.py:1372`; `app/chamados/reconciliador.py:1145`) |
| `respondida_em`, `validada_em`, `validada_por` | `carimbar` pode preenchê-los com texto não vazio; S128 usa `resposta='validar'`, HX usa `'responder'`. (`app/chamados/models.py:3130`, `:3134`; `app/chamados/services/disputa_emissao.py:1982`, `:2055`) | C6, respostas sem veredito/C7, estado da pergunta e fila do materializador. (`app/chamados/juizes.py:1331`, `:1347`; `app/chamados/models.py:1843`, `:1853`, `:3041`; `app/chamados/services/materializacao.py:1369`) |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | S128 solicita relavra no mesmo save do carimbo. (`app/chamados/services/disputa_emissao.py:1982`; `app/chamados/models.py:3149`, `:3151`) | São a lavra persistida associada ao carimbo; a recusa pula essa chamada S128, mas não elimina a relavra independente do núcleo do chamado. (`app/chamados/models.py:3149`, `:303`, `:312`) |
| `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | O laço final chama `d.fechar`; a porta grava os quatro juntos. (`app/chamados/services/disputa_emissao.py:2087`; `app/chamados/models.py:1932`, `:1936`) | `aberta`, consulta do app, lookup do fio e relatórios leem o estado aberto; signals usam fecho/autor/resolução para cascatas; a porta lê o conjunto para idempotência; o vigia conta motivo `vazia`. (`app/chamados/models.py:1824`, `:1880`; `app/chamados/services/pergunta_no_app.py:71`; `app/chamados/services/fio.py:131`; `app/chamados/relatorios_disputa.py:41`; `app/chamados/signals.py:45`, `:136`, `:163`; `app/chamados/services/vigia_fio.py:734`) |

Por fim, o item dos tetos comentados **não procede nesta cópia**: 213 e 269 são entradas ativas e são cobradas por `assertLessEqual`. Resultado de execução desses testes: **[nao verificado]**. (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`, `:292`, `:294`)

## 11/11 12:17 · sessao c17d1b00836f · - · - · origem=Ronald (tmux)

- **Único leitor semântico do retorno:** `app/ponto/management/commands/reconciliar_fantasmas.py:281` recebe o dict; `:282` calcula `mudou` pelas duas contagens. Com `rec=False` e `{0,0}`, pula nota e evento (`:283-305`), apenas imprime (`:306-309`). Não grava status depois desse retorno. Os demais chamadores descartam o retorno: `app/chamados/signals.py:115`, `app/chamados/services/becos.py:54`, `app/chamados/reconciliador.py:536`, `:1648`.

- **Fechamento comum permanece gravado:** `_gravar_estado` salva o chamado em `app/chamados/models.py:296`; o S143 recebe o estado encerrado (`app/chamados/signals.py:96`) e ignora o retorno (`:115`). A recusa normal `{0,0}` não lança exceção nem determina rollback; após o receiver, `_gravar_estado` ainda pode relavrar S2 (`app/chamados/models.py:303-312`). Portanto a persistência do chamado terminal com disputas abertas é sustentada pelo código; não decorre de um leitor explícito dizendo “nada a fazer”.

- **`{0,0}` não significa zero escrita:** o helper grava ocorrência de recusa antes de retornar (`app/chamados/services/disputa_emissao.py:1855-1859`). O laço pode reabrir uma âncora anterior antes de encontrar outra que recusa (`:1941-1947`); o `return` não desfaz isso. As contagens só incrementam carimbo S128 (`:1984`) e fechamento de disputa (`:2088`); nem carimbos HX de órfãs (`:2055`) entram em `perguntas_resolvidas`.

**Vigias/contadores existentes e limites:**

| Leitor | O que perceberia |
|---|---|
| `app/chamados/management/commands/fechar_becos_disputa.py:32-49` | Varre disputas abertas; lista FECHAVEL/BARRADA. `--alarme` dá exit 2 somente para FECHAVEL (`:67-70`). A classificação exige pai terminal e nenhuma pergunta sem resposta com destino não terminal (`app/chamados/services/becos.py:25-33`). Uma órfã sem destino e sem resposta passa fora desse recorte; viva exportada na mesma disputa pode produzir BARRADA, listada sem alarme. Cron declarado às 07:10 em `app/config/crons.py:663`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1601-1605` | C2 conta pergunta morta pela célula com `materializada_em`, `validada_em`, `respondida_em` nulos e veredito não acusador. Não é detector universal de órfãs e o filtro não exige disputa aberta. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:805-807` | I7 só cobre disputa aberta com pai inexistente ou cancelado; pai resolvido/fechado não entra. |
| `app/chamados/relatorios_disputa.py:18-20` e `:40-47` | Disputa aberta sem nenhuma viva aparece em `PRONTA_FECHAR`; é relatório, não cura nem guarda. Comando `relatorio_pendencias_disputa` usa esse coletor (`app/chamados/management/commands/relatorio_pendencias_disputa.py:15`); declarado sob demanda em `app/config/crons.py:1448`. |
| `app/chamados/management/commands/auditar_disputas.py:132-143` e `:203` | Toda disputa ainda aberta há mais de sete dias entra em `disputas_paradas_7d`; não identifica especificamente a recusa. Sob demanda: `app/config/crons.py:1450`. |
| `app/ponto/management/commands/alertar_disputas_pendentes.py:33-35` | Toda aberta entra no alerta genérico: push colaborador (`:48-65`), agregado supervisão (`:67-70`, `:103-115`), `disputa_velha` se >7 dias (`:73-85`). Cron declarado 08:00 diário: `app/config/crons.py:745`. |
| `app/chamados/services/vigia_fio.py:577-587` e `:687-699` | `censo_alcance` mede dias retidos em competências a lavrar; pode classificar `pergunta_viva_sob_chamado_terminal`. Não garante perceber órfã morta ou pergunta exportada fora desse universo. |

Execução real dos crons: **[nao verificado]**.

**Escritores dos campos omitidos pelo retorno antecipado:**

- S128 chama `carimbar(resposta='validar', lavrar_veredito=True)`: `app/chamados/services/disputa_emissao.py:1981-1982`.
- HX órfã chama `carimbar(resposta='responder')`: `app/chamados/services/disputa_emissao.py:2055`.
- Porta canônica escreve `materializada_em`, `materializada_por_id`, `via_resolucao`: `app/chamados/models.py:3097-3101`; pode preencher `respondida_em`, `validada_em`, `validada_por` apenas com texto não vazio (`:3130-3137`). S128 também lavra `veredito_lavrado`, `julgado_em`, `janela_expira_em` (`:3149-3152`).
- Fecho final chama `d.fechar`: `app/chamados/services/disputa_emissao.py:2086-2088`; a porta escreve `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` e evento: `app/chamados/models.py:1932-1940`.

**Leitores canônicos e efeitos desses campos:**

- C6: `validada_em` (`app/chamados/juizes.py:1331`), via/carimbo (`:1342`, `:1368`), resposta/carimbo (`:1347`), materialização (`:1374`, `:1405`).
- Estado da pergunta: `app/chamados/models.py:3041-3048`; pré-filtro `q_vivas`: `:3348-3359`.
- Respostas sem veredito e C7: `app/chamados/models.py:1842-1853`.
- Fila do materializador: `app/chamados/services/materializacao.py:1369-1373`; S144 seleciona materialização nula (`:1418-1419`).
- Disputa aberta: property `app/chamados/models.py:1823-1824`; apresentação app `app/chamados/services/pergunta_no_app.py:71`; lookup fio `app/chamados/services/fio.py:131-135`; relatório `app/chamados/relatorios_disputa.py:41`.
- `fechada_em` também dispara S88 para reconciliar pai (`app/chamados/signals.py:28-42`), retomada de regularizações (`:129-144`) e S144 (`:153-162`). Ao não fechar a disputa, esses eventos não são emitidos por esse caminho.

Esse último bloco é mapa dos leitores canônicos/efeitos relevantes, **não um censo literal de toda referência desses campos na árvore**.

**Correção importante sobre beco:** o evento só chama `fechar_beco` quando `classificar==FECHAVEL` (`app/chamados/services/becos.py:74-78`), e FECHAVEL depende de C7 (`:33`), que exige nenhuma viva na própria disputa (`app/chamados/models.py:1853`). Logo uma viva na própria disputa não demonstra esse evento chegando ao helper: seria BARRADA. O resolver pode, contudo, carregar âncoras do chamado pertencentes a outras disputas (`app/chamados/services/disputa_emissao.py:1938`).

`_C14` não é vigia de órfãs: pergunta pela competência do chamado, em `app/core/juizes.py:122`, registrada em `:286`; o I14 do auditor trata junção de célula (`app/chamados/management/commands/auditar_invariantes_chamados.py:1022`).

## 11/11 12:16 · sessao 5c659c5fb6aa · - · - · origem=Ronald (tmux)

Censo direto de produção: **cinco chamadas do resolver**, mais a chamada direta da porta no backfill; a exportação de `models.py` não é novo chamador. (`app/chamados/signals.py:115`; `app/chamados/services/becos.py:54`; `app/chamados/reconciliador.py:536`; `app/chamados/reconciliador.py:1648`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:135`; `app/chamados/models.py:4093`)

| Chamador | Gatilho e estado da entrada | Tratamento do retorno |
|---|---|---|
| `app/chamados/signals.py:115` | `post_save` de Chamado; exige `ENCERRADOS`, isto é, resolvido/fechado, e save que escreveu status. Tanto pode ser transição recém-encerrada quanto save nomeando `status_local` sem mudar valor. (`signals.py:90-115`; `catalogo/motor.py:30`; `signals.py:75-87`) | Ignora retorno; `{0,0}` não levanta nem desfaz o estado salvo antes do receiver. (`signals.py:114-117`; `models.py:296`) |
| `app/chamados/services/becos.py:54` | Reúne pai/destinos em `TERMINAIS`: inclui resolvido/fechado **e cancelado/superado**. (`becos.py:11-22`; `becos.py:42-54`; `catalogo/motor.py:30-31`) | Ignora retorno, relê disputa e fecha se C7 permite; devolve se terminou fechada. (`becos.py:54-59`) |
| `app/chamados/reconciliador.py:536` | `_retratar`: só chega depois de `ch.reconciliar()` retornar verdadeiro; portanto encerrou pela porta antes de chamar explicitamente. O save dessa porta já dispara também o receiver S143. (`reconciliador.py:522-536`; `models.py:650-655`; `models.py:296`; `signals.py:115`) | Ignora retorno, anexa ocorrência e retorna `True`. Não há recusa de `{0,0}`. (`reconciliador.py:536-541`) |
| `app/chamados/reconciliador.py:1648` | `reatar_pergunta_orfa`: candidato histórico único vem de consulta `status_local__in=ENCERRADOS`; reaponte deve ter aceitado antes da chamada. (`reconciliador.py:1585-1589`; `reconciliador.py:1623-1648`) | Ignora retorno e retorna `True`, mesmo que resolver retorne `{0,0}`. Isso contabiliza reata bem-sucedida, sem provar carimbo/fecho. (`reconciliador.py:1642-1649`; `reconciliador.py:1524-1527`) |
| `app/ponto/management/commands/reconciliar_fantasmas.py:281` | Só em `--apply`, nunca `--alarme`; consulta inicial **só VIVOS**. Primeiro tenta `retratar`; só chama explicitamente resolver se `rec=False`. Não constitui varredura inicial de chamados já encerrados. (`reconciliar_fantasmas.py:153`; `reconciliar_fantasmas.py:157-160`; `reconciliar_fantasmas.py:269-281`) | Único leitor de produção das duas contagens: `{0,0}` com `rec=False` dá `mudou=False`, pula ocorrência/log `reconciliar_fantasma`, mas imprime resultado. Se `rec=True`, não lê resultado interno do resolver e `mudou=True`. (`reconciliar_fantasmas.py:281-309`) |

A nova porta pode reabrir somente pergunta **sem `respondida_em`, viva pelo C6**, ancorada em chamado fora de VIVOS; exige carga de reabertura sem competência exportada/indeterminada e sem limbo, além das recusas da porta existente. Em sucesso chama `credor.reabrir()`. (`app/chamados/services/disputa_emissao.py:1821-1849`) A reabertura grava estado, pode relavrar S2 e chama `ressuscitar_perguntas()`: são efeitos efetivos, sem parâmetro dry no resolver/eventos. (`app/chamados/models.py:1442-1452`; `app/chamados/models.py:303-312`; `app/chamados/services/disputa_emissao.py:1945`)

**Ressalva de becos:** o evento só chama `fechar_beco` quando `classificar(d)==FECHAVEL`, que exige `d.pode_fechar()`; C7 exige ausência de pergunta viva e resposta sem veredito **nessa disputa**. Portanto não procede afirmar que uma viva sem resposta da própria disputa leva normalmente o evento até a porta nova. (`app/chamados/services/becos.py:25-33`; `app/chamados/services/becos.py:72-78`; `app/chamados/models.py:1848-1853`) Isso não prova impossibilidade geral: o resolver examina `_ancoradas` do chamado, incluindo perguntas de outras disputas e sem filtro de disputa aberta; seu fechamento ainda varre todas abertas do colaborador. (`app/chamados/models.py:344-347`; `app/chamados/services/disputa_emissao.py:1938-1945`; `app/chamados/services/disputa_emissao.py:2021-2023`)

Entradas indiretas concretas:

- **Batida/evento:** `post_save Batida` chama `reconciliar_chamados_colab`, que reata órfãs e retrata premissas mortas; passa respectivamente por `:1648` e `:536`. (`app/ponto/signals.py:206-215`; `app/chamados/reconciliador.py:1432-1441`; `app/chamados/reconciliador.py:1526`; `app/chamados/reconciliador.py:1464`)
- **Filho que sai de VIVOS:** `post_save Chamado` agenda, após commit, a mesma reconciliação do colaborador. (`app/chamados/signals.py:349-369`)
- **Pergunta salva:** `post_save PerguntaDisputa` agenda `fechar_beco_da_disputa` após commit. (`app/chamados/signals.py:227-238`)
- **Emissão de pergunta órfã nova:** emissor chama `reatar_pergunta_orfa`; pode chegar ao histórico encerrado de `:1648`. (`app/chamados/services/disputa_emissao.py:459-465`)
- **Outbox/cron:** consumidor chama retratação por célula e por premissa; cron declarado a cada minuto. (`app/chamados/reconciliador.py:1368-1372`; `app/config/crons.py:394`)
- **Humano, detalhe/fio:** ações da view chamam serviços que transitam para resolvido; resposta livre e fio permitem transição solicitada pelo gestor. Todas passam pelo núcleo que salva e pelo S143. (`app/chamados/views.py:423-427`; `app/chamados/services/acoes_chamado.py:70`; `app/chamados/services/acoes_chamado.py:108`; `app/chamados/services/acoes_chamado.py:149`; `app/chamados/services/acoes_chamado.py:222`; `app/chamados/services/acoes_chamado.py:277`; `app/chamados/services/acoes_chamado.py:315`; `app/chamados/services/fio.py:719-721`; `app/chamados/models.py:450`; `app/chamados/models.py:296`; `app/chamados/signals.py:115`)
- **Aprovação de justificativa:** encerra origem por `retratar`, após exigir chamado vivo. (`app/ponto/services/justificativa.py:33-46`)
- **Geofence:** reavaliação com `aplicar=True` chama `retratar`. (`app/ponto/services/geofence.py:435-443`)
- **Commands que delegam a `retratar`:** alertas de turno `:216`, grade `:309` condicionado a apply `:305`, vínculo `:68` condicionado a apply `:58`, lei cala `:59` condicionado a apply `:54`, fantasmas `:280`. (`app/ponto/management/commands/processar_alertas_turno.py:216`; `app/ponto/management/commands/reconciliar_grade.py:305`; `app/ponto/management/commands/reconciliar_grade.py:309`; `app/ponto/management/commands/reconciliar_vinculo.py:58`; `app/ponto/management/commands/reconciliar_vinculo.py:68`; `app/chamados/management/commands/retratar_chamados_lei_cala.py:54`; `app/chamados/management/commands/retratar_chamados_lei_cala.py:59`; `app/ponto/management/commands/reconciliar_fantasmas.py:280`)
- **Agendamentos declarados:** alertas turno `*/5`; fantasmas `*/15 --alarme`; reconciliar chamados `*/15 --alarme`; grade às 06:20 sem apply; vínculo às 06:47 `--alarme`; becos às 07:10 `--alarme`. Os crons fantasmas e becos nesses modos não executam o resolver. (`app/config/crons.py:362`; `app/config/crons.py:392`; `app/config/crons.py:395`; `app/config/crons.py:462`; `app/config/crons.py:489`; `app/config/crons.py:663`; `app/ponto/management/commands/reconciliar_fantasmas.py:153`; `app/ponto/management/commands/reconciliar_fantasmas.py:269-270`; `app/chamados/management/commands/fechar_becos_disputa.py:30`; `app/chamados/management/commands/fechar_becos_disputa.py:54-57`)
- **Backfill direto:** planeja com `executar=False` em `ato_do_dia`; executa em `_executar_dia`; lista exportadas em leitura. Possui DRY com rollback e apply condicionado a competência corrente. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:90`; `:135`; `:175`; `:221-230`; `:353-359`)

O caminho comum **pode persistir estado encerrado com `{0,0}`**, pois o save do chamado precede o receiver e a recusa retorna normalmente; o núcleo continua inclusive a possível relavra S2 depois do save. A formulação “tratam `{0,0}` como nada a fazer e **depois gravam** status fechado” não descreve a ordem principal: **status já foi gravado**, o resolver é chamado no `post_save`, e o retorno é ignorado. (`app/chamados/models.py:296-312`; `app/chamados/signals.py:114-117`; `app/chamados/services/disputa_emissao.py:1946-1947`)

## 11/11 12:16 · sessao 9864d761e438 · - · - · origem=Ronald (tmux)

- **Comparação literal com `fb9269ea9`: [nao verificado].** A cópia não tem `.git` e o `git cat-file` não executa como repositório. Existe somente um diff intermediário de c4-v2, cujo cabeçalho não identifica `fb9269ea9` (`app/docs/FIO-E0-V4-E4B-c4-v2.diff:1-5`). O recibo afirma que a base tinha o laço S128 e C7 fora de `tranca_da_consulta`, mas isso é documentação histórica, sem o corpo antigo disponível (`app/docs/FIO-E0-V4-E4B-c4-v14.txt:17-21`; comentário atual: `app/chamados/services/disputa_emissao.py:1926-1929`). Não afirmar que cada chamador foi introduzido/alterado pelo diff sem a base.

- **Cadeia atual comprovada:** o resolver enumera `_ancoradas()` e chama `reabrir_credor_de_pergunta_viva` antes dos carimbos (`app/chamados/services/disputa_emissao.py:1937-1947`). `_ancoradas` inclui perguntas não validadas por destino ou pai sem destino, sem filtro de disputa aberta ou materialização (`app/chamados/models.py:336-347`). A cura exige pergunta sem `respondida_em`, viva pelo C6 e credor fora de VIVOS (`app/chamados/services/disputa_emissao.py:1821-1828`).

- **Reabertura é condicional, com efeito real:** a cura mede toda carga de S2/ressuscitação (`app/chamados/services/disputa_emissao.py:1792-1802`), recusa exportada/indeterminada ou limbo e pergunta às guardas canônicas (`app/chamados/services/disputa_emissao.py:1835-1849`). A transição REABRIR existe somente para resolvido/fechado → em_analise (`app/chamados/catalogo/motor.py:113`). `reabrir` grava o estado e depois chama ressuscitação (`app/chamados/models.py:1440-1452`); `_gravar_estado` relavra S2 quando cruza ENCERRADOS, para perguntas por destino não validadas (`app/chamados/models.py:273`, `:303-312`). Ressuscitação limpa carimbos e pode reabrir disputas fechadas (`app/chamados/models.py:1562-1575`).

- **A cura automática não equivale ao backfill DRY:** `executar=True` é o default, e o resolver não passa False (`app/chamados/services/disputa_emissao.py:1805`, `:1945`). A reabertura deixa trilhas normais: evento de estado (`app/chamados/models.py:292-296`), log de ressuscitação (`:1587-1598`), nota de disputa reaberta (`:2177-2180`) e nota da cura (`app/chamados/services/disputa_emissao.py:1850-1852`). Presença/ausência de reversão em `logs/` e execução na frota: **[nao verificado]**.

- **F1 deixa passar exportada e indeterminada:** `_vivas_entre` ignora a pergunta quando `competencia_do_dia_entregue(...) is not False` (`app/chamados/models.py:364-365`). Essa autoridade retorna None para dia ausente e bool de K6 nos demais casos (`app/chamados/services/vigia_fio.py:822-844`). O tripwire usa `_vivas_entre` (`app/chamados/models.py:221-227`), enquanto a cura recusa a carga com o mesmo `is not False` (`app/chamados/services/disputa_emissao.py:1835-1844`). C6 não tem veto equivalente por competência exportada; tranca é outra condição (`app/chamados/juizes.py:1331-1343`, `:1377-1406`). Assim, o caminho de fechamento seguido de recusa está presente no código; ocorrência concreta em produção: **[nao verificado]**.

- **Precisão no contraexemplo do anexo:** a pergunta viva exportada e as mortas na mesma disputa comprovam que o retorno antecipado impede carimbar as mortas (`app/chamados/services/disputa_emissao.py:1946-1947`, `:1950-1985`). Não comprovam que **essa mesma disputa** deveria fechar: C7 exige nenhuma pergunta viva e nenhuma resposta sem veredito (`app/chamados/models.py:1848-1853`). O retorno também pula o fecho de **outras** disputas abertas do colaborador, pois HX-DISPUTA-ORFA varreria todo esse universo (`app/chamados/services/disputa_emissao.py:2021-2023`, `:2069-2088`). O teste existente exige expressamente não carimbar nem fechar após recusa (`app/chamados/tests/test_e4b_classe_e_memoria.py:960-972`); execução do teste nesta análise: **[nao verificado]**.

- **Achado 3 do anexo não procede na cópia:** os tetos 213 e 269 são entradas ativas em `TETOS` (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`), cobradas por `assertLessEqual` (`:292-294`). A linha 213 é comentário e 269 define `setUpTestData` (`:213`, `:269`). Resultado de execução dos tetos: **[nao verificado]**.

## 11/11 12:16 · sessao 74cb46222204 · MAPA-20 · verificador · origem=Code

**A cura fecha os quatro exemplos, mas não fecha as classes dos achados 1 e 2. Permanecem achados procedentes, de severidade alta:** tabela HTML adicional sem julgamento e “encerrada” renderizada a partir de entidade ou palavra dividida por tags. (`bin/mapa20_valida.py:126`, `bin/mapa20_valida.py:130`, `bin/mapa20_valida.py:56`, `bin/mapa20_valida.py:193`)

As reproduções abaixo usaram mapas derivados de `mapa()`, com E3 em `1 de 2`, alterados somente em memória. A renderização auxiliar usou CommonMark com extensão de tabelas; o renderizador efetivo da publicação é [nao verificado]. (`bin/tests/test_mapa20_valida.sh:27`, `bin/tests/test_mapa20_valida.sh:32`)

**1. Formas alternativas e decisão do validador**

“Passa” significa ausência de problemas sobre o mapa válido modificado; a tradução disso em rc 0 está em `bin/mapa20_valida.py:218`.

| Forma examinada | Resultado e decisão | Achado / severidade |
|---|---|---|
| Linha sem borda esquerda colada depois da tabela | **Recusa**, pela linha seguinte não branca. (`bin/mapa20_valida.py:135`) | Caso original curado. |
| Segunda tabela Markdown sem pipes nas bordas | **Recusa**, pelos pipes fora do bloco. (`bin/mapa20_valida.py:137`) | Caso original curado. |
| `&#124;` / `&vert;` em célula ou como delimitadores de uma segunda “tabela” | **Passa**: divisão e guarda procuram pipe literal. Na renderização auxiliar, entidades aparecem como pipes de texto, sem criar outra tabela. (`bin/mapa20_valida.py:92`, `bin/mapa20_valida.py:137`) | Evasão estrutural não procedente nesses exemplos. |
| `enc&#101;rrada` ou `encerrad&#97;` em célula | **Passa com X<N**: a palavra não existe contiguamente no texto examinado, mas renderiza “encerrada”. (`bin/mapa20_valida.py:56`, `bin/mapa20_valida.py:67`) | **Procedente / alta.** |
| Segunda `<table>…<td>E3</td><td>encerrada</td>…</table>`, após branca | **Passa**: sem pipe literal, não entra no bloco nem na checagem de encerramento. A guarda HTML proíbe apenas comentários. (`bin/mapa20_valida.py:126`, `bin/mapa20_valida.py:130`, `bin/mapa20_valida.py:193`) | **Procedente / alta**; segunda tabela no render auxiliar. Produção [nao verificado]. |
| `<em>encerrada</em>` | **Recusa**: a palavra literal continua presente. (`bin/mapa20_valida.py:56`, `bin/mapa20_valida.py:193`) | Evasão não procedente. |
| `enc<em>errada</em>` | **Passa com X<N**: não há remoção das tags antes da regex; visualmente a palavra fica completa. (`bin/mapa20_valida.py:56`, `bin/mapa20_valida.py:67`) | **Procedente / alta.** |
| `<br>encerrada` ou `encerrada<br>` | **Recusa**, pois mantém a palavra literal. `enc<br>errada` **passa**, mas produz quebra dentro da palavra. (`bin/mapa20_valida.py:56`, `bin/mapa20_valida.py:193`) | Palavra inteira com `<br>` adjacente: evasão não procedente. |
| Largura zero dentro da palavra, como `encer​rada` com U+200B | **Passa com X<N**: a regex exige os caracteres contíguos. Aparência efetiva na tela é [nao verificado]. (`bin/mapa20_valida.py:56`) | Lacuna procedente; impacto visual [nao verificado]. |
| Linha seguinte contendo somente espaços/tab | **Passa**, porque `.strip()` resulta vazio; encerrou a tabela no render auxiliar. (`bin/mapa20_valida.py:135`) | Evasão não procedente. |
| CRLF | **Passa**: `splitlines()` separa as linhas; a leitura textual também aceita CRLF. (`bin/mapa20_valida.py:123`, `bin/mapa20_valida.py:209`) | Evasão não procedente. |
| Pipe escapado dentro da tabela | **Recusa**, pela proibição de qualquer barra invertida. (`bin/mapa20_valida.py:145`) | Evasão não procedente. |
| Pipe escapado fora da tabela | **Recusa**, porque a guarda não distingue `\|` de delimitador. (`bin/mapa20_valida.py:137`) | Restrição mais forte que a sintaxe Markdown. |
| Segunda tabela em lista | **Recusa**: pipes fora do bloco; se começar com pipe indentado, a guarda de indentação decide antes. (`bin/mapa20_valida.py:128`, `bin/mapa20_valida.py:137`) | Evasão não procedente. |
| `- item` ou `1. item` imediatamente antes da tabela canônica | **Passa**, pois falta guarda da fronteira anterior. No render auxiliar, as linhas viram continuação do parágrafo da lista: **zero tabelas**. (`bin/mapa20_valida.py:130`, `bin/mapa20_valida.py:135`) | **Procedente / média**, forma visual não assegurada. |
| Segunda tabela toda prefixada por `>` | **Recusa**, pelos pipes fora do bloco. (`bin/mapa20_valida.py:137`) | Evasão não procedente. |
| `> quote` imediatamente antes da tabela canônica | **Passa**; no render auxiliar, a tabela vira continuação do parágrafo da citação: **zero tabelas**. Com branca entre ambos, há tabela normal. (`bin/mapa20_valida.py:130`, `bin/mapa20_valida.py:135`) | **Procedente / média**, falta fronteira anterior. |
| Heading setext antes da tabela ou separado depois por branca | **Passa** como conteúdo externo; palavras desse heading não são julgadas. `---`/`===` imediatamente depois da tabela **recusam** pela linha não branca. (`bin/mapa20_valida.py:135`, `bin/mapa20_valida.py:159`, `bin/mapa20_valida.py:193`) | Não cria segunda tabela nos exemplos; julgamento restrito às células. |

Também passa um `<pre>` ou `<table>` aberto antes da tabela canônica, com branca antes do fechamento: no render auxiliar, os pipes ficam em HTML bruto. Isso reforça a falta de equivalência entre “bloco julgado” e “tabela mostrada”. **Procedente / média**; resultado na publicação [nao verificado]. (`bin/mapa20_valida.py:126`, `bin/mapa20_valida.py:130`, `bin/mapa20_valida.py:135`)

**2. Compatibilidade com escritor e publicador** [nao verificado]

**Regressão no formato fabricado pelo selo: não procedente.** `mapa()` põe pipes nas bordas, prosa sem pipes e linha vazia após a tabela. O fixture do publicador termina em EOF, também aceito: a nova regra só exige branca quando existe linha seguinte. (`bin/tests/test_mapa20_valida.sh:25`, `bin/tests/test_mapa20_valida.sh:31`, `bin/tests/test_mapa20_valida.sh:33`, `bin/tests/test_mapa20_valida.sh:128`, `bin/mapa20_valida.py:135`)

**Regressão no fluxo do publicador: não procedente na leitura do código.** Ele copia o mapa, valida a cópia, seleciona essa mesma cópia para raspagem quando verde e mantém rc 1 quando vermelho, inclusive sem alterações para publicar. (`bin/relato.sh:94`, `bin/relato.sh:97`, `bin/relato.sh:98`, `bin/relato.sh:100`, `bin/relato.sh:143`, `bin/relato.sh:158`, `bin/relato.sh:165`)

Compatibilidade com o escritor produtivo real é **[nao verificado]**: ele está apenas declarado como `logs/MAPA-20/montar_mapa.py`; `logs/` é ignorado. (`bin/mapa20_valida.py:10`, `.gitignore:53`)

**3. Fronteira, remoção de nomes/caminhos e negação**

**Falso positivo procedente / média, introduzido para sublinhados:** `nao _encerrada_`, `_nao encerrada_` e `__nao encerrada__` agora recusam. A nova fronteira encontra a palavra, mas `_NEGADA` exige `\b` antes de “nao” e somente espaços até “encerrada”; os sublinhados impedem a isenção. (`bin/mapa20_valida.py:52`, `bin/mapa20_valida.py:56`, `bin/mapa20_valida.py:68`)

`nao **encerrada**`, `nao <em>encerrada</em>` e `n&#227;o encerrada` também recusam; são limitações preexistentes. `nao encerrada`, `não encerrada` e `**nao encerrada**` passam. (`bin/mapa20_valida.py:52`, `bin/mapa20_valida.py:68`)

A remoção mantém as exceções intencionais para `BO-ROTULO-ENCERRADO` e `logs/encerrada.md:10`. Porém, substitui nomes sem fronteira: `encerradaE3` vira `encerrada ` e recusa; `nao E3 encerrada` e `nao logs/estado.md encerrada` viram negação contígua e passam. O sentido afirmativo dessas últimas frases é **[nao verificado]**; não constituem prova inequívoca de evasão. (`bin/mapa20_valida.py:51`, `bin/mapa20_valida.py:61`, `bin/mapa20_valida.py:62`, `bin/mapa20_valida.py:63`, `bin/mapa20_valida.py:68`)

**4. Os quatro testes mordem pelo motivo certo** [nao verificado]

**Achado de teste inadequado: não procedente.** Na reconstrução anterior ao delta, feita em memória, os quatro passam; no atual, recusam pelas decisões abaixo. (`bin/tests/test_mapa20_valida.sh:62`, `bin/tests/test_mapa20_valida.sh:63`, `bin/tests/test_mapa20_valida.sh:65`, `bin/tests/test_mapa20_valida.sh:66`)

| Teste | Motivo atual | [nao verificado]
|---|---|
| Linha sem borda colada | Linha seguinte não branca. (`bin/mapa20_valida.py:135`) |
| Segunda tabela sem bordas | Pipe externo. (`bin/mapa20_valida.py:137`) |
| `_encerrada_` com X<N | Afirmação de encerramento incompleto. (`bin/mapa20_valida.py:193`) |
| `__encerrada__` com X<N | Afirmação de encerramento incompleto. (`bin/mapa20_valida.py:193`) |

O primeiro também cairia na guarda de pipes externos se só a guarda de linha colada fosse retirada: ele contém pipes internos. Portanto, não isola essa guarda. (`bin/tests/test_mapa20_valida.sh:62`, `bin/mapa20_valida.py:137`)

A execução integral do shell é **[nao verificado]**: ele cria temporários, arquivos e repositórios, contrariando a restrição desta revisão. (`bin/tests/test_mapa20_valida.sh:13`, `bin/tests/test_mapa20_valida.sh:37`, `bin/tests/test_mapa20_valida.sh:162`)

**5. QUEM ESCREVE e QUEM LÊ**

| Papel | Arquivo:linha |
|---|---|
| Escritor produtivo declarado: sessão / `montar_mapa.py` | Declaração em `bin/mapa20_valida.py:10`; implementação **[nao verificado]**. |
| Escritor sintético `mapa()` e gravação do teste | `bin/tests/test_mapa20_valida.sh:32`, `bin/tests/test_mapa20_valida.sh:37`. |
| Fixture do publicador; cópia para caminho imitado | `bin/tests/test_mapa20_valida.sh:128`, `bin/tests/test_mapa20_valida.sh:170`. |
| Simulação de regravação concorrente | `bin/tests/test_mapa20_valida.sh:147`. |
| Publicador: snapshot e arquivo público raspado | `bin/relato.sh:97`, `bin/relato.sh:143`, `bin/raspar_publico.py:40`. |
| Leitor produtivo do rc do validador | `if` em `bin/relato.sh:97`; falha registrada em `bin/relato.sh:100`. |
| Saída do validador | Impressa em `bin/mapa20_valida.py:216`; segue stdout pelo comando em `bin/relato.sh:97`, sem parser ali. |
| Leitores de rc nos testes | `bin/tests/test_mapa20_valida.sh:38`, `bin/tests/test_mapa20_valida.sh:109`, `bin/tests/test_mapa20_valida.sh:110`; wrapper em `bin/tests/test_mapa20_valida.sh:146`. |
| Leitores do resultado do publicador no teste | Captura saída/rc em `bin/tests/test_mapa20_valida.sh:175`, `bin/tests/test_mapa20_valida.sh:176`; comparação em `bin/tests/test_mapa20_valida.sh:184`. |
| Chamador declarado do publicador | `bin/hasner-relato.service:15`; alerta externo ou leitor operacional da falha é **[nao verificado]**. |

## 11/11 12:14 · sessao a92c83d22358 · - · - · origem=Ronald (tmux)

Achados procedentes no código; render testado em memória com `markdown_it` CommonMark + extensão table, renderer de produção [nao verificado].

| Variante sobre mapa válido | Validador | Render local / decisão |
|---|---|---|
| `&#124;` ou `&vert;` em célula | Passa | Mostra pipe literal na mesma célula; não cria coluna. `_celulas` divide só `|` literal: `bin/mapa20_valida.py:92`. |
| Segunda “tabela” usando apenas entidades como delimitadores | Passa | Renderiza parágrafo com pipes, sem segunda tabela; não é evasão estrutural observada. `bin/mapa20_valida.py:137`. |
| Segunda `<table><tr><td>E3</td><td>encerrada</td>…</tr></table>` após branca | **Passa** | Renderiza segunda tabela HTML. Guarda HTML só rejeita comentário, `bin/mapa20_valida.py:126`; coleta só linhas começadas por pipe, `:130`; julga encerrada só nas células coletadas, `:193`. **Alta/bloqueante.** |
| `enc&#101;rrada` em célula com 1 de 2 | **Passa** | Mostra `encerrada`; regex avalia bytes sem decodificar entidades: `bin/mapa20_valida.py:56`, `:67`, `:193`. **Alta/bloqueante.** |
| `<em>encerrada</em>` | Recusa | Palavra literal ainda casa: `bin/mapa20_valida.py:56`, `:193`. |
| `enc<em>errada</em>` | **Passa** | Leitor vê palavra encerrada com trecho em itálico; regex não remove tags: `bin/mapa20_valida.py:56`, `:67`. **Alta/bloqueante.** |
| `encerrada<br>` | Recusa | Palavra literal presente: `bin/mapa20_valida.py:56`, `:193`. |
| `encer\u200brada` | Passa | Mantém caractere invisível na saída; aparência efetiva depende de fonte/layout [nao verificado]. Regex exige palavra contígua: `bin/mapa20_valida.py:56`. |
| Branca com espaços/tab após tabela | Passa | `.strip()` considera branca e renderer encerra tabela: `bin/mapa20_valida.py:135`. |
| CRLF | Passa | `splitlines()` e leitura textual normalizam fronteiras: `bin/mapa20_valida.py:123`, `:209`. |
| `a\|b` dentro da tabela | Recusa | Proibição de qualquer barra invertida: `bin/mapa20_valida.py:145`. |
| Segunda tabela em lista, com pipes | Recusa | Pipe fora do bloco: `bin/mapa20_valida.py:137`; linhas canônicas indentadas caem antes em `:128`. |
| Segunda tabela com todas as linhas prefixadas por `>` | Recusa | Pipes fora: `bin/mapa20_valida.py:137`. |
| `> quote\n` imediatamente antes do cabeçalho canônico | **Passa** | **ZERO tabelas** no render local: todas as linhas canônicas viram continuação lazy do parágrafo do blockquote. Só há guarda de fronteira depois, `bin/mapa20_valida.py:135`; nenhuma antes, `:130`. **Média, forma fixa não assegurada.** |
| `- item\n` ou `1. item\n` imediatamente antes do cabeçalho | **Passa** | **ZERO tabelas**, continuação lazy da lista; mesmo defeito de fronteira anterior, `bin/mapa20_valida.py:130`, `:135`. **Média.** |
| Lista/blockquote seguidos de branca e tabela canônica em coluna zero | Passa | Uma tabela normal, sem evasão observada: `bin/mapa20_valida.py:130`. |
| Heading setext `titulo\n---\n` antes da tabela | Passa | Heading normal e uma tabela; não é evasão observada: `bin/mapa20_valida.py:130`. |
| `===` ou `---` imediatamente depois da tabela | Recusa | Qualquer linha seguinte não branca, embora `---` renderize como `<hr>` fora da tabela: `bin/mapa20_valida.py:135`. |
| Heading setext `encerrada\n---` separado por branca | Passa | Palavra fora da tabela fica sem julgamento; `bin/mapa20_valida.py:159`, `:193` só inspecionam bloco. |
| `<table>\n` antes da tabela e `\n\n</table>` depois | **Passa** | Zero tokens de tabela Markdown; pipes são HTML bruto. Também ocorre com `<pre>`, `<div>` e `<em>` em linha própria. Comentários são o único HTML vedado, `bin/mapa20_valida.py:126`; branca antes do fechamento satisfaz `:135`. **Média/alta conforme contrato de visibilidade.** |

Adicional: substituir todos os LF por VT, FF, NEL, U+2028 ou U+2029 mantém aprovação porque `splitlines()` os separa (`bin/mapa20_valida.py:123`), mas renderer local mostra zero tabelas. Interpretação do renderer de produção [nao verificado].

## 11/11 12:14 · sessao caef848ee75c · - · - · origem=Ronald (tmux)

Achados semânticos, todos reproduzidos em memória sem criar arquivos:

- **Procedente, alta:** a cura não fecha a classe de “encerrada” renderizada. Com `1 de 2`, `encerrad&#97;`, `encerr<em>ada</em>`, `encerrad\u200ba` e `encerrad\u200da` retornam zero problemas. `_ENCERRADA` procura caracteres contíguos; `_sem_nomes_e_caminhos` apenas remove nomes/caminhos, sem interpretar HTML ou caracteres invisíveis. Referências: `bin/mapa20_valida.py:56`, `bin/mapa20_valida.py:59`, `bin/mapa20_valida.py:67`, `bin/mapa20_valida.py:193`. A aparência exata dos caracteres invisíveis no publicador fica [nao verificado].

- **Procedente, média:** a cura introduz recusas de negação legítima com ênfase por `_`: `nao _encerrada_`, `_nao encerrada_` e `__nao encerrada__` retornam `"encerrada" com 1 de 2`. A nova fronteira encontra a palavra, mas `_NEGADA` exige `\b` antes de `nao` e somente espaços depois; os sublinhados impedem a isenção. Referências: `bin/mapa20_valida.py:52`, `bin/mapa20_valida.py:56`, `bin/mapa20_valida.py:68`, `bin/mapa20_valida.py:194`.

- **Procedente, média, preexistente:** `nao **encerrada**`, `nao <em>encerrada</em>` e `n&#227;o encerrada` também recusam. `nao encerrada`, `não encerrada` e `**nao encerrada**` passam. Referências: `bin/mapa20_valida.py:52`, `bin/mapa20_valida.py:68`.

- A remoção de nomes/caminhos continua preservando os casos intencionais `BO-ROTULO-ENCERRADO` e `logs/encerrada.md:10`. Referências: `bin/mapa20_valida.py:51`, `bin/mapa20_valida.py:61`, `bin/tests/test_mapa20_valida.sh:93`, `bin/tests/test_mapa20_valida.sh:95`.

- A remoção ocorre sem fronteiras, podendo fabricar contexto: `nao E3 encerrada` e `nao logs/estado.md encerrada` tornam-se `nao   encerrada` e passam; `encerradaE3` torna-se `encerrada ` e recusa. Se essas entradas representam uma afirmação ou texto legítimo na produção é [nao verificado]. Referências: `bin/mapa20_valida.py:61`, `bin/mapa20_valida.py:62`, `bin/mapa20_valida.py:63`, `bin/mapa20_valida.py:68`.

**Os quatro testes novos mordem pelo motivo certo; achado de teste inadequado não procedente.** Recriei o validador anterior exclusivamente em memória, removendo os dois novos bloqueios e restaurando `\bencerrad[ao]s?\b`: os quatro retornam zero problemas; no atual: [nao verificado]

| Caso | Resultado atual | Decisão |
|---|---|---|
| Linha sem borda colada | Recusa por linha colada | `bin/tests/test_mapa20_valida.sh:62`; `bin/mapa20_valida.py:135` |
| Segunda tabela sem bordas | Recusa por pipe fora | `bin/tests/test_mapa20_valida.sh:63`; `bin/mapa20_valida.py:137` |
| `_encerrada_` | Recusa por encerramento com `1 de 2` | `bin/tests/test_mapa20_valida.sh:65`; `bin/mapa20_valida.py:193` |
| `__encerrada__` | Recusa por encerramento com `1 de 2` | `bin/tests/test_mapa20_valida.sh:66`; `bin/mapa20_valida.py:193` |

A primeira entrada também cairia no bloqueio de pipe externo se somente o bloqueio de linha colada fosse removido; ela contém pipes internos. Referências: `bin/tests/test_mapa20_valida.sh:62`, `bin/mapa20_valida.py:137`.

## 11/11 12:13 · sessao 88846b740303 · - · - · origem=Ronald (tmux)

Compatibilidade da cura: **achado de regressão no formato fabricado pelo selo não procedente**. `mapa()` monta tabela com pipes nas bordas, prosa sem pipes fora dela e linha vazia depois dela; portanto não viola novas regras. `bin/tests/test_mapa20_valida.sh:25`, `:31`, `:32`, `:33`; decisões em `bin/mapa20_valida.py:135` e `:137`. O fixture adicional do publicador termina a tabela em EOF, também permitido, pois a condição exige existir linha seguinte. `bin/tests/test_mapa20_valida.sh:128`; `bin/mapa20_valida.py:135`.

No publicador, **regressão do fluxo não procedente**: o script copia `logs/MAPA-20/mapa.md` para snapshot e julga essa cópia; rc zero seleciona o par para publicação; falha fixa `MAPA_RC=1`, segue com o resto e devolve esse rc, inclusive quando nada mudou. `bin/relato.sh:94`, `:97`, `:98`, `:100`, `:143`, `:158`, `:165`. O raspador só transforma o snapshot depois do selo e escreve o destino público. `bin/relato.sh:143`; `bin/raspar_publico.py:28`, `:31`, `:40`. Resultado efetivo após raspagem: [nao verificado].

Quem escreve:

- Escritor produtivo é **declarado** como sessão, usando `logs/MAPA-20/montar_mapa.py`; implementação não está comprovada na cópia. `bin/mapa20_valida.py:10`; [nao verificado]. `logs/` está ignorado. `.gitignore:53`.
- Gerador sintético `mapa()`: `bin/tests/test_mapa20_valida.sh:32`; grava texto de teste em `:37`.
- Fixture sintético do publicador: `bin/tests/test_mapa20_valida.sh:128`; cópia para o caminho imitado em `:170`; fixture vermelho em `:171`.
- Wrapper sintético que simula regravação concorrente: `bin/tests/test_mapa20_valida.sh:147`.
- Publicador escreve snapshot por `cp`: `bin/relato.sh:97`; escreve `MAPA-20.md` através de `bin/raspar_publico.py:40`, selecionado em `bin/relato.sh:98` e `:143`.

Quem lê rc/saída:

- **Produção:** `bin/relato.sh:97` lê rc do validador pelo `if`; não faz parsing de sua saída, que segue stdout. `bin/relato.sh:97`, `:101`.
- **Teste do validador:** `bin/tests/test_mapa20_valida.sh:38` captura saída e retorna somente rc; `:109` compara rc esperado; `:110` verifica arquivo ausente; `:115` devolve falha.
- **Wrapper sintético:** `bin/tests/test_mapa20_valida.sh:146` lê rc e `:148` o repassa.
- **Teste do publicador:** `bin/tests/test_mapa20_valida.sh:175` captura saída do fluxo; `:176` guarda rc; `:180` lê esse rc; `:184` compara; `:185` exibe saída se falhar.
- **Chamador declarado do publicador:** unit executa `relato.sh`. `bin/hasner-relato.service:15`. Essa unit não declara `OnFailure`; tratamento/alerta externo e instalação viva: [nao verificado].

Severidade: nenhuma regressão demonstrada no formato sintético ou no fluxo; compatibilidade com o escritor produtivo real permanece [nao verificado]. `bin/tests/test_mapa20_valida.sh:33`; `bin/relato.sh:97`; `bin/mapa20_valida.py:10`.

## 11/11 12:12 · sessao c4149a800b40 · TRIAGEM-OFFLINE-SEM-CAMERA · censo · origem=Code

**Pela lei do sistema, NÃO: falta de selfie não deve impedir a batida. A cópia contém respostas contraditórias:** `apto` online libera; `apto` offline esconde a confirmação; v2 recusa explicitamente sem foto. (`CLAUDE.md:143-144`; `app/templates/ponto/triagem/apto.html:156-163`; `app/templates/ponto/triagem/apto.html:253-256`; `app/templates/ponto/bater_ponto_v2.html:285-290`.)

Inspeção somente leitura; não executei testes, banco ou logs. Identidade Git independente do hash informado e ocorrência concreta em produção: **[nao verificado]**. O anexo fornecido relata batida perdida; quantidade, usuários atingidos e frequência: **[nao verificado]**. [nao verificado]

**1. Censo dos sítios**

As rotas atuais selecionam triagem ou v2; `bater_ponto.html` permanece no censo como código existente, mas não é o template selecionado nessa view. (`app/ponto/urls.py:11-12`; `app/ponto/views.py:225-235`; `app/ponto/views.py:356-358`.)

| Sítio | Caminho | Resposta | Como |
|---|---|---|---|
| `apto`: abertura de câmera | Online | **Libera na falha**, inclusive permissão negada | O `catch` informa revisão e mostra `bloco-botoes`. Não diferencia causas. (`app/templates/ponto/triagem/apto.html:149-163`.) |
| `apto`: foto não capturada | Antes da escolha online/offline | **Bloqueia pela apresentação inicial** | Botões começam em `hidden`; ficam acessíveis pela confirmação ou pelo `catch`. (`app/templates/ponto/triagem/apto.html:93`; `app/templates/ponto/triagem/apto.html:156-185`.) |
| `apto`: conversão da foto | Online | **Libera com blob nulo**, se o callback ocorrer | Atribui o resultado sem validá-lo e mostra os botões. Não há proteção local para falha de desenho/conversão nem timeout do callback. (`app/templates/ponto/triagem/apto.html:167-186`.) |
| `apto`: refazer | Ambos, antes do envio | **Volta a esconder** | Zera foto, esconde os botões e tenta câmera novamente; uma rejeição volta ao `catch` permissivo. (`app/templates/ponto/triagem/apto.html:189-195`.) |
| `apto`: envio | Online | **Libera sem foto** | Anexa somente se `_fotoBlob` existir e submete o formulário. (`app/templates/ponto/triagem/apto.html:228-235`.) |
| `apto`: segunda câmera | Offline | **Bloqueia na rejeição** | Depois de desabilitar o botão principal, abre câmera offline; o `catch` esconde `btn-capturar-off`, pede supervisor e zera stream. (`app/templates/ponto/triagem/apto.html:210-212`; `app/templates/ponto/triagem/apto.html:236-256`.) |
| `apto`: confirmar sem stream | Offline | **Não tem veto por stream nulo** | Continua para `canvas.toBlob` e fila; o impedimento do clique está no ramo anterior. (`app/templates/ponto/triagem/apto.html:261-282`.) |
| `bater_ponto`: câmera inicial | Antes de online/offline | **Bloqueia implicitamente se continuar sem foto** | Botões nascem escondidos; o `catch` oferece câmera do sistema e telemetria, sem liberá-los. (`app/templates/ponto/bater_ponto.html:246`; `app/templates/ponto/bater_ponto.html:343-378`.) |
| `bater_ponto`: câmera do sistema | Ambos | **Bloqueia se cancelar** | Sem arquivo retorna; com arquivo libera somente no `FileReader.onload`. (`app/templates/ponto/bater_ponto.html:384-403`.) |
| `bater_ponto`: conversão | Ambos | **Libera em null, exceção ou timeout de `toBlob`** | Tenta dataURL e libera mesmo sem blob; salvaguarda de 1.500 ms. Entretanto, `drawImage`/`toDataURL` anteriores estão fora desse `try`. (`app/templates/ponto/bater_ponto.html:414-467`.) |
| `bater_ponto`: refazer | Ambos | **Volta a esconder** | Zera foto e reabre a câmera inicial. (`app/templates/ponto/bater_ponto.html:470-477`.) |
| `bater_ponto`: envio alcançado | Online/rede falha | **Libera sem foto** | Foto é condicional; falha de HTTP/rede enfileira `_fotoBlob`, inclusive nulo. (`app/templates/ponto/bater_ponto.html:519-543`.) |
| `bater_ponto`: segunda câmera | Offline | **Mantém confirmação disponível** | Mostra botão antes de pedir câmera; o `catch` apenas altera o texto. (`app/templates/ponto/bater_ponto.html:564-581`.) |
| `bater_ponto`: confirmação offline | Offline | **Sem veto explícito por foto/stream ausente** | Faz `toBlob` incondicional e passa o resultado à fila. Não protege o próprio `toBlob` com timeout ou `try`. (`app/templates/ponto/bater_ponto.html:584-610`.) |
| `bater_ponto`: confirmação de turno | Online | **Formulário sem requisito de selfie** | Envia confirmação/GPS sem campo de foto nessa etapa. (`app/templates/ponto/bater_ponto.html:174-196`.) |
| v2: câmera/fallback | V2 online/offline | **Fallback não libera sozinho** | Distingue `NotAllowedError`; oferece input do sistema. Seleção vazia retorna. (`app/templates/ponto/bater_ponto_v2.html:235-252`.) |
| v2: captura vazia/conversão | V2 online/offline | **Segue até a guarda de foto** | Sem vídeo pronto retorna foto anterior ou null; conversão tenta dataURL em null, exceção e timeout de 1.500 ms. (`app/templates/ponto/bater_ponto_v2.html:256-267`.) |
| v2: admissibilidade | **V2 online e offline** | **Bloqueia explicitamente** | Sem `_fotoBlob`, consulta permissão, mostra aviso e retorna false. O chamador retorna **antes** de escolher rede, POST ou fila. (`app/templates/ponto/bater_ponto_v2.html:285-290`; `app/templates/ponto/bater_ponto_v2.html:307-346`.) |
| Onboarding: etapa selfie | Configuração | **Etapa normal exige foto** | Falha oferece câmera do sistema; seleção vazia e `salvarFoto` sem blob retornam. Captura não trata null/timeout. (`app/templates/core/onboarding.html:152-200`; `app/templates/core/onboarding.html:212-213`.) |
| Onboarding: saída alternativa | Configuração/acesso | **Permite concluir sem selfie** | “Configurar depois” chama concluir; backend lista faltas, mas grava completo e responde `ok=True`. A view de bater ponto redireciona quem ainda não concluiu. (`app/templates/core/onboarding.html:120-125`; `app/core/views.py:479-489`; `app/ponto/views.py:243-244`.) |
| Enfileirador PWA | Offline/falha de envio | **Aceita sem foto** | Grava `fotoBlob || null`. (`app/static/pwa.js:107-122`.) |
| Sincronizador PWA | Retorno da rede | **Envia sem foto** | Só acrescenta arquivo quando presente. (`app/static/pwa.js:143-156`.) |
| Service worker | Offline/sync | **Não julga selfie** | Ignora não-GET; no sync delega às janelas ou notifica para abrir o app. (`app/static/sw.js:35-36`; `app/static/sw.js:112-129`.) |
| View web | Online triagem | **Aceita ausência de foto** | Registra primeiro; anexa somente quando existe foto. (`app/ponto/services/triagem_batida.py:700-713`; `app/ponto/services/triagem_batida.py:748-751`.) |
| API, duas cópias | V2/sync/app | **Aceita e sinaliza ausência** | Acrescenta `foto_ausente`; registro e anexação são separados. (`app/api/views.py:449-465`; `app/api/views_core.py:436-452`; `app/api/views_core.py:658-672`; `app/api/views_core.py:776-782`.) |
| Endpoints de anexação | Depois da batida | **Exigem foto para anexar** | Operam sobre batida existente; não são portas de criação da batida. (`app/api/views.py:847-859`; `app/api/views_core.py:832-844`; `app/ponto/views.py:2895-2905`.) |
| Endpoint offline antigo | Legado | **410, independentemente da selfie** | Foi aposentado em favor da API atual. (`app/ponto/views.py:419-428`.) |

Não há timeout próprio de **`getUserMedia`** nos ramos examinados; os 1.500 ms protegem conversão de imagem. Quando a promessa de câmera permanece pendente, não existe liberação alternativa temporizada nesses trechos; comportamento concreto no dispositivo: **[nao verificado]**. (`app/templates/ponto/triagem/apto.html:149-164`; `app/templates/ponto/triagem/apto.html:242-257`; `app/templates/ponto/bater_ponto.html:343-378`; `app/templates/ponto/bater_ponto_v2.html:235-267`; `app/templates/core/onboarding.html:152-162`.)

**2. `confirmarFotoOffline`, fila e servidor**

Com `_streamOffline === null`, `confirmarFotoOffline()` **não retorna por esse motivo**. O stream só condiciona parar as tracks. O desenho depende da visibilidade do vídeo; depois chama `canvas.toBlob` e enfileira seu resultado sem exigir foto. Também não reaproveita `_fotoBlob` da captura anterior. (`app/templates/ponto/triagem/apto.html:261-282`.)

Portanto, chamar essa função por outro meio pode alcançar a fila mesmo sem stream. Isso **não comprova selfie capturada**: o canvas pode não ter recebido frame nessa chamada; conteúdo efetivamente produzido pelo navegador: **[nao verificado]**. (`app/templates/ponto/triagem/apto.html:266-282`.)

A fila localizada é IndexedDB `juliani-offline`, store `batidas_pendentes`; aceita `foto:null`. Não foi localizada fila de batida em `localStorage` ou em `app/static/js`; implementação adicional: **[nao verificado]**. (`app/static/pwa.js:48-60`; `app/static/pwa.js:107-122`.)

O sync envia para `/api/ponto/bater/`, omitindo foto ausente. Sucesso remove a entrada; HTTP 400 também remove; rede/5xx preservam para retentativa. O SW não faz essa gravação diretamente. (`app/static/pwa.js:143-179`; `app/static/sw.js:112-129`.)

O chokepoint `registrar_batida(colaborador, timestamp, tipo, origem, ...)` **não exige foto nem permissão de câmera**; o modelo declara selfie opcional. Entretanto, isso não significa aceitação incondicional por outros motivos: há deduplicação e recusa por colisão de tipo oposto, além da guarda de competência para fato que não seja de chão. (`app/ponto/registro_batida.py:94-161`; `app/ponto/models.py:78`.)

**3. Fonte única e carregamento comum**

Não identifiquei autoridade consultada pelos fronts para **“selfie é impeditiva?”**. As decisões estão locais e contraditórias. Existe `selfie_ausente()`, mas sua pergunta é **cobrança da ausência**, por canal/prazo, e seus consumidores são backend; não é admissibilidade da captura. (`app/templates/ponto/triagem/apto.html:161-163`; `app/templates/ponto/triagem/apto.html:253-256`; `app/templates/ponto/bater_ponto_v2.html:285-290`; `app/ponto/registro_batida.py:64-91`; `app/core/juizes.py:1192-1195`.)

| Tela | Herança e scripts próprios |
|---|---|
| `apto` | Estende `base_triagem`, que estende `base.html`; acrescenta JS inline por `extra_js`. (`app/templates/ponto/triagem/apto.html:1`; `app/templates/ponto/triagem/base_triagem.html:1`; `app/templates/ponto/triagem/base_triagem.html:25-76`.) |
| `bater_ponto` | Estende `base.html`; lógica própria inline. (`app/templates/ponto/bater_ponto.html:1`; `app/templates/ponto/bater_ponto.html:285`.) |
| v2 | Estende `base.html`; acrescenta `/static/leaflet/leaflet.js` e lógica inline. (`app/templates/ponto/bater_ponto_v2.html:1`; `app/templates/ponto/bater_ponto_v2.html:135-137`.) |
| Onboarding | Documento independente, JS inline. (`app/templates/core/onboarding.html:1`; `app/templates/core/onboarding.html:115-330`.) |

Scripts herdados pelas três telas via `base.html`: `pwa.js`; `js/hasner-ui.js?v=hx3`; `hasner-table.js`; `hasner-filter.js?v=ta5`; `fita-mes.js?v=1`; `geo-colab-map.js`; `popover-acao.js`. Também carrega htmx externo. (`app/templates/base.html:19-20`; `app/templates/base.html:83-88`; `app/templates/base.html:440`.)

Includes da base: `_nudge_app_android` condicional, `includes/icons`, `_icone_pauta`, `_dialogo`, `_js_cobranca`, `_drawer_generico`, `_copiloto`; este também inclui `_icone_pauta`. (`app/templates/base.html:92-93`; `app/templates/base.html:290`; `app/templates/base.html:388-399`; `app/templates/base.html:432`; `app/templates/core/_copiloto.html:34`.)

Na outra casca, `base_app.html`, estão `pwa.js`, `hasner-ui`, `hasner-table`, `geo-colab-map`, `popover-acao` e htmx; includes `icons`, `_dialogo`, `_drawer_generico`. O teste de templates compõe apto/v2 com ambas as cascas. (`app/templates/base_app.html:19-20`; `app/templates/base_app.html:77-85`; `app/templates/base_app.html:145-154`; `app/templates/base_app.html:194`; `app/ponto/tests/test_l030_templates_renderizados.py:14-37`.)

**Inferência de localização:** o menor módulo existente ligado à batida e já carregado nas três telas é **`app/static/pwa.js`**, também presente nas duas cascas. É candidato para hospedar a resposta compartilhada sem acrescentar uma regra de domínio. (`app/templates/base.html:20`; `app/templates/base_app.html:20`; `app/static/pwa.js:305-318`.)

**4. Quem escreve e quem lê os sinais**

| Dado/sinal | Escritor | Leitor/efeito |
|---|---|---|
| Falha de câmera no `apto` | Escreve apenas texto/DOM; não envia causa específica. (`app/templates/ponto/triagem/apto.html:156-163`; `app/templates/ponto/triagem/apto.html:228-235`.) | Não há leitor de causa por batida nesse fluxo; a foto simplesmente pode faltar. (`app/ponto/services/triagem_batida.py:748-751`.) |
| Diagnóstico de câmera do `bater_ponto` | Envia erro, modo e user agent para `/api/diag/cam/`. (`app/templates/ponto/bater_ponto.html:367-375`.) | Endpoint escreve logger e responde `logged`; não associa sinal ao modelo da batida. Logs concretos: **[nao verificado]**. (`app/api/views.py:2647-2669`.) |
| Foto ausente na fila | `registrarBatidaOffline` grava `foto:null`; não grava motivo da ausência. (`app/static/pwa.js:113-119`.) | Sync lê `b.foto` e omite arquivo ausente. (`app/static/pwa.js:138-152`.) |
| `liveness_status/detail` | Sync sempre envia `indisponivel` / `PWA web sem liveness`. (`app/static/pwa.js:148-151`.) | APIs convertem em motivo/contexto `liveness_indisponivel`; não distingue câmera negada de outras causas. (`app/api/views.py:491-507`; `app/api/views_core.py:478-494`.) |
| `canal` | Derivado por `canal_da_batida(device_id)`; gravado nas APIs e no web. (`app/ponto/registro_batida.py:53-61`; `app/api/views_core.py:658-662`; `app/api/views.py:666-670`; `app/ponto/services/triagem_batida.py:700-705`.) | `selfie_ausente`/retro usam canal para prazo; modelo guarda o campo. (`app/ponto/registro_batida.py:75-91`; `app/ponto/models.py:55`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:73-77`.) |
| `foto_ausente` no ato | As duas APIs acrescentam motivo/contexto pela resposta de `selfie_ausente`. (`app/api/views.py:449-465`; `app/api/views_core.py:436-452`.) | API encaminha à emissão de disputa; persistência em `DisputaSupervisao.motivos/contexto`, geração de perguntas pelo serviço. (`app/api/views_core.py:763-774`; `app/chamados/models.py:1741-1747`; `app/chamados/services/disputa_emissao.py:634-685`.) |
| Foto efetivamente anexada | `anexar_foto`: escreve `Batida.foto` e, conforme modalidade, `FotoBatida`. (`app/ponto/registro_batida.py:296-311`.) | Retro lê **as duas fontes**; feed exibe foto ou ícone substituto. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:66-77`; `app/templates/core/_d2_feed.html:6-7`.) |
| `foto_ausente` retroativo | Comando consulta juiz e em `--apply` emite disputa. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:73-104`.) | Catálogo pergunta confirmação de presença; upload posterior lê motivo/última batida e apara ou fecha disputa. (`app/chamados/catalogo/perguntas.py:93-98`; `app/api/foto_recebida.py:57-80`.) |
| Permissão de câmera do onboarding | Front envia `{camera:true}`; backend grava `camera_concedida_em`. (`app/templates/core/onboarding.html:226-230`; `app/core/views.py:1085-1090`.) | `etapas_pendentes` e concluir onboarding leem esse campo de **Colaborador**, não de Batida. (`app/colaboradores/models.py:376`; `app/colaboradores/models.py:405-413`; `app/core/views.py:481-486`.) |

**Offline e online da triagem não gravam o mesmo sinal no ato.** O offline, ao sincronizar pela API, produz `foto_ausente` e liveness indisponível; o web registra sem foto, mas sua aprovação/justificativa considera horário/situação, sem consultar o juiz de selfie. A varredura posterior pode cobrar a ausência, sem recuperar a causa “câmera indisponível”. (`app/static/pwa.js:148-153`; `app/api/views_core.py:436-452`; `app/ponto/services/triagem_batida.py:685-713`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:73-104`.)

Não há campo específico `camera_indisponivel`/`sem_selfie` em `Batida`; os sinais citados vivem na ausência das fotos e nos motivos/contexto da disputa. (`app/ponto/models.py:8-112`; `app/chamados/models.py:1727-1747`.)

**5. Testes e selos existentes** [nao verificado]

Resultados atuais de execução: **[nao verificado]**. A tabela descreve o que o código dos testes verifica. [nao verificado]

| Teste/selo | Cobertura e capacidade de detectar regressão | [nao verificado]
|---|---|
| `test_api_bater_pwa` | Sem foto exige 200, Batida e `foto_ausente`; com foto exige ausência desse motivo. **Detectaria rejeição backend**, não bloqueio de clique. (`app/api/tests/test_api_bater_pwa.py:64-76`; `app/api/tests/test_api_bater_pwa.py:95-105`.) |
| `test_batida_offline` | Payload sem foto espera registro pela API e disputa; não exercita câmera/IndexedDB/clique. (`app/ponto/tests/test_batida_offline.py:49-76`; `app/ponto/tests/test_batida_offline.py:119-128`.) |
| `test_selfie_ausente_juiz` | Verifica canal/prazo e consulta ao juiz nas duas APIs. (`app/ponto/tests/test_selfie_ausente_juiz.py:27-47`; `app/ponto/tests/test_selfie_ausente_juiz.py:130-140`.) |
| `test_l030_forma_js` | Verifica barreiras de **GPS**; filtra condições por variáveis GPS. No apto, mede o trecho depois de `toBlob`. **Não detecta** o botão escondido no catch nem a guarda de foto da v2. (`app/ponto/tests/test_l030_forma_js.py:31-68`.) |
| `test_l030_templates_renderizados` | Aplica o mesmo oráculo a apto/v2 nas duas cascas; mantém a limitação anterior. (`app/ponto/tests/test_l030_templates_renderizados.py:14-37`.) |
| `check_a_lei_batida.py` | Procura assinaturas estreitas em apto/bater; foto+return na mesma linha. **Não enumera v2 nem procura botão escondido.** Checks de view apontam `app/apps/ponto/views.py`; arquivo ausente é pulado. (`app/scripts/check_a_lei_batida.py:13-35`.) |
| `node_check.sh` | Só sintaxe de JS estático próprio; `<script>` inline fica fora. **Não detecta essa regressão de comportamento.** (`bin/node_check.sh:11-20`; `bin/node_check.sh:33-37`.) |
| Smoke Chromium geral | Abre espelho nas duas cascas, verifica overlay/console; não registra batida nem exercita câmera. (`app/core/tests/test_smoke_chromium.py:151-189`.) |
| Teste do SW | Verifica fallback de rede/cache/Response, não selfie. (`app/core/tests/test_sw_offline_fallback.py:60-101`.) |
| Smoke de clique sem câmera nas duas cascas | Existência/cobertura específica: **[nao verificado]**. |

**6. Arquivos candidatos para conferir interseção**

Lista inferida dos pontos de decisão e cobertura; não representa alteração realizada. (`app/templates/ponto/triagem/apto.html:253-256`; `app/templates/ponto/bater_ponto_v2.html:285-290`; `app/scripts/check_a_lei_batida.py:13-30`.)

| Escopo | Arquivos candidatos e fundamento |
|---|---|
| Resposta compartilhada e migração dos fronts | `app/static/pwa.js`; `app/templates/ponto/triagem/apto.html`; `app/templates/ponto/bater_ponto_v2.html`; `app/templates/ponto/bater_ponto.html`. Já concentram fila e decisões locais. (`app/static/pwa.js:107`; `app/templates/ponto/triagem/apto.html:156-282`; `app/templates/ponto/bater_ponto_v2.html:285-346`; `app/templates/ponto/bater_ponto.html:343-610`.) |
| Proteção contra retorno do bloqueio | `app/scripts/check_a_lei_batida.py`; família `test_l030_forma_js`/`test_l030_templates_renderizados`; smoke de clique a acrescentar em arquivo ainda **[nao verificado]**. (`app/scripts/check_a_lei_batida.py:13-35`; `app/ponto/tests/test_l030_forma_js.py:31-68`; `app/ponto/tests/test_l030_templates_renderizados.py:14-37`.) |
| Se o escopo incluir uniformizar o sinal no ato | `app/ponto/services/triagem_batida.py`, `app/api/views.py`, `app/api/views_core.py`, com referência à cobrança existente em `app/ponto/registro_batida.py`. Essa diferença é separada do botão offline. (`app/ponto/services/triagem_batida.py:685-751`; `app/api/views.py:449-465`; `app/api/views_core.py:436-452`; `app/ponto/registro_batida.py:64-91`.) |
| Se houver mudança de carregamento | `app/templates/base.html` e `app/templates/base_app.html`; `pwa.js` já está carregado em ambas. (`app/templates/base.html:20`; `app/templates/base_app.html:20`.) |
| Onboarding, se incluído na cl

[truncado em 20000 de 20621 caracteres]

## 11/11 12:11 · sessao 4cfb93cca4fc · BO-ROTULO-ENCERRADO · verificador · origem=Code

**Bloqueia: o selo permite reintroduzir os dois estados com o mesmo texto visível.** A correção atual centraliza os rótulos, mas os testes deixam escapar opções com aspas simples e opções vindas de partials. ([test_bo_rotulo_encerrado.py:20](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:20), [test_bo_rotulo_encerrado.py:65](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:65))

1. **Censo dos controles.** Não encontrei seletor de destino remanescente com opções de estado manuais ou lendo `espera`. Os cinco encontrados usam a tag:

   | Seletor | Valores, na ordem atual |
   |---|---|
   | [detalhe.html:130](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe.html:130) | `""`, `em_analise`, `resolvido`, `fechado`, `aberto` |
   | [detalhe_local.html:393](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe_local.html:393) | `""`, `em_analise`, `resolvido`, `fechado`, `aberto` |
   | [fio_colaborador.html:265](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/fio_colaborador.html:265) | `""`, `em_analise`, `resolvido`, `fechado` |
   | [modal_fio.html:665](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/partials/modal_fio.html:665) | `""`, `em_analise`, `resolvido`, `fechado` |
   | [painel_chamado_turno.html:74](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/partials/painel_chamado_turno.html:74) | `""`, `em_analise`, `resolvido`, `fechado` |

   Há controles manuais de **ações** que alcançam mudança de estado, sem oferecer opções de destino pelo mapa:

   | Controle manual | Onde / escritor |
   |---|---|
   | Aprovar/rejeitar benefício | [painel_solicitacao_beneficio.html:23](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/colaboradores/painel_solicitacao_beneficio.html:23), também construído por JS em [fila_solicitacoes_beneficio.html:138](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/colaboradores/fila_solicitacoes_beneficio.html:138); escolhe `resolvido`/`fechado` em [views.py:2068](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/colaboradores/views.py:2068). |
   | Aceitar/recusar escala e agenda | [detalhe_local.html:163](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe_local.html:163), [detalhe_local.html:200](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe_local.html:200); concluem em `resolvido` em [acoes_chamado.py:149](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/acoes_chamado.py:149) e [acoes_chamado.py:108](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/acoes_chamado.py:108). |
   | Aprovar exceção | [detalhe_local.html:256](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe_local.html:256), [modal_fio.html:598](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/partials/modal_fio.html:598); `resolvido` em [acoes_chamado.py:70](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/acoes_chamado.py:70). |
   | Autorizar geofence, com/sem advertência, ou recusar | [detalhe_local.html:285](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe_local.html:285), [modal_fio.html:506](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/partials/modal_fio.html:506); `resolvido` em [acoes_chamado.py:222](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/acoes_chamado.py:222) e [acoes_chamado.py:277](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/acoes_chamado.py:277). |
   | Abrir registro de ausência | [detalhe_local.html:233](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe_local.html:233); chama `analisar()` em [views.py:1513](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/views.py:1513). |
   | Reabertura administrativa; resgatar | [modal_fio.html:390](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/partials/modal_fio.html:390) oferece **motivos** e reabre em [acoes_disputa.py:269](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/acoes_disputa.py:269); [botão resgatar:85](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/partials/_lista_chamados.html:85) pode reabrir pelo caso da tranca em [views_arquivar.py:64](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/views_arquivar.py:64). |

   O seletor manual fora de chamados, no histórico, é filtro: seu receptor aplica `qs.filter`, sem transição. ([historico_chamados.html:54](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/relatorios/historico_chamados.html:54), [relatorios/views.py:920](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/relatorios/views.py:920))

2. **Quem escreve e quem lê; dois achados de fila.**

   - **`value`:** a tag escreve exatamente o código recebido, separado do texto e do `title`. Os argumentos vêm das chamadas literais da tabela acima. ([chamados_tags.py:76](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/templatetags/chamados_tags.py:76))
   - **Local e painel de turno:** chegam a `detalhe_chamado_local`, que delega à resposta livre; o serviço exige gestor e código pertencente a `STATUS_CHOICES`, depois chama `transicionar`. ([views.py:547](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/views.py:547), [acoes_chamado.py:287](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/acoes_chamado.py:287), [acoes_chamado.py:313](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/acoes_chamado.py:313))
   - **Fio e modal:** delegam ao mesmo `processar_post_fio`; ele lê o código, exige chamado/conteúdo e permissão contextual, verifica `STATUS_CHOICES` e chama `transicionar`. ([views.py:669](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/views.py:669), [views.py:953](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/views.py:953), [fio.py:502](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/fio.py:502), [fio.py:709](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/fio.py:709))
   - **Fila — detalhe GLPI sem efeito de status:** envia `novo_status` para `followup`, mas `adicionar_followup` só lê conteúdo e envia a mensagem ao GLPI; nenhum dos quatro destinos é aplicado nesse caminho. ([detalhe.html:122](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe.html:122), [urls.py:12](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/urls.py:12), [views.py:292](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/views.py:292), [views.py:304](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/views.py:304))
   - **Fila — “Reabrir” pode ser recusado:** envia `aberto`; os receptores locais passam esse código ao modelo, mas `resolvido → aberto` é recusado para quem não é `admin_ti`. A matriz permite `em_analise → aberto`. ([verbos.py:136](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/verbos.py:136), [motor.py:99](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/catalogo/motor.py:99), [motor.py:153](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/catalogo/motor.py:153))

   `MOVER_PARA` tem definição única e é lido por `rotulo_de_movimento`; `ESTADO_TITLE` também alimenta `rotulo_de_linha` e `rotulo_para_pessoa`. A tag só monta HTML; seus leitores são os cinco templates e os testes. Não encontrei comparação do texto visível nos receptores: eles comparam códigos. ([verbos.py:100](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/verbos.py:100), [verbos.py:132](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/verbos.py:132), [verbos.py:146](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/verbos.py:146), [verbos.py:155](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/verbos.py:155), [verbos.py:166](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/verbos.py:166))

   A decisão e gravação continuam por `transicionar → decidir → _aplicar → _gravar_estado`, incluindo as guardas adicionais do modelo. ([models.py:123](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/models.py:123), [models.py:128](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/models.py:128), [models.py:199](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/models.py:199), [models.py:450](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/models.py:450))

3. **O diff fornecido preserva conjunto, ordem e `value`.** Os dois detalhes conservam quatro destinos; os demais, três, nas ordens da tabela. A tag escreve o próprio argumento como `value`. ([detalhe.html:132](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe.html:132), [detalhe_local.html:395](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe_local.html:395), [modal_fio.html:668](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/partials/modal_fio.html:668), [chamados_tags.py:77](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/templatetags/chamados_tags.py:77))

   **Fila — essa preservação não está protegida pelo selo:** adicionar `opcao_mover "aberto"` ao modal, remover/reordenar tags ou trocar argumentos por outros válidos passa pelas verificações existentes. Elas não comparam as opções de cada seletor com seu conjunto anterior. ([test_bo_rotulo_encerrado.py:43](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:43), [test_bo_rotulo_encerrado.py:62](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:62))

4. **KeyError: nenhum caminho dinâmico encontrado na cópia.** O acesso indexado pode levantar, mas o único chamador produtivo Python é a tag; suas 18 chamadas nos cinco templates são literais cobertos pelo mapa. Ocorrência em produção: `[nao verificado]`. ([verbos.py:146](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/verbos.py:146), [chamados_tags.py:76](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/templatetags/chamados_tags.py:76), chamadas na tabela do item 1)

5. **Bloqueia — selo insuficiente para impedir a reincidência.** Executei o varredor extraído do teste, em memória, substituindo apenas as duas tags do detalhe por: [nao verificado]

   ```html
   <option value='resolvido'>Encerrado</option>
   <option value='fechado'>Encerrado</option>
   ```

   O resultado continuou **cinco arquivos, `ruins=[]`**: `_OPTION` só reconhece aspas duplas e basta restar outra `opcao_mover` no seletor. Os testes de mapa/tag isolada não observam essa alteração; o teste dos templates apenas compila. Execução integral da suíte após mutação: `[nao verificado]`. ([detalhe.html:133](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe.html:133), [test_bo_rotulo_encerrado.py:20](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:20), [teste:49](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:49), [teste:69](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:69), [teste:73](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:73))

   Também escapam `name` com aspas simples, outro nome de controle e arquivos fora de `templates/chamados`. Um include com opções manuais passa mantendo uma tag no pai: o varredor não expande includes e o partial sem `<select>` fica invisível. **Mudar apenas a ordem dos atributos não escapa**: `title="..." value="resolvido"` foi detectado. ([teste:17](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:17), [teste:19](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:19), [teste:20](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:20), [teste:32](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:32))

6. **Classes anteriores:** está presente **selo sem mordida suficiente**, conforme item 5. Não encontrei segundo juiz de transição nem derivação paralela introduzidos: a autoridade registrada segue `motor.decidir`; o novo helper responde à apresentação das opções. Também não há sobrecarga nova demonstrada de código/texto/title, que permanecem separados; a desconexão “Reabrir”/`aberto` fica no achado de fila do item 2. ([core/juizes.py:274](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/core/juizes.py:274), [verbos.py:140](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/verbos.py:140), [chamados_tags.py:77](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/templatetags/chamados_tags.py:77))

## 11/11 12:10 · sessao 9288c5b22025 · - · - · origem=Ronald (tmux)

Backend / sinais / testes, somente leitura: [nao verificado]

- **Servidor aceita batida sem selfie.** A porta web grava antes de olhar foto e só anexa quando há arquivo: `app/ponto/services/triagem_batida.py:700`, `app/ponto/services/triagem_batida.py:748`. As duas APIs leem foto opcional, transformam ausência em motivo de revisão e seguem para `registrar_batida`: `app/api/views.py:436`, `app/api/views.py:449`, `app/api/views.py:666`; `app/api/views_core.py:423`, `app/api/views_core.py:436`, `app/api/views_core.py:658`. O modelo permite foto vazia/nula: `app/ponto/models.py:78`.

- **Chokepoint não exige câmera/foto.** Seus argumentos obrigatórios são colaborador, timestamp, tipo e origem; os demais vêm em `**campos`; cria `Batida` sem exigir foto: `app/ponto/registro_batida.py:94`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`. Ele possui dedup e recusa por colisão tipo-oposto, questões distintas de selfie: `app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:145`. A lei está em `CLAUDE.md:143`.

- **O juiz existente é de cobrança, não de admissibilidade.** `selfie_ausente` decide se foto ausente já é cobrável, com PWA no ato e APK após 90 minutos: `app/ponto/registro_batida.py:64`, `app/ponto/registro_batida.py:71`, `app/ponto/registro_batida.py:80`. Registro formal da pergunta “a foto da batida está ausente?” em `app/core/juizes.py:1192`. As duas APIs e o retro o consultam: `app/api/views.py:461`, `app/api/views_core.py:448`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:74`. A porta web não faz essa consulta; grava e anexa condicionalmente: `app/ponto/services/triagem_batida.py:700`, `app/ponto/services/triagem_batida.py:748`.

- **Sinais não são iguais online web e offline sincronizado.** O catch online de apto apenas escreve aviso e mostra botões, sem acrescentar flag ao formulário; submit só inclui foto se existir: `app/templates/ponto/triagem/apto.html:156`, `app/templates/ponto/triagem/apto.html:228`. O serviço web determina `aprovada`/`justificativa` por fora de horário e `sinal_acesso`, sem considerar câmera ou foto ausente: `app/ponto/services/triagem_batida.py:685`, `app/ponto/services/triagem_batida.py:693`, `app/ponto/services/triagem_batida.py:710`. A API acrescenta `foto_ausente` e contexto `tipo_batida`; `liveness_status=indisponivel` acrescenta `liveness_indisponivel` e `motivo_cliente` vindo de `liveness_detail`: `app/api/views.py:464`, `app/api/views.py:501`; casca core: `app/api/views_core.py:451`, `app/api/views_core.py:488`.

- **Modelo dos sinais:** `Batida` possui `canal`, `foto`, `justificativa` e `aprovada`, sem campo específico de falha da câmera ou `liveness_status`: `app/ponto/models.py:8`, `app/ponto/models.py:55`, `app/ponto/models.py:56`, `app/ponto/models.py:63`, `app/ponto/models.py:78`. `foto_ausente`/`liveness_indisponivel` são motivos de `DisputaSupervisao`, armazenados em JSON de `motivos`/`contexto`: `app/chamados/models.py:1727`, `app/chamados/models.py:1730`, `app/chamados/models.py:1741`, `app/chamados/models.py:1745`. Assim, foto ausente não registra a causa “câmera indisponível”; o retro só conhece origem/canal/presença/tempo: `app/ponto/management/commands/marcar_foto_ausente_retro.py:66`.

- **Escritores/consumidores posteriores:** APIs associam `ultima_batida` e chamam a fábrica de disputa: `app/api/views.py:767`, `app/api/views_core.py:762`. A fábrica agrega/grava motivos e contexto, ou cria a disputa: `app/chamados/services/disputa_emissao.py:535`, `app/chamados/services/disputa_emissao.py:562`, `app/chamados/services/disputa_emissao.py:587`; gera perguntas: `app/chamados/services/disputa_emissao.py:669`. O leitor obtém pergunta pelo motivo e texto pelo contexto: `app/chamados/services/disputa_emissao.py:309`, `app/chamados/services/disputa_emissao.py:344`; definições de perguntas de foto e liveness em `app/chamados/catalogo/perguntas.py:94`, `app/chamados/catalogo/perguntas.py:112`. Retro lê tanto `FotoBatida` quanto `Batida.foto` e escreve cobrança `foto_ausente`: `app/ponto/management/commands/marcar_foto_ausente_retro.py:70`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:95`.

- **Fotos recebidas depois:** porta `anexar_foto` escreve `Batida.foto` e, conforme modo, `FotoBatida`: `app/ponto/registro_batida.py:296`. Upload API exige foto para anexar a batida já existente, não para registrar a batida: `app/api/views_core.py:832`, `app/api/views.py:847`. Upload chama resolução das disputas de foto: `app/api/views_core.py:853`, `app/api/views.py:868`. Esse leitor filtra motivo, confere `ultima_batida.id`, apara motivo ou fecha disputa: `app/api/foto_recebida.py:57`, `app/api/foto_recebida.py:61`, `app/api/foto_recebida.py:67`, `app/api/foto_recebida.py:70`. Anexo web também só exige arquivo para anexar: `app/ponto/views.py:2895`.

- **Telemetria separada:** `api_diag_cam` recebe erro/modo/user-agent e escreve logger, sem associar sinal à batida: `app/api/views.py:2647`, `app/api/views.py:2655`, `app/api/views.py:2666`.

- **Rotas:** fila atual usa `/api/ponto/bater/`, com duas cascas de URLs: `app/api/urls.py:67`, `app/api/urls_core.py:14`. A rota antiga `/ponto/batida-offline/` existe, mas sempre responde 410: `app/ponto/urls.py:13`, `app/ponto/views.py:417`.

- **Testes relevantes, presença inspecionada, execução [nao verificado]:** `test_pwa_sem_foto_cria_batida_e_disputa` cobra HTTP 200, uma Batida e motivo `foto_ausente`; morderia bloqueio de servidor, não bloqueio do botão: `app/api/tests/test_api_bater_pwa.py:64`. A bateria offline também envia payload sem foto e cobra gravação: `app/ponto/tests/test_batida_offline.py:49`, `app/ponto/tests/test_batida_offline.py:68`. Z9 envia “camera bloqueada” e exige 200 + `liveness_indisponivel`: `app/api/tests/test_api_bater_ponto.py:185`. Testes do juiz cobrem PWA imediato, APK antes/depois do prazo, foto presente e origem não aparelho: `app/ponto/tests/test_selfie_ausente_juiz.py:27`. Teste de consumidores exige chamada ao juiz nos dois kernels: `app/ponto/tests/test_selfie_ausente_juiz.py:130`. Retro tem casos para marcação, foto recebida e prazo ainda aberto: `app/ponto/tests/test_bug126_selfie_invisivel.py:65`, `app/ponto/tests/test_bug126_selfie_invisivel.py:74`, `app/ponto/tests/test_bug126_selfie_invisivel.py:84`. O selo L030 cobre guardas GPS retornando 403, não foto/câmera nem JS: `app/api/tests/test_l030_selo_porta.py:3`, `app/api/tests/test_l030_selo_porta.py:18`, `app/api/tests/test_l030_selo_porta.py:83`.

## 11/11 12:10 · sessao b98828b87a03 · - · - · origem=Ronald (tmux)

Censo dos outros fronts, somente leitura.

| Sítio | Caminho | Resposta sem câmera/foto | Como |
|---|---|---|---|
| `app/templates/ponto/bater_ponto.html:246` | normal online/offline, antes de escolher E/S | **Bloqueia implicitamente** enquanto não liberar captura | `bloco-botoes` nasce `display:none`; `abrirCameraPre()` tenta câmera e, no catch, apenas oferece câmera do sistema, sem liberar botões (`:343-378`). Negação de permissão entra nesse mesmo catch (`:362`). |
| `app/templates/ponto/bater_ponto.html:384` | fallback do sistema | **Bloqueia** se cancelar/nenhum arquivo | `if (!file) return` (`:385-386`); libera botões somente após `FileReader.onload`, quando `_fotoBlob=file` (`:392-401`). |
| `app/templates/ponto/bater_ponto.html:407` | captura prévia | **Libera** se `toBlob` der null, lançar ou ficar sem callback | Tenta converter dataURL e libera botões mesmo sem blob (`:423-456`); salvaguarda de 1.500 ms também libera (`:458-467`). Porém `drawImage` e `toDataURL` ocorrem antes do `try` (`:414-417`), sem tratamento nessa função. |
| `app/templates/ponto/bater_ponto.html:470` | refazer | **Volta a bloquear** | Zera `_fotoBlob`, esconde botões e reabre câmera (`:471-477`); se nova câmera falhar, cai no fallback bloqueante descrito acima (`:362-378`). |
| `app/templates/ponto/bater_ponto.html:492` | envio online | **Libera sem foto**, se handler alcançado | Nenhum guard de `_fotoBlob`; anexa somente se existir (`:519-531`). Falha HTTP/rede cai na fila com `_fotoBlob`, inclusive null (`:540-543`). |
| `app/templates/ponto/bater_ponto.html:558` | offline, depois de escolher E/S | **Não bloqueia deliberadamente no catch** | Abre segunda câmera; mostra botão antes da solicitação (`:564-572`); catch apenas muda status para “Camera indisponivel” (`:579-581`). |
| `app/templates/ponto/bater_ponto.html:584` | confirmar offline sem stream | **Não retorna por falta de stream** | Só desenha se vídeo visível; faz `canvas.toBlob` incondicionalmente e enfileira o blob recebido, mesmo null (`:587-597`). Não possui fallback/timeout/try em volta de `toBlob` (`:594-610`); garantir comportamento em falha do canvas exige runtime **[nao verificado]**. Canvas nunca capturado pode produzir imagem vazia: inferência do desenho condicional e `toBlob` incondicional (`:587-597`), conteúdo real **[nao verificado]**. |
| `app/templates/ponto/bater_ponto.html:174` | confirmação de turno aberto | **Libera sem selfie nesse formulário** | POST com tipo/confirmar/latitude/longitude e botão submit, sem campo foto ou guarda de câmera (`:180-196`). |
| `app/templates/ponto/bater_ponto_v2.html:235` | v2 câmera, ambos caminhos | **Oferece fallback; não resolve admissibilidade** | Catch diferencia `NotAllowedError`, informa câmera negada/indisponível e abre input do sistema (`:241-247`); seleção vazia retorna (`:250-252`). |
| `app/templates/ponto/bater_ponto_v2.html:256` | v2 captura vazia/erro/timeout | **Continua até a guarda bloqueante** | Sem vídeo pronto resolve blob existente ou null (`:259`); `toBlob` null tenta dataURL (`:265`); catch e timeout de 1.500 ms tentam dataURL (`:266-267`). |
| `app/templates/ponto/bater_ponto_v2.html:285` | **v2 online e offline** | **Bloqueia explicitamente sem foto** | `garantirRequisitos` consulta permissão de câmera, mostra toast e retorna false quando `_fotoBlob` ausente (`:286-290`). `v2Confirmar` reabilita botão e retorna (`:307-312`) **antes** do teste `navigator.onLine` (`:315`); portanto não alcança POST online (`:322`) nem filas de fallback/offline (`:333`, `:346`). |
| `app/templates/core/onboarding.html:152` | onboarding, sem registro de batida próprio | **Etapa normal exige foto, mas permite configurar depois** | Catch troca para câmera do sistema (`:156-161`); seleção vazia retorna (`:168-169`); `salvarFoto` retorna sem blob (`:212-213`); captura `toBlob` não trata null/timeout (`:184-200`). Botão “Configurar depois” chama concluir e recarrega (`:120-125`). Backend computa faltas selfie/GPS/câmera (`app/core/views.py:479-486`) mas grava onboarding completo e responde ok mesmo com faltas (`:488-489`). |

Não há timeout explícito de **getUserMedia** nessas funções; os 1.500 ms são de conversão de imagem, e os demais timeouts próximos são de relógio/GPS (`app/templates/ponto/bater_ponto.html:343`, `:458`, `:480`; `app/templates/ponto/bater_ponto_v2.html:235`, `:267`, `:271`; `app/templates/core/onboarding.html:152`, `:263`). Comportamento do navegador quando a promessa nunca resolve **[nao verificado]**.

Scripts e includes:

- `bater_ponto.html` e `bater_ponto_v2.html` estendem `base.html` (`app/templates/ponto/bater_ponto.html:1`; `app/templates/ponto/bater_ponto_v2.html:1`). `apto.html` estende `base_triagem.html`, que estende `base.html` (`app/templates/ponto/triagem/apto.html:1`; `app/templates/ponto/triagem/base_triagem.html:1`).
- As três carregam `/static/pwa.js` defer (`app/templates/base.html:20`), `/static/js/hasner-ui.js?v=hx3` (`:83`), `hasner-table.js` (`:84`), `hasner-filter.js?v=ta5` (`:85`), `fita-mes.js?v=1` (`:86`), `geo-colab-map.js` (`:88`) e `popover-acao.js` (`:440`). Externo comum: htmx 1.9.10 do unpkg (`:19`).
- Só v2 acrescenta `/static/leaflet/leaflet.js` sob condição colaborador/sem sucesso (`app/templates/ponto/bater_ponto_v2.html:135-136`).
- Includes comuns da base: `core/_nudge_app_android.html`, condicional (`app/templates/base.html:92`); `includes/icons.html` (`:93`); `core/_icone_pauta.html` (`:290`); `core/_dialogo.html` (`:388`); `chamados/partials/_js_cobranca.html` (`:393`); `core/_drawer_generico.html` (`:399`); `core/_copiloto.html` (`:432`), este também inclui `_icone_pauta` (`app/templates/core/_copiloto.html:34`).
- A base de triagem acrescenta JS inline de relógio/GPS e bloco `extra_js`; nenhum `script src` adicional (`app/templates/ponto/triagem/base_triagem.html:25-76`).
- Onboarding é HTML independente com script inline, sem herança/includes/scripts externos (`app/templates/core/onboarding.html:1`, `:115`, `:330-332`).

Não identifiquei função/constante/endpoint comum consultado pelos três fronts para “selfie é impeditiva?”; a decisão hoje está nos ramos locais contraditórios acima (`app/templates/ponto/bater_ponto.html:362`, `:423`, `:492`; `app/templates/ponto/bater_ponto_v2.html:285`; `app/templates/ponto/triagem/apto.html:161`). Como **inferência de localização**, `app/static/pwa.js`, já carregado na base (`app/templates/base.html:20`) e chamado pelas telas para registrar (`app/templates/ponto/bater_ponto.html:543`; `app/templates/ponto/bater_ponto_v2.html:333`), é o menor módulo existente diretamente relacionado à batida que pode hospedar a resposta comum.

Sinais de câmera no onboarding são do **Colaborador**, não da Batida: front envia `{camera:true}` após salvar foto (`app/templates/core/onboarding.html:226-230`); backend grava `camera_concedida_em` (`app/core/views.py:1085-1090`); campo/modelo e leitura das etapas pendentes (`app/colaboradores/models.py:376`, `:405-413`). No catch de `bater_ponto` há telemetria `/api/diag/cam/` com erro/modo/user_agent (`app/templates/ponto/bater_ponto.html:367-377`); não vi sinal específico por batida sendo enviado nesse trecho (`:375`, `:530-543`).

Guarda existente relevante: `check_a_lei_batida.py` procura regex de guard de foto e frases em apto/bater (`app/scripts/check_a_lei_batida.py:13-23`); **não enumera v2**, não procura o `display:none` do catch e só encontra guard `_fotoBlob`/return na mesma linha (`:14-15`, `:20-21`). Seus checks de view apontam `app/apps/ponto/views.py` (`:24-29`) e arquivos ausentes são silenciosamente pulados (`:34-35`). Assim, esta guarda não morde os bloqueios observados do catch de `bater_ponto` ou da v2 (`app/templates/ponto/bater_ponto.html:362-378`; `app/templates/ponto/bater_ponto_v2.html:285-290`).

## 11/11 12:09 · sessao 56dd18a89d3f · - · - · origem=Ronald (tmux)

Fila e sincronização:

- A fila é IndexedDB `juliani-offline`, versão 2, store `batidas_pendentes`; nenhuma fila de batida em `localStorage` foi encontrada no escopo pesquisado. A implementação localizada está em `app/static/pwa.js:48-60`; outros enfileiradores [nao verificado].
- `registrarBatidaOffline()` aceita foto ausente: grava `foto: fotoBlob || null`, sem validação impeditiva da selfie. Guarda tipo, timestamps/gap, coordenadas/GPS e foto; não guarda flag de erro de câmera. `app/static/pwa.js:107-122`.
- A sincronização envia `foto` somente quando existe; usa POST multipart `/api/ponto/bater/`; acrescenta sempre `liveness_status=indisponivel` e `liveness_detail=PWA web sem liveness`. Isso informa ausência de liveness do PWA, sem distinguir câmera negada/indisponível/foto não capturada. `app/static/pwa.js:143-156`.
- Sucesso remove a entrada; HTTP 400 também remove definitivamente; erro de rede/5xx preserva para retentativa. `app/static/pwa.js:158-179`.
- `window.JulianiPWA` expõe o enfileirador; sincroniza no evento `online` e ao iniciar estando online. `app/static/pwa.js:305-328`.
- O SW ignora POST/PUT/PATCH/DELETE; não enfileira nem decide selfie. No evento `sync-batidas`, avisa janelas abertas para executarem a sincronização; sem janela, notifica para abrir o app. `app/static/sw.js:35-36`, `app/static/sw.js:112-129`.
- O fallback do SW trata resposta GET de rede/cache, sem requisito de câmera ou foto. `app/static/sw.js:29-30`, `app/static/sw.js:45-85`.

Falha específica offline:

- `apto.html` associa o botão a `confirmarFotoOffline`; no catch esconde o botão e põe `_streamOffline=null`, impedindo o clique normal de chegar à fila. `app/templates/ponto/triagem/apto.html:242-256`.
- Dentro de `confirmarFotoOffline`, `_streamOffline=null` não provoca retorno: só condiciona `getTracks().stop()`. A captura depende de `video.style.display`; `canvas.toBlob` continua e repassa o resultado à fila sem validar foto. Se o botão for chamado por outro meio, isso não comprova selfie válida: pode enviar conteúdo do canvas; resultado concreto do navegador [nao verificado]. `app/templates/ponto/triagem/apto.html:261-282`.
- V2 bloqueia antes da escolha online/offline: `garantirRequisitos()` retorna `false` sem `_fotoBlob`; o chamador volta antes tanto do fetch quanto da fila. `app/templates/ponto/bater_ponto_v2.html:285-290`, `app/templates/ponto/bater_ponto_v2.html:307-346`.

Selos/testes, apenas inspecionados; resultados atuais [nao verificado]: [nao verificado]

| Teste/selo | O que prova / limite | [nao verificado]
|---|---|
| `app/api/tests/test_api_bater_pwa.py:64-76` | Sem foto espera HTTP 200, Batida criada e disputa `foto_ausente`; morderia rejeição backend nesse caso. |
| `app/api/tests/test_api_bater_pwa.py:95-105` | Com foto espera 200 e ausência do motivo `foto_ausente`. |
| `app/ponto/tests/test_batida_offline.py:49-76` | Payload sem foto para `/api/ponto/bater/` espera 200 e Batida persistida. |
| `app/ponto/tests/test_batida_offline.py:119-128` | Espera disputa na batida sem foto/liveness. |
| `app/ponto/tests/test_selfie_ausente_juiz.py:27-47` | Cobra ausência por canal/prazo; não exercita captura ou clique. |
| `app/ponto/tests/test_selfie_ausente_juiz.py:130-140` | Confere consulta ao juiz nas duas cópias API. |
| `app/ponto/tests/test_l030_forma_js.py:31-55` | Selo de ausência de barreira GPS: filtra condições com `gpsLat/gpsLng/lat/lng`; no apto mede prefixo somente depois de `toBlob`. Não cobre catch que esconde o botão nem a barreira `_fotoBlob` de v2. |
| `app/ponto/tests/test_l030_templates_renderizados.py:14-37` | Renderiza apto/v2 nas duas cascas e aplica aquele mesmo oráculo GPS. |
| `bin/node_check.sh:11-20`, `bin/node_check.sh:33-37` | Apenas sintaxe dos JS estáticos próprios; inline de templates fica fora. Chamado pela régua em `bin/regua.sh:146-149`. |
| `app/core/tests/test_smoke_chromium.py:151-162`, `app/core/tests/test_smoke_chromium.py:172-189` | Abre `/ponto/espelho/` nas duas cascas e mede overlay/console; não captura câmera nem registra batida. |
| `app/core/tests/test_sw_offline_fallback.py:60-65`, `app/core/tests/test_sw_offline_fallback.py:79-101` | Mede Response/rede/cache do SW; não admite/rejeita selfie. |
| `app/core/tests/test_selo_modal_nao_nasce_vazio.py:135-143` | Guarda saída livre quando modal falha; não cobre câmera. |
| Smoke de clique específico sem câmera nas duas cascas | [nao verificado] |

O comando legado `testar_pwa` contém ensaio sem foto em `/ponto/batida-offline/`, mas não executa captura/click e sua expectativa “>48h rejeitada” está explícita; não tratar como prova do enfileirador atual, que envia para `/api/ponto/bater/`. `app/ponto/management/commands/testar_pwa.py:235-271`, `app/static/pwa.js:153`.

## 11/11 12:08 · sessao 555fd569cc38 · - · - · origem=Ronald (tmux)

Complemento do censo:

- **Resgatar** é controle manual que pode mudar estado: botão em `app/templates/chamados/partials/_lista_chamados.html:85`; quando o encerramento veio da competência trancada, chama `ch.reabrir(humano=True)` em `app/chamados/views_arquivar.py:64`. Não é seletor de destinos.
- **Abrir registro e validar** também muda estado: `app/templates/chamados/detalhe_local.html:233` → `ir_para_ausencia()` → `chamado.analisar()` em `app/chamados/views.py:1513`.
- **Atribuir/Assumir** não muda `status_local` nesse caminho: `app/chamados/views.py:1029` chama `assumir_responsavel`, que grava responsável e instante em `app/chamados/services/vinculos.py:131`.
- **Reabertura administrativa** oferece motivos manuais, por outro nome de select: `app/templates/chamados/partials/modal_fio.html:390` e `:393`; o receptor reabre chamado encerrado em `app/chamados/services/acoes_disputa.py:269`. As opções representam motivos, não destinos de estado.
- **Geofence** tem controles manuais nos dois locais: `app/templates/chamados/detalhe_local.html:285` e `app/templates/chamados/partials/modal_fio.html:506`. Autorizar, autorizar com advertência e recusar convergem para `resolvido` em `app/chamados/services/acoes_chamado.py:222` e `:277`.

Esses controles entram no censo amplo pedido, mas seus rótulos manuais não provam repetição do defeito “dois estados distintos com o mesmo texto”; são ações sobre o fato, com destinos fixados nos receptores acima.

`app/api/views.py:1143` é mapa manual de **exibição** (`status_display`), entregue na resposta GET em `app/api/views.py:1189`. O POST dessa API recebe `chamado_id` e `conteudo` e anexa ocorrência, sem receber estado (`app/api/views.py:982`).

Não achei registro/formulário de `ChamadoColaborador` nos arquivos `*admin*`/`*forms*`; `app/chamados/admin.py` está vazio. Existência de controle no Django admin publicado: `[nao verificado]`.

## 11/11 12:08 · sessao 1e67159c4f30 · - · - · origem=Ronald (tmux)

- **Bloqueia — selo permite reproduzir o defeito.** Em memória, substituí somente as tags de `resolvido` e `fechado` de `app/templates/chamados/detalhe.html:133` e `:134` por `<option value='resolvido'>Encerrado</option>` e `<option value='fechado'>Encerrado</option>`. O scanner real retornou cinco arquivos e `ruins=[]`: `_OPTION` reconhece apenas aspas duplas, e basta restar alguma ocorrência de `opcao_mover` no seletor. Âncoras: `app/chamados/tests/test_bo_rotulo_encerrado.py:19`, `:20`, `:58`, `:65`, `:69`. Executei o scanner em memória; execução da suíte Django completa: **[nao verificado]**. Os testes restantes consultam o mapa ou renderizam duas tags isoladas; o teste dos templates apenas chama `get_template`, sem verificar suas opções renderizadas (`:38`, `:43`, `:49`, `:73`).

- **Bloqueia — censo incompleto e includes não expandidos.** O universo limita-se a `templates/chamados` e a `name="novo_status"` com essa grafia; outro nome, aspas simples no `name` e controles fora desse diretório escapam (`app/chamados/tests/test_bo_rotulo_encerrado.py:17`, `:19`, `:26`). Um seletor contendo uma tag válida e um `{% include %}` com duas opções literais passa: o pai satisfaz a busca textual e o partial sem `<select>` não entra no censo (`:32`, `:65`, `:69`). A ordem dos atributos, isoladamente, **não** é fuga: `<option title="x" value="resolvido">` foi detectado pelo regex de `:20`.

- **Fila — selo não protege conjunto, ordem ou `value`.** Remover, repetir, reordenar tags ou trocar seu argumento por outro estado válido não encontra comparação com o conjunto anterior; o teste valida apenas que as chaves do mapa pertencem ao modelo e que não há opções literais reconhecidas (`app/chamados/tests/test_bo_rotulo_encerrado.py:43`, `:62`). A tag devolve exatamente o argumento como `value` (`app/chamados/templatetags/chamados_tags.py:77`).

- **Classes da pergunta 6:** selo sem mordida está presente nos casos acima. Não encontrei no trecho alterado outro juiz de transição: o registrado continua sendo `decidir` (`app/core/juizes.py:274`; `app/chamados/catalogo/motor.py:125`). O novo mapa responde à apresentação das opções (`app/chamados/services/verbos.py:132`, `:140`); o rótulo da pessoa permanece uma pergunta separada (`:162`). Duplicação de juiz registrado, derivação paralela ou campo com dois significados introduzidos pelo diff: **não demonstrados nesses caminhos**, com essas mesmas âncoras.

## 11/11 12:07 · sessao 80139f191a2c · - · - · origem=Ronald (tmux)

Achados e contratos rastreados, sem escrita de arquivos.

- **FILA — seletor sem efeito no detalhe GLPI, preexistente ao diff.** O formulário de `app/templates/chamados/detalhe.html:122` envia `novo_status` (`:130`–`:135`) para `chamados:followup`; a rota aponta a `adicionar_followup` (`app/chamados/urls.py:12`). Essa view lê apenas `conteudo` (`app/chamados/views.py:292`), chama `glpi.adicionar_followup` (`:304`) e retorna (`:310`), sem ler/aplicar `novo_status`. Portanto, nesse caminho, nenhum dos quatro valores muda `status_local`; efeito observado em produção `[nao verificado]`.

- **FILA — “Reabrir” não corresponde a toda reabertura tecnicamente permitida, preexistente ao diff.** Os detalhes enviam `aberto` sob “Reabrir” (`app/templates/chamados/detalhe_local.html:398`; `app/chamados/services/verbos.py:136`). O motor aceita `em_analise → aberto`, mas a matriz de `resolvido` só admite `fechado` e `em_analise` (`app/chamados/catalogo/motor.py:99`–`:106`), rejeitando `resolvido → aberto` para perfis diferentes de `admin_ti` (`:153`–`:155`). Superusuário vira `admin_ti` (`app/colaboradores/mixins.py:163`–`:164`). A regra não foi introduzida pelo diff fornecido `[nao verificado: classificação histórica na árvore sem git; comparação com anexo confirma value/label anterior iguais]`.

**Quem escreve/lê:**

| Item | Escritor | Leitores / efeito |
|---|---|---|
| `value` | `opcao_mover` escreve exatamente `status_local`, escapado por `format_html` (`app/chamados/templatetags/chamados_tags.py:74`–`:78`); argumentos literais nas cinco telas. | As views recebem códigos, nunca texto visível: local via `acao_resposta_livre`, fio/modal via `processar_post_fio`; GLPI ignora, conforme achado. |
| `novo_status`, detalhe local/painel turno | Formulários em `app/templates/chamados/detalhe_local.html:385` e `app/templates/chamados/partials/painel_chamado_turno.html:68`. | `detalhe_chamado_local` delega (`app/chamados/views.py:547`); serviço lê POST (`app/chamados/services/acoes_chamado.py:287`), exige gestor e pertencimento a `STATUS_CHOICES` (`:313`), chama `transicionar` (`:315`) e captura `ValueError` (`:320`). |
| `novo_status`, fio/modal | Controles em `app/templates/chamados/fio_colaborador.html:265` e `app/templates/chamados/partials/modal_fio.html:665`. | Views delegam ao mesmo serviço (`app/chamados/views.py:669`–`:674`, `:951`–`:954`); leitura em `app/chamados/services/fio.py:502`; ramo exige `chamado_id` **e conteúdo** (`:709`), RBAC (`:714`), código em `STATUS_CHOICES` (`:719`), `transicionar` (`:721`) e captura `ValueError` (`:722`). |
| Estados recebidos | `STATUS_CHOICES`: sete códigos (`app/chamados/models.py:28`–`:36`). | Pertencimento à lista é só filtro inicial; `transicionar` consulta perfil e juiz (`:130`–`:134`). Motor rejeita desconhecidos, terminais absolutos, destinos cancelado/superado, estado igual e transições fora da matriz (`app/chamados/catalogo/motor.py:139`–`:156`). Guardas de conteúdo permanecem (`app/chamados/models.py:136`, `:148`, `:181`). |
| Estado persistido | `transicionar → _aplicar` (`app/chamados/models.py:199`), `_aplicar → _gravar_estado` (`:447`–`:450`). | Atribuição a `self.status_local` em `:228`; persistência em `:296`. Nenhum rótulo participa da decisão. |
| `MOVER_PARA` | Definição única em `app/chamados/services/verbos.py:132`–`:137`. | Leitor produtivo único: `rotulo_de_movimento` (`:146`); testes também leem (`app/chamados/tests/test_bo_rotulo_encerrado.py:39`, `:45`–`:47`). |
| `ESTADO_TITLE` | Definição em `app/chamados/services/verbos.py:100`–`:108`. | `rotulo_de_movimento` (`:146`), `rotulo_de_linha` (`:155`), `rotulo_para_pessoa` (`:166`); testes (`app/chamados/tests/test_bo_rotulo_encerrado.py:47`). |
| `opcao_mover` | Tag registrada em `app/chamados/templatetags/chamados_tags.py:67`–`:78`. | Detalhe `:132`–`:135`, detalhe_local `:395`–`:398`, fio `:268`–`:270`, modal `:668`–`:670`, painel turno `:76`–`:78`, todos sob `app/templates/chamados/`. |

**KeyError:** nenhum chamador produtivo encontrado passa variável dinâmica. O único chamador Python produtivo é `app/chamados/templatetags/chamados_tags.py:76`; os 18 argumentos da tag são literais pertencentes aos mapas, nas linhas citadas acima. O acesso indexado pode lançar para outro código (`app/chamados/services/verbos.py:146`); ocorrência em produção `[nao verificado]`.

**Juiz duplicado / derivação paralela / semântica dupla:** o helper novo só escolhe apresentação (`app/chamados/services/verbos.py:140`–`:146`); não responde à pergunta de transição registrada em `app/core/juizes.py:108`, cuja autoridade continua `motor.decidir` (`:274`), chamada pelo modelo (`app/chamados/models.py:123`–`:125`). Texto, title e código permanecem separados na tag (`app/chamados/templatetags/chamados_tags.py:76`–`:78`); receptores comparam códigos (`app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/fio.py:719`). Não encontrei leitor que compare o texto da opção. O uso de “arquivar” para `fechado` pode ser confundido com o véu de silêncio, cujo contrato preserva estado (`app/chamados/models.py:87`–`:92`), mas a expressão já estava no modal no diff anexo `[nao verificado: origem histórica fora do anexo]`; não é nova sobrecarga de campo pelo diff.

## 11/11 12:07 · sessao 555fd569cc38 · - · - · origem=Ronald (tmux)

Censo global não encontrou seletor de destino restante com opções manuais ou `espera`: os cinco usam `opcao_mover` em `app/templates/chamados/detalhe.html:130`, `detalhe_local.html:393`, `fio_colaborador.html:265`, `partials/modal_fio.html:665` e `partials/painel_chamado_turno.html:74`.

As leituras restantes de `espera` nesses arquivos são pílulas de exibição, fora dos seletores: `app/templates/chamados/detalhe.html:36`, `detalhe_local.html:68`, `fio_colaborador.html:188`, `partials/modal_fio.html:102`, `partials/painel_chamado_turno.html:18`.

Existe seletor manual fora de chamados, mas filtra relatório: `app/templates/relatorios/historico_chamados.html:54`; seu receptor usa `qs.filter(status_local=f_status)`, sem transição (`app/relatorios/views.py:919`).

Há outros controles manuais que alteram estado como consequência de ações de domínio:

- Aprovar/rejeitar benefício: botões em `app/templates/colaboradores/painel_solicitacao_beneficio.html:23` e JS em `app/templates/colaboradores/fila_solicitacoes_beneficio.html:138`; receptor escolhe `resolvido`/`fechado` em `app/colaboradores/views.py:2068`.
- Aceitar/recusar escala: controles em `app/templates/chamados/detalhe_local.html:163` e `:169`; ambos concluem em `resolvido` (`app/chamados/services/acoes_chamado.py:149`).
- Aceitar/recusar agenda: controles em `app/templates/chamados/detalhe_local.html:200` e `:209`; ambos concluem em `resolvido` (`app/chamados/services/acoes_chamado.py:108`).
- Aprovar exceção: `app/templates/chamados/detalhe_local.html:256`, `app/templates/chamados/partials/modal_fio.html:598`; conclusão `resolvido` em `app/chamados/services/acoes_chamado.py:70`.
- Autorizar/recusar geofence: recusa em `app/templates/chamados/partials/modal_fio.html:528`; os serviços concluem em `resolvido` (`app/chamados/services/acoes_chamado.py:222`, `:277`).

Esses controles usam ações e textos próprios, sem oferecer dois destinos com o mesmo rótulo; não constituem repetição atual do defeito do seletor (`app/colaboradores/views.py:2068`; `app/chamados/services/acoes_chamado.py:108`, `:149`).

## 11/11 11:57 · sessao 0adb21eb0cd2 · BO-ROTULO-ENCERRADO · censo · origem=Code

A duplicação está no código: `resolvido` e `fechado` recebem “Encerrado” em `ESPERA_COLAB`. **O modal distingue as opções por literais no template; não consulta uma fonte compartilhada para esses textos.** (`app/chamados/services/verbos.py:118`, `app/chamados/services/verbos.py:119`, `app/templates/chamados/partials/modal_fio.html:669`, `app/templates/chamados/partials/modal_fio.html:670`)

**1. Onde o detalhe monta as opções**

A página local é renderizada por `detalhe_chamado_local`, com contexto de `montar_contexto_detalhe`; a outra página é renderizada por `detalhe_chamado`. Os formulários e as opções estão nos próprios templates. (`app/chamados/views.py:549`, `app/chamados/views.py:550`, `app/chamados/services/acoes_chamado.py:399`, `app/chamados/views.py:262`, `app/templates/chamados/detalhe_local.html:385`, `app/templates/chamados/detalhe.html:122`)

| Opção/value | Detalhe local | Outro detalhe | Fonte do texto |
|---|---|---|---|
| vazio | `app/templates/chamados/detalhe_local.html:394` | `app/templates/chamados/detalhe.html:131` | Literal “— manter atual —” |
| `em_analise` | `app/templates/chamados/detalhe_local.html:395` | `app/templates/chamados/detalhe.html:132` | `"em_analise"\|espera\|first` |
| `resolvido` | `app/templates/chamados/detalhe_local.html:396` | `app/templates/chamados/detalhe.html:133` | `"resolvido"\|espera\|first` |
| `fechado` | `app/templates/chamados/detalhe_local.html:397` | `app/templates/chamados/detalhe.html:134` | `"fechado"\|espera\|first` |
| `aberto` | `app/templates/chamados/detalhe_local.html:398` | `app/templates/chamados/detalhe.html:135` | Literal “Reabrir” |

A cadeia que produz o texto é `espera` → `rotulo_para_pessoa` → `ESPERA_COLAB`. A função devolve `(texto, title)`; `first` escolhe o texto de espera, que coincide para os dois estados. (`app/chamados/templatetags/chamados_tags.py:63`, `app/chamados/templatetags/chamados_tags.py:64`, `app/chamados/services/verbos.py:138`, `app/chamados/services/verbos.py:141`, `app/chamados/services/verbos.py:118`, `app/chamados/services/verbos.py:119`)

Há um terceiro seletor com a mesma cadeia no fio do colaborador: placeholder literal em `app/templates/chamados/fio_colaborador.html:267`; `em_analise`, `resolvido` e `fechado` em `app/templates/chamados/fio_colaborador.html:268`, `app/templates/chamados/fio_colaborador.html:269`, `app/templates/chamados/fio_colaborador.html:270`.

Participação de form Python ou JS na produção desses textos: [não verificado]. A produção demonstrada acima ocorre nas expressões das próprias `<option>`.

**2. Fonte do modal do painel**

A view identifica e renderiza o modal em `app/chamados/views.py:869`, `app/chamados/views.py:870`, `app/chamados/views.py:927`.

| Value | Texto visível | `title` | Fonte |
|---|---|---|---|
| vazio | — mover para — | — | Literal, `app/templates/chamados/partials/modal_fio.html:667` |
| `em_analise` | Assumir a apuração | Em analise | Literal, `app/templates/chamados/partials/modal_fio.html:668` |
| `resolvido` | Encerrar | Resolvido | Literal, `app/templates/chamados/partials/modal_fio.html:669` |
| `fechado` | Encerrar e arquivar | Fechado | Literal, `app/templates/chamados/partials/modal_fio.html:670` |

Portanto, **a fonte atual das opções do modal é o próprio template**. O painel de turno repete os três textos e títulos à mão. (`app/templates/chamados/partials/modal_fio.html:668`, `app/templates/chamados/partials/modal_fio.html:669`, `app/templates/chamados/partials/modal_fio.html:670`, `app/templates/chamados/partials/painel_chamado_turno.html:76`, `app/templates/chamados/partials/painel_chamado_turno.html:77`, `app/templates/chamados/partials/painel_chamado_turno.html:78`)

Existem fontes que distinguem os **nomes dos estados**, mas aquelas opções não as consultam: choices do modelo, “Resolvido”/“Fechado”, e `ESTADO_TITLE`, também “Resolvido”/“Fechado”. (`app/chamados/models.py:31`, `app/chamados/models.py:32`, `app/chamados/services/verbos.py:103`, `app/chamados/services/verbos.py:104`)

**3. Censo dos leitores que apresentam estado ao humano**

| Leitor/saída | Fonte |
|---|---|
| Badge do detalhe | `espera`: texto e tooltip em `app/templates/chamados/detalhe.html:36`, `app/templates/chamados/detalhe.html:38` |
| Badge do detalhe local | `espera`: `app/templates/chamados/detalhe_local.html:68`, `app/templates/chamados/detalhe_local.html:70` |
| Badge do fio do colaborador | `espera`: `app/templates/chamados/fio_colaborador.html:188`, `app/templates/chamados/fio_colaborador.html:190` |
| Badge do painel de turno | `espera`: `app/templates/chamados/partials/painel_chamado_turno.html:18`, `app/templates/chamados/partials/painel_chamado_turno.html:20` |
| Badge do modal | `espera`: `app/templates/chamados/partials/modal_fio.html:102`, `app/templates/chamados/partials/modal_fio.html:104` |
| Seletores dos dois detalhes e do fio | `ESPERA_COLAB`, pela cadeia do item 1 |
| Seletor do modal | Literais: `app/templates/chamados/partials/modal_fio.html:668`, `app/templates/chamados/partials/modal_fio.html:669`, `app/templates/chamados/partials/modal_fio.html:670` |
| Seletor do painel de turno | Literais: `app/templates/chamados/partials/painel_chamado_turno.html:76`, `app/templates/chamados/partials/painel_chamado_turno.html:77`, `app/templates/chamados/partials/painel_chamado_turno.html:78` |
| Lista detalhada do painel | `rotulo_de_linha`; produção em `app/chamados/services/painel.py:384`, `app/chamados/services/painel.py:388`, `app/chamados/services/painel.py:394`; consumo em `app/templates/chamados/partials/_lista_chamados.html:80`, `app/templates/chamados/partials/_lista_chamados.html:81`, `app/templates/chamados/partials/_lista_chamados.html:87` |
| Lista resumida do painel | Mesma função; produção em `app/chamados/services/painel.py:485`, `app/chamados/services/painel.py:487`; consumo em `app/templates/chamados/partials/_lista_resumo.html:68`, `app/templates/chamados/partials/_lista_resumo.html:69`, `app/templates/chamados/partials/_lista_resumo.html:72` |
| Card de atendimento | Choices, via `get_status_local_display`: `app/templates/chamados/partials/_card_atendimento.html:23` |
| Histórico de chamados | Choices, via `get_status_local_display`: `app/templates/relatorios/historico_chamados.html:121` |
| Painel da justificativa | Choices, via `get_status_local_display`: `app/templates/ponto/partials/painel_justificativa.html:79` |
| Pauta de resposta tardia | Choices, via `get_status_local_display().lower()`: `app/chamados/services/resposta_tardia.py:63`, `app/chamados/services/resposta_tardia.py:67` |
| API de atendimento | Dicionário próprio em `app/api/views.py:1143`, `app/api/views.py:1144`, `app/api/views.py:1145`; fallback para código cru em `app/api/views.py:1146` |
| Filtro do relatório histórico | Literais “Aberto”, “Em análise”, “Resolvido”, “Fechado”: `app/templates/relatorios/historico_chamados.html:56`, `app/templates/relatorios/historico_chamados.html:57`, `app/templates/relatorios/historico_chamados.html:58`, `app/templates/relatorios/historico_chamados.html:59` |
| Manual servido ao humano | Literais em `app/templates/core/manual.html:207`, `app/templates/core/manual.html:215`, `app/templates/core/manual.html:216`, `app/templates/core/manual.html:217`, `app/templates/core/manual.html:219`, `app/templates/core/manual.html:222`, `app/templates/core/manual.html:225`, `app/templates/core/manual.html:249`, `app/templates/core/manual.html:250`, `app/templates/core/manual.html:251`, `app/templates/core/manual.html:252`, `app/templates/core/manual.html:254`, `app/templates/core/manual.html:278`, `app/templates/core/manual.html:282`, `app/templates/core/manual.html:283`, `app/templates/core/manual.html:284`, `app/templates/core/manual.html:285`, `app/templates/core/manual.html:286`, `app/templates/core/manual.html:294` |

Nos badges, `esp.0` vem de `ESPERA_COLAB` e `esp.1` de `ESTADO_TITLE`. Na lista administrativa, o texto principal é o verbo ou “Registro”; o estado distinguido fica no tooltip. (`app/chamados/services/verbos.py:131`, `app/chamados/services/verbos.py:133`, `app/chamados/services/verbos.py:135`, `app/chamados/services/verbos.py:141`, `app/chamados/services/verbos.py:142`)

Também há exposições do **código cru**, sem tradução:

| Saída | Referências |
|---|---|
| PDF dos chamados | Lê `ch.status_local` em `app/relatorios/pdf_chamados.py:116`; imprime em `app/relatorios/pdf_chamados.py:122`, `app/relatorios/pdf_chamados.py:125` |
| Tooltip do chamado no calendário | Produtores em `app/colaboradores/services/calendario.py:441`, `app/colaboradores/services/calendario.py:444`; consumidor em `app/templates/colaboradores/partials/_calendario_grade.html:108` |
| API mensageria | Campos crus em `app/api/views_mensageria.py:417`, `app/api/views_mensageria.py:1082` |
| Estado no histórico/API | `app/chamados/services/historico.py:156` → `app/api/views_mensageria.py:2166` |
| Texto das transições no histórico | Descrição com códigos gravada em `app/chamados/models.py:292`, `app/chamados/models.py:293`; lida em `app/chamados/services/historico.py:92`; textualizada em `app/chamados/services/historico.py:208`; entregue em `app/api/views_mensageria.py:2168` |
| Saídas operacionais | `app/chamados/management/commands/silenciar_chamados_isentos.py:41`, `app/ponto/management/commands/reconciliar_vinculo.py:57`, `app/ponto/management/commands/reavaliar_ausencias_feriado.py:42`, `app/ponto/management/commands/reavaliar_ausencias_lancadas.py:43`, `app/ponto/management/commands/reconciliar_geofence.py:36` |
| Erros do smoke | `app/chamados/management/commands/smoke_chamados.py:137`, `app/chamados/management/commands/smoke_chamados.py:150` |

O domínio GLPI tem seu próprio `STATUS_MAP` numérico; `_formatar_ticket` produz `status_label`, exibido na tabela de tickets. (`app/chamados/glpi_client.py:126`, `app/chamados/glpi_client.py:137`, `app/chamados/glpi_client.py:142`, `app/templates/chamados/partials/tabela_chamados.html:40`)

Outros textos manuais associados ao estado:

- “Reabrir”: `app/templates/chamados/detalhe.html:135`, `app/templates/chamados/detalhe_local.html:398`.
- “Este chamado está fechado”: `app/templates/chamados/detalhe.html:148`, `app/templates/chamados/detalhe_local.html:412`.
- Datas “Resolvido em”/“Resolvido”: `app/templates/chamados/detalhe.html:56`, `app/templates/chamados/detalhe_local.html:88`, `app/templates/relatorios/historico_chamados.html:127`.
- Títulos literais “Em analise”: `app/templates/chamados/detalhe.html:101`, `app/templates/chamados/fio_colaborador.html:225`, `app/templates/chamados/partials/_relatorio_geo.html:22`.
- Agrupamentos “Histórico/Cancelados/Encerrados” e “registrados”, sem correspondência individual de estado: `app/templates/chamados/painel_gestao.html:153`, `app/templates/chamados/painel_gestao.html:191`.

Renderização dos consumidores externos da API, inclusive aplicativo nativo: [não verificado].

**4. Campo, escritores, leitores e comparações**

O campo é `ChamadoColaborador.status_local`, ligado aos sete `STATUS_CHOICES`. Os códigos e conjuntos usados pelas decisões ficam no catálogo. (`app/chamados/models.py:28`, `app/chamados/models.py:52`, `app/chamados/catalogo/motor.py:19`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`, `app/chamados/catalogo/motor.py:30`, `app/chamados/catalogo/motor.py:31`, `app/chamados/catalogo/motor.py:32`)

| Escritor/porta | Local |
|---|---|
| Escrita de transição: `_gravar_estado` | Atribuição em `app/chamados/models.py:228`; trilha em `app/chamados/models.py:292`; persistência em `app/chamados/models.py:296` |
| `_aplicar` | Chama o escritor em `app/chamados/models.py:450` |
| `reconciliar` | `app/chamados/models.py:654` |
| `cancelar` | `app/chamados/models.py:788` |
| `analisar` | `app/chamados/models.py:806` |
| `reabrir` | `app/chamados/models.py:1442` |
| `encerrar` | `app/chamados/models.py:1619` |
| `encerrar_por_competencia_trancada` | `app/chamados/models.py:1639` |
| Nascimento por `abrir` | Estado preparado em `app/chamados/models.py:938`; `get_or_create` em `app/chamados/models.py:971` |
| Nascimento por `criar` | Estado resolvido em `app/chamados/models.py:1060`; `create` em `app/chamados/models.py:1095` |
| Simulação em memória | Atribuição temporária em `app/chamados/models.py:356`; restauração no `finally` em `app/chamados/models.py:371` |
| Backfills históricos | `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `app/chamados/migrations/0038_a3_registrado_backfill.py:15`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/migrations/0039_a3_via_backfill.py:33` |

Chamadores humanos de `transicionar`: `app/chamados/services/acoes_chamado.py:70`, `app/chamados/services/acoes_chamado.py:108`, `app/chamados/services/acoes_chamado.py:149`, `app/chamados/services/acoes_chamado.py:222`, `app/chamados/services/acoes_chamado.py:233`, `app/chamados/services/acoes_chamado.py:277`, `app/chamados/services/acoes_chamado.py:315`, `app/chamados/services/fio.py:721`, `app/colaboradores/views.py:2068`. Chamadas do smoke: `app/chamados/management/commands/smoke_chamados.py:134`, `app/chamados/management/commands/smoke_chamados.py:147`, `app/chamados/management/commands/smoke_chamados.py:163`.

Inventário dos demais arquivos com leituras diretas do campo em Python de produção, incluindo filtros ORM, atributos e serialização; as linhas identificam os sítios:

```text
app/api/views_core.py:309
app/api/views.py:320,1044,1142,1146,1152,1972,1982,3127
app/api/views_mensageria.py:417,1082

app/chamados/models.py:109,112,124,165,207,219,355,417,427,673,712,713,714,736,760,768,783,803,822,830,921,927,1219,1220,1221,1337,1342,1348,1358,1373,1374,1375,1398,1404,1440,1614,1630,1632,2683
app/chamados/juizes.py:780,858,2017,2162,2201
app/chamados/reconciliador.py:246,839,936,938,997,1204,1454,1587
app/chamados/rejuncao.py:61,79
app/chamados/signals.py:33,70,86,87,96,184,192,201,285,293,354,358
app/chamados/views.py:976,1477
app/chamados/views_cobrar.py:76
app/chamados/views_arquivar.py:121

app/chamados/services/abertura.py:166
app/chamados/services/acoes_chamado.py:25,90,132,176,232,252
app/chamados/services/acoes_disputa.py:269
app/chamados/services/adesao.py:89,102,143
app/chamados/services/becos.py:22
app/chamados/services/celula_carimbada.py:42,89,146
app/chamados/services/competencia_trancada.py:88,155,183,192,193,270
app/chamados/services/disputa_emissao.py:200,572,755,963,1109,1318,1344,1884,1910,1913
app/chamados/services/feriado_reavaliar.py:17
app/chamados/services/fila.py:126
app/chamados/services/fio.py:389,403,432,770
app/chamados/services/furo_sem_canal.py:83
app/chamados/services/historico.py:156,180
app/chamados/services/juizes_do_dia.py:92
app/chamados/services/limbo_folgas.py:100,220,278,302,314
app/chamados/services/materializacao.py:778,784
app/chamados/services/na_fila.py:17
app/chamados/services/painel.py:155,189,213,214,215,217,218,224,245,303,305,307,316,379,384,465
app/chamados/services/pergunta_no_app.py:151,184
app/chamados/services/resposta_tardia.py:35,57
app/chamados/services/silencio_ausencia.py:60,125
app/chamados/services/silencio_isentos.py:29
app/chamados/services/sla.py:37
app/chamados/services/verbos.py:230
app/chamados/services/vigia_fio.py:335,338,607,608,618
app/chamados/services/vigia_loop.py:34,117,120

app/colaboradores/services/calendario.py:437,441,442,444
app/colaboradores/services/desligamento.py:152,155
app/colaboradores/services/drawer.py:89
app/colaboradores/services/ficha.py:163
app/colaboradores/services/pos_vinculo.py:36
app/colaboradores/services/vinculo.py:385
app/colaboradores/management/commands/corrigir_escala_retroativa.py:141

app/escala/utils.py:1059
app/escala/services/cadastro_realidade.py:494
app/escala/services/regeneracao.py:170
app/escala/management/commands/fechar_avisos_regua_na_lista.py:34

app/ponto/signals.py:303
app/ponto/views.py:284,395,1616
app/ponto/worklist.py:40
app/ponto/services/ausencia.py:709
app/ponto/services/cartorio.py:473,1370,1629,1648
app/ponto/services/cluster_espurio.py:67,255,285,289,294
app/ponto/services/cobertura.py:114,226
app/ponto/services/entrada_sincronizada.py:49
app/ponto/services/esmeril_espelho.py:182
app/ponto/services/faltas_de_hoje.py:64
app/ponto/services/furos_sem_cobranca.py:69,70,129
app/ponto/services/furos_vetados.py:95
app/ponto/services/geofence.py:82,355
app/ponto/services/justificativa.py:43,61,77,118,121
app/ponto/services/regularizacao.py:133
app/ponto/services/triagem_batida.py:105,290,328
app/ponto/services/veredito_celula.py:149,155
app/ponto/services/vigia_de_hora.py:111

app/relatorios/views.py:745,920
app/relatorios/pdf_chamados.py:116
```

Leitores em comandos e scripts operacionais:

```text
app/chamados/management/commands/arquivar_prescritos.py:49
app/chamados/management/commands/auditar_chamados_orfaos.py:36
app/chamados/management/commands/auditar_invariantes_chamados.py:573,589,602,616,637,653,683,807,978,1015,1041,1068,1070,1072,1339,1576,1577,1607,1628
app/chamados/management/commands/carimbar_por_batida_coincidente.py:61,68
app/chamados/management/commands/disparar_perguntas_competencia.py:99
app/chamados/management/commands/geofence_registrados_para_analise.py:22,36
app/chamados/management/commands/lavrar_perguntas_stale.py:57
app/chamados/management/commands/reabrir_orfas_enterradas.py:98,134,170,171,173,174
app/chamados/management/commands/reconciliar_chamados.py:59
app/chamados/management/commands/retratar_chamados_lei_cala.py:34
app/chamados/management/commands/silenciar_chamados_isentos.py:41
app/chamados/management/commands/smoke_chamados.py:136,137,149,150

app/ponto/management/commands/detectar_ausencias.py:152,193,258,300,348,373,404,514,672,739,743,798
app/ponto/management/commands/detectar_intervalo_ausente.py:125
app/ponto/management/commands/emitir_furo_retroativo.py:95
app/ponto/management/commands/emitir_turno_aberto_retroativo.py:119,123
app/ponto/management/commands/escalonar_chamados_supervisao.py:45
app/ponto/management/commands/fechar_chamados_ausencia.py:28
app/ponto/management/commands/fechar_chamados_meio_periodo.py:56
app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:42
app/ponto/management/commands/gerar_celulas.py:245
app/ponto/management/commands/processar_alertas_avancados.py:103,108,155
app/ponto/management/commands/processar_alertas_turno.py:165,179,381
app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:31
app/ponto/management/commands/reavaliar_ausencias_feriado.py:42
app/ponto/management/commands/reavaliar_ausencias_lancadas.py:30,43
app/ponto/management/commands/reconciliar_fantasmas.py:159
app/ponto/management/commands/reconciliar_geofence.py:29,36
app/ponto/management/commands/reconciliar_grade.py:96,207,529
app/ponto/management/commands/reconciliar_vinculo.py:33,57
app/ponto/management/commands/supra_juiz.py:189,212

bin/censo_folha.py:30
bin/r5_idempotencia_frota.py:61
bin/snapshot_o118_col221.py:22
```

Leitores nos templates, incluindo condicionais de cor e existência de ação:

```text
app/templates/chamados/detalhe.html:36,37,39,41,117
app/templates/chamados/detalhe_local.html:68,69,71,73,153,176,244,279,343,375
app/templates/chamados/fio_colaborador.html:167,188,189,191,193,254
app/templates/chamados/partials/_card_atendimento.html:13,19,20,21,23,203
app/templates/chamados/partials/_lista_chamados.html:33,37,38,40,41,79
app/templates/chamados/partials/modal_fio.html:76,102,103,105,107,500,544,559,588,636,651
app/templates/chamados/partials/painel_chamado_turno.html:18,19,21,23,65
app/templates/ponto/partials/painel_justificativa.html:79
app/templates/relatorios/historico_chamados.html:121
```

Leitores históricos adicionais nas migrations: filtros em `app/chamados/migrations/0023_rebaixar_urgencia_resolvidos.py:5`, `app/chamados/migrations/0023_rebaixar_urgencia_resolvidos.py:14`, `app/chamados/migrations/0024_rebaixar_urgencia_resolvidos_regressao.py:6`, `app/chamados/migrations/0024_rebaixar_urgencia_resolvidos_regressao.py:1

[truncado em 20000 de 24377 caracteres]

_3832 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
