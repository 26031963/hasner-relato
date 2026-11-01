# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 21:33** · publicado em **31/10/2026 21:35** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 21:33 · sessao a047dbbf9d5a · O261-E-F1C4-LEN-VIVOS · censo · origem=Code

Há **quatro sítios de chamada, em três arquivos produtivos**: dois no cartório e um em cada comando. As chamadas de relatórios/inteligência usam outro `classificar_dia`. (`app/ponto/services/cartorio.py:1028`, `app/ponto/services/cartorio.py:1212`, `app/ponto/management/commands/supra_juiz.py:206`, `app/ponto/management/commands/emitir_furo_retroativo.py:99`, `app/relatorios/furos.py:55`, `app/inteligencia/calculadores.py:26`)

O censo abaixo descreve a cópia local. Ocorrências, quantidades e efeitos efetivamente observados em produção nesta obra: **[nao verificado]**.

**1) Onde o classificador lê os parâmetros**

O contrato distingue explicitamente `chamados_vivos`/`chamados_total`, relativos ao **chamado**, de `cobrancas_vivas`, relativo ao **fio vivo**. Se `isento_ponto=True`, a função retorna lista vazia antes desses ramos. (`app/ponto/supra_juiz.py:204`, `app/ponto/supra_juiz.py:213`, `app/ponto/supra_juiz.py:220`)

| Parâmetro lido | Código acrescentado | Condições |
|---|---|---|
| `chamados_vivos` | `COBRANCA_SEM_FURO` | `tipo_dia == 'trabalho'`, previsto não zero, realizado ≥ previsto, `chamados_vivos` verdadeiro e `todas_casadas`. Esta última condição exige células não vazias e contagem de casadas igual ao tamanho da lista; prefere `n_casadas` lavrado e, sem ele, conta células com hora. (`app/ponto/supra_juiz.py:439`) |
| `chamados_vivos` | `COBRANCA_EM_DIA_SEM_TRABALHO` | Tipo fora de `SEM_JURISDICAO`, previsto zero, `chamados_vivos` verdadeiro, `cobertura_status is None` e resposta positiva de `_dia_sem_trabalho_pelo_juiz`. (`app/ponto/supra_juiz.py:470`) |
| `chamados_vivos` | `COBRANCA_EM_DIA_COBERTO` | Cobertura com status exatamente `'aprovada'` e `chamados_vivos` verdadeiro; esse ramo não exige cobertura integral. (`app/ponto/supra_juiz.py:255`, `app/ponto/supra_juiz.py:264`, `app/ponto/supra_juiz.py:501`) |

`SEM_JURISDICAO` contém `fora_vinculo`, `sem_celula` e `indefinido`. O juiz de dia sem trabalho responde `trabalha is False` **ou** `ata.tipo_dia` preenchido e diferente de `'trabalho'`; o classificador recebe esses insumos pelo dicionário, sem fallback quando a chave falta. (`app/ponto/supra_juiz.py:48`, `app/ponto/supra_juiz.py:65`, `app/chamados/juizes.py:398`)

Esses três ramos não exigem `turno_encerrado`; essa guarda pertence aos ramos de furo/fato indicados no código. (`app/ponto/supra_juiz.py:278`, `app/ponto/supra_juiz.py:443`, `app/ponto/supra_juiz.py:470`, `app/ponto/supra_juiz.py:501`)

`chamados_total` é lido **uma vez**, no `elif` que escolhe `FURO_COM_COBRANCA_MORTA`; se falso, o `else` escolhe `FURO_SEM_COBRANCA`. Para alcançar essa escolha, precisam valer todas estas condições: dia de cobrança — trabalho ou ausência parcial —, previsto não zero, realizado zero, nenhuma célula casada, nenhuma batida no dia, nenhuma `cobrancas_vivas`, ausência sem cobrir integralmente o dia e turno encerrado. (`app/ponto/supra_juiz.py:304`, `app/ponto/supra_juiz.py:352`)

Antes de consultar `chamados_total`, o mesmo bloco dá precedência a:

- pré-adoção → `FURO_PRE_ADOCAO`;
- sem vínculo → `PREVISTO_COM_DIA_INDEFINIDO`;
- sem âncora e sem fase → `FASE_NAO_DECLARADA_COM_PREVISTO`;
- régua degenerada → `FURO_COM_REGUA_DEGENERADA`;
- sem âncora → `PREVISTO_COM_DIA_INDEFINIDO`.  
  (`app/ponto/supra_juiz.py:326`, `app/ponto/supra_juiz.py:329`, `app/ponto/supra_juiz.py:331`, `app/ponto/supra_juiz.py:340`, `app/ponto/supra_juiz.py:348`)

Se `dia['justificado']` for verdadeiro, o bloco remove os códigos recém-acrescentados cujo nome começa por `FURO_`. Portanto, os dois resultados dependentes de `chamados_total` podem ser removidos antes do retorno. (`app/ponto/supra_juiz.py:363`)

**2–3) Todos os chamadores, seus universos e o consumo dos códigos**

Nos universos abaixo, `VIVOS` significa `aberto`, `em_analise` e **`registrado`**. Não equivale a `COBRANCA`, que contém somente `aberto` e `em_analise`. `FURO_MODULES` contém `batida_ausente`, `turno_aberto`, `intervalo_pendente` e `entrada_bloqueada_turno_aberto`. (`app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`, `app/chamados/catalogo/furo_modulos.py:15`)

**Cartório, ramo normal — chamada em `app/ponto/services/cartorio.py:1212`.**

- `lista`: chamados do colaborador em `FURO_MODULES`, **todos os status**, agrupados pelo valor de `data_do_chamado(modulo, contexto)` convertido em string; seleciona a chave do dia da célula. O agrupamento não usa `ch.celula.data`. (`app/ponto/services/cartorio.py:81`, `app/ponto/services/cartorio.py:96`, `app/ponto/services/cartorio.py:104`, `app/ponto/services/cartorio.py:1107`)
- `vivos`: elementos de `lista` com status em `VIVOS`. `_com_fio`: esses vivos para os quais `fio_vivo(x.perguntas_que_resolvem.all())` responde verdadeiro. (`app/ponto/services/cartorio.py:1136`, `app/ponto/services/cartorio.py:1141`)
- `_protestam`: vivos para os quais **`cel.trabalha is not False` e `marcos_acesos(x, ata_deste_julgamento) is not True`**. Portanto, `trabalha=None` permanece nesse universo; resultado desconhecido de marcos também permanece. (`app/ponto/services/cartorio.py:1184`, `app/ponto/services/cartorio.py:1206`)
- Passa **`chamados_vivos=len(_protestam)`**, **`chamados_total=len(lista)`** e **`cobrancas_vivas=len(_com_fio)`**. O filtro `_protesta` não é aplicado a `_com_fio` nessa passagem. (`app/ponto/services/cartorio.py:1212`)
- Julga as células recebidas; pode pular impressão já julgada e pode recortar o dia pelos marcos vencidos quando opera com teto. (`app/ponto/services/cartorio.py:1105`, `app/ponto/services/cartorio.py:1115`, `app/ponto/services/cartorio.py:1163`, `app/ponto/services/cartorio.py:1177`)

O destino dos resultados é:

| Resultado | O que o cartório faz |
|---|---|
| **Todos os códigos** | Guarda em `out['cods']`, conta dia com protesto e, com `apply_`, entrega códigos e ata à lavra da célula. (`app/ponto/services/cartorio.py:1224`, `app/ponto/services/cartorio.py:1448`, `app/ponto/services/cartorio.py:1565`) |
| `COBRANCA_SEM_FURO`, `COBRANCA_EM_DIA_SEM_TRABALHO` | Além da lavra, com `apply_`, tenta `reconciliar()` nos vivos; contabiliza fechamentos e pede rejulgamento. Aqui chama **`reconciliar` diretamente**, não `retratar`. (`app/ponto/services/cartorio.py:1313`) |
| `COBRANCA_EM_DIA_COBERTO` | Entra na lavra/derivação de veredito; não tem o executor específico dos dois códigos anteriores. (`app/ponto/services/cartorio.py:1313`, `app/ponto/services/cartorio.py:1565`, `app/escala/models.py:1582`) |
| `FURO_SEM_COBRANCA`, `FURO_COM_COBRANCA_MORTA` | Podem acionar emissão, com guardas de chamado vivo, horizonte/toque, desligamento, afastamento, vínculo, dia de trabalho conhecido e horário inicial. O código de cobrança morta também procura nomear os marcos apagados. (`app/ponto/services/cartorio.py:1475`, `app/ponto/services/cartorio.py:1483`, `app/ponto/services/cartorio.py:1488`, `app/ponto/services/cartorio.py:1495`, `app/ponto/services/cartorio.py:1500`) |
| `FURO_PARCIAL` | Conta emissão potencial; com `apply_`, abre chamado se não há vivos ou atualiza os marcos do `batida_ausente` existente, respeitando as guardas mostradas nesse bloco. (`app/ponto/services/cartorio.py:1537`) |
| `PREVISTO_COM_DIA_INDEFINIDO` | Também pode participar da emissão no caso específico `_limbo_de_hoje`. (`app/ponto/services/cartorio.py:1492`) |
| Demais códigos | Seguem para contagem/lavra e para o mapa de veredito; não têm outros executores por código nesse corpo. (`app/ponto/services/cartorio.py:1448`, `app/ponto/services/cartorio.py:1488`, `app/ponto/services/cartorio.py:1537`, `app/ponto/services/cartorio.py:1565`) |

Há ainda reconciliação **independente dos códigos** para chamados que não protestam e para marcos inexistentes. Quando necessário, o fluxo adia o fechamento e executa **lavra → E2 → reconciliar → rejulgamento**. (`app/ponto/services/cartorio.py:1281`, `app/ponto/services/cartorio.py:1298`, `app/ponto/services/cartorio.py:780`)

Sem códigos, pode lavrar `cobrado` se houver `_com_fio` que também satisfaça `_protesta`; caso contrário, o mapa vazio produz `concorde`. (`app/ponto/services/cartorio.py:1227`, `app/ponto/services/cartorio.py:1250`, `app/escala/models.py:1594`)

**Cartório, ramo `nunca_bateu` — chamada em `app/ponto/services/cartorio.py:1028`.**

Esse ramo é alcançado quando não existe primeira batida não retratada. Passa **zero literal** para `chamados_vivos`, `chamados_total` e `cobrancas_vivas`; não realiza um censo de chamados para esses argumentos. A chamada ocorre dentro de `if apply_`. (`app/ponto/services/cartorio.py:957`, `app/ponto/services/cartorio.py:965`, `app/ponto/services/cartorio.py:1009`, `app/ponto/services/cartorio.py:1029`)

Deriva o veredito dos códigos retornados; sem cobertura integral, substitui-o por `nunca_bateu`. Lava o resultado com **`codigos=[]`**, mesmo que a classificação tenha retornado códigos, e retorna antes do fluxo normal de emissão. Os três códigos que exigem `chamados_vivos` não podem nascer nessa chamada. (`app/ponto/services/cartorio.py:1033`, `app/ponto/services/cartorio.py:1043`, `app/ponto/services/cartorio.py:1044`, `app/ponto/services/cartorio.py:1070`, `app/ponto/supra_juiz.py:443`, `app/ponto/supra_juiz.py:470`, `app/ponto/supra_juiz.py:501`)

**Comando `supra_juiz` — chamada em `app/ponto/management/commands/supra_juiz.py:206`.**

Seleciona colaboradores ativos da empresa, pula quem não tem batida na janela carregada e julga dias de `grade_da_celula` dentro do intervalo solicitado, corrente ou anterior. (`app/ponto/management/commands/supra_juiz.py:74`, `app/ponto/management/commands/supra_juiz.py:97`, `app/ponto/management/commands/supra_juiz.py:116`, `app/ponto/management/commands/supra_juiz.py:188`)

Busca chamados em `FURO_MODULES`, todos os status, e agrupa pelo catálogo `data_do_chamado`. Passa **`len(vivos)`**, **`len(lista)`** e **`len(_com_fio)`**; não aplica ao contador de vivos o filtro de `trabalha` ou de marcos acesos do cartório. (`app/ponto/management/commands/supra_juiz.py:169`, `app/ponto/management/commands/supra_juiz.py:182`, `app/ponto/management/commands/supra_juiz.py:194`, `app/ponto/management/commands/supra_juiz.py:206`)

- **Todos os códigos** alimentam placar, contagem por colaborador e exemplos. (`app/ponto/management/commands/supra_juiz.py:256`)
- **`COBRANCA_SEM_FURO` e `COBRANCA_EM_DIA_SEM_TRABALHO`** acionam o executor: percorre todos os `vivos`, preserva conversa respondida e cobrança órfã de regeneração; com `--executar` e trava desligada, chama `retratar`. A trava está em `False`. A chamada ao retratador não garante fechamento, pois a porta ainda pode recusar. (`app/ponto/management/commands/supra_juiz.py:41`, `app/ponto/management/commands/supra_juiz.py:218`, `app/ponto/supra_juiz.py:509`, `app/chamados/reconciliador.py:520`)
- **`COBRANCA_EM_DIA_COBERTO`** apenas entra nas contagens desse comando. (`app/ponto/management/commands/supra_juiz.py:218`, `app/ponto/management/commands/supra_juiz.py:256`)
- `--alarme` examina somente `FURO_SEM_COBRANCA` e `COBRANCA_SEM_FURO`, podendo sair com código 2. (`app/ponto/management/commands/supra_juiz.py:268`)

**Comando `emitir_furo_retroativo` — chamada em `app/ponto/management/commands/emitir_furo_retroativo.py:99`.**

Seleciona colaboradores ativos da empresa, exige alguma batida na janela carregada e lê os dias pela célula. Busca chamados em `FURO_MODULES`, todos os status, mas agrupa por **`dia_do_fato(ch, fortes=True)`**: célula primeiro, depois data declarada no catálogo/fallback `data_turno`. (`app/ponto/management/commands/emitir_furo_retroativo.py:51`, `app/ponto/management/commands/emitir_furo_retroativo.py:59`, `app/ponto/management/commands/emitir_furo_retroativo.py:77`, `app/chamados/juizes.py:88`, `app/chamados/juizes.py:104`)

Passa **`len(vivos)`**, **`len(lista)`** e **`len(_com_fio)`**, sem filtro de marcos acesos ou `trabalha` na lista de vivos. (`app/ponto/management/commands/emitir_furo_retroativo.py:92`)

**Consome exclusivamente `FURO_SEM_COBRANCA`. Portanto, ignora todos os três códigos dependentes de `chamados_vivos`.** Também ignora `FURO_COM_COBRANCA_MORTA`, embora `chamados_total` possa fazer o juiz escolhê-lo no lugar do código consumido. (`app/ponto/management/commands/emitir_furo_retroativo.py:109`, `app/ponto/supra_juiz.py:352`)

Para emitir, ainda exige vínculo, `eh_dia_trabalho is True`, horário inicial e `lista` vazia; com `--apply`, chama o emissor canônico `_abrir_chamado`. Não lavra diretamente veredito da célula nessa chamada. (`app/ponto/management/commands/emitir_furo_retroativo.py:118`, `app/ponto/management/commands/emitir_furo_retroativo.py:127`, `app/ponto/management/commands/emitir_furo_retroativo.py:130`, `app/ponto/management/commands/emitir_furo_retroativo.py:136`, `app/ponto/management/commands/emitir_furo_retroativo.py:142`)

**4) Escritores, leitores e textos normativos**

O mapa converte os códigos de furo em `furo`; os códigos `COBRANCA_SEM_FURO`/`COBRANCA_EM_DIA_SEM_TRABALHO` em `cobranca_indevida`; e `COBRANCA_EM_DIA_COBERTO` em `coberta`. Havendo vários códigos, vence o primeiro grupo correspondente no mapa. (`app/escala/models.py:1579`, `app/escala/models.py:1597`)

| Estado/campo | Quem escreve | Quem lê no circuito dos efeitos |
|---|---|---|
| `ChamadoColaborador.status_local` | Nascimento por `abrir`; transição por `_gravar_estado`, chamada por `reconciliar`. `RECONCILIAR` leva os estados vivos a `resolvido`, sujeito aos gates. (`app/chamados/models.py:1046`, `app/chamados/models.py:222`, `app/chamados/models.py:605`, `app/chamados/catalogo/motor.py:109`) | Os contadores dos três chamadores; signals de encerramento/rejulgamento; painel/fila; fábrica de disputa. (`app/ponto/services/cartorio.py:1136`, `app/ponto/management/commands/supra_juiz.py:195`, `app/ponto/management/commands/emitir_furo_retroativo.py:93`, `app/chamados/signals.py:68`, `app/chamados/signals.py:155`, `app/chamados/services/painel.py:153`, `app/chamados/services/painel.py:186`, `app/chamados/services/disputa_emissao.py:690`) |
| `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata` | `CelulaDia.lavrar_veredito` delega à porta `ponto.portas.celula.lavrar_veredito`; esta atribui os campos. O cartório fornece códigos/ata; a decisão humana usa a mesma porta. (`app/escala/models.py:1787`, `app/ponto/portas/celula.py:709`, `app/ponto/portas/celula.py:768`, `app/ponto/services/cartorio.py:1565`, `app/ponto/services/veredito_celula.py:330`) | Leitor da célula, juiz de pergunta viva, fábrica, reconciliador e retenção da folha. (`app/escala/services/leitor_celula.py:345`, `app/chamados/juizes.py:1073`, `app/chamados/services/fabrica_da_celula.py:78`, `app/chamados/reconciliador.py:949`, `app/folha/export.py:1215`) |
| `ata.lampadas` / `acesa` | Produzidas do casamento das células por `lampadas_do_dia`, agregadas por `ata_do_dia`; dispensa por ausência/justificativa também pode acender lâmpadas. Persistem pela lavra. **Os códigos não atribuem `acesa` diretamente.** (`app/ponto/services/cartorio.py:429`, `app/ponto/services/cartorio.py:541`, `app/ponto/services/cartorio.py:560`, `app/ponto/services/cartorio.py:611`, `app/ponto/services/cartorio.py:1565`) | `_protesta` via `marcos_acesos`, juiz de pergunta viva, `_celula_furada` e leitor da célula. (`app/ponto/services/cartorio.py:1206`, `app/ponto/services/lastro.py:153`, `app/chamados/juizes.py:1073`, `app/chamados/juizes.py:442`, `app/escala/services/leitor_celula.py:63`) |
| `status_dia` / `status_rotulo` | A mesma porta de lavra calcula e aplica o status com o veredito dessa lavra. (`app/ponto/portas/celula.py:686`, `app/ponto/portas/celula.py:701`, `app/ponto/portas/celula.py:773`) | A autoridade de leitura do status lavrado é `status_do_dia_lavrado`, registrada na tabela de juízes. (`app/docs/JUIZES.md:81`) |
| Estado das perguntas alcançadas pelo fechamento | `retratar` chama `reconciliar`, resolve perguntas, pode rejeitar justificativa pendente vinculada e anexa ocorrência; no fluxo adiado do cartório, E2 carimba as perguntas absolvidas após a lavra. (`app/chamados/reconciliador.py:505`, `app/chamados/reconciliador.py:531`, `app/chamados/reconciliador.py:538`, `app/ponto/services/cartorio.py:787`) | `pergunta_viva` e `fio_vivo`; este último realimenta `cobrancas_vivas` nos três chamadores. (`app/chamados/juizes.py:1073`, `app/chamados/juizes.py:1133`, `app/ponto/services/cartorio.py:1141`, `app/ponto/management/commands/supra_juiz.py:198`, `app/ponto/management/commands/emitir_furo_retroativo.py:96`) |

A porta preserva veredito humano diante da lavra do cartório, podendo atualizar ata/status; a lavra humana conserva os protestos existentes. `furo` e `cobranca_indevida` estão em `VEREDITOS_ACUSAM`; `coberta` não está. A folha lê esse conjunto e os protestos para decidir retenção. (`app/ponto/portas/celula.py:737`, `app/ponto/portas/celula.py:751`, `app/escala/models.py:1735`, `app/folha/export.py:1215`)

A tabela acima cobre o circuito rastreado desses efeitos; exaustividade de **todo** leitor genérico desses campos em telas, APIs, relatórios e vigias: **[nao verificado]**.

Há textos normativos relevantes, com alcances distintos:

- **Lei geral:** “O CONSUMIDOR LE A LAMPADA, nunca re-julga: chamado, pergunta, tela e folha leem a celula/ata”. (`app/docs/LEIS.md:95`)
- **Corte que nomeia os três alimentadores:** “todo leitor de chamados_vivos (disputa_emissao.py:670, supra_juiz.py:163-168, core/juizes.py _C1, cartorio.py:780, commands/supra_juiz.py:182, emitir_furo_retroativo.py:79)” e “os leitores leem a lampada”. A numeração interna dessa citação é histórica. (`app/docs/CORTES.md:255`)
- **Corte sobre marcos acesos no julgamento:** “com todos os marcos acesos NESTE julgamento, o chamado nao conta como cobranca viva e o veredito nao le o proprio chamado vivo (4a).” (`app/docs/CORTES.md:405`)
- **Extensão:** “o veredito nunca le o proprio chamado vivo vale para TODA lampada, inclusive 'cobrado' em dia sem trabalho”. (`app/docs/CORTES.md:412`)
- **Separação das perguntas:** “`VIVOS` responde so a primeira metade”, distinguindo chamado vivo de fio; o registro dá autoridade própria a “alguma pergunta desta colecao esta viva?”. (`app/core/juizes.py:103`, `app/core/juizes.py:125`)
- **Limite explícito de `_protesta`:** “NAO MIGRADO”; lê “so a 1a clausula (`trabalha is not False`, alem dos marcos acesos”; “com trabalha=False zera chamados_vivos antes de classificar_dia”; a segunda cláusula, `ata.tipo_dia`, “seria predicado novo”. (`app/core/juizes.py:145`, `app/core/juizes.py:146`, `app/core/juizes.py:147`, `app/core/juizes.py:148`)
- **Vocabulário/dia:** “VIVOS/COBRANCA/TERMINAIS/NAO_FECHADOS, nunca tupla literal” e dia pelo catálogo, “nunca .get('data_turno') cru”. (`app/docs/JUIZES.md:52`, `app/docs/JUIZES.md:53`)

Assim, existe corte abrangente nomeando os três alimentadores e especificação explícita do recorte atual de `_protesta`. Uma especificação documental individual completa de todos os filtros para cada comando: **[nao verificado]**. (`app/docs/CORTES.md:255`, `app/core/juizes.py:145`)

**5) Existe função única compartilhada para esse universo?**

**Não há uma seleção compartilhada que produza o universo completo passado como `chamados_vivos` pelos três arquivos.** Os comandos montam `vivos` próprios; o cartório acrescenta seu `_protesta` local. (`app/ponto/management/commands/supra_juiz.py:173`, `app/ponto/management/commands/supra_juiz.py:195`, `app/ponto/management/commands/emitir_furo_retroativo.py:80`, `app/ponto/management/commands/emitir_furo_retroativo.py:93`, `app/ponto/services/cartorio.py:1206`)

Existem autoridades compartilhadas de alcance parcial:

