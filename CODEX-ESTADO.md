# CODEX-ESTADO

> Ultima resposta processada: **08/11/2026 12:12** · publicado em **08/11/2026 12:12** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 08/11 12:12 · sessao b0a85d04359f · FIO-E0-V4-E1 · verificador · origem=Code

**REPROVADA a garantia universal da 15b; PROCEDENTE a cura dos contraexemplos nomeados nas portas alteradas.** Permanecem o retorno da chave com A2 vazio, a emissão de conversa sobre retratada e a admissão de retratada no fecho. (`app/chamados/services/fio.py:1310`, `app/chamados/services/disputa_emissao.py:431`, `app/chamados/services/fio.py:1467`)

Os contraexemplos abaixo são construções pelo código. Ocorrência deles, números reais e migração integral em produção: **[nao verificado]**.

**1. Itens 3 e 5 anteriores: cura PROCEDENTE nos casos específicos; universalidade NÃO PROCEDENTE.**

| Contraexemplo | Trajeto na 15b |
|---|---|
| **3a, episódio retratado** | `_nascer` deriva a origem e consulta o helper; este chama a recusa, que transforma o dia nulo em `BatidaRetratadaNaoEFato`. Não alcança a busca nem o `create`. Repetindo sem mudança dos insumos, repete a recusa. (`app/chamados/models.py:2760`, `app/chamados/models.py:2769`; `app/chamados/services/fio.py:1301`, `app/chamados/services/fio.py:1268`; escrita em `app/chamados/models.py:2867`) |
| **3b, K sem episódio, respondida, gravada D0, admitida em DIA por A2** | O episódio vivo do pedido fornece DIA; o universo alcança K, a eleição prefere sua resposta e a costura recebe explicitamente `_dia_fato=DIA`. Não relê D0 como identidade nessa costura. (`app/chamados/models.py:2769`, `app/chamados/models.py:2800`, `app/chamados/models.py:2814`, `app/chamados/models.py:2829`; `app/chamados/services/fio.py:1005`; `app/chamados/juizes.py:2006`) |
| **Variante R, canônica respondida de episódio retratado** | O fecho ainda pode eleger R, mas `_nascer` chama `recusa_batida_retratada(ex)` antes de alterar célula ou destino, inclusive sem destino pedido. Agora recusa pelo motivo correto. (`app/chamados/models.py:2814`, `app/chamados/models.py:2819`, `app/chamados/models.py:2822`) |
| **3a/3b originais, episódio vivo** | Dia, célula, busca e nascimento usam `_dia_fato`; no reuso, a costura recebe esse mesmo dia. A data gravada antiga é preservada. (`app/chamados/models.py:2769`, `app/chamados/models.py:2789`, `app/chamados/models.py:2800`, `app/chamados/models.py:2829`, `app/chamados/models.py:2867`; `app/chamados/tests/test_e1_v15.py:98`) |

A resolvedora chamada diretamente também passou a consultar A2 para K e a recusar R. (`app/chamados/juizes.py:2008`; `app/chamados/services/fio.py:1301`, `app/chamados/services/fio.py:1309`)

**2. Retorno do pedido/gravado: PROCEDENTE para acervo com A2 vazio.**

Para linha de **fato com episódio**, não há mais esse fallback: recusa quando o juiz não responde; caso contrário, retorna `_dia_do_fato`. Conversa retorna antes desse ramo, portanto continua dispensada mesmo se receber origem explícita. (`app/chamados/services/fio.py:1298`, `app/chamados/services/fio.py:1300`)

Para acervo sem episódio e sem marco, permanece literalmente:

```python
alc = _dias_alcancados_sem_dia(...)
if not alc:
    return d
```

`d` é pedido, quando fornecido, senão gravada. Logo, “nenhum caminho devolve pedido/gravado para acervo” é **NÃO PROCEDENTE**. (`app/chamados/services/fio.py:1297`, `app/chamados/services/fio.py:1309`)

**A admissão do vazio é compartilhada; a atribuição de um dia não é a mesma operação.** Universo e fecho aceitam alcance vazio porque `set() <= {dia}`. Isso prova compatibilidade com um dia fornecido, não identifica um dia único. Sem dia fornecido e sem alcance, o fecho conserva `_dia=None`. (`app/chamados/services/fio.py:1467`, `app/chamados/services/fio.py:1425`, `app/chamados/services/fio.py:1413`)

Contraexemplo: K isolada, sem episódio/marco, gravada D0. O helper retorna D0; com `pedido=DIA`, retorna DIA. O fecho de K sem dia não infere nenhum; com `dia=DIA`, conserva K. Portanto o helper escolhe a chave onde A2 não respondeu um dia. Esse percurso foi reproduzido em memória com as funções reais e vizinhança vazia; persistência Django: **[nao verificado]**. (`app/chamados/services/fio.py:1297`, `app/chamados/services/fio.py:1311`, `app/chamados/services/fio.py:1414`, `app/chamados/services/fio.py:1433`)

**3. Censo de chamadores e efeito da recusa.**

Censo das chamadas explícitas de produção, incluindo os apelidos encontrados; chamadas dinâmicas arbitrárias: **[nao verificado]**.

| Função | Chamador e efeito |
|---|---|
| `resolvedora_valida` | **Plano de fechamento:** uma pergunta retratada que precise reatar deixa de ter destino admissível. (`app/chamados/models.py:392`) |
| `resolvedora_valida` | **Nascimento, proxy inicial:** usa o dia já calculado; a origem retratada já foi recusada pelo helper. (`app/chamados/models.py:2769`, `app/chamados/models.py:2781`) |
| `resolvedora_valida` | **Reuso:** usa o dia explícito do fato, após recusa da canônica retratada. (`app/chamados/models.py:2819`, `app/chamados/models.py:2829`) |
| `resolvedora_valida` | **Reapontamento:** retratada com destino não nulo recusa; descostura sem datação continua permitida pelo retorno antecipado para `chamado=None`. (`app/chamados/services/fio.py:865`; `app/chamados/juizes.py:1999`) |
| `resolvedora_valida` | **Rechaveamento:** recusa retratada com destino; sem destino, retorna antes da recusa e pode persistir o dia recebido, passando as demais guardas. (`app/chamados/services/fio.py:970`, `app/chamados/services/fio.py:974`; `app/chamados/juizes.py:1999`) |
| `resolvedora_valida` | **Auditoria I8b:** retratada viva com destino/data passa pela classificação de resolvedora inválida. (`app/chamados/management/commands/auditar_invariantes_chamados.py:891`, `app/chamados/management/commands/auditar_invariantes_chamados.py:896`) |
| `resolvedora_valida` | **Auditoria I22:** chamado vivo cujas resolvedoras de fato sejam todas recusadas entra na violação; conversas são excluídas. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1045`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1048`) |
| `dia_do_turno_da_linha` | **Resolvedora**, **nascimento** e **datação no reapontamento**. (`app/chamados/juizes.py:2008`; `app/chamados/models.py:2769`; `app/chamados/services/fio.py:846`) |
| `recusa_batida_retratada` | **Resolvedora com dia explícito**, **reuso da canônica**, **ramo de episódio do helper** e **confirmação do contador**. (`app/chamados/juizes.py:2005`; `app/chamados/models.py:2819`; `app/chamados/services/fio.py:1301`, `app/chamados/services/fio.py:1544`) |

**Fechamento recusado: PROCEDENTE, condicionalmente.** O caminho exato é:

1. `_ancoradas` reúne não validadas pelo destino ou pelo chamado da disputa. `_vivas_entre` conserva as de competência aberta que continuam vivas pelo C6 no pós-estado. (`app/chamados/models.py:344`, `app/chamados/models.py:364`, `app/chamados/models.py:368`)
2. O plano procura sobrevivente usando **data gravada**; havendo sobrevivente, consulta a resolvedora. (`app/chamados/models.py:390`, `app/chamados/models.py:392`)
3. Resolvedora → helper → recusa retratada → `(False, texto)`. O plano devolve `reatas=None`. (`app/chamados/juizes.py:2008`, `app/chamados/juizes.py:2009`; `app/chamados/services/fio.py:1301`; `app/chamados/models.py:393`)
4. O gate devolve `False`; reconciliação/cancelamento abortam antes de gravar o fechamento. (`app/chamados/models.py:422`, `app/chamados/models.py:431`, `app/chamados/models.py:685`, `app/chamados/models.py:786`)

Isso pode impedir um fechamento que antes conseguiria reatar a pergunta. Não vale para toda retratada: ela precisa sobreviver aos filtros anteriores. Sem viva, o gate permite. (`app/chamados/models.py:364`, `app/chamados/models.py:368`, `app/chamados/models.py:420`)

O motivo específico é perdido nesse plano: o registro usa `pergunta_viva_na_competencia_aberta` e afirma ausência de cobrança receptora, inclusive quando a cobrança existe e a pergunta foi recusada. (`app/chamados/models.py:392`, `app/chamados/models.py:425`)

**4. Caminho que ainda gera conversa sobre retratada: PROCEDENTE.**

Contraexemplo: `ultima_batida.id=br` retratada, motivo `gps_ausente`, sem data no contexto do motivo e sem conversa anterior. O motivo é BATIDA e sua resposta é `escolha`. (`app/chamados/catalogo/motivos.py:42`; `app/chamados/catalogo/perguntas.py:88`)

O derivador aceita a origem pelo dono, sem filtrar retratação; assim, não entra no dedup inicial reservado à ausência de episódio. O juiz retorna `None`, `_dia_p` fica nulo e somente perguntas de `hora` são descartadas. (`app/chamados/models.py:3316`; `app/chamados/services/disputa_emissao.py:325`, `app/chamados/services/disputa_emissao.py:391`, `app/chamados/services/disputa_emissao.py:424`)

O emissor então chama **`abrir_conversa`**. A conversa não deriva origem, o helper retorna o dia civil antes da recusa e a resolvedora também aprova conversa antecipadamente. É o bug no chamador abrangido pelo aval fornecido. (`app/chamados/services/disputa_emissao.py:431`, `app/chamados/services/disputa_emissao.py:439`; `app/chamados/models.py:2756`, `app/chamados/models.py:3297`; `app/chamados/services/fio.py:1298`; `app/chamados/juizes.py:1997`)

Também há reabertura sem a recusa nova: pergunta respondida sobre retratada, sem materialização, pode entrar em `perguntas_corrigiveis`, alcançar `reabrir_por_colab` e voltar a pendente pelo `update` de `reabrir_para_correcao`. Se “reatar pergunta” abrange voltar a pedi-la, é outro escape. (`app/chamados/juizes.py:1412`, `app/chamados/juizes.py:1245`; `app/chamados/models.py:2226`, `app/chamados/models.py:2235`; `app/chamados/services/disputa_emissao.py:1494`, `app/chamados/services/disputa_emissao.py:1533`)

**5. Ramo antigo `FatoEmOutroDia`: inalcançável com identidade íntegra; não há prova universal de inacessibilidade.**

Com episódio vivo, o helper obtém D e o memo por episódio fornece D às linhas desse episódio. Havendo linha da mesma base, ela semeia `datadas`; o universo não fica vazio. Episódio retratado recusa antes. Portanto o comentário de que o ramo “sobra só para retratada” ficou obsoleto. (`app/chamados/services/fio.py:1243`, `app/chamados/services/fio.py:1465`, `app/chamados/services/fio.py:1481`, `app/chamados/services/fio.py:1301`; `app/chamados/models.py:2807`)

Há, porém, este **contraexemplo de acervo inconsistente**:

- P pertence à disputa de C2 e tem origem explícita b, batida viva de C1. O `save` confere colaborador contra disputa, mas só deriva a origem quando ausente; a checagem do dono existe no derivador. (`app/chamados/models.py:3282`, `app/chamados/models.py:3297`, `app/chamados/models.py:3316`)
- `abrir(C1,b)` obtém D. `do_fato` encontra P pelo episódio/motivo sem filtrar colaborador. (`app/chamados/models.py:2769`, `app/chamados/models.py:2647`)
- O universo recusa P pela **base C2**, fica vazio e entrega P nas recusadas. O ramo antigo encontra o mesmo episódio e levanta `FatoEmOutroDia`, embora a divergência seja de dono. (`app/chamados/services/fio.py:1463`, `app/chamados/services/fio.py:1481`, `app/chamados/services/fio.py:1486`; `app/chamados/models.py:2802`, `app/chamados/models.py:2810`)

Esse acervo e um chamador atual de produção que o persista: **[nao verificado]**. A porta `abrir` 15b de C2 já recusaria a origem incompatível antes desse nascimento. (`app/chamados/services/fio.py:1248`, `app/chamados/services/fio.py:1301`)

**6. Retratada admitida no fecho por A2: PROCEDENTE.**

`_dia_do_fato` retorna nulo para a retratada; universo e fecho tratam esse nulo pelo mesmo ramo de A2, sem chamar a recusa. A premissa do teste novo confirma que R pode ser a canônica do universo de DIA. (`app/chamados/services/fio.py:1248`, `app/chamados/services/fio.py:1467`, `app/chamados/services/fio.py:1425`; `app/chamados/tests/test_e1_v15b.py:132`)

Consequências concretas:

- **Eleição:** R respondida vence A viva; `_nascer` recusa R depois da eleição, sem tentar outra canônica. Uma solicitação legítima sobre A pode, portanto, ser barrada por R. (`app/chamados/services/fio.py:1005`; `app/chamados/models.py:2814`, `app/chamados/models.py:2819`)
- **Dedup/backfill:** continuam elegendo sobre esse universo. O plano conta R como episódio; A+R pode produzir `episodios_unidos`, impedindo retratação. (`app/chamados/management/commands/dedup_perguntas_fato.py:76`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:265`; `app/chamados/services/fio.py:1060`, `app/chamados/services/fio.py:1065`)
- **Migração de resolvedoras:** também elege R; quando tenta reapontá-la, pode receber a recusa nova. A eleição em si não foi saneada. (`app/chamados/services/fio.py:1753`, `app/chamados/services/fio.py:1772`)
- **Migração de data:** R pode integrar o universo do grupo e seu dia gravado participar do veto de competência. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:254`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:277`)

**7. Contador: PROCEDENTE no universo declarado; não conta todas as perguntas/conversas sobre retratada.**

Conta linhas persistidas com `conversa=False`, origem preenchida, fora de `VIAS_FORA_DO_FATO`, cuja origem esteja fora de `batidas_apuraveis(None)`. Não filtra viva, resposta, validação, destino ou horizonte. (`app/chamados/services/fio.py:1537`)

A autoridade consultada inclui todos os colaboradores e define apuráveis por `retratada_em IS NULL`. Cada candidata é confirmada pela **função real** da recusa, acumulando somente `BatidaRetratadaNaoEFato`. (`app/ponto/turnos.py:2865`, `app/ponto/turnos.py:2871`; `app/chamados/services/fio.py:1544`)

O leitor imprime número, esperado, motivo e pks no backfill, antes da distinção entre DRY e apply. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:534`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:546`)

Não conta tentativas recusadas sem INSERT, conversas do escape do item 4 ou linhas já fora do fato. Portanto não serve como prova de ausência universal do bug nem como contador de tentativas recusadas. (`app/chamados/services/fio.py:1537`, `app/chamados/services/fio.py:1538`; `app/chamados/services/disputa_emissao.py:439`)

**8. Catraca: PROCEDENTE a mordida oficial; garantia semântica completa NÃO PROCEDENTE.**

Os 11 métodos AST de P13/V15 passaram na execução em memória, substituindo `SimpleTestCase` por `unittest.TestCase`, sem executar Django/BD. Isso inclui os 13 mutantes oficiais e o mutante adicional do parâmetro do fecho. Suíte Django/BD: **[nao verificado]**. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:141`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:235`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:263`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:304`)

Os escapes demonstrados em memória incluem:

| Escape | Por que passa |
|---|---|
| `alc = set()` | Fiscaliza formato dos retornos, não a procedência de `alc`; passa a devolver sempre a chave no acervo. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:148`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:157`) |
| Recusa chamada com proxy `conversa=True` | Verifica nome/posição, não argumentos; esse proxy é dispensado pela recusa. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:164`; `app/chamados/services/fio.py:1266`) |
| Guarda inicial da resolvedora trocada por `if True` | A fiscalização das atribuições não prova que o fluxo chega nelas. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:121`; retorno em `app/chamados/juizes.py:1997`) |
| `_dia += timedelta(days=1)` | A ligação ao juiz só acompanha `Assign`, ignorando `AugAssign`. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:71`) |
| `p.data_evento: object = data_evento` dentro da porta | V15 ignora `AnnAssign`; P13 detecta localização e aceita a porta. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:90`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:84`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:119`) |
| `update(**kw)` / `bulk_update(ps, campos)` com variáveis | P13 não acompanha os valores dessas variáveis. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:93`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:105`) |

Rechaveamento continua fiscalizado pelo **arquivo chamador**, sem provar o valor recebido ou escrito. A aceitação explícita de `return d` com A2 vazio também mantém verde o problema do item 2. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:217`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:260`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:155`)

**9. Escritores, leitores e significados.**

