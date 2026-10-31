# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 07:45** · publicado em **31/10/2026 07:46** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 07:45 · sessao 6507aa010fd3 · - · - · origem=Ronald (tmux)

Achados de selo confirmados por execução em memória, sem Django/banco:

- **Alta — `fatos_do_status=None` passa no selo.** O guard verifica apenas a presença do keyword; não prova o requisito de lavrar status junto do veredito (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:166`). A porta aceita `None` e pode conservar status anterior ou deixar status nulo (`app/ponto/portas/celula.py:695`).
- **Alta — formas inteiras de segundo escritor passam verdes:** `create`, `update_or_create(defaults=...)`, `bulk_create`, queryset em alias e atribuição em tupla seguida de `save()`. O detector limita operações/receptores e só reconhece atribuição cujo alvo imediato é `Attribute` (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:85`, `app/ponto/tests/test_selo_veredito_so_pela_porta.py:117`).
- **Média — escrita isolada de `veredito_em`/`protestos` por `save(update_fields=...)` passa.** Esses campos estão em `CAMPOS`, porém ausentes de `CAMPOS_DO_SAVE` (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:16`, `app/ponto/tests/test_selo_veredito_so_pela_porta.py:17`).
- **Média — migrations e testes não participam da garantia; dentro da porta só se fiscaliza `save()` sem `update_fields`.** Os exemplos novos mordem formas literais específicas, sem garantir escritor único universal (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:112`, `app/ponto/tests/test_selo_veredito_so_pela_porta.py:173`, `app/ponto/tests/test_selo_veredito_so_pela_porta.py:259`).

Censo de produção e índice de 114 arquivos candidatos a fixtures de testes enviados nas mensagens anteriores. O índice de fixtures usa heurísticas AST e não deve ser apresentado como prova exaustiva de tipos; inventário completo das leituras/assertions de testes [não verificado]. Nenhuma escrita direta persistida desses campos fora da porta foi encontrada no censo estático; completude diante de aliases/dicionários dinâmicos [não verificado]. [nao verificado]

Correção ao meu primeiro levantamento: `protestos` nasce **NULL**, como os demais campos antigos; não nasce `[]` (`app/escala/models.py:1741`, `app/escala/migrations/0036_b5_celula_veredito.py:16`).

## 31/10 07:43 · sessao 51914975b57e · - · - · origem=Ronald (tmux)

Achados demonstráveis na cópia:

- **Alta — D5 pode deixar status incompatível com decisão admin por concorrência.** O command materializa células sem trava em `app/ponto/management/commands/lavrar_status_competencia.py:53`; `_completar_status` usa o veredito daquela instância em `app/ponto/services/cartorio.py:733`. Se o admin gravar `faltou/admin/falta` depois da leitura, a instância antiga ainda pode gerar `ok` e salvar somente status em `app/ponto/portas/celula.py:753` e `app/ponto/portas/celula.py:760`. O `atomic` envolve o save, sem releitura ou comparação do estado persistido, em `app/ponto/portas/celula.py:639`. Resultado possível: `faltou/admin` junto de `ok`.

- **Alta — retirar a ausência da decisão admin não desfaz a palavra da célula.** O service cria ausência aprovada em `app/ponto/services/veredito_celula.py:315` e lavra admin em `app/ponto/services/veredito_celula.py:330`. Cancelar a ausência muda seu estado para rejeitada em `app/ponto/views.py:2724`; o signal rejulga em `app/ponto/signals.py:147`, mas a porta preserva a decisão admin em `app/ponto/portas/celula.py:738` e deriva novamente o status dela em `app/ponto/portas/celula.py:694`. Assim, `justificado/ausencia` ou `faltou/falta` sobrevivem à retirada da cobertura. Porta explícita de revogação desse veredito: **[não verificado]**.

- **Alta — feriado não invalida todas as células afetadas.** O evento de feriado chama somente reavaliação de chamados em `app/chamados/signals.py:265`; o universo contém apenas `batida_ausente` vivos em `app/chamados/services/feriado_reavaliar.py:17`. Sem chamado elegível, não ocorre o rejulgamento indireto. Remoção do feriado não tem receiver, explicitamente documentado em `app/chamados/signals.py:261`. Status já lavrado pode ficar velho.

- **Média — regeneração não garante atualização do status.** O rejulgamento depende de `recalculo=True`, exclui decisões humanas e dias futuros em `app/ponto/portas/celula.py:360` e `app/ponto/portas/celula.py:369`; suas falhas são absorvidas em `app/ponto/portas/celula.py:375`. `reverter_regeneracao_dia` restaura DNA/trabalho em `app/ponto/portas/celula.py:530` e salva esses campos em `app/ponto/portas/celula.py:538`, sem relavrar o status nesse caminho.

- **Média — “uma linha de trilha” não vale para o service admin.** `app/ponto/services/veredito_celula.py:330` chama a porta, que registra `veredito_celula` em `app/ponto/portas/celula.py:777`; depois, o service registra novamente a mesma ação em `app/ponto/services/veredito_celula.py:334`. O adaptador efetivamente chama `evento` em `app/core/auditoria.py:22`. A repetição sem alteração ainda alcança o segundo registro. O teste E3 repete somente o cartório e compara delta de contagem em `app/ponto/tests/test_e3_status_lavrado.py:172`.

- **Média — ramo protegido pelo admin escreve sem LogAuditoria da lavra.** Pode atualizar ata/status e retornar em `app/ponto/portas/celula.py:738` a `app/ponto/portas/celula.py:744`, antes dos registros de auditoria dos demais ramos.

- **Média — trilha incompleta e sem garantia absoluta.** Mudança exclusiva de `status_rotulo` é detectada em `app/ponto/portas/celula.py:703`, mas os payloads registram somente `status_dia` em `app/ponto/portas/celula.py:722` e `app/ponto/portas/celula.py:764`. O registro é best effort em `app/core/observ.py:105`, podendo terminar sem linha em `app/core/observ.py:139`. Com `save=False`, a auditoria também ocorre antes de `carimbar` persistir: `app/ponto/portas/celula.py:774`, `app/ponto/portas/celula.py:777` e `app/ponto/portas/celula.py:807`.

Comportamentos confirmados:

- Mesma entrada sequencial na mesma instância não escreve novamente quando ata, veredito, via, protestos e status coincidem: `app/ponto/portas/celula.py:735`, `app/ponto/portas/celula.py:753` e `app/ponto/portas/celula.py:755`.
- `veredito_em` anda no ramo completo em `app/ponto/portas/celula.py:772`; alteração exclusiva de status retorna antes disso em `app/ponto/portas/celula.py:766`.
- Outbox acorda por `veredito` ou `ata`, não por status: `app/ponto/portas/celula.py:623` e `app/ponto/portas/celula.py:638`. O consumidor chama `retratar_chamados_absolvidos_por_celula` e `encerrar_orfas_pela_lampada`: `app/chamados/reconciliador.py:1304` e `app/chamados/reconciliador.py:1307`.
- Batidas e ausências via save/delete acionam cartório: `app/ponto/signals.py:131`, `app/ponto/signals.py:138`, `app/ponto/signals.py:143` e `app/ponto/signals.py:150`. Raw/bulk estão excluídos desse mecanismo: `app/ponto/signals.py:83`.
- Folga manual atravessa regeneração: `app/escala/folgas.py:257`, `app/escala/folgas.py:261` e `app/escala/folgas.py:220`.

Censo de escrita: os escritores persistentes encontrados são `_aplicar_status`, `lavrar_veredito`, `carimbar_precedencia` e a persistência por `salvar_e_emitir`: `app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:768`, `app/ponto/portas/celula.py:769`, `app/ponto/portas/celula.py:820` e `app/ponto/portas/celula.py:640`. A sombra de ata fora da porta é cópia em memória: `app/escala/utils.py:1826`. Os testes E3 fazem escrita direta de status em `app/ponto/tests/test_e3_status_lavrado.py:197` e `app/ponto/tests/test_e3_status_lavrado.py:210`.

Segundo escritor persistente fora da porta, fixture de command escrevendo esses campos, migration de dados escrevendo seus valores ou `CelulaDia.save()` sem `update_fields`: **[não verificado]**; as buscas textuais e AST efetuadas não encontraram caso concreto.

## 31/10 07:42 · sessao c89ee4469920 · - · - · origem=Ronald (tmux)

Achados confirmados em leitura e testemunhas puras por AST, sem Django/banco:

1. **Alta — o status pode absolver a cobertura parcial enquanto o veredito acusa.** A grade conserva `tipo_dia='ausencia'` mesmo quando a cobertura é parcial e conserva as lâmpadas que faltam (`app/escala/utils.py:1180`, `app/escala/utils.py:1184`, `app/escala/utils.py:1190`). O juiz distingue cobertura parcial de integral e pode emitir `FURO_SEM_COBRANCA` pelo restante (`app/ponto/supra_juiz.py:243`, `app/ponto/supra_juiz.py:279`, `app/ponto/supra_juiz.py:334`), convertido em `furo` (`app/escala/models.py:1580`). Entretanto, `fatos_do_status` perde `cobertura_parcial`/`periodo_parcial` (`app/ponto/status_dia.py:103`), e `status_da_lavra` retorna `ausencia` antes de consultar o veredito (`app/ponto/status_dia.py:71`). Testemunha pura: `tipo_dia='ausencia', previsto_min=480, tem_batida=False, isento=False, veredito='furo'` → `ausencia` (`app/ponto/status_dia.py:68`).

2. **Alta — dia sem vínculo válido, com célula de trabalho, pode gravar `indefinida` e `folga` no mesmo ato.** A alimentação detecta célula de trabalho sem vínculo válido (`app/ponto/supra_juiz.py:168`), a grade chega como `folga`, previsto zero (`app/ponto/supra_juiz.py:222`), e o juiz emite `PREVISTO_COM_DIA_INDEFINIDO` mesmo sem previsto (`app/ponto/supra_juiz.py:405`), convertido em `indefinida` (`app/escala/models.py:1583`). O status retorna `folga` antecipadamente (`app/ponto/status_dia.py:73`), tornando inalcançável o tratamento `escala_suspeita` (`app/ponto/status_dia.py:77`). Testemunha pura reproduziu `(indefinida, folga)` (`app/ponto/status_dia.py:73`).

3. **Alta — FolgaDia com batida única pode perder o fato e virar `folga`.** O overlay elimina todas as células e órfãs (`app/escala/utils.py:1118`). A autoridade do realizado soma zero quando há apenas entrada, sem fim fechado (`app/ponto/turnos.py:731`, `app/ponto/turnos.py:736`, `app/ponto/turnos.py:769`); a grade materializa esse realizado (`app/escala/utils.py:1985`, `app/escala/utils.py:1998`). `fatos_do_status` só reconhece batida pelas células/órfãs remanescentes ou por realizado positivo (`app/ponto/status_dia.py:101`). Assim, após apagar o fato visual, retorna `tem_batida=False`, e a lavra grava `folga` (`app/ponto/status_dia.py:73`). Depois que o aberto expirar, o leitor exibirá esse status persistido (`app/ponto/status_dia.py:125`, `app/ponto/status_dia.py:131`).

Quanto ao juiz do encerramento, **não há segundo juiz entre `dia_encerrado` e `dias_encerrados`**: o primeiro delega a `dia_encerrado_motivo` (`app/ponto/turnos.py:2058`), que usa `_dia_encerrado_motivo_puro` (`app/ponto/turnos.py:2079`); o carregador `dias_encerrados` chama exatamente a mesma função (`app/ponto/turnos.py:2137`). `status_do_dia_lavrado` consulta esse carregador corretamente (`app/ponto/status_dia.py:126`).

Existe, porém, **exceção nova no leitor**: `MOTIVO_SEM_PREVISTO` significa que o juiz não afirma encerramento (`app/ponto/turnos.py:2103`), mas o leitor o ignora para devolver a coluna lavrada (`app/ponto/status_dia.py:127`). O texto original D4 manda vocabulário de relógio antes do encerramento (`app/docs/ADR/0002-celula-e-o-dia.md:21`); a exceção está apenas na documentação do novo leitor (`app/ponto/status_dia.py:113`). Registrar como desvio contratual, observando que isso também torna visível o erro do item 2 (`app/ponto/status_dia.py:131`).

Não demonstrado defeito na dependência da migration: 0044 depende de 0043 (`app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:8`), que cria `EventoCelula` (`app/escala/migrations/0043_eventocelula.py:11`). Aplicação efetiva da migration antes do código: **[não verificado]**. A regra exige migrar antes e bloqueia deploy com migration pendente (`CLAUDE.md:138`, `CLAUDE.md:740`).

A tabela `JUIZES.md` não recebeu a nova autoridade: seu catálogo de célula termina no furo (`app/docs/JUIZES.md:72`), enquanto o registro vivo contém o status (`app/core/juizes.py:173`); a própria tabela exige correção no mesmo commit quando diverge (`app/docs/JUIZES.md:14`). Gravidade baixa, documentação.

## 31/10 07:42 · sessao f877b3d9d8f5 · F1C4-3 · verificador · origem=Code