| Função | Resposta efetiva e reutilização |
|---|---|
| `fio_vivo(perguntas)` | Responde se há pergunta viva na coleção recebida. O contrato diz literalmente **“O UNIVERSO e do chamador”**; é usada nos três alimentadores, mas não seleciona chamados do dia. (`app/chamados/juizes.py:1133`, `app/ponto/services/cartorio.py:1141`, `app/ponto/management/commands/supra_juiz.py:198`, `app/ponto/manageme

[truncado em 20000 de 21058 caracteres]

## 31/10 21:32 · sessao 3c83aaaa49cc · - · - · origem=Ronald (tmux)

Censo dos chamadores: quatro sítios produtivos em três arquivos — `app/ponto/services/cartorio.py:1028`, `app/ponto/services/cartorio.py:1212`, `app/ponto/management/commands/supra_juiz.py:206` e `app/ponto/management/commands/emitir_furo_retroativo.py:99`. As chamadas de `app/inteligencia/calculadores.py:60` e `app/relatorios/furos.py:45,222,244` pertencem ao homônimo definido em `app/relatorios/furos.py:55`; a importação de inteligência confirma isso em `app/inteligencia/calculadores.py:26`.

**Comando `supra_juiz`:**

- Universo de colaboradores: empresa pedida, `situacao='ativo'`; pula o colaborador se nenhuma batida no intervalo de timestamps entre início−1 dia e fim+3 dias. Janela de dias pode ser explícita, anterior ou atual. (`app/ponto/management/commands/supra_juiz.py:74-105`)
- Lê os dias da célula por `grade_da_celula`, restringindo à janela. (`app/ponto/management/commands/supra_juiz.py:116`, `:188-191`)
- Busca chamados do colaborador em `FURO_MODULES`, **sem filtro de status nessa busca**, agrupa pelo retorno truthy de `data_do_chamado(modulo, contexto)` convertido em string. Não pergunta pela célula do chamado nesse agrupamento. (`app/ponto/management/commands/supra_juiz.py:169-185`)
- `lista = chs[iso]`; `vivos = status_local in VIVOS`; `_com_fio = vivos` com `fio_vivo(perguntas_que_resolvem)`. Passa `chamados_vivos=len(vivos)`, `chamados_total=len(lista)` e `cobrancas_vivas=len(_com_fio)`. Não restringe essas listas por `celula.trabalha` nem por marcos acesos. (`app/ponto/management/commands/supra_juiz.py:194-208`)
- Todo código retornado incrementa placar, placar por colaborador e exemplos; todos podem aparecer no relatório. (`app/ponto/management/commands/supra_juiz.py:256-267`, `:275-283`)
- Só `COBRANCA_SEM_FURO` e `COBRANCA_EM_DIA_SEM_TRABALHO` entram no executor. Para esses códigos percorre **todos os `vivos`**, preserva conversa respondida por `pode_executar_csf` e cobrança órfã de regeneração; com `--executar` e trava desligada chama `chamados.reconciliador.retratar`, usando `motivo_retratacao`. A trava é literalmente `False`. (`app/ponto/management/commands/supra_juiz.py:41`, `:218-255`)
- `COBRANCA_EM_DIA_COBERTO` só entra nas contagens; não tem executor próprio. (`app/ponto/management/commands/supra_juiz.py:218`, `:256-259`)
- `--alarme` considera exclusivamente `FURO_SEM_COBRANCA` e `COBRANCA_SEM_FURO`, podendo sair com código 2. (`app/ponto/management/commands/supra_juiz.py:268-274`)
- O gate `pode_executar_csf` recusa qualquer pergunta respondida tanto em `disputa_origem` quanto em `perguntas_que_resolvem`. (`app/ponto/supra_juiz.py:509-520`)

**Comando `emitir_furo_retroativo`:**

- Janela explícita; colaboradores da empresa ativos; pula quem não tem batida na janela de timestamps início−1 até fim+3. Lê `grade_da_celula`. (`app/ponto/management/commands/emitir_furo_retroativo.py:40-59`)
- Busca todos os status dos chamados do colaborador em `FURO_MODULES`; agrupa por `dia_do_fato(ch, fortes=True)`, diferentemente do agrupamento direto pelo catálogo do comando anterior. (`app/ponto/management/commands/emitir_furo_retroativo.py:77-84`)
- `dia_do_fato(..., fortes=True)` prefere `celula.data`; depois a chave declarada no catálogo, com fallback para `contexto.data_turno`; interpreta como data e retorna `None` sem esses degraus. (`app/chamados/juizes.py:78-119`)
- Para cada dia da janela: `lista`, `vivos=VIVOS`, `_com_fio` por `fio_vivo`; passa os respectivos comprimentos. Sem filtro de marcos acesos nem de `trabalha` na formação de `vivos`. (`app/ponto/management/commands/emitir_furo_retroativo.py:88-108`)
- **Consome exclusivamente `FURO_SEM_COBRANCA`**; todos os outros códigos são descartados pelo `continue`. Logo ignora os três códigos que dependem diretamente de `chamados_vivos`. (`app/ponto/management/commands/emitir_furo_retroativo.py:109-110`)
- Encontrando esse código, exige vínculo vigente, `eh_dia_trabalho(data) is True`, primeiro marco presente e `lista` vazia; conta descartes nas categorias correspondentes. Com `--apply` chama `detectar_ausencias.Command._abrir_chamado`; sem a flag apenas imprime. (`app/ponto/management/commands/emitir_furo_retroativo.py:111-146`)
- `chamados_total` continua relevante: pode fazer o classificador escolher `FURO_COM_COBRANCA_MORTA` em vez de `FURO_SEM_COBRANCA`; o retroativo ignora o primeiro. (`app/ponto/supra_juiz.py:352-362`, `app/ponto/management/commands/emitir_furo_retroativo.py:109-110`)
- A chamada ao emissor não lavra célula diretamente; `_abrir_chamado` é documentado como criador de chamado com push, e recebe sem `marcos_faltantes` nessa via. (`app/ponto/management/commands/emitir_furo_retroativo.py:142`, `app/ponto/management/commands/detectar_ausencias.py:570-579`)

**Vocabulários comuns:**

- `FURO_MODULES`: `batida_ausente`, `turno_aberto`, `intervalo_pendente`, `entrada_bloqueada_turno_aberto`. (`app/chamados/catalogo/furo_modulos.py:15-16`)
- `VIVOS`: `aberto`, `em_analise`, `registrado`; `COBRANCA` só `aberto`, `em_analise`. Portanto os contadores descritos incluem `registrado`. (`app/chamados/catalogo/motor.py:24-29`)
- `data_do_chamado` retorna o valor da chave declarada no catálogo, sem interpretar a data, e devolve `None` sem módulo/chave declarada. (`app/chamados/catalogo/modulos.py:219-225`)

**Questão 5 — distinguir funções existentes da função exata pedida:**

- Não há uma função compartilhada pelos três arquivos para produzir o universo de `chamados_vivos` passado ao classificador: os comandos montam suas próprias listas; o cartório monta `_protestam`. (`app/ponto/management/commands/supra_juiz.py:173-208`, `app/ponto/management/commands/emitir_furo_retroativo.py:80-101`, `app/ponto/services/cartorio.py:1206-1214`)
- Há `fio_vivo(perguntas)`, compartilhada, mas ela só responde se existe pergunta viva na lista recebida; seu próprio contrato diz literalmente: **“O UNIVERSO e do chamador”**. Não seleciona chamados do dia nem filtra módulo, marcos ou `trabalha`. (`app/chamados/juizes.py:1133-1142`)
- Há `cobranca_viva_do_dia(colaborador, modulo_origem, dia, excluir=None)`: retorna o chamado VIVOS mais novo dos módulos recebidos que bate com o dia forte; não verifica fio vivo, marcos acesos ou `trabalha`. (`app/chamados/juizes.py:1771-1787`)
- Essa função é reutilizada por cartório, triagem, entrada sincronizada e gates do modelo, mas **não monta os contadores dos chamadores do classificador**. (`app/ponto/services/cartorio.py:1382`, `app/ponto/services/triagem_batida.py:588-589`, `app/ponto/services/entrada_sincronizada.py:24-28`, `app/chamados/models.py:368`, `:690`)
- Há ainda `chamados_vivos_do_dia` em `veredito_celula`: une chamados VIVOS por `contexto.data_turno` e por relação com a célula, sem filtro de módulo, fio, marcos ou `trabalha`. Tem um uso direto, dentro de `fechar_o_fio_do_dia`. (`app/ponto/services/veredito_celula.py:136-157`, `:182`)
- `fechar_o_fio_do_dia` é reutilizada pelo veredito, pela declaração do dia e pela validação; isso não significa reutilização daquele universo na classificação do cartório/comandos. (`app/ponto/services/veredito_celula.py:325`, `app/ponto/services/declarar_dia.py:198`, `app/chamados/services/validacao.py:271`)
- Outra seleção particular é `_chamados_vivos_do_dia` de cobertura: todos os módulos VIVOS, preferindo célula e depois catálogo, usada só pela própria cobertura. (`app/ponto/services/cobertura.py:218-231`, `:309`)

## 31/10 21:32 · sessao 2fe618724668 · - · - · origem=Ronald (tmux)

Censo do circuito de estados, somente código local.

- **Código → estado:** `_MAPA_VEREDITO` mapeia `FURO_SEM_COBRANCA` e `FURO_COM_COBRANCA_MORTA` para `furo`; `COBRANCA_SEM_FURO` e `COBRANCA_EM_DIA_SEM_TRABALHO` para `cobranca_indevida`; `COBRANCA_EM_DIA_COBERTO` para `coberta`. A ordem do mapa decide a precedência quando há vários códigos; lista vazia vira `concorde`. (`app/escala/models.py:1579-1600`)
- **Escritor de veredito/protestos/ata:** `CelulaDia.lavrar_veredito` delega à porta `ponto.portas.celula.lavrar_veredito`. A porta atribui `ata`, `protestos`, `veredito`, `veredito_via`, `veredito_em` e salva; o cartório lhe entrega todos os `cods` e a ata da passada. (`app/escala/models.py:1787-1796`; `app/ponto/portas/celula.py:709-775`; `app/ponto/services/cartorio.py:1565-1586`)
- **Outros chamadores produtivos da mesma porta:** completar status de célula já lavrada; ramo nunca-bateu; julgamento comum do cartório; decisão humana pelo serviço `veredito_celula`. (`app/ponto/services/cartorio.py:734`; `app/ponto/services/cartorio.py:1044`; `app/ponto/services/cartorio.py:1565`; `app/ponto/services/veredito_celula.py:330`)
- **Admin prevalece:** lavra do cartório preserva o veredito humano, mas pode atualizar ata e status; lavra humana conserva os protestos existentes. (`app/ponto/portas/celula.py:737-751`)
- **Lâmpadas não são o efeito direto desses códigos:** `lampadas_do_dia` produz `acesa/luz/tipo_real` a partir das células casadas; `ata_do_dia` inclui a lista na ata; dispensa por ausência/justificativa aprovada pode ligar lâmpadas sem luz e marcá-las dispensadas. Os códigos seguem em `protestos`, enquanto lâmpadas seguem na ata entregue à mesma lavra. (`app/ponto/services/cartorio.py:429-443`; `app/ponto/services/cartorio.py:541-568`; `app/ponto/services/cartorio.py:611-624`; `app/ponto/services/cartorio.py:1565`)
- **Escritor de `status_local` nas retratações:** `reconciliar` consulta a decisão/gates e chama `_gravar_estado`; este atribui `status_local`, ajusta resolução/via/urgência, registra evento e salva. `RECONCILIAR` leva `aberto/em_analise/registrado` a `resolvido`. (`app/chamados/models.py:195-279`; `app/chamados/models.py:605-619`; `app/chamados/catalogo/motor.py:109-117`)
- **Nascimento de chamado:** a porta `abrir` constrói e cria `ChamadoColaborador` com `status_local=status`; é outro momento de escrita do campo, além das transições. (`app/chamados/models.py:1029`; `app/chamados/models.py:1046`)
- **Retratação completa:** `retratar → _retratar` testa retenção, chama `ch.reconciliar()`, pode rejeitar justificativa pendente vinculada, resolve perguntas e anexa ocorrência. O cartório, nos códigos CSF/SEM_TRABALHO, chama diretamente `x.reconciliar()`, não a unidade `retratar`. (`app/chamados/reconciliador.py:505-548`; `app/ponto/services/cartorio.py:1313-1325`)
- **Leitores síncronos de `status_local`:** signals rebaixam urgência e resolvem perguntas quando o chamado está em `ENCERRADOS`; outro signal compara status anterior/atual e chama `cartorio.julgar_celula` quando houve nascimento/mudança, exceto reentrada do próprio julgamento. (`app/chamados/signals.py:68-94`; `app/chamados/signals.py:155-180`)
- **Leitores de fila:** `qs_vivos` filtra `status_local__in=VIVOS`; `qs_na_fila` acrescenta `COBRANCA` e exclui silêncio futuro. A fábrica também lê `status_local in ENCERRADOS` para recusar chamado já encerrado. (`app/chamados/services/painel.py:153-155`; `app/chamados/services/painel.py:186-188`; `app/chamados/services/disputa_emissao.py:690-692`)
- **Leitor C6/pergunta viva:** lê a lâmpada do marco perguntado antes do veredito geral; depois considera se `cel.veredito` pertence a `VEREDITOS_ACUSAM`, com ressalva dos marcos pendentes. (`app/chamados/juizes.py:1073-1095`)
- **Leitor de premissa/furo:** `_celula_furada` consulta `dia_sem_trabalho`, exige chave `lampadas` e lê lâmpadas `acesa is False` e órfãs; `dia_sem_trabalho` lê `cel.trabalha is False` ou `ata.tipo_dia` preenchido diferente de trabalho. (`app/chamados/juizes.py:398-408`; `app/chamados/juizes.py:442-467`)
- **Leitor por marco:** `lastro.marcos_acesos` usa os marcos cobrados pelo chamado e cada lâmpada da ata; devolve `True`, `False` ou `None`. É usado pelo cartório e pela rede horária, conforme contrato do próprio método. (`app/ponto/services/lastro.py:153-181`)
- **Leitores emissores:** `fabrica_da_celula` cala quando célula tem veredito fora de `VEREDITOS_ACUSAM`; `disputa_emissao` usa a mesma lista para saber se pergunta encerrada retira chamado da fábrica. (`app/chamados/services/fabrica_da_celula.py:78-89`; `app/chamados/services/disputa_emissao.py:695-706`)
- **Leitor folha:** `motivos_retencao_celula` lê `VEREDITOS_ACUSAM` e protestos; retém `espelho_<veredito>` salvo protestos todos não cobráveis ou dispensa de pausa. `coberta` fica fora de `VEREDITOS_ACUSAM`, enquanto `furo` e `cobranca_indevida` estão dentro. (`app/folha/export.py:1215-1238`; `app/escala/models.py:1735-1736`)
- **Leitor/retratador assíncrono:** `_absolvido_pela_celula` aceita célula com veredito preenchido fora de `VEREDITOS_ACUSAM`, sem marcos pendentes; `retratar_chamados_absolvidos_por_celula` percorre chamados VIVOS de módulos de turno e retrata pela unidade canônica. `consumir_eventos_celula` chama esse leitor e encerra órfãs pela lâmpada. (`app/chamados/reconciliador.py:949-986`; `app/chamados/reconciliador.py:1300-1308`)
- **Evento da lavra:** salvar célula e inserir `EventoCelula` ocorrem no mesmo `atomic` quando campos relevantes mudam. (`app/ponto/portas/celula.py:626-642`)
- **Leitor que realimenta o juiz:** `grade_da_celula` leva o veredito, contagens lavradas, lâmpadas convertidas em células e insumo de `dia_sem_trabalho` para o dicionário `dia`. (`app/escala/services/leitor_celula.py:63-101`; `app/escala/services/leitor_celula.py:345-370`)

Não fiz afirmação de que esta lista seja o censo exaustivo de **todo** leitor de `status_local`/ata/veredito da aplicação; ela é o circuito produtivo diretamente relacionado aos efeitos em questão. A busca encontrou leitores adicionais em relatórios, APIs, calendários, vigias e comandos, cujo comportamento completo não foi inspecionado. [não verificado]

## 31/10 21:31 · sessao f063b36dd0dc · - · - · origem=Ronald (tmux)

Censo documental, somente leitura.

- **Regra geral de leitura da lâmpada:** literal “O CONSUMIDOR LE A LAMPADA, nunca re-julga: chamado, pergunta, tela e folha leem a celula/ata” (`app/docs/LEIS.md:95`); equivalente “CONSUMIDOR (chamado, pergunta, tela, folha) le a lampada, nunca re-julga” (`CLAUDE.md:245`).

- **Corte que nomeia os três alimentadores:** o aval literal inclui “todo leitor de chamados_vivos (disputa_emissao.py:670, supra_juiz.py:163-168, core/juizes.py _C1, cartorio.py:780, commands/supra_juiz.py:182, emitir_furo_retroativo.py:79)” e determina “FASE 2 direto pela CLAUDE.md 4a: cada escritor passa a fechar a pergunta APAGANDO a lampada pela porta da celula, ou se aposenta (L-111); lampada que reacende reabre o fio; os leitores leem a lampada.” As linhas internas citadas pelo documento são históricas, mas o texto literal está em `app/docs/CORTES.md:255`.

- **Separação chamado vivo × cobrança viva:** o registro diz literalmente “`VIVOS` responde so a primeira metade” e explica que ler `VIVOS` para saber se alguém cobra confundia chamado vivo com fio morto (`app/core/juizes.py:100-106`). A pergunta “o chamado esta vivo?” é `_C1` (`app/core/juizes.py:107`), registrada em `chamados/catalogo/motor.py::VIVOS` (`app/core/juizes.py:256`); “alguma pergunta desta colecao esta viva?” é `_C15` (`app/core/juizes.py:123-125`), registrada em `chamados/juizes.py::fio_vivo` (`app/core/juizes.py:270`).

- **Corte F1 específico sobre universo no julgamento:** “o corte das 22:1x vale tambem para 'cobranca_indevida' (COBRANCA_EM_DIA_SEM_TRABALHO e COBRANCA_SEM_FURO): com todos os marcos acesos NESTE julgamento, o chamado nao conta como cobranca viva e o veredito nao le o proprio chamado vivo (4a).” (`app/docs/CORTES.md:405`)

- **Extensão classe 4:** “o veredito nunca le o proprio chamado vivo, cura na origem sem predicado novo nem excecao” e “cobranca_indevida deixa de ler o chamado vivo do proprio dia” (`app/docs/CORTES.md:409`). A versão seguinte amplia: “o veredito nunca le o proprio chamado vivo vale para TODA lampada, inclusive 'cobrado' em dia sem trabalho” (`app/docs/CORTES.md:412`).

- **Universo de `_protesta` expressamente documentado:** “NAO MIGRADO (declarado, BACKLOG CARTORIO-PROTESTA-1A-CLAUSULA)” (`app/core/juizes.py:145`); lê “so a 1a clausula (`trabalha is not False`, alem dos marcos acesos; corte O261-E-F1-CLASSE-4 31/10 00:0x/00:4x)” (`app/core/juizes.py:146`); “com trabalha=False zera chamados_vivos antes de classificar_dia” (`app/core/juizes.py:147`); “a 2a clausula (ata.tipo_dia) seria predicado novo -- pergunta com numeros, nao cura calada” (`app/core/juizes.py:148`). O mesmo consta do anexo/local `app/docs/JUIZES.md:60-62` e do corte `app/docs/CORTES.md:415`.

- **O ramo COBRANCA_EM_DIA_SEM_TRABALHO tem juiz diverso de `_protesta`:** o corte diz literalmente “`trabalha is False` ou `ata.tipo_dia` PREENCHIDO e != trabalho, L-061; `trabalha=None` sem ata de outro tipo COBRA” (`app/docs/CORTES.md:415`). O registro especifica ata **deste julgamento, antes da lavra**, e distingue a pergunta de `eh_dia_trabalho` do vínculo, que responde pelo previsto (`app/core/juizes.py:136-140`). O corte preserva `SEM_JURISDICAO` como guarda anterior e declara o transporte de `trabalha` + `ata` pelo produtor do dict (`app/docs/CORTES.md:415`; `app/core/juizes.py:143-144`).

- **Histórico que alerta contra estreitar `vivos` indiscriminadamente:** o texto explica “no `cartorio.py:907` o MESMO `vivos` decide EMISSAO, logo a cura separa predicados (*ha chamado vivo* x *ha cobranca viva*) e nao estreita `vivos`” (`app/docs/CORTES.md:64`; repetido em `:67`). Trata-se de registro histórico, não evidência da numeração ou da implementação atual (`app/docs/CORTES.md:64`).

- **Declarações de autoridade complementares:** “VIVOS/COBRANCA/TERMINAIS/NAO_FECHADOS, nunca tupla literal, nem em aspas duplas” (`app/docs/JUIZES.md:51-52`) e dia por `catalogo/modulos.py::data_do_chamado`, “nunca .get('data_turno') cru” (`app/docs/JUIZES.md:53-54`).

Não localizei nesses três arquivos uma especificação individual completa de filtros de módulo/status/dia para **cada** comando; a ausência absoluta de outro texto normativo no repositório é **[não verificado]**. O que há documentalmente é a ordem abrangente que nomeia todos os alimentadores (`app/docs/CORTES.md:255`) e a especificação recente e explícita de `_protesta`/`dia_sem_trabalho` (`app/core/juizes.py:136-148`; `app/docs/CORTES.md:415`).

## 31/10 21:18 · sessao f0d8537b15e0 · GRADE-FOLGA-JUIZ · verificador · origem=Code

**Pré-veredito: não liberaria o pouso ainda.** A catraca deixa passar uma cópia literal do predicado que deveria proteger; há também uma diferença de alimentação em `fase_conflitante` que impede afirmar equivalência geral entre os sítios. O ensaio exigido antes do pouso não está demonstrado nos anexos: **[nao verificado]**. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:62`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:75`, `app/ponto/precedencia.py:237`, `app/escala/services/fase_conflitante.py:82`, `app/docs/BACKLOG.md:796`)

As referências abaixo são à cópia indicada pelo usuário como `1093275a`; a identidade Git da cópia é **[nao verificado]**. RED histórico, suíte Django e resultados de produção permanecem **[nao verificado]**.

**1. Quem escreve e quem lê**

O novo juiz está autorizado e registrado: a pergunta é de `folga_lancada_do_dia`; o carregador preserva todas as candidatas, ordenadas por PK, e o seletor consulta um único predicado. (`app/docs/CORTES.md:416`, `app/core/juizes.py:192`, `app/ponto/precedencia.py:194`, `app/ponto/precedencia.py:206`, `app/ponto/precedencia.py:212`)

| Função/campo | Quem produz ou escreve | Quem lê |
|---|---|---|
| `FolgaDia.escala_colaborador`, `data`, `calendario_origem` | `gerar_foto_folgas` cria a foto do calendário; preserva manuais e apaga a foto anterior de calendário. | Carregador novo, alimentação mensal, gerador, cadastro e auditor C5. (`app/escala/folgas.py:165`, `app/escala/folgas.py:173`, `app/ponto/precedencia.py:206`, `app/escala/alimentacao.py:66`, `app/escala/models.py:1257`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1532`) |
| Avulsa: vínculo, data e `tipo` | `lancar_folga` e `_foto_do_dia`; este também altera `tipo` e remove a manual. | Juiz lê vínculo/origem; `tipo` é rótulo informativo, não condição de validade. (`app/escala/folgas.py:254`, `app/escala/folgas.py:259`, `app/escala/folgas.py:274`, `app/escala/folgas.py:284`, `app/escala/folgas.py:288`, `app/escala/models.py:1564`, `app/ponto/precedencia.py:237`) |
| Reparentamento da avulsa | `reparentar_folga` altera `escala_colaborador`; calendário não é reparentado. | Os mesmos leitores da foto. (`app/escala/folgas.py:405`, `app/escala/folgas.py:434`) |
| Escrita implícita das FKs | Exclusão do vínculo usa `CASCADE`; exclusão do calendário usa `SET_NULL`. | O predicado interpreta origem nula como avulsa: essa escrita implícita também afeta a classificação. (`app/escala/models.py:1553`, `app/escala/models.py:1557`, `app/ponto/precedencia.py:237`) |
| `{data: [FolgaDia...]}` | `folgas_lancadas_do_periodo`; somente memória, sem persistência. | `fatos_do_periodo`, veto, declaração, comando de rejulgamento, medidor de fase e overlay da grade. (`app/ponto/precedencia.py:205`, `app/ponto/precedencia.py:281`, `app/ponto/precedencia.py:501`, `app/ponto/services/declarar_dia.py:74`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:64`, `app/escala/services/fase_conflitante.py:80`, `app/escala/utils.py:1119`) |
| Folga selecionada / validade | `folga_lancada_do_dia` retorna a primeira candidata aceita; `folga_lancada_vale_no_dia` decide origem e vínculo. | Os consumidores acima; só o seletor chama diretamente o predicado. (`app/ponto/precedencia.py:216`, `app/ponto/precedencia.py:222`) |
| `faixa`, `tipo_do_dia`, `era_dia_previsto`, `suprimidos`, `fatos.folga_lancada` | `fatos_do_periodo`; `fatos_do_dia` é sua entrada para um dia. | Calendário, fatos do status e consumidores do juiz, incluindo a coleta de chamados espúrios. (`app/ponto/precedencia.py:256`, `app/ponto/precedencia.py:397`, `app/colaboradores/services/calendario.py:807`, `app/ponto/status_dia.py:139`, `app/chamados/services/feriado_reavaliar.py:33`) |
| Grade: `tipo_dia`, `celulas`, `orfas` | Builders montam; `_aplicar_folgas_foto` aplica o resultado do juiz; ausência e isenção ainda fazem seus overlays. | Cartório e testemunhas da grade. (`app/escala/utils.py:1124`, `app/escala/utils.py:1794`, `app/escala/utils.py:1797`, `app/ponto/services/cartorio.py:946`, `app/ponto/services/cartorio.py:582`) |
| Calendário: `status`, `eh_trabalho` | `_classificar` e `trabalho_do_dia`, sobre o mesmo dict, publicados por `contexto_calendario`. | Template, contagens, `status_do_dia`, `contar_por_status` e oferta de “Resolver dia”. (`app/colaboradores/services/calendario.py:818`, `app/colaboradores/services/calendario.py:839`, `app/colaboradores/services/calendario.py:903`, `app/colaboradores/services/calendario.py:202`, `app/colaboradores/services/calendario.py:226`, `app/colaboradores/services/calendario.py:237`, `app/templates/colaboradores/partials/_calendario_grade.html:69`) |
| `REGRA_VERSOES` | Declaração em código, agora com versão 3. | `versao_para`; a versão entra na impressão dos insumos. (`app/ponto/services/cartorio.py:111`, `app/ponto/services/cartorio.py:193`, `app/ponto/services/cartorio.py:367`) |

Os demais campos do dict do calendário — `ausencia_desconta`, `futuro`, `isento`, `regua_suspeita`, `tem_batida`, `turno_aberto`, `desvio`, `celula`, `celula_trabalha`, `veredito`, `veredito_via`, `cobertura`, `cobertura_desconta` e `retido_folha` — são montados em `contexto_calendario` e consumidos por `_classificar`; o diff não os transforma em campos persistidos. (`app/colaboradores/services/calendario.py:818`, `app/colaboradores/services/calendario.py:122`, `app/colaboradores/services/calendario.py:169`)

Fora do diff, há consumidores relevantes das funções alteradas:

- `o_dia_ja_e_folga` participa do aceite idempotente e da repetição após corrida em validação. (`app/chamados/services/validacao.py:261`, `app/chamados/services/validacao.py:316`)
- O veto afeta ausência, férias e as portas de folga. (`app/ponto/services/ausencia.py:380`, `app/ferias/models.py:428`, `app/ferias/views.py:253`, `app/escala/folgas.py:258`, `app/escala/folgas.py:283`)
- O medidor entrega `total`, `vinculos` e detalhes; o comando grava `MetricaSnapshot`, e a API lê o snapshot. (`app/escala/services/fase_conflitante.py:100`, `app/escala/management/commands/lavrar_fase_conflitante.py:35`, `app/api/views_mensageria.py:643`)

**Sim, permanecem leitores com resposta própria.** O C5 aceita toda `FolgaDia` cujo vínculo cubra o dia **ou** para a qual encontre outro vínculo cobrindo; não distingue origem. Calendário velho inválido continua podendo virar “contradição” contra célula de trabalho. Está expressamente declarado como fila `GRADE-FOLGA-C5`. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1530`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1542`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:15`)

Também permanece a foto própria do gerador, discutida no item 6. As consultas de Plano de Folgas, diagnóstico e limbo são por vínculo/mês, portanto sua mera existência não prova segundo juiz da pergunta da pessoa. (`app/escala/models.py:1257`, `app/escala/views_plano_folgas.py:60`, `app/ponto/services/diagnostico_escala.py:102`, `app/relatorios/furos.py:112`)

**2. Antes, depois e contraexemplos**

Considere velho encerrado em 09/10, novo desde 10/10, célula de trabalho de 15/10 pertencente ao novo; sem outra ausência ou feriado prevalecente. Esse é o desenho dos testes nominais. (`app/ponto/tests/test_grade_folga_juiz_sitios.py:46`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:49`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:51`)