| Campo/valor | Quem escreve/fornece | Quem lê |
|---|---|---|
| `data_evento` | Nascimento `_dia_fato`; datação `_dia`; rechaveamento `novo_dia`. Apply obtém juiz; reversão obtém arquivo. (`app/chamados/models.py:2867`; `app/chamados/services/fio.py:849`, `app/chamados/services/fio.py:968`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:412`) | Busca/índice pela gravada; marco pela gravada; C6, tranca e plantio continuam lendo gravada. Costura usa helper ou parâmetro explícito. (`app/chamados/models.py:2678`, `app/chamados/models.py:3332`; `app/chamados/services/fio.py:1250`; `app/chamados/juizes.py:336`, `app/chamados/juizes.py:948`, `app/chamados/juizes.py:2006`; `app/chamados/services/materializacao.py:345`) |
| `celula` | Nascimento/reuso escolhem por `_dia_fato`; rechaveamento por `novo_dia`. (`app/chamados/models.py:2789`, `app/chamados/models.py:2823`, `app/chamados/models.py:2868`; `app/chamados/services/fio.py:965`, `app/chamados/services/fio.py:969`) | C6 exige compatibilidade entre dia da célula e `data_evento`; guarda terminal consulta o pós-estado. (`app/chamados/juizes.py:638`; `app/chamados/services/fio.py:779`) |
| `chamado_destino` | Nascimento, reuso após guardas, reapontamento. (`app/chamados/models.py:2868`, `app/chamados/models.py:2839`; `app/chamados/services/fio.py:877`) | Âncora do fio, fechamento, guarda terminal e plantio. (`app/chamados/services/fio.py:117`; `app/chamados/models.py:345`; `app/chamados/services/fio.py:770`; `app/chamados/services/materializacao.py:339`) |
| `batida_origem_id` | Explícita ou derivada antes do helper; inserção deriva quando ausente. (`app/chamados/models.py:2760`, `app/chamados/models.py:3297`) | Busca do episódio, juiz, recusa, contador, chave da família e veto de episódios unidos. (`app/chamados/models.py:2647`; `app/chamados/services/fio.py:1242`, `app/chamados/services/fio.py:1266`, `app/chamados/services/fio.py:1537`, `app/chamados/services/fio.py:1131`, `app/chamados/services/fio.py:1060`) |
| `dia_da_chave` | Helper fornece pedido/gravada. (`app/chamados/services/fio.py:1297`, `app/chamados/services/fio.py:1309`) | Modifica a consulta inicial de vizinhança e sua chave de cache; passos seguintes usam as chaves dos vizinhos. (`app/chamados/services/fio.py:1336`, `app/chamados/services/fio.py:1358`, `app/chamados/services/fio.py:1367`, `app/chamados/services/fio.py:1204`) |
| Parâmetro `dia_do_fato` | `_nascer` fornece `_dia_fato` ao proxy e à canônica. (`app/chamados/models.py:2781`, `app/chamados/models.py:2829`) | Resolvedora faz a recusa e usa o parâmetro diretamente. (`app/chamados/juizes.py:2004`) |

**Divergência de significado residual: PROCEDENTE.** Reuso pode atribuir célula/destino de DIA e conservar `data_evento=D0`; a costura aceita DIA, C6 descarta a célula divergente e tranca calcula competência por D0. O teste preserva expressamente essa divergência. (`app/chamados/models.py:2823`, `app/chamados/models.py:2839`; `app/chamados/tests/test_e1_v15.py:98`; `app/chamados/juizes.py:2006`, `app/chamados/juizes.py:638`, `app/chamados/juizes.py:948`)

O uso civil da conversa tem discriminador explícito. A premissa de que todo fato já teria “gravada == juiz” não está demonstrada pela cópia. (`app/chamados/models.py:2717`, `app/chamados/models.py:2722`; `app/chamados/tests/test_e1_v15.py:99`)

**10. Classes de reprova.**

| Classe | Veredito |
|---|---|
| **S

[truncado em 20000 de 22550 caracteres]

## 08/11 12:09 · sessao 2c277a08ce1c · - · - · origem=Ronald (tmux)

**7. Contador — PROCEDENTE no universo declarado, insuficiente como contador de recusas/tentativas.**

- Conta linhas persistidas `conversa=False`, com `batida_origem`, fora de `VIAS_FORA_DO_FATO`, cujo episódio não está em `batidas_apuraveis(None)`. Não exige pergunta viva, não validada, horizonte, destino ou ausência de resposta. (`app/chamados/services/fio.py:1537`; `app/chamados/services/fio.py:1540`)
- `VIAS_FORA_DO_FATO` contém somente `duplicata` e `motivo_retratado`. (`app/chamados/models.py:2318`)
- `batidas_apuraveis(None)` inclui todos os colaboradores e filtra `retratada_em IS NULL`; portanto o complemento seleciona episódios retratados, sob integridade normal da FK. (`app/ponto/turnos.py:2865`; `app/ponto/turnos.py:2871`)
- Cada candidata é confirmada pela função real `recusa_batida_retratada`; somente a exceção `BatidaRetratadaNaoEFato` acumula o pk. Retorna n, esperado=0, motivo e pks. (`app/chamados/services/fio.py:1542`; `app/chamados/services/fio.py:1548`)
- O leitor de produção imprime `PERGUNTA_SOBRE_RETRATADA dry`, antes de distinguir `--aplicar`; logo também aparece no apply. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:534`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:546`)
- Não conta tentativas recusadas sem INSERT, nem conversas sobre batida retratada, nem linhas já marcadas duplicata/motivo_retratado. Esse alcance resulta dos filtros, independentemente do título do contador. (`app/chamados/services/fio.py:1537`; `app/chamados/services/fio.py:1546`)
- N real de produção: **[nao verificado]**.

**8. Catraca — PROCEDENTE a mordida dos mutantes oficiais; NÃO PROCEDENTE como garantia semântica universal.**

Executei exclusivamente os ASTs em memória, com `python3 -B`, substituindo `SimpleTestCase` por `unittest.TestCase` e removendo os imports Django. Os 11 métodos passaram: três P13 e oito V15, incluindo os 13 mutantes de `test_a_catraca_MORDE` e o mutante adicional de `dia_do_fato`. Isso verifica a lógica AST desses métodos, sem BD ou execução dos caminhos Django. Os métodos/mutantes estão em `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:141`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:235`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:263`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:304`. Suíte Django/BD: **[nao verificado]**.

Censo real encontrado pelos ASTs:

| Local | Linha |
|---|---|
| `_nascer`, proxy | `app/chamados/models.py:2777` |
| `_nascer`, INSERT | `app/chamados/models.py:2867` |
| `reapontar_resolvedora`, valor | `app/chamados/services/fio.py:849` |
| `reapontar_resolvedora`, desfazimentos em memória | `app/chamados/services/fio.py:861`, `app/chamados/services/fio.py:870` |
| `rechavear_data_evento`, valor/save | `app/chamados/services/fio.py:968`, `app/chamados/services/fio.py:974` |

Os novos mutantes detectam regressões literais em setattr/bulk, retirada da recusa, fallback `or d`, mudança da guarda de A2 e leitura da gravada no parâmetro do fecho. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:275`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:295`)

Escapes demonstrados em memória, todos com detector retornando `[]`:

1. Trocar a origem de `alc` por `alc = set()`. O helper passa a devolver sempre a chave no ramo acervo; a catraca verifica formas dos returns/guardas, não a origem ou conteúdo de `alc`. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:148`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:176`; escrita real de alc em `app/chamados/services/fio.py:1309`)
2. Chamar `recusa_batida_retratada` com proxy `conversa=True`, em vez de p. A catraca verifica nome/posição da chamada, não argumentos; a recusa retorna imediatamente nesse proxy. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:164`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:169`; `app/chamados/services/fio.py:1266`)
3. Trocar a guarda inicial de conversa da resolvedora por `if True`, preservando as atribuições abaixo. A catraca não verifica early returns/controle de fluxo completo. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:121`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:138`; retorno real em `app/chamados/juizes.py:1997`)
4. Dentro da porta, `p.data_evento: object = data_evento`: V15 ignora `AnnAssign`; P13 detecta a escrita, mas aceita sua localização. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:89`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:95`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:84`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:119`)
5. Após `_dia = dia_do_turno_da_linha(...)`, acrescentar `_dia += datetime.timedelta(days=1)`. A ligação ao juiz só examina `Assign`, ignorando `AugAssign`; a escrita continua aceita. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:67`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:81`)
6. `kw={'data_evento':d}; qs.update(**kw)` e `campos=['data_evento']; qs.bulk_update(ps,campos)` escapam do P13; a variável não é rastreada. A primeira forma também escapa da fiscalização V15 dentro da porta. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:93`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:105`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:108`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:97`)
7. Rechaveamento só tem fiscalização de arquivo chamador nesta V15; a procedência do argumento e o valor escrito no corpo de `rechavear_data_evento` não são examinados. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:217`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:232`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:260`)

**10. Segundo juiz — NÃO PROCEDENTE no trajeto geométrico examinado.**

`recusa_batida_retratada` não replica `retratada_em` nem pareia turnos: transforma o `None` de `_dia_do_fato` em exceção. `_dia_do_fato` consulta `dia_do_turno_da_batida`; esta consulta `dia_das_batidas`, que chama `turnos_do_colab`, autoridade registrada para `_T1`. (`app/chamados/services/fio.py:1268`; `app/chamados/services/fio.py:1248`; `app/ponto/turnos.py:2925`; `app/ponto/turnos.py:2902`; `app/core/juizes.py:196`)

O contador também consulta a autoridade do universo, em vez de replicar o filtro. (`app/chamados/services/fio.py:1539`; `app/ponto/turnos.py:2846`)

Limite semântico: a recusa chama “retratada” qualquer episódio cujo juiz resulte `None`, incluindo batida inexistente ou incompatibilidade de colaborador; não verifica literalmente a retratação. `_dia_do_fato` retorna None quando não encontra a FK, e o juiz consulta o mapa do colaborador da linha. (`app/chamados/services/fio.py:1247`; `app/chamados/services/fio.py:1248`; `app/chamados/services/fio.py:1268`; `app/ponto/turnos.py:2925`) Ocorrência desses estados em produção: **[nao verificado]**.

`JUIZES.md` declara-se lookup e aponta para o registro vivo; a classificação acima usa `core/juizes.py`, não apenas o comentário do diff. (`app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:12`)

## 08/11 12:09 · sessao 07c04253531a · - · - · origem=Ronald (tmux)

**Item 5: inalcançável com identidade íntegra; alcançável com acervo de episódio atribuído a outro colaborador.** O código atual permite esse estado pelo `save`, embora `abrir` 15b já não o gere (`app/chamados/models.py:3279`, `:3285`, `:3297`; `app/chamados/services/fio.py:1300`, `:1301`).

Prova no caso íntegro: pedido com episódio vivo produz D; o memo é indexado pelo episódio, então todas as linhas desse episódio recebem D; com mesma base, entram em `datadas`, tornam as sementes não vazias e impedem a condição `not _linhas_do_fato` (`app/chamados/services/fio.py:1243`, `:1248`, `:1249`, `:1463`, `:1465`, `:1466`, `:1481`, `:1482`; `app/chamados/models.py:2802`). Episódio retratado é recusado antes de chegar ao ramo (`app/chamados/services/fio.py:1300`, `:1301`; `app/chamados/models.py:2769`).

Contraexemplo estático:

1. P persistida na disputa de C2, com `colaborador=C2` e `batida_origem=b`, sendo b uma batida viva de C1. O `save` confere colaborador contra a disputa, mas não confere dono da batida explícita; essa conferência existe apenas no derivador, que não é chamado quando o episódio já está preenchido (`app/chamados/models.py:3282`, `:3297`, `:3298`, `:3316`).
2. `abrir` para C1, episódio b, julga b para C1 e obtém D (`app/chamados/models.py:2769`, `:2772`; `app/chamados/services/fio.py:1247`, `:1248`).
3. `do_fato` busca pelo episódio e motivo, sem filtrar colaborador, e encontra P (`app/chamados/models.py:2647`, `:2648`, `:2649`).
4. `universo_do_fato` recusa P pela base C2, não pelo dia; sem outras linhas, devolve universo vazio e P em `recusadas` (`app/chamados/services/fio.py:1463`, `:1464`, `:1481`, `:1483`, `:1486`).
5. O ramo antigo testa somente universo vazio + mesmo episódio nas recusadas e levanta `FatoEmOutroDia`, atribuindo causa de dia a uma recusa de dono (`app/chamados/models.py:2802`, `:2810`, `:2811`).

A existência desse acervo em produção e um chamador de produção atual que persista essa inconsistência: **[nao verificado]**.

## 08/11 12:08 · sessao fc923f307369 · - · - · origem=Ronald (tmux)

**Censo explícito de produção** (apelidos incluídos; chamadas dinâmicas arbitrárias: [nao verificado]):

| Função | Chamadas |
|---|---|
| `resolvedora_valida` | `app/chamados/models.py:392`, `:2781`, `:2829`; `app/chamados/services/fio.py:865`, `:970`; `app/chamados/management/commands/auditar_invariantes_chamados.py:896` (`_rv8b`), `:1048` (`_rv22`). |
| `dia_do_turno_da_linha` | `app/chamados/juizes.py:2008`; `app/chamados/models.py:2769`; `app/chamados/services/fio.py:846`. |
| `recusa_batida_retratada` | `app/chamados/juizes.py:2005`; `app/chamados/models.py:2819`; `app/chamados/services/fio.py:1301`, `:1544`. |

**Mudanças por chamador:**

- Nascimento: episódio retratado recusa no helper antes da validação inicial e da escrita; canônica retratada também recusa antes do reuso, inclusive sem destino. (`app/chamados/models.py:2769`, `:2819`)
- Costuras iniciais/reuso: `dia_do_fato` só entra após `recusa_batida_retratada`; costuras externas usam helper e recebem `(False, motivo textual)`. (`app/chamados/juizes.py:2003-2010`; `app/chamados/models.py:2781`, `:2829`)
- Reapontamento: datar retratada recusa em `fio.py:846-848`; já datada com destino não nulo recusa em `fio.py:865-871`. **Descostura sem datação continua permitida**, pois `resolvedora_valida(None,p)` retorna antes da recusa. (`app/chamados/juizes.py:1999-2000`)
- Rechaveamento: com destino não nulo, retratada passa a recusar em `fio.py:970-972`; sem destino o juiz retorna verdadeiro e a porta pode persistir `novo_dia` fornecido, se demais guardas permitirem. Alcance pelo gatilho de migração depende dos filtros deste. (`app/chamados/services/fio.py:950-974`; `app/chamados/juizes.py:1999-2000`)
- Auditor I8b passa a classificar retratada viva com destino/data como inválida, depois separa os subpisos. Rótulo continua “OUTRO TURNO”. (`app/chamados/management/commands/auditar_invariantes_chamados.py:891-918`, `:935-936`)
- Auditor I22 passa a contar chamado vivo cujas resolvedoras de fato sejam todas recusadas por retratação; conversas ficam excluídas. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1041-1049`)
- Contador chama recusa real para confirmar cada candidata. (`app/chamados/services/fio.py:1542-1546`)

**Fechamento recusado: PROCEDENTE, com as condições do gate.**

1. `_ancoradas` busca não validadas pelo destino ou pelo chamado da disputa. (`app/chamados/models.py:344-347`)
2. `_vivas_entre` exige continuar ancorada, competência aberta (`is False`) e C6 viva no pós-estado do chamado. Retratação do episódio não é filtro aqui. (`app/chamados/models.py:349-369`)
3. `_plano_pergunta_viva` procura sobrevivente usando **data gravada**, não helper. Havendo sobrevivente, chama resolvedora. (`app/chamados/models.py:390-392`)
4. Resolvedora → helper → recusa retratada → `(False, 'batida_retratada: ...')`. (`app/chamados/juizes.py:2008-2010`; `app/chamados/services/fio.py:1301`, `:1268-1272`)
5. Plano devolve `reatas=None`; gate devolve `False`. O log **perde o motivo específico**: registra `pergunta_viva_na_competencia_aberta` e descreve inexistência de cobrança receptora mesmo quando ela existe, mas rejeitou a pergunta. (`app/chamados/models.py:392-393`, `:422-431`)
6. Reconciliar aborta decisão; cancelar/encerrar/encerrar por competência abortam antes da escrita de estado. (`app/chamados/models.py:685-689`, `:786-788`, `:1604-1606`, `:1624-1626`)

Logo, não é “qualquer pergunta retratada bloqueia qualquer fechamento”: a pergunta deve sobreviver ao filtro C6 e competência no pós-estado; sem viva, gate permite. (`app/chamados/models.py:364-369`, `:420-421`)

**Escape de produção confirmado: conversa sobre retratada.**

Contraexemplo estático: disputa com `contexto['ultima_batida']['id']=br` retratada, sem `contexto['gps_ausente']['data']`, emissão de `gps_ausente`, sem conversa anterior desse motivo. O motivo é BATIDA e sua pergunta é `escolha`. (`app/chamados/catalogo/motivos.py:42`; `app/chamados/catalogo/perguntas.py:88-91`)

O derivador de origem aceita `br` por colaborador, sem filtrar `retratada_em`, portanto o dedup inicial de motivo sem episódio não barra. (`app/chamados/models.py:3310-3316`; `app/chamados/services/disputa_emissao.py:325-328`) O juiz de `ultima_batida` responde `None`; `_dia_p` fica nulo; apenas tipo `hora` é descartado. (`app/chamados/services/disputa_emissao.py:387-392`, `:424-430`) Cai em `abrir_conversa`, caminho cujo próprio comentário menciona episódio retratado. (`app/chamados/services/disputa_emissao.py:431-441`) Conversa não deriva origem no nascimento nem no save; helper retorna dia civil antes da recusa. (`app/chamados/models.py:2755-2762`, `:3297-3298`; `app/chamados/services/fio.py:1297-1301`) Se destino existir, resolvedora também aprova conversa antes da recusa. (`app/chamados/juizes.py:1997-1998`) O contador exclui conversas. (`app/chamados/services/fio.py:1536`)

**Portanto PROCEDENTE o bug no chamador indicado pelo aval; ocorrência desse estado em produção: [nao verificado].** (`app/chamados/services/disputa_emissao.py:431-441`)

**Outro caminho de voltar a pedir resposta, se “reatar” abranger reabertura:** pergunta respondida sobre episódio retratado, sem `materializada_em`, pode voltar a pendente por `reabrir_por_colab`: `perguntas_corrigiveis` exige respondida e sem lastro/viva; sem carimbo satisfaz independentemente do episódio; a porta chama `reabrir_para_correcao`, que zera resposta/validação por update sem consultar a recusa nova. (`app/chamados/juizes.py:1412-1414`, `:1245`; `app/chamados/models.py:2226-2235`; `app/chamados/services/disputa_emissao.py:1494`, `:1530-1534`) Ocorrência em produção: [nao verificado].

## 08/11 12:08 · sessao 07c04253531a · - · - · origem=Ronald (tmux)

Achados dos campos e consequências:

| Campo/valor | Escritores | Leitores relevantes |
|---|---|---|
| `data_evento` | Nascimento usa `_dia_fato` (`app/chamados/models.py:2867`); datação usa `_dia` e salva (`app/chamados/services/fio.py:846`, `:849`, `:879`); rechaveamento usa `novo_dia` (`app/chamados/services/fio.py:968`, `:974`). Apply da migração obtém juiz (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`, `:154`, `:358`); reversão usa arquivo (`:412`, `:423`). | Chave do `do_fato` e busca (`app/chamados/models.py:2647`); dia próprio de marco (`app/chamados/services/fio.py:1250`); C6 lê gravada (`app/chamados/juizes.py:336`), tranca calcula competência por ela (`:948`, `:961`); plantio combina dia gravado com hora (`app/chamados/services/materializacao.py:344`, `:345`, `:414`, `:421`). |
| `celula` | Nascimento/reuso consulta dia `_dia_fato`, atribui e salva (`app/chamados/models.py:2789`, `:2823`, `:2863`, `:2868`); rechaveamento procura `novo_dia`, atribui FK e salva (`app/chamados/services/fio.py:965`, `:969`, `:974`). | C6 usa FK somente se dia da célula e `data_evento` coincidirem (`app/chamados/juizes.py:634`, `:638`, `:642`); a costura terminal chama C6 no pós-estado (`app/chamados/services/fio.py:779`, `:781`). |
| `chamado_destino` | Nascimento (`app/chamados/models.py:2868`); reuso após E4-c e guarda terminal (`:2829`, `:2837`, `:2839`, `:2863`); reapontamento (`app/chamados/services/fio.py:865`, `:867`, `:877`, `:879`). Atribuições `models.py:367` e `fio.py:779` são projeções em memória, a última desfeita em `:783`. | Âncora da listagem (`app/chamados/services/fio.py:117`); lookup da disputa (`:135`); status terminal (`:770`); destino do plantio (`app/chamados/services/materializacao.py:339`); E4-c compara o dia do chamado (`app/chamados/juizes.py:2018`, `:2027`). |
| `batida_origem_id` | Explícita ou derivada antes de pedir dia (`app/chamados/models.py:2760`, `:2762`), persistida no create via `campos` (`:2868`); derivação de inserção no save quando ausente (`:3269`, `:3297`, `:3298`). Derivador usa família BATIDA, contexto da disputa e confere colaborador, sem conferir retratação (`:3310`, `:3312`, `:3316`). | Busca de episódio (`app/chamados/models.py:2647`); juiz (`app/chamados/services/fio.py:1242`, `:1247`, `:1248`); recusa (`:1266`, `:1268`); chave da família (`:1131`); contagem de episódios no plano (`:1060`). |
| `dia_da_chave` | Helper passa `d`, que é pedido se disponível, senão gravada (`app/chamados/services/fio.py:1297`, `:1309`). | Só altera pergunta de vizinhança das sementes (`:1358`, `:1361`); vizinhos posteriores usam suas chaves (`:1367`); cache inclui override (`:1336`, `:1337`); `do_fato` recebe override (`:1204`). |
| `dia_do_fato` parâmetro | Somente `_nascer` passa `_dia_fato` ao proxy e à canônica (`app/chamados/models.py:2781`, `:2829`), valor obtido do helper (`:2769`). | `resolvedora_valida` chama recusa e usa parâmetro diretamente (`app/chamados/juizes.py:2004`, `:2005`, `:2006`); sem parâmetro, consulta helper (`:2008`). |

**Campo com significados divergentes — PROCEDENTE no acervo reusado.** A porta pode mudar célula/destino para DIA e conservar `data_evento=D0`; o teste faz essa preservação explicitamente (`app/chamados/models.py:2823`, `:2839`, `:2863`; `app/chamados/tests/test_e1_v15.py:98`, `:99`). E4-c passa pelo dia do fato enquanto C6/tranca/plantio continuam lendo gravada; C6 rejeita alimentação da célula divergente e tranca calcula competência de D0 (`app/chamados/juizes.py:2006`, `:336`, `:638`, `:948`, `:961`; `app/chamados/services/materializacao.py:345`, `:414`). Migração integral de produção e execução concreta de plantio nesse estado: **[nao verificado]**.

A própria função de identidade declara dois papéis para `data_evento`: chave gravada de localização e identidade calculada pelo juiz (`app/chamados/services/fio.py:1231`, `:1232`, `:1233`). Conversa também usa campo como dia civil, com discriminador explícito (`app/chamados/models.py:2717`, `:2718`, `:2722`; `app/chamados/services/fio.py:1298`, `:1299`).

**Consequências de R retratada admitida pelo fecho:** a eleição não chama recusa e considera resposta/menor id (`app/chamados/services/fio.py:1004`, `:1005`, `:1006`). `plano_de_retratacao` conta R como episódio e pode declarar `episodios_unidos` entre R e A vivo, bloqueando retratação (`:1060`, `:1065`, `:1066`).

- Dedup DRY elege essa canônica e aplica plano (`app/chamados/management/commands/dedup_perguntas_fato.py:76`, `:81`); apply delega ao backfill (`:104`, `:105`).
- Backfill elege a mesma canônica e chama o plano, sem recusa da canônica (`app/chamados/management/commands/backfill_decisoes_pergunta.py:265`, `:279`, `:284`, `:320`).
- Migração de resolvedoras elege canônica do universo; se R é a própria pergunta no chamado antigo, tenta reapontar e recebe recusa pela E4-c (`app/chamados/services/fio.py:1753`, `:1763`, `:1772`, `:1773`, `:865`). Se R é canônica externa, a eleição por si não passa pela recusa (`:1753`, `:1763`, `:1778`).
- Migração de data inclui R nos membros do grupo e lê seu dia gravado no veto de competência (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:254`, `:277`, `:279`, `:280`).