**1. Sim, o teste afirma a cláusula (1) e, pela leitura do caminho, morde a regressão.** Ele cria cobrança aberta, cria célula com `trabalha=None`, executa o comando com `apply=True` e exige status final `aberto` ou `pendente`. Apenas a montagem da grade está mockada; a premissa permanece real. [app/ponto/tests/test_a2_marco_morto.py:33](app/ponto/tests/test_a2_marco_morto.py#L33) [app/ponto/tests/test_a2_marco_morto.py:46](app/ponto/tests/test_a2_marco_morto.py#L46)

O caminho é:

- A grade simulada entrega `sem_ancora=True`; o comando inclui essas datas em `indefinidos`, depois em `mortos`, e solicita `retratar`. Portanto, o teste efetivamente chega à tentativa de fechamento. `app/ponto/tests/test_a2_marco_morto.py:30`; `app/ponto/management/commands/reconciliar_grade.py:139`, `:274`, `:284`, `:301`.
- `retratar → _retratar → ch.reconciliar → _decisao_do_fechamento → _gate_premissa_do_fechamento → premissa_regularizada(para='folha')`. `app/chamados/reconciliador.py:548`, `:525`; `app/chamados/models.py:615`, `:646`, `:699`.
- **Código atual:** `dia_sem_trabalho` retorna `False` para `None` sem ata. A premissa não regulariza por esse atalho; `_celula_furada` retorna `None` porque não há lâmpadas lavradas. O coletor então consulta os fatos, incluindo ausência de batidas. `app/chamados/juizes.py:408`, `:452`; `app/chamados/models.py:1232`, `:1247`, `:1257`.
- **Mutante `trabalha is not True`:** retorna `True` para essa célula; `premissa_regularizada` retorna imediatamente `True`; o gate permite fechar; `RECONCILIAR` leva `aberto → resolvido`. A asserção final rejeita `resolvido`. `app/chamados/models.py:1232`, `:699`, `:618`; `app/chamados/catalogo/motor.py:111`; `app/ponto/tests/test_a2_marco_morto.py:50`.

A cláusula (2), sobre ata declarando dia diferente de trabalho, **não é exercitada pelo teste alterado**: sua célula nasce sem ata fornecida, e `ata` é campo anulável. `app/ponto/tests/test_a2_marco_morto.py:46`; `app/escala/models.py:1746`.

Execução do teste completo e comprovação empírica verde/vermelho: **[não verificado]**. Comparação histórica dos dois commits: **[não verificado]**. [nao verificado]

**2. Existe um positivo com `None`, mas sustentado pela ata de folga.** O teste exige literalmente `dia_sem_trabalho(_cel(None, 'folga')) is True`, em conformidade com a cláusula (2). [app/chamados/tests/test_f1c4_3_none_cobra.py:47](app/chamados/tests/test_f1c4_3_none_cobra.py#L47)

Não encontrei outro teste que faça **`None` sozinho** calar cobrança ou fechar chamado por esses leitores; garantia absoluta de ausência de qualquer caso indireto: **[não verificado]**. Os testes explícitos encontrados exigem o contrário: [nao verificado]

| Caso | Asserção | Local |
|---|---|---|
| `None` + ata de trabalho | `dia_sem_trabalho=False` | `app/chamados/tests/test_f1c4_3_none_cobra.py:40` |
| `None` + ata sem `tipo_dia` | `dia_sem_trabalho=False` | `app/chamados/tests/test_f1c4_3_none_cobra.py:43` |
| `None` + lâmpada apagada | `_celula_furada=True` | `app/chamados/tests/test_f1c4_3_none_cobra.py:94`, `:95` |
| `None` + ata de trabalho | `premissa_regularizada=False` | `app/chamados/tests/test_f1c4_3_none_cobra.py:107`, `:109` |
| `None`, outro gate de emissão | `celula_veta_cobranca=None`, sem veto | `app/ponto/tests/test_t31_gate_celula.py:25`, `:34` |

Dois semelhantes merecem distinção:

- `test_tripwire_ata_fantasma` usa `None + ata folga`, mas afirma contagem pelo detector de ata fantasma, sem afirmar encerramento pelos leitores perguntados. `app/ponto/tests/test_tripwire_ata_fantasma.py:60`, `:62`, `:78`, `:81`.
- `test_selo_limbo_cala_o_dia` afirma silêncio por `limbo_folgas.pode_calar` sobre vínculo sem folga definida; não prepara célula `trabalha=None` para os três leitores perguntados. `app/ponto/tests/test_selo_limbo_cala_o_dia.py:37`, `:46`, `:73`, `:77`.

**3. Escritores encontrados e leitores centrais dos campos:**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| **`CelulaDia.trabalha`** | No teste, a fixture grava `None`. Na produção, a porta da célula gera, regenera, neutraliza célula fora da vigência e recompõe o valor na reversão. `app/ponto/tests/test_a2_marco_morto.py:46`; `app/ponto/portas/celula.py:231`, `:247`, `:260`, `:532`, `:595`, `:612`. | No caminho auditado, `dia_sem_trabalho`, chamado pela premissa e por `_celula_furada`. Outros leitores: `eh_dia_trabalho`, precedência, guarda dos marcos e veto de emissão. `app/chamados/juizes.py:408`, `:443`; `app/chamados/models.py:1232`; `app/escala/models.py:1072`; `app/ponto/precedencia.py:290`; `app/escala/servico_jornada.py:303`; `app/ponto/marcos.py:155`. |
| **`CelulaDia.ata`** | O cartório produz o conteúdo por `ata_do_dia` e entrega à lavra; a porta atribui e persiste. `carimbar_precedencia` também escreve a chave `precedencia`. `app/ponto/services/cartorio.py:558`, `:566`, `:1520`, `:1535`; `app/ponto/portas/celula.py:711`, `:725`, `:640`, `:775`. | `dia_sem_trabalho` lê `tipo_dia`; `_celula_furada` lê lâmpadas e órfãs; a premissa também consulta lastro dos marcos. Outros consumidores incluem prévia e validação PDF. `app/chamados/juizes.py:407`, `:452`, `:455`, `:457`; `app/chamados/models.py:1234`; `app/ponto/services/lastro.py:89`; `app/folha/previa.py:83`; `app/folha/validacao_pdf.py:47`. |
| **`ChamadoColaborador.status_local`** | Nasce pelos gates `abrir`/`criar`; transições atribuem em `_gravar_estado` e persistem no seu `save`. Neste caminho, `reconciliar` chama esse escritor. `app/chamados/models.py:897`, `:1046`, `:222`, `:279`, `:618`. | O teste relê do banco e verifica o status; `reconciliar_grade` seleciona vivos; o gate consulta o motor; C6, C8 e cartório também consomem o estado. `app/ponto/tests/test_a2_marco_morto.py:49`; `app/ponto/management/commands/reconciliar_grade.py:96`, `:207`; `app/chamados/models.py:637`; `app/chamados/juizes.py:719`, `:1780`; `app/ponto/services/cartorio.py:335`. |

`ata` é campo persistido, mas há projeções somente em memória: a célula pode receber a ata do julgamento corrente para leitura, e a montagem da grade também produz uma projeção. Assim, atribuição a `.ata` não implica, sozinha, escrita no banco. `app/escala/models.py:1746`; `app/chamados/juizes.py:394`; `app/ponto/portas/celula.py:678`; `app/escala/utils.py:1827`.

Exaustividade de todos os consumidores desses campos no repositório: **[não verificado]**.

## 31/10 07:41 · sessao 3a0fd74c031b · - · - · origem=Ronald (tmux)

Por leitura estática, **sim: o teste morde a volta de `trabalha is not True`**. Execução da suíte e mutante: [não verificado]. [nao verificado]

Caminho:

1. O teste cria chamado `batida_ausente` aberto, célula `trabalha=None` sem ata e mocka somente a montagem da grade; ao final exige `status_local` em `('aberto', 'pendente')`. (app/ponto/tests/test_a2_marco_morto.py:37–50; app/ponto/management/commands/reconciliar_grade.py:54)
2. `sem_ancora` coloca a data em `indefinidos`; ela entra em `mortos` e o command chama `retratar`. (app/ponto/management/commands/reconciliar_grade.py:139–150,274–301)
3. `retratar → _retratar → ch.reconciliar → _decisao_do_fechamento → _gate_premissa_do_fechamento`. (app/chamados/reconciliador.py:525,546–548; app/chamados/models.py:615,646)
4. O gate pergunta `premissa_regularizada(para='folha')`. No código atual, `dia_sem_trabalho` responde `False` para `None` sem ata; `_celula_furada` responde `None` porque faltam `lampadas`; o coletor recorre aos fatos e encontra falta descoberta, pois a fixture não cria batidas. Assim a premissa segue viva e o gate recusa o fechamento. (app/chamados/models.py:699,1232–1264,728–732; app/chamados/juizes.py:408,452–453; app/ponto/tests/test_a2_marco_morto.py:19–48)
5. No mutante, `dia_sem_trabalho=True` faz `premissa_regularizada` retornar `True` diretamente em `models.py:1232–1233`; o gate permite, `reconciliar` grava `resolvido`, e a asserção da linha 50 falha. (app/chamados/models.py:699–700,618; app/chamados/catalogo/motor.py:111; app/ponto/tests/test_a2_marco_morto.py:50)

Sinais não invalidam essa conclusão: o nascimento do chamado aciona o cartório, porém o cartório retorna sem criar célula quando ela ainda inexiste; a célula é criada depois, expressamente pelo teste. (app/chamados/signals.py:164–180; app/ponto/services/cartorio.py:1658–1660; app/ponto/tests/test_a2_marco_morto.py:37–46)

A fixture usa escala semanal com trabalho nos sete dias e o campo `trabalha_em_feriado` tem default `True`; portanto não encontrei dependência de fim de semana ou feriado que torne a premissa regularizada nessa fixture. (app/chamados/tests/_vinculo_do_dia.py:18–26; app/escala/models.py:896–898; app/ponto/precedencia.py:314–324)

Limite de cobertura: esse teste afirma sobrevivência de **cobrança já existente** com `None` e ata ausente; não testa emissão nova nem a cláusula de ata com `tipo_dia != 'trabalho'`. (app/ponto/tests/test_a2_marco_morto.py:37–50)

## 31/10 07:41 · sessao f1b5c3353445 · - · - · origem=Ronald (tmux)

Mapa confirmado dos campos, com escritores reais e leitores relevantes ao delta:

| Campo | Natureza | Quem escreve | Quem lê |
|---|---|---|---|
| `CelulaDia.trabalha` | `BooleanField(null=True)`, sem `default` explícito; `None=indeterminado (limbo cobra)`. `app/escala/models.py:1654` | Porta `ponto/portas/celula.py`: regeneração fora da vigência põe `False` (:231); regeneração cria (:247) ou atualiza (:260, salva :264); reversão recompõe pelo DNA (:532, salva :538); geração cria (:595) ou atualiza (:612, salva :615). | Juiz `dia_sem_trabalho`: `is False` (:408 de `app/chamados/juizes.py`); `EscalaColaborador.eh_dia_trabalho` devolve o valor preservando `None` (`app/escala/models.py:1072`); precedência (`app/ponto/precedencia.py:290`, :300, :306); guarda dos marcos (`app/escala/servico_jornada.py:303`); veto da emissão (`app/ponto/marcos.py:155`, :159). |
| `CelulaDia.ata` | Campo persistido `JSONField(null=True, blank=True)`, não property. `app/escala/models.py:1746` | O produtor do conteúdo é `cartorio.ata_do_dia`, incluindo `tipo_dia`, `lampadas`, `sem_ancora` etc. (`app/ponto/services/cartorio.py:558`, :566). Cartório envia à lavra (:1518, :1520); método do modelo delega (`app/escala/models.py:1781`). Porta `lavrar_veredito` atribui (`app/ponto/portas/celula.py:711`, :725); preserva chave `precedencia` (:703); `carimbar_precedencia` grava essa chave (:775, :776). Persistência em `salvar_e_emitir` (:640); cartório pode lavrar com `save=False` e persistir junto com carimbo (:1535 de `cartorio.py`, :762 de `portas/celula.py`). | `dia_sem_trabalho` lê `tipo_dia` (`app/chamados/juizes.py:407`); `_celula_furada` lê a ata e `lampadas`/órfãs (:442, :452, :455, :457); premissa chama esses leitores (`app/chamados/models.py:1232`, :1247). Lastro lê lâmpadas (`app/ponto/services/lastro.py:89`, :113, :128, :149); apresentação por célula (`app/folha/previa.py:83`); validação PDF (`app/folha/validacao_pdf.py:47`). |
| `ChamadoColaborador.status_local` | `CharField(..., default='aberto')`. `app/chamados/models.py:46` | Nascimento por `abrir`: monta status vindo do motor (:897) e cria pelo gate; `criar`: status definido em :1022, grava em :1046. Mudança de estado em `_gravar_estado`: atribui :222, persiste :279. `reconciliar` passa por decisão :615 e chama escritor :618. Há simulação temporária somente em memória para julgamento, restaurada em `finally` (:339, :352). | O teste atualiza instância do banco e verifica aberto/pendente (`app/ponto/tests/test_a2_marco_morto.py:49`, :50). Reconciliador seleciona chamados abertos (`app/ponto/management/commands/reconciliar_grade.py:96`, :207), lê encerrados para reabrir (:521). Gate consulta motor pelo estado (`app/chamados/models.py:637`); C6 usa estado do destino (`app/chamados/juizes.py:719`); C8 filtra vivos (:1780). Cartório usa o estado na impressão (`app/ponto/services/cartorio.py:335`) e filtra vivos (:1100). |

A geração é chamada pelo command `gerar_celulas` (`app/ponto/management/commands/gerar_celulas.py:311`); o command reexporta a porta (:13). Signals de escala também acionam regeneração (`app/escala/signals.py:71`, :117).

No teste, o escritor de `trabalha=None` é a própria fixture (`app/ponto/tests/test_a2_marco_morto.py:46`); ela não fornece `ata`, e a grade mockada não é uma gravação de ata (`app/ponto/tests/test_a2_marco_morto.py:26`, :30, :47`). Existe também substituição de ata apenas em memória no fluxo normal: `celula_do_dia_agora → celula_deste_julgamento → celula_como_o_julgamento_deixa` (`app/chamados/juizes.py:378`, :394; `app/ponto/portas/celula.py:678`, :680); e uma projeção em memória na montagem da grade (`app/escala/utils.py:1827`). Portanto, “só lavrar_veredito escreve ata” precisa distinguir persistência, chave `precedencia` e projeções em memória (`app/ponto/portas/celula.py:640`, :775, :680).

Este é o mapa dos escritores encontrados e dos leitores centrais/relevantes, não um censo exaustivo de todos os consumidores do repositório. [não verificado]

## 31/10 07:41 · sessao 1a64a44db582 · - · - · origem=Ronald (tmux)

Na busca global e inspeção dos testes, não encontrei outro teste que faça **`trabalha=None`, por si só**, calar cobrança ou regularizar premissa nesses leitores; ausência absoluta de caso indireto: [não verificado]. [nao verificado]

O positivo explícito é **`None` com ATA de folga**, autorizado pela cláusula 2: `dia_sem_trabalho(_cel(None, 'folga')) is True` em `app/chamados/tests/test_f1c4_3_none_cobra.py:47`.

As proteções contrárias são:

- `None` + ATA de trabalho → `False`: `app/chamados/tests/test_f1c4_3_none_cobra.py:40`.
- `None` + ATA sem `tipo_dia` → `False`: `app/chamados/tests/test_f1c4_3_none_cobra.py:43`.
- `None` + lâmpada apagada → `_celula_furada=True`: preparação em `app/chamados/tests/test_f1c4_3_none_cobra.py:94`, asserção em `:95`.
- `None` + ATA de trabalho → `premissa_regularizada=False`: preparação em `app/chamados/tests/test_f1c4_3_none_cobra.py:107`, asserção em `:109`.
- Outro gate também exige que `None` não vete cobrança: célula em `app/ponto/tests/test_t31_gate_celula.py:25`, asserção em `:34`.

Casos parecidos que **não exercitam esses leitores**:

- `None` + ATA folga é contabilizado pelo detector de ATA fantasma, sem asserção de encerramento: `app/ponto/tests/test_tripwire_ata_fantasma.py:60`, `:62`, `:78`, `:81`.
- Dia duvidoso isolado pode ser calado por `limbo_folgas.pode_calar`; o teste usa vínculo 6x1 sem folga, sem criar `CelulaDia`: `app/ponto/tests/test_selo_limbo_cala_o_dia.py:37`, `:46`, `:73`, `:77`.
- Testes próximos de fechamento por folga usam **`False`**, não `None`: `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:197`, `:201`, `:214`, `:215`; `app/ponto/tests/test_t32_folga_sem_turno.py:24`, `:31`.

## 31/10 07:35 · sessao 45b4164c7701 · F1C4-3 · verificador · origem=Code

**Não aprovaria a obra como “predicado único”.** A troca atende ao aval dentro de `dia_sem_trabalho`, mas altera a escolha entre dia e véspera, mantém decisões paralelas em produção e deixa um teste legado afirmando a regra revogada. (`app/chamados/juizes.py:408`, `app/chamados/juizes.py:443`, `app/ponto/services/cartorio.py:1167`, `app/ponto/tests/test_a2_marco_morto.py:38`)

**(a) Escritores, leitores e alcance da mudança**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `CelulaDia.trabalha` | A porta da célula: geração e regeneração recebem `eh_dia_trabalho_calculado`; também existem saneamento fora da vigência e reversão de regeneração. (`app/ponto/portas/celula.py:590`, `:595`, `:243`, `:260`, `:231`, `:532`) | O juiz registrado do vínculo devolve o valor ternário; `dia_sem_trabalho` interpreta esse valor junto da ata; `_protesta` faz sua própria seleção. (`app/escala/models.py:1072`, `app/chamados/juizes.py:408`, `app/ponto/services/cartorio.py:1167`) |
| `ata['tipo_dia']` | `cartorio.ata_do_dia` copia o tipo produzido pela grade; a porta persiste a ata. (`app/ponto/services/cartorio.py:566`, `app/ponto/portas/celula.py:725`, `:732`) | `dia_sem_trabalho`, leitor da célula e consumidores da grade, inclusive folha. (`app/chamados/juizes.py:408`, `app/escala/services/leitor_celula.py:344`, `app/folha/export.py:1071`) |
| `ata['lampadas']` | `lampadas_do_dia`, incorporado por `ata_do_dia`; dispensa acende lâmpadas e teto temporal marca pendentes com `acesa=None`. A persistência passa pela mesma porta. (`app/ponto/services/cartorio.py:427`, `:564`, `:617`, `:713`; `app/ponto/portas/celula.py:732`) | `_celula_furada`, lastro por marco, apresentação e folha. (`app/chamados/juizes.py:455`, `app/ponto/services/lastro.py:175`, `app/escala/services/leitor_celula.py:93`, `app/folha/previa.py:84`) |

Há atas projetadas **em memória**, sem constituir escritor persistente adicional: a grade reutiliza `lampadas_do_dia`, e a leitura do julgamento atual recebe uma cópia pela porta. (`app/escala/utils.py:1818`, `:1826`, `:1832`; `app/chamados/juizes.py:394`; `app/ponto/portas/celula.py:678`)

O efeito nas três funções é:

- **`dia_sem_trabalho`:** somente `None` com tipo ausente/vazio ou `'trabalho'` muda de `True` para `False`; a cláusula da ata permanece. Isso corresponde às duas cláusulas literais do aval. (`app/chamados/juizes.py:407`, `:408`)
- **`_celula_furada`:** nesses casos, deixa de recursar à véspera e passa a consultar as lâmpadas do próprio dia. Portanto, a mudança **não é monotônica no sentido de cobrar mais**. (`app/chamados/juizes.py:443`, `:446`, `:452`, `:455`)
- **`premissa_regularizada`:** perde o retorno antecipado de dia sem trabalho e passa pelos marcos, pelo furo e eventualmente pelos fallbacks. Justificativa aprovada e decisão administrativa continuam podendo regularizar antes disso. (`app/chamados/models.py:1214`, `:1230`, `:1232`, `:1236`, `:1245`, `:1247`, `:1253`)

**Contraexemplo relevante:** D tem `trabalha=None`, ata de trabalho e todas as lâmpadas acesas; D−1 tem lâmpada apagada, sem cobertura de ausência. Antes, `_celula_furada(D)` consultava D−1 e retornava `True`; depois retorna `False` pelas lâmpadas de D. Para `turno_aberto*`, o coletor deliberadamente não usa o atalho dos demais módulos: chega a essa mudança e pode considerar regularizado um chamado cuja véspera continua furada. É uma consequência estática do código; ocorrência em produção **[nao verificado]**. (`app/chamados/juizes.py:443`, `:446`, `:455`, `:465`; `app/chamados/models.py:1232`, `:1247`)

O aval não explicita retirar a consulta à véspera nesse cenário, e o RED novo não o cobre; já existe teste exigindo que um `turno_aberto` datado na folga continue vivo pela véspera furada, mas usando `trabalha=False`, não `None`. (`app/chamados/tests/test_f1c4_3_none_cobra.py:93`, `:105`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:65`, `:66`, `:75`, `:88`)

Também há um defeito **preexistente** nessa recursão: ela não transmite `para=para`; uma consulta para folha volta ao default cobrança na véspera, onde ausência aberta pode cobrir. Ao evitar a recursão em D=None, o diff também muda qual política de cobertura participa. (`app/chamados/juizes.py:411`, `:424`, `:426`, `:446`)

Os leitores indiretos afetados incluem:

| Porta/leitor | Consequência possível, se os demais requisitos permitirem |
|---|---|
| S122 humano | Premissa antes regularizada pode passar a impedir resolução com `ValueError`. (`app/chamados/models.py:175`, `:177`, `:185`) |
| Fechamento pelo sistema | Pode recusar reconciliação; permanecem as exceções de agrupamento e deduplicação. (`app/chamados/models.py:646`, `:699`, `:709`) |
| Reabertura | Pode deixar de recusar por premissa regularizada; veto do dia e fechamento administrativo sem fato novo continuam anteriores. (`app/chamados/models.py:1285`, `:1290`, `:1296`) |
| Emissor que encontra encerrado | `abrir` pede renascimento com os marcos do **novo pedido**; passa por `reabrir`, podendo gravar estado e ressuscitar perguntas. (`app/chamados/models.py:880`, `:882`, `:978`, `:1235`, `:1380`, `:1389`) |
| Defesa 1 | O fechamento do pai passa pelo mesmo `reconciliar` afetado. (`app/chamados/signals.py:42`) |

Esses efeitos são alcançados pelo coletor comum; não são automaticamente violações do aval. A lacuna é certificá-los apenas com chamadas diretas ao juiz/coletor, sem testar os portões e o caso da véspera. (`app/chamados/tests/test_f1c4_3_none_cobra.py:40`, `:95`, `:109`)

**(b) Mesma pergunta ou perguntas distintas**

**`_protesta` inteiro responde outra pergunta:** “este chamado deve participar como protesto neste julgamento?”. Inclui a satisfação dos marcos específicos, cujo juiz é `marcos_acesos`. Essa parte não equivale a “dia sem trabalho”. (`app/ponto/services/cartorio.py:1167`; `app/ponto/services/lastro.py:153`, `:170`)

**Mas contém uma subpergunta duplicada e divergente:**

```text
dia_sem_trabalho: trabalha é False OU ata.tipo_dia não trabalho
_protesta:       trabalha não é False E marcos não comprovadamente acesos
```

Para `trabalha=True` ou `None`, ata `'folga'` e marco apagado/sem marco: `dia_sem_trabalho=True`, enquanto `_protesta=True`. A dispensa pelo tipo lavrado é reconhecida num sítio e ignorada no outro. (`app/chamados/juizes.py:408`; `app/ponto/services/cartorio.py:1167`; `app/ponto/services/lastro.py:172`, `:179`)

Isso interfere na entrada de `classificar_dia`, na lâmpada `'cobrado'`, no adiamento e na seleção para reconciliação. A divergência **já existia**; o diff alinha o caso None/ata trabalho, mas não realiza a promessa de predicado único. (`app/ponto/services/cartorio.py:1168`, `:1170`, `:1206`, `:1236`, `:1237`, `:1242`)

Outros sítios ainda classificam None como folga/sem trabalho:

- Relatório soma em `folga` e deixa de examinar a acusação. (`app/relatorios/dia_da_frota.py:47`)
- Frase para admin/IA afirma “Dia sem trabalho previsto: nenhuma marca era esperada”. (`app/ponto/services/dia_do_colab.py:121`)
- Passivo classifica como `FOLGA`, classe selecionada por `a_retratar`. (`app/ponto/services/passivo_s84.py:96`, `:116`)
- `reconciliar_grade` mantém a derivação “previsto não sabido = premissa morta”, inclui indefinidos entre mortos e fornece fatos `False` literais antes de tentar retratar; o gate posterior pode recusar. (`app/ponto/management/commands/reconciliar_grade.py:272`, `:275`, `:292`, `:300`; `app/chamados/models.py:699`)

Há também **perguntas legitimamente distintas**: o juiz registrado do vínculo responde o valor ternário da previsão; `dia_sem_trabalho` acrescenta a decisão lavrada na ata; fabricar pergunta ou autorizar plantio exige evidência positiva e não constitui, por si, afirmação de folga. (`app/core/juizes.py:158`; `app/escala/models.py:1072`; `app/chamados/juizes.py:408`; `app/chamados/services/fabrica_da_celula.py:78`; `app/chamados/services/materializacao.py:158`)

Uma tensão adicional é preexistente: a grade pode lavrar `'ausencia'` **parcial**, preservando as células, enquanto `classificar_falta` não cala por ausência parcial. O aval manda manter a cláusula ampla da ata; não atribuo essa divergência à alteração proposta. (`app/escala/utils.py:1180`, `:1190`, `:1195`; `app/escala/servico_jornada.py:313`, `:344`; `app/chamados/juizes.py:408`)

**(c) Teste existente que afirma o comportamento antigo** [nao verificado]

**Sim:** `MarcoMortoTest.test_cobranca_em_dia_indefinido_e_retratada`. O comentário declara literalmente None como “dia sem furo”; a fixture cria `trabalha=None`, e a expectativa exige retratação. Essa especificação precisa ser invertida diante do aval. (`app/ponto/tests/test_a2_marco_morto.py:28`, `:38`, `:40`, `:44`)

Não afirmo que o teste necessariamente falhe agora: ele cria célula sem ata; depois do diff, esse caso alcança o retorno desconhecido e os fallbacks do coletor. Resultado executado **[nao verificado]**. (`app/ponto/tests/test_a2_marco_morto.py:40`; `app/chamados/juizes.py:452`; `app/chamados/models.py:1250`)

**(d) Checklist**

| Classe | Verificação |
|---|---|
| Segundo juiz duplicando registrado | Não há função nova no diff. Não basta chamar `dia_sem_trabalho` de clone do juiz ternário do vínculo; a duplicação comprovada está na interpretação paralela usada por `_protesta`. (`app/core/juizes.py:158`; `app/escala/models.py:1072`; `app/chamados/juizes.py:408`; `app/ponto/services/cartorio.py:1167`) |
| Segundo escritor fora da porta | Não aparece escritor novo de produção; a atualização direta no arquivo novo é fixture. (`app/chamados/juizes.py:407`; `app/chamados/tests/test_f1c4_3_none_cobra.py:83`) |
| Relógio fora da borda | O predicado alterado não lê relógio; o teste usa tempo congelado. (`app/chamados/juizes.py:407`; `app/chamados/tests/test_f1c4_3_none_cobra.py:59`) |
| Derivação paralela | Permanece em `_protesta`, nas classificações de None e no retratador de indefinidos descritos acima. (`app/ponto/services/cartorio.py:1167`; `app/ponto/services/passivo_s84.py:96`; `app/ponto/management/commands/reconciliar_grade.py:275`) |
| RED sem mordida | **O RED novo morde a volta de `is not True`:** os dois asserts diretos exigem `False` onde o predicado antigo retorna `True`. Falta cobertura da véspera e dos portões, não mordida dessa mutação. Execução **[nao verificado]**. (`app/chamados/tests/test_f1c4_3_none_cobra.py:40`, `:43`, `:95`, `:109`; `app/chamados/juizes.py:405`) |
| Ordem de trava | O diff não altera aquisição de locks; o fechamento atual trava a linha dentro do atomic. Prova concorrente desta obra **[nao verificado]**. (`app/chamados/juizes.py:407`; `app/chamados/models.py:666`, `:672`) |
| Pouso antes do pré-requisito | Há exigência documental de sombra antes do pouso e vai-e-volta zero. Seu cumprimento não está demonstrado pelo material fornecido **[nao verificado]**. (`app/docs/CORTES.md:409`, `:412`) |

## 31/10 07:33 · sessao d1ff92a88f33 · - · - · origem=Ronald (tmux)

Escritores e fontes:

- `trabalha`: a porta declara dono único e operações em `app/ponto/portas/celula.py:3`, `:66`; geração recebe `eh_dia_trabalho_calculado` e cria/atualiza em `:590`, `:595`, `:612`; regeneração em `:243`, `:247`, `:260`; fora do vínculo vira `False` em `:231`; reversão usa existência de `dna.marcos` em `:532`.
- `ata.tipo_dia` e `ata.lampadas`: `ata_do_dia` monta ambos em `app/ponto/services/cartorio.py:558`, `:564`, `:566`; produtor comum das lâmpadas em `:427`; ausência/justificativa dispensam acendendo lâmpadas em `:593`, `:611`, `:617`; teto modifica pendentes em `:706`.
- Persistência da ata: fachada `app/escala/models.py:1775` delega à porta; atribuições e salvamento em `app/ponto/portas/celula.py:697`, `:711`, `:725`, `:732`. `carimbar_precedencia` acrescenta somente chave `precedencia`, `:774`, `:775`, preservada no merge `:703`.
- Projeções em memória: `app/escala/utils.py:1826` copia célula e põe somente lâmpadas/orfas em `:1827`; `app/ponto/portas/celula.py:667` cria cópia do julgamento atual, ata em `:680`. Não equivalem a novo escritor persistente.

Comparação de perguntas:

- Autoridade registrada “dia de trabalho para o vínculo” é `eh_dia_trabalho`, `app/core/juizes.py:158`; devolve o valor ternário bruto da célula em `app/escala/models.py:1072`. `dia_sem_trabalho` adiciona uma pergunta composta: escala explicitamente folga **ou** ata não trabalho (`app/chamados/juizes.py:408`). Portanto não é simplesmente outra implementação da negação booleana do juiz do vínculo.
- `_protesta` responde se um chamado contribui para o julgamento atual, incluindo seus **marcos**, `app/ponto/services/cartorio.py:1166`, `:1167`. Função inteira difere de `dia_sem_trabalho`, mas o subpredicado “dia sem trabalho” diverge: `trabalha=True/None`, `ata.tipo_dia='folga'/'ausencia'`, marco apagado → `_protesta=True`, `dia_sem_trabalho=True`. A divergência da cláusula ata é preexistente; o diff alinha o caso `None + ata trabalho`, não elimina as duas definições (`app/chamados/juizes.py:408`; `app/ponto/services/cartorio.py:1167`).
- Atenção: `'ausencia'` na ata pode indicar cobertura **parcial**, preservando batidas e lâmpadas, `app/escala/utils.py:1180`, `:1190`, `:1195`. `classificar_falta` declara que parcial não cala cobrança em `app/escala/servico_jornada.py:313` e somente retorna sem furo quando ausência não parcial em `:342`, `:344`. A cláusula 2 do aval mantém a supressão ampla; não é falha introduzida por este diff.

Outros consumidores ainda equiparam `None` a folga:

- Relatório soma `folga` e exclui acusações em `app/relatorios/dia_da_frota.py:47`.
- Frase para admin/IA afirma “Dia sem trabalho previsto: nenhuma marca era esperada” em `app/ponto/services/dia_do_colab.py:121`.
- Passivo classifica `None=FOLGA` em `app/ponto/services/passivo_s84.py:96`; `a_retratar` seleciona essa classe em `:116`. São classificações do mesmo fato, com efeitos diferentes.
- Já exigir `True` para fabricar pergunta (`app/chamados/services/fabrica_da_celula.py:78`) ou dar lastro a plantio (`app/chamados/services/materializacao.py:158`) responde elegibilidade/autorização, não prova que `None` seja folga.
- Leitor da célula preserva indeterminação sem tipo lavrado: `'indefinido'`, `app/escala/services/leitor_celula.py:344`.

Classes leitoras de ata/lâmpadas que não usam necessariamente o juiz alterado:

- Grade/apresentação: `app/escala/services/leitor_celula.py:93`, `:98`, `:344`; worklist: `app/ponto/worklist.py:19`, `:24`.
- Lastro por marco: `app/ponto/services/lastro.py:89`, `:128`, `:175`; nomeação de cobrança: `app/chamados/reconciliador.py:673`, `:677`.
- Pergunta/resposta: `app/chamados/juizes.py:885`, `:888`, `:1006`.
- Geometria/realizado: entrada e contrato ternário em `app/ponto/juiz_batida.py:4`, `:15`.
- Folha/contadores: `app/folha/previa.py:84`, `:86`; `app/folha/services/sem_furo.py:20`, `:26`; soma por tipo em `app/folha/export.py:1071`, `:1075`, `:1080`.
- Detectores: `app/ponto/services/cluster_espurio.py:24`, `:27`.

Nenhuma medição de quantidade ou impacto em produção foi demonstrada por essas leituras [nao verificado].

## 31/10 07:33 · sessao c95fcf9aafe2 · - · - · origem=Ronald (tmux)

Achados para consolidação:

1. **A mudança não apenas acrescenta cobrança: pode trocar a célula julgada e até deixar de cobrar.** Em `None` com `ata.tipo_dia='trabalho'` ou ausente, o código antigo entrava no ramo de véspera; o novo consulta as lâmpadas de D. Contraprova estática: D=`None` com lâmpadas acesas; D−1 com lâmpada apagada. Antes `_celula_furada(D)=True`; depois `False`. Para `turno_aberto*`, o coletor não usa o atalho direto de dia sem trabalho e chega justamente nessa mudança; pode regularizar/fechar turno cuja véspera continua furada. Referências: `app/chamados/juizes.py:408`, `:443`, `:446`, `:452`, `:455`, `:465`; `app/chamados/models.py:1232`, `:1247`. Ocorrência em produção: [não verificado].

2. **Há perda preexistente de `para` na recursão.** `_celula_furada(..., para='folha')` recursa à véspera sem transmitir `para`, voltando a `'cobranca'`; nesse modo ausência aberta pode cobrir, segundo comentário e chamada a `ausencia_cobre`. O diff passa a evitar esse caminho em D=None, alterando também qual política de cobertura participa. Referências: `app/chamados/juizes.py:411`, `:424`, `:426`, `:446`. Não é defeito introduzido pela linha, mas precisa entrar no censo do efeito.

3. **Os efeitos alcançam todos estes portões, sem testes novos deles:** [nao verificado]
   - S122 humano pode passar de fechamento permitido a `ValueError`: `app/chamados/models.py:175`, `:177`, `:185`.
   - `reconciliar()` e seu dry `reconciliaria()` leem o mesmo gate; premissa viva pode impedir fechamento, com exceções de agrupado/irmão: `app/chamados/models.py:605`, `:615`, `:625`, `:646`, `:699`, `:709`.
   - `recusa_da_reabertura` deixa de recusar por premissa regularizada; continuam anteriores os vetos e admin sem fato novo: `app/chamados/models.py:1285`, `:1290`, `:1296`.
   - Emissor encontra encerrado em `abrir`, pede renascimento com **marcos do novo pedido**, passa por `reabrir`; se permitido grava estado e ressuscita perguntas: `app/chamados/models.py:880`, `:881`, `:978`, `:1235`, `:1338`, `:1380`, `:1389`.
   - Defesa 1 também passa pelo fechamento afetado: `app/chamados/signals.py:42`.
   - Os testes novos só chamam juiz, `_celula_furada` e coletor diretamente: `app/chamados/tests/test_f1c4_3_none_cobra.py:39`, `:93`, `:105`. Não demonstram efeito nos portões acima nem no turno noturno.

4. **`_protesta`: pergunta composta distinta, mas subpergunta duplicada.** Decide se uma cobrança protesta no julgamento: `_cel.trabalha is not False and _acesos(x, _ata) is not True`. Seu componente de “dia sem trabalho” não lê `ata.tipo_dia`, embora `dia_sem_trabalho` o leia. Exemplo: trabalha=True ou None, ata folga, marco apagado/sem marco → `dia_sem_trabalho=True` e `_protesta=True`. Referências: `app/ponto/services/cartorio.py:1166`, `:1167`; `app/chamados/juizes.py:408`; sem marco dá `marcos_acesos=None`: `app/ponto/services/lastro.py:170`, `:172`.
   
   O protesto decide entrada em `classificar_dia`, lâmpada `cobrado`, adiamento e seleção para reconciliação: `app/ponto/services/cartorio.py:1168`, `:1170`, `:1206`, `:1236`, `:1237`, `:1242`. Divergência é preexistente; o diff não realiza a promessa de predicado único. Não confundir a outra metade — “os marcos desta cobrança acenderam?” — com “dia sem trabalho”: aquela é pergunta própria e tem juiz `marcos_acesos`: `app/ponto/services/lastro.py:153`, `:156`.

5. **Teste antigo contraditório encontrado fora do arquivo novo:** `MarcoMortoTest.test_cobranca_em_dia_indefinido_e_retratada` documenta “previsto não sabido = premissa morta” e exige retratação de chamado sobre célula trabalha=None. Referências: `app/ponto/tests/test_a2_marco_morto.py:8`, `:12`, `:28`, `:38`, `:40`, `:44`. Deve mudar especificação/expectativa diante do aval. **Não afirmar que já falha em execução:** a fixture tem ata ausente; agora `_celula_furada` retorna None e o coletor usa os fallbacks. Referências: `app/chamados/juizes.py:452`, `app/chamados/models.py:1250`, `:1253`, `:1258`. Execução: [não verificado].

6. **Teste existente prova a relevância da véspera:** turno aberto de D+1 folga deve continuar vivo cobrando D furada, e a pergunta deve ressuscitar lendo a célula dela; o fixture exige `_celula_furada(D+1)=True`. Referências: `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:63`, `:65`, `:66`, `:75`, `:88`, `:90`. O novo RED não cobre variante D+1=None.

## 31/10 07:33 · sessao 5293b968580f · - · - · origem=Ronald (tmux)

- **Teste legado incompatível:** `app/ponto/tests/test_a2_marco_morto.py:28` testa `test_cobranca_em_dia_indefinido_e_retratada`; cria célula `trabalha=None` sem ata em `:40` e exige que a cobrança saia de aberta/pendente em `:44-45`. O comentário declara explicitamente “`dia_sem_trabalho` a lê como dia sem furo” em `:37-38`. Precisa inverter expectativa e descrição para cumprir o aval; resultado executado [nao verificado].

- **RED novo morde a volta de `is not True`:** os dois testes diretos esperam `False` para `None/trabalho` e `None/sem tipo` (`app/chamados/tests/test_f1c4_3_none_cobra.py:40`, `:43`); restaurar o predicado antigo retorna `True` nesses casos (`app/chamados/juizes.py:408`). Os outros dois RED cobrem célula com lâmpada apagada e premissa do furo (`app/chamados/tests/test_f1c4_3_none_cobra.py:93-95`, `:105-109`). Execução/mutação real [nao verificado].

- **Derivação paralela adicional:** `reconciliar_grade` ainda pula emissão nos dias `sem_ancora` (`app/ponto/management/commands/reconciliar_grade.py:139-150`), considera esses dias “mortos” (`:268-275`) e afirma fatos `False` literais ao chamar `premissa_morta` (`:290-293`), antes de tentar retratar (`:300-308`). Sua regra textual continua “previsto não sabido = premissa morta” (`:272`). O gate pode recusar o fechamento pelo coletor, pois a retratação chama `reconciliar()` (`app/chamados/reconciliador.py:525`) e este coletor entra na guarda (`app/chamados/models.py:699`); o produtor conserva interpretação contrária ao aval.

- **Registro não autoriza classificar tudo como clone:** o registrado para “dia de trabalho para o vínculo” é `EscalaColaborador.eh_dia_trabalho`, para precedência é `fatos_do_dia` e para furo é `classificar_falta` (`app/core/juizes.py:158-166`). `classificar_falta` explicitamente lê `trabalha is False` como veto de folga (`app/escala/servico_jornada.py:302-304`); `improcedencia_do_dia` distingue `False → improcedente`, `True → falta_real`, `None → ambiguo` (`:539-546`). São perguntas que precisam ser comparadas pela semântica, não pelo nome.

- **Checklist:** nenhum novo escritor de produção, leitura de relógio ou lock aparece no diff fornecido; o `update` do teste é fixture (`app/chamados/tests/test_f1c4_3_none_cobra.py:79-83`) e o relógio do teste está congelado (`:59`). Ordem e execução de travas [nao verificado]. Há portão documental de sombra antes do pouso e de vai-e-volta zero (`app/docs/CORTES.md:409`, `:412`); cumprimento [nao verificado]. A exigência de juiz único consta de `CLAUDE.md:279-283`, e a de não manter derivador paralelo, de `CLAUDE.md:17-22`.

## 31/10 07:21 · sessao 442855ea90ca · F1C4-3 · censo · origem=Code

**`None` é tratado como “sem trabalho” por `dia_sem_trabalho`, embora o campo o descreva como “indeterminado”. `_protesta` permite que esse mesmo `None` proteste, dependendo dos marcos.** (`app/chamados/juizes.py:401`; `app/escala/models.py:1656`; `app/ponto/services/cartorio.py:1167`)

**(1) Chamadores e cadeias de entrada**

| Alvo | Todos os chamadores diretos encontrados em produção |
|---|---|
| `dia_sem_trabalho` | `_celula_furada`, em `app/chamados/juizes.py:436`; `premissa_regularizada`, em `app/chamados/models.py:1232`, com **import local** em `app/chamados/models.py:1227`. |
| `_celula_furada` | A própria função, recursivamente, em `app/chamados/juizes.py:439`; `premissa_regularizada`, em `app/chamados/models.py:1247`, pelo **reexport de compatibilidade** importado em `app/chamados/models.py:3135`. |
| `premissa_regularizada` | `transicionar`, em `app/chamados/models.py:177`; `_gate_premissa_do_fechamento`, em `app/chamados/models.py:699`; `recusa_da_reabertura`, em `app/chamados/models.py:1297`. |

As entradas abaixo compartilham três finais de cadeia:

| Cadeia | Final comum |
|---|---|
| **H — resolução humana** | `transicionar` → `premissa_regularizada()`, somente quando o destino é `'resolvido'` e o chamado é de furo. (`app/chamados/models.py:175`; `app/chamados/models.py:176`; `app/chamados/models.py:177`) |
| **F — fechamento sistêmico** | `reconciliar`/`reconciliaria` → `_decisao_do_fechamento` → `_gate_premissa_do_fechamento` → `premissa_regularizada(para='folha')`. (`app/chamados/models.py:615`; `app/chamados/models.py:625`; `app/chamados/models.py:646`; `app/chamados/models.py:699`) |
| **R — reabertura** | `reabrir` → `recusa_da_reabertura` → `premissa_regularizada(para='folha', marcos=marcos)`, quando encerrado e de furo. (`app/chamados/models.py:1338`; `app/chamados/models.py:1296`; `app/chamados/models.py:1297`) |

Entradas da cadeia **H**:

| Entrada → chamador |
|---|
| Views de chamados → ações → `transicionar`: `app/chamados/views.py:427`, `app/chamados/views.py:461`, `app/chamados/views.py:464`, `app/chamados/views.py:541`, `app/chamados/views.py:544`, `app/chamados/views.py:547` → `app/chamados/services/acoes_chamado.py:70`, `app/chamados/services/acoes_chamado.py:108`, `app/chamados/services/acoes_chamado.py:149`, `app/chamados/services/acoes_chamado.py:222`, `app/chamados/services/acoes_chamado.py:277`, `app/chamados/services/acoes_chamado.py:315`. |
| Views do fio → `processar_post_fio` → `transicionar`: `app/chamados/views.py:674`, `app/chamados/views.py:954` → `app/chamados/services/fio.py:717`. |
| View de colaboradores → `transicionar`: `app/colaboradores/views.py:2068`. |

Há chamadas a `transicionar` que não alcançam a premissa: a ação que pede `'em_analise'`; `ChamadoColaborador.fechar`, chamado com destino padrão `'fechado'`; e o exercício `smoke`, cujo chamado tem categoria/módulo próprios. (`app/chamados/services/acoes_chamado.py:233`; `app/chamados/models.py:433`; `app/ponto/management/commands/fechar_chamados_meio_periodo.py:109`; `app/chamados/management/commands/smoke_chamados.py:125`; `app/chamados/management/commands/smoke_chamados.py:126`; `app/chamados/models.py:175`)

Entradas e chamadores da cadeia **F**:

| Entrada → serviço/chamador → `reconciliar` |
|---|
| Signal `post_save` de disputa → `disputa_fechada_fecha_chamado_pai`. (`app/chamados/signals.py:18`; `app/chamados/signals.py:42`) |
| Reconciliador → `_retratar`. Seus acessos são `retratar`, `retratar_chamados_absolvidos_por_celula` e `reconciliar_chamados_colab`. (`app/chamados/reconciliador.py:525`; `app/chamados/reconciliador.py:548`; `app/chamados/reconciliador.py:959`; `app/chamados/reconciliador.py:1373`) |
| Signal de batida → `fechar_adesao_com_batida`; também `reconciliar_apos_vinculo` → esse serviço. (`app/ponto/signals.py:234`; `app/colaboradores/services/pos_vinculo.py:42`; `app/colaboradores/services/pos_vinculo.py:88`) |
| Comando `marcar_feriado_do_posto` → `_marcar`. (`app/colaboradores/management/commands/marcar_feriado_do_posto.py:32`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:126`) |
| Comando `fechar_cobranca_com_lastro` → import local/chamada de `lastro.fechar`. (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:52`; `app/ponto/management/commands/fechar_cobranca_com_lastro.py:53`; `app/ponto/services/lastro.py:317`) |
| View `criar_cobertura` → `designar_cobertura`. (`app/ponto/views.py:3038`; `app/ponto/services/cobertura.py:122`) |
| `declarar_cobertura_passada` → `reconciliar`; entrada superior: **[não verificado]**. (`app/ponto/services/cobertura.py:317`) |
| Comando `processar_cartorio` ou `julgar_celula` → `julgar_colab` → corpo do cartório. Os seis sítios são o fechamento adiado, marcos satisfeitos, marcos inexistentes, cobrança indevida e os dois caminhos de deduplicação do fio. (`app/ponto/management/commands/processar_cartorio.py:65`; `app/ponto/services/cartorio.py:1671`; `app/ponto/services/cartorio.py:766`; `app/ponto/services/cartorio.py:1242`; `app/ponto/services/cartorio.py:1255`; `app/ponto/services/cartorio.py:1276`; `app/ponto/services/cartorio.py:1354`; `app/ponto/services/cartorio.py:1369`) |
| Views de API → `resolver_cobranca_da_entrada`. (`app/api/views.py:805`; `app/api/views_core.py:805`; `app/ponto/services/entrada_sincronizada.py:31`) |
| Cartório, com import por apelido **`_cluster`**, ou comando `detectar_cluster_espurio` → `cluster_espurio.reconciliar_colab`. (`app/ponto/services/cartorio.py:796`; `app/ponto/services/cartorio.py:797`; `app/ponto/management/commands/detectar_cluster_espurio.py:46`; `app/ponto/services/cluster_espurio.py:80`) |
| Signal de ausência ou comando `fechar_chamados_ausencia` → `fechar_chamados_ausencia_decidida` → `fechar_chamado_da_ausencia`. (`app/ponto/signals.py:271`; `app/ponto/management/commands/fechar_chamados_ausencia.py:44`; `app/ponto/services/ausencia.py:713`; `app/ponto/services/ausencia.py:678`) |
| View de veredito → `veredito_celula` → `fechar_o_fio_do_dia`; também declaração de folga e validação de pergunta chegam a esse fechamento. (`app/ponto/views.py:3304`; `app/ponto/services/veredito_celula.py:325`; `app/ponto/services/veredito_celula.py:183`; `app/ponto/services/declarar_dia.py:198`; `app/chamados/services/validacao.py:271`) |
| View de registro → `ramo_registrar_es` → `_autoresolve_f2_f4`. (`app/ponto/views.py:350`; `app/ponto/services/triagem_batida.py:741`; `app/ponto/services/triagem_batida.py:597`) |
| Comando/emissor `detectar_ausencias`: chamada no `handle` e nos três auxiliares de fechamento. (`app/ponto/management/commands/detectar_ausencias.py:158`; `app/ponto/management/commands/detectar_ausencias.py:816`; `app/ponto/management/commands/detectar_ausencias.py:826`; `app/ponto/management/commands/detectar_ausencias.py:837`) |
| Comando `curar_residuo_bug115` → `_ata_sem_furo`. (`app/ponto/management/commands/curar_residuo_bug115.py:106`; `app/ponto/management/commands/curar_residuo_bug115.py:151`) |
| Comando `detectar_intervalo_ausente`: usa `reconciliaria` no dry e `reconciliar` na aplicação. (`app/ponto/management/commands/detectar_intervalo_ausente.py:134`) |
| Signal de ausência ou comando `reavaliar_ausencias_lancadas` → `silencio_ausencia.reconciliar_colab` → `reconciliar_por_ausencia`. (`app/ponto/signals.py:269`; `app/ponto/management/commands/reavaliar_ausencias_lancadas.py:54`; `app/chamados/services/silencio_ausencia.py:127`; `app/chamados/services/silencio_ausencia.py:101`) |
| Emissor/materializador de resposta → `_materializar_ausencia_de_resposta`. (`app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/materializacao.py:539`) |
| Materializadores de hora/data-hora/escolha → `_reconciliar_destino_e_clones` → função local `_fecha`. (`app/chamados/services/materializacao.py:919`; `app/chamados/services/materializacao.py:1004`; `app/chamados/services/materializacao.py:1024`; `app/chamados/services/materializacao.py:711`) |
| View de aceite → `limbo_folgas.aceitar_sugestao`. (`app/chamados/views.py:433`; `app/chamados/services/limbo_folgas.py:101`) |
| Emissor `detectar_ausencias` ou `apurar_furos_diarios` → `agrupar_furo`, diretamente ou por `agrupar_furos_sob_limbo`. (`app/ponto/management/commands/detectar_ausencias.py:756`; `app/escala/management/commands/apurar_furos_diarios.py:19`; `app/chamados/services/limbo_folgas.py:291`; `app/chamados/services/limbo_folgas.py:243`; `app/chamados/services/limbo_folgas.py:246`) |
| Comando `reavaliar_ausencias_feriado` → `feriado_reavaliar.encerrar`. (`app/ponto/management/commands/reavaliar_ausencias_feriado.py:32`; `app/chamados/services/feriado_reavaliar.py:47`) |
| Emissor `apurar_furos_diarios` → `sincronizar_adesao`. (`app/escala/management/commands/apurar_furos_diarios.py:17`; `app/chamados/services/adesao.py:115`; `app/chamados/services/adesao.py:150`) |
| Comando `auditar_chamados_orfaos`, no `handle`. (`app/chamados/management/commands/auditar_chamados_orfaos.py:67`) |

O ramo do reconciliador acima recebe estas entradas:

- Comandos → `retratar`: `retratar_chamados_lei_cala`, `processar_alertas_turno`, `reconciliar_vinculo`, `reconciliar_grade` e `reconciliar_fantasmas`. (`app/ponto/management/commands/retratar_chamados_lei_cala.py:59`; `app/ponto/management/commands/processar_alertas_turno.py:216`; `app/ponto/management/commands/reconciliar_vinculo.py:68`; `app/ponto/management/commands/reconciliar_grade.py:301`; `app/ponto/management/commands/reconciliar_fantasmas.py:279`)
- Signal de posto ou comando de geofence → `reavaliar_chamados_geofence` → `retratar`. (`app/ponto/signals.py:291`; `app/ponto/signals.py:306`; `app/ponto/management/commands/reconciliar_geofence.py:33`; `app/ponto/services/geofence.py:419`)
- Aprovação/materialização de justificativa → `encerrar_chamado_de_origem` → `retratar`. (`app/ponto/views.py:1838`; `app/ponto/views.py:1917`; `app/ponto/services/justificativa.py:202`; `app/ponto/services/justificativa.py:393`; `app/ponto/services/justificativa.py:42`)
- Signal de batida ou comando `reconciliar_chamados` → `reconciliar_chamados_colab`. (`app/ponto/signals.py:215`; `app/chamados/management/commands/reconciliar_chamados.py:52`)
- Comandos `consumir_eventos_celula`/`reconciliar_chamados` → consumidor → `retratar_chamados_absolvidos_por_celula`. (`app/chamados/management/commands/consumir_eventos_celula.py:31`; `app/chamados/management/commands/reconciliar_chamados.py:62`; `app/chamados/reconciliador.py:1304`)

Os registros de cron incluem `detectar_ausencias`, consumidor de eventos, reconciliador, cartório e apuração encadeada. Os registros de `reconciliar_grade`, `detectar_intervalo_ausente`, cluster e reavaliações de ausência incluem caminhos de observação/alarme; registro não demonstra execução real. (`app/config/crons.py:345`; `app/config/crons.py:375`; `app/config/crons.py:376`; `app/config/crons.py:597`; `app/config/crons.py:1449`; `app/config/crons.py:443`; `app/config/crons.py:472`; `app/config/crons.py:579`; `app/config/crons.py:377`; `app/config/crons.py:378`)

Entradas da cadeia **R**:

| Entrada → chamador |
|---|
| View `desarquivar_chamado` → `reabrir(humano=True)`. (`app/chamados/views_arquivar.py:70`) |
| Views do fio → `processar_post_fio` → `acao_reabrir_disputa_admin` → `reabrir(humano=True)`. (`app/chamados/views.py:674`; `app/chamados/views.py:954`; `app/chamados/services/fio.py:579`; `app/chamados/services/acoes_disputa.py:266`) |
| View de reabertura da competência → fechamento mensal → import local **`reabrir as _reabrir_fila`** → reabertura dos chamados. (`app/ponto/views.py:1544`; `app/ponto/services/fechamento.py:1415`; `app/ponto/services/fechamento.py:1416`; `app/chamados/services/competencia_trancada.py:258`) |
| Emissor `sincronizar_adesao` → `reabrir`. (`app/chamados/services/adesao.py:92`; `app/escala/management/commands/apurar_furos_diarios.py:17`) |
| Signal `post_save` de chamado → `resolver_perguntas_por_chamado_resolvido` → reabertura do pai. A mesma função é chamada explicitamente pelo reconciliador, pelo comando de fantasmas e pelo serviço de becos. (`app/chamados/signals.py:75`; `app/chamados/signals.py:92`; `app/chamados/services/disputa_emissao.py:1606`; `app/chamados/reconciliador.py:538`; `app/chamados/reconciliador.py:1551`; `app/ponto/management/commands/reconciliar_fantasmas.py:280`; `app/chamados/services/becos.py:49`) |
| Emissores/comandos → `reabrir`. (`app/ponto/management/commands/detectar_ausencias.py:301`; `app/ponto/management/commands/reconciliar_grade.py:522`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`) |
| Comando `reabrir_orfas_enterradas` → consulta direta de `recusa_da_reabertura`; na aplicação, também chama `reabrir`. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:105`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`) |
| Emissor `ChamadoColaborador.abrir` → existente encerrado → `_renascer_por_premissa_viva` → `reabrir`. (`app/chamados/models.py:880`; `app/chamados/models.py:881`; `app/chamados/models.py:978`) |
| View de resposta tardia → `resposta_tardia.receber` → `_renascer_por_premissa_viva` → `reabrir`. (`app/chamados/views.py:1053`; `app/chamados/services/resposta_tardia.py:45`; `app/chamados/models.py:978`) |

**(2) Efeito de `None`, `True` e `False` em cada chamador direto**

Defina `A` como: `ata.get('tipo_dia')` é verdadeiro e diferente de `'trabalho'`. O predicado completo é `cel.trabalha is not True or bool(A)`. (`app/chamados/juizes.py:400`; `app/chamados/juizes.py:401`)

| Chamador | `None` | `False` | `True` |
|---|---|---|---|
| `_celula_furada` → `dia_sem_trabalho` | Consulta a **véspera**. | Igual a `None`. | Consulta a véspera se `A`; caso contrário, examina lâmpadas/órfãs do próprio dia. (`app/chamados/juizes.py:436`; `app/chamados/juizes.py:439`; `app/chamados/juizes.py:445`; `app/chamados/juizes.py:450`) |
| `premissa_regularizada` → `dia_sem_trabalho` | Para módulo que **não** começa com `turno_aberto`, devolve `True` imediatamente. | Igual a `None`. | Faz esse retorno imediato somente se `A`; senão continua nos marcos e no furo. (`app/chamados/models.py:1232`; `app/chamados/models.py:1233`; `app/chamados/models.py:1236`; `app/chamados/models.py:1247`) |
| Recursão → `_celula_furada` | Na célula consultada, volta novamente à véspera. | Igual a `None`. | Usa a mesma distinção por `A`; a cadeia devolve o resultado encontrado ou `False` quando a consulta anterior devolve `None`. (`app/chamados/juizes.py:436`; `app/chamados/juizes.py:439`; `app/chamados/juizes.py:440`) |
| `premissa_regularizada` → `_celula_furada` | Nos caminhos que chegam aqui, usa o resultado da véspera. | Igual a `None`. | Usa véspera se `A`, ou o próprio dia; resultado conhecido vira `not _f`, resultado desconhecido segue para outros juízes. (`app/chamados/models.py:1247`; `app/chamados/models.py:1249`; `app/chamados/models.py:1252`; `app/chamados/models.py:1262`) |

A recursão **não repassa `para`**: mesmo quando a primeira chamada recebeu `'folha'`, a véspera é consultada com o padrão `'cobranca'`. (`app/chamados/juizes.py:404`; `app/chamados/juizes.py:439`)

Para os três chamadores de `premissa_regularizada`, o efeito da resposta é:

| Chamador | Quando a premissa devolve `True` | Quando devolve `False` |
|---|---|---|
| `transicionar` | A guarda de furo permite prosseguir para resolução humana. | Recusa com `ValueError`, ressalvado o fallback legado sem data. (`app/chamados/models.py:177`; `app/chamados/models.py:181`; `app/chamados/models.py:185`; `app/chamados/models.py:193`) |
| `_gate_premissa_do_fechamento` | Permite prosseguir no fechamento sistêmico. Ainda existe a guarda da pergunta viva. | Pode permitir por agrupamento/deduplicação; fora dessas saídas, recusa. (`app/chamados/models.py:699`; `app/chamados/models.py:709`; `app/chamados/models.py:722`; `app/chamados/models.py:724`; `app/chamados/models.py:649`) |
| `recusa_da_reabertura` | Devolve `'premissa_regularizada'`; `reabrir` não reabre. | Prossegue para agrupamento, irmão vivo e transição; não garante reabertura. (`app/chamados/models.py:1297`; `app/chamados/models.py:1298`; `app/chamados/models.py:1299`; `app/chamados/models.py:1306`; `app/chamados/models.py:1348`; `app/chamados/models.py:1353`) |

Portanto, **`None` e `False` liberam imediatamente a premissa do chamado não-`turno_aberto*`, se alcançada essa leitura**; `True` depende também de `ata.tipo_dia`. Justificativa aprovada, ausência, decisão administrativa, marcos e outras guardas podem decidir antes/depois dessa leitura. (`app/chamados/models.py:1214`; `app/chamados/models.py:1230`; `app/chamados/models.py:1232`; `app/chamados/models.py:1236`; `app/chamados/juizes.py:419`)

No caminho do **emissor com chamado encerrado já existente**, a premissa regularizada impede renascer; `abrir` devolve o existente com `criado=False`, sem criar substituto nesse ramo. Nascimento sem existente segue outro ramo. (`app/chamados/models.py:881`; `app/chamados/models.py:883`; `app/chamados/models.py:892`; `app/chamados/models.py:978`)

**(3) Escritores**

| Escritor de `CelulaDia.trabalha` | Condição e valor |
|---|---|
| `regenerar_celulas_vinculo` | Dentro da vigência, calcula `valor = ec.eh_dia_trabalho_calculado(d)` e passa esse valor ao `create`/à atualização de célula gerada; portanto pode gravar `None`. Há proteção de período exportado. (`app/ponto/portas/celula.py:238`; `app/ponto/portas/celula.py:243`; `app/ponto/portas/celula.py:247`; `app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:260`) |
| Mesmo regenerador, fora da vigência | Grava **`False`**, inclusive sobre `None`, quando a célula é gerada por esse vínculo, não está protegida e não existe outro vínculo cobrindo a data. (`app/ponto/portas/celula.py:210`; `app/ponto/portas/celula.py:226`; `app/ponto/portas/celula.py:227`; `app/ponto/portas/celula.py:230`; `app/ponto/portas/celula.py:231`) |
| `gerar_celulas_periodo` | Calcula o mesmo `valor`; cria com ele e atualiza hoje/futuro. Preserva célula editada e `trabalha` do passado já existente. Pode criar/gravar `None` quando o cálculo o devolve. (`app/ponto/portas/celula.py:590`; `app/ponto/portas/celula.py:595`; `app/ponto/portas/celula.py:598`; `app/ponto/portas/celula.py:600`; `app/ponto/portas/celula.py:612`) |
| `reverter_regeneracao_dia` | Grava `(alvo or {}).get('marcos') is not None`: somente booleano, nunca `None`. (`app/ponto/portas/celula.py:532`; `app/ponto/portas/celula.py:538`) |

O cálculo pode devolver `None` quando o template não decide: 12x36/24x48 sem resolução pela âncora/foto, personalizado sem mapa útil ou ciclo sem regra. Âncora válida resolve 12x36/24x48 sem foto; a foto também pode resolver a fase; a conversão por feriado preserva `None`. (`app/escala/models.py:824`; `app/escala/models.py:829`; `app/escala/models.py:862`; `app/escala/models.py:867`; `app/escala/models.py:868`; `app/escala/models.py:1260`; `app/escala/models.py:1266`; `app/escala/models.py:1335`; `app/escala/models.py:1337`; `app/escala/models.py:1340`; `app/escala/models.py:1342`)

**`ata['tipo_dia']`** é produzido por `cartorio.ata_do_dia`, copiando `dia.get('tipo_dia')` da grade; a lavra persiste a ata inteira pela porta da célula. (`app/ponto/services/cartorio.py:566`; `app/ponto/services/cartorio.py:905`; `app/ponto/services/cartorio.py:1518`; `app/ponto/services/cartorio.py:1520`; `app/escala/models.py:1783`; `app/ponto/portas/celula.py:711`; `app/ponto/portas/celula.py:732`)

A relação anterior à gravação é:

| Etapa | Derivação |
|---|---|
| Autoridade da escala | Havendo célula, `eh_dia_trabalho` devolve `cel.trabalha`, preservando `None`. (`app/escala/models.py:1071`; `app/escala/models.py:1072`) |
| Grade por turno | Só `is False` exclui o dia da montagem normal. `None` pode entrar como `'trabalho'` com `sem_ancora=True`; dia sem montagem sai `'folga'`. Intermitente tem tratamento próprio para dia com batida. (`app/escala/utils.py:1548`; `app/escala/utils.py:1702`; `app/escala/utils.py:1722`; `app/escala

[truncado em 20000 de 30493 caracteres]

## 31/10 07:18 · sessao 35b8de0e7279 · - · - · origem=Ronald (tmux)

Censo dos chamadores diretos fechado:

| Alvo | Chamadores de produção |
|---|---|
| `dia_sem_trabalho` | `_celula_furada`, `app/chamados/juizes.py:436`; `premissa_regularizada`, `app/chamados/models.py:1232`, com import local em `:1227`. |
| `_celula_furada` | Recursão em `app/chamados/juizes.py:439`; `premissa_regularizada` em `app/chamados/models.py:1247`, usando reexport importado de `chamados.juizes` em `:3135-3136`. |
| `premissa_regularizada` | `transicionar`, `app/chamados/models.py:177`; `_gate_premissa_do_fechamento`, `:699`; `recusa_da_reabertura`, `:1297`. |

As cadeias compartilham estes sufixos:

- Resolução humana → `transicionar` → premissa; somente para `novo_status == 'resolvido'` e chamado de furo. `app/chamados/models.py:175-177`.
- Fechamento sistêmico → `reconciliar` ou `reconciliaria` → `_decisao_do_fechamento` → `_gate_premissa_do_fechamento` → premissa. `app/chamados/models.py:615`, `:625`, `:646`, `:699`.
- Reabertura → `reabrir` → `recusa_da_reabertura` → premissa, somente encerrado de furo. `app/chamados/models.py:1338`, `:1296-1298`.
- Emissor `abrir` → existente encerrado → `_renascer_por_premissa_viva` → `reabrir`. Mesmo recusada a reabertura, devolve o existente com `False`, sem nascimento novo. `app/chamados/models.py:880-883`, `:978`.

Todos os callsites de `reconciliar`/`reconciliaria`, além dos internos acima:

| Arquivo | Função e linhas |
|---|---|
| `app/chamados/signals.py` | `disputa_fechada_fecha_chamado_pai:42` — signal `post_save` de Disputa, registrado em `:18`. |
| `app/chamados/reconciliador.py` | `_retratar:525`; entra por wrapper `retratar:548`, `retratar_chamados_absolvidos_por_celula:959`, `reconciliar_chamados_colab:1373`. |
| `app/colaboradores/services/pos_vinculo.py` | `fechar_adesao_com_batida:42`; chamado pelo signal Batida `app/ponto/signals.py:234` e por `reconciliar_apos_vinculo:88`. |
| `app/colaboradores/management/commands/marcar_feriado_do_posto.py` | `_marcar:126`; comando com `handle:32`. |
| `app/ponto/services/lastro.py` | `fechar:317`; import local e chamada no comando `app/ponto/management/commands/fechar_cobranca_com_lastro.py:52-53`; cron `app/config/crons.py:710`. |
| `app/ponto/services/cobertura.py` | `designar_cobertura:122`, entrada view `app/ponto/views.py:3038`; `declarar_cobertura_passada:317`, entrada superior [não verificado]. |
| `app/ponto/services/cartorio.py` | `julgar_colab:766`; `_julgar_colab_corpo:1242,1255,1276,1354,1369`; entrada comando `processar_cartorio.py:65`, cron `app/config/crons.py:597`; entrada signal por `app/ponto/signals.py:93` → `julgar_celula` → `julgar_colab:1671`. |
| `app/ponto/services/entrada_sincronizada.py` | `resolver_cobranca_da_entrada:31`; entradas `app/api/views.py:805` e `app/api/views_core.py:805`. |
| `app/ponto/services/cluster_espurio.py` | `reconciliar_colab:80`; import com apelido `_cluster` em `cartorio.py:796`, chamada `:797`; comando `detectar_cluster_espurio.py:46`. |
| `app/ponto/services/ausencia.py` | `fechar_chamado_da_ausencia:678` ← `fechar_chamados_ausencia_decidida:713` ← signal `app/ponto/signals.py:271` ou comando `fechar_chamados_ausencia.py:44`. |
| `app/ponto/services/veredito_celula.py` | `fechar_o_fio_do_dia:183` ← `veredito_celula:325` ← view `app/ponto/views.py:3304`; também entradas `app/ponto/services/declarar_dia.py:198` e `app/chamados/services/validacao.py:271`. |
| `app/ponto/services/triagem_batida.py` | `_autoresolve_f2_f4:597` ← `ramo_registrar_es:741` ← view `app/ponto/views.py:350`. |
| `app/ponto/management/commands/detectar_ausencias.py` | `handle:158`; `_fechar_por_afastamento:816`, `_fechar_por_cobertura:826`, `_fechar_chamado:837`, chamados pelo `handle:260,351,381,412`; cron `app/config/crons.py:345`. |
| `app/ponto/management/commands/curar_residuo_bug115.py` | `_ata_sem_furo:151` ← `handle:106`. |
| `app/ponto/management/commands/detectar_intervalo_ausente.py` | `handle:134`: `reconciliaria` no dry, `reconciliar` no apply; cron usa `--alarme`, `app/config/crons.py:472`. |
| `app/chamados/services/silencio_ausencia.py` | `reconciliar_por_ausencia:101` ← `reconciliar_colab:127` ← signal `app/ponto/signals.py:269`; também comando `reavaliar_ausencias_lancadas.py:54`. |
| `app/chamados/services/materializacao.py` | `_materializar_ausencia_de_resposta:539` ← emissor/materializador `app/chamados/services/disputa_emissao.py:1295`; `_reconciliar_destino_e_clones._fecha:711` ← materializadores `:919,1004,1024`. |
| `app/chamados/services/limbo_folgas.py` | `aceitar_sugestao:101` ← view `app/chamados/views.py:433`; `agrupar_furo:243,246` ← emissor `detectar_ausencias.py:756` ou `agrupar_furos_sob_limbo:291` ← `app/escala/management/commands/apurar_furos_diarios.py:19`. |
| `app/chamados/services/feriado_reavaliar.py` | `encerrar:47` ← comando `reavaliar_ausencias_feriado.py:32`; cron `app/config/crons.py:589`. |
| `app/chamados/services/adesao.py` | `sincronizar_adesao:115,150` ← emissor `app/escala/management/commands/apurar_furos_diarios.py:17`; encadeado cron `app/config/crons.py:1449`. |
| `app/chamados/management/commands/auditar_chamados_orfaos.py` | `handle:67`. |

Todos os callsites externos de `reabrir`/`recusa_da_reabertura`:

- View `app/chamados/views_arquivar.py:70`.
- `acao_reabrir_disputa_admin`, `app/chamados/services/acoes_disputa.py:266` ← `processar_post_fio`, `app/chamados/services/fio.py:579` ← views `app/chamados/views.py:674,954`.
- Competência reaberta: view `app/ponto/views.py:1544` → `services/fechamento.py:1415-1416`, import local `reabrir as _reabrir_fila` → `services/competencia_trancada.py:258`.
- Emissor `sincronizar_adesao`, `app/chamados/services/adesao.py:92`.
- `resolver_perguntas_por_chamado_resolvido`, `app/chamados/services/disputa_emissao.py:1606` ← signal `post_save Chamado`, `app/chamados/signals.py:75,89-92`; também chamadas explícitas `reconciliador.py:538,1551`, `reconciliar_fantasmas.py:280`, `services/becos.py:49`.
- Emissores/comandos `app/ponto/management/commands/detectar_ausencias.py:301`, `reconciliar_grade.py:522`, `reabrir_chamados_ausencia_aberta.py:49`.
- Comando `app/chamados/management/commands/reabrir_orfas_enterradas.py:105` lê recusa; `:161` reabre.
- Resposta tardia: view `app/chamados/views.py:1050-1053` → `app/chamados/services/resposta_tardia.py:45` → `_renascer_por_premissa_viva`, `models.py:978`.

Os callsites de resolução humana chegam por `app/chamados/services/acoes_chamado.py:70,108,149,222,277,315` ← view `app/chamados/views.py:427,461,464,541,544,547`; por `app/chamados/services/fio.py:717` ← views `:674,954`; e pela view `app/colaboradores/views.py:2068`. O callsite `acoes_chamado.py:233` pede `'em_analise'`, portanto não alcança a consulta da premissa (`models.py:175`). O comando `fechar_chamados_meio_periodo.py:109` usa `ChamadoColaborador.fechar` com destino default `'fechado'` (`models.py:433`), também sem alcançar a premissa.

Efeitos triestado:

- `None` e `False` dão `dia_sem_trabalho=True` para qualquer ata; `True` também dá `True` se `tipo_dia` for truthy e diferente de `'trabalho'`. `app/chamados/juizes.py:400-401`.
- Na `_celula_furada`, esse `True` troca a leitura das lâmpadas/órfãs atuais pela véspera; devolve o resultado da véspera ou `False` se ela não souber. A recursão não propaga `para='folha'`: usa default `'cobranca'`. `app/chamados/juizes.py:404,436-440,445-458`.
- Na premissa de chamado que não começa com `turno_aberto`, esse `True` regulariza imediatamente; para `turno_aberto*`, seguem marcos e `_celula_furada`. `app/chamados/models.py:1232-1249`.
- Premissa regularizada libera o gate humano, permite o sistêmico prosseguir e recusa reabertura; não garante fechamento quando guardas anteriores/posteriores recusam. `app/chamados/models.py:175-193,646-653,1296-1307,1348-1353`.

## 31/10 07:16 · sessao 515bfa0184d7 · - · - · origem=Ronald (tmux)

Complementos ao censo já enviado:

- `bin/censo_folha.py:26` conta limbo por `trabalha__isnull=True`; `bin/censo_folha.py:27` conta dias previstos por `trabalha=True`.
- `app/folha/export.py:1237` usa `c.trabalha is not None` para distinguir estado conhecido em célula sem veredito; tanto `True` como `False` permitem retenção nesse ramo (`app/folha/export.py:1237-1238`).
- `app/ponto/portas/celula.py:227` usa `_cel_fv.trabalha is not False` para identificar célula gerada que precisa virar `False`, incluindo `None`, satisfeitas as demais condições (`app/ponto/portas/celula.py:225-233`).
- `app/ponto/management/commands/gerar_celulas.py:79` usa `trabalha is True` para materializar marcos; `app/ponto/management/commands/gerar_celulas.py:121` repete a guarda para gravar `extra_declarada`.
- `app/ponto/management/commands/tripwire_celulas.py:30` compara `cel.trabalha == vivo`: pergunta se a célula coincide com o resultado recalculado do vínculo, junto com a igualdade do DNA (`app/ponto/management/commands/tripwire_celulas.py:27-33`).

Não encontrei ocorrência executável de `trabalha == False` na busca da produção; inexistência absoluta de outras formas dinâmicas: **[não verificado]**.

Para o quadro de divergência, convém comparar `dia_sem_trabalho` com **o componente laboral** de `_protesta`: o resultado completo de `_protesta` também depende de `marcos_acesos(x, _ata) is not True` (`app/chamados/juizes.py:401`; `app/ponto/services/cartorio.py:1167`). Assim, quando todos os marcos estão acesos, `_protesta=False` também em dia de trabalho (`app/ponto/services/cartorio.py:1167`).

O teste encontrado para `None` é de integração: cria célula `trabalha=None`, executa `reconciliar_grade` sem mock da premissa e exige chamado fora de `aberto`/`pendente` (`app/ponto/tests/test_a2_marco_morto.py:37-45`). Ele não afirma diretamente o retorno de `_celula_furada` nem cobre sua recursão pela véspera; o fixture é um chamado do módulo canônico do reconciliador (`app/ponto/tests/test_a2_marco_morto.py:32-35`).

## 31/10 07:15 · sessao 6e231748ca71 · - · - · origem=Ronald (tmux)

**Escritores de `CelulaDia.trabalha`**

| Função | Escrita | Quando grava `None` |
|---|---|---|
| `regenerar_celulas_vinculo` | Fora da vigência, escreve `False` em célula gerada pela EC, sem outra EC cobrindo e sem lavra protegida; dentro, calcula `valor`, cria ou atualiza célula gerada. `app/ponto/portas/celula.py:210-247`, `:251-265` | Quando `ec.eh_dia_trabalho_calculado(d)` retorna `None`; esse retorno é passado diretamente ao create/update. `app/ponto/portas/celula.py:243-247`, `:260` |
| `gerar_celulas_periodo` | Cria com `valor`; atualiza célula gerada de hoje/futuro se valor, geradora ou DNA mudou. Pula editada e preserva `trabalha` do passado. `app/ponto/portas/celula.py:588-615` | Quando o cálculo retorna `None`; criação aceita qualquer data, atualização apenas hoje/futuro. `app/ponto/portas/celula.py:590-612` |
| `reverter_regeneracao_dia` | Restaura `cel.trabalha = (alvo or {}).get('marcos') is not None`. `app/ponto/portas/celula.py:527-539` | Nunca: a expressão produz booleano. `app/ponto/portas/celula.py:532` |

O command `gerar_celulas` reexporta os dois primeiros escritores; seu `handle` chama `gerar_celulas_periodo`. `app/ponto/management/commands/gerar_celulas.py:13`, `:303-312`. O cron registra `gerar_celulas` às 05:50. `app/config/crons.py:407`.

O campo permite `null=True` e não declara default. `app/escala/models.py:1654-1656`; a migration de criação também permite NULL. `app/escala/migrations/0030_celuladia.py:22`.

**Como o cálculo chega a `None`**

- Âncora válida (`year > 1940`), sem foto do mês, resolve os ciclos 12x36/24x48 em booleano. `app/escala/models.py:1260-1266`.
- Com foto: data fotografada é `False`; fora da foto, usa a resposta do template; se o template não decide, tenta paridade da folga de referência, retornando `None` quando não encontra referência. `app/escala/models.py:1280-1293`, `:1322-1335`.
- Sem foto, usa `TipoEscala.eh_dia_trabalho`. `app/escala/models.py:1336-1337`. O template devolve `None` para 12x36/24x48, personalizado sem mapa útil e ciclo sem regra. `app/escala/models.py:824-829`, `:862-868`.
- A conversão por feriado só altera `base is True`, preservando `None`. `app/escala/models.py:1338-1342`.

**Escritor de `ata['tipo_dia']`**

`cartorio.ata_do_dia` produz a chave copiando `dia.get('tipo_dia')`; não deriva diretamente de `cel.trabalha`. `app/ponto/services/cartorio.py:558-566`. O dia vem de `montar_grade_prevista_periodo`. `app/ponto/services/cartorio.py:905-909`.

A ata é entregue à lavra tanto no ramo nunca bateu como no ramo principal. `app/ponto/services/cartorio.py:981`, `:1012-1013`, `:1518-1520`. O método do modelo delega à porta. `app/escala/models.py:1775-1783`. A porta copia/substitui e persiste a ata inteira, inclusive quando o admin já decidiu o veredito. `app/ponto/portas/celula.py:697-714`, `:724-732`. `carimbar_precedencia` conserva as demais chaves e altera apenas `precedencia`. `app/ponto/portas/celula.py:767-778`.

A derivação anterior à ata é:

- `EscalaColaborador.eh_dia_trabalho` retorna `cel.trabalha` quando existe célula. `app/escala/models.py:1066-1073`.
- Builder por turno: só `is False` exclui o dia da montagem normal; `None` passa e marca `sem_ancora=True`. `app/escala/utils.py:1541-1549`, `:1701-1702`. Dia montado recebe `'trabalho'`; dia sem montagem recebe `'folga'`. `app/escala/utils.py:1722-1753`. Intermitente tem tratamento próprio: mesmo com previsão `False`, dia com batida pode ser montado como trabalho. `app/escala/utils.py:1720-1722`.
- Builder legado: `work is False` produz `'folga'`; `None` segue como `'trabalho'` com `sem_ancora=True`. `app/escala/utils.py:725-729`, `:752`. O despacho depende de `GRADE_POR_TURNO`. `app/escala/utils.py:2024-2030`.
- Foto de folgas pode substituir o tipo por `'folga'`; ausência aprovada pode substituir por `'falta'` ou `'ausencia'`. `app/escala/utils.py:1114-1120`, `:1170-1180`. Esses overlays são aplicados antes da entrega da grade ao cartório. `app/escala/utils.py:1785-1789`.

O fallback do **leitor**, distinto do produtor da ata, usa `'trabalho' if cel.trabalha else 'folga' if cel.trabalha is False else 'indefinido'` quando a ata não oferece tipo. `app/escala/services/leitor_celula.py:344-345`.

## 31/10 06:45 · sessao 9fffb8394a9e · FIO-ESTADO-LE-O-ALCANCE · verificador · origem=Code

**Há achados no tratamento de falhas e na força do selo. A conjunção de tipos do alcance está correta; o escritor produz o esquema esperado.** (`bin/fio_estado.py:71`, `bin/fio_estado.py:106`, `bin/tests/test_fio_estado.sh:135`, `app/chamados/management/commands/vigiar_fio_morto.py:216`)

**1. Contrato 3, 3’, 4 e 4’.**

Cada condição do 3’:

| Condição | Implementação literal |
|---|---|
| `alcance` é dict | Sim. Ausência, `None`, lista, string e booleano são rejeitados. (`bin/fio_estado.py:79`, `bin/fio_estado.py:80`) |
| `alcance_universo` int > 0 | Sim. Usa `type(v) is int`: rejeita bool, float, string, ausência e `None`; zero e negativos também invalidam. (`bin/fio_estado.py:71`, `bin/fio_estado.py:84`, `bin/fio_estado.py:87`) |
| `alcance_cego is False` | Sim. `True` dá CEGO; ausência, `None` e inteiro `0` não passam como `False`. (`bin/fio_estado.py:82`, `bin/fio_estado.py:89`) |
| `esperado` contém `fio_morto_alcance` | Sim, exige presença. **O valor dessa chave pode ser `None`, bool ou outro tipo**: o 3’ exige presença, e o código também. `esperado` inteiro ausente/`None` vira `{}` e falha. (`bin/fio_estado.py:91`, `bin/fio_estado.py:119`, `bin/fio_estado.py:172`) |
| `censo.fio_morto_alcance` int | Sim. Ausência, `None`, bool, float e string são rejeitados. `censo` ausente/`None` vira `{}`. (`bin/fio_estado.py:71`, `bin/fio_estado.py:93`, `bin/fio_estado.py:118`) |
| `medido_em` legível | Sim para a validação comum: ausência, `None`, vazio e valor não interpretável impedem a manchete numérica. (`bin/fio_estado.py:123`, `bin/fio_estado.py:125`, `bin/fio_estado.py:134`) |
| Velho quando idade > `TETO_H`, sem arredondamento prévio | A comparação usa horas cruas; arredonda depois. Há uma ressalva de fuso descrita na resposta 3. (`bin/fio_estado.py:127`, `bin/fio_estado.py:129`, `bin/fio_estado.py:130`) |

Os desvios encontrados:

- **Erro de leitura não vira ILEGIVEL.** JSON quebrado, como `{`, ou `PermissionError` devolvem `medido=False, ilegivel=False`; `linha()` publica **NUNCA MEDIDO**, inclusive dizendo “não há” arquivo existente. Isso contraria “exceção de leitura = ILEGIVEL”. (`bin/fio_estado.py:106`, `bin/fio_estado.py:107`, `bin/fio_estado.py:164`)
- **“Nunca levanta / gerador não cai” não é garantido:** há `RecursionError` não capturado, detalhado na resposta 3. (`bin/fio_estado.py:105`, `bin/fio_estado.py:106`, `bin/fio_estado.py:111`, `bin/gerar_estado.py:261`)
- **Ausente permanece descrito como amarelo**, com manchete NUNCA MEDIDO; o diff não estabelece o vermelho literal pedido. (`bin/fio_estado.py:26`, `bin/fio_estado.py:165`, `bin/gerar_estado.py:258`)
- **4/4’: a troca do contador tem mordida**, pois os casos cruzados exigem 4 e 0 na primeira linha; a garantia do selo contra falhas de data/esquema é incompleta, conforme resposta 5. (`bin/tests/test_fio_estado.sh:140`, `bin/tests/test_fio_estado.sh:141`, `bin/tests/test_fio_estado.sh:145`, `bin/tests/test_fio_estado.sh:146`, `bin/tests/test_fio_estado.sh:173`)

**2. Quem escreve e quem lê.**

O command grava o JSON por `_carimbar`; `censo_alcance` produz o bloco em memória. O gerador consome apenas `ler → linha`. (`app/chamados/management/commands/vigiar_fio_morto.py:114`, `app/chamados/management/commands/vigiar_fio_morto.py:206`, `bin/gerar_estado.py:260`)

| Chave | Escritor: nome, tipo e forma | Leitor |
|---|---|---|
| `alcance` | Dict integral retornado pelo serviço. (`app/chamados/services/vigia_fio.py:539`, `app/chamados/management/commands/vigiar_fio_morto.py:218`) | Valida o bloco e extrai campos. (`bin/fio_estado.py:79`, `bin/fio_estado.py:138`) |
| `alcance.alcance_universo` | `len(dias)`, int. (`app/chamados/services/vigia_fio.py:553`) | Valida e imprime. (`bin/fio_estado.py:85`, `bin/fio_estado.py:173`) |
| `alcance.alcance_cego` | `not dias`, bool. (`app/chamados/services/vigia_fio.py:554`) | Testa identidade booleana. (`bin/fio_estado.py:82`, `bin/fio_estado.py:89`) |
| `alcance.alcance_de_hoje_pra_frente` | Int, começa zero e incrementa. (`app/chamados/services/vigia_fio.py:540`, `app/chamados/services/vigia_fio.py:605`) | Extrai e imprime. (`bin/fio_estado.py:150`, `bin/fio_estado.py:175`) |
| `censo.fio_morto_alcance` | Int do bloco, copiado para `censo`. (`app/chamados/services/vigia_fio.py:650`, `app/chamados/management/commands/vigiar_fio_morto.py:217`) | Valida e usa na manchete. (`bin/fio_estado.py:93`, `bin/fio_estado.py:174`) |
| `esperado.fio_morto_alcance` | Int `0`, copiado de `ESPERADO_DO_CARIMBO`. (`app/chamados/management/commands/vigiar_fio_morto.py:95`, `app/chamados/management/commands/vigiar_fio_morto.py:211`) | Exige chave e compara valor recebido. (`bin/fio_estado.py:91`, `bin/fio_estado.py:172`, `bin/fio_estado.py:176`) |
| `alarma_em_0` | Lista de strings; atualmente sem alcance, por desenho declarado. (`app/chamados/management/commands/vigiar_fio_morto.py:82`, `app/chamados/management/commands/vigiar_fio_morto.py:92`, `app/chamados/management/commands/vigiar_fio_morto.py:213`) | Consulta pertença para o aviso. (`bin/fio_estado.py:143`, `bin/fio_estado.py:178`) |
| `medido_em` | String ISO com offset, derivada do mesmo `agora` usado na rodada. (`app/chamados/management/commands/vigiar_fio_morto.py:162`, `app/chamados/management/commands/vigiar_fio_morto.py:207`) | Interpreta, calcula idade e imprime. (`bin/fio_estado.py:125`, `bin/fio_estado.py:128`, `bin/fio_estado.py:194`) |

**Recorte indevidamente carimbado: sem achado.** `--empresa` transforma `empresas` em lista; somente `empresas is None` permite gravar. O outro ramo declara que não carimbou. (`app/chamados/management/commands/vigiar_fio_morto.py:152`, `app/chamados/management/commands/vigiar_fio_morto.py:156`, `app/chamados/management/commands/vigiar_fio_morto.py:205`, `app/chamados/management/commands/vigiar_fio_morto.py:223`)

**Sempre inválido por incompatibilidade: sem achado.** Os nomes e tipos coincidem; universo vazio produz legitimamente zero/cego e deve ser rejeitado. A frota do serviço já é a de `empresas_visiveis()`. (`app/chamados/services/vigia_fio.py:513`, `app/chamados/services/vigia_fio.py:553`, `app/chamados/services/vigia_fio.py:555`)

O destino pode ser sobrescrito por `VIGIA_FIO_CARIMBO`; o selo confere somente o default literal. A configuração ativa de produção está `[nao verificado]`. (`app/chamados/management/commands/vigiar_fio_morto.py:110`, `bin/tests/test_fio_estado.sh:119`, `bin/tests/test_fio_estado.sh:122`)

**3. Exceções e número na primeira linha.**

**Sim: JSON válido muito aninhado derruba `ler` e a seção do gerador.** Reproduzi em memória com `'[' * 10000 + '0' + ']' * 10000`: o parser levanta `RecursionError`; nenhum dos dois `except` a captura. Com os outros leitores substituídos em memória, `gerar()` também caiu na chamada do fio. (`bin/fio_estado.py:105`, `bin/fio_estado.py:106`, `bin/fio_estado.py:111`, `bin/gerar_estado.py:261`)

**`linha(ler(JSON))` levantar por formas comuns: sem achado.** Formas que provocam `AttributeError`, `TypeError` ou `KeyError` durante a leitura são convertidas em ILEGIVEL antes da composição. (`bin/fio_estado.py:111`, `bin/fio_estado.py:168`)

**Há manchete numérica com carimbo efetivamente velho se o offset diferir do host.** O leitor remove o offset sem converter o instante. Reprodução: `agora=2026-10-31 12:00` local −03 e `medido_em="2026-10-30T11:00:00+00:00"` significam **28 horas**, mas o leitor calcula **25**, aceita e publica `**fio morto: 0**`. A configuração base usa São Paulo. (`bin/fio_estado.py:127`, `bin/fio_estado.py:128`, `bin/fio_estado.py:129`, `bin/fio_estado.py:173`, `app/config/settings/base.py:93`)

Além disso, **a primeira linha inválida contém números diagnósticos**: universo `0`, contagem inválida `'4'` ou limite `26 h`. Portanto “nenhum número na PRIMEIRA linha”, tomado literalmente, não é cumprido; embora esses exemplos não publiquem uma contagem no título `**fio morto: N**`. (`bin/fio_estado.py:88`, `bin/fio_estado.py:94`, `bin/fio_estado.py:137`, `bin/fio_estado.py:181`)

**4. Regra própria do leitor.**

**Sem achado de novo limiar de negócio, filtro temporal do universo ou derivação paralela do alcance.** A manchete usa o contador recebido e compara com o esperado recebido; “hoje pra frente” é apenas impresso, já separado pelo serviço. As exigências de universo e cegueira são as expressamente pedidas pelo 3’. (`bin/fio_estado.py:85`, `bin/fio_estado.py:89`, `bin/fio_estado.py:172`, `bin/fio_estado.py:174`, `app/chamados/services/vigia_fio.py:604`)

A idade continua sendo derivação local; seu descarte do offset tem o problema descrito acima. (`bin/fio_estado.py:127`, `bin/fio_estado.py:128`)

**5. Selo: ausência de sinal e mutantes sobreviventes.**

- **Traceback pode passar como “sem número”.** `manchete()` mistura stderr e perde o status do leitor; `num()` retorna vazio para silêncio, traceback ou formato inesperado. Casos 12, 13, 15 e 16 exigem somente vazio. (`bin/tests/test_fio_estado.sh:19`, `bin/tests/test_fio_estado.sh:135`, `bin/tests/test_fio_estado.sh:136`, `bin/tests/test_fio_estado.sh:163`, `bin/tests/test_fio_estado.sh:166`, `bin/tests/test_fio_estado.sh:173`, `bin/tests/test_fio_estado.sh:183`)
- **Mutante de crash sobrevivente:** retirar `ValueError` do tratamento da data faz `ontem-de-manha` levantar exceção; o caso 15 continua satisfeito pelo `num()` vazio. (`bin/fio_estado.py:125`, `bin/fio_estado.py:131`, `bin/fio_estado.py:111`, `bin/tests/test_fio_estado.sh:172`, `bin/tests/test_fio_estado.sh:173`)
- **Mutante de segundo limiar sobrevivente:** trocar a leitura de `esp` por `esp = 0`. Todas as fixtures fixam o esperado **do alcance** em zero; o caso do esperado 7 varia somente o contador antigo. A comparação em memória preservou as saídas das fixtures reconstruídas. (`bin/fio_estado.py:172`, `bin/tests/test_fio_estado.sh:48`, `bin/tests/test_fio_estado.sh:49`, `bin/tests/test_fio_estado.sh:105`)
- **Mutantes de tipo sobreviventes:** `_inteiro` com `isinstance(v, int)` aceitaria bool; relaxar a identidade de `alcance_cego` poderia aceitar zero/ausência/`None`. Essas entradas não aparecem nas fixtures. (`bin/fio_estado.py:71`, `bin/fio_estado.py:89`, `bin/tests/test_fio_estado.sh:46`, `bin/tests/test_fio_estado.sh:53`, `bin/tests/test_fio_estado.sh:54`)
- **Caso 19 prova texto, não composição ou tipos:** seus três `grep`s podem continuar verdes com um retorno incompatível de `censo_alcance`. (`bin/tests/test_fio_estado.sh:199`, `bin/tests/test_fio_estado.sh:200`, `bin/tests/test_fio_estado.sh:201`)
- **Mordida efetiva:** os casos 8/9 detectam a volta geral ao contador antigo; o caso 18 exige rc zero, ausência de traceback e ILEGIVEL para as duas formas testadas. (`bin/tests/test_fio_estado.sh:141`, `bin/tests/test_fio_estado.sh:146`, `bin/tests/test_fio_estado.sh:192`, `bin/tests/test_fio_estado.sh:193`, `bin/tests/test_fio_estado.sh:194`)

Execução integral do selo e confirmação dos logs GREEN/RED/M1–M7: `[nao verificado]`. O shell cria e apaga arquivos, contrariando a restrição desta auditoria; as reproduções foram feitas em memória. (`bin/tests/test_fio_estado.sh:32`, `bin/tests/test_fio_estado.sh:71`, `bin/tests/test_fio_estado.sh:98`)

## 31/10 06:43 · sessao abb14f791384 · - · - · origem=Ronald (tmux)

- **Exceção não contida:** JSON válido com 10.000 arrays aninhados levanta `RecursionError` em `json.load`; os `except` não a capturam. A chamada direta do gerador também não tem proteção. Reproduzido em memória, sem arquivos. `bin/fio_estado.py:105`, `bin/fio_estado.py:106`, `bin/fio_estado.py:111`, `bin/gerar_estado.py:261`.

- **Erro de leitura rotulado incorretamente:** JSON sintaticamente inválido, como `{`, produz `NUNCA MEDIDO`, em vez de `ILEGIVEL`: o tratamento devolve `medido=False, ilegivel=False` e `linha` toma primeiro o ramo de ausência. `bin/fio_estado.py:106`, `bin/fio_estado.py:107`, `bin/fio_estado.py:164`.

- **Manchete numérica com medição inválida:** sem achado quanto ao contador `**fio morto: N**`; a conjunção rejeita ausência, `None`, booleanos nas contagens e data inválida/velha. Entretanto, a primeira linha contém números diagnósticos — universo `0`, censo inválido `'4'`, idade limite `26 h` — se “sem número” for interpretado literalmente como nenhuma ocorrência numérica. `bin/fio_estado.py:71`, `bin/fio_estado.py:80`, `bin/fio_estado.py:88`, `bin/fio_estado.py:94`, `bin/fio_estado.py:134`, `bin/fio_estado.py:137`, `bin/fio_estado.py:181`.

- **`esperado.fio_morto_alcance=None`:** aceita e publica o contador com `esperado None`; isso cumpre literalmente o 3', que exige presença da chave, sem especificar seu tipo/valor. `bin/fio_estado.py:91`, `bin/fio_estado.py:172`, `bin/fio_estado.py:173`.

- **`linha(ler(JSON))` com formas comuns:** sem achado de exceção adicional; erros estruturais relevantes são contidos antes da composição. Valores vazios no topo ou em `censo`/`esperado` são normalizados para `{}`. `bin/fio_estado.py:105`, `bin/fio_estado.py:111`, `bin/fio_estado.py:118`, `bin/fio_estado.py:119`, `bin/fio_estado.py:168`.

## 31/10 06:43 · sessao 8ae4b7d542a0 · - · - · origem=Ronald (tmux)

Achados do selo, com leitura e execução apenas em memória:

- **Ausência de sinal aceita como sucesso:** `manchete()` mistura stderr, pega só a primeira linha e perde o status do leitor; `num()` devolve vazio para traceback, silêncio ou qualquer formato inesperado. Só há `set -u`, sem `pipefail`. Os casos universo zero, esperado sem chave, data inválida/vazia e velho/fronteira exigem apenas esse vazio. Assim, nesses casos, uma exceção passa pela exigência “sem número”. (`bin/tests/test_fio_estado.sh:19`, `:135`, `:136`, `:163`, `:166`, `:173`, `:175`, `:179`, `:183`.)

- **Mutante plausível que passa:** trocar `esp = x['esperado'].get('fio_morto_alcance')` por `esp = 0`. Todas as fixtures fixam `esperado.fio_morto_alcance=0`; o caso que varia o esperado para 7 varia somente `fio_morto` antigo. Logo o selo deixa entrar exatamente um limiar próprio na manchete. Reconstruí 18 fixtures em memória e obtive saídas idênticas entre curado e mutante; com alcance medido=7/esperado=7, o mutante acusa indevidamente `FORA DO ESPERADO`. (`bin/fio_estado.py:172`, `:176`; `bin/tests/test_fio_estado.sh:44`, `:48`, `:49`, `:105`, `:106`, `:111`.)

- **Mutante de crash que passa:** retirar `ValueError` do tratamento de data. `medido_em='ontem-de-manha'` passa a levantar `ValueError`, não capturado pelo tratamento externo; seu traceback produz `num()` vazio e satisfaz o caso 15. Verificado em memória. (`bin/fio_estado.py:125`, `:131`, `:111`; `bin/tests/test_fio_estado.sh:135`, `:136`, `:172`, `:173`.)

- **Mutantes de tipo não cobertos:** trocar `type(v) is int` por `isinstance(v, int)` aceitaria bool; relaxar `alcance_cego is False` aceitaria ausência/None/zero conforme a substituição. As fixtures não produzem bool nas contagens nem ausência/None/0 em `alcance_cego`. (`bin/fio_estado.py:71`, `:89`; `bin/tests/test_fio_estado.sh:46`, `:47`, `:53`, `:54`.)

- **Caso 19 é textual:** os três `grep`s verificam fragmentos do command, sem executar a composição nem conferir o retorno de `censo_alcance` e seus tipos. Podem permanecer verdes se o command conservar aqueles textos e a função retornar esquema incompatível. (`bin/tests/test_fio_estado.sh:199`, `:200`, `:201`.)

- **Parte com mordida efetiva:** antigo=0/alcance=4 exige numeral 4 e divergência na primeira linha; antigo=9/alcance=0 exige numeral 0. Um retorno geral à manchete antiga é detectado. O caso 18 também exige rc=0, ausência de traceback e `ILEGIVEL` para as duas formas que realmente testa. (`bin/tests/test_fio_estado.sh:140`, `:141`, `:142`, `:145`, `:146`, `:189`, `:191`, `:192`, `:193`, `:194`.)

Não executei o shell completo porque ele cria e apaga arquivos; a comparação de mutantes foi feita com módulos compilados em memória e `open` substituído por `StringIO`. (`bin/tests/test_fio_estado.sh:32`, `:33`, `:71`, `:98`; execução em memória: `[nao verificado]` como artefato persistente.)

## 31/10 06:43 · sessao 751699ed982a · - · - · origem=Ronald (tmux)

Pergunta 2: **sem achado de incompatibilidade escritor/leitor**; as formas escritas correspondem às lidas. O command monta o registro e é quem grava JSON; `censo_alcance` apenas produz o dict em memória. (`app/chamados/management/commands/vigiar_fio_morto.py:114`, `app/chamados/management/commands/vigiar_fio_morto.py:206`, `app/chamados/services/vigia_fio.py:539`)

| Chave | Quem produz/escreve, nome e tipo | Quem lê |
|---|---|---|
| `alcance` | `censo_alcance` cria dict, retornado e inserido integralmente por `'alcance': alc`. (`app/chamados/services/vigia_fio.py:539`, `app/chamados/services/vigia_fio.py:655`, `app/chamados/management/commands/vigiar_fio_morto.py:218`) | `_alcance` exige dict; `_ler_carimbo` extrai seus campos. (`bin/fio_estado.py:79`, `bin/fio_estado.py:138`) |
| `alcance.alcance_universo` | `len(dias)`, int, inclusive zero. (`app/chamados/services/vigia_fio.py:553`) | Teste de int verdadeiro, `>0`, extração e impressão. (`bin/fio_estado.py:85`, `bin/fio_estado.py:87`, `bin/fio_estado.py:148`, `bin/fio_estado.py:173`) |
| `alcance.alcance_cego` | `not dias`, bool, presente mesmo no retorno antecipado vazio. (`app/chamados/services/vigia_fio.py:554`, `app/chamados/services/vigia_fio.py:555`) | Identidade `is True`/`is not False`. (`bin/fio_estado.py:82`, `bin/fio_estado.py:89`) |
| `alcance.alcance_de_hoje_pra_frente` | int inicial zero, incrementado por dia que entra nessa rota. (`app/chamados/services/vigia_fio.py:540`, `app/chamados/services/vigia_fio.py:604`) | Extraído e impresso sem recalcular. (`bin/fio_estado.py:150`, `bin/fio_estado.py:175`) |
| `censo.fio_morto_alcance` | int inicial zero, incrementado; command copia a mesma chave de `alc` para `censo`. (`app/chamados/services/vigia_fio.py:540`, `app/chamados/services/vigia_fio.py:650`, `app/chamados/management/commands/vigiar_fio_morto.py:216`) | `_alcance` exige int verdadeiro; manchete usa esse valor. (`bin/fio_estado.py:93`, `bin/fio_estado.py:147`, `bin/fio_estado.py:174`) |
| `esperado.fio_morto_alcance` | int literal `0` em `ESPERADO_DO_CARIMBO`, copiado ao registro. (`app/chamados/management/commands/vigiar_fio_morto.py:95`, `app/chamados/management/commands/vigiar_fio_morto.py:211`) | Exige presença; compara valor do próprio carimbo. (`bin/fio_estado.py:91`, `bin/fio_estado.py:172`, `bin/fio_estado.py:176`) |
| `alarma_em_0` | Lista JSON de strings a partir da tupla `ALARMA_EM_0`; atualmente exclui alcance por desenho explícito. (`app/chamados/management/commands/vigiar_fio_morto.py:82`, `app/chamados/management/commands/vigiar_fio_morto.py:92`, `app/chamados/management/commands/vigiar_fio_morto.py:213`) | Converte para lista e consulta pertença para o aviso. (`bin/fio_estado.py:143`, `bin/fio_estado.py:178`) |
| `medido_em` | String ISO com offset, `timezone.localtime(agora).isoformat()`; `agora` é capturado uma vez e alimenta também `hoje` dos censos. (`app/chamados/management/commands/vigiar_fio_morto.py:162`, `app/chamados/management/commands/vigiar_fio_morto.py:167`, `app/chamados/management/commands/vigiar_fio_morto.py:207`) | `fromisoformat`, idade, teto e impressão. (`bin/fio_estado.py:123`, `bin/fio_estado.py:125`, `bin/fio_estado.py:129`, `bin/fio_estado.py:194`) |

**Recorte: sem achado de caminho normal que carimbe `--empresa` como frota.** `empresas` começa `None`; qualquer argumento truthy vira lista, mesmo lista vazia para ids inexistentes, e o carimbo só ocorre sob `empresas is None`. O outro ramo declara “NAO carimbado”. (`app/chamados/management/commands/vigiar_fio_morto.py:152`, `app/chamados/management/commands/vigiar_fio_morto.py:153`, `app/chamados/management/commands/vigiar_fio_morto.py:156`, `app/chamados/management/commands/vigiar_fio_morto.py:205`, `app/chamados/management/commands/vigiar_fio_morto.py:223`)

A frota do alcance já significa o universo de `empresas_visiveis()` e competências da fonte: empresas ativas, sem CNPJ de diagnóstico; o serviço documenta isso e aplica eventual interseção. Não é filtro acrescentado pelo leitor. (`app/chamados/services/vigia_fio.py:513`, `app/chamados/services/vigia_fio.py:535`, `app/chamados/services/vigia_fio.py:536`, `app/chamados/services/vigia_fio.py:547`, `app/colaboradores/queries.py:78`, `app/colaboradores/queries.py:85`)

**Sempre inválido: sem achado de incompatibilidade de esquema.** Há retorno legítimo vazio com universo zero e cegueira verdadeira, que o contrato manda rejeitar; isso depende do dado, não torna toda rodada inválida. (`app/chamados/services/vigia_fio.py:553`, `app/chamados/services/vigia_fio.py:555`, `bin/fio_estado.py:82`)

O destino real pode ser alterado por `settings.VIGIA_FIO_CARIMBO`; o selo do host verifica apenas o caminho default literal. Isso limita a prova de concordância do caso 6, mas não demonstra divergência de produção. (`app/chamados/management/commands/vigiar_fio_morto.py:110`, `bin/tests/test_fio_estado.sh:119`, `bin/tests/test_fio_estado.sh:122`) Configuração ativa de produção: `[nao verificado]`.

Há testes Django existentes que conferem a fotografia do censo e o esperado, recusam carimbo em recorte e verificam alcance real não zero; não foram executados nesta auditoria de leitura. (`app/chamados/tests/test_o261_vigia_fio.py:780`, `app/chamados/tests/test_o261_vigia_fio.py:787`, `app/chamados/tests/test_o261_vigia_fio.py:810`, `app/chamados/tests/test_o261e_f5_alcance.py:444`) Execução: `[nao verificado]`.

Pergunta 4: **sem achado de novo limiar de negócio, filtro de data do universo ou derivação paralela do alcance.** O leitor compara com `esperado` e imprime o contador e o “hoje pra frente” recebidos; as condições de universo e cegueira são as exigidas pelo 3'. (`bin/fio_estado.py:85`, `bin/fio_estado.py:89`, `bin/fio_estado.py:172`, `bin/fio_estado.py:174`, `bin/fio_estado.py:176`) O recorte temporal de “hoje pra frente” já foi feito no serviço. (`app/chamados/services/vigia_fio.py:604`)

## 31/10 04:46 · sessao 512dd43d114e · O261-E-F1-CLASSE-4 · verificador · origem=Code

**Reprovaria o delta4 pelo rollback que apaga efeitos intencionais de `reabrir()`.** A projeção do julgamento também não garante que a lavra ocorrerá depois das escritas que ela autoriza. (`app/chamados/views_arquivar.py:81`; `app/chamados/models.py:1362`; `app/ponto/services/cartorio.py:1376`; `app/ponto/services/cartorio.py:1520`)

Execução dos testes, RED contra a base e incidência em produção: **[nao verificado]**. [nao verificado]

**1. Contraexemplos encontrados**

- **PROCEDENTE — rollback apaga o redirecionamento da recusa.** Chamado encerrado pela tranca, premissa ainda viva, irmão vivo do mesmo turno: `reabrir()` migra resolvedoras para o irmão, escreve nota e trilha de redirecionamento, depois retorna `False`. O novo `set_rollback(True)` desfaz tudo isso junto com o levantamento do véu. Mesmo sem pergunta migrável, a nota de continuidade e a trilha são escritas incondicionalmente nesse ramo e agora desaparecem. Portanto, “recusou” não equivale a “não escreveu nada”. (`app/chamados/models.py:1359`; `app/chamados/models.py:1362`; `app/chamados/models.py:1365`; `app/chamados/models.py:1368`; `app/chamados/models.py:1374`; `app/chamados/views_arquivar.py:81`)

- **PROCEDENTE — ressuscitação pode persistir sem a lavra prevista.** Contraexemplo: célula gravada `concorde`, julgamento atual acusador, pergunta carimbada sem resposta. A projeção permite ressuscitá-la; o `UPDATE` dos carimbos acontece antes da lavra. Depois disso, uma exceção no callback `write`, chamado em `[FIO-RESSUSCITA]`, impede chegar à lavra. `julgar_colab()` tem `finally`, mas não uma transação envolvendo essas duas escritas. O mapa é limpo; a pergunta pode ficar desmarcada sobre a célula antiga, que continua absolvendo. (`app/chamados/models.py:1434`; `app/chamados/models.py:1442`; `app/chamados/models.py:1485`; `app/ponto/services/cartorio.py:746`; `app/ponto/services/cartorio.py:1402`; `app/ponto/services/cartorio.py:1520`; `app/ponto/services/cartorio.py:854`)

  Há também uma chamada de produção entre ressuscitar e lavrar: `_espelhar_marcos()` grava contexto e nota; falha propagada nesse caminho igualmente impede a lavra posterior. Ocorrência dessa falha em produção: **[nao verificado]**. (`app/ponto/services/cartorio.py:1516`; `app/ponto/management/commands/detectar_ausencias.py:558`; `app/ponto/management/commands/detectar_ausencias.py:563`; `app/chamados/services/nota.py:107`)

- **PROCEDENTE — o toque de desarquivar continua invisível para chamado sem `celula_id`.** Contraexemplo: chamado cujo dia vem do contexto, pergunta em competência trancada, mas chamado sem célula ligada. A trilha de desarquivar é escrita, porém o leitor procura exclusivamente chamados ligados às células daquele dia; esse chamado não entra em `chs`. Antecipar a trilha não resolve esse caso. O teste novo liga explicitamente a célula ao chamado. (`app/chamados/juizes.py:102`; `app/chamados/juizes.py:967`; `app/chamados/juizes.py:968`; `app/chamados/juizes.py:972`; `app/chamados/services/arquivo.py:209`; `app/chamados/tests/test_competencia_trancada.py:188`)

- **PROCEDENTE — segundo resgate após retranca pode não produzir novo toque.** Primeiro resgate: véu limpo e primeiro `resgatado_em` preservado. Retrancar avança `fechado_em`; o resgate antigo deixa de proteger o chamado, que pode ser encerrado novamente sem receber novo véu. No segundo clique, `levantar()` retorna sem escrita nem trilha porque o véu está limpo e já existe `resgatado_em`. Na ausência de outro toque posterior à nova tranca, C6 continua calando a pergunta. É lacuna residual do desenho B. (`app/chamados/services/arquivo.py:193`; `app/chamados/services/arquivo.py:196`; `app/chamados/services/arquivo.py:204`; `app/ponto/services/fechamento.py:1383`; `app/chamados/services/competencia_trancada.py:104`; `app/chamados/models.py:1551`; `app/chamados/juizes.py:965`; `app/chamados/juizes.py:1030`)

**2. Quem escreve e quem lê**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `_tl.vereditos[(colab, dia)]` | Cartório calcula pela função da porta e publica a dupla; limpa no `finally`. (`app/ponto/services/cartorio.py:1214`; `app/ponto/services/cartorio.py:857`) | `veredito_deste_julgamento()` → `celula_deste_julgamento()`. (`app/ponto/services/cartorio.py:64`; `app/chamados/juizes.py:394`) |
| `_tl.atas[(colab, dia)]` | Cartório publica `_ata_agora.ata`; limpa no `finally`. (`app/ponto/services/cartorio.py:1148`; `app/ponto/services/cartorio.py:856`) | `ata_deste_julgamento()` → `celula_deste_julgamento()`. (`app/ponto/services/cartorio.py:55`; `app/chamados/juizes.py:393`) |
| `veredito/veredito_via` da cópia | `celula_deste_julgamento()` atribui a dupla projetada. No registro persistido, a lavra atribui esses campos. (`app/chamados/juizes.py:402`; `app/ponto/portas/celula.py:710`) | C6 lê veredito e via; o coletor da premissa também recebe a cópia, mas sua leitura de veredito limita-se à via `admin`. (`app/chamados/juizes.py:1038`; `app/chamados/juizes.py:1074`; `app/chamados/models.py:1230`) |
| `ata` da cópia | Recebe diretamente o dicionário publicado em `_tl.atas`. Na persistência, a lavra escreve ata e `carimbar_precedencia()` acrescenta precedência pela mesma porta. (`app/chamados/juizes.py:400`; `app/ponto/portas/celula.py:708`; `app/ponto/portas/celula.py:759`) | Guarda do marco, C6 via marcos pendentes, coletor da premissa e `_celula_furada()`. (`app/chamados/juizes.py:886`; `app/chamados/juizes.py:1088`; `app/chamados/models.py:1236`; `app/chamados/juizes.py:443`) |
| `Pergunta._state.fields_cache` | O queryset usa `select_related`; a cópia recebe `_state` e dicionário próprios antes de atribuir `_q.celula`. (`app/chamados/models.py:1424`; `app/chamados/models.py:1430`; `app/chamados/models.py:1431`; `app/chamados/models.py:1434`) | Acesso às relações na avaliação; `_chave_memo_pv()` também consulta explicitamente o cache de `chamado_destino`. (`app/chamados/juizes.py:777`; `app/chamados/juizes.py:778`; `app/chamados/juizes.py:779`) |
| Log `cobrar_dia_manual` | `cobrar_dia()` → `registrar_log()` → `evento()` → `LogAuditoria.objects.create()`. (`app/chamados/services/cobrar_dia.py:37`; `app/core/auditoria.py:22`; `app/core/observ.py:134`) | `_admin_tocou_o_dia_depois()`: usuário humano, posterior à tranca, colaborador e substring `dia=ISO`. (`app/chamados/juizes.py:965`; `app/chamados/juizes.py:969`) |
| Log `desarquivar_chamado` | View → `arquivo.levantar()` → `_trilha()` → o mesmo escritor `evento()`. (`app/chamados/views_arquivar.py:73`; `app/chamados/services/arquivo.py:209`; `app/chamados/services/arquivo.py:137`) | O mesmo leitor de toque, restringindo os objetos aos chamados ligados às células do dia. (`app/chamados/juizes.py:968`; `app/chamados/juizes.py:972`) |

`celula_do_dia_agora()` passa efetivamente a devolver também o veredito projetado. Nos dois leitores anteriores encontrados, isso não muda a decisão normal: `_celula_furada()` usa ata/trabalho; `premissa_regularizada()` só distingue a via `admin`, preservada pela projeção. (`app/chamados/juizes.py:378`; `app/chamados/juizes.py:443`; `app/chamados/models.py:1230`; `app/ponto/portas/celula.py:656`)

Na ressuscitação, a mudança funcional é real e coerente com o objetivo A: tanto o guarda do marco quanto C6 recebem a célula da pergunta com ata/veredito em curso. A ressalva é a falta de garantia de lavra posterior demonstrada acima. (`app/chamados/models.py:1434`; `app/chamados/models.py:1436`; `app/chamados/models.py:1442`; `app/ponto/services/cartorio.py:1520`)

**3. Igualdade entre previsão e lavra**

No caminho que chega normalmente à lavra, com os mesmos insumos, **a dupla final coincide**: publicação e lavra recebem `cel`, `via='cartorio'`, `_vd` e `cods`; a lavra chama a mesma função de derivação. Admin é preservado nos dois caminhos. (`app/ponto/services/cartorio.py:1214`; `app/ponto/services/cartorio.py:1520`; `app/ponto/portas/celula.py:698`; `app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:692`)

Isso não significa que sempre haja gravação:

- **`apply_=False`:** publica os mapas, mas não entra na lavra. Um callback `write` pode consultá-los durante os relatórios; consumidor de produção que faça essa consulta: **[nao verificado]**. (`app/ponto/services/cartorio.py:1214`; `app/ponto/services/cartorio.py:1463`; `app/ponto/services/cartorio.py:1501`; `app/ponto/services/cartorio.py:1517`)
- **Sem vivos:** não impede lavrar; a condição final exige apenas `apply_`. (`app/ponto/services/cartorio.py:1517`)
- **Exceção:** pode ocorrer depois da leitura da previsão e do `UPDATE` da pergunta, antes da lavra, conforme o contraexemplo acima. (`app/ponto/services/cartorio.py:1376`; `app/ponto/services/cartorio.py:1402`; `app/ponto/services/cartorio.py:1520`)
- **Admin ou repetição idempotente:** a lavra pode preservar a dupla e escrever apenas ata, ou retornar sem escrever. (`app/ponto/portas/celula.py:692`; `app/ponto/portas/celula.py:697`; `app/ponto/portas/celula.py:705`)
- **`calado`:** somente altera/restaura `_tl.ativo`; não limpa os mapas nem impede diretamente uma chamada explícita ao julgamento. Os signals consultam esse estado para evitar reentrada. (`app/ponto/services/cartorio.py:72`; `app/ponto/services/cartorio.py:77`; `app/chamados/signals.py:174`)

A ata projetada também não é literalmente toda a ata final: a lavra preserva chaves de outra operação, como `precedencia`, enquanto o mapa recebe a ata calculada antes desse merge. Efeito adverso dessa diferença nos leitores desta obra: **[nao verificado]**. (`app/ponto/services/cartorio.py:1148`; `app/ponto/portas/celula.py:94`; `app/ponto/portas/celula.py:684`; `app/ponto/portas/celula.py:686`)

**4. Cópia rasa, vazamento e `save()`**

A troca de FK na pergunta está protegida pela separação explícita de `_state` e `fields_cache`. A simulação retorna IDs; a escrita real acontece por outro queryset, sem salvar `_q` ou sua célula projetada nesses caminhos. (`app/chamados/models.py:1430`; `app/chamados/models.py:1431`; `app/chamados/models.py:1434`; `app/chamados/models.py:1443`; `app/chamados/models.py:1483`; `app/chamados/models.py:1485`)

**DUVIDA — isolamento de objetos mutáveis não é garantido.** A cópia é rasa e a ata projetada é o próprio dicionário do mapa: uma operação como `out.ata['lampadas'].append(...)` alteraria a ata compartilhada; mutação aninhada no DNA compartilhado poderia atingir o original. Não encontrei essas escritas nos consumidores atuais, que percorrem os valores para leitura. (`app/chamados/juizes.py:398`; `app/chamados/juizes.py:400`; `app/chamados/juizes.py:869`; `app/chamados/juizes.py:886`; `app/ponto/services/lastro.py:148`)

A cópia não recebe uma proteção contra persistência: continua sendo uma cópia do model. Um caminho atual que execute `save()` nela: **[nao verificado]**; os consumidores encontrados são o carregador da premissa e a simulação da ressuscitação. (`app/chamados/juizes.py:378`; `app/chamados/models.py:1434`)

**5. Ordem do admin, rollback e cobrança sem escala**

Levantar antes não altera diretamente as guardas de reabertura por `arquivado/resgatado/silenciado`: as guardas consultam veto, decisão administrativa, premissa, agrupamento, irmão e transição, sem ler esses campos do véu. A alteração pretendida atua depois, na ressuscitação que consulta a trilha. (`app/chamados/models.py:1285`; `app/chamados/models.py:1290`; `app/chamados/models.py:1296`; `app/chamados/models.py:1299`; `app/chamados/models.py:1302`; `app/chamados/models.py:1306`; `app/chamados/models.py:1389`)

O rollback abrange as escritas de banco feitas dentro daquele bloco, inclusive levantamento, logs e os efeitos do ramo de redirecionamento. Por isso resolve a compensação do véu, mas introduz a regressão do item 1. Não se deve confundir isso com desfazer atributos Python ou efeitos externos; garantia desses últimos: **[nao verificado]**. (`app/chamados/views_arquivar.py:65`; `app/chamados/views_arquivar.py:81`; `app/chamados/services/arquivo.py:201`; `app/chamados/models.py:1362`)

Em `cobrar_dia`, **sim, fica uma trilha mesmo quando não existe célula**: o log antecede o julgamento, que retorna `None` nesse caso. Não demonstrei violação de contador/selo: o teste existente exige uma linha por toque, explicitamente “não por efeito”. Esse toque poderá ser reconhecido depois se a célula passar a existir, pois o leitor não exige sucesso da cobrança original. (`app/chamados/services/cobrar_dia.py:37`; `app/chamados/services/cobrar_dia.py:40`; `app/ponto/services/cartorio.py:1658`; `app/chamados/tests/test_o44_tranca_nao_barra_o_toque.py:275`; `app/chamados/juizes.py:967`; `app/chamados/juizes.py:969`)

**DUVIDA — trilha obrigatória sobre escritor best-effort.** Ambos os fluxos prosseguem sem confirmar a criação do log; `evento()` pode falhar isoladamente e retornar `None`. Contraexemplo condicional: falha ao inserir a trilha, negócio prossegue, C6 continua enxergando “sem toque”. Ocorrência em produção: **[nao verificado]**. (`app/core/observ.py:105`; `app/core/observ.py:133`; `app/core/observ.py:139`; `app/chamados/services/cobrar_dia.py:39`; `app/chamados/services/arquivo.py:209`; `app/chamados/services/arquivo.py:210`; `app/chamados/juizes.py:1030`)

**DUVIDA — memo anterior ao toque.** Consultar o toque, gravar nova trilha e consultar novamente no mesmo dono de `_tranca_memo` conserva o resultado anterior. A nova ordem não invalida esse cache. Rota HTTP atual que necessariamente execute essa sequência: **[nao verificado]**. (`app/chamados/juizes.py:943`; `app/chamados/juizes.py:947`; `app/chamados/juizes.py:959`; `app/chamados/juizes.py:973`; `app/chamados/services/cobrar_dia.py:37`)

**6. Checklist das classes anteriores**

| Classe | Resultado desta leitura |
|---|---|
| Segundo juiz registrado | Não demonstrado: a ressuscitação consulta o C6 registrado; a previsão reutiliza a derivação chamada pela lavra. (`app/core/juizes.py:238`; `app/chamados/models.py:1442`; `app/ponto/portas/celula.py:698`) |
| Segundo escritor fora da porta | Não demonstrado na persistência nova: a projeção escreve memória; pergunta é atualizada pela porta existente; logs convergem para `evento()`. (`app/chamados/juizes.py:400`; `app/chamados/models.py:1485`; `app/core/observ.py:134`) |
| Relógio fora da borda | Nova leitura de relógio nos trechos acrescentados: **[nao verificado]**. O helper recebe insumos e deriva a dupla sem relógio; o julgamento já recebe `hoje/agora`. (`app/ponto/portas/celula.py:647`; `app/ponto/services/cartorio.py:749`) |
| Derivação paralela | Não demonstrada para o veredito de cartório: previsão e lavra chamam a mesma função. (`app/ponto/services/cartorio.py:1214`; `app/ponto/portas/celula.py:698`) |
| Selo/RED sem mordida | **DUVIDA:** o teste de isolamento não cria pergunta nem exercita a troca de FK/`fields_cache`; verifica atributos da célula. Os testes novos também não cobrem retranca, rollback de redirecionamento ou exceção após ressuscitar. RED executado: **[nao verificado]**. (`app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:185`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:197`; `app/chamados/tests/test_competencia_trancada.py:177`) |
| Ordem de trava | **DUVIDA:** a nova transação mantém a escrita do chamado desde `levantar`; `abrir()` possui trava da pessoa. Um novo ciclo concreto de deadlock causado por este delta: **[nao verificado]**. (`app/chamados/views_arquivar.py:65`; `app/chamados/services/arquivo.py:208`; `app/chamados/models.py:864`) |
| Pouso antes do pré-requisito | **PROCEDENTE:** a pergunta pode ser ressuscitada pela previsão antes de a lavra estar garantida; exceção posterior deixa a escrita sem seu fundamento persistido. (`app/ponto/services/cartorio.py:1376`; `app/ponto/services/cartorio.py:1402`; `app/ponto/services/cartorio.py:1520`) |

_901 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