| Sítio | Calendário do velho em 15/10: antes → depois | Avulsa no velho em 15/10: antes → depois |
|---|---|---|
| `_folga_avulsa` / veto | Encontrava e vetava → não veta por essa folga. | Encontrava e vetava → continua vetando folga/atestado. Férias atravessam folga. (`app/ponto/precedencia.py:161`, `app/ponto/precedencia.py:491`, `app/ponto/precedencia.py:505`) |
| `_folga_do_dia` / declaração | Não encontrava no vínculo novo → continua não encontrando folga válida. | Não encontrava → encontra; `o_dia_ja_e_folga=True` e recusa “já é folga”. (`app/ponto/services/declarar_dia.py:69`, `app/ponto/services/declarar_dia.py:95`, `app/ponto/services/declarar_dia.py:112`) |
| `rejulgar_ausencia_em_folga` | Entrava no cruzamento empresa/data → é descartado pelo juiz. | Entrava → continua entrando. Só vira achado se houver ausência aprovada não-falta cobrindo o dia. (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:24`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:56`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:64`) |
| `fase_conflitante.medir` | Foto presa ao velho não entrava na foto do novo → continua não entrando. | Não entrava → passa a entrar, dentro da vigência carregada do novo. (`app/escala/services/fase_conflitante.py:75`, `app/escala/services/fase_conflitante.py:80`) |
| Precedência | Já descartava calendário alheio, mas só examinava a primeira linha → percorre todas até achar uma válida. | Continua reconhecendo a avulsa da pessoa. (`app/ponto/precedencia.py:201`, `app/ponto/precedencia.py:216`, `app/ponto/precedencia.py:357`) |

Um contraexemplo importante corrigido: calendário inválido com PK menor seguido de calendário válido no mesmo dia. Antes, a primeira candidata escondia a segunda; agora ambas chegam ao seletor. (`app/ponto/precedencia.py:201`, `app/ponto/precedencia.py:206`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:85`)

**Sem vínculo**, o juiz rejeita calendário e aceita avulsa. `o_dia_ja_e_folga` passa a reconhecer essa avulsa, mas `_recusa_do_dia` continua recusando **nova declaração** por “sem vínculo” antes de consultar a folga. Existência do fato e autorização para declarar são perguntas diferentes. (`app/ponto/precedencia.py:235`, `app/ponto/services/declarar_dia.py:95`, `app/ponto/services/declarar_dia.py:109`)

O comando de rejulgamento também pode selecionar essa avulsa sem vínculo, pois usa `escala_vigente(...)=None`; a realização posterior depende de haver célula julgável. O medidor de fase, por sua vez, percorre somente vínculos ativos. (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:65`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:78`, `app/ponto/services/cartorio.py:1722`, `app/escala/services/fase_conflitante.py:72`)

**Na fronteira**, início e fim são inclusivos. Sem geradora, vence o vínculo ativo e de início mais recente entre os que cobrem a data. Com geradora declarada, ela manda enquanto vigente; se estiver fora da vigência, a juíza retorna `None`, sem escolher substituto. Portanto “dia da troca” exige informar a célula geradora, além das datas dos vínculos. (`app/escala/alimentacao.py:182`, `app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:250`, `app/escala/alimentacao.py:254`)

Há uma **ressalva no sítio nominal `fase_conflitante`**: ele entrega ao juiz o `ec` iterado, não o vínculo efetivo do dia. Contraexemplo: velho vigente até 20/10, novo ativo desde 10/10, célula de 15/10 gerada pelo velho, calendário preso ao novo. O medidor aceita essa candidata; a precedência, alimentada com o velho, rejeita. Isso é divergência de alimentação, não cópia do predicado. Para declarar os sítios equivalentes na pergunta do colaborador, precisa fechar esse caso ou explicitar que o contador responde outra pergunta. (`app/escala/services/fase_conflitante.py:80`, `app/escala/services/fase_conflitante.py:82`, `app/ponto/precedencia.py:350`, `app/ponto/precedencia.py:357`, `app/escala/alimentacao.py:247`)

**3. Catraca AST e mordida dos mutantes**

Executei em memória as funções da catraca: o censo atual encontrou **21 chaves, iguais às 21 da allowlist**, e os quatro testes sintéticos passaram. Isso verifica o scanner e suas amostras; execução da suíte Django permanece **[nao verificado]**. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:91`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:110`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:158`)

| Forma sondada em memória | Resultado |
|---|---|
| `from escala.models import FolgaDia as FD` | Pega o filtro. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:47`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:62`) |
| Import direto local dentro da função | Pega; os apelidos são coletados na árvore inteira. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:46`) |
| `import escala.models as m; m.FolgaDia...` | Escapa: a raiz é `m`, não um apelido reconhecido de `FolgaDia`. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:48`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:62`) |
| `getattr(fd, 'calendario_origem_id', None)` ou manager via `getattr` | Escapa. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:62`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:77`) |
| Método `__init__` ou função em arquivo `__init__.py` | Pega, se estiver numa forma reconhecida; não existe exclusão especial desses nomes. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:81`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:97`) |
| `from .models import FolgaDia as FD` | Pega. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:47`) |
| `from . import models as m; m.FolgaDia...` | Escapa pelo mesmo motivo do import de módulo. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:48`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:62`) |
| `Q(...)`, `**kwargs`, teste booleano `not fd.calendario_origem_id`, código de módulo | Escapam às formas examinadas. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:33`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:75`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:81`) |

**O escape decisivo é literal:** copiar `folga_lancada_vale_no_dia` para outro arquivo produziu zero achados na sonda. O predicado verdadeiro usa `getattr`, enquanto a catraca procura comparação de atributo direto com `None`. Além disso, todo `precedencia.py` é excluído: um segundo juiz ali, com outro nome, também não seria barrado por esse censo. (`app/ponto/precedencia.py:235`, `app/ponto/precedencia.py:237`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:77`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:99`)

Os mutantes do **scanner** mordem parcialmente: removendo reconhecimento de apelido, falhou o teste de apelido; removendo comparação de origem, falhou o de origem; removendo emissão de achado de filtro, falharam dois testes. Esses são os comportamentos assertados pelas amostras existentes. O mutante “segundo predicado literal” sobrevive. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:158`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:163`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:169`)

“Só encolhe” significa, mecanicamente, **igualdade entre código atual e allowlist atual**. Não há comparação com uma allowlist anterior: adicionar uma cópia reconhecida e sua autorização no mesmo patch passa nessa igualdade. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:144`)

Os testes comportamentais têm pares úteis para veto, declaração, rejulgamento e fase; porém a execução do RED contra o main indicado e a morte de mutantes desses caminhos são **[nao verificado]**. (`app/ponto/tests/test_grade_folga_juiz_sitios.py:76`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:90`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:122`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:162`)

**4. O portão trata igual ao juiz?**

**Depois de relavrar, os testes cobrem os dois lados corretos:** avulsa velha permite fechar; calendário velho inválido não permite. Ambos passam antes por `julgar_celula(..., forcar=True)`. Não demonstram igualdade incondicional antes da relavra. (`app/ponto/tests/test_grade_folga_juiz_sitios.py:196`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:202`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:209`)

O portão consulta `premissa_regularizada(para='folha')`; esse coletor prioriza célula/ata. No cartório, recebe a projeção deste julgamento; fora dele, lê o gravado. (`app/chamados/models.py:699`, `app/chamados/models.py:1227`, `app/chamados/juizes.py:381`)

Contraexemplos à afirmação “sempre igual”:

- Avulsa válida, mas ata antiga de trabalho com lâmpada apagada: pode continuar recusando fechamento. (`app/chamados/juizes.py:454`, `app/chamados/juizes.py:457`, `app/chamados/models.py:1247`)
- Calendário velho inválido, mas ata antiga `tipo_dia='folga'`: pode regularizar pela ata. (`app/chamados/juizes.py:408`, `app/chamados/models.py:1232`)
- Sem célula julgável, a consulta ao juiz ainda exige `era_dia_previsto` conhecido para considerar o chamado espúrio. Avulsa existente, sozinha, não garante fechamento. (`app/chamados/services/feriado_reavaliar.py:33`)

Isso **não prova um segundo juiz de folga no portão**: o contrato distingue cobrança pela célula/ata da pergunta do previsto. O que não procede é vender o par de testes como prova de todos os estados anteriores à relavra. (`app/docs/JUIZES.md:55`, `app/chamados/models.py:1203`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:199`)

**5. As classes de reprovação**

| Classe | Verificação |
|---|---|
| Segundo juiz duplicando autoridade registrada | As cópias nominais foram substituídas por chamadas. C5 e gerador permanecem declarados; a catraca permite nascer outra cópia literal. (`app/ponto/precedencia.py:505`, `app/ponto/services/declarar_dia.py:74`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:15`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:128`) |
| Segundo escritor fora da porta | Não demonstrado como introdução deste diff: `FolgaDia` continua na porta registrada, e ata/veredito/carimbo passam pelas operações existentes da célula. (`app/core/portas.py:466`, `app/ponto/services/cartorio.py:1579`, `app/ponto/services/cartorio.py:1599`, `app/ponto/portas/celula.py:709`) |
| Relógio fora da borda | Juiz, carregador e overlay novos não leem relógio. Permanecem leituras próprias em `medir` e `julgar_celula`, inclusive `localdate` mesmo recebendo `agora`; não são introduções mostradas pelo diff. (`app/ponto/precedencia.py:194`, `app/escala/utils.py:1103`, `app/escala/services/fase_conflitante.py:68`, `app/ponto/services/cartorio.py:1711`, `app/ponto/services/cartorio.py:1732`) |
| Derivação paralela de fonte única | Reduzida na grade/calendário; ainda existe no gerador e no C5. (`app/escala/utils.py:1124`, `app/colaboradores/services/calendario.py:820`, `app/escala/models.py:1281`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1532`) |
| Selo/RED sem mordida | Há mordida nas formas sintéticas, mas o predicado literal escapa; RED histórico e integração: **[nao verificado]**. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:158`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:77`, `app/ponto/precedencia.py:237`) |
| Ordem de trava | Não há inversão nova demonstrada pelos trechos alterados; o overlay só consulta e altera memória. Persistência de célula/evento mantém o mesmo `atomic`. Concorrência real: **[nao verificado]**. (`app/escala/utils.py:1115`, `app/ponto/portas/celula.py:639`, `app/escala/folgas.py:257`) |
| Pouso antes do pré-requisito | O pré-requisito é explícito: sombra `--apply` sobre main, reversão e frota antes do opus. Cumprimento: **[nao verificado]**. (`app/docs/BACKLOG.md:796`, `app/ponto/services/cartorio.py:174`) |

Há ainda correção documental devida no mesmo commit: `JUIZES.md` manda acompanhar o registro vivo, mas sua tabela omite o novo juiz. Não procede “juiz novo sem corte”; procede “lookup desatualizado”. (`app/docs/JUIZES.md:14`, `app/docs/JUIZES.md:72`, `app/core/juizes.py:192`, `app/docs/CORTES.md:416`)

**6. Grade, gerador persistente e competência exportada**

Em `escala/utils.py`, a mudança é no **builder da grade**: uma carga da pessoa e aplicação contra o vínculo do dia. Por turno, usa `esc_por_dia`; no legacy, os segmentos vêm da mesma juíza, e o builder individual usa o vínculo daquele segmento. A aplicação escreve somente os dicts `tipo_dia`,

[truncado em 20000 de 23379 caracteres]

## 31/10 21:14 · sessao c16e4ad39e68 · - · - · origem=Ronald (tmux)

Achados consolidados, somente por código:

- **Os quatro sítios nominais passam pelo juiz**, porém `fase_conflitante.medir` fornece o vínculo iterado `ec`, enquanto precedência/veto/declarar usam o vínculo efetivo do dia. Isso permite diferença em sobreposição: velho vigente até 20/10, novo desde 10/10, célula de 15/10 gerada pelo velho e folga de calendário presa ao novo → medidor aceita; precedência rejeita. É alimentação incorreta do juiz, sem cópia do predicado. Prova pura executada em memória confirmou `True` com `ec=new` e `False` com `ec=old`. (`app/escala/services/fase_conflitante.py:80-82`; `app/ponto/precedencia.py:350-357`; `app/escala/alimentacao.py:237-254`.)

- **Sem vínculo:** calendário é rejeitado; avulsa é aceita pelo juiz. `o_dia_ja_e_folga` agora retorna verdadeiro para avulsa sem vínculo, mas `_recusa_do_dia` continua retornando “sem vinculo” antes de consultar folga. São perguntas diferentes: fato existente versus autorização para declarar. (`app/ponto/precedencia.py:235-239`; `app/ponto/services/declarar_dia.py:95`; `app/ponto/services/declarar_dia.py:108-118`.)

- **Fronteira:** início/fim são inclusivos. Com célula nomeando geradora, ela vence se ainda vigente; geradora fora da vigência produz `None`, sem escolher outra. Sem geradora, desempate é vínculo ativo e início mais recente. (`app/escala/alimentacao.py:182-184`; `app/escala/alimentacao.py:237-254`.)

- **Segundo leitor da pergunta permanece no C5:** itera toda `FolgaDia`, verifica cobertura do vínculo ou existência de outro e acusa célula `trabalha=True`; não distingue origem nem pergunta ao juiz. Calendário do velho fora da vigência, havendo novo cobrindo o dia, continua contado indevidamente. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1530-1549`.) A própria catraca declara esse caso como fila `GRADE-FOLGA-C5`. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:15-18`.)

- **Gerador ainda lê foto do vínculo:** `folgas_do_periodo` agrupa datas por vínculo; `eh_dia_trabalho_calculado` consulta foto própria, sem carregar avulsas da pessoa presas ao velho. Isso permanece após alteração do overlay da grade. (`app/escala/alimentacao.py:49-70`; `app/escala/models.py:1251-1288`.) A catraca assume essa dívida como `GRADE-FOLGA-GERADOR`. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:125-126`.)

**Escritores de `FolgaDia` e campos:**

| Campo/ato | Escritor |
|---|---|
| calendário, vínculo, data | `gerar_foto_folgas`, cria com `calendario_origem=cal`, apaga calendário anterior do mês e preserva manuais — `app/escala/folgas.py:165-175` |
| avulsa: vínculo, data, tipo | `lancar_folga` — `app/escala/folgas.py:254-260`; `_foto_do_dia` — `app/escala/folgas.py:274-289` |
| alteração de tipo/exclusão manual | `_foto_do_dia` — `app/escala/folgas.py:277-289`, invocado pelas portas `alterar_folga` e lote — `app/escala/folgas.py:301-305`, `app/escala/folgas.py:328-342` |
| troca de vínculo da avulsa | `reparentar_folga` — `app/escala/folgas.py:434-436` |
| exclusão implícita/limpeza de origem | FK do vínculo usa CASCADE; FK do calendário usa SET_NULL — `app/escala/models.py:1553-1559` |

A porta registrada para `FolgaDia` é `escala/folgas.py`; os escritores explícitos encontrados estão nela. (`app/core/portas.py:466`.)

**Leitores fora do diff:**

- Foto de vínculo/mês: `alimentacao.folgas_do_periodo`, defeito de cadastro, gerador, grade do Plano, diagnóstico e relatório de limbo. (`app/escala/alimentacao.py:66`; `app/escala/models.py:1220`; `app/escala/models.py:1257`; `app/escala/models.py:1281`; `app/escala/models.py:1327`; `app/escala/views_plano_folgas.py:60`; `app/ponto/services/diagnostico_escala.py:102`; `app/relatorios/furos.py:112`.)
- Toggle lê a linha para decidir qual porta chamar e frase de calendário; escrita vai para `lancar_folga`/`alterar_folga`. (`app/escala/views_plano_folgas.py:112-136`.)
- `o_dia_ja_e_folga` alimenta aceite idempotente de validação e repetição após corrida. (`app/chamados/services/validacao.py:261`; `app/chamados/services/validacao.py:316`.)
- `veto_de_lancamento` afeta criação de ausência, férias e as portas de folga. (`app/ponto/services/ausencia.py:380`; `app/ferias/models.py:428`; `app/ferias/views.py:253`; `app/escala/folgas.py:258`; `app/escala/folgas.py:283`.)
- `folga_humana_ou_feriado` também alimenta fatos do status lavrado. (`app/ponto/status_dia.py:138-139`.)
- Medidor publica via `MetricaSnapshot`; API lê snapshot. (`app/escala/management/commands/lavrar_fase_conflitante.py:30-38`; `app/escala/services/fase_conflitante.py:113-121`; `app/api/views_mensageria.py:643`.)

**Antes/depois dos sítios:**

- Veto antes aceitava qualquer linha pessoa/data; depois calendário velho não veta e avulsa velha continua vetando folga/atestado, com férias atravessando folga. (`app/ponto/precedencia.py:161-165`; `app/ponto/precedencia.py:491-513`.)
- Declarar antes via só vínculo do dia; depois reconhece avulsa velha e mantém frase de calendário para calendário efetivo do dia. (`app/ponto/services/declarar_dia.py:69-75`; `app/ponto/services/declarar_dia.py:112-118`.)
- Rejulgar antes cruzava toda FolgaDia da empresa; depois usa pessoa + juiz + vínculo do dia. Continua só janela atual, ausência aprovada não-falta; `--apply` exige conjunto exatamente igual a `MEDIDAS`. (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:24-35`; `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:54-78`.)
- Fase antes via só linhas do vínculo ativo; depois incorpora avulsas da pessoa dentro da vigência carregada e rejeita calendário alheio ao `ec` fornecido, com ressalva de sobreposição acima. (`app/escala/services/fase_conflitante.py:72-88`.)
- Candidata inválida de menor PK não esconde válida posterior: carregador preserva lista ordenada por PK e seletor percorre até aceitar. (`app/ponto/precedencia.py:206-218`.)

**Documento desatualizado:** juiz novo está autorizado e registrado, sem duplicação nova do predicado; `JUIZES.md` não ganhou entrada correspondente, embora mande corrigir divergência no mesmo commit. (`app/docs/CORTES.md:416`; `app/core/juizes.py:188-192`; `app/docs/JUIZES.md:15-16`; `app/docs/JUIZES.md:72-83`.)

Não executei Django, banco, logs nem suíte. Resultado de RED histórico/produção permanece [nao verificado]. [nao verificado]

## 31/10 21:14 · sessao 047ccd303d90 · - · - · origem=Ronald (tmux)

- **O diff corrige a grade, não o gerador persistente de células.** `_aplicar_folgas_foto` agora carrega folgas da pessoa, consulta o juiz e altera somente `tipo_dia`, `celulas` e `orfas` do dicionário da grade; por turno usa o vínculo escolhido pela juíza, enquanto o legacy recorta segmentos pela mesma autoridade. (`app/escala/utils.py:1115`, `app/escala/utils.py:1124`, `app/escala/utils.py:1262`, `app/escala/utils.py:1794`, `app/escala/utils.py:973`, `app/escala/utils.py:995`)

- **Permanece uma derivação paralela no gerador:** `gerar_celulas_periodo` e `regenerar_celulas_vinculo` continuam chamando `ec.eh_dia_trabalho_calculado(d)`; esse método consulta somente as folgas do próprio vínculo, sem distinguir calendário/manual e sem procurar a manual presa ao vínculo antigo. Contraexemplo estático: novo vínculo 5x2 trabalhando na quinta, folga manual da pessoa presa ao velho nessa quinta → gerador pode escrever `trabalha=True`, enquanto juiz/overlay respondem folga. A catraca declara esse leitor como `INCOMPLETO GRADE-FOLGA-GERADOR`. (`app/ponto/portas/celula.py:243`, `app/ponto/portas/celula.py:590`, `app/escala/models.py:1251`, `app/escala/models.py:1257`, `app/escala/models.py:1280`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:129`)

- **A versão 3 muda a impressão, não seleciona uma regra antiga para a 09.** `versao_para` usa a competência e devolve a maior versão aplicável; a impressão incorpora `regra:N`. `processar_cartorio` seleciona todas as células passadas da empresa, e o pulo depende de carimbo, impressão e veredito em dia; `--forcar` desativa esse pulo. Portanto “09 só escreve com `--forcar`” é falso em geral: também pode escrever sem a flag quando houver impressão diferente ou carimbo incompleto. (`app/ponto/services/cartorio.py:193`, `app/ponto/services/cartorio.py:211`, `app/ponto/services/cartorio.py:367`, `app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:61`, `app/ponto/services/cartorio.py:1173`, `app/ponto/services/cartorio.py:1177`)

- **Há escrita em competência exportada:** célula/ata/veredito/status/carimbos podem ser relavrados. A grade alimenta `ata_do_dia`, que publica `tipo_dia`; a lavra e o carimbo gravam pela porta existente, com evento no mesmo `atomic`. Isso não constitui segundo escritor introduzido por este diff. A preservação integral de `FechamentoMensal` e exportações está afirmada pelo teste, incluindo linha inteira e conjunto de exportações; execução desse teste e preservação real da produção: **[nao verificado]**. (`app/ponto/services/cartorio.py:946`, `app/ponto/services/cartorio.py:582`, `app/ponto/services/cartorio.py:1577`, `app/ponto/services/cartorio.py:1599`, `app/ponto/portas/celula.py:709`, `app/ponto/portas/celula.py:787`, `app/ponto/portas/celula.py:639`, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:106`, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:118`)

- **Não reprovar estaticamente o teste sem `--forcar` por falta de carimbos na fixture:** as criações de batidas acionam julgamento por signal antes de o teste substituir somente a ata. Porém o teste cobre aquela fixture, não uma proteção universal da 09. (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:61`, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:74`, `app/ponto/signals.py:131`, `app/ponto/signals.py:85`, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:121`)

- **Relógio/ordem de trava:** o overlay novo não lê relógio nem abre transações; recebe datas e vínculo. O caminho normal da grade recebe `agora`, e o legacy recebe `ref` resolvido na entrada. Relógios adicionais já existem em `julgar_celula` e no comando, mas não foram introduzidos pelas mudanças examinadas; concorrência real ou inversão de travas em produção: **[nao verificado]**. (`app/escala/utils.py:1103`, `app/escala/utils.py:1118`, `app/escala/utils.py:2033`, `app/ponto/services/cartorio.py:1711`, `app/ponto/services/cartorio.py:1732`, `app/ponto/management/commands/processar_cartorio.py:38`)

- **Pré-requisito ainda não comprovado pelo material dado:** a obra exige sombra `--apply` sobre o main, reversão e prova de frota antes do opus/pouso. A existência de resultados válidos desses atos nesta árvore é **[nao verificado]**. O ato próprio de rejulgar a 09, medir pago versus devido e formar Pauta DP está explicitamente na fila posterior ao pouso. (`app/docs/BACKLOG.md:796`, `app/ponto/services/cartorio.py:174`, `app/docs/BACKLOG.md:846`)

## 31/10 21:14 · sessao e35f8f66a36f · - · - · origem=Ronald (tmux)

O portão concorda com o juiz nos testes novos **depois da relavra forçada**, mas esses testes não demonstram concordância incondicional: `_julgar` chama `julgar_celula(..., forcar=True)` antes das duas asserções. (`app/ponto/tests/test_grade_folga_juiz_sitios.py:196-214`)

- O portão consulta `premissa_regularizada(para='folha')`, além das exceções de agrupamento e deduplicação. (`app/chamados/models.py:699-722`)
- Esse coletor prioriza a célula/ata: veredito do admin, dia sem trabalho, marcos acesos/inexistentes e resultado de `_celula_furada`; só quando a célula não responde consulta `feriado_reavaliar.espurios`, que chama `fatos_do_dia`. (`app/chamados/models.py:1227-1254`; `app/chamados/services/feriado_reavaliar.py:27-39`)
- Dentro do cartório, a leitura projeta ata/veredito **deste julgamento** sobre os gravados; fora dele, lê a célula gravada. (`app/chamados/juizes.py:373-395`)

Contraexemplos estáticos ao enunciado “sempre responde igual”:

1. Folga avulsa antiga válida, célula `trabalha=True`, ata antiga `tipo_dia='trabalho'` com lâmpada apagada: o coletor encontra furo e retorna antes de consultar o juiz de folga; o portão pode recusar. (`app/chamados/models.py:1247-1254`; `app/chamados/juizes.py:443-458`; `app/chamados/models.py:709-724`)
2. Calendário antigo inválido, célula com ata antiga `tipo_dia='folga'`: `dia_sem_trabalho` regulariza antes de consultar o juiz, e o portão aceita. (`app/chamados/juizes.py:407-408`; `app/chamados/models.py:1232-1233`)
3. Sem célula/ata julgável, há consulta ao juiz, mas `espurios` exige `era_dia_previsto is not None`; portanto “há avulsa” sozinho não garante fechamento. (`app/chamados/juizes.py:437-455`; `app/chamados/models.py:1250-1254`; `app/chamados/services/feriado_reavaliar.py:33-39`)

Esses contraexemplos **não bastam para classificar o portão como segundo juiz de folga**: o contrato publicado distingue a pergunta da cobrança, respondida pela célula/ata, do trabalho previsto; o próprio coletor declara essa precedência. O achado seguro é o limite do teste e da afirmação “fechado por construção”. (`app/docs/JUIZES.md:55-57`; `app/chamados/models.py:1203-1208`; `app/ponto/tests/test_grade_folga_juiz_sitios.py:184-199`)

O relato histórico “juiz aceita avulsa velha, portão não” aparece em comentário de teste anterior, mas o novo teste afirma que uma sonda histórica encontrou concordância; nenhum desses comentários comprova o estado de produção permitido nesta auditoria. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:107-111`; `app/ponto/tests/test_grade_folga_juiz_sitios.py:184-187`; produção: [nao verificado])

O corte/registro do juiz está presente, portanto a acusação “juiz novo sem corte” não procede nesta cópia. (`app/core/juizes.py:188-192`; `app/docs/CORTES.md:416`)

A tabela `JUIZES.md` ainda omite o novo juiz na seção de célula/precedência, embora mande corrigir divergências no mesmo commit; cabe correção documental. (`app/docs/JUIZES.md:14`; `app/docs/JUIZES.md:72-83`; `app/core/juizes.py:192`)

O pré-requisito de pouso está escrito: ensaio `--apply` na sombra, medindo dias/minutos e `FORA-DO-HORIZONTE 0`, **antes** do pouso; a linha da obra exige sombra sobre main, reversão e frota antes do opus. O anexo fornecido não apresenta essa evidência: cumprimento [nao verificado]. (`app/ponto/services/cartorio.py:174-176`; `app/docs/BACKLOG.md:796`)

A Pauta da 09 está explicitamente em fila como ato posterior ao pouso, com ensaio `--forcar` e comparação pago/devido; não é pré-requisito documental desta obra. (`app/docs/BACKLOG.md:846`)

## 31/10 20:59 · sessao b9fa3134066a · PADRAO-DE-CIMA · verificador · origem=Code

**Veredito: bloqueia.** O fallback ainda pode liberar uma obra usando escopo antigo; o resolvedor pode bloquear sob a trava; e o harness aceita algumas mortes anteriores à decisão como “mordida”. (`bin/disjuntor.py:1488`, `bin/disjuntor.py:1530`, `bin/disjuntor.py:1247`, `bin/tests/_mordida.sh:219`, `bin/tests/_mordida.sh:260`)