**Travas/relógio/escritor extra:** `_nascer` trava colaborador antes da nova resolução e consultas (`app/chamados/models.py:2735`, `:2739`, `:2769`). Reapontamento resolve `_dia` antes do atomic interno, mas toma trava de colaborador antes da guarda de Chamado e do save (`app/chamados/services/fio.py:846`, `:852`, `:858`, `:864`, `:867`, `:879`); isso é leitura anterior à trava, sem nova aquisição invertida nesse trecho. Rechaveamento toma Colaborador/Disputa/Pergunta por porta (`:956`, `:957`) e resolvedora não adquire trava (`app/chamados/juizes.py:2003`, `:2008`). O novo diff não acrescenta leitura de relógio; conversa mantém `localdate()` na borda (`app/chamados/models.py:2722`). Auditoria transitiva universal de travas/relógios e formas dinâmicas de escrita: **[nao verificado]**.

## 08/11 11:32 · sessao ab592c50e326 · FIO-E0-V4-E1 · verificador · origem=Code

**REPROVADO no alcance universal do aval.** A correção atende aos casos originais com episódio cujo juiz responde um dia, mas o fallback ainda permite gravar o pedido e reproduzir 3a/3b. A catraca verifica a chamada ao helper, sem garantir essa semântica. (`app/chamados/services/fio.py:1258`, `app/chamados/services/fio.py:1262`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:59`)

1. **CENSO — NÃO PROCEDENTE a afirmação de que toda escrita grava exclusivamente o dia do juiz.**

   Os sítios de atribuição e persistência encontrados são:

   | Sítio | Valor e classificação |
   |---|---|
   | `PerguntaDisputa._nascer`: `app/chamados/models.py:2840` | `create(data_evento=_dia_fato)`. Obtém `_dia_fato` pelo helper em `app/chamados/models.py:2749`; o helper pode devolver o pedido, conforme `app/chamados/services/fio.py:1258`–`1262`. |
   | `reapontar_resolvedora`: `app/chamados/services/fio.py:843` | Atribui `_dia`, calculado pelo mesmo helper em `app/chamados/services/fio.py:842`. Acrescenta o campo à lista em `app/chamados/services/fio.py:844` e persiste em `app/chamados/services/fio.py:873`. Mesma ressalva de fallback. |
   | `rechavear_data_evento`: `app/chamados/services/fio.py:962` | Atribui o parâmetro `novo_dia` e persiste em `app/chamados/services/fio.py:968`; não consulta o juiz nessa porta. |
   | Chamador de rechaveamento, apply | O valor vem de `dia_do_turno_da_batida`, passa por `ln['novo']` e chega à porta. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:154`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:358`) |
   | Chamador de rechaveamento, reversão | Grava a data recuperada do arquivo de reversão, sem recalculá-la pelo juiz. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:412`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:423`) |

   Há ainda três atribuições **sem persistência própria**: o proxy da validação inicial e os dois desfazimentos em memória antes de retornar por recusa. (`app/chamados/models.py:2756`; `app/chamados/services/fio.py:855`, `app/chamados/services/fio.py:864`)

   Não encontrei escritor adicional de `data_evento` por `update`, bulk, `setattr` ou construtor com apelido. O bulk existente de Pergunta grava somente os três campos da lavra; o `save` do modelo delega a persistência sem normalizar `data_evento`. (`app/chamados/veredito_lavrado.py:194`, `app/chamados/veredito_lavrado.py:207`; `app/chamados/models.py:3242`–`3245`) Exaustividade contra formas dinâmicas arbitrárias: **[nao verificado]**.

   **Chamado incluído:** nesta cópia, a classe é `ChamadoColaborador` e **não possui campo `data_evento`**; possui `contexto_json`, `celula` e timestamps. Suas portas de INSERT são `abrir`, com `get_or_create`, e `criar`, com `create`. (`app/chamados/models.py:38`–`101`, `app/chamados/models.py:971`, `app/chamados/models.py:1095`)

   Essas duas portas preservam o contexto recebido e resolvem a célula por `_celula_de`, que consulta `juizes.dia_do_fato(fortes=True)`. Não são escritoras literais de `data_evento`, nem normalizam o contexto pelo novo helper da Pergunta. (`app/chamados/models.py:939`–`940`, `app/chamados/models.py:958`, `app/chamados/models.py:1067`, `app/chamados/models.py:1234`–`1240`)

2. **CATRACA — NÃO PROCEDENTE como garantia completa; PROCEDENTE a mordida dos cinco mutantes oficiais.**

   O P13 identifica os escritores explícitos atuais. Reconhece apelidos de import e atribuições simples de alias, `update(data_evento=...)`, `setattr` com nome literal e bulk com campo literal. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:37`–`54`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:84`–`108`)

   A V15 fiscaliza valores somente em `_nascer` e `reapontar_resolvedora`. Para rechaveamento, verifica o **arquivo chamador**, não a procedência do valor. Isoladamente, também não reprova escritores novos encontrados pelo censo: apenas exige `assertTrue(censo())`; a rejeição de sítios externos pertence a outro teste, no P13. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:24`–`30`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:118`–`134`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:143`–`146`)

   Escapes da fiscalização:

   - **Dentro de porta permitida:** trocar `p.data_evento = _dia` por `setattr(p, 'data_evento', data_evento)` escapa da V15. Ela examina `ast.Assign` e keywords diretos; o P13 aceita a localização. Bulk com campos posicionais também não recebe fiscalização de valor pela V15. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:74`–`88`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:117`–`119`)
   - **Fluxo por variável:** `kw={'data_evento': d}; qs.update(**kw)` e `campos=['data_evento']; qs.bulk_update(ps, campos)` escapam do P13, que procura constantes nos argumentos da própria chamada. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:93`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:105`–`108`)
   - **Outra classe:** construtor direto só é reconhecido quando pertence aos aliases de `PerguntaDisputa`; chamadas ORM com keyword literal são reconhecidas independentemente da classe. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:29`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:94`–`97`)
   - **Semântica do helper:** chamar uma função chamada `dia_do_turno_da_linha` basta para ligar o valor ao “juiz”; seu fallback não é examinado. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:45`–`66`)

   Os cinco mutantes oficiais detectam regressões específicas das atribuições e da costura. Isso não cobre os escapes acima. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:138`–`154`) Execução da suíte Django/BD: **[nao verificado]**.

3. **FALLBACK — PROCEDENTE: reabre o defeito em caminho de produção alcançável.**

   Sem episódio, o helper devolve pedido/gravado diretamente; com episódio e resposta `None`, faz o mesmo. Já o universo consulta A2 para decidir o dia dessas linhas. Batida retratada pode produzir `None`, e `_nascer` aceita episódio explícito sem verificar retratação. (`app/chamados/services/fio.py:1258`–`1262`, `app/chamados/services/fio.py:1401`–`1408`; `app/ponto/turnos.py:2917`–`2926`; `app/chamados/models.py:2745`–`2747`)

   **Contraexemplo construído de 3a**, com `D0=10/09` e `DIA=18/10`: K é acervo sem episódio gravado em D0; A tem episódio vivo `a`, também gravado em D0, mas o juiz responde DIA; `b` é episódio retratado ainda sem Pergunta. O vínculo entre acervo e episódios pela data gravada é exatamente o de `do_fato`. (`app/chamados/models.py:2627`–`2635`; datas da base: `app/chamados/tests/test_e1_v13.py:40`, `app/chamados/tests/test_e1_v13.py:60`–`63`)

   - Primeira `abrir(b, D0)`: helper responde D0. O universo encontra K; A2(K) alcança A, cujo dia é DIA, e recusa K. Não existe recusada do episódio `b`; nasce L com episódio `b` e data D0. (`app/chamados/services/fio.py:1258`–`1262`, `app/chamados/services/fio.py:1300`–`1308`, `app/chamados/services/fio.py:1406`; `app/chamados/models.py:2781`, `app/chamados/models.py:2840`)
   - Repetição idêntica: encontra K e L. A2(L) passa por K e alcança A=DIA; ambas são recusadas para o pedido D0. Agora há recusada do episódio `b`, portanto levanta **`FatoEmOutroDia`**. (`app/chamados/services/fio.py:1300`–`1308`, `app/chamados/services/fio.py:1406`–`1409`; `app/chamados/models.py:2781`–`2792`)

   Assim, a primeira chamada cria e a segunda idêntica recusa. Ocorrência desse estado na frota: **[nao verificado]**.

   **Conversa é diferente:** tem contrato explícito de nascer sempre, gravar dia civil e dispensar comparação de turno. Sua falta de dedup não constitui esse 3a. (`app/chamados/models.py:2700`–`2707`, `app/chamados/models.py:2765`; `app/chamados/juizes.py:1991`–`1992`)

4. **MEMO — NÃO PROCEDENTE a hipótese de colisão entre os usos indicados.**

   | Uso | Chave | Escritas |
   |---|---|---|
   | Dia da batida | `('dia_do_turno', batida_id)` | `app/chamados/services/fio.py:1233`, `app/chamados/services/fio.py:1238` |
   | Colaborador | `('colab', colaborador_id)` | `app/chamados/services/fio.py:1268`, `app/chamados/services/fio.py:1271`, `app/chamados/services/fio.py:1274` |
   | Vizinhos | `(pk, chave_do_dia, batida_id)` | `app/chamados/services/fio.py:1280`, `app/chamados/services/fio.py:1287`, `app/chamados/services/fio.py:1390` |

   As duas primeiras famílias têm duas posições e marcadores distintos; a terceira tem três posições. Os acessos usam essas chaves completas. A resolvedora apenas repassa o dict ao helper: não introduz outra escrita nele. (`app/chamados/services/fio.py:1234`–`1239`, `app/chamados/services/fio.py:1269`–`1288`; `app/chamados/juizes.py:1994`)

5. **3a/3b ORIGINAIS — PROCEDENTE a cura com juiz não nulo; NÃO PROCEDENTE a garantia universal.**

   **3a, episódio vivo:** pedidos D0 e DIA viram o mesmo `_dia_fato`; busca, célula e criação usam esse valor. A repetição encontra a canônica e retorna sem novo nascimento. Esse é também o comportamento exigido pelo teste novo. (`app/chamados/models.py:2749`–`2751`, `app/chamados/models.py:2768`, `app/chamados/models.py:2779`, `app/chamados/models.py:2837`, `app/chamados/models.py:2840`; `app/chamados/tests/test_e1_v15.py:52`–`66`)

   **3b, episódio vivo:** linha gravada em D0, juiz=DIA, destino=DIA passa a ser aceita, porque a resolvedora lê DIA pelo helper. O reuso preserva D0 no banco. (`app/chamados/juizes.py:1994`, `app/chamados/juizes.py:2013`–`2014`; `app/chamados/services/fio.py:1261`–`1262`; `app/chamados/tests/test_e1_v15.py:90`–`99`)

   **Ainda pode levantar `ResolvedoraCruzada`:** tome K sem episódio, gravada em D0 e canônica por resposta/menor pk, mais A do episódio vivo `a`, gravada em D0 e juiz=DIA. `abrir(a, DIA, destino=DIA)` passa no proxy, semeia A, alcança K por A2={DIA} e elege K. Na segunda validação, o helper de K devolve D0; o gap de 38 dias produz recusa e exceção. (`app/chamados/models.py:2756`–`2762`, `app/chamados/models.py:2802`–`2804`; `app/chamados/services/fio.py:998`–`1000`, `app/chamados/services/fio.py:1259`–`1260`, `app/chamados/services/fio.py:1361`–`1368`; `app/chamados/juizes.py:2022`–`2029`)

   **Variante com episódio na própria linha recusada:** acrescente R respondida, de episódio retratado, gravada em D0 e ligada por K a A. O fecho admite R por A2 e a canônica passa a ser R; seu juiz responde `None`, o helper devolve D0 e ocorre a mesma `ResolvedoraCruzada`. Portanto a resposta à pergunta sobre linha **com episódio** ainda é sim, quando ele está retratado. (`app/chamados/services/fio.py:1238`, `app/chamados/services/fio.py:1262`, `app/chamados/services/fio.py:1363`–`1368`, `app/chamados/services/fio.py:999`; `app/chamados/models.py:2804`) Ocorrência em produção: **[nao verificado]**.

6. **ESCRITORES/LEITORES — PROCEDENTE a divergência residual de significado.**

   | Campo | Quem escreve nos caminhos examinados | Quem lê |
   |---|---|---|
   | `data_evento` | Nascimento, datação e rechaveamento censados acima. (`app/chamados/models.py:2840`; `app/chamados/services/fio.py:843`, `app/chamados/services/fio.py:962`) | Costura pelo helper; fecho pelo juiz/A2; busca pela chave gravada; C6/tranca/plantio pela gravada. (`app/chamados/juizes.py:1994`; `app/chamados/services/fio.py:1196`, `app/chamados/services/fio.py:1232`; `app/chamados/juizes.py:336`, `app/chamados/juizes.py:948`; `app/chamados/services/materializacao.py:345`) |
   | `celula` | Nascimento/reuso pela célula de `_dia_fato`; rechaveamento pela célula de `novo_dia`. (`app/chamados/models.py:2768`, `app/chamados/models.py:2798`, `app/chamados/models.py:2841`; `app/chamados/services/fio.py:959`–`963`) | C6 lê a célula; alimentação da falta exige igualdade com `data_evento`. (`app/chamados/juizes.py:634`–`642`, `app/chamados/juizes.py:1199`–`1214`) |
   | `batida_origem_id` | Explícita ou derivada no nascimento; o `save` usa o mesmo derivador quando ausente. (`app/chamados/models.py:2745`–`2747`, `app/chamados/models.py:3270`–`3289`) | Busca do episódio, juiz do turno e seleção do fallback. (`app/chamados/models.py:2632`; `app/chamados/services/fio.py:1232`–`1239`, `app/chamados/services/fio.py:1259`) |
   | `chamado_destino` | Nascimento, reuso após guardas e reapontamento. (`app/chamados/models.py:2841`, `app/chamados/models.py:2812`; `app/chamados/services/fio.py:871`–`874`) | Validação de turno, destino da listagem, guarda de terminal e marcos do plantio. (`app/chamados/juizes.py:2004`; `app/chamados/services/fio.py:117`, `app/chamados/services/fio.py:778`–`783`; `app/chamados/services/materializacao.py:339`, `app/chamados/services/materializacao.py:409`) |

   O estado residual é explícito: reuso pode associar célula/destino de DIA e manter `data_evento=D0`. A costura julga DIA, enquanto tranca calcula competência por D0; C6 descarta a célula de DIA como alimentação para D0. (`app/chamados/models.py:2797`–`2813`; `app/chamados/tests/test_e1_v15.py:99`; `app/chamados/juizes.py:1994`, `app/chamados/juizes.py:948`–`961`, `app/chamados/juizes.py:638`)

   O derivador de plantio, quando alcançado, continua combinando a hora com a data gravada e ajustando overnight a partir dela. Isso identifica a divergência de leitura; plantio efetivamente executado nesses estados: **[nao verificado]**. (`app/chamados/services/materializacao.py:344`–`345`, `app/chamados/services/materializacao.py:414`–`421`)

   Os leitores permanecerem intactos atende ao recorte literal do aval. Isso, porém, não comprova a premissa “gravada == juiz”: o próprio teste conserva a divergência, e o fallback pode criá-la. Migração integral do acervo em produção: **[nao verificado]**. (`app/chamados/tests/test_e1_v15.py:98`–`99`; `app/chamados/services/fio.py:1262`)