A identidade Git da cópia e a sequência dos dois commits ficam **[não verificado]**: não há repositório Git acessível nesta cópia. Não executei os selos completos, pois criam, alteram e apagam arquivos temporários. (`bin/tests/test_padrao_de_cima.sh:29`, `bin/tests/test_padrao_de_cima.sh:39`, `bin/tests/test_disjuntor.sh:39`, `bin/tests/_mordida.sh:152`)

**1. Quem escreve e quem lê os estados**

| Estado | Quem escreve | Quem lê; multiplicidade |
|---|---|---|
| `logs/sessao/disjuntor_estado.json` | `_com_estado`: lê, chama o callback e substitui o arquivo sob `flock`. Há escritores lógicos B1, B3, PDC, B2 e B4, todos pela mesma porta física. | `_com_estado`, `ler_estado` e seus consumidores. **Uma porta física no instrumento**, vários produtores lógicos; os selos também escrevem diretamente nas árvores de teste. (`bin/disjuntor.py:244`, `bin/disjuntor.py:275`, `bin/disjuntor.py:281`, `bin/disjuntor.py:494`, `bin/disjuntor.py:986`, `bin/disjuntor.py:1460`, `bin/disjuntor.py:1745`, `bin/disjuntor.py:2029`, `bin/tests/test_padrao_de_cima.sh:556`) |
| `pdc.eventos[sha]`: `visto_em`, `tipo`, `obra`, `entidades`, `res` | `decide_pdc` cria o evento; na reclassificação altera `entidades/res`, preservando `visto_em`. | Deduplicação/reclassificação, janela, agrupamento, nomeação, histórico do escopo e painel. **Um escritor lógico no instrumento.** (`bin/disjuntor.py:1386`, `bin/disjuntor.py:1397`, `bin/disjuntor.py:1400`, `bin/disjuntor.py:1298`, `bin/disjuntor.py:1308`, `bin/disjuntor.py:1438`, `bin/disjuntor.py:1448`, `bin/disjuntor.py:1600`) |
| `pdc.nomeados` | `decide_pdc` marca `sha\|entidade` quando consome uma liberação. | `_pdc_disparadas` verifica se resta evento não nomeado. (`bin/disjuntor.py:1440`, `bin/disjuntor.py:1306`, `bin/disjuntor.py:1312`) |
| `pdc.libera_vistas` | `decide_pdc` registra o hash consumido. | A própria decisão impede reutilização da liberação. (`bin/disjuntor.py:1435`, `bin/disjuntor.py:1430`) |
| `pdc.fotos` | `decide_pdc` cria a fotografia do topo e remove fotografias de episódios encerrados. | A própria decisão impede liberação por linha anterior ao disparo. (`bin/disjuntor.py:1403`, `bin/disjuntor.py:1412`, `bin/disjuntor.py:1445`, `bin/disjuntor.py:1433`) |
| `pdc.disparadas` | `decide_pdc` substitui pelo agrupamento recalculado. | Reconhecimento de disparo novo, painel e cópia para o escopo. (`bin/disjuntor.py:1444`, `bin/disjuntor.py:1405`, `bin/disjuntor.py:1413`, `bin/disjuntor.py:1608`, `bin/disjuntor.py:1480`) |
| `hist`, `lido`, `duvida`, `final`, `liberadas`, `avisos` | A closure `_fn` mantém a fotografia e os resultados desta transição. | `_anuncia` grava escopo/trilha; depois, a construção de `DecisaoPDC` preserva negativas e incertezas. São estados por chamada, compartilhados pelas closures. (`bin/disjuntor.py:1366`, `bin/disjuntor.py:1378`, `bin/disjuntor.py:1392`, `bin/disjuntor.py:1447`, `bin/disjuntor.py:1451`, `bin/disjuntor.py:1462`) |
| `padrao_de_cima_escopo.json`: `em`, `disparadas`, `obras_com_evento`, `duvida` | `_pdc_grava_escopo`, chamado após gravar o estado, ainda sob sua trava. | `_pdc_escopo_lido` e fallback de `estado_pdc`. **Um escritor no instrumento**, mas gravação independente da gravação do estado. `em` é escrito e não é conferido no fallback. (`bin/disjuntor.py:293`, `bin/disjuntor.py:1456`, `bin/disjuntor.py:1480`, `bin/disjuntor.py:1487`, `bin/disjuntor.py:1496`, `bin/disjuntor.py:1512`) |
| `RELATO.md` | `relato_negacao` delega a `esteira_vigia.no_relato`, que tranca o diretório e faz substituição atômica. **Há outros escritores reais:** `gerar_cortes` e `deploy_agendado` escrevem diretamente, fora dessa trava. | PDC/B3 usam `_relato_consistente`; o Stop lê o topo; handoff, alarme de ociosidade e publicação também leem o documento. A multiplicidade de escritores já está declarada como resíduo. (`bin/disjuntor.py:236`, `app/core/esteira_vigia.py:848`, `app/core/esteira_vigia.py:884`, `bin/gerar_cortes.py:136`, `bin/deploy_agendado.sh:44`, `bin/disjuntor.py:697`, `bin/hook_stop_fila1.py:429`, `bin/handoff_sessao.sh:92`, `bin/alarme_sessao_ociosa.py:123`, `bin/relato.sh:104`, `app/core/esteira_vigia.py:839`) |
| Fontes `models.py`, `models/*.py` e arquivos citados; caches do resolvedor | O resolvedor somente lê as fontes; `_prepara` e `_por_sitio` preenchem `nomes`, `modelos`, mapas e `_fontes` por decisão. | `entidades` combina censo, modelos e cadastro `PROGRAMAS`. Autoria das alterações das fontes em produção: **[não verificado]**. (`bin/disjuntor.py:1214`, `bin/disjuntor.py:1235`, `bin/disjuntor.py:1273`, `bin/disjuntor.py:1289`, `app/core/censo_escritas.py:89`) |
| `payload`, `cru`, `_PDC_LIVRE_NESTA` | `main_pretool` lê stdin, faz o parse e limpa o conjunto; `pretool` acrescenta obra julgada livre. | Classificadores do envelope e `_ramo_pdc`, que dispensa nova negativa para aquela obra. (`bin/disjuntor.py:2038`, `bin/disjuntor.py:2041`, `bin/disjuntor.py:2045`, `bin/disjuntor.py:2018`, `bin/disjuntor.py:1591`) |

O mapa do harness é distinto: seus logs têm deliberadamente vários processos escritores. (`bin/tests/_mordida.sh:142`, `bin/tests/_mordida.sh:46`, `bin/tests/_mordida.sh:106`)

| Estado | Quem escreve | Quem lê; multiplicidade |
|---|---|---|
| `sonda_exc/sitecustomize.py` | O `source` do harness gera o arquivo. | Python descendente, pelo `PYTHONPATH` fornecido ao caso. (`bin/tests/_mordida.sh:35`, `bin/tests/_mordida.sh:37`, `bin/tests/_mordida.sh:143`) |
| `<raiz>.exc` | `_mordida_sondado` trunca; callbacks `RAISE` dos Python acrescentam registros. | Controle procura cinco tipos de erro; comparador verifica exceções declaradas. **Vários escritores.** (`bin/tests/_mordida.sh:139`, `bin/tests/_mordida.sh:106`, `bin/tests/_mordida.sh:176`, `bin/tests/_mordida.sh:222`, `bin/tests/_mordida.sh:252`) |
| `<raiz>.esc` | Truncador e `sys.excepthook` dos Python. | Comparador de multiplicidade de exceções escapadas. **Vários escritores.** (`bin/tests/_mordida.sh:139`, `bin/tests/_mordida.sh:117`, `bin/tests/_mordida.sh:223`, `bin/tests/_mordida.sh:258`) |
| `<raiz>.proc` | Truncador; startup escreve `INI`; `atexit` escreve `FIM`; handler escreve `SINAL`. | `abertos` compara conjuntos de PIDs e quantidades. **Vários escritores.** (`bin/tests/_mordida.sh:139`, `bin/tests/_mordida.sh:46`, `bin/tests/_mordida.sh:50`, `bin/tests/_mordida.sh:56`, `bin/tests/_mordida.sh:214`) |
| `<raiz>.marca` | Truncador e os três coletores Python. O novo trap Bash escreve suas provas em variáveis. | `_mordida_morre` consulta a última marca quando o último comando não é `return`. (`bin/tests/_mordida.sh:139`, `bin/tests/test_disjuntor.sh:776`, `bin/tests/test_disjuntor.sh:947`, `bin/tests/test_disjuntor.sh:1122`, `bin/tests/_mordida.sh:191`) |
| `base_<caso>.exc/.proc/.esc` | `_mordida_controle` copia cada arquivo somente se ainda não existir. | Mutantes posteriores reutilizam a base pelo **nome do caso**, entre diferentes raízes. (`bin/tests/_mordida.sh:179`, `bin/tests/_mordida.sh:196`, `bin/tests/_mordida.sh:225`, `bin/tests/_mordida.sh:258`, `bin/tests/test_padrao_de_cima.sh:648`) |
| `<raiz>.orig.d`, `<raiz>.novo` e fonte mutada | `_mordida_muta` remove/recria a fotografia original, aplica substituições e grava os textos novos. | Verificador da declaração de guarda. (`bin/tests/_mordida.sh:152`, `bin/tests/_mordida.sh:159`, `bin/tests/_mordida.sh:167`, `bin/tests/_mordida.sh:168`, `bin/tests/_mordida.sh:246`) |
| `_MORDIDA_CASO/PID/SEQ/ULT/VAL/MARCAS` | Inicializador e trap `DEBUG`. | Trap e julgamento do último comando. São globais dentro do selo. (`bin/tests/_mordida.sh:124`, `bin/tests/_mordida.sh:125`, `bin/tests/_mordida.sh:140`, `bin/tests/_mordida.sh:187`) |
| `SELO_EXC_LOG/DIR`, `SELO_ESC_LOG`, `SELO_PROC_LOG`, `MORDIDA_MARCAS`, `PYTHONPATH`, `PYTHONDONTWRITEBYTECODE` | `_mordida_sondado` fornece o ambiente ao caso. | Startup Python, sonda e coletores. `MORDIDA_SEQ` é exportado pelo trap e retirado ao terminar. (`bin/tests/_mordida.sh:41`, `bin/tests/_mordida.sh:126`, `bin/tests/_mordida.sh:142`, `bin/tests/_mordida.sh:145`, `bin/tests/test_disjuntor.sh:776`) |
| `HOOK_STOP_RAIZ`, `CODEX_SESSIONS_DIR`, `PADRAO_DE_CIMA_BIN` | Selos atribuem os dois primeiros aos subprocessos; o último pode selecionar o código do RED. | Inicialização das raízes, leitor de sessões e seleção de `BIN`. (`bin/tests/test_padrao_de_cima.sh:90`, `bin/tests/test_disjuntor.sh:523`, `bin/tests/test_padrao_de_cima.sh:634`, `bin/disjuntor.py:67`, `bin/hook_stop_fila1.py:30`) |
| SIGTERM, `atexit`, `sys.excepthook`, monitoramento | `sitecustomize` registra callbacks; instala SIGTERM somente se encontrar default; substitui `sys.excepthook`, guardando o anterior. O handler restaura default e reenvia SIGTERM. | Runtime Python chama esses callbacks. A sonda também escreve `_selo_visto/_selo_assin` nas exceções, lidos pelo monitor e hook. (`bin/tests/_mordida.sh:52`, `bin/tests/_mordida.sh:58`, `bin/tests/_mordida.sh:60`, `bin/tests/_mordida.sh:82`, `bin/tests/_mordida.sh:103`, `bin/tests/_mordida.sh:108`, `bin/tests/_mordida.sh:112`, `bin/tests/_mordida.sh:120`) |
| `meta.out`, `falhas` | Cada meta sobrescreve a saída; `_falha` incrementa o contador; o meta restaura o contador quando reconhece sua falha esperada. | Meta exige exatamente uma falha e o trecho esperado; conclusão do selo consulta `falhas`. (`bin/tests/_mordida.sh:273`, `bin/tests/_mordida.sh:274`, `bin/tests/test_disjuntor.sh:42`, `bin/tests/test_padrao_de_cima.sh:825`) |

**2. As sete classes**

| Classe | Resultado |
|---|---|
| **1. Segundo juiz registrado** | **Não identificado no diff.** As portas PDC continuam delegando a `estado_pdc/decide_pdc`; o vocabulário continua delegado a `padrao_vocab`. JUIZES declara que o registro vivo é `core/juizes.py`. (`bin/disjuntor.py:1509`, `bin/disjuntor.py:1552`, `bin/disjuntor.py:1565`, `bin/disjuntor.py:1344`, `app/docs/JUIZES.md:10`) |
| **2. Segundo escritor fora da porta** | **Nenhum novo identificado no instrumento.** Existe dívida anterior no RELATO: escritores diretos não compartilham a trava de `no_relato`. **Fila**, porque o diff não cria esses escritores. (`bin/disjuntor.py:236`, `bin/gerar_cortes.py:136`, `bin/deploy_agendado.sh:44`, `app/core/esteira_vigia.py:839`) |
| **3. Relógio fora da borda** | **O diff não acrescenta leitura de relógio.** `hist` usa o `agora` da decisão. Permanecem leituras internas anteriores em `decide_pdc` e `trilha`; eventual centralização é **fila**, sem regressão temporal nova demonstrada. (`bin/disjuntor.py:1448`, `bin/disjuntor.py:1364`, `bin/disjuntor.py:140`) |
| **4. Derivação paralela da fonte única** | **Incide no efeito:** o escopo derivado pode ficar antigo e virar prova independente de liberdade quando o estado não se lê. **Bloqueia**, pelo caminho descrito abaixo. (`bin/disjuntor.py:1488`, `bin/disjuntor.py:1496`, `bin/disjuntor.py:1530`) |
| **5. Selo/RED sem mordida** | **Sim.** Há mortes indevidas aceitas como mordida e falta prova específica da migração de `res=2`. **Bloqueia.** (`bin/tests/_mordida.sh:219`, `bin/tests/_mordida.sh:260`, `bin/tests/test_padrao_de_cima.sh:554`, `bin/tests/test_padrao_de_cima.sh:559`) |
| **6. Ordem de trava** | **Inversão nova não demonstrada.** O escopo continua gravado sob a trava do estado. Há, porém, nova possibilidade de leitura bloqueante enquanto essa trava está tomada: **bloqueia por espera**, não por inversão comprovada. (`bin/disjuntor.py:293`, `bin/disjuntor.py:1456`, `bin/disjuntor.py:1247`) |
| **7. Pouso antes do pré-requisito** | **[não verificado]**. O dado não prova sequência de suite, leitura final e pouso destes commits. A exigência está escrita, mas não substitui evidência de execução. (`CLAUDE.md:549`, `CLAUDE.md:553`) | [nao verificado]

**3. Como o harness decide “mordeu”, e onde aceita prova errada**

A decisão implementada exige: rc não zero; último comando aceito como assert ou marca do coletor com sequência correspondente; três arquivos de base existentes; nenhuma quantidade adicional de processos sem fechamento; nenhuma exceção escapada adicional, descontadas as declaradas; e, havendo declaração, trecho único, existente no original, ausente dos textos novos e observado em função/linha correspondentes. (`bin/tests/_mordida.sh:188`, `bin/tests/_mordida.sh:190`, `bin/tests/_mordida.sh:196`, `bin/tests/_mordida.sh:225`, `bin/tests/_mordida.sh:242`, `bin/tests/_mordida.sh:248`, `bin/tests/_mordida.sh:252`, `bin/tests/_mordida.sh:257`, `bin/tests/_mordida.sh:260`)

**Bloqueia — um mutante pode ser aceito sem o módulo sequer executar.** Caminho concreto:

1. Cadastrar mutante que troque `PDC_MINIMO = 2 ` por texto sintaticamente inválido, como `PDC_MINIMO = (`. O mutador verifica âncora, mas não compila o resultado. (`bin/disjuntor.py:1100`, `bin/tests/_mordida.sh:164`, `bin/tests/_mordida.sh:167`)
2. A compilação falha antes de executar frame da cópia. A sonda só atribui assinatura a exceções observadas em frames da cópia; o hook não registra exceção sem essa assinatura. (`bin/tests/_mordida.sh:75`, `bin/tests/_mordida.sh:79`, `bin/tests/_mordida.sh:115`)
3. `_pp` retorna erro e `i14_mesma` executa `return 1`, aceito como assert. (`bin/tests/test_padrao_de_cima.sh:90`, `bin/tests/test_padrao_de_cima.sh:116`, `bin/tests/_mordida.sh:190`)
4. Com `FIM`, `.esc` vazio e declaração vazia, o comparador não exige outra prova e aceita a mordida. O caso detectou falha de compilação, não a retirada da costura pretendida. (`bin/tests/_mordida.sh:49`, `bin/tests/_mordida.sh:219`, `bin/tests/_mordida.sh:234`, `bin/tests/_mordida.sh:260`, `bin/tests/_mordida.sh:265`)

**Bloqueia — `SystemExit` anterior à decisão também passa.** Inserir `raise SystemExit(1)` antes de `d = decide_pdc(...)` faz o mesmo caso executar `return 1`. `atexit` pode registrar `FIM`, enquanto a classificação de escape depende exclusivamente de `sys.excepthook`; não há verificação de saída antecipada em `.exc` quando não existe declaração. (`bin/disjuntor.py:1508`, `bin/disjuntor.py:1509`, `bin/tests/test_padrao_de_cima.sh:116`, `bin/tests/_mordida.sh:49`, `bin/tests/_mordida.sh:114`, `bin/tests/_mordida.sh:234`)

**Bloqueia — qualquer SIGTERM vira “prazo”.** Inserir `os.kill(os.getpid(), 15)` antes da decisão produz `SINAL`; `_pp` falha e o caso retorna `1`. `abertos` exclui esse processo dos sem fechamento, e o aumento de SIGTERM produz somente uma NOTA. Não exige que tenha havido `timeout`, nem confere a origem do sinal. (`bin/disjuntor.py:1509`, `bin/tests/_mordida.sh:55`, `bin/tests/test_padrao_de_cima.sh:90`, `bin/tests/test_padrao_de_cima.sh:116`, `bin/tests/_mordida.sh:219`, `bin/tests/_mordida.sh:228`, `bin/tests/_mordida.sh:265`)

**Bloqueia — a cura do legado `res=2` não tem mordida própria.** A versão `3` existe expressamente para reparar eventos vazios classificados pelo resolvedor `2`; porém, a fixture remove `res`, e a asserção passou a aceitar qualquer versão numérica. Reverter somente a constante para `2` mantém esse teste de legado passando, enquanto eventos reais `entidades=[]/res=2` voltam a ser pulados pela igualdade da versão. (`bin/disjuntor.py:1102`, `bin/disjuntor.py:1387`, `bin/tests/test_padrao_de_cima.sh:554`, `bin/tests/test_padrao_de_cima.sh:559`)

As alterações específicas de **P17** e **`escopo-fail-open`** têm caminhos coerentes de detecção: P17 exige stdout vazio **e nenhuma FALHA nova**, chegando ao `return 35`; OBRA-Z, ausente do escopo e presente apenas no RELATO novo, chega ao `return 6` se a guarda for retirada. Isso não elimina os contraexemplos do harness acima. (`bin/tests/test_disjuntor.sh:100`, `bin/tests/test_disjuntor.sh:103`, `bin/tests/test_disjuntor.sh:583`, `bin/codex_limite_estado.py:156`, `bin/disjuntor.py:2076`, `bin/tests/test_padrao_de_cima.sh:500`, `bin/tests/test_padrao_de_cima.sh:503`, `bin/tests/test_padrao_de_cima.sh:709`)

**4. Outros achados, com classificação**

- **Bloqueia — escopo antigo libera histórico que não conhece.** Existe escopo sem OBRA-A → a decisão registra evento de OBRA-A e salva o estado → salvar o escopo falha, conservando o antigo → a linha sai do RELATO → o estado fica ilegível → fallback aceita escopo antigo mais RELATO neutro e retorna `LIVRE`. O novo histórico só protege quando sua gravação funciona; não existe vínculo de geração conferido entre os dois arquivos. (`bin/disjuntor.py:1397`, `bin/disjuntor.py:281`, `bin/disjuntor.py:1456`, `bin/disjuntor.py:1488`, `bin/disjuntor.py:1496`, `bin/disjuntor.py:1523`, `bin/disjuntor.py:1530`)

- **Bloqueia — FIFO/dispositivo pode prender a decisão sob `flock`.** `_existe` aceita qualquer objeto existente, mas os leitores seguintes usam `open().read()` comum. FIFO sem escritor em `models.py` ou no arquivo citado não levanta imediatamente: pode esperar mantendo a trava do estado. O wrapper chama as guardas antes do `timeout` que envolve o Codex. (`bin/disjuntor.py:1230`, `bin/disjuntor.py:1247`, `bin/disjuntor.py:1273`, `bin/disjuntor.py:275`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:168`, `bin/codex_pergunta.sh:177`)

- **Bloqueia — payload cru ignora a chave DESLIGADO.** Com a chave presente, `["Agent", "construir a obra OBRA-C agora"]` levanta antes da consulta à chave e chega à negativa do envelope. Contraria “existindo, nenhum disjuntor age”; o contrato autoriza a chave de desligar. (`bin/disjuntor.py:40`, `bin/disjuntor.py:2044`, `bin/disjuntor.py:2048`, `bin/disjuntor.py:2067`, `bin/disjuntor.py:2074`, `app/docs/CORTES.md:407`)

- **Fila — classificação do cru continua incompleta.** `{"tool_name":"Agent","tool_input":["construir a obra OBRA-C agora"]}` passa pelo teste de objeto externo, falha nos `.get` internos e também faz `_ramo_pdc` devolver `None`, terminando liberado. Uma lista com `"\u0041gent"` igualmente escapa da busca literal por `Agent`. A ocorrência desses formatos no produtor real é **[não verificado]**. (`bin/disjuntor.py:2043`, `bin/disjuntor.py:2005`, `bin/disjuntor.py:1584`, `bin/disjuntor.py:1588`, `bin/disjuntor.py:1590`, `bin/disjuntor.py:2076`)

- **Fila — raiz do RELATO pode divergir da raiz do estado.** `decide_pdc(raiz=X)` usa X para estado/modelos/escopo, mas `_relato_consistente()` usa `hook_stop_fila1.RELATO`, fixado no import pelo ambiente ou localização do módulo. Processo reutilizado com outra raiz pode cruzar fontes; as invocações CLI dos selos evitam isso ao fornecer `HOOK_STOP_RAIZ`. (`bin/disjuntor.py:1365`, `bin/disjuntor.py:1371`, `bin/disjuntor.py:707`, `bin/disjuntor.py:1523`, `bin/hook_stop_fila1.py:30`, `bin/hook_stop_fila1.py:34`, `bin/tests/test_padrao_de_cima.sh:90`)

- **Fila — temporário órfão do escopo.** Depois de `mkstemp`, falhas de escrita ou substituição só geram trilha; falta limpeza do temporário. O escritor do estado já possui essa limpeza. (`bin/disjuntor.py:1484`, `bin/disjuntor.py:1488`, `bin/disjuntor.py:284`)

- **Fila — vermelho por privilégio do executor.** `resolvedor_fonte` presume que `chmod 000` retire acesso. Executor com privilégio suficiente pode continuar lendo e reprovar o controle por ambiente, sem relação com mutante. (`bin/tests/test_padrao_de_cima.sh:534`, `bin/tests/test_padrao_de_cima.sh:537`, `bin/tests/test_padrao_de_cima.sh:570`, `bin/tests/test_padrao_de_cima.sh:573`)

- **Fila — instalação da sonda não tem confirmação positiva.** Depende de `sys.monitoring` e disponibilidade do identificador `4`; stderr é descartado e o controle procura ausência de erros, sem exigir confirmação de monitoramento instalado. Os metas podem denunciar a falha, portanto isso sozinho não prova verde global. (`bin/tests/_mordida.sh:64`, `bin/tests/_mordida.sh:108`, `bin/tests/_mordida.sh:143`, `bin/tests/_mordida.sh:176`, `bin/tests/test_padrao_de_cima.sh:755`)

- **Fila — bases não certific

[truncado em 20000 de 21845 caracteres]

## 31/10 20:57 · sessao b2d04db14ac0 · V3-P5-PRORROGACAO-POR-INTERVALO · verificador · origem=Code

**A cura de 1914 procede, mas o corpo ainda não sustenta o aval geral: o piso introduz uma regressão com segundos, mesmo sem troca de offset.** O `max` aplica um minuto mínimo também quando a próxima hora já é posterior a `a`; isso pode ultrapassar a borda e transformar um trecho parcialmente noturno em inteiramente noturno. (`app/ponto/motor_calculo_v2.py:183–187`)

1. **Contraexemplo e limites**

Reproduzi em memória pelas funções reais da cópia, com `America/Sao_Paulo`, configurado em `app/config/settings/base.py:93`:

| Parâmetro | Valor |
|---|---|
| Entrada | 14/10/2026 22:00 |
| Saída | 15/10/2026 07:00 |
| Intervalo | 15/10/2026 04:59:30 → 05:01:00 |
| Prorrogação | `False` |
| Próxima hora localizada | 05:00:00 |
| Piso | **05:00:30**, escolhido como `b` |
| Desconto atual | **1 minuto** |
| Janela efetiva | **0,5 minuto** |
| Corpo anterior, representado pelo mutante | **0,5 minuto** |

O cursor escolhe `05:00:30`; entre `a` e `b`, o prefixo aumenta apenas 0,5 minuto, mas o corpo soma a duração inteira de 1 minuto. O restante, até `05:01`, não aumenta o prefixo. A borda continua sendo 05:00 na função consultada. (`app/ponto/motor_calculo_v2.py:101`, `:184–188`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:426–431`)

Também reproduzi o erro com as mesmas pontas fornecidas como **naive**, **UTC** e **Asia/Tokyo**: desconto de 1 minuto. As representações convergem pelo `localizar`; portanto, outro fuso de entrada não elimina o defeito. (`app/ponto/motor_calculo_v2.py:77–79`, `:162–164`, `:184–187`)

Há contraprova equivalente na entrada da janela: recorte começando em `21:59:30`, pausa `21:59:30–22:01`, sem prorrogação, produz **1,5 minuto**, contra **1 minuto** da janela. (`app/ponto/motor_calculo_v2.py:103`, `:184–187`)

Quanto às outras propriedades:

- **`b <= a`: NÃO procede**, considerando operações sem exceção. Dentro do laço, `_f > a`; o outro argumento do `min` é pelo menos `a + 1 minuto`. Ambos são posteriores a `a`, e a atualização faz progresso. (`app/ponto/motor_calculo_v2.py:179`, `:184`, `:188`)
- **Desconto negativo ou soma devolvida maior que `minutos_noturnos(entrada, saida)`: NÃO procede.** Só são somadas durações positivas, e o retorno aplica o teto do total noturno. (`app/ponto/motor_calculo_v2.py:176`, `:185–190`)
- **Excesso material sobre a duração da união: NÃO procede.** As peças são unidas e cada avanço é contado no máximo uma vez; isso não garante correspondência com a janela, como demonstra o contraexemplo. (`app/ponto/motor_calculo_v2.py:164–172`, `:177–188`)
- **Desigualdade literal em `float`: exige ressalva.** Reproduzi novamente `60.41666666666667` contra duração direta `60.416666666666664`, diferença de aproximadamente `7,1e-15` minuto, abaixo da tolerância dos testes. (`app/ponto/motor_calculo_v2.py:187`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:119–125`, `:185`)

Nos casos sondados, o histórico de 1914 passou a **354 minutos**, conforme a janela; os dois casos novos de verão deram **270** e **450**. Também sondei o início ambíguo `17/02/2018 23:30` nas duas ocorrências: **330** e **390**, ambos iguais à diferença dos prefixos. Isso confirma esses casos, sem certificar universalmente todas as transições. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:396–405`; `app/ponto/motor_calculo_v2.py:79`, `:183–190`)

2. **O piso pode ser escolhido?**

**Sim; a alegação de que ele só impede localização sem avanço NÃO procede.** No contraexemplo, `05:00` já avança 30 segundos, mas perde para o piso `05:00:30`, que atravessa a borda. O erro é de **30 segundos**, não arredondamento residual. (`app/ponto/motor_calculo_v2.py:181–187`)

O piso também é escolhido com uma hora inexistente fornecida como naive: pausa `04/11/2018 00:00–06:00`. `localizar` aplica `localize` e retorna diretamente; nessa sonda o cursor avançou em **300 passos de um minuto**, mantendo o offset carregado por `a`. O resultado foi **240 minutos**, igual à janela nesse caso. Portanto, uso do piso não implica necessariamente erro, mas sua inocuidade geral NÃO procede. (`app/ponto/motor_calculo_v2.py:77–79`, `:183–188`)

3. **Identidade com e4ed207c e chamadores**

**Identidade em qualquer recorte normal sem troca de offset: NÃO procede.** O contraexemplo de outubro de 2026 já distingue os corpos: **1 contra 0,5 minuto**. Com offset constante e pontas alinhadas a minutos inteiros, o piso não ultrapassa a próxima hora, e o cursor preserva os cortes anteriores. (`app/ponto/motor_calculo_v2.py:183–184`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:426`)

Os chamadores continuam usando o mesmo caminho; seus resultados podem mudar pelo desconto recebido:

| Leitor | Uso do desconto |
|---|---|
| `he_noturna` | Subtrai no recorte ancorado em `inicio_extra`. (`app/ponto/motor_calculo_v2.py:434–437`) |
| `_noturno_dos_intervalos` / `calcular_noturnas` | Delega à função e subtrai do total. (`app/ponto/motor_calculo_v2.py:1909–1910`, `:965–969`) |
| `_ContextoNoturno` / noturnas do dia | Empresta o mesmo método e subtrai por par. (`app/ponto/calculador/regras.py:63`, `:85–89`) |
| HE por período | Passa pontas, intervalos e flag a `he_noturna`. (`app/ponto/calculador/regras.py:886–890`) |
| `noturno_no_par` | Encaminha um par e os parâmetros da CCT; o retorno alimenta os campos do período. (`app/ponto/calculador/periodo.py:508–511`, `:752–754`) |

Impacto produtivo por colaborador ou rubrica: **[nao verificado]**.

4. **Testes novos e mordida** [nao verificado]

**Mordem o defeito histórico: SIM.** Os três métodos novos passaram na execução isolada em memória; o caso de 1914 produziu **354** no corpo atual e **360** no mutante. O `mock.patch` troca o símbolo efetivamente consultado e chama diretamente o teste dentro de `assertRaises(AssertionError)`. Vacuidade dessa mordida NÃO procede. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:396–398`, `:436–441`)

**Valor copiado: NÃO procede.** A expectativa calcula duração absoluta até a borda local das 05h. É adequada aos três casos escolhidos, todos iniciados dentro da janela e todos com `pr=False`. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:392`, `:398`, `:403`, `:405`)

**Proteção geral contra o piso: NÃO procede.** Os **24 testes puros** executados por AST passaram, incluindo os novos, enquanto o contraexemplo com segundos falhou na mesma abordagem. O teste existente de segundos cobre pausa inteiramente prorrogada; a grade usa minutos inteiros, e seu oráculo trunca frações de minuto. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:119–125`, `:149`, `:153–154`, `:200–207`)

**Relógio solto: NÃO procede.** `_local` constrói datas literais e aplica o fuso, sem consultar “agora”. Suíte Django canônica e testes de integração dos chamadores: **[nao verificado]**. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:377–405`)

5. **Classes reprovadas e escritores/leitores**

**Segundo juiz, segundo escritor fora da porta e derivação paralela da janela: NÃO procedem neste delta.** A decisão continua consultando os prefixos da mesma função; as atribuições novas são locais. **Selo sem mordida também NÃO procede para 1914**; existe uma lacuna distinta de cobertura do piso com segundos. (`app/ponto/motor_calculo_v2.py:183–190`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:439–441`)

| Valor | Quem escreve/fornece | Quem lê |
|---|---|---|
| Fuso | `settings.TIME_ZONE`, depois `TZ_LOCAL`. (`app/config/settings/base.py:93`; `app/ponto/motor_calculo_v2.py:28`) | Conversão, janela, cursor e fixtures. (`app/ponto/motor_calculo_v2.py:78–79`, `:100–104`, `:184`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:378–379`) |
| Pontas e intervalos | Chamadores fornecem os argumentos; o motor carimba os efetivos. (`app/ponto/motor_calculo_v2.py:436–437`, `:1822–1824`, `:1909–1910`) | Conversão/recorte e leitura por período. (`app/ponto/motor_calculo_v2.py:162–164`; `app/ponto/calculador/periodo.py:752`) |
| Flag de prorrogação | Atributo do motor, contexto e CCT. (`app/ponto/motor_calculo_v2.py:752–756`; `app/ponto/calculador/regras.py:61`; `app/ponto/calculador/nucleo.py:348`) | Prefixos, teto e delegações. (`app/ponto/motor_calculo_v2.py:185–190`, `:1910`; `app/ponto/calculador/periodo.py:509`) |
| `_ent`, `_sai`, `pecas`, `unidas` | Conversão, recorte e união. (`app/ponto/motor_calculo_v2.py:162–172`) | Laço, prefixos e teto. (`app/ponto/motor_calculo_v2.py:177–190`) |
| `a`, `hora_cheia`, `b` | Inicialização/avanço de `a`, construção da hora e escolha de `b`. (`app/ponto/motor_calculo_v2.py:178`, `:183–184`, `:188`) | Guarda, localização, prefixos, duração e próximo avanço. (`app/ponto/motor_calculo_v2.py:179–188`) |
| Prefixos e desconto | `minutos_noturnos` acumula/devolve os prefixos; o corpo acumula e limita o desconto. (`app/ponto/motor_calculo_v2.py:96`, `:116–118`, `:176–190`) | Chamadores da tabela anterior. (`app/ponto/motor_calculo_v2.py:436`, `:967`, `:1909`; `app/ponto/calculador/regras.py:86`) |
| Resultados noturnos | Escritores existentes do motor, rubricas e período. (`app/ponto/motor_calculo_v2.py:986–987`; `app/ponto/calculador/regras.py:896–898`, `:912–914`; `app/ponto/calculador/periodo.py:753–754`) | Alteração desses destinos ou impacto persistido por esta obra: **[nao verificado]**. |
| Fixtures, `desc`, `esperado` e substituição do mutante | Literais/helper, chamada real, duração absoluta e `mock.patch`. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:377–405`, `:439`) | Asserções e chamada direta sob o mutante. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:393–394`, `:440–441`) |

Ordem de trava, execução histórica do RED, pré-requisitos de pouso e pouso efetivo: **[nao verificado]**. [nao verificado]

## 31/10 20:56 · sessao 07231a507e95 · EMIS-LEITORES-RESTANTES · censo · origem=Code

O ponto central é: **`trabalha=None` não dispensa cobrança por si só**. O juiz também considera `ata.tipo_dia`; portanto, migrar apenas comparações de `trabalha` pode mudar tanto o limbo quanto dias com ata de outro tipo. A distinção entre cobrança e previsão está expressa no código e na tabela de autoridades. (`app/chamados/juizes.py:398`, `app/chamados/juizes.py:408`, `app/docs/JUIZES.md:55`, `app/docs/JUIZES.md:74`)

O anexo fornecido contém a tabela de autoridades, sem linhas de células, chamados ou perguntas. Quantidades afetadas, ocorrência das combinações abaixo e impacto atual em produção: **[nao verificado]**.

**1. O que o juiz efetivamente responde**

O predicado inteiro é:

```python
ata = cel.ata or {}
return cel.trabalha is False or bool(
    ata.get('tipo_dia') and ata.get('tipo_dia') != 'trabalho'
)
```

Fonte: `app/chamados/juizes.py:407`, `app/chamados/juizes.py:408`.

Na matriz, `True` significa **“dia sem trabalho”**. Todos os resultados decorrem desse predicado. (`app/chamados/juizes.py:408`)

| `cel.trabalha` | Ata inexistente, `{}`, chave ausente, `None` ou `''` | `tipo_dia='trabalho'` | `'folga'` | `'ausencia'`, `'ausente'`, `'falta'`, `'feriado'` ou qualquer outro texto preenchido |
|---|---:|---:|---:|---:|
| `True` | `False` | `False` | `True` | `True` |
| `False` | `True` | `True` | `True` | `True` |
| `None` | `False` | `False` | `True` | `True` |

O juiz não valida uma lista de tipos: qualquer valor verdadeiro diferente de `'trabalho'` satisfaz a segunda cláusula, inclusive o literal `'ausente'`. (`app/chamados/juizes.py:408`)

Feriado e ausência **não são consultados diretamente** nessa função. Só influenciam sua resposta se estiverem representados nos dois campos lidos. A supressão por feriado pertence à precedência; cobertura de ausência tem autoridade própria, chamada separadamente por `_celula_furada`. (`app/chamados/juizes.py:407`, `app/chamados/juizes.py:426`, `app/docs/JUIZES.md:47`, `app/docs/JUIZES.md:63`)

`cel=None` não é uma entrada tratada pelo juiz: a função acessa `cel.ata`. O consumidor `_celula_furada` trata a inexistência da célula antes de chamá-lo. (`app/chamados/juizes.py:407`, `app/chamados/juizes.py:437`, `app/chamados/juizes.py:443`)

Há duas alimentações distintas, relevantes para todo o censo:

| Alimentação | Comportamento |
|---|---|
| `EscalaColaborador.eh_dia_trabalho` | Havendo célula, devolve seu `trabalha` cru, sem ler ata; sem célula, usa cálculo do vínculo. (`app/escala/models.py:1066`, `app/escala/models.py:1072`, `app/escala/models.py:1073`) |
| `grade_da_celula` | Prefere `ata.tipo_dia`; sem tipo preenchido, transforma `True/False/None` em `trabalho/folga/indefinido`. (`app/escala/services/leitor_celula.py:345`) |
| Builder da grade | Pode representar previsão `None` como `tipo_dia='trabalho'` com `sem_ancora=True`; não é equivalente ao leitor lavrado. (`app/escala/utils.py:725`, `app/escala/utils.py:729`, `app/escala/utils.py:752`) |

**2. A — mesma pergunta: migrar a cláusula de dia**

A classificação abaixo alcança a cláusula que permite ou cala cobrança; os demais requisitos — vínculo, marcos, prazo, veredito, cobertura, horizonte — continuam sendo perguntas próprias. Isso é visível, por exemplo, no emissor retroativo e nos dois caminhos de emissão do cartório. (`app/ponto/management/commands/emitir_furo_retroativo.py:118`, `app/ponto/management/commands/emitir_furo_retroativo.py:130`, `app/ponto/services/cartorio.py:1495`, `app/ponto/services/cartorio.py:1608`)

Os chamadores abaixo são referências **no código de produção**. Sua execução no ambiente real: **[nao verificado]**.

| Sítio e predicado literal | Pergunta efetiva; `None` versus juiz; mudança ao migrar | Chamadores de produção |
|---|---|---|
| `app/ponto/management/commands/emitir_furo_retroativo.py:127` — `ec.eh_dia_trabalho(data) is not True` | Cala emissão de furo. `None` é descartado; o juiz permite prosseguir sem ata de outro tipo. Também muda `True` com ata não trabalho: hoje passa, juiz cala. Passar pelo gate ainda exige horário e demais condições. (`:127`, `:130`, `:142`; `app/chamados/juizes.py:408`) | `Command.handle`; comando declarado “passe retroativo sob ordem”. (`app/config/crons.py:1339`) |
| `app/ponto/management/commands/processar_alertas_avancados.py:49` — `not ec.eh_dia_trabalho(ontem)` | Cala o caminho de alerta/cobrança do turno aberto. `None` cala; juiz não cala sem ata de outro tipo. `True` com ata não trabalho passa hoje e seria barrado. (`:49`, `:82`, `:121`; `app/chamados/juizes.py:408`) | `Command.handle:19`; cron declarado em `app/config/crons.py:344`. |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:76` — `filter(..., trabalha=True)` | Pré-seleciona células para emitir perguntas. Exclui `None`; juiz permitiria `None` sem ata não trabalho. Inclui `True` com ata não trabalho, que o juiz excluiria. Veredito e motivos continuam filtrando. (`:76`, `:111`, `:120`, `:123`) | `Command.handle:57`; encadeamento após apuração em `app/config/crons.py:1464`. |
| `app/ponto/services/vigia_de_hora.py:21` — `filter(..., trabalha=True)` | Pré-seleciona células que chegarão ao julgamento/emissão. `None` nunca chega; juiz permitiria sua avaliação sem ata não trabalho. `True` com ata não trabalho sairia da seleção. Isso não garante emissão: ainda há vencimento de marco e outros gates. (`:21`, `:113`, `:122`, `:130`) | `_candidatas` → `rodar:113`; comando `app/ponto/management/commands/vigia_de_hora.py:28`; cron `app/config/crons.py:348`. |
| `app/chamados/services/declaracao_texto.py:95` — `tipo_dia != 'trabalho'` | Descarta dia como “sem marco a cobrar”. Pelo leitor lavrado, `None` sem tipo vira `indefinido` e é descartado; juiz não dispensa. `False` com ata `trabalho` passa hoje, mas juiz barra. `True/None` com ata não trabalho continuam barrados. (`:90`, `:95`, `:96`; `app/escala/services/leitor_celula.py:345`) | `_complementar_marcos_faltantes`: `app/chamados/services/disputa_emissao.py:794`, emissão em `:813`; `_ata_do_dia`: `:827`. Também há consumidores de apresentação em `declaracao_texto.py:285`, `:411`, `:524`, `:563`. |
| `app/ponto/services/cartorio.py:1504` — `ec is not None and ec.eh_dia_trabalho(data) is True` | Elegibilidade de emissão do furo total. `None` cala; juiz não cala sem ata de outro tipo. `True` com ata não trabalho seria barrado. Vínculo e marco inicial permanecem requisitos separados. (`:1503`, `:1504`, `:1507`, `:1514`) | Corpo de `julgar_colab`: `:777`, `:802`; lote `app/ponto/management/commands/processar_cartorio.py:65`; evento `cartorio.py:1723`. |
| `app/ponto/services/cartorio.py:1608` — `ec is None or ec.eh_dia_trabalho(data) is not True` | Faz `_marcos_do_furo_parcial` devolver `None`, impedindo a emissão parcial. Mesmas diferenças para `None` e `True` com ata não trabalho. O restante identifica os marcos apagados e sua âncora. (`:1608`, `:1611`, `:1624`, `:1635`) | Chamado em `cartorio.py:1512`, `:1543`; emissão em `:1550`. Mesmas entradas lote/evento da linha anterior. |
| `app/ponto/marcos.py:159` — `trabalha is False` | Cláusula `'FOLGA'` de `celula_veta_cobranca`. `None` não veta, igual ao juiz sem ata não trabalho. Como não lê ata, migrar acrescenta veto para `True/None` com ata não trabalho; outros vetos de veredito permanecem. (`:155`, `:159`, `:161`) | `detectar_ausencias.py:626`; `reconciliar_grade.py:502`; `app/chamados/reconciliador.py:842`; contador `app/ponto/services/furos_sem_cobranca.py:98`. |
| `app/escala/servico_jornada.py:303` — `_cel_dia is not None and _cel_dia.trabalha is False` | Primeira cláusula de dispensa dentro de `classificar_falta`. `None` não dispensa, igual ao juiz sem ata de outro tipo. Migrar acrescenta a cláusula da ata para `True/None`; não substitui o juiz inteiro de “qual furo existe?”. (`:279`, `:303`; `app/docs/JUIZES.md:81`) | Fábrica `app/chamados/services/fabrica_da_celula.py:56`; disputa `app/chamados/services/disputa_emissao.py:739`, `:1003`, `:1036`; `motivo_da_lei` em `servico_jornada.py:557`; retratação em `app/ponto/marcos.py:170`. |
| `app/ponto/management/commands/detectar_ausencias.py:305` — `not resultado_dia`, obtido em `:143` | Cala o caminho de cobrança por dia não previsto. **`None` já foi desviado antes**, pelo tratamento de limbo em `:159` e pelo `continue` em `:304`; trocar só `:305` não o faz chegar à cobrança. Para `True` que chega ao ramo, passar a ler ata não trabalho muda o gate. (`:143`, `:159`, `:304`, `:305`, `:439`) | `Command.handle:61`; cron `app/config/crons.py:345`. |
| `app/ponto/services/faltas_de_hoje.py:40` — `not (ec.eh_dia_trabalho(hoje) is True or limbo)` | Gate do contador de falta atual sem cobrança. `None` só passa se `limbo` for verdadeiro; juiz não dispensa `None` sem ata não trabalho independentemente desse defeito específico. Migrar pode mudar a população contada; não abre chamado diretamente. (`:13`, `:39`, `:40`, `:69`) | `app/ponto/management/commands/faltas_de_hoje_sem_cobranca.py:16`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:376` — `c.veredito in acusam or not c.trabalha or k in com_batida` | Na contraprova do piso, a cláusula `not c.trabalha` dispensa também `None`; juiz não dispensa sem ata de outro tipo. Migrar **só essa cláusula** muda a auditoria quando os outros termos não bastam para excluir. (`:305`, `:308`, `:376`) | `contraprova_piso` chamada em `:760`; cron auditor `app/config/crons.py:523`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1443` — `not _cel4.trabalha or _cel4.veredito not in _ACUSAM_a4` | Decide se a célula sustenta cobrança/reabertura. `None` é dispensado mesmo com veredito acusador; juiz sem ata não trabalho não o dispensa. Outros gates, inclusive ausência de julgamento, permanecem. (`:1433`, `:1434`, `:1443`) | Corpo de `Command.handle:552`; cron `app/config/crons.py:523`. |

**Migrar esses gates não é uma alteração neutra.** Além do limbo, existe a entrada `ata.tipo_dia='ausencia'` com cobertura parcial: a grade conserva esse tipo e os marcos, o suprajuiz admite cobrança parcial e `classificar_falta` só dispensa ausência que não seja parcial. O juiz `dia_sem_trabalho`, por sua vez, dispensa qualquer ata preenchida diferente de `trabalho`. Essa diferença precisa aparecer no eventual diff de comportamento; sua ocorrência no dado atual é **[nao verificado]**. (`app/escala/utils.py:1180`, `app/escala/utils.py:1190`, `app/ponto/supra_juiz.py:304`, `app/escala/servico_jornada.py:344`, `app/chamados/juizes.py:408`)

No cartório, os números antigos fornecidos caem hoje em comentários; os gates efetivos correspondentes estão em `:1504` e `:1608`. (`app/ponto/services/cartorio.py:1459`, `app/ponto/services/cartorio.py:1469`, `app/ponto/services/cartorio.py:1558`, `app/ponto/services/cartorio.py:1574`)

**3. B — pergunta diferente: não migrar ao juiz de cobrança**

A tabela de autoridades distingue previsão, precedência, minutos previstos, furo e status lavrado. Uma diferença para `None` nesses domínios não autoriza substituí-los por `dia_sem_trabalho`. (`app/docs/JUIZES.md:74`, `app/docs/JUIZES.md:76`, `app/docs/JUIZES.md:80`, `app/docs/JUIZES.md:81`, `app/docs/JUIZES.md:82`)

| Sítio e predicado literal | Pergunta real; `None`; efeito de uma troca mecânica | Chamadores / autoridade pertinente |
|---|---|---|
| `app/ponto/services/flip_auto.py:208` — `cel is not None and cel.trabalha is False`; `:212` — `v is None or v.trabalha is not True` | Escolhe dia/âncora para etiquetar E/S. `None` no dia atual não entra no ramo de folga; `None` na véspera não serve de âncora. Trocar pelo complemento do juiz permitiria véspera indeterminada sem ata contrária e poderia mudar flips. (`:165`, `:171`, `:176`, `:208`, `:212`) | `avaliar_fila:310`; comando `app/ponto/management/commands/flip_automatico.py:32`; tela `app/ponto/views.py:3295`. Autoridades de turno/marcos: `app/docs/JUIZES.md:22`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1542` — `_cel is None or _cel.trabalha is not True` | C5: célula contradiz `FolgaDia`? Só `True` prova a contradição; `None` não prova trabalho. Trocar pelo complemento do juiz acusaria limbo como contradição. (`:1530`, `:1542`, `:1549`) | `Command.handle:552`; cron `app/config/crons.py:523`. Juiz específico dessa contradição em JUIZES: **[nao verificado]**. |
| `app/ponto/services/cartorio.py:1714` — `data == hoje and cel.trabalha is not False and not teto_de_hoje` | Decide **quando aguardar o fim do turno para julgar hoje**. `None` aguarda. Usar o juiz poderia antecipar julgamento de `True/None` com ata não trabalho. (`:1714`, `:1715`, `:1719`) | Sinais `app/ponto/signals.py:93`, `app/chamados/signals.py:180`; vigia `app/ponto/services/vigia_de_hora.py:130`; reconciliador `app/chamados/reconciliador.py:675`. Juiz dessa guarda específica em JUIZES: **[nao verificado]**. |
| `app/ponto/services/dia_do_colab.py:121` — `not cel.trabalha` | Produz a frase “Dia sem trabalho previsto”. `None` recebe essa frase, embora o juiz não dispense cobrança sem ata contrária. Trocar muda apresentação, não o estado da cobrança. (`:121`, `:122`) | Chamada interna `:100`; API `app/api/views_mensageria.py:1963`; ficha `app/colaboradores/services/ficha.py:280`. Previsão/status: `app/docs/JUIZES.md:74`, `:82`. |
| `app/ponto/services/dia_do_colab.py:130` — `f.get('faixa') in (...) and cel.trabalha` | Apresenta divergência entre precedência e célula. `None` não prova essa divergência; o juiz responde outra coisa. (`:130`, `:131`, `:132`) | Mesma cadeia anterior; precedência: `app/docs/JUIZES.md:76`. |
| `app/relatorios/dia_da_frota.py:47` — `not cel.trabalha` | Conta folga e exclui do relatório de acusados. `None` conta folga; o juiz o manteria cobrável sem ata contrária. Trocar muda contagem/listagem. (`:39`, `:47`, `:48`, `:55`) | `app/api/views_mensageria.py:1814`. Previsão/status: `app/docs/JUIZES.md:74`, `:82`. |
| `app/chamados/services/fila_por_causa.py:192` — `filter(..., trabalha=True, ...)` | Seleciona **vizinhos previstos** para diagnosticar causa/adesão. `None` fica fora; usar o juiz mudaria os vizinhos e a gaveta atribuída. (`:192`, `:194`, `:224`, `:244`) | `quadro:271`, `toques:308`, `bloqueios_para_fechar:353`; API `app/api/views_mensageria.py:1582`; badge `app/core/management/commands/lavrar_badge_navbar.py:63`; comando `app/chamados/management/commands/fila_por_causa.py:13`. Previsão: `app/docs/JUIZES.md:74`. |
| `app/chamados/services/disputa_emissao.py:68` — `dia.get('data') == ref and dia.get('tipo_dia') == 'trabalho'` | Deriva uma **data confiável** quando o texto da pergunta não tem dia. `None` sem tipo na ata vira `indefinido` e não fornece data; cobrar o limbo não prova essa âncora. (`:42`, `:62`, `:68`, `:322`) | `gerar_perguntas_disputa`, chamada em `:322`; sem data, pergunta não nasce em `:332`. Juiz específico da inferência de data: **[nao verificado]**. |
| `app/chamados/services/disputa_emissao.py:753` — `_ec is not None and _ec.eh_dia_trabalho(data) is not False` | `_tem_base`: existe batida real **ou obrigação prevista** para o fallback? `None` com vínculo já passa; juiz também não dispensa sem ata contrária. Trocar acrescentaria a ata e mudaria a pergunta de base prevista. (`:742`, `:750`, `:753`, `:1024`) | Fallback de perguntas em `:1034`; classificação do furo ocorre depois, em `:1036`. Previsão/furo: `app/docs/JUIZES.md:74`, `:81`. |
| `app/chamados/detector_anomalias.py:164` — `eh_dia = esc.eh_dia_trabalho(...)`; `:165` — `eh_dia is None`; `:168` — `eh_dia is False` | Compara batida com escala: indeterminado não acusa; folga conhecida pode gerar anomalia. Trocar pelo juiz faria ata de ausência participar de “batida fora da escala”. (`:124`, `:164`, `:165`, `:168`, `:182`) | `app/api/views.py:730`; `app/api/views_core.py:727`. Previsão/turno: `app/docs/JUIZES.md:22`, `:74`. |
| `app/ponto/worklist.py:19` — `dia['tipo_dia'] != 'trabalho'` | Desenha gap pela ata, devolvendo “sem classificar”. `None` sem tipo é excluído; usar o juiz mudaria apresentação/classificação. (`:8`, `:12`, `:19`, `:20`) | Chamada interna `:60`; tela `app/colaboradores/views.py:3021`; comando `app/ponto/management/commands/worklist_dp.py:13`. Juiz específico do gap: **[nao verificado]**. |
| `app/ponto/propositor.py:36` e `:210` — `not dia or dia.get('tipo_dia') != 'trabalho'` | Diagnostica degeneração e amostra deltas para suspeita de template. O resultado para `trabalha=None` depende do produtor do dict: leitor lavrado pode dar `indefinido`; builder pode dar trabalho com `sem_ancora`. Trocar muda propostas/amostra. (`:34`, `:201`; `app/escala/services/leitor_celula.py:345`; `app/escala/utils.py:752`) | `app/ponto/management/commands/propor_reorganizacao.py:67`, `:71`; `app/inteligencia/calculadores.py:325`. Juiz específico em JUIZES: **[nao verificado]**. |
| `app/ponto/services/simular_dia.py:12` — `dd.get('tipo_dia') != 'trabalho' or dd.get('orfas')` | Decide **dia perfeito para uma correção simulada**, também exigindo ausência de missing e tipos corretos. `None` indireto depende da grade, não só do campo. Trocar altera critério de perfeição. (`:5`, `:12`, `:18`, `:21`; `app/escala/utils.py:729`) | `app/ponto/services/flip_auto.py:303`, `:304`; `app/ponto/services/veredito_celula.py:219`, `:223`. Juiz de cobrança não responde perfeição. (`simular_dia.py:11`) |
| `app/ponto/services/veredito_celula.py:221` — `dd0.get('tipo_dia') != 'trabalho'` | Há trabalho previsto para materializar a decisão administrativa “trabalhou”? A recusa diz “nada a materializar”; não é silêncio de chamado. (`:206`, `:221`, `:222`) | `app/ponto/views.py:3304`; `app/ponto/services/declarar_dia.py:206`. Previsão: `app/docs/JUIZES.md:74`. |
| `app/escala/services/furos_diarios.py:39` — `not d or d.get('tipo_dia') != 'trabalho'` | Apura `% marcos furados / previstos` e a projeção `FuroDiario`. `None` via leitor sem tipo fica fora; migrar muda dias/denominador, não emite chamado diretamente. (`:2`, `:19`, `:39`, `:98`) | `apurar:89`; comando `app/escala/management/commands/apurar_furos_diarios.py:8`; pós-vínculo `app/colaboradores/services/pos_vinculo.py:52`. Juiz específico da métrica em JUIZES: **[nao verificado]**. |
| `app/relatorios/furos.py:151` — `d.get('tipo_dia') != 'trabalho'`; `:218`, `:237` — mesmo teste `or d.get('sem_ancora')` | Exibição, dias em branco e matriz do Raio-X da folha. `None` sem tipo é rotulado folga/excluído; trocar muda relatório e contagens. (`:152`, `:210`, `:218`, `:229`, `:237`) | Dataset: `app/relatorios/views.py:1541`; matriz: `app/folha/views.py:616`. Chamador de produção de `dias_100_em_branco`: **[nao verificado]**. |
| `app/ponto/services/vigia_entrada.py:79`, `:120` — `filter(..., trabalha=True)` | Mede padrões de lâmpadas para investigar entrada perdida versus adesão. `None` fica fora; migrar muda a população estatística. (`:11`, `:63`, `:79`, `:111`, `:120`) | `contador:99`; comando `app/ponto/management/commands/vigia_entrada_faltante.py:24`, `:39`; cron `app/config/crons.py:716`. |
| `app/ponto/services/esmeril_espelho.py:71` — `x.trabalha is False ...`; `:121`, `:123` — `x.trabalha and ...` | Identifica assinaturas de escala/cadastro errado. `None` não prova folga nem trabalho conhecido; o juiz ampliaria essas assinaturas indevidamente. (`:66`, `:71`, `:117`, `:121`, `:123`) | Chamada interna `:105`; `app/escala/services/escala_certa.py:27`; comando `app/ponto/management/commands/esmeril_espelho.py:16`. |
| `app/ponto/services/passivo_s84.py:96` — `TRABALHO if cel.trabalha else FOLGA` | Classifica batidas antigas; a classe pode alimentar seleção para retratação. `None` vira `FOLGA`, diferente do juiz; migrar mudaria tratamento de batidas, não apenas cobrança. (`:62`, `:96`, `:108`) | `app/ponto/management/commands/passivo_disputa_retro.py:70`; `app/ponto/management/commands/diff_passivo_s84.py:45`. Autoridade dessa classificação em JUIZES: **[nao verificado]**. |
| `app/ponto/management/commands/tripwire_celulas.py:338` — `Q(trabalha=False) \| Q(trabalha__isnull=True)`; `:342` — `ata.get('tipo_dia') 