7. **CLASSES DE REPROVA — presentes as seguintes:**

   - **Derivação alternativa à fonte única: PROCEDENTE.** O juiz documenta que `None` significa “sem resposta [...] não há dia a gravar”; o helper substitui essa ausência pelo pedido/gravado e não consulta A2. (`app/ponto/turnos.py:2919`–`2920`; `app/chamados/services/fio.py:1258`–`1262`)
   - **Selo insuficiente: PROCEDENTE quanto à abrangência.** Os mutantes oficiais mordem, mas `setattr`/bulk dentro da porta e a semântica interna do helper permanecem descobertos. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:59`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:74`–`88`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:138`–`154`)
   - **Campo com significados divergentes: PROCEDENTE no acervo/fallback.** Identidade pelo juiz/A2 e chave gravada continuam podendo divergir, com consumidores decisórios lendo a gravada. (`app/chamados/services/fio.py:1222`–`1231`; `app/chamados/tests/test_e1_v15.py:99`; `app/chamados/juizes.py:336`)
   - **Segundo juiz geométrico duplicando o registrado: NÃO PROCEDENTE neste trajeto.** O helper chega a `_T1`, cuja implementação consulta `turnos_do_colab`, autoridade registrada. (`app/core/juizes.py:196`; `app/chamados/services/fio.py:1236`–`1238`; `app/ponto/turnos.py:2902`–`2903`, `app/ponto/turnos.py:2925`)
   - **Segundo escritor explícito fora da porta: NÃO PROCEDENTE no censo encontrado.** Os escritores concretos estão nos três sítios listados; rechaveamento é uma porta declarada, embora aceite valor fornecido. (`app/chamados/models.py:2840`; `app/chamados/services/fio.py:843`, `app/chamados/services/fio.py:962`)
   - **Relógio novo fora da borda: NÃO PROCEDENTE nas alterações examinadas.** O relógio identificado está na porta explícita de conversa. Auditoria transitiva integral de relógios: **[nao verificado]**. (`app/chamados/models.py:2706`–`2707`)
   - **Inversão nova de trava: NÃO PROCEDENTE nos caminhos examinados.** `_nascer` trava Colaborador antes das consultas; reapontamento adquire essa trava por `fato_livre_para` ou diretamente antes da guarda de Chamado. Todos os chamadores/transações externos: **[nao verificado]**. (`app/chamados/models.py:2724`, `app/chamados/models.py:2677`; `app/chamados/services/fio.py:852`–`861`, `app/chamados/services/fio.py:768`)

## 08/11 11:29 · sessao d1aba136826f · - · - · origem=Ronald (tmux)

Checagem adicional concluída: nenhum escritor real extra encontrado fora das portas já listadas. A busca incluiu todos os `.py` da árvore, aliases de import/atribuição e referências fora de `app/`. [nao verificado: artefato persistido do censo]

Os candidatos dinâmicos existentes não escrevem `data_evento`:

- `descarimbar_em_lote(**outros)` recebe campos de resposta/validação (`app/chamados/models.py:1555`; `app/chamados/services/disputa_emissao.py:1494`, `:1531`).
- Atualização dinâmica do Chamado usa somente `urgencia` e `urgencia_original` (`app/chamados/models.py:473`, `:631`, `:636`).
- Bulk de Pergunta grava somente os três campos da lavra (`app/chamados/veredito_lavrado.py:194`, `:207`).
- `CC = apps.get_model(...)` em `bin/r5_idempotencia_frota.py:30` somente consulta chamados em `bin/r5_idempotencia_frota.py:60`.

Os únicos INSERTs diretos de Chamado fora dos gates encontrados são testes em arquivos soltos: `app/tenants/tests.py:62` (`FastTenantTestCase`, `:5`) e `app/relatorios/tests_pdf_chamados.py:34` (`TestCase`, `:18`).

Não encontrei construtor aliased de Pergunta seguido de save em produção, nem bypass real de INSERT de Chamado; permanecem `abrir` e `criar` (`app/chamados/models.py:971`, `:1095`). [nao verificado: prova negativa exaustiva contra formas dinâmicas arbitrárias]

## 08/11 11:29 · sessao cc28a568237e · - · - · origem=Ronald (tmux)

**3. PROCEDENTE: o fallback mantém caminhos que reproduzem 3a/3b.** Sem episódio, devolve diretamente pedido/data gravada; com episódio cujo juiz responde `None`, também devolve esse valor. A ponte A2 usada pelo universo não é consultada pelo helper (`app/chamados/services/fio.py:1258`, `app/chamados/services/fio.py:1262`, `app/chamados/services/fio.py:1406`). Batida retratada pode produzir `None`; a porta aceita `batida_origem_id` explícita sem conferir retratação (`app/ponto/turnos.py:2917`, `app/ponto/turnos.py:2925`, `app/chamados/models.py:2745`).

Contraexemplo concreto de **3a**, por trace:

- D0=10/09; DIA=18/10. Estado inicial: K sem episódio gravada em D0; A com episódio vivo `a`, gravada em D0, juiz=DIA; episódio `b` retratado sem pergunta. Esses estados são representáveis pelo modelo; `do_fato` conecta acervo do mesmo dia aos episódios (`app/chamados/models.py:2630`, `app/chamados/models.py:2632`).
- Primeiro `abrir(b,D0)`: helper=D0; universo encontra K; A2(K) alcança A=DIA, logo recusa K. Não há recusada de episódio `b`, portanto nasce L(b,D0) (`app/chamados/services/fio.py:1300`, `app/chamados/services/fio.py:1304`, `app/chamados/services/fio.py:1406`, `app/chamados/models.py:2781`, `app/chamados/models.py:2840`).
- Repetição idêntica: universo encontra K e L; A2(L) passa por K e alcança A=DIA. Ambas ficam recusadas; agora existe recusada de episódio `b`, disparando `FatoEmOutroDia` (`app/chamados/services/fio.py:1300`, `app/chamados/services/fio.py:1406`, `app/chamados/models.py:2781`, `app/chamados/models.py:2789`).

Contraexemplo concreto de **3b**:

- K sem episódio, menor pk ou respondida, gravada em D0; A de episódio vivo, gravada em D0, juiz=DIA. `abrir(a,DIA,destino=DIA)` semeia A, alcança K pela ponte A2={DIA} e elege K (`app/chamados/services/fio.py:1401`, `app/chamados/services/fio.py:1421`, `app/chamados/services/fio.py:1358`, `app/chamados/services/fio.py:1363`, `app/chamados/services/fio.py:998`).
- A validação inicial do proxy passa usando DIA. No reuso, K sem episódio devolve D0; o gap de 38 dias termina em recusa e `ResolvedoraCruzada` (`app/chamados/models.py:2756`, `app/chamados/models.py:2760`, `app/chamados/models.py:2802`, `app/chamados/models.py:2804`, `app/chamados/services/fio.py:1259`, `app/chamados/juizes.py:2022`, `app/chamados/juizes.py:2029`).
- Variante com episódio: acrescente R respondida, de episódio retratado, gravada em D0, ligada por K ao mesmo A. O fecho admite R por A2 e a elege; o helper recebe `None` do juiz e volta a D0, produzindo a mesma rejeição. Portanto ainda pode rejeitar uma linha **com episódio**, quando ele está retratado (`app/chamados/services/fio.py:1238`, `app/chamados/services/fio.py:1262`, `app/chamados/services/fio.py:1363`, `app/chamados/services/fio.py:999`, `app/chamados/models.py:2804`).

A conversa tem tratamento explícito: nasce sempre, grava dia civil e não passa pela comparação de turno. Isso não reproduz o defeito contratual de dedup de pergunta datada (`app/chamados/models.py:2700`, `app/chamados/models.py:2707`, `app/chamados/models.py:2765`, `app/chamados/juizes.py:1991`).

**4. NÃO PROCEDENTE: não há colisão entre as chaves dos usos descritos.** Dia usa tupla de dois elementos `('dia_do_turno', id)` e escreve em 1238; colaborador usa `('colab', id)` e escreve em 1271/1274; vizinhança usa tupla de três elementos `(pk, chave_do_dia, batida_id)`, escrita em 1287 e 1390. Tamanho ou marcador distinguem esses espaços (`app/chamados/services/fio.py:1233`, `app/chamados/services/fio.py:1238`, `app/chamados/services/fio.py:1268`, `app/chamados/services/fio.py:1271`, `app/chamados/services/fio.py:1274`, `app/chamados/services/fio.py:1280`, `app/chamados/services/fio.py:1287`, `app/chamados/services/fio.py:1390`). A resolvedora apenas repassa o memo ao helper (`app/chamados/juizes.py:1994`).

**5. Cura dos casos originais com episódio vivo: PROCEDENTE; garantia universal: NÃO PROCEDENTE.** Com resposta não nula do juiz, pedidos D0/DIA viram o mesmo `_dia_fato`, usado na busca, célula e criação; a repetição retorna a canônica existente. A resolvedora lê esse mesmo juiz, independentemente da data gravada (`app/chamados/models.py:2749`, `app/chamados/models.py:2768`, `app/chamados/models.py:2779`, `app/chamados/models.py:2837`, `app/chamados/models.py:2840`, `app/chamados/services/fio.py:1261`, `app/chamados/juizes.py:1994`, `app/chamados/juizes.py:2013`). Os contraexemplos acima impedem estender a conclusão aos caminhos de fallback (`app/chamados/services/fio.py:1262`).

Executei uma reprodução somente em memória dos corpos AST reais de helper/A2/fecho/universo/canônica, adaptando o ORM para listas e pré-carregando respostas do juiz: primeira chamada do cenário 3a teve universo vazio/recusada K; repetição teve universo vazio/recusadas K,L; os dois cenários 3b elegeram a linha cujo helper retornou D0. Execução Django/BD e ocorrência em produção: **[nao verificado]**. [nao verificado]

## 08/11 11:28 · sessao d1aba136826f · - · - · origem=Ronald (tmux)

Censo literal de `data_evento`:

| Sítio persistente | Valor gravado |
|---|---|
| `app/chamados/models.py:2840` (`PerguntaDisputa._nascer`) | `_dia_fato`, obtido por `dia_do_turno_da_linha(..., pedido=data_evento)` em `app/chamados/models.py:2749`. |
| `app/chamados/services/fio.py:843`, persistido em `app/chamados/services/fio.py:873` | `_dia`, obtido pelo helper em `app/chamados/services/fio.py:842`; campo acrescentado à lista em `app/chamados/services/fio.py:844`. |
| `app/chamados/services/fio.py:962`, persistido em `app/chamados/services/fio.py:968` | Parâmetro `novo_dia`, sem consulta local ao juiz; apply passa o resultado de `dia_do_turno_da_batida` (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`, `:154`, `:358`); reversão passa a data lida do arquivo (`:412`, `:423`). |

`_px.data_evento = _dia_fato` é proxy em memória (`app/chamados/models.py:2756`); `p.data_evento = None` desfaz em memória duas recusas que retornam antes do save (`app/chamados/services/fio.py:855`, `:864`). Nenhum escritor explícito adicional em produção foi encontrado; exaustividade além das formas pesquisadas permanece [nao verificado].

**Chamado incluído:** `ChamadoColaborador` não declara campo `data_evento`; declara `contexto_json`, `celula` e timestamps (`app/chamados/models.py:38`–`:101`). Suas duas portas concretas de INSERT são `abrir` (`app/chamados/models.py:971`) e `criar` (`app/chamados/models.py:1095`). Elas gravam contexto recebido por `defaults`/`campos` (`app/chamados/models.py:939`, `:1095`) e resolvem célula em `app/chamados/models.py:958`/`:1067`; `_celula_de` pergunta a `chamados.juizes.dia_do_fato(fortes=True)` (`app/chamados/models.py:1234`), não ao helper de data da Pergunta. Portanto não se deve classificá-las como escritores de um campo inexistente.

**Catraca: PROCEDENTE para os escritores explícitos atuais; NÃO PROCEDENTE como garantia geral.** P13 identifica esses sete registros sintáticos: proxy 2756, create 2840, atribuições fio 843/855/864/962 e save explícito fio 968; seus detectores estão em `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:84`–`:108`. V15 verifica os valores de duas portas (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:24`); rechaveamento recebe somente fiscalização dos arquivos chamadores (`:97`, `:133`).

- Apelidos de import e aliases simples de `PerguntaDisputa` são cobertos pelo P13 (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:37`).
- `update(data_evento=...)`, bulk com campo literal e `setattr(...,'data_evento',...)` são detectados fora da porta (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:94`, `:98`, `:107`).
- Fluxo por variável escapa: `campos={'data_evento':d}; qs.update(**campos)` e `campos=['data_evento']; qs.bulk_update(ps,campos)`. Os detectores só procuram constantes nos argumentos locais da chamada (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:93`, `:105`, `:107`).
- Construtor de outra classe não entra na lista de aliases de Pergunta (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:29`, `:96`).
- Dentro de porta permitida, V15 ignora `setattr`, atribuição anotada/aumentada e bulk por campo posicional: inspeciona somente `ast.Assign` e keywords diretos (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:74`–`:88`); P13 aceita a localização (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:117`). Contra-mutante concreto, validado somente em memória: substituir fio:843 por `setattr(p, 'data_evento', data_evento)` mantém `violacoes_do_escritor=[]`; nenhum arquivo foi alterado [nao verificado: registro persistido da execução].
- Os cinco mutantes oficiais mordem nos analisadores AST (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:138`–`:154`); extraí e executei as funções puras via `python3 -B`, sem importar Django nem acessar banco. Resultado da execução sem artefato persistido: [nao verificado]. A limitação do mutante alternativo decorre diretamente de `:74`–`:88`.

## 08/11 11:28 · sessao d5d52281b203 · - · - · origem=Ronald (tmux)

**Item 6 — PROCEDENTE: os leitores continuam atribuindo significado de turno ao valor gravado, embora o reuso deixe o acervo com outra data.** A própria prova da volta espera `data_evento=D0` depois de costurar a linha no chamado de `DIA`; o reuso atualiza célula/destino, sem atualizar `data_evento`. `app/chamados/tests/test_e1_v15.py:90-99`; `app/chamados/models.py:2797-2813`; `app/chamados/models.py:2836`.

| Campo/valor | Quem escreve nos caminhos examinados | Quem lê e para quê |
|---|---|---|
| `data_evento` | `_nascer`: `_dia_fato` obtido em `dia_do_turno_da_linha`, depois `create`; `reapontar_resolvedora`: data pergunta sem dia pelo mesmo wrapper; `rechavear_data_evento`: grava parâmetro `novo_dia`. `app/chamados/models.py:2749-2751`; `app/chamados/models.py:2840`; `app/chamados/services/fio.py:842-844`; `app/chamados/services/fio.py:962-968`. | Costura: wrapper/juiz; fecho: `_dia_do_fato`; identidade de busca: valor gravado; C6/tranca/plantio: valor gravado. `app/chamados/juizes.py:1993-1994`; `app/chamados/services/fio.py:1217-1243`; `app/chamados/services/fio.py:1196`; `app/chamados/juizes.py:332-336`; `app/chamados/services/materializacao.py:344-345`. |
| `celula` | `_nascer` busca por `_dia_fato`, atribui ao existente ou cria com ela; rechaveamento busca célula de `novo_dia`. `app/chamados/models.py:2768`; `app/chamados/models.py:2797-2799`; `app/chamados/models.py:2841`; `app/chamados/services/fio.py:959-963`. | C6 lê veredito da célula; alimentação de falta exige que `celula.data == data_evento`, descartando a FK divergente; chamado deriva seu dia pela célula antes dos outros degraus. `app/chamados/juizes.py:1166-1168`; `app/chamados/juizes.py:1199-1214`; `app/chamados/juizes.py:634-642`; `app/chamados/juizes.py:98-99`. |
| `batida_origem_id` | `_nascer` recebe campo explícito ou deriva pelo mesmo `batida_origem_do_nascimento` utilizado no `save` de inserção; deriva de `disputa.contexto.ultima_batida.id`, validando dono quando a derivação ocorre. `app/chamados/models.py:2745-2747`; `app/chamados/models.py:3270-3271`; `app/chamados/models.py:3282-3289`. | Wrapper decide se consulta juiz; `_dia_do_fato` carrega Batida e consulta `_T1`; busca/dedup usa episódio. `app/chamados/services/fio.py:1259-1262`; `app/chamados/services/fio.py:1232-1239`; `app/chamados/models.py:2781`; `app/chamados/models.py:2680`. |
| `chamado_destino` | Nascimento cria com destino; reuso atribui após guardas; reapontamento atribui e salva com lavra. `app/chamados/models.py:2841`; `app/chamados/models.py:2800-2813`; `app/chamados/services/fio.py:859-874`. | Costura compara dia; `chamado_que_lista` escolhe destino antes do pai; plantio usa destino para marcos; guarda terminal julga C6 no pós estado. `app/chamados/juizes.py:1994-2029`; `app/chamados/services/fio.py:116-117`; `app/chamados/services/materializacao.py:339`; `app/chamados/services/materializacao.py:409`; `app/chamados/services/fio.py:778-783`. |

Contraexemplo de leitura mantido pelo código: linha com episódio cujo juiz diz `DIA`, gravada em `D0`; `abrir(..., DIA)` elege a linha, associa célula de `DIA`, aceita costura pelo juiz e preserva `D0`. Então:

- A costura compara `DIA`, enquanto `_dia_de_turno_da_pergunta` devolve `D0`. `app/chamados/models.py:2749-2751`; `app/chamados/models.py:2768`; `app/chamados/models.py:2802`; `app/chamados/juizes.py:1994`; `app/chamados/juizes.py:336`.
- A tranca calcula competência por `D0`. `app/chamados/juizes.py:948-961`.
- C6, quando alcança falta do dia, consulta `D0`; a célula de `DIA` não serve para alimentar essa consulta porque a igualdade falha. `app/chamados/juizes.py:1216-1222`; `app/chamados/juizes.py:638`.
- Plantio tipo hora combina resposta com `D0`, depois faz ajuste overnight a partir de `D0`. Não consulta o novo wrapper nesse caminho. `app/chamados/services/materializacao.py:344-345`; `app/chamados/services/materializacao.py:414-421`.
- A guarda de costura em chamado terminal decide competência entregue por `p.data_evento`, portanto também pelo gravado. `app/chamados/services/fio.py:776`.

Isso demonstra discrepância sem exigir migração de leitores, cuja preservação está expressamente no escopo dado; comprovar que a migração já eliminou o estado em produção é **[nao verificado]**. `app/chamados/tests/test_e1_v15.py:98-99`.

**Item 7 — classes verificadas.**

- **Campo com dois significados: PROCEDENTE.** O código declara expressamente data gravada como chave e `_dia_do_fato` como identidade distinta; o reuso conserva divergência observável entre leitores acima. `app/chamados/services/fio.py:1220-1227`; `app/chamados/tests/test_e1_v15.py:98-99`.
- **Segundo juiz geométrico duplicando `_T1`: não demonstrado.** O registro aponta `turnos_do_colab`; o wrapper chama `_dia_do_fato` → `dia_do_turno_da_batida` → `dia_das_batidas` → `turnos_do_colab`. Não há outro pareador nesse trajeto. `app/core/juizes.py:196`; `app/chamados/services/fio.py:1236-1238`; `app/ponto/turnos.py:2923-2926`; `app/ponto/turnos.py:2902-2903`.
- **Derivação paralela de fonte: PROCEDENTE no fallback sem resposta, não na geometria.** O juiz documenta `None` como “sem resposta do juiz não há dia a gravar”, mas `dia_do_turno_da_linha` devolve pedido/gravado nessa situação. `app/ponto/turnos.py:2919-2920`; `app/chamados/services/fio.py:1258-1262`.
- **Derivação duplicada de episódio: não demonstrada.** Porta e inserção chamam a mesma função. `app/chamados/models.py:2745`; `app/chamados/models.py:3271`.
- **Relógio fora da borda: não demonstrado nas linhas alteradas.** O relógio identificado é a porta própria de conversa, que captura `localdate()`; conversa é explicitamente civil e excluída de confronto de turno. `app/chamados/models.py:2700-2707`; `app/chamados/juizes.py:1991-1992`. Auditoria transitiva integral de relógios: **[nao verificado]**.
- **Ordem de trava nova incorreta: não demonstrada nas linhas alteradas.** `_nascer` toma Colaborador antes das novas consultas e da costura; reapontamento toma Colaborador por `fato_livre_para` ou diretamente antes da guarda que trava Chamado. `app/chamados/models.py:2720-2724`; `app/chamados/services/fio.py:852-861`; `app/chamados/services/fio.py:768`. Auditoria de todos os chamadores/transações externos: **[nao verificado]**.
- Segundo escritor fora da porta e alcance/mordida do selo pertencem ao censo do outro explorer: **[nao verificado]**.

## 08/11 11:23 · sessao c343206cd790 · FIO-E0-V4-E1 · censo · origem=Code

**As classes propostas não cobrem três dos cinco achados:** o Codex viu, e a triagem confirmou sem curar. Chamá-los de **(b)** falsearia o registro, porque a triagem manteve 3a/3b como bloqueadores. — `triagem_e1_v14b.txt:3,5-6`

Numeração abaixo: primeira linha após cada marcador `== anexo: ... ==` = linha 1, contando linhas vazias.

1. **3a — segunda chamada idêntica → `FatoEmOutroDia`: sem classe aplicável entre (a–e).** Foi visto e mantido como bloqueador. — `codex_e1_v14b.r:39`; `triagem_e1_v14b.txt:5`

   **Pergunta:** “o comportamento novo (FatoEmOutroDia ou reaproveitar a linha de outra data gravada e lhe dar a celula de data_evento) esta certo pela lei do aval?” — `codex_e1_v14b.q:8`

   **Resposta:** “Portanto `abrir(D0, batida cujo juiz diz DIA)` pode criar em D0; uma chamada direta posterior igual pode recusar a própria linha pelo juiz.” — `codex_e1_v14b.r:39`

   **Triagem:** “NAO curado: PERGUNTA DE DESENHO (duas curas, ver relato). Bloqueia pouso.” **Motivo escrito de descarte: nao ha trecho.** — `triagem_e1_v14b.txt:5` [nao verificado]

2. **3b — `_resolvedora_valida` compara a data gravada: sem classe aplicável entre (a–e).** Foi visto e mantido como bloqueador. — `codex_e1_v14b.r:43`; `triagem_e1_v14b.txt:6`

   **Pergunta:** “Todos os escritores e leitores supoem o MESMO significado (dia civil gravado x dia do turno x instante)?” — `codex_e1_v14b.q:6`

   **Resposta:** “A segunda validação lê essa data antiga e lança `ResolvedoraCruzada`, pois a diferença supera um dia.” O mesmo parágrafo cita `app/chamados/models.py:2786` e `app/chamados/juizes.py:1988`. — `codex_e1_v14b.r:43`

   **Triagem:** “PROCEDENTE, provado. NAO curado: mesma PERGUNTA DE DESENHO. Bloqueia pouso.” **Motivo escrito de descarte: nao ha trecho.** — `triagem_e1_v14b.txt:6` [nao verificado]

3. **`data_evento` com dois significados: sem classe aplicável entre (a–e).** Foi visto e confirmado como causa raiz, sem cura naquela volta. — `codex_e1_v14b.r:15,91`; `triagem_e1_v14b.txt:3`

   **Pergunta:** “campo com dois significados (data_evento: chave de busca x identidade -- esta declarado e respeitado em todo leitor?)” — `codex_e1_v14b.q:12`

   **Resposta:** “**Campo com dois significados não reconciliados — procedente, gravidade alta:** a identidade nova usa o juiz, enquanto destino, C6 e plantio continuam usando a data gravada como dia do fato.” — `codex_e1_v14b.r:91`

   **Triagem:** “PROCEDENTE como causa raiz de 3a/3b; vai junto da PERGUNTA DE DESENHO (nao curado nesta volta).” **Motivo escrito de descarte: nao ha trecho.** — `triagem_e1_v14b.txt:3`

4. **Outro componente fica fora sem recusa: (c), fora do alcance da árvore perguntada.** A pergunta identifica `5e55c211`; a eleição de um único componente aparece na triagem como cura da volta 14b. O laudo posterior aponta o residual dessa cura. — `codex_e1_v14b.q:1`; `triagem_e1_v14b.txt:4`; `opus_final2_e1_6695e972.md:8`

   **Pergunta relacionada:** “e o universo que a porta (universo_do_fato) elege e exatamente o universo_da_linha de seus membros?” — `codex_e1_v14b.q:7`

   **Resposta sobre a implementação anterior:** “o fecho preserva todas as sementes, sem exigir conectividade entre elas.” — `codex_e1_v14b.r:29`

   **Registro da mudança:** “CURADO na volta 14b: fio.py::universo_do_fato elege UM componente (o da canonica_do_grupo das sementes)”. — `triagem_e1_v14b.txt:4`

   **Pergunta ou resposta sobre o residual específico da nova escolha — componente omitido sem recusa: nao ha trecho.** Esse residual consta do laudo posterior: “o outro nao entra em recusadas nem fecho_cruza_dias, nenhum contador o ve”. — `opus_final2_e1_6695e972.md:8`

5. **Custo dos juízes: (b), visto e descartado como bloqueio, mantido em fila.** — `codex_e1_v14b.r:59,66,72`; `triagem_e1_v14b.txt:9`

   **Pergunta:** “quantas chamadas ao juiz por abrir, por fecho, por fecho_cruza_dias? Memo por batida basta?” — `codex_e1_v14b.q:10`

   **Resposta:** “Zero com busca vazia; caso contrário, B no universo/ponte/fecho compartilhado.” Também: “**Memo por batida não basta para compartilhar trabalho por turno/janela**”. — `codex_e1_v14b.r:66,72`

   **Motivo escrito na triagem:** “descartado como bloqueio -- nenhum selo de teto vermelho na suite de 14300; fila (medir na frota).” A medição na frota permanece **[nao verificado]**. — `triagem_e1_v14b.txt:9` [nao verificado]

## 08/11 11:04 · sessao b5a11839aaae · PERGUNTA-SEM-SELO · censo · origem=Code

**A medida antiga precisa de correção:** S1 recebe hoje a contagem como parâmetro; S3 mantém consulta crua para idempotência, mas o julgamento já verifica a retratação no retorno. Permanecem consultas cruas em S2, S4, S5 e S6. (`app/ponto/supra_juiz.py:207`; `app/chamados/juizes.py:235`; `app/chamados/juizes.py:775`; `app/ponto/services/veredito_celula.py:215`; `app/chamados/services/materializacao.py:94`; `app/ponto/services/flip_auto.py:35`; `app/ponto/utils.py:244`.)

**1. S1–S6 nesta cópia**

| Sítio | Universo atual e consequência |
|---|---|
| **S1 — `classificar_dia`** | Não monta universo: recebe `batidas_no_dia` e usa essa contagem no fallback. O comando `supra_juiz` carrega cru, conta e entrega esse insumo; `emitir_furo_retroativo` faz o mesmo. O cartório usa células da grade e órfãs. (`app/ponto/supra_juiz.py:207`; `app/ponto/supra_juiz.py:513`; `app/ponto/management/commands/supra_juiz.py:113`; `app/ponto/management/commands/supra_juiz.py:222`; `app/ponto/management/commands/supra_juiz.py:229`; `app/ponto/management/commands/emitir_furo_retroativo.py:53`; `app/ponto/management/commands/emitir_furo_retroativo.py:100`; `app/ponto/services/cartorio.py:1373`; `app/ponto/services/cartorio.py:1419`.) |
| **S2 — `veredito_celula`** | **Cru-JULGAMENTO.** `reais` entra em `grade_dia` e decide se o dia já está completo. `grade_dia` encaminha a lista recebida à grade. (`app/ponto/services/veredito_celula.py:215`; `app/ponto/services/veredito_celula.py:220`; `app/ponto/services/veredito_celula.py:223`; `app/ponto/services/simular_dia.py:5`.) |
| **S3 — `_batida_existente_na_janela`** | **Consulta crua de idempotência.** O julgamento `_pergunta_materializada_de_fato` já exige `retratada_em is None` no retorno. A justificativa para preservar o cru no plantio está escrita: impedir replantio da batida retratada. A cura é local ao julgamento, não ao helper inteiro. (`app/chamados/juizes.py:227`; `app/chamados/juizes.py:235`; `app/chamados/juizes.py:768`; `app/chamados/juizes.py:774`; `app/chamados/juizes.py:775`.) |
| **S4 — `_vizinha_mesmo_tipo`** | O código correspondente está em **`chamados/services/materializacao.py`**: duas consultas cruas decidem bloqueio de materialização por paridade. (`app/chamados/services/materializacao.py:85`; `app/chamados/services/materializacao.py:94`; `app/chamados/services/materializacao.py:100`; `app/chamados/services/materializacao.py:451`; `app/chamados/services/materializacao.py:850`.) |
| **S5 — `pares_mesmo_tipo`** | **Cru-JULGAMENTO.** A consulta crua define adjacência e pares que alimentam a fila de flip. A consulta posterior a `batidas_do_dia_turno` não altera esse universo inicial. (`app/ponto/services/flip_auto.py:35`; `app/ponto/services/flip_auto.py:41`; `app/ponto/services/flip_auto.py:104`; `app/ponto/services/flip_auto.py:146`.) |
| **S6 — `proxima_batida_de`** | **Cru-JULGAMENTO na função:** última marca crua determina o próximo tipo. A autoridade registrada para próximo tipo é `turnos.proximo_tipo_de`. Chamadores dinâmicos de `proxima_batida_de`: **[não verificado]**. (`app/ponto/utils.py:240`; `app/ponto/utils.py:244`; `app/ponto/utils.py:246`; `app/core/juizes.py:197`.) |

Commit de cura: **[não verificado]**, pois esta cópia não oferece histórico Git consultável. A prova atual de cura do julgamento de S3 é `app/chamados/juizes.py:775`.

Há outro uso de S3 que merece separação: a apresentação declara `ja_satisfeita` verificando somente `aprovada`; o plantio reutiliza qualquer existente. Isso não equivale à verificação explícita de retratação feita pelo juiz. (`app/chamados/services/materializacao.py:446`; `app/chamados/services/materializacao.py:447`; `app/chamados/services/materializacao.py:834`; `app/chamados/services/materializacao.py:840`; `app/chamados/juizes.py:775`.)

**2. Censo AST**

Unidade do inventário: uma raiz de consulta persistente ou uma chamada à porta; encadeamentos contam uma vez. São **154 raízes de consulta + 47 chamadas à porta = 201 sítios**, discriminados abaixo. A implementação da própria porta entra em **(a)**. Os três acessos por manager relacionado estão em `app/chamados/juizes.py:720`, `app/chamados/services/vigia_carimbo.py:113` e `app/ponto/management/commands/reconciliar_grade.py:118`.

| Classe | Total do inventário abaixo |
|---|---:|
| **a — porta**, incluindo sua implementação | **48** |
| **b — `retratada_em` explícito**, inclusive verificação em memória | **51** |
| **c — cru-exibição** de marcação, histórico ou indicadores da marcação | **22** |
| **d — cru-JULGAMENTO** | **28** |
| **e — cru-lookup**, identidade ou idempotência de escrita | **27** |
| **f — indeterminado na taxonomia**, com motivo indicado | **25** |
| **Total** | **201** |

Ocorrências executáveis de `.raw()` ou SQL direto de `ponto_batida` nesse recorte: **nenhuma encontrada pela varredura**; SQL construído dinamicamente e acessos externos à cópia: **[não verificado]**.

**(a) Porta — 48 sítios**

```text
app/api/views.py:2026 api_justificativas
app/chamados/juizes.py:1462 via_pelo_fato
app/chamados/services/validacao.py:828 classe_do_lote
app/colaboradores/services/detalhe.py:49 contexto_detalhe
app/core/services/painel_op.py:83 montar_painel_operacional
app/core/services/painel_op.py:269 montar_painel_operacional
app/core/views.py:114 _batidas_hoje
app/escala/detector_proposta.py:56 medir_padrao
app/escala/regua_defesa.py:153 diagnostico_regua
app/escala/regua_defesa.py:520 entradas_fora_do_inicio
app/escala/services/fase_na_tela.py:47 _trabalhados
app/escala/services/previsto_cego.py:159 medir
app/escala/services/semana_do_posto.py:117 _bateram
app/escala/utils.py:1505 montar_grade_prevista_periodo_por_turno
app/folha/export.py:499 dias_ferias_com_batida
app/folha/export.py:906 classificar_export
app/folha/previa.py:58 montar_previa
app/pautas/services.py:420 contexto_do_dia_texto
app/ponto/management/commands/aplicar_09_corte_b.py:126 _tem_saida_real_em_turno_aberto
app/ponto/management/commands/censo_col900.py:128 handle
app/ponto/management/commands/diff_espelho_alimentacao.py:92 handle
app/ponto/management/commands/e6_oraculo.py:152 __init__
app/ponto/management/commands/e6_oraculo.py:212 batida_fora_de_turno
app/ponto/management/commands/mapa_divergencia.py:96 handle
app/ponto/management/commands/reconciliar_fantasmas.py:96 _completude
app/ponto/management/commands/tripwire_celulas.py:283 medir_sobra_com_tipo_trocado
app/ponto/management/commands/vigiar_motor_autoridade.py:54 handle
app/ponto/services/abriu_nao_bateu.py:174 _bateu_no_dia
app/ponto/services/cartorio.py:1123 _julgar_colab_corpo
app/ponto/services/cobertura.py:299 declarar_cobertura_passada
app/ponto/services/deslogados.py:172 apurar
app/ponto/services/dia_do_colab.py:60 dia_do_colab
app/ponto/services/diagnostico_escala.py:142 diagnostico_escala
app/ponto/services/dias_em_aberto.py:122 passivo_da_competencia
app/ponto/services/esmeril_espelho.py:156 assinaturas_do_colab
app/ponto/services/espelho.py:1022 autoridade_do_periodo
app/ponto/services/faltas_de_hoje.py:57 faltas_de_hoje_sem_cobranca
app/ponto/services/fechamento.py:633 recalcular_fechamento_mes
app/ponto/services/fechamento.py:811 recalcular_fechamento_mes
app/ponto/services/triagem_batida.py:175 triagem_bater_ponto
app/ponto/services/vigia_de_hora.py:76 _acesas_de_sobra
app/ponto/turnos.py:2864 batidas_apuraveis — implementação
app/ponto/turnos.py:2962 _ultima_de
app/ponto/turnos.py:2982 turnos
app/ponto/utils_turno_orfao.py:28 detectar_bloqueio_turno_orfao
app/relatorios/management/commands/diff_extrato_parcial.py:37 _antes
app/relatorios/pdf_espelho.py:272 _coletar_dados_espelho
app/relatorios/views.py:502 extrato_parcial
```

**(b) Retratação explícita — 51 sítios**

```text
app/api/views.py:703 api_bater_ponto
app/api/views_core.py:700 api_bater_ponto
app/chamados/juizes.py:720 _pergunta_materializada_de_fato
app/chamados/management/commands/carimbar_por_batida_coincidente.py:70 handle
app/chamados/reconciliador.py:206 _par_integro — rejeição em memória na linha 216
app/chamados/services/adesao.py:60 sincronizar_adesao
app/chamados/services/adesao.py:144 sincronizar_adesao
app/chamados/services/desvio_escala.py:60 medir_desvio
app/chamados/services/fila_por_causa.py:190 particionar
app/chamados/services/limbo_folgas.py:35 sugerir_folga
app/chamados/services/limbo_folgas.py:155 semana_muda
app/chamados/services/materializacao.py:616 perguntas_absorvidas_por
app/chamados/services/vigia_carimbo.py:113 classificar
app/chamados/veredito_lavrado.py:140 marcos_do_lote
app/colaboradores/management/commands/corrigir_escala_retroativa.py:94 _rodar
app/colaboradores/services/calendario.py:239 contexto_calendario
app/colaboradores/services/ficha.py:145 ficha_do_colab
app/colaboradores/services/pos_vinculo.py:38 fechar_adesao_com_batida
app/colaboradores/services/senha_provisoria.py:24 senhas_provisorias
app/core/services/painel_op.py:92 montar_painel_operacional
app/core/views.py:173 dashboard
app/core/views.py:371 dashboard_supervisor
app/core/views.py:775 painel_tv
app/core/views.py:1177 dashboard_dados
app/escala/services/meio_periodo.py:158 observado_meio_periodo
app/escala/services/propositor.py:477 propostas
app/ferias/situacao.py:277 situacao_de_ferias
app/ponto/management/commands/diff_calculador.py:270 handle
app/ponto/management/commands/diff_janela_he.py:93 handle
app/ponto/management/commands/e6_piso_autopsia.py:71 handle
app/ponto/management/commands/marcar_foto_ausente_retro.py:66 handle
app/ponto/registro_batida.py:107 registrar_batida
app/ponto/registro_batida.py:139 registrar_batida
app/ponto/services/adesao.py:34 primeira_batida
app/ponto/services/adesao.py:58 ids_com_adesao
app/ponto/services/cartorio.py:1154 _julgar_colab_corpo
app/ponto/services/deslogados.py:161 apurar
app/ponto/services/deslogados.py:174 apurar
app/ponto/services/faltas_de_hoje.py:55 faltas_de_hoje_sem_cobranca
app/ponto/services/flip_auto.py:226 etiquetas_pela_celula — exclusão em memória na linha 229
app/ponto/services/lastro.py:261 julgar
app/ponto/services/passivo_s84.py:53 vivas
app/ponto/services/piso_intervalo.py:39 pares_abaixo_do_piso
app/ponto/turnos.py:1599 papel_por_minuto_da_ata
app/ponto/turnos.py:1861 turnos_do_colab
app/ponto/turnos.py:1899 _ultima_bat
app/ponto/turnos.py:2217 _turno_aberto_calc
app/ponto/turnos.py:2350 turnos_abertos_de
app/ponto/turnos.py:2382 sequencia_relogio_x_jornada
app/ponto/turnos.py:2466 proximo_tipo_de
app/ponto/turnos.py:2903 dia_das_batidas
```

**Duas ressalvas sobre (b):** calendário seleciona explicitamente as **retratadas para exibição**; `marcos_do_lote` seleciona o instante de retratação para invalidar lavra. Portanto, “tem `retratada_em` explícito” não significa automaticamente “consulta de apuráveis”. (`app/colaboradores/services/calendario.py:239`; `app/colaboradores/services/calendario.py:619`; `app/chamados/veredito_lavrado.py:140`.)

**(c) Cru-exibição — 22 sítios**

```text
app/api/views.py:297 api_me
app/api/views_core.py:286 api_me
app/chamados/services/acoes_chamado.py:377 montar_contexto_detalhe
app/chamados/views.py:692 fio_colaborador
app/colaboradores/painel_cell.py:33 montar_linha_colab
app/colaboradores/services/detalhe.py:20 contexto_detalhe
app/colaboradores/services/drawer.py:34 contexto_drawer
app/colaboradores/services/situacional.py:137 montar_situacional
app/colaboradores/views.py:53 vincular_colaboradores — indicador de marcação com GPS
app/colaboradores/views.py:337 vincular_posto — indicador tem_geo
app/colaboradores/views.py:356 vincular_posto — indicador tem_geo
app/colaboradores/views.py:395 _render_row — indicador tem_geo
app/colaboradores/views.py:928 painel_colaborador — total de registros
app/colaboradores/views.py:931 painel_colaborador — registros do mês
app/core/cell.py:10 cell_ultimas_batidas
app/core/views.py:194 dashboard — últimas marcações
app/core/views.py:219 dashboard — total de registros
app/core/views.py:991 batida_detalhe_json — histórico
app/core/views.py:1163 dashboard_dados — feed de marcações
app/ponto/views.py:191 espelho_ponto — últimas seis
app/ponto/views.py:441 batidas_offline_pendentes — fila de marcações
app/relatorios/services.py:230 gerar_pdf_espelho_lote_bytes — informação de movimento
```

O drawer calcula presença usando turnos, separadamente da lista crua exibida; o situacional também separa a última marca exibida do julgamento de uso/adesão. (`app/colaboradores/services/drawer.py:34`; `app/colaboradores/services/drawer.py:45`; `app/colaboradores/services/drawer.py:58`; `app/colaboradores/services/situacional.py:146`; `app/colaboradores/services/situacional.py:150`.)

**(d) Cru-JULGAMENTO — 28 sítios**

| Arquivo:linha | Função | O que o resultado decide |
|---|---|---|
| `app/chamados/management/commands/auditar_disputas.py:103` | `handle` | Qualificação de tipo invertido no relatório. |
| `app/chamados/services/explicador.py:40` | `_explicar_validada` | Existência/tipo da prova de materialização. |
| `app/chamados/services/explicador.py:53` | `_explicar_validada` | Diagnóstico e encaminhamento para cadastro. |
| `app/chamados/services/explicador.py:132` | `por_que_nao_fecha` | Alimentação desses diagnósticos em lote. |
| `app/chamados/services/materializacao.py:94` | `_vizinha_mesmo_tipo` | Vizinha anterior que barra plantio. |
| `app/chamados/services/materializacao.py:100` | `_vizinha_mesmo_tipo` | Vizinha posterior que barra plantio. |
| `app/core/views.py:360` | `dashboard_supervisor` | Entrada usada para excluir alguém dos ausentes. |
| `app/core/views.py:793` | `painel_tv` | Atividade que entra na classificação de cobertura. |
| `app/escala/services/furos_diarios.py:261` | `universo_empresa` | Nunca bateu/sumiu e última atividade. |
| `app/escala/services/furos_diarios.py:267` | `universo_empresa` | Escala versus realidade no período. |
| `app/ponto/management/commands/detectar_par_relampago.py:205` | `_alvo_retratavel` | Guardas e escolha da retratação. |
| `app/ponto/management/commands/detectar_par_relampago.py:290` | `varrer_pares_embutidos` | Adjacência e pares candidatos. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:53` | `handle` | Universo contado para classificar dia. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:74` | `handle` | Piso de adesão usado no julgamento. |
| `app/ponto/management/commands/reconciliar_grade.py:84` | `handle` | Seleção do universo de colaboradores. |
| `app/ponto/management/commands/reconciliar_grade.py:88` | `handle` | Piso de adesão que suprime dias. |
| `app/ponto/management/commands/reconciliar_grade.py:118` | `handle` | `c.batidas.all()` alimenta grade/missing. |
| `app/ponto/management/commands/supra_juiz.py:113` | `handle` | Universo contado para classificar dia. |
| `app/ponto/management/commands/supra_juiz.py:132` | `handle` | Adesão entregue ao classificador. |
| `app/ponto/propositor.py:346` | `dia_posicional_completo` | Completude posicional do dia. |
| `app/ponto/services/flip_auto.py:35` | `pares_mesmo_tipo` | Pares que alimentam fila de flip. |
| `app/ponto/services/flip_auto.py:291` | `avaliar_fila` | Dia perfeito antes/depois do flip. |
| `app/ponto/services/geofence.py:51` | `vinculo_contagem` | Evidência que julga divergência de vínculo. |
| `app/ponto/services/veredito_celula.py:215` | `veredito_celula` | Completude, flips e materialização. |
| `app/ponto/services/vinculo_na_lista.py:49` | `achar` | Classificação de evidência de vínculo/posto. |
| `app/ponto/utils.py:186` | `colabs_disponiveis_para_cobertura` | Exclusão de candidatos por suposto turno aberto. |
| `app/ponto/utils.py:244` | `proxima_batida_de` | Próximo tipo de batida. |
| `app/ponto/views.py:1598` | `_abrir_justificativa_fluxo` | Entrada vinculada à justificativa. |