[truncado em 20000 de 30842 caracteres]

## 31/10 20:56 · sessao b6ad06b587d6 · - · - · origem=Ronald (tmux)

**Bloqueia: o harness ainda aceita morte antes da decisão como mordida.** A prova positiva exige apenas `return N` do caso, e a prova de processo exige apenas fechamento por `atexit` ou classificação como SIGTERM; nenhuma prova exige que o código mutado tenha concluído a decisão. (`bin/tests/_mordida.sh:190`, `bin/tests/_mordida.sh:214`, `bin/tests/_mordida.sh:225`)

Três caminhos concretos:

1. **Mutante sintaticamente inválido:** trocar `PDC_MINIMO = 2 ` por `PDC_MINIMO = (` passa pelo mutador, que apenas conta âncoras e grava o texto; a compilação de `disjuntor.py` falha antes de qualquer frame da cópia executar; o Python encerra normalmente por `atexit`; `_pp` devolve 1 e `i14_mesma` executa seu `return 1`. Sem declaração, `.exc` não precisa conter nada e `.esc` vazio passa. (`bin/disjuntor.py:1100`, `bin/tests/_mordida.sh:162`, `bin/tests/_mordida.sh:167`, `bin/tests/_mordida.sh:79`, `bin/tests/_mordida.sh:49`, `bin/tests/test_padrao_de_cima.sh:116`, `bin/tests/_mordida.sh:234`, `bin/tests/_mordida.sh:260`)

2. **`raise SystemExit(1)` antes de `decide_pdc`:** `SystemExit` deixa executar `atexit`, mas não chama `sys.excepthook`; assim há INI/FIM e nenhuma exceção escapada registrada em `.esc`. O mesmo `return 1` do caso satisfaz a prova positiva. (`bin/disjuntor.py:1508`, `bin/tests/_mordida.sh:49`, `bin/tests/_mordida.sh:114`, `bin/tests/_mordida.sh:190`, `bin/tests/test_padrao_de_cima.sh:116`; comportamento do interpretador confirmado em experimento somente em memória, sem artefato: [não verificado])

3. **Autoenvio de SIGTERM antes da decisão:** o handler anota SINAL independentemente da origem; `abertos` exclui esse processo dos sem fechamento e emite somente uma nota de “prazo”. O comparador não exige `timeout`, rc 124 ou prova de prazo, e o caso pode terminar pelo mesmo `return 1`. (`bin/tests/_mordida.sh:55`, `bin/tests/_mordida.sh:219`, `bin/tests/_mordida.sh:228`, `bin/tests/_mordida.sh:265`, `bin/tests/test_padrao_de_cima.sh:116`)

A sonda original foi extraída e executada em memória com `open` substituído por coletores em RAM: SyntaxError produziu INI/FIM e logs de exceção vazios; SystemExit produziu INI/FIM e somente `.exc`; SIGTERM produziu INI/SINAL. Nenhum arquivo foi criado pelo experimento. [não verificado]

**Mapa dos estados do harness:**

| Estado | Quem escreve | Quem lê / multiplicidade |
|---|---|---|
| `sonda_exc/sitecustomize.py` | `source` do harness gera o arquivo. | Python descendente carrega via `PYTHONPATH`; uma geração por selo. (`bin/tests/_mordida.sh:35`, `bin/tests/_mordida.sh:37`, `bin/tests/_mordida.sh:143`) |
| `<raiz>.exc` | `_mordida_sondado` trunca; callbacks RAISE de todos os Python sondados acrescentam. | Controle procura cinco tipos; comparador verifica declarações. Há múltiplos escritores. (`bin/tests/_mordida.sh:139`, `bin/tests/_mordida.sh:106`, `bin/tests/_mordida.sh:176`, `bin/tests/_mordida.sh:222`, `bin/tests/_mordida.sh:252`) |
| `<raiz>.esc` | Truncador e hooks dos Python descendentes. | Comparador de multiplicidade. Há múltiplos escritores. (`bin/tests/_mordida.sh:139`, `bin/tests/_mordida.sh:117`, `bin/tests/_mordida.sh:223`, `bin/tests/_mordida.sh:258`) |
| `<raiz>.proc` | Truncador; startup, `atexit` e handler SIGTERM de cada Python. | `abertos` conta conjuntos de PIDs. Há múltiplos escritores. (`bin/tests/_mordida.sh:139`, `bin/tests/_mordida.sh:46`, `bin/tests/_mordida.sh:50`, `bin/tests/_mordida.sh:56`, `bin/tests/_mordida.sh:214`) |
| `<raiz>.marca` | Truncador e três coletores Python; o trap novo mantém seus dados em variáveis. | `_mordida_morre` lê a última marca quando o último comando não é `return`. (`bin/tests/_mordida.sh:139`, `bin/tests/test_disjuntor.sh:776`, `bin/tests/test_disjuntor.sh:947`, `bin/tests/test_disjuntor.sh:1122`, `bin/tests/_mordida.sh:191`) |
| `base_<caso>.exc/.proc/.esc` | `_mordida_controle`, somente se cada arquivo ainda não existe. | Comparador de mutantes; reaproveitados entre raízes pelo nome do caso. (`bin/tests/_mordida.sh:179`, `bin/tests/_mordida.sh:196`, `bin/tests/_mordida.sh:225`, `bin/tests/_mordida.sh:258`, `bin/tests/test_disjuntor.sh:1499`, `bin/tests/test_padrao_de_cima.sh:648`) |
| `<raiz>.orig.d` / `.novo` | `_mordida_muta` remove/recria o original e grava os textos novos. | Verificador de declaração. (`bin/tests/_mordida.sh:152`, `bin/tests/_mordida.sh:159`, `bin/tests/_mordida.sh:168`, `bin/tests/_mordida.sh:230`, `bin/tests/_mordida.sh:246`) |
| `_MORDIDA_CASO/PID/SEQ/ULT/VAL/MARCAS` | Inicializador do sondado e trap DEBUG. | Trap e julgamento do último comando; estado global compartilhado dentro do selo. (`bin/tests/_mordida.sh:124`, `bin/tests/_mordida.sh:125`, `bin/tests/_mordida.sh:140`, `bin/tests/_mordida.sh:187`) |
| `MORDIDA_SEQ`, `MORDIDA_MARCAS`, `SELO_EXC_LOG/DIR`, `SELO_PROC_LOG`, `SELO_ESC_LOG`, `PYTHONPATH` | Trap exporta sequência; sondado fornece ambiente. | Sonda e coletores Python. (`bin/tests/_mordida.sh:126`, `bin/tests/_mordida.sh:142`, `bin/tests/_mordida.sh:41`, `bin/tests/test_disjuntor.sh:776`) |
| Handler SIGTERM / `sys.excepthook` | Sonda instala SIGTERM se encontrar default e substitui excepthook preservando o anterior. | Runtime Python chama os handlers; SIGTERM restaura default e reenvia sinal. (`bin/tests/_mordida.sh:58`, `bin/tests/_mordida.sh:60`, `bin/tests/_mordida.sh:112`, `bin/tests/_mordida.sh:120`) |
| `meta.out`, `falhas` | Cada meta sobrescreve `meta.out`; `_falha` incrementa; meta pode restaurar o contador. | Meta exige exatamente uma falha e trecho esperado. (`bin/tests/_mordida.sh:273`, `bin/tests/_mordida.sh:274`, `bin/tests/_mordida.sh:275`, `bin/tests/test_disjuntor.sh:42`) |

**Fila:**

- A sonda depende de `sys.monitoring` e do tool ID 4, sem confirmação de instalação bem-sucedida. Um erro no startup pode deixar logs vazios, enquanto stderr é descartado; controles usam procura negativa de exceções. Os metas podem denunciar essa ausência, portanto não afirmo verde global da suíte nessa condição. (`bin/tests/_mordida.sh:64`, `bin/tests/_mordida.sh:108`, `bin/tests/_mordida.sh:139`, `bin/tests/_mordida.sh:143`, `bin/tests/_mordida.sh:176`, `bin/tests/test_padrao_de_cima.sh:755`)

- As bases são copiadas mesmo quando o controle falha. O contador conserva a reprovação, então isso sozinho não gera verde global, mas a existência da base não prova controle válido. (`bin/tests/_mordida.sh:175`, `bin/tests/_mordida.sh:179`, `bin/tests/_mordida.sh:196`)

- `.proc` compara somente quantidades de processos abertos: um processo sem FIM no controle pode compensar outro processo diferente morto no mutante. A ocorrência em caso atual não está demonstrada. (`bin/tests/_mordida.sh:214`, `bin/tests/_mordida.sh:225`; ocorrência: [não verificado])

- Não há prazo externo no sondado: dependem dos timeouts escolhidos pelos próprios casos; uma mutação bloqueante fora desses pontos pode pendurar o selo. (`bin/tests/_mordida.sh:142`, `bin/tests/_mordida.sh:146`, `bin/tests/test_disjuntor.sh:579`, `bin/tests/test_disjuntor.sh:661`)

- Os loops atuais são sequenciais. Paralelizar casos dentro da mesma execução exigiria isolar `A/P/EST`, `_MORDIDA_*`, bases e `meta.out`; atualmente compartilham esses estados. Execuções independentes dos selos têm `mktemp -d` próprio. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:50`, `bin/tests/test_disjuntor.sh:1483`, `bin/tests/test_padrao_de_cima.sh:29`, `bin/tests/test_padrao_de_cima.sh:35`, `bin/tests/test_padrao_de_cima.sh:635`, `bin/tests/_mordida.sh:140`, `bin/tests/_mordida.sh:273`)

- O rc de `_mordida_morre` não é seu veredito: caminhos de erro chamam `_falha` e depois `return`; o contador é a autoridade. Os chamadores atuais usam essa convenção, portanto não há mascaramento demonstrado pelo último `[ -z "$msg" ] || echo`. (`bin/tests/_mordida.sh:194`, `bin/tests/_mordida.sh:267`, `bin/tests/_mordida.sh:268`, `bin/tests/test_disjuntor.sh:42`, `bin/tests/test_disjuntor.sh:1502`)

- O trap avalia expressões aritméticas antes do `return`, mas filtra apenas seu texto externo; variáveis podem conter expressões com efeitos colaterais e ser avaliadas novamente pelo `return`. O caso atual `b3_limiar` usa `r1` obtido dos números de falha; efeito colateral nele não foi demonstrado. (`bin/tests/_mordida.sh:130`, `bin/tests/_mordida.sh:131`, `bin/tests/test_disjuntor.sh:824`, `bin/tests/test_disjuntor.sh:825`)

## 31/10 20:55 · sessao 90e4fc606f0d · - · - · origem=Ronald (tmux)

Os testes novos mordem o defeito histórico: executei os três métodos em memória, com AST real e `unittest`; passaram. No caso de 1914: corpo atual **354**, mutante **360**, expectativa **354 minutos**. O teste substitui o símbolo consumido e chama diretamente o método sob `assertRaises(AssertionError)`. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:396`, `:408`, `:436`)

A expectativa vem da duração absoluta até a borda local das 05h, sem copiar valor numérico do motor. Os casos escolhidos começam dentro da janela e usam exclusivamente `pr=False`. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:392`, `:398`, `:403`, `:405`)

**Relógio solto NÃO procede:** `_local` somente constrói a data literal e aplica `localize`; os testes têm datas fixas. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:377`, `:397`, `:402`, `:404`)

**A proteção geral NÃO procede:** os 24 testes puros passaram, mas o mesmo processo reproduziu pausa `04:59:30–05:01`, sem prorrogação: corpo atual **1 minuto**, mutante anterior **0,5 minuto**. O único teste de segundos cobre pausa inteiramente prorrogada, longe da borda; não detecta essa regressão. (`app/ponto/motor_calculo_v2.py:184`, `:187`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:119`)

A grade usa minutos inteiros e seu oráculo trunca segundos; não certifica o caso acima. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:89`, `:149`, `:153`)

Execução Django canônica e testes de integração dos chamadores: **[nao verificado]**. [nao verificado]

## 31/10 20:55 · sessao 061e807bef00 · - · - · origem=Ronald (tmux)

Os chamadores continuam consumindo a mesma função e a mesma flag; o delta do cursor propaga aos resultados sem nova regra nos chamadores. A âncora da HE continua sendo `inicio_extra`, calculada em `app/ponto/motor_calculo_v2.py:434`, com minuendo em `:435` e desconto em `:436-437`.

| Valor | Quem escreve/fornece | Quem lê |
|---|---|---|
| `TZ_LOCAL` | `pytz.timezone(settings.TIME_ZONE)` em `app/ponto/motor_calculo_v2.py:28`; configuração copiada em `app/config/settings/base.py:93` | `localizar` em `app/ponto/motor_calculo_v2.py:78-79`; construção das janelas em `:100-104`; cursor novo em `:184`; `_local` dos testes em `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:378-379` |
| `entrada`, `saida`, `intervalos`, `prorrogacao` | Argumentos da função em `app/ponto/motor_calculo_v2.py:121`; HE fornece `inicio_extra`, `fim`, intervalos e flag em `:436-437`; método fornece parâmetros e atributo em `:1909-1910` | Conversão e recorte em `:162-164`; prefixos em `:185-186`; teto em `:190` |
| `_ent`, `_sai` | `localizar` em `app/ponto/motor_calculo_v2.py:162` | Recorte em `:164`; `_ent` ancora os prefixos em `:185-186`; ambos delimitam o teto em `:190` |
| `pecas` | Gerador convertido em lista ordenada, `app/ponto/motor_calculo_v2.py:164` | União em `:166-172` |
| `unidas` | Lista em `app/ponto/motor_calculo_v2.py:165`; extensão em `:170`; inclusão em `:172` | Condição de união em `:169-170`; laço externo em `:177` |
| `_i`, `_f` | Desempacotamento em `app/ponto/motor_calculo_v2.py:166` e `:177` | Validação/união em `:167-172`; início de `a` em `:178`; limite do laço e de `b` em `:179,184` |
| `a` | Inicialização em `app/ponto/motor_calculo_v2.py:178`; atualização em `:188` | Guarda em `:179`; próxima hora em `:183`; piso em `:184`; prefixo em `:186`; duração em `:187` |
| `hora_cheia` | Parede naive arredondada, mais uma hora, `app/ponto/motor_calculo_v2.py:183` | Localização para candidato de `b` em `:184` |
| `b` | Mínimo entre fim da peça e máximo entre candidato localizado/piso em `app/ponto/motor_calculo_v2.py:184` | Prefixo em `:185`; duração em `:187`; atualização de `a` em `:188` |
| Valor de cada prefixo / total noturno | `minutos_noturnos` escreve `total` em `app/ponto/motor_calculo_v2.py:96,116` e devolve em `:118` | Presença noturna em `:185-186`; teto do desconto em `:190` |
| `total` do desconto | Zero em `app/ponto/motor_calculo_v2.py:176`; soma de durações em `:187` | Incremento em `:187`; retorno limitado em `:190` |

Os leitores e destinos imediatos são:

- `MotorBase._noturno_dos_intervalos` delega em `app/ponto/motor_calculo_v2.py:1909-1910`; `calcular_noturnas` subtrai em `:967`, aplica fator em `:968` e retorna em `:969`. `_escrever_noturnas` escreve os campos do período em `:986-987`.
- `_ContextoNoturno` escreve sua flag em `app/ponto/calculador/regras.py:61` e empresta **o mesmo objeto de método** em `:63`. `noturnas_do_dia` mede o total em `:85`, subtrai o desconto em `:86` e retorna relógio/legais em `:89`. `do_dia` grava rubrica e detalhes em `:912-914`.
- HE por período chama a função de módulo importada em `app/ponto/calculador/regras.py:868`, passando pontas, flag e intervalos em `:886-888`; soma em `:889-890` e grava rubricas em `:896,898`.
- `noturno_no_par` passa um único par, intervalos e flags da CCT em `app/ponto/calculador/periodo.py:508-510` e devolve `NoturnoV3` em `:511`. O leitor chama em `:752` e escreve os campos noturnos em `:753-754`.
- O motor carimba pontas efetivas e intervalos em `app/ponto/motor_calculo_v2.py:1822-1824`. O atributo de prorrogação é escrito em `:756`, com consulta da regra existente se omitido em `:752-755`; a CCT do calculador recebe esse atributo em `app/ponto/calculador/nucleo.py:348`.

**Segundo juiz / derivação paralela da janela: NÃO procede neste delta.** O cursor consulta a função existente para decidir presença e teto; não replica o predicado 22–05 nem a condição de prorrogação. Há, sim, dependência estrutural declarada de bordas em horas cheias, cuja validade cabe à análise dos casos de fuso. (`app/ponto/motor_calculo_v2.py:173-175,183-190`)

**Segundo escritor fora da porta: NÃO procede no delta.** As novas atribuições são locais (`hora_cheia`, `b`); o retorno segue pelos escritores já descritos. Não há escrita de campo ou persistência no corpo examinado. (`app/ponto/motor_calculo_v2.py:162-190`)

**Relógio lido fora da borda: NÃO procede no delta.** O cursor deriva seus valores de `a`; os novos testes constroem datas literais, sem consulta a “agora”. (`app/ponto/motor_calculo_v2.py:183-184`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:377-405`)

Ordem de trava, pouso antes de pré-requisito, execução histórica do RED e impacto produtivo: `[nao verificado]`. [nao verificado]

## 31/10 20:54 · sessao 6aa0072937f2 · - · - · origem=Ronald (tmux)

Achados e caminhos, sem execução dos selos. [não verificado]

1. **Bloqueia — SIGTERM direto é aceito como “prazo”.** Um mutante que insira `os.kill(os.getpid(), 15)` antes de `ents = sorted(...)` em `estado_pdc` interrompe a decisão; o caso `i14_mesma` vê rc diferente de 3 e executa `return 1`. O handler registra `SINAL`, e o comparador exclui esse processo dos “sem FIM”, produzindo apenas uma NOTA de prazo; não exige `timeout` na origem do sinal. Com declaração vazia e sem exceção escapada, aceita a mordida. Referências: `bin/disjuntor.py:1531`; `bin/tests/test_padrao_de_cima.sh:90`; `bin/tests/test_padrao_de_cima.sh:116`; `bin/tests/_mordida.sh:55`; `bin/tests/_mordida.sh:219`; `bin/tests/_mordida.sh:228`; `bin/tests/_mordida.sh:265`.

2. **Bloqueia — `sys.exit(1)` também pode virar mordida sem decisão.** Plantado no mesmo ponto, faz `i14_mesma` terminar pelo `return 1`; a comprovação de encerramento verifica apenas `INI/FIM/SINAL`, enquanto a comprovação de exceção escapada depende de `sys.excepthook`. `SystemExit` executa `atexit` e dispensa o hook padrão [não verificado quanto à execução nesta cópia]; o código não tem tratamento próprio de `SystemExit`. Portanto o caminho permite `.proc` fechado, `.esc` vazio e assert positivo sem julgamento concluído. Referências: `bin/tests/test_padrao_de_cima.sh:116`; `bin/tests/_mordida.sh:49`; `bin/tests/_mordida.sh:114`; `bin/tests/_mordida.sh:214`; `bin/tests/_mordida.sh:258`.

3. **Fila, ou bloqueia se exigida mordida de cada cura — migração `res=2` não provada.** A versão mudou para 3 precisamente para reprocessar eventos vazios com `res=2`; o skip usa igualdade com a versão atual. O caso de legado remove completamente `res`, e a antiga verificação da versão foi relaxada para um glob que aceita qualquer versão iniciada por dígito. Reverter somente `PDC_RESOLVEDOR_V = 3` para 2 mantém esse caso passando e deixa a cura do legado `res=2` sem prova; não há mutante dessa constante na lista v6. Referências: `bin/disjuntor.py:1102`; `bin/disjuntor.py:1387`; `bin/tests/test_padrao_de_cima.sh:552`; `bin/tests/test_padrao_de_cima.sh:559`; `bin/tests/test_padrao_de_cima.sh:711`.

4. **Fila — controles podem ficar vermelhos por privilégio do executor.** `resolvedor_fonte` presume que `chmod 000` torne arquivo/diretório ilegível, sem verificar a capacidade efetiva de leitura antes da asserção. Um executor que preserve acesso retorna resultado diferente de NAO_SEI e reprova o controle por ambiente, não pela mutação. Referências: `bin/tests/test_padrao_de_cima.sh:534`; `bin/tests/test_padrao_de_cima.sh:537`; `bin/tests/test_padrao_de_cima.sh:570`; `bin/tests/test_padrao_de_cima.sh:573`; privilégio efetivo nesta cópia [não verificado].

5. **Fila — casos não são isolados para execução paralela no mesmo selo.** Os loops atuais são sequenciais e cada processo de selo cria seu próprio `_tmp`; portanto não há corrida entre casos nesses loops. Entretanto casos invocados paralelamente dentro da mesma instância compartilhariam `A/EST`, `_arvore` apagaria a árvore alheia, e metas compartilhariam `meta.out` e o contador `falhas`. Referências: `bin/tests/test_padrao_de_cima.sh:29`; `bin/tests/test_padrao_de_cima.sh:35`; `bin/tests/test_padrao_de_cima.sh:39`; `bin/tests/test_padrao_de_cima.sh:635`; `bin/tests/test_disjuntor.sh:39`; `bin/tests/test_disjuntor.sh:54`; `bin/tests/test_disjuntor.sh:1483`; `bin/tests/_mordida.sh:273`.

As duas mudanças de selo que investiguei especificamente têm caminho coerente de detecção:

- **P17 novo:** JSON inválido sobe de `leituras_semanais` até `main_pretool`, que registra FALHA para `CODEX_PAPEL=censo`; `_passa` exige stdout vazio **e** nenhuma FALHA nova, então o caso executa `return 35`. Referências: `bin/codex_limite_estado.py:153`; `bin/codex_limite_estado.py:156`; `bin/disjuntor.py:1899`; `bin/disjuntor.py:1918`; `bin/disjuntor.py:2076`; `bin/tests/test_disjuntor.sh:100`; `bin/tests/test_disjuntor.sh:103`; `bin/tests/test_disjuntor.sh:583`.
- **`escopo-fail-open` agora em `escopo_duvida`:** OBRA-Z aparece somente no RELATO novo, após corromper o estado; o escopo anterior não a contém. Tirar `obra in com_evento` libera OBRA-Z, e o caso executa `return 6`. Referências: `bin/tests/test_padrao_de_cima.sh:500`; `bin/tests/test_padrao_de_cima.sh:503`; `bin/tests/test_padrao_de_cima.sh:709`; `bin/disjuntor.py:1528`.

## 31/10 20:54 · sessao 38e85514b6af · - · - · origem=Ronald (tmux)

Achados prioritários:

1. **BLOQUEIA — `_existe` permite uma espera sem prazo, sob a trava do estado.** `lstat` aceita FIFO/dispositivo; o leitor seguinte usa `open(...).read()` comum. Um FIFO em `app/outra/models.py` ou na fonte citada prende o processo sem chegar a `NAO_SEI`; enquanto isso, `_com_estado` conserva o `flock`. O wrapper também chama essas guardas sem `timeout`; o prazo começa apenas no comando Codex posterior. (`bin/disjuntor.py:1225`, `bin/disjuntor.py:1247`, `bin/disjuntor.py:1273`, `bin/disjuntor.py:260`, `bin/disjuntor.py:275`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:168`, `bin/codex_pergunta.sh:177`.)