Em `avaliar_fila`, excluir retratadas somente da lista de flips **não protege o julgamento**: `reais` continua inteira em `grade_dia`. (`app/ponto/services/flip_auto.py:291`; `app/ponto/services/flip_auto.py:298`; `app/ponto/services/flip_auto.py:303`.)

Os detectores de par filtram `aprovada=True`, mas isso não é o filtro legal de retratação: o modelo define `aprovada` como crivo do DP. Efeito concreto disso nos registros de produção: **[não verificado]**. (`app/ponto/management/commands/detectar_par_relampago.py:205`; `app/ponto/management/commands/detectar_par_relampago.py:290`; `app/ponto/models.py:66`.)

**(e) Cru-lookup/identidade/idempotência — 27 sítios**

```text
app/api/views.py:559 api_bater_ponto — idempotência
app/api/views.py:856 api_upload_foto — pk
app/api/views_core.py:551 api_bater_ponto — idempotência
app/api/views_core.py:846 api_upload_foto — pk
app/chamados/detector_anomalias.py:43 detectar_anomalias — pk
app/chamados/juizes.py:49 _dia_da_batida — pk
app/chamados/juizes.py:194 dias_do_fato — pk__in
app/chamados/juizes.py:235 _batida_existente_na_janela — idempotência compartilhada
app/chamados/services/acoes_chamado.py:262 acao_recusar_geofence — pk
app/chamados/services/passivo_validado.py:513 _plantar_o_dia — marcador de pk anterior à escrita
app/chamados/services/passivo_validado.py:527 _plantar_o_dia — identificação do que a escrita criou
app/chamados/services/regularizacao_ext.py:73 _cria_batida — chave colaborador/instante
app/chamados/views_regularizacao.py:109 regularizacao_add_item — pk
app/core/views.py:978 batida_detalhe_json — pk
app/ponto/management/commands/diff_passivo_s84.py:79 _mutacao — pk
app/ponto/management/commands/flip_automatico.py:44 handle — pk__in
app/ponto/management/commands/passivo_disputa_retro.py:158 handle — pk
app/ponto/registro_batida.py:120 registrar_batida — chave colaborador/instante
app/ponto/services/flip_auto.py:135 fila_flip_recusas — pk
app/ponto/services/flip_auto.py:272 avaliar_fila — pk
app/ponto/services/geofence.py:406 reavaliar_chamados_geofence — pk__in
app/ponto/services/regularizacao.py:52 regularizar_turno_orfao — pk
app/ponto/services/triagem_batida.py:257 ramo_regularizar_saida — pk
app/ponto/services/triagem_batida.py:670 ramo_registrar_es — idempotência
app/ponto/services/triagem_batida.py:722 ramo_registrar_es — identificação da oposta na resposta
app/ponto/views.py:456 batidas_offline_pendentes — pk
app/ponto/views.py:2899 salvar_foto_batida — id
```

O cru por chave exata em `registrar_batida` é deliberado: reenviar a mesma chave deve recuperar inclusive a marca retratada, evitando colisão com a constraint. (`app/ponto/registro_batida.py:116`; `app/ponto/registro_batida.py:120`.)

**(f) Indeterminado na taxonomia — 25 sítios, com motivo**

Aqui “indeterminado” significa que a leitura não cabe integralmente em exibição de marcação, julgamento, porta ou lookup de escrita; o uso observado está declarado em cada linha.

| Arquivo:linha | Função | Motivo |
|---|---|---|
| `app/api/hora_do_aparelho.py:173` | `descartadas` | Auditoria da diferença entre relógio do aparelho e timestamp gravado. |
| `app/chamados/veredito_lavrado.py:137` | `marcos_do_lote` | Carimbo de criação para invalidar lavra; não conta trabalho. |
| `app/core/media_protegida.py:31` | `servir_media_protegida` | Autorização de acesso à foto. |
| `app/core/views_config.py:528` | `saude_sistema` | Contagem física de saúde do sistema. |
| `app/escala/management/commands/hub_smoke.py:98` | `win_map` | Foto antes/depois para detectar mutação durante smoke. |
| `app/ponto/management/commands/censo_col900.py:133` | `handle` | Diagnóstico de existência crua quando a porta não encontrou marca. |
| `app/ponto/management/commands/checklist_golive.py:100` | `handle` | Contagem física global. |
| `app/ponto/management/commands/checklist_golive.py:101` | `handle` | Contagem para alerta sobre dados de teste. |
| `app/ponto/management/commands/gerar_batidas_smoke_motor.py:61` | `_limpar` | Seleção para limpeza de ensaio. |
| `app/ponto/management/commands/mapa_divergencia.py:62` | `handle` | Seleção ampla de candidatos; comparação carrega pela porta. |
| `app/ponto/management/commands/processar_alertas_turno.py:59` | `handle` | Seleção para disparar `recompute_turnos`; não entreg

[truncado em 20000 de 32584 caracteres]

## 08/11 11:04 · sessao 92c5caf6b911 · COBPARC-AUSENCIA-CALA-DOIS-JUIZES · sonda · origem=Code

1. Contagens por classe, competência e empresa:

| Classe | Competência | Empresa | Contagem | Entregue K6 | Trancada K4 | Referência |
|---|---|---:|---:|---|---|---|
| carimbo_sem_periodo | 10/2026 | 2 | 14 | False | False | [nao verificado] |
| carimbo_sem_periodo | 11/2026 | 2 | 4 | False | False | [nao verificado] |
| carimbo_sem_periodo | 10/2026 | 4 | 2 | False | False | [nao verificado] |
| carimbo_sem_periodo | 11/2026 | 4 | 1 | False | False | [nao verificado] |
| sem_carimbo | 08/2026 | 2 | 18 | True | True | [nao verificado] |
| sem_carimbo | 09/2026 | 2 | 27 | True | False | [nao verificado] |
| sem_carimbo | 08/2026 | 3 | 1 | False | True | [nao verificado] |
| sem_carimbo | 09/2026 | 3 | 5 | True | False | [nao verificado] |
| sem_carimbo | 08/2026 | 4 | 5 | False | True | [nao verificado] |
| sem_carimbo | 09/2026 | 4 | 5 | True | False | [nao verificado] |
| sem_celula | 07/2026 | 2 | 1 | False | False | [nao verificado] |

2. Colab-dias por classe, considerando “aberta” como `entregue=False` em K6. [nao verificado]

| Classe | Entregue/exportada | Aberta | Total | Referência |
|---|---:|---:|---:|---|
| carimbo_sem_periodo | 0 | 21 | 21 | [nao verificado] |
| sem_carimbo | 55 | 6 | 61 | [nao verificado] |
| sem_celula | 0 | 1 | 1 | [nao verificado] |
| **Total** | **55** | **28** | **83** | [nao verificado] |

3. Nenhuma linha indicando erro ou traceback no trecho fornecido. [nao verificado]

As referências estão marcadas assim porque o dado foi fornecido no texto, sem arquivo:linha. [nao verificado]

## 08/11 10:58 · sessao ce0c1ff64080 · - · - · origem=Ronald (tmux)

Censo AST de `app/**/management/commands/*.py`: **47 sítios** — **a=10, b=6, c=0, d=10, e=3, f=18**. Unidade: chamada à porta ou raiz de consulta; encadeamentos contam uma vez. Foram resolvidos aliases importados (`_B`, `_bap`); `FotoBatida` excluída. As linhas abaixo constituem o inventário e sua evidência.

Prefixos usados na tabela:

- `P/` = `app/ponto/management/commands/`
- `C/` = `app/chamados/management/commands/`

| Classe | Arquivo:linha | Função | Motivo/contexto |
|---|---|---|---|
| a | `P/aplicar_09_corte_b.py:126` | `_tem_saida_real_em_turno_aberto` | `batidas_apuraveis` alimenta guarda de proteção de horas. |
| a | `P/censo_col900.py:128` | `handle` | Primeira batida pela porta. |
| a | `P/diff_espelho_alimentacao.py:92` | `handle` | Alimentação do motor pela porta. |
| a | `P/e6_oraculo.py:152` | `__init__` | Timestamps do oráculo pela porta. |
| a | `P/e6_oraculo.py:212` | `batida_fora_de_turno` | Existência de órfã confirmada na porta. |
| a | `P/mapa_divergencia.py:96` | `handle` | Batidas comparadas nas grades pela porta. |
| a | `P/reconciliar_fantasmas.py:96` | `_completude` | Universo carregado pela porta. |
| a | `P/tripwire_celulas.py:283` | `medir_sobra_com_tipo_trocado` | Evidência em torno do marco pela porta. |
| a | `P/vigiar_motor_autoridade.py:54` | `handle` | Alimentação pela porta. |
| a | `app/relatorios/management/commands/diff_extrato_parcial.py:37` | `_antes` | Alias `_bap` da porta. |
| b | `C/carimbar_por_batida_coincidente.py:70` | `handle` | `retratada_em__isnull=True`, aplicado na linha 73. |
| b | `app/colaboradores/management/commands/corrigir_escala_retroativa.py:94` | `_rodar` | Filtro explícito no universo de dias batidos. |
| b | `P/diff_calculador.py:270` | `handle` | Filtro explícito. |
| b | `P/diff_janela_he.py:93` | `handle` | Filtro explícito. |
| b | `P/e6_piso_autopsia.py:71` | `handle` | Filtro explícito. |
| b | `P/marcar_foto_ausente_retro.py:66` | `handle` | Filtro explícito na linha 68. |
| d | `C/auditar_disputas.py:103` | `handle` | Universo cru usado para qualificar tipo invertido; decisão de inversão nas linhas 119–123. É relatório, sem abrir/fechar chamado (comentário linha 107). |
| d | `P/detectar_par_relampago.py:205` | `_alvo_retratavel` | Filtra `aprovada=True`, não retratação; lista decide guardas e escolha de retratação nas linhas 208–214. |
| d | `P/detectar_par_relampago.py:290` | `varrer_pares_embutidos` | Filtra `aprovada=True`, não retratação; adjacência e pares nas linhas 294–310. |
| d | `P/emitir_furo_retroativo.py:53` | `handle` | Lista crua gera `n_bat` na linha 100 e entra em `classificar_dia` nas linhas 101–106. |
| d | `P/emitir_furo_retroativo.py:74` | `handle` | Primeira crua decide `adesao`, enviada ao julgamento na linha 105. |
| d | `P/reconciliar_grade.py:84` | `handle` | Universo cru de colaboradores, utilizado na seleção da linha 106. |
| d | `P/reconciliar_grade.py:88` | `handle` | `Min(timestamp)` cru define piso de adesão; piso suprime dia nas linhas 136–138. |
| d | `P/reconciliar_grade.py:118` | `handle` | Related `c.batidas.all()` cru alimenta montagem da grade e detecção de missing nas linhas 151–155. |
| d | `P/supra_juiz.py:113` | `handle` | Lista crua contada na linha 222 e enviada a `classificar_dia` nas linhas 223–229. |
| d | `P/supra_juiz.py:132` | `handle` | Primeira crua decide adesão na linha 135 e julgamento na linha 227. |
| e | `P/diff_passivo_s84.py:79` | `handle._mutacao` | Lookup por `pk`, depois guarda de retratação na linha 80. |
| e | `P/flip_automatico.py:44` | `handle` | Alias `_B`, busca por `pk__in` de flips já propostos. |
| e | `P/passivo_disputa_retro.py:158` | `handle` | Lookup por `pk` do passivo já selecionado. |
| f | `app/escala/management/commands/hub_smoke.py:98` | `handle.win_map` | Mapa cru de antes/depois para detectar mutação e excluir instabilidade do smoke, linhas 112–131; não é exibição nem apuração. |
| f | `P/censo_col900.py:133` | `handle` | Contador diagnóstico “retratadas mudariam”, somente quando a porta não achou batida; motivo explícito nas linhas 129–135. |
| f | `P/checklist_golive.py:100` | `handle` | Contagem física global exibida na linha 103; não é marcação nem apuração. |
| f | `P/checklist_golive.py:101` | `handle` | Contagem de colaboradores de teste para alerta de limpeza, linhas 104–105. |
| f | `P/gerar_batidas_smoke_motor.py:61` | `_limpar` | Seleção por marcador para limpeza em banco de ensaio; não decide apuração. |
| f | `P/mapa_divergencia.py:62` | `handle` | Alias `_B`: seleção ampla de colaboradores para auditoria; batidas efetivamente comparadas vêm da porta na linha 96. |
| f | `P/processar_alertas_turno.py:59` | `handle` | Seleção ampla de colaboradores para disparar `recompute_turnos` nas linhas 65–67; não alimenta pareamento cru. Caso se adote “seleciona materialização” como suficiente para (d), mover este sítio a d. |
| f | `P/testar_pwa.py:169` | `t05_batida_entrada` | Limpeza preparatória de ensaio. |
| f | `P/testar_pwa.py:178` | `t05_batida_entrada` | Confirma persistência de POST no smoke. |
| f | `P/testar_pwa.py:194` | `t05_batida_entrada` | Confirma persistência após repetir POST. |
| f | `P/testar_pwa.py:199` | `t05_batida_entrada` | Verifica resultado de smoke. |
| f | `P/testar_pwa.py:213` | `t06_batida_saida` | Verifica persistência de saída no smoke. |
| f | `P/testar_pwa.py:226` | `t07_batida_duplicada` | Limpeza preparatória do teste de idempotência. |
| f | `P/testar_pwa.py:232` | `t07_batida_duplicada` | Limpeza final do ensaio. |
| f | `P/vigia_plantio_overnight.py:30` | `batidas_plantadas_no_dia_errado` | Seleciona colaboradores por cru aprovado; evidência julgada vem de `turnos_do_colab` e `t.batidas`, linhas 38–39. |
| f | `P/zerar_dados_operacionais.py:43` | `handle` | Contagem de objetos antes de limpeza. |
| f | `P/zerar_dados_operacionais.py:96` | `handle` | Seleção de todos os objetos para limpeza de ensaio. |
| f | `app/tenants/management/commands/cron_healthcheck.py:17` | `handle.check` | Contagem física para healthcheck. |

Para eliminar ambiguidade na consolidação: **f aqui significa “uso operacional/diagnóstico fora das outras cinco classes”, com motivo verificado, não desconhecimento do fluxo**. Se o critério do usuário “resultado decide materialização” incluir disparo amplo de recompute, `processar_alertas_turno.py:59` deve ser **d**, dando **d=11, f=17**, demais totais iguais.

O caminho de `reconciliar_grade.py:118` tem bifurcação pela configuração: `app/escala/utils.py:2107` decide builder por turno ou legacy; o wrapper repassa a lista crua nas linhas 2108–2111. Portanto não foi classificado como porta, ainda que um builder possa voltar a buscar outra fonte (`app/escala/utils.py:2105`).

Interseção com E1, limitada a este inventário: **`app/escala/management/commands/hub_smoke.py:98`** e **`app/ponto/management/commands/vigia_plantio_overnight.py:30`**; pertença à lista E1 vem do anexo do usuário [não verificado por arquivo local].

## 08/11 10:58 · sessao 87740fa7e361 · - · - · origem=Ronald (tmux)

S1..S6 no código atual:

| Sítio | Resultado e prova |
|---|---|
| S1 | `classificar_dia` é pura: recebe `batidas_no_dia`, sem construir universo (`app/ponto/supra_juiz.py:204`, `:207`). Usa a quantidade como fallback quando `dia` não tem `orfas` (`:513`, `:521`). O produtor do command segue **cru**: query `app/ponto/management/commands/supra_juiz.py:113`, contagem `:222`, passagem `:229`. Também há o produtor cru em `app/ponto/management/commands/emitir_furo_retroativo.py:53`, contagem `:100`, passagem `:106`. Já o cartório calcula sobre células da grade e órfãs (`app/ponto/services/cartorio.py:1373`), passando o resultado em `:1419`. Portanto a localização antiga não representa o sítio de leitura atual. |
| S2 | **Cru-JULGAMENTO**: `veredito_celula` carrega `Batida.objects.filter` sem marca em `app/ponto/services/veredito_celula.py:215`; passa as reais para `grade_dia` em `:220`; o resultado decide “nada a corrigir” em `:223`. `grade_dia` apenas encaminha as batidas à grade (`app/ponto/services/simular_dia.py:5`); a grade monta `locs` sem excluir retratação (`app/escala/utils.py:1344`). |
| S3 | `_batida_existente_na_janela` é **crua**, documentada como idempotência (`app/chamados/juizes.py:227`, query `:235`). O chamador de julgamento já exige a marca nula **no objeto retornado** (`:774`, `:775`): cura explícita local. A razão de manter a função crua para não replantar retratadas está escrita em `:768`. Materialização reutiliza a existente em `app/chamados/services/materializacao.py:834`, `:840`; apresentação também decide `ja_satisfeita` lendo apenas `aprovada`, sem checar retratação, em `:446`, `:447`. Não é lookup por chave, e sim por janela; separar seu uso de idempotência do uso de satisfação. |
| S4 | O caminho `app/ponto/services/materializacao.py` não existe nesta cópia [não verificado por arquivo:linha]. O código correspondente está em `app/chamados/services/materializacao.py:85`, `_vizinha_mesmo_tipo`, com duas queries **cruas**, `:94` e `:100`. Decide bloqueio/recusa de materialização em `:451`, `:452`, `:850`, `:851`. |
| S5 | `pares_mesmo_tipo` continua **cru-JULGAMENTO**, query em `app/ponto/services/flip_auto.py:35`; essa sequência define os pares em `:41`–`:46` e alimenta a fila em `:146`. Há cura separada da contagem de dia: `classificar_pares` usa `batidas_do_dia_turno` em `:104`, autoridade que exclui retratação por `turnos_do_colab` (`app/ponto/turnos.py:3024`, `:3033`). Isso não cura a query de formação dos pares. |
| S6 | **Cru-JULGAMENTO** na própria função: `proxima_batida_de` lê a última batida sem marca em `app/ponto/utils.py:244` e decide o tipo em `:246`. Não encontrei chamada dessa função em produção por busca AST/textual [não verificado quanto a chamadas dinâmicas]. A autoridade registrada para a próxima batida é outra: `ponto/turnos.py::proximo_tipo_de`, em `app/core/juizes.py:197`. |

Não é possível provar commits pela cópia sem metadados Git [não verificado]. As curas acima têm prova nas linhas atuais, não em comentários históricos. [nao verificado]

Selos:

- **B5-j** vigia chamadas a `parear_turnos` e `_turnos_do_colab_puro`, incluindo aliases de importação (`app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:63`, `:66`, `:118`). Busca apenas `Batida.objects` literal dentro do escopo que chama essas funções (`:89`, `:93`). Considera qualquer query cuja representação contenha `retratada_em` conforme/copiadora; aceita referência à porta ou lookup `pk/id` (`:125`–`:131`). Não resolve fluxo entre funções nem managers relacionados; os limites declarados estão em `:26`–`:34`. Executando apenas suas funções AST em memória, o resultado foi **0 crus e 6 copiadores** [não verificado por artefato persistido; execução somente leitura].
- **Bocais** limita-se a seis arquivos: `ponto/services/fechamento.py`, `folha/export.py`, `relatorios/views.py`, `relatorios/pdf_espelho.py`, `ponto/views.py`, `api/views.py` (`app/ponto/tests/test_tripwire_bocais_apuracao.py:13`). Conta texto que casa `(?:Batida|_Bat)\.objects\.filter`, sem AST nem inspeção do predicado (`:25`, `:29`, `:30`). Permite 3 ocorrências em `ponto/views.py` e 7 em `api/views.py` (`:18`, `:20`).
- **Todos os sítios S1..S6 e os dois commands produtores crus de S1 ficam fora dos dois selos**, pelas restrições acima; executei `universos` sobre esses oito arquivos e todos retornaram `([], [])` [não verificado por artefato persistido]. Nenhum desses arquivos está no `ALLOW` dos bocais (`app/ponto/tests/test_tripwire_bocais_apuracao.py:13`).

Juiz:

- **Não há autoridade registrada para “esta batida conta?”**: a família `batida` registra períodos/intervalo, horas do par, geofence e selfie (`app/core/juizes.py:1173`–`:1183`); `turno/marcos` registra outras perguntas, não o universo apurável (`:195`–`:237`).
- **Não precisa criar função/lei nova para o universo**: `batidas_apuraveis` já se declara autoridade única, exclui retratadas e preserva a distinção de exibição (`app/ponto/turnos.py:2844`–`:2850`, filtro `:2864`); a lei a nomeia em `CLAUDE.md:155`.
- Registrar agora essa autoridade no registro vivo exige observar a regra de nascimento de autoridade, que pede corte nominal (`CLAUDE.md:74`–`:76`). Isso é decisão de registro; não demonstra lacuna na lei que define o universo (`CLAUDE.md:155`).

Decisões técnicas extraídas:

1. Migrar os universos de S1 nos dois commands para a porta; `classificar_dia` recebe insumo e não deve ganhar query (`app/ponto/management/commands/supra_juiz.py:113`, `app/ponto/management/commands/emitir_furo_retroativo.py:53`, `app/ponto/supra_juiz.py:207`).
2. S2, S4 e a formação de pares de S5 devem consultar universo apurável, pois seus resultados escolhem materialização/paridade/flip (`app/ponto/services/veredito_celula.py:215`, `app/chamados/services/materializacao.py:94`, `app/ponto/services/flip_auto.py:35`).
3. Manter o uso cru de S3 para impedir repetição de escrita; separar o julgamento de satisfação, que precisa de prova viva (`app/chamados/juizes.py:768`–`:775`; apresentação ainda sem essa marca em `app/chamados/services/materializacao.py:447`).
4. Para S6, escolher remoção de código sem chamadas localizadas ou delegação à autoridade já registrada da próxima batida (`app/ponto/utils.py:240`, `app/core/juizes.py:197`); chamadas dinâmicas não comprovadas [não verificado].
5. Ampliar selo para as leituras de julgamento que não chamam pareador e para aliases/managers relacionados; hoje os limites são explícitos (`app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:26`, `:89`; bocais `app/ponto/tests/test_tripwire_bocais_apuracao.py:13`, `:25`).

Nenhuma dessas mudanças de universo exige lei nova: a distinção apuração/exibição já está escrita (`CLAUDE.md:155`, `app/ponto/turnos.py:2847`). A exceção de idempotência tem justificativa técnica expressa no código (`app/chamados/juizes.py:768`).

## 08/11 10:58 · sessao 5a08669ba673 · - · - · origem=Ronald (tmux)

Lei e semântica: `CLAUDE.md:155-164`, `app/ponto/models.py:64-67` e `app/ponto/turnos.py:2844-2871` concordam: `retratada_em != NULL` exclui geometria/apuração, preserva exibição; `aprovada` significa crivo do DP, e pendente do DP ainda conta.

**Escritores encontrados fora de tests/migrations:**

- Escritor físico genérico: `app/ponto/registro_batida.py:235`, `:252-254` (`anotar`, `setattr` e `save(update_fields)`); campo permitido em `:205`.
- Retratar: `app/ponto/registro_batida.py:267`, `:271-274`: idempotência, grava `aprovada=False` e `retratada_em=timezone.now()`. Agenda relavra financeira em `:281`.
- Desfazer: `app/ponto/services/retratar_humano.py:190`, `:197-209`: grava `aprovada=True` e `retratada_em=None` pela mesma `anotar`.
- Chamadores que mandam retratar: `app/chamados/services/acoes_chamado.py:269`; `app/ponto/management/commands/detectar_par_relampago.py:224`; `app/ponto/management/commands/diff_passivo_s84.py:82` (ensaio, conforme `:75-76`, `:91`); `app/ponto/management/commands/passivo_disputa_retro.py:164`; `app/ponto/services/retratar_humano.py:179`.
- `app/ponto/services/retratar_humano.py:9-12` é histórico e já não serve como lista completa: afirma dois chamadores, mas os cinco acima existem.
- `app/ponto/registro_batida.py:152-158` aceita `**campos` na criação; não há passagem literal atual de `retratada_em` identificada nessas chamadas. Conteúdo arbitrário recebido dinamicamente: [não verificado].

**Leitores executáveis de `retratada_em`, levantados por AST (linhas; funções):**

| Arquivo | Leitores |
|---|---|
| `app/api/views.py` | `705 api_bater_ponto` |
| `app/api/views_core.py` | `702 api_bater_ponto` |
| `app/chamados/juizes.py` | `720,775 _pergunta_materializada_de_fato` |
| `app/chamados/management/commands/carimbar_por_batida_coincidente.py` | `73 handle` |
| `app/chamados/reconciliador.py` | `216 _par_integro` |
| `app/chamados/services/adesao.py` | `60,145 sincronizar_adesao` |
| `app/chamados/services/desvio_escala.py` | `60 medir_desvio` |
| `app/chamados/services/fila_por_causa.py` | `191 particionar` |
| `app/chamados/services/limbo_folgas.py` | `37 sugerir_folga`; `156 semana_muda` |
| `app/chamados/services/materializacao.py` | `616 perguntas_absorvidas_por` |
| `app/chamados/services/vigia_carimbo.py` | `113 classificar` |
| `app/chamados/veredito_lavrado.py` | `140-141 marcos_do_lote` |
| `app/colaboradores/management/commands/corrigir_escala_retroativa.py` | `95 _rodar` |
| `app/colaboradores/services/calendario.py` | `240,621 contexto_calendario` |
| `app/colaboradores/services/ficha.py` | `145 ficha_do_colab` |
| `app/colaboradores/services/pos_vinculo.py` | `38 fechar_adesao_com_batida` |
| `app/colaboradores/services/senha_provisoria.py` | `25 senhas_provisorias` |
| `app/core/services/painel_op.py` | `93 montar_painel_operacional` |
| `app/core/views.py` | `173 dashboard`; `372 dashboard_supervisor`; `775 painel_tv`; `1177 dashboard_dados` |
| `app/escala/services/meio_periodo.py` | `160 observado_meio_periodo` |
| `app/escala/services/propositor.py` | `478 propostas` |
| `app/ferias/situacao.py` | `278 situacao_de_ferias` |
| `app/ponto/management/commands/diff_calculador.py` | `271 handle` |
| `app/ponto/management/commands/diff_janela_he.py` | `94 handle` |
| `app/ponto/management/commands/diff_passivo_s84.py` | `80 _mutacao` |
| `app/ponto/management/commands/e6_piso_autopsia.py` | `72 handle` |
| `app/ponto/management/commands/marcar_foto_ausente_retro.py` | `68 handle` |
| `app/ponto/management/commands/passivo_disputa_retro.py` | `161 handle` |
| `app/ponto/motor_calculo_v2.py` | `671 _periodos_pelo_marco` |
| `app/ponto/registro_batida.py` | `109,141 registrar_batida`; `271 retratar_batida`; leitura genérica dos valores anteriores em `244` |
| `app/ponto/services/adesao.py` | `35 primeira_batida`; `58 ids_com_adesao` |
| `app/ponto/services/cartorio.py` | `353 houve_fato_novo`; `1155 _julgar_colab_corpo` |
| `app/ponto/services/deslogados.py` | `161,174 apurar` |
| `app/ponto/services/faltas_de_hoje.py` | `55 faltas_de_hoje_sem_cobranca` |
| `app/ponto/services/flip_auto.py` | `229 etiquetas_pela_celula`; `287,298 avaliar_fila` |
| `app/ponto/services/flip_batida.py` | `15 flip_tipo` |
| `app/ponto/services/lastro.py` | `262 julgar` |
| `app/ponto/services/passivo_s84.py` | `54 vivas` |
| `app/ponto/services/piso_intervalo.py` | `41 pares_abaixo_do_piso` |
| `app/ponto/services/retratar_humano.py` | `48,53 prazo_de_desfazer`; `67,75 prazos_de_desfazer`; `88 decisao_de`; `173 retratar`; `197 desfazer` |
| `app/ponto/turnos.py` | `1600,1612 papel_por_minuto_da_ata`; `1862 turnos_do_colab`; `1900 _ultima_bat`; `2219 _turno_aberto_calc`; `2351 turnos_abertos_de`; `2382 sequencia_relogio_x_jornada`; `2467 proximo_tipo_de`; `2864 batidas_apuraveis`; `2903 dia_das_batidas` |

Essas referências cobrem atributos, `getattr`, filtros ORM e campo temporal dinâmico; não incluem comentários/docstrings nem snapshots `antes` de auditoria como se fossem leitura da coluna. Exemplos de snapshots: `app/ponto/management/commands/diff_passivo_s84.py:84`, `app/ponto/management/commands/passivo_disputa_retro.py:146,170`.

**Interpretações especiais que preservam o significado:**

- Exibição busca deliberadamente somente retratadas em `app/colaboradores/services/calendario.py:239-244`, organiza o render separado da apuração em `:619-621`, e o template desenha riscado e informa “fora do cálculo” em `app/templates/colaboradores/partials/_calendario_grade.html:121`.
- Invalidação usa o instante da retratação como fato novo, sem contar a marca como trabalho: `app/chamados/veredito_lavrado.py:137-142`; `app/ponto/services/cartorio.py:329-353`.
- Prazo/desfazer e decisão humana leem data para ações e explicação: `app/ponto/services/retratar_humano.py:42-53,66-77,80-101,190-210`.
- `_par_integro` consulta cru em `app/chamados/reconciliador.py:206`, mas recusa se membro selecionado foi retratado em `:216-217`: o resultado final tem filtro explícito em memória.
- `_batida_existente_na_janela` é propositalmente compartilhado com idempotência de escrita; justificativa atual em `app/chamados/juizes.py:768-773`; o leitor que prova materialização exige `retratada_em is None` em `:774-775`. Mudar o helper inteiro sem separar as duas perguntas pode replantar aquilo que o cron retratou, conforme `:770-772`.

**Falha semântica adicional que exige atenção no censo principal:** `app/ponto/services/flip_auto.py:291-292` carrega `reais` cru; `:298` exclui retratadas apenas da lista de flips, mas `:303` passa a lista crua inteira para `grade_dia` no cálculo de `p0`. Portanto, a presença de um leitor correto de `retratada_em` nessa função não prova que todo o julgamento usa o universo correto.

Todos os **leitores diretos identificados** reconhecem retratação como exclusão, apresentação ou instante de mudança pelos exemplos citados; o sistema inteiro não aplica uniformemente a exclusão, como demonstra `flip_auto.py:291-303`. Garantia sobre estado efetivo em produção: [não verificado].

## 08/11 10:52 · sessao d2eb00cbbb9c · SELOS-DO-CANON-0811 · leitura · origem=Code

As referências abaixo são da cópia fornecida. Ela não contém `.git`; não consegui revalidar sua identidade como HEAD `80df6a5dc22bd4868696699ab8bdb51ce033198d`. [nao verificado]

**I-1 — CENSO-ANTES-DA-VAGA**

**(a) Quem chama hoje.** Existe o ponto de interceptação: `.claude/settings.json:62` configura `PreToolUse`, chamando `disjuntor.py pretool` em `.claude/settings.json:68`. Porém, o ramo `Agent` atual lê o prompt e aplica B3/B4; não abre brief nem valida CENSO. Portanto, **o hook geral existe; a guarda de abertura do construtor seria nova**. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1460`)

O protocolo também difere: o disjuntor comunica negação por JSON `permissionDecision=deny`, e seu `main_pretool` retorna zero. O contrato novo de validador com `rc 1` ainda não corresponde a esse protocolo existente. (`bin/disjuntor.py:1395`, `bin/disjuntor.py:1493`, `bin/disjuntor.py:1507`)

Os briefs aparecem como artefatos preparados e despachados pela sessão: E1 “brief pronto” e depois abertura do construtor; AKMED e PLANTIO também têm caminhos registrados. **Não há nomenclatura uniforme:** `logs/fio_e0_v4/e1_brief.md` não casa com `logs/<obra>/brief*.md`; `logs/v2bugs/i2/brief_v6.md` tem um nível adicional. (`app/docs/RELATO.md:95`, `app/docs/RELATO.md:91`, `app/docs/RELATO.md:80`, `app/docs/RELATO.md:75`, `app/docs/RELATO.md:205`)

No inventário desta cópia: **zero arquivos `logs/*/brief*.md`, zero obras observáveis por esse glob**; `logs/` não foi fornecido. Quantas obras os possuem em produção: **[nao verificado]**. O diretório está ignorado pelo Git, portanto sua ausência na exportação não prova ausência no vivo. (`.gitignore:53`)

**(b) Colisão com PAREI.** A linha contratada **não satisfaz nenhum dos dois caminhos atuais**:

- PAREI precisa conter `!`; pergunta de lei tampouco devolve turno por esse caminho. Além disso, deve travar o item em curso, sem outros itens andáveis. (`bin/hook_stop_fila1.py:1109`, `bin/hook_stop_fila1.py:1119`, `bin/hook_stop_fila1.py:1155`, `bin/hook_stop_fila1.py:1076`)
- Espera provada precisa começar literalmente por `ESPERANDO:` e declarar `pid=` ou `arquivo=`. `PAREI: censo-<obra> | espera Codex` não declara nenhum deles. (`bin/hook_stop_fila1.py:656`, `bin/hook_stop_fila1.py:665`, `bin/hook_stop_fila1.py:673`)

Os sítios envolvidos na compatibilização são o reconhecedor de espera, sua memoização por Stop, o orçamento que fica pendente durante espera provada e a liberação final. Mudar apenas a decisão final deixaria o orçamento tratando o censo por outro caminho. (`bin/hook_stop_fila1.py:645`, `bin/hook_stop_fila1.py:688`, `bin/hook_stop_fila1.py:984`, `bin/hook_stop_fila1.py:1231`)

| Leitor/selo solicitado | O que faz hoje e a colisão |
|---|---|
| `test_hook_parei_so_lei.sh` | Exige `_SO_BANG`; rejeita decisão técnica e lei como portas; aceita `!`. Relaxar todo PAREI contrariaria esses casos. (`bin/tests/test_hook_parei_so_lei.sh:25`, `bin/tests/test_hook_parei_so_lei.sh:41`, `bin/tests/test_hook_parei_so_lei.sh:52`, `bin/tests/test_hook_parei_so_lei.sh:60`) |
| `test_hook_esperando_com_prova.sh` | Distingue produtor vivo, morto, outro programa e declaração sem prova; verifica que a liberação ocorreu especificamente por espera provada. (`bin/tests/test_hook_esperando_com_prova.sh:67`, `bin/tests/test_hook_esperando_com_prova.sh:93`) |
| `test_disjuntor.sh` | Exercita PAREI CLEAR, DIFF em curso, agente sem entrega e COTA com handoff; também impede usar arquivo escrito pelo próprio hook como prova. São interações a preservar, sem um caso censo demonstrado. (`bin/tests/test_disjuntor.sh:192`, `bin/tests/test_disjuntor.sh:200`, `bin/tests/test_disjuntor.sh:215`, `bin/tests/test_disjuntor.sh:477`, `bin/tests/test_disjuntor.sh:578`) |
| `handoff_sessao.sh` | Já exibiria a linha censo, mas diria “item em curso ANDA”/“não devolve turno”: consulta somente os predicados de PAREI genérico. (`bin/handoff_sessao.sh:108`, `bin/handoff_sessao.sh:120`) |
| `alarme_sessao_ociosa.py` | Já desarma com qualquer substring `PAREI:` nas primeiras 40 linhas, sem prova. Pode silenciar enquanto o Stop continua mandando seguir. (`bin/alarme_sessao_ociosa.py:119`, `bin/alarme_sessao_ociosa.py:127`, `bin/alarme_sessao_ociosa.py:42`) |

A prova PID atual verifica processo vivo, identidade do executável/script e estabilidade do PID durante a leitura; **não consulta `CODEX_OBRA` nem `CODEX_PAPEL`**. A alternativa por arquivo exige avanço de mtime; a primeira observação não prova progresso. (`bin/hook_stop_fila1.py:523`, `bin/hook_stop_fila1.py:535`, `bin/hook_stop_fila1.py:548`, `bin/hook_stop_fila1.py:640`)

O wrapper exige obra e papel, mas isso não estabelece vínculo entre o novo PAREI e um censo vivo daquela obra. Sua emissão explícita de `ESPERANDO` com PID/JSONL ocorre no ramo **construtor**, não no papel censo. (`bin/codex_pergunta.sh:81`, `bin/codex_pergunta.sh:85`, `bin/codex_pergunta.sh:375`)

Assim, o lastro específico “produtor vivo do censo desta obra” permanece **[nao verificado]**. Aceitar somente o literal dispensaria o requisito de prova; aceitar qualquer Codex vivo não distinguiria obra nem papel, campos que o wrapper já separa. (`bin/hook_stop_fila1.py:673`, `bin/codex_pergunta.sh:81`)

**(f) Furos.** O ramo atual não fotografa versão/conteúdo do brief; portanto não estabelece identidade abertura→brief. Também depende da passagem pelo hook configurado, e a chave `DISJUNTOR_DESLIGADO` pula suas verificações; exceções fora do ramo B3 liberam a ferramenta. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1487`, `bin/disjuntor.py:1496`)

Cobertura de agente aberto por outra via, allowlist vazia efetivamente guardada e proteção contra brief alterado após abertura: **[nao verificado]**. Existe ainda outra abertura de construtor, pelo wrapper Codex, com vaga própria e execução própria; cobri-la não decorre automaticamente do ramo `Agent`. (`bin/codex_pergunta.sh:233`, `bin/codex_pergunta.sh:394`)

**I-2 — UNIDADE-NO-AVAL**

**(a) Quem registra hoje.** A responsabilidade declarada é do **Code/sessão**: prompt recebido vira linha em `PROMPTS.md`, e pedido de obra vira BACKLOG no mesmo turno. Ronald aparece como autor dos avais registrados. Não localizei uma porta executável única responsável por registrar e despachar esses avais. (`CLAUDE.md:433`, `app/docs/PROMPTS.md:3`, `app/docs/PROMPTS.md:1931`; registrador único: **[nao verificado]**)

Os registros apontam arquivos privados como `logs/plantio_mais1/aval_i.txt`; quem efetivamente escreveu esses arquivos, e por qual ferramenta, é **[nao verificado]**. (`app/docs/PROMPTS.md:1931`, `.gitignore:53`)

`gerar_avais.py` não resolve essa identidade: ele transforma `PENDENTES_RONALD.json` em `AVAIS.md`, a lista de pedidos abertos para Ronald responder. Não é o escritor de `PROMPTS.md`/`aval_*.txt`. (`bin/gerar_avais.py:3`, `bin/gerar_avais.py:30`, `bin/gerar_avais.py:105`)

**(d) Marcadores detectáveis nos registros disponíveis.** Há exemplos textuais das seis classes; isso comprova vocabulário, não um classificador formal. (`app/docs/PROMPTS.md:1477`, `app/docs/PROMPTS.md:1931`, `app/docs/PROMPTS.md:1933`, `app/docs/PROMPTS.md:1966`, `app/docs/PROMPTS.md:1967`)

| Classe | Exemplo no registro |
|---|---|
| Backfill | “backfill de data_evento”. (`app/docs/PROMPTS.md:1931`) |
| Migração | “migracao e backfill chamam a MESMA porta”. (`app/docs/PROMPTS.md:1967`) |
| Dedup | “oraculo E6 perde o dedup < 10 min”. (`app/docs/PROMPTS.md:1477`) |
| Juiz | “juiz tarefa_do_dia nasce”. (`app/docs/PROMPTS.md:1933`) |
| Veto | “MESMO veto de merito”. (`app/docs/PROMPTS.md:1967`) |
| Catraca | Contagem de `select_for_update` sobre modelos nomeados. (`app/docs/PROMPTS.md:1966`) |

**Falso positivo concreto:** `dedup` aparece em instrumento de medição de consumo do Code; `catraca`/`juizes` aparecem numa ordem de gestão da noite. Casar essas palavras classificaria também esses textos como DADO. (`app/docs/PROMPTS.md:1872`, `app/docs/PROMPTS.md:1839`)

**Falso negativo no resumo disponível:** E1-FATO-E-O-TURNO muda o critério dos selos que recusam por `data_evento`, sem empregar nenhuma das seis palavras. O texto integral privado desse aval não foi fornecido. (`app/docs/PROMPTS.md:1977`; integral: **[nao verificado]**)

A isenção por título também falha: TOAST-E-NOME-CALENDARIO reúne veto de criação/salvamento e apresentação de toast no mesmo registro. (`app/docs/PROMPTS.md:1828`)

A memória citada tem exemplos explícitos: grep contou 13 arquivos contra nove por AST porque prosa citava a palavra; na O118, identidade por texto `HH:MM` produziu falsos positivos, corrigidos consultando o predicado de batidas apuráveis. (`bin/atomicidade_da_porta.py:41`, `app/docs/BACKLOG.md:1199`)

**(f) Furos.** Autoridade para classificar DADO versus gestão/instrumento/UI, tratamento de aval misto, campos vazios e caminho obrigatório antes do despacho não estão estabelecidos por um mecanismo localizado nesta cópia. **[nao verificado]**

Aval apenas no chat fica fora do alcance de um fiscal que só examina arquivo. O selo atual parte de promessas **já registradas** em PROMPTS e verifica IDs no BACKLOG; não comprova que todo aval recebido entrou no registro. (`bin/tests/test_prompt_virou_item.sh:21`, `bin/tests/test_prompt_virou_item.sh:30`)

**I-3 — ETAPAS-NO-BRIEF**

**(a/c) Abertura, opus final e B3.** A abertura compartilha o sítio do I-1, atualmente sem leitura do brief. O verificador recebe ID da obra, diff e log RED; seu checklist não exige brief, RELATO do pouso ou correspondência entre etapas e resultados. (`bin/disjuntor.py:1451`, `.claude/agents/verificador.md:3`, `.claude/agents/verificador.md:8`)

O **B3 atual não faz essa conferência**. Ele identifica obra/leitura final pelo prompt e extrai `APROVADO`/`REPROVADO` da primeira linha do `SubagentHandback`, filtrando agentes do tipo `verificador`. (`bin/disjuntor.py:546`, `bin/disjuntor.py:561`, `bin/disjuntor.py:742`)

Ele lê o RELATO para fotografar duas linhas úteis do topo e reconhecer `DESENHO`/`ERRO-BOBO` que liberam a obra. Conta reprovações e voltas Codex procedentes, não etapas entregues. (`bin/disjuntor.py:662`, `bin/disjuntor.py:626`, `bin/disjuntor.py:910`)

Há uma lacuna temporal: **o opus final precede o pouso**, enquanto a linha de medida é exigida no RELATO **do pouso**. O artefato e o instante da conferência de etapas antes de autorizar esse pouso não estão definidos no código atual. (`CLAUDE.md:374`, `CLAUDE.md:379`, `.claude/agents/verificador.md:3`)

**Como contaria hoje:** não há contador demonstrado. Pela letra do anexo, seria necessário comparar as etapas declaradas no brief com as etapas que têm resultado medido no RELATO; contar ocorrências de “etapa K” não comprova essa relação. Identidade de versão, resultado antigo versus atual e resultado esperado versus medido permanecem sem reconhecedor verificado. **[nao verificado]**

O checador de prova existente também não supre isso: procura `PROVA:` próximo de certos atos em título/negrito, sem comparar etapa, comando ou valor medido. (`bin/relato_afirma_com_prova.py:36`, `bin/relato_afirma_com_prova.py:47`)

**(f) Furos.** Brief editado depois da abertura perde uma referência estável para o denominador das etapas; o ramo atual não guarda essa referência. Além disso, B3 só reconhece os tipos/prompts finais previstos, e o verificador manda retornar apenas problemas — até 15 linhas —, sem relatório positivo N/N. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:555`, `bin/disjuntor.py:742`, `.claude/agents/verificador.md:18`)

Proteção específica contra reescopo, uso de brief anterior ou aval de alteração mantido apenas no chat: **[nao verificado]**.

**(e) MEDIDA — comum aos três instrumentos**

`ESTADO.md` é gerado por `bin/gerar_estado.py`, chamado por `relato.sh` para `$TMP/ESTADO.md`, e publicado pela raspagem. O gerador grava o destino recebido; não depende de existir `app/docs/ESTADO.md`. (`bin/relato.sh:38`, `bin/gerar_estado.py:390`, `bin/relato.sh:105`)

**Já existe molde de janela móvel de 24 h:** `disjuntor.ler` usa `agora - 86400`, lê eventos datados e conta disparos, negações e liberações. O ESTADO consome esse medidor por `ler/linha`. (`bin/disjuntor.py:1546`, `bin/disjuntor.py:1568`, `bin/disjuntor.py:1570`, `bin/gerar_estado.py:277`)

Há também contagem de chamadas Codex por papel em 24 h, baseada em eventos `ini`; e média de espera CLEAR baseada em `espera=Ns`. **Essa média não mede horas de vaga ociosa esperando censo.** (`bin/codex_limite_estado.py:24`, `bin/codex_limite_estado.py:71`, `bin/disjuntor.py:1580`, `bin/disjuntor.py:1688`)

Os contadores atuais não demonstram as novas medidas de I-1/I-2/I-3; zero construtores sem censo, 100% de avais conformes e 100% de pousos N/N em produção permanecem **[nao verificado]**. Os campos hoje publicados são os explicitados em `bin/disjuntor.py:1615` e `bin/disjuntor.py:1691`.

## 08/11 10:50 · sessao 3977fc5450fa · - · - · origem=Ronald (tmux)

**I-2 — quem registra.** A obrigação atual é do Code/sessão: prompt recebido ganha linha em `PROMPTS.md` e obra ganha item no BACKLOG no mesmo turno (`CLAUDE.md:433-435`; `app/docs/PROMPTS.md:3-9`). Ronald aparece como autor do aval; os registros dizem “recebido” e “repassado ao construtor” (`app/docs/PROMPTS.md:1948-1950`). Não localizei registrador central executável, validador `UNIDADE=`, diretório `avais_recusados` ou selo I-2 na cópia **[não verificado]**. O executor físico das escritas privadas em `logs/*/aval_*.txt` é **[não verificado]**; `logs/` está ignorado (`.gitignore:53`).

`gerar_cortes.py` é outro instrumento: lê `CORTES.json` e gera `CORTES.md`/tabela do RELATO; não registra o recebimento do aval (`bin/gerar_cortes.py:3-10`, `:23-25`). Handoff e placar **leem** PROMPTS (`bin/handoff_sessao.sh:128-140`; `bin/placar_code.sh:395-396`).

| Classe | Marcador textual já encontrado em registro de aval |
|---|---|
| Backfill | “backfill de data_evento” — `app/docs/PROMPTS.md:1931` |
| Migração | “migracao e backfill chamam a MESMA porta” — `app/docs/PROMPTS.md:1967` |
| Dedup | “oraculo E6 perde o dedup < 10 min” — `app/docs/PROMPTS.md:1477` |
| Juiz | “juiz tarefa_do_dia nasce” — `app/docs/PROMPTS.md:1933` |
| Veto | “MESMO veto de merito” — `app/docs/PROMPTS.md:1967` |
| Catraca | “conta so select_for_update sobre” quatro modelos — `app/docs/PROMPTS.md:1966` |

**Palavra não comprova classe.** “Dedup” aparece em medição de consumo de agentes, instrumento isento pelo contrato fornecido; usá-la como classe DADO recusaria esse instrumento (`app/docs/PROMPTS.md:1872`). “Catraca” e “juízes” aparecem em aval de ordem/teto da noite (`app/docs/PROMPTS.md:1839`). Título UI também não determina isenção: TOAST-E-NOME-CALENDARIO contém uma parte de veto de criação/salvamento e outra de apresentação (`app/docs/PROMPTS.md:1828`).

**Falso negativo lexical demonstrável:** o resumo do aval E1-FATO-E-O-TURNO-NAO-A-DATA-GRAVADA trata da recusa por `data_evento` sem nenhuma das seis palavras; classificador dessas palavras não o selecionaria (`app/docs/PROMPTS.md:1977`). O texto integral desse aval privado permanece **[não verificado]**.

A memória tem fundamento concreto: o varredor de atomicidade documenta grep contando 13 arquivos contra 9 por AST porque prosa citava a palavra, e proximidade textual de decorador não provava cobertura (`bin/atomicidade_da_porta.py:8-12`, `:41-44`). A memória O118 apontada pelo pai está em `app/docs/BACKLOG.md:1199`.

**Furo de chat/arquivo.** O selo atual considera somente IDs no idioma literal `BACKLOG **Oxx/Fxx**` já escritos em PROMPTS e verifica sua existência no BACKLOG; não estabelece que todo aval entrou no registro (`bin/tests/test_prompt_virou_item.sh:21-35`). A lacuna está explicitamente documentada no histórico (`app/docs/RELATO-ARQUIVO.md:30931`). Portanto, fiscalização restrita ao registro não observa aval que ficou apenas no chat; a ocorrência atual disso é **[não verificado]**.

**Allowlist/classificação.** O anexo I-2 isenta gestão/instrumento/UI, mas autoridade executável que decide essa classe, tratamento de aval misto e caminho único obrigatório de despacho permanecem **[não verificado]**. Não há evidência para chamar essas seis palavras de marcador formal existente **[não verificado]**.

## 08/11 10:49 · sessao fa5c2257202c · - · - · origem=Ronald (tmux)

I-3 — pontos comprovados na cópia:

- O verificador recebe **id da obra, diff e log do RED**; lê `CLAUDE.md`/`LEIS.md`, verifica nove itens e responde com primeira linha `APROVADO` ou `REPROVADO`. O arquivo não determina leitura de brief nem do RELATO do pouso. `.claude/agents/verificador.md:3`, `.claude/agents/verificador.md:8`, `.claude/agents/verificador.md:18`.
- O B3 lê os **transcripts dos verificadores**: identifica obra/leitura final no primeiro prompt e extrai o veredito da primeira linha de `SubagentHandback`; filtra `agentType=verificador`. Não extrai etapas ou resultados dessas respostas. `bin/disjuntor.py:546`, `bin/disjuntor.py:551`, `bin/disjuntor.py:561`, `bin/disjuntor.py:736`.
- O B3 lê o **RELATO**, mas o uso atual é fotografar duas linhas úteis do topo e reconhecer `DESENHO`/`ERRO-BOBO` para liberar a obra. Não é leitura do marco do pouso para conferir execução integral. `bin/disjuntor.py:626`, `bin/disjuntor.py:662`, `bin/disjuntor.py:686`, `bin/disjuntor.py:930`.
- O disparo atual conta reprovações do opus final e voltas Codex triadas procedentes; limiares são 1 e 3, respectivamente. `bin/disjuntor.py:76`, `bin/disjuntor.py:77`, `bin/disjuntor.py:896`, `bin/disjuntor.py:910`.
- O opus final ocorre **antes do pouso**, depois da suite verde; a linha de medida é exigida no RELATO **do pouso**. Portanto o I-3 deixa sem endereço/instante definido o relato que o opus precisaria conferir antes de autorizar esse pouso. `CLAUDE.md:374`, `CLAUDE.md:379`; definição desse endereço/instante no anexo: [não verificado]. [nao verificado]
- Contador existente de “etapa K citada com resultado”, frase `pela metade, faltam N`, parser `RESULTADO OBSERVAVEL` ou selo `test_etapas_no_brief.sh`: [não verificado]. O anexo não fixa vínculo entre obra/versão do brief e marco do RELATO, nem critério para distinguir resultado medido de resultado esperado, citado ou antigo. [não verificado]
- O checador de prova já existente só procura `PROVA:` nas seis linhas próximas de certos atos em título/negrito; sua implementação não compara comando, número, etapas ou medição. `bin/relato_afirma_com_prova.py:30`, `bin/relato_afirma_com_prova.py:36`, `bin/relato_afirma_com_prova.py:47`.
- Furo de rebrief: o ramo atual de abertura recebe `prompt` do `Agent` e faz B3/B4; não lê nem fotografa brief. Nenhum vínculo imutável abertura→brief→etapas fica estabelecido nesse ramo. `bin/disjuntor.py:1451`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1460`. Existência de outra proteção específica: [não verificado].
- Furo de identificação: B3 só captura como final prompts que correspondam ao reconhecedor `_eh_final` e verificadores com o tipo esperado; a existência de outros agentes/verificadores efetivamente lançados fora disso não pode ser inferida. `bin/disjuntor.py:499`, `bin/disjuntor.py:555`, `bin/disjuntor.py:742`; uso real fora desses filtros: [não verificado].

MEDIDA — gerador e moldes existentes:

- `ESTADO.md` é produzido por `gerar(raiz,dst)` e gravado em `dst`; `relato.sh` chama o gerador para `$TMP/ESTADO.md` e publica esse arquivo como `ESTADO.md`. `bin/gerar_estado.py:178`, `bin/gerar_estado.py:390`, `bin/relato.sh:38`, `bin/relato.sh:105`.
- O gerador **consome** medidores: importa `disjuntor.ler/linha` dentro de `try`; não reimplementa a contagem. `bin/gerar_estado.py:277`.
- Molde mais direto: `disjuntor.ler` usa `corte=agora-86400`, lê trilhas com carimbo, conta disparos/negações/liberações e acumula `espera=Ns` de eventos `ATENDIDO`. Trilhas ilegíveis tornam contadores `None`; a ausência de arquivo é tratada separadamente. `bin/disjuntor.py:1546`, `bin/disjuntor.py:1554`, `bin/disjuntor.py:1568`, `bin/disjuntor.py:1580`, `bin/disjuntor.py:1584`, `bin/disjuntor.py:1590`.
- Outro molde: `codex_limite_estado` conta recusas em 24 h e chamadas por papel, contando apenas eventos `ini`; o escritor declarado é `codex_pergunta.sh`. `bin/codex_limite_estado.py:4`, `bin/codex_limite_estado.py:24`, `bin/codex_limite_estado.py:27`, `bin/codex_limite_estado.py:71`.
- Esses moldes demonstram janela e integração ao painel; dados atuais para “construtor sem censo”, “horas de vaga esperando censo”, “aval recusado por UNIDADE” e “pouso N/N etapas” não aparecem nesses contadores. `bin/disjuntor.py:1615`, `bin/disjuntor.py:1691`, `bin/codex_limite_estado.py:45`; existência das novas trilhas e suas medidas em produção: [não verificado].

## 08/11 10:49 · sessao 2c9c1db42771 · - · - · origem=Ronald (tmux)

**I-1 / colisão e leitores**

- A linha proposta `PAREI: censo-<obra> | espera Codex` **não libera hoje por nenhum dos dois caminhos**: `_parei_do_em_curso` exige `PAREI:` e `!`; `_esperando_com_prova` exige prefixo literal `ESPERANDO:` e ao menos `pid=` ou `arquivo=`. (`bin/hook_stop_fila1.py:1104`, `bin/hook_stop_fila1.py:1119`, `bin/hook_stop_fila1.py:656`, `bin/hook_stop_fila1.py:673`.)
- O PAREI genérico ainda exige que a linha nomeie o item em curso e que não haja outros andáveis; acrescentar apenas `!` tampouco equivale ao contrato do censo. (`bin/hook_stop_fila1.py:1087`, `bin/hook_stop_fila1.py:1142`, `bin/hook_stop_fila1.py:1155`.)
- Os sítios dependentes da espera são o reconhecedor, a memoização por Stop, o orçamento que fica pendente durante espera provada e a decisão principal do Stop; mudar só o ramo final deixa o orçamento tratar a espera de censo diferentemente. (`bin/hook_stop_fila1.py:645`, `bin/hook_stop_fila1.py:688`, `bin/hook_stop_fila1.py:984`, `bin/hook_stop_fila1.py:1231`.)
- Antes desses caminhos, a checagem de afirmações do RELATO e o B4 têm precedência; a exceção do censo não pode ser presumida como passagem por eles. (`bin/hook_stop_fila1.py:1194`, `bin/hook_stop_fila1.py:1204`, `bin/hook_stop_fila1.py:1215`.)
- O handoff já **exibiria** a linha censo, pois coleta `PAREI` até a primeira seção, mas continuaria imprimindo “item em curso ANDA” / “não devolve turno”, por consultar exclusivamente os predicados de PAREI genérico. (`bin/handoff_sessao.sh:99`, `bin/handoff_sessao.sh:108`, `bin/handoff_sessao.sh:120`.)
- O alarme já aceita qualquer substring `PAREI:` nas primeiras 40 linhas e desarma, sem consultar prova ou `_SO_BANG`: com censo inexistente/morto ele já pode ficar silencioso enquanto o Stop manda seguir. (`bin/alarme_sessao_ociosa.py:119`, `bin/alarme_sessao_ociosa.py:127`, `bin/alarme_sessao_ociosa.py:42`.)

**Selos que precisam ser confrontados, sem execução**

- `test_hook_parei_so_lei.sh` preserva o filtro `_SO_BANG`, rejeita decisão técnica e pergunta de lei como passagem e aceita `!`; liberar todo PAREI violaria esses casos. Não contém caso censo. (`bin/tests/test_hook_parei_so_lei.sh:25`, `bin/tests/test_hook_parei_so_lei.sh:41`, `bin/tests/test_hook_parei_so_lei.sh:52`, `bin/tests/test_hook_parei_so_lei.sh:60`.)
- `test_hook_esperando_com_prova.sh` distingue vivo/morto/outro produtor/sem prova e exige trilha `ALLOW ESPERANDO com prova` ou `NOPROVA`; censo precisaria entrar com essa mesma distinção, sem transformar o literal em prova. (`bin/tests/test_hook_esperando_com_prova.sh:67`, `bin/tests/test_hook_esperando_com_prova.sh:79`, `bin/tests/test_hook_esperando_com_prova.sh:93`.)
- `test_disjuntor.sh` protege PAREI CLEAR com handoff, validade temporal, DIFF e agente sem entrega; B4 COTA também exige handoff; protege ainda contra `pisos.log` como prova escrita pelo próprio hook. São interações a preservar, não regras hoje alteradas pelo literal censo. (`bin/tests/test_disjuntor.sh:104`, `bin/tests/test_disjuntor.sh:192`, `bin/tests/test_disjuntor.sh:200`, `bin/tests/test_disjuntor.sh:215`, `bin/tests/test_disjuntor.sh:477`, `bin/tests/test_disjuntor.sh:578`.)

**Prova de vida e furo**

- A prova PID atual lê `/proc/<pid>/stat`, executable e cmdline, rejeita ancestrais, zumbi, troca de processo e produtor divergente; ela **não lê `CODEX_OBRA`**. (`bin/hook_stop_fila1.py:523`, `bin/hook_stop_fila1.py:535`, `bin/hook_stop_fila1.py:544`, `bin/hook_stop_fila1.py:548`.)
- A prova por arquivo exige arquivo regular e avanço de mtime desde avaliação anterior, sob trava; primeira observação não prova e arquivos do próprio painel/hook são excluídos. (`bin/hook_stop_fila1.py:563`, `bin/hook_stop_fila1.py:583`, `bin/hook_stop_fila1.py:604`, `bin/hook_stop_fila1.py:640`.)
- O wrapper exige `CODEX_OBRA` e `CODEX_PAPEL`, e o ramo construtor emite uma linha ESPERANDO com PID do wrapper e JSONL da chamada. Isso oferece identidade declarada da chamada, mas não há vínculo dela com o novo literal censo no Stop. (`bin/codex_pergunta.sh:81`, `bin/codex_pergunta.sh:85`, `bin/codex_pergunta.sh:374`.)
- Uma prova específica precisaria vincular processo vivo/chamada ao **mesmo censo e obra**, por exemplo identidade `CODEX_OBRA` conferível no produtor ou sinal próprio com progresso. A existência desse mecanismo é **[não verificado]**; aceitar somente “espera Codex” abre espera sem produtor, e aceitar qualquer processo Codex permite que outra obra justifique a parada. A insuficiência do código atual se apoia em `bin/hook_stop_fila1.py:656`, `bin/hook_stop_fila1.py:673`, `bin/hook_stop_fila1.py:535`.
- Processo Codex real ativo, acesso a `/proc` em produção e resultado dos selos nesta auditoria: **[não verificado]**.

_2874 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