2. **BLOQUEIA — escopo velho ainda pode autorizar uma obra com histórico.** Caminho: existe escopo antigo sem OBRA-A; `decide_pdc` incorpora evento de OBRA-A e grava o estado; a gravação do novo escopo falha, mas a exceção é engolida e o arquivo antigo permanece; a linha depois sai do RELATO; o estado fica ilegível; `estado_pdc` aceita o escopo antigo e RELATO neutro e retorna `LIVRE`. O campo `em` é escrito, mas não é usado na validação/fallback. Isso preserva o falso livre que a alteração v6 pretende eliminar. (`bin/disjuntor.py:1396`, `bin/disjuntor.py:281`, `bin/disjuntor.py:293`, `bin/disjuntor.py:1456`, `bin/disjuntor.py:1480`, `bin/disjuntor.py:1488`, `bin/disjuntor.py:1496`, `bin/disjuntor.py:1512`, `bin/disjuntor.py:1523`, `bin/disjuntor.py:1530`.)

3. **BLOQUEIA — o novo caminho de payload cru ignora a chave DESLIGADO.** Com a chave existente, `["Agent", "construir a obra OBRA-C agora"]` provoca o `ValueError` antes da consulta a `desligado()`; o envelope cru identifica a obra e emite `deny` sem consultar a chave. O JSON truncado equivalente produz a mesma recusa. Isso contraria a definição “existindo, nenhum disjuntor age”. Os testes de chave usam payload válido, e os novos testes crus não combinam chave ligada. (`bin/disjuntor.py:40`, `bin/disjuntor.py:151`, `bin/disjuntor.py:2042`, `bin/disjuntor.py:2044`, `bin/disjuntor.py:2048`, `bin/disjuntor.py:1583`, `bin/disjuntor.py:2067`, `bin/disjuntor.py:2074`, `bin/tests/test_padrao_de_cima.sh:309`, `bin/tests/test_padrao_de_cima.sh:624`.)

4. **FILA — parâmetro `raiz` não governa o RELATO.** `decide_pdc(raiz=X)` usa X para estado, modelos, vocabulário e escopo, mas `_relato_consistente()` lê `hook_stop_fila1.RELATO`, determinado no import por `HOOK_STOP_RAIZ` ou pela localização desse módulo. O mesmo erro existe na releitura do fallback. Uma chamada em processo reutilizado com outra raiz pode cruzar estado de X e RELATO de Y; os chamadores CLI usuais que fixam o ambiente evitam esse cruzamento. (`bin/disjuntor.py:1365`, `bin/disjuntor.py:1367`, `bin/disjuntor.py:1371`, `bin/disjuntor.py:1425`, `bin/disjuntor.py:1456`, `bin/disjuntor.py:1523`, `bin/disjuntor.py:703`, `bin/disjuntor.py:707`, `bin/hook_stop_fila1.py:26`, `bin/hook_stop_fila1.py:30`, `bin/hook_stop_fila1.py:34`, `bin/tests/test_padrao_de_cima.sh:90`.)

5. **FILA — temporário órfão do escopo.** Se `fdopen`, `json.dump` ou `os.replace` falhar após `mkstemp`, o bloco de exceção só registra trilha; não remove o temporário. O escritor do estado e o escritor do RELATO já têm limpeza correspondente. (`bin/disjuntor.py:1484`, `bin/disjuntor.py:1488`, `bin/disjuntor.py:284`, `app/core/esteira_vigia.py:885`.)

6. **FILA — fechamento incompleto do envelope para estruturas internas inválidas.** `{"tool_name":"Agent","tool_input":["construir a obra OBRA-A"]}` é objeto JSON, mas `pretool` falha no `.get` da lista; `_ramo_pdc` também falha e devolve `None`, deixando a ferramenta liberada. O novo fallback cru só entra quando `payload is None`. A aceitação desse formato pelo produtor real da chamada é [não verificado]. (`bin/disjuntor.py:2043`, `bin/disjuntor.py:2045`, `bin/disjuntor.py:2004`, `bin/disjuntor.py:2005`, `bin/disjuntor.py:1583`, `bin/disjuntor.py:1588`, `bin/disjuntor.py:1589`, `bin/disjuntor.py:2076`.)

Mapa de estado:

| Estado | Escritores | Leitores / multiplicidade |
|---|---|---|
| `disjuntor_estado.json` | Escritor físico `_com_estado`: lê sob `flock`, chama callback e substitui atomicamente o arquivo. Escritores lógicos incluem B1, B3, PDC, B2 e B4. (`bin/disjuntor.py:244`, `bin/disjuntor.py:260`, `bin/disjuntor.py:275`, `bin/disjuntor.py:281`, `bin/disjuntor.py:494`, `bin/disjuntor.py:986`, `bin/disjuntor.py:1460`, `bin/disjuntor.py:1745`, `bin/disjuntor.py:1771`, `bin/disjuntor.py:2029`.) | `_com_estado`, `ler_estado` e painel PDC. Há vários escritores lógicos, mas uma porta física no código do instrumento. Testes também escrevem diretamente no arquivo de mentira. (`bin/disjuntor.py:267`, `bin/disjuntor.py:299`, `bin/disjuntor.py:1599`, `bin/tests/test_padrao_de_cima.sh:551`, `bin/tests/test_padrao_de_cima.sh:556`.) |
| `pdc.eventos` | `decide_pdc` cria `visto_em/tipo/obra/entidades/res` e reclassifica eventos antigos vazios. (`bin/disjuntor.py:1396`, `bin/disjuntor.py:1400`.) | Deduplicação, cálculo de janela, disparadas, nomeação e painel. (`bin/disjuntor.py:1386`, `bin/disjuntor.py:1298`, `bin/disjuntor.py:1308`, `bin/disjuntor.py:1438`, `bin/disjuntor.py:1600`.) |
| `pdc.nomeados` | Marca pares `sha|entidade` ao consumir liberação. (`bin/disjuntor.py:1438`, `bin/disjuntor.py:1440`.) | `_pdc_disparadas` verifica se ainda existe evento não nomeado. (`bin/disjuntor.py:1306`, `bin/disjuntor.py:1312`.) |
| `pdc.libera_vistas` | Registra hash da liberação consumida. (`bin/disjuntor.py:1435`.) | Impede reutilização da linha. (`bin/disjuntor.py:1430`.) |
| `pdc.fotos` | Fotografa topo no disparo e remove foto de episódio encerrado. (`bin/disjuntor.py:1403`, `bin/disjuntor.py:1412`, `bin/disjuntor.py:1445`.) | Impede liberação por linha anterior ao disparo. (`bin/disjuntor.py:1433`.) |
| `pdc.disparadas` | Substituído pelo resultado recalculado. (`bin/disjuntor.py:1443`, `bin/disjuntor.py:1444`.) | Detecta disparo novo, informa painel e gera cópia do escopo. (`bin/disjuntor.py:1405`, `bin/disjuntor.py:1413`, `bin/disjuntor.py:1608`, `bin/disjuntor.py:1480`.) |
| Escopo | `_pdc_grava_escopo`, chamado sob a trava após salvar o estado. Testes removem, corrompem e alteram a cópia temporária. (`bin/disjuntor.py:1456`, `bin/disjuntor.py:1472`, `bin/tests/test_padrao_de_cima.sh:506`, `bin/tests/test_padrao_de_cima.sh:519`, `bin/tests/test_padrao_de_cima.sh:522`.) | `_pdc_escopo_lido` e fallback de `estado_pdc`; no instrumento há um escritor, mas estado e escopo são duas gravações independentes. (`bin/disjuntor.py:1492`, `bin/disjuntor.py:1512`, `bin/disjuntor.py:281`, `bin/disjuntor.py:1487`.) |
| RELATO | `relato_negacao` chama `esteira_vigia.no_relato`, que tranca o diretório e faz replace; `gerar_cortes` e `deploy_agendado` escrevem diretamente, fora dessa porta. Há múltiplos escritores reais, já declarados como resíduo/fila. (`bin/disjuntor.py:234`, `bin/disjuntor.py:236`, `app/core/esteira_vigia.py:815`, `app/core/esteira_vigia.py:848`, `app/core/esteira_vigia.py:884`, `bin/gerar_cortes.py:136`, `bin/deploy_agendado.sh:44`, `app/core/esteira_vigia.py:839`.) | PDC lê bytes completos duas vezes, compara inode/mtime/tamanho e conteúdo, e usa a mesma foto para corpo/topo; fallback relê fora da transição fracassada. (`bin/disjuntor.py:697`, `bin/disjuntor.py:704`, `bin/disjuntor.py:710`, `bin/disjuntor.py:714`, `bin/disjuntor.py:1379`, `bin/disjuntor.py:1523`.) |
| `_PDC_LIVRE_NESTA` | `main_pretool` limpa por chamada; `pretool` acrescenta a obra após `_nega_pdc` retornar livre. (`bin/disjuntor.py:2039`, `bin/disjuntor.py:2014`, `bin/disjuntor.py:2018`.) | `_ramo_pdc` não nega novamente a obra comprovadamente livre. (`bin/disjuntor.py:1591`.) |

Não encontrei segundo juiz de negócio criado pelas mudanças de PDC: a autoridade segue `decide_pdc`/`estado_pdc`, e as portas delegam a ela; as duplicações de RELATO são de escrita do documento, anteriores e expressamente declaradas. (`bin/disjuntor.py:1355`, `bin/disjuntor.py:1503`, `bin/disjuntor.py:1552`, `bin/disjuntor.py:1565`, `app/core/esteira_vigia.py:839`.)

A leitura de tempo continua dentro de `decide_pdc` quando não recebe `agora`; `trilha` também lê o relógio diretamente. A mudança v6 não acrescentou essas leituras, apenas usa `agora` ao calcular o histórico. (`bin/disjuntor.py:1364`, `bin/disjuntor.py:140`, `bin/disjuntor.py:1448`.)

## 31/10 20:54 · sessao db15a087b000 · - · - · origem=Ronald (tmux)

**Escritores e fontes dos campos**

- `CelulaDia.trabalha` admite `True`, `False`, `None`; significado declarado: trabalho, folga, indeterminado/limbo cobra (`app/escala/models.py:1654`).
- Escrita persistente encontrada em `app/ponto/portas/celula.py`: regeneração põe `False` fora da vigência (`:231`, salva `:233`); calcula `valor = ec.eh_dia_trabalho_calculado(d)` e cria (`:243`, `:247`) ou atualiza (`:260`, `:264`); reversão deriva de `(alvo or {}).get('marcos') is not None` (`:532`, salva `:538`); geração em lote calcula (`:590`), cria (`:595`) ou atualiza (`:612`, salva `:615`).
- Chamadores desses escritores: comando `gerar_celulas` (`app/ponto/management/commands/gerar_celulas.py:311`); sinais (`app/escala/signals.py:71`, `:117`); cadastro do tipo (`app/escala/services/cadastro_tipo.py:348`); ajuste/desfazer automático (`app/escala/services/escala_auto_executor.py:104`, `:143`); folgas (`app/escala/folgas.py:220`); correção retroativa (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:230`); regeneração em exportada (`app/ponto/portas/regen_exportada.py:174`); regeneração de um dia delega (`app/ponto/portas/celula.py:474`); reversão é chamada em `app/ponto/management/commands/aplicar_col900.py:77`.
- `ata.tipo_dia` é produzido por `ata_do_dia`: copia `dia.get('tipo_dia')` (`app/ponto/services/cartorio.py:560`, `:568`). As lavras recebem essa ata nos caminhos sem batida (`:1044`, `:1045`) e normal (`:1563`, `:1565`).
- Persistência da ata: método do modelo delega (`app/escala/models.py:1794`); porta copia a ata recebida (`app/ponto/portas/celula.py:730`), preserva apenas chaves de outra operação (`:732`, lista `:94`), atribui (`:741`, `:768`) e salva (`:743`, `:775`). `carimbar_precedencia` altera somente `ata['precedencia']` (`:818`, `:819`, `:820`).
- Fonte anterior de `tipo_dia`: builder monta `folga`/`trabalho` (`app/escala/utils.py:1743`, `:1749`); overlay de FolgaDia escreve `folga` (`:1118`); cobertura escreve `falta`/`ausencia` (`:1174`, `:1180`).
- Leitor central prefere `ata.get('tipo_dia')`; fallback: `True→trabalho`, `False→folga`, `None→indefinido` (`app/escala/services/leitor_celula.py:345`, `:346`). Transporta também os insumos brutos do juiz (`:370`).
- Cópias de ata em memória não são escritores persistentes: `app/ponto/portas/celula.py:680`; `app/escala/utils.py:1826`, `:1827` — a última contém apenas lâmpadas e órfãs (`:1832`, `:1835`).

**Leitor adicional importante da mesma família**

`app/ponto/marcos.py:155` lê `trabalha` por `values_list`, portanto escapa de busca apenas por `.trabalha`. O predicado literal é `if trabalha is False:` (`:159`), que retorna `'FOLGA'` (`:160`). `None` não veta (`:163`), igual ao juiz sem ata não trabalho; a ata não é consultada (`:155`). Migrar adicionaria veto para `trabalha=True/None` com ata preenchida diferente de trabalho (`app/chamados/juizes.py:408`). O próprio comentário declara que o gate atende emissores de atraso, deduplicação, reabertura e furo (`app/ponto/marcos.py:133`): classificar por chamador, não migrar o gate inteiro sem separar as perguntas.

**Outros predicados encontrados: B — perguntas diferentes**

| Sítio e literal | Pergunta efetiva / None / migração |
|---|---|
| `app/ponto/worklist.py:19`, `dia['tipo_dia'] != 'trabalho'` | Desenha gap pela ata; retorna “sem classificar”, não cala emissão (`:8`, `:12`, `:20`). `None` sem ata vira indefinido e sai, enquanto juiz permite cobrança (`app/escala/services/leitor_celula.py:346`; `app/chamados/juizes.py:408`). Chamadores `app/ponto/worklist.py:60`, `app/colaboradores/views.py:3021`, comando `app/ponto/management/commands/worklist_dp.py:13`. Não migrar. |
| `app/escala/services/furos_diarios.py:39`, `not d or d.get('tipo_dia') != 'trabalho'` | Métrica de marcos furados e projeção FuroDiario (`:2`, `:7`, `:19`). None/indefinido sai; trocar incluiria universo indeterminado sem necessariamente haver marcos (`:44`, `:45`). Chamadores `:89` e `app/colaboradores/services/pos_vinculo.py:81`. Não migrar automaticamente para juiz de chamados. |
| `app/relatorios/furos.py:151`, `d.get('tipo_dia') != 'trabalho'` | Exibição atribui `'folga'` (`:152`); None sem ata torna-se folga na tela, divergindo do juiz de cobrança. Chamador `app/relatorios/views.py:1541`. Não migrar; corrigir rótulo seria outra obra. |
| `app/relatorios/furos.py:218`, `:237`, `d.get('tipo_dia') != 'trabalho' or d.get('sem_ancora')` | Dias em branco/matriz do Raio-X de folha (`:211`, `:230`). None sai. Chamador de `furos_por_dia`: `app/folha/views.py:616`; chamador de `dias_100_em_branco` não localizado [nao verificado]. Não migrar. |
| `app/ponto/propositor.py:36`, `:210`, `not dia or dia.get('tipo_dia') != 'trabalho'` | Degeneração e suspeita de template, usadas para propor reorganização de batidas (`:34`, `:201`). None sai; permitir cobrança não prova geometria. Chamadores `app/ponto/management/commands/propor_reorganizacao.py:67`, `:71`, `app/inteligencia/calculadores.py:325`. Não migrar. |
| `app/ponto/services/simular_dia.py:12`, `dd.get('tipo_dia') != 'trabalho' or dd.get('orfas')` | “Dia perfeito” para autorizar correção de batidas (`:1`, `:11`). None retorna False. Chamadores `app/ponto/services/veredito_celula.py:216`, `:223` e `app/ponto/services/flip_auto.py:258`. Não migrar. |
| `app/ponto/services/veredito_celula.py:221`, `dd0.get('tipo_dia') != 'trabalho'` | Admin pode materializar marcos previstos? Recusa “nada a materializar” (`:222`), não emissão. None recusa. Não migrar. |
| `app/escala/orfa_classe.py:65`, `(dia.get('tipo_dia') or 'folga') != 'trabalho'` | Procedência de órfã, devolve FOLGA_DESMENTIDA (`:53`, `:66`), não cobrança. None é tratado como não trabalho. Chamador de produção não localizado [nao verificado]. Não migrar. |
| `app/ponto/services/vigia_entrada.py:79`, `:120`, filtros `trabalha=True` | Mede padrões de lâmpadas/entrada perdida (`:63`, `:111`). None não entra; juiz o cobraria sem ata contrária. Chamador comando `app/ponto/management/commands/vigia_entrada_faltante.py:24`, `:39`; cron `app/config/crons.py:716`. Não migrar. |
| `app/ponto/services/esmeril_espelho.py:71`, `x.trabalha is False ...`; `:121`, `:123`, `x.trabalha and ...` | Assinaturas de cadastro/escala errada (`:66`, `:67`, `:117`). None não caracteriza folga nem trabalho conhecido. Chamadores `:105`, `app/escala/services/escala_certa.py:27`, comando `app/ponto/management/commands/esmeril_espelho.py:16`. Não migrar. |
| `app/ponto/services/passivo_s84.py:96`, `TRABALHO if cel.trabalha else FOLGA` | Classificação de batidas antigas; FOLGA alimenta seleção para retratação (`:108`, `:109`). None vira FOLGA, diferente do juiz. Chamadores `app/ponto/management/commands/passivo_disputa_retro.py:70`, `app/ponto/management/commands/diff_passivo_s84.py:45`. Não é juiz de cobrança; não migrar. |
| `app/pautas/services.py:418`, `'folga' if cel and cel.trabalha is False else '-'` | Fallback textual quando não há marcos; None mostra `'-'`. Não decide cobrança; não migrar. |
| `app/folha/export.py:1237`, `c.veredito is None and c.trabalha is not None ...` | Retenção da folha por falta de julgamento (`:1238`). None não retém por esse ramo; não migrar ao juiz de chamados. |
| `app/ponto/management/commands/tripwire_celulas.py:338`, `Q(trabalha=False) \| Q(trabalha__isnull=True)`; `:342`, `ata.get('tipo_dia') != 'folga' or ata.get('lampadas')` | Detecta n_celulas fantasma em ata de folga (`:344`, `:345`), não dia sem cobrança. None é pré-filtro; só ata folga sem lâmpadas passa (`:342`). Não migrar. |
| `app/escala/services/semana_do_posto.py:86`, `trabalha=True` | Conjunto de dias previstos na semana (`:82`, `:83`); None fica fora. Não migrar. |
| `app/escala/services/propositor.py:474`, `:485`, `trabalha=True` | Dias discordantes e previstos para proposta de escala (`:470`, `:484`). None fica fora. Não migrar. |
| `app/escala/services/cadastro_realidade.py:208`, `trabalha=True` | Amostra da jornada medida versus cadastro (`:189`, `:215`, `:223`). None fica fora. Não migrar. |
| `app/colaboradores/services/feriado_do_posto.py:94`, `trabalha=True` | Presença em feriado versus dia normal (`:74`, `:75`). None fica fora. Não migrar. |

**Inventário direto para item 5**, além das linhas acima e dos sítios fornecidos pelo usuário:

- Leitura de `trabalha`: juiz `app/chamados/juizes.py:408`; `app/chamados/services/fabrica_da_celula.py:153`; `app/escala/models.py:1072`, `:1810`; `app/escala/servico_jornada.py:303`; `app/ponto/precedencia.py:306`, `:316`, `:322`; `app/ponto/supra_juiz.py:196`; calendário `app/colaboradores/services/calendario.py:814`; correção projetada `app/colaboradores/management/commands/corrigir_escala_retroativa.py:118`; censo `app/ponto/management/commands/censo_fase_12x36.py:392`; diagnóstico `app/ponto/management/commands/dna_x_batida_real.py:59`, `:65`; contagem `app/ponto/management/commands/plano_b_no_dinheiro.py:144`; tripwire `app/ponto/management/commands/tripwire_celulas.py:30`, `:33`, `:98`, `:257`; comparação com template `app/escala/management/commands/celulas_contra_o_template.py:29`, `:46`; a própria porta lê antes de decidir geração/regeneração `app/ponto/portas/celula.py:227`, `:251`, `:604`, `:611`.
- Leitura direta de `ata.tipo_dia`: juiz `app/chamados/juizes.py:408`; projeção do leitor `app/escala/services/leitor_celula.py:345`; tripwire `app/ponto/management/commands/tripwire_celulas.py:342`. Os demais predicados de `tipo_dia` consomem o dict da grade, não diretamente a chave da ata (`app/escala/services/leitor_celula.py:360`).
- Exemplos de leitores de `tipo_dia` para dinheiro, que não devem migrar: `app/folha/export.py:1071`, `:1075`, `:1080`; `app/ponto/services/dia_pago.py:386`, `:393`, `:398`; `app/folha/causas.py:40`, `:41`, `:100`; status do dia `app/ponto/status_dia.py:96`, `:98`, `:106`, `:115`.

## 31/10 20:53 · sessao e874054837e2 · - · - · origem=Ronald (tmux)

Consolidado de leitura; nenhum dado atual de banco/log foi inferido. [nao verificado]

**A — mesma pergunta; migrar a cláusula de dia, preservando outros gates.**

| Sítio/predicado literal | Pergunta efetiva; `None` versus juiz; mudança | Chamador de produção |
|---|---|---|
| `app/ponto/management/commands/emitir_furo_retroativo.py:127` — `ec.eh_dia_trabalho(data) is not True` | Cala emissão de furo antes de `_abrir_chamado`; `None` cala, enquanto juiz com ata ausente/`trabalho` permite cobrar. Migrar muda `None`; passa também a respeitar ata não trabalho quando previsão=True. `app/ponto/management/commands/emitir_furo_retroativo.py:118`, `:127`, `:142`; juiz `app/chamados/juizes.py:408`. | `Command.handle`, entrada declarada sob ordem em `app/config/crons.py:1339`. |
| `app/ponto/management/commands/processar_alertas_avancados.py:49` — `not ec.eh_dia_trabalho(ontem)` | Cala caminho de alerta/cobrança de turno aberto; `None` cala. Juiz sem ata de outro tipo não cala. Com previsão=True e ata de ausência/folga, migrar cala onde hoje deixa avançar. `app/ponto/management/commands/processar_alertas_avancados.py:49`, `:82`, `:121`; juiz `app/chamados/juizes.py:408`. | `Command.handle:19`; cron `app/config/crons.py:344`. |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:76` — `filter(..., trabalha=True)` | Seleciona células para emitir perguntas; `None` excluído. Juiz sem ata de outro tipo incluiria `None`; True com ata não trabalho seria excluído. Outros requisitos permanecem: motivos e veredito que acusa. `app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `:111`, `:120`, `:123`; juiz `app/chamados/juizes.py:408`. | `Command.handle:57`; encadeado de apuração `app/config/crons.py:1464`. |
| `app/ponto/services/vigia_de_hora.py:21` — `filter(..., trabalha=True)` | Seleciona células que podem chegar ao julgamento/emissão; `None` nunca chega. Juiz sem ata de outro tipo deixaria chegar; True com ata não trabalho deixaria de chegar. `app/ponto/services/vigia_de_hora.py:21`, `:113`, `:130`; juiz `app/chamados/juizes.py:408`. | `_candidatas` chamado por `rodar:113`; comando chama `rodar` em `app/ponto/management/commands/vigia_de_hora.py:28`; cron `app/config/crons.py:348`. |
| `app/chamados/services/declaracao_texto.py:95` — `tipo_dia != 'trabalho'` | Suprime marcos faltantes que sustentam perguntas. `trabalha=None`, sem tipo na ata, vira `indefinido` e é descartado; juiz retorna False. True/None com ata preenchida diferente de trabalho continuam descartados. False com ata trabalho hoje passa; juiz barraria. Leitura/fallback em `app/escala/services/leitor_celula.py:345`; gate `app/chamados/services/declaracao_texto.py:95`; juiz `app/chamados/juizes.py:408`. | `_complementar_marcos_faltantes` chama em `app/chamados/services/disputa_emissao.py:794`, criando pergunta em `:813`; `_ata_do_dia` chama em `:827`. Também leitores de declaração/exibição em `app/chamados/services/declaracao_texto.py:285`, `:411`, `:524`, `:563`. |
| `app/ponto/marcos.py:159` — `trabalha is False` | Cláusula “FOLGA” de `celula_veta_cobranca`; None não veta, igual ao juiz quando ata ausente/trabalho. Hoje não lê ata, portanto True/None com ata preenchida não trabalho só são vetados se outro veto de veredito os alcançar. Migrar apenas esta cláusula muda esses casos. `app/ponto/marcos.py:155`, `:159`, `:161`; juiz `app/chamados/juizes.py:408`. | `app/ponto/management/commands/detectar_ausencias.py:626`; `app/ponto/management/commands/reconciliar_grade.py:502`; `app/chamados/reconciliador.py:842`; métrica `app/ponto/services/furos_sem_cobranca.py:98`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:376` — `c.veredito in acusam or not c.trabalha or k in com_batida` | Em `contraprova_piso`, a cláusula `not c.trabalha` dispensa cobrança no dia None; juiz sem ata de outro tipo não dispensa. Trocar só cláusula de dia, preservando veredito/fato. Pergunta declarada: comprovar “fecha por lei, furo vive na célula”. `app/chamados/management/commands/auditar_invariantes_chamados.py:305`, `:308`, `:376`; juiz `app/chamados/juizes.py:408`. | `contraprova_piso` chamado em `app/chamados/management/commands/auditar_invariantes_chamados.py:760`; cron auditor `app/config/crons.py:523`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1443` — `not _cel4.trabalha or _cel4.veredito not in _ACUSAM_a4` | Dispensa célula como sustentação da cobrança/reabertura; None é dispensado mesmo com veredito acusador. Juiz sem ata de outro tipo não dispensa. Trocar apenas cláusula de dia, preservando veredito. `app/chamados/management/commands/auditar_invariantes_chamados.py:1423`, `:1433`, `:1443`; juiz `app/chamados/juizes.py:408`. | Corpo de `Command.handle` em `app/chamados/management/commands/auditar_invariantes_chamados.py:552`; cron `app/config/crons.py:523`. |

**B — pergunta diferente; não migrar.**

| Sítio | Pergunta, None e efeito de uma substituição mecânica | Chamador |
|---|---|---|
| `app/ponto/services/flip_auto.py:208` — `cel is not None and cel.trabalha is False`; `:212` — `v is None or v.trabalha is not True` | Geometria: escolher dia/âncora da batida para etiquetar E/S, não emissão. Current=None não entra no ramo de folga; véspera=None é recusada como âncora. Juiz não considera None sem ata como sem trabalho; substituição permitiria essa véspera, alterando âncora/flips. Pergunta de geometria/turno: autoridades de marcos e turno em `app/docs/JUIZES.md:22`; finalidade explícita em `app/ponto/services/flip_auto.py:165`, `:171`, `:176`; gates `:208`, `:212`. | `avaliar_fila` chama em `app/ponto/services/flip_auto.py:310`; comando `app/ponto/management/commands/flip_automatico.py:32`; tela `app/ponto/views.py:3295`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1542` — `_cel is None or _cel.trabalha is not True` | C5: célula contradiz FolgaDia? Só True prova contradição; None não prova trabalho. Migrar pelo complemento do juiz acusaria None como contradição sem evidência. Não há juiz específico de contradição FolgaDia identificado em JUIZES. `app/chamados/management/commands/auditar_invariantes_chamados.py:1530`, `:1542`, `:1549`; juiz `app/chamados/juizes.py:408`. | `Command.handle:552`; cron `app/config/crons.py:523`. |
| `app/ponto/worklist.py:19` — `dia['tipo_dia'] != 'trabalho'` | Desenho de gap na worklist: retorna “sem classificar”, não absolvição de cobrança. None sem ata vira indefinido e retorna gap=None. Trocar por juiz alteraria apresentação e poderia encaminhar para “sem lâmpada”, não produzir cobrança por si. `app/ponto/worklist.py:7`, `:10`, `:19`, `:22`; fallback `app/escala/services/leitor_celula.py:345`. | `coletar_gaps_dp:60`; tela `app/colaboradores/views.py:3021`; comando `app/ponto/management/commands/worklist_dp.py:13`. |
| `app/ponto/propositor.py:36` — `not dia or dia.get('tipo_dia') != 'trabalho'` | Diagnóstico de dia degenerado para proposta de movimentação de batidas. None sem ata descartado; trocar por juiz amplia universo de propostas e muda False+ata trabalho. `app/ponto/propositor.py:34`, `:36`, `:43`, `:48`; leitor `app/escala/services/leitor_celula.py:345`. | `app/ponto/management/commands/propor_reorganizacao.py:71`. |
| `app/ponto/propositor.py:210` — mesmo predicado | Estatística de deltas em dias de trabalho para suspeita de template; não decide calar cobrança. None sem ata descartado; substituição muda amostra estatística. `app/ponto/propositor.py:201`, `:203`, `:210`, `:221`; leitor `app/escala/services/leitor_celula.py:345`. | `app/ponto/management/commands/propor_reorganizacao.py:67`; `app/inteligencia/calculadores.py:325`. |
| `app/ponto/services/simular_dia.py:12` — `dd.get('tipo_dia') != 'trabalho' or dd.get('orfas')` | Juiz de “dia perfeito” para simular flips: exige tipo trabalho, nenhuma órfã, nenhum missing, tipos corretos. None sem ata falha; substituir cláusula alteraria perfeição/flip. `app/ponto/services/simular_dia.py:1`, `:12`, `:18`, `:21`. | `app/ponto/services/flip_auto.py:303`, `:304`; `app/ponto/services/veredito_celula.py:219`. |
| `app/escala/services/furos_diarios.py:39` — `not d or d.get('tipo_dia') != 'trabalho'` | Apuração da métrica `% marcos furados/marcos previstos`, persistida em FuroDiario; não é emissão de chamado. None sem ata descartado; substituição muda denominador/dias e False+ata trabalho. `app/escala/services/furos_diarios.py:1`, `:12`, `:19`, `:39`, `:98`; leitor `app/escala/services/leitor_celula.py:345`. | `apurar:89`; `app/escala/management/commands/apurar_furos_diarios.py:8`; `app/colaboradores/services/pos_vinculo.py:52`. |
| `app/ponto/services/vigia_entrada.py:79`, `:120` — `filter(..., trabalha=True)` | Contagem/diagnóstico de entrada faltante com saída presente; distingue falha de caminho versus adesão, não decide cobrar o dia. None excluído; juiz sem ata o incluiria na população, mudando estatística. `app/ponto/services/vigia_entrada.py:11`, `:63`, `:79`, `:111`, `:120`. | `contador` chama agregação em `app/ponto/services/vigia_entrada.py:99`; demais chamadores não rastreados [nao verificado]. |

A regra comum para os leitores de dict merece aparecer explicitamente: `trabalha=None` **não significa sempre** `tipo_dia=None`; o leitor prefere a ata e, sem tipo preenchido, produz `'indefinido'`. Assim None+ata trabalho passa em predicados de tipo, enquanto None sem ata é descartado. `app/escala/services/leitor_celula.py:345`.

## 31/10 20:53 · sessao 771f5d470aba · - · - · origem=Ronald (tmux)

Censo somente de leitura. Referências abaixo usam a linha atual da cópia.

| Classe | Sítio e predicado literal | Pergunta efetiva; `None` e efeito da migração | Chamadores de produção |
|---|---|---|---|
| **A — migrar a cláusula de elegibilidade** | `app/ponto/services/cartorio.py:1504`: `ec is not None and ec.eh_dia_trabalho(data) is True` | Decide se o furo total chega a `_abrir_chamado` (`:1495-1517`). Com célula `trabalha=None`, `eh_dia_trabalho` devolve `None` (`app/escala/models.py:1071-1072`) e este gate cala; juiz não cala sem ata não trabalho (`app/chamados/juizes.py:408`). Migração muda `None` e também `True` com ata preenchida diferente de trabalho. **Existência de vínculo e marcos continua sendo requisito separado** (`cartorio.py:1503-1507`). | `_julgar_colab_corpo` via `julgar_colab` (`cartorio.py:777,802`); batch `app/ponto/management/commands/processar_cartorio.py:65`; evento `cartorio.py:1721-1723`. |
| **A — migrar a cláusula de elegibilidade** | `app/ponto/services/cartorio.py:1608`: `ec is None or ec.eh_dia_trabalho(data) is not True` | `_marcos_do_furo_parcial` retorna `None` e impede emissão parcial; mesmos desvios acima. O restante responde quais marcos apagados têm motivo e âncora (`:1624-1635`), pergunta diferente que permanece. | Helper chamado em `cartorio.py:1512,1543`; emissão parcial em `:1550-1552`; cadeia batch/evento acima. |
| **C — exceção declarada, não migrar automaticamente** | `app/ponto/services/cartorio.py:1207`: `_cel.trabalha is not False and _acesos(x, _ata) is not True` | Decide se chamado **protesta**, alimentando `chamados_vivos`, lâmpada cobrado e reconciliação (`:1208,1214,1250,1281-1287`). `None` permite protesto se marcos não acesos: concorda com juiz sem ata não trabalho. Adicionar juiz mudaria `True/None` com ata não trabalho. Esta mudança está **explicitamente pendente**, porque acrescenta segunda cláusula ao universo do protesto (`app/docs/JUIZES.md:60-62`; `app/core/juizes.py:145-148`). | Mesma cadeia cartório; usos internos `cartorio.py:1208,1250,1281`. |
| **B — não migrar** | `app/ponto/services/cartorio.py:1714`: `data == hoje and cel.trabalha is not False and not teto_de_hoje` | Responde **quando aguardar fim do turno antes de julgar hoje**, não se cobrar (`:1715-1719`). `None` aguarda; juiz sem ata não trabalho também não declara dispensa. Substituir por juiz permitiria julgamento antecipado com `True/None` e ata não trabalho. Juiz específico de horário para esta guarda em `JUIZES.md`: **[nao verificado]**; documento distingue borda da meia-noite sem juiz (`app/docs/JUIZES.md:66-67`). | `app/ponto/signals.py:93`; `app/chamados/signals.py:180`; `app/chamados/reconciliador.py:675`; `app/ponto/services/vigia_de_hora.py:130`; `app/chamados/services/cobrar_dia.py:39`; `app/ponto/services/furos_sem_cobranca.py:128`; `app/colaboradores/services/pos_vinculo.py:72`; `app/ponto/portas/celula.py:372`; comandos `rejulgar_ausencia_em_folga.py:71`, `curar_residuo_bug115.py:143`. |
| **B — não migrar** | `app/ponto/services/dia_do_colab.py:121`: `not cel.trabalha` | Gera frase de apresentação **“Dia sem trabalho previsto: nenhuma marca era esperada”** (`:122-123`). `None` recebe a frase; juiz sem ata não trabalho retorna `False`. Migrar mudaria frase para `None`, e para `True` com ata não trabalho. Previsão tem juiz `EscalaColaborador.eh_dia_trabalho` (`app/docs/JUIZES.md:74-75`); status lavrado tem outro (`:82-83`). | `_veredito_do_dia` em `dia_do_colab.py:100`; porta `app/api/views_mensageria.py:1963`; ficha `app/colaboradores/services/ficha.py:280`. |
| **B — não migrar** | `app/ponto/services/dia_do_colab.py:130-131`: `f.get('faixa') in ('feriado', 'ferias', 'atestado', 'afastamento', 'falta', 'abono', 'folga') and cel.trabalha` | Compara **precedência e célula discordando** para frase administrativa (`:132-136`), não cala cobrança. `None` não acusa a discordância. Troca por juiz alteraria a natureza da comparação; precedência é `fatos_do_dia` (`app/docs/JUIZES.md:76-77`). | Mesma cadeia anterior. |
| **B — não migrar** | `app/relatorios/dia_da_frota.py:47`: `not cel.trabalha` | Conta **folga** e exclui da listagem de acusados (`:48-55`); não altera chamado. `None` conta folga, contra juiz `False` sem ata não trabalho. Migração alteraria contagens/listagem para `None` e `True` com ata não trabalho. Pergunta declarada é relatório “quem faltou”, pelo veredito/lâmpadas (`:1-6`); autoridades pertinentes são previsão/status (`app/docs/JUIZES.md:74,82`). | `app/api/views_mensageria.py:1814`. |
| **B — não migrar** | `app/chamados/services/fila_por_causa.py:192-193`: `CelulaDia.objects.filter(colaborador_id__in=cids, trabalha=True, data__gte=ini)` | Carrega **vizinhos previstos** (`:194`) para diagnosticar início de adesão, falta em dias vizinhos e ausência sustentada (`:224-251`). `None` não entra em `previstos`; juiz sem ata não trabalho permitiria cobrança. Migração alteraria vizinhos e gaveta, além de retirar `True` com ata não trabalho. Previsão: juiz de `app/docs/JUIZES.md:74-75`. | `particionar` via `quadro` (`fila_por_causa.py:271`), `toques` (`:308`) e `bloqueios_para_fechar` (`:353`); API `app/api/views_mensageria.py:1582`; badge `app/core/management/commands/lavrar_badge_navbar.py:63`; `validar_classe_a.py:44`; comando `app/chamados/management/commands/fila_por_causa.py:13`. |

Outros predicados semelhantes confirmados como perguntas diferentes:

| Classe | Predicado e pergunta | `None` / migração / produção |
|---|---|---|
| **B** | `app/ponto/services/simular_dia.py:12`: `dd.get('tipo_dia') != 'trabalho' or dd.get('orfas')`; responde **dia perfeito para simular correções**, verificando missing e coerência de tipos (`:14-23`). | Não lê diretamente `trabalha`; lê grade construída por `grade_dia` (`:5-8`). A grade constrói `None` como trabalho com `sem_ancora` (`app/escala/utils.py:725-752`). Trocar pelo juiz mudaria conceito de perfeição e perderia/alteraria requisito das órfãs. Prod: `app/ponto/services/flip_auto.py:303-304`; `app/ponto/services/veredito_celula.py:219,223`. |
| **B** | `app/ponto/services/veredito_celula.py:221`: `dd0.get('tipo_dia') != 'trabalho'`; responde **há trabalho previsto para materializar decisão administrativa trabalhou?**, explicitado na recusa (`:222`). | Não é silêncio de chamado. `None` indireto é grade/sem âncora acima. Prod: view `app/ponto/views.py:3304`; serviço declarativo `app/ponto/services/declarar_dia.py:206-207`. |
| **B** | `app/ponto/services/espelho.py:690`: `bool(_gd and _gd.get('tipo_dia') == 'folga')`; responde flag visual `folga` (`:781`). | `None` direto não é lido. Prod interno `espelho.py:1266`; tela `app/ponto/views.py:158`, API `app/api/views.py:1369`, ficha `app/colaboradores/services/ficha.py:199`. Juiz de status lavrado declarado em `app/docs/JUIZES.md:82-83`; não substituir por juiz de cobrança. |
| **B** | `app/ponto/worklist.py:19`: `dia['tipo_dia'] != 'trabalho'`; responde **há gap desenhável na ata?** (`:8-13`), devolve `None` para não classificar (`:20`). | `grade_da_celula` sem ata e `trabalha=None` produz `indefinido` (`app/escala/services/leitor_celula.py:345-346`), então não classifica; juiz retorna `False`. Migrar mudaria classificador/exibição, não cobrança. Chamado por `worklist.py:60`; produção `app/colaboradores/views.py:3017`; comando `app/ponto/management/commands/worklist_dp.py:12`. |
| **B** | `app/ponto/propositor.py:36` e `:210`: `not dia or dia.get('tipo_dia') != 'trabalho'`; respondem **degeneração para escalada/reorganização** e **template suspeito pela mediana dos deltas** (`:34-54,201-221`). | Não leem `trabalha` diretamente; não há resposta única para `None` sem identificar o produtor do dict. Com produtor leitor_celula resulta indefinido (`leitor_celula.py:345-346`); com builder grade, trabalho/sem_ancora (`utils.py:725-752`). Prod `app/ponto/management/commands/propor_reorganizacao.py:67,71`; `template_suspeito` também em `app/inteligencia/calculadores.py:325`. |
| **B** | `app/relatorios/furos.py:151`: `d.get('tipo_dia') != 'trabalho'`; `:218,:237`: `d.get('tipo_dia') != 'trabalho' or d.get('sem_ancora')`. | Perguntas de **relatório/calendário de furos e matriz Raio-X da folha** (`:129,210-214,229-233`). Leitor sem ata + `None` fornece indefinido e é excluído, contra juiz que cobra. Migração altera contagens, não estado de chamado. Prod dataset `app/relatorios/views.py:1541`; `furos_por_dia` em `app/folha/views.py:616`; chamador produção de `dias_100_em_branco`: **[nao verificado]**. |
| **C quanto a uso; B quanto à pergunta** | `app/escala/orfa_classe.py:65`: `(dia.get('tipo_dia') or 'folga') != 'trabalho'`; classifica **procedência de órfã**, devolvendo `FOLGA_DESMENTIDA` (`:53-66`). | Não lê `trabalha` diretamente. Busca nominal não encontrou chamada fora da própria definição; execução em produção **[nao verificado]**. Não é elegibilidade de cobrança. |

**Pontos relevantes de alimentação:** o leitor lavrado privilegia `ata.tipo_dia`, depois converte `trabalha=True/False/None` em `trabalho/folga/indefinido` (`app/escala/services/leitor_celula.py:345-346`); já `eh_dia_trabalho` devolve o `trabalha` cru da célula, sem consultar ata (`app/escala/models.py:1066-1073`). Portanto, gates com um ou outro insumo não são equivalentes ao juiz de cobrança (`app/chamados/juizes.py:407-408`).

## 31/10 20:49 · sessao 3017134f97d6 · V3-P5-PRORROGACAO-POR-INTERVALO · verificador · origem=Code

**A volta 2 está corrigida, mas a garantia geral sem ressalva de fuso NÃO procede.** Reproduzi uma entrada histórica, sem dupla contagem e abaixo de 26 h, em que o novo corpo desconta **360 minutos**, enquanto a janela efetiva dos prefixos cobre **354**. O problema está no cursor horário que conserva o offset inicial. (`app/ponto/motor_calculo_v2.py:180`, `app/ponto/motor_calculo_v2.py:183`)

1. **Inclinação, limites e casos de borda**

   Com offset constante e prefixos até 26 h, a premissa procede: as contribuições começam ou terminam em 00h, 05h ou 22h; a prorrogação continua linearmente. Entre essas bordas, a inclinação é constante, 0, 1 ou 2. Entrada fora da hora cheia e segundos apenas encurtam o primeiro ou último trecho. (`app/ponto/motor_calculo_v2.py:100`, `app/ponto/motor_calculo_v2.py:104`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:116`, `app/ponto/motor_calculo_v2.py:180`)

   **Universalmente, NÃO procede**, por dois motivos:

   - Acima de 26 h, o prefixo cai abruptamente para zero; esse corte pode ocorrer dentro de uma hora. O teto final mantém o desconto devolvido em zero, mas a afirmação sobre inclinação deixa de valer. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:186`)
   - Na mudança histórica de offset de São Paulo, a grade conservada pelo cursor deixa de coincidir com as horas locais das bordas. Reproduzi pelas funções reais extraídas por AST, somente em memória: (`app/ponto/motor_calculo_v2.py:79`, `app/ponto/motor_calculo_v2.py:101`, `app/ponto/motor_calculo_v2.py:180`)

   | Parâmetro | Contraprova |
   |---|---|
   | Entrada | 31/12/1913 22:00 −03:06 |
   | Saída | 01/01/1914 07:00 −03:00 |
   | Pausa | 31/12 23:00 −03:06 → 01/01 06:00 −03:00 |
   | Prorrogação | `False` |
   | Total noturno | 414 min |
   | Diferença dos prefixos da pausa | **354 min** |
   | Corpo novo | **360 min** |

   Esses resultados vêm da conversão e das medidas efetivamente executadas pelo corpo; o teto de 414 não elimina o excesso de seis minutos. (`app/ponto/motor_calculo_v2.py:162`, `app/ponto/motor_calculo_v2.py:181`, `app/ponto/motor_calculo_v2.py:186`)

   No trecho gerado `04:00−03:06 → 05:00−03:06`, o prefixo vale 360 no início, chega a 414 às `04:54−03:06` e permanece 414 até o fim. A inclinação muda de 1 para 0 dentro do trecho; o corpo transforma aumento de 54 em desconto de 60. A borda real `05:00−03:00` corresponde a `04:54−03:06`. (`app/ponto/motor_calculo_v2.py:101`, `app/ponto/motor_calculo_v2.py:180`, `app/ponto/motor_calculo_v2.py:183`)

   A contraprova **não viola os limites numéricos**: 360 é positivo e inferior tanto à duração da peça, 414, quanto ao total, 414. Em aritmética exata, união e avanço sem sobreposição limitam cada peça à sua duração; o `min` final garante desconto ≤ total. (`app/ponto/motor_calculo_v2.py:169`, `app/ponto/motor_calculo_v2.py:180`, `app/ponto/motor_calculo_v2.py:183`, `app/ponto/motor_calculo_v2.py:186`)

   **A desigualdade literal contra a duração em `float` merece ressalva:** no próprio caso de segundos, obtive desconto `60.41666666666667` e duração `60.416666666666664`, excesso de aproximadamente `7,1e-15` minuto. É arredondamento, abaixo da tolerância `1e-9` das propriedades. (`app/ponto/motor_calculo_v2.py:183`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:124`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:185`)

   Os casos da volta 2 agora dão **1.500, 1.470 e 1.455**, respectivamente. Exatamente 26 h continua admitido; acima disso e em recorte invertido, o resultado é zero. Naive local e UTC equivalente deram o mesmo resultado no caso de segundos. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:277`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:283`, `app/ponto/motor_calculo_v2.py:77`, `app/ponto/motor_calculo_v2.py:91`, `app/ponto/motor_calculo_v2.py:94`)

   Nas duas transições de horário de verão de 2018 sondadas, com pausa cobrindo o recorte, obtive: avanço de novembro, **360 sem prorrogação / 480 com**; retorno de fevereiro, **480 / 600**. A crítica de que conservar offset necessariamente quebra essas transições de uma hora **NÃO procede nesses casos**; isso não demonstra todos os históricos. (`app/ponto/motor_calculo_v2.py:100`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:180`)

2. **Recorte normal e chamadores**

   Para offset constante, prefixo sem multiplicidade, recorte ≤26 h e pausas válidas disjuntas, a medida nova coincide com a diferença de prefixos e com a janela. Identidade **bit a bit** não procede por arredondamento; somente “recorte normal” também não basta se houver pausas repetidas ou sobrepostas, porque a nova união muda deliberadamente esse resultado. A contraprova histórica acima também impede afirmar equivalência sem ressalva de fuso. (`app/ponto/motor_calculo_v2.py:164`, `app/ponto/motor_calculo_v2.py:169`, `app/ponto/motor_calculo_v2.py:181`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:293`)

   Todos os caminhos indicados continuam chegando ao mesmo corpo; nenhum recebe regra nova própria:

   | Chamador | Caminho |
   |---|---|
   | `he_noturna` | Usa `[inicio_extra, fim]`, intervalos e a mesma flag. (`app/ponto/motor_calculo_v2.py:430`, `app/ponto/motor_calculo_v2.py:432`) |
   | `_noturno_dos_intervalos` / `calcular_noturnas` | Delegação comum e subtração do desconto. (`app/ponto/motor_calculo_v2.py:963`, `app/ponto/motor_calculo_v2.py:1905`) |
   | `_ContextoNoturno` / noturnas do dia | Empresta o método por identidade. (`app/ponto/calculador/regras.py:63`, `app/ponto/calculador/regras.py:86`) |
   | HE por período | Chama `he_noturna` com as pontas, pausas e flag recebidas. (`app/ponto/calculador/regras.py:886`) |
   | `noturno_no_par` | Delega um par e os parâmetros da CCT. (`app/ponto/calculador/periodo.py:508`) |

   A perda de âncora da HE parcial permanece fora desta cura: o desconto continua ancorado em `inicio_extra`. Impacto produtivo por colaborador/rubrica: **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:430`, `app/ponto/motor_calculo_v2.py:432`)

3. **Premissa das bordas e mudança futura**

   As bordas inteiras estão no código executado de `minutos_noturnos`: 00h, 05h, 22h e meia-noite seguinte. O corpo novo lê uma propriedade estrutural dessa função; **segunda cópia da janela NÃO procede**, pois ele não reimplementa o predicado 22–05 ou seu gatilho. Contudo, depende de essa propriedade permanecer alinhada ao cursor — condição ausente do comentário geral. (`app/ponto/motor_calculo_v2.py:100`, `app/ponto/motor_calculo_v2.py:104`, `app/ponto/motor_calculo_v2.py:173`, `app/ponto/motor_calculo_v2.py:181`)

   **Há mordida se uma borda virar não inteira:** mutando apenas em memória e mantendo a referência atual, a grade longa apresentou 69 falhas com início às 22:01, 117 às 22:30 e 386 com término às 05:01. Essa proteção vem da janela 22–05 declarada pelo oráculo; não existe uma asserção estrutural específica sobre bordas inteiras. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:159`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:225`)

4. **Testes, vacuidade e relógio** [nao verificado]

   Executei **21 testes puros** isolados em memória, extraindo o AST real e substituindo o invólucro Django por `unittest`; passaram grades, sobrepostos, segundos, contraprovas e mutantes correspondentes. Isso não representa execução da suíte Django canônica, que permanece **[nao verificado]**. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:80`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:244`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:324`)

   - **Peças longas mordem:** `_limite_por_peca` produz seis falhas; grade antiga e sobrepostos produzem zero com esse mutante. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:225`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:347`)
   - **Vacuidade por `subTest` NÃO procede:** o método é chamado diretamente na instância criada, e a asserção propaga nessa execução; confirmei a mordida do teste da volta 2. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:327`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:358`)
   - O teste de segundos verifica **60 min 25 s**, mas o mutante anterior também passa nele; sua função é preservar frações, enquanto a regressão anterior é detectada pelas peças longas. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:119`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:302`)
   - `_regra` trunca minutos e declara relógio de parede para a grade sem horário de verão. **Não certifica segundos ou históricos genericamente**: no caso de segundos retorna 60, contra `60+25/60`; na contraprova de 1914 retorna 360 e deixa passar o erro. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:146`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:149`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:153`)
   - **Relógio solto NÃO procede:** datas constantes; `_aw` combina data/hora e aplica o fuso, sem consultar “agora”. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:169`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:26`)

5. **Classes reprovadas, custo e término**

   **Segundo juiz, segundo escritor, derivação paralela da janela e selo sem mordida NÃO procedem neste diff.** Há uma conversão defeituosa da medida no caso histórico identificado e uma lacuna de cobertura, embora os mutantes anteriores sejam detectados. Ordem de trava, RED histórico dos commits, suíte completa e pouso: **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:181`, `app/ponto/motor_calculo_v2.py:186`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:347`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:358`)

   Para uma única pausa de 60 min, são **3 chamadas** de `minutos_noturnos` se alinhada à hora e **5** se atravessa duas horas: duas por trecho, mais uma para o teto. Em várias peças, essa última chamada é compartilhada; o chamador ainda pode medir seu total separadamente. (`app/ponto/motor_calculo_v2.py:180`, `app/ponto/motor_calculo_v2.py:181`, `app/ponto/motor_calculo_v2.py:186`, `app/ponto/motor_calculo_v2.py:961`)

   **Laço infinito NÃO procede para datetimes válidos e operações sem exceção:** a próxima hora é estritamente posterior a `a`; como `_f > a`, `b > a`, e `a = b` faz progresso. Recortes enormes ainda percorrem todas as horas das peças, mesmo quando o teto final será zero. (`app/ponto/motor_calculo_v2.py:179`, `app/ponto/motor_calculo_v2.py:180`, `app/ponto/motor_calculo_v2.py:184`, `app/ponto/motor_calculo_v2.py:186`)

_1243 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
